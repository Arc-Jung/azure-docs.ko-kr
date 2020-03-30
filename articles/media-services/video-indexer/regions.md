---
title: Video Indexer를 사용할 수 있는 지역 - Azure
titleSuffix: Azure Media Services
description: 이 문서에서는 Azure 미디어 서비스 비디오 인덱서를 사용할 수 있는 Azure 지역에 대해 다수 있습니다.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: c91b38fcbfb9b517651adead010408425e519a82
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382752"
---
# <a name="azure-regions-in-which-video-indexer-exists"></a>Video Indexer가 있는 Azure 지역

Video Indexer API에는 호출을 라우팅할 Azure 지역으로 설정해야 하는 **location** 매개 변수가 있습니다. 이 매개 변수는 [Video Indexer를 사용할 수 있는 Azure 지역](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)이어야 합니다.

## <a name="locations"></a>위치

**location** 매개 변수에는 해당 값으로 Azure 지역 코드 이름을 지정해야 합니다. Video Indexer를 미리 보기 모드로 사용하는 경우 값으로 *"trial"* 을 사용해야 합니다. 그렇지 않으면, 계정이 있고 호출을 라우팅해야 하는 Azure 지역의 코드 이름을 가져오기 위해 [Azure CLI](/cli/azure)에서 다음 줄을 실행할 수 있습니다.

```azurecli-interactive
az account list-locations
```

일단 위에 표시된 줄을 실행하면 모든 Azure 지역 목록이 표시됩니다. 찾으려는 *displayName*이 있는 Azure 지역으로 이동한 후 **location** 매개 변수에 대해 해당 *name* 값을 사용합니다.

예를 들어 미국 서부 2의 Azure 지역에서(아래에 표시됨)는 **location** 매개 변수에 "westus2"를 사용합니다.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/35c2594a-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>다음 단계

- [API를 사용하여 언어 모델 사용자 지정](customize-language-model-with-api.md)
- [API를 사용하여 브랜드 모델 사용자 지정](customize-brands-model-with-api.md)
- [API를 사용하여 개인 모델 사용자 지정](customize-person-model-with-api.md)
