---
title: Azure Red Hat OpenShift 4 클러스터 지원 정책
description: Red Hat OpenShift 4의 지원 정책 요구 사항 이해
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 03/05/2021
ms.openlocfilehash: 30579536b8051e9a045c217751871287636a3976
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102454281"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Azure Red Hat OpenShift 지원 정책

Azure Red Hat OpenShift 4 클러스터에 대한 특정 구성은 클러스터의 지원 가능성에 영향을 줄 수 있습니다. Azure Red Hat OpenShift 4를 통해 클러스터 관리자는 내부 클러스터 구성 요소를 변경할 수 있지만 일부 변경은 지원되지 않습니다. 아래 지원 정책에서는 정책을 위반한 수정 사항과 Microsoft 및 Red Hat의 void 지원을 공유합니다.

> [!NOTE]
> OpenShift Container 플랫폼에서 기술 미리 보기로 표시된 기능은 Azure Red Hat OpenShift에서 지원되지 않습니다.

## <a name="cluster-configuration-requirements"></a>클러스터 구성 요구 사항

* 모든 OpenShift 클러스터 연산자는 관리 상태로 유지되어야 합니다. 클러스터 연산자 목록은 `oc get clusteroperators`를 실행하여 반환할 수 있습니다.
* 클러스터에는 최소 3 개의 작업자 노드와 3 개의 관리자 노드가 있어야 합니다. OpenShift 구성 요소를 예약 하지 못하도록 하는 taints 없습니다. 클러스터 작업자의 크기를 0으로 조정 하지 마십시오. 또는 클러스터를 종료 하는 것이 정상입니다.
* 클러스터 Prometheus 및 Alertmanager 서비스를 제거하거나 수정하지 마세요.
* Service Alertmanager 규칙을 제거하지 마세요.
* 네트워크 보안 그룹을 제거 하거나 수정 하지 마십시오.
* Azure Red Hat OpenShift 서비스 로깅(mdsd pod)을 제거하거나 수정하지 마세요.
* 'arosvc.azurecr.io' 클러스터 풀 비밀을 제거하거나 수정하지 마세요.
* 모든 클러스터 가상 머신에는 최소한 ARM(Azure Resource Manager) 및 서비스 로깅(Geneva) 엔드포인트에 대한 직접적인 아웃바운드 인터넷 액세스 권한이 있어야 합니다.  HTTPS 프록시 형식은 지원되지 않습니다.
* 클러스터의 가상 네트워크에 대한 DNS 구성을 수정하지 마세요. 기본 Azure DNS 확인자를 사용해야 합니다.
* 어떤 방식으로든 클러스터의 MachineConfig 개체(예: kubelet 구성)를 재정의하지 마세요.
* UnsupportedConfigOverrides 옵션을 설정 하지 마세요. 이러한 옵션을 설정 하면 부 버전 업그레이드를 수행할 수 없습니다.
* Azure Red Hat OpenShift 서비스는 Private Link Service를 통해 클러스터에 액세스합니다.  서비스 액세스를 제거하거나 수정하지 마세요.
* 비 RHCOS 컴퓨팅 노드는 지원되지 않습니다. 예를 들어 RHEL 컴퓨팅 노드를 사용할 수 없습니다.

## <a name="supported-virtual-machine-sizes"></a>지원되는 가상 머신 크기

Azure Red Hat OpenShift 4는 다음 가상 머신 크기의 작업자 노드 인스턴스를 지원합니다.

### <a name="general-purpose"></a>범용 가상 컴퓨터

|계열|크기|vCPU|메모리: GiB|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>메모리에 최적화

|계열|크기|vCPU|메모리: GiB|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>컴퓨팅 최적화

|계열|크기|vCPU|메모리: GiB|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|

### <a name="master-nodes"></a>마스터 노드

|계열|크기|vCPU|메모리: GiB|
|-|-|-|-|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|
