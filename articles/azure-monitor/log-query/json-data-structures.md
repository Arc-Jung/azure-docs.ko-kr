---
title: Azure Monitor 로그 쿼리에서 문자열 작업 | Microsoft Docs
description: 이 문서에서는 Azure Portal에서 Azure Monitor Log Analytics를 사용 하 여 Azure Monitor의 로그 데이터를 쿼리하고 분석 하는 방법에 대 한 자습서를 제공 합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: f792820b7b0dff20e647031410ba87ac26c2495a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80672964"
---
# <a name="working-with-json-and-data-structures-in-azure-monitor-log-queries"></a>Azure Monitor 로그 쿼리에서 JSON 및 데이터 구조 사용

> [!NOTE]
> 이 단원을 완료 하기 전에 [Azure Monitor Log Analytics 시작](get-started-portal.md) 을 완료 하 고 [Azure Monitor 로그 쿼리를 시작](get-started-queries.md) 해야 합니다.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

중첩된 개체는 배열 또는 키-값 쌍 맵에 다른 개체를 포함하는 개체입니다. 이러한 개체는 JSON 문자열로 표시됩니다. 이 문서에서는 JSON이 데이터를 검색하고 중첩된 개체를 분석하는 데 사용되는 방법을 설명합니다.

## <a name="working-with-json-strings"></a>JSON 문자열 사용
알려진 경로의 특정 JSON 요소에 액세스하려면 `extractjson`을 사용합니다. 이 함수에는 다음 규칙을 사용하는 경로 식이 필요합니다.

- _$_ 루트 폴더를 참조 하려면
- 다음 예제와 같이 인덱스 및 요소를 나타내려면 대괄호 또는 점 표기법을 사용합니다.


인덱스에는 대괄호를 사용하고 요소를 구분할 때는 점을 사용합니다.

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

다음은 대괄호 표기법만 사용할 때와 같은 결과를 나타냅니다.

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

요소가 하나만 있으면 점 표기법만 사용해도 됩니다.

```Kusto
let hosts_report=dynamic({"location":"North_DC", "status":"running", "rate":5});
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>개체 작업

### <a name="parsejson"></a>parsejson
JSON 구조에서 여러 요소에 액세스하려면 동적 개체로 액세스하는 것이 더 쉽습니다. 텍스트 데이터를 동적 개체로 캐스팅하려면 `parsejson`을 사용합니다. 동적 형식으로 변환되면 추가 함수를 사용하여 데이터를 분석할 수 있습니다.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
배열 내의 요소 수를 계산하려면 `arraylength`를 사용합니다.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
개체의 속성을 별도 행으로 구분하려면 `mvexpand`를 사용합니다.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
개체의 모든 값을 허용하는 스키마를 가져오려면 `buildschema`를 사용합니다.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

출력은 JSON 형식의 스키마입니다.
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
이 출력은 개체 필드의 이름과 일치하는 데이터 형식을 설명합니다. 

중첩된 개체는 다음 예제와 같이 스키마가 서로 다를 수 있습니다.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![스키마 빌드](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>다음 단계
Azure Monitor에서 로그 쿼리 사용에 대한 다른 단원을 참조하세요.

- [문자열 작업](string-operations.md)
- [날짜 및 시간 작업](datetime-operations.md)
- [집계 함수](aggregations.md)
- [고급 집계](advanced-aggregations.md)
- [고급 쿼리 작성](advanced-query-writing.md)
- [조인](joins.md)
- [차트](charts.md)
