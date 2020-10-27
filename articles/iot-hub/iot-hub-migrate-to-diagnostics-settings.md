---
title: Azure IoT Hub를 진단 설정으로 마이그레이션 | Microsoft Docs
description: 작업 모니터링 대신 Azure 진단 설정을 사용하여 IoT Hub의 작업 상태를 실시간으로 모니터링하도록 Azure IoT Hub를 업데이트하는 방법을 설명합니다.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: kgremban
ms.openlocfilehash: 40c90142330b0530f1127beae1624ff27d7eb6ca
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92541488"
---
# <a name="migrate-your-iot-hub-from-operations-monitoring-to-diagnostics-settings"></a>Azure IoT Hub를 작업 모니터링에서 진단 설정으로 마이그레이션

[작업 모니터링](iot-hub-operations-monitoring.md)을 사용하여 IoT Hub의 작업 상태를 추적하는 고객은 해당 워크플로를 Azure Monitor의 기능인 [Azure 진단 설정](../azure-monitor/platform/platform-logs-overview.md)으로 마이그레이션할 수 있습니다. 진단 설정은 여러 Azure 서비스에 대한 리소스 수준의 진단 정보를 제공합니다.

**IoT Hub의 작업 모니터링 기능은 더 이상 사용** 되지 않으며 포털에서 제거 되었습니다. 이 문서에서는 워크로드를 작업 모니터링에서 진단 설정으로 이동하는 단계를 설명합니다. 사용 중단 타임라인에 대한 자세한 내용은 [Azure Monitor 및 Azure Resource Health로 Azure IoT 솔루션 모니터링](https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health/)을 참조하세요.

## <a name="update-iot-hub"></a>IoT Hub 업데이트

Azure Portal에서 IoT Hub를 업데이트하려면 먼저 진단 설정을 켠 다음 작업 모니터링을 꺼야 합니다.  

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="turn-off-operations-monitoring"></a>작업 모니터링 끄기

> [!NOTE]
> 2019 년 3 월 11 일부 터 IoT Hub의 Azure Portal 인터페이스에서 작업 모니터링 기능이 제거 됩니다. 아래 단계는 더 이상 적용 되지 않습니다. 마이그레이션하려면 위의 Azure Monitor 진단 설정에서 올바른 범주가 설정 되어 있는지 확인 합니다.

워크플로에서 새 진단 설정을 테스트 한 후에는 작업 모니터링 기능을 해제할 수 있습니다. 

1. IoT Hub 메뉴에서 **작업 모니터링** 을 선택합니다.

2. 각 모니터링 범주에서 **없음** 을 선택합니다.

3. 작업 모니터링 변경 내용을 저장합니다.

## <a name="update-applications-that-use-operations-monitoring"></a>작업 모니터링을 사용하는 애플리케이션 업데이트

작업 모니터링과 진단 설정의 스키마는 약간 다릅니다. 오늘은 작업 모니터링에서 사용하는 애플리케이션을 업데이트하여 진단 설정에서 사용하는 스키마로 매핑하게 만드는 것이 중요합니다. 

또한 진단 설정은 추적을 위한 5 가지 새 범주를 제공 합니다. 기존 스키마에 대한 애플리케이션을 업데이트한 후에는 새 범주를 추가합니다.

* 클라우드-디바이스 쌍 작업
* 디바이스-클라우드 쌍 작업
* 쌍 쿼리
* 작업 연산
* 직접 메서드

특정 스키마 구조는 [리소스 로그](monitor-iot-hub-reference.md#resource-logs)를 참조 하세요.

## <a name="monitoring-device-connect-and-disconnect-events-with-low-latency"></a>디바이스 연결 모니터링 및 낮은 대기 시간을 사용하여 이벤트 연결 해제

프로덕션에서 장치 연결 및 연결 끊기 이벤트를 모니터링 하려면 경고를 받고 장치 연결 상태를 모니터링 하는 Event Grid에서 [ **장치 연결이 끊긴** 이벤트](iot-hub-event-grid.md#event-types) 를 구독 하는 것이 좋습니다. 이 [자습서](iot-hub-how-to-order-connection-state-events.md)를 사용하여 IoT 솔루션에서 IoT Hub의 디바이스 연결된 이벤트 및 디바이스 연결 해제된 이벤트를 통합하는 방법을 알아봅니다.

## <a name="next-steps"></a>다음 단계

[모니터 IoT Hub](monitor-iot-hub.md)
