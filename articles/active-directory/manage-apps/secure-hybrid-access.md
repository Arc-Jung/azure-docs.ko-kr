---
title: Azure AD 보안 하이브리드 액세스 | Microsoft Docs
description: 이 문서에서는 레거시 온-프레미스, 퍼블릭 클라우드 또는 프라이빗 클라우드 애플리케이션을 Azure AD와 통합하기 위한 파트너 솔루션에 대해 설명합니다. 앱 배달 컨트롤러 또는 네트워크를 Azure AD에 연결하여 레거시 앱을 보호하세요.
services: active-directory
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.date: 2/16/2021
ms.author: gasinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68a4241a70577ce359320d388882312cf06090ba
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101645428"
---
# <a name="secure-hybrid-access-secure-legacy-apps-with-azure-active-directory"></a>보안 하이브리드 액세스: Azure Active Directory을 사용 하 여 레거시 앱 보호

이제 다음을 사용 하 여 온-프레미스 및 클라우드 레거시 인증 응용 프로그램을 Azure Active Directory (AD)에 연결 하 여 보호할 수 있습니다.

- [Azure AD 응용 프로그램 프록시](#secure-hybrid-access-sha-through-azure-ad-application-proxy)

- [기존 응용 프로그램 제공 컨트롤러 및 네트워크](#sha-through-networking-and-delivery-controllers)

- [VPN (가상 사설망) 및 SDP (Software-Defined 경계) 응용 프로그램](#sha-through-vpn-and-sdp-applications)

Azure ad [조건부 액세스](../conditional-access/overview.md) 및 Azure Ad [id 보호](../identity-protection/overview-identity-protection.md)와 같은 azure ad 기능을 사용 하 여 모든 응용 프로그램에서 격차를 브리징 하 고 보안 상태를 강화할 수 있습니다.

## <a name="secure-hybrid-access-sha-through-azure-ad-application-proxy"></a>Azure AD 응용 프로그램 프록시를 통한 SHA (Secure hybrid access)
  
[응용 프로그램 프록시](./what-is-application-proxy.md) 를 사용 하 여 온-프레미스 웹 응용 프로그램에 대 한 [보안 원격 액세스](./application-proxy.md) 를 제공할 수 있습니다. 사용자가 VPN을 사용할 필요가 없습니다. 사용자는 [Single Sign-On](./add-application-portal-setup-sso.md)후에 모든 장치에서 응용 프로그램에 쉽게 연결 하 여 이점을 누릴 수 있습니다. 응용 프로그램 프록시는 원격 액세스를 서비스로 제공 하므로 [온-프레미스 응용 프로그램](./application-proxy-add-on-premises-application.md) 을 회사 네트워크 외부의 사용자에 게 쉽게 게시할 수 있습니다. 온-프레미스 응용 프로그램을 수정할 필요 없이 클라우드 액세스 관리를 확장 하는 데 도움이 됩니다. 다음 단계로 [Azure AD 응용 프로그램 프록시 배포를 계획](./application-proxy-deployment-plan.md) 합니다.

## <a name="azure-ad-partner-integrations"></a>Azure AD 파트너 통합

### <a name="sha-through-networking-and-delivery-controllers"></a>네트워킹 및 배달 컨트롤러를 통한 SHA

[Azure AD 응용 프로그램 프록시](./what-is-application-proxy.md)외에도, Microsoft에서 제공 하는 [제로 신뢰 프레임 워크](https://www.microsoft.com/security/blog/2020/04/02/announcing-microsoft-zero-trust-assessment-tool/)를 사용 하 여 타사 공급자와 협력 하 고 있습니다. 기존 네트워킹 및 배달 컨트롤러를 사용 하 고 비즈니스 프로세스에 중요 하지만 Azure AD를 사용 하기 전에는 보호할 수 없는 레거시 응용 프로그램을 쉽게 보호할 수 있습니다. 이러한 응용 프로그램을 보호 하기 시작 하는 데 필요한 모든 것이 이미 있을 가능성이 높습니다.

![네트워킹 파트너 및 앱 프록시를 사용 하 여 보안 하이브리드 액세스를 보여 주는 이미지](./media/secure-hybrid-access/secure-hybrid-access.png)

다음 네트워킹 공급 업체는 Azure AD와 통합 하기 위한 미리 작성 된 솔루션과 자세한 지침을 제공 합니다.

- [Akamai EAA(엔터프라이즈 애플리케이션 액세스)](https://docs.microsoft.com/azure/active-directory/saas-apps/akamai-tutorial)

- [Citrix ADC(Application Delivery Controller)](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/saas-apps/headerf5-tutorial)

- [Kemp](https://docs.microsoft.com/azure/active-directory/saas-apps/kemp-tutorial)

- [VTM (Pulse Secure Virtual Traffic Manager)](https://docs.microsoft.com/azure/active-directory/saas-apps/pulse-secure-virtual-traffic-manager-tutorial)

### <a name="sha-through-vpn-and-sdp-applications"></a>VPN 및 SDP 응용 프로그램을 통한 SHA

VPN 및 SDP 솔루션을 사용 하 여 조직의 데이터를 보호 하는 동안 언제 든 지 모든 장치에서 엔터프라이즈 네트워크에 대 한 보안 액세스를 제공할 수 있습니다. IDP (Id 공급자)로 Azure AD를 사용 하 여 Azure AD [Single sign-on](./what-is-single-sign-on.md) 및 [multi-factor authentication](../authentication/concept-mfa-howitworks.md) 과 같은 최신 인증 및 권한 부여 방법을 사용 하 여 온-프레미스 레거시 응용 프로그램을 보호할 수 있습니다.  

![VPN 파트너와 앱 프록시를 사용 하 여 보안 하이브리드 액세스를 보여 주는 이미지 ](./media/secure-hybrid-access/app-proxy-vpn.png)

다음 VPN 공급 업체는 Azure AD와 통합 하기 위한 미리 작성 된 솔루션과 자세한 지침을 제공 합니다.

- [Cisco AnyConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-anyconnect)

- [Fortinet](https://docs.microsoft.com/azure/active-directory/saas-apps/fortigate-ssl-vpn-tutorial)

- [F5 Big-IP APM](https://docs.microsoft.com/azure/active-directory/manage-apps/f5-aad-password-less-vpn)

- [Palo Alto Networks 글로벌 보호](https://docs.microsoft.com/azure/active-directory/saas-apps/paloaltoadmin-tutorial)

- [펄스 보안 펄스 연결 보안 (PC)](https://docs.microsoft.com/azure/active-directory/saas-apps/pulse-secure-pcs-tutorial)

다음 SDP 공급 업체는 Azure AD와 통합 하기 위한 미리 작성 된 솔루션과 자세한 지침을 제공 합니다.

- [Datawiza Access Broker](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-oidc-sso)

- [Perimeter 81](https://docs.microsoft.com/azure/active-directory/saas-apps/perimeter-81-tutorial#:~:text=For%20SSO%20to%20work,%20you%20need%20to%20establish,to%20test%20Azure%20AD%20single%20sign-on%20with%20B.Simon.)

- [Silverfort 인증 플랫폼](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal-setup-oidc-sso)

- [Strata](https://docs.microsoft.com/azure/active-directory/saas-apps/maverics-identity-orchestrator-saml-connector-tutorial)

- [ZPA(Zscaler Private Access)](https://docs.microsoft.com/azure/active-directory/saas-apps/zscalerprivateaccess-tutorial)
