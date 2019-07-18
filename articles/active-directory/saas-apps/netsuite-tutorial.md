---
title: '자습서: NetSuite와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 NetSuite 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2600989273d6ebfe4319a048cc65c8c3ff9ecdbc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67096310"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>자습서: NetSuite와 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 NetSuite를 통합하는 방법에 대해 알아봅니다.
NetSuite를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

* Azure AD에서 NetSuite에 액세스할 수 있는 사용자를 제어할 수 있습니다.
* 사용자가 해당 Azure AD 계정으로 NetSuite에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 응용 프로그램 액세스 및 Single Sign-On](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)을 참조하세요.
Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="prerequisites"></a>필수 조건

NetSuite와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

* Azure AD 구독 Azure AD 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 구할 수 있습니다.
* NetSuite Single Sign-On을 사용하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

* NetSuite에서 **IDP** 시작 SSO를 지원합니다.
* NetSuite에서 **Just-In-Time** 사용자 프로비전을 지원합니다.
* NetSuite에서 [자동 사용자 프로비전](NetSuite-provisioning-tutorial.md)을 지원합니다.

## <a name="adding-netsuite-from-the-gallery"></a>갤러리에서 NetSuite 추가

NetSuite를 Azure AD에 통합하도록 구성하려면 갤러리에서 NetSuite를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 NetSuite를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 응용 프로그램**으로 이동한 다음, **모든 응용 프로그램** 옵션을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 애플리케이션을 추가하려면 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **NetSuite**를 입력하고 결과 패널에서 **NetSuite**를 선택한 다음, **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

     ![결과 목록의 NetSuite](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 **Britta Simon**이라는 테스트 사용자를 기반으로 하여 NetSuite에서 Azure AD Single Sign-On을 구성하고 테스트합니다.
Single Sign-On이 작동하려면 Azure AD 사용자와 NetSuite의 관련 사용자 간에 연결 관계를 설정해야 합니다.

NetSuite에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[NetSuite Single Sign-On 구성](#configure-netsuite-single-sign-on)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
3. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[NetSuite 테스트 사용자 만들기](#create-netsuite-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 NetSuite에 만듭니다.
6. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정합니다.

NetSuite에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **NetSuite** 애플리케이션 통합 페이지에서 **Single Sign-On**을 선택합니다.

    ![Single Sign-On 구성 링크](common/select-sso.png)

2. **Single Sign-On 방법 선택** 대화 상자에서 **SAML/WS-Fed** 모드를 선택하여 Single Sign-On을 사용하도록 설정합니다.

    ![Single Sign-On 선택 모드](common/select-saml-option.png)

3. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 아이콘을 클릭하여 **기본 SAML 구성** 대화 상자를 엽니다.

    ![기본 SAML 구성 편집](common/edit-urls.png)

4. **기본 SAML 구성** 섹션에서 다음 단계를 수행합니다.

    ![NetSuite 도메인 및 URL Single Sign-On 정보](common/idp-reply.png)

    **회신 URL** 텍스트 상자에 다음 패턴으로 URL을 입력합니다.

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`

    > [!NOTE]
    > 이 값은 실제 값이 아닙니다. 실제 회신 URL로 값을 업데이트합니다. 해당 값을 얻으려면 [NetSuite 클라이언트 지원 팀](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

5. NetSuite 애플리케이션에는 특정 형식의 SAML 어설션이 필요합니다. 이 애플리케이션에 대해 다음 클레임을 구성합니다. 응용 프로그램 통합 페이지의 **사용자 특성** 섹션에서 이러한 특성의 값을 관리할 수 있습니다. **SAML로 Single Sign-On 설정** 페이지에서 **편집** 단추를 클릭하여 **사용자 특성** 대화 상자를 엽니다.

    ![이미지](common/edit-attribute.png)

6. 위의 이미지와 같이 SAML 토큰 특성을 구성하기 위해 **사용자 특성** 대화 상자의 **사용자 클레임** 섹션에서 **편집 아이콘**을 사용하여 클레임을 편집하거나 **새 클레임 추가**를 사용하여 클레임을 추가하고, 다음 단계를 수행합니다.
    
    | 이름 | 원본 특성 | 
    | ---------------| --------------- |
    | 계정  | `account id` |

    a. **새 클레임 추가**를 클릭하여 **사용자 클레임 관리** 대화 상자를 엽니다.

    ![이미지](common/new-save-attribute.png)

    ![이미지](common/new-attribute-details.png)

    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.

    다. **네임스페이스**를 비워 둡니다.

    d. 원본을 **특성**으로 선택합니다.

    e. **원본 특성** 목록에서 해당 행에 표시된 특성 값을 입력합니다.

    f. **확인**을 클릭합니다.

    g. **저장**을 클릭합니다.

    >[!NOTE]
    >계정 특성의 값은 실제 값이 아닙니다. 이 값은 업데이트되며, 자습서의 뒷부분에서 설명합니다.

4. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **다운로드**를 클릭하여 요구 사항에 따라 제공된 옵션에서 **메타데이터 XML**를 다운로드하고 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/metadataxml.png)

6. **NetSuite 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

    a. 로그인 URL

    b. Azure AD 식별자

    다. 로그아웃 URL

### <a name="configure-netsuite-single-sign-on"></a>NetSuite Single Sign-On 구성

1. 브라우저에서 새 탭을 열고 관리자 권한으로 NetSuite 회사 사이트에 로그인합니다.

2. 페이지의 맨 위에 있는 도구 모음에서 **설정**을 클릭한 다음, **회사**로 이동하고, **기능 사용**을 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-setupsaml.png)

3. 페이지의 가운데에 있는 도구 모음에서 **SuiteCloud**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-suitecloud.png)

4. **인증 관리** 섹션 아래에서 **SAML Single Sign-On**을 선택하여 NetSuite에서 SAML Single Sign-On 옵션을 사용하도록 설정합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-ticksaml.png)

5. 페이지의 위쪽에 있는 도구 모음에서 **설정**을 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-setup.png)

6. **설정 작업** 목록에서 **통합**을 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-integration.png)

7. **인증 관리** 섹션에서 **SAML Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-saml.png)

8. **SAML 설정** 페이지의 **NetSuite 구성** 섹션에서 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. **기본 인증 방법**을 선택합니다.

    b. **SAMLV2 ID 공급자 메타데이터**라는 레이블이 지정된 필드에서 **IDP 메타데이터 파일 업로드**를 선택합니다. 그런 다음 **찾아보기**를 클릭하여 Azure Portal에서 다운로드한 메타데이터 파일을 업로드합니다.

    다. **제출**을 클릭합니다.

9. NetSuite에서 **설정**을 클릭한 다음, **회사**로 이동하고, 위쪽 탐색 메뉴에서 **회사 정보**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-com.png)

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-account-id.png)

    b. 오른쪽 열의 **회사 정보** 페이지에서 **계정 ID**를 복사합니다.

    다. NetSuite 계정에서 복사한 **계정 ID**를 Azure AD의 **특성 값** 필드에 붙여넣습니다. 

10. 사용자가 NetSuite에 Single Sign-On을 수행하려면 먼저 NetSuite에 적절한 권한을 할당해야 합니다. 이러한 사용 권한을 할당하려면 아래 지침을 따릅니다.

    a. 위쪽 탐색 메뉴에서 **설정**을 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-setup.png)

    b. 왼쪽 탐색 메뉴에서 **사용자/역할**을 선택한 다음 **역할 관리**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-manage-roles.png)

    다. **새 역할**을 클릭합니다.

    d. 새 역할에 **이름**을 입력합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-new-role.png)

    e. **저장**을 클릭합니다.

    f. 위쪽에 있는 메뉴에서 **사용 권한**을 클릭합니다. **설치**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-sso.png)

    g. **SAML Single Sign-On**을 선택한 다음, **추가**를 클릭합니다.

    h. **저장**을 클릭합니다.

    i. 위쪽 탐색 메뉴에서 **설치**를 클릭한 다음 **설치 관리자**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-setup.png)

    j. 왼쪽 탐색 메뉴에서 **사용자/역할**을 선택한 다음 **사용자 관리**를 클릭합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-manage-users.png)

    k. 테스트 사용자를 선택합니다. **편집**을 클릭한 다음, **액세스** 탭으로 이동합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-edit-user.png)

    l. 역할 대화 상자에서 만든 적합한 역할을 할당합니다.

    ![Configure Single Sign-On](./media/NetSuite-tutorial/ns-add-role.png)

    m. **저장**을 클릭합니다.

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기 

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**를 차례로 선택하고 **모든 사용자**를 선택합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](common/users.png)

2. 화면 위쪽에서 **새 사용자**를 선택합니다.

    ![새 사용자 단추](common/new-user.png)

3. 사용자 속성에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](common/user-properties.png)

    a. **이름** 필드에 **BrittaSimon**을 입력합니다.
  
    b. **사용자 이름** 필드에 **brittasimon\@yourcompanydomain.extension**을 입력합니다.  
    예를 들어 BrittaSimon@contoso.com

    c. **암호 표시** 확인란을 선택한 다음, [암호] 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 NetSuite에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**, **모든 애플리케이션**, **NetSuite**를 차례로 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **NetSuite**를 입력하고 선택합니다.

    ![애플리케이션 목록의 NetSuite 링크](common/all-applications.png)

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 선택합니다.

    !["사용자 및 그룹" 링크](common/users-groups-blade.png)

4. **사용자 추가** 단추를 클릭한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창](common/add-assign-user.png)

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택하고 화면 아래쪽에서 **선택** 단추를 클릭합니다.

6. SAML 어설션 및 **역할 선택** 대화 상자에서 모든 역할 값이 필요한 경우 목록에서 적절한 사용자 역할을 선택한 다음, 화면 맨 아래에 있는 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="create-netsuite-test-user"></a>NetSuite 테스트 사용자 만들기

이 섹션에서는 NetSuite에서 Britta Simon이라는 사용자를 만듭니다. NetSuite는 기본적으로 사용하도록 설정되는 Just-In-Time 사용자 프로비전을 지원합니다. 이 섹션에 작업 항목이 없습니다. NetSuite에 사용자가 아직 없는 경우 인증 후에 새 사용자가 만들어집니다.

### <a name="test-single-sign-on"></a>Single Sign-On 테스트 

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 NetSuite 타일을 클릭하면 SSO를 설정한 NetSuite에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [사용자 프로비저닝 구성](NetSuite-provisioning-tutorial.md)

