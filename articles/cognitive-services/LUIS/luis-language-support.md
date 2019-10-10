---
title: 언어 지원 - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS는 서비스 내에 다양한 기능을 포함합니다. 모든 기능이 동일한 언어 패리티에 있는 것은 아닙니다. 관심 있는 기능이 사용자가 원하는 언어 문화권에서 지원되는지 확인합니다. LUIS 앱은 문화권에 관련되며 설정된 후에는 변경할 수 없습니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: bd1e665114fff4d5b7b0b2dca267207bdeebab56
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949557"
---
# <a name="language-and-region-support-for-luis"></a>LUIS에 대한 언어 및 지역 지원

LUIS는 서비스 내에 다양한 기능을 포함합니다. 모든 기능이 동일한 언어 패리티에 있는 것은 아닙니다. 관심 있는 기능이 사용자가 원하는 언어 문화권에서 지원되는지 확인합니다. LUIS 앱은 문화권에 관련되며 설정된 후에는 변경할 수 없습니다.

## <a name="multi-language-luis-apps"></a>다국어 LUIS 앱

챗봇과 같은 다국어 LUIS 클라이언트 애플리케이션이 필요한 경우, 몇 가지 옵션이 있습니다. LUIS가 모든 언어를 지원하는 경우에는 언어별로 LUIS 앱을 개발합니다. 각 LUIS 앱에는 고유한 앱 ID와 엔드포인트 로그가 있습니다. 지원되지 않는 언어 LUIS에 대한 Language Understanding을 제공해야 하는 경우, [Microsoft Translator API](../Translator/translator-info-overview.md)를 사용하여 발화를 지원되는 언어로 번역하고, 발화를 LUIS 엔드포인트에 제출하고, 결과 점수를 받을 수 있습니다.

## <a name="languages-supported"></a>지원되는 언어

LUIS는 발화를 다음 언어로 이해합니다.

| 언어 |로캘  |  미리 빌드된 도메인 | 미리 빌드된 엔터티 | 구 목록 권장 사항 | \**[Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(감정 및<br>키워드)|
|--|--|:--:|:--:|:--:|:--:|
| 미국 영어 |`en-US` | ✔ | ✔  |✔|✔|
| *[중국어](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| 네덜란드어 |`nl-NL` |✔|  -   |-|✔|
| 프랑스어 (프랑스) |`fr-FR` |✔| ✔ |✔ |✔|
| 프랑스어(캐나다) |`fr-CA` |-|   -   |-|✔|
| 독일어 |`de-DE` |✔| ✔ |✔ |✔|
| 힌디어 | `hi-IN`|-|-|-|-|
| 이탈리아어 |`it-IT` |✔| ✔ |✔|✔|
| *[일본어](#japanese-support-notes) |`ja-JP` |✔| ✔ |✔|주요 구문만|
| 한국어 |`ko-KR` |✔|   -   |-|주요 구문만|
| 포르투갈어(브라질) |`pt-BR` |✔| ✔ |✔ |일부 하위 문화권은 아님|
| 스페인어(스페인) |`es-ES` |✔| ✔ |✔|✔|
| 스페인어(멕시코)|`es-MX` |-|  -   |✔|✔|
| 터키어 | `tr-TR` |✔|-|-|감정만|


언어 지원은 [미리 빌드된 엔터티](luis-reference-prebuilt-entities.md) 및 [미리 빌드된 도메인](luis-reference-prebuilt-domains.md)에 따라 다릅니다.

### <a name="chinese-support-notes"></a>*중국어 지원 참고

 - `zh-cn` 문화권에서 LUIS는 기존 문자 집합 대신 중국어 간체 문자 집합을 예상합니다.
 - 의도, 엔터티, 기능 및 정규식의 이름은 중국어 또는 로마 문자일 수 있습니다.
 - @No__t-1 문화권에서 지원 되는 미리 빌드된 도메인에 대 한 자세한 내용은 미리 작성 된 [도메인 참조](luis-reference-prebuilt-domains.md) 를 참조 하세요.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>*일본어 지원 참고

 - LUIS는 구문 분석을 제공하지 않으며 Keigo와 비공식 일본어 간의 차이를 이해하지 못하므로 여러 수준의 형식을 애플리케이션의 학습 예제로 통합해야 합니다.
     - でございます는 です와 같지 않습니다.
     - です는 だ와 같지 않습니다.

### <a name="text-analytics-support-notes"></a>\*\*Text Analytics 지원 노트
Text Analytics에는 keyPhrase 미리 빌드된 엔터티 및 감정 분석이 포함되어 있습니다. 하위 문화권에는 포르투갈어만 지원됩니다. `pt-PT` 및 `pt-BR`. 다른 모든 문화권은 기본 문화권 수준에서 지원됩니다. Text Analytics [지원되는 언어](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)에 대해 자세히 알아봅니다.

### <a name="speech-api-supported-languages"></a>Speech API 지원되는 언어
Speech 받아쓰기 모드 언어에 대해서는 Speech [지원되는 언어](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode)를 참조하세요.

### <a name="bing-spell-check-supported-languages"></a>Bing Spell Check 지원되는 언어
지원되는 언어 및 상태 목록은 Bing Spell Check [지원되는 언어](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages)를 참조하세요.

## <a name="rare-or-foreign-words-in-an-application"></a>애플리케이션의 희귀 또는 외래 단어
`en-us` 문화권에서 LUIS는 은어를 포함하여 대부분의 영어 단어를 구분하도록 학습됩니다. `zh-cn` 문화권에서 LUIS는 대부분의 중국어 문자를 구분하도록 학습됩니다. `en-us`의 희귀 단어 또는 `zh-cn`의 문자를 사용할 때 LUIS가 해당 단어나 문자를 구분할 수 없는 것으로 보이면 해당 단어나 문자를 [구문 목록 기능](luis-how-to-add-features.md)에 추가할 수 있습니다. 예를 들어, 애플리케이션 문화권 외부의 단어(외래 단어)는 구문 목록 기능에 추가해야 합니다. 

<!--This phrase list should be marked non-interchangeable, to indicate that the set of rare words forms a class that LUIS should learn to recognize, but they are not synonyms or interchangeable with each other.-->

### <a name="hybrid-languages"></a>하이브리드 언어
하이브리드 언어는 영어 및 중국어와 같은 두 문화권의 단어를 결합합니다. 앱은 단일 문화권을 기반으로 하므로 이러한 언어는 LUIS에서 지원되지 않습니다.

## <a name="tokenization"></a>토큰화
기계 학습을 수행하기 위해 LUIS는 문화권에 따라 발화를 [토큰](luis-glossary.md#token)으로 구분합니다.

|언어|  모든 공백 또는 특수 문자 | 문자 수준|복합 단어|[토큰화된 엔터티가 반환됨](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|중국어||✔||✔|
|네덜란드어|||✔|✔|
|미국 영어(en-us)|✔ ||||
|프랑스어(fr-FR)|✔||||
|프랑스어(fr-CA)|✔||||
|독일어|||✔|✔|
| 힌디어 |✔|-|-|-|-|
|이탈리아어|✔||||
|일본어||||✔|
|한국어||✔||✔|
|포르투갈어(브라질)|✔||||
|스페인어(es-ES)|✔||||
|스페인어(es-MX)|✔||||

### <a name="custom-tokenizer-versions"></a>사용자 지정 토크 버전

다음 문화권에는 사용자 지정 토크 버전이 있습니다.

|Culture|버전|용도|
|--|--|--|
|독일어<br>`de-de`|1.0.0|복합 단어를 단일 구성 요소로 분할 하는 기계 학습 기반 토크 토큰화를 사용 하 여 단어를 분할 합니다.<br>사용자가 utterance으로 `Ich fahre einen krankenwagen`을 입력 하면-1 @no__t로 설정 됩니다. @No__t-0 및 `wagen`을 다른 엔터티로 독립적으로 표시할 수 있습니다.|
|독일어<br>`de-de`|1.0.2|단어를 공백으로 분할 하 여 단어를 토큰화.<br> 사용자가 utterance로 @no__t를 입력 하면 단일 토큰으로 유지 됩니다. 따라서 `krankenwagen`은 단일 엔터티로 표시 됩니다. |

### <a name="migrating-between-tokenizer-versions"></a>토크 버전 간 마이그레이션
<!--
Your first choice is to change the tokenizer version in the app file, then import the version. This action changes how the utterances are tokenized but allows you to keep the same app ID. 

Tokenizer JSON for 1.0.0. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.0",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.0",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 23,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```

Tokenizer JSON for version 1.0.1. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.1",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.1",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 16,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```
-->

토큰화는 앱 수준에서 발생 합니다. 버전 수준 토큰화는 지원 되지 않습니다. 

버전이 아닌 [새 앱으로 파일을 가져옵니다](luis-how-to-start-new-app.md#import-an-app-from-file). 이 작업은 새 앱의 앱 ID가 다르지만 파일에 지정 된 토크 토크 버전을 사용 함을 의미 합니다. 
