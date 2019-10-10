---
title: 기능-LUIS
titleSuffix: Azure Cognitive Services
description: 언어 모델에 기능을 추가하여 레이블을 지정하거나 분류하려는 입력을 인식하는 방법에 대한 힌트를 제공합니다.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dab4b4c6f41a95623a40e5d3fd859f9613afac27
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949609"
---
# <a name="phrase-list-features-in-your-luis-app"></a>LUIS 앱의 구문 목록 기능

기계 학습에서 *기능*은 시스템이 관찰하는 데이터의 구별되는 특성입니다. 

언어 모델에 기능을 추가하여 레이블을 지정하거나 분류하려는 입력을 인식하는 방법에 대한 힌트를 제공합니다. 기능을 사용하면 LUIS가 의도와 엔터티를 둘 다 인식할 수 있지만, 기능은 의도 또는 엔터티 자체가 아닙니다. 대신에, 기능은 관련 용어의 예제를 제공할 수 있습니다.  

## <a name="what-is-a-phrase-list-feature"></a>구문 목록 기능이란?
구문 목록은 앱에 대한 중요도가 발화의 다른 단어보다 높은 단어 또는 구문 목록입니다. 구문 목록은 해당 단어와 관련된 LUIS에 대한 추가 신호로 앱 도메인의 어휘에 추가됩니다. LUIS가 이러한 값 중 하나에 대해 학습하는 내용이 다른 값에도 자동으로 적용됩니다. 이 목록은 정확하게 일치하는 텍스트의 닫힌 [목록 엔터티](luis-concept-entity-types.md#types-of-entities)가 아닙니다.

구문 목록은 형태소 분석을 지원하지 않으므로 중요한 어휘 단어 및 구문에 다양한 형태소 분석을 사용하는 발화 예제를 추가해야 합니다.

## <a name="phrase-lists-help-all-models"></a>구문 목록은 모든 모델에 도움이 됩니다.

구문 목록은 특정 의도 또는 엔터티에 연결되는 것이 아니라 모든 의도와 엔터티에 도움을 주기 위해 추가됩니다. 의도 검색과 엔터티 분류를 향상시키는 것이 목적입니다.

## <a name="how-to-use-phrase-lists"></a>구문 목록 사용 방법

앱에 중요 한 단어나 구가 앱에 있는 경우 다음과 같이 [문구 목록을 만듭니다](luis-how-to-add-features.md) .

* 산업 용어
* 속어
* 약어
* 회사별 언어
* 다른 언어에서 왔지만 앱에서 자주 사용되는 언어
* 예제 발화의 핵심 단어 및 구문

단어 또는 구문을 입력했으면 **권장** 기능을 사용하여 관련 값을 찾습니다. 관련 값을 검토한 후 구문 목록 값에 추가합니다.

용어 목록은 동의어 인 값에 대 한 것입니다. 예를 들어 모든 물줄기를 찾고 싶은데 다음과 같은 예제 발화가 있는 경우: 

* 오대호와 가까운 도시는 어디인가요? 
* 하바수강을 따라 이어지는 도로는 무엇인가요?
* 나일강은 어디서 시작해서 어디서 끝나나요? 

물줄기와 관계 없이 각 발화의 의도와 엔터티를 확인해야 합니다. 

* [bodyOfWater]와(과) 가까운 도시는 어디인가요?
* [bodyOfWater]을(를) 따라 이어지는 도로는 무엇인가요?
* [bodyOfWater]은(는) 어디서 시작해서 어디서 끝나나요? 

워터 마크 본문의 단어나 구는 동의어 이며 길이 발언에서 교대로 사용할 수 있습니다. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>지원 되는 간단한 엔터티를 식별 하는 데 도움이 되는 문구 목록
서로 교환 가능한 구 목록은 LUIS 앱의 성능을 조정하는 좋은 방법입니다. 앱이 올바른 의도에 대한 발화를 예측하거나 엔터티를 인식하는 데 문제가 있는 경우에는 발화에 비정상적인 단어나 의미가 모호할 수 있는 단어가 포함되어 있는지 생각해 보세요. 이러한 단어는 구문 목록에 포함할 좋은 후보입니다.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>구문 목록은 컨텍스트를 보다 잘 이해하여 의도를 식별하는 데 도움이 됨
구문 목록은 엄격한 일치를 수행하거나 항상 구문 목록의 모든 용어에 똑같은 레이블을 지정하기 위한 LUIS에 대한 명령이 아닙니다. 힌트일 뿐입니다. 예를 들어, “Patti” 및 “Selma”가 이름임을 나타내는 구 목록이 있을 수 있지만, LUIS는 여전히 컨텍스트 정보를 사용하여 “Make a reservation for 2 at Patti's Diner for dinner”(Patti의 저녁 식사 손님으로 2명 예약) 및 “Find me driving directions to Selma, Georgia”(조지아주 셀마(Selma)로 향하는 주행 방향 찾기)에서 해당 이름이 서로 다른 것을 의미함을 인식할 수 있습니다. 

의도에 더 많은 예제 발화를 추가하는 대신 구문 목록을 추가할 수 있습니다. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>구문 목록 및 목록 엔터티를 사용하는 경우
구 목록과 [목록 엔터티](reference-entity-list.md) 는 모두 길이 발언에 영향을 줄 수 있지만 각각은 다른 방식으로이를 수행 합니다. 구문 목록을 사용하면 의도 예측 점수에 영향을 줍니다. 목록 엔터티를 사용하면 정확한 텍스트 일치를 위해 엔터티 추출에 영향을 줍니다. 

### <a name="use-a-phrase-list"></a>구문 목록 사용
구문 목록을 기반으로 LUIS는 여전히 컨텍스트를 고려하고 비슷하지만 정확히 일치하지는 않는 항목을 목록의 항목으로 식별하도록 일반화될 수 있습니다. LUIS 앱이 범주의 새 항목을 일반화하고 식별할 수 있게 하려면 구문 목록을 사용합니다. 

새 연락처의 이름 또는 새 제품을 인식 해야 하는 인벤토리 앱을 인식 해야 하는 모임 스케줄러와 같이 엔터티의 새 인스턴스를 인식할 수 있도록 하려면 간단한 엔터티와 같은 다른 유형의 시스템 학습 엔터티를 사용 합니다. 그런 다음, LUIS가 엔터티와 유사한 다른 단어를 찾는 데 도움이 되는 단어 및 구문의 구문 목록을 만듭니다. 이 목록은 해당 단어의 값에 중요도를 추가하여 엔터티 예제를 인식하도록 LUIS를 안내합니다. 

구문 목록은 의도 및 엔터티 모두의 이해 품질을 개선하도록 도와주는 도메인별 어휘와 비슷합니다. 일반적으로 구문 목록은 도시 이름과 같은 고유 명사에 사용됩니다. 도시 이름은 하이픈이나 아포스트로피를 포함한 여러 단어일 수 있습니다.
 
### <a name="dont-use-a-phrase-list"></a>구문 목록 사용 안 함 
목록 엔터티는 엔터티가 사용할 수 있는 모든 값을 명시적으로 정의하고 정확히 일치하는 값만 식별합니다. 엔터티의 모든 인스턴스가 알려지고 자주 변경되지 않는 앱의 경우 목록 엔터티가 적절할 수 있습니다. 자주 변경되지 않는 음식점 메뉴의 음식 항목을 예로 들 수 있습니다. 엔터티와 정확히 일치하는 텍스트가 필요한 경우, 구문 목록을 사용하지 마세요. 

## <a name="best-practices"></a>모범 사례
[모범 사례](luis-concept-best-practices.md)를 알아봅니다.

## <a name="next-steps"></a>다음 단계

LUIS 앱에 기능을 추가하는 방법에 대해 자세히 알아보려면 [기능 추가](luis-how-to-add-features.md)를 참조하세요.
