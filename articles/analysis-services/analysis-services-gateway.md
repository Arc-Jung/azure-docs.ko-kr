---
title: Azure Analysis Services에 대 한 온-프레미스 데이터 게이트웨이 Microsoft Docs
description: Azure의 Analysis Services 서버가 온-프레미스 데이터 원본에 연결되는 경우 온-프레미스 게이트웨이가 필요합니다.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6ca96f76287482a445d8a9a1cdc441333b36efbd
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97739606"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>온-프레미스 데이터 게이트웨이를 사용 하 여 온-프레미스 데이터 원본에 연결

온-프레미스 데이터 게이트웨이를 사용하면 온-프레미스 데이터 원본과 클라우드의 Azure Analysis Services 서버 사이에서 데이터를 안전하게 전송할 수 있습니다. 동일한 지역에서 여러 Azure Analysis Services 서버를 사용 하는 것 외에도 최신 버전의 게이트웨이는 Azure Logic Apps, Power BI, Power Apps 및 파워 자동화를 사용 하 여 작동 합니다. 설치한 게이트웨이는 이러한 모든 서비스에서 동일 하지만 Azure Analysis Services 및 Logic Apps에 몇 가지 추가 단계가 있습니다.

여기에서 제공 하는 정보는 Azure Analysis Services 온-프레미스 데이터 게이트웨이와 작동 하는 방식에 따라 다릅니다. 일반적으로 게이트웨이에 대 한 자세한 내용 및 다른 서비스와 함께 작동 하는 방법에 대 한 자세한 내용은 [온-프레미스 데이터 게이트웨이 란?](/data-integration/gateway/service-gateway-onprem)을 참조 하세요.

Azure Analysis Services의 경우 처음으로 게이트웨이를 사용 하 여 설치 하는 과정은 네 부분으로 구성 된 프로세스입니다.

- **설치 프로그램 다운로드 및 실행** - 이 단계는 조직의 컴퓨터에 게이트웨이 서비스를 설치합니다. 또한 [테넌트](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD의 계정을 사용하여 Azure에 로그인합니다. Azure B2B(게스트) 계정은 지원되지 않습니다.

- **게이트웨이 등록** - 이 단계에서는 게이트웨이에 대한 이름 및 복구 키를 지정하고 게이트웨이 클라우드 서비스로 게이트웨이를 등록한 지역을 선택합니다. 모든 지역에 게이트웨이 리소스를 등록할 수 있지만 Analysis Services 서버와 동일한 지역에 있는 것이 좋습니다. 

- **Azure에서 게이트웨이 리소스 만들기** -이 단계에서는 azure에서 게이트웨이 리소스를 만듭니다.

- **서버에 게이트웨이 리소스 연결** -게이트웨이 리소스가 있으면 서버 연결을 시작할 수 있습니다. 동일한 지역에 있는 경우 여러 서버 및 기타 리소스를 연결할 수 있습니다.

## <a name="installing"></a>설치

Azure Analysis Services 환경을 설치 하는 경우 [Azure Analysis Services에 대 한 온-프레미스 데이터 게이트웨이 설치 및 구성](analysis-services-gateway-install.md)에 설명 된 단계를 수행 하는 것이 중요 합니다. 이 문서는 Azure Analysis Services에만 적용 됩니다. Azure에서 온-프레미스 데이터 게이트웨이 리소스를 설정 하 고 리소스에 Azure Analysis Services 서버를 연결 하는 데 필요한 추가 단계를 포함 합니다.

## <a name="connecting-to-a-gateway-resource-in-a-different-subscription"></a>다른 구독의 게이트웨이 리소스에 연결

서버와 동일한 구독에서 Azure 게이트웨이 리소스를 만드는 것이 좋습니다. 그러나 다른 구독의 게이트웨이 리소스에 연결 하도록 서버를 구성할 수 있습니다. 기존 서버 설정을 구성 하거나 포털에서 새 서버를 만들 때 다른 구독에서 게이트웨이 리소스에 연결 하는 것은 지원 되지 않지만 PowerShell을 사용 하 여 구성할 수 있습니다. 자세히 알아보려면 [서버에 게이트웨이 리소스 연결](analysis-services-gateway-install.md#connect-gateway-resource-to-server)을 참조 하세요.

## <a name="ports-and-communication-settings"></a>포트 및 통신 설정

게이트웨이는 Azure Service Bus에 대한 아웃바운드 연결을 만듭니다. 아웃바운드 포트 TCP 443(기본값), 5671, 5672, 9350-9354에서 통신합니다.  게이트웨이에는 인바운드 포트가 필요하지 않습니다.

방화벽에 데이터 영역에 대 한 IP 주소를 포함 해야 할 수 있습니다. [Microsoft Azure 데이터 센터 IP 목록](https://www.microsoft.com/download/details.aspx?id=56519)을 다운로드할 수 있습니다. 이 목록은 매주 업데이트됩니다. Azure 데이터 센터 IP 목록에 나열된 IP 주소는 CIDR 표기법으로 작성됩니다. 자세한 내용은 [CIDR(Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)을 참조하세요.

다음은 게이트웨이에서 사용 되는 정규화 된 도메인 이름입니다.

| 도메인 이름 | 아웃바운드 포트 | Description |
| --- | --- | --- |
| *.powerbi.com |80 |설치 프로그램을 다운로드하는 데 사용되는 HTTP입니다. |
| *.powerbi.com |443 |HTTPS |
| *.analysis.windows.net |443 |HTTPS |
| *. login.windows.net, login.live.com, aadcdn.msauth.net |443 |HTTPS |
| \*.servicebus.windows.net |5671-5672 |AMQP(고급 메시지 큐 프로토콜) |
| \*.servicebus.windows.net |443, 9350-9354 |TCP의 Service Bus Relay에 대한 수신기(Access Control 토큰 획득에 443 필요) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |80 |Power BI 서비스에서 게이트웨이에 연결할 수 없는 경우 인터넷 연결을 테스트하는 데 사용합니다. |
| *.microsoftonline-p.com |443 |구성에 따라 인증에 사용합니다. |
| dc.services.visualstudio.com    |443 |AppInsights에서 원격 분석을 수집 하는 데 사용 됩니다. |

## <a name="next-steps"></a>다음 단계 

다음 문서는 게이트웨이에서 지 원하는 모든 서비스에 적용 되는 온-프레미스 데이터 게이트웨이 일반 콘텐츠에 포함 되어 있습니다.

* [온-프레미스 데이터 게이트웨이 FAQ](/data-integration/gateway/service-gateway-onprem-faq)   
* [온-프레미스 데이터 게이트웨이 앱 사용](/data-integration/gateway/service-gateway-app)   
* [테넌트 수준 관리](/data-integration/gateway/service-gateway-tenant-level-admin)
* [프록시 설정 구성](/data-integration/gateway/service-gateway-proxy)   
* [통신 설정 조정](/data-integration/gateway/service-gateway-communication)   
* [로그 파일 구성](/data-integration/gateway/service-gateway-log-files)   
* [문제 해결](/data-integration/gateway/service-gateway-tshoot)
* [게이트웨이 성능 모니터링 및 최적화](/data-integration/gateway/service-gateway-performance)
