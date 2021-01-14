---
title: Form Recognizer란?
titleSuffix: Azure Cognitive Services
description: Azure Form Recognizer 서비스를 사용하면 양식 문서에서 키/값 쌍 및 테이블 데이터를 식별 및 추출할 수 있을 뿐만 아니라 판매 영수증 및 비즈니스 카드에서 주요 정보를 추출할 수 있습니다.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 11/23/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: 자동화된 데이터 처리, 문서 처리, 자동화된 데이터 입력, 양식 처리
ms.openlocfilehash: e1e5a4abf8eab96af62b160e28f98d95cf527eaf
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98044767"
---
# <a name="what-is-form-recognizer"></a>Form Recognizer란?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Form Recognizer는 기계 학습 기술을 사용하여 자동화된 데이터 처리 소프트웨어를 빌드할 수 있는 인식 서비스입니다. 문서에서 텍스트, 키/값 쌍, 선택 표시, 표 및 구조를 식별하고 추출합니다. &mdash;이 서비스는 원본 파일의 관계, 경계 상자, 신뢰도 등을 포함하는 구조화된 데이터를 출력합니다. 많은 수동 작업 또는 광범위한 데이터 과학 전문 지식 없이도 특정 콘텐츠에 맞게 조정된 정확한 결과를 빠르게 얻을 수 있습니다. Form Recognizer를 사용하여 애플리케이션에서 데이터 입력을 자동화하고 문서 검색 기능을 보강합니다.

Form Recognizer는 사용자 지정 문서 처리 모델, 송장, 영수증 및 명함용으로 미리 빌드된 모델, 레이아웃 모델로 구성됩니다. REST API 또는 클라이언트 라이브러리 SDK를 사용하여 Form Recognizer 모델을 호출하여 복잡성을 줄이고 워크플로 또는 애플리케이션에 통합할 수 있습니다.

Form Recognizer는 다음과 같은 서비스로 구성됩니다.
* **[레이아웃 API](#layout-api)** - 문서에서 경계 상자 좌표와 함께 텍스트, 선택 표시 및 표 구조를 추출합니다.
* **[사용자 지정 모델](#custom-models)** - 양식에서 텍스트, 키/값 쌍, 선택 표시 및 표 데이터를 추출합니다. 이러한 모델은 사용자 고유의 데이터로 학습되므로 사용자의 양식에 맞게 조정됩니다.
* **[미리 빌드된 모델](#prebuilt-models)** - 미리 빌드된 모델을 사용하여 고유한 양식 유형에서 데이터를 추출합니다. 현재 사용할 수 있는 미리 빌드된 모델은 다음과 같습니다.
    * [송장](./concept-invoices.md)
    * [판매 영수증](./concept-receipts.md)
    * [명함](./concept-business-cards.md)


## <a name="try-it-out"></a>사용해 보기

Form Recognizer 서비스를 사용해보려면 다음과 같은 온라인 샘플 UI 도구로 이동합니다.


# <a name="v20"></a>[v2.0](#tab/v2-0)
> [!div class="nextstepaction"]
> [Form Recognizer 사용해보기](https://fott.azurewebsites.net/)

# <a name="v21-preview"></a>[v2.1 미리 보기](#tab/v2-1)
> [!div class="nextstepaction"]
> [Form Recognizer 사용해보기](https://fott-preview.azurewebsites.net/)

---

Form Recognizer 서비스를 사용해보려면 Azure 구독([무료로 하나 생성](https://azure.microsoft.com/free/cognitive-services))과 [Form Recognizer 리소스](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) 엔드포인트 및 키가 필요합니다.

## <a name="layout-api"></a>레이아웃 API

Form Recognizer는 고해상도 OCR(광학 문자 인식)과 문서의 향상된 딥 러닝 모델을 사용하여 텍스트, 선택 표시 및 표 구조(텍스트와 관련된 행 및 열 번호)를 추출할 수 있습니다. 자세한 내용은 [레이아웃](./concept-layout.md) 개념 가이드를 참조하세요.

:::image type="content" source="./media/tables-example.jpg" alt-text="표 예" lightbox="./media/tables-example.jpg":::

## <a name="custom-models"></a>사용자 지정 모델

Form Recognizer 사용자 지정 모델은 사용자 고유의 데이터로 학습하며, 샘플 입력 양식 5개만 있으면 시작할 수 있습니다. 학습된 문서 처리 모델은 원본 양식 문서의 관계가 포함된 정형 데이터를 출력할 수 있습니다. 모델이 학습되면 데이터를 더 많은 양식에서 안정적으로 추출하기 위해 필요에 따라 모델을 테스트, 재학습 및 최종적으로 사용할 수 있습니다.

사용자 지정 모델을 학습할 때 레이블 지정 데이터를 사용할 수도 있고 사용하지 않을 수도 있습니다.

### <a name="train-without-labels"></a>레이블 없이 학습

기본적으로 Form Recognizer는 자율 학습을 사용하여 양식의 필드와 항목 간의 레이아웃 및 관계를 이해합니다. 입력 양식을 제출하면 알고리즘에서 형식별로 양식을 클러스터링하고, 존재하는 키와 테이블을 검색하고, 값을 키에 연결하고, 항목을 테이블에 연결합니다. 이 과정에서 수동으로 데이터 레이블을 지정하거나 과도한 코딩 및 유지 관리가 필요 없으므로 이 방법을 먼저 시도해 볼 것을 권장합니다.

학습 문서를 수집하는 방법에 대한 팁은 [학습 데이터 세트 빌드](./build-training-data-set.md)를 참조하세요.

### <a name="train-with-labels"></a>레이블을 사용하여 학습

레이블 지정 데이터로 학습하는 경우 모델은 감독 학습을 통해 관심 있는 값을 추출하며, 사용자가 제공하는 레이블 지정 양식을 사용합니다. 이 방법은 모델 성능이 향상되며, 복잡한 양식 또는 키 없는 값을 포함하는 양식과 함께 작동하는 모델을 생성할 수 있습니다.

Form Recognizer는 [레이아웃 API](#layout-api)를 사용하여 인쇄 및 필기된 텍스트 요소의 예상 크기와 위치를 학습합니다. 그 후 사용자 지정 레이블을 사용하여 문서의 키/값 연결을 학습합니다. 새 모델을 학습시킬 때 동일한 유형(동일한 구조)의 수동 레이블 지정 양식 5개를 사용하여 시작한 후 필요에 따라 레이블 지정 데이터를 추가하여 모델 정확도를 개선하는 것이 좋습니다.

[레이블로 학습 시작](./quickstarts/label-tool.md)


> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Azure-Form-Recognizer/player]


## <a name="prebuilt-models"></a>미리 빌드된 모델

Form Recognizer에는 고유한 양식 유형의 자동화된 데이터 처리를 위한 미리 빌드된 모델도 포함되어 있습니다.

### <a name="prebuilt-invoice-model"></a>미리 빌드된 송장 모델
미리 빌드된 송장 모델은 다양한 형식의 청구서에서 데이터를 추출하고 구조화된 데이터를 반환합니다. 이 모델은 송장 ID, 고객 세부 정보, 공급 업체 세부 정보, 배송지, 청구지, 합계, 세금, 소계 등과 같은 주요 정보를 추출합니다. 또한 미리 빌드된 송장 모델은 송장의 모든 텍스트와 표를 인식하고 반환하도록 학습됩니다. 자세한 내용은 [송장](./concept-invoices.md) 개념 가이드를 참조하세요.

:::image type="content" source="./media/overview-invoices.jpg" alt-text="샘플 송장" lightbox="./media/overview-invoices.jpg":::

### <a name="prebuilt-receipt-model"></a>미리 빌드된 영수증 모델

미리 빌드된 영수증 모델은 오스트레일리아, 캐나다, 영국, 인도, 미국&mdash;(식당, 주유소, 소매점 등)에서 사용되는 유형의 영어 판매 영수증을 읽는 데 사용됩니다. 이 모델은 트랜잭션 시간 및 날짜, 판매자 정보, 세금의 합계, 줄 항목, 총액 등과 같은 주요 정보를 추출합니다. 또한 미리 빌드된 영수증 모델은 영수증의 모든 텍스트를 인식하고 반환하도록 학습됩니다. 자세한 내용은 [영수증](./concept-receipts.md) 개념 가이드를 참조하세요.

:::image type="content" source="./media/overview-receipt.jpg" alt-text="샘플 영수증" lightbox="./media/overview-receipt.jpg":::

### <a name="prebuilt-business-cards-model"></a>미리 빌드된 명함 모델

명함 모델을 사용하면 영어로 된 명함에서 사람의 이름, 직함, 주소, 이메일, 회사 및 전화번호와 같은 정보를 추출할 수 있습니다. 자세한 내용은 [비즈니스 카드](./concept-business-cards.md) 개념 가이드를 참조하세요.

:::image type="content" source="./media/overview-business-card.jpg" alt-text="샘플 명함" lightbox="./media/overview-business-card.jpg":::


## <a name="get-started"></a>시작하기

[샘플 Form Recognizer 도구](https://fott.azurewebsites.net/)를 사용하거나 빠른 시작에 따라 양식에서 데이터 추출을 시작합니다. 기술을 학습할 때 체험판 서비스를 이용하는 것이 좋습니다. 체험판 페이지는 한 달에 500페이지로 제한됩니다.

* [클라이언트 라이브러리 / REST API 빠른 시작](./quickstarts/client-library.md)(모든 언어, 여러 시나리오)
* 웹 UI 빠른 시작
  * [레이블을 사용하여 학습 - 샘플 레이블 지정 도구](quickstarts/label-tool.md)
* REST 샘플(GitHub)
 * 문서에서 텍스트, 선택 표시 및 표 구조 추출
    * [레이아웃 데이터 추출 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-layout.md)
  * 사용자 지정 모델 학습 및 양식 데이터 추출
    * [레이블 없이 학습 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-train-extract.md)
    * [레이블을 사용하여 학습 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-labeled-data.md)
  * 송장에서 데이터 추출
    * [송장 데이터 추출 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-invoices.md)
  * 판매 영수증에서 데이터 추출
    * [영수증 데이터 추출 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-receipts.md)
  * 비즈니스 카드에서 데이터 추출
    * [명함 데이터 추출 - Python](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/FormRecognizer/rest/python-business-cards.md)

### <a name="review-the-rest-apis"></a>REST API 검토

다음 API를 사용하여 모델을 학습시키고 양식에서 정형 데이터를 추출합니다.

|Name |Description |
|---|---|
| **레이아웃 분석** | 스트림으로 전달된 문서를 분석하여 문서에서 텍스트, 선택 표시, 표 및 구조 추출 |
| **사용자 지정 모델 학습**| 동일한 형식의 5개 양식을 사용하여 양식을 분석하는 새 모델을 학습시킵니다. _useLabelFile_ 매개 변수를 `true`로 설정하여 수동 레이블 지정 데이터로 학습합니다. |
| **양식 분석** |스트림으로 전달된 양식을 분석하여 사용자 지정 모델을 통해 양식에서 텍스트, 키/값 쌍 및 표를 추출합니다.  |
| **송장 분석** | 송장을 분석하여 주요 정보, 표 및 기타 송장 텍스트를 추출합니다.|
| **영수증 분석** | 영수증 문서를 분석하여 키 정보 및 다른 영수증 텍스트를 추출합니다.|
| **명함 분석** | 명함을 분석하여 주요 정보 및 텍스트를 추출합니다.|


# <a name="v20"></a>[v2.0](#tab/v2-0)
자세히 알아보려면 [REST API 참조 설명서](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2/operations/AnalyzeWithCustomForm)를 확인하세요. 이전 버전의 API에 대해 잘 알고 있는 경우에는 [새로운 기능](./whats-new.md) 문서를 읽고 최신 변경 내용에 대해 알아보세요.

# <a name="v21"></a>[v2.1](#tab/v2-1)
자세히 알아보려면 [REST API 참조 설명서](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/AnalyzeWithCustomForm)를 확인하세요. 이전 버전의 API에 대해 잘 알고 있는 경우에는 [새로운 기능](./whats-new.md) 문서를 읽고 최신 변경 내용에 대해 알아보세요.

---

## <a name="input-requirements"></a>입력 요구 사항

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="deploy-on-premises-using-docker-containers"></a>Docker 컨테이너를 사용하여 온-프레미스 배포

[Form Recognizer 컨테이너(미리 보기)를 사용](form-recognizer-container-howto.md)하여 온-프레미스에 API 기능을 배포합니다. 이 Docker 컨테이너는 규정 준수, 보안 또는 기타 운영상의 이유로 서비스를 데이터에 더 가깝게 가져올 수 있습니다. 

## <a name="service-availability-and-redundancy"></a>서비스 가용성 및 중복성

### <a name="is-form-recognizer-service-zone-resilient"></a>Form Recognizer 서비스가 영역 복원력이 있습니까?

예. Form Recognizer 서비스는 기본적으로 영역 복원력이 있습니다.

### <a name="how-do-i-configure-the-form-recognizer-service-to-be-zone-resilient"></a>영역 복원력을 갖도록 Form Recognizer 서비스를 구성하려면 어떻게 해야 하나요?

영역 복원력을 사용하도록 설정하기 위한 고객 구성은 필요 없습니다. Form Recognizer 리소스의 영역 복원력은 기본적으로 제공되며 서비스 자체에서 관리합니다.


## <a name="data-privacy-and-security"></a>데이터 개인 정보 보호 및 보안

모든 Cognitive Services와 마찬가지로 Form Recognizer 서비스를 사용하는 개발자는 고객 데이터에 대한 Microsoft 정책을 알고 있어야 합니다. Microsoft Trust Center의 [Cognitive Services 페이지](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices)에서 자세히 알아보세요.

## <a name="next-steps"></a>다음 단계

사용자가 선택한 언어로 Form Recognizer를 사용하여 양식 처리 앱 작성을 시작하려면 [빠른 시작](quickstarts/client-library.md)을 완료하세요.
