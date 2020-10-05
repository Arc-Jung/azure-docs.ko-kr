---
title: 포함 파일
description: 포함 파일
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: f738daab7ddcf0403f546e7c9ffeaeccb66bc6b7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91355649"
---
## <a name="create-an-azure-signalr-service-instance"></a>Azure SignalR Service 인스턴스 만들기

애플리케이션이 Azure의 SignalR Service 인스턴스에 연결됩니다.

1. Azure Portal의 왼쪽 위에 있는 새로 만들기 단추를 선택합니다. 새 화면의 검색 상자에서 *SignalR Service*를 입력한 후 Enter 키를 누릅니다.

    ![Azure Portal의 SignalR Service 검색을 보여주는 스크린샷](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. 검색 결과에서 **SignalR Service**를 선택한 다음, **만들기**를 선택합니다.

1. 다음 설정을 입력합니다.

    | 설정      | 제안 값  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **리소스 이름** | 전역적으로 고유한 이름 | 새 SignalR Service 인스턴스를 식별하는 이름입니다. 유효한 문자는 `a-z`, `0-9` 및 `-`입니다.  | 
    | **구독** | 사용자의 구독 | 이 새 SignalR Service 인스턴스가 생성되는 구독입니다. | 
    | **[리소스 그룹](../../azure-resource-manager/management/overview.md)** |  myResourceGroup | SignalR Service 인스턴스를 만들 새 리소스 그룹의 이름입니다. | 
    | **위치** | 미국 서부 | 근처 [지역](https://azure.microsoft.com/regions/)을 선택하세요. |
    | **가격 책정 계층** | 무료 | Azure SignalR Service를 체험해 보세요. |
    | **단위 수** |  해당 없음 | 단위 수는 SignalR Service 인스턴스가 허용할 수 있는 연결 수를 지정합니다. 표준 계층에서만 구성할 수 있습니다. |
    | **서비스 모드** |  서버를 사용하지 않음 | Azure Functions 또는 REST API와 함께 사용합니다. |

    ![값이 있는 SignalR 기본 사항 탭을 보여주는 스크린샷](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. **만들기**를 선택하여 SignalR Service 인스턴스 배포를 시작하세요.

1. 인스턴스가 배포되면 포털에서 열고 해당 설정 페이지를 찾습니다. Azure Functions 바인딩이나 REST API를 통해 Azure SignalR Service를 사용하는 경우에만 서비스 모드 설정을 *서버리스*로 변경합니다. 그렇지 않으면 *클래식*이나 *기본값*으로 유지합니다.