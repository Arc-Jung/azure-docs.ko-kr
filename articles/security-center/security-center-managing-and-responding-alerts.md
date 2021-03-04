---
title: Azure Security Center에서 보안 경고 관리| Microsoft Docs
description: 이 문서는 Azure Security Center 기능을 사용하여 보안 경고를 관리하고 대응하는 데 도움이 됩니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: how-to
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/17/2021
ms.author: memildin
ms.openlocfilehash: 490f94c71611e07e765f5882cb4e3eec13033a6a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099338"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Azure Security Center에서 보안 경고 관리 및 응답

이 항목에서는 Security Center의 경고를 확인 하 고 처리 하 고 리소스를 보호 하는 방법을 보여 줍니다.

보안 경고를 트리거하는 고급 검색은 Azure Defender 에서만 사용할 수 있습니다. 평가판을 사용할 수 있습니다. 업그레이드 하려면 [Azure Defender 사용](enable-azure-defender.md)을 참조 하세요.

## <a name="what-are-security-alerts"></a>보안 경고란?
보안 센터는 방화벽 및 엔드포인트 보호 솔루션과 같은 Azure 리소스, 네트워크 및 연결된 파트너 솔루션의 로그 데이터를 자동으로 수집하고 분석하며 통합하여 실제 위협을 감지하고 가양성을 줄입니다. 우선 순위가 지정된 보안 경고의 목록은 문제를 신속하게 조사해야 하는 정보 및 공격을 해결하는 방법에 대한 권장 사항과 함께 보안 센터에 표시됩니다.

다양 한 유형의 경고에 대 한 자세한 내용은 [보안 경고-참조 가이드](alerts-reference.md)를 참조 하세요.

Security Center에서 경고를 생성 하는 방법에 대 한 개요는 [Azure Security Center 검색 하 고 위협에 대응 하는 방법](security-center-alerts-overview.md)을 참조 하세요.


## <a name="manage-your-security-alerts"></a>보안 경고 관리

1. Security Center의 개요 페이지에서 페이지 맨 위에 있는 **보안 경고** 타일 또는 사이드바의 링크를 선택 합니다.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/overview-page-alerts-links.png" alt-text="Azure Security Center의 개요 페이지에서 보안 경고 페이지로 가져오기":::

    보안 경고 페이지가 열립니다.

    :::image type="content" source="media/security-center-managing-and-responding-alerts/alerts-page.png" alt-text="Azure Security Center의 보안 경고 목록":::

1. 경고 목록을 필터링 하려면 관련 필터를 선택 합니다. 필요에 따라 **필터 추가** 옵션을 사용 하 여 추가 필터를 추가할 수 있습니다.

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-small.png" alt-text="경고 보기에 필터 추가" lightbox="./media/security-center-managing-and-responding-alerts/alerts-adding-filters-large.png":::

    목록은 선택한 필터링 옵션에 따라 업데이트 됩니다. 필터링은 매우 유용할 수 있습니다. 예를 들어 시스템에서 잠재적 위반을 조사하고 있기 때문에 최근 24시간 동안 발생한 보안 경고를 해결하려고 할 수 있습니다.


## <a name="respond-to-security-alerts"></a>보안 경고에 대응

1. **보안 경고** 목록에서 경고를 선택 합니다. 사이드 창이 열리고 영향을 받는 모든 리소스 및 경고에 대 한 설명이 표시 됩니다. 

    :::image type="content" source="./media/security-center-managing-and-responding-alerts/alerts-details-pane.png" alt-text="보안 경고의 최소 세부 정보 보기":::

    > [!TIP]
    > 이 측면 창이 열려 있으면 키보드에서 위쪽 및 아래쪽 화살표를 사용 하 여 경고 목록을 신속 하 게 검토할 수 있습니다.

1. 자세한 내용을 **보려면 전체 세부 정보 보기** 를 선택 합니다.

    보안 경고 페이지의 왼쪽 창에는 보안 경고의 제목, 심각도, 상태, 작업 시간, 의심 스러운 활동 설명 및 영향을 받는 리소스에 대 한 높은 수준의 정보가 표시 됩니다. 영향을 받는 리소스와 함께 리소스와 관련 된 Azure 태그가 있습니다. 이를 사용 하 여 경고를 조사할 때 리소스의 조직 컨텍스트를 유추 합니다.

    오른쪽 창에는 문제를 조사 하는 데 도움이 되는 경고에 대 한 자세한 정보를 포함 하는 **경고 정보** 탭 (IP 주소, 파일, 프로세스 등)이 포함 되어 있습니다.
     
    ![보안 경고에 대해 수행할 작업에 대 한 제안](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    오른쪽 창에는 **작업 수행** 탭도 있습니다. 이 탭을 사용 하 여 보안 경고와 관련 된 추가 작업을 수행할 수 있습니다. 작업은 다음과 같습니다.
    - *위협 완화* -이 보안 경고에 대 한 수동 수정 단계를 제공 합니다.
    - *향후 공격 방지* -공격 노출 영역을 줄이고 보안 상태를 높이고 이후 공격을 방지 하는 데 도움이 되는 보안 권장 사항을 제공 합니다.
    - *자동 응답 트리거* -이 보안 경고에 대 한 응답으로 논리 앱을 트리거하는 옵션을 제공 합니다.
    - *유사한 경고 표시 안 함* -경고가 조직과 관련이 없는 경우 유사한 특성을 가진 이후 경고를 표시 하지 않는 옵션을 제공 합니다.

    ![작업 탭 사용](./media/security-center-managing-and-responding-alerts/alert-take-action.png)




## <a name="see-also"></a>참고 항목

이 문서에서는 보안 경고를 보는 방법에 대해 알아보았습니다. 관련 자료는 다음 페이지를 참조 하세요.

- [경고 억제 규칙 구성](alerts-suppression-rules.md)
- [Security Center 트리거에 대 한 응답 자동화](workflow-automation.md)
- [보안 경고 - 참조 가이드](alerts-reference.md)
