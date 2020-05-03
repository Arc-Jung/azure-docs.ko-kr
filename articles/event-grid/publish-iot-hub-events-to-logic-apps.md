---
title: 자습서 - IoT Hub 이벤트를 사용하여 Azure Logic Apps 트리거
description: 이 자습서에서는 Azure Event Grid의 이벤트 라우팅 서비스를 사용하여 IoT Hub 이벤트에 따라 Azure Logic Apps 작업을 수행하는 자동화된 프로세스를 만드는 방법을 보여줍니다.
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: tutorial
ms.date: 11/21/2019
ms.author: robinsh
ms.openlocfilehash: 889c5e68759a94682150ac88970b7123ad0fc412
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82201740"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-event-grid-and-logic-apps"></a>자습서: Event Grid 및 Logic Apps를 사용하여 Azure IoT Hub 이벤트에 대한 이메일 알림 보내기

Azure Event Grid를 사용하면 다운스트림 비즈니스 애플리케이션에서 작업을 트리거하여 IoT Hub에서 이벤트에 대응할 수 있습니다.

이 문서에서는 IoT Hub 및 Event Grid를 사용하는 샘플 구성을 안내합니다. 마지막으로 디바이스가 IoT 허브에 추가될 때마다 Azure 논리 앱에서 알림 이메일을 보내도록 설정합니다. 

## <a name="prerequisites"></a>사전 요구 사항

* 활성화된 Azure 구독. 구독이 없는 경우 [체험 Azure 계정을 만들](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

* Office 365 Outlook, Outlook.com 또는 Gmail과 같은 Azure Logic Apps에서 지원하는 이메일 공급자의 이메일 계정입니다. 이 이메일 계정은 이벤트 알림을 보내는 데 사용됩니다. 지원되는 Logic App 커넥터의 전체 목록은 [커넥터 개요](https://docs.microsoft.com/connectors/)를 참조하세요.

  > [!IMPORTANT]
  > Gmail을 사용하기 전에 G Suite 비즈니스 계정(사용자 지정 도메인이 있는 이메일 주소) 또는 Gmail 소비자 계정(@gmail.com 또는 @googlemail.com이 있는 이메일 주소)이 있는지 확인합니다. G Suite 비즈니스 계정만 논리 앱에서 제한 없이 다른 커넥터와 함께 Gmail 커넥터를 사용할 수 있습니다. Gmail 소비자 계정이 있는 경우 특정 Google 승인 서비스에서만 Gmail 커넥터를 사용하거나 [인증에 사용할 Google 클라이언트 앱을 만들](https://docs.microsoft.com/connectors/gmail/#authentication-and-bring-your-own-application) 수 있습니다. 자세한 내용은 [Azure Logic Apps의 Google 커넥터에 대한 데이터 보안 및 개인정보처리방침](../connectors/connectors-google-data-security-privacy-policy.md)을 참조하세요.

* Azure에 있는 IoT Hub입니다. 아직 만들지 않았다면, 연습으로 [IoT Hub 시작](../iot-hub/iot-hub-csharp-csharp-getstarted.md)을 참조하세요.

## <a name="create-a-logic-app"></a>논리 앱 만들기

먼저 논리 앱을 만들고, 가상 머신에 대한 리소스 그룹을 모니터링하는 Event Grid 트리거를 추가합니다. 

### <a name="create-a-logic-app-resource"></a>논리 앱 리소스 만들기

1. [Azure Portal](https://portal.azure.com)에서 **리소스 만들기**를 선택한 다음, 검색 상자에서 "논리 앱"을 입력하고, Enter 키를 선택합니다. 결과에서 **논리 앱**을 선택합니다.

   ![논리 앱 만들기](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

1. 다음 화면에서 **만들기**를 선택합니다. 

1. 구독에서 논리 앱에 고유한 이름을 제공하고 동일한 구독과 리소스 그룹 및 위치를 IoT 허브로 선택합니다. 

   ![논리 앱을 만들기 위한 필드](./media/publish-iot-hub-events-to-logic-apps/create-logic-app-fields.png)

1. **만들기**를 선택합니다.

1. 리소스가 만들어지면 논리 앱으로 이동합니다. 이렇게 하려면 **리소스 그룹**을 선택한 다음, 이 자습서에서 만든 리소스 그룹을 선택합니다. 그런 다음, 리소스 목록에서 논리 앱을 찾아서 선택합니다. 

1. Logic Apps 디자이너에서 페이지 아래쪽으로 이동하여 **템플릿**을 확인합니다. 논리 앱을 처음부터 빌드할 수 있도록 **비어 있는 논리 앱**을 선택합니다.

### <a name="select-a-trigger"></a>트리거 선택

트리거는 논리 앱을 시작하는 특정 이벤트입니다. 이 자습서에서는 워크플로를 시작하는 트리거를 HTTP를 통해 요청을 받습니다.  

1. 커넥터 및 트리거 검색 표시줄에서 **HTTP**를 입력합니다.

1. 트리거로 **요청 - HTTP 요청을 수신한 경우**를 선택합니다. 

   ![HTTP 요청 트리거 선택](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

1. **샘플 페이로드를 사용하여 스키마 생성**을 선택합니다. 

   ![HTTP 요청 트리거 선택](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

1. 다음 샘플 JSON 코드를 텍스트 상자에 붙여넣기한 다음 **완료**를 선택합니다.

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "deviceEtag": "null",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice"
     },
     "dataVersion": "1",
     "metadataVersion": "1"
   }]
   ```

1. **요청할 때 Content-Type 헤더 집합을 애플리케이션/json에 포함해야 합니다**라고 표시되는 팝업 알림을 받을 수도 있습니다. 이 제안을 안전하게 무시하고 다음 섹션으로 이동할 수 있습니다. 

### <a name="create-an-action"></a>작업 만들기

트리거가 논리 앱 워크플로를 시작한 후에 발생하는 모든 단계가 작업할 대상입니다. 이 자습서의 경우 작업은 메일 공급자의 이메일 알림을 보내는 것입니다. 

1. **새 단계**를 선택합니다. 그러면 **작업 선택** 창이 열립니다.

1. **이메일** 검색.

1. 전자 메일 공급자에 따라 일치하는 커넥터를 찾아 선택합니다. 이 자습서에서는 **Office 365 Outlook**이 사용됩니다. 다른 이메일 공급자를 위한 단계들도 비슷합니다. 

   ![이메일 공급자 커넥터 선택](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

1. **이메일 보내기** 작업을 선택합니다. 

1. 메시지가 표시되면 이메일 계정에 로그인합니다. 

1. 이메일 템플릿을 작성합니다. 

   * **받는 사람**: 알림 이메일을 받을 이메일 주소를 입력합니다. 이 자습서의 경우 테스트를 위해 액세스할 수 있는 이메일 계정을 사용합니다. 

   * **주체**: 제목의 텍스트를 입력합니다. [제목] 텍스트 상자를 클릭하면 포함할 동적 콘텐츠를 선택할 수 있습니다. 예를 들어 이 자습서에서는 `IoT Hub alert: {event Type}`을 사용합니다. 동적 콘텐츠가 표시되지 않으면 **동적 콘텐츠 추가** 하이퍼링크를 선택합니다. 그러면 이 링크가 설정/해제됩니다.

   * **본문**: 이메일의 텍스트를 작성합니다. 이벤트 데이터를 기반으로 하는 동적 콘텐츠를 포함하도록 선택기 도구에서 JSON 속성을 선택합니다. 동적 콘텐츠가 표시되지 않으면 **본문** 텍스트 상자에서 **동적 콘텐츠 추가** 하이퍼링크를 선택합니다. 원하는 필드가 표시되지 않으면 동적 콘텐츠 화면에서 *자세히*를 클릭하여 이전 작업의 필드를 포함시킵니다.

   이메일 템플릿은 다음 예와 같습니다.

   ![이메일 정보 입력](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

1. 논리 앱을 저장합니다. 

### <a name="copy-the-http-url"></a>HTTP URL 복사

Logic Apps Designer를 나가기 전에 논리 앱이 트리거에 대해 수신 대기하는 URL을 복사합니다. 이 URL을 사용하여 Event Grid를 구성합니다. 

1. **HTTP 요청을 받을 경우** 트리거 구성 상자를 클릭하여 확장합니다. 

1. **HTTP POST URL**의 값을 그 옆에 있는 복사 단추를 선택하여 복사합니다. 

   ![HTTP POST URL 복사](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

1. 다음 섹션에서 참조할 수 있도록 이 URL을 저장합니다. 

## <a name="configure-subscription-for-iot-hub-events"></a>IoT Hub 이벤트에 대한 구독 구성

이 섹션에서는 일어나는 순서대로 이벤트를 게시하도록 Azure IoT Hub 를 구성합니다. 

1. Azure Portal에서 IoT Hub로 이동합니다. 이 작업은 **리소스 그룹**을 선택하여 수행할 수 있습니다. 그러면 이 자습서의 리소스 그룹을 선택한 다음, 리소스 목록에서 IoT 허브를 선택합니다.

2. **이벤트**를 선택합니다.

   ![Event Grid 세부 정보 열기](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. **이벤트 구독**을 선택합니다. 

   ![새 이벤트 구독 만들기](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. 다음 값을 가진 이벤트 구독을 만듭니다. 

   * **이벤트 구독 정보**: 설명이 포함된 이름을 제공하고, **Event Grid 스키마**를 선택합니다.

   * **이벤트 유형**: **이벤트 유형으로 필터링**에서 **디바이스를 만들었습니다**를 제외한 모든 선택을 취소합니다.

       ![구독 이벤트 유형](./media/publish-iot-hub-events-to-logic-apps/subscription-event-types.png)

   * **엔드포인트 정보**: 엔드포인트 유형으로 **웹후크**를 선택하고, *엔드포인트 선택*을 선택하고,논리 앱에서 복사한 URL을 붙여넣고, 선택 사항을 확인합니다.

     ![엔드포인트 URL 선택](./media/publish-iot-hub-events-to-logic-apps/endpoint-webhook.png)

   완료되면 창이 다음 예와 같이 표시됩니다. 

    ![샘플 이벤트 구독 양식](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. 여기에 이벤트 구독을 저장하고 IoT 허브에서 생성된 모든 디바이스에 대한 알림을 받을 수 있습니다. 이 자습서에서는 선택적 필드를 사용하여 특정 디바이스를 필터링해 보겠습니다. 창의 위쪽에서 **필터**를 선택합니다.

6. **새 필터 추가**를 선택합니다. 필드에서 다음 값을 입력합니다.

   * **키**: `Subject`를 선택합니다.

   * **운영자**: `String begins with`를 선택합니다.

   * **값**:  건물 1에서 디바이스 이벤트를 필터링하려면 `devices/Building1_`을 입력합니다.
  
   다음 값을 사용하여 다른 필터를 추가합니다.

   * **키**: `Subject`를 선택합니다.

   * **운영자**: `String ends with`를 선택합니다.

   * **값**: 온도와 관련된 디바이스 이벤트를 필터링하려면 `_Temperature`를 입력합니다.

   이벤트 구독의 **필터** 탭은 이제 다음 이미지와 같이 표시됩니다.

   ![이벤트 구독에 필터 추가](./media/publish-iot-hub-events-to-logic-apps/event-subscription-filters.png)

7. **만들기**를 선택하여 이벤트 구독을 저장합니다.

## <a name="create-a-new-device"></a>새 디바이스 만들기

이벤트 알림 이메일을 트리거하는 새 디바이스를 만들어 논리 앱을 테스트합니다. 

1. IoT 허브에서 **IoT 디바이스**를 선택합니다. 

2. **새로 만들기**를 선택합니다.

3. **디바이스 ID**로 `Building1_Floor1_Room1_Light`를 입력합니다.

4. **저장**을 선택합니다. 

5. 이벤트 구독 필터를 테스트하기 위해 다른 디바이스 ID를 사용하여 여러 디바이스를 추가할 수 있습니다. 이러한 예제를 시도해 보십시오. 

   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

   네 가지 예제를 추가한 경우 IoT 디바이스 목록이 다음 이미지와 같이 표시됩니다.

   ![IoT Hub 디바이스 목록](./media/publish-iot-hub-events-to-logic-apps/iot-hub-device-list.png)

6. IoT 허브에 몇 개의 디바이스를 추가한 후 이메일을 확인하여 어떤 디바이스가 논리 앱을 트리거했는지 알아봅니다. 

## <a name="use-the-azure-cli"></a>Azure CLI 사용

Azure Portal을 사용하는 대신 Azure 명령줄 인터페이스를 사용하여 Azure IoT Hub 단계를 수행할 수 있습니다. 자세한 내용은 [이벤트 구독 만들기](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) 및 [IoT 디바이스 만들기](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity)에 대한 Azure 명령줄 인터페이스 페이지를 참조하세요.

## <a name="clean-up-resources"></a>리소스 정리

이 자습서에서는 Azure 구독에 요금을 부과하는 리소스를 사용했습니다. 자습서를 사용해 보고 결과 테스트를 완료한 경우 유지하지 않으려는 리소스를 사용하지 않도록 설정하거나 삭제합니다. 

이 자습서에서 만든 리소스를 모두 삭제하려면 리소스 그룹을 삭제합니다. 

1. **리소스 그룹**을 선택한 다음, 이 자습서에서 만든 리소스 그룹을 선택합니다.

2. [리소스 그룹] 창에서 **리소스 그룹 삭제**를 선택합니다. 리소스 그룹 이름을 입력하라는 메시지가 표시되면 해당 그룹을 삭제할 수 있습니다. 여기에 포함된 리소스도 모두 제거됩니다.

모든 리소스를 제거하지 않으려면 리소스를 하나씩 관리할 수 있습니다. 

논리 앱에 대한 작업을 잃지 않으려면 삭제하는 대신에 비활성화합니다. 

1. 논리 앱으로 이동

2. **개요** 블레이드에서 **삭제** 또는 **사용 안 함**을 선택합니다. 

구독마다 하나의 무료 Azure IoT Hub를 가질 수 있습니다. 이 자습서에 대한 무료 허브를 만든 경우 요금이 부과되는 것을 막기 위해 삭제할 필요가 없습니다.

1. IoT Hub로 이동 

2. **개요** 블레이드에서 **삭제**를 선택합니다. 

IoT 허브를 유지하더라도 만든 이벤트 구독을 삭제하는 것이 좋습니다. 

1. IoT 허브에서 **Event Grid**를 선택합니다.

2. 제거하려는 이벤트 구독을 선택합니다. 

3. **삭제**를 선택합니다. 

## <a name="next-steps"></a>다음 단계

* [작업을 트리거하기 위해 Event Grid를 사용하여 IoT Hub 이벤트에 대응](../iot-hub/iot-hub-event-grid.md)에 대해 자세히 알아봅니다.
* [디바이스 연결 및 연결 해제 이벤트를 정렬하는 방법](../iot-hub/iot-hub-how-to-order-connection-state-events.md)에 대해 알아봅니다.
* [Event Grid](overview.md)를 사용하여 다른 무엇을 할 수 있는지에 대해 알아봅니다.