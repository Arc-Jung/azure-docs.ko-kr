---
title: Azure 가상 네트워크의 공용 IPv6 주소 접두사
titlesuffix: Azure Virtual Network
description: Azure 가상 네트워크의 공용 IPv6 주소 접두사에 대해 알아봅니다.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: 3e3d3c01ba1190871da39ce27de6a95952d09be1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99538209"
---
# <a name="reserved-public-ipv6-address-prefix"></a>예약 된 공용 IPv6 주소 접두사

Azure에서 이중 스택 (IPv4 + IPv6) 가상 네트워크 (VNet) 및 Vm (가상 머신)은 인터넷에 연결 되어 있지 않기 때문에 기본적으로 안전 합니다. Azure에서 가져온 공용 IPv6 주소를 사용 하 여 Azure 부하 분산 장치 및 Vm에 대 한 IPv6 인터넷 연결을 쉽게 추가할 수 있습니다.

예약 된 모든 공용 Ip는 선택한 Azure 지역 및 Azure 구독과 연결 됩니다. 구독에서 Azure 부하 분산 장치 또는 Vm 간에 예약 된 (정적) IPv6 공용 IP를 이동할 수 있습니다. IPv6 공용 IP를 완전히 분리 하 고 준비가 되 면 사용할 수 있도록 유지 됩니다.

> [!WARNING]
> 실수로 공용 IP 주소를 삭제 하지 않도록 주의 해야 합니다. 공용 IP를 삭제 하면 구독에서 제거 되 고 Azure 지원의 도움을 받을 수 없는 경우에도 복구할 수 없습니다.

개별 IPv6 주소를 예약 하는 것 외에도 사용에 대 한 Azure IPv6 주소의 연속 범위 (IP 접두사 라고도 함)를 예약할 수 있습니다.  개별 IP 주소와 마찬가지로, 예약 된 접두사는 선택한 Azure 지역 및 Azure 구독과 연결 됩니다. 예측 가능 하 고 연속 된 주소 범위는 많은 용도로 예약 됩니다. 예를 들어 고정 IP 범위를 온-프레미스 방화벽으로 쉽게 프로그래밍할 수 있으므로 회사 및 고객이 Azure에서 호스트 하는 응용 프로그램의 IP *필터링* 을 크게 간소화할 수 있습니다.  필요에 따라 IP 접두사에서 개별 공용 ip를 만들 수 있으며, 이러한 개별 공용 Ip를 삭제 하는 경우 나중에 다시 사용할 수 있도록 예약 된 범위로 *반환* 됩니다. IP 접두사의 모든 IP 주소는 접두사를 삭제할 때까지 독점적으로 사용 하도록 예약 됩니다.



## <a name="ipv6-prefix-sizes"></a>IPv6 접두사 크기
사용할 수 있는 공용 IP 접두사 크기는 다음과 같습니다.

-  최소 IPv6 접두사 크기:/127 = 2 개 주소
-  최대 IPv6 접두사 크기:/124 = 16 개 주소

접두사 크기가 CIDR (클래스 없는 Inter-Domain 라우팅) 마스크 크기로 지정 되어 있습니다. 예를 들어/128의 마스크는 IPv6 주소를 128 비트로 구성 하므로 개별 IPv6 주소를 나타냅니다.

## <a name="pricing"></a>가격
 
개별 IP 주소와 IP 범위 모두에서 Azure 공용 ip를 사용 하는 것과 관련 된 비용은 [공용 Ip 주소 가격 책정](https://azure.microsoft.com/pricing/details/ip-addresses/)을 참조 하세요.

## <a name="limitations"></a>제한 사항
IPv6은 Azure에서 응용 프로그램 (VM 또는 부하 분산 장치)을 삭제 하 고 다시 배포 하는 경우 IPv6 주소가 변경 됨을 의미 하는 "동적" 할당 으로만 기본 공용 Ip에서 지원 됩니다. 표준 IPv6 공용 IP는 정적 (예약 된) 할당만 지원 합니다. 단, 표준 내부 부하 분산 장치는 할당 된 서브넷 내에서 동적 할당을 지원할 수도 있습니다.  

가장 좋은 방법은 IPv6 응용 프로그램에 표준 공용 Ip와 표준 부하 분산 장치를 사용 하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계
- 공용 [IPv6 주소 접두사](ipv6-reserve-public-ip-address-prefix.md)를 예약 합니다.
- [IPv6 주소](ipv6-overview.md)에 대해 자세히 알아보세요.
- Azure에서 공용 Ip (IPv4 및 IPv6 모두) [를 만들고 사용 하는 방법](virtual-network-public-ip-address.md) 에 대해 알아봅니다.
