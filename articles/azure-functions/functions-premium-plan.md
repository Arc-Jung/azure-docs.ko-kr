---
title: Azure Functions 프리미엄 계획
description: Azure Functions 프리미엄 계획에 대 한 세부 정보 및 구성 옵션 (VNet, 콜드 시작 안 함, 실행 기간 제한 없음)입니다.
author: jeffhollan
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: jehollan
ms.custom:
- references_regions
- fasttrack-edit
- devx-track-azurecli
ms.openlocfilehash: d944512e5f6126920ab4fba99fb70513b93177ba
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97936824"
---
# <a name="azure-functions-premium-plan"></a>Azure Functions 프리미엄 계획

Azure Functions 프리미엄 요금제 (탄력적 프리미엄 요금제 라고도 함)는 함수 앱에 대 한 호스팅 옵션입니다. 다른 호스팅 계획 옵션은 [호스팅 계획 문서](functions-scale.md)를 참조 하세요.

프리미엄 계획 호스팅을 통해 함수에 대 한 다음과 같은 이점을 얻을 수 있습니다.

* 영구적으로 웜 인스턴스로 콜드 시작 방지
* 가상 네트워크 연결.
* 무제한 실행 기간, 60 분이 보장 됩니다.
* 프리미엄 인스턴스 크기: 1 코어, 2 개 코어 및 4 개의 코어 인스턴스입니다.
* 소비 계획과 비교 하 여 보다 예측 가능한 가격 책정.
* 여러 함수 앱이 포함 된 계획에 대 한 고밀도 앱 할당입니다.

프리미엄 요금제를 사용 하는 경우에는 [소비 계획과](consumption-plan.md)마찬가지로 들어오는 이벤트의 수에 따라 Azure Functions 호스트 인스턴스가 추가 되 고 제거 됩니다. 여러 함수 앱을 동일한 프리미엄 계획에 배포할 수 있으며,이 계획을 통해 계산 인스턴스 크기, 기본 계획 크기 및 최대 계획 크기를 구성할 수 있습니다. 

## <a name="billing"></a>결제

프리미엄 요금제에 대 한 청구는 코어 초 수와 여러 인스턴스에 할당 된 메모리를 기준으로 합니다. 이 요금은 실행 및 사용 되는 메모리에 따라 청구 되는 소비 계획과 다릅니다. 프리미엄 요금제를 사용 하 여 실행 요금이 부과 되지 않습니다. 하나 이상의 인스턴스는 계획 당 항상 할당 되어야 합니다. 이 요금은 활성 계획 당 최소 월별 비용이 발생 합니다 .이는 함수가 활성 상태 인지 유휴 상태 인지 여부에 관계 없습니다. 프리미엄 계획의 모든 함수 앱은 할당 된 인스턴스를 공유 한다는 점에 유의 하세요. 자세히 알아보려면 [Azure Functions 가격 책정 페이지](https://azure.microsoft.com/pricing/details/functions/)를 참조 하세요.

## <a name="create-a-premium-plan"></a>프리미엄 플랜 만들기

Azure Portal에서 함수 앱을 만들 때 소비 계획이 기본값입니다. 프리미엄 계획에서 실행 되는 함수 앱을 만들려면 _탄력적 프리미엄_ sku 중 하나를 사용 하 여 App Service 계획을 명시적으로 만들어야 합니다. 만든 함수 앱은이 계획에서 호스팅됩니다. Azure Portal를 사용 하면 프리미엄 계획과 함수 앱을 동시에 간편 하 게 만들 수 있습니다. 동일한 프리미엄 계획에서 둘 이상의 함수 앱을 실행할 수 있지만 둘 다 동일한 운영 체제 (Windows 또는 Linux)에서 실행 됩니다. 

다음 문서에서는 프로그래밍 방식으로 또는 Azure Portal에서 프리미엄 요금제를 사용 하 여 함수 앱을 만드는 방법을 보여 줍니다.

+ [Azure Portal](create-premium-plan-function-app-portal.md)
+ [Azure CLI](scripts/functions-cli-create-premium-plan.md)
+ [Azure Resource Manager 템플릿](functions-infrastructure-as-code.md#deploy-on-premium-plan)

## <a name="eliminate-cold-starts"></a>콜드 시작 제거

소비 계획에서 이벤트 또는 실행이 발생 하지 않으면 앱이 0 개의 인스턴스로 확장 될 수 있습니다. 새 이벤트가 제공 될 때 앱에서 실행 되는 새 인스턴스가 특수화 되어야 합니다. 특수화 된 새 인스턴스는 앱에 따라 다소 시간이 걸릴 수 있습니다. 첫 번째 호출의 이러한 추가 대기 시간은 종종 앱 _콜드 시작_ 이라고 합니다.

프리미엄 요금제는 함수에서 콜드 시작을 효과적으로 제거 하기 위해 함께 작동 하는 두 가지 기능을 제공 합니다. _항상 준비 인스턴스와_ _사전 준비 인스턴스_ 를 제공 합니다. 

### <a name="always-ready-instances"></a>항상 준비 인스턴스

프리미엄 계획에서는 지정 된 수의 인스턴스에서 앱이 항상 준비 되도록 할 수 있습니다. 항상 준비 인스턴스의 최대 수는 20 개입니다. 이벤트가 응용 프로그램 트리거를 시작 하면 먼저 항상 준비 인스턴스로 라우팅됩니다. 함수가 활성화 되 면 추가 인스턴스가 버퍼로 준비 됩니다. 이 버퍼는 크기 조정 중에 필요한 새 인스턴스에 대 한 콜드 시작을 방지 합니다. 이러한 버퍼링 된 인스턴스를 [사전 준비 인스턴스](#pre-warmed-instances)라고 합니다. 항상 준비 인스턴스와 미리 준비 된 버퍼의 조합을 사용 하 여 앱에서 콜드 시작을 효과적으로 제거할 수 있습니다.

> [!NOTE]
> 모든 프리미엄 요금제에는 항상 하나 이상의 활성 (청구) 인스턴스가 있습니다.

# <a name="portal"></a>[포털](#tab/portal)

**함수 앱** 를 선택 하 고 **플랫폼 기능** 탭으로 이동한 다음 **Scale Out** 옵션을 선택 하 여 Azure Portal에서 항상 준비 인스턴스 수를 구성할 수 있습니다. 함수 앱 편집 창에서 항상 준비 인스턴스는 해당 앱에만 적용 됩니다.

![탄력적 크기 조정 설정](./media/functions-premium-plan/scale-out.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

Azure CLI를 사용 하 여 앱에 대 한 항상 준비 인스턴스를 구성할 수도 있습니다.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.minimumElasticInstanceCount=<desired_always_ready_count> --resource-type Microsoft.Web/sites
```
---

### <a name="pre-warmed-instances"></a>사전 준비 인스턴스

준비 인스턴스는 크기 조정 및 활성화 이벤트 중에 버퍼로 준비 인스턴스입니다. 준비 인스턴스는 최대 확장 한도에 도달할 때까지 계속 버퍼링 됩니다. 기본 사전 준비 인스턴스 수는 1이 고, 대부분의 시나리오에서는이 값을 1로 유지 해야 합니다.

앱에 긴 준비 (사용자 지정 컨테이너 이미지 등)가 있는 경우이 버퍼를 늘려야 할 수 있습니다. 준비 인스턴스는 모든 활성 인스턴스가 충분히 사용 된 후에만 활성화 됩니다.

항상 준비 인스턴스와 사전 준비 인스턴스가 함께 작동 하는 방법의 예를 살펴보겠습니다. 프리미엄 함수 앱에는 항상 구성 된 5 개의 준비 인스턴스 및 기본 준비 인스턴스 하나가 있습니다. 앱이 유휴 상태이 고 이벤트가 트리거되지 않을 때 앱은 5 개의 인스턴스로 프로 비전 되 고 실행 됩니다. 현재는 항상 준비 인스턴스를 사용 하지 않고 사전 준비 인스턴스가 할당 되지 않으므로 준비 인스턴스에 대 한 요금이 청구 되지 않습니다.

첫 번째 트리거가 제공 되는 즉시 5 개의 항상 준비 된 인스턴스가 활성화 되 고 사전 준비 인스턴스가 할당 됩니다. 이제 응용 프로그램은 6 개의 프로 비전 된 인스턴스로 실행 됩니다. 5 개의 현재 활성 상태인 항상 준비 된 인스턴스와 여섯 번째 사전 준비 및 비활성 버퍼가 있습니다. 실행 비율이 계속 증가 하는 경우 5 개의 활성 인스턴스가 결국 사용 됩니다. 플랫폼에서 5 개 이상의 인스턴스를 확장 하기로 결정 하는 경우에는 사전 준비 인스턴스로 확장 됩니다. 이러한 상황이 발생 하면 6 개의 활성 인스턴스가 있으며 일곱 번째 인스턴스가 즉시 프로 비전 되 고 준비 버퍼를 채웁니다. 앱의 최대 인스턴스 수에 도달할 때까지이 크기 조정 및 연속 작업이 계속 진행 됩니다. 준비는 인스턴스를 최대 수를 초과 하 여 활성화 하거나 비활성화 합니다.

Azure CLI를 사용 하 여 앱에 대 한 사전 준비 인스턴스 수를 수정할 수 있습니다.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="maximum-function-app-instances"></a>최대 함수 앱 인스턴스

[계획 최대 인스턴스 수](#plan-and-sku-settings)외에 앱 별 최대값을 구성할 수 있습니다. 앱 [크기 제한을](./event-driven-scaling.md#limit-scale-out)사용 하 여 최대 앱을 구성할 수 있습니다.

## <a name="private-network-connectivity"></a>개인 네트워크 연결

프리미엄 계획에 배포 된 함수 앱은 [웹 앱에 대 한 VNet 통합](../app-service/web-sites-integrate-with-vnet.md)을 활용할 수 있습니다. 구성 된 앱은 VNet 내의 리소스와 통신 하거나 서비스 끝점을 통해 보안을 설정할 수 있습니다. IP 제한은 앱에서 들어오는 트래픽을 제한 하는 데에도 사용할 수 있습니다.

프리미엄 계획의 함수 앱에 서브넷을 할당 하는 경우 각 잠재적 인스턴스에 대해 충분 한 IP 주소가 있는 서브넷이 필요 합니다. 사용 가능한 주소가 100 이상인 IP 블록이 필요 합니다.

자세한 내용은 [함수 앱을 VNet과 통합](functions-create-vnet.md)을 참조 하세요.

## <a name="rapid-elastic-scale"></a>신속한 탄력적 확장

소비 계획과 동일한 빠른 크기 조정 논리를 사용 하 여 앱에 대 한 추가 계산 인스턴스가 자동으로 추가 됩니다. 동일한 App Service 계획의 앱은 개별 앱의 요구에 따라 서로 독립적으로 확장 됩니다. 그러나 동일한 App Service 계획의 함수 앱은 가능한 경우 비용을 줄이기 위해 VM 리소스를 공유 합니다. VM에 연결 된 앱 수는 각 앱의 공간 및 VM 크기에 따라 달라 집니다.

크기 조정의 작동 방식에 대 한 자세한 내용은 [Azure Functions 이벤트 기반 크기 조정](event-driven-scaling.md)을 참조 하세요.

## <a name="longer-run-duration"></a>더 긴 실행 지속 시간

소비 계획의 Azure Functions은 단일 실행에 대해 10 분으로 제한 됩니다. 프리미엄 계획에서 실행 기간은 기본적으로 30 분으로 설정 되어 런어웨이 실행을 방지 합니다. 그러나 [구성에](./functions-host-json.md#functiontimeout) 대 한 host.js를 수정 하 여 프리미엄 계획 앱에 대 한 기간을 제한 없이 만들 수 있습니다. 제한 없는 기간으로 설정 된 경우 함수 앱은 최소 60 분 동안 실행 되도록 보장 됩니다. 

## <a name="plan-and-sku-settings"></a>요금제 및 SKU 설정

계획을 만들 때 최소 인스턴스 수 (또는 계획 크기)와 최대 버스트 제한의 두 가지 계획 크기 설정이 있습니다.

앱에서 항상 사용 가능한 인스턴스 이외의 인스턴스를 필요로 하는 경우 인스턴스 수가 최대 버스트 제한에 도달할 때까지 계속 규모를 확장할 수 있습니다. 실행 되는 동안 및 사용자에 게 할당 되는 경우에만 계획 크기를 초과 하는 인스턴스에 대 한 요금은 초 단위로 청구 됩니다. 플랫폼을 사용 하면 정의 된 최대 제한까지 앱을 확장 하는 것이 가장 좋습니다.

계획의 **Scale Out** 옵션 또는 해당 계획에 배포 된 함수 앱 ( **플랫폼 기능** 아래)을 선택 하 여 Azure Portal의 계획 크기 및 최대값을 구성할 수 있습니다.

Azure CLI에서 최대 버스트 제한을 늘릴 수도 있습니다.

```azurecli-interactive
az functionapp plan update -g <resource_group> -n <premium_plan_name> --max-burst <desired_max_burst>
```

모든 계획에 대 한 최소값은 하나 이상의 인스턴스가 됩니다. 실제 최소 인스턴스 수는 계획의 앱에서 요청 하는 항상 준비 된 인스턴스를 기반으로 자동으로 구성 됩니다. 예를 들어 앱 A가 항상 준비 된 인스턴스 5 개를 요청 하 고, 앱 B가 동일한 계획에 두 개의 항상 준비 인스턴스를 요청 하는 경우 최소 계획 크기는 5로 계산 됩니다. 앱 A는 모든 5에서 실행 되며, 앱 B는 2 에서만 실행 됩니다.

> [!IMPORTANT]
> 함수 실행 여부에 관계 없이 최소 인스턴스 수에 할당 된 각 인스턴스에 대해 요금이 청구 됩니다.

대부분의 경우이 autocalculated 최소한으로 충분 합니다. 그러나 최소값 이상으로 크기를 조정 하는 것은 최상의 노력으로 발생 합니다. 그러나 추가 인스턴스를 사용할 수 없는 경우에는 특정 시간 확장에서 지연이 발생할 수 있습니다. 최소 autocalculated 이상으로 설정 하 여 인스턴스를 축소 하기 전에 예약 합니다.

계획에 대해 계산 된 최소값을 늘리려면 Azure CLI를 사용 하 여 수행할 수 있습니다.

```azurecli-interactive
az functionapp plan update -g <resource_group> -n <premium_plan_name> --min-instances <desired_min_instances>
```

### <a name="available-instance-skus"></a>사용 가능한 인스턴스 Sku

계획을 만들거나 크기를 조정할 때 세 가지 인스턴스 크기 중에서 선택할 수 있습니다. 각 인스턴스가 할당 되는 초당 프로 비전 된 코어 및 메모리의 총 수에 대 한 요금이 청구 됩니다. 필요에 따라 앱이 여러 인스턴스로 자동 확장 될 수 있습니다.

|SKU|코어|메모리|스토리지|
|--|--|--|--|
|EP1|1|3.5 g b|250GB|
|E P 2|2|7GB|250GB|
|노출할|4|14GB|250GB|

### <a name="memory-usage-considerations"></a>메모리 사용량 고려 사항

메모리를 더 많이 사용 하는 컴퓨터에서를 실행 하면 항상 함수 앱에서 사용 가능한 모든 메모리를 사용 하는 것은 아닙니다.

예를 들어 JavaScript 함수 앱은 Node.js의 기본 메모리 제한에 의해 제한 됩니다. 이 고정 메모리 제한을 늘리려면 값을 사용 하 여 앱 설정을 추가 합니다 `languageWorkers:node:arguments` `--max-old-space-size=<max memory in MB>` .

## <a name="region-max-scale-out"></a>영역 최대 Scale Out

다음은 각 지역 및 OS 구성에서 단일 계획에 대해 현재 지원 되는 최대 확장 값입니다. 증가를 요청 하려면 지원 티켓을 열 수 있습니다.

[Azure 웹 사이트](https://azure.microsoft.com/global-infrastructure/services/?products=functions)에서 함수의 전체 지역 가용성을 참조 하세요.

|Region| Windows | Linux |
|--| -- | -- |
|오스트레일리아 중부| 100 | 사용할 수 없음 |
|오스트레일리아 중부 2| 100 | 사용할 수 없음 |
|오스트레일리아 동부| 100 | 20 |
|오스트레일리아 남동부 | 100 | 20 |
|브라질 남부| 100 | 20 |
|캐나다 중부| 100 | 20 |
|미국 중부| 100 | 20 |
|중국 동부 2| 100 | 20 |
|중국 북부 2| 100 | 20 |
|동아시아| 100 | 20 |
|미국 동부 | 100 | 20 |
|미국 동부 2| 100 | 20 |
|프랑스 중부| 100 | 20 |
|독일 중서부| 100 | 사용할 수 없음 |
|일본 동부| 100 | 20 |
|일본 서부| 100 | 20 |
|한국 중부| 100 | 20 |
|한국 남부| 사용할 수 없음 | 20 |
|미국 중북부| 100 | 20 |
|북유럽| 100 | 20 |
|노르웨이 동부| 100 | 20 |
|미국 중남부| 100 | 20 |
|인도 남부 | 100 | 사용할 수 없음 |
|동남 아시아| 100 | 20 |
|스위스 북부| 100 | 사용할 수 없음 |
|스위스 서부| 100 | 사용할 수 없음 |
|영국 남부| 100 | 20 |
|영국 서부| 100 | 20 |
|USGov 애리조나| 100 | 20 |
|USGov 버지니아| 100 | 20 |
|USNat 동부| 100 | 사용할 수 없음 |
|USNat 서 부| 100 | 사용할 수 없음 |
|서유럽| 100 | 20 |
|인도 서부| 100 | 20 |
|미국 중서부| 100 | 20 |
|미국 서부| 100 | 20 |
|미국 서부 2| 100 | 20 |

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Functions 호스팅 옵션 이해](functions-scale.md)
