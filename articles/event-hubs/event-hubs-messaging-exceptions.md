---
title: Azure Event Hubs-예외
description: 이 문서에서는 Azure Event Hubs 메시징 예외 및 제안된 작업의 목록을 제공합니다.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: b3a7023dc44d7280284fec1eccf00a3dbe88b7ec
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90908381"
---
# <a name="event-hubs-messaging-exceptions---net"></a>Event Hubs 메시징 예외-.NET
이 섹션에서는 .NET Framework Api에 의해 생성 된 .NET 예외를 나열 합니다. 

## <a name="exception-categories"></a>예외 범주

Event Hubs .NET Api는 다음 범주에 해당 하는 예외를 생성 하는 데 사용할 수 있는 연결 된 작업과 함께 다음 범주에 해당 하는 예외를 생성 합니다.

 - 사용자 코딩 오류: 
 
   - [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1&preserve-view=true)
   - [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1&preserve-view=true)
   - [System.OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1&preserve-view=true)
   - [SerializationException.](/dotnet/api/system.runtime.serialization.serializationexception?view=netcore-3.1&preserve-view=true)
   
   일반 작업: 계속 하기 전에 코드를 수정 하십시오.
 
 - 설치/구성 오류: 
 
   - [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception)
   - [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception)
   - [System.UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1&preserve-view=true)
   
   일반 작업: 구성을 검토 하 고 필요한 경우 변경 합니다.
   
 - 일시적 예외: 
 
   - [ServiceBus. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception)
   - [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception)
   - [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception)
   - [ServiceBus. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)
   
   일반 조치: 작업을 다시 시도하거나 사용자에게 알립니다.
 
 - 기타 예외: 
 
   - [TransactionException](/dotnet/api/system.transactions.transactionexception?view=netcore-3.1&preserve-view=true)
   - [System.TimeoutException](#timeoutexception)
   - [ServiceBus. Microsoft.servicebus.messaging.messagelocklostexception](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception)
   - [ServiceBus. Microsoft.servicebus.messaging.sessionlocklostexception](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)
   
   일반 작업: 예외 형식에만 적용 됩니다. 다음 섹션의 표를 참조 하세요. 

## <a name="exception-types"></a>예외 유형
다음 표에서는 메시징 예외 유형과 원인, 사용자가 수행할 수 있는 제안 조치 참고를 열거합니다.

| 예외 유형 | 설명/원인/예 | 권장 조치 | 자동/즉시 다시 시도 참고 |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1&preserve-view=true) |서버가 지정 된 시간 내에 요청 된 작업에 응답 하지 않았습니다 .이 작업은 [Operationtimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)에 의해 제어 됩니다. 서버가 요청된 작업을 완료했을 수도 있습니다. 이 예외는 네트워크 또는 기타 인프라 지연으로 인해 발생할 수 있습니다. |필요한 경우 시스템 상태에서 일관성을 확인하고 다시 시도합니다.<br /> [TimeoutException](#timeoutexception)을 참조하세요. | 일부 경우 다시 시도하면 문제가 해결될 수 있습니다. 코드에 재시도 논리를 추가합니다. |
| [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1&preserve-view=true) |요청 된 사용자 작업은 서버 또는 서비스 내에서 허용 되지 않습니다. 자세한 내용은 예외 메시지를 참조하세요. 예를 들어 [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) 모드에서 메시지를 받은 경우 [Complete](/dotnet/api/microsoft.servicebus.messaging.receivemode) 가 이 예외를 생성합니다. | 코드 및 설명서를 확인합니다. 요청된 작업이 유효한지 확인합니다. | 다시 시도 해도 도움이 되지 않습니다. |
| [OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1&preserve-view=true) | 이미 종료, 중단 또는 삭제된 개체에서 작업을 호출하려 시도합니다. 드문 경우지만 앰비언트 트랜잭션이 이미 삭제되었을 수 있습니다. | 코드를 확인 하 고 삭제 된 개체에 대 한 작업을 호출 하지 않는지 확인 합니다. | 다시 시도 해도 도움이 되지 않습니다. |
| [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1&preserve-view=true) | [Tokenprovider](/dotnet/api/microsoft.servicebus.tokenprovider) 개체가 토큰을 가져올 수 없습니다. 토큰이 잘못 되었거나 토큰에 작업을 수행 하는 데 필요한 클레임이 포함 되어 있지 않습니다. | 올바른 값을 사용하여 토큰 공급자를 만드는지 확인합니다. Access Control Service의 구성을 확인 합니다. | 일부 경우 다시 시도하면 문제가 해결될 수 있습니다. 코드에 재시도 논리를 추가합니다. |
| [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1&preserve-view=true)<br /> [ArgumentNullException](/dotnet/api/system.argumentnullexception?view=netcore-3.1&preserve-view=true)<br />[ArgumentOutOfRangeException](/dotnet/api/system.argumentoutofrangeexception?view=netcore-3.1&preserve-view=true) | 메서드에 제공된 하나 이상의 인수가 잘못되었습니다. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 또는 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)에 제공된 URI에 경로 세그먼트가 포함됩니다. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 또는 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory)에 제공된 URI 스키마가 올바르지 않습니다. 속성 값이 32KB보다 큽니다. | 호출 코드를 확인하고 인수가 정확한지 확인합니다. | 재시도로 해결되지 않습니다. |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | 작업과 연결된 엔터티가 없거나 삭제되었습니다. | 엔터티가 있는지 확인합니다. | 재시도로 해결되지 않습니다. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | 클라이언트가 이벤트 허브로 연결을 설정할 수 없습니다. |제공된 호스트 이름이 정확하며 호스트에 연결할 수 있는지 확인합니다. | 간헐적인 연결 문제라면 재시도로 문제를 해결할 수 있습니다. |
| [ServiceBus ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | 서비스가 지금은 요청을 처리할 수 없습니다. | 클라이언트가 잠시 대기한 후 작업을 다시 시도할 수 있습니다. <br /> [ServerBusyException](#serverbusyexception)을 참조하세요. | 클라이언트가 일정 시간 이후에 다시 시도할 수 있습니다. 재시도에서 다른 예외가 발생한 경우 해당 예외의 재시도 작동을 확인합니다. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | 다음 상황에 발생할 수 있는 일반 메시징 예외: 다른 엔터티 형식에 속하는(예: topic) 이름이나 경로를 사용하여 [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) 를 만들려고 시도합니다. 1MB보다 큰 메시지를 전송하려고 시도합니다. 서버 또는 서비스가 요청을 처리하는 동안 오류가 발생했습니다. 자세한 내용은 예외 메시지를 참조하세요. 이것은 보통 일시적인 예외입니다. | 코드를 확인하고 메시지 본문에서 직렬화 가능 개체(또는 사용자 지정 직렬 변환기 사용)만 사용하는지 확인합니다. 설명서에서 지원되는 속성 값 유형을 확인하고 지원되는 유형만 사용합니다. [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) 속성을 확인합니다. 이 값이 **true**이면 작업을 다시 시도할 수 있습니다. | 재시도 동작이 정의되지 않았으면 도움이 되지 않습니다. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | 해당 서비스 네임스페이스에서 이미 다른 엔터티가 사용하는 이름으로 엔터티를 만들려고 합니다. | 기존 엔터티를 삭제하거나 만들려는 엔터티에 다른 이름을 선택합니다. | 재시도로 해결되지 않습니다. |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | 메시징 엔터티의 최대 허용 크기에 도달했습니다. 이 예외는 소비자별 그룹 수준에서 최대 수신자 수(5)가 이미 열려 있는 경우에 발생할 수 있습니다. | 엔터티나 하위 큐에서 메시지를 수신하여 엔터티에 공간을 만듭니다. <br /> [QuotaExceededException](#quotaexceededexception)을 참조하세요. | 그 사이 메시지가 제거되었으면 재시도가 도움이 될 수 있습니다. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | 비활성화된 엔터티의 런타임 작업에 대한 요청입니다. |엔터티를 활성화합니다. | 그 사이에 엔터티가 활성화된 경우 다시 시도하면 문제가 해결될 수 있습니다. |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | 메시지 페이로드가 1mb 제한을 초과 합니다. 이 1mb 제한은 시스템 속성 및 모든 .NET 오버 헤드를 포함할 수 있는 총 메시지 수에 대 한 것입니다. | 메시지 페이로드의 크기를 줄인 다음 작업을 다시 시도합니다. |재시도로 해결되지 않습니다. |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) 은 특정 엔터티에 대한 할당량이 초과됐음을 나타냅니다.

이 예외는 소비자별 그룹 수준에서 최대 수신자 수(5)가 이미 열려 있는 경우에 발생할 수 있습니다.

### <a name="event-hubs"></a>Event Hubs
Event Hubs는 이벤트 허브당 20개의 소비자 그룹으로 제한됩니다. 더 만들려고 하면 [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception)을 받습니다. 

## <a name="timeoutexception"></a>TimeoutException
[TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1&preserve-view=true) 은 사용자가 시작한 작업이 작업 시간 제한보다 더 오래 걸린다는 것을 나타냅니다. 

Event Hubs의 경우 시간 제한은 연결 문자열의 일부로 또는 [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)를 통해 지정됩니다. 오류 메시지 자체는 다를 수 있지만 현재 작업에 대해 지정된 시간 제한 값을 항상 포함합니다. 

시간 제한은 서비스를 실행 하는 리소스에 대 한 Event Hubs 서비스 업데이트 (또는) OS 업데이트와 같은 유지 관리 작업 중 또는 유지 관리 작업 중에 발생 합니다. OS를 업데이트 하는 동안 엔터티가 이동 되 고 노드를 업데이트 하거나 다시 부팅 하 여 시간 초과가 발생할 수 있습니다. Azure Event Hubs 서비스에 대 한 SLA (서비스 수준 계약) 세부 정보는 [Event Hubs에 대 한 sla](https://azure.microsoft.com/support/legal/sla/event-hubs/)를 참조 하세요. 


### <a name="common-causes"></a>일반적인 원인
이 오류에 대한 일반적인 두 가지 원인은 잘못된 구성 또는 일시적 서비스 오류입니다.

- **잘못된 구성** 작동 조건에 대한 작업 시간 제한이 너무 작을 수도 있습니다. 클라이언트 SDK에서 작업 시간 제한에 대한 기본값은 60초입니다. 코드에 너무 작은 값으로 설정된 값이 있는지 확인합니다. 네트워크 및 CPU 사용량의 조건은 특정 작업을 완료 하는 데 걸리는 시간에 영향을 줄 수 있으므로 작업 시간 제한을 작은 값으로 설정 하면 안 됩니다.
- **일시적 서비스 오류** Event Hubs 서비스에서 예를 들어 트래픽이 높은 기간 동안 요청 처리에 지연이 발생할 수 있습니다. 이러한 경우 작업이 성공할 때까지 지연 후 작업을 다시 시도할 수 있습니다. 동일한 작업을 여러 번 시도한 후에도 계속 실패하는 경우 알려진 서비스 중단이 있는지 [Azure 서비스 상태 사이트](https://azure.microsoft.com/status/) 를 방문하세요.

## <a name="serverbusyexception"></a>ServerBusyException

[Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) 또는 [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception)은 서버가 오버로드되었음을 나타냅니다. 이 예외에 대한 두 개의 관련 오류 코드가 있습니다.

### <a name="error-code-50002"></a>오류 코드 50002
이 오류는 다음 두 가지 이유 중 하나로 발생할 수 있습니다.

- 이벤트 허브의 모든 파티션에서 부하가 균등 하 게 분산 되지 않으며 하나의 파티션이 로컬 처리량 단위 제한에 도달 합니다.
    
    **해결**방법: 파티션 배포 전략을 수정 하거나 [EventHubClient (eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) 를 시도 하는 것이 도움이 될 수 있습니다.

- Event Hubs 네임 스페이스에 충분 한 처리량 단위가 없습니다. [Azure Portal](https://portal.azure.com) 의 Event Hubs 네임 스페이스 창에서 **메트릭** 화면을 확인 하 여 확인할 수 있습니다. 포털에는 집계 된 (1 분) 정보가 표시 되지만 처리량은 실시간으로 측정 되므로 예상에 불과합니다.

    **해결**방법: 네임 스페이스에서 처리량 단위를 늘려도 도움이 될 수 있습니다. 포털의 Event Hubs 네임스페이스 화면에 있는 **규모** 창에서 이 작업을 수행할 수 있습니다. 또는 [자동 팽창](event-hubs-auto-inflate.md)을 사용할 수 있습니다.

### <a name="error-code-50001"></a>오류 코드 50001

이 오류는 드물게 발생합니다. 네임스페이스에 대한 코드를 실행하는 컨테이너의 CPU가 낮아서 Event Hubs 부하 분산 장치가 몇 초 이내에 시작되지 못할 때 이 오류가 발생합니다.

**해결**방법: GetRuntimeInformation 메서드에 대 한 호출을 제한 합니다. Azure Event Hubs는 초당 GetRuntimeInfo에 대 한 초당 최대 50 개의 호출을 지원 합니다. 한도에 도달 하면 다음과 유사한 예외가 표시 될 수 있습니다.

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```


## <a name="next-steps"></a>다음 단계

Event Hubs에 대한 자세한 내용은 다음 링크를 참조하세요.

* [Event Hubs 개요](./event-hubs-about.md)
* [이벤트 허브 만들기](event-hubs-create.md)
* [Event Hubs FAQ](event-hubs-faq.md)
