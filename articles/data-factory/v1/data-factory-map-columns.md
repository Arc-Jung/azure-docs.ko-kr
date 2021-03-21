---
title: Azure Data Factory의 데이터 집합 열 매핑
description: 원본 열을 대상 열에 매핑하는 방법에 대해 알아봅니다.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/10/2018
robots: noindex
ms.openlocfilehash: fef2c6f120ae25e6aa1846d4971ff707da9bab92
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100371127"
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>원본 데이터 세트 열을 대상 데이터 세트 열에 매핑
> [!NOTE]
> 이 아티클은 Data Factory 버전 1에 적용됩니다. 

열 매핑을 사용 하 여 원본 테이블의 "구조"에 지정 된 열이 싱크 테이블의 "structure"에 지정 된 열에 매핑되는 방식을 지정할 수 있습니다. **columnMapping** 속성은 Copy 작업의 **typeProperties** 섹션에서 사용할 수 있습니다.

열 매핑에서는 다음과 같은 시나리오가 지원됩니다.

* 원본 데이터 세트 구조에 있는 모든 열이 싱크 데이터 세트 구조의 모든 열에 매핑됩니다.
* 원본 데이터 세트 구조에 있는 열의 하위 집합이 싱크 데이터 세트 구조의 모든 열에 매핑됩니다.

다음은 예외가 발생하는 오류 조건입니다.

* 매핑에 지정 된 것 보다 더 많은 열이 싱크 테이블의 "구조"에 있습니다.
* 중복 매핑
* SQL 쿼리 결과에는 매개 변수 매핑에서 지정한 열 이름이 없습니다.

> [!NOTE]
> 다음 샘플은 Azure SQL 및 Azure Blob에 대한 것이지만, 직사각 데이터 세트를 지원하는 모든 데이터 저장소에 적용할 수 있습니다. 예에서 관련 데이터 원본의 데이터를 가리키도록 데이터 세트와 연결 서비스를 조정합니다.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>예제 1 – Azure SQL에서 Azure Blob으로의 열 매핑
이 샘플에서 입력 테이블에는 구조가 있고 Azure SQL Database의 SQL 테이블을 가리킵니다.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

File Storage에서 Blob Storage로 복사할 경우 기본 Blob 형식은 블록 Blob입니다.  옵션을 지정하면 사용자가 대상 Blob 유형을 변경할 수 있습니다.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

다음 JSON은 파이프라인으로 복사 작업을 정의합니다. 원본의 열과 싱크(**columnMappings**)의 열 간 매핑은 **Translator** 속성을 사용하여 이루어집니다.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**열 매핑 흐름:**

![열 매핑 흐름](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>예제 2 – SQL 쿼리를 사용하여 Azure SQL에서 Azure Blob로 열 매핑
이 샘플에서는 "structure" 섹션에서 테이블 이름과 열 이름을 단순히 지정 하는 대신 SQL 쿼리를 사용 하 여 Azure SQL에서 데이터를 추출 합니다. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
이 경우 쿼리 결과는 먼저 원본의 "structure"에 지정 된 열에 매핑됩니다. 그런 다음 원본 "structure"의 열이 columnMappings 지정 된 규칙을 사용 하 여 싱크 "structure"의 열에 매핑됩니다.  쿼리에서 원본의 "structure"에 지정 된 열 보다 두 개의 열을 반환 하는 5 개의 열을 반환 한다고 가정 합니다.

**열 매핑 흐름**

![열 매핑 흐름 - 2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>다음 단계
복사 작업 사용에 대한 자습서 문서를 참조하세요. 

- [Blob Storage에서 SQL Database로 데이터 복사](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
