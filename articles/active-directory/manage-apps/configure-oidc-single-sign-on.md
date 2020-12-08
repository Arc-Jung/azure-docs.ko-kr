---
title: Azure Active Directory 앱에 대 한 OIDC 기반 Single Sign-On (SSO) 이해
description: Azure Active Directory 앱에 대 한 OIDC 기반 SSO (Single Sign-On)를 이해 합니다.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 10/19/2020
ms.author: kenwith
ms.reviewer: arajpathak7
ms.custom: contperfq2
ms.openlocfilehash: 487bf3c83be16ee80838b3bbe8bf8532eb6ffe12
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96780062"
---
# <a name="understand-oidc-based-single-sign-on"></a>OIDC 기반 Single Sign-On 이해
응용 프로그램 관리에 대 한 [빠른 시작 시리즈](view-applications-portal.md) 에서는 응용 프로그램에 대 한 IdP (id 공급자)로 Azure AD를 사용 하는 방법을 알아보았습니다. 이 문서에서는 Openid connect Connect 표준을 사용 하 여 Single Sign-On를 구현 하는 앱에 대해 자세히 설명 합니다. 

## <a name="before-you-begin"></a>시작하기 전 주의 사항
Azure Active Directory 테 넌 트에 앱을 추가 하는 프로세스는 응용 프로그램 구현 Single Sign-On의 유형에 따라 달라 집니다. Id 관리에 Azure AD를 사용할 수 있는 앱에 사용할 수 있는 Single Sign-On 옵션에 대 한 자세한 내용은 [Single Sign-On 옵션](sso-options.md)을 참조 하세요. 이 문서에서는 OIDC 기반 앱에 대해 설명 합니다.


## <a name="basic-oidc-configuration"></a>기본 OIDC 구성
[퀵 스타트 시리즈](add-application-portal-setup-oidc-sso.md)에는 Single Sign-On를 구성 하는 문서가 있습니다. 여기서는 Azure 테 넌 트에 OIDC 기반 앱을 추가 하는 방법에 대해 알아봅니다.

Single Sign-On에 대 한 OIDC standard를 사용 하는 앱을 추가 하는 것은 구성이 최소화 된다는 것입니다. 다음은 테 넌 트에 OIDC 기반 앱을 추가 하는 방법을 보여 주는 짧은 비디오입니다.

Azure Active Directory에서 OIDC 기반 앱 추가

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4HoNI]

사용자 및 관리자 동의에 대해 자세히 알아보려면 [사용자 및 관리자 동의 이해](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [애플리케이션 관리에 대한 빠른 시작 시리즈](add-application-portal-setup-oidc-sso.md)
- [Single Sign-On 옵션](sso-options.md)
- [방법: 다중 테넌트 애플리케이션 패턴을 사용하여 Azure Active Directory 사용자 로그인](../develop/howto-convert-app-to-be-multi-tenant.md)
