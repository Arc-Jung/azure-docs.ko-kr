---
title: REST API 및 템플릿으로 리소스 배포
description: Azure Resource Manager와 REST API를 사용하여 Azure에 리소스를 배포합니다. 리소스는 Resource Manager 템플릿에 정의됩니다.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: e688d7abfaca442c3de395d25961b4e81e6c7b24
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104889198"
---
# <a name="deploy-resources-with-arm-templates-and-azure-resource-manager-rest-api"></a>ARM 템플릿 및 Azure Resource Manager REST API를 사용 하 여 리소스 배포

이 문서에서는 Azure Resource Manager 템플릿 (ARM 템플릿)과 함께 Azure Resource Manager REST API를 사용 하 여 Azure에 리소스를 배포 하는 방법을 설명 합니다.

요청 본문 또는 파일 링크에는 템플릿을 포함할 수 있습니다. 템플릿을 사용할 경우 템플릿은 로컬 파일이거나 URI를 통해 사용 가능한 외부 파일일 수 있습니다. 템플릿이 스토리지 계정에 있는 경우, 템플릿에 대한 액세스를 제한하고 배포 중에 SAS(공유 액세스 서명) 토큰을 제공할 수 있습니다.

## <a name="deployment-scope"></a>배포 범위

리소스 그룹, Azure 구독, 관리 그룹 또는 테 넌 트에 대 한 배포를 대상으로 지정할 수 있습니다. 배포의 범위에 따라 다른 명령을 사용합니다.

- **리소스 그룹** 에 배포하려면 [배포 - 만들기](/rest/api/resources/deployments/createorupdate)를 사용합니다. 요청이 다음으로 전송됩니다.

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

- **구독** 에 배포하려면 [배포 - 구독 범위에서 만들기](/rest/api/resources/deployments/createorupdateatsubscriptionscope)를 사용합니다. 요청이 다음으로 전송됩니다.

  ```HTTP
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  구독 수준 배포에 대한 자세한 내용은 [구독 수준에서 리소스 그룹 및 리소스 만들기](deploy-to-subscription.md)를 참조하세요.

- **관리 그룹** 에 배포하려면 [배포 - 관리 그룹 범위에서 만들기](/rest/api/resources/deployments/createorupdateatmanagementgroupscope)를 사용합니다. 요청이 다음으로 전송됩니다.

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  관리 그룹 수준 배포에 대한 자세한 내용은 [관리 그룹 수준에서 리소스 만들기](deploy-to-management-group.md)를 참조하세요.

- **테넌트** 에 배포하려면 [배포 - 테넌트 범위에서 만들기 또는 업데이트](/rest/api/resources/deployments/createorupdateattenantscope)를 사용합니다. 요청이 다음으로 전송됩니다.

  ```HTTP
  PUT https://management.azure.com/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
  ```

  테넌트 수준 배포에 대한 자세한 내용은 [테넌트 수준에서 리소스 만들기](deploy-to-tenant.md)를 참조하세요.

이 문서의 예제에서는 리소스 그룹 배포를 사용합니다.

## <a name="deploy-with-the-rest-api"></a>REST API를 사용하여 배포

1. 인증 토큰을 포함하여 [공통 매개 변수 및 헤더](/rest/api/azure/)를 설정합니다.

1. 존재 하지 않는 리소스 그룹에 배포 하는 경우 리소스 그룹을 만듭니다. 솔루션에 필요한 구독 ID, 새 리소스 그룹 이름 및 위치를 제공합니다. 자세한 내용은 [리소스 그룹 만들기](/rest/api/resources/resourcegroups/createorupdate)를 참조하세요.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2020-06-01
   ```

   다음과 같은 요청 본문을 사용합니다.

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. 템플릿을 배포 하기 전에 템플릿이 사용자 환경에 적용 되는 변경 내용을 미리 볼 수 있습니다. [가상 작업](template-deploy-what-if.md) 을 사용 하 여 템플릿이 필요한 변경을 수행 하는지 확인 합니다. 이 경우에서 템플릿의 오류도 확인 합니다.

1. 템플릿을 배포하려면 요청 URI에 구독 ID, 리소스 그룹의 이름, 배포 이름을 제공합니다.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2020-10-01
   ```

   요청 본문에서 템플릿 및 매개 변수 파일에 대한 링크를 제공합니다. 매개 변수 파일에 대한 자세한 내용은 [Resource Manager 매개 변수 파일 만들기](parameter-files.md)를 참조하세요.

   `mode`가 **증분** 로 설정 되었는지 확인 합니다. 전체 배포를 실행 하려면를 `mode` **완료** 로 설정 합니다. 이 완전한 모드를 사용할 때는 템플릿에 없는 리소스를 실수로 삭제할 수 있으므로 주의해야 합니다.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    응답 콘텐츠, 요청 콘텐츠 또는 둘 다를 기록 하려면 요청에를 포함 `debugSetting` 합니다.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    공유 액세스 서명(SAS) 토큰을 사용하여 스토리지 계정을 설정할 수 있습니다. 자세한 내용은 [공유 액세스 서명을 사용 하 여 액세스 위임](/rest/api/storageservices/delegate-access-with-shared-access-signature)을 참조 하세요.

    매개 변수에 대해 중요한 값(예: 암호)을 제공해야 할 경우 해당 값을 주요 자격 증명 모음에 추가합니다. 앞의 예제에 표시된 대로 배포하는 동안 주요 자격 증명 모음을 검색합니다. 자세한 내용은 [Azure Key Vault를 사용하여 배포 중에 보안 매개 변수 값 전달](key-vault-parameter.md)을 참조하세요.

1. 템플릿 및 매개 변수의 파일에 연결하는 대신, 요청 본문에 포함할 수 있습니다. 다음 예에서는 인라인 템플릿 및 매개 변수가 지정된 요청 본문을 보여 줍니다.

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. 템플릿 배포의 상태를 가져오려면 [배포 - 가져오기](/rest/api/resources/deployments/get)를 사용합니다.

   ```HTTP
   GET https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2020-10-01
   ```

## <a name="deployment-name"></a>배포 이름

배포의 이름 (예:)을 지정할 수 있습니다 `ExampleDeployment` .

배포를 실행할 때마다 배포 이름으로 리소스 그룹의 배포 기록에 항목이 추가 됩니다. 다른 배포를 실행 하 고 동일한 이름을 지정 하는 경우 이전 항목이 현재 배포로 바뀝니다. 배포 기록에서 고유한 항목을 유지 하려면 각 배포에 고유한 이름을 지정 합니다.

고유 이름을 만들려면 난수를 할당 하면 됩니다. 또는 날짜 값을 추가 합니다.

동일한 배포 이름을 가진 동일한 리소스 그룹에 동시 배포를 실행 하는 경우 마지막 배포만 완료 됩니다. 완료 되지 않은 동일한 이름의 배포는 마지막 배포로 대체 됩니다. 예를 들어 라는 저장소 계정을 배포 하는 라는 배포를 실행 하는 경우 동일한에 이라는 `newStorage` `storage1` 저장소 계정을 배포 하는 이라는 다른 배포를 실행 하는 경우 `newStorage` `storage2` 저장소 계정을 하나만 배포 합니다. 결과 저장소 계정의 이름은 `storage2` 입니다.

그러나 이라는 이름의 저장소 계정을 배포 하는 라는 배포를 실행 하는 경우,가 완료 된 직후에 라는 `newStorage` `storage1` 저장소 계정을 배포 하는 이라는 다른 배포를 실행 하면 `newStorage` `storage2` 두 개의 저장소 계정이 있습니다. 하나는 이름이이 `storage1` 고 다른 하나는 이름이로 지정 됩니다 `storage2` . 그러나 배포 기록에는 하나의 항목만 있습니다.

각 배포에 고유한 이름을 지정 하는 경우 충돌 없이 동시에 실행할 수 있습니다. 이라는 저장소 계정을 배포 하는 라는 배포를 실행 하는 경우 동시에 라는 `newStorage1` `storage1` 저장소 계정을 배포 하는 이라는 다른 배포를 실행 하는 경우 `newStorage2` `storage2` 두 개의 저장소 계정 및 두 개의 항목이 배포 기록에 있습니다.

동시 배포와의 충돌을 방지 하 고 배포 기록에서 고유한 항목을 확인 하려면 각 배포에 고유한 이름을 지정 합니다.

## <a name="next-steps"></a>다음 단계

- 오류 발생 시 성공적인 배포로 롤백하려면 [오류 발생 시 성공적인 배포로 롤백](rollback-on-error.md)을 참조하세요.
- 리소스 그룹에 있지만 템플릿에 정의되지 않은 리소스를 처리하는 방법을 지정하려면 [Azure Resource Manager 배포 모드](deployment-modes.md)를 참조하세요.
- 비동기 REST 작업 처리에 대해 알아보려면 [Azure 비동기 작업 추적](../management/async-operations.md)을 참조하세요.
- 템플릿에 대한 자세한 내용은 [ARM 템플릿의 구조 및 구문 이해](template-syntax.md)를 참조하세요.
