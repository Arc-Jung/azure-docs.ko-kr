---
title: 애플리케이션에 대한 사용자 액세스를 제거하는 방법 | Microsoft Docs
description: 애플리케이션에 대한 사용자 액세스를 제거하는 방법 이해
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/17/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f9626c256755e2fce81b593d95b8680f4bb55ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84763162"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>애플리케이션에 대한 사용자 액세스를 제거하는 방법

이 문서는 애플리케이션에 대한 사용자 액세스를 제거하는 방법을 이해하는 데 도움이 됩니다.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>애플리케이션에 대한 특정 사용자 또는 그룹의 할당을 제거하려는 경우

애플리케이션에 대한 사용자 또는 그룹 할당을 제거하려면 [Azure Active Directory의 엔터프라이즈 앱에서 사용자 또는 그룹 할당 제거](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) 문서에 나열된 단계를 따릅니다.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>모든 사용자에 대해 애플리케이션에 대한 모든 액세스를 비활성화하려는 경우

애플리케이션에 대한 모든 사용자 로그인을 비활성화하려면 [Azure Active Directory의 엔터프라이즈 앱에 대한 사용자 로그인 비활성화](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) 문서에 나열된 단계를 따릅니다.

## <a name="i-want-to-delete-an-application-entirely"></a>애플리케이션을 완전히 삭제하려는 경우

**애플리케이션을 삭제**하려면 다음 지침을 따릅니다.

1. [**Azure Portal**](https://portal.azure.com/) 을 열고 **전역 관리자** 또는 공동 관리자 권한으로 로그인 **합니다.**

2. 왼쪽 주 탐색 메뉴의 맨 위에서 **모든 서비스**를 클릭하여 **Azure Active Directory 확장**을 엽니다.

3. 필터 검색 상자에 **“Azure Active Directory**”를 입력하고 **Azure Active Directory** 항목을 선택합니다.

4. Azure Active Directory 왼쪽 탐색 메뉴에서 **엔터프라이즈 응용 프로그램** 을 클릭 합니다.

5. 모든 **응용 프로그램을 클릭 하** 여 모든 응용 프로그램의 목록을 봅니다.

   * 여기에 표시하려는 애플리케이션이 표시되지 않으면 **모든 애플리케이션 목록**의 맨 위에서 **필터** 컨트롤을 사용하고 **표시** 옵션을 **모든 애플리케이션**으로 설정합니다.

6. 삭제하려는 애플리케이션을 선택합니다.

7. 애플리케이션이 로드되면 맨 위의 애플리케이션 **개요** 창에서 **삭제** 아이콘을 클릭합니다.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>모든 애플리케이션에 대한 모든 이후 사용자 동의 작업을 비활성화하려는 경우

전체 디렉터리에 대한 사용자 동의를 비활성화하면 모든 애플리케이션에 대한 최종 사용자 동의를 방지합니다. 관리자는 사용자를 대신 하 여 동의할 수 있습니다. 애플리케이션 동의 및 이 작업을 수행하거나 수행하지 않을 수 있는 이유에 대한 자세한 내용은 [사용자 및 관리자 동의 이해](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)를 참조하세요. [권한 및 동의](../develop/v2-permissions-and-consent.md)도 참조 하세요.

**전체 디렉터리에서 모든 이후 사용자 동의 작업을 비활성화**하려면 다음 지침을 따릅니다.

1.  [**Azure Portal**](https://portal.azure.com/)을 열고 **전역 관리자** 권한으로 로그인합니다.

2.  **Azure Active Directory 확장**을 엽니다. 

3.  탐색 메뉴에서 **엔터프라이즈 애플리케이션**을 클릭합니다.

5.  **사용자 설정**을 클릭합니다.

6.  **사용자는 앱이 자신을 대신하여 회사 데이터에 액세스하도록 허용할 수 있습니다** 토글을 **아니요**로 설정하고, [저장] 단추를 클릭합니다.


## <a name="next-steps"></a>다음 단계

[앱에 대한 액세스 관리](what-is-access-management.md)
