---
author: msftradford
ms.service: spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 80685dee7907b81832c94044d1feb8fcf2e41bde
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185374"
---
### <a name="open-the-publish-wizard"></a>게시 마법사를 엽니다.

**솔루션 탐색기** 에서 **SharingService** 프로젝트를 마우스 오른쪽 단추로 클릭한 다음, **게시** 를 선택합니다.

게시 마법사가 시작됩니다. 

**App Service** > **게시** 를 선택하여 **App Service 만들기** 창을 엽니다.

### <a name="sign-in-to-azure"></a>Azure에 로그인

Azure Portal에 로그인합니다.

**App Service 만들기** 창에서 **계정 추가** 를 선택한 다음, Azure 구독에 로그인합니다. 이미 로그인한 경우 드롭다운 목록에서 원하는 계정을 선택합니다.

   > [!NOTE]
   > 이미 로그인한 경우 아직 **만들기** 를 선택하지 마십시오.
   >

### <a name="create-a-resource-group"></a>리소스 그룹 만들기

[!INCLUDE [resource group intro text](resource-group.md)]

**리소스 그룹** 옆에 있는 **새로 만들기** 를 선택합니다.

리소스 그룹의 이름을 **myResourceGroup** 으로 지정한 다음, **확인** 을 선택합니다.

### <a name="create-an-app-service-plan"></a>App Service 플랜 만들기

[!INCLUDE [app-service-plan](app-service-plan.md)]

**호스팅 계획** 옆에 있는 **새로 만들기** 를 선택합니다.

**호스팅 계획 구성** 창에서 다음 설정을 사용합니다.

| 설정 | 제안 값 | Description |
|-|-|-|
|App Service 계획| MySharingServicePlan | App Service 계획의 이름 |
| 위치 | 미국 서부 | 웹앱이 호스팅된 데이터 센터 |
| 크기 | 무료 | 호스팅 기능을 결정하는 [가격 책정 계층](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |

**확인** 을 선택합니다.

### <a name="create-and-publish-the-web-app"></a>웹앱 만들기 및 게시

**앱 이름** 에 고유한 앱 이름을 입력합니다. 유효한 문자는 a~z, 0~9 및 대시(-)이거나 자동으로 생성된 고유한 이름을 적용합니다. 웹앱의 URL은 `https://<app_name>.azurewebsites.net`이며, 여기서 `<app_name>`은 앱 이름입니다.

**만들기** 를 선택하여 Azure 리소스를 만듭니다.

   마법사를 마치면 마법사가 ASP.NET Core 웹앱을 Azure에 게시한 다음, 기본 브라우저에서 앱을 엽니다.

  ![Azure에서 게시된 ASP.NET 웹앱의 스크린샷.](./media/spatial-anchors-azure/web-app-running-live.png)

이 섹션에서 사용한 앱 이름은 `https://<app_name>.azurewebsites.net` 형식의 URL 접두사로 사용됩니다. 나중에 사용할 수 있도록 이 URL을 텍스트 편집기에 복사합니다.
