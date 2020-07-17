---
title: Bing Web Search JavaScript 클라이언트 라이브러리 빠른 시작
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/05/2020
ms.author: aahi
ms.openlocfilehash: 0380dc8d2ff34cf9eecaad063a305491a357ca29
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78926067"
---
Bing Web Search 클라이언트 라이브러리를 사용하면 Bing Web Search를 Node.js 애플리케이션에 쉽게 통합할 수 있습니다. 이 빠른 시작에서는 클라이언트를 인스턴스화하고, 요청을 보내며, 응답을 출력하는 방법에 대해 알아봅니다.

지금 코드를 보시겠나요? [JavaScript용 Bing Search 클라이언트 라이브러리](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/tree/master/Samples) 샘플은 GitHub에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항
이 빠른 시작을 실행하기 전에 필요한 몇 가지 조건은 다음과 같습니다.

* [Node.js 6](https://nodejs.org/en/download/) 이상
* 구독 키  

[!INCLUDE [bing-web-search-quickstart-signup](~/includes/bing-web-search-quickstart-signup.md)]


## <a name="set-up-your-development-environment"></a>개발 환경 설정

Node.js 프로젝트에 대한 개발 환경을 설정해 보겠습니다.

1. 프로젝트에 대한 새 디렉터리를 만듭니다.

    ```console
    mkdir YOUR_PROJECT
    ```

1. 새 패키지 파일을 만듭니다.

    ```console
    cd YOUR_PROJECT
    npm init
    ```

1. 이제 일부 Azure 모듈을 설치하고 `package.json`에 추가해 보겠습니다.

    ```console
    npm install --save azure-cognitiveservices-websearch
    npm install --save ms-rest-azure
    ```

## <a name="create-a-project-and-declare-required-modules"></a>프로젝트 만들기 및 필요한 모듈 선언

`package.json`과 같은 디렉터리에서 즐겨찾는 IDE 또는 편집기를 사용하여 새 Node.js 프로젝트를 만듭니다. 예: `sample.js`

다음으로, 이 코드를 프로젝트에 복사합니다. 이전 섹션에서 설치한 모듈을 로드합니다.

```javascript
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>클라이언트 인스턴스화

이 코드는 클라이언트를 인스턴스화하고 `azure-cognitiveservices-websearch` 모듈을 사용합니다. 계속하기 전에 Azure 계정에 유효한 구독 키를 입력했는지 확인합니다.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>요청 및 결과 출력

클라이언트를 사용하여 Bing Web Search에 검색 쿼리를 보냅니다. 응답에 `properties` 배열의 항목에 대한 결과가 포함되어 있으면 콘솔에 `result.value`가 출력됩니다.

```javascript
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})
```

## <a name="run-the-program"></a>프로그램 실행

마지막 단계는 프로그램을 실행하는 것입니다!

## <a name="clean-up-resources"></a>리소스 정리

이 프로젝트가 완료되면 프로그램의 코드에서 구독 키를 제거해야 합니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Cognitive Services Node.js SDK 샘플](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>참고 항목

* [Azure Node SDK 참조](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-websearch/)
