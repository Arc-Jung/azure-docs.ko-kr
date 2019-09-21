---
title: 포함 파일
description: 포함 파일
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 14f5998ee1c562b649257f7dce9ffc2f52a66226
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174973"
---
## <a name="deployment-considerations"></a>배포 고려 사항

* N 시리즈 VM의 가용성에 대해서는 [지역별 사용 가능한 제품](https://azure.microsoft.com/regions/services/)을 참조하세요.

* N 시리즈 VM은 Resource Manager 배포 모델에만 배포할 수 있습니다.

* N 시리즈 VM은 디스크에 지원되는 Azure Storage의 유형이 다양합니다. NC 및 NV VM은 표준 디스크 스토리지(HDD)로 백업되는 VM 디스크만 지원합니다. NCv2, NCv3, ND, NDv2 및 NVv2 VM은 프리미엄 디스크 스토리지(SSD)로 백업되는 VM 디스크만 지원합니다.

* 몇 개의 N 시리즈 VM을 배포하려는 경우 종량제 구독 또는 기타 구매 옵션을 고려합니다. [Azure 무료 계정](https://azure.microsoft.com/free/)을 사용하는 경우, 제한된 수의 Azure 컴퓨팅 코어만 사용할 수 있습니다.

* Azure 구독에서 코어 할당량(지역당)을 늘리고 NC, NCv2, NCv3, ND, NDv2, NV 또는 NVv2 코어에 대한 별도의 할당량을 늘려야 할 수 있습니다. 할당량 증가를 요청하려면 무료로 [온라인 고객 지원 요청을 개설](../articles/azure-supportability/how-to-create-azure-support-request.md) 합니다. 기본 제한은 구독 범주에 따라 달라질 수 있습니다.




