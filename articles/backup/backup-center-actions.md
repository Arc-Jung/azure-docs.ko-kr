---
title: 백업 센터를 사용 하 여 작업 수행
description: 이 문서에서는 Backup center를 사용 하 여 작업을 수행 하는 방법을 설명 합니다.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 8c21475e5a52cdce7e38bbeb9d00df3c3ac3a752
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102506452"
---
# <a name="perform-actions-using-backup-center"></a>백업 센터를 사용 하 여 작업 수행

백업 센터를 사용 하면 개별 자격 증명 모음으로 이동할 필요 없이 중앙 인터페이스에서 키 백업 관련 작업을 수행할 수 있습니다. 백업 센터에서 수행할 수 있는 몇 가지 작업은 다음과 같습니다.

* 데이터 원본에 대 한 백업 구성
* 백업 인스턴스 복원
* 새 자격 증명 모음 만들기
* 새 백업 정책 만들기
* 백업 인스턴스에 대 한 주문형 백업 트리거
* 백업 인스턴스 백업 중지

## <a name="supported-scenarios"></a>지원되는 시나리오

* Backup center는 현재 azure vm 백업, azure vm 백업에서의 SQL, Azure VM 백업 SAP HANA Azure Files 백업 및 Azure Database for PostgreSQL Server 백업에 대해 지원 됩니다.
* 지원 되는 시나리오 및 지원 되지 않는 시나리오에 대 한 자세한 목록은 [지원 매트릭스](backup-center-support-matrix.md) 를 참조 하세요.

## <a name="configure-backup"></a>백업 구성

Azure vm을 백업 하는 경우 azure vm의 SQL, azure vm 또는 Azure Files SAP HANA Recovery Services 자격 증명 모음을 사용 해야 합니다. PostgreSQL 서버용 Azure 데이터베이스를 백업 하는 경우 백업 자격 증명 모음을 사용 해야 합니다. 

백업 하려는 데이터 원본 유형에 따라 아래에 설명 된 적절 한 지침을 따르세요.

### <a name="configure-backup-to-a-recovery-services-vault"></a>Recovery Services 자격 증명 모음에 백업 구성

1. 백업 센터로 이동 하 고 **개요** 탭의 맨 위에 있는 **+ 백업** 을 선택 합니다.

    ![백업 센터 개요](./media/backup-center-actions/backup-center-overview-configure-backup.png)

2. 백업 하려는 데이터 원본의 유형을 선택 합니다.

    ![VM 백업을 구성 하기 위한 데이터 원본 선택](./media/backup-center-actions/backup-select-datasource-vm.png)

3. Recovery Services 자격 증명 모음을 선택 하 고 **계속** 을 선택 합니다. 그러면 Recovery Services 자격 증명 모음에서 연결할 수 있는 것과 동일한 백업 구성 환경으로 이동할 수 있습니다. [Recovery Services 자격 증명 모음을 사용 하 여 Azure 가상 머신에 대 한 백업을 구성 하는 방법에 대해 자세히 알아보세요](tutorial-backup-vm-at-scale.md).

### <a name="configure-backup-to-a-backup-vault"></a>백업 자격 증명 모음에 백업 구성

1. 백업 센터로 이동 하 고 **개요** 탭의 맨 위에 있는 **+ 백업** 을 선택 합니다.
2. 백업 하려는 데이터 원본 유형 (이 경우 Azure Database for PostgreSQL 서버)을 선택 합니다.

    ![데이터 원본을 선택 하 Azure Database for PostgreSQL 서버 백업 구성](./media/backup-center-actions/backup-select-datasource-type-postgresql.png)

3. **계속** 을 선택합니다. 그러면 백업 자격 증명 모음에서 연결할 수 있는 것과 동일한 백업 구성 환경으로 이동할 수 있습니다. [백업 자격 증명 모음을 사용 하 여 Azure Database for PostgreSQL 서버에 대 한 백업을 구성 하는 방법에 대해 자세히 알아보세요](backup-azure-database-postgresql.md#configure-backup-on-azure-postgresql-databases).

## <a name="restore-a-backup-instance"></a>백업 인스턴스 복원

복원 하려는 데이터 원본 유형에 따라 아래에 설명 된 적절 한 지침을 따릅니다.

### <a name="if-youre-restoring-from-a-recovery-services-vault"></a>Recovery Services 자격 증명 모음에서 복원 하는 경우

1. 백업 센터로 이동 하 고 **개요** 탭의 위쪽에서 **복원** 을 선택 합니다.

    ![VM 복원에 대 한 백업 센터 개요](./media/backup-center-actions/backup-center-overview-restore.png)

2. 복원 하려는 데이터 원본의 유형을 선택 합니다.

    ![VM 복원에 사용할 데이터 원본 선택](./media/backup-center-actions/restore-select-datasource-vm.png)

3. 백업 인스턴스를 선택 하 고 **계속** 을 선택 합니다. 그러면 Recovery Services 자격 증명 모음에서 연결할 수 있는 것과 동일한 복원 설정 환경으로 이동할 수 있습니다. [Recovery Services 자격 증명 모음을 사용 하 여 Azure 가상 머신을 복원 하는 방법에 대해 자세히 알아보세요](backup-azure-arm-restore-vms.md#before-you-start).

### <a name="if-youre-restoring-from-a-backup-vault"></a>백업 자격 증명 모음에서 복원 하는 경우

1. 백업 센터로 이동 하 고 **개요** 탭의 위쪽에서 **복원** 을 선택 합니다.
2. 복원 하려는 데이터 원본 유형 (이 경우 Azure Database for PostgreSQL 서버)을 선택 합니다.

    ![Azure Database for PostgreSQL 서버 복원에 대 한 데이터 원본 선택](./media/backup-center-actions/restore-select-datasource-postgresql.png)

3. 백업 인스턴스를 선택 하 고 **계속** 을 선택 합니다. 그러면 Recovery Services 자격 증명 모음에서 연결할 수 있는 것과 동일한 복원 설정 환경으로 이동할 수 있습니다. [백업 자격 증명 모음을 사용 하 여 Azure Database for PostgreSQL 서버를 복원 하는 방법에 대해 자세히 알아보세요](backup-azure-database-postgresql.md#restore).

## <a name="create-a-new-vault"></a>새 자격 증명 모음 만들기

Backup center로 이동 하 고 **개요** 탭의 맨 위에 있는 **+ 자격 증명 모음** 을 선택 하 여 새 자격 증명 모음을 만들 수 있습니다.

![자격 증명 모음 만들기](./media/backup-center-actions/backup-center-create-vault.png)

* [Recovery services 자격 증명 모음 만들기에 대 한 자세한 정보](backup-create-rs-vault.md)
* [백업 자격 증명 모음 만들기에 대 한 자세한 정보](backup-vault-overview.md)

## <a name="create-a-new-backup-policy"></a>새 백업 정책 만들기

백업 하려는 데이터 원본 유형에 따라 아래에 설명 된 적절 한 지침을 따릅니다.

### <a name="if-youre-backing-up-to-a-recovery-services-vault"></a>Recovery Services 자격 증명 모음에 백업 하는 경우

1. 백업 센터로 이동 하 고 **개요** 탭의 맨 위에 있는 **+ 정책** 을 선택 합니다.

    ![백업 정책에 대 한 백업 센터 개요](./media/backup-center-actions/backup-center-overview-policy.png)

2. 백업 하려는 데이터 원본의 유형을 선택 합니다.

    ![VM 백업 정책에 대 한 데이터 원본 선택](./media/backup-center-actions/policy-select-datasource-vm.png)

3. Recovery services 자격 증명 모음을 선택 하 고 **계속** 을 선택 합니다. 그러면 Recovery Services 자격 증명 모음에서 도달할 수 있는 것과 동일한 정책 생성 환경으로 연결 됩니다. [Recovery services 자격 증명 모음을 사용 하 여 Azure 가상 머신에 대 한 새 백업 정책을 만드는 방법에 대해 자세히 알아보세요](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

### <a name="if-youre-backing-up-to-a-backup-vault"></a>백업 자격 증명 모음에 백업 하는 경우

1. 백업 센터로 이동 하 고 **개요** 탭의 맨 위에 있는 **+ 정책** 을 선택 합니다.
2. 백업 하려는 데이터 원본 유형 (이 경우 Azure Database for PostgreSQL 서버)을 선택 합니다.

    ![Azure Database for PostgreSQL 서버 백업 정책에 대 한 데이터 원본 선택](./media/backup-center-actions/policy-select-datasource-postgresql.png)

3. **계속** 을 선택합니다. 그러면 백업 자격 증명 모음에서 도달할 수 있는 것과 동일한 정책 생성 환경으로 연결 됩니다. [백업 자격 증명 모음을 사용 하 여 새 백업 정책을 만드는 방법에 대해 자세히 알아보세요](backup-azure-database-postgresql.md#create-backup-policy).

## <a name="execute-an-on-demand-backup-for-a-backup-instance"></a>백업 인스턴스에 대해 주문형 백업 실행

백업 센터에서는 백업 공간에서 백업 인스턴스를 검색 하 고 요청 시 백업 작업을 실행할 수 있습니다.

주문형 백업을 트리거하려면 Backup center로 이동 하 여 **Backup Instances** 메뉴 항목을 선택 합니다. 이를 선택 하면 액세스 권한이 있는 모든 백업 인스턴스의 세부 정보를 볼 수 있습니다. 백업 하려는 백업 인스턴스를 검색할 수 있습니다. 표의 항목을 마우스 오른쪽 단추로 클릭 하면 사용 가능한 작업 목록이 열립니다. **지금 백업** 옵션을 선택 하 여 요청 시 백업을 실행 합니다.

![주문형 백업](./media/backup-center-actions/backup-center-on-demand-backup.png)

[Azure Virtual Machines에 대 한 주문형 백업 수행에 대해 자세히 알아보세요](backup-azure-manage-vms.md#run-an-on-demand-backup).

[Azure Database for PostgreSQL 서버에 대 한 요청 시 백업을 수행 하는 방법에 대해 자세히 알아보세요](backup-azure-database-postgresql.md#on-demand-backup).

## <a name="stop-backup-for-a-backup-instance"></a>백업 인스턴스 백업 중지

백업 중인 기본 리소스가 더 이상 존재 하지 않는 경우와 같이 백업 인스턴스에 대 한 백업을 중지 하려는 경우가 있습니다.

주문형 백업을 트리거하려면 Backup center로 이동 하 여 **Backup Instances** 메뉴 항목을 선택 합니다. 이를 선택 하면 액세스 권한이 있는 모든 백업 인스턴스의 세부 정보를 볼 수 있습니다. 백업 하려는 백업 인스턴스를 검색할 수 있습니다. 표의 항목을 마우스 오른쪽 단추로 클릭 하면 사용 가능한 작업 목록이 열립니다. 백업 **중지** 옵션을 선택 하 여 백업 인스턴스에 대 한 백업을 중지 합니다.

![보호 중지](./media/backup-center-actions/backup-center-stop-protection.png)

[Azure Virtual Machines에 대 한 백업 중지에 대해 자세히 알아보세요](backup-azure-manage-vms.md#stop-protecting-a-vm).

[Azure Database for PostgreSQL 서버에 대 한 백업 중지에 대해 자세히 알아보세요.](backup-azure-database-postgresql.md#stop-protection)

## <a name="next-steps"></a>다음 단계

* [백업 모니터링 및 작동](backup-center-monitor-operate.md)
* [백업 공간 관리](backup-center-govern-environment.md)
* [백업에 대 한 통찰력 얻기](backup-center-obtain-insights.md)
