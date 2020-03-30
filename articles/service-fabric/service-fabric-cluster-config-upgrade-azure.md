---
title: Azure 서비스 패브릭 클러스터의 구성 업그레이드
description: Resource Manager 템플릿을 사용하여 Azure에서 Service Fabric 클러스터를 실행하는 구성을 업그레이드하는 방법을 알아봅니다.
author: dkkapur
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 476a2d910b916ea29132b108478d06f756454813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463294"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Azure에서 클러스터의 구성 업그레이드 

이 문서에서는 Service Fabric 클러스터에 대한 다양한 패브릭 설정을 사용자 지정하는 방법을 설명합니다. Azure에서 호스팅된 클러스터의 경우 [Azure Portal](https://portal.azure.com)을 통해 또는 Azure Resource Manager 템플릿을 사용하여 설정을 사용자 지정할 수 있습니다.

> [!NOTE]
> 포털에서 모든 설정을 사용할 수 있는 것은 아니며 [Azure Resource Manager 템플릿을 사용하여 사용자 지정하는 것이 가장 좋은 방법](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)입니다. 포털은 Service Fabric Dev\Test 시나리오 전용입니다.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Resource Manager 템플릿을 사용하여 클러스터 설정 사용자 지정
JSON Resource Manager 템플릿을 통해 Azure 클러스터를 구성할 수 있습니다. 다른 설정에 대해 자세히 알아보려면 [클러스터에 대한 구성 설정](service-fabric-cluster-fabric-settings.md)을 참조하세요. 예제로 아래 단계에서는 Azure Resource Explorer를 사용하여 새로운 *MaxDiskQuotaInMB* 설정을 *Diagnostics* 섹션에 추가하는 방법을 보여줍니다.

1. [https://editor.swagger.io](https://resources.azure.com) 으로 이동합니다.
2. **\<구독>**  -> 리소스 **그룹** -> 리소스**\<를 **확장하여 **구독으로** -> 이동 리소스 그룹> ->  **공급자** -> **Microsoft.ServiceFabric** -> **clusters** -> **\<클러스터클러스터>**
3. 오른쪽 상단 모서리에서 **읽기/쓰기를 선택합니다.**
4. JSON 요소 편집 및 업데이트 및 새 요소를 추가합니다. **Edit** `fabricSettings`

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

또한 Azure Resource Manager를 사용하여 다음 방법 중 하나에서 클러스터 설정을 사용자 지정할 수 있습니다.

- [Azure Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template)을 사용하여 Resource Manger 템플릿을 내보내고 업데이트합니다.
- [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell)을 사용하여 Resource Manger 템플릿을 내보내고 업데이트합니다.
- [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli)를 사용하여 Resource Manger 템플릿을 내보내고 업데이트합니다.
- Azure PowerShell [설정-AzServiceFabric설정](https://docs.microsoft.com/powershell/module/az.servicefabric/Set-azServiceFabricSetting) 및 [제거-AzServiceFabricSet](https://docs.microsoft.com/powershell/module/az.servicefabric/Remove-azServiceFabricSetting) 명령을 사용하여 설정을 직접 수정합니다.
- Azure CLI [az sf 클러스터 설정](https://docs.microsoft.com/cli/azure/sf/cluster/setting) 명령을 사용하여 설정을 직접 수정합니다.

## <a name="next-steps"></a>다음 단계
* [Service Fabric 클러스터 설정](service-fabric-cluster-fabric-settings.md)에 대해 알아봅니다.
* [클러스터를 확장하고 확장하는](service-fabric-cluster-scale-up-down.md)방법에 대해 알아봅니다.
