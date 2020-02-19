---
title: Xamarin.Android 앱에 푸시 알림 추가
description: Azure App Service 및 Azure Notification Hubs를 사용 하 여 Xamarin Android 앱에 푸시 알림을 보내는 방법에 대해 알아봅니다.
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 5657be0dbaeb46f8f899a9b4a2f8ba9b4fe9ebaa
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461405"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android 앱에 푸시 알림 추가

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>개요

이 자습서에서는 푸시 알림을 [Xamarin.Android 빠른 시작](app-service-mobile-windows-store-dotnet-get-started.md) 프로젝트에 추가하여 레코드가 삽입될 때마다 디바이스에 푸시 알림이 전송됩니다.

다운로드한 빠른 시작 서버 프로젝트를 사용하지 않는 경우 푸시 알림 확장 패키지가 필요합니다. 자세한 내용은 [Azure Mobile Apps용 .NET 백 엔드 서버 SDK 사용](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) 가이드를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 사용하려면 설정이 필요합니다.

* 활성 Google 계정. [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302)에서 Google 계정을 등록할 수 있습니다.
* [Google Cloud Messaging 클라이언트 구성 요소](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>알림 허브 구성

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Firebase Cloud Messaging 사용

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>푸시 요청을 전송하도록 Azure 구성

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>푸시 알림을 전송하도록 서버 프로젝트 업데이트

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>푸시 알림에 대한 클라이언트 프로젝트 구성

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>앱에 푸시 알림 코드 추가

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>앱에서 푸시 알림 테스트

에뮬레이터에서 가상 디바이스를 사용하여 앱을 테스트할 수 있습니다. 에뮬레이터에서 실행할 때 필요한 추가 구성 단계가 있습니다.

1. 가상 디바이스는 AVD(Android 가상 디바이스) 관리자에서 Google API를 대상으로 설정해야 합니다.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. **앱** > **설정** > **계정 추가**를 클릭하여 Android 디바이스에 Google 계정을 추가한 다음, 지시를 따릅니다.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. 이전처럼 todolist 앱을 실행하고 새 todo 항목을 삽입합니다. 이때 알림 아이콘이 알림 영역에 표시됩니다. 알림 서랍을 열어서 전체 알림 텍스트를 볼 수 있습니다.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
