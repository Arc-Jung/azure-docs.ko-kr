---
title: SQL Data Warehouse에서 레이블을 사용하여 쿼리 계측 | Microsoft Docs
description: 솔루션 개발을 위해 Azure SQL Data Warehouse에서 레이블을 사용하여 쿼리를 계측하기 위한 팁.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: ee991fdfcd93ea064d1205d61d07adf377cce667
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480028"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse에서 레이블을 사용하여 쿼리 계측
솔루션 개발을 위해 Azure SQL Data Warehouse에서 레이블을 사용하여 쿼리를 계측하기 위한 팁.


## <a name="what-are-labels"></a>레이블이란?
SQL Data Warehouse는 쿼리 레이블이라는 개념을 지원합니다. 좀더 깊이 들어가기 전에 한 가지 예를 살펴보겠습니다.

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

마지막 줄은 쿼리에 문자열 '내 쿼리 레이블' 태그를 추가합니다. 레이블은 DMV를 통해 쿼리가 가능하므로 이 태그는 특히 유용합니다. 레이블을 쿼리하면 문제 쿼리를 찾고 ELT 실행을 통해 진행률을 파악할 수 있는 메커니즘을 제공합니다.

좋은 명명 규칙은 큰 도움이 됩니다. 예를 들어 PROJECT, PROCEDURE, STATEMENT 또는 COMMENT 문으로 레이블을 시작하면 소스 제어의 모든 코드 중에서 쿼리를 고유하게 식별하는 데 도움이 됩니다.

다음 쿼리는 동적 관리 뷰를 사용하여 레이블별로 검색을 수행합니다.

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
더 많은 개발 팁은 [개발 개요](sql-data-warehouse-overview-develop.md)를 참조하세요.


