---
title: Azure DDoS Protection 표준 질문과 대답
description: DDoS 공격에 대 한 방어를 제공 하는 데 도움이 되는 Azure DDoS Protection 표준에 대 한 질문과 대답입니다.
services: virtual-network
documentationcenter: na
author: yitoh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/28/2020
ms.author: yitoh
ms.openlocfilehash: b53501bf5aa4bafb0229158edfe0cb244116b909
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98600848"
---
# <a name="azure-ddos-protection-standard-frequent-asked-questions"></a>Azure DDoS Protection 표준 질문과 대답

이 문서에서는 Azure DDoS Protection 표준에 대 한 일반적인 질문에 답변 합니다. 

## <a name="what-is-a-distributed-denial-of-service-ddos-attack"></a>DDoS (분산 서비스 거부) 공격 이란?
배포 된 서비스 거부 또는 DDoS은 공격자가 응용 프로그램이 처리할 수 있는 것 보다 많은 요청을 응용 프로그램에 전송 하는 공격 유형입니다. 결과는 리소스가 고갈 되어 응용 프로그램의 가용성 및 고객 서비스 기능에 영향을 주는 영향을 받습니다. 지난 몇 년 동안 업계에서는 공격이 점점 더 정교 해지고 크기가 더 커지고 있습니다. 인터넷을 통해 공개적으로 도달 가능한 모든 엔드포인트는 DDoS 공격의 대상이 될 수 있습니다.

## <a name="what-is-azure-ddos-protection-standard-service"></a>Azure DDoS Protection Standard 서비스는 무엇 인가요?
응용 프로그램 설계 모범 사례와 결합 된 Azure DDoS Protection 표준은 DDoS 공격 으로부터 보호 하는 향상 된 DDoS 완화 기능을 제공 합니다. 가상 네트워크의 특정 Azure 리소스를 보호 하도록 자동으로 조정 됩니다. 보호는 새 가상 네트워크 또는 기존 가상 네트워크에서 간단하게 설정할 수 있으며 애플리케이션 또는 리소스를 변경할 필요가 없습니다. 로깅, 경고 및 원격 분석을 포함하여 기본 서비스에 비해 여러 가지 장점이 있습니다. 자세한 내용은 [Azure DDoS Protection 표준 개요](ddos-protection-overview.md) 를 참조 하세요. 

## <a name="how-does-pricing-work"></a>가격은 어떻게 책정됩니까?
DDoS 보호 계획에는 최대 100 개의 공용 IP 주소를 포함 하는 월 $2944의 고정 된 월별 요금이 적용 됩니다. 추가 리소스에 대 한 보호는 월별 리소스 당 $30을 추가로 청구 합니다. 

테 넌 트에서 단일 DDoS 보호 계획을 여러 구독에서 사용할 수 있으므로 둘 이상의 DDoS 보호 계획을 만들 필요가 없습니다.

자세한 내용은 [Azure DDoS Protection 표준 가격 책정](https://azure.microsoft.com/pricing/details/ddos-protection/) 을 참조 하세요.

## <a name="is-the-service-zone-resilient"></a>서비스 영역 복원 력이 있나요?
예. Azure DDoS Protection은 기본적으로 영역 복원 가능 합니다.

## <a name="how-do-i-configure-the-service-to-be-zone-resilient"></a>서비스를 영역 복원 력이 되도록 구성 어떻게 할까요?
영역 복원력을 사용하도록 설정하기 위한 고객 구성은 필요 없습니다. 영역-Azure DDoS Protection 리소스에 대 한 복원 력은 기본적으로 제공 되며 서비스 자체에서 관리 됩니다.

## <a name="what-about-protection-at-the-service-layer-layer-7"></a>서비스 계층 (계층 7)의 보호는 어떻습니까?
고객은 네트워크 계층 (Azure DDoS Protection 표준에서 제공 되는 레이어 3 및 4) 및 응용 프로그램 계층 (WAF에서 제공 하는 계층 7)에서 보호를 위해 WAF (웹 응용 프로그램 방화벽)와 함께 Azure DDoS Protection 서비스를 사용할 수 있습니다. WAF 제품은 Azure [APPLICATION GATEWAY WAF SKU](../web-application-firewall/ag/ag-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 뿐만 아니라 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall)에서 사용할 수 있는 타사 웹 응용 프로그램 방화벽 제품을 포함 합니다.

## <a name="are-services-unsafe-in-azure-without-the-service"></a>서비스 없이 Azure에서 서비스를 안전 하 게 사용할 수 있나요?
Azure에서 실행 되는 서비스는 기본적으로 Azure의 인프라를 보호 하는 Azure DDoS Protection Basic에 의해 보호 됩니다. 그러나 인프라를 보호 하는 보호는 대부분의 응용 프로그램에서 처리할 수 있는 용량을 가지 며, 원격 분석 또는 경고를 제공 하지 않으므로, 트래픽 볼륨이 플랫폼에서 무해 한 것으로 인식 될 수 있지만 응용 프로그램에서이를 수신 하는 데 큰 영향을 미칠 수 있습니다. 

응용 프로그램은 Azure DDoS Protection 표준 서비스에 등록 하 여 공격 및 응용 프로그램별 임계값을 검색 하는 전용 모니터링을 가져옵니다. 서비스는 예상 되는 트래픽 볼륨에 맞게 조정 되는 프로필을 사용 하 여 보호 되므로 DDoS 공격에 대해 훨씬 더 긴밀 한 방어를 제공 합니다.

## <a name="what-are-the-supported-protected-resource-types"></a>지원 되는 보호 된 리소스 종류는 무엇 인가요?
ARM 기반 Vnet의 공용 Ip는 현재 보호 된 리소스의 유일한 유형입니다. PaaS 서비스 (다중 테 넌 트)는 제공 된에서 지원 되지 않습니다. 자세한 내용은 [Azure DDoS Protection 표준 참조 아키텍처](ddos-protection-reference-architectures.md) 를 참조 하세요.

## <a name="are-classicrdfe-protected-resources-supported"></a>클래식/RDFE 보호 된 리소스가 지원 되나요?
ARM 기반 보호 된 리소스만 미리 보기에서 지원 됩니다. 클래식/RDFE 배포의 Vm은 지원 되지 않습니다. 현재 클래식/RDFE 리소스에 대 한 지원이 계획 되지 않았습니다. 자세한 내용은 [Azure DDoS Protection 표준 참조 아키텍처](ddos-protection-reference-architectures.md) 를 참조 하세요.

## <a name="can-i-protect-my-paas-resources-using-ddos-protection"></a>DDoS Protection를 사용 하 여 PaaS 리소스를 보호할 수 있나요?
다중 테 넌 트에 연결 된 공용 Ip, 단일 VIP PaaS 서비스는 현재 지원 되지 않습니다. 지원 되지 않는 리소스의 예로는 저장소 Vip, Event Hub Vip 및 App/Cloud Services 응용 프로그램이 있습니다. 자세한 내용은 [Azure DDoS Protection 표준 참조 아키텍처](ddos-protection-reference-architectures.md) 를 참조 하세요.

## <a name="can-i-protect-my-on-premise-resources-using-ddos-protection"></a>DDoS Protection를 사용 하 여 온-프레미스 리소스를 보호할 수 있나요?
DDoS 보호를 사용 하도록 설정 하려면 Azure의 VNet에 연결 된 서비스의 공용 끝점이 있어야 합니다. 예제 디자인은 다음과 같습니다.
- Azure의 웹 사이트 (IaaS) 및 온-프레미스 데이터 센터의 백 엔드 데이터베이스. 
- Azure의 Application Gateway (App Gateway/WAF에서 DDoS protection을 사용 하도록 설정) 및 프레미스 데이터 센터의 websites.

자세한 내용은 [Azure DDoS Protection 표준 참조 아키텍처](ddos-protection-reference-architectures.md) 를 참조 하세요.

## <a name="can-i-register-a-domain-outside-of-azure-and-associate-that-to-a-protected-resource-like-vm-or-elb"></a>Azure 외부에 도메인을 등록 하 고 VM 또는 ELB와 같은 보호 된 리소스에 연결할 수 있나요?
공용 IP 시나리오의 경우 연결 된 공용 IP가 Azure에서 호스트 되는 한, DDoS Protection 서비스는 연결 된 도메인이 등록 되거나 호스트 되는 위치에 관계 없이 모든 응용 프로그램을 지원 합니다. 

## <a name="can-i-manually-configure-the-ddos-policy-applied-to-the-vnetspublic-ips"></a>Vnet/공용 Ip에 적용 된 DDoS 정책을 수동으로 구성할 수 있나요?
아니요, 아쉽게도 지금은 정책 사용자 지정을 사용할 수 없습니다.

## <a name="can-i-allowlistblocklist-specific-ip-addresses"></a>특정 IP 주소를 allowlist/차단 목록 수 있나요?
아니요, 아쉽게도 수동 구성은 현재 사용할 수 없습니다.

## <a name="how-can-i-test-ddos-protection"></a>DDoS Protection를 테스트 하려면 어떻게 해야 하나요?
[시뮬레이션을 통해 테스트](test-through-simulations.md)를 참조 하십시오.

## <a name="how-long-does-it-take-for-the-metrics-to-load-on-portal"></a>포털이 포털에서 로드 하는 데 얼마나 걸립니까?
메트릭은 5 분 이내에 포털에 표시 되어야 합니다. 리소스가 공격을 받고 있는 경우 다른 메트릭은 5-7 분 내에 포털에 표시 되기 시작 합니다. 
    
