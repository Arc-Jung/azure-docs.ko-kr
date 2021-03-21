---
title: VPN 게이트웨이 또는 연결을 다시 설정 하 여 IPsec 터널 다시 설정
titleSuffix: Azure VPN Gateway
description: IPsec 터널을 다시 설정 하려면 연결 또는 VPN 게이트웨이를 다시 설정 합니다.
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/22/2021
ms.author: cherylmc
ms.openlocfilehash: adc2ffd63d73baaddce00324787df61061ea69dc
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101726645"
---
# <a name="reset-a-vpn-gateway-or-a-connection"></a>VPN gateway 또는 연결 다시 설정

하나 이상의 사이트 간 VPN 터널에서 크로스-프레미스 VPN 연결이 손실 되는 경우 Azure VPN gateway 또는 게이트웨이 연결을 다시 설정 하는 것이 유용 합니다. 이 상황에서 온-프레미스 VPN 디바이스는 모두 올바르게 작동하지만 Azure VPN 게이트웨이와 IPsec 터널을 설정할 수 없습니다. 이 문서에서는 VPN gateway 또는 게이트웨이 연결을 다시 설정 하는 방법을 설명 합니다.

## <a name="what-happens-during-a-reset"></a>다시 설정 하는 동안 수행 되는 작업

### <a name="gateway-reset"></a>게이트웨이 다시 설정

VPN Gateway는 활성-대기 구성에서 실행 중인 두 VM 인스턴스로 구성됩니다. 게이트웨이를 다시 설정한 경우 게이트웨이가 다시 부팅된 후 크로스-프레미스 구성이 다시 적용됩니다. 게이트웨이는 기존의 공용 IP 주소를 유지합니다. 즉, Azure VPN 게이트웨이에 대한 새 공용 IP 주소로 VPN 라우터 구성을 업데이트할 필요가 없습니다.

게이트웨이를 다시 설정하는 명령을 실행하면 Azure VPN Gateway의 현재 활성 인스턴스가 즉시 재부팅됩니다. 활성 인스턴스(재부팅)에서 대기 인스턴스로 장애 조치(Failover) 중에 잠깐의 간격이 있습니다. 이 간격은 1분 미만이어야 합니다.

첫 번째 재부팅 후 연결이 복원되지 않으면 같은 명령을 다시 실행하여 두 번째 VM인스턴스(새 활성 게이트웨이)를 재부팅합니다. 두 번의 재부팅이 연속으로 요청되는 경우 두 VM 인스턴스(활성 및 대기)가 재부팅되는 기간이 약간 길어집니다. 이로 인해 Vm이 다시 부팅을 완료 하는 데 최대 30 ~ 45 분까지 VPN 연결에서 더 긴 간격이 발생 합니다.

두 번의 재부팅 후 크로스-프레미스 연결 문제가 여전히 발생하는 경우 Azure Portal에서 지원 요청을 여세요.

### <a name="connection-reset"></a>연결 다시 설정

연결을 다시 설정 하도록 선택 하면 게이트웨이가 다시 부팅 되지 않습니다. 선택한 연결만 다시 설정 되 고 복원 됩니다.

## <a name="reset-a-connection"></a>연결 다시 설정

Azure Portal를 사용 하 여 연결을 쉽게 다시 설정할 수 있습니다.

1. 다시 설정 하려는 **연결** 로 이동 합니다. **모든 리소스** 에서 연결 리소스를 찾거나 **' 게이트웨이 이름 ' > 연결-> ' 연결 이름 '** 으로 이동 하 여 연결 리소스를 찾을 수 있습니다.
1. **연결** 페이지의 왼쪽 메뉴에서 **다시 설정** 을 선택 합니다.
1. **다시 설정** 페이지에서 **다시 설정** 을 클릭 하 여 연결을 다시 설정 합니다.

   :::image type="content" source="./media/reset-gateway/reset-connection.png" alt-text="재설정을 보여 주는 스크린샷":::

## <a name="reset-a-vpn-gateway"></a>VPN Gateway 다시 설정

게이트웨이를 재설정하기 전에 각 IPsec 사이트 간(S2S) VPN 터널에 대해 아래 나열된 키 항목을 확인합니다. 일치하지 않는 항목은 S2S VPN 터널에서 연결이 끊깁니다. 온-프레미스 및 Azure VPN Gateway에 대한 구성을 확인 및 수정하면 게이트웨이에서 작동 중인 기타 연결에 대해 불필요한 재부팅 및 중단을 피할 수 있습니다.

게이트웨이를 재설정하기 전에 다음 항목을 확인합니다.

* Azure VPN 게이트웨이 및 온-프레미스 VPN 게이트웨이 모두에 대한 인터넷 IP 주소(VIP)가 Azure 및 온-프레미스 VPN 정책에서 바르게 구성되어 있습니다.
* 미리 공유한 키가 Azure와 온-프레미스 VPN 게이트웨이에서 동일해야 합니다.
* 암호화, 해시 알고리즘 및 전달 완전 보안(PFS)과 같은 특정 IPsec/IKE 구성을 적용하는 경우 Azure와 온-프레미스 VPN 게이트웨이에서 동일한 구성을 포함하는지 확인하세요.

### <a name="azure-portal"></a><a name="portal"></a>Azure Portal

Azure Portal을 사용하여 Resource Manager VPN Gateway를 다시 설정할 수 있습니다. 클래식 게이트웨이를 다시 설정 하려는 경우 [클래식 배포 모델](#resetclassic)에 대 한 PowerShell 단계를 참조 하세요.

[!INCLUDE [portal steps](../../includes/vpn-gateway-reset-gw-portal-include.md)]

### <a name="powershell"></a><a name="ps"></a>PowerShell

#### <a name="resource-manager-deployment-model"></a>리소스 관리자 배포 모델

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

게이트웨이를 다시 설정하는 cmdlet은 **Reset-AzVirtualNetworkGateway** 입니다. 다시 설정을 수행 하기 전에 최신 버전의 [PowerShell Az cmdlet](/powershell/module/az.network)이 있는지 확인 합니다. 다음 예제에서는 TestRG1 리소스 그룹에서 VNet1GW라는 가상 네트워크 게이트웨이를 다시 설정합니다.

```powershell
$gw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
```

결과:

반환 결과를 받으면 게이트웨이 재설정에 성공한 것으로 가정할 수 있습니다. 그러나 반환 결과에는 재설정에 성공했음을 명시적으로 나타내는 항목이 없습니다. 기록을 면밀히 확인하여 게이트웨이 재설정이 발생한 시점을 정확히 알아보려면 [Azure Portal](https://portal.azure.com)에서 해당 정보를 보면 됩니다. 포털에서 **'GatewayName' -> Resource Health** 로 이동합니다.

#### <a name="classic-deployment-model"></a><a name="resetclassic"></a>클래식 배포 모델

게이트웨이를 다시 설정하는 cmdlet은 **Reset-AzureVNetGateway** 입니다. 서비스 관리를 위한 Azure PowerShell cmdlet은 데스크톱에 로컬로 설치 해야 합니다. Azure Cloud Shell를 사용할 수 없습니다. 다시 설정을 수행하기 전에 최신 버전의 [SM(Service Management) PowerShell cmdlet](/powershell/azure/servicemanagement/install-azure-ps#azure-service-management-cmdlets)이 있는지 확인합니다. 이 명령을 사용 하는 경우 가상 네트워크의 전체 이름을 사용 하 고 있는지 확인 합니다. 포털을 사용 하 여 만든 클래식 Vnet에는 PowerShell에 필요한 긴 이름이 있습니다. ' Get-AzureVNetConfig-ExportToFile C:\Myfoldername\NetworkConfig.xml '를 사용 하 여 긴 이름을 볼 수 있습니다.

다음 예제에서는 "Group TestRG1 TestVNet1" 이라는 가상 네트워크에 대 한 게이트웨이를 다시 설정 합니다 .이는 포털에서 단순히 "TestVNet1"로 표시 됩니다.

```powershell
Reset-AzureVNetGateway –VnetName 'Group TestRG1 TestVNet1'
```

결과:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

### <a name="azure-cli"></a><a name="cli"></a>Azure CLI

게이트웨이를 다시 설정하려면 [az network vnet-gateway reset](/cli/azure/network/vnet-gateway) 명령을 사용합니다. 다음 예제에서는 TestRG5 리소스 그룹에서 VNet5GW라는 가상 네트워크 게이트웨이를 다시 설정합니다.

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

결과:

반환 결과를 받으면 게이트웨이 재설정에 성공한 것으로 가정할 수 있습니다. 그러나 반환 결과에는 재설정에 성공했음을 명시적으로 나타내는 항목이 없습니다. 기록을 면밀히 확인하여 게이트웨이 재설정이 발생한 시점을 정확히 알아보려면 [Azure Portal](https://portal.azure.com)에서 해당 정보를 보면 됩니다. 포털에서 **'GatewayName' -> Resource Health** 로 이동합니다.
