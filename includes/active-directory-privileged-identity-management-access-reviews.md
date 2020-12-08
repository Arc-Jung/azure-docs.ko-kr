---
title: 포함 파일
description: 포함 파일
services: active-directory
author: barclayn
ms.service: active-directory
ms.topic: include
ms.date: 12/07/2020
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: cbcd4b459faa3bf67f591cc7afab0bf0027062e1
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96842297"
---
## <a name="create-one-or-more-access-reviews"></a>하나 이상의 액세스 검토 만들기

1. 새 액세스 검토를 만들려면 **새로 만들기** 를 클릭합니다.

1. 액세스 검토의 이름을 지정합니다. 필요한 경우 검토에 설명을 지정합니다. 이름 및 설명이 검토자에게 표시됩니다.

    ![액세스 검토 만들기 - 이름 및 설명 검토](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. **시작 날짜** 를 설정합니다. 기본적으로 액세스 검토는 한 번 발생하고, 만들어진 시간에 시작되고, 1개월 후에 종료됩니다. 시작 날짜와 종료 날짜를 변경하여 액세스 검토를 나중에 시작하고 원하는 기간(일 수) 동안 지속할 수 있습니다.

    ![시작 날짜, 빈도, 기간, 종료, 횟수 및 종료 날짜](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. 액세스 검토를 반복하려면 **빈도** 설정을 **한 번** 에서 **매주**, **매월**, **분기마다**, **매년** 또는 **반년마다** 로 변경합니다. **기간** 슬라이더 또는 텍스트 상자를 사용하여 검토자의 입력에 대해 반복 시리즈의 각 검토가 열릴 일수를 정의합니다. 예를 들어 검토가 겹치는 상황을 방지하기 위해 월별 검토에 대해 설정할 수 있는 최대 기간은 27일입니다.

1. **종료** 설정을 사용하여 되풀이 액세스 검토 시리즈를 종료하는 방법을 지정합니다. 이 시리즈는 세 가지 방법으로 종료할 수 있습니다. 무기한으로, 특정 날짜까지, 또는 정의된 되풀이 횟수가 완료된 이후에 검토를 시작하도록 연속적으로 실행됩니다. 사용자, 다른 사용자 관리자 또는 다른 전역 관리자는 해당 날짜에 종료되도록 **설정** 에서 날짜를 변경하여 생성 후 시리즈를 중지할 수 있습니다.

1. **사용자** 섹션에서 멤버 자격을 검토할 역할을 하나 이상 선택합니다.

    ![역할 멤버 자격을 검토할 사용자 범위](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > - 여기에서 선택한 역할에는 [영구적 역할 및 적격 역할](../articles/active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)이 모두 포함됩니다.
    > - 둘 이상의 역할을 선택하면 여러 액세스 검토가 생성됩니다. 예를 들어 5개의 역할을 선택하면 별도의 액세스 검토가 5개 생성됩니다.

    **Azure AD 역할** 의 액세스 검토를 만드는 경우 다음은 멤버 자격 검토 목록 예제를 보여 줍니다.

    ![선택할 수 있는 Azure AD 역할을 나열하는 멤버 자격 검토 창](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    **Azure 리소스 역할** 의 액세스 검토를 만드는 경우 다음 이미지는 멤버 자격 검토 목록 예제를 보여 줍니다.

    ![선택할 수 있는 Azure 리소스 역할을 나열하는 멤버 자격 검토 창](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. **검토자** 섹션에서 모든 사용자를 검토할 한 명 이상의 사용자를 선택합니다. 또는 구성원이 자신의 액세스 권한을 검토하도록 할 수도 있습니다.

    ![선택한 사용자 또는 멤버(자신)의 검토자 목록](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **선택한 사용자** - 액세스가 필요한 사용자를 알 수 없는 경우 이 옵션을 사용합니다. 이 옵션을 사용하여 리소스 소유자 또는 관리자 그룹에게 검토를 완료하도록 할당할 수 있습니다.
    - **멤버(자신)** - 사용자에게 자신의 역할 할당을 검토하게 하려면 이 옵션을 사용합니다.
    - **(미리 보기) 관리자** – 사용자의 관리자가 해당 역할 할당을 검토 하도록 하려면이 옵션을 사용 합니다. (미리 보기) 관리자를 선택 하면 대체 검토자를 지정 하는 옵션도 제공 됩니다. 사용자가 디렉터리에 관리자를 지정 하지 않은 경우 대체 검토자에 게 사용자를 검토 하 라는 메시지가 표시 됩니다.

### <a name="upon-completion-settings"></a>완료 시 설정

1. 검토가 완료된 후 수행할 작업을 지정하려면 **완료 시 설정** 섹션을 확장합니다.

    ![자동 응답 및 검토자가 응답하지 않을 경우에 대한 완료 시 설정](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. 거부된 사용자에 대한 액세스를 자동으로 제거하려면 **결과를 리소스에 자동 적용** 을 **사용** 으로 설정합니다. 검토가 완료될 때 결과를 수동으로 적용하려면 스위치를 **사용 안 함** 으로 설정합니다.

1. **검토자가 응답하지 않는 경우** 목록을 사용하여 검토 기간 내에 검토자가 검토하지 않은 사용자를 어떻게 할 것인지 지정합니다. 이 설정은 검토자가 수동으로 검토한 사용자에게 영향을 주지 않습니다. 최종 검토자의 결정이 [거부]이면 사용자의 액세스 권한이 제거됩니다.

    - **변경 없음** - 사용자의 액세스 권한을 그대로 유지
    - **액세스 권한 제거** - 사용자의 액세스 권한을 제거
    - **액세스 권한 승인** - 사용자의 액세스 권한을 승인
    - **권장 작업 수행** - 사용자의 지속적인 액세스를 거부할 것인지 아니면 승인할 것인지에 대한 시스템의 권장 사항을 수용

### <a name="advanced-settings"></a>고급 설정

1. 추가 설정을 지정하려면 **고급 설정** 섹션을 확장합니다.

    ![권장 사항 표시, 승인 이유, 메일 알림 및 미리 알림에 대한 고급 설정](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. **권장 사항 표시** 를 **사용** 으로 설정하면 사용자의 액세스 정보에 따라 검토자에게 시스템 권장 사항이 표시됩니다.

1. **승인 시 이유 필요** 를 **사용** 으로 설정하면 검토자가 승인 이유를 입력해야 합니다.

1. **메일 알림** 을 **사용** 으로 설정하면 Azure AD는 액세스 검토가 시작될 때 검토자에게 이메일 알림을 보내고 검토가 완료될 때 관리자에게 이메일 알림을 보냅니다.

1. **미리 알림** 을 **사용** 으로 설정하면 Azure AD는 검토를 완료하지 않은 검토자에게 진행 중인 액세스 검토에 대한 미리 알림을 보냅니다.
1. 검토자에 게 보내는 전자 메일의 내용은 검토 이름, 리소스 이름, 기한 등의 검토 세부 정보를 기반으로 자동 생성 됩니다. 추가 지침이 나 연락처 정보와 같은 추가 정보를 전달 하는 방법이 필요한 경우 할당 된 검토자에 게 전송 되는 초대 및 미리 알림 전자 메일에 포함 되는 **검토자의 추가 내용 전자 메일** 에 이러한 세부 정보를 지정할 수 있습니다. 아래 강조 표시 된 섹션은이 정보가 표시 되는 위치입니다.

    ![강조 표시를 사용 하 여 검토자에 게 보낸 전자 메일의 콘텐츠](./media/active-directory-privileged-identity-management-access-reviews/email-info.png)