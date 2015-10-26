<properties 
	pageTitle="종속성, 예외 및 Java 웹앱에서의 실행 시간을 모니터링합니다." 
	description="Application Insights를 사용하여 Java 웹 사이트에 대한 모니터링을 확장합니다." 
	services="application-insights" 
    documentationCenter="java"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/09/2015" 
	ms.author="awills"/>
 
# 종속성, 예외 및 Java 웹앱에서의 실행 시간을 모니터링합니다.

*Application Insights는 미리 보기 상태입니다.*

[Application Insights로 Java 웹앱을 계측][java]한 경우, Java Agent를 사용하여 코드의 변경 없이 보다 심층적인 정보를 얻을 수 있습니다.


* **종속성:** 응용 프로그램이 다음을 포함한 다른 구성 요소에 수행하는 호출에 대한 데이터:
 * HttpClient, OkHttp 및 RestTemplate(Spring)을 통해 수행한 **REST 호출**
 * Jedis 클라이언트를 통해 수행한 **Redis** 호출. 호출 시간이 10초보다 길면, 에이전트도 호출 인수를 가져옵니다.
 * **[JDBC 호출](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB 또는 Apache Derby DB. "executeBatch"호출이 지원됩니다. MySQL 및 PostgreSQL의 경우 호출 시간이 10초보다 길면 에이전트가 쿼리 계획을 보고합니다. 
* **예외 포착:** 코드에서 처리하는 예외에 대한 데이터입니다.
* **메서드 실행 시간:** 특정 메서드를 실행하는 데 걸리는 시간에 대한 데이터입니다.

Java 에이전트를 사용하려면 사용자의 서버에 설치합니다. [Application Insights Java SDK][java]를 사용하여 웹앱을 계측해야 합니다.

## Java용 Application Insights 에이전트 설치

1. Java 서버를 실행 중인 컴퓨터에서 [에이전트를 다운로드](http://go.microsoft.com/fwlink/?LinkId=618633)합니다.
2. 응용 프로그램 서버 시작 스크립트를 편집하여 다음 JVM을 추가합니다.

    `javaagent:`*에이전트 JAR 파일에 대한 전체 경로*

    예를 들어 Linux 컴퓨터의 Tomcat에서:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. 응용 프로그램 서버를 다시 시작합니다.

## 에이전트 구성

`AI-Agent.xml` 파일을 만들어 에이전트 JAR 파일과 동일한 폴더에 배치합니다.

xml 파일의 내용을 설정합니다. 다음 예제를 편집하여 원하는 기능을 포함 또는 생략합니다.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments will be sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan will be reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String:I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

개별 메서드에 대한 메서드 타이밍과 예외를 보고할 수 있도록 설정해야 합니다.

기본적으로 `reportExecutionTime`은 true이고 `reportCaughtExceptions`는 false입니다.

## 데이터 보기

Application Insights 리소스에서 원격 종속성과 메서드 실행 시간의 합계는 [성능 타일 아래][metrics]에 나타납니다.

종속성의 개별 인스턴스, 예외 및 메서드 보고서를 찾으려면 [검색][diagnostic]을 엽니다.

[종속성 문제 진단 - 자세한 내용](app-insights-dependencies.md#diagnosis).



## 질문이 있으십니까? 문제가 있습니까?

[Java 문제 해결](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 

<!---HONumber=Oct15_HO3-->