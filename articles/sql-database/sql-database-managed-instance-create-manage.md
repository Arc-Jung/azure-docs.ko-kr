---
title: 관리 되는 인스턴스에 대 한 관리 API 참조
description: Azure SQL Database Managed Instance를 만들고 관리하는 방법을 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 713217a933c646cc4d04759f5697bbc0312827ce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79268861"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Azure SQL Database Managed Instance에 대한 관리 API 참조

Azure Portal, PowerShell, Azure CLI, REST API 및 Transact-SQL을 사용하여 Azure SQL Database Managed Instance를 만들고 관리할 수 있습니다. 이 문서에서는 Managed Instance를 만들고 구성하는 데 사용할 수 있는 기능 및 API 개요를 찾을 수 있습니다.

## <a name="azure-portal-create-a-managed-instance"></a>Azure Portal: 관리 되는 인스턴스 만들기

Azure SQL Database Managed Instance을 만드는 방법을 보여 주는 빠른 시작은 [빠른 시작: Azure SQL Database Managed Instance 만들기](sql-database-managed-instance-get-started.md)를 참조 하세요.

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: 관리 되는 인스턴스 만들기 및 관리

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager 모듈은 Azure SQL Database에서 계속 지원 되지만 모든 향후 개발은 Az. Sql 모듈에 대 한 것입니다. 이러한 cmdlet에 대 한 자세한 내용은 [AzureRM](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)를 참조 하세요. Az module 및 AzureRm 모듈의 명령에 대 한 인수는 실질적으로 동일 합니다.

Azure PowerShell을 사용하여 Managed Instance를 만들고 관리하려면 다음 PowerShell cmdlet을 사용합니다. PowerShell을 설치하거나 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요.

> [!TIP]
> PowerShell 예제 스크립트에 대해서는 [빠른 시작 스크립트: PowerShell 라이브러리를 사용 하 여 AZURE SQL Managed Instance 만들기](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/)를 참조 하세요.

| Cmdlet | Description |
| --- | --- |
|[New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Azure SQL Database Managed Instance를 만듭니다. |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Azure SQL Managed Instance에 대한 정보를 반환합니다.|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Azure SQL Database Managed Instance에 대한 속성을 설정합니다.|
|[AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Azure SQL Database Managed Instance를 제거합니다.|
|[AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Azure SQL Database Managed Instance 데이터베이스 만들기|
|[AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Azure SQL Managed Instance 데이터베이스에 대한 정보를 반환합니다.|
|[AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Azure SQL Database Managed Instance 데이터베이스를 제거합니다.|
|[Restore-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Azure SQL Database Managed Instance 데이터베이스를 복원합니다.|

## <a name="azure-cli-create-and-manage-managed-instances"></a>Azure CLI: 관리 되는 인스턴스 만들기 및 관리

[Azure CLI](/cli/azure)를 사용하여 Managed Instance를 만들고 관리하려면 다음 [Azure CLI SQL Managed Instance](/cli/azure/sql/mi) 명령을 사용합니다. [Cloud Shell](/azure/cloud-shell/overview)을 사용하여 CLI 브라우저에서 실행하거나 macOS, Linux 또는 Windows에서 [설치](/cli/azure/install-azure-cli)합니다.

> [!TIP]
> Azure CLI 빠른 시작을 보려면 [Azure CLI를 사용하여 SQL Managed Instance 작업](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)을 참조하세요.

| Cmdlet | Description |
| --- | --- |
|[az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Managed Instance를 만듭니다.|
|[az sql mi list](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|사용 가능한 Managed Instance를 나열합니다.|
|[az sql mi show](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Managed Instance에 대한 세부 정보를 가져옵니다.|
|[az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Managed Instance를 업데이트합니다.|
|[az sql mi delete](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Managed Instance를 제거합니다.|
|[az sql midb create](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |관리되는 데이터베이스를 만듭니다.|
|[az sql midb list](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|사용 가능한 관리되는 데이터베이스를 나열합니다.|
|[az sql midb restore](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|관리되는 데이터베이스를 복원합니다.|
|[az sql midb delete](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|관리되는 데이터베이스를 제거합니다.|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-sql: 인스턴스 데이터베이스 만들기 및 관리

Managed Instance를 만든 후에 인스턴스 데이터베이스를 만들고 관리하려면 다음 T-SQL 명령을 사용합니다. Azure Portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is), [Visual Studio Code](https://code.visualstudio.com/docs) 또는 Azure SQL Database 서버에 연결하고 Transact-SQL 명령을 전달할 수 있는 기타 프로그램을 사용하여 이러한 명령을 실행할 수 있습니다.

> [!TIP]
> Microsoft Windows의 SQL Server Management Studio를 사용 하 여 Managed Instance를 구성 하 고 연결 해야 함을 보여 주는 빠른 시작은 [빠른 시작: AZURE VM을 구성 하 여 Azure SQL Database Managed Instance에 연결](sql-database-managed-instance-configure-vm.md) 하 고 [빠른 시작: 온-프레미스에서 Azure SQL Database Managed Instance에 대 한 지점 및 사이트 간 연결 구성](sql-database-managed-instance-configure-p2s.md)을 참조 하세요.
> [!IMPORTANT]
> Transact-SQL을 사용하여 Managed Instance를 만들거나 삭제할 수 없습니다.

| 명령 | Description |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|새 Managed Instance 데이터베이스를 만듭니다. 새 데이터베이스를 만들려면 master 데이터베이스에 연결해야 합니다.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Azure SQL Managed Instance 데이터베이스를 수정합니다.|

## <a name="rest-api-create-and-manage-managed-instances"></a>REST API: 관리 되는 인스턴스 만들기 및 관리

Managed Instance를 만들고 관리하려면 다음 REST API 요청을 사용합니다.

| 명령 | Description |
| --- | --- |
|[Managed Instances - Create 또는 Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Managed Instance를 만들거나 업데이트합니다.|
|[Managed Instances - Delete](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Managed Instance를 삭제합니다.|
|[Managed Instances - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Managed Instance를 가져옵니다.|
|[Managed Instances - List](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|구독에서 Managed Instance 목록을 반환합니다.|
|[Managed Instances - List By Resource Group](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|리소스 그룹에서 Managed Instance 목록을 반환합니다.|
|[Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Managed Instance를 업데이트합니다.|

## <a name="next-steps"></a>다음 단계

- SQL Server 데이터베이스를 Azure로 마이그레이션하는 방법에 대한 자세한 내용은 [Azure SQL Database로 마이그레이션](sql-database-single-database-migrate.md)을 참조하세요.
- 지원되는 기능에 대한 자세한 내용은 [기능](sql-database-features.md)을 참조하세요.
