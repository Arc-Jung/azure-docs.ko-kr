---
title: 대규모로 위임 된 리소스 모니터링
description: 관리 중인 고객 테 넌 트에서 확장 가능한 방식으로 Azure Monitor 로그를 효과적으로 사용 하는 방법을 알아봅니다.
ms.date: 02/11/2021
ms.topic: how-to
ms.openlocfilehash: 98fd984492276dbdfbc2f8001bca19560764a2a7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101742588"
---
# <a name="monitor-delegated-resources-at-scale"></a>대규모로 위임 된 리소스 모니터링

서비스 공급자로 서 [Azure Lighthouse](../overview.md)에 여러 고객 테 넌 트를 등록 했을 수 있습니다. Azure Lighthouse를 사용하여 서비스 공급자는 여러 테넌트에 걸쳐 대규모로 작업을 한 번에 수행할 수 있으므로 관리 작업을 보다 효율적으로 수행할 수 있습니다.

이 항목에서는 관리 중인 고객 테 넌 트에서 확장 가능한 방식으로 [Azure Monitor 로그](../../azure-monitor/logs/data-platform-logs.md) 를 사용 하는 방법을 보여 줍니다. 이 항목의 서비스 공급자 및 고객을 참조 하지만이 지침은 [Azure Lighthouse를 사용 하 여 여러 테 넌 트를 관리](../concepts/enterprise.md)하는 기업에도 적용 됩니다.

> [!NOTE]
> 사용자를 관리 하는 테 넌 트의 사용자에 게 위임 된 고객 구독의 [Log Analytics 작업 영역을 관리 하는 데 필요한 역할이](../../azure-monitor/logs/manage-access.md#manage-access-using-azure-permissions) 부여 되어 있어야 합니다.

## <a name="create-log-analytics-workspaces"></a>Log Analytics 작업 영역 만들기

데이터를 수집 하려면 Log Analytics 작업 영역을 만들어야 합니다. 이러한 Log Analytics 작업 영역은 Azure Monitor에 의해 수집 되는 데이터에 대 한 고유한 환경입니다. 각 작업 영역에는 자체 데이터 리포지토리 및 구성이 있으며 데이터 원본 및 솔루션은 특정 작업 영역에 데이터를 저장하도록 구성됩니다.

이러한 작업 영역을 고객 테 넌 트에 직접 만드는 것이 좋습니다. 이러한 방식으로 데이터가 자신의 테 넌 트로 내보내지지 않고 테 넌 트에 유지 됩니다. 또한 Log Analytics에서 지 원하는 모든 리소스 또는 서비스를 중앙에서 모니터링할 수 있으므로 모니터링 하는 데이터 형식에 대 한 유연성을 높일 수 있습니다.

> [!TIP]
> Log Analytics 작업 영역에서 데이터에 액세스 하는 데 사용 되는 모든 automation 계정은 작업 영역과 동일한 테 넌 트에 만들어야 합니다.

[Azure Portal](../../azure-monitor/logs/quick-create-workspace.md)를 사용 하거나 [Azure CLI](../../azure-monitor/logs/quick-create-workspace-cli.md)를 사용 하거나 [Azure PowerShell](../../azure-monitor/logs/powershell-workspace-configuration.md)를 사용 하 여 Log Analytics 작업 영역을 만들 수 있습니다.

> [!IMPORTANT]
> 모든 작업 영역이 고객 테 넌 트에서 생성 되더라도 Microsoft Insights 리소스 공급자는 관리 테 넌 트의 구독에도 등록 해야 합니다.

## <a name="deploy-policies-that-log-data"></a>데이터를 기록 하는 정책 배포

Log Analytics 작업 영역을 만든 후에는 각 테 넌 트의 적절 한 작업 영역에 진단 데이터를 보내도록 고객 계층 구조 전체에 [Azure Policy](../../governance/policy/index.yml) 를 배포할 수 있습니다. 배포 하는 정확한 정책은 모니터링 하려는 리소스 종류에 따라 달라질 수 있습니다.

정책을 만드는 방법에 대 한 자세한 내용은 [자습서: 규정 준수를 적용 하는 정책 만들기 및 관리](../../governance/policy/tutorials/create-and-manage.md)를 참조 하세요. 이 [커뮤니티 도구](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) 는 사용자가 선택 하는 특정 리소스 종류를 모니터링 하는 정책을 만드는 데 도움이 되는 스크립트를 제공 합니다.

배포할 정책을 결정 한 경우 [대규모로 위임 된 구독에 배포할](policy-at-scale.md)수 있습니다.

## <a name="analyze-the-gathered-data"></a>수집 된 데이터 분석

정책을 배포한 후에는 각 고객 테 넌 트에서 만든 Log Analytics 작업 영역에 데이터가 로깅됩니다. 모든 관리 되는 고객에 대 한 통찰력을 얻기 위해 [Azure Monitor 통합 문서](../../azure-monitor/visualize/workbooks-overview.md) 와 같은 도구를 사용 하 여 여러 데이터 원본의 정보를 수집 하 고 분석할 수 있습니다.

## <a name="view-alerts-across-customers"></a>고객 간에 경고 보기

사용자가 관리 하는 고객 테 넌 트에서 위임 된 구독에 대 한 [경고](../../azure-monitor/alerts/alerts-overview.md) 를 볼 수 있습니다.

관리 테 넌 트에서 Azure Portal 또는 Api 및 관리 도구를 통해 [활동 로그 경고를 만들고, 보고, 관리할](../../azure-monitor/alerts/alerts-activity-log.md) 수 있습니다.

여러 고객에 대해 경고를 자동으로 새로 고치려면 [Azure 리소스 그래프](../../governance/resource-graph/overview.md) 쿼리를 사용 하 여 경고를 필터링 합니다. 대시보드에 쿼리를 고정 하 고 해당 하는 모든 고객 및 구독을 선택할 수 있습니다. 예를 들어 아래 쿼리는 심각도 0 및 경고 1 개를 표시 하 고 60 분 마다 새로 고칩니다.

```kusto
alertsmanagementresources
| where type == "microsoft.alertsmanagement/alerts"
| where properties.essentials.severity =~ "Sev0" or properties.essentials.severity =~ "Sev1"
| where properties.essentials.monitorCondition == "Fired"
| where properties.essentials.startDateTime > ago(60m)
| project StartTime=properties.essentials.startDateTime,name,Description=properties.essentials.description, Severity=properties.essentials.severity, subscriptionId
| sort by tostring(StartTime)
```

## <a name="next-steps"></a>다음 단계

- GitHub에서 도메인 통합 문서 [별로 활동 로그](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) 를 사용해 보세요.
- 여러 Log Analytics 작업 영역에서 [업데이트 관리 로그를 쿼리하여](../../automation/update-management/query-logs.md) 패치 준수 보고를 추적 하는이 [MVP 제작 샘플 통합 문서](https://github.com/scautomation/Azure-Automation-Update-Management-Workbooks)를 살펴보세요. 
- 다른 [교차 테 넌 트 관리 환경](../concepts/cross-tenant-management-experience.md)에 대해 알아봅니다.