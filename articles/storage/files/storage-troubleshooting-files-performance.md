---
title: Azure 파일 공유 성능 문제 해결 가이드
description: Azure 파일 공유의 알려진 성능 문제를 해결 합니다. 이러한 문제가 발생할 경우 잠재적 원인과 관련 해결 방법을 검색 합니다.
author: gunjanj
ms.service: storage
ms.topic: troubleshooting
ms.date: 11/16/2020
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 54b92c24b5a50ef1674dcb47df555b27259a350b
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100393856"
---
# <a name="troubleshoot-azure-file-shares-performance-issues"></a>Azure 파일 공유 성능 문제 해결

이 문서에서는 Azure 파일 공유와 관련 된 몇 가지 일반적인 문제를 나열 합니다. 이러한 문제가 발생 하는 경우에 대 한 잠재적 원인과 해결 방법을 제공 합니다.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>대기 시간이 짧고 처리량이 높고 일반 성능 문제가 발생 합니다.

### <a name="cause-1-share-was-throttled"></a>원인 1: 공유가 제한 됨

파일 공유에 대 한 IOPS (초당 I/o 작업 수), 수신 또는 송신 한도에 도달 하면 요청이 제한 됩니다. 표준 및 프리미엄 파일 공유에 대 한 한도를 이해 하려면 [파일 공유 및 파일 배율 목표](./storage-files-scale-targets.md#azure-file-share-scale-targets)를 참조 하세요.

공유를 제한 하 고 있는지 확인 하려면 포털에서 Azure 메트릭에 액세스 하 고 사용할 수 있습니다.

1. Azure Portal에서 스토리지 계정으로 이동합니다.

1. 왼쪽 창의 **모니터링** 아래에서 **메트릭** 을 선택 합니다.

1. 저장소 계정 범위에 대 한 메트릭 네임 스페이스로 **파일** 을 선택 합니다.

1. **트랜잭션을** 메트릭으로 선택 합니다.

1. **응답 형식** 에 대 한 필터를 추가한 다음 요청이 제한 되었는지 여부를 확인 합니다. 

    표준 파일 공유의 경우 요청이 제한 되는 경우 다음 응답 유형이 기록 됩니다.

    - SuccessWithThrottling
    - ClientThrottlingError

    프리미엄 파일 공유의 경우 요청이 제한 되는 경우 다음 응답 유형이 기록 됩니다.

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

    각 응답 유형에 대해 자세히 알아보려면 [메트릭 차원](https://docs.microsoft.com/azure/storage/files/storage-files-monitoring-reference#metrics-dimensions)을 참조 하세요.

    !["응답 유형" 속성 필터를 표시 하는 프리미엄 파일 공유에 대 한 메트릭 옵션의 스크린샷](media/storage-troubleshooting-premium-fileshares/metrics.png)

    > [!NOTE]
    > 경고를 수신 하려면이 문서의 뒷부분에 나오는 ["파일 공유를 제한 하는 경우 경고를 만드는 방법"](#how-to-create-an-alert-if-a-file-share-is-throttled) 섹션을 참조 하세요.

### <a name="solution"></a>해결 방법

- 표준 파일 공유를 사용 하는 경우 저장소 계정에서 [대량 파일 공유](./storage-files-how-to-create-large-file-share.md?tabs=azure-portal) 를 사용 하도록 설정 합니다. 대량 파일 공유는 공유 당 최대 1만 IOPS를 지원 합니다.
- 프리미엄 파일 공유를 사용 하는 경우 프로 비전 된 파일 공유 크기를 늘려 IOPS 제한을 늘립니다. 자세히 알아보려면 [프리미엄 파일 공유에 대 한 프로 비전 이해](./understanding-billing.md#provisioned-model)를 참조 하세요.

### <a name="cause-2-metadata-or-namespace-heavy-workload"></a>원인 2: 메타 데이터 또는 네임 스페이스 작업량이 많은 작업

대부분의 요청이 메타 데이터 중심 (예: createfile, system.windows.forms.openfiledialog.openfile, closefile, queryinfo 또는 queryinfo) 인 경우 대기 시간이 읽기/쓰기 작업 보다 더 심각 하지 않습니다.

대부분의 요청이 메타 데이터 중심 인지 확인 하려면 앞에서 설명한 것 처럼 1-4 단계를 수행 하 여 시작 합니다. 5 단계의 경우 **응답 형식** 에 대 한 필터를 추가 하는 대신 **API 이름** 에 대 한 속성 필터를 추가 합니다.

!["API 이름" 속성 필터를 표시 하는 프리미엄 파일 공유에 대 한 메트릭 옵션의 스크린샷](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>해결 방법

- 메타 데이터 작업의 수를 줄이기 위해 응용 프로그램을 수정할 수 있는지 여부를 확인 합니다.
- 파일 공유에 VHD (가상 하드 디스크)를 추가 하 고 클라이언트에서 SMB를 통해 VHD를 탑재 하 여 데이터에 대 한 파일 작업을 수행 합니다. 이 방법은 단일 작성기 및 다중 판독기 시나리오에서 작동 하며 메타 데이터 작업을 로컬으로 허용 합니다. 설치 프로그램은 로컬에서 직접 연결 된 저장소와 비슷한 성능을 제공 합니다.

### <a name="cause-3-single-threaded-application"></a>원인 3: 단일 스레드 응용 프로그램

사용 중인 응용 프로그램이 단일 스레드 인 경우 프로 비전 된 공유 크기에 따라이 설정으로 인해 가능한 최대 처리량 보다 IOPS 처리량이 상당히 줄어들 수 있습니다.

### <a name="solution"></a>해결 방법

- 스레드 수를 늘려 응용 프로그램 병렬 처리를 늘립니다.
- 병렬 처리를 사용할 수 있는 응용 프로그램으로 전환 합니다. 예를 들어 복사 작업의 경우 Windows 클라이언트 또는 Linux 클라이언트의 **병렬** 명령에서 AzCopy 또는 RoboCopy를 사용할 수 있습니다.

## <a name="very-high-latency-for-requests"></a>요청에 대 한 대기 시간 매우 높음

### <a name="cause"></a>원인

클라이언트 VM (가상 컴퓨터)은 파일 공유와 다른 지역에 있을 수 있습니다. 대기 시간이 긴 다른 이유는 클라이언트나 네트워크에서 발생 하는 대기 시간으로 인해 발생할 수 있습니다.

### <a name="solution"></a>해결 방법

- 파일 공유와 동일한 지역에 있는 VM에서 응용 프로그램을 실행 합니다.
- 저장소 계정의 경우 Azure Portal에서 **Azure Monitor** 를 통해 트랜잭션 메트릭 **SuccessE2ELatency** 및 **SuccessServerLatency** 를 검토 합니다. SuccessE2ELatency 및 SuccessServerLatency 메트릭 값 간의 큰 차이는 네트워크 또는 클라이언트에 의해 발생 하는 대기 시간을 나타냅니다. Azure Files 모니터링 데이터 참조의 [트랜잭션 메트릭](storage-files-monitoring-reference.md#transaction-metrics) 을 참조 하세요.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>클라이언트에서 네트워크에 의해 지원 되는 최대 처리량을 달성할 수 없습니다.

### <a name="cause"></a>원인
한 가지 가능한 원인은 표준 파일 공유에 대 한 SMB 다중 채널 지원이 부족 한 것입니다. 현재 Azure Files는 단일 채널만 지원 하므로 클라이언트 VM에서 서버로의 연결은 하나 뿐입니다. 이 단일 연결은 클라이언트 VM의 단일 코어로 해석 VM에서 달성 가능한 최대 처리량은 단일 코어에 의해 바인딩됩니다.

### <a name="workaround"></a>해결 방법

- 프리미엄 파일 공유의 경우 [FileStorage 계정에서 SMB 다중 채널을 사용 하도록 설정](storage-files-enable-smb-multichannel.md)합니다.
- 코어 크기가 더 큰 VM을 가져오면 처리량을 향상 시킬 수 있습니다.
- 여러 Vm에서 클라이언트 응용 프로그램을 실행 하면 처리량이 증가 합니다.
- 가능한 경우 REST Api를 사용 합니다.

## <a name="throughput-on-linux-clients-is-significantly-lower-than-that-of-windows-clients"></a>Linux 클라이언트의 처리량이 Windows 클라이언트의 처리량 보다 훨씬 낮습니다.

### <a name="cause"></a>원인

이는 Linux에서 SMB 클라이언트를 구현 하는 것과 관련 하 여 알려진 문제입니다.

### <a name="workaround"></a>해결 방법

- 부하를 여러 Vm에 분산 합니다.
- 동일한 VM에서 **nosharesock** 옵션을 사용 하 여 여러 탑재 위치를 사용 하 고 이러한 탑재 지점의 부하를 분산 합니다.
- Linux에서 **nostrictsync** 옵션으로 탑재를 시도 하 여 모든 **fsync** 호출에서 SMB 플러시를 방지 합니다. Azure Files에서이 옵션은 데이터 일관성을 방해 하지 않지만 디렉터리 목록 (**ls-l** 명령)에서 오래 된 파일 메타 데이터를 초래할 수 있습니다. **Stat** 명령을 사용 하 여 파일 메타 데이터를 직접 쿼리하면 최신 파일 메타 데이터가 반환 됩니다.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>메타 데이터에 대 한 긴 대기 시간-광범위 한 열기/닫기 작업과 관련 된 많은 워크 로드

### <a name="cause"></a>원인

디렉터리 임대에 대 한 지원이 부족 합니다.

### <a name="workaround"></a>해결 방법

- 가능 하면 짧은 시간 내에 동일한 디렉터리에서 과도 한 여는 핸들을 사용 하지 마십시오.
- Linux Vm의 경우 **actimeo = \<sec>** 를 탑재 옵션으로 지정 하 여 디렉터리 항목 캐시 제한 시간을 늘립니다. 기본적으로 시간 제한은 1 초 이므로 3 또는 5 초와 같은 큰 값이 도움이 될 수 있습니다.
- RHEL (CentOS Linux 또는 Red Hat Enterprise Linux) Vm의 경우 시스템을 CentOS Linux 8.2 또는 RHEL 8.2로 업그레이드 합니다. 다른 Linux Vm의 경우 커널을 5.0 이상으로 업그레이드 합니다.

## <a name="low-iops-on-centos-linux-or-rhel"></a>CentOS Linux 또는 RHEL의 낮은 IOPS

### <a name="cause"></a>원인

CentOS Linux 또는 RHEL에서는 1 보다 큰 i/o 깊이가 지원 되지 않습니다.

### <a name="workaround"></a>해결 방법

- CentOS Linux 8 또는 RHEL 8로 업그레이드 합니다.
- Ubuntu로 변경 합니다.

## <a name="slow-file-copying-to-and-from-azure-file-shares-in-linux"></a>Linux에서 Azure 파일 공유에 대 한 저속 파일 복사

파일 복사 속도가 느려지는 경우 [linux 문제 해결 가이드](storage-troubleshoot-linux-file-connection-problems.md#slow-file-copying-to-and-from-azure-files-in-linux)의 "Linux에서 Azure 파일 공유에 대 한 저속 파일 복사" 섹션을 참조 하세요.

## <a name="jittery-or-sawtooth-pattern-for-iops"></a>IOPS에 대 한 떨림 또는 패턴이 톱니 모양 패턴

### <a name="cause"></a>원인

클라이언트 응용 프로그램이 지속적으로 기준선 IOPS를 초과 합니다. 현재는 요청 로드의 서비스 측 다듬기가 없습니다. 클라이언트는 기준선 IOPS를 초과 하는 경우 서비스에 의해 제한 됩니다. 이 제한을 통해 클라이언트에서 떨림 또는 패턴이 톱니 모양 IOPS 패턴이 발생할 수 있습니다. 이 경우 클라이언트에서 달성 하는 평균 IOPS는 기준선 IOPS 보다 낮을 수 있습니다.

### <a name="workaround"></a>해결 방법
- 공유가 제한 되지 않도록 클라이언트 응용 프로그램에서 요청 부하를 줄입니다.
- 공유가 제한 되지 않도록 공유의 할당량을 늘립니다.

## <a name="excessive-directoryopendirectoryclose-calls"></a>과도 한 DirectoryOpen/Directoryopen 호출

### <a name="cause"></a>원인

**Directoryopen/Directoryopen** 호출 수가 최상위 API 호출 중에 있고 클라이언트에서 많은 호출을 수행 하지 않을 경우 AZURE 클라이언트 VM에 설치 된 바이러스 백신 소프트웨어 때문에 문제가 발생할 수 있습니다.

### <a name="workaround"></a>해결 방법

- 이 문제에 대 한 해결 방법은 [Windows 용 4 월 플랫폼 업데이트](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform)에서 확인할 수 있습니다.

## <a name="file-creation-is-slower-than-expected"></a>파일 만들기가 예상 보다 느립니다.

### <a name="cause"></a>원인

많은 수의 파일을 만드는 데 사용 하는 작업에는 프리미엄 파일 공유와 표준 파일 공유 간의 성능 차이가 크게 표시 되지 않습니다.

### <a name="workaround"></a>해결 방법

- 없음

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Windows 8.1 또는 Server 2012 r 2의 성능 저하

### <a name="cause"></a>원인

I/o를 많이 사용 하는 워크 로드의 Azure 파일 공유에 액세스 하는 데 예상 되는 대기 시간 보다 높습니다.

### <a name="workaround"></a>해결 방법

- 사용 가능한 [핫픽스](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1)를 설치 합니다.

## <a name="smb-multichannel-option-not-visible-under-file-share-settings"></a>파일 공유 설정에서 SMB 다중 채널 옵션이 표시 되지 않습니다. 

### <a name="cause"></a>원인

구독이 기능에 대해 등록 되지 않았거나 지역 및 계정 유형이 지원 되지 않습니다.

### <a name="solution"></a>해결 방법

구독이 SMB 다중 채널 기능에 등록 되었는지 확인 합니다. [시작](storage-files-enable-smb-multichannel.md#getting-started) 을 참조 하 여 계정 종류가 계정 개요 페이지에서 FileStorage (프리미엄 파일 계정) 인지 확인 합니다. 

## <a name="smb-multichannel-is-not-being-triggered"></a>SMB 다중 채널이 트리거되지 않습니다.

### <a name="cause"></a>원인

다시 탑재 없이 SMB 다중 채널 구성 설정에 대 한 최근 변경 내용입니다.

### <a name="solution"></a>해결 방법
 
-   Windows SMB 클라이언트 또는 계정 SMB 다중 채널 구성 설정이 변경 된 후에는 공유를 분리 하 고, 60 초 동안 기다린 다음, 다중 채널을 트리거하기 위해 공유를 다시 탑재 해야 합니다.
-   Windows 클라이언트 OS의 경우 큐 깊이가 높은 IO 로드를 생성 합니다. 예를 들어 파일을 복사 하 여 SMB 다중 채널을 트리거합니다.  서버 OS의 경우 SMB 다중 채널은 QD = 1로 트리거되고,이는 공유에 대 한 IO를 시작 하는 즉시 발생 합니다.

## <a name="high-latency-on-web-sites-hosted-on-file-shares"></a>파일 공유에서 호스트 되는 웹 사이트의 긴 대기 시간 

### <a name="cause"></a>원인  

파일 공유에 대 한 파일 변경 알림이 높으면 상당한 대기 시간이 발생할 수 있습니다. 이는 일반적으로 중첩 된 디렉터리 구조가 깊은 파일 공유에 호스트 되는 웹 사이트에서 발생 합니다. 일반적인 시나리오는 IIS에서 호스트 되는 웹 응용 프로그램으로, 기본 구성의 각 디렉터리에 대해 파일 변경 알림이 설정 됩니다. SMB 클라이언트에서 등록 된 공유에 대 한 각 변경 내용 ([Readdirectorychanges w](/windows/win32/api/winbase/nf-winbase-readdirectorychangesw))은 파일 서비스에서 클라이언트로의 변경 알림을 푸시하 며,이는 시스템 리소스를 사용 하 고 변경 내용 수를 사용 하 여 증가를 발급 합니다. 이로 인해 공유 제한이 발생할 수 있으므로 클라이언트 쪽 대기 시간이 더 높아질 수 있습니다. 

확인 하려면 포털에서 Azure 메트릭을 사용할 수 있습니다. 

1. Azure Portal에서 스토리지 계정으로 이동합니다. 
1. 왼쪽 메뉴의 모니터링 아래에서 메트릭을 선택 합니다. 
1. 저장소 계정 범위에 대 한 메트릭 네임 스페이스로 파일을 선택 합니다. 
1. 트랜잭션을 메트릭으로 선택 합니다. 
1. ResponseType에 대 한 필터를 추가 하 고 요청에 SuccessWithThrottling (SMB 용) 또는 ClientThrottlingError (REST)의 응답 코드가 있는지 확인 합니다.

### <a name="solution"></a>해결 방법 

- 파일 변경 알림이 사용 되지 않으면 파일 변경 알림 (기본 설정)을 사용 하지 않도록 설정 합니다.
    - FCNMode를 업데이트 하 여 [파일 변경 알림을 사용 하지 않도록 설정](https://support.microsoft.com/help/911272/fix-asp-net-2-0-connected-applications-on-a-web-site-may-appear-to-sto) 합니다. 
    - `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds `레지스트리에서를 설정 하 고 w3wp 프로세스를 다시 시작 하 여 IIS 작업자 프로세스 (w3wp.exe) 폴링 간격을 0으로 업데이트 합니다. 이 설정에 대 한 자세한 내용은 [IIS의 여러 부분에서 사용 되는 일반적인 레지스트리 키](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp)를 참조 하세요.
- 볼륨을 줄이기 위해 파일 변경 알림 폴링 간격의 빈도를 늘립니다.
    - 사용자 요구 사항에 따라 w3wp.exe 작업자 프로세스 폴링 간격을 더 높은 값 (예: 10 분 또는 30 분)으로 업데이트 합니다. `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ConfigPollMilliSeconds ` [레지스트리에서](/troubleshoot/iis/use-registry-keys#registry-keys-that-apply-to-iis-worker-process-w3wp) 를 설정 하 고 W3WP 프로세스를 다시 시작 합니다.
- 웹 사이트의 매핑된 물리적 디렉터리에 중첩 된 디렉터리 구조가 있는 경우 파일 변경 알림 범위를 제한 하 여 알림 볼륨을 줄일 수 있습니다. 기본적으로 IIS는 가상 디렉터리가 매핑되는 물리적 디렉터리 및 해당 물리적 디렉터리의 모든 자식 디렉터리에 있는 Web.config 파일의 구성을 사용 합니다. 하위 디렉터리에 Web.config 파일을 사용 하지 않으려는 경우 가상 디렉터리의 allowSubDirConfig 특성에 대해 false를 지정 합니다. 자세한 내용은 [여기](/iis/get-started/planning-your-iis-architecture/understanding-sites-applications-and-virtual-directories-on-iis#virtual-directories)에서 찾을 수 있습니다. 
    - Web.Config에서 IIS 가상 디렉터리 "allowSubDirConfig" 설정을 *false* 로 설정 하 여 범위에서 매핑된 물리적 자식 디렉터리를 제외 합니다.  

## <a name="how-to-create-an-alert-if-a-file-share-is-throttled"></a>파일 공유를 제한 하는 경우 경고를 만드는 방법

1. **Azure Portal** 의 **저장소 계정** 으로 이동 합니다.
2. **모니터링** 섹션에서 **경고** 를 클릭 한 다음 **+ 새 경고 규칙** 을 클릭 합니다.
3. **리소스 편집** 을 클릭 하 고 저장소 계정에 대 한 **파일 리소스 유형을** 선택한 다음 **완료** 를 클릭 합니다. 예를 들어 저장소 계정 이름이 인 경우 `contoso` 리소스를 선택 `contoso/file` 합니다.
4. 조건 **추가** 를 클릭 하 여 조건을 추가 합니다.
5. 저장소 계정에 대해 지원 되는 신호 목록이 표시 되 면 **트랜잭션** 메트릭을 선택 합니다.
6. **신호 논리 구성** 블레이드에서 **차원 이름** 드롭다운을 클릭 하 고 **응답 유형** 을 선택 합니다.
7. **차원 값** 드롭다운을 클릭 하 고 파일 공유에 대 한 적절 한 응답 유형을 선택 합니다.

    표준 파일 공유의 경우 다음 응답 유형을 선택 합니다.

    - SuccessWithThrottling
    - ClientThrottlingError

    프리미엄 파일 공유의 경우 다음 응답 유형을 선택 합니다.

    - SuccessWithShareEgressThrottling
    - SuccessWithShareIngressThrottling
    - SuccessWithShareIopsThrottling
    - ClientShareEgressThrottlingError
    - ClientShareIngressThrottlingError
    - ClientShareIopsThrottlingError

   > [!NOTE]
   > 응답 유형이 **차원 값** 드롭다운에 나열 되지 않는 경우이는 리소스가 제한 되지 않았음을 의미 합니다. 차원 값을 추가 하려면 **차원 값** 드롭다운 목록 옆에 있는 **사용자 지정 값 추가** 를 선택 하 고 응답 유형 (예: **SuccessWithThrottling**)을 입력 한 다음 **확인** 을 선택 하 고이 단계를 반복 하 여 해당 파일 공유에 적용 가능한 모든 응답 유형을 추가 합니다.

8. **차원 이름** 드롭다운을 클릭 하 고 **파일 공유** 를 선택 합니다.
9. **차원 값** 드롭다운을 클릭 하 고 경고를 표시 하려는 파일 공유를 선택 합니다.


   > [!NOTE]
   > 파일 공유가 표준 파일 공유 인 경우 **모든 현재 및 미래 값** 을 선택 합니다. 표준 파일 공유에는 공유 별 메트릭을 사용할 수 없으므로 차원 값 드롭다운에는 파일 공유가 나열 되지 않습니다. 저장소 계정 내에서 파일 공유를 제한 하 고 경고에서 제한 된 파일 공유를 식별 하지 않으면 표준 파일 공유에 대 한 제한 경고가 트리거됩니다. 표준 파일 공유에는 공유 별 메트릭을 사용할 수 없으므로 저장소 계정 마다 하나의 파일 공유를 사용 하는 것이 좋습니다.

10. **경고 매개 변수** (임계값, 연산자, 집계 세분성 및 평가 빈도)를 정의 하 고 **완료** 를 클릭 합니다.

    > [!TIP]
    > 정적 임계값을 사용 하는 경우 메트릭 차트는 파일 공유가 현재 제한 되는 경우 적절 한 임계값을 결정 하는 데 도움이 될 수 있습니다. 동적 임계값을 사용 하는 경우 메트릭 차트에는 최근 데이터를 기반으로 계산 된 임계값이 표시 됩니다.

11. 기존 작업 그룹을 선택 하거나 새 작업 그룹을 만들어 경고에 **작업 그룹** (메일, SMS 등)을 추가 하려면 **작업 그룹 추가** 를 클릭 합니다.
12. 경고 **규칙 이름**, **설명** 및 **심각도** 와 같은 **경고 정보** 를 입력 합니다.
13. 경고 **규칙 만들기** 를 클릭 하 여 경고를 만듭니다.

Azure Monitor에서 경고를 구성 하는 방법에 대 한 자세한 내용은 [Microsoft Azure의 경고 개요]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)를 참조 하세요.

## <a name="how-to-create-alerts-if-a-premium-file-share-is-trending-toward-being-throttled"></a>프리미엄 파일 공유의 제한에 대 한 추세를 파악 하는 경우 경고를 만드는 방법

1. Azure Portal에서 스토리지 계정으로 이동합니다.
2. **모니터링** 섹션에서 **경고** 를 선택한 다음 **새 경고 규칙** 을 선택 합니다.
3. **리소스 편집** 을 선택 하 고 저장소 계정에 대 한 **파일 리소스 종류** 를 선택한 다음 **완료** 를 선택 합니다. 예를 들어 저장소 계정 이름이 *contoso* 인 경우 contoso/file 리소스를 선택 합니다.
4. 조건 **선택** 을 선택 하 여 조건을 추가 합니다.
5. 저장소 계정에 대해 지원 되는 신호 목록에서 **송신** 메트릭을 선택 합니다.

   > [!NOTE]
   > 수신, 송신 또는 트랜잭션 값이 설정 된 임계값을 초과 하는 경우 경고를 표시 하려면 세 개의 개별 경고를 만들어야 합니다. 모든 조건이 충족 되는 경우에만 경고가 트리거됩니다. 예를 들어 모든 조건을 하나의 경고에 배치 하는 경우 수신, 송신 및 트랜잭션이 임계값을 초과 하는 경우에만 경고가 표시 됩니다.

6. 아래로 스크롤합니다. **차원 이름** 드롭다운 목록에서 **파일 공유** 를 선택 합니다.
7. **차원 값** 드롭다운 목록에서 경고를 발생 시킬 파일 공유를 선택 합니다.
8. **연산자**, **임계값 값**, **집계 세분성** 및 **평가 빈도** 드롭다운 목록에서 값을 선택 하 여 경고 매개 변수를 정의한 다음 **완료** 를 선택 합니다.

   수신, 수신 및 트랜잭션 메트릭은 초당 전송, 수신 및 i/o를 프로 비전 하는 경우 분 단위로 표현 됩니다. 예를 들어 프로 비전 된 송신이 초당 90 &nbsp; mebibytes 인 경우 (MiB/s), &nbsp; 프로 비전 된 송신의 80%를 임계값으로 하려면 다음 경고 매개 변수를 선택 합니다. 
   - **임계값**: *75497472* 
   - **연산자**: *크거나 같음*
   - **집계 유형**: *평균*
   
   경고를 표시 하려는 경우에 따라 **집계 세분성** 및 **평가 빈도** 값을 선택할 수도 있습니다. 예를 들어 경고를 1 시간 동안 평균 수신 시간을 확인 하 고 1 시간 마다 경고 규칙을 실행 하려면 다음을 선택 합니다.
   - **집계 세분성**: *1 시간*
   - **평가 빈도**: *1 시간*

9. **작업 그룹 추가** 를 선택한 다음 기존 작업 그룹을 선택 하거나 새 작업 그룹을 만들어 경고에 대 한 작업 그룹 (예: 전자 메일 또는 SMS)을 추가 합니다.
10. 경고 **규칙 이름**, **설명** 및 **심각도** 와 같은 경고 세부 정보를 입력 합니다.
11. **경고 규칙 만들기** 를 선택하여 경고를 만듭니다.

    > [!NOTE]
    > - *프로 비전 된 수신으로 인해* 프리미엄 파일 공유의 제한에 대 한 알림이 표시 되려면 앞의 지침을 따르고 다음과 같이 변경 합니다.
    >    - 5 단계에서 **송신** 대신 **수신** 메트릭을 선택 합니다.
    >
    > - *프로 비전 된 IOPS로 인해* 프리미엄 파일 공유의 제한에 대 한 알림이 표시 되려면 앞의 지침을 따르고 다음과 같이 변경 합니다.
    >    - 5 단계에서 **송신** 이 아닌 **트랜잭션** 메트릭을 선택 합니다.
    >    - 10 단계에서 **집계 유형에** 대 한 유일한 옵션은 *합계* 입니다. 따라서 임계값은 선택한 집계 세분성에 따라 달라 집니다. 예를 들어 &nbsp; 프로 비전 된 기준선 iops의 80%로 임계값을 선택 하 고 **집계 세분성** 으로 *1 시간* 을 선택 하는 경우 **임계값** 은 기준선 iops (바이트) &times; &nbsp; 0.8 3600가 됩니다 &times; &nbsp; . 

Azure Monitor에서 경고를 구성 하는 방법에 대 한 자세한 내용은 [Microsoft Azure의 경고 개요]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)를 참조 하세요.

## <a name="see-also"></a>참고 항목
- [Windows에서 Azure Files 문제 해결](storage-troubleshoot-windows-file-connection-problems.md)  
- [Linux에서 Azure Files 문제 해결](storage-troubleshoot-linux-file-connection-problems.md)  
- [Azure Files FAQ](storage-files-faq.md)
