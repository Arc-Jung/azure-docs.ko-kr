---
title: ML 파이프라인에서 데이터 이동
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 파이프라인이 데이터를 수집 하는 방법과 파이프라인 단계 간에 데이터를 관리 하 고 이동 하는 방법에 대해 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 02/26/2021
ms.topic: conceptual
ms.custom: how-to, contperf-fy20q4, devx-track-python, data4ml
ms.openlocfilehash: 5a83211654ad1abafff59d5968c191ec1fa63616
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692405"
---
# <a name="moving-data-into-and-between-ml-pipeline-steps-python"></a>ML 파이프라인 단계로/단계 간에 데이터 이동(Python)

이 문서에서는 Azure Machine Learning 파이프라인의 단계 간에 데이터를 가져오고, 변환 하 고, 이동 하는 코드를 제공 합니다. Azure Machine Learning에서 데이터가 작동 하는 방식에 대 한 개요는 [Azure storage 서비스에서 데이터 액세스](how-to-access-data.md)를 참조 하세요. Azure Machine Learning 파이프라인의 이점과 구조는 [Azure Machine Learning 파이프라인 이란?](concept-ml-pipelines.md)을 참조 하세요.

이 문서에서 설명하는 방법:

- `Dataset`기존 데이터에 대 한 개체 사용
- 사용자의 단계 내에서 데이터에 액세스
- `Dataset`데이터를 하위 집합 (예: 학습 및 유효성 검사 하위 집합)으로 분할
- `OutputFileDatasetConfig`다음 파이프라인 단계로 데이터를 전송 하는 개체 만들기
- `OutputFileDatasetConfig`파이프라인 단계에 대 한 입력으로 개체 사용
- `Dataset`Wisƒh에서 유지할 새 개체 만들기 `OutputFileDatasetConfig`

## <a name="prerequisites"></a>사전 요구 사항

필요한 사항:

- Azure 구독 Azure 구독이 없는 경우 시작하기 전에 체험 계정을 만듭니다. [Azure Machine Learning 평가판 또는 유료 버전](https://aka.ms/AMLFree)을 사용해 보세요.

- [Python용 Azure Machine Learning SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py) 또는 [Azure Machine Learning 스튜디오](https://ml.azure.com/)에 대한 액세스 권한

- Azure Machine Learning 작업 영역
  
  [Azure Machine Learning 작업 영역](how-to-manage-workspace.md)을 만들거나 Python SDK를 통해 기존 작업 영역을 사용합니다. `Workspace` 및 `Datastore` 클래스를 가져오고, `from_config()` 함수를 사용하여 `config.json` 파일의 구독 정보를 로드합니다. 이 함수는 기본적으로 현재 디렉터리에서 JSON 파일을 검색 하지만를 사용 하 여 파일을 가리키도록 경로 매개 변수를 지정할 수도 있습니다 `from_config(path="your/file/path")` .

   ```python
   import azureml.core
   from azureml.core import Workspace, Datastore
        
   ws = Workspace.from_config()
   ```

- 일부 기존 데이터. 이 문서에서는 [Azure blob 컨테이너](../storage/blobs/storage-blobs-overview.md)를 사용 하는 방법을 간략하게 설명 합니다.

- 선택 사항: [AZURE MACHINE LEARNING SDK를 사용 하 여 machine learning 파이프라인 만들기 및 실행](./how-to-create-machine-learning-pipelines.md)에 설명 된 것과 같은 기존 machine learning 파이프라인입니다.

## <a name="use-dataset-objects-for-pre-existing-data"></a>`Dataset`기존 데이터에 대 한 개체 사용 

파이프라인으로 데이터를 수집 하는 기본 방법은 [Dataset](/python/api/azureml-core/azureml.core.dataset%28class%29?preserve-view=true&view=azure-ml-py) 개체를 사용 하는 것입니다. `Dataset` 개체는 작업 영역 전체에서 사용할 수 있는 영구적 데이터를 나타냅니다.

개체를 만들고 등록 하는 방법에는 여러 가지가 있습니다 `Dataset` . 테이블 형식 데이터 집합은 하나 이상의 파일에서 사용할 수 있는 구분 된 데이터에 대 한 것입니다. 파일 데이터 집합은 이진 데이터 (예: 이미지) 또는 구문 분석 하는 데이터에 대 한 것입니다. 개체를 만드는 가장 간단한 프로그래밍 방법은 `Dataset` 작업 영역 저장소 또는 공용 url에서 기존 blob을 사용 하는 것입니다.

```python
datastore = Datastore.get(workspace, 'training_data')
iris_dataset = Dataset.Tabular.from_delimited_files(DataPath(datastore, 'iris.csv'))

datastore_path = [
    DataPath(datastore, 'animals/dog/1.jpg'),
    DataPath(datastore, 'animals/dog/2.jpg'),
    DataPath(datastore, 'animals/cat/*.jpg')
]
cats_dogs_dataset = Dataset.File.from_files(path=datastore_path)
```

다른 옵션을 사용 하 여 데이터 집합을 만들고 다른 원본에서 데이터를 등록 하 고 Azure Machine Learning 검토 하는 방법에 대 한 추가 옵션을 사용 하 여 데이터 크기가 계산 용량과 상호 작용 하는 방식을 이해 하 고 버전을 관리 하는 방법은 [Azure Machine Learning 데이터 집합 만들기](how-to-create-register-datasets.md)를 참조 

### <a name="pass-datasets-to-your-script"></a>데이터 집합을 스크립트에 전달 합니다.

데이터 집합의 경로를 스크립트에 전달 하려면 `Dataset` 개체의 메서드를 사용 `as_named_input()` 합니다. 결과 개체를 스크립트에 인수로 전달 하거나 `DatasetConsumptionConfig` 파이프라인 스크립트에 인수를 사용 하 여를 `inputs` 사용 하 여 데이터 집합을 검색할 수 있습니다 `Run.get_context().input_datasets[]` .

명명 된 입력을 만든 후에는 또는의 액세스 모드를 선택할 수 `as_mount()` 있습니다 `as_download()` . 스크립트에서 데이터 집합의 모든 파일을 처리 하 고 계산 리소스의 디스크가 데이터 집합에 충분 한 크기 이면 다운로드 액세스 모드를 선택 하는 것이 좋습니다. 다운로드 액세스 모드는 런타임에 데이터를 스트리밍하는 오버 헤드를 방지 합니다. 스크립트가 데이터 집합의 하위 집합에 액세스 하거나 너무 커서 계산에 사용할 수 없는 경우 탑재 액세스 모드를 사용 합니다. 자세한 내용은 [탑재 및 다운로드를 참조 하세요.](./how-to-train-with-datasets.md#mount-vs-download)

파이프라인 단계에 데이터 집합을 전달 하려면 다음을 수행 합니다.

1. `TabularDataset.as_named_input()`또는 `FileDataset.as_named_input()` (' end '의 ' ')를 사용 하 여 개체를 만듭니다 `DatasetConsumptionConfig` .
1. `as_mount()`또는 `as_download()` 를 사용 하 여 액세스 모드 설정
1. `arguments`또는 인수를 사용 하 여 데이터 집합을 파이프라인 단계에 전달 합니다. `inputs`

다음 코드 조각에서는 생성자 내에서 이러한 단계를 결합 하는 일반적인 패턴을 보여 줍니다 `PythonScriptStep` . 

```python

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[iris_dataset.as_named_input('iris').as_mount()]
)
```

> [!NOTE]
> 이러한 모든 인수 (즉,,, 및)에 대 한 값을 `"train_data"` `"train.py"` `cluster` `iris_dataset` 사용자 고유의 데이터로 바꾸어야 합니다. 위의 코드 조각은 호출의 형태를 보여주고 Microsoft 샘플의 일부가 아닙니다. 

및와 같은 메서드 `random_split()` 를 사용 하 여 `take_sample()` 여러 입력을 만들거나 파이프라인 단계로 전달 되는 데이터의 양을 줄일 수도 있습니다.

```python
seed = 42 # PRNG seed
smaller_dataset = iris_dataset.take_sample(0.1, seed=seed) # 10%
train, test = smaller_dataset.random_split(percentage=0.8, seed=seed)

train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    inputs=[train.as_named_input('train').as_download(), test.as_named_input('test').as_download()]
)
```

### <a name="access-datasets-within-your-script"></a>스크립트 내에서 데이터 집합에 액세스

파이프라인 단계 스크립트에 대 한 명명 된 입력은 개체 내에서 사전으로 사용할 수 있습니다 `Run` . 를 사용 하 여 활성 개체를 검색 `Run` `Run.get_context()` 한 다음를 사용 하 여 명명 된 입력의 사전을 검색 `input_datasets` 합니다. `DatasetConsumptionConfig`인수 대신 인수를 사용 하 여 개체를 전달한 경우 `arguments` `inputs` 코드를 사용 하 여 데이터에 액세스 합니다 `ArgParser` . 다음 코드 조각에서는 두 가지 방법을 모두 보여 줍니다.

```python
# In pipeline definition script:
# Code for demonstration only: It would be very confusing to split datasets between `arguments` and `inputs`
train_step = PythonScriptStep(
    name="train_data",
    script_name="train.py",
    compute_target=cluster,
    arguments=['--training-folder', train.as_named_input('train').as_download()]
    inputs=[test.as_named_input('test').as_download()]
)

# In pipeline script
parser = argparse.ArgumentParser()
parser.add_argument('--training-folder', type=str, dest='train_folder', help='training data folder mounting point')
args = parser.parse_args()
training_data_folder = args.train_folder

testing_data_folder = Run.get_context().input_datasets['test']
```

전달 된 값은 데이터 집합 파일의 경로입니다.

직접 등록 된에 액세스할 수도 있습니다 `Dataset` . 등록 된 데이터 집합은 영구적 이며 작업 영역에서 공유 되므로 직접 검색할 수 있습니다.

```python
run = Run.get_context()
ws = run.experiment.workspace
ds = Dataset.get_by_name(workspace=ws, name='mnist_opendataset')
```

> [!NOTE]
> 위의 코드 조각은 호출의 형식을 보여 주고 Microsoft 샘플에 포함 되지 않습니다. 다양 한 인수를 고유한 프로젝트의 값으로 바꾸어야 합니다.

## <a name="use-outputfiledatasetconfig-for-intermediate-data"></a>`OutputFileDatasetConfig`중간 데이터에 사용

`Dataset`개체는 영구적 데이터만 나타내므로 [`OutputFileDatasetConfig`](/python/api/azureml-core/azureml.data.outputfiledatasetconfig?preserve-view=true&view=azure-ml-py) 파이프라인 단계 **및** 영구 출력 데이터의 임시 데이터 출력에는 개체를 사용할 수 있습니다. `OutputFileDatasetConfig` blob storage, 파일 공유, adlsgen1 또는 adlsgen2에 데이터 쓰기를 지원 합니다. 탑재 모드와 업로드 모드를 모두 지원 합니다. 탑재 모드에서 탑재 된 디렉터리에 기록 된 파일은 파일을 닫을 때 영구적으로 저장 됩니다. 업로드 모드에서 출력 디렉터리에 기록 된 파일은 작업 끝에 업로드 됩니다. 작업이 실패 하거나 취소 되 면 출력 디렉터리는 업로드 되지 않습니다.

 `OutputFileDatasetConfig` 개체의 기본 동작은 작업 영역의 기본 데이터 저장소에 쓰는 것입니다. `OutputFileDatasetConfig`매개 변수를 사용 하 여 개체를에 전달 `PythonScriptStep` `arguments` 합니다.

```python
from azureml.data import OutputFileDatasetConfig
dataprep_output = OutputFileDatasetConfig()
input_dataset = Dataset.get_by_name(workspace, 'raw_data')

dataprep_step = PythonScriptStep(
    name="prep_data",
    script_name="dataprep.py",
    compute_target=cluster,
    arguments=[input_dataset.as_named_input('raw_data').as_mount(), dataprep_output]
    )
```

실행이 끝날 때 개체의 내용을 업로드 하도록 선택할 수 있습니다 `OutputFileDatasetConfig` . 이 경우에는 `as_upload()` 개체와 함께 함수를 사용 하 `OutputFileDatasetConfig` 고 대상에서 기존 파일을 덮어쓸지 여부를 지정 합니다. 

```python
#get blob datastore already registered with the workspace
blob_store= ws.datastores['my_blob_store']
OutputFileDatasetConfig(name="clean_data", destination=(blob_store, 'outputdataset')).as_upload(overwrite=False)
```

> [!NOTE]
> 에 대 한 동시 쓰기는 `OutputFileDatasetConfig` 실패 합니다. 단일을 동시에 사용 하지 마십시오 `OutputFileDatasetConfig` . `OutputFileDatasetConfig`분산 학습을 사용 하는 경우와 같이 다중 처리 상황에서는 단일를 공유 하지 마십시오. 

### <a name="use-outputfiledatasetconfig-as-outputs-of-a-training-step"></a>`OutputFileDatasetConfig`학습 단계의 출력으로 사용

파이프라인의 `PythonScriptStep` 내에서 프로그램의 인수를 사용하여 사용 가능한 출력 경로를 검색할 수 있습니다. 이 단계가 첫 번째이고 출력 데이터를 초기화하는 경우 지정된 경로에 디렉터리를 만들어야 합니다. 그런 다음에 포함 하려는 모든 파일을 작성할 수 있습니다 `OutputFileDatasetConfig` .

```python
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()

# Make directory for file
os.makedirs(os.path.dirname(args.output_path), exist_ok=True)
with open(args.output_path, 'w') as f:
    f.write("Step 1's output")
```

### <a name="read-outputfiledatasetconfig-as-inputs-to-non-initial-steps"></a>`OutputFileDatasetConfig`초기 단계가 아닌 단계에 대 한 입력으로 읽기

초기 파이프라인 단계에서 일부 데이터를 경로에 기록 하 `OutputFileDatasetConfig` 고 해당 초기 단계의 출력이 되 면 이후 단계에 대 한 입력으로 사용할 수 있습니다. 

다음 코드에서: 

* `step1_output_data` PythonScriptStep의 출력이 `step1` ADLS Gen 2 데이터 저장소에 기록 되 고, `my_adlsgen2` 업로드 액세스 모드로 기록 됨을 나타냅니다. ADLS Gen 2 데이터 저장소에 데이터를 다시 쓰기 위해 [역할 권한을 설정](how-to-access-data.md#azure-data-lake-storage-generation-2) 하는 방법에 대해 자세히 알아보세요. 

* `step1`가 완료 되 고 출력이에 지정 된 대상에 기록 되 면 `step1_output_data` 2 단계가 입력으로 사용 될 준비가 된 것입니다 `step1_output_data` . 

```python
# get adls gen 2 datastore already registered with the workspace
datastore = workspace.datastores['my_adlsgen2']
step1_output_data = OutputFileDatasetConfig(name="processed_data", destination=(datastore, "mypath/{run-id}/{output-name}")).as_upload()

step1 = PythonScriptStep(
    name="generate_data",
    script_name="step1.py",
    runconfig = aml_run_config,
    arguments = ["--output_path", step1_output_data]
)

step2 = PythonScriptStep(
    name="read_pipeline_data",
    script_name="step2.py",
    compute_target=compute,
    runconfig = aml_run_config,
    arguments = ["--pd", step1_output_data.as_input()]

)

pipeline = Pipeline(workspace=ws, steps=[step1, step2])
```

## <a name="register-outputfiledatasetconfig-objects-for-reuse"></a>`OutputFileDatasetConfig`재사용할 개체 등록

`OutputFileDatasetConfig`실험 기간 보다 오랫동안 사용할 수 있게 하려는 경우 실험에서 공유 하 고 다시 사용할 수 있도록 작업 영역에 등록 하세요.

```python
step1_output_ds = step1_output_data.register_on_complete(name='processed_data', 
                                                         description = 'files from step1`)
```

## <a name="delete-outputfiledatasetconfig-contents-when-no-longer-needed"></a>`OutputFileDatasetConfig`더 이상 필요 하지 않은 경우 콘텐츠 삭제

Azure는로 작성 된 중간 데이터를 자동으로 삭제 하지 않습니다 `OutputFileDatasetConfig` . 대량의 불필요 한 데이터에 대해 저장소 요금을 방지 하려면 다음 중 하나를 수행 해야 합니다.

* 더 이상 필요 하지 않은 경우 파이프라인 실행의 끝에서 프로그래밍 방식으로 중간 데이터를 삭제 합니다.
* 중간 데이터를 위한 단기 저장소 정책으로 blob 저장소 사용 ( [Azure Blob Storage 액세스 계층을 자동화 하 여 비용 최적화](../storage/blobs/storage/blobs/storage-lifecycle-management-concepts.md)참조) 
* 더 이상 필요 하지 않은 데이터를 정기적으로 검토 및 삭제

자세한 내용은 [Azure Machine Learning에 대 한 비용 계획 및 관리](concept-plan-manage-cost.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

* [Azure 기계 학습 데이터 세트 만들기](how-to-create-register-datasets.md)
* [Azure Machine Learning SDK를 사용하여 기계 학습 파이프라인 만들기 및 실행](./how-to-create-machine-learning-pipelines.md)