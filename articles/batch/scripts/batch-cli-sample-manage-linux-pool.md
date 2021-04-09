---
title: Azure CLI 스크립트 예제 - Batch의 Linux 풀
description: 이 스크립트는 Azure Batch에서 Linux 컴퓨팅 노드 풀을 만들고 관리할 수 있는 Azure CLI 명령 중 일부를 보여줍니다.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: b2e0fbf44be5718cf5577f6bc9aea436968e2fc3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93073560"
---
# <a name="cli-example-create-and-manage-a-linux-pool-in-azure-batch"></a>CLI 예제: Azure Batch에서 Linux 풀 만들기 및 관리

이 스크립트는 Azure Batch에서 Linux 컴퓨팅 노드 풀을 만들고 관리할 수 있는 Azure CLI 명령 중 일부를 보여줍니다.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 이 자습서에는 Azure CLI 버전 2.0.20 이상이 필요합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다. 

## <a name="example-script"></a>예제 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

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
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | 추가 CLI 상호 작용을 위해 지정된 Batch 계정에 대해 인증합니다.  |
| [az batch pool node-agent-skus list](../batch-linux-nodes.md#list-of-virtual-machine-images) | 사용 가능한 노드 에이전트 SKU 및 이미지 정보를 나열합니다.  |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | 컴퓨팅 노드 풀을 만듭니다.  |
| [az batch pool resize](/cli/azure/batch/pool#az-batch-pool-resize) | 지정된 풀에서 실행 중인 VM 수를 조정합니다.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | 풀의 속성을 표시합니다.  |
| [az batch node list](/cli/azure/batch/node#az-batch-node-list) | 지정된 풀의 컴퓨팅 노드를 모두 나열합니다.  |
| [az batch node reboot](/cli/azure/batch/node#az-batch-node-reboot) | 지정된 컴퓨팅 노드를 다시 부팅합니다.  |
| [az batch node delete](/cli/azure/batch/node#az-batch-node-delete) | 지정된 풀에서 나열된 노드를 삭제합니다.  |
| [az group delete](/cli/azure/group#az-group-delete) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.
