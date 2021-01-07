---
title: 읽기 복제본 관리-Azure CLI, REST API Azure Database for MariaDB
description: 이 문서에서는 Azure CLI 및 REST API를 사용 하 여 Azure Database for MariaDB에서 읽기 복제본을 설정 하 고 관리 하는 방법을 설명 합니다.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 6/10/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 70da1e9c70bf80737065362c68781652dd9ab6e5
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96023638"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-cli-and-rest-api"></a>Azure CLI 및 REST API를 사용 하 여 Azure Database for MariaDB에서 읽기 복제본을 만들고 관리 하는 방법

이 문서에서는 Azure CLI 및 REST API를 사용 하 여 Azure Database for MariaDB 서비스에서 읽기 복제본을 만들고 관리 하는 방법을 설명 합니다.

## <a name="azure-cli"></a>Azure CLI
Azure CLI를 사용 하 여 읽기 복제본을 만들고 관리할 수 있습니다.

### <a name="prerequisites"></a>필수 조건

- [Azure CLI 2.0 설치](/cli/azure/install-azure-cli)
- 원본 서버로 사용 되는 [Azure Database for MariaDB 서버](quickstart-create-mariadb-server-database-using-azure-portal.md) 입니다. 

> [!IMPORTANT]
> 복제본 읽기 기능은 범용 또는 메모리 액세스에 최적화 된 가격 책정 계층의 Azure Database for MariaDB 서버에 대해서만 사용할 수 있습니다. 원본 서버가 이러한 가격 책정 계층 중 하나에 있는지 확인 합니다.

### <a name="create-a-read-replica"></a>읽기 복제본 만들기

> [!IMPORTANT]
> 기존 복제본이 없는 원본에 대 한 복제본을 만드는 경우 원본 복제본이 먼저 다시 시작 되어 복제를 위한 준비가 됩니다. 이를 고려하고 사용량이 적은 기간 동안 이러한 작업을 수행합니다.

다음 명령을 사용하여 읽기 복제본 서버를 만들 수 있습니다.

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica create` 명령에는 다음과 같은 매개 변수가 필요합니다.

| 설정 | 예제 값 | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  복제본 서버가 만들어지는 리소스 그룹입니다.  |
| name | mydemoreplicaserver | 만들어지는 새 복제본 서버의 이름입니다. |
| source-server | mydemoserver | 복제할 기존 원본 서버의 이름 또는 ID입니다. |

지역 간 읽기 복제본을 만들려면 매개 변수를 사용 `--location` 합니다. 

아래 CLI 예제에서는 미국 서 부에 복제본을 만듭니다.

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> 복제본을 만들 수 있는 지역에 대해 자세히 알아보려면 [읽기 복제본 개념 문서](concepts-read-replicas.md)를 참조하세요. 

> [!NOTE]
> 읽기 복제본은 마스터와 같은 서버 구성을 사용하여 생성됩니다. 복제본이 생성된 후에 복제본 서버 구성을 변경할 수 있습니다. 복제본이 마스터를 유지할 수 있도록 복제본 서버 구성을 원본과 같거나 큰 값으로 유지 하는 것이 좋습니다.

### <a name="list-replicas-for-a-source-server"></a>원본 서버에 대 한 복제본 나열

지정 된 원본 서버에 대 한 모든 복제본을 보려면 다음 명령을 실행 합니다. 

```azurecli-interactive
az mariadb server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mariadb server replica list` 명령에는 다음과 같은 매개 변수가 필요합니다.

| 설정 | 예제 값 | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  복제본 서버가 만들어지는 리소스 그룹입니다.  |
| 서버 이름 | mydemoserver | 원본 서버의 이름 또는 ID입니다. |

### <a name="stop-replication-to-a-replica-server"></a>복제본 서버로의 복제 중지

> [!IMPORTANT]
> 서버로의 복제는 중지하고 나면 취소할 수 없습니다. 원본 및 복제본 간에 복제가 중지 된 후에는 실행 취소할 수 없습니다. 복제를 중지하고 나면 복제본 서버는 독립 실행형 서버가 되어 읽기와 쓰기를 모두 지원합니다. 이 서버를 다시 복제본으로 설정할 수는 없습니다.

다음 명령을 사용하여 읽기 복제본 서버에 대한 복제를 중지할 수 있습니다.

```azurecli-interactive
az mariadb server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mariadb server replica stop` 명령에는 다음과 같은 매개 변수가 필요합니다.

| 설정 | 예제 값 | Description  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  복제본 서버가 있는 리소스 그룹입니다.  |
| name | mydemoreplicaserver | 복제를 중지할 복제본 서버의 이름입니다. |

### <a name="delete-a-replica-server"></a>복제본 서버 삭제

Read replica 서버 삭제는 **[az aadb server delete](/cli/azure/mariadb/server)** 명령을 실행 하 여 수행할 수 있습니다.

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-source-server"></a>원본 서버 삭제

> [!IMPORTANT]
> 원본 서버를 삭제하면 모든 복제본 서버에 대한 복제가 중지되며 원본 서버 자체도 삭제됩니다. 그러면 복제본 서버는 읽기와 쓰기를 모두 지원하는 독립 실행형 서버로 설정됩니다.

원본 서버를 삭제 하려면 **[az mariadb server delete](/cli/azure/mariadb/server)** 명령을 실행 합니다.

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="rest-api"></a>REST API
[Azure REST API](/rest/api/azure/)를 사용 하 여 읽기 복제본을 만들고 관리할 수 있습니다.

### <a name="create-a-read-replica"></a>읽기 복제본 만들기
[CREATE API](/rest/api/mariadb/servers/create)를 사용 하 여 읽기 복제본을 만들 수 있습니다.

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> 복제본을 만들 수 있는 지역에 대해 자세히 알아보려면 [읽기 복제본 개념 문서](concepts-read-replicas.md)를 참조하세요. 

`azure.replication_support`범용 또는 메모리 액세스에 최적화 된 원본 서버에서 매개 변수를 **REPLICA** 로 설정 하지 않은 경우 서버를 다시 시작 하면 오류가 발생 합니다. 복제본을 만들기 전에이 두 단계를 완료 합니다.

복제본은 마스터와 동일한 계산 및 저장소 설정을 사용 하 여 생성 됩니다. 복제본을 만든 후에는 계산 세대, vCores, 저장소 및 백업 보존 기간 등의 여러 설정을 원본 서버와 독립적으로 변경할 수 있습니다. 가격 책정도 기본 계층에서 다른 계층으로 또는 다른 계층에서 기본 계층으로 변경하는 경우를 제외하고 독립적으로 변경할 수 있습니다.


> [!IMPORTANT]
> 원본 서버 설정을 새 값으로 업데이트 하기 전에 복제본 설정을 같거나 큰 값으로 업데이트 합니다. 이 작업을 수행 하면 복제본이 마스터의 모든 변경 내용을 유지 하는 데 도움이 됩니다.

### <a name="list-replicas"></a>복제본 나열
[복제본 목록 API](/rest/api/mariadb/replicas/listbyserver)를 사용 하 여 원본 서버의 복제본 목록을 볼 수 있습니다.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>복제본 서버로의 복제 중지
[업데이트 API](/rest/api/mariadb/servers/update)를 사용 하 여 원본 서버와 읽기 복제본 간의 복제를 중지할 수 있습니다.

원본 서버 및 읽기 복제본에 대 한 복제를 중지 한 후에는 실행 취소할 수 없습니다. 읽기 복제본은 읽기 및 쓰기를 둘 다 지원하는 독립 실행형 서버가 됩니다. 독립 실행형 서버를 다시 복제본으로 만들 수 없습니다.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-source-or-replica-server"></a>원본 또는 복제 서버 삭제
원본 또는 복제 서버를 삭제 하려면 [DELETE API](/rest/api/mariadb/servers/delete)를 사용 합니다.

원본 서버를 삭제 하면 모든 읽기 복제본으로의 복제가 중지 됩니다. 그러면 읽기 복제본은 읽기와 쓰기를 모두 지원하는 독립 실행형 서버가 됩니다.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMariaDB/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>다음 단계

- [읽기 복제본](concepts-read-replicas.md)에 대해 자세히 알아보기