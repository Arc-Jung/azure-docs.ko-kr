---
title: 명명 된 엔터티 인식에 대해 지원 되는 범주
titleSuffix: Azure Cognitive Services
description: 텍스트 분석 API에서 지원 되는 엔터티 범주에 대해 알아봅니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/22/2021
ms.author: aahi
ms.openlocfilehash: 883c5a20612f4dab44c0d06776ee287b27174ab9
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99097306"
---
# <a name="supported-entity-categories-in-the-text-analytics-api-v3"></a>텍스트 분석 API v3에서 지원 되는 엔터티 범주

이 문서를 사용 하 여 [명명 된 엔터티 인식](how-tos/text-analytics-how-to-entity-linking.md) (NER)에서 반환할 수 있는 엔터티 범주를 찾을 수 있습니다. NER는 예측 모델을 실행 하 여 입력 문서에서 명명 된 엔터티를 식별 하 고 분류 합니다.

NER v 3.1의 미리 보기를 사용할 수도 있습니다. 여기에는 개인 ( `PII` ) 및 상태 () 정보를 검색 하는 기능이 포함 됩니다 `PHI` . 또한 상태 탭을 클릭 **하 여 상태** 에 대 한 Text Analytics에서 지원 되는 범주 목록을 표시 합니다. 

[마이그레이션 가이드](migration-guide.md?tabs=named-entity-recognition) 의 버전 2.1에서 반환 된 유형 목록을 찾을 수 있습니다.

## <a name="entity-categories"></a>엔터티 범주

#### <a name="general"></a>[일반](#tab/general)

[!INCLUDE [supported entity types - general](./includes/entity-types/general-entities.md)]

#### <a name="pii"></a>[PII](#tab/personal)

[!INCLUDE [supported entity types - personally identifying information](./includes/entity-types/personal-information-entities.md)]

#### <a name="health"></a>[의료](#tab/health)

[!INCLUDE [biomedical entity types](./includes/entity-types/health-entities.md)]

**_

## <a name="next-steps"></a>다음 단계

_ [Text Analytics에서 명명 된 엔터티 인식을 사용 하는 방법](how-tos/text-analytics-how-to-entity-linking.md)
