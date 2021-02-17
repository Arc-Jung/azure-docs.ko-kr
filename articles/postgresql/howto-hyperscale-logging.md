---
title: Logs-Hyperscale (Citus)-Azure Database for PostgreSQL
description: Azure Database for PostgreSQL-Hyperscale (Citus)에 대 한 데이터베이스 로그에 액세스 하는 방법
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: ca3cc2873fbc6db72b10c80daecddf1471e30ff4
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100577064"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Azure Database for PostgreSQL-Hyperscale (Citus)의 로그

PostgreSQL 로그는 Citus (Hyperscale) 서버 그룹의 모든 노드에서 사용할 수 있습니다. 저장소 서버 또는 분석 서비스에 로그를 제공할 수 있습니다. 로그를 사용 하 여 구성 오류를 식별 하 고, 문제를 해결 하 고, 문제를 해결할 수 있습니다.

## <a name="accessing-logs"></a>로그 액세스

Citus (Hyperscale) 코디네이터 또는 작업자 노드에 대 한 PostgreSQL 로그에 액세스 하려면 Azure Portal에서 노드를 엽니다.

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="노드 목록":::

선택한 노드에 대해 **진단 설정** 을 열고 **+ 진단 설정 추가** 를 클릭 합니다.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="진단 설정 추가 단추":::

새 진단 설정의 이름을 선택 하 고 **PostgreSQLLogs** 상자를 선택 합니다.  로그를 받을 대상을 선택 합니다.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="PostgreSQL 로그 선택":::

## <a name="next-steps"></a>다음 단계

- [Log analytics 쿼리 시작](../azure-monitor/logs/log-analytics-tutorial.md)
- [Azure event hubs](../event-hubs/event-hubs-about.md) 에 대 한 자세한 정보