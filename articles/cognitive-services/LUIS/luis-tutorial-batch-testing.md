---
title: '자습서: 일괄 처리 테스트 - LUIS'
titleSuffix: Azure Cognitive Services
description: 이 자습서에서는 일괄 처리 테스트를 사용하여 앱의 발언 예측 문제를 찾아 수정하는 방법을 보여줍니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: ac88931d79df6c2527a2a5fd72b440baeb463115
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499056"
---
# <a name="tutorial-batch-test-data-sets"></a>자습서: 일괄 처리 테스트 데이터 세트

이 자습서에서는 일괄 처리 테스트를 사용하여 앱의 발언 예측 문제를 찾아 수정하는 방법을 보여줍니다.  

일괄 테스트를 사용하면 레이블이 지정된 발언 및 엔터티의 알려진 집합을 사용하여 학습된 활성 모델의 상태가 유효한지 확인할 수 있습니다. JSON 형식의 일괄 처리 파일에서 발언을 추가하고, 발언 내에서 예측해야 하느 엔터티 레이블을 설정합니다. 

일괄 테스트를 위한 요구 사항:

* 테스트당 최대 1000개 발언 
* 중복되지 않습니다. 
* 허용되는 엔터티 형식: 기계 학습된 단순 및 복합 엔터티만. 일괄 테스트는 기계 학습된 의도 및 엔터티에만 유용합니다.

이 자습서 이외의 앱을 사용할 때에는 의도에 이미 추가된 발언 예제를 사용하지 *마세요*. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**이 자습서에서 학습할 내용은 다음과 같습니다.**

<!-- green checkmark -->
> [!div class="checklist"]
> * 예제 앱 가져오기
> * 일괄 테스트 파일 만들기 
> * 일괄 테스트 실행
> * 테스트 결과 검토
> * 오류 수정 
> * 다시 일괄 테스트

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>예제 앱 가져오기

마지막 자습서에서 만든 **HumanResources**라는 앱을 사용하여 계속 진행합니다. 

다음 단계를 사용하세요.

1.  [앱 JSON 파일](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-review-HumanResources.json)을 다운로드하고 저장합니다.

2. JSON을 새 앱으로 가져옵니다.

3. **관리** 섹션의 **버전** 탭에서 버전을 복제하고 `batchtest`라는 이름을 지정합니다. 복제는 원래 버전에 영향을 주지 않고도 다양한 LUIS 기능을 사용할 수 있는 좋은 방법입니다. 버전 이름이 URL 경로의 일부로 사용되므로 이름에는 URL에 유효하지 않은 문자가 포함될 수 없습니다. 

4. 앱을 학습합니다.

## <a name="batch-file"></a>일괄 처리 파일

1. 텍스트 편집기에서 `HumanResources-jobs-batch.json`을 만들거나 [다운로드](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-jobs-batch.json)합니다. 

2. JSON 형식의 일괄 처리 파일에서 테스트에서 예측할 **의도**가 포함된 발언을 추가합니다. 

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>일괄 처리 실행

1. 맨 위 탐색 모음에서 **테스트**를 선택합니다. 

2. 오른쪽 패널에서 **Batch testing panel**(일괄 테스트 패널)을 선택합니다. 

    [![일괄 테스트 패널이 강조 표시된 LUIS 앱의 스크린샷](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. **데이터 세트 가져오기**를 선택합니다.

    [![데이터 세트 가져오기가 강조 표시된 LUIS 앱의 스크린샷](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. `HumanResources-jobs-batch.json` 파일의 파일 위치를 선택합니다.

5. 데이터 세트의 이름을 `intents only`로 지정하고 **완료**를 선택합니다.

    ![파일 선택](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. **실행** 단추를 선택합니다. 

7. **See results**(결과 보기)를 선택합니다.

8. 그래프 및 범례에서 결과를 검토합니다.

    [![일괄 테스트 결과가 표시되는 LUIS 앱의 스크린샷](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>일괄 처리 결과 검토

일괄 처리 차트는 결과의 사분면으로 표시됩니다. 차트의 오른쪽이 필터입니다. 필터에는 의도와 엔터티가 포함됩니다. [차트의 섹션](luis-concept-batch-test.md#batch-test-results) 또는 차트 내의 한 점을 선택하면 차트 아래에 관련된 발언이 표시됩니다. 

차트 위로 마우스를 가져간 후 마우스 휠로 차트의 발언을 확대하거나 축소할 수 있습니다. 차트에 많은 점이 밀집되어 있을 때 이렇게 하면 편리합니다. 

차트는 사분면으로 구분되며 그중 두 개의 섹션이 빨간색으로 표시됩니다. **이러한 섹션에 집중해야 합니다**. 

### <a name="getjobinformation-test-results"></a>GetJobInformation 테스트 결과

필터에 표시되는 **GetJobInformation** 테스트 결과는 4개 예측 중 2개가 성공적이었음을 보여 줍니다. 3사분면에서 **False positive**라는 이름을 선택하여 차트 아래의 발화를 확인합니다. 

사용자 발화의 정확한 텍스트를 보려면 키보드 Ctrl+E를 사용하여 레이블 뷰로 전환합니다. 

`Is there a database position open in Los Colinas?` 발화는 _GetJobInformation_으로 레이블이 지정되었지만, 현재 모델에서 발화를 _ApplyForJob_으로 예측했습니다. 

**ApplyForJob**의 예제가 **GetJobInformation**보다 거의 3배 더 많습니다. 이러한 예제 발화 수의 불균형 때문에 **ApplyForJob** 의도에 가중치가 커져서 잘못된 예측이 발생합니다. 

두 의도는 동일한 오류 수를 갖습니다. 한 의도에서 예측이 잘못되면 다른 의도에도 영향을 미칩니다. 발언이 한 의도에 대해 잘못 예측되었고, 다른 의도에 대해서도 잘못 예측되었으므로 둘 다 오류가 있습니다. 

<a name="fix-the-app"></a>

## <a name="how-to-fix-the-app"></a>앱 수정 방법

이 섹션의 목적은 앱을 수정하여 **GetJobInformation**에 대해 모든 발언이 올바르게 예측되도록 하는 것입니다. 

간단해 보이는 해결 방법은 올바른 의도에 이러한 일괄 처리 파일 발언을 추가하는 것입니다. 하지만 원하는 동작은 아닙니다. 이러한 발언을 예제로 추가하지 않으면서 LUIS에서 발언이 올바르게 예측되도록 할 것입니다. 

**ApplyForJob** 발언 수량이 **GetJobInformation**와 같아질 때까지 발언을 제거하는 방법에 대해서도 관심이 있을 수 있습니다. 이렇게 하면 테스트 결과는 수정되지만 LUIS에서 다음 번에 해당 의도를 정확하게 예측하지 못할 수 있습니다. 

해결 방법은 **GetJobInformation**에 발화를 더 추가하는 것입니다. 작업 정보를 찾으려는 의도를 유지하면서 발화 길이, 단어 선택 및 단어 배열을 변경하되, 작업에 적용하면 ‘안 됩니다’. 

### <a name="add-more-utterances"></a>발언 추가

1. 위쪽 탐색 패널에서 **테스트** 단추를 선택하여 일괄 테스트 패널을 닫습니다. 

2. 의도 목록에서 **GetJobInformation**을 선택합니다. 

3. 길이, 단어 선택 및 단어 배열이 다른 발언을 더 추가하고 용어 `resume`, `c.v.` 및 `apply`도 포함해야 합니다.

    |**GetJobInformation** 의도에 대한 예제 발언|
    |--|
    |Does the new job in the warehouse for a stocker require that I apply with a resume?|
    |Where are the roofing jobs today?|
    |I heard there was a medical coding job that requires a resume.|
    |I would like a job helping college kids write their c.v.s. |
    |Here is my resume, looking for a new post at the community college using computers.|
    |What positions are available in child and home care?|
    |Is there an intern desk at the newspaper?|
    |My C.v. shows I'm good at analyzing procurement, budgets, and lost money. Is there anything for this type of work?|
    |Where are the earth drilling jobs right now?|
    |I've worked 8 years as an EMS driver. Any new jobs?|
    |New food handling jobs require application?|
    |How many new yard work jobs are available?|
    |Is there a new HR post for labor relations and negotiations?|
    |I have a masters in library and archive management. Any new positions?|
    |Are there any babysitting jobs for 13 year olds in the city today?|

    발언에서 **Job** 엔터티에 레이블을 지정하지 않도록 합니다. 이 자습서의 이 섹션에서는 의도 예측에만 주안점을 둡니다.

4. 오른쪽 상단 탐색 영역에서 **학습**을 선택하여 앱을 학습합니다.

## <a name="verify-the-new-model"></a>새 모델 확인

일괄 테스트의 발언이 올바르게 예측되는지 확인하려면 일괄 테스트를 다시 실행합니다.

1. 맨 위 탐색 모음에서 **테스트**를 선택합니다. 일괄 처리 결과가 계속 열려 있으면 **목록으로 돌아가기**를 선택합니다.  

1. 일괄 처리 이름 오른쪽에 있는 줄임표(***...***) 단추를 선택한 다음, **실행**을 선택합니다. 일괄 테스트가 완료될 때까지 기다립니다. **결과 보기** 단추가 이제 녹색으로 표시됩니다. 이것은 전체 일괄 처리가 성공적으로 실행되었음을 나타냅니다.

1. **See results**(결과 보기)를 선택합니다. 모든 의도의 이름 왼쪽에 녹색 아이콘이 있어야 합니다. 

## <a name="create-batch-file-with-entities"></a>엔터티를 사용하여 일괄 처리 파일 만들기 

일괄 테스트에서 엔터티를 확인하려면 일괄 처리 JSON 파일에서 엔터티에 레이블이 지정되어 있어야 합니다. 

전체 단어에 대한 엔터티 변형([토큰](luis-glossary.md#token)) 개수는 예측 품질에 영향을 줄 수 있습니다. 레이블이 지정된 발언과 함께 의도에 제공된 학습 데이터에 다양한 길이의 엔터티가 포함되는지 확인합니다. 

처음에 일괄 처리 파일을 작성하고 테스트할 때는 잘못 예측될 것으로 생각되는 소수의 발언과 엔터티 뿐만 아니라 잘 예측되는 것으로 알고 있는 소수의 발언 및 엔터티로 시작하는 것이 가장 좋습니다. 이렇게 하면 문제 영역에 빠르게 집중할 수 있습니다. 예측되지 않았던 몇 가지 다른 작업 이름을 사용하여 **GetJobInformation** 및 **ApplyForJob** 의도를 테스트한 후에 **Job** 엔터티의 특정 값에 예측 문제가 있는지 확인하기 위해 이 일괄 테스트 파일이 개발되었습니다. 

테스트 발언에 제공된 **Job** 엔터티의 값은 일반적으로 하나 또는 두 개의 단어로 되어 있지만 더 많은 단어를 포함하는 예제도 일부 있습니다. _직접 만든_ Human Resources 앱에 여러 단어의 작업 이름이 있는 경우 이 앱에서 **Job** 엔터티 레이블이 지정된 예제 발언은 잘 작동하지 않습니다.

1. [VSCode](https://code.visualstudio.com/) 같은 텍스트 편집기에서 `HumanResources-entities-batch.json`을 만들거나 [다운로드](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-entities-batch.json)합니다.

2. JSON 형식의 일괄 처리 파일에서 발언에서 엔터티의 위치 뿐만 아니라 테스트에서 예측하려는 **의도**가 있는 발언을 포함하는 개체 배열을 추가합니다. 엔터티는 토큰 기반이므로 한 문자에서 각 엔터티를 시작 및 중지해야 합니다. 발언 앞뒤에 공백을 포함하지 않도록 합니다. 공백이 있으면 일괄 처리 파일을 가져올 때 오류가 발생합니다.  

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>엔터티를 사용하여 일괄 처리 실행

1. 맨 위 탐색 모음에서 **테스트**를 선택합니다. 

2. 오른쪽 패널에서 **Batch testing panel**(일괄 테스트 패널)을 선택합니다. 

3. **데이터 세트 가져오기**를 선택합니다.

4. `HumanResources-entities-batch.json` 파일의 파일 시스템 위치를 선택합니다.

5. 데이터 세트의 이름을 `entities`로 지정하고 **완료**를 선택합니다.

6. **실행** 단추를 선택합니다. 테스트가 완료될 때까지 기다립니다.

7. **See results**(결과 보기)를 선택합니다.

## <a name="review-entity-batch-results"></a>엔터티 일괄 처리 결과 검토

모든 의도가 올바르게 예측된 차트가 열립니다. 오른쪽 필터에서 아래로 스크롤하여 오류가 있는 엔터티 예측을 찾습니다. 

1. 필터에서 **Job** 엔터티를 선택합니다.

    ![필터의 오류 엔터티 예측](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    엔터티 예측을 표시하도록 차트가 변경됩니다. 

2. 차트의 3사분면에서 **False Negative**를 선택합니다. 그런 다음, control + E 키보드 조합을 사용하여 토큰 보기로 전환합니다. 

    [![엔터티 예측의 토큰 보기](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)
    
    차트 아래의 발언을 검토하면 작업 이름에 `SQL`이 포함될 경우 일관된 오류가 나타납니다. 예제 발언과 Job 구 목록을 검토할 경우 SQL은 한 번만 사용되고 더 큰 작업 이름 `sql/oracle database administrator`의 일부에 불과한 것을 알 수 있습니다.

## <a name="fix-the-app-based-on-entity-batch-results"></a>엔터티 일괄 처리 결과를 기준으로 앱 수정

앱을 수정하려면 LUIS에서 SQL 작업 변형을 올바르게 판단해야 합니다. 이러한 수정에는 몇 가지 옵션이 있습니다. 

* 몇 가지 예제 발언을 명시적으로 추가합니다. 이러한 발언은 SQL을 사용하고 해당 단어에 Job 엔터티 레이블을 지정합니다. 
* 구 목록에 더 많은 SQL 작업을 명시적으로 추가

이러한 작업은 사용자가 수행합니다.

엔터티가 올바르게 예측되기 전에 [패턴](luis-concept-patterns.md)을 추가하면 문제가 해결되지 않습니다. 패턴의 모든 엔터티가 검색될 때까지 패턴이 일치되지 못하기 때문입니다. 

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>다음 단계

이 자습서에서는 일괄 처리 테스트를 사용하여 현재 모델의 문제점을 찾았습니다. 일괄 처리 파일을 사용하여 모델을 수정하고 다시 시작하여 올바르게 변경되었는지 확인했습니다.

> [!div class="nextstepaction"]
> [패턴에 대해 알아보기](luis-tutorial-pattern.md)

