---
title: 경고 마이그레이션을 위해 논리 앱 & runbook 업데이트
description: 자발적 마이그레이션을 준비 하기 위해 웹 후크, 논리 앱 및 runbook을 수정 하는 방법에 대해 알아봅니다.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: ce61c3539a4ea29cbeb48c379ed143363500701e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102038019"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>클래식 경고 규칙 마이그레이션을 위한 논리 앱 및 Runbook 준비

> [!NOTE]
> [이전에 발표](monitoring-classic-retirement.md)한 대로 Azure Monitor의 클래식 경고는 공용 클라우드 사용자에 게 사용이 중지 되었지만 31 년 5 **월 2021** 일까 지 계속 사용 됩니다. Azure Government 클라우드 및 Azure 중국 21Vianet에 대 한 클래식 경고는 **2024 년 2 월 29 일** 에 사용 중지 됩니다.
>

기존 경고 규칙을 자발적으로 새 경고 규칙으로 마이그레이션하기를 선택 하는 경우 두 시스템 간에 약간의 차이가 있습니다. 이 문서에서는 이러한 차이점과 변경을 준비 하는 방법을 설명 합니다.

## <a name="api-changes"></a>API 변경 내용

클래식 경고 규칙 ()을 만들고 관리 하는 Api는 `microsoft.insights/alertrules` 새 메트릭 경고 ()를 만들고 관리 하는 api와 다릅니다 `microsoft.insights/metricalerts` . 지금 기존 경고 규칙을 프로그래밍 방식으로 만들고 관리 하는 경우 새 Api를 사용 하도록 배포 스크립트를 업데이트 합니다.

다음 표는 클래식 및 새 경고의 프로그래밍 인터페이스에 대 한 참조입니다.

| 배포 스크립트 유형 | 클래식 경고 | 새 메트릭 경고 |
| ---------------------- | -------------- | ----------------- |
|REST API     | [microsoft insights/alertrules](/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](/rest/api/monitor/metricalerts)       |
|Azure CLI     | [az monitor alert](/cli/azure/monitor/alert)        | [az monitor 메트릭 경고](/cli/azure/monitor/metrics/alert)        |
|PowerShell      | [참조](/powershell/module/az.monitor/add-azmetricalertrule)       |  [참조](/powershell/module/az.monitor/add-azmetricalertrulev2)    |
| Azure Resource Manager 템플릿 | [클래식 경고의 경우](./alerts-enable-template.md)|[새 메트릭 경고의 경우](./alerts-metric-create-templates.md)|

## <a name="notification-payload-changes"></a>알림 페이로드 변경

알림 페이로드 형식은 [클래식 경고 규칙과](alerts-webhooks.md) [새 메트릭 경고](alerts-metric-near-real-time.md#payload-schema)간에 약간 다릅니다. Webhook, 논리 앱 또는 runbook 작업을 사용 하는 클래식 경고 규칙이 있는 경우 새 페이로드 형식을 적용 하도록 대상을 업데이트 해야 합니다.

다음 표를 사용 하 여 기본 형식에서 새 형식으로 webhook 페이로드 필드를 매핑합니다.

| 알림 끝점 유형 | 클래식 경고 | 새 메트릭 경고 |
| -------------------------- | -------------- | ----------------- |
|경고가 활성화 또는 해결 되었습니까?    | **status**       | **데이터. 상태** |
|경고에 대 한 컨텍스트 정보     | **context**        | **데이터. 컨텍스트**        |
|경고가 활성화 되거나 해결 된 타임 스탬프입니다.     | **context. timestamp**       | **data. context. timestamp**        |
| 경고 규칙 ID | **context.id** | **data.context.id** |
| 경고 규칙 이름 | **context.name** | **data.context.name** |
| 경고 규칙에 대 한 설명 | **컨텍스트입니다. 설명** | **데이터. 컨텍스트 설명** |
| 경고 규칙 조건 | **context. 조건** | **data. context. 조건** |
| 메트릭 이름 | **metricName** | **metricName [0]..** |
| 시간 집계 (평가 기간 동안 메트릭이 집계 되는 방법)| **컨텍스트별. timeAggregation** | **컨텍스트별. timeAggregation** |
| 평가 기간 | **windowSize** | **windowSize.** |
| 연산자 (집계 된 메트릭 값을 임계값과 비교 하는 방법) | **컨텍스트별. 연산자** | **data. condition. 연산자** |
| 임계값 | **컨텍스트별. threshold** | **data. context. condition. allOf [0]. threshold** |
| 메트릭 값 | **metricValue** | **metricValue [0]..** |
| 구독 ID | **context. subscriptionId** | **데이터. context. subscriptionId** |
| 영향을 받는 리소스의 리소스 그룹 | **컨텍스트별. resourceGroup** | **데이터. 컨텍스트와 resourceGroup** |
| 영향을 받는 리소스의 이름 | **Context.resourcename** | **data. Context.resourcename** |
| 영향을 받는 리소스의 유형입니다. | **컨텍스트별. resourceType** | **데이터. 컨텍스트 resourceType** |
| 영향을 받는 리소스의 리소스 ID | **컨텍스트별. resourceId** | **데이터. 컨텍스트 resourceId** |
| 포털 리소스 요약 페이지에 대 한 직접 링크 | **portalLink** | **portalLink** |
| 웹 후크 또는 논리 앱에 전달할 사용자 지정 페이로드 필드 | **properties** | **데이터. 속성** |

페이로드는 볼 수 있는 것과 유사 합니다. 다음 섹션에서 다음을 제공 합니다.

- 새 형식으로 작업 하기 위해 논리 앱을 수정 하는 방법에 대해 자세히 설명 합니다.
- 새 경고에 대 한 알림 페이로드를 구문 분석 하는 runbook 예제입니다.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>메트릭 경고 알림을 받도록 논리 앱 수정

클래식 경고를 사용 하는 논리 앱을 사용 하는 경우 새 메트릭 경고 페이로드를 구문 분석 하도록 논리 앱 코드를 수정 해야 합니다. 다음 단계를 수행합니다.

1. 새 논리 앱을 만듭니다.

1. "Azure Monitor-메트릭 경고 처리기" 템플릿을 사용 합니다. 이 템플릿에는 적절 한 스키마가 정의 된 **HTTP 요청** 트리거가 있습니다.

    ![빈 논리 앱 및 Azure Monitor-메트릭 경고 처리기의 두 단추를 보여 주는 스크린샷](media/alerts-prepare-migration/logic-app-template.png "메트릭 경고 템플릿")

1. 처리 논리를 호스트 하는 작업을 추가 합니다.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>메트릭 경고 알림을 수신 하는 automation runbook 사용

다음 예제에서는 runbook에서 사용할 수 있는 PowerShell 코드를 제공 합니다. 이 코드는 클래식 메트릭 경고 규칙 및 새 메트릭 경고 규칙에 대 한 페이로드를 구문 분석할 수 있습니다.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

경고가 트리거될 때 가상 머신을 중지 하는 runbook의 전체 예는 [Azure Automation 설명서](../../automation/automation-create-alert-triggered-runbook.md)를 참조 하세요.

## <a name="partner-integration-via-webhooks"></a>웹 후크를 통한 파트너 통합

[클래식 경고와 통합 되는](../partners.md) 대부분의 파트너는 통합을 통해 최신 메트릭 경고를 이미 지원 합니다. 이미 새 메트릭 경고와 함께 작동 하는 알려진 통합은 다음과 같습니다.

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Signl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

여기에 나열 되지 않은 파트너 통합을 사용 하는 경우 공급자에 게 새 메트릭 경고를 사용 하 고 있는지 확인 합니다.

## <a name="next-steps"></a>다음 단계

- [마이그레이션 도구 사용 방법](alerts-using-migration-tool.md)
- [마이그레이션 도구의 작동 방식 이해](alerts-understand-migration.md)