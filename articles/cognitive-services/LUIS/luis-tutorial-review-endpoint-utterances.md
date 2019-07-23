---
title: 엔드포인트 발화 검토
titleSuffix: Azure Cognitive Services
description: LUIS에서 알 수 없는 LUIS HTTP 엔드포인트를 통해 수신된 발언을 확인하거나 수정하여 앱 예측을 향상시키는 방법을 학습합니다. 의도를 확인해야 하는 발언도 있고, 엔터티를 확인해야 하는 발언도 있습니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 2994f7b19d5a104b129dc4d7aff29dabbc89f0f4
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275999"
---
# <a name="tutorial-fix-unsure-predictions-by-reviewing-endpoint-utterances"></a>자습서: 엔드포인트 발언을 검토하여 알 수 없는 예측 수정
이 자습서에서는 LUIS에서 알 수 없는 LUIS HTTPS 엔드포인트를 통해 수신된 발언을 확인하거나 수정하여 앱 예측을 향상시키는 방법을 학습합니다. 의도를 확인해야 하는 발언도 있고, 엔터티를 확인해야 하는 발언도 있습니다. 예약된 LUIS 유지 관리의 정기적인 과정 형태로 엔드포인트 발언 검토를 진행해야 합니다. 

이 검토 프로세스는 LUIS가 앱 도메인을 학습하는 또 다른 방법입니다. LUIS는 검토 목록에 표시되는 발언을 선택했습니다. 이 목록은 다음과 같습니다.

* 앱에만 적용됩니다.
* 앱의 예측 정확도를 높이기 위한 것입니다. 
* 정기적으로 검토해야 합니다. 

엔드포인트 발언을 검토하여 발언의 예측된 의도를 확인하거나 수정합니다. 또한 예측되지 않았거나 잘못 예측된 사용자 지정 엔터티에도 레이블을 지정합니다. 

**이 자습서에서 학습할 내용은 다음과 같습니다.**

<!-- green checkmark -->
> [!div class="checklist"]
> * 앱 가져오기 예제
> * 엔드포인트 utterances 검토
> * 구문 목록 업데이트
> * 앱 교육
> * 앱 게시
> * 앱의 엔드포인트를 쿼리하여 LUIS JSON 응답 확인

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>앱 가져오기 예제

마지막 자습서에서 만든 **HumanResources**라는 앱을 사용하여 계속 진행합니다. 

다음 단계를 사용하세요.

1.  [앱 JSON 파일](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-sentiment-HumanResources.json)을 다운로드하고 저장합니다.

1. JSON을 새 앱으로 가져옵니다.

1. **관리** 섹션의 **버전** 탭에서 버전을 복제하고 `review`라는 이름을 지정합니다. 복제는 원래 버전에 영향을 주지 않고도 다양한 LUIS 기능을 사용할 수 있는 좋은 방법입니다. 버전 이름이 URL 경로의 일부로 사용되므로 이름에는 URL에 유효하지 않은 문자가 포함될 수 없습니다.

1. 새 앱을 학습하고 게시합니다.

1. 엔드포인트를 사용하여 다음 발화를 추가합니다. [스크립트](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) 또는 브라우저의 엔드포인트에서 이 작업을 수행할 수 있습니다. 추가할 발언은 다음과 같습니다.

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

    일련의 자습서를 통해 모든 버전의 앱을 사용하는 경우 버전에 따라 **엔드포인트 발언 검토** 목록이 변경되지 않는다는 점을 확인할 수 있을 것입니다. 적극적으로 편집 중인 버전이나 엔드포인트에서 게시된 앱 버전에 관계없이 검토할 단일 발언 풀이 있습니다. 

## <a name="review-endpoint-utterances"></a>엔드포인트 utterances 검토

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. 왼쪽 탐색에서 **엔드포인트 발언 검토**를 선택합니다. 목록은 **ApplyForJob** 의도에 대해 필터링됩니다. 

    [![왼쪽 탐색에 있는 엔드포인트 발화 검토 단추의 스크린샷](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-entity-view.png#lightbox)

1. **엔터티 보기**를 설정/해제하여 레이블이 지정된 엔터티를 확인합니다. 
    
    [![엔터티 보기 설정/해제가 강조 표시된 엔드포인트 발화 검토의 스크린샷](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png)](./media/luis-tutorial-review-endpoint-utterances/review-endpoint-utterances-with-token-view.png#lightbox)


    이 발화(`I'm looking for a job with Natural Language Processing`)는 올바른 의도에 있지 않습니다. 

    발화가 잘못 예측된 이유는 **GetJobInformation**의 7개 발화와 비교하여 **ApplyForJob** 의도에 21개 발화가 있기 때문입니다. 더 많은 발화가 있는 의도가 더 높은 예측성을 갖습니다. 의도 전반에서 발화의 개수와 품질에 균형을 유지하는 것이 중요합니다.

1.  이 발화를 정렬하려면 올바른 의도를 선택하고 그 안에서 작업 엔터티를 표시합니다. 녹색 확인란을 선택하여 변경된 발화를 앱에 추가합니다. 

    |발화|올바른 의도|누락된 엔터티|
    |:--|:--|:--|
    |`I'm looking for a job with Natural Language Processing`|GetJobInfo|작업 - "자연어 처리"|

    발화를 추가하면 발화가 **엔드포인트 발화 검토**에서 **GetJobInformation** 의도로 이동합니다. 엔드포인트 발언은 이제 해당 의도에 대한 예제 발언이 되었습니다. 

    이 발화를 올바르게 정렬하는 것과 함께 더 많은 발화를 **GetJobInformation** 의도에 추가해야 합니다. 이는 스스로 완료하도록 연습으로 남겨둡니다. **None** 의도를 제외한 각 의도에는 대략적으로 예제와 동일한 수의 발언이 있어야 합니다. **None** 의도에는 앱의 총 발언의 10%가 있어야 합니다. 

1. 이 의도에서 남은 발언을 검토하고, 발언에 레이블을 지정하고, 올바르지 않은 경우 **정렬된 의도**를 수정합니다.

1. 목록에는 더 이상 이런 발언이 없어야 합니다. 발언이 여전히 표시되는 경우, 목록이 빌 때까지 의도를 수정하고, 누락된 엔터티에 레이블을 지정하여 계속 작업합니다. 

1. 필터 목록에서 다음 의도를 선택한 후 계속 발언을 수정하고 엔터티에 레이블을 지정합니다. 각 의도의 마지막 단계는 발언 행에서 **정렬된 의도를 추가**를 선택하거나 각 의도별로 확인란을 선택하고 테이블 위의 **추가 선택**을 선택하는 것입니다.

    필터 목록의 모든 의도와 엔터티가 비워질 때까지 계속합니다. 이는 매우 작은 앱입니다. 검토 프로세스는 몇 분 정도만 걸립니다. 

## <a name="update-phrase-list"></a>구문 목록 업데이트
새로 검색된 작업 이름으로 문구 목록을 최신 상태로 유지합니다. 

1. 왼쪽 탐색에서 **문구 목록**을 선택합니다.

2. **작업** 문구 목록을 선택합니다.

3. 값으로 `Natural Language Processing`을 추가한 다음, **저장**을 선택합니다. 

## <a name="train"></a>학습

LUIS는 학습할 때까지 변경 내용을 알지 못합니다. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>게시

이 앱을 가져온 경우 **감정 분석**을 선택해야 합니다.

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>엔드포인트에서 의도 및 엔터티 가져오기

수정된 발언에 가까운 발언을 시도합니다. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. 주소의 URL 끝으로 이동하고 `Are there any natural language processing jobs in my department right now?`를 입력합니다. 마지막 쿼리 문자열 매개 변수는 발언 **쿼리**를 나타내는 `q`입니다. 

   ```json
   {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
   }
   }
   ```

   올바른 의도는 높은 점수를 통해 예측되며 **작업** 엔터티는 `natural language processing`으로 감지됩니다. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>더 많은 발언을 추가하여 검토를 대체할 수 있습니까? 
더 많은 예제 발언을 추가하지 않은 이유가 궁금할 것입니다. 엔드포인트 발언 검토의 목적은 무엇입니까? 실제 LUIS 앱에서 엔드포인트 발언은 아직 사용하지 않은 단어 선택 및 배열을 사용하는 사용자로부터 나온 것입니다. 동일한 단어 선택과 배열을 사용한 경우, 원래의 예측은 더 높은 백분율을 가집니다. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>발언 목록에 최고 의도가 있는 이유는 무엇입니까? 
일부 엔드포인트 발언은 검토 목록에서 높은 예측 점수를 갖습니다. 여전히 이러한 발언을 검토하고 확인해야 합니다. 그 다음 높은 의도가 최고 의도 점수에 매우 가까운 점수를 가졌기 때문에 이러한 발언이 목록에 있습니다. 최고 의도 두 개는 약 15% 정도 차이가 있는 것이 좋습니다.

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>다음 단계
이 자습서에서는 엔드포인트에 제출된 발언 중 LUIS가 알지 못하는 발언을 검토했습니다. 이러한 발언이 확인되고 올바른 의도에 예시 발언으로 이동하면 LUIS가 예측 정확도를 개선합니다.

> [!div class="nextstepaction"]
> [패턴 사용 방법 알아보기](luis-tutorial-pattern.md)
