---
title: 스토리지 계정 만들기 - Azure Storage
description: 이 방법 문서에서는 Azure Portal, Azure PowerShell 또는 Azure CLI를 사용 하 여 저장소 계정을 만드는 방법에 대해 알아봅니다. Azure Storage 계정은 Azure Storage에서 만든 데이터 개체의 저장 및 액세스를 위해 Microsoft Azure의 고유한 네임스페이스를 제공합니다.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: article
ms.date: 06/28/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e72f8c3ed0658765827c4a9b0669c08fefd9044f
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69016322"
---
# <a name="create-a-storage-account"></a>스토리지 계정 만들기

Azure Storage 계정에는 Blob, 파일, 큐, 테이블, 디스크 등 모든 Azure Storage 데이터 개체가 포함됩니다. 저장소 계정은 Azure Storage 데이터에 대한 고유한 네임 스페이스를 제공하며 전 세계 어디에서나 HTTP 또는 HTTPS를 통해 접근할 수 있게 합니다. Azure Storage 계정의 데이터는 내구성 및 고가용성을 제공하며 안전하고 대규모로 확장 가능합니다.

이 방법 문서에서는 [Azure Portal](https://portal.azure.com/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)또는 [Azure Resource Manager 템플릿을](../../azure-resource-manager/resource-group-overview.md)사용 하 여 저장소 계정을 만드는 방법에 대해 알아봅니다.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>전제 조건

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/)을 만듭니다.

# <a name="portaltabazure-portal"></a>[포털](#tab/azure-portal)

없음

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

이 방법 문서에는 Azure PowerShell 모듈 Az version 0.7 이상이 필요 합니다. `Get-Module -ListAvailable Az`을 실행하여 현재 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-Az-ps)를 참조하세요.

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure에 로그인하고 다음 두 방법 중 하나로 Azure CLI 명령을 실행할 수 있습니다.

- Azure Portal에서 Azure Cloud Shell의 CLI 명령을 실행할 수 있습니다.
- CLI를 설치 하 고 CLI 명령을 로컬로 실행할 수 있습니다.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell 사용

Azure Cloud Shell은 Azure Portal에서 직접 실행할 수 있는 평가판 Bash 셸입니다. Azure CLI는 사전 설치 되며 계정에 사용 하도록 구성 됩니다. Azure Portal의 오른쪽 위 섹션에 있는 메뉴에서 **Cloud Shell** 단추를 클릭 합니다.

[![Cloud Shell](./media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

이 단추는이 방법 문서에 설명 된 단계를 실행 하는 데 사용할 수 있는 대화형 셸을 시작 합니다.

[![포털에서 Cloud Shell 창을 보여 주는 스크린샷](./media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>로컬에서 CLI 설치

Azure CLI를 로컬에서 설치하여 사용할 수도 있습니다. 이 방법 문서에서는 Azure CLI 버전 2.0.4 이상을 이상을 실행 해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요. 

# <a name="templatetabtemplate"></a>[템플릿](#tab/template)

없음

---

## <a name="sign-in-to-azure"></a>Azure에 로그인

# <a name="portaltabazure-portal"></a>[포털](#tab/azure-portal)

[Azure Portal](https://portal.azure.com)에 로그인합니다.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

`Connect-AzAccount` 명령으로 Azure 구독에 로그인하고 화면의 지시에 따라 인증합니다.

```powershell
Connect-AzAccount
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell를 시작 하려면 [Azure Portal](https://portal.azure.com)에 로그인 합니다.

CLI의 로컬 설치에 로그인 하려면 [az login](/cli/azure/reference-index#az-login) 명령을 실행 합니다.

```cli
az login
```

# <a name="templatetabtemplate"></a>[템플릿](#tab/template)

N/A

---

## <a name="create-a-storage-account"></a>스토리지 계정 만들기

이제 스토리지 계정을 만들 준비가 되었습니다.

모든 스토리지 계정은 Azure 리소스 그룹에 속해야 합니다. 리소스 그룹은 Azure 리소스를 그룹화하기 위한 논리적 컨테이너입니다. 스토리지 계정을 만들 때 새 리소스 그룹을 만들거나 기존 리소스 그룹을 사용할 수 있는 옵션이 있습니다. 이 문서에서는 새 리소스 그룹을 만드는 방법을 보여 줍니다.

**범용 v2** 저장소 계정은 모든 Azure Storage 서비스(Blob, 파일, 큐, 테이블 및 디스크)에 대한 액세스를 제공합니다. 여기에 설명 된 단계에서는 범용 v2 저장소 계정을 만들지만 모든 유형의 저장소 계정을 만드는 단계는 비슷합니다.

# <a name="portaltabazure-portal"></a>[포털](#tab/azure-portal)

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

먼저 [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) 명령을 사용하여 PowerShell을 통해 새 리소스 그룹을 만듭니다.

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hard-coding it repeatedly
$resourceGroup = "storage-resource-group"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

`-Location` 매개 변수에 어떤 지역을 지정할지 확실하지 않으면 [Get-AzLocation](/powershell/module/az.resources/get-azlocation) 명령을 사용하여 해당 구독에 대해 지원되는 지역 목록을 검색할 수 있습니다.

```powershell
Get-AzLocation | select Location
$location = "westus"
```

그런 다음 [AzStorageAccount](/powershell/module/az.storage/New-azStorageAccount) 명령을 사용 하 여 읽기 액세스 지역 중복 저장소 (RA-GRS)를 사용 하는 범용 v2 저장소 계정을 만듭니다. 저장소 계정의 이름은 Azure에서 고유 해야 하므로 괄호 안의 자리 표시자 값을 고유한 값으로 바꿉니다.

```powershell
New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name <account-name> `
  -Location $location `
  -SkuName Standard_RAGRS `
  -Kind StorageV2 
```

다른 복제 옵션을 사용 하 여 범용 v2 저장소 계정을 만들려면 아래 테이블에서 해당 하는 값을 원하는 값으로 대체 합니다 .

|복제 옵션  |SkuName 매개 변수  |
|---------|---------|
|LRS(로컬 중복 스토리지)     |Standard-LRS         |
|ZRS(영역 중복 스토리지)     |Standard_ZRS         |
|GRS(지역 중복 스토리지)     |Standard_GRS         |
|읽기 액세스 GRS(지역 중복 스토리지)     |Standard_RAGRS         |
|GZRS (지역 중복 저장소) (미리 보기)    |Standard_GZRS         |
|읽기 액세스 지리적 영역 중복 저장소 (RA-GZRS) (미리 보기)    |Standard_RAGZRS         |

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

먼저 [az group create](/cli/azure/group#az_group_create) 명령을 사용하여 Azure CLI를 통해 새 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create \
    --name storage-resource-group \
    --location westus
```

`--location` 매개 변수에 어떤 지역을 지정할지 확실하지 않으면 [az account list-locations](/cli/azure/account#az_account_list) 명령을 사용하여 해당 구독에 대해 지원되는 지역을 검색할 수 있습니다.

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

다음으로 [az storage account create](/cli/azure/storage/account#az_storage_account_create) 명령을 사용 하 여 읽기 액세스 지역 중복 저장소를 사용 하는 범용 v2 저장소 계정을 만듭니다. 저장소 계정의 이름은 Azure에서 고유 해야 하므로 괄호 안의 자리 표시자 값을 고유한 값으로 바꿉니다.

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group storage-resource-group \
    --location westus \
    --sku Standard_RAGRS \
    --kind StorageV2
```

다른 복제 옵션을 사용 하 여 범용 v2 저장소 계정을 만들려면 아래 테이블에서 **sku** 매개 변수를 원하는 값으로 대체 합니다.

|복제 옵션  |sku 매개 변수  |
|---------|---------|
|LRS(로컬 중복 스토리지)     |Standard-LRS         |
|ZRS(영역 중복 스토리지)     |Standard_ZRS         |
|GRS(지역 중복 스토리지)     |Standard_GRS         |
|읽기 액세스 GRS(지역 중복 스토리지)     |Standard_RAGRS         |
|GZRS (지역 중복 저장소) (미리 보기)    |Standard_GZRS         |
|읽기 액세스 지리적 영역 중복 저장소 (RA-GZRS) (미리 보기)    |Standard_RAGZRS         |

# <a name="templatetabtemplate"></a>[템플릿](#tab/template)

Azure PowerShell 또는 Azure CLI를 사용하여 스토리지 계정을 만드는 Resource Manager 템플릿을 배포할 수 있습니다. 이 방법 문서에 사용 된 템플릿은 [Azure Resource Manager 빠른 시작 템플릿에서](https://azure.microsoft.com/resources/templates/101-storage-account-create/)사용 됩니다. 스크립트를 실행하려면 **사용해 보세요.** 를 선택하여 Azure Cloud Shell을 엽니다. 스크립트를 붙여넣으려면 셸을 마우스 오른쪽 단추로 클릭하고 **붙여넣기**를 선택합니다.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group create --name $resourceGroupName --location "$location" &&
az group deployment create --resource-group $resourceGroupName --template-file "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

템플릿을 만드는 방법을 알아보려면 다음을 참조하세요.

- [Azure Resource Manager 설명서](/azure/azure-resource-manager/)
- [스토리지 계정 템플릿 참조](/azure/templates/microsoft.storage/allversions).
- [추가 스토리지 계정 템플릿 샘플](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Storage)

---

사용 가능한 복제 옵션에 대한 자세한 내용은 [스토리지 복제 옵션](storage-redundancy.md)을 참조하세요.

## <a name="clean-up-resources"></a>리소스 정리

이 방법 문서에서 만든 리소스를 정리 하려는 경우 리소스 그룹을 삭제할 수 있습니다. 리소스 그룹을 삭제하면 연결된 스토리지 계정과 기타 해당 리소스 그룹에 연결된 다른 모든 리소스가 함께 삭제됩니다.

# <a name="portaltabazure-portal"></a>[포털](#tab/azure-portal)

Azure Portal을 사용하여 리소스 그룹을 제거하려면

1. Azure Portal에서 왼쪽의 메뉴를 확장하여 서비스 메뉴를 열고 **리소스 그룹**을 선택하여 리소스 그룹 목록을 표시합니다.
2. 삭제할 리소스 그룹을 찾아 목록 오른쪽에 있는 **자세히** 단추( **...** )를 마우스 오른쪽 단추로 클릭합니다.
3. **리소스 그룹 삭제**를 선택하고 확인합니다.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

새 스토리지 계정을 포함하여 리소스 그룹 및 관련 리소스를 제거하려면 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) 명령을 사용합니다.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

새 스토리지 계정을 포함하여 리소스 그룹과 관련 리소스를 제거하려면 [az group delete](/cli/azure/group#az_group_delete) 명령을 사용합니다.

```azurecli-interactive
az group delete --name storage-resource-group
```

# <a name="templatetabtemplate"></a>[템플릿](#tab/template)

새 스토리지 계정을 포함하여 리소스 그룹 및 관련 리소스를 제거하려면 Azure PowerShell 또는 Azure CLI를 사용합니다.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>다음 단계

이 방법 문서에서는 범용 v2 standard storage 계정을 만들었습니다. 저장소 계정에서 blob을 업로드 하 고 다운로드 하는 방법을 알아보려면 Blob storage 빠른 시작 중 하나를 계속 진행 하세요.

# <a name="portaltabazure-portal"></a>[포털](#tab/azure-portal)

> [!div class="nextstepaction"]
> [Azure Portal을 통한 Blob 사용](../blobs/storage-quickstart-blobs-portal.md)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

> [!div class="nextstepaction"]
> [PowerShell을 통한 Blob 사용](../blobs/storage-quickstart-blobs-powershell.md)

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!div class="nextstepaction"]
> [Azure CLI를 통한 Blob 사용](../blobs/storage-quickstart-blobs-cli.md)

# <a name="templatetabtemplate"></a>[템플릿](#tab/template)

> [!div class="nextstepaction"]
> [Azure Portal을 통한 Blob 사용](../blobs/storage-quickstart-blobs-portal.md)

---
