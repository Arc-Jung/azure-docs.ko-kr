---
title: Azure Event Grid에 게시하는 Durable Functions(미리 보기)
description: Durable Functions에 대한 자동 Azure Event Grid 게시를 구성하는 방법을 알아봅니다.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: f0fbb46320b896008b6a1343357f016a9f57b0fe
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231440"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Event Grid에 게시하는 Durable Functions(미리 보기)

이 문서에서는 오케스트레이션 수명 주기 이벤트(예: 만들어짐, 완료됨 및 실패)를 사용자 지정 [Azure Event Grid 토픽](https://docs.microsoft.com/azure/event-grid/overview)에 게시하도록 Durable Functions를 설정하는 방법을 설명합니다.

이 기능이 유용한 몇 가지 시나리오는 다음과 같습니다.

* **DevOps scenarios like blue/green deployments**: You might want to know if any tasks are running before implementing the [side-by-side deployment strategy](durable-functions-versioning.md#side-by-side-deployments).

* **고급 모니터링 및 진단 지원**: 쿼리에 최적화된 외부 저장소(예: SQL 데이터베이스 또는 CosmosDB)에서 오케스트레이션 상태 정보를 추적할 수 있습니다.

* **장기 실행 백그라운드 작업**: 장기 실행 백그라운드 작업에 Durable Functions를 사용하는 경우 이 기능을 사용하면 현재 상태를 알 수 있습니다.

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

## <a name="prerequisites"></a>전제 조건

* Install [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) in your Durable Functions project.
* [Azure Storage 에뮬레이터](../../storage/common/storage-use-emulator.md)를 설치합니다.
* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)를 설치하거나 [Azure Cloud Shell](../../cloud-shell/overview.md)을 사용합니다.

## <a name="create-a-custom-event-grid-topic"></a>Create a custom event grid topic

Create an event grid topic for sending events from Durable Functions. 다음 지침에서는 Azure CLI를 사용하여 토픽을 만드는 방법을 보여 줍니다. PowerShell 또는 Azure Portal을 사용하여 수행하는 방법에 대한 자세한 내용은 다음 문서를 참조하세요.

* [Event Grid 빠른 시작: 사용자 지정 이벤트 만들기 - PowerShell](../../event-grid/custom-event-quickstart-powershell.md)
* [Event Grid 빠른 시작: 사용자 지정 이벤트 만들기 - Azure Portal](../../event-grid/custom-event-quickstart-portal.md)

### <a name="create-a-resource-group"></a>리소스 그룹 만들기

`az group create` 명령을 사용하여 리소스 그룹을 만듭니다. Currently, Azure Event Grid doesn't support all regions. For information about which regions are supported, see the [Azure Event Grid overview](../../event-grid/overview.md).

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>사용자 지정 토픽 만들기

An event grid topic provides a user-defined endpoint that you post your event to. `<topic_name>`을 토픽의 고유한 이름으로 바꿉니다. 토픽 이름은 DNS 항목이 되므로 고유해야 합니다.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>엔드포인트 및 키 가져오기

토픽의 엔드포인트를 가져옵니다. `<topic_name>`을 선택한 이름으로 바꿉니다.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

토픽 키를 가져옵니다. `<topic_name>`을 선택한 이름으로 바꿉니다.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

이제 토픽에 이벤트를 보낼 수 있습니다.

## <a name="configure-azure-event-grid-publishing"></a>Azure Event Grid 게시 구성

Durable Functions 프로젝트에서 `host.json` 파일을 찾습니다.

`durableTask` 속성에 `eventGridTopicEndpoint` 및 `eventGridKeySettingName`을 추가합니다.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

The possible Azure Event Grid configuration properties can be found in the [host.json documentation](../functions-host-json.md#durabletask). After you configure the `host.json` file, your function app sends lifecycle events to the event grid topic. This works when you run your function app both locally and in Azure.```

함수 앱 및 `local.setting.json`에서 토픽 키에 대한 앱 설정을 지정합니다. 다음 JSON은 로컬 디버깅에 대한 `local.settings.json` 샘플입니다. `<topic_key>`를 토픽 키로 바꿉니다.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

[스토리지 에뮬레이터](../../storage/common/storage-use-emulator.md)가 작동하는지 확인합니다. 실행하기 전에 `AzureStorageEmulator.exe clear all` 명령을 실행하는 것이 좋습니다.

## <a name="create-functions-that-listen-for-events"></a>이벤트를 수신 대기하는 함수 만들기

함수 앱을 만듭니다. It's best to locate it in the same region as the event grid topic.

### <a name="create-an-event-grid-trigger-function"></a>Create an event grid trigger function

수명 주기 이벤트를 받는 함수를 만듭니다. **사용자 지정 함수**를 선택합니다.

![사용자 지정 함수 만들기 선택](./media/durable-functions-event-publishing/functions-portal.png)

Event Grid 트리거를 선택하고, `C#`을 선택합니다.

![Event Grid 트리거 선택](./media/durable-functions-event-publishing/eventgrid-trigger.png)

함수 이름을 입력한 다음, `Create`를 선택합니다.

![Event Grid 트리거 만들기](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

다음 코드가 있는 함수가 만들어집니다.

#### <a name="precompiled-c"></a>미리 컴파일된 C#
```csharp
public static void Run([HttpTrigger] JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

#### <a name="c-script"></a>C# 스크립트

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

`Add Event Grid Subscription`를 선택합니다. This operation adds an event grid subscription for the event grid topic that you created. 자세한 내용은 [Azure Event Grid의 개념](https://docs.microsoft.com/azure/event-grid/concepts)을 참조하세요.

![Event Grid 트리거 링크 선택](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

**토픽 종류**에 대해 `Event Grid Topics`를 선택합니다. Select the resource group that you created for the event grid topic. Then select the instance of the event grid topic. `Create`를 누릅니다.

![Event Grid 구독을 만듭니다.](./media/durable-functions-event-publishing/eventsubscription.png)

이제 수명 주기 이벤트를 받을 준비가 되었습니다.

## <a name="create-durable-functions-to-send-the-events"></a>이벤트를 보내는 Durable Functions 만들기

Durable Functions 프로젝트에서 로컬 컴퓨터에 대한 디버깅을 시작합니다.  다음 코드는 Durable Functions에 대한 템플릿 코드와 동일합니다. 이미 로컬 컴퓨터에 `host.json` 및 `local.settings.json`을 구성했습니다.

### <a name="precompiled-c"></a>미리 컴파일된 C#

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.DurableTask;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] IDurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequestMessage req,
            [DurableClient] IDurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

> [!NOTE]
> The previous code is for Durable Functions 2.x. For Durable Functions 1.x, you must use `DurableOrchestrationContext` instead of `IDurableOrchestrationContext`, `OrchestrationClient` attribute instead of the `DurableClient` attribute, and you must use the `DurableOrchestrationClient` parameter type instead of `IDurableOrchestrationClient`. For more information about the differences between versions, see the [Durable Functions versions](durable-functions-versions.md) article.

Postman 또는 브라우저를 통해 `Sample_HttpStart`를 호출하면 Durable Functions에서 수명 주기 이벤트를 보내기 시작합니다. 로컬 디버깅에 대한 엔드포인트는 일반적으로 `http://localhost:7071/api/Sample_HttpStart`입니다.

Azure Portal에서 만든 함수의 로그를 봅니다.

```
2019-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2019-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2019-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2019-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2019-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2019-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>이벤트 스키마

다음 목록에서는 수명 주기 이벤트 스키마를 설명합니다.

* **`id`** : Unique identifier for the event grid event.
* **`subject`** : Path to the event subject. `durable/orchestrator/{orchestrationRuntimeStatus}`에 대한 답변에 설명되어 있는 단계를 성공적으로 완료하면 활성화됩니다. `{orchestrationRuntimeStatus}`는 `Running`, `Completed`, `Failed` 및 `Terminated`입니다.  
* **`data`** : Durable Functions Specific Parameters.
  * **`hubName`** : [TaskHub](durable-functions-task-hubs.md) name.
  * **`functionName`** : Orchestrator function name.
  * **`instanceId`** : Durable Functions instanceId.
  * **`reason`** : Additional data associated with the tracking event. 자세한 내용은 [Durable Functions의 진단(Azure Functions)](durable-functions-diagnostics.md)을 참조하세요.
  * **`runtimeStatus`** : Orchestration Runtime Status. 실행 중, 완료됨, 실패, 취소됨입니다.
* **`eventType`** : "orchestratorEvent"
* **`eventTime`** : Event time (UTC).
* **`dataVersion`** : Version of the lifecycle event schema.
* **`metadataVersion`** :  Version of the metadata.
* **`topic`** : Event grid topic resource.

## <a name="how-to-test-locally"></a>로컬로 테스트하는 방법

로컬로 테스트하려면 [ngrok](../functions-bindings-event-grid.md#local-testing-with-ngrok)를 사용합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Durable Functions의 인스턴스 관리 알아보기](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Durable Functions의 버전 관리 알아보기](durable-functions-versioning.md)