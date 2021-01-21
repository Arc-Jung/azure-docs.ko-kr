---
title: Azure VM을 부팅할 때 블루 스크린 오류 | Microsoft Docs
description: 부팅할 때 블루 스크린이 오류가 수신되는 문제를 해결하는 방법 알아보기 | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: genli
ms.openlocfilehash: 9a95ddf882e5edba9daa8ff91c02d1df1f50bceb
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632979"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>Windows는 Azure VM을 부팅할 때 블루 스크린 오류를 표시
이 문서에서는 Microsoft Azure에서 Windows VM(Virtual Machine)을 부팅할 때 발생할 수 있는 블루 스크린 오류에 대해 설명합니다. 지원 티켓에 대한 데이터를 수집할 수 있도록 하는 단계를 제공합니다. 


## <a name="symptom"></a>증상 

Windows VM이 시작되지 않습니다. [부트 진단](./boot-diagnostics.md)에서 부트 스크린샷을 확인하면 블루 스크린에 다음 오류 메시지 중 하나가 표시됩니다.

- 이 PC에 문제가 생겨 다시 시작해야 합니다. 방금 몇 가지 오류 정보를 수집하고 있습니다. 이후에 다시 시작할 수 있습니다.
- PC에 문제가 생겨 다시 시작해야 합니다.

이 섹션은 VM을 관리할 때 발생할 수 있는 일반적인 오류 메시지를 나열합니다.

## <a name="cause"></a>원인

중지 오류가 발생하는 원인으로는 여러 가지 이유가 있을 수 있습니다. 가장 일반적인 원인은 다음과 같습니다.

- 드라이버 문제
- 손상된 시스템 파일 또는 메모리
- 애플리케이션에서 메모리의 금지된 섹터에 액세스

## <a name="collect-memory-dump-file"></a>메모리 덤프 파일 수집

> [!TIP]
> VM의 최근 백업이 있는 경우 [백업에서 vm을 복원](../../backup/backup-azure-arm-restore-vms.md) 하 여 부팅 문제를 해결할 수 있습니다.

이 문제를 해결하려면 먼저 크래시에 대한 덤프 파일을 수집하고 덤프 파일을 사용하여 지원에 문의해야 합니다. 덤프 파일을 수집하려면 다음 단계를 수행합니다.

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>복구 VM에 OS 디스크 연결

1. 영향을 받는 VM의 OS 디스크 스냅샷을 백업으로 만듭니다. 자세한 내용은 [디스크 스냅샷](../windows/snapshot-copy-managed-disk.md)을 참조하세요.
2. [OS 디스크를 복구 VM에 연결](./troubleshoot-recovery-disks-portal-windows.md)합니다. 
3. 복구 VM에 원격 데스크톱을 연결합니다.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>덤프 파일을 찾아서 지원 티켓을 제출

1. 복구 VM에서 연결된 OS 디스크의 Windows 폴더로 이동합니다. 연결된 OS 디스크에 할당된 드라이브 문자가 F인 경우 F:\Windows로 이동해야 합니다.
2. Memory.dmp 파일을 찾은 다음 덤프 파일을 사용 하 여 [지원 티켓을 제출](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 합니다. 

덤프 파일을 찾을 수 없는 경우 덤프 로그 및 직렬 콘솔을 사용하도록 설정하는 단계로 이동합니다.

### <a name="enable-dump-log-and-serial-console"></a>덤프 로그 및 직렬 콘솔을 사용하도록 설정

덤프 로그 및 직렬 콘솔을 사용하도록 설정하려면 다음 스크립트를 실행합니다.

1. 관리자 권한 명령 프롬프트 세션을 엽니다(관리자 권한으로 실행).
2. 다음 스크립트를 실행합니다.

    이 스크립트에서 연결된 OS 디스크에 할당된 드라이브 문자가 F라고 가정합니다. 이를 VM에서 적절한 값으로 바꿉니다.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

    1. RAM처럼 많은 메모리를 할당할 디스크에 충분한 공간이 있는지 확인합니다. 이 VM에 대해 선택하는 크기에 따라 달라집니다.
    2. 충분한 공간이 없거나 큰 크기의 VM인 경우(G, GS 또는 E 시리즈) 이 파일이 생성될 위치를 변경하고 VM에 연결된 다른 모든 데이터 디스크를 참조할 수 있습니다. 이를 위해 다음 키를 변경해야 합니다.

    ```config-reg
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

    reg unload HKLM\BROKENSYSTEM
    ```

3. [OS 디스크를 분리한 다음, OS 디스크를 영향을 받는 VM에 다시 연결합니다](./troubleshoot-recovery-disks-portal-windows.md).
4. 문제를 재현할 VM을 시작한 다음, 덤프 파일이 생성됩니다.
5. OS 디스크를 복구 VM에 연결하고, 덤프 파일을 수집한 다음, 덤프 파일을 사용하여 [지원 티켓을 제출](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)합니다.
