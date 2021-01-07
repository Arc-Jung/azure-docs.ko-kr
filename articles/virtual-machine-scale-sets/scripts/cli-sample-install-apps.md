---
title: Azure CLI 샘플 - 앱 설치
description: 이 스크립트는 Ubuntu를 실행하는 가상 머신 확장 집합을 만들고, 사용자 지정 스크립트 확장을 사용하여 기본 웹 애플리케이션을 설치합니다.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: cli
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 78569459de9d6a632f256d25ffb0870e36ce4a79
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87499690"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI를 사용하여 가상 머신 확장 집합에 애플리케이션 설치
이 스크립트는 Ubuntu를 실행하는 가상 머신 확장 집합을 만들고, 사용자 지정 스크립트 확장을 사용하여 기본 웹 애플리케이션을 설치합니다. 스크립트가 실행되면 웹 브라우저를 통해 웹 응용 프로그램에 액세스할 수 있습니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/install-apps/install-apps.sh "Install apps into a scale set")]

## <a name="clean-up-deployment"></a>배포 정리
다음 명령을 실행하여 리소스 그룹, 확장 집합 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>스크립트 설명
이 스크립트는 다음 명령을 사용하여 리소스 그룹, 가상 머신 확장 집합 및 관련된 모든 리소스를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](/cli/azure/ad/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az vmss create](/cli/azure/vmss) | 가상 머신 확장 집합을 만들고 가상 네트워크, 서브넷 및 네트워크 보안 그룹에 연결합니다. 부하 분산 장치도 여러 VM 인스턴스에 트래픽을 분산하기 위해 만듭니다. 또한 이 명령은 사용할 VM 이미지와 관리 자격 증명도 지정합니다.  |
| [az vmss extension set](/cli/azure/vmss/extension) | Azure 사용자 지정 스크립트 확장을 설치하여 각 VM 인스턴스에서 데이터 디스크를 준비하는 스크립트를 실행합니다. |
| [az network lb rule create](/cli/azure/network/lb/rule) | 80 TCP 포트의 트래픽을 확장 집합의 VM 인스턴스에 분산하는 부하 분산 장치 규칙을 만듭니다. |
| [az network public-ip show](/cli/azure/network/public-ip) | 부하 분산 장치에서 사용하는 할당된 공용 IP 주소에 대한 정보를 가져옵니다. |
| [az group delete](/cli/azure/ad/group) | 모든 중첩 리소스를 포함한 리소스 그룹을 삭제합니다. |

## <a name="next-steps"></a>다음 단계
Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](/cli/azure/overview)를 참조하세요.
