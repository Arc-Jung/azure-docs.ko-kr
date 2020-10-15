---
title: Azure CLI 스크립트 - 서버 구성 변경(PostgreSQL)
description: 이 샘플 CLI 스크립트는 모든 사용 가능한 서버 구성 옵션을 나열하고 옵션 중 하나의 값을 업데이트합니다.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 02/28/2018
ms.openlocfilehash: 722db134d6cd5287a0eddee844ccbf6b6ef98a10
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91709056"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Azure CLI를 사용하여 Azure Database for PostgreSQL의 구성 나열 및 업데이트
이 샘플 CLI 스크립트는 사용 가능한 모든 구성 매개 변수와 Azure Database for PostgreSQL에 대해 허용되는 값을 나열하고 *log_retention_days*를 기본값 이외의 값으로 설정합니다.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 실행하도록 선택한 경우 이 문서에 Azure CLI 버전 2.0 이상이 필요합니다. `az --version`을 실행하여 버전을 확인합니다. Azure CLI 버전을 설치하거나 업그레이드하려면 [Azure CLI 설치]( /cli/azure/install-azure-cli)를 참조하세요. 

## <a name="sample-script"></a>샘플 스크립트
이 샘플 스크립트에서 강조 표시된 줄을 편집하여 관리자 사용자 이름 및 암호를 자신의 이름과 암호로 업데이트합니다.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>배포 정리
스크립트가 실행 된 후 다음 명령을 사용하여 리소스 그룹 및 관련된 모든 리소스를 제거합니다. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>스크립트 설명
이 스크립트에는 다음 표에 설명된 명령이 사용됩니다.

| **명령** | **참고** |
|---|---|
| [az group create](/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az postgres server create](/cli/azure/postgres/server) | 데이터베이스를 호스팅하는 PostgreSQL 서버를 만듭니다. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration) | Azure Database for PostgreSQL의 구성을 나열합니다. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration) | Azure Database for PostgreSQL의 구성을 업데이트합니다. |
| [az postgres server configuration show](/cli/azure/postgres/server/configuration) | Azure Database for PostgreSQL의 구성을 보여 줍니다. |
| [az group delete](/cli/azure/group) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계
- Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.
- 추가 스크립트 시도: [PostgreSQL용 Azure Database에 대한 Azure CLI 샘플](../sample-scripts-azure-cli.md)
- 서버 매개 변수에 대한 자세한 내용은 [Azure Portal에서 서버 매개 변수를 구성하는 방법](../howto-configure-server-parameters-using-portal.md)을 참조하세요.
