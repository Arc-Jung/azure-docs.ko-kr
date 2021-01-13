---
title: Azure Lab Services 시작
description: 이 문서에서는 Azure Lab Services를 시작 하는 방법을 설명 합니다.
ms.topic: article
ms.date: 11/18/2020
ms.openlocfilehash: d260ace7d7819c3ca2db96d6c4984bf834e170da
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165045"
---
# <a name="get-started-with-lab-services"></a>Lab Services 시작 

Azure Lab Services는 학생 및 교사에 게 자신의 컴퓨터에서 직접 virtual computer labs에 대 한 액세스 권한을 제공 합니다.

교사는 일대일 학생이 발급 된 하드웨어를 통해 해당 명령에서 랩 서비스를 활용 하도록 학생/부모를 학습 하는 방법을 알고 있어야 합니다. 따라서 학생이 Virtual Machines (VM)을 통해 연구 프로그램에 필요한 업계 표준 소프트웨어에 액세스할 수 있습니다. 

VM은 가상 컴퓨터 역할을 하는 가상 환경입니다. Vm에는 자체 프로세서, 메모리 및 저장소가 있습니다. Vm은 실제 컴퓨터에 대 한 대체를 제공 하 고 사용자가 자신의 장치에 없어도 운영 체제 및 소프트웨어에 대 한 액세스 권한을 제공할 수 있습니다. Azure Lab Services는 학생 들이 가상 컴퓨터 랩을 관리 하기 위해 Vm에 액세스 하 고 탐색 하는 데 사용할 수 있는 도구를 제공 합니다. 

이 문서에서는 Azure Lab Services를 활용 하기 위해 학생/부모를 액세스, 관리 및 학습 하는 방법에 대 한 직원 교육 정보를 제공 합니다.

## <a name="key-concepts"></a>주요 개념

### <a name="quota-hours"></a>할당량 시간

학생 들은 할당량 시간에 영향을 주지 않고 예약 된 클래스 시간 동안 언제 든 지 Vm에 액세스할 수 있습니다. 할당량 시간은 전체 반기로 설정 되며, 학생이 정기적으로 예약 된 클래스 시간을 벗어나 VM을 사용할 수 있는 시간 수를 결정 합니다.

주별 8 시간, 일요일에는 누적 안 함으로 다시 설정 됩니다.

자세한 내용은 [할당량 설정](how-to-configure-student-usage.md#set-quotas-for-users)을 참조 하세요.

### <a name="automatic-shut-down"></a>자동 종료

비용을 유지 하 고 학생 할당량 시간을 절약 하기 위해 랩에 대해 자동 종료를 사용 하도록 설정 합니다. 자동 종료는 비활성 기간 (마우스 또는 키보드 입력 없음) 후에 Vm을 해제 합니다. 자동 종료는 두 단계로 작동 하며, 먼저 비활성 기간 후에는 VM에서 학생의 연결을 끊습니다. 이 시점에서 VM은 계속 **실행** 되 고 있으며 학생 들에 게 연결할 수 있습니다. 연결을 끊은 후에는 다른 비활성 시간이 지나면 VM이 종료 됩니다.

자동 종료는 중요 한 비용 절감 도구 이지만, 작업을 저장 하 고 큰 프로젝트 파일을 렌더링 하는 것과 관련 하 여 학생 들에 게 챌린지를 제공 합니다. 학생 들이 자주 연결 되지 않거나 Vm이 너무 빨리 꺼져 있는 경우 CTE 관리자에 게 연락 하세요. 

자세한 내용은 [랩 계정에 대 한 vm의 자동 종료 구성](how-to-configure-lab-accounts.md)을 참조 하세요.

### <a name="managing-virtual-machines"></a>가상 컴퓨터 관리

랩을 관리 하면 교사가 랩 용량 (학생용으로 사용할 수 있는 Vm의 수)과 같은 작업을 제어 하 고 Vm을 수동으로 시작, 중지 또는 다시 설정할 수 있습니다. 또한 교사는 학생 인터페이스를 경험 하 고, 파일에 액세스 하 고, 소프트웨어 또는 VM 자체와 관련 된 문제를 해결 하기 위해 Vm에 연결할 수 있습니다.

Vm을 관리할 때 기억해 야 하는 가장 중요 한 사항은 컴퓨터가 **실행** 될 때마다 VM에 연결 되어 있지 않은 경우에도 비용이 발생 한다는 것입니다.

## <a name="lab-dashboards"></a>랩 대시보드

### <a name="overview"></a>개요

Azure Lab Services의 랩에 대 한 대시보드는 VM 정보, 할당 되 고 할당 되지 않은 Vm 수, 등록 된 사용자 수 및 등록 되지 않은 사용자 수, 랩 일정에 대 한 정보 등 특정 랩의 다양 한 측면에 대 한 스냅숏을 제공 합니다. 

> [!NOTE]
> 대시보드 및 [Azure Lab Services 웹 사이트](https://labs.azure.com/) 의 대부분 관리 측면은 교사에 게 표시 되지만, 사용자의 역할과 관련 된 사용 권한은 대시보드의 특정 기준을 수정 하는 기능에 영향을 줄 수 있습니다. 특정 랩 설정에 문제가 발생 하는 경우 CTE 관리자에 게 문의 하세요.

:::image type="content" source="./media/use-dashboard/dashboard.png" alt-text="Azure Lab Services 포털":::

### <a name="examine-a-dashboard"></a>대시보드 검사

1. [Azure Lab Services 웹 사이트로](https://labs.azure.com/)이동 하 여 로그인 합니다.
1. 랩을 선택 합니다.
1. 창의 왼쪽에 **대시보드** 가 표시 됩니다. **대시보드를** 클릭 하면 대시보드에 많은 타일이 표시 됩니다.
1. **비용 & 청구** 타일 아래에는 템플릿, 가상 컴퓨터 풀, 사용자 및 일정에 대 한 타일도 있으며,이를 통해 여러 측면을 수정 하 고 교실 랩에서 자세한 정보를 볼 수 있습니다.

    * 템플릿-템플릿이 생성 된 날짜와 마지막으로 게시 한 날짜를 설명 합니다. 
    * 가상 컴퓨터 풀-할당 되거나 할당 되지 않은 Vm의 수입니다.
    * 사용자-랩에 추가 되었지만 등록 되지 않은 등록 된 사용자 및 사용자 수입니다.
    * 일정-랩에 예정 된 예정 이벤트를 표시 하 고 더 많은 이벤트를 볼 수 있는 링크를 표시 합니다.

자세한 내용은 [Use dashboard](use-dashboard.md)를 참조 하세요.

### <a name="manually-starting-vms"></a>수동으로 Vm 시작

1. **가상 컴퓨터 풀** 페이지의 맨 위에 있는 **모두 시작** 단추를 클릭 하 여 랩에서 모든 vm을 시작할 수 있습니다.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/start-all-vms-button.png" alt-text="Vm 시작":::
1. 상태 전환을 클릭 하 여 개별 Vm을 시작할 수 있습니다. 

    설정/해제는 VM이 시작 되 면 **시작** 되 고 vm이 시작 되 면 **실행** 됩니다.
1. **이름** 열의 왼쪽에 있는 검사를 사용 하 여 여러 vm을 선택할 수도 있습니다. 

    원하는 Vm을 선택 했으면 화면 위쪽에서 **시작** 단추를 클릭 합니다.
1. 시작 되 면 **모두 중지** 단추를 클릭 하 여 모든 vm을 중지할 수 있습니다.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png" alt-text="Vm 중지":::

### <a name="stopping-and-resetting-vms"></a>Vm 중지 및 다시 설정

* 상태 전환을 클릭 하 여 개별 Vm을 중지할 수 있습니다.
* 확인을 사용 하 고 화면 맨 위에 있는 "중지" 단추를 클릭 하 여 여러 Vm을 중지할 수도 있습니다.

학생에 게 VM에 연결 하는 데 문제가 있거나 다른 이유로 VM을 다시 설정 해야 하는 경우 reset 함수를 사용할 수 있습니다.
1. 하나 이상의 Vm을 다시 설정 하려면 검사를 사용 하 여 Vm을 선택한 다음 페이지 맨 위에 있는 **다시 설정** 단추를 클릭 합니다.
1. 팝업 창에서 **다시 설정** 을 클릭 합니다.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png" alt-text="VM 다시 설정":::

    > [!NOTE]
    > 학생 VM을 켜면 학생의 할당량에 영향을 주지 않습니다. 사용자에 대 한 할당량은 예약 된 클래스 시간 외에 사용자가 사용할 수 있는 랩 시간 수를 지정 합니다.

### <a name="connect-to-vms"></a>Vm에 연결

교사는 설정 되어 있는 동안 학생 VM에 연결할 수 있으며, 학생이 VM에 연결 되지 않습니다. VM에 연결 하면 VM의 로컬 파일에 액세스할 수 있으며 학생 들이 문제를 해결 하는 데 도움이 됩니다.

1. 학생 VM에 연결 하려면 목록에서 VM을 마우스로 가리키고 **연결** 단추를 클릭 합니다. 
1. 그런 다음 Chromebooks, Mac 또는 Pc에 대 한 학생용 시작 가이드를 따르세요.

:::image type="content" source="./media/how-to-set-virtual-machine-passwords/connect-student-vm.png" alt-text="학생 VM에 연결 단추":::

## <a name="manage-users-in-a-lab"></a>랩에서 사용자 관리

교사는 학생 사용자를 랩에 추가 하 고 해당 시간 할당량을 모니터링할 수 있습니다. 전자 메일 주소로 사용자를 추가 하거나 스프레드시트 목록을 사용 하 여 사용자를 추가 하거나 사용자를 등록 하는 방법에 대 한 자세한 내용은 [랩 사용자 추가 및 관리](how-to-configure-student-usage.md)를 참조 하세요.

사용자를 초대 하거나 링크를 공유한 후에는 **상태** 열의 **사용자** 페이지에서 성공적으로 등록 된 사용자를 모니터링할 수 있습니다. 

## <a name="clean-up-resources"></a>리소스 정리

이 빠른 시작에서 만든 리소스를 계속 사용 하지 않으려면 리소스를 삭제 합니다.

## <a name="next-steps"></a>다음 단계

[랩 계정 설정](tutorial-setup-lab-account.md)
