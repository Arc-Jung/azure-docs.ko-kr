---
title: RBAC 및 REST API를 사용 하 여 Azure 리소스에 대 한 액세스 관리-Azure | Microsoft Docs
description: RBAC(역할 기반 액세스 제어) 및 REST API를 사용하여 Azure 리소스에 대한 사용자, 그룹 및 애플리케이션의 액세스를 관리하는 방법을 알아봅니다. 여기에는 액세스 권한을 나열, 부여 및 제거하는 방법이 포함됩니다.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 86ee030e8c97cf3033b9d2d76b8125c64ecf8065
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996465"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-rest-api"></a>RBAC 및 REST API를 사용하여 Azure 리소스에 대한 액세스 관리

[RBAC(역할 기반 액세스 제어)](overview.md)는 Azure 리소스에 대한 액세스를 관리하는 방법입니다. 이 문서에서는 RBAC 및 REST API를 사용하여 사용자, 그룹 및 애플리케이션의 액세스 권한을 관리하는 방법을 설명합니다.

## <a name="list-access"></a>액세스 권한 나열

RBAC에서 역할 할당을 나열하면 액세스 권한이 나열됩니다. 역할 할당을 나열하려면 [역할 할당 - 나열](/rest/api/authorization/roleassignments/list) REST API 중 하나를 사용합니다. 결과를 구체화하려면 범위와 선택적 필터를 지정합니다.

1. 다음 요청으로 시작합니다.

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI 내에서 *{scope}* 를 역할 할당을 나열하려는 범위로 바꿉니다.

    | 범위 | 형식 |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | 관리 그룹 |
    | `subscriptions/{subscriptionId1}` | 구독 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | 리소스 그룹 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | 리소스 |

    이전 예제에서 microsoft. web은 App Service 인스턴스를 참조 하는 리소스 공급자입니다. 마찬가지로 다른 리소스 공급자를 사용 하 여 범위를 지정할 수 있습니다. 자세한 내용은 [Azure 리소스 공급자 및 형식](../azure-resource-manager/resource-manager-supported-services.md) 및 지원 되는 [Azure Resource Manager 리소스 공급자 작업](resource-provider-operations.md)을 참조 하세요.  
     
1. *{filter}* 를 역할 할당 목록을 필터링하기 위해 적용하려는 조건으로 바꿉니다.

    | Filter | 설명 |
    | --- | --- |
    | `$filter=atScope()` | 하위 범위에 역할 할당을 포함 하지 않고 지정 된 범위에 대 한 역할 할당을 나열 합니다. |
    | `$filter=principalId%20eq%20'{objectId}'` | 지정 된 사용자, 그룹 또는 서비스 사용자에 대 한 역할 할당을 나열 합니다. |
    | `$filter=assignedTo('{objectId}')` | 지정 된 사용자 또는 서비스 사용자에 대 한 역할 할당을 나열 합니다. 사용자가 역할 할당을 포함 하는 그룹의 구성원 인 경우 해당 역할 할당도 나열 됩니다. 이 필터는 그룹에 대해 전이적입니다. 즉, 사용자가 그룹의 구성원이 고 해당 그룹이 역할 할당을 포함 하는 다른 그룹의 멤버인 경우 해당 역할 할당도 나열 됩니다. 이 필터는 사용자 또는 서비스 사용자의 개체 ID만 허용 합니다. 그룹의 개체 ID를 전달할 수 없습니다. |

## <a name="grant-access"></a>액세스 허용

RBAC에서 액세스 권한을 부여하기 위해 역할 할당을 만듭니다. 역할 할당을 만들려면 [역할 할당 - 만들기](/rest/api/authorization/roleassignments/create) REST API를 사용하고 보안 주체, 역할 정의 및 범위를 지정합니다. 이 API를 호출하려면 `Microsoft.Authorization/roleAssignments/write` 작업에 액세스할 수 있어야 합니다. 기본 제공 역할의 경우 [소유자](built-in-roles.md#owner) 및 [사용자 액세스 관리자](built-in-roles.md#user-access-administrator)에게만 이러한 작업의 권한이 부여됩니다.

1. 할당하려는 역할 정의에 대한 식별자를 가져오려면 [역할 정의 - 나열](/rest/api/authorization/roledefinitions/list) REST API를 사용하거나 [기본 제공 역할](built-in-roles.md)을 참조하세요.

1. GUID 도구를 사용하여 역할 할당 식별자에 사용할 고유 식별자를 생성합니다. 식별자의 형식은 `00000000-0000-0000-0000-000000000000`입니다.

1. 다음 요청 및 본문으로 시작합니다.

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. URI 내에서 *{scope}* 를 역할 할당에 대한 범위로 바꿉니다.

    | 범위 | 형식 |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | 관리 그룹 |
    | `subscriptions/{subscriptionId1}` | 구독 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | 리소스 그룹 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | 리소스 |

1. *{roleAssignmentName}* 을 역할 할당의 GUID 식별자로 바꿉니다.

1. 요청 본문 내에서 *{scope}* 를 역할 할당의 범위로 바꿉니다.

    | 범위 | 형식 |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | 관리 그룹 |
    | `subscriptions/{subscriptionId1}` | 구독 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | 리소스 그룹 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | 리소스 |

1. *{roleDefinitionId}* 를 역할 정의 식별자로 바꿉니다.

1. *{principalId}* 를 역할이 할당될 사용자, 그룹 또는 서비스 주체의 개체 식별자로 바꿉니다.

## <a name="remove-access"></a>액세스 권한 제거

RBAC에서 액세스 권한을 제거하려면 역할 할당을 제거해야 합니다. 역할 할당을 제거하려면 [역할 할당 - 삭제](/rest/api/authorization/roleassignments/delete) REST API를 사용합니다. 이 API를 호출하려면 `Microsoft.Authorization/roleAssignments/delete` 작업에 액세스할 수 있어야 합니다. 기본 제공 역할의 경우 [소유자](built-in-roles.md#owner) 및 [사용자 액세스 관리자](built-in-roles.md#user-access-administrator)에게만 이러한 작업의 권한이 부여됩니다.

1. 역할 할당 식별자(GUID)를 가져옵니다. 이 식별자는 역할 할당을 처음 만들 때 반환되거나 역할 할당을 나열하여 가져올 수 있습니다.

1. 다음 요청으로 시작합니다.

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. URI 내에서 *{scope}* 를 제거할 역할 할당에 대한 범위로 바꿉니다.

    | 범위 | 형식 |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | 관리 그룹 |
    | `subscriptions/{subscriptionId1}` | 구독 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | 리소스 그룹 |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | 리소스 |

1. *{roleAssignmentName}* 을 역할 할당의 GUID 식별자로 바꿉니다.

## <a name="next-steps"></a>다음 단계

- [Resource Manager 템플릿과 Resource Manager REST API로 리소스 배포](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API 참조](/rest/api/azure/)
- [REST API를 사용하여 Azure 리소스에 대한 사용자 지정 역할 만들기](custom-roles-rest.md)
