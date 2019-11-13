---
title: 참조 Swagger 설명서를 사용 하는 방법-Azure Digital Twins | Microsoft Docs
description: Azure Digital Twins Swagger 참조 설명서를 사용하는 방법을 알아봅니다.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: ccea63e8edee739ce6743d7638b4e5300ad07f8f
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74009893"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Azure Digital Twins Swagger 참조 설명서

프로비전된 Azure Digital Twins 인스턴스에는 각각 자동으로 생성된 고유한 Swagger 참조 설명서가 포함됩니다.

[Swagger](https://swagger.io/) 또는 [OpenAPI](https://www.openapis.org/)는 복잡한 API 정보를 대화형 및 언어 중립적 참조 리소스로 통합합니다. Swagger는 API에 대한 작업을 수행하는 데 사용할 JSON 페이로드, HTTP 메서드 및 특정 엔드포인트에 대한 중요 참고 자료를 제공합니다.

## <a name="swagger-summary"></a>Swagger 요약

Swagger는 다음을 비롯한 API의 대화형 요약을 제공합니다.

* API 및 개체 모델 정보.
* 필수 요청 페이로드, 헤더, 매개 변수, 컨텍스트 경로 및 HTTP 메서드를 지정하는 REST API 엔드포인트.
* API 기능 테스트
* HTTP 응답의 유효성을 검사하고 확인하는 예제 응답 정보.
* 오류 코드 정보

Swagger는 Azure Digital Twins 관리 API에 대한 호출 테스트와 개발을 보조하는 편리한 도구입니다.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>참조 자료

자동으로 생성되는 Swagger 참조 자료는 중요한 개념에 대한 간단한 개요, 사용 가능한 관리 API 엔드포인트 및 개발과 테스트를 보조하는 각 개체 모델에 대한 설명을 제공합니다.

간결한 요약에서는 API를 설명합니다.

[![Swagger 상단](media/how-to-use-swagger/swagger-management-top-img.png)](media/how-to-use-swagger/swagger-management-top-img.png#lightbox)

관리 API 개체 모델도 나열됩니다.

[![Swagger 모델](media/how-to-use-swagger/swagger-management-models-img.png)](media/how-to-use-swagger/swagger-management-models-img.png#lightbox)

키 특성의 더 자세한 요약은 각 나열된 개체 모델을 선택하면 됩니다.

[![Swagger 모델](media/how-to-use-swagger/swagger-management-model-img.png)](media/how-to-use-swagger/swagger-management-model-img.png#lightbox)

생성된 Swagger 개체 모델은 사용 가능한 모든 Azure Digital Twins [개체 및 API](./concepts-objectmodel-spatialgraph.md)를 보는 데 편리합니다. 개발자는 Azure Digital Twins에서 솔루션을 빌드할 때 이 리소스를 사용할 수 있습니다.

## <a name="endpoint-summary"></a>엔드포인트 요약

또한 Swagger는 관리 API를 구성하는 모든 엔드포인트에 대한 철저한 개요를 제공합니다.

나열된 각 엔드포인트에는 다음과 같은 필수 요청 정보도 포함됩니다.

* 필수 매개 변수
* 필수 매개 변수 데이터 형식
* 리소스에 액세스하는 HTTP 메서드.

[![Swagger 끝점](media/how-to-use-swagger/swagger-management-endpoints-img.png)](media/how-to-use-swagger/swagger-management-endpoints-img.png#lightbox)

더 자세한 개요를 보려면 각 리소스를 선택합니다.

## <a name="use-swagger-to-test-endpoints"></a>Swagger를 사용하여 엔드포인트 테스트

Swagger의 강력한 기능 중 하나는 문서 UI를 통해 직접 API 엔드포인트를 테스트할 수 있는 기능입니다.

특정 엔드포인트를 선택하면 **사용해보기**가 표시됩니다.

[![Swagger 시도](media/how-to-use-swagger/swagger-management-try-img.png)](media/how-to-use-swagger/swagger-management-try-img.png#lightbox)

해당 섹션을 확장하면 각 필수 및 선택적 매개 변수에 대한 입력 필드가 표시됩니다. 올바른 값을 입력하고 **실행**을 선택합니다.

[![Swagger 시도](media/how-to-use-swagger/swagger-management-tried-img.png)](media/how-to-use-swagger/swagger-management-tried-img.png#lightbox)

테스트를 실행한 후 응답 데이터의 유효성을 검사할 수 있습니다.

## <a name="swagger-response-data"></a>Swagger 응답 데이터

나열된 각 엔드포인트에는 개발 및 테스트의 유효성을 검사하는 응답 본문 데이터도 포함됩니다. 이러한 예제에는 성공적인 HTTP 요청에 대해 원하는 상태 코드 및 JSON이 포함됩니다.

[![Swagger 응답](media/how-to-use-swagger/swagger-management-response-img.png)](media/how-to-use-swagger/swagger-management-response-img.png#lightbox)

예제에는 실패한 테스트를 디버그하거나 개선하는 오류 코드도 포함됩니다.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0 권한 부여

> [!NOTE]
> * Azure Digital Twins 리소스를 만든 사용자 보안 주체는 공간 관리자 역할 할당을 포함 하 고 다른 사용자에 대 한 추가 역할 할당을 만들 수 있습니다. 이러한 사용자 및 해당 역할은 Api를 호출할 수 있는 권한이 부여 될 수 있습니다.

1. [이 빠른](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) 시작의 단계에 따라 Azure AD 응용 프로그램을 만들고 구성 합니다. 또는 기존 앱 등록을 다시 사용할 수 있습니다.

1. 앱 등록에 다음 회신 url을 추가 합니다.

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | 이름  | 다음 항목으로 교체 | 예 |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | 포털에서 찾을 수 있는 관리 REST API 설명서 URL  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

1. Azure AD 앱의 ID를 복사 합니다.

Azure Active Directory 등록을 완료 한 후:

1. Swagger 페이지에서 **권한 부여** 단추를 선택 합니다.

    [![Swagger 권한 부여 단추를 선택 합니다.](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

1. 응용 프로그램 ID를 **client_id** 필드에 붙여넣습니다.

    [![Swagger client_id 필드](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

1. 그러면 다음과 같은 성공 모달로 리디렉션됩니다.

    [![Swagger 리디렉션 모달](media/how-to-use-swagger/swagger-auth-redirect-img.png)](media/how-to-use-swagger/swagger-auth-redirect-img.png#lightbox)

OAuth 2.0으로 보호되는 요청을 대화형으로 테스트하는 방법에 대해 자세히 알아보려면 [공식 설명서](https://swagger.io/docs/specification/authentication/oauth2/)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- Azure Digital Twins 개체 모델 및 공간 인텔리전스 그래프에 대해 자세히 알아보려면 [Azure Digital Twins 개체 모델 이해](./concepts-objectmodel-spatialgraph.md)를 읽어보세요.

- 관리 API를 사용하여 인증하는 방법을 알아보려면 [API를 사용하여 인증](./security-authenticating-apis.md)을 읽어보세요.