---
title: 웹 Api를 호출 하는 데스크톱 앱을 프로덕션으로 이동-Microsoft id 플랫폼 | Microsoft
description: 웹 Api를 호출 하는 데스크톱 앱을 프로덕션으로 이동 하는 방법을 알아봅니다.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 325f95f2830ef021a4ac79de48695dda570f7817
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97629785"
---
# <a name="desktop-app-that-calls-web-apis-move-to-production"></a>웹 Api를 호출 하는 데스크톱 앱: 프로덕션으로 이동

이 문서에서는 웹 Api를 호출 하는 데스크톱 앱을 프로덕션으로 이동 하는 방법에 대해 알아봅니다.

## <a name="handle-errors-in-desktop-applications"></a>데스크톱 응용 프로그램의 오류 처리

다른 흐름에서는 코드 조각에 표시 된 것 처럼 자동 흐름에 대 한 오류를 처리 하는 방법을 배웠습니다. 또한 증분 동의 및 조건부 액세스와 같이 상호 작용이 필요한 경우도 있습니다.

## <a name="have-the-user-consent-upfront-for-several-resources"></a>사용자에 게 여러 리소스에 대 한 사전 승인이 필요 합니다.

> [!NOTE]
> 여러 리소스에 대 한 동의를 얻는 것은 Microsoft id 플랫폼에서 작동 하지만 Azure Active Directory (Azure AD) B2C의 경우에는 작동 하지 않습니다. Azure AD B2C은 사용자 동의가 아닌 관리자 동의만 지원 합니다.

V2.0 (Microsoft identity platform) 끝점을 사용 하 여 한 번에 여러 리소스에 대 한 토큰을 가져올 수 없습니다. `scopes`매개 변수는 단일 리소스에 대 한 범위만 포함할 수 있습니다. 사용자가 매개 변수를 사용 하 여 여러 리소스를 미리 동의 수 있습니다 `extraScopesToConsent` .

예를 들어, 두 개의 범위가 각각 포함 된 두 개의 리소스가 있을 수 있습니다.

- `https://mytenant.onmicrosoft.com/customerapi` 범위 `customer.read` 및 `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` 범위 `vendor.read` 및 `vendor.write`

이 예제에서는 `.WithAdditionalPromptToConsent` 매개 변수가 있는 한정자를 사용 합니다 `extraScopesToConsent` .

예:

### <a name="in-msalnet"></a>MSAL.NET에서

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopesToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>iOS 및 macOS용 MSAL에서

Objective-C:

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.onmicrosoft.com/customerapi/customer.read",
                                @"https://mytenant.onmicrosoft.com/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                              @"https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift:

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

이 호출은 첫 번째 web API에 대 한 액세스 토큰을 가져옵니다.

두 번째 web API를 호출 해야 하는 경우 api를 호출 `AcquireTokenSilent` 합니다.

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsent-each-time-the-app-runs"></a>앱을 실행할 때마다 Microsoft 개인 계정에 다시 동의 해야 함

Microsoft 개인 계정 사용자의 경우 권한 부여를 위해 각 native client (데스크톱 또는 모바일 앱) 호출에 대 한 동의를 다시 확인 하는 것은 의도 된 동작입니다. Native client id는 본질적으로 안전 하지 않습니다 .이는 기밀 클라이언트 응용 프로그램 id와 반대입니다. 기밀 클라이언트 응용 프로그램은 Microsoft Id 플랫폼과 암호를 교환 하 여 해당 id를 증명 합니다. Microsoft id 플랫폼은 응용 프로그램에 권한이 부여 될 때마다 사용자에 게 동의 하 라는 메시지를 표시 하 여이 입력할 소비자 서비스를 완화 하도록 선택 했습니다.

## <a name="next-steps"></a>다음 단계

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
