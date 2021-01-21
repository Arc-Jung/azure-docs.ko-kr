---
title: Azure Service Bus FAQ | Microsoft Docs
description: 이 문서에서는 Azure Service Bus에 대 한 FAQ (질문과 대답)를 제공 합니다.
ms.topic: article
ms.date: 09/16/2020
ms.openlocfilehash: 576df12a9d53ec3585f8691016cd250bf1ba4be3
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98623110"
---
# <a name="azure-service-bus---frequently-asked-questions-faq"></a>Azure Service Bus-질문과 대답 (FAQ)

이 문서는 Microsoft Azure Service Bus에 대한 일부 자주 묻는 질문에 대해 설명합니다. 또한 일반적인 Azure 가격 책정 및 지원 정보는 [Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)에서 확인할 수 있습니다.


## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus에 대한 일반적인 질문
### <a name="what-is-azure-service-bus"></a>Azure Service Bus란?
[Azure Service Bus](service-bus-messaging-overview.md) 은 분리 된 시스템 간에 데이터를 보낼 수 있도록 하는 비동기 메시징 클라우드 플랫폼입니다. Microsoft는이 기능을 서비스로 제공 합니다. 즉,이 기능을 사용 하기 위해 자체 하드웨어를 호스트할 필요가 없습니다.

### <a name="what-is-a-service-bus-namespace"></a>Service Bus 네임스페이스란?
[네임 스페이스](service-bus-create-namespace-portal.md) 는 응용 프로그램 내에서 Service Bus 리소스의 주소를 지정 하기 위한 범위 지정 컨테이너를 제공 합니다. 네임스페이스를 만드는 작업은 Service Bus를 사용하는 데 필요하고 Service Bus를 시작하는 첫 번째 단계 중 하나입니다.

### <a name="what-is-an-azure-service-bus-queue"></a>Azure Service Bus 큐란?
[Service Bus 큐](service-bus-queues-topics-subscriptions.md)는 메시지가 저장되는 엔터티입니다. 큐는 서로 통신해야 하는 여러 애플리케이션 또는 분산된 애플리케이션의 여러 부분이 있는 경우에 유용합니다. 큐는 여러 제품(메시지)를 해당 위치에서 받고 보내는 배포 센터와 비슷합니다.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Azure Service Bus 토픽 및 구독이란?
토픽은 큐로 시각화할 수 있고 여러 구독을 사용하는 경우 더 풍부한 메시징 모델이 되며 기본적으로는 일대다 통신 도구입니다. 이 게시/구독 모델(또는 *pub/sub*)을 사용하면 여러 애플리케이션에서 받은 메시지를 여러 구독을 사용하여 토픽에 메시지를 전송하는 애플리케이션을 활성화합니다.

### <a name="what-is-a-partitioned-entity"></a>분할된 엔터티란?
일반적인 큐 또는 항목은 단일 메시지 broker에서 처리되며 하나의 메시징 저장소에 저장됩니다. 기본 및 표준 메시징 계층 에서만 지원 되며 [분할 된 큐 또는 토픽](service-bus-partitioning.md) 은 여러 메시지 브로커에 의해 처리 되 고 여러 메시징 저장소에 저장 됩니다. 즉, 분할된 큐 또는 항목의 전체 처리량은 단일 메시지 broker 또는 메시징 저장소의 성능으로 제한되지 않습니다. 또한 메시징 저장소의 일시적인 중단은 분할 된 큐 또는 토픽을 사용할 수 없게 렌더링 하지 않습니다.

분할 된 엔터티를 사용 하는 경우 순서가 보장 되지 않습니다. 파티션을 사용할 수 없는 이벤트에서도 여전히 메세지를 보내고 다른 파티션에서 메시지를 수신할 수 있습니다.

 분할된 엔터티는 [프리미엄 SKU](service-bus-premium-messaging.md)에서 지원되지 않습니다. 

### <a name="where-does-azure-service-bus-store-customer-data"></a><a name="in-region-data-residency"></a>어디에서 고객 데이터를 저장 Azure Service Bus?
Azure Service Bus 고객 데이터를 저장 합니다. 이 데이터는 Service Bus에 의해 단일 지역에 자동으로 저장 되므로이 서비스는 [보안 센터](https://azuredatacentermap.azurewebsites.net/)에 지정 된 데이터를 포함 하 여 지역 데이터 상주 요구 사항을 자동으로 충족 합니다.

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>방화벽에서 열어야 하는 포트는 어느 것인가요? 
Azure Service Bus에서 다음 프로토콜을 사용하여 메시지를 주고받을 수 있습니다.

- 고급 메시지 큐 프로토콜 1.0 (AMQP)
- TLS (HTTPS)를 사용한 하이퍼텍스트 전송 프로토콜 1.1

이러한 프로토콜을 사용 하 여 Azure Service Bus와 통신 하기 위해 열어야 하는 아웃 바운드 TCP 포트는 다음 표를 참조 하세요.

| 프로토콜 | 포트 | 세부 정보 | 
| -------- | ----- | ------- | 
| AMQP | 5671 | TLS를 사용 하는 AMQP [AMQP 프로토콜 가이드](service-bus-amqp-protocol-guide.md)를 참조하세요. | 
| HTTPS | 443 | 이 포트는 HTTP/REST API 및 AMQP over Websocket에 사용 됩니다. |

HTTPS 포트는 일반적으로 포트 5671을 통해 AMQP를 사용 하는 경우, 클라이언트 Sdk에서 수행 하는 여러 관리 작업 및 Azure Active Directory (사용 되는 경우)에서 토큰을 획득 하 여 HTTPS를 통해 실행 되는 경우에도 아웃 바운드 통신에 필요 합니다. 

공식 Azure Sdk는 일반적으로 Service Bus에서 메시지를 보내고 받기 위해 AMQP 프로토콜을 사용 합니다. 

[!INCLUDE [service-bus-websockets-options](../../includes/service-bus-websockets-options.md)]

.NET Framework에 대 한 이전 Windowsazure.servicebus ServiceBus 패키지에는 "NetMessaging"이 라고도 하는 레거시 "SBMP" (Service Bus Messaging Protocol)를 사용 하는 옵션이 있습니다. 이 프로토콜은 TCP 포트 9350-9354를 사용 합니다. 이 패키지의 기본 모드는 해당 포트가 통신에 사용할 수 있는지 여부를 자동으로 감지 하 고, 그렇지 않은 경우 포트 443을 통해 TLS를 사용 하는 Websocket로 전환 합니다. `Https` [](/dotnet/api/microsoft.servicebus.connectivitymode) [`ServiceBusEnvironment.SystemConnectivity`](/dotnet/api/microsoft.servicebus.servicebusenvironment.systemconnectivity) 응용 프로그램에 전역적으로 적용 되는 설정에 대해 하도록 connectivitymode.autodetect 지정할를 설정 하 여이 설정을 재정의 하 고이 모드를 적용할 수 있습니다.

### <a name="what-ip-addresses-do-i-need-to-add-to-allow-list"></a>허용 목록에 추가 해야 하는 IP 주소는 무엇 인가요?
연결에 대 한 허용 목록에 추가할 올바른 IP 주소를 찾으려면 다음 단계를 수행 합니다.

1. 명령 프롬프트에서 다음 명령을 실행합니다. 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. `Non-authoritative answer`에서 반환된 IP 주소를 적어 둡니다. 

네임 스페이스에 **영역 중복성** 을 사용 하는 경우 몇 가지 추가 단계를 수행 해야 합니다. 

1. 먼저 네임스페이스에서 nslookup을 실행합니다.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. **신뢰할 수 없는 응답** 섹션에서 다음 형식 중 하나로 표시되는 이름을 적어 둡니다. 

    ```
    <name>-s1.cloudapp.net
    <name>-s2.cloudapp.net
    <name>-s3.cloudapp.net
    ```
3. 접미사 s1, s2 및 s3를 포함하는 각 이름에 대해 nslookup을 실행하여 세 개의 가용성 영역에서 실행되는 세 인스턴스의 IP 주소를 모두 가져옵니다. 

    > [!NOTE]
    > 명령에서 반환 된 IP 주소는 `nslookup` 고정 ip 주소가 아닙니다. 그러나 기본 배포가 삭제 되거나 다른 클러스터로 이동 될 때까지 일정 하 게 유지 됩니다.

### <a name="where-can-i-find-the-ip-address-of-the-client-sendingreceiving-messages-tofrom-a-namespace"></a>네임 스페이스에서 메시지를 보내고 받는 클라이언트의 IP 주소는 어디에서 찾을 수 있나요? 
네임 스페이스에서 메시지를 보내거나 받는 클라이언트의 IP 주소를 기록 하지 않습니다. 키를 다시 생성 하 여 모든 기존 클라이언트가 인증에 실패 하 고 [AZURE RBAC (역할 기반 액세스 제어](authenticate-application.md#azure-built-in-roles-for-azure-service-bus)) 설정을 검토 하 여 허용 된 사용자나 응용 프로그램만 네임 스페이스에 액세스할 수 있도록 합니다. 

**프리미엄** 네임 스페이스를 사용 하는 경우 [IP 필터링](service-bus-ip-filtering.md), [가상 네트워크 서비스 끝점](service-bus-service-endpoints.md)및 [개인 끝점](private-link-service.md) 을 사용 하 여 네임 스페이스에 대 한 액세스를 제한 합니다. 

## <a name="best-practices"></a>모범 사례
### <a name="what-are-some-azure-service-bus-best-practices"></a>일부 Azure Service Bus 모범 사례는 무엇인가요?
[Service Bus를 사용한 성능 향상에 대한 모범 사례][Best practices for performance improvements using Service Bus] 참조 - 이 문서에서는 메시지를 교환할 때 성능을 최적화하는 방법을 설명합니다.

### <a name="what-should-i-know-before-creating-entities"></a>엔터티를 만들기 전에 무엇을 알아야 하나요?
큐 및 토픽에서 다음 속성을 변경할 수 없습니다. 엔터티를 프로 비전 할 때 이러한 제한 사항을 고려해 야 합니다. 이러한 속성은 새 대체 엔터티를 만들지 않고 수정할 수 없기 때문입니다.

* 분할
* 세션
* 중복 검색
* Express 엔터티

## <a name="pricing"></a>가격 책정
이 섹션은 Service Bus 가격 책정 구조에 대한 일부 자주 묻는 질문을 답변합니다.

[Service Bus 가격 및 대금 청구](https://azure.microsoft.com/pricing/details/service-bus/) 문서에서는 Service Bus의 요금 청구 기준에 대해 설명합니다. Service Bus 가격 책정 옵션에 대한 특정 정보는 [Service Bus 가격 책정 세부 정보](https://azure.microsoft.com/pricing/details/service-bus/)를 참조하세요.

또한 일반적인 Azure 가격 책정 정보는 [Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)에서 확인할 수 있습니다. 

### <a name="how-do-you-charge-for-service-bus"></a>Service Bus 요금을 어떻게 청구하나요?
Service Bus 가격 책정에 대한 전체 내용은 [Service Bus 가격 책정 세부 정보][Pricing overview]를 참조하세요. 언급된 가격 외에도 애플리케이션이 프로비전되는 데이터 센터의 외부에서 송신을 위해 연결된 데이터 전송에 요금이 부과됩니다.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-isnt"></a>Service Bus를 어떻게 사용하면 데이터가 전송의 대상이 되나요? 무엇 인가요?
지정된 Azure 지역 내에서 데이터 전송은 비용뿐만 아니라 인바운드 데이터 전송 없이 제공됩니다. 지역 외부의 데이터 전송은 [여기](https://azure.microsoft.com/pricing/details/bandwidth/)에서 찾을 수 있는 송신 요금이 청구됩니다.

### <a name="does-service-bus-charge-for-storage"></a>Service Bus는 스토리지에 대한 요금을 청구하나요?
아니요. Service Bus는 저장소에 대해 요금이 청구 되지 않습니다. 그러나 큐/토픽 마다 지속 될 수 있는 최대 데이터 양을 제한 하는 할당량이 있습니다. 다음 FAQ를 참조하세요.

### <a name="i-have-a-service-bus-standard-namespace-why-do-i-see-charges-under-resource-group-system"></a>Service Bus 표준 네임 스페이스가 있습니다. 리소스 그룹 ' $system '에 요금이 표시 되는 이유는 무엇 인가요?
최근 청구 구성 요소를 업그레이드 Azure Service Bus. 이러한 변경으로 인해 Service Bus 표준 네임 스페이스가 있는 경우 리소스 그룹 ' $system ' 아래에 리소스 '/subscriptions/<azure_subscription_id>/Hsourceg/$system/providers/Microsoft.ServiceBus/namespaces/$system '에 대 한 줄 항목이 표시 될 수 있습니다.

이러한 요금은 Service Bus 표준 네임 스페이스를 프로 비전 한 Azure 구독 당 기본 요금을 나타냅니다. 

이러한 요금은 새로운 것이 아니라 이전 청구 모델에도 존재 한다는 것을 명심 해야 합니다. 지금은 ' $system ' 아래에 나열 됩니다. ' $System ' 리소스 ID의 특정 리소스에 연결 되지 않은 구독 수준 요금을 그룹화 하는 새로운 청구 시스템의 제약 조건으로 인해 수행 됩니다.

## <a name="quotas"></a>할당량

Service Bus 제한 및 할당량 목록은 [Service Bus 할당량 개요][Quotas overview]를 참조하세요.

### <a name="how-to-handle-messages-of-size--1-mb"></a>1MB를 초과하는 메시지 크기를 처리하는 방법
Service Bus 메시징 서비스(큐 및 토픽/구독)를 사용하면 애플리케이션이 최대 256KB(표준 계층) 또는 1MB(프리미엄 계층) 크기의 메시지를 보낼 수 있습니다. 크기가 1mb를 초과 하는 메시지를 처리 하는 경우 [이 블로그 게시물](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern)에 설명 된 클레임 검사 패턴을 사용 합니다.

## <a name="troubleshooting"></a>문제 해결
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>네임스페이스를 다른 구독에서 삭제한 후 만들 수 없는 이유는 무엇인가요? 
구독에서 네임스페이스를 삭제한 후에 다른 구독에서 동일한 이름으로 다시 만들려면 4시간 정도 기다려야 합니다. 그렇지 않으면 다음 오류 메시지가 표시될 수 있습니다. `Namespace already exists` 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Azure Service Bus API 및 해당 제안된 작업에 의해 생성된 일부 예외는 무엇인가요?
가능한 Service Bus 예외의 목록은 [예외 개요][Exceptions overview]를 참조하세요.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>공유 액세스 서명이란 무엇이고 어떤 언어가 서명 생성을 지원하나요?
공유 액세스 서명은 SHA-256 보안 해시 또는 URI에 따른 인증 메커니즘입니다. Node.js, PHP, Java, Python 및 c #에서 고유한 서명을 생성 하는 방법에 대 한 자세한 내용은 [공유 액세스 서명][Shared Access Signatures] 문서를 참조 하세요.

## <a name="subscription-and-namespace-management"></a>구독 및 네임스페이스 관리
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>다른 Azure 구독으로 네임스페이스를 마이그레이션하려면 어떻게 해야 하나요?

[Azure Portal](https://portal.azure.com) 또는 PowerShell 명령을 사용하여 Azure 구독 간에 네임스페이스를 이동할 수 있습니다. 작업을 실행 하려면 네임 스페이스가 이미 활성화 되어 있어야 합니다. 명령을 실행하는 사용자는 원본 및 대상 구독에 대한 관리자여야 합니다.

#### <a name="portal"></a>포털

Azure Portal을 사용하여 Service Bus 네임스페이스를 다른 구독으로 마이그레이션하려면 [여기](../azure-resource-manager/management/move-resource-group-and-subscription.md#use-the-portal)에 있는 지침을 따르세요. 

#### <a name="powershell"></a>PowerShell

다음 PowerShell 명령 시퀀스는 네임스페이스를 Azure 구독 간에 이동합니다. 이 작업을 실행하려면 네임스페이스 이미 활성 상태여야 하며 PowerShell 명령을 실행하는 사용자는 원본 및 대상 구독의 관리자여야 합니다.

```powershell
# Create a new resource group in target subscription
Select-AzSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>다음 단계
Service Bus에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Azure Service Bus 프리미엄 소개(블로그 게시물)](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Azure Service Bus 프리미엄 소개(Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus 개요](service-bus-messaging-overview.md)
* [Service Bus 큐 시작](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
