---
title: Azure IoT Central 응용 프로그램에서 청구서를 관리 하 고 무료 가격 책정 계획에서 변환 | Microsoft Docs
description: 관리자는 청구서를 관리 하 고 Azure IoT Central 응용 프로그램에서 무료 요금제를 표준 가격 책정 요금제로 이동 하는 방법에 대해 알아봅니다.
author: dominicbetts
ms.author: dobett
ms.date: 11/23/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 50d0119b08d2c76a5f6111e485408ebcdace83c6
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96549024"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>IoT Central 응용 프로그램에서 청구 관리

이 문서에서는 관리자가 Azure IoT Central 청구를 관리할 수 있는 방법을 설명 합니다. 응용 프로그램을 무료 요금제에서 표준 요금제로 이동 하 고, 가격 책정 계획을 업그레이드 하거나 다운 그레이드할 수도 있습니다.

**관리** 섹션에 액세스 하려면 *관리자* 역할에 있거나 청구를 볼 수 있는 *사용자 지정 사용자 역할이* 있어야 합니다. Azure IoT Central 응용 프로그램을 만드는 경우 **관리자** 역할에 자동으로 할당 됩니다.

## <a name="move-from-free-to-standard-pricing-plan"></a>무료에서 표준 가격 책정 요금제로 이동

- 무료 가격 책정 요금제를 사용 하는 응용 프로그램은 만료 되기 7 일 전에 무료입니다. 데이터가 손실 되지 않도록 하기 위해 언제 든 지 표준 가격 책정 계획으로 이동할 수 있습니다.
- 표준 요금제를 사용 하는 응용 프로그램은 장치별로 청구 되며, 처음 두 장치는 응용 프로그램당 무료로 제공 됩니다.

[Azure IoT Central 가격 책정 페이지](https://azure.microsoft.com/pricing/details/iot-central/)에서 가격 책정에 대해 자세히 알아보세요.

가격 책정 섹션에서는 응용 프로그램을 무료에서 표준 가격 책정 계획으로 이동할 수 있습니다.

이 셀프 서비스 프로세스를 완료하려면 다음 단계를 수행합니다.

1. **관리** 섹션의 **가격 책정** 페이지로 이동 합니다.

    :::image type="content" source="media/howto-view-bill/freetrialbilling.png" alt-text="평가판 상태":::

1. **유료 요금제로 변환을** 선택 합니다.

    :::image type="content" source="media/howto-view-bill/convert.png" alt-text="평가판 변환":::

1. 적절 한 Azure Active Directory를 선택 하 고 유료 요금제를 사용 하는 응용 프로그램에 사용할 Azure 구독을 선택 합니다.

1. **변환** 을 선택 하면 응용 프로그램에서 유료 요금제를 사용 하 고 청구를 시작 합니다.

> [!Note]
> 기본적으로 *표준 2* 가격 책정 요금제로 변환 됩니다.

## <a name="how-to-change-your-application-pricing-plan"></a>응용 프로그램 가격 책정 계획을 변경 하는 방법

표준 요금제를 사용 하는 응용 프로그램은 장치별로 청구 되며, 처음 두 장치는 응용 프로그램당 무료로 제공 됩니다.

가격 책정 섹션에서는 언제 든 지 Azure IoT 가격 책정 계획을 업그레이드 하거나 다운 그레이드할 수 있습니다.

1. **관리** 섹션의 **가격 책정** 페이지로 이동 합니다.

    :::image type="content" source="media/howto-view-bill/pricing.png" alt-text="업그레이드 가격 책정 계획":::

1. **계획** 을 선택한 다음 **저장** 을 선택 하 여 업그레이드 또는 다운 그레이드를 선택 합니다.

## <a name="view-your-bill"></a>청구서 보기

1. 적절 한 Azure Active Directory를 선택 하 고 유료 요금제를 사용 하는 응용 프로그램에 사용할 Azure 구독을 선택 합니다.

1. **변환** 을 선택 하면 응용 프로그램에서 유료 요금제를 사용 하 고 청구를 시작 합니다.

> [!Note]
> 기본적으로 *표준 2* 가격 책정 요금제로 변환 됩니다.

## <a name="next-steps"></a>다음 단계

Azure IoT Central 응용 프로그램에서 청구서를 관리 하는 방법에 대해 알아보았습니다. 다음 단계에서는 Azure IoT Central에서 [응용 프로그램 UI 사용자 지정](howto-customize-ui.md) 에 대해 알아봅니다.