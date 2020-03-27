---
title: '빠른 시작: Python을 사용하여 Bing Custom Search 엔드포인트 호출 | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작을 사용하여 Python을 사용하여 Bing Custom Search 인스턴스의 검색 결과를 요청할 수 있습니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: a601b309d18e489f6b631cb26e5f3e13ef790b42
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80238842"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>빠른 시작: Python을 사용하여 Bing Custom Search 엔드포인트 호출

이 빠른 시작을 사용하여 Bing Custom Search 인스턴스의 검색 결과를 요청할 수 있습니다. 이 애플리케이션은 Python으로 작성되지만 Bing Custom Search API는 대부분의 프로그래밍 언어와 호환되는 RESTful 웹 서비스입니다. 이 샘플의 소스 코드는 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py)에 제공됩니다.

## <a name="prerequisites"></a>사전 요구 사항

- Bing Custom Search 인스턴스 [빠른 시작: 처음으로 Bing Custom Search 인스턴스 만들기](quick-start.md)에서 자세한 내용을 참조하세요.
- [Python](https://www.python.org/) 2.x 또는 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>애플리케이션 만들기 및 초기화

1. 선호하는 IDE 또는 편집기에서 새 Python 파일을 만들고, 다음 import 문을 추가합니다. 구독 키, 사용자 지정 구성 ID 및 검색어에 대한 변수를 만듭니다. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>검색 요청 보내고 받기 

1. 검색어를 `q=` 쿼리 매개 변수에 추가하고 검색 인스턴스의 사용자 지정 구성 ID를 `customconfig=`에 추가하여 요청 URL을 구성합니다. 매개 변수를 `&` 문자로 분리합니다. 아래의 글로벌 엔드포인트를 사용하거나 리소스의 Azure Portal에 표시되는 [사용자 지정 하위 도메인](../../cognitive-services/cognitive-services-custom-subdomains.md) 엔드포인트를 사용할 수 있습니다.

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Bing Custom Search 인스턴스에 요청을 보내고 반환된 검색 결과를 출력합니다.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Custom Search 웹앱 빌드](./tutorials/custom-search-web-page.md)
