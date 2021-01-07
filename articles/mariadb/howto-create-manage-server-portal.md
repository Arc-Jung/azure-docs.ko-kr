---
title: 서버 관리-Azure Portal-Azure Database for MariaDB
description: Azure Portal에서 Azure Database for MariaDB 서버를 관리 하는 방법을 알아봅니다.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: ade70c884a3ef01db45273e4789f34b629473661
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96005030"
---
# <a name="manage-an-azure-database-for-mariadb-server-using-the-azure-portal"></a>Azure Portal를 사용 하 여 Azure Database for MariaDB 서버 관리
이 문서에서는 Azure Database for MariaDB 서버를 관리 하는 방법을 보여 줍니다. 관리 작업에는 계산 및 저장소 크기 조정, 관리자 암호 재설정 및 서버 세부 정보 보기가 포함 됩니다.

## <a name="sign-in"></a>로그인
[Azure Portal](https://portal.azure.com)에 로그인합니다.

## <a name="create-a-server"></a>서버 만들기
Azure Database for MariaDB 서버를 만들고 시작 하는 방법을 알아보려면 [빠른](quickstart-create-mariadb-server-database-using-azure-portal.md) 시작을 참조 하세요.

## <a name="scale-compute-and-storage"></a>계산 및 저장소 크기 조정

서버를 만든 후에는 요구 사항이 변경 됨에 따라 범용 및 메모리 액세스에 최적화 된 계층 사이를 확장할 수 있습니다. VCores를 늘리거나 줄여서 계산 및 메모리를 확장할 수도 있습니다. 저장소를 확장할 수 있습니다 (그러나 저장소 크기를 줄일 수 없음).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>범용 및 메모리 액세스에 최적화 된 계층 간의 크기 조정

범용에서 메모리 액세스에 최적화 된 메모리 또는 그 반대로 확장할 수 있습니다. 서버를 만든 후 기본 계층에서로의 변경은 지원 되지 않습니다. 

1. Azure Portal에서 서버를 선택 합니다. **설정** 섹션에 있는 **가격 책정 계층** 을 선택 합니다.

2. 크기를 조정 하는 대상에 따라 **범용** 또는 **메모리 최적화** 를 선택 합니다. 

    ![가격 책정 계층을 선택 하 고 메모리 최적화 된 값을 선택 하 여 Azure Portal을 보여 주는 스크린샷](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > 계층을 변경 하면 서버가 다시 시작 됩니다.

4. **확인** 을 선택 하 여 변경 내용을 저장 합니다.


### <a name="scale-vcores-up-or-down"></a>VCores 확장 또는 축소

1. Azure Portal에서 서버를 선택 합니다. **설정** 섹션에 있는 **가격 책정 계층** 을 선택 합니다.

2. 슬라이더를 원하는 값으로 이동하여 **vCore** 설정을 변경합니다.

    ![scale-compute](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > VCores 크기를 조정 하면 서버를 다시 시작 합니다.

3. **확인** 을 선택 하 여 변경 내용을 저장 합니다.


### <a name="scale-storage-up"></a>저장소 크기 조정

1. Azure Portal에서 서버를 선택 합니다. **설정** 섹션에 있는 **가격 책정 계층** 을 선택 합니다.

2. 슬라이더를 원하는 값으로 이동 하 여 **저장소** 설정을 변경 합니다.

    ![scale-storage](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > 저장소 크기를 축소할 수 없습니다.

3. **확인** 을 선택 하 여 변경 내용을 저장 합니다.


## <a name="update-admin-password"></a>관리자 암호 업데이트
Azure Portal를 사용 하 여 관리자 역할의 암호를 변경할 수 있습니다.

1. Azure Portal에서 서버를 선택 합니다. **개요** 창에서 **암호 재설정** 을 선택 합니다.

   ![개요](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. 새 암호를 입력하고 암호를 확인합니다. 텍스트 상자에 암호 복잡성 요구 사항에 대 한 메시지가 표시 됩니다.

   ![스크린샷 암호 및 암호 확인 대화 상자를 표시 합니다.](./media/howto-create-manage-server-portal/reset-password.png)

3. **확인** 을 선택 하 여 새 암호를 저장 합니다.


## <a name="delete-a-server"></a>서버 삭제

서버를 더 이상 필요 하지 않은 경우 삭제할 수 있습니다. 

1. Azure Portal에서 서버를 선택 합니다. **개요** 창에서 **삭제** 를 선택 합니다.

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. 입력 상자에 서버 이름을 입력 하 여 삭제 하려는 서버 인지 확인 합니다.

    ![스크린샷에는 데이터베이스를 삭제할지 여부를 확인 하는 대화 상자가 표시 됩니다 .이 대화 상자는 취소할 수 없습니다.](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > 서버 삭제는 취소할 수 없습니다.

3. **삭제** 를 선택합니다.


## <a name="next-steps"></a>다음 단계
- [백업 및 서버 복원](howto-restore-server-portal.md) 에 대 한 자세한 정보
- [Azure Database for MariaDB의 튜닝 및 모니터링 옵션](concepts-monitoring.md) 에 대해 알아봅니다.
