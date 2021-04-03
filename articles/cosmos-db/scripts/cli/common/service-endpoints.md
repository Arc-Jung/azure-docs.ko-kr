---
title: 가상 네트워크 서비스 엔드포인트를 사용하여 Azure Cosmos 계정 만들기
description: 가상 네트워크 서비스 엔드포인트를 사용하여 Azure Cosmos 계정 만들기
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 09dce9658dc602e68d87f7b989301f6934efbce9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94563072"
---
# <a name="create-an-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Azure CLI를 사용하여 가상 네트워크 서비스 엔드포인트와 함께 Azure Cosmos 계정 만들기
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- 이 문서에는 Azure CLI 버전 2.9.1 이상이 필요합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다.

## <a name="sample-script"></a>샘플 스크립트

이 샘플에서는 프런트 및 백 엔드 서브넷이 있는 새 가상 네트워크를 만들고 `Microsoft.AzureCosmosDB`에 대한 서비스 엔드포인트를 사용하도록 설정합니다. 그런 다음, 이 서브넷에 대한 리소스 ID를 검색하여 Azure Cosmos 계정에 적용하고 해당 계정에 대해 서비스 엔드포인트를 사용하도록 설정합니다.

> [!NOTE]
> 이 샘플에서는 Core(SQL) API 계정을 사용하는 방법을 보여줍니다. 이 샘플을 다른 API에 사용하려면 아래 스크립트의 `enable-virtual-network` 및 `virtual-network-rules` 매개 변수를 API 특정 스크립트에 적용합니다.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints.sh "Create an Azure Cosmos account with service endpoints.")]

## <a name="clean-up-deployment"></a>배포 정리

스크립트 샘플을 실행한 후에 다음 명령을 사용하여 리소스 그룹 및 관련된 모든 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Azure 가상 네트워크를 만듭니다. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Azure 가상 네트워크에 대한 서브넷을 만듭니다. |
| [az network vnet subnet show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Azure 가상 네트워크에 대한 서브넷을 반환합니다. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB 계정을 만듭니다. |
| [az group delete](/cli/azure/resource#az-resource-delete) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure Cosmos DB CLI에 대한 자세한 내용은 [Azure Cosmos DB CLI 설명서](/cli/azure/cosmosdb)를 참조하세요.

모든 Azure Cosmos DB CLI 스크립트 샘플은 [Azure Cosmos DB CLI GitHub 리포지토리](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)에서 확인할 수 있습니다.
