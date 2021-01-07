---
title: Synapse SQL에서 쿼리 레이블 사용
description: 이 문서에는 Synapse SQL에서 쿼리 레이블을 사용 하기 위한 필수 팁이 포함 되어 있습니다.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 59fa68d12f1d8be598810399fc5623c2af983979
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96462253"
---
# <a name="use-query-labels-in-synapse-sql"></a>Synapse SQL에서 쿼리 레이블 사용

이 문서에는 Synapse SQL에서 쿼리 레이블을 사용 하기 위한 필수 팁이 포함 되어 있습니다.

> [!NOTE]
> 서버를 사용 하지 않는 SQL 풀은 labelling 쿼리를 지원 하지 않습니다.

## <a name="what-are-query-labels"></a>쿼리 레이블 이란?

전용 SQL 풀은 쿼리 레이블 이라는 개념을 지원 합니다. 좀더 깊이 들어가기 전에 한 가지 예를 살펴보겠습니다.

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

마지막 줄은 쿼리에 문자열 '내 쿼리 레이블' 태그를 추가합니다. 이 태그는 해당 레이블이 Dmv를 통해 쿼리 가능 하므로 유용 합니다. 레이블 쿼리는 문제 쿼리를 찾고 ELT 실행을 통해 진행률을 식별 하는 데 도움이 되는 메커니즘을 제공 합니다.

좋은 명명 규칙이 가장 유용 합니다. 예를 들어 프로젝트, 프로시저, 문 또는 주석을 사용 하 여 레이블을 시작 하면 소스 제어의 모든 코드에서 쿼리를 고유 하 게 식별 합니다.

다음 쿼리에서는 동적 관리 뷰를 사용 하 여 레이블로 검색 합니다.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> 쿼리할 때 반드시 단어 레이블을 대괄호 또는 큰따옴표로 묶어야 합니다. 레이블은 예약어이므로 구분하지 않으면 오류가 발생합니다. 
> 
> 

## <a name="next-steps"></a>다음 단계
더 많은 개발 팁은 [개발 개요](develop-overview.md)를 참조하세요.


