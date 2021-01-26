---
title: 변경을 방지하기 위해 리소스 잠그기
description: 사용자가 모든 사용자 및 역할에 대 한 잠금을 적용 하 여 Azure 리소스를 업데이트 하거나 삭제 하지 못하도록 합니다.
ms.topic: conceptual
ms.date: 11/11/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0e8fc74b2da0c253ec9c5bf34ec7543398aea48f
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98802443"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>예기치 않은 변경을 방지하기 위해 리소스 잠그기

관리자는 구독, 리소스 그룹 또는 리소스에 잠금을 설정하여 조직의 다른 사용자가 실수로 중요한 리소스를 삭제 또는 수정하지 못하게 할 수 있습니다. 잠금 수준을 **CanNotDelete** 또는 **ReadOnly** 로 설정할 수 있습니다. 포털에서 잠금은 각각 **삭제** 및 **읽기 전용** 으로 지칭됩니다.

* **CanNotDelete** 는 권한이 부여된 사용자가 리소스를 읽고 수정할 수 있지만 삭제할 수 없음을 의미합니다.
* **ReadOnly** 는 권한이 부여된 사용자가 리소스를 읽을 수 있지만 해당 리소스를 삭제하거나 업데이트할 수 없음을 의미합니다. 이 잠금을 적용하는 것은 권한 있는 모든 사용자에게 **판독기** 역할에서 부여한 권한을 제한하는 것과 유사합니다.

## <a name="how-locks-are-applied"></a>잠금을 적용하는 방법

부모 범위에서 잠금을 적용하면 해당 범위 내 모든 리소스가 동일한 잠금을 상속합니다. 나중에 추가하는 리소스도 부모의 잠금을 상속합니다. 상속의 가장 제한적인 잠금이 우선 적용됩니다.

역할 기반 액세스 제어와 달리 관리 잠금을 사용하여 모든 사용자와 역할에 걸쳐 제한을 적용합니다. 사용자 및 역할에 대 한 사용 권한을 설정 하는 방법에 대 한 자세한 내용은 azure [역할 기반 액세스 제어 (AZURE RBAC)](../../role-based-access-control/role-assignments-portal.md)를 참조 하세요.

리소스 관리자 잠금은 `https://management.azure.com`에 전송된 작업으로 구성되는 관리 수준에서 발생된 작업에만 적용됩니다. 잠금은 리소스 자체 기능을 수행하는 방법을 제한하지 않습니다. 리소스 변경은 제한되지만 리소스 작업은 제한되지 않습니다. 예를 들어 SQL Database 논리 서버에 대 한 ReadOnly 잠금은 서버를 삭제 하거나 수정 하는 것을 방지 합니다. 해당 서버의 데이터베이스에서 데이터를 생성, 업데이트 또는 삭제 하는 것을 방지 하지 않습니다. 이러한 작업이 `https://management.azure.com`로 전송되지 않기 때문에 데이터 트랜잭션이 허용됩니다.

## <a name="considerations-before-applying-locks"></a>잠금 적용 전 고려 사항

리소스를 수정하지 않는 일부 작업에 실제로 잠금에 의해 차단되는 작업이 필요하므로 잠금을 적용하면 예기치 않은 결과가 발생할 수 있습니다. 잠금은 Azure Resource Manager API에 대 한 POST 요청이 필요한 모든 작업을 방지 합니다. 잠금에 의해 차단되는 작업의 몇 가지 일반적인 예제는 다음과 같습니다.

* **스토리지 계정** 에 읽기 전용 잠금을 설정하면 모든 사용자가 키를 나열하지 않도록 방지합니다. 반환되는 키를 쓰기 작업에 사용할 수 있기 때문에 목록 키 작업은 POST 요청을 통해 처리됩니다.

* **App Service** 리소스에 읽기 전용 잠금을 설정하면 해당 상호 작용이 쓰기 액세스를 필요로 하기 때문에 Visual Studio 서버 탐색기가 리소스에 대한 파일을 표시하지 않도록 방지합니다.

* **가상 머신** 을 포함하는 **리소스 그룹** 에 대한 읽기 전용 잠금은 모든 사용자가 가상 머신을 시작하거나 다시 시작하지 못하게 합니다. 이러한 작업에는 POST 요청이 필요합니다.

* **리소스 그룹** 에 대 한 삭제 잠금을 설정 하면 Azure Resource Manager가 기록에서 [배포를 자동으로 삭제할](../templates/deployment-history-deletions.md) 수 없습니다. 기록에서 800 배포에 도달 하면 배포에 실패 합니다.

* **Azure Backup 서비스** 가 생성한 **리소스 그룹** 에 대해 잠금을 삭제할 수 없으므로 백업이 실패합니다. 이 서비스는 최대 18개의 복원 지점을 지원합니다. 잠겨 있으면 백업 서비스에서 복원 지점이 정리할 수 없습니다. 자세한 내용은 [질문과 대답-Azure VM 백업](../../backup/backup-azure-vm-backup-faq.yml)을 참조하세요.

* **구독** 에 대해 읽기 전용 잠금을 설정하면 **Azure Advisor** 가 제대로 작동하지 않습니다. Advisor가 쿼리 결과를 저장할 수 없습니다.

## <a name="who-can-create-or-delete-locks"></a>잠금을 만들거나 삭제할 수 있는 사람

관리 잠금을 만들거나 삭제하려면 `Microsoft.Authorization/*` 또는 `Microsoft.Authorization/locks/*` 작업에 대한 액세스 권한이 있어야 합니다. 기본 제공 역할의 경우 **소유자** 및 **사용자 액세스 관리자** 에게만 이러한 작업의 권한이 부여됩니다.

## <a name="managed-applications-and-locks"></a>관리형 애플리케이션 및 잠금

Azure Databricks와 같은 일부 Azure 서비스는 [관리형 애플리케이션](../managed-applications/overview.md)을 사용하여 서비스를 구현합니다. 이 경우 서비스는 두 개의 리소스 그룹을 만듭니다. 한 리소스 그룹에는 서비스에 대한 개요가 포함되어 있으며 잠겨 있지 않습니다. 다른 리소스 그룹에는 서비스에 대 한 인프라가 포함되어 있으며 잠겨집니다.

인프라 리소스 그룹을 삭제하려고 하면 리소스 그룹이 잠겨 있음을 나타내는 오류가 표시됩니다. 인프라 리소스 그룹에 대한 잠금을 삭제하려고 하면 시스템 애플리케이션이 소유하고 있으므로 잠금을 삭제할 수 없다는 오류 메시지가 표시됩니다.

대신, 서비스를 삭제합니다. 그러면 인프라 리소스 그룹도 삭제됩니다.

관리형 애플리케이션의 경우 배포한 서비스를 선택합니다.

![서비스 선택](./media/lock-resources/select-service.png)

서비스에는 **관리형 리소스 그룹** 에 대한 링크가 포함되어 있습니다. 해당 리소스 그룹은 인프라를 보유하고 있으며 잠겨집니다. 직접 삭제할 수 없습니다.

![관리형 그룹 표시](./media/lock-resources/show-managed-group.png)

잠긴 인프라 리소스 그룹을 포함하여 서비스에 대한 모든 항목을 삭제하려면 서비스에 대해 **삭제** 를 선택합니다.

![서비스 삭제](./media/lock-resources/delete-service.png)

## <a name="configure-locks"></a>잠금 구성

### <a name="portal"></a>포털

[!INCLUDE [resource-manager-lock-resources](../../../includes/resource-manager-lock-resources.md)]

### <a name="arm-template"></a>ARM 템플릿

Azure Resource Manager 템플릿 (ARM 템플릿)을 사용 하 여 잠금을 배포 하는 경우 잠금의 범위와 배포 범위를 알고 있어야 합니다. 리소스 그룹 또는 구독 잠금과 같은 배포 범위에서 잠금을 적용 하려면 범위 속성을 설정 하지 마세요. 배포 범위 내에서 리소스를 잠그면 범위 속성을 설정 합니다.

다음 템플릿은 배포 되는 리소스 그룹에 잠금을 적용 합니다. 잠금 범위가 배포 범위와 일치 하므로 잠금 리소스에 범위 속성이 없습니다. 이 템플릿은 리소스 그룹 수준에서 배포 됩니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {  
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/locks",
            "apiVersion": "2016-09-01",
            "name": "rgLock",
            "properties": {
                "level": "CanNotDelete",
                "notes": "Resource Group should not be deleted."
            }
        }
    ]
}
```

리소스 그룹을 만들어 잠그려면 해당 구독 수준에서 다음 템플릿을 배포 합니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2019-10-01",
            "name": "[parameters('rgName')]",
            "location": "[parameters('rgLocation')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "lockDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/locks",
                            "apiVersion": "2016-09-01",
                            "name": "rgLock",
                            "properties": {
                                "level": "CanNotDelete",
                                "notes": "Resource group and its resources should not be deleted."
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

리소스 그룹 내의 **리소스** 에 잠금을 적용 하는 경우 scope 속성을 추가 합니다. 범위를 잠글 리소스의 이름으로 설정 합니다.

다음 예제에서는 웹 사이트에 App Service 계획, 웹 사이트 및 잠금을 만드는 템플릿을 보여 줍니다. 잠금의 범위는 웹 사이트로 설정 됩니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "hostingPlanName": {
      "type": "string"
    },
    "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2020-06-01",
      "name": "[parameters('hostingPlanName')]",
      "location": "[parameters('location')]",
      "sku": {
        "tier": "Free",
        "name": "f1",
        "capacity": 0
      },
      "properties": {
        "targetWorkerCount": 1
      }
    },
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2020-06-01",
      "name": "[variables('siteName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      }
    },
    {
      "type": "Microsoft.Authorization/locks",
      "apiVersion": "2016-09-01",
      "name": "siteLock",
      "scope": "[concat('Microsoft.Web/sites/', variables('siteName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
      ],
      "properties": {
        "level": "CanNotDelete",
        "notes": "Site should not be deleted."
      }
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell

[New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) 명령을 사용하여 Azure PowerShell을 통해 배포된 리소스를 잠급니다.

리소스를 잠그려면 리소스 이름, 리소스 종류 및 해당 리소스 그룹 이름을 제공합니다.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

리소스 그룹을 잠그려면 해당 리소스 그룹의 이름을 제공합니다.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

잠금에 대한 정보를 가져오려면 [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock)을 사용합니다. 구독의 모든 잠금을 가져오려면 다음을 사용합니다.

```azurepowershell-interactive
Get-AzResourceLock
```

리소스에 대한 모든 잠금을 가져오려면 다음을 사용합니다.

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

리소스 그룹에 대한 모든 잠금을 가져오려면 다음을 사용합니다.

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

리소스에 대 한 잠금을 삭제 하려면 다음을 사용 합니다.

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

리소스 그룹에 대 한 잠금을 삭제 하려면 다음을 사용 합니다.

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup).LockId
Remove-AzResourceLock -LockId $lockId
```

### <a name="azure-cli"></a>Azure CLI

[az lock create](/cli/azure/lock#az-lock-create) 명령을 사용하여 Azure CLI를 통해 배포된 리소스를 잠급니다.

리소스를 잠그려면 리소스 이름, 리소스 종류 및 해당 리소스 그룹 이름을 제공합니다.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

리소스 그룹을 잠그려면 해당 리소스 그룹의 이름을 제공합니다.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

잠금에 대한 정보를 가져오려면 [az lock list](/cli/azure/lock#az-lock-list)를 사용합니다. 구독의 모든 잠금을 가져오려면 다음을 사용합니다.

```azurecli
az lock list
```

리소스에 대한 모든 잠금을 가져오려면 다음을 사용합니다.

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

리소스 그룹에 대한 모든 잠금을 가져오려면 다음을 사용합니다.

```azurecli
az lock list --resource-group exampleresourcegroup
```

리소스에 대 한 잠금을 삭제 하려면 다음을 사용 합니다.

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

리소스 그룹에 대 한 잠금을 삭제 하려면 다음을 사용 합니다.

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup  --output tsv --query id)
az lock delete --ids $lockid
```

### <a name="rest-api"></a>REST API

[관리 잠금을 위한 REST API](/rest/api/resources/managementlocks)를 사용하여 배포된 리소스를 잠글 수 있습니다. REST API를 사용하여 잠금을 만들고, 삭제하고, 기존 잠금에 대한 정보를 검색할 수 있습니다.

잠금을 만들려면 다음을 실행합니다.

```http
PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}
```

범위는 구독, 리소스 그룹 또는 리소스일 수 있습니다. lock-name은 원하는 잠금 이름입니다. api-version에는 **2016-09-01** 을 사용합니다.

요청에서 잠금에 대한 속성을 지정하는 JSON 개체를 포함합니다.

```json
{
  "properties": {
  "level": "CanNotDelete",
  "notes": "Optional text notes."
  }
}
```

## <a name="next-steps"></a>다음 단계

* 리소스를 논리적으로 구성하는 방법에 대한 자세한 내용은 [태그를 사용하여 리소스 구성](tag-resources.md)
* 사용자 지정된 정책을 사용하여 구독을 통해 제한 사항 및 규칙을 적용할 수 있습니다. 자세한 내용은 [Azure Policy란?](../../governance/policy/overview.md)을 참조하세요.
* 엔터프라이즈에서 리소스 관리자를 사용하여 구독을 효과적으로 관리할 수 있는 방법에 대한 지침은 [Azure 엔터프라이즈 스캐폴드 - 규범적 구독 거버넌스](/azure/architecture/cloud-adoption-guide/subscription-governance)를 참조하세요.
