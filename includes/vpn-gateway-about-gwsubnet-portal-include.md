---
title: 파일 포함
description: 포함 파일
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 09/24/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 92a6fef3305891b47c4dc2e0f0432e1079df5a69
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2020
ms.locfileid: "71266532"
---
가상 네트워크 게이트웨이는 '게이트웨이 서브넷'이라는 특정 서브넷을 사용합니다. 게이트웨이 서브넷은 가상 네트워크를 구성할 때 지정하는 가상 네트워크 IP 주소 범위에 속합니다. 여기에는 가상 네트워크 게이트웨이 리소스 및 서비스에서 사용하는 IP 주소가 포함됩니다. 


게이트웨이 서브넷을 만드는 경우 서브넷이 포함하는 IP 주소의 수를 지정합니다. 필요한 IP 주소의 수는 만들려는 VPN 게이트웨이 구성에 따라 다릅니다. 일부 구성에는 다른 구성보다 더 많은 IP 주소가 필요합니다. /27 또는 /28을 사용하는 게이트웨이 서브넷을 만드는 것이 좋습니다.


주소 공간이 서브넷과 겹치거나 서브넷이 가상 네트워크에 대한 주소 공간에 포함되지 않는다고 상술하는 오류가 표시되면 VNet 주소 범위를 확인합니다. 가상 네트워크에 대해 만든 주소 범위에 사용할 수 있는 IP 주소가 충분하지 않을 수 있습니다. 예를 들어 기본 서브넷이 전체 주소 범위를 포함하는 경우 추가 서브넷을 만들기 위한 IP 주소가 남아 있지 않습니다. 기존 주소 공간 내에서 서브넷을 조정하여 IP 주소를 확보하거나 추가 주소 범위를 지정하여 해당 범위에 속한 게이트웨이 서브넷을 만들 수 있습니다.