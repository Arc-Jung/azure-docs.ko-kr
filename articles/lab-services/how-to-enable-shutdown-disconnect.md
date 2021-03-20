---
title: Azure Lab Services에서 랩에 대 한 Vm의 자동 종료 구성
description: 원격 데스크톱 연결이 끊어질 때 VM 자동 종료를 사용하거나 사용하지 않도록 설정하는 방법에 대해 알아봅니다.
ms.topic: article
ms.date: 10/01/2020
ms.openlocfilehash: 8f9080f3b7b762d3b9fa448a903a4167cd2cec4a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96433939"
---
# <a name="configure-automatic-shutdown-of-vms-for-a-lab"></a>랩에 대 한 Vm의 자동 종료 구성

이 문서에서는 랩에 대 한 Vm의 자동 종료를 구성할 수 있는 방법을 보여 줍니다.

여러 자동 종료 비용 제어 기능을 사용 하도록 설정 하 여 가상 컴퓨터를 적극적으로 사용 하지 않을 때 추가 비용을 사전에 방지할 수 있습니다. 다음 세 가지 자동 종료 및 연결 끊기 기능의 조합은 사용자가 실수로 가상 컴퓨터를 실행 하는 대부분의 경우를 포착 합니다.
 
* OS가 유휴 상태로 하다 고 판단 가상 머신에서 사용자를 자동으로 연결을 끊습니다.
* 사용자가 연결을 끊으면 자동으로 가상 컴퓨터를 종료 합니다.
* 시작 되었지만 사용자가 연결 하지 않는 가상 컴퓨터를 자동으로 종료 합니다.

[자동 종료 설정을 사용 하 여 비용 제어 최대화](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) 섹션에서 자동 종료 기능에 대 한 자세한 내용을 검토 합니다.

랩 계정 관리자는 랩을 만드는 랩 계정에 대해 이 설정을 구성할 수 있습니다. 자세한 내용은 [랩 계정에 대 한 vm의 자동 종료 구성](how-to-configure-lab-accounts.md)을 참조 하세요. 랩 소유자는 랩을 만들 때 또는 랩을 만든 후 설정을 재정의할 수 있습니다. 

## <a name="configure-for-the-lab-level"></a>랩 수준 구성

[Azure Lab Services](https://labs.azure.com/)에서 자동 종료 설정을 구성할 수 있습니다.

* 랩 **정책** 에서 랩을 만들 때 또는
* 랩을 만든 후 ( **설정**)

> [!div class="mx-imgBorder"]
> ![랩 생성 시 구성](./media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

[자동 종료 설정을 사용 하 여 비용 제어 최대화](cost-management-guide.md#automatic-shutdown-settings-for-cost-control) 섹션에서 자동 종료에 대 한 세부 정보를 검토 해야 합니다.

> [!WARNING]
> Vm에 대 한 RDP 세션의 연결을 끊기 전에 VM에서 Linux 또는 Windows 운영 체제 (OS)를 종료 하는 경우 자동 종료 기능이 제대로 작동 하지 않습니다.  
## <a name="next-steps"></a>다음 단계

[랩에 대 한 대시보드](use-dashboard.md)
