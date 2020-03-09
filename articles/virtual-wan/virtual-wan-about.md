---
title: Azure Virtual WAN 개요 | Microsoft Docs
description: Virtual WAN의 자동화된 확장성 있는 지점 간 연결, 사용 가능한 지역 및 파트너에 대해 알아봅니다.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 02/05/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 9ac70252ce7c818ccbdecfd996b9970f011aa967
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78379389"
---
# <a name="about-azure-virtual-wan"></a>Azure Virtual WAN 정보

Azure Virtual WAN은 Azure를 통해 최적화된 자동 분기 연결을 제공하는 네트워킹 서비스입니다. Azure 지역은 분기를 연결하도록 선택할 수 있는 허브 역할을 합니다. 또한 Azure 백본을 활용하여 분기를 연결하고 분기 및 VNet 간 연결을 설정할 수 있습니다. Azure Virtual WAN VPN을 사용하여 연결 자동화를 지원하는 파트너 목록이 있습니다. 자세한 내용은 [Virtual WAN 파트너 및 위치](virtual-wan-locations-partners.md) 문서를 참조하세요.

Azure Virtual WAN은 사이트 간 VPN, 사용 VPN(지점 및 사이트 간) 및 ExpressRoute와 같은 다양한 Azure 클라우드 연결 서비스를 단일 운영 인터페이스로 통합합니다. Azure VNet에 대한 연결은 가상 네트워크 연결을 사용하여 설정합니다. 이를 통해 기본 허브 및 스포크 연결 모델을 기반으로 하는 [글로벌 전송 네트워크 아키텍처](virtual-wan-global-transit-network-architecture.md)가 가능합니다. 기본 허브 및 스포크 연결 모델에서는 클라우드 호스팅된 네트워크 '허브'가 여러 유형의 '스포크'에 분산될 수 있는 엔드포인트 간의 전이적 연결을 지원합니다.

![Virtual WAN 다이어그램](./media/virtual-wan-about/virtualwan1.png)

이 문서에서는 Azure Virtual WAN의 네트워크 연결에 대해 간략하게 설명합니다. Virtual WAN은 다음과 같은 이점을 제공합니다.

* **허브 및 스포크의 통합 연결 솔루션:** 온-프레미스 사이트와 Azure 허브 간의 사이트 간 구성 및 연결을 자동화 합니다.
* **자동화된 스포크 설치 및 구성:** Azure 허브로 가상 네트워크 및 워크로드를 원활하게 연결합니다.
* **직관적인 문제 해결:** Azure 내에서 종단 간 흐름을 확인 한 다음이 정보를 사용 하 여 필요한 작업을 수행할 수 있습니다.

## <a name="basicstandard"></a>기본 및 표준 가상 WAN

가상 Wan에는 기본 및 표준의 두 가지 유형이 있습니다. 다음 표에서는 각 유형에 사용 가능한 구성을 보여 줍니다.

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

가상 WAN을 업그레이드하는 단계는 [기본에서 표준으로 가상 WAN 업그레이드](upgrade-virtual-wan.md)를 참조하세요.

## <a name="architecture"></a>아키텍처

Virtual WAN 아키텍처 및 Virtual WAN으로 마이그레이션하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Virtual WAN 아키텍처](migrate-from-hub-spoke-topology.md)
* [글로벌 전송 네트워크 아키텍처](virtual-wan-global-transit-network-architecture.md)

## <a name="resources"></a>Virtual WAN 리소스

엔드투엔드 가상 WAN을 구성하려면 다음 리소스를 만듭니다.

* **virtualWAN:** virtualWAN 리소스는 Azure 네트워크의 가상 오버레이를 나타내며 여러 리소스의 컬렉션입니다. 가상 WAN 내에서 가지려는 모든 가상 허브에 대한 링크를 포함합니다. Virtual WAN 리소스는 서로 격리되며 공통의 허브를 포함할 수 없습니다. Virtual WAN에서 가상 허브는 서로 통신하지 않습니다.

* **허브:** 가상 허브는 Microsoft에서 관리하는 가상 네트워크입니다. 허브에는 연결을 활성화하는 다양한 서비스 엔드포인트가 있습니다. 사용자의 온-프레미스 네트워크(vpnsite)에서 가상 허브 내의 VPN Gateway에 연결하거나, 가상 허브에 ExpressRoute 회로를 연결할 수 있으며 가상 허브의 지점 및 사이트 간 게이트웨이에 모바일 사용자를 연결할 수도 있습니다. 허브는 지역에서 네트워크의 핵심입니다. Azure 지역당 하나의 허브만 있을 수 있습니다.

  허브 게이트웨이는 ExpressRoute 및 VPN Gateway에 사용하는 가상 네트워크 게이트웨이와 동일하지 않습니다. 예를 들어 Virtual WAN을 사용하는 경우 온-프레미스 사이트에서 VNet으로의 직접적인 사이트 간 연결을 만들지 않습니다. 대신 사이트 간 연결을 허브에 만듭니다. 트래픽은 항상 허브 게이트웨이를 통해 이동합니다. VNet에 자체 가상 네트워크 게이트웨이가 필요하지 않다는 것을 의미합니다. Virtual WAN을 사용하면 VNet은 가상 허브와 가상 허브 게이트웨이를 통해 쉽게 크기 조정을 활용할 수 있습니다.

* **허브 가상 네트워크 연결:** 허브 가상 네트워크 연결 리소스는 가상 네트워크에 허브를 원활하게 연결하는 데 사용됩니다.

* **(미리 보기) 허브-허브 연결** - 가상 WAN에서 모든 허브가 서로 연결됩니다. 따라서 로컬 허브에 연결된 분기, 사용자 또는 VNet이 연결된 허브의 풀 메시 아키텍처를 사용하여 다른 분기나 VNet과 통신할 수 있습니다. 가상 허브를 통해 전송 중인 허브 내의 VNet을 연결할 수 있을 뿐만 아니라 허브-허브 연결 프레임워크를 사용하여 허브 전체의 VNet을 연결할 수도 있습니다.

* **허브 경로 테이블:** 가상 허브 경로를 만들어 가상 허브 경로 테이블에 적용할 수 있습니다. 가상 허브 경로 테이블에 여러 경로를 적용할 수 있습니다.

**추가 Virtual WAN 리소스**

  * **사이트:** 이 리소스는 사이트 간 연결에만 사용 됩니다. 사이트 리소스는 **vpnsite**입니다. 온-프레미스 VPN 디바이스와 해당 설정을 나타냅니다. Virtual WAN 파트너와 작업하여 이 정보를 Azure로 자동으로 내보내는 기본 제공 솔루션을 갖습니다.

## <a name="connectivity"></a>연결 유형

가상 WAN은 사이트 간 VPN, 사용자 VPN (지점 및 사이트 간) 및 Express 경로와 같은 유형의 연결을 허용 합니다.

### <a name="s2s"></a>사이트 간 VPN 연결

![Virtual WAN 다이어그램](./media/virtual-wan-about/virtualwan.png)

가상 WAN 사이트 간 연결을 만드는 경우 사용 가능한 파트너를 사용할 수 있습니다. 파트너를 사용하지 않으려면 연결을 수동으로 구성하면 됩니다. 자세한 내용은 [Virtual WAN을 사용하여 사이트 간 연결 만들기](virtual-wan-site-to-site-portal.md)를 참조하세요.

#### <a name="s2spartner"></a>Virtual WAN 파트너 워크플로

Virtual WAN 파트너를 사용하는 경우 워크플로는 다음과 같습니다.

1. 분기 디바이스(VPN/SDWAN) 컨트롤러가 [Azure 서비스 주체](../active-directory/develop/howto-create-service-principal-portal.md)를 사용하여 Azure에 사이트 중심 정보를 내보내도록 인증됩니다.
2. 분기 디바이스(VPN/SDWAN) 컨트롤러는 Azure 연결 구성을 가져오고 로컬 디바이스를 업데이트합니다. 구성 다운로드, 온-프레미스 VPN 디바이스의 편집 및 업데이트를 자동화합니다.
3. 디바이스에 올바른 Azure 구성이 적용되면 Azure WAN에 대한 사이트 간 연결(활성 터널 2개)이 설정됩니다. Azure는 IKEv1 및 IKEv2를 둘 다 지원합니다. BGP는 선택 사항입니다.

#### <a name="partners"></a>사이트 간 가상 WAN 연결을 위한 파트너

사용 가능한 파트너와 위치의 목록은 [Virtual WAN 파트너 및 위치](virtual-wan-locations-partners.md) 문서를 참조하세요.

### <a name="uservpn"></a>사용자 VPN(지점 및 사이트 간) 연결

IPsec/IKE(IKEv2) 또는 OpenVPN 연결을 통해 Azure의 리소스에 연결할 수 있습니다. 이 연결 유형은 클라이언트 컴퓨터에서 VPN 클라이언트를 구성해야 합니다. 자세한 내용은 [지점 및 사이트 간 연결 만들기](virtual-wan-point-to-site-portal.md)를 참조하세요.

### <a name="er"></a>ExpressRoute 연결
ExpressRoute를 사용하면 프라이빗 연결을 통해 온-프레미스 네트워크를 Azure에 연결할 수 있습니다. 연결을 만들려면 [Virtual WAN을 사용하여 ExpressRoute 연결 만들기](virtual-wan-expressroute-portal.md)를 참조하세요.

## <a name="locations"></a>위치

위치 정보는 [Virtual WAN 파트너 및 위치](virtual-wan-locations-partners.md) 문서를 참조하세요.

## <a name="faq"></a>FAQ

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>다음 단계

[Virtual WAN을 사용하여 사이트 간 연결 만들기](virtual-wan-site-to-site-portal.md)
