---
title: Conditional Access require managed device - Azure Active Directory
description: Learn how to configure Azure Active Directory (Azure AD) device-based Conditional Access policies that require managed devices for cloud app access.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31b7aa906cbefc0ffda707a228f2a9d50be351a8
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74380039"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>How To: Require managed devices for cloud app access with Conditional Access

모바일 우선, 클라우드 우선 세계에서 Azure AD(Active Directory)는 어디에서나 앱 및 서비스에 대한 Single Sign-On을 가능하게 합니다. 권한 있는 사용자는 모바일 및 개인 디바이스를 포함하여 광범위한 디바이스에서 클라우드 앱에 액세스할 수 있습니다. 하지만 여러 환경에는 보안 및 준수를 위해 표준을 충족하는 디바이스에 의해서만 액세스되어야 하는 몇 가지 앱이 있습니다. 이러한 디바이스는 관리 디바이스라고도 합니다. 

This article explains how you can configure Conditional Access policies that require managed devices to access certain cloud apps in your environment. 

## <a name="prerequisites"></a>전제 조건

Requiring managed devices for cloud app access ties **Azure AD Conditional Access** and **Azure AD device management** together. 이러한 내용에 익숙하지 않은 경우 먼저 다음 항목을 읽어보세요.

- **[Conditional Access in Azure Active Directory](../active-directory-conditional-access-azure-portal.md)** - This article provides you with a conceptual overview of Conditional Access and the related terminology.
- **[Azure Active Directory의 디바이스 관리 소개](../devices/overview.md)** - 이 문서에서는 디바이스를 조직에서 제어해야 하는 다양한 옵션에 대해 간략히 설명합니다. 

## <a name="scenario-description"></a>시나리오 설명

보안과 생산성 간의 균형을 유지하기가 어렵습니다. 클라우드 리소스에 액세스하도록 지원되는 디바이스의 범위가 넓어지면 사용자의 생산성을 개선할 수 있습니다. 반대로 환경에서 특정 리소스에 알려지지 않은 보호 수준을 가진 디바이스가 액세스하지 않도록 하는 것이 좋습니다. 영향을 받는 리소스의 경우 사용자가 관리 디바이스를 사용해서만 액세스할 수 있도록 해야 합니다. 

With Azure AD Conditional Access, you can address this requirement with a single policy that grants access:

- 선택한 클라우드 앱으로
- 선택한 사용자 및 그룹에 대해
- 관리 디바이스 필요

## <a name="managed-devices"></a>관리 디바이스  

간단히 말해서 관리되는 디바이스는 *일종*의 조직 제어 하에 있는 디바이스입니다. Azure AD에서 관리 디바이스의 필수 조건은 Azure AD에 등록된 것입니다. 디바이스를 등록하면 디바이스 개체 형태로 디바이스의 ID가 만들어집니다. 이 개체는 Azure에서 디바이스에 대한 상태 정보를 추적하는 데 사용됩니다. Azure AD 관리자는 이미 이 개체를 사용하여 디바이스의 상태를 토글(설정/해제)할 수 있습니다.
  
![디바이스 기반 조건](./media/require-managed-devices/32.png)

Azure AD에 등록된 디바이스를 가져오는 데는 세 가지 옵션이 있습니다. 

- **Azure AD registered devices** - to get a personal device registered with Azure AD
- **Azure AD joined devices** - to get an organizational Windows 10 device that is not joined to an on-premises AD registered with Azure AD. 
- **Hybrid Azure AD joined devices** - to get a Windows 10 or supported down-level device that is joined to an on-premises AD registered with Azure AD.

These three options are discussed in the article [What is a device identity?](../devices/overview.md)

관리되는 디바이스가 되려면, 등록된 디바이스는 **하이브리드 Azure AD 조인 디바이스** 또는 **준수 상태로 표시된 디바이스**여야 합니다.  

![디바이스 기반 조건](./media/require-managed-devices/47.png)
 
## <a name="require-hybrid-azure-ad-joined-devices"></a>하이브리드 Azure AD 조인된 디바이스 필요

In your Conditional Access policy, you can select **Require Hybrid Azure AD joined device** to state that the selected cloud apps can only be accessed using a managed device. 

![디바이스 기반 조건](./media/require-managed-devices/10.png)

이 설정은 온-프레미스 AD에 조인된 Windows 10 또는 Windows 7 또는 Windows 8과 같은 하위 수준 디바이스에만 적용됩니다. 등록된 Windows 10 디바이스를 가져오기 위한 [자동화된 프로세스](../devices/hybrid-azuread-join-plan.md)인 하이브리드 Azure AD 조인을 사용하여 이러한 디바이스를 Azure AD에만 등록할 수 있습니다. 

![디바이스 기반 조건](./media/require-managed-devices/45.png)

하이브리드 Azure AD 조인 디바이스를 관리 디바이스로 어떻게 만드나요?  온-프레미스 AD에 조인된 디바이스의 경우, 이러한 디바이스에 대한 제어는 디바이스를 관리하는 **SCCM(System Center Configuration Manager)** 또는 **GP(그룹 정책)** 과 같은 관리 솔루션을 사용하여 시행되는 것으로 가정합니다. Azure AD가 이러한 방법 중 어떤 것을 디바이스에 적용할지 결정할 방법이 없기 때문에 하이브리드 Azure AD 조인 디바이스를 요구하는 것은 관리 디바이스를 필요로 하는 비교적 약한 메커니즘입니다. 온-프레미스 도메인에 가입된 디바이스에 적용되는 방법이 관리 디바이스(이러한 디바이스가 하이브리드 Azure AD 조인 디바이스이기도 하다면)를 구성할 만큼 충분히 강력한지 여부를 판단하는 것은 관리자의 몫입니다.

## <a name="require-device-to-be-marked-as-compliant"></a>디바이스를 준수 상태로 표시해야 함

*디바이스를 준수 상태로 표시해야 하는* 옵션은 관리 디바이스를 요청하는 가장 강력한 형태입니다.

![디바이스 기반 조건](./media/require-managed-devices/11.png)

이 옵션을 사용하려면 디바이스를 Azure AD에 등록해야 하며 다음과 같이 준수 상태로도 표시되어야 합니다.
         
- Intune
- Azure AD 통합을 통해 Windows 10 디바이스를 관리하는 타사 MDM(모바일 디바이스 관리) 시스템입니다. Windows 10 이외의 디바이스 OS 형식에 대한 타사 MDM 시스템은 지원되지 않습니다.
 
![디바이스 기반 조건](./media/require-managed-devices/46.png)

준수 상태로 표시된 디바이스의 경우 다음과 같이 가정할 수 있습니다. 

- 직원이 회사 데이터에 액세스하는 데 사용하는 모바일 디바이스가 관리됩니다.
- 직원이 사용하는 모바일 앱이 관리됩니다.
- 직원이 정보에 액세스하고 공유하는 방식을 제어함으로써 회사 정보가 보호됩니다.
- 디바이스와 해당 앱은 회사 보안 요구 사항을 준수합니다.

## <a name="next-steps"></a>다음 단계

Before configuring a device-based Conditional Access policy in your environment, you should take a look at the [best practices for Conditional Access in Azure Active Directory](best-practices.md).
