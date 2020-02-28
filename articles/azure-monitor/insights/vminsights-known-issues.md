---
title: VM용 Azure Monitor(미리 보기)의 알려진 문제 | Microsoft Docs
description: 이 문서에서는 Azure VM 운영 체제의 상태, 애플리케이션 종속성 검색 및 성능 모니터링을 결합하는 Azure의 솔루션인 VM용 Azure Monitor의 알려진 문제를 설명합니다.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/02/2019
ms.openlocfilehash: 711b3707d536c4858578817589670edf0f467b64
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77670732"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM용 Azure Monitor(미리 보기)의 알려진 문제

이 문서에서는 Azure VM 운영 체제의 상태, 애플리케이션 구성 요소 검색 및 성능 모니터링을 결합하는 Azure의 솔루션인 VM용 Azure Monitor의 알려진 문제를 설명합니다. 

## <a name="health"></a>상태 
현재 상태 기능 릴리스의 알려진 문제는 다음과 같습니다.

- 제거하거나 삭제한 Azure VM은 얼마 동안 VM 목록 보기에 표시됩니다. 또한 제거하거나 삭제한 VM의 상태를 클릭하면 **상태 진단** 보기가 열리고 로딩 루프가 시작됩니다. 삭제한 VM의 이름을 선택하면 VM이 삭제되었다는 내용의 메시지가 포함된 창이 열립니다.
- 포털 또는 워크로드 모니터 API에서 즉시 업데이트할 수 있더라도 임계값 업데이트와 같은 구성 변경은 적용하는 데 최대 30분이 걸립니다. 
- 상태 진단 환경은 다른 보기보다 빠르게 업데이트됩니다. 보기 간에 전환할 때 정보가 지연될 수 있습니다. 
- Linux VM의 경우 단일 VM 보기의 상태 조건을 나열하는 페이지의 제목에는 사용자 정의 VM 이름 대신 VM의 전체 도메인 이름이 포함됩니다. 
- 지원되는 방법 중 하나를 사용하여 VM에 대한 모니터링을 사용하지 않도록 설정한 후에 다시 배포하려는 경우 동일한 작업 영역에 배포해야 합니다. 다른 작업 영역을 사용하고 해당 VM의 상태를 보려고 하면 일관되지 않은 동작이 표시될 수 있습니다.
- 작업 영역에서 솔루션 구성 요소를 제거한 후에는 Azure VM의 성능 상태, 특히 포털에서 보기로 이동할 경우 성능 및 맵 데이터를 계속 확인할 수 있습니다. 결국 데이터는 잠시 후 성능 및 맵 보기에서 표시되지 않지만 상태 보기는 VM에 대한 성능 상태를 계속 표시합니다. **지금 사용해 보기** 옵션을 사용하여 성능 및 맵 보기에서만 다시 온보딩할 수 있습니다.

## <a name="next-steps"></a>다음 단계
가상 컴퓨터의 모니터링을 사용 하도록 설정 하는 데 필요한 요구 사항 및 방법에 대 한 자세한 내용은 [VM용 Azure Monitor 사용](vminsights-enable-overview.md)을 참조 하세요.
