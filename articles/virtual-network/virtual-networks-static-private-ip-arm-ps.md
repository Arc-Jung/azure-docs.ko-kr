---
title: 고정 개인 IP 주소를 사용 하 여 VM 만들기-Azure PowerShell
description: PowerShell을 사용 하 여 개인 IP 주소를 사용 하는 가상 머신을 만드는 방법에 대해 알아봅니다.
services: virtual-network
documentationcenter: na
author: KumudD
editor: ''
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.subservice: ip-services
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2019
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 43c2e2c8905ca60e59caed3fdf1b4920f509fd88
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98783787"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>PowerShell을 사용 하 여 고정 개인 IP 주소를 사용 하는 가상 머신 만들기

고정 개인 IP 주소를 사용 하 여 VM (가상 머신)을 만들 수 있습니다. VM에 할당 되는 서브넷의 주소를 선택 하려면 동적 주소가 아닌 고정 개인 IP 주소를 할당 합니다. [정적 개인 IP 주소](./public-ip-addresses.md#allocation-method)에 대해 자세히 알아보세요. 기존 VM에 할당 된 개인 IP 주소를 동적에서 정적으로 변경 하거나 공용 IP 주소를 사용 하려면 [ip 주소 추가, 변경 또는 제거](virtual-network-network-interface-addresses.md)를 참조 하세요.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-virtual-machine"></a>가상 머신 만들기

로컬 컴퓨터에서 또는 Azure Cloud Shell을 사용하여 다음 단계를 완료할 수 있습니다. 로컬 컴퓨터를 사용하려면 [Azure PowerShell을 설치](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json)했는지 확인합니다. Azure Cloud Shell을 사용하려면 뒤에 오는 명령 상자의 오른쪽 상단 모서리에서 **시도** 를 선택합니다. Cloud Shell을 통해 Azure에 로그인합니다.

1. Cloud Shell을 사용하는 경우 2단계로 건너뜁니다. 명령 세션을 열고 `Connect-AzAccount`를 사용하여 Azure에 로그인합니다.
2. [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) 명령을 사용하여 리소스 그룹을 만듭니다. 다음 예제에서는 미국 동부 Azure 지역의 리소스 그룹을 만듭니다.

   ```azurepowershell-interactive
   $RgName = "myResourceGroup"
   $Location = "eastus"
   New-AzResourceGroup -Name $RgName -Location $Location
   ```

3. [AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) 및 [AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) 명령을 사용 하 여 서브넷 구성 및 가상 네트워크를 만듭니다.

   ```azurepowershell-interactive
   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object for use in a later step.
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

4. 가상 네트워크에서 네트워크 인터페이스를 만들고 [AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) 및 [AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) 명령을 사용 하 여 서브넷의 개인 IP 주소를 네트워크 인터페이스에 할당 합니다.

   ```azurepowershell-interactive
   $IpConfigName1 = "IPConfig-1"
   $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

   $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
   ```

5. [AzVMConfig](/powershell/module/Az.Compute/New-AzVMConfig)를 사용 하 여 vm 구성을 만든 다음 [new-azvm](/powershell/module/az.Compute/New-azVM)를 사용 하 여 vm을 만듭니다. 메시지가 표시 되 면 VM에 대 한 로그인 자격 증명으로 사용할 사용자 이름 및 암호를 제공 합니다.

   ```azurepowershell-interactive
   $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
   $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
   $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
   $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
   New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
   ```

> [!WARNING]
> 운영 체제에 개인 IP 주소 설정을 추가할 수는 있지만, [운영 체제에 개인 ip 주소 추가](virtual-network-network-interface-addresses.md#private)를 읽은 후에는이 작업을 수행 하지 않는 것이 좋습니다.
> 
> 
> <a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>
> 
> [!IMPORTANT]
> 인터넷에서 VM에 액세스 하려면 VM에 공용 IP 주소를 할당 해야 합니다. 정적 할당에 대 한 동적 개인 IP 주소 할당을 변경할 수도 있습니다. 자세한 내용은 [IP 주소 추가 또는 변경](virtual-network-network-interface-addresses.md)을 참조 하세요. 또한 네트워크 보안 그룹을 네트워크 인터페이스, 네트워크 인터페이스를 만든 서브넷 또는 둘 다에 연결 하 여 VM에 대 한 네트워크 트래픽을 제한 하는 것이 좋습니다. 자세한 내용은 [네트워크 보안 그룹 관리](manage-network-security-group.md)를 참조 하세요.

## <a name="clean-up-resources"></a>리소스 정리

더 이상 필요하지 않은 경우 [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)을 사용하여 리소스 그룹 및 해당 그룹에 포함된 모든 리소스를 제거할 수 있습니다.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>다음 단계

- [개인 ip 주소](./private-ip-addresses.md) 및 Azure 가상 머신에 [고정 개인 ip 주소](virtual-network-network-interface-addresses.md#add-ip-addresses) 할당에 대해 자세히 알아보세요.
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 및 [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 가상 머신 만들기에 대해 자세히 알아보세요.
