---
title: 데이터 내보내기 또는 삭제 - Custom Vision Service
titleSuffix: Azure Cognitive Services
description: 데이터에 대 한 모든 권한을 유지 합니다. 이 문서에서는 Custom Vision Service에서 데이터를 확인, 내보내기 또는 삭제 하는 방법을 설명 합니다.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: pafarley
ms.openlocfilehash: 82d9f4508db376ebbe69ef772c15fb732391a31d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "73718974"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Custom Vision에서 사용자 데이터 내보내기 또는 삭제

Custom Vision은 서비스를 작동 하는 사용자 데이터를 수집 하지만, 고객은 Custom Vision [학습 api](https://go.microsoft.com/fwlink/?linkid=865446)를 사용 하 여 데이터 보기, 내보내기 및 삭제를 완전히 제어할 수 있습니다.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Custom Vision에서 사용자 데이터를 내보내고 삭제 하는 방법에 대 한 자세한 내용은 다음 표를 참조 하세요.

| 데이터 | 내보내기 작업 | 삭제 작업 |
| ---- | ---------------- | ---------------- |
| 계정 정보(구독 키) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Azure Portal(Azure 구독)을 사용하여 삭제합니다. 또는 CustomVision.ai 설정 페이지에서 "계정 삭제" 단추를 사용합니다(Microsoft 계정 구독). | 
| 반복 세부 정보 | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| 반복 성능 세부 정보 | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| 반복 목록 | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| 프로젝트 및 프로젝트 세부 정보 | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) 및 [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| 이미지 태그 | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) 및 [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| 이미지 | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446)(이미지 다운로드를 위한 URI 제공) 및 [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446)(이미지 다운로드를 위한 URI 제공) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| 내보낸 모델 | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | 계정 삭제 시 삭제됨 |
