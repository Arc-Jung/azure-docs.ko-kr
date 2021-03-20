---
title: Azure IoT Central 응용 프로그램에서 x.509 인증서를 사용 하 여 장치 연결
description: IoT Central 응용 프로그램용 Node.js 장치 SDK를 사용 하 여 x.509 인증서를 사용 하 여 장치를 연결 하는 방법
author: dominicbetts
ms.author: dobett
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: cf0db71600c9350b4d70e6375f509a6e88709f70
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100378335"
---
# <a name="how-to-connect-devices-with-x509-certificates-using-nodejs-device-sdk-for-iot-central-application"></a>IoT Central 응용 프로그램용 Node.js 장치 SDK를 사용 하 여 x.509 인증서를 사용 하 여 장치를 연결 하는 방법

IoT Central는 SAS (공유 액세스 서명) 및 x.509 인증서를 모두 지원 하 여 장치와 응용 프로그램 간의 통신을 보호 합니다. [클라이언트 응용 프로그램을 만들어 Azure IoT Central 응용 프로그램에 연결](./tutorial-connect-device.md) 자습서에서는 SAS를 사용 합니다. 이 문서에서는 x.509를 사용 하도록 코드 샘플을 수정 하는 방법에 대해 알아봅니다.  X.509 인증서는 프로덕션 환경에서 권장 됩니다. 자세한 내용은 [Azure IoT Central에 연결](./concepts-get-connected.md)을 참조 하세요.

이 문서에서는 일반적으로 프로덕션 환경에서 사용 되는 x.509 [등록](how-to-connect-devices-x509.md#use-a-group-enrollment) 를 사용 하는 두 가지 방법을 보여 주고 [개별 등록](how-to-connect-devices-x509.md#use-an-individual-enrollment) 는 테스트에 유용 합니다.

이 문서의 코드 조각에서는 JavaScript를 사용 합니다. 다른 언어의 코드 샘플은 다음을 참조 하세요.

- [C](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_ll_client_x509_sample)
- [C#](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/iot-hub/Samples/device/X509DeviceCertWithChainSample)
- [Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/send-event-x509)
- [Python](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/sync-samples)

## <a name="prerequisites"></a>필수 구성 요소

- [클라이언트 응용 프로그램을 만들고 Azure IoT Central 응용 프로그램 (JavaScript) 자습서에 연결](./tutorial-connect-device.md) 합니다.
- [Git](https://git-scm.com/download/)
- [OpenSSL](https://www.openssl.org/)를 다운로드 하 여 설치 합니다. Windows를 사용 하는 경우 [SourceForge의 OpenSSL 페이지](https://sourceforge.net/projects/openssl/)에서 이진 파일을 사용할 수 있습니다.

## <a name="use-a-group-enrollment"></a>그룹 등록 사용

프로덕션 환경에서 그룹 등록에 x.509 인증서를 사용 합니다. 그룹 등록에서 루트 또는 중간 x.509 인증서를 IoT Central 응용 프로그램에 추가 합니다. 루트 또는 중간 인증서에서 파생 된 리프 인증서가 있는 장치는 응용 프로그램에 연결할 수 있습니다.

## <a name="generate-root-and-device-cert"></a>루트 및 장치 인증서 생성

이 섹션에서는 x.509 인증서를 사용 하 여 등록 그룹의 cert에서 파생 된 인증서를 사용 하 여 장치를 연결 합니다 .이 인증서는 IoT Central 응용 프로그램에 연결할 수 있습니다.

> [!WARNING]
> X.509 인증서를 생성 하는이 방법은 테스트용 으로만 사용할 수 있습니다. 프로덕션 환경의 경우 인증서를 생성 하는 데 사용 되는 공식 보안 메커니즘을 사용 해야 합니다.

1. 명령 프롬프트를 엽니다. 인증서 생성 스크립트에 대 한 GitHub 리포지토리를 복제 합니다.

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node.git
    ```

1. 인증서 생성기 스크립트로 이동 하 여 필요한 패키지를 설치 합니다.

    ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    npm install
    ```

1. 루트 인증서를 만든 다음 스크립트를 실행 하 여 장치 인증서를 파생 합니다.

    ```cmd/sh
    node create_test_cert.js root mytestrootcert
    node create_test_cert.js device sample-device-01 mytestrootcert
    ```

    > [!TIP]
    > 디바이스 ID는 문자, 숫자 및 `-` 문자를 포함할 수 있습니다.

이러한 명령은 루트 및 장치 인증서 각각에 대해 3 개의 파일을 생성 합니다.

파일 이름 | 내용
-------- | --------
\<name\>_cert. pem | X509 인증서의 공개 부분
\<name\>_key. pem | X509 인증서에 대 한 개인 키
\<name\>_fullchain. pem | X509 인증서에 대 한 전체 키 집합입니다.

## <a name="create-a-group-enrollment"></a>그룹 등록 만들기

1. IoT Central 응용 프로그램을 열고 왼쪽 창에서 **관리**  로 이동 하 여 **장치 연결** 을 선택 합니다.

1. **+ 등록 그룹 만들기** 를 선택 하 고 증명 형식 **인증서 (x.509)** 를 사용 하 여 _MyX509Group_ 라는 새 등록 그룹을 만듭니다.

1. 만든 등록 그룹을 열고 **기본 관리** 를 선택 합니다.

1. 파일 옵션을 선택 하 고 이전에 생성 한 _mytestrootcert_cert. pem_ 이라는 루트 인증서 파일을 업로드 합니다.

    ![인증서 업로드](./media/how-to-connect-devices-x509/certificate-upload.png)

1. 확인을 완료 하려면 확인 코드를 생성 하 고 복사한 후이를 사용 하 여 명령 프롬프트에서 x.509 확인 인증서를 만듭니다.

    ```cmd/sh
    node create_test_cert.js verification --ca mytestrootcert_cert.pem --key mytestrootcert_key.pem --nonce  {verification-code}
    ```

1. 서명 된 확인 verification_cert 인증서를 업로드 하 여 확인을 완료 _합니다._

    ![확인 된 인증서](./media/how-to-connect-devices-x509/verified.png)

이제이 주 루트 인증서에서 파생 된 x.509 인증서가 있는 장치를 연결할 수 있습니다.

등록 그룹을 저장 한 후에는 ID 범위를 기록해 둡니다.

## <a name="run-sample-device-code"></a>샘플 장치 코드 실행

1. **simple_thermostat.js** 응용 프로그램을 포함 하는 **sampleDevice01_key. pem** 및 **sampleDevice01_cert** 를 _azure-iot-sdk-노드/장치/샘플/a p i_ 폴더에 복사 합니다. [장치 연결 (JavaScript) 자습서](./tutorial-connect-device.md)를 완료할 때이 응용 프로그램을 사용 했습니다.

1. **simple_thermostat.js** 응용 프로그램이 포함 된 _azure-iot-sdk-node/device/samples/pnp_ 폴더로 이동 하 고 다음 명령을 실행 하 여 x.509 패키지를 설치 합니다.

    ```cmd/sh
    npm install azure-iot-security-x509 --save
    ```

1. 텍스트 편집기에서 **simple_thermostat.js** 파일을 엽니다.

1. 다음을 `require` 포함 하도록 문을 편집 합니다.

    ```javascript
    const fs = require('fs');
    const X509Security = require('azure-iot-security-x509').X509Security;
    ```

1. "DPS 연결 정보" 섹션에 다음 네 줄을 추가 하 여 변수를 초기화 합니다 `deviceCert` .

    ```javascript
    const deviceCert = {
      cert: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_CERT).toString(),
      key: fs.readFileSync(process.env.IOTHUB_DEVICE_X509_KEY).toString()
    };
    ```

1. `provisionDevice`첫 번째 줄을 다음으로 바꿔 클라이언트를 만드는 함수를 편집 합니다.

    ```javascript
    var provSecurityClient = new X509Security(registrationId, deviceCert);
    ```

1. 동일한 함수에서 다음과 같이 변수를 설정 하는 줄을 수정 합니다 `deviceConnectionString` .

    ```javascript
    deviceConnectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';x509=true';
    ```

1. 함수에서 `main` 를 호출 하는 줄 뒤에 다음 줄을 추가 합니다 `Client.fromConnectionString` .

    ```javascript
    client.setOptions(deviceCert);
    ```

1. 셸 환경에서 다음 두 가지 환경 변수를 설정 합니다.

    ```cmd/sh
    set IOTHUB_DEVICE_X509_CERT=sampleDevice01_cert.pem
    set IOTHUB_DEVICE_X509_KEY=sampleDevice01_key.pem
    ```

    > [!TIP]
    > [Azure IoT Central 응용 프로그램에 클라이언트 응용 프로그램 만들기 및 연결](./tutorial-connect-device.md) 자습서를 완료할 때 다른 필수 환경 변수를 설정 합니다.

1. 스크립트를 실행 하 고 장치가 성공적으로 프로 비전 되었는지 확인 합니다.

    ```cmd/sh
    node simple_thermostat.js
    ```

    원격 분석이 대시보드에 나타나는지 확인할 수도 있습니다.

    ![장치 원격 분석 확인](./media/how-to-connect-devices-x509/telemetry.png)

## <a name="use-an-individual-enrollment"></a>개별 등록 사용

개별 등록에 x.509 인증서를 사용 하 여 장치와 솔루션을 테스트 합니다. 개별 등록에서는 IoT Central 응용 프로그램에 루트 또는 중간 X. x.509 인증서가 없습니다. 장치는 자체 서명 된 x.509 인증서를 사용 하 여 응용 프로그램에 연결 합니다.

## <a name="generate-self-signed-device-cert"></a>자체 서명 된 장치 인증서 생성

이 섹션에서는 자체 서명 된 x.509 인증서를 사용 하 여 개별 등록을 위해 장치를 연결 합니다 .이는 단일 장치를 등록 하는 데 사용 됩니다. 자체 서명 된 인증서는 테스트용 으로만 사용할 수 있습니다.

스크립트를 실행 하 여 자체 서명 된 x.509 장치 인증서를 만듭니다. 인증서 이름에는 소문자 영숫자 및 하이픈만 사용 해야 합니다.

  ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    node create_test_cert.js device mytestselfcertprimary
    node create_test_cert.js device mytestselfcertsecondary 
  ```

## <a name="create-individual-enrollment"></a>개별 등록 만들기

1. Azure IoT Central 응용 프로그램에서 **장치** 를 선택 하 고 자동 온도 조절기 장치 템플릿에서 _MYTESTSELFCERTPRIMARY_ 로 **장치 ID** 를 사용 하 여 새 장치를 만듭니다. **ID 범위** 를 기록해 둡니다. 나중에 사용 합니다.

1. 만든 장치를 열고 **연결** 을 선택 합니다.

1. **Connect 방법** 으로 **개별 등록** 를 선택 하 고 메커니즘으로 **인증서 (x.509)** 를 선택 합니다.

    ![개별 등록](./media/how-to-connect-devices-x509/individual-device-connect.png)

1. 기본에서 파일 옵션을 선택 하 고 이전에 생성 한 _mytestselfcertprimary_cert. pem_ 이라는 인증서 파일을 업로드 합니다.

1. 보조 인증서에 대 한 파일 옵션을 선택 하 고 _mytestselfcertsecondary_cert. pem_ 이라는 인증서 파일을 업로드 합니다. 그런 다음, **저장** 을 선택 합니다.

    ![개별 등록 인증서 업로드](./media/how-to-connect-devices-x509/individual-enrollment.png)

이제 장치가 x.509 인증서로 프로 비전 됩니다.

## <a name="run-a-sample-individual-enrollment-device"></a>샘플 개별 등록 장치 실행

1. **simple_thermostat.js** 응용 프로그램을 포함 하는 _mytestselfcertprimary_key. pem_ 및 _mytestselfcertprimary_cert_ 를 _azure-iot-sdk-노드/장치/샘플/a p i_ 폴더에 복사 합니다. [장치 연결 (JavaScript) 자습서](./tutorial-connect-device.md)를 완료할 때이 응용 프로그램을 사용 했습니다.

1. 위의 샘플에서 사용한 환경 변수를 다음과 같이 수정 합니다.

    ```cmd/sh
    set IOTHUB_DEVICE_DPS_DEVICE_ID=mytestselfcertprimary
    set IOTHUB_DEVICE_X509_CERT=mytestselfcertprimary_cert.pem
    set IOTHUB_DEVICE_X509_KEY=mytestselfcertprimary_key.pem
    ```

1. 스크립트를 실행 하 고 장치가 성공적으로 프로 비전 되었는지 확인 합니다.

    ```cmd/sh
    node environmentalSensor.js
    ```

    원격 분석이 대시보드에 나타나는지 확인할 수도 있습니다.

    ![원격 분석 개별 등록](./media/how-to-connect-devices-x509/telemetry-primary.png)

_Mytestselfcertsecondary_ certificate에 대해서도 위의 단계를 반복할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이제 x.509 인증서를 사용 하 여 장치를 연결 하는 방법을 배웠으므로 제안 된 다음 단계는 [Azure CLI를 사용 하 여 장치 연결을 모니터링](howto-monitor-devices-azure-cli.md) 하는 방법을 배우는 것입니다.
