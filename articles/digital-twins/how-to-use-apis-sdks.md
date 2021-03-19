---
title: Azure Digital Twins API 및 SDK 사용
titleSuffix: Azure Digital Twins
description: SDK를 통해 Azure 디지털 쌍 Api를 사용 하는 방법을 참조 하세요.
author: baanders
ms.author: baanders
ms.date: 06/04/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f4f3fc8c928cd284088cc51120f1a7b485b4fac0
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104595348"
---
# <a name="use-the-azure-digital-twins-apis-and-sdks"></a>Azure Digital Twins API 및 SDK 사용

Azure Digital Twins는 인스턴스 및 해당 요소를 관리 하기 위한 **제어 평면** api와 **데이터 평면 api** 를 모두 제공 합니다. 
* 제어 평면 Api는 [ARM (Azure Resource Manager)](../azure-resource-manager/management/overview.md) api 이며, 인스턴스를 만들고 삭제 하는 것과 같은 리소스 관리 작업을 다룹니다. 
* 데이터 평면 Api는 Azure Digital Twins Api 이며 모델, 쌍 및 그래프 관리와 같은 데이터 관리 작업에 사용 됩니다.

이 문서에서는 사용할 수 있는 Api와 상호 작용 하는 방법에 대 한 개요를 제공 합니다. [Postman](how-to-use-postman.md)과 같은 도구를 통해 또는 SDK를 통해 REST api를 연결 된 swagger에 직접 사용할 수 있습니다.

## <a name="overview-control-plane-apis"></a>개요: 제어 평면 Api

제어 평면 Api는 전체 인스턴스를 만들거나 삭제 하는 것과 같은 작업을 수행 하기 위해 Azure Digital Twins 인스턴스 전체를 관리 하는 데 사용 되는 [ARM](../azure-resource-manager/management/overview.md) api입니다. 또한 끝점을 만들고 삭제 하는 데 사용 합니다.

최신 제어 평면 API 버전은 _**2020-12-01**_ 입니다.

제어 평면 Api를 사용 하려면 다음을 수행 합니다.
* [컨트롤 평면 swagger 리포지토리의](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins/stable)최신 Swagger 폴더를 참조 하 여 api를 직접 호출할 수 있습니다. 이 폴더에는 사용법을 보여 주는 예제 폴더도 포함 되어 있습니다.
* 현재에서 컨트롤 Api에 대 한 Sdk에 액세스할 수 있습니다.
  - [**.Net (c #)**](https://www.nuget.org/packages/Microsoft.Azure.Management.DigitalTwins/) ([참조 [자동 생성]](/dotnet/api/overview/azure/digitaltwins/management)) ([원본](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Microsoft.Azure.Management.DigitalTwins))
  - [**Java**](https://search.maven.org/search?q=a:azure-mgmt-digitaltwins) ([참조 [자동 생성]](/java/api/overview/azure/digitaltwins)) ([원본](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/digitaltwins))
  - [**JavaScript**](https://www.npmjs.com/package/@azure/arm-digitaltwins) ([원본](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/digitaltwins/arm-digitaltwins))
  - [**Python**](https://pypi.org/project/azure-mgmt-digitaltwins/) ([원본](https://github.com/Azure/azure-sdk-for-python/tree/release/v3/sdk/digitaltwins/azure-mgmt-digitaltwins))
  - [**Go**](https://pkg.go.dev/github.com/Azure/azure-sdk-for-go/services/digitaltwins/mgmt) ([원본](https://github.com/Azure/azure-sdk-for-go/tree/master/services/digitaltwins/mgmt))

또한 [Azure Portal](https://portal.azure.com) 및 [CLI](how-to-use-cli.md)를 통해 Azure Digital twins와 상호 작용 하 여 제어 평면 api를 연습할 수 있습니다.

## <a name="overview-data-plane-apis"></a>개요: 데이터 평면 Api

데이터 평면 Api는 Azure Digital Twins 인스턴스 내에서 요소를 관리 하는 데 사용 되는 Azure Digital Twins Api입니다. 여기에는 경로 만들기, 모델 업로드, 관계 만들기, 쌍 관리 등의 작업이 포함 됩니다. 이러한 항목은 다음 범주로 크게 나눌 수 있습니다.
* **DigitalTwinModels** -DigitalTwinModels 범주에는 Azure Digital twins 인스턴스에서 [모델](concepts-models.md) 을 관리 하는 api가 포함 되어 있습니다. 관리 작업에는 DTDL에서 작성 된 모델의 업로드, 유효성 검사, 검색 및 삭제가 포함 됩니다.
* **DigitalTwins** -DigitalTwins 범주에는 개발자가 Azure digital 쌍 인스턴스에서 [디지털](concepts-twins-graph.md) 쌍 및 해당 관계를 만들고, 수정 하 고, 삭제할 수 있도록 하는 api가 포함 되어 있습니다.
* **쿼리** -쿼리 범주를 통해 개발자는 관계 간에 쌍으로 된 쌍의 [디지털 쌍 집합을 찾을](how-to-query-graph.md) 수 있습니다.
* **이벤트 경로** -이벤트 경로 범주는 시스템 및 다운스트림 서비스를 통해 [데이터를 라우팅하](concepts-route-events.md)는 api를 포함 합니다.

최신 데이터 평면 API 버전은 _**2020-10-31**_ 입니다.

데이터 평면 Api를 사용 하려면 다음을 수행 합니다.
* Api는 다음을 통해 직접 호출할 수 있습니다.
   - [데이터 평면 swagger 리포지토리의](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins)최신 Swagger 폴더를 참조 합니다. 이 폴더에는 사용법을 보여 주는 예제 폴더도 포함 되어 있습니다. 
   - [API 참조 설명서](/rest/api/azure-digitaltwins/)보기
* **.Net (c #) SDK** 를 사용할 수 있습니다. .NET SDK를 사용 하려면 ...
   - NuGet: [DigitalTwins](https://www.nuget.org/packages/Azure.DigitalTwins.Core)에서 패키지를 보고 추가할 수 있습니다. 
   - [SDK 참조 설명서](/dotnet/api/overview/azure/digitaltwins/client)를 볼 수 있습니다.
   - GitHub: [Azure IoT Digital Twins client library for .net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)의 샘플 폴더를 포함 하 여 SDK 원본을 찾을 수 있습니다. 
   - 이 문서의 [*.net (c #) SDK (데이터 평면)*](#net-c-sdk-data-plane) 섹션을 계속 진행 하 여 자세한 정보 및 사용 예를 볼 수 있습니다.
* **JAVA SDK** 를 사용할 수 있습니다. Java SDK를 사용 하려면 ...
   - Maven에서 패키지를 보고 설치할 수 있습니다. [`com.azure:azure-digitaltwins-core`](https://search.maven.org/artifact/com.azure/azure-digitaltwins-core/1.0.0/jar)
   - [SDK 참조 설명서](/java/api/overview/azure/digitaltwins/client) 를 볼 수 있습니다.
   - GitHub에서 SDK 원본을 찾을 수 있습니다. [Java 용 Azure IoT Digital Twins 클라이언트 라이브러리](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/digitaltwins/azure-digitaltwins-core)
* **JAVASCRIPT SDK** 를 사용할 수 있습니다. JavaScript SDK를 사용 하려면 ...
   - npm: [Azure Azure Digital Twins Core client library For JavaScript](https://www.npmjs.com/package/@azure/digital-twins-core)에서 패키지를 보고 설치할 수 있습니다.
   - [SDK 참조 설명서](/javascript/api/@azure/digital-twins-core/)를 볼 수 있습니다.
   - GitHub에서 SDK 원본을 찾을 수 있습니다. [JavaScript 용 Azure Azure Digital Twins 핵심 클라이언트 라이브러리](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/digitaltwins/digital-twins-core)
* **PYTHON SDK** 를 사용할 수 있습니다. Python SDK를 사용 하려면 ...
   - PyPi: [Python 용 Azure Azure Digital Twins 핵심 클라이언트 라이브러리](https://pypi.org/project/azure-digitaltwins-core/)에서 패키지를 보고 설치할 수 있습니다.
   - [SDK 참조 설명서](/python/api/azure-digitaltwins-core/azure.digitaltwins.core)를 볼 수 있습니다.
   - GitHub에서 SDK 원본을 찾을 수 있습니다. [Python 용 Azure Azure Digital Twins 핵심 클라이언트 라이브러리](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/digitaltwins/azure-digitaltwins-core)
* AutoRest를 사용 하 여 다른 언어용 SDK를 생성할 수 있습니다. [*방법: AutoRest를 사용 하 여 Azure Digital Twins 용 사용자 지정 Sdk 만들기*](how-to-create-custom-sdks.md)의 지침을 따릅니다.

[CLI](how-to-use-cli.md)를 통해 Azure Digital twins와 상호 작용 하 여 날짜 평면 api를 연습할 수도 있습니다.

## <a name="net-c-sdk-data-plane"></a>.NET (c #) SDK (데이터 평면)

Azure Digital Twins .NET (c #) SDK는 Azure SDK for .NET의 일부입니다. 오픈 소스 이며 Azure Digital Twins 데이터 평면 Api를 기반으로 합니다.

> [!NOTE]
> SDK 디자인에 대 한 자세한 내용은 [Azure sdk에 대 한 일반적인 디자인 원칙과](https://azure.github.io/azure-sdk/general_introduction.html) 특정 [.net 디자인 지침](https://azure.github.io/azure-sdk/dotnet_introduction.html)을 참조 하세요.

SDK를 사용 하려면 NuGet 패키지 **DigitalTwins** 를 프로젝트에 포함 합니다. 최신 버전의 **Azure. id** 패키지도 필요 합니다. Visual Studio에서 nuget 패키지 관리자를 사용 하 여 이러한 패키지를 추가할 수 있습니다 ( *도구 > Nuget 패키지 관리자를 통해 액세스 > 솔루션에 대 한 Nuget 패키지 관리*). 또는 아래 NuGet 패키지 링크에 있는 명령과 함께 .NET 명령줄 도구를 사용 하 여 프로젝트에 추가할 수 있습니다.
* [**Azure.DigitalTwins.Core**](https://www.nuget.org/packages/Azure.DigitalTwins.Core). [.NET용 Azure Digital Twins SDK](/dotnet/api/overview/azure/digitaltwins/client)의 패키지입니다. 
* [**Azure.Identity**](https://www.nuget.org/packages/Azure.Identity). 이 라이브러리는 Azure에 대한 인증에 유용한 도구를 제공합니다.

실제로 Api를 사용 하는 방법에 대 한 자세한 연습은 [*자습서: 클라이언트 앱 코드*](tutorial-code.md)를 참조 하세요. 

### <a name="net-sdk-usage-examples"></a>.NET SDK 사용 예

.NET SDK 사용을 보여 주는 몇 가지 코드 샘플은 다음과 같습니다.

서비스에 대해 인증:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/authentication.cs" id="DefaultAzureCredential_basic":::

[!INCLUDE [Azure Digital Twins: local credentials note](../../includes/digital-twins-local-credentials-note.md)] 

모델 업로드:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/model_operations.cs" id="CreateModel":::

모델 나열:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/model_operations.cs" id="GetModels":::

Twins 만들기:

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwin_withHelper":::

쿼리 쌍을 쿼리하고 결과를 반복 합니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/queries.cs" id="FullQuerySample":::

자습서:이 샘플 앱 코드의 연습을 보려면 [*클라이언트 앱 코딩*](tutorial-code.md) 을 참조 하세요. 

[.Net (c #) SDK 용 GitHub 리포지토리](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core/samples)에서 추가 샘플을 찾을 수도 있습니다.

#### <a name="serialization-helpers"></a>Serialization 도우미

Serialization 도우미는 기본 정보에 대 한 액세스를 위해 쌍 데이터를 신속 하 게 만들거나 deserialize 하기 위해 SDK 내에서 사용할 수 있는 도우미 함수입니다. 핵심 SDK 메서드는 기본적으로 쌍으로 된 데이터를 JSON으로 반환 하므로 이러한 도우미 클래스를 사용 하 여 쌍 데이터를 더 아래로 분할 하는 것이 유용할 수 있습니다.

사용 가능한 도우미 클래스는 다음과 같습니다.
* `BasicDigitalTwin`: 일반적으로 디지털 쌍의 핵심 데이터를 나타냅니다.
* `BasicDigitalTwinComponent`: 일반적으로의 속성에서 구성 요소를 나타냅니다. `Contents``BasicDigitalTwin`
* `BasicRelationship`: 일반적으로 관계의 핵심 데이터를 나타냅니다.
* `DigitalTwinsJsonPropertyName`: JSON serialization에 사용 되는 문자열 상수와 사용자 지정 디지털 쌍 형식의 deserialization을 포함 합니다.

##### <a name="deserialize-a-digital-twin"></a>디지털 쌍 Deserialize

또는와 같이 선택한 JSON 라이브러리를 사용 하 여 항상 쌍 데이터를 deserialize 할 수 있습니다 `System.Text.Json` `Newtonsoft.Json` . 쌍에 대 한 기본 액세스의 경우 도우미 클래스를 사용 하면 편리 하 게 만들 수 있습니다.

`BasicDigitalTwin`도우미 클래스는를 통해 쌍에 정의 된 속성에 대 한 액세스도 제공 `Dictionary<string, object>` 합니다. 쌍의 속성을 나열 하려면 다음을 사용할 수 있습니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="GetTwin":::

> [!NOTE]
> `BasicDigitalTwin``System.Text.Json`특성을 사용 합니다. `BasicDigitalTwin` [DigitalTwinsClient](/dotnet/api/azure.digitaltwins.core.digitaltwinsclient?view=azure-dotnet&preserve-view=true)와 함께 사용 하려면 기본 생성자를 사용 하 여 클라이언트를 초기화 하거나 serializer 옵션을 사용자 지정 하려면 [JsonObjectSerializer](/dotnet/api/azure.core.serialization.jsonobjectserializer?view=azure-dotnet&preserve-view=true)를 사용 합니다.

##### <a name="create-a-digital-twin"></a>디지털 쌍 만들기

클래스를 사용 하 여 쌍 `BasicDigitalTwin` 인스턴스를 만들기 위한 데이터를 준비할 수 있습니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwin_withHelper":::

위의 코드는 다음 "수동" 변형과 동일 합니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="CreateTwin_noHelper":::

##### <a name="deserialize-a-relationship"></a>관계 Deserialize

언제 든 지 관계 데이터를 선택한 형식으로 deserialize 할 수 있습니다. 관계에 대 한 기본 액세스를 위해 형식을 사용 `BasicRelationship` 합니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_sample.cs" id="GetRelationshipsCall":::

`BasicRelationship`도우미 클래스는를 통해 관계에 정의 된 속성에 대 한 액세스도 제공 `IDictionary<string, object>` 합니다. 속성을 나열 하려면 다음을 사용할 수 있습니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_other.cs" id="ListRelationshipProperties":::

##### <a name="create-a-relationship"></a>관계 만들기

클래스를 사용 하 여 쌍으로 `BasicRelationship` 된 쌍 인스턴스에서 관계를 만들기 위해 데이터를 준비할 수도 있습니다.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_other.cs" id="CreateRelationship_short":::

##### <a name="create-a-patch-for-twin-update"></a>쌍 업데이트에 대 한 패치 만들기

쌍 및 관계에 대 한 Update 호출은 [JSON 패치](http://jsonpatch.com/) 구조를 사용 합니다. JSON 패치 작업 목록을 만들려면 아래와 같이를 사용할 수 있습니다 `JsonPatchDocument` .

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="UpdateTwin":::

## <a name="general-apisdk-usage-notes"></a>일반 API/SDK 사용 메모

> [!NOTE]
> Azure Digital Twins는 현재 **CORS (크로스-원본 자원 공유)** 를 지원 하지 않습니다. 영향 및 해결 방법에 대 한 자세한 내용은 *개념: Azure Digital Twins 솔루션에 대 한 보안* 의 [*CORS (크로스-원본 자원 공유)*](concepts-security.md#cross-origin-resource-sharing-cors) 섹션을 참조 하세요.

다음 목록에서는 Api 및 Sdk 사용에 대 한 추가 세부 정보 및 일반적인 지침을 제공 합니다.

* Postman과 같은 HTTP REST 테스트 도구를 사용 하 여 Azure Digital Twins Api에 대 한 직접 호출을 수행할 수 있습니다. 이 프로세스에 대 한 자세한 내용은 [*방법: Postman을 사용 하 여 요청 만들기*](how-to-use-postman.md)를 참조 하세요.
* SDK를 사용 하려면 클래스를 인스턴스화합니다 `DigitalTwinsClient` . 생성자에는 패키지의 다양 한 인증 방법을 사용 하 여 얻을 수 있는 자격 증명이 필요 합니다 `Azure.Identity` . 에 대 한 자세한 `Azure.Identity` 내용은 [네임 스페이스 설명서](/dotnet/api/azure.identity)를 참조 하세요. 
* `InteractiveBrowserCredential`시작 하는 동안 유용 하지만, Azure Digital Twins에 대해 [MSI로 설정 된 azure 함수](../app-service/overview-managed-identity.md?tabs=dotnet) 를 인증 하는 데 사용할 수 있는 [관리 되는 id](/dotnet/api/azure.identity.interactivebrowsercredential)에 대 한 자격 증명을 비롯 한 몇 가지 다른 옵션이 있습니다. 에 대 한 자세한 내용은 `InteractiveBrowserCredential` 해당 [클래스 설명서](/dotnet/api/azure.identity.interactivebrowsercredential)를 참조 하세요.
* 모든 서비스 API 호출은 클래스에서 멤버 함수로 노출 됩니다 `DigitalTwinsClient` .
* 모든 서비스 함수는 동기 및 비동기 버전에 존재 합니다.
* 모든 서비스 함수는 400 이상의 반환 상태에 대 한 예외를 throw 합니다. 호출 `try` 을 섹션으로 래핑하고 최소한의 catch를 수행 해야 `RequestFailedExceptions` 합니다. 이러한 형식의 예외에 대 한 자세한 내용은 [여기](/dotnet/api/azure.requestfailedexception)를 참조 하세요.
* 대부분의 서비스 메서드는 `Response<T>` 또는 ( `Task<Response<T>>` 비동기 호출의 경우)를 반환 `T` 합니다. 여기서은 서비스 호출에 대 한 반환 개체의 클래스입니다. [`Response`](/dotnet/api/azure.response-1)클래스는 서비스 반환을 캡슐화 하 고 해당 필드에 반환 값을 제공 합니다 `Value` .  
* 페이징 결과를 포함 하는 서비스 메서드 `Pageable<T>` `AsyncPageable<T>` 는 또는 결과로 반환 됩니다. 클래스에 대 한 자세한 내용은 `Pageable<T>` [여기](/dotnet/api/azure.pageable-1)를 참조 하십시오 .에 대 한 자세한 내용은 `AsyncPageable<T>` [여기](/dotnet/api/azure.asyncpageable-1)를 참조 하세요.
* 루프를 사용 하 여 페이징 결과를 반복할 수 있습니다 `await foreach` . 이 프로세스에 대 한 자세한 내용은 [여기](/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8)를 참조 하세요.
* 기본 SDK는 `Azure.Core` 입니다. SDK 인프라 및 유형에 대 한 참조는 [Azure 네임 스페이스 설명서](/dotnet/api/azure) 를 참조 하세요.

서비스 메서드는 가능한 경우 항상 강력한 형식의 개체를 반환 합니다. 그러나 Azure Digital Twins는 런타임에 사용자가 구성한 모델을 기반으로 하기 때문에 (서비스에 업로드 된 DTDL 모델을 통해) 많은 서비스 Api가 쌍 데이터를 JSON 형식으로 사용 하 고 반환 합니다.

## <a name="monitor-api-metrics"></a>API 메트릭 모니터링

요청, 대기 시간 및 실패율과 같은 API 메트릭은 [Azure Portal](https://portal.azure.com/)볼 수 있습니다. 

포털 홈페이지에서 Azure Digital Twins 인스턴스를 검색 하 여 세부 정보를 가져옵니다. Azure Digital Twins 인스턴스 메뉴에서 **메트릭** 옵션을 선택 하 여 *메트릭* 페이지를 표시 합니다.

:::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Azure Digital Twins의 메트릭 페이지를 보여 주는 스크린샷":::

여기에서 인스턴스에 대 한 메트릭을 보고 사용자 지정 보기를 만들 수 있습니다.

## <a name="next-steps"></a>다음 단계

Postman을 사용 하 여 Api에 대 한 직접 요청을 만드는 방법을 참조 하세요.
* [*방법: Postman을 사용 하 여 요청 만들기*](how-to-use-postman.md)

또는이 자습서를 사용 하 여 클라이언트 앱을 만들어 .NET SDK를 사용 하는 것이 좋습니다.
* [*자습서: 클라이언트 앱 코딩*](tutorial-code.md)