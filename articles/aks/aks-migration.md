---
title: Azure Kubernetes Service로 마이그레이션 (AKS)
description: Azure Kubernetes 서비스 (AKS)로 마이그레이션합니다.
services: container-service
ms.topic: article
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: 5881d03603002cc8d5bef1680083f6b4145bc77f
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99526690"
---
# <a name="migrate-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service로 마이그레이션 (AKS)

이 문서에서는 Azure Kubernetes Service (AKS)로의 성공적인 마이그레이션을 계획 하 고 실행 하는 방법을 설명 합니다. 주요 결정을 내리는 데 도움이 되도록이 가이드에서는 AKS에 대 한 현재 권장 구성에 대 한 세부 정보를 제공 합니다. 이 문서에서는 모든 시나리오에 대해 설명 하지 않으며, 해당 하는 경우 성공적인 마이그레이션 계획에 대 한 자세한 정보에 대 한 링크가 포함 되어 있습니다.

이 문서를 사용 하 여 다음 시나리오를 지원할 수 있습니다.

* [가용성 집합](../virtual-machines/windows/tutorial-availability-sets.md) 으로 지원 되는 AKS 클러스터를 [Virtual Machine Scale Sets](../virtual-machine-scale-sets/overview.md) 로 마이그레이션
* [표준 SKU 부하 분산 장치](./load-balancer-standard.md) 를 사용 하도록 AKS 클러스터 마이그레이션
* [Azure Container Service (ACS)에서 마이그레이션-2020 년 1 월 31 일 사용 중지](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/) AKS
* [AKS 엔진](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) 에서 AKS로 마이그레이션
* 비 Azure 기반 Kubernetes 클러스터에서 AKS로 마이그레이션
* 기존 리소스를 다른 지역으로 이동

마이그레이션할 때 대상 Kubernetes 버전이 AKS에 대해 지원 되는 창 내에 있는지 확인 합니다. 이전 버전을 사용 하는 경우 지원 되는 범위 내에 있지 않을 수 있으며 AKS에서 업그레이드 버전을 지원 해야 합니다. 자세한 내용은 [지원 되는 AKS Kubernetes 버전](./supported-kubernetes-versions.md) 을 참조 하세요.

최신 버전의 Kubernetes로 마이그레이션하려는 경우 [Kubernetes 버전 및 버전 기울기 지원 정책을](https://kubernetes.io/docs/setup/release/version-skew-policy/#supported-versions)검토 하세요.

시나리오에 따라 몇 가지 오픈 소스 도구를 사용 하 여 쉽게 마이그레이션할 수 있습니다.

* [Velero](https://velero.io/) (Kubernetes 1.7 이상 필요)
* [Azure Kube CLI 확장](https://github.com/yaron2/azure-kube-cli)
* [Resster](https://github.com/mhausenblas/reshifter)

이 문서에서는 다음에 대 한 마이그레이션 세부 정보를 요약 합니다.

> [!div class="checklist"]
> * 표준 Load Balancer 및 Virtual Machine Scale Sets AKS
> * 기존에 연결 된 Azure 서비스
> * 유효한 할당량 확인
> * 고가용성 및 비즈니스 연속성
> * 상태 비저장 응용 프로그램에 대 한 고려 사항
> * 상태 저장 응용 프로그램에 대 한 고려 사항
> * 클러스터 구성 배포

## <a name="aks-with-standard-load-balancer-and-virtual-machine-scale-sets"></a>표준 Load Balancer 및 Virtual Machine Scale Sets AKS

AKS는 관리 오버 헤드가 낮은 고유한 기능을 제공 하는 관리 되는 서비스입니다. 관리 되는 서비스의 결과로 AKS에서 지 원하는 [지역](./quotas-skus-regions.md) 집합에서 선택 해야 합니다. 기존 클러스터에서 AKS로 전환 하는 경우 기존 응용 프로그램이 AKS 관리 되는 제어 평면에서 정상 상태를 유지 하도록 수정 해야 할 수 있습니다.

[Virtual Machine Scale Sets](../virtual-machine-scale-sets/index.yml) 및 [Azure 표준 Load Balancer](./load-balancer-standard.md) 에 의해 지원 되는 AKS 클러스터를 사용 하는 것이 좋습니다 .이를 통해 [여러 노드 풀](./use-multiple-node-pools.md), [가용성 영역](../availability-zones/az-overview.md), [권한이 부여 된 IP 범위](./api-server-authorized-ip-ranges.md), [클러스터 Autoscaler](./cluster-autoscaler.md), [AKS에 대 한 Azure Policy](../governance/policy/concepts/policy-for-kubernetes.md)및 다른 새로운 기능이 출시 될 때 이러한 기능을 얻을 수 있습니다.

[가상 컴퓨터 가용성 집합](../virtual-machines/availability.md#availability-sets) 으로 지원 되는 AKS 클러스터는 이러한 기능 중 상당수를 지원 하지 않습니다.

다음 예제에서는 가상 머신 확장 집합에 의해 지원 되는 단일 노드 풀로 AKS 클러스터를 만듭니다. 표준 부하 분산 장치를 사용 합니다. 또한 클러스터의 노드 풀에서 클러스터 자동 크기 조정기를 사용하도록 설정하고 최소 *1* 개 및 최대 *3* 개 노드를 설정합니다.

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

## <a name="existing-attached-azure-services"></a>기존에 연결 된 Azure 서비스

클러스터를 마이그레이션할 때 외부 Azure 서비스를 연결 했을 수 있습니다. 이러한 작업에는 리소스 재작성이 필요 하지 않지만 기능을 유지 하려면 이전 클러스터에서 새 클러스터로의 연결을 업데이트 해야 합니다.

* Azure Container Registry
* Log Analytics
* Application Insights
* Traffic Manager
* 스토리지 계정
* 외부 데이터베이스

## <a name="ensure-valid-quotas"></a>유효한 할당량 확인

마이그레이션 중에 추가 가상 머신이 구독에 배포되므로 할당량과 한도가 이러한 리소스에 충분한지 확인해야 합니다. [Vcpu 할당량](../azure-portal/supportability/per-vm-quota-requests.md)증가를 요청 해야 할 수도 있습니다.

[네트워크 할당량](../azure-portal/supportability/networking-quota-requests.md) 에 대 한 증가를 요청 하 여 ip가 고갈 되지 않도록 해야 할 수도 있습니다. 추가 정보는 [AKS에 대 한 네트워킹 및 IP 범위](./configure-kubenet.md) 를 참조 하세요.

자세한 내용은 [Azure 구독 및 서비스 제한](../azure-resource-manager/management/azure-subscription-service-limits.md)을 참조 하세요. 현재 할당량을 확인 하려면 Azure Portal [구독 블레이드](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)로 이동 하 여 구독을 선택한 다음 **사용량 + 할당량** 을 선택 합니다.

## <a name="high-availability-and-business-continuity"></a>고가용성 및 비즈니스 연속성

응용 프로그램에서 가동 중지 시간을 처리할 수 없는 경우 고가용성 마이그레이션 시나리오에 대 한 모범 사례를 따라야 합니다.  복잡 한 비즈니스 연속성 계획, 재해 복구 및 가동 시간 최대화에 대 한 모범 사례는이 문서의 범위를 벗어나는 것입니다.  자세한 내용은 [Azure Kubernetes 서비스 (AKS)에서 비즈니스 연속성 및 재해 복구에 대 한 모범 사례](./operator-best-practices-multi-region.md) 를 참조 하세요.

복잡한 애플리케이션의 경우 일반적으로 한 번에 마이그레이션되는 것이 아니라 시간이 지남에 따라 마이그레이션됩니다. 즉, 이전 환경과 새 환경이 네트워크를 통해 통신 해야 할 수 있습니다. 이전에 서비스를 사용 하 여 통신 하는 응용 프로그램을 `ClusterIP` 형식으로 노출 하 `LoadBalancer` 고 적절 하 게 보호 해야 할 수 있습니다.

마이그레이션을 완료 하기 위해 클라이언트가 AKS에서 실행 되는 새 서비스를 가리키도록 할 수 있습니다. AKS 클러스터 앞에 있는 Load Balancer를 가리키도록 DNS를 업데이트 하 여 트래픽을 리디렉션하는 것이 좋습니다.

[Azure Traffic Manager](../traffic-manager/index.yml) 는 고객에 게 원하는 Kubernetes 클러스터 및 응용 프로그램 인스턴스로 지시할 수 있습니다.  Traffic Manager는 지역 간에 네트워크 트래픽을 분산할 수 있는 DNS 기반 트래픽 부하 분산 장치입니다.  최상의 성능 및 중복성을 위해 AKS 클러스터로 이동 하기 전에 Traffic Manager를 통해 모든 응용 프로그램 트래픽을 보냅니다.  Multicluster 배포에서 고객은 각 AKS 클러스터의 서비스를 가리키는 Traffic Manager DNS 이름에 연결 해야 합니다. Traffic Manager 끝점을 사용 하 여 이러한 서비스를 정의 합니다. 각 끝점은 *서비스 부하 분산 장치 IP* 입니다. 이 구성을 사용 하 여 한 지역의 Traffic Manager 끝점에서 다른 지역의 끝점으로의 네트워크 트래픽을 보낼 수 있습니다.

![Traffic Manager AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

[Azure Front 도어 서비스](../frontdoor/front-door-overview.md) 는 AKS 클러스터에 대 한 트래픽을 라우팅하는 또 다른 옵션입니다.  Azure Front Door Service를 사용하면 최적의 성능과 고가용성을 지원하는 즉시 글로벌 장애 조치(failover)를 최적으로 구현하여 웹 트래픽의 글로벌 라우팅을 정의, 관리, 모니터링할 수 있습니다. 

### <a name="considerations-for-stateless-applications"></a>상태 비저장 응용 프로그램에 대 한 고려 사항

상태 비저장 애플리케이션 마이그레이션은 가장 간단한 경우입니다. 리소스 정의 (YAML 또는 투구)를 새 클러스터에 적용 하 고 모든 항목이 예상 대로 작동 하는지 확인 하 고 트래픽을 리디렉션하여 새 클러스터를 활성화 합니다.

### <a name="considerations-for-stateful-applications"></a>상태 저장 응용 프로그램에 대 한 고려 사항

데이터 손실 또는 예기치 않은 가동 중지 시간을 방지 하기 위해 상태 저장 응용 프로그램의 마이그레이션을 신중 하 게 계획 합니다.

Azure Files를 사용 하는 경우 파일 공유를 볼륨으로 새 클러스터에 탑재할 수 있습니다.
* [정적 Azure Files를 볼륨으로 탑재](./azure-files-volume.md#mount-the-file-share-as-a-volume)

Azure Managed Disks를 사용 하는 경우 VM에 연결 되지 않은 경우에만 디스크를 탑재할 수 있습니다.
* [정적 Azure 디스크를 볼륨으로 탑재](./azure-disk-volume.md#mount-disk-as-volume)

이러한 방법이 작동 하지 않는 경우 백업 및 복원 옵션을 사용할 수 있습니다.
* [Azure의 Velero](https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md)

#### <a name="azure-files"></a>Azure 파일

디스크와 달리 Azure Files는 여러 호스트에 동시에 탑재할 수 있습니다. AKS 클러스터에서 Azure 및 Kubernetes는 ACS 클러스터가 여전히 사용 하는 pod를 만들 수 있도록 방지 하지 않습니다. 데이터 손실 및 예기치 않은 동작을 방지 하려면 클러스터가 같은 파일에 동시에 쓰지 않도록 합니다.

응용 프로그램이 동일한 파일 공유를 가리키는 여러 복제본을 호스팅할 수 있는 경우 상태 비저장 마이그레이션 단계를 수행 하 고 새 클러스터에 YAML 정의를 배포 합니다. 그렇지 않은 경우 한 가지 가능한 마이그레이션 방법에 다음 단계가 포함됩니다.

* 응용 프로그램이 올바르게 작동 하는지 확인 합니다.
* 새 AKS 클러스터에 대 한 라이브 트래픽을 가리킵니다.
* 이전 클러스터의 연결을 끊습니다.

빈 공유로 시작 하 고 원본 데이터의 복사본을 만들려면 [`az storage file copy`](/cli/azure/storage/file/copy) 명령을 사용 하 여 데이터를 마이그레이션합니다.


#### <a name="migrating-persistent-volumes"></a>영구적 볼륨 마이그레이션

기존 영구 볼륨을 AKS로 마이그레이션하는 경우 일반적으로 다음 단계를 수행 합니다.

* 응용 프로그램에 쓰기를 정지 합니다. (이 단계는 선택 사항이 며 가동 중지 시간이 필요 합니다.)
* 디스크의 스냅숏을 생성 합니다.
* 스냅숏에서 새 관리 디스크를 만듭니다.
* AKS에서 영구적 볼륨을 만듭니다.
* PersistentVolumeClaims (정적 프로 비전)가 아닌 [기존 볼륨을 사용](./azure-disk-volume.md) 하도록 pod 사양을 업데이트 합니다.
* 응용 프로그램을 AKS에 배포 합니다.
* 응용 프로그램이 올바르게 작동 하는지 확인 합니다.
* 새 AKS 클러스터에 대 한 라이브 트래픽을 가리킵니다.

> [!IMPORTANT]
> 쓰기를 정지 하지 않도록 선택 하는 경우 새 배포에 데이터를 복제 해야 합니다. 그렇지 않으면 디스크 스냅숏을 만든 후 작성 된 데이터를 놓치지 않습니다.

일부 오픈 소스 도구는 관리 디스크를 만들고 Kubernetes 클러스터 간에 볼륨을 마이그레이션하는 데 도움이 될 수 있습니다.

* [Azure CLI 디스크 복사 확장](https://github.com/noelbundick/azure-cli-disk-copy-extension) 은 리소스 그룹 및 Azure 지역에서 디스크를 복사 하 고 변환 합니다.
* [Azure KUBE CLI 확장](https://github.com/yaron2/azure-kube-cli) 은 ACS Kubernetes 볼륨을 열거 하 고이를 AKS 클러스터로 마이그레이션합니다.


### <a name="deployment-of-your-cluster-configuration"></a>클러스터 구성 배포

기존 CI (지속적인 통합) 및 CD (지속적인 배달) 파이프라인을 사용 하 여 AKS에 알려진 양호한 구성을 배포 하는 것이 좋습니다. Azure Pipelines를 사용 하 여 [응용 프로그램을 빌드하고 응용 프로그램을 AKS에 배포할](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)수 있습니다. 기존 배포 작업을 복제 하 고이 `kubeconfig` 새 AKS 클러스터를 가리키는지 확인 합니다.

가능 하지 않은 경우 기존 Kubernetes 클러스터에서 리소스 정의를 내보낸 다음 AKS에 적용 합니다. `kubectl`을 사용하여 개체를 내보낼 수 있습니다.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

### <a name="moving-existing-resources-to-another-region"></a>기존 리소스를 다른 영역으로 이동

AKS 클러스터를 [AKS에서 지 원하는 다른 지역][region-availability]으로 이동 하는 것이 좋습니다. 다른 지역에 새 클러스터를 만든 다음 리소스 및 응용 프로그램을 새 클러스터에 배포 하는 것이 좋습니다. 또한 AKS 클러스터에서 실행 중인 [Azure Dev Spaces][azure-dev-spaces] 와 같은 서비스가 있는 경우 해당 서비스를 새 지역의 클러스터에 설치 하 고 구성 해야 합니다.


이 문서에서는 다음에 대 한 마이그레이션 세부 정보를 요약 했습니다.

> [!div class="checklist"]
> * 표준 Load Balancer 및 Virtual Machine Scale Sets AKS
> * 기존에 연결 된 Azure 서비스
> * 유효한 할당량 확인
> * 고가용성 및 비즈니스 연속성
> * 상태 비저장 응용 프로그램에 대 한 고려 사항
> * 상태 저장 응용 프로그램에 대 한 고려 사항
> * 클러스터 구성 배포


[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[azure-dev-spaces]: ../dev-spaces/index.yml
