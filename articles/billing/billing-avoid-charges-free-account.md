---
title: Azure 체험 계정으로 요금 청구 방지
description: Azure 무료 계정에 요금이 표시되는 이유를 이해합니다. 이러한 요금을 방지하는 방법을 알아봅니다.
author: amberbhargava
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: ac5bcf0a4dd9c7ee593c70a63509a21f28ba1456
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67491469"
---
# <a name="avoid-charges-with-your-azure-free-account"></a>Azure 체험 계정으로 요금 청구 방지

Azure 체험 계정은 처음 30일 동안 $200의 Azure 크레딧과 12개월 동안 무료 서비스라는 제한된 수량을 제공합니다. 자세한 내용은 [Azure 체험 계정](https://azure.microsoft.com/free/)을 참조하세요. 크레딧 상태에 따라 크레딧을 사용하거나 체험 서비스 및 수량을 초과한 사용량에 대한 요금이 청구될 수 있습니다.

## <a name="azure-free-account-might-use-account-credit"></a>Azure 체험 계정은 계정 크레딧을 사용할 수 있습니다.
아직 만료되지 않은 Azure 크레딧이 있는 경우 Azure에서는 체험 서비스 및 수량을 초과한 사용량에 대한 요금을 지불하기 위해 크레딧을 사용합니다.

## <a name="your-credit-runs-out-or-is-expired"></a>크레딧이 소진되었거나 만료되었습니다.
크레딧이 부족하거나 30일이 만료된 경우 Azure는 구독을 비활성화합니다. Azure 서비스를 계속 사용하려면 구독을 종량제 요금을 사용하는 개별 구독으로 업그레이드해야 합니다. 자세한 내용은 [평가판 Azure 구독 업그레이드](billing-upgrade-azure-subscription.md)를 참조하세요. 업그레이드한 후에도 구독은 12개월 동안 체험 서비스에 액세스할 수 있습니다. 체험 서비스 및 수량을 초과한 사용량에 대해서만 요금이 청구됩니다.

Azure 무료 계정에 요금이 부과되는 몇 가지 이유를 알아보겠습니다.

### <a name="usage-exceeds-the-limits-of-free-services"></a>사용량이 평가판 서비스 한도를 초과했습니다.

Azure 무료 계정으로 매월 제한된 양의 무료 서비스를 얻습니다. 사용 가능한 양은 해당 월이 끝날 때 만료되고 다음 달로 이월되지 않습니다. 예를 들어 매월 5GB의 File Storage를 얻는다고 가정합니다. 한 달에 2GB만 사용할 경우 나머지 3GB는 다음 달로 이월되지 않습니다. 요금이 청구되는 것을 방지하려면 사용량을 한도 내로 유지합니다. 평가판 서비스의 한도에 대해 알아보려면 [Azure 체험 계정 FAQ](https://azure.microsoft.com/free/free-account-faq/)를 참조하세요. 무료 서비스 사용량을 확인하려면 [Azure 무료 계정에 포함된 무료 서비스 사용량 확인](billing-check-free-service-usage.md)을 참조하세요.

### <a name="some-services-are-not-free"></a>일부 서비스는 무료가 아닙니다.

Azure 체험 계정에 체험용으로 포함되지 않은 서비스를 사용하면 종량제 요금이 청구됩니다. 체험 계정에 포함되는 서비스에 대해 알아보려면 [Azure 체험 계정 FAQ](https://azure.microsoft.com/free/free-account-faq/)를 참조하세요. Azure Portal 또는 Azure 사용량 파일에서 서비스 사용량을 확인할 수 있습니다. 자세한 내용은 [Portal에서 비용 분석 및 진행 속도를 정기적으로 확인](billing-getting-started.md#costs) 및 [계정 센터에서 사용량 다운로드](billing-download-azure-invoice-daily-usage-date.md)를 참조하세요.

### <a name="you-reached-the-end-of-your-free-12-months"></a>12개월 무료 사용 기간이 만료되었습니다.

체험 서비스 및 수량이 12개월 후에 만료됩니다. Azure Portal에서 무료 서비스가 만료되는 시기를 확인할 수 있습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.

2. 왼쪽 탐색 영역에서 **모든 서비스**를 선택합니다.

3.  **구독**을 선택합니다.

4.  무료 계정을 등록할 때 만든 구독을 선택합니다.

5.  아래로 스크롤하여 체험 서비스 그리드를 찾습니다. 그리드의 왼쪽 위에 있는 도구 설명을 클릭합니다.

![체험 계정 혜택 만료 시기를 확인할 수 있는 위치를 보여주는 스크린샷](./media/billing-avoid-charges-free-account/freeaccount-benefits-expiration-date.png)


체험 서비스 및 수량이 만료되면 Azure에서는 사용자에게 사용 중인 모든 서비스에 대해 종량제 요금을 청구합니다. Azure Portal을 사용하여 사용하지 않는 서비스에 대한 리소스를 삭제할 수 있습니다. 모든 Azure 서비스를 사용하지 않으려는 경우 [구독을 취소](billing-how-to-cancel-azure-subscription.md)할 수 있습니다.

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계
- [평가판 Azure 구독 업그레이드](billing-upgrade-azure-subscription.md)
