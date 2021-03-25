---
title: '빠른 시작: 서버 만들기 - Azure CLI - Azure Database for MySQL - 유연한 서버'
description: 이 빠른 시작에서는 Azure CLI를 사용하여 Azure 리소스 그룹에서 Azure Database for MySQL 유연한 서버를 만드는 방법을 살펴봅니다.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 9/21/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 65cc3d2fdcbdea934e80a5f0012ca4f3da157ca3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94843437"
---
# <a name="quickstart-create-an-azure-database-for-mysql-flexible-server-using-azure-cli"></a>빠른 시작: Azure CLI를 사용하여 Azure Database for MySQL 유연한 서버 만들기

이 빠른 시작에서는 [Azure Cloud Shell](https://shell.azure.com)에서 [Azure CLI](/cli/azure/get-started-with-azure-cli) 명령을 사용하여 5분 안에 Azure Database for MySQL 유연한 서버를 만드는 방법을 보여 줍니다. Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

> [!IMPORTANT] 
> Azure Database for MySQL 유연한 서버는 현재 공개 미리 보기로 제공됩니다.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell 시작

[Azure Cloud Shell](../../cloud-shell/overview.md)은 이 문서의 단계를 실행하는 데 무료로 사용할 수 있는 대화형 셸입니다. 공용 Azure 도구가 사전 설치되어 계정에서 사용하도록 구성되어 있습니다.

Cloud Shell을 열려면 코드 블록의 오른쪽 위 모서리에 있는 **사용해 보세요** 를 선택하기만 하면 됩니다. 또한 [https://shell.azure.com/bash](https://shell.azure.com/bash)로 이동하여 별도의 브라우저 탭에서 Cloud Shell을 열 수도 있습니다. **복사** 를 선택하여 코드 블록을 복사하여 Cloud Shell에 붙여넣고, **Enter** 를 선택하여 실행합니다.

CLI를 로컬로 설치하고 사용하려면 이 빠른 시작에서 Azure CLI 버전 2.0 이상이 필요합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

[az login](/cli/azure/reference-index#az-login) 명령을 사용하여 계정에 로그인해야 합니다. Azure 계정에 대한 **구독 ID** 를 참조하는 **id** 속성을 기록해 둡니다.

```azurecli-interactive
az login
```

[az account set](/cli/azure/account#az-account-set) 명령을 사용하여 계정에 속한 특정 구독을 선택합니다. 명령에서 **subscription** 인수에 대한 값으로 사용할 **az login** 출력의 **id** 값을 적어 둡니다. 구독이 여러 개인 경우 리소스가 과금되어야 할 적절한 구독을 선택합니다. 모든 구독을 가져오려면 [az account list](/cli/azure/account#az-account-list)를 사용합니다.

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-a-flexible-server"></a>유연한 서버 만들기

`az group create` 명령을 사용하여 [Azure 리소스 그룹](../../azure-resource-manager/management/overview.md)을 만든 다음, 이 리소스 그룹 내에 MySQL 유연한 서버를 만듭니다. 고유한 이름을 제공해야 합니다. 다음 예제에서는 `eastus2` 위치에 `myresourcegroup`이라는 리소스 그룹을 만듭니다.

```azurecli-interactive
az group create --name myresourcegroup --location eastus2
```

`az mysql flexible-server create` 명령을 사용하여 유연한 서버를 만듭니다. 서버는 여러 데이터베이스를 포함할 수 있습니다. 다음 명령은 Azure CLI의 [로컬 컨텍스트](/cli/azure/local-context)에 있는 서비스 기본값 및 값을 사용하여 서버를 만듭니다. 

```azurecli
az mysql flexible-server create
```

생성된 서버에는 다음과 같은 특성이 있습니다. 
- 자동 생성된 서버 이름, 관리자 이름, 관리자 암호, 리소스 그룹 이름(로컬 컨텍스트에서 아직 지정되지 않은 경우) 및 리소스 그룹과 동일한 위치. 
- 나머지 서버 구성에 대한 서비스 기본값: 컴퓨팅 계층(버스트 가능), 컴퓨팅 크기/SKU(B1MS), 백업 보존 기간(7일) 및 MySQL 버전(5.7)
- 기본 연결 방법은 자동 생성된 가상 네트워크 및 서브넷을 사용하는 프라이빗 액세스(VNet 통합)입니다.

> [!NOTE] 
> 서버를 만든 후에는 연결 방법을 변경할 수 없습니다. 예를 들어 만드는 동안 *프라이빗 액세스(VNet 통합)* 를 선택한 경우 만든 후에 *퍼블릭 액세스(허용된 IP 주소)* 로 변경할 수 없습니다. VNet 통합을 사용하여 서버에 안전하게 액세스하려면 프라이빗 액세스 권한이 있는 서버를 만드는 것이 좋습니다. [개념 문서](./concepts-networking.md)에서 프라이빗 액세스에 대해 자세히 알아보세요.

기본값을 변경하려는 경우 구성 가능한 CLI 매개 변수의 전체 목록에 대한 Azure CLI [참조 설명서](/cli/azure/mysql/flexible-server)를 참조하세요. 

다음은 몇 가지 샘플 출력입니다. 

```json
Command group 'mysql flexible-server' is in preview. It may be changed/removed in a future release.
Creating Resource Group 'groupXXXXXXXXXX'...
Creating new vnet "serverXXXXXXXXXVNET" in resource group "groupXXXXXXXXXX"...
Creating new subnet "serverXXXXXXXXXSubnet" in resource group "groupXXXXXXXXXX" and delegating it to "Microsoft.DBforMySQL/flexibleServers"...
Creating MySQL Server 'serverXXXXXXXXX' in group 'groupXXXXXXXXXX'...
Your server 'serverXXXXXXXXX' is using sku 'Standard_B1ms' (Paid Tier). Please refer to https://aka.ms/mysql-pricing for pricing details
Creating MySQL database 'flexibleserverdb'...
Make a note of your password. If you forget, you would have to reset your password with 'az mysql flexible-server update -n serverXXXXXXXXX -g groupXXXXXXXXXX -p <new-password>'.
{
  "connectionString": "server=serverXXXXXXXXX.mysql.database.azure.com;database=flexibleserverdb;uid=secureusername;pwd=securepasswordstring",
  "databaseName": "flexibleserverdb",
  "host": "serverXXXXXXXXX.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/groupXXXXXXXXXX/providers/Microsoft.DBforMySQL/flexibleServers/serverXXXXXXXXX",
  "location": "East US 2",
  "password": "securepasswordstring",
  "resourceGroup": "groupXXXXXXXXXX",
  "skuname": "Standard_B1ms",
  "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/groupXXXXXXXXXX/providers/Microsoft.Network/virtualNetworks/serverXXXXXXXXXVNET/subnets/serverXXXXXXXXXSubnet",
  "username": "secureusername",
  "version": "5.7"
}
```

기본값을 변경하려는 경우 구성 가능한 CLI 매개 변수의 전체 목록에 대한 Azure CLI [참조 설명서](/cli/azure/mysql/flexible-server)를 참조하세요. 

> [!NOTE]
> Azure Database for MySQL에 대한 연결은 포트 3306을 통해 통신합니다. 회사 네트워크 내에서 연결하려고 하면 3306 포트를 통한 아웃바운드 트래픽이 허용되지 않을 수 있습니다. 이 경우 IT 부서에서 3306 포트를 열지 않으면 서버에 연결할 수 없습니다.

## <a name="get-the-connection-information"></a>연결 정보 가져오기

서버에 연결하려면 호스트 정보와 액세스 자격 증명을 제공해야 합니다.

```azurecli-interactive
az mysql flexible-server show --resource-group myresourcegroup --name mydemoserver
```

결과는 JSON 형식입니다. **fullyQualifiedDomainName** 및 **administratorLogin** 을 기록해 둡니다. 다음은 JSON 출력의 샘플입니다. 

```json
{
  "administratorLogin": "myadminusername",
  "administratorLoginPassword": null,
  "delegatedSubnetArguments": {
    "subnetArmResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/mydemoserverVNET/subnets/mydemoserverSubnet"
  },
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/flexibleServers/mydemoserver",
  "location": "East US 2",
  "name": "mydemoserver",
  "publicNetworkAccess": "Disabled",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 0,
    "name": "Standard_B1ms",
    "tier": "Burstable"
  },
  "storageProfile": {
    "backupRetentionDays": 7,
    "fileStorageSkuName": "Premium_LRS",
    "storageAutogrow": "Disabled",
    "storageIops": 0,
    "storageMb": 10240
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/flexibleServers",
  "version": "5.7"
}
```

## <a name="connect-using-mysql-command-line-client"></a>mysql 명령줄 클라이언트를 사용하여 연결

*프라이빗 액세스(VNet 통합)* 를 사용하여 유연한 서버를 만든 경우 서버와 동일한 VNet 내의 리소스에서 서버에 연결해야 합니다. 가상 머신을 만들고, 생성된 가상 네트워크에 추가할 수 있습니다. 

VM을 만든 후에는 머신에 SSH를 수행할 수 있으며 인기 있는 클라이언트 도구인 **[mysql.exe](https://dev.mysql.com/downloads/)** 명령줄 도구를 설치할 수 있습니다.

mysql.exe를 사용하면 다음 명령으로 연결합니다. 값을 실제 서버 이름 및 암호로 바꿉니다. 

```bash
 mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p
```

## <a name="clean-up-resources"></a>리소스 정리

다른 빠른 시작/자습서에서 이러한 리소스가 필요하지 않으면 다음 명령을 수행하여 삭제할 수 있습니다.

```azurecli-interactive
az group delete --name myresourcegroup
```

새로 만든 서버만 삭제하려면 `az mysql server delete` 명령을 실행할 수 있습니다.

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
>[MySQL로 PHP(Laravel) 웹앱 빌드](tutorial-php-database-app.md)
