---
title: '시나리오: 임의의 위치'
titleSuffix: Azure Virtual WAN
description: 라우팅에 대 한 시나리오-임의-임의
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 3b5002873160490dfb7b8d3ad9790f9c6f1e8ae6
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99525791"
---
# <a name="scenario-any-to-any"></a>시나리오: 임의의 위치

가상 WAN 가상 허브 라우팅으로 작업할 때 몇 가지 시나리오를 사용할 수 있습니다. 임의의 시나리오에서 모든 스포크는 다른 스포크에 도달할 수 있습니다. 여러 허브가 있는 경우 표준 가상 WAN에서 허브-허브 라우팅 (허브 간)이 기본적으로 사용 됩니다. Azure Portal, [Azure 빠른 시작 템플릿](quickstart-any-to-any-template.md)등의 다양 한 방법을 사용 하 여이 구성을 만들 수 있습니다. 가상 허브 라우팅에 대 한 자세한 내용은 [가상 허브 라우팅 정보](about-virtual-hub-routing.md)를 참조 하세요. 

## <a name="design"></a><a name="design"></a>디자인

가상 WAN 시나리오에서 필요한 경로 테이블 수를 파악 하기 위해 연결 행렬을 작성할 수 있습니다. 여기서 각 셀은 원본 (행)이 대상 (열)과 통신할 수 있는지 여부를 나타냅니다.

| From |   대상 |  *Vnet* | *분기* |
| -------------- | -------- | ---------- | ---|
| VNet     | &#8594;| 직접 | 직접 |
| 분기   | &#8594;| 직접  | 직접 |

위의 표에 나와 있는 각 셀은 가상 WAN 연결 (흐름의 "From" 쪽에서 행 머리글)이 대상 접두사 (흐름의 "To" 쪽, 즉 열 머리글이 이탤릭체 인 경우)와 통신 하는지 여부를 설명 합니다. 이 시나리오에서는 방화벽이 나 네트워크 가상 어플라이언스를 사용할 수 없으므로 가상 WAN (따라서 테이블의 "Direct")에서 직접 통신 흐름이 전달 됩니다.

Vnet 및 분기 (VPN, Express 경로 및 사용자 VPN)의 모든 연결에는 동일한 연결 요구 사항이 있으므로 단일 경로 테이블이 필요 합니다. 결과적으로 모든 연결이 연결 되 고 동일한 경로 테이블인 기본 경로 테이블로 전파 됩니다.

* 가상 네트워크:
  * 연결 된 경로 테이블: **기본값**
  * 경로 테이블에 전파: **기본값**
* 분기
  * 연결 된 경로 테이블: **기본값**
  * 경로 테이블에 전파: **기본값**

가상 허브 라우팅에 대 한 자세한 내용은 [가상 허브 라우팅 정보](about-virtual-hub-routing.md)를 참조 하세요.

## <a name="architecture"></a><a name="architecture"></a>아키텍처

**그림 1** 에서는 모든 Vnet 및 분기 (VPN, express 경로, P2S)가 서로 연결할 수 있습니다. 가상 허브에서 연결은 다음과 같이 작동 합니다.

* Vpn 연결은 vpn 사이트를 VPN gateway에 연결 합니다.
* 가상 네트워크 연결은 가상 네트워크를 가상 허브에 연결 합니다. 가상 허브의 라우터는 Vnet 간의 전송 기능을 제공 합니다.
* Express 경로 연결은 express 경로 회로를 Express 경로 게이트웨이에 연결 합니다.

연결의 라우팅 구성 ( **없음**) 또는 사용자 지정 경로 테이블을 설정 하지 않는 한 이러한 연결은 기본적으로 만들 때 기본 경로 테이블에 연결 됩니다. 이러한 연결은 기본적으로 기본 경로 테이블에도 경로를 전파 합니다. 이를 통해 모든 스포크 (VNet, VPN, ER, P2S)에 연결할 수 있는 임의의 시나리오를 사용할 수 있습니다.

**그림 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="그림 1":::

## <a name="workflow"></a><a name="workflow"></a>워크플로

이 시나리오는 표준 가상 WAN에 대해 기본적으로 사용 하도록 설정 됩니다. WAN 구성에서 분기 간 설정을 사용 하지 않도록 설정한 경우 분기 스포크 간의 연결을 허용 하지 않습니다. VPN/Express 경로/사용자 VPN은 가상 WAN에서 분기 스포크로 간주 됩니다.

## <a name="next-steps"></a>다음 단계

* Virtual WAN에 대한 자세한 내용은 [FAQ](virtual-wan-faq.md)를 참조하세요.
* 가상 허브 라우팅에 대 한 자세한 내용은 [가상 허브 라우팅 정보](about-virtual-hub-routing.md)를 참조 하세요.
