---
title: Azure Functions에 대한 앱 설정 참조
description: Azure Functions 앱 설정 또는 환경 변수에 대한 참조 설명서입니다.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/22/2018
ms.author: glenga
ms.openlocfilehash: 3aa3176b1d6d9e5665fd3a8988b71159a4fc20c0
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68735704"
---
# <a name="app-settings-reference-for-azure-functions"></a>Azure Functions에 대한 앱 설정 참조

함수 앱의 앱 설정에는 해당 함수 앱의 모든 함수에 영향을 주는 전역 구성 옵션이 포함됩니다. 로컬에서 실행할 때 이러한 설정은 로컬 [환경 변수](functions-run-local.md#local-settings-file)로 액세스합니다. 이 문서에는 함수 앱에서 사용할 수 있는 앱 설정이 나열되어 있습니다.

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

[host.json](functions-host-json.md) 파일과 [local.settings.json](functions-run-local.md#local-settings-file) 파일에는 다른 전역 구성 옵션이 있습니다.

## <a name="appinsights_instrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

Application Insights를 사용하는 경우 Application Insights 계측 키입니다. [Azure Functions 모니터링](functions-monitoring.md)을 참조하세요.

|Key|샘플 값|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azure_functions_environment"></a>AZURE_FUNCTIONS_ENVIRONMENT

함수 런타임의 버전 2.x에서는 런타임 환경에 따라 앱 동작을 구성 합니다. [초기화 하는 동안](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/Program.cs#L43)이 값을 읽습니다. 는 임의의 값 `AZURE_FUNCTIONS_ENVIRONMENT` 으로 설정할 수 있지만 다음과 같은 [세 가지 값](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) 이 지원 됩니다. [개발](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [스테이징](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging)및 [프로덕션](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). 가 `AZURE_FUNCTIONS_ENVIRONMENT` 설정 되지 않은 경우 로컬 환경 `Development` 및 `Production` Azure에서 기본적으로로 설정 됩니다. 런타임 환경을 설정 `ASPNETCORE_ENVIRONMENT` 하는 대신이 설정을 사용 해야 합니다. 

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

로그를 저장하고 포털의 **모니터** 탭에 표시하기 위한 선택적인 스토리지 계정 연결 문자열입니다. 저장소 계정은 Blob, 큐 및 테이블을 지원하는 범용 계정이어야 합니다. [저장소 계정](functions-infrastructure-as-code.md#storage-account) 및 [저장소 계정 요구 사항](functions-create-function-app-portal.md#storage-account-requirements)을 참조하세요.

|Key|샘플 값|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

> [!TIP]
> 성능 및 환경을 위해 AzureWebJobsDashboard 대신 모니터링을 위해 App Insights 및 APPINSIGHTS_INSTRUMENTATIONKEY를 사용하는 것이 좋습니다.

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true`는 함수 앱의 루트 URL에 표시되는 기본 방문 페이지를 사용하지 않도록 설정한다는 의미입니다. 기본값은 `false`입니다.

|Key|샘플 값|
|---|------------|
|AzureWebJobsDisableHomepage|true|

이 앱 설정이 생략되거나 `false`로 설정되면 URL `<functionappname>.azurewebsites.net`에 대한 응답으로 다음 예와 유사한 페이지가 표시됩니다.

![함수 앱 방문 페이지](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true`는 .NET 코드를 컴파일할 때 릴리스 모드 사용을 의미합니다. `false`는 디버그 모드 사용을 의미합니다. 기본값은 `true`입니다.

|Key|샘플 값|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|true|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

사용하도록 설정할 쉼표로 구분된 베타 기능 목록입니다. 이 플래그로 사용할 수 있는 베타 기능은 프로덕션 준비가 되지 않았지만 실제로 사용하기 전에 실험적 용도로 사용할 수 있습니다.

|Key|샘플 값|
|---|------------|
|AzureWebJobsFeatureFlags|feature1,feature2|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

키 저장소에 사용할 리포지토리 또는 공급자를 지정합니다. 현재 지원되는 리포지토리는 Blob Storage(&quot;Blob&quot;) 및 로컬 파일 시스템(&quot;Files&quot;)입니다. 기본값은 버전 2에서는 Blob, 버전 1에서는 파일 시스템입니다.

|Key|샘플 값|
|---|------------|
|AzureWebJobsSecretStorageType|파일|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

Azure Functions 런타임은 HTTP 트리거 함수를 제외한 모든 함수에 대해 이 저장소 계정 연결 문자열을 사용합니다. 저장소 계정은 Blob, 큐 및 테이블을 지원하는 범용 계정이어야 합니다. [저장소 계정](functions-infrastructure-as-code.md#storage-account) 및 [저장소 계정 요구 사항](functions-create-function-app-portal.md#storage-account-requirements)을 참조하세요.

|Key|샘플 값|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

## <a name="azurewebjobs_typescriptpath"></a>AzureWebJobs_TypeScriptPath

TypeScript에 사용되는 컴파일러의 경로입니다. 필요한 경우 기본값을 재정의할 수 있습니다.

|Key|샘플 값|
|---|------------|
|AzureWebJobs_TypeScriptPath|%HOME%\typescript|

## <a name="function_app_edit_mode"></a>FUNCTION\_APP\_EDIT\_MODE

Azure Portal에서 편집할 수 있는지 여부를 지정 합니다. 유효한 값은 "readwrite"및 "readonly"입니다.

|Key|샘플 값|
|---|------------|
|FUNCTION\_APP\_EDIT\_MODE|readonly|

## <a name="functions_extension_version"></a>FUNCTIONS\_EXTENSION\_VERSION

이 함수 앱에 사용할 Functions 런타임의 버전입니다. 주 버전의 물결표는 해당 주 버전의 최신 버전(예: "~2")을 사용한다는 의미입니다. 동일한 주 버전의 새 버전을 사용할 수 있으면 함수 앱에 자동으로 설치됩니다. 앱을 특정 버전으로 고정하려면 전체 버전 번호(예: "2.0.12345")를 사용합니다. 기본값은 "~2"입니다. 값이 `~1`이면 앱을 런타임의 버전 1.x로 고정합니다.

|Key|샘플 값|
|---|------------|
|FUNCTIONS\_EXTENSION\_VERSION|~2|

## <a name="functions_worker_process_count"></a>함수\_작업자\_프로세스수\_

기본값을 사용 하 여 언어 작업자 프로세스의 최대 수를 지정 합니다 `1`. 허용 되는 최대 값 `10`은입니다. 함수 호출은 언어 작업자 프로세스 간에 균등 하 게 분산 됩니다. 언어 작업자 프로세스는 함수\_작업자\_프로세스\_수로 설정 된 개수에 도달할 때까지 10 초 마다 생성 됩니다. 여러 언어 작업자 프로세스를 사용 하는 것은 [크기 조정과](functions-scale.md)동일 하지 않습니다. 작업에 CPU 바인딩과 i/o 바인딩된 호출이 혼합 되어 있는 경우이 설정을 사용 하는 것이 좋습니다. 이 설정은 모든 non-.NET 언어에 적용 됩니다.

|Key|샘플 값|
|---|------------|
|함수\_작업자\_프로세스수\_|2|


## <a name="functions_worker_runtime"></a>FUNCTIONS\_WORKER\_RUNTIME

함수 앱에 로드할 언어 작업자 런타임입니다.  이것은 애플리케이션(예: "dotnet")에 사용되는 언어에 해당합니다. 여러 언어로 된 함수는 여러 개의 앱을 각각 해당하는 작업자 런타임 값을 사용하여 게시해야 합니다.  유효한 값은 `dotnet` (C#/F#), `node` (JavaScript/TypeScript), `java` (Java), `powershell` (PowerShell) 및 `python` (Python)입니다.

|Key|샘플 값|
|---|------------|
|FUNCTIONS\_WORKER\_RUNTIME|dotnet|

## <a name="website_contentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

소비 & 프리미엄 요금제에만 해당 합니다. 함수 앱 코드 및 구성이 저장된 저장소 계정의 연결 문자열입니다. [함수 앱 만들기](functions-infrastructure-as-code.md#create-a-function-app)를 참조하세요.

|Key|샘플 값|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol=https;AccountName=[name];AccountKey=[key]|

## <a name="website_contentshare"></a>WEBSITE\_CONTENTSHARE

소비 & 프리미엄 요금제에만 해당 합니다. 함수 앱 코드 및 구성에 대한 파일 경로입니다. Used with WEBSITE_CONTENTAZUREFILECONNECTIONSTRING. 기본값은 함수 앱 이름으로 시작하는 고유한 문자열입니다. [함수 앱 만들기](functions-infrastructure-as-code.md#create-a-function-app)를 참조하세요.

|Key|샘플 값|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="website_max_dynamic_application_scale_out"></a>WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT

함수 앱이 확장할 수 있는 최대 인스턴스 수입니다. 기본값은 무제한입니다.

> [!NOTE]
> 이 설정은 미리 보기 기능이며 값이 <= 5로 설정된 경우에만 신뢰할 수 있습니다.

|Key|샘플 값|
|---|------------|
|WEBSITE\_MAX\_DYNAMIC\_APPLICATION\_SCALE\_OUT|5|

## <a name="website_node_default_version"></a>WEBSITE\_NODE\_DEFAULT_VERSION

기본값은 "8.11.1"입니다.

|Key|샘플 값|
|---|------------|
|WEBSITE\_NODE\_DEFAULT_VERSION|8.11.1|

## <a name="website_run_from_package"></a>WEBSITE\_RUN\_FROM\_PACKAGE

탑재된 패키지 파일에서 함수 앱을 실행하도록 설정합니다.

|Key|샘플 값|
|---|------------|
|WEBSITE\_RUN\_FROM\_PACKAGE|1|

유효한 값은 배포 패키지 파일의 위치를 확인하는 URL이거나 `1`입니다. `1`로 설정하면 패키지는 `d:\home\data\SitePackages` 폴더에 있어야 합니다. 이 설정으로 zip 배포를 사용하면 패키지가 자동으로 이 위치에 업로드됩니다. 미리 보기에서 이 설정은 `WEBSITE_RUN_FROM_ZIP`으로 명명되었습니다. 자세한 내용은 [패키지 파일에서 Functions 실행](run-functions-from-deployment-package.md)을 참조하세요.

## <a name="azure_function_proxy_disable_local_call"></a>AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL

기본적으로 Functions 프록시는 바로 가기를 사용하여 새 HTTP 요청을 생성하는 대신 프록시에서 동일한 함수 앱의 함수로 API 호출을 직접 보냅니다. 이 설정을 사용하면 해당 동작을 사용하지 않도록 설정할 수 있습니다.

|Key|값|Description|
|-|-|-|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|true|로컬 함수 앱에서 함수를 가리키는 백 엔드 URL을 사용하는 호출은 함수로 더 이상 전송되지 않으며, 대신 함수 앱에 대한 HTTP 프런트 엔드에 다시 전달됩니다.|
|AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL|false|기본값입니다. 로컬 함수 앱에서 함수를 가리키는 백 엔드 URL을 사용하는 호출은 해당 함수로 직접 전달됩니다.|


## <a name="azure_function_proxy_backend_url_decode_slashes"></a>AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES

이 설정은 백 엔드 URL로 삽입될 때 %2F가 경로 매개 변수에서 슬래시로 디코딩되는지 여부를 제어합니다. 

|Key|값|Description|
|-|-|-|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|true|인코딩된 슬래시를 사용하는 경로 매개 변수는 해당 항목을 디코딩합니다. `example.com/api%2ftest`는 `example.com/api/test`가 됩니다.|
|AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES|false|이는 기본 동작입니다. 모든 경로 매개 변수는 변경 안됨으로 전달됩니다.|

### <a name="example"></a>예제

다음은 URL myfunction.com에서 함수 앱의 예제 proxies.json입니다.

```JSON
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "root": {
            "matchCondition": {
                "route": "/{*all}"
            },
            "backendUri": "example.com/{all}"
        }
    }
}
```
|URL 디코딩|입력|출력|
|-|-|-|
|true|myfunction.com/test%2fapi|example.com/test/api
|false|myfunction.com/test%2fapi|example.com/test%2fapi|


## <a name="next-steps"></a>다음 단계

[앱 설정 업데이트 방법 알아보기](functions-how-to-use-azure-function-app-settings.md#settings)

[host.json 파일의 전역 설정 보기](functions-host-json.md)

[App Service 앱에 대한 다른 앱 설정 보기](https://github.com/projectkudu/kudu/wiki/Configurable-settings)
