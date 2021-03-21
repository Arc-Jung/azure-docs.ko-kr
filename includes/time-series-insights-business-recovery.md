---
ms.topic: include
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.date: 07/09/2020
ms.openlocfilehash: f25c335c568c112c05f81df51d69e83aeff423e2
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96026807"
---
## <a name="business-disaster-recovery"></a>비즈니스 재해 복구

이 섹션에서는 재해가 발생 하더라도 ( *비즈니스 재해 복구* 라고도 함) 앱과 서비스를 실행 하는 Azure Time Series Insights 기능에 대해 설명 합니다.

### <a name="high-availability"></a>고가용성

Azure 서비스인 Azure Time Series Insights는 Azure 지역 수준에서 중복성을 사용 하 *여 특정 고가용성* 기능을 제공 합니다. 예를 들어 Azure는 Azure의 *지역 간 가용성* 기능을 통해 재해 복구 기능을 지원 합니다.

Azure를 통해 제공 되는 추가 고가용성 기능 (그리고 Azure Time Series Insights 인스턴스에도 제공)은 다음과 같습니다.

- **장애 조치 (Failover)**: Azure는 [지역에서 복제 및 부하 분산을](/azure/architecture/resiliency/recovery-loss-azure-region)제공 합니다.
- **데이터 복원** 및 **저장소 복구**: Azure는 [데이터를 유지 하 고 복구 하기 위한 몇 가지 옵션을](/azure/architecture/resiliency/recovery-data-corruption)제공 합니다.
- **Azure Site Recovery**: Azure는 [Azure Site Recovery](../articles/site-recovery/index.yml)를 통해 Site Recovery 기능을 제공 합니다.
- **Azure Backup**: [Azure Backup](../articles/backup/backup-architecture.md) 는 Azure vm의 온-프레미스 및 클라우드 간 백업을 모두 지원 합니다.

관련 Azure 기능을 사용 하도록 설정 하 여 장치 및 사용자에 대 한 글로벌 지역 간 고가용성을 제공 해야 합니다.

> [!NOTE]
> 지역 간 가용성을 사용 하도록 Azure를 구성 하는 경우 Azure Time Series Insights에 추가 지역 간 가용성 구성이 필요 하지 않습니다.

### <a name="iot-and-event-hubs"></a>IoT 및 event hubs

일부 Azure IoT 서비스에는 기본 제공 비즈니스 재해 복구 기능도 포함 되어 있습니다.

- 지역 내 중복성을 포함 하는 고가용성 [재해 복구 Azure IoT Hub](../articles/iot-hub/iot-hub-ha-dr.md)
- [Azure Event Hubs 정책](../articles/event-hubs/event-hubs-geo-dr.md)
- [Azure Storage 중복성](../articles/storage/common/storage-redundancy.md)

다른 서비스와 Azure Time Series Insights를 통합 하면 추가적인 재해 복구 기회가 제공 됩니다. 예를 들어 이벤트 허브로 전송 되는 원격 분석은 백업 Azure Blob storage 데이터베이스에 유지 될 수 있습니다.

### <a name="azure-time-series-insights"></a>Azure Time Series Insights

Azure Time Series Insights 데이터, 앱 및 서비스가 중단 되더라도이를 실행 하는 데는 여러 가지 방법이 있습니다. 

그러나 다음과 같은 목적으로 Azure 시계열 환경의 전체 백업 복사본도 필요 하다 고 결정할 수도 있습니다.

- 특히 데이터 및 트래픽을로 리디렉션하는 Azure Time Series Insights에 대 한 *장애 조치 (failover) 인스턴스로*
- 데이터 및 감사 정보를 보존 하려면

일반적으로 Azure Time Series Insights 환경을 복제 하는 가장 좋은 방법은 백업 Azure 지역에서 두 번째 Azure Time Series Insights 환경을 만드는 것입니다. 이벤트는 주 이벤트 원본에서이 보조 환경으로도 전송 됩니다. 두 번째 전용 소비자 그룹을 사용 하는지 확인 합니다. 앞에서 설명한 대로 해당 원본의 비즈니스 재해 복구 지침을 따릅니다.

중복 환경을 만들려면 다음을 수행 합니다.

1. 두 번째 Azure 지역에 환경을 만듭니다. 자세한 내용은 [Azure Portal에서 새 Azure Time Series Insights 환경 만들기](../articles/time-series-insights/time-series-insights-get-started.md)를 참조 하세요.
1. 이벤트 원본에 대해 두 번째 전용 소비자 그룹을 만듭니다.
1. 해당 이벤트 원본을 새 환경에 연결합니다. 두 번째 전용 소비자 그룹을 지정 했는지 확인 합니다.
1. Azure Time Series Insights [IoT Hub](../articles/time-series-insights/how-to-ingest-data-iot-hub.md) 및 [Event Hubs](../articles/time-series-insights/concepts-access-policies.md) 설명서를 검토 합니다.

이벤트가 발생 하는 경우:

1. 재해 인시던트에 따라 주 지역이 영향을 받는 경우 백업 Azure Time Series Insights 환경으로 작업을 다시 라우팅합니다.
1. 두 번째 영역을 사용 하 여 모든 Azure Time Series Insights 원격 분석 및 쿼리 데이터를 백업 및 복구할 수 있습니다.

> [!IMPORTANT]
> 장애 조치 (failover)가 발생 하는 경우:
> 
> * 지연이 발생할 수도 있습니다.
> * 작업이 다시 라우팅될 때 메시지 처리에 일시적 스파이크가 발생할 수 있습니다.
> 
> 자세한 내용은 [Azure Time Series Insights에서 대기 시간 완화](../articles/time-series-insights/time-series-insights-environment-mitigate-latency.md)를 참조 하세요.