---
title: Recovery Services 자격 증명 모음 개요
description: Recovery Services 자격 증명 모음 및 Azure Backup 자격 증명 모음 간의 개요 및 비교입니다.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: a0dacd82b7cf4258c0147bbaf9dc39ee6fc0fa25
ms.sourcegitcommit: acc558d79d665c8d6a5f9e1689211da623ded90a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82597956"
---
# <a name="recovery-services-vaults-overview"></a>Recovery Services 자격 증명 모음 개요

이 문서에서는 Recovery Services 자격 증명 모음의 기능을 설명합니다. Recovery Services 자격 증명 모음은 Azure에서 데이터를 저장하는 스토리지 엔터티입니다. 데이터는 일반적으로 데이터의 복사본 또는 가상 머신(VM), 워크로드, 서버 또는 워크스테이션에 대한 구성 정보입니다. Recovery Services 자격 증명 모음을 사용하여 IaaS VM(Linux 또는 Windows) 및 Azure SQL 데이터베이스와 같은 다양한 Azure 서비스에 대한 백업 데이터를 저장할 수 있습니다. Recovery Services 자격 증명 모음은 System Center DPM, Windows Server, Azure Backup Server 등을 지원합니다. Recovery Services 자격 증명 모음을 사용하면 관리 오버헤드를 최소화하면서 백업 데이터를 쉽게 구성할 수 있습니다.

Azure 구독 내에서 지역당 구독당 최대 500개의 Recovery Services 자격 증명 모음을 만들 수 있습니다.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Recovery Services 자격 증명 모음 및 Backup 자격 증명 모음 비교

백업 자격 증명 모음이 있는 경우 Recovery Services 자격 증명 모음으로 자동 업그레이드 됩니다. 2017년 11월까지 모든 Backup 자격 증명 모음은 Recovery Services 자격 증명 모음으로 업그레이드됩니다.

Recovery Services 자격 증명은 Azure의 Azure Resource Manager 모델을 기준으로 하지만 Backup 자격 증명 모음은 Azure 서비스 관리자 모델을 기준으로 합니다. Backup 자격 증명 모음을 Recovery Services 자격 증명 모음으로 업그레이드할 때 업그레이드 프로세스 전후의 백업 데이터는 그대로 유지됩니다. Recovery Services 자격 증명 모음은 다음과 같은 Backup 자격 증명 모음에 사용할 수 없는 기능을 제공합니다.

- **백업 데이터 보호 기능 향상**: Recovery Services 자격 증명 모음에서 Azure Backup은 클라우드 백업을 보호하는 보안 기능을 제공합니다. 이러한 보안 기능을 통해 백업을 보호하고 프로덕션 및 백업 서버가 손상된 경우에도 데이터를 안전하게 복구할 수 있습니다. [자세한 정보](backup-azure-security-feature.md)

- **하이브리드 IT 환경을 위한 중심 모니터링**: Recovery Services 자격 증명 모음에서 [Azure IaaS VM](backup-azure-manage-vms.md)뿐만 아니라 중앙 포털에서 [온-프레미스 자산](backup-azure-manage-windows-server.md#manage-backup-items)도 모니터링할 수 있습니다. [자세한 정보](https://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **RBAC(역할 기반 Access Control)**: RBAC는 Azure에서 세밀한 액세스 관리 제어를 제공합니다. [Azure는 다양한 기본 제공 역할을 제공](../role-based-access-control/built-in-roles.md)하고 Azure Backup에는 3가지 [복구 지점을 관리하는 기본 제공 역할](backup-rbac-rs-vault.md)이 포함됩니다. Recovery Services 자격 증명 모음은 RBAC와 호환되어 백업을 제한하고 정의된 집합의 사용자 역할에 대한 액세스를 복원합니다. [자세한 정보](backup-rbac-rs-vault.md)

- **Azure Virtual Machines의 모든 구성 보호**: Recovery Services 자격 증명 모음은 프리미엄 디스크, Managed Disks 및 암호화된 VM을 비롯한 Resource Manager 기반 VM을 보호합니다. Backup 자격 증명 모음을 Recovery Services 자격 증명 모음으로 업그레이드하면 서비스 관리자 기반 VM을 Resource Manager 기반 VM으로 업그레이드할 수 있습니다. 자격 증명 모음을 업그레이드하는 동안 서비스 관리자 기반 VM 복구 지점을 유지하고 업그레이드된(Resource Manager 사용 가능) VM에 대한 보호를 구성할 수 있습니다. [자세한 정보](https://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **IaaS VM에 대한 인스턴트 복원**: Recovery Services 자격 증명 모음을 사용하여 전체 VM을 복원하지 않고 IaaS VM의 파일 및 폴더를 복원할 수 있습니다. 그러면 복원 시간이 빨라집니다. IaaS VM에 대한 인스턴트 복원은 Windows 및 Linux VM 모두에서 제공됩니다. [자세한 정보](backup-instant-restore-capability.md)

## <a name="storage-settings-in-the-recovery-services-vault"></a>Recovery Services 자격 증명 모음의 저장소 설정

Recovery Services 자격 증명 모음은 시간에 따라 생성된 모든 백업과 복구 지점을 저장하는 엔터티입니다. Recovery Services 자격 증명 모음에는 보호된 가상 머신과 연결된 백업 정책도 포함됩니다.

Azure Backup는 자격 증명 모음에 대 한 저장소를 자동으로 처리 합니다. [저장소 설정을 변경](https://docs.microsoft.com/azure/backup/backup-create-rs-vault#set-storage-redundancy)하는 방법을 참조 하세요.

저장소 중복성에 대 한 자세한 내용은 [지역](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs) 및 [로컬](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs) 중복성에 대 한 문서를 참조 하세요.

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>포털에서 Recovery Services 자격 증명 모음 관리

Backup 서비스가 다른 Azure 서비스에 통합되기 때문에 Azure Portal에서 Recovery Services 자격 증명 모음을 쉽게 만들고 관리할 수 있습니다. 이 통합으로 인해 *대상 서비스의 컨텍스트에서* Recovery Services 자격 증명 모음을 만들거나 관리할 수 있습니다. 예를 들어 VM의 복구 지점을 보려면 해당 VM을 선택하고 작업 메뉴에서 **Backup**을 클릭합니다.

![Recovery Services 자격 증명 모음 세부 정보 VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context-vm.png)

VM에 백업이 구성되지 않은 경우 백업을 구성하라는 메시지가 표시됩니다. 백업이 구성된 경우 복원 지점 목록을 포함하여 VM에 대한 백업 정보가 표시 됩니다.  

![Recovery Services 자격 증명 모음 세부 정보 VM](./media/backup-azure-recovery-services-vault-overview/vm-recovery-point-list.png)

이전 예제에서 **ContosoVM**은 가상 머신의 이름입니다. **ContosoVM-demovault**는 Recovery Services 자격 증명 모음의 이름입니다. 가상 머신에서 이 정보에 액세스할 수 있으므로 복구 지점을 저장하는 Recovery Services 자격 증명 모음의 이름을 기억할 필요가 없습니다.  

하나의 Recovery Services 자격 증명 모음이 여러 서버를 보호하는 경우 Recovery Services 자격 증명 모음을 살펴보는 것이 좋습니다. 구독에서 모든 Recovery Services 자격 증명 모음을 검색하고 목록에서 하나를 선택할 수 있습니다.

다음 섹션에는 각 종류의 활동에서 Recovery Services 자격 증명 모음을 사용하는 방법을 설명하는 문서에 대한 링크가 있습니다.

> [!NOTE]
> Recovery Services 자격 증명 모음을 삭제한 후 24시간 동안은 같은 이름의 자격 증명 모음을 만들 수 없습니다. 다른 리소스 이름을 사용하거나 다른 리소스 그룹을 선택하고 24시간 후에 다시 시도하세요.

### <a name="back-up-data"></a>데이터 백업

- [Azure VM 백업](backup-azure-vms-first-look-arm.md)
- [Windows Server 또는 Windows 워크스테이션 백업](backup-try-azure-backup-in-10-mins.md)
- [Azure에 DPM 워크로드 백업](backup-azure-dpm-introduction.md)
- [Azure Backup Server를 사용하여 워크로드 백업 준비](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>복구 지점 관리

- [Azure VM 백업 관리](backup-azure-manage-vms.md)
- [파일 및 폴더 구성](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>자격 증명 모음에서 데이터 복원

- [Azure VM에서 개별 파일 복구](backup-azure-restore-files-from-vm.md)
- [Azure VM 복원](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>자격 증명 모음 보안

- [Recovery Services 자격 증명 모음에서 클라우드 백업 데이터 보안](backup-azure-security-feature.md)

## <a name="azure-advisor"></a>Azure Advisor

[Azure Advisor](https://docs.microsoft.com/azure/advisor/) 는 Azure 사용을 최적화 하는 데 도움이 되는 개인 설정 된 클라우드 컨설턴트입니다. Azure 사용량을 분석 하 고 배포를 최적화 하 고 보호 하는 데 도움이 되는 적시에 권장 사항을 제공 합니다. 고가용성, 보안, 성능 및 비용의 네 가지 범주로 권장 사항을 제공 합니다.

Azure Advisor는 백업 되지 않은 Vm에 대 한 시간별 [권장 사항을](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations#protect-your-virtual-machine-data-from-accidental-deletion) 제공 하므로 중요 한 vm은 백업 하지 않아도 됩니다. Snoozing 하 여 권장 사항을 제어할 수도 있습니다.  권장 사항을 클릭 하 고 Vm에 대 한 백업을 온라인에서 사용 하도록 설정할 수 있습니다 (백업이 저장 되는 위치) 및 백업 정책 (백업 일정 및 백업 복사본의 보존).

![Azure Advisor](./media/backup-azure-recovery-services-vault-overview/azure-advisor.png)

## <a name="next-steps"></a>다음 단계

다음 문서를 사용하여 해당 작업을 수행하세요.</br>
[IaaS VM 백업](backup-azure-arm-vms-prepare.md)</br>
[Azure Backup Server 백업](backup-azure-microsoft-azure-backup.md)</br>
[Windows Server 백업](backup-windows-with-mars-agent.md)
