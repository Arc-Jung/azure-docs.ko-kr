---
title: PIM-Azure Active Directory |에서 Azure 리소스 역할에 대 한 액세스 검토 완료 Microsoft Docs
description: Azure AD Privileged Identity Management (PIM)에서 Azure 리소스 역할에 대 한 액세스 검토를 완료 하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b9563a4032011b999bf867fc782ba4cbb9c3fac
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804246"
---
# <a name="complete-an-access-review-of-azure-resource-roles-in-pim"></a>PIM의 Azure 리소스 역할에 대 한 액세스 검토 완료
[액세스 검토가 시작](pim-resource-roles-start-access-review.md)된 후에 권한 있는 역할 관리자가 권한이 있는 액세스를 검토할 수 있습니다. Azure Active Directory (Azure AD) Privileged Identity Management (PIM)는 사용자에 게 액세스를 검토 하도록 요청 하는 전자 메일을 자동으로 보냅니다. 사용자가 메일을 받지 못하는 경우 [액세스 검토를 수행하는 방법](pim-resource-roles-perform-access-review.md)에 대한 지침을 보낼 수 있습니다.

액세스 검토 기간이 끝나거나 모든 사용자가 자체 검토를 완료하면 이 문서의 단계에 따라 검토를 관리하고 결과를 표시합니다.

## <a name="manage-access-reviews"></a>액세스 검토 관리
1. [Azure 포털](https://portal.azure.com/)로 이동합니다. 그런 다음, 대시보드에서 **Azure 리소스** 애플리케이션을 선택합니다.

2. 리소스를 선택합니다.

3. 대시보드의 **액세스 검토** 섹션을 선택합니다.

    ![Azure 리소스-역할, 소유자, 시작 날짜, 종료 날짜 및 상태를 표시 하는 액세스 검토 목록](media/pim-resource-roles-complete-access-review/rbac-access-review-home-list.png)

4. 관리하려는 액세스 검토를 선택합니다.

액세스 검토의 세부 정보 블레이드에는 해당 검토를 관리하는 다양한 옵션이 있습니다. 옵션은 다음과 같습니다.

![검토 관리 옵션-중지, 다시 설정, 적용, 삭제](media/pim-resource-roles-complete-access-review/rbac-access-review-menu.png)

### <a name="stop"></a>Stop
모든 액세스 검토는 종료 날짜가 있지만, **중지** 단추를 사용하여 일찍 완료할 수 있습니다. 이 시간까지 자신의 검토를 완료하지 않은 모든 사용자는 검토를 중지한 후 완료할 수 없습니다. 검토를 중단한 후에는 다시 시작할 수 없습니다.

### <a name="reset"></a>재설정
액세스 검토를 다시 설정하여 모든 결정 사항을 제거할 수 있습니다. 액세스 검토를 다시 설정하면 모든 사용자가 다시 검토되지 않음으로 표시됩니다. 

### <a name="apply"></a>적용
액세스 검토가 완료된 후 **적용** 단추를 사용하여 검토 결과를 구현합니다. 사용자의 액세스가 검토 중에 거부되었다면, 이 단계는 해당 역할 할당을 제거합니다.  

### <a name="delete"></a>삭제
더 이상 검토가 필요 없는 경우 검토를 삭제합니다. **삭제** 단추는 PIM 애플리케이션에서 검토를 제거합니다.

## <a name="results"></a>결과
**결과** 페이지에서 검토 결과 목록을 확인 하 고 다운로드 합니다. 

![사용자, 결과, 이유, 검토 한 사람, 적용 결과, 적용 결과 페이지를 표시 합니다.](media/pim-resource-roles-complete-access-review/rbac-access-review-results.png)

## <a name="reviewers"></a>검토자
검토자를 보고 기존 액세스 검토에 추가합니다. 검토자에게 검토를 완료하라는 메시지를 표시합니다.

![검토자 페이지 목록 이름 및 사용자 계정 이름](media/pim-resource-roles-complete-access-review/rbac-access-review-reviewers.png)

## <a name="next-steps"></a>다음 단계

- [PIM에서 Azure 리소스 역할에 대한 액세스 검토 시작](pim-resource-roles-start-access-review.md)
- [PIM에서 Azure 리소스 역할에 대한 액세스 검토 수행](pim-resource-roles-perform-access-review.md)
