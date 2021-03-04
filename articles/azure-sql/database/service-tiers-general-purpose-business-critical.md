---
title: 범용 및 중요 비즈니스용 서비스 계층
titleSuffix: Azure SQL Database & SQL Managed Instance
description: 이 문서에서는 Azure SQL Database 및 Azure SQL Managed Instance에서 사용 하는 vCore 기반 구매 모델의 범용 및 업무상 중요 한 서비스 계층에 대해 설명 합니다.
services: sql-database
ms.service: sql-db-mi
ms.subservice: features
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake
ms.date: 12/14/2020
ms.openlocfilehash: 95e11e98be8a58611a435de533ffcc16ec5ce357
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102048559"
---
# <a name="azure-sql-database-and-azure-sql-managed-instance-service-tiers"></a>Azure SQL Database 및 Azure SQL Managed Instance 서비스 계층
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL Database 및 Azure SQL Managed Instance는 인프라 오류가 발생 하는 경우에도 99.99%의 가용성을 보장 하기 위해 클라우드 환경에 맞게 조정 된 SQL Server 데이터베이스 엔진 아키텍처를 기반으로 합니다. 두 서비스 계층은 Azure SQL Database 및 Azure SQL Managed Instance에서 각각 다른 아키텍처 모델을 사용 합니다. 이러한 서비스 계층은 다음과 같습니다.

- 예산 중심 작업을 위해 설계 [된 범용입니다](service-tier-general-purpose.md).
- [중요 비즈니스용](service-tier-business-critical.md)으로, 오류 및 빠른 장애 조치 (failover)에 대 한 복원 력이 높은 짧은 대기 시간 워크 로드 용으로 설계 되었습니다.

Azure SQL Database에는 추가 서비스 계층이 있습니다. 

- 매우 확장 가능한 저장소, 읽기 확장 및 빠른 데이터베이스 복원 기능을 제공 하는 대부분의 비즈니스 워크 로드에 맞게 설계 된 [Hyperscale](service-tier-hyperscale.md)

이 문서에서는 vCore 기반 구매 모델의 범용 및 중요 비즈니스용 서비스 계층에 대 한 서비스 계층, 저장소 및 백업 고려 사항 간의 차이점을 설명 합니다.

## <a name="service-tier-comparison"></a>서비스 계층 비교

다음 표에서는 최신 세대에 대 한 서비스 계층의 주요 차이점에 대해 설명 합니다 (Gen5). 서비스 계층 특성은 SQL Database 및 SQL Managed Instance에서 다를 수 있습니다.

|-| 리소스 종류 | 범용 |  하이퍼스케일 | 중요 비즈니스용 |
|:---:|:---:|:---:|:---:|:---:|
| **적합한 대상** | |  예산 중심의 균형 잡힌 컴퓨팅 및 스토리지 옵션을 제공합니다. | 대부분의 비즈니스 워크로드. 자동 크기 조정 저장소 크기는 최대 100 TB, 유체 수직 및 수평 계산 크기 조정, 빠른 데이터베이스 복원입니다. | 트랜잭션 속도가 높고 IO 대기 시간이 낮은 OLTP 응용 프로그램 는 동시에 업데이트 된 여러 복제본을 사용 하 여 오류 및 빠른 장애 조치에 가장 높은 복원 력을 제공 합니다|
|  **리소스 종류에서 사용 가능:** ||SQL Database/SQL Managed Instance | 단일 Azure SQL Database | SQL Database/SQL Managed Instance |
| **컴퓨팅 크기**| SQL Database | vCore 1~80개 | 1 ~ 80 vCores | vCore 1~80개 |
| | SQL Managed Instance | 4, 8, 16, 24, 32, 40, 64, 80 vCores | 해당 없음 | 4, 8, 16, 24, 32, 40, 64, 80 vCores |
| | SQL Managed Instance 풀 | 2, 4, 8, 16, 24, 32, 40, 64, 80 vCores | 해당 없음 | 해당 없음 |
| **스토리지 유형** | 모두 | 프리미엄 원격 스토리지(인스턴스별) | 로컬 SSD 캐시를 사용한 분리형 스토리지(인스턴스별) | 초고속 로컬 SSD 스토리지(인스턴스별) |
| **데이터베이스 크기** | SQL Database | 5GB~4TB | 최대 100TB | 5GB~4TB |
| | SQL Managed Instance  | 32GB~8TB | 해당 없음 | 32GB~4TB |
| **스토리지 크기** | SQL Database | 5GB~4TB | 최대 100TB | 5GB~4TB |
| | SQL Managed Instance  | 32GB~8TB | 해당 없음 | 32GB~4TB |
| **TempDB 크기** | SQL Database | [vCore 당 32 GB](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) | [vCore 당 32 GB](resource-limits-vcore-single-databases.md#hyperscale---provisioned-compute---gen5) | [vCore 당 32 GB](resource-limits-vcore-single-databases.md#business-critical---provisioned-compute---gen4) |
| | SQL Managed Instance  | [vCore 당 24gb](../managed-instance/resource-limits.md#service-tier-characteristics) | 해당 없음 | [저장소 크기에 따라](../managed-instance/resource-limits.md#service-tier-characteristics) 최대 4 TB 제한 |
| **로그 쓰기 처리량** | SQL Database | [vCore 당 1.875 m b/초 (최대 30 m b/초)](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) | 100MB/초 | [vCore 당 6mb/s (최대 96 m b/초)](resource-limits-vcore-single-databases.md#business-critical---provisioned-compute---gen4) |
| | SQL Managed Instance | [vCore 당 3MB/s (최대 22 m b/초)](../managed-instance/resource-limits.md#service-tier-characteristics) | 해당 없음 | [vcore 당 4mb/s (최대 48 m b/초)](../managed-instance/resource-limits.md#service-tier-characteristics) |
|**가용성**|모두| 99.99% |  [보조 복제본이 하나인 99.95%, 추가 복제본이 있는 99.99%](service-tier-hyperscale-frequently-asked-questions-faq.md#what-slas-are-provided-for-a-hyperscale-database) | 99.99% <br/> [영역 중복 단일 데이터베이스가 포함 된 99.995%](https://azure.microsoft.com/blog/understanding-and-leveraging-azure-sql-database-sla/) |
|**Backup**|모두|RA-GRS, 7-35 일 (기본적으로 7 일) 기본 계층에 대 한 최대 보존 기간은 7 일입니다. | RA-GRS, 7 일, 상수 시간 지정 시간 복구 (PITR) | RA-GRS, 7-35일(기본값: 7일) |
|**메모리 내 OLTP** | | 해당 없음 | 해당 없음 | 사용 가능 |
|**읽기 전용 복제본**| | 0 기본 제공 <br> 0-4 [지역에서 복제](active-geo-replication-overview.md) 사용 | 0-4 기본 제공 | 1 기본 제공, 가격에 포함 <br> 0-4 [지역에서 복제](active-geo-replication-overview.md) 사용 |
|**가격 책정/청구** | SQL Database | [Vcore, 예약 된 저장소 및 백업 저장소가](https://azure.microsoft.com/pricing/details/sql-database/single/) 청구 됩니다. <br/>IOPS에는 요금이 부과 되지 않습니다. | [각 복제본 및 사용 된 저장소에 대 한 Vcore에](https://azure.microsoft.com/pricing/details/sql-database/single/) 는 요금이 부과 됩니다. <br/>IOPS는 아직 청구 되지 않습니다. | [Vcore, 예약 된 저장소 및 백업 저장소가](https://azure.microsoft.com/pricing/details/sql-database/single/) 청구 됩니다. <br/>IOPS에는 요금이 부과 되지 않습니다. |
|| SQL Managed Instance | [Vcore, 예약 된 저장소 및 백업 저장소](https://azure.microsoft.com/pricing/details/sql-database/managed/) 에 대 한 요금이 청구 됩니다. <br/>IOPS는 요금이 청구 되지 않습니다.| 해당 없음 | [Vcore, 예약 된 저장소 및 백업 저장소](https://azure.microsoft.com/pricing/details/sql-database/managed/) 에 대 한 요금이 청구 됩니다. <br/>IOPS에는 요금이 부과 되지 않습니다.| 
|**할인 모델**| | [예약 인스턴스](reserved-capacity-overview.md)<br/>[Azure 하이브리드 혜택](../azure-hybrid-benefit.md) (개발/테스트 구독에서 사용할 수 없음)<br/>[엔터프라이즈](https://azure.microsoft.com/offers/ms-azr-0148p/) 및 [종 량](https://azure.microsoft.com/offers/ms-azr-0023p/) 제 개발/테스트 구독| [Azure 하이브리드 혜택](../azure-hybrid-benefit.md) (개발/테스트 구독에서 사용할 수 없음)<br/>[엔터프라이즈](https://azure.microsoft.com/offers/ms-azr-0148p/) 및 [종 량](https://azure.microsoft.com/offers/ms-azr-0023p/) 제 개발/테스트 구독| [예약 인스턴스](reserved-capacity-overview.md)<br/>[Azure 하이브리드 혜택](../azure-hybrid-benefit.md) (개발/테스트 구독에서 사용할 수 없음)<br/>[엔터프라이즈](https://azure.microsoft.com/offers/ms-azr-0148p/) 및 [종 량](https://azure.microsoft.com/offers/ms-azr-0023p/) 제 개발/테스트 구독|

자세한 내용은 [Azure SQL Database (vCore)](resource-limits-vcore-single-databases.md)의 서비스 계층, [단일 Azure SQL Database (dtu](resource-limits-dtu-single-databases.md)), [풀링된 Azure SQL Database (dtu)](resource-limits-dtu-single-databases.md)및 [Azure SQL Managed Instance](../managed-instance/resource-limits.md) 페이지 간의 자세한 차이점을 참조 하세요.

> [!NOTE]
> Vcore 기반 구매 모델의 하이퍼 크기 조정 서비스 계층에 대 한 자세한 내용은 [대규모 service 계층](service-tier-hyperscale.md)을 참조 하세요. DTU 기반 구매 모델과 vCore 기반 구매 모델을 비교 하는 방법에 대해서는 [모델 및 리소스 구매](purchasing-models.md)를 참조 하세요.

## <a name="data-and-log-storage"></a>데이터 및 로그 스토리지

다음 요소는 데이터 및 로그 파일에 사용 되는 저장소의 양에 영향을 주며 범용 및 중요 비즈니스용에 적용 됩니다. Hyperscale의 데이터 및 로그 저장소에 대 한 자세한 내용은 [hyperscale service 계층](service-tier-hyperscale.md)을 참조 하세요.

- 할당 된 저장소는 데이터 파일 (MDF) 및 로그 파일 (LDF)에서 사용 됩니다.
- 각 단일 데이터베이스 계산 크기는 최대 데이터베이스 크기를 지원 하며 기본 최대 크기는 32 GB입니다.
- 필요한 단일 데이터베이스 크기 (MDF 파일의 크기)를 구성 하면 LDF 파일을 지원 하기 위해 30% 더 많은 추가 저장소가 자동으로 추가 됩니다.
- 10gb와 지원 되는 최대의 단일 데이터베이스 크기를 선택할 수 있습니다.
  - Standard 또는 범용 서비스 계층의 저장소의 경우 10gb 증분으로 크기를 늘리거나 줄입니다.
  - 프리미엄 또는 중요 비즈니스용 서비스 계층의 저장소에 대해 250-GB 증가값으로 크기를 늘리거나 줄입니다.
- 범용 서비스 계층에서는 연결 된 `tempdb` SSD를 사용 하며,이 저장소 비용은 vCore 가격에 포함 됩니다.
- 중요 비즈니스용 서비스 계층에서는 `tempdb` 연결 된 SSD와 MDF 및 LDF 파일을 공유 하며, `tempdb` 저장소 비용은 vcore 가격에 포함 됩니다.
- DTU 프리미엄 서비스 계층에서는 `tempdb` 연결 된 SSD와 MDF 및 LDF 파일을 공유 합니다.
- SQL Managed Instance의 저장소 크기는 32 GB의 배수로 지정 해야 합니다.


> [!IMPORTANT]
> MDF 및 LDF 파일에 대해 할당 된 총 저장소에 대 한 요금이 청구 됩니다.

MDF 및 LDF 파일의 현재 총 크기를 모니터링 하려면 [sp_spaceused](/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql)을 사용 합니다. 개별 MDF 및 LDF 파일의 현재 크기를 모니터링하려면 [sys.database_files](/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql)를 사용합니다.

> [!IMPORTANT]
> 경우에 따라 사용하지 않는 공간을 회수하기 위해 데이터베이스를 축소해야 할 수도 있습니다. 자세한 내용은 [Azure SQL Database에서 파일 공간 관리](file-space-manage.md)를 참조 하세요.

## <a name="backups-and-storage"></a>백업 및 스토리지

데이터베이스 백업용 저장소는 SQL Database 및 SQL Managed Instance의 PITR (특정 시점 복원) 및 [LTR (장기 보존)](long-term-retention-overview.md) 기능을 지원 하기 위해 할당 됩니다. 이 스토리지는 각 데이터베이스에 대해 개별적으로 할당되며 데이터베이스당 별도의 두 가지 요금으로 청구됩니다.

- **PITR**: 개별 데이터베이스 백업은 [읽기 액세스 지역 중복 (RA-GRS) 저장소](../../storage/common/geo-redundant-design.md) 로 자동 복사 됩니다. 저장소 크기는 새 백업이 생성 될 때 동적으로 늘어납니다. 저장소는 매주 전체 백업, 일별 차등 백업 및 5 분 마다 복사 되는 트랜잭션 로그 백업에 사용 됩니다. 저장소 사용량은 데이터베이스의 변경 률과 백업 보존 기간에 따라 달라 집니다. 각 데이터베이스에 대한 개별적인 보존 기간은 7-35일 사이에서 구성할 수 있습니다. 데이터베이스 크기의 100% (1x)와 같은 최소 저장소 크기는 추가 요금 없이 제공 됩니다. 대부분의 데이터베이스에서 이 크기는 7일간의 백업을 저장하기에 충분합니다.
- **LTR**: [SQL Managed Instance](long-term-retention-overview.md)최대 10 년 동안 전체 백업의 장기 보존을 구성 하는 옵션도 있습니다. LTR 정책을 설정 하는 경우 이러한 백업은 RA GRS 저장소에 자동으로 저장 되지만 백업 복사 빈도를 제어할 수 있습니다. 서로 다른 규정 준수 요구 사항을 충족 하기 위해 주별, 월별 및/또는 매년 백업에 대해 서로 다른 보존 기간을 선택할 수 있습니다. 선택한 구성에 따라 LTR 백업에 사용 되는 저장소의 양이 결정 됩니다. LTR 저장소의 비용을 예상 하려면 LTR 가격 계산기를 사용할 수 있습니다. 자세한 내용은 [장기 보존 SQL Database](long-term-retention-overview.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

범용 및 중요 비즈니스용 서비스 계층에서 사용할 수 있는 특정 계산 및 저장소 크기에 대 한 자세한 내용은 다음을 참조 하세요. 

- [Azure SQL Database에 대 한 Vcore 기반 리소스 제한](resource-limits-vcore-single-databases.md)입니다.
- [Azure SQL Database의 풀링된 데이터베이스에 대 한 Vcore 기반 리소스 제한](resource-limits-vcore-elastic-pools.md)입니다.
- [AZURE SQL Managed Instance에 대 한 Vcore 기반 리소스 제한](../managed-instance/resource-limits.md)입니다.
