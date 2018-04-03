---
title: Azure Files 백업 FAQ
description: 이 문서에서는 Azure 파일 공유를 보호하는 방법을 자세히 설명합니다.
services: backup
keywords: SEO champ와 상담하지 않고 키워드를 추가하거나 편집하지 마세요.
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 850d4d1e2ef6a13fcd8a072e6da210d558c7769b
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Azure Files 백업에 대한 질문
이 문서에서는 Azure Files 백업에 대한 일반적인 질문에 대답합니다. 대답 중 일부에는 포괄적인 정보를 포함하는 문서에 대한 링크가 있습니다. 또한 [토론 포럼](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)에 Azure Backup 서비스에 대한 질문도 게시할 수 있습니다.

이 문서의 섹션을 신속하게 검색하려면 **이 문서** 아래의 오른쪽에서 링크를 사용합니다.

## <a name="configuring-the-backup-job-for-azure-files"></a>Azure Files에 대한 백업 작업 구성

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>유효한 Azure 파일 공유가 포함되어 있고 보호하려는 일부 저장소 계정이 표시되지 않는 이유는 무엇인가요? <br/>
미리 보기로 있는 동안에는 Azure 파일 공유에 대한 Backup에서 일부 유형의 저장소 계정이 지원되지 않습니다. 지원되는 저장소 계정 목록을 보려면 [여기](troubleshoot-azure-files.md#preview-boundaries) 목록을 참조하세요.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>백업을 구성하려고 할 때 저장소 계정에서 일부 Azure 파일 공유가 표시되지 않는 이유는 무엇인가요? <br/>
Azure 파일 공유가 동일한 Recovery Services 자격 증명 모음에서 이미 보호되었거나 최근에 삭제되었는지 확인합니다.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-file-sync-br"></a>Azure File Sync에서 동기화 그룹에 연결된 파일 공유를 보호할 수 없는 이유는 무엇인가요? <br/>
동기화 그룹에 연결된 Azure 파일 공유는 제한된 미리 보기로 보호되고 있습니다. 요청된 액세스에 대한 구독 ID를 사용하여 [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com)으로 문의하세요. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>Azure 파일 공유를 백업할 수 있는 지역은 어디인가요? <br/>
Azure 파일 공유에 대한 Backup은 현재 미리 보기로 제공되며 다음 지역에서만 사용할 수 있습니다. 
-   오스트레일리아 동남부(ASE) 
- 브라질 남부(BRS)
- 캐나다 중부(CNC)
-   캐나다 동부(CE)
-   미국 중부(CUS)
-   동아시아(EA)
-   오스트레일리아 동부(AE) 
-   미국 동부(EUS)
-   미국 동부 2(EUS2)
-   인도 중부(INC) 
-   미국 중북부(NCUS) 
-   북유럽(NE) 
-   미국 중남부(SCUS) 
-   동남 아시아(SEA)
-   영국 남부(UKS) 
-   영국 서부(UKW) 
-   유럽 서부(WE) 
-   미국 서부(WUS)
-   미국 중서부(WCUS)
-   미국 서부 2(WUS 2)

위에 나열되지 않은 특정 지역에서 사용해야 하는 경우 [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com)으로 문의하세요.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>자격 증명 모음에서 보호할 수 있는 Azure 파일 공유 수는 몇 개인가요?<br/>
미리 보기 동안에는 자격 증명 모음당 최대 25개의 저장소 계정에서 Azure 파일 공유를 보호할 수 있습니다. 또한 자격 증명 모음당 최대 200개의 Azure 파일 공유를 보호할 수 있습니다. 

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>파일 공유당 만들 수 있는 주문형 백업은 몇 개입니까? <br/>
언제든지 파일 공유에 대해 최대 200개의 스냅숏을 가질 수 있습니다. 이 한도에는 정책에 정의된 대로 Azure Backup에서 생성한 스냅숏이 포함됩니다. 한도에 도달한 후 백업이 실패하기 시작하면 향후 백업에 성공하기 위해 주문형 복원 지점을 삭제합니다.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>저장소 계정에서 가상 네트워크를 사용하도록 설정한 후부터 계정에서 파일 공유 백업이 실패하기 시작합니다. 그 이유는
Azure 파일 공유에 대한 Backup은 Virtual Networks를 사용하도록 설정된 저장소 계정을 지원하지 않습니다. 백업을 성공적으로 수행하려면 저장소 계정에서 Virtual Network를 사용하지 않도록 설정합니다. 

## <a name="restore"></a>복원

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>삭제된 Azure 파일 공유에서 복구할 수 있나요? <br/>
Azure 파일 공유가 삭제되면 삭제될 백업 목록도 표시되고 확인이 요구됩니다. 삭제된 Azure 파일 공유는 복원할 수 없습니다.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Azure 파일 공유에 대한 보호를 중지한 경우 백업에서 복원할 수 있나요? <br/>
예. 보호를 중지할 때 **백업 데이터 보관**을 선택한 경우 모든 기존 복원 지점에서 복원할 수 있습니다.

## <a name="manage-backup"></a>Backup 관리

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Azure Backup에서 만든 스냅숏에 액세스하고 탑재할 수 있나요? <br/>
Azure Backup에서 만든 모든 스냅숏은 포털의 스냅숏 보기, PowerShell 또는 CLI로 액세스할 수 있습니다. Azure 파일 공유 스냅숏에 대한 자세한 내용은 [Azure Files(미리 보기)의 공유 스냅숏 개요](../storage/files/storage-snapshots-files.md)를 참조하세요.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>백업에 대해 구성할 수 있는 최대 보존 기간은 얼마인가요? <br/>
Azure 파일 공유에 대한 Backup에서는 매일 백업을 최대 120일 동안 유지할 수 있습니다.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Azure 파일 공유에 대한 Backup 정책을 변경하면 어떻게 되나요? <br/>
파일 공유에 대한 새 정책이 적용되면 새 정책의 일정 및 보존 기간을 따릅니다. 보존 기간을 늘리면 기존 복구 지점이 새 정책에 따라 유지되도록 표시됩니다. 보존 기간을 줄이면 다음 정리 작업에서 정리(prune) 표시되고 결과적으로 삭제됩니다.

## <a name="see-also"></a>참고 항목
이 정보는 Azure 파일 백업에 대한 내용일 뿐이며, Azure Backup의 다른 영역에 대한 자세한 내용은 다음 Backup FAQ를 참조하세요.
-  [Recovery Services 자격 증명 모음 FAQ](backup-azure-backup-faq.md)
-  [Azure VM 백업 FAQ](backup-azure-vm-backup-faq.md)
-  [Azure Backup 에이전트 FAQ](backup-azure-file-folder-backup-faq.md)