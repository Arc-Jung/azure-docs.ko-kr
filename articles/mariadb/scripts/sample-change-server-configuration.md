---
title: CLI 스크립트 - 서버 매개 변수 변경 - Azure Database for MariaDB
description: 이 샘플 CLI 스크립트는 사용 가능한 모든 서버 구성과 Azure Database for MariaDB 데이터베이스의 업데이트를 나열합니다.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: c7a46f98f74648ccae9f9f9f94c218d42056decb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99822298"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLI를 사용하여 Azure Database for MariaDB 서버의 구성 나열 및 업데이트
이 샘플 CLI 스크립트는 사용 가능한 모든 구성 매개 변수와 Azure Database for MariaDB 서버에 허용되는 값을 나열하고 *innodb_lock_wait_timeout* 을 기본값 이외의 값으로 설정합니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- 이 문서에는 Azure CLI 버전 2.0 이상이 필요합니다. Azure Cloud Shell을 사용하는 경우 최신 버전이 이미 설치되어 있습니다.

## <a name="sample-script"></a>샘플 스크립트
이 샘플 스크립트에서 강조 표시된 줄을 편집하여 관리자 사용자 이름 및 암호를 자신의 이름과 암호로 업데이트합니다.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>배포 정리
스크립트가 실행 된 후 다음 명령을 사용하여 리소스 그룹 및 관련된 모든 리소스를 제거합니다.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>스크립트 설명
이 스크립트에는 다음 표에 설명된 명령이 사용됩니다.

| **명령** | **참고** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create) | 데이터베이스를 호스팅하는 MariaDB 서버를 만듭니다. |
| [az mariadb server configuration list](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | Azure Database for MariaDB 서버의 구성을 나열합니다. |
| [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | Azure Database for MariaDB 서버의 구성을 업데이트합니다. |
| [az mariadb server configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) | Azure Database for MariaDB 서버의 구성을 표시합니다. |
| [az group delete](/cli/azure/group#az-group-delete) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계
- Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.
- 추가 스크립트 시도: [Azure Database for MariaDB용 Azure CLI 샘플](../sample-scripts-azure-cli.md)
- 서버 매개 변수에 대한 자세한 내용은 [Azure Database for MariaDB에서 서버 매개 변수 구성 방법](../howto-server-parameters.md)을 참조하세요.
