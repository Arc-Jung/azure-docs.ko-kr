---
title: Microsoft Authenticator 앱 (미리 보기)을 사용 하 여 passwordless 로그인-Azure Active Directory
description: Microsoft Authenticator 앱으로 암호를 사용하지 않고 Azure AD에 로그인(공개 미리 보기)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.custom: seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c464874708c7b93ec5620cc9ae253912ce1a4790
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68357145"
---
# <a name="passwordless-phone-sign-in-with-the-microsoft-authenticator-app-public-preview"></a>Microsoft Authenticator 앱으로 암호 없는 휴대폰 로그인(공개 미리 보기)

암호를 사용하지 않고 Microsoft Authenticator 앱을 사용하여 모든 Azure AD 계정에 로그인할 수 있습니다. [비즈니스용 Windows Hello](/windows/security/identity-protection/hello-for-business/hello-identity-verification) 기술과 마찬가지로 Microsoft Authenticator는 키 기반 인증을 사용하여 디바이스에 연결되고 생체 인식 또는 PIN을 사용하는 사용자 자격 증명을 사용하도록 설정합니다.

![사용자가 로그인을 승인 하도록 요청 하는 브라우저 로그인의 예](./media/howto-authentication-phone-sign-in/phone-sign-in-microsoft-authenticator-app.png)

Microsoft Authenticator 앱에서 휴대폰 로그인을 사용하도록 설정한 사람은 사용자 이름을 입력한 후 암호를 묻는 대신 자신의 앱에서 번호를 탭하라는 메시지를 보게 됩니다. 앱에서 사용자는 번호가 일치해야 하고 [승인]을 선택한 다음, PIN 또는 생체 인식을 제공해야 인증이 완료됩니다.

## <a name="enable-my-users"></a>내 사용자를 사용하도록 설정

### <a name="tenant-prerequisites"></a>테넌트 필수 구성 요소

* Azure Active Directory
* Azure Multi-Factor Authentication이 가능한 최종 사용자
* 사용자가 디바이스를 등록할 수 있습니다.

### <a name="steps-to-enable"></a>사용하도록 설정하는 단계

[Azure AD에 대해 암호 없는 로그인 사용](howto-authentication-passwordless-enable.md#enable-new-passwordless-authentication-methods)문서의 단계를 수행 하 여 디렉터리에서 암호 없는 인증 방법을 사용 하도록 설정 합니다.

> [!NOTE]
> 이전에 PowerShell 스크립트를 사용 하 여 테 넌 트에 대해이 기능을 사용 하도록 설정한 경우 사용자 및 그룹에 대 한 새 정책을 설정 하면 기존 테 넌 트 전체 정책을 덮어씁니다. 
>

## <a name="how-do-my-end-users-enable-phone-sign-in"></a>최종 사용자가 휴대폰 로그인을 사용할 수 있도록 하려면 어떻게 하나요?

공개 미리 보기의 경우 사용자가 이 새 자격 증명을 만들거나 사용할 수 있도록 하는 방법은 없습니다. 관리자가 테 넌 트를 사용 하도록 설정 하 고 사용자가 휴대폰 로그인을 사용 하도록 Microsoft Authenticator 앱을 업데이트 한 후에만 최종 사용자에 게 암호 없는 로그인이 발생 합니다.

> [!NOTE]
> 이 기능은 2017년 3월부터 앱에 포함되었으므로 정책이 테넌트에 대해 활성화되면 사용자에게 이 흐름이 즉시 발생할 가능성이 있습니다. 이러한 변화를 인지하고 사용자가 대비하도록 하세요.
>

1. Azure Multi-Factor Authentication에 등록
1. iOS 8.0 이상 또는 Android 6.0 이상을 실행하는 디바이스에 설치된 최신 버전의 Microsoft Authenticator
1. 앱에 푸시 알림이 추가된 직장 또는 학교 계정. 최종 사용자 설명서는 [https://aka.ms/authappstart](https://aka.ms/authappstart)에서 확인할 수 있습니다.

사용자가 Microsoft Authenticator 앱에 푸시 알림이 설정된 MFA 계정을 가지고 있으면 [암호가 아닌 휴대폰을 사용하여 로그인](../user-help/microsoft-authenticator-app-phone-signin-faq.md) 문서의 단계를 수행하여 휴대폰 로그인 등록을 완료할 수 있습니다.

## <a name="known-issues"></a>알려진 문제

### <a name="ad-fs-integration"></a>AD FS 통합

사용자가 Microsoft Authenticator 암호 없는 자격 증명을 사용하도록 설정하면 해당 사용자의 인증은 항상 승인을 위해 알림을 보내는 것으로 기본 설정됩니다. 이 논리는 하이브리드 테넌트의 사용자가 [암호를 대신 사용]을 클릭하는 추가 단계를 수행하지 않고 로그인 확인을 위해 ADFS로 이동하는 것을 방지합니다. 또한 이 프로세스는 온-프레미스 조건부 액세스 정책 및 통과 인증 흐름을 무시합니다. 이 프로세스에 대 한 예외는 login_hint 지정 된 경우 사용자가 AD FS autoforwarded 하 고 암호 없는 자격 증명을 사용 하는 옵션을 무시 하는 것입니다.

### <a name="azure-mfa-server"></a>Azure MFA 서버

조직의 온-프레미스 Azure MFA 서버를 통해 MFA를 사용할 수 있는 최종 사용자는 단일 암호 없는 휴대폰 로그인 자격 증명을 만들고 사용할 수 있습니다. 사용자가 자격 증명으로 Microsoft Authenticator의 여러 설치(5개 이상)를 업그레이드하려고 시도하면 이 변경으로 인해 오류가 발생할 수 있습니다.  

### <a name="device-registration"></a>디바이스 등록

이 새롭고 강력한 자격 증명을 만들기 위한 필수 구성 요소 중 하나는 상주하는 디바이스가 Azure AD 테넌트 내에 개별 사용자에 대해 등록되어 있어야 합니다. 디바이스 등록 제한으로 인해 디바이스는 단일 테넌트에만 등록할 수 있습니다. 이 제한은 Microsoft Authenticator 앱에서 휴대폰 로그인에 대해 하나의 회사 또는 학교 계정만 설정할 수 있음을 의미합니다.

## <a name="next-steps"></a>다음 단계

[Passwordless 란?](concept-authentication-passwordless.md)

[디바이스 등록에 대한 자세한 정보](../devices/overview.md#getting-devices-in-azure-ad)

[Azure Multi-Factor Authentication에 대한 자세한 정보](../authentication/howto-mfa-getstarted.md)
