---
title: Azure-SSIS 통합 런타임을 Azure 가상 네트워크에 조인 | Microsoft Docs
description: Azure-SSIS 통합 런타임을 Azure 가상 네트워크에 조인하는 방법을 알아봅니다.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: douglasl
ms.openlocfilehash: 4f1100b7e4fa2250baf282b53ef83c5f1aaa1c0e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2018
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Azure-SSIS 통합 런타임을 Azure 가상 네트워크에 조인
다음 시나리오에서 Azure-SSIS IR(통합 런타임)을 Azure 가상 네트워크에 조인합니다. 

- 가상 네트워크의 Azure SQL Database 관리되는 인스턴스(비공개 미리 보기)에 SSIS(SQL Server Integration Services) 카탈로그 데이터베이스를 호스팅합니다.
- Azure-SSIS 통합 런타임에서 실행되는 SSIS 패키지에서 온-프레미스 데이터 저장소에 연결하려고 합니다.

 Azure Data Factory 버전 2(미리 보기)를 사용하면 클래식 배포 모델 또는 Azure Resource Manager 배포 모델을 통해 만들어진 가상 네트워크에 Azure SSIS 통합 런타임을 조인할 수 있습니다. 

> [!NOTE]
> 이 문서는 현재 미리 보기 상태인 Data Factory 버전 2에 적용됩니다. 일반 공급(GA)되는 Data Factory 버전 1 서비스를 사용하는 경우 [Data Factory 버전 1 설명서](v1/data-factory-introduction.md)를 참조하세요.

## <a name="access-to-on-premises-data-stores"></a>온-프레미스 데이터 저장소 액세스
SSIS 패키지가 공용 클라우드 데이터 저장소에만 액세스하는 경우 Azure-SSIS IR을 가상 네트워크에 조인할 필요가 없습니다. SSIS 패키지가 온-프레미스 데이터 저장소에 액세스하는 경우 Azure-SSIS IR을 온-프레미스 네트워크에 연결된 가상 네트워크에 조인해야 합니다. 

SSIS 카탈로그가 가상 네트워크에 없는 Azure SQL Database 인스턴스에 호스트되는 경우 적절한 포트를 열어야 합니다. 

SSIS 카탈로그가 가상 네트워크의 SQL Database 관리되는 인스턴스에 호스트되는 경우 다음 네트워크에 Azure-SSIS IR을 조인할 수 있습니다.

- 동일한 가상 네트워크.
- SQL Database 관리되는 인스턴스가 있는 가상 네트워크와의 네트워크 간 연결이 설정된 다른 가상 네트워크. 

가상 네트워크는 클래식 배포 모델 또는 Azure Resource Manager 배포 모델을 통해 배포할 수 있습니다. SQL Database 관리되는 인스턴스가 있는 *동일한 가상 네트워크*에 Azure-SSIS IR을 조인하려는 경우 Azure-SSIS IR이 SQL Database 관리되는 인스턴스가 있는 서브넷과 *다른 서브넷*에 있어야 합니다.   

다음 섹션에 자세한 내용이 제공됩니다.

다음은 몇 가지 유의할 사항입니다. 

- 온-프레미스 네트워크에 연결된 기존 가상 네트워크가 없는 경우 먼저 Azure-SSIS 통합 런타임이 조인할 [Azure Resource Manager 가상 네트워크](../virtual-network/quick-create-portal.md#create-a-virtual-network) 또는 [클래식 가상 네트워크](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)를 만듭니다. 그런 다음, 해당 가상 네트워크에서 온-프레미스 네트워크로 사이트 간 [VPN 게이트웨이 연결](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) 또는 [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) 연결을 구성합니다.
- Azure-SSIS IR과 같은 위치에 온-프레미스 네트워크에 연결된 기존 Azure Resource Manager 또는 클래식 가상 네트워크가 있는 경우 IR을 해당 가상 네트워크에 조인할 수 있습니다.
- Azure-SSIS IR과 다른 위치에 온-프레미스 네트워크에 연결된 기존 클래식 가상 네트워크가 있는 경우 먼저 Azure-SSIS IR을 조인할 [클래식 가상 네트워크](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)를 만듭니다. 그런 다음, [클래식-클래식 가상 네트워크](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) 연결을 구성합니다. 또는 Azure-SSIS 통합 런타임에서 조인할 [Azure Resource Manager 가상 네트워크](../virtual-network/quick-create-portal.md#create-a-virtual-network)를 만들 수 있습니다. 그런 다음, [클래식-Azure Resource Manager 가상 네트워크](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) 연결을 구성합니다.
- 온-프레미스 네트워크에 연결된 기존 Azure Resource Manager 가상 네트워크가 Azure-SSIS IR과 다른 위치에 있는 경우, 먼저 Azure-SSIS IR에서 조인할 [Azure Resource Manager 가상 네트워크](../virtual-network/quick-create-portal.md##create-a-virtual-network)를 만듭니다. 그런 다음, Azure Resource Manager-Azure Resource Manager 가상 네트워크 연결을 구성합니다. 또는 Azure-SSIS IR에서 조인할 [클래식 가상 네트워크](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)를 만들 수 있습니다. 그런 다음, [클래식-Azure Resource Manager 가상 네트워크](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) 연결을 구성합니다.

## <a name="domain-name-services-server"></a>도메인 이름 서비스 서버 
Azure-SSIS 통합 런타임에서 조인한 가상 네트워크에서 자체 DNS(도메인 이름 서비스) 서버를 사용해야 하는 경우 지침에 따라 [가상 네트워크의 Azure-SSIS 통합 런타임 노드에서 Azure 엔드포인트를 확인할 수 있는지 확인하세요](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

## <a name="network-security-group"></a>네트워크 보안 그룹
Azure-SSIS 통합 런타임에서 조인한 가상 네트워크에 NSG(네트워크 보안 그룹)를 구현해야 하는 경우 다음 포트를 통해 인바운드/아웃바운드 트래픽을 허용합니다.

| 포트 | 방향 | 전송 프로토콜 | 목적 | 인바운드 원본/아웃바운드 대상 |
| ---- | --------- | ------------------ | ------- | ----------------------------------- |
| 10100, 20100, 30100(IR을 클래식 가상 네트워크에 조인하는 경우)<br/><br/>29876, 29877(IR을 Azure Resource Manager 가상 네트워크에 조인하는 경우) | 인바운드 | TCP | Azure 서비스는 이러한 포트를 사용하여 가상 네트워크의 Azure-SSIS 통합 런타임 노드와 통신합니다. | 인터넷 | 
| 443 | 아웃바운드 | TCP | 가상 네트워크의 Azure-SSIS 통합 런타임 노드는 이 포트를 사용하여 Azure Storage, Azure Event Hubs 등의 Azure 서비스에 액세스합니다. | 인터넷 | 
| 1433<br/>11000-11999<br/>14000-14999  | 아웃바운드 | TCP | 가상 네트워크의 Azure-SSIS 통합 런타임 노드는 이러한 포트를 사용하여 Azure SQL Database 서버가 호스트하는 SSISDB에 액세스합니다(SQL Database 관리되는 인스턴스가 호스트하는 SSISDB에는 이 목적이 적용되지 않음). | 인터넷 | 

## <a name="azure-portal-data-factory-ui"></a>Azure Portal(데이터 팩터리 UI)
이 섹션에서는 Azure Portal과 데이터 팩터리 UI를 사용하여 기존 Azure SSIS 런타임을 가상 네트워크(클래식 또는 Azure Resource Manager)에 조인하는 방법을 보여줍니다. Azure SSIS IR을 가상 네트워크에 조인하기 전에 먼저 가상 네트워크를 적합하게 구성해야 합니다. 가상 네트워크(클래식 또는 Azure Resource Manager)의 형식에 따라 다음 두 섹션 중 하나를 진행합니다. 그런 다음, 세 번째 섹션을 진행하여 Azure SSIS IR을 가상 네트워크에 조인합니다. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>포털을 사용하여 클래식 가상 네트워크 구성
먼저 가상 네트워크를 구성해야 Azure-SSIS IR을 가상 네트워크에 조인할 수 있습니다.

1. Microsoft Edge 또는 Google Chrome을 시작합니다. 현재는 두 웹 브라우저에서만 Data Factory UI가 지원됩니다.
2. [Azure Portal](https://portal.azure.com)에 로그인합니다.
3. **추가 서비스**를 선택합니다. **가상 네트워크(클래식)**를 필터링하여 선택합니다.
4. 목록에서 가상 네트워크를 필터링하고 선택합니다. 
5. **가상 네트워크(클래식)** 페이지에서 **속성**을 선택합니다. 

    ![클래식 가상 네트워크 리소스 ID](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)
5. **RESOURCE ID**에 대한 복사 단추를 선택하여 클래식 네트워크의 리소스 ID를 클립보드에 복사합니다. 클립 보드의 ID를 OneNote 또는 파일에 저장합니다.
6. 왼쪽 메뉴에서 **서브넷**을 선택합니다. **사용 가능한 주소** 수가 Azure-SSIS 통합 런타임의 노드보다 큰지 확인합니다.

    ![가상 네트워크에서 사용 가능한 주소 수](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)
7. **MicrosoftAzureBatch**를 가상 네트워크의 **클래식 가상 머신 참가자** 역할에 조인합니다.

    a. 왼쪽 메뉴에서 **액세스 제어(IAM)**를 선택하고, 도구 모음에서 **추가**를 선택합니다. 
       !["액세스 제어" 및 "추가" 단추](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    나. **권한 추가** 페이지에서 **역할**에 **클래식 가상 머신 참가자**를 선택합니다. **선택** 상자에 **ddbf3205-c6bd-46ae-8127-60eb93363864**를 붙여넣고 검색 결과 목록에서 **Microsoft Azure Batch**를 선택합니다.   
       !["권한 추가" 페이지의 검색 결과](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    다. **저장**을 선택하여 설정을 저장하고 페이지를 닫습니다.  
       ![액세스 설정 저장](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    d. 참가자 목록에 **Microsoft Azure Batch**가 보이는지 확인합니다.  
       ![Azure Batch 액세스 확인](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

5. 가상 네트워크가 있는 Azure 구독에 Azure Batch 공급자가 등록되었는지 확인합니다. 또는 Azure Batch 공급자를 등록합니다. Azure 배치 계정이 구독에 이미 있는 경우 구독이 Azure 배치에 등록됩니다.

   a. Azure Portal의 왼쪽 메뉴에서 **구독**을 선택합니다.

   나. 사용 중인 구독을 선택합니다.

   다. 왼쪽에서 **리소스 공급자**를 선택하고 **Microsoft.Batch**가 등록된 공급자인지 확인합니다.     
      !["등록됨" 상태 확인](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   목록에서 **Microsoft.Batch**가 보이지 않으면 지금 등록할 수 있도록 구독에서 [빈 Azure Batch 계정을 만듭니다](../batch/batch-account-create-portal.md). 나중에 삭제할 수 있습니다. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>포털을 사용하여 Azure Resource Manager 가상 네트워크 구성
먼저 가상 네트워크를 구성해야 Azure-SSIS IR을 가상 네트워크에 조인할 수 있습니다.

1. Microsoft Edge 또는 Google Chrome을 시작합니다. 현재는 두 웹 브라우저에서만 Data Factory UI가 지원됩니다.
2. [Azure Portal](https://portal.azure.com)에 로그인합니다.
3. **추가 서비스**를 선택합니다. **가상 네트워크**를 필터링하여 선택합니다.
4. 목록에서 가상 네트워크를 필터링하고 선택합니다. 
5. **가상 네트워크** 페이지에서 **속성**을 선택합니다. 
6. **리소스 ID**에 대한 복사 단추를 선택하여 가상 네트워크에 대한 리소스 ID를 클립보드에 복사합니다. 클립 보드의 ID를 OneNote 또는 파일에 저장합니다.
7. 왼쪽 메뉴에서 **서브넷**을 선택합니다. **사용 가능한 주소** 수가 Azure-SSIS 통합 런타임의 노드보다 큰지 확인합니다.
8. 가상 네트워크가 있는 Azure 구독에 Azure Batch 공급자가 등록되었는지 확인합니다. 또는 Azure Batch 공급자를 등록합니다. Azure 배치 계정이 구독에 이미 있는 경우 구독이 Azure 배치에 등록됩니다.

   a. Azure Portal의 왼쪽 메뉴에서 **구독**을 선택합니다.

   나. 사용 중인 구독을 선택합니다. 
   
   다. 왼쪽에서 **리소스 공급자**를 선택하고 **Microsoft.Batch**가 등록된 공급자인지 확인합니다.     
      !["등록됨" 상태 확인](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   목록에서 **Microsoft.Batch**가 보이지 않으면 지금 등록할 수 있도록 구독에서 [빈 Azure Batch 계정을 만듭니다](../batch/batch-account-create-portal.md). 나중에 삭제할 수 있습니다.

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR을 가상 네트워크에 조인


1. Microsoft Edge 또는 Google Chrome을 시작합니다. 현재는 두 웹 브라우저에서만 Data Factory UI가 지원됩니다.
2. [Azure Portal](https://portal.azure.com)의 왼쪽 메뉴에서 **데이터 팩터리**를 선택합니다. 메뉴에 **데이터 팩터리**가 표시되지 않으면 **다른 서비스**를 선택하고, **INTELLIGENCE + ANALYTICS** 섹션에서 **데이터 팩터리**를 선택합니다. 
    
   ![데이터 팩터리 목록](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)
2. 목록에서 Azure SSIS 통합 런타임의 데이터 팩터리를 선택합니다. 데이터 팩터리의 홈 페이지가 표시됩니다. **작성자 및 배포** 타일을 선택합니다. 별도의 탭에 Data Factory UI가 표시됩니다. 

   ![데이터 팩터리 홈페이지](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)
3. 데이터 팩터리 UI에서 **편집** 탭으로 전환하고 **연결**을 선택한 다음, **통합 런타임** 탭으로 전환합니다. 

   !["통합 런타임" 탭](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)
4. Azure-SSIS IR이 실행 중이면 통합 런타임 목록에서 Azure-SSIS IR에 대한 **동작** 열의 **중지** 단추를 선택합니다. 중지해야 IR을 편집할 수 있습니다. 

   ![IR 중지](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)
1. 통합 런타임 목록에서 Azure-SSIS IR에 대한 **동작** 열의 **편집** 단추를 선택합니다.

   ![통합 런타임 편집](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)
5. **통합 런타임 설정**의 **일반 설정** 페이지에서 **다음**을 선택합니다. 

   ![IR 설치를 위한 일반 설정](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-general-settings.png)
6. **SQL 설정** 페이지에서 관리자 암호를 입력하고 **다음**을 선택합니다.

   ![IR 설치를 위한 SQL Server 설정](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-sql-settings.png)
7. **고급 설정** 페이지에서 다음을 수행합니다. 

   a. **Azure-SSIS Integration Runtime이 조인할 VNet을 선택하고 Azure Services가 VNet 권한/설정을 구성하도록 허용**에 대한 확인란을 선택합니다.

   나. **형식**에서는 가상 네트워크가 클래식 가상 네트워크인지 아니면 Azure Resource Manager 가상 네트워크인지 지정합니다. 

   다. **VNet 이름**에서는 해당 가상 네트워크를 선택합니다.

   d. **서브넷 이름**에서는 해당 가상 네트워크의 서브넷을 선택합니다.

   e. **업데이트**를 선택합니다. 

   ![IR 설치를 위한 고급 설정](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)
8. 이제 Azure SSIS IR에 대한 **동작** 열의 **시작** 단추를 사용하여 IR을 시작할 수 있습니다. Azure SSIS IR을 시작하는 데 약 20분이 걸립니다. 


## <a name="azure-powershell"></a>Azure PowerShell

### <a name="configure-a-virtual-network"></a>가상 네트워크 구성
먼저 가상 네트워크를 구성해야 Azure-SSIS IR을 가상 네트워크에 조인할 수 있습니다. 조인할 Azure-SSIS 통합 런타임에 대한 가상 네트워크 권한/설정을 자동으로 구성하려면 다음 스크립트를 추가합니다.

```powershell
# Register to the Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Azure-SSIS IR을 만들어 가상 네트워크에 조인
Azure-SSIS IR을 만드는 동시에 가상 네트워크에 조인할 수 있습니다. 전체 스크립트 및 지침은 [Azure-SSIS 통합 런타임 만들기](create-azure-ssis-integration-runtime.md#azure-powershell)를 참조하세요.

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>기존 Azure-SSIS IR을 가상 네트워크에 조인
[Azure-SSIS 통합 런타임 만들기](create-azure-ssis-integration-runtime.md) 문서의 스크립트는 동일한 스크립트에서 Azure-SSIS IR을 만들고 가상 네트워크에 조인하는 방법을 보여줍니다. 기존 Azure-SSIS가 있는 경우에는 다음 단계를 수행하여 가상 네트워크에 조인합니다. 

1. Azure-SSIS IR을 중지합니다.
2. 가상 네트워크에 조인하도록 Azure-SSIS IR을 구성합니다. 
3. Azure-SSIS IR을 시작합니다. 

### <a name="define-the-variables"></a>변수를 정의합니다.

```powershell
$ResourceGroupName = "<Azure resource group name>"
$DataFactoryName = "<Data factory name>" 
$AzureSSISName = "<Specify Azure-SSIS IR name>"
## These two parameters apply if you are using a virtual network and Azure SQL Database Managed Instance (private preview) 
# Specify information about your classic or Azure Resource Manager virtual network.
$VnetId = "<Name of your Azure virtual network>"
$SubnetName = "<Name of the subnet in the virtual network>"
```

#### <a name="guidelines-for-selecting-a-subnet"></a>서브넷 선택 지침
-   GatewaySubnet은 가상 네트워크 게이트웨이 전용이므로 Azure SSIS Integration Runtime을 배포할 때는 선택하지 마세요.
-   선택한 서브넷에 Azure-SSIS IR에 사용할 수 있는 충분한 주소 공간이 있는지 확인합니다. 사용 가능한 IP 주소를 IR 노드 수의 2배 이상으로 유지합니다. Azure는 각 서브넷 내의 일부 IP 주소를 예약하며, 이러한 주소는 사용할 수 없습니다. 서브넷의 첫 번째 및 마지막 IP 주소는 Azure 서비스에 사용되는 3개 이상의 주소와 함께 프로토콜 적합성을 위해 예약됩니다. 자세한 내용은 [이러한 서브넷 내에서 IP 주소를 사용하는데 제한 사항이 있습니까?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)를 참조하세요.


### <a name="stop-the-azure-ssis-ir"></a>Azure-SSIS IR 중지
가상 네트워크에 조인하려면 먼저 Azure-SSIS 통합 런타임을 중지합니다. 이 명령은 모든 노드를 해제하고 청구를 중지합니다.

```powershell
Stop-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force 
```
### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Azure-SSIS IR이 조인하기 위한 가상 네트워크 설정을 구성합니다.
Azure Batch 리소스 공급자에 등록합니다.

```powershell
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Azure-SSIS IR 구성
가상 네트워크에 조인하도록 Azure-SSIS 통합 런타임을 구성하려면 `Set-AzureRmDataFactoryV2IntegrationRuntime` 명령을 실행합니다. 

```powershell
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure-SSIS IR 시작
Azure-SSIS 통합 런타임을 시작하려면 다음 명령을 실행합니다. 

```powershell
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```
이 명령을 완료하는 데 20~30분이 걸립니다.

## <a name="use-azure-expressroute-with-the-azure-ssis-ir"></a>Azure-SSIS IR에서 Azure ExpressRoute 사용

[Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) 회로를 가상 네트워크 인프라로 연결하여 온-프레미스 네트워크를 Azure로 확장할 수 있습니다. 

일반적인 구성은 조사 및 로깅을 위해 아웃바운드 인터넷 트래픽을 강제로 VNet 흐름에서 온-프레미스 네트워크 어플라이언스로 변경하는 강제 터널링(BGP 경로, 0.0.0.0/0을 VNet에 보급)을 사용하는 것입니다. 이 트래픽 흐름은 VNet의 Azure-SSIS IR과 Azure Data Factory 서비스 간의 연결을 끊습니다. 해결책은 하나의(또는 그 이상) [UDR(사용자 정의 경로)](../virtual-network/virtual-networks-udr-overview.md)을 Azure-SSIS IR을 포함하는 서브넷에 정의하는 것입니다. UDR이 정의한 특정 서브넷 경로는 BGP 경로 대신 적용됩니다.

가능한 경우 다음 구성을 사용합니다.
-   ExpressRoute 구성은 0.0.0.0/0을 보급하고 기본적으로 모든 아웃바운드 트래픽 온-프레미스를 강제로 터널링합니다.
-   Azure-SSIS IR을 포함하는 서브넷에 적용된 UDR은 다음 홉 형식을 갖는 0.0.0.0/0 경로를 ‘Internet’으로 정의합니다.
- 
이러한 단계의 결합된 효과는 서브넷 수준 UDR이 강제된 터널링에 ExpressRoute를 담당하고 Azure-SSIS IR에서 아웃바운드 인터넷 액세스를 보장합니다.

해당 서브넷에서의 아웃바운드 인터넷 트래픽을 조사하는 기능을 그대로 유지하려는 경우, 아웃바운드 대상을 [Azure 데이터 센터 IP 주소](https://www.microsoft.com/download/details.aspx?id=41653)로 제한하는 NSG 규칙을 서브넷에 추가할 수도 있습니다.

예제를 보려면 [이 PowerShell 스크립트](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c)를 참조하세요. 이 스크립트를 매주 실행하여 Azure 데이터 센터 IP 주소 목록을 최신 상태로 유지해야 합니다.

## <a name="next-steps"></a>다음 단계
Azure-SSIS 런타임에 대한 자세한 내용은 다음 항목을 참조하세요. 

- [Azure-SSIS 통합 런타임](concepts-integration-runtime.md#azure-ssis-integration-runtime). 이 문서에서는 Azure-SSIS IR을 비롯한 일반적인 통합 런타임에 대한 개념 정보를 제공합니다. 
- [자습서: Azure에 SSIS 패키지 배포](tutorial-create-azure-ssis-runtime-portal.md). 이 문서에서는 Azure-SSIS IR을 만드는 단계별 지침을 제공합니다. Azure SQL 데이터베이스를 사용하여 SSISDB 카탈로그를 호스트합니다. 
- [Azure-SSIS 통합 런타임을 만듭니다](create-azure-ssis-integration-runtime.md). 자습서의 내용을 보충하는 이 문서에서는 Azure SQL Database 관리되는 인스턴스(비공개 미리 보기)를 사용하고 IR을 가상 네트워크에 조인하는 방법에 대한 지침을 제공합니다. 
- [Azure-SSIS IR 모니터링](monitor-integration-runtime.md#azure-ssis-integration-runtime). 이 문서는 Azure-SSIS IR에 대한 정보와 반환된 정보의 상태 설명을 검색하는 방법을 설명합니다. 
- [Azure-SSIS IR 관리](manage-azure-ssis-integration-runtime.md). 이 문서는 Azure-SSIS IR을 중지, 시작 또는 제거하는 방법을 설명합니다. 또한 노드를 추가하여 Azure-SSIS IR을 규모 확장하는 방법을 보여줍니다. 
