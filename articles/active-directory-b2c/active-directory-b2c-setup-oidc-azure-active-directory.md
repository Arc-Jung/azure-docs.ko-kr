---
title: Azure Active Directory 조직에 대 한 로그인 설정-Azure Active Directory B2C
description: Azure Active Directory B2C에서 특정 Azure Active Directory 조직에 대한 로그인을 설정합니다.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 58c6d1b032f5b492c5641ff51da80426124069b1
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716771"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C에서 특정 Azure Active Directory 조직에 대한 로그인 설정

>[!NOTE]
> 이 기능은 공개 미리 보기 상태입니다. 프로덕션 환경에서는 이 기능을 사용하지 마세요.

Azure AD B2C에서 Azure AD(Azure Active Directory)를 [ID 공급자](active-directory-b2c-reference-oauth-code.md)로 사용하려면 해당 계정을 나타내는 애플리케이션을 만들어야 합니다. 이 문서에서는 Azure AD B2C의 사용자 흐름을 사용하여 특정 Azure AD 조직의 사용자에 대한 로그인을 설정하는 방법을 보여 줍니다.

## <a name="create-an-azure-ad-app"></a>Azure AD 앱 만들기

특정 Azure AD 조직의 사용자에 대한 로그인을 사용하도록 설정하려면 Azure AD B2C 테넌트와 동일하지 않은 조직의 Azure AD 테넌트 내에 애플리케이션을 등록해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. Azure AD 테 넌 트를 포함 하는 디렉터리를 사용 하 고 있는지 확인 합니다. 상단 메뉴에서 **디렉터리 및 구독 필터** 를 선택 하 고 Azure AD 테 넌 트가 포함 된 디렉터리를 선택 합니다. Azure AD B2C 테 넌 트와 같은 테 넌 트가 아닙니다.
3. Azure Portal의 왼쪽 상단 모서리에서 **모든 서비스**를 선택한 다음, **앱 등록**을 검색하여 선택합니다.
4. **새 등록**을 선택합니다.
5. 애플리케이션의 이름을 입력합니다. `Azure AD B2C App` )을 입력합니다.
6. 이 응용 프로그램에 대해서 **만이 조직 디렉터리에서 선택한 계정** 에 동의 합니다.
7. **리디렉션 URI**의 경우 **웹**의 값을 그대로 사용 하 고 다음 URL을 소문자로 입력 합니다. 여기서 `your-B2C-tenant-name` 은 Azure AD B2C 테 넌 트의 이름으로 바뀝니다. 예 `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    이제 모든 URL은 [b2clogin.com](b2clogin.md)을 사용해야 합니다.

8. **등록**을 클릭합니다. 나중에 사용할 **응용 프로그램 (클라이언트) ID** 를 복사 합니다.
9. 응용 프로그램 메뉴에서 **인증서 & 암호** 를 선택 하 고 **새 클라이언트 암호**를 선택 합니다.
10. 클라이언트 암호의 이름을 입력 합니다. `Azure AD B2C App Secret` )을 입력합니다.
11. 만료 기간을 선택 합니다. 이 응용 프로그램의 경우 **1 년의**선택 항목을 수락 합니다.
12. **추가** 를 선택 하 고 나중에 사용할 수 있도록 표시 되는 새 클라이언트 암호의 값을 복사 합니다.

## <a name="configure-azure-ad-as-an-identity-provider"></a>테넌트에서 Azure AD를 ID 공급자로 구성

1. Azure AD B2C 테 넌 트가 포함 된 디렉터리를 사용 하 고 있는지 확인 합니다. 상단 메뉴에서 **디렉터리 및 구독 필터** 를 선택 하 고 Azure AD B2C 테 넌 트를 포함 하는 디렉터리를 선택 합니다.
2. Azure Portal의 왼쪽 상단 모서리에서 **모든 서비스**를 선택하고 **Azure AD B2C**를 검색하여 선택합니다.
3. **ID 공급자**를 선택한 다음, **추가**를 선택합니다.
4. **이름**을 입력합니다. 예를 들어 `Contoso Azure AD`을 입력합니다.
5. **Id 공급자 유형**을 선택 하 고 **Openid connect Connect (미리 보기)** 를 선택한 다음 **확인**을 클릭 합니다.
6. **이 id 공급자 설정** 선택
7. **메타데이터 URL**에 대해 다음 URL을 입력합니다. 여기서 `your-AD-tenant-domain`는 Azure AD 테넌트의 도메인 이름으로 대체됩니다. 예 `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. **클라이언트 id**에 대해 이전에 기록한 응용 프로그램 ID를 입력 하 고 **클라이언트 암호**에 이전에 기록한 클라이언트 암호를 입력 합니다.
9. 선택적으로 **Domain_hint**의 값을 입력합니다. `ContosoAD` )을 입력합니다. 이 값은 요청에서 *domain_hint*를 사용하여 이 ID 공급자를 참조할 때 사용할 값입니다.
10. **확인**을 클릭합니다.
11. **이 ID 공급자의 클레임을 매핑**하고 다음 클레임을 설정하세요.

    - **사용자 ID**에 `oid`를 입력합니다.
    - **표시 이름**에 `name`을 입력합니다.
    - **이름**에 `given_name`을 입력합니다.
    - **성**에 `family_name`을 입력합니다.
    - **메일**에 대해 `unique_name`을 입력합니다.

12. **확인**을 클릭한 다음, **만들기**를 클릭하여 구성을 저장합니다.
