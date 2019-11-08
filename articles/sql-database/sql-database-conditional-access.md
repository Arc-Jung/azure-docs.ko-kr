---
title: 조건부 액세스
description: Azure SQL Database 및 데이터 웨어하우스에 대한 조건부 액세스를 구성하는 방법에 대해 알아봅니다.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 03/29/2019
ms.openlocfilehash: 9b8c0dbe03e47d32d8194408663973f07a07b1b9
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827155"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Azure SQL Database 및 데이터 웨어하우스를 사용하여 조건부 액세스(MFA)  

Azure [SQL Database](sql-database-technical-overview.md), [Managed Instance](sql-database-managed-instance.md)및 [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 은 Microsoft 조건부 액세스를 지원 합니다. 

> [!NOTE]
> 이 항목은 Azure SQL 서버 및 Azure SQL 서버에서 생성된 SQL Database와 SQL Data Warehouse 데이터베이스에 적용됩니다. 간단히 하기 위해 SQL Database는 SQL Database와 SQL Data Warehouse를 참조할 때 사용 됩니다.

다음 단계에서는 조건부 액세스 정책을 적용하기 위해 SQL Database를 구성하는 방법을 보여 줍니다.  

## <a name="prerequisites"></a>필수 조건  
- Azure Active Directory 인증을 지원하도록 SQL Database 또는 SQL Data Warehouse를 구성해야 합니다. 자세한 단계는 [SQL Database 또는 SQL Data Warehouse에서 Azure Active Directory 인증 구성 및 관리](sql-database-aad-authentication-configure.md)를 참조하세요.  
- 다단계 인증을 사용하는 경우 최신 SSMS와 같은 지원되는 도구에 연결해야 합니다. 자세한 내용은 [SQL Server Management Studio에 대한 Azure SQL Database 다단계 인증 구성](sql-database-ssms-mfa-authentication-configure.md)을 참조하세요.  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Azure SQL DB/DW에 대한 CA 구성  
1. 포털에 로그인 하 고 **Azure Active Directory**를 선택한 다음 **조건부 액세스**를 선택 합니다. 자세한 내용은 [Azure Active Directory 조건부 액세스 기술 참조](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference)를 참조하세요.  
   조건부 액세스 블레이드를 ![](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. **조건부 액세스 정책** 블레이드에서 **새 정책**을 클릭하고 이름을 입력한 다음 **규칙 구성**을 클릭합니다.  
3. **할당**아래에서 **사용자 및 그룹**을 선택 하 고 **사용자 및 그룹 선택**을 선택 하 고 조건부 액세스를 위한 사용자 또는 그룹을 선택 합니다. **선택**을 클릭한 다음 **완료**를 클릭하여 선택 사항을 적용합니다.  
   ![사용자 및 그룹 선택](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. **클라우드 앱**을 선택하고 **앱 선택**을 클릭합니다. 조건부 액세스에 사용할 수 있는 모든 앱이 표시 됩니다. **Azure SQL Database**를 선택하고 아래쪽에서 **선택**을 클릭한 다음, **완료**를 클릭합니다.  
   ![SQL Database 선택](./media/sql-database-conditional-access/select-sql-database.png)  
   다음 세 번째 스크린샷에서 나열 **Azure SQL Database** 를 찾을 수 없는 경우 다음 단계를 완료 합니다.   
   - SSMS를 사용하여 AAD 관리자 계정으로 Azure SQL DB/DW 인스턴스에 로그인합니다.  
   - `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`를 실행합니다.  
   - AAD에 로그인하고 Azure SQL Database 및 데이터 웨어하우스가 AAD의 애플리케이션에 나열되어 있는지 확인합니다.  

5. **액세스 제어**를 선택하고 **권한 부여**를 선택한 다음 적용하려는 정책을 선택합니다. 예를 들어 **다단계 인증 필요**를 선택합니다.  
   ![액세스 권한 부여 선택](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>요약  
Azure AD Premium을 사용하여 Azure SQL DB/DW에 대한 연결을 허용하는 선택한 애플리케이션(Azure SQL Database)은 이제 선택한 조건부 액세스 정책, **필요한 다단계 인증**을 적용합니다.  
다단계 인증 문제에 관한 Azure SQL Database 및 데이터 웨어하우스에 대한 질문은 MFAforSQLDB@microsoft.com에 문의합니다.  

## <a name="next-steps"></a>다음 단계  

자습서는 [Azure SQL Database 보안](sql-database-security-tutorial.md)을 참조하세요.
