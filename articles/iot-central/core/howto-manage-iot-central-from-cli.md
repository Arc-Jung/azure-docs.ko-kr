---
title: Azure CLI에서 IoT Central 관리 | Microsoft Docs
description: 이 문서에서는 CLI를 사용 하 여 IoT Central 응용 프로그램을 만들고 관리 하는 방법을 설명 합니다. CLI를 사용 하 여 응용 프로그램을 보고, 수정 하 고, 제거할 수 있습니다.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
ms.custom: devx-track-azurecli
manager: philmea
ms.openlocfilehash: d414b86ff81a33f9e818a0a28031e73d88cabec2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102202266"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLI에서 IoT Central 관리

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central application manager](https://aka.ms/iotcentral) 웹 사이트에서 IoT Central 응용 프로그램을 만들고 관리 하는 대신 [Azure CLI](/cli/azure/) 를 사용 하 여 응용 프로그램을 관리할 수 있습니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - 다른 Azure 구독에서 CLI 명령을 실행 해야 하는 경우 [활성 구독 변경](/cli/azure/manage-azure-subscriptions-azure-cli#change-the-active-subscription)을 참조 하세요.

## <a name="create-an-application"></a>애플리케이션 만들기

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

[Az iot central app create](/cli/azure/iot/central/app#az-iot-central-app-create) 명령을 사용 하 여 Azure 구독에 IoT Central 응용 프로그램을 만듭니다. 예를 들면 다음과 같습니다.

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iot central app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku ST1 --template "iotc-pnp-preview" \
  --display-name "My Custom Display Name"
```

이러한 명령은 먼저 응용 프로그램에 대 한 미국 동부 지역에 리소스 그룹을 만듭니다. 다음 표에서는 **az iot central app create** 명령에 사용 되는 매개 변수에 대해 설명 합니다.

| 매개 변수         | Description |
| ----------------- | ----------- |
| resource-group    | 애플리케이션을 포함하는 리소스 그룹입니다. 리소스 그룹이 구독에 이미 있어야 합니다. |
| 위치          | 기본적으로이 명령은 리소스 그룹의 위치를 사용 합니다. 현재 **오스트레일리아**, **아시아 태평양**, **유럽** **, 미국, 영국** 및 **일본** 지역에서 IoT Central 응용 **프로그램을 만들** 수 있습니다. |
| name              | Azure Portal의 애플리케이션 이름입니다. |
| 도메인이         | 애플리케이션 URL의 하위 도메인입니다. 예제에서 애플리케이션 URL은 `https://mysubdomain.azureiotcentral.com`입니다. |
| sku               | 현재 **ST1** 또는 **ST2** 중 하나를 사용할 수 있습니다. [Azure IoT Central 가격 책정](https://azure.microsoft.com/pricing/details/iot-central/)을 참조하세요. |
| template          | 사용할 애플리케이션 템플릿입니다. 자세한 내용은 다음 표를 참조하세요. |
| display-name      | UI에 표시되는 애플리케이션 이름입니다. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>애플리케이션 보기

[Az iot central app list](/cli/azure/iot/central/app#az-iot-central-app-list) 명령을 사용 하 여 IoT Central 응용 프로그램을 나열 하 고 메타 데이터를 볼 수 있습니다.

## <a name="modify-an-application"></a>애플리케이션 수정

[Az iot central app update](/cli/azure/iot/central/app#az-iot-central-app-update) 명령을 사용 하 여 IoT Central 응용 프로그램의 메타 데이터를 업데이트 합니다. 예를 들어 애플리케이션의 표시 이름을 변경합니다.

```azurecli-interactive
az iot central app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>애플리케이션 제거

[Az iot central app delete](/cli/azure/iot/central/app#az-iot-central-app-delete) 명령을 사용 하 여 IoT Central 응용 프로그램을 삭제 합니다. 예를 들면 다음과 같습니다.

```azurecli-interactive
az iot central app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>다음 단계

이제 Azure CLI에서 Azure IoT Central 응용 프로그램을 관리 하는 방법을 배웠으므로 제안 된 다음 단계는 다음과 같습니다.

> [!div class="nextstepaction"]
> [애플리케이션 관리](howto-administer.md)
