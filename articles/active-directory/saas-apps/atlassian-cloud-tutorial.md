---
title: '자습서: Atlassian Cloud와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Atlassian Cloud 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: celested
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cdbfb07b73d591bbab64f38e8c986fb1b7e072a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67106581"
---
# <a name="tutorial-integrate-atlassian-cloud-with-azure-active-directory"></a>자습서: Azure Active Directory와 Atlassian Cloud 통합

이 자습서에서는 Atlassian Cloud와 Azure AD(Azure Active Directory)를 통합하는 방법에 대해 알아봅니다. Azure AD와 Atlassian Cloud를 통합하는 경우 다음을 수행할 수 있습니다.

* Azure AD에서는 Atlassian Cloud에 대한 액세스 권한이 있는 사용자를 제어합니다.
* 사용자가 자신의 Azure AD 계정으로 Atlassian Cloud에 자동으로 로그인되도록 설정합니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 다운로드할 수 있습니다.
* Atlassian Cloud SSO(Single Sign-On)를 사용하도록 설정된 구독
* Atlassian Cloud 제품에 SAML(Security Assertion Markup Language) Single Sign-On을 사용하도록 설정하려면 Atlassian Access를 설정해야 합니다. [Atlassian Access]( https://www.atlassian.com/enterprise/cloud/identity-manager)에 대해 자세히 알아보세요.

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다. Atlassian Cloud에서 **SP 및 IDP** 시작 SSO를 지원합니다.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>갤러리에서 Atlassian Cloud 추가

Azure AD에 Atlassian Cloud와 Azure AD를 통합하도록 구성하려면 갤러리의 Atlassian Cloud를 관리되는 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **Atlassian Cloud**를 입력합니다.
1. 결과 패널에서 **Atlassian Cloud**를 선택한 다음 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

**B.Simon**이라는 테스트 사용자를 사용하여 Atlassian Cloud에서 Azure AD SSO를 구성하고 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 Atlassian Cloud의 관련 사용자 간에 링크 관계를 설정해야 합니다.

Atlassian Cloud에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. **[Azure AD SSO 구성](#configure-azure-ad-sso)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Atlassian Cloud SSO 구성](#configure-atlassian-cloud-sso)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - B.Simon을 사용하여 Azure AD Single Sign-On을 테스트합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - B.Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Atlassian Cloud 테스트 사용자 만들기](#create-atlassian-cloud-test-user)** - B.Simon의 Azure AD 표현과 연결된 해당 사용자를 Atlassian Cloud에 만듭니다.
6. **[SSO 테스트](#test-sso)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Atlassian Cloud** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾은 다음, **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

1. **IDP** 시작 모드에서 애플리케이션을 구성하려면 **기본 SAML 구성** 섹션에서 다음 필드 값을 입력합니다.

    a. **식별자** 텍스트 상자에서 `https://auth.atlassian.com/saml/<unique ID>` 패턴을 사용하여 URL을 입력합니다.

    b. **회신 URL** 텍스트 상자에서 `https://auth.atlassian.com/login/callback?connection=saml-<unique ID>` 패턴을 사용하여 URL을 입력합니다.

    다. **추가 URL 설정**을 클릭합니다.

    d. **릴레이 상태** 텍스트 상자에서 `https://<instancename>.atlassian.net` 패턴을 사용하는 URL을 입력합니다.

    > [!NOTE]
    > 위의 값은 실제가 아닙니다. 실제 식별자 및 회신 URL로 해당 값을 업데이트합니다. 이러한 값은 이 자습서의 뒷부분의 **Atlassian Cloud Single Sign-On 구성**에 설명된 **Atlassian Cloud SAML 구성** 화면에서 얻을 수 있습니다.

1. **SP** 시작 모드에서 애플리케이션을 구성하려면 **추가 URL 설정**를 클릭하고 다음 단계를 수행합니다.

    **로그인 URL** 텍스트 상자에서 `https://<instancename>.atlassian.net` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE]
    > 로그온 URL 값은 실제 값이 아닙니다. Atlassian Cloud 관리 포털에 로그인하는 데 사용하는 인스턴스에서 값을 붙여넣습니다.

    ![Single Sign-On 구성](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-10.png)

1. Atlassian Cloud 애플리케이션은 특정 형식의 SAML 어설션을 예상하며, 따라서 SAML 토큰 특성 구성에 사용자 지정 특성 매핑을 추가해야 합니다. 다음 스크린샷에서는 **nameidentifier**가 **user.userprincipalname**과 매핑되는 기본 특성 목록을 보여줍니다. Atlassian Cloud 애플리케이션에서는 **nameidentifier**가 **user.mail**과 매핑되므로 특성 매핑을 변경하려면 **편집** 아이콘을 클릭하고 특성 매핑을 편집해야 합니다.

    ![이미지](common/edit-attribute.png)

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드**를 선택하여 인증서를 컴퓨터에 다운로드하고 본인의 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/certificatebase64.png)

1. **Atlassian Cloud 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-atlassian-cloud-sso"></a>Atlassian Cloud SSO 구성

1. Atlassian Cloud 내에서 구성을 자동화하려면 **확장 설치**를 클릭하여 **내 앱 보안 로그인 브라우저 확장**을 설치해야 합니다.

    ![내 앱 확장](common/install-myappssecure-extension.png)

2. 브라우저에 확장을 추가한 후 **Atlassian Cloud 설정**을 클릭하면 Atlassian Cloud 애플리케이션으로 이동됩니다. 여기에서 관리자 자격 증명을 입력하여 Atlassian Cloud에 로그인합니다. 브라우저 확장에서 애플리케이션을 자동으로 구성하고 3-7단계를 자동화합니다.

    ![설정 구성](common/setup-sso.png)

3. Atlassian Cloud를 수동으로 설정하려면 새 웹 브라우저 창을 열고 Atlassian Cloud 회사 사이트에 관리자로 로그인한 후에 다음 단계를 수행합니다.

4. Single Sign-On을 구성하기 전에 도메인을 확인해야 합니다. 자세한 내용은 [Atlassian 도메인 확인](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) 문서를 참조하세요.

5. 왼쪽 창에서 **보안** > **SAML Single Sign-On**을 선택합니다. 아직 하지 않는 경우 Atlassian Identity Manager를 구독합니다.

    ![Single Sign-On 구성](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-11.png)

6. **Add SAML configuration**(SAML 구성 추가) 창에서 다음을 수행합니다.

    ![Single Sign-On 구성](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-12.png)

    a. **Identity provider Entity ID**(ID 공급자 엔터티 ID) 상자에 Azure Portal에서 복사한 **Azure AD 식별자**를 붙여넣습니다.

    b. **Identity provider SSO URL**(ID 공급자 SSO URL) 상자에 Azure Portal에서 복사한 **로그인 URL**을 붙여넣습니다.

    다. Azure Portal에서 .txt 파일로 다운로드한 인증서를 열고 값(*시작 인증서* 및 *끝 인증서* 행 제외)을 복사하여 **공용 X509 인증서** 상자에 붙여넣습니다.

    d. **구성 저장**을 클릭하십시오.

7. 올바른 URL을 설정했는지 확인하려면 다음을 수행하여 Azure AD 설정을 업데이트합니다.

    ![Single Sign-On 구성](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-13.png)

    a. SAML 창에서 **SP Identity ID**(SP ID)를 복사한 후 Azure Portal의 Atlassian Cloud **기본 SAML 구성** 아래에 있는 **식별자** 상자에 붙여넣습니다.

    b. SAML 창에서 **SP Assertion Consumer Service URL**을 복사한 후 Azure Portal의 Atlassian Cloud **기본 SAML 구성** 아래에 있는 **회신 URL** 상자에 붙여넣습니다. [로그인 URL]은 Atlassian Cloud의 테넌트 URL입니다.

    > [!NOTE]
    > 기존 고객인 경우 Azure Portal에서 **SP Identity ID**(SP ID) 및 **SP Assertion Consumer Service URL** 값을 업데이트한 후 **Yes, update configuration**(예, 구성 업데이트)을 선택합니다. 새 고객인 경우 이 단계를 건너뛸 수 있습니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션에서는 Azure Portal에서 B.Simon이라는 테스트 사용자를 만듭니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**, **모든 사용자**를 차례로 선택합니다.
1. 화면 위쪽에서 **새 사용자**를 선택합니다.
1. **사용자** 속성에서 다음 단계를 수행합니다.
   1. **이름** 필드에 `B.Simon`을 입력합니다.  
   1. **사용자 이름** 필드에서 username@companydomain.extension을 입력합니다. 예: `BrittaSimon@contoso.com`
   1. **암호 표시** 확인란을 선택한 다음, **암호** 상자에 표시된 값을 적어둡니다.
   1. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 B.Simon에게 Atlassian Cloud에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **Atlassian Cloud**를 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **B.Simon**을 선택한 다음, 화면 아래쪽에서 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-atlassian-cloud-test-user"></a>Atlassian Cloud 테스트 사용자 만들기

Azure AD 사용자가 Atlassian Cloud에 로그인하도록 하려면 Atlassian Cloud에서 다음을 수행하여 사용자 계정을 수동으로 프로비전합니다.

1. **관리** 창에서 **사용자**를 선택합니다.

    ![Atlassian Cloud 사용자 링크](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-14.png)

2. Atlassian Cloud에서 사용자를 만들려면 **사용자 초대**를 선택합니다.

    ![Atlassian Cloud 사용자 만들기](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-15.png)

3. **메일 주소** 상자에 사용자의 메일 주소를 입력한 다음 애플리케이션 액세스 권한을 할당합니다.

    ![Atlassian Cloud 사용자 만들기](./media/atlassian-cloud-tutorial/tutorial-atlassiancloud-16.png)

4. 사용자에게 메일 초대를 보내려면 **사용자 초대**를 선택합니다. 사용자에게 메일 초대가 전송되고 사용자가 초대를 수락하면 시스템에서 활성화됩니다.

> [!NOTE]
> **사용자** 섹션에서 **대량 만들기** 단추를 선택하여 사용자를 대량으로 만들 수도 있습니다.

### <a name="test-sso"></a>SSO 테스트

액세스 패널에서 Atlassian Cloud 타일을 선택하면 SSO를 설정한 Atlassian Cloud에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)