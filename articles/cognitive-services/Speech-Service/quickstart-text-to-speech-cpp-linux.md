---
title: '빠른 시작: 음성 합성, C++(Linux) - Speech Service'
titleSuffix: Azure Cognitive Services
description: Speech SDK를 사용하여 Linux에서 C++로 음성을 합성하는 방법 알아보기
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: 0846af20a2ee993742f648840bcbe49e187f6db9
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803181"
---
# <a name="quickstart-synthesize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>빠른 시작: Speech SDK를 사용하여 Linux 기반 C++에서 음성 합성

빠른 시작은 [음성 인식](quickstart-cpp-linux.md)에도 사용할 수 있습니다.

이 문서에서는 Linux용(Ubuntu 16.04, Ubuntu 18.04, Debian 9) C++ 콘솔 애플리케이션을 만듭니다. Cognitive Services [Speech SDK](speech-sdk.md)를 사용하여 실시간으로 텍스트를 음성으로 합성하여 PC 스피커에서 재생합니다. 애플리케이션은 [Linux용 Speech SDK](https://aka.ms/csspeech/linuxbinary) 및 Linux 배포판의 C++ 컴파일러(예: `g++`)를 사용하여 빌드됩니다.

## <a name="prerequisites"></a>필수 조건

이 빠른 시작을 완료하려면 Speech Services 구독 키가 필요합니다. 무료로 가져올 수 있습니다. 자세한 내용은 [Speech Services를 무료로 체험해보기](get-started.md)를 참조하세요.

## <a name="install-speech-sdk"></a>Speech SDK 설치

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Linux용 Speech SDK는 64비트 및 32비트 애플리케이션을 빌드하는 데 사용할 수 있습니다. 필요한 라이브러리 및 헤더 파일은 https://aka.ms/csspeech/linuxbinary 에서 tar 파일로 다운로드할 수 있습니다.

SDK를 다음과 같이 다운로드하고 설치합니다.

1. SDK의 종속 요소가 설치되었는지 확인합니다.

   * Ubuntu에서:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * Debian 9:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

1. Speech SDK 파일을 추출할 디렉터리를 선택하고, 해당 디렉터리를 가리키도록 `SPEECHSDK_ROOT` 환경 변수를 설정합니다. 이 변수는 이후 명령에서 디렉터리를 쉽게 참조할 수 있게 해줍니다. 예를 들어 홈 디렉터리에서 `speechsdk` 디렉터리를 사용하려는 경우 다음과 비슷한 명령을 사용합니다.

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. 아직 디렉터리가 없으면 디렉터리를 만듭니다.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Speech SDK 이진 파일이 들어 있는 `.tar.gz` 아카이브를 다운로드하여 추출합니다.

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. 추출된 패키지의 최상위 디렉터리의 내용을 확인합니다.

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   디렉터리 목록에는 헤더(`.h`) 파일을 포함하고 있는 `include` 디렉터리와 라이브러리를 포함하고 있는 `lib` 디렉터리뿐 아니라 타사 통지 및 라이선스 파일이 들어 있습니다.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>샘플 코드 추가

1. `helloworld.cpp`라는 C++ 원본 파일을 만들고 다음 코드를 파일에 붙여넣습니다.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-linux/helloworld.cpp#code)]

1. 이 새 파일에서 `YourSubscriptionKey` 문자열을 Speech Services 구독 키로 바꿉니다.

1. 문자열 `YourServiceRegion`을 구독과 연결된 [지역](regions.md)으로 바꿉니다(예를 들어 평가판 구독에 대해 `westus`).

## <a name="build-the-app"></a>앱 빌드

> [!NOTE]
> 아래 명령을 _단일 명령줄_로 입력합니다. 이 작업을 수행하는 가장 쉬운 방법은 각 명령 옆에 있는 **복사** 단추를 누른 다음, 셸 프롬프트에 붙여넣는 것입니다.

* **x64**(64비트) 시스템에서 다음 명령을 실행하여 애플리케이션을 빌드합니다.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* **x86**(32비트) 시스템에서 다음 명령을 실행하여 애플리케이션을 빌드합니다.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

* **ARM64**(64비트) 시스템에서 다음 명령을 실행하여 애플리케이션을 빌드합니다.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/arm64" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>앱 실행

1. Speech SDK 라이브러리를 가리키도록 로더의 라이브러리 경로를 구성합니다.

   * **x64**(64비트) 시스템에서 다음 명령을 입력합니다.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * **x86**(32비트) 시스템에서 다음 명령을 입력합니다.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

   * **ARM64**(64비트) 시스템에서 다음 명령을 입력합니다.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/arm64"
     ```

1. 애플리케이션을 실행합니다.

   ```sh
   ./helloworld
   ```

1. 콘솔 창에서 일부 텍스트를 입력하라는 메시지가 나타납니다. 몇 가지 단어나 문장을 입력합니다. 입력한 텍스트는 Speech Services로 전송되고 스피커에서 재생되는 음성으로 합성됩니다.

   ```text
   Type some text that you want to speak...
   > hello
   Speech synthesized to speaker for text [hello]
   Press enter to exit...
   ```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [GitHub에서 C++ 샘플 살펴보기](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>참고 항목

- [음성 글꼴 사용자 지정](how-to-customize-voice-font.md)
- [음성 샘플 레코드](record-custom-voice-samples.md)
