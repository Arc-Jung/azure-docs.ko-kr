---
title: Azure NetApp Files에 대 한 볼륨의 서비스 수준을 동적으로 변경 | Microsoft Docs
description: 볼륨의 서비스 수준을 동적으로 변경 하는 방법을 설명 합니다.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/14/2021
ms.author: b-juche
ms.openlocfilehash: 7b5bbad1f0691f76c12f161d1dd1f9d6ddc43270
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102184324"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>볼륨의 서비스 수준을 동적으로 변경

> [!IMPORTANT] 
> 복제 대상 볼륨의 서비스 수준을 동적으로 변경 하는 것은 현재 지원 되지 않습니다.

볼륨에 대해 원하는 [서비스 수준을](azure-netapp-files-service-levels.md) 사용 하는 다른 용량 풀로 볼륨을 이동 하 여 기존 볼륨의 서비스 수준을 변경할 수 있습니다. 볼륨에 대한 이러한 내부 서비스 수준 변경에서는 데이터를 마이그레이션할 필요가 없습니다. 또한 볼륨에 대 한 액세스에는 영향을 주지 않습니다.  

이 기능을 사용 하면 요구에 따라 워크 로드 요구 사항을 충족할 수 있습니다.  더 높은 서비스 수준을 사용하여 성능을 향상시키거나 더 낮은 서비스 수준을 사용하여 비용을 최적화하도록 기존 볼륨을 변경할 수 있습니다. 예를 들어 볼륨이 현재 *표준* 서비스 수준을 사용 하는 용량 풀에 있고 해당 볼륨에서 *프리미엄* 서비스 수준을 사용 하려는 경우 *프리미엄* 서비스 수준을 사용 하는 용량 풀로 볼륨을 동적으로 이동할 수 있습니다.  

볼륨을 이동할 용량 풀이 이미 있어야 합니다. 용량 풀에는 다른 볼륨이 포함 될 수 있습니다.  볼륨을 새 용량 풀로 이동 하려는 경우 볼륨을 이동 하기 전에 [용량 풀을 만들어야](azure-netapp-files-set-up-capacity-pool.md) 합니다.  

## <a name="considerations"></a>고려 사항

* 볼륨을 다른 용량 풀로 이동한 후에는 더 이상 이전 볼륨 활동 로그 및 볼륨 메트릭에 액세스할 수 없습니다. 새 용량 풀 아래의 새 활동 로그 및 메트릭으로 볼륨이 시작 됩니다.

* 볼륨을 상위 서비스 수준의 용량 풀로 이동 하는 경우 (예: *표준* 에서 *프리미엄* 또는 *Ultra* service 수준으로 전환) 해당 볼륨을 더 낮은 서비스 수준의 용량 풀 (예: *Ultra* 에서 *프리미엄* 또는 *표준* 으로 이동)로 *다시* 이동 하려면 7 일 이상 기다려야 합니다.  

## <a name="register-the-feature"></a>기능 등록

볼륨을 다른 용량 풀로 이동 하는 기능은 현재 미리 보기 상태입니다. 이 기능을 처음 사용하는 경우 먼저 기능을 등록해야 합니다.

1. 기능을 등록 합니다. 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. 기능 등록의 상태를 확인 합니다. 

    > [!NOTE]
    >  `Registering` 로 변경 하기 전까지 최대 60 분 동안 registrationstate 상태가 될 수 있습니다 `Registered` . 계속 하기 전에 상태가 **등록** 될 때까지 기다립니다.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```
[Azure CLI 명령을](/cli/azure/feature) 사용 하 여 `az feature register` 기능을 `az feature show` 등록 하 고 등록 상태를 표시할 수도 있습니다. 
 
## <a name="move-a-volume-to-another-capacity-pool"></a>볼륨을 다른 용량 풀로 이동

1.  볼륨 페이지에서 변경할 서비스 수준을 가진 볼륨을 마우스 오른쪽 단추로 클릭 합니다. **풀 변경** 을 선택 합니다.

    ![볼륨을 마우스 오른쪽 단추로 클릭 합니다.](../media/azure-netapp-files/right-click-volume.png)

2. 풀 변경 창에서 볼륨을 이동할 대상 용량 풀을 선택 합니다. 

    ![풀 변경](../media/azure-netapp-files/change-pool.png)

3.  **확인** 을 클릭합니다.


## <a name="next-steps"></a>다음 단계  

* [Azure NetApp Files에 대한 서비스 수준](azure-netapp-files-service-levels.md)
* [용량 풀 설정](azure-netapp-files-set-up-capacity-pool.md)
* [볼륨의 용량 풀을 변경 하는 문제 해결](troubleshoot-capacity-pools.md#issues-when-changing-the-capacity-pool-of-a-volume)
