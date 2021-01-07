---
title: Azure 애플리케이션 제안을 테스트 하 고 게시 하는 방법
description: 파트너 센터를 사용 하 여 Azure 응용 프로그램 제품을 미리 보기, 제품 미리 보기, 테스트 한 다음 Microsoft 상용 marketplace에 게시 합니다.
author: aarathin
ms.author: aarathin
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 11/06/2020
ms.openlocfilehash: 461f9354bb3a6eae0af186de8fe9f39c6b5fff2c
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620922"
---
# <a name="how-to-test-and-publish-an-azure-application-offer"></a>Azure 애플리케이션 제안을 테스트 하 고 게시 하는 방법

이 문서에서는 파트너 센터를 사용 하 여 게시, 제품 미리 보기, 테스트를 위한 Azure 애플리케이션 제품을 제출 하 고 상용 marketplace에 게시 하는 방법을 설명 합니다. 게시 하려는 제품이 이미 만들어져 있어야 합니다.

## <a name="submit-your-offer-for-publishing"></a>게시를 위해 제품 제출

1. [파트너 센터](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)에서 상용 마켓플레이스 대시보드에 로그인 합니다.
1. **개요** 페이지에서 게시 하려는 제품을 선택 합니다.
1. 포털의 오른쪽 위 모서리에서 **검토 및 게시** 를 선택 합니다.
1. 각 페이지의 **상태** 열에 **완료** 가 표시 되는지 확인 합니다. 가능한 세 가지 상태는 다음과 같습니다.
    - **시작 되지 않음** -페이지가 완전 하지 않습니다.
    - **불완전** -페이지에 필요한 정보가 없거나 수정 해야 하는 오류가 있습니다. 페이지로 돌아가서 업데이트 해야 합니다.
    - **완료** – 페이지가 완료 되었습니다. 필요한 모든 데이터가 제공 되었으며 오류가 없습니다.
1. 페이지의 상태가 **완료** 가 아닌 경우 페이지 이름을 선택 하 고 문제를 수정한 다음 페이지를 저장 하 고 **검토 및 게시** 를 다시 선택 하 여이 페이지로 돌아갑니다.
1. 모든 페이지가 완료 되 면 **인증에 대 한 정보** 상자에서 인증 팀에 테스트 지침을 제공 하 여 앱이 제대로 테스트 되었는지 확인 합니다. 앱을 이해 하는 데 도움이 되는 보조 노트를 제공 합니다.
1. 제품에 대 한 게시 프로세스를 시작 하려면 **게시** 를 선택 합니다. **제품 개요** 페이지가 표시 되 고 제품의 **게시 상태가** 표시 됩니다.

제품의 게시 상태는 게시 프로세스를 통해 이동할 때 변경 됩니다. 이 프로세스에 대 한 자세한 내용은 [유효성 검사 및 게시 단계](review-publish-offer.md#validation-and-publishing-steps)를 참조 하세요.

## <a name="preview-and-test-your-offer"></a>제품 미리 보기 및 테스트

사용자가 로그 오프할 준비가 되 면 제품 미리 보기를 검토 하 고 승인 하도록 요청 하는 전자 메일을 보냅니다. 브라우저에서 **제품 개요** 페이지를 새로 고쳐 제품이 게시자 로그 오프 단계에 도달 했는지 확인할 수도 있습니다. 있는 경우 **라이브 이동** 단추 및 미리 보기 링크를 사용할 수 있습니다. Microsoft를 통해 제품을 판매 하도록 선택한 경우 미리 보기 대상에 추가 된 모든 사용자가 제품의 획득 및 배포를 테스트 하 여이 단계에서 요구 사항을 충족 하는지 확인할 수 있습니다.

다음 스크린샷에서는 **라이브 이동** 단추 아래에 두 개의 미리 보기 링크를 사용 하 여 제품에 대 한 **제품 개요** 페이지를 보여 줍니다. 이 페이지에 표시 되는 유효성 검사 단계는 제품을 만들 때 선택한 내용에 따라 달라 집니다.

[![파트너 센터의 제품에 대 한 제품 개요 페이지를 보여 줍니다. 라이브 이동 단추와 미리 보기 링크가 표시 됩니다.](media/create-new-azure-app-offer/azure-app-publish-status.png)](media/create-new-azure-app-offer/azure-app-publish-status.png#lightbox)

다음 단계를 사용 하 여 제품을 미리 봅니다.

1. **제품 개요** 페이지에서 **라이브 이동** 단추 아래의 미리 보기 링크를 선택 합니다. 

1. 종단 간 구매 및 설정 흐름의 유효성을 검사 하려면 미리 보기로 제공 되는 동안 제품을 구매 하세요. 먼저 Microsoft에 [지원 티켓](https://aka.ms/marketplacesupport) 을 알리고 요금을 처리 하지 않도록 합니다.

1. Azure 응용 프로그램이 [상업적 marketplace 계량 서비스를 사용 하 여 요금제 요금제](./partner-center-portal/azure-app-metered-billing.md)를 지 원하는 경우 [Marketplace 요금제 청구 api](./partner-center-portal/marketplace-metering-service-apis.md#development-and-testing-best-practices)에 자세히 설명 된 테스트 모범 사례를 검토 하 고 따릅니다.

1. 제품을 미리 보고 테스트 한 후에 변경 해야 하는 경우 새 미리 보기를 게시 하도록 편집 하 고 다시 제출할 수 있습니다. 자세한 내용은 [상용 marketplace에서 기존 제품 업데이트](./partner-center-portal/update-existing-offer.md)를 참조 하세요.

## <a name="publish-your-offer-live"></a>제품을 라이브 게시

미리 보기에서 모든 테스트를 완료 한 후 **Live로 전환** 을 선택 하 여 제품을 상업적 marketplace에 게시 합니다.

   > [!TIP]
   > 제품이 이미 상업적 marketplace에 있는 경우 **Live go** 를 선택할 때까지 모든 업데이트가 라이브 상태로 전환 되지 않습니다.

이제 상업적 marketplace에서 제품을 사용할 수 있도록 선택 했으므로 제품의 미리 보기 버전과 마찬가지로 live 제품이 구성 되도록 일련의 최종 유효성 검사를 수행 합니다. 이러한 유효성 검사에 대 한 자세한 내용은 [게시 단계](review-publish-offer.md#publish-phase)를 참조 하세요.

이러한 유효성 검사를 완료 한 후에는 제품이 marketplace에 살고 있습니다.

### <a name="errors-and-review-feedback"></a>오류 및 검토 피드백

게시 프로세스의 **수동 유효성 검사** 단계는 제품에 대 한 광범위 한 검토를 나타내며 관련 기술 자산 (특히 Azure Resource Manager 템플릿) 문제는 일반적으로 PR (끌어오기 요청) 링크로 표시 됩니다. 이러한 PR을 보고 응답하는 방법에 대한 설명은 [검토 피드백 처리](partner-center-portal/azure-apps-review-feedback.md)를 참조하세요.

하나 이상의 게시 단계에서 오류가 발생하면 제품을 게시하기 전에 해당 오류를 수정합니다.

## <a name="next-step"></a>다음 단계

- [파트너 센터에서 상업적 marketplace에 대 한 분석 보고서 액세스](partner-center-portal/analytics.md)
- Csp 프로그램을 통해 Microsoft 및 재판매와 공동 판매를 통해 [Azure 애플리케이션 제품을 판매 하는 방법을](create-new-azure-apps-offer-marketing.md) 알아봅니다.
