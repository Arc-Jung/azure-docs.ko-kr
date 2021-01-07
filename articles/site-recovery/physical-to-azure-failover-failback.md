---
title: Site Recovery를 사용 하 여 물리적 서버에 대 한 장애 조치 (failover) 및 장애 복구 설정
description: Azure Site Recovery를 사용한 재해 복구를 위해 물리적 서버를 Azure로 장애 조치(Failover)하고, 온-프레미스 사이트로 장애 복구(Failback)하는 방법에 대한 자세한 정보
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2019
ms.author: raynew
ms.openlocfilehash: 2994f68e4159c7c4aa7d82bef7a5891deb5055a0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017426"
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>복제된 물리적 서버를 Azure로 장애 조치(Failover) 및 장애 복구(Failback)

이 자습서에서는 [Azure Site Recovery](site-recovery-overview.md)를 사용 하 여 Azure에 복제 하는 온-프레미스 물리적 서버를 장애 조치 (failover) 하는 방법을 설명 합니다. 장애 조치 (failover) 한 후 Azure에서 온-프레미스 사이트로 장애 복구 (failback) 할 수 있습니다.

## <a name="before-you-start"></a>시작하기 전에

- 재해 복구의 장애 조치 (failover) 프로세스에 [대해 알아봅니다](failover-failback-overview.md) .
- 여러 컴퓨터를 장애 조치 (failover) 하려는 경우 복구 계획에서 컴퓨터를 함께 수집 하는 방법에 [대해 알아봅니다](recovery-plan-overview.md) .
- 전체 장애 조치 (failover)를 수행 하기 전에 [재해 복구 훈련](site-recovery-test-failover-to-azure.md) 을 실행 하 여 모든 것이 예상 대로 작동 하는지 확인 합니다.
- 장애 조치 (failover) 후 Azure Vm에 대 한 연결을 준비 하려면 [다음 지침](site-recovery-failover.md#prepare-to-connect-after-failover) 을 따르세요.



## <a name="run-a-failover"></a>장애 조치(Failover) 실행

### <a name="verify-server-properties"></a>서버 속성 확인

서버 속성을 확인하고 서버가 Azure VM에 대한 [Azure 요구 사항](vmware-physical-azure-support-matrix.md#replicated-machines)을 준수하는지 확인합니다.

1. **보호된 항목** 에서 **복제된 항목** 을 클릭하고 컴퓨터를 선택합니다.
2. **복제된 항목** 창에 컴퓨터 정보, 상태 및 최신 사용 가능한 복구 지점의 요약이 제공됩니다. 자세한 내용을 보려면 **속성** 을 클릭합니다.
3. **Compute 및 네트워크** 에서 Azure 이름, 리소스 그룹, 대상 크기, [가용성 집합](../virtual-machines/windows/tutorial-availability-sets.md)및 관리 디스크 설정을 수정할 수 있습니다.
4. 장애 조치(failover) 후 Azure VM이 배치될 네트워크/서브넷 및 할당되는 IP 주소를 포함한 네트워크 설정을 보고 수정할 수 있습니다.
5. **디스크** 에서 컴퓨터 운영 체제 및 데이터 디스크에 대한 정보를 볼 수 있습니다.

### <a name="fail-over-to-azure"></a>Azure로 장애 조치(failover)

1. **설정** > **복제된 항목** 에서 컴퓨터 > **장애 조치(Failover)** 를 클릭합니다.
2. **장애 조치(Failover)** 에서 장애 조치할 **복구 시점** 을 선택합니다. 다음 옵션 중 하나를 사용할 수 있습니다.
   - **최신**: 이 옵션은 먼저 Site Recovery로 전송된 모든 데이터를 처리합니다. 이 옵션은 장애 조치(failover) 후에 생성된 Azure VM은 장애 조치(failover)가 트리거되었을 때 Site Recovery로 복제된 모든 데이터를 보유하므로 가장 낮은 RPO(복구 지점 목표)를 제공합니다.
   - **가장 최근에 처리됨**: 이 옵션은 컴퓨터를 Site Recovery에서 처리된 최신 복구 지점으로 장애 조치(Failover)합니다. 이 옵션은 처리되지 않은 데이터를 처리하는 데 시간이 투입되지 않으므로 낮은 RTO(복구 시간 목표)를 제공합니다.
   - **최신 앱 일치**: 이 옵션은 컴퓨터를 Site Recovery에서 처리된 최신 앱 일치 복구 지점으로 장애 조치(Failover)합니다.
   - **사용자 지정**: 복구 지점을 지정합니다.

3. 장애 조치(Failover)를 트리거하기 전에 Site Recovery에서 원본 컴퓨터를 종료하려고 시도하는 경우 **장애 조치(Failover)를 시작하기 전에 컴퓨터를 종료합니다.** 를 선택합니다. 종료가 실패하더라도 장애 조치는 계속됩니다. **작업** 페이지에서 장애 조치 진행 상황 확인을 수행할 수 있습니다.
4. Azure VM에 연결할 준비가 된 경우 연결하고, 장애 조치(failover) 후에 유효성을 검사합니다.
5. 확인한 후 장애 조치(failover)를 **커밋** 합니다. 그러면 사용 가능한 복구 지점이 모두 삭제됩니다.

> [!WARNING]
> 진행 중인 장애 조치(Failover)는 취소하지 마세요. 장애 조치(Failover)를 시작하기 전에 컴퓨터 복제가 중지됩니다. 장애 조치(Failover)를 취소하면 중지되지만, 컴퓨터는 다시 복제하지 않습니다.
> 물리적 서버의 경우 추가 장애 조치(Failover) 처리를 완료하는 데 8~ 10분 정도 걸릴 수 있습니다.

## <a name="automate-actions-during-failover"></a>장애 조치 (failover) 중 작업 자동화

장애 조치 (failover) 중에 작업을 자동화할 수 있습니다. 이렇게 하려면 복구 계획에서 스크립트나 Azure automation runbook을 사용할 수 있습니다.

- 스크립트 추가를 포함 하 여 복구 계획을 만들고 사용자 지정 하는 방법에 [대해 알아봅니다](site-recovery-create-recovery-plans.md) .
- Azure Automation runbook을 복구 계획에 추가 하는 [방법에 대해 알아봅니다](site-recovery-runbook-automation.md) 섹션인.

## <a name="configure-settings-after-failover"></a>장애 조치 (failover) 후 설정 구성

장애 조치 (failover) 후 복제 된 Azure Vm에 연결 하도록 [azure 설정을 구성](site-recovery-failover.md#prepare-in-azure-to-connect-after-failover) 해야 합니다. 또한 [내부 및 공용](site-recovery-failover.md#set-up-ip-addressing) IP 주소 지정을 설정 합니다.

## <a name="prepare-for-reprotection-and-failback"></a>다시 보호 및 장애 복구 준비

Azure로 장애 조치 (failover) 한 후에는 Azure Vm을 온-프레미스 사이트에 복제 하 여 다시 보호 합니다. 그런 다음 복제 된 후에 Azure에서 온-프레미스 사이트로 장애 조치 (failover)를 실행 하 여 온-프레미스로 다시 장애 조치할 수 있습니다.

1. Site Recovery를 사용하여 Azure로 복제된 물리적 서버는 VMware VM으로만 장애 복구(Failback)할 수 있습니다. 장애 복구(Failback)하려면 VMware 인프라가 필요합니다. [이 문서의](vmware-azure-prepare-failback.md) 단계에 따라 Azure에서 프로세스 서버를 설정 하 고 온-프레미스 마스터 대상 서버를 설정 하 고 장애 복구 (failback)를 위해 사이트 간 VPN 또는 express 경로 개인 피어 링을 구성 하는 등 다시 보호 및 장애 복구 (failback)를 준비 합니다.
2. 온-프레미스 구성 서버가 실행 중이 고 Azure에 연결 되어 있는지 확인 합니다. Azure로 장애 조치 (failover) 하는 동안 온-프레미스 사이트에 액세스할 수 없으며 구성 서버가 사용할 수 없거나 종료 될 수 있습니다. 장애 복구(failback) 중에 VM이 구성 서버 데이터베이스에 있어야 합니다. 그렇지 않으면 장애 복구가 실패합니다.
3. 온-프레미스 마스터 대상 서버에서 스냅숏을 삭제 합니다. 스냅샷이 있는 경우 다시 보호가 작동하지 않습니다.  VM의 스냅샷은 다시 보호 작업 중에 자동으로 병합됩니다.
4. 다중 VM 일관성을 위해 복제 그룹으로 수집된 VM을 다시 보호하는 경우에는 모든 VM의 운영 체제(Windows 또는 Linux)가 동일한지 확인하고 배포하는 마스터 대상 서버의 운영 체제가 동일한지 확인합니다. 복제 그룹의 모든 VM이 동일한 마스터 대상 서버를 사용해야 합니다.
5. 장애 복구(failback)에 [필요한 포트](vmware-azure-prepare-failback.md#ports-for-reprotectionfailback)를 엽니다.
6. 장애 복구(failback) 전에 vCenter Server가 연결되어 있는지 확인합니다. 연결되지 않은 경우 디스크의 연결을 끊고 가상 머신을 다시 연결하는 작업이 실패합니다.
7. vCenter Server에서 장애 복구(failback)하려는 VM을 관리하는 경우 필요한 권한이 있는지 확인합니다. 읽기 전용 사용자 vCenter 검색을 수행하고 가상 머신을 보호하면 보호에 성공하고 장애 조치가 작동합니다. 그러나 다시 보호 중에는 데이터 저장소를 검색할 수 없기 때문에 장애 조치(failover)가 실패하고, 다시 보호 중에는 나열되지 않습니다. 이 문제를 해결하려면 vCenter 자격 증명을 [적절한 계정/권한](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)으로 업데이트한 다음, 작업을 다시 시도하면 됩니다. 
8. 템플릿을 사용하여 가상 머신을 만든 경우 각 VM에 디스크에 대한 고유의 UUID가 있는지 확인합니다. 둘 다 동일한 템플릿에서 만들어졌으므로 온-프레미스 VM UUID가 마스터 대상 서버의 UUID와 충돌 하는 경우 다시 보호가 실패 합니다. 다른 템플릿에서 배포하세요.
9. 대체 vCenter Server로 장애 복구(failback)하려는 경우 새 vCenter Server 및 마스터 대상 서버가 검색되는지 확인합니다. 검색되지 않는 경우 일반적으로 데이터 저장소에 액세스할 수 없거나 **다시 보호** 에 표시되지 않습니다.
10. 장애 복구(failback)할 수 없는 다음 시나리오를 확인합니다.
    - ESXi 5.5 체험판 버전 또는 vSphere 6 Hypervisor 체험판 버전을 사용 중인 경우. 다른 버전으로 업그레이드하세요.
    - Windows Server 2008 R2 SP1 물리적 서버가 있는 경우.
    - VM이 마이그레이션되었습니다.
    - VM이 다른 리소스 그룹으로 이동되었습니다.
    - 복제본 Azure VM이 삭제되었습니다.
    - 보호 되지 않는 복제본 Azure VM (온-프레미스 사이트에 복제)
10. 사용 가능한 [장애 복구(failback) 유형](concepts-types-of-failback.md) - 원래 위치 복구 및 대체 위치 복구를 검토합니다.


## <a name="reprotect-azure-vms-to-an-alternate-location"></a>대체 위치에 Azure Vm 다시 보호

이 절차에서는 온-프레미스 VM을 사용할 수 없는 것으로 가정 합니다.

1. 자격 증명 모음 > **설정**  >  **복제 된 항목** 에서 장애 조치 (failover) 된 컴퓨터를 마우스 오른쪽 단추로 클릭 하 > **다시 보호** 합니다.
2. **다시 보호** 에서 **Azure - 온-프레미스** 를 선택합니다.
3. 온-프레미스 마스터 대상 서버 및 프로세스 서버를 지정합니다.
4. **데이터 저장소** 에서 온-프레미스에서 디스크를 복구하려는 마스터 대상 데이터 저장소를 선택합니다.
       - 온-프레미스 VM이 삭제 되었거나 존재 하지 않으며 새 디스크를 만들어야 하는 경우이 옵션을 사용 합니다.
       - 디스크가 이미 존재 하는 경우에는이 설정이 무시 되지만 값을 지정 해야 합니다.
5. 마스터 대상 보존 드라이브를 선택합니다. 장애 복구 정책이 자동으로 선택됩니다.
6. **확인** 을 클릭하여 다시 보호를 시작합니다. 작업은 Azure VM을 온-프레미스 사이트에 복제 하기 시작 합니다. **작업** 탭에서 진행률을 추적할 수 있습니다.

> [!NOTE]
> Azure VM을 기존 온-프레미스 VM으로 복구하려면 마스터 대상 서버의 ESXi 호스트에 읽기/쓰기 액세스 권한으로 온-프레미스 VM의 데이터 저장소를 탑재합니다.


## <a name="fail-back-from-azure"></a>Azure에서 장애 복구

다음과 같이 장애 조치(Failover)를 실행합니다.

1. **복제된 항목** 페이지에서 컴퓨터를 마우스 오른쪽 단추로 클릭한 다음 **계획되지 않은 장애 조치(Failover)** 를 선택합니다.
2. **장애 조치(Failover) 확인** 에서 장애 조치(Failover) 방향이 Azure부터인지 확인합니다.
3. 장애 조치 (failover)에 사용할 복구 지점을 선택 합니다.
    - **최신** 복구 지점을 사용하는 것이 좋습니다. 앱 일치 시점이 최신 지점보다 늦으면 데이터 손실이 발생합니다.
    - **최신** 은 크래시 일치 복구 지점입니다.
    - 장애 조치(Failover)를 실행하면 Site Recovery가 Azure VM을 종료하고 온-프레미스 VM을 부팅합니다. 일정 시간 동안 가동 중지되므로 적절한 시간을 선택합니다.
4. 컴퓨터를 마우스 오른쪽 단추로 클릭하고 **커밋** 을 클릭합니다. 그러면 Azure VM을 제거하는 작업이 트리거됩니다.
5. Azure VM이 예상대로 종료되었는지 확인합니다.


## <a name="reprotect-on-premises-machines-to-azure"></a>Azure로 온-프레미스 컴퓨터 다시 보호

이제 데이터가 온-프레미스 사이트에서 다시 복구되지만 Azure로 복제되지는 않습니다. 다음과 같이 Azure로의 복제를 다시 시작할 수 있습니다.

1. 자격 증명 모음 > **설정** >**복제된 항목** 에서 장애 복구(Failback)된 VM을 선택하고 **다시 보호** 를 클릭합니다.
2. Azure로 복제된 데이터를 전송하는 데 사용되는 프로세스 서버를 선택하고 **확인** 을 클릭합니다.


## <a name="next-steps"></a>다음 단계

다시 보호 작업이 완료 되 면 온-프레미스 VM이 Azure에 복제 됩니다. 필요에 따라 Azure로 [또 다른 장애 조치(failover)를 실행](site-recovery-failover.md)할 수 있습니다.
