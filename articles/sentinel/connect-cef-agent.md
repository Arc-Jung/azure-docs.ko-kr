---
title: Azure 센티널에 CEF 데이터를 연결 하는 로그 전달자 배포 | Microsoft Docs
description: Azure 센티널에 CEF 데이터를 연결 하는 에이전트를 배포 하는 방법을 알아봅니다.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2021
ms.author: yelevin
ms.openlocfilehash: a4303f43dffa98f842bd3daf9e3a0cd5214932b1
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585374"
---
# <a name="step-1-deploy-the-log-forwarder"></a>1 단계: 로그 전달자 배포


이 단계에서는 보안 솔루션에서 Azure 센티널 작업 영역으로 로그를 전달 하는 Linux 컴퓨터를 지정 하 고 구성 합니다. 이 컴퓨터는 온-프레미스 환경, Azure VM 또는 다른 클라우드의 VM에 있는 물리적 또는 가상 컴퓨터 일 수 있습니다. 제공 된 링크를 사용 하 여 다음 작업을 수행 하는 지정 된 컴퓨터에서 스크립트를 실행 합니다.

- Linux 용 Log Analytics 에이전트 (OMS 에이전트 라고도 함)를 설치 하 고 다음과 같은 목적으로 구성 합니다.
    - TCP 포트 25226의 기본 제공 Linux Syslog 디먼에서 CEF 메시지 수신 대기
    - Azure 센티널 작업 영역에 TLS를 통해 안전 하 게 메시지 보내기 (구문 분석 및 보강)

- 는 다음과 같은 목적으로 기본 제공 Linux Syslog 디먼 (rsyslog. d/syslog 기능)을 구성 합니다.
    - TCP 포트 514의 보안 솔루션에서 Syslog 메시지 수신 대기
    - TCP 포트 25226를 사용 하 여 로컬 호스트의 Log Analytics 에이전트에 대 한 CEF로 식별 되는 메시지만 전달
 
## <a name="prerequisites"></a>사전 요구 사항

- 지정 된 Linux 컴퓨터에 상승 된 권한 (sudo)이 있어야 합니다.

- Linux 컴퓨터에 **python 2.7** 또는 **3** 이 설치 되어 있어야 합니다.<br>명령을 사용 `python -version` 하 여 확인 합니다.

- Log Analytics 에이전트를 설치 하기 전에 Linux 컴퓨터가 Azure 작업 영역에 연결 되어 있지 않아야 합니다.

- Linux 컴퓨터에는 최소 **4 개의 CPU 코어와 8GB RAM** 이 있어야 합니다.

    > [!NOTE]
    > - **Rsyslog** 디먼을 사용 하는 단일 로그 전달자 컴퓨터에는 **최대 8500 개의 EPS (초당 이벤트)** 의 지원 되는 용량이 있습니다.

- 이 프로세스의 어느 시점에서 작업 영역 ID 및 작업 영역 기본 키가 필요할 수 있습니다. 작업 영역 리소스의 **에이전트 관리** 에서 찾을 수 있습니다.

## <a name="run-the-deployment-script"></a>배포 스크립트 실행
 
1. Azure 센티널 탐색 메뉴에서 **데이터 커넥터** 를 클릭 합니다. 커넥터 목록에서 **CEF (일반 이벤트 형식)** 타일을 클릭 한 다음 오른쪽 아래에 있는 **커넥터 페이지 열기** 단추를 클릭 합니다. 

1. 1.2 아래에서 **Linux 컴퓨터에 CEF 수집기를 설치** 하 고, 다음 스크립트 실행에서 제공 된 링크를 복사 **하 여 cef 수집기를 설치 하 고 적용** 하거나 아래 텍스트에서 (자리 표시자 대신 작업 영역 ID 및 기본 키를 적용 합니다.)

    ```bash
    sudo wget -O cef_installer.py https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]
    ```

1. 스크립트를 실행 하는 동안 오류 또는 경고 메시지가 표시 되지 않는지 확인 합니다.
    - 명령을 실행 하 여 *컴퓨터* 필드의 매핑에 대 한 문제를 해결 하도록 지시 하는 메시지가 표시 될 수 있습니다. 자세한 내용은 [배포 스크립트의 설명을](#mapping-command) 참조 하십시오.

1. [2 단계: CEF 메시지를 전달 하도록 보안 솔루션 구성](connect-cef-solution-config.md)을 계속 진행 합니다.


> [!NOTE]
> **동일한 컴퓨터를 사용 하 여 일반 Syslog *및* cef 메시지 모두 전달**
>
> 이 로그 전달자 컴퓨터를 사용 하 여 CEF 뿐만 아니라 [syslog 메시지](connect-syslog.md) 를 전달 하려는 경우 Syslog 및 CommonSecurityLog 테이블에 대 한 이벤트 중복을 방지 하기 위해 다음을 수행 합니다.
>
> 1. CEF 형식의 전달자에 게 로그를 전송 하는 각 원본 컴퓨터에서 Syslog 구성 파일을 편집 하 여 CEF 메시지를 보내는 데 사용 되는 기능을 제거 해야 합니다. 이러한 방식으로 CEF에서 전송 되는 기능은 Syslog로 보내지지 않습니다. 이 작업을 수행 하는 방법에 대 한 자세한 지침은 [Linux 에이전트에서 Syslog 구성](../azure-monitor/agents/data-sources-syslog.md#configure-syslog-on-linux-agent) 을 참조 하세요.
>
> 1. Azure 센티널의 Syslog 구성과 에이전트의 동기화를 사용 하지 않도록 설정 하려면 해당 컴퓨터에서 다음 명령을 실행 해야 합니다. 이렇게 하면 이전 단계에서 변경한 구성 변경을 덮어쓰지 않습니다.<br>
> `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable'`

> [!NOTE]
> **TimeGenerated 필드의 원본 변경**
>
> - 기본적으로 Log Analytics 에이전트는 스키마의 *Timegenerated* 필드를 Syslog 디먼에서 에이전트가 이벤트를 받은 시간으로 채웁니다. 따라서 원본 시스템에서 이벤트가 생성된 시간은 Azure Sentinel에 기록되지 않습니다.
>
> - 그러나 스크립트를 다운로드 하 여 실행 하는 다음 명령을 실행할 수 있습니다 `TimeGenerated.py` . 이 스크립트는 에이전트에서 수신 된 시간 대신 원본 시스템에서 *Timegenerated* 필드를 이벤트의 원래 시간으로 채우도록 Log Analytics 에이전트를 구성 합니다.
>
>    ```bash
>    wget -O TimeGenerated.py https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/TimeGenerated.py && python TimeGenerated.py {ws_id}
>    ```

## <a name="deployment-script-explained"></a>배포 스크립트 설명

다음은 배포 스크립트 동작에 대 한 명령 단계별 설명입니다.

Syslog 디먼을 선택 하 여 적절 한 설명을 확인 합니다.

# <a name="rsyslog-daemon"></a>[rsyslog 디먼](#tab/rsyslog)

1. **Log Analytics 에이전트 다운로드 및 설치:**

    - Log Analytics (OMS) Linux 에이전트에 대 한 설치 스크립트를 다운로드 합니다.

        ```bash
        wget -O onboard_agent.sh https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/
            master/installer/scripts/onboard_agent.sh
        ```

    - Log Analytics 에이전트를 설치 합니다.
    
        ```bash
        sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com
        ```

1. **Log Analytics 에이전트 구성이 포트 25226에서 수신 대기 하도록 설정 하 고 CEF 메시지를 Azure 센티널로 전달 합니다.**

    - Log Analytics agent GitHub 리포지토리에서 구성을 다운로드 합니다.

        ```bash
        wget -O /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf
            https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/
            omsagent.d/security_events.conf
        ```

1. **Syslog 디먼 구성:**

    - Syslog 구성 파일을 사용 하 여 TCP 통신용 포트 514를 엽니다 `/etc/rsyslog.conf` .

    - Syslog 디먼 디렉터리에 특수 구성 파일을 삽입 하 여 CEF 메시지를 TCP 포트 25226의 Log Analytics 에이전트로 전달 하도록 디먼을 구성 `security-config-omsagent.conf` `/etc/rsyslog.d/` 합니다.

        파일의 내용 `security-config-omsagent.conf` :

        ```bash
        if $rawmsg contains "CEF:" or $rawmsg contains "ASA-" then @@127.0.0.1:25226 
        ```

1. **Syslog 디먼 및 Log Analytics 에이전트를 다시 시작 하는 중:**

    - Rsyslog 디먼을 다시 시작 합니다.
    
        ```bash
        service rsyslog restart
        ```

    - Log Analytics 에이전트를 다시 시작 합니다.

        ```bash
        /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

1. **예상 대로 *컴퓨터* 필드의 매핑을 확인 하는 중:**

    - 다음 명령을 사용 하 여 syslog 원본의 *컴퓨터* 필드가 Log Analytics 에이전트에서 올바르게 매핑되는지 확인 합니다. 

        ```bash
        grep -i "'Host' => record\['host'\]"  /opt/microsoft/omsagent/plugin/filter_syslog_security.rb
        ```
    - <a name="mapping-command"></a>매핑에 문제가 있는 경우 스크립트는 **다음 명령을 수동으로 실행** 하는 오류 메시지를 생성 합니다 (자리 표시자 대신 작업 영역 ID 적용). 이 명령은 올바른 매핑을 확인 하 고 에이전트를 다시 시작 합니다.
    
        ```bash
        sed -i -e "/'Severity' => tags\[tags.size - 1\]/ a \ \t 'Host' => record['host']" -e "s/'Severity' => tags\[tags.size - 1\]/&,/" /opt/microsoft/omsagent/plugin/filter_syslog_security.rb && sudo /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

# <a name="syslog-ng-daemon"></a>[syslog-기능 데몬](#tab/syslogng)

1. **Log Analytics 에이전트 다운로드 및 설치:**

    - Log Analytics (OMS) Linux 에이전트에 대 한 설치 스크립트를 다운로드 합니다.

        ```bash
        wget -O onboard_agent.sh https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/
            master/installer/scripts/onboard_agent.sh
        ```

    - Log Analytics 에이전트를 설치 합니다.
    
        ```bash
        sh onboard_agent.sh -w [workspaceID] -s [Primary Key] -d opinsights.azure.com
        ```

1. **Log Analytics 에이전트 구성이 포트 25226에서 수신 대기 하도록 설정 하 고 CEF 메시지를 Azure 센티널로 전달 합니다.**

    - Log Analytics agent GitHub 리포지토리에서 구성을 다운로드 합니다.

        ```bash
        wget -O /etc/opt/microsoft/omsagent/[workspaceID]/conf/omsagent.d/security_events.conf
            https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/installer/conf/
            omsagent.d/security_events.conf
        ```

1. **Syslog 디먼 구성:**

    - Syslog 구성 파일을 사용 하 여 TCP 통신용 포트 514를 엽니다 `/etc/syslog-ng/syslog-ng.conf` .

    - Syslog 디먼 디렉터리에 특수 구성 파일을 삽입 하 여 CEF 메시지를 TCP 포트 25226의 Log Analytics 에이전트로 전달 하도록 디먼을 구성 `security-config-omsagent.conf` `/etc/syslog-ng/conf.d/` 합니다.

        파일의 내용 `security-config-omsagent.conf` :

        ```bash
        filter f_oms_filter {match(\"CEF\|ASA\" ) ;};destination oms_destination {tcp(\"127.0.0.1\" port(25226));};
        log {source(s_src);filter(f_oms_filter);destination(oms_destination);};
        ```

1. **Syslog 디먼 및 Log Analytics 에이전트를 다시 시작 하는 중:**

    - Syslog를 다시 시작 합니다.
    
        ```bash
        service syslog-ng restart
        ```

    - Log Analytics 에이전트를 다시 시작 합니다.

        ```bash
        /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```

1. **예상 대로 *컴퓨터* 필드의 매핑을 확인 하는 중:**

    - 다음 명령을 사용 하 여 syslog 원본의 *컴퓨터* 필드가 Log Analytics 에이전트에서 올바르게 매핑되는지 확인 합니다. 

        ```bash
        grep -i "'Host' => record\['host'\]"  /opt/microsoft/omsagent/plugin/filter_syslog_security.rb
        ```
    - <a name="mapping-command"></a>매핑에 문제가 있는 경우 스크립트는 **다음 명령을 수동으로 실행** 하는 오류 메시지를 생성 합니다 (자리 표시자 대신 작업 영역 ID 적용). 이 명령은 올바른 매핑을 확인 하 고 에이전트를 다시 시작 합니다.
    
        ```bash
        sed -i -e "/'Severity' => tags\[tags.size - 1\]/ a \ \t 'Host' => record['host']" -e "s/'Severity' => tags\[tags.size - 1\]/&,/" /opt/microsoft/omsagent/plugin/filter_syslog_security.rb && sudo /opt/microsoft/omsagent/bin/service_control restart [workspaceID]
        ```
---

## <a name="next-steps"></a>다음 단계

이 문서에서는 CEF 어플라이언스를 Azure 센티널에 연결 하는 Log Analytics 에이전트를 배포 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- [데이터에 대한 가시성을 얻고 재적 위협을 확인](quickstart-get-visibility.md)하는 방법을 알아봅니다.
- [Azure Sentinel을 사용하여 위협 검색](./tutorial-detect-threats-built-in.md)을 시작합니다.
