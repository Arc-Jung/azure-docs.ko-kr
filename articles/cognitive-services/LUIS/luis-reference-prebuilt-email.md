---
title: LUIS 미리 작성 한 엔터티 전자 메일 참조
titleSuffix: Azure Cognitive Services
description: 이 아티클에는 LUIS(Language Understanding)의 이메일 미리 빌드된 엔터티가 포함됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 9c9c7b373f820dd23c70a67a1de8545935a1d93c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560243"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>LUIS 앱용 이메일 미리 빌드된 엔터티
이메일 추출에는 발언의 전체 이메일 주소가 포함됩니다. 이 엔터티를 이미 학습했기 때문에 이메일을 포함하는 예제 발언을 애플리케이션 의도에 추가할 필요가 없습니다. 이메일 엔터티는 `en-us` 문화권에서만 지원됩니다. 

## <a name="resolution-for-prebuilt-email"></a>미리 빌드된 이메일의 해결

### <a name="api-version-2x"></a>API 버전 2.x

다음 예제에서는 **builtin.email** 엔터티의 해결을 보여줍니다.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55,
      "resolution": {
        "value": "patti.owens@microsoft.com"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview API 버전 3(sp3)

다음 JSON은 `verbose` 매개 변수를로 `false`설정 하는입니다.

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ]
        }
    }
}
```


다음 JSON은 `verbose` 매개 변수를로 `true`설정 하는입니다.

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ],
            "$instance": {
                "email": [
                    {
                        "type": "builtin.email",
                        "text": "patti.owens@microsoft.com",
                        "startIndex": 31,
                        "length": 25,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>다음 단계

[number](luis-reference-prebuilt-number.md), [ordinal](luis-reference-prebuilt-ordinal.md) 및 [percentage](luis-reference-prebuilt-percentage.md)에 대해 알아봅니다. 
