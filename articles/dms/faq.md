---
title: FAQ-Azure Database Migration Service
description: Azure Database Migration Service를 사용 하 여 데이터베이스 마이그레이션을 수행 하는 방법에 대 한 질문과 대답입니다.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 02/20/2020
ms.openlocfilehash: f4d65c97bfccd223453583b25ee0586c5bc0b1ec
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101091444"
---
# <a name="faq-about-using-azure-database-migration-service"></a>Azure Database Migration Service 사용에 대 한 FAQ

이 문서에서는 관련 응답과 함께 Azure Database Migration Service를 사용 하는 방법에 대 한 일반적인 질문과 대답을 제공 합니다.

## <a name="overview"></a>개요

**대답. Azure Database Migration Service 이란?**
Azure Database Migration Service은 가동 중지 시간을 최소화 하면서 여러 데이터베이스 소스에서 Azure 데이터 플랫폼으로 원활 하 게 마이그레이션할 수 있도록 설계 된 완전히 관리 되는 서비스입니다. 이 서비스는 현재 일반 공급으로 제공 되며 지속적인 개발 노력이 다음과 같이 집중 됩니다.

* 안정성 및 성능.
* 원본 -대상 쌍의 반복적 추가
* 충돌 없는 마이그레이션에 대한 지속적인 투자

**대답. 현재 지원 Azure Database Migration Service 소스/대상 쌍은 무엇 인가요?**
서비스는 현재 다양 한 원본/대상 쌍 또는 마이그레이션 시나리오를 지원 합니다. 사용 가능한 각 마이그레이션 시나리오의 상태에 대한 전체 목록은 [Azure Database Migration Service에서 지원하는 마이그레이션 시나리오의 상태](./resource-scenario-status.md) 문서를 참조하세요.

다른 마이그레이션 시나리오는 미리 보기 상태 이며 DMS Preview 사이트를 통해 추천을 제출 해야 합니다. 미리 보기로 제공 되는 시나리오의 전체 목록과 등록 하려면 [DMS preview 사이트](https://aka.ms/dms-preview/)를 참조 하세요.

**대답. 원본으로 지원 Azure Database Migration Service SQL Server 버전**
SQL Server에서 마이그레이션할 때 Azure Database Migration Service에 대해 지원 되는 소스는 2005 SQL Server에서 SQL Server 2019 까지입니다.

**Q: Azure Database Migration Service를 사용 하는 경우 오프 라인 마이그레이션과 온라인 마이그레이션 간의 차이점은 무엇 인가요?**
Azure Database Migration Service를 사용 하 여 오프 라인 및 온라인 마이그레이션을 수행할 수 있습니다. *오프 라인* 마이그레이션을 사용 하면 마이그레이션이 시작 될 때 응용 프로그램 가동 중지 시간이 시작 됩니다. *온라인* 마이그레이션을 사용 하는 경우 마이그레이션 종료 시 중단 시간으로 제한 됩니다. 오프라인 마이그레이션을 테스트하여 가동 중지 시간이 용납 가능한 수준인지 판단하고, 용납되지 않는다면 온라인 마이그레이션을 수행하는 것이 좋습니다.

> [!NOTE]
> Azure Database Migration Service를 사용하여 온라인 마이그레이션을 수행하려면 프리미엄 가격 책정 계층에 따라 인스턴스를 만들어야 합니다. 자세한 내용은 Azure Database Migration Service [가격 책정](https://azure.microsoft.com/pricing/details/database-migration/) 페이지를 참조하세요.

**대답. Azure Database Migration Service은 데이터베이스 Migration Assistant (DMA) 또는 SQL Server Migration Assistant (SSMA)와 같은 다른 Microsoft 데이터베이스 마이그레이션 도구와 어떻게 비교 되나요?**
Azure Database Migration Service은 데이터베이스 마이그레이션이 대규모로 Microsoft Azure 되는 기본 방법입니다. Azure Database Migration Service 다른 Microsoft 데이터베이스 마이그레이션 도구와 비교 하는 방법 및 다양 한 시나리오에 서비스 사용에 대 한 권장 사항에 대 한 자세한 내용은 [Microsoft의 데이터베이스 마이그레이션 도구 및 서비스 차별화](https://techcommunity.microsoft.com/t5/microsoft-data-migration/differentiating-microsoft-s-database-migration-tools-and/ba-p/368529)의 블로그 게시물을 참조 하세요.

**대답. Azure Database Migration Service는 Azure Migrate 제공과 어떻게 비교 되나요?**
Azure Migrate 온-프레미스 가상 머신을 Azure IaaS로 마이그레이션하는 데 도움이 됩니다. 이 서비스는 마이그레이션 적합성 및 성능 기반 크기 조정을 평가하며, Azure에서 온-프레미스 가성 머신을 실행할 때 드는 비용을 예측합니다. Azure Migrate는 온-프레미스 VM 기반 워크로드를 Azure IaaS VM으로 리프트 앤 시프트 마이그레이션하는 데 유용합니다. 그러나 Azure Database Migration Service와 달리 Azure Migrate는 Azure SQL Database 또는 Azure SQL Managed Instance와 같은 Azure PaaS 관계형 데이터베이스 플랫폼에 대 한 특수화 된 database Migration Service 제품이 아닙니다.

**대답. 고객 데이터를 저장 Database Migration Service 합니까?**
아니요. Database Migration Service는 고객 데이터를 저장 하지 않습니다.

## <a name="setup"></a>설치

**대답. Azure Database Migration Service 사용을 위한 필수 구성 요소는 무엇 인가요?**
데이터베이스 마이그레이션을 수행할 때 Azure Database Migration Service 원활 하 게 실행 되도록 하기 위해 필요한 몇 가지 필수 구성 요소가 있습니다. 일부 필수 구성 요소는 서비스가 지원하는 모든 시나리오(원본-대상 쌍)에 적용되는 반면에 특정 시나리오에만 적용되는 필수 구성 요소도 있습니다.

지원되는 모든 마이그레이션 시나리오에 공통적인 Azure Database Migration Service 필구 구성 요소는 다음을 수행해야 합니다.

* Azure Resource Manager 배포 모델을 사용하여 Azure Database Migration Service용 Microsoft Azure Virtual Network를 만듭니다. 그러면 [ExpressRoute](../expressroute/expressroute-introduction.md) 또는 [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md)을 사용하여 온-프레미스 원본 서버에 사이트 간 연결이 제공됩니다.
* 가상 네트워크 네트워크 보안 그룹 규칙이 ServiceTags ServiceBus, Storage 및 AzureMonitor에 대 한 포트 443을 차단 하지 않는지 확인 합니다. 가상 네트워크 NSG 트래픽 필터링에 대한 자세한 내용은 [네트워크 보안 그룹을 사용하여 네트워크 트래픽 필터링](../virtual-network/virtual-network-vnet-plan-design-arm.md) 문서를 참조하세요.
* 원본 데이터베이스 앞에 방화벽 어플라이언스를 사용하는 경우 Azure Database Migration Service에서 원본 데이터베이스에 액세스하여 마이그레이션할 수 있도록 허용하는 방화벽 규칙을 추가해야 합니다.

Azure Database Migration Service를 사용 하 여 특정 마이그레이션 시나리오를 경합 하는 데 필요한 모든 필수 구성 요소 목록은 docs.microsoft.com의 Azure Database Migration Service [설명서](./dms-overview.md) 에서 관련 자습서를 참조 하십시오.

**대답. 마이그레이션을 위해 원본 데이터베이스에 액세스 하는 데 사용 되는 방화벽 규칙에 대 한 허용 목록을 만들 수 있도록 Azure Database Migration Service에 대 한 IP 주소를 찾을 어떻게 할까요??**
마이그레이션을 위해 원본 데이터베이스에 대 한 액세스 Azure Database Migration Service 허용 하는 방화벽 규칙을 추가 해야 할 수 있습니다. 서비스의 IP 주소는 동적 이지만 Express 경로를 사용 하는 경우이 주소는 회사 네트워크에서 개인적으로 할당 합니다. 적절 한 IP 주소를 식별 하는 가장 쉬운 방법은 프로 비전 된 Azure Database Migration Service 리소스와 동일한 리소스 그룹에서 연결 된 네트워크 인터페이스를 찾는 것입니다. 일반적으로 네트워크 인터페이스 리소스의 이름은 NIC 접두사로 시작되고 그 뒤에 고유한 문자와 숫자 시퀀스가 붙습니다(예 : NIC-jj6tnztnmarpsskr82rbndyp). 이 네트워크 인터페이스 리소스를 선택하면 Azure Portal 리소스 개요 페이지에서 허용 목록에 포함되어야 하는 IP 주소를 볼 수 있습니다.

SQL Server가 수신 대기하는 포트 원본을 허용 목록에 포함해야 할 수도 있습니다. 기본적으로 이 포트는 1433이지만 원본 SQL Server는 다른 포트도 수신 대기하도록 구성될 수 있습니다. 이 경우 해당 포트도 허용 목록에 포함시켜야 합니다. 동적 관리 뷰 쿼리를 사용하여 SQL Server가 수신 대기하는 포트를 확인할 수 있습니다.

```sql
    SELECT DISTINCT
        local_tcp_port
    FROM sys.dm_exec_connections
    WHERE local_tcp_port IS NOT NULL
```

SQL Server 오류 로그를 쿼리하여 SQL Server가 수신 대기하는 포트를 확인할 수도 있습니다.

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on'
    GO
```

**대답. Microsoft Azure Virtual Network 어떻게 할까요? 설정 하 시겠습니까?**
가상 네트워크를 설정 하는 과정을 안내 하는 여러 Microsoft 자습서는 있지만 [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)문서에 공식 설명서가 나와 있습니다.

## <a name="usage"></a>사용량

**대답. Azure Database Migration Service를 사용 하 여 데이터베이스 마이그레이션을 수행 하는 데 필요한 단계에 대 한 요약은 무엇입니까?**
일반적이고 간단한 데이터베이스 마이그레이션 단계:

1. 대상 데이터베이스를 만듭니다.
2. 원본 데이터베이스를 평가 합니다.
    * 동일한 마이그레이션의 경우 [DMA](https://www.microsoft.com/download/details.aspx?id=53595)를 사용 하 여 기존 데이터베이스를 평가 합니다.
    * 다른 유형의 마이그레이션 (경쟁 원본에서)의 경우 [Ssma](/sql/ssma/sql-server-migration-assistant)를 사용 하 여 기존 데이터베이스를 평가 합니다. 또한 SSMA를 사용 하 여 데이터베이스 개체를 변환 하 고 대상 플랫폼으로 스키마를 마이그레이션합니다.
3. Azure Database Migration Service 인스턴스를 만듭니다.
4. 원본 데이터베이스, 대상 데이터베이스 및 마이그레이션할 테이블을 지정 하는 마이그레이션 프로젝트를 만듭니다.
5. 전체 로드를 시작 합니다.
6. 후속 유효성 검사를 선택합니다.
7. 새 클라우드 기반 데이터베이스로 프로덕션 환경의 수동 전환을 수행합니다.

## <a name="troubleshooting-and-optimization"></a>문제 해결 및 최적화

**대답. DMS로 마이그레이션 프로젝트를 설정 하 고 있으며 원본 데이터베이스에 연결 하는 데 어려움이 있습니다. 제가 뭘 해야 하나요?**
마이그레이션 작업을 수행 하는 동안 원본 데이터베이스 시스템에 연결 하는 데 문제가 있는 경우 DMS 인스턴스를 설정 하는 가상 네트워크의 동일한 서브넷에 가상 컴퓨터를 만듭니다. 가상 컴퓨터에서 UDL 파일을 사용 하 여 SQL Server에 대 한 연결을 테스트 하거나 MongoDB 연결을 테스트 하기 위해 Robo 3T를 다운로드 하는 등의 연결 테스트를 실행할 수 있어야 합니다. 연결 테스트가 성공 하면 원본 데이터베이스에 연결 하는 데 문제가 없어야 합니다. 연결 테스트에 성공 하지 못한 경우 네트워크 관리자에 게 문의 하십시오.

**대답. 내 Azure Database Migration Service를 사용할 수 없거나 중지 된 이유는 무엇 인가요?**
사용자가 명시적으로 Azure Database Migration Service (DMS)를 중지 하거나 서비스가 24 시간 동안 비활성 상태 이면 서비스는 중지 됨 또는 자동 일시 중지 됨 상태가 됩니다. 각각의 경우에서 서비스는 사용할 수 없으며 중지된 상태에 있게 됩니다.  활성 마이그레이션을 다시 시작하려면 서비스를 다시 시작합니다.

**대답. Azure Database Migration Service의 성능을 최적화 하기 위한 권장 사항이 있나요?**
몇 가지 작업을 수행하면 서비스를 사용한 데이터베이스 마이그레이션 속도를 높일 수 있습니다.

* 서비스 인스턴스를 만들 때 다중 CPU 범용 가격 책정 계층을 사용하면 서비스에서 다중 vCPU를 활용하여 병렬 처리 및 빠른 데이터 전송이 가능합니다.
* 하위 수준의 SKU를 사용할 때 데이터 전송 작업에 영향을 줄 수 있는 Azure SQL 데이터베이스 제한을 최소화하기 위해 데이터 마이그레이션 작업 중에 Azure SQL Database 대상 인스턴스를 프리미엄 계층 SKU로 일시 강화합니다.

## <a name="next-steps"></a>다음 단계

Azure Database Migration Service 및 국가별 가용성에 대한 개요는 [Azure Database Migration Service란?](dms-overview.md) 문서를 참조하세요.
