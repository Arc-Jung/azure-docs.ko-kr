---
title: 클라우드의 데이터 액세스 보안
titleSuffix: Azure Machine Learning
description: Azure Machine Learning 데이터 저장소 및 데이터 집합을 사용 하 여 Azure에서 데이터 저장소에 안전 하 게 연결 하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 08/31/2020
ms.custom: devx-track-python, data4ml
ms.openlocfilehash: 601be8409db22162a410d481e6609d378718a7b4
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102503592"
---
# <a name="secure-data-access-in-azure-machine-learning"></a>Azure Machine Learning에서 데이터 액세스 보안

Azure Machine Learning를 사용 하면 클라우드에서 데이터에 쉽게 연결할 수 있습니다. 기본 저장소 서비스에 대 한 추상화 계층을 제공 하므로 저장소 유형과 관련 된 코드를 작성 하지 않고도 안전 하 게 데이터에 액세스 하 고 작업할 수 있습니다. 또한 Azure Machine Learning는 다음과 같은 데이터 기능을 제공 합니다.

*    Pandas 및 Spark 데이터 프레임와의 상호 운용성
*    데이터 계보 버전 관리 및 추적
*    데이터 레이블 지정 
*    데이터 드리프트 모니터링
    
## <a name="data-workflow"></a>데이터 워크플로

클라우드 기반 저장소 솔루션에서 데이터를 사용할 준비가 되 면 다음과 같은 데이터 배달 워크플로를 사용 하는 것이 좋습니다. 이 워크플로는 azure [저장소 계정](../storage/common/storage-account-create.md?tabs=azure-portal) 및 데이터를 azure의 클라우드 기반 저장소 서비스에 보유 하 고 있다고 가정 합니다. 

1. Azure storage에 연결 정보를 저장 하는 [Azure Machine Learning 데이터](#datastores) 저장소를 만듭니다.

2. 해당 데이터 저장소에서 기본 저장소에 있는 특정 파일을 가리키도록 [Azure Machine Learning 데이터 집합](#datasets) 을 만듭니다. 

3. 기계 학습 실험에서 해당 데이터 집합을 사용 하려면 다음 중 하나를 수행할 수 있습니다.
    1. 모델 학습을 위해 실험의 계산 대상에 탑재 합니다.

        **OR** 

    1. 자동화 된 기계 학습 (자동화 된 기계 학습) 실험 실행, 기계 학습 파이프라인 또는 [Azure Machine Learning 디자이너](concept-designer.md)와 같은 Azure Machine Learning 솔루션에서 직접 사용 하세요.

4. 데이터 드리프트를 검색할 모델 출력 데이터 집합에 대 한 [데이터 집합 모니터](#drift) 를 만듭니다. 

5. 데이터 드리프트가 검색 되 면 입력 데이터 집합을 업데이트 하 고 모델을 적절 하 게 다시 학습.

다음 다이어그램에서는이 권장 워크플로의 시각적 데모를 제공 합니다.

![다이어그램은 데이터 집합으로 흐르는 데이터 저장소로 이동 하는 Azure Storage 서비스를 보여 줍니다. 데이터 집합은 데이터 드리프트로 흐르는 모델 학습으로 이동 하 여 데이터 집합으로 다시 흐릅니다.](./media/concept-data/data-concept-diagram.svg)

<a name="datastores"></a>
## <a name="connect-to-storage-with-datastores"></a>데이터 저장소를 사용 하 여 저장소에 연결

데이터 저장소는 Azure에서 데이터 저장소에 대 한 연결 정보를 안전 하 게 유지 하므로 스크립트에서 코딩할 필요가 없습니다. Azure Machine Learning 저장소 계정에 쉽게 연결 하 고 기본 저장소 서비스의 데이터에 액세스 하는 데이터 저장소를 [등록 하 고 만듭니다](how-to-access-data.md) . 

데이터 저장소로 등록할 수 있는 Azure에서 지원 되는 클라우드 기반 저장소 서비스:

+ Azure Blob 컨테이너
+ Azure 파일 공유
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL Database
+ Azure Database for PostgreSQL
+ Databricks 파일 시스템
+ Azure Database for MySQL

>[!TIP]
> 일반적으로 데이터 저장소를 만드는 데 사용할 수 있는 기능에는 서비스 주체 또는 SAS (공유 액세스 서명) 토큰과 같은 저장소 서비스에 액세스 하기 위한 자격 증명 기반 인증이 필요 합니다. 이러한 자격 증명은 작업 영역에 대 한 *읽기* 권한이 있는 사용자가 액세스할 수 있습니다. <br><br>이 문제가 있는 경우  [저장소 서비스 (미리 보기)에 대 한 id 기반 데이터 액세스를 사용 하는 데이터 저장소를 만듭니다](how-to-identity-based-data-access.md). 이 기능은 [실험적](/python/api/overview/azure/ml/#stable-vs-experimental) 미리 보기 기능으로, 언제 든 지 변경 될 수 있습니다.

<a name="datasets"></a>
## <a name="reference-data-in-storage-with-datasets"></a>데이터 집합을 사용 하 여 저장소의 데이터 참조

Azure Machine Learning 데이터 집합은 데이터의 복사본이 아닙니다. 데이터 집합을 만들면 해당 메타 데이터의 복사본과 함께 저장소 서비스에서 데이터에 대 한 참조를 만들 수 있습니다. 

데이터 집합은 지연 평가 되 고 데이터는 기존 위치에 남아 있기 때문에

* 추가 저장소 비용이 들지 않습니다.
* 실수로 원래 데이터 원본을 변경 하는 것은 위험 하지 않습니다.
* ML 워크플로 성능 속도를 향상 시킵니다.

저장소에서 데이터와 상호 작용 하려면 기계 학습 작업을 위한 사용 가능 개체에 데이터를 패키지할 데이터 [집합을 만듭니다](how-to-create-register-datasets.md) . 데이터 집합을 작업 영역에 등록 하 여 데이터 수집 복잡성 없이 다른 실험에서 데이터 집합을 공유 하 고 다시 사용할 수 있습니다.

데이터 집합은 로컬 파일, 공용 url, [Azure Open 데이터 집합](https://azure.microsoft.com/services/open-datasets/)또는 datastores를 통해 azure storage 서비스에서 만들 수 있습니다. 

데이터 집합에는 두 가지 유형이 있습니다. 

+ [Filedataset](/python/api/azureml-core/azureml.data.file_dataset.filedataset) 은 데이터 저장소 또는 public url의 단일 또는 여러 파일을 참조 합니다. 데이터가 이미 정리 되어 학습 실험에서 사용할 준비가 된 경우 FileDatasets에서 참조 하는 파일을 계산 대상으로 [다운로드 하거나 탑재할](how-to-train-with-datasets.md#mount-files-to-remote-compute-targets) 수 있습니다.

+ [TabularDataset](/python/api/azureml-core/azureml.data.tabulardataset) 는 제공 된 파일 또는 파일 목록을 구문 분석 하 여 테이블 형식으로 데이터를 나타냅니다. 추가 조작 및 정리를 위해 TabularDataset를 pandas 또는 Spark 데이터 프레임에 로드할 수 있습니다. TabularDatasets에서 만들 수 있는 데이터 형식의 전체 목록은 [TabularDatasetFactory 클래스](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory)를 참조 하세요.

다음 설명서에서 추가 데이터 집합 기능을 찾을 수 있습니다.

+ [버전 및 추적](how-to-version-track-datasets.md) 데이터 집합 계보.
+ 데이터 드리프트 검색에 도움이 되도록 데이터 [집합을 모니터링](how-to-monitor-datasets.md) 합니다.    

## <a name="work-with-your-data"></a>데이터 작업

데이터 집합을 사용 하면 Azure Machine Learning 기능과의 원활한 통합을 통해 많은 기계 학습 작업을 수행할 수 있습니다. 

+ [데이터 레이블 지정 프로젝트](#label)를 만듭니다.
+ 기계 학습 모델 학습:
     + [자동 ML 실험](how-to-use-automated-ml-for-ml-models.md)
     + [디자이너](tutorial-designer-automobile-price-train-score.md#import-data)
     + [전자](how-to-train-with-datasets.md)
     + [파이프라인 Azure Machine Learning](./how-to-create-machine-learning-pipelines.md)
+ [기계 학습 파이프라인](./how-to-create-machine-learning-pipelines.md)에서 [일괄 처리 유추](./tutorial-pipeline-batch-scoring-classification.md) 를 사용 하 여 점수 매기기를 위한 데이터 집합에 액세스 합니다.
+ [데이터 드리프트](#drift) 검색을 위한 데이터 집합 모니터를 설정 합니다.

<a name="label"></a>

## <a name="label-data-with-data-labeling-projects"></a>데이터 레이블 프로젝트를 사용 하 여 데이터 레이블 지정

많은 양의 데이터에 레이블을 지정하는 것은 기계 학습 프로젝트에서 골칫거리인 경우가 많습니다. 이미지 분류 또는 개체 검색과 같은 컴퓨터 비전 구성 요소가 있는 컴퓨터에는 일반적으로 수천 개의 이미지와 해당 레이블이 필요 합니다.

Azure Machine Learning은 레이블 지정 프로젝트를 만들고, 관리하고, 모니터링할 수 있는 중앙 위치를 제공합니다. 레이블 지정 프로젝트를 사용하면 데이터, 레이블 및 팀 멤버를 조정하여 레이블 지정 작업을 더 효율적으로 관리할 수 있습니다. 현재 지원되는 작업은 이미지 분류(다중 레이블 또는 다중 클래스) 및 경계 상자를 사용하는 개체 식별입니다.

[데이터 레이블 지정 프로젝트](how-to-create-labeling-projects.md)를 만들고 기계 학습 실험에서 사용할 데이터 집합을 출력 합니다.

<a name="drift"></a>

## <a name="monitor-model-performance-with-data-drift"></a>데이터 드리프트를 사용 하 여 모델 성능 모니터링

기계 학습의 컨텍스트에서 데이터 드리프트는 모델의 성능 저하를 초래 하는 모델 입력 데이터의 변경 내용입니다. 모델 정확도가 시간에 따라 저하 되는 가장 중요 한 이유 중 하나 이므로 데이터 드리프트를 모니터링 하면 모델 성능 문제를 감지 하는 데 도움이 됩니다.

데이터 집합의 새 데이터에 대 한 데이터 드리프트를 검색 하 고 경고 하는 방법에 대 한 자세한 내용은 데이터 [집합 모니터 만들기](how-to-monitor-datasets.md) 문서를 참조 하세요.

## <a name="next-steps"></a>다음 단계 

+ [이러한 단계를 사용 하 여](how-to-create-register-datasets.md) Azure Machine Learning Studio 또는 Python SDK를 사용 하 여 데이터 집합을 만듭니다.
+ [샘플 노트북](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/work-with-data/)을 사용 하 여 데이터 집합 학습 예제를 사용해 보세요.