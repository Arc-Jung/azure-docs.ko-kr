---
title: Azure AD에서 공용 클라이언트 앱 등록-FHIR 용 Azure API
description: 이 문서에서는 Azure에서 FHIR API 배포를 준비 하기 위해 Azure Active Directory에 공용 클라이언트 응용 프로그램을 등록 하는 방법을 설명 합니다.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 3624812cf9119c270c30cf5a7cbebb57030a5113
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020102"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Azure Active Directory에서 공용 클라이언트 응용 프로그램 등록

이 문서에서는 Azure Active Directory에서 공용 응용 프로그램을 등록 하는 방법에 대해 알아봅니다.  

클라이언트 응용 프로그램 등록은 사용자를 대신 하 여 API 권한을 인증 하 고 요청할 수 있는 응용 프로그램의 Azure Active Directory 표현입니다. 공용 클라이언트는 비밀을 비밀으로 유지할 수 없는 모바일 응용 프로그램 및 단일 페이지 JavaScript 응용 프로그램과 같은 응용 프로그램입니다. 이 절차는 [기밀 클라이언트를 등록](register-confidential-azure-ad-client-app.md)하는 것과 유사 하지만 공용 클라이언트는 응용 프로그램 암호를 저장 하기 위해 신뢰할 수 없기 때문에 추가할 필요가 없습니다.

빠른 시작에서는 [Microsoft id 플랫폼을 사용 하 여 응용 프로그램을 등록](../../active-directory/develop/quickstart-register-app.md)하는 방법에 대 한 일반적인 정보를 제공 합니다.

## <a name="app-registrations-in-azure-portal"></a>Azure Portal 앱 등록

1. [Azure Portal](https://portal.azure.com)의 왼쪽 탐색 창에서 **Azure Active Directory** 를 클릭합니다.

2. **Azure Active Directory** 블레이드에서 **앱 등록** 를 클릭 합니다.

    ![Azure Portal. 새 앱 등록.](media/how-to-aad/portal-aad-new-app-registration.png)

3. **새 등록** 을 클릭 합니다.

## <a name="application-registration-overview"></a>응용 프로그램 등록 개요

1. 응용 프로그램에 표시 이름을 지정 합니다.

2. 회신 URL을 제공 합니다. 회신 URL은 클라이언트 응용 프로그램에 인증 코드가 반환 되는 위치입니다. 더 많은 회신 Url을 추가 하 고 나중에 기존 항목을 편집할 수 있습니다.

    ![Azure Portal. 새 공용 앱 등록.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)


[데스크톱](../../active-directory/develop/scenario-desktop-app-registration.md), [모바일](../../active-directory/develop/scenario-mobile-app-registration.md) 또는 [단일 페이지](../../active-directory/develop/scenario-spa-app-registration.md) 응용 프로그램을 공용 응용 프로그램으로 구성 하려면 다음을 수행 합니다.

1. [Azure Portal](https://portal.azure.com)의 **앱 등록** 에서 앱을 선택한 다음 **인증** 을 선택 합니다.

2. **고급 설정**  >  **기본 클라이언트 유형** 을 선택 합니다. **응용 프로그램을 공용 클라이언트로 처리** 에 대해 **예** 를 선택 합니다.

3. 단일 페이지 응용 프로그램의 경우 **액세스 토큰** 및 **ID 토큰** 을 선택 하 여 암시적 흐름을 사용 하도록 설정 합니다.

   - 애플리케이션에서 사용자를 로그인하는 경우 **ID 토큰** 을 선택합니다.
   - 애플리케이션에서 보호된 웹 API를 호출해야 하는 경우에는 **액세스 토큰** 을 선택합니다.

## <a name="api-permissions"></a>API 사용 권한

[기밀 클라이언트 응용 프로그램과](register-confidential-azure-ad-client-app.md)마찬가지로이 응용 프로그램이 사용자를 대신 하 여 요청할 수 있는 API 권한을 선택 해야 합니다.

1. **API 사용 권한을** 엽니다.

    FHIR 용 Azure API를 사용 하는 경우 **조직에서 사용 하는 api** 에서 Azure 의료 api를 검색 하 여 Azure 의료 api에 대 한 사용 권한을 추가 합니다. [FHIR 용 AZURE API를 이미 배포한](fhir-paas-powershell-quickstart.md)경우에만이를 찾을 수 있습니다.

    
    다른 리소스 응용 프로그램을 참조 하는 경우 이전에 **내 api** 에서 만든 [Fhir Api 리소스 응용 프로그램 등록](register-resource-azure-ad-client-app.md) 을 선택 합니다.

    ![Azure Portal. 새 공용 API 권한-Azure API for FHIR 기본값](media/public-client-app/api-permissions.png)


2. 응용 프로그램에서 요청할 수 있는 권한 (Azure Portal)을 선택 ![ 합니다. 앱 사용 권한](media/public-client-app/app-permissions.png)

## <a name="validate-fhir-server-authority"></a>FHIR 서버 인증 기관 유효성 검사
이 문서에 등록 된 응용 프로그램 및 FHIR 서버는 동일한 Azure AD 테 넌 트에 있는 경우 다음 단계를 진행 하는 것이 좋습니다.

FHIR 서버와 다른 Azure AD 테 넌 트에서 클라이언트 응용 프로그램을 구성 하는 경우에는 **권한을** 업데이트 해야 합니다. FHIR 용 Azure API에서 설정--> 인증에서 권한을 설정 합니다. 권한을로 설정 **https://login.microsoftonline.com/\<TENANT-ID>** 합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 Azure Active Directory에 공용 클라이언트 응용 프로그램을 등록 하는 방법에 대해 알아보았습니다. 다음으로 Postman을 사용 하 여 FHIR 서버에 대 한 액세스를 테스트 합니다.
 
>[!div class="nextstepaction"]
>[Postman을 사용하여 Azure API for FHIR 액세스](access-fhir-postman-tutorial.md)