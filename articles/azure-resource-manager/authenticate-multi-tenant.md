---
title: 테넌트에서 인증 - Azure Resource Manager
description: Azure Resource Manager가 테넌트에서 인증 요청을 처리하는 방법을 설명합니다.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: tomfitz
ms.openlocfilehash: 625a17156eaf199af0d51151c6fd37769b8f7b4a
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848763"
---
# <a name="authenticate-requests-across-tenants"></a>테넌트에서 요청 인증

다중 테넌트 애플리케이션을 만들 때 다른 테넌트에 있는 리소스에 대한 인증 요청을 처리해야 합니다. 일반적인 시나리오는 하나의 테넌트에 있는 가상 머신에서 다른 테넌트의 가상 네트워크에 조인해야 하는 경우입니다. Azure Resource Manager는 다른 테넌트에 대한 요청을 인증하기 위한 보조 토큰을 저장하기 위해 헤더 값을 제공합니다.

## <a name="header-values-for-authentication"></a>인증에 대한 헤더 값

요청에는 다음 인증 헤더 값이 있습니다.

| 헤더 이름 | Description | 예제 값 |
| ----------- | ----------- | ------------ |
| Authorization | 기본 토큰 | Bearer &lt;primary-token&gt; |
| x-ms-authorization-auxiliary | 보조 토큰 | 전달자 &lt;보조-token1&gt;, encryptedbearer &lt;보조-token2&gt;, 전달자 &lt;보조-token3&gt; |

보조 헤더는 최대 세 개의 보조 토큰을 유지할 수 있습니다. 

다중 테넌트 앱의 코드에서 다른 테넌트에 대한 인증 토큰을 가져오고 보조 헤더에 저장합니다. 모든 토큰은 동일한 사용자 또는 애플리케이션에서 가져와야 합니다. 사용자 또는 애플리케이션은 다른 테넌트에 대한 게스트로 초대되어야 합니다.

## <a name="processing-the-request"></a>요청 처리

앱에서 Resource Manager로 요청을 보낼 때 요청은 기본 토큰의 ID에서 실행됩니다. 기본 토큰은 유효하고 만료되지 않아야 합니다. 이 토큰은 구독을 관리할 수 있는 테넌트에서 가져와야 합니다.

요청의 다른 테넌트의 리소스를 참조하는 경우 Resource Manager는 보조 토큰을 확인하여 요청을 처리할 수 있는지 여부를 확인합니다. 헤더의 모든 보조 토큰은 유효하고 만료되지 않아야 합니다. 토큰이 만료된 경우 Resource Manager는 401 응답 코드를 반환합니다. 응답은 유효하지 않은 토큰에서 클라이언트 ID 및 테넌트 ID를 포함합니다. 보조 헤더가 테넌트에 대한 유효한 토큰을 포함하는 경우 교차 테넌트 요청이 처리됩니다.

## <a name="next-steps"></a>다음 단계
* Azure Resource Manager API를 사용하여 인증 요청 보내기에 대해 자세히 알아보려면 [Resource Manager 인증 API를 사용하여 구독에 액세스](resource-manager-api-authentication.md)를 참조하세요.
* 토큰에 대한 자세한 내용은 [Azure Active Directory 액세스 토큰](/azure/active-directory/develop/access-tokens)을 참조하세요.
