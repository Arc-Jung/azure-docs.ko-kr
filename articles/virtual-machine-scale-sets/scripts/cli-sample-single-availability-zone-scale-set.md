---
title: Azure CLI 샘플 - 단일 영역 확장 집합
description: 이 스크립트는 단일 가용성 영역에서 Ubuntu를 실행하는 Azure 가상 머신 확장 집합을 만듭니다.
author: ju-shim
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.date: 03/27/2018
ms.author: jushiman
ms.custom: mvc
ms.openlocfilehash: cd41f21c5390ca0d2c62c1989d46f3d4c0ec36f8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81011007"
---
# <a name="create-a-single-zone-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI를 사용하여 단일 영역 가상 머신 확장 집합 만들기
이 스크립트는 단일 가용성 영역에서 Ubuntu를 실행하는 가상 머신 확장 집합을 만듭니다. 스크립트를 실행하면 RDP를 통해 가상 머신에 액세스할 수 있습니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh "Create single-zone scale set")]

## <a name="clean-up-deployment"></a>배포 정리
다음 명령을 실행하여 리소스 그룹, 확장 집합 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명
이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 머신 확장 집합 및 관련된 모든 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/ad/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az vmss create](/cli/azure/vmss) | 가상 머신 확장 집합을 만들고 가상 네트워크, 서브넷 및 네트워크 보안 그룹에 연결합니다. 부하 분산 장치도 여러 VM 인스턴스에 트래픽을 분산하기 위해 만듭니다. 또한 이 명령은 사용할 VM 이미지와 관리 자격 증명도 지정합니다.  |
| [az group delete](/cli/azure/ad/group) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계
Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure/overview)를 참조하세요.

추가 가상 머신 확장 집합 Azure CLI 스크립트 샘플은 [Azure 가상 머신 확장 집합 설명서](../cli-samples.md)에서 찾을 수 있습니다.
