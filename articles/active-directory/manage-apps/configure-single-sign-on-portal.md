---
title: Single Sign-On 구성 - Azure Active Directory | Microsoft Docs
description: 이 자습서는 Azure Portal을 사용하여 Azure AD(Azure Active Directory)에서 애플리케이션에 대해 SAML 기반 Single Sign-On을 구성합니다.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 04/08/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f6707c780931eac58e2a870c321385e63bd948a
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550462"
---
# <a name="tutorial-configure-saml-based-single-sign-on-for-an-application-with-azure-active-directory"></a>자습서: Azure Active Directory에서 애플리케이션에 대한 SAML 기반 Single Sign-On 구성

이 자습서는 [Azure Portal](https://portal.azure.com)을 사용하여 Azure AD(Azure Active Directory)에서 애플리케이션에 대해 SAML 기반 Single Sign-On을 구성합니다. [애플리케이션 관련 자습서](../saas-apps/tutorial-list.md)를 사용할 수 없는 경우 이 자습서를 사용합니다. 

이 자습서에서는 다음 작업에 Azure Portal을 사용합니다.

> [!div class="checklist"]
> * SAML 기반 Single Sign-On 모드 선택
> * 애플리케이션 관련 도메인 및 URL 구성
> * 사용자 특성 구성
> * SAML 서명 인증서 만들기
> * 애플리케이션에 사용자 할당
> * SAML 기반 Single Sign-On에 대한 애플리케이션 구성
> * SAML 설정 테스트

## <a name="before-you-begin"></a>시작하기 전에

1. 애플리케이션이 Azure AD 테넌트에 추가되지 않은 경우 [빠른 시작: Azure AD 테넌트에 애플리케이션 추가](add-application-portal.md)를 참조하세요.

2. [기본 SAML 옵션 구성](#configure-basic-saml-options)에 설명된 정보는 애플리케이션 공급업체에 요청합니다.

3. 비 프로덕션 환경을 사용하여 이 자습서의 단계를 테스트합니다. Azure AD 비-프로덕션 환경이 없으면 [1개월 평가판](https://azure.microsoft.com/pricing/free-trial/)을 받을 수 있습니다.

4. [Azure Portal](https://portal.azure.com)에 Azure AD 테넌트의 클라우드 애플리케이션 관리자 또는 애플리케이션 관리자로 로그인합니다.

## <a name="select-a-single-sign-on-mode"></a>Single Sign-On 모드 선택

애플리케이션이 Azure AD 테넌트에 추가되면 애플리케이션에 대한 Single Sign-On을 구성할 준비가 되었습니다.

Single Sign-On 설정을 열려면:

1. [Azure Portal](https://portal.azure.com)의 왼쪽 탐색 패널에서 **Azure Active Directory**를 선택합니다. 

2. 표시되는 **Azure Active Directory** 탐색 패널의 **관리** 아래에서 **엔터프라이즈 애플리케이션**을 선택합니다. Azure AD 테넌트에 있는 애플리케이션 샘플이 임의로 표시됩니다. 

3. **애플리케이션 유형** 메뉴에서 **모든 애플리케이션**을 선택한 다음, **적용**을 선택합니다.

4. Single Sign-On을 구성하려는 애플리케이션의 이름을 입력합니다. 예를 들어 **GitHub-test**를 입력하여 [애플리케이션 추가](add-application-portal.md) 빠른 시작에서 추가한 애플리케이션을 구성할 수 있습니다.  

     ![애플리케이션 검색 창을 보여주는 스크린샷](media/configure-single-sign-on-portal/azure-portal-application-search.png)

5. Single Sign-On을 구성하려는 애플리케이션을 선택합니다.

6. **관리** 섹션 아래에서 **Single Sign-On**을 선택합니다. 

7. **SAML**을 선택하여 Single Sign-On을 구성합니다. **SAML로 Single Sign-On 설정 - 미리 보기** 페이지가 표시됩니다.

## <a name="configure-basic-saml-options"></a>기본 SAML 옵션 구성

도메인 및 URL을 구성하려면:

1. 다음 설정에 대한 정확한 정보를 가져오려면 애플리케이션 공급 업체에 문의하세요.

    | 구성 설정 | SP 시작 | idP 시작 | 설명 |
    |:--|:--|:--|:--|
    | 식별자(엔터티 ID) | 일부 앱의 경우 필수 | 일부 앱의 경우 필수 | 구성될 Single Sign-On에 대해 애플리케이션을 고유하게 식별합니다. Azure AD는 SAML 토큰의 대상 매개 변수로 애플리케이션에 식별자를 보냅니다. 애플리케이션이 식별자의 유효성을 검사해야 합니다. 또한 이 값은 애플리케이션에서 제공하는 모든 SAML 메타데이터 내에서 엔터티 ID로 표시됩니다.|
    | 회신 URL | 옵션 | 필수 | 애플리케이션이 SAML 토큰을 수신해야 하는 위치를 지정합니다. 회신 URL은 ACS(Assertion Consumer Service) URL이라고도 합니다. |
    | 로그온 URL | 필수 | 지정하지 않음 | 사용자가 이 URL을 열면 서비스 공급자가 Azure AD를 리디렉션하여 사용자를 인증하고 로그온하도록 합니다. Azure AD는 URL을 사용하여 Office 365 또는 Azure AD 액세스 패널에서 애플리케이션을 시작합니다. 비어 있는 경우 사용자가 애플리케이션을 시작할 때 Azure AD에서 ID 공급자를 사용하여 Single Sign-On을 시작합니다.|
    | 릴레이 상태 | 옵션 | 옵션 | 인증이 완료되면 사용자를 리디렉션할 위치를 애플리케이션에 지정합니다. 일반적으로 이 값은 애플리케이션에 대한 올바른 URL입니다. 그러나 일부 애플리케이션에서는 이 필드를 다르게 사용합니다. 자세한 내용은 애플리케이션 공급 업체에 요청하세요.
    | 로그아웃 URL | 옵션 | 옵션 | SAML 로그아웃 응답을 애플리케이션에 다시 보내는 데 사용됩니다.


2. 기본 SAML 구성 옵션을 편집하려면 **기본 SAML 구성** 섹션의 오른쪽 위 모서리에 있는 **편집** 아이콘(연필 모양)을 선택합니다.

     ![인증서 구성](media/configure-single-sign-on-portal/basic-saml-configuration-edit-icon.png)

3. 1단계에서 애플리케이션 공급업체가 제공한 정보를 페이지의 해당 필드에 입력합니다.

4. 페이지 맨 위에서 **저장**을 선택합니다.

## <a name="configure-user-attributes-and-claims"></a>사용자 특성 및 클레임 구성 

사용자가 로그인할 때 Azure AD가 SAML 토큰에서 애플리케이션에 보내는 정보를 제어할 수 있습니다. 이 정보는 사용자 특성을 구성하여 제어합니다. 예를 들어 사용자가 로그인할 때 사용자의 이름, 이메일 및 직원 ID를 애플리케이션에 보내도록 Azure AD를 구성할 수 있습니다. 

이러한 특성은 Single Sign-On이 제대로 작동하기 위해 필수 또는 선택 사항일 수 있습니다. 자세한 내용은 [애플리케이션 관련 자습서](../saas-apps/tutorial-list.md) 또는 애플리케이션 공급 업체를 참조하세요.

1. 사용자 특성 및 클레임을 편집하려면 **사용자 특성 및 클레임** 섹션의 오른쪽 위 모서리에 있는 **편집** 아이콘(연필 모양)을 선택합니다.

   **이름 식별자 값**은 *user.principalname*(기본값)으로 설정되어 있습니다. 사용자 ID는 애플리케이션 내에서 각 사용자를 고유하게 식별합니다. 예를 들어 이메일 주소가 사용자 이름 및 고유 식별자 모두인 경우 값을 *user.mail*로 설정합니다.

2. **이름 식별자 값**을 수정하려면 **이름 식별자 값** 필드에 대한 **편집** 아이콘(연필 모양)을 선택합니다. 필요에 따라 식별자 형식과 원본을 적절하게 변경합니다. 완료되면 변경 내용을 저장합니다. 클레임 사용자 지정에 대한 자세한 내용은 [엔터프라이즈 애플리케이션에 대한 SAML 토큰에서 발급된 클레임 사용자 지정](../develop/active-directory-saml-claims-customization.md) 문서를 참조하세요.

3. 클레임을 추가하려면 페이지의 위쪽에서 **새 클레임 추가**를 선택합니다. **이름**을 입력하고 적절한 원본을 선택합니다. **특성** 원본을 선택하는 경우 사용하려는 **원본 특성**을 선택해야 합니다. **번역** 원본을 선택하는 경우 사용하려는 **변환** 및 **매개 변수 1**을 선택해야 합니다.

4. **저장**을 선택합니다. 테이블에 새 클레임이 표시됩니다.
 
## <a name="generate-a-saml-signing-certificate"></a>SAML 서명 인증서 생성

Azure AD에서는 인증서를 사용하여 애플리케이션에 보내는 SAML 토큰에 서명합니다. 

1. 새 인증서를 생성하려면 **SAML 서명 인증서** 섹션의 오른쪽 위 모서리에 있는 **편집** 아이콘(연필 모양)을 선택합니다.

2. **SAML 서명 인증서** 섹션 아래에서 **새 인증서**를 선택합니다.

3. 표시되는 새 인증서 행에서 **만료 날짜**를 설정합니다. 사용 가능한 구성 옵션에 대한 자세한 내용은 [고급 인증서 서명 옵션](certificate-signing-options.md) 문서를 참조하세요.

4. **SAML 서명 인증서** 섹션의 위쪽에서 **저장**을 선택합니다. 

## <a name="assign-users-to-the-application"></a>애플리케이션에 사용자 할당

애플리케이션을 조직에 롤아웃하기 전에 여러 사용자 또는 그룹에서 Single Sign-On을 테스트하는 것이 좋습니다.

> [!NOTE]
>
> 다음 단계에서는 포털의 **사용자 및 그룹** 구성 섹션으로 이동합니다. 완료되면 **Single Sign-On** 섹션으로 다시 이동하여 자습서를 완료해야 합니다.

애플리케이션에 사용자 또는 그룹을 할당하려면:

1. 애플리케이션이 열려 있지 않으면 포털에서 엽니다.
2. 애플리케이션에 대한 왼쪽 탐색 패널에서 **사용자 및 그룹**을 선택합니다.
3. **사용자 추가**를 선택합니다.
4. **할당 추가** 섹션에서 **사용자 및 그룹**을 선택합니다.
5. 특정 사용자를 찾으려면 **멤버 선택 또는 외부 사용자 초대** 상자에서 사용자 이름을 입력합니다. 그런 다음, 사용자의 프로필 사진 또는 로고를 선택한 다음, **선택**을 선택합니다. 
6. **할당 추가** 섹션에서 **할당**을 선택합니다. 완료되면 선택한 사용자가 **사용자 및 그룹** 목록에 표시됩니다.

## <a name="set-up-the-application-to-use-azure-ad"></a>Azure AD를 사용하도록 애플리케이션 설정

이제 거의 완료되었습니다.  마지막 단계로, Azure AD를 SAML ID 공급자로 사용하도록 애플리케이션을 설정해야 합니다. 

1. **\<applicationName> 설정** 섹션까지 아래로 스크롤합니다. 이 자습서에서 이 섹션은 **GitHub-test 설정**이라고 합니다. 
2. 이 섹션에서 각 행의 값을 복사합니다. 그런 다음, **기본 SAML 구성** 섹션의 해당 행에 각 값을 붙여넣습니다. 예를 들어 **GitHub-test 설정** 섹션에서 **로그인 URL** 값을 복사하여 **기본 SAML 구성** 섹션의 **로그온 URL** 필드 등에 붙여넣습니다.
3. 모든 값을 해당 필드에 붙여넣은 다음, **저장**을 선택합니다.

## <a name="test-single-sign-on"></a>Single Sign-On 테스트

설정을 테스트할 준비가 되었습니다.  

1. 애플리케이션의 Single Sign-On 설정을 엽니다. 
2. **\<applicationName>을 사용하여 Single Sign-On 유효성 검사** 섹션으로 스크롤합니다. 이 자습서에서 이 섹션은 **GitHub-test 설정**이라고 합니다.
3. **테스트**를 선택합니다. 테스트 옵션이 표시됩니다.
4. **현재 사용자로 로그인**을 선택합니다. 이 테스트를 통해 관리자인 사용자에 대해 Single Sign-On이 작동하는지를 먼저 확인할 수 있습니다.

오류가 있으면 오류 메시지가 표시됩니다. 다음 단계를 완료합니다.

1. 세부 정보를 **어떤 오류인 것 같습니까?** 상자에 복사하고 붙여넣습니다.

    ![해결 지침 가져오기](media/configure-single-sign-on-portal/error-guidance.png)

2. **해결 지침 가져오기**를 선택합니다. 근본 원인 및 해결 지침이 표시됩니다.  이 예제에서 사용자는 애플리케이션에 할당되지 않았습니다.

3. 해결 지침을 읽은 다음, 가능한 경우 문제를 해결합니다.

4. 성공적으로 완료될 때까지 테스트를 다시 실행합니다.

## <a name="next-steps"></a>다음 단계
이 자습서에서는 애플리케이션에 대한 Single Sign-On 설정을 구성했습니다. 구성을 마치면 애플리케이션에 사용자를 할당하고, SAML 기반 Single Sign-On을 사용하도록 애플리케이션을 구성했습니다. 이 작업이 모두 완료되면 SAML 로그온이 제대로 작동하는지 확인했습니다.

다음 작업을 수행했습니다.
> [!div class="checklist"]
> * Single Sign-On 모드에 대해 SAML 선택
> * 도메인 및 URL을 구성하기 위해 애플리케이션 공급 업체 연결
> * 사용자 특성 구성
> * SAML 서명 인증서 생성
> * 애플리케이션에 사용자 또는 그룹 수동으로 할당
> * Azure AD를 SAML ID 공급자로 사용하도록 애플리케이션 구성
> * SAML 기반 Single Sign-On 테스트

조직에서 더 많은 사용자에게 애플리케이션을 롤아웃하려면 자동 사용자 프로비저닝을 사용합니다.

> [!div class="nextstepaction"]
> [자동 프로비전을 사용하여 사용자를 할당하는 방법 알아보기](configure-automatic-user-provisioning-portal.md)


