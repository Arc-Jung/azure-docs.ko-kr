---
title: Azure Database Migration Service 도구 매트릭스
description: 데이터베이스를 마이그레이션하고 마이그레이션 프로세스의 다양한 단계를 지원하는 데 사용할 수 있는 서비스 및 도구를 알아봅니다.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: reference
ms.date: 03/03/2020
ms.openlocfilehash: 258874ffdbc471305b76f7d52406d2f45e708829
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575740"
---
# <a name="services-and-tools-available-for-data-migration-scenarios"></a>데이터 마이그레이션 시나리오에 사용할 수 있는 서비스 및 도구

이 문서에서는 다양한 데이터베이스 및 데이터 마이그레이션 시나리오와 특수 작업을 지원하는 데 사용할 수 있는 Microsoft와 타사의 서비스 및 도구 행렬을 제공합니다.

다음 표에서는 데이터 마이그레이션에 대 한 계획을 수립 하 고 다양 한 단계를 완료 하는 데 사용할 수 있는 서비스 및 도구를 식별 합니다.

> [!NOTE]
> 다음 표에서 별표(*)가 표시된 항목은 타사 도구입니다.

## <a name="business-justification-phase"></a>비즈니스 타당성 단계

| 원본 | 대상 | 검색 /<br/>재고 | 대상 및 SKU<br/>권장 사항 | TCO/ROI 및<br/>비즈니스 사례 |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
 SQL Server | Azure SQL DB MI | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure SQL VM | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) | [Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| SQL Server | Azure Synapse Analytics | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/)<br/>[Cloudamize](https://www.cloudamize.com/) |  | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS SQL | Azure SQL DB, MI, VM |  | [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| Oracle | Azure SQL DB, MI, VM | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[MigVisor*](https://www.migvisor.com/) |  |
| Oracle | Azure Synapse Analytics | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL 용 Azure DB-<br/>단일 서버 | [MAP 도구 키트](/previous-versions//bb977556(v=technet.10))<br/>[Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) |  |  |
| MongoDB | Cosmos DB | [Cloudamize](https://www.cloudamize.com/) | [Cloudamize](https://www.cloudamize.com/) |  |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, MI, VM | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/) | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| MySQL | MySQL용 Azure DB | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS MySQL | MySQL용 Azure DB |  |  | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) |  | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| RDS PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |  | [TCO 계산기](https://azure.microsoft.com/pricing/tco/calculator/) |
| DB2 | Azure SQL DB, MI, VM | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| 액세스 권한 | Azure SQL DB, MI, VM | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP ASE | Azure SQL DB, MI, VM | [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP IQ | Azure SQL DB, MI, VM |  |  |  |
| | | | | |

## <a name="pre-migration-phase"></a>마이그레이션 전 단계

| 원본 | 대상 | 앱 데이터 액세스<br/>레이어 평가 | 데이터베이스<br/>평가 | 성능<br/>평가 |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure Synapse Analytics | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) |  |  |
| RDS SQL | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DEA](https://www.microsoft.com/download/details.aspx?id=54090) |
| Oracle | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [Ssma](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | Azure Synapse Analytics | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [Ssma](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Oracle | PostgreSQL 용 Azure DB-<br/>단일 서버 |  | [Ora2Pg*](http://ora2pg.darold.net/start.html) |  |
| MongoDB | Cosmos DB |  | [Cloudamize](https://www.cloudamize.com/) | [Cloudamize](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |  |  |
| MySQL | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [Ssma](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/) |  |
| MySQL | MySQL용 Azure DB |  |  |  |
| RDS MySQL | MySQL용 Azure DB |  |  |  |
| PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |  |  |
| RDS PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |  |  |
| DB2 | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [Ssma](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| 액세스 권한 | Azure SQL DB, MI, VM |  | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP ASE | Azure SQL DB, MI, VM | [DAMT](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)  /  [Ssma](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |  |
| Sybase-SAP IQ | Azure SQL DB, MI, VM |  | |  |
| | | | | |

## <a name="migration-phase"></a>마이그레이션 단계

| 원본 | 대상 | 스키마 | 데이터<br/>라인인 | 데이터<br/>전환 |
| --- | --- | --- | --- | --- |
| SQL Server | Azure SQL DB | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL DB MI | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure SQL VM | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize](https://www.cloudamize.com/) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/)<br>[Cloudamize](https://www.cloudamize.com/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| SQL Server | Azure Synapse Analytics |  |  |  |
| RDS SQL | Azure SQL DB, MI, VM | [DMA](/sql/dma/dma-overview?view=sql-server-2017) | [DMA](/sql/dma/dma-overview?view=sql-server-2017)<br/>[DMS](https://azure.microsoft.com/services/database-migration/) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure SQL DB, MI, VM | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | Azure Synapse Analytics | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[SharePlex*](https://www.quest.com/products/shareplex/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Oracle | PostgreSQL 용 Azure DB-<br/>단일 서버 | [Ispirer*](https://www.ispirer.com/solutions) | [Ispirer*](https://www.ispirer.com/solutions) | [DMS](https://azure.microsoft.com/services/database-migration/) |
| MongoDB | Cosmos DB | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview) | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Cloudamize](https://www.cloudamize.com/)<br/>[Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Cassandra | Cosmos DB | [Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview) | [Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview) | [Imanis Data*](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/talena-inc.talena-solution-template?tab=Overview) |
| MySQL | Azure SQL DB, MI, VM | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| MySQL | MySQL용 Azure DB | [MySQL dump*](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS MySQL | MySQL용 Azure DB | [MySQL dump*](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 | [PG dump*](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| RDS PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 | [PG dump*](https://www.postgresql.org/docs/11/static/app-pgdump.html) |  | [DMS](https://azure.microsoft.com/services/database-migration/)<br/>[Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| DB2 | Azure SQL DB, MI, VM | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| 액세스 권한 | Azure SQL DB, MI, VM | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017) |
| Sybase-SAP ASE | Azure SQL DB, MI, VM | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [SSMA](/sql/ssma/sql-server-migration-assistant?view=sql-server-2017)<br/>[Ispirer*](https://www.ispirer.com/solutions) | [Attunity](https://www.attunity.com/products/replicate/)<br/>[Striim*](https://www.striim.com/partners/striim-for-microsoft-azure/) |
| Sybase-SAP IQ | Azure SQL DB, MI, VM | [Ispirer*](https://www.ispirer.com/solutions) | [Ispirer*](https://www.ispirer.com/solutions) | |
| | | | | |

## <a name="post-migration-phase"></a>마이그레이션 후 단계

| 원본 | 대상 | 최적화 |
| --- | --- | --- |
| SQL Server | Azure SQL DB | [Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL DB MI | [Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure SQL VM | [Cloud Atlas*](https://www.unifycloud.com/cloud-migration-tool/)<br/>[Cloudamize](https://www.cloudamize.com/) |
| SQL Server | Azure Synapse Analytics |  |
| RDS SQL | Azure SQL DB, MI, VM |  |
| Oracle | Azure SQL DB, MI, VM |  |
| Oracle | Azure Synapse Analytics |  |
| Oracle | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |
| MongoDB | Cosmos DB | [Cloudamize](https://www.cloudamize.com/) |
| Cassandra | Cosmos DB |  |
| MySQL | Azure SQL DB, MI, VM |  |
| MySQL | MySQL용 Azure DB |  |
| RDS MySQL | MySQL용 Azure DB |  |
| PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |
| RDS PostgreSQL | PostgreSQL 용 Azure DB-<br/>단일 서버 |  |
| DB2 | Azure SQL DB, MI, VM |  |
| 액세스 권한 | Azure SQL DB, MI, VM |  |
| Sybase-SAP ASE | Azure SQL DB, MI, VM |  |
| Sybase-SAP IQ | Azure SQL DB, MI, VM |  |
| | | |

## <a name="next-steps"></a>다음 단계

Azure Database Migration Service에 대 한 개요는 [Azure Database Migration Service 이란?](dms-overview.md)문서를 참조 하세요.