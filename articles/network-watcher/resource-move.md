---
title: Azure Network Watcher 리소스 이동 | Microsoft Docs
description: 지역 간에 Azure Network Watcher 리소스 이동
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: damendo
ms.openlocfilehash: 4853f485e4424c3c3263a18d27834d0f9ae94918
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019655"
---
# <a name="moving-azure-network-watcher-resources-across-regions"></a>지역 간에 Azure Network Watcher 리소스 이동

## <a name="moving-the-network-watcher-resource"></a>Network Watcher 리소스 이동
Network Watcher 리소스는 Network Watcher에 대 한 백 엔드 서비스를 나타내며 Azure를 통해 완전히 관리 됩니다. 고객은 관리할 필요가 없습니다. 이동 작업은이 리소스에서 지원 되지 않습니다.

## <a name="moving-child-resources-of-network-watcher"></a>Network Watcher의 자식 리소스 이동
리소스를 지역 간에 이동 하는 것은 현재 리소스 종류의 모든 자식 리소스에 대해 지원 되지 않습니다 `*networkWatcher*` .

## <a name="next-steps"></a>다음 단계
* [Network Watcher 개요](./network-watcher-monitoring-overview.md) 읽기
* [NETWORK WATCHER FAQ](./frequently-asked-questions.md) 참조