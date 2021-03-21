---
title: 음성 번역 개요-음성 서비스
titleSuffix: Azure Cognitive Services
description: 음성 번역을 사용 하면 응용 프로그램, 도구 및 장치에 음성의 종단 간 실시간 다중 언어 번역을 추가할 수 있습니다. 같은 API를 음성 대 음성 및 음성 대 텍스트 번역 모두에 사용될 수 있습니다. 이 문서는 음성 번역 서비스의 이점과 기능을 개략적으로 설명 합니다.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: erhopf
ms.custom: devx-track-csharp, cog-serv-seo-aug-2020
keywords: 음성 번역
ms.openlocfilehash: 36c2943d7cd5ce2c5c7c4a8501dc0033aa8c9cbc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95015208"
---
# <a name="what-is-speech-translation"></a>음성 번역이란?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

이 개요에서는 음성 변환 서비스의 이점과 기능에 대해 알아보고, 오디오 스트림에 대 한 실시간 다중 언어 음성-음성-음성-텍스트 변환을 가능 하 게 합니다. Speech SDK를 사용하면 애플리케이션, 도구 및 디바이스를 통해 제공된 오디오의 원본 대화 내용 기록과 번역 결과에 액세스할 수 있습니다. 음성이 감지 되 면 임시 기록 및 번역 결과가 반환 되 고 최종 결과는 합성 된 음성으로 변환 될 수 있습니다.

Microsoft의 번역 엔진은 SMT(통계적 기계 번역) 및 NMT(신경망 기계 번역)의 두 가지 방식으로 구동됩니다. SMT는 고급 통계 분석을 사용하여 몇 가지 단어로 구성된 주어진 컨텍스트에서 가능한 최상의 번역을 추정합니다. NMT는 전체 문장 컨텍스트를 사용하여 단어를 번역함으로써 보다 정확하고 자연스러운 번역을 제공하는 신경망을 사용합니다.

현재 Microsoft는 가장 인기 있는 언어로 번역 하는 데 NMT를 사용 합니다. [음성 대 음성 번역에 사용할 수 있는 모든 언어](language-support.md#speech-translation)는 NMT를 통해 지원됩니다. 음성 대 텍스트 번역은 언어 쌍에 따라 SMT 또는 NMT를 사용할 수 있습니다. NMT에서 대상 언어를 지 원하는 경우 전체 번역이 NMT로 구동 됩니다. NMT에서 대상 언어를 지원 하지 않는 경우 변환은 두 언어 간의 "피벗"로 영어를 사용 하는 NMT 및 SMT의 하이브리드입니다.

## <a name="core-features"></a>핵심 기능

* 인식 결과를 사용 하 여 음성 텍스트 변환
* 음성-음성 변환.
* 여러 대상 언어로의 변환을 지원 합니다.
* 중간 인식 및 번역 결과.

## <a name="get-started"></a>시작 

음성 번역을 시작 하려면 [빠른](get-started-speech-translation.md) 시작을 참조 하세요. 음성 번역 서비스는 [음성 SDK](speech-sdk.md) 및 [음성 CLI](spx-overview.md)를 통해 사용할 수 있습니다.

## <a name="sample-code"></a>샘플 코드

이 샘플 코드는 GitHub에서 사용할 수 있습니다. 이러한 샘플은 파일이 나 스트림에서 오디오 읽기, 연속 및 단일 샷 인식/번역, 사용자 지정 모델 사용과 같은 일반적인 시나리오를 다룹니다.

* [음성 텍스트 및 번역 샘플 (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>마이그레이션 가이드

응용 프로그램, 도구 또는 제품이 [Translator Speech API](./how-to-migrate-from-translator-speech-api.md)를 사용 하는 경우 음성 서비스로 마이그레이션하는 데 도움이 되는 가이드를 만들었습니다.

* [Translator Speech API에서 Speech service로 마이그레이션](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>참조 문서

* [Speech SDK](./speech-sdk.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [REST API: 음성 텍스트 변환](rest-speech-to-text.md)
* [REST API: 텍스트 음성 변환](rest-text-to-speech.md)
* [REST API: 일괄 처리 기록 및 사용자 지정](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)

## <a name="next-steps"></a>다음 단계

* 음성 번역 [빠른](get-started-speech-translation.md) 시작 완료
* [평가판 Speech Service 구독 키 받기](overview.md#try-the-speech-service-for-free)
* [Speech SDK 가져오기](speech-sdk.md)