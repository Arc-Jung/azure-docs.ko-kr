---
title: Azure Migrate를 사용 하 여 Azure로의 마이그레이션에 대 한 많은 수의 VMware Vm 평가
description: Azure Migrate 서비스를 사용 하 여 Azure로 마이그레이션하기 위해 많은 수의 VMware Vm을 평가 하는 방법을 설명 합니다.
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 0be7a7ea4afc400787456533689fe00b1db1c116
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96492933"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Azure로의 마이그레이션에 대 한 많은 수의 VMware Vm 평가


이 문서에서는 Azure Migrate Server 평가 도구를 사용 하 여 Azure로 마이그레이션하기 위한 온-프레미스 VMware Vm의 많은 숫자 (1000-35000)를 평가 하는 방법을 설명 합니다.

[Azure Migrate](migrate-services-overview.md)는 앱, 인프라 및 워크로드를 검색, 평가 및 Microsoft Azure로 마이그레이션하는 데 도움이 되는 도구의 허브를 제공합니다. 허브에는 Azure Migrate 도구와 타사 ISV(독립 소프트웨어 공급업체) 제품이 포함되어 있습니다. 

이 문서에서는 다음 방법을 설명합니다.
> [!div class="checklist"]
> * 규모에 대 한 평가 계획.
> * Azure 사용 권한을 구성 하 고 평가를 위해 VMware를 준비 합니다.
> * Azure Migrate 프로젝트를 만들고 평가를 만듭니다.
> * 마이그레이션을 계획할 때 평가를 검토 합니다.


> [!NOTE]
> 규모를 평가 하기 전에 몇 가지 Vm을 평가 하는 개념 증명을 사용해 보려면 [자습서 시리즈](./tutorial-discover-vmware.md) 를 따르세요.

## <a name="plan-for-assessment"></a>평가 계획

많은 수의 VMware Vm에 대 한 평가를 계획할 때 고려해 야 할 몇 가지 사항이 있습니다.

- **Azure Migrate 프로젝트 계획**: Azure Migrate 프로젝트를 배포 하는 방법을 파악 합니다. 예를 들어 데이터 센터가 다른 지역에 있거나 검색, 평가 또는 마이그레이션 관련 메타 데이터를 다른 지리에 저장 해야 하는 경우 여러 프로젝트가 필요할 수 있습니다. 
- **계획 어플라이언스**: Azure Migrate는 VMware vm으로 배포 된 온-프레미스 Azure Migrate 어플라이언스를 사용 하 여 vm을 지속적으로 검색 합니다. 어플라이언스는 Vm, 디스크 또는 네트워크 어댑터를 추가 하는 등의 환경 변경을 모니터링 합니다. 또한 Azure에 대 한 메타 데이터 및 성능 데이터를 전송 합니다. 배포 해야 하는 어플라이언스 수를 파악 해야 합니다.
- **검색을 위한 계정 계획**: Azure Migrate 어플라이언스는 평가 및 마이그레이션을 위해 vm을 검색 하기 위해 vCenter Server에 대 한 액세스 권한이 있는 계정을 사용 합니다. 1만 개 보다 많은 Vm을 검색 하는 경우, 필요한 경우 여러 계정을 설정 합니다 .이는 프로젝트의 두 어플라이언스에서 검색 된 Vm 간에 중복 되지 않습니다. 

> [!NOTE]
> 여러 어플라이언스를 설정 하는 경우 제공 된 vCenter 계정에 있는 Vm 간에 겹치지 않도록 해야 합니다. 이러한 겹치는 검색은 지원되지 않는 시나리오입니다. 하나 이상의 어플라이언스에서 VM을 검색 하는 경우이로 인해 검색 및 문제가 중복 되며 서버 마이그레이션의 Azure Portal를 사용 하 여 VM에 대 한 복제를 사용 하도록 설정할 수 있습니다.

## <a name="planning-limits"></a>제한 계획
 
계획을 위해이 표에 요약 된 제한을 사용 합니다.

**계획** | **제한**
--- | --- 
**Azure Migrate 프로젝트** | 프로젝트에서 최대 35000 개의 Vm을 평가 합니다.
**Azure Migrate 어플라이언스** | 어플라이언스는 vCenter Server에서 최대 1만 개의 Vm을 검색할 수 있습니다.<br/> 어플라이언스는 단일 vCenter Server에만 연결할 수 있습니다.<br/> 어플라이언스는 단일 Azure Migrate 프로젝트에만 연결할 수 있습니다.<br/>  모든 수의 어플라이언스를 단일 Azure Migrate 프로젝트에 연결할 수 있습니다. <br/><br/> 
**그룹** | 단일 그룹에서 최대 35000 개의 Vm을 추가할 수 있습니다.
**Azure Migrate 평가** | 단일 평가에서 최대 35,000개의 VM을 평가할 수 있습니다.

이러한 한도를 염두에 두면 몇 가지 예 배포를 고려해 야 합니다.


**vCenter 서버** | **서버의 Vm** | **권장** | **작업**
---|---|---|---
하나 | < 1만 | Azure Migrate 프로젝트 하나<br/> 어플라이언스 하나<br/> 검색을 위한 vCenter 계정이 하나 있습니다. | 어플라이언스를 설정 하 고 계정을 사용 하 여 vCenter Server에 연결 합니다.
하나 | > 1만 | Azure Migrate 프로젝트 하나<br/> 여러 어플라이언스.<br/> 여러 vCenter 계정. | 1만 Vm 마다 어플라이언스를 설정 합니다.<br/><br/> VCenter 계정을 설정 하 고, 계정에 대 한 액세스를 1만 Vm 미만으로 제한 하도록 인벤토리를 나눕니다.<br/> 계정을 사용 하 여 각 어플라이언스를 vCenter server에 연결 합니다.<br/> 다른 어플라이언스로 검색 된 컴퓨터 간에 종속성을 분석할 수 있습니다. <br/> <br/> 제공 된 vCenter 계정에 있는 Vm 간에 중복이 없는지 확인 합니다. 이러한 겹치는 검색은 지원되지 않는 시나리오입니다. 하나 이상의 어플라이언스에서 VM을 검색 하는 경우이로 인해 검색에서 중복이 발생 하 고, 서버 마이그레이션의 Azure Portal를 사용 하 여 VM에 대 한 복제를 사용 하도록 설정 하는 데 문제가 발생 합니다.
여러 | < 1만 |  Azure Migrate 프로젝트 하나<br/> 여러 어플라이언스.<br/> 검색을 위한 vCenter 계정이 하나 있습니다. | 어플라이언스를 설정 하 고 계정을 사용 하 여 vCenter Server에 연결 합니다.<br/> 다른 어플라이언스로 검색 된 컴퓨터 간에 종속성을 분석할 수 있습니다.
여러 | > 1만 | Azure Migrate 프로젝트 하나<br/> 여러 어플라이언스.<br/> 여러 vCenter 계정. | 1만 Vm < vCenter Server 경우 각 vCenter Server에 대 한 어플라이언스를 설정 합니다.<br/><br/> 1만 Vm > vCenter Server 경우 1만 Vm 마다 어플라이언스를 설정 합니다.<br/> VCenter 계정을 설정 하 고, 계정에 대 한 액세스를 1만 Vm 미만으로 제한 하도록 인벤토리를 나눕니다.<br/> 계정을 사용 하 여 각 어플라이언스를 vCenter server에 연결 합니다.<br/> 다른 어플라이언스로 검색 된 컴퓨터 간에 종속성을 분석할 수 있습니다. <br/><br/> 제공 된 vCenter 계정에 있는 Vm 간에 중복이 없는지 확인 합니다. 이러한 겹치는 검색은 지원되지 않는 시나리오입니다. 하나 이상의 어플라이언스에서 VM을 검색 하는 경우이로 인해 검색에서 중복이 발생 하 고, 서버 마이그레이션의 Azure Portal를 사용 하 여 VM에 대 한 복제를 사용 하도록 설정 하는 데 문제가 발생 합니다.



## <a name="plan-discovery-in-a-multi-tenant-environment"></a>다중 테 넌 트 환경에서 검색 계획

다중 테 넌 트 환경을 계획 하는 경우 vCenter Server 검색 범위를 지정할 수 있습니다.

- 어플라이언스 검색 범위는 vCenter Server 데이터 센터, 클러스터 또는 클러스터, 호스트 또는 호스트의 폴더 또는 개별 Vm으로 설정할 수 있습니다.
- 사용자 환경이 테 넌 트 간에 공유 되 고 각 테 넌 트를 개별적으로 검색 하려는 경우 어플라이언스에서 검색에 사용 하는 vCenter 계정에 대 한 액세스 범위를 지정할 수 있습니다. 
    - 테 넌 트가 호스트를 공유 하는 경우 VM 폴더로 범위를 지정할 수 있습니다. Vcenter 계정에 vCenter VM 폴더 수준에서 액세스 권한이 부여 된 경우 Vm을 검색할 수 Azure Migrate. 검색 범위를 VM 폴더로 지정하려는 경우 vCenter 계정에 VM 수준의 읽기 전용 액세스 권한이 할당되도록 하여 검색을 수행할 수 있습니다. [자세히 알아보기](set-discovery-scope.md).

## <a name="prepare-for-assessment"></a>평가 준비

서버 평가를 위해 Azure 및 VMware를 준비 합니다. 

1. [VMware 지원 요구 사항 및 제한 사항을](migrate-support-matrix-vmware.md)확인 합니다.
2. Azure Migrate와 상호 작용 하도록 Azure 계정에 대 한 사용 권한을 설정 합니다.
3. 평가를 위해 VMware를 준비 합니다.

[이 자습서](./tutorial-discover-vmware.md) 의 지침에 따라 이러한 설정을 구성 합니다.


## <a name="create-a-project"></a>프로젝트 만들기

계획 요구 사항에 따라 다음을 수행 합니다.

1. Azure Migrate 프로젝트를 만듭니다.
2. 프로젝트에 Azure Migrate 서버 평가 도구를 추가 합니다.

[자세한 정보](./create-manage-projects.md)

## <a name="create-and-review-an-assessment"></a>평가 만들기 및 검토

1. VMware Vm에 대 한 평가를 만듭니다.
1. 마이그레이션 계획 준비에 대 한 평가를 검토 합니다.


[이 자습서](./tutorial-assess-vmware-azure-vm.md) 의 지침에 따라 이러한 설정을 구성 합니다.
    

## <a name="next-steps"></a>다음 단계

이 문서에서는 다음 작업을 수행합니다.
 
> [!div class="checklist"] 
> * VMware Vm에 대 한 Azure Migrate 평가를 확장할 계획
> * 평가를 위해 준비 된 Azure 및 VMware
> * Azure Migrate 프로젝트를 만들고 평가를 실행 했습니다.
> * 마이그레이션 준비 과정에서 평가를 검토 했습니다.

이제 평가를 계산 하는 방법 및 [평가를 수정](how-to-modify-assessment.md)하는 방법을 [알아봅니다](concepts-assessment-calculation.md) .