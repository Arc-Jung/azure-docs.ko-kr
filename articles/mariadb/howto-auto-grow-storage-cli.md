---
title: 저장소 자동 증가-Azure CLI-Azure Database for MariaDB
description: 이 문서에서는 Azure Database for MariaDB에서 Azure CLI를 사용 하 여 저장소 자동 증가를 사용 하도록 설정 하는 방법을 설명 합니다.
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 3c7fc5165766ff5b492f05047000814475235280
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98664438"
---
# <a name="auto-grow-azure-database-for-mariadb-storage-using-the-azure-cli"></a>Azure CLI를 사용 하 여 Azure Database for MariaDB 저장소 자동 증가
이 문서에서는 워크 로드에 영향을 주지 않고 확장 되도록 Azure Database for MariaDB server 저장소를 구성 하는 방법을 설명 합니다.

[저장소 제한에 도달](concepts-pricing-tiers.md#reaching-the-storage-limit)하는 서버는 읽기 전용으로 설정 됩니다. 저장소 자동 증가를 사용 하는 경우 프로 비전 된 저장소 수가 100 미만인 서버에 대해 사용 가능한 저장소가 프로 비전 된 저장소의 1gb 또는 10%를 초과 하는 즉시 프로 비전 된 저장소 크기는 5gb 씩 증가 합니다. 프로 비전 된 저장소 수가 100 GB를 넘는 서버에서는 사용 가능한 저장소 공간이 프로 비전 된 저장소 크기의 5% 미만이 면 프로 비전 된 저장소 크기가 5% 증가 합니다. [여기](concepts-pricing-tiers.md#storage) 에 지정 된 대로 최대 저장소 제한이 적용 됩니다.

## <a name="prerequisites"></a>필수 구성 요소

이 가이드를 완료 하려면:

- [Azure Database for MariaDB 서버가](quickstart-create-mariadb-server-database-using-azure-cli.md)필요 합니다.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- 이 문서에는 Azure CLI 버전 2.0 이상이 필요합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다.

## <a name="enable-mariadb-server-storage-auto-grow"></a>서버 저장소 자동 증가를 사용 하도록 설정

다음 명령을 사용 하 여 기존 서버에서 서버 자동 증가 저장소를 사용 하도록 설정 합니다.

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

다음 명령을 사용 하 여 새 서버를 만드는 동안 서버 자동 증가 저장소를 사용 하도록 설정 합니다.

```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.3
```

## <a name="next-steps"></a>다음 단계

[메트릭에 대 한 경고를 만드는 방법](howto-alert-metric.md)에 대해 알아봅니다.
