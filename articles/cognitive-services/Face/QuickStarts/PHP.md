---
title: '빠른 시작: REST API 및 PHP를 사용하여 이미지에서 얼굴 감지'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 PHP와 함께 Face API를 사용하여 이미지에서 얼굴을 감지합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.openlocfilehash: fb9539c8aa0477cbfbf11ce08635e4e9ccebb2ce
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977933"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-php"></a>빠른 시작: REST API 및 PHP를 사용하여 이미지에서 얼굴 감지

이 빠른 시작에서는 이미지에서 사람 얼굴을 감지하기 위해 PHP와 함께 Azure Face REST API를 사용합니다.

## <a name="prerequisites"></a>필수 조건

- Face API 구독 키. [Cognitive Services 사용해보기](https://azure.microsoft.com/try/cognitive-services/?api=face-api)에서 평가판 구독 키를 가져올 수 있습니다. 또는 [Cognitive Services 계정 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)의 지침에 따라 Face API 서비스를 구독하고 키를 가져옵니다.
- [Visual Studio Code](https://code.visualstudio.com/download) 같은 코드 편집기
- PHP [HTTP_Request2](https://pear.php.net/package/HTTP_Request2) 패키지
- PHP 지원 웹 브라우저입니다. 이를 설정하지 않은 경우 머신에 [XAMPP](https://www.apachefriends.org/)를 설치 및 설정하면 됩니다.

## <a name="initialize-the-html-file"></a>HTML 파일 초기화

*detectFaces.html*이라는 새 HTML 파일을 만들고 다음 코드를 추가합니다.

```html
<html>
    <head>
        <title>Face Detect Sample</title>
    </head>
    <body></body>
</html>
```

## <a name="write-the-php-script"></a>PHP 스크립트 작성

문서의 `body` 요소 내에 다음 코드를 추가합니다. 이 코드는 URL 필드, **얼굴 분석** 단추, 응답 창 및 이미지 표시 창이 있는 기본 사용자 인터페이스를 설정합니다.

```php
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// Replace <My Endpoint String> with the string in your endpoint URL.
$uriBase = 'https:/<My Endpoint String>.com/face/v1.0/';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg';

// This sample uses the PHP5 HTTP_Request2 package
// (https://pear.php.net/package/HTTP_Request2).
require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/detect');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'returnFaceId' => 'true',
    'returnFaceLandmarks' => 'false',
    'returnFaceAttributes' => 'age,gender,headPose,smile,facialHair,glasses,' .
        'emotion,hair,makeup,occlusion,accessories,blur,exposure,noise');
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
```

`subscriptionKey` 필드를 구독 키의 값으로 업데이트하고, 올바른 엔드포인트 문자열이 포함되도록 `uriBase` 문자열을 변경해야 합니다. `returnFaceAttributes` 필드는 검색할 얼굴 특성을 지정하며 원하는 용도에 맞게 이 문자열을 변경할 수 있습니다.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="run-the-script"></a>스크립트 실행

PHP 지원 웹 브라우저에서 파일을 엽니다. 다음과 유사한 얼굴 데이터의 JSON 문자열이 표시됩니다.

```json
[
    {
        "faceId": "e93e0db1-036e-4819-b5b6-4f39e0f73509",
        "faceRectangle": {
            "top": 621,
            "left": 616,
            "width": 195,
            "height": 195
        },
        "faceAttributes": {
            "smile": 0,
            "headPose": {
                "pitch": 0,
                "roll": 6.8,
                "yaw": 3.7
            },
            "gender": "male",
            "age": 37,
            "facialHair": {
                "moustache": 0.4,
                "beard": 0.4,
                "sideburns": 0.1
            },
            "glasses": "NoGlasses",
            "emotion": {
                "anger": 0,
                "contempt": 0,
                "disgust": 0,
                "fear": 0,
                "happiness": 0,
                "neutral": 0.999,
                "sadness": 0.001,
                "surprise": 0
            },
            "blur": {
                "blurLevel": "high",
                "value": 0.89
            },
            "exposure": {
                "exposureLevel": "goodExposure",
                "value": 0.51
            },
            "noise": {
                "noiseLevel": "medium",
                "value": 0.59
            },
            "makeup": {
                "eyeMakeup": true,
                "lipMakeup": false
            },
            "accessories": [],
            "occlusion": {
                "foreheadOccluded": false,
                "eyeOccluded": false,
                "mouthOccluded": false
            },
            "hair": {
                "bald": 0.04,
                "invisible": false,
                "hairColor": [
                    {
                        "color": "black",
                        "confidence": 0.98
                    },
                    {
                        "color": "brown",
                        "confidence": 0.87
                    },
                    {
                        "color": "gray",
                        "confidence": 0.85
                    },
                    {
                        "color": "other",
                        "confidence": 0.25
                    },
                    {
                        "color": "blond",
                        "confidence": 0.07
                    },
                    {
                        "color": "red",
                        "confidence": 0.02
                    }
                ]
            }
        }
    },
    {
        "faceId": "37c7c4bc-fda3-4d8d-94e8-b85b8deaf878",
        "faceRectangle": {
            "top": 693,
            "left": 1503,
            "width": 180,
            "height": 180
        },
        "faceAttributes": {
            "smile": 0.003,
            "headPose": {
                "pitch": 0,
                "roll": 2,
                "yaw": -2.2
            },
            "gender": "female",
            "age": 56,
            "facialHair": {
                "moustache": 0,
                "beard": 0,
                "sideburns": 0
            },
            "glasses": "NoGlasses",
            "emotion": {
                "anger": 0,
                "contempt": 0.001,
                "disgust": 0,
                "fear": 0,
                "happiness": 0.003,
                "neutral": 0.984,
                "sadness": 0.011,
                "surprise": 0
            },
            "blur": {
                "blurLevel": "high",
                "value": 0.83
            },
            "exposure": {
                "exposureLevel": "goodExposure",
                "value": 0.41
            },
            "noise": {
                "noiseLevel": "high",
                "value": 0.76
            },
            "makeup": {
                "eyeMakeup": false,
                "lipMakeup": false
            },
            "accessories": [],
            "occlusion": {
                "foreheadOccluded": false,
                "eyeOccluded": false,
                "mouthOccluded": false
            },
            "hair": {
                "bald": 0.06,
                "invisible": false,
                "hairColor": [
                    {
                        "color": "black",
                        "confidence": 0.99
                    },
                    {
                        "color": "gray",
                        "confidence": 0.89
                    },
                    {
                        "color": "other",
                        "confidence": 0.64
                    },
                    {
                        "color": "brown",
                        "confidence": 0.34
                    },
                    {
                        "color": "blond",
                        "confidence": 0.07
                    },
                    {
                        "color": "red",
                        "confidence": 0.03
                    }
                ]
            }
        }
    }
]
```

## <a name="next-steps"></a>다음 단계

이미지에서 사람 얼굴을 감지하고, 사각형으로 얼굴 경계를 표시하고, 연령이나 성별 같은 특성을 반환하는 데 사용되는 Face API를 살펴봅니다.

> [!div class="nextstepaction"]
> [Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
