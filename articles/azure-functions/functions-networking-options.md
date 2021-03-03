---
title: Azure Functions 네트워킹 옵션
description: Azure Functions에서 사용할 수 있는 모든 네트워킹 옵션에 대한 개요입니다.
author: cachai2
ms.topic: conceptual
ms.date: 1/21/2021
ms.author: cachai
ms.openlocfilehash: ceef827f7406f8915d205349372a43626c917e4b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101729235"
---
# <a name="azure-functions-networking-options"></a>Azure Functions 네트워킹 옵션

이 문서에서는 Azure Functions에 대한 호스팅 옵션에서 사용할 수 있는 네트워킹 기능을 설명합니다. 다음 네트워킹 옵션을 통해 인터넷 라우팅 가능한 주소를 사용하지 않고 리소스에 액세스하거나 함수 앱에 대한 인터넷 액세스를 제한할 수 있습니다.

호스팅 모델은 서로 다른 수준의 네트워크 격리를 사용할 수 있습니다. 올바른 격리를 선택하면 네트워크 격리 요구 사항을 충족할 수 있습니다.

다음 두 가지 방법으로 함수 앱을 호스트할 수 있습니다.

* 다양한 수준의 가상 네트워크 연결 및 확장 옵션을 사용하여 다중 테넌트 인프라에서 실행되는 요금제 옵션 중에서 선택할 수 있습니다.
    * [사용 계획](consumption-plan.md)은 부하에 대응하여 동적으로 확장되고 최소한의 네트워크 격리 옵션을 제공합니다.
    * [프리미엄 계획](functions-premium-plan.md)은 동적으로 확장되고 보다 포괄적인 네트워크 격리를 제공합니다.
    * Azure [App Service 계획](dedicated-plan.md)은 고정된 규모에서 작동하며 프리미엄 계획과 비슷한 네트워크 격리를 제공합니다.
* [App Service Environment](../app-service/environment/intro.md)에서 함수를 실행할 수 있습니다. 이 메서드는 가상 네트워크에 함수를 배포하고 전체 네트워크 제어 및 격리를 제공합니다.

## <a name="matrix-of-networking-features"></a>네트워킹 기능 매트릭스

[!INCLUDE [functions-networking-features](../../includes/functions-networking-features.md)]

## <a name="inbound-access-restrictions"></a>인바운드 액세스 제한

액세스 제한을 사용 하 여 앱에 대 한 액세스를 허용 하거나 거부 하는 우선 순위가 지정 된 IP 주소 목록을 정의할 수 있습니다. 이 목록에는 IPv4 및 IPv6 주소 또는 [서비스 끝점](#use-service-endpoints)을 사용 하는 특정 가상 네트워크 서브넷이 포함 될 수 있습니다. 하나 이상의 항목이 있는 경우 목록 끝에 암시적 "모두 거부"가 표시됩니다. IP 제한은 모든 함수 호스팅 옵션에서 작동합니다.

액세스 제한은 [프리미엄](functions-premium-plan.md), [소비](consumption-plan.md)및 [App Service](dedicated-plan.md)에서 사용할 수 있습니다.

> [!NOTE]
> 네트워크 제한이 적용 된 상태에서, 가상 네트워크 내 에서만 배포 하거나, 사용 중인 컴퓨터의 IP 주소를 안전 받는 사람 목록에 Azure Portal 액세스 하는 데 사용 하는 컴퓨터의 IP 주소를 배치할 수 있습니다. 그러나 포털을 사용 하 여 함수를 관리할 수는 있습니다.

자세히 알아보려면 [Azure App Service 정적 액세스 제한](../app-service/app-service-ip-restrictions.md)을 참조하세요.

### <a name="use-service-endpoints"></a>서비스 엔드포인트 사용

서비스 끝점을 사용 하 여 선택한 Azure virtual network 서브넷에 대 한 액세스를 제한할 수 있습니다. 특정 서브넷에 대 한 액세스를 제한 하려면 **Virtual Network** 유형을 사용 하 여 제한 규칙을 만듭니다. 그런 다음 액세스를 허용 하거나 거부할 구독, 가상 네트워크 및 서브넷을 선택할 수 있습니다. 

선택한 서브넷에 대해 서비스 끝점이 아직 Microsoft 웹에서 사용 하도록 설정 되지 않은 경우 **누락 된 microsoft. 웹 서비스 끝점 무시** 확인란을 선택 하지 않으면 자동으로 사용 하도록 설정 됩니다. 앱에서 서비스 끝점을 사용 하도록 설정 하 고 서브넷은 사용 하지 않을 수 있는 시나리오는 주로 서브넷에서 사용 하도록 설정할 수 있는 권한이 있는지 여부에 따라 달라 집니다. 

다른 사용자가 서브넷에서 서비스 끝점을 사용 하도록 설정 해야 하는 경우 **누락 된 Microsoft 웹 서비스 끝점 무시** 확인란을 선택 합니다. 앱은 나중에 서브넷에서 사용 하도록 설정 하는 것을 예측 하 여 서비스 끝점에 대해 구성 됩니다. 

![Virtual Network 유형이 선택 된 "IP 제한 추가" 창의 스크린샷](../app-service/media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

서비스 끝점을 사용 하 여 App Service Environment에서 실행 되는 앱에 대 한 액세스를 제한할 수 없습니다. 앱이 App Service Environment에 있는 경우 IP 액세스 규칙을 적용 하 여 해당 앱에 대 한 액세스를 제어할 수 있습니다. 

서비스 끝점을 설정 하는 방법을 알아보려면 [Azure Functions 개인 사이트 액세스](functions-create-private-site-access.md)설정을 참조 하세요.

## <a name="private-endpoint-connections"></a>개인 끝점 연결

[!INCLUDE [functions-private-site-access](../../includes/functions-private-site-access.md)]

저장소 또는 service bus와 같은 개인 끝점 연결이 있는 다른 서비스를 호출 하려면 [전용 끝점에 대 한 아웃 바운드 호출](#private-endpoints)을 수행 하도록 앱을 구성 해야 합니다.

## <a name="virtual-network-integration"></a>가상 네트워크 통합

가상 네트워크 통합을 사용하면 함수 앱이 가상 네트워크 내의 리소스에 액세스할 수 있습니다.
Azure Functions는 두 종류의 가상 네트워크 통합을 지원합니다.

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Azure Functions의 가상 네트워크 통합은 App Service 웹 앱에 공유 인프라를 사용합니다. 가상 네트워크 통합의 두 유형을 자세히 알아보려면 다음을 참조하세요.

* [지역 가상 네트워크 통합](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [게이트웨이 필요 가상 네트워크 통합](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

가상 네트워크 통합을 설정하는 방법에 대한 자세한 내용은 [Azure 가상 네트워크에 함수 앱 통합](functions-create-vnet.md)을 참조하세요.

## <a name="regional-virtual-network-integration"></a>지역 가상 네트워크 통합

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connect-to-service-endpoint-secured-resources"></a>서비스 엔드포인트 보안 리소스에 연결

높은 수준의 보안을 제공하기 위해 서비스 엔드포인트를 사용하여 여러 Azure 서비스를 가상 네트워크로 제한할 수 있습니다. 그런 다음 해당 가상 네트워크와 함수 앱을 통합하여 리소스에 액세스해야 합니다. 이 구성은 가상 네트워크 통합을 지원하는 모든 계획에서 지원됩니다.

자세한 내용은 [가상 네트워크 서비스 엔드포인트](../virtual-network/virtual-network-service-endpoints-overview.md)를 참조하세요.

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>가상 네트워크에 대한 스토리지 계정 제한 

함수 앱을 만들 때 Blob, 큐 및 Table Storage을 지원하는 범용 Azure Storage 계정을 만들거나 연결해야 합니다.  이 저장소 계정은 서비스 끝점이 나 개인 끝점으로 보안이 유지 되는 계정으로 바꿀 수 있습니다.  이 기능은 현재 Standard 및 Premium을 포함 하는 모든 Vnet 지원 sku에 대해서만 작동 합니다. 단,이 경우에는 Vnet을 Premium sku에만 사용할 수 있는 플렉스 스탬프를 제외 합니다. 개인 네트워크로 제한 된 저장소 계정을 사용 하 여 함수를 설정 하려면 다음을 수행 합니다.

1. 서비스 끝점이 사용 하도록 설정 되지 않은 저장소 계정을 사용 하 여 함수를 만듭니다.
1. 가상 네트워크에 연결 하도록 함수를 구성 합니다.
1. 다른 저장소 계정을 만들거나 구성 합니다.  서비스 끝점을 사용 하 여 보안을 설정 하 고 함수를 연결 하는 저장소 계정입니다.
1. 보안 저장소 계정에서 [파일 공유를 만듭니다](../storage/files/storage-how-to-create-file-share.md#create-file-share) .
1. 저장소 계정에 대 한 서비스 끝점 또는 개인 끝점을 사용 하도록 설정 합니다.  
    * 개인 끝점 연결을 사용 하는 경우 저장소 계정에는 및 하위 리소스에 대 한 개인 끝점이 필요 합니다 `file` `blob` .  Durable Functions와 같은 특정 기능을 사용 하는 경우 `queue` `table` 개인 끝점 연결을 통해 액세스 하 고 액세스할 수도 있습니다.
    * 서비스 끝점을 사용 하는 경우 저장소 계정에 대 한 함수 앱 전용 서브넷을 사용 하도록 설정 합니다.
1. 필드 함수 앱 저장소 계정에서 보안 저장소 계정 및 파일 공유로 파일 및 blob 콘텐츠를 복사 합니다.
1. 이 저장소 계정에 대 한 연결 문자열을 복사 합니다.
1. 함수 앱에 대 한 **구성** 아래의 **응용 프로그램 설정을** 다음으로 업데이트 합니다.
    - `AzureWebJobsStorage` 보안 저장소 계정에 대 한 연결 문자열입니다.
    - `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` 보안 저장소 계정에 대 한 연결 문자열입니다.
    - `WEBSITE_CONTENTSHARE` 보안 저장소 계정에 생성 된 파일 공유의 이름입니다.
    - 의 이름과 값을 사용 하 여 새 설정을 만듭니다 `WEBSITE_CONTENTOVERVNET` `1` .
    - 저장소 계정이 개인 끝점 연결을 사용 하는 경우 다음 설정을 확인 하거나 추가 합니다.
        - `WEBSITE_VNET_ROUTE_ALL` 값을 가진 `1` 입니다.
        - `WEBSITE_DNS_SERVER` 값이 `168.63.129.16` 
1. 응용 프로그램 설정을 저장 합니다.  

함수 앱이 다시 시작 되 고 이제 보안 저장소 계정에 연결 됩니다.

## <a name="use-key-vault-references"></a>Key Vault 참조 사용

Azure Key Vault 참조를 사용하여 코드 변경 없이도 Azure Functions 애플리케이션에서 Azure Key Vault의 비밀을 사용할 수 있습니다. Azure Key Vault는 액세스 정책 및 감사 기록에 대한 전체 제어와 함께 중앙 집중식 비밀 관리를 제공하는 서비스입니다.

현재 서비스 엔드포인트로 Ket Vault가 보호되지 않는 경우 [Key Vault 참조](../app-service/app-service-key-vault-references.md)가 작동하지 않습니다. 가상 네트워크 통합을 사용하여 Key Vault에 연결하려면 애플리케이션 코드에서 Key Vault를 호출해야 합니다.

## <a name="virtual-network-triggers-non-http"></a>가상 네트워크 트리거(비 HTTP)

현재 다음 두 가지 방법 중 하나로 가상 네트워크 내에서 비 HTTP 트리거 함수를 사용할 수 있습니다.

+ 프리미엄 계획에서 함수 앱을 실행하고 가상 네트워크 트리거 지원을 사용 설정합니다.
+ App Service 계획 또는 App Service Environment에서 함수 앱을 실행합니다.

### <a name="premium-plan-with-virtual-network-triggers"></a>가상 네트워크 트리거를 사용하는 프리미엄 계획

프리미엄 계획을 실행하는 경우 비 HTTP 트리거 함수를 가상 네트워크 내에서 실행되는 서비스에 연결할 수 있습니다. 이렇게 하려면 함수 앱에 대한 가상 네트워크 트리거 지원을 사용하도록 설정해야 합니다. **런타임 규모 모니터링** 설정은 **구성** [](https://portal.azure.com)  >  **함수 런타임 설정** 아래 Azure Portal에 있습니다.

:::image type="content" source="media/functions-networking-options/virtual-network-trigger-toggle.png" alt-text="VNETToggle":::

다음 Azure CLI 명령을 사용하여 가상 네트워크 트리거를 사용하도록 설정할 수도 있습니다.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

> [!TIP]
> 가상 네트워크 트리거를 사용 하도록 설정 하면 응용 프로그램 성능에 영향을 줄 수 있습니다. App Service 계획 인스턴스는 크기를 조정 하는 시기를 결정 하기 위해 트리거를 모니터링 해야 하기 때문입니다. 이러한 영향은 매우 작을 가능성이 높습니다.

가상 네트워크 트리거는 Functions 런타임의 버전 2.x 이상에서 지원됩니다. 다음과 같은 비 HTTP 트리거 형식이 지원됩니다.

| 내선 번호 | 최소 버전 |
|-----------|---------| 
|[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 이상 |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 이상|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 이상|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 이상|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 이상|

> [!IMPORTANT]
> 가상 네트워크 트리거 지원을 사용하도록 설정하면 이전 표에 표시된 트리거 유형만 애플리케이션을 사용하여 동적으로 확장됩니다. 표에 없는 트리거는 계속 사용할 수 있지만 예열된 인스턴스 수 이상으로 크기가 조정되지 않습니다. 트리거의 전체 목록은 [트리거 및 바인딩](./functions-triggers-bindings.md#supported-bindings)을 참조하세요.

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>가상 네트워크 트리거를 사용하는 App Service 계획 및 App Service Environment

함수 앱이 App Service 계획 또는 App Service Environment에서 실행되는 경우 비 HTTP 트리거 함수를 사용할 수 있습니다. 함수가 올바르게 트리거되려면 트리거 연결에 정의된 리소스에 대한 액세스 권한이 있는 가상 네트워크에 연결되어야 합니다.

예를 들어 가상 네트워크의 트래픽만 허용하도록 Azure Cosmos DB를 구성하고자 한다고 가정하겠습니다. 이 경우 해당 가상 네트워크에 가상 네트워크 통합을 제공하는 App Service 계획에 함수 앱을 배포해야 합니다. 통합을 사용하면 해당 Azure Cosmos DB 리소스에서 함수를 트리거할 수 있습니다.

## <a name="hybrid-connections"></a>하이브리드 연결

[하이브리드 연결](../azure-relay/relay-hybrid-connections-protocol.md)은 다른 네트워크의 애플리케이션에 액세스하는 데 사용할 수 있는 Azure Relay의 기능입니다. 이를 통해 앱에서 애플리케이션 엔드포인트에 액세스할 수 있습니다. 이를 사용하여 애플리케이션 액세스할 수 없습니다. 하이브리드 연결은 사용 계획을 제외한 Windows에서 실행되는 함수에서 사용할 수 있습니다.

Azure Functions에서 사용되는 것처럼 각 하이브리드 연결은 단일 TCP 호스트 및 포트 조합에 상호 연결됩니다. 즉, TCP 수신 대기 포트에 액세스하는 한, 하이브리드 연결 엔드포인트는 모든 운영 체제 및 모든 애플리케이션에 있을 수 있습니다. 하이브리드 연결 기능은 애플리케이션 프로토콜이 무엇인지 또는 사용자가 무엇에 액세스하고 있는지 인식하거나 상관하지 않습니다. 네트워크 액세스만 제공합니다.

자세한 내용은 [하이브리드 연결 관련 App Service 설명서](../app-service/app-service-hybrid-connections.md)를 참조하세요. 동일한 구성 단계는 Azure Functions를 지원합니다.

>[!IMPORTANT]
> 하이브리드 연결은 Windows 플랜에서만 지원됩니다. Linux는 지원되지 않습니다.

## <a name="outbound-ip-restrictions"></a>아웃바운드 IP 제한

아웃바운드 IP 제한은 프리미엄 계획, App Service 계획 또는 App Service Environment에서 사용할 수 있습니다. App Service Environment가 배포되는 가상 네트워크에 대한 아웃바운드 제한을 구성할 수 있습니다.

프리미엄 계획 또는 App Service 계획에서 함수 앱과 가상 네트워크를 통합하는 경우 앱에서 기본적으로 인터넷에 아웃바운드 호출을 수행할 수 있습니다. 애플리케이션 설정 `WEBSITE_VNET_ROUTE_ALL=1`을 추가하여 모든 아웃바운드 트래픽이 트래픽 제한에 네트워크 보안 그룹 규칙을 사용할 수 있는 가상 네트워크로 전송되도록 합니다.

## <a name="automation"></a>Automation
다음 Api를 사용 하면 프로그래밍 방식으로 지역 가상 네트워크 통합을 관리할 수 있습니다.

+ **Azure CLI**: 명령을 사용 [`az functionapp vnet-integration`](/cli/azure/functionapp/vnet-integration) 하 여 지역 가상 네트워크 통합을 추가, 나열 또는 제거 합니다.  
+ **ARM 템플릿**: 지역 가상 네트워크 통합은 Azure Resource Manager 템플릿을 사용 하 여 사용 하도록 설정할 수 있습니다. 전체 예제는 [이 함수 빠른 시작 템플릿](https://azure.microsoft.com/resources/templates/101-function-premium-vnet-integration/)을 참조 하세요.

## <a name="troubleshooting"></a>문제 해결

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>다음 단계

네트워킹 및 Azure Functions에 대한 자세한 내용은 다음을 확인하세요.

* [가상 네트워크 통합 시작하기에 관한 자습서 따라하기](./functions-create-vnet.md)
* [Functions 네트워킹 FAQ 참조하기](./functions-networking-faq.md)
* [App Service/Functions와 가상 네트워크 통합에 대해 자세히 알아보기](../app-service/web-sites-integrate-with-vnet.md)
* [Azure의 가상 네트워크에 대해 자세히 알아보기](../virtual-network/virtual-networks-overview.md)
* [App Service Environment로 제어 및 네트워킹 기능 활성화](../app-service/environment/intro.md)
* [하이브리드 연결을 사용하여 방화벽 변경 없이 개별 온-프레미스 리소스에 연결](../app-service/app-service-hybrid-connections.md)
