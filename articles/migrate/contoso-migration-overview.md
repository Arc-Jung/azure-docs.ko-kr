---
title: 콘토소 마이그레이션 시리즈 | 마이크로 소프트 문서
description: Azure로 마이그레이션하기 위한 Contoso 예제 마이그레이션 시나리오에 대한 링크입니다.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: c57a9f85e8b12bd4e1e66a4fcd5d08ab5f7b9118
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676321"
---
# <a name="contoso-migration-series"></a>Contoso 마이그레이션 시리즈


가상 조직 Contoso가 온-프레미스 인프라를 [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) 클라우드로 마이그레이션하는 방법을 보여 주는 일련의 문서가 있습니다. 

이 시리즈에는 인프라 마이그레이션을 설정하는 방법과 다양한 유형의 마이그레이션을 실행하는 방법을 설명하는 시나리오가 포함되어 있습니다. 시나리오는 진행됨에 따라 복잡해짐에 따라 증가합니다. 이 문서에서는 Contoso 회사가 마이그레이션을 처리하는 방법을 보여 주지만 일반적인 지침과 포인터가 전체적으로 제공됩니다.

## <a name="migration-articles"></a>마이그레이션 문서

시리즈의 문서는 아래 표에 요약되어 있습니다.  

- 각 마이그레이션 시나리오는 마이그레이션 전략을 결정하는 약간 다른 비즈니스 요구 사항에 의해 결정됩니다.
- 각 배포 시나리오에 대해 비즈니스 동인 및 목표, 제안된 아키텍처, 마이그레이션 을 수행하는 단계, 마이그레이션이 완료된 후 정리 및 다음 단계에 대한 권장 사항을 제공합니다.


**기술** | **세부 정보** 
--- | --- 
[문서 1: 개요](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | 문서 시리즈, Contoso의 마이그레이션 전략 및 시리즈에서 사용되는 샘플 앱에 대해 간략히 설명합니다. 
[문서 2: Azure 인프라 배포](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso는 온-프레미스 인프라와 마이그레이션을 위한 Azure 인프라를 준비합니다. 이 시리즈의 모든 문서에서 동일한 인프라가 사용됩니다. 
[문서 3: Azure로 마이그레이션할 온-프레미스 리소스 평가](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso가 VMware에서 실행되는 온-프레미스 SmartHotel360 앱의 평가를 실행합니다. Contoso에서 Azure Migrate 서비스를 사용하여 앱 VM을 평가하고, Database Migration Assistant를 사용하여 앱 SQL Server 데이터베이스를 평가합니다.
[문서 4: Azure VM 및 SQL Database Managed Instance에서 앱 다시 호스트](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso가 온-프레미스 SmartHotel360 앱을 Azure로 리프트 앤 시프트 방식으로 마이그레이션합니다. Contoso는 Azure 마이그레이션을 사용하여 앱 프런트 엔드 VM을 [마이그레이션합니다.](https://docs.microsoft.com/azure/migrate/migrate-services-overview) Contoso는 Azure 데이터베이스 [마이그레이션 서비스를](https://docs.microsoft.com/azure/dms/dms-overview)사용하여 앱 데이터베이스를 Azure SQL Database 관리 인스턴스로 마이그레이션합니다.
[문서 5: 앱을 Azure VM에 다시 호스트](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso는 Azure 마이그레이션 서비스를 사용하여 SmartHotel360 앱 VM을 Azure VM으로 마이그레이션합니다. 
[문서 6: Azure VM 및 SQL Server AlwaysOn 가용성 그룹에서 앱 다시 호스트](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso가 SmartHotel360 앱을 마이그레이션합니다. Contoso는 Azure 마이그레이션을 사용하여 앱 VM을 마이그레이션합니다. Database Migration Service를 사용하여 앱 데이터베이스를 AlwaysOn 가용성 그룹으로 보호되는 SQL Server 클러스터로 마이그레이션합니다. 
[문서 7: Azure VM에서 Linux 앱 다시 호스트](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso는 Azure 마이그레이션을 사용하여 Linux osTicket 앱을 Azure VM으로 리프트 앤 시프트 마이그레이션을 완료합니다.
[문서 8: Azure VM 및 Azure Database for MySQL에서 Linux 앱 다시 호스트](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso는 Linux osTicket 앱을 Azure 마이그레이션을 사용하여 Azure VM으로 마이그레이션합니다. Azure 데이터베이스 마이그레이션 서비스(MySQL Workbench를 사용하는 대체 옵션 포함)를 사용하여 앱 데이터베이스를 MySQ용 Azure 데이터베이스로 마이그레이션합니다.
[문서 9: Azure 웹앱 및 Azure SQL Database에서 앱 리팩터링](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso는 SmartHotel360 앱을 Azure 웹 앱으로 마이그레이션하고 Azure 데이터베이스 마이그레이션 서비스를 사용하여 앱 데이터베이스를 Azure SQL Server 인스턴스로 마이그레이션합니다.
[조 10: Azure 앱 서비스 및 SQL 관리 인스턴스를 사용하여 Windows 앱 리팩터링](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso는 온-프레미스 Windows 기반 앱을 Azure 웹 앱으로 마이그레이션하고 Azure 데이터베이스 마이그레이션 서비스를 사용하여 앱 데이터베이스를 Azure SQL 관리 인스턴스로 마이그레이션합니다.
[조 11: Azure 웹 앱에서 Linux 앱 및 MySQL용 Azure 데이터베이스 리팩터링](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso는 지속적인 배달을 위해 GitHub와 통합된 Azure 트래픽 관리자를 사용하여 Linux osTicket 앱을 여러 Azure 지역의 Azure 웹 앱으로 마이그레이션합니다. Contoso에서 앱 데이터베이스를 Azure Database for MySQL 인스턴스로 마이그레이션합니다. 
[조 12: Azure DevOps 서비스에 팀 기반 서버 리팩터링](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso에서 온-프레미스 Team Foundation Server 배포를 Azure의 Azure DevOps Services로 마이그레이션합니다.
[문서 13: Azure에서 앱 다시 빌드](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso는 Azure 앱 서비스, AKS(Azure Kubernetes Service), Azure 기능, Azure 인지 서비스 및 Azure Cosmos DB를 비롯한 다양한 Azure 기능 및 서비스를 사용하여 SmartHotel 앱을 다시 빌드합니다.
[문서 14: Azure로의 마이그레이션 확장](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | 마이그레이션 조합을 시도한 후 Contoso는 Azure로 전체 마이그레이션을 확장할 준비를 합니다.



## <a name="next-steps"></a>다음 단계

- 클라우드 [마이그레이션에 대해 자세히 알아봅니다.](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/)
- 데이터베이스 마이그레이션 [가이드에서](https://datamigration.microsoft.com/)다른 시나리오(소스/대상 쌍)에 대한 마이그레이션 전략에 대해 알아봅니다.
