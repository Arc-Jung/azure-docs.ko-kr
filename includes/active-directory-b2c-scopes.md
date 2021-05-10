---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: bb004afe6cdd6992f742877318426052d1bead6d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106073212"
---
#### <a name="app-registrations"></a>[앱 등록](#tab/app-reg-ga/) 

1. **앱 등록** 을 선택합니다.
1. *webapi1* 애플리케이션을 선택하여 해당 **개요** 페이지를 엽니다.
1. **관리** 에서 **API 표시** 를 선택합니다.
1. **애플리케이션 ID URI** 옆에 있는 **설정** 링크를 선택합니다.
1. 기본값(GUID)을 `api`로 바꾼 다음, **저장** 을 선택합니다. `https://your-tenant-name.onmicrosoft.com/api` 형식의 전체 URI가 표시됩니다. 웹 애플리케이션이 API에 대한 액세스 토큰을 요청하는 경우 API에 대해 정의한 각 범위의 접두사로 이 URI가 추가됩니다.
1. **이 API에서 정의한 범위** 에서 **범위 추가** 를 선택합니다.
1. 다음 값을 입력하여 API에 대한 읽기 권한을 정의하는 범위를 만든 다음, **범위 추가** 를 선택합니다.
    1. **범위 이름**: `demo.read`
    1. **관리자 동의 표시 이름**: `Read access to demo API`
    1. **관리자 동의 설명**: `Allows read access to the demo API`
1. **범위 추가** 를 선택하고 다음 값을 입력하여 API에 대한 쓰기 권한을 정의하는 범위를 추가하고 **범위 추가** 를 선택합니다.
    1. **범위 이름**: `demo.write`
    1. **관리자 동의 표시 이름**: `Write access to demo API`
    1. **관리자 동의 설명**: `Allows write access to the demo API`

#### <a name="applications-legacy"></a>[애플리케이션(레거시)](#tab/applications-legacy/)

1. **애플리케이션(레거시)** 을 선택합니다.
1. *webapi1* 애플리케이션을 선택하여 해당 **속성** 페이지를 엽니다.
1. **게시된 범위** 를 선택합니다. 게시된 범위는 클라이언트 애플리케이션에 웹 API에 대한 특정 권한을 부여하는 데 사용할 수 있습니다.
1. **범위** 에 대해 `demo.read`를 입력하고, **설명** 에 대해 `Read access to the web API`를 입력합니다.
1. **범위** 에 대해 `demo.write`를 입력하고, **설명** 에 대해 `Write access to the web API`를 입력합니다.
1. **저장** 을 선택합니다.