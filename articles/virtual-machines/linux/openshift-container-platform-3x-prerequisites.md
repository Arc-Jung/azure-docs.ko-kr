---
title: Azure 필수 구성 요소에서 OpenShift Container Platform 3.11
description: Azure에서 OpenShift Container Platform 3.11을 배포 하기 위한 필수 구성 요소입니다.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/23/2019
ms.author: haroldw
ms.openlocfilehash: 069561c4bed55bf6021b594d693e076ef8d313bd
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2019
ms.locfileid: "74035467"
---
# <a name="common-prerequisites-for-deploying-openshift-container-platform-311-in-azure"></a>Azure에서 OpenShift Container Platform 3.11을 배포 하기 위한 일반적인 필수 구성 요소

이 문서는 Azure에서 OpenShift Container Platform 또는 OKD를 배포하기 위한 일반적인 필수 조건을 설명합니다.

OpenShift 설치에는 Ansible 플레이북이 사용됩니다. Ansible은 SSH(Secure Shell)를 사용하여 설치 단계를 완료하는 모든 클러스터 호스트에 연결합니다.

Ansible가 원격 호스트에 대 한 SSH 연결을 설정 하면 암호를 입력할 수 없습니다. 따라서 프라이빗 키에는 연결된 암호를 사용할 수 없으며, 사용할 경우 배포에 실패합니다.

VM(가상 머신)은 Azure Resource Manager 템플릿을 통해 배포되기 때문에 동일한 공개 키가 모든 VM에 액세스하는 데 사용됩니다. 모든 플레이 북을 실행 하는 VM에 해당 개인 키가 있어야 합니다. 이 작업을 안전 하 게 수행 하기 위해 Azure key vault는 개인 키를 VM에 전달 하는 데 사용 됩니다.

컨테이너에 대한 영구 스토리지가 필요한 경우에는 영구적 볼륨이 필요합니다. OpenShift는 영구 볼륨에 대해 Azure Vhd (가상 하드 디스크)를 지원 하지만 먼저 Azure를 클라우드 공급자로 구성 해야 합니다.

이 모델에서 OpenShift는 다음을 수행합니다.

- Azure storage 계정 또는 관리 디스크에 VHD 개체를 만듭니다.
- VM에 VHD를 탑재하고 볼륨을 포맷합니다.
- Pod에 볼륨을 탑재합니다.

이 구성이 작동하려면 OpenShift에 Azure에서 이러한 작업을 수행할 수 있는 권한이 필요합니다. 이 목적을 위해 서비스 주체가 사용 됩니다. SP(서비스 주체)는 리소스에 대한 권한이 부여된 Azure Active Directory의 보안 계정입니다.

서비스 주체는 클러스터를 구성하는 VM 및 스토리지 계정에 대한 액세스 권한이 있어야 합니다. 모든 OpenShift 클러스터 리소스가 단일 리소스 그룹에 배포된 경우 서비스 주체는 해당 리소스 그룹에 대한 권한을 부여 받을 수 있습니다.

이 가이드에서는 필수 조건과 연결된 아티팩트를 만드는 방법을 설명합니다.

> [!div class="checklist"]
> * OpenShift 클러스터에 대한 SSH 키를 관리하는 키 자격 증명 모음을 만듭니다.
> * Azure 클라우드 공급자가 사용할 서비스 주체를 만듭니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 을 만듭니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인 
[az login](/cli/azure/reference-index) 명령으로 Azure 구독에 로그인하고 화면의 지시를 따르거나 **시도**를 클릭하여 Cloud Shell을 사용합니다.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>리소스 그룹 만들기

[az group create](/cli/azure/group) 명령을 사용하여 리소스 그룹을 만듭니다. Azure 리소스 그룹은 Azure 리소스가 배포 및 관리되는 논리적 컨테이너입니다. 키 자격 증명 모음을 호스트 하려면 전용 리소스 그룹을 사용 해야 합니다. 이 그룹은 OpenShift 클러스터 리소스가 배포되는 리소스 그룹과는 다릅니다.

다음 예제에서는 *eastus* 위치에 *keyvaultrg*이라는 리소스 그룹을 만듭니다.

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>키 자격 증명 모음 만들기
[az keyvault create](/cli/azure/keyvault) 명령을 사용하여 클러스터에 대한 SSH 키를 저장할 키 자격 증명 모음을 만듭니다. 키 자격 증명 모음 이름은 전역적으로 고유 해야 하며 템플릿 배포를 사용 하도록 설정 해야 합니다. 그렇지 않으면 "KeyVaultParameterReferenceSecretRetrieveFailed" 오류로 인해 배포가 실패 합니다.

다음 예제에서는 *keyvaultrg* 리소스 그룹에 *keyvault*라는 키 자격 증명 모음을 만듭니다.

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH 키 만들기 
SSH 키는 OpenShift 클러스터에 대한 액세스를 보호하기 위해 필요합니다. `ssh-keygen` 명령(Linux 또는 macOS에서)을 사용하여 SSH 키 쌍을 만듭니다.
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> SSH 키 쌍에는 암호를 포함할 수 없습니다.

Windows의 SSH 키에 대한 자세한 내용은 [Windows에서 SSH 키를 만드는 방법](/azure/virtual-machines/linux/ssh-from-windows)을 참조하세요. OpenSSH 형식으로 프라이빗 키를 내보내야 합니다.

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>Azure Key Vault에 SSH 프라이빗 키 저장
OpenShift 배포는 사용자가 만든 SSH 키를 사용하여 OpenShift 마스터에 대한 액세스를 보호합니다. SSH 키를 안전하게 검색하기 위해 배포를 사용하도록 설정하려면 다음 명령을 사용하여 Key Vault에 키를 저장합니다.

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>서비스 주체 만들기 
OpenShift는 사용자 이름 및 암호 또는 서비스 주체를 사용하여 Azure와 통신합니다. Azure 서비스 주체는 앱, 서비스 및 OpenShift와 같은 자동화 도구를 사용할 수 있는 보안 ID입니다. 서비스 주체가 Azure에서 수행할 수 있는 작업에 대한 권한은 사용자가 제어하고 정의합니다. 서비스 사용자의 사용 권한을 전체 구독이 아닌 특정 리소스 그룹으로 범위를 지정 하는 것이 가장 좋습니다.

[az ad sp create-for-rbac](/cli/azure/ad/sp)를 사용하여 서비스 주체를 만들고 OpenShift가 필요로 하는 자격 증명을 출력합니다.

다음 예제에서는 서비스 주체를 만들고 openshiftrg라는 리소스 그룹에 대한 contributor 권한을 할당합니다.

먼저 openshiftrg라는 리소스 그룹을 만듭니다.

```azurecli
az group create -l eastus -n openshiftrg
```

서비스 주체 만들기:

```azurecli
az group show --name openshiftrg --query id
```
명령 출력을 저장 하 고 다음 명령에서 $scope 대신 사용 합니다.

```azurecli
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --scopes $scope \
```

명령에서 반환 된 appId 속성 및 암호를 기록해 둡니다.
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > 이 암호를 다시 검색할 수 없게 되므로 보안 암호를 기록해 두어야 합니다.

서비스 주체에 대한 자세한 내용은 [Azure CLI를 사용하여 Azure 서비스 주체 만들기](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)를 참조하세요.

## <a name="prerequisites-applicable-only-to-resource-manager-template"></a>리소스 관리자 템플릿에만 적용 되는 필수 구성 요소

**SshPrivateKey**(SSH 개인 키), Azure AD 클라이언트 암호 (**AadClientSecret**), Openshift admin password (**OpenshiftPassword**) 및 Red Hat Subscription Manager 암호 또는**rhsmPasswordOrActivationKey**(활성화 키)에 대 한 암호를 만들어야 합니다.  또한 사용자 지정 SSL 인증서를 사용 하는 경우에는 **routingcafile**, **routingcertfile**, **routingkeyfile**, **mastercafile**, **mastercertfile**및 **masterkeyfile**의 6 가지 추가 암호를 만들어야 합니다.  이러한 매개 변수에 대해서는 자세히 설명 합니다.

템플릿은 특정 비밀 이름을 참조 하므로 위에 나열 된 굵게 표시 된 이름을 사용 **해야** 합니다 (대/소문자 구분).

### <a name="custom-certificates"></a>사용자 지정 인증서

기본적으로 템플릿은 OpenShift 웹 콘솔과 라우팅 도메인에 대 한 자체 서명 된 인증서를 사용 하 여 OpenShift 클러스터를 배포 합니다. 사용자 지정 SSL 인증서를 사용 하려면 ' routingCertType '을 ' custom '으로, ' masterCertType '을 ' custom '으로 설정 합니다.  인증서에 대 한 pem 형식의 CA, 인증서 및 키 파일이 필요 합니다.  하나에 대해서는 사용자 지정 인증서를 사용할 수 있지만 다른 인증서는 사용할 수 없습니다.

이러한 파일을 Key Vault 비밀에 저장 해야 합니다.  개인 키에 사용 되는 것과 동일한 Key Vault를 사용 합니다.  비밀 이름에 대해 6 개의 추가 입력이 필요 하지 않고, 각 SSL 인증서 파일에 대해 특정 비밀 이름을 사용 하도록 템플릿이 하드 코딩 됩니다.  다음 표의 정보를 사용 하 여 인증서 데이터를 저장 합니다.

| 비밀 이름      | 인증서 파일   |
|------------------|--------------------|
| mastercafile     | 마스터 CA 파일     |
| mastercertfile   | 마스터 인증서 파일   |
| masterkeyfile    | 마스터 키 파일    |
| routingcafile    | 라우팅 CA 파일    |
| routingcertfile  | 라우팅 인증서 파일  |
| routingkeyfile   | 라우팅 키 파일   |

Azure CLI를 사용 하 여 비밀을 만듭니다. 예를 들면 다음과 같습니다.

```bash
az keyvault secret set --vault-name KeyVaultName -n mastercafile --file ~/certificates/masterca.pem
```

## <a name="next-steps"></a>다음 단계

이 문서는 다음 항목을 설명합니다.
> [!div class="checklist"]
> * OpenShift 클러스터에 대한 SSH 키를 관리하는 키 자격 증명 모음을 만듭니다.
> * Azure 클라우드 솔루션 공급자가 사용할 서비스 주체를 만듭니다.

다음으로 OpenShift 클러스터를 배포합니다.

- [OpenShift Container Platform 배포](./openshift-container-platform-3x.md)
- [OpenShift Container Platform 자체 관리 Marketplace 제품 배포](./openshift-container-platform-3x-marketplace-self-managed.md)
