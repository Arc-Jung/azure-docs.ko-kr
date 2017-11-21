---
title: "Azure Policy를 사용하여 조직의 규정 준수를 적용하는 정책 만들기 및 관리 | Microsoft Docs"
description: "Azure Policy를 사용하여 표준을 적용하고, 규정 준수 및 감사 요구 사항을 충족하며, 비용을 통제하고, 보안 및 성능 일관성을 유지하며, 엔터프라이즈 수준 디자인 원칙을 적용합니다."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark
ms.date: 11/01/2017
ms.topic: tutorial
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: adbf6e13efaad196c39e4fce0900fa40d7511122
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2017
---
# <a name="create-and-manage-policies-to-enforce-compliance"></a>규정 준수를 적용하는 정책 만들기 및 관리

회사 표준 및 서비스 수준 계약의 준수를 유지하는 데 있어 Azure에서 정책을 만들고 관리하는 방법을 이해하는 것이 중요합니다. 이 자습서에서는 Azure Policy를 사용하여 조직 전체에서 정책을 생성, 할당 및 관리하는 것과 관련된 보다 일반적인 작업 중 일부를 수행하는 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 나중에 만들 리소스에 대한 조건을 적용하는 정책 할당
> * 여러 리소스에 대한 규정 준수를 추적하기 위한 이니셔티브 정의 만들기 및 할당
> * 규정 비준수 또는 거부된 리소스 해결
> * 조직 전체에서 새 정책 구현

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 을 만듭니다.

## <a name="opt-in-to-azure-policy"></a>Azure Policy에 옵트인

Azure Policy는 제한된 미리 보기에서 사용할 수 있으므로 액세스를 요청하려면 등록해야 합니다.

1. https://aka.ms/getpolicy에 있는 Azure Policy로 이동하고 왼쪽 창에서 **등록**을 선택합니다.

   ![정책 검색](media/assign-policy-definition/sign-up.png)

2. **구독** 목록에서 사용하려는 구독을 선택하여 Azure Policy에 옵트인합니다. 그런 다음 **등록**을 선택합니다.

   구독 목록에는 모든 Azure 구독이 포함되어 있습니다.

   ![Azure Policy를 사용하기 위한 옵트인](media/assign-policy-definition/preview-opt-in.png)

   필요에 따라 등록 요청을 수락하는 데 며칠이 걸릴 수 있습니다. 요청이 수락되면 서비스 사용을 시작할 수 있다는 내용의 전자 메일이 발송됩니다.

## <a name="assign-a-policy"></a>정책 할당

Azure Policy 준수를 적용하기 위한 첫 번째 단계는 정책 정의를 할당하는 것입니다. 정책 정의는 정책이 적용되는 조건과 수행할 작업을 정의합니다. 이 예제에서는 *SQL Server 버전 12.0 필요*라는 기본 제공 정책 정의를 할당하여 모든 SQL Server 데이터베이스에서 V12.0을 준수해야 한다는 조건을 적용합니다.

1. 왼쪽 창에서 **정책**을 검색하고 선택하여 Azure Portal에서 Azure Policy 서비스를 시작합니다.

   ![정책 검색](media/assign-policy-definition/search-policy.png)

2. Azure Policy 페이지의 왼쪽 창에서 **할당**을 선택합니다. 할당은 특정 범위 내에서 수행하도록 할당된 정책입니다.
3. **할당** 창의 위쪽에서 **정책 할당**을 선택합니다.

   ![정책 정의 할당](media/create-manage-policy/select-assign-policy.png)

4. **정책 할당** 페이지에서 **정책** 필드 옆에 있는 ![정책 정의 단추](media/assign-policy-definition/definitions-button.png)를 클릭하여 사용 가능한 정의 목록을 엽니다.

   ![사용 가능한 정책 정의 열기](media/create-manage-policy/open-policy-definitions.png)

5. **SQL Server 버전 12.0 필요**를 선택합니다.

   ![정책 찾기](media/create-manage-policy/select-available-definition.png)

6. 정책 할당에 대한 표시 **이름**을 제공합니다. 여기서는 *SQL Server 버전 12.0 필요*를 사용하겠습니다. 선택적인 **설명**을 추가할 수도 있습니다. 설명은 이 정책 할당이 이 환경에서 만드는 모든 SQL Server가 버전 12.0이 되도록 하는 방법에 대한 세부 정보를 제공합니다.
7. 정책이 기존 리소스에 적용되도록 하려면 가격 책정 계층을 **표준**으로 변경합니다.

   Azure Policy 내에는 *체험* 및 *표준*의 두 가지 가격 책정 계층이 있습니다. 체험 계층을 사용하면 미래의 리소스에만 정책을 적용할 수 있지만, 표준 계층을 사용하면 기존 리소스에도 정책을 적용하여 규정 준수 상태를 보다 더 잘 파악할 수 있습니다. 제한된 미리 보기로 인해 아직 가격 책정 모델이 공개되지 않아 *표준*을 선택해도 요금이 청구되지 않습니다. 가격 책정에 대한 자세한 내용은 [Azure Policy 가격](https://acom-milestone-ignite.azurewebsites.net/pricing/details/azure-policy/)을 참조하세요.

8. **범위**를 선택합니다(이전에 Azure Policy에 옵트인할 때 등록한 구독 또는 리소스 그룹). 범위는 정책 할당이 적용되는 리소스 또는 리소스 그룹을 결정합니다. 구독에서 리소스 그룹까지 다양한 범위가 있습니다.

   이 예제에서는 사용자의 구독 정보와 다를 수 있는 **Azure Analytics 용량 개발** 구독을 사용합니다.

10. **할당**을 선택합니다.

## <a name="implement-a-new-custom-policy"></a>새 사용자 지정 정책 구현

이제 정책 정의를 할당했으므로 환경 전체에서 만들어진 VM이 G 시리즈에 포함될 수 없도록 하여 비용을 절감하는 새 정책을 만듭니다. 이렇게 하면 조직의 사용자가 G 시리즈에서 VM을 만들려고 할 때마다 해당 요청이 거부됩니다.

1. 왼쪽 창의 **제작** 아래에서 **정의**를 선택합니다.

   ![제작 아래의 정의](media/create-manage-policy/definition-under-authoring.png)

2. **+ 정책 정의**를 선택합니다.
3. 다음을 입력합니다.

   - 정책 정의 이름 - *G 시리즈보다 작은 VM SKU 필요*
   - 정책 정의의 용도에 대한 설명 - 여기서는 이 범위에서 만든 모든 VM에서 G 시리즈보다 작은 SKU를 사용하여 비용을 줄인다는 것을 정책 정의에 적용하고 있습니다.
   - 정책 정의가 배치되는 구독 - 여기서는 정책 정의가 사용자의 구독 목록과 다를 수 있는 **Advisor Analytics 용량 개발**에 배치됩니다.
   - 작성되는 json 코드에 포함되는 항목은 다음과 같습니다.
      - 정책 매개 변수
      - 정책 규칙/조건 - 여기서는 'VM SKU 크기가 G 시리즈와 같음'입니다.
      - 정책 효과 - 여기서는 **거부**입니다.

   json 코드는 다음과 같습니다.

```json
{
    "policyRule": {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "like": "Standard_G*"
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
}
```

<!-- Update the following link to the top level samples page
-->
   json 코드 샘플을 보려면 [리소스 정책 개요](../azure-resource-manager/resource-manager-policy.md) 문서를 참조하세요.

4. **저장**을 선택합니다.

## <a name="create-a-policy-definition-with-rest-api"></a>REST API를 사용하여 정책 정의 만들기

정책 정의에 대한 REST API를 사용하여 정책을 만들 수 있습니다. REST API를 사용하여 정책 정의를 만들고, 삭제하고, 기존 정의에 관한 정보를 가져올 수 있습니다.
정책 정의를 만들려면 다음 예제를 사용합니다.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

```
다음 예제와 비슷한 요청 본문을 포함합니다.

```
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>PowerShell을 사용하여 정책 정의 만들기

PowerShell 예제를 진행하기 전에 최신 버전의 Azure PowerShell을 설치했는지 확인합니다. 정책 매개 변수가 버전 3.6.0에 추가되었습니다. 이전 버전이 있는 경우 예제에서는 매개 변수를 찾을 수 없음을 나타내는 오류를 반환합니다.

`New-AzureRmPolicyDefinition` cmdlet을 사용하여 정책 정의를 만들 수 있습니다.

파일에서 정책 정의를 만들려면 경로를 파일에 전달합니다. 외부 파일에 대해서는 다음 예제를 사용합니다.

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

로컬 파일에 대해서는 다음 예제를 사용합니다.

```
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

인라인 규칙을 사용하여 정책 정의를 만들려면 다음 예제를 사용합니다.

```
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

출력 `$definition` 개체에 저장되어 정책 할당 중에 사용됩니다.
다음 예제에서는 매개 변수를 포함하는 정책 정의를 만듭니다.

```
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters
```

## <a name="view-policy-definitions"></a>정책 정의 보기

구독에서 모든 정책 정의를 보려면 다음 명령을 사용합니다.

```
Get-AzureRmPolicyDefinition
```

기본 제공 정책을 비롯한 사용 가능한 모든 정책 정의를 반환합니다. 각 정책은 다음 형식으로 반환됩니다.

```
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Azure CLI를 사용하여 정책 정의 만들기

정책 정의 명령과 함께 Azure CLI를 사용하여 정책 정의를 만들 수 있습니다.
인라인 규칙을 사용하여 정책 정의를 만들려면 다음 예제를 사용합니다.

```
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```

## <a name="view-policy-definitions"></a>정책 정의 보기

구독에서 모든 정책 정의를 보려면 다음 명령을 사용합니다.

```
az policy definition list
```

기본 제공 정책을 비롯한 사용 가능한 모든 정책 정의를 반환합니다. 각 정책은 다음 형식으로 반환됩니다.

```
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>이니셔티브 정의 만들기 및 할당

이니셔티브 정의를 사용하면 여러 정책 정의를 그룹화하여 매우 중요한 하나의 목표를 달성할 수 있습니다. 정의 범위 내의 리소스에서 이니셔티브 정의를 구성하는 정책 정의의 준수를 유지하도록 하는 이니셔티브 정의를 만듭니다.  이니셔티브 정의에 대한 자세한 내용은 [Azure Policy 개요](./azure-policy-introduction.md)를 참조하세요.

### <a name="create-an-initiative-definition"></a>이니셔티브 정의 만들기

1. 왼쪽 창의 **제작** 아래에서 **정의**를 선택합니다.

   ![정의 선택](media/create-manage-policy/select-definitions.png)

2. 페이지 위쪽에서 **이니셔티브 정의**를 선택합니다. 그러면 **이니셔티브 정의** 양식으로 이동합니다.
3. 이니셔티브의 이름과 설명을 입력합니다.

   이 예제에서는 리소스에서 보안 가져오기에 대한 정책 정의를 준수하도록 했으며, 이니셔티브의 이름은 **보안 가져오기**이고, 설명은 **이 이니셔티브는 리소스 가져오기와 관련된 모든 정책 정의를 처리하기 위해 만들어졌습니다**입니다.

   ![이니셔티브 정의](media/create-manage-policy/initiative-definition.png)

4. **사용 가능한 정의** 목록을 검색하고 해당 이니셔티브에 추가하려는 정책 정의를 선택합니다. **보안 가져오기** 이니셔티브에는 다음의 기본 제공 정책 정의를 추가합니다.
   - SQL Server 버전 12.0 필요
   - Security Center에서 보호되지 않는 웹 응용 프로그램을 모니터링합니다.
   - Security Center에서 허용 네트워크를 모니터링합니다.
   - Security Center에서 가능한 앱 허용 목록을 모니터링합니다.
   - Security Center에서 암호화되지 않은 VM 디스크를 모니터링합니다.

   ![이니셔티브 정의](media/create-manage-policy/initiative-definition-2.png)

   목록에서 정책 정의를 선택하면 위와 같이 **정책 및 매개 변수** 아래에 표시됩니다.

5. **만들기**를 선택합니다.

### <a name="assign-an-initiative-definition"></a>이니셔티브 정의 할당

1. **제작** 아래의 **정의** 탭으로 이동합니다.
2. 만든 **보안 가져오기** 이니셔티브 정의를 검색합니다.
3. 해당 이니셔티브 정의를 선택하고 **할당**을 선택합니다.

   ![정의 할당](media/create-manage-policy/assign-definition.png)

4. 다음을 입력하여 **할당** 양식을 작성합니다.
   - 이름: 보안 가져오기 할당
   - 설명: 이 이니셔티브 할당은 **Azure Advisor 용량 개발** 구독에서 이 정책 정의 그룹을 적용하도록 조정됩니다.
   - 가격 책정 계층: 표준
   - 이 할당이 적용되는 범위: **Azure Advisor 용량 개발**

5. **할당**을 선택합니다.

## <a name="resolve-a-non-compliant-or-denied-resource"></a>규정 비준수 또는 거부된 리소스 해결

위의 예제에 따라 SQL Server 버전 12.0을 요구하도록 정책 정의를 할당한 후에는 다른 버전으로 만든 SQL Server는 거부됩니다. 이 섹션에서는 다른 버전의 SQL 서버를 만들 때 거부된 시도를 해결하는 방법을 단계별로 안내합니다.

1. 왼쪽 창에서 **할당**을 선택합니다.
2. 모든 정책 할당을 검색하고 *SQL Server 버전 12.0 필요* 할당을 시작합니다.
3. SQL Server를 만들려는 리소스 그룹에 대한 제외를 요청합니다. 여기서는 Microsoft.Sql/servers/databases 아래에 있는 *baconandbeer/Cheetos* 및 *baconandbeer/Chorizo*를 제외합니다.

   ![제외 요청](media/create-manage-policy/request-exclusion.png)

   거부된 리소스를 해결할 수 있는 다른 방법으로, 만든 SQL Server가 필요하다는 정당한 근거가 있으면 정책과 연결된 연락처에 문의하고, 정책에 대한 액세스 권한이 있으면 해당 정책을 직접 편집합니다.

4. **저장**을 선택합니다.

이 섹션에서는 리소스에 대한 제외를 요청하여 SQL 서버 버전 12.0 만들기 시도 거부를 해결했습니다.

## <a name="clean-up-resources"></a>리소스 정리

후속 자습서를 계속 사용하려면 이 자습서에서 만든 리소스를 정리하지 마세요. 계속하지 않으려면 다음 단계를 사용하여 위에서 만든 할당 또는 정의를 삭제합니다.

1. 왼쪽 창에서 **정의**(또는 할당을 삭제하려는 경우 **할당**)를 선택합니다.
2. 방금 만든 새 이니셔티브 또는 정책 정의(또는 할당)를 검색합니다.
3. 해당 정의 또는 할당의 끝에 있는 줄임표를 선택하고 **정의 삭제**(또는 **할당 삭제**)를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서 다음 작업을 성공적으로 완료했습니다.

> [!div class="checklist"]
> * 나중에 만들 리소스에 대한 조건을 적용하는 정책 할당
> * 여러 리소스에 대한 규정 준수를 추적하기 위한 이니셔티브 정의 만들기 및 할당
> * 규정 비준수 또는 거부된 리소스 해결
> * 조직 전체에서 새 정책 구현

정책 정의의 구조에 대한 자세한 내용은 다음 문서를 참조하세요.

> [!div class="nextstepaction"]
> [Azure Policy 정의 구조](policy-definition.md)
