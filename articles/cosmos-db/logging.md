---
title: Azure Cosmos DB의 진단 로깅
description: Azure Cosmos DB에 저장된 데이터를 기록 및 모니터링하는 다양한 방법에 대해 알아봅니다.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: d8a9963edd689a32ae0642ac6fa4a622c248bc5b
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232372"
---
# <a name="diagnostic-logging-in-azure-cosmos-db"></a>Azure Cosmos DB의 진단 로깅 

하나 이상의 Azure Cosmos 데이터베이스 사용을 시작한 후에는 데이터베이스에 액세스 하는 방법과 시기를 모니터링할 수 있습니다. 이 문서에서는 Azure 플랫폼에서 사용할 수 있는 로그의 개요를 제공합니다. 모니터링을 위해 진단 로깅을 사용 하도록 설정 하 여 [Azure Storage](https://azure.microsoft.com/services/storage/)에 로그를 전송 하는 방법, 로그를 [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)로 스트리밍하는 방법, 로그를 [Azure Monitor 로그](https://azure.microsoft.com/services/log-analytics/)로 내보내는 방법에 대해 알아봅니다.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="logs-available-in-azure"></a>Azure에서 사용할 수 있는 로그

Azure Cosmos DB 계정 모니터링 방법을 논의하기 전에 로깅과 모니터링에 관한 몇 가지 사항을 확인해 보겠습니다. Azure 플랫폼에는 여러 로그 유형이 있습니다. 바로 [Azure 활동 로그](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [Azure 진단 로그](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [Azure 메트릭](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), 이벤트, 하트비트 모니터링, 작업 로그 등입니다. 로그는 다양합니다. Azure Portal에서 [Azure Monitor 로그](https://azure.microsoft.com/services/log-analytics/) 의 전체 로그 목록을 볼 수 있습니다. 

다음 이미지는 사용할 수 있는 다양 한 종류의 Azure 로그를 보여 줍니다.

![다양한 종류의 Azure 로그](./media/logging/azurelogging.png)

이 이미지에서 **컴퓨팅 리소스**는 Microsoft 게스트 OS에 액세스할 수 있는 Azure 리소스를 나타냅니다. 예를 들어, Azure Virtual Machines, 가상 머신 확장 집합, Azure Container Service 등은 컴퓨팅 리소스로 간주됩니다. 컴퓨팅 리소스는 활동 로그, 진단 로그 및 애플리케이션 로그를 생성합니다. 자세한 내용은 [Azure의 모니터링 데이터 원본](../azure-monitor/platform/data-sources.md) 문서를 참조하세요.

**비계산 리소스**는 기본 OS에 액세스할 수 없고 리소스를 바로 사용하는 경우의 리소스입니다. 예를 들면 네트워크 보안 그룹, Logic Apps 등이 있습니다. Azure Cosmos DB는 비계산 리소스입니다. 활동 로그에서 또는 포털에서 진단 로그 옵션을 사용하도록 설정하여 비계산 리소스에 대한 로그를 볼 수 있습니다. 자세한 내용은 [Azure Monitor의 데이터 원본](../azure-monitor/platform/data-sources.md) 문서를 참조하세요.

활동 로그는 Azure Cosmos DB에 대한 구독 수준에서 작업을 기록합니다. Listkey, Write DatabaseAccounts 등의 작업이 로깅됩니다. 진단 로그는 보다 세부적인 로깅을 제공하고 DataPlaneRequests(만들기, 읽기, 쿼리 등) 및 MongoRequests를 로깅할 수 있도록 합니다.


이 문서에서는 Azure 활동 로그, Azure 진단 로그 및 Azure 메트릭을 집중적으로 설명합니다. 이 세 로그의 차이는 무엇인가요? 

### <a name="azure-activity-log"></a>Azure 동작 로그

Azure Activity Log는 Azure에서 발생하는 구독 수준 이벤트에 대한 정보를 제공하는 구독 로그입니다. 활동 로그는 관리 범주 하에서 구독에 대한 제어-평면 이벤트를 보고합니다. 활동 로그를 사용하여 구독의 리소스에 대한 모든 쓰기 작업(PUT, POST, DELETE)에서 '무엇을, 누가, 언제'를 판단할 수 있습니다. 또한 작업 및 기타 관련 속성의 상태도 이해할 수 있습니다. 

활동 로그는 진단 로그와 다릅니다. 활동 로그는 외부(_제어 평면_)의 리소스에 대한 작업 관련 데이터를 제공합니다. Azure Cosmos DB 컨텍스트에서 제어 평면 작업에는 컨테이너 만들기, 키 나열, 키 삭제, 데이터베이스 나열 등이 포함됩니다. 진단 로그는 리소스에 의해 내보내지며, 해당 리소스의 작업(_데이터 평면_)에 대한 정보를 제공합니다. 진단 로그에서 데이터 평면 작업의 예에는 Delete, Insert 및 ReadFeed가 있습니다.

활동 로그(제어 평면 작업)는 본질적으로 더 다양한데, 호출자의 전체 전자 메일 주소, 호출자 IP 주소, 리소스 이름, 작업 이름 및 테넌트 ID 등을 포함할 수 있습니다. 활동 로그에는 몇 가지 데이터 [범주](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema)가 있습니다. 이러한 범주의 스키마에 대한 전체 정보는 [Azure Activity Log 이벤트 스키마](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema)를 참조하세요. 그러나 진단 로그는 개인 데이터가 제거되는 경우가 종종 있으므로 본질적으로 제한적일 수 있습니다. 호출자의 IP 주소는 있지만 마지막 팔분위가 제거됩니다.

### <a name="azure-metrics"></a>Azure 메트릭

[Azure 메트릭](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics)에는 대부분의 Azure 리소스에서 내보내는 Azure 원격 분석 데이터(_성능 카운터_라고도 함)의 가장 중요한 유형이 있습니다. 메트릭을 통해 Azure Cosmos DB 리소스의 처리량, 스토리지, 일관성, 가용성 및 대기 시간에 대한 정보를 파악할 수 있습니다. 자세한 내용은 [Azure Cosmos DB에서 메트릭을 사용하여 모니터링 및 디버깅](use-metrics.md)을 참조하세요.

### <a name="azure-diagnostic-logs"></a>Azure 진단 로그

Azure 진단 로그는 리소스에서 내보내며, 해당 리소스의 작업에 대한 풍부하고 빈번한 데이터를 제공합니다. 이러한 로그는 요청당 캡처됩니다. 이러한 로그의 내용은 리소스 종류에 따라 달라집니다. 리소스 수준 진단 로그도 게스트 OS 수준 진단 로그와 다릅니다. 게스트 OS 진단 로그는 가상 머신이나 다른 지원되는 리소스 유형 안에서 실행되는 에이전트가 수집합니다. 리소스 수준 진단 로그는 에이전트가 필요하지 않고 Azure 플랫폼 자체에서 리소스 관련 데이터를 캡처합니다. 게스트 OS 수준 진단 로그는 가상 머신에서 실행되는 운영 체제 및 애플리케이션에서 데이터를 캡처합니다.

![Storage, Event Hubs 또는 Azure Monitor 로그에 진단 로깅](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>Azure Diagnostic Logs에 기록되는 내용

* 액세스 권한, 시스템 오류 또는 잘못된 요청으로 인해 실패한 요청을 포함하여 모든 인증된 백엔드 요청(REST API)이 로깅됩니다. 사용자가 시작한 Graph, Cassandra 및 Table API 요청은 현재 사용할 수 없습니다.
* 모든 문서, 컨테이너 및 데이터베이스에 대한 CRUD 작업을 포함하는 데이터베이스 자체에 대한 작업입니다.
* 키 만들기, 수정 또는 삭제를 포함하는 계정 키에 대한 작업입니다.
* 401 응답이 발생하는 인증되지 않은 요청. 예를 들어 전달자 토큰이 없거나 형식이 잘못되거나 만료되거나 토큰이 잘못된 요청이 해당합니다.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Azure Portal에서 로깅 켜기

Azure Portal에서 진단 로깅을 사용 하도록 설정 하려면 다음 단계를 수행 합니다.

1. [Azure 포털](https://portal.azure.com)할 수 있습니다. 

1. Azure Cosmos 계정으로 이동합니다. **진단 설정** 창을 열고 **진단 설정 추가** 옵션을 선택 합니다.

    ![Azure Portal에서 Azure Cosmos DB에 대한 진단 로깅 설정](./media/logging/turn-on-portal-logging.png)

1. **진단 설정** 페이지에서 다음 세부 정보로 양식을 채웁니다. 

    * **이름**: 만들 로그에 대한 이름을 입력합니다.

    * 다음 서비스에 로그를 저장할 수 있습니다.

      * **스토리지 계정에 보관**: 이 옵션을 사용하려면 연결할 기존 스토리지 계정이 필요합니다. 포털에서 새 저장소 계정을 만들려면 [저장소 계정 만들기](../storage/common/storage-create-storage-account.md) 문서를 참조 하세요. 그런 다음 포털의 Azure Cosmos Db 진단 설정 창으로 돌아가 저장소 계정을 선택 합니다. 새로 만든 스토리지 계정이 드롭다운 메뉴에 나타나기까지 몇 분 정도 걸릴 수 있습니다.

      * **이벤트 허브로의 스트림**: 이 옵션을 사용하려면 연결할 기존 Event Hubs 네임스페이스 및 이벤트 허브가 필요합니다. Event Hubs 네임스페이스를 만들려면 [Azure Portal을 사용하여 Event Hubs 네임스페이스 및 이벤트 허브 만들기](../event-hubs/event-hubs-create.md)를 참조하세요. 그런 다음 포털의이 페이지로 돌아와서 이벤트 허브 네임 스페이스 및 정책 이름을 선택 합니다.

      * **Log Analytics에 보내기**: 이 옵션을 사용하려면 기존 작업 영역을 사용하거나 포털에서 [새 작업 영역 만들기](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace) 단계를 따라 새 Log Analytics 작업 영역을 만듭니다. 

   * 다음 데이터를 기록할 수 있습니다.

      * **DataPlaneRequests**: SQL, Graph, MongoDB, Cassandra 및 Table API 계정을 포함 하는 모든 Api에 대 한 백 엔드 요청을 Azure Cosmos DB에 기록 하려면이 옵션을 선택 합니다. 스토리지 계정으로 보관하려는 경우 진단 로그의 보존 기간을 선택할 수 있습니다. 로그는 보존 기간이 만료된 후에 자동 삭제됩니다. 다음 JSON 데이터는 DataPlaneRequests을 사용 하 여 기록 된 세부 정보 출력의 예입니다. 유의 해야 할 주요 속성은 다음과 같습니다. Requestcharge, statusCode, clientIPaddress 및 partitionID:

       ```
       { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
       ```

      * **MongoRequests**: 프런트 엔드에서 사용자가 시작한 요청을 MongoDB의 API Azure Cosmos DB에 대 한 요청을 처리 하도록 기록 하려면이 옵션을 선택 합니다. MongoDB 요청은 MongoRequests 및 DataPlaneRequests에도 표시 됩니다. 스토리지 계정으로 보관하려는 경우 진단 로그의 보존 기간을 선택할 수 있습니다. 로그는 보존 기간이 만료된 후에 자동 삭제됩니다. 다음 JSON 데이터는 MongoRequests을 사용 하 여 기록 된 세부 정보 출력의 예입니다. 유의 해야 할 주요 속성은 다음과 같습니다. Requestcharge, opCode:

       ```
       { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
       ```

      * **QueryRuntimeStatistics**: 실행 된 쿼리 텍스트를 기록 하려면이 옵션을 선택 합니다.  다음 JSON 데이터는 QueryRuntimeStatistics를 사용 하 여 기록 되는 세부 정보의 예제 출력입니다.

       ```
       { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
       ```

      * **메트릭 요청**: [Azure 메트릭](../azure-monitor/platform/metrics-supported.md)에 자세한 데이터를 저장하려면 이 옵션을 선택합니다. 스토리지 계정으로 보관하려는 경우 진단 로그의 보존 기간을 선택할 수 있습니다. 로그는 보존 기간이 만료된 후에 자동 삭제됩니다.

3. **저장**을 선택합니다.

    “\<작업 영역 이름>의 진단을 업데이트하지 못했습니다. 구독 \<구독 ID&gt;는 microsoft.insights를 사용하도록 등록되지 않았습니다." [Azure Diagnostics 문제 해결](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) 지침을 따라 계정을 등록하고 이 절차를 다시 시도하세요.

    나중에 진단 로그를 저장하는 방식을 변경하려면 이 페이지로 돌아와 사용자 계정의 진단 로그 설정을 수정합니다.

## <a name="turn-on-logging-by-using-azure-cli"></a>Azure CLI를 사용하여 로깅 설정

Azure CLI를 사용하여 메트릭 및 진단 로깅을 사용하도록 설정하려면 다음 명령을 사용합니다.

- 스토리지 계정에서 진단 로그의 스토리지를 사용하도록 설정하려면 다음 명령을 사용합니다.

   ```azurecli-interactive
   az monitor diagnostic-settings create --name DiagStorage --resource <resourceId> --storage-account <storageAccountName> --logs '[{"category": "QueryRuntimeStatistics", "enabled": true, "retentionPolicy": {"enabled": true, "days": 0}}]'
   ```

   `resource`는 Azure Cosmos DB 계정의 이름입니다. 리소스는 "/subscriptions/`<subscriptionId>`/stgg/`<resource_group_name>`/providers/Microsoft.DocumentDB/databaseAccounts/ `storage-account` < Azure_Cosmos_account_name >" 형식으로 되어 있습니다. 로그를 전송 하려고 합니다. 범주 매개 변수 값을 "MongoRequests" 또는 "DataPlaneRequests"로 업데이트 하 여 다른 로그를 기록할 수 있습니다. 

- 이벤트 허브로의 진단 로그 스트리밍을 사용하도록 설정하려면 다음 명령을 사용합니다.

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --event-hub-rule <eventHubRuleID> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

   `resource`는 Azure Cosmos DB 계정의 이름입니다. `event-hub-rule` 는 이벤트 허브 규칙 ID입니다. 

- 진단 로그를 Log Analytics 작업 영역으로 보낼 수 있게 하려면 다음 명령을 사용합니다.

   ```azurecli-interactive
   az monitor diagnostic-settings create --name cdbdiagsett --resourceId <resourceId> --workspace <resource id of the log analytics workspace> --logs '[{"category":"QueryRuntimeStatistics","enabled":true,"retentionPolicy":{"days":6,"enabled":true}}]'
   ```

이러한 매개 변수를 결합하여 여러 출력 옵션을 사용하도록 설정할 수 있습니다.

## <a name="turn-on-logging-by-using-powershell"></a>PowerShell을 사용하여 로깅 켜기

PowerShell을 사용하여 진단 로깅을 켜려면 Azure Powershell 버전 1.0.1 이상이 필요합니다.

Azure PowerShell을 설치하고 Azure 구독에 연결하려면 [Azure PowerShell 설치 및 구성하는 방법](/powershell/azure/overview)을 참조하세요.

이미 Azure PowerShell가 설치되어 있고 버전을 알 수 없는 경우, PowerShell 콘솔에서 `(Get-Module azure -ListAvailable).Version`을 입력합니다.  

### <a id="connect"></a>구독에 연결
Azure PowerShell 세션을 시작하고 다음 명령 사용하여 Azure 계정에 로그인합니다.  

```powershell
Connect-AzAccount
```

팝업 브라우저 창에 Azure 계정 사용자 이름 및 암호를 입력합니다. Azure PowerShell은 이 계정과 연관된 모든 구독을 가져와서 기본적으로 첫 번째 구독을 사용합니다.

구독이 여러 개인 경우 Azure Key Vault을 만드는 데 사용된 특정된 구독을 지정해야 할 수 있습니다. 계정에 대한 구독을 보려면 다음 명령을 입력합니다.

```powershell
Get-AzSubscription
```

그런 다음, 로깅하려는 Azure Cosmos DB 계정과 연결된 구독을 지정하려면 다음 명령을 입력합니다.

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> 사용자 계정에 여러 구독이 연결된 경우 사용하려는 구독을 지정하는 것이 중요합니다.
>
>

Azure PowerShell 구성 방법에 대한 자세한 내용은 [Azure PowerShell 설치 및 구성 방법](/powershell/azure/overview)을 참조하세요.

### <a id="storage"></a>로그에 대한 새 스토리지 계정 만들기
로그에 대해 기존 스토리지 계정을 사용할 수 있지만 이 자습서에서는 Azure Cosmos DB 로그 전용 새 스토리지 계정을 만듭니다. 편의를 위해 스토리지 계정 세부 정보를 **sa**라는 변수에 저장합니다.

추가 관리 용이성을 위해이 자습서에서는 Azure Cosmos 데이터베이스를 포함 하는 것과 동일한 리소스 그룹을 사용 합니다. **ContosoResourceGroup**, **contosocosmosdblogs** 및 **미국 중북부** 매개 변수에 대한 값을 적절히 바꿉니다.

```powershell
$sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> 기존 스토리지 계정을 사용하려는 경우 계정은 Azure Cosmos DB 구독과 동일한 구독을 사용해야 합니다. 또한 계정은 클래식 배포 모델이 아닌 Resource Manager 배포 모델을 사용해야 합니다.
>
>

### <a id="identify"></a>로그를 위한 Azure Cosmos DB 계정 식별
Azure Cosmos DB 계정 이름을 **account**라는 변수로 설정합니다. 여기서 **ResourceName**은 Azure Cosmos DB 계정의 이름입니다.

```powershell
$account = Get-AzResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>로깅 사용
Azure Cosmos DB에 대한 로깅을 사용하도록 설정하려면 새 스토리지 계정, Azure Cosmos DB 계정 및 로깅을 사용하도록 설정할 범주에 대한 변수와 함께 `Set-AzDiagnosticSetting` cmdlet을 사용합니다. 다음 명령을 실행하고 **-Enabled** 플래그를 **$true**로 설정합니다.

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

명령의 출력은 다음 샘플과 유사합니다.

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

명령의 출력을 통해 이제 사용자의 데이터베이스에 대해 로깅이 사용되도록 설정되어 있으며 정보가 스토리지 계정에 저장되는 것을 알 수 있습니다.

선택적으로 오래된 로그를 자동으로 삭제하는 로그에 대한 보존 정책을 설정할 수도 있습니다. 예를 들어, **-RetentionEnabled** 플래그를 **$true**로 지정하여 보존 정책을 설정합니다. **-RetentionInDays** 매개 변수를 **90**으로 설정하여 90일보다 오래된 로그는 자동으로 삭제되도록 합니다.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>로그에 액세스
**DataPlaneRequests** 범주에 대 한 Azure Cosmos DB 로그는 제공 된 저장소 계정의 **DataPlaneRequests** 컨테이너에 저장 됩니다. 

먼저, 컨테이너 이름에 대한 변수를 만듭니다. 이 변수는 연습 과정 전체에서 사용됩니다.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

이 컨테이너에 있는 모든 Blob을 나열하려면 다음을 입력합니다.

```powershell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

명령의 출력은 다음 샘플과 유사합니다.

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

이 출력에서 확인할 수 있듯이 Blob은 명명 규칙 `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`을 따릅니다.

날짜 및 시간 값은 UTC를 사용합니다.

여러 리소스에 대한 로그를 수집하는 데 동일한 스토리지 계정을 사용할 수 있으므로 Blob 이름에 있는 정규화된 리소스 ID를 사용하여 필요한 특정 Blob을 액세스하거나 다운로드할 수 있습니다. 이러한 작업을 수행하기 전에, 모든 Blob을 다운로드하는 방법을 다룹니다.

먼저 Blob을 다운로드할 폴더를 만듭니다. 예:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

그런 다음, 모든 Blob 목록을 가져옵니다.  

```powershell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

`Get-AzStorageBlobContent` 명령을 통해 이 목록을 파이프하여 blob을 대상 폴더에 다운로드합니다.

```powershell
$blobs | Get-AzStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

이 두 번째 명령을 실행할 때 Blob 이름의 **/** 구분 기호는 대상 폴더 아래에 전체 폴더 구조를 만듭니다. 이 폴더 구조는 Blob을 파일로 다운로드하고 저장하는 데 사용됩니다.

선택적으로 Blob을 다운로드하려면 와일드카드를 사용합니다. 예를 들어:

* 여러 데이터베이스가 있고 **CONTOSOCOSMOSDB3**라는 하나의 데이터베이스에 대한 로그를 다운로드하려는 경우 다음 명령을 사용합니다.

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* 리소스 그룹이 여러 개이고 하나의 리소스 그룹에 대한 로그를 다운로드하려는 경우 `-Blob '*/RESOURCEGROUPS/<resource group name>/*'` 명령을 사용합니다.

    ```powershell
    Get-AzStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* 2017년 7월의 모든 로그를 다운로드하려는 경우 `-Blob '*/year=2017/m=07/*'` 명령을 사용합니다.

    ```powershell
    Get-AzStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

다음 명령을 실행할 수도 있습니다.

* 데이터베이스 리소스의 진단 설정 상태를 쿼리하려면 `Get-AzDiagnosticSetting -ResourceId $account.ResourceId` 명령을 사용합니다.
* 데이터베이스 계정 리소스에 대한 **DataPlaneRequests** 범주의 로깅을 사용하지 않도록 설정하려면 `Set-AzDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests` 명령을 사용합니다.


이러한 각 쿼리에서 반환된 Blob은 아래 코드와 같이 텍스트로 저장되거나 JSON Blob으로 서식이 지정됩니다.

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

각 JSON Blob의 데이터에 대해 자세히 알아보려면 [Azure Cosmos DB 로그 해석](#interpret)을 참조하세요.

## <a name="manage-your-logs"></a>로그 관리

진단 로그는 Azure Cosmos DB 작업이 수행된 시간부터 2시간 동안 계정에서 사용 가능합니다. 스토리지 계정에서 로그를 관리하는 것은 사용자의 책임입니다.

* 표준 Azure 액세스 제어 메서드를 사용하여 액세스할 수 있는 사용자를 제한하여 로그를 보호합니다.
* 스토리지 계정에 더 이상 보존하지 않을 로그를 삭제합니다.
* 스토리지 계정에 보관된 데이터 평면 요청의 보존 기간은 **DataPlaneRequests 로그** 설정이 선택된 경우 포털에서 구성됩니다. 해당 설정을 변경하려면 [Azure Portal에서 로깅 켜기](#turn-on-logging-in-the-azure-portal)를 참조하세요.


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-azure-monitor-logs"></a>Azure Monitor 로그에서 로그 보기

진단 로깅을 설정할 때 **Log Analytics 보내기** 옵션을 선택한 경우 컨테이너의 진단 데이터가 2 시간 이내에 Azure Monitor 로그에 전달 됩니다. 로깅을 설정한 후 즉시 Azure Monitor 로그를 살펴보면 어떤 데이터도 표시 되지 않습니다. 2시간을 기다린 후 다시 시도하세요. 

로그를 보기 전에 새 Kusto 쿼리 언어를 사용 하도록 Log Analytics 작업 영역이 업그레이드 되었는지 확인 하세요. 확인 하려면 [Azure Portal](https://portal.azure.com)열고 맨 왼쪽의 **Log Analytics 작업 영역** 을 선택한 후 다음 이미지에 표시 된 것 처럼 작업 영역 이름을 선택 합니다. **Log Analytics 작업 영역** 페이지가 표시됩니다.

![Azure Portal Azure Monitor 로그](./media/logging/azure-portal.png)

>[!NOTE]
>OMS 작업 영역을 이제 Log Analytics 작업 영역이라고 합니다.  

**Log Analytics 작업 영역** 페이지에 다음 메시지가 표시되는 경우 작업 영역이 새 언어를 사용할 수 있도록 업그레이드되지 않은 것입니다. 새 쿼리 언어로 업그레이드하는 방법에 대한 자세한 내용은 [새 로그 검색으로 Azure Log Analytics 작업 영역 업그레이드](../log-analytics/log-analytics-log-search-upgrade.md)를 참조하세요. 

![Azure Monitor 로그 업그레이드 메시지](./media/logging/upgrade-notification.png)

Azure Monitor 로그에서 진단 데이터를 보려면 다음 그림과 같이 왼쪽 메뉴 또는 페이지의 **관리** 영역에서 **로그 검색** 페이지를 엽니다.

![Azure Portal에서 로그 검색 옵션](./media/logging/log-analytics-open-log-search.png)

이제 데이터 수집을 사용하도록 설정한 경우 새 쿼리 언어를 사용하여 다음 로그 검색 예제를 실행하여 최근 10개 로그 `AzureDiagnostics | take 10`을 확인합니다.

![최근 10개 로그에 대한 샘플 로그 검색](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>쿼리

Azure Cosmos 컨테이너를 모니터링 하는 데 도움이 되는 **로그 검색** 상자에 입력할 수 있는 몇 가지 추가 쿼리는 다음과 같습니다. 이러한 쿼리는 [새 언어](../log-analytics/log-analytics-log-search-upgrade.md)에서 작동합니다. 

각 로그 검색에서 반환된 데이터의 의미를 알아보려면 [Azure Cosmos DB 로그 해석](#interpret)을 참조하세요.

* 지정된 기간 동안 Azure Cosmos DB의 모든 진단 로그를 쿼리하려면

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* 가장 최근에 로깅된 10개 이벤트를 쿼리하려면

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* 모든 작업을 쿼리하고 작업 유형별로 그룹화하려면

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* 모든 작업을 쿼리하고 **리소스**별로 그룹화하려면

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* 모든 사용자 작업을 쿼리하고 리소스별로 그룹화하려면

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > 이 명령은 진단 로그가 아닌 활동 로그용입니다.

* 3밀리초보다 오래 소요되는 작업을 쿼리하려면

    ```
    AzureDiagnostics | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* 작업을 실행 중인 에이전트를 쿼리하려면

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* 장기 실행 작업이 수행된 시점을 쿼리하려면

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , duration_s | render timechart
    ```

새 로그 검색 언어를 사용 하는 방법에 대 한 자세한 내용은 [Azure Monitor 로그의 로그 검색 이해](../log-analytics/log-analytics-log-search-new.md)를 참조 하세요. 

## <a id="interpret"></a>로그 해석

Azure Storage 및 Azure Monitor 로그에 저장 된 진단 데이터는 유사한 스키마를 사용 합니다. 

다음 표는 각 로그 항목의 내용에 대해 설명합니다.

| Azure Storage 필드 또는 속성 | Azure Monitor logs 속성 | 설명 |
| --- | --- | --- |
| **time** | **TimeGenerated** | 작업이 발생한 날짜 및 시간(UTC)입니다. |
| **resourceId** | **Resource** | 로그가 사용하도록 설정된 Azure Cosmos DB 계정입니다.|
| **category** | **범주** | Azure Cosmos DB 로그의 경우 **DataPlaneRequests**가 사용 가능한 유일한 값입니다. |
| **operationName** | **OperationName** | 작업의 이름입니다. 이 값은 Create, Update, Read, ReadFeed, Delete, Replace, Execute, SqlQuery, Query, JSQuery, Head, HeadFeed 또는 Upsert 작업 중 하나일 수 있습니다.   |
| **properties** | n/a | 이 필드의 내용은 다음 행에 설명되어 있습니다. |
| **activityId** | **activityId_g** | 기록된 작업의 고유 GUID입니다. |
| **userAgent** | **userAgent_s** | 요청을 수행하는 클라이언트 사용자 에이전트를 지정하는 문자열입니다. 형식은 {사용자 에이전트 이름}/{버전}입니다.|
| **requestResourceType** | **requestResourceType_s** | 액세스한 리소스 유형입니다. 이 값은 Database, Container, Document, Attachment, User, Permission, StoredProcedure, Trigger, UserDefinedFunction 또는 Offer 같은 리소스 유형 중 하나일 수 있습니다. |
| **statusCode** | **statusCode_s** | 작업의 응답 상태입니다. |
| **requestResourceId** | **ResourceId** | 요청에 관련된 리소스 ID입니다. 값은 수행된 작업에 따라 databaseRid, collectionRid 또는 documentRid를 가리킬 수 있습니다.|
| **clientIpAddress** | **clientIpAddress_s** | 클라이언트의 IP 주소입니다. |
| **requestCharge** | **requestCharge_s** | 작업에서 사용된 RU 수입니다. |
| **collectionRid** | **collectionId_s** | 컬렉션의 고유 ID입니다.|
| **duration** | **duration_s** | 작업의 기간 (밀리초)입니다. |
| **requestLength** | **requestLength_s** | 요청 길이(바이트)입니다. |
| **responseLength** | **responseLength_s** | 길이 (바이트)에서 응답입니다.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | 이 값은 인증에 [리소스 토큰](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens)이 사용될 경우 비어 있지 않습니다. 값은 사용자의 리소스 ID를 가리킵니다. |

## <a name="next-steps"></a>다음 단계

- 로깅을 사용하도록 설정하는 방법뿐 아니라 여러 Azure 서비스에서 지원하는 메트릭 및 로그 범주를 이해하려면 [Microsoft Azure의 메트릭 개요](../monitoring-and-diagnostics/monitoring-overview-metrics.md) 및 [Azure 진단 로그 개요](../azure-monitor/platform/diagnostic-logs-overview.md) 문서를 읽어보세요.
- Event Hubs에 대한 자세한 내용은 다음 문서를 참조하세요.
   - [Azure Event Hubs 정의](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Event Hubs 시작](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- [Azure Storage에서 메트릭 및 진단 로그 다운로드](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs)를 읽어보세요.
- [Azure Monitor 로그의 로그 검색 이해를](../log-analytics/log-analytics-log-search-new.md)참조 하세요.
