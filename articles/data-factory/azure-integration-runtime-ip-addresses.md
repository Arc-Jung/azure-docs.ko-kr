---
title: Azure Integration Runtime IP 주소
description: 데이터 저장소에 대 한 네트워크 액세스를 보호 하기 위해 방화벽을 제대로 구성 하기 위해에서 인바운드 트래픽을 허용 해야 하는 IP 주소에 대해 알아봅니다.
ms.author: abnarain
author: nabhishek
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/06/2020
ms.openlocfilehash: 7b663c8d6e5849d39bb8366c82f45e0fd66d77dd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100371399"
---
# <a name="azure-integration-runtime-ip-addresses"></a>Azure Integration Runtime IP 주소

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Integration Runtime 사용 하는 IP 주소는 Azure Integration Runtime이 있는 지역에 따라 다릅니다. *모두* 동일한 지역에 있는 Azure integration runtime은 동일한 IP 주소 범위를 사용 합니다.

> [!IMPORTANT]  
> 관리 Virtual Network를 사용 하도록 설정 하는 데이터 흐름 및 Azure Integration Runtime 고정 IP 범위의 사용을 지원 하지 않습니다.
>
> 이러한 IP 범위는 데이터 이동, 파이프라인 및 외부 활동 실행에 사용할 수 있습니다. 이러한 IP 범위는 Azure Integration runtime에서 인바운드 액세스를 위해 데이터 저장소/n e t i n g (네트워크 보안 그룹)/방화벽에서 필터링 하는 데 사용할 수 있습니다. 

## <a name="azure-integration-runtime-ip-addresses-specific-regions"></a>Azure Integration Runtime IP 주소: 특정 지역

리소스가 있는 특정 Azure 지역에서 Azure Integration runtime에 대해 나열 된 IP 주소의 트래픽을 허용 합니다. [서비스 태그 IP 범위 다운로드 링크](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)에서 서비스 태그의 IP 범위 목록을 가져올 수 있습니다. 예를 들어 Azure 지역이 **AustraliaEast** 인 경우 **DATAFACTORY AustraliaEast** 에서 IP 범위 목록을 가져올 수 있습니다.


## <a name="known-issue-with-azure-storage"></a>Azure Storage의 알려진 문제

* Azure Storage 계정에 연결할 때 IP 네트워크 규칙은 저장소 계정과 동일한 지역의 Azure integration runtime에서 시작 된 요청에 영향을 주지 않습니다. 자세한 내용은 [이 문서를 참조](../storage/common/storage-network-security.md#grant-access-from-an-internet-ip-range)하세요. 

  대신 [Azure Storage에 연결 하는 동안 신뢰할 수 있는 서비스](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993)를 사용 하는 것이 좋습니다. 

## <a name="next-steps"></a>다음 단계

* [Azure Data Factory에서 데이터 이동을 위한 보안 고려 사항](data-movement-security-considerations.md)