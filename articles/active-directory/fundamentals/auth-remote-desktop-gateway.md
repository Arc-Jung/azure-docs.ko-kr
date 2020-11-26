---
title: Azure Active Directory를 사용 하 여 서비스 원격 데스크톱 게이트웨이
description: Azure Active Directory를 사용 하 여 원격 데스크톱 게이트웨이 서비스를 실현 하는 아키텍처 지침입니다.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4baaf2de6fbe4a56f64d449644b8594217dc432c
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96172740"
---
# <a name="remote-desktop-gateway-services"></a>원격 데스크톱 게이트웨이 서비스

표준 원격 데스크톱 서비스 (RDS) 배포에는 Windows Server에서 실행 되는 다양 한 [원격 데스크톱 역할 서비스가](/windows-server/remote/remote-desktop-services/Desktop-hosting-logical-architecture) 포함 되어 있습니다. Azure Active Directory (Azure AD) 응용 프로그램 프록시를 사용 하는 RDS 배포는 커넥터 서비스를 실행 하는 서버에서 영구 아웃 바운드 연결을 사용 합니다. 기타 배포에서는 부하 분산 장치를 통해 열린 인바운드 연결을 유지합니다. 이 인증 패턴을 사용 하면 원격 데스크톱 서비스를 통해 온-프레미스 응용 프로그램을 게시 하 여 더 많은 유형의 응용 프로그램을 제공할 수 있습니다. 또한 Azure AD 응용 프로그램 프록시를 사용 하 여 배포의 공격 노출 영역을 줄입니다.

## <a name="use-when"></a>적용 가능한 상황

사전 인증을 사용 하 여 원격 액세스를 제공 하 고 원격 데스크톱 서비스 배포를 보호 해야 합니다.

![아키텍처 다이어그램](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>시스템의 구성 요소

* **사용자**: 응용 프로그램 프록시에서 제공 하는 RDS에 액세스 합니다.

* **웹 브라우저**: 응용 프로그램의 외부 URL에 액세스 하기 위해 사용자가 상호 작용 하는 구성 요소입니다.

* **AZURE AD**: 사용자를 인증 합니다. 

* **응용 프로그램 프록시 서비스**: 사용자의 요청을 RDS로 전달 하는 역방향 프록시 역할을 합니다. 응용 프로그램 프록시는 모든 조건부 액세스 정책을 적용할 수도 있습니다. 

* **원격 데스크톱 서비스**: 개별 가상화 된 응용 프로그램에 대 한 플랫폼 역할을 하며, 안전한 모바일 및 원격 데스크톱 액세스를 제공 하 고, 최종 사용자에 게 클라우드에서 응용 프로그램과 데스크톱을 실행할 수 있는 기능을 제공 합니다. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Azure AD를 사용 하 여 원격 데스크톱 게이트웨이 서비스 구현

* [Azure AD 응용 프로그램 프록시를 사용 하 여 원격 데스크톱 게시](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md) 

* [Azure AD에서 응용 프로그램 프록시를 통해 원격 액세스를 위한 온-프레미스 응용 프로그램 추가](../manage-apps/application-proxy-add-on-premises-application.md)

