---
title: Azure Service Fabric 모니터링 파트너 | Microsoft Docs
description: 파트너 모니터링 솔루션을 사용하여 Azure Service Fabric을 모니터링하는 방법 알아보기
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: 8353c8846820aa996214e6b119a2f9027b8b6fe6
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74232443"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Azure Service Fabric 모니터링 파트너

이 문서에서는 몇 가지 파트너 솔루션을 사용하여 Service Fabric 애플리케이션, 클러스터 및 인프라를 모니터링하는 방법을 설명합니다. Service Fabric에 대한 통합된 제품을 만들기 위해 아래의 각 파트너와 협력했습니다.

## <a name="dynatrace"></a>Dynatrace

Dynatrace와의 통합은 Service Fabric 클러스터를 모니터링하는 대부분의 기본 제공 기능을 제공합니다. VMSS 인스턴스에 Dynatrace OneAgent를 설치하면 해당 앱 수준까지 성능 카운터 및 Service Fabric 배포의 토폴로지를 제공합니다. Dynatrace는 온-프레미스 모니터링에도 적합합니다. Check out more of the features listed in the [announcement](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) and [instructions](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) to enable Dynatrace on your cluster. 

## <a name="datadog"></a>Datadog

Datadog에는 Windows 및 Linux 인스턴스 모두에 대한 VMSS의 확장명이 있습니다. Datadog를 사용하여 Windows 이벤트 로그를 수집할 수 있으므로 Windows에서 Service Fabric 플랫폼 이벤트를 수집할 수 있습니다. [여기](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric)에서 Datadog에 진단 데이터를 보내는 방법에 대한 지침을 확인합니다.

## <a name="appdynamics"></a>AppDynamics

AppDynamics와 Service Fabric 통합은 애플리케이션 수준에 있습니다. 환경 변수를 업데이트하고 App Dynamics NuGets를 사용하여 AppDynamics에 애플리케이션 원격 분석을 보낼 수 있습니다. AppDynamics와 .NET Service Fabric 애플리케이션을 통합하는 방법은 이러한 [지침](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric)을 참조하세요.

## <a name="new-relic"></a>New Relic

New Relic은 Service Fabric 애플리케이션과 잘 통합되는 다른 애플리케이션 성능 관리 도구입니다. New Relic NuGet 패키지를 설치하고 매니페스트 파일에서 특정 환경 변수를 추가하여 New Relic에 애플리케이션 원격 분석을 보낼 수 있습니다. .NET Service Fabric 애플리케이션에 대해 New Relic 원격 분석을 활성화하려면 이러한 [지침](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric)을 확인합니다.

## <a name="elk"></a>ELK 

ELK 스택은 오픈 소스 기술의 컬렉션입니다. Elasticsearch, Logstash 및 Kibana 이를 조합으로 사용하여 Service Fabric 모니터링 및 진단 데이터를 수집, 저장 및 분석할 수 있습니다. [여기](service-fabric-tutorial-java-elk.md)에 Service Fabric 네이티브 Java 애플리케이션을 사용하여 이 작업을 수행하는 방법에 대한 자습서가 있습니다. 

## <a name="humio"></a>Humio

Humio is a log collection service that can gather logs from your applications and events from Service Fabric in the cloud or on-premises in real time. In addition to live observability, Humio offers state of the art analysis and visualization capabilities for viewing and collecting insights from your diagnostics. Humio has cost effective pricing plans and is built to scale while retaining it's lightening fast speed. It directly integrates with Service Fabric platform events and Application telemetry. You can read more about the Humio and Service Fabric integration [here](https://github.com/humio/service-fabric-humio).

## <a name="next-steps"></a>다음 단계

* Service Fabric에서 [모니터링 및 진단의 개요](service-fabric-diagnostics-overview.md)를 가져옵니다.
* 첫 번째 파티 도구를 사용하여 [일반적인 시나리오를 진단](service-fabric-diagnostics-common-scenarios.md)하는 방법을 알아봅니다.
