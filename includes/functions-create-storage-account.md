---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 25cfcefb600bc12de3dad5b6fe2bcb76d00f0198
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "68444109"
---
## <a name="create-an-azure-storage-account"></a>Azure Storage 계정 만들기

함수는 Azure Storage의 범용 계정을 사용하여 함수에 대한 상태 및 기타 정보를 유지 관리합니다. [az storage account create](/cli/azure/storage/account) 명령을 사용하여 만든 리소스 그룹에 범용 스토리지 계정을 만듭니다.

다음 명령에서 `<storage_name>` 자리 표시자를 전역적으로 고유한 스토리지 계정 이름으로 바꿉니다. Storage 계정 이름은 3자에서 24자 사이여야 하고 숫자 및 소문자만 포함할 수 있습니다.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```
