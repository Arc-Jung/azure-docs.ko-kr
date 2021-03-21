---
author: IEvangelist
ms.author: dapine
ms.date: 10/02/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: a6f399e19cadf3d6ce9edfaecb3d904e62c498aa
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96006879"
---
컨테이너의 구성 설정은 다음과 같습니다.

|필수|설정|용도|
|--|--|--|
|예|[ApiKey](#apikey-configuration-setting)|청구 정보를 추적 합니다.|
|아니요|[ApplicationInsights](#applicationinsights-setting)|컨테이너에 [Azure 애플리케이션 Insights](/azure/application-insights) 원격 분석 지원을 추가할 수 있습니다.|
|예|[Billing](#billing-configuration-setting)|Azure에서 서비스 리소스의 엔드포인트 URI를 지정합니다.|
|예|[Eula](#eula-setting)| 컨테이너에 대한 라이선스에 동의했음을 나타냅니다.|
|아니요|[Fluentd](#fluentd-settings)|Fluentd 서버에 로그 및 필요에 따라 메트릭 데이터를 씁니다.|
|아니요|HTTP 프록시|아웃 바운드 요청을 만들기 위해 HTTP 프록시를 구성 합니다.|
|아니요|[Logging](#logging-settings)|컨테이너에 대한 ASP.NET Core 로깅 지원을 제공합니다. |
|아니요|[Mounts](#mount-settings)|호스트 컴퓨터의 데이터를 읽고 컨테이너에서 호스트 컴퓨터로 다시 씁니다.|