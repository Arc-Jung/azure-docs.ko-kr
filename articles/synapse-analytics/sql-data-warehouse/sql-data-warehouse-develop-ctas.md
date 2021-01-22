---
title: CREATE TABLE AS SELECT (CTAS)
description: 솔루션 개발을 위한 Synapse SQL의 CTAS (CREATE TABLE SELECT) 문에 대 한 설명 및 예제입니다.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/26/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019, azure-synapse
ms.openlocfilehash: 6750f010e3992a2b76cc688449ad44efa7ec76d0
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98683276"
---
# <a name="create-table-as-select-ctas"></a>CREATE TABLE AS SELECT (CTAS)

이 문서에서는 Synapse SQL에서 솔루션 개발을 위한 CTAS (CREATE TABLE AS SELECT) T-sql 문을 설명 합니다. 코드 예제도 제공합니다.

## <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

Ctas ( [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) ) 문은 사용할 수 있는 가장 중요 한 t-sql 기능 중 하나입니다. CTAS는 SELECT 문의 출력을 기반으로 새 테이블을 만드는 병렬 작업입니다. CTAS는 단일 명령을 사용 하 여 테이블에 데이터를 만들고 삽입 하는 가장 간단 하 고 빠른 방법입니다.

## <a name="selectinto-vs-ctas"></a>...를 선택 합니다. Vs와 CTAS

CTAS는 더 사용자 지정이 가능한 버전의 [SELECT ... INTO](/sql/t-sql/queries/select-into-clause-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) 문.

다음은 간단한 SELECT ... 범주로

```sql
SELECT *
INTO    [dbo].[FactInternetSales_new]
FROM    [dbo].[FactInternetSales]
```

...를 선택 합니다. 을 (를) 사용 하면 작업의 일부로 배포 메서드나 인덱스 유형을 변경할 수 없습니다. `[dbo].[FactInternetSales_new]`ROUND_ROBIN 기본 배포 유형 및 클러스터형 COLUMNSTORE 인덱스의 기본 테이블 구조를 사용 하 여 만듭니다.

반면 CTAS를 사용 하면 테이블 데이터의 배포와 테이블 구조 유형을 모두 지정할 수 있습니다. 이전 예제를 CTAS로 변환 하려면 다음을 수행 합니다.

```sql
CREATE TABLE [dbo].[FactInternetSales_new]
WITH
(
 DISTRIBUTION = ROUND_ROBIN
 ,CLUSTERED COLUMNSTORE INDEX
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales];
```

> [!NOTE]
> CTAS 작업에서 인덱스를 변경 하려는 경우에만 원본 테이블이 hash distributed 인 경우 동일한 배포 열과 데이터 형식을 유지 합니다. 이렇게 하면 작업 중에 교차 분산 데이터 이동이 방지 되므로 효율성이 높아집니다.

## <a name="use-ctas-to-copy-a-table"></a>CTAS를 사용하여 테이블 복사

CTAS의 가장 일반적인 사용 중 하나는 DDL을 변경 하기 위해 테이블의 복사본을 만드는 것입니다. 원래 테이블을로 만들었으며 `ROUND_ROBIN` 이제 열에 배포 된 테이블로 변경 하 려 한다고 가정해 보겠습니다. CTAS는 배포 열을 변경 하는 방법입니다. CTAS를 사용 하 여 분할, 인덱싱 또는 열 유형을 변경할 수도 있습니다.

의 `ROUND_ROBIN` 배포 열을 지정 하지 않고의 기본 배포 유형을 사용 하 여이 테이블을 만든 경우를 가정해 보겠습니다 `CREATE TABLE` .

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25));
```

이제를 사용 하 여이 테이블의 새 복사본을 만들어 `Clustered Columnstore Index` 클러스터형 Columnstore 테이블의 성능을 활용할 수 있습니다. 이 `ProductKey` 열에 대 한 조인을 예측 하 고 조인 하는 동안 데이터 이동을 방지 하려는 경우에도이 테이블을에 배포 하려고 합니다 `ProductKey` . 마지막으로, `OrderDateKey` 이전 파티션을 삭제 하 여 오래 된 데이터를 신속 하 게 삭제할 수 있도록 분할을 추가 하려고 합니다. 다음은 이전 테이블을 새 테이블에 복사 하는 CTAS 문입니다.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

마지막으로, 테이블 이름을 바꾸고 새 테이블에서 바꾼 후 이전 테이블을 삭제할 수 있습니다.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

## <a name="use-ctas-to-work-around-unsupported-features"></a>CTAS를 사용 하 여 지원 되지 않는 기능 해결

CTAS를 사용 하 여 아래 나열 된 지원 되지 않는 여러 기능을 해결할 수도 있습니다. 이 메서드는 코드가 규격을 준수할 뿐만 아니라 Synapse SQL에서 더 빠르게 실행 되는 경우가 많기 때문에 종종 유용할 수 있습니다. 이 성능은 완전히 병렬화 된 디자인의 결과입니다. 시나리오는 다음과 같습니다.

* UPDATE에 대한 ANSI JOINS
* DELETE에 대한 ANSI JOIN
* MERGE 문

> [!TIP]
> "CTAS를 먼저" 생각해 보세요. 결과적으로 더 많은 데이터를 작성 하는 경우에도 CTAS를 사용 하 여 문제를 해결 하는 것은 일반적으로 좋은 방법입니다.

## <a name="ansi-join-replacement-for-update-statements"></a>update 문에 대한 ANSI 조인 대체

복잡 한 업데이트가 있는 것을 확인할 수 있습니다. 업데이트는 ANSI 조인 구문을 사용 하 여 업데이트 또는 삭제를 수행 하는 두 개 이상의 테이블을 결합 합니다.

이 테이블을 업데이트해야 했다고 가정해 보십시오.

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
( [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
, [CalendarYear]                    SMALLINT        NOT NULL
, [TotalSalesAmount]                MONEY            NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
);
```

원본 쿼리는 다음 예제와 같이 보일 수 있습니다.

```sql
UPDATE    acs
SET        [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT [EnglishProductCategoryName]
        , [CalendarYear]
        , SUM([SalesAmount])                AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]        AS s
        JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
        JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
        WHERE     [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,        [CalendarYear]
        ) AS fis
ON    [acs].[EnglishProductCategoryName]    = [fis].[EnglishProductCategoryName]
AND    [acs].[CalendarYear]                = [fis].[CalendarYear];
```

Synapse SQL은 문의 절에서 ANSI 조인을 지원 하지 `FROM` `UPDATE` 않으므로 앞의 예제를 수정 하지 않고 사용할 수 없습니다.

CTAS와 암시적 조인의 조합을 사용 하 여 이전 예제를 바꿀 수 있습니다.

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT    ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0) AS [EnglishProductCategoryName]
, ISNULL(CAST([CalendarYear] AS SMALLINT),0)  AS [CalendarYear]
, ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)  AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]        AS s
JOIN    [dbo].[DimDate]                    AS d    ON s.[OrderDateKey]                = d.[DateKey]
JOIN    [dbo].[DimProduct]                AS p    ON s.[ProductKey]                = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]    AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]        AS c    ON u.[ProductCategoryKey]        = c.[ProductCategoryKey]
WHERE     [CalendarYear] = 2004
GROUP BY [EnglishProductCategoryName]
, [CalendarYear];

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]  = AnnualCategorySales.[CalendarYear] ;

--Drop the interim table
DROP TABLE CTAS_acs;
```

## <a name="ansi-join-replacement-for-merge"></a>병합을 위한 ANSI 조인 대체 

Azure Synapse Analytics에서 대상으로 일치 하지 않는 [병합](/sql/t-sql/statements/merge-transact-sql?view=sql-server-ver15) (미리 보기)을 사용 하려면 대상이 해시 분산 테이블 이어야 합니다.  사용자는 [UPDATE](/sql/t-sql/queries/update-transact-sql?view=sql-server-ver15) 또는 [DELETE](/sql/t-sql/statements/delete-transact-sql?view=sql-server-ver15) 와 함께 ANSI JOIN을 사용 하 여 다른 테이블과의 조인 결과에 따라 대상 테이블 데이터를 수정할 수 있습니다.  다음은 예제입니다.

```sql
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;

```

## <a name="explicitly-state-data-type-and-nullability-of-output"></a>데이터 형식 및 출력의 null 허용 여부를 명시적으로 지정

코드를 마이그레이션할 때 이러한 유형의 코딩 패턴을 통해 실행 되는 것을 확인할 수 있습니다.

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f;
```

이 코드를 CTAS로 마이그레이션해야 하는 것으로 생각할 수 있습니다. 그러나 여기에는 숨겨진 문제가 있습니다.

다음 코드는 동일한 결과를 생성 하지 않습니다.

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result;
```

참고로 "결과" 열은 식의 데이터 형식 및 NULL 허용 여부 값을 이월합니다. 데이터 형식을 앞으로 옮기면 주의 하지 않으면 값의 미묘한 차이가 발생할 수 있습니다.

다음 예제를 사용해 보세요.

```sql
SELECT result,result*@d
from result;

SELECT result,result*@d
from ctas_r;
```

결과에 저장된 값이 다릅니다. 결과 열에서 지속형 값이 다른 식에 사용 되 면 오류가 훨씬 더 중요 하 게 됩니다.

![CTAS 결과의 스크린샷](./media/sql-data-warehouse-develop-ctas/ctas-results.png)

이는 데이터 마이그레이션에 중요 합니다. 두 번째 쿼리는 매우 정확 하지만 문제가 있습니다. 데이터는 원본 시스템과 비교 하 여 서로 다르며 마이그레이션의 무결성에 대 한 질문이 발생 합니다. 이는 "오답"이 실제로는 정답인, 드문 경우 중 하나입니다!

두 결과 사이에 차이가가 표시 되는 이유는 암시적 형식 캐스팅 때문입니다. 첫 번째 예제에서 테이블은 열 정의를 정의 합니다. 행이 삽입 되 면 암시적 형식 변환이 발생 합니다. 두 번째 예에서는 식에서 열의 데이터 형식을 정의 하므로 암시적 형식 변환이 없습니다.

또한 두 번째 예제의 열은 Null을 허용 하는 열로 정의 된 반면 첫 번째 예에서는 그렇지 않습니다. 첫 번째 예제에서 테이블을 만든 경우 열 null 허용 여부가 명시적으로 정의 되었습니다. 두 번째 예제에서는 식이 식으로 남겨진 데 기본적으로 NULL 정의가 생성 됩니다.

이러한 문제를 해결 하려면 CTAS 문의 SELECT 부분에서 형식 변환 및 null 허용 여부를 명시적으로 설정 해야 합니다. ' CREATE TABLE '에서는 이러한 속성을 설정할 수 없습니다.
다음 예제에서는 코드를 수정 하는 방법을 보여 줍니다.

```sql
DECLARE @d decimal(7,2) = 85.455
, @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

다음 사항에 유의하세요.

* CAST 또는 CONVERT를 사용할 수 있습니다.
* Null 허용 여부를 적용 하려면 병합이 아닌 ISNULL을 사용 합니다. 다음 참고를 참조 하세요.
* ISNULL은 가장 바깥쪽 함수입니다.
* ISNULL의 두 번째 부분은 상수, 0입니다.

> [!NOTE]
> Null 허용 여부를 올바르게 설정 하려면 ISNULL을 사용 하 고 병합 하지 않는 것이 중요 합니다. 병합은 결정적 함수가 아니므로 식의 결과는 항상 Null을 허용 합니다. ISNULL은 다릅니다. 결정적입니다. 따라서 ISNULL 함수의 두 번째 부분이 상수 또는 리터럴인 경우 결과 값은 NULL이 아닙니다.

테이블 파티션 전환에도 계산의 무결성을 유지 하는 것이 중요 합니다. 이 테이블이 팩트 테이블로 정의 되어 있다고 가정 합니다.

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
, [product]   INT     NOT NULL
, [store]     INT     NOT NULL
, [quantity]  INT     NOT NULL
, [price]     MONEY   NOT NULL
, [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
);
```

그러나 amount 필드는 계산 된 식입니다. 원본 데이터의 일부가 아닙니다.

분할 된 데이터 집합을 만들려면 다음 코드를 사용 하는 것이 좋습니다.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]
,   [product]
,   [store]
,   [quantity]
,   [price]
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

쿼리가 완벽 하 게 실행 됩니다. 이 문제는 파티션 전환을 수행 하려고 할 때 발생 합니다. 테이블 정의가 일치 하지 않습니다. 테이블 정의를 일치 시키려면 `ISNULL` 열 null 허용 여부 특성을 유지 하는 함수를 추가 하도록 CTAS를 수정 합니다.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH
( DISTRIBUTION = HASH([product])
, PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
  [date]
, [product]
, [store]
, [quantity]
, [price]
, ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

CTAS에서 형식 일관성 및 null 허용 여부 속성을 유지 하는 것이 엔지니어링 모범 사례 임을 확인할 수 있습니다. 계산의 무결성을 유지 하는 데 도움이 되며 파티션 전환이 가능 합니다.

CTAS는 Synapse SQL에서 가장 중요 한 문 중 하나입니다. 완전하게 이해해야 합니다. [Ctas 설명서](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

더 많은 개발 팁은 [개발 개요](sql-data-warehouse-overview-develop.md)를 참조하세요.