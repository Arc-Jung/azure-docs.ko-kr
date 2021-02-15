---
title: 보안 모듈 및 장치 쌍
description: 보안 모듈 쌍의 개념 및 IoT 용 Defender에서이를 사용 하는 방법에 대해 알아봅니다.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2019
ms.author: mlottner
ms.openlocfilehash: feb84a1261e37600c68d8e372162033d4021f564
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100522923"
---
# <a name="security-module"></a>보안 모듈

이 문서에서는 Defender for IoT에서 장치 쌍 및 모듈을 사용 하는 방법을 설명 합니다.

## <a name="device-twins"></a>디바이스 쌍

Azure에 구축된 IoT 솔루션의 경우 디바이스 쌍은 디바이스 관리와 프로세스 자동화 모두에서 중요한 역할을 수행합니다.

Defender for IoT는 기존 IoT 디바이스 관리 플랫폼과 완벽하게 통합되어 디바이스 보안 상태를 관리하고 기존 디바이스 제어 기능을 활용할 수 있도록 지원합니다. IoT Hub 쌍 메커니즘을 사용 하 여 통합을 수행 합니다.

Azure IoT Hub에서 [장치](../iot-hub/iot-hub-devguide-device-twins.md) 쌍의 개념에 대해 자세히 알아보세요.

## <a name="security-module-twins"></a>보안 모듈 쌍

IoT 용 Defender는 서비스의 각 장치에 대 한 보안 모듈 쌍을 유지 관리 합니다.
보안 모듈 쌍은 솔루션의 각 특정 장치에 대 한 장치 보안과 관련 된 모든 정보를 포함 합니다.
장치 보안 속성은 보다 안전한 통신을 위해 전용 보안 모듈 쌍으로 유지 되 고 더 많은 리소스가 필요한 업데이트 및 유지 관리를 가능 하 게 합니다.

쌍을 만들고 사용자 지정 하 고 구성 하는 방법을 알아보려면 [보안 모듈 쌍 만들기](quickstart-create-security-twin.md) 및 [보안 에이전트 구성](how-to-agent-configuration.md) 을 참조 하세요. IoT Hub에서 모듈 쌍의 개념에 대해 자세히 알아보려면 [모듈 쌍 이해](../iot-hub/iot-hub-devguide-module-twins.md) 를 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [IoT 용 Defender 개요](overview.md)
- [보안 에이전트 배포](how-to-deploy-agent.md)
- [보안 에이전트 인증 방법](concept-security-agent-authentication-methods.md)