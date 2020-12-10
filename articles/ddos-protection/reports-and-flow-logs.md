---
title: Azure DDoS Protection 표준 보고서 및 흐름 로그
description: 보고서 및 흐름 로그를 구성 하는 방법에 대해 알아봅니다.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 0a59c748dba87765537fc7c9d6382c7d2b726f43
ms.sourcegitcommit: 273c04022b0145aeab68eb6695b99944ac923465
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008064"
---
# <a name="configure-ddos-attack-mitigation-reports-and-flow-logs"></a>DDoS 공격 완화 보고서 및 흐름 로그 구성 

Azure DDoS Protection 표준은 DDoS 공격 분석을 통해 자세한 공격 인사이트와 시각화를 제공합니다. DDoS 공격으로부터 자신의 가상 네트워크를 보호하는 고객은 공격 완화 보고서 및 완화 흐름 로그를 통해 공격 트래픽 및 공격을 완화하는 데 수행된 작업에 대해 자세히 파악할 수 있습니다. 다양 한 원격 분석은 DDoS 공격 기간 동안 상세 메트릭을 포함 하 여 Azure Monitor를 통해 노출 됩니다. DDoS Protection에서 노출하는 Azure Monitor 메트릭에 대한 경고를 구성할 수 있습니다. 로깅은 Azure Monitor 진단 인터페이스를 통한 고급 분석을 위해 [Azure 센티널](../sentinel/connect-azure-ddos-protection.md), Splunk (azure Event Hubs), OMS Log Analytics 및 Azure Storage와 추가로 통합 될 수 있습니다.

이 자습서에서는 다음과 같은 작업을 수행하는 방법을 알아봅니다.

> [!div class="checklist"]
> * DDoS 공격 완화 보고서 보기 및 구성
> * DDoS 공격 완화 흐름 로그 보기 및 구성

## <a name="prerequisites"></a>사전 요구 사항

- Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.
- 이 자습서의 단계를 완료 하기 전에 먼저 [Azure DDoS Standard 보호 계획](manage-ddos-protection.md)을 만들어야 합니다.

## <a name="view-and-configure-ddos-attack-mitigation-reports"></a>DDoS 공격 완화 보고서 보기 및 구성

공격 완화 보고서는 집계된 Netflow 프로토콜 데이터를 사용하여 리소스에 대한 공격에 대한 자세한 정보를 제공합니다. 공용 IP 리소스가 공격을 받을 때마다 완화가 시작되는 즉시 보고서 생성이 시작됩니다. 5분마다 생성되는 증분 보고서와 전체 완화 기간에 대한 사후 완화 보고서가 있습니다. 이렇게 하면 DDoS 공격이 더 오랜 시간 지속되는 경우 5분마다 완화 보고서의 최신 스냅샷을 볼 수 있고 공격 완화가 완료되면 전체 요약을 볼 수 있습니다. 

1. 포털의 왼쪽 위에서 **모든 서비스** 를 선택합니다.
2. **필터** 상자에서 *모니터* 를 입력합니다. 결과에 **모니터** 가 표시되면 해당 모니터를 선택합니다.
3. **설정** 아래에서 **진단 설정** 을 선택합니다.
4. 로깅하려는 공용 IP 주소가 포함된 **구독** 및 **리소스 그룹** 을 선택합니다.
5. **리소스 종류** 에 대해 **공용 IP 주소** 를 선택한 다음, 메트릭을 로깅하려는 특정 공용 IP 주소를 선택합니다.
6. **DDoSMitigationReports 로그를 수집하도록 진단 켜기** 를 선택한 다음, 다음 옵션 중에서 필요한 만큼 선택합니다.

    - **스토리지 계정에 보관**: 데이터가 Azure Storage 계정에 기록됩니다. 이 옵션에 대해 자세히 알아보려면 [리소스 로그 보관](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-azure-storage)을 참조 하세요.
    - **이벤트 허브로의 스트림**: 로그 수신기에서 Azure Event Hub를 사용하여 로그를 선택할 수 있도록 합니다. 이벤트 허브는 Splunk 또는 기타 SIEM 시스템과 통합할 수 있습니다. 이 옵션에 대 한 자세한 내용은 [이벤트 허브로 리소스 로그 스트리밍](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-azure-event-hubs)을 참조 하세요.
    - **Log Analytics 보내기**: Azure Monitor 서비스에 로그를 기록 합니다. 이 옵션에 대 한 자세한 내용은 [Azure Monitor 로그에서 사용할 로그 수집](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-log-analytics-workspace)을 참조 하세요.

증분 및 사후 공격 완화 보고서 모두에 포함된 필드는 다음과 같습니다
- 공격 벡터
- 트래픽 통계
- 패킷이 삭제된 이유
- 관련 프로토콜
- 상위 10개 원본 국가 또는 지역
- 상위 10개 원본 ASN

## <a name="view-and-configure-ddos-attack-mitigation-flow-logs"></a>DDoS 공격 완화 흐름 로그 보기 및 구성
공격 완화 흐름 로그를 사용 하면 활성 DDoS 공격 중에 거의 실시간으로 삭제 된 트래픽, 전달 된 트래픽 및 기타 흥미로운 datapoints을 검토할 수 있습니다. 거의 실시간으로 모니터링할 수 있도록 이벤트 허브를 통해이 데이터의 상수 스트림을 Azure 센티널 또는 타사 SIEM 시스템에 수집할 수 있으며, 잠재적인 조치를 취하여 방어 작업의 필요성을 해결할 수 있습니다.

1. 포털의 왼쪽 위에서 **모든 서비스** 를 선택합니다.
2. **필터** 상자에서 *모니터* 를 입력합니다. 결과에 **모니터** 가 표시되면 해당 모니터를 선택합니다.
3. **설정** 아래에서 **진단 설정** 을 선택합니다.
4. 로깅하려는 공용 IP 주소가 포함된 **구독** 및 **리소스 그룹** 을 선택합니다.
5. **리소스 종류** 에 대해 **공용 IP 주소** 를 선택한 다음, 메트릭을 로깅하려는 특정 공용 IP 주소를 선택합니다.
6. **DDoSMitigationFlowLogs 로그를 수집하도록 진단 켜기** 를 선택한 다음, 다음 옵션 중에서 필요한 만큼 선택합니다.

    - **스토리지 계정에 보관**: 데이터가 Azure Storage 계정에 기록됩니다. 이 옵션에 대해 자세히 알아보려면 [리소스 로그 보관](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-azure-storage)을 참조 하세요.
    - **이벤트 허브로의 스트림**: 로그 수신기에서 Azure Event Hub를 사용하여 로그를 선택할 수 있도록 합니다. 이벤트 허브는 Splunk 또는 기타 SIEM 시스템과 통합할 수 있습니다. 이 옵션에 대 한 자세한 내용은 [이벤트 허브로 리소스 로그 스트리밍](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-azure-event-hubs)을 참조 하세요.
    - **Log Analytics 보내기**: Azure Monitor 서비스에 로그를 기록 합니다. 이 옵션에 대 한 자세한 내용은 [Azure Monitor 로그에서 사용할 로그 수집](../azure-monitor/platform/resource-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#send-to-log-analytics-workspace)을 참조 하세요.

이 [템플릿은](https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Enable%20Diagnostic%20Logging/Azure%20Policy) 진단 로깅을 사용 하는 Azure Policy 정의를 만듭니다.

### <a name="azure-sentinel-data-connector"></a>Azure 센티널 데이터 커넥터

공격 완화 흐름 로그를 Azure 센티널에 연결 하 고, 통합 문서에서 데이터를 보고 분석 하며, 사용자 지정 경고를 만들어 조사 프로세스에 통합할 수 있습니다. Azure 센티널에 연결 하려면 [Azure 센티널에 연결](../sentinel/connect-azure-ddos-protection.md)을 참조 하세요. 

![Azure 센티널 DDoS 커넥터](./media/ddos-attack-telemetry/azure-sentinel-ddos.png)

### <a name="azure-ddos-protection-workbook"></a>Azure DDoS Protection 통합 문서

이 Azure Resource Manager (ARM) 템플릿을 사용 하 여 공격 분석 통합 문서를 배포할 수 있습니다. 이 통합 문서는 항상 stake의 기능을 쉽게 이해 하기 위해 여러 개의 필터링 가능한 패널에서 공격 데이터를 시각화 합니다. 이 ARM 템플릿을 배포할 때 다음 사항을 입력 해야 합니다.

* 작업 영역 이름
* 작업 영역 ResourceGroup
* 작업 영역 구독 ID

[![Azure에 배포](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzure-Network-Security%2Fmaster%2FAzure%2520DDoS%2520Protection%2FAzure%2520DDoS%2520Protection%2520Workbook%2FAzureDDoSWorkbook_ARM.json)

흐름 로그에는 다음 필드가 포함 됩니다. 
- 원본 IP
- 대상 IP
- 원본 포트 
- 대상 포트 
- 프로토콜 유형 
- 완화 중에 수행된 작업

![DDoS Protection 통합 문서](./media/ddos-attack-telemetry/ddos-attack-analytics-workbook.png)

공격 분석은 공용 IP 주소의 가상 네트워크에서 DDoS Protection Standard를 사용 하도록 설정한 경우에만 작동 합니다. 

## <a name="sample-log-outputs"></a>샘플 로그 출력

다음 스크린샷은 로그 출력의 예입니다.

### <a name="ddosmitigationflowlogs"></a>DDoSMitigationFlowLogs

![DDoS Protection Gdosmitilogs](./media/ddos-attack-telemetry/ddos-mitigation-flow-logs.png)

### <a name="ddosprotectionnotifications"></a>DDoSProtectionNotifications

![DDoS Protection DDoSProtectionNotifications](./media/ddos-attack-telemetry/ddos-protection-notifications.png)

### <a name="ddosmitigationreports"></a>DDoSMitigationReports

![DDoS Protection Gdosmitigationreports](./media/ddos-attack-telemetry/ddos-mitigation-reports.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 작업 방법을 알아보았습니다.

- DDoS 공격 완화 보고서 보기 및 구성
- DDoS 공격 완화 흐름 로그 보기 및 구성

DDoS 공격을 테스트 하 고 시뮬레이트하는 방법에 대해 알아보려면 시뮬레이션 테스트 가이드를 참조 하세요.

> [!div class="nextstepaction"]
> [시뮬레이션을 통해 테스트](test-through-simulations.md)