---
title: 공용 네트워크 액세스 거부-Azure Portal-Azure Database for MariaDB
description: Azure Database for MariaDB에 대 한 Azure Portal를 사용 하 여 공용 네트워크 액세스 거부를 구성 하는 방법을 알아봅니다.
author: mksuni
ms.author: sumuth
ms.service: jroth
ms.topic: how-to
ms.date: 03/10/2020
ms.openlocfilehash: 7925107f4334df7a844b3f3e029f3769eef51a9c
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98665075"
---
# <a name="deny-public-network-access-in-azure-database-for-mariadb-using-azure-portal"></a>Azure Portal를 사용 하 여 Azure Database for MariaDB에서 공용 네트워크 액세스 거부

이 문서에서는 모든 공용 구성을 거부 하도록 Azure Database for MariaDB 서버를 구성 하 고 개인 끝점을 통해서만 연결을 허용 하 여 네트워크 보안을 강화 하는 방법을 설명 합니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법 가이드를 완료하려면 다음이 필요합니다.

* [Azure Database for MariaDB](quickstart-create-MariaDB-server-database-using-azure-portal.md)

## <a name="set-deny-public-network-access"></a>공용 네트워크 액세스 거부 설정

다음 단계를 수행 하 여 MariaDB 서버에서 공용 네트워크 액세스 거부를 설정 합니다.

1. [Azure Portal](https://portal.azure.com/)에서 기존 Azure Database for MariaDB 서버를 선택 합니다.

1. MariaDB 서버 페이지의 **설정** 에서 **연결 보안** 을 클릭 하 여 연결 보안 구성 페이지를 엽니다.

1. 공용 네트워크 액세스 거부에서 **예** 를 선택 하 여 MariaDB 서버에 대 한 공용 액세스 거부를 사용 하도록 설정 합니다.

    ![네트워크 액세스 거부 Azure Database for MariaDB](./media/howto-deny-public-network-access/deny-public-network-access.PNG)

1. **저장** 을 클릭하여 변경 내용을 저장합니다.

1. 연결 보안 설정이 성공적으로 설정 되었는지 확인 하는 알림이 나타납니다.

    ![Azure Database for MariaDB 네트워크 액세스 거부 성공](./media/howto-deny-public-network-access/deny-public-network-access-success.png)

## <a name="next-steps"></a>다음 단계

[메트릭에 대 한 경고를 만드는 방법](howto-alert-metric.md)에 대해 알아봅니다.