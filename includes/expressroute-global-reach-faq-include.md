---
title: 파일 포함
description: 포함 파일
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 317a480c13c5c6e00653fd61878a379df3f65ac4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67181939"
---
### <a name="what-is-expressroute-global-reach"></a>ExpressRoute Global Reach란?

ExpressRoute Global Reach는 Microsoft의 글로벌 네트워크를 통해 ExpressRoute 서비스를 통해 온-프레미스 네트워크를 연결하는 Azure 서비스입니다. 예를 들어 캘리포니아의 프라이빗 데이터 센터는 실리콘 밸리의 ExpressRoute에 연결되어 있고 텍사스의 프라이빗 데이터 센터는 댈러스의 ExpressRoute에 연결되어 있는 경우 ExpressRoute Global Reach를 사용하면 ExpressRoute 연결 2개를 통해 프라이빗 데이터 센터를 연결할 수 있으며 데이터 센터 간 트래픽은 Microsoft의 네트워크 백본을 통해 트래버스됩니다.

### <a name="how-do-i-enable-or-disable-expressroute-global-reach"></a>ExpressRoute Global Reach를 활성화 또는 비활성화하려면 어떻게 할까요?

ExpressRoute 회로를 연결하여 ExpressRoute Global Reach를 활성화합니다. 회로 연결을 끊어 기능을 해제합니다. [구성](../articles/expressroute/expressroute-howto-set-global-reach.md)을 참조하세요.

### <a name="do-i-need-expressroute-premium-for-expressroute-global-reach"></a>ExpressRoute Global Reach에 대해 ExpressRoute 프리미엄이 필요한가요?

ExpressRoute 회로가 동일한 지역에 있는 경우 ExpressRoute 프리미엄을 연결할 필요가 없습니다. 두 개의 ExpressRoute 회로가 서로 다른 지역에 있는 경우 회로 간에 연결을 설정하기 위해 두 회로 모두에 ExpressRoute 프리미엄이 필요합니다. 

### <a name="how-will-i-be-charged-for-expressroute-global-reach"></a>ExpressRoute Global Reach에 대해 어떻게 요금이 청구되나요?

ExpressRoute를 사용하면 온-프레미스 네트워크를 Microsoft 클라우드 서비스에 연결할 수 있습니다. ExpressRoute Global Reach를 사용하면 Microsoft의 글로벌 네트워크를 활용하고 기존 ExpressRoute 회로를 통해 사용자 고유의 온-프레미스 네트워크 간 연결을 설정할 수 있습니다. ExpressRoute Global Reach는 기존 ExpressRoute 서비스와 별도로 청구됩니다. 각 ExpressRoute 회로에서 이 기능을 설정하기 위한 추가 항목 요금이 있습니다. ExpressRoute Global Reach로 활성화된 온-프레미스 네트워크 간 트래픽에는 원본에서는 송신 요금이, 대상에서는 수신 요금이 청구됩니다. 이 요금은 회로가 있는 영역을 기반으로 합니다.

### <a name="where-is-expressroute-global-reach-supported"></a>ExpressRoute Global Reach가 지원되는 곳은 어디인가요?

ExpressRoute Global Reach는 [국가/지역 또는 장소 선택](../articles/expressroute/expressroute-global-reach.md)에서 지원됩니다. ExpressRoute 회로는 다음 국가/지역 또는 장소의 피어링 위치에 만들어야 합니다.

### <a name="i-have-more-than-two-on-premises-networks-each-connected-to-an-expressroute-circuit-can-i-enable-expressroute-global-reach-to-connect-all-of-my-on-premises-networks-together"></a>각각 ExpressRoute 회로에 연결된 두 개를 초과하는 온-프레미스 네트워크가 있습니다. ExpressRoute Global Reach를 사용하여 모든 온-프레미스 네트워크를 함께 연결할 수 있나요?

예, 회로가 지원되는 국가/지역에 있기만 하면 가능합니다. 한 번에 두 개의 ExpressRoute 회로를 연결해야 합니다. 완전한 메시형 네트워크를 만들려면 모든 회로 쌍을 열거하고 구성을 반복해야 합니다. 

### <a name="can-i-enable-expressroute-global-reach-between-two-expressroute-circuits-at-the-same-peering-location"></a>동일한 피어링 위치에서 두 개의 ExpressRoute 회로 간에 ExpressRoute Global Reach를 활성화할 수 있나요?

아니요. 두 회로는 서로 다른 피어링 위치의 것이어야 합니다. 지원되는 국가/지역의 대도시에 둘 이상의 ExpressRoute 피어링 위치가 있는 경우 해당 대도시의 다른 피어링 위치에서 생성된 ExpressRoute 회로를 함께 연결할 수 있습니다. 

### <a name="if-expressroute-global-reach-is-enabled-between-circuit-x-and-circuit-y-and-between-circuit-y-and-circuit-z-will-my-on-premises-networks-connected-to-circuit-x-and-circuit-z-talk-to-each-other-via-microsofts-network"></a>ExpressRoute Global Reach가 회로 X와 회로 Y 사이, 회로 Y와 회로 Z 사이 활성화되어 있는 경우 회로 X와 회로 Z에 연결된 온-프레미스 네트워크가 Microsoft 네트워크를 통해 서로 통신하나요?

아니요. 온-프레미스 네트워크 중 두 개의 네트워크 간에 연결을 설정하려면 해당 ExpressRoute 회로를 명시적으로 연결해야 합니다. 위의 예제에서 회로 X와 회로 Z를 연결해야 합니다. 

### <a name="what-is-the-network-throughput-i-can-expect-between-my-on-premises-networks-after-i-enable-expressroute-global-reach"></a>ExpressRoute Global Reach를 활성화한 후 온-프레미스 네트워크 간에 기대할 수 있는 네트워크 처리량은 어느 정도인가요?

ExpressRoute Global Reach로 설정된 온-프레미스 네트워크 간의 네트워크 처리량은 두 ExpressRoute 회로 중 작은 것으로 제한됩니다. 프레미스와 Azure 간 트래픽 및 프레미스 간 트래픽은 동일한 회로를 공유하며 동일한 대역폭 제한이 적용됩니다. 

### <a name="with-expressroute-global-reach-what-are-the-limits-on-the-number-of-routes-i-can-advertise-and-the-number-of-routes-i-will-receive"></a>ExpressRoute Global Reach를 사용할 경우 보급할 수 있는 경로 수와 수신하는 경로 수는 몇 개로 제한되나요?

Azure 프라이빗 피어링에 대해 Microsoft로 보급할 수 있는 경로 수는 Standard 회로에서는 4000개, Premium 회로에서는 10,000개로 유지됩니다. Azure 프라이빗 피어링에 대해 Microsoft로부터 수신하는 경로 수는 Azure Virtual Network의 경로와 ExpressRoute Global Reach를 통해 연결된 다른 온-프레미스 네트워크의 경로 수를 합한 것입니다. 온-프레미스 라우터에서 적절한 최대 접두사 제한을 설정해야 합니다. 

### <a name="what-is-the-sla-for-expressroute-global-reach"></a>ExpressRoute Global Reach에 대한 SLA란 무엇인가요?

ExpressRoute Global Reach에서는 동일한 [가용성 SLA](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/)를 일반 ExpressRoute 서비스로 제공합니다.
