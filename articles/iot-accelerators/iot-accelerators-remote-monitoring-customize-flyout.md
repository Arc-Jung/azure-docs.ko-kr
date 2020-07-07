---
title: 원격 모니터링 솔루션 UI에 플라이아웃 추가 - Azure | Microsoft Docs
description: 이 문서에서는 원격 모니터링 솔루션 가속기 웹 UI의 페이지에 새 플라이아웃을 추가하는 방법을 보여줍니다.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 3820b34a9c79f3b514327fa45a05eebaf8a2a1ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82193176"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 플라이아웃 추가

이 문서에서는 원격 모니터링 솔루션 가속기 웹 UI의 페이지에 새 플라이아웃을 추가하는 방법을 보여줍니다. 문서에서는 다음을 설명합니다.

- 로컬 개발 환경을 준비하는 방법.
- 웹 UI의 페이지에 새 플라이아웃을 추가하는 방법

이 문서의 예제 플라이아웃은 [원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 그리드 추가](iot-accelerators-remote-monitoring-customize-grid.md) 방법 문서에서 추가 방법을 보여주는 그리드가 있는 페이지에 표시됩니다.

## <a name="prerequisites"></a>필수 구성 요소

이 방법 가이드의 단계를 완료하려면 로컬 개발 머신에 다음과 같은 소프트웨어가 설치되어 있어야 합니다.

- [Git](https://git-scm.com/downloads)
- [Node.JS](https://nodejs.org/download/)

## <a name="before-you-start"></a>시작하기 전에

계속하려면 다음 문서의 단계를 완료해야 합니다.

- [원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 페이지 추가](iot-accelerators-remote-monitoring-customize-page.md)
- [원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 서비스 추가](iot-accelerators-remote-monitoring-customize-service.md)
- [원격 모니터링 솔루션 가속기 웹 UI에 사용자 지정 그리드 추가](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>플라이아웃 추가

웹 UI에 플라이아웃을 추가하려면 플라이아웃을 정의하는 원본 파일을 추가하고, 웹 UI가 새 구성 요소를 인식하도록 일부 기존 파일을 수정해야 합니다.

### <a name="add-the-new-files-that-define-the-flyout"></a>플라이아웃을 정의하는 새 파일 추가

시작할 수 있도록 **src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout** 폴더에 플라이아웃을 정의하는 파일이 포함되어 있습니다.

**exampleFlyout.container.js**

[!code-javascript[Example flyout container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example flyout container")]

**exampleFlyout.js**

[!code-javascript[Example flyout](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example flyout")]

**src/walkthrough/components/pages/pageWithFlyout/flyouts** 폴더를 **src/components/pages/example** 폴더에 복사합니다.

### <a name="add-the-flyout-to-the-page"></a>페이지에 플라이아웃 추가

플라이아웃을 추가하도록 **src/components/pages/example/basicPage.js**를 수정합니다.

**components/shared**에서 가져온 항목에 **Btn**을 추가하고 **svgs** 및 **ExampleFlyoutContainer**에 대한 가져오기를 추가합니다.

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

**closedFlyoutState**에 대한 **const** 정의를 추가하고 생성자의 상태에 추가합니다.

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

다음 함수를 **BasicPage** 클래스에 추가합니다.

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

다음 **const** 정의를 **render** 함수에 추가합니다.

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

플라이아웃을 여는 단추를 바로 가기 메뉴에 추가합니다.

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

페이지 콘텐츠에 텍스트 및 플라이아웃 컨테이너를 추가합니다.

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>플라이아웃 테스트

아직 로컬에서 웹 UI가 실행되고 있지 않으면 리포지토리의 로컬 복사본 루트에서 다음 명령을 실행합니다.

```cmd/sh
npm start
```

이전 명령은 `http://localhost:3000/dashboard`에서 UI를 로컬로 실행합니다. **예제** 페이지로 이동하여 **플라이아웃** 열기를 클릭합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 원격 모니터링 솔루션 가속기의 웹 UI에서 페이지를 추가하거나 사용자 지정하는 데 용이하게 사용할 수 있는 리소스에 대해 알아보았습니다.

페이지에서 플라이아웃을 정의했으니, 그 다음 단계는 [원격 모니터링 솔루션 가속기 웹 UI의 대시보드에 패널을 추가](iot-accelerators-remote-monitoring-customize-panel.md)하는 것입니다.

원격 모니터링 솔루션 가속기에 대 한 자세한 개념 정보는 [원격 모니터링 아키텍처](iot-accelerators-remote-monitoring-sample-walkthrough.md)를 참조 하세요.
