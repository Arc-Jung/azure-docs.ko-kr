---
title: Azure 개인 링크를 사용 하 여 네트워크를 Azure Automation에 안전 하 게 연결
description: Azure 개인 링크를 사용 하 여 네트워크를 Azure Automation에 안전 하 게 연결
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 12/11/2020
ms.subservice: ''
ms.openlocfilehash: f3c9197faaae89e0ffb238f987ee66dafea8abdd
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100579803"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-automation"></a>Azure 개인 링크를 사용 하 여 네트워크를 Azure Automation에 안전 하 게 연결

Azure 프라이빗 엔드포인트는 Azure Private Link가 제공하는, 서비스에 비공개로 안전하게 연결하는 네트워크 인터페이스입니다. 개인 끝점은 VNet의 개인 IP 주소를 사용 하 여 자동화 서비스를 VNet에 효과적으로 제공 합니다. VNet 및 Automation 계정의 컴퓨터 간 네트워크 트래픽은 VNet을 통해 이동 하 여 공용 인터넷에서 노출 되는 것을 제거 하는 Microsoft 백본 네트워크의 개인 링크를 통해 이동 합니다.

예를 들어 아웃 바운드 인터넷 액세스를 사용 하지 않도록 설정 하는 VNet이 있습니다. 그러나 Automation 계정에 개인적으로 액세스 하 고 하이브리드 Runbook 작업자의 웹 후크, 상태 구성 및 runbook 작업 같은 자동화 기능을 사용 하려고 합니다. 또한 사용자가 VNET을 통해서만 Automation 계정에 액세스할 수 있도록 하려고 합니다.  개인 끝점 배포는 이러한 목표를 달성 합니다.

이 문서에서는를 사용 하는 경우 및 Automation 계정으로 개인 끝점을 설정 하는 방법을 설명 합니다.

![Azure Automation에 대 한 개인 링크의 개념적 개요](./media/private-link-security/private-endpoints-automation.png)

>[!NOTE]
> Azure Automation에 대 한 개인 링크 지원은 Azure 상용 및 Azure 미국 정부 클라우드에서만 사용할 수 있습니다.

## <a name="advantages"></a>장점

Private Link를 사용하면 다음을 수행할 수 있습니다.

- 공용 네트워크 액세스를 열지 않고 Azure Automation에 비공개로 연결 합니다.
- 공용 네트워크 액세스를 열지 않고 Azure Monitor Log Analytics 작업 영역에 비공개로 연결 합니다.

    >[!NOTE]
    >Log Analytics 작업 영역에 대 한 별도의 개인 끝점은 작업 데이터를 전달 하기 위해 Automation 계정이 Log Analytics 작업 영역에 연결 되어 있고 업데이트 관리, 변경 내용 추적 및 인벤토리, 상태 구성 또는 작업 시간 외 VM 시작/중지 같은 기능을 사용 하도록 설정한 경우에 필요 합니다. Azure Monitor에 대 한 개인 링크에 대 한 자세한 내용은 [Azure 개인 링크를 사용 하 여 네트워크를 Azure Monitor에 안전 하 게 연결을](../../azure-monitor/logs/private-link-security.md)참조 하세요.

- 권한 있는 개인 네트워크를 통해 자동화 데이터가 액세스 되는지 확인 합니다.
- 개인 끝점을 통해 연결 되는 Azure Automation 리소스를 정의 하 여 개인 네트워크에서 데이터 반출을 방지 합니다.
- Express 경로 및 개인 링크를 사용 하 여 Azure Automation에 개인 온-프레미스 네트워크를 안전 하 게 연결 합니다.
- 모든 트래픽을 Microsoft Azure 백본 네트워크 내에 유지 합니다.

자세한 내용은 [Private Link의 주요 이점](../../private-link/private-link-overview.md#key-benefits)을 참조하세요.

## <a name="limitations"></a>제한 사항

- 현재 개인 링크 구현에서 Automation 계정 클라우드 작업은 개인 끝점을 사용 하 여 보호 되는 Azure 리소스에 액세스할 수 없습니다. 예를 들어 Azure Key Vault, Azure SQL, Azure Storage 계정 등이 있습니다. 이 문제를 해결 하려면 [Hybrid Runbook Worker](../automation-hybrid-runbook-worker.md) 를 대신 사용 합니다.
- Windows 또는 Linux 용 [Log Analytics 에이전트](../../azure-monitor/agents/log-analytics-agent.md) 의 최신 버전을 사용 해야 합니다.
- [Log Analytics 게이트웨이에서](../../azure-monitor/agents/gateway.md) 개인 링크를 지원 하지 않습니다.

## <a name="how-it-works"></a>작동 방법

개인 링크 Azure Automation 하나 이상의 개인 끝점 (그리고 그에 포함 된 가상 네트워크)을 Automation 계정 리소스에 연결 합니다. 이러한 끝점은 웹 후크를 사용 하 여 runbook을 시작 하는 컴퓨터, Hybrid Runbook Worker 역할을 호스트 하는 컴퓨터 및 DSC (Desired State Configuration) 노드를 사용 합니다.

자동화를 위한 개인 끝점을 만든 후에는 각 공용 연결 자동화 Url이 VNet의 하나의 개인 끝점에 매핑됩니다. 사용자 또는 컴퓨터에서 자동화 Url에 직접 연결할 수 있습니다.

### <a name="webhook-scenario"></a>Webhook 시나리오

웹 후크 URL에 게시를 수행 하 여 runbook을 시작할 수 있습니다. 예를 들어 URL은 다음과 같습니다. `https://<automationAccountId>.webhooks.<region>.azure-automation.net/webhooks?token=gzGMz4SMpqNo8gidqPxAJ3E%3d`

### <a name="hybrid-runbook-worker-scenario"></a>Hybrid Runbook Worker 시나리오

Azure Automation의 사용자 Hybrid Runbook Worker 기능을 사용 하면 azure Arc 사용 서버에 등록 된 서버를 포함 하 여 azure 또는 비 Azure 컴퓨터에서 직접 runbook을 실행할 수 있습니다. 역할을 호스트 하는 컴퓨터 또는 서버에서 직접 runbook을 실행 하 고 환경의 리소스에 대해 해당 로컬 리소스를 관리할 수 있습니다.

JRDS 끝점은 hybrid worker가 runbook을 시작/중지 하 고, runbook을 작업자에 다운로드 하 고, 작업 로그 스트림을 자동화 서비스로 다시 보내는 데 사용 됩니다.JRDS 끝점을 사용 하도록 설정한 후 URL `https://<automationaccountID>.jobruntimedata.<region>.azure-automation.net` 은 다음과 같습니다. 이렇게 하면 Azure Virtual Network에 연결 된 hybrid worker에서 runbook을 실행 하 여 인터넷에 대 한 아웃 바운드 연결을 열지 않고도 작업을 실행할 수 있습니다.  

> [!NOTE]
>Azure Automation에 대 한 개인 링크의 현재 구현을 사용 하 여 Azure 가상 네트워크에 연결 된 Hybrid Runbook Worker에 대 한 실행 작업만 지원 하 고 클라우드 작업을 지원 하지 않습니다.

## <a name="hybrid-worker-scenario-for-update-management"></a>업데이트 관리에 대 한 Hybrid Worker 시나리오  

시스템 Hybrid Runbook Worker는 Windows 및 Linux 컴퓨터에 사용자 지정 업데이트를 설치 하도록 디자인 된 업데이트 관리 기능에서 사용 하는 숨겨진 runbook 집합을 지원 합니다. Azure Automation 업데이트 관리를 사용 하도록 설정 하면 Log Analytics 작업 영역에 연결 된 모든 컴퓨터가 자동으로 시스템 Hybrid Runbook Worker 구성 됩니다.

& 이해 하려면 [업데이트 관리에 대 한](../update-management/overview.md)업데이트 관리 검토를 구성 합니다. 업데이트 관리 기능은 Log Analytics 작업 영역에 종속 되므로 작업 영역을 Automation 계정과 연결 해야 합니다. Log Analytics 작업 영역에서는 솔루션에 의해 수집 된 데이터를 저장 하 고 해당 로그 검색 및 보기를 호스팅합니다.

업데이트 관리를 위해 구성 된 컴퓨터가 개인 링크 채널 보다 안전한 방식으로 자동화 & Log Analytics 작업 영역에 연결 되도록 하려면 개인 링크로 구성 된 Automation 계정에 연결 된 Log Analytics 작업 영역에 대 한 개인 링크를 사용 하도록 설정 해야 합니다.

[Log Analytics 구성](../../azure-monitor/logs/private-link-security.md#configure-log-analytics)에 설명 된 단계를 수행 하 여 개인 링크 범위 외부에서 Log Analytics 작업 영역에 연결할 수 있는 방법을 제어할 수 있습니다. **수집을 위해 공용 네트워크 액세스 허용** 을 **아니요** 로 설정하면 연결된 범위 외부의 머신에서 데이터를 이 작업 영역에 업로드할 수 없습니다. **쿼리를 위해 공용 네트워크 액세스 허용** 을 **아니요** 로 설정하면 범위 외부의 머신에서 이 작업 영역의 데이터에 액세스할 수 없습니다.

**DSCAndHybridWorker** 대상 하위 리소스를 사용 하 여 사용자 & 시스템 hybrid worker에 대 한 개인 링크를 사용 하도록 설정 합니다.

> [!NOTE]
> 업데이트 관리에서 관리 하는 Azure 외부에서 호스트 되는 컴퓨터는 Express 경로 개인 피어 링, VPN 터널 및 개인 끝점을 사용 하는 피어 링 가상 네트워크 지원 개인 링크를 통해 Azure VNet에 연결 됩니다.

### <a name="state-configuration-agentsvc-scenario"></a>상태 구성 (agentsvc) 시나리오

상태 구성은 모든 클라우드 또는 온-프레미스 데이터 센터의 노드에 대 한 PowerShell DSC (필요한 상태 구성) 구성을 작성, 관리 및 컴파일할 수 있도록 하는 Azure 구성 관리 서비스를 제공 합니다.

컴퓨터의 에이전트는 DSC 서비스를 사용 하 여 등록 한 다음 서비스 끝점을 사용 하 여 DSC 구성을 가져옵니다. 에이전트 서비스 끝점 `https://<automationAccountId>.agentsvc.<region>.azure-automation.net` 은 다음과 같습니다.

공용 & 개인 끝점의 URL은 동일 하지만 개인 링크를 사용 하는 경우 개인 IP 주소에 매핑됩니다.

## <a name="planning-based-on-your-network"></a>네트워크 기반 계획 수립

Automation 계정 리소스를 설정 하기 전에 네트워크 격리 요구 사항을 고려 하세요. 공용 인터넷에 대 한 가상 네트워크 액세스 및 automation 계정에 대 한 액세스 제한 (Automation 계정과 통합 된 경우 개인 링크 그룹 범위를 Azure Monitor 로그로 설정 포함)을 평가 합니다. 또한 지원 되는 기능이 문제 없이 작동 하는지 확인 하기 위한 계획의 일부로 Automation 서비스 [DNS 레코드](./automation-region-dns-records.md) 검토를 포함 합니다.

### <a name="connect-to-a-private-endpoint"></a>프라이빗 엔드포인트에 연결

네트워크를 연결 하는 개인 끝점을 만듭니다. [Azure Portal 개인 링크 센터](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints)에서 만들 수 있습니다. PublicNetworkAccess 및 개인 링크에 대 한 변경 내용이 적용 되 면 적용 되는 데 최대 35 분이 걸릴 수 있습니다.

이 섹션에서는 Automation 계정에 대 한 개인 끝점을 만듭니다.

1. 화면 왼쪽 상단에서 **리소스 만들기 > 네트워킹 > 개인 링크 센터** 를 선택 합니다.

2. **Private Link 센터 - 개요** 의 **서비스에 대한 프라이빗 연결 설정** 옵션에서 **시작** 을 선택합니다.

3. **가상 컴퓨터 만들기-기본 사항** 에서 다음 정보를 입력 하거나 선택 합니다.

    | 설정 | 값 |
    | ------- | ----- |
    | **프로젝트 정보** | |
    | Subscription | 구독을 선택합니다. |
    | Resource group | **myResourceGroup** 을 선택합니다. 이전 섹션에서 만든 것입니다.  |
    | **인스턴스 정보** |  |
    | 속성 | *PrivateEndpoint* 를 입력 합니다. |
    | 지역 | **영역** 을 선택 합니다. |
    |||

4. 완료되면 **다음: 리소스** 를 선택합니다.

5. **개인 끝점 만들기-리소스** 에서 다음 정보를 입력 하거나 선택 합니다.

    | 설정 | 값 |
    | ------- | ----- |
    |연결 방법  | 내 디렉터리의 Azure 리소스에 연결하도록 선택합니다.|
    | Subscription| 구독을 선택합니다. |
    | 리소스 유형 | **Microsoft Automation/automationAccounts** 를 선택 합니다. |
    | 리소스 |*Myautomationaccount* 를 선택 합니다.|
    |대상 하위 리소스 |시나리오에 따라 *Webhook* 또는 *DSCAndHybridWorker* 를 선택 합니다.|
    |||

6. 완료되면 **다음: 구성** 을 선택합니다.

7. **개인 끝점 만들기-구성** 에서 다음 정보를 입력 하거나 선택 합니다.

    | 설정 | 값 |
    | ------- | ----- |
    |**네트워킹**| |
    | 가상 네트워크| *MyVirtualNetwork* 를 선택합니다. |
    | 서브넷 | *mySubnet* 을 선택합니다. |
    |**프라이빗 DNS 통합**||
    |프라이빗 DNS 영역과 통합 |**예** 를 선택합니다. |
    |프라이빗 DNS 영역 |Select *(New) privatelink-automation.net* |
    |||

8. **검토 + 만들기** 를 선택합니다. **검토 + 만들기** 페이지로 이동됩니다. 여기서 구성이 유효한지 검사됩니다.

9. **유효성 검사 통과** 메시지가 표시되면 **만들기** 를 선택합니다.

개인 **링크 센터** 에서 개인 **끝점** 을 선택 하 여 개인 링크 리소스를 확인 합니다.

![Automation 리소스 개인 링크](./media/private-link-security/private-link-automation-resource.png)

모든 세부 정보를 보려면 리소스를 선택 합니다. 이렇게 하면 Automation 계정에 대 한 새 개인 끝점을 만들고 가상 네트워크에서 개인 IP를 할당 합니다. **연결 상태가** **승인** 됨으로 표시 됩니다.

마찬가지로, 상태 구성 (agentsvc)에 대 한 고유 FQDN (정규화 된 도메인 이름)과 jrds (Hybrid Runbook Worker 작업 런타임)에 대 한 고유 FQDN (정규화 된 도메인 이름)이 만들어집니다. 각 컴퓨터에는 VNet과 별도의 IP가 할당 되며 **연결 상태** 는 **승인** 됨으로 표시 됩니다.

서비스 소비자에 게 Automation 리소스에 대 한 Azure RBAC 권한이 있는 경우 자동 승인 방법을 선택할 수 있습니다. 이 경우 요청이 자동화 공급자 리소스에 도달 하면 서비스 공급자의 작업이 필요 하지 않으며 연결이 자동으로 승인 됩니다.

## <a name="set-public-network-access-flags"></a>공용 네트워크 액세스 플래그 설정

모든 공용 구성을 거부 하도록 Automation 계정을 구성 하 고, 개인 끝점을 통해서만 연결을 허용 하 여 네트워크 보안을 더욱 향상 시킬 수 있습니다. VNet 내 에서만 Automation 계정에 대 한 액세스를 제한 하 고 공용 인터넷에서의 액세스를 허용 하지 않으려면 `publicNetworkAccess` 속성을로 설정할 수 있습니다 `$false` .

**공용 네트워크 액세스** 설정이로 설정 되 면 `$false` 개인 끝점을 통한 연결만 허용 되며, 공개 끝점을 통한 모든 연결은 거부 된 오류 메시지 및 401의 HTTP 상태와 함께 거부 됩니다.

다음 PowerShell 스크립트는 `Get` `Set` Automation 계정 수준에서 및 **공용 네트워크 액세스** 속성을 보여 줍니다.

```powershell
$account = Get-AzResource -ResourceType Microsoft.Automation/automationAccounts -ResourceGroupName "<resourceGroupName>" -Name "<automationAccountName>" -ApiVersion "2020-01-13-preview"
$account.Properties | Add-Member -Name 'publicNetworkAccess' -Type NoteProperty -Value $false
$account | Set-AzResource -Force -ApiVersion "2020-01-13-preview"
```

Azure Portal에서 공용 네트워크 액세스 속성을 제어할 수도 있습니다. Automation 계정의 **계정 설정** 섹션 아래에 있는 왼쪽 창에서 **네트워크 격리** 를 선택 합니다. 공용 네트워크 액세스 설정이 **아니요** 로 설정 된 경우 개인 끝점을 통한 연결만 허용 되며 공용 끝점을 통한 모든 연결은 거부 됩니다.

![공용 네트워크 액세스 설정](./media/private-link-security/allow-public-network-access.png)

## <a name="dns-configuration"></a>DNS 구성

연결 문자열의 일부로 FQDN(정규화된 도메인 이름)을 사용하여 프라이빗 링크 리소스에 연결하는 경우 할당된 프라이빗 IP 주소를 확인하도록 DNS 설정을 올바르게 구성하는 것이 중요합니다. 기존 Azure 서비스에는 공용 엔드포인트를 통해 연결할 때 사용할 DNS 구성이 이미 있을 수 있습니다. DNS 구성을 검토 하 고 개인 끝점을 사용 하 여 연결 하도록 업데이트 해야 합니다.

프라이빗 엔드포인트와 연결된 네트워크 인터페이스에는 지정된 프라이빗 링크 리소스에 대해 할당된 프라이빗 IP 주소 및 FQDN을 포함하여 DNS를 구성하는 데 필요한 전체 정보 집합이 포함되어 있습니다.

다음 옵션을 사용하여 프라이빗 엔드포인트에 대한 DNS 설정을 구성할 수 있습니다.

* 호스트 파일 (테스트에만 권장 됨)을 사용 합니다. 가상 컴퓨터의 호스트 파일을 사용 하 여 이름 확인을 위해 DNS를 사용 하 여 재정의할 수 있습니다. DNS 항목은 다음 예제와 같습니다 `privatelinkFQDN.jrds.sea.azure-automation.net` .

* [개인 DNS 영역](../../dns/private-dns-privatednszone.md)을 사용 합니다. 개인 DNS 영역을 사용 하 여 특정 개인 끝점에 대 한 DNS 확인을 재정의할 수 있습니다. 프라이빗 DNS 영역을 가상 네트워크에 연결하여 특정 도메인을 확인할 수 있습니다. 가상 컴퓨터에서 에이전트가 개인 끝점을 통해 통신할 수 있도록 하려면 사설 DNS 레코드를로 만듭니다 `privatelink.azure-automation.net` . 새 DNS *a* 레코드 매핑을 개인 끝점의 IP에 추가 합니다.

* DNS 전달자를 사용 합니다 (선택 사항). DNS 전달자를 사용 하 여 특정 개인 링크 리소스에 대 한 DNS 확인을 재정의할 수 있습니다. [DNS 서버](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)가 가상 네트워크에서 호스트되는 경우 프라이빗 DNS 영역을 사용하여 모든 프라이빗 링크 리소스에 대한 구성을 간소화하는 DNS 전달 규칙을 만들 수 있습니다.

자세한 내용은 [Azure 개인 끝점 DNS 구성](../../private-link/private-endpoint-dns.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

개인 끝점에 대 한 자세한 내용은 [Azure 개인 끝점 이란?](../../private-link/private-endpoint-overview.md)을 참조 하세요.