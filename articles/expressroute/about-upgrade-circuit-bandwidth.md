---
title: 회로 대역폭 업그레이드 정보 | Azure Express 경로
description: 이 문서에서는 Express 경로 회로 대역폭을 업그레이드 하기 위한 모범 사례를 알아봅니다.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: duau
ms.openlocfilehash: 7831e7944321e074c312853e1534c47970ebdfdb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "89397953"
---
# <a name="about-upgrading-expressroute-circuit-bandwidth"></a>Express 경로 회로 대역폭 업그레이드 정보

Express 경로를 사용 하면 Microsoft의 글로벌 네트워크에 전용 및 개인 연결을 사용할 수 있습니다. Express 경로 네트워크 또는 MSEE (Microsoft Enterprise Edge) 장치에 직접 연결 하 여 연결을 촉진 합니다. 물리적 연결을 구성 하 고 테스트 한 후에는 Express 경로 회로를 만들고 피어 링을 구성 하 여 계층 2 및 계층 3 연결을 사용 하도록 설정할 수 있습니다.

## <a name="upgrade-circuit-bandwidth"></a><a name="upgrade"></a>회로 대역폭 업그레이드

회로 대역폭을 업그레이드 하기 위해 Express 경로 직접 또는 Express 경로 파트너는 업그레이드 성공에 [충분 한 대역폭을 사용](#considerations) 해야 합니다.

용량을 사용할 수 있는 경우 다음 방법을 사용 하 여 회로를 업그레이드할 수 있습니다.

* [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [PowerShell](expressroute-howto-circuit-arm.md#modify)
* [Azure CLI](howto-circuit-cli.md#modify)

## <a name="capacity-considerations"></a><a name="considerations"></a>용량 고려 사항

### <a name="insufficient-expressroute-partner-bandwidth"></a><a name="bandwidth"></a>Express 경로 파트너 대역폭이 부족 합니다.

Express 경로 파트너에 충분 한 용량이 없는 경우 원하는 대역폭으로 구성 된 새 회로를 만들어야 합니다. 연결을 유지 하기 위해 새로 생성 된 회로가 프로 비전 되 고 피어 링이 구성 되 고 (개인 피어 링에 대 한) Express 경로 가상 네트워크 게이트웨이에 대 한 연결 개체가 프로 비전 될 때까지 이전 회로를 삭제 하지 마세요.

Express 경로 파트너에 사용 가능한 용량이 충분 하지 않은 경우 원하는 피어 링 위치에서 추가 용량을 요청 해야 합니다. 새 용량이 프로 비전 되 면 [회로 대역폭 업그레이드](#upgrade) 섹션의 문서에 포함 된 단계를 사용 하 여 새 회로를 만들고, 연결을 구성 하 고, 이전 회로를 삭제할 수 있습니다.


### <a name="insufficient-expressroute-direct-bandwidth"></a><a name="bandwidth"></a>Express 경로 직접 대역폭이 부족 합니다.

Express 경로 다이렉트에 충분 한 용량이 없는 경우 더 이상 필요 하지 않은 Express 경로 직접 리소스에 연결 된 회로를 삭제 하거나 새 Express 경로 직접 리소스를 만들 수 있습니다. Express 경로 직접 리소스를 관리 하는 방법에 대 한 지침은 [express 경로 다이렉트를 구성 하는 방법](how-to-expressroute-direct-portal.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

* [회로 만들기 및 수정](expressroute-howto-circuit-portal-resource-manager.md)
* [피어링 구성 만들기 및 수정](expressroute-howto-routing-portal-resource-manager.md)
* [Express 경로 회로에 가상 네트워크 연결](expressroute-howto-linkvnet-portal-resource-manager.md)
