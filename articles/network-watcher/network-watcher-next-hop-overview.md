---
title: Azure Network Watcher에서 다음 홉 소개 | Microsoft Docs
description: 이 문서는 Network Watcher 다음 홉 기능에 대한 개요를 제공합니다.
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 33c22b5b68b6677e8cf271dc185007316ec44500
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "76844061"
---
# <a name="use-next-hop-to-diagnose-virtual-machine-routing-problems"></a>다음 홉을 사용하여 가상 머신 라우팅 문제 진단

VM(가상 머신)의 트래픽은 NIC(네트워크 인터페이스)와 연결된 유효 경로에 따라 대상에 전송됩니다. 다음 홉은 특정 VM 및 NIC에서 다음 홉 유형 및 패킷의 IP 주소를 가져옵니다. 다음 홉을 알고 있으면 트래픽이 의도한 대상으로 향하는지 트래픽이 다른 곳으로 전송되는지 여부를 확인할 수 있습니다. 부적절한 경로의 구성으로 연결 문제가 발생할 수 있습니다. 여기서 트래픽은 온-프레미스 위치 또는 가상 어플라이언스로 향합니다. 또한 다음 홉은 다음 홉과 연결된 경로 테이블을 반환합니다. 경로가 사용자 정의 경로로 정의된 경우 해당 경로가 반환됩니다. 그렇지 않은 경우 다음 홉은 **시스템 경로** 를 반환합니다.

![다음 홉 개요](./media/network-watcher-next-hop-overview/figure1.png)

다음 홉 기능에서 반환될 수 있는 다음 홉은 다음과 같습니다.

* 인터넷
* VirtualAppliance
* VirtualNetworkGateway
* VirtualNetwork
* VirtualNetworkPeering
* VirtualNetworkServiceEndpoint 
* MicrosoftEdge
* 없음

각 다음 홉 형식에 대해 자세히 알아보려면 [라우팅 개요](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)를 참조하세요.

## <a name="next-steps"></a>다음 단계

다음 홉을 사용하여 VM 네트워크 라우팅 문제를 진단하는 방법을 알아보려면 [Azure Portal](diagnose-vm-network-routing-problem.md), [PowerShell](diagnose-vm-network-routing-problem-powershell.md) 또는 [Azure CLI](diagnose-vm-network-routing-problem-cli.md)를 사용하여 VM 네트워크 라우팅 문제 진단을 참조하세요.
