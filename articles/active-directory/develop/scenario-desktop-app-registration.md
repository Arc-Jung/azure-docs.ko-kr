---
title: 웹 Api를 호출 하는 데스크톱 앱 등록-Microsoft identity platform | Microsoft
description: 웹 Api (앱 등록)를 호출 하는 데스크톱 앱을 빌드하는 방법 알아보기
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 08e07ac3a8079d725611f9b072e8d21dabb32867
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98011563"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>웹 Api를 호출 하는 데스크톱 앱: 앱 등록

이 문서에서는 데스크톱 응용 프로그램에 대 한 앱 등록 세부 사항을 설명 합니다.

## <a name="supported-account-types"></a>지원되는 계정 유형

데스크톱 응용 프로그램에서 지원 되는 계정 유형은 강화 하려는 환경에 따라 달라 집니다. 이 관계로 인해 지원 되는 계정 유형은 사용 하려는 흐름에 따라 달라 집니다.

### <a name="audience-for-interactive-token-acquisition"></a>대화형 토큰 획득을 위한 대상 그룹

데스크톱 응용 프로그램에서 대화형 인증을 사용 하는 경우 모든 [계정 유형에](quickstart-register-app.md)서 사용자에 게 로그인 할 수 있습니다.

### <a name="audience-for-desktop-app-silent-flows"></a>데스크톱 앱 자동 흐름의 대상

- Windows 통합 인증 또는 사용자 이름 및 암호를 사용 하려면 응용 프로그램이 사용자의 테 넌 트에 로그인 해야 합니다 (예: LOB (기간 업무) 개발자 인 경우). 또는 Azure Active Directory 조직에서는 ISV 시나리오용 경우 응용 프로그램에서 사용자 고유의 테 넌 트에서 사용자를 로그인 해야 합니다. 이러한 인증 흐름은 Microsoft 개인 계정에 대해 지원 되지 않습니다.
- B2C (비즈니스 간) 기관 및 정책을 전달 하는 소셜 id를 사용 하 여 사용자를 로그인 하는 경우에는 대화형 및 사용자 이름-암호 인증만 사용할 수 있습니다.

## <a name="redirect-uris"></a>리디렉션 URI

데스크톱 응용 프로그램에서 사용 하는 리디렉션 Uri는 사용 하려는 흐름에 따라 다릅니다.

- 대화형 인증 또는 장치 코드 흐름을 사용 하는 경우를 사용 `https://login.microsoftonline.com/common/oauth2/nativeclient` 합니다. 이 구성을 얻으려면 응용 프로그램에 대 한 **인증** 섹션에서 해당 URL을 선택 합니다.

  > [!IMPORTANT]
  > 현재 MSAL.NET는 Windows ()에서 실행 되는 데스크톱 응용 프로그램에서 기본적으로 다른 리디렉션 URI를 사용 `urn:ietf:wg:oauth:2.0:oob` 합니다. 나중에이 기본값을 변경 하려고 하므로를 사용 하는 것이 좋습니다 `https://login.microsoftonline.com/common/oauth2/nativeclient` .

- MacOS에 대 한 기본 목표-C 또는 Swift 앱을 빌드하는 경우 응용 프로그램의 번들 식별자를 기반으로 하는 리디렉션 URI를 형식으로 등록 `msauth.<your.app.bundle.id>://auth` 합니다. `<your.app.bundle.id>`을 응용 프로그램의 번들 식별자로 바꿉니다.
- 응용 프로그램에서 Windows 통합 인증 또는 사용자 이름 및 암호만 사용 하는 경우 응용 프로그램에 대 한 리디렉션 URI를 등록할 필요가 없습니다. 이러한 흐름은 Microsoft identity platform v2.0 끝점으로의 왕복을 수행 합니다. 응용 프로그램은 특정 URI에서 다시 호출 되지 않습니다.
- [디먼 응용](scenario-daemon-overview.md)프로그램에서 사용 되는 클라이언트 자격 증명 흐름을 사용 하 여 비밀 클라이언트 응용 프로그램에서 [장치 코드 흐름](scenario-desktop-acquire-token.md#device-code-flow), [Windows 통합 인증](scenario-desktop-acquire-token.md#integrated-windows-authentication)및 [사용자 이름 및 암호](scenario-desktop-acquire-token.md#username-and-password) 를 구분 하려면, 리디렉션 URI가 필요 하지 않은 경우 공용 클라이언트 응용 프로그램으로 구성 해야 합니다. 이 구성을 설정하려면

    1. <a href="https://portal.azure.com/" target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> Azure Portal</a>에서 **앱 등록** 의 앱을 선택한 다음 **인증** 을 선택 합니다.
    1. **고급 설정** 에서  >  **공용 클라이언트 흐름 허용** 에서  >  **다음과 같은 모바일 및 데스크톱 흐름을 사용 하도록 설정 합니다.** 에서 **예** 를 선택 합니다.

        :::image type="content" source="media/scenarios/default-client-type.png" alt-text="Azure Portal의 인증 창에서 공용 클라이언트 설정 사용":::

## <a name="api-permissions"></a>API 사용 권한

데스크톱 응용 프로그램은 로그인 한 사용자에 대 한 Api를 호출 합니다. 위임 된 권한을 요청 해야 합니다. [디먼 응용 프로그램](scenario-daemon-overview.md)에서만 처리 되는 응용 프로그램 사용 권한을 요청할 수 없습니다.

## <a name="next-steps"></a>다음 단계

이 시나리오에서 [앱 코드 구성](scenario-desktop-app-configuration.md)의 다음 문서로 이동 합니다.
