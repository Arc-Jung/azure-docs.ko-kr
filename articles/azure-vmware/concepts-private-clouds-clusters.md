---
title: 개념-사설 클라우드 및 클러스터
description: Azure VMware 솔루션 소프트웨어 정의 데이터 센터 및 vSphere 클러스터의 주요 기능에 대해 알아봅니다.
ms.topic: conceptual
ms.date: 03/13/2021
ms.openlocfilehash: aff66e01ae4b056eb082d2000611718b1a5cf66a
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104773926"
---
#  <a name="azure-vmware-solution-private-cloud-and-cluster-concepts"></a>Azure VMware 솔루션 사설 클라우드 및 클러스터 개념

Azure VMware 솔루션은 Azure에서 VMware 기반 사설 클라우드를 제공 합니다. 사설 클라우드에는 완전 한 운영 체제 미 설치 Azure 호스트로 빌드된 클러스터가 포함 됩니다. Azure Portal, CLI 또는 PowerShell을 통해 배포 되 고 관리 됩니다.  사설 클라우드에 프로 비전 된 클러스터에는 VMware vSphere, vCenter, vSAN 및 NSX 소프트웨어가 포함 됩니다. Azure VMware 솔루션 사설 클라우드 하드웨어 및 소프트웨어 배포는 Azure에서 완벽 하 게 통합 되 고 자동화 됩니다.

Azure 구독, Azure VMware 솔루션 사설 클라우드, vSAN 클러스터 및 호스트 간에는 논리적 관계가 있습니다. 이 다이어그램은 개발 및 프로덕션 환경을 나타내는 두 사설 클라우드를 포함 하는 단일 Azure 구독을 보여 줍니다.  각 사설 클라우드에서는 두 개의 클러스터가 있습니다. 

이 문서에서는 이러한 모든 개념을 설명 합니다.

![고객 구독의 두 사설 클라우드 이미지](./media/hosts-clusters-private-clouds-final.png)


## <a name="private-clouds"></a>프라이빗 클라우드

사설 클라우드에는 완전 한 운영 체제 미 설치 Azure 호스트로 빌드된 vSAN 클러스터가 포함 됩니다. 각 사설 클라우드에는 동일한 vCenter server 및 NSX Manager에서 관리 하는 여러 클러스터가 있을 수 있습니다. 포털, CLI 또는 PowerShell에서 사설 클라우드를 배포 하 고 관리할 수 있습니다. 

다른 리소스와 마찬가지로 사설 클라우드는 Azure 구독 내에서 설치 및 관리 됩니다. 구독 내의 사설 클라우드 수는 확장할 수 있습니다. 처음에는 구독 당 하나의 사설 클라우드로 제한 됩니다.

## <a name="clusters"></a>클러스터
만든 각 사설 클라우드에 대해 기본적으로 하나의 vSAN 클러스터가 있습니다. Azure Portal를 사용 하거나 API를 통해 클러스터를 추가, 삭제 및 크기를 조정할 수 있습니다.  모든 클러스터의 기본 크기는 세 개의 호스트로 구성 되며 최대 16 개의 호스트로 확장할 수 있습니다. 사설 클라우드 당 최대 4 개의 클러스터를 사용할 수 있습니다.

평가판 클러스터는 평가용으로 제공 되며 호스트 수는 세 개로 제한 됩니다. 사설 클라우드 당 단일 평가판 클러스터가 있습니다. 평가 기간 중 단일 호스트를 기준으로 평가판 클러스터의 크기를 조정할 수 있습니다.

VSphere NSX Manager를 사용 하 여 클러스터 구성 또는 작업의 다른 대부분의 측면을 관리 합니다. 클러스터의 각 호스트에 대 한 모든 로컬 저장소는 vSAN의 제어를 받고 있습니다.

## <a name="hosts"></a>호스트

Azure VMware 솔루션 클러스터는 하이퍼 수렴 형 운영 체제 미 설치 인프라를 기반으로 합니다. 다음 표에서는 호스트의 RAM, CPU 및 디스크 용량을 보여 줍니다.

| 호스트 유형              |             CPU             |   RAM(GB)   |  vSAN NVMe 캐시 계층 (TB, 원시)  |  vSAN SSD 용량 계층 (TB, 원시)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| AVS36          |  듀얼 Intel 18 코어 2.3 g h z  |     576      |                3.2               |                15.20               |

클러스터를 빌드하거나 크기를 조정 하는 데 사용 되는 호스트는 격리 된 호스트 풀에서 제공 됩니다. 이러한 호스트는 하드웨어 테스트를 통과 하 고 모든 데이터를 안전 하 게 삭제 했습니다. 

## <a name="vmware-software-versions"></a>VMware 소프트웨어 버전

[!INCLUDE [vmware-software-versions](includes/vmware-software-versions.md)]

## <a name="update-frequency"></a>업데이트 빈도

[!INCLUDE [vmware-software-update-frequency](includes/vmware-software-update-frequency.md)]

## <a name="host-maintenance-and-lifecycle-management"></a>호스트 유지 관리 및 수명 주기 관리

호스트 유지 관리 및 수명 주기 관리는 사설 클라우드 클러스터의 용량 또는 성능에 영향을 주지 않습니다.  자동화 된 호스트 유지 관리의 예로는 펌웨어 업그레이드, 하드웨어 복구 또는 교체 등이 있습니다.

Microsoft는 NSX Manager 및 NSX Edge와 같은 NSX-T 어플라이언스의 수명 주기 관리를 담당 합니다. Microsoft는 계층 0 게이트웨이를 만들고 North-South 라우팅을 사용 하는 것과 같은 네트워크 구성 부트스트래핑을 담당 합니다. NSX-T SDN 구성을 담당 하 고 있습니다. 예를 들어 네트워크 세그먼트, 분산 방화벽 규칙, 계층 1 게이트웨이 및 부하 분산 장치 등이 있습니다.

## <a name="backup-and-restoration"></a>백업 및 복원

사설 클라우드 vCenter 및 NSX 구성은 매시간 백업 일정에 있습니다.  백업은 3 일 동안 유지 됩니다. 백업에서 복원 해야 하는 경우 Azure Portal에서 [지원 요청](https://rc.portal.azure.com/#create/Microsoft.Support) 을 열어 복원을 요청 합니다.

## <a name="next-steps"></a>다음 단계

이제 Azure VMware 솔루션 사설 클라우드 개념을 살펴보았습니다. 다음에 대해 알아볼 수 있습니다. 

- [Azure VMware 솔루션 네트워킹 및 상호 연결과 개념](concepts-networking.md).
- [Azure VMware 솔루션 저장소 개념](concepts-storage.md).
- [Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법](enable-azure-vmware-solution.md)

<!-- LINKS - internal -->
[concepts-networking]: ./concepts-networking.md

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

