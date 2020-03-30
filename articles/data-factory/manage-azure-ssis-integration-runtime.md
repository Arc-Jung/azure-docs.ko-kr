---
title: Azure-SSIS 통합 런타임 다시 구성
description: 이미 프로비전한 Azure Data Factory에서 Azure-SSIS Integration Runtime을 다시 구성하는 방법을 알아봅니다.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: anandsub
ms.openlocfilehash: fbac52d65433f2137d565ca60fcf754e49199640
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931389"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>Azure-SSIS 통합 런타임 다시 구성
이 문서는 기존 Azure-SSIS 통합 런타임을 다시 구성하는 방법을 설명합니다. Azure Data Factory에서 Azure-SSIS IR(통합 런타임) 만들려면 [Azure-SSIS IR 만들기](create-azure-ssis-integration-runtime.md)를 참조합니다.  

## <a name="data-factory-ui"></a>Data Factory UI 
Data Factory UI를 사용하여 Azure-SSIS IR을 중지하거나, 편집/다시 구성하거나 삭제할 수 있습니다. 

1. 데이터 **팩터리 UI에서** **편집** 탭으로 전환합니다. 데이터 팩터리 UI를 시작하려면 데이터 팩터리의 홈 페이지에서 **모니터 작성&을** 클릭합니다.
2. 왼쪽 창에서 **연결**을 클릭합니다.
3. 오른쪽 창에서 **Integration Runtime**으로 전환합니다. 
4. 작업 열에서 단추를 사용하여 통합 런타임을 **중지**, **편집** 또는 **삭제**할 수 있습니다. **작업** 열에서 **코드** 단추를 사용하여 통합 런타임과 연결된 JSON 정의를 볼 수 있습니다.  
    
    ![Azure SSIS IR에 대한 작업](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Azure-SSIS IR을 다시 구성하려면
1. **작업** 열에서 **중지**를 클릭하여 통합 런타임을 중지합니다. 목록 보기를 새로 고치려면 도구 모음에서 **새로 고침**을 클릭합니다. IR이 중지된 후에 첫 번째 작업을 통해 IR을 시작한다는 것을 확인할 수 있습니다. 

    ![Azure SSIS IR에 대한 작업 - 중지된 후](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. **작업** 열에서 **편집** 단추를 클릭하여 IR을 편집/다시 구성합니다. **Integration Runtime 설정** 창에서 설정을 변경합니다(예: 노드 크기, 노드 수 또는 노드당 병렬 실행 최대수). 
3. IR을 다시 시작하려면 **작업** 열에서 **시작** 단추를 클릭합니다.     

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure-SSIS 통합 런타임 인스턴스를 프로비전하고 시작한 후에는 일련의 `Stop` - `Set` - `Start` PowerShell cmdlet을 연속적으로 실행하여 다시 구성할 수 있습니다. 예를 들어 다음 PowerShell 스크립트는 Azure-SSIS 통합 런타임 인스턴스에 할당된 노드 수를 5로 변경합니다.

### <a name="reconfigure-an-azure-ssis-ir"></a>Azure-SSIS IR 다시 구성

1. 먼저 [Stop-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/stop-Azdatafactoryv2integrationruntime) cmdlet을 사용하여 Azure-SSIS 통합 런타임을 중지합니다. 이 명령은 모든 노드를 해제하고 청구를 중지합니다.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. 그런 다음 [Set-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) cmdlet을 사용하여 Azure-SSIS IR을 다시 구성합니다. 다음 샘플 명령은 Azure-SSIS Integration Runtime을 5개 노드로 확장합니다.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. 그런 다음 [시작-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) cmdlet을 사용하여 Azure-SSIS 통합 런타임을 시작합니다. 이 명령은 SSIS 패키지 실행을 위한 모든 노드를 할당합니다.   

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Azure-SSIS IR 삭제
1. 먼저 데이터 팩터리에 기존의 모든 Azure SSIS IRs를 나열합니다.

    ```powershell
    Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. 다음으로 데이터 팩터리에서 기존의 모든 Azure SSIS IRs를 중지합니다.

    ```powershell
    Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. 다음으로 데이터 팩터리에서 기존의 모든 Azure SSIS IRs를 제거합니다.

    ```powershell
    Remove-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. 마지막으로 데이터 팩터리를 제거합니다.

    ```powershell
    Remove-AzDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. 새 리소스 그룹을 만들 때 리소스 그룹을 제거합니다.

    ```powershell
    Remove-AzResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>다음 단계
Azure-SSIS 런타임에 대한 자세한 내용은 다음 항목을 참조하세요. 

- [Azure-SSIS 통합 런타임.](concepts-integration-runtime.md#azure-ssis-integration-runtime) 이 문서는 Azure-SSIS IR을 비롯한 일반적인 통합 런타임에 대한 개념 정보를 제공합니다. 
- [자습서: Azure에 SSIS 패키지 배포](tutorial-create-azure-ssis-runtime-portal.md). 이 문서는 Azure-SSIS IR을 만들고 Azure SQL 데이터베이스를 사용하여 SSIS 카탈로그를 호스트하는 단계별 지침을 제공합니다. 
- [방법: Azure-SSIS 통합 런타임 만들기](create-azure-ssis-integration-runtime.md). 자습서의 내용을 보충하는 이 문서에서는 Azure SQL Database Managed Instance를 사용하고 IR을 가상 네트워크에 조인하는 방법에 대한 지침을 제공합니다. 
- [Azure-SSIS IR을 가상 네트워크에 조인](join-azure-ssis-integration-runtime-virtual-network.md). 이 문서에서는 Azure-SSIS IR을 Azure 가상 네트워크에 조인하는 방법에 대한 개념 정보를 제공합니다. 또한 Azure Portal을 사용하여 Azure-SSIS IR이 가상 네트워크에 조인할 수 있도록 가상 네트워크를 구성하는 단계도 제공합니다. 
- [Azure-SSIS IR 모니터링](monitor-integration-runtime.md#azure-ssis-integration-runtime). 이 문서는 Azure-SSIS IR에 대한 정보와 반환된 정보의 상태 설명을 검색하는 방법을 설명합니다. 
 
