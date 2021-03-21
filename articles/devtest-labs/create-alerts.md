---
title: Azure DevTest Labs에서 랩을 위한 활동 로그 경고 만들기
description: 이 문서에서는 Azure DevTest Labs 랩에 대 한 활동 로그 경고를 만드는 단계를 제공 합니다.
ms.topic: how-to
ms.date: 07/10/2020
ms.openlocfilehash: f774e3291961c58f55a9ed24026535e076235b98
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100588766"
---
# <a name="create-activity-log-alerts-for-labs-in-azure-devtest-labs"></a>Azure DevTest Labs에서 랩을 위한 활동 로그 경고 만들기
이 문서에서는 Azure DevTest Labs에서 랩에 대 한 활동 로그 경고를 만드는 방법을 설명 합니다 (예: VM을 만들 때 또는 VM이 삭제 될 때).

## <a name="create-alerts"></a>경고 만들기
이 예에서는 구독 소유자에 게 전자 메일을 보내는 작업을 사용 하 여 랩에 대 한 모든 관리 작업에 대 한 경고를 만듭니다. 

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. Azure Portal 검색 창에서 **monitor** 를 입력 하 고 결과 목록에서 **모니터** 를 선택 합니다. 

    :::image type="content" source="./media/activity-logs/search-monitor.png" alt-text="모니터 검색":::        
1. 왼쪽 메뉴에서 **경고** 를 선택한 다음 도구 모음에서 **새 경고 규칙** 을 선택 합니다. 

    :::image type="content" source="./media/activity-logs/alerts-page.png" alt-text="경고 페이지":::    
1. **경고 규칙 만들기** 페이지에서 **리소스 선택** 을 클릭 합니다. 

    :::image type="content" source="./media/activity-logs/select-resource-link.png" alt-text="경고에 대 한 리소스를 선택 합니다.":::        
1. **리소스 유형별 필터링** 에 대해 **DevTest Labs** 를 선택 하 고 목록에서 랩을 선택한 후 **완료** 를 선택 합니다.

    :::image type="content" source="./media/activity-logs/select-lab-resource.png" alt-text="랩을 리소스로 선택":::
1. **경고 규칙 만들기** 페이지로 돌아가서 **조건 선택** 을 클릭 합니다. 

    :::image type="content" source="./media/activity-logs/select-condition-link.png" alt-text="조건 링크 선택":::    
1. **신호 논리 구성** 페이지에서 DevTest Labs가 지 원하는 신호를 선택 합니다. 

    :::image type="content" source="./media/activity-logs/select-signal.png" alt-text="신호 선택":::
1. **이벤트 수준** (자세한 정보, 정보, 경고, 오류, 중요, 모두), **상태** (실패, 시작 됨, 성공) 및 이벤트를 **시작한 사람** 을 기준으로 필터링 합니다. 
1. **완료** 를 선택 하 여 조건 구성을 완료 합니다. 

    :::image type="content" source="./media/activity-logs/configure-signal-logic-done.png" alt-text="신호 논리 구성-완료":::
1. 범위 (랩) 및 경고 조건에 대해를 지정 했습니다. 이제 조건이 충족 될 때 실행할 동작을 포함 하는 작업 그룹을 지정 해야 합니다. **경고 규칙 만들기** 페이지로 돌아가 **작업 그룹 선택** 을 선택 합니다. 

    :::image type="content" source="./media/activity-logs/select-action-group-link.png" alt-text="작업 그룹 링크 선택":::
1. 도구 모음에서 **작업 그룹 만들기** 링크를 선택 합니다. 

    :::image type="content" source="./media/activity-logs/create-action-group-link.png" alt-text="작업 그룹 링크 만들기":::
1. **작업 그룹 추가** 페이지에서 다음 단계를 수행 합니다.
    1. 작업 그룹의 **이름** 을 입력합니다.
    1. 작업 그룹의 **짧은 이름** 을 입력합니다. 
    1. 경고를 만들려는 **리소스 그룹** 을 선택 합니다. 
    1. **동작의 이름을** 입력 합니다. 
    1. **작업 유형** (이 예제에서는 **전자 메일 Azure Resource Manager 역할**)을 선택 합니다. 

        :::image type="content" source="./media/activity-logs/add-action-group.png" alt-text="작업 그룹 페이지 추가":::
    1. **전자 메일 Azure Resource Manager 역할** 페이지에서 역할을 선택 합니다. 이 예제에서는 **소유자** 입니다. 그런 다음 **확인** 을 선택합니다. 

        :::image type="content" source="./media/activity-logs/select-role.png" alt-text="직무 선택":::            
    1. **작업 그룹 추가** 페이지에서 **확인** 을 선택합니다. 
1. 이제 **경고 규칙 만들기** 페이지에서 경고 규칙의 이름을 입력 하 고 **확인** 을 선택 합니다. 

    :::image type="content" source="./media/activity-logs/create-alert-rule-done.png" alt-text="경고 규칙 만들기-완료":::

## <a name="view-alerts"></a>경고 보기 
1. 모든 관리 작업 (이 예에서는)에 **대 한 경고에 경고가 표시** 됩니다. 경고가 표시 되는 데 시간이 오래 걸릴 수 있습니다. 

    :::image type="content" source="./media/activity-logs/alerts.png" alt-text="화면 캡처는 대시보드에 경고를 표시 합니다.":::
1. 열에서 숫자를 선택 하는 경우 (예: **총 경고**) 발생 한 경고가 표시 됩니다. 

    :::image type="content" source="./media/activity-logs/all-alerts.png" alt-text="모든 경고":::
1. 경고를 선택 하면 해당 경고에 대 한 세부 정보가 표시 됩니다. 

    :::image type="content" source="./media/activity-logs/alert-details.png" alt-text="경고 세부 정보":::
1. 이 예제에서는 다음 예제와 같이 콘텐츠가 포함 된 전자 메일을 받게 됩니다. 

    :::image type="content" source="./media/activity-logs/alert-email.png" alt-text="경고 이메일":::

## <a name="next-steps"></a>다음 단계
- 다른 작업 유형을 사용 하 여 작업 그룹을 만드는 방법에 대 한 자세한 내용은 [Azure Portal에서 작업 그룹 만들기 및 관리](../azure-monitor/alerts/action-groups.md)를 참조 하세요.
- 활동 로그에 대해 자세히 알아보려면  [Azure 활동 로그](../azure-monitor/essentials/activity-log.md)를 참조 하세요.
- 활동 로그에 대 한 경고를 설정 하는 방법에 대해 알아보려면 [활동 로그에 대 한 경고](../azure-monitor/alerts/activity-log-alerts.md)를 참조 하세요.

