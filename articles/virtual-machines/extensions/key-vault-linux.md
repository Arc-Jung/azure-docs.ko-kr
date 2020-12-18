---
title: Linux용 Azure Key Vault VM 확장
description: 가상 머신 확장을 사용하여 가상 머신에 Key Vault 인증서의 자동 새로 고침을 수행하는 에이전트를 배포합니다.
services: virtual-machines-linux
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines-linux
ms.subservice: extensions
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.openlocfilehash: c9b624a1efc72bebec8547e8ecf9f3bf9fc99863
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97680647"
---
# <a name="key-vault-virtual-machine-extension-for-linux"></a>Linux용 Key Vault 가상 머신 확장

Key Vault VM 확장은 Azure Key Vault에 저장된 인증서의 자동 새로 고침을 제공합니다. 특히 확장은 Key Vault에 저장되어 있는 관찰된 인증서 목록을 모니터링합니다.  변경 사항을 감지하면 확장이 검색을 하고 해당 인증서를 설치합니다. Microsoft는 현재 Linux VM에 대한 Key Vault VM 확장을 게시하고 지원합니다. 이 문서에서는 Linux용 Key Vault VM 확장에 지원되는 플랫폼, 구성 및 배포 옵션에 대해 자세히 설명합니다. 

### <a name="operating-system"></a>운영 체제

Key Vault VM 확장은 다음 Linux 배포를 지원합니다.

- Ubuntu-1604
- Ubuntu-1804
- Debian-9
- Suse-15 

### <a name="supported-certificate-content-types"></a>지원되는 인증서 콘텐츠 형식

- PKCS #12
- PEM

## <a name="prerequisities"></a>필수 구성 요소
  - 인증서를 사용 하 여 인스턴스를 Key Vault 합니다. [Key Vault 만들기를](../../key-vault/general/quick-create-portal.md) 참조 하세요.
  - VM/VMSS에서 [관리 id](../../active-directory/managed-identities-azure-resources/overview.md) 를 할당 해야 함
  - `get` `list` 암호의 인증서 부분을 검색 하려면 VM/vmss 관리 id에 대 한 암호 및 사용 권한을 Key Vault 액세스 정책을 설정 해야 합니다. [Key Vault에 인증](../../key-vault/general/authentication.md) 하 고 [Key Vault 액세스 정책을 할당](../../key-vault/general/assign-access-policy-cli.md)하는 방법을 참조 하세요.

## <a name="extension-schema"></a>확장 스키마

다음 JSON은 Key Vault VM 확장에 대한 스키마를 보여 줍니다. 확장에는 protected 설정이 필요하지 않습니다. 모든 설정은 보안의 영향을 받지 않는 정보로 간주됩니다. 확장에는 모니터링되는 비밀 목록, 폴링 빈도 및 대상 인증서 저장소가 필요합니다. 특히 다음에 대한 내용을 설명합니다.  
```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <It is ignored on Linux>,
          "linkOnRenewal": <Not available on Linux e.g.: false>,
          "certificateStoreLocation": <disk path where certificate is stored, default: "/var/lib/waagent/Microsoft.Azure.KeyVault">,
          "requireInitialSync": <initial synchronization of certificates e..g: true>,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: ["https://myvault.vault.azure.net/secrets/mycertificate", "https://myvault.vault.azure.net/secrets/mycertificate2"]>
        },
        "authenticationSettings": {
                "msiEndpoint":  <Optional MSI endpoint e.g.: "http://169.254.169.254/metadata/identity">,
                "msiClientId":  <Optional MSI identity e.g.: "c7373ae5-91c2-4165-8ab6-7381d6e75619">
        }
       }
      }
    }
```

> [!NOTE]
> 관찰된 인증서 URL은 `https://myVaultName.vault.azure.net/secrets/myCertName` 형식이어야 합니다.
> 
> `/secrets` 경로가 프라이빗 키를 포함하여 전체 인증서를 반환하지만 `/certificates` 경로에서는 반환하지 않기 때문입니다. 인증서에 대한 자세한 내용은 여기에서 찾을 수 있습니다. [Key Vault 인증서](../../key-vault/general/about-keys-secrets-certificates.md)

> [!IMPORTANT]
> ' AuthenticationSettings ' 속성은 **사용자가 할당 한 id** 를 가진 vm에만 **필요** 합니다.
> Key Vault에 대 한 인증에 사용할 id를 지정 합니다.


### <a name="property-values"></a>속성 값

| 속성 | 값/예제 | 데이터 형식 |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | date |
| publisher | Microsoft.Azure.KeyVault | 문자열 |
| type | KeyVaultForLinux | 문자열 |
| typeHandlerVersion | 1.0 | int |
| pollingIntervalInS | 3600 | 문자열 |
| certificateStoreName | Linux에서 무시 됩니다. | 문자열 |
| linkOnRenewal | false | boolean |
| certificateStoreLocation  | /var/lib/waagent/Microsoft.Azure.KeyVault | 문자열 |
| requireInitialSync | true | boolean |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate", "https://myvault.vault.azure.net/secrets/mycertificate2"] | 문자열 배열
| msiEndpoint | http://169.254.169.254/metadata/identity | 문자열 |
| msiClientId | c7373ae5-91c2-4165-8ab6-7381d6e75619 | 문자열 |


## <a name="template-deployment"></a>템플릿 배포

Azure Resource Manager 템플릿을 사용하여 Azure VM 확장을 배포할 수 있습니다. 배포 후에 인증서를 새로 고칠 필요가 있는 하나 이상의 가상 머신을 배포하는 경우 템플릿을 사용하는 것이 좋습니다. 확장은 개별 VM 또는 가상 머신 확장 집합에 배포할 수 있습니다. 스키마와 구성은 두 템플릿 형식 모두에 공통적으로 적용됩니다. 

가상 머신 확장에 대한 JSON 구성은 템플릿의 가상 머신 리소스 조각, 특히 가상 머신 템플릿의 `"resources": []` 개체 및 `"virtualMachineProfile":"extensionProfile":{"extensions" :[]` 개체의 가상 머신 확장 집합 내에 중첩되어야 합니다.

 > [!NOTE]
> VM 확장을 사용 하려면 키 자격 증명 모음에 인증 하기 위해 시스템 또는 사용자 관리 id를 할당 해야 합니다.  [Key Vault에 인증 하 고 Key Vault 액세스 정책을 할당 하는 방법](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md) 을 참조 하세요.
> 

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "1.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
          "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <ingnored on linux>,
          "certificateStoreLocation": <disk path where certificate is stored, default: "/var/lib/waagent/Microsoft.Azure.KeyVault">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```


## <a name="azure-powershell-deployment"></a>Azure PowerShell 배포
> [!WARNING]
> PowerShell 클라이언트는 `\` settings.js에를 추가 하 여 `"` akvvm_service 오류로 인해 실패 하는 경우가 많습니다. `[CertificateManagementConfiguration] Failed to parse the configuration settings with:not an object.`

Azure PowerShell은 기존 가상 머신 또는 가상 머신 확장 집합에 Key Vault VM 확장을 배포하는 데 사용할 수 있습니다. 

* VM에 확장을 배포하려면 다음과 같습니다.
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCert1> + '","' + <observedCert2> + '"] } }'
        $extName =  "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "1.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* 가상 머신 확장 집합에 확장 배포:

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCert1> + '","' + <observedCert2> + '"] } }'
        $extName = "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "1.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Azure CLI 배포

Azure CLI는 기존 가상 머신 또는 가상 머신 확장 집합에 Key Vault VM 확장을 배포하는 데 사용할 수 있습니다. 
 
* VM에 확장을 배포하려면 다음과 같습니다.
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForLinux" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\" <observedCert1> \", \" <observedCert2> \"] }}'
    ```

* 가상 머신 확장 집합에 확장 배포:

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForLinux" `
        --publisher Microsoft.Azure.KeyVault `
        -g "<resourcegroup>" `
        --vm-name "<vmName>" `
        --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\" <observedCert1> \", \" <observedCert2> \"] }}'
    ```
다음 제한 사항/요구 사항에 주의하세요.
- Key Vault 제한 사항:
  - 배포 시점에 있어야 합니다. 
  - 관리 Id를 사용 하 여 VM/VMSS Id에 대 한 Key Vault 액세스 정책을 설정 해야 합니다. [Key Vault에 인증](../../key-vault/general/authentication.md) 하 고 [Key Vault 액세스 정책을 할당](../../key-vault/general/assign-access-policy-cli.md)하는 방법을 참조 하세요.

### <a name="frequently-asked-questions"></a>질문과 대답

* 설정할 수 있는 observedCertificates 수에 제한이 있나요?
  아니요, Key Vault VM 확장은 observedCertificates 수에 제한이 없습니다.


### <a name="troubleshoot"></a>문제 해결

확장 배포 상태에 대한 데이터는 Azure PowerShell 또는 Azure Portal을 통해 검색할 수 있습니다. 지정된 VM에 대한 확장의 배포 상태를 보려면 Azure PowerShell을 사용하여 다음 명령을 실행합니다.

**Azure PowerShell**
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

**Azure CLI**
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```
#### <a name="logs-and-configuration"></a>로그 및 구성

```
/var/log/waagent.log
/var/log/azure/Microsoft.Azure.KeyVault.KeyVaultForLinux/*
/var/lib/waagent/Microsoft.Azure.KeyVault.KeyVaultForLinux-<most recent version>/config/*
```
### <a name="using-symlink"></a>Symlink 사용

기호화 된 링크 또는 Symlink는 기본적으로 고급 바로 가기입니다. 폴더 모니터링을 방지 하 고 최신 인증서를 자동으로 얻으려면이 symlink를 사용 하 여 `([VaultName].[CertificateName])` Linux에서 최신 버전의 인증서를 가져올 수 있습니다.

### <a name="frequently-asked-questions"></a>질문과 대답

* 설정할 수 있는 observedCertificates 수에 제한이 있나요?
  아니요, Key Vault VM 확장은 observedCertificates 수에 제한이 없습니다.

### <a name="support"></a>지원

이 문서의 어디에서든 도움이 필요한 경우 [MSDN Azure 및 Stack Overflow 포럼](https://azure.microsoft.com/support/forums/)에서 Azure 전문가에게 문의할 수 있습니다. 또는 Azure 기술 지원 인시던트를 제출할 수 있습니다. [Azure 지원 사이트](https://azure.microsoft.com/support/options/)로 가서 지원 받기를 선택합니다. Azure 지원을 사용하는 방법에 대한 자세한 내용은 [Microsoft Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)를 참조하세요.
