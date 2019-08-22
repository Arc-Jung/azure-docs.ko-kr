---
title: Azure IoT Central에서 웹후크를 사용하여 Azure Functions 트리거
description: Azure IoT Central에서 규칙이 트리거될 때마다 실행되는 함수 앱을 만듭니다.
author: viv-liu
ms.author: viviali
ms.date: 07/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e7c0f0abdf4a96f4af904f76549bdebd62b803cd
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877329"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Azure IoT Central에서 웹후크를 사용하여 Azure Functions 트리거

*이 항목은 빌더 및 관리자에게 적용됩니다.*

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Azure Functions를 사용하여 서버리스 코드를 IoT Central 규칙의 웹후크 출력에서 실행합니다. VM을 프로 비전 하거나 Azure Functions를 사용 하기 위해 웹 앱을 게시할 필요는 없으며, 대신이 코드를 서버 리스로 실행할 수 있습니다. Azure Functions를 사용하여 SQL Database 또는 Event Grid와 같은 최종 대상에 보내기 전에 웹후크 페이로드를 변환합니다.

## <a name="prerequisites"></a>필수 구성 요소

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="how-to-connect-azure-functions"></a>Azure Functions를 연결하는 방법

1. [Azure Portal에서 새 함수 앱을 만듭니다](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Azure Portal에서 새 함수 앱 만들기](media/howto-trigger-azure-functions/createfunction.png)

2. 함수 앱을 확장 하 고 함수 옆에 있는 **+ 단추** 를 선택 합니다. 이 함수가 함수 앱에서 첫 번째 함수가 면 **포털에서** 개발 환경으로를 선택 하 고 **계속**을 선택 합니다.

    ![함수 앱에서 사용자 지정 함수 선택](media/howto-trigger-azure-functions/customfunction.png)

3. **Webhook + API** 템플릿을 선택 하 고 **만들기**를 선택 합니다. 이 항목에서는 .NET 기반 Azure 함수를 사용 합니다.

    ![제네릭 웹후크 트리거 선택](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. 새 함수에서 **</> 함수 URL 가져오기**를 선택한 다음 값을 복사 하 여 저장 합니다. 이 값을 사용하여 웹후크를 구성합니다.

    ![함수의 URL 가져오기](media/howto-trigger-azure-functions/getfunctionurl.png)

4. IoT Central에서 함수 앱에 연결하려는 규칙으로 이동합니다.

5. 웹후크 동작을 추가합니다. **표시 이름**을 입력하고 이전에 **콜백 URL**로 복사한 함수 URL에 붙여넣습니다.

    ![콜백 URL 필드에 함수 URL을 입력합니다.](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. 규칙을 저장합니다. 이제 규칙이 트리거되면 웹 후크는 함수 앱을 호출 하 여 실행 합니다. 함수 앱에서 **모니터** 를 선택 하 여 함수의 호출 기록을 볼 수 있습니다. App Insights 또는 클래식 보기를 사용하여 기록을 검색할 수 있습니다.

    ![함수의 호출 기록 모니터링](media/howto-trigger-azure-functions/monitorfunction.PNG)

자세한 내용은 [제네릭 웹후크를 통해 트리거되는 함수 만들기](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function)에 관한 Azure Functions 문서를 확인해보세요.

## <a name="next-steps"></a>다음 단계
웹후크를 설정하고 사용하는 방법을 배웠으므로 제안되는 다음 단계는 [Microsoft Flow에서 워크플로 만들기](howto-add-microsoft-flow.md)입니다.
