---
title: 포함 파일
description: 포함 파일
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40ae634897361219c39e60d2161d3576cc44a400
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077503"
---
VNet을 신속하게 만들려면 이 문서에서 "사용해 보세요"를 클릭하고 Azure Cloud Shell에서 PowerShell 콘솔을 열면 됩니다. 값을 조정한 다음, 명령을 복사하여 콘솔 창에 붙여 넣습니다. 

사용자가 만든 VNet에 대한 주소 공간이 연결하려는 다른 VNet의 주소 범위 또는 온-프레미스 네트워크 주소 공간과 겹치지 않도록 해야 합니다.

### <a name="create-a-resource-group"></a>리소스 그룹 만들기

사용할 리소스 그룹이 아직 없는 경우 새로 만듭니다. 사용하려는 리소스 그룹 이름에 맞게 PowerShell 명령을 조정하고 다음 cmdlet을 실행합니다.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>VNet 만들기

PowerShell 명령을 조정하여 사용자 환경에 호환되는 VNet을 만듭니다.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```
