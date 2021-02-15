---
title: 프라이빗 Azure Kubernetes Service 클러스터 만들기
description: 프라이빗 AKS(Azure Kubernetes Service) 클러스터를 만드는 방법 알아보기
services: container-service
ms.topic: article
ms.date: 7/17/2020
ms.openlocfilehash: d3b53c860c150b5b67d38cf5d11db9f070ffb81d
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100392802"
---
# <a name="create-a-private-azure-kubernetes-service-cluster"></a>프라이빗 Azure Kubernetes Service 클러스터 만들기

개인 클러스터에서 제어 평면이 나 API 서버에는 [개인 인터넷 문서의 RFC1918 할당](https://tools.ietf.org/html/rfc1918) 에 정의 된 내부 IP 주소가 있습니다. 개인 클러스터를 사용 하 여 API 서버와 노드 풀 간의 네트워크 트래픽이 개인 네트워크에만 남아 있는지 확인할 수 있습니다.

컨트롤 플레인 또는 API 서버는 AKS(Azure Kubernetes Service)에서 관리되는 Azure 구독에 있습니다. 고객의 클러스터 또는 노드 풀은 고객의 구독에 있습니다. 서버와 클러스터 또는 노드 풀은 API 서버 가상 네트워크의 [Azure Private Link 서비스][private-link-service]를 통해 서로 통신할 수 있으며, 고객의 AKS 클러스터의 서브넷에 노출되는 프라이빗 엔드포인트입니다.

## <a name="region-availability"></a>지역 가용성

개인 클러스터는 [AKS가 지원](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service)되는 공용 지역, Azure Government 및 Azure 중국 21vianet 지역에서 사용할 수 있습니다.

> [!NOTE]
> Azure Government 사이트는 지원 되지만 개인 링크 지원이 없으므로 현재 US Gov 텍사스 지원 되지 않습니다.

## <a name="prerequisites"></a>사전 요구 사항

* Azure CLI 버전 2.2.0 이상
* Private Link 서비스는 표준 Azure Load Balancer에서만 지원됩니다. 기본 Azure Load Balancer는 지원되지 않습니다.  
* 사용자 지정 DNS 서버를 사용하려면 Azure DNS IP 168.63.129.16을 사용자 지정 DNS 서버에 업스트림 DNS 서버로 추가합니다.

## <a name="create-a-private-aks-cluster"></a>프라이빗 AKS 클러스터 만들기

### <a name="create-a-resource-group"></a>리소스 그룹 만들기

리소스 그룹을 만들거나 AKS 클러스터를 위한 기존 리소스 그룹을 사용합니다.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>기본값으로 설정된 기본 네트워킹 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
여기서 `--enable-private-cluster` 는 개인 클러스터에 대 한 필수 플래그입니다. 

### <a name="advanced-networking"></a>고급 네트워킹  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
여기서 `--enable-private-cluster` 는 개인 클러스터에 대 한 필수 플래그입니다. 

> [!NOTE]
> Docker 브리지 주소 CIDR(172.17.0.1/16)이 서브넷 CIDR과 충돌하는 경우, Docker 브리지 주소를 적절하게 변경합니다.

## <a name="configure-private-dns-zone"></a>사설 DNS 영역 구성 

다음 매개 변수를 활용 하 여 사설 DNS 영역을 구성할 수 있습니다.

1. "System"이 기본값입니다. --Private-dns 영역 인수를 생략 하면 AKS는 노드 리소스 그룹에 사설 DNS 영역을 만듭니다.
2. "None"은 AKS가 사설 DNS 영역을 만들지 않음을 의미 합니다.  이렇게 하려면 자체 DNS 서버를 가져오고 개인 FQDN에 대 한 DNS 확인을 구성 해야 합니다.  DNS 확인을 구성 하지 않으면 DNS는 에이전트 노드 내 에서만 확인할 수 있으며 배포 후에 클러스터 문제가 발생 합니다.
3. "사용자 지정 개인 dns 영역 이름"은 azure global cloud에 대해이 형식 이어야 `privatelink.<region>.azmk8s.io` 합니다. 해당 사설 DNS 영역의 리소스 Id가 필요 합니다.  또한 사용자 할당 id 또는 서비스 사용자가 적어도 `private dns zone contributor` 사용자 지정 개인 dns 영역에 대 한 역할이 있어야 합니다.

### <a name="prerequisites"></a>사전 요구 사항

* AKS Preview 버전 0.4.71 이상
* Api 버전 2020-11-01 이상

### <a name="create-a-private-aks-cluster-with-private-dns-zone-preview"></a>사설 DNS 영역 (미리 보기)을 사용 하 여 개인 AKS 클러스터 만들기

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster --enable-managed-identity --assign-identity <ResourceId> --private-dns-zone [none|system|custom private dns zone ResourceId]
```
## <a name="options-for-connecting-to-the-private-cluster"></a>프라이빗 클러스터에 연결하기 위한 옵션

API 서버 엔드포인트에 공용 IP 주소가 없습니다. API 서버를 관리 하려면 AKS 클러스터의 Azure Virtual Network (VNet)에 대 한 액세스 권한이 있는 VM을 사용 해야 합니다. 프라이빗 클러스터에 대한 네트워크 연결을 설정할 경우, 몇 가지 옵션이 있습니다.

* AKS 클러스터와 동일한 Azure Virtual Network(VNet)에서 VM을 만듭니다.
* 별도의 네트워크에서 VM을 사용하고 [가상 네트워크 피어링][virtual-network-peering]을 설정합니다.  이 옵션에 관한 자세한 내용은 아래의 해당 섹션을 참조하세요.
* [Express Route 또는 VPN][express-route-or-VPN] 연결을 사용합니다.

AKS 클러스터와 동일한 VNET에 VM을 만드는 것이 가장 쉬운 옵션입니다.  Express Route 및 VPN은 비용을 증가시키며 추가적인 네트워킹 복잡성을 요구합니다.  가상 네트워크 피어링을 사용하려면 중첩되는 범위가 없도록 네트워크 CIDR 범위를 계획해야 합니다.

## <a name="virtual-network-peering"></a>가상 네트워크 피어링

앞서 언급 했 듯이 가상 네트워크 피어 링은 개인 클러스터에 액세스 하는 한 가지 방법입니다. 가상 네트워크 피어 링을 사용 하려면 가상 네트워크와 개인 DNS 영역 간의 링크를 설정 해야 합니다.
    
1. Azure Portal에서 노드 리소스 그룹으로 이동 합니다.  
2. 프라이빗 DNS 영역을 선택합니다.   
3. 왼쪽 창에서 **가상 네트워크** 링크를 선택합니다.  
4. VM의 가상 네트워크를 프라이빗 DNS 영역에 추가하는 새 링크를 만듭니다. DNS 영역 링크를 사용할 수 있을 때까지 몇 분 정도 걸립니다.  
5. Azure Portal에서 클러스터의 가상 네트워크를 포함 하는 리소스 그룹으로 이동 합니다.  
6. 오른쪽 창에서 가상 네트워크를 선택합니다. 가상 네트워크 이름은 *aks-vnet-\** 형식으로 되어 있습니다.  
7. 왼쪽 창에서 **피어링** 을 선택합니다.  
8. **추가** 를 선택하고 VM의 가상 네트워크를 추가한 다음, 피어링을 만듭니다.  
9. VM이 있는 가상 네트워크로 이동하여 **피어링** 을 선택하고 AKS 가상 네트워크를 선택한 후 피어링을 만듭니다. AKS 가상 네트워크의 주소 범위와 VM의 가상 네트워크가 충돌하는 경우, 피어링이 실패합니다. 자세한 내용은 [가상 네트워크 피어링][virtual-network-peering]을 참조하세요.

## <a name="hub-and-spoke-with-custom-dns"></a>사용자 지정 DNS를 사용하는 허브 및 스포크

[허브 및 스포크 아키텍처](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)는 일반적으로 Azure에서 네트워크를 배포하는 데 사용됩니다. 이러한 배포 중 상당수에서 스포크 VNet의 DNS 설정은 중앙 DNS 전달자를 참조하여 온-프레미스 및 Azure 기반 DNS 확인을 허용하도록 구성됩니다. 이러한 네트워킹 환경에 AKS 클러스터를 배포하는 경우, 고려해야 할 몇 가지 특별한 사항들이 있습니다.

![프라이빗 클러스터 허브 및 스포크](media/private-clusters/aks-private-hub-spoke.png)

1. 기본적으로 개인 클러스터가 프로 비전 되 면 개인 끝점 (1)과 개인 DNS 영역 (2)이 클러스터 관리 리소스 그룹에 만들어집니다. 클러스터는 프라이빗 영역의 A 레코드를 사용하여 API 서버와 통신할 프라이빗 엔드포인트의 IP를 확인합니다.

2. 프라이빗 DNS 영역은 클러스터 노드가 (3)과 연결된 VNet에만 연결됩니다. 즉, 연결된 VNet의 호스트만 프라이빗 엔드포인트를 확인할 수 있습니다. VNet에 사용자 지정 DNS가 구성 되지 않은 경우 (기본값),이는 연결로 인해 개인 DNS 영역에 있는 레코드를 확인할 수 있는 168.63.129.16 for DNS의 호스트 지점으로 문제 없이 작동 합니다.

3. 클러스터를 포함하는 VNet에 사용자 지정 DNS 설정(4)이 있는 시나리오에서는 프라이빗 DNS 영역이 사용자 지정 DNS 확인자(5)를 포함하는 VNet에 연결되어 있지 않으면 클러스터 배포가 실패합니다. 이 링크는 클러스터를 프로 비전 하는 동안 또는 이벤트 기반 배포 메커니즘 (예: Azure Event Grid 및 Azure Functions)을 사용 하 여 영역 생성 검색 시 자동화를 통해 만들 때 수동으로 만들 수 있습니다.

> [!NOTE]
> Kubenet를 사용 하 여 [고유한 경로 테이블](./configure-kubenet.md#bring-your-own-subnet-and-route-table-with-kubenet) 을 만들고 개인 클러스터를 사용 하 여 자체 DNS를 가져오는 경우 클러스터 만들기가 실패 합니다. 클러스터를 만드는 데 실패 한 후 노드 리소스 그룹의 [RouteTable](./configure-kubenet.md#bring-your-own-subnet-and-route-table-with-kubenet) 를 서브넷에 연결 하 여 성공적으로 만들 수 있도록 해야 합니다.

## <a name="limitations"></a>제한 사항 
* IP 권한이 부여 된 범위는 개인 api 서버 끝점에 적용할 수 없으며 공용 API 서버에만 적용 됩니다.
* [Azure Private Link 서비스 제한][private-link-service]은 프라이빗 클러스터에 적용됩니다.
* 프라이빗 클러스터를 사용하는 Azure DevOps Microsoft 호스팅 에이전트를 지원하지 않습니다. [자체 호스팅 에이전트](/azure/devops/pipelines/agents/agents?tabs=browser)를 사용하는 것이 좋습니다. 
* Azure Container Registry를 프라이빗 AKS와 함께 사용해야 하는 고객의 경우, Container Registry 가상 네트워크는 에이전트 클러스터 가상 네트워크와 피어링되어야 합니다.
* 기존 AKS 클러스터를 프라이빗 클러스터로 변환하는 기능이 지원되지 않습니다.
* 고객 서브넷에서 프라이빗 엔드포인트를 삭제하거나 수정하면 클러스터의 작동이 중지됩니다. 
* 고객이 자신의 DNS 서버에서 A 레코드를 업데이트 한 후 해당 Pod는 다시 시작 될 때까지 마이그레이션 후에 apiserver FQDN을 이전 IP로 계속 확인 합니다. 고객은 제어 평면 마이그레이션 후 hostNetwork Pod 및 기본-DNSPolicy Pod를 다시 시작 해야 합니다.
* 제어 평면에 대 한 유지 관리의 경우 [AKS IP](./limit-egress-traffic.md) 가 변경 될 수 있습니다. 이 경우 사용자 지정 DNS 서버에서 API 서버 개인 IP를 가리키는 A 레코드를 업데이트 하 고 hostNetwork를 사용 하 여 모든 사용자 지정 pod 또는 배포를 다시 시작 해야 합니다.

<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: ../private-link/private-link-service-overview.md#limitations
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/tutorial-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md
[devops-agents]: /azure/devops/pipelines/agents/agents?view=azure-devops
[availability-zones]: availability-zones.md
