---
title: Azure 애플리케이션 Insights Profiler를 사용 하 여 문제 해결
description: 이 문서에는 Application Insights Profiler를 사용하도록 설정하거나 사용하는 데 문제가 있는 개발자에게 도움이 되는 문제 해결 단계 및 정보가 나와 있습니다.
ms.topic: conceptual
author: cweining
ms.author: cweining
ms.date: 08/06/2018
ms.reviewer: mbullwin
ms.openlocfilehash: aa9b186e74ed3b8fe5496afd5b21c54f50537d5f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87049781"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Application Insights Profiler를 사용하도록 설정하거나 볼 때 발생하는 문제 해결

> [!CAUTION]
> Azure App Service에서 ASP.NET Core 앱에 대 한 프로파일러를 실행 하는 버그가 있습니다. 해결 방법이 있지만 전 세계에 배포 하는 데 몇 주가 걸립니다. [여기](./asp-net-core.md#enable-application-insights-server-side-telemetry-visual-studio)에 지침을 사용 하 여 응용 프로그램에 Application Insights SDK를 추가 하 여 버그를 해결할 수 있습니다.

## <a name="general-troubleshooting"></a><a id="troubleshooting"></a>일반 문제 해결

### <a name="profiles-are-uploaded-only-if-there-are-requests-to-your-application-while-profiler-is-running"></a>Profiler가 실행되는 동안 애플리케이션에 대한 요청이 있을 때만 프로필에 업로드됨

Azure Application Insights Profiler는 1시간마다 2분 동안 프로파일링 데이터를 수집합니다. 또한 **Application Insights Profiler 구성** 창에서 **지금 프로필** 단추를 선택해도 데이터를 수집합니다. 하지만 프로파일링 데이터는 Profiler가 실행되는 동안 발생한 요청에 첨부할 수 있을 때만 업로드됩니다. 

Profiler는 Application Insights 리소스에 추적 메시지 및 사용자 지정 이벤트를 씁니다. 해당 이벤트를 사용하여 Profiler의 실행 방식을 확인할 수 있습니다. Profiler가 실행 중이며 추적을 캡처해야 하는데 **성능** 창에 추적이 표시되지 않으면 Profiler의 실행 방식을 확인할 수 있습니다.

1. Profiler가 Application Insights 리소스에 보낸 추적 메시지 및 사용자 지정 이벤트를 검색합니다. 이러한 검색 문자열을 사용하여 관련 데이터를 찾을 수 있습니다.

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    다음 이미지에는 두 AI 리소스에서 수행한 검색의 두 가지 예가 나와 있습니다. 
    
   * 왼쪽 예의 경우 Profiler가 실행되는 동안 애플리케이션이 요청을 수신하지 않습니다. 그리고 활동이 없어서 업로드가 취소되었음을 설명하는 메시지가 표시됩니다. 

   * 오른쪽 예의 경우에는 Profiler가 시작되었으며, Profiler가 실행되는 동안 발생한 요청이 검색되면 사용자 지정 이벤트가 전송되었습니다. ServiceProfilerSample 사용자 지정 이벤트가 표시되는 경우 Profiler가 추적을 요청에 첨부한 것이며, 그러면 **Application Insights 성능** 창에서 추적을 확인할 수 있습니다.

     원격 분석이 표시되지 않으면 Profiler 실행되고 있지 않은 것입니다. 이러한 문제를 해결하려면 이 문서 뒷부분에서 사용 중인 특정 앱 유형에 해당하는 문제 해결 섹션을 참조하세요.  

     ![Profiler 원격 분석 검색][profiler-search-telemetry]

1. Profiler가 실행되는 동안 요청이 발생한 경우 Profiler가 사용하도록 설정된 애플리케이션 부분에서 해당 요청을 처리했는지 확인합니다. 애플리케이션은 여러 구성 요소를 포함할 수도 있지만 Profiler는 일부 구성 요소에서만 사용하도록 설정됩니다. 추적이 업로드된 구성 요소는 **Application Insights Profiler 구성** 창에 표시됩니다.

### <a name="other-things-to-check"></a>기타 확인해야 할 사항
* 앱이 .NET Framework 4.6에서 실행 중인지 확인합니다.
* 웹앱이 ASP.NET Core 애플리케이션인 경우 ASP.NET Core 2.0 이상을 실행해야 합니다.
* 확인하려는 데이터가 생성된 지 2주 이상 지난 경우에는 시간 필터의 범위를 제한한 후에 다시 시도해 봅니다. 추적은 7일 후에 삭제됩니다.
* 프록시 또는 방화벽이 https://gateway.azureserviceprofiler.net에 대한 액세스를 차단하지 않았는지 확인합니다.
* 프로파일러는 무료 또는 공유 app service 계획에서 지원 되지 않습니다. 이러한 계획 중 하나를 사용 하는 경우 기본 계획 중 하나로 확장을 시도 하 고 프로파일러가 작업을 시작 해야 합니다.

### <a name="double-counting-in-parallel-threads"></a><a id="double-counting"></a>병렬 스레드에서 이중 계산

경우에 따라 스택 뷰어의 총 시간 메트릭은 요청 기간 이상입니다.

요청과 연결된 둘 이상의 스레드가 병렬로 작동 중이면 이러한 상황이 발생할 수 있습니다. 이 경우 총 스레드 시간은 경과된 시간 이상입니다. 하나의 스레드가 다른 스레드가 완료될 때까지 대기 중일 수 있습니다. 뷰어는 이러한 상황의 검색을 시도하여 불필요한 대기를 생략합니다. 그런데 이 과정에서 중요할 수 있는 정보를 생략할 위험을 방지하기 위해 너무 많은 정보가 표시되는 문제가 발생합니다.

추적에 병렬 스레드가 표시되면 대기 중인 스레드를 확인하여 요청에 중요한 경로를 파악합니다. 대기 상태로 빠르게 전환되는 스레드는 다른 스레드를 기다리는 중일 뿐인 경우가 많습니다. 다른 스레드에 집중하고 대기 중인 스레드 대기 시간을 무시합니다.

### <a name="error-report-in-the-profile-viewer"></a>프로파일링 뷰어의 오류 보고서
포털에서 지원 티켓을 제출합니다. 오류 메시지의 상관 관계 ID를 포함해야 합니다.

## <a name="troubleshoot-profiler-on-azure-app-service"></a>Azure App Service에서 Profiler 문제 해결
Profiler가 제대로 작동하도록 하려면 다음 조건을 충족해야 합니다.
* 웹앱 서비스 계획이 기본 계층 이상이어야 합니다.
* 웹앱에서 Application Insights를 사용할 수 있어야 합니다.
* 웹 앱에는 다음과 같은 앱 설정이 있어야 합니다.

    |앱 설정    | 값    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights 리소스용 iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |


* **ApplicationInsightsProfiler3** 웹 작업이 실행되고 있어야 합니다. 웹 작업을 확인하려면 다음 단계를 수행합니다.
   1. [Kudu](/archive/blogs/cdndevs/the-kudu-debug-console-azure-websites-best-kept-secret)로 이동합니다.
   1. **도구** 메뉴에서 **웹 작업 대시보드**를 선택합니다.  
      **웹 작업** 창이 열립니다. 
   
      ![profiler-webjob]   
   
   1. 로그를 포함 하 여 webjob의 세부 정보를 보려면 **ApplicationInsightsProfiler3** 링크를 선택 합니다.  
     **지속적인 웹 작업 세부 정보** 창이 열립니다.

      ![profiler-webjob-log]

프로파일러가 작동 하지 않는 이유를 알 수 없는 경우 로그를 다운로드 하 여 팀에 보내 도움을 받을 수 있습니다 serviceprofilerhelp@microsoft.com . 
    
### <a name="manual-installation"></a>수동 설치

Profiler를 구성하면 웹앱의 설정에 업데이트가 이루어집니다. 사용 중인 환경에 필요한 경우 수동으로 업데이트를 적용할 수 있습니다. 예제에서 애플리케이션이 PowerApps의 Web Apps 환경에서 실행될 수 있습니다. 수동으로 업데이트를 적용 하려면:

1. **웹 앱 제어** 창에서 **설정**을 엽니다.

1. **.NET Framework 버전** 을 **v 4.6**으로 설정 합니다.

1. **무중단**을 **사용**으로 설정합니다.
1. 다음 앱 설정을 만듭니다.

    |앱 설정    | 값    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights 리소스용 iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |

### <a name="too-many-active-profiling-sessions"></a>너무 많은 활성 프로파일링 세션

현재 동일한 서비스 계획으로 실행 중인 최대 4개의 Azure 웹앱 및 배포 슬롯에서 Profiler를 사용하도록 설정할 수 있습니다. App Service 계획 하나에서 웹앱을 5개 이상 실행 중인 경우 Profiler에서 *Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException*을 throw할 수 있습니다. Profiler는 각 웹앱에 대해 개별적으로 실행되며 각 앱에 대한 ETW(Windows용 이벤트 추적) 세션을 시작하려고 합니다. 하지만 한 번에 활성화할 수 있는 ETW 세션의 수는 제한되어 있습니다. Profiler 웹 작업이 너무 많은 활성 프로파일링 세션을 보고하는 경우 일부 웹앱을 다른 Service 계획으로 이동합니다.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootapp_datajobs"></a>배포 오류: 디렉터리가 비어 있지 않음 'D:\\home\\site\\wwwroot\\App_Data\\jobs'

Profiler를 사용하는 Web Apps 리소스에 웹앱을 다시 배포하는 경우 다음과 같은 메시지가 표시될 수 있습니다.

*디렉터리가 비어 있지 않음 ' d: \\ home \\ site \\ wwwroot \\ App_Data \\ job '*

스크립트 또는 Azure DevOps 배포 파이프라인에서 웹 배포를 실행하는 경우 이 오류가 발생합니다. 솔루션은 웹 배포 작업에 다음과 같은 배포 매개 변수를 더 추가합니다.

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

이러한 매개 변수는 Application Insights Profiler에서 사용하는 폴더를 삭제하고 재배포 프로세스의 차단을 해제합니다. 현재 실행 중인 Profiler 인스턴스에는 영향을 주지 않습니다.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Application Insights Profiler가 실행되고 있는지 어떻게 알 수 있나요?

Profiler는 웹앱에서 지속적인 웹 작업으로 실행됩니다. [Azure Portal](https://portal.azure.com)에서 웹 앱 리소스를 열 수 있습니다. **WebJobs** 창에서 **ApplicationInsightsProfiler**의 상태를 확인합니다. 실행되지 않는 경우 **로그**를 열어 자세한 정보를 찾습니다.

## <a name="troubleshoot-vms-and-cloud-services"></a>Vm 및 Cloud Services 문제 해결

>**Cloud Services에 대 한 WAD에서 제공 되는 프로파일러의 버그가 수정 되었습니다.** Cloud Services에 대 한 최신 버전의 WAD (1.12.2.0)는 최신 버전의 App Insights SDK와 함께 작동 합니다. 클라우드 서비스 호스트는 WAD를 자동으로 업그레이드 하지만 즉시 실행 되지 않습니다. 강제로 업그레이드 하려면 서비스를 다시 배포 하거나 노드를 다시 부팅 하면 됩니다.

Azure Diagnostics를 통해 Profiler가 올바르게 구성되어 있는지 여부를 확인하려면 다음의 세 가지 작업을 수행합니다. 
1. 먼저 배포된 Azure Diagnostics 구성의 내용이 필요한 구성인지를 확인합니다. 

1. 다음으로 Azure Diagnostics가 Profiler 명령줄에서 적절한 iKey를 전달하는지 확인합니다. 

1. 셋째로는 Profiler 로그 파일에서 Profiler가 실행되었으나 오류가 반환되었는지 여부를 확인합니다. 

Azure Diagnostics를 구성하는 데 사용된 설정을 확인하려면 다음 단계를 수행합니다.

1. VM (가상 컴퓨터)에 로그인 한 다음이 위치에서 로그 파일을 엽니다. 컴퓨터에서 플러그 인 버전이 최신 버전이 될 수 있습니다.
    
    Vm의 경우:
    ```
    c:\WindowsAzure\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log
    ```
    
    Cloud Services의 경우:
    ```
    c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log  
    ```

1. 해당 파일에서 **WadCfg** 문자열을 검색하면 Azure Diagnostics를 구성하기 위해 VM에 전달된 설정을 확인할 수 있습니다. Profiler 싱크에서 사용하는 iKey가 올바른지 확인할 수 있습니다.

1. Profiler를 시작하는 데 사용되는 명령줄을 확인합니다. 프로파일러를 시작 하는 데 사용 되는 인수는 다음 파일에 있습니다. 드라이브는 c: 또는 d: 이며 디렉터리를 숨길 수도 있습니다.

    Vm의 경우:
    ```
    C:\ProgramData\ApplicationInsightsProfiler\config.json
    ```
    
    Cloud Services:
    ```
    D:\ProgramData\ApplicationInsightsProfiler\config.json
    ```

1. Profiler 명령줄의 ikey가 올바른지 확인합니다. 

1. 파일 *의 이전config.js* 에 있는 경로를 사용 하 여 **BootstrapN**라는 프로파일러 로그 파일을 확인 합니다. 이 파일에는 Profiler가 사용하는 설정을 나타내는 디버그 정보가 표시됩니다. 또한 Profiler의 상태 및 오류 메시지도 표시됩니다.  

    Vm의 경우 파일은 일반적으로 다음 위치에 있습니다.
    ```
    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.17.0.6\ApplicationInsightsProfiler
    ```

    Cloud Services의 경우:
    ```
    C:\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.17.0.6\ApplicationInsightsProfiler
    ```

    응용 프로그램에서 요청을 수신 하는 동안 프로파일러가 실행 중인 경우 *iKey에서 검색 된 작업*인 다음 메시지가 표시 됩니다. 

    추적을 업로드 하는 동안 다음 메시지가 표시 됩니다. *추적 업로드를 시작*합니다. 


## <a name="edit-network-proxy-or-firewall-rules"></a>네트워크 프록시 또는 방화벽 규칙 편집

응용 프로그램이 프록시 또는 방화벽을 통해 인터넷에 연결 하는 경우 응용 프로그램이 Application Insights Profiler 서비스와 통신할 수 있도록 규칙을 편집 해야 할 수 있습니다. Application Insights Profiler에서 사용 하는 Ip는 Azure Monitor 서비스 태그에 포함 됩니다.


[profiler-search-telemetry]:./media/profiler-troubleshooting/Profiler-Search-Telemetry.png
[profiler-webjob]:./media/profiler-troubleshooting/Profiler-webjob.png
[profiler-webjob-log]:./media/profiler-troubleshooting/Profiler-webjob-log.png
