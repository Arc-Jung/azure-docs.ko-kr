---
title: 느리게 쿼리 로그-Azure Database for MariaDB
description: Azure Database for MariaDB에서 사용할 수 있는 로그와, 다양한 로깅 수준을 활성화하는 데 사용할 수 있는 매개 변수에 대해 설명합니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: b38838c20e4ab18b64cabcb2749ec39163f1b52d
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76515057"
---
# <a name="slow-query-logs-in-azure-database-for-mariadb"></a>Azure Database for MariaDB의 저속 쿼리 로그
Azure Database for MariaDB에서는 사용자에게 느린 쿼리 로그를 제공합니다. 트랜잭션 로그에 대한 액세스는 지원되지 않습니다. 느린 쿼리 로그를 사용하여 문제 해결을 위한 성능 병목을 파악할 수 있습니다.

느린 쿼리 로그에 대한 자세한 내용은 MariaDB 설명서에서 [느린 쿼리 로그](https://mariadb.com/kb/en/library/slow-query-log-overview/)를 참조하세요.

## <a name="access-slow-query-logs"></a>저속 쿼리 로그 액세스
Azure Portal 및 Azure CLI를 사용 하 여 Azure Database for MariaDB 느리게 쿼리 로그를 나열 하 고 다운로드할 수 있습니다.

Azure Portal에서 Azure Database for MariaDB 서버를 찾습니다. **모니터링** 머리글 아레에서 **서버 로그** 페이지를 선택합니다.

Azure CLI에 대한 자세한 내용은 [Azure CLI를 사용한 서버 로그 구성 및 액세스](howto-configure-server-logs-cli.md)를 참조하세요.

마찬가지로 진단 로그를 사용 하 여 Azure Monitor로 로그를 파이프 할 수 있습니다. 자세한 내용은 [아래](concepts-server-logs.md#diagnostic-logs) 를 참조 하세요.

## <a name="log-retention"></a>로그 보존
로그는 생성 시점에서 최대 7일까지 사용할 수 있습니다. 사용 가능한 로그의 전체 크기가 7GB를 초과하면 여유 공간이 생길 때까지 가장 오래된 파일이 삭제됩니다.

즉, 24시간이 지나거나 전체 크기가 7GB를 초과할 때마다(먼저 해당되는 쪽) 로그가 가장 오래된 파일부터 삭제됩니다.

## <a name="configure-slow-query-logging"></a>저속 쿼리 로깅 구성
기본적으로 느린 쿼리 로그는 비활성화됩니다. 활성화하려면 slow_query_log를 ON으로 설정합니다.

조정할 수 있는 다른 매개 변수는 다음과 같습니다.

- **long_query_time**: 쿼리가 기록되는 long_query_time(초)보다 쿼리가 오래 걸릴 경우 기본값은 10초입니다.
- **log_slow_admin_statements**: ON에 slow_query_log에 쓰여진 문에서 ALTER_TABLE 및 ANALYZE_TABLE 등과 같은 관리 문이 포함된 경우
- **log_queries_not_using_indexes**: 인덱스를 사용하지 않는 쿼리가 slow_query_log에 기록되는지 여부를 결정합니다.
- **log_throttle_queries_not_using_indexes**:이 매개 변수는 느린 쿼리 로그에 쓸 수 있는 비 인덱스 쿼리의 수 한도를 결정합니다. 이 매개 변수는 log_queries_not_using_indexes가 ON으로 설정된 경우 적용됩니다.
- **log_output**: "File" 이면 저속 쿼리 로그가 로컬 서버 저장소에 기록 되 고 진단 로그를 Azure Monitor 수 있습니다. "None" 인 경우 저속 쿼리 로그는 Azure Monitor 진단 로그에만 기록 됩니다. 

> [!IMPORTANT]
> 테이블이 인덱싱되지 않은 경우에는 인덱싱되지 않은 이러한 테이블에 대해 실행 되는 모든 쿼리가 저속 쿼리 로그에 기록 되기 때문에 `log_queries_not_using_indexes` 및 `log_throttle_queries_not_using_indexes` 매개 변수를 ON으로 설정 하면 MariaDB 성능에 영향을 줄 수 있습니다.<br><br>
> 오랜 시간 동안 느리게 쿼리를 로깅할 계획인 경우 `log_output`를 "없음"으로 설정 하는 것이 좋습니다. "File"로 설정 된 경우 이러한 로그는 로컬 서버 저장소에 기록 되 고, MariaDB 성능에 영향을 줄 수 있습니다. 

느린 쿼리 로그 매개 변수의 전체 설명은 MariaDB [느린 쿼리 로그 설명서](https://mariadb.com/kb/en/library/slow-query-log-overview/)를 참조하세요.

## <a name="diagnostic-logs"></a>진단 로그
Azure Database for MariaDB은 Azure Monitor 진단 로그와 통합 됩니다. Aadb 서버에서 느리게 쿼리 로그를 사용 하도록 설정 하면 로그, Event Hubs 또는 Azure Storage를 Azure Monitor 하도록 선택할 수 있습니다. 진단 로그를 사용하도록 설정하는 방법에 대한 자세한 내용은 [진단 로그 설명서](../azure-monitor/platform/platform-logs-overview.md)의 방법 섹션을 참조하세요.

> [!IMPORTANT]
> 이 서버 로그에 대한 진단 기능은 범용 및 메모리 최적화 [가격 책정 계층](concepts-pricing-tiers.md)에서만 사용할 수 있습니다.

아래 표에는 각 로그의 내용에 대한 설명이 나와 있습니다. 포함되는 필드와 이러한 필드가 표시되는 순서는 출력 방법에 따라 달라질 수 있습니다.

| **속성** | **설명** |
|---|---|
| `TenantId` | 테넌트 ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | UTC에 로그가 기록된 때의 타임스탬프 |
| `Type` | 로그의 형식 항상 `AzureDiagnostics` |
| `SubscriptionId` | 서버가 속한 구독의 GUID |
| `ResourceGroup` | 서버가 속한 리소스 그룹의 이름 |
| `ResourceProvider` | 리소스 공급자의 이름. 항상 `MICROSOFT.DBFORMARIADB` |
| `ResourceType` | `Servers` |
| `ResourceId` | 리소스 URI |
| `Resource` | 서버의 이름 |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | 서버의 이름 |
| `start_time_t` [UTC] | 쿼리가 시작된 시간 |
| `query_time_s` | 쿼리를 실행하는 데 걸린 총 시간 |
| `lock_time_s` | 쿼리가 잠긴 총 시간 |
| `user_host_s` | 사용자 이름 |
| `rows_sent_s` | 전송된 행 수 |
| `rows_examined_s` | 검사된 행 수 |
| `last_insert_id_s` | [last_insert_id](https://mariadb.com/kb/en/library/last_insert_id/) |
| `insert_id_s` | ID 삽입 |
| `sql_text_s` | 전체 쿼리 |
| `server_id_s` | 서버 ID입니다. |
| `thread_id_s` | 스레드 ID |
| `\_ResourceId` | 리소스 URI |

## <a name="analyze-logs-in-azure-monitor-logs"></a>Azure Monitor 로그의 로그 분석

저속 쿼리 로그가 진단 로그를 통해 Azure Monitor 로그에 전달 되 면 속도가 느려지는 쿼리를 추가로 분석할 수 있습니다. 다음은 시작 하는 데 도움이 되는 몇 가지 샘플 쿼리입니다. 아래를 서버 이름으로 업데이트 해야 합니다.

- 특정 서버에서 10 초 보다 긴 쿼리

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- 특정 서버에 가장 긴 쿼리 5 개 나열

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- 특정 서버에서 최소, 최대, 평균 및 표준 편차 쿼리 시간을 기준으로 느리게 쿼리를 요약 합니다.

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- 특정 서버에서 느리게 쿼리를 배포 하는 그래프

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- 진단 로그가 사용 하도록 설정 된 모든 Badb 서버에서 10 초 보다 긴 쿼리를 표시 합니다.

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>다음 단계
- [Azure Portal에서 느리게 쿼리 로그를 구성 하는 방법](howto-configure-server-logs-portal.md)
- [Azure CLI에서 느리게 쿼리 로그를 구성 하는 방법](howto-configure-server-logs-cli.md)
