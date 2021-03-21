---
title: Azure 앱 구성 REST API-일관성
description: Azure 앱 구성을 사용 하 여 실시간 일관성을 유지 하기 위한 참조 페이지 REST API
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: b324d23ce9abc1eb3893f316365aff828de2063d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96932628"
---
# <a name="real-time-consistency"></a>실시간 일관성

일부 분산 시스템의 특성으로 인해 요청 간의 실시간 일관성은 암시적으로 적용 하기 어렵습니다. 솔루션은 여러 동기화 토큰의 형태로 프로토콜 지원을 허용 하는 것입니다. 동기화 토큰은 선택 사항입니다.

## <a name="initial-request"></a>초기 요청

서로 다른 클라이언트 인스턴스와 요청 간에 실시간 일관성을 보장 하려면 선택적 `Sync-Token` 요청 및 응답 헤더를 사용 합니다.

구문

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|매개 변수|설명|
|--|--|
| `<id>` | 토큰 ID (불투명) |
| `<value>` | 토큰 값 (불투명)입니다. Base64 인코딩 문자열을 허용 합니다. |
| `<sn>` | 토큰 시퀀스 번호 (버전)입니다. 상위는 동일한 토큰의 최신 버전을 의미 합니다. 를 사용 하면 동시성 및 클라이언트 캐싱이 향상 됩니다. 토큰 버전이 포함 되어 있으므로 클라이언트는 토큰의 마지막 버전만 사용 하도록 선택할 수 있습니다. 이 매개 변수는 요청에 필요 하지 않습니다. |

## <a name="response"></a>응답

서비스는 `Sync-Token` 각 응답과 함께 헤더를 제공 합니다.

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>후속 요청

모든 후속 요청은 제공 된와 관련 하 여 실시간으로 일관 된 응답을 보장 `Sync-Token` 합니다.

```http
Sync-Token: <id>=<value>
```

요청에서 헤더를 생략 하는 경우 `Sync-Token` 서비스는 내부적으로 사용 하기 전에 짧은 기간 (최대 몇 초) 동안 캐시 된 데이터를 사용 하 여 응답할 수 있습니다. 읽기 직전에 변경 내용이 발생 하면이 동작으로 인해 읽기가 일치 하지 않을 수 있습니다.

## <a name="multiple-sync-tokens"></a>다중 동기화-토큰

서버는 단일 요청에 대해 여러 동기화 토큰을 사용 하 여 응답할 수 있습니다. 다음 요청에 대 한 실시간 일관성을 유지 하기 위해 클라이언트는 수신 된 모든 동기화 토큰을 사용 하 여 응답 해야 합니다. 여러 헤더 값은 쉼표로 구분 해야 합니다.

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
