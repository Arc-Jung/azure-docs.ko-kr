---
title: Bing Spell Check API에 요청 보내기
titleSuffix: Azure Cognitive Services
description: Bing Spell Check 모드, 설정 및 API와 관련된 기타 정보에 대해 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: 761cc3908b677b129e85be442b7a593a38a367f9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2021
ms.locfileid: "96341566"
---
# <a name="sending-requests-to-the-bing-spell-check-api"></a>Bing Spell Check API에 요청 보내기

> [!WARNING]
> Bing Search API는 Cognitive Services에서 Bing Search Services로 이동합니다. **2020년 10월 30일** 부터 Bing Search의 모든 새 인스턴스는 [여기](/bing/search-apis/bing-web-search/create-bing-search-service-resource)에 설명된 프로세스에 따라 프로비저닝되어야 합니다.
> Cognitive Services를 사용하여 프로비저닝된 Bing Search API는 향후 3년 동안 또는 기업계약이 종료될 때까지(둘 중 먼저 도래할 때까지) 지원됩니다.
> 마이그레이션 지침은 [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource)를 참조하세요.

텍스트 문자열의 맞춤법 및 문법 오류를 검사하려면 다음 엔드포인트로 GET 요청을 보냅니다.  

```
https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
```  
  
요청은 HTTPS 프로토콜을 사용해야 합니다.

모든 요청은 서버에서 시작되는 것이 좋습니다. 클라이언트 애플리케이션의 일부로 키를 배포하면 제3자가 나쁜 목적을 갖고 애플리케이션에 액세스할 가능성이 높아집니다. 서버는 향후 API 버전에 대한 단일 업그레이드 지점도 제공합니다.

요청에서는 검증할 텍스트 문자열을 포함하고 있는 [텍스트](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#text) 쿼리 매개 변수를 지정해야 합니다. 선택 사항이지만, 요청에서 결과를 가져올 지역/국가를 식별하는 [mkt](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#mkt) 쿼리 매개 변수도 지정해야 합니다. `mode` 같은 선택적 쿼리 매개 변수 목록은 [쿼리 매개 변수](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#query-parameters)를 참조하세요. 모든 쿼리 매개 변수 값은 URL로 인코드되어야 합니다.  
  
요청에서 [Ocp-Apim-Subscription-Key](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#subscriptionkey) 헤더를 지정해야 합니다. 선택 사항이지만, 다음 헤더도 지정하는 것이 좋습니다. 이러한 헤더는 Bing Spell Check API가 보다 정확한 결과를 반환하는 데 도움이 됩니다.  
  
-   [User-Agent](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#location)  

모든 요청 및 응답 헤더 목록은 [헤더](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#headers)를 참조하세요.

JavaScript를 사용하여 Bing Spell Check API를 호출하면, 브라우저에 내장된 보안 기능으로 인해 이러한 헤더의 값에 액세스하지 못할 수 있습니다

이 문제를 해결하려면 CORS 프록시를 통해 Bing Spell Check API 요청을 수행할 수 있습니다. 이러한 프록시의 응답에는 응답 헤더를 필터링하고 JavaScript에서 응답 헤더를 사용할 수 있게 해주는 `Access-Control-Expose-Headers` 헤더가 포함됩니다.

[자습서 앱](../tutorials/spellcheck.md)이 선택적 클라이언트 헤더에 액세스할 수 있도록 CORS 프록시를 쉽게 설치할 수 있습니다. 먼저 [Node.js가 없는 경우 설치](https://nodejs.org/en/download/)합니다. 그런 다음, 명령 프롬프트에서 다음 명령을 입력합니다.

```console
npm install -g cors-proxy-server
```

다음으로, HTML 파일에서 Bing Spell Check API 엔드포인트를 다음으로 변경합니다.
`http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/`

마지막으로 다음 명령을 사용하여 CORS 프록시를 시작합니다.

```console
cors-proxy-server
```

자습서 앱을 사용하는 동안에는 명령 창을 열어 두세요. 창을 닫으면 프록시가 중지됩니다. 검색 결과 아래의 확장 가능한 HTTP 헤더 섹션에서 여러 `X-MSEdge-ClientID` 헤더를 볼 수 있으며 요청마다 동일한지 확인합니다.

## <a name="example-api-request"></a>API 요청 예

다음은 모든 제안된 쿼리 매개 변수 및 헤더를 포함하는 요청을 보여줍니다. Bing API 중 하나를 처음 호출하는 경우 클라이언트 ID 헤더를 포함하면 안 됩니다. 전에 Bing API를 호출하고 Bing이 사용자 및 디바이스 조합에 대한 클라이언트 ID를 반환한 경우만 클라이언트 ID를 포함합니다. 
  
```http
GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

다음은 이전 요청에 대한 응답을 보여줍니다. 또한 이 예제는 Bing 관련 응답 헤더를 보여 줍니다.

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  

## <a name="next-steps"></a>다음 단계

- [Bing Spell Check API란?](../overview.md)
- [Bing Spell Check API v7 참조](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)