---
title: Azure Log Analytics 데이터를 Power BI로 가져오기 | Microsoft Docs
description: Power BI는 서로 다른 데이터 집합의 분석을 위해 다양한 시각화 및 보고서를 제공하는 Microsoft의 클라우드 기반 비즈니스 분석 서비스입니다.  이 아티클에서는 Log Analytics 데이터를 구성하고 Power BI로 가져오고 자동으로 새로 고침하도록 구성하는 방법을 설명합니다.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/01/2019
ms.openlocfilehash: 8ff24d508eb35c4f2a04c7d024254fa6f1875da8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77659287"
---
# <a name="import-azure-monitor-log-data-into-power-bi"></a>Azure Monitor 로그 데이터를 Power BI로 가져오기


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) 은 다양 한 데이터 집합을 분석 하기 위한 풍부한 시각화 및 보고서를 제공 하는 Microsoft의 클라우드 기반 비즈니스 분석 서비스입니다.  여러 소스의 데이터를 결합 하 고 웹 및 모바일 장치에서 보고서를 공유 하는 등의 기능을 활용할 수 있도록 Azure Monitor 로그 쿼리의 결과를 Power BI 데이터 집합으로 가져올 수 있습니다.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview"></a>개요
Azure Monitor의 [Log Analytics 작업 영역](manage-access.md) 에서 Power BI으로 데이터를 가져오려면 Azure Monitor의 [로그 쿼리](../log-query/log-query-overview.md) 를 기반으로 Power BI에서 데이터 집합을 만듭니다.  쿼리는 데이터 세트가 새로 고침될 때마다 실행됩니다.  데이터 세트의 데이터를 사용하는 Power BI 보고서를 작성할 수 있습니다.  Power BI에서 데이터 세트를 만들려면 Log Analytics에서 [파워 쿼리(M) 언어](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification)로 쿼리를 내보냅니다.  그런 다음, Power BI Desktop에 쿼리를 만들고 Power BI에 데이터 세트로 게시합니다.  이 프로세스에 대한 세부 정보는 아래에 설명되어 있습니다.

![Log Analytics에서 Power BI로](media/powerbi/overview.png)

## <a name="export-query"></a>내보내기 쿼리
먼저 Power BI 데이터 집합을 채울 데이터를 반환 하는 [로그 쿼리](../log-query/log-query-overview.md) 를 만듭니다.  그런 다음 해당 쿼리를 [파워 쿼리(M) 언어](https://docs.microsoft.com/powerquery-m/power-query-m-language-specification)로 내보내면 Power BI Desktop에서 사용할 수 있습니다.

1. 데이터 집합에 대 한 데이터를 추출 하는 [Log Analytics에서 로그 쿼리를 만듭니다](../log-query/get-started-portal.md) .
2. **내보내기**  >  **Power BI 쿼리 (M)** 를 선택 합니다.  이렇게 하면 쿼리가 **PowerBIQuery.txt**라는 텍스트 파일로 내보내집니다. 

    ![로그 검색 내보내기](media/powerbi/export-analytics.png)

3. 텍스트 파일을 열고 내용을 복사합니다.

## <a name="import-query-into-power-bi-desktop"></a>Power BI Desktop으로 쿼리 가져오기
Power BI Desktop은 Power BI에 게시할 수 있는 데이터 세트 및 보고서를 만들 수 있는 데스크톱 애플리케이션입니다.  Azure Monitor에서 내보낸 파워 쿼리 언어를 사용 하 여 쿼리를 만드는 데 사용할 수도 있습니다. 

1. 아직 설치하지 않았으면 [Power BI Desktop](https://powerbi.microsoft.com/desktop/)을 설치하고 애플리케이션을 엽니다.
2. **데이터 가져오기**  >  **빈 쿼리** 를 선택 하 여 새 쿼리를 엽니다.  그런 다음 **고급 편집기**를 선택하고 내보낸 파일의 내용을 쿼리에 붙여넣습니다. **Done**을 클릭합니다.

    ![Power BI 데스크톱 쿼리](media/powerbi/desktop-new-query.png)

5. 쿼리가 실행되고 결과가 표시됩니다.  Azure에 연결하려면 자격 증명이 필요할 수도 있습니다.  
6. 쿼리를 설명하는 이름을 입력합니다.  기본값은 **Query1**입니다. **닫고 적용**을 클릭하여 보고서에 데이터 세트를 추가합니다.

    ![Power BI Desktop 이름](media/powerbi/desktop-results.png)



## <a name="publish-to-power-bi"></a>Power BI에 게시
Power BI에 게시할 때 데이터 세트 및 보고서가 생성됩니다.  Power BI Desktop에서 보고서를 만드는 경우 사용자의 데이터를 사용하여 다음 항목이 게시됩니다.  그렇지 않은 경우 빈 보고서가 생성됩니다.  Power BI에서 보고서를 수정할 수도 있고 데이터 세트에 따라 새로 만들 수도 있습니다.

1. 데이터를 기반으로 보고서를 만듭니다.  아직 익숙하지 않은 경우 [Power BI Desktop 설명서](https://docs.microsoft.com/power-bi/desktop-report-view)를 사용합니다.  
1. Power BI로 보낼 준비가 완료되면 **게시**를 클릭합니다.  
1. 메시지가 표시되면 Power BI 계정에서 대상을 선택합니다.  염두에 둔 특정 대상이 없으면 **My workspace**를 사용합니다.

    ![Power BI Desktop 게시](media/powerbi/desktop-publish.png)

1. 게시가 완료되면 **Power BI에서 열기**를 클릭하여 새 데이터 세트로 Power BI를 엽니다.


### <a name="configure-scheduled-refresh"></a>예약된 새로 고침 구성
Power BI에서 생성된 데이터 세트는 이전에 Power BI Desktop에서 본 것과 동일한 데이터를 갖습니다.  쿼리를 다시 실행 하 고 Azure Monitor의 최신 데이터로 채우려면 데이터 집합을 주기적으로 새로 고쳐야 합니다.  

1. 보고서를 업로드한 작업 영역을 클릭하고 **데이터 세트** 메뉴를 선택합니다. 
1. 새 데이터 세트 옆에 있는 상황에 맞는 메뉴를 선택하고 **설정**을 선택합니다. 
1. **데이터 원본 자격 증명** 아래에 자격 증명이 유효하지 않다는 내용의 메시지가 표시될 것입니다.  데이터 세트가 데이터를 새로 고칠 때 사용할 자격 증명을 아직 제공하지 않았기 때문입니다.  
1. **자격 증명 편집** 을 클릭 하 고 Azure Monitor의 Log Analytics 작업 영역에 액세스할 수 있는 자격 증명을 지정 합니다. 2 단계 인증이 필요한 경우 **인증 방법** 으로 **OAuth2** 를 선택 하 여 자격 증명으로 로그인 하 라는 메시지를 표시 합니다.

    ![Power BI 일정](media/powerbi/powerbi-schedule.png)

5. **예약된 새로 고침** 아래에서 **데이터를 최신 상태로 유지** 옵션을 선택합니다.  필요에 따라 **새로 고침 빈도** 및 새로 고침을 실행할 하나 이상의 특정 시간을 변경할 수 있습니다.

    ![Power BI 새로 고침](media/powerbi/powerbi-schedule-refresh.png)



## <a name="next-steps"></a>다음 단계
* Power BI로 내보낼 수 있는 쿼리를 작성하려면 [로그 검색](../log-query/log-query-overview.md) 에 대해 알아봅니다.
* Azure Monitor 로그 내보내기를 기반으로 시각화를 빌드하려면 [Power BI](https://powerbi.microsoft.com) 에 대해 자세히 알아보세요.
