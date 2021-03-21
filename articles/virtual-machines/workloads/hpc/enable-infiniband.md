---
title: HPC Vm에서 InifinBand 사용-Azure Virtual Machines | Microsoft Docs
description: Azure HPC Vm에서 InfiniBand을 사용 하도록 설정 하는 방법에 대해 알아봅니다.
author: vermagit
ms.service: virtual-machines
ms.subservice: hpc
ms.topic: article
ms.date: 03/18/2021
ms.author: amverma
ms.reviewer: cynthn
ms.openlocfilehash: 089976f2009e006f53dd2a77f09f57d5090429b7
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "104721233"
---
# <a name="enable-infiniband"></a>InfiniBand 사용

[RDMA 지원](../../sizes-hpc.md#rdma-capable-instances) [H 시리즈](../../sizes-hpc.md) 및 [N 시리즈](../../sizes-gpu.md) VM은 낮은 대기 시간과 높은 대역폭 InfiniBand 네트워크를 통해 통신합니다. 이러한 상호 연결에 대한 RDMA 기능은 분산 노드 HPC 및 AI 워크로드의 확장성과 성능을 향상하는 데 매우 중요합니다. InfiniBand 지원 H 시리즈 및 N 시리즈 VM은 최적화되고 일관적인 RDMA 성능을 제공하기 위해 지름이 작은 비중단 팻 트리에서 연결됩니다.

지원 되는 VM 크기에 대 한 InfiniBand를 사용 하도록 설정 하는 다양 한 방법이 있습니다.

## <a name="vm-images-with-infiniband-drivers"></a>InfiniBand 드라이버를 사용 하는 VM 이미지
Marketplace에서 지원 되는 VM 이미지 목록은 [Vm 이미지](configure.md#vm-images) 를 참조 하세요 .이는 InfiniBand 드라이버 (sr-iov 또는 sr-iov가 아닌 vm 용)를 사용 하 여 미리 로드 하거나 [RDMA 지원 vm](../../sizes-hpc.md#rdma-capable-instances)에 대 한 적절 한 드라이버를 사용 하 여 구성할 수 있습니다.
- Marketplace의 [CentOS](configure.md#centos-hpc-vm-images) VM 이미지를 시작 하는 가장 쉬운 방법입니다.
- [Ubuntu](configure.md#ubuntu-vm-images) VM 이미지는 올바른 IB 드라이버를 사용 하 여 구성할 수 있습니다.

## <a name="infiniband-driver-vm-extensions"></a>InfiniBand 드라이버 VM 확장
Linux에서는 [INFINIBANDDRIVERLINUX vm 확장](../../extensions/hpc-compute-infiniband-linux.md) 을 사용 하 여 Mellanox OFED 드라이버를 설치 하 고 Sr-iov 사용 H-및 N 시리즈 Vm에서 InfiniBand를 사용 하도록 설정할 수 있습니다.

Windows에서 [INFINIBANDDRIVERWINDOWS vm 확장](../../extensions/hpc-compute-infiniband-windows.md) 은 RDMA 연결에 Windows 네트워크 직접 드라이버 (sr-iov vm이 아닌 vm) 또는 Mellanox OFED 드라이버 (sr-iov vm)를 설치 합니다. A8 및 A9 인스턴스의 특정 배포에서 HpcVmDrivers 확장이 자동으로 추가 됩니다. HpcVmDrivers VM 확장은 더 이상 사용 되지 않습니다. 업데이트 되지 않습니다.

VM에 VM 확장을 추가해야 하는 경우 [Azure PowerShell](/powershell/azure/) cmdlet을 사용할 수 있습니다. 자세한 내용은 [가상 머신 확장 및 기능](../../extensions/overview.md)을 참조하세요. 또한 [클래식 배포 모델](/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic)에서 배포된 VM의 확장으로 작업할 수 있습니다.

## <a name="manual-installation"></a>수동 설치
[OFED (Mellanox openfabrics 드라이버)](https://www.mellanox.com/products/InfiniBand-VPI-Software) 는 [sr-iov 사용](../../sizes-hpc.md#rdma-capable-instances) [H 시리즈](../../sizes-hpc.md) 및 [N 시리즈](../../sizes-gpu.md) vm에 수동으로 설치할 수 있습니다.

### <a name="linux"></a>Linux
아래 예제를 사용 하 여 [Linux 용 OFED 드라이버](https://www.mellanox.com/products/infiniband-drivers/linux/mlnx_ofed) 를 설치할 수 있습니다. 여기에 나와 있는 예제는 RHEL/CentOS에 대 한 것 이지만 일반적인 단계 이며 Ubuntu (16.04, 18.04 19.04, 20.04) 및 SLES (12 SP4 및 15)와 같은 호환 되는 Linux 운영 체제에 사용할 수 있습니다. 다른 사용자에 대 한 추가 예제는 [azhpc 리포지토리](https://github.com/Azure/azhpc-images/blob/master/ubuntu/ubuntu-18.x/ubuntu-18.04-hpc/install_mellanoxofed.sh)에 있습니다. 또한 수신함 드라이버도 작동 하지만, Mellanox OFED 드라이버는 더 많은 기능을 제공 합니다.

```bash
MLNX_OFED_DOWNLOAD_URL=http://content.mellanox.com/ofed/MLNX_OFED-5.0-2.1.8.0/MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz
# Optionally verify checksum
wget --retry-connrefused --tries=3 --waitretry=5 $MLNX_OFED_DOWNLOAD_URL
tar zxvf MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64.tgz

KERNEL=( $(rpm -q kernel | sed 's/kernel\-//g') )
KERNEL=${KERNEL[-1]}
# Uncomment the lines below if you are running this on a VM
#RELEASE=( $(cat /etc/centos-release | awk '{print $4}') )
#yum -y install http://olcentgbl.trafficmanager.net/centos/${RELEASE}/updates/x86_64/kernel-devel-${KERNEL}.rpm
yum install -y kernel-devel-${KERNEL}
./MLNX_OFED_LINUX-5.0-2.1.8.0-rhel7.7-x86_64/mlnxofedinstall --kernel $KERNEL --kernel-sources /usr/src/kernels/${KERNEL} --add-kernel-support --skip-repo
```

### <a name="windows"></a>Windows
Windows의 경우 [windows 용 MELLANOX OFED 드라이버](https://www.mellanox.com/products/adapter-software/ethernet/windows/winof-2)를 다운로드 하 여 설치 합니다.

## <a name="enable-ip-over-infiniband-ib"></a>IP over InfiniBand (IB) 사용
MPI 작업을 실행 하려는 경우 일반적으로 IPoIB가 필요 하지 않습니다. Mpi 라이브러리는 MPI 라이브러리의 TCP/IP 채널을 명시적으로 사용 하지 않는 한 IB 통신을 위한 동사 인터페이스를 사용 합니다. 그러나 통신에 TCP/IP를 사용 하는 앱이 있는 경우 IB를 통해 실행 하려면 IB 인터페이스를 통해 IPoIB를 사용할 수 있습니다. RHEL/CentOS의 경우 다음 명령을 사용 하 여 InfiniBand를 통해 IP를 사용 하도록 설정 합니다.

```bash
sudo sed -i -e 's/# OS.EnableRDMA=n/OS.EnableRDMA=y/g' /etc/waagent.conf
sudo systemctl restart waagent
```

## <a name="next-steps"></a>다음 단계

- 지원 되는 다양 한 [MPI 라이브러리](setup-mpi.md) 및 vm에 대 한 최적의 구성을 설치 하는 방법에 대해 자세히 알아보세요.
- [HBv3 시리즈 개요](hbv3-series-overview.md) 및 [HC 시리즈 개요](hc-series-overview.md)를 검토 합니다.
- [Azure Compute 기술 커뮤니티 블로그](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)에서 최신 공지 사항, HPC 워크 로드 예제 및 성능 결과에 대해 읽어 보세요.
- HPC 워크로드를 실행하는 상위 수준의 아키텍처 보기는 [Azure의 HPC(고성능 컴퓨팅)](/azure/architecture/topics/high-performance-computing/)를 참조하세요.
