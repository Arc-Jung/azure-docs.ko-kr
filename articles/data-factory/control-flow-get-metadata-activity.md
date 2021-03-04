---
title: Azure Data Factory에서 메타 데이터 가져오기 작업
description: Data Factory 파이프라인에서 메타 데이터 가져오기 작업을 사용 하는 방법에 대해 알아봅니다.
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/25/2021
ms.author: jingwang
ms.openlocfilehash: 151f4352ce7c845050c899792fd7285c97f844bc
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102049987"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Azure Data Factory에서 메타 데이터 가져오기 작업

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

메타 데이터 가져오기 작업을 사용 하 여 Azure Data Factory에 있는 모든 데이터의 메타 데이터를 검색할 수 있습니다. 조건 식에서 메타 데이터 가져오기 작업의 출력을 사용 하 여 유효성 검사를 수행 하거나 후속 작업에서 메타 데이터를 사용할 수 있습니다.

## <a name="supported-capabilities"></a>지원되는 기능

메타 데이터 가져오기 작업은 데이터 집합을 입력으로 사용 하 고 메타 데이터 정보를 출력으로 반환 합니다. 현재 다음 커넥터와 해당 하는 검색할 수 있는 메타 데이터가 지원 됩니다. 반환 되는 메타 데이터의 최대 크기는 **4mb** 입니다.

### <a name="supported-connectors"></a>지원되는 커넥터

**File Storage**

| 커넥터/메타데이터 | itemName<br>(파일/폴더) | itemType<br>(파일/폴더) | 크기<br>(파일) | created<br>(파일/폴더) | lastModified<sup>1</sup><br>(파일/폴더) |childItems<br>(폴더) |contentMD5<br>(파일) | 구조<sup>2</sup><br/>(파일) | columnCount<sup>2</sup><br>(파일) | exists<sup>3</sup><br>(파일/폴더) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Google Cloud Storage](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Blob Storage](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | √ | √ | √ | √/√ |
| [Azure 파일](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [파일 시스템](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | x/x | √ | x | √ | √ | √/√ |

<sup>1</sup> 메타 데이터 `lastModified` :
- Amazon S3 및 Google Cloud Storage의 경우는 `lastModified` 버킷 및 키에 적용 되 고 가상 폴더에는 적용 되지 않으며, `exists` 버킷 및 키에 적용 되 고 접두사 또는 가상 폴더에는 적용 되지 않습니다. 
- Azure Blob storage의 경우 `lastModified` 컨테이너와 Blob에는 적용 되지만 가상 폴더에는 적용 되지 않습니다.

<sup>2</sup> 메타 데이터 `structure` 및 `columnCount` 는 이진 파일, JSON 또는 XML 파일에서 메타 데이터를 가져올 때 지원 되지 않습니다.

<sup>3</sup> 메타 데이터 `exists` : Amazon S3 및 Google Cloud Storage의 경우 `exists` 버킷 및 키에 적용 되 고 접두사 또는 가상 폴더에는 적용 되지 않습니다.

다음 사항에 유의하세요.

- 폴더에 대해 메타 데이터 가져오기 작업을 사용 하는 경우 지정 된 폴더에 대 한 목록/실행 권한이 있는지 확인 합니다.
- 폴더/파일에 대 한 와일드 카드 필터는 메타 데이터 가져오기 작업에 대해 지원 되지 않습니다.
- `modifiedDatetimeStart``modifiedDatetimeEnd`커넥터에 설정 되는 필터:

    - 이러한 두 속성은 폴더에서 메타 데이터를 가져올 때 자식 항목을 필터링 하는 데 사용 됩니다. 파일에서 메타 데이터를 가져올 때에는 적용 되지 않습니다.
    - 이러한 필터를 사용 하는 경우 `childItems` 출력에는 지정 된 범위 내에서 수정 된 파일만 포함 되 고 폴더는 포함 되지 않습니다.
    - 이러한 필터를 적용 하기 위해 GetMetadata 작업은 지정 된 폴더의 모든 파일을 열거 하 고 수정 된 시간을 확인 합니다. 예상 되는 정규화 된 파일 수가 작은 경우에도 파일 수가 많은 폴더를 가리키지 않도록 합니다. 

**관계형 데이터베이스**

| 커넥터/메타데이터 | structure | columnCount | exists |
|:--- |:--- |:--- |:--- |
| [Azure SQL Database](connector-azure-sql-database.md) | √ | √ | √ |
| [Azure SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md) | √ | √ | √ |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>메타데이터 옵션

메타 데이터 가져오기 작업 필드 목록에서 다음 메타 데이터 형식을 지정 하 여 해당 정보를 검색할 수 있습니다.

| 메타데이터 유형 | 설명 |
|:--- |:--- |
| itemName | 파일 또는 폴더의 이름입니다. |
| itemType | 파일 또는 폴더의 형식입니다. 반환 된 값은 `File` 또는 `Folder` 입니다. |
| 크기 | 파일의 크기 (바이트)입니다. 파일에만 적용 됩니다. |
| created | 파일 또는 폴더를 만든 날짜/시간입니다. |
| lastModified | 파일 또는 폴더를 마지막으로 수정한 날짜/시간입니다. |
| childItems | 지정 된 폴더의 하위 폴더 및 파일 목록입니다. 폴더에만 적용 됩니다. 반환 된 값은 각 자식 항목의 이름과 형식 목록입니다. |
| contentMD5 | 파일의 MD5입니다. 파일에만 적용 됩니다. |
| structure | 파일 또는 관계형 데이터베이스 테이블의 데이터 구조입니다. 반환 값은 열 이름과 열 형식의 목록입니다. |
| columnCount | 파일이 나 관계형 테이블의 열 수입니다. |
| exists| 파일, 폴더 또는 테이블이 있는지 여부입니다. `exists`메타 데이터 가져오기 필드 목록에가 지정 된 경우에는 파일, 폴더 또는 테이블이 없어도 작업이 실패 합니다. 대신 `exists: false` 가 출력에서 반환 됩니다. |

>[!TIP]
>파일, 폴더 또는 테이블이 있는지 확인 하려면 `exists` 메타 데이터 가져오기 작업 필드 목록에서를 지정 합니다. 그런 다음 `exists: true/false` 활동 출력의 결과를 확인할 수 있습니다. `exists`필드 목록에을 지정 하지 않으면 개체를 찾을 수 없는 경우 메타 데이터 가져오기 작업이 실패 합니다.

## <a name="syntax"></a>구문

**메타 데이터 가져오기 작업**

```json
{
    "name":"MyActivity",
    "type":"GetMetadata",
    "dependsOn":[

    ],
    "policy":{
        "timeout":"7.00:00:00",
        "retry":0,
        "retryIntervalInSeconds":30,
        "secureOutput":false,
        "secureInput":false
    },
    "userProperties":[

    ],
    "typeProperties":{
        "dataset":{
            "referenceName":"MyDataset",
            "type":"DatasetReference"
        },
        "fieldList":[
            "size",
            "lastModified",
            "structure"
        ],
        "storeSettings":{
            "type":"AzureBlobStorageReadSettings"
        },
        "formatSettings":{
            "type":"JsonReadSettings"
        }
    }
}
```

**데이터 세트**

```json
{
    "name":"MyDataset",
    "properties":{
        "linkedServiceName":{
            "referenceName":"AzureStorageLinkedService",
            "type":"LinkedServiceReference"
        },
        "annotations":[

        ],
        "type":"Json",
        "typeProperties":{
            "location":{
                "type":"AzureBlobStorageLocation",
                "fileName":"file.json",
                "folderPath":"folder",
                "container":"container"
            }
        }
    }
}
```

## <a name="type-properties"></a>형식 속성

현재, 메타 데이터 가져오기 작업은 다음과 같은 유형의 메타 데이터 정보를 반환할 수 있습니다.

속성 | 설명 | 필수
-------- | ----------- | --------
fieldList | 필요한 메타 데이터 정보의 형식입니다. 지원 되는 메타 데이터에 대 한 자세한 내용은이 문서의 [메타 데이터 옵션](#metadata-options) 섹션을 참조 하세요. | 예 
데이터 세트 | 메타 데이터 가져오기 작업에서 메타 데이터를 검색할 참조 데이터 집합입니다. 지원 되는 커넥터에 대 한 자세한 내용은 [기능](#supported-capabilities) 섹션을 참조 하세요. 데이터 집합 구문에 대 한 자세한 내용은 특정 커넥터 항목을 참조 하세요. | 예
formatSettings | 서식 유형 데이터 집합을 사용 하는 경우 적용 합니다. | No
나이 설정 | 서식 유형 데이터 집합을 사용 하는 경우 적용 합니다. | No

## <a name="sample-output"></a>샘플 출력

메타 데이터 가져오기 결과는 활동 출력에 표시 됩니다. 다음은 광범위 한 메타 데이터 옵션을 보여 주는 두 샘플입니다. 후속 작업에서 결과를 사용 하려면 다음 패턴을 사용 합니다. `@{activity('MyGetMetadataActivity').output.itemName}`

### <a name="get-a-files-metadata"></a>파일의 메타데이터 가져오기

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>폴더의 메타데이터 가져오기

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>다음 단계
Data Factory에서 지 원하는 다른 제어 흐름 작업에 대해 알아봅니다.

- [파이프라인 실행 작업](control-flow-execute-pipeline-activity.md)
- [ForEach 작업](control-flow-for-each-activity.md)
- [조회 작업](control-flow-lookup-activity.md)
- [웹 작업](control-flow-web-activity.md)
