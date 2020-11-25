---
title: '빠른 시작: REST API 및 JavaScript를 사용하여 이미지에서 얼굴 감지'
titleSuffix: Azure Cognitive Services
description: 이 빠른 시작에서는 Cognitive Services에서 JavaScript와 함께 Face API를 사용하여 이미지의 얼굴을 감지합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.custom: devx-track-js
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: f302000529e0dbf7ecce69ac9bebe77af59561ee
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96020843"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-javascript"></a>빠른 시작: REST API 및 JavaScript를 사용하여 이미지에서 얼굴 감지

이 빠른 시작에서는 이미지에서 사람 얼굴을 감지하기 위해 JavaScript와 함께 Azure Face REST API를 사용합니다.

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 - [체험 구독 만들기](https://azure.microsoft.com/free/cognitive-services/)
* Azure 구독을 보유한 후에는 Azure Portal에서 <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Face 리소스 만들기"  target="_blank">Face 리소스 <span class="docon docon-navigate-external x-hidden-focus"></span></a>를 만들어 키와 엔드포인트를 가져옵니다. 배포 후 **리소스로 이동** 을 클릭합니다.
    * 애플리케이션을 Face API에 연결하려면 만든 리소스의 키와 엔드포인트가 필요합니다. 이 빠른 시작의 뒷부분에 나오는 코드에 키와 엔드포인트를 붙여넣습니다.
    * 평가판 가격 책정 계층(`F0`)을 통해 서비스를 사용해보고, 나중에 프로덕션용 유료 계층으로 업그레이드할 수 있습니다.
* [Visual Studio Code](https://code.visualstudio.com/download) 같은 코드 편집기

## <a name="initialize-the-html-file"></a>HTML 파일 초기화

*detectFaces.html* 이라는 새 HTML 파일을 만들고 다음 코드를 추가합니다.

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Detect Faces Sample</title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
    </head>
    <body></body>
</html>
```

그런 다음, 문서의 `body` 요소 내에 다음 코드를 추가합니다. 이 코드는 URL 필드, **얼굴 분석** 단추, 응답 창 및 이미지 표시 창이 있는 기본 사용자 인터페이스를 설정합니다.

:::code language="html" source="~/cognitive-services-quickstart-code/javascript/web/face/rest/detect.html" id="html_include":::

## <a name="write-the-javascript-script"></a>JavaScript 스크립트 작성

문서의 `h1` 요소 바로 위에 다음 코드를 추가합니다. 이 코드는 Face API를 호출하는 JavaScript 코드를 설정합니다.

:::code language="html" source="~/cognitive-services-quickstart-code/javascript/web/face/rest/detect.html" id="script_include":::

`subscriptionKey` 필드를 구독 키의 값으로 업데이트하고, 올바른 엔드포인트 문자열이 포함되도록 `uriBase` 문자열을 변경해야 합니다. `returnFaceAttributes` 필드는 검색할 얼굴 특성을 지정하며 원하는 용도에 맞게 이 문자열을 변경할 수 있습니다.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="run-the-script"></a>스크립트 실행

브라우저에서 *detectFaces.html* 을 엽니다. **얼굴 분석** 단추를 클릭하면 앱에 지정된 URL의 이미지가 표시되고 얼굴 데이터의 JSON 문자열이 출력됩니다.

![GettingStartCSharpScreenshot](../Images/face-detect-javascript.png)

다음 텍스트는 성공적인 JSON 응답의 예제입니다.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    }
  }
]
```

## <a name="extract-face-attributes"></a>얼굴 특성 추출
 
얼굴 특성을 추출하려면 검색 모델 1을 사용하고 `returnFaceAttributes` 쿼리 매개 변수를 추가합니다.

```javascript
// Request parameters.
var params = {
    "detectionModel": "detection_01",
    "returnFaceAttributes": "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise",
    "returnFaceId": "true"
};
```

이제 응답에 얼굴 특성이 포함됩니다. 예를 들면 다음과 같습니다.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 Azure Face 서비스를 호출하는 JavaScript 스크립트를 작성하여 이미지에서 얼굴을 감지하고 특성을 나타냈습니다. 이제 Face API 참조 설명서에서 자세한 내용을 알아보겠습니다.

> [!div class="nextstepaction"]
> [Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
