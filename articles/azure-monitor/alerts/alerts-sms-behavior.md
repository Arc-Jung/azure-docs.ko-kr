---
title: 작업 그룹의 SMS 경고 동작
description: SMS 메시지 형식 및 SMS 메시지에 대한 응답으로 구독 취소, 재구독 또는 도움을 요청합니다.
author: dkamstra
ms.author: dukek
services: monitoring
ms.topic: conceptual
ms.date: 02/16/2018
ms.subservice: alerts
ms.openlocfilehash: 98558c2b1525094277dd6c165349f25ce5cd3b23
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101717879"
---
# <a name="sms-alert-behavior-in-action-groups"></a>작업 그룹에서 SMS 경고 동작

## <a name="overview"></a>개요 
작업 그룹을 사용하여 작업 목록을 구성할 수 있습니다. 이 그룹은 경고를 정의할 때 사용되며, 경고가 트리거될 때 특정 작업 그룹이 알림을 받을 수 있도록 합니다. 지원되는 작업 중 하나는 SMS입니다. SMS 알림은 양방향 통신을 지원합니다. 사용자는 SMS에 응답하여 다음을 수행할 수 있습니다.

- **경고 구독 취소:** 사용자는 모든 작업 그룹 또는 단일 작업 그룹에 대한 모든 SMS 경고를 구독 취소할 수 있습니다.
- **경고 재구독:** 사용자는 모든 작업 그룹 또는 단일 작업 그룹에 대한 모든 SMS 경고를 재구독할 수 있습니다.  
- **도움 요청:** 사용자는 SMS에 대한 자세한 정보를 요청할 수 있습니다. 사용자는 이 문서로 리디렉션됩니다.

이 문서에서는 SMS 경고 동작과 사용자가 사용자의 로캘을 기반으로 취할 수 있는 응답 작업에 대해 다룹니다.

## <a name="receiving-an-sms-alert"></a>SMS 경고 받기
작업 그룹의 일부로 구성된 SMS 수신자는 경고가 발생할 때 SMS를 수신합니다. SMS에는 다음 정보가 포함됩니다.
* 이 경고가 전달된 작업 그룹의 짧은 이름
* 경고 제목

| 회신 | Description |
| ----- | ----------- |
| DISABLE `<Action Group Short name>` | 작업 그룹의 추가 SMS 해제 |
| ENABLE `<Action Group Short name>` | 작업 그룹의 SMS 다시 설정 |
| STOP | 모든 작업 그룹의 추가 SMS 해제 |
| START | 모든 작업 그룹의 SMS 다시 설정 |
| HELP | 이 문서에 대한 링크가 포함된 응답이 사용자에게 전송됩니다. |

>[!NOTE]
>사용자가 SMS 경고를 구독 취소했지만 새 작업 그룹에 추가된 경우 새 작업 그룹에 대한 SMS 경고를 받게 되지만 이전의 모든 작업 그룹에서 구독 취소 상태로 남아 있습니다.

## <a name="next-steps"></a>다음 단계
[활동 로그 경고의 개요](./alerts-overview.md)를 확인하고 알림을 받는 방법 알아보기  
[SMS 속도 제한](alerts-rate-limiting.md)에 대해 자세히 알아보기  
[작업 그룹](./action-groups.md) 에 대 한 자세한 정보