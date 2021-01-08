---
title: Azure Active Directory에서 앱에 사용자를 할당 하는 방법 이해
description: Id 관리를 위해 Azure Active Directory를 사용 하는 앱에 사용자가 할당 되는 방식을 이해 합니다.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: reference
ms.date: 01/07/2021
ms.author: kenwith
ms.openlocfilehash: aedcecf528f07d9d7f89c1d5659d8fcd2cb36df2
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98016731"
---
# <a name="understand-how-users-are-assigned-to-apps-in-azure-active-directory"></a>Azure Active Directory에서 앱에 사용자를 할당 하는 방법 이해
이 문서에서는 테넌트의 애플리케이션에 사용자가 할당되는 방법을 이해하는 데 도움을 줍니다.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Azure AD의 애플리케이션에 사용자가 할당되는 방법
사용자는 애플리케이션에 액세스하려면 어떤 방식으로든 먼저 애플리케이션에 할당되어야 합니다. 할당은 관리자, 비즈니스 대리자, 경우에 따라 사용자 자신이 수행할 수 있습니다. 아래에서는 사용자가 애플리케이션에 할당될 수 있는 방법을 설명합니다.

*  관리자가 애플리케이션에 직접 [사용자를 할당](./assign-user-or-group-access-portal.md)
*  관리자가 다음을 포함하여 해당 사용자가 애플리케이션의 구성원인 [그룹 할당](./assign-user-or-group-access-portal.md)
    * 온-프레미스에서 동기화된 그룹
    * 클라우드에서 만든 정적 보안 그룹
    * 클라우드에서 만든 [동적 보안 그룹](../enterprise-users/groups-dynamic-membership.md)
    * 클라우드에서 만든 Microsoft 365 그룹
    * [모든 사용자](../fundamentals/active-directory-groups-create-azure-portal.md) 그룹
*  관리자는 [셀프 서비스 응용 프로그램 액세스](./manage-self-service-access.md) 를 통해 사용자가 [내 앱](../user-help/my-apps-portal-end-user-access.md) 을 사용 하 여 응용 프로그램을 추가할 수 있습니다. **비즈니스 승인 없이** 앱 **추가** 기능을 사용 합니다.
*  관리자는 [셀프 서비스 응용 프로그램 액세스](./manage-self-service-access.md) 를 통해 사용자가 [내 앱](../user-help/my-apps-portal-end-user-access.md) **앱 추가** 기능을 사용 하 여 응용 프로그램을 추가할 수 있지만 **선택한 비즈니스 승인자의 사전 승인만** 사용 합니다.
*  관리자가 [셀프 서비스 그룹 관리](../enterprise-users/groups-self-service-management.md)를 통해 사용자가 **비즈니스 승인 없이** 애플리케이션이 할당된 그룹에 가입하도록 허용
*  관리자가 [셀프 서비스 그룹 관리](../enterprise-users/groups-self-service-management.md)를 통해 사용자가 **선택된 비즈니스 승인자 세트의 사전 승인이 있는 경우에만** 애플리케이션이 할당된 그룹에 가입하도록 허용
*  관리자는 [Microsoft 365](https://products.office.com/) 같은 자사 응용 프로그램에 대해 사용자에 게 직접 라이선스를 할당 합니다.
*  관리자가 사용자가 구성원 인 그룹에 대 한 라이선스를 자사 응용 프로그램에 할당 하는 것과 같은 [Microsoft 365](https://products.office.com/)
*  [관리자가 모든 사용자가 사용할 수 있게 애플리케이션에 동의](../develop/howto-convert-app-to-be-multi-tenant.md)한 다음, 사용자가 애플리케이션에 로그인
* 사용자가 애플리케이션에 로그인하여 [애플리케이션에 직접 동의](../develop/howto-convert-app-to-be-multi-tenant.md)

## <a name="next-steps"></a>다음 단계
* [애플리케이션 관리에 대한 빠른 시작 시리즈](view-applications-portal.md)
* [애플리케이션 관리란?](what-is-application-management.md)
* [Single Sign-On 이란?](what-is-single-sign-on.md)