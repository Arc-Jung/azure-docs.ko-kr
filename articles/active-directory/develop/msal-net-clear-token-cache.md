---
title: 토큰 캐시 지우기 (MSAL.NET) | Microsoft
titleSuffix: Microsoft identity platform
description: Microsoft Authentication Library for .NET (MSAL.NET)을 사용 하 여 토큰 캐시를 지우는 방법에 대해 알아봅니다.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 3049a1213b8b92153fc0fce96b2dadace01a4ca8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98064762"
---
# <a name="clear-the-token-cache-using-msalnet"></a>MSAL.NET를 사용 하 여 토큰 캐시 지우기

Microsoft Authentication Library for .NET (MSAL.NET)을 사용 하 여 [액세스 토큰을 획득](msal-acquire-cache-tokens.md) 하면 토큰이 캐시 됩니다. 응용 프로그램에 토큰이 필요한 경우 먼저 메서드를 호출 하 여 `AcquireTokenSilent` 허용 되는 토큰이 캐시에 있는지 확인 해야 합니다. 

캐시를 지우면 캐시에서 계정이 제거 됩니다. 이 경우 브라우저에 있는 세션 쿠키는 제거되지 않습니다.  다음 예제에서는 공용 클라이언트 응용 프로그램을 인스턴스화하고 응용 프로그램에 대 한 계정을 가져오고 계정을 제거 합니다.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

토큰 획득 및 캐싱에 대 한 자세한 내용은 [액세스 토큰 가져오기](msal-acquire-cache-tokens.md)를 참조 하세요.
