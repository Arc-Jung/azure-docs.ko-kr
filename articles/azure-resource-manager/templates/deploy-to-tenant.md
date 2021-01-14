---
title: 테넌트에 리소스 배포
description: Azure Resource Manager 템플릿의 테넌트 범위에서 리소스를 배포하는 방법을 설명합니다.
ms.topic: conceptual
ms.date: 01/13/2021
ms.openlocfilehash: 0b3ddc63e49b272c93349ada91e9a1599ea4be4f
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98186212"
---
# <a name="tenant-deployments-with-arm-templates"></a>ARM 템플릿을 사용 하 여 테 넌 트 배포

조직이 완성 되 면 Azure AD 테 넌 트에서 [정책](../../governance/policy/overview.md) 또는 [azure RBAC (역할 기반 액세스 제어)](../../role-based-access-control/overview.md) 를 정의 하 고 할당 해야 할 수 있습니다. 테넌트 수준 템플릿을 사용하면 선언적으로 정책을 적용하고 전역 수준에서 역할을 할당할 수 있습니다.

## <a name="supported-resources"></a>지원되는 리소스

모든 리소스 유형을 테 넌 트 수준에 배포할 수 있는 것은 아닙니다. 이 섹션에서는 지원 되는 리소스 종류를 나열 합니다.

Azure 정책의 경우 다음을 사용 합니다.

* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)

Azure RBAC (역할 기반 액세스 제어)의 경우 다음을 사용 합니다.

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)

관리 그룹, 구독 또는 리소스 그룹에 배포 하는 중첩 된 템플릿의 경우 다음을 사용 합니다.

* [배포](/azure/templates/microsoft.resources/deployments)

관리 그룹을 만들려면 다음을 사용 합니다.

* [managementGroups](/azure/templates/microsoft.management/managementgroups)

구독을 만들려면 다음을 사용 합니다.

* [별칭](/azure/templates/microsoft.subscription/aliases)

비용 관리를 위해 다음을 사용 합니다.

* [billingProfiles](/azure/templates/microsoft.billing/billingaccounts/billingprofiles)
* [따릅니다](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/instructions)
* [invoiceSections](/azure/templates/microsoft.billing/billingaccounts/billingprofiles/invoicesections)

포털을 구성 하려면 다음을 사용 합니다.

* [tenantConfigurations](/azure/templates/microsoft.portal/tenantconfigurations)

## <a name="schema"></a>스키마

테넌트 배포에 사용하는 스키마는 리소스 그룹 배포에 대한 스키마와 다릅니다.

템플릿의 경우 다음을 사용합니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
    ...
}
```

매개 변수 파일에 대한 스키마는 모든 배포 범위에 대해 동일합니다. 매개 변수 파일의 경우 다음을 사용합니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="required-access"></a>필요한 액세스

템플릿을 배포하는 주체는 테넌트 범위에서 리소스를 만들 수 있는 권한이 있어야 합니다. 주체는 배포 작업(`Microsoft.Resources/deployments/*`)을 실행하고 템플릿에 정의된 리소스를 만들 수 있는 권한이 있어야 합니다. 예를 들어 관리 그룹을 만들려면 주체는 테넌트 범위에서 기여자 권한이 있어야 합니다. 역할 할당을 만들려면 주체에 소유자 권한이 있어야 합니다.

Azure Active Directory의 전역 관리자에게 역할을 할당할 수 있는 권한이 자동으로 생기지는 않습니다. 테넌트 범위에서 템플릿 배포를 사용하도록 설정하려면 전역 관리자가 다음 단계를 수행해야 합니다.

1. 계정 액세스 권한을 승격하여 전역 관리자가 역할을 할당할 수 있도록 합니다. 자세한 내용은 [모든 Azure 구독 및 관리 그룹을 관리할 수 있도록 액세스 권한 상승](../../role-based-access-control/elevate-access-global-admin.md)을 참조하세요.

1. 템플릿을 배포해야 하는 주체에 소유자 또는 기여자를 할당합니다.

   ```azurepowershell-interactive
   New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
   ```

   ```azurecli-interactive
   az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
   ```

이제 주체는 템플릿을 배포하는 데 필요한 권한이 있습니다.

## <a name="deployment-commands"></a>배포 명령

테넌트 배포에 대한 명령은 리소스 그룹 배포에 대한 명령과 다릅니다.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI의 경우 [az deployment tenant create](/cli/azure/deployment/tenant#az-deployment-tenant-create)를 사용합니다.

```azurecli-interactive
az deployment tenant create \
  --name demoTenantDeployment \
  --location WestUS \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Azure PowerShell의 경우 [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment)를 사용합니다.

```azurepowershell-interactive
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "West US" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-deployments/new-mg/azuredeploy.json"
```

---

ARM 템플릿 배포에 대 한 배포 명령 및 옵션에 대 한 자세한 내용은 다음을 참조 하세요.

* [ARM 템플릿을 사용 하 여 리소스 배포 및 Azure Portal](deploy-portal.md)
* [ARM 템플릿 및 Azure CLI를 사용하여 리소스 배포](deploy-cli.md)
* [ARM 템플릿 및 Azure PowerShell을 사용하여 리소스 배포](deploy-powershell.md)
* [ARM 템플릿 및 Azure Resource Manager REST API를 사용 하 여 리소스 배포](deploy-rest.md)
* [배포 단추를 사용 하 여 GitHub 리포지토리에서 템플릿 배포](deploy-to-azure-button.md)
* [Cloud Shell에서 ARM 템플릿 배포](deploy-cloud-shell.md)

## <a name="deployment-location-and-name"></a>배포 위치 및 이름

테넌트 수준 배포의 경우 배포할 위치를 제공해야 합니다. 배포 위치는 배포하는 리소스의 위치와는 별개입니다. 배포 위치는 배포 데이터를 저장할 위치를 지정합니다. [구독](deploy-to-subscription.md) 및 [관리 그룹](deploy-to-management-group.md) 배포에도 위치가 필요 합니다. [리소스 그룹](deploy-to-resource-group.md) 배포의 경우 리소스 그룹의 위치는 배포 데이터를 저장 하는 데 사용 됩니다.

배포 이름을 제공하거나 기본 배포 이름을 사용할 수 있습니다. 기본 이름은 템플릿 파일의 이름입니다. 예를 들어 **azuredeploy.json** 이라는 템플릿을 배포하면 **azuredeploy** 라는 기본 배포 이름을 만듭니다.

각 배포 이름의 경우 위치는 변경할 수 없습니다. 다른 위치의 이름이 동일한 기존 배포가 있는 경우 하나의 위치에서 배포를 만들 수 없습니다. 예를 들어 **centralus** 에서 이름이 **deployment1** 인 테 넌 트 배포를 만드는 경우 나중에 **deployment1** 이름 이지만 **westus** 의 위치로 다른 배포를 만들 수 없습니다. 오류 코드 `InvalidDeploymentLocation`을 수신하게 되면 해당 이름의 이전 배포와 다른 이름이나 동일한 위치를 사용합니다.

## <a name="deployment-scopes"></a>배포 범위

테 넌 트에 배포 하는 경우 다음에 리소스를 배포할 수 있습니다.

* 테 넌 트
* 테 넌 트 내의 관리 그룹
* 구독
* 리소스 그룹

[확장 리소스](scope-extension-resources.md) 의 범위는 배포 대상과 다른 대상으로 지정할 수 있습니다.

템플릿을 배포 하는 사용자에 게는 지정 된 범위에 대 한 액세스 권한이 있어야 합니다.

이 섹션에서는 다양 한 범위를 지정 하는 방법을 보여 줍니다. 단일 템플릿에서 이러한 여러 범위를 결합할 수 있습니다.

### <a name="scope-to-tenant"></a>테 넌 트로 범위

템플릿의 resources 섹션 내에 정의 된 리소스는 테 넌 트에 적용 됩니다.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-tenant.json" highlight="5":::

### <a name="scope-to-management-group"></a>관리 그룹에 대 한 범위

테 넌 트 내에서 관리 그룹을 대상으로 지정 하려면 중첩 된 배포를 추가 하 고 속성을 지정 `scope` 합니다.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-mg.json" highlight="10,17,18,22":::

### <a name="scope-to-subscription"></a>구독 범위

테 넌 트 내에서 구독을 대상으로 지정할 수도 있습니다. 템플릿을 배포 하는 사용자에 게는 지정 된 범위에 대 한 액세스 권한이 있어야 합니다.

테 넌 트 내에서 구독을 대상으로 하려면 중첩 된 배포 및 `subscriptionId` 속성을 사용 합니다.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-subscription.json" highlight="9,10,18":::

### <a name="scope-to-resource-group"></a>리소스 그룹에 대 한 범위

또한 테 넌 트 내의 리소스 그룹을 대상으로 지정할 수 있습니다. 템플릿을 배포 하는 사용자에 게는 지정 된 범위에 대 한 액세스 권한이 있어야 합니다.

테 넌 트 내의 리소스 그룹을 대상으로 하려면 중첩 된 배포를 사용 합니다. `subscriptionId` 및 `resourceGroup` 속성을 설정합니다. 중첩 된 배포는 리소스 그룹의 위치에 배포 되므로 위치를 설정 하지 마세요.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/tenant-to-rg.json" highlight="9,10,18":::

## <a name="create-management-group"></a>관리 그룹 만들기

다음 템플릿은 관리 그룹을 만듭니다.

:::code language="json" source="~/quickstart-templates/tenant-deployments/new-mg/azuredeploy.json":::

계정에 테 넌 트에 배포할 수 있는 권한이 없는 경우에도 다른 범위에 배포 하 여 관리 그룹을 만들 수 있습니다. 자세한 내용은 [관리 그룹](deploy-to-management-group.md#management-group)을 참조 하세요.

## <a name="assign-role"></a>역할 할당

다음 템플릿은 테넌트 범위에서 역할을 할당합니다.

:::code language="json" source="~/quickstart-templates/tenant-deployments/tenant-role-assignment/azuredeploy.json":::

## <a name="next-steps"></a>다음 단계

* 역할 할당에 대해 알아보려면 [Azure Resource Manager 템플릿을 사용 하 여 Azure 역할 할당 추가](../../role-based-access-control/role-assignments-template.md)를 참조 하세요.
* [구독 수준](deploy-to-subscription.md) 또는 [관리 그룹 수준](deploy-to-management-group.md)에서 템플릿을 배포할 수도 있습니다.
