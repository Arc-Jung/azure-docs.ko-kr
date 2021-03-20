---
title: Azure SQL Managed Instance 에이전트를 사용 하 여 SSIS 패키지 실행
description: Azure SQL Managed Instance 에이전트를 사용 하 여 SSIS 패키지를 실행 하는 방법을 알아봅니다.
ms.service: data-factory
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: 916d799ba08f46cb86ee2e22c4af7fc1b92b385f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100386155"
---
# <a name="run-ssis-packages-by-using-azure-sql-managed-instance-agent"></a>Azure SQL Managed Instance 에이전트를 사용 하 여 SSIS 패키지 실행

이 문서에서는 Azure SQL Managed Instance 에이전트를 사용 하 여 SSIS (SQL Server Integration Services) 패키지를 실행 하는 방법을 설명 합니다. 이 기능은 온-프레미스 환경에서 SQL Server 에이전트를 사용 하 여 SSIS 패키지를 예약 하는 경우와 유사한 동작을 제공 합니다.

이 기능을 사용 하 여 SQL Managed Instance, Azure Files 같은 파일 시스템 또는 Azure SSIS integration runtime 패키지 저장소에 저장 된 SSIS 패키지를 실행할 수 있습니다.

## <a name="prerequisites"></a>필수 구성 요소

이 기능을 사용 하려면 최신 SQL Server Management Studio (SSMS)를 [다운로드](/sql/ssms/download-sql-server-management-studio-ssms) 하 여 설치 합니다. 버전 지원 세부 정보:

- SSISDB 또는 파일 시스템에서 패키지를 실행 하려면 SSMS 버전 18.5 이상을 설치 합니다.
- 패키지 저장소에서 패키지를 실행 하려면 SSMS 버전 18.6 이상을 설치 합니다.

또한 Azure Data Factory에서 [AZURE SSIS 통합 런타임을 프로 비전](./tutorial-deploy-ssis-packages-azure.md) 해야 합니다. SQL Managed Instance를 끝점 서버로 사용 합니다.

## <a name="run-an-ssis-package-in-ssisdb"></a>SSISDB에서 SSIS 패키지 실행

이 절차에서는 SQL Managed Instance 에이전트를 사용 하 여 SSISDB에 저장 된 SSIS 패키지를 호출 합니다.

1. 최신 버전의 SSMS에서 SQL Managed Instance에 연결 합니다.
1. 새 에이전트 작업 및 새 작업 단계를 만듭니다. **SQL Server 에이전트** 에서 **작업** 폴더를 마우스 오른쪽 단추로 클릭 한 다음 **새 작업** 을 선택 합니다.

   ![새 에이전트 작업을 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **새 작업 단계** 페이지에서 형식으로 **SQL Server Integration Services 패키지** 를 선택 합니다.

   ![새 SSIS 작업 단계를 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **패키지** 탭에서 **SSIS 카탈로그** 를 패키지 위치로 선택 합니다.
1. SSISDB는 SQL Managed Instance에 있으므로 인증을 지정할 필요가 없습니다.
1. SSISDB에서 SSIS 패키지를 지정 합니다.

   ![패키지 원본 유형에 대 한 선택 항목이 포함 된 패키지 탭](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

1. **구성** 탭에서 다음을 수행할 수 있습니다.
  
   - **매개 변수 아래에** 매개 변수 값을 지정 합니다.
   - **연결 관리자** 아래에서 값을 재정의 합니다.
   - 속성을 재정의 하 고 **고급** 에서 로깅 수준을 선택 합니다.

   ![패키지 원본 유형에 대 한 선택 항목이 포함 된 구성 탭](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

1. **확인** 을 선택 하 여 에이전트 작업 구성을 저장 합니다.
1. 에이전트 작업을 시작 하 여 SSIS 패키지를 실행 합니다.

## <a name="run-an-ssis-package-in-the-file-system"></a>파일 시스템에서 SSIS 패키지 실행

이 절차에서는 SQL Managed Instance 에이전트를 사용 하 여 파일 시스템에 저장 된 SSIS 패키지를 실행 합니다.

1. 최신 버전의 SSMS에서 SQL Managed Instance에 연결 합니다.
1. 새 에이전트 작업 및 새 작업 단계를 만듭니다. **SQL Server 에이전트** 에서 **작업** 폴더를 마우스 오른쪽 단추로 클릭 한 다음 **새 작업** 을 선택 합니다.

   ![새 에이전트 작업을 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **새 작업 단계** 페이지에서 형식으로 **SQL Server Integration Services 패키지** 를 선택 합니다.

   ![새 SSIS 작업 단계를 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **패키지** 탭에서 다음을 수행 합니다.

   1. **패키지 위치** 에서 **파일 시스템** 을 선택 합니다.

   1. **파일 원본 유형**:

      - 패키지가 Azure Files에 업로드 된 경우 **Azure 파일 공유** 를 선택 합니다.

        ![파일 원본 유형에 대 한 옵션](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)

        패키지 **`\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`** 경로가 인 경우

        **패키지 파일 액세스 자격 증명** 에서 azure 파일 계정 이름 및 계정 키를 입력 하 여 azure 파일에 액세스 합니다. 도메인은 **Azure** 로 설정 됩니다.

      - 패키지를 네트워크 공유에 업로드 하는 경우 **네트워크 공유** 를 선택 합니다.

        패키지 경로는 package.dtsx 확장명을 사용 하는 패키지 파일의 UNC 경로입니다.

        해당 도메인, 사용자 이름 및 암호를 입력 하 여 네트워크 공유 패키지 파일에 액세스 합니다.
   1. 패키지 파일이 암호로 암호화 된 경우 **암호화 암호** 를 선택 하 고 암호를 입력 합니다.
1. 구성 파일이 SSIS 패키지를 실행 하는 데 필요한 경우 **구성 탭에서** 구성 파일 경로를 입력 합니다.
   Azure Files에 구성을 저장 하는 경우 해당 구성 경로는 **`\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`** 입니다.
1. **실행 옵션** 탭에서 **Windows 인증** 또는 **32 비트 런타임을** 사용 하 여 SSIS 패키지를 실행할지 여부를 선택할 수 있습니다.
1. **로깅** 탭에서 로깅 경로 및 해당 로깅 액세스 자격 증명을 선택 하 여 로그 파일을 저장할 수 있습니다. 
   기본적으로 로깅 경로는 패키지 폴더 경로와 같으며 로깅 액세스 자격 증명은 패키지 액세스 자격 증명과 동일 합니다.
   Azure Files에 로그를 저장 하는 경우 로깅 경로는가 됩니다 **`\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`** .
1. **값 설정** 탭에서 속성 경로 및 값을 입력 하 여 패키지 속성을 재정의할 수 있습니다.

   예를 들어 사용자 변수의 값을 재정의 하려면 다음 형식으로 해당 경로를 입력 **`\Package.Variables[User::<variable name>].Value`** 합니다.
1. **확인** 을 선택 하 여 에이전트 작업 구성을 저장 합니다.
1. 에이전트 작업을 시작 하 여 SSIS 패키지를 실행 합니다.

## <a name="run-an-ssis-package-in-the-package-store"></a>패키지 저장소에서 SSIS 패키지 실행

이 절차에서는 SQL Managed Instance 에이전트를 사용 하 여 Azure-SSIS IR 패키지 저장소에 저장 된 SSIS 패키지를 실행 합니다.

1. 최신 버전의 SSMS에서 SQL Managed Instance에 연결 합니다.
1. 새 에이전트 작업 및 새 작업 단계를 만듭니다. **SQL Server 에이전트** 에서 **작업** 폴더를 마우스 오른쪽 단추로 클릭 한 다음 **새 작업** 을 선택 합니다.

   ![새 에이전트 작업을 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **새 작업 단계** 페이지에서 형식으로 **SQL Server Integration Services 패키지** 를 선택 합니다.

   ![새 SSIS 작업 단계를 만들기 위한 선택 항목](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **패키지** 탭에서 다음을 수행 합니다.

   1. **패키지 위치** 에서 **패키지 저장소** 를 선택 합니다.

   1. **패키지 경로**:

      패키지 **`<package store name>\<folder name>\<package name>`** 경로가 인 경우

      ![패키지 저장소 형식에 대 한 옵션](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-package-store.png)

   1. 패키지 파일이 암호로 암호화 된 경우 **암호화 암호** 를 선택 하 고 암호를 입력 합니다.
1. 구성 파일이 SSIS 패키지를 실행 하는 데 필요한 경우 **구성 탭에서** 구성 파일 경로를 입력 합니다.
   Azure Files에 구성을 저장 하는 경우 해당 구성 경로는 **`\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`** 입니다.
1. **실행 옵션** 탭에서 **Windows 인증** 또는 **32 비트 런타임을** 사용 하 여 SSIS 패키지를 실행할지 여부를 선택할 수 있습니다.
1. **로깅** 탭에서 로깅 경로 및 해당 로깅 액세스 자격 증명을 선택 하 여 로그 파일을 저장할 수 있습니다.
   기본적으로 로깅 경로는 패키지 폴더 경로와 같으며 로깅 액세스 자격 증명은 패키지 액세스 자격 증명과 동일 합니다.
   Azure Files에 로그를 저장 하는 경우 로깅 경로는가 됩니다 **`\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`** .
1. **값 설정** 탭에서 속성 경로 및 값을 입력 하 여 패키지 속성을 재정의할 수 있습니다.

   예를 들어 사용자 변수의 값을 재정의 하려면 다음 형식으로 해당 경로를 입력 **`\Package.Variables[User::<variable name>].Value`** 합니다.
1. **확인** 을 선택 하 여 에이전트 작업 구성을 저장 합니다.
1. 에이전트 작업을 시작 하 여 SSIS 패키지를 실행 합니다.

## <a name="cancel-ssis-package-execution"></a>SSIS 패키지 실행 취소

SQL Managed Instance 에이전트 작업에서 패키지 실행을 취소 하려면 에이전트 작업을 직접 중지 하는 대신 다음 단계를 수행 합니다.

1. **msdb.dbo.sys작업** 에서 SQL 에이전트 **jobId** 를 찾습니다.
1. 이 쿼리를 사용 하 여 작업 ID를 기반으로 해당 SSIS **Executionid** 를 찾습니다.
   ```sql
   select * from '{table for job execution}' where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
   ```
   SSIS 패키지가 SSISDB에 있는 경우 작업 실행을 위해 **ssisdb.internal.execution_parameter_values** 를 테이블로 사용 합니다. SSIS 패키지가 파일 시스템에 있는 경우 **ssisdb.internal.execution_parameter_values_noncatalog** 를 사용 합니다.
1. SSISDB 카탈로그를 마우스 오른쪽 단추로 클릭 한 다음 **활성 작업** 을 선택 합니다.

   ![SSISDB 카탈로그에 대 한 바로 가기 메뉴의 "활성 작업"](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

1. **Executionid** 를 기준으로 해당 작업을 중지 합니다.

## <a name="next-steps"></a>다음 단계
Azure Data Factory를 사용 하 여 SSIS 패키지를 예약할 수도 있습니다. 단계별 지침은 [Azure Data Factory 이벤트 트리거](how-to-create-event-trigger.md)를 참조 하세요.