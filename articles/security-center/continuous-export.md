---
title: 연속 내보내기는 Azure Security Center의 경고 및 권장 사항을 Log Analytics 작업 영역 또는 Azure에 보낼 수 있습니다 Event Hubs
description: Log Analytics 작업 영역 또는 Azure에 대 한 보안 경고 및 권장 사항의 연속 내보내기를 구성 하는 방법에 대해 알아봅니다 Event Hubs
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 12/24/2020
ms.author: memildin
ms.openlocfilehash: 9b8dc635781c96dcbd7aa423c77f60ff0556bd71
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100634069"
---
# <a name="continuously-export-security-center-data"></a>Security Center 데이터 연속 내보내기

Azure Security Center은 자세한 보안 경고 및 권장 사항을 생성 합니다. 포털에서 또는 프로그래밍 방식 도구를 통해 볼 수 있습니다. 사용자 환경에서 다른 모니터링 도구를 사용 하 여 추적 하기 위해이 정보 중 일부 또는 모두를 내보내야 할 수도 있습니다. 

**연속 내보내기를** 사용 하면 내보낼 *대상과* *위치* 를 완전히 사용자 지정할 수 있습니다. 예를 들어 다음을 수행 하도록 구성할 수 있습니다.

- 모든 심각도가 높은 경고는 Azure 이벤트 허브로 전송 됩니다.
- SQL server의 취약성 평가 검사에서 발생 하는 모든 중간 또는 높은 심각도가 특정 Log Analytics 작업 영역으로 전송 됩니다.
- 특정 권장 사항은 생성 될 때마다 이벤트 허브 또는 Log Analytics 작업 영역으로 전달 됩니다. 
- 구독에 대 한 보안 점수는 컨트롤의 점수가 0.01 이상 변경 될 때마다 Log Analytics 작업 영역으로 전송 됩니다. 

이 기능을 *연속* 이라고 하는 경우에도 보안 점수 또는 규정 준수 데이터의 주간 스냅숏을 내보내는 옵션도 있습니다.

이 문서에서는 Log Analytics 작업 영역 또는 Azure Event Hubs에 연속 내보내기를 구성 하는 방법을 설명 합니다.

> [!NOTE]
> SIEM과 Security Center를 통합 해야 하는 경우 [SIEM, 대화 충성도 또는 IT 서비스 관리 솔루션에 대 한 경고 스트림](export-to-siem.md)을 참조 하세요.

> [!TIP]
> 또한 Security Center는 CSV로 일회성 수동 내보내기를 수행 하는 옵션을 제공 합니다. [경고 및 권장 사항의 수동 일회성 내보내기](#manual-one-time-export-of-alerts-and-recommendations)에서 자세히 알아보세요.


## <a name="availability"></a>가용성

|양상|세부 정보|
|----|:----|
|릴리스 상태:|GA(일반 공급)|
|가격 책정:|Free|
|필요한 역할 및 권한:|<ul><li>리소스 그룹에 대 한 **보안 관리자** 또는 **소유자**</li><li>대상 리소스에 대 한 쓰기 권한</li><li>아래에서 설명 하는 ' DeployIfNotExist ' 정책을 사용 하는 경우에는 정책을 할당할 수 Azure Policy 있는 권한도 필요 합니다.</li></ul>|
|클라우드:|![예](./media/icons/yes-icon.png) 상용 클라우드<br>![예](./media/icons/yes-icon.png) 미국 정부, 기타 정부<br>![예](./media/icons/yes-icon.png) 중국 .Gov (이벤트 허브로)|
|||


## <a name="what-data-types-can-be-exported"></a>내보낼 수 있는 데이터 형식은 무엇입니까?

연속 내보내기는 변경 될 때마다 다음 데이터 형식을 내보낼 수 있습니다.

- 보안 경고
- 보안 권장 사항 
- 취약성 평가 스캐너 또는 특정 시스템 업데이트의 결과와 같은 ' sub ' 권장 사항으로 간주할 수 있는 보안 결과입니다. "시스템 업데이트를 컴퓨터에 설치 해야 합니다."와 같은 ' 부모 ' 권장 사항을 사용 하 여 포함 하도록 선택할 수 있습니다.
- 보안 점수 (구독 당 또는 컨트롤 단위)
- 규정 준수 데이터

> [!NOTE]
> 보안 점수 및 규정 준수 데이터 내보내기는 미리 보기 기능이 며 정부 클라우드에서 사용할 수 없습니다. 

## <a name="set-up-a-continuous-export"></a>연속 내보내기 설정 

제공 된 Azure Policy 템플릿을 사용 하 여 Azure Portal의 Security Center 페이지에서 Security Center REST API를 통해 또는 대규모로 연속 내보내기를 구성할 수 있습니다. 각각에 대 한 자세한 내용을 보려면 아래에서 해당 탭을 선택 합니다.

### <a name="use-the-azure-portal"></a>[**Azure Portal 사용**](#tab/azure-portal)

### <a name="configure-continuous-export-from-the-security-center-pages-in-azure-portal"></a>Azure Portal의 Security Center 페이지에서 연속 내보내기 구성

Log Analytics 작업 영역 또는 Azure Event Hubs에 대 한 연속 내보내기를 설정 하 고 있는지 여부에 따라 다음 단계가 필요 합니다.

1. Security Center의 사이드바에서 **가격 책정 & 설정** 을 선택 합니다.
1. 데이터 내보내기를 구성할 특정 구독을 선택 합니다.
1. 해당 구독에 대 한 설정 페이지의 사이드바에서 **연속 내보내기** 를 선택 합니다.

    :::image type="content" source="./media/continuous-export/continuous-export-options-page.png" alt-text="Azure Security Center의 내보내기 옵션":::

    여기에서 내보내기 옵션을 볼 수 있습니다. 사용 가능한 각 내보내기 대상에 대 한 탭이 있습니다. 

1. 내보낼 데이터 형식을 선택 하 고 각 유형의 필터에서 선택 합니다 (예: 높은 심각도 경고만 내보내기).
1. 적절 한 내보내기 빈도를 선택 합니다.
    - **스트리밍** – 리소스의 상태가 업데이트되면 평가가 실시간으로 전송됩니다(업데이트가 발생하지 않으면 데이터가 전송되지 않음).
    - **스냅샷** – 모든 규정 준수 평가의 현재 상태에 대한 스냅샷이 매주 전송됩니다(이는 보안 점수 및 규정 준수 데이터의 주간 스냅샷에 대한 미리 보기 기능임).

1. 선택 사항에 따라 이러한 권장 사항 중 하나가 포함 된 경우 취약성 평가 결과를 함께 포함할 수 있습니다.
    - SQL 데이터베이스에 대 한 취약성 평가 결과를 재구성 해야 합니다.
    - 컴퓨터의 SQL server에 대 한 취약성 평가 결과를 재구성 해야 함 (미리 보기)
    - Azure Container Registry 이미지의 취약성을 수정해야 함(Qualys 제공)
    - 가상 머신의 취약성을 수정해야 함
    - 시스템 업데이트를 머신에 설치해야 합니다.

    이러한 권장 사항을 포함 하는 결과를 포함 하려면 **보안 결과 포함** 옵션을 사용 하도록 설정 합니다.

    :::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="연속 내보내기 구성의 보안 결과 포함 설정/해제" :::

1. "대상 내보내기" 영역에서 데이터를 저장 하려는 위치를 선택 합니다. 데이터는 다른 구독 (예: 중앙 이벤트 허브 인스턴스 또는 중앙 Log Analytics 작업 영역)의 대상에 저장할 수 있습니다.
1. **저장** 을 선택합니다.

### <a name="use-the-rest-api"></a>[**REST API 사용**](#tab/rest-api)

### <a name="configure-continuous-export-using-the-rest-api"></a>REST API를 사용 하 여 연속 내보내기 구성

연속 내보내기는 Azure Security Center [자동화 API](/rest/api/securitycenter/automations)를 통해 구성 및 관리할 수 있습니다. 이 API를 사용 하 여 가능한 다음 대상으로 내보내기 위한 규칙을 만들거나 업데이트 합니다.

- Azure Event Hub
- Log Analytics 작업 영역
- Azure Logic Apps 

API는 Azure Portal에서 사용할 수 없는 추가 기능을 제공 합니다. 예를 들면 다음과 같습니다.

* **큰 볼륨** -API를 사용 하면 단일 구독에서 여러 내보내기 구성을 만들 수 있습니다. Security Center의 포털 UI에서 **연속 내보내기** 페이지는 구독 당 하나의 내보내기 구성만 지원 합니다.

* **추가 기능** -API는 UI에 표시 되지 않는 추가 매개 변수를 제공 합니다. 예를 들어 automation 리소스에 태그를 추가 하 고, Security Center의 포털 UI에서 **연속 내보내기** 페이지에서 제공 되는 것 보다 광범위 한 경고 및 권장 사항 속성 집합을 기반으로 내보내기를 정의할 수 있습니다.

* **더 집중 된 범위** -API는 내보내기 구성의 범위에 대해 보다 세분화 된 수준을 제공 합니다. API를 사용 하 여 내보내기를 정의할 때 리소스 그룹 수준에서이 작업을 수행할 수 있습니다. Security Center의 포털 UI에서 **연속 내보내기** 페이지를 사용 하는 경우 구독 수준에서 정의 해야 합니다.

    > [!TIP]
    > API를 사용 하 여 여러 내보내기 구성을 설정 했거나 API 전용 매개 변수를 사용한 경우 해당 추가 기능은 Security Center UI에 표시 되지 않습니다. 대신 다른 구성이 존재 한다는 것을 알리는 배너가 표시 됩니다.

[REST API 설명서](/rest/api/securitycenter/automations)의 자동화 API에 대해 자세히 알아보세요.





### <a name="deploy-at-scale-with-azure-policy"></a>[**Azure Policy를 사용 하 여 대규모로 배포**](#tab/azure-policy)

### <a name="configure-continuous-export-at-scale-using-the-supplied-policies"></a>제공 된 정책을 사용 하 여 대규모로 연속 내보내기 구성

조직의 모니터링 및 인시던트 대응 프로세스를 자동화하면 보안 인시던트를 조사하고 완화하는 데 걸리는 시간을 크게 향상시킬 수 있습니다.

조직 전체에 연속 내보내기 구성을 배포 하려면 아래에서 설명 하는 제공 된 Azure Policy ' DeployIfNotExist ' 정책을 사용 하 여 연속 내보내기 프로시저를 만들고 구성 합니다.

**이러한 정책을 구현 하려면**

1. 아래 표에서 적용 하려는 정책을 선택 합니다.

    |목표  |정책  |정책 ID  |
    |---------|---------|---------|
    |이벤트 허브로 연속 내보내기|[Azure Security Center 경고 및 권장 사항에 대한 Event Hub로 내보내기 배포](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fcdfcce10-4578-4ecd-9703-530938e4abcb)|cdfcce10-4578-4ecd-9703-530938e4abcb|
    |Log Analytics 작업 영역으로 연속 내보내기|[Azure Security Center 경고 및 권장 사항에 대한 Log Analytics 작업 영역으로 내보내기 배포](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2fproviders%2fMicrosoft.Authorization%2fpolicyDefinitions%2fffb6f416-7bd2-4488-8828-56585fef2be9)|ffb6f416-7bd2-4488-8828-56585fef2be9|
    ||||

    > [!TIP]
    > Azure Policy 검색 하 여 찾을 수도 있습니다.
    > 1. Azure Policy를 엽니다.
    > :::image type="content" source="./media/continuous-export/opening-azure-policy.png" alt-text="Azure Policy 액세스":::
    > 2. Azure Policy 메뉴에서 **정의** 를 선택 하 고 이름으로 검색 합니다. 

1. 관련 Azure Policy 페이지에서 **할당** 을 선택 합니다.
    :::image type="content" source="./media/continuous-export/export-policy-assign.png" alt-text="Azure Policy 할당":::

1. 각 탭을 열고 필요에 따라 매개 변수를 설정 합니다.
    1. **기본 사항** 탭에서 정책에 대 한 범위를 설정 합니다. 중앙 집중식 관리를 사용 하려면 연속 내보내기 구성을 사용 하는 구독을 포함 하는 관리 그룹에 정책을 할당 합니다. 
    1. **매개 변수** 탭에서 리소스 그룹 및 데이터 형식 세부 정보를 설정 합니다. 
        > [!TIP]
        > 각 매개 변수에는 사용할 수 있는 옵션을 설명 하는 도구 설명이 있습니다.
        >
        > Azure Policy의 매개 변수 탭 (1)은 Security Center의 연속 내보내기 페이지 (2)와 유사한 구성 옵션에 대 한 액세스를 제공 합니다.
        > :::image type="content" source="./media/continuous-export/azure-policy-next-to-continuous-export.png" alt-text="Azure Policy와 연속 내보내기의 매개 변수 비교" lightbox="./media/continuous-export/azure-policy-next-to-continuous-export.png":::
    1. 필요에 따라이 할당을 기존 구독에 적용 하려면 **수정** 탭을 열고 수정 작업을 만드는 옵션을 선택 합니다.
1. 요약 페이지를 검토 하 고 **만들기** 를 선택 합니다.

--- 

## <a name="information-about-exporting-to-a-log-analytics-workspace"></a>Log Analytics 작업 영역으로 내보내기에 대 한 정보

Log Analytics 작업 영역 내의 Azure Security Center 데이터를 분석 하거나 Azure 경고를 Security Center 알림과 함께 사용 하려면 Log Analytics 작업 영역으로 연속 내보내기를 설정 합니다.

### <a name="log-analytics-tables-and-schemas"></a>테이블 및 스키마 Log Analytics

보안 경고 및 권장 사항은 각각 *Securityalert* 및 *securityalert* 테이블에 저장 됩니다. 

이러한 테이블을 포함 하는 Log Analytics 솔루션의 이름은 Azure Defender를 사용 하는지 여부 (보안 (' 보안 및 감사 ') 또는 Securitycenter 무료)에 따라 다릅니다. 

> [!TIP]
> 대상 작업 영역에서 데이터를 보려면 이러한 솔루션 중 하나를 사용 하도록 설정 하거나 **securitycenter** 를 사용 하도록 설정 해야 합니다 **보안 및 감사** .

![Log Analytics의 * SecurityAlert * 테이블](./media/continuous-export/log-analytics-securityalert-solution.png)

내보낸 데이터 형식의 이벤트 스키마를 보려면 [Log Analytics 테이블 스키마](https://aka.ms/ASCAutomationSchemas)를 참조 하세요.


##  <a name="view-exported-alerts-and-recommendations-in-azure-monitor"></a>Azure Monitor에서 내보낸 경고 및 권장 사항 보기

[Azure Monitor](../azure-monitor/alerts/alerts-overview.md)에서 내보낸 보안 경고 및/또는 권장 사항을 보도록 선택할 수도 있습니다. 

Azure Monitor는 진단 로그, 메트릭 경고 및 Log Analytics 작업 영역 쿼리를 기반으로 하는 사용자 지정 경고를 비롯 한 다양 한 Azure 경고에 대 한 통합 경고 환경을 제공 합니다.

Azure Monitor에서 Security Center의 경고 및 권장 사항을 보려면 Log Analytics 쿼리 (로그 경고)를 기반으로 경고 규칙을 구성 합니다.

1. Azure Monitor의 **경고** 페이지에서 **새 경고 규칙** 을 선택 합니다.

    ![Azure Monitor의 경고 페이지](./media/continuous-export/azure-monitor-alerts.png)

1. 규칙 만들기 페이지에서 새 규칙을 구성 합니다 ( [Azure Monitor에서 로그 경고 규칙](../azure-monitor/alerts/alerts-unified-log.md)을 구성 하는 것과 같은 방식으로).

    * **리소스** 에서 보안 경고 및 권장 사항을 내보낸 Log Analytics 작업 영역을 선택 합니다.

    * **조건** 에 대해 **사용자 지정 로그 검색** 을 선택 합니다. 표시 되는 페이지에서 query, lookback period 및 frequency period를 구성 합니다. 검색 쿼리에서는 Log Analytics으로 연속 내보내기를 사용 하도록 설정 하는 경우 *Securityalert* 또는 *securityalert* 을 입력 하 여 Security Center 연속으로 내보낼 데이터 형식을 쿼리할 수 있습니다. 
    
    * 필요에 따라 트리거할 [작업 그룹](../azure-monitor/alerts/action-groups.md) 을 구성 합니다. 작업 그룹은 전자 메일 전송, ITSM 티켓, 웹 후크 등을 트리거할 수 있습니다.
    ![Azure Monitor 경고 규칙](./media/continuous-export/azure-monitor-alert-rule.png)

이제 작업 그룹 (제공 된 경우)의 자동 트리거를 사용 하 여 Azure Monitor 경고에서 구성 된 연속 내보내기 규칙 및 Azure Monitor 경고 규칙에서 정의한 조건에 따라 새로운 Azure Security Center 경고 또는 권장 사항이 표시 됩니다.

## <a name="manual-one-time-export-of-alerts-and-recommendations"></a>경고 및 권장 사항의 수동 1 회 내보내기

경고 또는 권장 사항에 대 한 CSV 보고서를 다운로드 하려면 **보안 경고** 또는 **권장 사항** 페이지를 열고 **csv 보고서 다운로드** 단추를 선택 합니다.

:::image type="content" source="./media/continuous-export/download-alerts-csv.png" alt-text="경고 데이터를 CSV 파일로 다운로드" lightbox="./media/continuous-export/download-alerts-csv.png":::

> [!NOTE]
> 이러한 보고서에는 현재 선택한 구독의 리소스에 대 한 경고 및 권장 사항이 포함 되어 있습니다.


## <a name="faq---continuous-export"></a>FAQ-연속 내보내기

### <a name="what-are-the-costs-involved-in-exporting-data"></a>데이터를 내보내는 데 드는 비용은 얼마 인가요?

연속 내보내기를 사용하기 위한 비용은 없습니다. 사용자의 구성에 따라 Log Analytics 작업 영역에서 데이터를 수집 하 고 보존 하는 데 비용이 발생할 수 있습니다. 

[Log Analytics 작업 영역 가격 책정](https://azure.microsoft.com/pricing/details/monitor/)에 대해 자세히 알아보세요.

[Azure 이벤트 허브 가격 책정](https://azure.microsoft.com/pricing/details/event-hubs/)에 대해 자세히 알아보세요.


### <a name="does-the-export-include-data-about-the-current-state-of-all-resources"></a>내보내기에 모든 리소스의 현재 상태에 대 한 데이터가 포함 됩니까?

아니요. 연속 내보내기는 **이벤트** 스트리밍을 위해 작성 되었습니다.

- 내보내기를 사용 하도록 설정 하기 전에 받은 **경고** 는 내보내지 않습니다.
- **권장 사항은** 리소스의 호환성 상태가 변경 될 때마다 전송 됩니다. 예를 들어 리소스가 정상에서 비정상으로 전환 되는 경우입니다. 따라서 경고와 마찬가지로 내보내기를 사용 하도록 설정한 이후 상태를 변경 하지 않은 리소스에 대 한 권장 사항을 내보내지 않습니다.
- 보안 제어 또는 구독에 대 한 보안 **점수 (미리 보기)** 는 보안 제어의 점수가 0.01 이상으로 변경 될 때 전송 됩니다. 
- **규정 준수 상태 (미리 보기)** 는 리소스의 호환성 상태가 변경 될 때 전송 됩니다.



### <a name="why-are-recommendations-sent-at-different-intervals"></a>권장 사항이 다른 간격으로 전송되는 이유는 무엇인가요?

권장 사항 마다 다른 규정 준수 평가 간격이 있습니다. 몇 분 마다 며칠 마다 달라질 수 있습니다. 따라서 권장 사항은 내보내기에 표시 되는 데 걸리는 시간에 따라 달라 집니다.

### <a name="does-continuous-export-support-any-business-continuity-or-disaster-recovery-bcdr-scenarios"></a>연속 내보내기는 BCDR (비즈니스 연속성 또는 재해 복구) 시나리오를 지원 하나요?

대상 리소스에 가동 중단 또는 기타 재해가 발생 하는 BCDR 시나리오에 대 한 환경을 준비 하는 경우 Azure Event Hubs, Log Analytics 작업 영역 및 논리 앱의 지침에 따라 백업을 설정 하 여 데이터 손실을 방지 하는 것이 조직의 책임입니다.

[Azure Event Hubs-지역 재해 복구](../event-hubs/event-hubs-geo-dr.md)에 대해 자세히 알아보세요.


### <a name="is-continuous-export-available-with-azure-security-center-free"></a>연속 내보내기는 Azure Security Center 무료로 사용할 수 있나요?

예! Azure Defender를 사용 하도록 설정한 경우에만 많은 Security Center 경고가 제공 됩니다. 내보낸 데이터에서 얻을 수 있는 경고를 미리 보는 좋은 방법은 Azure Portal의 Security Center 페이지에 표시 된 경고를 확인 하는 것입니다.



## <a name="next-steps"></a>다음 단계

이 문서에서는 권장 사항 및 경고의 연속 내보내기를 구성 하는 방법에 대해 알아보았습니다. 또한 경고 데이터를 CSV 파일로 다운로드 하는 방법도 배웠습니다. 

관련 자료는 다음 설명서를 참조 하세요. 

- [워크플로 자동화 템플릿에](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation)대해 자세히 알아보세요.
- [Azure Event Hubs 설명서](../event-hubs/index.yml)
- [Azure Sentinel 설명서](../sentinel/index.yml)
- [Azure Monitor 설명서](../azure-monitor/index.yml)
- [데이터 형식 스키마 내보내기](https://aka.ms/ASCAutomationSchemas)