---
title: Azure IoT Central에 대 한 장치 연결 문자열 생성 | Microsoft Docs
description: 장치 개발자로 서 IoT Central 응용 프로그램에 연결 해야 하는 장치에 대 한 연결 문자열을 생성 하려면 어떻게 해야 하나요?
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 97c0332656b75c3c8d0cddecb41c7a15ac2f218c
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019787"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Azure IoT Central 응용 프로그램에 연결할 장치 연결 문자열을 생성 합니다.

이 문서에서는 장치 개발자가 IoT Central 응용 프로그램에 연결 해야 하는 장치에 대 한 연결 문자열을 생성 하는 방법을 설명 합니다. 이 문서에 설명 된 절차에서는 SAS (공유 액세스 서명)를 사용 하 여 단일 장치를 신속 하 게 연결 하는 방법을 보여 줍니다. 이 방법은 IoT Central 또는 테스트 장치를 시험 하는 경우에 유용 합니다. 프로덕션 환경에서 사용할 수 있는 다른 방법에 대 한 자세한 내용은 [Azure IoT Central의 장치 연결](concepts-connectivity.md)을 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 문서의 단계를 완료하려면 다음이 필요합니다.

- Azure IoT Central 애플리케이션. 자세한 내용은 [애플리케이션 만들기 빠른 시작](quick-deploy-iot-central.md)을 참조하세요.
- [Node.js](https://nodejs.org/) 버전 8.0.0 이상이 설치 된 개발 컴퓨터. 명령줄에서 `node --version` 명령을 실행하여 버전을 확인할 수 있습니다. Node.js는 다양한 운영 체제에 사용할 수 있습니다.

## <a name="get-connection-information"></a>연결 정보 가져오기

다음 단계는 장치에 대 한 SAS 연결 문자열을 생성 하는 데 필요한 정보를 가져오는 방법을 설명 합니다.

1. **Device Explorer**에서 응용 프로그램에 연결 하려는 실제 장치를 찾습니다.

    ![실제 장치 선택](media/howto-generate-connection-string/real-devices.png)

1. **장치** 페이지에서 **연결**을 선택 합니다.

    ![연결 선택](media/howto-generate-connection-string/connect.png)

1. 다음 단계에서 사용할 연결 정보, **범위 ID**, **장치 Id**및 **장치 기본 키**를 적어 둡니다.

    ![연결 세부 정보](media/howto-generate-connection-string/device-connect.png)

    이 페이지에서 값을 복사 하 여 저장할 수 있습니다.

## <a name="generate-the-connection-string"></a>연결 문자열 생성

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>다음 단계

이제 실제 장치에 대 한 연결 문자열을 생성 하 여 Azure IoT Central 응용 프로그램에 연결 했으므로 제안 된 다음 단계는 다음과 같습니다.

* [DevKit 디바이스 준비 및 연결(C)](howto-connect-devkit.md)
* [Raspberry Pi 준비 및 연결(Python)](howto-connect-raspberry-pi-python.md)
* [Raspberry Pi 준비 및 연결(C#)](howto-connect-raspberry-pi-csharp.md)
* [Windows 10 IoT Core 디바이스 준비 및 연결(C#)](howto-connect-windowsiotcore.md)
* [Azure IoT Central 애플리케이션에 일반 Node.js 클라이언트 연결](howto-connect-nodejs.md)