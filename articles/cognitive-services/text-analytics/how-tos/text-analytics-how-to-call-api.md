---
title: 텍스트 분석 API 호출
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 Azure Cognitive Services Text Analytics REST API 및 Postman을 호출 하는 방법을 설명 합니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: b2c994d23e63f9e2118cd3e6571c5dcc0449a367
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90601098"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>텍스트 분석 REST API를 호출하는 방법

**텍스트 분석 API** 호출은 어떤 언어로도 작성 가능한 HTTP POST/GET 호출입니다. 이 문서에서는 REST 및 [Postman](https://www.postman.com/downloads/)을 사용하여 주요 개념을 보여 줍니다.

각 요청에는 액세스 키 및 HTTP 엔드포인트를 포함해야 합니다. 엔드포인트는 등록 시 선택한 지역, 서비스 URL 및 요청에 사용되는 리소스를 지정합니다(`sentiment`, `keyphrases`, `languages` 및 `entities`). 

다시 말하지만 Text Analytics는 상태 비저장 서비스이므로 관리할 데이터 자산이 없습니다. 텍스트는 업로드되고, 수신된 후 분석됩니다. 그러면 결과가 호출 애플리케이션에 즉시 반환됩니다.

[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

<a name="json-schema"></a>

## <a name="json-schema-definition"></a>JSON 스키마 정의

입력은 구조화되지 않은 원시 텍스트의 JSON이어야 합니다. XML은 지원되지 않습니다. 스키마는 간단하며, 다음 목록에 설명된 요소로 구성됩니다. 

현재 모든 Text Analytics석 작업(감정, 핵심 구, 언어 감지 및 엔터티 식별)을위 해 동일한 문서를 제출할 수 있습니다. (스키마는 추후에 분석별로 달라질 수 있습니다.)

| 요소 | 유효한 값 | 필수 여부 | 사용량 |
|---------|--------------|-----------|-------|
|`id` |데이터 형식은 문자열이지만 실제로 문서 ID는 정수인 경우가 많습니다. | 필수 | 시스템은 사용자가 제공하는 ID를 사용하여 출력을 구성합니다. 언어 코드, 핵심 구 및 감정 점수가 요청의 각 ID에 대해 생성됩니다.|
|`text` | 최대 5120 자의 비구조적 원시 텍스트입니다. | 필수 | 언어 감지의 경우 텍스트를 어떤 언어로도 나타낼 수 있습니다. 감정 분석, 핵심 구 추출 및 엔터티 식별의 경우 텍스트는 [지원되는 언어](../text-analytics-supported-languages.md)로 작성되어야 합니다. |
|`language` | [지원되는 언어](../text-analytics-supported-languages.md)의 2자리 [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 코드 | 상황에 따라 다름 | 감정 분석, 핵심 구 추출 및 엔터티 연결의 경우 필수. 언어 감지의 경우 옵션 제외해도 오류는 발생하지 않지만 분석 효과가 약해집니다. 언어 코드는 사용자가 제공한 `text`와 일치해야 합니다. |

제한에 대한 자세한 내용은 [Text Analytics 개요 > 데이터 제한](../overview.md#data-limits)을 참조하세요. 


```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    },
    {
      "language": "en",
      "id": "3",
      "text": "Pike place market is my favorite Seattle attraction."
    }
  ]
}
```


## <a name="set-up-a-request-in-postman"></a>Postman에서 요청 설정

이 서비스는 최대 1MB 크기의 요청을 수락합니다. Postman(또는 다른 Web API 테스트 도구)을 사용하는 경우 사용하려는 리소스를 포함하도록 엔드포인트을 설정하고 요청 헤더에 액세스 키를 제공합니다. 각 작업에서 사용자는 엔드포인트에 적절한 리소스를 추가해야 합니다. 

1. Postman:

   + 요청 형식으로 **Post**를 선택합니다.
   + 포털 페이지에서 복사한 엔드포인트를 붙여 넣습니다.
   + 리소스를 추가합니다.

   리소스 엔드포인트는 다음과 같습니다(지역마다 다를 수 있음).

   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/sentiment`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/keyPhrases`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/languages`
   + `https://westus.api.cognitive.microsoft.com/text/analytics/v3.0/entities/recognition/general`

2. 다음 세 개의 요청 헤더를 설정합니다.

   + `Ocp-Apim-Subscription-Key`: Azure Portal에서 가져온 액세스 키
   + `Content-Type`: application/json
   + `Accept`: application/json

   요청은 **/keyPhrases** 리소스라고 가정하고 다음 스크린샷과 유사해야 합니다.

   ![엔드포인트 및 헤더를 포함하는 요청 스크린샷](../media/postman-request-keyphrase-1.png)

4. **본문**을 클릭하고 형식으로 **raw**를 선택합니다.

   ![본문 설정을 포함하는 요청 스크린샷](../media/postman-request-body-raw.png)

5. 의도한 분석에 적합한 형식으로 일부 JSON 문서를 붙여 넣습니다. 특정 분석에 대한 자세한 내용은 아래 항목을 참조하세요.

  + [언어 감지](text-analytics-how-to-language-detection.md)  
  + [핵심 구 추출](text-analytics-how-to-keyword-extraction.md)  
  + [감정 분석](text-analytics-how-to-sentiment-analysis.md)  
  + [엔터티 인식](text-analytics-how-to-entity-linking.md)  


6. **보내기**를 클릭하여 요청을 제출합니다. 분당 보낼 수 있는 요청 수와 초에 대 한 자세한 내용은 개요의 [데이터 제한](../overview.md#data-limits) 섹션을 참조 하세요.

   Postman에서 응답은 아래의 다음 창에 단일 JSON 문서로 표시되며, 각 문서 ID 항목이 요청에 제공됩니다.

## <a name="see-also"></a>추가 정보 

 [Text Analytics 개요](../overview.md)  
 [FAQ(질문과 대답)](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [언어 감지](text-analytics-how-to-language-detection.md)
