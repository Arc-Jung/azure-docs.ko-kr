---
title: Azure CLI 스크립트 샘플 - 클러스터링이 있는 프리미엄 Azure Cache for Redis 만들기
description: Azure CLI 스크립트 샘플 - 클러스터링이 있는 프리미엄 계층 Azure Cache for Redis를 만듭니다.
author: yegu-ms
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: 1813bf7112969436012627147b94f656537029d5
ms.sourcegitcommit: 5a8c65d7420daee9667660d560be9d77fa93e9c9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74122496"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>클러스터링이 있는 프리미엄 Azure Cache for Redis 만들기

이 시나리오에서는 사용하도록 설정된 클러스터링 및 두 개의 분할이 있는 6GB 프리미엄 계층 Azure Cache for Redis를 만드는 방법을 알아봅니다.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 리소스 그룹 및 사용하도록 설정된 클러스터링이 있는 프리미엄 계층 Azure Cache for Redis를 만듭니다. 테이블에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 모든 리소스가 저장되는 리소스 그룹을 만듭니다. |
| [az redis create](https://docs.microsoft.com/cli/azure/redis) | Azure Cache for Redis 인스턴스를 만듭니다. |


## <a name="next-steps"></a>다음 단계

Azure CLI에 대한 자세한 내용은 [Azure CLI 설명서](https://docs.microsoft.com/cli/azure)를 참조하세요.

추가 Azure Cache for Redis CLI 스크립트 샘플은 [Azure Cache for Redis 설명서](../cli-samples.md)에서 찾을 수 있습니다.
