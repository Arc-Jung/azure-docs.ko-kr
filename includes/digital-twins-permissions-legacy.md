---
title: 포함 파일
description: 포함 파일
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/16/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: ef6b395aeff18a63f52f58e2477679b48a19b002
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624760"
---
1. [Azure Portal](https://portal.azure.com)의 왼쪽 창에서 **Azure Active Directory**를 연 다음, **속성** 창을 엽니다. **디렉터리 ID**를 임시 파일에 복사합니다. 이 값은 다음 섹션에서 샘플 애플리케이션을 구성하는 데 사용됩니다.

    ![Azure Active Directory 디렉터리 ID](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. [Azure Portal](https://portal.azure.com)의 왼쪽 창에서 **Azure Active Directory** 를 열고 **앱 등록 (레거시)** 창을 엽니다. **새 응용 프로그램 등록** 단추를 선택 합니다.

1. **이름** 상자에 이 앱 등록의 이름을 입력합니다. **애플리케이션 유형**으로 **네이티브**를 선택하고, **리디렉션 URI**로 `https://microsoft.com`을 선택합니다. **만들기**를 선택합니다.

    ![창 만들기](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  등록된 앱을 열고, **애플리케이션 ID** 필드의 값을 임시 파일에 복사합니다. 이 값은 Azure Active Directory 앱을 식별합니다. 애플리케이션 ID는 다음 섹션에서 샘플 애플리케이션을 구성하는 데 사용됩니다.

    ![Azure Active Directory 애플리케이션 ID](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. 앱 등록 창을 엽니다. **설정** > **필수 사용 권한**을 선택하고 다음 작업을 수행합니다.

   a. 왼쪽 위에서 **추가**를 선택하여 **API 액세스 추가** 창을 엽니다.

   b. **API 선택**을 선택하고 **Azure Digital Twins**를 검색합니다. 검색에서 API를 찾을 수 없는 경우 **Azure Smart Spaces**을 대신 검색합니다.

   c. **Azure Digital Twins(Azure Smart Spaces 서비스)** 옵션을 선택하고 **선택**을 선택합니다.

   d. **권한 선택**을 선택합니다. **읽기/쓰기 액세스** 위임된 권한 확인란을 선택하고 **선택**을 클릭합니다.

   e. **API 액세스 추가** 창에서 **완료**를 선택합니다.

   f. **필요한 권한** 창에서 **권한 부여** 단추를 선택 하 고 표시 되는 승인을 수락 합니다. 이 API에 대 한 권한이 부여 되지 않은 경우 관리자에 게 문의 하십시오.

      ![필요한 권한 창](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 
