---
title: Azure IoT Hub 디바이스 스트림을 통해 Node.js에서 디바이스 앱과 통신
description: 이 빠른 시작에서는 디바이스 스트림을 통해 IoT 디바이스와 통신하는 Node.js 서비스 쪽 애플리케이션을 실행합니다.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc, devx-track-js
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: 7401bc0661459e1322c9b2f2226d45794f814732
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91302947"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>빠른 시작: IoT Hub 디바이스 스트림을 통해 Node.js에서 디바이스 애플리케이션과 통신(미리 보기)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

이 빠른 시작에서는 서비스쪽 애플리케이션을 실행하고 디바이스 스트림을 사용하여 디바이스와 서비스 간의 통신을 설정합니다. Azure IoT Hub 디바이스 스트림은 서비스 및 디바이스 애플리케이션이 안전하고 방화벽 친화적인 방식으로 통신할 수 있도록 합니다. 공개 미리 보기 동안 Node.js SDK는 서비스 쪽에서 디바이스 스트림만 지원합니다. 결과적으로 이 빠른 시작에서는 서비스 쪽 애플리케이션을 실행하는 지침만 설명합니다.

## <a name="prerequisites"></a>사전 요구 사항

* [IoT Hub 디바이스 스트림을 통해 C에서 디바이스 앱과 통신](./quickstart-device-streams-echo-c.md) 또는 [IoT Hub 디바이스 스트림을 통해 C# 디바이스 앱과 통신](./quickstart-device-streams-echo-csharp.md) 완료.

* 활성 구독이 있는 Azure 계정. [체험 계정 만들기](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Node.js 10 이상](https://nodejs.org).

* [샘플 Node.js 프로젝트](https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip).

다음 명령을 사용하여 개발 컴퓨터에서 Node.js의 현재 버전을 확인할 수 있습니다.

```cmd/sh
node --version
```

Microsoft Azure IoT Hub는 현재 디바이스 스트림을 [미리 보기 기능](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)으로 지원합니다.

> [!IMPORTANT]
> 디바이스 스트림의 미리 보기는 현재 다음 지역에서 만든 IoT Hub에 대해서만 지원됩니다.
>
> * 미국 중부
> * 미국 중부 EUAP
> * 북유럽
> * 동남아시아

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="add-azure-iot-extension"></a>Azure IoT 확장 추가

다음 명령을 실행하여 Cloud Shell 인스턴스에 Azure CLI용 Microsoft Azure IoT 확장을 추가합니다. IoT 확장은 Azure CLI에 IoT Hub, IoT Edge 및 IoT DPS(Device Provisioning Service) 명령을 추가합니다.

```azurecli-interactive
az extension add --name azure-iot
```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>IoT Hub 만들기

이전 [빠른 시작: 디바이스에서 IoT 허브로 원격 분석 보내기](quickstart-send-telemetry-node.md)를 완료한 경우 이 단계를 건너뛸 수 있습니다.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>디바이스 등록

이전 [빠른 시작: 디바이스에서 IoT 허브로 원격 분석 보내기](quickstart-send-telemetry-node.md)를 완료한 경우 이 단계를 건너뛸 수 있습니다.

연결을 위해 디바이스를 IoT Hub에 등록해야 합니다. 이 빠른 시작에서는 Azure Cloud Shell을 사용하여 시뮬레이션된 디바이스를 등록합니다.

1. Azure Cloud Shell에서 다음 명령을 실행하여 디바이스 ID를 만듭니다.

   **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

   **MyDevice**: 등록 중인 디바이스의 이름입니다. 표시된 대로 **MyDevice**를 사용하는 것이 좋습니다. 다른 디바이스 이름을 선택하는 경우 이 문서 전체에서도 해당 이름을 사용해야 하며, 샘플 애플리케이션에서 디바이스 이름을 업데이트한 후 실행해야 합니다.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDevice
    ```

2. 또한 백 엔드 애플리케이션을 IoT 허브에 연결하여 메시지를 검색할 수 있게 하려면 *서비스 연결 문자열*이 필요합니다. 다음 명령은 IoT Hub에 대한 서비스 연결 문자열을 검색합니다.

    **YourIoTHubName**: 이 자리 표시자를 IoT 허브용으로 선택한 이름으로 바꿉니다.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
    ```

    나중에 이 빠른 시작에서 사용할 수 있도록 반환된 서비스 연결 문자열을 적어 두세요. 다음 예제와 유사합니다.

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>디바이스 스트림을 통해 디바이스와 서비스 간의 통신

이 섹션에서는 디바이스 쪽 애플리케이션과 서비스 쪽 애플리케이션을 둘 다 실행하고 서로 통신합니다.

### <a name="run-the-device-side-application"></a>디바이스 쪽 애플리케이션 실행

앞에서 설명한 대로 IoT Hub Node.js SDK는 서비스 쪽에서 디바이스 스트림을 지원합니다. 디바이스 쪽 애플리케이션의 경우 다음 빠른 시작에서 함께 제공되는 디바이스 프로그램 중 하나를 사용합니다.

* [IoT Hub 디바이스 스트림을 통해 C에서 디바이스 앱과 통신](./quickstart-device-streams-echo-c.md)

* [IoT Hub 디바이스 스트림을 통해 C#에서 디바이스 앱과 통신](./quickstart-device-streams-echo-csharp.md)

다음 단계로 진행하기 전에 디바이스 쪽 애플리케이션이 실행 중인지 확인합니다.

### <a name="run-the-service-side-application"></a>서비스 쪽 애플리케이션 실행

이 빠른 시작의 서비스 쪽 Node.js 애플리케이션에는 다음과 같은 기능이 있습니다.

* IoT 디바이스에 디바이스 스트림을 만듭니다.
* 명령줄에서 입력을 읽고 다시 에코하는 디바이스 애플리케이션에 보냅니다.

코드는 디바이스 스트림의 초기화 프로세스와 데이터를 보내고 받는 데 사용하는 방법을 보여줍니다.

디바이스 쪽 애플리케이션이 실행되고 있다고 가정하여 로컬 터미널 창의 아래 단계에 따라 Node.js에서 서비스 쪽 애플리케이션을 실행합니다.

* 환경 변수로 서비스 자격 증명 및 디바이스 ID를 제공합니다.
 
   ```cmd/sh
   # In Linux
   export IOTHUB_CONNECTION_STRING="{ServiceConnectionString}"
   export STREAMING_TARGET_DEVICE="MyDevice"

   # In Windows
   SET IOTHUB_CONNECTION_STRING={ServiceConnectionString}
   SET STREAMING_TARGET_DEVICE=MyDevice
   ```
  
   ServiceConnectionString 자리 표시자를 서비스 연결 문자열과 일치하도록 변경하고 다른 이름을 지정한 경우 디바이스 ID와 일치하도록 **MyDevice**를 변경합니다.

* 압축을 푼 프로젝트 폴더의 `Quickstarts/device-streams-service`로 이동하고 노드를 사용하여 샘플을 실행합니다.

   ```cmd/sh
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
    
   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   node echo.js
   ```

마지막 단계의 끝에서 서비스 쪽 프로그램은 디바이스에 대한 스트림을 시작하고 설정되면 스트림을 통해 문자열 버퍼를 서비스로 보냅니다. 이 샘플에서 서비스 쪽 프로그램은 단순히 터미널에서 `stdin`을 읽고 다시 에코하는 디바이스에 보냅니다. 두 애플리케이션 간의 성공적인 양방향 통신을 보여줍니다.

![서비스 쪽 콘솔 출력](./media/quickstart-device-streams-echo-nodejs/service-console-output.png)

그런 다음, 엔터 키를 다시 눌러 프로그램을 종료할 수 있습니다.

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 IoT 허브를 설정했고, 디바이스를 등록했으며, 디바이스 및 서비스 쪽에서 애플리케이션 간 디바이스 스트림을 설정했고, 이 스트림을 사용하여 애플리케이션 간에 데이터를 주고 받습니다.

디바이스 스트림에 대해 자세히 알아보려면 아래 링크를 사용합니다.

> [!div class="nextstepaction"]
> [디바이스 스트림 개요](./iot-hub-device-streams-overview.md) 