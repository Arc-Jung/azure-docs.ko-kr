---
title: Azure 애플리케이션 Insights 에이전트 문제 해결 및 알려진 문제 | Microsoft Docs
description: Application Insights 에이전트 및 문제 해결 예제에 대 한 알려진 문제입니다. 웹 사이트를 다시 배포 하지 않고 웹 사이트 성능을 모니터링 합니다. 온-프레미스, Vm 또는 Azure에서 호스트 되는 ASP.NET 웹 앱에서 작동 합니다.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 2641218fa9ddef65c45f2f1a9c9ce807cef35048
ms.sourcegitcommit: c8b50a8aa8d9596ee3d4f3905bde94c984fc8aa2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2021
ms.locfileid: "105642743"
---
# <a name="troubleshooting-application-insights-agent-formerly-named-status-monitor-v2"></a>Application Insights 에이전트 문제 해결 (이전에 명명 된 상태 모니터 v2)

모니터링을 사용 하도록 설정 하면 데이터 수집을 방해 하는 문제가 발생할 수 있습니다.
이 문서에서는 알려진 모든 문제를 나열 하 고 문제 해결 예제를 제공 합니다.

## <a name="known-issues"></a>알려진 문제

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>응용 프로그램의 bin 디렉터리에서 Dll이 충돌 합니다.

이러한 Dll이 bin 디렉터리에 있는 경우 모니터링이 실패할 수 있습니다.

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

이러한 Dll 중 일부는 앱에서 사용 하지 않는 경우에도 Visual Studio 기본 앱 템플릿에 포함 되어 있습니다.
문제 해결 도구를 사용 하 여 증상 동작을 확인할 수 있습니다.

- PerfView
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- IISReset 및 앱 부하 (원격 분석 없음) Sysinternals (Handle.exe 및 ListDLLs.exe)를 사용 하 여 조사 합니다.
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="powershell-versions"></a>PowerShell 버전
이 제품은 PowerShell v 5.1을 사용 하 여 작성 및 테스트 되었습니다.
이 모듈은 PowerShell 버전 6 또는 7과 호환 되지 않습니다.
PowerShell v 5.1은 최신 버전과 함께 사용 하는 것이 좋습니다. 자세한 내용은 powershell [5.1과 함께 powershell 7 사용](/powershell/scripting/install/migrating-from-windows-powershell-51-to-powershell-7#using-powershell-7-side-by-side-with-windows-powershell-51)을 참조 하세요.

### <a name="conflict-with-iis-shared-configuration"></a>IIS 공유 구성과 충돌 합니다.

웹 서버 클러스터가 있는 경우 [공유 구성을](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211)사용할 수 있습니다.
HttpModule은이 공유 구성에 삽입할 수 없습니다.
각 웹 서버에서 Enable 명령을 실행 하 여 각 서버의 GAC에 DLL을 설치 합니다.

Enable 명령을 실행 한 후 다음 단계를 완료 합니다.
1. 공유 구성 디렉터리로 이동 하 여 applicationHost.config 파일을 찾습니다.
2. 구성의 모듈 섹션에 다음 줄을 추가 합니다.
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```

### <a name="iis-nested-applications"></a>IIS 중첩 응용 프로그램

1.0 버전에서 IIS의 중첩 된 응용 프로그램을 계측 하지 않습니다.

### <a name="advanced-sdk-configuration-isnt-available"></a>고급 SDK 구성을 사용할 수 없습니다.

SDK 구성은 버전 1.0에서 최종 사용자에 게 노출 되지 않습니다.

    
    
## <a name="troubleshooting"></a>문제 해결
    
### <a name="troubleshooting-powershell"></a>PowerShell 문제 해결

#### <a name="determine-which-modules-are-available"></a>사용할 수 있는 모듈 확인
명령을 사용 하 여 `Get-Module -ListAvailable` 설치 된 모듈을 확인할 수 있습니다.

#### <a name="import-a-module-into-the-current-session"></a>모듈을 현재 세션으로 가져오기
모듈이 PowerShell 세션으로 로드 되지 않은 경우 명령을 사용 하 여 수동으로 로드할 수 있습니다 `Import-Module <path to psd1>` .


### <a name="troubleshooting-the-application-insights-agent-module"></a>Application Insights Agent 모듈 문제 해결

#### <a name="list-the-commands-available-in-the-application-insights-agent-module"></a>Application Insights Agent 모듈에서 사용할 수 있는 명령을 나열 합니다.
명령을 실행 `Get-Command -Module Az.ApplicationMonitor` 하 여 사용 가능한 명령을 가져옵니다.

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.4.0      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.4.0      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Start-ApplicationInsightsMonitoringTrace           0.4.0      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-application-insights-agent-module"></a>Application Insights 에이전트 모듈의 현재 버전 확인
명령을 실행 `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` 하 여 모듈에 대 한 다음 정보를 표시 합니다.
   - PowerShell 모듈 버전
   - Application Insights SDK 버전
   - PowerShell 모듈의 파일 경로
    
이 cmdlet을 사용 하는 방법에 대 한 자세한 내용은 [API 참조를 참조](status-monitor-v2-api-reference.md) 하세요.


### <a name="troubleshooting-running-processes"></a>실행 중인 프로세스 문제 해결

계측 된 컴퓨터에서 프로세스를 검사 하 여 모든 Dll이 로드 되었는지 확인할 수 있습니다.
모니터링이 작동 하는 경우 12 개 이상의 Dll을 로드 해야 합니다.

명령을 사용 `Get-ApplicationInsightsMonitoringStatus -InspectProcess` 하 여 dll을 확인 합니다.

이 cmdlet을 사용 하는 방법에 대 한 자세한 내용은 [API 참조를 참조](status-monitor-v2-api-reference.md) 하세요.


### <a name="collect-etw-logs-by-using-perfview"></a>PerfView를 사용 하 여 ETW 로그 수집

#### <a name="setup"></a>설정

1. [GitHub](https://github.com/Microsoft/perfview/releases)에서 PerfView.exe 및 PerfView64.exe를 다운로드 합니다.
2. PerfView64.exe를 시작 합니다.
3. **고급 옵션** 을 확장합니다.
4. 다음 확인란의 선택을 취소 합니다.
    - **우편번호**
    - **병합**
    - **.NET 기호 컬렉션**
5. 다음과 같은 **추가 공급자** 를 설정 합니다. `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>로그 수집

1. 관리자 권한이 있는 명령 콘솔에서 명령을 실행 하 여 `iisreset /stop` IIS 및 모든 웹 앱을 해제 합니다.
2. PerfView에서 **컬렉션 시작** 을 선택 합니다.
3. 관리자 권한이 있는 명령 콘솔에서 명령을 실행 하 여 `iisreset /start` IIS를 시작 합니다.
4. 앱으로 이동 해 보세요.
5. 앱이 로드 된 후 PerfView로 돌아가서 **수집 중지** 를 선택 합니다.

### <a name="how-to-capture-full-sql-command-text"></a>전체 SQL 명령 텍스트를 캡처하는 방법

전체 SQL 명령 텍스트를 캡처하려면 다음과 같이 applicationinsights.config 파일을 수정 해야 합니다.

```xml
<Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">,
<EnableSqlCommandTextInstrumentation>true</EnableSqlCommandTextInstrumentation>
</Add>
```

## <a name="next-steps"></a>다음 단계

- [API 참조](status-monitor-v2-overview.md#powershell-api-reference) 를 검토 하 여 누락 된 매개 변수에 대해 알아보세요.