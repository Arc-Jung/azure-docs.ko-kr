---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 31634abe2768ec47ee2aa66051a7a363f83c6009
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105043224"
---
:::row:::
    :::column span="3":::
        IOS 용으로 개발할 때 사용할 수 있는 두 가지 음성 Sdk가 있습니다. 목표-C Speech SDK는 기본적으로 iOS CocoaPod 패키지로 사용할 수 있습니다. 또는 .NET Speech SDK는 .NET Standard 2.0를 구현 하므로 Xamarin.ios와 함께 사용할 수 있습니다.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="iOS" src="https://docs.microsoft.com/media/logos/logo_ios.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Swift와 함께 목표-C Speech SDK를 사용 하는 방법에 대 한 자세한 내용은 <a href="https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift" target="_blank">Swift로 목표-c 가져오기 </a>를 참조 하세요.

### <a name="system-requirements"></a>시스템 요구 사항

- MacOS 버전 10.3 이상
- IOS 9.3 이상 대상

# <a name="xcode"></a>[Xcode](#tab/ios-xcode)

:::row:::
    :::column span="3":::
        IOS CocoaPod 패키지는 <a href="https://apps.apple.com/us/app/xcode/id497799835" target="_blank">Xcode 9.4.1 (이상) </a> IDE (통합 개발 환경)에서 다운로드 하 여 사용할 수 있습니다. 먼저 <a href="https://aka.ms/csspeech/iosbinary" target="_blank">이진 CocoaPod를 다운로드 </a>합니다. 사용 하기 위해 동일한 디렉터리에서 pod를 추출 하 고, *Podfile* 을 만들고,를 `pod` 로 나열 합니다 `target` .
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xcode" src="https://docs.microsoft.com/media/logos/logo_xcode.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

```
platform :ios, '9.3'
use_frameworks!

target 'AppName' do
  pod 'MicrosoftCognitiveServicesSpeech', '~> 1.10.0'
end
```

# <a name="xamarinios"></a>[Xamarin.iOS](#tab/ios-xamarin)

:::row:::
    :::column span="3":::
        Xamarin.iOS는 .NET 개발자를 위한 전체 iOS SDK를 제공합니다. Visual Studio에서 C# 또는 F#을 사용하여 완전한 네이티브 iOS 앱을 빌드합니다. 자세한 내용은 <a href="/xamarin/ios/" target="_blank">xamarin.ios </a>를 참조 하세요.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xamarin" src="https://docs.microsoft.com/media/logos/logo_xamarin.svg" width="60px">
            &nbsp;❤️ &nbsp;        <img alt="iOS" src="https://docs.microsoft.com/media/logos/logo_ios.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

---

#### <a name="additional-resources"></a>추가 자료

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/ios" target="_blank">iOS Speech SDK 퀵 스타트 목표-C 소스 코드 </a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/swift/ios" target="_blank">iOS Speech SDK 퀵 스타트 Swift 원본 코드 </a>