---
title: 읽기 복제본 관리-Azure Portal Azure Database for PostgreSQL-단일 서버
description: Azure Portal에서 읽기 복제본 Azure Database for PostgreSQL-단일 서버를 관리 하는 방법에 대해 알아봅니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: dd79618b8d9f016c92166edb9ecdb0bfb113947e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76768956"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>Azure Portal에서 Azure Database for PostgreSQL 단일 서버에서 읽기 복제본 만들기 및 관리

이 문서에서는 Azure Portal에서 Azure Database for PostgreSQL의 읽기 복제본을 만들고 관리하는 방법에 대해 알아봅니다. 읽기 복제본에 대한 자세한 내용은 [개요](concepts-read-replicas.md)를 참조하세요.


## <a name="prerequisites"></a>전제 조건
마스터 서버가 될 [Azure Database for PostgreSQL 서버](quickstart-create-server-database-portal.md)

## <a name="prepare-the-master-server"></a>마스터 서버 준비
이러한 단계는 범용 또는 메모리 최적화 계층에서 마스터 서버를 준비하는 데 사용합니다. 마스터 서버는 azure. replication_support 매개 변수를 설정 하 여 복제용으로 준비 됩니다. 복제 매개 변수가 변경 되 면 변경 내용을 적용 하려면 서버를 다시 시작 해야 합니다. Azure Portal에서이 두 단계는 단일 단추로 캡슐화 되며 **복제 지원을 사용 하도록 설정**합니다.

1. Azure Portal에서 마스터로 사용할 기존 Azure Database for PostgreSQL 서버를 선택합니다.

2. 서버 사이드바의 **설정**에서 **복제**를 선택 합니다.

> [!NOTE] 
> **복제 지원 사용 안 함** 이 회색으로 표시 되는 경우 기본적으로 복제 설정이 서버에 이미 설정 되어 있습니다. 다음 단계를 건너뛰고 읽기 복제본 만들기로 이동할 수 있습니다. 

3. **복제 지원 사용**을 선택 합니다. 

   ![복제 지원 사용](./media/howto-read-replicas-portal/enable-replication-support.png)

4. 복제 지원을 사용할 수 있는지 확인 합니다. 이 작업을 수행 하면 마스터 서버가 다시 시작 됩니다. 

   ![복제 지원 사용 확인](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. 작업이 완료 되 면 두 개의 Azure Portal 알림을 받게 됩니다. 서버 매개 변수를 업데이트 하는 한 가지 알림이 있습니다. 서버 다시 시작에 대 한 또 다른 알림은 즉시 따릅니다.

   ![성공 알림-사용](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. Azure Portal 페이지를 새로 고쳐 복제 도구 모음을 업데이트 합니다. 이제이 서버에 대 한 읽기 복제본을 만들 수 있습니다.

   ![업데이트 된 도구 모음](./media/howto-read-replicas-portal/updated-toolbar.png)
   
복제 지원 설정은 마스터 서버당 일회성 작업입니다. 사용자 편의를 위해 **복제 지원 사용 안 함** 단추가 제공 됩니다. 복제 지원을 사용 하지 않는 것이 좋습니다. 단,이 경우에는이 마스터 서버에서 복제본을 만들지 않는 것이 좋습니다. 마스터 서버에 기존 복제본이 있는 경우에는 복제 지원을 사용 하지 않도록 설정할 수 없습니다.


## <a name="create-a-read-replica"></a>읽기 복제본 만들기
읽기 복제본을 만들려면 다음 단계를 수행합니다.

1. 마스터로 사용할 기존 Azure Database for PostgreSQL 서버를 선택합니다. 

2. 서버 사이드바의 **설정**에서 **복제**를 선택 합니다.

3. **복제본 추가**를 선택합니다.

   ![복제본 추가](./media/howto-read-replicas-portal/add-replica.png)

4. 읽기 복제본의 이름을 입력합니다. 

    ![복제본 이름 지정](./media/howto-read-replicas-portal/name-replica.png)

5. 복제본의 위치를 선택 합니다. 기본 위치는 마스터 서버의와 동일 합니다.

    ![위치 선택](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > 에서 복제본을 만들 수 있는 지역에 대 한 자세한 내용은 [복제본 개념 읽기 문서](concepts-read-replicas.md)를 참조 하세요. 

6. **확인**을 선택하여 복제본 만들기를 확인합니다.

복제본은 마스터와 동일한 계산 및 저장소 설정을 사용 하 여 생성 됩니다. 복제본을 만든 후에는 마스터 서버와는 별도로 컴퓨팅 생성, vCores, 스토리지 및 백업 보존 기간 등의 일부 설정을 변경할 수 있습니다. 가격 책정도 기본 계층에서 다른 계층으로 또는 다른 계층에서 기본 계층으로 변경하는 경우를 제외하고 독립적으로 변경할 수 있습니다.

> [!IMPORTANT]
> 마스터 서버 설정을 새 값으로 업데이트 하기 전에 복제본 설정을 같거나 큰 값으로 업데이트 합니다. 이 작업을 수행 하면 복제본이 마스터의 모든 변경 내용을 유지 하는 데 도움이 됩니다.

읽기 복제본을 만든 후에는 **복제** 창에서 확인할 수 있습니다.

![복제 창에서 새 복제본 보기](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>복제 중지
마스터 서버와 읽기 복제본 간의 복제를 중지할 수 있습니다.

> [!IMPORTANT]
> 마스터 서버와 읽기 복제본에 대한 복제를 중지한 경우 실행 취소할 수 없습니다. 읽기 복제본은 읽기 및 쓰기를 둘 다 지원하는 독립 실행형 서버가 됩니다. 독립 실행형 서버를 다시 복제본으로 만들 수 없습니다.

Azure Portal에서 마스터 서버와 읽기 복제본 간의 복제를 중지하려면 다음 단계를 수행합니다.

1. Azure Portal에서 마스터 Azure Database for PostgreSQL 서버를 선택합니다.

2. 서버 메뉴의 **설정** 아래에서 **복제**를 선택합니다.

3. 복제를 중지할 복제본 서버를 선택합니다.

   ![복제본 선택](./media/howto-read-replicas-portal/select-replica.png)
 
4. **복제 중지**를 선택 합니다.

   ![복제 중지 선택](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. **확인**을 선택하여 복제를 중지합니다.

   ![복제 중지 확인](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>마스터 서버 삭제
마스터 서버를 삭제하려면 독립 실행형 Azure Database for PostgreSQL 서버를 삭제할 때와 동일한 단계를 사용합니다. 

> [!IMPORTANT]
> 마스터 서버를 삭제하면 모든 읽기 복제본에 대한 복제가 중지됩니다. 그러면 읽기 복제본은 읽기와 쓰기를 모두 지원하는 독립 실행형 서버가 됩니다.

Azure Portal에서 서버를 삭제하려면 다음 단계를 수행합니다.

1. Azure Portal에서 마스터 Azure Database for PostgreSQL 서버를 선택합니다.

2. 서버의 **개요** 페이지를 엽니다. **삭제**를 선택합니다.

   ![서버 개요 페이지에서 마스터 서버를 선택하여 삭제](./media/howto-read-replicas-portal/delete-server.png)
 
3. 삭제할 마스터 서버의 이름을 입력합니다. **삭제**를 선택하여 마스터 서버의 삭제를 확인합니다.

   ![마스터 서버 삭제 확인](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>복제본 삭제
마스터 서버를 삭제하는 방법과 비슷한 방법으로 읽기 복제본을 삭제할 수 있습니다.

- Azure Portal에서 읽기 복제본에 대한 **개요** 페이지를 엽니다. **삭제**를 선택합니다.

   ![복제본 개요 페이지에서 복제본을 선택하여 삭제](./media/howto-read-replicas-portal/delete-replica.png)
 
다음 단계에 따라 **복제** 창에서 읽기 복제본을 삭제할 수도 있습니다.

1. Azure Portal에서 마스터 Azure Database for PostgreSQL 서버를 선택합니다.

2. 서버 메뉴의 **설정** 아래에서 **복제**를 선택합니다.

3. 삭제할 읽기 복제본을 선택합니다.

   ![삭제할 복제본 선택](./media/howto-read-replicas-portal/select-replica.png)
 
4. **복제본 삭제**를 선택합니다.

   ![복제본 삭제 선택](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. 삭제할 복제본의 이름을 입력합니다. **삭제**를 선택하여 복제본 삭제를 확인합니다.

   ![복제본 삭제 확인](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>복제본 모니터링
두 메트릭을 사용하여 읽기 복제본을 모니터링할 수 있습니다.

### <a name="max-lag-across-replicas-metric"></a>복제본 간 최대 지연 시간 메트릭
**복제본 간 최대 지연 시간** 메트릭은 마스터 서버와 가장 오래 지연된 복제본 간의 지연 시간을 바이트로 보여줍니다. 

1.  Azure Portal에서 마스터 Azure Database for PostgreSQL 서버를 선택합니다.

2.  **메트릭**을 선택 합니다. **메트릭** 창에서 **복제본 간 최대 지연 시간**을 선택합니다.

    ![복제본 간 최대 지연 시간 모니터링](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  **집계**에 대해 **최대**를 선택합니다.


### <a name="replica-lag-metric"></a>복제본 지연 시간 메트릭
**복제본 지연 시간** 메트릭은 복제본에서 마지막으로 재생된 트랜잭션 이후의 시간을 보여 줍니다. 마스터에서 트랜잭션이 발생하지 않으면 이 메트릭은 이 지연 시간을 반영합니다.

1. Azure Portal에서 Azure Database for PostgreSQL 읽기 복제본을 선택합니다.

2. **메트릭**을 선택 합니다. **메트릭** 창에서 **복제본 지연 시간**을 선택합니다.

   ![복제본 지연 시간 모니터링](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. **집계**에 대해 **최대**를 선택합니다. 
 
## <a name="next-steps"></a>다음 단계
* [Azure Database for PostgreSQL의 읽기 복제본](concepts-read-replicas.md)에 대해 자세히 알아봅니다.
* [Azure CLI 및 REST API에서 읽기 복제본을 만들고 관리](howto-read-replicas-cli.md)하는 방법에 대해 알아봅니다.