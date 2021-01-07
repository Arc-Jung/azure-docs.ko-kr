---
title: Azure Security Center에서 지원하는 플랫폼 | Microsoft Docs
description: 이 문서에서는 Azure Security Center에서 지원하는 플랫폼 목록을 제공합니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2020
ms.author: memildin
ms.openlocfilehash: 31ff3620bcab6b10b59582daa255ff1f259ab0d3
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96852645"
---
# <a name="supported-platforms"></a>지원되는 플랫폼 

이 페이지에서는 Azure Security Center에서 지 원하는 플랫폼 및 환경을 보여 줍니다.

## <a name="combinations-of-environments"></a>환경 조합 <a name="vm-server"></a>

Azure Security Center는 여러 유형의 하이브리드 환경에서 가상 머신 및 서버를 지원합니다.

* Azure만
* Azure 및 온-프레미스
* Azure 및 기타 클라우드
* Azure, 기타 클라우드 및 온-프레미스

Azure 구독에서 활성화된 Azure 환경의 경우 Azure Security Center는 구독 내에 배포되는 IaaS 리소스를 자동으로 검색합니다.

## <a name="supported-operating-systems"></a>지원되는 운영 체제

Security Center는 [Log Analytics 에이전트](../azure-monitor/platform/agents-overview.md#log-analytics-agent)에 따라 다릅니다. 다음 페이지에 설명된 대로, 이 에이전트에 지원되는 운영 체제 중 하나를 머신이 실행하고 있는지 확인하세요.

* [Windows에서 지원되는 운영 체제에 대한 Log Analytics 에이전트](../azure-monitor/platform/agents-overview.md#supported-operating-systems)
* [Linux에서 지원되는 운영 체제에 대한 Log Analytics 에이전트](../azure-monitor/platform/agents-overview.md#supported-operating-systems)

또한 Log Analytics 에이전트가 [Security Center에 데이터를 전송하도록 제대로 구성](security-center-enable-data-collection.md#manual-agent)되었는지 확인

Windows 및 Linux에서 사용할 수 있는 특정 Security Center 기능에 대한 자세한 내용은 [머신의 기능 적용 범위](security-center-services.md)를 참조하세요.

> [!NOTE]
> Azure Defender는 서버를 보호하도록 설계되었지만 **서버용 Azure Defender** 의 대부분 기능은 Windows 10 머신에서 지원됩니다. 현재 지원되지 않는 기능 중 하나는 [Security Center의 통합 EDR 솔루션: 엔드포인트용 Microsoft Defender](security-center-wdatp.md)입니다.

## <a name="managed-virtual-machine-services"></a>관리형 가상 머신 서비스 <a name="virtual-machine"></a>

AKS(Azure Kubernetes), Azure Databricks 등의 일부 Azure 관리 서비스에 포함된 고객 구독에서 가상 머신을 만들 수도 있습니다. Security Center는 이러한 가상 머신도 검색하며, 지원되는 OS를 사용할 수 있는 경우 Log Analytics 에이전트를 설치 및 구성할 수 있습니다.

## <a name="cloud-services"></a>Cloud Services <a name="cloud-services"></a>

클라우드 서비스에서 실행되는 가상 머신도 지원됩니다. 프로덕션 슬롯에서 실행되는 클라우드 서비스 웹 및 작업자 역할만 모니터링됩니다. 클라우드 서비스에 대한 자세한 내용은 [Azure Cloud Services 개요](../cloud-services/cloud-services-choose-me.md)를 참조하세요.

Azure Stack에 있는 VM에 대한 보호도 지원됩니다. Azure Stack과 Security Center의 통합에 대한 자세한 내용은 [Azure Stack 가상 머신을 Security Center에 온보딩](quickstart-onboard-machines.md)을 참조하세요. 

## <a name="next-steps"></a>다음 단계

- [Security Center에서 Log Analytics 에이전트를 사용하여 데이터를 수집](security-center-enable-data-collection.md)하는 방법에 대해 알아봅니다.
- [Security Center에서 데이터를 관리하고 보호](security-center-data-security.md)하는 방법을 알아봅니다.
- [디자인 고려 사항을 계획하고 이해하여 Azure Security Center를 채택](security-center-planning-and-operations-guide.md)하는 방법을 알아봅니다.