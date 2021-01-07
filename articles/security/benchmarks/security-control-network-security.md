---
title: Azure 보안 제어-네트워크 보안
description: Azure 보안 제어 네트워크 보안
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 3a232f8e8c35e265a8243ac79e465c03f6b9650e
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487867"
---
# <a name="security-control-network-security"></a>보안 제어: 네트워크 보안

네트워크 보안 권장 사항은 Azure 서비스에 대 한 액세스가 허용 되거나 거부 되는 네트워크 프로토콜, TCP/UDP 포트 및 네트워크 연결 서비스를 지정 하는 데 중점을 둡니다.

## <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: 가상 네트워크 내에서 Azure 리소스 보호

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.1 | 9.2, 9.4, 14.1, 14.2, 14.3 | Customer |

모든 Virtual Network 서브넷 배포에는 응용 프로그램의 신뢰할 수 있는 포트 및 원본에 특정 한 네트워크 액세스 제어와 함께 적용 되는 네트워크 보안 그룹이 있는지 확인 합니다. 사용할 수 있는 경우 개인 끝점을 개인 링크와 함께 사용 하 여 VNet id를 서비스로 확장 하 여 가상 네트워크에 대 한 Azure 서비스 리소스를 보호 합니다. 개인 끝점 및 개인 링크를 사용할 수 없는 경우 서비스 끝점을 사용 합니다. 서비스별 요구 사항은 해당 특정 서비스에 대 한 보안 권장 사항을 참조 하세요. 

또는 특정 사용 사례가 있는 경우 Azure 방화벽을 구현 하 여 요구 사항을 충족할 수 있습니다.

- [Virtual Network 서비스 끝점 이해](../../virtual-network/virtual-network-service-endpoints-overview.md)

- [Azure 개인 링크 이해](../../private-link/private-link-overview.md)

- [Virtual Network를 만드는 방법](../../virtual-network/quick-create-portal.md)

- [보안 구성을 사용 하 여 NSG를 만드는 방법](../../virtual-network/tutorial-filter-network-traffic.md)

- [Azure 방화벽을 배포 및 구성 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

## <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1.2: 가상 네트워크, 서브넷 및 Nic의 구성과 트래픽을 모니터링 하 고 기록 합니다.

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.2 | 9.3, 12.2, 12.8 | Customer |

Azure Security Center를 사용 하 고 네트워크 보호 권장 사항을 따라 Azure에서 네트워크 리소스를 보호 합니다. NSG 흐름 로그를 사용하도록 설정하고, 트래픽 감사를 위해 로그를 스토리지 계정에 보냅니다. 또한 Log Analytics 작업 영역에 NSG 흐름 로그를 보내고 트래픽 분석를 사용 하 여 Azure 클라우드의 트래픽 흐름에 대 한 통찰력을 제공할 수 있습니다. 트래픽 분석의 장점 중 일부는 네트워크 활동을 시각화하고, 핫 스폿을 식별하며, 보안 위협을 식별하고, 트래픽 흐름 패턴을 이해하며, 잘못된 네트워크 구성을 파악할 수 있다는 것입니다.

- [NSG 흐름 로그를 사용하도록 설정하는 방법](../../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [트래픽 분석을 사용하도록 설정하고 사용하는 방법](../../network-watcher/traffic-analytics.md)

- [Azure Security Center에서 제공 하는 네트워크 보안 이해](../../security-center/security-center-network-recommendations.md)

## <a name="13-protect-critical-web-applications"></a>1.3: 중요한 웹 애플리케이션 보호

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.3 | 9.5 | Customer |

들어오는 트래픽의 추가 검사를 위해 중요 한 웹 응용 프로그램 앞에 Azure WAF (웹 응용 프로그램 방화벽)를 배포 합니다. WAF에 대해 진단 설정을 사용 하도록 설정 하 고 로그를 저장소 계정, 이벤트 허브 또는 Log Analytics 작업 영역에 수집 합니다.

- [Azure WAF를 배포 하는 방법](../../web-application-firewall/ag/create-waf-policy-ag.md)

## <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: 알려진 악성 IP 주소와의 통신 거부

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.4 | 12.3 | Customer |

DDoS 공격 으로부터 보호 하기 위해 Azure 가상 네트워크에서 DDoS 표준 보호를 사용 하도록 설정 합니다. Azure Security Center 통합 위협 인텔리전스를 사용 하 여 알려진 악성 IP 주소와의 통신을 거부 합니다.

위협 인텔리전스를 사용 하도록 설정 하 고 악의적인 네트워크 트래픽에 대해 "경고 및 거부"로 구성 된 각 조직의 네트워크 경계에서 Azure 방화벽을 배포 합니다.

Azure Security Center Just-in-time 네트워크 액세스를 사용 하 여 제한 된 기간 동안 끝점의 노출을 승인 된 IP 주소로 제한 하는 NSGs를 구성 합니다.

적응 네트워크 강화 Azure Security Center 사용 하 여 실제 트래픽 및 위협 인텔리전스에 따라 포트와 원본 Ip를 제한 하는 NSG 구성을 권장 합니다.

- [DDoS 보호를 구성 하는 방법](../../ddos-protection/manage-ddos-protection.md)

- [Azure 방화벽을 배포 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Security Center 통합 위협 인텔리전스 이해](../../security-center/azure-defender.md)

- [적응 네트워크 강화 Azure Security Center 이해](../../security-center/security-center-adaptive-network-hardening.md)

- [Azure Security Center Just-in-time 네트워크 Access Control 이해](../../security-center/security-center-just-in-time.md)

## <a name="15-record-network-packets"></a>1.5: 네트워크 패킷을 기록 합니다.

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.5 | 12.5 | Customer |

Network Watcher 패킷 캡처를 사용 하 여 비정상적인 활동을 조사할 수 있습니다.

- [Network Watcher를 사용하도록 설정하는 방법](../../network-watcher/network-watcher-create.md)

## <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: 네트워크 기반 IDS/IPS(침입 탐지/침입 방지 시스템) 배포

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.6 | 12.6, 12.7 | Customer |

페이로드 검사 기능을 사용 하 여 IDS/IPS 기능을 지 원하는 Azure Marketplace의 제안을 선택 합니다.  페이로드 검사에 기반한 침입 탐지 및/또는 방지 기능이 요구 사항이 아니면, 위협 인텔리전스가 포함된 Azure Firewall을 사용할 수 있습니다. Azure Firewall 위협 인텔리전스 기반 필터링은 알려진 악성 IP 주소 및 도메인과 주고받는 트래픽을 경고하고 거부할 수 있습니다. IP 주소 및 도메인은 Microsoft 위협 인텔리전스 피드에서 제공됩니다.

악의적인 트래픽을 감지 하거나 거부 하기 위해 각 조직의 네트워크 경계에서 원하는 방화벽 솔루션을 배포 합니다.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Azure 방화벽을 배포 하는 방법](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure 방화벽을 사용 하 여 경고를 구성 하는 방법](../../firewall/threat-intel.md)

## <a name="17-manage-traffic-to-web-applications"></a>1.7: 웹 애플리케이션에 대한 트래픽 관리

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.7 | 12.9, 12.10 | Customer |

신뢰할 수 있는 인증서에 대해 HTTPS/TLS를 사용 하도록 설정 된 웹 응용 프로그램용 Azure 애플리케이션 게이트웨이를 배포 합니다.

- [Application Gateway 배포 하는 방법](../../application-gateway/quick-create-portal.md)

- [HTTPS를 사용 하도록 Application Gateway를 구성 하는 방법](../../application-gateway/create-ssl-portal.md)

- [Azure 웹 응용 프로그램 게이트웨이를 사용 하 여 계층 7 부하 분산 이해](../../application-gateway/overview.md)

## <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: 네트워크 보안 규칙의 복잡성 및 관리 오버헤드 최소화

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.8 | 1.5 | Customer |

Virtual Network 서비스 태그를 사용 하 여 네트워크 보안 그룹 또는 Azure 방화벽에서 네트워크 액세스 제어를 정의 합니다. 보안 규칙을 만들 때 특정 IP 주소 대신 서비스 태그를 사용할 수 있습니다. 서비스 태그 이름(예: ApiManagement)을 규칙의 적절한 원본 또는 대상 필드에 지정하면 해당 서비스에 대한 트래픽을 허용하거나 거부할 수 있습니다. Microsoft는 서비스 태그에 포함되는 주소 접두사를 관리하고 주소가 변경되면 서비스 태그를 자동으로 업데이트합니다.

응용 프로그램 보안 그룹을 사용 하 여 복잡 한 보안 구성의 간소화를 지원할 수도 있습니다. 애플리케이션 보안 그룹을 사용하면 네트워크 보안을 애플리케이션 구조의 자연 확장으로 구성하여 가상 머신을 그룹화하고 해당 그룹에 따라 네트워크 보안 정책을 정의할 수 있습니다.

- [서비스 태그 이해 및 사용](../../virtual-network/service-tags-overview.md)

- [응용 프로그램 보안 그룹 이해 및 사용](../../virtual-network/network-security-groups-overview.md#application-security-groups)

## <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: 네트워크 디바이스에 대한 표준 보안 구성 유지 관리

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.9 | 11.1 | Customer |

Azure Policy를 사용 하 여 네트워크 리소스에 대 한 표준 보안 구성을 정의 하 고 구현 합니다.

Azure 청사진을 사용 하 여 azure 리소스 관리자 템플릿, Azure RBAC 컨트롤 및 정책과 같은 주요 환경 아티팩트를 단일 청사진 정의로 패키지화 하 여 대규모 Azure 배포를 간소화할 수도 있습니다. 청사진을 새 구독에 적용 하 고 버전 관리를 통해 제어 및 관리를 세부적으로 조정할 수 있습니다.

- [Azure Policy를 구성하고 관리하는 방법](../../governance/policy/tutorials/create-and-manage.md)

- [네트워킹에 대 한 Azure Policy 샘플](../../governance/policy/samples/built-in-policies.md#network)

- [Azure Blueprint를 만드는 방법](../../governance/blueprints/create-blueprint-portal.md)

## <a name="110-document-traffic-configuration-rules"></a>1.10: 트래픽 구성 규칙 문서화

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.10 | 11.2 | Customer |

NSGs 및 네트워크 보안 및 트래픽 흐름과 관련 된 기타 리소스에 대 한 태그를 사용 합니다. 개별 NSG 규칙의 경우 "설명" 필드를 사용하여 네트워크에서 주고 받는 트래픽을 허용하는 모든 규칙에 대한 비즈니스 요구 및/또는 기간(등)을 지정합니다.

태그를 사용 하 여 모든 리소스를 만들고 태그가 지정 되지 않은 기존 리소스를 알리도록 하려면 태그 지정과 관련 된 기본 제공 Azure Policy 정의 (예: "태그 및 해당 값 필요")를 사용 합니다.

Azure PowerShell 또는 Azure CLI를 사용 하 여 태그를 기준으로 리소스에 대 한 작업을 조회 하거나 수행할 수 있습니다.

- [태그를 만들고 사용하는 방법](../../azure-resource-manager/management/tag-resources.md)

- [Virtual Network를 만드는 방법](../../virtual-network/quick-create-portal.md)

- [보안 구성을 사용하여 NSG를 만드는 방법](../../virtual-network/tutorial-filter-network-traffic.md)

## <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: 자동화된 도구를 사용하여 네트워크 리소스 구성 모니터링 및 변경 내용 검색

| Azure ID | CIS Id | 책임 |
|--|--|--|
| 1.11 | 11.3 | Customer |

Azure 활동 로그를 사용 하 여 리소스 구성을 모니터링 하 고 Azure 리소스에 대 한 변경 내용을 검색 합니다. 중요 한 리소스의 변경 내용이 발생 하는 경우 트리거할 Azure Monitor 내에서 경고를 만듭니다.

- [Azure 활동 로그 이벤트를 확인하고 검색하는 방법](../../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Azure Monitor에서 경고를 만드는 방법](../../azure-monitor/platform/alerts-activity-log.md)

## <a name="next-steps"></a>다음 단계

- 다음 보안 제어: [로깅 및 모니터링](security-control-logging-monitoring.md) 을 참조 하세요.