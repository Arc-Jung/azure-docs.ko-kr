---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: styli365
ms.service: virtual-machines
ms.topic: include
ms.date: 11/05/2020
ms.author: sttsinar
ms.custom: include file
ms.openlocfilehash: 3d78441e56e23cf49b09073fdf88bef4b3434da9
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473782"
---
Azure Compute는 특정 하드웨어 유형에서 격리되고 단일 고객 전용인 가상 머신 크기를 제공합니다. 격리 된 크기는 라이브 및 특정 하드웨어 생성에 대해 작동 하며 하드웨어 생성이 사용 중지 되 면 사용 되지 않습니다.

격리 된 가상 머신 크기는 다른 고객의 워크 로드와의 높은 수준의 격리를 필요로 하는 작업에 가장 적합 하며,이는 회의 규정 준수 및 규제 요구 사항을 포함 합니다.  격리 크기를 사용하면 가상 머신이 특정 서버 인스턴스에서 하나만 실행되도록 보장합니다. 


또한 격리 된 크기의 Vm이 큰 경우 고객은 [중첩 된 가상 컴퓨터에 대 한 Azure 지원](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)을 사용 하 여 이러한 vm의 리소스를 세분 하도록 선택할 수 있습니다.

현재 격리 가상 머신 제품에는 다음이 포함됩니다.
* Standard_E80ids_v4
* Standard_E80is_v4
* Standard_F72s_v2
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5


> [!NOTE]
> 격리 된 VM 크기는 하드웨어의 수명이 제한적입니다. 자세한 내용은 아래를 참조 하세요.

## <a name="deprecation-of-isolated-vm-sizes"></a>격리 된 VM 크기의 사용 중단

격리 된 VM 크기는 하드웨어의 수명이 제한적입니다. Azure는 크기의 공식 사용 중단 날짜를 미리 미리 알림을 12 개월 후 고려 하 여 업데이트 된 격리 된 제품을 제공 합니다.

| 크기 | 격리 사용 중지 날짜 | 
| --- | --- |
| Standard_DS15_v2 | 2020년 5월 15일 |
| Standard_D15_v2  | 2020년 5월 15일 |
| Standard_G5  | 2021 년 2 월 15 일 |
| Standard_GS5  | 2021 년 2 월 15 일 |
| Standard_E64i_v3  | 2021 년 2 월 15 일 |
| Standard_E64is_v3  | 2021 년 2 월 15 일 |


## <a name="faq"></a>FAQ
### <a name="q-is-the-size-going-to-get-retired-or-only-its-isolation-feature"></a>Q: 크기는 사용 중지 되거나 "격리" 기능 으로만 사용 되나요?
**A**: 현재 VM 크기의 격리 기능만 사용 중지 됩니다. 사용 되지 않는 격리 된 크기는 격리 되지 않은 상태로 계속 유지 됩니다. 격리가 필요 하지 않은 경우에는 수행할 작업이 없으며 VM은 예상 대로 계속 작동 합니다.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>Q: vm이 격리 되지 않은 하드웨어에 있는 경우 가동 중지 시간이 있나요?
**A**: 격리가 필요 하지 않은 경우에는 작업이 필요 하지 않으며 가동 중지 시간이 발생 하지 않습니다. 격리를 반드시 수행 해야 하는 경우에는 발표에 권장 되는 대체 크기를 포함 합니다. 교체 크기를 선택 하면 고객이 Vm의 크기를 조정 해야 합니다.  

### <a name="q-is-there-any-cost-delta-for-moving-to-a-non-isolated-virtual-machine"></a>Q: 격리 되지 않은 가상 머신으로 전환 하는 데 비용 델타가 있나요?
**A**: 아니요

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>Q: 다른 격리 된 크기는 언제 사용 중지 될 예정 인가요?
**A**: 격리 된 크기의 공식 사용 중단 전에 미리 알림을 12 개월 후에 제공 합니다. 최신 공지에는 Standard_G5, Standard_GS5, Standard_E64i_v3 및 Standard_E64i_v3의 격리 기능 사용 중지가 포함 되어 있습니다.  

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>Q: 실버 또는 골드 내구성 계층에 의존 하는 Azure Service Fabric 고객입니다. 이 변경 내용이 영향을 미칩니까?
**A**: 아니요. 이 변경 후에도 Service Fabric의 [내구성 계층](../articles/service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) 에서 제공 하는 보장은 계속 작동 합니다. 다른 이유로 인해 물리적 하드웨어 격리가 필요한 경우에도 위에서 설명한 작업 중 하나를 수행 해야 할 수 있습니다. 
 
### <a name="q-what-are-the-milestones-for-d15_v2-or-ds15_v2-isolation-retirement"></a>Q: D15_v2 또는 DS15_v2 격리 사용 중지에 대 한 마일스 톤은 무엇 인가요? 
**A**: 
 
| Date | 작업 |
|---|---| 
| 5 월 15 일, 2019<sup>1</sup> | D/DS15_v2 격리 사용 중지 알림| 
| 2020년 5월 15일 | D/DS15_v2 격리 보장 제거| 

<sup>1</sup> 이 크기를 사용 하는 기존 고객은 다음 단계에 대 한 자세한 지침이 포함 된 알림 전자 메일을 받게 됩니다.  

### <a name="q-what-are-the-milestones-for-g5-gs5-e64i_v3-and-e64is_v3-isolation-retirement"></a>Q: G5, Gs5, E64i_v3 및 E64is_v3 격리 사용 중지에 대 한 마일스 톤은 무엇 인가요? 
**A**: 
 
| Date | 작업 |
|---|---|
| 2 월 15 일, 2020<sup>1</sup> | G5/GS5/E64i_v3/E64is_v3 격리 사용 중지 알림 |
| 2 월 15 일 2021 | G5/GS5/E64i_v3/E64is_v3 격리 보장 제거 됨 |

<sup>1</sup> 이 크기를 사용 하는 기존 고객은 다음 단계에 대 한 자세한 지침이 포함 된 알림 전자 메일을 받게 됩니다.  

## <a name="next-steps"></a>다음 단계

고객은 [중첩된 가상 머신에 대한 Azure 지원](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)을 사용하여 이러한 격리된 가상 머신의 리소스를 보다 세분화할 수도 있습니다.
