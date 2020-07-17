---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: travisw
ms.openlocfilehash: d59381c220d7118724db7685740af76ba3776034
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84609312"
---
1. Android Studio를 시작하고 **시작** 창에서 **새 Android Studio 프로젝트 시작**을 선택합니다.

    ![Android Studio 시작 창 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **프로젝트 선택** 마법사가 표시됩니다. 작업 선택 상자에서 **전화 및 태블릿** 및 **빈 작업**을 선택합니다. **다음**을 선택합니다.

   ![프로젝트 선택 마법사의 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. **프로젝트 구성** 화면에서 **이름**으로 *Quickstart*를 입력하고 **패키지 이름**으로 *samples.speech.cognitiveservices.microsoft.com*을 입력합니다. 그런 다음, 프로젝트 디렉터리를 선택합니다. **최소 API 수준**으로 **API 23: Android 6.0(Marshmallow)** 를 선택합니다. 다른 모든 확인란의 선택을 취소하고 **마침**을 선택합니다.

   ![프로젝트 구성 마법사의 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio가 새 Android 프로젝트를 준비하는 데 잠시 시간이 걸립니다. 다음으로, Azure Cognitive Services Speech SDK에 대해 알고 Java 8을 사용하는 프로젝트를 구성합니다.

[!INCLUDE [License notice](cognitive-services-speech-service-license-notice.md)]

Cognitive Services Speech SDK의 현재 버전은 1.12.1입니다.

Android용 Speech SDK는 필요한 라이브러리와 필요한 Android 권한을 포함하는 [AAR(Android 라이브러리)](https://developer.android.com/studio/projects/android-library)로 패키지됩니다.
https:\//csspeechstorage.blob.core.windows.net/maven/의 Maven 리포지토리에서 호스팅됩니다.

Speech SDK를 사용하도록 프로젝트를 설정합니다. Android Studio 메뉴 모음에서 **파일** > **프로젝트 구조**를 선택하여 **프로젝트 구조** 창을 엽니다. **프로젝트 구조** 창에서 다음과 같이 변경합니다.

1. 창의 왼쪽 목록에서 **프로젝트**를 선택합니다. 쉼표를 추가하고 작은따옴표로 묶은 Maven 리포지토리 URL(‘https:\//csspeechstorage.blob.core.windows.net/maven/’)을 추가하여 **기본 라이브러리 리포지토리** 설정을 편집합니다.

   ![프로젝트 구조 창 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. 같은 화면의 왼쪽에서 **앱**을 선택합니다. 그런 다음, 창 맨 위의 **종속성** 탭을 선택합니다. 녹색 더하기 기호( **+** )를 선택하고 드롭다운 메뉴에서 **라이브러리 종속성**을 선택합니다.

   ![라이브러리 종속성의 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. 표시되는 창에서 Android용 Speech SDK의 이름 및 버전(*com.microsoft.cognitiveservices.speech:client-sdk:1.12.1*)을 입력합니다. 그런 다음, **확인**을 선택합니다.
   Speech SDK가 이제 다음과 같이 종속성 목록에 추가됩니다.

   ![종속성 목록에서 Speech SDK의 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **속성** 탭을 선택합니다. **원본 호환성** 및 **대상 호환성** 둘 다에 대해 **1.9**를 선택합니다.

   ![원본 호환성 및 대상 호환성의 스크린샷](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. **확인**을 선택하여 **프로젝트 구조** 창을 닫고 프로젝트에 변경 내용을 적용합니다.
