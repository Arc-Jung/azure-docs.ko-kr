---
title: 번역 구성
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 다양 한 번역 옵션을 구성 하는 방법을 보여 줍니다.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: bb90cb3a86acb6a94fa92c33d78ec596659e105f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102608702"
---
# <a name="how-to-configure-translation"></a>번역을 구성 하는 방법

이 문서에서는 몰입 형 판독기에서 다양 한 번역 옵션을 구성 하는 방법을 보여 줍니다.

## <a name="configure-translation-language"></a>번역 언어 구성

`options`매개 변수는 번역을 구성 하는 데 사용할 수 있는 모든 플래그를 포함 합니다. `language`매개 변수를 변환 하려는 언어로 설정 합니다. 지원 되는 언어의 전체 목록은 [언어 지원](./language-support.md) 을 참조 하세요.

```typescript
const options = {
    translationOptions: {
        language: 'fr-FR'
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="automatically-translate-the-document-on-load"></a>로드 시 자동으로 문서 변환

`autoEnableDocumentTranslation` `true` 몰입 형 판독기가 로드 될 때 전체 문서를 자동으로 변환할 수 있도록 하려면로 설정 합니다.

```typescript
const options = {
    translationOptions: {
        autoEnableDocumentTranslation: true
    }
};
```

## <a name="automatically-enable-word-translation"></a>자동으로 단어 번역 사용

`autoEnableWordTranslation` `true` 단일 단어 변환을 사용 하려면로 설정 합니다.

```typescript
const options = {
    translationOptions: {
        autoEnableWordTranslation: true
    }
};
```

## <a name="next-steps"></a>다음 단계

* [몰입형 리더 SDK 참조](./reference.md) 살펴보기