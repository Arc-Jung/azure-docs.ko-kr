---
title: 데이터 원본 및 콘텐츠 형식-QnA Maker
description: 데이터 원본 및 지원 되는 콘텐츠 형식에서 질문 및 답변 쌍을 가져오는 방법에 대해 알아봅니다. 여기에는 PDF, .DOCX 및 TXT QnA Maker와 같은 여러 표준 구조적 문서가 포함 됩니다.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: 0d4d32aba34a97c6a060c999694f66d79933d011
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99556046"
---
# <a name="importing-from-data-sources"></a>데이터 원본에서 가져오기

기술 자료는 공용 Url 및 파일에서 가져온 질문과 대답 쌍으로 구성 됩니다.

## <a name="data-source-locations"></a>데이터 원본 위치

콘텐츠는 데이터 원본에서 기술 자료로 가져옵니다. 데이터 원본 위치는 인증이 필요 하지 않은 **공용 url 또는 파일** 입니다.

인증으로 보안이 설정 된 [SharePoint 파일](../how-to/add-sharepoint-datasources.md)은 예외입니다. SharePoint 리소스는 웹 페이지가 아닌 파일 이어야 합니다. URL은 .ASPX와 같은 웹 확장명으로 끝나는 경우 SharePoint에서 QnA Maker로 가져오지 않습니다.

## <a name="chit-chat-content"></a>Chit-채팅 콘텐츠

Chit-채팅 콘텐츠 집합은 여러 언어 및 대화형 스타일로 전체 콘텐츠 데이터 원본으로 제공 됩니다. 이는 봇 성격의 시작점 역할을 하며, 잡담을 처음부터 작성하는 데 드는 비용과 시간을 많이 절약할 수 있습니다. [Chit-채팅 콘텐츠](../how-to/chit-chat-knowledge-base.md) 를 기술 자료에 추가 하는 방법에 대해 알아봅니다.

## <a name="structured-data-format-through-import"></a>가져오기를 통한 구조화된 데이터 형식

기술 자료를 가져오면 기존 기술 자료의 콘텐츠가 바뀝니다. 가져오기에는 `.tsv` 질문과 답변이 포함 된 구조적 파일이 필요 합니다. 이 정보는 QnA Maker가 질문-답변 쌍을 그룹화하고 특정 데이터 원본에 귀속하는 데 도움이 됩니다.

| 질문  | Answer  | 원본| 메타 데이터 (1 개 키: 1 값) |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Editorial|    `Key:Value`       |

## <a name="structured-multi-turn-format-through-import"></a>가져오기를 통해 구조적 다중 전환 형식

파일 형식으로 다중 전환 대화를 만들 수 있습니다 `.tsv` . 형식은 이전 채팅 로그 (QnA Maker를 사용 하지 않는 다른 프로세스와 함께)를 분석 한 다음 자동화를 통해 파일을 만드는 기능을 제공 합니다 `.tsv` . 파일을 가져와 기존 기술 자료를 바꿉니다.

> [!div class="mx-imgBorder"]
> ![3 수준의 다중 전환 질문에 대 한 개념적 모델](../media/qnamaker-concepts-knowledgebase/nested-multi-turn.png)

다중 전환에 대 한 열에는 다중 전환에 대 한 `.tsv` **메시지가 표시** 됩니다. `.tsv`Excel에 표시 된 예제는 다중 턴 자식을 정의 하기 위해 포함할 정보를 표시 합니다.

```JSON
[
    {"displayOrder":0,"qnaId":2,"displayText":"Level 2 Question A"},
    {"displayOrder":0,"qnaId":3,"displayText":"Level 2 - Question B"}
]
```

**DisplayOrder** 은 숫자이 고 **인 displaytext** 는 markdown를 포함 하지 않아야 하는 텍스트입니다.

> [!div class="mx-imgBorder"]
> ![Excel에 표시 된 다중 전환 질문 예제](../media/qnamaker-concepts-knowledgebase/multi-turn-tsv-columns-excel-example.png)

## <a name="export-as-example"></a>예제로 내보내기

파일에서 QnA 쌍을 표시 하는 방법을 잘 모르겠으면 `.tsv` 다음을 수행 합니다.
* [GitHub에서이 다운로드 가능한 예제](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx?raw=true) 사용
* 또는 쌍을 나타내는 방법에 대 한 예제를 보려면 QnA Maker 포털에서 쌍을 만든 후 기술 자료를 내보냅니다.

## <a name="content-types-of-documents-you-can-add-to-a-knowledge-base"></a>기술 자료에 추가할 수 있는 문서 내용 유형
콘텐츠 형식에는 PDF, DOC 및 TXT와 같은 많은 표준 구조적 문서가 포함 됩니다.

### <a name="file-and-url-data-types"></a>파일 및 URL 데이터 형식

다음 표에는 QnA Maker에서 지원되는 콘텐츠 형식 및 파일 형식이 요약되어 있습니다.

|원본 유형|콘텐츠 유형| 예제|
|--|--|--|
|URL|FAQ(질문과 대답)<br> (플랫, 섹션 또는 토픽 홈페이지 포함)<br>지원 페이지 <br> (단일 페이지 방법 문서, 문제 해결 문서 등)|[일반 FAQ](../troubleshooting.md), <br>[하이퍼링크가 있는 FAQ](https://www.microsoft.com/en-us/software-download/faq),<br> [토픽 홈페이지가 있는 FAQ](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[지원 문서](./best-practices.md)|
|PDF/DOC|FAQ,<br> 제품 설명서,<br> 브로슈어,<br> 페이퍼,<br> 전단 정책,<br> 지원 가이드,<br> 구조화된 QnA,<br> 기타|**다중 전환 하지 않음**<br>[구조화 된 QnA.docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/structured.docx),<br> [Sample Product Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [샘플 semi-structured.docx](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx)<br> [샘플 흰색 paper.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf),<br><br>**다중 전환**:<br>[.Docx (Surface Pro)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Contoso 이점 (.docx)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Contoso 이점 (pdf)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)|
|* Excel|구조화된 QnA 파일<br> (RTF, HTML 지원 포함)|**다중 전환 하지 않음**:<br>[샘플 QnA FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)<br><br>**다중 전환**:<br>[구조적 단순 FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Structured-multi-turn-format.xlsx)<br>[Surface 노트북 FAQ.xls](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-Surface-Pro.xlsx)|
|* TXT/TSV|구조화된 QnA 파일|[샘플 chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

데이터 원본에 대 한 인증이 필요한 경우 다음 방법을 고려 하 여 해당 콘텐츠를 QnA Maker으로 가져와야 합니다.

* 수동으로 파일을 다운로드 하 고 QnA Maker로 가져오기
* 인증 된 [Sharepoint 위치의](../how-to/add-sharepoint-datasources.md) 파일 추가

### <a name="url-content"></a>URL 콘텐츠

QnA Maker에서 **URL** 을 통해 두 가지 형식의 문서를 가져올 수 있습니다.

* FAQ URL
* 지원 URL

각 형식은 필요한 형식을 나타냅니다.

### <a name="file-based-content"></a>파일 기반 콘텐츠

[QnA Maker 포털](https://www.qnamaker.ai)에서 공용 원본 또는 로컬 파일 시스템의 기술 자료에 파일을 추가할 수 있습니다.

### <a name="content-format-guidelines"></a>콘텐츠 형식 지침

여러 파일에 대 한 [서식 지침](../reference-document-format-guidelines.md) 에 대해 자세히 알아보세요.

## <a name="next-steps"></a>다음 단계

[QnAs를 편집](../how-to/edit-knowledge-base.md)하는 방법에 대해 알아봅니다.
