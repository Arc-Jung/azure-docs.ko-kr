---
title: 'Azure AD Connect: Azure AD 용 페더레이션에서 PTA로 마이그레이션'
description: 이 문서에는 하이브리드 ID 환경을 페더레이션에서 통과 인증으로 전환하는 방법에 대한 정보가 나와 있습니다.
services: active-directory
author: billmath
manager: daveba
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/29/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e5a5b06bc95d022cfad66118db4b55e9369b5bd
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661889"
---
# <a name="migrate-from-federation-to-pass-through-authentication-for-azure-active-directory"></a>Azure Active Directory를 페더레이션에서 통과 인증으로 마이그레이션

이 문서에서는 조직 도메인을 AD FS(Active Directory Federation Services)에서 통과 인증으로 전환하는 방법에 대해 설명합니다.

> [!NOTE]
> 인증 방법을 변경하려면 계획, 테스트, 경우에 따라 가동 중지 시간이 필요합니다. [단계적 출시](how-to-connect-staged-rollout.md) 는 통과 인증을 사용 하 여 페더레이션에서 클라우드 인증으로 테스트 하 고 점진적으로 마이그레이션하는 대체 방법을 제공 합니다.
> 
> 단계적 출시를 사용 하려는 경우에는 롤아웃이 완료 되 면 준비 된 롤아웃 기능을 해제 해야 합니다.  자세한 내용은 준비 된 [롤아웃을 사용 하 여 클라우드 인증으로 마이그레이션을](how-to-connect-staged-rollout.md) 참조 하세요.


## <a name="prerequisites-for-migrating-to-pass-through-authentication"></a>통과 인증으로 마이그레이션하기 위한 필수 조건

AD FS 사용에서 통과 인증 사용으로 마이그레이션하는 데 필요한 필수 조건은 다음과 같습니다.

### <a name="update-azure-ad-connect"></a>Azure AD Connect 업데이트

통과 인증을 사용하도록 마이그레이션하는 데 필요한 단계를 성공적으로 완료하려면 [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594)(Azure Active Directory Connect) 1.1.819.0 이상이 있어야 합니다. 로그인 변환을 수행하는 방법이 Azure AD Connect 1.1.819.0에서 크게 변경되었습니다. 이 버전에서는 AD FS에서 클라우드 인증으로 마이그레이션하는 데 걸리는 전체 시간이 잠재적으로 몇 시간에서 몇 분으로 줄어듭니다.

> [!IMPORTANT]
> 도메인을 페더레이션 ID에서 관리 ID로 변환할 때 사용자 변환이 필요한 오래된 문서, 도구 및 블로그에서 읽을 수 있습니다. *사용자 변환*은 더 이상 필요하지 않습니다. Microsoft는 이러한 변경을 반영하기 위해 설명서와 도구를 업데이트하려고 노력하고 있습니다.

Azure AD Connect를 업데이트 하려면 [Azure AD Connect: 최신 버전으로 업그레이드](./how-to-upgrade-previous-version.md)의 단계를 완료 합니다.

### <a name="plan-authentication-agent-number-and-placement"></a>인증 에이전트 번호 및 배치 계획

통과 인증을 사용하려면 Azure AD Connect 서버와 Windows 서버를 실행하는 온-프레미스 컴퓨터에 경량 에이전트를 배포해야 합니다. 대기 시간을 줄이려면 Active Directory 도메인 컨트롤러에 최대한 가깝게 에이전트를 설치합니다.

대부분의 고객의 경우 고가용성과 필요한 용량을 제공하는 데 2~3개의 인증 에이전트로 충분합니다. 테넌트는 최대 12개의 에이전트를 등록할 수 있습니다. 첫 번째 에이전트는 항상 Azure AD Connect 서버 자체에 설치됩니다. 에이전트 제한 사항 및 에이전트 배포 옵션에 대 한 자세한 내용은 [AZURE AD 통과 인증: 현재 제한 사항](./how-to-connect-pta-current-limitations.md)을 참조 하세요.

### <a name="plan-the-migration-method"></a>마이그레이션 방법 계획

두 가지 방법 중 하나를 선택하여 페더레이션 ID 관리에서 통과 인증 및 SSO(Single Sign-On)로 마이그레이션할 수 있습니다. 사용하는 방법은 AD FS 인스턴스를 처음 구성한 방법에 따라 달라집니다.

* **Azure AD Connect**. 원래 Azure AD Connect를 사용하여 AD FS를 구성한 경우 Azure AD Connect 마법사를 사용하여 통과 인증으로 *변경해야 합니다*.

   ‎Azure AD Connect는 사용자 로그인 방법을 변경할 때 **Set-MsolDomainAuthentication** cmdlet을 자동으로 실행합니다. Azure AD Connect는 Azure AD 테넌트에서 확인된 모든 페더레이션된 도메인을 자동으로 페더레이션 해제합니다.

   > [!NOTE]
   > 현재 원래 Azure AD Connect를 사용하여 AD FS를 구성한 경우 사용자 로그인을 통과 인증으로 변경하면 테넌트에 있는 모든 도메인의 페더레이션이 해제됩니다.
‎
* **PowerShell을 사용하는 Azure AD Connect**. 이 방법은 원래 Azure AD Connect를 사용하여 AD FS를 구성하지 않은 경우에만 사용할 수 있습니다. 이 옵션에서는 여전히 Azure AD Connect 마법사를 통해 사용자 로그인 방법을 변경해야 합니다. 이 옵션의 핵심적인 차이점은 마법사에서 **Set-MsolDomainAuthentication** cmdlet을 자동으로 실행하지 않는다는 것입니다. 이 옵션을 사용하면 변환할 도메인과 순서를 완전히 제어할 수 있습니다.

사용해야 하는 방법을 이해하려면 다음 섹션의 단계를 완료합니다.

#### <a name="verify-current-user-sign-in-settings"></a>현재 사용자 로그인 설정 확인

1. 글로벌 관리자 계정을 사용하여 [Azure AD 포털](https://aad.portal.azure.com/)에 로그인합니다.
2. **사용자 로그인** 섹션에서 다음 설정을 확인합니다.
   * **페더레이션**이 **사용**으로 설정되어 있습니다.
   * **Seamless Single Sign-On**이 **사용 안 함**으로 설정되어 있습니다.
   * **통과 인증**이**사용 안 함**으로 설정되어 있습니다.

   ![Azure AD Connect 사용자 로그인 섹션의 설정 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image1.png)

#### <a name="verify-how-federation-was-configured"></a>페더레이션 구성 방식 확인

1. Azure AD Connect 서버에서 Azure AD Connect를 엽니다. **구성**을 선택합니다.
2. **추가 작업** 페이지에서 **현재 구성 보기**를 선택하고, **다음**을 선택합니다.<br />
 
   ![추가 작업 페이지에 있는 현재 구성 보기 옵션의 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image2.png)<br />
3. **추가 작업 > 페더레이션 관리**에서 **Active Directory Federation Services (AD FS)** 로 스크롤합니다.<br />

   * 이 섹션에 AD FS 구성이 표시되면 AD FS가 원래 Azure AD Connect를 사용하여 구성되었다고 가정할 수 있습니다. Azure AD Connect **사용자 로그인 변경** 옵션을 사용하여 도메인을 페더레이션 ID에서 관리 ID로 변환할 수 있습니다. 프로세스에 대 한 자세한 내용은 **옵션 A: Azure AD Connect 사용 하 여 통과 인증 구성**섹션을 참조 하세요.
   * AD FS가 현재 설정에 나열되지 않으면 PowerShell을 사용하여 도메인을 페더레이션 ID에서 관리 ID로 수동으로 변환해야 합니다. 이 프로세스에 대 한 자세한 내용은 **옵션 B: Azure AD Connect 및 PowerShell을 사용 하 여 페더레이션에서 통과 인증으로 전환**섹션을 참조 하세요.

### <a name="document-current-federation-settings"></a>현재 페더레이션 설정 문서화

현재 페더레이션 설정을 찾으려면 **Get-MsolDomainFederationSettings** cmdlet을 실행합니다.

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

예:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```

페더레이션 설계 및 배포 설명서에 맞게 사용자 지정되었을 수 있는 설정을 확인합니다. 특히 **PreferredAuthenticationProtocol**, **SupportsMfa** 및 **PromptLoginBehavior**에서 사용자 지정을 찾습니다.

자세한 내용은 다음 문서를 참조하세요.

* [AD FS 프롬프트에서 로그인 매개 변수 지원](/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)
* [Set-MsolDomainAuthentication](/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> **SupportsMfa**가 **True**로 설정되어 있으면 온-프레미스 다단계 인증 솔루션을 사용하여 사용자 인증 흐름에 2단계 챌린지를 삽입합니다. 이 설정은 Azure AD 인증 시나리오에서 더 이상 작동하지 않습니다. 
>
> 대신, Azure Multi-Factor Authentication 클라우드 기반 서비스를 사용하여 동일한 기능을 수행합니다. 계속하기 전에 다단계 인증 요구 사항을 신중하게 평가하세요. 도메인을 변환하기 전에 Azure Multi-Factor Authentication을 사용하는 방법, 라이선스 의미 및 사용자 등록 프로세스를 이해해야 합니다.

#### <a name="back-up-federation-settings"></a>페더레이션 설정 백업

이 문서에서 설명하는 프로세스 중에 AD FS 팜의 다른 신뢰 당사자는 변경되지 않지만 복원할 수 있는 AD FS 팜에 대한 현재 유효한 백업이 있는 것이 좋습니다. Microsoft [AD FS 신속 복원 도구](/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool) 평가판을 사용하여 현재 유효한 백업을 만들 수 있습니다. 이 도구를 사용하여 AD FS를 백업하고 기존 팜을 복원하거나 새 팜을 만들 수 있습니다.

AD FS 빠른 복원 도구를 사용 하지 않도록 선택 하는 경우 최소한 Microsoft 365 Id 플랫폼 신뢰 당사자 트러스트 및 추가한 관련 사용자 지정 클레임 규칙을 내보내야 합니다. 다음 PowerShell 예제를 사용하여 신뢰 당사자 트러스트와 연결된 클레임 규칙을 내보낼 수 있습니다.

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-using-ad-fs"></a>배포 고려 사항 및 AD FS 사용

이 섹션에서는 AD FS 사용과 관련된 배포 고려 사항과 세부 정보에 대해 설명합니다.

### <a name="current-ad-fs-use"></a>현재 AD FS 사용

페더레이션된 id에서 관리 되는 id로 변환 하기 전에 현재 Azure AD, Microsoft 365 및 기타 응용 프로그램 (신뢰 당사자 트러스트)에 대 한 AD FS를 사용 하는 방법을 자세히 살펴보세요. 특히 다음 표에서 설명하는 시나리오를 고려해야 합니다.

| 조건 | 결과 |
|-|-|
| 다른 응용 프로그램 (Azure AD 및 Microsoft 365 이외의 다른 응용 프로그램)과 AD FS를 계속 사용 하도록 계획 합니다. | 도메인이 변환되면 AD FS와 Azure AD를 모두 사용할 수 있습니다. 사용자 환경을 고려합니다. 일부 시나리오에서는 사용자가 Azure AD에 한 번, (예: 사용자가 Microsoft 365 같은 다른 응용 프로그램에 대 한 SSO 액세스를 받는 경우), 신뢰 당사자 트러스트로 AD FS에 계속 바인딩되어 있는 모든 응용 프로그램에 대해 두 번 인증 해야 할 수 있습니다. |
| AD FS 인스턴스는 상당히 많이 사용자 지정되고 onload.js 파일의 특정 사용자 지정 설정에 종속됩니다(예: 사용자가 UPN(사용자 계정 이름) 대신 **SamAccountName** 형식만 사용자 이름에 사용하도록 로그인 환경을 변경한 경우 또는 조직에서 로그인 환경의 브랜드를 많이 지정한 경우). onload.js 파일은 Azure AD에서 중복될 수 없습니다. | 계속하기 전에 Azure AD에서 현재 사용자 지정 요구 사항을 충족할 수 있는지 확인해야 합니다. 자세한 내용과 지침은 AD FS 브랜딩 및 AD FS 사용자 지정 섹션을 참조하세요.|
| AD FS를 사용하여 이전 버전의 인증 클라이언트를 차단합니다.| [조건부 액세스 제어](../conditional-access/concept-conditional-access-conditions.md) 와 [Exchange Online 클라이언트 액세스 규칙](https://aka.ms/EXOCAR)의 조합을 사용 하 여 이전 버전의 인증 클라이언트를 차단 하는 AD FS 컨트롤을 대체 하는 것이 좋습니다. |
| 사용자가 AD FS에 대해 인증할 때 온-프레미스 다단계 인증 서버 솔루션에 대해 다단계 인증을 수행해야 합니다.| 관리 ID 도메인에서는 온-프레미스 다단계 인증 솔루션을 통해 인증 흐름에 다단계 인증 챌린지를 삽입할 수 없습니다. 그러나 도메인이 변환되면 다단계 인증에 Azure Multi-Factor Authentication 서비스를 사용할 수 있습니다.<br /><br /> 사용자가 현재 Azure Multi-Factor Authentication을 사용하지 않는 경우 일회성 사용자 등록 단계를 수행해야 합니다. 계획된 등록을 준비하고 사용자에게 전달해야 합니다. |
| 현재 AD FS에서 액세스 제어 정책 (인증 규칙)을 사용 하 여 Microsoft 365에 대 한 액세스를 제어 합니다.| 정책을 해당 하는 Azure AD [조건부 액세스 정책](../conditional-access/overview.md) 및 [Exchange Online 클라이언트 액세스 규칙](https://aka.ms/EXOCAR)으로 바꾸는 것이 좋습니다.|

### <a name="common-ad-fs-customizations"></a>일반적인 AD FS 사용자 지정

이 섹션에서는 일반적인 AD FS 사용자 지정에 대해 설명합니다.

#### <a name="insidecorporatenetwork-claim"></a>InsideCorporateNetwork 클레임

인증하는 사용자가 회사 네트워크 내부에 있는 경우 AD FS는 **InsideCorporateNetwork** 클레임을 발급합니다. 그러면 이 클레임이 Azure AD에 전달될 수 있습니다. 클레임은 사용자의 네트워크 위치에 따라 다단계 인증을 무시하는 데 사용됩니다. 이 기능이 현재 AD FS에서 사용할 수 있는지 확인하는 방법을 알아보려면 [페더레이션 사용자를 위한 신뢰할 수 있는 IP](../authentication/howto-mfa-adfs.md)를 참조하세요.

도메인이 통과 인증으로 변환되면 **InsideCorporateNetwork** 클레임을 사용할 수 없습니다. 이 기능은 [Azure AD에서 명명된 위치](../reports-monitoring/quickstart-configure-named-locations.md)를 사용하여 대체할 수 있습니다.

명명 된 위치를 구성한 후에는 네트워크를 포함 하거나 제외 하도록 구성 된 모든 조건부 액세스 정책을 업데이트 해야 합니다. 여기에는 새 명명 된 위치를 반영 하는 **모든 신뢰할 수 있는 위치** 또는 **MFA 신뢰할 수 있는 ip** 값이 포함 됩니다.

조건부 액세스의 **위치** 조건에 대 한 자세한 내용은 [Active Directory 조건부 액세스 위치](../conditional-access/location-condition.md)를 참조 하세요.

#### <a name="hybrid-azure-ad-joined-devices"></a>하이브리드 Azure AD 조인 디바이스

장치를 Azure AD에 가입 하는 경우 보안 및 규정 준수에 대 한 액세스 표준을 충족 하는 장치를 적용 하는 조건부 액세스 규칙을 만들 수 있습니다. 또한 사용자가 개인 계정 대신 회사 조직 또는 학교 계정을 사용하여 디바이스에 로그인할 수 있습니다. 하이브리드 Azure AD 조인 디바이스를 사용하는 경우 Active Directory 도메인 조인 디바이스를 Azure AD에 조인할 수 있습니다. 페더레이션 환경에서 이 기능을 사용하도록 설정되었을 수 있습니다.

도메인이 통과 인증으로 변환된 후 도메인에 조인된 모든 디바이스에서 하이브리드 조인이 계속 작동하도록 하려면 Windows 10 클라이언트의 경우 Azure AD Connect를 사용하여 Active Directory 컴퓨터 계정을 Azure AD와 동기화해야 합니다.

Windows 8 및 Windows 7 컴퓨터 계정의 경우 하이브리드 조인은 Seamless SSO를 사용하여 Azure AD에서 컴퓨터를 등록합니다. Windows 10 디바이스처럼 Windows 8 및 Windows 7 컴퓨터 계정을 동기화할 필요는 없습니다. 그러나 업데이트된 workplacejoin.exe 파일(.msi 파일을 통해)을 Windows 8 및 Windows 7 클라이언트에 배포해야 Seamless SSO를 사용하여 직접 등록할 수 있습니다. [.msi 파일을 다운로드하세요](https://www.microsoft.com/download/details.aspx?id=53554).

자세한 내용은 [하이브리드 Azure AD 조인 디바이스 구성](../devices/hybrid-azuread-join-plan.md)을 참조하세요.

#### <a name="branding"></a>브랜딩

조직에 더 적합한 정보를 표시하도록 조직에서 [AD FS 로그인 페이지를 사용자 지정](/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization)한 경우 [Azure AD 로그인 페이지에도 비슷하게 사용자 지정](../fundamentals/customize-branding.md)하는 것이 좋습니다.

비슷한 사용자 지정을 사용할 수 있지만 변환 후에 로그인 페이지 중 일부를 시각적으로 변경할 필요가 있습니다. 사용자와의 통신에 필요한 변경에 대한 정보를 제공할 수 있습니다.

> [!NOTE]
> 조직 브랜드는 Azure Active Directory에 대 한 프리미엄 또는 기본 라이선스를 구매 하거나 Microsoft 365 라이선스가 있는 경우에만 사용할 수 있습니다.

## <a name="plan-for-smart-lockout"></a>스마트 잠금 계획

Azure AD 스마트 잠금은 무차별 암호 대입 공격으로부터 보호합니다. 통과 인증이 사용되고 계정 잠금 그룹 정책이 Active Directory에 설정된 경우 스마트 잠금은 온-프레미스 Active Directory 계정이 잠기지 않도록 방지합니다.

자세한 내용은 [Azure Active Directory 스마트 잠금](../authentication/howto-password-smart-lockout.md)을 참조하세요.

## <a name="plan-deployment-and-support"></a>배포 및 지원 계획

이 섹션에서 설명하는 작업을 완료하면 배포 및 지원을 계획하는 데 도움이 됩니다.

### <a name="plan-the-maintenance-window"></a>유지 관리 기간 계획

도메인 변환 프로세스가 비교적 빠르지만, Azure AD는 도메인 변환이 완료된 후 최대 4시간 동안 AD FS 서버에 일부 인증 요청을 계속 보낼 수 있습니다. 이 4시간 동안 다양한 서비스 쪽 캐시에 따라 Azure AD에서 이러한 인증을 수락하지 않을 수 있습니다. 사용자가 오류 메시지를 받을 수 있습니다. 사용자가 여전히 AD FS에 대해 성공적으로 인증할 수 있지만, 이제 해당 페더레이션 트러스트가 제거되었으므로 Azure AD에서 사용자가 발급한 토큰을 더 이상 수락하지 않습니다.

이 변환 후 기간 동안 서비스 쪽 캐시를 지우기 전에 웹 브라우저를 통해 서비스에 액세스하는 사용자만 영향을 받습니다. Exchange Online은 일정 기간 동안 자격 증명의 캐시를 유지하므로 레거시 클라이언트(Exchange ActiveSync, Outlook 2010/2013)는 영향을 받지 않습니다. 캐시는 사용자를 자동으로 다시 인증하는 데 사용됩니다. 사용자는 AD FS로 돌아갈 필요가 없습니다. 이 캐시된 자격 증명이 지워지면 이러한 클라이언트에 대해 디바이스에 저장된 자격 증명이 자동으로 다시 인증하는 데 사용됩니다. 도메인 변환 프로세스의 결과로 사용자에게 암호 프롬프트가 표시되지 않습니다.

최신 인증 클라이언트(Office 2016 및 Office 2013, iOS 및 Android 앱)는 유효한 새로 고침 토큰을 사용하여 AD FS로 돌아가는 대신 리소스에 계속 액세스할 수 있는 새 액세스 토큰을 얻습니다. 이러한 클라이언트는 도메인 변환 프로세스에서 발생하는 모든 암호 프롬프트에 영향을 받지 않습니다. 클라이언트는 추가 구성 없이 계속 작동합니다.

> [!IMPORTANT]
> 모든 사용자가 클라우드 인증을 사용 하 여 성공적으로 인증할 수 있는지 확인할 때까지 AD FS 환경을 종료 하거나 Microsoft 365 신뢰 당사자 트러스트를 제거 하지 마세요.

### <a name="plan-for-rollback"></a>롤백 계획

신속하게 해결할 수 없는 주요 문제가 발생하면 솔루션을 페더레이션으로 롤백하도록 결정할 수 있습니다. 배포가 계획대로 롤아웃되지 않을 경우 수행할 작업을 계획해야 합니다. 배포 중에 도메인 또는 사용자의 변환이 실패하거나 페더레이션으로 롤백해야 하는 경우, 중단을 완화하고 사용자에게 미치는 영향을 줄이는 방법을 이해해야 합니다.

#### <a name="to-roll-back"></a>롤백

롤백을 계획하려면 특정 배포 세부 정보에 대한 페더레이션 설계 및 배포 설명서를 확인합니다. 프로세스에 포함되는 작업은 다음과 같습니다.

* **Convert-MSOLDomainToFederated** cmdlet을 사용하여 관리되는 도메인을 페더레이션된 도메인으로 변환합니다.
* 필요한 경우 추가 클레임 규칙을 구성합니다.

### <a name="plan-communications"></a>통신 계획

배포 및 지원을 계획하는 데 있어 중요한 부분은 사용자가 향후 변경에 대해 사전에 알 수 있도록 하는 것입니다. 사용자는 자신이 경험할 수 있는 것과 필요한 것을 미리 알고 있어야 합니다.

통과 인증과 원활한 SSO가 배포 된 후에는 Azure AD 변경을 통해 인증 된 Microsoft 365 및 기타 리소스에 액세스 하기 위한 사용자 로그인 환경이 제공 됩니다. 네트워크 외부에 있는 사용자는 Azure AD 로그인 페이지만 볼 수 있습니다. 이러한 사용자는 외부 웹 애플리케이션 프록시 서버에서 제공하는 양식 기반 페이지로 리디렉션되지 않습니다.

통신 전략에 포함되는 요소는 다음과 같습니다.

* 다음 항목을 사용하여 사용자에게 예정된 기능과 릴리스된 기능을 알립니다.
   * 이메일 및 기타 내부 통신 채널
   * 시각적 개체(예: 포스터)
   * 경영진 라이브 또는 기타 통신 방식
* 통신을 사용자 지정하는 사람, 보내는 사람 및 시기를 결정합니다.

## <a name="implement-your-solution"></a>솔루션 구현

솔루션을 계획했습니다. 이제 이 솔루션을 구현할 수 있습니다. 구현에 포함되는 구성 요소는 다음과 같습니다.

* Seamless SSO 준비
* 로그인 방법을 통과 인증으로 변경 및 Seamless SSO 사용

### <a name="step-1-prepare-for-seamless-sso"></a>1 단계: 원활한 SSO 준비

디바이스에서 Seamless SSO를 사용하려면 Active Directory의 그룹 정책을 사용하여 사용자의 인트라넷 영역 설정에 하나의 Azure AD URL을 추가해야 합니다.

기본적으로 웹 브라우저는 URL에서 올바른 영역(인터넷 또는 인트라넷)을 자동으로 계산합니다. 예를 들어 **http: \/ \/ contoso/** 는 인트라넷 영역에 매핑되고 **http: \/ \/ INTRANET.CONTOSO.COM** 은 인터넷 영역에 매핑됩니다 (URL에 마침표가 포함 되어 있기 때문). URL을 브라우저의 인트라넷 영역에 명시적으로 추가하는 경우에만 브라우저에서 Kerberos 티켓을 클라우드 엔드포인트(예: Azure AD URL)에 보냅니다.

필요한 변경을 디바이스에 [롤아웃](./how-to-connect-sso-quick-start.md)하는 단계를 수행합니다.

> [!IMPORTANT]
> 이렇게 변경하더라도 사용자가 Azure AD에 로그인하는 방법이 수정되지 않습니다. 그러나 계속 진행하기 전에 이 구성을 모든 디바이스에 적용해야 합니다. 또한 이 구성을 받지 않은 디바이스에 로그인하는 사용자는 Azure AD에 로그인하기 위해 사용자 이름과 암호만 입력하면 됩니다.

### <a name="step-2-change-the-sign-in-method-to-pass-through-authentication-and-enable-seamless-sso"></a>2 단계: 로그인 방법을 통과 인증으로 변경 하 고 원활한 SSO를 사용 하도록 설정

로그인 방법을 통과 인증으로 변경하고 Seamless SSO를 사용하도록 설정하는 두 가지 옵션이 있습니다.

#### <a name="option-a-configure-pass-through-authentication-by-using-azure-ad-connect"></a>옵션 A: Azure AD Connect을 사용 하 여 통과 인증 구성

원래 Azure AD Connect를 사용하여 AD FS 환경을 구성한 경우 이 방법을 사용합니다. 원래 Azure AD Connect를 사용하여 AD FS 환경을 구성하지 *않은* 경우에는 이 방법을 사용할 수 없습니다.

> [!IMPORTANT]
> 다음 단계가 완료되면 모든 도메인이 페더레이션 ID에서 관리 ID로 변환됩니다. 자세한 내용은 [마이그레이션 방법 계획](#plan-the-migration-method)을 검토하세요.

먼저 로그인 방법을 변경합니다.

1. Azure AD Connect 서버에서 Azure AD Connect 마법사를 엽니다.
2. **사용자 로그인 변경**을 선택 하 고 **다음**을 선택 합니다. 
3. **Azure AD에 연결** 페이지에서 글로벌 관리자 계정의 사용자 이름과 암호를 입력합니다.
4. **사용자 로그인** 페이지에서 **통과 인증** 단추를 선택하고, **Single Sign-On 사용**을 선택하고, **다음**을 선택합니다.
5. **Single Sign-On 사용** 페이지에서 도메인 관리자 계정의 자격 증명을 입력하고, **다음**을 선택합니다.

   > [!NOTE]
   > Seamless SSO를 사용하도록 설정하려면 도메인 관리자 계정 자격 증명이 필요합니다. 이 프로세스에서 다음 작업을 수행하는 데 이러한 관리자 권한이 필요합니다. 도메인 관리자 계정 자격 증명은 Azure AD Connect 또는 Azure AD에 저장되지 않습니다. 도메인 관리자 계정 자격 증명은 기능을 설정하는 데만 사용됩니다. 프로세스가 성공적으로 완료되면 자격 증명이 삭제됩니다.
   >
   > 1. Azure AD를 나타내는 AZUREADSSOACC라는 컴퓨터 계정을 온-프레미스 Active Directory 인스턴스에 만듭니다.
   > 2. 컴퓨터 계정의 Kerberos 암호 해독 키를 Azure AD와 안전하게 공유합니다.
   > 3. Azure AD 로그인 중에 사용되는 두 개의 URL을 나타내기 위해 두 개의 Kerberos SPN(서비스 사용자 이름)을 만듭니다.

6. **구성 준비 완료** 페이지에서 **구성이 완료되면 동기화 프로세스 시작** 확인란이 선택되어 있는지 확인합니다. 그런 다음 **구성**을 선택 합니다.<br />

   ![구성 준비 완료 페이지의 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image8.png)<br />
7. Azure AD 포털에서 **Azure Active Directory**를 선택한 다음, **Azure AD Connect**를 선택합니다.
8. 다음 설정을 확인합니다.
   * **페더레이션**이 **사용 안 함**으로 설정되어 있습니다.
   * **Seamless Single Sign-On**이 **사용**으로 설정되어 있습니다.
   * **통과 인증**이**사용**으로 설정되어 있습니다.<br />

   ![사용자 로그인 섹션의 설정을 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image9.png)<br />

다음으로, 추가 인증 메서드를 배포합니다.

1. Azure Portal에서 **Azure Active Directory**  >  **Azure AD Connect**으로 이동한 후 **통과 인증**을 선택 합니다.
2. **통과 인증** 페이지에서 **다운로드** 단추를 선택합니다.
3. **에이전트 다운로드** 페이지에서 **약관 동의 및 다운로드**를 선택합니다.

   추가 인증 에이전트가 다운로드되기 시작합니다. 도메인에 조인된 서버에 보조 인증 에이전트를 설치합니다. 

   > [!NOTE]
   > 첫 번째 에이전트는 항상 Azure AD Connect 도구의 **사용자 로그인** 섹션에서 수행된 구성 변경의 일부로 Azure AD Connect 서버 자체에 설치됩니다. 추가 인증 에이전트는 모두 별도의 서버에 설치합니다. 추가로 2~3 개의 인증 에이전트를 사용하는 것이 좋습니다. 

4. 인증 에이전트 설치를 실행합니다. 설치 중에 글로벌 관리자 계정의 자격 증명을 입력해야 합니다.

   ![Microsoft Azure AD Connect 인증 에이전트 패키지 페이지의 설치 단추를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image11.png)

   ![로그인 페이지를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image12.png)

5. 인증 에이전트가 설치되면 통과 인증 에이전트 상태 페이지로 돌아가서 추가 에이전트의 상태를 확인할 수 있습니다.

[테스트 및 다음 단계](#testing-and-next-steps)로 건너뜁니다.

> [!IMPORTANT]
> **Azure AD Connect 및 PowerShell을 사용 하 여 옵션 B: 페더레이션에서 통과 인증으로 전환**섹션을 건너뜁니다. 옵션 A를 선택하여 로그인 방법을 통과 인증으로 변경하고 Seamless SSO를 사용하도록 설정한 경우에는 이 섹션의 단계가 적용되지 않습니다. 

#### <a name="option-b-switch-from-federation-to-pass-through-authentication-by-using-azure-ad-connect-and-powershell"></a>옵션 B: Azure AD Connect 및 PowerShell을 사용 하 여 페더레이션에서 통과 인증으로 전환

원래 Azure AD Connect를 사용하여 페더레이션된 도메인을 구성하지 않은 경우 이 옵션을 사용합니다.

먼저 통과 인증을 사용하도록 설정합니다.

1. Azure AD Connect 서버에서 Azure AD Connect 마법사를 엽니다.
2. **사용자 로그인 변경**을 선택 하 고 **다음**을 선택 합니다.
3. **Azure AD에 연결** 페이지에서 글로벌 관리자 계정의 사용자 이름과 암호를 입력합니다.
4. **사용자 로그인** 페이지에서 **통과 인증** 단추를 선택합니다. **Single Sign-On 인증 사용**을 선택하고, **다음**을 선택합니다.
5. **Single Sign-On 사용** 페이지에서 도메인 관리자 계정의 자격 증명을 입력하고, **다음**을 선택합니다.

   > [!NOTE]
   > Seamless SSO를 사용하도록 설정하려면 도메인 관리자 계정 자격 증명이 필요합니다. 이 프로세스에서 다음 작업을 수행하는 데 이러한 관리자 권한이 필요합니다. 도메인 관리자 계정 자격 증명은 Azure AD Connect 또는 Azure AD에 저장되지 않습니다. 도메인 관리자 계정 자격 증명은 기능을 설정하는 데만 사용됩니다. 프로세스가 성공적으로 완료되면 자격 증명이 삭제됩니다.
   > 
   > 1. Azure AD를 나타내는 AZUREADSSOACC라는 컴퓨터 계정을 온-프레미스 Active Directory 인스턴스에 만듭니다.
   > 2. 컴퓨터 계정의 Kerberos 암호 해독 키를 Azure AD와 안전하게 공유합니다.
   > 3. Azure AD 로그인 중에 사용되는 두 개의 URL을 나타내기 위해 두 개의 Kerberos SPN(서비스 사용자 이름)을 만듭니다.

6. **구성 준비 완료** 페이지에서 **구성이 완료되면 동기화 프로세스 시작** 확인란이 선택되어 있는지 확인합니다. 그런 다음 **구성**을 선택 합니다.<br />

   ‎![구성 준비 완료 페이지와 구성 단추를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image18.png)<br />
   **구성**을 선택하면 다음 단계가 수행됩니다.

   1. 첫 번째 통과 인증 에이전트가 설치됩니다.
   2. 통과 기능을 사용하도록 설정됩니다.
   3. Seamless SSO를 사용하도록 설정됩니다.

7. 다음 설정을 확인합니다.
   * **페더레이션**이 **사용**으로 설정되어 있습니다.
   * **Seamless Single Sign-On**이 **사용**으로 설정되어 있습니다.
   * **통과 인증**이**사용**으로 설정되어 있습니다.
   
   ![사용자 로그인 섹션의 설정을 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image19.png)
8. **통과 인증** 을 선택 하 고 상태가 **활성**인지 확인 합니다.<br />
   
   인증 에이전트가 활성 상태가 아닌 경우 다음 단계에서 도메인 변환 프로세스를 계속 진행하기 전에 몇 가지 [문제 해결 단계](./tshoot-connect-pass-through-authentication.md)를 수행합니다. 통과 인증 에이전트가 성공적으로 설치되었는지와 Azure Portal에서 해당 상태가 **활성**인지를 확인하기 전에 도메인을 변환하면 인증이 중단될 위험이 있습니다.

다음으로, 추가 인증 에이전트를 배포합니다.

1. Azure Portal에서 **Azure Active Directory**  >  **Azure AD Connect**으로 이동한 후 **통과 인증**을 선택 합니다.
2. **통과 인증** 페이지에서 **다운로드** 단추를 선택합니다. 
3. **에이전트 다운로드** 페이지에서 **약관 동의 및 다운로드**를 선택합니다.
 
   인증 에이전트가 다운로드되기 시작합니다. 도메인에 조인된 서버에 보조 인증 에이전트를 설치합니다.

   > [!NOTE]
   > 첫 번째 에이전트는 항상 Azure AD Connect 도구의 **사용자 로그인** 섹션에서 수행된 구성 변경의 일부로 Azure AD Connect 서버 자체에 설치됩니다. 추가 인증 에이전트는 모두 별도의 서버에 설치합니다. 추가로 2~3 개의 인증 에이전트를 사용하는 것이 좋습니다.
 
4. 인증 에이전트 설치를 실행합니다. 설치 중에 글로벌 관리자 계정의 자격 증명을 입력해야 합니다.<br />

   ![Microsoft Azure AD Connect 인증 에이전트 패키지 페이지의 설치 단추를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image23.png)<br />
   ![로그인 페이지를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image24.png)<br />
5. 인증 에이전트가 설치되면 통과 인증 에이전트 상태 페이지로 돌아가서 추가 에이전트의 상태를 확인할 수 있습니다.

이 시점에서 도메인에 대한 페더레이션이 여전히 활성 상태이고 작동하고 있습니다. 배포를 계속하려면 각 도메인을 페더레이션 ID에서 관리 ID로 변환하여 통과 인증에서 도메인에 대한 인증 요청의 처리를 시작해야 합니다.

모든 도메인을 동시에 변환할 필요는 없습니다. 프로덕션 테넌트에서 테스트 도메인으로 시작하거나 사용자 수가 가장 적은 도메인으로 시작하도록 선택할 수 있습니다.

Azure AD PowerShell 모듈을 사용하여 변환을 수행합니다.

1. PowerShell에서 글로벌 관리자 계정을 사용하여 Azure AD에 로그인합니다.
2. 첫 번째 도메인을 변환하려면 다음 명령을 실행합니다.
 
   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domain name>
   ```
 
3. Azure AD 포털에서 **Azure Active Directory**  >  **Azure AD Connect**를 선택 합니다.
4. 페더레이션된 모든 도메인이 변환되면 다음 설정을 확인합니다.
   * **페더레이션**이 **사용 안 함**으로 설정되어 있습니다.
   * **Seamless Single Sign-On**이 **사용**으로 설정되어 있습니다.
   * **통과 인증**이**사용**으로 설정되어 있습니다.<br />

   ![사용자 로그인 섹션의 설정을 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image26.png)<br />

## <a name="testing-and-next-steps"></a>테스트 및 다음 단계

다음 작업을 수행하여 통과 인증을 확인하고 변환 프로세스를 완료합니다.

### <a name="test-pass-through-authentication"></a>통과 인증 테스트 

테넌트에서 페더레이션 ID를 사용한 경우 사용자가 Azure AD 로그인 페이지에서 AD FS 환경으로 리디렉션되었습니다. 이제 테넌트에서 페더레이션 인증 대신 통과 인증을 사용하도록 구성되었으므로 사용자가 AD FS로 리디렉션되지 않습니다. 대신, 사용자는 Azure AD 로그인 페이지에서 직접 로그인합니다.

통과 인증을 테스트하려면,

1. Seamless SSO가 자동으로 로그인하지 않도록 InPrivate 모드에서 Internet Explorer를 엽니다.
2. Office 365 로그인 페이지 ()로 이동 [https://portal.office.com](https://portal.office.com/) 합니다.
3. 사용자 UPN을 입력하고, **다음**을 선택합니다. 온-프레미스 Active Directory 인스턴스에서 동기화되고 이전에 페더레이션 인증을 사용한 하이브리드 사용자의 UPN을 입력해야 합니다. 사용자 이름과 암호를 입력하는 페이지가 표시됩니다.

   ![사용자 이름을 입력하는 로그인 페이지를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image27.png)

   ![암호를 입력하는 로그인 페이지를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image28.png)

4. 암호를 입력하고 **로그인**을 선택하면 Office 365 포털로 리디렉션됩니다.

   ![Office 365 포털을 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image29.png)

### <a name="test-seamless-sso"></a>Seamless SSO 테스트

Seamless SSO를 테스트하려면,

1. 회사 네트워크에 연결된 도메인 조인 머신에 로그인합니다.
2. Internet Explorer 또는 Chrome에서 다음 URL 중 하나로 이동합니다("contoso"를 사용자의 도메인으로 바꿈).

   * https:\/\/myapps.microsoft.com/contoso.com
   * https:\/\/myapps.microsoft.com/contoso.onmicrosoft.com

   사용자가 "로그인 시도 중"이라는 메시지를 보여 주는 Azure AD 로그인 페이지로 간단히 리디렉션됩니다. 사용자에게 사용자 이름 또는 암호를 입력하라는 메시지가 표시되지 않습니다.<br />

   ![Azure AD 로그인 페이지 및 메시지를 보여 주는 스크린샷](media/plan-migrate-adfs-pass-through-authentication/migrating-adfs-to-pta_image30.png)<br />
3. 사용자가 리디렉션되고 액세스 패널에 성공적으로 로그인됩니다.

   > [!NOTE]
   > 원활한 SSO는 도메인 힌트 (예: myapps.microsoft.com/contoso.com)를 지 원하는 Microsoft 365 서비스에서 작동 합니다. 현재 Microsoft 365 포털 (portal.office.com)은 도메인 힌트를 지원 하지 않습니다. 사용자가 UPN을 입력해야 합니다. UPN이 입력되면 Seamless SSO에서 사용자를 대신하여 Kerberos 티켓을 검색합니다. 사용자가 암호를 입력하지 않고 로그인됩니다.

   > [!TIP]
   > 향상된 SSO 환경을 위해 [Windows 10에서 Azure AD 하이브리드 조인](../devices/overview.md)을 배포하는 것이 좋습니다.

### <a name="remove-the-relying-party-trust"></a>신뢰 당사자 트러스트 제거

모든 사용자 및 클라이언트가 Azure AD를 통해 성공적으로 인증 되는지 확인 한 후에는 Microsoft 365 신뢰 당사자 트러스트를 제거 하는 것이 안전 합니다.

AD FS를 다른 용도(즉, 다른 신뢰 당사자 트러스트)로 사용하지 않는 경우 이 시점에서 AD FS 서비스를 해제하는 것이 안전합니다.

### <a name="rollback"></a>롤백

주요 문제를 검색했지만 빠르게 해결할 수 없는 경우 솔루션을 페더레이션으로 롤백하도록 선택할 수 있습니다.

특정 배포 세부 정보에 대해서는 페더레이션 설계 및 배포 설명서를 참조하세요. 프로세스에 포함되는 작업은 다음과 같습니다.

* **Convert-MSOLDomainToFederated** cmdlet을 사용하여 관리되는 도메인을 페더레이션 인증으로 변환합니다.
* 필요한 경우 추가 클레임 규칙을 구성합니다.

### <a name="sync-userprincipalname-updates"></a>userPrincipalName 업데이트 동기화

지금까지 다음 조건이 모두 충족되지 않으면 온-프레미스 환경에서 동기화 서비스를 사용하는 **UserPrincipalName** 특성에 대한 업데이트가 차단되었습니다.

* 사용자가 관리(페더레이션되지 않음) ID 도메인에 있습니다.
* 사용자에게 라이선스가 할당되지 않았습니다.

이 기능을 확인하거나 설정하는 방법을 알아보려면 [userPrincipalName 업데이트 동기화](./how-to-connect-syncservice-features.md)를 참조하세요.

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Seamless SSO Kerberos 암호 해독 키 롤오버

AZUREADSSOACC 컴퓨터 계정(Azure AD를 나타냄)의 Kerberos 암호 해독 키를 자주 롤오버해야 합니다. AZUREADSSOACC 컴퓨터 계정은 온-프레미스 Active Directory 포리스트에 만들어집니다. Active Directory 도메인 멤버가 암호 변경을 제출하는 방법에 따라 Kerberos 암호 해독 키를 적어도 30일마다 롤오버하는 것이 좋습니다. 연결된 디바이스가 AZUREADSSOACC 컴퓨터 계정 개체에 연결되어 있지 않으므로 롤오버를 수동으로 수행해야 합니다.

Azure AD Connect를 실행하는 온-프레미스 서버에서 Seamless SSO Kerberos 암호 해독 키의 롤오버를 시작합니다.

자세한 내용은 [AZUREADSSOACC 컴퓨터 계정의 Kerberos 암호 해독 키를 롤오버하려면 어떻게 하나요?](./how-to-connect-sso-faq.md)를 참조하세요.

## <a name="monitoring-and-logging"></a>모니터링 및 로깅

인증 에이전트를 실행하는 서버를 모니터링하여 솔루션 가용성을 유지합니다. 인증 에이전트는 일반 서버 성능 카운터 외에도 인증 통계 및 오류를 이해하는 데 도움이 되는 성능 개체를 공개합니다.

인증 에이전트는 Application and Service Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin 아래에 있는 Windows 이벤트 로그에 작업을 기록합니다.

또한 문제 해결을 위해 로깅을 설정할 수도 있습니다.

자세한 내용은[Azure Active Directory 통과 인증 문제 해결](./tshoot-connect-pass-through-authentication.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

* [Azure AD Connect 설계 개념](plan-connect-design-concepts.md)에 대해 알아봅니다.
* 적절 한 [인증](./choose-ad-authn.md)을 선택 합니다.
* [지원되는 토폴로지](plan-connect-design-concepts.md)에 대해 알아봅니다.