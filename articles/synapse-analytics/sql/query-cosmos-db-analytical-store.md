---
title: Azure Synapse Link Preview에서 서버를 사용 하지 않는 SQL 풀을 사용 하 여 데이터 쿼리 Azure Cosmos DB
description: 이 문서에서는 Azure Synapse Link Preview에서 서버를 사용 하지 않는 SQL 풀을 사용 하 여 Azure Cosmos DB를 쿼리 하는 방법에 대해 알아봅니다.
services: synapse analytics
author: jovanpop-msft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 12/04/2020
ms.author: jovanpop
ms.reviewer: jrasnick
ms.openlocfilehash: 22103ad580fa474f44eaf42c696d19bbbd137c8e
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97095103"
---
# <a name="query-azure-cosmos-db-data-with-a-serverless-sql-pool-in-azure-synapse-link-preview"></a>Azure Synapse Link Preview에서 서버를 사용 하지 않는 SQL 풀을 사용 하 여 Azure Cosmos DB 데이터 쿼리

> [!IMPORTANT]
> Azure Cosmos DB에 대 한 Azure Synapse 링크에 대 한 서버 리스 SQL 풀 지원은 현재 미리 보기 상태입니다. 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 자세한 내용은 Microsoft Azure Preview에 대한 [추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.


서버 리스 SQL 풀을 사용 하면 트랜잭션 워크 로드의 성능에 영향을 주지 않고 거의 실시간으로 [Azure Synapse Link](../../cosmos-db/synapse-link.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 로 설정 된 Azure Cosmos DB 컨테이너의 데이터를 분석할 수 있습니다. [분석 저장소](../../cosmos-db/analytical-store-introduction.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) 에서 데이터를 쿼리 하는 친숙 한 T-sql 구문과 t-sql 인터페이스를 통한 다양 한 BI (비즈니스 인텔리전스) 및 임시 쿼리 도구에 대 한 통합 연결을 제공 합니다.

Azure Cosmos DB 쿼리를 위해 전체 [SELECT](/sql/t-sql/queries/select-transact-sql?view=sql-server-ver15) 노출 영역은 대부분의 [SQL 함수 및 연산자](overview-features.md)를 포함 하는 [OPENROWSET](develop-openrowset.md) 함수를 통해 지원 됩니다. [Create external table as select](develop-tables-cetas.md#cetas-in-serverless-sql-pool) (CETAS)를 사용 하 여 Azure Blob Storage 또는 Azure Data Lake Storage의 데이터와 함께 Azure Cosmos DB에서 데이터를 읽는 쿼리 결과를 저장할 수도 있습니다. 현재 CETAS를 사용 하 여 Azure Cosmos DB에 서버 리스 SQL 풀 쿼리 결과를 저장할 수 없습니다.

이 문서에서는 Azure Synapse Link를 사용 하 여 사용 하도록 설정 된 Azure Cosmos DB 컨테이너에서 데이터를 쿼리 하는 서버를 사용 하지 않는 SQL 풀로 쿼리를 작성 하는 방법을 알아봅니다. 그런 다음 Azure Cosmos DB 컨테이너를 통해 서버 리스 SQL 풀 뷰를 빌드하고 [이 자습서](./tutorial-data-analyst.md)의 Power BI 모델에 연결 하는 방법에 대해 자세히 알아볼 수 있습니다.

> [!IMPORTANT]
> 이 자습서에서는 [Azure Cosmos DB 잘 정의 된 스키마](../../cosmos-db/analytical-store-introduction.md#schema-representation)가 있는 컨테이너를 사용 합니다. 서버를 사용 하지 않는 SQL 풀에서 [Azure Cosmos DB 전체 충실도 스키마](#full-fidelity-schema) 를 제공 하는 쿼리 환경은 미리 보기 피드백에 따라 변경 되는 임시 동작입니다. `OPENROWSET` `WITH` 쿼리 환경이 잘 정의 된 스키마를 기준으로 정렬 되 고 변경 될 수 있으므로 전체 충실도 스키마를 사용 하 여 컨테이너에서 데이터를 읽는 절 없이 함수의 결과 집합 스키마를 사용 하지 마세요. [Azure Synapse Analytics 피드백 포럼](https://feedback.azure.com/forums/307516-azure-synapse-analytics)에 피드백을 게시할 수 있습니다. [Azure Synapse 링크 제품 팀](mailto:cosmosdbsynapselink@microsoft.com) 에 연락 하 여 피드백을 제공할 수도 있습니다.

## <a name="overview"></a>개요

서버 리스 SQL 풀을 사용 하면 함수를 사용 하 여 Azure Cosmos DB 분석 저장소를 쿼리할 수 있습니다 `OPENROWSET` . 
- `OPENROWSET` 인라인 키를 사용 합니다. 이 구문을 사용 하 여 자격 증명을 준비할 필요 없이 Azure Cosmos DB 컬렉션을 쿼리할 수 있습니다.
- `OPENROWSET` Cosmos DB 계정 키를 포함 하는 참조 된 자격 증명입니다. 이 구문을 사용 하 여 Azure Cosmos DB 컬렉션에 대 한 뷰를 만들 수 있습니다.

### <a name="openrowset-with-key"></a>[키가 있는 OPENROWSET](#tab/openrowset-key)

Azure Cosmos DB 분석 저장소에서 데이터 쿼리 및 분석을 지원 하기 위해 서버 리스 SQL 풀은 다음 구문을 사용 합니다 `OPENROWSET` .

```sql
OPENROWSET( 
       'CosmosDB',
       '<Azure Cosmos DB connection string>',
       <Container name>
    )  [ < with clause > ] AS alias
```

Azure Cosmos DB 연결 문자열에는 Azure Cosmos DB 계정 이름, 데이터베이스 이름, 데이터베이스 계정 마스터 키 및 함수에 대 한 선택적 지역 이름이 지정 `OPENROWSET` 됩니다.

연결 문자열의 형식은 다음과 같습니다.
```sql
'account=<database account name>;database=<database name>;region=<region name>;key=<database account master key>'
```

구문에서 따옴표 없이 Azure Cosmos DB 컨테이너 이름을 지정 합니다 `OPENROWSET` . 컨테이너 이름에 특수 문자 (예: 대시 (-))가 있는 경우이 이름은 구문에서 대괄호 () 안에 래핑됩니다 `[]` `OPENROWSET` .

### <a name="openrowset-with-credential"></a>[OPENROWSET 및 자격 증명](#tab/openrowset-credential)

`OPENROWSET`자격 증명을 참조 하는 구문을 사용할 수 있습니다.

```sql
OPENROWSET( 
       PROVIDER = 'CosmosDB',
       CONNECTION = '<Azure Cosmos DB connection string without account key>',
       OBJECT = '<Container name>',
       [ CREDENTIAL | SERVER_CREDENTIAL ] = '<credential name>'
    )  [ < with clause > ] AS alias
```

이 경우 Azure Cosmos DB 연결 문자열에는 키가 포함 되지 않습니다. 연결 문자열의 형식은 다음과 같습니다.
```sql
'account=<database account name>;database=<database name>;region=<region name>'
```

데이터베이스 계정 마스터 키가 서버 수준 자격 증명 또는 데이터베이스 범위 자격 증명에 배치 됩니다. 

---

> [!IMPORTANT]
> `Latin1_General_100_CI_AS_SC_UTF8`Azure Cosmos DB 분석 저장소의 문자열 값이 utf-8 텍스트로 인코딩 되기 때문에 일부 utf-8 데이터베이스 데이터 정렬 (예:)을 사용 하 고 있는지 확인 합니다.
> 파일 및 데이터 정렬에서 텍스트 인코딩이 일치 하지 않으면 예기치 않은 텍스트 변환 오류가 발생할 수 있습니다.
> T-sql 문을 사용 하 여 현재 데이터베이스의 기본 데이터 정렬을 쉽게 변경할 수 있습니다 `alter database current collate Latin1_General_100_CI_AI_SC_UTF8` .

> [!NOTE]
> 서버를 사용 하지 않는 SQL 풀은 Azure Cosmos DB 트랜잭션 저장소 쿼리를 지원 하지 않습니다.

## <a name="sample-dataset"></a>샘플 데이터 세트

이 문서의 예는 [에 대 한 유럽 센터에서 질병 방지 및 제어 (ECDC) COVID-19 사례](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 와 [Covid-19 Open Research 데이터 집합 (코드-19), doi: 10.5281/zenodo](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/)에 대 한 데이터를 기반으로 합니다.

이러한 페이지에서 데이터의 라이선스 및 구조를 확인할 수 있습니다. [Ecdc](https://pandemicdatalake.blob.core.windows.net/public/curated/covid-19/ecdc_cases/latest/ecdc_cases.json) 및 [코드-19](https://azureopendatastorage.blob.core.windows.net/covid19temp/comm_use_subset/pdf_json/000b7d1517ceebb34e1e3e817695b6de03e2fa78.json) 데이터 집합에 대 한 샘플 데이터를 다운로드할 수도 있습니다.

이 문서의 내용을 따르려면 서버를 사용 하지 않는 SQL 풀을 사용 하 여 Azure Cosmos DB 데이터를 쿼리 하는 방법 보여주는 다음 리소스를 만들어야 합니다.

* [Azure Synapse Link를 사용](../../cosmos-db/configure-synapse-link.md)하는 Azure Cosmos DB 데이터베이스 계정
* 이라는 Azure Cosmos DB 데이터베이스 `covid` 입니다.
* `Ecdc` `Cord19` 앞의 샘플 데이터 집합으로 명명 되 고 로드 된 두 개의 Azure Cosmos DB 컨테이너

테스트 목적으로 다음 연결 문자열을 사용할 수 있습니다 `Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==` . 이 계정은 Synapse SQL 끝점과 비교 하 여 원격 지역에 있을 수 있으므로이 연결은 성능을 보장 하지 않습니다.

## <a name="explore-azure-cosmos-db-data-with-automatic-schema-inference"></a>자동 스키마 유추를 사용 하 여 Azure Cosmos DB 데이터 탐색

Azure Cosmos DB에서 데이터를 탐색 하는 가장 쉬운 방법은 자동 스키마 유추 기능을 사용 하는 것입니다. 문에서 절을 생략 하면 서버를 사용 `WITH` `OPENROWSET` 하지 않는 SQL 풀에서 Azure Cosmos DB 컨테이너의 분석 저장소 스키마를 자동으로 검색 (유추) 하도록 지시할 수 있습니다.

### <a name="openrowset-with-key"></a>[키가 있는 OPENROWSET](#tab/openrowset-key)

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==',
       Ecdc) as documents
```

### <a name="openrowset-with-credential"></a>[OPENROWSET 및 자격 증명](#tab/openrowset-credential)

```sql
/*  Setup - create server-level or database scoped credential with Azure Cosmos DB account key:
    CREATE CREDENTIAL MyCosmosDbAccountCredential
    WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 's5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==';
*/
SELECT TOP 10 *
FROM OPENROWSET(
      PROVIDER = 'CosmosDB',
      CONNECTION = 'Account=synapselink-cosmosdb-sqlsample;Database=covid',
      OBJECT = 'Ecdc',
      SERVER_CREDENTIAL = 'MyCosmosDbAccountCredential'
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

---

앞의 예제에서는 서버를 사용 하지 않는 SQL 풀에서 `covid` `MyCosmosDbAccount` Azure Cosmos DB 키 (위 예제의 더미)를 사용 하 여 인증 된 Azure Cosmos DB 계정에서 데이터베이스에 연결 하도록 지시 했습니다. 그런 다음 해당 `Ecdc` 지역의 컨테이너 분석 저장소에 액세스 합니다 `West US 2` . 특정 속성의 프로젝션이 없으므로 `OPENROWSET` 함수는 Azure Cosmos DB 항목의 모든 속성을 반환 합니다.

Azure Cosmos DB 컨테이너의 항목에 `date_rep` , 및 속성이 있다고 가정 하면 `cases` `geo_id` 이 쿼리의 결과는 다음 표에 나와 있습니다.

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

동일한 Azure Cosmos DB 데이터베이스의 다른 컨테이너에서 데이터를 탐색 해야 하는 경우 동일한 연결 문자열을 사용 하 고 세 번째 매개 변수로 필요한 컨테이너를 참조할 수 있습니다.

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==',
       Cord19) as cord19
```

## <a name="explicitly-specify-schema"></a>명시적으로 스키마 지정

의 자동 스키마 유추 기능은 `OPENROWSET` 간단 하 고 사용 하기 쉬운 querience을 제공 하지만, 비즈니스 시나리오에서는 Azure Cosmos DB 데이터의 관련 속성을 읽기 전용으로 지정 하도록 스키마를 명시적으로 지정 해야 할 수 있습니다.

`OPENROWSET`함수를 사용 하면 컨테이너의 데이터에서 읽을 속성을 명시적으로 지정 하 고 해당 데이터 형식을 지정할 수 있습니다.

다음 구조를 포함 하는 [Ecdc COVID 데이터 집합](https://azure.microsoft.com/services/open-datasets/catalog/ecdc-covid-19-cases/) 의 일부 데이터를 Azure Cosmos DB으로 가져왔습니다.

```json
{"date_rep":"2020-08-13","cases":254,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-12","cases":235,"countries_and_territories":"Serbia","geo_id":"RS"}
{"date_rep":"2020-08-11","cases":163,"countries_and_territories":"Serbia","geo_id":"RS"}
```

Azure Cosmos DB의 이러한 플랫 JSON 문서를 Synapse SQL의 행 및 열 집합으로 표시할 수 있습니다. `OPENROWSET`함수를 사용 하면 절에서 읽을 속성의 하위 집합 및 정확한 열 형식을 지정할 수 있습니다 `WITH` .

### <a name="openrowset-with-key"></a>[키가 있는 OPENROWSET](#tab/openrowset-key)
```sql
SELECT TOP 10 *
FROM OPENROWSET(
      'CosmosDB',
      'Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==',
       Ecdc
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```
### <a name="openrowset-with-credential"></a>[OPENROWSET 및 자격 증명](#tab/openrowset-credential)
```sql
/*  Setup - create server-level or database scoped credential with Azure Cosmos DB account key:
    CREATE CREDENTIAL MyCosmosDbAccountCredential
    WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 's5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==';
*/
SELECT TOP 10 *
FROM OPENROWSET(
      PROVIDER = 'CosmosDB',
      CONNECTION = 'Account=synapselink-cosmosdb-sqlsample;Database=covid',
      OBJECT = 'Ecdc',
      SERVER_CREDENTIAL = 'MyCosmosDbAccountCredential'
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```
---
이 쿼리의 결과는 다음 표와 같습니다.

| date_rep | cases | geo_id |
| --- | --- | --- |
| 2020-08-13 | 254 | RS |
| 2020-08-12 | 235 | RS |
| 2020-08-11 | 163 | RS |

Azure Cosmos DB 값에 사용 해야 하는 SQL 형식에 대 한 자세한 내용은 문서의 끝에 있는 [sql 형식 매핑 규칙](#azure-cosmos-db-to-sql-type-mappings) 을 참조 하세요.

## <a name="create-view"></a>뷰 만들기

스키마를 확인 한 후 Azure Cosmos DB 데이터를 기반으로 보기를 준비할 수 있습니다. Azure Cosmos DB 계정 키를 별도의 자격 증명에 넣고 함수에서이 자격 증명을 참조 해야 합니다 `OPENROWSET` . 계정 키를 보기 정의에 보관 하지 마세요.

```sql
CREATE CREDENTIAL MyCosmosDbAccountCredential
WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 's5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==';
GO
CREATE OR ALTER VIEW Ecdc
AS SELECT *
FROM OPENROWSET(
      PROVIDER = 'CosmosDB',
      CONNECTION = 'Account=synapselink-cosmosdb-sqlsample;Database=covid',
      OBJECT = 'Ecdc',
      SERVER_CREDENTIAL = 'MyCosmosDbAccountCredential'
    ) with ( date_rep varchar(20), cases bigint, geo_id varchar(6) ) as rows
```

`OPENROWSET`성능에 영향을 줄 수 있으므로 명시적으로 정의 하지 않은 스키마를 사용 하지 마세요. 열에 사용할 수 있는 최소 크기를 사용 해야 합니다 (예: 기본 VARCHAR (8000) 대신 VARCHAR (100)). Utf-8 [변환 문제](/azure/synapse-analytics/troubleshoot/reading-utf8-text)를 방지 하려면 utf-8 데이터 정렬을 기본 데이터베이스 데이터 정렬로 사용 하거나 명시적 열 데이터 정렬로 설정 해야 합니다. `Latin1_General_100_BIN2_UTF8`데이터 정렬은 yu에서 일부 문자열 열을 사용 하 여 데이터를 필터링 할 때 최상의 성능을 제공 합니다.

## <a name="query-nested-objects-and-arrays"></a>중첩 된 개체 및 배열 쿼리

Azure Cosmos DB를 사용 하면 중첩 된 개체 또는 배열로 작성 하 여 더 복잡 한 데이터 모델을 나타낼 수 있습니다. Azure Cosmos DB에 대 한 Azure Synapse 링크의 autosync 기능은 서버 리스 SQL 풀에서 다양 한 쿼리를 허용 하는 중첩 된 데이터 형식 처리를 포함 하 여 분석 저장소에서 스키마 표현을 관리 합니다.

예를 들어, [코드-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) 데이터 집합은이 구조를 따르는 JSON 문서를 포함 합니다.

```json
{
    "paper_id": <str>,                   # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": <array of objects>    # list of author dicts, in order
        ...
     }
     ...
}
```

Azure Cosmos DB의 중첩 된 개체와 배열은 함수에서이를 읽을 때 쿼리 결과에 JSON 문자열로 표시 됩니다 `OPENROWSET` . 절을 사용 하는 경우 개체에서 중첩 된 값의 경로를 지정할 수 있습니다 `WITH` .

```sql
SELECT TOP 10 *
FROM OPENROWSET( 
       'CosmosDB',
       'Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==',
       Cord19)
WITH (  paper_id    varchar(8000),
        title        varchar(1000) '$.metadata.title',
        metadata     varchar(max),
        authors      varchar(max) '$.metadata.authors'
) AS docs;
```

이 쿼리의 결과는 다음 표와 같습니다.

| paper_id | title | metadata | authors |
| --- | --- | --- |
| bb11206963e831f... | Epidemi에 대 한 보충 정보 ... | `{"title":"Supplementary Informati…` | `[{"first":"Julien","last":"Mélade","suffix":"","af…`| 
| bb1206963e831f1... | 면역에 Convalescent 세라를 사용 합니다. | `{"title":"The Use of Convalescent…` | `[{"first":"Antonio","last":"Lavazza","suffix":"", …` |
| bb378eca9aac649... | Marama (Tylosema esculentum) | `{"title":"Tylosema esculentum (Ma…` | `[{"first":"Walter","last":"Chingwaru","suffix":"",…` | 

Azure Synapse 링크 및 서버를 사용 하지 않는 [SQL 풀의 중첩 된 구조](query-parquet-nested-types.md)에서 [복합 데이터 형식을](../how-to-analyze-complex-schema.md) 분석 하는 방법에 대해 자세히 알아보세요.

> [!IMPORTANT]
> 대신 텍스트에서 예기치 않은 문자가 표시 되는 경우 `MÃƒÂ©lade` `Mélade` 데이터베이스 데이터 정렬은 [utf-8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 데이터 정렬로 설정 되지 않습니다.
> 과 같은 SQL 문을 사용 하 여 [데이터베이스의 데이터 정렬을](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) utf-8 데이터 정렬로 변경 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 합니다.

## <a name="flatten-nested-arrays"></a>중첩 된 배열 평면화

Azure Cosmos DB 데이터에는 [코드-19](https://azure.microsoft.com/services/open-datasets/catalog/covid-19-open-research/) 데이터 집합에서 저자의 배열과 같이 중첩 된 subarrays 있을 수 있습니다.

```json
{
    "paper_id": <str>,                      # 40-character sha1 of the PDF
    "metadata": {
        "title": <str>,
        "authors": [                        # list of author dicts, in order
            {
                "first": <str>,
                "middle": <list of str>,
                "last": <str>,
                "suffix": <str>,
                "affiliation": <dict>,
                "email": <str>
            },
            ...
        ],
        ...
}
```

경우에 따라 최상위 항목 (메타 데이터)의 속성을 배열 (작성자)의 모든 요소와 "조인" 해야 할 수도 있습니다. 서버를 사용 하지 않는 SQL 풀을 사용 하면 `OPENJSON` 중첩 된 배열에 함수를 적용 하 여 중첩 된 구조를 평면화 할 수 있습니다.

```sql
SELECT
    *
FROM
    OPENROWSET(
      'CosmosDB',
      'Account=synapselink-cosmosdb-sqlsample;Database=covid;Key=s5zarR2pT0JWH9k8roipnWxUYBegOuFGjJpSjGlR36y86cW0GQ6RaaG8kGjsRAQoWMw1QKTkkX8HQtFpJjC8Hg==',
       Cord19
    ) WITH ( title varchar(1000) '$.metadata.title',
             authors varchar(max) '$.metadata.authors' ) AS docs
      CROSS APPLY OPENJSON ( authors )
                  WITH (
                       first varchar(50),
                       last varchar(50),
                       affiliation nvarchar(max) as json
                  ) AS a
```

이 쿼리의 결과는 다음 표와 같습니다.

| title | authors | first | last | 정보 |
| --- | --- | --- | --- | --- |
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Julien","last":"Mélade","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Julien | Mélade | `   {"laboratory":"Centre de Recher…` |
Epidemi에 대 한 보충 정보 ... | `[{"first":"Nicolas","last":"4#","suffix":"","affiliation":{"laboratory":"","institution":"U…` | Nicolas | 3-4 # |`{"laboratory":"","institution":"U…` | 
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Beza","last":"Ramazindrazana","suffix":"","affiliation":{"laboratory":"Centre de Recher…` | Beza | Ramazindrazana | `{"laboratory":"Centre de Recher…` |
| Epidemi에 대 한 보충 정보 ... |   `[{"first":"Olivier","last":"Flores","suffix":"","affiliation":{"laboratory":"UMR C53 CIRAD, …` | Olivier | Flores |`{"laboratory":"UMR C53 CIRAD, …` |     

> [!IMPORTANT]
> 대신 텍스트에서 예기치 않은 문자가 표시 되는 경우 `MÃƒÂ©lade` `Mélade` 데이터베이스 데이터 정렬은 [utf-8](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support#utf8) 데이터 정렬로 설정 되지 않습니다. 과 같은 SQL 문을 사용 하 여 [데이터베이스의 데이터 정렬을](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-database-collation#to-change-the-database-collation) utf-8 데이터 정렬로 변경 `ALTER DATABASE MyLdw COLLATE LATIN1_GENERAL_100_CI_AS_SC_UTF8` 합니다.

## <a name="azure-cosmos-db-to-sql-type-mappings"></a>SQL 형식 매핑 Azure Cosmos DB

Azure Cosmos DB 트랜잭션 저장소는 스키마에 구애 받지 않지만 분석 저장소는 분석 쿼리 성능을 최적화 하기 위해 스키마 화 된 됩니다. Azure Synapse 링크의 autosync 기능을 사용 하 여 Azure Cosmos DB는 분석 저장소에서 중첩 된 데이터 형식 처리를 포함 하는 스키마 표현을 관리 합니다. 서버를 사용 하지 않는 SQL 풀은 분석 저장소를 쿼리 하므로 Azure Cosmos DB 입력 데이터 형식을 SQL 데이터 형식에 매핑하는 방법을 이해 하는 것이 중요 합니다.

SQL (Core) API의 Azure Cosmos DB 계정은 숫자, 문자열, 부울, null, 중첩 된 개체 또는 배열의 JSON 속성 유형을 지원 합니다. 에서 절을 사용 하는 경우 이러한 JSON 유형과 일치 하는 SQL 유형을 선택 해야 `WITH` `OPENROWSET` 합니다. 다음 표에서는 Azure Cosmos DB의 다양 한 속성 유형에 사용 해야 하는 SQL 열 유형을 보여 줍니다.

| Azure Cosmos DB 속성 유형 | SQL 열 형식 |
| --- | --- |
| 부울 | bit |
| 정수 | bigint |
| Decimal | float |
| String | varchar (UTF-8 데이터베이스 데이터 정렬) |
| 날짜/시간 (ISO 형식 문자열) | varchar (30) |
| 날짜 시간 (UNIX 타임 스탬프) | bigint |
| Null | `any SQL type` 
| 중첩 된 개체 또는 배열 | varchar (max) (UTF-8 데이터베이스 데이터 정렬) (JSON 텍스트로 직렬화 됨) |

## <a name="full-fidelity-schema"></a>전체 충실도 스키마

전체 충실도 스키마 Azure Cosmos DB 컨테이너의 모든 속성에 대해 값과 가장 일치 하는 항목을 모두 기록 합니다. `OPENROWSET`전체 충실도 스키마를 사용 하는 컨테이너의 함수는 각 셀의 형식과 실제 값을 모두 제공 합니다. 다음 쿼리는 전체 충실도 스키마를 사용 하 여 컨테이너에서 항목을 읽는 것으로 가정 하겠습니다.

```sql
SELECT *
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Ecdc
    ) as rows
```

이 쿼리의 결과는 JSON 텍스트로 형식이 지정 된 형식 및 값을 반환 합니다.

| date_rep | cases | geo_id |
| --- | --- | --- |
| {"date": "2020-08-13"} | {"int32": "254"} | {"string": "RS"} |
| {"date": "2020-08-12"} | {"int32": "235"}| {"string": "RS"} |
| {"date": "2020-08-11"} | {"int32": "316"} | {"string": "RS"} |
| {"date": "2020-08-10"} | {"int32": "281"} | {"string": "RS"} |
| {"date": "2020-08-09"} | {"int32": "295"} | {"string": "RS"} |
| {"string": "2020/08/08"} | {"int32": "312"} | {"string": "RS"} |
| {"date": "2020-08-07"} | {"float64": "339.0"} | {"string": "RS"} |

모든 값에 대해 Azure Cosmos DB 컨테이너 항목에서 식별 된 형식을 확인할 수 있습니다. 속성 값의 대부분은 `date_rep` `date` 값을 포함 하지만, 이러한 값 중 일부는 Azure Cosmos DB의 문자열로 잘못 저장 됩니다. Full 충실도 스키마는 올바르게 형식화 된 `date` 값과 잘못 된 형식의 값을 모두 반환 `string` 합니다.
사례 수는 값으로 저장 된 정보 `int32` 이지만 10 진수로 입력 된 값이 하나 있습니다. 이 값의 `float64` 형식은입니다. 가장 큰 값을 초과 하는 값이 있는 경우 해당 값은 `int32` 형식으로 저장 됩니다 `int64` . `geo_id`이 예제의 모든 값은 형식으로 저장 됩니다 `string` .

> [!IMPORTANT]
> `OPENROWSET`절이 없는 함수는 `WITH` 예상 형식이 있는 값과 형식이 잘못 입력 된 값을 모두 노출 합니다. 이 함수는 보고에 대 한 데이터 탐색이 아닌 데이터 탐색을 위해 설계 되었습니다. 보고서를 빌드하기 위해이 함수에서 반환 된 JSON 값을 구문 분석 하지 않습니다. 명시적 [WITH 절](#query-items-with-full-fidelity-schema) 을 사용 하 여 보고서를 만듭니다. 전체 품질 분석 저장소에서 수정 내용을 적용 하려면 Azure Cosmos DB 컨테이너에 잘못 된 형식이 있는 값을 정리 해야 합니다.

Mongo DB API 종류의 Azure Cosmos DB 계정을 쿼리해야 하는 경우 분석 저장소에서 전체 충실도 스키마 표현과 [분석 저장소 (미리 보기) Azure Cosmos DB](../../cosmos-db/analytical-store-introduction.md#analytical-schema)에 사용할 확장 속성 이름에 대해 자세히 알아볼 수 있습니다.

### <a name="query-items-with-full-fidelity-schema"></a>전체 충실도 스키마를 사용 하는 쿼리 항목

전체 충실도 스키마를 쿼리 하는 동안 절에서 SQL 유형 및 필요한 Azure Cosmos DB 속성 유형을 명시적으로 지정 해야 합니다 `WITH` . `OPENROWSET` `WITH` 피드백에 따라 미리 보기에서 결과 집합의 형식이 변경 될 수 있으므로 보고서에서 절 없이를 사용 하지 마세요.

다음 예제에서는 `string` 가 속성의 올바른 형식이 `geo_id` 고 `int32` 속성의 올바른 형식 이라고 가정 합니다 `cases` .

```sql
SELECT geo_id, cases = SUM(cases)
FROM OPENROWSET(
      'CosmosDB'
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Ecdc
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string',
             cases INT '$.cases.int32'
    ) as rows
GROUP BY geo_id
```

`geo_id` `cases` 다른 형식이 있는 및의 값은 값으로 반환 됩니다 `NULL` . 이 쿼리는 `cases` 식에서 지정 된 형식의만 참조 합니다 ( `cases.int32` ).

`cases.int64`Azure Cosmos DB 컨테이너에서 정리할 수 없는 다른 형식 (,)의 값이 있는 경우 `cases.float64` 절에서 명시적으로 참조 하 `WITH` 고 결과를 결합 해야 합니다. 다음 쿼리는 `int32` `int64` `float64` 열에 저장 된, 및를 집계 합니다 `cases` .

```sql
SELECT geo_id, cases = SUM(cases_int) + SUM(cases_bigint) + SUM(cases_float)
FROM OPENROWSET(
      'CosmosDB',
      'account=MyCosmosDbAccount;database=covid;region=westus2;key=C0Sm0sDbKey==',
       Ecdc
    ) WITH ( geo_id VARCHAR(50) '$.geo_id.string', 
             cases_int INT '$.cases.int32',
             cases_bigint BIGINT '$.cases.int64',
             cases_float FLOAT '$.cases.float64'
    ) as rows
GROUP BY geo_id
```

이 예에서는 사례 수가 `int32` , 또는 값으로 저장 됩니다 `int64` `float64` . 국가 당 사례 수를 계산 하려면 모든 값을 추출 해야 합니다.

## <a name="known-issues"></a>알려진 문제

- 서버를 사용 하지 않는 SQL 풀에서 [Azure Cosmos DB 전체 충실도 스키마](#full-fidelity-schema) 를 제공 하는 쿼리 환경은 미리 보기 피드백에 따라 변경 되는 임시 동작입니다. `OPENROWSET` `WITH` 사용자 의견을 기반으로 하는 쿼리 환경이 잘 정의 된 스키마와 정렬 될 수 있으므로, 절이 없는 함수가 공개 미리 보기 중에 제공 하는 스키마를 사용 하지 마세요. 사용자 의견을 제공 하려면 [Azure Synapse 링크 제품 팀](mailto:cosmosdbsynapselink@microsoft.com)에 문의 하세요.
- 서버를 `OPENROWSET` 사용 하지 않는 SQL 풀은 열 데이터 정렬에 utf-8 인코딩이 없으면 컴파일 시간 경고를 반환 합니다. `OPENROWSET`T-sql 문을 사용 하 여 현재 데이터베이스에서 실행 되는 모든 함수에 대 한 기본 데이터 정렬을 쉽게 변경할 수 있습니다 `alter database current collate Latin1_General_100_CI_AS_SC_UTF8` .

가능한 오류 및 문제 해결 작업은 다음 표에 나와 있습니다.

| 오류 | 근본 원인 |
| --- | --- |
| 구문 오류:<br/> -근처의 구문이 잘못 되었습니다. `Openrowset`<br/> - `...` 은 (는) 인식할 수 없는 `BULK OPENROWSET` 공급자 옵션입니다.<br/> -근처의 구문이 잘못 되었습니다. `...` | 가능한 근본 원인은 다음과 같습니다.<br/> -첫 번째 매개 변수로 CosmosDB를 사용 하지 않습니다.<br/> -세 번째 매개 변수에서 식별자 대신 문자열 리터럴을 사용 합니다.<br/> -세 번째 매개 변수 (컨테이너 이름)를 지정 하지 않습니다. |
| CosmosDB 연결 문자열에 오류가 있습니다. | -계정, 데이터베이스 또는 키가 지정 되지 않았습니다. <br/> -인식 되지 않는 연결 문자열에는 몇 가지 옵션이 있습니다.<br/> -세미콜론 ( `;` )은 연결 문자열의 끝에 배치 됩니다. |
| "잘못 된 계정 이름" 또는 "잘못 된 데이터베이스 이름" 오류로 인해 CosmosDB 경로를 확인 하지 못했습니다. | 지정 된 계정 이름, 데이터베이스 이름 또는 컨테이너를 찾을 수 없거나 지정 된 컬렉션에 대해 분석 저장소를 사용 하도록 설정 하지 않았습니다.|
| "잘못 된 비밀 값" 또는 "암호가 null 이거나 비어 있습니다." 오류로 인해 CosmosDB 경로를 확인 하지 못했습니다. | 계정 키가 잘못 되었거나 없습니다. |
| 유형의 열이 `column name` `type name` 외부 데이터 유형과 호환 되지 않습니다 `type name` . | 절에서 지정 된 열 유형이 `WITH` Azure Cosmos DB 컨테이너의 유형과 일치 하지 않습니다. Azure Cosmos DB 섹션에 설명 된 대로 [SQL 형식 매핑에 대](#azure-cosmos-db-to-sql-type-mappings)한 열 형식으로 변경 하거나 형식을 사용 하십시오 `VARCHAR` . |
| `NULL`모든 셀의 값을 포함 하는 열입니다. | 절에 잘못 된 열 이름 또는 경로 식이 있을 수 있습니다 `WITH` . 절에서 열 이름 (또는 열 유형 뒤의 경로 식)은 `WITH` Azure Cosmos DB 컬렉션의 일부 속성 이름과 일치 해야 합니다. 비교는 *대/소문자를 구분* 합니다. 예를 들어 `productCode` 및 `ProductCode` 는 서로 다른 속성입니다. |

[Azure Synapse Analytics 피드백 페이지](https://feedback.azure.com/forums/307516-azure-synapse-analytics?category_id=387862)에서 제안 및 문제를 보고할 수 있습니다.

## <a name="next-steps"></a>다음 단계

자세한 내용은 다음 문서를 참조하세요.

- [Azure Synapse 링크를 사용 하 여 Power BI 및 서버 리스 SQL 풀 사용](../../cosmos-db/synapse-link-power-bi.md)
- [서버를 사용 하지 않는 SQL 풀에서 보기 만들기 및 사용](create-use-views.md)
- [Azure Cosmos DB에 대해 서버 리스 SQL 풀 뷰를 빌드하고 DirectQuery를 통해 Power BI 모델에 연결 하는 방법에 대 한 자습서](./tutorial-data-analyst.md)
