---
title: Application Insights를 사용하여 SharePoint 사이트 모니터링
description: 새 계측 키를 사용하여 새 애플리케이션 모니터링 시작
ms.topic: conceptual
ms.date: 07/11/2018
ms.openlocfilehash: 395e8d667985318f4a084428c6fd4c395ee8b956
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671446"
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Application Insights를 사용하여 SharePoint 사이트 모니터링
Azure Application Insights는 애플리케이션의 가용성, 성능 및 사용량을 모니터링합니다. 여기에서는 SharePoint 사이트에 맞게 설정하는 방법을 알아봅니다.

## <a name="create-an-application-insights-resource"></a>Application Insights 리소스 만들기
[Azure Portal](https://portal.azure.com)에서 새 Application Insights 리소스를 만듭니다. 애플리케이션 유형으로 ASP.NET을 선택합니다.

![속성 클릭, 키 선택 및 ctrl+C 누르기](./media/sharepoint/001.png)

열리는 창에서 앱의 성능 및 사용량 현황 데이터를 볼 수 있습니다. 다음에 Azure에 로그인할 때 다시 이 블레이드로 돌아가려면 시작 화면에서 해당 타일을 찾아야 합니다. 또는 찾아보기를 클릭하여 찾아야 합니다.

## <a name="add-the-script-to-your-web-pages"></a>웹 페이지에 스크립트 추가

```HTML
<!-- 
To collect user behavior analytics tools about your application, 
insert the following script into each page you want to track.
Place this code immediately before the closing </head> tag,
and before any other scripts. Your first data will appear 
automatically in just a few seconds.
-->
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

추적 하려는 모든 페이지의 &lt;/head&gt; 태그 바로 앞에 스크립트를 삽입 합니다. 웹 사이트에 마스터 페이지가 있으면 스크립트를 넣을 수 있습니다. 예를 들어 ASP.NET MVC 프로젝트에서는 View\Shared\_Layout.cshtml에 추가합니다.

스크립트에는 Application Insights 리소스에 원격 분석을 전달하는 계측 키가 포함됩니다.

### <a name="add-the-code-to-your-site-pages"></a>사이트 페이지에 코드를 추가합니다.
#### <a name="on-the-master-page"></a>마스터 페이지에서
사이트의 마스터 페이지를 편집할 수 있는 경우 사이트의 모든 페이지에 대한 모니터링을 제공합니다.

마스터 페이지를 체크 아웃하고 SharePoint Designer 또는 다른 편집기를 사용하여 편집합니다.

![](./media/sharepoint/03-master.png)

</head> 태그 바로 앞에 코드를 추가합니다. 

![](./media/sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>또는 개별 페이지에서
제한된 페이지 집합을 모니터링하려면 각 페이지에 개별적으로 스크립트를 추가합니다. 

웹 파트를 삽입하고 코드 조각을 포함합니다.

![](./media/sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>앱에 대한 데이터 보기
응용 프로그램을 다시 배포 합니다.

[Azure Portal](https://portal.azure.com)에서 사용자 애플리케이션 블레이드로 돌아갑니다.

첫 번째 이벤트가 검색에 표시됩니다. 

![](./media/sharepoint/09-search.png)

더 많은 데이터를 기대하는 경우 몇 초 후에 새로고침을 클릭합니다.

## <a name="capturing-user-id"></a>사용자 ID 캡처
표준 웹 페이지 코드 조각은 SharePoint에서 사용자 ID를 캡처하지 않지만 약간 수정하여 캡처할 수 있습니다.

1. Application Insights의 Essentials 드롭 다운에서 앱의 계측 키를 복사합니다. 

    ![](./media/sharepoint/02-props.png)

1. 다음 코드 조각에서 'XXXX'에 대한 계측 키를 대체합니다. 
2. 포털에서 가져온 코드 조각 대신 SharePoint 앱에 스크립트를 포함합니다.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>다음 단계
* [웹 테스트](../../azure-monitor/app/monitor-web-app-availability.md)
* 다른 유형의 앱에 대한[Application Insights](../../azure-monitor/app/app-insights-overview.md).

<!--Link references-->


