---
title: PowerShell 샘플
description: Vm에 대 한 PowerShell 샘플 목록
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 6d20f02b846c7ae47aef395694aef2bc5732957e
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978648"
---
# <a name="azure-vm-powershell-samples-for-creating-and-managing-linux-vms"></a>Linux Vm을 만들고 관리 하기 위한 Azure VM PowerShell 샘플

다음 표에는 Linux 가상 머신을 만들고 관리하는 PowerShell 스크립트 샘플에 대한 링크가 제공됩니다.

| 스크립트 | Description |
|---|---|
|**가상 머신 만들기**||
| [완벽히 구성된 가상 머신 만들기](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 리소스 그룹, 가상 머신 및 모든 관련된 리소스를 만듭니다.|
| [Docker가 활성화된 VM 만들기](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 가상 머신을 만들고 이 VM을 Docker 호스트로 구성하고 NGINX 컨테이너를 실행합니다. |
| [VM 만들기 및 구성 스크립트 실행](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 가상 머신을 만들고 Azure 사용자 지정 스크립트 확장을 사용하여 NGINX를 설치합니다. |
| [WordPress가 설치된 VM 만들기](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 가상 머신을 만들고 Azure 사용자 지정 스크립트 확장을 사용하여 WordPress를 설치합니다. |
| [관리되는 OS 디스크에서 VM 만들기](./../scripts/virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 기존 관리되는 디스크를 OS 디스크로 연결하여 가상 머신을 만듭니다. |
| [스냅샷에서 VM 만들기](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 먼저 스냅샷에서 관리 디스크를 만든 다음, 새 관리 디스크를 OS 디스크로 연결하여 스냅샷에서 가상 머신을 만듭니다. |
|**스토리지 관리**||
| [동일하거나 다른 구독에 VHD의 관리 디스크 만들기](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 동일하거나 다른 구독에 특수화된 VHD의 관리 디스크를 OS 디스크로 만들거나 데이터 VHD의 관리 디스크를 데이터 디스크로 만듭니다.  |
| [스냅샷에서 관리 디스크 만들기](../scripts/virtual-machines-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 스냅샷에서 관리 디스크를 만듭니다. |
| [스토리지 계정에 스냅샷을 VHD로 내보내기](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 다른 지역의 스토리지 계정에 관리 스냅샷을 VHD로 내보냅니다. |
| [관리 디스크의 VHD를 스토리지 계정으로 내보내기](../scripts/virtual-machines-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 관리 디스크의 기본 VHD를 다른 지역의 스토리지 계정으로 내보냅니다. |
| [VHD에서 스냅샷 만들기](../scripts/virtual-machines-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | VHD에서 스냅샷을 만든 다음, 해당 스냅샷을 사용하여 동일한 여러 관리 디스크를 빠르게 만듭니다.  |
| [동일하거나 다른 구독에 스냅샷 복사](../scripts/virtual-machines-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 부모 스냅샷과 동일한 지역에 있는 동일하거나 다른 구독에 스냅샷을 복사합니다. |
|**가상 머신 모니터링**||
| [Azure Monitor 로그를 사용하여 VM 모니터링](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 가상 머신을 만들고, Log Analytics 에이전트를 설치하고, VM을 Log Analytics 작업 영역에 등록합니다.  |
| [동일하거나 다른 구독에 관리 디스크 복사](../scripts/virtual-machines-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%252fazure%252fvirtual-machines%252flinux%252ftoc.json) | 부모 관리 디스크와 동일한 지역에 있는 동일하거나 다른 구독에 관리 디스크를 복사합니다.
| [PowerShell을 사용하여 구독의 모든 VM에 대한 세부 정보 수집](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | 제공된 구독에서 VM 이름, 리소스 그룹 이름, 지역, Virtual Network, 서브넷, 개인 IP 주소, OS 유형 및 VM의 공용 IP 주소가 포함된 csv를 만듭니다.
| | |