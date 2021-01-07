---
title: Azure WAF (웹 응용 프로그램 방화벽) 정책 개요
description: 이 문서는 WAF (웹 응용 프로그램 방화벽) global, 사이트별 및 URI 별 정책에 대 한 개요입니다.
services: web-application-firewall
ms.topic: article
author: winthrop28
ms.service: web-application-firewall
ms.date: 11/20/2020
ms.author: victorh
ms.openlocfilehash: 59ca0b85ba2aff29bdb2ad3379c1054041d2b4cb
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96518739"
---
# <a name="azure-web-application-firewall-waf-policy-overview"></a>Azure WAF (웹 응용 프로그램 방화벽) 정책 개요

웹 응용 프로그램 방화벽 정책에는 모든 WAF 설정 및 구성이 포함 됩니다. 여기에는 제외, 사용자 지정 규칙, 관리 되는 규칙 등이 포함 됩니다. 이러한 정책은 응용 프로그램 게이트웨이 (전역), 수신기 (사이트당) 또는 경로 기반 규칙 (URI 당)에 연결 되어 적용 됩니다.

만들 수 있는 정책 수에는 제한이 없습니다. 정책을 만들 때 적용 하려면 응용 프로그램 게이트웨이에 연결 해야 합니다. 응용 프로그램 게이트웨이, 수신기 및 경로 기반 규칙의 조합에 연결 될 수 있습니다.

## <a name="global-waf-policy"></a>전역 WAF 정책

WAF 정책을 전역적으로 연결 하는 경우 Application Gateway WAF의 모든 사이트는 동일한 관리 되는 규칙, 사용자 지정 규칙, 제외 및 기타 구성 된 설정으로 보호 됩니다.

단일 정책을 모든 사이트에 적용 하려면 정책을 응용 프로그램 게이트웨이와 연결할 수 있습니다. 자세한 내용은 Azure Portal를 사용 하 여 WAF 정책을 만들고 적용 하는 [Application Gateway에 대 한 웹 응용 프로그램 방화벽 정책 만들기](create-waf-policy-ag.md) 를 참조 하세요. 

## <a name="per-site-waf-policy"></a>사이트별 WAF 정책

사이트별 WAF 정책을 사용하면 사이트별 정책을 사용하여 단일 WAF 뒤에 서로 다른 보안 요구 사항으로 여러 사이트를 보호할 수 있습니다. 예를 들어 WAF 뒤에 5개의 사이트가 있는 경우 5개의 개별 WAF 정책(각 수신기에 대해 하나)을 사용하여 각 사이트에 대한 제외, 사용자 지정 규칙, 관리 규칙 집합 및 기타 모든 WAF 설정을 사용자 지정할 수 있습니다.

Application Gateway에 글로벌 정책이 적용되었다고 가정해 보겠습니다. 그런 다음 해당 Application Gateway의 수신기에 다른 정책을 적용합니다. 이제 수신기의 정책이 해당 수신기에만 적용됩니다. Application Gateway의 글로벌 정책은 특정 정책이 할당되지 않은 다른 모든 수신기 및 경로 기반 규칙에 계속 적용됩니다.

## <a name="per-uri-policy"></a>URI 별 정책

URI 수준까지 더 많은 사용자 지정을 위해 경로 기반 규칙과 WAF 정책을 연결할 수 있습니다. 단일 사이트 내에 다른 정책이 필요한 특정 페이지가 있으면 지정 된 URI에만 영향을 주는 WAF 정책을 변경할 수 있습니다. 이는 WAF 뒤의 다른 사이트 보다 훨씬 더 구체적인 WAF 정책을 필요로 하는 지불 또는 로그인 페이지나 기타 모든 Uri에 적용 될 수 있습니다.

사이트별 WAF 정책과 마찬가지로 보다 구체적인 정책은 더 낮은 특정 정책을 무시 합니다. 이는 URL 경로 맵에 대 한 URI 별 정책이 위의 사이트별 또는 전역 WAF 정책을 재정의 함을 의미 합니다.

### <a name="example"></a>예

Contoso.com, fabrikam.com 및 adatum.com의 세 사이트가 동일한 응용 프로그램 게이트웨이 뒤에 있다고 가정해 보겠습니다. 세 사이트 모두에 WAF를 적용 하려고 하지만 고객이 제품을 방문 하 고, 검색 하 고, 구입 하는 경우 adatum.com를 사용 하 여 보안을 추가 해야 합니다.

몇 가지 기본 설정, 제외 또는 사용자 지정 규칙을 사용 하 여 전역 정책을 WAF에 적용할 수 있습니다 .이 경우 일부 거짓 긍정을 차단 하는 트래픽을 중지할 수 있습니다. 이 경우 fabrikam.com 및 contoso.com가 SQL 백 엔드가 없는 정적 페이지 이기 때문에 전역 SQL 삽입 규칙을 실행할 필요가 없습니다. 따라서 전역 정책에서 이러한 규칙을 사용 하지 않도록 설정할 수 있습니다.

이 글로벌 정책은 contoso.com 및 fabrikam.com에 적합 하지만 로그인 정보 및 지불을 처리 하는 adatum.com에 대 한 추가 주의가 필요 합니다. Adatum 수신기에 사이트별 정책을 적용 하 고 SQL 규칙을 계속 실행할 수 있습니다. 또한 일부 트래픽을 차단 하는 쿠키가 있다고 가정 하므로 해당 쿠키에 대 한 제외를 만들어 거짓 긍정을 중지할 수 있습니다. 

Adatum.com/payments URI는 주의 해야 하는 곳입니다. 따라서 해당 URI에 다른 정책을 적용 하 고 모든 규칙을 사용 하도록 설정 된 상태로 두고 모든 제외 항목을 제거 합니다.

이 예제에서는 두 사이트에 적용 되는 전역 정책이 있습니다. 한 사이트에 적용 되는 사이트별 정책 및 특정 한 경로 기반 규칙에 적용 되는 URI 별 정책을 보유 하 고 있습니다. 이 예제에서는 해당 PowerShell에 대 한 [Azure PowerShell를 사용 하 여 사이트별 WAF 정책 구성](per-site-policies.md) 을 참조 하세요.

## <a name="existing-waf-configurations"></a>기존 WAF 구성

모든 새 웹 응용 프로그램 방화벽의 WAF 설정 (사용자 지정 규칙, 관리 되는 규칙 집합 구성, 제외 등)이 WAF 정책에 존재 합니다. 기존 WAF가 있는 경우 이러한 설정은 WAF 구성에 여전히 있을 수 있습니다. 새 WAF 정책으로 이동 하는 방법에 대 한 자세한 내용을 보려면 waf [구성을 Waf 정책으로 마이그레이션합니다](./migrate-policy.md). 


## <a name="next-steps"></a>다음 단계

- [Azure PowerShell를 사용 하 여 사이트별 및 URI 별 정책을 만듭니다](per-site-policies.md).
