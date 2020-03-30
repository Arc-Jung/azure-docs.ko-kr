---
title: 스마트 잠금을 사용한 공격 방지 - Azure Active Directory
description: Azure Active Directory 스마트 잠금을 통해 암호를 추측하려는 무차별 암호 대입 공격으로부터 조직을 보호합니다.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfd49a4429dc0d7f5db07a577016c21de8fc58d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75762878"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory 스마트 잠금

스마트 잠금은 사용자의 암호를 추측하려거나 무차별 암호 대입 공격을 사용하여 침입하려는 불량 작업자를 차단하도록 도와줍니다. 이 기능은 유효한 사용자로부터 오는 로그인을 인식하고 이를 공격자 및 기타 알 수 없는 원본 중 하나와 다르게 취급합니다. 스마트 잠금은 사용자가 계정에 계속 액세스하여 생산성을 유지할 수 있게 해주면서도 공격자를 차단합니다.

스마트 잠금 기능은 기본적으로 로그인 실패가 10회 발생하면 1분 동안 계정에 로그인 시도를 차단합니다. 후속 로그인 시도가 실패할 때마다 계정이 다시 잠기는데, 처음에는 1분간 잠기고 그 이후에는 더 길게 잠깁니다.

스마트 잠금 기능은 동일한 암호에 대해 잠금 카운터가 증가하는 것을 방지하기 위해 마지막 세 개의 잘못된 암호 해시를 추적합니다. 동일한 잘못된 암호를 여러 번 입력하면 이 동작으로 인해 계정이 잠기지 않습니다.

 > [!NOTE]
 > 통과 인증을 사용하도록 설정한 고객은 인증이 클라우드가 아닌 온-프레미스에서 발생하므로 해시 추적 기능을 사용할 수 없습니다.

AD FS 2016 및 AF FS 2019를 사용하는 페더레이션 배포는 [AD FS 엑스트라넷 잠금 및 엑스트라넷 스마트 잠금을](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)사용하여 유사한 이점을 지원할 수 있습니다.

스마트 잠금은 보안 및 유용성의 적절한 혼합을 제공하는 기본 설정으로 모든 Azure AD 고객에 대해 항상 활성 상태입니다. 조직에 특정한 값을 사용하여 스마트 잠금 설정을 사용자 지정하려면 사용자에 대한 유료 Azure AD 라이선스가 필요합니다.

스마트 잠금을 사용한다고 해서 정품 사용자가 절대로 잠기지 않는다는 보장은 없습니다. 스마트 잠금이 사용자 계정을 잠글 때, 우리는 진짜 사용자를 잠그지 않도록 최선을 다합니다. 잠금 서비스는 악의적인 행위자가 진정한 사용자 계정에 액세스할 수 없도록 합니다.  

* 각 Azure Active Directory 데이터 센터는 독립적으로 잠금을 추적합니다. 사용자가 각 데이터 센터에 방문하면 사용자는 (threshold_limit * datacenter_count) 시도 횟수를 갖게 됩니다.
* 스마트 잠금은 익숙한 위치 및 낯선 위치를 사용하여 악의적 행위자와 진정한 사용자를 구별합니다. 낯설고 익숙한 위치는 모두 별도의 잠금 카운터를 갖게 됩니다.

스마트 잠금은 암호 해시 동기화 또는 통과 인증을 사용하여 공격자에 의해 차단되는 온-프레미스 Active Directory 계정을 보호하도록 하이브리드 배포와 통합될 수 있습니다. Azure AD에서 스마트 잠금 정책을 적절하게 설정하여 공격자가 온-프레미스 Active Directory에 도달하기 전에 필터링할 수 있습니다.

[통과 인증](../hybrid/how-to-connect-pta.md)을 사용하는 경우 다음을 확인해야 합니다.

* Azure AD 잠금 임계값이 Active Directory 계정 잠금 임계값**보다 작습니다**. Active Directory의 계정 잠금 임계값을 Azure AD의 잠금 임계값보다 최소 2~3배 이상 크게 설정합니다. 
* Azure AD 잠금 기간은 기간 이후에 Active Directory 재설정 계정 잠금 카운터보다 더 길게 설정해야 합니다. Azure AD 기간은 초 단위로 설정되고 AD 기간은 분단위로 설정됩니다. 

예를 들어 Azure AD 카운터를 AD보다 높게 설정하려는 경우 Azure AD는 120초(2분)가 되고 온-프레미스 AD는 1분(60초)로 설정됩니다.

> [!IMPORTANT]
> 현재 관리자는 스마트 잠금 기능으로 잠긴 경우 사용자의 클라우드 계정을 잠금 해제할 수 없습니다. 잠금 기간이 만료될 때까지 기다려야 합니다. 그러나 사용자는 신뢰할 수 있는 장치 또는 위치에서 셀프 서비스 암호 재설정(SSPR)을 사용하여 잠금을 해제할 수 있습니다.

## <a name="verify-on-premises-account-lockout-policy"></a>온-프레미스 계정 잠금 정책 확인

온-프레미스 Active Directory 계정 잠금 정책을 확인하려면 다음 지침을 사용합니다.

1. 그룹 정책 관리 도구를 엽니다.
2. 조직의 계정 잠금 정책(예: **기본 도메인 정책**)을 포함하는 그룹 정책을 편집합니다.
3. 컴퓨터 **구성** > **정책으로** > 찾아보기**Windows 설정** > **보안 설정** > **계정 정책** > **계정 잠금 정책**.
4. 계정 **잠금 임계값을** 확인하고 값 **후 계정 잠금 카운터를 재설정합니다.**

![온-프레미스 Active Directory 계정 잠금 정책 수정](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD 스마트 잠금 값 관리

조직의 요구 사항에 따라 스마트 잠금 값은 사용자 지정되어야 할 수 있습니다. 조직에 특정한 값을 사용하여 스마트 잠금 설정을 사용자 지정하려면 사용자에 대한 유료 Azure AD 라이선스가 필요합니다.

조직에 대한 스마트 잠금 값을 확인하거나 수정하려면 다음 단계를 사용합니다.

1. [Azure 포털에](https://portal.azure.com)로그인합니다.
1. *Azure Active Directory*를 검색하고 선택합니다. **보안** > **인증 방법** > **선택 암호 보호**.
1. 첫 번째 잠금 전에 계정에서 허용되는 실패한 로그인 횟수에 따라 **잠금 임계값**을 설정합니다. 기본값은 10입니다.
1. **잠금 지속 기간(초)** 을 각 잠금의 길이(초)로 설정합니다. 기본값은 60초(1분)입니다.

> [!NOTE]
> 잠금 후 첫 번째 로그인도 실패하는 경우 계정은 다시 잠깁니다. 계정이 반복적으로 잠기는 경우 잠금 지속 시간이 증가합니다.

![Azure Portal에서 Azure AD 스마트 잠금 정책 사용자 지정](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)

## <a name="how-to-determine-if-the-smart-lockout-feature-is-working-or-not"></a>스마트 잠금 기능이 작동하는지 여부를 확인하는 방법

스마트 잠금 임계값이 트리거되면 계정이 잠겨 있는 동안 다음 메시지가 표시됩니다.

**무단 사용을 방지하기 위해 계정이 일시적으로 잠깁니다. 나중에 다시 시도하면 문제가 계속되면 관리자에게 문의하십시오.**

## <a name="next-steps"></a>다음 단계

* [Azure AD를 사용하여 조직에서 불량 암호를 금지하는 방법을 알아봅니다.](howto-password-ban-bad.md)
* [사용자가 고유의 계정을 잠금 해제하도록 셀프 서비스 암호 재설정을 구성합니다.](quickstart-sspr.md)
