---
title: Azure Monitor에서 Windows 이벤트 로그 수집 및 분석 | Microsoft Docs
description: Azure Monitor에 의한 Windows 이벤트 로그 수집을 구성하는 방법을 설명하고,생성되는 레코드에 대한 자세한 정보를 제공합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: aa34196233ce4037ef6fa49b782b9aa958f7632d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87075252"
---
# <a name="windows-event-log-data-sources-in-azure-monitor"></a>Azure Monitor의 Windows 이벤트 로그 데이터 원본
많은 애플리케이션이 Windows 이벤트 로그에 기록되기 때문에 Windows 이벤트 로그는 Windows 에이전트를 사용하여 데이터를 수집하는 가장 일반적인 [데이터 원본](agent-data-sources.md) 중 하나입니다.  모니터링해야 하는 애플리케이션에서 만든 모든 사용자 지정 로그를 지정하는 것 외에 시스템 및 애플리케이션 같은 표준 로그에서 이벤트를 수집할 수 있습니다.

![Windows 이벤트](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Windows 이벤트 로그 수집
[고급 설정의 데이터 메뉴](agent-data-sources.md#configuring-data-sources)에서 Windows 이벤트 로그를 구성합니다.

Azure Monitor에서는 설정에 지정된 Windows 이벤트 로그에서만 이벤트를 수집합니다.  로그 이름을 입력 하 고를 클릭 하 여 이벤트 로그를 추가할 수 있습니다 **+** .  각 로그의 경우 선택한 심각도의 이벤트만 수집됩니다.  수집하려는 특정 로그에 대한 심각도를 확인합니다.  이벤트를 필터링할 추가 조건을 제공할 수 없습니다.

이벤트 로그 이름을 입력하면 Azure Monitor는 일반적인 이벤트 로그 이름을 제안합니다. 추가하려는 로그가 목록에 나타나지 않으면 로그의 전체 이름을 입력하여 추가할 수 있습니다. 이벤트 뷰어를 사용하여 로그의 전체 이름을 찾을 수 있습니다. 이벤트 뷰어에서 로그의 *속성*을 열고 *전체 이름* 필드에서 문자열을 복사합니다.

![Windows 이벤트 구성](media/data-sources-windows-events/configure.png)

> [!NOTE]
> Windows 이벤트 로그의 중요 이벤트는 Azure Monitor 로그의 심각도 "오류"입니다.

## <a name="data-collection"></a>데이터 수집
Azure Monitor에서는 이벤트가 생성될 때 모니터링되는 이벤트 로그에서 선택한 심각도와 일치하는 각 이벤트를 수집합니다.  에이전트는 수집하는 각 이벤트 로그에 해당 위치를 기록합니다.  에이전트가 일정 기간 동안 오프라인 상태로 전환된 경우 마지막으로 오프라인 상태가 유지된 위치에서 이벤트를 수집하며, 이는 에이전트가 오프라인 상태에 있는 동안 해당 이벤트가 생성된 경우에도 마찬가지입니다.  에이전트가 오프라인 상태인 동안 이벤트 로그가 수집되지 않은 이벤트로 래핑하여 덮어쓰여지면 이벤트가 수집되지 않을 가능성이 있습니다.

>[!NOTE]
>Azure Monitor는 키워드 *클래식* 또는 *감사 성공* 및 키워드 *0xa0000000000000*을 포함하는 이벤트 ID 18453과 함께 원본 *MSSQLSERVER*의 SQL Server에서 생성되는 감사 이벤트를 수집하지 않습니다.
>

## <a name="windows-event-records-properties"></a>Windows 이벤트 레코드 속성
Windows 이벤트 레코드는 **이벤트** 형식이며, 다음 테이블에 있는 속성이 있습니다.

| 속성 | Description |
|:--- |:--- |
| Computer |이벤트가 수집된 컴퓨터의 이름입니다. |
| EventCategory |이벤트의 범주. |
| EventData |원시 형식의 모든 이벤트 데이터입니다. |
| EventID |이벤트의 번호입니다. |
| EventLevel |숫자 형식으로 된 이벤트의 심각도입니다. |
| EventLevelName |텍스트 형식으로 된 이벤트의 심각도입니다. |
| EventLog |이벤트가 수집된 이벤트 로그의 이름입니다. |
| ParameterXml |XML 형식의 이벤트 매개 변수 값입니다. |
| ManagementGroupName |System Center Operations Manager 에이전트의 관리 그룹 이름입니다.  다른 에이전트의 경우이 값은입니다. `AOI-<workspace ID>` |
| RenderedDescription |매개 변수 값을 포함하는 이벤트 설명입니다. |
| 원본 |이벤트의 원본입니다. |
| SourceSystem |이벤트가 수집된 에이전트의 유형입니다. <br> OpsManager – Windows 에이전트, 직접 연결 또는 관리된 Operations Manager <br> Linux – 모든 Linux 에이전트  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |Windows에서 이벤트가 만들어진 날짜 및 시간입니다. |
| UserName |이벤트를 로깅한 계정의 사용자 이름입니다. |

## <a name="log-queries-with-windows-events"></a>Windows 이벤트로 로그 쿼리
다음 표에서는 Windows 이벤트 레코드를 검색하는 로그 쿼리의 다양한 예제를 제공합니다.

| 쿼리 | Description |
|:---|:---|
| 이벤트 |모든 Windows 이벤트 |
| Event &#124; where EventLevelName == "error" |심각도가 오류인 모든 Windows 이벤트 |
| Event &#124; summarize count() by Source |원본별 Windows 이벤트 수 |
| Event &#124; where EventLevelName == "error" &#124; summarize count() by Source |원본별 Windows 오류 이벤트 수 |


## <a name="next-steps"></a>다음 단계
* 분석을 위해 다른 [데이터 원본](agent-data-sources.md) 을 수집하도록 Log Analytics를 구성합니다.
* 데이터 원본 및 솔루션에서 수집한 데이터를 분석하는 [로그 쿼리](../log-query/log-query-overview.md)에 대해 알아봅니다.  
* Windows 에이전트에서 [성능 카운터 수집](data-sources-performance-counters.md)을 구성합니다.
