---
title: Azure Monitor에서 System Center Operations Manager, Zabbix 및 Nagios의 경고 관리
description: Azure Monitor에서 System Center Operations Manager, Zabbix 및 Nagios의 경고 관리
ms.service: azure-monitor
ms.subservice: alerts
ms.topic: conceptual
author: anantr
ms.author: robb
ms.date: 09/24/2018
ms.openlocfilehash: 0e774633542f9b7f8b7de9f8f187821bfbe6b9cf
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555625"
---
# <a name="manage-alerts-from-system-center-operations-manager-zabbix-and-nagios-in-azure-monitor"></a>Azure Monitor에서 System Center Operations Manager, Zabbix 및 Nagios의 경고 관리

이제 [Azure Monitor](https://aka.ms/azure-alerts-overview)에서 Nagios, Zabbix 및 System Center Operations Manager의 경고를 볼 수 있습니다. 이러한 경고는 Nagios/Zabbix 서버 또는 System Center Operations Manager와의 통합에서 Log Analytics로 제공됩니다. 

## <a name="prerequisites"></a>전제 조건
Log Analytics 리포지토리에서 경고 유형인 모든 레코드를 Azure Monitor로 가져오므로, 이러한 레코드를 수집하는 데 필요한 구성을 수행해야 합니다.
1. **Nagios** 및 **Zabbix** 경고의 경우 Log Analytics로 [경고를 보내도록](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-alerts-nagios-zabbix?toc=%2Fazure%2Fazure-monitor%2Ftoc.json) [해당 서버를 구성](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)합니다.
1. **System Center Operations Manager** 경고의 경우 [Operations Manager 관리 그룹을 Log Analytics 작업 영역에 연결](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents)합니다. 이 작업 후에 Azure 솔루션 Marketplace에서 [경고 관리](https://docs.microsoft.com/azure/azure-monitor/platform/alert-management-solution) 솔루션을 배포합니다. 완료되면 System Center Operations Manager에서 생성된 모든 경고를 Log Analytics로 가져옵니다.

## <a name="view-your-alert-instances"></a>경고 인스턴스 보기
Log Analytics로 가져오기를 구성하고 나면, [Azure Monitor](https://aka.ms/azure-alerts-overview)에서 이러한 모니터링 서비스의 경고 인스턴스 보기를 시작할 수 있습니다. Azure Monitor에 표시되면 [경고 인스턴스 관리](https://aka.ms/managing-alert-instances), [이 경고에 대해 생성된 스마트 그룹 관리](https://aka.ms/managing-smart-groups), [경고 및 스마트 그룹의 상태 변경](https://aka.ms/managing-alert-smart-group-states)을 수행할 수 있습니다.

> [!NOTE]
>  1. 이 솔루션을 사용 하면 Azure Monitor에서 발생 한 System Center Operations Manager/Zabbix/Nagios 경고 인스턴스만 볼 수 있습니다. 경고 규칙 구성을 보고 편집하는 작업은 해당 모니터링 도구에서만 가능합니다. 
>  1. 발생한 모든 경고 인스턴스는 Azure Monitor 및 Azure Log Analytics 둘 다에서 사용할 수 있습니다. 현재 둘 중에서 선택하거나 발생한 특정 경고만 수집할 수는 없습니다.
>  1. 기본 원격 분석 형식을 사용할 수 없기 때문에 System Center Operations Manager Zabbix 및 Nagios의 모든 경고에는 "알 수 없음" 신호 유형이 있습니다.
>  1. Nagios 경고는 상태 저장이 아닙니다. 예를 들어 경고의 [모니터 조건](https://aka.ms/azure-alerts-overview)이 “발생함”에서 “해결됨”으로 바뀌지 않습니다. 대신, “발생함” 및 “해결됨”이 모두 별도의 경고 인스턴스로 표시됩니다. 

