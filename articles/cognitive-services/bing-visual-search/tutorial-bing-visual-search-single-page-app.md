---
title: " 단일 페이지 웹앱 빌드 - Bing Visual Search"
titleSuffix: Azure Cognitive Services
description: Bing Visual Search API를 단일 페이지 웹 애플리케이션에 통합하는 방법을 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: b1ca88cd654b7bf373bee60b1e9b7a2e7a129fa2
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499036"
---
# <a name="tutorial-create-a-visual-search-single-page-web-app"></a>자습서: Visual Search 단일 페이지 웹앱 만들기

> [!WARNING]
> Bing Search API는 Cognitive Services에서 Bing Search Services로 이동합니다. **2020년 10월 30일** 부터 Bing Search의 모든 새 인스턴스는 [여기](/bing/search-apis/bing-web-search/create-bing-search-service-resource)에 설명된 프로세스에 따라 프로비저닝되어야 합니다.
> Cognitive Services를 사용하여 프로비저닝된 Bing Search API는 향후 3년 동안 또는 기업계약이 종료될 때까지(둘 중 먼저 도래할 때까지) 지원됩니다.
> 마이그레이션 지침은 [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource)를 참조하세요.

Bing Visual Search API는 이미지에 대한 인사이트를 반환합니다. 이미지를 업로드하거나 이미지에 대한 URL을 제공할 수 있습니다. 인사이트는 시각적으로 비슷한 이미지, 쇼핑 소스, 이미지가 포함된 웹 페이지 등입니다. Bing Visual Search API가 반환하는 인사이트는 Bing.com/이미지에 표시되는 것과 유사합니다.

이 자습서에서는 Bing Image Search API용 단일 페이지 웹앱을 확장하는 방법에 대해 설명합니다. 해당 자습서를 보거나 여기에 사용되는 소스 코드를 가져오려면 [자습서: Bing Image Search API용 단일 페이지 앱 만들기](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)를 참조하세요.

이 애플리케이션에 대한 전체 소스 코드(Bing Visual Search API를 사용하도록 확장한 후)는 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html)에서 사용할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Bing Visual Search API 호출 및 응답 처리

Bing Image Search 자습서를 편집하고 `<script>` 요소의 끝(및 닫는 `</script>` 태그의 앞)에 다음 코드를 추가합니다. 다음 코드는 API의 시각적 검색 응답을 처리하고 결과를 반복하여 표시합니다.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

다음 코드는 `handleVisualSearchResponse()`를 호출하는 이벤트 수신기를 사용하여 검색 요청을 API에 보냅니다.

```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;

    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>인사이트 토큰 캡처

다음 코드를 `searchItemsRenderer` 개체에 추가합니다. 이 코드는 클릭할 때 `bingVisualSearch` 함수를 호출하는 **유사 항목 찾기** 링크를 추가합니다. 함수는 인수로 `imageInsightsToken`을 받습니다.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>유사한 이미지 표시

다음 HTML 코드를 601줄에 추가합니다. 이 태그 코드는 Bing Visual Search API 호출의 결과를 표시하는 요소를 추가합니다.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

모든 새 JavaScript 코드 및 HTML 요소가 있으면 검색 결과는 **유사 항목 찾기** 링크로 표시됩니다. 링크를 클릭하면 선택한 것과 비슷한 이미지로 **유사 항목** 섹션을 채웁니다. **유사 항목** 섹션을 확장하여 이미지를 표시해야 할 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [자습서: Bing Visual Search SDK for C#을 사용하여 이미지 자르기](tutorial-visual-search-crop-area-results.md)