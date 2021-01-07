---
title: '자습서: Sansan과 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 Sansan 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/27/2020
ms.author: jeedes
ms.openlocfilehash: 6c35ce5e4b420b7f203b33b640a3175ba4c2739b
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96181512"
---
# <a name="tutorial-integrate-sansan-with-azure-active-directory"></a>자습서: Sansan과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Sansan을 통합하는 방법에 대해 알아봅니다. Azure AD와 Sansan을 통합하는 경우 다음을 수행할 수 있습니다.

* Azure AD에서는 Sansan에 대한 액세스 권한이 있는 사용자를 제어합니다.
* 사용자가 해당 Azure AD 계정으로 Sansan에 자동으로 로그인되도록 합니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* SanSan SSO(Single Sign-On)가 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다.
* Sansan에서 **SP** 시작 SSO를 지원합니다.
* Sansan이 구성되면 세션 제어를 적용하여 조직의 중요한 데이터의 반출 및 반입을 실시간으로 보호할 수 있습니다. 세션 제어는 조건부 액세스에서 확장됩니다. [Microsoft Cloud App Security를 사용하여 세션 제어를 적용하는 방법을 알아봅니다](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-sansan-from-the-gallery"></a>갤러리에서 Sansan 추가

Sansan이 Azure AD에 통합되도록 구성하려면 갤러리의 Sansan을 관리되는 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션** 으로 이동한 다음, **모든 애플리케이션** 을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션** 을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에 **Sansan** 을 입력합니다.
1. 결과 패널에서 **Sansan** 을 선택한 후 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO 구성 및 테스트

테스트 사용자 **Britta Simon** 을 사용하여 Sansan으로 Azure AD SSO를 구성 및 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 Sansan의 관련 사용자 간에 연결이 형성되어야 합니다.

Sansan에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. 사용자가 이 기능을 사용할 수 있도록 **[Azure AD SSO를 구성](#configure-azure-ad-sso)** 합니다.
   * **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
   * **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Sansan 구성](#configure-sansan)** - 애플리케이션 쪽에서 SSO 설정을 구성합니다.
   * **[Sansan 테스트 사용자 만들기](#create-sansan-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Sansan에 만듭니다.
1. **[SSO를 테스트](#test-sso)** 하여 구성이 작동하는지 여부를 확인합니다.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **Sansan** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾은 후 **Single Sign-On** 을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML** 을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성** 에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

1. **기본 SAML 구성** 페이지에서 다음 필드에 대한 값을 입력합니다.

    1. **로그온 URL** 텍스트 상자에 `https://ap.sansan.com/` URL을 입력합니다.

   1. **식별자(엔터티 ID)** 텍스트 상자에 다음 URL을 입력합니다.  
   `https://ap.sansan.com/saml2/<company name>`

   1. **회신 URL** 텍스트 상자에서 다음 패턴을 사용하여 URL 중 하나를 입력합니다.

    
       | Environment | URL |
      |:--- |:--- |
      | PC |`https://ap.sansan.com/v/saml2/<company name>/acs` |
      | Smartphone 앱 |`https://internal.api.sansan.com/<company name>/acs` |
      | Smartphone 웹 |`https://ap.sansan.com/s/saml2/<company name>/acs` |

    > [!NOTE]
    > 이러한 값은 실제 값이 아닙니다. **Sansan 관리자 설정** 에서 실제 값을 확인합니다.

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드** 를 선택하여 인증서를 컴퓨터에 다운로드하고 본인의 컴퓨터에 저장합니다.

   ![인증서 다운로드 링크](common/certificatebase64.png)

1. **Sansan 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

   ![구성 URL 복사](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션에서는 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만듭니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**, **모든 사용자** 를 차례로 선택합니다.
1. 화면 위쪽에서 **새 사용자** 를 선택합니다.
1. **사용자** 속성에서 다음 단계를 수행합니다.
   1. **이름** 필드에 `Britta Simon`을 입력합니다.  
   1. **사용자 이름** 필드에서 username@companydomain.extension을 입력합니다. 예들 들어 `BrittaSimon@contoso.com`입니다.
   1. **암호 표시** 확인란을 선택한 다음, **암호** 상자에 표시된 값을 적어둡니다.
   1. **만들기** 를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Sansan에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션** 을 선택한 다음, **모든 애플리케이션** 을 선택합니다.
1. 애플리케이션 목록에서 **Sansan** 을 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹** 을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가** 를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹** 을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon** 을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

## <a name="configure-sansan"></a>Sansan 구성

**Sansan** 쪽에서 **Single Sign-On 설정** 을 수행하려면 요구 사항에 따라 아래 단계를 수행하세요.

   * [일본어](https://jp-help.sansan.com/hc/ja/articles/900001551383 ) 버전.

   * [영어](https://jp-help.sansan.com/hc/en-us/articles/900001551383 ) 버전.


### <a name="create-sansan-test-user"></a>Sansan 테스트 사용자 만들기

이 섹션에서는 Sansan에서 Britta Simon이라는 사용자를 만듭니다. 사용자를 만드는 방법에 대한 자세한 내용은 [이러한](https://jp-help.sansan.com/hc/en-us/articles/206508997-Adding-users) 단계를 참조하세요.

## <a name="test-sso"></a>SSO 테스트

액세스 패널에서 Sansan 타일을 선택하면 SSO를 설정한 Sansan에 자동으로 로그인됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/my-apps-portal-end-user-access.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](./tutorial-list.md)

- [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory의 조건부 액세스란?](../conditional-access/overview.md)