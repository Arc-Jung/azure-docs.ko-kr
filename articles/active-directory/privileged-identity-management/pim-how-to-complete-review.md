---
title: PIM에서 Azure AD 역할에 대 한 액세스 검토 완료-Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM)에서 Azure AD 역할에 대 한 액세스 검토를 완료 하 고 결과를 확인 하는 방법에 대해 알아봅니다.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/06/2017
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e50ccc208219896e89bcc80f40c846f69c759f9b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804409"
---
# <a name="complete-an-access-review-of-azure-ad-roles-in-pim"></a>PIM에서 Azure AD 역할에 대 한 액세스 검토 완료
[액세스 검토가 시작](pim-how-to-start-security-review.md)되면 권한 있는 역할 관리자가 권한이 있는 액세스를 검토할 수 있습니다. Azure Active Directory (Azure AD) Privileged Identity Management (PIM)는 사용자에 게 액세스를 검토 하도록 요청 하는 전자 메일을 자동으로 보냅니다. 사용자가 메일을 받지 못한 경우 [액세스 검토를 수행하는 방법](pim-how-to-perform-security-review.md)에 대한 지침을 보낼 수 있습니다.

액세스 검토 기간이 끝나거나 모든 사용자가 자체 검토를 완료하면 이 문서의 단계에 따라 검토를 관리하고 결과를 표시합니다.

## <a name="manage-access-reviews"></a>액세스 검토 관리
1. [Azure Portal](https://portal.azure.com/)로 이동하고 대시보드에서 **Azure AD Privileged Identity Management** 애플리케이션을 선택합니다.
2. 대시보드의 **액세스 검토** 섹션을 선택합니다.
3. 관리하려는 액세스 검토를 선택합니다.

액세스 검토의 세부 정보 블레이드에는 검토를 관리하는 몇 가지 옵션이 있습니다.

![PIM 액세스 검토 단추-스크린 샷](./media/pim-how-to-complete-review/review-buttons.png)

### <a name="remind"></a>알림
액세스 검토가 설정되어 사용자가 자체적으로 검토하는 경우 **알림** 단추가 알림을 보냅니다. 

### <a name="stop"></a>Stop
모든 액세스 검토는 종료 날짜가 있지만, **중지** 단추를 사용하여 일찍 완료할 수 있습니다. 만일 이 때까지 검토하지 않은 사용자가 있다면, 검토를 중지한 후에는 그들을 검토할 수 없습니다. 검토를 중단한 후에는 다시 시작할 수 없습니다.

### <a name="apply"></a>적용
종료 날짜가 되었거나 수동으로 중지하여 액세스 검토가 완료되면, **적용** 단추가 검토 결과를 구현합니다. 사용자의 액세스가 검토 중에 거부되었다면, 이는 그들의 역할 할당을 제거할 단계입니다.  

### <a name="export"></a>내보내기
액세스 검토 결과를 수동으로 적용하려는 경우 검토를 내보낼 수 있습니다. **내보내기** 단추는 CSV 파일 다운로드를 시작합니다. Excel 또는 CSV 파일을 여는 다른 프로그램에서 결과를 관리할 수 있습니다.

### <a name="delete"></a>삭제
더 이상 검토가 필요 없는 경우 검토를 삭제합니다. **삭제** 단추는 PIM 애플리케이션에서 검토를 제거합니다.

> [!IMPORTANT]
> 삭제가 일어나기 전에 경고가 표시되지 않으므로 삭제를 원하는 검토인지 확인해야 합니다. 

## <a name="next-steps"></a>다음 단계

- [PIM에서 Azure AD 역할에 대 한 액세스 검토 시작](pim-how-to-start-security-review.md)
- [PIM에서 내 Azure AD 역할에 대 한 액세스 검토 수행](pim-how-to-perform-security-review.md)
