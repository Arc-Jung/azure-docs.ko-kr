---
title: Bing Entity Search JavaScript 클라이언트 라이브러리 빠른 시작
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: f5a3b48fd6be48d468b66009a77de100962be10d
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405072"
---
JavaScript용 Bing Entity Search 클라이언트 라이브러리를 통해 엔터티 검색을 시작하려면 이 빠른 시작을 사용하세요. Bing Entity Search에는 대부분의 프로그래밍 언어와 호환되는 REST API가 있는 반면, 클라이언트 라이브러리를 사용하면 서비스를 애플리케이션에 쉽게 통합할 수 있습니다. 이 샘플의 소스 코드는 [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js)에서 확인할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

* 최신 버전의 [Node.js](https://nodejs.org/en/download/).

* [Node.js용 Bing Entity Search SDK](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

Bing Entity Search SDK를 설치하려면:

1. 개발 환경에서 `npm install ms-rest-azure` 명령을 실행합니다.
2. 개발 환경에서 `npm install @azure/cognitiveservices-entitysearch` 명령을 실행합니다.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>애플리케이션 만들기 및 초기화

1. 선호하는 IDE 또는 편집기에서 새 JavaScript 파일을 만들고, 다음 요구 사항을 추가합니다.

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. 구독 키를 사용하여 `CognitiveServicesCredentials` 인스턴스를 만듭니다. 그 후 해당 인스턴스를 사용하여 검색 클라이언트의 인스턴스를 만듭니다.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>요청을 보내고 응답 수신

1. `entitiesOperations.search()`를 사용하여 엔터티 검색 요청을 보냅니다. 응답을 받은 후 `queryContext`, 반환된 결과 수, 첫 번째 결과에 대한 설명을 인쇄합니다.

    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [단일 페이지 웹앱 만들기](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Entity Search API란?](../../overview.md)