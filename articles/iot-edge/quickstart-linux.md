---
title: '빠른 시작: Linux에서 Azure IoT Edge 디바이스 만들기 | Microsoft Docs'
description: 이 빠른 시작에서는 IoT Edge 디바이스를 만든 다음, Azure Portal에서 원격으로 미리 빌드된 코드를 배포하는 방법을 알아봅니다.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/30/2020
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: a5829057aed913ea824cbd2fd6b52369b5e70d88
ms.sourcegitcommit: a989fb89cc5172ddd825556e45359bac15893ab7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85801848"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-linux-device"></a>빠른 시작: 가상 Linux 디바이스에 첫 번째 IoT Edge 모듈 배포

이 빠른 시작에서는 컨테이너화된 코드를 가상 Linux IoT Edge 디바이스에 배포하여 Azure IoT Edge를 테스트합니다. IoT Edge를 사용하면 디바이스에서 코드를 원격으로 관리하여 더 많은 워크로드를 에지로 전송할 수 있습니다. 이 빠른 시작에서는 IoT Edge 디바이스에 Azure 가상 머신을 사용하는 것이 좋습니다. 이를 통해 IoT Edge 서비스가 설치된 테스트 컴퓨터를 빠르게 만든 다음, 빠른 시작을 마친 후 삭제할 수 있습니다.

이 빠른 시작에서 다음을 수행하는 방법을 알아봅니다.

1. IoT Hub를 만듭니다.
2. IoT Edge 디바이스를 IoT Hub에 등록합니다.
3. 가상 디바이스에 IoT Edge 런타임을 설치하고 시작합니다.
4. 모듈을 IoT Edge 디바이스에 원격으로 배포합니다.

![다이어그램 - 빠른 시작: 디바이스 및 클라우드의 아키텍처](./media/quickstart-linux/install-edge-full.png)

이 빠른 시작에서는 IoT Edge 디바이스로 구성되는 Linux 가상 머신을 만드는 과정을 안내합니다. 그런 다음, Azure Portal에서 디바이스로 모듈을 배포합니다. 이 빠른 시작에 사용되는 모듈은 온도, 습도 및 압력 데이터를 생성하는 시뮬레이션된 센서입니다. 다른 Azure IoT Edge 자습서에서는 비즈니스 인사이트를 위해 시뮬레이션된 데이터를 분석하는 추가 모듈을 배포하는 과정을 설명하므로 여기서 수행하는 작업을 토대로 진행됩니다.

활성 Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free)을 만드세요.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

이 빠른 시작에서는 Azure CLI를 사용하여 많은 단계를 수행하고, Azure IoT에는 추가 기능을 사용할 수 있게 하는 확장이 있습니다.

Azure IoT 확장을 Cloud Shell 인스턴스에 추가합니다.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>필수 구성 요소

클라우드 리소스:

* 이 빠른 시작에서 사용하는 모든 리소스를 관리하는 리소스 그룹입니다. 이 빠른 시작과 다음 자습서에서는 **IoTEdgeResources**라는 예제 리소스 그룹을 사용합니다.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

## <a name="create-an-iot-hub"></a>IoT Hub 만들기

Azure CLI를 사용하여 IoT Hub를 만들어서 빠른 시작을 시작합니다.

![다이어그램 - 클라우드에 IoT 허브 만들기](./media/quickstart-linux/create-iot-hub.png)

이 빠른 시작에는 무료 수준의 IoT Hub가 작동합니다. 이전에 IoT Hub를 사용했고 이미 만든 허브가 있으면 해당 IoT 허브를 사용할 수 있습니다.

다음 코드는 **IoTEdgeResources** 리소스 그룹에서 무료 **F1** 허브를 만듭니다. `{hub_name}`을 IoT 허브의 고유한 이름으로 바꿉니다.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   구독에 이미 한 개의 무료 허브가 있기 때문에 오류가 발생하는 경우 SKU를 **S1**으로 변경합니다. 구독마다 하나의 무료 IoT Hub만 가질 수 있습니다. IoT Hub 이름을 사용할 수 없다는 오류가 발생할 경우 다른 사용자에게 해당 이름의 허브가 이미 있는 것입니다. 새 이름을 사용해 보세요.

## <a name="register-an-iot-edge-device"></a>IoT Edge 디바이스 등록

새로 만든 IoT Hub에 IoT Edge 디바이스를 등록합니다.

![다이어그램 - IoT Hub ID로 디바이스 등록](./media/quickstart-linux/register-device.png)

IoT 허브와 통신할 수 있도록 IoT Edge 디바이스에 대한 디바이스 ID를 만듭니다. 디바이스 ID는 클라우드에 있으며, 사용자는 고유한 디바이스 연결 문자열을 사용하여 물리적 디바이스를 디바이스 ID에 연결합니다.

IoT Edge 디바이스는 일반적인 IoT 디바이스와 다르게 작동하며 다른 방식으로 관리될 수 있으므로, `--edge-enabled` 플래그를 사용하여 이 ID를 IoT Edge 디바이스로 선언합니다.

1. Azure Cloud Shell에서 다음 명령을 입력하여 **myEdgeDevice**라는 디바이스를 허브에 만듭니다.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --edge-enabled --hub-name {hub_name}
   ```

   iothubowner 정책 키에 대한 오류가 표시될 경우 Cloud Shell에서 최신 버전의 azure-iot 확장이 실행 중인지 확인합니다.

2. IoT Hub에서 물리적 디바이스를 해당 ID에 연결하는 디바이스의 연결 문자열을 확인합니다. 연결 문자열에는 IoT 허브 이름, 디바이스 이름 및 둘 간의 연결을 인증하는 공유 키가 포함됩니다. 다음 섹션에서 IoT Edge 디바이스를 설정할 때 이 연결 문자열을 다시 살펴보겠습니다.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

   ![CLI 출력에서 연결 문자열 확인](./media/quickstart/retrieve-connection-string.png)

## <a name="configure-your-iot-edge-device"></a>IoT Edge 디바이스 구성

Azure IoT Edge 런타임이 있는 가상 머신을 만듭니다.

![다이어그램 - 디바이스에서 런타임 시작](./media/quickstart-linux/start-runtime.png)

IoT Edge 런타임은 모든 IoT Edge 디바이스에 배포되며, 세 가지 구성 요소가 있습니다. *IoT Edge 보안 디먼*은 IoT Edge 디바이스가 부팅되고 IoT Edge 에이전트를 시작하여 디바이스를 부트스트랩할 때마다 시작됩니다. *IoT Edge 에이전트*는 IoT Edge 허브를 포함하여 IoT Edge 디바이스에서 모듈을 쉽게 배포하고 모니터링할 수 있습니다. *IoT Edge 허브*는 IoT Edge 디바이스의 모듈 간 통신과 디바이스와 IoT Hub 간의 통신을 관리합니다.

런타임을 구성하는 동안 디바이스 연결 문자열을 입력합니다. Azure CLI에서 검색한 문자열입니다. 이 문자열은 물리적 디바이스를 Azure의 IoT Edge 디바이스 ID에 연결합니다.

### <a name="deploy-the-iot-edge-device"></a>IoT Edge 디바이스 배포

이 섹션에서는 Azure Resource Manager 템플릿을 사용하여 새 가상 머신을 만들고 IoT Edge 런타임을 설치합니다. 사용자 고유의 Linux 디바이스를 대신 사용하려는 경우 [Linux에 Azure IoT Edge 런타임 설치](how-to-install-iot-edge-linux.md)의 설치 단계를 수행한 다음, 이 빠른 시작으로 돌아오면 됩니다.

다음 CLI 명령을 사용하여 미리 빌드된 [iotedge-vm-deploy](https://github.com/Azure/iotedge-vm-deploy) 템플릿을 기반으로 IoT Edge 디바이스를 만듭니다.

* bash 또는 cloud shell 사용자의 경우 다음 명령을 텍스트 편집기에 복사하고, 자리 표시자 텍스트를 해당 정보로 바꾸고, bash 또는 cloud shell 창에 복사합니다.

   ```azurecli-interactive
   az deployment group create \
   --resource-group IoTEdgeResources \
   --template-uri "https://aka.ms/iotedge-vm-deploy" \
   --parameters dnsLabelPrefix='my-edge-vm' \
   --parameters adminUsername='azureUser' \
   --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name <REPLACE_WITH_HUB_NAME> -o tsv) \
   --parameters authenticationType='password' \
   --parameters adminPasswordOrKey="<REPLACE_WITH_PASSWORD>"
   ```

* PowerShell 사용자의 경우 PowerShell 창에 다음 명령을 복사한 다음, 자리 표시자 텍스트를 해당 정보로 바꿉니다.

   ```azurecli
   az deployment group create `
   --resource-group IoTEdgeResources `
   --template-uri "https://aka.ms/iotedge-vm-deploy" `
   --parameters dnsLabelPrefix='my-edge-vm1' `
   --parameters adminUsername='azureUser' `
   --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name <REPLACE_WITH_HUB_NAME> -o tsv) `
   --parameters authenticationType='password' `
   --parameters adminPasswordOrKey="<REPLACE_WITH_PASSWORD>"
   ```

이 템플릿은 다음 매개 변수를 사용합니다.

| 매개 변수 | Description |
| --------- | ----------- |
| **resource-group** | 네트워크를 만들 리소스 그룹입니다. 지금까지 이 문서 전체에서 사용한 기본 **IoTEdgeResources**를 사용하거나, 구독의 기존 리소스 그룹 이름을 입력합니다. |
| **template-uri** | 사용 중인 Resource Manager 템플릿을 가리키는 포인터입니다. |
| **dnsLabelPrefix** | 가상 머신의 호스트 이름을 만드는 데 사용되는 문자열입니다. **my-edge-vm** 예제를 사용하거나 새 문자열을 입력합니다. |
| **adminUsername** | 가상 머신의 관리자 계정 사용자 이름입니다. **azureUser** 예제를 사용하거나 새 사용자 이름을 입력합니다. |
| **deviceConnectionString** | IoT Hub에서 디바이스 ID의 연결 문자열입니다. 가상 머신의 IoT Edge 디바이스의 런타임을 구성하는 데 사용됩니다. 이 매개 변수 내의 CLI 명령은 사용자 대신 연결 문자열을 가져옵니다. 자리 표시자 텍스트를 IoT 허브 이름으로 바꿉니다. |
| **authenticationType** | 관리자 계정의 인증 방법입니다. 이 빠른 시작에서는 **암호** 인증을 사용하지만, 이 매개 변수를 **sshPublicKey**로 설정할 수도 있습니다. |
| **adminPasswordOrKey** | 관리 계정의 암호 또는 SSH 키 값입니다. 자리 표시자 텍스트를 안전한 암호로 바꿉니다. 암호의 길이는 12자 이상이어야 하며 소문자, 대문자, 숫자 및 특수 문자 중 세 가지를 포함해야 합니다. |

배포가 완료되면 가상 머신에 연결하기 위한 SSH 정보를 포함하고 있는 JSON 형식의 출력이 CLI에 표시됩니다. **출력** 섹션의 **공용 SSH** 항목 값을 복사합니다.

   ![출력에서 공용 ssh 값 검색](./media/quickstart-linux/outputs-public-ssh.png)

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge 런타임 상태 보기

이 빠른 시작의 나머지 명령은 IoT Edge 디바이스 자체에서 발생하기 때문에 디바이스에서 발생하는 상황을 볼 수 있습니다. 가상 머신을 사용하는 경우 설정하는 관리 사용자 이름과 배포 명령에서 출력한 DNS 이름을 사용하여 해당 컴퓨터에 지금 연결합니다. Azure Portal의 가상 머신 개요 페이지에서 DNS 이름을 확인할 수도 있습니다. 다음 명령을 사용하여 가상 머신에 연결합니다. `{admin username}` 및 `{DNS name}`를 사용자 고유의 값으로 바꿉니다.

   ```console
   ssh {admin username}@{DNS name}
   ```

가상 머신에 연결되면 IoT Edge 디바이스에 런타임이 성공적으로 설치 및 구성되었는지 확인합니다.

1. IoT Edge 보안 디먼이 시스템 서비스로 실행되고 있는지 확인합니다.

   ```bash
   sudo systemctl status iotedge
   ```

   ![시스템 서비스로 실행되는 IoT Edge 디먼 확인](./media/quickstart-linux/iotedged-running.png)

   >[!TIP]
   >`iotedge` 명령을 실행하려면 상승된 권한이 필요합니다. IoT Edge 런타임을 설치한 후 처음으로 머신에서 로그아웃했다가 다시 로그인하면 권한이 자동으로 업데이트됩니다. 그 전까지는 명령 앞에 `sudo`를 사용합니다.

2. 서비스 문제를 해결해야 할 경우 서비스 로그를 검색합니다.

   ```bash
   journalctl -u iotedge
   ```

3. IoT Edge 디바이스에서 실행되는 모든 모듈을 봅니다. 서비스를 처음 시작했으므로 **edgeAgent** 모듈이 실행되는 것을 확인해야 합니다. edgeAgent 모듈은 기본적으로 실행되며, 디바이스에 배포하는 추가 모듈을 설치하고 시작하는 데 도움이 됩니다.

   ```bash
   sudo iotedge list
   ```

   ![디바이스에서 하나의 모듈 보기](./media/quickstart-linux/iotedge-list-1.png)

IoT Edge 디바이스가 구성되었습니다. 클라우드 배포 모듈을 실행할 준비가 완료된 것입니다.

## <a name="deploy-a-module"></a>모듈 배포

클라우드에서 Azure IoT Edge 디바이스를 관리하여 원격 분석 데이터를 IoT Hub로 보낼 모듈을 배포합니다.

![다이어그램 - 클라우드에서 디바이스로 모듈 배포](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>생성된 데이터 보기

이 빠른 시작에서는 새 IoT Edge 디바이스를 만들고 여기에 IoT Edge 런타임을 설치했습니다. 그런 다음, 디바이스 자체를 변경하지 않고도 디바이스에서 실행할 수 있도록 Azure Portal을 사용하여 IoT Edge 모듈을 배포했습니다.

여기서 푸시한 모듈은 나중에 테스트에 사용할 수 있는 샘플 환경 데이터를 생성합니다. 시뮬레이션된 센서는 머신과 머신 주변의 환경을 모니터링합니다. 예를 들어 이 센서가 서버실, 공장 또는 풍력 터빈에 장착될 수 있습니다. 메시지에는 주변 온도 및 습도, 머신 온도 및 압력, 타임스탬프가 포함됩니다. IoT Edge 자습서는 이 모듈에서 만든 데이터를 분석용 테스트 데이터로 사용합니다.

IoT Edge 디바이스에서 명령 프롬프트를 다시 열거나 Azure CLI에서 SSH 연결을 사용합니다. 클라우드에서 배포된 모듈을 IoT Edge 디바이스에서 실행 중인지 확인합니다.

   ```bash
   sudo iotedge list
   ```

   ![디바이스에서 세 가지 모듈 보기](./media/quickstart-linux/iotedge-list-2.png)

온도 센서 모듈에서 전송되는 메시지를 봅니다.

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >IoT Edge 명령은 모듈 이름을 참조하는 경우 대/소문자를 구분합니다.

   ![모듈의 데이터 보기](./media/quickstart-linux/iotedge-logs.png)

[Visual Studio Code용 Azure IoT Hub 확장](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)을 사용하여 IoT 허브에 메시지가 들어오는 것을 확인할 수도 있습니다.

## <a name="clean-up-resources"></a>리소스 정리

IoT Edge 자습서로 계속 진행하려면 이 빠른 시작에서 등록하고 설정한 디바이스를 사용할 수 있습니다. 그렇지 않으면 요금이 발생하지 않도록 Azure 리소스를 삭제할 수 있습니다.

새 리소스 그룹에서 가상 머신 및 IoT 허브를 만든 경우 해당 그룹 및 모든 관련 리소스를 삭제할 수 있습니다. 리소스 그룹의 콘텐츠를 한 번 더 확인하여 유지할 내용이 없는지 검토합니다. 전체 그룹을 삭제하지는 않으려는 경우 대신, 개별 리소스를 삭제할 수 있습니다.

**IoTEdgeResources** 그룹을 제거합니다.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 새 IoT Edge 디바이스를 만들고 Azure IoT Edge 클라우드 인터페이스를 사용하여 디바이스에 코드를 배포했습니다. 이제 해당 환경에 대한 원시 데이터를 생성하는 테스트 디바이스가 준비되었습니다.

다음 단계는 비즈니스 논리를 실행하는 IoT Edge를 만들기 시작할 수 있도록 로컬 개발 환경을 설정하는 것입니다.

> [!div class="nextstepaction"]
> [Linux 디바이스를 위한 IoT Edge 모듈 개발 시작](tutorial-develop-for-linux.md)
