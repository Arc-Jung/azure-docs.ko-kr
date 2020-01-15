---
title: 동일한 구독에 있는 스토리지 계정의 VHD 파일에서 관리 디스크 만들기 - CLI 샘플
description: Azure CLI 스크립트 샘플 - 동일한 구독에 있는 스토리지 계정의 VHD 파일에서 관리 디스크 만들기
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 9f7126d98b5b769ecf87608861b43af8af9dcaa4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75375834"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>CLI를 사용하여 동일한 구독에 있는 스토리지 계정의 VHD 파일에서 관리 디스크 만들기

이 스크립트는 동일한 구독에 있는 스토리지 계정의 VHD 파일에서 관리 디스크를 만듭니다. 이 스크립트를 사용하여 관리 OS 디스크에 특수화된(일반화/sysprep되지 않음) VHD를 가져와 가상 머신을 만듭니다 또는 데이터 VHD를 관리되는 데이터 디스크로 가져오는 데 사용합니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트에서는 다음 명령을 사용하여 VHD에서 관리 디스크를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az disk create](https://docs.microsoft.com/cli/azure/disk) | 동일한 구독에 있는 스토리지 계정의 VHD URI를 사용하여 관리 디스크 만들기 |

## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 가상 머신 및 관리 디스크 CLI 스크립트 샘플은 [Azure Windows VM 설명서](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)에서 확인할 수 있습니다.
