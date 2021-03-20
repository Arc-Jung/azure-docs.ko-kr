---
title: 이메일 주소를 확인 방법으로 설정 - Azure AD
description: 확인 방법으로 이메일 주소를 사용하여 ID를 확인하는 보안 정보(미리 보기) 페이지를 설정하는 방법입니다.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 4f0ea55149fc8cfeeb6ef719d67346019b81078a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "83744461"
---
# <a name="set-up-an-email-address-as-your-verification-method"></a>이메일 주소를 확인 방법으로 설정

다음 단계에 따라 암호 재설정 방법을 추가할 수 있습니다. 이 정보를 처음 설정한 후 **보안 정보** 페이지로 돌아가서 보안 정보를 추가, 업데이트 또는 삭제할 수 있습니다.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>이메일 옵션이 표시되지 않으면 조직에서 암호 재설정 방법에 이 옵션을 사용하지 못하게 할 수 있습니다. 이 경우 다른 방법을 선택하거나 조직의 지원 데스크에 문의하여 자세한 도움을 요청해야 합니다.

## <a name="security-vs-password-reset-verification"></a>보안 및 암호 재설정 확인

보안 정보 방법은 2단계 보안 확인 및 암호 재설정에 모두 사용됩니다. 그러나 모든 보안 정보 방법을 둘 모두에 사용할 수 있는 것은 아닙니다.

| 방법 | 사용 대상 |
| ------ | -------- |
| 인증자 앱 | 2단계 인증 및 암호 재설정 인증입니다. |
| 문자 메시지 | 2단계 인증 및 암호 재설정 인증입니다. |
| 전화 통화 | 2단계 인증 및 암호 재설정 인증입니다. |
| 보안 키 | 2단계 인증 및 암호 재설정 인증입니다. |
| 이메일 계정 | 암호 재설정 인증 전용입니다. 2단계 인증에는 다른 보안 정보 방법을 선택해야 합니다. |
| 보안 질문 | 암호 재설정 인증 전용입니다. 2단계 인증에는 다른 보안 정보 방법을 선택해야 합니다. |

## <a name="set-up-your-email-address-from-the-security-info-page"></a>보안 정보 페이지에서 이메일 주소 설정

조직의 설정에 따라 이메일 주소를 보안 정보 방법 중 하나로 사용할 수 있습니다.

>[!Note]
>액세스를 위해 네트워크 암호를 요구하지 않는 이메일 주소를 사용하는 것이 좋습니다. 이메일 옵션을 보이지 않으면 조직에서 확인을 위해 이메일 사용을 허용하지 않는 것일 수 있습니다. 이 경우 다른 방법을 선택하거나 관리자에게 문의하여 자세한 도움을 받아야 합니다.

### <a name="to-set-up-your-email-address"></a>이메일 주소를 설정하려면

1. 회사 또는 학교 계정에 로그인한 다음, https://myaccount.microsoft.com/ 페이지로 이동합니다.

    ![보안 정보 링크가 강조 표시된 내 프로필 페이지](media/security-info/securityinfo-myprofile-page.png)

2. 왼쪽 탐색 창 또는 **보안 정보** 블록의 링크에서 **보안 정보** 를 선택한 다음, **보안 정보** 페이지에서 **메서드 추가** 를 선택합니다.

    ![메서드 추가 옵션이 강조 표시된 보안 정보 페이지](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. **방법 추가** 페이지의 드롭다운 목록에서 **이메일** 을 선택한 다음, **추가** 를 선택합니다.

    ![이메일이 선택된 방법 추가 상자](media/security-info/securityinfo-myprofile-addemail.png)

4. **이메일** 페이지에서 이메일 주소(예: alain@gmail.com)를 선택한 후 **다음** 을 선택합니다.

    ![전화 번호를 추가하고 전화 통화 선택](media/security-info/securityinfo-myprofile-emailaddress.png)

    >[!Important]
    >이 이메일 주소는 회사 또는 학교 이메일일일 수 없습니다.

5. 지정된 이메일 주소로 전송된 코드를 입력하고 **다음** 을 선택합니다.

    ![전화 번호를 추가하고 문자 메시지 선택](media/security-info/securityinfo-myprofile-emailcode.png)

    암호 재설정을 사용할 때 메일 주소를 사용하여 본인 여부를 확인하도록 보안 정보가 업데이트됩니다.

## <a name="delete-your-email-address-from-your-security-info-methods"></a>보안 정보 방법에서 이메일 주소 삭제

더 이상 이메일 주소를 보안 정보 방법으로 사용하지 않으려는 경우 **보안 정보** 페이지에서 제거할 수 있습니다.

>[!Important]
>실수로 이메일 주소를 삭제하면 실행 취소할 수 없습니다. 이 문서의 [이메일 주소 설정](#set-up-your-email-address-from-the-security-info-page) 섹션에 있는 단계에 따라 방법을 다시 추가해야 합니다.

### <a name="to-delete-your-email-address"></a>메일 주소를 삭제하려면

1. **보안 정보** 페이지에서 **이메일** 옵션 옆에 있는 **삭제** 링크를 선택합니다.

    ![보안 정보에서 전화 방법을 삭제하기 위한 링크](media/security-info/securityinfo-myprofile-emaildelete.png)

2. 확인 상자에서 **예** 를 선택하여 **이메일** 계정을 삭제합니다. 이메일 계정이 삭제되면 보안 정보에서 제거되고 **보안 정보** 페이지에서 사라집니다.

## <a name="additional-security-info-methods"></a>추가 보안 정보 메서드

수행하려는 작업에 따라, 조직이 본인 여부를 확인하기 위해 사용자에게 연락하는 방식에 대한 추가 옵션이 제공됩니다. 옵션에는 다음이 포함됩니다.

- **Authenticator 앱.** Authenticator 앱을 다운로드한 후 사용하여 2단계 인증 또는 암호 재설정을 위한 승인 알림 또는 임의로 생성된 승인 코드를 가져올 수 있습니다. Microsoft Authenticator 앱을 설치 및 사용하는 방법에 대한 단계별 지침은 [Authenticator 앱을 사용하도록 보안 정보 설정](security-info-setup-auth-app.md)을 참조하세요.

- **모바일 디바이스 텍스트.** 모바일 디바이스 번호를 입력하면 2단계 인증 또는 암호 재설정에 사용할 코드가 포함된 문자를 받습니다. 문자 메시지(SMS)를 사용하여 본인 여부를 확인하는 방법에 대한 단계별 지침은 [문자 메시지(SMS)를 사용하도록 보안 정보 설정](security-info-setup-text-msg.md)을 참조하세요.

- **모바일 디바이스 또는 회사 전화 통화.** 모바일 디바이스 번호를 입력하면 2단계 인증 또는 암호 재설정을 위한 전화가 걸려옵니다. 전화 번호를 사용하여 본인 여부를 확인하는 방법에 대한 단계별 지침은 [전화 통화를 사용하도록 보안 정보 설정](security-info-setup-phone-number.md)을 참조하세요.

- **보안 키.** Microsoft 호환 보안 키를 등록하고 2단계 인증 또는 암호 재설정을 위해 PIN과 함께 사용합니다. 보안 키를 사용하여 ID를 확인하는 방법에 대한 단계별 지침은 [보안 키를 사용하도록 보안 정보 설정](security-info-setup-security-key.md)을 참조하세요.

- **보안 질문.** 조직의 관리자가 만든 몇 가지 보안 질문에 대답합니다. 이 옵션은 2단계 인증이 아닌 암호 재설정에만 사용할 수 있습니다. 보안 질문을 설정하는 방법에 대한 단계별 지침은 [보안 질문을 사용하도록 보안 정보 설정](security-info-setup-questions.md) 문서를 참조하세요.

    >[!Note]
    >이러한 옵션 중 일부가 누락된 경우 사용자의 조직에서 해당 방법을 허용하지 않았을 가능성이 있습니다. 이 경우 사용 가능한 방법을 선택하거나 관리자에게 문의하여 자세한 도움을 받아야 합니다.

## <a name="next-steps"></a>다음 단계

- 분실했거나 잊어버린 경우 [암호 재설정 포털](https://passwordreset.microsoftonline.com/)에서 암호를 다시 설정하거나 [회사 또는 학교 암호 재설정](active-directory-passwords-update-your-own-password.md) 문서의 단계를 수행합니다.

- [Microsoft 계정에 로그인할 수 없는 경우](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) 문서에서 로그인 문제에 대한 문제 해결 팁 및 도움말을 확인합니다.
