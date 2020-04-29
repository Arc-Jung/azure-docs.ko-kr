---
title: N-최적 번역 반환-Translator Text
titleSuffix: Azure Cognitive Services
description: Translator Text API를 사용 하 여 N-최상의 번역을 반환 합니다.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ROBOTS: NOINDEX
ms.openlocfilehash: eff25877165ac365e0af77651147fcdd1eebe294
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73837253"
---
# <a name="how-to-return-n-best-translations"></a>N-Best 번역 반환 방법

> [!NOTE]
> 이 메서드는 더 이상 사용되지 않습니다. Translator Text API V3.0에서는 사용할 수 없습니다.

Microsoft Translator API의 GetTranslations() 및 GetTranslationsArray() 메서드에는 선택적인 부울 플래그 "IncludeMultipleMTAlternatives"가 포함됩니다.
메서드는 번역기 엔진의 N-Best 목록에서 데이터를 제공하는 최대 maxTranslations 대안을 반환합니다.

서명은 다음과 같습니다.

**구문**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**매개 변수**

| 매개 변수 | Description |
|:---|:---|
| appId | **필수** 인증 헤더를 사용하는 경우 appid 필드를 비워 두고, 사용하지 않는 경우 "Bearer" + " " + 액세스 토큰을 포함하는 문자열을 지정합니다.|
| text | **필수** 번역할 텍스트를 나타내는 문자열입니다. 텍스트 크기는 10000자를 초과하지 않아야 합니다.|
| 원본 | **필수** 번역할 텍스트의 언어 코드를 나타내는 문자열입니다. |
| to | **필수** 텍스트를 번역할 언어 코드를 나타내는 문자열입니다. |
| maxTranslations | **필수** 반환할 번역의 최대 수를 나타내는 int입니다. |
| 옵션 | **선택 사항** 아래에 나열된 값을 포함하는 TranslateOptions 개체입니다. 모두 선택 사항이며 가장 일반적인 설정이 기본값이 됩니다.

* 범주: 유일하게 지원되는 기본 옵션은 "general"입니다.
* ContentType: 유일하게 지원되는 기본 옵션은 "text/plain"입니다.
* 상태: 요청 및 응답을 상호 연결하게 하는 사용자 상태입니다. 응답에서 동일한 내용이 반환됩니다.
* IncludeMultipleMTAlternatives: MT 엔진에서 하나를 초과하는 대안을 반환할 것인지 여부를 결정하는 플래그입니다. 기본값이 False이고 1개의 대안만 포함합니다.

## <a name="ratings"></a>등급
등급은 다음과 같이 적용됩니다. 즉, 최고의 자동 번역 등급은 5입니다.
자동으로 생성된(N-Best) 번역 대안의 등급은 0이며, 일치 정도는 100입니다.

## <a name="number-of-alternatives"></a>대안 수
반환된 대안 수는 최대 maxTranslations이지만 더 작을 수도 있습니다.

## <a name="language-pairs"></a>언어 쌍
이 기능은 중국어 간체 및 번체 간 양방향 번역에 사용할 수 없습니다. Microsoft Translator의 다른 모든 지원되는 언어 쌍에 대해 사용할 수 있습니다.
