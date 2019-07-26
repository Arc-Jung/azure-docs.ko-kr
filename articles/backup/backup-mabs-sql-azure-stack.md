---
title: Azure Stack에 SQL Server 워크로드 백업
description: Azure Backup Server를 사용하여 Azure Stack에서 SQL Server 워크로드를 보호할 수 있습니다.
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: adigan
ms.openlocfilehash: 11d03a9c5cc81b915f48bc66f5a0e5ab034662ed
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465148"
---
# <a name="back-up-sql-server-on-stack"></a>Stack에 SQL Server 백업
이 문서를 사용하여 Azure Stack에서 SQL Server 데이터베이스를 보호하도록 MABS(Microsoft Azure Backup Server)를 구성할 수 있습니다.

Azure에 SQL Server 데이터베이스를 백업하고 Azure에서 데이터베이스를 복구하는 것을 관리하는 작업에는 세 가지 단계가 포함됩니다.

1. SQL server 데이터베이스를 보호하기 위한 백업 정책 만들기
2. 주문형 백업 복사본을 만들기
3. Azure 및 디스크에서 데이터베이스 복구

## <a name="before-you-start"></a>시작하기 전에

[Azure Backup Server를 설치하고 준비](backup-mabs-install-azure-stack.md)합니다.

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Azure에 대해 SQL server 데이터베이스를 보호하기 위한 백업 정책을 만듭니다.
1. Azure Backup Server UI에서 **보호** 작업 영역을 클릭합니다.

2. 도구 리본에서 **새로 만들기**를 클릭하여 새 보호 그룹을 만듭니다.

    ![보호 그룹 만들기](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup Server가 **보호 그룹**을 만드는 방법을 안내하는 보호 그룹 마법사를 시작합니다. **다음**을 클릭합니다.

3. **보호 그룹 형식 선택** 화면에서 **서버**를 선택합니다.

    ![선택하는 보호 그룹 종류 - '서버'](./media/backup-azure-backup-sql/pg-servers.png)

4. **그룹 멤버 선택** 화면에서, 사용 가능한 멤버 목록에 다양한 데이터 원본이 표시됩니다. **+** 기호를 클릭하여 폴더를 확장하고 하위 폴더를 표시합니다. 확인란을 클릭하여 항목을 선택합니다.

    ![SQL DB를 선택합니다.](./media/backup-azure-backup-sql/pg-databases.png)

    선택한 모든 항목은 선택한 멤버 목록에 표시됩니다. 보호할 서버 또는 데이터베이스를 선택한 후 **다음**을 클릭합니다.

5. **데이터 보호 방법 선택** 화면에서 보호 그룹의 이름을 입력하고 **온라인 보호를 사용하려고 합니다** 확인란을 선택합니다.

    ![데이터 보호 방법 - 단기 디스크 및 온라인 Azure](./media/backup-azure-backup-sql/pg-name.png)

6. **단기 목표 지정** 화면에서, 디스크에 백업 지점을 만드는 데 필요한 입력을 포함하고 **다음**을 클릭합니다.

    이 예제에서 **보존 범위**는 **5일**, **동기화 빈도**는 백업 빈도인 **15분**입니다. **빠른 전체 Backup** 을 **오후 8시**로 설정합니다.

    ![단기 목표](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 표시된 예제는 매일 오후 8시에 전날의 오후 8시 백업 지점에서 수정된 데이터를 전송하여 백업 지점을 만듭니다. 이 프로세스를 **빠른 전체 Backup**이라고 합니다. 트랜잭션 로그는 15분마다 동기화됩니다. 오후 9시에 데이터베이스를 복구해야 하는 경우 마지막 고속 전체 백업 지점(이 예에서는 8PM)의 로그에서 지점이 생성됩니다.
   >
   >

7. **디스크 할당 검토** 화면에서 사용 가능한 전체 저장소 공간 및 잠재적인 디스크 공간을 확인합니다. **다음**을 클릭합니다.

8. **복제본 만들기 방법 선택**에서, 첫 번째 복구 지점을 만들 방법을 선택합니다. 대역폭 정체를 방지하기 위해 초기 백업을 수동으로(오프 네트워크) 전송해도 되고 아니면 네트워크를 통해 전송해도 됩니다. 첫 번째 백업이 전송되기를 기다리기로 선택하는 경우 초기 전송의 시간을 지정할 수 있습니다. **다음**을 클릭합니다.

    ![초기 복제 방법](./media/backup-azure-backup-sql/pg-manual.png)

    초기 백업 복사본은 프로덕션 서버(SQL Server 컴퓨터)에서 Azure Backup Server로 전체 데이터 원본(SQL Server 데이터베이스)을 전송해야 합니다. 이 데이터는 규모가 클 수 있으며 네트워크를 통한 데이터 전송이 대역폭을 초과할 수 있습니다. 이러한 이유로 초기 백업을 다음과 같이 전송하도록 선택할 수 있습니다. 대역폭 정체 방지를 위해 이동식 미디어를 사용하여 **수동**으로 전송하거나, 지정된 시간에 **네트워크를 통해 자동**으로 전송할 수도 있습니다.

    초기 백업을 완료하면 백업의 나머지 부분은 초기 백업 복사본의 증분 백업입니다. 증분 백업은 크기가 작으며 네트워크를 통해 간편하게 전송될 수 있습니다.

9. 일관성 확인을 실행하려는 경우 선택하고 **다음**을 클릭합니다.

    ![일관성 확인](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup Server는 백업 시점의 무결성을 지속적으로 확인합니다. Azure Backup Server는 프로덕션 서버(이 시나리오에서는 SQL Server 컴퓨터)의 백업 파일과 해당 파일에 대한 백업 데이터의 체크섬을 계산합니다. 충돌이 있는 경우 Azure Backup Server에 백업된 파일이 손상된 것으로 가정합니다. Azure Backup Server는 체크섬 불일치에 해당하는 블록을 전송하여 백업된 데이터를 균형 있게 조정합니다. 일관성 검사는 많은 성능이 필요한 작업이므로 일관성 검사를 예약하거나 자동으로 실행할 수 있습니다.

10. 데이터 원본에 온라인 보호를 지정하려면 데이터베이스를 Azure에 보호되도록 선택하고 **다음**을 클릭합니다.

    ![데이터 원본 선택](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. 조직 정책에 맞게 백업 일정 및 보존 정책을 선택합니다.

    ![일정 및 보존](./media/backup-azure-backup-sql/pg-schedule.png)

    이 예에서 백업은 매일 한 번 오후 12시 및 오후 8시에 수행됩니다.(화면의 아래쪽 부분)

    > [!NOTE]
    > 신속한 복구를 위해 디스크에 몇 가지 단기 복구 지점이 있는 것이 좋습니다. 이러한 복구 지점은 운영 복구에 사용됩니다. Azure는 높은 SLA 및 보장된 가용성이 있는 좋은 오프사이트 위치를 제공합니다.
    >
    >

    **모범 사례**: 로컬 디스크 백업이 완료된 후 Azure로 백업을 시작하도록 예약하면 항상 마지막 디스크 백업이 Azure로 복사됩니다.

12. 보존 정책 일정을 선택합니다. 보존 정책이 작동하는 방법에 대한 자세한 내용은 [Azure Backup을 사용하여 테이프 인프라 대체 문서](backup-azure-backup-cloud-as-tape.md)에서 제공됩니다.

    ![보존 정책](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    이 예제에서:

    * Backup은 매일 한 번 오후 12시 및 오후 8시에 수행되며(화면의 아래쪽 부분) 180일 동안 유지됩니다.
    * 토요일 오후 12시에 수행되는 백업은 104주 동안 유지됩니다.
    * 마지막 주 토요일 오후 12시에 수행되는 백업은 60개월 동안 유지됩니다.
    * 3월 마지막 주 토요일 오후 12시에 수행되는 백업은 10년 동안 유지됩니다.
13. **다음** 을 클릭하고 초기 백업 복사본을 Azure에 전송하기 위한 적절한 옵션을 선택합니다. **네트워크를 통해 자동으로**를 선택할 수 있습니다.

14. **요약** 화면에서 정책 세부 정보를 검토한 후에는 **그룹 만들기**를 클릭하여 워크플로를 완료합니다. **닫기**를 클릭하고 작업 영역 모니터링에서 작업 진행 상태를 모니터링할 수 있습니다.

    ![진행 중인 보호 그룹 만들기](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>SQL Server 데이터베이스의 주문형 백업
이전 단계가 백업 정책을 만드는 동안 첫 번째 백업이 발생하면 "복구 지점"을 만듭니다. 스케줄러가 시작되기를 기다리는 대신 다음 단계는 수동으로 복구 지점의 생성을 트리거합니다.

1. 복구 지점을 만들기 전에 보호 그룹 상태가 데이터베이스를 **확인**으로 표시될 때까지 대기합니다.

    ![보호 그룹 멤버](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. 데이터베이스를 마우스 오른쪽 단추로 클릭하고 **복구 지점 만들기**를 선택합니다.

    ![온라인 복구 지점 만들기](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. 드롭다운 메뉴에서 **온라인 보호**를 선택하고 **확인**을 클릭하여 Azure에서 복구 지점 만들기를 시작합니다.

    ![복구 지점 만들기](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. **모니터링** 작업 영역에서 작업 진행률을 봅니다.

    ![콘솔 모니터링](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure에서 SQL Server 데이터베이스 복구
Azure에서 보호되는 엔터티(SQL Server 데이터베이스)를 복구하려면 다음 단계가 필요합니다.

1. Azure Backup Server 관리 콘솔을 엽니다. 보호되는 서버를 볼 수 있는 **복구** 작업 영역으로 이동합니다. 필요한 데이터베이스로 이동합니다.(이 경우 ReportServer$ MSDPM2012) **온라인** 지점으로 지정된 **복구 시간**을 선택합니다.

    ![복구 지점 선택](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. 데이터베이스 이름을 마우스 오른쪽 단추로 클릭하고 **복구**를 클릭합니다.

    ![Azure에서 복구](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS에서는 복구 지점에 대한 세부 정보를 보여줍니다. **다음**을 클릭합니다. 데이터베이스를 덮어쓰려면 복구 형식 **SQL Server의 원본 인스턴스에 복구**를 선택합니다. **다음**을 클릭합니다.

    ![원래 위치로 복구](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    이 예제에서 MABS는 다른 SQL Server 인스턴스 또는 독립 실행형 네트워크 폴더로 데이터베이스를 복구합니다.

4. **복구 옵션 지정** 화면에서 네트워크 대역폭 사용 제한을 같은 복구 옵션을 선택하여 복구에서 사용되는 대역폭을 제한할 수 있습니다. **다음**을 클릭합니다.

5. **요약** 화면에서는 지금까지 제공하는 모든 복구 구성을 표시합니다. **복구**를 클릭합니다.

    복구 상태는 복구 중인 데이터베이스를 표시합니다. **닫기**를 클릭하여 마법사를 닫고 **모니터링** 작업 영역에서 진행률을 봅니다.

    ![복구 프로세스 시작](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    복구가 완료되면 복원된 데이터베이스는 애플리케이션과 일치합니다.

## <a name="next-steps"></a>다음 단계

[파일 및 애플리케이션 백업](backup-mabs-files-applications-azure-stack.md) 문서를 참조하세요.
[Azure Stack에 SharePoint 백업](backup-mabs-sharepoint-azure-stack.md) 문서를 참조하세요.
