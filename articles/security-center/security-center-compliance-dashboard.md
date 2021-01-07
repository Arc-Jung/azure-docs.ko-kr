---
title: '자습서: 규정 준수 검사 - Azure Security Center'
description: '자습서: Azure Security Center를 사용하여 규정 준수를 개선하는 방법을 알아봅니다.'
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5f50c4dc-ea42-418d-9ea8-158ffeb93706
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2020
ms.author: memildin
ms.openlocfilehash: bbc36dbb2a17d379d31a9a235898500aea36247d
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96533913"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>자습서: 규정 준수 개선

Azure Security Center를 통해 **규정 준수 대시보드** 를 사용하여 규정 준수 요구 사항을 충족하기 위한 프로세스를 간소화할 수 있습니다. 대시보드에서 Security Center는 Azure 환경의 지속적인 평가에 기반하여 준수 상태에 대한 인사이트를 제공합니다. Security Center는 보안 모범 사례에 따라 하이브리드 클라우드 환경에서 위험 요소를 분석합니다. 이러한 평가는 지원되는 표준 세트에서 준수 제어에 매핑됩니다. 규정 준수 대시보드를 통해 특정 표준 또는 규정의 컨텍스트에서 사용자 환경 내의 이러한 모든 평가의 상태를 확인할 수 있습니다. 권장 사항에 따라 작업하고 환경에서 위험 요소를 줄임에 따라 규정 준수 상태가 개선됩니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 규정 준수 대시보드를 사용하여 규정 준수 평가
> * 권장 사항에 따라 작업을 수행하여 준수 상태를 개선합니다

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/)을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서에서 설명하는 기능을 단계별로 실행하려면 다음을 수행합니다.

- [Azure Defender](azure-defender.md)를 사용하도록 설정해야 합니다. 30일 동안 무료로 Azure Defender를 사용해 볼 수 있습니다.
- 정책 규정 준수 데이터에 대한 리더 액세스 권한이 있는 계정으로 로그인해야 합니다(**보안 리더** 로는 충분하지 않음). 구독에 대한 **글로벌 리더** 의 역할이 작동합니다. 최소한 **리소스 정책 기여자** 및 **보안 관리자** 역할이 할당되어야 합니다.

##  <a name="assess-your-regulatory-compliance"></a>규정 준수 평가

Security Center는 지속적으로 리소스 구성을 평가하여 보안 문제 및 취약성을 식별합니다. 이러한 평가는 보안 예방 조치를 향상시키는 데 중점을 두는 권장 사항으로 표시됩니다. 규정 준수 대시보드에서 지원되는 요구 사항이 적용 가능한 보안 평가에 매핑되는 모든 해당 요구 사항이 있는 규정 준수 표준 세트를 볼 수 있습니다. 이를 통해 이러한 평가의 상태를 기반으로 하는 표준에 대한 준수 상태를 볼 수 있습니다.

규정 준수 대시보드 보기는 사용자에게 중요한 표준 또는 규정에 따라 차이에 주목하는 데 도움이 될 수 있습니다. 이 초점이 맞춰진 보기를 사용하면 동적 클라우드 및 하이브리드 환경 내에서 시간이 지남에 따라 준수 점수를 지속적으로 모니터링할 수 있습니다.

>[!NOTE]
> 기본적으로 Security Center는 다음과 같은 규정 표준을 지원합니다. Azure CIS, PCI DSS 3.2, ISO 27001 및 SOC TSP 
>
> [동적 준수 패키지(미리 보기)](update-regulatory-compliance-packages.md) 기능을 사용하여 규정 준수 대시보드에 표시되는 표준을 새 *동적* 패키지로 업그레이드할 수 있습니다. 또한 동일한 미리 보기 기능을 사용하여 새 규정 준수 패키지를 추가하고 추가 표준 준수를 모니터링할 수도 있습니다. 

1. Security Center 메뉴에서 **규정 준수** 를 선택합니다. <br>
화면 맨 위에 지원되는 규정 준수 세트로 준수 상태의 개요가 있는 대시보드가 표시됩니다. 전반적인 준수 점수 및 각 표준과 관련된 통과와 실패 평가의 수를 볼 수 있습니다.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="규정 준수 대시보드":::

1. (1)과 관련된 규정 준수 표준에 대한 탭을 선택합니다. 표준이 (2)에 적용되는 구독 및 해당 표준 (3)에 대한 모든 컨트롤의 목록이 표시됩니다. 해당 컨트롤의 경우 해당 컨트롤 (4)와 관련된 통과 및 실패 평가의 세부 정보와 영향을 받는 리소스 수 (5)를 볼 수 있습니다. 일부 컨트롤은 회색으로 표시됩니다. 이러한 컨트롤에는 연결된 Security Center 평가가 없습니다. 직접 이에 대한 요구 사항을 분석하고 사용자 환경에서 평가하십시오. 일부는 프로세스와 관련되며 기술적이지 않습니다.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="특정 표준 규정 준수에 대한 세부 정보 살펴보기":::

1. 특정 표준에 대한 현재 규정 준수 상태를 요약하는 PDF 보고서를 생성하고 다운로드하려면 **보고서 다운로드** 를 클릭합니다.

    이 보고서는 Security Center 평가 데이터를 기반으로 선택한 표준의 규정 준수 상태에 대한 요약 정보를 제공하며, 해당 표준의 컨트롤에 따라 구성됩니다. 이 보고서는 관련자와 공유할 수 있으며, 내부 및 외부 감사자에게 증거 자료를 제공할 수 있습니다.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="규정 준수 보고서 다운로드":::

## <a name="improve-your-compliance-posture"></a>준수 상태 개선

규정 준수 대시보드에서 정보를 지정하여 대시보드 내에서 직접 권장 사항을 해결하여 규정 준수 상태를 개선할 수 있습니다.

1.  대시보드에 표시되는 실패 평가를 클릭하여 해당 권장 사항에 대한 세부 정보를 봅니다. 각 권장 사항에는 문제를 해결하기 위해 수행해야 하는 수정 단계 세트가 포함되어 있습니다.

1.  특정 리소스를 선택하여 자세한 세부 정보를 보고 해당 리소스에 대한 권장 사항을 해결할 수 있습니다. <br>예를 들어 **Azure CIS 1.1.0(신규) 표준** 에서 **가상 머신에 디스크 암호화를 적용해야 함** 권장 사항을 선택할 수 있습니다.

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="표준에서 권장 사항을 선택하면 권장 사항 세부 정보 페이지로 바로 연결됩니다.":::

1. 이 예제에서는 권장 사항 세부 정보 페이지에서 **작업 수행** 을 선택하면 Azure Portal의 Azure Virtual Machine 페이지가 표시됩니다. 이 페이지에서 **보안** 탭을 열고 암호화를 사용하도록 설정할 수 있습니다.

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="권장 사항 세부 정보 페이지의 작업 가져오기 단추를 클릭하면 수정 옵션으로 연결됩니다.":::

    권장 사항을 적용하는 방법에 대한 자세한 내용은 [Azure Security Center에서 보안 권장 사항 구현](security-center-recommendations.md)을 참조하세요.

1.  권장 사항을 해결하기 위한 작업을 수행한 후 준수 점수가 개선되므로 준수 대시보드 보고서에 영향이 나타납니다.

    > [!NOTE]
    > 평가는 약 12시간마다 실행되므로 다음 관련 평가가 실행된 후에만 규정 준수 데이터에 미치는 영향이 표시됩니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Security Center의 규정 준수 대시보드를 사용하여 다음을 수행하는 방법을 알아보았습니다.

-   사용자에게 중요한 표준 및 규정에 대한 준수 상태 보기 및 모니터링
-   관련 권장 사항을 해결하고 준수 점수 개선을 확인하여 준수 상태 개선

규정 준수 대시보드는 규정 준수 프로세스를 크게 간소화하고, Azure 및 하이브리드 환경에 대한 규정 준수 증거를 수집하는 데 필요한 시간을 크게 절감할 수 있습니다.

자세한 내용은 다음 관련 문서를 참조하세요.

-   [규정 준수 대시보드(미리 보기)에서 동적 규정 준수 패키지로 업데이트](update-regulatory-compliance-packages.md) - 규정 준수 대시보드에 표시되는 표준을 새 *동적* 패키지로 업데이트할 수 있는 이 미리 보기 기능에 대해 알아봅니다. 또한 동일한 미리 보기 기능을 사용하여 새 규정 준수 패키지를 추가하고 추가 표준 준수를 모니터링할 수도 있습니다. 
-   [Azure Security Center에서 보안 상태 모니터링](security-center-monitoring.md) - Azure 리소스의 상태를 모니터링하는 방법을 알아봅니다.
-   [Azure Security Center에서 보안 권장 사항 관리](security-center-recommendations.md) - Azure 리소스 보호에 도움이 되도록 Azure Security Center에서 권장 사항을 사용하는 방법을 알아봅니다.
-   [Azure Security Center에서 보안 점수 향상](secure-score-security-controls.md) - 보안 상태를 최대한 향상시키기 위해 취약성 및 보안 추천 사항의 우선 순위를 지정하는 방법을 알아봅니다.
