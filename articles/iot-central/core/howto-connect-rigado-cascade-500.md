---
title: Azure IoT Central에서 Rigado Cascade 500 연결 | Microsoft Docs
description: IoT Central 응용 프로그램에 Rigado Cascade 500 게이트웨이 장치를 연결 하는 방법에 대해 알아봅니다.
services: iot-central
ms.service: iot-central
ms.topic: how-to
ms.custom:
- iot-storeAnalytics-conditionMonitor
- iot-p0-scenario
ms.author: avneets
author: avneet723
ms.date: 11/27/2019
ms.openlocfilehash: 0000e7690ab92f469a7417e82cb375c524e0b343
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96014765"
---
# <a name="connect-a-rigado-cascade-500-gateway-device-to-your-azure-iot-central-application"></a>Rigado Cascade 500 게이트웨이 장치를 Azure IoT Central 응용 프로그램에 연결

이 문서는 솔루션 빌더에 적용됩니다.

이 문서에서는 솔루션 빌더를 통해 Microsoft Azure IoT Central 응용 프로그램에 Rigado Cascade 500 게이트웨이 장치를 연결할 수 있는 방법을 설명 합니다. 

## <a name="what-is-cascade-500"></a>Cascade 500 이란?

Cascade 500 IoT gateway는 Rigado에서 제공 하는 하드웨어 제품으로, Cascade 최첨단 서비스 솔루션의 일부로 포함 되어 있습니다. 유연한 edge 컴퓨팅 기능, 강력한 컨테이너 화 된 응용 프로그램 환경 및 Bluetooth 5, LTE & Wi-fi를 비롯 한 다양 한 무선 장치 연결 옵션을 제공 하는 상용 IoT 프로젝트 및 제품 팀을 제공 합니다.

Cascade 500은 솔루션 빌더가 종단 간 솔루션에 장치를 쉽게 등록할 수 있도록 Azure IoT 플러그 앤 플레이 (미리 보기)를 미리 인증 합니다. Cascade gateway를 사용 하면 게이트웨이 장치와 근접 한 다양 한 조건 모니터링 센서에 무선으로 연결할 수 있습니다. 이러한 센서는 게이트웨이 장치를 통해 IoT Central에 등록 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소
이 방법 가이드를 단계별로 실행 하려면 다음 리소스가 필요 합니다.

* Rigado Cascade 500 장치입니다. 자세한 내용은 [Rigado](https://www.rigado.com/)를 참조 하세요.
* Azure IoT Central 애플리케이션. 자세한 내용은 [새 응용 프로그램 만들기](./quick-deploy-iot-central.md)를 참조 하세요.

## <a name="add-a-device-template"></a>디바이스 템플릿 추가하기

캐스케이드 500 게이트웨이 장치를 Azure IoT Central 응용 프로그램 인스턴스에 등록 하려면 응용 프로그램 내에서 해당 하는 장치 템플릿을 구성 해야 합니다.

캐스케이드 500 장치 템플릿을 추가 하려면: 

1. 왼쪽 창에서 ***장치 템플릿** _ 탭으로 이동 하 여 _ * + 새로 만들기 * *: ![ 새 장치 템플릿 만들기를 선택 합니다.](./media/howto-connect-rigado-cascade-500/device-template-new.png)
1. 이 페이지에서는 **사용자 지정 템플릿 만들기** _ 또는 *_미리 구성 된 장치 템플릿 사용_* 에 대 한 옵션을 제공 합니다.*
1. 아래와 같이 미리 구성 된 장치 템플릿 목록에서 C500 장치 템플릿을 선택 합니다. ![ C500 장치 템플릿 선택](./media/howto-connect-rigado-cascade-500/device-template-preconfigured.png)
1. 다음 ***: 사용자 지정*** 을 선택 하 여 다음 단계를 계속 합니다. 
1. 다음 화면에서 ***만들기*** 를 선택 하 여 IoT Central 응용 프로그램에 C500 장치 템플릿을 등록 합니다.

## <a name="retrieve-application-connection-details"></a>응용 프로그램 연결 정보 검색

이제 캐스케이드 500 장치를 연결 하기 위해 Azure IoT Central 응용 프로그램에 대 한 **범위 ID** 및 **기본 키** 를 검색 해야 합니다. 

1. 왼쪽 창에서 **관리**  로 이동 하 여 **장치 연결** 을 클릭 합니다. 
2. IoT Central 응용 프로그램의 **범위 ID** 를 적어 둡니다.
![앱 범위 ID](./media/howto-connect-rigado-cascade-500/app-scope-id.png)
3. 이제 **키 보기** 를 클릭 하 고 **기본 키** 
 ![ 기본 키를 적어 둡니다.](./media/howto-connect-rigado-cascade-500/primary-key-sas.png)  

## <a name="contact-rigado-to-connect-the-gateway"></a>게이트웨이를 연결 하려면 Rigado에 문의 하세요. 

Cascade 500 장치를 IoT Central 응용 프로그램에 연결 하려면 Rigado에 문의 하 고 위의 단계에서 제공 하는 응용 프로그램 연결 세부 정보를 제공 해야 합니다. 

장치가 인터넷에 연결 되 면 Rigado은 보안 채널을 통해 케스케이드 500 게이트웨이 장치에 구성 업데이트를 푸시할 수 있습니다. 

이 업데이트는 캐스케이드 500 장치에 IoT Central 연결 정보를 적용 하 고 장치 목록에 표시 됩니다. 

![디바이스 목록](./media/howto-connect-rigado-cascade-500/devices-list-c500.png)  

이제 IoT Central 응용 프로그램에서 C500 장치를 사용할 준비가 되었습니다.

## <a name="next-steps"></a>다음 단계

디바이스 개발자라면 다음과 같은 몇 가지 단계를 살펴보세요.

- [Azure IoT Central에서 디바이스 연결](./concepts-get-connected.md)에 대해 알아봅니다.
- [Azure CLI를 사용하여 디바이스 연결을 모니터링](./howto-monitor-devices-azure-cli.md)하는 방법을 알아봅니다.
