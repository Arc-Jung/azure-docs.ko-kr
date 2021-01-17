---
title: Azure Machine Learning 데이터 세트 만들기
titleSuffix: Azure Machine Learning
description: 기계 학습 실험 실행을 위해 데이터에 액세스 하는 Azure Machine Learning 데이터 집합을 만드는 방법에 대해 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, contperf-fy21q1, data4ml
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 07/31/2020
ms.openlocfilehash: 8dac15f359d8ab6c7a84bbc30dba392322e84bb5
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98538188"
---
# <a name="create-azure-machine-learning-datasets"></a>Azure Machine Learning 데이터 세트 만들기

이 문서에서는 Azure Machine Learning Python SDK를 사용 하 여 로컬 또는 원격 실험의 데이터에 액세스 하는 Azure Machine Learning 데이터 집합을 만드는 방법에 대해 알아봅니다. 데이터 집합이 Azure Machine Learning의 전체 데이터 액세스 워크플로에 적합 한 위치를 이해 하려면 [안전 하 게 데이터 액세스](concept-data.md#data-workflow) 문서를 참조 하세요.

데이터 세트를 만들면 데이터 원본 위치에 대한 참조와 해당 메타데이터의 복사본을 만듭니다. 데이터는 기존 위치에 남아 있으므로 추가 저장소 비용이 발생 하지 않으며 데이터 원본의 무결성을 위험 하지 않습니다. 또한 데이터 집합은 지연 계산 되며 워크플로 성능 속도를 지원 합니다. 데이터 저장소, 공용 Url 및 [Azure Open 데이터 집합](../open-datasets/how-to-create-azure-machine-learning-dataset-from-open-dataset.md)에서 데이터 집합을 만들 수 있습니다.

낮은 코드 환경에서는 [Azure Machine Learning studio를 사용 하 여 Azure Machine Learning 데이터 집합을 만듭니다](how-to-connect-data-ui.md#create-datasets).

Azure Machine Learning 데이터 집합을 사용 하 여 다음을 수행할 수 있습니다.

* 데이터 집합에서 참조 하는 데이터의 단일 복사본을 저장소에 보관 합니다.

* 연결 문자열 또는 데이터 경로를 걱정 하지 않고 모델 학습 중에 데이터에 원활 하 게 액세스 합니다. [데이터 집합을 사용 하 여 학습 하는 방법에 대해 자세히 알아보세요](how-to-train-with-datasets.md).

* 데이터를 공유 하 고 다른 사용자와 공동 작업 합니다.

## <a name="prerequisites"></a>사전 요구 사항

데이터 집합을 만들고 작업 하려면 다음이 필요 합니다.

* Azure 구독 구독이 없으면 시작하기 전에 계정을 만드세요. [Azure Machine Learning 평가판 또는 유료 버전](https://aka.ms/AMLFree)을 사용해 보세요.

* [Azure Machine Learning 작업 영역](how-to-manage-workspace.md)입니다.

* Azureml 데이터 집합 패키지가 포함 된 [Python 용 AZURE MACHINE LEARNING SDK가 설치 되어](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py)있습니다.

    * 통합 된 노트북 및 SDK가 이미 설치 되어 있는 완전히 구성 되 고 관리 되는 개발 환경인 [Azure Machine Learning 계산 인스턴스](how-to-create-manage-compute-instance.md)를 만듭니다.

    **OR**

    * 자신의 Jupyter 노트북에서 작업 하 고 [이러한 지침](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py)을 사용 하 여 SDK를 직접 설치 합니다.

> [!NOTE]
> 일부 데이터 집합 클래스에는 64 비트 Python과만 호환 되는 [azureml-dataprep](/python/api/azureml-dataprep/?preserve-view=true&view=azure-ml-py) 패키지에 대 한 종속성이 있습니다. Linux 사용자의 경우 이러한 클래스는 Red Hat Enterprise Linux (7, 8), Ubuntu (14.04, 16.04, 18.04), Fedora (27, 28), Debian (8, 9) 및 CentOS (7) 배포판 에서만 지원 됩니다. 지원 되지 않는 배포판을 사용 하는 경우 [이 가이드](/dotnet/core/install/linux) 에 따라 .net Core 2.1을 설치 하 여 계속 진행 하세요. 

## <a name="compute-size-guidance"></a>계산 크기 지침

데이터 집합을 만들 때 메모리에서 계산 처리 능력과 데이터 크기를 검토 합니다. 저장소에 있는 데이터의 크기는 데이터 프레임의 데이터 크기와 동일 하지 않습니다. 예를 들어 CSV 파일의 데이터는 데이터 프레임에서 최대 10 배까지 확장할 수 있으므로 1gb CSV 파일은 데이터 프레임에서 10gb가 될 수 있습니다. 

데이터가 압축 되 면 추가로 확장할 수 있습니다. 압축 된 parquet 형식으로 저장 된 20gb의 비교적 스파스 데이터는 메모리에서 800 GB로 확장 될 수 있습니다. Parquet 파일은 데이터를 열 형식으로 저장 하기 때문에 열 절반이 필요한 경우에는 메모리에서 400 ~ GB를 로드 하기만 하면 됩니다.

[Azure Machine Learning에서 데이터 처리를 최적화 하는 방법에 대해 자세히 알아보세요](concept-optimize-data-processing.md).

## <a name="dataset-types"></a>데이터 세트 형식

사용자가 학습에서 사용 하는 방법에 따라 두 가지 데이터 집합 형식이 있습니다. FileDatasets 및 TabularDatasets. 두 형식은 모두 추정, AutoML, hyperDrive 및 파이프라인을 포함 하는 Azure Machine Learning 학습 워크플로에 사용할 수 있습니다. 

### <a name="filedataset"></a>FileDataset

[Filedataset](/python/api/azureml-core/azureml.data.file_dataset.filedataset?preserve-view=true&view=azure-ml-py) 은 데이터 저장소 또는 public url의 단일 또는 여러 파일을 참조 합니다. 데이터가 이미 정리 되어 있고 학습 실험에서 사용할 준비가 된 경우 파일을 FileDataset 개체로 계산에 [다운로드 하거나 탑재할](how-to-train-with-datasets.md#mount-vs-download) 수 있습니다. 

원본 파일이 모든 형식일 수 있으므로 기계 학습 워크플로에 FileDatasets을 사용 하는 것이 좋습니다 .이를 통해 심층 학습을 비롯 한 다양 한 기계 학습 시나리오를 사용할 수 있습니다.

[PYTHON SDK](#create-a-filedataset) 또는 [Azure Machine Learning studio](how-to-connect-data-ui.md#create-datasets) 를 사용 하 여 filedataset을 만듭니다.
### <a name="tabulardataset"></a>TabularDataset

[TabularDataset](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py) 는 제공 된 파일 또는 파일 목록을 구문 분석 하 여 테이블 형식으로 데이터를 나타냅니다. 이를 통해 pandas 또는 Spark 데이터 프레임로 데이터를 구체화할 수 있으므로 노트북을 떠나지 않고도 익숙한 데이터 준비 및 학습 라이브러리를 사용할 수 있습니다. `TabularDataset`.Csv,. tsv, parquet,. jsonl 파일 및 [SQL 쿼리 결과](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?preserve-view=true&view=azure-ml-py#&preserve-view=truefrom-sql-query-query--validate-true--set-column-types-none--query-timeout-30-)에서 개체를 만들 수 있습니다.

TabularDatasets를 사용 하면 데이터의 열에서 타임 스탬프를 지정 하거나 경로 패턴 데이터가 저장 되는 위치에서 시계열 특성을 사용할 수 있습니다. 이 사양에서는 시간을 기준으로 쉽고 효율적인 필터링을 수행할 수 있습니다. 예를 들어 [NOAA 날씨 데이터를 사용 하 여 테이블 형식 시계열 관련 API 데모](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb)를 참조 하세요.

[PYTHON SDK](#create-a-tabulardataset) 또는 [Azure Machine Learning studio](how-to-connect-data-ui.md#create-datasets)를 사용 하 여 TabularDataset를 만듭니다.

>[!NOTE]
> Azure Machine Learning studio를 통해 생성 된 AutoML 워크플로는 현재 TabularDatasets만 지원 합니다. 

## <a name="access-datasets-in-a-virtual-network"></a>가상 네트워크의 데이터 집합에 액세스

작업 영역이 가상 네트워크에 있는 경우 유효성 검사를 건너뛰도록 데이터 집합을 구성 해야 합니다. 가상 네트워크에서 데이터 저장소 및 데이터 집합을 사용 하는 방법에 대 한 자세한 내용은 [작업 영역 및 연결 된 리소스 보호](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets)를 참조 하세요.

<a name="datasets-sdk"></a>

## <a name="create-datasets"></a>데이터 세트 만들기

Azure Machine Learning에서 데이터에 액세스할 수 있도록 하려면 [Azure 데이터 저장소](how-to-access-data.md) 또는 공용 웹 url의 경로에서 데이터 집합을 만들어야 합니다. 

Python SDK를 사용 하 여 [Azure 데이터 저장소](how-to-access-data.md) 에서 데이터 집합을 만들려면 다음을 수행 합니다.

1. `contributor`또는 `owner` 등록 된 Azure 데이터 저장소에 대 한 액세스 권한이 있는지 확인 합니다.

2. 데이터 저장소에서 경로를 참조 하 여 데이터 집합을 만듭니다. 여러 데이터 저장소의 여러 경로에서 데이터 집합을 만들 수 있습니다. 데이터 집합을 만들 수 있는 파일 수 또는 데이터 크기에 대 한 하드 제한은 없습니다. 

> [!NOTE]
> 각 데이터 경로에 대해 몇 가지 요청은 저장소 서비스로 전송 되어 파일이 나 폴더를 가리키는지 확인 합니다. 이러한 오버 헤드로 인해 성능이 저하 되거나 오류가 발생할 수 있습니다. 내에서 1000 파일을 포함 하는 한 개의 폴더를 참조 하는 데이터 집합은 하나의 데이터 경로를 참조 하 고 최적의 성능을 위해 데이터 저장소에서 100 미만의 경로를 참조 하는 데이터 집합을 만드는 것이 좋습니다.

### <a name="create-a-filedataset"></a>FileDataset 만들기

[`from_files()`](/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?preserve-view=true&view=azure-ml-py#&preserve-view=truefrom-files-path--validate-true-)클래스의 메서드를 사용 `FileDatasetFactory` 하 여 모든 형식의 파일을 로드 하 고 등록 되지 않은 filedataset을 만듭니다. 

저장소가 가상 네트워크 또는 방화벽 뒤에 있는 경우 메서드에서 매개 변수를 설정 `validate=False` `from_files()` 합니다. 이렇게 하면 초기 유효성 검사 단계가 무시 되 고 이러한 보안 파일에서 데이터 집합을 만들 수 있습니다. [가상 네트워크에서 데이터 저장소 및 데이터 집합을 사용](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets)하는 방법에 대해 자세히 알아보세요.

```Python
# create a FileDataset pointing to files in 'animals' folder and its subfolders recursively
datastore_paths = [(datastore, 'animals')]
animal_ds = Dataset.File.from_files(path=datastore_paths)

# create a FileDataset from image and label files behind public web urls
web_paths = ['https://azureopendatastorage.blob.core.windows.net/mnist/train-images-idx3-ubyte.gz',
             'https://azureopendatastorage.blob.core.windows.net/mnist/train-labels-idx1-ubyte.gz']
mnist_ds = Dataset.File.from_files(path=web_paths)
```
작업 영역에서 실험을 통해 데이터 집합을 다시 사용 하 고 공유 하려면 [데이터 집합을 등록](#register-datasets)합니다. 

> [!TIP] 
> 로컬 디렉터리에서 파일을 업로드 하 고 공개 미리 보기 메서드인 [upload_directory ()](/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?preserve-view=true&view=azure-ml-py#upload-directory-src-dir--target--pattern-none--overwrite-false--show-progress-true-)를 사용 하 여 단일 메서드에서 filedataset을 만듭니다. 이 방법은 [실험적](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py#stable-vs-experimental) 미리 보기 기능으로, 언제 든 지 변경 될 수 있습니다. 
> 
>  이 메서드는 데이터를 기본 저장소에 업로드 하므로 저장소 비용이 발생 합니다. 

### <a name="create-a-tabulardataset"></a>TabularDataset 만들기

클래스의 [`from_delimited_files()`](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory) 메서드를 사용 `TabularDatasetFactory` 하 여 .csv 또는. tsv 형식의 파일을 읽고 등록 되지 않은 TabularDataset를 만듭니다. 여러 파일에서 읽는 경우 결과는 하나의 테이블 형식 표현으로 집계 됩니다. 

저장소가 가상 네트워크 또는 방화벽 뒤에 있는 경우 메서드에서 매개 변수를 설정 `validate=False` `from_delimited_files()` 합니다. 이렇게 하면 초기 유효성 검사 단계가 무시 되 고 이러한 보안 파일에서 데이터 집합을 만들 수 있습니다. [가상 네트워크에서 데이터 저장소 및 데이터 집합](how-to-secure-workspace-vnet.md#secure-datastores-and-datasets)을 사용 하는 방법에 대해 자세히 알아보세요.

다음 코드는 기존 작업 영역 및 원하는 데이터 저장소를 이름별로 가져옵니다. 그런 다음는 데이터 저장소 및 파일 위치를 `path` 매개 변수에 전달 하 여 새 TabularDataset를 만듭니다 `weather_ds` .

```Python
from azureml.core import Workspace, Datastore, Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
    
# retrieve an existing datastore in the workspace by name
datastore = Datastore.get(workspace, datastore_name)

# create a TabularDataset from 3 file paths in datastore
datastore_paths = [(datastore, 'weather/2018/11.csv'),
                   (datastore, 'weather/2018/12.csv'),
                   (datastore, 'weather/2019/*.csv')]

weather_ds = Dataset.Tabular.from_delimited_files(path=datastore_paths)
```

기본적으로 TabularDataset를 만들면 열 데이터 형식이 자동으로 유추 됩니다. 유추 된 형식이 예상과 일치 하지 않는 경우에는 다음 코드를 사용 하 여 열 형식을 지정할 수 있습니다. 매개 변수는 `infer_column_type` 분리 된 파일에서 만든 데이터 집합에만 적용 됩니다. [지원 되는 데이터 형식에 대해 자세히 알아보세요](/python/api/azureml-core/azureml.data.dataset_factory.datatype?preserve-view=true&view=azure-ml-py).


```Python
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType

# create a TabularDataset from a delimited file behind a public web url and convert column "Survived" to boolean
web_path ='https://dprepdata.blob.core.windows.net/demo/Titanic.csv'
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_path, set_column_types={'Survived': DataType.to_bool()})

# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

|인덱싱할|PassengerId|Survived|Pclass|Name|성|나이|SibSp|Parch|티켓|요금|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|male|22.0|1|0|A/5 21171|7.2500||S
1|2|True|1|Cumings, Mrs Bradley (Florence Briggs Th ...|female|38.0|1|0|PC 17599|71.2833|C85|C
2|3|True|3|Heikkinen, 누락. Laina|female|26.0|0|0|STON/O2. 3101282|7.9250||S

작업 영역의 실험에서 데이터 집합을 다시 사용 하 고 공유 하려면 [데이터 집합을 등록](#register-datasets)합니다.

## <a name="explore-data"></a>데이터 탐색

데이터 집합을 만들고 [등록](#register-datasets) 한 후 모델 학습 전에 데이터 탐색을 위해 전자 필기장에 로드할 수 있습니다. 데이터 탐색을 수행할 필요가 없는 경우 데이터 [집합을 사용 하 여 학습](how-to-train-with-datasets.md)에서 ML 실험을 제출 하기 위해 학습 스크립트에서 데이터 집합을 사용 하는 방법을 참조 하세요.

FileDatasets의 경우 데이터 집합을 **탑재** 하거나 **다운로드** 하 고 데이터 탐색에 일반적으로 사용 하는 python 라이브러리를 적용할 수 있습니다. [Mount vs 다운로드에 대해 자세히 알아보세요](how-to-train-with-datasets.md#mount-vs-download).

```python
# download the dataset 
dataset.download(target_path='.', overwrite=False) 

# mount dataset to the temp directory at `mounted_path`

import tempfile
mounted_path = tempfile.mkdtemp()
mount_context = dataset.mount(mounted_path)

mount_context.start()
```

TabularDatasets의 경우 메서드를 사용 [`to_pandas_dataframe()`](/python/api/azureml-core/azureml.data.tabulardataset?preserve-view=true&view=azure-ml-py#to-pandas-dataframe-on-error--null---out-of-range-datetime--null--) 하 여 데이터 프레임에서 데이터를 볼 수 있습니다. 

```python
# preview the first 3 rows of titanic_ds
titanic_ds.take(3).to_pandas_dataframe()
```

|인덱싱할|PassengerId|Survived|Pclass|Name|성|나이|SibSp|Parch|티켓|요금|Cabin|Embarked
-|-----------|--------|------|----|---|---|-----|-----|------|----|-----|--------|
0|1|False|3|Braund, Mr. Owen Harris|male|22.0|1|0|A/5 21171|7.2500||S
1|2|True|1|Cumings, Mrs Bradley (Florence Briggs Th ...|female|38.0|1|0|PC 17599|71.2833|C85|C
2|3|True|3|Heikkinen, 누락. Laina|female|26.0|0|0|STON/O2. 3101282|7.9250||S

## <a name="create-a-dataset-from-pandas-dataframe"></a>Pandas 데이터 프레임에서 데이터 집합 만들기

In memory pandas 데이터 프레임에서 TabularDataset을 만들려면 csv와 같은 로컬 파일에 데이터를 작성 하 고 해당 파일에서 데이터 집합을 만듭니다. 다음 코드에서는이 워크플로를 보여 줍니다.

```python
# azureml-core of version 1.0.72 or higher is required
# azureml-dataprep[pandas] of version 1.1.34 or higher is required

from azureml.core import Workspace, Dataset
local_path = 'data/prepared.csv'
dataframe.to_csv(local_path)

# upload the local file to a datastore on the cloud

subscription_id = 'xxxxxxxxxxxxxxxxxxxxx'
resource_group = 'xxxxxx'
workspace_name = 'xxxxxxxxxxxxxxxx'

workspace = Workspace(subscription_id, resource_group, workspace_name)

# get the datastore to upload prepared data
datastore = workspace.get_default_datastore()

# upload the local file from src_dir to the target_path in datastore
datastore.upload(src_dir='data', target_path='data')

# create a dataset referencing the cloud location
dataset = Dataset.Tabular.from_delimited_files(path = [(datastore, ('data/prepared.csv'))])
```

> [!TIP]
> 공용 미리 보기 메서드 및를 사용 하 여 단일 메서드를 사용 하 여 메모리 내 spark 또는 pandas 데이터 프레임에서 TabularDataset을 만들고 등록 [`register_spark_dataframe()`](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?preserve-view=true&view=azure-ml-py#methods) [`register_pandas_dataframe()`](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory?preserve-view=true&view=azure-ml-py#methods) 합니다. 이러한 register 메서드는 [실험적](/python/api/overview/azure/ml/?preserve-view=true&view=azure-ml-py#stable-vs-experimental) 미리 보기 기능 이며 언제 든 지 변경 될 수 있습니다. 
> 
>  이러한 메서드는 데이터를 기본 저장소에 업로드 하므로 저장소 비용이 발생 합니다. 

## <a name="register-datasets"></a>데이터 집합 등록

만들기 프로세스를 완료 하려면 작업 영역을 사용 하 여 데이터 집합을 등록 합니다. 메서드를 사용 하 여 [`register()`](/python/api/azureml-core/azureml.data.abstract_dataset.abstractdataset?preserve-view=true&view=azure-ml-py#&preserve-view=trueregister-workspace--name--description-none--tags-none--create-new-version-false-) 다른 사람과 공유 하 고 작업 영역의 실험에서 다시 사용 하기 위해 작업 영역에 데이터 집합을 등록 합니다.

```Python
titanic_ds = titanic_ds.register(workspace=workspace,
                                 name='titanic_ds',
                                 description='titanic training data')
```

## <a name="create-datasets-using-azure-resource-manager"></a>Azure Resource Manager를 사용 하 여 데이터 집합 만들기

에는 [https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-dataset-create-*](https://github.com/Azure/azure-quickstart-templates/tree/master/) 데이터 집합을 만드는 데 사용할 수 있는 여러 가지 템플릿이 있습니다.

이러한 템플릿 사용에 대 한 자세한 내용은 [Azure Resource Manager 템플릿을 사용 하 여 Azure Machine Learning에 대 한 작업 영역 만들기](how-to-create-workspace-template.md)를 참조 하세요.


## <a name="create-datasets-with-azure-open-datasets"></a>Azure Open 데이터 집합을 사용 하 여 데이터 집합 만들기

[Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/)는 기계 학습 솔루션에 시나리오별 기능을 추가하여 보다 정확한 모델을 만들 수 있는 큐레이팅된 공개 데이터 세트입니다. 데이터 세트에는 기계 학습 모델을 학습시키고 예측 솔루션을 보강할 수 있는 날씨, 인구, 휴일, 공공 안전 및 위치에 대한 공개 도메인 데이터가 포함되어 있습니다. 개방형 데이터 집합은 Microsoft Azure의 클라우드에 있으며 SDK 및 studio에 모두 포함 되어 있습니다.

[Azure Open 데이터 집합에서 Azure Machine Learning 데이터 집합](../open-datasets/how-to-create-azure-machine-learning-dataset-from-open-dataset.md)을 만드는 방법에 대해 알아봅니다. 

## <a name="train-with-datasets"></a>데이터 세트로 학습

기계 학습 실험에서 ML 모델 학습을 위해 데이터 집합을 사용 합니다. [데이터 집합을 사용 하 여 학습 하는 방법에 대 한 자세한 정보](how-to-train-with-datasets.md)

## <a name="version-datasets"></a>버전 데이터 집합

새 버전을 만들어 동일한 이름으로 새 데이터 집합을 등록할 수 있습니다. 데이터 집합 버전은 실험 또는 향후 복제를 위해 특정 버전의 데이터 집합을 적용할 수 있도록 데이터의 상태에 책갈피를 지정 하는 방법입니다. [데이터 집합 버전](how-to-version-track-datasets.md)에 대해 자세히 알아보세요.
```Python
# create a TabularDataset from Titanic training data
web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
             'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

# create a new version of titanic_ds
titanic_ds = titanic_ds.register(workspace = workspace,
                                 name = 'titanic_ds',
                                 description = 'new titanic training data',
                                 create_new_version = True)
```

## <a name="next-steps"></a>다음 단계

* [데이터 집합을 사용 하 여 학습 하는 방법을](how-to-train-with-datasets.md)알아봅니다.
* 자동화 된 machine learning을 사용 하 여 [TabularDatasets으로 학습](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)합니다.
* 더 많은 데이터 집합 학습 예제는 [샘플 노트북](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/)을 참조 하세요.
