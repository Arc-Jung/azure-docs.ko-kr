---
title: 로컬 세션 관리자 서비스를 기다리는 동안 VM이 응답 하지 않습니다.
description: 이 문서에서는 Azure VM을 부팅 하는 동안 로컬 세션 관리자가 처리를 마칠 때까지 대기 하는 게스트 OS의 문제를 해결 하는 단계를 제공 합니다.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: fc3bd5d2590e969db07e9dffa61b4902ea4604c3
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632549"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager-service"></a>로컬 세션 관리자 서비스를 기다리는 동안 VM이 응답 하지 않습니다.

이 문서에서는 게스트 운영 체제 (게스트 OS)가 Azure VM (가상 머신)을 부팅 하는 동안 로컬 세션 관리자의 처리가 완료 될 때까지 대기 하는 문제를 해결 하는 단계를 제공 합니다.

## <a name="symptoms"></a>증상

[부팅 진단을](./boot-diagnostics.md) 사용 하 여 VM의 출력에 대 한 스크린샷을 볼 때 스크린 샷에서 "로컬 세션 관리자를 기다리는 중입니다." 라는 메시지가 표시 되는 것을 볼 수 있습니다.

![Windows Server 2012 r 2에서 중단 된 게스트 OS를 보여 주는 스크린샷 "로컬 세션 관리자를 기다리는 중" 메시지가 표시 됩니다.](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>원인

VM이 로컬 세션 관리자를 대기 하는 데는 여러 가지 이유가 있을 수 있습니다. 이 문제가 지속 되 면 분석을 위해 메모리 덤프를 수집 해야 합니다.

## <a name="solution"></a>솔루션

> [!TIP]
> VM의 최근 백업이 있는 경우 [백업에서 vm을 복원](../../backup/backup-azure-arm-restore-vms.md) 하 여 부팅 문제를 해결할 수 있습니다.

경우에 따라 프로세스가 완료 될 때까지 대기 하면 문제가 해결 됩니다. VM이 응답 하지 않고 1 시간 넘게 대기 화면에 남아 있는 경우 메모리 덤프를 수집 하 고 Microsoft 지원에 문의 해야 합니다.

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>새 복구 VM에 OS 디스크를 연결 합니다.

1. 복구 VM을 준비 하려면 [vm 복구 명령의](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md)1-3 단계를 수행 합니다.
1. 원격 데스크톱 연결를 사용 하 여 복구 VM에 연결 합니다.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>덤프 파일을 찾고 지원 티켓을 제출 합니다.

1. 복구 VM에서 연결 된 OS 디스크의 Windows 폴더로 이동 합니다. 예를 들어 연결 된 OS 디스크에 할당 된 드라이브 문자에 *F* 라는 레이블이 지정 된 경우로 이동 `F:\Windows` 합니다.
1. *Memory.dmp* 파일을 찾은 다음 연결 된 메모리 덤프 파일을 사용 하 여 [지원 티켓을 제출](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 합니다.
1. *Memory.dmp* 파일을 찾는 데 문제가 있는 경우이 가이드의 지침에 따라 [NMI (비 마스크 인터럽트) 호출을 사용 하 여 크래시 덤프 파일을 생성](/windows/client-management/generate-kernel-or-complete-crash-dump)합니다.

NMI 호출에 대 한 자세한 내용은 [Azure 직렬 콘솔에서 nmi 호출](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) 사용자 가이드를 참조 하세요.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure 가상 머신 부팅 오류 문제 해결](boot-error-troubleshoot.md)