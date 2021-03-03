---
title: 공용 네트워크 액세스 거부-Azure Portal Azure Database for PostgreSQL 단일 서버
description: Azure Database for PostgreSQL 단일 서버에 대해 Azure Portal를 사용 하 여 공용 네트워크 액세스 거부를 구성 하는 방법을 알아봅니다.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: e195c005676df27385e5e00736b04bdb689fafc5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727110"
---
# <a name="deny-public-network-access-in-azure-database-for-postgresql-single-server-using-azure-portal"></a>Azure Portal를 사용 하 여 Azure Database for PostgreSQL 단일 서버에서 공용 네트워크 액세스 거부

이 문서에서는 모든 공용 구성을 거부 하도록 Azure Database for PostgreSQL 단일 서버를 구성 하 고 개인 끝점을 통해서만 연결을 허용 하 여 네트워크 보안을 강화 하는 방법을 설명 합니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법 가이드를 완료하려면 다음이 필요합니다.

* 범용 또는 메모리 액세스에 최적화 된 가격 책정 계층을 사용 하는 [Azure Database for PostgreSQL 단일 서버](quickstart-create-server-database-portal.md) .

## <a name="set-deny-public-network-access"></a>공용 네트워크 액세스 거부 설정

PostgreSQL 단일 서버 공용 네트워크 액세스 거부를 설정 하려면 다음 단계를 수행 합니다.

1. [Azure Portal](https://portal.azure.com/)에서 기존 Azure Database for PostgreSQL 단일 서버를 선택 합니다.

1. PostgreSQL 단일 서버 페이지의 **설정** 에서 **연결 보안** 을 클릭 하 여 연결 보안 구성 페이지를 엽니다.

1. **공용 네트워크 액세스 거부** 에서 **예** 를 선택 하 여 PostgreSQL 단일 서버에 대 한 공용 액세스 거부를 사용 하도록 설정 합니다.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access.PNG" alt-text="Azure Database for PostgreSQL 단일 서버 네트워크 액세스 거부":::

1. **저장** 을 클릭하여 변경 내용을 저장합니다.

1. 연결 보안 설정이 성공적으로 설정 되었는지 확인 하는 알림이 나타납니다.

    :::image type="content" source="./media/howto-deny-public-network-access/deny-public-network-access-success.png" alt-text="Azure Database for PostgreSQL 단일 서버 네트워크 액세스 거부 성공":::

## <a name="next-steps"></a>다음 단계

[메트릭에 대 한 경고를 만드는 방법](howto-alert-on-metric.md)에 대해 알아봅니다.
