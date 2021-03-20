---
title: Azure Analysis Services의 서버 관리자 관리 | Microsoft Docs
description: 이 문서에서는 Azure Portal, PowerShell 또는 REST Api를 사용 하 여 Azure Analysis Services 서버에 대 한 서버 관리자를 관리 하는 방법을 설명 합니다.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 2/4/2021
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 62acb526a247362b17c4dfd4e26c52760deecd71
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99573503"
---
# <a name="manage-server-administrators"></a>서버 관리자 관리

서버 관리자는 서버가 상주 하는 테 넌 트에 대 한 유효한 사용자, 서비스 주체 또는 보안 Azure Active Directory 그룹 (Azure AD) 이어야 합니다. Azure Portal 또는 SSMS의 [서버 속성], PowerShell 또는 REST API에서 서버에 대한 **Analysis Services 관리자** 를 사용하여 서버 관리자를 관리할 수 있습니다. 

**보안 그룹** 을 추가할 때를 사용 `obj:groupid@tenantid` 합니다. 서비스 사용자는 서버 관리자 역할에 추가 된 보안 그룹에서 지원 되지 않습니다.

서버 관리자 역할에 서비스 주체를 추가 하는 방법에 대 한 자세한 내용은 [서버 관리자 역할에 서비스 주체 추가](analysis-services-addservprinc-admins.md)를 참조 하세요.

서버 방화벽을 사용 하는 경우 서버 관리자 클라이언트 컴퓨터의 IP 주소가 방화벽 규칙에 포함 되어야 합니다. 자세히 알아보려면 [서버 방화벽 구성](analysis-services-qs-firewall.md)을 참조 하세요.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Azure Portal을 사용하여 서버 관리자를 추가하려면

1. 포털에서 서버에 대해 **Analysis Services 관리자** 를 클릭합니다.
2. **\<servername> Analysis Services 관리자** 에서 **추가** 를 클릭 합니다.
3. **서버 관리자 추가** 에서 Azure AD의 사용자 계정을 선택하거나 이메일 주소를 통해 외부 사용자를 초대합니다.

    ![Azure Portal의 서버 관리자](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>SSMS를 사용하여 서버 관리자를 추가하려면

1. 서버를 마우스 오른쪽 단추로 클릭하고 **속성** 을 선택합니다.
2. **Analysis Server 속성** 에서 **보안** 을 클릭합니다.
3. **추가** 를 클릭한 다음 Azure AD에 있는 사용자 또는 그룹의 이메일 주소를 입력합니다.
   
    ![SSMS에서 서버 관리자 추가](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

새 서버를 만들 때 [AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet을 사용 하 여 관리자 매개 변수를 지정 합니다. <br>
[AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet을 사용 하 여 기존 서버에 대 한 관리자 매개 변수를 수정 합니다.

## <a name="rest-api"></a>REST API

새 서버를 만들 때 [만들기](/rest/api/analysisservices/servers/create)를 사용하여 asAdministrator 속성을 지정합니다. <br>
기존 서버를 수정할 때 [업데이트](/rest/api/analysisservices/servers/update)를 사용하여 asAdministrator 속성을 지정합니다. <br>



## <a name="next-steps"></a>다음 단계 

[인증 및 사용자 권한](analysis-services-manage-users.md)  
[데이터베이스 역할 및 사용자 관리](analysis-services-database-users.md)  
[Azure RBAC(Azure 역할 기반 액세스 제어)](../role-based-access-control/overview.md)
