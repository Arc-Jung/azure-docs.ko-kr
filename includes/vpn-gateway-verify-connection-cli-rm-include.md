---
title: 파일 포함
description: 파일 포함
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4e958026b1167d65f47bddbe5e89ec4d6fed7ee3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92217965"
---
[az network vpn-connection show](/cli/azure/network/vpn-connection) 명령을 사용하여 연결이 성공했는지 확인할 수 있습니다. 예제에서  --name은 테스트하려는 연결의 이름을 의미합니다. 연결이 계속 설정 중이면 연결 상태가 '연결 중'으로 표시됩니다. 연결이 설정되면 상태가 '연결됨'으로 변경됩니다.

```azurecli-interactive
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
