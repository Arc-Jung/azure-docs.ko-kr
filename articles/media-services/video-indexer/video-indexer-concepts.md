---
title: Video Indexer 개념
titleSuffix: Azure Media Services
description: 이 문서에서는 Azure Media Services Video Indexer 서비스의 몇 가지 개념을 설명 합니다.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 73dad1db4f44134f871c9f3d6e7edcdd3bd1e2ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74900684"
---
# <a name="video-indexer-concepts"></a>Video Indexer 개념
 
이 문서에서는 Video Indexer 서비스의 몇 가지 개념에 대해 설명합니다.
    
## <a name="summarized-insights"></a>요약된 인사이트

요약된 인사이트에는 얼굴, 주제, 감정 등의 데이터에 대한 집계된 보기가 포함됩니다. 예를 들어 수천 개의 시간 범위 각각을 검토하고 포함된 얼굴을 확인하는 대신, 요약된 인사이트에는 모든 얼굴과 각 얼굴에 대해 표시된 시간 범위 및 시간의 비율(%)을 나타냅니다.

## <a name="time-range-vs-adjusted-time-range"></a>시간 범위 및 조정된 시간 범위

TimeRange는 원본 비디오의 시간 범위입니다. AdjustedTimeRange는 현재 재생 목록을 기준으로 한 시간 범위입니다. 다른 비디오의 다른 줄에서 재생 목록을 만들 수 있으므로 1시간 분량의 비디오를 가져오고 이 비디오에서 한 줄만 사용할 수 있습니다(예: 10:00-10:15). 이 경우 TimeRange는 10:00-10:15이지만 adjustedTimeRange는 00:00-00:15인 한 줄의 재생 목록을 갖게 됩니다.
 
## <a name="blocks"></a>블록

블록을 사용하면 데이터를 더 쉽게 살펴볼 수 있습니다. 예를 들어 블록은 화자가 변경되거나 오랫동안 일시 중지되는 경우를 기준으로 나눌 수 있습니다.

## <a name="next-steps"></a>다음 단계

시작하는 방법에 대한 자세한 내용은 [가입하고 첫 번째 비디오를 업로드하는 방법](video-indexer-get-started.md)을 참조하세요.

## <a name="see-also"></a>참고 항목

[Video Indexer 개요](video-indexer-overview.md)
