---
title: 서버를 사용 하지 않는 SQL 풀을 사용 하 여 CSV 파일 쿼리
description: 이 문서에서는 서버를 사용 하지 않는 SQL 풀을 사용 하 여 다른 파일 형식으로 단일 CSV 파일을 쿼리 하는 방법에 대해 알아봅니다.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: f2f0cdf307e91fb40c55d4a98139bad1a5eca886
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96462597"
---
# <a name="query-csv-files"></a>CSV 파일 쿼리

이 문서에서는 Azure Synapse Analytics에서 서버를 사용 하지 않는 SQL 풀을 사용 하 여 단일 CSV 파일을 쿼리 하는 방법에 대해 알아봅니다. CSV 파일의 형식은 서로 다를 수 있습니다. 

- 헤더 행 포함 또는 제외
- 쉼표 및 탭으로 구분된 값
- Windows 및 Unix 스타일 줄 끝
- 따옴표가 붙지 않은 값 및 따옴표가 붙은 값과 문자 이스케이프

위의 모든 변형은 아래에서 다룹니다.

## <a name="quickstart-example"></a>빠른 시작 예제

`OPENROWSET` 함수를 사용 하면 파일에 대 한 URL을 제공 하 여 CSV 파일의 내용을 읽을 수 있습니다.

### <a name="read-a-csv-file"></a>Csv 파일 읽기

파일의 콘텐츠를 확인 하는 가장 쉬운 방법은 `CSV` 함수에 파일 URL을 제공 하 고 `OPENROWSET` csv 및 2.0을 지정 하는 것입니다 `FORMAT` `PARSER_VERSION` . 파일이 공개적으로 사용 가능한 경우 또는 Azure AD id가이 파일에 액세스할 수 있는 경우 다음 예제와 같이 쿼리를 사용 하 여 파일의 내용을 볼 수 있어야 합니다.

```sql
select top 10 *
from openrowset(
    bulk 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.csv',
    format = 'csv',
    parser_version = '2.0',
    firstrow = 2 ) as rows
```

옵션 `firstrow` 은이 경우 헤더를 나타내는 CSV 파일의 첫 번째 행을 건너뛰는 데 사용 됩니다. 이 파일에 액세스할 수 있는지 확인 합니다. 파일이 SAS 키 또는 사용자 지정 id를 사용 하 여 보호 되는 경우 [sql 로그인에 대 한 서버 수준 자격 증명](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#server-scoped-credential)을 설정 해야 합니다.

> [!IMPORTANT]
> CSV 파일에 UTF-8 문자가 포함 된 경우 UTF-8 데이터베이스 데이터 정렬을 사용 하 고 있는지 확인 합니다 (예: `Latin1_General_100_CI_AS_SC_UTF8` ).
> 파일의 텍스트 인코딩과 데이터 정렬이 일치 하지 않으면 예기치 않은 변환 오류가 발생할 수 있습니다.
> 다음 T-sql 문을 사용 하 여 현재 데이터베이스의 기본 데이터 정렬을 쉽게 변경할 수 있습니다. `alter database current collate Latin1_General_100_CI_AI_SC_UTF8`

### <a name="data-source-usage"></a>데이터 원본 사용

이전 예에서는 파일에 대 한 전체 경로를 사용 합니다. 대신 저장소의 루트 폴더를 가리키는 위치를 사용 하 여 외부 데이터 원본을 만들 수 있습니다.

```sql
create external data source covid
with ( location = 'https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases' );
```

데이터 원본을 만든 후에는 해당 데이터 원본 및 함수에 있는 파일에 대 한 상대 경로를 사용할 수 있습니다 `OPENROWSET` .

```sql
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.csv',
        data_source = 'covid',
        format = 'csv',
        parser_version ='2.0',
        firstrow = 2
    ) as rows
```

데이터 원본이 SAS 키 또는 사용자 지정 id를 사용 하 여 보호 되는 경우 [데이터베이스 범위 자격 증명을 사용 하 여 데이터 원본을](develop-storage-files-storage-access-control.md?tabs=shared-access-signature#database-scoped-credential)구성할 수 있습니다.

### <a name="explicitly-specify-schema"></a>명시적으로 스키마 지정

`OPENROWSET` 에서는 절을 사용 하 여 파일을 읽을 열을 명시적으로 지정할 수 있습니다 `WITH` .

```sql
select top 10 *
from openrowset(
        bulk 'latest/ecdc_cases.csv',
        data_source = 'covid',
        format = 'csv',
        parser_version ='2.0',
        firstrow = 2
    ) with (
        date_rep date 1,
        cases int 5,
        geo_id varchar(6) 8
    ) as rows
```

절에서 데이터 형식 뒤의 숫자는 `WITH` CSV 파일의 열 인덱스를 나타냅니다.

> [!IMPORTANT]
> CSV 파일에 UTF-8 문자가 포함 된 경우 절에 있는 모든 열에 대 한 UTF-8 데이터 정렬 (예:)을 지정 `Latin1_General_100_CI_AS_SC_UTF8` `WITH` 하거나 데이터베이스 수준에서 utf-8 데이터 정렬을 설정 하는 것이 explicilty 확인 합니다.
> 파일 및 데이터 정렬에서 텍스트 인코딩이 일치 하지 않으면 예기치 않은 변환 오류가 발생할 수 있습니다.
> 다음 T-sql 문을 사용 하 여 현재 데이터베이스의 기본 데이터 정렬을 쉽게 변경할 수 있습니다. `alter database current collate Latin1_General_100_CI_AI_SC_UTF8`
> 다음 정의를 사용 하 여 열 형식에 대 한 데이터 정렬을 쉽게 설정할 수 있습니다. `geo_id varchar(6) collate Latin1_General_100_CI_AI_SC_UTF8 8`

다음 섹션에서는 다양 한 유형의 CSV 파일을 쿼리 하는 방법을 볼 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

첫 번째 단계는 테이블을 만들 **데이터베이스를 만드는 것** 입니다. 그런 다음 해당 데이터베이스에서 [설치 스크립트](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql)를 실행하여 개체를 초기화합니다. 이 설치 스크립트는 이러한 예에서 사용되는 데이터 원본, 데이터베이스 범위의 자격 증명 및 외부 파일 형식을 만듭니다.

## <a name="windows-style-new-line"></a>Windows 스타일 줄 바꿈

다음 쿼리는 Windows 스타일 새 행과 쉼표로 구분된 열을 사용하여 헤더 행 없는 CSV 파일을 읽는 방법을 보여 줍니다.

파일 미리 보기:

![헤더가 없는 CSV 파일의 처음 10개 행, Windows 스타일 새 줄.](./media/query-single-csv-file/population.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="unix-style-new-line"></a>Unix 스타일 줄 바꿈

다음 쿼리에서는 헤더 행이 없이 Unix 스타일 줄 바꿈 및 쉼표로 구분된 열로 구성된 파일을 읽는 방법을 보여 줍니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![헤더 행이 없이 Unix 스타일 줄 바꿈이 있는 CSV 파일의 처음 10개 행입니다.](./media/query-single-csv-file/population-unix.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="header-row"></a>헤더 행

다음 쿼리는 헤더 행이 있고 Unix 스타일 줄 바꿈 및 쉼표로 구분된 열로 구성된 파일을 읽는 방법을 보여 줍니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![헤더 행과 Unix 스타일 줄 바꿈이 있는 CSV 파일의 처음 10개 행입니다.](./media/query-single-csv-file/population-unix-hdr.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

## <a name="custom-quote-character"></a>사용자 지정 따옴표

다음 쿼리에서는 헤더 행이 있고 Unix 스타일 줄 바꿈, 쉼표로 구분된 열 및 따옴표가 붙은 값으로 구성된 파일을 읽는 방법을 보여 줍니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![헤더 행이 있고 Unix 스타일 줄 바꿈 및 따옴표가 붙은 값이 있는 CSV 파일의 처음 10개 행입니다.](./media/query-single-csv-file/population-unix-hdr-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-quoted/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        FIELDQUOTE = '"'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017;
```

> [!NOTE]
> FIELDQUOTE의 기본값은 큰따옴표이므로 FIELDQUOTE 매개 변수를 생략하면 이 쿼리는 동일한 결과를 반환합니다.

## <a name="escape-characters"></a>이스케이프 문자

다음 쿼리에서는 헤더 행이 있고 Unix 스타일 줄 바꿈, 쉼표로 구분된 열 및 값 내의 필드 구분 기호(쉼표)에 사용되는 이스케이프 문자로 구성된 파일을 읽는 방법을 보여 줍니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![헤더 행이 있고 Unix 스타일 줄 바꿈 및 필드 구분 기호에 사용되는 이스케이프 문자가 있는 CSV 파일의 처음 10개 행입니다.](./media/query-single-csv-file/population-unix-hdr-escape.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-escape/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2,
        ESCAPECHAR = '\\'
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slovenia';
```

> [!NOTE]
> "Slov,enia"의 쉼표가 국가/지역 이름의 일부가 아니라 필드 구분 기호로 처리되기 때문에 ESCAPECHAR를 지정하지 않으면 이 쿼리는 실패합니다. "Slov,enia"는 두 개의 열로 처리됩니다. 따라서 특정 행에는 다른 행보다 하나의 열이 더 있고, WITH 절에서 정의한 것보다 하나의 열이 더 있습니다.

### <a name="escape-quoting-characters"></a>이스케이프 따옴표 문자

다음 쿼리에서는 헤더 행이 있는 파일을 읽는 방법을 보여 줍니다. 여기에는 Unix 스타일의 줄 바꿈 및 쉼표로 구분 된 열과 값 내 이스케이프 된 큰따옴표 문자를 사용 합니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![다음 쿼리에서는 헤더 행이 있는 파일을 읽는 방법을 보여 줍니다. 여기에는 Unix 스타일의 줄 바꿈 및 쉼표로 구분 된 열과 값 내 이스케이프 된 큰따옴표 문자를 사용 합니다.](./media/query-single-csv-file/population-unix-hdr-escape-quoted.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-escape-quoted/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Slovenia';
```

> [!NOTE]
> 따옴표 문자는 다른 따옴표 문자로 이스케이프해야 합니다. 따옴표로 묶은 문자는 값이 따옴표 문자로 캡슐화된 경우에만 열 값 내에 나타날 수 있습니다.

## <a name="tab-delimited-files"></a>탭으로 구분된 파일

다음 쿼리는 헤더 행이 있고 Unix 스타일 줄 바꿈 및 탭으로 구분된 열로 구성된 파일을 읽는 방법을 보여 줍니다. 다른 예와 비교하면 파일의 위치가 다르다는 것을 알 수 있습니다.

파일 미리 보기:

![헤더 행이 있고 Unix 스타일 줄 바꿈 및 탭 구분 기호가 있는 CSV 파일의 처음 10개 행입니다.](./media/query-single-csv-file/population-unix-hdr-tsv.png)

```sql
SELECT *
FROM OPENROWSET(
        BULK 'csv/population-unix-hdr-tsv/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR ='\t',
        ROWTERMINATOR = '0x0a',
        FIRSTROW = 2
    )
    WITH (
        [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
        [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
        [year] smallint,
        [population] bigint
    ) AS [r]
WHERE
    country_name = 'Luxembourg'
    AND year = 2017
```

## <a name="return-a-subset-of-columns"></a>열의 하위 집합 반환

지금까지 WITH를 사용하고 모든 열을 나열하는 CSV 파일 스키마를 지정했습니다. 필요한 각 열에 서수를 사용하여 쿼리에 실제로 필요한 열만 지정할 수 있습니다. 또한 관심이 없는 열은 생략합니다.

다음 쿼리는 필요한 열만 지정하여 파일의 고유한 국가/지역 이름 수를 반환합니다.

> [!NOTE]
> 아래 쿼리의 WITH 절을 보면 *[country_name]* 열을 정의하는 행의 끝에 "2"(따옴표 제외)가 있습니다. 이는 *[country_name]* 열이 파일의 두 번째 열임을 의미합니다. 이 쿼리는 파일에서 두 번째 열을 제외한 모든 열을 무시합니다.

```sql
SELECT
    COUNT(DISTINCT country_name) AS countries
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0',
        FIELDTERMINATOR =',',
        ROWTERMINATOR = '\n'
    )
WITH (
    --[country_code] VARCHAR (5),
    [country_name] VARCHAR (100) 2
    --[year] smallint,
    --[population] bigint
) AS [r]
```

## <a name="next-steps"></a>다음 단계

다음 문서에서는 다음을 수행하는 방법을 보여 줍니다.

- [Parquet 파일 쿼리](query-parquet-files.md)
- [폴더 및 여러 파일 쿼리](query-folders-multiple-csv-files.md)
