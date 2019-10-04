---
title: Azure Digital Twins를 사용 하 여 API 인증 이해 | Microsoft Docs
description: Azure Digital Twins를 사용 하 여 Api에 연결 하 고 인증 하는 방법을 알아봅니다.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c75db8d1885c8680dd316952a5f67e11dc26edb1
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949790"
---
# <a name="connect-to-and-authenticate-with-apis"></a>Api에 연결 및 인증

Azure Digital Twins는 Azure AD(Azure Active Directory)를 사용하여 사용자를 인증하고 애플리케이션을 보호합니다. Azure AD는 다양한 최신 아키텍처의 인증을 지원합니다. 모두 업계 표준 프로토콜인 OAuth 2.0 또는 OpenID Connect를 기반으로 합니다. 또한 개발자는 Azure AD를 사용하여 단일 테넌트 및 LOB(기간 업무) 애플리케이션을 빌드할 수 있습니다. Azure AD를 사용하여 다중 테넌트 애플리케이션을 개발할 수도 있습니다.

Azure AD 개요의 경우 단계별 가이드, 개념 및 빠른 시작은 [기본 페이지](https://docs.microsoft.com/azure/active-directory/fundamentals/)를 방문하세요.

> [!TIP]
> [자습서](tutorial-facilities-setup.md) 에 따라 Azure Digital twins 샘플 앱을 설정 하 고 실행 합니다.

애플리케이션 또는 서비스를 Azure AD와 통합하려면 개발자가 먼저 Azure AD에 애플리케이션을 등록해야 합니다. 자세한 지침과 스크린샷은 [이 빠른 시작](../active-directory/develop/quickstart-register-app.md)을 참조하세요.

Azure AD에서 지원되는 [5가지 기본 애플리케이션 시나리오](../active-directory/develop/v2-app-types.md)는 다음과 같습니다.

* SPA (단일 페이지 응용 프로그램): 사용자는 Azure AD로 보호 되는 단일 페이지 응용 프로그램에 로그인 해야 합니다.
* 웹 브라우저-웹 응용 프로그램: 사용자가 Azure AD로 보호 되는 웹 응용 프로그램에 로그인 해야 합니다.
* 웹 API에 대 한 네이티브 응용 프로그램: 휴대폰, 태블릿 또는 PC에서 실행 되는 네이티브 응용 프로그램은 Azure AD로 보호 되는 web API에서 리소스를 가져오기 위해 사용자를 인증 해야 합니다.
* 웹 응용 프로그램-web API: 웹 애플리케이션에서 Azure AD로 보호되는 웹 API로부터 리소스를 가져와야 합니다.
* 웹 API에 대 한 디먼 또는 서버 응용 프로그램: 웹 UI가 없는 데몬 응용 프로그램 또는 서버 응용 프로그램은 Azure AD로 보호 되는 web API에서 리소스를 가져와야 합니다.

> [!IMPORTANT]
> Azure Digital Twins는 다음 인증 라이브러리를 모두 지원 합니다.
> * 최신 [MSAL (Microsoft 인증 라이브러리)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)
> * [ADAL (Azure Active Directory 인증 라이브러리)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>중간 계층 웹 API에서 Digital Twins 호출

개발자가 Digital Twins 솔루션을 설계할 때는 일반적으로 중간 계층 애플리케이션이나 API를 만듭니다. 그런 다음, 이 앱 또는 API가 Digital Twins API 다운스트림을 호출합니다. 이 표준 웹 솔루션 아키텍처를 지원하려면 사용자는 먼저 다음을 수행해야 합니다.

1. 중간 계층 애플리케이션에서 인증 받기

1. 인증 중에 OAuth 2.0 On-Behalf-Of 토큰이 필요합니다.

1. 획득한 토큰은 인증을 받거나 On-Behalf-Of 흐름을 사용하여 좀 더 다운스트림되는 API를 호출하는 데 사용됩니다.

On-Behalf-Of 흐름을 오케스트레이션하는 방법에 대한 지침은 [OAuth 2.0 On-Behalf-Of 흐름](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)을 참조하세요. [다운스트림 웹 API 호출](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/)에서 코드 샘플을 볼 수도 있습니다.

## <a name="next-steps"></a>다음 단계

OAuth 2.0 암시적 허용 흐름을 사용하여 Azure Digital Twins를 구성 및 테스트하려면 [Postman 구성](./how-to-configure-postman.md)을 읽어보세요.

Azure Digital Twins 보안에 대한 자세한 내용은 [역할 할당 만들기 및 관리](./security-create-manage-role-assignments.md)를 참고하세요.