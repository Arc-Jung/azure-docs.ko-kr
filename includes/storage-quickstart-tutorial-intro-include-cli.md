---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 44ee258567ca357687feb24337f2d5974e2532b0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182817"
---
## <a name="create-a-resource-group"></a>리소스 그룹 만들기

[az group create](/cli/azure/group) 명령을 사용하여 Azure 리소스 그룹을 만듭니다. 리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다.

```azurecli-interactive
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-a-storage-account"></a>스토리지 계정 만들기

[az storage account create](/cli/azure/storage/account) 명령을 사용하여 범용 스토리지 계정을 만듭니다. 범용 스토리지 계정은 4개의 모든 서비스(Blob, 파일, 테이블 및 큐)에 사용할 수 있습니다. 

```azurecli-interactive
az storage account create \
    --name mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --sku Standard_LRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>스토리지 계정 자격 증명 지정

Azure CLI는 이 자습서의 명령 대부분에 스토리지 계정 자격 증명을 필요로 합니다. 이를 수행하는 여러 가지 옵션이 있지만 가장 쉬운 방법 중 하나는 `AZURE_STORAGE_ACCOUNT` 및 `AZURE_STORAGE_KEY` 환경 변수를 설정하는 것입니다.

먼저, [az storage account keys list](/cli/azure/storage/account/keys) 명령을 사용하여 스토리지 계정 키를 표시합니다.

```azurecli-interactive
az storage account keys list \
    --account-name mystorageaccount \
    --resource-group myResourceGroup \
    --output table
```

이제 `AZURE_STORAGE_ACCOUNT` 및 `AZURE_STORAGE_KEY` 환경 변수를 설정합니다. `export` 명령을 사용하여 Bash 셸에서 이 작업을 수행할 수 있습니다.

```bash
export AZURE_STORAGE_ACCOUNT="mystorageaccountname"
export AZURE_STORAGE_KEY="myStorageAccountKey"
```
