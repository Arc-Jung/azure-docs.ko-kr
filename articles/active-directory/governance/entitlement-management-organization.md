---
title: Azure AD 자격 관리에서 연결 된 조직 추가-Azure Active Directory
description: 조직 외부 사용자가 프로젝트에 대 한 공동 작업을 수행할 수 있도록 액세스 패키지를 요청 하는 방법을 알아봅니다.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 12/11/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f08c25749bbd21e3624dee898d9a8c97fd74164
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2021
ms.locfileid: "98059384"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Azure AD 자격 관리에서 연결 된 조직 추가

Azure Active Directory (Azure AD) 자격 관리를 통해 조직 외부 사용자와 공동 작업을 수행할 수 있습니다. 외부 Azure AD 디렉터리 또는 도메인의 사용자와 자주 공동 작업 하는 경우 연결 된 조직으로 추가할 수 있습니다. 이 문서에서는 조직 외부 사용자가 디렉터리에서 리소스를 요청할 수 있도록 연결 된 조직을 추가 하는 방법을 설명 합니다.

## <a name="what-is-a-connected-organization"></a>연결 된 조직 이란?

연결 된 조직은와 관계가 있는 다른 조직입니다.  해당 조직의 사용자가 SharePoint Online 사이트 또는 앱과 같은 리소스에 액세스할 수 있도록 하려면 해당 조직의 사용자에 게 해당 디렉터리에 대 한 표현이 필요 합니다.  대부분의 경우 해당 조직의 사용자가 이미 Azure AD 디렉터리에 있지 않기 때문에 필요에 따라 자격 관리를 사용 하 여 Azure AD 디렉터리에 가져올 수 있습니다.  

세 가지 방법으로 자격 관리를 통해 연결 된 조직을 형성 하는 사용자를 지정할 수 있습니다.  다음이 될 수 있습니다.

* 다른 Azure AD 디렉터리의 사용자
* 직접 페더레이션을 위해 구성 된 다른 비 Azure AD 디렉터리의 사용자 또는
* 다른 비 Azure AD 디렉터리의 사용자는 모두 공통 된 동일한 도메인 이름을 포함 하는 전자 메일 주소입니다.

예를 들어, Woodgrove Bank에서 작업 하 고 두 개의 외부 조직과 공동 작업 하려는 경우를 가정해 보겠습니다. 이러한 두 조직의 구성은 다음과 같이 다릅니다.

- 그래픽 디자인 협회는 Azure AD를 사용 하며, 사용자는 *graphicdesigninstitute.com* 로 끝나는 사용자 계정 이름을 갖습니다.
- Contoso는 아직 Azure AD를 사용 하지 않습니다. Contoso 사용자에 게는 *contoso.com* 로 끝나는 사용자 계정 이름이 있습니다.

이 경우 연결 된 두 조직을 구성할 수 있습니다. 그래픽 디자인 협회와 Contoso 용으로 하나의 연결 된 조직을 만듭니다. 그런 다음 두 개의 연결 된 조직을 정책에 추가 하면 정책과 일치 하는 사용자 보안 주체 이름을 가진 각 조직의 사용자가 액세스 패키지를 요청할 수 있습니다. *Graphicdesigninstitute.com* 도메인을 포함 하는 사용자 계정 이름을 가진 사용자는 그래픽 디자인 협회 연결 조직과 일치 하 고 요청을 제출할 수 있습니다. *Contoso.com* 도메인을 포함 하는 사용자 계정 이름을 가진 사용자는 contoso 연결 조직과 일치 하 고 패키지를 요청할 수 있습니다. 또한 그래픽 디자인 기관에서는 Azure AD를 사용 하기 때문에 graphicdesigninstitute와 같이 테 넌 트에 추가 된 [확인 된 도메인과](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) 일치 하는 보안 주체 이름을 가진 사용자도 동일한 정책을 사용 하 여 액세스 패키지를 요청할 수 있습니다 *.*

![연결 된 조직 예](./media/entitlement-management-organization/connected-organization-example.png)

Azure AD 디렉터리 또는 도메인의 사용자가 인증 하는 방법은 인증 유형에 따라 달라 집니다. 연결 된 조직의 인증 유형은 다음과 같습니다.

- Azure AD
- [직접 페더레이션](../external-identities/direct-federation.md)
- [일회용 암호](../external-identities/one-time-passcode.md) (도메인)

연결 된 조직을 추가 하는 방법에 대 한 데모를 보려면 다음 비디오를 시청 하세요.

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>연결된 조직 추가

외부 Azure AD 디렉터리 또는 도메인을 연결 된 조직으로 추가 하려면이 섹션의 지침을 따르세요.

**필수 구성 요소 역할**: *전역 관리자* 또는 *사용자 관리자*

1. Azure Portal에서 **Azure Active Directory** 를 선택 하 고 **id 관리** 를 선택 합니다.

1. 왼쪽 창에서 **연결 된 조직** 을 선택 하 고 **연결 된 조직 추가** 를 선택 합니다.

    !["연결 된 조직 추가" 단추](./media/entitlement-management-organization/connected-organization.png)

1. **기본** 탭을 선택한 다음 조직의 표시 이름 및 설명을 입력 합니다.

    !["연결 된 조직 추가" 기본 사항 창](./media/entitlement-management-organization/organization-basics.png)

1. 새 연결 된 조직을 만들 때 상태가 자동으로 **구성** 됨으로 설정 됩니다. 상태 속성에 대 한 자세한 내용은 [연결 된 조직의 상태 속성](#state-properties-of-connected-organizations) 을 참조 하세요.

1. **디렉터리 + 도메인** 탭을 선택 하 고 **디렉터리 추가 + 도메인** 을 선택 합니다.

    **디렉터리 + 도메인 선택** 창이 열립니다.

1. 검색 상자에 도메인 이름을 입력 하 여 Azure AD 디렉터리 또는 도메인을 검색 합니다. 전체 도메인 이름을 입력 해야 합니다.

1. 조직 이름과 인증 유형이 올바른지 확인 합니다. 사용자가 로그인 하는 방법은 인증 유형에 따라 달라 집니다.

    !["디렉터리 + 도메인 선택" 창](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. **추가** 를 선택 하 여 Azure AD 디렉터리 또는 도메인을 추가 합니다. 현재는 연결 된 조직 마다 하나의 Azure AD 디렉터리 또는 도메인만 추가할 수 있습니다.

    > [!NOTE]
    > Azure AD 디렉터리 또는 도메인의 모든 사용자가이 액세스 패키지를 요청할 수 있습니다. 이러한 도메인은 Azure AD B2B (business to business) 허용 또는 거부 목록에 의해 차단 되지 않는 한, 디렉터리와 연결 된 모든 하위 도메인에서 Azure AD의 사용자를 포함 합니다. 자세한 내용은 [특정 조직의 B2B 사용자 초대 허용 또는 차단](../external-identities/allow-deny-list.md)을 참조하세요.

1. Azure AD 디렉터리 또는 도메인을 추가한 후 **선택** 을 선택 합니다.

    조직이 목록에 표시 됩니다.

    !["디렉터리 + 도메인" 창](./media/entitlement-management-organization/organization-directory-domain.png)

1. **스폰서** 탭을 선택 하 고이 연결 된 조직의 스폰서 (선택 사항)를 추가 합니다.

    스폰서는이 연결 된 조직과의 관계에 대 한 연락처 지점인 디렉터리에 이미 있는 내부 또는 외부 사용자입니다. 내부 스폰서는 디렉터리의 멤버 사용자입니다. 외부 스폰서는 이전에 초대 되었으며 이미 디렉터리에 있는 연결 된 조직의 게스트 사용자입니다. 이 연결 된 조직의 사용자가이 액세스 패키지에 대 한 액세스를 요청 하는 경우 스폰서를 승인자로 사용할 수 있습니다. 디렉터리에 게스트 사용자를 초대 하는 방법에 대 한 자세한 내용은 [Azure Portal에서 B2B 공동 작업 사용자 추가 Azure Active Directory](../external-identities/add-users-administrator.md)를 참조 하세요.

    **추가/제거** 를 선택 하면 내부 또는 외부 스폰서를 선택할 수 있는 창이 열립니다. 이 창에는 디렉터리의 필터링 되지 않은 사용자 및 그룹 목록이 표시 됩니다.

    ![스폰서 창](./media/entitlement-management-organization/organization-sponsors.png)

1. **검토 + 만들기** 탭을 선택 하 고 조직 설정을 검토 한 후 **만들기** 를 선택 합니다.

    !["검토 + 만들기" 창](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>연결 된 조직 업데이트 

연결 된 조직이 다른 도메인으로 변경 되거나 조직의 이름이 변경 되거나 스폰서를 변경 하려는 경우이 섹션의 지침에 따라 연결 된 조직을 업데이트할 수 있습니다.

**필수 구성 요소 역할**: *전역 관리자* 또는 *사용자 관리자*

1. Azure Portal에서 **Azure Active Directory** 를 선택 하 고 **id 관리** 를 선택 합니다.

1. 왼쪽 창에서 **연결 된 조직** 을 선택 하 고 연결 된 조직을 선택 하 여 엽니다.

1. 연결 된 조직의 개요 창에서 **편집** 을 선택 하 여 조직 이름, 설명 또는 상태를 변경 합니다.  

1. **디렉터리 + 도메인** 창에서 **디렉터리 업데이트 + 도메인** 을 선택 하 여 다른 디렉터리나 도메인으로 변경 합니다.

1. **스폰서** 창에서 **내부 스폰서 추가** 또는 **외부 스폰서 추가** 를 선택 하 여 사용자를 스폰서로 추가 합니다. 스폰서를 제거 하려면 스폰서를 선택 하 고 오른쪽 창에서 **삭제** 를 선택 합니다.


## <a name="delete-a-connected-organization"></a>연결 된 조직 삭제

외부 Azure AD 디렉터리 또는 도메인과 더 이상 관계가 없으면 연결 된 조직을 삭제할 수 있습니다.

**필수 구성 요소 역할**: *전역 관리자* 또는 *사용자 관리자*

1. Azure Portal에서 **Azure Active Directory** 를 선택 하 고 **id 관리** 를 선택 합니다.

1. 왼쪽 창에서 **연결 된 조직** 을 선택 하 고 연결 된 조직을 선택 하 여 엽니다.

1. 연결 된 조직의 개요 창에서 **삭제** 를 선택 하 여 삭제 합니다.

    ![연결 된 조직 삭제 단추](./media/entitlement-management-organization/organization-delete.png)

## <a name="managing-a-connected-organization-programmatically"></a>프로그래밍 방식으로 연결 된 조직 관리

또한 Microsoft Graph를 사용 하 여 연결 된 조직을 만들고, 나열 하 고, 업데이트 하 고, 삭제할 수 있습니다. 위임 된 권한이 있는 응용 프로그램을 사용 하는 적절 한 역할의 사용자는 `EntitlementManagement.ReadWrite.All` API를 호출 하 여 [connectedOrganization](/graph/api/resources/connectedorganization?view=graph-rest-beta) 개체를 관리 하 고 스폰서를 설정할 수 있습니다.

## <a name="state-properties-of-connected-organizations"></a>연결 된 조직의 상태 속성

Azure AD 자격 관리에서 연결 된 조직의 상태 속성에는 현재 구성 되어 있고 제안 된 두 가지 유형이 있습니다. 

- 구성 된 연결 된 구성은 해당 조직 내의 사용자가 패키지에 액세스할 수 있도록 하는 완전 한 기능을 갖춘 연결 된 조직입니다. 관리자가 Azure Portal에 연결 된 새 조직을 만들면 관리자가 만들고 연결 된 조직을 사용 하려고 하기 때문에 기본적으로 **구성** 된 상태가 됩니다. 또한 API를 통해 프로그래밍 방식으로 연결 된 조직을 만든 경우에는 명시적으로 다른 상태로 설정 하지 않는 한 기본 상태를 **구성** 해야 합니다. 

    구성 된 연결 된 조직은 연결 된 조직의 선택 영역에 표시 되 고 "모든" 연결 된 조직을 대상으로 하는 모든 정책에 대해 범위 내에 있습니다.

- 제안 된 연결 된 구성은 자동으로 만들어졌지만 관리자가 조직을 만들거나 승인 하지 않은 연결 된 조직입니다. 사용자가 구성 된 연결 된 조직 외부에서 액세스 패키지에 등록 하는 경우 테 넌 트의 관리자가 해당 파트너 관계를 설정 하지 않았기 때문에 자동으로 생성 된 모든 연결 된 조직이 **제안 됨** 상태가 됩니다. 
    
    제안 된 연결 된 조직은 모든 정책에 대 한 "구성 된 모든 연결 된 조직"에 대해 범위에 속하지 않지만 특정 조직을 대상으로 하는 정책에 대해서만 정책에 사용할 수 있습니다. 

구성 된 연결 된 조직의 사용자만 구성 된 모든 조직의 사용자가 사용할 수 있는 액세스 패키지를 요청할 수 있습니다. 제안 된 연결 된 조직의 사용자에 게는 해당 도메인에 대 한 연결 된 조직이 없는 것 처럼 환경이 있습니다. 는 특정 조직으로 범위가 지정 되거나 사용자로 범위가 지정 된 액세스 패키지만 보고 요청할 수 있습니다.

> [!NOTE]
> 이 새로운 기능을 배포 하는 과정의 일환으로 09/09/20 이전에 생성 된 모든 연결 된 조직은 **구성** 된 것으로 간주 됩니다. 모든 조직의 사용자가 등록할 수 있도록 허용 하는 액세스 패키지가 있는 경우 해당 날짜 이전에 생성 된 연결 된 조직의 목록을 검토 하 여 **구성** 된 것으로 miscategorized 되지 않도록 해야 합니다.  관리자는 **상태** 속성을 적절 하 게 업데이트할 수 있습니다. 지침은 [연결 된 조직 업데이트](#update-a-connected-organization)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [외부 사용자를 위한 액세스 제어](./entitlement-management-external-users.md)
- [디렉터리에 없는 사용자에 대 한 액세스 제어](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
