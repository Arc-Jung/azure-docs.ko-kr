---
title: Azure에서 예기치 않은 비용을 방지 하 고 청구를 관리 합니다.
description: Azure 청구서에 예기치 않은 요금이 부과되지 않도록 하는 방법을 알아봅니다. Azure 구독에 대 한 비용 추적 및 관리 기능을 사용 합니다.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: b64e84c3fff27675029ff35f27972a4aca014ec3
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68611999"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Azure 청구 및 비용 관리를 사용하여 예기치 않은 비용 방지

Azure에 등록할 때 지출에 대해 더 잘 이해 하기 위해 수행할 수 있는 몇 가지 작업이 있습니다.

- [가격 계산기](https://azure.microsoft.com/pricing/calculator/)는 Azure 리소스를 만들기 전에 예상 비용을 제공할 수 있습니다. 

- [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)은 사용자 구독에 대한 현재 비용 분석 및 예측을 제공합니다. 

- 여러 다른 프로젝트 또는 팀의 비용을 그룹화하고 파악하려는 경우 [리소스 태그](../azure-resource-manager/resource-group-using-tags.md)를 살펴봅니다. 선호하는 보고 시스템이 조직에 있는 경우 [청구 API](billing-usage-rate-card-overview.md)를 확인하세요.

- EA (기업계약)에서 구독을 만든 경우 Azure Portal에서 비용을 볼 수 있습니다. 구독이 CSP (클라우드 솔루션 공급자) 또는 Azure 스폰서쉽을 통해 수행 되는 경우 다음 기능 중 일부는 적용 되지 않을 수 있습니다. 자세한 내용은 [EA, CSP 및 후원을 위한 추가 리소스](#other-offers)를 참조 하세요.

- 구독이 무료 평가판 제품, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), AZURE IN OPEN (AIO) 또는 BizSpark 경우 모든 크레딧을 사용할 때 구독이 자동으로 사용 하지 않도록 설정 됩니다. 구독이 예기치 않게 사용할 수 없도록 설정되는 것을 방지하려면 [지출 한도](#spending-limit)에 대해 자세히 알아보세요.

- [Azure 무료 계정](https://azure.microsoft.com/free/)에 등록 한 경우 [12 개월 동안 가장 인기 있는 azure 서비스 중 일부를 무료로 사용할 수](billing-create-free-services-included-free-account.md)있습니다. 아래에 나열된 권장 사항과 함께 [체험 계정에서 요금 청구 방지](billing-avoid-charges-free-account.md)를 참조하세요.

## <a name="get-estimated-costs-before-adding-azure-services"></a>Azure 서비스를 추가하기 전에 예상 비용 얻기

다음 도구를 사용 하 여 비용을 계산 하는 데 대 한 몇 가지 추가 정보는 다음과 같습니다.
- Azure 요금 계산기
- Azure Portal
- 지출 한도

다음 섹션의 이미지에서는 미국 달러의 예제 가격 책정을 보여 줍니다.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>가격 계산기를 사용하여 온라인으로 비용 예측

[가격 계산기](https://azure.microsoft.com/pricing/calculator/)를 확인하여 관심 있는 서비스의 월별 비용을 예측하세요. 첫 번째 파티 Azure 리소스를 추가 하 여 예상 비용을 얻을 수 있습니다. 가격 계산기에서 통화 유형을 변경할 수 있습니다.

![가격 계산기 메뉴의 스크린샷](./media/billing-getting-started/pricing-calc.png)

예를 들어 가격 계산기에서 A1 Windows VM (가상 머신)은 전체 시간을 실행 상태로 유지 하는 경우 계산 시간에 특정 금액/월을 비용으로 예상 합니다.

![A1 Windows VM에 대 한 월별 예상 비용을 보여 주는 가격 계산기의 스크린샷](./media/billing-getting-started/pricing-calcvm.png)

가격 책정에 대 한 자세한 내용은 [가격 책정 FAQ](https://azure.microsoft.com/pricing/faq/)를 참조 하세요. Azure 영업 사원의 의견을 보내려면 FAQ 페이지의 맨 위에 표시 되는 전화 번호를 호출 합니다.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Azure Portal의 예상 비용 검토

일반적으로 Azure Portal에 서비스를 추가 하는 경우 청구 된 통화에서 월별 예상 비용을 보여 주는 보기가 있습니다. 예를 들어 Windows VM의 크기를 선택하는 경우 컴퓨팅 시간에 대해 예상되는 월별 비용이 표시됩니다.

![예: 월별 예상 비용을 보여 주는 A1 Windows VM](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a> 지출 한도가 설정되어 있는지 확인

크레딧을 사용하는 구독이 있는 경우 기본적으로 지출 한도가 켜져 있습니다. 이러한 방식으로 모든 크레딧을 지출하면 신용 카드에 요금이 청구되지 않습니다. [전체 Azure 제품 목록 및 지출 한도 가용성](https://azure.microsoft.com/support/legal/offer-details/)을 참조하세요.

그러나 지출 한도에 도달 하면 서비스를 사용할 수 없게 됩니다. 즉, VM이 할당 취소됩니다. 서비스 가동 중지를 방지하려면 지출 한도를 해제해야 합니다. 초과분이 파일의 신용 카드에 부과됩니다.

지출 한도가 있는지 확인 하려면 [계정 센터의 구독 보기](https://account.windowsazure.com/Subscriptions)로 이동 합니다. 지출 한도가 다음과 같이 설정 되어 있으면 배너가 표시 됩니다.

![계정 센터에서 지출 한도가 설정되어 있다는 사실에 대한 경고를 표시하는 스크린 샷](./media/billing-getting-started/spending-limit-banner.png)

배너를 클릭 하 고 프롬프트에 따라 지출 한도를 제거 합니다. 등록 시 신용 카드 정보를 입력하지 않은 경우 지출 한도를 제거하려면 해당 정보를 입력해야 합니다. 자세한 내용은 [Azure 지출 한도 – 작동 방식 및 사용 또는 제거하는 방법](https://azure.microsoft.com/pricing/spending-limits/)을 참조하세요.

## <a name="use-budgets-and-cost-alerts"></a>예산 및 비용 경고 사용

[예산을](../cost-management/tutorial-acm-create-budgets.md) 만들어 비용을 관리 하 고 관련자에 게 잘못 된 지출 및 과도 한 지출 위험을 자동으로 알리는 [경고](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) 를 만들 수 있습니다. 경고는 예산 및 비용 임계값에 비해 지출에 따라 결정 됩니다.

## <a name="monitor-costs-when-using-azure-services"></a>Azure 서비스를 사용 하는 경우 비용 모니터링
다음 도구를 사용 하 여 비용을 모니터링할 수 있습니다.

- Tags
- 비용 분석 및 진행 율
- 비용 분석

### <a name="tags"></a>리소스에 태그를 추가 하 여 청구 데이터 그룹화

태그를 사용하여 지원되는 서비스에 대한 청구 데이터를 그룹화할 수 있습니다. 예를 들어 다른 팀에 대해 여러 Vm을 실행 하는 경우 태그를 사용 하 여 비용 센터 별로 비용을 분류할 수 있습니다. 예를 들면 다음과 같습니다. HR, 마케팅, 재무 등) 또는 환경 (예: 프로덕션, 사전 프로덕션, 테스트).

![포털의 태그 설정을 보여 주는 스크린 샷](./media/billing-getting-started/tags.png)

태그는 여러 다른 비용 보고 보기에 표시됩니다. 예를 들어, 첫 번째 청구 기간 후에는 세부 사용 현황 CSV 파일에서 바로 [비용 분석 보기](#costs) 에 표시 됩니다.

자세한 내용은 [태그를 사용하여 Azure 리소스 구성](../azure-resource-manager/resource-group-using-tags.md)을 참조하세요.

### <a name="costs"></a>비용 분석 및 진행 속도로 모니터링

Azure 서비스를 실행 한 후 요금을 정기적으로 확인 합니다. Azure Portal에서 현재 지출 및 진행 속도가 표시 됩니다.

1. [Azure Portal의 구독](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)을 방문하여 구독을 선택합니다.

2. 구독에 대해 지원 되는 경우 비용 분석 및 진행 비율이 표시 됩니다.

    ![Azure Portal의 진행 속도 및 분석 스크린 샷](./media/billing-getting-started/burn-rate.PNG)

3. 왼쪽 목록에서 [비용 분석](../cost-management/quick-acm-cost-analysis.md) 을 클릭 하 여 자원별 비용 분석을 확인 합니다. 서비스를 추가한 후 24 시간 동안 데이터가 표시 될 때까지 기다립니다.

    ![Azure Portal의 비용 분석 보기의 스크린 샷](./media/billing-getting-started/cost-analysis.png)

4. [태그](#tags), 리소스 종류, 리소스 그룹 및 시간 간격 등의 다양한 속성별로 필터링할 수 있습니다. 보기를 쉼표로 구분 된 값 (.csv) 파일로 내보내려는 경우 **적용** 을 클릭 하 여 필터를 확인 하 고 **다운로드** 합니다.

5. 또한 리소스를 클릭하여 매일 사용 기록과 일일 리소스 비용을 확인할 수 있습니다.

    ![Azure Portal의 사용 기록 보기 스크린 샷](./media/billing-getting-started/costhistory.png)

서비스를 선택할 때 표시 되는 추정치와 표시 되는 비용을 비교 합니다. 비용이 예상 비용과 크게 다른 경우 리소스에 대해 선택한 가격 책정 계획을 확인 합니다.

## <a name="optimize-and-reduce-costs"></a>비용 최적화 및 절감
Cost management의 원칙에 익숙하지 않은 경우 Azure Cost Management를 사용 하 [여 클라우드 투자를 최적화 하는 방법](../cost-management/cost-mgt-best-practices.md)을 참조 하세요.

Azure Portal에서 Vm 및 Advisor 권장 사항에 대 한 자동 종료를 사용 하 여 Azure 비용을 최적화 하 고 줄일 수도 있습니다.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Vm에 대 한 자동 종료와 같은 비용 절감 기능 고려

시나리오에 따라 Azure Portal에서 VM에 대한 자동 종료를 구성할 수 있습니다. 자세한 내용은 [Azure Resource Manager를 사용한 VM에 대한 자동 종료](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)를 참조하세요.

![포털의 자동 종료 옵션 스크린샷](./media/billing-getting-started/auto-shutdown.png)

자동 종료는 전원 옵션을 사용하여 VM 내에서 종료할 때와는 다릅니다. 자동 종료는 VM을 중지한 후 할당 취소하여 추가 사용 요금 부과를 중지합니다. 자세한 내용은 VM 상태에 대한 [Linux VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) 및 [Windows VM](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)의 가격 책정 FAQ를 참조하세요.

개발 및 테스트 환경에 대한 추가 비용 절감에 기능에 대해서는 [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)를 확인하세요.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Advisor 권장 사항을 설정 하 고 검토 합니다.

[Azure Advisor](../advisor/advisor-overview.md) 를 사용 하면 사용량이 낮은 리소스를 식별 하 여 비용을 절감할 수 있습니다. Azure Portal에서 Advisor를 방문합니다.

![Azure Portal의 Azure Advisor 단추 스크린 샷](./media/billing-getting-started/advisor-button.png)

Advisor 대시보드의 **비용** 탭에서 조치 가능한 권장 사항을 얻을 수 있습니다.

![Advisor cost 권장 지침 예제 스크린 샷](./media/billing-getting-started/advisor-action.png)

비용 절약 Advisor 권장 사항에 대 한 단계별 자습서는 [권장 구성에서 비용 최적화](../cost-management/tutorial-acm-opt-recommendations.md) 자습서를 검토 합니다.

## <a name="review-costs-against-your-latest-invoice"></a>최신 청구서에 대 한 비용 검토

청구 주기가 끝날 때 최신 청구서를 사용할 수 있습니다. 또한 [청구서와 자세한 사용 현황 파일을 다운로드](billing-download-azure-invoice-daily-usage-date.md) 하 여 적절 하 게 요금이 청구 되는지 확인할 수 있습니다. 청구서에서 일간 사용 현황 비교에 대한 자세한 내용은 [Microsoft Azure 청구서 이해](billing-understand-your-bill.md)를 참조하세요.

### <a name="billing-api"></a>청구 API

Azure 청구 API를 사용 하 여 프로그래밍 방식으로 사용 현황 데이터를 가져옵니다. RateCard API 및 사용 현황 API를 함께 사용하여 청구된 사용량을 확인합니다. 자세한 내용은 [Microsoft Azure 리소스 소비에 대한 통찰력 얻기](billing-usage-rate-card-overview.md)를 참조하세요.

## <a name="other-offers"></a> 추가 리소스 및 특수 사례

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP 및 스폰서쉽 고객
계정 관리자 또는 Azure 파트너에 시작하도록 알립니다.

| 제공 | 리소스 |
|-------------------------------|-----------------------------------------------------------------------------------|
| EA(기업 계약) | [EA 포털](https://ea.azure.com/), [도움말 문서](https://ea.azure.com/helpdocs) 및 [Power BI 보고서](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| CSP(클라우드 솔루션 공급자) | 공급자에게 알림 |
| Azure 스폰서쉽 | [스폰서쉽 포털](https://www.microsoftazuresponsorships.com/) |

대기업의 IT를 관리하는 경우 [Azure 엔터프라이즈 스 캐폴드](/azure/architecture/cloud-adoption-guide/subscription-governance) 및 [엔터프라이즈 IT 백서](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf)(.pdf 다운로드, 영어 버전만 제공)를 읽어보세요.

#### <a name="EA"></a>Azure Portal에서 비용 보기 기업계약

엔터프라이즈 비용 보기는 현재 공개 미리 보기 상태입니다. 참고할 항목:

- 구독 비용은 사용을 기반으로 하며 사전에 지불한 금액, 초과분, 포함된 수량, 조정 및 세금을 포함하지 않습니다. 실제 요금은 등록 수준에서 계산됩니다.
- Azure Portal 내에 표시되는 금액은 엔터프라이즈 포털에 있는 금액과 다를 수 있습니다. 엔터프라이즈 포털의 업데이트는 Azure Portal에 변경 내용이 표시되기 전에 몇 분 정도 걸릴 수 있습니다.
- 비용이 표시되지 않는 경우 다음 이유 중 하나에 해당할 수 있습니다.
    - 구독 수준에서 권한이 없습니다. Enterprise 비용 보기를 표시하려면 구독 수준에서 청구 읽기 권한자, 읽기 권한자, 참가자 또는 소유자여야 합니다.
    - 사용자는 계정 소유자이며 등록 관리자가 “AO 보기 요금” 설정을 비활성화했습니다.  등록 관리자에게 비용에 대한 액세스 권한을 문의하세요.
    - 사용자가 부서 관리자 이며 등록 관리자가 **DA 보기 요금** 설정을 사용 하지 않도록 설정 했습니다.  등록 관리자에게 액세스 권한을 문의하세요.
    - 채널 파트너를 통해 Azure를 구매했고 파트너가 가격 정보를 릴리스하지 않았습니다.  
- 엔터프라이즈 포털에서 비용 액세스와 관련된 설정을 업데이트하는 경우 Azure Portal에 변경 내용이 표시되기 전에 몇 분 정보 지연됩니다.
- 지출 한도 및 송장 지침은 EA 구독에 적용되지 않습니다.

### <a name="check-your-subscription-and-access"></a>구독 및 액세스 권한 확인

비용을 보려면 [청구 정보에 대한 구독 수준 액세스 권한](billing-manage-access.md)이 있어야 합니다. 계정 관리자만 [계정 센터](https://account.azure.com/Subscriptions)에 액세스하고, 청구 정보를 변경하고, 구독을 관리할 수 있습니다. 계정 관리자는 가입 프로세스를 거친 사용자입니다. 자세한 내용은 [구독 또는 서비스를 관리하는 Azure 관리자 역할 추가 또는 변경](billing-add-change-azure-subscription-administrator.md)을 참조하세요.

계정 관리자인지 확인하려면 [Azure Portal의 구독](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)으로 이동합니다. 구독 목록을 확인 하 고 **내 역할**을 찾습니다. *계정 관리자* 가 역할인 경우에는 모든 권한이 부여 됩니다. *Owner*와 같이 다른 항목이 표시 되는 경우에는 모든 권한이 없습니다.

![Azure Portal의 구독 보기에 표시되는 역할의 스크린 샷](./media/billing-getting-started/sub-blade-view.PNG)

구독을 관리하고 청구 정보를 변경하려면 [계정 관리자를 찾습니다](billing-subscription-transfer.md#whoisaa). 계정 관리자에 게 작업을 완료 하거나 [구독을 양도](billing-subscription-transfer.md)하도록 요청 하세요.

계정 관리자가 더 이상 조직에 있지 않은 상태에서 청구를 관리해야 하는 경우에는 [문의하세요](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>서비스 인시던트에 대 한 Service Level Agreement(서비스 수준 약정) 크레딧 요청

SLA(서비스 수준 계약)에서는 작동 시간 및 연결에 대한 Microsoft의 정책을 설명합니다. 서비스 인시던트는 Azure 서비스에서 작동 시간 또는 연결에 영향을 주는 문제가 발생 하는 경우 (종종 *중단*이라고 함) 보고 됩니다. SLA에 설명 된 대로 각 서비스에 대 한 서비스 수준을 달성 하 고 유지 관리 하지 않는 경우 월간 서비스 요금 중 일부에 대 한 크레딧에 대 한 자격이 있을 수 있습니다.

크레딧을 요청 하려면:

1. [Azure 포털](https://portal.azure.com/)에 로그인합니다. 계정이 여러 개인 경우 Azure 가동 중지 시간의 영향을 받는 계정을 사용 해야 합니다. 
2. 새 지원 요청을 만듭니다.
3. **문제 유형**에서 **청구**를 선택 합니다.
4. **문제 유형**에서 **환불 요청**을 선택 합니다.
5. 세부 정보를 추가 하 여 SLA 크레딧을 요청 함을 지정 하 고, 날짜/시간/시간 영역 뿐만 아니라 영향을 받는 서비스 (Vm, 웹 사이트 등)를 언급 합니다.
6. 연락처 세부 정보를 확인 하 고 **만들기** 를 선택 하 여 요청을 제출 합니다.

SLA 임계값은 서비스에 따라 다릅니다. 예를 들어 SQL 웹 계층의 SLA는 99.9%이 고, Vm의 SLA는 99.95%이 고, SQL 표준 계층에는 99.99%의 SLA가 있습니다.

일부 서비스의 경우 SLA를 적용 하기 위한 필수 구성 요소가 있습니다. 예를 들어 가상 컴퓨터에는 동일한 가용성 집합에 배포 된 두 개 이상의 인스턴스가 있어야 합니다.

자세한 내용은 [서비스 수준 계약](https://azure.microsoft.com/support/legal/sla/) 및 [Azure 서비스에 대 한 SLA 요약](https://azure.microsoft.com/support/legal/sla/summary/) 설명서를 참조 하세요.

## <a name="need-help-contact-us"></a>도움이 필요하십니까? 문의하세요.

궁금한 사항이 있거나 도움이 필요 하면 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계
- [지출 한도](billing-spending-limit.md) 를 사용 하 여 과잉 낭비를 방지 하는 방법을 알아봅니다.
- [Azure 비용 분석](../cost-management/quick-acm-cost-analysis.md)을 시작 합니다.
