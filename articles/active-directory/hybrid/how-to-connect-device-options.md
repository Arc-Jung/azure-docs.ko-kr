---
title: 'Azure AD Connect: 디바이스 옵션 | Microsoft Docs'
description: 이 문서에서는 Azure AD Connect에서 사용할 수 있는 디바이스 옵션에 대해 자세히 설명합니다.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96ddcdb67ef079cfa23902a1dcb03b0ec61077fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109535"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Connect: 디바이스 옵션

다음 설명서에서는 Azure AD Connect에서 사용할 수 있는 다양한 디바이스 옵션에 대한 정보를 제공합니다. Azure AD Connect를 사용하여 다음 두 작업을 구성할 수 있습니다. 
* **하이브리드 Azure AD 조인**: 사용자의 환경에 온-프레미스 AD 공간이 있고 Azure AD의 이점을 원하는 경우 하이브리드 Azure AD 가입 디바이스를 구현할 수 있습니다. 이 디바이스는 온-프레미스 Active Directory와 Azure Active Directory에 모두 가입됩니다.
* **장치 쓰기 :** 장치 쓰기 는 AD FS (2012 R2 이상) 보호 된 장치에 장치를 기반으로 조건부 액세스를 활성화하는 데 사용됩니다

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect에서 디바이스 옵션 구성

1.  Azure AD Connect를 실행합니다. **추가 작업** 페이지에서 **디바이스 옵션 구성**을 선택합니다.  **다음**을 클릭합니다.
    ![디바이스 옵션 구성](./media/how-to-connect-device-options/deviceoptions.png) 

    **개요** 페이지는 세부 정보를 표시합니다.
    ![개요](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > 새로운 디바이스 구성 옵션은 버전 1.1.819.0 이상에서만 사용할 수 있습니다.

2.  Azure AD의 자격 증명을 제공한 후에는 [디바이스 옵션] 페이지에서 수행할 작업을 선택할 수 있습니다.
    ![디바이스 작업](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>다음 단계

* [하이브리드 Azure AD 조인 구성](../device-management-hybrid-azuread-joined-devices-setup.md)
* [디바이스 쓰기 저장 구성/해제](how-to-connect-device-writeback.md)

