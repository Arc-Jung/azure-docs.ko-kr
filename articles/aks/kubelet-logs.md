---
title: AKS(Azure Kubernetes Service)에서 kubelet 로그 보기
description: AZURE Kubernetes 서비스(AKS) 노드에서 쿠벨렛 로그에서 문제 해결 정보를 보는 방법에 대해 알아봅니다.
services: container-service
ms.topic: article
ms.date: 03/05/2019
ms.openlocfilehash: b7a74803af916f9e9de72dd528273007ce37832f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595385"
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>AKS(Azure Kubernetes Service) 클러스터 노드에서 kubelet 로그 가져오기

AKS 클러스터 운영의 일환으로 로그를 검토하여 문제를 해결해야 할 수 있습니다. Azure 포털에 기본 제공되는 기능은 AKS 클러스터에서 [AKS 마스터 구성 요소][aks-master-logs] 또는 컨테이너에 대한 로그를 볼 수 있는 [기능입니다.][azure-container-logs] 경우에 따라 문제 해결을 위해 AKS 노드에서 *kubelet* 로그를 가져와야 할 수도 있습니다.

이 문서에서는 AKS `journalctl` 노드에서 *kubelet* 로그를 보는 데 사용하는 방법을 보여 주며 있습니다.

## <a name="before-you-begin"></a>시작하기 전에

이 문서에서는 기존 AKS 클러스터가 있다고 가정합니다. AKS 클러스터가 필요한 경우 AKS 빠른 시작[Azure CLI 사용][aks-quickstart-cli] 또는 [Azure Portal 사용][aks-quickstart-portal]을 참조하세요.

## <a name="create-an-ssh-connection"></a>SSH 연결 만들기

먼저 *kubelet* 로그를 확인해야 하는 노드에 SSH 연결을 만듭니다. 이 작업 방법은 [AKS(Azure Kubernetes Service) 클러스터 노드에 대한 SSH 연결 만들기][aks-ssh] 문서에 자세히 설명되어 있습니다.

## <a name="get-kubelet-logs"></a>kubelet 로그 가져오기

노드에 연결되면 다음 명령을 실행하여 *kubelet* 로그를 가져옵니다.

```console
sudo journalctl -u kubelet -o cat
```

다음 샘플 출력은 *kubelet* 로그 데이터를 보여 줍니다.

```
I0508 12:26:17.905042    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:27.943494    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:28.920125    8672 server.go:796] GET /stats/summary: (10.370874ms) 200 [[Ruby] 10.244.0.2:52292]
I0508 12:26:37.964650    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:47.996449    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:26:58.019746    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:05.107680    8672 server.go:796] GET /stats/summary/: (24.853838ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:27:08.041736    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:18.068505    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:28.094889    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:38.121346    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:44.015205    8672 server.go:796] GET /stats/summary: (30.236824ms) 200 [[Ruby] 10.244.0.2:52588]
I0508 12:27:48.145640    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:27:58.178534    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:05.040375    8672 server.go:796] GET /stats/summary/: (27.78503ms) 200 [[Go-http-client/1.1] 10.244.0.3:44660]
I0508 12:28:08.214158    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:18.242160    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:28.274408    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:38.296074    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:48.321952    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
I0508 12:28:58.344656    8672 kubelet_node_status.go:497] Using Node Hostname from cloudprovider: "aks-agentpool-11482510-0"
```

## <a name="next-steps"></a>다음 단계

Kubernetes 마스터의 추가 문제 해결 정보가 필요하면 [ AKS에서 Kubernetes 마스터 노드 로그 보기][aks-master-logs]를 참조하세요.

<!-- LINKS - internal -->
[aks-ssh]: ssh.md
[aks-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-master-logs]: view-master-logs.md
[azure-container-logs]: ../azure-monitor/insights/container-insights-overview.md
