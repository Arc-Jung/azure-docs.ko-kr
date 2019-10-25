---
title: Azure Resource Manager 템플릿을 사용하여 Azure Application Insights 스마트 검색 규칙 설정 구성 | Microsoft Docs
description: Azure Resource Manager 템플릿을 사용하여 Azure Application Insights 스마트 검색 규칙의 관리 및 구성 자동화
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 06/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 8b55271b39bf2a65dababbef58f7389ca07d57d8
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72818841"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Azure Resource Manager 템플릿을 사용하여 Application Insights 스마트 검색 규칙 관리

[Azure Resource Manager 템플릿](../../azure-resource-manager/resource-group-authoring-templates.md)을 사용하여 Application Insights의 스마트 검색 규칙을 관리하고 구성할 수 있습니다.
이 방법은 Azure Resource Manager 자동화를 사용하여 새 Application Insights 리소스를 배포하거나 기존 리소스의 설정을 수정할 때 사용할 수 있습니다.

## <a name="smart-detection-rule-configuration"></a>스마트 검색 규칙 구성

스마트 검색 규칙에 대한 다음 설정을 구성할 수 있습니다.
- 규칙을 사용하도록 설정된 경우(기본값은 **true**.)
- 검색이 검색 될 때 구독의 [모니터링 판독기](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) 및 [모니터링 참여자](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) 역할에 연결 된 사용자에 게 전자 메일을 보내야 하는 경우 (기본값은 **true**입니다.)
- 항목이 검색되었을 때 알림을 받아야 하는 추가 메일 수신인
    -  _미리 보기로_표시 된 스마트 검색 규칙에는 전자 메일 구성을 사용할 수 없습니다.

Azure Resource Manager를 통해 규칙 설정을 구성할 수 있도록, 이제 스마트 검색 규칙 구성은 Application Insights 리소스 내부에서 **ProactiveDetectionConfigs**라고 하는 내부 리소스로 제공됩니다.
유연성을 극대화하기 위해 고유한 알림 설정을 사용하여 각 스마트 검색 규칙을 구성할 수 있습니다.

## 

## <a name="examples"></a>예시

아래는 Azure Resource Manager 템플릿을 사용하여 스마트 검색 규칙 설정을 구성하는 방법을 보여주는 몇 가지 예입니다.
모든 샘플은 _“myApplication”_ 이라는 Application Insights 리소스 및 내부에서 _“longdependencyduration”_ 이라고 부르는 "긴 종속성 기간 스마트 검색 규칙"을 참조합니다.
잊지 말고 Application Insights 리소스 이름을 바꾸고, 관련 스마트 검색 규칙 내부 이름을 지정하세요. 아래 표에서 각 스마트 검색 규칙에 해당하는 내부 Azure Resource Manager 이름 목록을 확인하세요.

### <a name="disable-a-smart-detection-rule"></a>스마트 검색 규칙 사용 안 함

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>스마트 검색 규칙에 이메일 전송 알림 사용 안 함

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>스마트 검색 규칙에 대한 추가 이메일 수신인 추가

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```

### <a name="failure-anomalies-v2-non-classic-alert-rule"></a>실패 변칙 v2 (비 클래식) 경고 규칙

이 Azure Resource Manager 템플릿에서는 심각도가 2 인 오류 비정상 v2 경고 규칙을 구성 하는 방법을 보여 줍니다. 이 새 버전의 오류 비정상 경고 규칙은 새 Azure 경고 플랫폼의 일부 이며, [클래식 경고](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/)사용 중지 프로세스의 일부로 사용 중지 중인 클래식 버전을 대체 합니다.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "microsoft.alertsmanagement/smartdetectoralertrules",
            "apiVersion": "2019-03-01",
            "name": "Failure Anomalies - my-app",
            "location": "global", 
            "properties": {
                  "description": "Failure Anomalies notifies you of an unusual rise in the rate of failed HTTP requests or dependency calls.",
                  "state": "Enabled",
                  "severity": "2",
                  "frequency": "PT1M",
                  "detector": {
                  "id": "FailureAnomaliesDetector"
                  },
                  "scope": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/MyResourceGroup/providers/microsoft.insights/components/my-app"],
                  "actionGroups": {
                        "groupIds": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourcegroups/MyResourceGroup/providers/microsoft.insights/actiongroups/MyActionGroup"]
                  }
            }
        }
    ]
}
```

> [!NOTE]
> 이 Azure Resource Manager 템플릿은 오류 변칙 v2 경고 규칙에 고유 하며이 문서에 설명 된 다른 클래식 스마트 검색 규칙과 다릅니다.   

## <a name="smart-detection-rule-names"></a>스마트 검색 규칙 이름

아래 표에는 포털에 나타나는 스마트 검색 규칙 이름과 Azure Resource Manager 템플릿에서 사용해야 하는 내부 이름이 정리되어 있습니다.

> [!NOTE]
> _미리 보기로_ 표시 된 스마트 검색 규칙은 전자 메일 알림을 지원 하지 않습니다. 따라서 이러한 규칙에 대해 _enabled_ 속성만 설정할 수 있습니다. 

| Azure Portal 규칙 이름 | 내부 이름
|:---|:---|
| 느린 페이지 로드 시간 | slowpageloadtime |
| 느린 서버 응답 시간 | slowserverresponsetime |
| 긴 종속성 기간 | longdependencyduration |
| 서버 응답 시간 저하 | degradationinserverresponsetime |
| 종속성 기간 저하 | degradationindependencyduration |
| 추적 심각도 비율의 저하(미리 보기) | extension_traceseveritydetector |
| 예외 볼륨의 비정상적인 증가(미리 보기) | extension_exceptionchangeextension |
| 잠재적인 메모리 누수 검색됨(미리 보기) | extension_memoryleakextension |
| 잠재적인 보안 문제 검색됨(미리 보기) | extension_securityextensionspackage |
| 일일 데이터 볼륨의 비정상적인 증가 (미리 보기) | extension_billingdatavolumedailyspikeextension |

## <a name="next-steps"></a>다음 단계

자동으로 감지하는 방법에 대해 자세히 알아봅니다.

- [오류 잘못된 부분](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [메모리 누수](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [성능 이상](../../azure-monitor/app/proactive-performance-diagnostics.md)
