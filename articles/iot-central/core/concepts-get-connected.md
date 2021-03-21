---
title: Azure IoT Central에서 디바이스 연결 | Microsoft Docs
description: 이 문서에서는 Azure IoT Central의 디바이스 연결과 관련된 주요 개념을 소개합니다.
author: dominicbetts
ms.author: dobett
ms.date: 1/15/2020
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.custom:
- amqp
- mqtt
- device-developer
ms.openlocfilehash: dc0655aba424d29a4055f0d50a20057f22d084ed
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103015458"
---
# <a name="get-connected-to-azure-iot-central"></a>Azure IoT Central에 연결

*이 문서는 운영자 및 장치 개발자에 게 적용 됩니다.*

이 문서에서는 장치가 Azure IoT Central 응용 프로그램에 연결 하는 방법을 설명 합니다. 장치가 IoT Central와 데이터를 교환 하려면 먼저 다음을 수행 해야 합니다.

- *인증*. IoT Central 응용 프로그램 인증에서는 _SAS (공유 액세스 서명) 토큰_ 또는 _x.509 인증서_ 를 사용 합니다. X.509 인증서는 프로덕션 환경에서 권장 됩니다.
- *등록*. IoT Central 응용 프로그램에 장치를 등록 해야 합니다. 응용 프로그램의 **장치** 페이지에서 등록 된 장치를 볼 수 있습니다.
- *장치 템플릿에 연결* 합니다. IoT Central 응용 프로그램에서 장치 템플릿은 운영자가 연결 된 장치를 보고 관리 하는 데 사용 하는 UI를 정의 합니다.

IoT Central는 다음과 같은 두 가지 장치 등록 시나리오를 지원 합니다.

- *자동 등록*. 장치는 처음 연결 될 때 자동으로 등록 됩니다. 이 시나리오에서는 Oem이 등록을 먼저 하지 않고 연결할 수 있는 대용량 제조업체 장치를 사용할 수 있습니다. OEM은 적절 한 장치 자격 증명을 생성 하 고 팩터리에서 장치를 구성 합니다. 필요에 따라 데이터 보내기를 시작 하기 전에 운영자가 장치를 승인 하도록 요구할 수 있습니다. 이 시나리오에서는 응용 프로그램에서 x.509 또는 SAS _그룹 등록_ 을 구성 해야 합니다.
- *수동 등록*. 운영자는 **장치** 페이지에서 개별 장치를 등록 하거나 [CSV 파일을 가져와서](howto-manage-devices.md#import-devices) 대량 등록 장치를 등록 합니다. 이 시나리오에서는 x.509 또는 SAS _그룹 등록_, X.509 또는 sas _개별 등록_ 을 사용할 수 있습니다.

IoT Central에 연결 하는 장치는 *IoT 플러그 앤 플레이 규칙* 을 따라야 합니다. 이러한 규칙 중 하나는 장치가 연결 될 때 구현 하는 장치 모델의 _모델 ID_ 를 전송 해야 한다는 것입니다. 모델 ID를 사용 하 여 IoT Central 응용 프로그램에서 장치를 올바른 장치 템플릿에 연결할 수 있습니다.

IoT Central는 [DPS (Azure IoT Hub 장치 프로 비전 서비스)](../../iot-dps/about-iot-dps.md) 를 사용 하 여 연결 프로세스를 관리 합니다. 장치는 먼저 DPS 끝점에 연결 하 여 응용 프로그램에 연결 하는 데 필요한 정보를 검색 합니다. 내부적으로 IoT Central 응용 프로그램은 IoT hub를 사용 하 여 장치 연결을 처리 합니다. DPS를 사용하면 다음과 같은 장점이 있습니다.

- IoT Central이 디바이스를 대규모로 온보딩하고 연결하는 것을 지원합니다.
- IoT Central UI를 통해 디바이스를 등록할 필요 없이 오프라인으로 디바이스 자격 증명을 생성하고 디바이스를 구성할 수 있습니다.
- 사용자의 고유한 디바이스 ID를 사용하여 IoT Central에서 디바이스를 등록할 수 있습니다. 사용자의 고유한 디바이스 ID를 사용하면 기존 백 오피스 시스템과 간편하게 통합할 수 있습니다.
- 디바이스를 IoT Central에 연결하는 한 가지 일관적인 방법이 있습니다.

이 문서에서는 다음 장치 연결 단계에 대해 설명 합니다.

- [X.509 그룹 등록](#x509-group-enrollment)
- [SAS 그룹 등록](#sas-group-enrollment)
- [개별 등록](#individual-enrollment)
- [디바이스 등록](#device-registration)
- [장치 템플릿과 장치 연결](#associate-a-device-with-a-device-template)

## <a name="x509-group-enrollment"></a>X.509 그룹 등록

프로덕션 환경에서 x.509 인증서를 사용 하는 것이 IoT Central에 권장 되는 장치 인증 메커니즘입니다. 자세한 내용은 [X.509 CA 인증서를 사용하여 디바이스 인증](../../iot-hub/iot-hub-x509ca-overview.md)을 참조하세요.

X.509 인증서가 있는 장치를 응용 프로그램에 연결 하려면 다음을 수행 합니다.

1. **인증서 (x.509)** 증명 유형을 사용 하는 *등록 그룹* 을 만듭니다.
1. 등록 그룹에서 중간 또는 루트 x.509 인증서를 추가 하 고 확인 합니다.
1. 등록 그룹의 루트 또는 중간 인증서에서 리프 인증서를 생성 합니다. 응용 프로그램에 연결할 때 장치에서 리프 인증서를 보냅니다.

자세히 알아보려면 [x.509 인증서를 사용 하 여 장치를 연결 하는 방법](how-to-connect-devices-x509.md) 을 참조 하세요.

### <a name="for-testing-purposes-only"></a>테스트 목적 으로만 사용

테스트 전용으로 다음 유틸리티를 사용 하 여 루트, 중간 및 장치 인증서를 생성할 수 있습니다.

- [Azure IoT 장치 프로 비전 장치 SDK에 대 한 도구](https://github.com/Azure/azure-iot-sdk-node/blob/master/provisioning/tools/readme.md): x.509 인증서 및 키를 생성 및 확인 하는 데 사용할 수 있는 도구 Node.js 모음입니다.
- DevKit 장치를 사용 하는 경우이 [명령줄 도구](https://aka.ms/iotcentral-docs-dicetool) 는 인증서를 확인 하기 위해 IoT Central 응용 프로그램에 추가할 수 있는 CA 인증서를 생성 합니다.
- [샘플 및 자습서에 대 한 테스트 CA 인증서 관리](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md): 다음에 대 한 PowerShell 및 Bash 스크립트 컬렉션입니다.
  - 인증서 체인을 만듭니다.
  - 인증서를 .cer 파일로 저장 하 여 IoT Central 응용 프로그램에 업로드 합니다.
  - IoT Central 응용 프로그램의 확인 코드를 사용 하 여 확인 인증서를 생성 합니다.
  - 장치 Id를 도구에 대 한 매개 변수로 사용 하 여 장치에 대 한 리프 인증서를 만듭니다.

## <a name="sas-group-enrollment"></a>SAS 그룹 등록

장치 SAS 키가 있는 장치를 응용 프로그램에 연결 하려면 다음을 수행 합니다.

1. **SAS (공유 액세스 서명)** 증명 유형을 사용 하는 *등록 그룹* 을 만듭니다.
1. 등록 그룹에서 그룹 기본 또는 보조 키를 복사 합니다.
1. Azure CLI를 사용 하 여 그룹 키에서 장치 키를 생성 합니다.

    ```azurecli
    az iot central device compute-device-key --primary-key <enrollment group primary key> --device-id <device ID>
    ```

1. 장치가 IoT Central 응용 프로그램에 연결 될 때 생성 된 장치 키를 사용 합니다.

## <a name="individual-enrollment"></a>개별 등록

각각 고유한 인증 자격 증명이 있는 장치를 연결 하는 고객은 개별 등록를 사용 합니다. 개별 등록은 연결할 수 있는 단일 장치에 대 한 항목입니다. 개별 등록는 x.509 리프 인증서 또는 SAS 토큰 (실제 또는 가상의 신뢰할 수 있는 플랫폼 모듈)을 증명 메커니즘으로 사용할 수 있습니다. 디바이스 ID는 문자, 숫자 및 `-` 문자를 포함할 수 있습니다. 자세한 내용은 [DPS 개별 등록](../../iot-dps/concepts-service.md#individual-enrollment)을 참조 하세요.

> [!NOTE]
> 장치에 대 한 개별 등록을 만들 때 IoT Central 응용 프로그램의 기본 그룹 등록 옵션 보다 우선 적용 됩니다.

### <a name="create-individual-enrollments"></a>개별 등록 만들기

IoT Central은 개별 등록 다음과 같은 증명 메커니즘을 지원 합니다.

- **대칭 키 증명:** 대칭 키 증명은 DPS 인스턴스를 사용 하 여 장치를 인증 하는 간단한 방법입니다. 대칭 키를 사용 하는 개별 등록을 만들려면 장치에 대 한 **장치 연결** 페이지를 열고, **개별 등록** 을 연결 방법으로, **공유 액세스 서명 (SAS)** 을 메커니즘으로 선택 합니다. Base64 인코딩 기본 키 및 보조 키를 입력 하 고 변경 내용을 저장 합니다. **Id 범위**, **장치 id** 및 기본 또는 보조 키 중 하나를 사용 하 여 장치를 연결 합니다.

    > [!TIP]
    > 테스트를 위해 **OpenSSL** 를 사용 하 여 b a s e 64로 인코드된 키를 생성할 수 있습니다. `openssl rand -base64 64`

- **X.509 인증서:** X.509 인증서를 사용 하 여 개별 등록을 만들려면 **장치 연결** 페이지에서 **개별 등록** 을 연결 방법으로 선택 하 고 **인증서 (x.509)** 를 메커니즘으로 선택 합니다. 개별 등록 항목에 사용 되는 장치 인증서에는 발급자와 주체 CN이 장치 ID로 설정 되어 있어야 합니다.

    > [!TIP]
    > 테스트를 위해 [Node.js용 Azure IoT 장치 프로 비전 장치 SDK 용 도구 ](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/tools) 를 사용 하 여 자체 서명 된 인증서를 생성할 수 있습니다. `node create_test_cert.js device "mytestdevice"`

- **TPM (신뢰할 수 있는 플랫폼 모듈) 증명:** [TPM](../../iot-dps/concepts-tpm-attestation.md) 은 하드웨어 보안 모듈의 한 유형입니다. TPM을 사용 하는 것은 장치를 가장 안전 하 게 연결 하는 방법 중 하나입니다. 이 문서에서는 불연속, 펌웨어 또는 통합 TPM을 사용 하 고 있다고 가정 합니다. 소프트웨어 에뮬레이트된 Tpm은 프로토타입 또는 테스트에 적합 하지만 불연속, 펌웨어 또는 통합 된 Tpm과 동일한 수준의 보안을 제공 하지 않습니다. 프로덕션 환경에서 소프트웨어 Tpm을 사용 하지 마세요. TPM을 사용 하는 개별 등록을 만들려면 **장치 연결** 페이지를 열고 **개별 등록** 을 연결 방법으로 선택 하 고 **TPM** 을 메커니즘으로 선택 합니다. TPM 인증 키를 입력 하 고 장치 연결 정보를 저장 합니다.

## <a name="device-registration"></a>디바이스 등록

장치를 IoT Central 응용 프로그램에 연결 하려면 먼저 응용 프로그램에 등록 해야 합니다.

- 장치는 처음 연결 될 때 자동으로 등록 될 수 있습니다. 이 옵션을 사용 하려면 [x.509 그룹 등록](#x509-group-enrollment) 또는 [SAS 그룹 등록](#sas-group-enrollment)중 하나를 사용 해야 합니다.
- 운영자는 CSV 파일을 가져와서 응용 프로그램에서 장치 목록을 대량 등록할 수 있습니다.
- 운영자는 응용 프로그램의 **장치** 페이지에서 개별 장치를 수동으로 등록할 수 있습니다.

IoT Central를 사용 하면 Oem에서 자동으로 등록할 수 있는 대용량 제조업체 장치를 사용할 수 있습니다. OEM은 적절 한 장치 자격 증명을 생성 하 고 팩터리에서 장치를 구성 합니다. 고객이 처음으로 장치를 켜면 DPS에 연결 하 여 장치를 올바른 IoT Central 응용 프로그램에 자동으로 연결 합니다. 필요에 따라 응용 프로그램에 데이터를 보내기 전에 운영자가 장치를 승인 하도록 요구할 수 있습니다.

> [!TIP]
> **관리 > 장치 연결** 페이지에서 **자동 승인** 옵션은 운영자가 데이터 보내기를 시작 하기 전에 장치를 수동으로 승인 해야 하는지 여부를 제어 합니다.

### <a name="automatically-register-devices-that-use-x509-certificates"></a>X.509 인증서를 사용 하는 장치를 자동으로 등록

1. [X.509 등록 그룹](#x509-group-enrollment)에 추가한 루트 또는 중간 인증서를 사용 하 여 장치에 대 한 리프 인증서를 생성 합니다. 리프 인증서의로 장치 Id를 사용 합니다 `CNAME` . 디바이스 ID는 문자, 숫자 및 `-` 문자를 포함할 수 있습니다.

1. OEM으로 장치 ID, 생성 된 x.509 리프 인증서 및 응용 프로그램 **ID 범위** 값을 사용 하 여 각 장치를 플래시 합니다. 장치 코드는 구현 하는 장치 모델의 모델 ID도 전송 해야 합니다.

1. 장치를 전환 하면 먼저 DPS에 연결 하 여 IoT Central 연결 정보를 검색 합니다.

1. 장치는 DPS의 정보를 사용 하 여 IoT Central 응용 프로그램에 연결 하 고 등록 합니다.

IoT Central 응용 프로그램은 장치에서 보낸 모델 ID를 사용 하 여 [등록 된 장치를 장치 템플릿에 연결](#associate-a-device-with-a-device-template)합니다.

### <a name="automatically-register-devices-that-use-sas-tokens"></a>SAS 토큰을 사용 하는 장치를 자동으로 등록

1. **SAS-IoT-장치** 등록 그룹에서 그룹 기본 키를 복사 합니다.

    :::image type="content" source="media/concepts-get-connected/group-primary-key.png" alt-text="SAS의 그룹 기본 키-IoT-장치 등록 그룹":::

1. 명령을 사용 `az iot central device compute-device-key` 하 여 장치 SAS 키를 생성 합니다. 이전 단계의 그룹 기본 키를 사용 합니다. 장치 ID에는 문자, 숫자 및 문자를 사용할 수 있습니다 `-` .

    ```azurecli
    az iot central device compute-device-key --primary-key <enrollment group primary key> --device-id <device ID>
    ```

1. OEM으로 장치 ID, 생성 된 장치 SAS 키 및 응용 프로그램 **ID 범위** 값을 사용 하 여 각 장치를 플래시 합니다. 장치 코드는 구현 하는 장치 모델의 모델 ID도 전송 해야 합니다.

1. 장치를 전환 하면 먼저 DPS에 연결 하 여 IoT Central 등록 정보를 검색 합니다.

1. 장치는 DPS의 정보를 사용 하 여 IoT Central 응용 프로그램에 연결 하 고 등록 합니다.

IoT Central 응용 프로그램은 장치에서 보낸 모델 ID를 사용 하 여 [등록 된 장치를 장치 템플릿에 연결](#associate-a-device-with-a-device-template)합니다.

### <a name="bulk-register-devices-in-advance"></a>장치를 미리 대량 등록

IoT Central 응용 프로그램으로 많은 수의 장치를 등록 하려면 CSV 파일을 사용 하 여 [장치 id 및 장치 이름을 가져옵니다](howto-manage-devices.md#import-devices).

장치가 SAS 토큰을 사용 하 여 인증 하 [는 경우 IoT Central 응용 프로그램에서 CSV 파일을 내보냅니다](howto-manage-devices.md#export-devices). 내보낸 CSV 파일에는 장치 Id와 SAS 키가 포함 되어 있습니다.

장치에서 x.509 인증서를 사용 하 여 인증 하는 경우 x.509 등록 그룹에 업로드 한에서 루트 또는 중간 인증서를 사용 하 여 장치에 대 한 x.509 리프 인증서를 생성 합니다. 리프 인증서의 값으로 가져온 장치 Id를 사용 `CNAME` 합니다.

장치는 응용 프로그램에 대 한 **Id 범위** 값을 사용 하 고 연결할 때 모델 id를 보내야 합니다.

> [!TIP]
> **관리 > 장치 연결** 에서 **ID 범위** 값을 찾을 수 있습니다.

### <a name="register-a-single-device-in-advance"></a>단일 장치를 미리 등록

이 방법은 IoT Central 또는 테스트 장치를 시험 하는 경우에 유용 합니다. **장치** 페이지에서 **+ 새로 만들기** 를 선택 하 여 개별 장치를 등록 합니다. 장치 연결 SAS 키를 사용 하 여 장치를 IoT Central 응용 프로그램에 연결할 수 있습니다. 등록 된 장치에 대 한 연결 정보에서 _장치 SAS 키_ 를 복사 합니다.

![개별 장치에 대 한 SAS 키](./media/concepts-get-connected/single-device-sas.png)

## <a name="associate-a-device-with-a-device-template"></a>장치 템플릿과 장치 연결

장치를 연결할 때 장치가 장치 템플릿에 자동으로 연결 IoT Central. 장치는 연결 될 때 [모델 ID](../../iot-fundamentals/iot-glossary.md?toc=/azure/iot-central/toc.json&bc=/azure/iot-central/breadcrumb/toc.json#model-id) 를 보냅니다. IoT Central는 모델 ID를 사용 하 여 특정 장치 모델에 대 한 장치 템플릿을 식별 합니다. 검색 프로세스는 다음과 같이 작동 합니다.

1. 장치 템플릿이 IoT Central 응용 프로그램에 이미 게시 된 경우 장치는 장치 템플릿과 연결 됩니다.
1. 장치 템플릿이 IoT Central 응용 프로그램에 아직 게시 되지 않은 경우 IoT Central는 [공용 모델 리포지토리에서](https://github.com/Azure/iot-plugandplay-models)장치 모델을 찾습니다. IoT Central 모델을 찾은 경우이 모델을 사용 하 여 기본 장치 템플릿을 생성 합니다.
1. IoT Central 공용 모델 리포지토리에서 모델을 찾을 수 없는 경우 장치가 연결 되지 않은 것으로 표시 **됩니다.** 운영자는 장치에 대 한 장치 템플릿을 만든 다음 연결 되지 않은 장치를 새 장치 템플릿으로 마이그레이션할 수 있습니다.

다음 스크린샷은 IoT Central에서 장치 템플릿의 모델 ID를 보는 방법을 보여 줍니다. 장치 템플릿에서 구성 요소를 선택한 다음, **Id 보기** 를 선택 합니다.

:::image type="content" source="media/concepts-get-connected/model-id.png" alt-text="자동 온도 조절기 장치 템플릿의 모델 ID를 보여 주는 스크린샷":::

공용 모델 리포지토리에서 [자동 온도 조절기 모델](https://github.com/Azure/iot-plugandplay-models/blob/main/dtmi/com/example/thermostat-1.json) 을 볼 수 있습니다. 모델 ID 정의는 다음과 같습니다.

```json
"@id": "dtmi:com:example:Thermostat;1"
```

## <a name="device-status-values"></a>장치 상태 값

실제 장치가 IoT Central 응용 프로그램에 연결 되 면 장치 상태가 다음과 같이 변경 됩니다.

1. 장치 상태는 먼저 **등록** 됩니다. 이 상태는 장치가 IoT Central에서 만들어지고 장치 ID가 있음을 의미 합니다. 장치는 다음과 같은 경우에 등록 됩니다.
    - **장치** 페이지에 새 실제 장치가 추가 됩니다.
    - **장치 페이지에서** **가져오기** 를 사용 하 여 일련의 장치를 추가 합니다.

1. 유효한 자격 증명을 사용 하 여 IoT Central 응용 프로그램에 연결 된 장치에서 프로 비전 단계가 완료 되 면 장치 상태가 **프로 비전** 됨으로 변경 됩니다. 이 단계에서 장치는 DPS를 사용 하 여 IoT Central 응용 프로그램에서 사용 하는 IoT Hub 연결 문자열을 자동으로 검색 합니다. 이제 장치에서 IoT Central에 연결 하 여 데이터 보내기를 시작할 수 있습니다.

1. 운영자는 장치를 차단할 수 있습니다. 장치가 차단 되 면 데이터를 IoT Central 응용 프로그램으로 보낼 수 없습니다. 차단 된 장치의 상태가 **차단 됨** 입니다. 운영자는 데이터 보내기를 다시 시작 하기 전에 장치를 다시 설정 해야 합니다. 운영자가 장치를 차단 해제 하면 상태가 이전 값으로 반환 되 고 **등록** 되거나 **프로 비전** 됩니다.

1. 장치 상태가 **승인을 기다리고** 있으면 **자동 승인** 옵션이 사용 되지 않도록 설정 된 것입니다. 운영자는 데이터 보내기를 시작 하기 전에 장치를 명시적으로 승인 해야 합니다. **장치 페이지에** 수동으로 등록 되지 않았지만 유효한 자격 증명으로 연결 된 장치는 **승인 대기 중인** 장치 상태를 가집니다. 운영자는 **승인** 단추를 사용 하 여 **장치** 페이지에서 이러한 장치를 승인할 수 있습니다.

1. 장치 상태가 **연결 되지 않음 인 경우** IoT Central 연결 하는 장치에 연결 된 장치 템플릿이 없음을 의미 합니다. 이 상황은 일반적으로 다음과 같은 시나리오에서 발생 합니다.

    - 장치 템플릿을 지정 하지 않고 **장치 페이지에서** **가져오기** 를 사용 하 여 일련의 장치를 추가 합니다.
    - 장치 템플릿을 지정 하지 않고 **장치 페이지에서** 장치를 수동으로 등록 했습니다. 그런 다음 장치는 유효한 자격 증명으로 연결 됩니다.  

    운영자는 **마이그레이션** 단추를 사용 하 **여 장치를 장치 페이지에서** 장치 템플릿에 연결할 수 있습니다.

## <a name="sdk-support"></a>SDK 지원

Azure 장치 Sdk는 장치 코드를 구현 하는 가장 쉬운 방법을 제공 합니다. 현재 다음과 같은 디바이스 SDK를 사용할 수 있습니다.

- [C용 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-c)
- [Python용 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-python)
- [Node.js용 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-node)
- [Java용 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-java)
- [.NET용 Azure IoT SDK](https://github.com/azure/azure-iot-sdk-csharp)

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK 기능 및 IoT Hub 연결

IoT Hub와의 모든 디바이스 통신에 다음 IoT Hub 연결 옵션이 사용됩니다.

- [디바이스-클라우드 메시징](../../iot-hub/iot-hub-devguide-messages-d2c.md)
- [클라우드-디바이스 메시징](../../iot-hub/iot-hub-devguide-messages-c2d.md)
- [디바이스 쌍](../../iot-hub/iot-hub-devguide-device-twins.md)

다음 표에는 Azure IoT Central 디바이스 기능이 IoT Hub 기능에 매핑되는 방식이 요약되어 있습니다.

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| 원격 분석 | 디바이스-클라우드 메시징 |
| 오프 라인 명령 | 클라우드-디바이스 메시징 |
| 속성 | 디바이스 쌍 reported 속성 |
| 속성(쓰기 가능) | 디바이스 쌍 desired 및 reported 속성 |
| 명령 | 직접 메서드 |

### <a name="protocols"></a>프로토콜

디바이스 SDK는 IoT Hub에 연결하기 위한 다음과 같은 네트워크 프로토콜을 지원합니다.

- MQTT
- AMQP
- HTTPS

프로토콜 간 차이점 및 프로토콜 선택 방법에 대한 지침은 [통신 프로토콜 선택](../../iot-hub/iot-hub-devguide-protocols.md)을 참조하세요.

장치에서 지원 되는 프로토콜을 사용할 수 없는 경우 Azure IoT Edge를 사용 하 여 프로토콜 변환을 수행 합니다. IoT Edge는 Azure IoT Central 응용 프로그램에서 처리를 오프 로드 하는 다른 최첨단 시나리오를 지원 합니다.

## <a name="security"></a>보안

디바이스와 Azure IoT Central 간에 교환되는 모든 데이터는 암호화됩니다. IoT Hub는 디바이스 연결 IoT Hub 엔드포인트 중 하나에 연결하는 디바이스의 모든 요청을 인증합니다. 유선으로 자격 증명을 교환하지 못하도록 방지하기 위해 디바이스에서는 서명된 토큰을 사용하여 인증합니다. 자세한 내용은 IoT Hub에 대 한 [액세스 제어](../../iot-hub/iot-hub-devguide-security.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

디바이스 개발자라면 다음과 같은 몇 가지 단계를 살펴보세요.

- 장치 개발을 위한 [모범 사례](concepts-best-practices.md) 를 검토 합니다.
- [자습서: 클라이언트 응용 프로그램을 만들어 Azure IoT Central 응용 프로그램에 연결](tutorial-connect-device.md) 에서 SAS 토큰을 사용 하는 방법을 보여 주는 샘플 코드를 검토 합니다.
- [IoT Central 응용 프로그램용 Node.js 장치 SDK를 사용 하 여 x.509 인증서](how-to-connect-devices-x509.md) 를 사용 하 여 장치를 연결 하는 방법에 대해 알아봅니다.
- [Azure CLI를 사용하여 디바이스 연결을 모니터링](./howto-monitor-devices-azure-cli.md)하는 방법을 알아봅니다.
- [Azure IoT Edge 장치 및 Azure IoT Central](./concepts-iot-edge.md) 에 대해 읽기
