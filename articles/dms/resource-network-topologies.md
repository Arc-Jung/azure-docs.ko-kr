---
title: Azure Database Migration Service를 사용한 Azure SQL DB 관리되는 인스턴스 마이그레이션에 대한 네트워크 토폴로지 | Microsoft Docs
description: 데이터베이스 마이그레이션 서비스에 대한 원본 및 대상 구성을 알아봅니다.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Azure Database Migration Service를 사용한 Azure SQL DB 관리되는 인스턴스 마이그레이션에 대한 네트워크 토폴로지
이 문서에서는 Azure Database Migration Service가 온-프레미스 SQL Server에서 Azure SQL Database 관리되는 인스턴스로의 원활한 마이그레이션 환경을 제공하기 위해 작동하는 다양한 네트워크 토폴로지에 대해 알아봅니다.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>하이브리드 워크로드에 대해 구성된 Azure SQL Database 관리되는 인스턴스 
Azure SQL Database 관리되는 인스턴스가 온-프레미스 네트워크에 연결되어 있는 경우 이 토폴로지를 사용합니다. 이 방법은 가장 간소화된 네트워크 라우팅을 제공하고 마이그레이션 동안 최대 데이터 처리량을 제공합니다.

![하이브리드 워크로드에 대한 네트워크 토폴로지](media\resource-network-topologies\hybrid-workloads.png)

**요구 사항**
- 이 시나리오에서는 Azure SQL Database 관리되는 인스턴스 및 Azure Database Migration Service 인스턴스가 같은 Azure VNET에서 생성되지만 다른 서브넷을 사용합니다.  
- 이 시나리오에 사용된 VNET은 ExpressRoute 또는 VPN을 사용하여 온-프레미스 네트워크에도 연결됩니다.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>온-프레미스 네트워크에서 격리된 Azure SQL Database 관리되는 인스턴스
작업 환경에 다음과 같은 시나리오 중 하나 이상이 필요한 경우 이 네트워크 토폴로지를 사용합니다.
- Azure SQL Database 관리되는 인스턴스는 온-프레미스 연결에서 분리되지만, Azure Database Migration Service 인스턴스는 온-프레미스 네트워크에 연결됩니다.
- RBAC(역할 기반 액세스 제어) 정책이 설정되어 있고 사용자 액세스를 Azure SQL Database 관리되는 인스턴스를 호스트하는 동일한 구독으로 제한합니다.
- Azure SQL Database 관리되는 인스턴스 및 Azure Database Migration Service에 사용되는 VNET은 서로 다른 구독에 있습니다.

![온-프레미스 네트워크에서 격리된 관리되는 인스턴스의 네트워크 토폴로지](media\resource-network-topologies\mi-isolated-workload.png)

**요구 사항**
- Azure Database Migration Service가 이 시나리오에 사용하는 VNET은 ExpressRoute 또는 VPN을 사용하여 온-프레미스 네트워크에도 연결되어야 합니다.
- Azure SQL Database 관리되는 인스턴스 및 Azure Database Migration Service에 사용되는 VNET 간에 VNET 네트워크 피어링을 만듭니다.


## <a name="cloud-to-cloud-migrations"></a>클라우드-클라우드 마이그레이션
원본 SQL Server가 Azure Virtual Machine에 호스트되는 경우 이 토폴로지를 사용합니다.

![클라우드-클라우드 마이그레이션에 대한 네트워크 토폴로지](media\resource-network-topologies\cloud-to-cloud.png)

**요구 사항**
- Azure SQL Database 관리되는 인스턴스 및 Azure Database Migration Service에 사용되는 VNET 간에 VNET 네트워크 피어링을 만듭니다.

## <a name="see-also"></a>참고 항목
- [Azure SQL Database 관리되는 인스턴스로 SQL Server 마이그레이션](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure Database Migration Service 사용을 위한 필수 구성 요소 개요](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure Portal을 사용하여 가상 네트워크 만들기](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>다음 단계
공개 미리 보기 중 Azure Database Migration Service 및 국가별 가용성에 대한 개요는 [Azure Database Migration Service 미리 보기란 무엇인가요?](dms-overview.md) 문서를 참조하세요. 