---
title: Azure Storage Blob에 대한 일시 삭제 | Microsoft Docs
description: Azure Storage는 이제 애플리케이션 또는 다른 스토리지 계정 사용자에 의해 잘못 수정되거나 삭제될 때 데이터를 보다 쉽게 복구할 수 있도록 Blob 개체에 대한 일시 삭제를 제공합니다.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 360930b708d6358692de2af7325701b73d5cf9c9
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79370562"
---
# <a name="soft-delete-for-azure-storage-blobs"></a>Azure Storage Blob에 대한 일시 삭제

Azure Storage는 이제 애플리케이션 또는 다른 스토리지 계정 사용자에 의해 잘못 수정되거나 삭제될 때 데이터를 보다 쉽게 복구할 수 있도록 Blob 개체에 대한 일시 삭제를 제공합니다.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="how-soft-delete-works"></a>일시 삭제 작동 방식

일시 삭제를 사용 하도록 설정 하면 blob 또는 blob 스냅숏이 삭제 될 때 데이터를 저장 하 고 복구할 수 있습니다. 이 보호는 덮어쓰기의 결과로 삭제되는 Blob 데이터로 확장합니다.

데이터가 삭제되면 영구적으로 삭제되는 대신 일시 삭제된 상태로 전환됩니다. 일시 삭제가 있고 데이터를 덮어쓰는 경우 일시 삭제된 스냅샷은 덮어쓴 데이터의 상태를 저장하기 위해 생성됩니다. 일시 삭제된 개체는 명시적으로 나열되지 않는 한 표시되지 않습니다. 일시 삭제된 데이터가 영구적으로 만료되기 전에 복구 가능한 기간을 구성할 수 있습니다.

일시 삭제는 이전 버전과 호환 되므로 응용 프로그램을 변경할 필요가 없으므로이 기능의 보호를 활용할 수 있습니다. 그러나 [데이터 복구](#recovery)는 새로운 **삭제 취소 Blob** API를 도입합니다.

### <a name="configuration-settings"></a>구성 설정

새 계정을 만드는 경우 일시 삭제는 기본적으로 해제되어 있습니다. 일시 삭제는 기존 스토리지 계정에 대해서도 기본적으로 해제되어 있습니다. 스토리지 계정의 수명 동안 언제든지 기능의 설정 및 해제를 전환할 수 있습니다.

기능이 이전에 설정되어 있던 경우 일시 삭제된 데이터가 저장되었다고 가정하면, 기능이 해제된 경우 일시 삭제된 데이터에 계속해서 액세스하고 이를 복구할 수 있습니다. 일시 삭제를 설정하면 보존 기간도 구성해야 합니다.

보존 기간은 일시 삭제된 데이터가 저장되고 복구에 사용할 수 있는 기간을 나타냅니다. 명시적으로 삭제된 Blob 및 Blob 스냅샷의 경우 데이터가 삭제되면 보존 기간 시계가 시작됩니다. 데이터가 덮어쓰여지는 경우 일시 삭제 기능에서 생성되는 일시 삭제된 스냅샷의 경우 스냅샷이 생성될 때 시계가 시작됩니다. 현재 1일에서 365일 동안 일시 삭제된 데이터를 유지할 수 있습니다.

언제든지 일시 삭제 보존 기간을 변경할 수 있습니다. 업데이트된 보존 기간은 새로 삭제된 데이터에만 적용됩니다. 이전에 삭제된 데이터는 해당 데이터가 삭제될 때 구성된 보존 기간에 따라 만료됩니다. 일시 삭제된 개체를 삭제하는 시도는 만료 시간에 영향을 주지 않습니다.

### <a name="saving-deleted-data"></a>삭제된 데이터 저장

일시 삭제는 Blob 또는 Blob 스냅샷이 삭제되거나 덮어쓰여지는 대부분의 경우에서 데이터를 유지합니다.

Blob 넣기, **블록**배치, **블록 목록 배치**또는 **blob 복사** **를 사용 하**여 blob을 덮어쓰는 경우 쓰기 작업 전에 blob의 상태 스냅숏이 자동으로 생성 됩니다. 이 스냅샷은 일시 삭제된 스냅샷이며 일시 삭제된 개체가 명시적으로 나열된 경우가 아니면 표시되지 않습니다. 일시 삭제된 개체를 나열하는 방법을 알아보려면 [복구](#recovery) 섹션을 참조하세요.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*일시 삭제 된 데이터는 회색 이며 활성 데이터는 파란색입니다. 이전 데이터 아래에 더 최근에 작성 된 데이터가 표시 됩니다. B0을 B1로 덮어쓰면 일시 삭제 된 B0 스냅숏이 생성 됩니다. B2를 B2로 덮어쓰면 B1의 일시 삭제 된 스냅숏이 생성 됩니다.*

> [!NOTE]  
> 일시 삭제는 대상 Blob의 계정이 설정된 경우, 복사 작업에 대한 덮어쓰기 보호만 제공합니다.

> [!NOTE]  
> 일시 삭제는 보관 계층의 Blob에 대한 덮어쓰기 보호를 제공하지 않습니다. 보관 계층의 Blob이 계층에서 새 Blob으로 덮어쓰여지는 경우 덮어쓰여진 Blob이 영구적으로 만료됩니다.

**Blob 삭제**가 스냅샷에서 호출되는 경우 해당 스냅샷은 일시 삭제됨으로 표시됩니다. 새 스냅샷이 생성되지 않습니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*일시 삭제 된 데이터는 회색 이며 활성 데이터는 파란색입니다. 이전 데이터 아래에 더 최근에 작성 된 데이터가 표시 됩니다. **Snapshot Blob** 이 호출 되 면 B0은 스냅숏이 되 고 B1은 blob의 활성 상태입니다. B0 스냅숏이 삭제 되 면 일시 삭제로 표시 됩니다.*

기본 Blob에서 **Blob 삭제**가 호출되는 경우(자체 스냅샷이 아닌 모든 Blob) 해당 Blob은 일시 삭제됨으로 표시됩니다. 이전 동작과 일치하는 활성 스냅샷이 있는 Blob에서 **Blob 삭제**를 호출하면 오류를 반환합니다. 일시 삭제된 스냅샷이 있는 Blob에서 **Blob 삭제**를 호출하면 오류를 반환하지 않습니다. 일시 삭제가 설정되면 단일 작업에서 Blob 및 모든 해당 스냅샷을 여전히 삭제할 수 있습니다. 이렇게 하면 기본 Blob 및 스냅샷을 일시 삭제됨으로 표시합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*일시 삭제 된 데이터는 회색 이며 활성 데이터는 파란색입니다. 이전 데이터 아래에 더 최근에 작성 된 데이터가 표시 됩니다. 여기서 **Delete Blob** 호출은 B2와 연결 된 모든 스냅숏을 삭제 합니다. 활성 blob, B2 및 연결 된 모든 스냅숏이 일시 삭제로 표시 됩니다.*

> [!NOTE]  
> 일시 삭제된 Blob이 덮어쓰여지는 경우 쓰기 작업 전에 Blob 상태의 일시 삭제된 스냅샷이 자동으로 생성됩니다. 새 Blob은 덮어쓰여진 Blob의 계층을 상속합니다.

일시 삭제는 컨테이너 또는 계정이 삭제되고, Blob 메타데이터 및 Blob 속성이 덮어쓰여지는 경우에 데이터를 저장하지 않습니다. 잘못된 삭제로부터 스토리지 계정을 보호하기 위해 Azure Resource Manager를 사용하여 잠금을 구성할 수 있습니다. 자세히 알아보려면 Azure Resource Manager 문서 [예기치 않은 변경을 방지하기 위해 리소스 잠그기](../../azure-resource-manager/management/lock-resources.md)를 참조하세요.

다음 표는 일시 삭제가 설정된 경우 예상되는 동작을 자세히 설명합니다.

| REST API 작업 | 리소스 유형 | Description | 동작 변경 |
|--------------------|---------------|-------------|--------------------|
| [Delete](/rest/api/storagerp/StorageAccounts/Delete) | 계정 | 포함하는 모든 컨테이너 및 Blob을 포함하여 스토리지 계정을 삭제합니다.                           | 변경 없음 삭제된 계정의 컨테이너 및 Blob은 복구할 수 없습니다. |
| [컨테이너 삭제](/rest/api/storageservices/delete-container) | 컨테이너 | 포함하는 모든 Blob을 포함하여 컨테이너를 삭제합니다. | 변경 없음 삭제된 컨테이너의 Blob은 복구할 수 없습니다. |
| [Blob 배치](/rest/api/storageservices/put-blob) | 블록, 추가 및 페이지 Blob | 새 Blob을 만들거나 컨테이너 내 기존 Blob 교체 | 기존 Blob을 교체하는 데 사용되는 경우, 호출 전에 Blob의 상태 스냅샷이 자동으로 생성됩니다. 이는 동일한 유형의 blob (블록, 추가 또는 페이지)로 대체 되는 경우에만 이전에 일시 삭제 된 blob에도 적용 됩니다. 다른 형식의 Blob으로 교체되는 경우 기존의 모든 일시 삭제된 데이터는 영구적으로 만료됩니다. |
| [Blob 삭제](/rest/api/storageservices/delete-blob) | 블록, 추가 및 페이지 Blob | 삭제를 위한 Blob 또는 Blob 스냅샷을 표시합니다. 가비지 수집 중 Blob 또는 스냅샷은 나중에 삭제됩니다. | Blob 스냅샷을 삭제하는 데 사용되는 경우 해당 스냅샷은 일시 삭제됨으로 표시됩니다. Blob을 삭제하는 데 사용되는 경우 해당 스냅숏은 일시 삭제됨으로 표시됩니다. |
| [Blob 복사](/rest/api/storageservices/copy-blob) | 블록, 추가 및 페이지 Blob | 동일한 스토리지 계정 또는 다른 스토리지 계정에서 대상 Blob에 원본 Blob을 복사합니다. | 기존 Blob을 교체하는 데 사용되는 경우, 호출 전에 Blob의 상태 스냅샷이 자동으로 생성됩니다. 이는 동일한 유형의 blob (블록, 추가 또는 페이지)로 대체 되는 경우에만 이전에 일시 삭제 된 blob에도 적용 됩니다. 다른 형식의 Blob으로 교체되는 경우 기존의 모든 일시 삭제된 데이터는 영구적으로 만료됩니다. |
| [블록 배치](/rest/api/storageservices/put-block) | 블록 Blob | 블록 Blob의 일부로 커밋될 새 블록을 만듭니다. | 활성 상태인 blob에 블록을 커밋하는 데 사용 되는 경우 변경 내용이 없습니다. 일시 삭제된 Blob에 블록을 커밋하는 데 사용되는 경우 새 Blob이 만들어지고 일시 삭제된 Blob의 상태를 캡처하도록 스냅샷이 자동으로 생성됩니다. |
| [블록 목록 배치](/rest/api/storageservices/put-block-list) | 블록 Blob | 블록 Blob을 구성하는 블록 ID의 집합을 지정하여 Blob을 커밋합니다. | 기존 Blob을 교체하는 데 사용되는 경우, 호출 전에 Blob의 상태 스냅샷이 자동으로 생성됩니다. 블록 Blob인 경우에만 이전에 일시 삭제된 Blob에도 적용됩니다. 다른 형식의 Blob으로 교체되는 경우 기존의 모든 일시 삭제된 데이터는 영구적으로 만료됩니다. |
| [페이지 배치](/rest/api/storageservices/put-page) | 페이지 Blob | 페이지 Blob에 페이지의 범위를 작성합니다. | 변경 없음 이 작업을 사용하여 덮어쓰여지거나 제거된 페이지 Blob 데이터는 저장되지 않고 복구할 수 없습니다. |
| [추가 블록](/rest/api/storageservices/append-block) | 추가 Blob | 추가 Blob의 끝에 데이터 블록을 작성합니다. | 변경 없음 |
| [Blob 속성 설정](/rest/api/storageservices/set-blob-properties) | 블록, 추가 및 페이지 Blob | Blob에 대해 정의된 시스템 속성에 대한 값을 설정합니다. | 변경 없음 덮어쓰여진 Blob 속성은 복구할 수 없습니다. |
| [Blob 메타데이터 설정](/rest/api/storageservices/set-blob-metadata) | 블록, 추가 및 페이지 Blob | 하나 이상의 이름 값 쌍으로 지정된 Blob에 대한 사용자 정의 메타데이터를 설정합니다. | 변경 없음 덮어쓰여진 Blob 메타데이터는 복구할 수 없습니다. |

페이지 Blob의 범위를 덮어쓰거나 지우기 위해 "페이지 배치"를 호출하면 스냅샷이 자동으로 생성되지 않습니다. 가상 머신 디스크는 페이지 Blob에서 지원되며 **페이지 배치**를 사용하여 데이터를 쓸 수 있습니다.

### <a name="recovery"></a>복구

일시 삭제 된 기본 blob에서 [blob 삭제 취소](/rest/api/storageservices/undelete-blob) 작업을 호출 하면 해당 작업 및 연결 된 모든 일시 삭제 된 스냅숏이 활성으로 복원 됩니다. 활성 기본 blob에 대 한 `Undelete Blob` 작업을 호출 하면 연결 된 모든 일시 삭제 된 스냅숏이 활성으로 복원 됩니다. 스냅샷이 활성으로 복원되는 경우 사용자 생성 스냅샷처럼 보입니다. 이는 기본 Blob을 덮어쓰지 않습니다.

Blob을 일시 삭제 된 특정 스냅숏으로 복원 하려면 기본 blob에서 `Undelete Blob`를 호출 하면 됩니다. 그런 다음, 현재 활성 Blob을 통해 스냅샷을 복사할 수 있습니다. 새 Blob에 스냅샷을 복사할 수도 있습니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*일시 삭제 된 데이터는 회색 이며 활성 데이터는 파란색입니다. 이전 데이터 아래에 더 최근에 작성 된 데이터가 표시 됩니다. 여기서는 blob B에서 **삭제 취소 blob** 을 호출 하 여 기본 Blob, B1 및 연결 된 모든 스냅숏을 복원 합니다. 여기에서는 B0만 활성으로 복원 합니다. 두 번째 단계에서는 B0이 기본 blob을 통해 복사 됩니다. 이 복사 작업은 B1의 일시 삭제 된 스냅숏을 생성 합니다.*

일시 삭제된 Blob 및 Blob 스냅샷을 보려면 **Blob 나열**에 삭제된 데이터를 포함하도록 선택할 수 있습니다. 일시 삭제된 기본 Blob만을 보거나 일시 삭제된 Blob 스냅샷도 포함하도록 선택할 수 있습니다. 모든 일시 삭제된 데이터의 경우 데이터가 삭제되었던 시간 및 데이터가 영구적으로 만료되기 전까지의 일 수를 볼 수 있습니다.

### <a name="example"></a>예제

다음은 일시 삭제가 설정 된 경우 *HelloWorld* 라는 blob을 업로드, 덮어쓰기, 스냅숏, 삭제 및 복원 하는 .net 스크립트의 콘솔 출력입니다.

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

이 출력을 생성한 애플리케이션에 대한 포인터는 [다음 단계](#next-steps) 섹션을 참조하세요.

## <a name="pricing-and-billing"></a>가격 책정 및 대금 청구

일시 삭제된 모든 데이터는 활성 데이터와 동일한 요율로 요금이 청구됩니다. 구성된 보존 기간 후 영구적으로 삭제되는 데이터에 대한 비용은 청구되지 않습니다. 스냅샷 및 요금이 발생하는 방법에 대해 자세히 알아보려면 [스냅샷 요금 청구 방법 이해](storage-blob-snapshots.md)를 참조하세요.

스냅샷의 자동 생성과 관련된 트랜잭션에 대해 요금이 청구되지 않습니다. 쓰기 작업에 대 한 속도에서 **삭제 취소 Blob** 트랜잭션에 대 한 요금이 청구 됩니다.

일반적인 Azure Blob Storage에 대한 가격의 자세한 내용은 [Azure Blob Storage 가격 책정 페이지](https://azure.microsoft.com/pricing/details/storage/blobs/)를 참조하세요.

일시 삭제를 처음 켤 때 기능이 요금에 영향을 주는 방법을 더 잘 이해할 수 있도록 작은 보존 기간을 사용하는 것이 좋습니다.

## <a name="get-started"></a>시작하기

다음 단계는 일시 삭제를 시작 하는 방법을 보여 줍니다.

# <a name="portal"></a>[포털](#tab/azure-portal)

Azure Portal를 사용 하 여 저장소 계정의 blob에 대해 일시 삭제를 사용 하도록 설정 합니다.

1. [Azure Portal](https://portal.azure.com/)에서 저장소 계정을 선택 합니다. 

2. **Blob Service**에서 **데이터 보호** 옵션으로 이동 합니다.

3. **Blob 일시 삭제** 아래에서 **사용** 을 클릭 합니다.

4. **보존 정책** 에서 *보존할* 일 수를 입력 합니다.

5. **저장** 단추를 선택 하 여 데이터 보호 설정 확인

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

일시 삭제된 Blob을 보려면 **삭제된 Blob 표시** 확인란을 선택합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

지정된 Blob에 대한 일시 삭제된 스냅샷을 보려면 Blob을 선택한 다음, **스냅샷 보기**를 클릭합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

**삭제된 스냅샷 표시** 확인란이 선택되었는지 확인합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

일시 삭제된 Blob 또는 스냅샷을 클릭할 때 새 Blob 속성을 확인합니다. 개체가 삭제된 시점과 Blob 또는 Blob 스냅샷이 영구적으로 만료될 때까지 남은 일 수를 나타냅니다. 일시 삭제된 개체가 스냅샷이 아닌 경우 삭제를 취소하는 옵션을 갖게 됩니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Blob 삭제 취소는 연결된 모든 스냅샷도 삭제 취소합니다. 활성 Blob에 대해 일시 삭제된 스냅샷을 삭제 취소하려면 Blob을 클릭하고 **모든 스냅샷 삭제 취소**를 선택합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Blob의 스냅샷을 삭제 취소하면 **승격**을 클릭하여 루트 Blob에 대한 스냅샷을 복사할 수 있으므로 Blob을 스냅샷으로 복원합니다.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

일시 삭제를 활성화하려면 Blob 클라이언트의 서비스 속성을 업데이트합니다. 다음 예제에서는 구독에서 계정의 하위 집합에 대한 일시 삭제를 활성화합니다.

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```
다음 명령을 사용하여 일시 삭제가 켜졌는지 확인할 수 있습니다.

```powershell
$MatchingAccounts | Get-AzStorageServiceProperty -ServiceType Blob
```

실수로 삭제된 Blob을 복구하려면 해당 Blob에서 삭제 취소를 호출합니다. 활성 및 일시 삭제된 Blob 모두에서 **삭제 취소 Blob**을 호출하면 활성으로 연결된 모든 일시 삭제된 스냅샷을 복원합니다. 다음 예제에서는 컨테이너의 모든 일시 삭제된 Blob 및 활성 Blob에서 삭제 취소를 호출합니다.

```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
현재 일시 삭제 보존 정책을 찾으려면 다음 명령을 사용합니다.

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

# <a name="cli"></a>[CLI](#tab/azure-CLI)

일시 삭제를 활성화하려면 Blob 클라이언트의 서비스 속성을 업데이트합니다.

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

일시 삭제를 켰는지 확인하려면 다음 명령을 사용합니다. 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

# <a name="python"></a>[Python](#tab/python)

일시 삭제를 활성화하려면 Blob 클라이언트의 서비스 속성을 업데이트합니다.

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(
    account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(
    delete_retention_policy=DeleteRetentionPolicy(enabled=True, days=7))
```

# <a name="net"></a>[.NET](#tab/net)

일시 삭제를 활성화하려면 Blob 클라이언트의 서비스 속성을 업데이트합니다.

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

실수로 삭제된 Blob을 복구하려면 해당 Blob에서 삭제 취소를 호출합니다. 활성 및 일시 삭제된 Blob 모두에서 **삭제 취소 Blob**을 호출하면 활성으로 연결된 모든 일시 삭제된 스냅샷을 복원합니다. 다음 예제에서는 컨테이너의 모든 일시 삭제된 Blob 및 활성 Blob에서 삭제 취소를 호출합니다.

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

특정 Blob 버전을 복구하려면 먼저 Blob에서 삭제 취소를 호출한 다음, Blob을 통해 원하는 스냅샷을 복사합니다. 다음 예제에서는 블록 Blob을 가장 최근에 생성된 스냅샷으로 복구합니다.

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

---

## <a name="special-considerations"></a>특별 고려 사항

응용 프로그램 또는 다른 저장소 계정 사용자가 실수로 데이터를 수정 하거나 삭제할 가능성이 있는 경우 일시 삭제를 켜는 것이 좋습니다. 자주 덮어쓴 데이터에 대해 일시 삭제를 사용 하도록 설정 하면 blob을 나열할 때 저장소 용량 요금이 증가 하 고 대기 시간이 길어질 수 있습니다. 일시 삭제를 사용 하지 않도록 설정 된 별도의 저장소 계정에 자주 덮어쓴 데이터를 저장 하 여 이러한 추가 비용 및 대기 시간을 줄일 수 있습니다. 

## <a name="faq"></a>FAQ

### <a name="for-which-storage-services-can-i-use-soft-delete"></a>일시 삭제를 사용할 수 있는 저장소 서비스는 무엇 인가요?

현재 일시 삭제는 Blob(개체) 스토리지에 대해서만 사용 가능합니다.

### <a name="is-soft-delete-available-for-all-storage-account-types"></a>일시 삭제는 모든 저장소 계정 유형에 사용할 수 있나요?

예, 일시 삭제는 Blob storage 계정 뿐만 아니라 범용 (GPv1 및 GPv2) 저장소 계정의 blob에 대해서만 사용할 수 있습니다. Standard 및 premium 계정 유형이 모두 지원 됩니다. 일시 삭제는 관리 되지 않는 디스크에 사용할 수 있으며,이는 페이지 blob입니다. 일시 삭제는 관리 디스크에 사용할 수 없습니다.

### <a name="is-soft-delete-available-for-all-storage-tiers"></a>일시 삭제는 모든 저장소 계층에 사용할 수 있나요?

예, 일시 삭제는 핫, 쿨 및 보관을 비롯 한 모든 저장소 계층에 사용할 수 있습니다. 그러나 일시 삭제는 보관 계층의 Blob에 대한 덮어쓰기 보호를 제공하지 않습니다.

### <a name="can-i-use-the-set-blob-tier-api-to-tier-blobs-with-soft-deleted-snapshots"></a>Blob 계층 API 집합을 사용 하 여 일시 삭제 된 스냅숏이 있는 blob 계층을 만들 수 있나요?

예. 일시 삭제된 스냅샷은 원본 계층에 유지되지만 기본 Blob은 새 계층으로 이동합니다. 

### <a name="premium-storage-accounts-have-a-per-blob-snapshot-limit-of-100-do-soft-deleted-snapshots-count-toward-this-limit"></a>Premium storage 계정에는 blob 당 스냅숏 제한인 100이 있습니다. 일시 삭제 된 스냅숏 수가이 한도에 도달 하나요?

아니요, 일시 삭제된 스냅샷은 이 제한에 포함되지 않습니다.

### <a name="can-i-turn-on-soft-delete-for-existing-storage-accounts"></a>기존 저장소 계정에 대해 일시 삭제를 켤 수 있나요?

예, 일시 삭제는 기존 및 새 스토리지 계정 모두에 대해 구성할 수 있습니다.

### <a name="if-i-delete-an-entire-account-or-container-with-soft-delete-turned-on-will-all-associated-blobs-be-saved"></a>일시 삭제가 설정 된 상태에서 전체 계정 또는 컨테이너를 삭제 하면 연결 된 모든 blob이 저장 됩니까?

아니요, 전체 계정 또는 컨테이너를 삭제하면 연결된 모든 Blob은 영구적으로 삭제됩니다. 저장소 계정이 실수로 삭제 되지 않도록 보호 하는 방법에 대 한 자세한 내용은 [리소스를 잠가 예기치 않은 변경 방지](../../azure-resource-manager/management/lock-resources.md)를 참조 하세요.

### <a name="can-i-view-capacity-metrics-for-deleted-data"></a>삭제 된 데이터에 대 한 용량 메트릭을 볼 수 있나요?

일시 삭제된 데이터는 총 스토리지 계정 용량의 일부분으로 포함됩니다. 저장소 용량 추적 및 모니터링에 대 한 자세한 내용은 [스토리지 분석](../common/storage-analytics.md)를 참조 하세요.

### <a name="if-i-turn-off-soft-delete-will-i-still-be-able-to-access-soft-deleted-data"></a>일시 삭제를 해제 한 경우에도 일시 삭제 된 데이터에 계속 액세스할 수 있나요?

예, 일시 삭제가 해제된 경우 만료되지 않은 일시 삭제된 데이터에 여전히 액세스하고 이를 복구할 수 있습니다.

### <a name="can-i-read-and-copy-out-soft-deleted-snapshots-of-my-blob"></a>Blob의 일시 삭제 된 스냅숏을 읽고 복사할 수 있나요?  

예, 하지만 Blob에서 먼저 삭제 취소를 호출해야 합니다.

### <a name="is-soft-delete-available-for-all-blob-types"></a>일시 삭제는 모든 blob 유형에 사용할 수 있나요?

예, 일시 삭제는 블록 blob, 추가 blob 및 페이지 blob에 사용할 수 있습니다.

### <a name="is-soft-delete-available-for-virtual-machine-disks"></a>가상 컴퓨터 디스크에 대해 일시 삭제를 사용할 수 있나요?  

일시 삭제는 프리미엄 및 표준 관리 되지 않는 디스크 모두에 사용할 수 있습니다. 이러한 디스크는 내부적으로 페이지 blob입니다. 일시 삭제는 **Blob 삭제**, **Blob 배치**, **블록 목록**배치, **블록 배치** 및 **blob 복사** 작업을 통해 삭제 된 데이터를 복구 하는 데만 도움이 됩니다. **페이지 배치**에 대한 호출로 덮어쓰여진 데이터는 복구할 수 없습니다.

Azure 가상 머신은 **Put 페이지**호출을 사용 하 여 관리 되지 않는 디스크에 기록 되므로, 일시 삭제를 사용 하 여 azure VM에서 관리 되지 않는 디스크에 대 한 쓰기를 실행 취소 하는 것은 지원 되지 않습니다.

### <a name="do-i-need-to-change-my-existing-applications-to-use-soft-delete"></a>일시 삭제를 사용 하도록 기존 응용 프로그램을 변경 해야 하나요?

사용하는 API 버전에 관계 없이 일시 삭제를 활용할 수 있습니다. 그러나 일시 삭제된 Blob 및 Blob 스냅샷을 나열하고 복구하려면 [스토리지 서비스 REST API](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services)의 2017-07-29 버전 이상을 사용해야 합니다. Microsoft에서 항상 최신 버전의 Azure Storage API를 사용 하는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

* [.NET 샘플 코드](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)
* [BLOB 서비스 REST API](/rest/api/storageservices/blob-service-rest-api)
* [Azure Storage 복제](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [RA-GRS를 사용하여 항상 사용 가능한 애플리케이션 설계](../common/storage-designing-ha-apps-with-ragrs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure Storage에서 재해 복구 및 스토리지 계정 장애 조치(Failover)(미리 보기)](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
