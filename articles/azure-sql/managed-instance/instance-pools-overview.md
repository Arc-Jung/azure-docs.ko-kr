---
title: Azure SQL Managed Instance 풀 이란?
titleSuffix: Azure SQL Managed Instance
description: 더 작은 SQL Server 데이터베이스를 대규모로 클라우드로 마이그레이션하고 여러 관리 되는 인스턴스를 관리 하는 편리 하 고 비용 효율적인 방법을 제공 하는 Azure SQL Managed Instance 풀 (미리 보기)에 대해 알아봅니다.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein
ms.date: 09/05/2019
ms.openlocfilehash: bc345509db1c2a14afb0ae781eccad8f77395c18
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97347067"
---
# <a name="what-is-an-azure-sql-managed-instance-pool-preview"></a>Azure SQL Managed Instance 풀 (미리 보기) 이란?
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance의 인스턴스 풀은 더 작은 SQL Server 인스턴스를 대규모로 클라우드로 마이그레이션하는 데 편리 하 고 비용 효율적인 방법을 제공 합니다.

인스턴스 풀을 사용하면 총 마이그레이션 요구 사항에 따라 계산 리소스를 미리 프로비전할 수 있습니다. 그런 다음 미리 프로비전된 계산 수준까지 여러 개의 개별 관리되는 인스턴스를 배포할 수 있습니다. 예를 들어 vCores 8 개를 사전 프로 비전 하는 경우 2 2-Vcores 및 1 4-Vcores 인스턴스를 배포 하 고 데이터베이스를 이러한 인스턴스로 마이그레이션할 수 있습니다. 인스턴스 풀을 사용할 수 있기 전에는 더 작고 적은 계산 집약적인 워크 로드를 클라우드로 마이그레이션할 때 더 큰 관리 되는 인스턴스로 통합 해야 하는 경우가 많습니다. 데이터베이스 그룹을 대형 인스턴스로 마이그레이션하려면 일반적으로 용량 계획 및 리소스 관리, 추가 보안 고려 사항 및 인스턴스 수준에서의 추가 데이터 통합 작업을 수행 해야 합니다.

또한 인스턴스 풀은 기본 VNet 통합을 지원 하므로 동일한 서브넷에 여러 인스턴스 풀 및 여러 개의 단일 인스턴스를 배포할 수 있습니다.

## <a name="key-capabilities"></a>주요 기능

인스턴스 풀은 다음과 같은 이점을 제공 합니다.

1. 2 vCore 인스턴스를 호스트할 수 있습니다. *\* 인스턴스 풀의 인스턴스에만 해당* 됩니다.
2. 예측 가능 하 고 빠른 인스턴스 배포 시간 (최대 5 분)
3. 최소 IP 주소 할당.

다음 다이어그램에서는 가상 네트워크 서브넷에 배포 된 여러 관리 되는 인스턴스가 있는 인스턴스 풀을 보여 줍니다.

![여러 인스턴스를 포함 하는 인스턴스 풀](./media/instance-pools-overview/instance-pools1.png)

인스턴스 풀을 사용 하면 동일한 가상 컴퓨터에 여러 인스턴스를 배포할 수 있습니다. 여기서 가상 컴퓨터의 계산 크기는 풀에 할당 된 총 vCores 수를 기반으로 합니다. 이 아키텍처를 사용 하면 가상 컴퓨터를 여러 인스턴스로 *분할할* 수 있습니다 .이 경우 2 개 vcores를 포함 하 여 지원 되는 모든 크기를 가질 수 있습니다 (2-vcores 인스턴스는 풀의 인스턴스에서만 사용할 수 있음).

초기 배포 후에 풀의 인스턴스에 대 한 관리 작업은 훨씬 더 빠릅니다. 이는 [가상 클러스터](connectivity-architecture-overview.md#high-level-connectivity-architecture) 의 배포 또는 확장 (가상 컴퓨터의 전용 집합)이 관리 되는 인스턴스를 프로 비전 하는 부분이 아니기 때문입니다.

풀의 모든 인스턴스가 동일한 가상 컴퓨터를 공유 하기 때문에 총 IP 할당은 배포 된 인스턴스 수에 따라 달라 지므로 IP 범위가 좁은 서브넷에 배포 하는 데 편리 합니다.

각 풀에는 9 개의 IP 주소에 대 한 고정 IP 할당이 있습니다 (자체 요구에 맞게 예약 된 서브넷에 5 개의 IP 주소를 포함 하지 않음). 자세한 내용은 [단일 인스턴스에 대 한 서브넷 크기 요구 사항](vnet-subnet-determine-size.md)을 참조 하세요.

## <a name="application-scenarios"></a>애플리케이션 시나리오

다음 목록에서는 인스턴스 풀을 고려해 야 하는 주요 사용 사례를 제공 합니다.

- 동시에 *SQL Server 인스턴스 그룹* 의 마이그레이션. 과반수는 작은 크기 (예: 2 또는 4 개 vcores)입니다.
- 예측 가능 *하 고 짧은 인스턴스를 만들거나 크기를 조정 하* 는 것이 중요 한 시나리오 예를 들어 인스턴스 수준 기능이 필요한 다중 테 넌 트 SaaS 응용 프로그램 환경에서 새 테 넌 트를 배포 합니다.
- *고정 비용* 또는 *지출 한도가* 있는 시나리오는 중요 합니다. 예를 들어, 필요에 따라 관리 되는 인스턴스를 정기적으로 배포 하는 고정 된 (또는 자주 변경 되지 않는) 크기의 공유 개발-테스트 또는 데모 환경을 실행 합니다.
- VNet 서브넷의 *최소 IP 주소 할당이* 중요 한 시나리오입니다. 풀의 모든 인스턴스는 가상 컴퓨터를 공유 하 고 있으므로 할당 된 IP 주소 수는 단일 인스턴스의 경우 보다 낮습니다.

## <a name="architecture"></a>아키텍처

인스턴스 풀은 일반 (*단일*) 관리 되는 인스턴스의 아키텍처와 유사 합니다. [Azure 가상 네트워크 내에서 배포](../../virtual-network/virtual-network-for-azure-services.md) 를 지원 하 고 고객에 대 한 격리 및 보안을 제공 하기 위해 인스턴스 풀은 [가상 클러스터](connectivity-architecture-overview.md#high-level-connectivity-architecture)에도 의존 합니다. 가상 클러스터는 고객의 가상 네트워크 서브넷 내에 배포 된 격리 된 가상 머신의 전용 집합을 나타냅니다.

두 배포 모델의 주요 차이점은 인스턴스 풀이 [Windows 작업 개체](/windows/desktop/ProcThread/job-objects)를 사용 하 여 리소스를 관리 하는 동일한 가상 컴퓨터 노드에서 여러 SQL Server 프로세스 배포를 허용 하는 반면 단일 인스턴스는 항상 가상 컴퓨터 노드에 있는 경우입니다.

다음 다이어그램에서는 인스턴스 풀과 동일한 서브넷에 배포 된 두 개의 개별 인스턴스를 보여 주고 두 배포 모델에 대 한 주요 아키텍처 세부 정보를 보여 줍니다.

![인스턴스 풀 및 두 개의 개별 인스턴스](./media/instance-pools-overview/instance-pools2.png)

모든 인스턴스 풀은 아래에 별도의 가상 클러스터를 만듭니다. 풀 내의 인스턴스와 동일한 서브넷에 배포 된 단일 인스턴스는 SQL Server 프로세스 및 게이트웨이 구성 요소에 할당 된 계산 리소스를 공유 하지 않으므로 성능 예측 가능성을 보장 합니다.

## <a name="resource-limitations"></a>리소스 제한

인스턴스 풀 및 풀 내의 인스턴스에 대한 몇 가지 리소스 제한이 있습니다.

- 인스턴스 풀은 Gen5 하드웨어 에서만 사용할 수 있습니다.
- 풀 내의 관리 되는 인스턴스에는 전용 CPU와 RAM이 있으므로 모든 인스턴스의 집계 된 vCores 수는 풀에 할당 된 vCores의 수보다 작거나 같아야 합니다.
- 모든 [인스턴스 수준 제한은](resource-limits.md#service-tier-characteristics) 풀 내에 생성 된 인스턴스에 적용 됩니다.
- 인스턴스 수준 제한 외에도 *인스턴스 풀 수준에* 적용 되는 두 가지 제한이 있습니다.
  - 풀 당 총 저장소 크기 (8TB)
  - 풀 당 총 사용자 데이터베이스 수입니다. 이 제한은 풀 vCores 값에 따라 달라 집니다.
    - 8 vCores 풀은 최대 200 개의 데이터베이스를 지원 합니다.
    - 16 개 vCores 풀은 최대 400 개의 데이터베이스를 지원 합니다.
    - 24 개 이상 vCores 풀은 최대 500 개의 데이터베이스를 지원 합니다.
- 인스턴스 풀 내에 배포 된 인스턴스에 대해 AAD 관리자를 설정할 수 없으므로 AAD 인증을 사용할 수 없습니다.

모든 인스턴스의 전체 저장소 할당과 데이터베이스 수는 인스턴스 풀에 의해 노출 되는 한도 보다 낮거나 같아야 합니다.

- 인스턴스 풀은 8, 16, 24, 32, 40, 64 및 80 vCores를 지원 합니다.
- 풀 내의 관리 되는 인스턴스는 2, 4, 8, 16, 24, 32, 40, 64 및 80 vCores를 지원 합니다.
- 풀 내의 관리 되는 인스턴스는 다음을 제외 하 고 32 GB에서 8TB 사이의 저장소 크기를 지원 합니다.
  - 2 vCore 인스턴스는 32 GB와 640 GB 사이의 크기를 지원 합니다.
  - 4 vCore 인스턴스는 32 GB에서 2tb 사이의 크기를 지원 합니다.
- 풀 내의 관리 되는 인스턴스는 인스턴스당 최대 100 개의 사용자 데이터베이스를 지 50 원하는 2 개의 vCore 인스턴스를 제외 하 고 인스턴스당 최대 개의 사용자 데이터베이스를 제한 합니다.

[서비스 계층 속성](resource-limits.md#service-tier-characteristics) 은 인스턴스 풀 리소스와 연결 되므로 풀의 모든 인스턴스는 풀의 서비스 계층과 동일한 서비스 계층 이어야 합니다. 이번에는 범용 서비스 계층만 사용할 수 있습니다 (현재 미리 보기의 제한 사항에 대해서는 다음 섹션 참조).

### <a name="public-preview-limitations"></a>공용 미리 보기 제한 사항

공개 미리 보기에는 다음과 같은 제한 사항이 있습니다.

- 현재는 범용 서비스 계층만 사용할 수 있습니다.
- 공개 미리 보기 동안에는 인스턴스 풀의 크기를 조정할 수 없으므로 배포 하기 전에 용량을 신중 하 게 계획 해야 합니다.
- 인스턴스 풀 만들기 및 구성에 대 한 Azure Portal 지원이 아직 제공 되지 않습니다. 인스턴스 풀의 모든 작업은 PowerShell을 통해서만 지원 됩니다. 미리 만든 풀의 초기 인스턴스 배포도 PowerShell을 통해서만 지원 됩니다. 풀에 배포 된 후에는 Azure Portal를 사용 하 여 관리 되는 인스턴스를 업데이트할 수 있습니다.
- 풀 외부에서 만들어진 관리 되는 인스턴스는 기존 풀로 이동할 수 없으며 풀 내에서 만든 인스턴스는 단일 인스턴스나 다른 풀 외부로 이동할 수 없습니다.
- [예약 용량](../database/reserved-capacity-overview.md) 인스턴스 가격 책정을 사용할 수 없습니다.

## <a name="sql-features-supported"></a>지원되는 SQL 기능

풀에서 만든 관리 되는 인스턴스는 [단일 관리 되는 인스턴스에서 지원 되는 것과 동일한 호환성 수준 및 기능](sql-managed-instance-paas-overview.md#sql-features-supported)을 지원 합니다.

풀에 배포 된 모든 관리 되는 인스턴스에는 SQL 에이전트의 개별 인스턴스가 있습니다.

특정 값을 선택 해야 하는 선택적 기능 (예: 인스턴스 수준 데이터 정렬, 표준 시간대, 데이터 트래픽의 공용 끝점, 장애 조치 (failover) 그룹)은 인스턴스 수준에서 구성 되며 풀의 각 인스턴스마다 다를 수 있습니다.

## <a name="performance-considerations"></a>성능 고려 사항

풀 내의 관리 되는 인스턴스는 전용 vCore 및 RAM을 갖지만 로컬 디스크 (tempdb 사용의 경우) 및 네트워크 리소스를 공유 합니다. 가능한 것은 아니지만 풀의 여러 인스턴스가 동시에 많은 리소스를 사용 하는 경우 *잡음이 있는 환경* 효과를 경험할 수 있습니다. 이 동작을 관찰 하는 경우 이러한 인스턴스를 더 큰 풀이나 단일 인스턴스로 배포 하는 것이 좋습니다.

## <a name="security-considerations"></a>보안 고려 사항

풀에 배포 된 인스턴스가 동일한 가상 컴퓨터를 공유 하기 때문에 더 높은 보안 위험을 야기 하는 기능을 사용 하지 않도록 설정 하거나 이러한 기능에 대 한 액세스 권한을 확실 하 게 제어 하는 것이 좋습니다. 예를 들어 CLR 통합, 네이티브 백업 및 복원, 데이터베이스 전자 메일 등이 있습니다.

## <a name="instance-pool-support-requests"></a>인스턴스 풀 지원 요청

[Azure Portal](https://portal.azure.com)에서 인스턴스 풀에 대 한 지원 요청을 만들고 관리 합니다.

인스턴스 풀 배포 (생성 또는 삭제)와 관련 된 문제가 발생 하는 경우 **문제 하위 유형** 필드에서 **인스턴스 풀** 을 지정 해야 합니다.

![인스턴스 풀 지원 요청](./media/instance-pools-overview/support-request.png)

풀 내의 단일 관리 되는 인스턴스 또는 데이터베이스와 관련 된 문제가 발생 하는 경우 Azure SQL Managed Instance에 대 한 일반 지원 티켓을 만들어야 합니다.

인스턴스 풀을 사용 하거나 사용 하지 않고 대규모 SQL Managed Instance 배포를 만들려면 더 큰 지역 할당량을 얻어야 할 수 있습니다. 자세한 내용은 [Azure SQL Database에 대 한 요청 할당량 늘리기](../database/quota-increase-request.md)를 참조 하세요. 인스턴스 풀의 배포 논리는 할당량을 초과 하지 않고 새 리소스를 만들 수 있는지 여부를 확인 하기 위해 *풀 수준의* 총 vcore 사용량을 할당량과 비교 합니다.

## <a name="instance-pool-billing"></a>인스턴스 풀 청구

인스턴스 풀을 사용 하면 계산 및 저장소를 독립적으로 확장할 수 있습니다. 고객은 vCores에서 측정 된 풀 리소스와 연결 된 계산에 대 한 요금을 지불 하 고, 모든 인스턴스와 연결 된 저장소 (기가바이트)로 측정 됩니다 (처음 32 GB는 모든 인스턴스에 대해 무료로 제공 됨).

풀에 대 한 vCore 가격은 해당 풀에 배포 되는 인스턴스 수에 관계 없이 요금이 청구 됩니다.

계산 가격 (vCores로 측정)의 경우 두 가지 가격 책정 옵션을 사용할 수 있습니다.

  1. *라이선스 포함*: SQL Server 라이선스 가격이 포함 됩니다. 이는 소프트웨어 보증이 적용 되는 기존 SQL Server 라이선스를 적용 하지 않도록 선택 하는 고객을 위한 것입니다.
  2. *Azure 하이브리드 혜택*: SQL Server의 Azure 하이브리드 혜택를 포함 하는 절감 된 가격입니다. 고객은 소프트웨어 보증이 있는 기존 SQL Server 라이선스를 사용 하 여이 가격을 옵트인 (opt in) 할 수 있습니다. 자격 및 기타 세부 정보는 [Azure 하이브리드 혜택](https://azure.microsoft.com/pricing/hybrid-benefit/)를 참조 하세요.

풀의 개별 인스턴스에 대해 다른 가격 책정 옵션을 설정할 수 없습니다. 부모 풀의 모든 인스턴스는 라이선스 포함 가격 또는 Azure 하이브리드 혜택 가격 중 하나 여야 합니다. 풀을 만든 후에 풀의 라이선스 모델을 변경할 수 있습니다.

> [!IMPORTANT]
> 풀에서와 다른 인스턴스에 대 한 라이선스 모델을 지정 하는 경우 풀 가격이 사용 되며 인스턴스 수준 값은 무시 됩니다.

[개발-테스트 혜택](https://azure.microsoft.com/pricing/dev-test/)을 받을 수 있는 구독에 대 한 인스턴스 풀을 만드는 경우 Azure SQL Managed Instance에서 최대 55%의 할인 된 요금이 자동으로 수신 됩니다.

인스턴스 풀 가격 책정에 대 한 자세한 내용은 [SQL Managed Instance 가격 책정 페이지](https://azure.microsoft.com/pricing/details/sql-database/managed/)의 *인스턴스 풀* 섹션을 참조 하세요.

## <a name="next-steps"></a>다음 단계

- 인스턴스 풀을 시작 하려면 [SQL Managed Instance 풀 방법 가이드](instance-pools-configure.md)를 참조 하세요.
- 첫 번째 Managed Instance를 만드는 방법을 알아보려면 [빠른 시작 가이드](instance-create-quickstart.md)를 참조하세요.
- 기능 및 비교 목록은 [AZURE SQL 일반 기능](../database/features-comparison.md)을 참조 하세요.
- VNet 구성에 대한 자세한 내용은 [SQL Managed Instance VNet 구성](connectivity-architecture-overview.md)을 참조하세요.
- 백업 파일에서 관리형 인스턴스를 만들고 데이터베이스를 복원하는 방법에 대한 빠른 시작은 [관리형 인스턴스 만들기](instance-create-quickstart.md)를 참조하세요.
- Azure Database Migration Service를 사용하여 마이그레이션하는 방법에 대한 자습서는 [Database Migration Service를 사용한 SQL Managed Instance 마이그레이션](../../dms/tutorial-sql-server-to-managed-instance.md)을 참조하세요.
- 기본 제공 문제 해결 인텔리전스를 사용하는 SQL Managed Instance 데이터베이스의 고급 성능 모니터링에 대해 자세히 알아보려면 [Azure SQL 분석을 사용하여 Azure SQL Managed Instance 모니터링](../../azure-monitor/insights/azure-sql.md)을 참조하세요.
- 가격 책정 정보는 [SQL Managed Instance 가격 책정](https://azure.microsoft.com/pricing/details/sql-database/managed/)을 참조 하세요.
