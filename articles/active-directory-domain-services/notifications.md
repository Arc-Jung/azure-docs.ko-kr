---
title: Azure AD Domain Services에 대 한 전자 메일 알림 Microsoft Docs
description: Azure Active Directory Domain Services 관리 되는 도메인의 문제를 경고 하도록 전자 메일 알림을 구성 하는 방법에 대해 알아봅니다.
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 4242092af86f8bc06ef9c9eba068ce07725de01f
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96619167"
---
# <a name="configure-email-notifications-for-issues-in-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services의 문제에 대 한 전자 메일 알림 구성

Azure 플랫폼에서 Azure Active Directory Domain Services (Azure AD DS) 관리 되는 도메인의 상태를 모니터링 합니다. Azure Portal의 상태 페이지에는 관리 되는 도메인에 대 한 경고가 표시 됩니다. 문제가 적시에 응답 하는지 확인 하기 위해 Azure AD DS 관리 되는 도메인에서 검색 되는 즉시 상태 경고를 보고 하도록 전자 메일 알림을 구성할 수 있습니다.

이 문서에서는 관리 되는 도메인에 대 한 전자 메일 알림 받는 사람을 구성 하는 방법을 보여 줍니다.

## <a name="email-notification-overview"></a>전자 메일 알림 개요

관리 되는 도메인에 대 한 문제를 경고 하려면 전자 메일 알림을 구성할 수 있습니다. 이러한 전자 메일 알림은 경고가 표시 되는 관리 되는 도메인을 지정 하 고, 검색 시간 및 Azure Portal의 상태 페이지에 대 한 링크를 제공 합니다. 그런 다음 제공 된 문제 해결 조언을 따라 문제를 해결할 수 있습니다.

다음 예제 메일 알림은 관리 되는 도메인에 대 한 경고 또는 경고가 생성 되었음을 나타냅니다.

![이메일 알림 예제](./media/active-directory-domain-services-alerts/email-alert.png)

> [!WARNING]
> 메시지의 링크를 클릭 하기 전에 항상 확인 된 Microsoft 발신자의 전자 메일을 받았는지 확인 합니다. 전자 메일 알림은 항상 주소에서 제공 `azure-noreply@microsoft.com` 됩니다.

### <a name="why-would-i-receive-email-notifications"></a>이메일 알림을 수신하는 이유

Azure AD DS는 관리 되는 도메인에 대 한 중요 업데이트에 대 한 전자 메일 알림을 보냅니다. 이러한 알림은 서비스에 영향을 주는 긴급 한 문제에만 해당 되며 즉시 해결 해야 합니다. 각 전자 메일 알림은 관리 되는 도메인에 대 한 경고에 의해 트리거됩니다. 또한 경고는 Azure Portal에 표시 되며 [Azure AD DS health 페이지][check-health]에서 볼 수 있습니다.

Azure AD DS는 광고, 업데이트 또는 판매 용도로 전자 메일을 보내지 않습니다.

### <a name="when-will-i-receive-email-notifications"></a>이메일 알림을 수신하는 경우

관리 되는 도메인에서 [새 경고가][troubleshoot-alerts] 발견 되 면 즉시 알림이 전송 됩니다. 경고가 해결 되지 않으면 4 일 마다 추가 전자 메일 알림이 미리 알림으로 전송 됩니다.

### <a name="who-should-receive-the-email-notifications"></a>이메일 알림을 수신하는 대상

Azure AD DS에 대 한 전자 메일 받는 사람 목록은 관리 되는 도메인을 관리 하 고 변경할 수 있는 사용자로 구성 되어야 합니다. 이 전자 메일 목록은 경고 및 문제에 대 한 "첫 번째 응답자"로 간주 되어야 합니다.

전자 메일 알림을 받을 수 있는 전자 메일을 최대 5 개까지 더 추가할 수 있습니다. 전자 메일 알림을 받는 사람이 5 명 이상이 면 메일 그룹을 만들고 대신이를 알림 목록에 추가 합니다.

Azure AD 디렉터리의 모든 *전역 관리자* 와 *AAD DC Administrators* 그룹의 모든 구성원에 게 전자 메일 알림을 받도록 선택할 수도 있습니다. Azure AD DS는 전역 관리자 및 AAD DC 관리자 목록을 포함 하 여 최대 100 전자 메일 주소로 알림을 보냅니다.

## <a name="configure-email-notifications"></a>전자 메일 알림 구성

기존 전자 메일 알림 받는 사람을 검토 하거나 수신자를 더 추가 하려면 다음 단계를 완료 합니다.

1. Azure Portal에서 **Azure AD Domain Services** 를 검색하여 선택합니다.
1. 관리 되는 도메인 (예: *aaddscontoso.com*)을 선택 합니다.
1. Azure AD DS 리소스 창의 왼쪽에서 **알림 설정** 을 선택 합니다. 전자 메일 알림의 기존 받는 사람이 표시 됩니다.
1. 전자 메일 받는 사람을 추가 하려면 추가 받는 사람 테이블에 전자 메일 주소를 입력 합니다.
1. 완료 되 면 상단 탐색에서 **저장** 을 선택 합니다.

> [!WARNING]
> 알림 설정을 변경 하면 사용자 뿐 아니라 전체 관리 되는 도메인에 대 한 알림 설정이 업데이트 됩니다.

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>경고 이메일 알림을 받았지만 Azure Portal에 로그온하면 경고가 없습니다. 어떻게 되었나요?

경고가 해결 되 면 Azure Portal에서 경고가 지워집니다. 전자 메일 알림을 받는 사용자가 관리 되는 도메인에 대 한 경고를 해결 했거나 Azure platform에 의해 autoresolved 이유 때문일 수 있습니다.

### <a name="why-can-i-not-edit-the-notification-settings"></a>알림 설정을 편집할 수 없는 이유

Azure Portal에서 알림 설정 페이지에 액세스할 수 없는 경우 관리 되는 도메인을 편집할 수 있는 권한이 없습니다. 전역 관리자에 게 문의 하 여 Azure AD DS 리소스를 편집 하거나 받는 사람 목록에서 제거할 수 있는 권한을 얻으십시오.

### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>내 이메일 주소를 제공했지만 이메일 알림이 수신되지 않는 것 같습니다. 그 이유는

전자 메일에서 알림을 받을 스팸 또는 정크 폴더를 확인 하 고의 발신자를 허용 해야 `azure-noreply@microsoft.com` 합니다.

## <a name="next-steps"></a>다음 단계

보고 될 수 있는 일부 문제를 해결 하는 방법에 대 한 자세한 내용은 [관리 되는 도메인에서 경고 해결][troubleshoot-alerts]을 참조 하세요.

<!-- INTERNAL LINKS -->
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
