---
title: SQL Database의 Azure CLI 스크립트 예제 | Microsoft Docs
description: Azure SQL Database 서버, 탄력적 풀, 데이터베이스 및 방화벽을 만들고 관리하는 Azure CLI 스크립트 예제입니다.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 02/03/2019
ms.openlocfilehash: 5366fb1d32020bfbcfaba36c60c0eb5441e92070
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71055198"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure SQL Database에 대한 Azure CLI 샘플

<a href="/cli/azure">Azure CLI</a>를 사용하여 Azure SQL Database를 구성할 수 있습니다.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택한 경우 이 항목에서 Azure CLI 버전 2.0 이상을 실행해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요.

## <a name="single-database--elastic-poolstabsingle-database"></a>[단일 데이터베이스 및 탄력적 풀](#tab/single-database)

다음 표에서는 Azure SQL Database의 Azure CLI 스크립트 예제에 대한 링크를 포함합니다.

| |  |
|---|---|
|**단일 데이터베이스 및 탄력적 풀 만들기**||
| [단일 데이터베이스 만들기 및 방화벽 규칙 구성](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | 이 CLI 스크립트 예제는 단일 Azure SQL 데이터베이스를 만들고 서버 수준 방화벽 규칙을 구성합니다. |
| [탄력적 풀 만들기 및 풀링된 데이터베이스 이동](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | 이 CLI 스크립트 예제는 SQL 탄력적 풀을 만들고, 풀링된 Azure SQL Database를 이동하고, 컴퓨팅 크기를 변경합니다.|
|**단일 데이터베이스 및 탄력적 풀 크기 조정**||
| [단일 데이터베이스 크기 조정](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | 이 CLI 스크립트 예제는 데이터베이스의 크기 정보를 쿼리한 후에 단일 Azure SQL 데이터베이스를 다양한 컴퓨팅 크기로 확장합니다. |
| [탄력적 풀 크기 조정](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | 이 CLI 스크립트 예제는 SQL 탄력적 풀을 다른 컴퓨팅 크기로 조정합니다.  |
|**장애 조치(failover) 그룹**||
| [장애 조치(failover) 그룹에 단일 데이터베이스 추가](scripts/sql-database-add-single-db-to-failover-group-cli.md?toc=%2fcli%2fazure%2ftoc.json)| 이 CLI 스크립트는 데이터베이스 및 장애 조치(failover) 그룹을 만들고, 장애 조치(failover) 그룹에 데이터베이스를 추가하고, 보조 서버로 장애 조치(failover)를 테스트합니다.|
|||

[단일 데이터베이스 Azure CLI API](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases)에 대해 자세히 알아봅니다.

## <a name="managed-instancetabmanaged-instance"></a>[Managed Instance](#tab/managed-instance)

다음 표에서는 Azure SQL Database - Managed Instance의 Azure CLI 스크립트 예제에 대한 링크를 포함합니다.

| |  |
|---|---|
| [Managed Instance 만들기](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) | 이 CLI 스크립트는 Managed Instance를 만드는 방법을 보여 줍니다. |
| [Managed Instance 업데이트](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/) | 이 CLI 스크립트는 Managed Instance를 업데이트하는 방법을 보여 줍니다. |
| [데이터베이스를 다른 Managed Instance로 이동](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | 이 CLI 스크립트는 한 인스턴스에서 다른 인스턴스로 데이터베이스의 백업을 복원하는 방법을 보여 줍니다. |
|||

[Managed Instance Azure CLI API](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances)에 대해 자세히 알아보고 [여기에서 추가 예제](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44)를 확인해 보세요.

---
