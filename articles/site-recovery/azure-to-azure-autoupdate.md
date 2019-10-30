---
title: Azure 재해 복구에 대 한 Azure의 모바일 서비스 자동 업데이트 | Microsoft Docs
description: Azure Site Recovery를 사용 하 여 Azure Vm을 복제 하는 경우 모바일 서비스의 자동 업데이트 개요입니다.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/24/2019
ms.author: rajanaki
ms.openlocfilehash: 0a8f47e0eea8908fcf6aa11c694e09efef14bbf1
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053525"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Azure 간 복제에서 모바일 서비스의 자동 업데이트

Azure Site Recovery는 월별 릴리스 주기를 사용 하 여 모든 문제를 해결 하 고 기존 기능을 향상 시키거나 새 기능을 추가 합니다. 서비스의 최신 상태를 유지 하려면 매월 패치 배포를 계획 해야 합니다. 각 업그레이드와 관련 된 오버 헤드를 방지 하기 위해 Site Recovery 구성 요소 업데이트를 관리할 수 있습니다.

[Azure-azure 재해 복구 아키텍처](azure-to-azure-architecture.md)에 설명 된 것 처럼, azure 지역 간에 vm을 복제 하는 동안 복제를 사용 하도록 설정 하는 모든 azure vm (가상 머신)에 모바일 서비스가 설치 됩니다. 자동 업데이트를 사용 하는 경우 각 새 릴리스는 모바일 서비스 확장을 업데이트 합니다.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-automatic-updates-work"></a>자동 업데이트 작동 방법

Site Recovery를 사용 하 여 업데이트를 관리 하는 경우 자격 증명 모음과 동일한 구독에서 만든 automation 계정을 통해 Azure 서비스에서 사용 되는 글로벌 runbook을 배포 합니다. 각 자격 증명 모음은 하나의 automation 계정을 사용 합니다. Runbook은 자격 증명 모음의 각 VM에 대해 활성 자동 업데이트를 확인 하 고 최신 버전을 사용할 수 있는 경우 모바일 서비스 확장을 업그레이드 합니다.

기본 runbook 일정은 복제 된 VM 지역의 표준 시간대에 매일 오전 12:00에 되풀이 됩니다. Automation 계정을 통해 runbook 일정을 변경할 수도 있습니다.

> [!NOTE]
> 업데이트 롤업 35부터 업데이트에 사용할 기존 automation 계정을 선택할 수 있습니다. 이 업데이트 이전에는 기본적으로이 계정을 Site Recovery 만들었습니다. VM에 대 한 복제를 사용 하도록 설정한 경우에만이 옵션을 선택할 수 있습니다. 복제 하는 VM에는 사용할 수 없습니다. 선택한 설정은 동일한 자격 증명 모음에서 보호 되는 모든 Azure Vm에 적용 됩니다.
 
> 자동 업데이트를 설정 하려면 Azure Vm을 다시 시작 하거나 진행 중인 복제에 영향을 주지 않습니다.

> Automation 계정의 작업 요금은 한 달에 사용 된 작업 런타임 시간 (분)을 기반으로 합니다. 기본적으로 500 분은 automation 계정의 무료 단위로 포함 됩니다. 작업 실행은 하루에 약 1 분 정도 걸리며 무료 단위로 적용 됩니다.

| 포함 된 무료 단위 (매월) | 가격 |
|---|---|
| 작업 런타임 500 분 | ₹ 0.14/분

## <a name="enable-automatic-updates"></a>자동 업데이트 사용

다음과 같은 방법으로 Site Recovery 업데이트를 관리할 수 있습니다.

### <a name="manage-as-part-of-the-enable-replication-step"></a>복제 사용 단계의 일부로 관리

[Vm 보기](azure-to-azure-quickstart.md) 또는 [recovery services 자격 증명 모음](azure-to-azure-how-to-enable-replication.md)에서 시작 하 여 vm에 대 한 복제를 사용 하도록 설정 하는 경우 Site Recovery Site Recovery 확장에 대 한 업데이트를 관리 하거나 수동으로 관리 하도록 허용할 수 있습니다.

![확장 프로그램 설정](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>자격 증명 모음 내에서 확장 업데이트 설정 전환

1. 자격 증명 모음 내에서 **관리** > **Site Recovery 인프라**로 이동 합니다.
2. **Azure Virtual Machines** > **확장 업데이트 설정**에서 **Site Recovery를 관리 하도록 허용** 토글을 설정 합니다. 수동으로 관리 하려면이 기능을 해제 합니다. 
3. **저장**을 선택합니다.

![확장 업데이트 설정](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important]
> **Site Recovery에서 관리 하도록 허용**을 선택 하면 해당 자격 증명 모음에 있는 모든 vm에 설정이 적용 됩니다.


> [!Note]
> 두 옵션은 업데이트를 관리 하는 데 사용 되는 automation 계정을 알려 줍니다. 자격 증명 모음에서이 기능을 처음 사용 하는 경우에는 기본적으로 새 automation 계정이 생성 됩니다. 또는 설정을 사용자 지정 하 고 기존 automation 계정을 선택할 수 있습니다. 동일한 자격 증명 모음에 있는 모든 후속 복제는 이전에 만든 것을 사용 합니다. 현재 드롭다운은 자격 증명 모음과 동일한 리소스 그룹에 있는 Automation 계정만 나열 합니다.  

> [!IMPORTANT]
> 아래 스크립트는 사용자 지정 automation 계정에 대 한 automation 계정의 컨텍스트에서 실행 해야 합니다. 다음 스크립트를 사용 합니다.

```azurepowershell
param(
    [Parameter(Mandatory=$true)]
    [String] $VaultResourceId,

    [Parameter(Mandatory=$true)]
    [ValidateSet("Enabled",'Disabled')]
    [Alias("Enabled or Disabled")]
    [String] $AutoUpdateAction,

    [Parameter(Mandatory=$false)]
    [String] $AutomationAccountArmId
)

$SiteRecoveryRunbookName = "Modify-AutoUpdateForVaultForPatner"
$TaskId = [guid]::NewGuid().ToString()
$SubscriptionId = "00000000-0000-0000-0000-000000000000"
$AsrApiVersion = "2018-01-10"
$RunAsConnectionName = "AzureRunAsConnection"
$ArmEndPoint = "https://management.azure.com"
$AadAuthority = "https://login.windows.net/"
$AadAudience = "https://management.core.windows.net/"
$AzureEnvironment = "AzureCloud"
$Timeout = "160"

function Throw-TerminatingErrorMessage
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    throw ("Message: {0}, TaskId: {1}.") -f $Message, $TaskId
}

function Write-Tracing
{
    Param
    (
        [Parameter(Mandatory=$true)]      
        [ValidateSet("Informational", "Warning", "ErrorLevel", "Succeeded", IgnoreCase = $true)]
        [String]
        $Level,

        [Parameter(Mandatory=$true)]
        [String]
        $Message,

        [Switch]
        $DisplayMessageToUser
    )

    Write-Output $Message

}

function Write-InformationTracing
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    Write-Tracing -Message $Message -Level Informational -DisplayMessageToUser
}

function ValidateInput()
{
    try
    {
        if(!$VaultResourceId.StartsWith("/subscriptions", [System.StringComparison]::OrdinalIgnoreCase))
        {
            $ErrorMessage = "The vault resource id should start with /subscriptions."
            throw $ErrorMessage
        }

        $Tokens = $VaultResourceId.SubString(1).Split("/")
        if(!($Tokens.Count % 2 -eq 0))
        {
            $ErrorMessage = ("Odd Number of tokens: {0}." -f $Tokens.Count)
            throw $ErrorMessage
        }

        if(!($Tokens.Count/2 -eq 4))
        {
            $ErrorMessage = ("Invalid number of resource in vault ARM id expected:4, actual:{0}." -f ($Tokens.Count/2))
            throw $ErrorMessage
        }

        if($AutoUpdateAction -ieq "Enabled" -and [string]::IsNullOrEmpty($AutomationAccountArmId))
        {
            $ErrorMessage = ("The automation account ARM id should not be null or empty when AutoUpdateAction is enabled.")
            throw $ErrorMessage
        }
    }
    catch
    {
        $ErrorMessage = ("ValidateInput failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Initialize-SubscriptionId()
{
    try
    {
        $Tokens = $VaultResourceId.SubString(1).Split("/")

        $Count = 0
        $ArmResources = @{}
        while($Count -lt $Tokens.Count)
        {
            $ArmResources[$Tokens[$Count]] = $Tokens[$Count+1]
            $Count = $Count + 2
        }
        
        return $ArmResources["subscriptions"]
    }
    catch
    {
        Write-Tracing -Level ErrorLevel -Message ("Initialize-SubscriptionId: failed with [Exception: {0}]." -f $_.Exception) -DisplayMessageToUser
        throw
    }
}

function Invoke-InternalRestMethod($Uri, $Headers, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-RestMethod -Uri $Uri -Headers $Headers    
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Invoke-InternalWebRequest($Uri, $Headers, $Method, $Body, $ContentType, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-WebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -UseBasicParsing
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Get-Header([ref]$Header, $AadAudience, $AadAuthority, $RunAsConnectionName){
    try 
    {
        $RunAsConnection = Get-AutomationConnection -Name $RunAsConnectionName
        $TenantId = $RunAsConnection.TenantId
        $ApplicationId = $RunAsConnection.ApplicationId
        $CertificateThumbprint = $RunAsConnection.CertificateThumbprint
        $Path = "cert:\CurrentUser\My\{0}" -f $CertificateThumbprint
        $Secret = Get-ChildItem -Path $Path
        $ClientCredential = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.ClientAssertionCertificate(
                $ApplicationId,
                $Secret)

        # Trim the forward slash from the AadAuthority if it exist.
        $AadAuthority = $AadAuthority.TrimEnd("/")
        $AuthContext = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(
            "{0}/{1}" -f $AadAuthority, $TenantId )
        $AuthenticationResult = $authContext.AcquireToken($AadAudience, $Clientcredential)
        $Header.Value['Content-Type'] = 'application\json'
        $Header.Value['Authorization'] = $AuthenticationResult.CreateAuthorizationHeader()
        $Header.Value["x-ms-client-request-id"] = $TaskId + "/" + (New-Guid).ToString() + "-" + (Get-Date).ToString("u")
    }
    catch
    {
        $ErrorMessage = ("Get-BearerToken: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Get-ProtectionContainerToBeModified([ref] $ContainerMappingList)
{
    try 
    {
        Write-InformationTracing ("Get protection container mappings : {0}." -f $VaultResourceId)
        $ContainerMappingListUrl = $ArmEndPoint + $VaultResourceId + "/replicationProtectionContainerMappings" + "?api-version=" + $AsrApiVersion
        
        Write-InformationTracing ("Getting the bearer token and the header.")
        Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
        
        $Result = @()
        Invoke-InternalRestMethod -Uri $ContainerMappingListUrl -Headers $header -Result ([ref]$Result)
        $ContainerMappings = $Result[0]

        Write-InformationTracing ("Total retrieved container mappings: {0}." -f $ContainerMappings.Value.Count)
        foreach($Mapping in $ContainerMappings.Value)
        {
            if(($Mapping.properties.providerSpecificDetails -eq $null) -or ($Mapping.properties.providerSpecificDetails.instanceType -ine "A2A"))
            {
                Write-InformationTracing ("Mapping properties: {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the provider does not match." -f ($Mapping.Id))
                continue;
            }

            if($Mapping.Properties.State -ine "Paired")
            {
                Write-InformationTracing ("Ignoring container mapping: {0} as the state is not paired." -f ($Mapping.Id))
                continue;
            }

            Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties.providerSpecificDetails))
            $MappingAutoUpdateStatus = $Mapping.properties.providerSpecificDetails.agentAutoUpdateStatus
            $MappingAutomationAccountArmId = $Mapping.properties.providerSpecificDetails.automationAccountArmId
            $MappingHealthErrorCount = $Mapping.properties.HealthErrorDetails.Count

            if($AutoUpdateAction -ieq "Enabled" -and
                ($MappingAutoUpdateStatus -ieq "Enabled") -and
                ($MappingAutomationAccountArmId -ieq $AutomationAccountArmId) -and
                ($MappingHealthErrorCount -eq 0))
            {
                Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the auto update is already enabled and is healthy." -f ($Mapping.Id))
                continue;
            }

            ($ContainerMappingList.Value).Add($Mapping.id)
        }
    }
    catch
    {
        $ErrorMessage = ("Get-ProtectionContainerToBeModified: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

$OperationStartTime = Get-Date
$ContainerMappingList = New-Object System.Collections.Generic.List[System.String]
$JobsInProgressList = @()
$JobsCompletedSuccessList = @()
$JobsCompletedFailedList = @()
$JobsFailedToStart = 0
$JobsTimedOut = 0
$Header = @{}

$AzureRMProfile = Get-Module -ListAvailable -Name Az.Accounts | Select Name, Version, Path
$AzureRmProfileModulePath = Split-Path -Parent $AzureRMProfile.Path
Add-Type -Path (Join-Path $AzureRmProfileModulePath "Microsoft.IdentityModel.Clients.ActiveDirectory.dll")

$Inputs = ("Tracing inputs VaultResourceId: {0}, Timeout: {1}, AutoUpdateAction: {2}, AutomationAccountArmId: {3}." -f $VaultResourceId, $Timeout, $AutoUpdateAction, $AutomationAccountArmId)
Write-Tracing -Message $Inputs -Level Informational -DisplayMessageToUser
$CloudConfig = ("Tracing cloud configuration ArmEndPoint: {0}, AadAuthority: {1}, AadAudience: {2}." -f $ArmEndPoint, $AadAuthority, $AadAudience)
Write-Tracing -Message $CloudConfig -Level Informational -DisplayMessageToUser
$AutomationConfig = ("Tracing automation configuration RunAsConnectionName: {0}." -f $RunAsConnectionName)
Write-Tracing -Message $AutomationConfig -Level Informational -DisplayMessageToUser

ValidateInput
$SubscriptionId = Initialize-SubscriptionId
Get-ProtectionContainerToBeModified ([ref]$ContainerMappingList)

$Input = @{
  "properties"= @{
    "providerSpecificInput"= @{
        "instanceType" = "A2A"
        "agentAutoUpdateStatus" = $AutoUpdateAction
        "automationAccountArmId" = $AutomationAccountArmId
    }
  }
}
$InputJson = $Input |  ConvertTo-Json

if ($ContainerMappingList.Count -eq 0)
{
    Write-Tracing -Level Succeeded -Message ("Exiting as there are no container mappings to be modified.") -DisplayMessageToUser
    exit
}

Write-InformationTracing ("Container mappings to be updated has been retrieved with count: {0}." -f $ContainerMappingList.Count)

try
{
    Write-InformationTracing ("Start the modify container mapping jobs.")
    ForEach($Mapping in $ContainerMappingList)
    {
    try {
            $UpdateUrl = $ArmEndPoint + $Mapping + "?api-version=" + $AsrApiVersion
            Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
            
            $Result = @()
            Invoke-InternalWebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -Result ([ref]$Result)
            $Result = $Result[0]

            $JobAsyncUrl = $Result.Headers['Azure-AsyncOperation']
            Write-InformationTracing ("The modify container mapping job invoked with async url: {0}." -f $JobAsyncUrl)
            $JobsInProgressList += $JobAsyncUrl;

            # Rate controlling the set calls to maximum 60 calls per minute.
            # ASR throttling for set calls is 200 in 1 minute.
            Start-Sleep -Milliseconds 1000
        }
        catch{
            Write-InformationTracing ("The modify container mappings job creation failed for: {0}." -f $Ru)
            Write-InformationTracing $_
            $JobsFailedToStart++
        }
    }

    Write-InformationTracing ("Total modify container mappings has been initiated: {0}." -f $JobsInProgressList.Count)
}
catch
{
    $ErrorMessage = ("Modify container mapping jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

try
{
    while($JobsInProgressList.Count -ne 0)
    {
        Sleep -Seconds 30
        $JobsInProgressListInternal = @()
        ForEach($JobAsyncUrl in $JobsInProgressList)
        {
            try
            {
                Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
                $Result = Invoke-RestMethod -Uri $JobAsyncUrl -Headers $header
                $JobState = $Result.Status
                if($JobState -ieq "InProgress")
                {
                    $JobsInProgressListInternal += $JobAsyncUrl
                }
                elseif($JobState -ieq "Succeeded" -or `
                    $JobState -ieq "PartiallySucceeded" -or `
                    $JobState -ieq "CompletedWithInformation")
                {
                    Write-InformationTracing ("Jobs succeeded with state: {0}." -f $JobState)
                    $JobsCompletedSuccessList += $JobAsyncUrl
                }
                else
                {
                    Write-InformationTracing ("Jobs failed with state: {0}." -f $JobState)
                    $JobsCompletedFailedList += $JobAsyncUrl
                }
            }
            catch
            {
                Write-InformationTracing ("The get job failed with: {0}. Ignoring the exception and retrying the next job." -f $_.Exception)

                # The job on which the tracking failed, will be considered in progress and tried again later.
                $JobsInProgressListInternal += $JobAsyncUrl
            }

            # Rate controlling the get calls to maximum 120 calls each minute.
            # ASR throttling for get calls is 10000 in 60 minutes.
            Start-Sleep -Milliseconds 500
        }

        Write-InformationTracing ("Jobs remaining {0}." -f $JobsInProgressListInternal.Count)

        $CurrentTime = Get-Date
        if($CurrentTime -gt $OperationStartTime.AddMinutes($Timeout))
        {
            Write-InformationTracing ("Tracing modify cloud pairing jobs has timed out.")
            $JobsTimedOut = $JobsInProgressListInternal.Count
            $JobsInProgressListInternal = @()
        }

        $JobsInProgressList = $JobsInProgressListInternal
    }
}
catch
{
    $ErrorMessage = ("Tracking modify cloud pairing jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage  -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage 
}

Write-InformationTracing ("Tracking modify cloud pairing jobs completed.")
Write-InformationTracing ("Modify cloud pairing jobs success: {0}." -f $JobsCompletedSuccessList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed: {0}." -f $JobsCompletedFailedList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed to start: {0}." -f $JobsFailedToStart)
Write-InformationTracing ("Modify cloud pairing jobs timedout: {0}." -f $JobsTimedOut)

if($JobsTimedOut -gt  0)
{
    $ErrorMessage = "One or more modify cloud pairing jobs has timedout."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)   
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}
elseif($JobsCompletedSuccessList.Count -ne $ContainerMappingList.Count)
{
    $ErrorMessage = "One or more modify cloud pairing jobs failed."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

Write-Tracing -Level Succeeded -Message ("Modify cloud pairing completed.") -DisplayMessageToUser
```

### <a name="manage-updates-manually"></a>수동으로 업데이트 관리

1. Vm에 설치 된 모바일 서비스에 대 한 새 업데이트가 있는 경우 다음과 같은 알림이 표시 됩니다. "새 Site Recovery 복제 에이전트 업데이트를 사용할 수 있습니다. 설치 하려면 클릭 하십시오. "

     ![복제된 항목 창](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)
2. 알림을 선택 하 여 VM 선택 페이지를 엽니다.
3. 업그레이드할 Vm을 선택 하 고 **확인**을 선택 합니다. 선택한 각 VM에 대해 모바일 서비스 업데이트를 시작 합니다.

     ![복제된 항목 VM 목록](./media/vmware-azure-install-mobility-service/update-okpng.png)


## <a name="common-issues-and-troubleshooting"></a>일반적인 문제 및 문제 해결

자동 업데이트에 문제가 있는 경우 자격 증명 모음 대시보드의 **구성 문제** 에 오류 알림이 표시 됩니다.

자동 업데이트를 사용 하도록 설정할 수 없는 경우 다음과 같은 일반적인 오류 및 권장 되는 작업을 참조 하세요.

- **오류**: Azure 실행 계정(서비스 사용자)을 만들고 서비스 사용자에게 기여자 역할을 부여할 권한이 없습니다.

   **권장 조치**: 로그인 된 계정이 참가자로 할당 되었는지 확인 하 고 다시 시도 하세요. 권한 할당에 대 한 자세한 내용은 [포털을 사용 하 여 리소스에 액세스할 수 있는 AZURE AD 응용 프로그램 및 서비스 주체 만들기](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) 의 필수 권한 섹션을 참조 하세요.
 
   자동 업데이트를 사용 하도록 설정한 후 대부분의 문제를 해결 하려면 **복구**를 선택 합니다. 복구 단추를 사용할 수 없는 경우 확장 업데이트 설정 창에 표시 되는 오류 메시지를 참조 하세요.

   ![확장 업데이트 설정의 Site Recovery 서비스 복구 단추](./media/azure-to-azure-autoupdate/repair.png)

- **오류**: 복구 서비스 리소스에 액세스할 수 있는 권한이 실행 계정에 없습니다.

    **권장 작업**: 실행 계정을 삭제 하 고 [다시 만듭니다](https://docs.microsoft.com/azure/automation/automation-create-runas-account). 또는 Automation 실행 계정의 Azure Active Directory 응용 프로그램에 recovery services 리소스에 대 한 액세스 권한이 있는지 확인 합니다.

- **오류**: 실행 계정을 찾을 수 없습니다. Azure Active Directory 애플리케이션, 서비스 사용자, 역할, Automation 인증서 자산, Automation 연결 자산 중 하나가 삭제되었거나 생성되지 않았습니다. 또는 인증서와 연결 사이에서 지문이 일치하지 않습니다. 

    **권장 작업**: 실행 계정을 삭제 하 고 [다시 만듭니다](https://docs.microsoft.com/azure/automation/automation-create-runas-account).

-  **오류**: automation 계정에서 사용 하는 Azure 실행 인증서가 곧 만료 됩니다. 

    실행 계정에 대해 생성 된 자체 서명 된 인증서는 만든 날짜 로부터 1 년 후에 만료 됩니다. 만료되기 전에 언제든지 갱신할 수 있습니다. 전자 메일 알림을 등록 한 경우 사용자 측에서 조치가 필요한 경우에도 전자 메일을 받게 됩니다. 이 오류는 만료 날짜 전에 2 개월 후에 표시 되며, 인증서가 만료 되 면 심각한 오류로 변경 됩니다. 인증서가 만료 된 후에는 동일한을 갱신할 때까지 자동 업데이트가 작동 하지 않습니다.

   **권장 작업**:이 문제를 해결 하려면 ' 복구 '를 클릭 한 다음 ' 인증서 갱신 '을 클릭 하세요.
    
   ![갱신-인증서](media/azure-to-azure-autoupdate/automation-account-renew-runas-certificate.PNG)

> [!NOTE]
> 인증서를 갱신 한 후에는 현재 상태가 업데이트 되도록 페이지를 새로 고 치세요.
