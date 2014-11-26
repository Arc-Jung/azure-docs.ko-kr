<properties linkid="develop-mobile-how-to-guides-register-windows-store-app-server-auth" urlDisplayName="Shared Access Signature Part 1" pageTitle="Register your Windows Store app package for Microsoft authentication" metaKeywords="" description="Learn how to register your Windows Store app for Microsoft authentication in your Azure Mobile Services application" metaCanonical="" services="mobile-services" documentationCenter="Mobile" title="Register your Windows Store app package for Microsoft authentication" authors="glenga" solutions="" manager="" editor="" />

<tags ms.service="mobile-services" ms.workload="mobile" ms.tgt_pltfrm="mobile-multiple" ms.devlang="multiple" ms.topic="article" ms.date="01/01/1900" ms.author="glenga" />

# Microsoft 인증을 위해 Windows 스토어 앱 패키지 등록

Azure 모바일 서비스에서는 클라이언트 기반 인증과 서버 기반 인증 방법을 모두 지원합니다. 서버 기반 인증에서는 Microsoft 계정과 같은 ID 공급자를 사용합니다. 모바일 서비스를 통해 앱을 등록하지 않고 서버 기반 인증을 통해 Microsoft 계정을 사용할 경우 인증을 등록할 때마다 자격 증명을 제공하라는 메시지가 표시됩니다. 앱을 등록하면 Microsoft 계정 로그인 자격 증명이 캐시되어 인증에 사용될 수 있으므로 사용자에게 자격 증명을 다시 제공하라는 메시지가 표시되지 않습니다. 이 항목에서는 Azure 모바일 서비스를 사용하여 인증할 때의 향상된 Microsoft 계정 로그인 환경을 위해 Windows 스토어 앱 패키지를 등록하는 방법을 보여 줍니다.

> [WACOM.NOTE]Visual Studio 2013을 사용하면 Windows 스토어 앱 패키지를 모바일 서비스에 손쉽게 등록할 수 있습니다. 자세한 내용은 [빠른 시작: 모바일 서비스에 대한 푸시 알림 추가][빠른 시작: 모바일 서비스에 대한 푸시 알림 추가](영문)를 참조하십시오.

클라이언트 관리 인증을 사용하면 Live Connect를 사용하여 Windows 장치에서 Single Sign-On 환경을 제공할 수 있습니다. Live Connect API를 사용하는 경우에는 이 항목의 단계를 완료할 필요가 없습니다. 자세한 내용은 [Live Connect Single Sign-On을 사용하여 Windows 스토어 앱 인증][Live Connect Single Sign-On을 사용하여 Windows 스토어 앱 인증]을 참조하십시오.

> [WACOM.NOTE]Live Connect를 사용한 클라이언트 관리 인증은 아직 .NET 백 엔드 모바일 서비스에서 지원되지 않습니다.

[WACOM.INCLUDE [mobile-services-register-windows-store-app](../includes/mobile-services-register-windows-store-app.md)]

앱 패키지를 등록한 후 [LoginAsync][LoginAsync] 메서드를 호출할 때 *useSingleSignOn*에 대해 **true** 값을 제공해야 합니다. 그러면 사용자가 Microsoft 계정을 사용할 때 향상된 로그인 환경이 제공됩니다.

 
 


  [빠른 시작: 모바일 서비스에 대한 푸시 알림 추가]: http://go.microsoft.com/fwlink/p/?LinkId=309101
  [Live Connect Single Sign-On을 사용하여 Windows 스토어 앱 인증]: /ko-kr/develop/mobile/tutorials/single-sign-on-windows-8-dotnet
  [LoginAsync]: http://go.microsoft.com/fwlink/p/?LinkId=311594
