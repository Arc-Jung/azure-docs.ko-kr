---
title: (사용되지 않음) Azure Container Service 자습서 - 클러스터 배포
description: Azure Container Service 자습서 - 클러스터 배포
author: iainfoulds
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5cb21bff2834751843061910184499f37bde834e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275446"
---
# <a name="deprecated-deploy-a-kubernetes-cluster-in-azure-container-service"></a>(사용되지 않음) Azure Container Service에서 Kubernetes 클러스터 배포

> [!TIP]
> Azure Kubernetes Service를 사용하는 이 자습서의 업데이트된 버전은 [자습서: AKS(Azure Kubernetes Service) 클러스터 배포](../../aks/tutorial-kubernetes-deploy-cluster.md)를 참조하세요.

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Kubernetes는 컨테이너화된 애플리케이션용 분산 플랫폼을 제공합니다. Azure Container Service를 통해 프로덕션이 준비된 Kubernetes 클러스터를 프로비전하는 작업은 간단하고 빠릅니다. 이 자습서(전체 7부 중 3부)에서는 Azure Container Service Kubernetes 클러스터를 배포했습니다. 완료되는 단계는 다음과 같습니다.

> [!div class="checklist"]
> * Kubernetes ACS 클러스터 배포
> * Kubernetes CLI(kubectl) 설치
> * kubectl 구성

후속 자습서에서는 Azure 투표 애플리케이션을 클러스터에 배포하고 확장/업데이트하며, Kubernetes 클러스터를 모니터링하도록 Log Analytics를 구성합니다.

## <a name="before-you-begin"></a>시작하기 전에

이전 자습서에서는 컨테이너 이미지를 만들어 Azure Container Registry 인스턴스에 업로드했습니다. 이러한 단계를 수행하지 않은 경우 수행하려면 [자습서 1 - 컨테이너 이미지 만들기](./container-service-tutorial-kubernetes-prepare-app.md)로 돌아갑니다.

## <a name="create-kubernetes-cluster"></a>Kubernetes 클러스터 만들기

[az acs create](/cli/azure/acs#az-acs-create) 명령을 사용하여 Azure Container Service에서 Kubernetes 클러스터를 만듭니다. 

다음 예제에서는 `myResourceGroup` 리소스 그룹에 `myK8sCluster` 클러스터를 만듭니다. 이 리소스 그룹은 [이전 자습서](./container-service-tutorial-kubernetes-prepare-acr.md)에서 만든 것입니다.

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

제한 평가판과 같이 Azure 구독의 Azure 리소스 액세스 권한이 제한되는 경우도 있습니다. 사용 가능한 코어 제한으로 인해 배포가 실패하는 경우 [az acs create](/cli/azure/acs#az-acs-create) 명령에 `--agent-count 1`을 추가하여 기본 에이전트 수를 줄이세요. 

몇 분 후 배포가 완료되고 json 형식의 ACS 배포 관련 정보가 반환됩니다.

## <a name="install-the-kubectl-cli"></a>kubectl CLI 설치

클라이언트 컴퓨터에서 Kubernetes 클러스터에 연결하려면 Kubernetes 명령줄 클라이언트인 [kubectl](https://kubernetes.io/docs/user-guide/kubectl/)을 사용합니다. 

Azure Cloud Shell을 사용하는 경우 kubectl이 이미 설치되어 있습니다. 로컬로 설치하려면 [az acs kubernetes install-cli](/cli/azure/acs/kubernetes) 명령을 사용합니다.

Linux 또는 macOS에서 실행하는 경우 sudo를 사용하여 실행해야 할 수 있습니다. Windows에서는 셸이 관리자 권한으로 실행되었는지 확인합니다.

```azurecli-interactive 
az acs kubernetes install-cli 
```

Windows에서 기본 설치는 *c:\program files (x86)\kubectl.exe*입니다. Windows 경로에 이 파일을 추가해야 할 수 있습니다. 

## <a name="connect-with-kubectl"></a>Kubectl로 연결

Kubernetes 클러스터에 연결하도록 kubectl을 구성하려면 [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes) 명령을 실행합니다.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

클러스터에 대한 연결을 확인하려면 [kubectl get nodes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) 명령을 실행합니다.

```azurecli-interactive
kubectl get nodes
```

출력:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

이 자습서를 마치면 ACS Kubernetes 클러스터가 워크로드에 대해 준비됩니다. 이후 자습서에서는 다중 컨테이너 애플리케이션이 이 클러스터에 배포, 규모 확장, 업데이트 및 모니터링됩니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure Container Service Kubernetes 클러스터를 배포했습니다. 다음 단계가 완료되었습니다.

> [!div class="checklist"]
> * Kubernetes ACS 클러스터 배포
> * Kubernetes CLI(kubectl) 설치
> * kubectl 구성

다음 자습서로 이동하여 클러스터에서 애플리케이션 실행에 대해 알아봅니다.

> [!div class="nextstepaction"]
> [Kubernetes에서 애플리케이션 배포](./container-service-tutorial-kubernetes-deploy-application.md)
