---
title: IPv6 이중 스택 응용 프로그램 배포-표준 Load Balancer-CLI
titlesuffix: Azure Virtual Network
description: 이 문서에서는 Azure CLI를 사용 하 여 Azure 가상 네트워크에서 IPv6 이중 스택 응용 프로그램을 배포 하는 방법을 보여 줍니다.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: kumud
ms.openlocfilehash: 81635a5f81548f0b4edeeb62fae19c567c6942bb
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98932732"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure-virtual-network---cli"></a>Azure virtual network에서 IPv6 이중 스택 응용 프로그램 배포-CLI

이 문서에서는 이중 스택 서브넷을 사용 하는 이중 스택 가상 네트워크, 이중 (IPv4 + IPv6) 프런트 엔드 구성이 포함 된 표준 Load Balancer, 이중 IP 구성이 있는 Nic를 사용 하는 Vm, 이중 네트워크 보안 그룹 규칙 및 이중 공용 Ip를 포함 하는 Azure의 표준 Load Balancer를 사용 하 여 이중 스택 (IPv4 + IPv6) 응용 프로그램을 배포 하는 방법을 보여 줍니다.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- 이 문서에는 Azure CLI 버전 2.0.49 이상이 필요 합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다.

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

이중 스택 가상 네트워크를 만들려면 먼저 [az group create](/cli/azure/group)를 사용 하 여 리소스 그룹을 만들어야 합니다. 다음 예에서는 *e미국* 위치에 *DsResourceGroup01* 이라는 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create \
--name DsResourceGroup01 \
--location eastus
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses-for-load-balancer"></a>부하 분산 장치에 대 한 IPv4 및 IPv6 공용 IP 주소 만들기
인터넷에서 IPv4 및 IPv6 끝점에 액세스 하려면 부하 분산 장치에 대 한 IPv4 및 IPv6 공용 IP 주소가 필요 합니다. [az network public-ip create](/cli/azure/network/public-ip)를 사용하여 공용 IP 주소를 만듭니다. 다음 예제에서는 *DsResourceGroup01* 리소스 그룹에 *dsPublicIP_v4* 및 *dsPublicIP_v6* 라는 IPv4 및 IPv6 공용 IP 주소를 만듭니다.

```azurecli-interactive
# Create an IPV4 IP address
az network public-ip create \
--name dsPublicIP_v4  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku STANDARD  \
--allocation-method static  \
--version IPv4

# Create an IPV6 IP address
az network public-ip create \
--name dsPublicIP_v6  \
--resource-group DsResourceGroup01  \
--location eastus \
--sku STANDARD  \
--allocation-method static  \
--version IPv6

```

## <a name="create-public-ip-addresses-for-vms"></a>Vm에 대 한 공용 IP 주소 만들기

인터넷에서 Vm에 원격으로 액세스 하려면 Vm에 대 한 IPv4 공용 IP 주소가 필요 합니다. [az network public-ip create](/cli/azure/network/public-ip)를 사용하여 공용 IP 주소를 만듭니다.

```azurecli-interactive
az network public-ip create \
--name dsVM0_remote_access  \
--resource-group DsResourceGroup01 \
--location eastus  \
--sku Standard  \
--allocation-method static  \
--version IPv4

az network public-ip create \
--name dsVM1_remote_access  \
--resource-group DsResourceGroup01  \
--location eastus  \
--sku Standard  \
--allocation-method static  \
--version IPv4
```

## <a name="create-standard-load-balancer"></a>표준 Load Balancer 만들기

이 섹션에서는 부하 분산 장치에 대 한 이중 프런트 엔드 IP (IPv4 및 IPv6) 및 백 엔드 주소 풀을 구성 하 고 표준 Load Balancer를 만듭니다.

### <a name="create-load-balancer"></a>부하 분산 장치 만들기

이전 단계에서 만든 **DsPublicIP_v4** IPV4 공용 IP 주소와 연결 된 **dsLbBackEndPool_v4** 이라는 백 엔드 풀 인 **dsLbFrontEnd_v4** 라는 프런트 엔드 풀을 포함 하는 이름이 **eslb** 인 [az network lb create](/cli/azure/network/lb) 를 사용 하 여 표준 Load Balancer를 만듭니다. 

```azurecli-interactive
az network lb create \
--name dsLB  \
--resource-group DsResourceGroup01 \
--sku Standard \
--location eastus \
--frontend-ip-name dsLbFrontEnd_v4  \
--public-ip-address dsPublicIP_v4  \
--backend-pool-name dsLbBackEndPool_v4
```

### <a name="create-ipv6-frontend"></a>IPv6 프런트 엔드 만들기

[Az network lb 프런트 엔드-ip create](/cli/azure/network/lb/frontend-ip#az-network-lb-frontend-ip-create)를 사용 하 여 IPV6 프런트 엔드 ip를 만듭니다. 다음 예제에서는 *dsLbFrontEnd_v6* 라는 프런트 엔드 IP 구성을 만들고 *dsPublicIP_v6* 주소를 연결 합니다.

```azurecli-interactive
az network lb frontend-ip create \
--lb-name dsLB  \
--name dsLbFrontEnd_v6  \
--resource-group DsResourceGroup01  \
--public-ip-address dsPublicIP_v6

```

### <a name="configure-ipv6-back-end-address-pool"></a>IPv6 백 엔드 주소 풀 구성

[Az network lb address pool create](/cli/azure/network/lb/address-pool#az-network-lb-address-pool-create)를 사용 하 여 IPv6 백 엔드 주소 풀을 만듭니다. 다음 예제에서는 IPv6 NIC 구성을 사용 하 여 Vm을 포함 하도록 *dsLbBackEndPool_v6*  라는 백 엔드 주소 풀을 만듭니다.

```azurecli-interactive
az network lb address-pool create \
--lb-name dsLB  \
--name dsLbBackEndPool_v6  \
--resource-group DsResourceGroup01
```

### <a name="create-a-health-probe"></a>상태 프로브 만들기
[az network lb probe create](/cli/azure/network/lb/probe)를 사용하여 가상 머신의 상태를 모니터링하는 상태 프로브를 만듭니다. 

```azurecli-interactive
az network lb probe create -g DsResourceGroup01  --lb-name dsLB -n dsProbe --protocol tcp --port 3389
```

### <a name="create-a-load-balancer-rule"></a>부하 분산 장치 규칙 만들기

부하 분산 장치 규칙은 VM으로 트래픽이 분산되는 방법을 정의하는 데 사용됩니다. 들어오는 트래픽에 대한 프런트 엔드 IP 구성 및 트래픽을 수신할 백 엔드 IP 풀과 필요한 원본 및 대상 포트를 함께 정의합니다. 

[az network lb rule create](/cli/azure/network/lb/rule#az-network-lb-rule-create)를 사용하여 부하 분산 장치 규칙을 만듭니다. 다음 예제에서는 *dsLBrule_v4* 이라는 부하 분산 장치 규칙을 만들고 *TCP* 포트 *80* 의 트래픽을 IPv4 및 IPv6 프런트 엔드 IP 구성으로 *dsLBrule_v6* 및 분산 합니다.

```azurecli-interactive
az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v4  \
--resource-group DsResourceGroup01  \
--frontend-ip-name dsLbFrontEnd_v4  \
--protocol Tcp  \
--frontend-port 80  \
--backend-port 80  \
--probe-name dsProbe \
--backend-pool-name dsLbBackEndPool_v4


az network lb rule create \
--lb-name dsLB  \
--name dsLBrule_v6  \
--resource-group DsResourceGroup01 \
--frontend-ip-name dsLbFrontEnd_v6  \
--protocol Tcp  \
--frontend-port 80 \
--backend-port 80  \
--probe-name dsProbe \
--backend-pool-name dsLbBackEndPool_v6

```

## <a name="create-network-resources"></a>네트워크 리소스 만들기
일부 Vm을 배포 하기 전에 지원 네트워크 리소스-가용성 집합, 네트워크 보안 그룹, 가상 네트워크 및 가상 Nic를 만들어야 합니다. 
### <a name="create-an-availability-set"></a>가용성 집합 만들기
앱의 가용성을 향상 시키려면 Vm을 가용성 집합에 저장 합니다.

[az vm availability-set create](/cli/azure/vm/availability-set)를 사용하여 가용성 집합을 만듭니다. 다음 예에서는 *dsavset* 이라는 가용성 집합을 만듭니다.

```azurecli-interactive
az vm availability-set create \
--name dsAVset  \
--resource-group DsResourceGroup01  \
--location eastus \
--platform-fault-domain-count 2  \
--platform-update-domain-count 2  
```

### <a name="create-network-security-group"></a>네트워크 보안 그룹 만들기

VNet에서 인바운드 및 아웃 바운드 통신을 제어 하는 규칙에 대 한 네트워크 보안 그룹을 만듭니다.

#### <a name="create-a-network-security-group"></a>네트워크 보안 그룹 만들기

[Az network nsg create](/cli/azure/network/nsg#az-network-nsg-create) 를 사용 하 여 네트워크 보안 그룹 만들기


```azurecli-interactive
az network nsg create \
--name dsNSG1  \
--resource-group DsResourceGroup01  \
--location eastus

```

#### <a name="create-a-network-security-group-rule-for-inbound-and-outbound-connections"></a>인바운드 및 아웃 바운드 연결에 대 한 네트워크 보안 그룹 규칙 만들기

포트 3389을 통해 RDP 연결을 허용 하는 네트워크 보안 그룹 규칙을 만들고, 포트 80을 통한 인터넷 연결을 허용 하 고, [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)를 사용 하 여 아웃 바운드 연결

```azurecli-interactive
# Create inbound rule for port 3389
az network nsg rule create \
--name allowRdpIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 100  \
--description "Allow Remote Desktop In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges 3389

# Create inbound rule for port 80
az network nsg rule create \
--name allowHTTPIn  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 200  \
--description "Allow HTTP In"  \
--access Allow  \
--protocol "*"  \
--direction Inbound  \
--source-address-prefixes "*"  \
--source-port-ranges 80  \
--destination-address-prefixes "*"  \
--destination-port-ranges 80

# Create outbound rule

az network nsg rule create \
--name allowAllOut  \
--nsg-name dsNSG1  \
--resource-group DsResourceGroup01  \
--priority 300  \
--description "Allow All Out"  \
--access Allow  \
--protocol "*"  \
--direction Outbound  \
--source-address-prefixes "*"  \
--source-port-ranges "*"  \
--destination-address-prefixes "*"  \
--destination-port-ranges "*"
```


### <a name="create-a-virtual-network"></a>가상 네트워크 만들기

[az network vnet create](/cli/azure/network/vnet#az-network-vnet-create)를 사용하여 가상 네트워크를 만듭니다. 다음 예제에서는 서브넷 *dsSubNET_v4* 및 *dsSubNET_v6* 를 사용 하 여 *dsvnet* 이라는 가상 네트워크를 만듭니다.

```azurecli-interactive
# Create the virtual network
az network vnet create \
--name dsVNET \
--resource-group DsResourceGroup01 \
--location eastus  \
--address-prefixes "10.0.0.0/16" "fd00:db8:deca::/48"

# Create a single dual stack subnet

az network vnet subnet create \
--name dsSubNET \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--address-prefixes "10.0.0.0/24" "fd00:db8:deca:deed::/64" \
--network-security-group dsNSG1
```

### <a name="create-nics"></a>NIC 만들기

[Az network nic create](/cli/azure/network/nic#az-network-nic-create)를 사용 하 여 각 VM에 대 한 가상 nic를 만듭니다. 다음 예제에서는 각 VM에 대 한 가상 NIC를 만듭니다. 각 NIC에는 두 개의 IP 구성 (1 개의 IPv4 구성, 1 IPv6 구성)이 있습니다. [Az network nic ip config create](/cli/azure/network/nic/ip-config#az-network-nic-ip-config-create)를 사용 하 여 IPV6 구성을 만듭니다.
 
```azurecli-interactive
# Create NICs
az network nic create \
--name dsNIC0  \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1  \
--vnet-name dsVNET  \
--subnet dsSubNet  \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4  \
--lb-name dsLB  \
--public-ip-address dsVM0_remote_access

az network nic create \
--name dsNIC1 \
--resource-group DsResourceGroup01 \
--network-security-group dsNSG1 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv4 \
--lb-address-pools dsLbBackEndPool_v4 \
--lb-name dsLB \
--public-ip-address dsVM1_remote_access

# Create IPV6 configurations for each NIC

az network nic ip-config create \
--name dsIp6Config_NIC0  \
--nic-name dsNIC0  \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name dsNIC1 \
--resource-group DsResourceGroup01 \
--vnet-name dsVNET \
--subnet dsSubNet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB
```

### <a name="create-virtual-machines"></a>가상 머신 만들기

[az vm create](/cli/azure/vm#az-vm-create)로 VM을 만듭니다. 다음 예제에서는 2개의 VM 및 아직 없는 경우 필요한 가상 네트워크 구성 요소를 만듭니다. 

다음과 같이 가상 머신 *dsVM0* 를 만듭니다.

```azurecli-interactive
 az vm create \
--name dsVM0 \
--resource-group DsResourceGroup01 \
--nics dsNIC0 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest  
```

다음과 같이 가상 머신 *dsVM1* 를 만듭니다.

```azurecli-interactive
az vm create \
--name dsVM1 \
--resource-group DsResourceGroup01 \
--nics dsNIC1 \
--size Standard_A2 \
--availability-set dsAVset \
--image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest 
```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure Portal에서 IPv6 이중 스택 가상 네트워크 보기
다음과 같이 Azure Portal에서 IPv6 이중 스택 가상 네트워크를 볼 수 있습니다.
1. 포털의 검색 창에서 *Dsvnet* 을 입력 합니다.
2. 검색 결과에 **myVirtualNetwork** 가 표시되면 선택합니다. 그러면 *Dsvnet* 이라는 이중 스택 가상 네트워크의 **개요** 페이지가 시작 됩니다. 이중 스택 가상 네트워크는 *Dssubnet* 이라는 이중 스택 서브넷에 있는 IPv4 및 IPv6 구성을 모두 사용 하 여 두 개의 nic를 표시 합니다.

  ![Azure의 IPv6 이중 스택 가상 네트워크](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

## <a name="clean-up-resources"></a>리소스 정리

더 이상 필요하지 않은 경우 [az group delete](/cli/azure/group#az-group-delete) 명령을 사용하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
 az group delete --name DsResourceGroup01
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 이중 프런트 엔드 IP 구성 (IPv4 및 IPv6)을 사용 하 여 표준 Load Balancer를 만들었습니다. 또한 부하 분산 장치의 백 엔드 풀에 추가 된 Nic를 포함 하는 두 개의 가상 머신 (IPV4 + IPv6)를 만들었습니다. Azure virtual network의 IPv6 지원에 대 한 자세한 내용은 [azure Virtual Network에 대 한](ipv6-overview.md) i p v 6을 참조 하세요.
