---
title: 관리 되는 가상 네트워크 & 관리 되는 전용 끝점
description: Azure Data Factory에서 관리 되는 가상 네트워크 및 관리 되는 개인 끝점에 대해 알아봅니다.
ms.author: abnarain
author: nabhishek
ms.service: data-factory
ms.topic: conceptual
ms.custom:
- seo-lt-2019
- references_regions
ms.date: 07/15/2020
ms.openlocfilehash: d950b05dd34788c2c5ef0b34b8ec8ac0b20ad4b6
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379576"
---
# <a name="azure-data-factory-managed-virtual-network-preview"></a>Azure Data Factory 관리 Virtual Network (미리 보기)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

이 문서에서는 Azure Data Factory에서 관리 되는 Virtual Network 및 관리 되는 개인 끝점을 설명 합니다.


## <a name="managed-virtual-network"></a>관리형 가상 네트워크

Azure Data Factory 관리 되는 Virtual Network (VNET) 내에서 IR (Azure Integration Runtime)을 만들면 Integration Runtime이 관리 되는 Virtual Network 프로 비전 되 고 개인 끝점을 활용 하 여 지원 되는 데이터 저장소에 안전 하 게 연결 됩니다. 

관리 되는 Virtual Network 내 Azure IR를 만들면 데이터 통합 프로세스가 격리 되 고 안전 하 게 유지 됩니다. 

관리 되는 Virtual Network 사용의 이점:

- 관리 되는 Virtual Network을 사용 하면 Virtual Network를 관리 하는 부담을 Azure Data Factory으로 오프 로드할 수 있습니다. 궁극적으로 Virtual Network에서 많은 개인 Ip를 사용 하 고 이전 네트워크 인프라를 계획 해야 하는 Azure Integration Runtime에 대 한 서브넷을 만들 필요가 없습니다. 
- 데이터 통합을 안전 하 게 수행 하기 위해 심층 Azure 네트워킹 정보는 필요 하지 않습니다. 대신 보안 ETL을 시작 하는 것은 데이터 엔지니어에 게 매우 간단 합니다. 
- 관리 되는 Virtual Network는 관리 되는 개인 끝점과 함께 데이터 exfiltration을 방지 합니다. 

> [!IMPORTANT]
>현재 관리 되는 VNet은 Azure Data Factory 지역과 동일한 지역 에서만 지원 됩니다.
 

![ADF 관리 Virtual Network 아키텍처](./media/managed-vnet/managed-vnet-architecture-diagram.png)

## <a name="managed-private-endpoints"></a>관리형 프라이빗 엔드포인트

관리 되는 개인 끝점은 Azure 리소스에 대 한 개인 링크를 설정 하는 Virtual Network Azure Data Factory 관리 되는 전용 끝점입니다. Azure Data Factory은 사용자 대신 이러한 개인 끝점을 관리 합니다. 

![새 관리형 프라이빗 엔드포인트](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png)

Azure Data Factory은 개인 링크를 지원 합니다. 개인 링크를 사용 하면 Azure (PaaS) 서비스 (예: Azure Storage, Azure Cosmos DB, Azure Synapse Analytics)에 액세스할 수 있습니다.

개인 링크를 사용 하는 경우 데이터 저장소와 관리 되는 Virtual Network 간의 트래픽이 Microsoft 백본 네트워크를 통해 완전히 트래버스 됩니다. Private Link는 데이터 반출 위험을 방지합니다. 프라이빗 엔드포인트를 만들어 리소스에 대한 프라이빗 링크를 설정합니다.

개인 끝점은 관리 되는 Virtual Network에서 개인 IP 주소를 사용 하 여 서비스를 효과적으로 가져옵니다. 프라이빗 엔드포인트는 전체 서비스가 아닌 Azure의 특정 리소스에 매핑됩니다. 고객은 연결 범위를 조직에서 승인한 특정 리소스로 제한할 수 있습니다. [프라이빗 링크 및 프라이빗 엔드포인트](../private-link/index.yml)에 대해 자세히 알아보세요.

> [!NOTE]
> 모든 Azure 데이터 원본에 연결하는 관리형 프라이빗 엔드포인트를 만드는 것이 좋습니다. 
 
> [!WARNING]
> PaaS 데이터 저장소 (Blob, ADLS Gen2, Azure Synapse Analytics)에 이미이에 대해 만들어진 개인 끝점이 있고 모든 네트워크에서 액세스할 수 있는 경우에도 ADF는 관리 되는 개인 끝점을 사용 하 여 액세스할 수 있습니다. 이러한 시나리오에서는 개인 끝점을 만들어야 합니다. 

Azure Data Factory에서 관리 되는 개인 끝점을 만들 때 "보류 중" 상태로 개인 끝점 연결이 생성 됩니다. 승인 워크플로가 시작됩니다. 프라이빗 링크 리소스 소유자가 연결을 승인 또는 거부합니다.

![프라이빗 엔드포인트 관리](./media/tutorial-copy-data-portal-private/manage-private-endpoint.png)

소유자가 연결을 승인하면 프라이빗 링크가 설정됩니다. 거부하면 프라이빗 링크가 설정되지 않습니다. 어떤 경우든 관리형 프라이빗 엔드포인트는 연결 상태로 업데이트 됩니다.

![관리 되는 개인 끝점 승인](./media/tutorial-copy-data-portal-private/approve-private-endpoint.png)

승인된 상태의 관리형 프라이빗 엔드포인트만이 지정된 프라이빗 링크 리소스에 트래픽을 보낼 수 있습니다.

## <a name="interactive-authoring"></a>대화형 제작
대화형 제작 기능은 연결 테스트, 폴더 목록 및 테이블 목록 찾아보기, 스키마 가져오기, 데이터 미리 보기 등의 기능에 사용 됩니다. ADF로 관리 되는 가상 네트워크에 있는 Azure Integration Runtime을 만들거나 편집할 때 대화형 작성을 사용 하도록 설정할 수 있습니다. 백 엔드 서비스는 대화형 제작 기능을 위해 계산을 미리 할당 합니다. 그렇지 않은 경우에는 더 많은 시간이 소요 되는 대화형 작업이 수행 될 때마다 계산이 할당 됩니다. 대화형 작성에 대 한 TTL (Time To Live)은 60 분입니다. 즉, 마지막 대화형 작성 작업의 60 분 후에 자동으로 사용 하지 않도록 설정 됩니다.

![대화형 작성](./media/managed-vnet/interactive-authoring.png)

## <a name="limitations-and-known-issues"></a>제한 사항 및 알려진 문제
### <a name="supported-data-sources"></a>지원되는 데이터 원본
아래 데이터 원본은 ADF 관리 Virtual Network에서 개인 링크를 통해 연결할 수 있습니다.
- Azure Blob Storage
- Azure Table Storage
- Azure 파일
- Azure Data Lake Gen2
- Azure SQL Database (Azure SQL Managed Instance를 포함 하지 않음)
- Azure Synapse Analytics
- Azure CosmosDB SQL
- Azure Key Vault
- Azure 개인 링크 서비스
- Azure Search
- Azure Database for MySQL
- Azure Database for PostgreSQL
- Azure Database for MariaDB

### <a name="azure-data-factory-managed-virtual-network-is-available-in-the-following-azure-regions"></a>Azure Data Factory 관리 Virtual Network는 다음 Azure 지역에서 사용할 수 있습니다.
- 미국 동부
- 미국 동부 2
- 미국 중서부
- 미국 서부
- 미국 서부 2
- 미국 중남부
- 미국 중부
- 북유럽
- 서유럽
- 영국 남부
- 동남 아시아
- 오스트레일리아 동부
- 오스트레일리아 남동부

### <a name="outbound-communications-through-public-endpoint-from-adf-managed-virtual-network"></a>ADF 관리 Virtual Network에서 공용 끝점을 통한 아웃 바운드 통신
- 아웃 바운드 통신용 포트 443만 열립니다.
- Azure Storage 및 Azure Data Lake Gen2는 ADF 관리 Virtual Network의 공용 끝점을 통해 연결할 수 없습니다.

### <a name="linked-service-creation-of-azure-key-vault"></a>Azure Key Vault 연결 된 서비스 만들기 
- Azure Key Vault에 대한 연결된 서비스를 만들 때 Azure Integration Runtime 참조가 없습니다. 따라서 연결 된 서비스를 만드는 동안에는 Azure Key Vault 개인 끝점을 만들 수 없습니다. 그러나 Azure Key Vault 연결 된 서비스를 참조 하는 데이터 저장소에 대 한 연결 된 서비스를 만들 때이 연결 된 서비스에서 관리 되는 Virtual Network를 사용 하도록 설정 된 Azure Integration Runtime 참조 하는 경우 생성 하는 동안 Azure Key Vault 연결 된 서비스에 대 한 개인 끝점을 만들 수 있습니다. 
- 연결 된 Azure Key Vault 서비스에 대 한 **연결 테스트** 작업은 URL 형식의 유효성만 검사 하지만 네트워크 작업은 수행 하지 않습니다.

## <a name="next-steps"></a>다음 단계

- 자습서: [관리 되는 Virtual Network 및 개인 끝점을 사용 하 여 복사 파이프라인 빌드](tutorial-copy-data-portal-private.md) 
- 자습서: [관리 되는 Virtual Network 및 개인 끝점을 사용 하 여 데이터 흐름 파이프라인 빌드 매핑](tutorial-data-flow-private.md)