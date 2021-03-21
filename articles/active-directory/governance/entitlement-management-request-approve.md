---
title: 액세스 요청 승인 또는 거부-Azure AD 자격 관리
description: 내 액세스 포털을 사용 하 여 Azure Active Directory 자격 관리에서 액세스 패키지에 대 한 요청을 승인 또는 거부 하는 방법을 알아봅니다.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: fddb3b171e5a26273cb2e0045f11e3a4dbb48c5f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97655104"
---
# <a name="approve-or-deny-access-requests-in-azure-ad-entitlement-management"></a>Azure AD 자격 관리에서 액세스 요청 승인 또는 거부

Azure AD 자격 관리를 사용 하 여 액세스 패키지에 대 한 승인을 요구 하는 정책을 구성 하 고 하나 이상의 승인자를 선택할 수 있습니다. 이 문서에서는 지정 된 승인자가 액세스 패키지에 대 한 요청을 승인 또는 거부 하는 방법을 설명 합니다.

## <a name="open-request"></a>요청 열기

액세스 요청을 승인 하거나 거부 하는 첫 번째 단계는 승인 보류 중인 액세스 요청을 찾아서 여는 것입니다. 액세스 요청을 여는 방법에는 두 가지가 있습니다.

**필수 역할:** 명의

1. 요청을 승인 또는 거부 하도록 요청 하는 Microsoft Azure에서 전자 메일을 찾습니다. 전자 메일 예제는 다음과 같습니다.

    ![패키지 전자 메일에 액세스 하는 요청 승인](./media/entitlement-management-shared/approver-request-email.png)

1. **요청 승인 또는 거부** 링크를 클릭 하 여 액세스 요청을 엽니다.

1. 내 액세스 포털에 로그인 합니다.

전자 메일이 없는 경우 다음 단계에 따라 승인 보류 중인 액세스 요청을 찾을 수 있습니다.

1. 에서 내 액세스 포털에 로그인 [https://myaccess.microsoft.com](https://myaccess.microsoft.com) 합니다.  (미국 정부의 경우 내 액세스 포털 링크의 도메인은 `myaccess.microsoft.us` 입니다.)

1. 왼쪽 메뉴 **에서 승인을 클릭 하** 여 승인 보류 중인 액세스 요청 목록을 확인 합니다.

1. **보류 중** 탭에서 요청을 찾습니다.

## <a name="view-requestors-answers-to-questions-preview"></a>요청자의 질문에 대 한 답변 보기 (미리 보기)

1. 내 액세스에서 **승인** 탭으로 이동 합니다.

1. 승인 하려는 요청으로 이동 하 고 **세부 정보** 를 클릭 합니다. 결정을 내릴 준비가 되었으면 **승인** 또는 **거부** 를 클릭할 수도 있습니다.

1. **요청 정보** 를 클릭 합니다.

    ![내 액세스 포털-액세스 요청-클릭 요청 세부 정보](./media/entitlement-management-request-approve/requestor-information-request-details.png)

1. 요청자에 의해 제공 되는 정보는 패널의 맨 아래에 있습니다.

    ![요청에 대 한 세부 정보를 보여 주는 스크린샷](./media/entitlement-management-request-approve/requestor-information-requestor-answers.png)

1. 요청자는 제공 된 정보에 따라 요청을 승인 하거나 거부할 수 있습니다. 지침은 요청 승인 또는 거부의 단계를 참조 하세요.

## <a name="approve-or-deny-request"></a>요청 승인 또는 거부

승인 보류 중인 액세스 요청을 열면 승인 또는 거부 결정을 내리는 데 도움이 되는 세부 정보를 볼 수 있습니다.

**필수 역할:** 명의

1. **보기** 링크를 클릭하여 액세스 요청 창을 엽니다.

1. 액세스 요청에 대 한 세부 정보를 보려면 **세부 정보** 를 클릭 합니다.

    세부 정보에는 사용자 이름, 조직, 액세스 시작 날짜 및 종료 날짜, 제공 된 경우 요청 제출 시, 요청이 만료 되는 시간 등이 포함 됩니다.

1. **승인** 또는 **거부** 를 클릭 합니다.

1. 필요한 경우 이유를 입력 합니다.

    ![요청을 수락 하거나 거부 하는 페이지가 스크린샷 화면에 표시 됩니다.](./media/entitlement-management-request-approve/my-access-approve-request.png)

1. **제출** 을 클릭하여 결정을 제출합니다.

    정책이 여러 승인자를 사용 하 여 구성 된 경우 하나의 승인자만 보류 중인 승인에 대해 결정을 내려야 합니다. 승인자가 액세스 요청에 대 한 결정을 제출한 후 요청이 완료 되 고 다른 승인자가 요청을 승인 하거나 거부 하는 데 더 이상 사용할 수 없습니다. 다른 승인자는 내 액세스 포털에서 요청 결정 및 의사 결정자를 볼 수 있습니다. 이번에는 단일 단계 승인만 지원 됩니다.

    구성 된 승인자가 액세스 요청을 승인 하거나 거부할 수 없는 경우 요청은 구성 된 요청 기간 후에 만료 됩니다. 사용자에 게 액세스 요청이 만료 되었으며 액세스 요청을 다시 제출 해야 한다는 알림이 표시 됩니다.

## <a name="next-steps"></a>다음 단계

- [액세스 패키지에 대 한 액세스 요청](entitlement-management-request-access.md)
- [요청 프로세스 및 전자 메일 알림](entitlement-management-process.md)
