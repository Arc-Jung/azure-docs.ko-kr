---
title: 시작 쿼리 샘플
description: Azure Resource Graph를 사용하여 리소스 개수 계산, 리소스 정렬 또는 특정 태그를 포함한 일부 시작 쿼리를 실행합니다.
ms.date: 10/14/2020
ms.topic: sample
ms.openlocfilehash: 013e865f543f966d88132d2dc6aca6102d52d20c
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92057113"
---
# <a name="starter-resource-graph-query-samples"></a>스타터 Resource Graph 쿼리 샘플

Azure Resource Graph를 사용하는 쿼리를 이해하는 첫 번째 단계는 [쿼리 언어](../concepts/query-language.md)를 기본적으로 이해하는 것입니다. 아직 [KQL(Kusto Query Language)](/azure/kusto/query/index)에 익숙하지 않은 경우에는 [KQL용 자습서](/azure/kusto/query/tutorial)를 검토하여 원하는 리소스에 대한 요청을 작성하는 방법을 이해하는 것이 좋습니다.

다음 시작 쿼리를 살펴보겠습니다.

- [Azure 리소스 개수 계산](#count-resources)
- [Key Vault 리소스 수](#count-keyvaults)
- [리소스를 이름별로 나열](#list-resources)
- [이름의 내림차순으로 모든 가상 머신 나열](#show-vms)
- [이름 및 해당 OS 유형별로 처음 5개의 가상 머신 표시](#show-sorted)
- [OS 유형별 가상 머신 개수 계산](#count-os)
- [스토리지를 포함하는 리소스 표시](#show-storage)
- [모든 공용 IP 주소 나열](#list-publicip)
- [구독에 의해 IP 주소가 구성된 리소스 개수 계산](#count-resources-by-ip)
- [특정 태그 값이 있는 리소스 나열](#list-tag)
- [특정 태그 값이 있는 모든 스토리지 계정 나열](#list-specific-tag)
- [가상 머신 리소스에 대한 별칭 표시](#show-aliases)
- [특정 별칭에 대한 고유 값 표시](#distinct-alias-values)
- [연결되지 않은 네트워크 보안 그룹 표시](#unassociated-nsgs)
- [Azure Advisor에서 비용 절감 요약 가져오기](#advisor-savings)
- [게스트 구성 정책 범위에 있는 머신 수](#count-gcmachines)

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free)을 만듭니다.

## <a name="language-support"></a>언어 지원

Azure CLI(확장을 통해) 및 Azure PowerShell(모듈을 통해)은 Azure Resource Graph를 지원합니다. 다음 쿼리를 실행하기 전에 사용자 환경이 준비되었는지 확인합니다. 선택한 셸 환경의 설치 및 유효성 검사 단계에 대해서는 [Azure CLI](../first-query-azurecli.md#add-the-resource-graph-extension) 및 [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module)을 참조하세요.

## <a name="count-azure-resources"></a><a name="count-resources"></a>Azure 리소스 개수 계산

이 쿼리는 액세스 권한이 있는 구독에 있는 Azure 리소스의 개수를 반환합니다. 또한 이 쿼리는 선택한 셀에 적절한 Azure Resource Graph 구성 요소가 설치되어 있고 작동 중인지 검증하는 데도 적절합니다.

```kusto
Resources
| summarize count()
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | summarize count()"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | summarize count()"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20summarize%20count%28%29" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20summarize%20count%28%29" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20summarize%20count%28%29" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="count-key-vault-resources"></a><a name="count-keyvaults"></a>Key Vault 리소스 수

이 쿼리는 `summarize` 대신 `count`를 사용하여 반환된 레코드 수를 계산합니다. Key Vault만 개수에 포함됩니다.

```kusto
Resources
| where type =~ 'microsoft.keyvault/vaults'
| count
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.keyvault/vaults' | count"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27microsoft.keyvault%2Fvaults%27%0D%0A%7C%20count" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27microsoft.keyvault%2Fvaults%27%0D%0A%7C%20count" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27microsoft.keyvault%2Fvaults%27%0D%0A%7C%20count" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="list-resources-sorted-by-name"></a><a name="list-resources"></a>리소스를 이름별로 나열

이 쿼리는 모든 종류의 리소스를 반환하지만 **name**, **type** 및 **location** 속성만 반환합니다. `order by`를 사용하여 속성을 **name** 속성의 오름차순(`asc`)으로 정렬합니다.

```kusto
Resources
| project name, type, location
| order by name asc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, type, location | order by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, type, location | order by name asc"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20type%2C%20location%0D%0A%7C%20order%20by%20name%20asc" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20type%2C%20location%0D%0A%7C%20order%20by%20name%20asc" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20type%2C%20location%0D%0A%7C%20order%20by%20name%20asc" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="show-all-virtual-machines-ordered-by-name-in-descending-order"></a><a name="show-vms"></a>이름의 내림차순으로 모든 가상 머신 나열

가상 머신(type이 `Microsoft.Compute/virtualMachines`로 지정됨)만 나열하려면 결과에서 **type** 속성만 매칭하면 됩니다. 이전 쿼리와 마찬가지로 `desc`는 `order by`를 내림차순으로 변경합니다. 형식 일치의 `=~`는 Resource Graph에 대/소문자를 구분하지 않도록 알립니다.

```kusto
Resources
| project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20location%2C%20type%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20order%20by%20name%20desc" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20location%2C%20type%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20order%20by%20name%20desc" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20project%20name%2C%20location%2C%20type%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20order%20by%20name%20desc" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="show-first-five-virtual-machines-by-name-and-their-os-type"></a><a name="show-sorted"></a>이름 및 해당 OS 유형별로 처음 5개의 가상 머신 표시

이 쿼리는 `top`를 사용하여 이름별로 정렬된 5개 일치 레코드만 검색합니다. Azure 리소스의 형식은 `Microsoft.Compute/virtualMachines`입니다. `project`는 Azure Resource Graph에 포함할 속성을 알려 줍니다.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%0D%0A%7C%20top%205%20by%20name%20desc" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%0D%0A%7C%20top%205%20by%20name%20desc" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20project%20name%2C%20properties.storageProfile.osDisk.osType%0D%0A%7C%20top%205%20by%20name%20desc" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="count-virtual-machines-by-os-type"></a><a name="count-os"></a>OS 유형별 가상 머신 개수 계산

이전 쿼리를 토대로 작성되며 `Microsoft.Compute/virtualMachines` 종류의 Azure 리소스에 의해 제한되지만 반환되는 레코드 수가 더 이상 제한되지 않습니다.
대신 `summarize` 및 `count()`를 사용하여 속성(이 예제에서 `properties.storageProfile.osDisk.osType`)별로 값을 그룹화하고 집계하는 방법을 정의했습니다. 전체 개체에서 이 문자열이 나타나는 방법에 대한 예제를 보려면 [리소스 탐색 - 가상 머신 검색](../concepts/explore-resources.md#virtual-machine-discovery)을 참조하세요.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28properties.storageProfile.osDisk.osType%29" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28properties.storageProfile.osDisk.osType%29" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28properties.storageProfile.osDisk.osType%29" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

동일한 쿼리를 작성하는 다른 방법은 속성을 `extend`한 후 쿼리 내에서 사용할 임시 이름(이 경우에는 **os**)을 부여하는 것입니다. 그런 다음 **OS** 는 이전 예제에서와 같이 `summarize` 및 `count()`에서 사용됩니다.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28os%29" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28os%29" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20os%20%3D%20properties.storageProfile.osDisk.osType%0D%0A%7C%20summarize%20count%28%29%20by%20tostring%28os%29" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

> [!NOTE]
> `=~`는 대/소문자를 구분하지 않는 일치를 허용하지만, 쿼리에서 속성(예: **properties.storageProfile.osDisk.osType**)을 사용하려면 대/소문자를 올바르게 사용해야 합니다. 속성의 대/소문자가 올바르지 않은 경우 null 또는 잘못된 값이 반환되며 그룹화 또는 요약이 올바르지 않게 됩니다.

## <a name="show-resources-that-contain-storage"></a><a name="show-storage"></a>스토리지를 포함하는 리소스 표시

일치시킬 형식을 명시적으로 정의하는 대신, 이 예제에서는 단어 **storage** 를 `contains`하는 Azure 리소스를 찾습니다.

```kusto
Resources
| where type contains 'storage' | distinct type
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'storage' | distinct type"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'storage' | distinct type"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27storage%27%20%7C%20distinct%20type" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27storage%27%20%7C%20distinct%20type" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27storage%27%20%7C%20distinct%20type" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="list-all-public-ip-addresses"></a><a name="list-publicip"></a>모든 공용 IP 주소 나열

이전 쿼리와 마찬가지로 **publicIPAddresses** 단어를 포함하는 형식에 해당하는 모든 항목을 찾습니다.
이 쿼리는 **properties.ipAddress**
`isnotempty`인 결과만 포함하고 **properties.ipAddress** 를 반환하고, 맨 위에서 `limit`까지의 결과만 포함하도록 확장됩니다.
100. 선택한 셸에 따라 따옴표를 이스케이프해야 할 수도 있습니다.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| project properties.ipAddress
| limit 100
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | project properties.ipAddress | limit 100"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20project%20properties.ipAddress%0D%0A%7C%20limit%20100" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20project%20properties.ipAddress%0D%0A%7C%20limit%20100" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20project%20properties.ipAddress%0D%0A%7C%20limit%20100" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="count-resources-that-have-ip-addresses-configured-by-subscription"></a><a name="count-resources-by-ip"></a>구독에 의해 IP 주소가 구성된 리소스 개수 계산

이전 예제 쿼리를 사용하고 `summarize` 및 `count()`를 추가하면 구성된 IP 주소를 갖는 리소스 구독별 목록을 가져올 수 있습니다.

```kusto
Resources
| where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress)
| summarize count () by subscriptionId
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type contains 'publicIPAddresses' and isnotempty(properties.ipAddress) | summarize count () by subscriptionId"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20summarize%20count%20%28%29%20by%20subscriptionId" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20summarize%20count%20%28%29%20by%20subscriptionId" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20contains%20%27publicIPAddresses%27%20and%20isnotempty%28properties.ipAddress%29%0D%0A%7C%20summarize%20count%20%28%29%20by%20subscriptionId" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="list-resources-with-a-specific-tag-value"></a><a name="list-tag"></a>특정 태그 값이 있는 리소스 나열

태그와 같은 Azure 리소스 형식 이외의 속성으로 결과를 제한할 수 있습니다. 이 예제에서는 값이 **Internal** 이며 태그 이름이 **Environment** 인 Azure 리소스를 필터링합니다.

```kusto
Resources
| where tags.environment=~'internal'
| project name
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

또한 리소스의 태그와 값을 지정하려면 `project` 키워드에 **tags** 속성을 추가합니다.

```kusto
Resources
| where tags.environment=~'internal'
| project name, tags
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where tags.environment=~'internal' | project name, tags"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name%2C%20tags" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name%2C%20tags" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20tags.environment%3D~%27internal%27%0D%0A%7C%20project%20name%2C%20tags" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="list-all-storage-accounts-with-specific-tag-value"></a><a name="list-specific-tag"></a>특정 태그 값이 있는 모든 스토리지 계정 나열

이전 예제의 필터 기능을 결합하고 Azure 리소스 유형을 **type** 속성으로 필터링합니다. 이 쿼리는 또한 특정 태그 이름 및 값을 사용하여 특정한 종류의 Azure 리소스에 대한 검색을 제한합니다.

```kusto
Resources
| where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Storage%2FstorageAccounts%27%0D%0A%7C%20where%20tags%5B%27tag%20with%20a%20space%27%5D%3D%3D%27Custom%20value%27" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Storage%2FstorageAccounts%27%0D%0A%7C%20where%20tags%5B%27tag%20with%20a%20space%27%5D%3D%3D%27Custom%20value%27" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Storage%2FstorageAccounts%27%0D%0A%7C%20where%20tags%5B%27tag%20with%20a%20space%27%5D%3D%3D%27Custom%20value%27" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

> [!NOTE]
> 이 예제에서는 일치를 위해 `=~` 조건 대신 `==`을 사용합니다. `==`은 대/소문자 구분 일치입니다.

## <a name="show-aliases-for-a-virtual-machine-resource"></a><a name="show-aliases"></a>가상 머신 리소스에 대한 별칭 표시

[Azure Policy 별칭](../../policy/concepts/definition-structure.md#aliases)은 Azure Policy에서 리소스 규정 준수를 관리하는 데 사용됩니다. Azure Resource Graph는 리소스 종류의 _별칭_ 을 반환할 수 있습니다. 이러한 값은 사용자 지정 정책 정의를 만들 때 별칭의 현재 값을 비교하는 데 유용합니다. _별칭_ 배열은 쿼리의 결과에 기본적으로 제공되지 않습니다. 결과에 명시적으로 추가하려면 `project aliases`를 사용합니다.

```kusto
Resources
| where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
| project aliases
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'Microsoft.Compute/virtualMachines' | limit 1 | project aliases" | ConvertTo-Json
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20limit%201%0D%0A%7C%20project%20aliases" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20limit%201%0D%0A%7C%20project%20aliases" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20limit%201%0D%0A%7C%20project%20aliases" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="show-distinct-values-for-a-specific-alias"></a><a name="distinct-alias-values"></a>특정 별칭에 대한 고유 값 표시

단일 리소스에 별칭의 값을 표시하는 것이 유용하지만 구독 간에 쿼리할 때 Azure Resource Graph를 사용하는 실제 값은 표시되지 않습니다. 이 예제에서는 특정 별칭의 모든 값을 찾고 고유 값을 반환합니다.

```kusto
Resources
| where type=~'Microsoft.Compute/virtualMachines'
| extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType']
| distinct tostring(alias)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type=~'Microsoft.Compute/virtualMachines' | extend alias = aliases['Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType'] | distinct tostring(alias)"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%3D~%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20alias%20%3D%20aliases%5B%27Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType%27%5D%0D%0A%7C%20distinct%20tostring%28alias%29" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%3D~%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20alias%20%3D%20aliases%5B%27Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType%27%5D%0D%0A%7C%20distinct%20tostring%28alias%29" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%3D~%27Microsoft.Compute%2FvirtualMachines%27%0D%0A%7C%20extend%20alias%20%3D%20aliases%5B%27Microsoft.Compute%2FvirtualMachines%2FstorageProfile.osDisk.managedDisk.storageAccountType%27%5D%0D%0A%7C%20distinct%20tostring%28alias%29" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="show-unassociated-network-security-groups"></a><a name="unassociated-nsgs"></a>연결되지 않은 네트워크 보안 그룹 표시

이 쿼리는 네트워크 인터페이스 또는 서브넷에 연결되지 않은 NSG(네트워크 보안 그룹)를 반환합니다.

```kusto
Resources
| where type =~ "microsoft.network/networksecuritygroups" and isnull(properties.networkInterfaces) and isnull(properties.subnets)
| project name, resourceGroup
| sort by name asc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "Resources | where type =~ 'microsoft.network/networksecuritygroups' and isnull(properties.networkInterfaces) and isnull(properties.subnets) | project name, resourceGroup | sort by name asc"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%22microsoft.network%2Fnetworksecuritygroups%22%20and%20isnull%28properties.networkInterfaces%29%20and%20isnull%28properties.subnets%29%0D%0A%7C%20project%20name%2C%20resourceGroup%0D%0A%7C%20sort%20by%20name%20asc" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%22microsoft.network%2Fnetworksecuritygroups%22%20and%20isnull%28properties.networkInterfaces%29%20and%20isnull%28properties.subnets%29%0D%0A%7C%20project%20name%2C%20resourceGroup%0D%0A%7C%20sort%20by%20name%20asc" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure China 21Vianet Portal: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/Resources%0D%0A%7C%20where%20type%20%3D~%20%22microsoft.network%2Fnetworksecuritygroups%22%20and%20isnull%28properties.networkInterfaces%29%20and%20isnull%28properties.subnets%29%0D%0A%7C%20project%20name%2C%20resourceGroup%0D%0A%7C%20sort%20by%20name%20asc" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="get-cost-savings-summary-from-azure-advisor"></a><a name="advisor-savings"></a>Azure Advisor에서 비용 절감 요약 가져오기

이 쿼리는 각 [Azure Advisor](../../../advisor/advisor-overview.md) 권장 사항의 비용 절감을 요약합니다.

```kusto
advisorresources
| where type == 'microsoft.advisor/recommendations'
| where properties.category == 'Cost'
| extend
    resources = tostring(properties.resourceMetadata.resourceId),
    savings = todouble(properties.extendedProperties.savingsAmount),
    solution = tostring(properties.shortDescription.solution),
    currency = tostring(properties.extendedProperties.savingsCurrency)
| summarize
    dcount(resources), 
    bin(sum(savings), 0.01)
    by solution, currency
| project solution, dcount_resources, sum_savings, currency
| order by sum_savings desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "advisorresources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "advisorresources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/advisorresources%0D%0A%7C%20where%20type%20%3D%3D%20%27microsoft.advisor%2Frecommendations%27%0D%0A%7C%20where%20properties.category%20%3D%3D%20%27Cost%27%0D%0A%7C%20extend%0D%0A%20%20%20%20resources%20%3D%20tostring%28properties.resourceMetadata.resourceId%29%2C%0D%0A%20%20%20%20savings%20%3D%20todouble%28properties.extendedProperties.savingsAmount%29%2C%0D%0A%20%20%20%20solution%20%3D%20tostring%28properties.shortDescription.solution%29%2C%0D%0A%20%20%20%20currency%20%3D%20tostring%28properties.extendedProperties.savingsCurrency%29%0D%0A%7C%20summarize%0D%0A%20%20%20%20dcount%28resources%29%2C%20%0D%0A%20%20%20%20bin%28sum%28savings%29%2C%200.01%29%0D%0A%20%20%20%20by%20solution%2C%20currency%0D%0A%7C%20project%20solution%2C%20dcount_resources%2C%20sum_savings%2C%20currency%0D%0A%7C%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure Government Portal: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/advisorresources%0D%0A%7C%20where%20type%20%3D%3D%20%27microsoft.advisor%2Frecommendations%27%0D%0A%7C%20where%20properties.category%20%3D%3D%20%27Cost%27%0D%0A%7C%20extend%0D%0A%20%20%20%20resources%20%3D%20tostring%28properties.resourceMetadata.resourceId%29%2C%0D%0A%20%20%20%20savings%20%3D%20todouble%28properties.extendedProperties.savingsAmount%29%2C%0D%0A%20%20%20%20solution%20%3D%20tostring%28properties.shortDescription.solution%29%2C%0D%0A%20%20%20%20currency%20%3D%20tostring%28properties.extendedProperties.savingsCurrency%29%0D%0A%7C%20summarize%0D%0A%20%20%20%20dcount%28resources%29%2C%20%0D%0A%20%20%20%20bin%28sum%28savings%29%2C%200.01%29%0D%0A%20%20%20%20by%20solution%2C%20currency%0D%0A%7C%20project%20solution%2C%20dcount_resources%2C%20sum_savings%2C%20currency%0D%0A%7C%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.us <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- Azure 중국 21Vianet 포털: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/advisorresources%0D%0A%7C%20where%20type%20%3D%3D%20%27microsoft.advisor%2Frecommendations%27%0D%0A%7C%20where%20properties.category%20%3D%3D%20%27Cost%27%0D%0A%7C%20extend%0D%0A%20%20%20%20resources%20%3D%20tostring%28properties.resourceMetadata.resourceId%29%2C%0D%0A%20%20%20%20savings%20%3D%20todouble%28properties.extendedProperties.savingsAmount%29%2C%0D%0A%20%20%20%20solution%20%3D%20tostring%28properties.shortDescription.solution%29%2C%0D%0A%20%20%20%20currency%20%3D%20tostring%28properties.extendedProperties.savingsCurrency%29%0D%0A%7C%20summarize%0D%0A%20%20%20%20dcount%28resources%29%2C%20%0D%0A%20%20%20%20bin%28sum%28savings%29%2C%200.01%29%0D%0A%20%20%20%20by%20solution%2C%20currency%0D%0A%7C%20project%20solution%2C%20dcount_resources%2C%20sum_savings%2C%20currency%0D%0A%7C%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.cn <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="count-machines-in-scope-of-guest-configuration-policies"></a><a name="count-gcmachines"></a>게스트 구성 정책 범위에 있는 머신 수

[Azure Policy 게스트 구성](../../policy/concepts/guest-configuration.md) 할당 범위에 있는 Azure 가상 머신 및 Arc 연결 서버의 수를 표시합니다.

```kusto
GuestConfigurationResources
| extend vmid = split(properties.targetResourceId,'/')
| mvexpand properties.latestAssignmentReport.resources
| where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.'
| project machine = tostring(vmid[(-1)]),
    type = tostring(vmid[(-3)])
| distinct machine, type
| summarize count() by type
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.' | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]) | distinct machine, type | summarize count() by type"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "GuestConfigurationResources | extend vmid = split(properties.targetResourceId,'/') | mvexpand properties.latestAssignmentReport.resources | where properties_latestAssignmentReport_resources.resourceId != 'Invalid assignment package.' | project machine = tostring(vmid[(-1)]), type = tostring(vmid[(-3)]) | distinct machine, type | summarize count() by type"
```

# <a name="portal"></a>[포털](#tab/azure-portal)

:::image type="icon" source="../media/resource-graph-small.png"::: Azure Resource Graph Explorer에서 이 쿼리를 사용해 보세요.

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/GuestConfigurationResources%20%7C%20extend%20vmid%20%3D%20split(properties.targetResourceId%2C%22%2F%22)%20%7C%20mvexpand%20properties.latestAssignmentReport.resources%20%7C%20where%20properties_latestAssignmentReport_resources.resourceId%20!%3D%20'Invalid%20assignment%20package.'%20%7C%20project%20machine%20%3D%20tostring(vmid%5B(-1)%5D)%2C%20type%20%3D%20tostring(vmid%5B(-3)%5D)%20%7C%20distinct%20machine%2C%20type%20%7C%20summarize%20count()%20by%20type" target="_blank">portal.azure.com <span class="docon docon-navigate-external x-hidden-focus"></span></a>

---

## <a name="next-steps"></a>다음 단계

- [쿼리 언어](../concepts/query-language.md)에 대해 자세히 알아보기
- [리소스 검색](../concepts/explore-resources.md) 방법에 대해 자세히 알아보기
- [고급 쿼리](advanced.md) 샘플을 살펴봅니다.