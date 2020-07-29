---
title: Azure Application Insights Funnels
description: Funnels를 사용하여 고객이 애플리케이션과 상호 작용하는 방법을 검색하는 방법을 알아봅니다.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/17/2017
ms.reviewer: mbullwin
ms.openlocfilehash: a81c4f32385cf06238f36ca0162923f93d682807
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323726"
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Application Insights Funnels를 통해 고객이 애플리케이션을 사용하는 방법 검색

고객 환경을 이해하는 것이 비즈니스에 가장 중요합니다. 애플리케이션이 여러 단계와 관련된 경우, 고객 대부분이 전체 프로세스를 거치는지 아니면 특정 시점에서 프로세스를 종료하는지를 알아야 합니다. 웹 애플리케이션에서 일련의 단계를 거치는 것을 *깔때기*라고 합니다. Azure Application Insights Funnels를 사용하여 사용자에 대한 정보를 얻고 단계별 전환율을 모니터링할 수 있습니다. 

## <a name="create-your-funnel"></a>깔때기 만들기
깔때기를 만들기 전에 먼저 답변하려는 질문에 관해 결정합니다. 예를 들어, 고객 프로필을 보고 티켓을 만들어 몇 명의 사용자가 홈페이지를 보고 있는지 확인하려고 할 수 있습니다. 이 예제에서는 Fabrikam Fiber 회사 소유자가 성공적으로 고객 티켓을 생성한 고객의 비율을 확인하려고 합니다.

깔때기를 만들기 위해 수행하는 단계는 다음과 같습니다.

1. Application Insights Funnels 도구에서 **새로 만들기**를 선택합니다.
1. **시간 범위** 드롭다운 메뉴에서 **지난 90일**을 선택합니다. **내 깔때기** 또는 **공유 깔때기**를 선택합니다.
1. **1단계** 드롭다운 목록에서 **인덱스**를 선택합니다. 
1. **2단계** 목록에서 **고객**을 선택합니다.
1. **3단계** 목록에서 **만들기**를 선택합니다.
1. 깔때기에 이름을 추가하고 **저장**을 선택합니다.

다음 스크린샷은 Funnels 도구가 생성하는 데이터 종류의 예를 보여 줍니다. Fabrikam 소유자는 지난 90일 동안 홈페이지를 방문한 고객 중 54.3%가 고객 티켓을 생성했다는 사실을 알 수 있습니다. 또한 고객 2,700명이 홈 페이지에서 인덱스로 들어온 것을 알 수 있습니다. 이것은 새로 고침 문제를 의미할 수 있습니다.


![데이터가 표시된 Funnels 도구 스크린샷](media/usage-funnels/funnel1.png)

### <a name="funnels-features"></a>Funnels 기능
앞의 스크린샷에는 5개의 강조 표시된 영역이 있습니다. 이들은 Funnels의 기능입니다. 다음 목록에서는 스크린샷의 각 해당 영역을 상세히 설명합니다.
1. 앱이 샘플링된 경우 샘플링 배너가 표시됩니다. 배너를 선택하면 컨텍스트 창이 열려 샘플링을 끄는 방법을 설명합니다. 
2. 깔때기를 [Power BI](./export-power-bi.md)로 내보낼 수 있습니다.
3. 단계를 선택하면 오른쪽에 더 상세한 내용이 표시됩니다. 
4. 기록 전환 그래프에서 지난 90일 동안의 전환율을 확인할 수 있습니다. 
5. 사용자 도구에 액세스하여 사용자에 대해 더 깊이 있게 파악할 수 있습니다. 각 단계에서 필터를 사용할 수 있습니다. 

## <a name="next-steps"></a>다음 단계
  * [사용 현황 개요](usage-overview.md)
  * [사용자, 세션 및 이벤트](usage-segmentation.md)
  * [보존](usage-retention.md)
  * [통합 문서](../platform/workbooks-overview.md)
  * [사용자 컨텍스트 추가](usage-send-user-context.md)
  * [Power BI에 내보내기](./export-power-bi.md)

