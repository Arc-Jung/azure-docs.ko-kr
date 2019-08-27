---
title: Azure 네트워킹 리소스를 새 구독 또는 리소스 그룹으로 이동 | Microsoft Docs
description: Azure Resource Manager를 사용 하 여 가상 네트워크 및 기타 네트워킹 리소스를 새 리소스 그룹 또는 구독으로 이동할 수 있습니다.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: 6ad7f32704d8cb73999a6e3cf60cb2a238268242
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034540"
---
# <a name="move-guidance-for-networking-resources"></a>네트워킹 리소스에 대 한 이동 지침

이 문서에서는 특정 시나리오에 대 한 가상 네트워크 및 기타 네트워킹 리소스를 이동 하는 방법을 설명 합니다.

## <a name="dependent-resources"></a>종속 리소스

가상 네트워크를 이동할 때는 해당 종속 리소스도 함께 이동해야 합니다. VPN Gateway의 경우 IP 주소, 가상 네트워크 게이트웨이 및 모든 관련된 연결 리소스를 이동해야 합니다. 로컬 네트워크 게이트웨이는 다른 리소스 그룹에 있을 수 있습니다.

네트워크 인터페이스 카드를 사용 하 여 가상 컴퓨터를 이동 하려면 모든 종속 리소스를 이동 해야 합니다. 네트워크 인터페이스 카드, 가상 네트워크의 다른 모든 네트워크 인터페이스 카드 및 VPN gateway에 대 한 가상 네트워크를 이동 합니다.

## <a name="peered-virtual-network"></a>피어 링 가상 네트워크

피어링된 가상 네트워크를 이동하려면 먼저 가상 네트워크 피어링을 사용하지 않도록 설정해야 합니다. 사용하지 않도록 설정되면 가상 네트워크를 이동할 수 있습니다. 이동 후에는 가상 네트워크 피어링을 사용하도록 다시 설정합니다.

## <a name="subnet-links"></a>서브넷 링크

리소스 탐색 링크가 있는 서브넷이 가상 네트워크에 있는 경우 가상 네트워크를 다른 구독으로 이동할 수 없습니다. 예를 들어 Azure Cache for Redis 리소스가 서브넷에 배포된 경우 해당 서브넷에는 리소스 탐색 링크가 있습니다.

## <a name="next-steps"></a>다음 단계

리소스를 이동하는 명령은 [새 리소스 그룹 또는 구독으로 리소스 이동](../resource-group-move-resources.md)을 참조하세요.
