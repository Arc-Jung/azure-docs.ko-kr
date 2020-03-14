---
title: Azure Stream Analytics에서 쿼리 병렬 처리 및 크기 조정 사용
description: 이 문서에서는 입력 파티션을 구성하고, 쿼리 정의를 조정하고, 작업 스트리밍 단위를 설정하여 Stream Analytics 작업의 크기를 조정하는 방법을 설명합니다.
author: JSeb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: d1afb6037b5fc290de93faba405982ebd1fb68ea
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79254340"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Azure Stream Analytics에서 쿼리 병렬 처리 사용
이 문서에서는 Azure Stream Analytics에서 병렬 처리 기능을 활용하는 방법을 보여 줍니다. 입력 파티션을 구성하고, 분석 쿼리 정의를 조정하여 Stream Analytics 작업의 크기를 조정하는 방법을 알아봅니다.
필수 구성 요소로, [스트리밍 단위 이해 및 조정](stream-analytics-streaming-unit-consumption.md)에 설명된 스트리밍 단위 개념을 잘 알고 있어야 합니다.

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Stream Analytics 작업은 무엇으로 구성되나요?
Stream Analytics 작업 정의에는 입력, 쿼리 및 출력이 포함됩니다. 입력은 작업이 데이터 스트림을 읽는 위치입니다. 쿼리는 데이터 입력 스트림을 변환하는 데 사용되며, 출력은 작업이 작업 결과를 전송하는 위치입니다.

작업에는 데이터 스트림에 대해 하나 이상의 입력 소스가 필요합니다. 데이터 스트림 입력 원본은 Azure 이벤트 허브 또는 Azure Blob Storage에 저장될 수 있습니다. 자세한 내용은 [Azure Stream Analytics 소개](stream-analytics-introduction.md) 및 [Azure Stream Analytics 사용 시작](stream-analytics-real-time-fraud-detection.md)을 참조하세요.

## <a name="partitions-in-sources-and-sinks"></a>원본 및 싱크의 파티션
Stream Analytics 작업 크기 조정은 입력 또는 출력에 있는 파티션을 활용합니다. 분할을 통해 데이터를 파티션 키에 따라 하위 집합으로 분할할 수 있습니다. 데이터를 사용하는 프로세스(예: Streaming Analytics 작업)는 서로 다른 파티션을 병렬로 사용 및 기록하여 처리량을 증가시킬 수 있습니다. 

### <a name="inputs"></a>입력
모든 Azure Stream Analytics 입력은 분할을 사용할 수 있습니다.
-   EventHub(PARTITION BY 키워드로 파티션 키를 명시적으로 설정해야 함)
-   IoT Hub(PARTITION BY 키워드로 파티션 키를 명시적으로 설정해야 함)
-   Blob Storage

### <a name="outputs"></a>outputs

Stream Analytics로 작업할 때 다음 출력에서 분할을 활용할 수 있습니다.
-   Azure Data Lake 스토리지
-   Azure 기능
-   Azure 테이블
-   Blob Storage(파티션 키를 명시적으로 설정할 수 있음)
-   Cosmos DB(파티션 키를 명시적으로 설정해야 함)
-   Event Hubs(파티션 키를 명시적으로 설정해야 함)
-   IoT Hub(파티션 키를 명시적으로 설정해야 함)
-   Service Bus
- 선택적 분할을 사용하는 SQL 및 SQL Data Warehouse: [Azure SQL Database로의 출력 페이지](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf)에서 자세한 내용을 참조하세요.

Power BI는 분할을 지원하지 않습니다. 그러나 [이 섹션](#multi-step-query-with-different-partition-by-values)에 설명된 대로 입력은 여전히 분할할 수 있습니다. 

파티션에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Event Hubs 기능 개요](../event-hubs/event-hubs-features.md#partitions)
* [데이터 분할](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>병렬 처리가 적합한 작업
*병렬 처리가 적합한* 작업은 Azure Stream Analytics에서 가장 확장성이 뛰어난 시나리오입니다. 하나의 쿼리 인스턴스에 대한 하나의 입력 파티션을 하나의 출력 파티션에 연결합니다. 이 병렬 처리에는 다음 요구 사항이 있습니다.

1. 쿼리 논리가 동일한 쿼리 인스턴스에서 처리되는 동일한 키에 따라 다른 경우 이벤트가 동일한 입력 파티션으로 이동하는지 확인해야 합니다. Event Hubs 또는 IoT Hub의 경우 이것은 이벤트 데이터에 **PartitionKey** 값 집합이 있어야 한다는 의미입니다. 또는 분할된 보낸 사람을 사용할 수 있습니다. Blob Storage의 경우 이것은 이벤트가 같은 파티션 폴더에 전송된다는 것을 의미합니다. 쿼리 논리가 동일한 쿼리 인스턴스에서 동일한 키를 처리할 필요가 없는 경우 이 요구 사항을 무시할 수 있습니다. 이 논리에 대한 예로 간단한 선택/프로젝트/필터 쿼리를 참조하세요.  

2. 데이터가 입력 측에 배치되면 쿼리가 분할되었는지 확인해야 합니다. 그러려면 모든 단계에서 **PARTITION BY**를 사용해야 합니다. 여러 단계가 허용되지만 모두 동일한 키로 분할되어야 합니다. 호환성 수준 1.0 및 1.1에서 작업을 완전히 병렬 처리 하려면 분할 키를 **PartitionId** 로 설정 해야 합니다. Compatility 수준이 1.2 이상인 작업의 경우 사용자 지정 열을 입력 설정에서 파티션 키로 지정할 수 있으며 PARTITION BY 절이 없는 경우에도 작업은 자동으로 paralellized 됩니다. 이벤트 허브 출력의 경우 "파티션 키 열" 속성은 "PartitionId"를 사용 하도록 설정 해야 합니다.

3. 대부분의 출력은 분할을 활용할 수 있지만 분할을 지원하지 않는 출력 형식을 사용하는 경우 작업이 완벽하게 병렬 처리되지 않습니다. 자세한 내용은 [출력 섹션](#outputs)을 참조하세요.

4. 입력 파티션 수가 출력 파티션 수와 같아야 합니다. Blob Storage 출력은 파티션을 지원할 수 있고 업스트림 쿼리의 파티션 구성표를 상속합니다. Blob Storage에 대한 파티션 키가 지정되면 데이터는 입력 파티션별로 분할되므로 결과는 여전히 완전 병렬 상태입니다. 다음은 완전한 병렬 작업을 허용하는 파티션 값의 예입니다.

   * 8개의 이벤트 허브 입력 파티션 및 8개의 이벤트 허브 출력 파티션
   * 8개의 이벤트 허브 입력 파티션 및 Blob Storage 출력
   * 임의 카디널리티를 사용하여 사용자 지정 필드별로 분할되는 8개의 이벤트 허브 입력 파티션 및 Blob Storage 출력
   * 8개의 Blob Storage 입력 파티션 및 Blob Storage 출력
   * 8개의 Blob Storage 입력 파티션 및 8개의 이벤트 허브 출력 파티션

다음 섹션에서는 병렬 처리가 적합한 작업의 몇 가지 예제 시나리오를 살펴봅니다.

### <a name="simple-query"></a>단순 쿼리

* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: 8 개의 파티션이 있는 이벤트 허브 ("파티션 키 열"은 "PartitionId"를 사용 하도록 설정 해야 함)

쿼리:

```SQL
    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100
```

이 쿼리는 간단한 필터입니다. 따라서 이벤트 허브로 전송되는 입력을 분할하는 것에 대해 걱정하지 않아도 됩니다. 1\.2 이전의 호환성 수준이 있는 작업에는 **PARTITION By PartitionId** 절이 포함 되어 있으므로 이전에 #2 요구 사항을 충족 해야 합니다. 출력의 경우 파티션 키가 **PartitionId**로 설정되도록 작업에서 이벤트 허브 출력을 구성해야 합니다. 마지막 한 가지 검사는 입력 파티션 수가 출력 파티션 수와 같은지 확인하는 것입니다.

### <a name="query-with-a-grouping-key"></a>그룹화 키가 있는 쿼리

* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: Blob Storage

쿼리:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

이 쿼리에 그룹화 키가 있습니다. 따라서 함께 그룹화된 이벤트를 동일한 이벤트 허브 파티션으로 전송해야 합니다. 이 예제에서는 TollBoothID를 기준으로 그룹화하므로 이벤트가 이벤트 허브로 전송될 때 TollBoothID를 파티션 키로 사용해야 합니다. 그런 후 ASA에서 **PARTITION BY PartitionId**를 사용하여 이 파티션 구성표에서 상속하고 전체 병렬 처리를 사용하도록 설정할 수 있습니다. 출력이 Blob Storage이므로 요구 사항 4번에 따라 파티션 키 값 구성에 대해 걱정할 필요가 없습니다.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>병렬 처리가 적합하지 *않은* 예제 시나리오

이전 섹션에서는 병렬 처리가 적합한 시나리오를 살펴보았습니다. 이 섹션에서는 병렬 처리에 적합하기 위한 모든 요구 사항을 충족하지 않는 시나리오에 대해 설명합니다. 

### <a name="mismatched-partition-count"></a>일치하지 않는 파티션 수
* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: 32개의 파티션이 있는 이벤트 허브

이 경우 쿼리 내용은 중요하지 않습니다. 입력 파티션 수가 출력 파티션 수와 일치하지 않는 토폴로지는 병렬 처리가 적합하지 않습니다. 그렇지만 특정 수준의 병렬 처리는 여전히 사용할 수 있습니다.

### <a name="query-using-non-partitioned-output"></a>분할되지 않은 출력을 사용하여 쿼리
* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: Power BI

Power BI 출력은 현재 분할을 지원하지 않습니다. 따라서 이 시나리오는 병렬 처리가 적합하지 않습니다.

### <a name="multi-step-query-with-different-partition-by-values"></a>서로 다른 PARTITION BY 값이 있는 다중 단계 쿼리
* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: 8개의 파티션이 있는 이벤트 허브

쿼리:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

보이는 것처럼 두 번째 단계에서는 **TollBoothId** 를 분할 키로 사용합니다. 이 단계는 첫 번째 단계와 동일하지 않으므로 순서를 섞어야 합니다. 

앞의 예제에서는 병렬 처리가 적합한 토폴로지를 준수(또는 준수하지 않는) 일부 Stream Analytics 작업을 보여 줍니다. 준수하는 경우 최대 규모의 가능성을 포함합니다. 이러한 프로필 중 하나에 적합하지 않는 작업의 경우 향후 업데이트에서 크기 조정 지침이 제공될 예정입니다. 현재는 다음 섹션에 있는 일반적인 지침을 사용하세요.

### <a name="compatibility-level-12---multi-step-query-with-different-partition-by-values"></a>호환성 수준 1.2-값으로 다른 파티션이 있는 다단계 쿼리 
* 입력: 8개의 파티션이 있는 이벤트 허브
* 출력: 8 개의 파티션이 있는 이벤트 허브 ("파티션 키 열"은 "TollBoothId"를 사용 하도록 설정 해야 함)

쿼리:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

호환성 수준 1.2은 기본적으로 병렬 쿼리 실행을 가능 하 게 합니다. 예를 들어, "TollBoothId" 열이 입력 파티션 키로 설정 되어 있으면 이전 섹션의 쿼리가 parttioned 됩니다. PARTITION BY ParttionId 절은 필요 하지 않습니다.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>작업의 최대 스트리밍 단위 계산
Stream Analytics 작업에 사용될 수 있는 스트리밍 단위의 총 수는 작업에 대해 정의된 쿼리의 단계 수와 각 단계에 대한 파티션 수에 따라 결정됩니다.

### <a name="steps-in-a-query"></a>쿼리의 단계
하나의 쿼리에는 하나 이상의 단계가 있을 수 있습니다. 각 단계는 **WITH** 키워드를 사용하여 정의된 하위 쿼리입니다. **WITH** 키워드(한 개 쿼리만) 밖에 있는 쿼리도 한 단계로 계산됩니다. 예를 들어 다음 쿼리에서 **SELECT** 문입니다.

쿼리:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId
```

이 쿼리에는 2개의 단계가 있습니다.

> [!NOTE]
> 이 쿼리는 문서 뒷부분에서 좀 더 자세히 설명합니다.
>  

### <a name="partition-a-step"></a>단계 분할
단계를 분할하려면 다음 조건이 필요합니다.

* 입력 소스는 분할해야 합니다. 
* 쿼리의 **SELECT** 문은 분할된 입력 원본에서 읽어와야 합니다.
* 단계 내의 쿼리에는 **PARTITION BY** 키워드가 있어야 합니다.

쿼리가 분할되면 입력 이벤트가 처리되고 별도의 파티션 그룹에 집계되며 각 그룹에 대해 출력 이벤트가 생성됩니다. 결합된 집계를 원하는 경우 집계할 또 다른 분할되지 않은 단계를 만들어야 합니다.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>작업의 최대 스트리밍 단위 계산
하나의 Stream Analytics 작업에 대해 분할되지 않은 모든 단계를 최대 6개의 SU(스트리밍 단위)로 확장할 수 있습니다. 이외에 분할 단계에서 각 파티션에 대해 6개의 SU를 추가할 수 있습니다.
아래 표에서 일부 **예제**를 확인할 수 있습니다.

| 쿼리                                               | 작업에 대한 최대 SU |
| --------------------------------------------------- | ------------------- |
| <ul><li>쿼리는 한 단계를 포함합니다.</li><li>이 단계는 분할되지 않습니다.</li></ul> | 6 |
| <ul><li>입력 데이터 스트림은 16으로 분할됩니다.</li><li>쿼리는 한 단계를 포함합니다.</li><li>이 단계는 분할됩니다.</li></ul> | 96(6 * 16개 파티션) |
| <ul><li>쿼리는 두 단계를 포함합니다.</li><li>두 단계모두 분할되지 않습니다.</li></ul> | 6 |
| <ul><li>입력 데이터 스트림은 3으로 분할됩니다.</li><li>쿼리는 두 단계를 포함합니다. 입력 단계는 분할되며 두 번째 단계는 분할되지 않습니다.</li><li><strong>SELECT</strong> 문은 분할된 입력에서 읽어옵니다.</li></ul> | 24(분할된 단계에 대한 18+분할되지 않은 단계에 대한 6) |

### <a name="examples-of-scaling"></a>크기 조정 예제

다음 쿼리는 3분의 기간 내에 세 곳의 요금 징수소가 있는 요금 스테이션을 통과하는 자동차 수를 계산합니다. 이 쿼리는 6개 SU까지 확장될 수 있습니다.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

쿼리에 더 많은 SU를 사용하려면 입력 데이터 스트림 및 쿼리를 모두 분할해야 합니다. 데이터 스트림 파티션이 3으로 설정되므로 다음의 수정된 쿼리를 최대 18개 SU로 확장할 수 있습니다.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

쿼리가 분할되면 입력 이벤트가 처리되고 별도의 파티션 그룹에 집계되며 각 그룹에 대해 출력 이벤트가 생성됩니다. 출력 이벤트도 각 그룹에 대해 생성됩니다. **GROUP BY** 필드가 입력 데이터 스트림의 파티션 키가 아닌 상태에서 분할을 수행하면 예기치 않은 결과가 발생할 수 있습니다. 예를 들어 이전 쿼리의 **TollBoothId** 필드는 **Input1**의 파티션 키가 아닙니다. 따라서 TollBooth #1의 데이터는 여러 파티션으로 분산될 수 있습니다.

각각의 **Input1** 파티션은 Stream Analytics에 의해 별도로 처리됩니다. 결과적으로, 동일한 연속 창에 동일한 요금 징수소에 대한 자동차 수 레코드가 여러 개 생성됩니다. 입력 파티션 키를 변경할 수 없는 경우 다음 예제처럼 비분할 단계를 추가하여 파티션 간의 값을 집계함으로써 이 문제를 해결할 수 있습니다.

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

이 쿼리는 24개 SU까지 확장될 수 있습니다.

> [!NOTE]
> 두 스트림을 조인하는 경우 스트림이 조인을 생성하는 데 사용하는 열의 파티션 키별로 분할되는지 확인합니다. 또한 두 스트림에서 파티션 수가 동일한지도 확인합니다.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>대규모로 더 높은 처리량 달성

[처리가 적합 병렬](#embarrassingly-parallel-jobs) 작업이 필요 하지만 대규모로 더 높은 처리량을 유지 하는 데 충분 하지 않습니다. 모든 저장소 시스템과 해당 Stream Analytics 출력에는 최대한의 쓰기 처리량을 얻는 방법에 대 한 변형이 있습니다. 모든 규모의 시나리오와 마찬가지로 올바른 구성을 사용 하 여 해결할 수 있는 몇 가지 과제가 있습니다. 이 섹션에서는 몇 가지 일반적인 출력에 대 한 구성에 대해 설명 하 고 초당 1K, 5K 및 10K 이벤트의 수집 비율을 유지 하는 샘플을 제공 합니다.

다음 관찰에서는 이벤트 허브, Azure SQL DB 또는 Cosmos DB에 쓰는 기본 JavaScript UDF 인 상태 비저장 (통과) 쿼리를 사용 하는 Stream Analytics 작업을 사용 합니다.

#### <a name="event-hub"></a>이벤트 허브

|수집 률 (초당 이벤트 수) | 스트리밍 단위 | 출력 리소스  |
|--------|---------|---------|
| 1K     |    1    |  2 TU   |
| 5K     |    6    |  6 TU   |
| 10K    |    12   |  10 TU  |

[이벤트 허브](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs) 솔루션은 SU (스트리밍 단위) 및 처리량 측면에서 선형적으로 확장 되므로 Stream Analytics에서 데이터를 분석 하 고 스트리밍할 수 있는 가장 효율적이 고 성능이 뛰어난 방법입니다. 작업은 최대 200 m b/초 또는 하루 19조 이벤트를 처리 하는 데 약 192 SU로 확장 될 수 있습니다.

#### <a name="azure-sql"></a>Azure SQL
|수집 률 (초당 이벤트 수) | 스트리밍 단위 | 출력 리소스  |
|---------|------|-------|
|    1K   |   3  |  S3   |
|    5K   |   18 |  P4   |
|    10K  |   36 |  P6   |

[AZURE SQL](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) 은 분할 상속 이라는 병렬 작성을 지원 하지만 기본적으로 사용 하도록 설정 되어 있지 않습니다. 그러나 완전히 병렬 쿼리와 함께 상속 분할을 사용 하도록 설정 하는 것 만으로는 처리량이 높아질 수 있습니다. SQL 쓰기 처리량은 SQL Azure 데이터베이스 구성 및 테이블 스키마에 따라 크게 달라 집니다. [SQL 출력 성능](./stream-analytics-sql-output-perf.md) 문서에는 쓰기 처리량을 최대화할 수 있는 매개 변수에 대 한 자세한 정보가 있습니다. [Azure SQL Database Azure Stream Analytics 출력](./stream-analytics-sql-output-perf.md#azure-stream-analytics) 에 설명 된 것 처럼이 솔루션은 8 개의 파티션을 초과 하는 완전히 병렬 파이프라인으로 선형으로 확장 되지 않으므로 SQL 출력 전에 다시 분할 해야 할 수 있습니다 [(참조)](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count). 프리미엄 Sku는 몇 분 마다 발생 하는 로그 백업에서 발생 하는 오버 헤드와 함께 높은 IO 비율을 유지 하는 데 필요 합니다.

#### <a name="cosmos-db"></a>Cosmos DB
|수집 률 (초당 이벤트 수) | 스트리밍 단위 | 출력 리소스  |
|-------|-------|---------|
|  1K   |  3    | 20K  |
|  5K   |  24   | 60K 이상  |
|  10K  |  48   | 120K 이상 |

Stream Analytics의 [Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb) 출력이 [호환성 수준 1.2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12)에서 네이티브 통합을 사용 하도록 업데이트 되었습니다. 호환성 수준 1.2을 사용 하면 처리량이 크게 향상 되 고 새 작업에 대 한 기본 호환성 수준인 1.1에 비해 사용이 감소 합니다. 솔루션은/deviceId에서 분할 된 CosmosDB 컨테이너를 사용 하 고 나머지 솔루션은 동일 하 게 구성 됩니다.

[Azure 샘플 규모의 모든 스트리밍은](https://github.com/Azure-Samples/streaming-at-scale) 부하를 시뮬레이션 하 여 테스트 클라이언트를 입력으로 사용 하 여 공급 되는 이벤트 허브를 사용 합니다. 각 입력 이벤트는 구성 된 수집 속도를 처리량 속도 (1MB/s, 5MB/s 및 10MB)로 쉽게 변환 하는 1KB JSON 문서입니다. 이벤트는 최대 1K 장치에 대해 다음과 같은 JSON 데이터를 전송 하는 IoT 장치를 시뮬레이션 합니다.

```
{
    "eventId": "b81d241f-5187-40b0-ab2a-940faf9757c0",
    "complexData": {
        "moreData0": 51.3068118685458,
        "moreData22": 45.34076957651598
    },
    "value": 49.02278128887753,
    "deviceId": "contoso://device-id-1554",
    "type": "CO2",
    "createdAt": "2019-05-16T17:16:40.000003Z"
}
```

> [!NOTE]
> 솔루션에서 사용 되는 다양 한 구성 요소 때문에 구성이 변경 될 수 있습니다. 보다 정확한 예측을 위해 시나리오에 맞게 예제를 사용자 지정 합니다.

### <a name="identifying-bottlenecks"></a>병목 상태 식별

Azure Stream Analytics 작업의 메트릭 창을 사용 하 여 파이프라인의 병목 상태를 식별할 수 있습니다. 처리량 및 ["워터 마크 지연"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) 또는 **백로그 이벤트** 에 대 한 **입/출력 이벤트** 를 검토 하 여 작업이 입력 속도를 유지 하는지 확인 합니다. 이벤트 허브 메트릭에 대해 **제한 된 요청** 을 찾아 임계값 단위를 적절 하 게 조정 합니다. Cosmos DB 메트릭은 처리량에서 **파티션 키 범위별 최대** 사용량을 검토 하 여 파티션 키 범위를 균등 하 게 사용 하는지 확인 합니다. Azure SQL DB의 경우 **로그 IO** 및 **CPU**를 모니터링 합니다.

## <a name="get-help"></a>도움말 보기

추가 지원이 필요한 경우 [Azure Stream Analytics 포럼](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)을 참조하세요.

## <a name="next-steps"></a>다음 단계
* [Azure Stream Analytics 소개](stream-analytics-introduction.md)
* [Azure Stream Analytics 사용 시작](stream-analytics-real-time-fraud-detection.md)
* [Azure  Stream Analytics 쿼리 언어 참조](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Stream Analytics 관리 REST API 참조](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

