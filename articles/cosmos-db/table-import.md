---
title: Azure Cosmos DB에서 Table API 계정으로 기존 데이터 마이그레이션
description: Azure Cosmos DB에서 온-프레미스 또는 클라우드 데이터를 Azure Table API 계정으로 마이그레이션하거나 가져오는 방법을 알아봅니다.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/07/2017
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: e876ca028532bb3721146e90a91d68c4c12bf79f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93096082"
---
# <a name="migrate-your-data-to-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Table API 계정으로 데이터 마이그레이션
[!INCLUDE[appliesto-table-api](includes/appliesto-table-api.md)]

이 자습서에서는 Azure Cosmos DB [테이블 API](table-introduction.md)로 사용할 데이터 가져오기에 대한 지침을 제공합니다. Azure Table Storage에 저장된 데이터가 있는 경우 데이터 마이그레이션 도구 또는 AzCopy를 사용하여 데이터를 Azure Cosmos DB Table API로 가져올 수 있습니다. 데이터가 Azure Cosmos DB 테이블 API(미리 보기) 계정에 저장된 경우 데이터 마이그레이션 도구를 사용하여 데이터를 마이그레이션해야 합니다. 

이 자습서에서 다루는 작업은 다음과 같습니다.

> [!div class="checklist"]
> * 데이터 마이그레이션 도구를 사용하여 데이터 가져오기
> * AzCopy를 사용하여 데이터 가져오기
> * 테이블 API(미리 보기)에서 테이블 API로 마이그레이션 

## <a name="prerequisites"></a>필수 구성 요소

* **처리량 증가:** 데이터 마이그레이션 기간은 개별 컨테이너 또는 컨테이너 세트에 대해 설정한 처리량에 따라 달라집니다. 대량 데이터 마이그레이션의 경우 처리량을 늘려야 합니다. 마이그레이션을 완료한 후에는 비용을 절약하기 위해 처리량을 줄이세요. Azure Portal에서 처리량을 늘리는 방법에 대한 자세한 내용은 Azure Cosmos DB의 성능 수준 및 가격 책정 계층을 참조하세요.

* **Azure Cosmos DB 리소스 만들기:** 데이터 마이그레이션을 시작하기 전에 Azure Portal에서 모든 테이블을 미리 만듭니다. 데이터베이스 수준 처리량이 있는 Azure Cosmos DB 계정으로 마이그레이션하는 경우에는 Azure Cosmos DB 테이블을 만들 때 파티션 키를 제공해야 합니다.

## <a name="data-migration-tool"></a>데이터 마이그레이션 도구

명령줄 Azure Cosmos DB 데이터 마이그레이션 도구(dt.exe)를 사용하여 기존의 Azure Table Storage 데이터를 테이블 API GA 계정으로 가져오거나 테이블 API(미리 보기) 계정에서 테이블 API GA 계정으로 데이터를 마이그레이션할 수 있습니다. 다른 원본은 현재 지원되지 않습니다. UI 기반 데이터 마이그레이션 도구(dtui.exe)는 현재 Table API 계정에 대해 지원되지 않습니다. 

테이블 데이터 마이그레이션을 수행하려면 다음 작업을 완료하세요.

1. 마이그레이션 도구를 [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool)에서 다운로드합니다.
2. 시나리오에 맞는 명령줄 인수를 사용하여 `dt.exe`를 실행합니다. `dt.exe`에서 사용하는 형식은 다음과 같습니다.

   ```bash
    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 
   ```

이 명령에 지원되는 옵션은 다음과 같습니다.

* **/ErrorLog:** (선택 사항) 데이터 전송 실패를 리디렉션하는 CSV 파일 이름
* **/OverwriteErrorLog:** (선택 사항) 오류 로그 파일 덮어쓰기
* **/ProgressUpdateInterval:** 선택 사항입니다. 기본값은 00:00:01입니다. 화면 데이터 전송 진행률을 새로 고치는 시간 간격
* **/ErrorDetails:** 선택 사항입니다. 기본값은 None입니다. 다음 오류에 대한 자세한 오류 정보를 표시하도록 지정합니다. 없음, 중요, 모두
* **/EnableCosmosTableLog:** (선택 사항) 로그를 cosmos 테이블 계정으로 보냅니다. 설정하는 경우 /CosmosTableLogConnectionString도 제공되지 않는 한 이 기본값은 대상 계정 연결 문자열입니다. 이는 DT의 여러 인스턴스가 동시에 실행되는 경우에 유용합니다.
* **/CosmosTableLogConnectionString:** (선택 사항) 로그를 원격 cosmos 테이블 계정으로 보내기 위한 ConnectionString입니다.

### <a name="command-line-source-settings"></a>명령줄 원본 설정

Azure Table Storage 또는 테이블 API 미리 보기를 마이그레이션 원본으로 정의할 때는 다음 원본 옵션을 사용하세요.

* **/s:AzureTable:** Azure Table 스토리지에서 데이터 읽기
* **/s.ConnectionString:** 테이블 엔드포인트의 연결 문자열입니다. 이는 Azure Portal에서 검색할 수 있습니다.
* **/s.LocationMode:** 선택 사항입니다. 기본값은 PrimaryOnly입니다. Azure Table 스토리지에 연결할 때 사용할 위치 모드를 지정합니다. PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
* **/s.Table:** Azure Table의 이름
* **/s.InternalFields:** RowKey 및 PartitionKey가 가져오기에 필요하므로 테이블 마이그레이션의 경우 모두로 설정합니다.
* **/s.Filter:** (선택 사항) 적용할 필터 문자열
* **/s.Projection:** (선택 사항) 선택할 열 목록

Azure Table Storage에서 가져올 때 원본 연결 문자열을 검색하려면 Azure Portal을 열고 **스토리지 계정** > **계정** > **액세스 키** 를 클릭한 후 복사 단추를 사용하여 **연결 문자열** 을 복사합니다.

:::image type="content" source="./media/table-import/storage-table-access-key.png" alt-text="스토리지 계정 > 계정 > 액세스 키 옵션을 보여주고 복사 단추를 강조 표시하는 스크린샷.":::

Azure Cosmos DB 테이블 API(미리 보기) 계정에서 가져올 때 원본 연결 문자열을 검색하려면 Azure Portal을 열고 **Azure Cosmos DB** > **계정** > **연결 문자열** 을 클릭하고 복사 단추를 사용하여 **연결 문자열** 을 복사합니다.

:::image type="content" source="./media/table-import/cosmos-connection-string.png" alt-text="HBase 원본 옵션의 스크린샷":::

[Azure Table Storage 명령 예제](#azure-table-storage)

[Azure Cosmos DB 테이블 API(미리 보기) 명령 예제](#table-api-preview)

### <a name="command-line-target-settings"></a>명령줄 대상 설정

Azure Cosmos DB 테이블 API를 마이그레이션 대상으로 정의할 때는 다음 대상 옵션을 사용하세요.

* **/t:TableAPIBulk:** 일괄 처리로 데이터를 Azure CosmosDB Table에 업로드
* **/t.ConnectionString:** 테이블 엔드포인트의 연결 문자열
* **/t.TableName:** 쓸 테이블의 열 이름 지정
* **/t.Overwrite:** 선택 사항입니다. 기본값은 false입니다. 기존 값을 덮어쓸지 여부를 지정
* **/t.MaxInputBufferSize:** 선택 사항입니다. 기본값은 1GB입니다. 싱크로 데이터를 플러시하기 전에 버퍼에 입력할 바이트의 대략적인 추정치
* **/t.Throughput:** 선택 사항입니다. 지정되지 않은 경우 서비스 기본값입니다. 테이블에 구성할 처리량 지정
* **/t.MaxBatchSize:** 선택 사항입니다. 기본값은 2MB입니다. 일괄 처리 크기(바이트) 지정

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>명령 샘플: 원본이 Azure Table 스토리지인 경우

Azure Table Storage에서 테이블 API로 가져오는 방법을 보여주는 명령줄 예제는 다음과 같습니다.

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>명령 샘플: 원본이 Azure Cosmos DB Table API(미리 보기)인 경우

테이블 API 미리 보기에서 테이블 API GA로 가져오는 명령줄 예제는 다음과 같습니다.

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="migrate-data-by-using-azcopy"></a>AzCopy를 사용하여 데이터 마이그레이션

AzCopy 명령줄 유틸리티를 사용하는 것은 Azure Table Storage에서 Azure Cosmos DB 테이블 API로 데이터를 마이그레이션하는 다른 옵션입니다. AzCopy를 사용하려면 먼저 [Table Storage에서 데이터 내보내기](/previous-versions/azure/storage/storage-use-azcopy#export-data-from-table-storage)에 설명된 대로 데이터를 내보낸 다음 [Azure Cosmos DB 테이블 API](/previous-versions/azure/storage/storage-use-azcopy#import-data-into-table-storage)에 설명된 대로 Azure Cosmos DB로 데이터를 가져옵니다.

Azure Cosmos DB에서 가져오기를 수행할 때는 다음 예제를 참조하세요. /Dest 값이 코어가 아닌 cosmosdb를 사용한다는 점을 참고하세요.

가져오기 명령 예제:

```bash
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>테이블 API(미리 보기)에서 테이블 API로 마이그레이션

> [!WARNING]
> 일반 공급 테이블의 이점을 즉시 누리려면 이 섹션에 지정된 기존의 미리 보기를 마이그레이션하세요. 그렇지 않을 경우 앞으로 몇 주 동안 기존 미리 보기 고객을 위한 자동 마이그레이션이 수행됩니다. 하지만 자동으로 마이그레이션된 미리 보고 테이블은 새로 생성된 테이블과 달리 특정한 제한 사항이 있다는 사실을 참고하세요.

테이블 API는 현재 일반 공급(GA)됩니다. 클라우드에서 실행되는 코드와 클라이언트에서 실행되는 코드에서 미리 보기 버전과 GA 버전의 테이블에는 차이점이 있습니다. 따라서 GA 테이블 API 계정과 미리 보기 SDK 클라이언트를 혼합하거나 그 반대로 결합하려는 시도는 하지 않는 것이 좋습니다. 기존의 테이블을 프로덕션 환경에서 계속 사용하고자 하는 테이블 API 미리 보기 고객은 미리 보기에서 GA 환경으로 마이그레이션하거나 자동 마이그레이션을 기다려야 합니다. 자동 마이그레이션을 기다리는 경우 마이그레이션된 테이블에 대한 제한 사항 알림이 수신될 것입니다. 마이그레이션 후에는 제한 사항 없이 기존 게정으로 새 테이블을 만들 수 있습니다(마이그레이션된 테이블에만 제한 사항이 있음).

테이블 API(미리 보기)에서 일반 공급 테이블 API로 마이그레이션하려면:

1. [데이터베이스 계정 만들기](create-table-dotnet.md#create-a-database-account)에 설명된 대로새 Azure Cosmos DB 계정을 만들고 API 유형을 Azure 테이블로 설정합니다.

2. [테이블 API SDK](table-sdk-dotnet.md)의 GA 릴리스를 사용하도록 클라이언트를 변경합니다.

3. 데이터 마이그레이션 도구를 사용하여 미리 보기 테이블의 클라이언트 데이터를 GA 테이블로 마이그레이션합니다. 이러한 용도로 데이터 마이그레이션 도구를 사용하는 방법에 대한 지침은 [데이터 마이그레이션 도구](#data-migration-tool)에 설명되어 있습니다. 

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 데이터 마이그레이션 도구를 사용하여 데이터 가져오기
> * AzCopy를 사용하여 데이터 가져오기
> * 테이블 API(미리 보기)에서 테이블 API로 마이그레이션

이제 다음 자습서로 진행하여 Azure Cosmos DB 테이블 API를 사용하여 데이터를 쿼리하는 방법을 알아볼 수 있습니다. 

> [!div class="nextstepaction"]
>[데이터는 어떻게 쿼리하나요?](../cosmos-db/tutorial-query-table.md)