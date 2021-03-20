---
title: Azure Event Grid 이벤트에 대한 이벤트 처리기로 릴레이 하이브리드 연결
description: Azure Event Grid 이벤트에 대한 이벤트 처리기로 Azure Relay 하이브리드 연결을 사용하는 방법을 설명합니다.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 38233a2e103600f07837ce9a1ad8d63fe7e4fb99
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91270205"
---
# <a name="relay-hybrid-connection-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid 이벤트에 대한 이벤트 처리기로 릴레이 하이브리드 연결
이벤트 처리기는 이벤트가 전송된 위치입니다. 처리기는 이벤트를 처리하기 위한 추가 작업을 수행합니다. 이벤트를 처리하도록 여러 Azure 서비스가 자동으로 구성되며, **Azure Relay** 가 그 중 하나입니다. 

Azure **Relay 하이브리드 연결** 을 사용하여 엔터프라이즈 네트워크 내에 있으며 공개적으로 액세스할 수 있는 엔드포인트가 없는 애플리케이션에 이벤트를 보냅니다.

## <a name="tutorials"></a>자습서
Azure Relay 하이브리드 연결을 이벤트 처리기로 사용하는 예제는 다음 자습서를 참조하세요. 

|제목  |Description  |
|---------|---------|
| [자습서: 하이브리드 연결로 이벤트 전송](custom-event-to-hybrid-connection.md) | 수신기 애플리케이션에서 프로세싱에 대한 기존 하이브리드 연결에 사용자 지정 이벤트를 보냅니다. |

## <a name="rest-example-for-put"></a>REST 예제(PUT의 경우)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "HybridConnection",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Relay/namespaces/<RELAY NAMESPACE NAME>/hybridconnections/<HYBRID CONNECTION NAME>"
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>다음 단계
지원되는 이벤트 처리기 목록은 [이벤트 처리기](event-handlers.md) 문서를 참조하세요. 