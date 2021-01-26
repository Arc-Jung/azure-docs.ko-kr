---
title: Azure Media Player 전체 설정
description: Azure Media Player를 설정 하는 방법을 알아봅니다.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: how-to
ms.date: 04/20/2020
ms.custom: devx-track-js
ms.openlocfilehash: 13abe333bcf3f67ea1a1ba823c693deaa60bc723
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98788810"
---
# <a name="azure-media-player-full-setup"></a>Azure Media Player 전체 설정 #

Azure Media Player는 설정하기 쉽습니다. Azure Media Services 계정에서 바로 미디어 콘텐츠의 기본 재생을 가져오는 데 몇 분 밖에 걸리지 않습니다. [샘플](https://github.com/Azure-Samples/azure-media-player-samples) 은 릴리스의 samples 디렉터리에도 제공 됩니다.

<!--//aka.ms/ampembed?url=https%3A%2F%2Fxpouyatdemo-euwe.streaming.media.azure.net%2Fc9b6ac82-c187-4882-a3d3-1a67204ac58e%2Fconnect2017-v3.ism%2Fmanifest-->

AMS 비디오의 예는 다음과 같습니다.

> [!VIDEO https://aka.ms/ampembed?url=https%3A%2F%2Fxpouyatdemo-euwe.streaming.media.azure.net%2Fc9b6ac82-c187-4882-a3d3-1a67204ac58e%2Fconnect2017-v3.ism%2Fmanifest]

## <a name="step-1-include-the-javascript-and-css-files-in-the-head-of-your-page"></a>1 단계: 페이지의 맨 위에 JavaScript 및 CSS 파일 포함 ##

Azure Media Player를 사용 하 여 CDN 호스팅된 버전에서 스크립트에 액세스할 수 있습니다. 이제는 대신 최종 본문 태그 앞에 JavaScript를 배치 하는 것이 좋지만, `<body>` `<head>` Azure Meia 플레이어에는 ' HTML5 shiv '가 포함 되어 있습니다 .이는 이전 IE 버전의 head에 있어야 비디오 태그를 유효한 요소로 지정할 수 있습니다.

> [!NOTE]
> [Modernizr](https://modernizr.com/) 와 같은 HTML5 shiv를 이미 사용 하 고 있는 경우 어디서 든 Azure Media Player JavaScript를 포함할 수 있습니다. 그러나 Modernizr의 버전에 비디오 용 shiv가 포함 되어 있는지 확인 합니다.

### <a name="cdn-version"></a>CDN 버전 ###

```html
    <link href="//amp.azure.net/libs/amp/latest/skins/amp-default/azuremediaplayer.min.css" rel="stylesheet">
    <script src= "//amp.azure.net/libs/amp/latest/azuremediaplayer.min.js"></script>
```

> [!IMPORTANT]
> 주문형으로 변경될 수 있으므로 프로덕션에 `latest` 버전을 사용하지 **않아야** 합니다. `latest`Azure Media Player 버전으로 대체 합니다. 예를 들어 `latest`를 `2.1.1`로 바꿉니다. [여기에서](azure-media-player-changelog.md) Azure Media Player 버전을 쿼리할 수 있습니다.

> [!NOTE]
> 릴리스 이후에는 `1.2.0` 더 이상 대체 techs 위치를 포함 하는 데 필요 하지 않습니다 (azuremediaplayer.min.js 파일의 상대 경로에서 위치가 자동으로 선택 됨). 위의 스크립트 뒤에에 다음 스크립트를 추가 하 여 대체 (fallback) techs의 위치를 수정할 수 있습니다 `<head>` .

> [!NOTE]
> Flash 및 Silverlight 플러그 인의 특성 때문에 swf 및 xap 파일은 중요 한 정보 또는 데이터 없이 도메인에서 호스팅되어야 합니다 .이는 Azure CDN 호스팅된 버전을 사용 하 여 자동으로 처리 됩니다.

```javascript
    <script>
      amp.options.flashSS.swf = "//amp.azure.net/libs/amp/latest/techs/StrobeMediaPlayback.2.0.swf"
      amp.options.silverlightSS.xap = "//amp.azure.net/libs/amp/latest/techs/SmoothStreamingPlayer.xap"
    </script>
```

## <a name="step-2-add-an-html5-video-tag-to-your-page"></a>2 단계: 페이지에 HTML5 비디오 태그 추가 ##

Azure Media Player를 사용 하면 HTML5 video 태그를 사용 하 여 비디오를 포함할 수 있습니다. 그러면 Azure Media Player은 태그를 읽고 HTML5 비디오를 지 원하는 것 뿐만 아니라 모든 브라우저에서 작동 하도록 합니다. 기본 태그 외에도 Azure Media Player 몇 가지 추가 부분이 필요 합니다.

1. 의 `<data-setup>` 특성은 `<video>` 페이지가 준비 될 때 비디오를 자동으로 설정 하 고 특성에서 JSON 형식으로 읽을 수 있도록 Azure Media Player에 지시 합니다.
1. `id`특성: 동일한 페이지의 모든 비디오에 대해 고유한 특성을 사용 해야 합니다.
1. 특성에는 `class` 다음과 같은 두 개의 클래스가 포함 됩니다.
    - `azuremediaplayer` Azure Media Player UI 기능에 필요한 스타일을 적용 합니다.
    - `amp-default-skin` HTML5 컨트롤에 기본 스킨을 적용 합니다.
1. 에는 `<source>` 두 개의 필수 특성이 포함 되어 있습니다.
    - `src` 특성에는 *. p s */매니페스트* 파일이 추가 Azure Media Services Azure Media Player 자동으로 플레이어에 게 대시, 부드러운 및 HLS에 대 한 url이 추가 됩니다.
    - `type` attribute는 스트림의 필수 MIME 형식입니다. *".Manifest/manifest"* 와 연결 된 MIME 형식은 *"application/vnd + xml"입니다.*
1. 의 *선택적* `<data-setup>` 특성은 `<source>` 암호화 유형 (AES 또는 PlayReady, Widevine 또는) 및 토큰을 포함 하 여 Azure Media Services에서 스트림에 대 한 고유한 배달 정책이 있는 경우 Azure Media Player에 게 알립니다.

HTML5 비디오와 동일 하 게 특성, 설정, 원본 및 트랙을 포함/제외 합니다.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" autoplay controls width="640" height="400" poster="poster.jpg" data-setup='{"techOrder": ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"], "nativeControlsForTouch": false}'>
        <source src="http://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest" type="application/vnd.ms-sstr+xml" />
        <p class="amp-no-js">
            To view this video please enable JavaScript, and consider upgrading to a web browser that supports HTML5 video
        </p>
    </video>
```

기본적으로 긴 재생 단추는 왼쪽 위 모서리에 위치 하므로 포스터의 흥미로운 부분은 다루지 않습니다. 크게 재생 단추를 가운데에 맞추려면 요소에 추가를 추가 `amp-big-play-centered` `class` `<video>` 합니다.

### <a name="alternative-setup-for-dynamically-loaded-html"></a>동적으로 로드 된 HTML에 대 한 대체 설정 ###

웹 페이지 또는 응용 프로그램이 비디오 태그를 동적으로 로드 하 여 (ajax, appendChild 등) 페이지가 로드 될 때 존재 하지 않도록 하려면 데이터 설치 특성에 의존 하는 대신 플레이어를 수동으로 설정 해야 합니다. 이렇게 하려면 먼저 태그에서 데이터 설치 특성을 제거 하 여 플레이어가 초기화 될 때 혼동 하지 않도록 합니다. 그런 다음 JavaScript가 로드 된 Azure Media Player 후, 그리고 video 태그가 DOM에 로드 된 후에 다음 JavaScript를 실행 합니다.

```javascript
    var myPlayer = amp('vid1', { /* Options */
            techOrder: ["azureHtml5JS", "flashSS", "html5FairPlayHLS","silverlightSS", "html5"],
            "nativeControlsForTouch": false,
            autoplay: false,
            controls: true,
            width: "640",
            height: "400",
            poster: ""
        }, function() {
              console.log('Good to go!');
               // add an event listener
              this.addEventListener('ended', function() {
                console.log('Finished!');
            }
          }
    );
    myPlayer.src([{
        src: "http://samplescdn.origin.mediaservices.windows.net/e0e820ec-f6a2-4ea2-afe3-1eed4e06ab2c/AzureMediaServices_Overview.ism/manifest",
        type: "application/vnd.ms-sstr+xml"
    }]);
```

함수의 첫 번째 인수는 `amp` 비디오 태그의 ID입니다. 사용자 고유의으로 바꿉니다.

두 번째 인수는 옵션 개체입니다. 데이터 설정 특성을 사용할 수 있는 것과 같은 추가 옵션을 설정할 수 있습니다.

세 번째 인수는 ' ready ' 콜백입니다. Azure Media Player 초기화 되 면이 함수를 호출 합니다. 준비 된 콜백에서 ' this ' 개체는 플레이어 인스턴스를 참조 합니다.

요소 ID를 사용 하는 대신 요소 자체에 참조를 전달할 수도 있습니다.

```javascript

    amp(document.getElementById('example_video_1'), {/*Options*/}, function() {
        //This is functionally the same as the previous example.
    });
    myPlayer.src([{ src: "//example/path/to/myVideo.ism/manifest", type: "application/vnd.ms-sstr+xml"]);
```

## <a name="next-steps"></a>다음 단계 ##

- [Azure Media Player 빠른 시작](azure-media-player-quickstart.md)