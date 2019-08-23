---
title: 단일 서버-PostgreSQL용 Azure Database에서의 성능 권장 사항
description: 이 문서에서는 PostgreSQL-단일 서버용 Azure Database에서의 성능 권장 사항 기능을 설명합니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: e1e9e998c2ac4695d955a546d0f02fbc2b517d5e
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69907485"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>PostgreSQL-단일 서버용 Azure Database에서의 성능 권장 사항

**적용 대상:** Azure Database for PostgreSQL-단일 서버 9.6 및 10

성능 권장 사항 기능은 향상된 성능을 위해 사용자 지정된 제안을 만들기 위해 데이터베이스를 분석합니다. 권장 사항을 생성하기 위해, 해당 분석은 스키마를 포함하여 다양한 데이터베이스 특성을 살펴봅니다. 성능 권장 사항 기능을 완벽하게 활용하려면 서버의 [쿼리 저장소](concepts-query-store.md)를 사용 설정합니다. 성능 권장 사항을 구현한 후, 이러한 변경의 영향을 평가하는 성능을 테스트 해야 합니다. 

## <a name="permissions"></a>사용 권한
성능 권장 사항 기능을 사용하여 분석을 실행하는 데는 **소유자** 또는 **참가자** 권한이 필요합니다.

## <a name="performance-recommendations"></a>성능 권장 사항
[성능 권장 사항](concepts-performance-recommendations.md) 기능은 서버 전체의 워크로드를 분석하여 성능 향상 가능성이 있는 인덱스를 식별합니다.

PostgreSQL 서버용 Azure 포털 페이지에서 메뉴 표시줄의 **지능형 성능** 섹션에서 **성능 권장 사항**을 선택합니다.

![성능 권장 사항 방문 페이지](./media/concepts-performance-recommendations/performance-recommendations-page.png)

분석을 시작할 데이터베이스를 선택하고 **분석**을 선택합니다. 워크로드에 따라 분석을 완료하려면 몇 분 정도 걸릴 수 있습니다. 분석이 완료되면 포털에 알림이 표시됩니다. 분석은 데이터베이스에 대 한 심층 검사를 수행 합니다. 사용량이 적은 기간 동안 분석을 수행하는 것이 좋습니다. 

권장 사항이 발견된 경우 **권장 사항** 창에 해당 목록이 표시됩니다.

![성능 권장 사항 새 페이지](./media/concepts-performance-recommendations/performance-recommendations-result.png)

권장 사항은 자동으로 적용되지 않습니다. 권장 사항을 적용할 경우, 쿼리 텍스트를 복사하고 원하는 클라이언트에서 실행합니다. 권장 구성 평가를 모니터링 및 테스트해야 합니다. 

## <a name="recommendation-types"></a>권장 사항 유형

현재 지원 되는 두 가지 유형의 권장 사항은 다음과 같습니다. *인덱스* 를 만들고 *인덱스를 삭제*합니다.

### <a name="create-index-recommendations"></a>인덱스 만들기 권장 사항
*인덱스 만들기* 권장 사항은 워크로드에서 가장 자주 실행하거나 시간이 많이 걸리는 쿼리의 속도 향상을 위해 새 인덱스를 제안합니다. 이 권장 사항 유형은 [쿼리 저장소](concepts-query-store.md) 사용 설정을 필요로 합니다. 쿼리 저장소는 쿼리 정보를 수집하고 그 분석이 권장 사항을 작성하는 데 사용하는 자세한 쿼리 빈도 및 런타임 통계를 제공합니다.

### <a name="drop-index-recommendations"></a>인덱스 삭제 권장 사항
PostgreSQL용 Azure Database는 누락된 인덱스를 검색하는 것 외에도 기존 인덱스의 성능을 분석합니다. 인덱스가 드물게 사용되거나 중복인 경우, 분석기는 해당 인덱스를 삭제하도록 권장합니다.

## <a name="considerations"></a>고려 사항
* [읽기 복제본](concepts-read-replicas.md)에는 성능 권장 사항을 사용할 수 없습니다.
## <a name="next-steps"></a>다음 단계
- Azure Database for PostgreSQL의 [모니터링 및 튜닝](concepts-monitoring.md)에 대해 자세히 알아보세요.

