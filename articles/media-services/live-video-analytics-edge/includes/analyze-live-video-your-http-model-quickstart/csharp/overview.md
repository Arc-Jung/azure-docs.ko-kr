---
ms.openlocfilehash: c4c58724f8ba2accd827e1a1b3819e40210aa9c0
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97486747"
---
> [!div class="mx-imgBorder"]
> :::image type="content" source="../../../media/quickstarts/overview-qs5.svg" alt-text="신호 흐름":::

이 다이어그램에서는 이 빠른 시작의 신호 흐름을 보여 줍니다. [에지 모듈](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)은 RTSP(Real-Time Streaming Protocol) 서버를 호스팅하는 IP 카메라를 시뮬레이션합니다. [RTSP 원본](../../../media-graph-concept.md#rtsp-source) 노드는 이 서버에서 비디오 피드를 가져와서 비디오 프레임을 [HTTP 확장 프로세서](../../../media-graph-concept.md#http-extension-processor) 노드로 보냅니다. 

HTTP 확장 노드는 프록시 역할을 수행합니다. `samplingOptions` 필드에서 설정한 수신 비디오 프레임을 샘플링하고 비디오 프레임을 지정된 이미지 형식으로 변환합니다. 그런 다음, REST를 통해 이미지를 HTTP 엔드포인트 내부에서 AI 모델을 실행하는 다른 에지 모듈에 릴레이합니다. 이 예에서 해당 에지 모듈은 다양한 형식의 개체를 감지할 수 있는 [YOLOv3](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) 모델을 사용하여 빌드됩니다. HTTP 확장 프로세서 노드는 감지 결과를 수집하고, 이벤트를 [IoT Hub 싱크](../../../media-graph-concept.md#iot-hub-message-sink) 노드에 게시합니다. 그런 다음, 노드에서 이러한 이벤트를 [IoT Edge Hub](../../../../../iot-edge/iot-edge-glossary.md#iot-edge-hub)에 보냅니다.

이 빠른 시작에서는 다음을 수행합니다.

1. 미디어 그래프를 만들고 배포합니다.
1. 결과를 해석합니다.
1. 리소스를 정리합니다.
