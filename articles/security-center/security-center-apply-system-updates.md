---
title: Azure Security Center의 시스템 업데이트 적용 | Microsoft Docs
description: 이 문서에서는 Azure Security Center 권장 사항 **시스템 업데이트 적용** 및 **시스템 업데이트 후 다시 부팅**을 구현하는 방법을 보여 줍니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 1688e85c6e6ed57892ccdffdf0813c8628127cc5
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202453"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Azure Security Center의 시스템 업데이트 적용
Azure Security Center는 매일 Windows 및 Linux VM(가상 머신)과 컴퓨터에서 누락된 운영 체제 업데이트를 모니터링합니다. Security Center는 Windows 컴퓨터에 구성된 서비스에 따라 Windows 업데이트 또는 WSUS(Windows Server Update Services)에서 사용 가능한 보안 및 중요 업데이트의 목록을 검색합니다. 보안 센터는 또한 Linux 시스템에서 최신 업데이트를 확인합니다. VM 또는 컴퓨터에 누락된 시스템 업데이트가 있으면 Security Center는 시스템 업데이트를 적용하는 것이 좋다는 메시지를 표시합니다.

## <a name="implement-the-recommendation"></a>권장 사항 구현
시스템 업데이트 적용은 Security Center에서 권장 사항으로 표시됩니다. VM 또는 컴퓨터에 누락된 시스템 업데이트가 있으면 **권장 사항**과 **컴퓨팅** 아래에 이 권장 사항이 표시됩니다.  권장 사항을 선택하면 **시스템 업데이트 적용** 대시보드가 열립니다.

이 예제에서는 **컴퓨팅**을 사용합니다.

1. Security Center 주 메뉴 아래에서 **컴퓨팅**을 선택합니다.

   ![컴퓨팅 선택][1]

2. **컴퓨팅** 아래에서 **시스템 업데이트 누락**을 선택합니다. **시스템 업데이트 적용** 대시보드가 열립니다.

   ![시스템 업데이트 적용 대시보드][2]

   대시보드 위쪽에는 다음 정보가 제공됩니다.

    - 시스템 업데이트가 누락된 Windows 및 Linux VM과 컴퓨터의 총 수
    - VM과 컴퓨터 전체에서 누락된 중요 업데이트의 총 수
    - VM과 컴퓨터 전체에서 누락된 보안 업데이트의 총 수

   대시보드 아래쪽에는 VM과 컴퓨터 전체에서 누락된 모든 업데이트와 누락된 업데이트의 심각도가 나열됩니다.  이 목록에는 다음과 같은 정보가 포함됩니다.

    - 이름: 누락된 업데이트의 이름입니다.
    - VM 및 컴퓨터 수: 이 업데이트가 누락된 VM 및 컴퓨터의 총 수입니다.
    - 상태: 권장 사항의 현재 상태입니다.

      - 미해결: 권장 사항이 아직 해결되지 않았습니다.
      - 진행 중: 현재 권장 사항이 해당 리소스에 적용되고 있으며 아무런 작업도 수행할 필요가 없습니다.
      - 해결됨: 권장 사항이 이미 완료되었습니다. 문제가 해결되었으면 해당 항목이 회색으로 표시됩니다.

    - 심각도: 특정 권장 사항의 심각도를 설명합니다.

      - 높음: 중요한 리소스(애플리케이션, 가상 머신 또는 네트워크 보안 그룹)에 취약성이 있으며 주의가 필요합니다.
      - 보통: 중요하지 않은 단계나 추가적인 단계를 수행하면 프로세스를 완료하거나 취약성을 제거할 수 있습니다.
      - 낮음: 취약성을 해결해야 하지만 즉각적인 주의가 필요하지는 않습니다. (기본적으로 낮은 권장 사항은 표시되지 않지만 보려는 경우 낮은 권장 사항으로 필터링할 수 있습니다.)

3. 목록에서 누락된 업데이트를 선택하면 세부 정보를 확인할 수 있습니다.

   ![누락된 보안 업데이트][3]

4. 위쪽 리본에서 **검색** 아이콘을 선택합니다.  업데이트가 누락 된 컴퓨터에 필터링 된 Azure Monitor 로그 검색 쿼리가 열립니다.

   ![Azure Monitor 로그 검색][4]

5. 목록에서 컴퓨터를 선택하면 자세한 내용을 확인할 수 있습니다. 해당 컴퓨터와 관련된 정보만 표시되도록 필터링된 다른 검색 결과가 열립니다.

    ![Azure Monitor 로그 검색][5]

## <a name="next-steps"></a>다음 단계
보안 센터에 대한 자세한 내용은 다음을 참조하세요.

* [Azure Security Center에서 보안 정책 설정](tutorial-security-policy.md) -- Azure 구독 및 리소스 그룹에 대해 보안 정책을 구성하는 방법을 알아봅니다.
* [Azure Security Center에서 보안 권장 사항 관리](security-center-recommendations.md) -- 권장 사항이 Azure 리소스 보호에 어떤 도움이 되는지를 알아봅니다.
* [Azure Security Center에서 보안 상태 모니터링](security-center-monitoring.md) –- Azure 리소스의 상태를 모니터링하는 방법을 알아봅니다.
* [Azure Security Center에서 보안 경고 관리 및 대응](security-center-managing-and-responding-alerts.md) - 보안 경고를 관리하고 대응하는 방법을 알아봅니다.
* [Azure Security Center를 사용하여 파트너 솔루션 모니터링](security-center-partner-solutions.md) -- 파트너 솔루션의 상태를 모니터링하는 방법을 알아봅니다.
* [Azure Security Center FAQ](security-center-faq.md) - 서비스 사용에 관한 질문과 대답을 찾습니다.
* [Azure 보안 블로그](https://blogs.msdn.com/b/azuresecurity/) -- Azure 보안 및 규정 준수에 관한 블로그 게시물을 찾습니다.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
