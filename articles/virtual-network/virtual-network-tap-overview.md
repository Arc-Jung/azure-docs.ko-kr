---
title: Azure 가상 네트워크 TAP 개요 | Microsoft Docs
description: 가상 네트워크 TAP에 대해 알아봅니다. 가상 네트워크 TAP은 패킷 수집기에 스트리밍될 수 있는 가상 머신 네트워크 트래픽의 전체 복사본을 제공합니다.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2019
ms.author: kaanan
ms.openlocfilehash: 47db03460ad3c5194a5445f0b25cb8e742e60c21
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79279612"
---
# <a name="virtual-network-tap"></a>가상 네트워크 TAP

Azure 가상 네트워크 TAP(터미널 액세스 지점)을 사용하면 네트워크 패킷 수집기 또는 분석 도구로 가상 머신 네트워크 트래픽을 지속적으로 스트리밍할 수 있습니다. 수집기 또는 분석 도구는 [네트워크 가상 어플라이언스](https://azure.microsoft.com/solutions/network-appliances/) 파트너를 통해 제공됩니다. 가상 네트워크 TAP 사용의 유효성이 검사된 파트너 솔루션 목록은 [파트너 솔루션](#virtual-network-tap-partner-solutions)을 참조하세요.

> [!IMPORTANT]
> 가상 네트워크 탭은 현재 모든 Azure 지역에서 미리 보기로 제공 됩니다. 가상 네트워크 탭을 사용 하려면 구독 ID를 사용 하 여 <azurevnettap@microsoft.com>에 전자 메일을 전송 하 여 미리 보기에 등록 해야 합니다. 구독이 등록되면 이메일 회신을 받게 됩니다. 확인 이메일을 받을 때까지는 기능을 사용할 수 없습니다. 이 미리 보기는 서비스 수준 계약 없이 제공 되며 프로덕션 워크 로드에 사용할 수 없습니다. 특정 기능은 지원되지 않을 수 있거나, 기능이 제한될 수 있거나 모든 Azure 위치에서 사용하지는 못할 수 있습니다. 자세한 내용은 [Microsoft Azure 미리 보기 에 대 한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 을 참조 하세요.

## <a name="virtual-network-tap-partner-solutions"></a>가상 네트워크 TAP 파트너 솔루션

### <a name="network-packet-brokers"></a>네트워크 패킷 브로커

- [Big Switch Big Monitoring Fabric](https://www.bigswitch.com/products/big-monitoring-fabric/public-cloud/microsoft-azure)
- [Gigamon GigaSECURE](https://blog.gigamon.com/2018/09/13/why-microsofts-new-vtap-service-works-even-better-with-gigasecure-for-azure)
- [Ixia CloudLens](https://www.ixiacom.com/cloudlens/cloudlens-azure)
- [Nubeva Prisms](https://www.nubeva.com/azurevtap)

### <a name="security-analytics-networkapplication-performance-management"></a>보안 분석, 네트워크/애플리케이션 성능 관리

- [활성 보안](https://awakesecurity.com/technology-partners/microsoft-azure/)
- [Cisco Stealthwatch 클라우드](https://blogs.cisco.com/security/cisco-stealthwatch-cloud-and-microsoft-azure-reliable-cloud-infrastructure-meets-comprehensive-cloud-security)
- [Darktrace](https://www.darktrace.com/en/azure/)
- [ExtraHop Reveal(x)](https://www.extrahop.com/partners/tech-partners/microsoft/)
- [Fidelis Cybersecurity](https://www.fidelissecurity.com/technology-partners/microsoft-azure )
- [Flowmon](https://www.flowmon.com/blog/azure-vtap)
- [NetFort LANGuardian](https://www.netfort.com/languardian/solutions/visibility-in-azure-network-tap/)
- [Netscout vSTREAM]( https://www.netscout.com/technology-partners/microsoft/azure-vtap)
- [Riverbed SteelCentral AppResponse]( https://www.riverbed.com/products/steelcentral/steelcentral-appresponse-11.html)
- [RSA NetWitness® Platform](https://www.rsa.com/azure)
- [Vectra Cognito](https://vectra.ai/microsoftazure)

다음 그림은 가상 네트워크 TAP이 작동하는 방법을 보여 줍니다. 가상 네트워크에 배포된 가상 머신에 연결된 [네트워크 인터페이스](virtual-network-network-interface.md)에 TAP 구성을 추가할 수 있습니다. 대상은 모니터링된 네트워크 인터페이스 또는 [피어링된 가상](virtual-network-peering-overview.md) 네트워크와 동일한 가상 네트워크의 가상 네트워크 IP 주소입니다. 고가용성을 위해 가상 네트워크 탭에 대 한 수집기 솔루션을 Azure 내부 부하 분산 장치 뒤에 배포할 수 있습니다. 개별 솔루션에 대한 배포 옵션을 평가하려면 [파트너 솔루션](#virtual-network-tap-partner-solutions)을 참조하세요.

![가상 네트워크 TAP이 작동하는 방법](./media/virtual-network-tap/architecture.png)

## <a name="prerequisites"></a>사전 요구 사항

가상 네트워크 탭을 만들기 전에 미리 보기에 등록 된 확인 메일을 수신 하 고, [Azure Resource Manager](../azure-resource-manager/management/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 배포 모델을 사용 하 여 만든 하나 이상의 가상 컴퓨터와 동일한 Azure 지역에서 탭 트래픽을 집계 하기 위한 파트너 솔루션을 받아야 합니다. 가상 네트워크에 파트너 솔루션이 없다면 [파트너 솔루션](#virtual-network-tap-partner-solutions)을 참조하여 하나의 솔루션을 배포합니다. 동일한 가상 네트워크 TAP 리소스를 사용하여 동일하거나 다른 구독의 여러 네트워크 인터페이스에서 트래픽을 집계할 수 있습니다. 모니터링된 네트워크 인터페이스가 다른 구독에 있는 경우 구독은 동일한 Azure Active Directory 테넌트에 연결되어야 합니다. 또한 모니터링된 네트워크 인터페이스 및 TAP 트래픽 집계를 위한 대상 엔드포인트는 동일한 지역의 피어링된 가상 네트워크에 있을 수 있습니다. 이 배포 모델을 사용하는 경우 [가상 네트워크 피어링](virtual-network-peering-overview.md)이 가상 네트워크 TAP을 구성하기 전에 사용하도록 설정되어 있어야 합니다.

## <a name="permissions"></a>사용 권한

네트워크 인터페이스에 TAP 구성을 적용하는 데 사용하는 계정은 다음 표에서 필요한 작업이 할당된 [네트워크 기여자](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) 역할 또는 [사용자 지정](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 역할에 할당되어야 합니다.

| 작업 | 속성 |
|---|---|
| Microsoft.Network/virtualNetworkTaps/* | 가상 네트워크 TAP 리소스를 만들고,업데이트하고, 읽고, 삭제해야 함 |
| Microsoft.Network/networkInterfaces/read | TAP이 구성될 네트워크 인터페이스 리소스를 읽어야 함 |
| Microsoft.Network/tapConfigurations/* | 네트워크 인터페이스에서 TAP 구성을 만들고, 업데이트하고, 읽고, 삭제해야 함 |

## <a name="next-steps"></a>다음 단계

- [가상 네트워크 TAP을 만드는](tutorial-tap-virtual-network-cli.md) 방법에 대해 알아봅니다.
