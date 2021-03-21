---
title: 덤프 및 복원을 사용 하 여 업그레이드-Azure Database for PostgreSQL-단일 서버
description: 덤프 및 복원 데이터베이스를 사용 하 여 Azure Database for PostgreSQL-단일 서버를 상위 버전으로 마이그레이션하기 위한 오프 라인 업그레이드 방법에 대해 설명 합니다.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 11/10/2020
ms.openlocfilehash: 42bbe1c9f4056ae0dae0ccd59b452db90a7c63c5
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96493664"
---
# <a name="upgrade-your-postgresql-database-using-dump-and-restore"></a>덤프 및 복원을 사용 하 여 PostgreSQL 데이터베이스 업그레이드

다음 방법을 사용 하 여 데이터베이스를 더 높은 주 버전 서버로 마이그레이션하여 Azure Database for PostgreSQL 단일 서버에 배포 된 PostgreSQL 서버를 업그레이드할 수 있습니다.
* PostgreSQL [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 및 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) 를 사용 하는 **오프 라인 방법으로** 데이터 마이그레이션에 대 한 가동 중지 시간이 발생 합니다. 이 문서에서는이 업그레이드/마이그레이션 방법에 대해 다룹니다.
* [Database Migration Service](../dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal.md) (DMS)를 사용 하는 **온라인** 메서드입니다. 이 방법을 사용 하면 가동 중지 시간을 줄일 수 있으며 대상 데이터베이스를 원본과 동기화 상태로 유지 하 고,이를 잘라낼 시기를 선택할 수 있습니다. 그러나 DMS를 사용 하기 위해 몇 가지 필수 구성 요소 및 제한 사항을 해결 해야 합니다. 자세한 내용은 [DMS 설명서](../dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal.md)를 참조 하세요. 

 다음 표에서는 데이터베이스 크기 및 시나리오에 따른 몇 가지 권장 사항을 제공 합니다.

| **데이터베이스/시나리오** | **덤프/복원 (오프 라인)** | **DMS (온라인)** |
| ------ | :------: | :-----: |
| 작은 데이터베이스가 있고 업그레이드 하는 데 가동 중지 시간을 감당할 수 있습니다.  | X | |
| 소규모 데이터베이스 (< 10gb)  | X | X | 
| 중소 Db (10gb – 100 g b) | X | X |
| 대량 데이터베이스 (> 100 GB) |  | X |
| 데이터베이스 크기와 관계 없이 업그레이드 하는 데 가동 중지 시간을 감당할 수 있습니다. | X |  |
| 다시 부팅을 포함 하 여 DMS [필수](../dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal.md#prerequisites)구성 요소를 처리할 수 있나요? |  | X |
| 업그레이드 프로세스 중에 DDLs 및 기록 되지 않은 테이블을 피할 수 있나요? | |  X |

이 가이드에는 원본 서버에서 더 높은 버전의 PostgreSQL를 실행 하는 대상 서버로 마이그레이션하는 방법을 보여 주는 몇 가지 오프 라인 마이그레이션 방법과 예제가 나와 있습니다.

> [!NOTE]
> PostgreSQL 덤프 및 복원은 여러 가지 방법으로 수행할 수 있습니다. 이 가이드에 제공 된 방법 중 하나를 사용 하 여 마이그레이션하도록 선택 하거나 필요에 따라 다른 방법을 선택할 수 있습니다. 추가 매개 변수를 사용 하는 자세한 덤프 및 복원 구문은 [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 및 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)문서를 참조 하세요. 


## <a name="prerequisites-for-using-dump-and-restore-with-azure-database-for-postgresql"></a>Azure Database for PostgreSQL에서 덤프 및 복원을 사용 하기 위한 필수 구성 요소
 
이 방법 가이드를 단계별로 실행 하려면 다음이 필요 합니다.

- 업그레이드 하려는 9.5, 9.6 또는 10을 실행 하는 **원본** PostgreSQL 데이터베이스
- 원하는 주 버전 [Azure Database for PostgreSQL 서버](quickstart-create-server-database-portal.md)를 사용 하는 **대상** PostgreSQL 데이터베이스 서버입니다. 
- 덤프 및 복원 명령을 실행할 PostgreSQL 클라이언트 시스템입니다.
  - PostgreSQL이 설치 된 Linux 또는 Windows 클라이언트 이며 [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) 및 [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) 명령줄 유틸리티가 설치 되어 있을 수 있습니다. 
  - 또는 [Azure Cloud Shell](https://shell.azure.com) 를 사용 하거나 [Azure Portal](https://portal.azure.com)의 오른쪽 위에 있는 메뉴 모음에서 Azure Cloud Shell를 클릭 하 여 사용할 수 있습니다. `az login`덤프 및 복원 명령을 실행 하기 전에 계정에 로그인 해야 합니다.
- PostgreSQL 클라이언트는 원본 및 대상 서버와 동일한 지역에서 실행 되는 것이 좋습니다. 


## <a name="additional-details-and-considerations"></a>추가 세부 정보 및 고려 사항
- 포털에서 "연결 문자열"을 클릭 하 여 원본 및 대상 데이터베이스에 대 한 연결 문자열을 찾을 수 있습니다. 
- 서버에서 둘 이상의 데이터베이스를 실행 하 고 있을 수 있습니다. 원본 서버에 연결 하 고를 실행 하 여 데이터베이스 목록을 찾을 수 있습니다 `\l` .
- 대상 데이터베이스 서버에 해당 데이터베이스를 만듭니다.
- 업그레이드 또는 템플릿 데이터베이스를 건너뛸 수 있습니다 `azure_maintenance` .
- 이 마이그레이션 모드에 적합 한 데이터베이스를 확인 하려면 위의 표를 참조 하세요.
- Azure Cloud Shell를 사용 하려면 20 분 후 세션 시간이 초과 되는 것을 확인 하세요. 데이터베이스 크기가 10gb < 경우 세션 시간이 초과 되지 않고 업그레이드를 완료할 수 있습니다. 그렇지 않으면 <Enter> 10-15 분에 한 번 키 누르기와 같이 다른 방법으로 세션을 열어 두어야 할 수 있습니다. 


## <a name="example-database-used-in-this-guide"></a>이 가이드에 사용 되는 예제 데이터베이스

이 가이드에서는 예제를 보여 주기 위해 다음 원본 및 대상 서버와 데이터베이스 이름을 사용 합니다.

 | **설명** | **값** |
 | ------- | ------- |
 | 원본 서버 (v 9.5) | pg-95.postgres.database.azure.com |
 | 원본 데이터베이스 | bench5gb |
 | 원본 데이터베이스 크기 | 5GB |
 | 원본 사용자 이름 | pg@pg-95 |
 | 대상 서버 (v11) | pg-11.postgres.database.azure.com |
 | 대상 데이터베이스 | bench5gb |
 | 대상 사용자 이름 | pg@pg-11 |

## <a name="upgrade-your-databases-using-offline-migration-methods"></a>오프 라인 마이그레이션 방법을 사용 하 여 데이터베이스 업그레이드
업그레이드에 대해이 섹션에서 설명 하는 방법 중 하나를 사용 하도록 선택할 수 있습니다. 작업을 수행 하는 동안 다음 팁을 사용할 수 있습니다.

- 원본 및 대상 데이터베이스에 대해 동일한 암호를 사용 하는 경우 환경 변수를 설정할 수 있습니다 `PGPASSWORD=yourPassword` .  그런 다음 psql, pg_dump 및 pg_restore와 같은 명령을 실행할 때마다 암호를 제공할 필요가 없습니다.  마찬가지로, 등의 추가 변수를 설정할 수 있습니다 `PGUSER` `PGSSLMODE` . [PostgreSQL 환경 변수](https://www.postgresql.org/docs/11/libpq-envars.html)를 참조 하세요.
  
- PostgreSQL 서버에 TLS/SSL 연결이 필요한 경우 (기본적으로 Azure Database for PostgreSQL 서버의 경우) `PGSSLMODE=require` pg_restore 도구가 tls와 연결 되도록 환경 변수를 설정 합니다. TLS를 사용 하지 않으면 오류를 읽을 수 있습니다.  `FATAL:  SSL connection is required. Please specify SSL options and retry.`

- Windows 명령줄에서 pg_restore 명령을 실행하기 전에 명령 `SET PGSSLMODE=require`를 실행합니다. Linux 또는 Bash에서 pg_restore 명령을 실행하기 전에 명령 `export PGSSLMODE=require`를 실행합니다.

### <a name="method-1-migrate-using-dump-file"></a>방법 1: 덤프 파일을 사용 하 여 마이그레이션

이 메서드에는 두 단계가 포함 됩니다. 먼저 원본 서버에서 덤프를 만듭니다. 두 번째 단계는 덤프 파일을 대상 서버로 복원 하는 것입니다. 자세한 내용은 [덤프 및 복원 설명서를 사용 하 여 마이그레이션](howto-migrate-using-dump-and-restore.md) 을 참조 하세요. 데이터베이스가 크고 클라이언트 시스템에 덤프 파일을 저장할 수 있는 충분 한 저장소가 있는 경우이 방법이 권장 됩니다.

### <a name="method-2-migrate-using-streaming-the-dump-data-to-the-target-database"></a>방법 2: 덤프 데이터 스트리밍을 사용 하 여 대상 데이터베이스에 마이그레이션

PostgreSQL 클라이언트가 없거나 Azure Cloud Shell를 사용 하려는 경우이 방법을 사용할 수 있습니다. 데이터베이스 덤프는 대상 데이터베이스 서버로 직접 스트리밍되 며 덤프를 클라이언트에 저장 하지 않습니다. 따라서 저장소는 제한 된 클라이언트와 함께 사용할 수 있으며 Azure Cloud Shell 에서도 실행할 수 있습니다. 

1. 명령을 사용 하 여 대상 서버에 데이터베이스가 있는지 확인 `\l` 합니다. 데이터베이스가 없는 경우 데이터베이스를 만듭니다.
   ```azurecli-interactive
    psql "host=myTargetServer port=5432 dbname=postgres user=myUser password=###### sslmode=mySSLmode"
    ```
    ```SQL
    postgres> \l   
    postgres> create database myTargetDB;
   ```

2. 파이프를 사용 하 여 덤프를 실행 하 고 하나의 명령줄로 복원 합니다. 
    ```azurecli-interactive
    pg_dump -Fc -v --mySourceServer --port=5432 --username=myUser --dbname=mySourceDB | pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB
    ```

    예제:

    ```azurecli-interactive
    pg_dump -Fc -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb | pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb
    ```  
3. 업그레이드 (마이그레이션) 프로세스가 완료 되 면 대상 서버를 사용 하 여 응용 프로그램을 테스트할 수 있습니다. 
4. 서버 내의 모든 데이터베이스에 대해이 프로세스를 반복 합니다.

 예를 들어 다음 표에서는 streaming dump 메서드를 사용 하 여 마이그레이션하는 데 걸린 시간을 보여 줍니다. 이 샘플 데이터는 [pgbench](https://www.postgresql.org/docs/10/pgbench.html)를 사용 하 여 채워집니다. 데이터베이스의 개체 수가 pgbench에서 생성 된 테이블 및 인덱스 보다 크기가 다를 수 있으므로 데이터베이스를 업그레이드 하는 데 걸리는 실제 시간을 이해 하기 위해 데이터베이스의 덤프와 복원을 테스트 하는 것이 좋습니다. 

| **데이터베이스 크기** | **대략적인 시간** | 
| ----- | ------ |
| 1GB  | 1-2 분 |
| 5GB | 8-10 분 |
| 10GB | 15-20분 |
| 50GB | 1-1.5 시간 |
| 100GB | 2.5-3 시간|
   
### <a name="method-3-migrate-using-parallel-dump-and-restore"></a>방법 3: 병렬 덤프 및 복원을 사용 하 여 마이그레이션 

데이터베이스에 더 많은 테이블이 있고 해당 데이터베이스의 덤프 및 복원 프로세스를 병렬화 하려는 경우이 방법을 고려할 수 있습니다. 또한 백업 덤프를 수용할 수 있도록 클라이언트 시스템에 충분 한 저장소가 필요 합니다. 이러한 병렬 덤프 및 복원 프로세스는 전체 마이그레이션을 완료 하는 데 걸리는 시간을 줄입니다. 예를 들어 마이그레이션하는 데 1-1.5 시간이 걸린 50 기가바이트의 데이터베이스는이 메서드를 사용 하 여 30 분 이내에 완료 되었습니다.

1. 원본 서버의 각 데이터베이스에 대해 대상 서버에 해당 데이터베이스를 만듭니다.

   ```bash
    psql "host=myTargetServer port=5432 dbname=postgres user=myuser password=###### sslmode=mySSLmode"
    postgresl> create database myDB;
   ```
   예제:
    ```bash
    psql "host=pg-11.postgres.database.azure.com port=5432 dbname=postgres user=pg@pg-11 password=###### sslmode=require"

    postgres> create database bench5gb;
    postgres> \q
    ```

2. 작업 수 = 4 (데이터베이스의 테이블 수)를 사용 하 여 디렉터리 형식으로 pg_dump 명령을 실행 합니다. 더 큰 계산 계층과 더 많은 테이블이 있는 경우 더 큰 값으로 늘릴 수 있습니다. 이 pg_dump는 각 작업에 대해 압축 된 파일을 저장할 디렉터리를 만듭니다.

    ```bash
    pg_dump -Fd -v --host=sourceServer --port=5432 --username=myUser --dbname=mySourceDB -j 4 -f myDumpDirectory
    ```
    예제:
    ```bash
    pg_dump -Fd -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb -j 4 -f dump.dir
    ```

3. 그런 다음 대상 서버에서 백업을 복원 합니다.
    ```bash
    $ pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB -j 4 myDumpDir
    ```
    예제:
    ```bash
    $ pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb -j 4 dump.dir
    ```

> [!TIP]
> 이 문서에서 설명 하는 프로세스를 사용 하 여 미리 보기 상태인 Azure Database for PostgreSQL 유연한 서버를 업그레이드할 수도 있습니다. 주요 차이점은를 제외 하 고 유연한 서버 대상에 대 한 연결 문자열입니다 `@dbName` .  예를 들어 사용자 이름이 인 경우 `pg` 연결 문자열의 단일 서버 사용자 이름은이 고 `pg@pg-95` , 유연한 서버에서는를 사용 하기만 하면 됩니다 `pg` .

## <a name="next-steps"></a>다음 단계

- 대상 데이터베이스 함수에 만족 했으면 이전 데이터베이스 서버를 삭제할 수 있습니다. 
- 원본 서버와 동일한 데이터베이스 끝점을 사용 하려는 경우 이전 원본 데이터베이스 서버를 삭제 한 후에는 이전 데이터베이스 서버 이름을 사용 하 여 읽기 복제본을 만들 수 있습니다. 안정적인 상태가 설정 되 면 복제본을 중지할 수 있습니다. 그러면 복제본 서버가 독립 서버로 승격 됩니다. 자세한 내용은 [복제](./concepts-read-replicas.md)를 참조하세요.
- 이러한 명령은 프로덕션 환경에서 사용하기 전에 테스트 환경에서 테스트하여 유효성을 검사해야 합니다.