---
title: 가상 네트워크 격리 및 보안 개요
titleSuffix: Azure Machine Learning
description: 격리 된 Azure Virtual Network Azure Machine Learning와 함께 사용 하 여 작업 영역 리소스 및 계산 환경을 보호 합니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: peterlu
author: peterclu
ms.date: 03/02/2021
ms.topic: conceptual
ms.custom: how-to, devx-track-python, references_regions, contperf-fy21q1
ms.openlocfilehash: 1c3d9b286a8262efa126ba9c661c50dd88e78b64
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103573475"
---
# <a name="virtual-network-isolation-and-privacy-overview"></a>가상 네트워크 격리 및 개인 정보 개요

이 문서에서는 Vnet (가상 네트워크)를 사용 하 여 Azure Machine Learning에서 네트워크 통신을 보호 하는 방법에 대해 알아봅니다. 이 문서에서는 예제 시나리오를 사용 하 여 전체 가상 네트워크를 구성 하는 방법을 보여 줍니다.

이 문서는 Azure Machine Learning 워크플로를 보호 하는 과정을 안내 하는 다섯 부분으로 구성 된 시리즈 중 하나입니다. 개념을 먼저 이해 하려면이 개요 문서를 읽어 보는 것이 좋습니다. 

이 시리즈의 다른 문서는 다음과 같습니다.

**1. VNet 개요**  >  [2. 작업 영역 3을 보호](how-to-secure-workspace-vnet.md)합니다  >  [. 학습 환경 4를 안전 하 게 보호](how-to-secure-training-vnet.md)합니다  >  [. 추론 환경 5를 보호](how-to-secure-inferencing-vnet.md)합니다  >  [. 스튜디오 기능 사용](how-to-enable-studio-virtual-network.md)

## <a name="prerequisites"></a>필수 조건

이 문서에서는 다음 항목에 대해 잘 알고 있다고 가정 합니다.
+ [Azure 가상 네트워크](../virtual-network/virtual-networks-overview.md)
+ [IP 네트워킹](../virtual-network/public-ip-addresses.md)
+ [Azure Private Link](how-to-configure-private-link.md)
+ [NSG (네트워크 보안 그룹)](../virtual-network/network-security-groups-overview.md)
+ [네트워크 방화벽](../firewall/overview.md)
## <a name="example-scenario"></a>예제 시나리오

이 섹션에서는 개인 IP 주소를 사용 하 여 Azure Machine Learning 통신을 보호 하기 위해 일반적인 네트워크 시나리오를 설정 하는 방법에 대해 알아봅니다.

아래 표에서는 서비스에서 vnet과 vnet을 사용 하지 않고 Azure Machine Learning 네트워크의 서로 다른 부분에 액세스 하는 방법을 비교 합니다.

| 시나리오 | 작업 영역 | 연결 된 리소스 | 계산 환경 학습 | 추론 계산 환경 |
|-|-|-|-|-|-|
|**가상 네트워크 없음**| 공용 IP | 공용 IP | 공용 IP | 공용 IP |
|**가상 네트워크의 리소스 보안**| 개인 IP (개인 끝점) | 공용 IP (서비스 끝점) <br> **디스크나** <br> 개인 IP (개인 끝점) | 프라이빗 IP | 프라이빗 IP  | 

* **작업 영역** -VNet에서 개인 끝점을 만들어 작업 영역의 개인 링크에 연결 합니다. 개인 끝점은 여러 개인 IP 주소를 통해 vnet에 작업 영역을 연결 합니다.
* **연결 된 리소스** -서비스 끝점 또는 개인 끝점을 사용 하 여 azure storage, Azure Key Vault 및 Azure Container service와 같은 작업 영역 리소스에 연결 합니다.
    * **서비스 끝점** 은 Azure 서비스에 대 한 가상 네트워크의 id를 제공 합니다. 가상 네트워크에서 서비스 끝점을 사용 하도록 설정 하면 가상 네트워크 규칙을 추가 하 여 가상 네트워크에 대 한 Azure 서비스 리소스를 보호할 수 있습니다. 서비스 끝점은 공용 IP 주소를 사용 합니다.
    * **개인 끝점** 은 Azure 개인 링크를 통해 구동 되는 서비스에 안전 하 게 연결 하는 네트워크 인터페이스입니다. 개인 끝점은 VNet의 개인 IP 주소를 사용 하 여 서비스를 VNet에 효과적으로 제공 합니다.
* **계산 액세스 교육** -개인 IP 주소를 사용 하 여 Azure Machine Learning 계산 인스턴스와 같은 계산 대상과 안전 하 게 계산 클러스터 Azure Machine Learning를 학습 합니다. 
* **추론 계산 액세스** -개인 IP 주소를 사용 하 여 AKS (Azure Kubernetes Services) 계산 클러스터에 액세스 합니다.


다음 5 개 섹션에서는 위에서 설명한 네트워크 시나리오를 보호 하는 방법을 보여 줍니다. 네트워크를 보호 하려면 다음을 수행 해야 합니다.

1. [**작업 영역 및 연결 된 리소스**](#secure-the-workspace-and-associated-resources)를 보호 합니다.
1. [**학습 환경을**](#secure-the-training-environment)보호 합니다.
1. [**추론 환경을**](#secure-the-inferencing-environment)보호 합니다.
1. 선택적으로: [**studio 기능을 사용 하도록 설정**](#optional-enable-studio-functionality)합니다.
1. [**방화벽 설정을**](#configure-firewall-settings)구성 합니다.
1. [DNS 이름 확인](#custom-dns)을 구성 합니다.
## <a name="secure-the-workspace-and-associated-resources"></a>작업 영역 및 연결 된 리소스를 보호 합니다.

작업 영역 및 관련 리소스를 보호 하려면 다음 단계를 사용 합니다. 이러한 단계를 통해 서비스가 가상 네트워크에서 통신할 수 있습니다.

1. VNet과 작업 영역 간에 통신을 사용 하도록 설정 하려면 [개인 링크 사용 작업 영역](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint) 을 만듭니다.
1. __서비스 끝점이__ 나 __개인 끝점__ _을 사용 하 여 가상_ 네트워크에 다음 서비스를 추가 합니다. 또한 신뢰할 수 있는 Microsoft 서비스에서 다음 서비스에 액세스할 수 있도록 허용 해야 합니다.
    
    | 서비스 | 엔드포인트 정보 | 신뢰할 수 있는 정보 허용 |
    | ----- | ----- | ----- |
    | __Azure Key Vault__| [서비스 끝점](../key-vault/general/overview-vnet-service-endpoints.md)</br>[개인 끝점](../key-vault/general/private-link-service.md) | [신뢰할 수 있는 Microsoft 서비스에서이 방화벽을 우회 하도록 허용](how-to-secure-workspace-vnet.md#secure-azure-key-vault) |
    | __Azure Storage 계정__ | [서비스 끝점](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints)</br>[개인 끝점](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints) | [신뢰할 수 있는 Azure 서비스에 대 한 액세스 권한 부여](../storage/common/storage-network-security.md#grant-access-to-trusted-azure-services) |
    | __Azure Container Registry__ | [서비스 끝점](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr)</br>[개인 끝점](../container-registry/container-registry-private-link.md) | [신뢰할 수 있는 서비스 허용](../container-registry/allow-access-trusted-services.md) |


![작업 영역 및 연결 된 리소스가 VNet 내부의 서비스 끝점 또는 개인 끝점을 통해 서로 통신 하는 방법을 보여 주는 아키텍처 다이어그램](./media/how-to-network-security-overview/secure-workspace-resources.png)

이러한 단계를 완료 하는 방법에 대 한 자세한 지침은 [Azure Machine Learning 작업 영역 보안](how-to-secure-workspace-vnet.md)설정을 참조 하세요. 

### <a name="limitations"></a>제한 사항

가상 네트워크 내에서 작업 영역 및 연결 된 리소스를 보호 하는 경우 다음과 같은 제한 사항이 있습니다.
- 개인 링크로 Azure Machine Learning 작업 영역을 사용 하는 것은 Azure Government 또는 Azure 중국 21Vianet 지역에서 사용할 수 없습니다.
- 모든 리소스는 동일한 VNet 뒤에 있어야 합니다. 그러나 동일한 VNet 내의 서브넷은 허용 됩니다.

## <a name="secure-the-training-environment"></a>교육 환경 보안

이 섹션에서는 Azure Machine Learning에서 학습 환경을 보호 하는 방법에 대해 알아봅니다. 또한 Azure Machine Learning 학습 작업을 완료 하 여 네트워크 구성이 함께 작동 하는 방식을 이해 하는 방법을 알아봅니다.

학습 환경을 보호 하려면 다음 단계를 사용 합니다.

1. [가상 네트워크에 Azure Machine Learning 계산 인스턴스 및 컴퓨터 클러스터](how-to-secure-training-vnet.md#compute-instance) 를 만들어 학습 작업을 실행 합니다.
1. Batch 서비스가 계산 리소스에 작업을 제출할 수 있도록 [Azure Batch 서비스에서 인바운드 통신을 허용](how-to-secure-training-vnet.md#mlcports) 합니다. 

![관리 되는 계산 클러스터 및 인스턴스를 보호 하는 방법을 보여 주는 아키텍처 다이어그램](./media/how-to-network-security-overview/secure-training-environment.png)

이러한 단계를 완료 하는 방법에 대 한 자세한 지침은 [학습 환경 보호](how-to-secure-training-vnet.md)를 참조 하세요. 

### <a name="example-training-job-submission"></a>학습 작업 제출 예 

이 섹션에서는 Azure Machine Learning 하 여 교육 작업을 제출 하는 서비스 간에 안전 하 게 통신 하는 방법에 대해 알아봅니다. 모든 구성이 함께 작동 하 여 통신을 보호 하는 방법을 보여 줍니다.

1. 클라이언트는 서비스 또는 개인 끝점으로 보안이 유지 되는 저장소 계정에 학습 스크립트 및 학습 데이터를 업로드 합니다.

1. 클라이언트는 개인 끝점을 통해 Azure Machine Learning 작업 영역에 학습 작업을 제출 합니다.

1. Azure Batch 서비스는 작업 영역에서 작업을 수신 하 고 계산 리소스로 프로 비전 된 공용 부하 분산 장치를 통해 학습 작업을 계산 환경으로 전송 합니다. 

1. 계산 리소스는 작업을 받고 학습을 시작 합니다. 계산 리소스는 보안 저장소 계정에 액세스 하 여 학습 파일을 다운로드 하 고 출력을 업로드 합니다.

### <a name="limitations"></a>제한 사항

- Azure 계산 인스턴스 및 Azure 계산 클러스터는 작업 영역 및 연결 된 리소스와 동일한 VNet, 지역 및 구독에 있어야 합니다. 

## <a name="secure-the-inferencing-environment"></a>추론 환경 보안

이 섹션에서는 추론 환경을 보호 하는 데 사용할 수 있는 옵션에 대해 알아봅니다. 높은 규모의 프로덕션 배포에는 AKS (Azure Kubernetes Services) 클러스터를 사용 하는 것이 좋습니다.

가상 네트워크에서 AKS 클러스터에 대 한 두 가지 옵션을 사용할 수 있습니다.

- VNet에 기본 AKS 클러스터를 배포 하거나 연결 합니다.
- 개인 AKS 클러스터를 VNet에 연결 합니다.

**기본 AKS 클러스터** 에는 공용 IP 주소를 사용 하는 제어 평면이 있습니다. 배포 하는 동안 VNet에 기본 AKS 클러스터를 추가 하거나 클러스터를 만든 후에 연결할 수 있습니다.

**개인 AKS 클러스터** 에는 개인 ip를 통해서만 액세스할 수 있는 제어 평면이 있습니다. 클러스터를 만든 후 개인 AKS 클러스터를 연결 해야 합니다.

기본 및 개인 클러스터를 추가 하는 방법에 대 한 자세한 지침은 [추론 환경 보안](how-to-secure-inferencing-vnet.md)을 참조 하세요. 

다음 네트워크 다이어그램에서는 개인 AKS 클러스터가 가상 네트워크에 연결 된 보안 Azure Machine Learning 작업 영역을 보여 줍니다.

![개인 AKS 클러스터를 가상 네트워크에 연결 하는 방법을 보여 주는 아키텍처 다이어그램 AKS 컨트롤 평면은 고객 VNet 외부에 배치 됩니다.](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>제한 사항
- AKS 클러스터는 작업 영역 및 연결 된 리소스와 동일한 VNet에 속해야 합니다. 

## <a name="optional-enable-public-access"></a>선택 사항: 공용 액세스 사용

개인 끝점을 사용 하 여 VNet 뒤의 작업 영역에 보안을 설정 하 고 공용 인터넷을 통한 액세스를 계속 허용할 수 있습니다. 초기 구성은 [작업 영역 및 관련 리소스를 보호](#secure-the-workspace-and-associated-resources)하는 것과 같습니다. 

개인 링크를 사용 하 여 작업 영역을 보호 한 후에 [공용 액세스를 사용 하도록 설정](how-to-configure-private-link.md#enable-public-access)합니다. 그러면 공용 인터넷과 VNet 모두에서 작업 영역에 액세스할 수 있습니다.

### <a name="limitations"></a>제한 사항

- 공용 인터넷을 통해 Azure Machine Learning studio를 사용 하는 경우 디자이너와 같은 일부 기능에서 데이터에 액세스 하지 못할 수 있습니다. 이 문제는 VNet 뒤에서 보호 되는 서비스에 데이터를 저장할 때 발생 합니다. 예를 들어 Azure Storage 계정이 있습니다.
## <a name="optional-enable-studio-functionality"></a>선택 사항: studio 기능 사용

[작업 영역 보안](#secure-the-workspace-and-associated-resources)  >  [교육 환경 보안](#secure-the-training-environment)  >  [추론 환경 보안](#secure-the-inferencing-environment)  >  **스튜디오 기능 사용**  >  [방화벽 설정 구성](#configure-firewall-settings)

저장소가 VNet에 있는 경우 먼저 추가 구성 단계를 수행 하 여 [스튜디오](overview-what-is-machine-learning-studio.md)에서 전체 기능을 사용할 수 있도록 해야 합니다. 기본적으로 다음 기능을 사용할 수 없습니다.

* Studio에서 데이터를 미리 봅니다.
* 디자이너에서 데이터를 시각화 합니다.
* 디자이너에서 모델을 배포 합니다.
* AutoML 실험을 제출 합니다.
* 레이블 지정 프로젝트를 시작 합니다.

VNet 내부에서 전체 studio 기능을 사용 하도록 설정 하려면 [가상 네트워크에서 Azure Machine Learning Studio 사용](how-to-enable-studio-virtual-network.md#configure-data-access-in-the-studio)을 참조 하세요. 스튜디오는 서비스 끝점이 나 개인 끝점을 사용 하 여 저장소 계정을 지원 합니다.

### <a name="limitations"></a>제한 사항

[ML 지원 데이터 레이블](how-to-create-labeling-projects.md#use-ml-assisted-data-labeling) 지정은 가상 네트워크 뒤에 보안 된 기본 저장소 계정을 지원 하지 않습니다. ML 지원 데이터 레이블 지정을 위해 기본이 아닌 저장소 계정을 사용 해야 합니다. 기본이 아닌 저장소 계정은 가상 네트워크 뒤에 보안을 설정할 수 있습니다. 

## <a name="configure-firewall-settings"></a>방화벽 설정 구성하기

Azure Machine Learning 작업 영역 리소스 및 공용 인터넷에 대 한 액세스를 제어 하도록 방화벽을 구성 합니다. Azure 방화벽을 권장 하는 동안 다른 방화벽 제품을 사용 하 여 네트워크를 보호할 수 있어야 합니다. 방화벽을 통한 통신을 허용 하는 방법에 대 한 질문이 있는 경우 사용 중인 방화벽에 대 한 설명서를 참조 하세요.

방화벽 설정에 대 한 자세한 내용은 [방화벽 뒤에 작업 영역 사용](how-to-access-azureml-behind-firewall.md)을 참조 하세요.

## <a name="custom-dns"></a>사용자 지정 DNS

가상 네트워크에 대 한 사용자 지정 DNS 솔루션을 사용 해야 하는 경우 작업 영역에 대 한 호스트 레코드를 추가 해야 합니다.

필요한 도메인 이름 및 IP 주소에 대 한 자세한 내용은 [사용자 지정 DNS 서버에서 작업 영역을 사용 하는 방법](how-to-custom-dns.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

이 문서는 5 부 가상 네트워크 시리즈의 1 부입니다. 가상 네트워크를 보호 하는 방법을 알아보려면 나머지 문서를 참조 하세요.

* [2 부: 가상 네트워크 개요](how-to-secure-workspace-vnet.md)
* [3 부: 학습 환경 보안](how-to-secure-training-vnet.md)
* [4 부: 추론 환경 보안](how-to-secure-inferencing-vnet.md)
* [5 부: studio 기능 사용](how-to-enable-studio-virtual-network.md)

또한 이름 확인에 [사용자 지정 DNS](how-to-custom-dns.md) 사용에 대 한 문서를 참조 하세요.