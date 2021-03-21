---
title: B2B 협업을 위한 셀프 서비스 등록 포털 - Azure AD
description: 조직의 요구 사항에 맞게 Azure Active Directory B2B 사용자에 대 한 온 보 딩 워크플로를 사용자 지정 하는 방법을 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a5b800e78448afcc970010535ba12b543d3cc74
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96860510"
---
# <a name="self-service-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B 공동 작업 등록을 위한 셀프 서비스

고객은 최종 사용자를 위한 [Azure Portal](https://portal.azure.com) 및 [애플리케이션 액세스 패널](https://myapps.microsoft.com)을 통해 제공되는 기본 제공 기능으로 많은 작업을 수행할 수 있습니다. 하지만 B2B 사용자가 조직의 요구에 맞출 수 있도록 온보딩 워크플로를 사용자 지정해야 할 수도 있습니다.

## <a name="azure-ad-entitlement-management-for-b2b-guest-user-sign-up"></a>B2B 게스트 사용자 등록을 위한 Azure AD 자격 관리

초대 하는 조직에서는 개별 외부 협력자가 리소스에 액세스 해야 하는 사람을 미리 알 수 없습니다. 파트너 회사의 사용자가 제어 하는 정책에 따라 자신에 게 로그인 할 수 있는 방법이 필요 합니다. 다른 조직의 사용자가 액세스를 요청할 수 있도록 하 고, 승인이 게스트 계정으로 프로 비전 되 고 그룹, 앱 및 SharePoint Online 사이트에 할당 된 경우 [AZURE AD 자격 관리](../governance/entitlement-management-overview.md) 를 사용 하 여 [외부 사용자에 대 한 액세스를 관리](../governance/entitlement-management-external-users.md#how-access-works-for-external-users)하는 정책을 구성할 수 있습니다.

## <a name="azure-active-directory-b2b-invitation-api"></a>Azure Active Directory B2B 초대 API

조직에서는 [Microsoft Graph 초대 관리자 API](/graph/api/resources/invitation) 를 사용 하 여 B2B 게스트 사용자에 대 한 고유한 온 보 딩 환경을 만들 수 있습니다. 셀프 서비스 B2B 게스트 사용자 등록을 제공 하려는 경우 [AZURE AD 자격 관리](../governance/entitlement-management-overview.md)를 사용 하는 것이 좋습니다. 그러나 사용자 고유의 환경을 구축 하려는 경우 [초대 만들기 API](/graph/api/invitation-post?tabs=http) 를 사용 하 여 사용자 지정 된 초대 전자 메일을 B2B 사용자에 게 직접 자동으로 보낼 수 있습니다 (예:). 또는 앱이 생성 응답에서 반환 된 inviteRedeemUrl를 사용 하 여 초대 된 사용자에 게 원하는 통신 메커니즘을 통해 자신의 초대를 만들 수 있습니다.

## <a name="next-steps"></a>다음 단계

* [Azure AD B2B 협업이란?](what-is-b2b.md)
* [외부 ID 가격](external-identities-pricing.md)
* [Azure Active Directory B2B 협업 자주 묻는 질문(FAQ)](faq.md)
