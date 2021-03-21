---
title: 서버 매개 변수 구성-Azure PowerShell-Azure Database for MySQL
description: 이 문서에서는 PowerShell을 사용 하 여 Azure Database for MySQL에서 서비스 매개 변수를 구성 하는 방법을 설명 합니다.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: how-to
ms.date: 10/1/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 973025dfd8c0141ed0884539fe5207cc64ec822c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94541864"
---
# <a name="configure-server-parameters-in-azure-database-for-mysql-using-powershell"></a>PowerShell을 사용 하 여 Azure Database for MySQL에서 서버 매개 변수 구성

PowerShell을 사용 하 여 Azure Database for MySQL 서버에 대 한 구성 매개 변수를 나열 하 고, 표시 하 고, 업데이트할 수 있습니다. 엔진 구성의 하위 집합은 서버 수준에서 노출되고 수정할 수 있습니다.

>[!Note]
> 서버 매개 변수는 서버 수준에서 전역적으로 업데이트될 수 있으며 [Azure CLI](./howto-configure-server-parameters-using-cli.md), [PowerShell](./howto-configure-server-parameters-using-powershell.md) 또는 [Azure Portal](./howto-server-parameters.md)을 사용합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법 가이드를 완료하려면 다음이 필요합니다.

- 로컬에 설치 되거나 브라우저에 [Azure Cloud Shell](https://shell.azure.com/) 된 [Az PowerShell 모듈](/powershell/azure/install-az-ps)
- [Azure Database for MySQL 서버](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az.MySql PowerShell 모듈이 미리 보기에 있지만 `Install-Module -Name Az.MySql -AllowPrerelease` 명령을 사용하여 Az PowerShell 모듈과 별도로 설치해야 합니다.
> Az.MySql PowerShell 모듈이 일반 공급되면 이후 Az PowerShell 모듈 릴리스에 포함되며 Azure Cloud Shell 내에서 기본적으로 사용할 수 있습니다.

PowerShell을 로컬로 사용 하도록 선택 하는 경우 [AzAccount](/powershell/module/az.accounts/Connect-AzAccount) cmdlet을 사용 하 여 Azure 계정에 연결 합니다.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Azure Database for MySQL에 대한 서버 구성 매개 변수 나열

서버에 있는 수정 가능한 모든 매개 변수와 해당 값을 나열 하려면 cmdlet을 실행 `Get-AzMySqlConfiguration` 합니다.

다음 예에서는 **myresourcegroup** 리소스 그룹의 **mydemoserver** 서버에 대 한 서버 구성 매개 변수를 나열 합니다.

```azurepowershell-interactive
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

나열된 각 매개 변수의 정의를 보려면 [서버 시스템 변수](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)에서 MySQL 참조 섹션을 참조하세요.

## <a name="show-server-configuration-parameter-details"></a>서버 구성 매개 변수 세부 정보 표시

서버에 대 한 특정 구성 매개 변수에 대 한 세부 정보를 표시 하려면 cmdlet을 실행 하 `Get-AzMySqlConfiguration` 고 **Name** 매개 변수를 지정 합니다.

이 예에서는 리소스 그룹 **myresourcegroup** 에서 서버 **mydemoserver** 에 대 한 **저속 \_ 쿼리 \_ 로그** 서버 구성 매개 변수의 세부 정보를 보여 줍니다.

```azurepowershell-interactive
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>서버 구성 매개 변수 값 수정

특정 서버 구성 매개 변수의 값을 수정할 수 있습니다. 그러면 MySQL 서버 엔진에 대한 기본 구성 값이 업데이트됩니다. 구성을 업데이트 하려면 cmdlet을 사용 `Update-AzMySqlConfiguration` 합니다.

리소스 그룹 **myresourcegroup** 에서 서버 **mydemoserver** 의 **저속 \_ 쿼리 \_ 로그** 서버 구성 매개 변수를 업데이트 합니다.

```azurepowershell-interactive
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [PowerShell을 사용 하 여 Azure Database for MySQL 서버에서 저장소를 자동으로 확장](howto-auto-grow-storage-powershell.md)합니다.
