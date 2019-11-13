---
title: 데이터 액세스 권한 부여를 위한 보안 구성-Azure Time Series Insights 미리 보기 | Microsoft Docs
description: Azure Time Series Insights 미리 보기 환경에서 보안, 권한 및 데이터 액세스 정책 관리를 구성 하는 방법에 대해 알아봅니다.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: f49567b8060be2bf2a9ca2b8a1bdee23f58fdd6b
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012680"
---
# <a name="grant-data-access-to-an-environment"></a>환경에 대한 데이터 액세스 권한 부여

이 문서에서는 두 가지 유형의 Azure Time Series Insights 미리 보기 액세스 정책에 대해 설명합니다.

## <a name="sign-in-to-time-series-insights"></a>Time Series Insights에 로그인

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
1. Time Series Insights 환경을 찾습니다. `Time Series`검색**상자에**를 입력합니다. 검색 결과에서 **시계열 환경**을 선택합니다.
1. 목록에서 Time Series Insights 환경을 선택합니다.

## <a name="grant-data-access"></a>데이터 액세스 권한 부여

다음 단계에 따라 사용자 계정에 대해 데이터 액세스 권한을 부여합니다.

1. **데이터 액세스 정책**을 선택한 다음, **+ 추가**를 선택합니다.

    [![Data-access-one](media/data-access/data-access-one.png)](media/data-access/data-access-one.png#lightbox)

1. **사용자 선택**을 선택합니다. 사용자 이름 또는 메일 주소를 검색하여 추가할 사용자를 찾습니다. 선택 **을 선택 하 여** 선택 내용을 확인 합니다.

    [![Data-access-two](media/data-access/data-access-two.png)](media/data-access/data-access-two.png#lightbox)

1. **역할 선택**을 선택합니다. 사용자에 대한 적절한 액세스 역할을 선택합니다.

    * 사용자가 참조 데이터를 변경하고 저장된 쿼리 및 큐브 뷰를 환경의 다른 사용자와 공유할 수 있도록 하려면 **참가자**를 선택합니다.

    * 또는 사용자가 환경의 데이터를 쿼리하고 공유되지 않는 개인 쿼리를 환경에 저장할 수 있게 허용하려면 **읽기 권한자**를 선택합니다.

   **확인**을 선택하여 역할 선택을 확인합니다.

    [![Data-access-three](media/data-access/data-access-three.png)](media/data-access/data-access-three.png#lightbox)

1. **사용자 역할 선택** 페이지에서 **확인**을 선택합니다.

    [![Data-access-four](media/data-access/data-access-four.png)](media/data-access/data-access-four.png#lightbox)

1. **데이터 액세스 정책** 페이지에 사용자 및 각 사용자의 역할이 표시되는지 확인합니다.

    [![Data-access-five](media/data-access/data-access-five.png)](media/data-access/data-access-five.png#lightbox)

## <a name="provide-guest-access-from-another-aad-tenant"></a>다른 AAD 테 넌 트에서 게스트 액세스 제공

`Guest`는 관리 역할이 아니라 특정 테넌트에서 다른 테넌트로 초대된 계정에 사용되는 용어입니다. 게스트 계정을 테넌트의 디렉터리에 초대한 후에는 다른 계정과 동일한 액세스 제어가 적용될 수 있습니다. 액세스 제어(IAM) 블레이드를 사용하여 Time Series Insights 환경에 대한 관리 액세스 권한을 부여할 수 있습니다. 또는 데이터 액세스 정책 블레이드를 통해 환경의 데이터에 대한 액세스 권한을 부여할 수 있습니다. Azure AD(Azure Active Directory) 테넌트 게스트 액세스에 대한 자세한 내용은 [Azure Portal에서 Azure Active Directory B2B 협업 사용자 추가](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator)를 참조하세요.

다른 테넌트의 Azure AD 사용자에게 Time Series Insights 환경에 대한 게스트 액세스 권한을 부여하려면 다음 단계를 수행합니다.

1. **데이터 액세스 정책**을 선택한 다음, **+ 초대**를 선택합니다.

    [![Data-access-six](media/data-access/data-access-six.png)](media/data-access/data-access-six.png#lightbox)

1. 초대할 사용자의 메일 주소를 입력합니다. 이 메일 주소는 Azure AD와 연결되어야 합니다. 필요한 경우 초대에 개인 메시지를 포함할 수 있습니다.

    [![Data-access-seven](media/data-access/data-access-seven.png)](media/data-access/data-access-seven.png#lightbox)

1. 화면에 표시되는 확인 거품을 찾습니다.

    [![Data-access-eight](media/data-access/data-access-eight.png)](media/data-access/data-access-eight.png#lightbox)

1. **사용자 선택**을 선택합니다. 초대한 게스트 사용자의 메일 주소를 검색하여 추가할 사용자를 찾습니다. 그런 다음 **선택** 항목을 확인 하려면 선택 합니다.

    [![Data-access-nine](media/data-access/data-access-nine.png)](media/data-access/data-access-nine.png#lightbox)

1. **역할 선택**을 선택합니다. 게스트 사용자의 적절한 액세스 역할을 선택합니다.

    * 사용자가 참조 데이터를 변경하고 저장된 쿼리 및 큐브 뷰를 환경의 다른 사용자와 공유할 수 있도록 하려면 **참가자**를 선택합니다.

    * 또는 사용자가 환경의 데이터를 쿼리하고 공유되지 않는 개인 쿼리를 환경에 저장할 수 있게 허용하려면 **읽기 권한자**를 선택합니다.

   **확인**을 선택하여 역할 선택을 확인합니다.

    [![Data-access-ten](media/data-access/data-access-ten.png)](media/data-access/data-access-ten.png#lightbox)

1. **사용자 역할 선택** 페이지에서 **확인**을 선택합니다.

1. **데이터 액세스 정책** 페이지에 게스트 사용자 및 각 게스트 사용자의 역할이 표시되는지 확인합니다.

    [![Data-access-eleven](media/data-access/data-access-eleven.png)](media/data-access/data-access-eleven.png#lightbox)

1. 이제 게스트 사용자가 초대를 받은 Azure 테넌트에 있는 환경에 액세스하기 위한 단계를 따라야 합니다. 먼저 전송된 초대를 수락합니다. 이 초대는 5단계에서 사용된 메일 주소로 메일을 통해 전송됩니다. **시작**을 선택하여 수락합니다.

    [![Data-access-twelve](media/data-access/data-access-twelve.png)](media/data-access/data-access-twelve.png#lightbox)

1. 다음으로, 게스트 사용자가 관리자 조직과 연결된 사용 권한을 수락합니다.

    [![Data-access-thirteen](media/data-access/data-access-thirteen.png)](media/data-access/data-access-thirteen.png#lightbox)

1. 초대하는 데 사용된 메일 주소에 게스트 사용자가 로그인하고 초대를 수락하면 insights.azure.com으로 이동됩니다. 이 페이지에서 화면 오른쪽 위의 메일 주소 옆에 있는 아바타를 선택합니다.

    [![데이터-액세스-14](media/data-access/data-access-fourteen.png)](media/data-access/data-access-fourteen.png#lightbox)

1. 다음으로, 게스트 사용자가 디렉터리 드롭다운 메뉴에서 Azure 테넌트를 선택합니다. 이 테넌트는 초대 받은 테넌트입니다.

    [![Data-access-fifteen](media/data-access/data-access-fifteen.png)](media/data-access/data-access-fifteen.png#lightbox)

게스트 사용자가 테넌트를 선택하면 액세스 권한이 부여된 Time Series Insights 환경이 표시됩니다. 이제 **5 단계**에서 제공 하는 역할과 관련 된 모든 기능을 제공 합니다.

## <a name="next-steps"></a>다음 단계

* Time Series Insights 환경에 [Azure Event Hubs 이벤트 원본을 추가하는 방법](./time-series-insights-how-to-add-an-event-source-eventhub.md)을 알아봅니다.

* [이벤트 원본으로 이벤트를 전송](./time-series-insights-send-events.md)합니다.

* [Time Series Insights 미리 보기 탐색기에서 사용자 환경을 봅니다](./time-series-insights-update-explorer.md).
