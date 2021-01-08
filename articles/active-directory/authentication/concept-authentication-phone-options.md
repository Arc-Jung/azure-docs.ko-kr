---
title: 전화 인증 방법-Azure Active Directory
description: Azure Active Directory에서 전화 인증 방법을 사용 하 여 로그인 이벤트를 개선 하 고 보안을 유지 하는 방법에 대해 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: justinha
author: justinha
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdff7e62753e75a14d6711b77dd451603353dae5
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98012838"
---
# <a name="authentication-methods-in-azure-active-directory---phone-options"></a>Azure Active Directory 전화 옵션의 인증 방법

문자 메시지를 사용하는 직접 인증의 경우 [SMS 기반 인증(미리 보기)에 대해 사용자를 구성하고 사용하도록 설정](howto-authentication-sms-signin.md)할 수 있습니다. SMS 기반 로그인은 일선 작업자에게 적합합니다. SMS 기반 로그인을 사용하면 사용자가 애플리케이션 및 서비스에 액세스하기 위해 사용자 이름과 암호를 몰라도 됩니다. 대신 사용자가 등록된 휴대폰 번호를 입력하고 확인 코드를 포함하는 문자 메시지를 받은 후 로그인 인터페이스에 입력합니다.

사용자는 Azure AD Multi-Factor Authentication 또는 SSPR (셀프 서비스 암호 재설정) 중에 사용 되는 인증의 보조 양식으로 휴대폰 또는 사무실 전화를 사용 하 여 자신을 확인할 수도 있습니다.

올바르게 작동하려면 전화번호가 *+CountryCode PhoneNumber* 형식으로 저장되어야 합니다(예: *+1 4251234567*).

> [!NOTE]
> 국가/지역 코드와 전화번호 사이에 공백이 필요합니다.
>
> 암호 재설정은 내선 번호를 지원하지 않습니다. *+1 4251234567X12345* 형식에서도 전화를 걸기 전에 내선 번호가 제거됩니다.

## <a name="mobile-phone-verification"></a>휴대폰 확인

Azure AD Multi-Factor Authentication 또는 SSPR의 경우 사용자는 인증 코드를 사용 하 여 로그인 인터페이스에 입력 하거나 전화 통화를 받을 문자 메시지를 받도록 선택할 수 있습니다.

사용자가 자신의 휴대폰 번호를 디렉터리에 표시하지 않는 대신 암호 재설정에는 사용하도록 하려면 관리자가 디렉터리에 해당 휴대폰 번호를 채우지 않아야 합니다. 대신 사용자가 [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)에서 조합된 보안 정보 등록을 통해 **인증 전화** 특성을 채워야 합니다. 관리자는 사용자의 프로필에서 이 정보를 볼 수 있지만 다른 곳에 게시되지는 않습니다.

:::image type="content" source="media/concept-authentication-methods/user-authentication-methods.png" alt-text="전화번호가 채워진 인증 방법을 보여 주는 Azure Portal의 스크린샷":::

Microsoft는 동일한 번호를 사용 하 여 일관 된 SMS 또는 음성 기반 Azure AD Multi-Factor Authentication 메시지를 배달 하는 것을 보장 하지 않습니다. 사용자를 위해 SMS 이행성을 향상하기 위한 조정 작업을 수시로 진행하면서 언제든지 짧은 코드를 추가하거나 제거할 수 있습니다. Microsoft는 미국 및 캐나다 이외의 국가/지역에는 짧은 코드를 지원하지 않습니다.

### <a name="text-message-verification"></a>문자 메시지 확인

SSPR 또는 Azure AD Multi-Factor Authentication 중에 문자 메시지를 확인 하면 SMS가 확인 코드를 포함 하는 휴대폰 번호로 전송 됩니다. 로그인 프로세스를 완료하려면 제공된 확인 코드를 로그인 인터페이스에 입력합니다.

### <a name="phone-call-verification"></a>전화 통화 확인

SSPR 또는 Azure AD Multi-Factor Authentication 중에 전화 통화 확인을 사용 하 여 사용자가 등록 한 전화 번호로 자동 음성 통화가 이루어집니다. 로그인 프로세스를 완료 하려면 해당 키패드에서 #을 누르라는 메시지가 사용자에 게 표시 됩니다.

## <a name="office-phone-verification"></a>사무실 전화 확인

SSPR 또는 Azure AD Multi-Factor Authentication 중에 전화 통화 확인을 사용 하 여 사용자가 등록 한 전화 번호로 자동 음성 통화가 이루어집니다. 로그인 프로세스를 완료 하려면 해당 키패드에서 #을 누르라는 메시지가 사용자에 게 표시 됩니다.

## <a name="troubleshooting-phone-options"></a>전화 옵션 문제 해결

Azure AD에 관한 전화 인증에 문제가 있는 경우 다음 문제 해결 단계를 검토합니다.

* "인증 통화에 대 한 제한에 도달 했습니다." 또는 "로그인 하는 동안 텍스트 확인 코드에 대 한 제한에 도달 했습니다" 라는 오류 메시지가 표시 됩니다.
   * Microsoft는 짧은 시간 동안 동일한 사용자가 수행 하는 반복적인 인증 시도를 제한할 수 있습니다. 이러한 제한 사항은 Microsoft Authenticator 또는 확인 코드에는 적용 되지 않습니다. 이러한 제한에 도달 하면 인증 앱, 확인 코드를 사용 하거나 몇 분 후에 다시 로그인 해 볼 수 있습니다.
* "로그인 하는 동안 계정을 확인 하는 데 문제가 있습니다." 오류 메시지
   * Microsoft는 실패 한 음성 또는 SMS 인증 시도의 수가 많기 때문에 동일한 사용자, 전화 번호 또는 조직에서 수행 하는 음성 또는 SMS 인증 시도를 제한 하거나 차단할 수 있습니다. 이 오류가 발생 하는 경우 인증자 앱 또는 확인 코드와 같은 다른 방법을 시도 하거나 관리자에 게 문의 하 여 지원을 받을 수 있습니다.
* 단일 디바이스에서 차단된 발신자 ID입니다.
   * 디바이스에 구성된 모든 차단된 숫자를 검토합니다.
* 전화번호 또는 국가/지역 코드가 잘못되었거나 개인 전화번호와 회사 전화번호 간에 혼란이 있습니다.
   * 사용자 개체 및 구성된 인증 방법의 문제를 해결합니다. 올바른 전화번호가 등록되었는지 확인합니다.
* 잘못된 PIN을 입력했습니다.
   * 사용자가 자신의 계정에 대해 등록 된 대로 올바른 PIN을 사용 했는지 확인 합니다 (MFA 서버 사용자에만 해당).
* 통화가 음성 사서함으로 전달됩니다.
   * 사용자의 전화기가 켜져 있고 사용자의 지역에서 해당 서비스를 사용할 수 있는지 확인하거나 대체 방법을 사용합니다.
* 사용자가 차단됨
   * Azure AD 관리자가 Azure Portal에서 사용자의 차단을 해제하도록 합니다.
* 디바이스가 SMS에 가입되지 않았습니다.
   * 사용자가 디바이스에서 방법을 변경하거나 SMS를 활성화하도록 합니다.
* 검색된 휴대폰 입력 없음, DTMF 톤 누락 문제, 여러 디바이스에서 차단된 발신자 ID, 여러 디바이스에서 차단된 SMS 등의 결함 있는 통신 공급자
   * Microsoft는 여러 통신 공급자를 사용하여 인증을 위한 전화 통화와 SMS 메시지를 라우팅합니다. 위의 문제가 발생한 경우 사용자가 5분 이내에 5번 이상 방법을 시도하도록 하고 Microsoft 지원에 문의할 때 해당 사용자의 정보를 제공하도록 합니다.

## <a name="next-steps"></a>다음 단계

시작하려면 [SSPR(셀프 서비스 암호 재설정)에 대한 자습서][tutorial-sspr] 및 [Azure AD Multi-Factor Authentication][tutorial-azure-mfa]을 참조하세요.

SSPR 개념에 대한 자세한 내용은 [Azure AD 셀프 서비스 암호 재설정 작동 방법][concept-sspr]을 참조하세요.

MFA 개념에 대해 자세히 알아보려면 [AZURE AD Multi-Factor Authentication 작동 방법][concept-mfa]을 참조 하세요.

[Microsoft Graph REST API 베타](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta&preserve-view=true)를 사용 하 여 인증 방법 구성에 대해 자세히 알아보세요.

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
