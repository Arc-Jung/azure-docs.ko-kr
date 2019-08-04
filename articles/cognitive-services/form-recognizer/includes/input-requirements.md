---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 17dc32f8948387b90229d3c4c07102cff98e3018
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562700"
---
Form Recognizer는 다음 요구 사항을 충족하는 입력 문서에서 작동합니다.

* JPG, PNG 또는 PDF 형식(텍스트 또는 스캔)이어야 합니다. 문자 추출 및 위치에 오류가 발생할 가능성이 없으므로 텍스트가 포함된 PDF가 가장 좋습니다.
* PDF가 암호로 잠긴 경우에는 제출하기 전에 잠금을 제거해야 합니다.
* 파일 크기는 4MB 미만이어야 합니다.
* 이미지에 대한 크기는 50 x 50 픽셀에서 4,200 x 4,200 픽셀 사이여야 합니다.
* 종이 문서에서 스캔한 경우 양식은 고품질 스캔이어야 합니다.
* 텍스트는 라틴어 알파벳(영어 문자)을 사용해야 합니다.
* 데이터는 키와 값을 포함해야 합니다.
* 키는 값의 위쪽 또는 왼쪽에 표시될 수 있지만, 아래쪽 또는 오른쪽에는 표시될 수 없습니다.

Form Recognizer에서 현재 지원하지 않는 입력 데이터의 유형은 다음과 같습니다.

* 복합 테이블(중첩된 테이블, 병합된 헤더 또는 셀 등)
* 확인란 또는 라디오 단추
* 50페이지가 넘는 PDF 문서