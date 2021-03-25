---
title: 컨테이너 insights 사용 | Microsoft Docs
description: 이 문서에서는 컨테이너를 사용 하는 방법 및 식별 된 성능 관련 문제를 이해할 수 있도록 Container insights를 사용 하도록 설정 하 고 구성 하는 방법을 설명 합니다.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 01246a728f204ed9cb43eee392c637b495208aaf
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105109355"
---
# <a name="enable-container-insights"></a>컨테이너 insights 사용

이 문서에서는 Kubernetes 환경에 배포 되 고에서 호스팅되는 워크 로드의 성능을 모니터링 하기 위해 컨테이너 정보를 설정 하는 데 사용할 수 있는 옵션의 개요를 제공 합니다.

- [AKS(Azure Kubernetes Service)](../../aks/index.yml)  
- [Azure Red Hat OpenShift](../../openshift/intro-openshift.md) 버전 3(sp3) 및 4.x  
- [Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/welcome/index.html) 버전 4.x  
- [Arc 사용 Kubernetes 클러스터](../../azure-arc/kubernetes/overview.md)

에서 호스트 되는 자체 관리 되는 Kubernetes 클러스터에 배포 된 작업의 성능도 모니터링할 수 있습니다.
- Azure, [AKS 엔진](https://github.com/Azure/aks-engine) 사용
- AKS 엔진을 사용 하 여 [Azure Stack](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) 또는 온-프레미스를 사용 합니다.

다음 지원 되는 방법 중 하나를 사용 하 여 새 배포 또는 Kubernetes의 기존 배포 하나 이상에 대해 컨테이너 정보를 사용 하도록 설정할 수 있습니다.

- Azure 포털
- Azure PowerShell
- Azure CLI
- [Terraform 및 AKS](/azure/developer/terraform/create-k8s-cluster-with-tf-and-aks)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>사전 요구 사항

시작 하기 전에 다음 요구 사항을 충족 하는지 확인 합니다.

> [!IMPORTANT]
> Replicaset pod (Log Analytics 컨테이너 화 된 Linux Agent)는 클러스터 내에서 Kubelet Secure 포트 (10250)의 모든 Windows 노드에 대 한 API 호출을 수행 하 여 노드 및 컨테이너 성능 관련 메트릭을 수집 합니다. Kubelet 보안 포트 (: 10250)는 클러스터의 가상 네트워크에서 열려 있어야 합니다 .이 노드는 인바운드 및 아웃 바운드 Windows 노드 및 컨테이너 성능 관련 메트릭 컬렉션이 작동 하기 위한 것입니다.
>
> Windows 노드가 포함 된 Kubernetes 클러스터가 있는 경우 네트워크 보안 그룹 및 네트워크 정책을 검토 하 고 구성 하 여 클러스터의 가상 네트워크에서 Kubelet 보안 포트 (: 10250)가 인바운드 및 아웃 바운드 모두에 대해 열려 있는지 확인 하세요.


- Log Analytics 작업 영역이 있습니다.

   컨테이너 insights는 [지역별 사용 가능한 제품](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)에 나열 된 지역에서 Log Analytics 작업 영역을 지원 합니다.

   새 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정 하는 경우 작업 영역을 만들거나 온 보 딩 환경에서 AKS 클러스터 구독의 기본 리소스 그룹에 기본 작업 영역을 만들도록 할 수 있습니다. 
   
   작업 영역을 직접 만들도록 선택 하는 경우 다음을 통해 만들 수 있습니다. 
   - [Azure Resource Manager](../logs/resource-manager-workspace.md)
   - [PowerShell](../logs/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)
   - [Azure 포털](../logs/quick-create-workspace.md) 
   
   기본 작업 영역에 사용할 수 있는 지원 되는 매핑 쌍 목록은 [컨테이너 insights에 대 한 지역 매핑](container-insights-region-mapping.md)을 참조 하세요.

- 컨테이너 모니터링을 사용 하도록 설정 하기 위해 *Log Analytics 참가자* 그룹의 구성원입니다. Log Analytics 작업 영역에 대한 액세스를 제어하는 방법에 대한 자세한 내용은 [작업 영역 관리](../logs/manage-access.md)를 참조하세요.

- AKS 클러스터 리소스에서 [ *소유자* 그룹](../../role-based-access-control/built-in-roles.md#owner) 의 멤버입니다.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

- 모니터링 데이터를 보려면 컨테이너 insights를 사용 하 여 구성 된 Log Analytics 작업 영역에 [*Log Analytics 읽기 권한자*](../logs/manage-access.md#manage-access-using-azure-permissions) 역할이 있어야 합니다.

- 기본적으로 프로메테우스 메트릭은 수집 되지 않습니다. 메트릭을 수집 하도록 [에이전트를 구성](container-insights-prometheus-integration.md) 하기 전에 스크랩 수 있는 데이터 및 지원 되는 방법을 이해 하려면 [프로메테우스 설명서](https://prometheus.io/) 를 검토 하는 것이 중요 합니다.
- AKS 클러스터는 동일한 Azure AD 테 넌 트의 다른 Azure 구독에 있는 Log Analytics 작업 영역에 연결할 수 있습니다. 현재는 Azure Portal을 사용 하 여이 작업을 수행할 수 없지만 Azure CLI 또는 리소스 관리자 템플릿을 사용 하 여 수행할 수 있습니다.

## <a name="supported-configurations"></a>지원되는 구성

컨테이너 정보는 공식적으로 다음 구성을 지원 합니다.

- 환경: azure Red Hat OpenShift, Kubernetes 온-프레미스 및 Azure의 AKS 엔진 및 Azure Stack 자세한 내용은 [Azure Stack에서 AKS 엔진](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview)을 참조 하세요.
- Kubernetes 및 지원 정책의 버전은 [AKS (Azure Kubernetes Service)에서 지원](../../aks/supported-kubernetes-versions.md)되는 것과 동일 합니다. 

## <a name="network-firewall-requirements"></a>네트워크 방화벽 요구 사항

다음 표에서는 컨테이너 화 된 에이전트가 Container insights와 통신 하는 데 필요한 프록시 및 방화벽 구성 정보를 나열 합니다. 에이전트의 모든 네트워크 트래픽은 Azure Monitor로 아웃 바운드 됩니다.

|에이전트 리소스|포트 |
|--------------|------|
| `*.ods.opinsights.azure.com` | 443 |
| `*.oms.opinsights.azure.com` | 443 |
| `dc.services.visualstudio.com` | 443 |
| `*.monitoring.azure.com` | 443 |
| `login.microsoftonline.com` | 443 |

다음 표에서는 Azure 중국 21Vianet에 대 한 프록시 및 방화벽 구성 정보를 나열 합니다.

|에이전트 리소스|포트 |Description | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.cn` | 443 | 데이터 수집 |
| `*.oms.opinsights.azure.cn` | 443 | OMS 온 보 딩 |
| `dc.services.visualstudio.com` | 443 | Azure 공용 클라우드를 사용 하는 에이전트 원격 분석의 경우 Application Insights |

다음 표에서는 Azure 미국 정부에 대 한 프록시 및 방화벽 구성 정보를 나열 합니다.

|에이전트 리소스|포트 |Description | 
|--------------|------|-------------|
| `*.ods.opinsights.azure.us` | 443 | 데이터 수집 |
| `*.oms.opinsights.azure.us` | 443 | OMS 온 보 딩 |
| `dc.services.visualstudio.com` | 443 | Azure 공용 클라우드를 사용 하는 에이전트 원격 분석의 경우 Application Insights |

## <a name="components"></a>구성 요소

성능을 모니터링 하는 기능은 컨테이너 통찰력을 위해 특별히 개발 된 Linux 용 컨테이너 화 된 Log Analytics 에이전트에 의존 합니다. 이 특수 에이전트는 클러스터의 모든 노드에서 성능 및 이벤트 데이터를 수집하며, 배포 과정에서 에이전트가 지정된 Log Analytics 작업 영역에 자동으로 배포 및 등록됩니다. 

에이전트 버전은 microsoft/oms: ciprod04202018 이상 이며 다음 형식의 날짜로 표시 됩니다. *mmddyyyy*.

>[!NOTE]
>AKS에 대 한 Windows Server 지원의 일반 공급으로, Windows Server 노드가 있는 AKS 클러스터에는 로그를 수집 하 고 Log Analytics에 전달 하기 위해 각 개별 Windows server 노드에 daemonset pod로 설치 된 미리 보기 에이전트가 있습니다. 성능 메트릭의 경우 표준 배포의 일부로 클러스터에 자동으로 배포 되는 Linux 노드는 클러스터의 모든 Windows 노드를 대신 하 여 Azure Monitor 데이터를 수집 하 고 전달 합니다.

새 버전의 에이전트가 릴리스되면 AKS (Azure Kubernetes Service)에 호스트 된 관리 되는 Kubernetes 클러스터에서 자동으로 업그레이드 됩니다. 릴리스된 버전을 추적 하려면 [에이전트 릴리스 알림](https://github.com/microsoft/docker-provider/tree/ci_feature_prod)을 참조 하세요.

> [!NOTE]
> AKS 클러스터를 이미 배포한 경우이 문서의 뒷부분에서 설명한 대로 Azure CLI 또는 제공 된 Azure Resource Manager 템플릿을 사용 하 여 모니터링을 사용 하도록 설정 했습니다. `kubectl`를 사용 하 여 에이전트를 업그레이드, 삭제, 다시 배포 또는 배포할 수 없습니다.
>
> 템플릿을 클러스터와 동일한 리소스 그룹에 배포해야 합니다.

컨테이너 insights를 사용 하도록 설정 하려면 다음 표에 설명 된 방법 중 하나를 사용 합니다.

| 배포 상태 | 메서드 | 설명 |
|------------------|--------|-------------|
| 새 Kubernetes 클러스터 | [Azure CLI를 사용 하 여 AKS 클러스터 만들기](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Azure CLI를 사용 하 여 만든 새 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Terraform을 사용 하 여 AKS 클러스터 만들기](container-insights-enable-new-cluster.md#enable-using-terraform)| 오픈 소스 도구인 Terraform을 사용 하 여 만든 새 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure Resource Manager 템플릿을 사용 하 여 OpenShift 클러스터 만들기](container-insights-azure-redhat-setup.md#enable-for-a-new-cluster-using-an-azure-resource-manager-template) | 미리 구성 된 Azure Resource Manager 템플릿을 사용 하 여 만드는 새 OpenShift 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure CLI를 사용 하 여 OpenShift 클러스터를 만듭니다.](/cli/azure/openshift#az-openshift-create) | Azure CLI를 사용 하 여 새 OpenShift 클러스터를 배포할 때 모니터링을 사용 하도록 설정할 수 있습니다. |
| 기존 Kubernetes 클러스터 | [Azure CLI를 사용 하 여 AKS 클러스터의 모니터링 사용](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Azure CLI를 사용 하 여 이미 배포 된 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| |[Terraform을 사용 하 여 AKS 클러스터에 대해 사용](container-insights-enable-existing-clusters.md#enable-using-terraform) | 오픈 소스 도구 Terraform을 사용 하 여 이미 배포 된 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure Monitor에서 AKS 클러스터 사용](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Azure Monitor의 다중 클러스터 페이지에서 이미 배포 된 하나 이상의 AKS 클러스터에 대해 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [AKS 클러스터에서 사용](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Azure Portal의 AKS 클러스터에서 직접 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure Resource Manager 템플릿을 사용 하 여 AKS 클러스터에 대해 사용](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| 미리 구성 된 Azure Resource Manager 템플릿을 사용 하 여 AKS 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [하이브리드 Kubernetes 클러스터에 대해 사용](container-insights-hybrid-setup.md) | Azure Stack 또는 온-프레미스에서 호스트 되는 Kubernetes 클러스터에 대해 호스트 되는 AKS 엔진에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Arc Enabled Kubernetes cluster를 사용 하도록 설정](container-insights-enable-arc-enabled-clusters.md)합니다. | Azure 외부에서 호스트 되 고 Azure Arc에서 사용 하도록 설정 된 Kubernetes 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure Resource Manager 템플릿을 사용 하 여 OpenShift 클러스터 사용](container-insights-azure-redhat-setup.md#enable-using-an-azure-resource-manager-template) | 미리 구성 된 Azure Resource Manager 템플릿을 사용 하 여 기존 OpenShift 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |
| | [Azure Monitor에서 OpenShift 클러스터 사용](container-insights-azure-redhat-setup.md#from-the-azure-portal) | Azure Monitor의 multicluster 페이지에서 이미 배포 된 하나 이상의 OpenShift 클러스터에 대 한 모니터링을 사용 하도록 설정할 수 있습니다. |

## <a name="next-steps"></a>다음 단계

이제 모니터링을 사용 하도록 설정 했으므로 AKS (Azure Kubernetes Service), Azure Stack 또는 다른 환경에서 호스트 되는 Kubernetes 클러스터의 성능 분석을 시작할 수 있습니다. 컨테이너 insights를 사용 하는 방법에 대 한 자세한 내용은 [View Kubernetes cluster performance](container-insights-analyze.md)을 참조 하세요.
