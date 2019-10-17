---
title: Azure에서 업데이트 관리 배포에 대 한 사전 및 사후 스크립트 구성
description: 이 문서에서는 업데이트 배포에 대 한 사전 및 사후 스크립트를 구성 하 고 관리 하는 방법을 설명 합니다.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 84cd5db812d995f1160a02917eac5857ee076c7f
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72374450"
---
# <a name="manage-pre-and-post-scripts"></a>사전 및 사후 스크립트 관리

사전 및 사후 스크립트를 사용 하면 (사전 작업) 전과 업데이트 배포 (사후 작업) 전에 Automation 계정에서 PowerShell runbook을 실행할 수 있습니다. 사전 및 사후 스크립트는 로컬이 아닌 Azure 컨텍스트에서 실행 됩니다. 사전 스크립트는 업데이트 배포를 시작할 때 실행 됩니다. 사후 스크립트는 배포가 끝나고, 구성된 모든 다시 부팅이 진행된 후에 실행됩니다.

## <a name="runbook-requirements"></a>Runbook 요구 사항

Runbook을 사전 또는 사후 스크립트로 사용하려면 Runbook을 Automation 계정으로 가져와서 게시해야 합니다. 이 프로세스에 대한 자세한 내용은 [Runbook 게시](manage-runbooks.md#publish-a-runbook)를 참조하세요.

## <a name="using-a-prepost-script"></a>사전/사후 스크립트 사용

업데이트 배포에 사전 및/또는 사후 스크립트를 사용하려면 업데이트 배포부터 만들어야 합니다. **사전 스크립트 + 사후 스크립트**를 선택 합니다. 그러면 **사전 스크립트 + 사후 스크립트 선택** 페이지가 열립니다.

![스크립트 선택](./media/pre-post-scripts/select-scripts.png)

사용하려는 스크립트를 선택합니다. 이 예에는 **UpdateManagement-TurnOnVms** Runbook이 사용되었습니다. Runbook을 선택할 때 **스크립트 구성** 페이지가 열리면 **사전 스크립트**를 선택 합니다. 완료되면 **확인**을 클릭합니다.

**UpdateManagement-TurnOffVms** 스크립트에 대해 이 프로세스를 반복합니다. 그러나 **스크립트 유형**을 선택할 때는 **사후 스크립트**를 선택합니다.

**선택한 항목** 섹션에 선택한 스크립트가 모두 표시되고 on은 사전 스크립트이고 다른 하나는 사후 스크립트입니다.

![선택한 항목](./media/pre-post-scripts/selected-items.png)

업데이트 배포 구성을 마칩니다.

업데이트 배포가 완료되면 **업데이트 배포**로 이동하여 결과를 볼 수 있습니다. 여기서 알 수 있듯이, 사전 스크립트 및 사후 스크립트의 상태가 제공됩니다.

![업데이트 결과](./media/pre-post-scripts/update-results.png)

업데이트 배포 실행을 클릭 하 여 사전 및 사후 스크립트에 대 한 추가 세부 정보를 제공 합니다. 실행 시 스크립트 원본에 대한 링크가 제공됩니다.

![배포 실행 결과](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>매개 변수 전달

사전 및 사후 스크립트를 구성 하는 경우 runbook을 예약 하는 것 처럼 매개 변수를 전달할 수 있습니다. 매개 변수는 업데이트 배포를 만들 때 정의됩니다. 사전 및 사후 스크립트는 다음 형식을 지원 합니다.

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

다른 개체 형식이 필요한 경우 Runbook에서 사용자 고유의 논리를 사용하여 다른 형식으로 캐스트할 수 있습니다.

표준 Runbook 매개 변수 외에도 추가 매개 변수가 제공됩니다. 이 매개 변수는 **SoftwareUpdateConfigurationRunContext**입니다. 이 매개 변수는 JSON 문자열이 고, 사전 또는 사후 스크립트에서 매개 변수를 정의 하는 경우 업데이트 배포에 의해 자동으로 전달 됩니다. 이 매개 변수에는 [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration)에서 반환되는 정보의 하위 집합인 업데이트 배포에 대한 정보가 포함되어 있습니다. 다음 표에서는 변수에 제공되는 속성을 보여줍니다.


### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext 속성

|자산  |설명  |
|---------|---------|
|SoftwareUpdateConfigurationName     | 소프트웨어 업데이트 구성의 이름        |
|SoftwareUpdateConfigurationRunId     | 실행의 고유 ID입니다.        |
|SoftwareUpdateConfigurationSettings     | 소프트웨어 업데이트 구성과 관련된 속성의 컬렉션         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | 업데이트 배포를 대상으로 하는 운영 체제         |
|SoftwareUpdateConfigurationSettings.duration     | 업데이트 배포의 최대 기간은 ISO8601에 따라 `PT[n]H[n]M[n]S`로 실행됩니다. "유지 관리 기간"이라고도 합니다.          |
|SoftwareUpdateConfigurationSettings.Windows     | Windows 컴퓨터와 관련된 속성의 컬렉션         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | 업데이트 배포에서 제외되는 KB(기술 자료) 목록        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | 업데이트 배포에 대해 선택한 업데이트 분류        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | 업데이트 배포에 대한 다시 부팅 설정        |
|azureVirtualMachines     | 업데이트 배포의 Azure VM에 대한 resourceId 목록        |
|nonAzureComputerNames|업데이트 배포의 비 Azure 컴퓨터 FQDN 목록|

다음 예제는 **SoftwareUpdateConfigurationRunContext** 매개 변수에 전달되는 JSON 문자열입니다.

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ],
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

모든 속성이 포함된 전체 예제는 [소프트웨어 업데이트 구성 - 이름으로 가져오기](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)에서 찾을 수 있습니다.

> [!NOTE]
> @No__t-0 개체에는 컴퓨터에 대 한 중복 된 항목이 포함 될 수 있습니다. 이로 인해 동일한 컴퓨터에서 Pre 및 Post 스크립트가 여러 번 실행 될 수 있습니다. 이 동작을 해결 하려면 `Sort-Object -Unique`을 사용 하 여 스크립트에서 고유한 VM 이름만 선택 합니다.


## <a name="stopping-a-deployment"></a>배포 중지

사전 스크립트를 기반으로 배포를 중지 하려면 예외를 [throw](automation-runbook-execution.md#throw) 해야 합니다. 예외를 throw 하지 않으면 배포 및 게시 스크립트도 계속 실행 됩니다. 아래 코드 조각에서는 예외를 throw 하는 방법을 보여 줍니다.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```

## <a name="samples"></a>샘플

사전 및 사후 스크립트에 대 한 샘플은 [스크립트 센터 갤러리](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell), [PowerShell 갤러리](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22)또는 Azure Portal를 통해 가져올 수 있습니다. 포털을 통해 가져오려면 Automation 계정의 **프로세스 자동화** 아래에서 **Runbook 갤러리**를 선택합니다. 필터에 대해 **업데이트 관리**를 사용합니다.

![갤러리 목록](./media/pre-post-scripts/runbook-gallery.png)

또는 다음 목록에서 표시한 대로 스크립트 이름으로 검색할 수 있습니다.

* 업데이트 관리 - VM 켜기
* 업데이트 관리 - VM 끄기
* 업데이트 관리 - 로컬에서 스크립트 실행
* 업데이트 관리 - 사전/사후 스크립트에 대한 템플릿
* 업데이트 관리 - 실행 명령을 사용하여 스크립트 실행

> [!IMPORTANT]
> 가져온 Runbook은 먼저 **게시**한 후에 사용해야 합니다. Automation 계정에서 Runbook을 찾으려면 **편집**을 선택하고 **게시**를 클릭합니다.

샘플은 모두 다음 예제에 정의된 기본 템플릿을 기반으로 합니다. 이 템플릿은 사전 및 사후 스크립트와 함께 사용할 고유한 runbook을 만드는 데 사용할 수 있습니다. Azure로 인증하고 `SoftwareUpdateConfigurationRunContext` 매개 변수를 처리하는 데 필요한 논리가 포함되어 있습니다.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management Pre/Post scripts.
  It requires a RunAs account.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)
#region BoilerplateAuthentication
#This requires a RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

## <a name="interacting-with-machines"></a>컴퓨터와 상호 작용

사전 및 사후 작업은 Automation 계정에서 runbook으로 실행 되며 배포의 컴퓨터에서 직접 실행 되지 않습니다. 사전 및 사후 작업은 Azure 컨텍스트에서도 실행 되며 비 Azure 컴퓨터에 대 한 액세스 권한이 없습니다. 다음 섹션에서는 Azure VM 또는 비 Azure 컴퓨터 인지에 관계 없이 컴퓨터와 직접 상호 작용 하는 방법을 보여 줍니다.

### <a name="interacting-with-azure-machines"></a>Azure 컴퓨터와 상호 작용

사전 및 사후 작업은 runbook으로 실행 되며 배포의 Azure Vm에서 기본적으로 실행 되지 않습니다. Azure Vm과 상호 작용 하려면 다음 항목이 있어야 합니다.

* 실행 계정
* 실행 하려는 runbook

Azure 컴퓨터와 상호 작용 하려면 [AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) cmdlet을 사용 하 여 azure vm과 상호 작용 해야 합니다. 이 작업을 수행 하는 방법에 대 한 예는 runbook 예제 [업데이트 관리-실행 명령을 사용 하 여 스크립트 실행](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc)을 참조 하세요.

### <a name="interacting-with-non-azure-machines"></a>비 Azure 머신과의 상호 작용

사전 및 사후 작업은 Azure 컨텍스트에서 실행 되며 비 Azure 컴퓨터에 대 한 액세스 권한이 없습니다. 비 Azure 머신과 상호 작용하려면 다음 항목이 있어야 합니다.

* 실행 계정
* 머신에 설치된 Hybrid Runbook Worker
* 로컬에서 실행하려는 Runbook
* 부모 Runbook

비 Azure 머신과 상호 작용하기 위해 부모 Runbook이 Azure 컨텍스트에서 실행됩니다. 이 Runbook은 [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet을 사용하여 자식 Runbook을 호출합니다. `-RunOn` 매개 변수를 지정하고, 스크립트가 실행될 Hybrid Runbook Worker의 이름을 제공해야 합니다. 예제는 runbook 예제 [업데이트 관리-로컬로 스크립트 실행](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44)을 참조 하세요.

## <a name="abort-patch-deployment"></a>패치 배포 중단

사전 스크립트가 오류를 반환 하는 경우 배포를 중단할 수 있습니다. 이렇게 하려면 오류를 구성 하는 논리에 대 한 스크립트에서 오류를 [throw](/powershell/module/microsoft.powershell.core/about/about_throw) 해야 합니다.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>알려진 문제

* 사전 및 사후 스크립트를 사용할 때 매개 변수에 부울, 개체 또는 배열을 전달할 수 없습니다. Runbook이 실패합니다. 지원 되는 형식의 전체 목록은 [매개 변수](#passing-parameters)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

Windows 가상 머신에 대한 업데이트를 관리하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [Azure Windows VM에 대한 업데이트 및 패치 관리](automation-tutorial-update-management.md)

