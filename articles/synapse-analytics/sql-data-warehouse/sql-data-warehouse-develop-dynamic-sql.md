---
title: 동적 SQL 사용
description: 솔루션 개발을 위한 Azure SQL Data Warehouse의 동적 SQL 사용 팁.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a44bec72029a50c2ef348bcdda497803e35f586d
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350547"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL Data Warehouse의 동적 SQL
솔루션 개발을 위한 Azure SQL Data Warehouse의 동적 SQL 사용 팁.

## <a name="dynamic-sql-example"></a>동적 SQL 예제

SQL Data Warehouse용 애플리케이션 코드를 개발할 때 유연하고 일반적인 모듈식 솔루션을 제공하기 위해 동적 SQL을 사용해야 할 수 있습니다. SQL Data Warehouse는 현재 Blob 데이터 형식을 지원하지 않습니다. Blob 데이터 형식을 지원하지 않으면 Blob 데이터 형식에 varchar(max) 및 nvarchar(max) 형식이 둘 다 포함되므로 문자열 크기가 제한될 수 있습니다. 매우 큰 문자열을 작성할 때 이러한 형식을 애플리케이션 코드에 사용하는 경우 코드를 청크로 나누고 EXEC 문을 대신 사용해야 합니다.

간단한 예는 다음과 같습니다.

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

문자열이 짧은 경우 일반적으로 [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql)을 사용할 수 있습니다.

> [!NOTE]
> 동적 SQL로 실행되는 문은 모든 TSQL 유효성 검사 규칙에 적용됩니다.
> 
> 

## <a name="next-steps"></a>다음 단계
더 많은 개발 팁은 [개발 개요](sql-data-warehouse-overview-develop.md)를 참조하세요.

