---
title: Azure 센티널에 끝점 데이터를 위한 Microsoft Defender 연결 | Microsoft Docs
description: Microsoft Defender for Endpoint (이전의 Microsoft Defender ATP) 데이터를 Azure 센티널에 연결 하는 방법에 대해 알아봅니다.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: 0db4e0fe0472c75f1eae392980ae697f53007244
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98623369"
---
# <a name="connect-alerts-from-microsoft-defender-for-endpoint-formerly-microsoft-defender-atp"></a>Microsoft Defender for Endpoint (이전의 Microsoft Defender ATP)에서 경고 연결

> [!IMPORTANT]
>
> - **Microsoft defender For Endpoint** 는 이전의 **Microsoft Defender Advanced Threat Protection** 또는 **mdatp** 로 알려져 있었습니다.
>
>     제품에서 아직 사용 중인 이전 이름 (Azure 센티널의 데이터 커넥터 포함)을 일정 기간 동안 볼 수 있습니다.

[Microsoft defender For endpoint](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) connector를 사용 하 여 끝점에 대 한 microsoft defender의 경고를 Azure 센티널로 스트리밍할 수 있습니다. 이렇게 하면 조직 전체에서 보안 이벤트를 보다 포괄적으로 분석 하 고, 유효 하 고 즉각적인 응답을 위해 플레이 북을 빌드할 수 있습니다.

> [!NOTE]
>
> 끝점의 [고급 구하기](/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview)에 대해 microsoft defender에서 새로운 원시 데이터 로그를 수집 하려면 Microsoft 365 Defender에 대 한 새 커넥터 (이전에는 Microsoft 위협 방지, [설명서 참조](./connect-microsoft-365-defender.md))를 사용 합니다.

## <a name="prerequisites"></a>필수 구성 요소

- [끝점 배포용 Microsoft Defender 설정](/windows/security/threat-protection/microsoft-defender-atp/licensing)에 설명 된 대로 끝점에 대 한 microsoft defender에 대 한 유효한 라이선스가 있어야 합니다. 

- Azure 센티널 테 넌 트의 전역 관리자 또는 보안 관리자 여야 합니다.

## <a name="connect-to-microsoft-defender-for-endpoint"></a>끝점에 대해 Microsoft Defender에 연결

Microsoft Defender for Endpoint가 배포 되 고 데이터를 수집 경우 경고를 Azure 센티널로 쉽게 스트리밍할 수 있습니다.

1. Azure 센티널에서 **데이터 커넥터** 를 선택 하 고, 갤러리에서 **Microsoft defender For Endpoint** ( *microsoft defender Advanced Threat Protection* 이 라고도 함)를 선택 하 고 **커넥터 열기 페이지** 를 선택 합니다.

1. **연결** 을 클릭합니다. 

1. **로그** 의 끝점 경고에 대해 Microsoft Defender를 쿼리하려면 쿼리 창에 **securityalert** 를 입력 하 고 **공급자 이름이** **mdatp** 인 필터를 추가 합니다.

## <a name="next-steps"></a>다음 단계
이 문서에서는 Microsoft Defender for Endpoint를 Azure 센티널에 연결 하는 방법을 알아보았습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.
- [데이터에 대한 가시성을 얻고 재적 위협을 확인](quickstart-get-visibility.md)하는 방법을 알아봅니다.
- [Azure Sentinel을 사용하여 위협 검색](./tutorial-detect-threats-built-in.md)을 시작합니다.