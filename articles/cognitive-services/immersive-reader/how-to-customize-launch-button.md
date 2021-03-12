---
title: 몰입 형 판독기 시작 단추를 편집 합니다.
titleSuffix: Azure Cognitive Services
description: 이 문서에서는 몰입 형 판독기를 시작 하는 단추를 사용자 지정 하는 방법을 보여 줍니다.
services: cognitive-services
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 03/08/2021
ms.author: metang
ms.openlocfilehash: d60e37a437cacda8afbe88a901089f9478a53c16
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102608617"
---
# <a name="how-to-customize-the-immersive-reader-button"></a>몰입 형 판독기 단추를 사용자 지정 하는 방법

이 문서에서는 응용 프로그램의 요구 사항에 맞게 몰입 형 판독기를 시작 하는 단추를 사용자 지정 하는 방법을 보여 줍니다.

## <a name="add-the-immersive-reader-button"></a>몰입 형 판독기 단추 추가

몰입 형 판독기 SDK는 몰입 형 판독기를 시작 하는 단추에 대 한 기본 스타일을 제공 합니다. `immersive-reader-button`이 스타일을 사용 하려면 class 특성을 사용 합니다.

```html
<div class='immersive-reader-button'></div>
```

## <a name="customize-the-button-style"></a>단추 스타일 사용자 지정

특성을 사용 `data-button-style` 하 여 단추의 스타일을 설정 합니다. 허용 되는 값은 `icon` , `text` 및 `iconAndText` 입니다. 기본값은 `icon`입니다.

### <a name="icon-button"></a>아이콘 단추

```html
<div class='immersive-reader-button' data-button-style='icon'></div>
```

그러면 다음과 같이 렌더링 됩니다.

![렌더링 된 텍스트 단추입니다.](./media/button-icon.png)

### <a name="text-button"></a>텍스트 단추

```html
<div class='immersive-reader-button' data-button-style='text'></div>
```

그러면 다음과 같이 렌더링 됩니다.

![렌더링 된 몰입 형 판독기 단추입니다.](./media/button-text.png)

### <a name="icon-and-text-button"></a>아이콘 및 텍스트 단추

```html
<div class='immersive-reader-button' data-button-style='iconAndText'></div>
```

그러면 다음과 같이 렌더링 됩니다.

![아이콘 단추](./media/button-icon-and-text.png)

## <a name="customize-the-button-text"></a>단추 텍스트 사용자 지정

특성을 사용 하 여 단추에 대 한 언어 및 대체 텍스트를 구성 합니다 `data-locale` . 기본 언어는 한국어(Korean)입니다.

```html
<div class='immersive-reader-button' data-locale='fr-FR'></div>
```

## <a name="customize-the-size-of-the-icon"></a>아이콘 크기 사용자 지정

몰입 형 판독기 아이콘의 크기는 특성을 사용 하 여 구성할 수 있습니다 `data-icon-px-size` . 이는 아이콘의 크기 (픽셀)를 설정 합니다. 기본 크기는 20px입니다.

```html
<div class='immersive-reader-button' data-icon-px-size='50'></div>
```

## <a name="next-steps"></a>다음 단계

* [몰입형 리더 SDK 참조](./reference.md) 살펴보기