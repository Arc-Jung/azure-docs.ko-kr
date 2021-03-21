---
title: Cognitive Services에 대 한 Azure 보안 기준
description: Cognitive Services 보안 기준은 Azure 보안 벤치 마크에 지정 된 보안 권장 사항을 구현 하기 위한 절차 지침과 리소스를 제공 합니다.
author: msmbaldwin
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: b243fa18b17fdd15f3c39545b7d81f5796bd8429
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101699864"
---
# <a name="azure-security-baseline-for-cognitive-services"></a>Cognitive Services에 대 한 Azure 보안 기준

이 보안 기준은 [Azure Security 벤치 마크 버전 1.0](../security/benchmarks/overview-v1.md) 의 지침을 Cognitive Services 적용 합니다. Azure Security Benchmark는 Azure에서 클라우드 솔루션을 보호하는 방법에 대한 권장 사항을 제공합니다.
콘텐츠는 Azure 보안 벤치 마크에 정의 된 **보안 컨트롤** 및 Cognitive Services에 적용 되는 관련 지침에 따라 그룹화 됩니다. Cognitive Services에 적용할 수 없는 **컨트롤** 은 제외 되었습니다.

 
Cognitive Services 완전히 Azure 보안 벤치 마크에 매핑되는 방법을 보려면 [전체 Cognitive Services 보안 기준 매핑 파일](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)을 참조 하세요.

## <a name="network-security"></a>네트워크 보안

자세한 내용은 [Azure Security Benchmark: 네트워크 보안](../security/benchmarks/security-control-network-security.md)을 참조하세요.

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1: 가상 네트워크 내에서 Azure 리소스 보호

**지침**: Microsoft Azure Cognitive Services는 계층화 된 보안 모델을 제공 합니다. 이 모델을 사용하여 Cognitive Services 계정을 특정 네트워크 하위 집합으로 보호할 수 있습니다. 네트워크 규칙이 구성되면 지정된 네트워크 세트를 통해 데이터를 요청하는 애플리케이션만 계정에 액세스할 수 있습니다. 요청 필터링을 사용 하 여 리소스에 대 한 액세스를 제한 하 여 지정 된 IP 주소, IP 범위 또는 Azure Virtual Network의 서브넷 목록에서 시작 되는 요청만 허용 합니다.

Cognitive Services에 대 한 가상 네트워크 및 서비스 끝점 지원은 특정 지역 집합으로 제한 됩니다.

- [Azure Cognitive Services 가상 네트워크를 구성 하는 방법](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks?tabs=portal)

- [Azure Virtual Network 개요](../virtual-network/virtual-networks-overview.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2: 가상 네트워크, 서브넷 및 네트워크 인터페이스의 구성 및 트래픽을 모니터링 하 고 기록 합니다.

**지침**: Virtual Machines Cognitive Services 컨테이너와 동일한 가상 네트워크에 배포 된 경우 네트워크 보안 그룹을 사용 하 여 데이터 exfiltration의 위험을 줄일 수 있습니다. 네트워크 보안 그룹의 흐름 로그를 사용 하도록 설정 하 고 트래픽 감사를 위해 Azure Storage 계정에 로그를 보냅니다. 또한 네트워크 보안 그룹 흐름 로그를 Log Analytics 작업 영역으로 보내고 트래픽 분석를 사용 하 여 Azure 클라우드의 트래픽 흐름에 대 한 정보를 제공할 수 있습니다. 트래픽 분석의 장점 중 일부는 네트워크 활동을 시각화하고, 핫 스폿을 식별하며, 보안 위협을 식별하고, 트래픽 흐름 패턴을 이해하며, 잘못된 네트워크 구성을 파악할 수 있다는 것입니다.

- [NSG 흐름 로그를 사용하도록 설정하는 방법](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [트래픽 분석을 사용하도록 설정하고 사용하는 방법](../network-watcher/traffic-analytics.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="13-protect-critical-web-applications"></a>1.3: 중요한 웹 애플리케이션 보호

**지침**: 컨테이너 내에서 Cognitive Services를 사용 하는 경우, 악성 트래픽을 필터링 하 고 종단 간 TLS 암호화를 지 원하는 프런트 엔드 웹 응용 프로그램 방화벽 솔루션으로 컨테이너 배포를 보강 하 여 컨테이너 엔드포인트를 개인 및 안전 하 게 유지할 수 있습니다. 

청구 목적으로 계량 정보를 제출 하려면 Cognitive Services 컨테이너가 필요 합니다. 유일한 예외는 다른 청구 방법을 따르는 오프 라인 컨테이너입니다. 허용 하지 않으면 Cognitive Services 컨테이너에서 사용 하는 다양 한 네트워크 채널을 나열할 때 컨테이너가 작동 하지 않습니다. 호스트는 443 포트와 다음 도메인을 허용 해야 합니다.

- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

또한 Cognitive Services 컨테이너가 Microsoft 서버에 만드는 보안 채널에서 방화벽 솔루션에 대 한 심층 패킷 검사를 사용 하지 않도록 설정 해야 합니다. 이렇게 하지 않으면 컨테이너가 제대로 작동 하지 않습니다.

- [Azure Cognitive Services 컨테이너 보안 이해](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: 알려진 악성 IP 주소와의 통신 거부

**지침**: 가상 컴퓨터가 Cognitive Services 컨테이너와 동일한 가상 네트워크에 배포 되는 경우 Azure Policy를 사용 하 여 관련 네트워크 리소스에 대 한 표준 보안 구성을 정의 하 고 구현 합니다. "Cognitiveservices account" 및 "Redis" 네임 스페이스의 Azure Policy 별칭을 사용 하 여 사용자 지정 정책을 만들어 인스턴스에 대 한 Azure Cache의 네트워크 구성을 감사 하거나 적용 합니다. 다음과 같은 기본 제공 정책 정의를 사용할 수도 있습니다.

- DDoS Protection 표준을 사용하도록 설정해야 합니다.

Azure 청사진을 사용 하 여 단일 청사진 정의에서 Azure Resource Manager 템플릿, azure RBAC (역할 기반 액세스 제어) 및 정책과 같은 주요 환경 아티팩트를 패키지 하 여 대규모 Azure 배포를 간소화 합니다. Blueprint를 새로운 구독 및 환경에 쉽게 적용하고 버전 관리를 통해 제어 및 관리를 세부적으로 조정합니다.

컨테이너 내에서 Cognitive Services를 사용 하는 경우, 악성 트래픽을 필터링 하 고 종단 간 TLS 암호화를 지 원하는 프런트 엔드 웹 응용 프로그램 방화벽 솔루션을 사용 하 여 컨테이너 배포를 보강 하 여 컨테이너 끝점을 비공개로 안전 하 게 유지할 수 있습니다. 

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprint를 만드는 방법](../governance/blueprints/create-blueprint-portal.md)

- [Azure Cognitive Services 컨테이너 보안 이해](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="15-record-network-packets"></a>1.5: 네트워크 패킷을 기록 합니다.

**지침**: 가상 컴퓨터가 Cognitive Services 컨테이너와 동일한 가상 네트워크에 배포 된 경우 네트워크 보안 그룹을 사용 하 여 데이터 exfiltration의 위험을 줄일 수 있습니다. 네트워크 보안 그룹의 흐름 로그를 사용 하도록 설정 하 고 트래픽 감사를 위해 Azure Storage 계정에 로그를 보냅니다. 또한 네트워크 보안 그룹 흐름 로그를 Log Analytics 작업 영역으로 보내고 트래픽 분석를 사용 하 여 Azure 클라우드의 트래픽 흐름에 대 한 정보를 제공할 수 있습니다. 트래픽 분석의 장점 중 일부는 네트워크 활동을 시각화하고, 핫 스폿을 식별하며, 보안 위협을 식별하고, 트래픽 흐름 패턴을 이해하며, 잘못된 네트워크 구성을 파악할 수 있다는 것입니다.

- [NSG 흐름 로그를 사용하도록 설정하는 방법](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [트래픽 분석을 사용하도록 설정하고 사용하는 방법](../network-watcher/traffic-analytics.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: 네트워크 기반 침입 감지/침입 방지 시스템 (IDS/IPS) 배포

**지침**: 컨테이너 내에서 Cognitive Services를 사용 하는 경우 악성 트래픽을 필터링 하 고 종단 간 TLS 암호화를 지 원하는 프런트 엔드 웹 응용 프로그램 방화벽 솔루션을 사용 하 여 컨테이너 배포를 보강 하 여 컨테이너 엔드포인트를 개인 및 안전 하 게 유지할 수 있습니다.  페이로드 검사를 사용 하지 않도록 설정 하는 기능을 사용 하 여 IDS/IPS 기능을 지 원하는 Azure Marketplace에서 제품을 선택할 수 있습니다.

청구 목적으로 계량 정보를 제출 하려면 Cognitive Services 컨테이너가 필요 합니다. 유일한 예외는 다른 청구 방법을 따르기 때문에 오프 라인 컨테이너입니다. 허용 하지 않으면 Cognitive Services 컨테이너에서 사용 하는 다양 한 네트워크 채널을 나열할 때 컨테이너가 작동 하지 않습니다. 호스트는 443 포트와 다음 도메인을 허용 해야 합니다.

- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

또한 Cognitive Services 컨테이너가 Microsoft 서버에 만드는 보안 채널에서 방화벽 솔루션에 대 한 심층 패킷 검사를 사용 하지 않도록 설정 해야 합니다. 이렇게 하지 않으면 컨테이너가 제대로 작동 하지 않습니다.

- [Azure Cognitive Services 컨테이너 보안 이해](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="17-manage-traffic-to-web-applications"></a>1.7: 웹 애플리케이션에 대한 트래픽 관리

**지침**: 컨테이너 내에서 Cognitive Services를 사용 하는 경우 악성 트래픽을 필터링 하 고 종단 간 TLS 암호화를 지 원하는 프런트 엔드 웹 응용 프로그램 방화벽 솔루션을 사용 하 여 컨테이너 배포를 보강 하 여 컨테이너 엔드포인트를 개인 및 안전 하 게 유지할 수 있습니다. 

청구 목적으로 계량 정보를 제출 하려면 Cognitive Services 컨테이너가 필요 합니다. 유일한 예외는 다른 청구 방법을 따르는 오프 라인 컨테이너입니다. 허용 하지 않으면 Cognitive Services 컨테이너에서 사용 하는 다양 한 네트워크 채널을 나열할 때 컨테이너가 작동 하지 않습니다. 호스트는 443 포트와 다음 도메인을 허용 해야 합니다.

- *. cognitive.microsoft.com
- *. cognitiveservices.azure.com

또한 Cognitive Services 컨테이너가 Microsoft 서버에 만드는 보안 채널에서 방화벽 솔루션에 대 한 심층 패킷 검사를 사용 하지 않도록 설정 해야 합니다. 이렇게 하지 않으면 컨테이너가 제대로 작동 하지 않습니다.

- [Azure Cognitive Services 컨테이너 보안 이해](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support#azure-cognitive-services-container-security)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: 네트워크 보안 규칙의 복잡성 및 관리 오버헤드 최소화

**지침**: 가상 네트워크 서비스 태그를 사용 하 여 네트워크 보안 그룹 또는 Azure 방화벽에서 네트워크 액세스 제어를 정의 합니다. 보안 규칙을 만들 때 특정 IP 주소 대신 서비스 태그를 사용할 수 있습니다. 규칙의 적절 한 원본 또는 대상 필드에서 서비스 태그 이름 (예: Microsoft.apimanagement)을 지정 하 여 해당 서비스에 대 한 트래픽을 허용 하거나 거부할 수 있습니다. Microsoft는 서비스 태그에 포함되는 주소 접두사를 관리하고 주소가 변경되면 서비스 태그를 자동으로 업데이트합니다.

응용 프로그램 보안 그룹을 사용 하 여 복잡 한 보안 구성의 간소화를 지원할 수도 있습니다. 애플리케이션 보안 그룹을 사용하면 네트워크 보안을 애플리케이션 구조의 자연 확장으로 구성하여 가상 머신을 그룹화하고 해당 그룹에 따라 네트워크 보안 정책을 정의할 수 있습니다.

- [가상 네트워크 서비스 태그](../virtual-network/service-tags-overview.md)

- [응용 프로그램 보안 그룹](https://docs.microsoft.com/azure/virtual-network/network-security-groups-overview#application-security-groups)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: 네트워크 디바이스에 대한 표준 보안 구성 유지 관리

**지침**: Azure Policy를 사용 하 여 Cognitive Services 컨테이너와 관련 된 네트워크 리소스에 대 한 표준 보안 구성을 정의 하 고 구현 합니다. "Cognitiveservices account" 및 "Redis" 네임 스페이스의 Azure Policy 별칭을 사용 하 여 사용자 지정 정책을 만들어 인스턴스에 대 한 Azure Cache의 네트워크 구성을 감사 하거나 적용 합니다.

또한 Azure 청사진을 사용 하 여 단일 청사진 정의에서 Azure Resource Manager 템플릿, Azure RBAC (역할 기반 액세스 제어) 및 정책과 같은 주요 환경 아티팩트를 패키지화 하 여 대규모 Azure 배포를 간소화할 수 있습니다. Blueprint를 새로운 구독 및 환경에 쉽게 적용하고 버전 관리를 통해 제어 및 관리를 세부적으로 조정합니다.

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

- [Azure Blueprint를 만드는 방법](../governance/blueprints/create-blueprint-portal.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="110-document-traffic-configuration-rules"></a>1.10: 트래픽 구성 규칙 문서화

**지침**: 논리적으로 분류로 구성 하기 위해 Cognitive Services 컨테이너와 연결 된 네트워크 리소스에 대 한 태그를 사용 합니다.

- [태그를 만들고 사용하는 방법](../azure-resource-manager/management/tag-resources.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: 자동화된 도구를 사용하여 네트워크 리소스 구성 모니터링 및 변경 내용 검색

**지침**: Azure 활동 로그를 사용 하 여 네트워크 리소스 구성을 모니터링 하 고 Cognitive Services 컨테이너와 관련 된 네트워크 리소스에 대 한 변경 내용을 검색 합니다. Azure Monitor 내에서 중요한 네트워크 리소스가 변경되면 트리거되는 경고를 만듭니다.

- [Azure 활동 로그 이벤트를 확인하고 검색하는 방법](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Azure Monitor에서 경고를 만드는 방법](/azure/azure-monitor/platform/alerts-activity-log)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="logging-and-monitoring"></a>로깅 및 모니터링

*자세한 내용은 [Azure 보안 벤치 마크: 로깅 및 모니터링](../security/benchmarks/security-control-logging-monitoring.md)을 참조 하세요.*

### <a name="22-configure-central-security-log-management"></a>2.2: 중앙 보안 로그 관리 구성

**지침**: Azure 활동 로그 진단 설정을 사용하도록 설정하고 보관을 위해 로그를 Log Analytics 작업 영역, Azure 이벤트 허브 또는 Azure 스토리지 계정으로 보냅니다. 활동 로그는 제어 평면 수준에서 Cognitive Services 컨테이너에 대해 수행 된 작업에 대 한 통찰력을 제공 합니다. Azure 활동 로그 데이터를 사용 하 여 Redis 인스턴스에 대 한 Azure 캐시에 대 한 제어 평면 수준에서 수행 되는 모든 쓰기 작업 (PUT, POST, DELETE)에 대해 "무엇을, 누가, 언제"를 결정할 수 있습니다.

- [진단 설정을 Azure 활동 로그에 사용하도록 설정하는 방법](/azure/azure-monitor/platform/activity-log)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure 리소스에 대한 감사 로깅 사용

**지침**: 제어 평면 감사 로깅의 경우 Azure 활동 로그 진단 설정을 사용 하도록 설정 하 고 Log Analytics 작업 영역, azure 이벤트 허브 또는 보관을 위해 azure storage 계정으로 로그를 보냅니다. Azure 활동 로그 데이터를 사용하면 Azure 리소스의 컨트롤 플레인 수준에서 수행되는 모든 쓰기 작업(PUT, POST, DELETE)에 대한 "무엇을, 누가, 언제"를 판단할 수 있습니다.

또한 Cognitive Services는 분석, 경고 및 보고를 위해 수집 하 고 사용할 수 있는 진단 이벤트를 보냅니다. Azure Portal를 통해 Cognitive Services 컨테이너에 대 한 진단 설정을 구성할 수 있습니다. 하나 이상의 진단 이벤트를 저장소 계정, 이벤트 허브 또는 Log Analytics 작업 영역으로 보낼 수 있습니다.

- [진단 설정을 Azure 활동 로그에 사용하도록 설정하는 방법](/azure/azure-monitor/platform/diagnostic-settings-legacy)

- [Azure Cognitive Services에 대 한 진단 설정 사용](diagnostic-logging.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="25-configure-security-log-storage-retention"></a>2.5: 보안 로그 스토리지 보존 기간 구성

**지침**: Azure Monitor 내에서 조직의 규정 준수 규칙에 따라 Log Analytics 작업 영역 보존 기간을 설정합니다. Azure Storage 계정을 장기/보관 스토리지에 사용합니다.

- [Log Analytics 작업 영역에 대한 로그 보존 기간 매개 변수를 설정하는 방법](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="26-monitor-and-review-logs"></a>2.6: 로그를 모니터링 하 고 검토 합니다.

**지침**: Azure 활동 로그 진단 설정을 사용 하도록 설정 하 고 로그를 Log Analytics 작업 영역으로 보냅니다. 이러한 로그는 문제를 식별 하 고 디버깅 하는 데 사용 되는 리소스 작업에 대 한 풍부 하 고 빈번한 데이터를 제공 합니다. Log Analytics에서 쿼리를 수행 하 여 용어를 검색 하 고, 추세를 식별 하 고, 패턴을 분석 하 고, Azure Cognitive Services에 대해 수집 되었을 수 있는 활동 로그 데이터를 기반으로 다양 한 통찰력을 제공 합니다.

- [진단 설정을 Azure 활동 로그에 사용하도록 설정하는 방법](/azure/azure-monitor/platform/activity-log)

- [Azure Monitor의 Log Analytics 작업 영역에서 Azure 활동 로그를 수집 하 고 분석 하는 방법](/azure/azure-monitor/platform/activity-log)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: 비정상적인 활동에 대해 경고를 사용 하도록 설정

**지침**: Azure Monitor의 경고 및 메트릭 섹션으로 이동 하 여 Cognitive Services 지원 되는 메트릭에 대 한 경고를 발생 시킬 수 있습니다.

Cognitive Services 컨테이너에 대 한 진단 설정을 구성 하 고 Log Analytics 작업 영역으로 로그를 보냅니다. Log Analytics 작업 영역 내에서 미리 정의한 일련의 조건이 발생하면 경보가 발생하도록 구성합니다. 또는 데이터를 사용하도록 설정하여 Azure Sentinel 또는 타사 SIEM에 온보딩할 수 있습니다.

- [Azure Sentinel을 온보딩하는 방법](../sentinel/quickstart-onboard.md)

- [Azure Monitor를 사용하여 로그 경고 만들기, 보기 및 관리](/azure/azure-monitor/platform/alerts-log)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="identity-and-access-control"></a>ID 및 Access Control

*자세한 내용은 [Azure 보안 벤치 마크: id 및 Access Control](../security/benchmarks/security-control-identity-access-control.md)을 참조 하세요.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: 관리 계정의 인벤토리 유지 관리

**지침**: Azure Active Directory (Azure AD)에는 명시적으로 할당 되어야 하며 쿼리할 수 있는 기본 제공 역할이 있습니다. Azure AD PowerShell 모듈을 사용 하 여 임시 쿼리를 수행 하 여 관리 그룹의 구성원 인 계정을 검색 합니다.

- [PowerShell을 사용 하 여 Azure AD에서 디렉터리 역할을 가져오는 방법](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [PowerShell을 사용 하 여 Azure AD에서 디렉터리 역할의 멤버를 가져오는 방법](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="32-change-default-passwords-where-applicable"></a>3.2: 기본 암호 변경(해당하는 경우)

**지침**: Cognitive Services에 대 한 제어 평면 액세스는 Azure Active Directory (Azure AD)를 통해 제어 됩니다. Azure AD에는 기본 암호 개념이 없습니다.

Cognitive Services에 대 한 데이터 평면 액세스는 액세스 키를 통해 제어 됩니다. 이러한 키는 캐시에 연결 하는 클라이언트에서 사용 되며 언제 든 지 다시 생성할 수 있습니다.

응용 프로그램에 기본 암호를 빌드하는 것은 권장 되지 않습니다. 대신 Azure Key Vault에 암호를 저장 하 고 Azure AD를 사용 하 여 검색할 수 있습니다.

- [Redis 액세스 키에 대 한 Azure 캐시를 다시 생성 하는 방법](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-configure#settings)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: 전용 관리 계정 사용

**지침**: 전용 관리 계정 사용에 대한 표준 운영 절차를 만듭니다. Security Center Id 및 액세스 관리를 사용 하 여 관리 계정 수를 모니터링 합니다.

또한 전용 관리 계정을 추적 하는 데 도움이 되도록 다음과 같은 Security Center 또는 기본 제공 Azure 정책의 권장 사항을 사용할 수 있습니다.

- 구독에 둘 이상의 소유자를 할당해야 합니다.
- 소유자 권한이 있는 사용되지 않는 계정은 구독에서 제거해야 합니다.
- 소유자 권한이 있는 외부 계정은 구독에서 제거해야 합니다.

- [Azure Security Center를 사용하여 ID 및 액세스를 모니터링하는 방법(미리 보기)](../security-center/security-center-identity-access.md)

- [Azure Policy를 사용하는 방법](../governance/policy/tutorials/create-and-manage.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4: SSO (Azure Active Directory Single Sign-On)를 사용 하십시오.

**지침**: Cognitive Services 액세스 키를 사용 하 여 사용자를 인증 하 고 데이터 평면 수준에서 SINGLE SIGN-ON (SSO)를 지원 하지 않습니다. Cognitive Services에 대 한 제어 평면에 대 한 액세스는 REST API를 통해 제공 되며 SSO를 지원 합니다. 인증 하려면 Azure Active Directory (Azure AD)에서 가져오는 JSON (JavaScript Object Notation) 웹 토큰에 대 한 요청에 대 한 권한 부여 헤더를 설정 합니다.

- [Azure Cognitive Services REST API 이해](/rest/api/cognitiveservices/)

- [Azure AD를 사용 하 여 SSO 이해](../active-directory/manage-apps/what-is-single-sign-on.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: 모든 Azure Active Directory 기반 액세스에 대해 multi-factor authentication을 사용 하십시오.

**지침**: Azure Active Directory (Azure AD) 다단계 인증을 사용 하도록 설정 하 고 Security Center Id 및 액세스 관리 권장 사항을 따릅니다.

- [Azure에서 다단계 인증을 사용 하도록 설정 하는 방법](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Security Center 내에서 ID 및 액세스를 모니터링하는 방법](../security-center/security-center-identity-access.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6: 관리 작업에는 안전한 Azure 관리 워크스테이션 사용

**지침**: 다단계 인증을 사용 하는 PAW (권한 있는 액세스 워크스테이션)를 사용 하 여 Azure 리소스에 로그인 하 고 구성 합니다. 

- [Privileged Access Workstation에 대한 자세한 정보](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Azure에서 다단계 인증을 사용 하도록 설정 하는 방법](../active-directory/authentication/howto-mfa-getstarted.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7: 관리 계정에서 의심 스러운 활동에 대 한 로그 및 경고

**지침**: 환경에서 의심 스러운 작업이 나 안전 하지 않은 활동이 발생 하는 경우 로그 및 경고를 생성 하는 데 Azure Active Directory (Azure AD) PRIVILEGED IDENTITY MANAGEMENT (PIM)를 사용 합니다.

또한 Azure AD 위험 탐지를 사용하여 위험한 사용자 동작에 대한 경고 및 보고서를 봅니다.

- [PIM(Privileged Identity Management)을 배포하는 방법](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure AD 위험 탐지 이해](../active-directory/identity-protection/overview-identity-protection.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: 승인된 위치에서만 Azure 리소스 관리

**지침**: IP 주소 범위 또는 국가 및 지역의 특정 논리적 그룹 에서만 액세스할 수 있도록 Azure Active Directory (Azure AD) 조건부 액세스의 명명 된 위치를 구성 합니다.

- [Azure에서 명명된 위치를 구성하는 방법](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="39-use-azure-active-directory"></a>3.9: Azure Active Directory 사용

**지침**: Azure Active Directory (Azure AD)를 중앙 인증 및 권한 부여 시스템으로 사용 합니다. Azure AD는 미사용 데이터 및 전송 중인 데이터에 대해 강력한 암호화를 사용 하 여 데이터를 보호 하 고 사용자 자격 증명을 salts, 해시 및 안전 하 게 저장 합니다. 사용 사례가 AD 인증을 지 원하는 경우 Azure AD를 사용 하 여 Cognitive Services API에 대 한 요청을 인증 합니다. 

현재는 Azure AD를 사용 하 여 인증을 지 원하는 Computer Vision API, Face API, 텍스트 분석 API, 몰입 형 판독기, 폼 인식기, 변칙 탐지기 및 모든 Bing Bing Custom Search 서비스만 지원 합니다.

- [Cognitive Services에 대 한 요청을 인증 하는 방법](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-azure-active-directory)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: 정기적으로 사용자 액세스 검토 및 조정

**지침**: Azure Active Directory (Azure AD)는 오래 된 계정을 검색 하는 데 도움이 되는 로그를 제공 합니다. 또한 고객은 Azure Id 액세스 검토를 활용 하 여 그룹 멤버 자격, 엔터프라이즈 응용 프로그램에 대 한 액세스 및 역할 할당을 효율적으로 관리 합니다. 사용자의 액세스를 정기적으로 검토 하 여 활성 사용자만 계속 액세스할 수 있도록 할 수 있습니다.

고객은 API Management 사용자 계정에 대 한 인벤토리를 유지 관리 하 고 필요에 따라 액세스를 조정 합니다. API Management에서 개발자는 API Management 사용을 공개하는 API의 사용자입니다. 기본적으로 새로 만든 개발자 계정은 활성 상태이며, 개발자 그룹과 연결됩니다. 활성 상태의 개발자 계정은 구독을 사용하는 모든 API에 액세스하는 데 사용할 수 있습니다.

- [Azure API Management에서 사용자 계정을 관리하는 방법](../api-management/api-management-howto-create-or-invite-developers.md)

- [API Management 사용자 목록을 가져오는 방법](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-4.8.0&amp;preserve-view=true)

- [Azure ID 액세스 검토를 사용하는 방법](../active-directory/governance/access-reviews-overview.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11: 비활성화 되는 자격 증명에 대 한 액세스 시도를 모니터링 합니다.

**지침**: azure 센티널 또는 타사 siem과 통합할 수 있는 azure AD (Azure Active Directory) 로그인 활동, 감사 및 위험 이벤트 로그 원본에 액세스할 수 있습니다.

Azure AD 사용자 계정에 대 한 진단 설정을 만들고 감사 로그 및 로그인 로그를 Log Analytics 작업 영역으로 전송 하 여이 프로세스를 간소화할 수 있습니다. Log Analytics 내에서 원하는 로그 경고를 구성할 수 있습니다.

- [Azure 활동 로그를 Azure Monitor에 통합하는 방법](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Azure Sentinel을 온보딩하는 방법](../sentinel/quickstart-onboard.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12: 계정 로그인 동작 편차에 대 한 경고

**지침**: 제어 평면의 계정 로그인 동작 편차에 대해 Azure Active Directory (Azure AD) id 보호 및 위험 검색 기능을 사용 하 여 사용자 id와 관련 된 검색 된 의심 스러운 작업에 대 한 자동화 된 응답을 구성 합니다. 추가 조사를 위해 데이터를 Azure Sentinel로 수집할 수도 있습니다.

- [Azure AD 위험한 로그인을 확인하는 방법](../active-directory/identity-protection/overview-identity-protection.md)

- [ID 보호 위험 정책을 구성하고 사용하도록 설정하는 방법](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel을 온보딩하는 방법](../sentinel/quickstart-onboard.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: 지원 시나리오에서 관련 고객 데이터에 대한 액세스 권한을 Microsoft에 제공

**지침**: Cognitive Services에는 사용할 수 없습니다. 고객 Lockbox는 Cognitive Services에 대해 아직 지원 되지 않습니다.

- [Customer Lockbox 지원 서비스 목록](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="data-protection"></a>데이터 보호

자세한 내용은 [Azure Security Benchmark: 데이터 보호](../security/benchmarks/security-control-data-protection.md)를 참조하세요.

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: 중요한 정보의 인벤토리 유지 관리

**지침**: 태그를 사용하여 중요한 정보를 저장하거나 처리하는 Azure 리소스를 추적할 수 있도록 지원합니다.

- [태그를 만들고 사용하는 방법](../azure-resource-manager/management/tag-resources.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: 중요한 정보를 저장하거나 처리하는 시스템 격리

**지침**: 개발, 테스트 및 프로덕션을 위한 별도의 구독 또는 관리 그룹을 구현 합니다. 리소스는 가상 네트워크 또는 서브넷으로 구분 되며, 적절 하 게 태그가 지정 되 고, 네트워크 보안 그룹 또는 Azure 방화벽으로 보호 됩니다. 중요 한 데이터를 저장 하거나 처리 하는 리소스는 충분히 격리 되어야 합니다. 중요 한 데이터를 저장 하거나 처리 하는 Virtual Machines 사용 하지 않을 때이를 해제 하는 정책과 프로시저를 구현 합니다.

- [추가 Azure 구독을 만드는 방법](../cost-management-billing/manage/create-subscription.md)

- [관리 그룹을 만드는 방법](../governance/management-groups/create-management-group-portal.md)

- [태그를 만들고 사용하는 방법](../azure-resource-manager/management/tag-resources.md)

- [Virtual Network를 만드는 방법](../virtual-network/quick-create-portal.md)

- [보안 구성을 사용하여 NSG를 만드는 방법](../virtual-network/tutorial-filter-network-traffic.md)

- [Azure 방화벽을 배포 하는 방법](../firewall/tutorial-firewall-deploy-portal.md)

- [Azure 방화벽을 사용 하 여 경고 또는 경고 및 거부를 구성 하는 방법](../firewall/threat-intel.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: 중요한 정보에 대한 무단 전송 모니터링 및 차단

**지침**: 아직 사용할 수 없습니다. 데이터 식별, 분류 및 손실 방지 기능은 아직 Cognitive Services 사용할 수 없습니다.

Microsoft는 Cognitive Services에 대 한 기본 인프라를 관리 하 고, 고객 데이터의 손실 또는 노출을 방지 하기 위해 엄격한 컨트롤을 구현 했습니다.

- [Azure의 고객 데이터 보호 이해](../security/fundamentals/protection-customer-data.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: 전송 중인 모든 중요한 정보 암호화

**지침**: 모든 Cognitive Services 끝점은 HTTP 적용 TLS 1.2을 통해 노출 됩니다. 적용 되는 보안 프로토콜을 사용 하 여 Cognitive Services 끝점을 호출 하려는 소비자는 다음 지침을 준수 해야 합니다.

- 클라이언트 운영 체제 (OS)는 TLS 1.2를 지원 해야 합니다.
- HTTP 호출을 수행 하는 데 사용 되는 언어 (및 플랫폼)는 요청의 일부로 TLS 1.2을 지정 해야 합니다. 언어 및 플랫폼에 따라 TLS를 암시적으로 또는 명시적으로 지정 합니다.

- [Azure Cognitive Services에 대 한 전송 계층 보안 이해](cognitive-services-security.md)

**책임**: 공유됨

**Azure Security Center 모니터링**: 없음

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: 활성 검색 도구를 사용하여 중요한 데이터 식별

**지침**: 데이터 식별, 분류 및 손실 방지 기능은 아직 Cognitive Services 사용할 수 없습니다. 중요 한 정보를 포함 하는 인스턴스에 태그를 적용 하 고 규정 준수를 위해 필요한 경우 타사 솔루션을 구현 합니다.

Microsoft는 기본 플랫폼을 관리 하 고 모든 고객 콘텐츠를 중요 한 것으로 간주 하 고 고객 데이터 손실 및 노출을 방지 하기 위해 좋은 길이로 이동 합니다. Azure 내에서 고객 데이터를 안전하게 유지하기 위해 Microsoft는 강력한 데이터 보호 제어 및 기능 모음을 구현하고 유지 관리합니다.

- [Azure의 고객 데이터 보호 이해](../security/fundamentals/protection-customer-data.md)

**책임**: 공유됨

**Azure Security Center 모니터링**: 없음

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Azure RBAC를 사용하여 리소스에 대한 액세스 제어 

**지침**: azure RBAC (역할 기반 액세스 제어)를 사용 하 여 Cognitive Services 제어 평면 (Azure Portal)에 대 한 액세스를 제어 합니다. 

- [Azure RBAC를 구성 하는 방법](../role-based-access-control/role-assignments-portal.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: 중요한 저장 정보 암호화

**지침**: Cognitive Services에 대 한 휴지 상태의 암호화는 사용 되는 특정 서비스에 따라 달라 집니다. 대부분의 경우 데이터는 FIPS 140-2 규격 256 비트 AES 암호화를 사용 하 여 암호화 되 고 암호가 해독 됩니다. 암호화 및 암호 해독은 투명 합니다. 즉, Microsoft의 고객을 위해 암호화 및 액세스를 관리 합니다. 고객 데이터는 기본적으로 안전 하며 암호화를 활용 하기 위해 코드 또는 응용 프로그램을 수정할 필요가 없습니다.

Azure Key Vault를 사용 하 여 고객 관리 키를 저장할 수도 있습니다. 사용자 고유의 키를 만들어 키 자격 증명 모음에 저장할 수도 있고, Azure Key Vault API를 사용하여 키를 생성할 수도 있습니다.

- [휴지 상태의 정보를 암호화 하는 서비스 목록](/azure/cognitive-services/encryption/cognitive-services-encryption-keys-portal)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: 중요한 Azure 리소스에 대한 변경 내용 로그 및 경고

**지침**: Azure 활동 로그와 함께 Azure Monitor를 사용 하 여 프로덕션 인스턴스 Cognitive Services 및 기타 중요 한 리소스 또는 관련 된 리소스에 대 한 변경 내용이 발생 하는 경우에 대 한 경고를 만듭니다.

- [Azure 활동 로그 이벤트에 대한 경고를 만드는 방법](/azure/azure-monitor/platform/alerts-activity-log)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="inventory-and-asset-management"></a>인벤토리 및 자산 관리

*자세한 내용은 [Azure 보안 벤치 마크: 인벤토리 및 자산 관리](../security/benchmarks/security-control-inventory-asset-management.md)를 참조 하세요.*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1: 자동화 된 asset discovery 솔루션 사용

**지침**: Azure 리소스 그래프를 사용 하 여 구독 내에서 계산, 저장소, 네트워크, 포트, 프로토콜 등의 모든 리소스 (예: 계산, 저장소, 네트워크, 포트 등)를 쿼리하거나 검색 합니다. 테넌트에서 적절한 권한(읽기)을 확인하고, 모든 Azure 구독 및 구독 내의 리소스를 열거합니다.

클래식 Azure 리소스는 Resource Graph를 통해 검색할 수 있지만 앞으로 Azure Resource Manager 리소스를 만들어 사용하는 것이 좋습니다.

- [Azure Resource Graph를 사용하여 쿼리를 만드는 방법](../governance/resource-graph/first-query-portal.md)

- [Azure 구독을 확인하는 방법](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Azure RBAC 이해](../role-based-access-control/overview.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="62-maintain-asset-metadata"></a>6.2: 자산 메타데이터 유지 관리

**지침**: 메타데이터를 제공하는 Azure 리소스에 태그를 적용하여 논리적인 분류로 구성합니다.

- [태그를 만들고 사용하는 방법](../azure-resource-manager/management/tag-resources.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: 권한 없는 Azure 리소스 삭제

**지침**: 적절 한 태그 지정, 관리 그룹 및 별도의 구독을 사용 하 여 Redis 인스턴스 및 관련 리소스에 대 한 Azure 캐시를 구성 하 고 추적 합니다. 정기적으로 인벤토리를 조정하고, 구독에서 권한 없는 리소스가 적시에 삭제되도록 합니다.

또한 Azure Policy를 사용 하 여 다음 기본 제공 정책 정의를 사용 하 여 고객 구독에서 만들 수 있는 리소스 유형에 대 한 제한을 설정할 수 있습니다.

- 허용되지 않는 리소스 종류
- 허용되는 리소스 유형

- [추가 Azure 구독을 만드는 방법](../cost-management-billing/manage/create-subscription.md)

- [관리 그룹을 만드는 방법](../governance/management-groups/create-management-group-portal.md)

- [태그를 만들고 사용하는 방법](../azure-resource-manager/management/tag-resources.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: 승인되지 않은 Azure 리소스 모니터링

**지침**: Azure Policy을 사용 하 여 다음 기본 제공 정책 정의를 사용 하 여 고객 구독에서 만들 수 있는 리소스 유형에 대 한 제한을 설정할 수 있습니다.

- 허용되지 않는 리소스 종류
- 허용되는 리소스 유형

또한 Azure 리소스 그래프를 사용 하 여 구독 내에서 리소스를 쿼리하거나 검색 합니다.

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

- [Azure Resource Graph를 사용하여 쿼리를 만드는 방법](../governance/resource-graph/first-query-portal.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="69-use-only-approved-azure-services"></a>6.9: 승인된 Azure 서비스만 사용

**지침**: Azure Policy을 사용 하 여 다음 기본 제공 정책 정의를 사용 하 여 고객 구독에서 만들 수 있는 리소스 유형에 대 한 제한을 설정할 수 있습니다.
- 허용되지 않는 리소스 종류
- 허용되는 리소스 유형

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

- [Azure Policy를 사용하여 특정 리소스 종류를 거부하는 방법](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11: 사용자가 Azure Resource Manager 상호 작용할 수 있도록 제한

**지침**: "Microsoft Azure 관리" 앱에 대한 "액세스 차단"을 구성하여 사용자가 Azure Resource Manager와 상호 작용하는 기능을 제한하도록 Azure 조건부 액세스를 구성합니다.

- [Azure Resource Manager에 대한 액세스를 차단하도록 조건부 액세스를 구성하는 방법](../role-based-access-control/conditional-access-azure-management.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="secure-configuration"></a>보안 구성

*자세한 내용은 [Azure 보안 벤치 마크: 보안 구성](../security/benchmarks/security-control-secure-configuration.md)을 참조 하세요.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: 모든 Azure 리소스에 대한 보안 구성 설정

**지침**: Azure Policy를 사용 하 여 Cognitive Services 컨테이너에 대 한 표준 보안 구성을 정의 하 고 구현 합니다. "Cognitiveservices account" 네임 스페이스의 Azure Policy 별칭을 사용 하 여 Redis 인스턴스의 Azure Cache 구성을 감사 하거나 적용 하는 사용자 지정 정책을 만듭니다.

- [사용 가능한 Azure 정책 별칭을 확인하는 방법](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: 보안 Azure 리소스 구성 유지 관리

**지침**: Azure Policy [거부] 및 [없는 경우 배포] 효과를 사용 하 여 Azure 리소스에서 보안 설정을 적용 합니다.

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

- [Azure Policy 효과 이해](../governance/policy/concepts/effects.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure 리소스 구성을 안전하게 저장

**지침**: Cognitive Services 컨테이너 및 관련 리소스에 대 한 사용자 지정 Azure Policy 정의 또는 Azure Resource Manager 템플릿을 사용 하는 경우 Azure Repos를 사용 하 여 코드를 안전 하 게 저장 하 고 관리 합니다.

- [Azure DevOps에 코드를 저장하는 방법](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Azure Repos 설명서](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7: Azure 리소스에 대 한 구성 관리 도구 배포

**지침**: "Microsoft Cache" 네임 스페이스의 Azure Policy 별칭을 사용 하 여 시스템 구성을 경고, 감사 및 적용 하기 위한 사용자 지정 정책을 만들 수 있습니다. 또한 정책 예외를 관리하는 프로세스와 파이프라인을 개발합니다.

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9: Azure 리소스에 대 한 자동화 된 구성 모니터링 구현

**지침**: "cognitiveservices account" 네임 스페이스에 Azure Policy 별칭을 사용 하 여 사용자 지정 Azure Policy 정의를 만들어 시스템 구성을 경고, 감사 및 적용 합니다. Redis 인스턴스 및 관련 리소스에 대 한 Azure 캐시에 대 한 구성을 자동으로 적용 하려면 [감사], [거부] 및 [없는 경우 배포] 효과를 Azure Policy 사용 합니다.

- [Azure Policy를 구성하고 관리하는 방법](../governance/policy/tutorials/create-and-manage.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="711-manage-azure-secrets-securely"></a>7.11: 안전하게 Azure 비밀 관리

**지침**: Cognitive Services API에 액세스 하는 데 사용 되는 Azure App Service에서 실행 되는 Azure 가상 머신 또는 웹 응용 프로그램의 경우 Azure Key Vault와 함께 관리 서비스 ID를 사용 하 여 Cognitive Services 키 관리를 간소화 하 고 보호 합니다. Key Vault 일시 삭제를 사용 하도록 설정 했는지 확인 합니다.

- [Azure 관리 Id와 통합 하는 방법](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Key Vault를 만드는 방법](/azure/key-vault/quick-create-portal)

- [Key Vault에 인증 하는 방법](../key-vault/general/authentication.md)

- [Key Vault 액세스 정책을 할당 하는 방법](../key-vault/general/assign-access-policy-portal.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: 안전하게 자동으로 ID 관리

**지침**: Cognitive Services API에 액세스 하는 데 사용 되는 Azure App Service에서 실행 되는 Azure 가상 머신 또는 웹 응용 프로그램의 경우 Azure Key Vault와 함께 관리 서비스 ID를 사용 하 여 Cognitive Services 키 관리를 간소화 하 고 보호 합니다. Key Vault 일시 삭제를 사용하도록 설정되어 있는지 확인합니다.

관리 되는 id를 사용 하 여 azure AD (Azure Active Directory)에서 자동으로 관리 되는 id를 Azure 서비스에 제공 합니다. 관리 Id를 사용 하면 코드에 자격 증명 없이 Azure Key Vault를 포함 하 여 Azure AD 인증을 지 원하는 모든 서비스에 인증할 수 있습니다.

- [관리 Id를 구성 하는 방법](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

- [Azure 관리 Id와 통합 하는 방법](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: 의도하지 않은 자격 증명 노출 제거

**지침**: 자격 증명 스캐너를 구현하여 코드 내에서 자격 증명을 식별합니다. 또한 자격 증명 스캐너는 검색된 자격 증명을 더 안전한 위치(예: Azure Key Vault)로 이동하도록 추천합니다.

- [자격 증명 스캐너를 설정하는 방법](https://secdevtools.azurewebsites.net/helpcredscan.html)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="malware-defense"></a>맬웨어 방어

*자세한 내용은 [Azure 보안 벤치 마크: 맬웨어 방어](../security/benchmarks/security-control-malware-defense.md)를 참조 하세요.*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: 비 컴퓨팅 Azure 리소스에 업로드할 파일 미리 검사

**지침**: Microsoft 맬웨어 방지는 azure 서비스 (예: azure Cache for Redis)를 지 원하는 기본 호스트에서 사용 하도록 설정 되어 있지만 고객 콘텐츠에서 실행 되지 않습니다.

App Service, Data Lake Storage, Blob Storage, Azure Database for PostgreSQL 등의 비 계산 Azure 리소스에 업로드 되는 콘텐츠를 사전 검사 합니다. Microsoft는 이러한 인스턴스의 데이터에 액세스할 수 없습니다.

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="data-recovery"></a>데이터 복구

*자세한 내용은 [Azure 보안 벤치 마크: 데이터 복구](../security/benchmarks/security-control-data-recovery.md)를 참조 하세요.*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: 정기 자동 백업 확인

**지침**: Microsoft Azure 저장소 계정의 데이터는 항상 자동으로 복제 되어 내구성 및 고가용성을 보장 합니다. Azure Storage는 계획된 이벤트 그리고 일시적인 하드웨어 오류, 네트워크 또는 정전, 대규모 자연 재해 등의 계획되었거나 계획되지 않은 이벤트로부터 데이터를 보호하기 위해 데이터를 복사합니다. 동일한 데이터 센터, 동일한 지역 내의 영역 데이터 센터 또는 지리적으로 분리된 지역 간에 데이터를 복제하도록 선택할 수 있습니다. 

또한 수명 주기 관리 기능을 사용 하 여 데이터를 보관 계층에 백업할 수 있습니다. 또한 저장소 계정에 저장 된 백업에 대해 일시 삭제를 사용 하도록 설정 합니다.

- [Azure Storage 중복성 및 Service-Level 계약 이해](../storage/common/storage-redundancy.md)

- [Azure Blob Storage 수명 주기 관리](../storage/blobs/storage-lifecycle-management-concepts.md)

- [Azure Storage Blob에 대한 일시 삭제](../storage/blobs/soft-delete-blob-overview.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: 전체 시스템 백업을 수행 하 고 고객 관리 키를 백업 합니다.

**지침**: Azure Resource Manager을 사용 하 여 Cognitive Services 및 관련 리소스를 배포할 수 있습니다. Azure Resource Manager은 템플릿을 내보내는 기능을 제공 하므로 개발 수명 주기 내내 솔루션을 다시 배포 하 고 리소스가 일관 된 상태로 배포 될 수 있습니다. Azure Automation를 사용 하 여 정기적으로 Azure Resource Manager 템플릿 내보내기 API를 호출 합니다. Azure Key Vault 내에서 미리 공유한 키를 백업 합니다.

- [Azure Resource Manager 개요](../azure-resource-manager/management/overview.md)

- [Azure Resource Manager 템플릿을 사용 하 여 Cognitive Services 리소스를 만드는 방법](https://docs.microsoft.com/azure/cognitive-services/resource-manager-template?tabs=portal)

- [Azure Portal에서 템플릿에 대 한 단일 및 다중 리소스 내보내기](../azure-resource-manager/templates/export-template-portal.md)

- [리소스 그룹-템플릿 내보내기](/rest/api/resources/resourcegroups/exporttemplate)

- [Azure Automation 소개](../automation/automation-intro.md)

- [Azure에서 키 자격 증명 모음 키를 백업하는 방법](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: 고객 관리 키를 비롯 한 모든 백업 유효성 검사

**지침**: 필요한 경우 격리 된 구독에 정기적으로 Azure Resource Manager 템플릿 배포를 정기적으로 수행할 수 있는지 확인 합니다. 백업 된 미리 공유한 키의 복원을 테스트 합니다.

- [ARM 템플릿을 사용 하 여 리소스 배포 및 Azure Portal](../azure-resource-manager/templates/deploy-portal.md)

- [Azure에서 키 자격 증명 모음 키를 복원하는 방법](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: 백업 및 고객이 관리 하는 키를 보호 해야 합니다.

**지침**: Azure devops를 사용 하 여 Azure Resource Manager 템플릿을 안전 하 게 저장 하 고 관리 합니다. Azure DevOps에서 관리 하는 리소스를 보호 하기 위해 Azure DevOps와 통합 된 경우 Azure Active Directory (Azure AD)에 정의 된 특정 사용자, 기본 제공 보안 그룹 또는 그룹에 대 한 사용 권한을 부여 하거나 거부할 수 있습니다. 또는 Team Foundation Server와 통합 된 경우 Active Directory.

Azure 역할 기반 액세스 제어를 사용 하 여 고객이 관리 하는 키를 보호 합니다. Soft-Delete를 사용 하도록 설정 하 고 Key Vault 보호를 제거 하 여 실수로 또는 악의적으로 삭제 되지 않도록 키를 보호 합니다. 

- [Azure DevOps에 코드를 저장하는 방법](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Azure DevOps의 사용 권한 및 그룹 정보](/azure/devops/organizations/security/about-permissions)

- [Azure Storage Blob에 대한 일시 삭제](../storage/blobs/soft-delete-blob-overview.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="incident-response"></a>사고 대응

자세한 내용은 [Azure Security Benchmark: 인시던트 응답](../security/benchmarks/security-control-incident-response.md)을 참조하세요.

### <a name="101-create-an-incident-response-guide"></a>10.1: 인시던트 대응 지침 만들기

**지침**: 조직에 대한 인시던트 대응 지침을 작성합니다. 검색에서 사후 검토에 이르는 인시던트 처리/관리 단계뿐만 아니라 담당자의 모든 역할을 정의하는 인시던트 대응 계획이 있는지 확인합니다.

- [Azure Security Center 내에서 워크플로 자동화를 구성하는 방법](../security-center/security-center-planning-and-operations-guide.md)

- [자체 보안 인시던트 대응 프로세스를 구축하는 방법에 대한 지침](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process)

- [또한 NIST의 컴퓨터 보안 인시던트 처리 가이드를 활용 하 여 고유한 인시던트 대응 계획을 만들 수 있습니다.](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: 인시던트 점수 매기기 및 우선 순위 지정 절차 만들기

**지침**: Security Center에서 심각도를 각 경고에 할당하여 먼저 조사해야 하는 경고에 대한 우선 순위를 지정합니다. 심각도는 경고를 실행 하는 데 사용 되는 찾기 또는 분석에 사용 되는 것과 경고를 발생 시킨 활동의 악의적인 의도를 받은 신뢰 수준에 Security Center 따라 달라 집니다. 

또한 구독(예: 프로덕션, 비 프로덕션)을 명확하게 표시하고 Azure 리소스를 명확하게 식별하고 분류하는 명명 시스템을 만듭니다.

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="103-test-security-response-procedures"></a>10.3: 보안 대응 프로시저 테스트

**지침**: 시스템의 인시던트 대응 기능을 정기적으로 테스트합니다. 약점과 격차를 식별하고 필요에 따라 계획을 수정합니다.

- [NIST의 게시물을 참조하세요. IT 계획 및 기능에 대한 테스트, 학습 및 연습 프로그램에 대한 안내](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: 보안 인시던트 연락처 세부 정보 제공 및 보안 인시던트에 대한 경고 알림 구성

**지침**: MSRC(Microsoft 보안 대응 센터)에서 불법적이거나 권한이 없는 당사자가 고객 데이터에 액세스했다고 검색하는 경우 Microsoft에서 보안 인시던트 연락처 정보를 사용하여 사용자에게 연락합니다.  문제가 해결되었는지 확인하기 위해 사후에 인시던트를 검토합니다.

- [Azure Security Center 보안 연락처를 설정하는 방법](../security-center/security-center-provide-security-contact-details.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: 보안 경고를 인시던트 대응 시스템에 통합

**지침**: 연속 내보내기 기능을 사용 하 여 Security Center 경고 및 권장 사항을 내보냅니다. 연속 내보내기를 사용하면 경고 및 추천 사항을 수동으로 또는 지속적으로 내보낼 수 있습니다. Security Center data connector를 사용 하 여 경고를 Azure 센티널로 스트리밍할 수 있습니다.

- [연속 내보내기를 구성하는 방법](../security-center/continuous-export.md)

- [경고를 Azure Sentinel로 스트림하는 방법](../sentinel/connect-azure-security-center.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: 보안 경고에 대한 대응 자동화

**지침**: Security Center의 워크플로 자동화 기능을 사용 하 여 보안 경고 및 권장 사항에 대 한 "Logic Apps"를 통해 응답을 자동으로 트리거합니다.

- [워크플로 자동화 및 Logic Apps를 구성하는 방법](../security-center/workflow-automation.md)

**책임**: Customer

**Azure Security Center 모니터링**: 없음

## <a name="penetration-tests-and-red-team-exercises"></a>침투 테스트 및 레드 팀 연습

*자세한 내용은 [Azure 보안 벤치 마크: 침투 테스트 및 레드 팀 연습](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md)을 참조 하세요.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1: Azure 리소스에 대 한 정기적인 침투 테스트를 수행 하 고 모든 중요 한 보안 결과를 수정 하세요.

**지침**: Engagement의 Microsoft 클라우드 침투 테스트 규칙에 따라 침투 테스트가 Microsoft 정책을 위반 하지 않는지 확인 합니다. Microsoft의 전략과 Microsoft에서 관리하는 클라우드 인프라, 서비스, 애플리케이션에 대한 레드 팀 실행 및 실시간 사이트 침투 테스트를 사용합니다. 

- [침투 테스트 시행 규칙](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud 레드 팀](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**책임**: 공유됨

**Azure Security Center 모니터링**: 없음

## <a name="next-steps"></a>다음 단계

- [Azure Security Benchmark V2 개요](/azure/security/benchmarks/overview)를 참조하세요.
- [Azure 보안 기준](/azure/security/benchmarks/security-baselines-overview)에 대해 자세히 알아보세요.
