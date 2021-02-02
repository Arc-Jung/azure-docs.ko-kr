---
title: 테넌트 제한을 사용하여 SaaS 앱에 대한 액세스 관리 - Azure AD
description: 테넌트 제한을 사용하여 Azure AD 테넌트를 기준으로 앱에 액세스할 수 있는 사용자를 관리하는 방법입니다.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/26/2020
ms.author: kenwith
ms.reviewer: hpsin
ms.collection: M365-identity-device-management
ms.openlocfilehash: f605b2bb48855d70ea305dcda194b26da71ee9ec
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99252477"
---
# <a name="use-tenant-restrictions-to-manage-access-to-saas-cloud-applications"></a>테넌트 제한을 사용하여 SaaS 클라우드 애플리케이션에 대한 액세스 관리

보안을 강조 하는 대기업은 Microsoft 365와 같은 클라우드 서비스로 이동 하려고 하지만, 사용자가 승인 된 리소스에만 액세스할 수 있다는 것을 알고 있어야 합니다. 일반적으로 회사에서는 액세스를 관리하려는 경우에 도메인 이름 또는 IP 주소를 제한합니다. 이 방법은 SaaS(Software as a Service) 앱이 퍼블릭 클라우드에서 호스트되고 [outlook.office.com](https://outlook.office.com/) 및 [login.microsoftonline.com](https://login.microsoftonline.com/)과 같은 공유 도메인 이름으로 실행되는 환경에서는 실패합니다. 이러한 주소를 차단하면 사용자가 단순히 승인된 ID 및 리소스로만 제한되는 것이 아니라 웹상의 Outlook에 완전히 액세스할 수 없게 됩니다.

이러한 경우에 대한 Azure AD(Azure Active Directory) 해결 방법은 테넌트 제한이라는 기능입니다. 테넌트 제한을 사용하면 조직은 애플리케이션이 Single Sign-On에 대해 사용하는 사용할 Azure AD 테넌트에 따라, SaaS 클라우드 애플리케이션에 대한 액세스를 제어할 수 있습니다. 예를 들어 조직의 Microsoft 365 응용 프로그램에 대 한 액세스를 허용 하는 동시에 이러한 동일한 응용 프로그램의 다른 조직 인스턴스에 대 한 액세스를 방지할 수 있습니다.  

테넌트 제한을 사용하면 조직은 사용자에게 액세스가 허용된 테넌트 목록을 지정할 수 있습니다. 그러면 Azure AD는 이러한 허용된 테넌트에 대해서만 액세스를 허가합니다.

이 문서에서는 Microsoft 365에 대 한 테 넌 트 제한 사항에 중점을 둔 반면,이 기능은 Azure AD에서 Single Sign-On에 최신 인증 프로토콜을 사용 하는 모든 SaaS 클라우드 앱에서 작동 해야 합니다. Microsoft 365에서 사용 하는 테 넌 트의 다른 Azure AD 테 넌 트에서 SaaS 앱을 사용 하는 경우 필요한 모든 테 넌 트가 허용 되는지 확인 합니다. 클라우드 SaaS 앱에 대한 자세한 내용은 [Active Directory Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActiveDirectory)를 참조하세요.

## <a name="how-it-works"></a>작동 방법

전체 솔루션은 다음 구성 요소로 구성됩니다.

1. **AZURE ad**: `Restrict-Access-To-Tenants: <permitted tenant list>` 헤더가 있는 경우 azure ad는 허용 되는 테 넌 트에 대해서만 보안 토큰을 발급 합니다.

2. **온-프레미스 프록시 서버 인프라**: 이 인프라는 TLS(전송 계층 보안) 검사를 수행할 수 있는 프록시 디바이스입니다. 허용되는 테넌트 목록을 포함하는 헤더를 Azure AD로 향하는 트래픽에 삽입하도록 프록시를 구성해야 합니다.

3. **클라이언트 소프트웨어**: 테넌트 제한을 지원하기 위해 클라이언트 소프트웨어는 Azure AD에서 직접 토큰을 요청해야 하므로 프록시 인프라에서 트래픽을 가로챌 수 있습니다. 현재 브라우저 기반 Microsoft 365 응용 프로그램은 최신 인증을 사용 하는 Office 클라이언트 (예: OAuth 2.0)와 같이 테 넌 트 제한을 지원 합니다.

4. **최신 인증**: 클라우드 서비스는 테넌트 제한을 사용하고 허용되지 않는 모든 테넌트로의 액세스를 차단하기 위해 최신 인증을 사용해야 합니다. 기본적으로 최신 인증 프로토콜을 사용 하도록 Microsoft 365 cloud services를 구성 해야 합니다. 최신 인증에 대 한 Microsoft 365 지원에 대 한 최신 정보는 [업데이트 된 Office 365 최신 인증](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)을 참조 하세요.

다음 다이어그램은 고급 트래픽 흐름을 보여 줍니다. 테 넌 트 제한에는 Microsoft 365 cloud services가 아닌 Azure AD에 대 한 트래픽에만 TLS 검사가 필요 합니다. Azure AD에서 인증을 받으려는 트래픽 볼륨이 일반적으로 Exchange Online 및 SharePoint Online과 같은 SaaS 애플리케이션에 대한 트래픽 볼륨보다 훨씬 작기 때문에 이러한 구분이 필요합니다.

![테넌트 제한 트래픽 흐름 - 다이어그램](./media/tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>테넌트 제한 설정

테넌트 제한을 시작하는 방법은 두 단계로 구성됩니다. 첫째, 클라이언트가 올바른 주소에 연결할 수 있도록 합니다. 둘째, 프록시 인프라를 구성합니다.

### <a name="urls-and-ip-addresses"></a>URL 및 IP 주소

테넌트 제한을 사용하려면 클라이언트는 인증을 받기 위해 Azure AD URL [login.microsoftonline.com](https://login.microsoftonline.com/), [login.microsoft.com](https://login.microsoft.com/) 및 [login.windows.net](https://login.windows.net/)에 연결할 수 있어야 합니다. 또한 Office 365에 액세스하려면 클라이언트는 [Office 365 URL 및 IP 주소 범위](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)에 정의된 FQDN(정규화된 도메인 이름)/URL에 연결할 수 있어야 합니다. 

### <a name="proxy-configuration-and-requirements"></a>프록시 구성 및 요구 사항

프록시 인프라를 통해 테넌트 제한을 적용하려면 다음과 같은 구성이 필요합니다. 이 지침은 일반적인 것이므로 보다 구체적인 구현 단계는 프록시 공급업체의 설명서를 참조차세요.

#### <a name="prerequisites"></a>사전 요구 사항

- 프록시는 FQDN/URL을 사용하여 TLS 가로채기, HTTP 헤더 삽입 및 대상 필터링을 수행할 수 있어야 합니다.

- 클라이언트는 TLS 통신을 위해 프록시에서 제공한 인증서 체인을 신뢰해야 합니다. 예를 들어 내부 [PKI(퍼블릭 키 인프라)](/windows/desktop/seccertenroll/public-key-infrastructure)에서 발급한 인증서가 사용될 경우 내부 발급 루트 인증 기관 인증서를 신뢰할 수 있어야 합니다.

- Azure AD Premium 1 라이선스는 테 넌 트 제한을 사용 하는 데 필요 합니다. 

#### <a name="configuration"></a>구성

login.microsoftonline.com, login.microsoft.com 및 login.windows.net으로 들어오는 각 요청에 대해 두 개의 HTTP 헤더인 *Restrict-Access-To-Tenants* 및 *Restrict-Access-Context* 를 삽입합니다.

> [!NOTE]
> SSL 가로채기 및 헤더 주입을 구성할 때에 대 한 트래픽이 https://device.login.microsoftonline.com 제외 되는지 확인 합니다. 이 URL은 장치 인증에 사용 되며 TLS 중단 및 검사는 클라이언트 인증서 인증을 방해할 수 있으며,이로 인해 장치 등록 및 장치 기반 조건부 액세스와 관련 된 문제가 발생할 수 있습니다.



헤더에는 다음 요소가 포함되어야 합니다.

- *-테 넌 트에 대 한 액세스 제한* 의 경우 \<permitted tenant list\> 사용자에 게 액세스를 허용 하려는 테 넌 트의 쉼표로 구분 된 목록 값을 사용 합니다. 테 넌 트에 등록 된 모든 도메인은이 목록에서 테 넌 트를 식별 하는 데 사용할 수 있으며 디렉터리 ID 자체에도 사용할 수 있습니다. 테 넌 트를 설명 하는 세 가지 방법에 대 한 예제를 보려면 Contoso, Fabrikam 및 Microsoft를 허용 하는 이름/값 쌍은 다음과 같습니다. `Restrict-Access-To-Tenants: contoso.com,fabrikam.onmicrosoft.com,72f988bf-86f1-41af-91ab-2d7cd011db47`

- *Restrict-Access-Context* 의 경우 테넌트 제한을 설정하는 테넌트를 선언하는 단일 디렉터리 ID 값을 사용합니다. 예를 들어 테 넌 트 제한 정책을 설정 하는 테 넌 트로 Contoso를 선언 하는 경우 이름/값 쌍은와 `Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d` 같습니다.  이 지점에서는 고유한 디렉터리 ID를 사용 **해야** 합니다.

> [!TIP]
> [Azure Active Directory 포털](https://aad.portal.azure.com/)에서 디렉터리 ID를 찾을 수 있습니다. 관리자로 로그인한 후 **Azure Active Directory** 를 선택하고 **속성** 을 선택합니다. 
>
> 디렉터리 ID 또는 도메인 이름이 동일한 테 넌 트를 참조 하는지 확인 하려면이 URL에서 대신 해당 ID 또는 도메인을 사용 <tenant> `https://login.microsoftonline.com/<tenant>/v2.0/.well-known/openid-configuration` 합니다.  도메인 및 ID를 포함 하는 결과는 동일한 테 넌 트를 참조 합니다. 

사용자가 승인되지 않은 테넌트에 와 자체 HTTP 헤더를 삽입하지 못하도록 하기 위해 프록시는 *Restrict-Access-To-Tenants* 헤더가 들어오는 요청에 이미 있는 경우 바꾸어야 합니다.

클라이언트는 Login.microsoftonline.com, login.microsoft.com 및 login.windows.net에 대한 모든 요청에 강제로 해당 프록시를 사용하도록 지정되어야 합니다. 예를 들어 PAC 파일을 사용하여 클라이언트가 프록시를 사용하도록 하면 최종 사용자는 PAC 파일을 편집하거나 사용하지 않도록 설정할 수 없습니다.

> [!NOTE]
> 프록시 구성에서 *. login.microsoftonline.com에 하위 도메인을 포함 하지 마십시오. 이렇게 하면 device.login.microsoftonline.com 포함 되며 장치 등록 및 장치 기반 조건부 액세스 시나리오에서 사용 되는 클라이언트 인증서 인증을 방해할 수 있습니다. TLS 중단 및 검사 및 헤더 주입에서 device.login.microsoftonline.com을 제외 하도록 프록시 서버를 구성 합니다.

## <a name="the-user-experience"></a>사용자 환경

이 섹션에서는 최종 사용자와 관리자를 위한 환경을 설명합니다.

### <a name="end-user-experience"></a>최종 사용자 환경

예제 사용자는 Contoso 네트워크에 있지만 Outlook 온라인과 같은 공유 SaaS 애플리케이션의 Fabrikam 인스턴스에 액세스하려고 합니다. Fabrikam이 Contoso 인스턴스에 대해 허용되지 않는 테넌트인 경우 사용자에게 IT 부서에서 승인하지 않은 조직에 속한 리소스에 액세스하려고 한다는 액세스 거부 메시지가 표시됩니다.

### <a name="admin-experience"></a>관리자 환경

기업 프록시 인프라에서 테넌트 제한 구성이 완료되면 관리자는 Azure Portal의 테넌트 제한 보고서에 직접 액세스할 수 있습니다. 보고서를 보려면

1. [Azure Active Directory 포털](https://aad.portal.azure.com/)에 로그인합니다. **Azure Active Directory 관리 센터** 대시보드가 표시됩니다.

2. 왼쪽 창에서 **Azure Active Directory** 를 선택합니다. Azure Active Directory 개요 페이지가 나타납니다.

3. 개요 페이지에서 **테 넌 트 제한** 을 선택 합니다.

Restricted-Access-Context 테넌트로 지정된 테넌트의 관리자는 이 보고서를 사용하여 사용된 ID 및 대상 디렉터리 ID를 비롯하여 테넌트 제한 정책으로 인해 차단된 로그인을 확인할 수 있습니다. 이러한 제한을 설정하는 테넌트가 로그인에 대해 사용자 테넌트이거나 리소스 테넌트인 경우 로그인이 포함됩니다.

> [!NOTE]
> Restricted-Access-Context 테넌트 이외의 테넌트에 있는 사용자가 로그인하는 경우 보고서에는 대상 디렉터리 ID와 같은 제한된 정보가 포함될 수 있습니다. 이 경우 다른 테 넌 트의 사용자 데이터 ("")를 보호 하기 위해 이름, 사용자 계정 이름 등의 사용자 식별 가능 정보를 마스킹할 수 있습니다. 00000000-0000-0000-0000-00000000@domain.com 

Azure Portal의 다른 보고서와 마찬가지로, 필터를 사용하여 보고서의 범위를 지정할 수 있습니다. 특정 시간 간격, 사용자, 애플리케이션, 클라이언트 또는 상태에 따라 필터링할 수 있습니다. **열** 단추를 선택하는 경우 다음 필드를 조합하여 데이터를 표시하도록 선택할 수 있습니다.

- **사용자**
- **애플리케이션**
- **상태**
- **Date**
- **날짜(UTC)** (UTC는 협정 세계시임)
- **MFA 인증 방법**(다단계 인증 방법)
- **MFA 인증 세부 정보**(다단계 인증 세부 정보)
- **MFA 결과**
- **IP 주소**
- **클라이언트**
- **사용자 이름**
- **위치**
- **대상 테넌트 ID**

## <a name="microsoft-365-support"></a>Microsoft 365 지원

Microsoft 365 응용 프로그램은 테 넌 트 제한을 완전히 지원 하기 위해 두 가지 조건을 충족 해야 합니다.

1. 사용되는 클라이언트에서 최신 인증을 지원해야 합니다.
2. 최신 인증이 클라우드 서비스에 대한 기본 인증 프로토콜로 사용되어야 합니다.

현재 최신 인증을 지원하는 Office 클라이언트에 대한 최신 정보를 보려면 [업데이트된 Office 365 최신 인증](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/)을 참조하세요. 해당 페이지에는 특정 Exchange Online 및 비즈니스용 Skype Online 테넌트에 대해 최신 인증을 사용하도록 설정하기 위한 지침으로 연결되는 링크도 포함되어 있습니다. SharePoint Online에서는 이미 최신 인증이 사용하도록 설정되어 있습니다.

Microsoft 365 브라우저 기반 응용 프로그램 (웹의 Office Portal, Yammer, SharePoint 사이트, Outlook 등)은 현재 테 넌 트 제한을 지원 합니다. 씩 클라이언트(Outlook, 비즈니스용 Skype, Word, Excel, PowerPoint 등)는 최신 인증을 사용하는 경우에만 테넌트 제한을 적용할 수 있습니다.  

최신 인증을 지원하는 Outlook 및 비즈니스용 Skype 클라이언트는 최신 인증이 사용되도록 설정되지 않은 테넌트에 대해 레거시 프로토콜을 사용하여 테넌트 제한을 효과적으로 우회할 수 있습니다. 레거시 프로토콜을 사용하는 애플리케이션은 인증하는 동안 login.microsoftonline.com, login.microsoft.com 또는 login.windows.net에 접속하는 경우 테넌트 제한으로 차단될 수 있습니다.

Windows의 Outlook에서 고객은 최종 사용자가 프로필에 승인되지 않은 메일 계정을 추가하는 것을 방지하는 제한을 구현하도록 선택할 수 있습니다. 예제를 보려면 [비기본 Exchange 계정 추가 금지](https://gpsearch.azurewebsites.net/default.aspx?ref=1) 그룹 정책 설정을 참조하세요.

## <a name="testing"></a>테스트

전체 조직을 위해 구현하기 전에 테넌트 제한을 시험해보려면 Fiddler와 같은 도구를 사용한 호스트 기반 접근 방법과 프록시 설정의 단계별 롤아웃 방법 중에서 선택할 수 있습니다.

### <a name="fiddler-for-a-host-based-approach"></a>호스트 기반 접근 방식을 위한 Fiddler

Fiddler는 HTTP 헤더 삽입을 비롯하여 HTTP/HTTPS 트래픽을 캡처하고 수정하는 데 사용할 수 있는 무료 웹 디버깅 프록시입니다. 테넌트 제한을 테스트하도록 Fiddler를 구성하려면 다음 단계를 수행합니다.

1. [Fiddler 다운로드하고 설치합니다](https://www.telerik.com/fiddler).

2. [Fiddler 도움말 설명서](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)에 따라 HTTPS 트래픽 암호를 해독하도록 Fiddler를 구성합니다.

3. 사용자 지정 규칙을 사용하여 *Restrict-Access-To-Tenants* 및 *Restrict-Access-Context* 머리글을 삽입하도록 Fiddler를 구성합니다.

   1. Fiddler Web Debugger 도구에서 **Rules** 메뉴를 선택하고 **Customize Rules...** 를 선택하여 CustomRules 파일을 엽니다.

   2. `OnBeforeRequest` 함수의 시작 부분에 다음 줄을 추가합니다. \<tenant domain\>을 테 넌 트에 등록 된 도메인 (예:)으로 바꿉니다 `contoso.onmicrosoft.com` . \<directory ID\>을 테 넌 트의 AZURE AD GUID 식별자로 바꿉니다.

      ```JScript.NET
      if (
          oSession.HostnameIs("login.microsoftonline.com") ||
          oSession.HostnameIs("login.microsoft.com") ||
          oSession.HostnameIs("login.windows.net")
      )
      {
          oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";
          oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";
      }
      ```

      여러 테넌트를 허용해야 하는 경우 쉼표로 테넌트 이름을 구분합니다. 다음은 그 예입니다.

      `oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";`

4. CustomRules 파일을 저장한 후 닫습니다.

Fiddler를 구성한 후 **파일** 메뉴로 이동하고 **트래픽 캡처** 를 선택하여 트래픽을 캡처할 수 있습니다.

### <a name="staged-rollout-of-proxy-settings"></a>프록시 설정의 단계별 롤아웃

프록시 인프라의 기능에 따라 사용자에게 설정을 단계별로 롤아웃할 수 있습니다. 다음은 고려해야 할 몇 가지 고급 옵션입니다.

1. 일반 사용자는 프로덕션 프록시 인프라를 계속 사용할 수 있지만 PAC 파일을 사용하여 테스트 사용자는 테스트 프록시 인프라를 가리키도록 합니다.
2. 일부 프록시 서버는 그룹을 사용하여 다양한 구성을 지원할 수 있습니다.

특정 세부 정보에 대해서는 프록시 서버 설명서를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [업데이트된 Office 365 최신 인증](https://www.microsoft.com/en-us/microsoft-365/blog/2015/03/23/office-2013-modern-authentication-public-preview-announced/) 참조
- [Office 365 URL 및 IP 주소 범위](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) 검토
