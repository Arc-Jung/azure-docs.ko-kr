---
title: 제한 사항 - Azure Database for MySQL
description: 이 문서에서는 Azure Database for MySQL에 대한 연결 수 및 스토리지 엔진 옵션과 같은 제한 사항을 설명합니다.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 10/1/2020
ms.openlocfilehash: 9b18b24686908ac92f97ea0cae892369919ae4d6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101721023"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Azure Database for MySQL의 제한 사항
다음 섹션에서는 데이터베이스 서비스의 용량, 스토리지 엔진 지원, 권한 지원, 데이터 조작 명령문 지원 및 기능 제한 사항에 대해 설명합니다. 또한 MySQL 데이터베이스 엔진에 적용할 수 있는 [일반적인 제한 사항](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html)도 참조하세요.

## <a name="server-parameters"></a>서버 매개 변수

> [!NOTE]
> 및와 같은 서버 매개 변수에 대 한 최소/최대 값을 찾고 있는 경우 `max_connections` `innodb_buffer_pool_size` 이 정보는 **[서버 매개 변수](./concepts-server-parameters.md)** 문서로 이동 되었습니다.

Azure Database for MySQL 서버 매개 변수 값의 튜닝을 지원 합니다. 일부 매개 변수의 min 및 max 값 (예: `max_connections`, `join_buffer_size` , `query_cache_size` )는 서버의 가격 책정 계층 및 vcores에 의해 결정 됩니다. 이러한 제한에 대 한 자세한 내용은 [서버 매개 변수](./concepts-server-parameters.md) 를 참조 하세요.

초기 배포 시 Azure for MySQL 서버는 표준 시간대 정보에 대 한 시스템 테이블을 포함 하지만 이러한 테이블은 채워지지 않습니다. MySQL 명령줄 또는 MySQL Workbench와 같은 도구에서 `mysql.az_load_timezone` 저장 프로시저를 호출하여 표준 시간대 테이블을 채울 수 있습니다. 저장 프로시저를 호출하고 글로벌 또는 세션 수준 표준 시간대를 설정하는 방법은 [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) 또는 [Azure CLI](howto-configure-server-parameters-using-cli.md#working-with-the-time-zone-parameter) 문서를 참조하세요.

"Validate_password" 및 "caching_sha2_password"와 같은 암호 플러그 인은 서비스에서 지원 되지 않습니다.

## <a name="storage-engines"></a>저장소 엔진

MySQL은 많은 저장소 엔진을 지원 합니다. Azure Database for MySQL에서 지원 되 고 지원 되지 않는 저장소 엔진은 다음과 같습니다.

### <a name="supported"></a>지원됨
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>지원되지 않음
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERATED](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privileges--data-manipulation-support"></a>데이터 조작 지원 & 권한

많은 서버 매개 변수 및 설정이 실수로 서버 성능을 저하 시키거나 MySQL 서버의 ACID 속성을 무시할 수 있습니다. 제품 수준에서 서비스 무결성 및 SLA를 유지 하기 위해이 서비스는 여러 역할을 노출 하지 않습니다. 

MySQL 서비스는 기본 파일 시스템에 대 한 직접 액세스를 허용 하지 않습니다. 일부 데이터 조작 명령은 지원 되지 않습니다. 

### <a name="unsupported"></a>지원되지 않음

지원 되지 않는 기능은 다음과 같습니다.
- DBA 역할: 제한 됨 또는 새 서버를 만드는 동안 만들어진 관리자 사용자를 사용 하 여 대부분의 DDL 및 DML 문을 수행할 수 있습니다. 
- SUPER 권한: 마찬가지로 [슈퍼 권한도](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) 제한 됩니다.
- DEFINER: 생성하려면 SUPER 권한이 필요하며, 제한됩니다. 백업을 사용하여 데이터를 가져올 경우 mysqldump를 수행할 때 수동으로 또는 `--skip-definer` 명령을 사용하여 `CREATE DEFINER` 명령을 제거하세요.
- 시스템 데이터베이스: [mysql 시스템 데이터베이스](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) 는 읽기 전용 이며 다양 한 PaaS 기능을 지 원하는 데 사용 됩니다. 시스템 데이터베이스를 변경할 수 없습니다 `mysql` .
- `SELECT ... INTO OUTFILE`: 서비스에서 지원 되지 않습니다.
- `LOAD_FILE(file_name)`: 서비스에서 지원 되지 않습니다.

### <a name="supported"></a>지원됨
- `LOAD DATA INFILE`은 지원되지만 `[LOCAL]` 매개 변수를 지정하고 UNC 경로(SMB를 통해 탑재된 Azure Storage)로 전달해야 합니다.

## <a name="functional-limitations"></a>기능 제한 사항

### <a name="scale-operations"></a>크기 조정 작업
- 기본 가격 책정 계층 간의 동적 크기 조정은 현재 지원되지 않습니다.
- 서버 스토리지 크기를 줄이는 것은 지원되지 않습니다.

### <a name="server-version-upgrades"></a>서버 버전 업그레이드
- 주 데이터베이스 엔진 버전 간에 자동화된 마이그레이션은 현재 지원되지 않습니다. 다음의 주 버전으로 업그레이드하려는 경우 새 엔진 버전을 사용하여 만든 서버에 주 버전을 [덤프 및 복원](./concepts-migrate-dump-restore.md)합니다.

### <a name="point-in-time-restore"></a>특정 시점 복원
- PITR 기능을 사용하면 새 서버가 기반으로 하는 서버와 동일한 구성으로 새 서버가 만들어집니다.
- 삭제된 서버 복원은 지원되지 않습니다.

### <a name="vnet-service-endpoints"></a>VNet 서비스 엔드포인트
- VNet 서비스 엔드포인트는 범용 및 메모리 최적화 서버에 대해서만 지원됩니다.

### <a name="storage-size"></a>스토리지 크기
- 가격 책정 계층당 스토리지 크기 제한에 대한 [가격 책정 계층](concepts-pricing-tiers.md)을 참조하세요.

## <a name="current-known-issues"></a>현재 알려진 문제
- 연결이 설정된 후에 MySQL 서버 인스턴스에서 잘못된 서버 버전을 표시합니다. 올바른 서버 인스턴스 엔진 버전을 설치하려면 `select version();` 명령을 사용합니다.

## <a name="next-steps"></a>다음 단계
- [각 서비스 계층에서 사용할 수 있는 기능](concepts-pricing-tiers.md)
- [지원되는 MySQL 데이터베이스 버전](concepts-supported-versions.md)
