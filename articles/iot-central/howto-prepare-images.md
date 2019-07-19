---
title: Azure IoT Central 애플리케이션에 이미지 업로드 | Microsoft Docs
description: 작성자로서 이미지를 준비하여 Azure IoT Central 애플리케이션에 업로드하는 방법을 알아봅니다.
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 255033f03acd40580fb4d4a92b0aa9b3e16969f3
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850155"
---
# <a name="prepare-and-upload-images-to-your-azure-iot-central-application"></a>이미지를 준비하여 Azure IoT Central 애플리케이션에 업로드

이 문서에서는 작성자로서 사용자 지정 이미지를 업로드하여 Azure IoT Central 애플리케이션을 사용자 지정하는 방법을 설명합니다. 예를 들어 디바이스 그림을 사용하여 디바이스 대시보드를 사용자 지정할 수 있습니다.

## <a name="before-you-begin"></a>시작하기 전 주의 사항

이 문서의 단계를 완료하려면 다음이 필요합니다.

1. Azure IoT Central 애플리케이션. 자세한 내용은 참조는 [애플리케이션 빠른 시작 만들기](quick-deploy-iot-central.md)를 참조하세요.
1. 이미지 파일의 배율과 크기를 조정하는 데 사용되는 도구.

## <a name="choose-where-to-use-custom-images"></a>사용자 지정 이미지를 사용할 위치 선택

Azure IoT Central 애플리케이션에서 다음 위치에 사용자 지정 이미지를 추가할 수 있습니다.

* **내 응용 프로그램** 페이지

    ![애플리케이션 관리자 페이지의 이미지](media/howto-prepare-images/applicationmanager.png)

* 응용 프로그램 대시보드

    ![응용 프로그램 대시보드의 이미지](media/howto-prepare-images/homepage.png)

* 디바이스 템플릿

    ![디바이스 템플릿의 이미지](media/howto-prepare-images/devicetemplate.png)

* 디바이스 대시보드의 타일

    ![디바이스 타일의 이미지](media/howto-prepare-images/devicetile.png)

* 디바이스 집합 대시보드의 타일

    ![디바이스 집합 타일의 이미지](media/howto-prepare-images/devicesettile.png)

## <a name="prepare-the-images"></a>이미지 준비

네 위치 모두 PNG, GIF 또는 JPEG 이미지를 사용할 수 있습니다.

다음 표에는 사용할 수 있는 이미지 크기가 요약되어 있습니다.

| 위치 | 크기 |
| -------- | ------ |
| 애플리케이션 관리자 | 268x160픽셀 |
| 디바이스 템플릿 | 64x64픽셀 |
| 대시보드 타일 | 최소 타일 크기는 200x200픽셀이고, 작은 타일 여러 개를 정사각형 또는 직사각형 모양으로 이어 붙여서 큰 파일을 만들 수 있습니다. 예를 들어 200x400픽셀, 400x200픽셀 또는 400x400픽셀 타일을 만들 수 있습니다. |

애플리케이션에서 최상의 디스플레이를 원한다면 앞의 표에 정리된 치수와 일치하는 이미지를 만들어야 합니다.

## <a name="upload-the-images"></a>이미지 업로드

다음 섹션에서는 여러 위치에 이미지를 업로드하는 방법을 설명합니다.

### <a name="application-manager"></a>애플리케이션 관리자

**내 응용 프로그램** 페이지에서 사용할 이미지를 업로드 하려면 **관리** 섹션의 **응용 프로그램 설정** 페이지로 이동 합니다. 이 작업을 완료하려면 관리자여야 합니다.

![애플리케이션 이미지 업로드](media/howto-prepare-images/uploadapplicationmanager.png)

**응용 프로그램 이미지** 타일을 선택 하 여 로컬 컴퓨터에서 이미지 (268x160 px)를 업로드 합니다.

### <a name="application-dashboard"></a>애플리케이션 대시보드

응용 프로그램 대시보드에서 이미지를 업로드 하려면 응용 프로그램의 **대시보드** 페이지로 이동 하 여 **편집**을 선택 합니다. 이 작업을 완료하려면 작성자여야 합니다.

![대시보드 이미지 업로드](media/howto-prepare-images/uploadhomepage.png)

**이미지 구성**에서 **이미지** 타일을 선택 하 여 로컬 컴퓨터에서 이미지를 업로드 합니다. 최소 타일 크기는 200x200픽셀이고, 작은 타일 여러 개를 정사각형 또는 직사각형 모양으로 이어 붙여서 큰 파일을 만들 수 있습니다. 예를 들어 200x400픽셀, 400x200픽셀 또는 400x400픽셀 타일을 만들 수 있습니다.

업로드된 이미지를 **저장**합니다. 편집 모드에서 크기를 조정할 수 있습니다. 완료 되 면 **완료** 를 선택 합니다.

### <a name="device-template"></a>디바이스 템플릿

디바이스 템플릿에 이미지를 업로드하려면 **디바이스 템플릿**으로 이동하여 디바이스 템플릿을 선택합니다. 이 작업을 완료하려면 작성자여야 합니다.

![디바이스 템플릿 이미지 업로드](media/howto-prepare-images/uploaddevicetemplate.png)

이미지 타일을 선택 하 여 로컬 컴퓨터에서 이미지 (64x64 px)를 업로드 합니다.

### <a name="device-dashboard"></a>디바이스 대시보드

디바이스 대시보드에 이미지를 업로드하려면 **디바이스 템플릿**으로 이동하여 디바이스 템플릿을 선택합니다. 그런 다음, **대시보드** 탭을 선택합니다. 이 작업을 완료하려면 작성자여야 합니다.

![디바이스 대시보드 이미지 업로드](media/howto-prepare-images/uploaddevicedashboard.png)

**이미지 구성**에서 **이미지** 타일을 선택한 다음 로컬 컴퓨터에서 업로드할 파일을 선택 합니다. 최소 타일 크기는 200x200픽셀이고, 작은 타일 여러 개를 정사각형 또는 직사각형 모양으로 이어 붙여서 큰 파일을 만들 수 있습니다. 예를 들어 200x400픽셀, 400x200픽셀 또는 400x400픽셀 타일을 만들 수 있습니다.

업로드된 이미지를 **저장**합니다. 편집 모드에서 크기와 위치를 조정할 수 있습니다. 완료 되 면 **완료** 를 선택 합니다.

### <a name="device-set-dashboard"></a>디바이스 집합 대시보드

디바이스 집합 대시보드에 사용할 이미지를 업로드하려면  **집합**으로 이동하여 디바이스 집합을 선택하고, 디바이스를 선택합니다. 그런 다음 **대시보드** 페이지를 선택 하 고 **편집**을 선택 합니다.

![디바이스 집합 대시보드 이미지 업로드](media/howto-prepare-images/uploaddevicesetdashboard.png)

**이미지 구성**에서 **이미지** 타일을 선택 하 여 로컬 컴퓨터에서 이미지를 업로드 합니다. 최소 타일 크기는 200x200픽셀이고, 작은 타일 여러 개를 정사각형 또는 직사각형 모양으로 이어 붙여서 큰 파일을 만들 수 있습니다. 예를 들어 200x400픽셀, 400x200픽셀 또는 400x400픽셀 타일을 만들 수 있습니다.

업로드된 이미지를 **저장**합니다. 편집 모드에서 크기와 위치를 조정할 수 있습니다. 완료 되 면 **완료** 를 선택 합니다.

## <a name="next-steps"></a>다음 단계

이제 Azure IoT Central 응용 프로그램에 이미지를 준비 하 고 업로드 하는 방법을 배웠으므로 제안 된 다음 단계는 다음과 같습니다.

* [Azure IoT Central UI 사용자 지정](./howto-customize-ui.md)
* [응용 프로그램 대시보드 구성](./howto-configure-homepage.md)
* [Azure IoT Central 애플리케이션에서 장치 관리](howto-manage-devices.md)