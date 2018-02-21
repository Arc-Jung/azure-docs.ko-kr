---
title: "Azure에서 VMware로 장애 복구하는 방법 | Microsoft Docs"
description: "가상 머신을 Azure로 장애 조치(failover)한 후 장애 복구를 시작하여 가상 머신을 온-프레미스로 복구할 수 있습니다. 장애 복구 방법에 대해 알아보세요."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: ruturajd
ms.openlocfilehash: 7a76ff45fe6209698532a7abc2d561bf7a7b3284
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/09/2018
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Azure에서 온-프레미스 사이트로 장애 복구

이 문서에서는 가상 머신을 Azure Virtual Machines에서 온-프레미스 VMware 환경으로 장애 복구(failback)하는 방법을 설명합니다. [Site Recovery의 장애 복구(failback)](site-recovery-failover.md) 자습서를 사용하여 온-프레미스 사이트에서 Azure로 장애 조치(failover)한 후 VMware 가상 머신 또는 Windows/Linux 물리적 서버를 장애 복구(failback)하려면 이 문서의 지침을 따르세요.

## <a name="prerequisites"></a>필수 조건
- [다양한 장애 복구(failback) 유형](concepts-types-of-failback.md) 및 해당 주의 사항에 대한 세부 정보를 읽어야 합니다.

> [!WARNING]
> [마이그레이션을 완료](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration)했거나, 가상 컴퓨터를 다른 리소스 그룹으로 이동했거나, Azure 가상 컴퓨터를 삭제한 후에 장애 복구(failback)를 수행할 수 없습니다. 가상 머신의 보호를 사용하지 않으면 장애 복구를 수행할 수 없습니다.

> [!WARNING]
> 보호되고 Azure로 장애 조치(Failover)된 경우 Windows Server 2008 R2 SP1 물리적 서버.

> [!NOTE]
> VMware 가상 머신에서 장애가 발생하면 Hyper-V 호스트로 장애 복구(failback)를 수행할 수 없습니다.


- 계속 진행하기 전에 가상 머신이 복제된 상태가 되고 온-프레미스로 장애 조치를 다시 시작할 수 있도록 다시 보호 단계를 완료하세요. 자세한 내용은 [Azure에서 온-프레미스로 다시 보호하는 방법](site-recovery-how-to-reprotect.md)을 참조하세요.

- 장애 복구를 수행 하기 전에 vCenter가 연결되었는지 확인합니다. 연결되지 않은 경우 디스크의 연결을 끊고 가상 머신을 다시 연결하는 작업이 실패합니다.

- Azure로 장애 조치(failover)하는 동안에는 온-프레미스 사이트에 액세스할 수 없으므로 구성 서버가 사용할 수 없거나 종료된 상태일 수 있습니다. 다시 보호 및 장애 복구 중에 온-프레미스 구성 서버는 실행 중이며 연결 양호 상태여야 합니다. 

- 장애 복구(failback)하는 동안 구성 서버 데이터베이스에 가상 컴퓨터가 있어야 하며, 그러지 않으면 장애 복구(failback)가 실패하게 됩니다. 따라서 서버의 예정된 정기 백업을 수행해야 합니다. 재해가 발생한 경우 장애 복구(failback)가 작동할 수 있도록 원래 IP 주소를 사용하여 서버를 복원해야 합니다.

- 다시 보호/장애 조치를 트리거하려면 마스터 대상 서버에 스냅숏이 없어야 합니다.

## <a name="overview-of-failback"></a>장애 복구 개요
Azure로 장애 조치(failover)한 후 다음 단계를 실행하여 온-프레미스 사이트로 장애 복구(failback)할 수 있습니다.

1. 온-프레미스 사이트의 VMware 가상 머신으로 복제를 시작하도록 Azure의 가상 머신을 [다시 보호](site-recovery-how-to-reprotect.md)합니다. 이 프로세스의 일부로 다음 작업을 수행해야 합니다.
    1. 온-프레미스 마스터 대상 설정: Windows 가상 머신에 대한 Windows 마스터 대상 및 Linux 가상 머신에 대한 [Linux 마스터 대상](site-recovery-how-to-install-linux-master-target.md).
    2. [프로세스 서버](site-recovery-vmware-setup-azure-ps-resource-manager.md) 설정.
    3. [다시 보호](site-recovery-how-to-reprotect.md) 시작. 이렇게 하면 온-프레미스 가상 머신이 꺼지고 Azure 가상 머신의 데이터가 온-프레미스 디스크와 동기화됩니다.

1. Azure의 가상 머신을 온-프레미스 사이트로 복제한 후 Azure에서 온-프레미스로 장애 조치(failover)를 시작합니다.

1. 데이터를 장애 복구(failback)한 후 Azure로 복제를 시작하도록 온-프레미스 가상 머신을 다시 보호합니다.

간략한 개요를 보려면 온-프레미스 사이트로 장애 복구(failback)하는 방법에 대한 다음 동영상을 시청하세요.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>장애 복구 단계

> [!IMPORTANT]
> 장애 복구를 시작하기 전에 가상 머신의 다시 보호를 완료했는지 확인합니다. 가상 머신은 **양호**한 상태로 보호되어야 합니다. 가상 머신을 다시 보호하려면 [다시 보호하는 방법](site-recovery-how-to-reprotect.md)을 참조하세요.

1. 복제된 항목 페이지에서 가상 컴퓨터를 선택하고 마우스 오른쪽 단추를 클릭하여 **계획되지 않은 장애 조치**를 선택합니다.
2. **장애 조치(failover) 확인** 에서 장애 조치(failover) 방향(Azure에서)을 확인하고 장애 조치(failover)에 사용할 복구 지점(최신 또는 최근 응용 프로그램 일치)을 선택합니다. 앱 일치 시점이 최신 시점보다 늦어지고 이로 인해 데이터 손실이 발생합니다.
3. 장애 조치(failover) 중에 Site Recovery가 Azure 가상 머신을 종료합니다. 장애 복구가 예상대로 완료되었는지 확인한 후에 Azure 가상 머신이 종료되었는지 확인할 수 있습니다.
4. Azure에서 장애 복구(failback)한 가상 머신을 제거하려면 **커밋**해야 합니다. 보호된 항목을 마우스 오른쪽 단추로 클릭한 다음, **커밋**을 클릭합니다. Azure에서 장애 조치된 가상 머신을 제거하는 작업이 수행됩니다.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>가상 머신을 어떤 복구 지점으로 장애 복구할 수 있습니까?

장애 복구 중에 가상 머신/복구 계획을 장애 복구할 수 있는 두 가지 옵션이 있습니다.

- 가장 최근에 처리된 시점을 선택하면 모든 가상 머신이 사용 가능한 최근 시점으로 장애 조치됩니다. 복구 계획 내에 복제 그룹이 있는 경우 복제 그룹의 각 가상 머신이 별도의 최근 시점으로 장애 조치(failover)됩니다.

    가상 머신에 복구 지점이 하나 이상 있어야 가상 머신을 장애 복구할 수 있습니다. 모든 가상 머신에 복구 지점이 하나 이상 있어야 복구 계획을 장애 복구할 수 있습니다.

> [!NOTE]
> 최신 복구 지점은 크래시 일관성 복구 지점입니다.

- 응용 프로그램 일치 복구 지점을 선택하면 단일 가상 머신 장애 복구(failback)가 사용 가능한 최신 응용 프로그램 일치 복구 지점으로 복구됩니다. 복제 그룹이 있는 복구 계획의 경우 각 복제 그룹은 사용 가능한 일반 복구 지점으로 복구됩니다.
응용 프로그램 일치 복구 지점의 경우 복구가 지연될 수 있고 데이터가 손실될 수 있습니다.

## <a name="what-happens-to-vmware-tools-post-failback"></a>장애 복구 후 VMware 도구는 어떻게 되나요?

Windows 가상 머신의 경우 Azure Site Recovery는 장애 조치(failover) 중에 VMware 도구를 사용하지 않도록 설정합니다. Windows 가상 머신의 장애 복구(failback) 중에 VMware 도구가 다시 사용하도록 설정됩니다. 


## <a name="reprotect-from-on-premises-to-azure"></a>온-프레미스에서 Azure로 다시 보호
장애 복구(failback)가 완료되고 커밋을 시작한 후 Azure의 복구된 가상 머신이 삭제됩니다. 이제 가상 머신이 다시 온-프레미스 사이트에 있지만 보호되지 않습니다. Azure로 다시 복제하려면 다음 작업을 수행합니다.

1. **자격 증명 모음** > **설정** > **복제된 항목**에서 장애 복구한 가상 머신을 선택한 다음 **다시 보호**를 클릭합니다.
2. Azure로 데이터를 다시 전송하는 데 사용해야 하는 프로세스 서버의 값을 제공합니다.
3. **확인**을 클릭하여 다시 보호 작업을 시작합니다.

> [!NOTE]
> 온-프레미스 가상 머신이 부팅된 후 에이전트에서 구성 서버에 다시 등록하는 데 약간의 시간(최대 15분)이 걸립니다. 이 시간 동안 다시 보호가 실패하고 에이전트가 설치되지 않았다는 오류 메시지가 반환됩니다. 몇 분 동안 기다린 다음 다시 보호를 다시 시도합니다.

## <a name="next-steps"></a>다음 단계

다시 보호 작업이 완료되면 가상 머신이 Azure로 다시 복제되며, [장애 조치(failover)](site-recovery-failover.md)를 수행하여 가상 머신을 Azure로 다시 이동할 수 있습니다.


