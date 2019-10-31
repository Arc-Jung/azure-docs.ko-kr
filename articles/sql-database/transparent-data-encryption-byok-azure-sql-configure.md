---
title: 'PowerShell 및 CLI: SQL TDE 사용 Azure Key Vault-사용자 고유의 키 가져오기-Azure SQL Database | Microsoft Docs'
description: PowerShell 또는 CLI를 통해 미사용 데이터 암호화에 TDE(투명한 데이터 암호화)를 사용하도록 Azure SQL Database 및 Data Warehouse를 구성하는 방법을 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: beeb5fa4f979ac457db8a779dd8f8f2e94ef87f5
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73163860"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell 및 CLI: Azure Key Vault에서 고객이 관리 하는 키로 투명한 데이터 암호화를 사용 하도록 설정

이 문서에서는 SQL Database 또는 Data Warehouse에서 TDE(투명한 데이터 암호화)에 대해 Azure Key Vault의 키를 사용하는 방법에 대해 설명합니다. Azure Key Vault 통합으로 TDE - BYOK(Bring Your Own Key) 지원에 대해 자세히 알아보려면 [Azure Key Vault에서 고객 관리형 키로 TDE](transparent-data-encryption-byok-azure-sql.md)를 참조하세요. 

## <a name="prerequisites-for-powershell"></a>PowerShell용 필수 구성 요소

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager 모듈은 Azure SQL Database에서 계속 지원 되지만 모든 향후 개발은 Az. Sql 모듈에 대 한 것입니다. 이러한 cmdlet에 대 한 자세한 내용은 [AzureRM](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)를 참조 하세요. Az module 및 AzureRm 모듈의 명령에 대 한 인수는 실질적으로 동일 합니다.

- Azure 구독 및 해당 구독에 대한 관리자 권한이 있어야 합니다.
- [권장되는 선택 사항] TDE 보호기 키 자료의 로컬 복사본을 만들기 위한 HSM(하드웨어 보안 모듈) 또는 로컬 키 저장소가 있어야 합니다.
- Azure PowerShell 설치 되 고 실행 중 이어야 합니다. 
- TDE에 사용할 Azure Key Vault 및 키를 만듭니다.
  - [PowerShell에 대한 Key Vault 지침](../key-vault/quick-create-powershell.md)
  - [HSM(하드웨어 보안 모듈) 및 Key Vault 사용 지침](../key-vault/key-vault-hsm-protected-keys.md)
    - 키 자격 증명 모음에는 TDE에 사용할 다음 속성이 있어야 합니다.
  - [일시 삭제](../key-vault/key-vault-ovw-soft-delete.md) 및 보호 제거
  - [PowerShell로 Key Vault 일시 삭제를 사용하는 방법](../key-vault/key-vault-soft-delete-powershell.md) 
- 키에는 TDE에 사용할 다음 특성이 있어야 합니다.
   - 만료 날짜 없음
   - 사용 안 함 없음
   - *가져오기*, *키 래핑*, *키 래핑 해제* 작업도 수행 가능

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>1단계. 서버에 Azure AD ID 할당 

기존 서버가 있는 경우 다음과 같이 Azure AD ID를 서버에 추가합니다.

   ```powershell
   $server = Set-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

서버를 만드는 경우에는 [AzSqlServer](/powershell/module/az.sql/new-azsqlserver) cmdlet을 태그 id와 함께 사용 하 여 서버 생성 중에 Azure AD id를 추가 합니다.

   ```powershell
   $server = New-AzSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>2단계. 서버에 Key Vault 권한 부여

TDE에 대해 키를 사용 하기 전에 [AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet을 사용 하 여 서버에 키 자격 증명 모음에 대 한 액세스 권한을 부여 합니다.

   ```powershell
   Set-AzKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3단계. Key Vault 키를 서버에 추가하고 TDE 보호기를 설정합니다


- [AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey?view=azps-2.4.0) cmdlet을 사용 하 여 key vault에서 키 ID를 검색 합니다.
- [AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) cmdlet을 사용 하 여 Key Vault 서버에 키를 추가 합니다.
- [AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet을 사용 하 여 모든 서버 리소스에 대 한 tde 보호기로 키를 설정 합니다.
- [AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet을 사용 하 여 tde 보호기가 의도 한 대로 구성 되었는지 확인 합니다.

> [!Note]
> 키 자격 증명 모음 이름과 키 이름을 결합한 길이는 94자를 초과할 수 없습니다.
> 

>[!Tip]
>Key Vault의 KeyId 예제: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>4단계. TDE 설정 

[AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) cmdlet을 사용 하 여 tde를 설정 합니다.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

이제 데이터베이스 또는 데이터 웨어하우스에서 Key Vault에 있는 암호화 키를 사용하여 TDE를 사용할 수 있도록 설정되었습니다.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5단계. 암호화 상태 및 암호화 작업 확인

[AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) 를 사용 하 여 암호화 상태를 가져오고 [AzSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) 를 사용 하 여 데이터베이스 또는 데이터 웨어하우스의 암호화 진행률을 확인 합니다.

   ```powershell
   # Get the encryption state
   Get-AzSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>다른 유용한 PowerShell cmdlet

- [AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) cmdlet을 사용 하 여 tde를 해제 합니다.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled"
   ```
 
- [AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) cmdlet을 사용 하 여 서버에 추가 된 Key Vault 키 목록을 반환 합니다.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- [AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) 를 사용 하 여 서버에서 Key Vault 키를 제거 합니다.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>문제 해결

문제가 발생하면 다음과 같이 확인합니다.
- 키 자격 증명 모음을 찾을 수 없는 경우 [AzSubscription](/powershell/module/az.accounts/get-azsubscription) cmdlet을 사용 하 여 올바른 구독에 있는지 확인 하세요.

   ```powershell
   Get-AzSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- 새 키를 서버에 추가할 수 없거나 TDE 보호기로 업데이트할 수 없으면 다음을 확인합니다.
   - 키의 만료 날짜가 없어야 합니다.
   - 키에는 사용하도록 설정된 *가져오기*, *키 래핑* 및 *키 래핑 해제* 작업이 있어야 합니다.

## <a name="next-steps"></a>다음 단계

- 보안 요구 사항에 맞게 서버의 TDE 보호기를 회전하는 방법을 알아봅니다([PowerShell을 사용하여 투명한 데이터 암호화 보호기 회전](transparent-data-encryption-byok-azure-sql-key-rotation.md)).
- 보안 위험이 있는 경우 잠재적으로 손상될 수 있는 TDE 보호기를 제거하는 방법을 알아봅니다([잠재적으로 손상될 수 있는 키 제거](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)). 

## <a name="prerequisites-for-cli"></a>CLI용 필수 구성 요소

- Azure 구독 및 해당 구독에 대한 관리자 권한이 있어야 합니다.
- [권장되는 선택 사항] TDE 보호기 키 자료의 로컬 복사본을 만들기 위한 HSM(하드웨어 보안 모듈) 또는 로컬 키 저장소가 있어야 합니다.
- 명령줄 인터페이스 버전 2.0 이상. 최신 버전을 설치하고 Azure 구독에 연결하려면 [Azure 플랫폼 간 명령줄 인터페이스 2.0 설치 및 구성](https://docs.microsoft.com/cli/azure/install-azure-cli)을 참조하세요. 
- TDE에 사용할 Azure Key Vault 및 키를 만듭니다.
  - [CLI 2.0을 사용하여 Key Vault 관리](../key-vault/key-vault-manage-with-cli2.md)
  - [HSM(하드웨어 보안 모듈) 및 Key Vault 사용 지침](../key-vault/key-vault-hsm-protected-keys.md)
    - 키 자격 증명 모음에는 TDE에 사용할 다음 속성이 있어야 합니다.
  - [일시 삭제](../key-vault/key-vault-ovw-soft-delete.md)
  - [CLI로 Key Vault 일시 삭제를 사용하는 방법](../key-vault/key-vault-soft-delete-cli.md) 
- 키에는 TDE에 사용할 다음 특성이 있어야 합니다.
   - 만료 날짜 없음
   - 사용 안 함 없음
   - *가져오기*, *키 래핑*, *키 래핑 해제* 작업도 수행 가능
   
## <a name="step-1-create-a-server-with-an-azure-ad-identity"></a>1단계. Azure AD ID를 사용하여 서버 만들기
      cli
      # create server (with identity) and database
      az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
      az sql db create --name <dbname> --server <servername> --resource-group <rgname>  
 
 
>[!Tip]
>서버를 만드는 과정에서 얻은 “principalID”를 보관합니다. 이 ID는 다음 단계에서 키 자격 증명 모음 권한을 할당하는 데 사용되는 개체 ID입니다.
>
 
## <a name="step-2-grant-key-vault-permissions-to-the-logical-sql-server"></a>2단계. 논리적 SQL Server에 Key Vault 권한 부여
      cli
      # create key vault, key and grant permission
       az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
       az keyvault key create --name <keyname> --vault-name <kvname> --protection software
       az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get 


>[!Tip]
>다음 단계를 위해 새 키의 키 URI 또는 keyID를 보관합니다(예: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h ).
>
 
       
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>3단계. Key Vault 키를 서버에 추가하고 TDE 보호기를 설정합니다
  
     cli
     # add server key and update encryption protector
     az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
     az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>

        
  > [!Note]
> 키 자격 증명 모음 이름과 키 이름을 결합한 길이는 94자를 초과할 수 없습니다.
> 

  
## <a name="step-4-turn-on-tde"></a>4단계. TDE 설정 
      cli
      # enable encryption
      az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled 
      

이제 데이터베이스 또는 데이터 웨어하우스에서 Azure Key Vault에 있는 고객 관리형 암호화 키로 TDE를 사용할 수 있습니다.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>5단계. 암호화 상태 및 암호화 작업 확인

     cli
      # get encryption scan progress
      az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

      # get whether encryption is on or off
      az sql db tde show --database <dbname> --server <servername> --resource-group <rgname> 

## <a name="sql-cli-references"></a>SQL CLI 참조

https://docs.microsoft.com/cli/azure/sql 

https://docs.microsoft.com/cli/azure/sql/server/key 

https://docs.microsoft.com/cli/azure/sql/server/tde-key 

https://docs.microsoft.com/cli/azure/sql/db/tde 

