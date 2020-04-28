---
title: Azure Service Fabric의 주문형 백업
description: Service Fabric의 백업 및 복원 기능을 사용하여 필요할 때 애플리케이션 데이터를 백업할 수 있습니다.
author: aagup
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: d5eada62bec49fe771373671e9438d2786d6b165
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75458426"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Azure Service Fabric의 주문형 백업

Reliable Stateful 서비스 및 Reliable Actors의 데이터를 백업하여 재해 또는 데이터 손실 시나리오를 해결할 수 있습니다.

Azure Service Fabric은 [정기적 데이터 백업](service-fabric-backuprestoreservice-quickstart-azurecluster.md) 및 필요 시 데이터를 백업하는 기능을 포함합니다. 주문형 백업은 기본 서비스 또는 해당 환경에서 계획 된 변경 사항으로 인해 _데이터 손실_/_데이터 손상을_ 방지 하기 때문에 유용 합니다.

주문형 백업 기능은 서비스 또는 서비스 환경 작업을 수동으로 트리거하기 전에 서비스 상태를 캡처하는 데 유용합니다. 예를 들어 서비스를 업그레이드하거나 다운그레이드할 때 서비스 바이너리를 변경하는 경우가 있습니다. 이러한 경우 주문형 백업은 애플리케이션 코드 버그로 인한 손상에 대해 데이터를 보호하는 데 도움이 될 수 있습니다.
## <a name="prerequisites"></a>사전 요구 사항

- 구성 호출을 위해 ServiceFabric 모듈 [미리 보기]를 설치 합니다.

```powershell
    Install-Module -Name Microsoft.ServiceFabric.Powershell.Http -AllowPrerelease
```

- ServiceFabric 모듈을 사용 하 여 구성 요청 `Connect-SFCluster` 을 수행 하기 전에 명령을 사용 하 여 클러스터를 연결 했는지 확인 합니다.

```powershell

    Connect-SFCluster -ConnectionEndpoint 'https://mysfcluster.southcentralus.cloudapp.azure.com:19080'   -X509Credential -FindType FindByThumbprint -FindValue '1b7ebe2174649c45474a4819dafae956712c31d3' -StoreLocation 'CurrentUser' -StoreName 'My' -ServerCertThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'  

```


## <a name="triggering-on-demand-backup"></a>주문형 백업 트리거

주문형 백업을 사용하려면 백업 파일을 업로드하기 위한 스토리지 세부 정보가 필요합니다. 정기적 백업 정책이나 주문형 백업 요청에서 주문형 백업 위치를 지정합니다.

### <a name="on-demand-backup-to-storage-specified-by-a-periodic-backup-policy"></a>정기적 백업 정책에 지정된 스토리지로 주문형 백업

스토리지로 추가 주문형 백업을 위해 Reliable Stateful 서비스 또는 Reliable Actor의 파티션을 사용하도록 정기적인 백업 정책을 구성할 수 있습니다.

다음 사례는 [Reliable Stateful 서비스 및 Reliable Actors에 대해 정기적 백업 사용](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors) 시나리오를 계속 설명한 것입니다. 이 경우 파티션을 사용하도록 백업 정책을 설정하고 백업은 Azure Storage에 설정된 빈도로 발생합니다.

#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell을 사용 하 여 ServiceFabric 모듈을 사용 합니다.

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' 

```

#### <a name="rest-call-using-powershell"></a>Powershell을 사용 하 여 Rest 호출

파티션 ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22`에 대한 주문형 백업 트리거를 설정하려면 [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API를 사용합니다.

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

[주문형 백업 진행률](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress)에 대한 추적을 사용하도록 설정하려면 [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API를 사용합니다.

### <a name="on-demand-backup-to-specified-storage"></a>지정된 스토리지에 주문형 백업

Reliable Stateful 서비스 또는 Reliable Actor의 파티션에 대한 주문형 백업을 요청할 수 있습니다. 주문형 백업 요청의 일부로 스토리지 정보를 제공합니다.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell을 사용 하 여 ServiceFabric 모듈을 사용 합니다.

```powershell

Backup-SFPartition -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22' -AzureBlobStore -ConnectionString  'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' -ContainerName 'backup-container'

```

#### <a name="rest-call-using-powershell"></a>Powershell을 사용 하 여 Rest 호출

파티션 ID `974bd92a-b395-4631-8a7f-53bd4ae9cf22`에 대한 주문형 백업 트리거를 설정하려면 [BackupPartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) API를 사용합니다. 다음 Azure Storage 정보를 포함합니다.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

[주문형 백업 진행률](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress)에 대한 추적을 설정하려면 [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress) API를 사용할 수 있습니다.

### <a name="using-service-fabric-explorer"></a>Service Fabric Explorer 사용
Service Fabric Explorer 설정에서 고급 모드를 사용 하도록 설정 했는지 확인 합니다.
1. 원하는 파티션을 선택 하 고 작업을 클릭 합니다. 
2. 파티션 백업 트리거를 선택 하 고 Azure에 대 한 정보를 입력 합니다.

    ![파티션 백업 트리거][0]

    또는 파일 공유:

    ![파티션 백업 파일 공유 트리거][1]

## <a name="tracking-on-demand-backup-progress"></a>주문형 백업 진행 상태 추적

Reliable Stateful 서비스 또는 Reliable Actor의 파티션은 한 번에 하나의 주문형 백업 요청만 수락합니다. 현재 주문형 백업 요청이 완료된 후에만 또 다른 요청을 수락할 수 있습니다.

여러 파티션에서 주문형 백업 요청을 동시에 트리거할 수 있습니다.


#### <a name="powershell-using-microsoftservicefabricpowershellhttp-module"></a>Powershell을 사용 하 여 ServiceFabric 모듈을 사용 합니다.

```powershell

Get-SFPartitionBackupProgress -PartitionId '974bd92a-b395-4631-8a7f-53bd4ae9cf22'

```
#### <a name="rest-call-using-powershell"></a>Powershell을 사용 하 여 Rest 호출

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

주문형 백업 요청은 다음 상태 중 하나일 수 있습니다.

- **수락**됨: 파티션에서 백업이 시작 되어 진행 중입니다.
  ```
  BackupState             : Accepted
  TimeStampUtc            : 0001-01-01T00:00:00Z
  BackupId                : 00000000-0000-0000-0000-000000000000
  BackupLocation          :
  EpochOfLastBackupRecord :
  LsnOfLastBackupRecord   : 0
  FailureError            :
  ```
- **성공**, **실패**또는 **시간 제한**: 요청 된 주문형 백업은 다음과 같은 상태에서 완료할 수 있습니다.
  - **성공**: _성공_ 백업 상태는 파티션 상태가 성공적으로 백업 되었음을 나타냅니다. 응답에는 시간(UTC)과 함께 파티션의 _BackupEpoch_ 및 _BackupLSN_이 제공됩니다.
    ```
    BackupState             : Success
    TimeStampUtc            : 2018-11-21T20:00:01Z
    BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
    BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
    EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
    LsnOfLastBackupRecord   : 36
    FailureError            :
    ```
  - **실패**: _실패_ 백업 상태는 파티션 상태를 백업 하는 동안 오류가 발생 했음을 나타냅니다. 응답에 실패 이유가 설명되어 있습니다.
    ```
    BackupState             : Failure
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
    ```
  - **시간**제한: _시간 제한_ 백업 상태는 지정 된 시간 내에 파티션 상태 백업을 만들 수 없음을 나타냅니다. 시간 제한은 기본적으로 10분입니다. 이 시나리오에서는 새로운 주문형 백업 요청의 [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) 값을 더 높게 설정하여 시작합니다.
    ```
    BackupState             : Timeout
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          :
    EpochOfLastBackupRecord :
    LsnOfLastBackupRecord   : 0
    FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
    ```

## <a name="next-steps"></a>다음 단계

- [정기적 백업 구성 이해](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [BackupRestore REST API 참조](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

[0]: ./media/service-fabric-backuprestoreservice/trigger-partition-backup.png
[1]: ./media/service-fabric-backuprestoreservice/trigger-backup-fileshare.png