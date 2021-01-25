---
title: Web Api를 호출 하는 웹 API에 대 한 토큰을 가져옵니다. Microsoft
titleSuffix: Microsoft identity platform
description: 앱에 대 한 토큰을 획득 해야 하는 웹 Api를 호출 하는 web API를 빌드하는 방법에 대해 알아봅니다.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9f9758ec765ad34e5ef5d8b4d4e0a420a6701b6e
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98756388"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>웹 Api를 호출 하는 web API: 응용 프로그램에 대 한 토큰 획득

클라이언트 응용 프로그램 개체를 빌드한 후에는이 개체를 사용 하 여 web API를 호출 하는 데 사용할 수 있는 토큰을 가져옵니다.

## <a name="code-in-the-controller"></a>컨트롤러의 코드

### <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft. Identity* 는 Microsoft Graph 또는 다운스트림 웹 API를 호출 하기 위한 편리한 서비스를 제공 하는 확장 메서드를 추가 합니다. 이러한 메서드는 web api를 [호출 하는 WEB api](scenario-web-api-call-api-call-api.md)에 자세히 설명 되어 있습니다. Api를 호출 합니다. 이러한 도우미 메서드를 사용 하 여 토큰을 수동으로 가져올 필요가 없습니다.

그러나 토큰을 수동으로 획득 하려는 경우 다음 코드에서는 API 컨트롤러에서 이러한 작업을 *수행 하는 데 사용 하* 는 예제를 보여 줍니다. *Todolist* 이라는 다운스트림 API를 호출 합니다.
다운스트림 API를 호출 하는 토큰을 가져오려면 `ITokenAcquisition` 컨트롤러의 생성자 (또는 Blazor를 사용 하는 경우 페이지 생성자)에서 종속성 주입을 사용 하 여 서비스를 삽입 하 고, `GetAccessTokenForUserAsync` `GetAccessTokenForAppAsync` 디먼 시나리오의 경우 사용자 () 또는 응용 프로그램 자체 ()에 대 한 토큰을 가져오는 컨트롤러 작업에서 해당 서비스를 사용 합니다.

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

메서드에 대 한 자세한 `callTodoListService` 내용은 web api를  [호출 하는 web API: api 호출](scenario-web-api-call-api-call-api.md)을 참조 하세요.

### <a name="java"></a>[Java](#tab/java)

API 컨트롤러의 작업에서 호출 되는 코드의 예는 다음과 같습니다. 다운스트림 API Microsoft Graph를 호출 합니다.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

### <a name="python"></a>[Python](#tab/python)
 
Python web API를 사용 하려면 미들웨어를 사용 하 여 클라이언트에서 받은 전달자 토큰의 유효성을 검사 해야 합니다. Web API는 메서드를 호출 하 여 MSAL Python 라이브러리를 사용 하 여 다운스트림 API에 대 한 액세스 토큰을 가져올 수 있습니다 [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) .
 
다음은 `acquire_token_on_behalf_of` 메서드 및 Flask 프레임 워크를 사용 하 여 액세스 토큰을 획득 하는 코드의 예제입니다. 다운스트림 API-Azure 관리 구독 끝점을 호출 합니다.
 
```python
def get(self):
 
        _scopes = ["https://management.azure.com/user_impersonation"]
        _azure_management_subscriptions_uri = "https://management.azure.com/subscriptions?api-version=2020-01-01"
 
        current_access_token = request.headers.get("Authorization", None)
        
        #This example only uses the default memory token cache and should not be used for production
        msal_client = msal.ConfidentialClientApplication(
                client_id=os.environ.get("CLIENT_ID"),
                authority=os.environ.get("AUTHORITY"),
                client_credential=os.environ.get("CLIENT_SECRET"))
 
        #acquire token on behalf of the user that called this API
        arm_resource_access_token = msal_client.acquire_token_on_behalf_of(
            user_assertion=current_access_token.split(' ')[1],
            scopes=_scopes
        )
 
        headers = {'Authorization': arm_resource_access_token['token_type'] + ' ' + arm_resource_access_token['access_token']}
 
        subscriptions_list = req.get(_azure_management_subscriptions_uri), headers=headers).json()
 
        return jsonify(subscriptions_list)
```

## <a name="advanced-accessing-the-signed-in-users-token-cache-from-background-apps-apis-and-services"></a>고급 백그라운드 앱, Api 및 서비스에서 로그인 한 사용자의 토큰 캐시에 액세스

[!INCLUDE [advanced-token-caching](../../../includes/advanced-token-cache.md)]

---

## <a name="next-steps"></a>다음 단계

이 시나리오에서 다음 문서로 이동 하 여 API를 [호출](scenario-web-api-call-api-call-api.md)합니다.
