---
title: '자습서: 새 Azure Application Gateway 인스턴스를 사용하여 새 AKS 클러스터에 대한 수신 컨트롤러 추가 기능을 사용하도록 설정합니다.'
description: 이 자습서에서는 Azure CLI를 사용하여 새 Application Gateway 인스턴스로 새 AKS 클러스터에 대한 수신 컨트롤러 추가 기능을 사용하도록 설정하는 방법을 알아봅니다.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: tutorial
ms.date: 09/24/2020
ms.author: caya
ms.openlocfilehash: 775dc2133473354a1e534275fb0d813f299217d1
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99593823"
---
# <a name="tutorial-enable-the-ingress-controller-add-on-preview-for-a-new-aks-cluster-with-a-new-application-gateway-instance"></a>자습서: 새 Application Gateway 인스턴스를 사용하여 새 AKS 클러스터에 대한 수신 컨트롤러 추가 기능(미리 보기)을 사용하도록 설정합니다.

Azure CLI를 사용하여 [AKS(Azure Kubernetes Services)](https://azure.microsoft.com/services/kubernetes-service/) 클러스터에 대한 [AGIC(Application Gateway 수신 컨트롤러)](ingress-controller-overview.md) 추가 기능을 사용하도록 설정할 수 있습니다. 추가 기능은 현재 미리 보기에 있습니다.

이 자습서에서는 AGIC 추가 기능을 사용하도록 설정하여 AKS 클러스터를 만듭니다. 클러스터를 만들면 사용할 Azure Application Gateway 인스턴스가 자동으로 만들어집니다. 그런 다음, 추가 기능을 사용하여 Application Gateway를 통해 애플리케이션을 노출하는 샘플 애플리케이션을 배포합니다. 

이 추가 기능은 [이전에 Helm을 통한](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on) 것보다 AKS 클러스터에 대해 AGIC를 배포하는 훨씬 빠른 방법을 제공합니다. 또한 완전 관리형 환경을 제공합니다.    

이 자습서에서는 다음 작업 방법을 알아봅니다.

> [!div class="checklist"]
> * 리소스 그룹을 만듭니다. 
> * AGIC 추가 기능을 사용하도록 설정하여 새 AKS 클러스터를 만듭니다. 
> * AKS 클러스터에서 수신에 AGIC를 사용하여 샘플 애플리케이션을 배포합니다.
> * Application Gateway를 통해 애플리케이션에 연결할 수 있는지 확인합니다.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

 - 이 자습서에는 Azure CLI 버전 2.0.4 이상이 필요합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다. Azure CLI를 사용하는 경우 다음 명령을 사용하여 CLI에 미리 보기 확장을 설치해야 합니다(아직 없는 경우).
    ```azurecli-interactive
    az extension add --name aks-preview
    ```

 - 다음 예제와 같이 [az feature register](/cli/azure/feature#az-feature-register) 명령을 사용하여 *AKS-IngressApplicationGatewayAddon* 기능 플래그를 등록합니다. 추가 기능이 미리 보기 상태인 동안에는 구독당 한 번만 이 작업을 수행해야 합니다.
    ```azurecli-interactive
    az feature register --name AKS-IngressApplicationGatewayAddon --namespace Microsoft.ContainerService
    ```

   상태가 `Registered`를 표시할 때까지 몇 분 정도 걸립니다. [az feature list](/cli/azure/feature#az-feature-register) 명령을 사용하여 등록 상태를 확인할 수 있습니다.
    ```azurecli-interactive
    az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-IngressApplicationGatewayAddon')].{Name:name,State:properties.state}"
    ```

 - 준비가 되면 [az provider register](/cli/azure/provider#az-provider-register) 명령을 사용하여 Microsoft.ContainerService 리소스 공급자 등록을 새로 고칩니다.
    ```azurecli-interactive
    az provider register --namespace Microsoft.ContainerService
    ```

## <a name="create-a-resource-group"></a>리소스 그룹 만들기

Azure에서 관련 리소스를 리소스 그룹에 할당합니다. [az group create](/cli/azure/group#az-group-create)를 사용하여 리소스 그룹을 만듭니다. 다음 예제에서는 *canadacentral* 위치(지역)에 *myResourceGroup* 이라는 리소스 그룹을 만듭니다. 

```azurecli-interactive
az group create --name myResourceGroup --location canadacentral
```

## <a name="deploy-an-aks-cluster-with-the-add-on-enabled"></a>추가 기능을 사용하도록 설정하여 AKS 클러스터 배포

이제 AGIC 추가 기능을 사용하도록 설정하여 새 AKS 클러스터를 배포합니다. 이 프로세스에서 사용할 기존 Application Gateway 인스턴스를 제공하지 않는 경우 AKS 클러스터에 대한 트래픽을 제공하기 위해 새 Application Gateway 인스턴스를 자동으로 만들고 설정합니다.  

> [!NOTE]
> Application Gateway 수신 컨트롤러 추가 기능에서는 Application Gateway v2 SKU(표준 및 WAF)*만* 지원하고 Application Gateway v1 SKU를 지원하지 *않습니다*. AGIC 추가 기능을 통해 새 Application Gateway 인스턴스를 배포하는 경우 Application Gateway Standard_v2 SKU만 배포할 수 있습니다. Application Gateway WAF_v2 SKU에 대한 추가 기능을 사용하도록 설정하려면 다음 메서드 중 하나를 사용합니다.
>
> - 포털을 통해 Application Gateway에서 WAF를 사용하도록 설정합니다. 
> - 먼저 WAF_v2 Application Gateway 인스턴스를 만든 다음, [기존 AKS 클러스터 및 기존 Application Gateway 인스턴스를 사용하여 AGIC 추가 기능을 사용하도록 설정](tutorial-ingress-controller-add-on-existing.md)하는 방법에 대한 지침을 따르세요. 

다음 예제에서는 [Azure CNI](../aks/concepts-network.md#azure-cni-advanced-networking) 및 [관리 ID](../aks/use-managed-identity.md)를 사용하여 *myCluster* 라는 새 AKS 클러스터를 배포합니다. AGIC 추가 기능은 사용자가 만든 *myResourceGroup* 이라는 리소스 그룹에서 사용하도록 설정할 수 있습니다. 

기존 Application Gateway 인스턴스를 지정하지 않고 AGIC 추가 기능을 사용하도록 설정하여 새 AKS 클러스터를 배포하면 Standard_v2 SKU Application Gateway 인스턴스가 자동으로 생성됩니다. 따라서 Application Gateway 인스턴스의 이름 및 서브넷 주소 공간도 지정합니다. Application Gateway 인스턴스의 이름은 *myApplicationGateway* 가 되며 사용 중인 서브넷 주소 공간은 10.2.0.0/16입니다. 이 자습서의 시작 부분에서 aks-preview 확장을 추가하거나 업데이트했는지 확인합니다. 

```azurecli-interactive
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-prefix "10.2.0.0/16" --generate-ssh-keys
```

`az aks create` 명령에 대한 추가 매개 변수를 구성하려면 [이러한 참조](/cli/azure/aks#az-aks-create)를 참조하세요. 

> [!NOTE]
> 만든 AKS 클러스터는 사용자가 만든 *myResourceGroup* 이라는 리소스 그룹에 표시됩니다. 그러나 자동으로 생성된 Application Gateway 인스턴스는 노드 리소스 그룹에 있습니다. 이 그룹에는 에이전트 풀이 있습니다. 노드 리소스 그룹의 이름은 기본적으로 *MC_resource-group-name_cluster-name_location* 이지만 수정할 수 있습니다. 

## <a name="deploy-a-sample-application-by-using-agic"></a>AGIC를 사용하여 샘플 애플리케이션 배포

이제 만든 AKS 클러스터에 샘플 애플리케이션을 배포합니다. 애플리케이션은 수신에 AGIC 추가 기능을 사용하고 Application Gateway 인스턴스를 AKS 클러스터에 연결합니다. 

먼저 `az aks get-credentials` 명령을 실행하여 AKS 클러스터에 대한 자격 증명을 가져옵니다. 

```azurecli-interactive
az aks get-credentials -n myCluster -g myResourceGroup
```

이제 자격 증명이 있으므로 다음 명령을 실행하여 클러스터를 수신하는 데 AGIC를 사용하는 샘플 애플리케이션을 설정합니다. AGIC는 이전에 해당 라우팅 규칙을 사용하여 설정한 Application Gateway 인스턴스를 배포한 새 샘플 애플리케이션으로 업데이트합니다.  

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml 
```

## <a name="check-that-the-application-is-reachable"></a>애플리케이션에 연결할 수 있는지 확인

Application Gateway 인스턴스가 AKS 클러스터에 대한 트래픽을 제공하도록 설정되었으므로 이제 애플리케이션에 연결할 수 있는지 확인합니다. 먼저 수신의 IP 주소를 가져옵니다. 

```azurecli-interactive
kubectl get ingress
```

다음 방법을 사용하여 만든 샘플 애플리케이션이 실행되고 있는지 확인합니다.

- 이전 명령을 실행하여 얻은 Application Gateway 인스턴스의 IP 주소를 방문합니다.
- `curl`을 사용합니다. 

Application Gateway에서 업데이트를 다운로드하는 데 1분 정도 걸릴 수 있습니다. Application Gateway에서 아직 포털의 상태를 **업데이트** 하는 경우에는 IP 주소에 연결하기 전에 완료하도록 합니다. 

## <a name="clean-up-resources"></a>리소스 정리

더 이상 필요 없는 경우 리소스 그룹, Application Gateway 인스턴스 및 모든 관련 리소스를 제거합니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [AGIC 추가 기능을 사용하지 않도록 설정하는 방법 알아보기](./ingress-controller-disable-addon.md)
