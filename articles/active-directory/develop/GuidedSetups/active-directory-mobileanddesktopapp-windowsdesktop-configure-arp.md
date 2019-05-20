---
title: Azure AD v2 Windows 데스크톱 시작 - 구성 | Microsoft Docs
description: Windows Desktop .NET(XAML) 애플리케이션이 액세스 토큰을 얻고 Azure Active Directory v2 엔드포인트로 보호되는 API를 호출하는 방식
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef32cef67ae5f32959e026157f5126647b01401d
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540572"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>앱에 애플리케이션의 등록 정보 추가
이 단계에서는 프로젝트에 애플리케이션 ID를 추가해야 합니다.

1.  `App.xaml.cs`를 열고 다음으로 `ClientId`를 바꿉니다.

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>다음 내용

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
