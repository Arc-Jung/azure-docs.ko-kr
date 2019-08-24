---
title: Azure CLI 스크립트 샘플 - 스토리지 계정 액세스 키 회전 | Microsoft Docs
description: Azure Storage 계정을 만들고 나서 해당 계정 액세스 키를 검색 및 회전합니다.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: ac58886225221677aa003833167ff58cd578255d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693924"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>스토리지 계정 만들기 및 계정 액세스 키 회전

이 스크립트는 Azure Storage 계정을 만들고, 새 스토리지 계정의 액세스 키를 표시하고 나서, 키를 갱신(회전)합니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>배포 정리 

다음 명령을 실행하여 리소스 그룹, 스토리지 계정 및 모든 관련된 리소스를 제거합니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 스토리지 계정을 만들고 해당 액세스 키를 검색 및 회전합니다. 표에 있는 각 항목은 명령 관련 설명서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az storage account create](/cli/azure/storage/account) | 지정된 리소스 그룹에서 Azure Storage 계정을 만듭니다. |
| [az storage account keys list](/cli/azure/storage/account/keys) | 지정된 계정에 대한 스토리지 액세스 키를 표시합니다. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | 기본 또는 보조 스토리지 계정 액세스 키를 다시 생성합니다. |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure)를 참조하세요.

추가 스토리지 CLI 스크립트 샘플은 [Azure Blob Storage에 대한 Azure CLI 샘플](../blobs/storage-samples-blobs-cli.md)에서 찾을 수 있습니다.
