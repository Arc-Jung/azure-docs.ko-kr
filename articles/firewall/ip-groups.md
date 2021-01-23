---
title: Azure 방화벽의 IP 그룹
description: IP 그룹을 사용 하 여 Azure 방화벽 규칙에 대 한 IP 주소를 그룹화 및 관리할 수 있습니다.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 01/22/2021
ms.author: victorh
ms.openlocfilehash: 6910fda855402d2f99994da1e835560be84ca2dd
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98703380"
---
# <a name="ip-groups-in-azure-firewall"></a>Azure 방화벽의 IP 그룹

IP 그룹을 사용 하 여 다음과 같은 방법으로 Azure 방화벽 규칙에 대 한 IP 주소를 그룹화 하 고 관리할 수 있습니다.

- DNAT 규칙의 원본 주소로
- 네트워크 규칙에서 원본 또는 대상 주소로
- 응용 프로그램 규칙의 원본 주소로


IP 그룹은 단일 IP 주소, 여러 IP 주소 또는 하나 이상의 IP 주소 범위를 가질 수 있습니다.

Azure의 지역 및 구독에서 여러 방화벽에 대 한 Azure 방화벽 DNAT, 네트워크 및 응용 프로그램 규칙에서 IP 그룹을 다시 사용할 수 있습니다. 그룹 이름은 고유 해야 합니다. Azure Portal, Azure CLI 또는 REST API에서 IP 그룹을 구성할 수 있습니다. 시작 하는 데 도움이 되는 샘플 템플릿이 제공 됩니다.

> [!NOTE]
> 현재 Azure 국가별 클라우드 환경에서는 IP 그룹을 사용할 수 없습니다.

## <a name="sample-format"></a>샘플 형식

IP 그룹에서 사용할 수 있는 IPv4 주소 형식 예는 다음과 같습니다.

- 단일 주소: 10.0.0.0
- CIDR 표기법: 10.1.0.0/32
- 주소 범위: 10.2.0.0-10.2.0.31

## <a name="create-an-ip-group"></a>IP 그룹 만들기

IP 그룹은 Azure Portal, Azure CLI 또는 REST API를 사용 하 여 만들 수 있습니다. 자세한 내용은 [IP 그룹 만들기](create-ip-group.md)를 참조 하세요.

## <a name="browse-ip-groups"></a>IP 그룹 찾아보기
1. Azure Portal 검색 창에서 **IP 그룹** 을 입력 하 고 선택 합니다. IP 그룹 목록을 보거나 **추가** 를 선택 하 여 새 ip 그룹을 만들 수 있습니다.
2. IP 그룹을 선택 하 여 개요 페이지를 엽니다. IP 주소 또는 IP 그룹을 편집, 추가 또는 삭제할 수 있습니다.

   ![IP 그룹 개요](media/ip-groups/overview.png)

## <a name="manage-an-ip-group"></a>IP 그룹 관리

IP 그룹의 모든 IP 주소와 연결 된 규칙 또는 리소스를 볼 수 있습니다. IP 그룹을 삭제 하려면 먼저 ip 그룹을 사용 하는 리소스에서 IP 그룹을 분리 해야 합니다.

1. IP 주소를 보거나 편집 하려면 왼쪽 창의 **설정** 에서 **ip 주소** 를 선택 합니다.
2. 단일 또는 여러 IP 주소를 추가 하려면 **ip 주소 추가** 를 선택 합니다. 업로드에 대 한 **끌기 또는 찾아보기** 페이지가 열리거나 수동으로 주소를 입력할 수 있습니다.
3.    오른쪽에 있는 줄임표 (**...**)를 선택 하 여 IP 주소를 편집 하거나 삭제 합니다. 여러 IP 주소를 편집 하거나 삭제 하려면 상자를 선택 하 고 위쪽에서 **편집** 또는 **삭제** 를 선택 합니다.
4. 마지막으로는 CSV 파일 형식으로 파일을 내보낼 수 있습니다.

> [!NOTE]
> 규칙에서 아직 사용 중인 IP 그룹의 모든 IP 주소를 삭제 하면 해당 규칙을 건너뜁니다.


## <a name="use-an-ip-group"></a>IP 그룹 사용

이제, 응용 프로그램 또는 네트워크 규칙에 따라 Azure 방화벽 DNAT 만들 **때 ip 주소** 를 **원본 유형** 또는 ip 주소에 대 한 **대상 유형** 으로 선택할 수 있습니다.

![방화벽의 IP 그룹](media/ip-groups/fw-ipgroup.png)

## <a name="region-availability"></a>지역 가용성

모든 공용 클라우드 지역에서 IP 그룹을 사용할 수 있습니다.

## <a name="ip-address-limits"></a>IP 주소 제한

각 IP 그룹당 최대 5000 개별 IP 주소 또는 IP 접두사를 사용 하 여 방화벽 당 최대 100 개의 IP 그룹을 사용할 수 있습니다.

## <a name="related-azure-powershell-cmdlets"></a>관련 Azure PowerShell cmdlet

다음 Azure PowerShell cmdlet을 사용 하 여 IP 그룹을 만들고 관리할 수 있습니다.

- [New-AzIpGroup](/powershell/module/az.network/new-azipgroup?view=azps-3.4.0)
- [AzIPGroup](/powershell/module/az.network/remove-azipgroup?view=azps-3.4.0)
- [Get-AzIpGroup](/powershell/module/az.network/get-azipgroup?view=azps-3.4.0)
- [Set-AzIpGroup](/powershell/module/az.network/set-azipgroup?view=azps-3.4.0)
- [AzFirewallNetworkRule](/powershell/module/az.network/new-azfirewallnetworkrule?view=azps-3.4.0)
- [New-AzFirewallApplicationRule](/powershell/module/az.network/new-azfirewallapplicationrule?view=azps-3.4.0)
- [AzFirewallNatRule](/powershell/module/az.network/new-azfirewallnatrule?view=azps-3.4.0)

## <a name="next-steps"></a>다음 단계

- [Azure 방화벽을 배포 하 고 구성](tutorial-firewall-deploy-portal.md)하는 방법에 대해 알아봅니다.