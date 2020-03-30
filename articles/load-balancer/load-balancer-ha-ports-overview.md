---
title: Azure의 고가용성 포트 개요
titleSuffix: Azure Load Balancer
description: 내부 부하 분산 장치에서 고가용성 포트 부하 분산에 대해 자세히 알아봅니다.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2019
ms.author: allensu
ms.openlocfilehash: 5ada709350802344bfa65cce269735baa416edf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234442"
---
# <a name="high-availability-ports-overview"></a>고가용성 포트 개요

Azure Standard Load Balancer는 내부 부하 분산 장치를 사용하는 경우 모든 포트에서 TCP 및 UDP 흐름의 부하를 동시에 분산하도록 도와줍니다. 

고가용성(HA) 포트 로드 밸런싱 규칙은 내부 표준 부하 분산기에서 구성된 부하 분산 규칙의 변형입니다. 내부 Standard Load Balancer의 모든 포트에 도달하는 모든 TCP 및 UDP 흐름의 부하를 분산하기 위한 단일 규칙을 제공하여 부하 분산 장치 사용을 간소화할 수 있습니다. 부하 분산 의사 결정은 흐름 단위로 이루어집니다. 원본 IP 주소, 원본 포트, 대상 IP 주소, 대상 포트 및 프로토콜의 5 튜플 연결을 기준으로 합니다.

HA 포트 로드 밸런싱 규칙은 가상 네트워크 내의 NV(네트워크 가상 어플라이언스)에 대한 고가용성 및 확장과 같은 중요한 시나리오를 수행하는 데 도움이 됩니다. 이 기능은 많은 수의 포트에서 부하를 분산시켜야 할 때도 도움이 될 수 있습니다. 

HA 포트 로드 밸런싱 규칙은 프런트 엔드 및 백 엔드 포트를 **0으로** 설정하고 프로토콜을 **모두로**설정할 때 구성됩니다. 그러면 내부 Load Balancer 리소스가 포트 번호에 관계없이 모든 TCP 및 UDP 흐름의 부하를 분산합니다.

## <a name="why-use-ha-ports"></a>HA 포트를 사용하는 이유

### <a name="network-virtual-appliances"></a><a name="nva"></a>네트워크 가상 어플라이언스

여러 유형의 보안 위협에서 Azure 워크로드를 보호하기 위해 NVA를 사용할 수 있습니다. NVA가 이러한 시나리오에서 사용되는 경우 신뢰할 수 있고 가용성이 뛰어나며 요청 시 확장 가능해야 합니다.

내부 부하 분산 장치의 백 엔드 풀에 NVA 인스턴스를 추가하고 HA 포트 부하 분산 장치 규칙을 구성하기만 하면 이러한 목표를 달성할 수 있습니다.

NVA HA 시나리오의 경우 HA 포트는 다음과 같은 장점을 제공합니다.
- 인스턴스 상태 프로브별로 정상 인스턴스로 빠른 장애 조치(Failover)
- *n*개 활성 인스턴스로 확장 가능한 뛰어난 성능
- *n*개의 활성 및 활성-수동 시나리오
- 어플라이언스를 모니터링하기 위해 Apache ZooKeeper 노드 같은 복합 솔루션의 필요성 해소

다음 다이어그램은 허브 및 스포크 가상 네트워크 배포를 나타냅니다. 스포크는 신뢰할 수 있는 공간을 벗어나기 전에 허브 가상 네트워크 및 NVA를 통해 트래픽을 강제 터널링합니다. NVA는 HA 포트가 구성된 내부 Standard Load Balancer를 통해 지원됩니다. 그에 따라 모든 트래픽을 처리하고 전달할 수 있습니다. 다음 다이어그램에 표시로 구성된 경우 HA 포트 로드 분산 규칙은 유입 및 송신 트래픽에 대한 흐름 대칭을 추가로 제공합니다.

<a node="diagram"></a>
![HA 모드에 배포된 NAS를 갖춘 허브 및 스포크 가상 네트워크 다이어그램](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> NVA를 사용하는 경우 해당 공급자에게 HA 포트를 사용하는 최선의 방법과 지원되는 시나리오에 대해 문의하세요.

### <a name="load-balancing-large-numbers-of-ports"></a>많은 수의 포트에서 부하 분산

또한 많은 수의 포트에서 부하를 분산해야 하는 애플리케이션에 대해 HA 포트를 사용할 수도 있습니다. HA 포트와 함께 내부 [Standard Load Balancer](load-balancer-standard-overview.md)를 사용하여 이러한 시나리오를 단순화할 수 있습니다. 단일 부하 분산 규칙은 여러 개의 개별 부하 분산 규칙을 모든 포트에 대해 하나씩 대체합니다.

## <a name="region-availability"></a>지역 가용성

HA 포트 기능은 모든 글로벌 Azure 지역에서 사용할 수 있습니다.

## <a name="supported-configurations"></a>지원되는 구성

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>내부 Standard Load Balancer의 단일 비부동 IP(비 Direct Server Return) HA 포트 구성

이 구성은 기본 HA 포트 구성입니다. 다음을 수행하여 단일 프런트 엔드 IP 주소에 HA 포트 부하 분산 규칙을 구성할 수 있습니다.
1. Standard Load Balancer를 구성하는 동안 Load Balancer 규칙 구성에서 **HA 포트** 확인란을 선택합니다.
2. **부동 IP**를 **사용 안 함**으로 선택합니다.

이 구성에서는 현재 부하 분산 장치 리소스에 대해 다른 부하 분산 규칙 구성이 허용되지 않습니다. 지정된 백 엔드 인스턴스 세트에 대해 다른 내부 부하 분산 장치 리소스 구성도 허용하지 않습니다.

그러나 이 HA 포트 규칙 외에 백 엔드 인스턴스에 대한 공용 Standard Load Balancer를 구성할 수 있습니다.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>내부 Standard Load Balancer의 단일 부동 IP(Direct Server Return) HA 포트 구성

단일 프런트 엔드가 있고 **부동 IP**가 **사용**으로 설정된 **HA 포트**로 부하 분산 규칙을 사용하도록 부하 분산 장치를 유사하게 구성할 수 있습니다. 

이 구성을 사용하면 다른 부동 IP 부하 분산 규칙 및/또는 공용 부하 분산 장치를 추가할 수 있습니다. 그러나 이 구성 위에 비부동 IP HA 포트 부하 분산 구성을 사용할 수는 없습니다.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>내부 Standard Load Balancer의 다중 HA 포트 구성

동일한 백 엔드 풀에 대해 둘 이상의 HA 포트 프런트 엔드를 구성해야 하는 시나리오의 경우 다음을 수행하면 됩니다. 
- 단일 내부 Standard Load Balancer 리소스에 대해 둘 이상의 프런트 엔드 개인 IP 주소를 구성합니다.
- 각 규칙에 대해 단일 고유 프런트 엔드 IP 주소를 선택하여 여러 부하 분산 규칙을 구성합니다.
- **HA 포트** 옵션을 선택하고 모든 부하 분산 규칙에 대해 **부동 IP**를 **사용**으로 설정합니다.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>HA 포트가 있는 내부 부하 분산 장치 및 동일한 백 엔드 인스턴스의 공용 부하 분산 장치

HA 포트가 있는 단일 내부 표준 로드 밸런서와 함께 백 엔드 리소스에 대해 *하나의* 공용 표준 로드 밸런서 리소스를 구성할 수 있습니다.

>[!NOTE]
>이 기능은 현재 Azure Resource Manager 템플릿을 통해서만 사용할 수 있고 Azure Portal에서는 사용할 수 없습니다.

## <a name="limitations"></a>제한 사항

- HA 포트 로드 밸런싱 규칙은 내부 표준 부하 밸런서에만 사용할 수 있습니다.
- HA 포트 부하 분산 규칙과 비 HA 포트 부하 분산 규칙을 함께 사용할 수는 없습니다.
- 기존 IP 조각은 HA 포트 로드 밸런싱 규칙에 의해 첫 번째 패킷과 동일한 대상으로 전달됩니다.  UDP 또는 TCP 패킷을 조각화하는 IP는 지원되지 않습니다.
- 흐름 대칭(주로 NVA 시나리오의 경우)은 위의 다이어그램에 표시된 대로 사용되고 HA 포트 로드 밸런싱 규칙을 사용하는 경우에만 백 엔드 인스턴스 및 단일 NIC(및 단일 IP 구성)에서 지원됩니다. 다른 시나리오에서는 제공되지 않습니다. 즉, 둘 이상의 Load Balancer 리소스와 해당 규칙이 독립적인 의사 결정을 하며 조정되지 않음을 의미합니다. [네트워크 가상 어플라이언스](#nva)에 대한 설명과 다이어그램을 참조하세요. 여러 NIC를 사용하거나 공용 및 내부 로드 밸런서 간에 NVA를 끼우는 경우 흐름 대칭을 사용할 수 없습니다.  회신이 동일한 NVA에 도착할 수 있도록 수신 흐름을 어플라이언스의 IP로 소스 NAT 처리하여 이 문제를 해결할 수 있습니다.  그러나 위의 다이어그램에 표시된 참조 아키텍처와 단일 NIC를 사용하는 것이 좋습니다.


## <a name="next-steps"></a>다음 단계

- [내부 표준 부하 밸런서에서 HA 포트 구성](load-balancer-configure-ha-ports.md)
- [표준 로드 밸워서에 대해 알아보기](load-balancer-standard-overview.md)
