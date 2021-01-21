---
title: IoT Edge 릴리스 정보에 대 한 라이브 비디오 분석-Azure
description: 이 항목에서는 IoT Edge 릴리스, 개선 사항, 버그 수정 및 알려진 문제에 대 한 라이브 비디오 분석의 릴리스 정보를 제공 합니다.
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 328fe97c4e03f039a1224d13ce6712ccff06b3b7
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629779"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>IoT Edge 릴리스 정보에 대 한 라이브 비디오 분석

>이 URL(`https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us`)을 RSS 피드 판독기에 복사하고 붙여넣어 업데이트를 위해 이 페이지를 다시 방문해야 하는 시기에 대한 알림을 받습니다.

이 문서에서는 다음에 대한 정보를 제공합니다.

* 최신 릴리스
* 알려진 문제
* 버그 수정
* 사용되지 않는 기능

<hr width=100%>

## <a name="january-12-2021"></a>2021년 1월 12일

이 릴리스 태그는 모듈의 1 월 2021 새로 고침에 대 한 것입니다.

```
mcr.microsoft.com/media/live-video-analytics:2.0.1
```

> [!NOTE]
> 퀵 스타트 및 자습서에서 배포 매니페스트는 2 (라이브-비디오-분석: 2) 태그를 사용 합니다. 따라서 이러한 매니페스트를 다시 배포 하면 edge > 장치에서 모듈을 업데이트 해야 합니다.
### <a name="bug-fixes"></a>버그 수정 

* `ActivationSignalOffset` `MinimumActivationTime` `MaximumActivationTime` 신호 게이트 프로세서의 필드 및가 필수 속성으로 잘못 설정 되었습니다. 이제 **선택적** 속성입니다.
* 특정 지역에 배포 될 때 IoT Edge 모듈의 라이브 비디오 분석이 중단 되도록 하는 사용 버그를 수정 했습니다.

<hr width=100%>

## <a name="december-14-2020"></a>2020 년 12 월 14 일
이 릴리스는 IoT Edge에 대 한 라이브 비디오 분석의 공개 미리 보기 새로 고침 릴리스입니다. 릴리스 태그는

```
     mcr.microsoft.com/media/live-video-analytics:2.0.0
```
### <a name="module-updates"></a>모듈 업데이트
* 두 개 이상의 HTTP 확장 프로세서를 사용 하 고 그래프 토폴로지에서 gRPC 확장 프로세서를 사용 하기 위한 지원이 추가 되었습니다.
* 싱크 노드에 대 한 디스크 공간 관리에 대 한 지원이 추가 되었습니다.
* `MediaGraphGrpcExtension` 이제 노드는 단일 gRPC 서버 내에서 여러 AI 모델을 사용 하기 위한 [Extensionconfiguration](grpc-extension-protocol.md) 속성을 지원 합니다.
* [프로메테우스 형식의](https://prometheus.io/docs/practices/naming/)라이브 비디오 분석 모듈 메트릭을 수집 하는 지원이 추가 되었습니다. [Azure Monitor에서 메트릭 및 보기를 수집](monitoring-logging.md#azure-monitor-collection-via-telegraf) 하는 방법에 대해 자세히 알아보세요. 
* 출력 선택을 필터링 하는 기능이 추가 되었습니다. 의 도움말을 사용 하 여 모든 그래프 노드에 **오디오 전용** 또는 **비디오 전용** 이나 **오디오 및 비디오** 를 전달할 수 있습니다 `outputSelectors` . 
* 프레임 요금 필터 프로세서가 **사용 되지** 않습니다.  
    * 이제 그래프 확장 프로세서 노드 자체에서 프레임 주기 관리를 사용할 수 있습니다.

### <a name="visual-studio-code-extension"></a>Visual Studio Code 확장
* [IoT Edge에서 라이브 비디오 분석](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.live-video-analytics-edge) 릴리스-lva 미디어 그래프를 관리 하는 데 도움이 되는 Visual Studio Code 확장입니다. 이 확장은 **Lva 2.0 모듈** 에서 작동 하며 매우 슬림 하 고 사용 하기 쉬운 그래픽 인터페이스로 미디어 그래프를 편집 및 관리 하는 기능을 제공 합니다.
## <a name="september-22-2020"></a>2020년 9월 22일

이 릴리스 태그는 모듈의 9 월 2020 새로 고침에 대 한 것입니다.

```
mcr.microsoft.com/media/live-video-analytics:1.0.4
```

> [!NOTE]
> 빠른 시작 및 자습서에서 배포 매니페스트는 1 (라이브-비디오-분석: 1) 태그를 사용 합니다. 따라서 이러한 매니페스트를 다시 배포 하면 edge > 장치에서 모듈을 업데이트 해야 합니다.

### <a name="module-updates"></a>모듈 업데이트

* 새 그래프 확장 노드인 [MediaGraphCognitiveServicesVisionExtension](spatial-analysis-tutorial.md) 는 Cognitive Services에서 [공간 분석](/legal/cognitive-services/computer-vision/intro-to-spatial-analysis-public-preview)(미리 보기) 모듈과 통합할 수 있습니다.
* Linux ARM64 장치에 대 한 지원이 추가 됨-이러한 장치에 배포 하는 데 [수동 단계](deploy-iot-edge-device.md) 를 사용 합니다.

### <a name="documentation-updates"></a>설명서 업데이트

* [지침은](deploy-azure-stack-edge-how-to.md) Azure Stack에 지 장치의 IoT Edge에서 라이브 비디오 분석을 사용 하는 데 사용할 수 있습니다.
* [Custom Vision 서비스](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) 를 사용 하 고 실시간 비디오를 실시간으로 [분석](custom-vision-tutorial.md) 하는 데 사용 하는 시나리오 특정 컴퓨터의 비전 모델 개발에 대 한 새로운 자습서입니다.

<hr width=100%>

## <a name="august-19-2020"></a>2020 년 8 월 19 일

이 릴리스 태그는 모듈의 8 월 2020 새로 고침에 대 한 것입니다.

```
mcr.microsoft.com/media/live-video-analytics:1.0.3
```

> [!NOTE]
> 빠른 시작 및 자습서에서 배포 매니페스트는 1 (라이브-비디오-분석: 1) 태그를 사용 합니다. 따라서 이러한 매니페스트를 다시 배포 하면 edge > 장치에서 모듈을 업데이트 해야 합니다.

### <a name="module-updates"></a>모듈 업데이트

* 이제 gRPC 프레임 워크를 사용 하 여 IoT Edge에서 라이브 비디오 분석과 사용자 지정 확장 간에 높은 데이터 콘텐츠 전송 성능을 얻을 수 있습니다. 시작 하려면 [이](analyze-live-video-use-your-grpc-model-quickstart.md) 항목을 참조 하세요.
* 라이브 비디오 분석의 광범위 한 지역 배포와 클라우드 서비스만 업데이트 되었습니다.  
* Live Video Analytics는 이제 전 세계 25 개의 추가 지역에서 사용할 수 있습니다. 다음은 사용 가능한 모든 지역 [목록](https://azure.microsoft.com/global-infrastructure/services/?products=media-services) 입니다.  
* 빠른 시작에 대 한 [설정](https://aka.ms/lva-edge/setup-resources-for-samples) 도 새 지역 지원과 함께 업데이트 되었습니다.
    * 이미 리소스를 설정 하는 사용자에 대 한 작업 호출은 없습니다.

### <a name="bug-fixes"></a>버그 수정 

* 설정 스크립트에서 사용 되지 않는 Azure 확장의 사용을 제거 합니다.

<hr width=100%>

## <a name="july-13-2020"></a>2020 년 7 월 13 일

이 릴리스 태그는 모듈의 7 월 2020 새로 고침에 대 한 것입니다.

```
mcr.microsoft.com/media/live-video-analytics:1.0.2
```

> [!NOTE]
> 빠른 시작 및 자습서에서 배포 매니페스트는 1 (라이브-비디오-분석: 1) 태그를 사용 합니다. 따라서 이러한 매니페스트를 다시 배포 하면 edge > 장치에서 모듈을 업데이트 해야 합니다.

### <a name="module-updates"></a>모듈 업데이트

* 이제 자산 싱크 노드가 있는 그래프 토폴로지와 신호 게이트 프로세서 노드의 다운스트림 파일 싱크 노드를 만들 수 있습니다. 예제는 [이](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-motion-assets-files) 항목을 참조 하세요.

### <a name="bug-fixes"></a>버그 수정

* Desired 속성의 유효성 검사에 대 한 향상 된 기능

<hr width=100%>

## <a name="june-1-2020"></a>2020년 6월 1일

이 릴리스는 IoT Edge에서 라이브 비디오 분석의 첫 번째 공개 미리 보기 릴리스입니다. 릴리스 태그는

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>지원 되는 기능

* 원하는 AI 모듈을 사용 하 여 라이브 비디오 스트림을 분석 하 고 필요에 따라 edge 장치 또는 클라우드에서 비디오 녹화
* IoT Edge에서 [지 원하는](../../iot-edge/support.md) Linux AMD64 운영 체제에서 사용
* Azure Portal 또는 Visual Studio Code를 사용 하 여 IoT Hub를 통해 모듈을 배포 하 고 구성 합니다.
* 다음 직접 메서드 호출을 통해 [그래프 토폴로지 및 그래프 인스턴스](media-graph-concept.md#media-graph-topologies-and-instances) 를 원격 또는 로컬로 관리

    *   GraphTopologyGet
    *   GraphTopologySet
    *   GraphTopologyDelete
    *   GraphTopologyList
    *   GraphInstanceGet
    *   GraphInstanceSet
    *   GraphInstanceDelete
    *   GraphInstanceList

## <a name="next-steps"></a>다음 단계

[개요](overview.md)
