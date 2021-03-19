---
title: 네트워크 규칙 (미리 보기)에서 Azure 방화벽 관리자 필터링
description: 네트워크 규칙에서 FQDN 필터링을 사용 하는 방법
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 07/30/2020
ms.author: victorh
ms.openlocfilehash: 28cd26532ca5bdf83902854b7910f7d6c18a4eab
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87460153"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>네트워크 규칙의 FQDN 필터링 (미리 보기)

> [!IMPORTANT]
> 네트워크 규칙의 FQDN 필터링은 현재 공개 미리 보기 상태입니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

FQDN (정규화 된 도메인 이름)은 호스트 또는 IP 주소의 도메인 이름을 나타냅니다. Azure 방화벽 및 방화벽 정책에서 DNS 확인을 기반으로 네트워크 규칙에서 Fqdn을 사용할 수 있습니다. 이 기능을 사용 하면 모든 TCP/UDP 프로토콜 (NTP, SSH, RDP 등 포함)을 사용 하 여 아웃 바운드 트래픽을 필터링 할 수 있습니다. 네트워크 규칙에서 Fqdn을 사용 하려면 DNS 프록시를 사용 하도록 설정 해야 합니다. 자세한 내용은 [Azure 방화벽 정책 DNS 설정 (미리 보기)](dns-settings.md)을 참조 하세요.

## <a name="how-it-works"></a>작동 방법

조직에 필요한 DNS 서버 (Azure DNS 또는 사용자 지정 DNS)를 정의 하 고 나면 Azure 방화벽은 선택한 DNS 서버를 기반으로 FQDN을 IP 주소로 변환 합니다. 이러한 변환은 응용 프로그램 및 네트워크 규칙 처리 모두에서 발생 합니다.

응용 프로그램 규칙에서 도메인 이름을 사용 하는 경우와 네트워크 규칙의 차이점은 무엇 인가요? 

- HTTP/S 및 MSSQL의 응용 프로그램 규칙에서 FQDN 필터링은 응용 프로그램 수준 투명 프록시와 SNI 헤더를 기반으로 합니다. 따라서 동일한 IP 주소로 확인 되는 두 Fqdn을 구분할 수 있습니다. 네트워크 규칙의 FQDN 필터링은 그렇지 않습니다. 가능 하면 항상 응용 프로그램 규칙을 사용 합니다.
- 응용 프로그램 규칙에서 HTTP/S 및 MSSQL을 선택한 프로토콜로 사용할 수 있습니다. 네트워크 규칙에서 임의의 TCP/UDP 프로토콜을 대상 Fqdn과 함께 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

[Azure 방화벽 DNS 설정](dns-settings.md)
