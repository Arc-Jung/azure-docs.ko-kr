---
title: Spark를 사용하여 Cassandra API 테이블 데이터 읽기
titleSufix: Azure Cosmos DB
description: 이 문서에서는 Azure Cosmos DB의 Cassandra API 테이블의 데이터를 읽는 방법을 설명합니다.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 06/02/2020
ms.custom: seodec18
ms.openlocfilehash: ceede96cbf3be12a6129e27d34e318e4c4163458
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93073499"
---
# <a name="read-data-from-azure-cosmos-db-cassandra-api-tables-using-spark"></a>Spark를 사용하여 Azure Cosmos DB Cassandra API 테이블의 데이터 읽기
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

 이 문서에서는 Spark에서 Azure Cosmos DB Cassandra API에 저장된 데이터를 읽는 방법을 설명합니다.

## <a name="cassandra-api-configuration"></a>Cassandra API 구성
```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")
//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```
## <a name="dataframe-api"></a>데이터 프레임 API

### <a name="read-table-using-sessionreadformat-command"></a>session.read.format 명령을 사용하여 테이블 읽기

```scala
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

readBooksDF.explain
readBooksDF.show
```
### <a name="read-table-using-sparkreadcassandraformat"></a>spark.read.cassandraFormat을 사용하여 테이블 읽기 

```scala
val readBooksDF = spark.read.cassandraFormat("books", "books_ks", "").load()
```

### <a name="read-specific-columns-in-table"></a>테이블의 특정 열 읽기

```scala
val readBooksDF = spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_author", "book_pub_year")

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

### <a name="apply-filters"></a>필터 적용

조건부를 데이터베이스에 푸시하여 더 나은 최적화 된 Spark 쿼리를 사용할 수 있습니다. 조건자는 일반적으로 WHERE 절에 있는 true 또는 false를 반환 하는 쿼리에 대 한 조건입니다. 조건자 푸시 다운은 데이터베이스 쿼리의 데이터를 필터링 하 여 데이터베이스에서 검색 되는 항목 수를 줄이고 쿼리 성능을 향상 시킵니다. 기본적으로 Spark 데이터 집합 API는 유효한 WHERE 절을 데이터베이스에 자동으로 푸시합니다. 

```scala
val df = spark.read.cassandraFormat("books", "books_ks").load
df.explain
val dfWithPushdown = df.filter(df("book_pub_year") > 1891)
dfWithPushdown.explain

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

물리적 계획의 PushedFilters 섹션에는 GreaterThan push down 필터가 포함 됩니다. 

:::image type="content" source="./media/cassandra-spark-read-ops/pushdown-predicates.png" alt-text="영역":::

## <a name="rdd-api"></a>RDD API

### <a name="read-table"></a>테이블 읽기
```scala
val bookRDD = sc.cassandraTable("books_ks", "books")
bookRDD.take(5).foreach(println)
```

### <a name="read-specific-columns-in-table"></a>테이블의 특정 열 읽기

```scala
val booksRDD = sc.cassandraTable("books_ks", "books").select("book_id","book_name").cache
booksRDD.take(5).foreach(println)
```

## <a name="sql-views"></a>SQL 보기 

### <a name="create-a-temporary-view-from-a-dataframe"></a>데이터 프레임에서 임시 보기 만들기

```scala
spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load.createOrReplaceTempView("books_vw")
```

### <a name="run-queries-against-the-view"></a>보기에 대해 쿼리 실행

```sql
select * from books_vw where book_pub_year > 1891
```

## <a name="next-steps"></a>다음 단계

아래에는 Spark에서 Azure Cosmos DB Cassandra API로 작업하는 방법을 소개하는 추가 문서가 나와 있습니다.
 
 * [Upsert 작업](cassandra-spark-upsert-ops.md)
 * [삭제 작업](cassandra-spark-delete-ops.md)
 * [집계 작업](cassandra-spark-aggregation-ops.md)
 * [테이블 복사 작업](cassandra-spark-table-copy-ops.md)

