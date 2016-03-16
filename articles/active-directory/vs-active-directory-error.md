<properties 
	pageTitle="인증 검색 중 오류 발생" 
	description="Active Directory 연결 마법사에서 호환되지 않는 인증 유형 검색" 
	services="active-directory" 
	documentationCenter="" 
	authors="TomArcher" 
	manager="douge" 
	editor=""/>
  
<tags 
	ms.service="active-directory" 
	ms.workload="web" 
	ms.tgt_pltfrm="vs-getting-started" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="12/10/2015" 
	ms.author="tarcher"/>

# 인증 검색 중 오류 발생

이전 인증 코드를 검색하는 동안 마법사에서 호환되지 않는 인증 유형을 검색했습니다.

##무엇을 확인합니까?

**참고:** 프로젝트에서 이전 인증 코드를 제대로 감지 하기 위해 프로젝트를 빌드할 수 있어야 합니다. 이 오류가 발생했고 프로젝트에 이전 인증 코드가 없는 경우 다시 작성한 다음 다시 시도합니다.

###프로젝트 형식

이 마법사는 프로젝트에 올바른 인증 논리를 삽입할 수 있도록 사용자가 개발 중인 프로젝트 형식을 확인합니다. 프로젝트의 `ApiController`에서 파생되는 컨트롤러가 있으면 프로젝트가 WebAPI 프로젝트로 간주됩니다. 프로젝트의 `MVC.Controller`에서 파생되는 컨트롤러만 있으면 프로젝트가 MVC 프로젝트로 간주됩니다. 다른 항목은 이 마법사에서 지원되지 않습니다. 현재는 WebForms 프로젝트가 지원되지 않습니다.

###호환 가능한 인증 코드

또한 이 마법사에서는 이전에 이 마법사로 구성되었거나 이 마법사와 호환되는 인증 설정이 있는지도 확인합니다. 모든 설정이 있는 경우 재진입 사례로 간주되고 마법사가 열릴 때 해당 설정이 표시됩니다. 설정이 일부만 있으면 오류 사례로 간주됩니다.

MVC 프로젝트에서 이 마법사는 이전에 마법사를 사용한 결과에 따라 다음과 같은 설정을 확인합니다.

	<add key="ida:ClientId" value="" />
	<add key="ida:Tenant" value="" />
	<add key="ida:AADInstance" value="" />
	<add key="ida:PostLogoutRedirectUri" value="" />

또한 이 마법사는 이전에 마법사를 사용한 결과에 따라 다음과 같은 Web API 프로젝트 설정을 확인합니다.

	<add key="ida:ClientId" value="" />
	<add key="ida:Tenant" value="" />
	<add key="ida:Audience" value="" />

###호환되지 않는 인증 코드

마지막으로, 이 마법사에서는 이전 버전의 Visual Studio를 사용하여 구성된 인증 코드의 버전을 감지하려고 합니다. 이 오류가 발생한 경우에는 프로젝트에 호환되지 않는 인증 코드가 있음을 의미합니다. 마법사에서는 이전 버전의 Visual Studio에서 다음과 같은 인증 유형을 감지합니다.

* Windows 인증 
* 개별 사용자 계정 
* 조직 계정 
 

MVC 프로젝트에서 Windows 인증을 감지하기 위해 마법사는 사용자의 **web.config** 파일에서 `authentication` 요소를 찾습니다.

<pre>
	&lt;configuration>
	    &lt;system.web>
	        <span style="background-color: yellow">&lt;authentication mode="Windows" /></span>
	    &lt;/system.web>
	&lt;/configuration>
</pre>

Web API 프로젝트에서 Windows 인증을 감지하기 위해 마법사는 사용자 프로젝트의 **.csproj** 파일에서 `IISExpressWindowsAuthentication` 요소를 찾습니다.

<pre>
	&lt;Project>
	    &lt;PropertyGroup>
	        <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication>enabled&lt;/IISExpressWindowsAuthentication></span>
	    &lt;/PropertyGroup>
	&lt;/Project>
</pre>

개별 사용자 계정 인증을 감지하기 위해 마법사는 사용자의 **Packages.config** 파일에서 패키지 요소를 찾습니다.

<pre>
	&lt;packages>
	    <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
	&lt;/packages>
</pre>

조직 계정 인증의 이전 양식을 감지하기 위해 마법사는 **web.config**에서 다음 요소를 찾습니다.

<pre>
	&lt;configuration>
	    &lt;appSettings>
	        <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /></span>
	    &lt;/appSettings>
	&lt;/configuration>
</pre>

인증 유형을 변경하려면 호환되지 않는 인증 유형을 제거하고 마법사를 다시 실행하세요.

자세한 내용은 [Azure AD의 인증 시나리오](active-directory-authentication-scenarios.md)를 참조하세요.

<!---HONumber=AcomDC_1217_2015-->