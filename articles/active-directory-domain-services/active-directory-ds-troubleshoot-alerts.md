---
title: "Azure Active Directory Domain Services: 경고 문제 해결 | Microsoft Docs"
description: "Azure AD Domain Services에 대한 경고 문제 해결"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: ergreenl
ms.openlocfilehash: 8a0b30e6c975bd8f3bfbe70a64c085b729115f24
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services - 경고 문제 해결
이 문서에서는 관리되는 도메인에서 발생할 수 있는 경고에 대한 문제 해결 가이드를 제공합니다.


발생하는 경고 ID 또는 메시지에 해당하는 문제 해결 단계를 선택합니다.

| **경고 ID** | **경고 메시지** | **해결 방법** |
| --- | --- | :--- |
| AADDS001 | *관리되는 도메인에 대해 인터넷을 통해 보안 LDAP가 사용되도록 설정됩니다. 그러나 포트 636에 대한 액세스는 네트워크 보안 그룹을 사용하여 잠글 수 없습니다. 이로 인해 관리되는 도메인의 사용자 계정은 암호 무차별 대입 공격에 노출될 수 있습니다.* | [잘못된 보안 LDAP 구성](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *관리되는 도메인에 연결된 Azure AD 디렉터리가 삭제되었을 수 있습니다. 관리되는 도메인은 더 이상 지원되는 구성에 포함되지 않습니다. Microsoft는 관리되는 도메인을 모니터링, 관리, 패치 및 동기화할 수 없습니다.* | [누락된 디렉터리](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services를 Azure AD B2B 디렉터리에서 사용 가능하게 설정할 수 없습니다.* | [이 디렉터리에서 Azure AD B2C가 실행되고 있습니다.](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD Domain Services가 제대로 작동하는 데 필요한 서비스 주체가 Azure AD 디렉터리에서 삭제되었습니다. 이 구성은 관리되는 도메인을 모니터링, 관리, 패치, 동기화하는 Microsoft의 기능에 영향을 줍니다.* | [누락된 서비스 주체](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Azure AD Domain Services를 사용하도록 설정한 가상 네트워크의 IP 주소 범위가 공용 IP 범위에 있습니다. Azure AD Domain Services는 개인 IP 주소 범위를 갖는 가상 네트워크에서 사용되도록 설정해야 합니다. 이 구성은 관리되는 도메인을 모니터링, 관리, 패치, 동기화하는 Microsoft의 기능에 영향을 줍니다.* | [주소가 공용 IP 범위에 있습니다.](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft는 이 관리되는 도메인에 대한 도메인 컨트롤러에 연결할 수 없습니다. 가상 네트워크에 구성된 NSG(네트워크 보안 그룹)가 관리되는 도메인에 대한 액세스를 차단하려고 할 때 이 문제가 발생할 수 있습니다. 다른 가능한 이유는 인터넷에서 들어오는 트래픽을 차단하는 사용자 정의 경로가 있는 경우입니다.* | [네트워크 오류](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: 누락된 디렉터리
**경고 메시지:**

*관리되는 도메인에 연결된 Azure AD 디렉터리가 삭제되었을 수 있습니다. 관리되는 도메인은 더 이상 지원되는 구성에 포함되지 않습니다. Microsoft는 관리되는 도메인을 모니터링, 관리, 패치 및 동기화할 수 없습니다.*

**재구성:**

이 오류는 일반적으로 Azure 구독을 새 Azure AD 디렉터리로 잘못 이동하고, 여전히 Azure AD Domain Services와 연결되어 있는 이전 Azure AD 디렉터리를 삭제하는 경우에 발생합니다.

이 오류는 복구할 수 없습니다. 이 문제를 해결하려면 [기존의 관리되는 도메인을 삭제](active-directory-ds-disable-aadds.md)하고 새 디렉터리에 다시 만듭니다. 삭제하는 데 문제가 있으면 Azure Active Directory Domain Services 제품 팀에 문의하여 [지원](active-directory-ds-contact-us.md)을 요청하세요.

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C가 이 디렉터리에서 실행 중임
**경고 메시지:**

*Azure AD Domain Services를 Azure AD B2B 디렉터리에서 사용 가능하게 설정할 수 없습니다.*

**재구성:**

>[!NOTE]
>Azure AD Domain Services를 계속 사용하려면 Azure AD B2C가 아닌 디렉터리에서 Azure AD Domain Services 인스턴스를 다시 만들어야 합니다.

서비스를 복원하려면 다음 단계를 수행합니다.

1. 기존 Azure AD 디렉터리에서 [관리되는 도메인을 삭제](active-directory-ds-disable-aadds.md)합니다.
2. Azure AD B2C 디렉터리가 아닌 새 디렉터리를 만듭니다.
3. [시작](active-directory-ds-getting-started.md) 가이드에 따라 관리되는 도메인을 다시 만듭니다.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: 주소가 공용 IP 범위에 있음

**경고 메시지:**

*Azure AD Domain Services를 사용하도록 설정한 가상 네트워크의 IP 주소 범위가 공용 IP 범위에 있습니다. Azure AD Domain Services는 개인 IP 주소 범위를 갖는 가상 네트워크에서 사용되도록 설정해야 합니다. 이 구성은 관리되는 도메인을 모니터링, 관리, 패치, 동기화하는 Microsoft의 기능에 영향을 줍니다.*

**재구성:**

> [!NOTE]
> 이 문제를 해결하기 위해 기존의 관리되는 도메인을 삭제하고, 개인 IP 주소 범위를 사용하여 가상 네트워크에 다시 만들어야 합니다. 이 프로세스는 중단됩니다.

시작하기 전에 [이 문서](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces)의 **개인 IP V4 주소 공간** 섹션을 읽습니다.

가상 네트워크 내에서 컴퓨터는 서브넷에 대해 구성된 것과 동일한 IP 주소 범위에 있는 Azure 리소스를 요청할 수 있습니다. 그러나 가상 네트워크가 이 범위에서 구성되므로 해당 요청은 가상 네트워크 내에서 라우팅되고 의도한 웹 리소스에 도달하지 않습니다. 그러면 Azure AD Domain Services에서 예측할 수 없는 오류가 발생할 수 있습니다.

**가상 네트워크에서 구성된 인터넷의 IP 주소 범위가 있는 경우 이 경고는 무시될 수 있습니다. 그러나 Azure AD Domain Services는 예측할 수 없는 오류를 일으킬 수 있으므로 이 구성을 사용하여 [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)]에 커밋할 수 없습니다.**


1. 디렉터리에서 [관리되는 도메인을 삭제](active-directory-ds-disable-aadds.md)합니다.
2. 서브넷에 대한 IP 주소 범위 수정
  1. [Azure Portal의 가상 네트워크 페이지](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks)로 이동합니다.
  2. Azure AD Domain Services에 사용하려는 가상 네트워크를 선택합니다.
  3. 설정 아래의 **주소 공간**을 클릭합니다.
  4. 기존 주소 범위를 클릭하고 편집하거나 다른 주소 범위를 추가하여 주소 범위를 업데이트합니다. 새 주소 범위가 개인 IP 범위에 있는지 확인합니다. 변경 내용을 저장합니다.
  5. 왼쪽 탐색 창에서 **서브넷**을 클릭합니다.
  6. 테이블에서 편집하려는 서브넷을 클릭합니다.
  7. 주소 범위를 업데이트하고 변경 내용을 저장합니다.
3. [Azure AD Domain Services를 사용하여 시작 가이드](active-directory-ds-getting-started.md)에 따라 관리되는 도메인을 다시 만듭니다. 개인 IP 주소 범위를 갖는 가상 네트워크를 선택해야 합니다.
4. 가상 머신을 새 도메인에 가입하려면 [이 가이드](active-directory-ds-admin-guide-join-windows-vm-portal.md)를 따릅니다.
8. 경고를 해결하려면 두 시간 내에 도메인의 상태를 확인합니다.


## <a name="contact-us"></a>문의처
[지원이 필요하거나 피드백을 공유하려면](active-directory-ds-contact-us.md)Azure Active Directory Domain Services 제품 팀에 문의하세요.
