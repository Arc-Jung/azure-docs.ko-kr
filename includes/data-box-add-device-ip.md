---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: e4b366075cb16f62a0e16b5b06da6fb19ffefdb9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67182868"
---
1. Data Box 디바이스에 로그인합니다. 잠겨 있지 않은지 확인합니다.

    ![Data Box 대시보드](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. **세트 네트워크 인터페이스**로 이동합니다. 클라이언트에 연결하는 데 사용된 네트워크 인터페이스의 디바이스 IP 주소를 기록해 둡니다.

    ![Data Box 대시보드](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. **연결 및 복사**로 이동하고 **나머지**를 클릭합니다.

    ![Data Box 대시보드](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. **스토리지 계정 액세스 및 데이터 업로드** 대화 상자에서 **Blob Service 엔드포인트**를 복사합니다.

    ![Data Box 대시보드](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. 관리자 권한으로 **메모장**을 시작하고 `C:\Windows\System32\Drivers\etc`에 있는 **호스트** 파일을 엽니다.
6. 다음 항목을 **호스트** 파일에 추가합니다. `<device IP address> <Blob service endpoint>`
7. 참조를 위해 다음 이미지를 사용합니다. **호스트** 파일을 저장합니다.

    ![Data Box 대시보드](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
