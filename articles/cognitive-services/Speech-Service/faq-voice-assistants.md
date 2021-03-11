---
title: 음성 도우미 faq (질문과 대답)
titleSuffix: Azure Cognitive Services
description: 사용자 지정 명령 또는 직접 선 음성 채널을 사용 하 여 음성 도우미에 대해 가장 인기 있는 질문에 대 한 답변을 받으세요.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 511eb12df511fd037fc0b5bec701c0cc5c29bad2
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102617780"
---
# <a name="voice-assistants-frequently-asked-questions"></a>음성 도우미 faq (질문과 대답)

이 문서에서 질문에 대 한 답변을 찾을 수 없는 경우 [다른 지원 옵션](../cognitive-services-support-options.md?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext%253fcontext%253d%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext)을 확인 하세요.

## <a name="general"></a>일반

**Q: 음성 지원 이란 무엇 인가요?**

**A:** Cortana와 마찬가지로 음성 도우미는 사용자의 음성 길이 발언을 수신 하 고, 해당 길이 발언의 콘텐츠를 분석 하 고, utterance 의도에 대 한 응답으로 하나 이상의 작업을 수행한 다음, 자주 음성 구성 요소를 포함 하는 사용자에 게 응답을 제공 하는 솔루션입니다. 시스템과의 상호 작용을 위한 "음성 제공, 음성 출력" 환경입니다. 음성 도우미 작성자는 음성 SDK에서를 사용 하 여 장치 응용 프로그램을 만들고, `DialogServiceConnector` [사용자 지정 명령](custom-commands.md) 또는 봇 Framework의 [직접 선 음성](direct-line-speech.md) 채널을 사용 하 여 만든 비서와 통신 합니다. 이러한 도우미는 사용자 지정 키워드, 사용자 지정 음성 및 사용자 지정 음성을 사용 하 여 브랜드 또는 제품에 맞게 조정 된 환경을 제공할 수 있습니다.

**Q: 사용자 지정 명령 또는 직접 줄 음성을 사용 해야 하나요? 차이점은 뭔가요?**

**A:** [사용자 지정 명령은](custom-commands.md) 작업 완료 시나리오에 적합 한 길잡이를 쉽게 만들고 호스트할 수 있는 복잡성이 뛰어난 도구 집합입니다. [직접 라인 음성](direct-line-speech.md) 은 강력한 대화형 시나리오를 사용할 수 있는 보다 풍부 하 고 정교한 기능을 제공 합니다. 자세한 내용은 [길잡이 솔루션의 비교](voice-assistants.md#choosing-an-assistant-solution) 를 참조 하세요.

**Q: 어떻게 시작하나요?**

**A:** 사용자 지정 명령 (미리 보기) 응용 프로그램 또는 기본 봇 프레임 워크 봇을 만들기 시작 하는 가장 좋은 방법입니다.

- [사용자 지정 명령 (미리 보기) 응용 프로그램 만들기](./quickstart-custom-commands-application.md)
- [기본 봇 프레임 워크 봇 만들기](/azure/bot-service/bot-builder-tutorial-basic-deploy)
- [직접 선 음성 채널에 봇 연결](/azure/bot-service/bot-service-channel-connect-directlinespeech)

## <a name="debugging"></a>디버깅

**Q: 내 채널 비밀은 어디에 있나요?**

**A:** 직접 라인 음성의 미리 보기 버전을 사용 했거나 관련 설명서를 읽는 경우 직접 라인 음성 채널 등록 페이지에서 비밀 키를 찾을 수 있습니다. `DialogServiceConfig`SPEECH SDK의 v 1.7 팩터리 메서드에 `FromBotSecret` 도이 값이 필요 합니다.

직접 라인 음성의 최신 버전은 장치에서 봇에 연결 하는 프로세스를 간소화 합니다. 채널 등록 페이지에서 위쪽의 드롭다운은 직접 선 음성 채널 등록과 음성 리소스를 연결 합니다. 연결 된 후에 `BotFrameworkConfig::FromSubscription` 는 구독에 연결 된 봇에 연결 하도록를 구성 하는 팩터리 메서드가 v 1.8 SPEECH SDK에 포함 됩니다 `DialogServiceConnector` .

여전히 클라이언트 응용 프로그램을 v 1.7에서 v 1.8으로 마이그레이션하는 경우에 `DialogServiceConfig::FromBotSecret` 는 사용 했던 이전 암호와 같이 해당 채널 비밀 매개 변수에 대해 null이 아닌 비어 있지 않은 값을 계속 사용할 수 있습니다. 최신 채널 등록과 연결 된 음성 구독을 사용 하는 경우에만 무시 됩니다. 이 값은 null이 아니어야 하 고 비어 있지 _않아야_ 합니다 .이는 서비스 측 연결이 관련 되기 전에 장치에서 확인 되기 때문입니다.

자세한 가이드는 채널 등록을 안내 하는 [자습서 섹션](tutorial-voice-enable-your-bot-speech-sdk.md#register-the-direct-line-speech-channel) 을 참조 하세요.

**Q: 연결할 때 401 오류가 발생 하 고 아무 작업도 수행 되지 않습니다. 음성 구독 키가 유효함을 알고 있습니다. 무슨 일이죠?**

**A:** Azure Portal에서 구독을 관리 하는 경우 **Cognitive Services** 리소스 (CognitiveServicesAllInOne, "모든 Cognitive Services")가 _아닌_ **음성** 리소스 (CognitiveServicesSpeechServices, "speech")를 사용 중인지 확인 하세요. 음성 [도우미에 대 한 음성 서비스 지역 지원](regions.md#voice-assistants)도 확인 하세요.

![직접 라인 음성에 대 한 올바른 구독](media/voice-assistants/faq-supported-subscription.png "호환 되는 음성 구독의 예")

**Q: 내에서 인식 텍스트를 다시 가져오지만 `DialogServiceConnector` ' 1011 ' 오류가 표시 되 고 봇에서 아무것도 표시 되지 않습니다. 굳이?**

**A:** 이 오류는 길잡이와 음성 도우미 서비스 간의 통신 문제가 있음을 나타냅니다.

- 사용자 지정 명령의 경우 사용자 지정 명령 응용 프로그램이 게시 되었는지 확인 합니다.
- 직접 라인 음성의 경우 [직접 선 음성 채널에 봇을 연결](/azure/bot-service/bot-service-channel-connect-directlinespeech)하 고, 봇에 [스트리밍 프로토콜 지원을 추가](/azure/bot-service/directline-speech-bot) 하 고 (관련 웹 소켓 지원), 봇이 채널에서 들어오는 요청에 응답 하 고 있는지 확인 합니다.

**Q:이 코드는 여전히 작동 하지 않으며를 사용할 때 다른 오류가 발생 `DialogServiceConnector` 합니다. 제가 뭘 해야 하나요?**

**A:** 파일 기반 로깅은 상당한 정보를 제공 하며 지원 요청을 가속화 하는 데 도움이 됩니다. 이 기능을 사용 하도록 설정 하려면 [파일 로깅을 사용 하는 방법](how-to-use-logging.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [문제 해결](troubleshooting.md)
- [릴리스 정보](releasenotes.md)