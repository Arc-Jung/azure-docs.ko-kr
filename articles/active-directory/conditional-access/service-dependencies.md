---
title: 조건부 액세스 서비스 종속성-Azure Active Directory
description: 조건부 액세스 Azure Active Directory에서 조건을 사용 하 여 정책을 트리거하는 방법에 대해 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 09/21/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6592add5e33ba240c0f1d9fdbd23d82adfe5229
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91258611"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>조건부 액세스 Azure Active Directory의 서비스 종속성은 무엇 인가요? 

조건부 액세스 정책을 사용 하 여 웹 사이트 및 서비스에 대 한 액세스 요구 사항을 지정할 수 있습니다. 예를 들어 액세스 요구 사항에는 MFA (multi-factor authentication) 또는 [관리 되는 장치](require-managed-devices.md)를 요구 하는 기능이 포함 될 수 있습니다. 

사이트 또는 서비스에 직접 액세스 하는 경우 일반적으로 관련 정책의 영향을 쉽게 평가할 수 있습니다. 예를 들어 SharePoint Online에 대해 MFA (multi-factor authentication)를 구성 해야 하는 정책이 있는 경우 SharePoint 웹 포털에 로그인 할 때마다 MFA가 적용 됩니다. 그러나 다른 클라우드 앱에 대 한 종속성이 있는 클라우드 앱이 있으므로 정책의 영향을 평가 하는 것은 항상 직접적인 것은 아닙니다. 예를 들어 Microsoft 팀은 SharePoint Online에서 리소스에 대 한 액세스를 제공할 수 있습니다. 따라서 현재 시나리오에서 Microsoft 팀에 액세스할 때 SharePoint MFA 정책도 적용 됩니다. 

> [!TIP]
> Office [365](concept-conditional-access-cloud-apps.md#office-365) 앱을 사용 하면 office 스택에서 서비스 종속성 문제를 방지 하기 위해 모든 office 앱을 대상으로 합니다.

## <a name="policy-enforcement"></a>정책 적용 

서비스 종속성이 구성 된 경우 초기 바인딩 또는 런타임에 바인딩된 적용을 사용 하 여 정책을 적용할 수 있습니다. 

- **초기 바인딩 정책 적용** 은 사용자가 호출 하는 앱에 액세스 하기 전에 종속 서비스 정책을 충족 해야 함을 의미 합니다. 예를 들어 MS 팀에 로그인 하기 전에 사용자가 SharePoint 정책을 충족 해야 합니다. 
- **런타임에 바인딩된 정책 적용** 은 사용자가 호출 앱에 로그인 한 후에 발생 합니다. 적용은 다운스트림 서비스용 토큰 인 앱 요청을 호출할 때로 지연 됩니다. 예제에는 Planner에 액세스 하는 MS 팀과 SharePoint 액세스 Office.com 포함 됩니다. 

아래 다이어그램은 MS 팀 서비스 종속성을 보여 줍니다. 흰색 화살표는 초기 바인딩 적용을 나타냅니다. Planner의 파선 화살표는 런타임에 바인딩된 적용을 나타냅니다. 

![MS 팀 서비스 종속성](./media/service-dependencies/01.png)

모범 사례에 따라 가능한 경우 관련 앱과 서비스 간에 공통 정책을 설정 해야 합니다. 일관 된 보안 상태를 유지 하면 최상의 사용자 환경을 제공 합니다. 예를 들어 Exchange Online, SharePoint Online, Microsoft 팀 및 비즈니스용 Skype에서 일반적인 정책을 설정 하면 다운스트림 서비스에 적용 되는 여러 정책에 의해 발생할 수 있는 예기치 않은 프롬프트가 현저 하 게 줄어듭니다. 

Office stack의 응용 프로그램을 사용 하 여이를 수행 하는 좋은 방법은 개별 응용 프로그램을 대상으로 지정 하는 대신 [office 365 앱](concept-conditional-access-cloud-apps.md#office-365) 을 사용 하는 것입니다.

아래 표에는 클라이언트 앱이 충족 해야 하는 추가 서비스 종속성이 나열 되어 있습니다.  

| 클라이언트 앱         | 다운스트림 서비스                          | 적용 |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |
| Microsoft 교실 | Exchange                                    | 초기 바인딩 |
|                     | SharePoint                                  | 초기 바인딩 |
| Microsoft Teams     | Exchange                                    | 초기 바인딩 |
|                     | MS Planner                                  | 런타임에 바인딩  |
|                     | Microsoft Stream                            | 런타임에 바인딩  |
|                     | SharePoint                                  | 초기 바인딩 |
|                     | 비즈니스 온라인용 Skype                   | 초기 바인딩 |
| Office 포털       | Exchange                                    | 런타임에 바인딩  |
|                     | SharePoint                                  | 런타임에 바인딩  |
| Outlook 그룹      | Exchange                                    | 초기 바인딩 |
|                     | SharePoint                                  | 초기 바인딩 |
| PowerApps           | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |
|                     | Windows Azure Active Directory              | 초기 바인딩 |
| Project             | Dynamics CRM                                | 초기 바인딩 |
| 비즈니스용 Skype  | Exchange                                    | 초기 바인딩 |
| Visual Studio       | Microsoft Azure 관리 (포털 및 API) | 초기 바인딩 |
| Microsoft Forms     | Exchange                                    | 초기 바인딩 |
|                     | SharePoint                                  | 초기 바인딩 |
| Microsoft To-Do     | Exchange                                    | 초기 바인딩 |

## <a name="next-steps"></a>다음 단계

사용자 환경에서 조건부 액세스를 구현 하는 방법에 대 한 자세한 내용은 [Azure Active Directory에서 조건부 액세스 배포 계획](plan-conditional-access.md)을 참조 하세요.
