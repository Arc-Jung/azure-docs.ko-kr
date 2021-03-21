---
title: Azure HPC 캐시 저장소 대상 업데이트
description: Azure HPC 캐시 저장소 대상을 편집 하는 방법
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 03/10/2021
ms.author: v-erkel
ms.openlocfilehash: 78010ef2d93b23a12fc7f3e988a536b4993b4dd4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103471828"
---
# <a name="edit-storage-targets"></a>스토리지 대상 편집

Azure Portal 또는 Azure CLI를 사용 하 여 저장소 대상을 제거 하거나 수정할 수 있습니다.

저장소 유형에 따라 다음과 같은 저장소 대상 값을 수정할 수 있습니다.

* Blob 저장소 대상의 경우 네임 스페이스 경로 및 액세스 정책을 변경할 수 있습니다.

* NFS 저장소 대상의 경우 다음 값을 변경할 수 있습니다.

  * 네임 스페이스 경로
  * 액세스 정책
  * 네임 스페이스 경로와 연결 된 저장소 내보내기 또는 내보내기 하위 디렉터리
  * 사용 모델

* ADLS-NFS 저장소 대상의 경우 네임 스페이스 경로, 액세스 정책 및 사용 모델을 변경할 수 있습니다.

저장소 대상의 이름, 유형 또는 백 엔드 저장소 시스템 (Blob 컨테이너 또는 NFS 호스트 이름/i p 주소)은 편집할 수 없습니다. 이러한 속성을 변경 해야 하는 경우 저장소 대상을 삭제 하 고 새 값으로 대체를 만듭니다.

> [!TIP]
> [AZURE HPC 캐시 관리 비디오](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) 는 Azure Portal에서 저장소 대상을 편집 하는 방법을 보여 줍니다.

## <a name="remove-a-storage-target"></a>저장소 대상 제거

### <a name="portal"></a>[포털](#tab/azure-portal)

저장소 대상을 제거 하려면 **저장소** 대상 페이지를 엽니다. 목록에서 저장소 대상을 선택 하 고 **삭제** 단추를 클릭 합니다.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[AZURE HPC 캐시에 대 한 Azure CLI를 설정](./az-cli-prerequisites.md)합니다.

[Az hpc-cache storage-target remove](/cli/azure/ext/hpc-cache/hpc-cache/storage-target#ext-hpc-cache-az-hpc-cache-storage-target-remove) 를 사용 하 여 캐시에서 저장소 대상을 삭제 합니다.

```azurecli
$ az hpc-cache storage-target remove --resource-group cache-rg --cache-name doc-cache0629 --name blob1

{- Finished ..
  "endTime": "2020-07-09T21:45:06.1631571+00:00",
  "name": "2f95eac1-aded-4860-b19c-3f089531a7ec",
  "startTime": "2020-07-09T21:43:38.5461495+00:00",
  "status": "Succeeded"
}
```

---

저장소 대상을 삭제 하면이 Azure HPC 캐시 시스템과의 저장소 시스템 연결이 제거 되지만 백 엔드 저장소 시스템은 변경 되지 않습니다. 예를 들어 Azure Blob 저장소 컨테이너를 사용 하는 경우 캐시에서 컨테이너와 해당 콘텐츠를 삭제 한 후에도 컨테이너와 해당 콘텐츠는 계속 존재 합니다. 컨테이너를 다른 Azure HPC 캐시에 추가 하거나,이 캐시에 다시 추가 하거나, Azure Portal를 사용 하 여 삭제할 수 있습니다.

저장소 대상이 제거 되기 전에 캐시에 저장 된 모든 파일 변경 내용이 백 엔드 저장소 시스템에 기록 됩니다. 변경 된 많은 데이터가 캐시에 있는 경우이 프로세스는 한 시간 이상 걸릴 수 있습니다.

## <a name="change-a-blob-storage-targets-namespace-path"></a>Blob 저장소 대상의 네임 스페이스 경로 변경

네임 스페이스 경로는 클라이언트가이 저장소 대상을 탑재 하는 데 사용 하는 경로입니다. 자세한 내용은 [집계 된 네임 스페이스 계획](hpc-cache-namespace.md) 및 [집계 된 네임 스페이스 설정](add-namespace-paths.md)을 읽어 보십시오.

Azure Blob 저장소 대상에 대해 수행할 수 있는 유일한 업데이트는 네임 스페이스 경로입니다. Azure Portal 또는 Azure CLI를 사용 하 여 변경 합니다.

### <a name="portal"></a>[포털](#tab/azure-portal)

Azure HPC 캐시에 **네임 스페이스** 페이지를 사용 합니다. 네임 스페이스 페이지는 [집계 된 네임 스페이스 설정](add-namespace-paths.md)문서에 자세히 설명 되어 있습니다.

변경할 경로의 이름을 클릭 하 고 표시 되는 편집 창에서 새 경로를 만듭니다.

![Blob 네임 스페이스 경로를 클릭 한 후 네임 스페이스 페이지의 스크린샷-오른쪽의 창에 편집 필드가 표시 됩니다.](media/edit-namespace-blob.png)

변경 후에는 **확인** 을 클릭 하 여 저장소 대상을 업데이트 하거나 **취소** 를 클릭 하 여 변경 내용을 취소 합니다.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[AZURE HPC 캐시에 대 한 Azure CLI를 설정](./az-cli-prerequisites.md)합니다.

Azure CLI를 사용 하 여 blob 저장소 대상의 네임 스페이스를 변경 하려면 [az hpc-cache blob-storage-target update](/cli/azure/ext/hpc-cache/hpc-cache/blob-storage-target#ext-hpc-cache-az-hpc-cache-blob-storage-target-update)명령을 사용 합니다. 값만 `--virtual-namespace-path` 변경할 수 있습니다.

  ```azurecli
  az hpc-cache blob-storage-target update --cache-name cache-name --name target-name \
    --resource-group rg --virtual-namespace-path "/new-path"
  ```

---

## <a name="update-an-nfs-storage-target"></a>NFS 저장소 대상 업데이트

NFS 저장소 대상의 경우 가상 네임 스페이스 경로를 변경 또는 추가 하 고, 네임 스페이스 경로가 가리키는 NFS 내보내기 또는 하위 디렉터리 값을 변경 하 고, 사용 모델을 변경할 수 있습니다.

일부 유형의 사용자 지정 DNS 설정으로 캐시의 저장소 대상에도 해당 IP 주소를 새로 고치는 컨트롤이 있습니다. 이러한 종류의 구성은 드물게 발생 합니다.

세부 정보는 다음과 같습니다.

* [집계 된 네임 스페이스 값 변경](#change-aggregated-namespace-values) (가상 네임 스페이스 경로, 액세스 정책, 내보내기 및 하위 디렉터리 내보내기)
* [사용 모델 변경](#change-the-usage-model)
* [DNS 새로 고침](#update-ip-address-custom-dns-configurations-only)

### <a name="change-aggregated-namespace-values"></a>집계 된 네임 스페이스 값 변경

Azure Portal 또는 Azure CLI를 사용 하 여 클라이언트 연결 네임 스페이스 경로, 저장소 내보내기 및 내보내기 하위 디렉터리 (사용 되는 경우)를 변경할 수 있습니다.

하나의 저장소 대상에서 유효한 경로를 여러 개 만드는 방법에 대 한 미리 알림이 필요한 경우 [NFS 네임 스페이스 경로 추가](add-namespace-paths.md#nfs-namespace-paths) 의 지침을 참조 하세요.

### <a name="portal"></a>[포털](#tab/azure-portal)

Azure HPC 캐시에 **네임 스페이스** 페이지를 사용 하 여 네임 스페이스 값을 업데이트 합니다. 이 페이지는 [집계 된 네임 스페이스 설정](add-namespace-paths.md)문서에 자세히 설명 되어 있습니다.

![NFS 업데이트 페이지가 오른쪽에 열려 있는 포털 네임 스페이스 페이지의 스크린샷](media/update-namespace-nfs.png)

1. 변경할 경로의 이름을 클릭 합니다.
1. 편집 창을 사용 하 여 새 가상 경로, 내보내기 또는 하위 디렉터리 값을 입력 하거나 다른 액세스 정책을 선택할 수 있습니다.
1. 변경 후에는 **확인** 을 클릭 하 여 저장소 대상을 업데이트 하거나 **취소** 를 클릭 하 여 변경 내용을 취소 합니다.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[AZURE HPC 캐시에 대 한 Azure CLI를 설정](./az-cli-prerequisites.md)합니다.

``--junction`` [Az hpc-cache nfs-storage-target update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target) 명령의 옵션을 사용 하 여 네임 스페이스 경로, nfs export 또는 export 하위 디렉터리를 변경할 수 있습니다.

``--junction``매개 변수는 다음 값을 사용 합니다.

* ``namespace-path`` -클라이언트 연결 가상 파일 경로
* ``nfs-export`` -클라이언트 연결 경로와 연결할 저장소 시스템 내보내기
* ``target-path`` (선택 사항)-필요한 경우 내보내기의 하위 디렉터리입니다.

예: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

문의 각 경로에 대해 세 가지 값을 모두 제공 해야 합니다 ``--junction`` . 변경 하지 않으려는 값에 대해서는 기존 값을 사용 합니다.

캐시 이름, 저장소 대상 이름 및 리소스 그룹도 모든 업데이트 명령에 필요 합니다.

예제 명령:

```azurecli
az hpc-cache nfs-storage-target update --cache-name mycache \
  --name st-name --resource-group doc-rg0619 \
  --junction namespace-path="/new-path" nfs-export="/my-export" target-path="my-subdirectory"
```

---

### <a name="change-the-usage-model"></a>사용 모델 변경

사용 모델은 캐시가 데이터를 보존 하는 방법에 영향을 줍니다. 자세히 알아보려면 [사용 모델 선택](hpc-cache-add-storage.md#choose-a-usage-model) 을 참조 하세요.

NFS 저장소 대상에 대 한 사용 모델을 변경 하려면 다음 방법 중 하나를 사용 합니다.

### <a name="portal"></a>[포털](#tab/azure-portal)

Azure Portal의 **저장소 대상** 페이지에서 사용 모델을 변경 합니다. 변경할 저장소 대상의 이름을 클릭 합니다.

![NFS 저장소 대상에 대 한 편집 페이지의 스크린샷](media/edit-storage-nfs.png)

드롭다운 선택기를 사용 하 여 새 사용 모델을 선택 합니다. **확인** 을 클릭 하 여 저장소 대상을 업데이트 하거나 **취소** 를 클릭 하 여 변경 내용을 취소 합니다.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[AZURE HPC 캐시에 대 한 Azure CLI를 설정](./az-cli-prerequisites.md)합니다.

[Az hpc-cache-storage-target update](/cli/azure/ext/hpc-cache/hpc-cache/nfs-storage-target#ext-hpc-cache-az-hpc-cache-nfs-storage-target-update) 명령을 사용 합니다.

Update 명령은 NFS 저장소 대상을 추가 하는 데 사용 하는 명령과 거의 동일 합니다. 자세한 내용과 예제는 [NFS 저장소 대상 만들기](hpc-cache-add-storage.md#create-an-nfs-storage-target) 를 참조 하세요.

사용 모델을 변경 하려면 옵션을 업데이트 ``--nfs3-usage-model`` 합니다. 예: ``--nfs3-usage-model WRITE_WORKLOAD_15``

캐시 이름, 저장소 대상 이름 및 리소스 그룹 값도 필요 합니다.

사용 모델의 이름을 확인 하려는 경우에는 [az hpc-cache usage-model list](/cli/azure/ext/hpc-cache/hpc-cache/usage-model#ext-hpc-cache-az-hpc-cache-usage-model-list)명령을 사용 합니다.

캐시가 중지 되었거나 정상 상태가 아니면 캐시가 정상 상태가 된 후 업데이트가 적용 됩니다.

---

### <a name="update-ip-address-custom-dns-configurations-only"></a>IP 주소 업데이트 (사용자 지정 DNS 구성에만 해당)

캐시에서 기본이 아닌 DNS 구성을 사용 하는 경우 백 엔드 DNS 변경으로 인해 NFS 저장소 대상의 IP 주소가 변경 될 수 있습니다. DNS 서버에서 백 엔드 저장소 시스템의 IP 주소를 변경 하는 경우 Azure HPC 캐시는 저장소 시스템에 대 한 액세스 권한을 손실할 수 있습니다.

캐시의 사용자 지정 DNS 시스템 관리자와 협력 하 여 업데이트를 계획 하는 것이 가장 좋습니다. 이러한 변경으로 인해 저장소를 사용할 수 없게 됩니다.

저장소 대상의 DNS에서 제공 하는 IP 주소를 업데이트 해야 하는 경우 저장소 대상 목록에 단추가 있습니다. **Dns 새로 고침** 을 클릭 하 여 새 IP 주소에 대 한 사용자 지정 dns 서버를 쿼리 합니다.

![저장소 대상 목록의 스크린샷. 하나의 저장소 대상의 경우 "..." 오른쪽 끝 열의 메뉴가 열리고 두 개의 옵션이 표시 됩니다. 삭제 및 DNS 새로 고침](media/refresh-dns.png)

성공 하면 업데이트는 2 분 이내에 수행 되어야 합니다. 한 번에 하나의 저장소 대상만 새로 고칠 수 있습니다. 다른 작업을 시도 하기 전에 이전 작업이 완료 될 때까지 기다립니다.

## <a name="update-an-adls-nfs-storage-target-preview"></a>ADLS-NFS 저장소 대상 업데이트 (미리 보기)

NFS 대상과 마찬가지로 ADLS-NFS 저장소 대상에 대 한 네임 스페이스 경로 및 사용 모델을 변경할 수 있습니다.

### <a name="change-an-adls-nfs-namespace-path"></a>ADLS-NFS 네임 스페이스 경로 변경

Azure HPC 캐시에 **네임 스페이스** 페이지를 사용 하 여 네임 스페이스 값을 업데이트 합니다. 이 페이지는 [집계 된 네임 스페이스 설정](add-namespace-paths.md)문서에 자세히 설명 되어 있습니다.

![광고 NFS 업데이트 페이지가 오른쪽에 열려 있는 포털 네임 스페이스 페이지의 스크린샷](media/update-namespace-adls.png)

1. 변경할 경로의 이름을 클릭 합니다.
1. 편집 창을 사용 하 여 새 가상 경로를 입력 하거나 액세스 정책을 업데이트 합니다.
1. 변경 후에는 **확인** 을 클릭 하 여 저장소 대상을 업데이트 하거나 **취소** 를 클릭 하 여 변경 내용을 취소 합니다.

### <a name="change-adls-nfs-usage-models"></a>ADLS-NFS 사용 모델 변경

ADLS-NFS 사용 모델에 대 한 구성은 NFS 사용 모델 선택과 동일 합니다. 위의 NFS 섹션에서 [사용 모델 변경](#change-the-usage-model) 의 포털 지침을 읽습니다. ADLS-NFS 저장소 대상을 업데이트 하기 위한 추가 도구는 개발 중입니다.


## <a name="next-steps"></a>다음 단계

* 이러한 옵션에 대 한 자세한 내용은 [저장소 대상 추가](hpc-cache-add-storage.md) 를 참조 하세요.
* 가상 경로 사용에 대 한 자세한 팁 [은 집계 된 네임 스페이스 계획](hpc-cache-namespace.md) 을 참조 하세요.
