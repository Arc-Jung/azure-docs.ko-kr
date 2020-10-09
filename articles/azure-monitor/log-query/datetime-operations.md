---
title: Azure Monitor 로그 쿼리에서 날짜/시간 값 사용 | Microsoft Docs
description: Azure Monitor 로그 쿼리에서 날짜/시간 데이터를 사용하는 방법을 설명합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: ea7c98a1b5b4059c5fea0cf1e8ea2ff5ef08d9d1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77655381"
---
# <a name="working-with-date-time-values-in-azure-monitor-log-queries"></a>Azure Monitor 로그 쿼리에서 날짜/시간 값 사용

> [!NOTE]
> 이 단원을 완료하기 전에 [Analytics 포털 시작](get-started-portal.md) 및 [쿼리 시작](get-started-queries.md)을 완료해야 합니다.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

이 문서에서는 Azure Monitor 로그 쿼리에서 날짜/시간 데이터를 사용하는 방법을 설명합니다.


## <a name="date-time-basics"></a>날짜/시간 기본 사항
Kusto 쿼리 언어에는 날짜 및 시간과 관련된 두 가지 기본 데이터 형식인 datetime과 timespan이 있습니다. 모든 날짜는 UTC로 표현됩니다. 여러 날짜/시간 형식이 지원되지만 ISO8601 형식이 선호됩니다. 

timespan은 10진수 다음에 시간 단위를 사용해서 표현됩니다.

|축약형   | 시간 단위    |
|:---|:---|
|일           | 일          |
|h           | hour         |
|분           | minute       |
|초           | second       |
|ms          | 밀리초  |
|microsecond | microsecond  |
|tick        | 나노초   |

날짜/시간은 `todatetime` 연산자로 문자열을 캐스팅하여 만들 수 있습니다. 예를 들어 특정 시간 프레임에 전송된 VM 하트비트를 검토하려면 `between` 연산자를 사용하여 시간 범위를 지정합니다.

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

또 다른 일반적인 시나리오는 날짜/시간을 현재와 비교하는 것입니다. 예를 들어, 지난 2분 동안의 모든 하트비트를 보려면 2분을 나타내는 시간 범위와 `now` 연산자를 함께 사용할 수 있습니다.

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

이러한 함수에 대해 축약형을 사용할 수도 있습니다.
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

그렇지만 가장 짧고 읽기 쉬운 방법은 `ago` 연산자를 사용하는 것입니다.
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

시작 및 종료 시간을 아는 대신, 시작 시간과 기간만 알고 있다고 가정합니다. 다음과 같이 쿼리를 다시 작성할 수 있습니다.

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>시간 단위 변환
datetime 또는 timespan을 기본값 이외의 시간 단위로 표시할 수도 있습니다. 예를 들어 지난 30분 동안의 오류 이벤트를 검토하고 있으며 얼마나 오래전에 이벤트가 발생했는지를 보여 주는 계산 열이 필요한 경우가 있습니다.

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

열에는 `timeAgo` "00:09:31.5118992"와 같은 값이 포함 됩니다. 즉, hh: mm: ss. fffffff로 형식이 지정 됩니다. 이러한 값을 시작 시간 이후에 경과한 분의 `numver` 형식으로 지정하려면 해당 값을 “1분”으로 나눕니다.

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>시간 간격에 따른 집계 및 버킷팅
또 다른 일반적인 시나리오는 특정 기간에 특정 시간 조직으로 통계를 얻어야 하는 경우입니다. 이 시나리오에서는 `bin` 연산자를 summarize 절의 일부로 사용할 수 있습니다.

다음 쿼리를 사용하여 마지막 30분 동안 5분 간격으로 발생한 이벤트의 수를 가져옵니다.

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

이 쿼리는 다음 테이블을 생성합니다.  

|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

결과의 버킷을 만드는 또 다른 방법은 `startofday`와 같은 함수를 사용하는 것입니다.

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

이 쿼리는 다음 결과를 생성합니다.

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7,136|
|2018-07-29T00:00:00.000|12,315|
|2018-07-30T00:00:00.000|16,847|
|2018-07-31T00:00:00.000|12,616|
|2018-08-01T00:00:00.000|5,416|


## <a name="time-zones"></a>표준 시간대
모든 날짜/시간 값은 UTC로 표시되므로 이러한 값을 현지 표준 시간대로 변환하는 것이 유용한 경우도 많습니다. 예를 들어이 계산을 사용하여 UTC에서 PST 시간으로 변환하려면 다음을 입력합니다.

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>관련 함수

| Category | 기능 |
|:---|:---|
| 데이터 형식 변환 | [todatetime](/azure/kusto/query/todatetimefunction)  [totimespan](/azure/kusto/query/totimespanfunction)  |
| 값을 bin 크기로 반올림 | [저장소](/azure/kusto/query/binfunction) |
| 특정 날짜 또는 시간 가져오기 | [ago](/azure/kusto/query/agofunction) [now](/azure/kusto/query/nowfunction)   |
| 값의 부분 가져오기 | [datetime_part](/azure/kusto/query/datetime-partfunction) [getmonth](/azure/kusto/query/getmonthfunction) [monthofyear](/azure/kusto/query/monthofyearfunction) [getyear](/azure/kusto/query/getyearfunction) [dayofmonth](/azure/kusto/query/dayofmonthfunction) [dayofweek](/azure/kusto/query/dayofweekfunction) [dayofyear](/azure/kusto/query/dayofyearfunction) [weekofyear](/azure/kusto/query/weekofyearfunction) |
| 상대 날짜 값 가져오기  | [endofday](/azure/kusto/query/endofdayfunction) [endofweek](/azure/kusto/query/endofweekfunction) [endofmonth](/azure/kusto/query/endofmonthfunction) [endofyear](/azure/kusto/query/endofyearfunction) [startofday](/azure/kusto/query/startofdayfunction) [startofweek](/azure/kusto/query/startofweekfunction) [startofmonth](/azure/kusto/query/startofmonthfunction) [startofyear](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>다음 단계
Azure Monitor 로그 데이터에 [Kusto 쿼리 언어](/azure/kusto/query/)를 사용하는 방법에 대한 다른 단원을 참조하세요.

- [문자열 작업](string-operations.md)
- [집계 함수](aggregations.md)
- [고급 집계](advanced-aggregations.md)
- [JSON 및 데이터 구조](json-data-structures.md)
- [고급 쿼리 작성](advanced-query-writing.md)
- [조인](joins.md)
- [차트](charts.md)
