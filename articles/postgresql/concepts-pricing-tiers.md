---
title: "Azure Database for PostgreSQL의 가격 책정 계층"
description: "이 문서에서는 Azure Database for PostgreSQL의 가격 책정 계층을 설명합니다."
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ee75af9b1d7b77799fe02d87da257ff73bc567e6
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Azure Database for PostgreSQL 가격 책정 계층

Azure Database for PostgreSQL 서버는 기본, 범용 및 메모리 최적화의 세 가지 가격 책정 계층 중 하나에서 만들 수 있습니다. 가격 책정 계층은 프로비전할 수 있는 계산의 vCore 수, vCore당 메모리 및 데이터를 저장하는 데 사용되는 저장소 기술로 구분됩니다. 모든 리소스는 PostgreSQL 서버 수준에서 프로비전됩니다. 서버는 하나 이상의 데이터베이스를 갖출 수 있습니다.

|    | **Basic** | **범용** | **메모리 최적화** |
|:---|:----------|:--------------------|:---------------------|
| 계산 세대 | 4세대, 5세대 | 4세대, 5세대 | 5세대 |
| vCore 수 | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16, 32 |
| vCore 메모리 | 1x | 2x 기본 | 2x 범용 |
| 저장소 크기 | 5GB-1TB | 5GB-1TB | 5GB-1TB |
| 저장소 유형 | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| 데이터베이스 백업 보존 기간 | 7-35일 | 7-35일 | 7-35일 |

다음 표는 가격 책정 계층을 선택하기 위한 시작 지점으로 사용할 수 있습니다.

| 가격 책정 계층  | 대상 워크로드 |
|:-------------|:-----------------|
| Basic | 부담 없는 컴퓨팅 및 I/O 성능이 필요한 워크로드. 예를 들어 개발 또는 시험, 또는 자주 사용하지 않는 소규모 응용 프로그램에 사용되는 서버가 이에 해당합니다. |
| 범용 | 확장 가능한 I/O 처리량을 갖춘 부하 분산된 컴퓨팅 및 메모리가 필요한 대부분의 비즈니스 워크로드. 예를 들어 웹 및 모바일 응용 프로그램 호스팅용 서버 및 기타 엔터프라이즈 응용 프로그램이 있습니다.|
| 메모리 최적화 | 빠른 트랜잭션 처리와 높은 동시성을 위해 메모리 내 성능이 필요한 고성능 데이터베이스 워크로드. 예를 들어 실시간 데이터 처리용 서버 및 고성능 트랜잭션 또는 분석 응용 프로그램이 있습니다.|

서버가 만들어지면 vCore 수는 몇 초 이내에 늘리거나 줄일 수 있습니다. 또한 응용 프로그램 중단 시간 없이 독립적으로 저장소 용량을 늘리거나 백업 보존 기간을 늘리거나 줄일 수 있습니다. 자세한 내용은 아래의 크기 조정 섹션을 참조하세요.

## <a name="compute-generations-vcores-and-memory"></a>세대, vCore 수 및 메모리 계산

계산 리소스는 기본 하드웨어의 논리적 CPU를 나타내는 vCore 수로 제공됩니다. 현재 2개의 계산 세대, 즉 4세대와 5세대가 제공됩니다. 4세대 논리적 CPU는 Intel E5-2673 v3(Haswell) 2.4GHz 프로세서를 기반으로 하며, 5세대 논리적 CPU는 Intel E5-2673 v4(Broadwell) 2.3GHz 프로세서를 기반으로 합니다.

가격 책정 계층에 따라 각 vCore는 특정 양의 메모리로 프로비전됩니다. 서버에 대한 vCore 수를 늘리거나 줄이면 이에 비례하여 메모리도 늘어나거나 줄어듭니다. 범용 계층은 기본 계층에 비해 vCore당 두 배의 메모리를 제공하고, 메모리 최적화 계층은 범용 계층에 비해 두 배의 메모리를 제공합니다.

## <a name="storage"></a>Storage

프로비전하는 저장소는 Azure Database for PostgreSQL 서버에 사용할 수 있는 저장소 용량입니다. 저장소는 데이터베이스 파일, 임시 파일, 트랜잭션 로그 및 PostgreSQL 서버 로그에 사용됩니다. 프로비전하는 총 저장소 용량도 서버에 사용할 수 있는 I/O 용량을 정의합니다.

|    | **Basic** | **범용** | **메모리 최적화** |
|:---|:----------|:--------------------|:---------------------|
| 저장소 유형 | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| 저장소 크기 | 5GB-1TB | 5GB-1TB | 5GB-1TB |
| 저장소 증분 크기 | 1 GB | 1 GB | 1 GB |
| IOPS | 변수 |3IOPS/GB<br/>최소 100IOPS | 3IOPS/GB<br/>최소 100IOPS |

추가 저장소 용량은 서버를 만드는 동안 및 그 후에 추가할 수 있습니다. 기본 계층에서는 IOPS 보장을 제공하지 않습니다. 범용 및 메모리 최적화 가격 책정 계층에서 IOPS의 크기는 프로비전된 저장소 크기와 3:1 비율로 조정됩니다.

Azure Portal 또는 Azure CLI 명령을 사용하여 I/O 사용량을 모니터링할 수 있습니다. 모니터링할 관련 메트릭은 [저장소 제한, 저장소 비율, 저장소 사용됨 및 IO 백분율](concepts-monitoring.md)입니다.

## <a name="backup"></a>Backup

서비스에서 서버 백업을 자동으로 수행합니다. 최소 백업 보존 기간은 7일입니다. 보존 기간은 최대 35일까지 설정할 수 있습니다. 보존 기간은 서버 수명 기간 동안 언제든지 조정할 수 있습니다. 로컬 중복 백업과 지역 중복 백업 중에서 선택할 수 있습니다. 지역 중복 백업은 서버가 만들어진 지역의 [쌍으로 연결된 지역](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)에도 저장됩니다. 이렇게 하면 재해 발생 시 일정 수준의 보호를 제공합니다. 또한 지역 중복 백업을 통해 서비스를 사용할 수 있는 다른 Azure 지역으로 서버를 복원할 수 있습니다. 서버가 만들어지면 두 개의 백업 저장소 옵션 간에 변경할 수 없습니다.

## <a name="scale-resources"></a>리소스 크기 조정

서버가 만들어지면 vCore 수, 저장소 크기 및 백업 보존 기간을 독립적으로 변경할 수 있습니다. 그러나 가격 책정 계층 또는 백업 저장소 유형은 변경할 수 없습니다. vCore 수 및 백업 보존 기간은 늘리거나 줄일 수 있습니다. 저장소 크기는 늘릴 수 있습니다. 리소스의 크기 조정은 포털 또는 Azure CLI를 통해 수행할 수 있습니다. CLI를 사용하여 크기 조정하는 예제는 [여기](scripts/sample-scale-server-up-or-down.md)에 있습니다.

vCore 수를 변경하면 새 계산 할당을 사용하여 원본 서버의 복사본이 만들어집니다. 새 서버가 시작되고 실행되면 새 서버에 대한 연결로 전환됩니다. 시스템이 새 서버로 전환되면 잠시 동안 새 연결을 설정할 수 없으며, 커밋되지 않은 모든 트랜잭션이 롤백됩니다. 이 기간은 다양하지만, 대부분의 경우 1분 미만입니다.

저장소 크기 조정 및 백업 보존 기간 변경은 온라인 작업입니다. 응용 프로그램에 가동 중지 시간이 없거나 아무런 영향이 없습니다. IOPS가 프로비전된 저장소 크기로 조정되면 저장소를 확장하여 서버에서 사용할 수 있는 IOPS를 늘릴 수 있습니다.

## <a name="pricing"></a>가격

최신 가격 책정 정보는 서비스 [가격 페이지](https://azure.microsoft.com/pricing/details/PostgreSQL/)를 검토하세요. 원하는 구성 비용을 확인하려면 [Azure Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer)에서 선택한 옵션에 따라 **가격 책정 계층** 탭에 월별 비용이 표시됩니다. Azure 구독이 없는 경우 Azure 가격 책정 계산기를 사용하여 예상 가격을 구할 수 있습니다. [Azure 가격 책정 계산기](https://azure.microsoft.com/pricing/calculator/) 웹 사이트를 방문하여 **항목 추가**를 클릭하고, **데이터베이스** 범주를 확장하고, **PostgreSQL용 Azure Database**를 선택하여 옵션을 사용자 지정합니다.

## <a name="next-steps"></a>다음 단계

- [포털에서 PostgreSQL 서버를 만드는 방법](tutorial-design-database-using-azure-portal.md)을 알아봅니다.
- [Azure CLI를 사용하여 Azure Database for PostgreSQL 서버 모니터링 및 확장](scripts/sample-scale-server-up-or-down.md)을 알아봅니다.
- [서비스 제한 사항](concepts-limits.md)을 알아봅니다.
