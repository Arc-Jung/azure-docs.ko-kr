---
title: 파일 포함
description: 포함 파일
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 11/02/2020
ms.openlocfilehash: 1f7abcdd1439fe5e6eeb2f718862f4875c61230c
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509171"
---
모델을 호스팅하는 데 사용하는 컴퓨팅 대상은 배포된 엔드포인트의 비용 및 가용성에 영향을 줍니다. 이 표를 사용하여 적절한 컴퓨팅 대상을 선택합니다.

| 컴퓨팅 대상 | 사용 대상 | GPU 지원 | FPGA 지원 | Description |
| ----- | ----- | ----- | ----- | ----- |
| [로컬&nbsp;웹&nbsp;서비스](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | 테스트/디버깅 | &nbsp; | &nbsp; | 제한된 테스트 및 문제 해결에 사용합니다. 하드웨어 가속은 로컬 시스템에서 라이브러리를 사용하는지에 따라 달라집니다.
| [AKS(Azure Kubernetes Service)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | 실시간 유추 |  [예](../articles/machine-learning/how-to-deploy-inferencing-gpus.md)(웹 서비스 배포) | [예](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |대규모 프로덕션 배포에 사용합니다. 배포된 서비스의 빠른 응답 시간 및 자동 크기 조정을 제공합니다. 클러스터 자동 크기 조정은 Azure Machine Learning SDK를 통해 지원되지 않습니다. AKS 클러스터의 노드를 변경하려면 Azure Portal에서 AKS 클러스터의 UI를 사용합니다. <br/><br/> 디자이너에서 지원됩니다. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | 테스트 또는 개발 | &nbsp;  | &nbsp; | 48GB 미만의 RAM이 필요한 소규모 CPU 기반 워크로드에 사용합니다. <br/><br/> 디자이너에서 지원됩니다. |
| [Azure Machine Learning 컴퓨팅 클러스터](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md) | 일괄 처리&nbsp;유추 | [예](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md)(기계 학습 파이프라인) | &nbsp;  | 서버리스 컴퓨팅에서 일괄 처리 채점을 실행합니다. 우선 순위가 보통이거나 낮은 VM을 지원합니다. |

> [!NOTE]
> 로컬, Azure Machine Learning 컴퓨팅 및 Azure Machine Learning 컴퓨팅 클러스터와 같은 컴퓨팅 대상은 학습 및 실험을 위해 GPU를 지원하지만, _웹 서비스로 배포할 때_ GPU를 유추에 사용하는 것은 AKS에서만 지원됩니다.
>
> _기계 학습 파이프라인을 통해 채점할 때_ GPU를 유추에 사용하는 것은 Azure Machine Learning 컴퓨팅에서만 지원됩니다.

> [!NOTE]
> * 컨테이너 인스턴스는 크기가 1GB 미만인 작은 모델에만 적합합니다.
> * 큰 모델의 개발/테스트에는 단일 노드 AKS 클러스터를 사용합니다.