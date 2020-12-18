---
title: 템플릿 배포 가상
description: Azure Resource Manager 템플릿을 배포 하기 전에 리소스에 대해 수행 되는 변경 내용을 확인 합니다.
author: tfitzmac
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: tomfitz
ms.openlocfilehash: fa70d88b046cf38aa74582066d230c15580465b9
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97673973"
---
# <a name="arm-template-deployment-what-if-operation"></a>ARM 템플릿 배포 가상 작업

Azure Resource Manager 템플릿 (ARM 템플릿)을 배포 하기 전에 발생 하는 변경 내용을 미리 볼 수 있습니다. Azure Resource Manager은 템플릿을 배포할 때 리소스가 어떻게 변경 되는지 확인할 수 있는 가상 작업을 제공 합니다. 가상 작업은 기존 리소스를 변경하지 않습니다. 대신, 지정된 템플릿이 배포되는 경우 변경 내용을 예측합니다.

Azure PowerShell, Azure CLI 또는 REST API 작업을 사용 하 여 가상 작업을 사용할 수 있습니다. 가상은 리소스 그룹, 구독, 관리 그룹 및 테 넌 트 수준 배포에 대해 지원 됩니다.

## <a name="install-azure-powershell-module"></a>Azure PowerShell 모듈 설치

PowerShell에서이를 사용 하려면 **Az module 버전 4.2 이상이** 있어야 합니다.

그러나 필수 모듈을 설치 하기 전에 PowerShell Core (6.x 또는 4.x)가 있는지 확인 합니다. PowerShell 5.x 이전 버전인 경우 [powershell의 버전을 업데이트](/powershell/scripting/install/installing-powershell)합니다. 필수 모듈은 PowerShell 5.x 또는 이전 버전에 설치할 수 없습니다.

### <a name="install-latest-version"></a>최신 버전 설치

모듈을 설치 하려면 다음을 사용 합니다.

```powershell
Install-Module -Name Az -Force
```

모듈을 설치 하는 방법에 대 한 자세한 내용은 [Install Azure PowerShell](/powershell/azure/install-az-ps)를 참조 하세요.

## <a name="install-azure-cli-module"></a>Azure CLI 모듈 설치

Azure CLI에서 가상을 사용하려면 Azure CLI 2.5.0 이상이 있어야 합니다. 필요한 경우 [최신 버전의 Azure CLI를 설치](/cli/azure/install-azure-cli)합니다.

## <a name="see-results"></a>결과 보기

PowerShell 또는 Azure CLI에서 what if를 사용 하는 경우 다양 한 유형의 변경 내용을 볼 수 있도록 하는 색으로 구분 된 결과가 출력에 포함 됩니다.

![리소스 관리자 템플릿 배포 가상 작업 fullresourcepayload 및 형식 변경](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

텍스트 출력은 다음과 같습니다.

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

> [!NOTE]
> 가상 작업은 [참조 함수](template-functions-resource.md#reference)를 확인할 수 없습니다. 속성을 참조 함수를 포함 하는 템플릿 식으로 설정할 때마다 속성은 변경 됩니다. 이 동작은가 속성 (예: 또는 부울 값)의 현재 값을 확인 되지 `true` 않은 템플릿 식과 비교 하기 때문에 발생 `false` 합니다. 분명히 이러한 값은 일치 하지 않습니다. 템플릿을 배포할 때 속성은 템플릿 식이 다른 값으로 확인 되는 경우에만 변경 됩니다.

## <a name="what-if-commands"></a>가상 명령

### <a name="azure-powershell"></a>Azure PowerShell

템플릿을 배포 하기 전에 변경 내용을 미리 보려면 [AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) 또는 [AzSubscriptionDeployment](/powershell/module/az.resources/new-azdeployment)를 사용 합니다. `-Whatif`스위치 매개 변수를 deployment 명령에 추가 합니다.

* `New-AzResourceGroupDeployment -Whatif` 리소스 그룹 배포의 경우
* `New-AzSubscriptionDeployment -Whatif``New-AzDeployment -Whatif`구독 수준 배포의 경우

`-Confirm`스위치 매개 변수를 사용 하 여 변경 내용을 미리 보고 배포를 계속 하 라는 메시지를 받을 수 있습니다.

* `New-AzResourceGroupDeployment -Confirm` 리소스 그룹 배포의 경우
* `New-AzSubscriptionDeployment -Confirm``New-AzDeployment -Confirm`구독 수준 배포의 경우

이전 명령은 수동으로 검사할 수 있는 텍스트 요약을 반환 합니다. 프로그래밍 방식으로 변경 내용을 검사할 수 있는 개체를 가져오려면 [AzResourceGroupDeploymentWhatIfResult](/powershell/module/az.resources/get-azresourcegroupdeploymentwhatifresult) 또는 [AzSubscriptionDeploymentWhatIfResult](/powershell/module/az.resources/get-azdeploymentwhatifresult)를 사용 합니다.

* `$results = Get-AzResourceGroupDeploymentWhatIfResult` 리소스 그룹 배포의 경우
* `$results = Get-AzSubscriptionDeploymentWhatIfResult` 또는 `$results = Get-AzDeploymentWhatIfResult` 구독 수준 배포의 경우

### <a name="azure-cli"></a>Azure CLI

템플릿을 배포 하기 전에 변경 내용을 미리 보려면 다음을 사용 합니다.

* [az deployment group what-](/cli/azure/deployment/group#az-deployment-group-what-if) 리소스 그룹 배포의 경우
* [az deployment sub what-](/cli/azure/deployment/sub#az-deployment-sub-what-if) 구독 수준 배포의 경우
* [az deployment mg (](/cli/azure/deployment/mg#az-deployment-mg-what-if) 관리 그룹 배포의 경우)
* [az deployment 테 넌 트-](/cli/azure/deployment/tenant#az-deployment-tenant-what-if) 테 넌 트 배포의 경우

`--confirm-with-what-if`스위치 (또는 약식 `-c` )를 사용 하 여 변경 내용을 미리 보고 배포를 계속 하 라는 메시지를 받을 수 있습니다. 다음에이 스위치를 추가 합니다.

* [az deployment group create](/cli/azure/deployment/group#az-deployment-group-create)
* [az deployment sub create](/cli/azure/deployment/sub#az-deployment-sub-create).
* [az deployment mg create](/cli/azure/deployment/mg#az-deployment-mg-create)
* [az deployment tenant create](/cli/azure/deployment/tenant#az-deployment-tenant-create)

예를 들어 `az deployment group create --confirm-with-what-if` `-c` 리소스 그룹 배포의 경우 또는를 사용 합니다.

이전 명령은 수동으로 검사할 수 있는 텍스트 요약을 반환 합니다. 프로그래밍 방식으로 변경 내용을 검사할 수 있는 JSON 개체를 가져오려면 스위치를 사용 `--no-pretty-print` 합니다. 예를 들어 `az deployment group what-if --no-pretty-print` 리소스 그룹 배포에는를 사용 합니다.

색 없이 결과를 반환 하려는 경우 [Azure CLI 구성](/cli/azure/azure-cli-configuration) 파일을 엽니다. **No_color** 를 **예** 로 설정 합니다.

### <a name="azure-rest-api"></a>Azure REST API

REST API의 경우 다음을 사용 합니다.

* [배포-](/rest/api/resources/deployments/whatif) 리소스 그룹 배포에 대 한 What If
* 배포-구독 배포를 위한 [구독 범위에서 What If](/rest/api/resources/deployments/whatifatsubscriptionscope)
* 배포-관리 그룹 배포에 대 한 [관리 그룹 범위에서 What If](/rest/api/resources/deployments/whatifatmanagementgroupscope)
* 배포-테 넌 트 배포에 대 한 [테 넌 트 범위에서 What If](/rest/api/resources/deployments/whatifattenantscope) 합니다.

## <a name="change-types"></a>변경 형식

가상 작업은 다음과 같은 6 가지 유형의 변경 내용을 나열 합니다.

- **만들기**: 리소스가 현재 존재 하지는 않지만 템플릿에 정의 되어 있습니다. 리소스가 생성됩니다.

- **Delete**:이 변경 형식은 배포에 [전체 모드](deployment-modes.md) 를 사용 하는 경우에만 적용 됩니다. 리소스가 있지만 템플릿에 정의되어 있지 않습니다. ‘전체 모드’를 사용하면 리소스가 삭제됩니다. [완료 모드 삭제를 지 원하는](complete-mode-deletion.md) 리소스만이 변경 형식에 포함 됩니다.

- **Ignore**: 리소스가 있지만 템플릿에 정의 되어 있지 않습니다. 리소스가 배포 또는 수정되지 않습니다.

- **NoChange**: 리소스가 있고 템플릿에 정의 되어 있습니다. 리소스가 다시 배포되지만 리소스의 속성이 변경되지 않습니다. 이 변경 형식은 [Resultformat](#result-format) 이 기본값인로 설정 된 경우 반환 됩니다 `FullResourcePayloads` .

- **Modify**: 리소스가 있으며 템플릿에 정의 되어 있습니다. 리소스가 다시 배포되고 리소스의 속성이 변경됩니다. 이 변경 형식은 [Resultformat](#result-format) 이 기본값인로 설정 된 경우 반환 됩니다 `FullResourcePayloads` .

- **배포**: 리소스가 있으며 템플릿에서 정의 됩니다. 리소스가 다시 배포됩니다. 리소스의 속성은 변경되거나 변경되지 않을 수 있습니다. 해당 작업은 속성이 변경될지 여부를 확인할 충분한 정보가 없는 경우 이 변경 형식을 반환합니다. [Resultformat](#result-format) 이로 설정 된 경우에만이 조건이 표시 됩니다 `ResourceIdOnly` .

## <a name="result-format"></a>결과 형식

예측 된 변경 내용에 대해 반환 되는 세부 정보 수준을 제어 합니다. 다음 두 가지 옵션을 사용할 수 있습니다.

* **FullResourcePayloads** -변경 되는 리소스 목록 및 변경 되는 속성에 대 한 세부 정보를 반환 합니다.
* **ResourceIdOnly** -변경 되는 리소스의 목록을 반환 합니다.

기본값은 **FullResourcePayloads** 입니다.

PowerShell 배포 명령의 경우 `-WhatIfResultFormat` 매개 변수를 사용 합니다. 프로그래밍 개체 명령에서 매개 변수를 사용 `ResultFormat` 합니다.

Azure CLI의 경우 `--result-format` 매개 변수를 사용합니다.
 
다음 결과는 두 가지 출력 형식을 보여 줍니다.

- 전체 리소스 페이로드

  ```powershell
  Resource and property changes are indicated with these symbols:
    - Delete
    + Create
    ~ Modify

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
      - tags.Owner: "Team A"
      ~ properties.addressSpace.addressPrefixes: [
        - 0: "10.0.0.0/16"
        + 0: "10.0.0.0/15"
        ]
      ~ properties.subnets: [
        - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

        ]

  Resource changes: 1 to modify.
  ```

- 리소스 ID만

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>What-if 작업 실행

### <a name="set-up-environment"></a>환경 설정

가 작동 하는 방식을 확인 하기 위해 일부 테스트를 실행 해 보겠습니다. 먼저 [가상 네트워크를 만드는 템플릿을](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json)배포 합니다. 이 가상 네트워크를 사용 하 여 변경 내용이 어떻게 보고 되는지 테스트할 수 있습니다.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group create \
  --name ExampleGroup \
  --location "Central US"
az deployment group create \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

---

### <a name="test-modification"></a>테스트 수정

배포가 완료 되 면 가상 작업을 테스트할 준비가 된 것입니다. 이번에는 [가상 네트워크를 변경 하는 템플릿을](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json)배포할 수 있습니다. 원본 태그 중 하나가 누락 되 고, 서브넷이 제거 되 고, 주소 접두사가 변경 되었습니다.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group what-if \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

---

‘가상’ 출력은 다음과 같습니다.

![리소스 관리자 템플릿 배포 가상 작업 출력](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

텍스트 출력은 다음과 같습니다.

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

        name:                     "subnet001"
        properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

출력의 위쪽에서 색이 변경 형식을 나타내는 것으로 정의 됩니다.

출력 아래쪽에는 태그 소유자가 삭제 되었음을 표시 합니다. 주소 접두사가 10.0.0.0/16에서 10.0.0.0/15로 변경 되었습니다. Subnet001 라는 서브넷이 삭제 되었습니다. 이러한 변경 내용은 배포 되지 않습니다. 템플릿을 배포 하는 경우 발생 하는 변경 내용에 대 한 미리 보기가 표시 됩니다.

삭제 된 것으로 표시 되는 일부 속성은 실제로 변경 되지 않습니다. 속성은 템플릿에 없을 때 삭제 된 것으로 잘못 보고 될 수 있지만 배포 중에 기본값으로 자동 설정 됩니다. 이 결과는 가상 응답에서 "노이즈"로 간주 됩니다. 최종 배포 된 리소스는 속성에 대해 설정 된 값을 갖습니다. 가상 작업이 완성 됨에 따라 이러한 속성은 결과에서 필터링 됩니다.

## <a name="programmatically-evaluate-what-if-results"></a>What-if 결과를 프로그래밍 방식으로 평가

이제 명령을 변수에 설정 하 여 가상의 결과를 프로그래밍 방식으로 평가 해 보겠습니다.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIfResult `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

각 변경 내용에 대 한 요약을 볼 수 있습니다.

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
results=$(az deployment group what-if --resource-group ExampleGroup --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json" --no-pretty-print)
```

---

## <a name="confirm-deletion"></a>삭제 확인

가상 작업은 [배포 모드](deployment-modes.md)사용을 지원 합니다. 완료 모드로 설정 된 경우 템플릿에 없는 리소스가 삭제 됩니다. 다음 예제에서는 전체 모드에서 [정의 된 리소스가 없는 템플릿을](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json) 배포 합니다.

템플릿을 배포하기 전에 변경 내용을 미리 보려면 배포 명령에서 confirm switch 매개 변수를 사용합니다. 예상대로 변경되면 배포를 완료하도록 확인합니다.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -Mode Complete `
  -Confirm `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --resource-group ExampleGroup \
  --mode Complete \
  --confirm-with-what-if \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json"
```

---

템플릿에 정의 된 리소스가 없고 배포 모드가 완료로 설정 되어 있기 때문에 가상 네트워크가 삭제 됩니다.

![리소스 관리자 템플릿 배포 가상 작업 출력 배포 모드 완료](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

텍스트 출력은 다음과 같습니다.

```powershell
Resource and property changes are indicated with this symbol:
  - Delete

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  - Microsoft.Network/virtualNetworks/vnet-001

      id:
"/subscriptions/./resourceGroups/ExampleGroup/providers/Microsoft.Network/virtualNet
works/vnet-001"
      location:        "centralus"
      name:            "vnet-001"
      tags.CostCenter: "12345"
      tags.Owner:      "Team A"
      type:            "Microsoft.Network/virtualNetworks"

Resource changes: 1 to delete.

Are you sure you want to execute the deployment?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

예상 되는 변경 내용을 확인 하 고 배포를 실행 하려는 것을 확인할 수 있습니다.

## <a name="sdks"></a>SDK

Azure Sdk를 통해 가상 작업을 사용할 수 있습니다.

* Python의 [경우 what-if](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.deploymentsoperations#what-if-resource-group-name--deployment-name--properties--location-none--custom-headers-none--raw-false--polling-true----operation-config-)를 사용 합니다.

* Java의 경우 [Deploymentwhatif 클래스](/java/api/com.microsoft.azure.management.resources.deploymentwhatif)를 사용 합니다.

* .NET의 경우 [Deploymentwhatif 클래스](/dotnet/api/microsoft.azure.management.resourcemanager.models.deploymentwhatif)를 사용 합니다.

## <a name="next-steps"></a>다음 단계

- 가상 작업에서 잘못 된 결과가 표시 되는 경우에서 문제를 보고 하세요 [https://aka.ms/whatifissues](https://aka.ms/whatifissues) .
- 무엇을 사용 하는지 설명 하는 Microsoft Learn 모듈의 경우 [변경 내용 미리 보기 및 가상 및 ARM 템플릿 테스트 도구 키트를 사용 하 여 Azure 리소스 유효성 검사](/learn/modules/arm-template-test/)를 참조 하세요.
- Azure PowerShell를 사용 하 여 템플릿을 배포 하려면 [ARM 템플릿과 Azure PowerShell를 사용 하 여 리소스 배포](deploy-powershell.md)를 참조 하세요.
- Azure CLI를 사용 하 여 템플릿을 배포 하려면 [ARM 템플릿과 Azure CLI를 사용 하 여 리소스 배포](deploy-cli.md)를 참조 하세요.
- REST를 사용 하 여 템플릿을 배포 하려면 [ARM 템플릿을 사용 하 여 리소스 배포 및 리소스 관리자 REST API](deploy-rest.md)를 참조 하세요.
