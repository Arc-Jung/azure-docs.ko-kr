---
title: 파일 포함
description: 파일 포함
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "71174971"
---
## <a name="terminology"></a>용어

Azure의 Marketplace 이미지에는 다음과 같은 특성이 있습니다.

* **Publisher**: 이미지를 만든 조직입니다. 예: Canonical, MicrosoftWindowsServer
* **Offer**: 게시자가 만든 관련 이미지 그룹의 이름입니다. 예: UbuntuServer, WindowsServer
* **SKU**: 제공의 인스턴스(예: 배포의 주 릴리스)입니다. 예: 18.04-LTS, 2019-Datacenter
* **Version**: 이미지 SKU의 버전 번호입니다. 

프로그래밍 방식으로 VM을 배포할 때 Marketplace 이미지를 식별하기 위해 이러한 값을 개별적인 매개변수로 제공합니다. 일부 도구는 이미지 *URN* 을 허용합니다. 다음 값을 콜론(:) 문자로 구분해서 조합합니다. *Publisher*:*Offer*:*Sku*:*Version* URN에서 버전 번호를 "latest"로 바꿀 수 있으며, 그러면 이미지의 최신 버전이 선택됩니다. 

이미지 게시자가 추가 라이선스 및 구매 약관을 제공하는 경우에는, 해당 약관에 동의하고 프로그래밍 방식 배포를 사용하도록 설정해야 합니다. VM을 프로그래밍 방식으로 배포하는 경우에는 *구매 계획* 매개 변수도 제공해야 합니다. [Marketplace 약관으로 이미지 배포](#deploy-an-image-with-marketplace-terms)를 참조하세요.
