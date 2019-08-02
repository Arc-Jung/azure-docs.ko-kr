---
title: Azure AD 셀프 서비스 암호 재설정 사용자 지정-Azure Active Directory
description: Azure AD 셀프 서비스 암호 재설정의 사용자 지정 옵션
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 527dd99f122ec70cc47305947a5cbce3207b9664
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68666309"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Azure AD의 셀프 서비스 암호 재설정 기능 사용자 지정

Azure Active Directory(Azure AD)에서 셀프 서비스 암호 재설정(SSPR)을 배포하려는 IT 전문가는 사용자들의 필요에 맞게 환경을 사용자 지정할 수 있습니다.

## <a name="customize-the-contact-your-administrator-link"></a>“관리자에게 문의” 링크 사용자 지정

셀프 서비스 암호 재설정 사용자는 암호 재설정 포털에서 "관리자에 게 문의" 링크를 사용할 수 있습니다. 사용자가이 링크를 선택 하면 다음 두 가지 중 하나를 수행 합니다.

* 기본 상태로 유지 되는 경우:
   * 전자 메일이 관리자에 게 전송 되 고 사용자 암호 변경에 대 한 지원을 제공 하도록 요청 합니다. 아래의 [샘플 전자 메일](#sample-email) 을 참조 하세요.
* 사용자 지정 된 경우:
   * 사용자를 관리자가 지정한 웹 페이지 또는 전자 메일 주소로 보내 도움을 요청 합니다.

> [!TIP]
> 이를 사용자 지정 하는 경우 사용자가 지원 하기 위해 이미 익숙한 항목으로 설정 하는 것이 좋습니다.

> [!WARNING]
> 암호를 재설정 해야 하는 전자 메일 주소 및 계정을 사용 하 여이 설정을 사용자 지정 하는 경우 사용자가 도움을 요청 하지 못할 수 있습니다.

### <a name="sample-email"></a>샘플 메일

![관리자에 게 보낸 전자 메일 재설정 요청 샘플][Contact]

문의 메일은 다음과 같은 순서로 받는 사람에게 전송됩니다.

1. **암호 관리자** 역할이 할당된 경우 이 역할을 갖는 관리자에게 전송됩니다.
2. 암호 관리자가 할당되지 않은 경우 **사용자 관리자** 역할을 갖는 관리자에게 전송됩니다.
3. 암호 관리자와 사용자 관리자가 할당되지 않은 경우 **전역 관리자**에게 전송됩니다.

어떠한 경우에도 최대 100명의 받는 사람에게 알림이 제공됩니다.

다양한 관리자 역할과 각 역할을 할당하는 방법에 대해 자세히 알아보려면 [Azure Active Directory에서 관리자 역할 할당](../users-groups-roles/directory-assign-admin-roles.md)을 참조하세요.

### <a name="disable-contact-your-administrator-emails"></a>“관리자에게 문의” 메일 사용 안 함

관리자에게 암호 재설정 요청을 알리지 않으려는 경우 다음 구성을 사용할 수 있습니다.

* 모든 최종 사용자에게 셀프 서비스 암호 재설정 사용 이 옵션은 **암호 재설정** > **속성** 아래에 있습니다. 사용자가 자신의 암호를 재설정할 수 없도록 하려면 빈 그룹에 대한 액세스 범위를 지정하면 됩니다. *이 옵션은 권장되지 않습니다.*
* 사용자가 도움을 받을 수 있는 웹 URL 또는 mailto: 주소를 제공하는 고객 지원 센터 링크를 사용자 지정합니다. 이 옵션은 **암호 재설정** > **사용자 지정** >  **사용자 지정 기술 지원 팀 메일 또는 URL** 아래에 있습니다.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>SSPR을 위해 AD FS 로그인 페이지 사용자 지정

AD FS(Active Directory Federation Services) 관리자는 [로그인 페이지 설명 추가](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) 문서의 지침에 따라 로그인 페이지에 링크를 추가할 수 있습니다.

AD FS 로그인 페이지에 링크를 추가하려면 AD FS 서버에서 아래 명령을 사용합니다. 이렇게 하면 사용자가 이 페이지에서 SSPR 워크플로우를 시작할 수 있습니다.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>로그인 페이지와 액세스 패널의 디자인 사용자 지정

로그인 페이지를 사용자 지정할 수 있습니다. 회사 브랜드에 맞는 이미지와 로고를 추가할 수 있습니다.

이미지는 다음과 같은 경우에 표시됩니다.

* 사용자가 사용자 이름을 입력한 뒤
* 사용자가 다음과 같은 방식으로 사용자 지정된 URL에 액세스하는 경우
   * 다음과 같이 `whr` 매개 변수를 암호 재설정 페이지에 전달 합니다.`https://login.microsoftonline.com/?whr=contoso.com`
   * 다음과 같이 `username` 매개 변수를 암호 재설정 페이지에 전달 합니다.`https://login.microsoftonline.com/?username=admin@contoso.com`

회사 브랜딩을 구성하는 방법에 대한 자세한 내용은 문서 [Azure AD에서 로그인 페이지에 회사 브랜딩 추가](../fundamentals/customize-branding.md)에서 찾습니다.

### <a name="directory-name"></a>디렉터리 이름

**Azure Active Directory** > **속성**에서 디렉터리 이름 속성을 변경할 수 있습니다. 여기에서 포털과 자동 커뮤니케이션에 사용할 조직 이름을 입력할 수 있습니다. 이 옵션은 다음과 같은 자동 메일에 표시됩니다.

* 메일의 식별 이름(예: “CONTOSO 데모를 대신하는 Microsoft”)
* 메일의 제목 줄(예: “CONTOSO 데모 계정 메일 확인 코드”)

## <a name="next-steps"></a>다음 단계

* [성공적인 SSPR 롤아웃을 어떻게 완료합니까?](howto-sspr-deployment.md)
* [암호 재설정 또는 변경](../user-help/active-directory-passwords-update-your-own-password.md)
* [셀프 서비스 암호 재설정 등록](../user-help/active-directory-passwords-reset-register.md)
* [라이선스 관련 질문이 있습니까?](concept-sspr-licensing.md)
* [SSPR에서 사용하는 데이터는 무엇이며, 사용자에 대해 어떤 데이터를 채워야 합니까?](howto-sspr-authenticationdata.md)
* [사용자가 사용할 수 있는 인증 방법은 무엇입니까?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR에서 사용하는 정책 옵션은 무엇입니까?](concept-sspr-policy.md)
* [비밀번호 쓰기 저장은 무엇이며, 왜 관심을 가져야 합니까?](howto-sspr-writeback.md)
* [SSPR 작업은 어떻게 보고 합니까?](howto-sspr-reporting.md)
* [모든 SSPR 옵션과 그 의미는 무엇입니까?](concept-sspr-howitworks.md)
* [무엇인가 손상된 문제가 있습니다. SSPR 문제는 어떻게 해결합니까?](active-directory-passwords-troubleshoot.md)
* [다른 곳에서 다루지 않았던 질문이 있습니다.](active-directory-passwords-faq.md)

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "암호 전자 메일을 다시 설정 하는 방법에 대 한 도움말은 관리자에 게 문의 하세요."
