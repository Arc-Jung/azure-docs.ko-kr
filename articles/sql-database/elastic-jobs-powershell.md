---
title: PowerShell을 사용하여 Azure SQL Database 탄력적 작업 에이전트 만들기 | Microsoft Docs
description: PowerShell을 사용하여 탄력적 작업 에이전트를 만드는 방법을 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: johnpaulkee
ms.author: joke
ms.reviwer: sstein
ms.date: 03/13/2019
ms.openlocfilehash: 0d64bd150a43666679253f8244d80411e25dfdcd
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68935052"
---
# <a name="create-an-elastic-job-agent-using-powershell"></a>PowerShell을 사용하여 탄력적 작업 에이전트 만들기

[탄력적 작업](sql-database-job-automation-overview.md#elastic-database-jobs-preview)을 사용하면 여러 데이터페이스에 병렬적으로 하나 이상의 T-SQL(Transact-SQL) 스크립트를 실행할 수 있습니다.

이 자습서에서는 여러 데이터베이스에서 쿼리를 실행하는 데 필요한 단계를 알아봅니다.

> [!div class="checklist"]
> * 탄력적 작업 에이전트 만들기
> * 작업이 해당 대상에서 스크립트를 실행할 수 있도록 작업 자격 증명 만들기
> * 작업을 실행하려는 대상(서버, 탄력적 풀, 데이터베이스, 분할 데이터베이스 맵) 정의
> * 에이전트가 작업을 연결하고 실행하도록 대상 데이터베이스에서 데이터베이스 범위 자격 증명 만들기
> * 작업 만들기
> * 작업에 작업 단계 추가
> * 작업 실행 시작
> * 작업 모니터링

## <a name="prerequisites"></a>필수 조건

업그레이드된 버전의 Elastic Database 작업에는 마이그레이션 중에 사용할 새로운 PowerShell cmdlet 집합이 포함됩니다. 이 새로운 cmdlet은 모든 기존 작업 자격 증명, 대상(데이터베이스, 서버, 사용자 지정 컬렉션), 작업 트리거, 작업 예약, 작업 콘텐츠 및 작업을 새로운 탄력적 작업 에이전트로 전환합니다.

### <a name="install-the-latest-elastic-jobs-cmdlets"></a>최신 탄력적 작업 cmdlet 설치

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정을 만듭니다](https://azure.microsoft.com/free/).

**Az.Sql** 1.1.1-preview 모듈을 설치하여 최신 탄력적 작업 cmdlet을 가져옵니다. 관리자 권한으로 PowerShell에서 다음 명령을 실행합니다.

```powershell
# Installs the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force

# Installs the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Restart your powershell session with administrative access

# Places Az.Sql preview cmdlets side by side with existing Az.Sql version
Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease

# Import the Az.Sql module
Import-Module Az.Sql -RequiredVersion 1.1.1

# Confirm if module successfully imported - if the imported version is 1.1.1, then continue
Get-Module Az.Sql
```

- **Az.Sql** 1.1.1-preview 모듈 외에도 이 자습서에는 *sqlserver* PowerShell 모듈도 필요합니다. 자세한 내용은 [SQL Server PowerShell 모듈 설치](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module)를 참조하세요.


## <a name="create-required-resources"></a>필수 리소스 만들기

탄력적 작업 에이전트를 만들려면 [작업 데이터베이스](sql-database-job-automation-overview.md#job-database)로 사용할 데이터베이스(S0 이상)가 필요합니다. 

*아래 스크립트에서는 작업 데이터베이스로 사용할 새 리소스 그룹, 서버 및 데이터베이스를 만듭니다. 아래 스크립트에서는 작업을 실행하기 위해 2개의 비어 있는 데이터베이스를 포함한 두 번째 서버도 만듭니다.*

[Azure 요구 사항](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)을 준수하면 원하는 명명 규칙을 사용할 수 있도록 탄력적 작업에는 특정 명명 요구 사항이 포함되지 않습니다.

```powershell
# Sign in to your Azure account
Connect-AzAccount

# Create a resource group
Write-Output "Creating a resource group..."
$ResourceGroupName = Read-Host "Please enter a resource group name"
$Location = Read-Host "Please enter an Azure Region"
$Rg = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
$Rg

# Create a server
Write-Output "Creating a server..."
$AgentServerName = Read-Host "Please enter an agent server name"
$AgentServerName = $AgentServerName + "-" + [guid]::NewGuid()
$AdminLogin = Read-Host "Please enter the server admin name"
$AdminPassword = Read-Host "Please enter the server admin password"
$AdminPasswordSecure = ConvertTo-SecureString -String $AdminPassword -AsPlainText -Force
$AdminCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AdminLogin, $AdminPasswordSecure
$AgentServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $AgentServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set server firewall rules to allow all Azure IPs
Write-Output "Creating a server firewall rule..."
$AgentServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$AgentServer

# Create the job database
Write-Output "Creating a blank SQL database to be used as the Job Database..."
$JobDatabaseName = "JobDatabase"
$JobDatabase = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $JobDatabaseName -RequestedServiceObjectiveName "S0"
$JobDatabase
```

```powershell
# Create a target server and some sample databases - uses the same admin credential as the agent server just for simplicity
Write-Output "Creating target server..."
$TargetServerName = Read-Host "Please enter a target server name"
$TargetServerName = $TargetServerName + "-" + [guid]::NewGuid()
$TargetServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $TargetServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set target server firewall rules to allow all Azure IPs
$TargetServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$TargetServer | New-AzSqlServerFirewallRule -StartIpAddress 0.0.0.0 -EndIpAddress 255.255.255.255 -FirewallRuleName AllowAll
$TargetServer

# Create some sample databases to execute jobs against...
$Db1 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb1"
$Db1
$Db2 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb2"
$Db2
```

## <a name="enable-the-elastic-jobs-preview-for-your-subscription"></a>구독에 대해 탄력적 작업 미리 보기 사용

탄력적 작업을 사용하려면 다음 명령을 실행하여 Azure 구독에서 해당 기능을 등록합니다. 탄력적 작업 에이전트를 프로비저닝하려는 구독에 대해 이 명령을 한 번씩 실행합니다. 작업 대상인 데이터베이스만 포함하는 구독은 등록할 필요가 없습니다.

```powershell
Register-AzProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql
```

## <a name="create-the-elastic-job-agent"></a>탄력적 작업 에이전트 만들기

탄력적 작업 에이전트는 작업을 생성하고 실행하고 관리하기 위한 Azure 리소스입니다. 에이전트는 일정에 따라 또는 일회성 작업으로 작업을 실행합니다.

*ResourceGroupName*, *ServerName* 및  *DatabaseName* 매개 변수가 기존 리소스를 모두 가리켜야 하므로 **New-AzSqlElasticJobAgent** cmdlet에는 Azure SQL 데이터베이스 cmdlet이 있어야 합니다.

```powershell
Write-Output "Creating job agent..."
$AgentName = Read-Host "Please enter a name for your new Elastic Job agent"
$JobAgent = $JobDatabase | New-AzSqlElasticJobAgent -Name $AgentName
$JobAgent
```

## <a name="create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets"></a>작업이 해당 대상에서 스크립트를 실행할 수 있도록 작업 자격 증명 만들기

작업은 데이터베이스 범위 자격 증명을 사용하여 실행 시 대상 그룹에서 지정한 대상 데이터베이스에 연결합니다. 이러한 데이터베이스 범위 자격 증명이 대상 그룹 멤버 형식으로 사용되는 경우 해당 자격 증명을 사용하여 서버 또는 탄력적 풀의 모든 데이터베이스를 열거하기 위해 master 데이터베이스에 연결합니다.

데이터베이스 범위 자격 증명은 작업 데이터베이스에서 만들어져야 합니다.  
모든 대상 데이터베이스는 작업이 성공적으로 완료되게 하려면 충분한 사용 권한으로 로그인해야 합니다.

![탄력적 작업 자격 증명](media/elastic-jobs-overview/job-credentials.png)

이미지의 자격 증명 외에도 다음 스크립트에서 *GRANT* 명령을 추가합니다. 이러한 사용 권한은 이 예제 작업에서 선택한 스크립트에 필요합니다. 예제가 대상 데이터베이스에서 새로운 테이블을 만들기 때문에 각 대상 데이터베이스에는 성공적으로 실행할 적절한 사용 권한이 필요합니다.

작업 데이터베이스에서 필수 작업 자격 증명을 만들려면 다음 스크립트를 실행합니다.

```powershell
# In the master database (target server)
# - Create the master user login
# - Create the master user from master user login
# - Create the job user login
$Params = @{
  'Database' = 'master'
  'ServerInstance' =  $TargetServer.ServerName + '.database.windows.net'
  'Username' = $AdminLogin
  'Password' = $AdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN masteruser WITH PASSWORD='password!123'"
}
Invoke-SqlCmd @Params
$Params.Query = "CREATE USER masteruser FROM LOGIN masteruser"
Invoke-SqlCmd @Params
$Params.Query = "CREATE LOGIN jobuser WITH PASSWORD='password!123'"
Invoke-SqlCmd @Params

# For each of the target databases
# - Create the jobuser from jobuser login
# - Make sure they have the right permissions for successful script execution
$TargetDatabases = @( $Db1.DatabaseName, $Db2.DatabaseName )
$CreateJobUserScript =  "CREATE USER jobuser FROM LOGIN jobuser"
$GrantAlterSchemaScript = "GRANT ALTER ON SCHEMA::dbo TO jobuser"
$GrantCreateScript = "GRANT CREATE TABLE TO jobuser"

$TargetDatabases | % {
  $Params.Database = $_

  $Params.Query = $CreateJobUserScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantAlterSchemaScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantCreateScript
  Invoke-SqlCmd @Params
}

# Create job credential in Job database for master user
Write-Output "Creating job credentials..."
$LoginPasswordSecure = (ConvertTo-SecureString -String "password!123" -AsPlainText -Force)

$MasterCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "masteruser", $LoginPasswordSecure
$MasterCred = $JobAgent | New-AzSqlElasticJobCredential -Name "masteruser" -Credential $MasterCred

$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "jobuser", $LoginPasswordSecure
$JobCred = $JobAgent | New-AzSqlElasticJobCredential -Name "jobuser" -Credential $JobCred
```

## <a name="define-the-target-databases-you-want-to-run-the-job-against"></a>작업을 실행하려는 대상 데이터베이스 정의

[대상 그룹](sql-database-job-automation-overview.md#target-group)은 작업 단계에서 실행될 데이터베이스 중 하나 이상의 집합을 정의합니다. 

다음 코드 조각은 다음과 같은 두 개의 대상 그룹을 만듭니다. *ServerGroup* 및 *ServerGroupExcludingDb2*. *ServerGroup*은 실행 시 서버에 존재하는 모든 데이터베이스를 대상으로 지정하고 *ServerGroupExcludingDb2*는 *TargetDb2*를 제외한 서버의 모든 데이터베이스를 대상으로 지정합니다.

```powershell
Write-Output "Creating test target groups..."
# Create ServerGroup target group
$ServerGroup = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroup'
$ServerGroup | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName

# Create ServerGroup with an exclusion of Db2
$ServerGroupExcludingDb2 = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroupExcludingDb2'
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -Database $Db2.DatabaseName -Exclude
```

## <a name="create-a-job"></a>작업 만들기

```powershell
Write-Output "Creating a new job"
$JobName = "Job1"
$Job = $JobAgent | New-AzSqlElasticJob -Name $JobName -RunOnce
$Job
```

## <a name="create-a-job-step"></a>작업 만들기 단계

이 예제에서는 실행할 작업에 대해 두 개의 작업 단계를 정의합니다. 첫 번째 작업 단계(*step1*)는 대상 그룹 *ServerGroup*의 모든 데이터베이스에서 새로운 테이블(*Step1Table*)을 만듭니다. [이전에 정의된](#define-the-target-databases-you-want-to-run-the-job-against) 대상 그룹이 해당 항목을 제외하도록 지정되었기 때문에 두 번째 작업 단계(*step2*)는 *TargetDb2*의 모든 데이터베이스에서 새로운 테이블(*Step2Table*)을 만듭니다.

```powershell
Write-Output "Creating job steps"
$SqlText1 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step1Table')) CREATE TABLE [dbo].[Step1Table]([TestId] [int] NOT NULL);"
$SqlText2 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step2Table')) CREATE TABLE [dbo].[Step2Table]([TestId] [int] NOT NULL);"

$Job | Add-AzSqlElasticJobStep -Name "step1" -TargetGroupName $ServerGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText1
$Job | Add-AzSqlElasticJobStep -Name "step2" -TargetGroupName $ServerGroupExcludingDb2.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText2
```


## <a name="run-the-job"></a>작업 실행

작업을 즉시 시작하려면 다음 명령을 실행합니다.

```powershell
Write-Output "Start a new execution of the job..."
$JobExecution = $Job | Start-AzSqlElasticJob
$JobExecution
```

성공적으로 완료한 후에 TargetDb1에서 새로운 두 개의 테이블이 표시되고 TargetDb2에서 새로운 하나의 테이블이 표시됩니다.


   ![SSMS에서 새 테이블 유효성 검사](media/elastic-jobs-overview/job-execution-verification.png)




## <a name="monitor-status-of-job-executions"></a>작업 실행 상태 모니터링

다음 코드 조각은 작업 실행 세부 정보를 가져옵니다.

```powershell
# Get the latest 10 executions run
$JobAgent | Get-AzSqlElasticJobExecution -Count 10

# Get the job step execution details
$JobExecution | Get-AzSqlElasticJobStepExecution

# Get the job target execution details
$JobExecution | Get-AzSqlElasticJobTargetExecution -Count 2
```

### <a name="job-execution-states"></a>작업 실행 상태

다음 표에서 가능한 작업 실행 상태를 나열합니다.

|시스템 상태|설명|
|:---|:---|
|**생성일** | 작업 실행이 방금 만들어졌으며 아직 진행 중이 아닙니다.|
|**InProgress** | 작업 실행이 현재 진행 중입니다.|
|**WaitingForRetry** | 작업 실행이 해당 작업을 완료할 수 없어 다시 시도를 기다리고 있습니다.|
|**성공함** | 작업 실행이 성공적으로 완료되었습니다.|
|**SucceededWithSkipped** | 작업 실행이 성공적으로 완료되었지만 자식 중 일부를 건너뛰었습니다.|
|**실패** | 작업 실행이 실패했으며 해당 재시도 횟수를 소진했습니다.|
|**TimedOut** | 작업 실행 시간이 초과되었습니다.|
|**Canceled** | 작업 실행이 취소되었습니다.|
|**생략** | 동일한 작업 단계의 또 다른 실행이 동일한 대상에서 이미 실행 중이므로 작업 실행을 건너뛰었습니다.|
|**WaitingForChildJobExecutions** | 작업 실행에서 해당 자식 실행이 완료되기를 기다리고 있습니다.|

## <a name="schedule-the-job-to-run-later"></a>작업이 나중에 실행되도록 예약

특정 시점에 작업이 실행되도록 예약하려면 다음 명령을 실행합니다.

```powershell
# Run every hour starting from now
$Job | Set-AzSqlElasticJob -IntervalType Hour -IntervalCount 1 -StartTime (Get-Date) -Enable
```

## <a name="clean-up-resources"></a>리소스 정리

리소스 그룹을 삭제하여 이 빠른 시작에서 만든 Azure 리소스를 삭제합니다.

> [!TIP]
> 이러한 작업을 계속 사용하려는 경우 이 아티클에서 만든 리소스를 정리하지 마세요. 계속하지 않으려는 경우 다음 단계를 사용하여 이 아티클에서 만든 모든 리소스를 삭제합니다.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
```


## <a name="next-steps"></a>다음 단계

이 자습서에서 데이터베이스 집합에 대해 Transact-SQL 스크립트를 실행했습니다.  다음 작업을 수행하는 방법을 알아보았습니다.

> [!div class="checklist"]
> * 탄력적 작업 에이전트 만들기
> * 작업이 해당 대상에서 스크립트를 실행할 수 있도록 작업 자격 증명 만들기
> * 작업을 실행하려는 대상(서버, 탄력적 풀, 데이터베이스, 분할 데이터베이스 맵) 정의
> * 에이전트가 작업을 연결하고 실행하도록 대상 데이터베이스에서 데이터베이스 범위 자격 증명 만들기
> * 작업 만들기
> * 작업에 작업 단계 추가
> * 작업 실행 시작
> * 작업 모니터링

> [!div class="nextstepaction"]
>[Transact-SQL을 사용하여 탄력적 작업 관리](elastic-jobs-tsql.md)
