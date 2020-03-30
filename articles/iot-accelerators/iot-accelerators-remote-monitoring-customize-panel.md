---
title: 원격 모니터링 솔루션 UI에 패널 추가 - Azure | Microsoft Docs
description: 이 문서에서는 원격 모니터링 솔루션 가속기 웹 UI의 대시보드에 새 패널을 추가하는 방법을 보여 줍니다.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 3b855c3bed75945f44b55463bdacd049b7930aa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447066"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>원격 모니터링 솔루션 가속기 웹 UI의 대시보드에 사용자 지정 패널 추가

이 문서에서는 원격 모니터링 솔루션 가속기 웹 UI의 대시보드 페이지에 새 패널을 추가하는 방법을 보여 줍니다. 문서에서는 다음을 설명합니다.

- 로컬 개발 환경을 준비하는 방법.
- 웹 UI의 대시보드 페이지에 새 패널을 추가하는 방법.

이 문서의 예제 패널은 기존 대시보드 페이지에 표시됩니다.

## <a name="prerequisites"></a>사전 요구 사항

이 방법 가이드의 단계를 완료하려면 로컬 개발 머신에 다음과 같은 소프트웨어가 설치되어 있어야 합니다.

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>시작하기 전에

계속하기 전에 [원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 페이지 추가](iot-accelerators-remote-monitoring-customize-page.md) 문서의 단계를 완료해야 합니다.

## <a name="add-a-panel"></a>패널 추가

웹 UI에 패널을 추가하려면 패널을 정의하는 소스 파일을 추가한 후 패널을 표시하도록 대시보드를 수정해야 합니다.

### <a name="add-the-new-files-that-define-the-panel"></a>패널을 정의하는 새 파일 추가

시작할 수 있도록 **src/walkthrough/components/pages/dashboard/panels/examplePanel** 폴더에 패널을 정의하는 파일이 포함되어 있습니다.

**examplePanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

**src/walkthrough/components/pages/dashboard/panels/examplePanel** 폴더를 **src/components/pages/dashboard/panels** 폴더에 복사합니다.

**src/walkthrough/components/pages/dashboard/panels/index.js** 파일에 다음 내보내기를 추가합니다.

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>대시보드에 패널 추가

**src/components/pages/dashboard/dashboard.js**를 수정하여 패널을 추가합니다.

패널의 가져오기 목록에 예제 패널을 추가합니다.

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

페이지 콘텐츠의 그리드에 다음 셀 정의를 추가합니다.

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>플라이아웃 테스트

아직 로컬에서 웹 UI가 실행되고 있지 않으면 리포지토리의 로컬 복사본 루트에서 다음 명령을 실행합니다.

```cmd/sh
npm start
```

이전 명령은 에서 [http://localhost:3000/dashboard](http://localhost:3000/dashboard)UI를 로컬로 실행합니다. **대시보드** 페이지로 이동하여 새 패널을 확인합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 원격 모니터링 솔루션 가속기의 웹 UI에서 대시보드를 추가하거나 사용자 지정하는 데 용이하게 사용할 수 있는 리소스에 대해 알아보았습니다.

원격 모니터링 솔루션 가속기에 대한 자세한 개념 정보는 [원격 모니터링 아키텍처](iot-accelerators-remote-monitoring-sample-walkthrough.md)를 참조하세요.
