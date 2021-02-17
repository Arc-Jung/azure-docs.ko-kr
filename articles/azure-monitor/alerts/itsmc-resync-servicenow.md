---
title: ServiceNow 동기화 문제를 수동으로 수정 하는 방법
description: Microsoft Azure에서 경고가 다시 ServiceNow를 호출할 수 있도록 ServiceNow에 대 한 연결을 다시 설정 합니다.
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/17/2021
ms.openlocfilehash: 1b2e914057aa72934524cc4977e3d2292394d0b7
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100617159"
---
# <a name="how-to-manually-fix-sync-problems"></a>동기화 문제를 수동으로 해결 하는 방법

Azure Monitor 타사 ITSM (IT Service Management) 공급자에 연결할 수 있습니다. ServiceNow는 이러한 공급자 중 하나입니다.

보안상의 이유로 ServiceNow를 사용 하 여 연결 하는 데 사용 되는 인증 토큰을 새로 고쳐야 할 수도 있습니다.
다음 동기화 프로세스를 사용 하 여 연결을 다시 활성화 하 고 토큰을 새로 고칩니다.

1. 위쪽 검색 배너에서 솔루션을 검색 한 다음 관련 솔루션을 선택 합니다.

    ![위쪽 검색 배너와 관련 솔루션을 선택할 수 있는 위치를 보여 주는 스크린샷](media/itsmc-resync-servicenow/solution-search-8-bit.png)

1. 솔루션 화면에서 구독 필터의 "모두 선택"을 선택한 다음 "ServiceDesk"로 필터링 합니다.

    ![모두 선택을 선택할 수 있는 위치와 ServiceDesk를 기준으로 필터링 할 위치를 보여 주는 스크린샷](media/itsmc-resync-servicenow/solutions-list-8-bit.png)

1. ITSM 연결의 솔루션을 선택 합니다.
1. 왼쪽 배너에서 ITSM 연결을 선택 합니다.

    ![ITSM 연결을 선택할 수 있는 위치를 보여 주는 스크린샷](media/itsmc-resync-servicenow/itsm-connector-8-bit.png)

1. 목록에서 각 커넥터를 선택 합니다. 
    1. 커넥터 이름을 클릭 하 여 구성 합니다.
    1. 더 이상 사용 하지 않는 모든 커넥터 삭제

    1. 파트너 유형에 따라 [이러한 정의](./itsmc-connections.md) 에 따라 필드를 업데이트 합니다.

    1. 동기화를 클릭 합니다.

       ![동기화 단추를 강조 표시 하는 스크린샷](media/itsmc-resync-servicenow/resync-8-bit-2.png)

    1. 저장을 클릭 합니다.

        ![새 연결](media/itsmc-resync-servicenow/save-8-bit.png)

f.    알림을 검토 하 여 프로세스가 시작 되는지 확인 합니다.
