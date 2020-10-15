---
title: Azure CLI 스크립트 예제 - Batch 계정 만들기 - 사용자 구독
description: 이 스크립트는 사용자 구독 모드에서 Batch 계정을 만듭니다. 이 계정은 컴퓨팅 노드를 구독에 할당합니다.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1c60b9c3de5f2370d7bbc557d36db61d23d32ccf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87494403"
---
# <a name="cli-example-create-a-batch-account-in-user-subscription-mode"></a>CLI 예제: 사용자 구독 모드에서 Batch 계정 만들기

이 스크립트는 사용자 구독 모드에서 Batch 계정을 만듭니다. 구독에 컴퓨팅 노드를 할당하는 계정은 Azure Active Directory 토큰을 통해 인증되어야 합니다. 할당된 컴퓨팅 노드는 구독의 vCPU(코어) 할당량에 집계됩니다. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI를 로컬로 설치하여 사용하도록 선택하는 경우 이 문서에서 Azure CLI 버전 2.0.20 이상을 실행해야 합니다. `az --version`을 실행하여 버전을 찾습니다. 설치 또는 업그레이드해야 하는 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요. 

## <a name="example-script"></a>예제 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh "Create Account using user subscription")]

## <a name="clean-up-deployment"></a>배포 정리

다음 명령을 실행하여 리소스 그룹 및 모든 관련 리소스를 제거합니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az role assignment create](/cli/azure/role) | 사용자, 그룹 또는 서비스 사용자에 대한 새 역할 할당을 만듭니다. |
| [az group create](/cli/azure/group#az-group-create) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az keyvault create](/cli/azure/keyvault#az-keyvault-create) | 키 자격 증명 모음을 만듭니다. |
| [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) | 지정된 주요 자격 증명 모음의 보안 정책을 업데이트합니다. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch 계정을 만듭니다.  |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | 추가 CLI 상호 작용을 위해 지정된 Batch 계정에 대해 인증합니다.  |
| [az group delete](/cli/azure/group#az-group-delete) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.
