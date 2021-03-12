---
title: Azure AD Domain Services에서 계정 잠금 문제 해결 | Microsoft Docs
description: Azure Active Directory Domain Services에서 사용자 계정을 잠그기 위해 발생 하는 일반적인 문제를 해결 하는 방법에 대해 알아봅니다.
services: active-directory-ds
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 3341f290a5a5bb169b6e70ea22459a2afafedbbc
ms.sourcegitcommit: 5f32f03eeb892bf0d023b23bd709e642d1812696
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103198954"
---
# <a name="troubleshoot-account-lockout-problems-with-an-azure-active-directory-domain-services-managed-domain"></a>Azure Active Directory Domain Services 관리 되는 도메인의 계정 잠금 문제 해결

악의적인 로그인을 반복적으로 시도 하지 않도록 방지 하기 위해 Azure Active Directory Domain Services (Azure AD DS) 관리 되는 도메인은 정의 된 임계값 후에 계정을 잠급니다. 이 계정 잠금은 로그인 공격 인시던트 없이 우연히도 발생할 수 있습니다. 예를 들어 사용자가 반복 해 서 잘못 된 암호를 입력 하거나 서비스에서 이전 암호를 사용 하려고 하면 계정이 잠깁니다.

이 문제 해결 문서에서는 계정 잠금이 발생 하는 이유와 동작을 구성 하는 방법과 보안 감사를 검토 하 여 잠금 이벤트 문제를 해결 하는 방법을 설명 합니다.

## <a name="what-is-an-account-lockout"></a>계정 잠금 이란?

실패 한 로그인 시도에 대해 정의 된 임계값이 충족 되 면 Azure AD DS 관리 되는 도메인의 사용자 계정이 잠깁니다. 이 계정 잠금 동작은 자동 디지털 공격을 나타낼 수 있는 무작위 로그인 시도를 반복 해 서 방지 하도록 설계 되었습니다.

**기본적으로 2 분 내에 잘못 된 암호를 5 번 시도 하면 계정이 30 분 동안 잠깁니다.**

기본 계정 잠금 임계값은 세분화 된 암호 정책을 사용 하 여 구성 됩니다. 특정 요구 사항 집합이 있는 경우 이러한 기본 계정 잠금 임계값을 재정의할 수 있습니다. 그러나 계정 잠금의 수를 줄이기 위해 임계값 제한을 늘리는 것은 권장 되지 않습니다. 먼저 계정 잠금 동작의 원본 문제를 해결 합니다.

### <a name="fine-grained-password-policy"></a>세분화 되는 암호 정책

Fgpp입니다 (세분화 된 암호 정책)를 사용 하면 암호 및 계정 잠금 정책에 대 한 특정 제한을 도메인의 다른 사용자에 게 적용할 수 있습니다. FGPP는 관리 되는 도메인 내의 사용자 에게만 영향을 줍니다. Azure AD에서 관리 되는 도메인으로 동기화 되는 클라우드 사용자 및 도메인 사용자는 관리 되는 도메인 내에서 암호 정책의 영향을 받습니다. Azure AD 또는 온-프레미스 디렉터리의 계정이 영향을 받지 않습니다.

정책은 관리 되는 도메인의 그룹 연결을 통해 배포 되 고 변경 내용은 다음 사용자 로그인 시 적용 됩니다. 정책을 변경 하면 이미 잠겨 있는 사용자 계정의 잠금이 해제 되지 않습니다.

세분화 된 암호 정책에 대 한 자세한 내용 및 azure에서 AD DS 직접 만든 사용자와 Azure AD에서 동기화 된 사용자 간의 차이점에 대 한 자세한 내용은 [암호 및 계정 잠금 정책 구성][configure-fgpp]을 참조 하세요.

## <a name="common-account-lockout-reasons"></a>일반적인 계정 잠금 이유

악의적인 의도 나 요인을 제외 하 고 계정이 잠기는 가장 일반적인 이유는 다음과 같습니다.

* **사용자가 자신을 잠 궜 습니다.**
    * 최근 암호를 변경한 후 사용자가 이전 암호를 계속 사용 하 고 있습니까? 사용자가 실수로 이전 암호를 다시 시도 하 여 2 분 이내에 실패 한 5 번의 기본 계정 잠금 정책이 발생할 수 있습니다.
* **이전 암호를 포함 하는 응용 프로그램 또는 서비스가 있습니다.**
    * 응용 프로그램 또는 서비스에서 계정을 사용 하는 경우 해당 리소스는 이전 암호를 사용 하 여 반복적으로 로그인을 시도할 수 있습니다. 이 동작으로 인해 계정이 잠깁니다.
    * 여러 다른 응용 프로그램 또는 서비스에서 계정 사용을 최소화 하 고 자격 증명이 사용 되는 위치를 기록 합니다. 계정 암호가 변경 된 경우 연결 된 응용 프로그램 또는 서비스를 적절 하 게 업데이트 합니다.
* **다른 환경에서 암호가 변경 되었으며 새 암호가 아직 동기화 되지 않았습니다.**
    * 프레미스 AD DS 환경과 같이 관리 되는 도메인 외부에서 계정 암호를 변경 하는 경우 Azure AD를 통해 관리 되는 도메인으로 암호 변경을 동기화 하는 데 몇 분 정도 걸릴 수 있습니다.
    * 해당 암호 동기화 프로세스가 완료 되기 전에 관리 되는 도메인의 리소스에 로그인 하려고 시도 하는 사용자는 계정이 잠깁니다.

## <a name="troubleshoot-account-lockouts-with-security-audits"></a>보안 감사를 사용 하 여 계정 잠금 문제 해결

계정 잠금 이벤트가 발생 하는 경우와 이러한 이벤트가 발생 하는 위치를 해결 하려면 [Azure AD DS에 대 한 보안 감사를 사용 하도록 설정][security-audit-events]합니다. 감사 이벤트는 기능을 사용 하도록 설정한 시간에만 캡처됩니다. 문제를 해결 하기 위해 계정 잠금 문제가 발생 *하기 전에* 보안 감사를 사용 하도록 설정 하는 것이 가장 좋습니다. 사용자 계정에 잠금 문제가 반복적으로 발생 하는 경우 다음에 상황이 발생할 때 보안 감사를 준비 하도록 설정할 수 있습니다.

보안 감사를 사용 하도록 설정 하면 다음 샘플 쿼리에서 *계정 잠금 이벤트*, 코드 *4740* 을 검토 하는 방법을 보여 줍니다.

지난 7 일간의 모든 계정 잠금 이벤트를 표시 합니다.

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

*Driley* 이라는 계정에 대해 지난 7 일간의 모든 계정 잠금 이벤트를 표시 합니다.

```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
| where "driley" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

2020 년 6 월 26 일 오전 9 시 사이의 모든 계정 잠금 이벤트 보기 날짜 및 시간을 기준으로 하 여 2020 년 7 월 1 일에서 내림차순으로 정렬 됨:

```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2020-06-26 09:00) and TimeGenerated <= datetime(2020-07-01)
| where OperationName has "4740"
| sort by TimeGenerated asc
```

**참고**

"원본 워크스테이션:"이 비어 있는 4776 및 4740 이벤트 세부 정보를 확인할 수 있습니다. 이는 다른 장치를 통해 네트워크 로그온을 통해 잘못 된 암호가 발생 했기 때문입니다.
예: RADIUS 서버를 사용할 경우 AAD DS에 인증을 전달할 수 있습니다. DC로 RDP를 사용 하 여 백 엔드를 구성할 수 있는지 확인 하려면 netlogon 로그를 구성 합니다.

03/04 19:07:29 [LOGON] [10752] contoso: SamLogon: contoso\Nagappan.Veerappan from (LOB11-RADIUS)의 전이적 네트워크 로그온 

03/04 19:07:29 [LOGON] [10752] contoso: SamLogon: contoso\Nagappan.Veerappan from (LOB11-RADIUS)의 전이적 네트워크 로그온은 0xC000006A을 반환 합니다.

03/04 19:07:35 [LOGON] [10753] contoso: SamLogon: contoso\Nagappan.Veerappan from (LOB11-RADIUS)의 전이적 네트워크 로그온 

03/04 19:07:35 [LOGON] [10753] contoso: SamLogon: contoso\Nagappan.Veerappan from (LOB11-RADIUS)의 전이적 네트워크 로그온은 0xC000006A을 반환 합니다.

진단 캡처를 구성 하기 위해 NSG에서 Dc에 대 한 RDP를 백 엔드에 설정 (즉, netlogon) https://docs.microsoft.com/azure/active-directory-domain-services/alert-nsg#inbound-security-rules 기본 NSG를 이미 수정 했으면 PSlet을 사용 하 여 사용 하도록 설정 하세요. https://docs.microsoft.com/azure/active-directory-domain-services/network-considerations#port-3389---management-using-remote-desktop

모든 서버에서 Netlogon 로그를 사용 하도록 설정 하려면 https://docs.microsoft.com/troubleshoot/windows-client/windows-security/enable-debug-logging-netlogon-service

## <a name="next-steps"></a>다음 단계

계정 잠금 임계값을 조정 하는 세분화 되는 암호 정책에 대 한 자세한 내용은 [암호 및 계정 잠금 정책 구성][configure-fgpp]을 참조 하세요.

VM을 관리 되는 도메인에 가입 하는 데 문제가 있는 경우 [도움말을 찾고 Azure Active Directory에 대 한 지원 티켓을 엽니다][azure-ad-support].

<!-- INTERNAL LINKS -->
[configure-fgpp]: password-policy.md
[security-audit-events]: security-audit-events.md
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
