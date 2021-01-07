---
title: Azure Notification Hubs 및 Google Cloud Messaging을 사용하여 특정 Android 디바이스에 알림 보내기 | Microsoft Docs
description: Notification Hubs를 사용하여 Azure Notification Hubs 및 Google Cloud Messaging을 사용하여 특정 Android 디바이스에 알림을 푸시하는 방법을 알아봅니다.
services: notification-hubs
documentationcenter: android
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc, devx-track-java
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: c0c0018ac3007f77da820b9b0cecbb69c68bef31
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92308304"
---
# <a name="tutorial-send-push-notifications-to-specific-android-devices-using-google-cloud-messaging-deprecated"></a>자습서: Google Cloud Messaging을 사용하여 특정 Android 디바이스에 푸시 알림 보내기(더 이상 사용되지 않음)

> [!WARNING]
> 2018년 4월 10일 기준으로 Google은 GCM(Google Cloud Messaging)을 더 이상 지원하지 않습니다. GCM 서버 및 클라이언트 API는 더 이상 사용되지 않으며 2019년 5월 29일에 제거될 예정입니다. 자세한 내용은 [GCM 및 FCM 질문과 대답](https://developers.google.com/cloud-messaging/faq)을 참조하세요.

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>개요

이 자습서에서는 Azure Notification Hubs를 사용하여 Android 앱에 속보 알림을 브로드캐스트하는 방법을 보여줍니다. 완료하면, 관심이 있는 속보 범주를 등록하고 해당 범주의 푸시 알림만 받을 수 있습니다. 이 시나리오는 RSS 수집기, 음악 애호가를 위한 앱 등 이전에 관심을 보인 사용자 그룹에 알림을 보내야 하는 많은 앱에 공통된 패턴입니다.

브로드캐스트 시나리오를 사용하려면 알림 허브에서 등록을 만들 때 하나 이상의 *태그*를 포함하면 됩니다. 태그에 알림이 전송되면 태그에 대해 등록된 모든 디바이스에서 알림을 받게 됩니다. 태그는 단순히 문자열이므로 사전에 프로비전해야 할 필요가 없습니다. 태그에 대한 자세한 내용은 [Notification Hubs 라우팅 및 태그 식](notification-hubs-tags-segment-push-message.md)을 참조하세요.

이 자습서에서는 다음 작업을 수행합니다.

> [!div class="checklist"]
> * 모바일 앱에 범주 선택 추가
> * 태그를 사용하여 알림 등록
> * 태그가 지정된 알림 보내기
> * 앱 테스트

## <a name="prerequisites"></a>사전 요구 사항

이 자습서는 [자습서: Azure Notification Hubs 및 Google Cloud Messaging을 사용하여 Android 디바이스에 알림 푸시][get-started]에서 만든 알림 허브를 기반으로 합니다. 이 자습서를 시작하기 전에 [자습서: Azure Notification Hubs 및 Google Cloud Messaging을 사용하여 Android 디바이스에 알림 푸시][get-started]에서 만든 알림 허브를 기반으로 합니다.

## <a name="add-category-selection-to-the-app"></a>앱에 범주 선택 추가

첫 번째 단계는 기존의 기본 활동에 사용자가 등록할 범주를 선택할 수 있도록 하는 UI 요소를 추가하는 것입니다. 사용자가 선택한 범주는 디바이스에 저장됩니다. 앱을 시작하면 디바이스 등록이 선택한 범주와 함께 태그로서 알림 허브에 생성됩니다.

1. `res/layout/activity_main.xml file`을 열고 콘텐츠를 다음으로 바꿉니다.

    ```xml
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context="com.example.breakingnews.MainActivity"
        android:orientation="vertical">

            <CheckBox
                android:id="@+id/worldBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_world" />
            <CheckBox
                android:id="@+id/politicsBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_politics" />
            <CheckBox
                android:id="@+id/businessBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_business" />
            <CheckBox
                android:id="@+id/technologyBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_technology" />
            <CheckBox
                android:id="@+id/scienceBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_science" />
            <CheckBox
                android:id="@+id/sportsBox"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/label_sports" />
            <Button
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="subscribe"
                android:text="@string/button_subscribe" />
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Hello World!"
                android:id="@+id/text_hello"
            />
    </LinearLayout>
    ```
2. `res/values/strings.xml` 파일을 열고 다음 줄을 추가합니다.

    ```xml
    <string name="button_subscribe">Subscribe</string>
    <string name="label_world">World</string>
    <string name="label_politics">Politics</string>
    <string name="label_business">Business</string>
    <string name="label_technology">Technology</string>
    <string name="label_science">Science</string>
    <string name="label_sports">Sports</string>
    ```

    `main_activity.xml` 그래픽 레이아웃은 다음 이미지와 같이 표시되어야 합니다.

    ![앱 화면이 보이는 개발 환경의 스크린샷. 앱에는 코드에 추가된 뉴스 범주가 나열됩니다.][A1]
3. `MainActivity` 클래스와 동일한 패키지에서 `Notifications` 클래스를 만듭니다.

    ```java
    import java.util.HashSet;
    import java.util.Set;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.os.AsyncTask;
    import android.util.Log;
    import android.widget.Toast;
    import android.view.View;

    import com.google.android.gms.gcm.GoogleCloudMessaging;
    import com.microsoft.windowsazure.messaging.NotificationHub;

    public class Notifications {
        private static final String PREFS_NAME = "BreakingNewsCategories";
        private GoogleCloudMessaging gcm;
        private NotificationHub hub;
        private Context context;
        private String senderId;

        public Notifications(Context context, String senderId, String hubName,
                                String listenConnectionString) {
            this.context = context;
            this.senderId = senderId;

            gcm = GoogleCloudMessaging.getInstance(context);
            hub = new NotificationHub(hubName, listenConnectionString, context);
        }

        public void storeCategoriesAndSubscribe(Set<String> categories)
        {
            SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
            settings.edit().putStringSet("categories", categories).commit();
            subscribeToCategories(categories);
        }

        public Set<String> retrieveCategories() {
            SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
            return settings.getStringSet("categories", new HashSet<String>());
        }

        public void subscribeToCategories(final Set<String> categories) {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(senderId);

                        String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                        hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM,
                            categories.toArray(new String[categories.size()]));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to register - " + e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    String message = "Subscribed for categories: "
                            + categories.toString();
                    Toast.makeText(context, message,
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

    }
    ```

    이 클래스는 로컬 스토리지를 사용하여, 이 디바이스에서 받아야 할 뉴스의 범주를 저장합니다. 이러한 범주를 등록하기 위한 메서드도 이 클래스에 포함됩니다.
4. `MainActivity` 클래스에서 `NotificationHub` 및 `GoogleCloudMessaging`에 대한 프라이빗 필드를 제거하고 `Notifications`에 대한 필드를 추가합니다.

    ```java
    // private GoogleCloudMessaging gcm;
    // private NotificationHub hub;
    private Notifications notifications;
    ```
5. 그런 다음, `onCreate` 메서드에서 `hub` 필드 및 `registerWithNotificationHubs` 메서드의 초기화를 제거합니다. 그런 다음, `Notifications` 클래스의 인스턴스를 초기화하는 다음 줄을 추가합니다.

    ```java
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mainActivity = this;

        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId,
                MyHandler.class);

        notifications = new Notifications(this, NotificationSettings.SenderId, NotificationSettings.HubName, NotificationSettings.HubListenConnectionString);

        notifications.subscribeToCategories(notifications.retrieveCategories());
    }
    ```

    NotificationSettings 클래스에서 허브 이름 및 연결 문자열이 올바르게 설정되어 있는지 확인합니다.

    > [!NOTE]
    > 클라이언트 앱과 함께 배포되는 자격 증명은 일반적으로 안전하지 않기 때문에 클라이언트 앱과 함께 listen access용 키만 배포해야 합니다. Listen access를 통해 앱에서 알림을 등록할 수 있지만, 기존 등록을 수정할 수 없으며 알림을 전송할 수도 없습니다. 안전한 백 엔드 서비스에서 알림을 보내고 기존 등록을 변경하는 데에는 모든 액세스 키가 사용됩니다.

6. 그런 다음, 다음 가져오기를 추가합니다.

    ```java
    import android.widget.CheckBox;
    import java.util.HashSet;
    import java.util.Set;
    ```
7. 다음 `subscribe` 메서드를 추가하여 구독 단추 클릭 이벤트를 처리합니다.

    ```java
    public void subscribe(View sender) {
        final Set<String> categories = new HashSet<String>();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        if (world.isChecked())
            categories.add("world");
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        if (politics.isChecked())
            categories.add("politics");
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        if (business.isChecked())
            categories.add("business");
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        if (technology.isChecked())
            categories.add("technology");
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        if (science.isChecked())
            categories.add("science");
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        if (sports.isChecked())
            categories.add("sports");

        notifications.storeCategoriesAndSubscribe(categories);
    }
    ```

    이 메서드는 범주 목록을 만들고 `Notifications` 클래스를 사용하여, 로컬 스토리지에 목록을 저장하고 알림 허브에 해당 태그를 등록합니다. 범주가 변경되면 새 범주로 등록이 다시 생성됩니다.

이제 사용자가 범주 선택을 변경할 때마다 앱은 범주 집합을 디바이스의 로컬 스토리지에 저장하고 알림 허브에 등록할 수 있습니다.

## <a name="register-for-notifications"></a>알림 등록

다음 단계에서는 로컬 스토리지에 저장된 범주를 사용하여 시작 시 알림 허브에 등록합니다.

> [!NOTE]
> GCM(Google Cloud Messaging)에서 할당하는 registrationId는 언제든지 변경될 수 있으므로 알림 실패를 피하려면 알림을 자주 등록해야 합니다. 이 예제에서는 앱이 시작될 때마다 알림을 등록합니다. 자주(하루 두 번 이상) 실행되는 앱에서는 이전 등록 이후 만 하루가 지나지 않은 경우 대역폭 유지를 위한 등록을 건너뛸 수 있습니다.

1. `MainActivity` 클래스의 `onCreate` 메서드 끝에 다음 코드를 추가합니다.

    ```java
    notifications.subscribeToCategories(notifications.retrieveCategories());
    ```

    이 코드를 통해 앱이 시작될 때마다 로컬 스토리지에서 범주를 검색하고, 이러한 범주에 대한 등록을 요청하게 됩니다.
2. 그런 후 다음과 같이 `MainActivity` 클래스의 `onStart()` 메서드를 업데이트합니다.

    ```java
    @Override
    protected void onStart() {

        super.onStart();
        isVisible = true;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }
    ```

    이 코드는 이전에 저장한 범주의 상태를 기반으로 기본 활동을 업데이트합니다.

이제 앱이 완료되며, 사용자가 범주 선택을 변경할 때마다 알림 허브 등록에 사용된 디바이스의 로컬 스토리지에 범주 집합을 저장할 수 있습니다. 다음에는 범주 알림을 이 앱에 보낼 수 있는 백 엔드를 정의합니다.

## <a name="send-tagged-notifications"></a>태그가 지정된 알림 보내기

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="test-the-app"></a>앱 테스트

1. Android Studio의 Android 디바이스 또는 에뮬레이터에서 앱을 실행합니다. 앱 UI는 구독할 범주를 선택하도록 하는 토글 집합을 제공합니다.
2. 하나 이상의 범주 토글을 사용하도록 설정한 후 **구독**을 클릭합니다. 앱은 선택한 범주를 태그로 변환하고 알림 허브에서 선택한 태그에 대한 새로운 디바이스 등록을 요청합니다. 등록된 범주가 반환되어 알림 메시지에 표시됩니다.

    ![범주에 대한 구독](./media/notification-hubs-aspnet-backend-android-breaking-news/subscribe-for-categories.png)
3. 각 범주에 대한 알림을 전송하는 .NET 콘솔 앱을 실행합니다. 선택한 범주에 대한 알림이 알림 메시지로 나타납니다.

    ![기술 뉴스 알림](./media/notification-hubs-aspnet-backend-android-breaking-news/technolgy-news-notification.png)

## <a name="next-steps"></a>다음 단계

이 자습서에서는 범주에 등록한 특정 Android 디바이스에 브로드캐스트 알림을 보냈습니다. 특정 사용자에게 알림을 푸시하는 방법을 알아보려면 다음 자습서를 계속 진행합니다.

> [!div class="nextstepaction"]
>[특정 사용자에 알림 푸시](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md)

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: /previous-versions/azure/azure-services/jj927170(v=azure.100)
[Notification Hubs How-To for Windows Store]: /previous-versions/azure/azure-services/jj927170(v=azure.100)
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure portal]: https://portal.azure.com
[wns object]: /previous-versions/azure/reference/jj860484(v=azure.100)