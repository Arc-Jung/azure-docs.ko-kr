---
title: 기술 자료 - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker 기술 자료는 질문/답변(QnA) 쌍, 각 QnA 쌍과 연결된 선택적 메타데이터의 집합으로 구성됩니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 022b16669791b9b9cce066b3dd17c70b33569cc0
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955244"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>QnA Maker 기술 자료란?

QnA Maker 기술 자료는 질문/답변(QnA) 쌍, 각 QnA 쌍과 연결된 선택적 메타데이터의 집합으로 구성됩니다.

## <a name="key-knowledge-base-concepts"></a>주요 기술 자료 개념

* **질문** - 사용자 쿼리를 가장 잘 나타내는 텍스트를 포함하는 질문입니다. 
* **답변** - 답변은 사용자 쿼리가 관련 질문과 일치할 경우 반환되는 응답입니다.  
* **메타데이터** - 메타데이터는 QnA 쌍과 연결된 태그이며 키-값 쌍으로 표현됩니다. 메타데이터 태그는 QnA 쌍을 필터링하고 쿼리 매칭이 수행되는 집합을 제한하는 데 사용됩니다.

숫자 QnA ID로 표현되는 단일 QnA에는 모두 단일 답변에 매핑되는 질문의 여러 변형(대체 질문)이 있습니다. 또한 각 쌍에는 하나의 키와 하나의 값과 연결 된 메타 데이터 필드가 여러 개 있을 수 있습니다.

![QnA Maker 기술 자료](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>기술 자료 콘텐츠 형식

기술 자료에 풍부한 콘텐츠를 수집하면 QnA Maker가 콘텐츠를 markdown으로 변환하려고 합니다. 대부분의 채팅 클라이언트에서 이해할 수 있는 markdown 형식을 이해하려면 [이](https://aka.ms/qnamaker-docs-markdown-support) 블로그를 읽어 보세요.

메타데이터 필드는 콜론으로 구분된 키-값 쌍으로 이루어져 있습니다 **(Product:Shredder)** . 키와 값은 모두 텍스트로만 구성되어야 합니다. 메타데이터 키에는 공백이 포함되어서는 안 됩니다. 메타 데이터는 키 당 하나의 값만 지원 합니다.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>QnA Maker에서 사용자 쿼리를 처리 하 여 최상의 답을 선택 하는 방법

학습 및 [게시](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) 된 QnA Maker 기술 자료는 [generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)에서 봇 또는 다른 클라이언트 응용 프로그램의 사용자 쿼리를 수신 합니다. 다음 다이어그램에서는 사용자 쿼리를 수신 하는 프로세스를 보여 줍니다.

![사용자 쿼리에 대 한 순위 지정 프로세스](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

프로세스는 다음 표에 설명 되어 있습니다.

|단계|용도|
|--|--|
|1|클라이언트 응용 프로그램이 사용자 쿼리를 [Generateanswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage)로 보냅니다.|
|2|Qna Maker는 언어 검색, spellers 및 단어 분리기를 사용 하 여 사용자 쿼리를 전처리 합니다.|
|3|이러한 전처리는 최상의 검색 결과를 위해 사용자 쿼리를 변경 하는 데 사용 됩니다.|
|4|이 변경 된 쿼리는 결과 `top` 수를 수신 하는 Azure Search 인덱스에 전송 됩니다. 이러한 결과에 올바른 답변이 없는 경우 값 `top` 을 약간 늘립니다. 일반적으로에 대 한 `top` 값 10은 쿼리의 90%에서 작동 합니다.|
|5|QnA Maker 고급 기능화 적용 하 여 사용자 쿼리에 대해 인출 된 Azure Search 결과의 정확성을 확인 합니다. |
|6|학습 된 ranker 모델은 5 단계의 기능 점수를 사용 하 여 Azure Search 결과의 순위를 결정 합니다.|
|7|새 결과는 순서 대로 클라이언트 응용 프로그램에 반환 됩니다.|
|||

사용 되는 기능에는 단어 수준 의미 체계, 모음의 용어 수준 중요도 및 두 텍스트 문자열 간의 유사성과 연관성을 확인 하는 심층 학습 의미 체계 모델이 포함 되지만이에 국한 되지 않습니다.


## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [기술 자료의 개발 수명 주기](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>참고자료

[QnA Maker 개요](../Overview/overview.md)
