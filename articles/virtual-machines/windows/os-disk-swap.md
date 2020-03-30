---
title: PowerShell '를 사용하여 Azure VM용 OS 디스크 교체
description: PowerShell을 사용하여 Azure Virtual Machine에서 사용하는 운영 체제 디스크를 변경합니다.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: ec66892804f3c2d1f831168a2955f2498462cbf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033020"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>PowerShell을 사용하여 Azure VM에서 사용하는 OS 디스크를 변경합니다.

기존의 VM이 있지만 백업 디스크 또는 다른 OS 디스크에 대한 디스크를 교체하려는 경우 Azure PowerShell을 사용하여 OS 디스크를 교체할 수 있습니다. VM을 삭제하고 다시 만들 필요가 없습니다. 이미 사용 중이 아니면 관리 디스크를 다른 리소스에서 사용할 수도 있습니다.

 

VM은 반드시 중지/할당 취소되어야 합니다. 그래야만 관리 디스크의 리소스 ID를 다른 관리 디스크의 리소스 ID로 바꿀 수 있습니다.

VM 크기 및 스토리지 유형이 연결할 디스크와 호환되어야 합니다. 예를 들어, 사용하려는 디스크가 Premium Storage에 있으면, VM이 Premium Storage를 지원할 수 있어야 합니다(예: DS 시리즈 크기). 두 디스크의 크기도 같아야 합니다.

[Get-AzDisk](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk)를 사용하여 리소스 그룹에서 디스크 목록 가져오기

```azurepowershell-interactive
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
사용할 디스크의 이름이 있는 경우 해당 이름을 VM에 대한 OS 디스크로 설정합니다. 이 예제에서는 *myVM*이라는 VM을 중지/할당 취소하고 *newDisk*라는 이름의 디스크를 새 OS제 디스크로 할당합니다. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**다음 단계**

디스크의 복사본을 만들려면 [디스크 스냅샷](snapshot-copy-managed-disk.md)을 참조하세요.
