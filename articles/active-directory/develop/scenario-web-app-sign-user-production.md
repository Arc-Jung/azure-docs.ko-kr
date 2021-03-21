---
title: 사용자에 게 로그인 하는 웹 앱을 프로덕션으로 이동 | Microsoft
titleSuffix: Microsoft identity platform
description: 사용자를 로그인 하는 웹 앱을 빌드하는 방법 알아보기 (프로덕션으로 이동)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f670af1fca4b4638988e53075f092ca1bbac55b2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104578263"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>사용자가 로그인 하는 웹 앱: 프로덕션으로 이동

이제 웹 Api를 호출 하는 토큰을 가져오는 방법을 배웠으므로 응용 프로그램을 프로덕션 환경으로 이동할 때 고려해 야 할 몇 가지 사항이 있습니다.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="troubleshooting"></a>문제 해결
사용자가 처음으로 웹 응용 프로그램에 로그인 하는 경우 동의 해야 합니다. 그러나 일부 조직에서는 사용자가 다음과 같은 메시지를 볼 수 있습니다. *AppName은 관리자만 부여할 수 있는 조직의 리소스에 액세스 하기 위한 권한이 필요 합니다. 이 앱을 사용 하려면 먼저 관리자에 게이 앱에 대 한 권한을 부여 하도록 요청 하세요.*
이는 테 넌 트 관리자가 사용자에 게 동의할 수 없는 기능을 **사용 하지 않도록 설정** 했기 때문입니다. 이 경우 테 넌 트 관리자에 게 응용 프로그램에서 요구 하는 범위에 대 한 관리자 동의를 수행 하도록 요청 합니다.

## <a name="same-site"></a>같은 사이트

Chrome 브라우저 [에서 SameSite 쿠키 변경을 처리 하는 방법에서](howto-handle-samesite-cookie-changes-chrome-browser.md)새 버전의 chrome 브라우저와 관련 된 문제를 이해 하 고 있어야 합니다.

SameSite NuGet 패키지는 가장 일반적인 문제를 처리 합니다.

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>심층 살펴보기: ASP.NET Core 웹 앱 자습서

이 ASP.NET Core 자습서를 사용 하 여 사용자를 로그인 하는 다른 방법에 대해 알아봅니다. 

[개발자를 위한 Microsoft id 플랫폼을 사용 하 여 웹 앱에서 사용자를 로그인 하 고 Api를 호출할 수 있도록 설정](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial)

이 프로그레시브 자습서에는의 계정을 사용 하 여 로그인을 추가 하는 방법을 비롯 하 여 웹 앱에 대 한 프로덕션 준비 코드가 있습니다.

- 조직
- 여러 조직
- 회사 또는 학교 계정 또는 개인 Microsoft 계정
- [Azure AD B2C](../../active-directory-b2c/overview.md)
- 국가별 클라우드

## <a name="tutorial-nodejs-web-app"></a>자습서: 웹 앱 Node.js

이 자습서의 Node.js 웹에 대해 자세히 알아보세요.

[자습서: Node.js & Express 웹 앱에서 로그인 사용자](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-nodejs-webapp-msal)

## <a name="sample-code-java-web-app"></a>샘플 코드: Java 웹 앱

GitHub에서이 샘플의 Java 웹 앱에 대해 자세히 알아보세요. 

[Microsoft id 플랫폼을 사용 하 여 사용자를 로그인 하 고를 호출 하는 Java 웹 응용 프로그램 Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp)

## <a name="next-steps"></a>다음 단계

웹 앱이 로그인 한 후에는 로그인 한 사용자를 대신 하 여 web Api를 호출할 수 있습니다. 웹 앱에서 web Api를 호출 하는 것은 웹 [api를 호출 하는 웹 앱](scenario-web-app-call-api-overview.md)의 개체입니다.