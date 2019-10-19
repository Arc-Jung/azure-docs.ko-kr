---
title: Azure Maps에서 웹 맵 컨트롤 시작 Microsoft Docs
description: Azure Maps 맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용 하는 방법에 대해 알아봅니다.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 4fd213a6550d045fe4bea280449ed228ba9eda35
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72550279"
---
# <a name="use-the-azure-maps-map-control"></a>Azure Maps 맵 컨트롤 사용

맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용하면 맵 및 포함된 Azure Maps 기능을 웹 또는 모바일 애플리케이션에 렌더링할 수 있습니다.

## <a name="create-a-new-map-in-a-web-page"></a>웹 페이지에 새 맵 만들기

맵 컨트롤 클라이언트 쪽 Javascript 라이브러리를 사용하여 웹 페이지에 지도를 포함할 수 있습니다.

1. 새 HTML 파일 만들기

2. Azure Maps 웹 SDK에서 로드합니다. 두 가지 옵션 중 하나를 사용하여 수행할 수 있습니다.

    a. 파일의 `<head>` 요소에 있는 스타일시트 및 스크립트 참조에 URL 엔드포인트를 추가하여 Azure Maps 웹 SDK의 글로벌 호스팅 CDN 버전을 사용합니다.

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

    b. 아니면 [azure-maps-control](https://www.npmjs.com/package/azure-maps-control) NPM 패키지를 사용하여 로컬에서 Azure Maps 웹 SDK 소스 코드를 로드하여 앱으로 호스트합니다. 이 패키지에는 TypeScript 정의도 포함됩니다.

    > npm install azure-maps-control

    그런 다음, Azure Maps 스타일시트에 대한 참조와 스크립트 소스 참조를 파일의 `<head>` 요소에 추가합니다.

    ```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/atlas.min.css" type="text/css"> 
    <script src="node_modules/azure-maps-control/dist/atlas.min.js"></script>
    ```

3. 맵이 페이지 본문 전체를 채우도록 렌더링하려면 `<head>` 요소에 다음 `<style>` 요소를 추가합니다.

    ```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
    ```

4. 페이지 본문에 `<div>` 요소를 추가하고 **myMap**이라는 `id`를 부여합니다.

    ```HTML
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. 맵 컨트롤을 초기화하려면 HTML 본문에 새 섹션을 정의하고 스크립트를 만듭니다. @No__t_4 클래스의 인스턴스를 만들 때 맵 `<div>`의 `id` 또는 `HTMLElement` (예: `document.getElementById('myMap')`)를 첫 번째 매개 변수로 전달 합니다. 자신의 Azure Maps 계정 키나 AAD(Azure Active Directory) 자격 증명으로 [인증 옵션](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions)을 사용하여 맵을 인증합니다. 계정을 만들거나 키를 찾아야 하는 경우 [Azure Maps 계정과 키를 관리하는 방법](how-to-manage-account-keys.md)을 참조하세요. **언어** 옵션은 맵 레이블 및 컨트롤에 사용할 언어를 지정합니다. 지원되는 언어에 대한 자세한 내용은 [지원되는 언어](supported-languages.md)를 참조하세요. 인증에 구독 키를 사용하는 경우입니다.

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

    인증에 AAD(Azure Active Directory)를 사용하는 경우입니다.

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: '<Your AAD Tenant Id>'
            }
        });
    </script>
    ```

    자세한 내용은 [Azure Maps를 사용 하 여 인증](azure-maps-authentication.md) 문서를 참조 하세요.

6. 선택적으로 다음 메타 태그 요소를 페이지 머리글에 추가하는 것이 도움이 될 수 있습니다.

    ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

7. 모든 HTML 파일을 함께 배치 하는 것은 다음 코드와 유사 해야 합니다.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. 웹 브라우저에서 파일을 열고 렌더링된 맵을 봅니다. 이 코드는 다음 코드와 같습니다.

    <iframe height="700" style="width: 100%;" scrolling="no" title="맵 컨트롤을 사용하는 방법" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true"><a href='https://codepen.io'>CodePen</a>에서 <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>지도 컨트롤을 사용</a> 하는 방법 Azure Maps (<a href='https://codepen.io/azuremaps'> @azuremaps</a>)을 참조 하세요.
    </iframe>

## <a name="localizing-the-map"></a>맵 지역화

Azure Maps은 지도의 언어 및 지역 보기를 설정 하는 두 가지 다른 방법을 제공 합니다. 첫 번째 옵션은 전역 `atlas` 네임 스페이스에이 정보를 추가 하는 것입니다. 그러면 앱의 모든 맵 컨트롤 인스턴스가 이러한 설정을 기본값으로 설정 합니다. 다음은 언어를 프랑스어 ("fr-fr")로 설정 하 고 국가별 보기를 "auto"로 설정 합니다.

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

두 번째 옵션은 다음과 같이 맵을 로드할 때 맵 옵션에이 정보를 전달 하는 것입니다.

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'aad',
        clientId: '<Your AAD Client Id>',
        aadAppId: '<Your AAD App Id>',
        aadTenant: '<Your AAD Tenant Id>'
    }
});
```

> [!Note]
> 웹 SDK를 사용 하면 다른 언어 및 지역 설정을 사용 하 여 동일한 페이지에 여러 개의 맵 인스턴스를 로드할 수 있습니다. 또한 지도의 `setStyle` 함수를 사용 하 여 맵을 로드 한 후에 이러한 설정을 업데이트할 수 있습니다. 

언어가 "fr-fr"로 설정 되 고 지역 보기가 "auto"로 설정 된 Azure Maps의 예는 다음과 같습니다.

![프랑스어로 레이블을 표시 하는 지도 이미지](./media/how-to-use-map-control/websdk-localization.png)

지원 되는 언어 및 지역 보기의 전체 목록은 [여기](supported-languages.md)에 설명 되어 있습니다.

## <a name="next-steps"></a>다음 단계

맵을 만들고 상호 작용하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [맵 만들기](map-create.md)

맵 스타일을 지정하는 방법을 알아봅니다.

> [!div class="nextstepaction"]
> [맵 스타일 선택](choose-map-style.md)

지도에 데이터를 더 추가 하려면:

> [!div class="nextstepaction"]
> [맵 만들기](map-create.md)

> [!div class="nextstepaction"]
> [코드 샘플](https://docs.microsoft.com/samples/browse/?products=azure-maps)