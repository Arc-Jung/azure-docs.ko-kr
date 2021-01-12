---
title: '자습서: 뉴욕 택시 데이터 로드'
description: 자습서에서는 Azure Portal 및 SQL Server Management Studio를 사용 하 여 Synapse SQL 용 Azure blob에서 뉴욕 택시 데이터를 로드 합니다.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 11/23/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: ec577e2a70e2b354b8d2013fe259aa9ea622c50e
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98120141"
---
# <a name="tutorial-load-the-new-york-taxicab-dataset"></a>자습서: 뉴욕 택시 데이터 집합 로드

이 자습서에서는 [COPY 문을](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) 사용 하 여 Azure blob storage 계정에서 뉴욕 택시 데이터 집합을 로드 합니다. 이 자습서에서는 [Azure Portal](https://portal.azure.com) 및 SSMS([SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest))를 사용합니다.

> [!div class="checklist"]
>
> * 데이터 로드용으로 지정된 사용자 만들기
> * 예제 데이터 집합에 대 한 테이블 만들기 
> * COPY T-sql 문을 사용 하 여 데이터 웨어하우스로 데이터 로드
> * 로드될 때 데이터의 진행 상태 확인

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="before-you-begin"></a>시작하기 전에

이 자습서를 시작하기 전에 최신 버전의 SSMS([SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest))를 다운로드하여 설치합니다.  

이 자습서에서는 다음 [자습서](./create-data-warehouse-portal.md#connect-to-the-server-as-server-admin)에서 SQL 전용 풀을 이미 만들었다고 가정 합니다.

## <a name="create-a-user-for-loading-data"></a>데이터를 로드하기 위한 사용자 만들기

서버 관리자 계정은 관리 작업을 수행하며 사용자 데이터에 대해 쿼리를 실행하는 데는 적합하지 않습니다. 데이터 로드는 메모리를 많이 사용하는 작업입니다. 메모리 최대값은 구성 된 [데이터 웨어하우스 단위](what-is-a-data-warehouse-unit-dwu-cdwu.md) 및 [리소스 클래스](resource-classes-for-workload-management.md) 에 따라 정의 됩니다.

데이터 로드 전용 로그인 및 사용자를 만드는 것이 좋습니다. 그런 후 로드 사용자를 [리소스 클래스](resource-classes-for-workload-management.md)에 추가하여 적절한 최대 메모리가 할당되도록 합니다.

로그인 및 사용자를 만들 수 있도록 서버 관리자로 연결 합니다. 다음 단계를 사용하여 **LoaderRC20** 이라는 로그인 및 사용자를 만듭니다. 그런 후 **staticrc20** 리소스 클래스에 해당 사용자를 할당합니다.

1. SSMS에서 **master** 를 마우스 오른쪽 단추로 선택 하 여 드롭다운 메뉴를 표시 하 고 **새 쿼리** 를 선택 합니다. 새 쿼리 창이 열립니다.

    ![master에서의 새 쿼리](./media/load-data-from-azure-blob-storage-using-polybase/create-loader-login.png)

2. 쿼리 창에서 다음 T-SQL 명령을 입력하여 LoaderRC20이라는 로그인 및 사용자를 만들고 'a123STRONGpassword!' 대신 사용자 고유의 암호를 사용합니다.

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    ```

3. **실행** 을 선택합니다.

4. 마우스 오른쪽 단추로 **mySampleDataWarehouse** 를 클릭하고 **새 쿼리** 를 선택합니다. 새 쿼리 창이 열립니다.  

    ![예제 데이터 웨어하우스에 대한 새 쿼리](./media/load-data-from-azure-blob-storage-using-polybase/create-loading-user.png)

5. 다음 T-SQL 명령을 입력하여 LoaderRC20 로그인에 대해 LoaderRC20이라는 데이터베이스 사용자를 만듭니다. 두 번째 줄은 새 데이터 웨어하우스에 대한 제어 권한을 새 사용자에게 부여합니다.  이러한 권한 부여은 해당 사용자를 데이터베이스의 소유자로 만드는 것과 비슷합니다. 세 번째 줄은 staticrc20 [리소스 클래스](resource-classes-for-workload-management.md)의 구성원으로 새 사용자를 추가합니다.

    ```sql
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

6. **실행** 을 선택합니다.

## <a name="connect-to-the-server-as-the-loading-user"></a>로드 사용자 권한으로 서버에 연결

로드 데이터에 대한 첫 번째 단계는 LoaderRC20으로 로그인하는 것입니다.  

1. 개체 탐색기에서 **연결** 드롭다운 메뉴를 선택 하 고 **데이터베이스 엔진** 를 선택 합니다. **서버에 연결** 대화 상자가 표시됩니다.

    ![새 로그인으로 연결](./media/load-data-from-azure-blob-storage-using-polybase/connect-as-loading-user.png)

2. 정규화된 서버 이름을 입력하고, 로그인으로 **LoaderRC20** 을 입력합니다.  LoaderRC20에 대한 암호를 입력합니다.

3. **연결** 을 선택합니다.

4. 연결이 준비되면 개체 탐색기에서 2개의 서버 연결이 표시됩니다. 한 연결은 ServerAdmin을 통한 연결이고 또 다른 연결은 MedRCLogin을 통한 연결입니다.

    ![연결에 성공함](./media/load-data-from-azure-blob-storage-using-polybase/connected-as-new-login.png)

## <a name="create-tables-for-the-sample-data"></a>샘플 데이터에 대 한 테이블 만들기

새 데이터 웨어하우스로 데이터를 로드하는 프로세스를 시작할 준비가 되었습니다. 이 자습서의 부분에서는 COPY 문을 사용 하 여 Azure Storage blob에서 뉴욕 도시 taxi cab 데이터 집합을 로드 하는 방법을 보여 줍니다. 나중에 참조할 수 있도록 데이터를 Azure blob 저장소로 가져오거나 원본에서 직접 로드 하는 방법에 대 한 자세한 내용은 [로드 개요](design-elt-data-loading.md)를 참조 하세요.

다음 SQL 스크립트를 실행 하 고 로드 하려는 데이터에 대 한 정보를 지정 합니다. 이 정보에는 데이터가 있는 위치, 데이터 콘텐츠 형식 및 데이터에 대한 테이블 정의가 포함됩니다.

1. 이전 섹션에서는 LoaderRC20 권한으로 데이터 웨어하우스에 로그인했습니다. SSMS에서 LoaderRC20 연결을 마우스 오른쪽 단추로 클릭하고 **새 쿼리** 를 선택합니다.  새 쿼리 창이 표시됩니다.

    ![새 로드 쿼리 창](./media/load-data-from-azure-blob-storage-using-polybase/new-loading-query.png)

2. 쿼리 창을 이전 이미지와 비교합니다.  새 쿼리 창이 LoaderRC20 권한으로 실행되고 있으며 MySampleDataWarehouse 데이터베이스에서 쿼리를 수행하고 있는지 확인합니다. 이 쿼리 창을 사용하여 모든 로드 단계를 수행합니다.

7. 다음 T-sql 문을 실행 하 여 테이블을 만듭니다.

    ```sql
    CREATE TABLE [dbo].[Date]
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    
    CREATE TABLE [dbo].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    );
    ```
    

## <a name="load-the-data-into-your-data-warehouse"></a>데이터 웨어하우스로 데이터 로드

이 섹션에서는 [COPY 문을](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) 사용 하 여 Azure Storage Blob에서 샘플 데이터를 로드 합니다.  

> [!NOTE]
> 이 자습서에서는 최종 테이블에 직접 데이터를 로드합니다. 일반적으로 프로덕션 워크 로드에 대 한 준비 테이블로 로드 합니다. 데이터가 준비 테이블에 있는 동안에는 필요한 모든 변환을 수행할 수 있습니다. 

1. 다음 문을 실행 하 여 데이터를 로드 합니다.

    ```sql
    COPY INTO [dbo].[Date]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Date'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = ''
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Date] - Taxi dataset');
    
    
    COPY INTO [dbo].[Geography]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Geography'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = ''
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Geography] - Taxi dataset');
    
    COPY INTO [dbo].[HackneyLicense]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/HackneyLicense'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = ''
    )
    OPTION (LABEL = 'COPY : Load [dbo].[HackneyLicense] - Taxi dataset');
    
    COPY INTO [dbo].[Medallion]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Medallion'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = ''
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Medallion] - Taxi dataset');
    
    COPY INTO [dbo].[Time]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Time'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = ''
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Time] - Taxi dataset');
    
    COPY INTO [dbo].[Weather]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Weather'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = ',',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Weather] - Taxi dataset');
    
    COPY INTO [dbo].[Trip]
    FROM 'https://nytaxiblob.blob.core.windows.net/2013/Trip2013'
    WITH
    (
        FILE_TYPE = 'CSV',
        FIELDTERMINATOR = '|',
        FIELDQUOTE = '',
        ROWTERMINATOR='0X0A',
        COMPRESSION = 'GZIP'
    )
    OPTION (LABEL = 'COPY : Load [dbo].[Trip] - Taxi dataset');
    ```

2. 로드되는 데이터를 봅니다. 데이터의 여러 Gb를 로드 하 고 성능이 뛰어난 클러스터형 columnstore 인덱스로 압축 합니다. DMV(동적 관리 보기)를 사용하는 다음과 같은 쿼리를 실행하여 로드 상태를 봅니다.

    ```sql
    SELECT  r.[request_id]                           
    ,       r.[status]                               
    ,       r.resource_class                         
    ,       r.command
    ,       sum(bytes_processed) AS bytes_processed
    ,       sum(rows_processed) AS rows_processed
    FROM    sys.dm_pdw_exec_requests r
                  JOIN sys.dm_pdw_dms_workers w
                         ON r.[request_id] = w.request_id
    WHERE [label] = 'COPY : Load [dbo].[Date] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[Geography] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[HackneyLicense] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[Medallion] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[Time] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[Weather] - Taxi dataset' OR
        [label] = 'COPY : Load [dbo].[Trip] - Taxi dataset' 
    and session_id <> session_id() and type = 'WRITER'
    GROUP BY r.[request_id]                           
    ,       r.[status]                               
    ,       r.resource_class                         
    ,       r.command;
    ```
    
3. 모든 시스템 쿼리를 표시합니다.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. 데이터 웨어하우스에 잘 로드된 데이터를 보세요.

    ![로드된 테이블 보기](./media/load-data-from-azure-blob-storage-using-polybase/view-loaded-tables.png)

## <a name="clean-up-resources"></a>리소스 정리

컴퓨팅 리소스 및 데이터 웨어하우스에 로드한 데이터에 대해 요금이 청구됩니다. 이러한 요금은 별도로 청구됩니다.

* 데이터를 스토리지에 보관하려는 경우 데이터 웨어하우스를 사용하지 않을 때 컴퓨팅을 일시 중지할 수 있습니다. 컴퓨팅을 일시 중지하면 데이터 스토리지에 대해서만 요금이 부과되며, 데이터를 사용할 준비가 될 때마다 컴퓨팅을 재개할 수 있습니다.
* 앞으로 요금이 부과되지 않게 하려면 데이터 웨어하우스를 삭제하면 됩니다.

필요에 따라 다음 단계에 따라 리소스를 정리합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인 하 고 데이터 웨어하우스를 선택 합니다.

    ![리소스 정리](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. 컴퓨팅을 일시 중지하려면 **일시 중지** 단추를 선택합니다. 데이터 웨어하우스가 일시 중지되면 **시작** 단추가 표시됩니다.  컴퓨팅을 다시 시작하려면 **시작** 을 선택합니다.

3. 계산 또는 저장소에 대 한 요금이 청구 되지 않도록 데이터 웨어하우스를 제거 하려면 **삭제** 를 선택 합니다.

4. 만든 서버를 제거 하려면 이전 이미지에서 **mynewserver-20180430.database.windows.net** 를 선택 하 고 **삭제** 를 선택 합니다.  서버를 삭제하면 서버에 할당된 모든 데이터베이스가 삭제되므로 주의해야 합니다.

5. 리소스 그룹을 제거하려면 **myResourceGroup** 을 선택한 다음, **리소스 그룹 삭제** 를 선택합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 데이터를 로드하기 위해 데이터 웨어하우스를 만들고 사용자를 만드는 방법을 배웠습니다. 단순 [복사 문을](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#examples) 사용 하 여 데이터 웨어하우스에 데이터를 로드 했습니다.

다음 작업을 수행했습니다.
> [!div class="checklist"]
>
> * Azure Portal에서 데이터 웨어하우스 생성
> * Azure Portal에서 서버 수준 방화벽 규칙 설정
> * SSMS로 데이터 웨어하우스에 연결
> * 데이터 로드용으로 지정된 사용자 생성
> * 샘플 데이터에 대 한 테이블을 만들었습니다.
> * 복사 T-sql 문을 사용 하 여 데이터 웨어하우스에 데이터를 로드 합니다.
> * 로드될 때 데이터의 진행 상태 확인

개발 개요로 이동 하 여 기존 데이터베이스를 Azure Synapse Analytics로 마이그레이션하는 방법에 대해 알아봅니다.

> [!div class="nextstepaction"]
> [기존 데이터베이스를 Azure Synapse Analytics로 마이그레이션하기 위한 설계 결정](sql-data-warehouse-overview-develop.md)

예제 및 참조를 로드 하는 방법에 대 한 자세한 내용은 다음 설명서를 참조 하세요.

- [COPY 문 참조 설명서](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#syntax)
- [각 인증 방법에 대 한 복사 예제](./quickstart-bulk-load-copy-tsql-examples.md)
- [단일 테이블에 대 한 빠른 시작 복사](./quickstart-bulk-load-copy-tsql.md)