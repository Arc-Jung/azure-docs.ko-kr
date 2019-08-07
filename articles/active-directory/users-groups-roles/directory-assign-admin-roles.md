---
title: 관리자 역할 설명 및 사용 권한 - Azure Active Directory | Microsoft Docs
description: 관리자 역할은 사용자를 추가하고, 관리자 역할을 할당하며, 사용자 암호를 다시 설정하고, 사용자 라이선스 또는 도메인을 관리하는 데 사용할 수 있습니다.
services: active-directory
author: curtand
manager: mtillman
search.appverid: MET150
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/04/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e139b274ab8a1f7d91d46ec56171b84db4f5025e
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812838"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory에서 관리자 역할 사용 권한

Azure Active Directory (Azure AD)를 사용 하 여 제한 된 관리자에 게 권한이 낮은 역할의 id 작업을 관리 하도록 지정할 수 있습니다. 사용자 추가 또는 변경, 관리 역할 할당, 사용자 암호 다시 설정, 사용자 라이선스 관리, 도메인 이름 관리 등을 위해 관리자를 할당할 수 있습니다. 기본 사용자 권한은 Azure AD의 사용자 설정에서만 변경할 수 있습니다.

## <a name="limit-the-use-of-global-administrator"></a>전역 관리자의 사용 제한

전역 관리자 역할에 할당 된 사용자는 Azure AD 조직의 모든 관리 설정을 읽고 수정할 수 있습니다. 기본적으로 Azure 구독에 등록 하는 사람에 게는 Azure AD 조직에 대 한 전역 관리자 역할이 할당 됩니다. 전역 관리자 및 권한 있는 역할 관리자만 관리자 역할을 위임할 수 있습니다. 비즈니스에 대 한 위험을 줄이려면 조직에서 가능한 최소한의 사용자에 게이 역할을 할당 하는 것이 좋습니다.

## <a name="best-practices"></a>최선의 구현 방법

모범 사례로, 조직에서 5 명 미만의 사용자에 게이 역할을 할당 하는 것이 좋습니다. 조직의 전역 관리자 역할에 할당 된 5 명 이상의 사용자가 있는 경우 사용량을 줄이는 몇 가지 방법은 다음과 같습니다.

### <a name="find-the-role-you-need"></a>필요한 역할 찾기

많은 역할 목록에서 필요한 역할을 찾을 수 없는 경우 Azure AD는 역할 범주에 따라 더 짧은 목록을 제공할 수 있습니다. 선택한 유형의 역할만 표시 하도록 [AZURE AD 역할 및 관리자](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) 에 대 한 새 **유형** 필터를 확인 합니다.

### <a name="a-role-exists-now-that-didnt-exist-when-you-assigned-the-global-administrator-role"></a>전역 관리자 역할을 할당할 때 존재 하지 않는 역할이 있습니다.

일부 사용자를 전역 관리자로 승격 시킬 때 옵션이 아닌 보다 세분화 된 사용 권한을 제공 하는 Azure AD에 역할 또는 역할을 추가할 수 있습니다. 시간이 지남에 따라 전역 관리자 역할만이 수행할 수 있는 작업을 수행 하는 추가 역할을 배포 합니다. 다음 [사용 가능한 역할](#available-roles)에 이러한 내용이 반영 된 것을 볼 수 있습니다.

## <a name="assign-or-remove-administrator-roles"></a>관리자 역할 할당 또는 제거

Azure Active Directory에서 사용자에게 관리 역할을 할당하는 방법을 알아보려면 [Azure Active Directory에서 관리자 역할 보기 및 할당](directory-manage-roles-portal.md)을 참조하세요.

## <a name="available-roles"></a>사용 가능한 역할

다음과 같은 관리자 역할을 사용할 수 있습니다.

* **[애플리케이션 관리자](#application-administrator)** : 이 역할의 사용자는 엔터프라이즈 애플리케이션, 애플리케이션 등록 및 애플리케이션 프록시 설정의 모든 측면을 만들고 관리할 수 있습니다. 또한 이 역할은 위임된 권한 및 Microsoft Graph와 Azure AD Graph를 제외한 애플리케이션 사용 권한에 동의하는 기능을 부여합니다. 이 역할에 할당 된 사용자는 새 응용 프로그램 등록 또는 엔터프라이즈 응용 프로그램을 만들 때 소유자로 추가 되지 않습니다.

  <b>중요</b>: 이 역할은 애플리케이션 자격 증명을 관리하는 기능을 부여합니다. 이 역할이 할당된 사용자는 애플리케이션에 자격 증명을 추가하고 해당 자격 증명을 사용하여 애플리케이션의 ID를 가장할 수 있습니다. 애플리케이션의 ID에 사용자 또는 다른 개체를 만들거나 업데이트하는 기능과 같이 Azure Active Directory에 대한 액세스 권한이 부여된 경우 이 역할에 할당된 사용자는 애플리케이션을 가장하는 동안 이러한 작업을 수행할 수 있습니다. 애플리케이션의 ID를 가장하는 이 기능은 Azure AD에서 본인의 역할 할당을 통해 사용자가 수행할 수 있는 권한 상승이 될 수 있습니다. 애플리케이션 관리자 역할에 사용자를 할당하면 애플리케이션의 ID를 가장하는 기능이 해당 사용자에게 부여된다는 점을 이해해야 합니다.

* **[애플리케이션 개발자](#application-developer)** : 이 역할의 사용자는 "사용자가 애플리케이션을 등록할 수 있음" 설정이 아니오로 설정된 경우 애플리케이션 등록을 만들 수 있습니다. 또한이 역할은 "사용자가 대신 회사 데이터에 액세스 하는 앱에 동의할 수 있습니다." 설정이 아니요로 설정 된 경우 사용자를 대신 하 여 동의할 수 있는 권한을 부여 합니다. 이 역할에 할당 된 사용자는 새 응용 프로그램 등록 또는 엔터프라이즈 응용 프로그램을 만들 때 소유자로 추가 됩니다.

* **[인증 관리자](#authentication-administrator)** : 이 역할을 가진 사용자는 암호 이외의 자격 증명을 설정 하거나 다시 설정할 수 있으며 모든 사용자에 대 한 암호를 업데이트할 수 있습니다. 인증 관리자는 사용자가 기존 비 암호 자격 증명 (예: MFA 또는 FIDO)에 다시 등록 하도록 요구 하 고, **장치에서 mfa 기억을**취소 하 여 관리자가 아닌 사용자가 다음에 로그인 할 때 mfa를 묻는 메시지를 표시할 수 있습니다. 다음 역할만 할당 됨:
  * 인증 관리자
  * 디렉터리 읽기 권한자
  * 게스트 초대자
  * 메시지 센터 읽기 권한자
  * 보고서 읽기 권한자

  인증 관리자 역할은 현재 공개 미리 보기로 제공 됩니다. 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

  <b>중요</b>: 이 역할의 사용자는 Azure Active Directory 내부 및 외부에 있는 중요한 프라이빗 정보 또는 중요한 구성에 대한 액세스 권한이 있을 수 있는 사용자의 자격 증명을 변경할 수 있습니다. 사용자의 자격 증명을 변경한다는 것은 사용자의 ID 및 사용 권한을 가정할 수 있음을 의미할 수 있습니다. 예를 들어:

  * 애플리케이션 등록 및 엔터프라이즈 애플리케이션 소유자: 소유한 앱의 자격 증명을 관리할 수 있습니다. 이러한 앱은 Azure AD에서 사용 권한이 부여되었을 수 있으며, 다른 위치에서는 인증 관리자에 권한이 부여되지 않습니다. 이 경로를 통해 인증 관리자는 응용 프로그램 소유자의 id를 가정 하 고 응용 프로그램에 대 한 자격 증명을 업데이트 하 여 권한 있는 응용 프로그램의 id를 추가로 가정할 수 있습니다.
  * Azure 구독 소유자: Azure에서 중요한 프라이빗 정보 또는 중요한 구성에 액세스할 수 있습니다.
  * 보안 그룹 및 Office 365 그룹 소유자: 그룹 멤버 자격을 관리할 수 있습니다. 해당 그룹은 중요한 프라이빗 정보 또는 Azure AD 및 다른 위치의 중요한 구성에 대한 액세스 권한을 부여할 수 있습니다.
  * Exchange Online, Office 보안 및 준수 센터, 인사 관리 시스템과 같은 Azure AD 외부의 다른 서비스에 있는 관리자
  * 중요한 프라이빗 정보에 액세스할 수 있는 임원, 법률 고문 및 인사 관리 직원과 같은 비관리자.

* **[Azure Information Protection 관리자](#azure-information-protection-administrator)** : 이 역할을 가진 사용자는 Azure Information Protection 서비스에 대한 모든 사용 권한을 갖습니다. 이 역할은 Azure Information Protection 정책에 대한 레이블을 구성하고, 보호 템플릿을 관리하고, 보호를 활성화하는 권한을 갖습니다. 이 역할은 ID 보호 센터, Privileged Identity Management, Office 365 Service Health 또는 Office 365 보안 및 준수 센터에서 어느 권한도 부여하지 않습니다.

* **[B2C 사용자 흐름 관리자](#b2c-user-flow-administrator)** : 이 역할을 가진 사용자는 Azure Portal에서 B2C 사용자 흐름 (즉, "기본 제공" 정책)을 만들고 관리할 수 있습니다. 사용자 흐름을 만들거나 편집 하 여 이러한 사용자는 사용자 환경의 html/CSS/javascript 콘텐츠를 변경 하 고, 사용자 흐름 별로 MFA 요구 사항을 변경 하 고, 토큰에서 클레임을 변경 하 고, 테 넌 트의 모든 정책에 대 한 세션 설정을 조정할 수 있습니다. 반면,이 역할에는 사용자 데이터를 검토 하거나 테 넌 트 스키마에 포함 된 특성을 변경할 수 있는 기능이 포함 되지 않습니다. 즉, 사용자 지정 정책에 대 한 변경 내용도이 역할의 범위 밖에 있습니다.

* **[B2C 사용자 흐름 특성 관리자](#b2c-user-flow-attribute-administrator)** : 이 역할을 가진 사용자는 테 넌 트의 모든 사용자 흐름에서 사용할 수 있는 사용자 지정 특성을 추가 하거나 삭제 합니다. 이와 같이이 역할을 가진 사용자는 최종 사용자 스키마에 새 요소를 변경 하거나 추가 하 고, 모든 사용자 흐름의 동작에 영향을 줄 수 있으며 최종 사용자에 게 요청 될 수 있으며 궁극적으로 응용 프로그램에 대 한 클레임으로 전송 되는 데이터에 대 한 변경을 간접적으로 수행할 수 있습니다 이 역할은 사용자 흐름을 편집할 수 없습니다.

* **[B2C IEF 키 집합 관리자](#b2c-ief-keyset-administrator)** :    사용자는 토큰 암호화, 토큰 서명 및 클레임 암호화/암호 해독에 대 한 정책 키와 암호를 만들고 관리할 수 있습니다. 기존 키 컨테이너에 새 키를 추가 하 여 제한 된 관리자는 기존 응용 프로그램에 영향을 주지 않고 필요에 따라 비밀을 롤오버 할 수 있습니다. 이 사용자는 이러한 비밀의 전체 콘텐츠와 만료 날짜를 만든 후에도 볼 수 있습니다.
    
  <b>중요:</b> 중요 한 역할입니다. 키 집합 관리자 역할은 사전 프로덕션 및 프로덕션 중 주의 해 서 신중 하 게 감사 하 고 할당 해야 합니다.

* **[B2C IEF 정책 관리자](#b2c-ief-policy-administrator)** : 이 역할의 사용자는 Azure AD B2C에서 모든 사용자 지정 정책을 만들고, 읽고, 업데이트 하 고, 삭제할 수 있으므로 관련 Azure AD B2C 테 넌 트에서 Id 경험 프레임 워크를 완전히 제어할 수 있습니다. 이 사용자는 정책을 편집 하 여 외부 id 공급자와 직접 페더레이션을 설정 하 고, 디렉터리 스키마를 변경 하 고, 모든 사용자 관련 콘텐츠 (HTML, CSS, JavaScript)를 변경 하 고, 요구 사항을 변경 하 여 인증을 완료 하 고, 새 사용자를 만들고, 보낼 수 있습니다. 전체 마이그레이션을 포함 하는 외부 시스템에 대 한 사용자 데이터 및 암호 및 전화 번호와 같은 중요 한 필드를 비롯 한 모든 사용자 정보를 편집 합니다. 반대로이 역할은 테 넌 트에서 페더레이션에 사용 되는 암호를 편집 하거나 암호화 키를 변경할 수 없습니다.

  <b>중요:</b> B2 IEF 정책 관리자는 프로덕션에서 테 넌 트에 대해 매우 제한 된 방식으로 할당 되어야 하는 매우 중요 한 역할입니다. 특히 프로덕션 테 넌 트의 경우 이러한 사용자의 활동을 면밀 하 게 감사 해야 합니다.

* **[대금 청구 관리자](#billing-administrator)** : 구매, 구독 관리, 지원 티켓 관리 및 서비스 상태 모니터링을 수행할 수 있습니다.

* **[클라우드 애플리케이션 관리자](#cloud-application-administrator)** : 이 역할의 사용자는 애플리케이션 관리자 역할과 동일한 권한을 가집니다. 다만 애플리케이션 프록시를 관리하는 권한은 없습니다. 이 역할은 엔터프라이즈 애플리케이션 및 애플리케이션 등록의 모든 측면을 만들고 관리하는 기능을 부여합니다. 또한 이 역할은 위임된 권한 및 Microsoft Graph와 Azure AD Graph를 제외한 애플리케이션 사용 권한에 동의하는 기능을 부여합니다. 이 역할에 할당 된 사용자는 새 응용 프로그램 등록 또는 엔터프라이즈 응용 프로그램을 만들 때 소유자로 추가 되지 않습니다.

  <b>중요</b>: 이 역할은 애플리케이션 자격 증명을 관리하는 기능을 부여합니다. 이 역할이 할당된 사용자는 애플리케이션에 자격 증명을 추가하고 해당 자격 증명을 사용하여 애플리케이션의 ID를 가장할 수 있습니다. 애플리케이션의 ID에 사용자 또는 다른 개체를 만들거나 업데이트하는 기능과 같이 Azure Active Directory에 대한 액세스 권한이 부여된 경우 이 역할에 할당된 사용자는 애플리케이션을 가장하는 동안 이러한 작업을 수행할 수 있습니다. 애플리케이션의 ID를 가장하는 이 기능은 Azure AD에서 본인의 역할 할당을 통해 사용자가 수행할 수 있는 권한 상승이 될 수 있습니다. 클라우드 애플리케이션 관리자 역할에 사용자를 할당하면 애플리케이션의 ID를 가장하는 기능이 해당 사용자에게 부여된다는 점을 이해해야 합니다.

* **[클라우드 디바이스 관리자](#cloud-device-administrator)** : 이 역할의 사용자는 Azure AD에서 디바이스를 사용하고 사용하지 않도록 설정하며, 삭제하고, Azure Portal에서 Windows 10 BitLocker 키(있는 경우)를 읽을 수 있습니다. 역할은 디바이스에서 다른 속성을 관리하는 사용 권한을 부여하지 않습니다.

* **[규정 준수 관리자](#compliance-administrator)** : 이 역할이 있는 사용자에게는 Microsoft 365 규정 준수 센터, Microsoft 365 관리 센터, Azure, Office 365 보안 및 준수 센터에서 규정 준수 관련 기능을 관리할 권한이 있습니다. 이러한 사용자는 Exchange 관리 센터, 준수 관리자, Teams 및 비즈니스용 Skype 관리 센터 내의 모든 기능을 관리하고 Azure 및 Microsoft 365 관련 지원 티켓을 만들 수도 있습니다. 자세한 내용은 [Office 365 관리자 역할 정보](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)를 참조하세요.

  입력 | 가능한 작업
  ----- | ----------
  [Microsoft 365 규정 준수 센터](https://protection.office.com) | Microsoft 365 서비스 전반에 걸쳐 조직의 데이터 보호 및 관리<br>규정 준수 경고 관리
  [준수 관리자](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | 조직의 규정 준수 활동 추적, 할당, 확인
  [Office 365 보안 및 준수 센터](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | 데이터 거버넌스 관리<br>법적 조사/데이터 조사 수행<br>데이터 주체 요청 관리
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | 모든 Intune 감사 데이터 확인
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | 읽기 전용 권한 소유/경고를 관리할 수 있음<br>파일 정책을 생성/수정하고 파일 거버넌스 작업을 허용할 수 있음<br> 데이터 관리에서 모든 기본 제공 보고서를 확인할 수 있음

* **[준수 데이터 관리자](#compliance-data-administrator)** : 이 역할을 가진 사용자는 Microsoft 365 준수 센터, Microsoft 365 관리 센터 및 Azure에서 데이터를 보호 하 고 추적할 수 있는 권한이 있습니다. 또한 사용자는 Exchange 관리 센터, 준수 관리자, Teams 및 비즈니스용 Skype 관리 센터 내의 모든 기능을 관리하고 Azure 및 Microsoft 365의 지원 티켓을 만들 수 있습니다.

  입력 | 가능한 작업
  ----- | ----------
  [Microsoft 365 규정 준수 센터](https://protection.office.com) | Microsoft 365 서비스에서 준수 관련 정책 모니터링<br>규정 준수 경고 관리
  [준수 관리자](https://docs.microsoft.com/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud) | 조직의 규정 준수 활동 추적, 할당, 확인
  [Office 365 보안 및 준수 센터](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | 데이터 거버넌스 관리<br>법적 조사/데이터 조사 수행<br>데이터 주체 요청 관리
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | 모든 Intune 감사 데이터 확인
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | 읽기 전용 권한 소유/경고를 관리할 수 있음<br>파일 정책을 생성/수정하고 파일 거버넌스 작업을 허용할 수 있음<br> 데이터 관리에서 모든 기본 제공 보고서를 확인할 수 있음

* **[조건부 액세스 관리자](#conditional-access-administrator)** : 이 역할의 사용자는 조건부 액세스 설정을 Azure Active Directory 관리할 수 있습니다.
  > [!NOTE]
  > Azure에서 Exchange ActiveSync 조건부 액세스 정책을 배포 하려면 사용자도 전역 관리자 여야 합니다.
  
* **[고객 Lockbox 액세스 승인자](#customer-lockbox-access-approver)** : 조직에서 [고객 Lockbox 요청](https://docs.microsoft.com/office365/admin/manage/customer-lockbox-requests)을 관리합니다. 이러한 고객 Lockbox 요청에 대한 이메일 알림을 수신하고 Microsoft 365 관리 센터에서 요청을 승인 및 거부할 수 있습니다. 고객 Lockbox 기능을 켜거나 끌 수도 있습니다. 글로벌 관리자만 이 역할에 할당된 사용자의 암호를 재설정할 수 있습니다.
  <!--  This was announced in August of 2018. https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Customer-Lockbox-Approver-Role-Now-Available/ba-p/223393-->

* **[데스크톱 분석 관리자](#desktop-analytics-administrator)** : 이 역할의 사용자는 데스크톱 분석 및 Office 사용자 지정 & 정책 서비스를 관리할 수 있습니다. 데스크톱 분석의 경우 여기에는 자산 인벤토리를 확인 하 고, 배포 계획을 만들고, 배포 및 상태를 볼 수 있는 기능이 포함 됩니다. Office 사용자 지정 & 정책 서비스의 경우이 역할을 통해 사용자는 Office 정책을 관리할 수 있습니다.

* **[장치 관리자](#device-administrators)** : 이 역할은 [디바이스 설정](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/)의 추가 로컬 관리자로서 할당을 위해서만 사용할 수 있습니다. 이 역할을 가진 사용자가 Azure Active Directory에 가입된 모든 Windows 10 디바이스에서 로컬 컴퓨터 관리자가 됩니다. Azure Active Directory의 디바이스 개체를 관리하는 기능이 없습니다. 

* **[디렉터리 읽기 권한자](#directory-readers)** : [승인 프레임 워크](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)를 지원 하지 않는 레거시 응용 프로그램에만 할당 해야 하는 역할입니다. 사용자에 게 할당 하지 않습니다.

* **[디렉터리 동기화 계정](#directory-synchronization-accounts)** : 사용하지 마십시오. 이 역할은 Azure AD Connect 서비스에 자동으로 할당되고 다른 사용에 적합하거나 지원되지 않습니다.

* **[디렉터리 작성자](#directory-writers)** : [동의 프레임워크](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)를 지원하지 않는 애플리케이션에 할당될 레거시 역할입니다. 이 역할은 어느 사용자에게나 할당되면 안 됩니다.

* **[Dynamics 365 관리자/CRM 관리자](#crm-service-administrator)** : 이 역할을 가진 사용자는 해당 서비스가 있는 경우 Microsoft Dynamics 365 Online 내에서 글로벌 사용 권한을 가질 뿐만 아니라 지원 티켓을 관리하고 서비스 상태를 모니터링할 수 있습니다. 자세한 내용은 [서비스 관리자 역할을 사용하여 테넌트 관리](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant)를 참조하세요.
  > [!NOTE] 
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “Dynamics 365 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "Dynamics 365 관리자"입니다.

* **[Exchange 관리자](#exchange-service-administrator)** : 이 역할을 가진 사용자는 해당 서비스가 있는 경우 Microsoft Exchange Online 내에서 전역 권한을 소유합니다. 또한 모든 Office 365 그룹을 만들고 관리하는 기능, 지원 티켓을 관리하는 기능 및 서비스 상태를 모니터링하는 기능도 포함합니다. 자세한 내용은 [Office 365 관리자 역할 정보](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)를 참조하세요.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “Exchange 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "Exchange 관리자"입니다. [Exchange 관리 센터](https://go.microsoft.com/fwlink/p/?LinkID=529144)의 “Exchange Online 관리자”입니다. 

* **[외부 Id 공급자 관리자](#external-identity-provider-administrator)** : 이 관리자는 Azure Active Directory 테 넌 트와 외부 id 공급자 간의 페더레이션을 관리 합니다. 이 역할을 통해 사용자는 새 id 공급자를 추가 하 고 사용 가능한 모든 설정 (예: 인증 경로, 서비스 id, 할당 된 키 컨테이너)을 구성할 수 있습니다. 이 사용자는 테 넌 트가 외부 id 공급자의 인증을 신뢰할 수 있도록 설정할 수 있습니다. 최종 사용자 환경에 미치는 영향은 테 넌 트의 유형에 따라 달라 집니다.
  * 직원 및 파트너에 대 한 Azure Active Directory 테 넌 트: 페더레이션 (예: Gmail) 추가는 아직 회수 되지 않은 모든 게스트 초대에 즉시 영향을 줍니다. [B2B 게스트 사용자에 대 한 id 공급자로 Google 추가](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)를 참조 하세요.
  * Azure Active Directory B2C 테 넌 트: 페더레이션 (예: Facebook 또는 다른 Azure Active Directory) 추가는 id 공급자가 사용자 흐름 (즉, 기본 제공 정책)에서 옵션으로 추가 될 때까지 최종 사용자 흐름에 즉시 영향을 주지 않습니다. 예는 [id 공급자로 Microsoft 계정 구성을](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-msa-app) 참조 하세요. 사용자 흐름을 변경 하려면 "B2C 사용자 흐름 관리자"의 제한 된 역할이 필요 합니다.

* **[글로벌 관리자/회사 관리자](#company-administrator)** : 이 역할의 사용자는 Azure Active Directory의 모든 관리 기능뿐 아니라 Microsoft 365 보안 센터, Microsoft 365 규정 준수 센터, Exchange Online, SharePoint Online 및 비즈니스용 Skype Online과 같이 Azure Active Directory ID를 사용하는 서비스에도 액세스할 수 있습니다. Azure Active Directory 테넌트에 등록하는 사람이 전역 관리자가 됩니다. 전역 관리자만 다른 관리자 역할을 할당할 수 있습니다. 회사에 여러 전역 관리자가 있을 수 있습니다. 전역 관리자는 모든 사용자 및 모든 다른 관리자의 암호를 다시 설정할 수 있습니다.

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 "회사 관리자"로 식별됩니다. [Azure portal](https://portal.azure.com)에서는 "전역 관리자"입니다.
  >
  >

* **[게스트 초대자](#guest-inviter)** : 이 역할의 사용자는 **멤버가 초대할 수 있음** 사용자 설정을 아니요로 설정하는 경우 Azure Active Directory B2B 게스트 사용자 초대를 관리할 수 있습니다. B2B 협업에 대한 자세한 내용은 [Azure AD B2B 협업 정보](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)를 참조하세요. 다른 권한은 포함되지 않습니다.

* **[기술 지원팀 관리자](#helpdesk-administrator)** : 이 역할을 가진 사용자는 암호를 변경하고, 새로 고침 토큰을 무효화하고, 서비스 요청을 관리하며, 서비스 상태를 모니터링할 수 있습니다. 새로 고침 토큰을 무효화하면 사용자가 다시 로그인해야 합니다. 기술 지원팀 관리자는 암호를 다시 설정 하 고 관리자가 아닌 사용자 또는 다음 역할만 할당 된 다른 사용자의 새로 고침 토큰을 무효화할 수 있습니다.
  * 디렉터리 읽기 권한자
  * 게스트 초대자
  * 기술 지원팀 관리자
  * 메시지 센터 읽기 권한자
  * 보고서 읽기 권한자
  
  <b>중요</b>: 이 역할의 사용자는 Azure Active Directory 내부 및 외부에 있는 중요한 프라이빗 정보 또는 중요한 구성에 대한 액세스 권한이 있을 수 있는 사용자의 암호를 변경할 수 있습니다. 사용자의 암호를 변경한다는 것은 사용자의 ID 및 권한을 가정할 수 있다는 것을 의미합니다. 예를 들어:
  * 애플리케이션 등록 및 엔터프라이즈 애플리케이션 소유자: 소유한 앱의 자격 증명을 관리할 수 있습니다. 이러한 앱은 Azure AD에서 권한이 부여되었을 수 있으며, 다른 위치에서는 기술 지원 팀 관리자에 권한이 부여되지 않습니다. 이 경로를 통해 기술 지원 팀 관리자는 애플리케이션 소유자의 ID를 가정하고, 애플리케이션의 자격 증명을 업데이트하여 권한 있는 애플리케이션의 ID를 추가로 가정할 수 있습니다.
  * Azure 구독 소유자: Azure에서 중요한 프라이빗 정보 또는 중요한 구성에 액세스할 수 있습니다.
  * 보안 그룹 및 Office 365 그룹 소유자: 그룹 멤버 자격을 관리할 수 있습니다. 해당 그룹은 중요한 프라이빗 정보 또는 Azure AD 및 다른 위치의 중요한 구성에 대한 액세스 권한을 부여할 수 있습니다.
  * Exchange Online, Office 보안 및 준수 센터, 인사 관리 시스템과 같은 Azure AD 외부의 다른 서비스에 있는 관리자
  * 중요한 프라이빗 정보에 액세스할 수 있는 임원, 법률 고문 및 인사 관리 직원과 같은 비관리자.


  > [!NOTE]
  > [관리 단위 (미리 보기)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units)를 사용 하 여 사용자의 하위 집합에 대 한 관리 권한을 위임 하 고 일부 사용자에 게 정책을 적용 하는 것이 가능 합니다.


  > [!NOTE]
  > 이 역할은 이전에 [Azure Portal](https://portal.azure.com/)"암호 관리자" 라고 했습니다. Azure AD PowerShell, Azure AD Graph API 및 Microsoft Graph API에서 해당 이름과 일치 하도록 이름을 "기술 지원팀 관리자"로 변경 했습니다.

* **[Intune 관리자](#intune-service-administrator)** : 이 역할을 가진 사용자는 해당 서비스가 있는 경우 Microsoft Intune Online 내에서 글로벌 사용 권한을 갖습니다. 또한 이 역할은 정책을 연결하고 그룹을 만들고 관리하기 위해 사용자와 디바이스를 관리하는 기능을 포함합니다. 자세한 내용은 [Microsoft Intune에서 RBAC(역할 기반 관리 제어)](https://docs.microsoft.com/intune/role-based-access-control)를 참조하세요
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “Intune 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "Intune 관리자"입니다.
  
 * **[Kaizala 관리자](#kaizala-administrator)** : 이 역할을 가진 사용자는 서비스가 있는 경우 Microsoft Kaizala 내에서 설정을 관리할 수 있는 전역 권한을 가지 며 지원 티켓을 관리 하 고 서비스 상태를 모니터링할 수 있습니다.
또한 사용자는 조직 구성원 및 Kaizala 작업을 사용 하 여 생성 된 비즈니스 보고서의 Kaizala 사용과 관련 된 보고서 &에 액세스할 수 있습니다. 

* **[라이선스 관리자](#license-administrator)** : 이 역할의 사용자는 사용자 및 그룹의(그룹 기반 라이선스 사용) 라이선스 할당을 추가, 제거 및 업데이트하고, 사용자의 사용 위치를 관리합니다. 이 역할은 사용 위치를 벗어나서 구독을 구매 또는 관리하고, 그룹을 만들거나 관리하고, 사용자를 만들거나 관리하는 기능을 부여하지 않습니다. 이 역할에는 지원 티켓 보기, 생성 또는 관리 권한은 없습니다.

* **[메시지 센터 개인 정보 판독기](#message-center-privacy-reader)** : 이 역할의 사용자는 데이터 개인 정보 메시지를 포함 하 여 메시지 센터의 모든 알림을 모니터링할 수 있습니다. 메시지 센터 개인 정보 읽기 권한자는 데이터 개인 정보 보호와 관련 된 알림을 포함 하 여 전자 메일 알림을 받고 메시지 센터 기본 설정을 사용 하 여 구독을 취소할 전역 관리자 및 메시지 센터 개인 정보 취급 방침만 데이터 프라이버시 메시지를 읽을 수 있습니다. 또한이 역할에는 그룹, 도메인 및 구독을 볼 수 있는 기능이 포함 되어 있습니다. 이 역할에는 서비스 요청을 보거나 만들거나 관리할 수 있는 권한이 없습니다.

* **[메시지 센터 읽기 권한자](#message-center-reader)** : 이 역할의 사용자는 Exchange, Intune 및 Microsoft Teams와 같은 구성된 서비스에서 조직에 대한 [Office 365 메시지 센터](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093)의 알림 및 자문 상태 업데이트를 모니터링할 수 있습니다. 메시지 센터 읽기 권한자는 게시물 및 업데이트 이메일 다이제스트를 매주 수신하며, 메시지 센터 게시물을 Office 365에서 공유할 수 있습니다. Azure AD에서 이 역할에 할당된 사용자는 Azure AD 서비스에서 사용자 및 그룹처럼 읽기 전용 권한만 있습니다. 이 역할에는 지원 티켓 보기, 생성 또는 관리 권한은 없습니다.

* **[파트너 계층1 지원](#partner-tier1-support)** : 사용하지 마십시오. 이 역할은 사용되지 않으며 향후 Azure AD에서 제거됩니다. 이 역할은 적은 수의 Microsoft 전매 파트너에서 사용하기 위한 것으로 일반적인 용도로는 적합하지 않습니다.

* **[파트너 계층2 지원](#partner-tier2-support)** : 사용하지 마십시오. 이 역할은 사용되지 않으며 향후 Azure AD에서 제거됩니다. 이 역할은 적은 수의 Microsoft 전매 파트너에서 사용하기 위한 것으로 일반적인 용도로는 적합하지 않습니다.

* **[암호 관리자](#password-administrator)** : 이 역할의 사용자는 암호를 관리 하는 기능이 제한 됩니다. 이 역할은 서비스 요청을 관리 하거나 서비스 상태를 모니터링 하는 기능을 부여 하지 않습니다. 암호 관리자는 관리자가 아닌 사용자 또는 다음 역할의 구성원 인 다른 사용자의 암호를 다시 설정할 수 있습니다.
  * 디렉터리 읽기 권한자
  * 게스트 초대자
  * 암호 관리자

* **[Power BI 관리자](#power-bi-service-administrator)** : 이 역할을 가진 사용자는 해당 서비스가 있는 경우 Microsoft Power BI 내에서 글로벌 사용 권한을 가질 뿐만 아니라 지원 티켓을 관리하고 서비스 상태를 모니터링할 수 있습니다. 자세한 내용은 [Power BI 관리자 역할 이해](https://docs.microsoft.com/power-bi/service-admin-role)를 참조하세요.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “Power BI 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "Power BI 관리자"입니다.

* **[권한 있는 인증 관리자](#privileged-authentication-administrator)** : 이 역할을 가진 사용자는 전역 관리자를 비롯 한 모든 사용자에 대해 암호 이외의 자격 증명을 설정 하거나 다시 설정할 수 있으며 모든 사용자에 대 한 암호를 업데이트할 수 있습니다. 권한 있는 인증 관리자는 사용자가 기존 비 암호 자격 증명 (예: MFA, FIDO)에 다시 등록 하 고 ' 장치에서 MFA 기억을 기억을 ' 하 여 모든 사용자의 다음 로그인에 MFA를 묻는 메시지를 표시 하도록 할 수 있습니다. 권한 있는 인증 관리자는 다음을 수행할 수 있습니다.
  * 사용자가 기존의 암호 없는 자격 증명 (예: MFA, FIDO)에 다시 등록 하도록 합니다.
  * 다음 로그인 시 MFA를 묻는 메시지를 표시 하 고 ' 장치에서 MFA를 잊지 마세요. '를 취소 합니다.

* **[권한 있는 역할 관리자](#privileged-role-administrator)** : 이 역할을 가진 사용자는 Azure Active Directory 및 Azure AD Privileged Identity Management에서 역할 할당을 관리할 수 있습니다. 또한이 역할을 통해 Privileged Identity Management 및 관리 단위의 모든 측면을 관리할 수 있습니다.

  <b>중요</b>: 이 역할은 전역 관리자 역할을 비롯 한 모든 Azure AD 역할에 대 한 할당을 관리할 수 있는 기능을 부여 합니다. 이 역할은 사용자 생성 또는 업데이트와 같은 Azure AD의 다른 모든 권한 있는 기능을 포함하지는 않습니다. 그러나 이 역할에 할당된 사용자는 추가 역할을 할당하여 본인 또는 다른 사용자에게 추가 권한을 부여할 수 있습니다.

* **[보고서 읽기 권한자](#reports-reader)** : 이 역할을 가진 사용자는 Microsoft 365 관리 센터의 사용 현황 보고 데이터 및 보고서 대시보드와 Power BI의 채택 컨텍스트 팩을 볼 수 있습니다. 또한 역할은 Azure AD의 로그온 보고서 및 활동과 함께 Microsoft Graph Reporting API에서 반환되는 데이터에 대한 액세스를 제공합니다. 보고서 구독자 역할에 할당된 사용자는 관련된 사용량 및 채택 메트릭에만 액세스할 수 있습니다. 설정을 구성하거나 Exchange와 같은 제품 특정 관리 센터에 액세스할 수 있는 관리자 권한은 없습니다. 이 역할에는 지원 티켓 보기, 생성 또는 관리 권한은 없습니다.

* **[검색 관리자](#search-administrator)** : 이 역할의 사용자는 Microsoft 365 관리 센터의 모든 Microsoft Search 관리 기능에 대 한 모든 권한을 갖습니다. 검색 관리자는 검색 관리자 및 검색 편집기 역할을 사용자에 게 위임 하 고, 책갈피, Q &와 같은 콘텐츠를 만들고 관리할 수 있습니다. 또한 이러한 사용자는 메시지 센터를 확인 하 고, 서비스 상태를 모니터링 하 고, 서비스 요청을 만들 수 있습니다.

* **[검색 편집기](#search-editor)** : 이 역할의 사용자는 책갈피, Q & 및 위치를 포함 하 여 Microsoft 365 관리 센터에서 Microsoft Search에 대 한 콘텐츠를 만들고 관리 하 고 삭제할 수 있습니다.

* **[보안 관리자](#security-administrator)** : 이 역할의 사용자에게는 Microsoft 365 보안 센터, Azure Active Directory ID 보호, Azure Information Protection, Office 365 보안 및 준수 센터의 보안 관련 기능 관리 권한이 있습니다. Office 365 사용 권한에 대한 자세한 정보는 [Office 365 보안 및 규정 준수 센터의 사용 권한](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)에서 제공됩니다.
  
  그런 다음 | 가능한 작업
  --- | ---
  [Microsoft 365 보안 센터](https://protection.office.com) | Microsoft 365 서비스 전반의 보안 관련 정책 모니터링<br>보안 위협과 경고 관리<br>보고서 보기
  ID 보호 센터 | 보안 읽기 권한자 역할의 모든 권한<br>암호 재설정을 제외한 모든 ID 보호 센터 작업을 수행하는 기능도 있음
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | 보안 읽기 권한자 역할의 모든 권한<br>Azure AD 역할 할당 또는 설정을 관리할 **수 없습니다** .
  [Office 365 보안 및 준수 센터](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | 보안 정책 관리<br>보안 위협 확인/조사/대응<br>보고서 보기
  Azure Advanced Threat Protection | 의심스러운 보안 활동 모니터링/대응
  Windows Defender ATP 및 EDR | 역할 할당<br>머신 그룹 관리<br>엔드포인트 위협 검색 및 자동 수정 구성<br>경고 확인/조사/대응
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | 사용자, 디바이스, 등록, 구성 및 애플리케이션 정보 확인<br>Intune을 변경할 수는 없음
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | 관리자/정책/설정 추가, 로그 업로드, 거버넌스 작업 수행
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | 보안 정책 보기, 보안 상태 보기, 보안 정책 편집, 경고 및 권장 사항 보기, 경고 및 권장 사항 해제를 수행합니다.
  [Office 365 서비스 상태](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 서비스의 상태 확인

* **[보안 운영자](#security-operator)** : 이 역할을 가진 사용자는 경고를 관리 하 고 보안 관련 기능에 대 한 전역 읽기 전용 액세스 권한을 가질 수 있습니다. 여기에는 Microsoft 365 security center, Azure Active Directory, Id 보호, Privileged Identity Management 및 Office 365의 모든 정보가 포함 됩니다. 보안 및 준수 센터. Office 365 사용 권한에 대한 자세한 정보는 [Office 365 보안 및 규정 준수 센터의 사용 권한](https://docs.microsoft.com/office365/securitycompliance/permissions-in-the-security-and-compliance-center)에서 제공됩니다.

  그런 다음 | 가능한 작업
  --- | ---
  [Microsoft 365 보안 센터](https://protection.office.com) | 보안 읽기 권한자 역할의 모든 권한<br>보안 위협 경고 보기, 조사 및 대응
  ID 보호 센터 | 보안 읽기 권한자 역할의 모든 권한<br>암호 재설정을 제외한 모든 ID 보호 센터 작업을 수행하는 기능도 있음
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | 보안 읽기 권한자 역할의 모든 권한
  [Office 365 보안 및 준수 센터](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | 보안 읽기 권한자 역할의 모든 권한<br>보안 경고 보기, 조사 및 응답
  Windows Defender ATP 및 EDR | 보안 읽기 권한자 역할의 모든 권한<br>보안 경고 보기, 조사 및 응답
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | 보안 읽기 권한자 역할의 모든 권한
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | 보안 읽기 권한자 역할의 모든 권한
  [Office 365 서비스 상태](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 서비스의 상태 확인
<!--* **[Security Operator](#security-operator)**: Users with this role can manage alerts and have global read-only access on security-related feature, including all information in Microsoft 365 security center, Azure Active Directory, Identity Protection, Privileged Identity Management.-->

* **[보안 읽기 권한자](#security-reader)** : 이 역할의 사용자에게는 Microsoft 365 보안 센터, Azure Active Directory, ID 보호, Privileged Identity Management의 모든 정보를 비롯한 보안 관련 기능에 대한 전역 읽기 전용 액세스 권한이 있습니다. 또한 Azure Active Directory 로그인 보고서와 감사 로그 읽기 권한 및 Office 365 보안 및 준수 센터의 읽기 권한도 있습니다. Office 365 사용 권한에 대한 자세한 정보는 [Office 365 보안 및 규정 준수 센터의 사용 권한](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)에서 제공됩니다.

  그런 다음 | 가능한 작업
  --- | ---
  [Microsoft 365 보안 센터](https://protection.office.com) | Microsoft 365 서비스 전반에서 보안 관련 정책 확인<br>보안 위협 및 경고 확인<br>보고서 보기
  ID 보호 센터 | 모든 보안 보고서 및 보안 기능에 대한 설정 정보를 읽습니다.<br><ul><li>스팸 방지<li>암호화<li>데이터 손실 방지<li>맬웨어 방지<li>Advanced Threat Protection<li>피싱 방지<li>메일 흐름 규칙
  [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) | Azure AD PIM에 표시되는 다음과 같은 모든 정보에 대한 읽기 전용 액세스 권한을 갖습니다. Azure AD 역할 할당에 대한 정책 및 보고서, 보안 검토, Azure AD 역할 할당 외의 시나리오에 대한 정책 데이터 및 보고서에 대한 향후 읽기 액세스 권한<br>Azure AD PIM에 로그인을 하거나 어떠한 변경도 할 수 **없습니다**. PIM 포털 또는 PowerShell을 통해이 역할의 누군가가 사용자에 게 적합 한 경우 추가 역할 (예: 전역 관리자 또는 권한 있는 역할 관리자)을 활성화할 수 있습니다.
  [Office 365 보안 및 준수 센터](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) | 보안 정책 보기<br>보안 위협 확인/조사<br>보고서 보기
  Windows Defender ATP 및 EDR | 경고 확인/조사
  [Intune](https://docs.microsoft.com/intune/role-based-access-control) | 사용자, 디바이스, 등록, 구성 및 애플리케이션 정보 확인. Intune을 변경할 수는 없음
  [Cloud App Security](https://docs.microsoft.com/cloud-app-security/manage-admins) | 읽기 전용 권한 소유/경고를 관리할 수 있음
  [Azure Security Center](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) | 권장 사항 및 경고를 보고, 보안 정책을 보고, 보안 상태를 볼 수 있지만 변경할 수는 없습니다.
  [Office 365 서비스 상태](https://docs.microsoft.com/office365/enterprise/view-service-health) | Office 365 서비스의 상태 확인

* **[서비스 지원 관리자](#service-support-administrator)** : 이 역할을 가진 사용자는 Azure 및 Office 365 서비스에 대해 Microsoft를 사용 하 여 지원 요청을 열 수 있으며, [Azure Portal](https://portal.azure.com) 및 [Microsoft 365 관리 센터](https://admin.microsoft.com)에서 서비스 대시보드와 메시지 센터를 볼 수 있습니다. 자세한 내용은 [Office 365 관리자 역할 정보](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)를 참조하세요.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 "서비스 지원 관리자"로 표시됩니다. [Azure Portal](https://portal.azure.com), [Microsoft 365 관리 센터](https://admin.microsoft.com)및 Intune 포털에서 "서비스 관리자"입니다.

* **[SharePoint 관리자](#sharepoint-service-administrator)** : 이 역할의 사용자는 해당 서비스가 있는 경우 Microsoft SharePoint Online 내에서 글로벌 사용 권한을 가질 뿐만 아니라 모든 Office 365 그룹을 생성 및 관리하고, 지원 티켓을 관리하고, 서비스 상태를 모니터링할 수 있습니다. 자세한 내용은 [Office 365 관리자 역할 정보](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)를 참조하세요.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “SharePoint 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "SharePoint 관리자"입니다.

* **[비즈니스용 Skype/Lync 관리자](#lync-service-administrator)** : 이 역할의 사용자는 해당 서비스가 있는 경우 Microsoft 비즈니스용 Skype 내에서 글로벌 사용 권한을 가질 뿐만 아니라 Azure Active Directory에서 Skype 관련 사용자 특성을 관리할 수 있습니다. 또한 이 역할은 지원 티켓을 관리하고 서비스 상태를 모니터링하고, Teams 및 Business용 Skype 관리 센터에 액세스하는 기능을 부여합니다. 계정에는 Teams에 대한 라이선스가 있어야 합니다. 그렇지 않으면 Teams PowerShell cmdlet을 실행할 수 없습니다. [비즈니스용 Skype 관리자 역할 정보](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5)의 자세한 내용 및 [비즈니스용 Skype 및 Microsoft Teams 추가 기능 라이선스](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)의 Teams 라이선스 정보

  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 "Lync Service 관리자"로 식별됩니다. [Azure Portal](https://portal.azure.com/)에서는 “비즈니스용 Skype 관리자”입니다.

* **[Teams 관리자](#teams-service-administrator)** : 이 역할의 사용자는 Microsoft Teams 및 비즈니스용 Skype 관리 센터와 해당하는 PowerShell 모듈을 통해 Microsoft Teams 워크로드의 모든 측면을 관리할 수 있습니다. 여기에는 다른 영역 중 전화 통신, 메시징, 회의 및 팀 자체와 관련된 모든 관리 도구가 포함됩니다. 이 역할은 추가적으로 모든 Office 365 그룹 만들기 및 관리 기능뿐만 아니라 지원 티켓을 관리하고 서비스 상태를 모니터링하는 기능도 부여합니다.
  > [!NOTE]
  > Microsoft Graph API, Azure AD Graph API 및 Azure AD PowerShell에서 이 역할은 “Teams 서비스 관리자”로 식별됩니다. [Azure Portal](https://portal.azure.com)에서 이 역할은 "Teams 관리자"입니다.

* **[Teams 통신 관리자](#teams-communications-administrator)** : 이 역할의 사용자는 음성 및 전화 통신과 관련된 Microsoft Teams 워크로드의 측면을 관리할 수 있습니다. 여기에는 전화 번호 할당, 음성 및 회의 정책 및 호출 분석 도구 집합에 대한 전체 액세스를 위한 관리 도구가 포함됩니다.

* **[Teams 통신 지원 엔지니어](#teams-communications-support-engineer)** : 이 역할의 사용자는 Microsoft Teams 및 비즈니스용 Skype 관리 센터에서 사용자 호출 문제 해결 도구를 사용하여 Microsoft Teams 및 비즈니스용 Skype 내에서 통신 문제를 해결할 수 있습니다. 이 역할의 사용자는 관련된 모든 참가자에 대한 전체 호출 레코드 정보를 볼 수 있습니다. 이 역할에는 지원 티켓 보기, 생성 또는 관리 권한은 없습니다.

* **[Teams 통신 지원 전문가](#teams-communications-support-specialist)** : 이 역할의 사용자는 Microsoft Teams 및 비즈니스용 Skype 관리 센터에서 사용자 호출 문제 해결 도구를 사용하여 Microsoft Teams 및 비즈니스용 Skype 내에서 통신 문제를 해결할 수 있습니다. 이 역할의 사용자는 조회하는 특정 사용자에 대한 호출에서 사용자 세부 정보를 보기만 할 수 있습니다. 이 역할에는 지원 티켓 보기, 생성 또는 관리 권한은 없습니다.

* **[사용자 관리자](#user-administrator)** : 이 역할을 가진 사용자는 사용자를 만들고 몇 가지 제한 사항이 있는 사용자의 모든 측면을 관리할 수 있습니다 (아래 참조). 암호 만료 정책을 업데이트할 수 있습니다. 또한 이 역할의 사용자는 모든 그룹을 만들고 관리할 수 있습니다. 이 역할은 사용자 보기를 만들고 관리하며, 지원 티켓을 관리하고, 서비스 상태를 모니터링하는 기능도 포함합니다.

  | | |
  | --- | --- |
  |일반적인 사용 권한|<p>사용자 및 그룹 만들기</p><p>사용자 보기 만들기 및 관리</p><p>Office 지원 티켓 관리<p>암호 만료 정책 업데이트|
  |<p>모든 관리자를 포함한 모든 사용자에게</p>|<p>라이선스 관리</p><p>사용자 계정 이름을 제외한 모든 사용자 속성 관리</p>
  |비관리자 또는 다음의 제한된 관리자 역할의 사용자에만 적용:<ul><li>디렉터리 읽기 권한자<li>게스트 초대자<li>기술 지원팀 관리자<li>메시지 센터 읽기 권한자<li>보고서 읽기 권한자<li>사용자 관리자|<p>삭제 및 복원</p><p>사용 안 함 및 사용</p><p>새로 고침 토큰 무효화</p><p>사용자 계정 이름을 포함한 모든 사용자 속성 관리</p><p>암호 다시 설정</p><p>(FIDO) 디바이스 키 업데이트</p>
  
  <b>중요</b>: 이 역할의 사용자는 Azure Active Directory 내부 및 외부에 있는 중요한 프라이빗 정보 또는 중요한 구성에 대한 액세스 권한이 있을 수 있는 사용자의 암호를 변경할 수 있습니다. 사용자의 암호를 변경한다는 것은 사용자의 ID 및 권한을 가정할 수 있다는 것을 의미합니다. 예:
  * 애플리케이션 등록 및 엔터프라이즈 애플리케이션 소유자: 소유한 앱의 자격 증명을 관리할 수 있습니다. 이러한 앱은 Azure AD에서 권한이 부여되었을 수 있으며, 다른 위치에서는 사용자 관리자에게 권한이 부여되지 않습니다. 이 경로를 통해 사용자 관리자는 애플리케이션 소유자의 ID를 가정하고, 애플리케이션의 자격 증명을 업데이트하여 권한 있는 애플리케이션의 ID를 추가로 가정할 수 있습니다.
  * Azure 구독 소유자: Azure에서 중요한 프라이빗 정보 또는 중요한 구성에 액세스할 수 있습니다.
  * 보안 그룹 및 Office 365 그룹 소유자: 그룹 멤버 자격을 관리할 수 있습니다. 해당 그룹은 중요한 프라이빗 정보 또는 Azure AD 및 다른 위치의 중요한 구성에 대한 액세스 권한을 부여할 수 있습니다.
  * Exchange Online, Office 보안 및 준수 센터, 인사 관리 시스템과 같은 Azure AD 외부의 다른 서비스에 있는 관리자
  * 중요한 프라이빗 정보에 액세스할 수 있는 임원, 법률 고문 및 인사 관리 직원과 같은 비관리자.

## <a name="role-permissions"></a>역할 권한
다음 표에서는 각 역할에 부여되는 Azure Active Directory의 특정 권한에 대해 설명합니다. 일부 역할에는 Azure Active Directory 외부의 Microsoft 서비스에 대한 추가 사용 권한이 있을 수 있습니다.

### <a name="application-administrator"></a>애플리케이션 관리자
앱 등록 및 엔터프라이즈 앱의 모든 측면을 만들고 관리할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Azure Active Directory에서 applications.audience 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/authentication/update | Azure Active Directory에서 applications.authentication 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/basic/update | Azure Active Directory에서 애플리케이션의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/create | Azure Active Directory에서 애플리케이션을 만듭니다. |
| microsoft.aad.directory/applications/credentials/update | Azure Active Directory에서 applications.credentials 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/delete | Azure Active Directory에서 애플리케이션을 삭제합니다. |
| microsoft.aad.directory/applications/owners/update | Azure Active Directory에서 applications.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/permissions/update | Azure Active Directory에서 applications.permissions 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/policies/update | Azure Active Directory에서 applications.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory에서 appRoleAssignments를 만듭니다. |
| microsoft.aad.directory/appRoleAssignments/read | Azure Active Directory에서 appRoleAssignments를 읽습니다. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory에서 appRoleAssignments를 업데이트합니다. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory에서 appRoleAssignments를 삭제합니다. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory에서 policies.applicationConfiguration 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory에서 정책을 만듭니다. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory에서 정책을 삭제합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory에서 policies.applicationConfiguration 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory에서 servicePrincipals.appRoleAssignedTo 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory에서 servicePrincipals.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory에서 servicePrincipals.audience 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory에서 servicePrincipals.authentication 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory에서 servicePrincipals의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory에서 servicePrincipals를 만듭니다. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory에서 servicePrincipals.credentials 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory에서 servicePrincipals를 삭제합니다. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory에서 servicePrincipals.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory에서 servicePrincipals.permissions 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory에서 servicePrincipals.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="application-developer"></a>애플리케이션 개발자
‘사용자가 애플리케이션을 등록할 수 있음’ 설정에 관계없이 애플리케이션 등록을 만들 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | Azure Active Directory에서 애플리케이션을 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | Azure Active Directory에서 appRoleAssignments를 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | Azure Active Directory에서 oAuth2PermissionGrants를 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/servicePrincipals/createAsOwner | Azure Active Directory에서 servicePrincipals를 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |

### <a name="authentication-administrator"></a>인증 관리자
관리 사용자가 아닌 사용자의 인증 방법 정보를 보고, 설정하고, 다시 설정할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/strongAuthentication/update | MFA 자격 증명 정보와 같은 강력한 인증 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.aad.directory/users/password/update | Office 365 조직의 모든 사용자에 대 한 암호를 업데이트 합니다. 자세한 내용은 온라인 설명서를 참조하세요. |

### <a name="azure-information-protection-administrator"></a>Azure Information Protection 관리자
Azure Information Protection 서비스의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection의 모든 측면을 관리합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="b2c-user-flow-administrator"></a>B2C 사용자 흐름 관리자
사용자 흐름의 모든 측면을 만들고 관리 합니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.b2c/userFlows/allTasks | Azure Active Directory B2C에서 사용자 흐름을 읽고 구성 합니다. |

### <a name="b2c-user-flow-attribute-administrator"></a>B2C 사용자 흐름 특성 관리자
모든 사용자 흐름에서 사용할 수 있는 특성 스키마를 만들고 관리 합니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.b2c/userAttributes/allTasks | Azure Active Directory B2C에서 사용자 특성을 읽고 구성 합니다. |

### <a name="b2c-ief-keyset-administrator"></a>B2C IEF 키 집합 관리자
Id 경험 프레임 워크에서 페더레이션 및 암호화에 대 한 암호를 관리 합니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/keySets/allTasks | Azure Active Directory B2C에서 키 집합을 읽고 구성 합니다. |

### <a name="b2c-ief-policy-administrator"></a>B2C IEF 정책 관리자
Id 경험 프레임 워크에서 신뢰 프레임 워크 정책을 만들고 관리 합니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.b2c/trustFramework/policies/allTasks | Azure Active Directory B2C에서 사용자 지정 정책을 읽고 구성 합니다. |

### <a name="billing-administrator"></a>대금 청구 관리자
결제 정보 업데이트와 같은 일반 결제 관련 작업을 수행할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | Azure Active Directory에서 조직의 기본 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 청구의 모든 측면을 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |


### <a name="cloud-application-administrator"></a>클라우드 애플리케이션 관리자
앱 프록시를 제외한 앱 등록 및 엔터프라이즈 앱의 모든 측면을 만들고 관리할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | Azure Active Directory에서 applications.audience 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/authentication/update | Azure Active Directory에서 applications.authentication 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/basic/update | Azure Active Directory에서 애플리케이션의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/create | Azure Active Directory에서 애플리케이션을 만듭니다. |
| microsoft.aad.directory/applications/credentials/update | Azure Active Directory에서 applications.credentials 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/delete | Azure Active Directory에서 애플리케이션을 삭제합니다. |
| microsoft.aad.directory/applications/owners/update | Azure Active Directory에서 applications.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/permissions/update | Azure Active Directory에서 applications.permissions 속성을 업데이트합니다. |
| microsoft.aad.directory/applications/policies/update | Azure Active Directory에서 applications.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory에서 appRoleAssignments를 만듭니다. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory에서 appRoleAssignments를 업데이트합니다. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory에서 appRoleAssignments를 삭제합니다. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/create | Azure Active Directory에서 정책을 만듭니다. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | Azure Active Directory에서 policies.applicationConfiguration 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/delete | Azure Active Directory에서 정책을 삭제합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | Azure Active Directory에서 policies.applicationConfiguration 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | Azure Active Directory에서 policies.applicationConfiguration 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory에서 servicePrincipals.appRoleAssignedTo 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory에서 servicePrincipals.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory에서 servicePrincipals.audience 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory에서 servicePrincipals.authentication 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory에서 servicePrincipals의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory에서 servicePrincipals를 만듭니다. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory에서 servicePrincipals.credentials 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/delete | Azure Active Directory에서 servicePrincipals를 삭제합니다. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory에서 servicePrincipals.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory에서 servicePrincipals.permissions 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory에서 servicePrincipals.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="cloud-device-administrator"></a>클라우드 디바이스 관리자
Azure AD에서 디바이스를 관리하기 위한 모든 권한입니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory에서 devices.bitLockerRecoveryKeys 속성을 읽습니다. |
| microsoft.aad.directory/devices/delete | Azure Active Directory에서 디바이스를 삭제합니다. |
| microsoft.aad.directory/devices/disable | Azure Active Directory에서 디바이스를 사용하지 않도록 설정합니다. |
| microsoft.aad.directory/devices/enable | Azure Active Directory에서 디바이스를 사용하도록 설정합니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="company-administrator"></a>회사 관리자
Azure AD 및 Azure AD ID를 사용하는 Microsoft 서비스의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | microsoft.aad.cloudAppSecurity에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | Azure Active Directory에서 administrativeUnits를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/applications/allProperties/allTasks | Azure Active Directory에서 애플리케이션을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | Azure Active Directory에서 appRoleAssignments를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/contacts/allProperties/allTasks | Azure Active Directory에서 연락처를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/contracts/allProperties/allTasks | Azure Active Directory에서 계약을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/devices/allProperties/allTasks | Azure Active Directory에서 디바이스를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | Azure Active Directory에서 directoryRoles를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | Azure Active Directory에서 directoryRoleTemplates를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/domains/allProperties/allTasks | Azure Active Directory에서 도메인을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/groups/allProperties/allTasks | Azure Active Directory에서 그룹을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | Azure Active Directory에서 groupSettings를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | Azure Active Directory에서 groupSettingTemplates를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | Azure Active Directory에서 loginTenantBranding을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | Azure Active Directory에서 oAuth2PermissionGrants를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/organization/allProperties/allTasks | Azure Active Directory에서 조직을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/policies/allProperties/allTasks | Azure Active Directory에서 정책을 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | Azure Active Directory에서 roleAssignments를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | Azure Active Directory에서 roleDefinitions를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | Azure Active Directory에서 scopedRoleMemberships를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/serviceAction/activateService | Azure Active Directory에서 Activateservice 서비스 작업을 수행할 수 있습니다. |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | Azure Active Directory에서 Disabledirectoryfeature 서비스 작업을 수행할 수 있습니다. |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | Azure Active Directory에서 Enabledirectoryfeature 서비스 작업을 수행할 수 있습니다. |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | Azure Active Directory에서 Getavailableextentionproperties 서비스 작업을 수행할 수 있습니다. |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | Azure Active Directory에서 servicePrincipals를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | Azure Active Directory에서 subscribedSkus를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/users/allProperties/allTasks | Azure Active Directory에서 사용자를 만들고 삭제하고, 모든 속성을 읽고 업데이트합니다. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect에서 모든 작업을 수행합니다. |
| microsoft.aad.identityProtection/allEntities/allTasks | microsoft.aad.identityProtection에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement의 모든 리소스를 읽습니다. |
| microsoft.azure.advancedThreatProtection/allEntities/read | microsoft.azure.advancedThreatProtection에서 모든 리소스를 읽습니다. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection의 모든 측면을 관리합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.commerce.billing/allEntities/allTasks | Office 365 청구의 모든 측면을 관리합니다. |
| microsoft.intune/allEntities/allTasks | Intune의 모든 측면을 관리합니다. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 준수 관리자의 모든 측면을 관리합니다. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics의 모든 측면을 관리합니다. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online의 모든 측면을 관리합니다. |
| microsoft.office365.lockbox/allEntities/allTasks | Office 365 고객 Lockbox의 모든 측면을 관리합니다. |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter에서 메시지를 읽습니다. |
| microsoft.office365.messageCenter/securityMessages/read | microsoft.office365.messageCenter의 securityMessages를 읽습니다. |
| microsoft.office365.protectionCenter/allEntities/allTasks | Office 365 보호 센터의 모든 측면을 관리합니다. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | microsoft.office365.securityComplianceCenter에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.sharepoint/allEntities/allTasks | microsoft.office365.sharepoint에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 비즈니스용 Skype Online의 모든 측면을 관리합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스의 기본 속성을 읽습니다. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365의 모든 측면을 관리합니다. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI의 모든 측면을 관리합니다. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | microsoft.windows.defenderAdvancedThreatProtection에서 모든 리소스를 읽습니다. |

### <a name="compliance-administrator"></a>준수 관리자
Azure AD 및 Office 365의 준수 구성 및 보고서를 읽고 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 준수 관리자의 모든 측면을 관리합니다. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online의 모든 측면을 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.sharepoint/allEntities/allTasks | microsoft.office365.sharepoint에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 비즈니스용 Skype Online의 모든 측면을 관리합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="compliance-data-administrator"></a>준수 데이터 관리자
규정 준수 콘텐츠를 만들고 관리 합니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security를 읽고 구성 합니다. |
| microsoft.azure.informationProtection/allEntities/allTasks | Azure Information Protection의 모든 측면을 관리합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.complianceManager/allEntities/allTasks | Office 365 준수 관리자의 모든 측면을 관리합니다. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online의 모든 측면을 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.sharepoint/allEntities/allTasks | microsoft.office365.sharepoint에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 비즈니스용 Skype Online의 모든 측면을 관리합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="conditional-access-administrator"></a>조건부 액세스 관리자
조건부 액세스 기능을 관리할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | Azure Active Directory에서 policies.conditionalAccess 속성을 읽습니다. |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | Azure Active Directory에서 policies.conditionalAccess 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/conditionalAccess/create | Azure Active Directory에서 정책을 만듭니다. |
| microsoft.aad.directory/policies/conditionalAccess/delete | Azure Active Directory에서 정책을 삭제합니다. |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | Azure Active Directory에서 policies.conditionalAccess 속성을 읽습니다. |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | Azure Active Directory에서 policies.conditionalAccess 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | Azure Active Directory에서 policies.conditionalAccess 속성을 읽습니다. |
| microsoft.aad.directory/policies/conditionalAccess/tenantDefault/update | Azure Active Directory에서 policies.conditionalAccess 속성을 업데이트합니다. |

### <a name="crm-service-administrator"></a>CRM 서비스 관리자
Dynamics 365 제품의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.powerApps.dynamics365/allEntities/allTasks | Dynamics 365의 모든 측면을 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="customer-lockbox-access-approver"></a>고객 LockBox 액세스 승인자
고객 조직 데이터에 액세스하려는 Microsoft 지원 요청을 승인할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.lockbox/allEntities/allTasks | Office 365 고객 Lockbox의 모든 측면을 관리합니다. |

### <a name="desktop-analytics-administrator"></a>데스크톱 분석 관리자
는 데스크톱 분석 및 Office 사용자 지정 & 정책 서비스를 관리할 수 있습니다. 데스크톱 분석의 경우 여기에는 자산 인벤토리를 확인 하 고, 배포 계획을 만들고, 배포 및 상태를 볼 수 있는 기능이 포함 됩니다. Office 사용자 지정 & 정책 서비스의 경우이 역할을 통해 사용자는 Office 정책을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | Desktop Analytics의 모든 측면을 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="device-administrators"></a>디바이스 관리자
이 역할에 할당 된 사용자는 Azure AD 조인 장치에서 로컬 관리자 그룹에 추가 됩니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory에서 groupSettings의 기본 속성을 읽습니다. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory에서 groupSettingTemplates의 기본 속성을 읽습니다. |

### <a name="directory-readers"></a>디렉터리 읽기 권한자
기본 디렉터리 정보를 읽을 수 있습니다. 애플리케이션에 대한 액세스 권한은 사용자를 위한 것이 아닙니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | Azure Active Directory에서 administrativeUnits의 기본 속성을 읽습니다. |
| microsoft.aad.directory/administrativeUnits/members/read | Azure Active Directory에서 administrativeUnits.members 속성을 읽습니다. |
| microsoft.aad.directory/applications/basic/read | Azure Active Directory에서 애플리케이션의 기본 속성을 읽습니다. |
| microsoft.aad.directory/applications/owners/read | Azure Active Directory에서 applications.owners 속성을 읽습니다. |
| microsoft.aad.directory/applications/policies/read | Azure Active Directory에서 applications.policies 속성을 읽습니다. |
| microsoft.aad.directory/contacts/basic/read | Azure Active Directory에서 연락처의 기본 속성을 읽습니다. |
| microsoft.aad.directory/contacts/memberOf/read | Azure Active Directory에서 contacts.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/contracts/basic/read | Azure Active Directory에서 계약의 기본 속성을 읽습니다. |
| microsoft.aad.directory/devices/basic/read | Azure Active Directory에서 디바이스의 기본 속성을 읽습니다. |
| microsoft.aad.directory/devices/memberOf/read | Azure Active Directory에서 devices.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/devices/registeredOwners/read | Azure Active Directory에서 devices.registeredOwners 속성을 읽습니다. |
| microsoft.aad.directory/devices/registeredUsers/read | Azure Active Directory에서 devices.registeredUsers 속성을 읽습니다. |
| microsoft.aad.directory/directoryRoles/basic/read | Azure Active Directory에서 directoryRoles의 기본 속성을 읽습니다. |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | Azure Active Directory에서 directoryRoles.eligibleMembers 속성을 읽습니다. |
| microsoft.aad.directory/directoryRoles/members/read | Azure Active Directory에서 directoryRoles.members 속성을 읽습니다. |
| microsoft.aad.directory/domains/basic/read | Azure Active Directory에서 도메인의 기본 속성을 읽습니다. |
| microsoft.aad.directory/groups/appRoleAssignments/read | Azure Active Directory에서 groups.appRoleAssignments 속성을 읽습니다. |
| microsoft.aad.directory/groups/basic/read | Azure Active Directory에서 그룹의 기본 속성을 읽습니다. |
| microsoft.aad.directory/groups/memberOf/read | Azure Active Directory에서 groups.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/groups/members/read | Azure Active Directory에서 groups.members 속성을 읽습니다. |
| microsoft.aad.directory/groups/owners/read | Azure Active Directory에서 groups.owners 속성을 읽습니다. |
| microsoft.aad.directory/groups/settings/read | Azure Active Directory에서 groups.settings 속성을 읽습니다. |
| microsoft.aad.directory/groupSettings/basic/read | Azure Active Directory에서 groupSettings의 기본 속성을 읽습니다. |
| microsoft.aad.directory/groupSettingTemplates/basic/read | Azure Active Directory에서 groupSettingTemplates의 기본 속성을 읽습니다. |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | Azure Active Directory에서 oAuth2PermissionGrants의 기본 속성을 읽습니다. |
| microsoft.aad.directory/organization/basic/read | Azure Active Directory에서 조직의 기본 속성을 읽습니다. |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | Azure Active Directory에서 organization.trustedCAsForPasswordlessAuth 속성을 읽습니다. |
| microsoft.aad.directory/roleAssignments/basic/read | Azure Active Directory에서 roleAssignments의 기본 속성을 읽습니다. |
| microsoft.aad.directory/roleDefinitions/basic/read | Azure Active Directory에서 roleDefinitions의 기본 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory에서 servicePrincipals.appRoleAssignedTo 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory에서 servicePrincipals.appRoleAssignments 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory에서 servicePrincipals의 기본 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory에서 servicePrincipals.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory에서 servicePrincipals.oAuth2PermissionGrants 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory에서 servicePrincipals.ownedObjects 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory에서 servicePrincipals.owners 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory에서 servicePrincipals.policies 속성을 읽습니다. |
| microsoft.aad.directory/subscribedSkus/basic/read | Azure Active Directory에서 subscribedSkus의 기본 속성을 읽습니다. |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory에서 users.appRoleAssignments 속성을 읽습니다. |
| microsoft.aad.directory/users/basic/read | Azure Active Directory에서 사용자의 기본 속성을 읽습니다. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory에서 users.directReports 속성을 읽습니다. |
| microsoft.aad.directory/users/manager/read | Azure Active Directory에서 users.manager 속성을 읽습니다. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory에서 users.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory에서 users.oAuth2PermissionGrants 속성을 읽습니다. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory에서 users.ownedDevices 속성을 읽습니다. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory에서 users.ownedObjects 속성을 읽습니다. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory에서 users.registeredDevices 속성을 읽습니다. |

### <a name="directory-synchronization-accounts"></a>디렉터리 동기화 계정
Azure AD Connect에서만 사용됩니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | Azure Active Directory에서 organization.dirSync 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/create | Azure Active Directory에서 정책을 만듭니다. |
| microsoft.aad.directory/policies/delete | Azure Active Directory에서 정책을 삭제합니다. |
| microsoft.aad.directory/policies/basic/read | Azure Active Directory에서 정책의 기본 속성을 읽습니다. |
| microsoft.aad.directory/policies/basic/update | Azure Active Directory에서 정책의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/owners/read | Azure Active Directory에서 policies.owners 속성을 읽습니다. |
| microsoft.aad.directory/policies/owners/update | Azure Active Directory에서 policies.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/policiesAppliedTo/read | Azure Active Directory에서 policies.policiesAppliedTo 속성을 읽습니다. |
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory에서 policies.tenantDefault 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | Azure Active Directory에서 servicePrincipals.appRoleAssignedTo 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | Azure Active Directory에서 servicePrincipals.appRoleAssignedTo 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | Azure Active Directory에서 servicePrincipals.appRoleAssignments 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | Azure Active Directory에서 servicePrincipals.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/audience/update | Azure Active Directory에서 servicePrincipals.audience 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/authentication/update | Azure Active Directory에서 servicePrincipals.authentication 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/basic/read | Azure Active Directory에서 servicePrincipals의 기본 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/basic/update | Azure Active Directory에서 servicePrincipals의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/create | Azure Active Directory에서 servicePrincipals를 만듭니다. |
| microsoft.aad.directory/servicePrincipals/credentials/update | Azure Active Directory에서 servicePrincipals.credentials 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/memberOf/read | Azure Active Directory에서 servicePrincipals.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | Azure Active Directory에서 servicePrincipals.oAuth2PermissionGrants 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/owners/read | Azure Active Directory에서 servicePrincipals.owners 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/owners/update | Azure Active Directory에서 servicePrincipals.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | Azure Active Directory에서 servicePrincipals.ownedObjects 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/permissions/update | Azure Active Directory에서 servicePrincipals.permissions 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/policies/read | Azure Active Directory에서 servicePrincipals.policies 속성을 읽습니다. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory에서 servicePrincipals.policies 속성을 업데이트합니다. |
| microsoft.aad.directorySync/allEntities/allTasks | Azure AD Connect에서 모든 작업을 수행합니다. |

### <a name="directory-writers"></a>디렉터리 작성기
기본 디렉터리 정보를 읽고 쓸 수 있습니다. 애플리케이션에 대한 액세스 권한은 사용자를 위한 것이 아닙니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/groups/create | Azure Active Directory에서 그룹을 만듭니다. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory에서 그룹을 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory에서 groups.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/basic/update | Azure Active Directory에서 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/members/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/owners/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/settings/update | Azure Active Directory에서 groups.settings 속성을 업데이트합니다. |
| microsoft.aad.directory/groupSettings/basic/update | Azure Active Directory에서 groupSettings의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groupSettings/create | Azure Active Directory에서 groupSettings를 만듭니다. |
| microsoft.aad.directory/groupSettings/delete | Azure Active Directory에서 groupSettings를 삭제합니다. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory에서 users.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory에서 사용자의 라이선스를 관리합니다. |
| microsoft.aad.directory/users/basic/update | Azure Active Directory에서 사용자의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/manager/update | Azure Active Directory에서 users.manager 속성을 업데이트합니다. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory에서 users.userPrincipalName 속성을 업데이트합니다. |

### <a name="exchange-service-administrator"></a>Exchange 서비스 관리자
Exchange 제품의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory에서 groups.unified 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/basic/update | Office 365 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/create | Office 365 그룹을 만듭니다. |
| microsoft.aad.directory/groups/unified/delete | Office 365 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/unified/members/update | Office 365 그룹의 멤버 자격을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/owners/update | Office 365 그룹의 소유권을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.exchange/allEntities/allTasks | Exchange Online의 모든 측면을 관리합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="external-identity-provider-administrator"></a>외부 Id 공급자 관리자
직접 페더레이션에서 사용할 id 공급자를 구성 합니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.b2c/identityProviders/allTasks | Azure Active Directory B2C에서 id 공급자를 읽고 구성 합니다. |

### <a name="guest-inviter"></a>게스트 초대자
'멤버가 게스트를 초대할 수 있음' 설정에 관계없이 게스트 사용자를 초대할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | Azure Active Directory에서 users.appRoleAssignments 속성을 읽습니다. |
| microsoft.aad.directory/users/basic/read | Azure Active Directory에서 사용자의 기본 속성을 읽습니다. |
| microsoft.aad.directory/users/directReports/read | Azure Active Directory에서 users.directReports 속성을 읽습니다. |
| microsoft.aad.directory/users/inviteGuest | Azure Active Directory에서 게스트 사용자를 초대합니다. |
| microsoft.aad.directory/users/manager/read | Azure Active Directory에서 users.manager 속성을 읽습니다. |
| microsoft.aad.directory/users/memberOf/read | Azure Active Directory에서 users.memberOf 속성을 읽습니다. |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | Azure Active Directory에서 users.oAuth2PermissionGrants 속성을 읽습니다. |
| microsoft.aad.directory/users/ownedDevices/read | Azure Active Directory에서 users.ownedDevices 속성을 읽습니다. |
| microsoft.aad.directory/users/ownedObjects/read | Azure Active Directory에서 users.ownedObjects 속성을 읽습니다. |
| microsoft.aad.directory/users/registeredDevices/read | Azure Active Directory에서 users.registeredDevices 속성을 읽습니다. |

### <a name="helpdesk-administrator"></a>기술 지원팀 관리자
관리자가 아닌 사용자 및 기술 지원팀 관리자의 암호를 재설정할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory에서 devices.bitLockerRecoveryKeys 속성을 읽습니다. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/password/update | Azure Active Directory의 모든 사용자에 대한 암호를 업데이트합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="intune-service-administrator"></a>Intune 서비스 관리자
Intune 제품의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Azure Active Directory에서 연락처의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/contacts/create | Azure Active Directory에서 연락처를 만듭니다. |
| microsoft.aad.directory/contacts/delete | Azure Active Directory에서 연락처를 삭제합니다. |
| microsoft.aad.directory/devices/basic/update | Azure Active Directory에서 디바이스의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory에서 devices.bitLockerRecoveryKeys 속성을 읽습니다. |
| microsoft.aad.directory/devices/create | Azure Active Directory에서 디바이스를 만듭니다. |
| microsoft.aad.directory/devices/delete | Azure Active Directory에서 디바이스를 삭제합니다. |
| microsoft.aad.directory/devices/registeredOwners/update | Azure Active Directory에서 devices.registeredOwners 속성을 업데이트합니다. |
| microsoft.aad.directory/devices/registeredUsers/update | Azure Active Directory에서 devices.registeredUsers 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory에서 groups.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/basic/update | Azure Active Directory에서 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/create | Azure Active Directory에서 그룹을 만듭니다. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory에서 그룹을 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/groups/delete | Azure Active Directory에서 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory에서 groups.hiddenMembers 속성을 읽습니다. |
| microsoft.aad.directory/groups/members/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/owners/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/restore | Azure Active Directory에서 그룹을 복원합니다. |
| microsoft.aad.directory/groups/settings/update | Azure Active Directory에서 groups.settings 속성을 업데이트합니다. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory에서 users.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/users/basic/update | Azure Active Directory에서 사용자의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/manager/update | Azure Active Directory에서 users.manager 속성을 업데이트합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.intune/allEntities/allTasks | Intune의 모든 측면을 관리합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스의 기본 속성을 읽습니다. |

### <a name="kaizala-administrator"></a>Kaizala 관리자
Microsoft Kaizala에 대 한 설정을 관리할 수 있습니다.  

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >  
  
| **actions** | **설명** |
| --- | --- |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | Office 365 관리 센터를 참조 하세요. |

### <a name="license-administrator"></a>라이선스 관리자
사용자 및 그룹의 제품 라이선스를 관리할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory에서 사용자의 라이선스를 관리합니다. |
| microsoft.aad.directory/users/usageLocation/update | Azure Active Directory에서 users.usageLocation 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="lync-service-administrator"></a>Lync Service 관리자
비즈니스용 Skype 제품의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 비즈니스용 Skype Online의 모든 측면을 관리합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="message-center-privacy-reader"></a>메시지 센터 개인 정보 읽기 권한자
메시지 센터 게시물, 데이터 개인 정보 보호 메시지, 그룹, 도메인 및 구독을 읽을 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter에서 메시지를 읽습니다. |
| microsoft.office365.messageCenter/securityMessages/read | microsoft.office365.messageCenter의 securityMessages를 읽습니다. |

### <a name="message-center-reader"></a>메시지 센터 읽기 권한자
Office 365 메시지 센터에서만 조직의 메시지 및 업데이트를 읽을 수 있습니다. 

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter의 메시지를 읽습니다. |

### <a name="partner-tier1-support"></a>파트너 계층1 지원
사용하지 마세요. 일반적인 용도로는 적합하지 않습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Azure Active Directory에서 연락처의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/contacts/create | Azure Active Directory에서 연락처를 만듭니다. |
| microsoft.aad.directory/contacts/delete | Azure Active Directory에서 연락처를 삭제합니다. |
| microsoft.aad.directory/groups/create | Azure Active Directory에서 그룹을 만듭니다. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory에서 그룹을 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/groups/members/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/owners/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory에서 users.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory에서 사용자의 라이선스를 관리합니다. |
| microsoft.aad.directory/users/basic/update | Azure Active Directory에서 사용자의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/delete | Azure Active Directory에서 사용자를 삭제합니다. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/manager/update | Azure Active Directory에서 users.manager 속성을 업데이트합니다. |
| microsoft.aad.directory/users/password/update | Azure Active Directory의 모든 사용자에 대한 암호를 업데이트합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
| microsoft.aad.directory/users/restore | Azure Active Directory에서 삭제된 사용자를 복원합니다. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory에서 users.userPrincipalName 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="partner-tier2-support"></a>파트너 계층2 지원
사용하지 마세요. 일반적인 용도로는 적합하지 않습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | Azure Active Directory에서 연락처의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/contacts/create | Azure Active Directory에서 연락처를 만듭니다. |
| microsoft.aad.directory/contacts/delete | Azure Active Directory에서 연락처를 삭제합니다. |
| microsoft.aad.directory/domains/allTasks | Azure Active Directory에서 도메인을 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/groups/create | Azure Active Directory에서 그룹을 만듭니다. |
| microsoft.aad.directory/groups/delete | Azure Active Directory에서 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/members/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/restore | Azure Active Directory에서 그룹을 복원합니다. |
| microsoft.aad.directory/organization/basic/update | Azure Active Directory에서 조직의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory에서 users.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory에서 사용자의 라이선스를 관리합니다. |
| microsoft.aad.directory/users/basic/update | Azure Active Directory에서 사용자의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/delete | Azure Active Directory에서 사용자를 삭제합니다. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/manager/update | Azure Active Directory에서 users.manager 속성을 업데이트합니다. |
| microsoft.aad.directory/users/password/update | Azure Active Directory의 모든 사용자에 대한 암호를 업데이트합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
| microsoft.aad.directory/users/restore | Azure Active Directory에서 삭제된 사용자를 복원합니다. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory에서 users.userPrincipalName 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="password-administrator"></a>암호 관리자
관리자가 아닌 관리자 및 암호 관리자에 대 한 암호를 다시 설정할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/users/password/update | Azure Active Directory의 모든 사용자에 대한 암호를 업데이트합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스의 기본 속성을 읽습니다. |

### <a name="power-bi-service-administrator"></a>Power BI 서비스 관리자
Power BI 제품의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.powerApps.powerBI/allEntities/allTasks | Power BI의 모든 측면을 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="privileged-authentication-administrator"></a>권한 있는 인증 관리자
사용자(관리자 또는 비관리자)의 인증 메서드 정보를 보고, 설정하고, 다시 설정할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/strongAuthentication/update | MFA 자격 증명 정보와 같은 강력한 인증 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.aad.directory/users/password/update | Office 365 조직의 모든 사용자에 대 한 암호를 업데이트 합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
### <a name="privileged-role-administrator"></a>권한있는 역할 관리자
Azure AD의 역할 할당 및 Privileged Identity Management의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | 모든 리소스를 만들고 삭제한 다음 microsoft.aad.privilegedIdentityManagement에서 표준 속성을 읽고 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/allTasks | Azure Active Directory에서 servicePrincipals. appRoleAssignedTo 속성을 읽고 구성 합니다. |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/allTasks | Azure Active Directory에서 oAuth2PermissionGrants 속성을 읽고 구성 합니다. |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | 관리 단위 만들기 및 관리 (구성원 포함) |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | 역할 할당을 만들고 관리 합니다. |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | 역할 정의를 만들고 관리 합니다. |

### <a name="reports-reader"></a>보고서 읽기 권한자
로그인 및 감사 보고서를 읽을 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |

### <a name="search-administrator"></a>관리자 검색
Microsoft 검색 설정의 모든 측면을 만들고 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter의 메시지를 읽습니다. |
| microsoft.office365.search/allEntities/allProperties/allTasks | 모든 리소스를 만들고 삭제 하 고 office365에서 모든 속성을 읽고 업데이트 합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스의 기본 속성을 읽습니다. |

### <a name="search-editor"></a>검색 편집기
책갈피, Q, As, 위치, floorplan 등의 편집 콘텐츠를 만들고 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.office365.messageCenter/messages/read | microsoft.office365.messageCenter의 메시지를 읽습니다. |
| microsoft.office365.search/content/allProperties/allTasks | Office365에서 콘텐츠를 만들고 삭제 하 고, 모든 속성을 읽고 업데이트 합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |

### <a name="security-administrator"></a>보안 관리자
Azure AD 및 Office 365에서 보안 정보 및 보고서를 읽고 구성을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | Azure Active Directory에서 applications.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory에서 devices.bitLockerRecoveryKeys 속성을 읽습니다. |
| microsoft.aad.directory/policies/basic/update | Azure Active Directory에서 정책의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/create | Azure Active Directory에서 정책을 만듭니다. |
| microsoft.aad.directory/policies/delete | Azure Active Directory에서 정책을 삭제합니다. |
| microsoft.aad.directory/policies/owners/update | Azure Active Directory에서 policies.owners 속성을 업데이트합니다. |
| microsoft.aad.directory/policies/tenantDefault/update | Azure Active Directory에서 policies.tenantDefault 속성을 업데이트합니다. |
| microsoft.aad.directory/servicePrincipals/policies/update | Azure Active Directory에서 servicePrincipals.policies 속성을 업데이트합니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection에서 모든 리소스를 읽습니다. |
| microsoft.aad.identityProtection/allEntities/update | microsoft.aad.identityProtection에서 모든 리소스를 업데이트합니다. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement의 모든 리소스를 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 보호 센터의 모든 측면을 읽습니다. |
| microsoft.office365.protectionCenter/allEntities/update | microsoft.office365.protectionCenter의 모든 리소스를 업데이트합니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="security-operator"></a>보안 운영자
보안 이벤트를 만들고 관리 합니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.cloudAppSecurity/allEntities/allTasks | Microsoft Cloud App Security를 읽고 구성 합니다. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection에서 모든 리소스를 읽습니다. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement의 모든 리소스를 읽습니다. |
| microsoft.azure.advancedThreatProtection/allEntities/read | Azure AD Advanced Threat Protection을 읽고 구성 합니다. |
| microsoft.intune/allEntities/allTasks | Intune의 모든 측면을 관리합니다. |
| microsoft.office365.securityComplianceCenter/allEntities/allTasks | 보안 및 준수 센터를 읽고 구성 합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |
| microsoft.windows.defenderAdvancedThreatProtection/allEntities/read | Windows Defender Advanced Threat Protection을 읽고 구성 합니다. |

### <a name="security-reader"></a>보안 읽기 권한자
Azure AD 및 Office 365의 보안 정보 및 보고서를 읽을 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/auditLogs/allProperties/read | Azure Active Directory에서 auditLogs에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.directory/devices/bitLockerRecoveryKeys/read | Azure Active Directory에서 devices.bitLockerRecoveryKeys 속성을 읽습니다. |
| microsoft.aad.directory/signInReports/allProperties/read | Azure Active Directory에서 signInReports에 대한 모든 속성(권한 있는 속성 포함)을 읽습니다. |
| microsoft.aad.identityProtection/allEntities/read | microsoft.aad.identityProtection에서 모든 리소스를 읽습니다. |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | microsoft.aad.privilegedIdentityManagement의 모든 리소스를 읽습니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.protectionCenter/allEntities/read | Office 365 보호 센터의 모든 측면을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="service-support-administrator"></a>서비스 지원 관리자
서비스 상태 정보를 읽고 지원 티켓을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="sharepoint-service-administrator"></a>SharePoint Service 관리자
SharePoint 서비스의 모든 측면을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory에서 groups.unified 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/basic/update | Office 365 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/create | Office 365 그룹을 만듭니다. |
| microsoft.aad.directory/groups/unified/delete | Office 365 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/unified/members/update | Office 365 그룹의 멤버 자격을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/owners/update | Office 365 그룹의 소유권을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.sharepoint/allEntities/allTasks | microsoft.office365.sharepoint에서 모든 리소스를 만들고 삭제하고, 표준 속성을 읽고 업데이트합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

### <a name="teams-communications-administrator"></a>Teams 통신 관리자
Microsoft Teams 서비스 내에서 통화 및 모임 기능을 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |

### <a name="teams-communications-support-engineer"></a>Teams 통신 지원 엔지니어
고급 도구를 사용하여 Teams 내의 통신 문제를 해결할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="teams-communications-support-specialist"></a>Teams 통신 지원 전문가
기본 도구를 사용하여 Teams 내의 통신 문제를 해결할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |

### <a name="teams-service-administrator"></a>Teams 서비스 관리자
Microsoft Teams 서비스를 관리할 수 있습니다.

  > [!NOTE]
  > 이 역할에는 Azure Active Directory 외부의 추가 권한이 있습니다. 자세한 내용은 위에 나온 역할 설명을 참조하세요.
  >
  >

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory에서 groups.hiddenMembers 속성을 읽습니다. |
| microsoft.aad.directory/groups/unified/appRoleAssignments/update | Azure Active Directory에서 groups.unified 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/basic/update | Office 365 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/create | Office 365 그룹을 만듭니다. |
| microsoft.aad.directory/groups/unified/delete | Office 365 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/unified/members/update | Office 365 그룹의 멤버 자격을 업데이트합니다. |
| microsoft.aad.directory/groups/unified/owners/update | Office 365 그룹의 소유권을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.usageReports/allEntities/read | Office 365 사용 현황 보고서를 읽습니다. |

### <a name="user-administrator"></a>사용자 관리자
제한된 관리자의 암호 재설정을 비롯하여 사용자 및 그룹의 모든 측면을 관리할 수 있습니다.

| **actions** | **설명** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | Azure Active Directory에서 appRoleAssignments를 만듭니다. |
| microsoft.aad.directory/appRoleAssignments/delete | Azure Active Directory에서 appRoleAssignments를 삭제합니다. |
| microsoft.aad.directory/appRoleAssignments/update | Azure Active Directory에서 appRoleAssignments를 업데이트합니다. |
| microsoft.aad.directory/contacts/basic/update | Azure Active Directory에서 연락처의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/contacts/create | Azure Active Directory에서 연락처를 만듭니다. |
| microsoft.aad.directory/contacts/delete | Azure Active Directory에서 연락처를 삭제합니다. |
| microsoft.aad.directory/groups/appRoleAssignments/update | Azure Active Directory에서 groups.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/basic/update | Azure Active Directory에서 그룹의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/create | Azure Active Directory에서 그룹을 만듭니다. |
| microsoft.aad.directory/groups/createAsOwner | Azure Active Directory에서 그룹을 만듭니다. 작성자는 첫 번째 소유자로 추가되고, 만들어진 개체는 작성자의 250개 개체 만들기 할당량과 대조하여 계산됩니다. |
| microsoft.aad.directory/groups/delete | Azure Active Directory에서 그룹을 삭제합니다. |
| microsoft.aad.directory/groups/hiddenMembers/read | Azure Active Directory에서 groups.hiddenMembers 속성을 읽습니다. |
| microsoft.aad.directory/groups/members/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/owners/update | Azure Active Directory에서 groups.members 속성을 업데이트합니다. |
| microsoft.aad.directory/groups/restore | Azure Active Directory에서 그룹을 복원합니다. |
| microsoft.aad.directory/groups/settings/update | Azure Active Directory에서 groups.settings 속성을 업데이트합니다. |
| microsoft.aad.directory/users/appRoleAssignments/update | Azure Active Directory에서 users.appRoleAssignments 속성을 업데이트합니다. |
| microsoft.aad.directory/users/assignLicense | Azure Active Directory에서 사용자의 라이선스를 관리합니다. |
| microsoft.aad.directory/users/basic/update | Azure Active Directory에서 사용자의 기본 속성을 업데이트합니다. |
| microsoft.aad.directory/users/create | Azure Active Directory에서 사용자를 만듭니다. |
| microsoft.aad.directory/users/delete | Azure Active Directory에서 사용자를 삭제합니다. |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | Azure Active Directory에서 모든 사용자 새로 고침 토큰을 무효화합니다. |
| microsoft.aad.directory/users/manager/update | Azure Active Directory에서 users.manager 속성을 업데이트합니다. |
| microsoft.aad.directory/users/password/update | Azure Active Directory의 모든 사용자에 대한 암호를 업데이트합니다. 자세한 내용은 온라인 설명서를 참조하세요. |
| microsoft.aad.directory/users/restore | Azure Active Directory에서 삭제된 사용자를 복원합니다. |
| microsoft.aad.directory/users/userPrincipalName/update | Azure Active Directory에서 users.userPrincipalName 속성을 업데이트합니다. |
| microsoft.azure.serviceHealth/allEntities/allTasks | Azure Service Health를 읽고 구성합니다. |
| microsoft.azure.supportTickets/allEntities/allTasks | Azure 지원 티켓을 만들고 관리합니다. |
| microsoft.office365.webPortal/allEntities/basic/read | microsoft.office365.webPortal에서 모든 리소스에 대한 기본 속성을 읽습니다. |
| microsoft.office365.serviceHealth/allEntities/allTasks | Office 365 Service Health를 읽고 구성합니다. |
| microsoft.office365.supportTickets/allEntities/allTasks | Office 365 지원 티켓을 만들고 관리합니다. |

## <a name="role-template-ids"></a>역할 템플릿 Id

역할 템플릿 Id는 주로 Graph API 또는 PowerShell 사용자가 사용 합니다.

그래프 displayName | Azure Portal 표시 이름 | directoryRoleTemplateId
----------------- | ------------------------- | -------------------------
애플리케이션 관리자 | 애플리케이션 관리자 | 9B895D92-2CD3-44C7-9D02-A6AC2D5EA5C3
애플리케이션 개발자 | 응용 프로그램 개발자 | CF1C38E5-3621-4004-A7CB-879624DCED7C
인증 관리자 | 인증 관리자 | c4e39bd9-1100-46d3-8c65-fb160da0071f
Azure Information Protection 관리자 | Azure Information Protection 관리자 | 7495fdc4-34c4-4d15-a289-98788ce399fd
B2C 사용자 흐름 관리자 | B2C 사용자 흐름 관리자 | 6e591065-9bad-43ed-90f3-e9424366d2f0
B2C 사용자 흐름 특성 관리자 | B2C 사용자 흐름 특성 관리자 | 0f971eea-41eb-4569-a71e-57bb8a3eff1e
B2C IEF 키 집합 관리자 | B2C IEF 키 집합 관리자 | aaf43236-0c0d-4d5f-883a-6955382ac081
B2C IEF 정책 관리자 | B2C IEF 정책 관리자 | 3edaf663-341e-4475-9f94-5c398ef6c070
대금 청구 관리자 | 대금 청구 관리자 | b0f54661-2d74-4c50-afa3-1ec803f12efe
클라우드 애플리케이션 관리자 | 클라우드 애플리케이션 관리자 | 158c047a-c907-4556-b7ef-446551a6b5f7
클라우드 디바이스 관리자 | 클라우드 디바이스 관리자 | 7698a772-787b-4ac8-901f-60d6b08affd2
회사 관리자 | 전역 관리자 | 62e90394-69f5-4237-9190-012177145e10
준수 관리자 | 준수 관리자 | 17315797-102d-40b4-93e0-432062caca18
준수 데이터 관리자 | 준수 데이터 관리자 | e6d1a23a-da11-4be4-9570-befc86d067a7
조건부 액세스 관리자 | 조건부 액세스 관리자 | b1be1c3e-b65d-4f19-8427-f6fa0d97feb9
CRM 서비스 관리자 | Dynamics 365 관리자 | 44367163-eba1-44c3-98af-f5787879f96a
고객 LockBox 액세스 승인자 | 고객 Lockbox 액세스 승인자 | 5c4f9dcd-47dc-4cf7-8c9a-9e4207cbfc91
데스크톱 분석 관리자 | 데스크톱 분석 관리자 | 38a96431-2bdf-4b4c-8b6e-5d3d8abac1a4
디바이스 관리자 | 장치 관리자 | 9f06204d-73c1-4d4c-880a-6edb90606fd8
디바이스 연결 | 장치 조인 | 9c094953-4995-41c8-84c8-3ebb9b32c93f
디바이스 관리자 | 장치 관리자 | 2b499bcd-da44-4968-8aec-78e1674fa64d
디바이스 사용자 | 장치 사용자 | d405c6df-0af8-4e3b-95e4-4d06e542189e
디렉터리 읽기 권한자 | 디렉터리 읽기 권한자 | 88d8e3e3-8f55-4a1e-953a-9b9898b8876b
디렉터리 동기화 계정 | 디렉터리 동기화 계정 | d29b2b05-8046-44ba-8758-1e26182fcf32
디렉터리 작성기 | 디렉터리 쓰기 권한자 | 9360feb5-f418-4baa-8175-e2a00bac4301
Exchange 서비스 관리자 | Exchange 관리자 | 29232cdf-9323-42fd-ade2-1d097af3e4de
외부 Id 공급자 관리자 | 외부 Id 공급자 관리자 | be2f45a1-457d-42af-a067-6ec1fa63bc45
게스트 초대자 | 게스트 초대자 | 95e79109-95c0-4d8e-aee3-d01accf2d47b
기술 지원팀 관리자 | 암호 관리자 | 729827e3-9c14-49f7-bb1b-9608f156bbb8
Intune 서비스 관리자 | Intune 관리자 | 3a2c62db-5318-420d-8d74-23affee5d9d5
Kaizala 관리자 | Kaizala 관리자 | 74ef975b-6605-40af-a5d2-b9539d836353
라이선스 관리자 | 라이선스 관리자 | 4d6ac14f-3453-41d0-bef9-a3e0c569773a
Lync Service 관리자 | 비즈니스용 Skype 관리자 | 75941009-915a-4869-abe7-691bff18279e
메시지 센터 개인 정보 읽기 권한자 | 메시지 센터 개인 정보 읽기 권한자 | ac16e43d-7b2d-40e0-ac05-243ff356ab5b
메시지 센터 읽기 권한자 | 메시지 센터 읽기 권한자 | 790c1fb9-7f7d-4f88-86a1-ef1f95c05c1b
파트너 계층1 지원 | 파트너 계층1 지원 | 4ba39ca4-527c-499a-b93d-d9b492c50246
파트너 계층2 지원 | 파트너 계층2 지원 | e00e864a-17c5-4a4b-9c06-f5b95a8d5bd8
암호 관리자 | 암호 관리자 | 966707d0-3269-4727-9be2-8c3a10f19b9d
Power BI 서비스 관리자 | Power BI 관리자 | a9ea8996-122f-4c74-9520-8edcd192826c
권한 있는 인증 관리자 | 권한 있는 인증 관리자 | 7be44c8a-adaf-4e2a-84d6-ab2649e08a13
권한있는 역할 관리자 | 권한 있는 역할 관리자 | e8611ab8-c189-46e8-94e1-60213ab1f814
보고서 읽기 권한자 | 보고서 읽기 권한자 | 4a5d8f65-41da-4de4-8968-e035b65339cf
관리자 검색 | 관리자 검색 | 0964bb5e-9bdb-4d7b-ac29-58e794862a40
검색 편집기 | 검색 편집기 | 8835291a-918c-4fd7-a9ce-faa49f0cf7d9
보안 관리자 | 보안 관리자 | 194ae4cb-b126-40b2-bd5b-6091b380977d
보안 운영자 | 보안 운영자 | 5f2222b1-57c3-48ba-8ad5-d4759f1fde6f
보안 읽기 권한자 | 보안 독자 | 5d6b6bb7-de71-4623-b4af-96380a352509
서비스 지원 관리자 | 서비스 관리자 | f023fd81-a637-4b56-95fd-791ac0226033
SharePoint Service 관리자 | Sharepoint 관리자 | f28a1f50-f6e7-4571-818b-6a12f2af6b6c
Teams 통신 관리자 | Teams 통신 관리자 | baf37b3a-610e-45da-9e62-d9d1e5e8914b
Teams 통신 지원 엔지니어 | Teams 통신 지원 엔지니어 | f70938a0-fc10-4177-9e90-2178f8765737
Teams 통신 지원 전문가 | Teams 통신 지원 전문가 | fcf91098-03e3-41a9-b5ba-6f0ec8188a12
Teams 서비스 관리자 | Teams 서비스 관리자 | 69091246-20e8-4a56-aa4d-066075b2a7a8
사용자 | 사용자 | a0b1b346-4d3e-4e8b-98f8-753987be4970
사용자 계정 관리자 | 사용자 관리자 | fe930be7-5e62-47db-91af-98c3a49a38b1
작업 공간 디바이스 연결 | 작업 공간 장치 연결 | c34f683f-4d5a-4403-affd-6615e00e3a7f


## <a name="deprecated-roles"></a>사용되지 않는 역할

다음 역할은 사용할 수 없습니다. 해당 역할은 사용되지 않으며 향후 Azure AD에서 제거됩니다.

* 애드혹 라이선스 관리자
* 디바이스 연결
* 디바이스 관리
* 디바이스 사용자
* 전자 메일 확인 사용자 생성자
* 사서함 관리자
* 작업 공간 디바이스 연결

## <a name="next-steps"></a>다음 단계

* Azure 구독의 관리자로서 사용자를 할당하는 방법에 대해 자세히 알아보려면 [RBAC 및 Azure Portal을 사용하여 액세스 관리](../../role-based-access-control/role-assignments-portal.md)를 참조하세요.
* Microsoft Azure에서 리소스 액세스를 제어하는 방법을 자세히 알아보려면 [Azure에서 리소스 액세스 이해](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* Azure 구독에 Azure Active Directory가 연결되는 방법에 대한 자세한 내용은 [Azure 구독을 Azure Active Directory에 연결하는 방법](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
