---
title: Azure Functions에서 함수 앱 설정 구성
description: Azure Functions에서 함수 앱 설정을 구성 하는 방법에 대해 알아봅니다.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 04/13/2020
ms.custom: cc996988-fb4f-47, devx-track-azurecli
ms.openlocfilehash: 5080d16a7b14506b24e07e2ee4ba862c645f83a8
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98875452"
---
# <a name="manage-your-function-app"></a>함수 앱 관리 

Azure Functions에서 함수 앱은 개별 함수에 대한 실행 컨텍스트를 제공합니다. 함수 앱 동작은 지정된 함수 앱에서 호스트하는 모든 함수에 적용됩니다. 함수 앱의 모든 함수는 동일한 [언어](supported-languages.md)여야 합니다. 

함수 앱의 개별 함수는 함께 배포 되며 함께 확장 됩니다. 동일한 함수 앱의 모든 함수는 함수 앱이 확장 될 때 인스턴스당 리소스를 공유 합니다. 

연결 문자열, 환경 변수 및 기타 응용 프로그램 설정은 각 함수 앱에 대해 개별적으로 정의 됩니다. 함수 앱 간에 공유 해야 하는 모든 데이터는 지속형 저장소에 외부적으로 저장 해야 합니다.

## <a name="get-started-in-the-azure-portal"></a>Azure Portal에서 시작

1. 시작하려면 [Azure Portal]로 이동한 후 Azure 계정으로 로그인합니다. 포털 맨 위에 있는 검색 표시줄에 함수 앱의 이름을 입력 하 고 목록에서 선택 합니다. 

2. 왼쪽 창의 **설정** 에서 **구성** 을 선택 합니다.

    :::image type="content" source="./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png" alt-text="Azure Portal의 함수 앱 개요":::

개요 페이지, 특히 **[응용 프로그램 설정](#settings)** 및 **[플랫폼 기능](#platform-features)** 에서 함수 앱을 관리 하는 데 필요한 모든 항목으로 이동할 수 있습니다.

## <a name="work-with-application-settings"></a><a name="settings"></a>응용 프로그램 설정 작업

[Azure Portal](functions-how-to-use-azure-function-app-settings.md?tabs=portal#settings) 에서 응용 프로그램 설정을 관리 하 고 [Azure CLI](functions-how-to-use-azure-function-app-settings.md?tabs=azurecli#settings) 및 [Azure PowerShell](functions-how-to-use-azure-function-app-settings.md?tabs=powershell#settings)를 사용할 수 있습니다. [Visual Studio Code](functions-develop-vs-code.md#application-settings-in-azure) 및 [Visual Studio](functions-develop-vs.md#function-app-settings)에서 응용 프로그램 설정을 관리할 수도 있습니다. 

이러한 설정은 암호화 되어 저장 됩니다. 자세히 알아보려면 [응용 프로그램 설정 보안](security-concepts.md#application-settings)을 참조 하세요.

# <a name="portal"></a>[포털](#tab/portal)

응용 프로그램 설정을 찾으려면 [Azure Portal에서 시작](#get-started-in-the-azure-portal)을 참조 하세요. 

**응용 프로그램 설정** 탭은 함수 앱에서 사용 하는 설정을 유지 합니다. 포털에서 값 **표시** 를 선택 하 여 값을 확인 해야 합니다. 포털에서 설정을 추가 하려면 **새 응용 프로그램 설정** 을 선택 하 고 새 키-값 쌍을 추가 합니다.

![Azure Portal의 함수 앱 설정입니다.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

[`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list)이 명령은 다음 예제와 같이 기존 응용 프로그램 설정을 반환 합니다.

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

[`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set)명령은 응용 프로그램 설정을 추가 하거나 업데이트 합니다. 다음 예에서는 라는 키와 값을 사용 하 여 설정을 만듭니다 `CUSTOM_FUNCTION_APP_SETTING` `12345` .


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

[`Get-AzFunctionAppSetting`](/powershell/module/az.functions/get-azfunctionappsetting)Cmdlet은 다음 예제와 같이 기존 응용 프로그램 설정을 반환 합니다. 

```azurepowershell-interactive
Get-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME>
```

[`Update-AzFunctionAppSetting`](/powershell/module/az.functions/update-azfunctionappsetting)명령은 응용 프로그램 설정을 추가 하거나 업데이트 합니다. 다음 예에서는 라는 키와 값을 사용 하 여 설정을 만듭니다 `CUSTOM_FUNCTION_APP_SETTING` `12345` .

```azurepowershell-interactive
Update-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME> -AppSetting @{"CUSTOM_FUNCTION_APP_SETTING" = "12345"}
```

---

### <a name="use-application-settings"></a>응용 프로그램 설정 사용

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

함수 앱을 로컬로 개발 하는 경우 프로젝트 파일의 local.settings.js에 이러한 값의 로컬 복사본을 유지 해야 합니다. 자세히 알아보려면 [로컬 설정 파일](functions-run-local.md#local-settings-file)을 참조 하세요.

## <a name="hosting-plan-type"></a>호스팅 계획 유형

함수 앱을 만들 때 앱이 실행 되는 호스팅 계획도 만들 수도 있습니다. 계획에는 하나 이상의 함수 앱이 있을 수 있습니다. 기능의 기능, 크기 조정 및 가격은 계획 유형에 따라 달라 집니다. 자세히 알아보려면 [Azure Functions 호스팅 옵션](functions-scale.md)을 참조 하세요.

Azure Portal에서 함수 앱에 사용 되는 계획의 유형을 결정 하거나 Azure CLI 또는 Azure PowerShell Api를 사용 하 여 확인할 수 있습니다. 

다음 값은 계획 유형을 표시 합니다.

| 플랜 유형 | 포털 | Azure CLI/PowerShell |
| --- | --- | --- |
| [Consumption](consumption-plan.md) | **Consumption** | `Dynamic` |
| [Premium](functions-premium-plan.md) | **ElasticPremium** | `ElasticPremium` |
| [전용 (App Service)](dedicated-plan.md) | 다양 | 다양 |

# <a name="portal"></a>[포털](#tab/portal)

함수 앱에서 사용 하는 계획 유형을 확인 하려면 [Azure Portal](https://portal.azure.com)에서 함수 앱에 대 한 **개요** 탭의 **App Service 계획** 을 참조 하세요. 가격 책정 계층을 보려면 **App Service 계획** 의 이름을 선택한 다음 왼쪽 창에서 **속성** 을 선택 합니다.

![포털에서 크기 조정 계획 보기](./media/functions-scale/function-app-overview-portal.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

다음 Azure CLI 명령을 실행 하 여 호스팅 계획 유형을 가져옵니다.

```azurecli-interactive
functionApp=<FUNCTION_APP_NAME>
resourceGroup=FunctionMonitoringExamples
appServicePlanId=$(az functionapp show --name $functionApp --resource-group $resourceGroup --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv

```  

이전 예제에서 및를 `<RESOURCE_GROUP>` `<FUNCTION_APP_NAME>` 리소스 그룹 및 함수 앱 이름으로 바꿉니다. 

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

다음 Azure PowerShell 명령을 실행 하 여 호스팅 계획 유형을 가져옵니다.

```azurepowershell-interactive
$FunctionApp = '<FUNCTION_APP_NAME>'
$ResourceGroup = '<RESOURCE_GROUP>'

$PlanID = (Get-AzFunctionApp -ResourceGroupName $ResourceGroup -Name $FunctionApp).AppServicePlan
(Get-AzFunctionAppPlan -Name $PlanID -ResourceGroupName $ResourceGroup).SkuTier
```
이전 예제에서 및를 `<RESOURCE_GROUP>` `<FUNCTION_APP_NAME>` 리소스 그룹 및 함수 앱 이름으로 바꿉니다. 

---

## <a name="plan-migration"></a>마이그레이션 계획

Azure CLI 명령을 사용 하 여 Windows의 소비 계획과 프리미엄 계획 간에 함수 앱을 마이그레이션할 수 있습니다. 특정 명령은 마이그레이션의 방향에 따라 달라 집니다. 전용 (App Service) 요금제에 대 한 직접 마이그레이션은 현재 지원 되지 않습니다.

이 마이그레이션은 Linux에서 지원 되지 않습니다.

### <a name="consumption-to-premium"></a>프리미엄에 대 한 사용량

다음 절차를 사용 하 여 소비 계획에서 Windows의 프리미엄 계획으로 마이그레이션할 수 있습니다.

1. 다음 명령을 실행 하 여 기존 함수 앱과 동일한 지역 및 리소스 그룹에 새 App Service 계획 (탄력적 프리미엄)을 만듭니다.  

    ```azurecli-interactive
    az functionapp plan create --name <NEW_PREMIUM_PLAN_NAME> --resource-group <MY_RESOURCE_GROUP> --location <REGION> --sku EP1
    ```

1. 다음 명령을 실행 하 여 기존 함수 앱을 새 프리미엄 계획으로 마이그레이션합니다.

    ```azurecli-interactive
    az functionapp update --name <MY_APP_NAME> --resource-group <MY_RESOURCE_GROUP> --plan <NEW_PREMIUM_PLAN>
    ```

1. 이전 소비 함수 앱 계획이 더 이상 필요 하지 않은 경우 새 함수로 성공적으로 마이그레이션 되었는지 확인 한 후 원래 함수 앱 계획을 삭제 합니다. 다음 명령을 실행 하 여 리소스 그룹의 모든 소비 계획 목록을 가져옵니다.

    ```azurecli-interactive
    az functionapp plan list --resource-group <MY_RESOURCE_GROUP> --query "[?sku.family=='Y'].{PlanName:name,Sites:numberOfSites}" -o table
    ```

    사이트를 0 개 포함 하는 계획을 안전 하 게 삭제할 수 있습니다. 여기서는 마이그레이션 한 것입니다.

1. 다음 명령을 실행 하 여에서 마이그레이션한 소비 계획을 삭제 합니다.

    ```azurecli-interactive
    az functionapp plan delete --name <CONSUMPTION_PLAN_NAME> --resource-group <MY_RESOURCE_GROUP>
    ```

### <a name="premium-to-consumption"></a>프리미엄에서 사용

다음 절차를 사용 하 여 프리미엄 계획에서 Windows의 소비 계획으로 마이그레이션할 수 있습니다.

1. 다음 명령을 실행 하 여 기존 함수 앱과 동일한 지역 및 리소스 그룹에 새 함수 앱 (소비)을 만듭니다. 또한이 명령은 함수 앱이 실행 되는 새 소비 계획을 만듭니다.

    ```azurecli-interactive
    az functionapp create --resource-group <MY_RESOURCE_GROUP> --name <NEW_CONSUMPTION_APP_NAME> --consumption-plan-location <REGION> --runtime dotnet --functions-version 3 --storage-account <STORAGE_NAME>
    ```

1. 다음 명령을 실행 하 여 기존 함수 앱을 새 소비 계획으로 마이그레이션합니다.

    ```azurecli-interactive
    az functionapp update --name <MY_APP_NAME> --resource-group <MY_RESOURCE_GROUP> --plan <NEW_CONSUMPTION_PLAN>
    ```

1. 기존 함수 앱을 실행 하기 위해 만든 계획만 필요 하므로 1 단계에서 만든 함수 앱을 삭제 합니다.

    ```azurecli-interactive
    az functionapp delete --name <NEW_CONSUMPTION_APP_NAME> --resource-group <MY_RESOURCE_GROUP>
    ```

1. 이전 프리미엄 함수 앱 계획이 더 이상 필요 하지 않은 경우 새 함수로 성공적으로 마이그레이션 되었는지 확인 한 후 원래 함수 앱 계획을 삭제 합니다. 계획을 삭제 하지 않으면 프리미엄 요금제에 대 한 요금이 계속 청구 됩니다. 다음 명령을 실행 하 여 리소스 그룹의 모든 프리미엄 계획 목록을 가져옵니다.

    ```azurecli-interactive
    az functionapp plan list --resource-group <MY_RESOURCE_GROUP> --query "[?sku.family=='EP'].{PlanName:name,Sites:numberOfSites}" -o table
    ```

1. 다음 명령을 실행 하 여에서 마이그레이션한 프리미엄 요금제를 삭제 합니다.

    ```azurecli-interactive
    az functionapp plan delete --name <PREMIUM_PLAN> --resource-group <MY_RESOURCE_GROUP>
    ```

## <a name="platform-features"></a>플랫폼 기능

함수 앱은에서 실행 되며 Azure App Service 플랫폼에서 유지 관리 됩니다. 따라서 함수 앱은 Azure의 핵심 웹 호스팅 플랫폼 기능 대부분에 액세스할 수 있습니다. 왼쪽 창에는 함수 앱에서 사용할 수 있는 App Service 플랫폼의 다양 한 기능에 액세스할 수 있습니다. 

> [!NOTE]
> 함수 앱이 소비 호스팅 계획에서 실행될 때 모든 App Service 기능을 사용할 수 있는 것은 아닙니다.

이 문서의 나머지 부분에서는 함수에 유용한 Azure Portal의 다음 App Service 기능에 대해 집중적으로 설명 합니다.

+ [App Service 편집기](#editor)
+ [콘솔](#console)
+ [고급 도구(Kudu)](#kudu)
+ [배포 옵션](#deployment)
+ [CORS](#cors)
+ [인증](#auth)

App Service 설정을 사용하는 방법에 대한 자세한 내용은 [Azure App Service 설정 구성](../app-service/configure-common.md)을 참조하세요.

### <a name="app-service-editor"></a><a name="editor"></a>App Service 편집기

![App Service 편집기](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

App Service 편집기는 JSON 구성 파일과 코드 파일을 둘 다 수정하는 데 사용할 수 있는 포털 내 고급 편집기입니다. 이 옵션을 선택하면 기본 편집기와 함께 별도의 브라우저 탭이 실행됩니다. 이를 통해 Git 리포지토리와 통합하고 코드를 실행 및 디버깅하며 함수 앱 설정을 수정할 수 있습니다. 이 편집기는 기본 제공 함수 편집기와 비교 하 여 함수에 대 한 향상 된 개발 환경을 제공 합니다.  

로컬 컴퓨터에서 함수를 개발 하는 것이 좋습니다. 로컬로 개발 하 고 Azure에 게시 하는 경우 프로젝트 파일은 포털에서 읽기 전용입니다. 자세히 알아보려면 [로컬에서 코드 및 테스트 Azure Functions](functions-develop-local.md)를 참조 하세요.

### <a name="console"></a><a name="console"></a>콘솔

![함수 앱 콘솔](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

포털 내 콘솔은 명령줄에서 함수 앱과 상호 작용하려는 경우에 이상적인 개발자 도구입니다. 일반적인 명령에는 배치 파일 및 스크립트 실행과 함께 디렉터리 및 파일의 생성 및 탐색이 포함됩니다. 

로컬로 개발 하는 경우 [Azure Functions Core Tools](functions-run-local.md) 및 [Azure CLI]를 사용 하는 것이 좋습니다.

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>고급 도구(Kudu)

![Kudu 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

App Service용 고급 도구(Kudu라고도 함)를 사용하면 함수 앱의 고급 관리 기능에 액세스할 수 있습니다. Kudu에서 시스템 정보, 앱 설정, 환경 변수, 사이트 확장, HTTP 헤더 및 서버 변수를 관리할 수 있습니다. `https://<myfunctionapp>.scm.azurewebsites.net/`과 같은 함수 앱에 대한 SCM 엔드포인트로 이동하여 **Kudu** 를 시작할 수도 있습니다. 


### <a name="deployment-center"></a><a name="deployment"></a>배포 센터

소스 제어 솔루션을 사용 하 여 함수 코드를 개발 하 고 유지 관리 하는 경우 Deployment Center를 사용 하 여 소스 제어에서 빌드 및 배포할 수 있습니다. 업데이트를 수행할 때 프로젝트가 빌드되고 Azure에 배포 됩니다. 자세한 내용은 [Azure Functions 배포 기술](functions-deployment-technologies.md)(영문)을 참조 하세요.

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>크로스-원본 자원 공유

클라이언트에서 악의적인 코드가 실행 되는 것을 방지 하기 위해 최신 브라우저는 웹 응용 프로그램의 요청을 별도의 도메인에서 실행 되는 리소스로 차단 합니다. [CORS (원본 간 리소스 공유)](https://developer.mozilla.org/docs/Web/HTTP/CORS) 를 사용 하면 `Access-Control-Allow-Origin` 헤더에서 함수 앱의 끝점을 호출할 수 있는 원본을 선언할 수 있습니다.

#### <a name="portal"></a>포털

함수 앱에 대해 **허용 된 원본** 목록을 구성할 때 `Access-Control-Allow-Origin` 헤더는 함수 앱의 HTTP 끝점에서 모든 응답에 자동으로 추가 됩니다. 

![함수 앱의 CORS 목록 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

와일드 카드 ( `*` )를 사용 하면 다른 모든 도메인은 무시 됩니다. 

명령을 사용 [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add) 하 여 허용 된 원본 목록에 도메인을 추가 합니다. 다음 예에서는 contoso.com 도메인을 추가 합니다.

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

명령을 사용 [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show) 하 여 현재 허용 된 원본을 나열 합니다.

### <a name="authentication"></a><a name="auth"></a>인증

![함수 앱에 대한 인증 구성](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

함수가 HTTP 트리거를 사용하는 경우 먼저 호출이 인증되도록 요구할 수 있습니다. App Service는 Facebook, Microsoft 및 Twitter와 같은 소셜 공급자를 사용 하 여 Azure Active Directory 인증 및 로그인을 지원 합니다. 특정 인증 공급자를 구성하는 방법에 대한 자세한 내용은 [Azure App Service 인증 개요](../app-service/overview-authentication-authorization.md)를 참조하세요. 


## <a name="next-steps"></a>다음 단계

+ [Azure App Service 설정 구성](../app-service/configure-common.md)
+ [Azure Functions에 대한 연속 배포](functions-continuous-deployment.md)

[Azure CLI]: /cli/azure/
[Azure Portal]: https://portal.azure.com
