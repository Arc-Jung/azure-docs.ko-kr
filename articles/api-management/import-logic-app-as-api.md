---
title: Azure Portal에서 논리 앱을 API로 가져오기 | Microsoft Docs
description: 이 자습서에서는 APIM(API Management)을 사용하여논리 앱을 API로 가져오는 방법을 보여 줍니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: apimpm
ms.openlocfilehash: 4077187fe04e3be914a6f7fba84c03df1b79d06a
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74108401"
---
# <a name="import-a-logic-app-as-an-api"></a>논리 앱을 API로 가져오기

이 문서에서는 논리 앱을 API로 가져오고 가져온 API를 테스트하는 방법을 보여줍니다.

이 문서에서는 다음 방법을 설명합니다.

> [!div class="checklist"]
>
> -   논리 앱을 API로 가져오기
> -   Azure Portal에서 API 테스트
> -   개발자 포털에서 API 테스트

## <a name="prerequisites"></a>필수 조건

-   다음 빠른 시작을 완료합니다. [Azure API Management 인스턴스 만들기](get-started-create-service-instance.md)
-   HTTP 엔드포인트를 노출하는 구독에 Logic Apps가 있는지 확인합니다. 자세한 내용은 [HTTP 엔드포인트를 통해 워크플로 트리거](../logic-apps/logic-apps-http-endpoint.md)를 참조하세요.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>백 엔드 API 가져오기 및 게시

1. **API Management**에서 **API**를 선택합니다.
2. **새 API 추가** 목록에서 **논리 앱**을 선택합니다.

    ![논리 앱](./media/import-logic-app-as-api/logic-app-api.png)

3. **찾아보기**를 눌러 구독의 HTTP 트리거가 포함된 Logic Apps 목록을 표시합니다. (HTTP 트리거가 없는 Logic Apps는 목록에 표시되지 않습니다.)
4. 앱을 선택합니다. API Management는 선택한 앱과 연결된 swagger를 찾아서 페치하고 가져옵니다.
5. API URL 접미사를 추가합니다. 접미사는 이 API Management 인스턴스에서 이 특정 API를 식별하는 이름입니다. 접미사는 이 API Management 인스턴스에서 고유해야 합니다.
6. API를 제품에 연결하여 API를 게시합니다. 이 경우 "_Unlimited_" 제품이 사용됩니다. API를 게시하고 개발자가 사용할 수 있게 하려면 제품에 추가합니다. API를 만드는 동안 이 작업을 수행할 수도 있고 나중에 설정할 수도 있습니다.

    제품은 하나 이상의 API와 연결됩니다. 다양한 API를 포함하고 개발자 포털을 통해 개발자에게 제공할 수 있습니다. 개발자는 API에 액세스하려면 먼저 제품을 구독해야 합니다. 구독할 경우 해당 제품의 모든 API에 적절한 구독 키를 받게 됩니다. API Management 인스턴스를 만든 경우 사용자는 이미 관리자이므로 기본적으로 모든 제품을 구독한 상태가 됩니다.

    기본적으로 각 API Management 인스턴스는 두 개의 샘플 제품과 함께 제공됩니다.

    - **Starter**
    - **무제한**

7. **만들기**를 선택합니다.

## <a name="test-the-api-in-the-azure-portal"></a>Azure Portal에서 API 테스트

dAzure Portal에서 직접 작업을 호출할 수 있으며, 이 포털을 사용하면 편리한 방법으로 API의 작업을 보고 테스트할 수 있습니다.

1. 이전 단계에서 만든 API를 선택합니다.
2. **테스트** 탭을 누릅니다.
3. 작업을 선택합니다.

    페이지에 쿼리 매개 변수에 대한 필드와 헤더 필드가 표시됩니다. 헤더 중 하나는 이 API와 연결된 제품의 구독 키에 대한 "Ocp-Apim-Subscription-Key"입니다. API Management 인스턴스를 만든 경우 사용자는 이미 관리자이므로 키가 자동으로 채워집니다.

4. **보내기**를 누릅니다.

    백 엔드는 **200 정상** 및 일부 데이터로 응답합니다.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>모든 논리 앱에는 **manual-invoke** 작업이 있습니다. API를 여러 논리 앱으로 구성하려는 경우 충돌하지 않도록 하기 위해 함수 이름을 변경해야 합니다.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
>
> [게시된 API 변환 및 보호](transform-api.md)
