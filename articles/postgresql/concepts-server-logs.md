---
title: PostgreSQL-단일 서버용 Azure Database에서의 서버 로그
description: 이 문서에서는 PostgreSQL-단일 서버용 Azure 데이터베이스를 설명합니다. 쿼리 및 오류 로그를 생성하고 로그 보존을 구성하는 방법을 설명합니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 17083029f2377037b99abfa3ce8371661eccb957
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029981"
---
# <a name="server-logs-in-azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL의 서버 로그-단일 서버
Azure Database for PostgreSQL에서는 쿼리 및 오류 로그를 생성합니다. 쿼리 및 오류 로그는 구성 오류 및 최적 상태가 아닌 성능 문제를 식별하고, 문제를 해결하고, 복구하는 데 사용될 수 있습니다. 단, 트랜잭션 로그 액세스 권한은 포함되지 않습니다. 

## <a name="configure-logging"></a>로깅 구성 
로깅 서버 매개 변수를 사용하면 서버에서 로깅을 구성할 수 있습니다. 각 새 서버의 **log_checkpoints** 및 **log_connections**는 기본적으로 설정되어 있습니다. 로깅 요구에 맞게 매개 변수를 추가로 조정할 수도 있습니다. 

![Azure Database for PostgreSQL - 로깅 매개 변수](./media/concepts-server-logs/log-parameters.png)

이러한 매개 변수에 대한 자세한 내용은 PostgreSQL의 [오류 보고 및 로깅](https://www.postgresql.org/docs/current/static/runtime-config-logging.html) 설명서를 참조하세요. Azure Database for PostgreSQL 매개 변수를 구성하는 방법을 알아보려면 [포털 설명서](howto-configure-server-parameters-using-portal.md) 또는 [CLI 설명서](howto-configure-server-parameters-using-cli.md)를 참조하세요.

## <a name="access-server-logs-through-portal-or-cli"></a>포털 또는 CLI를 통해 서버 로그 액세스
로그를 사용하도록 설정한 경우 [Azure Portal](howto-configure-server-logs-in-portal.md), [Azure CLI](howto-configure-server-logs-using-cli.md) 및 Azure REST API를 사용하여 Azure Database for PostgreSQL 로그 스토리지에서 로그에 액세스할 수 있습니다. 로그 파일은 1시간마다 또는 100MB 크기가 될 때마다(둘 중 먼저 도달할 때) 순환됩니다. 서버에 연결된 **log\_retention\_period** 매개 변수를 사용하여 이 로그 스토리지의 보존 기간을 설정할 수 있습니다. 기본값은 3일이며 최대값은 7일입니다. 로그 파일을 저장하기에 충분한 스토리지를 서버에 할당해야 합니다. Azure 진단 로그는 이 보존 매개 변수를 통해 제어되지 않습니다.


## <a name="diagnostic-logs"></a>진단 로그
Azure Database for PostgreSQL은 Azure Monitor 진단 로그와 통합됩니다. PostgreSQL 서버에 로그를 사용 설정한 후에 [Azure Monitor 로그](../azure-monitor/log-query/log-query-overview.md), Event Hubs 또는 Azure 저장소에 내보내도록 선택할 수 있습니다. 

> [!IMPORTANT]
> 이 서버 로그에 대한 진단 기능은 범용 및 메모리 최적화 [가격 책정 계층](concepts-pricing-tiers.md)에서만 사용할 수 있습니다.

Azure Portal를 사용 하 여 진단 로그를 사용 하도록 설정 하려면

   1. 포털에서 Postgres server의 탐색 메뉴에 있는 *진단 설정* 으로 이동 합니다.
   2. *진단 설정 추가*를 선택 합니다.
   3. 이 설정의 이름을로 설정 합니다. 
   4. 기본 다운스트림 위치 (저장소 계정, 이벤트 허브, log analytics)를 선택 합니다. 
   5. 원하는 데이터 형식을 선택 합니다.
   6. 설정을 저장합니다.

다음 표에서는 각 로그의 내용에 대해 설명 합니다. 포함되는 필드와 이러한 필드가 표시되는 순서는 선택한 출력 엔드포인트에 따라 달라질 수 있습니다. 

|**필드** | **설명** |
|---|---|
| TenantId | 테넌트 ID |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | UTC에 로그가 기록된 때의 타임스탬프 |
| Type | 로그의 형식 항상 `AzureDiagnostics` |
| SubscriptionId | 서버가 속한 구독의 GUID |
| ResourceGroup | 서버가 속한 리소스 그룹의 이름 |
| ResourceProvider | 리소스 공급자의 이름. 항상 `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | 리소스 URI |
| Resource | 서버의 이름 |
| Category | `PostgreSQLLogs` |
| OperationName | `LogEvent` |
| errorLevel | 로깅 수준(예: LOG, ERROR, NOTICE) |
| Message | 기본 로그 메시지 | 
| Domain | 서버 버전(예: postgres-10) |
| Detail | 보조 로그 메시지(해당하는 경우) |
| ColumnName | 열 이름(해당하는 경우) |
| SchemaName | 스키마 이름(해당하는 경우) |
| DatatypeName | 데이터 형식 이름(해당하는 경우) |
| LogicalServerName | 서버의 이름 | 
| _ResourceId | 리소스 URI |
| Prefix | 로그 줄의 접두사 |



## <a name="next-steps"></a>다음 단계
- [Azure Portal](howto-configure-server-logs-in-portal.md) 또는 [Azure CLI](howto-configure-server-logs-using-cli.md)에서 로그에 액세스하는 방법에 대해 자세히 알아봅니다.
- [Azure Monitor 가격](https://azure.microsoft.com/pricing/details/monitor/)에 대해 자세히 알아봅니다.
