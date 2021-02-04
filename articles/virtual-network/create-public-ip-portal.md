---
title: 공용 IP 만들기-Azure Portal
description: Azure Portal에서 공용 IP를 만드는 방법에 대해 알아봅니다.
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: 7d0c83f1ae18d36557a7a5b0222aee2905e05cb7
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550238"
---
# <a name="quickstart-create-a-public-ip-address-using-the-azure-portal"></a>빠른 시작: Azure Portal을 사용 하 여 공용 IP 주소 만들기

이 문서에서는 Azure Portal를 사용 하 여 공용 IP 주소 리소스를 만드는 방법을 보여 줍니다. 이에 연결할 수 있는 리소스, 기본 및 표준 SKU 간의 차이점 및 기타 관련 정보에 대 한 자세한 내용은 [공용 IP 주소](./public-ip-addresses.md)를 참조 하세요.  이 예에서는 IPv4 주소에만 중점을 둡니다. IPv6 주소에 대 한 자세한 내용은 [Azure VNet 용 ipv6](./ipv6-overview.md)을 참조 하세요.

# <a name="standard-sku"></a>[**표준 SKU**](#tab/option-create-public-ip-standard-zones)

다음 단계를 사용 하 여 **Mystandardzrpublicip** 라는 표준 영역 중복 공용 IP 주소를 만듭니다.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **리소스 만들기** 를 선택합니다. 
3. 검색 상자에 *공용 IP 주소* 를 입력합니다.
4. 검색 결과에서 **공용 IP 주소** 를 선택합니다. 그런 다음, **공용 IP 주소** 페이지에서 **만들기** 를 선택합니다.
5. **공용 IP 주소 만들기** 페이지에서 다음 정보를 입력 하거나 선택 합니다. 

    | Setting                 | 값                       |
    | ---                     | ---                         |
    | IP 버전              | IPv4 선택                 |    
    | SKU                     | **표준** 선택         |
    | 계층 (표시 된 경우)                  | **지역** 선택         |
    | Name                    | *Mystandardzrpublicip* 를 입력 합니다.          |
    | IP 주소 할당   | 이는 "정적"으로 잠깁니다.                                        |
    | 유휴 시간 제한 (분)  | 값을 4로 그대로 둡니다.        |
    | DNS 이름 레이블          | 값을 공백으로 둡니다.    |
    | Subscription            | 구독을 선택합니다.   |
    | Resource group          | **새로 만들기** 를 선택 하 고 myresourcegroup을 입력 한 다음 **확인** 을 선택 합니다. |
    | 위치                | **미국 동부 2** 선택      |
    | 가용성 영역       | **영역 중복**, 영역 없음 또는 특정 영역 선택 (아래 참고 참조) |

이러한 항목은 [가용성 영역](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones)있는 지역 에서만 유효 합니다.  이러한 지역에서 특정 영역을 선택할 수도 있지만 영역 오류에 대 한 복원 력이는 없습니다.  가용성 영역에 대 한 자세한 내용은 [가용성 영역 개요](https://docs.microsoft.com/azure/availability-zones/az-overview)를 참조 하세요.

\* = 계층은 현재 미리 보기로 제공 되는 [지역 간 Load Balancer](../load-balancer/cross-region-overview.md) 기능과 관련이 있습니다.

# <a name="basic-sku"></a>[**기본 SKU**](#tab/option-create-public-ip-basic)

다음 단계를 사용 하 여 **Mybasicpublicip** 이라는 기본 고정 공용 IP 주소를 만듭니다.  기본 공용 Ip에는 가용성 영역의 개념이 없습니다.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.
2. **리소스 만들기** 를 선택합니다. 
3. 검색 상자에 *공용 IP 주소* 를 입력합니다.
4. 검색 결과에서 **공용 IP 주소** 를 선택합니다. 그런 다음, **공용 IP 주소** 페이지에서 **만들기** 를 선택합니다.
5. **공용 IP 주소 만들기** 페이지에서 다음 정보를 입력 하거나 선택 합니다. 

    | Setting                 | 값                       |
    | ---                     | ---                         |
    | IP 버전              | IPv4 선택                 |    
    | SKU                     | **표준** 선택         |
    | Name                    | *Mybasicpublicip* 입력          |
    | IP 주소 할당   | **정적** 선택 (아래 참고 참조)                                     |
    | 유휴 시간 제한 (분)  | 값을 4로 그대로 둡니다.        |
    | DNS 이름 레이블          | 값을 공백으로 둡니다.    |
    | Subscription            | 구독을 선택합니다.   |
    | Resource group          | **새로 만들기** 를 선택 하 고 myresourcegroup을 입력 한 다음 **확인** 을 선택 합니다. |
    | 위치                | **미국 동부 2** 선택      |

시간에 따라 IP 주소를 변경 하는 것이 허용 되는 경우 **동적** ip 할당을 선택할 수 있습니다.

---

## <a name="additional-information"></a>추가 정보 

위에 나열 된 개별 필드에 대 한 자세한 내용은 [공용 IP 주소 관리](./virtual-network-public-ip-address.md#create-a-public-ip-address)를 참조 하세요.

## <a name="next-steps"></a>다음 단계
- [가상 컴퓨터에 공용 IP 주소](./associate-public-ip-address-vm.md#azure-portal) 연결
- Azure에서 [공용 IP 주소](./public-ip-addresses.md#public-ip-addresses)에 대해 자세히 알아봅니다.
- 모든 [공용 IP 주소 설정](virtual-network-public-ip-address.md#create-a-public-ip-address)에 대해 자세히 알아봅니다.
