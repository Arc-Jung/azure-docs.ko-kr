---
title: 전용 SQL 풀 Azure Advisor 권장 사항
description: Synapse SQL 권장 사항 및 생성 방법에 대해 알아보기
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 06/26/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: bd32b9690f8a9aef92eb1f2fbcc4ec926a65584e
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121195"
---
# <a name="azure-advisor-recommendations-for-dedicated-sql-pool-in-azure-synapse-analytics"></a>Azure Synapse Analytics의 전용 SQL 풀에 대 한 Azure Advisor 권장 사항

이 문서에서는 Azure Advisor에서 사용할 수 있는 전용 SQL 풀 권장 사항을 설명 합니다.  

전용 SQL 풀은 데이터 웨어하우스 작업이 성능에 대해 일관 되 게 최적화 되도록 권장 사항을 제공 합니다. 권장 사항은 [Azure Advisor](../../advisor/advisor-performance-recommendations.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)와 긴밀하게 통합되어 [Azure Portal](https://aka.ms/Azureadvisor) 내에서 직접 모범 사례를 제공합니다. 전용 SQL 풀은 일별 흐름에서 활성 워크 로드에 대 한 원격 분석 및 표면 권장 사항을 수집 합니다. 지원되는 권장 시나리오 및 권장 조치를 적용하는 방법이 아래에 설명되어 있습니다.

지금 [권장 사항](https://aka.ms/Azureadvisor)을 확인할 수 있습니다. 

## <a name="data-skew"></a>데이터 기울이기

데이터 기울이기는 워크로드를 실행할 때 추가 데이터 이동 또는 리소스 병목 현상을 일으킬 수 있습니다. 다음 문서에서는 데이터 기울이기를 발견하고 최적의 배포 키를 선택하여 데이터 기울이기를 방지하는 방법을 설명합니다.

- [기울이기 식별 및 제거](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>통계가 없거나 오래된 통계

최적 통계가 아니면 SQL 쿼리 최적화 프로그램이 최적이 아닌 쿼리 계획을 생성할 수 있으므로 쿼리 성능에 심각한 악영향을 미칠 수 있습니다. 다음 문서에서는 통계 생성 및 업데이트와 관련된 모범 사례를 설명합니다.

- [테이블 통계 생성 및 업데이트](sql-data-warehouse-tables-statistics.md)

이러한 권장 사항의 영향을 받은 테이블 목록을 확인하려면 다음 [T-SQL 스크립트](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables)를 실행합니다. Advisor는 계속 동일한 T-SQL 스크립트를 실행하여 이러한 권장 사항을 생성합니다.

## <a name="replicate-tables"></a>테이블 복제

복제된 테이블 권장 사항의 경우 Advisor는 다음과 같은 물리적 특징에 따라 테이블 후보를 검색합니다.

- 복제 테이블 크기
- 열 수
- 테이블 배포 형식
- 파티션 수

Advisor는 테이블 액세스 빈도, 평균으로 반환되는 행 및 데이터 웨어하우스 크기 및 작업의 임계값과 같은 워크로드 기반의 추론을 지속적으로 활용하여 고품질 권장 사항이 생성되는지 확인합니다.

다음 섹션에서는 Azure Portal에서 찾을 수 있는 각 복제 테이블 권장 사항에 대한 워크로드 기반 추론을 설명합니다.

- 검사 평균 - 지난 7일 동안 각 테이블 액세스에 대해 테이블에서 반환된 행의 평균 백분율
- 빈번한 읽기, 업데이트 안 됨 - 액세스 활동을 표시하는 동시에 지난 7일 동안 테이블을 업데이트하지 않았음을 나타냄
- 읽기/업데이트 비율 - 지난 7일 동안 테이블이 업데이트된 경우에 따라 테이블에 액세스한 빈도율
- 활동 - 액세스 활동을 기준으로 사용량을 측정합니다. 이 활동은 지난 7일 동안의 데이터 웨어하우스 간 평균 테이블 액세스 활동과 관련된 테이블 액세스 활동을 비교합니다.

현재 Advisor는 가장 높은 우선 순위의 작업을 지정하는 클러스터형 Columnstore 인덱스를 사용하여 한 번에 최대 4개의 복제된 테이블 후보를 보여줍니다.

> [!IMPORTANT]
> 복제된 테이블 권장 사항은 전체 증명으로, 데이터 이동 작업을 고려하지 않습니다. Microsoft에서는 추론을 바탕으로 이를 추가하려고 노력하지만 그 사이에 사용자는 권장 사항을 적용한 후 항상 워크로드의 유효성을 검사해야 합니다. 복제된 테이블에 대한 자세한 내용은 다음 [설명서](design-guidance-for-replicated-tables.md#what-is-a-replicated-table)를 참조하세요.


## <a name="adaptive-gen2-cache-utilization"></a>적응(Gen2) 캐시 사용률
작업 집합이 크면 캐시 적중률이 낮고 캐시 사용률이 높아질 수 있습니다. 이 시나리오에서는 캐시 용량을 늘리고 워크로드를 다시 실행하도록 확장해야 합니다. 자세한 내용은 다음 [설명서](./sql-data-warehouse-how-to-monitor-cache.md)를 참조하세요. 

## <a name="tempdb-contention"></a>Tempdb 경합

tempdb 경합이 높은 경우 쿼리 성능이 저하될 수 있습니다.  Tempdb 경합은 사용자 정의 임시 테이블을 통해 또는 많은 양의 데이터 이동이 있을 때 발생할 수 있습니다. 이 시나리오에서는 추가 tempdb 할당을 위해 크기를 조정하고 [리소스 클래스 및 워크로드를 관리](./sql-data-warehouse-workload-management.md)하여 쿼리에 더 많은 메모리를 제공할 수 있습니다. 

## <a name="data-loading-misconfiguration"></a>데이터 로드 잘못 구성

대기 시간을 최소화 하려면 항상 전용 SQL 풀과 동일한 지역의 저장소 계정에서 데이터를 로드 해야 합니다. [높은 처리량 데이터 수집을 위해 COPY 문을](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) 사용 하 고 준비 된 파일을 저장소 계정에 분할 하 여 처리량을 최대화 합니다. COPY 문을 사용할 수 없는 경우 더 나은 처리량을 위해 일괄 처리 크기가 높은 SqlBulkCopy API 또는 bcp를 사용할 수 있습니다. 추가 데이터 로드 지침은 다음 [설명서](./guidance-for-loading-data.md)를 참조 하세요.