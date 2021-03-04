---
title: 지원 되는 버전-Azure Database for PostgreSQL 유연한 서버
description: Azure Database for PostgreSQL 유연한 서버에서 지원 되는 PostgreSQL 주 버전 및 부 버전을 설명 합니다.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 474545b7c5eca64b558e5fe26e23e6800b0cab96
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037169"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Azure Database for PostgreSQL 신축 서버에서 지원 되는 PostgreSQL 주 버전

> [!IMPORTANT]
> Azure Database for PostgreSQL - 유연한 서버는 미리 보기로 제공됨

Azure Database for PostgreSQL-유연한 서버는 현재 다음 주 버전을 지원 합니다.

## <a name="postgresql-version-12"></a>PostgreSQL 버전 12

현재 부 릴리스는 12.5입니다. 이 부 릴리스의 개선 사항 및 수정 내용에 대 한 자세한 내용은 [PostgreSQL 설명서](https://www.postgresql.org/docs/12/static/release-12-4.html) 를 참조 하세요.

## <a name="postgresql-version-11"></a>PostgreSQL 버전 11

현재 부 릴리스는 11.10입니다. 이 부 릴리스의 개선 사항 및 수정 내용에 대 한 자세한 내용은 [PostgreSQL 설명서](https://www.postgresql.org/docs/11/static/release-11-9.html) 를 참조 하세요.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL 버전 10 이상

Azure Database for PostgreSQL 유연한 서버에는 PostgreSQL 버전 10 및 이전 버전을 지원 하지 않습니다. 이전 버전이 필요한 경우에는 [단일 서버](../concepts-supported-versions.md) 배포 옵션을 사용 하세요.

## <a name="managing-upgrades"></a>업그레이드 관리

PostgreSQL 프로젝트는 보고 된 버그를 수정 하기 위해 부분적 릴리스를 정기적으로 실행 합니다. Azure Database for PostgreSQL은 서비스의 월별 배포 중에 부 릴리스로 서버를 자동으로 패치합니다.

주 버전 업그레이드에 대 한 자동화는 아직 지원 되지 않습니다. 예를 들어 현재 PostgreSQL 11에서 PostgreSQL 12로 자동 업그레이드 되지 않습니다.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->