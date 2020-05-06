---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 21c19027d21a87e199d74644cfc5c8f3cd52ba4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79129426"
---
장치를 다시 설정 하려면 데이터 디스크의 모든 데이터와 장치의 부팅 디스크를 안전 하 게 초기화 해야 합니다. 

`Reset-HcsAppliance` Cmdlet을 사용 하 여 데이터 디스크와 부팅 디스크 또는 데이터 디스크를 모두 지울 수 있습니다. `ClearData` 및 `BootDisk` 스위치를 사용 하 여 데이터 디스크와 부팅 디스크를 각각 초기화할 수 있습니다.

스위치 `BootDisk` 는 부팅 디스크를 초기화 하 고 장치를 사용할 수 없게 만듭니다. 장치를 Microsoft에 반환 해야 하는 경우에만 사용 해야 합니다. 자세한 내용은 [Microsoft로 장치 반환](https://docs.microsoft.com/azure/databox-online/data-box-edge-return-device)을 참조 하세요.

로컬 웹 UI에서 장치 재설정을 사용 하는 경우 데이터 디스크만 안전 하 게 초기화 되지만 부팅 디스크는 그대로 유지 됩니다. 부팅 디스크에는 장치 구성이 포함 됩니다.

1. [PowerShell 인터페이스에 연결](#connect-to-the-powershell-interface)합니다.
2. 명령 프롬프트에서 다음을 입력합니다.

    `Reset-HcsAppliance -ClearData -BootDisk`

    다음 예에서는이 cmdlet을 사용 하는 방법을 보여 줍니다.

    ```powershell
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk

    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
