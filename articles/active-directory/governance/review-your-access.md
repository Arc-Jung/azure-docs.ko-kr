---
title: 액세스 검토에서 앱 & 그룹에 대 한 액세스 검토-Azure AD
description: Azure Active Directory 액세스 검토에서 그룹 또는 응용 프로그램에 대 한 자신의 액세스를 검토 하는 방법에 대해 알아봅니다.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 12/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3fef2f85ca7e7b4034c8582477796d49446ea44
ms.sourcegitcommit: 6e2d37afd50ec5ee148f98f2325943bafb2f4993
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97746782"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Azure AD 액세스 검토에서 그룹 또는 응용 프로그램에 대 한 액세스를 검토 합니다.

Azure Active Directory (Azure AD)는 기업이 azure ad 액세스 검토 라는 기능을 사용 하 여 Azure AD 및 기타 Microsoft Online Services의 그룹 또는 응용 프로그램에 대 한 액세스를 관리 하는 방법을 간소화 합니다.

이 문서에서는 그룹 또는 응용 프로그램에 대 한 자신의 액세스를 검토 하는 방법을 설명 합니다.

## <a name="review-your-access-using-my-apps"></a>내 앱을 사용 하 여 액세스 검토

액세스 검토를 수행 하는 첫 번째 단계는 액세스 검토를 찾아서 여는 것입니다.

>[!IMPORTANT]
> 전자 메일을 받을 때 지연이 발생할 수 있으며, 일부 경우에는 최대 24 시간이 걸릴 수 있습니다. 받는 azure-noreply@microsoft.com 사람 목록에를 추가 하 여 모든 전자 메일을 수신 하 고 있는지 확인 합니다.

1. Microsoft에서 액세스를 검토 하도록 요청 하는 전자 메일을 찾습니다. 그룹에 대 한 액세스를 검토 하는 예제 메일은 다음과 같습니다.

    ![그룹에 대 한 액세스를 검토 하는 Microsoft의 예제 메일](./media/review-your-access/access-review-email.png)

1. **액세스 검토 링크를** 클릭 하 여 액세스 검토를 엽니다.

전자 메일이 없는 경우 다음 단계를 수행 하 여 보류 중인 액세스 검토를 찾을 수 있습니다.

1. 에서 내 앱 포털에 로그인 [https://myapps.microsoft.com](https://myapps.microsoft.com) 합니다.

    ![사용 권한이 있는 앱을 나열 하는 내 앱 포털](./media/review-your-access/myapps-access-panel.png)

1. 페이지의 오른쪽 위 모서리에 있는 사용자 기호를 클릭하면 이름과 기본 조직이 표시됩니다. 둘 이상의 조직이 나열되는 경우 액세스 검토를 요청한 조직을 선택합니다.

1. 페이지의 오른쪽에서 **액세스 검토** 타일을 클릭 하 여 보류 중인 액세스 검토의 목록을 확인 합니다.

    타일이 표시되지 않은 경우 해당 조직에 대해 수행할 액세스 검토가 없으므로 이 시점에서는 어떤 작업도 필요하지 않습니다.

    ![앱 및 그룹에 대 한 보류 중인 액세스 검토 목록](./media/review-your-access/access-reviews-list.png)

1. 수행 하려는 액세스 검토에 대 한 **검토 시작** 링크를 클릭 합니다.

### <a name="perform-the-access-review"></a>액세스 검토 수행

액세스 검토를 열면 액세스를 볼 수 있습니다.

1. 액세스를 검토 하 고 여전히 액세스 권한이 필요한 지 여부를 결정 합니다.

    다른 사용자에 대 한 액세스를 검토 하도록 요청 하는 경우 페이지가 다르게 표시 됩니다. 자세한 내용은 [그룹 또는 응용 프로그램에 대 한 액세스 검토](perform-access-review.md)를 참조 하세요.

    ![그룹에 대 한 액세스 권한이 여전히 필요한 지 여부를 묻는 오픈 액세스 검토를 보여 주는 스크린샷](./media/review-your-access/perform-access-review.png)

1. 액세스를 유지 하려면 **예** 를 클릭 하 고, 액세스를 제거 하려면 **아니요** 를 클릭 합니다.

1. **예** 를 클릭 하면 **이유** 상자에 근거를 지정 해야 할 수 있습니다.

    !["예"를 선택 하 여 그룹에 계속 액세스 해야 하는지 여부를 묻는 완료 된 액세스 검토를 보여 주는 스크린샷](./media/review-your-access/perform-access-review-submit.png)

1. **제출** 을 클릭합니다.

    선택 항목이 제출 되 고 내 앱 포털로 돌아갑니다.

    응답을 변경 하려면 액세스 검토 페이지를 다시 열고 응답을 업데이트 합니다. 액세스 검토가 종료 될 때까지 언제 든 지 응답을 변경할 수 있습니다.

    > [!NOTE]
    > 더 이상 액세스할 필요가 없는 것으로 표시 되 면 즉시 제거 되지 않습니다. 검토가 종료 되거나 관리자가 검토를 중지 하면 제거 됩니다.

## <a name="review-your-own-access-using-my-access-new"></a>내 액세스 (신규)를 사용 하 여 자신의 액세스를 검토 합니다.

업데이트 된 사용자 인터페이스로 새로운 환경을 사용해 볼 수 있습니다.

### <a name="my-apps-portal"></a>내 앱 포털

1. 에서 내 앱 포털에 로그인 [https://myapps.microsoft.com](https://myapps.microsoft.com) 합니다.

    ![사용 권한이 있는 앱을 나열 하는 내 앱 포털](./media/review-your-access/myapps-access-panel.png)

2. **액세스 검토** 타일을 클릭 하 여 보류 중인 액세스 검토 목록을 표시 합니다.

    > [!NOTE]
    > **액세스 검토** 타일이 표시 되지 않는 경우 해당 조직에 대해 수행할 액세스 검토가 없으며 지금은 아무런 조치도 필요 하지 않습니다.

3. **시도** 를 클릭 합니다. 페이지 맨 위에 있는 배너에서 새 내 액세스 환경으로 이동 합니다.

    ![미리 보기 중에 표시 되는 새로운 경험을 사용 하 여 앱 및 그룹에 대 한 보류 중인 액세스 검토 목록](./media/review-your-access/banner-your-access.png)

4. **액세스 검토 수행** 섹션에서 계속 진행 합니다.

### <a name="email"></a>메일

>[!IMPORTANT]
> 전자 메일을 받을 때 지연이 발생할 수 있으며, 일부 경우에는 최대 24 시간이 걸릴 수 있습니다. 받는 azure-noreply@microsoft.com 사람 목록에를 추가 하 여 모든 전자 메일을 수신 하 고 있는지 확인 합니다.

1. Microsoft에서 액세스를 검토 하도록 요청 하는 전자 메일을 찾습니다. 아래 예제 메일 메시지를 확인할 수 있습니다.

 ![그룹에 대 한 액세스를 검토 하기 위한 Microsoft의 예제 메일](./media/review-your-access/access-review-email-preview.png)

2. **액세스 검토 링크를** 클릭 하 여 액세스 검토를 엽니다.

3. **액세스 검토 수행** 섹션에서 계속 진행 합니다.

>[!NOTE]
>시작을 클릭 하 여 **내 앱** 으로 이동 하는 경우 **내 앱 포털** 아래 섹션에 나열 된 단계를 수행 합니다.

### <a name="directly-at-my-access"></a>직접 액세스

브라우저를 사용 하 여 내 액세스를 열어 보류 중인 액세스 검토를 볼 수도 있습니다.

1. 에서 내 액세스에 로그인 https://myaccess.microsoft.com/

2. 왼쪽 막대의 메뉴에서 **액세스 검토** 를 선택 하 여 사용자에 게 할당 된 보류 중인 액세스 검토 목록을 표시 합니다.

   ![메뉴의 액세스 검토](./media/review-your-access/access-review-menu.png)

### <a name="perform-the-access-review"></a>액세스 검토 수행

1. 그룹 및 앱 아래에서 볼 수 있습니다.
    
    - **이름** 액세스 검토의 이름입니다.
    - **기한** 검토의 기한입니다. 이 날짜를 거부 한 후에는 검토 중인 그룹 또는 앱에서 사용자를 제거할 수 있습니다.
    - **리소스** 검토 중인 리소스의 이름입니다.
    - **진행률** 이 액세스 검토의 총 사용자 수에 대해 검토 한 사용자 수입니다.
    
2. 액세스 검토의 이름을 클릭 하 여 시작 합니다.

   ![앱 및 그룹에 대 한 보류 중인 액세스 검토 목록](./media/review-your-access/access-reviews-list-preview.png)

3. 액세스를 검토 하 고 여전히 액세스 권한이 필요한 지 여부를 결정 합니다.

    다른 사용자에 대 한 액세스를 검토 하도록 요청 하는 경우 페이지가 다르게 표시 됩니다. 자세한 내용은 [그룹 또는 응용 프로그램에 대 한 액세스 검토](perform-access-review.md)를 참조 하세요.

    ![그룹에 대 한 액세스가 필요한 지 여부를 묻는 액세스 검토를 엽니다.](./media/review-your-access/review-access-preview.png)

1. 액세스를 유지 하려면 **예** 를 선택 하 고, 액세스를 제거 하려면 **아니요** 를 선택 합니다.

1. **예** 를 클릭 하면 **이유** 상자에 근거를 지정 해야 할 수 있습니다.

    ![그룹에 대 한 액세스가 필요한 지 여부를 묻는 액세스 검토 완료](./media/review-your-access/review-access-yes-preview.png)

1. **제출** 을 클릭합니다.

    선택 항목이 제출 되 고 내 액세스 페이지로 돌아갑니다.

    응답을 변경 하려면 액세스 검토 페이지를 다시 열고 응답을 업데이트 합니다. 액세스 검토가 종료 될 때까지 언제 든 지 응답을 변경할 수 있습니다.

    > [!NOTE]
    > 더 이상 액세스할 필요가 없는 것으로 표시 되 면 즉시 제거 되지 않습니다. 검토가 종료 되거나 관리자가 검토를 중지 하면 제거 됩니다.

## <a name="next-steps"></a>다음 단계

- [그룹 또는 애플리케이션의 액세스 검토 완료](complete-access-review.md)
