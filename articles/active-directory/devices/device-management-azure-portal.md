---
title: Azure Portal을 사용하여 디바이스를 관리하는 방법 | Microsoft Docs
description: Azure Portal을 사용하여 디바이스를 관리하는 방법을 알아봅니다.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 03/23/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: hafowler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18b43a99eb561cbfa340e0b3f318782bef2ca17c
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105023438"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Azure Portal을 사용하여 디바이스 ID 관리

Azure AD는 장치 id를 관리 하는 중앙의 장소를 제공 합니다.

**모든 장치** 페이지에서 다음을 수행할 수 있습니다.

- 다음을 포함 하 여 장치를 식별 합니다.
   - Azure AD에서 조인 또는 등록 된 장치입니다.
   - [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot)를 사용 하 여 배포 된 장치.
   - [유니버설 인쇄](/universal-print/fundamentals/universal-print-getting-started) 를 사용 하는 프린터
- 사용, 사용 안 함, 삭제 또는 관리와 같은 장치 id 관리 작업을 수행 합니다.
   - [프린터](/universal-print/fundamentals/) 및 [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) 장치는 Azure AD에서 제한 된 관리 옵션을 포함 합니다. 각 관리 인터페이스에서 관리 해야 합니다.
- 장치 id 설정을 구성 합니다.
- Enterprise State Roaming 사용 하거나 사용 하지 않도록 설정 합니다.
- 장치 관련 감사 로그 검토
- 장치 다운로드 (미리 보기)

[![Azure Portal의 모든 장치 보기](./media/device-management-azure-portal/all-devices-azure-portal.png)](./media/device-management-azure-portal/all-devices-azure-portal.png#lightbox)

다음 단계를 사용 하 여 장치 포털에 액세스할 수 있습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **Azure Active Directory**  >  **장치로** 이동 합니다.

## <a name="manage-devices"></a>디바이스 관리

Azure AD에서 장치를 관리 하는 데는 두 가지 위치가 있습니다.

- **Azure Portal**  >  **Azure Active Directory**  >  **장치**
- **Azure Portal**  >  **Azure Active Directory**  >  **사용자 > >** **장치** 선택

두 옵션 모두 관리자가 다음을 수행할 수 있습니다.

- 장치를 검색 합니다.
- 다음을 비롯 한 장치 세부 정보를 참조 하세요.
    - 디바이스 이름
    - 디바이스 ID
    - OS 및 버전
    - 조인 유형
    - 소유자
    - 모바일 장치 관리 및 규정 준수
    - BitLocker 복구 키
- , 사용, 사용 안 함, 삭제 또는 관리와 같은 장치 id 관리 작업을 수행 합니다.
   - [프린터](/universal-print/fundamentals/) 및 [Windows Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) 장치는 Azure AD에서 제한 된 관리 옵션을 포함 합니다. 각 관리 인터페이스에서 관리 해야 합니다.

> [!TIP]
> - 하이브리드 Azure AD 조인 Windows 10 디바이스에는 소유자가 없습니다. 소유자에 의해 장치를 찾고 있지 않은 경우 장치 ID를 검색 합니다.
>
> - 등록 된 열에서 "보류 중" 상태인 "하이브리드 Azure AD 조인" 장치가 표시 되 면 장치가 Azure AD connect에서 동기화 되 고 클라이언트에서 등록을 완료 하기 위해 대기 중임을 나타냅니다. [하이브리드 AZURE AD 조인 구현을 계획](hybrid-azuread-join-plan.md)하는 방법을 참조 하세요. 추가 정보는 [장치 faq](faq.md)(질문과 대답) 문서에서 찾을 수 있습니다.
>
> - 일부 iOS 디바이스의 경우 아포스트로피를 포함하는 디바이스 이름이 아포스트로피처럼 보이는 다른 문자를 사용할 수 있습니다. 따라서 이러한 장치를 검색 하는 것은 다소 복잡 합니다. 검색 결과가 올바르게 표시 되지 않는 경우 검색 문자열에 일치 하는 아포스트로피 문자를 포함 해야 합니다.

### <a name="manage-an-intune-device"></a>Intune 디바이스 관리

Intune 관리자 인 경우 MDM이 **Microsoft Intune** 으로 표시 된 장치를 관리할 수 있습니다. 장치가 Microsoft Intune 등록 되지 않은 경우 "관리" 옵션이 회색으로 표시 됩니다.

### <a name="enable-or-disable-an-azure-ad-device"></a>Azure AD 장치 사용 또는 사용 안 함

장치를 사용 하거나 사용 하지 않도록 설정 하려면 다음 두 가지 옵션을 사용할 수 있습니다.

- 하나 이상의 장치를 선택한 후 **모든 장치** 페이지의 도구 모음
- 특정 장치로 드릴 다운 한 후의 도구 모음입니다.

> [!IMPORTANT]
> - 장치를 사용 하거나 사용 하지 않도록 설정 하려면 Azure AD의 전역 관리자 또는 클라우드 장치 관리자 여야 합니다. 
> - 장치를 사용 하지 않도록 설정 하면 장치가 Azure AD에서 성공적으로 인증 되지 않으므로 장치 기반 조건부 액세스 또는 비즈니스용 Windows Hello 자격 증명을 사용 하 여 보호 되는 Azure AD 리소스에 액세스할 수 없게 됩니다.
> - 장치를 사용 하지 않도록 설정 하면 장치에서 주 새로 고침 토큰 (PRT) 및 새로 고침 토큰 (RT)이 모두 해지 됩니다.
> - Azure AD에서 프린터를 사용 하거나 사용 하지 않도록 설정할 수 없습니다.

### <a name="delete-an-azure-ad-device"></a>Azure AD 디바이스 삭제

디바이스를 삭제하려면 두 가지 옵션이 있습니다.

- 하나 이상의 장치를 선택한 후 **모든 장치** 페이지의 도구 모음
- 특정 장치로 드릴 다운 한 후의 도구 모음입니다.

> [!IMPORTANT]
> - 장치를 삭제 하려면 Azure AD에서 클라우드 장치 관리자, Intune 관리자 또는 전역 관리자 역할을 할당 받아야 합니다.
> - Azure AD에서 프린터 및 Windows Autopilot 장치를 삭제할 수 없음
> - 디바이스 삭제:
>    - 디바이스에서 Azure AD 리소스에 액세스할 수 없게 됩니다.
>    - 디바이스에 연결되어 있는 모든 세부 정보(예: Windows 디바이스에 대한 BitLocker 키)를 제거합니다.  
>    - 복구할 수 없으며, 반드시 필요한 경우가 아니면 권장되지 않는 작업을 나타냅니다.

장치를 다른 관리 기관 (예: Microsoft Intune)에서 관리 하는 경우, Azure AD에서 장치를 삭제 하기 전에 장치를 초기화/사용 중지 했는지 확인 합니다. 장치를 삭제 하기 전에 [오래 된 장치를 관리](manage-stale-devices.md) 하는 방법을 검토 합니다.

### <a name="view-or-copy-device-id"></a>디바이스 ID 확인 또는 복사

디바이스 ID를 사용하여 디바이스의 디바이스 ID 세부 정보를 확인하거나 문제 해결 동안 PowerShell을 사용할 수 있습니다. 복사 옵션에 액세스하려면 디바이스를 클릭합니다.

![디바이스 ID 보기](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>BitLocker 키 확인 또는 복사

사용자가 암호화 된 드라이브를 복구할 수 있도록 BitLocker 키를 보고 복사할 수 있습니다. 이러한 키는 암호화되고 해당 키가 Azure AD에 저장된 Windows 디바이스에 대해서만 사용할 수 있습니다. **복구 키 표시** 를 선택 하 여 장치 세부 정보에 액세스할 때 이러한 키를 찾을 수 있습니다. **복구 키 표시** 를 선택 하면 해당 범주에서 찾을 수 있는 감사 로그가 생성 됩니다 `KeyManagement` .

![BitLocker 키 보기](./media/device-management-azure-portal/device-details-show-bitlocker-key.png)

BitLocker 키를 보거나 복사하려면, 디바이스의 소유자 또는 다음 역할 중 하나 이상이 할당된 사용자여야 합니다.

- 클라우드 디바이스 관리자
- 전역 관리자
- 기술 지원팀 관리자
- Intune 서비스 관리자
- 보안 관리자
- 보안 Reader

### <a name="device-list-filtering-preview"></a>장치 목록 필터링 (미리 보기)

이전에는 작업 및 사용 상태를 기준으로 장치 목록만 필터링 할 수 있었습니다. 이제이 미리 보기를 통해 장치에서 다음 특성을 사용 하 여 장치 목록을 필터링 할 수 있습니다.

- 활성화 상태
- 규격 상태
- 조인 유형 (Azure AD 조인, 하이브리드 Azure AD 조인, Azure AD 등록 됨)
- 활동 타임스탬프
- OS
- 장치 유형 (프린터, 보안 Vm, 공유 장치, 등록 된 장치)

**모든 장치** 보기에서 미리 보기 필터링 기능을 사용 하려면 다음을 수행 합니다.

![필터링 미리 보기 기능 사용](./media/device-management-azure-portal/device-filter-preview-enable.png)

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **Azure Active Directory**  >  **장치로** 이동 합니다.
1. 표시 되는 배너를 선택 하 고 **새 장치 필터링 기능을 사용해 보세요. 미리 보기를 사용 하려면 클릭 합니다.**

이제 **모든 장치** 보기에 필터를 **추가할** 수 있습니다.

### <a name="download-devices-preview"></a>장치 다운로드 (미리 보기)

클라우드 장치 관리자, Intune 관리자 및 전역 관리자는 **장치 다운로드 (미리 보기)** 옵션을 사용 하 여 적용 된 필터를 기반으로 하는 장치를 CSV 파일로 내보낼 수 있습니다. 목록에 필터를 적용 하지 않으면 모든 장치를 내보냅니다. 내보내기는 다음에 따라 최대 1 시간 동안 실행 될 수 있습니다. 

내보낸 목록에는 다음과 같은 장치 id 특성이 포함 됩니다.

`accountEnabled, approximateLastLogonTimeStamp, deviceOSType, deviceOSVersion, deviceTrustType, dirSyncEnabled, displayName, isCompliant, isManaged, lastDirSyncTime, objectId, profileType, registeredOwners, systemLabels, registrationTime, mdmDisplayName`

## <a name="configure-device-settings"></a>디바이스 설정 구성

Azure AD 포털을 사용 하 여 장치 id를 관리 하려면 해당 장치를 Azure AD에 [등록 하거나 조인](overview.md) 해야 합니다. 관리자는 다음 장치 설정을 구성 하 여 장치를 등록 하 고 조인 하는 프로세스를 제어할 수 있습니다.

Azure Portal에서 장치 설정을 보거나 관리 하려면 다음 역할 중 하나가 할당 되어야 합니다.

- 전역 관리자
- 클라우드 디바이스 관리자
- 전역 독자
- 디렉터리 판독기

![Azure AD와 관련 된 장치 설정](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **사용자가 AZURE ad에 장치를 조인할 수 있습니다** .-이 설정을 사용 하면 장치를 azure ad 조인 장치로 등록할 수 있는 사용자를 선택할 수 있습니다. 기본값은 **All** 입니다.

> [!NOTE]
> **사용자가 AZURE ad에 장치를 조인할 수 있습니다** 설정은 Windows 10의 azure ad 조인에만 적용 됩니다. 하이브리드 azure ad 조인 장치, azure ad 조인 된 [vm](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure) 및 [Windows Autopilot 자체 배포 모드](/mem/autopilot/self-deploying) 를 사용 하는 azure ad 조인 장치에는이 설정이 적용 되지 않습니다. 이러한 메서드는 사용자가 없는 컨텍스트에서 작동 합니다.

- **Azure AD 조인 디바이스의 추가 로컬 관리자** - 디바이스에서 로컬 관리자 권한이 부여된 사용자를 선택할 수 있습니다. 이러한 사용자는 Azure AD의 *장치 관리자* 역할에 추가 됩니다. Azure AD의 전역 관리자 및 디바이스 소유자에게는 기본적으로 로컬 관리자 권한이 부여됩니다. 이 옵션은 Azure AD Premium 또는 EMS(Enterprise Mobility Suite) 등의 제품을 통해 사용할 수 있는 프리미엄 버전 기능입니다.
- **사용자가 AZURE ad에 장치를 등록할 수 있음** -Windows 10 개인, IOS, Android 및 macos 장치를 azure ad에 등록할 수 있도록이 설정을 구성 해야 합니다. **없음** 을 선택 하는 경우 장치는 Azure AD에 등록할 수 없습니다. Microsoft Intune 또는 Microsoft 365 용 MDM (모바일 장치 관리)에 등록 하려면 등록 해야 합니다. 이러한 서비스 중 하나를 구성한 경우 **모두** 가 선택되고 **없음** 은 사용할 수 없습니다.
- **AZURE ad에 가입 하거나 azure ad에 등록 된 장치에는 Multi-Factor Authentication 필요** -사용자가 장치를 Azure AD에 가입 하거나 등록 하기 위해 추가 인증 요소를 제공 해야 하는지 여부를 선택할 수 있습니다. 기본값은 **아니요** 입니다. 장치를 등록 하거나 조인할 때 multi-factor authentication을 요구 하는 것이 좋습니다. 이 서비스에 대해 Multi-Factor Authentication을 사용하도록 설정하기 전에 디바이스를 등록하는 사용자에 대해 Multi-Factor Authentication을 구성해야 합니다. 여러 Azure AD Multi-Factor Authentication 서비스에 대 한 자세한 내용은 [AZURE ad Multi-Factor Authentication 시작](../authentication/concept-mfa-howitworks.md)하기를 참조 하세요. 

> [!NOTE]
> **AZURE ad에 가입 된 장치 또는 azure ad에 등록 된 장치** 는 azure ad 조인 (몇 가지 예외 포함) 또는 azure ad에 등록 된 장치에 Multi-Factor Authentication 설정이 적용 되어야 합니다. 하이브리드 Azure AD 조인 장치, azure [의 AZURE ad 조인 된 vm](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure) 및 [Windows Autopilot 자체 배포 모드](/mem/autopilot/self-deploying)를 사용 하는 azure ad 조인 장치에는이 설정이 적용 되지 않습니다.

> [!IMPORTANT]
> - 장치를 연결 하거나 등록 하기 위해 다단계 인증을 적용 하려면 조건부 액세스에서 ["장치 등록 또는 연결" 사용자 작업](../conditional-access/concept-conditional-access-cloud-apps.md#user-actions) 을 사용 하는 것이 좋습니다. 
> - 조건부 액세스 정책을 사용 하 여 multi-factor authentication을 요구 하는 경우이 설정을 **아니요** 로 설정 해야 합니다. 

- **최대 장치 수** -이 설정을 사용 하면 azure ad에 가입 된 azure ad 또는 azure ad에 등록 된 장치의 최대 수를 선택할 수 있습니다. 사용자가 이 할당량에 도달하는 경우 기존 디바이스 중 하나 이상이 제거될 때까지 디바이스를 더 추가할 수 없습니다. 기본값은 **50** 입니다. 값을 최대 100까지 늘릴 수 있으며, 100 보다 높은 값을 입력 하면 Azure AD에서 100로 설정 됩니다. 무제한 값을 사용 하 여 기존 할당량 한도 이외의 제한을 적용 하지 않을 수도 있습니다.

> [!NOTE]
> **최대 장치 수** 설정은 azure ad에 가입 된 장치 또는 azure ad에 등록 된 장치에 적용 됩니다. 하이브리드 Azure AD 조인 장치에는이 설정이 적용 되지 않습니다.

- [엔터프라이즈 상태 로밍](enterprise-state-roaming-overview.md)

## <a name="audit-logs"></a>감사 로그

디바이스 활동은 활동 로그를 통해 사용할 수 있습니다. 이러한 로그는 장치 등록 서비스 및 사용자에 의해 트리거되는 활동을 포함 합니다.

- 디바이스 만들기 및 디바이스에 소유자/사용자 추가
- 디바이스 설정 변경
- 디바이스 삭제 또는 업데이트 등의 디바이스 작업

감사 데이터에 대한 진입점은 **디바이스** 페이지의 **작업** 섹션에 있는 **감사 로그** 입니다.

감사 로그에는 다음을 보여 주는 기본 목록 보기가 있습니다.

- 발생 날짜와 시간
- 대상
- 작업의 초기자/행위자(누가)
- 작업(무엇)

:::image type="content" source="./media/device-management-azure-portal/63.png" alt-text="4 개 감사 로그에 대 한 날짜, 대상, 행위자 및 작업을 나열 하는 장치 페이지의 작업 섹션에 있는 테이블의 스크린샷" border="false":::

도구 모음에서 **열** 을 클릭 하 여 목록 보기를 사용자 지정할 수 있습니다.

:::image type="content" source="./media/device-management-azure-portal/64.png" alt-text="장치 페이지의 도구 모음을 보여 주는 스크린샷 열 항목이 강조 표시 됩니다." border="false":::

보고된 데이터를 자신에게 적합한 수준으로 좁히려면 다음 필드를 사용하여 감사 데이터를 필터링할 수 있습니다.

- 범주
- 활동 리소스 종류
- 활동
- 날짜 범위
- 대상
- 초기자(작업자)

필터 이외의 방법으로도 특정 항목을 검색할 수 있습니다.

:::image type="content" source="./media/device-management-azure-portal/65.png" alt-text="범주, 활동 리소스 종류, 작업, 날짜 범위, 대상 및 행위자 필드와 검색 필드가 있는 감사 데이터 필터 컨트롤의 스크린샷" border="false":::

## <a name="next-steps"></a>다음 단계

[Azure AD에서 오래 된 장치를 관리 하는 방법](manage-stale-devices.md)

[엔터프라이즈 상태 로밍](enterprise-state-roaming-overview.md)
