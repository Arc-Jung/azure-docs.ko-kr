---
title: Azure Portal에서 Azure Database for MariaDB에 대 한 서버 로그 구성 및 액세스
description: 이 문서에서는 Azure Portal에서 Azure Database for MariaDB의 서버 로그를 구성 하 고 액세스 하는 방법을 설명 합니다.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c8be9519d3393330b3022fadd2de6a49e58ecdcf
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703507"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Azure Portal에서 서버 로그 구성 및 액세스

Azure Portal에서 [Azure Database for MariaDB 저속 쿼리 로그](concepts-server-logs.md) 를 구성, 나열 및 다운로드할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항
이 방법 가이드를 단계별로 실행하려면 다음이 필요합니다.
- [Azure Database for MariaDB 서버](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>로깅 구성
느린 쿼리 로그에 대한 액세스를 구성합니다. 

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.

2. Azure Database for MariaDB 서버를 선택합니다.

3. 사이드바의 **모니터링** 섹션에서 **서버 로그**를 선택합니다. 
   ![서버 로그 선택, 클릭하여 구성](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. 서버 매개 변수를 보려면 제목 **로그를 사용하도록 설정하고 로그 매개 변수를 구성하려면 여기를 클릭합니다**를 선택합니다.

5. "slow_query_log"를 "ON"으로 설정하는 것을 포함하여 조정해야 하는 매개 변수를 변경합니다. 이 세션에서 변경할 내용은 모두 자주색으로 강조 표시되어 있습니다. 

   매개 변수를 변경한 다음 **저장**을 클릭합니다. 또는 변경 사항을 **취소**할 수 있습니다.

   ![저장 또는 제거 클릭](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. **서버 매개 변수** 페이지에서 **닫기 단추**(X 아이콘)를 클릭하여 로그 목록으로 돌아갑니다.

## <a name="view-list-and-download-logs"></a>목록 보기 및 로그 다운로드
로깅이 시작 되 면 사용 가능한 쿼리 로그 목록을 보고 서버 로그 창에서 개별 로그 파일을 다운로드할 수 있습니다. 

1. Azure Portal을 엽니다.

2. Azure Database for MariaDB 서버를 선택합니다.

3. 사이드바의 **모니터링** 섹션에서 **서버 로그**를 선택합니다. 이 페이지에는 다음과 같이 로그 파일 목록이 표시됩니다.

   ![로그 목록](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > 로그의 명명 규칙은 **mysql-slow-< your server name>-yyyymmddhh.log**입니다. 파일 이름에 사용된 날짜와 시간은 로그가 발행된 시간입니다. 로그 파일은 24시간마다 또는 7.5GB마다 회전됩니다(먼저 해당되는 쪽).

4. 필요한 경우 **검색 상자**를 사용하여 날짜/시간을 기준으로 특정 로그로 신속하게 범위를 좁힙니다. 로그의 이름이 검색됩니다.

5. 다음과 같이 테이블 행의 각 로그 파일 옆에 있는 **다운로드** 단추(아래쪽 화살표 아이콘)를 사용하여 개별 로그 파일을 다운로드합니다.

   ![다운로드 아이콘을 클릭합니다.](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>진단 로그 설정

1. 사이드바의 **모니터링** 섹션에서 **진단 설정**을 선택 합니다.

1. "+ 진단 설정 추가" ![ 추가 진단 설정 @ no__t-1을 클릭 합니다.

1. 진단 설정 이름을 제공 합니다.

1. 저속 쿼리 로그를 보낼 데이터 싱크 (저장소 계정, 이벤트 허브 및/또는 Log Analytics 작업 영역)를 지정 합니다.

1. 로그 형식으로 "MySqlSlowLogs"을 선택 합니다.
![ 진단 설정 구성 @ no__t-1

1. 저속 쿼리 로그를 파이프 하도록 데이터 싱크를 구성한 후에는 **저장**을 클릭할 수 있습니다.
![ 진단 설정 저장 @ no__t-1

1. 구성 된 데이터 싱크에서 탐색 하 여 느리게 쿼리 로그에 액세스 합니다. 로그가 표시 되는 데 최대 10 분이 소요 될 수 있습니다.

## <a name="next-steps"></a>다음 단계
- 느리게 쿼리 로그를 프로그래밍 방식으로 다운로드 하는 방법을 알아보려면 [CLI에서 느리게 쿼리 로그 액세스](howto-configure-server-logs-cli.md) 를 참조 하세요.
- Azure Database for MariaDB의 [저속 쿼리 로그](concepts-server-logs.md) 에 대해 자세히 알아보세요.
- 매개 변수 정의 및 로깅에 대한 자세한 내용은 [로그](https://mariadb.com/kb/en/library/slow-query-log-overview/)의 MariaDB 설명서를 참조하세요.