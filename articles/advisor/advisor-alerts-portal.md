---
title: Azure Portal를 사용 하 여 새 권장 사항에 대 한 Azure Advisor 경고 만들기
description: 새 추천에 대한 Azure Advisor 경고를 만듭니다.
ms.topic: article
ms.date: 09/09/2019
ms.openlocfilehash: 3c51479821914ef34edcd13d8708344169f17aae
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100590113"
---
# <a name="create-azure-advisor-alerts-on-new-recommendations-using-the-azure-portal"></a>Azure Portal를 사용 하 여 새 권장 사항에 대 한 Azure Advisor 경고 만들기 

이 문서에서는 Azure Portal를 사용 하 여 Azure Advisor의 새 권장 사항에 대 한 경고를 설정 하는 방법을 보여 줍니다. 

Azure Advisor에서 리소스 중 하나에 대한 새 추천을 검색할 때마다 이벤트가 [Azure 활동 로그](../azure-monitor/essentials/platform-logs-overview.md)에 저장됩니다. 추천별 경고 만들기 환경을 사용하여 Azure Advisor에서 이러한 이벤트에 대한 경고를 설정할 수 있습니다. 구독을 선택하고 필요에 따라 리소스 그룹을 선택하여 경고를 받도록 하려는 리소스를 지정할 수 있습니다. 

다음 속성을 사용하여 추천 유형을 확인할 수도 있습니다.

* 범주
* 영향 수준
* 추천 유형

다음과 같은 방법으로 경고가 트리거될 때 수행되는 작업을 구성할 수도 있습니다.  

* 기존 작업 그룹 선택
* 새 작업 그룹 만들기

작업 그룹에 대해 자세히 알아보려면 [작업 그룹 만들기 및 관리](../azure-monitor/alerts/action-groups.md)를 참조하세요.

> [!NOTE] 
> Advisor 경고는 현재 고가용성, 성능 및 비용 추천에만 사용할 수 있습니다. 보안 추천은 지원되지 않습니다. 

## <a name="create-alert-rule"></a>경고 규칙 만들기
1. **포털** 에서 **Azure Advisor** 를 선택 합니다.

    ![포털의 Azure Advisor](./media/advisor-alerts/create1.png)

2. 왼쪽 메뉴의 **모니터링** 섹션에서 **경고** 를 선택 합니다. 

    ![Advisor의 경고](./media/advisor-alerts/create2.png)

3. **새 Advisor 경고** 를 선택 합니다.

    ![새 Advisor 경고](./media/advisor-alerts/create3.png)

4. **범위** 섹션에서 구독을 선택 하 고, 필요에 따라 경고를 표시 하려는 리소스 그룹을 선택 합니다. 

    ![Advisor 경고 범위](./media/advisor-alerts/create4.png)

5. **조건** 섹션에서 경고를 구성 하는 데 사용할 방법을 선택 합니다. 특정 범주 및/또는 영향 수준에 대 한 모든 권장 사항에 대 한 경고를 표시 하려면 **범주 및 영향 수준** 을 선택 합니다. 특정 유형의 모든 권장 사항에 대 한 경고를 표시 하려면 **권장 사항 유형** 을 선택 합니다.

    ![Azure Advisor 경고 조건](./media/advisor-alerts/create5.png)

6. 선택한 구성 옵션에 따라 조건을 지정할 수 있습니다. 모든 권장 사항을 적용 하려면 나머지 필드를 비워 둡니다. 

    ![Advisor 경고 작업 그룹](./media/advisor-alerts/create6.png)

7. **작업 그룹** 섹션에서 **기존 항목 추가** 를 선택 하 여 이미 만든 작업 그룹을 사용 하거나 **새로 만들기** 를 선택 하 여 새 [작업 그룹](../azure-monitor/alerts/action-groups.md)을 설정 합니다. 

    ![Advisor 경고 기존 추가](./media/advisor-alerts/create7.png)

8. 경고 정보 섹션에서 경고에 이름 및 간단한 설명을 제공 합니다. 경고를 사용 하도록 설정 하려면 **만들 때 규칙 사용** 을 **예** 로 설정 된 상태로 둡니다. 그런 다음 경고를 저장할 리소스 그룹을 선택 합니다. 권장 구성의 대상 범위에는 영향을 주지 않습니다. 

    :::image type="content" source="./media/advisor-alerts/create8.png" alt-text="경고 정보 섹션의 스크린샷":::


## <a name="configure-recommendation-alerts-to-use-a-webhook"></a>Webhook를 사용 하도록 권장 구성 경고 구성
이 섹션에서는 기존 시스템에 대 한 웹 후크를 통해 권장 사항 데이터를 보내도록 Azure Advisor 경고를 구성 하는 방법을 보여 줍니다. 

리소스 중 하나에 새 Advisor 권장 사항이 있는 경우 알림을 받도록 경고를 설정할 수 있습니다. 이러한 경고는 전자 메일 또는 문자 메시지를 통해 알림을 받을 수 있지만 webhook를 통해 기존 시스템과 통합 하는 데 사용할 수도 있습니다. 


### <a name="using-the-advisor-recommendation-alert-payload"></a>Advisor 권장 사항 경고 페이로드 사용
웹 후크를 사용 하 여 사용자 고유의 시스템에 Advisor 경고를 통합 하려면 알림에서 전송 되는 JSON 페이로드를 구문 분석 해야 합니다. 

이 경고에 대 한 작업 그룹을 설정할 때 일반적인 경고 스키마를 사용할지 여부를 선택 합니다. 일반적인 경고 스키마를 선택 하는 경우 페이로드는 다음과 같습니다. 

```json
{  
   "schemaId":"azureMonitorCommonAlertSchema",
   "data":{  
      "essentials":{  
         "alertId":"/subscriptions/<subid>/providers/Microsoft.AlertsManagement/alerts/<alerted>",
         "alertRule":"Webhhook-test",
         "severity":"Sev4",
         "signalType":"Activity Log",
         "monitorCondition":"Fired",
         "monitoringService":"Activity Log - Recommendation",
         "alertTargetIDs":[  
            "/subscriptions/<subid>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>"
         ],
         "originAlertId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "firedDateTime":"2019-07-17T23:00:57.3858656Z",
         "description":"A new recommendation is available.",
         "essentialsVersion":"1.0",
         "alertContextVersion":"1.0"
      },
      "alertContext":{  
         "channels":"Operation",
         "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
         "caller":"Microsoft.Advisor",
         "correlationId":"8554b847-2a72-48ef-9776-600aca3c3aab",
         "eventSource":"Recommendation",
         "eventTimestamp":"2019-07-17T22:28:54.1566942+00:00",
         "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
         "eventDataId":"001d8b40-5d41-4310-afd7-d65c9d4428ed",
         "level":"Informational",
         "operationName":"Microsoft.Advisor/recommendations/available/action",
         "properties":{  
            "recommendationSchemaVersion":"1.0",
            "recommendationCategory":"Performance",
            "recommendationImpact":"Medium",
            "recommendationName":"Increase the MariaDB server vCores",
            "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%<subscription id>%2FresourceGroups%2<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
            "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
         },
         "status":"Active",
         "subStatus":"",
         "submissionTimestamp":"2019-07-17T22:28:54.1566942+00:00"
      }
   }
}
  ```

공통 스키마를 사용 하지 않는 경우 페이로드는 다음과 같습니다. 

```json
{  
   "schemaId":"Microsoft.Insights/activityLogs",
   "data":{  
      "status":"Activated",
      "context":{  
         "activityLog":{  
            "channels":"Operation",
            "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
            "caller":"Microsoft.Advisor",
            "correlationId":"3ea7320f-c002-4062-adb8-96d3bd92a5f4",
            "description":"A new recommendation is available.",
            "eventSource":"Recommendation",
            "eventTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
            "eventDataId":"a12b8e59-0b1d-4003-bfdc-3d8152922e59",
            "level":"Informational",
            "operationName":"Microsoft.Advisor/recommendations/available/action",
            "properties":{  
               "recommendationSchemaVersion":"1.0",
               "recommendationCategory":"Performance",
               "recommendationImpact":"Medium",
               "recommendationName":"Increase the MariaDB server vCores",
               "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/source/ActivityLog/recommendationTypeId/a5f888e3-8cf4-4491-b2ba-b120e14eb7ce/resourceId/%2Fsubscriptions%2F<subscription id>%2FresourceGroups%2F<resource group name>%2Fproviders%2FMicrosoft.DBforMariaDB%2Fservers%2F<resource name>",
               "recommendationType":"a5f888e3-8cf4-4491-b2ba-b120e14eb7ce"
            },
            "resourceId":"/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/microsoft.dbformariadb/servers/<resource name>",
            "resourceGroupName":"<resource group name>",
            "resourceProviderName":"MICROSOFT.DBFORMARIADB",
            "status":"Active",
            "subStatus":"",
            "subscriptionId":"<subscription id>",
            "submissionTimestamp":"2019-07-17T20:36:39.3966926+00:00",
            "resourceType":"MICROSOFT.DBFORMARIADB/SERVERS"
         }
      },
      "properties":{  
 
      }
   }
}
```

어느 스키마에서 나 **eventSource** 를 찾고 `Recommendation` **OperationName**  이 인 경우 Advisor 추천 이벤트를 식별할 수 있습니다 `Microsoft.Advisor/recommendations/available/action` .

사용할 수 있는 다른 중요 한 필드는 다음과 같습니다. 

* *alertTargetIDs* (common schema) 또는 *resourceId* (레거시 스키마)
* *recommendationType*
* *recommendationName*
* *recommendationCategory*
* *recommendationImpact*
* *recommendationResourceLink*


## <a name="manage-your-alerts"></a>경고 관리 

Azure Advisor에서 권장 구성 경고를 편집, 삭제 또는 사용 하지 않도록 설정 하 고 사용 하도록 설정할 수 있습니다. 

1. **포털** 에서 **Azure Advisor** 를 선택 합니다.

    :::image type="content" source="./media/advisor-alerts/create1.png" alt-text="선택 Azure Advisor를 표시 하는 Azure Portal 메뉴의 스크린샷":::

2. 왼쪽 메뉴의 **모니터링** 섹션에서 **경고** 를 선택 합니다.

    :::image type="content" source="./media/advisor-alerts/create2.png" alt-text="선택한 경고를 표시 하는 Azure Portal 메뉴의 스크린샷":::

3. 경고를 편집 하려면 경고 이름을 클릭 하 여 경고를 열고 편집 하려는 필드를 편집 합니다.

4. 경고를 삭제, 활성화 또는 비활성화 하려면 행 끝에 있는 줄임표를 클릭 한 다음 수행할 작업을 선택 합니다.
 

## <a name="next-steps"></a>다음 단계
- [활동 로그 경고의 개요](../azure-monitor/alerts/alerts-overview.md)를 확인하고 경고를 받는 방법에 대해 알아보세요.
- [작업 그룹](../azure-monitor/alerts/action-groups.md)에 대해 자세히 알아보세요.
