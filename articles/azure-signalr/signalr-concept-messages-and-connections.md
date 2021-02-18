---
title: Azure SignalR Service의 메시지 및 연결
description: Azure SignalR Service의 메시지와 연결에 대한 주요 개념을 설명합니다.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: zhshang
ms.openlocfilehash: 9d0e94cf2318db777bb44c15037f73531cd969fa
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100593330"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Azure SignalR Service의 메시지 및 연결

Azure SignalR Service의 청구 모델은 연결 수와 메시지 수를 기반으로 합니다. 이 문서에서는 요금을 청구하기 위해 메시지 및 연결을 정의하고 계산하는 방식을 설명합니다.


## <a name="message-formats"></a>메시지 형식 

Azure SignalR Service는 ASP.NET Core SignalR와 동일한 형식 ( [JSON](https://www.json.org/) 및 [MessagePack](/aspnet/core/signalr/messagepackhubprotocol))을 지원 합니다.

## <a name="message-size"></a>메시지 크기

Azure SignalR Service는 메시지 크기에 제한이 없습니다.

큰 메시지는 2KB 이하의 작은 메시지로 분할되어 별도로 전송됩니다. SDK는 메시지 분할 및 어셈블리를 처리합니다. 개발자의 수고가 필요하지 않습니다.

큰 메시지는 메시지 성능에 부정적인 영향을 줍니다. 되도록이면 작은 메시지를 사용하고, 테스트를 통해 각 사용 사례 시나리오에 맞는 최적의 메시지 크기를 확인하세요.

## <a name="how-messages-are-counted-for-billing"></a>요금을 청구할 메시지를 계산하는 방법

Azure SignalR Service에서 나가는 아웃바운드 메시지만 청구 대상에 포함됩니다. 클라이언트와 서버 간의 ping 메시지는 무시됩니다.

2KB를 초과하는 메시지는 2KB 크기의 메시지 여러 개로 계산됩니다. Azure Portal의 메시지 수 차트는 허브당 메시지 100개마다 업데이트됩니다.

예를 들어 하나의 응용 프로그램 서버와 세 개의 클라이언트가 있다고 가정해 보겠습니다.

앱 서버는 모든 연결 된 클라이언트에 1kb 메시지를 브로드캐스트합니다. 앱 서버에서 서비스로의 메시지는 무료 인바운드 메시지로 간주 됩니다. 서비스에서 각 클라이언트로 보내는 세 개의 메시지만 아웃 바운드 메시지로 청구 됩니다.

클라이언트 A는 앱 서버를 거치지 않고 1kb 메시지를 다른 클라이언트 B로 보냅니다. 클라이언트 A에서 서비스로의 메시지는 무료 인바운드 메시지입니다. 서비스에서 클라이언트로 B로의 메시지는 아웃 바운드 메시지로 청구 됩니다.

3 개의 클라이언트와 하나의 응용 프로그램 서버가 있는 경우 한 클라이언트는 서버에서 모든 클라이언트에 브로드캐스트할 수 있는 4KB 메시지를 보냅니다. 청구 되는 메시지 수는 8 개입니다. 서비스에서 응용 프로그램 서버로 메시지 하나, 서비스에서 클라이언트로 보내는 3 개의 메시지입니다. 각 메시지는 2KB 메시지 2개로 계산됩니다.

## <a name="how-connections-are-counted"></a>연결 수를 계산하는 방법

Azure SignalR Service를 사용 하는 서버 연결 및 클라이언트 연결이 있습니다. 기본적으로 각 응용 프로그램 서버는 허브 당 5 개의 초기 연결로 시작 하 고 각 클라이언트에는 하나의 클라이언트 연결이 있습니다.

Azure Portal에 표시되는 연결 수에는 서버 연결과 클라이언트 연결이 모두 포함됩니다.

예를 들어 두 개의 응용 프로그램 서버가 있고 코드에 5 개의 허브를 정의 한다고 가정 합니다. 서버 연결 수는 50:2 개의 앱 서버 * 5 허브 * 허브 당 5 개의 연결입니다.

ASP.NET SignalR은 다른 방법으로 서버 연결 수를 계산합니다. 고객이 정의하는 허브 외에도 기본 허브 하나가 포함됩니다. 기본적으로 각 응용 프로그램 서버에는 5 개의 초기 서버 연결이 필요 합니다. 기본 허브의 초기 연결 수는 다른 허브와 동일 하 게 유지 됩니다.

서비스와 응용 프로그램 서버는 연결 상태 동기화를 유지 하 고 서버 연결을 조정 하 여 더 나은 성능 및 서비스 안정성을 얻습니다.  따라서 서버 연결 번호가 시간에서 변경 되는 것을 볼 수 있습니다.

## <a name="how-inboundoutbound-traffic-is-counted"></a>인바운드/아웃바운드 트래픽을 계산하는 방법

서비스로 전송 된 메시지는 인바운드 메시지입니다. 서비스에서 전송 된 메시지는 아웃 바운드 메시지입니다. 트래픽은 바이트 단위로 계산됩니다.

## <a name="related-resources"></a>관련 참고 자료

- [Azure Monitor의 집계 유형](../azure-monitor/essentials/metrics-supported.md#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR 구성](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)