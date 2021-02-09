---
title: 사용자 지정 DNS
titleSuffix: Azure SQL Managed Instance
description: 이 항목에서는 Azure SQL Managed Instance를 사용 하 여 사용자 지정 DNS에 대 한 구성 옵션을 설명 합니다.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova
ms.date: 07/17/2019
ms.openlocfilehash: a54907dd3f7b3fbc06033624f14b12de14d9afb9
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99831504"
---
# <a name="configure-a-custom-dns-for-azure-sql-managed-instance"></a>Azure SQL Managed Instance에 대한 사용자 지정 DNS 구성
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance는 Azure [VNet (가상 네트워크)](../../virtual-network/virtual-networks-overview.md)내에 배포 되어야 합니다. 프라이빗 호스트 이름이 SQL Managed Instance에서 확인되어야 하는 몇 가지 시나리오(예: db 메일, 클라우드 또는 하이브리드 환경의 다른 SQL Server 인스턴스에 연결된 서버)가 있습니다. 이 경우에 Azure 내에서 사용자 지정 DNS를 구성해야 합니다. 

SQL Managed Instance는 내부 작동에 동일한 DNS를 사용 하기 때문에 공용 도메인 이름을 확인할 수 있도록 사용자 지정 DNS 서버를 구성 합니다.

> [!IMPORTANT]
> 항상 메일 서버에 대 한 FQDN (정규화 된 도메인 이름), SQL Server 인스턴스 및 다른 서비스에 대 한 FQDN (정규화 된 도메인 이름)은 개인 DNS 영역 내에 있는 경우에도 사용 합니다. 예를 들어 메일 서버에 대해를 사용 하는 경우가 `smtp.contoso.com` `smtp` 올바르게 확인 되지 않기 때문입니다. 동일한 가상 네트워크 내의 Vm SQL Server 참조 하는 연결 된 서버 또는 복제를 만들려면 FQDN과 기본 DNS 접미사도 필요 합니다. 예들 들어 `SQLVM.internal.cloudapp.net`입니다. 자세한 내용은 [자체 DNS 서버를 사용 하는 이름 확인](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)을 참조 하세요.

> [!IMPORTANT]
> 가상 네트워크 DNS 서버를 업데이트 해도 SQL Managed Instance에 즉시 영향을 주지는 않습니다. 자세한 내용은 [SQL Managed Instance 가상 클러스터의 가상 네트워크 DNS 서버를 동기화 하는 방법 설정을](synchronize-vnet-dns-servers-setting-on-virtual-cluster.md) 참조 하세요.

## <a name="next-steps"></a>다음 단계

- 개요는 [AZURE SQL Managed Instance?](sql-managed-instance-paas-overview.md)을 참조 하세요.
- 새 관리 되는 인스턴스를 만드는 방법을 보여 주는 자습서는 [관리 되는 인스턴스 만들기](instance-create-quickstart.md)를 참조 하세요.
- 관리 되는 인스턴스에 대해 VNet을 구성 하는 방법에 대 한 자세한 내용은 [관리 되는 인스턴스의 vnet 구성](connectivity-architecture-overview.md)을 참조 하세요.
