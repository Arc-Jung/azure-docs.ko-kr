---
title: 동적 사전 변환기
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 Azure Cognitive Services Translator의 동적 사전 기능을 사용 하는 방법을 설명 합니다.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: ebfccb6fed46db65f6a70937516552f5280c8016
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98897767"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>동적 사전을 사용 하는 방법

단어나 구에 적용할 번역을 이미 알고 있는 경우 요청 내에 태그로 제공할 수 있습니다. 동적 사전은 적절 한 이름 및 제품 이름과 같은 복합 명사에만 안전 합니다.

**구문:**

<mstrans: dictionary translation = "구 변환" >구</mstrans: dictionary>

**요구 사항:**

* `From`및 `To` 언어는 영어와 다른 지원 되는 언어를 포함 해야 합니다. 
* 자동 `From` 검색 기능을 사용 하는 대신 API 번역 요청에 매개 변수를 포함 해야 합니다. 

**예제: en-de:**

원본 입력: `The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

대상 출력: `Das Wort "wordomatic" ist ein Wörterbucheintrag.`

이 기능은 HTML 모드를 사용할 때와 그렇지 않을 때 같은 결과를 가져옵니다.

이 기능을 사용 하는 경우에만 사용 합니다. 변환을 사용자 지정 하는 더 좋은 방법은 사용자 지정 번역기를 사용 하는 것입니다. Custom Translator는 컨텍스트 및 통계적 확률을 최대한 활용합니다. 컨텍스트에 따라 사용자 작업 또는 구를 보여 주는 학습 데이터를 보유하고 있거나 이러한 데이터를 만들 수 있으면 훨씬 더 나은 결과를 얻을 수 있습니다. 에서 사용자 지정 변환기에 대 한 자세한 정보를 찾을 수 있습니다 [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator) .