---
title: 가상 네트워크 게이트웨이를 Azure 가상 WAN에 연결
description: 이 문서는 azure 가상 네트워크 게이트웨이를 Azure 가상 WAN VPN gateway에 연결 하는 데 도움이 됩니다.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 469d7ba9e86751312ebf6a6c82b35f065ee6cb50
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98880375"
---
# <a name="connect-a-vpn-gateway-virtual-network-gateway-to-virtual-wan"></a>가상 WAN에 VPN Gateway (가상 네트워크 게이트웨이) 연결

이 문서는 Azure VPN Gateway (가상 네트워크 게이트웨이)에서 Azure 가상 WAN (VPN Gateway)으로의 연결을 설정 하는 데 도움이 됩니다. VPN Gateway (가상 네트워크 게이트웨이)에서 가상 WAN (VPN Gateway)으로의 연결 만들기는 분기 VPN 사이트에서 가상 WAN에 대 한 연결을 설정 하는 것과 비슷합니다.

두 기능 사이에서 혼동을 최소화 하기 위해 게이트웨이 앞에는 참조 하는 기능의 이름이 포함 됩니다. 예를 들어 가상 네트워크 게이트웨이 및 가상 WAN VPN gateway를 VPN Gateway 합니다.

## <a name="before-you-begin"></a>시작하기 전에

시작 하기 전에 다음 리소스를 만듭니다.

Azure Virtual WAN

* [가상 WAN을 만듭니다](virtual-wan-site-to-site-portal.md#openvwan).
* [허브를 만듭니다](virtual-wan-site-to-site-portal.md#hub). 가상 허브에는 가상 WAN VPN gateway가 포함 됩니다.

Azure Virtual Network

* 가상 네트워크 게이트웨이를 사용 하지 않고 가상 네트워크를 만듭니다. 온-프레미스 네트워크의 어떤 서브넷도 연결하려는 가상 네트워크 서브넷과 중첩되지 않는지 확인합니다. Azure Portal에서 가상 네트워크를 만들려면 [빠른 시작](../virtual-network/quick-create-portal.md)을 참조하세요.

## <a name="1-create-a-vpn-gateway-virtual-network-gateway"></a><a name="vnetgw"></a>1. VPN Gateway 가상 네트워크 게이트웨이 만들기

가상 네트워크에 대 한 활성-활성 모드에서 **VPN Gateway** 가상 네트워크 게이트웨이를 만듭니다. 게이트웨이를 만들 때 게이트웨이의 두 인스턴스에 대해 기존 공용 IP 주소를 사용 하거나 새 공용 IP를 만들 수 있습니다. 가상 WAN 사이트를 설정할 때 이러한 공용 Ip를 사용 합니다. 활성-활성 VPN 게이트웨이 및 구성 단계에 대 한 자세한 내용은 [활성-활성 vpn 게이트웨이 구성](../vpn-gateway/vpn-gateway-activeactive-rm-powershell.md#aagateway)을 참조 하세요.

### <a name="active-active-mode-setting"></a><a name="active-active"></a>활성-활성 모드 설정

가상 네트워크 게이트웨이 **구성** 페이지에서 활성-활성 모드를 사용 하도록 설정 합니다.

![활성-활성](./media/connect-virtual-network-gateway-vwan/active.png "액티브-액티브")

### <a name="bgp-setting"></a><a name="BGP"></a>BGP 설정

가상 네트워크 게이트웨이 **구성** 페이지에서 **BGP ASN** 을 구성할 수 있습니다. BGP ASN을 변경 합니다. BGP ASN은 65515 일 수 없습니다. 65515는 Azure 가상 WAN에서 사용 됩니다.

![스크린샷에 BGP ASN 구성이 선택 된 가상 네트워크 게이트웨이 구성 페이지를 보여 줍니다.](./media/connect-virtual-network-gateway-vwan/bgp.png "bgp")

### <a name="public-ip-addresses"></a><a name="pip"></a>공용 IP 주소

게이트웨이가 만들어지면 **속성** 페이지로 이동 합니다. 속성 및 구성 설정은 다음 예제와 유사 합니다. 게이트웨이에 사용 되는 두 개의 공용 IP 주소를 확인 합니다.

![properties](./media/connect-virtual-network-gateway-vwan/publicip.png "properties")

## <a name="2-create-virtual-wan-vpn-sites"></a><a name="vwansite"></a>2. 가상 WAN VPN 사이트 만들기

가상 WAN VPN 사이트를 만들려면 가상 WAN으로 이동 하 고 **연결** 에서 **VPN 사이트** 를 선택 합니다. 이 섹션에서는 이전 섹션에서 만든 가상 네트워크 게이트웨이에 해당 하는 두 개의 가상 WAN VPN 사이트를 만듭니다.

1. **+ 사이트 만들기** 를 선택 합니다.
2. **VPN 사이트 만들기** 페이지에서 다음 값을 입력 합니다.

   * **Region** -Azure VPN Gateway 가상 네트워크 게이트웨이와 동일한 지역입니다.
   * **장치 공급 업체** -장치 공급 업체 (이름)를 입력 합니다.
   * **개인 주소 공간** -BGP를 사용 하는 경우 값을 입력 하거나 비워 둡니다.
   * **Border Gateway Protocol** -Azure VPN Gateway 가상 네트워크 게이트웨이에서 BGP를 사용 하도록 설정한 경우 **사용** 으로 설정 합니다.
   * **허브에 연결** -드롭다운에서 필수 구성 요소에서 만든 허브를 선택 합니다. 허브가 표시 되지 않으면 허브에 대 한 사이트 간 VPN gateway를 만들었는지 확인 합니다.
3. **링크** 에서 다음 값을 입력 합니다.

   * **공급자 이름** -링크 이름 및 공급자 이름 (모든 이름)을 입력 합니다.
   * **속도** -속도 (임의 수)입니다.
   * **Ip 주소** -ip 주소를 입력 합니다 (VPN Gateway) 가상 네트워크 게이트웨이 속성 아래에 표시 되는 첫 번째 공용 IP 주소와 동일 합니다.
   * **Bgp 주소** 및 **asn** -bgp 주소 및 asn. 이러한 항목은 [1 단계](#vnetgw)에서 구성한 VPN Gateway 가상 네트워크 게이트웨이의 BGP 피어 IP 주소 및 ASN과 동일 해야 합니다.
4. 검토 하 고 **확인** 을 선택 하 여 사이트를 만듭니다.
5. 이전 단계를 반복 하 여 두 번째 사이트를 만들고 VPN Gateway 가상 네트워크 게이트웨이의 두 번째 인스턴스와 일치 시킵니다. VPN Gateway 구성의 두 번째 공용 IP 주소와 두 번째 BGP 피어 IP 주소를 사용 하는 경우를 제외 하 고 동일한 설정을 유지 합니다.
6. 이제 두 사이트가 성공적으로 프로 비전 되 고 다음 섹션으로 이동 하 여 구성 파일을 다운로드할 수 있습니다.

## <a name="3-download-the-vpn-configuration-files"></a><a name="downloadconfig"></a>3. VPN 구성 파일을 다운로드 합니다.

이 섹션에서는 이전 섹션에서 만든 각 사이트에 대 한 VPN 구성 파일을 다운로드 합니다.

1. 가상 WAN **vpn 사이트** 페이지의 맨 위에서 **사이트** 를 선택 하 고 **사이트 간 vpn 구성 다운로드** 를 선택 합니다. Azure는 설정을 사용 하 여 구성 파일을 만듭니다.

   !["사이트 간 VPN 구성 다운로드" 작업이 선택 된 "VPN 사이트" 페이지를 보여 주는 스크린샷](./media/connect-virtual-network-gateway-vwan/download.png "다운로드")
2. 구성 파일을 다운로드 하 여 엽니다.
3. 두 번째 사이트에 대해 이러한 단계를 반복 합니다. 두 구성 파일을 모두 연 후에는 다음 섹션으로 진행할 수 있습니다.

## <a name="4-create-the-local-network-gateways"></a><a name="createlocalgateways"></a>4. 로컬 네트워크 게이트웨이 만들기

이 섹션에서는 두 개의 Azure VPN Gateway 로컬 네트워크 게이트웨이를 만듭니다. 이전 단계의 구성 파일에는 게이트웨이 구성 설정이 포함 되어 있습니다. 이러한 설정을 사용 하 여 Azure VPN Gateway 로컬 네트워크 게이트웨이를 만들고 구성 합니다.

1. 이러한 설정을 사용 하 여 로컬 네트워크 게이트웨이를 만듭니다. VPN Gateway 로컬 네트워크 게이트웨이를 만드는 방법에 대 한 자세한 내용은 VPN Gateway 문서 [로컬 네트워크 게이트웨이 만들기](../vpn-gateway/tutorial-site-to-site-portal.md#LocalNetworkGateway)를 참조 하세요.

   * **IP 주소** -구성 파일에서 *게이트웨이 구성* 에 표시 된 Instance0 IP 주소를 사용 합니다.
   * **Bgp** -연결이 bgp를 통해 설정 된 경우 **bgp 설정 구성** 을 선택 하 고 ASN ' 65515 '을 입력 합니다. BGP 피어 IP 주소를 입력 합니다. 구성 파일에서 *게이트웨이 구성* 에 ' Instance0 BgpPeeringAddresses '를 사용 합니다.
   * **구독, 리소스 그룹 및 위치** 는 가상 WAN 허브의 경우와 동일 합니다.
2. 로컬 네트워크 게이트웨이를 검토 하 고 만듭니다. 로컬 네트워크 게이트웨이는이 예제와 유사 하 게 표시 됩니다.

   ![IP 주소가 강조 표시 되 고 "BGP 설정 구성"이 선택 된 "구성" 페이지를 보여 주는 스크린샷](./media/connect-virtual-network-gateway-vwan/lng1.png "instance0")
3. 다른 로컬 네트워크 게이트웨이를 만들려면이 단계를 반복 합니다. 그러나 이번에는 구성 파일에서 ' Instance0 ' 값 대신 ' Instance1 ' 값을 사용 합니다.

   ![구성 파일 다운로드](./media/connect-virtual-network-gateway-vwan/lng2.png "instance1")

## <a name="5-create-connections"></a><a name="createlocalgateways"></a>5. 연결 만들기

이 섹션에서는 VPN Gateway 로컬 네트워크 게이트웨이와 가상 네트워크 게이트웨이 간의 연결을 만듭니다. VPN Gateway 연결을 만드는 방법에 대 한 단계는 [연결 구성](../vpn-gateway/tutorial-site-to-site-portal.md#CreateConnection)을 참조 하세요.

1. 포털에서 가상 네트워크 게이트웨이로 이동 하 고 **연결** 을 클릭 합니다. 연결 페이지의 맨 위에 있는 **+추가** 를 클릭하여 **연결 추가** 페이지를 엽니다.
2. **연결 추가** 페이지에서 연결에 대해 다음 값을 구성 합니다.

   * **이름:** 연결의 이름을 지정합니다.
   * **연결 형식:** **사이트 간 (IPSec)을 선택 합니다** .
   * **가상 네트워크 게이트웨이:** 이 게이트웨이에서 연결하기 때문에 값이 고정됩니다.
   * **로컬 네트워크 게이트웨이:** 이 연결을 통해 가상 네트워크 게이트웨이를 로컬 네트워크 게이트웨이에 연결 합니다. 이전에 만든 로컬 네트워크 게이트웨이 중 하나를 선택 합니다.
   * **공유 키:** 공유 키를 입력 하십시오.
   * **IKE 프로토콜:** IKE 프로토콜을 선택 합니다.
3. **확인** 을 클릭하여 연결을 만듭니다.
4. 가상 네트워크 게이트웨이의 **연결** 페이지에서 연결을 볼 수 있습니다.

   ![연결](./media/connect-virtual-network-gateway-vwan/connect.png "connection")
5. 이전 단계를 반복 하 여 두 번째 연결을 만듭니다. 두 번째 연결의 경우 만든 다른 로컬 네트워크 게이트웨이를 선택 합니다.
6. 연결이 BGP를 통해 연결 된 경우 연결을 만든 후 연결로 이동 하 여 **구성** 을 선택 합니다. **구성** 페이지에서 **BGP** 에 대해 **사용** 을 선택 합니다. 그런 다음 **저장** 을 클릭합니다. 두 번째 연결에 대해 반복 합니다.

## <a name="6-test-connections"></a><a name="test"></a>6. 연결 테스트

두 가상 컴퓨터 VPN Gateway를 만들고 가상 네트워크 게이트웨이의 가상 네트워크에 있는 가상 컴퓨터 하나를 만든 다음 두 가상 컴퓨터를 ping 하 여 연결을 테스트할 수 있습니다.

1. Azure VPN Gateway (Test1-VNG)의 가상 네트워크 (Test1-VNet)에서 가상 컴퓨터를 만듭니다. 가상 컴퓨터를 만들지 마십시오.
2. 가상 WAN에 연결 하는 데 다른 가상 네트워크를 만듭니다. 이 가상 네트워크의 서브넷에 가상 컴퓨터를 만듭니다. 이 가상 네트워크는 가상 네트워크 게이트웨이를 포함할 수 없습니다. [사이트 간 연결](virtual-wan-site-to-site-portal.md#vnet) 문서의 PowerShell 단계를 사용 하 여 가상 네트워크를 빠르게 만들 수 있습니다. Cmdlet을 실행 하기 전에 값을 변경 해야 합니다.
3. 가상 WAN 허브에 VNet을 연결 합니다. 가상 WAN에 대 한 페이지에서 **가상 네트워크 연결** 을 선택 하 고 **+ 연결 추가** 를 선택 합니다. **연결 추가** 페이지에서 다음 필드를 채웁니다.

    * **연결 이름** - 연결의 이름을 지정합니다.
    * **허브** - 이 연결과 연결할 허브를 선택합니다.
    * **구독** - 구독을 확인합니다.
    * **가상 네트워크** - 이 허브에 연결할 가상 네트워크를 선택합니다. 가상 네트워크에는 기존의 가상 네트워크 게이트웨이를 사용할 수 없습니다.
4. **확인** 을 클릭하여 가상 네트워크 연결을 만듭니다.
5. 이제 Vm 간에 연결이 설정 됩니다. 통신을 차단 하는 방화벽이 나 기타 정책이 없는 한 VM 하나를 ping 할 수 있어야 합니다.

## <a name="next-steps"></a>다음 단계

사용자 지정 IPsec 정책을 구성 하는 단계는 [가상 WAN에 대 한 사용자 지정 ipsec 정책 구성](virtual-wan-custom-ipsec-portal.md)을 참조 하세요.
Virtual WAN에 대한 자세한 내용은 [Azure Virtual WAN 정보](virtual-wan-about.md) 및 [Azure Virtual WAN FAQ](virtual-wan-faq.md)를 참조하세요.