---
title: Azure IoT Central 애플리케이션 만들기 | Microsoft Docs
description: 새로운 Azure IoT Central 애플리케이션을 만듭니다. 애플리케이션 템플릿을 사용하여 평가판 또는 종량제 애플리케이션을 만듭니다.
author: viv-liu
ms.author: viviali
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 23ef0afbf3a3fd3e0d0db6e3b4130b45530916be
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001255"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central 애플리케이션 만들기

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

_작성기_로 Azure IoT Central UI를 사용하여 Microsoft Azure IoT Central 애플리케이션을 정의합니다. 이 빠른 시작에서는 샘플 _디바이스 템플릿_을 포함하는 Azure IoT Central 애플리케이션을 만드는 방법을 보여줍니다.

## <a name="create-an-application"></a>애플리케이션 만들기

[Azure IoT Central 애플리케이션 관리자](https://aka.ms/iotcentral) 웹 사이트로 이동합니다. 그런 다음, Microsoft 개인, 회사 또는 학교 계정으로 로그인합니다.

새로운 Azure IoT Central 애플리케이션 만들기를 시작하려면 **새 애플리케이션**을 선택합니다. 그러면 **애플리케이션 만들기** 페이지로 이동합니다.

![Azure IoT Central 애플리케이션 만들기 페이지](media/quick-deploy-iot-central/iotcentralcreate.png)

새로운 Azure IoT Central 애플리케이션을 만들려면:

1. 결제 계획을 선택합니다.
   - **평가판** 애플리케이션은 7일 동안 무료이며 이후에는 만료됩니다. 만료되기 전에 언제든지 **종량제**로 변환할 수 있습니다. **평가판** 애플리케이션을 만드는 경우 연락처 정보를 입력하고 Microsoft에서 정보 및 팁을 받을 것인지 여부를 선택합니다.
   - **종량제** 애플리케이션은 처음 5개의 디바이스는 무료로 사용하며, 디바이스별로 요금이 청구됩니다. **종량제** 애플리케이션을 만드는 경우 *디렉터리*, *Azure 구독* 및 *지역*을 선택해야 합니다.
        - *디렉터리*는 애플리케이션을 만드는 Azure AD(Active Directory)입니다. 사용자 ID, 자격 증명 및 기타 조직 정보가 포함됩니다. Azure AD가 없는 경우 Azure 구독을 만들면 자동으로 하나가 생성됩니다.
        - *Azure 구독*을 사용하여 Azure 서비스 인스턴스를 만들 수 있습니다. IoT Central은 구독에서 리소스를 프로비저닝합니다. Azure 구독이 아직 없는 경우 [Azure 등록 페이지](https://aka.ms/createazuresubscription)에서 만들 수 있습니다. Azure 구독을 만든 후 다시 **애플리케이션 만들기** 페이지로 돌아갑니다. **Azure 구독** 드롭다운에 새 구독이 표시됩니다.
        - ‘지역’은 애플리케이션을 만들려는 실제 위치 또는 [지리](https://azure.microsoft.com/global-infrastructure/geographies/)입니다.  일반적으로 최적의 성능을 얻으려면 디바이스와 물리적으로 가장 가까운 지역을 선택해야 합니다. Azure IoT Central을 사용할 수 있는 지역은 [지역별 사용 가능한 제품](https://azure.microsoft.com/global-infrastructure/services/?products=iot-central) 페이지에서 확인할 수 있습니다. 지역을 선택하면 나중에 다른 지역으로 애플리케이션을 이동할 수 없습니다.

        [Azure IoT Central 가격 책정 페이지](https://azure.microsoft.com/pricing/details/iot-central/)에서 가격 책정에 대해 자세히 알아보세요.

1. 애플리케이션 템플릿을 선택합니다. 애플리케이션 템플릿에는 시작하는 데 도움이 되는 디바이스 템플릿이나 대시보드 같은 미리 정의된 항목이 포함될 수 있습니다.

    | 애플리케이션 템플릿 | 설명 |
    | -------------------- | ----------- |
    | 샘플 Contoso       | Refrigerated Vending Machine에 대해 이미 만든 디바이스 템플릿을 포함하는 애플리케이션을 만듭니다. 이 템플릿을 사용하여 Azure IoT Central 탐색을 시작하세요. |
    | 샘플 Devkits       | MXChip 또는 Raspberry Pi 디바이스를 연결할 수 있는 디바이스 템플릿을 사용하여 애플리케이션을 만듭니다. 다음 디바이스 중 하나를 실험하는 디바이스 개발자인 경우 이 템플릿을 사용합니다. |
    | 사용자 지정 애플리케이션   | 사용자 고유의 디바이스 템플릿 및 디바이스로 채울 빈 애플리케이션을 만듭니다. |

1. Azure IoT Central은 선택한 애플리케이션 템플릿을 기반으로 애플리케이션 이름을 자동으로 제안합니다. 이 이름을 그대로 적용하거나 **Contoso IoT**와 같은 친숙한 애플리케이션 이름을 입력할 수 있습니다. 또한 Azure IoT Central은 애플리케이션 이름에 따라 고유한 URL 접두사를 생성합니다. 원하는 경우 이 URL 접두사를 더욱 기억하기 쉬운 것으로 자유롭게 변경할 수 있습니다.

1. 이전에 선택한 결제 계획에 필요한 추가 정보를 1단계에서 입력합니다.

1. 페이지의 맨 아래에서 **만들기**를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 IoT Central 애플리케이션을 만들었습니다. 권장되는 단계는 다음과 같습니다.

> [!div class="nextstepaction"]
> [Azure IoT Central 애플리케이션에서 새 디바이스 유형 정의](./tutorial-define-device-type.md)
