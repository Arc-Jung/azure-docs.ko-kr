---
title: Azure CLI 스크립트 예제 - Batch의 Windows 풀
description: 이 스크립트는 Azure Batch에서 Windows 컴퓨팅 노드 풀을 만들고 관리할 수 있는 Azure CLI 명령 중 일부를 보여줍니다.
ms.topic: sample
ms.date: 12/12/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8863831d236581f07df85acda26268659e3a7d8b
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87500268"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI 예제: Azure Batch에서 Windows 풀 만들기 및 관리

이 스크립트는 Azure Batch에서 Windows 컴퓨팅 노드 풀을 만들고 관리할 수 있는 Azure CLI 명령 중 일부를 보여줍니다. Windows 풀은 Cloud Services 구성 또는 Virtual Machine 구성의 두 가지 방법으로 구성할 수 있습니다. 이 예제에는 Cloud Services 구성을 사용하여 Windows 풀을 만드는 방법을 보여줍니다.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택하는 경우 이 문서에서 Azure CLI 버전 2.0.20 이상을 실행해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요. 

## <a name="example-script"></a>예제 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 실행하여 리소스 그룹 및 모든 관련 리소스를 제거합니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch 계정을 만듭니다. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | 추가 CLI 상호 작용을 위해 지정된 Batch 계정에 대해 인증합니다. |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | 컴퓨팅 노드 풀을 만듭니다.  |
| [az batch pool set](/cli/azure/batch/pool#az-batch-pool-set) | 풀의 속성을 업데이트합니다.  |
| [az batch pool autoscale enable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-enable) | 풀에서 자동 확장을 사용하고 수식을 적용합니다.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | 풀의 속성을 표시합니다.  |
| [az batch pool autoscale disable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-disable) | 풀에서 자동 확장을 사용하지 않습니다. |
| [az group delete](/cli/azure/group#az-group-delete) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |


## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.
