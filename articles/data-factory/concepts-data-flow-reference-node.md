---
title: Azure Data Factory 데이터 흐름 매핑 참조 노드
description: 조인, 조회, 공용 구조체에 대한 참조 노드를 추가하는 Data Factory 데이터 흐름
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 4ed17114cc0ce586c68c5b3e087acffdb82ea96c
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030499"
---
# <a name="mapping-data-flow-reference-node"></a>Mapping Data Flow 참조 노드



![참조 노드](media/data-flow/referencenode.png "참조 노드")

참조 노드는 자동으로 캔버스에 추가되어 노드가 캔버스에 존재하는 또 다른 노드에 연결되어 있다는 것을 나타냅니다. 참조 노드를 다른 데이터 흐름 변환에 대한 포인터 또는 참조로 생각할 수 있습니다.

예를 들어 다음과 같은 가치를 제공해야 합니다. 둘 이상의 데이터 스트림의 조인 또는 공용 구조체를 수행하는 경우 데이터 흐름 캔버스가 기본이 아닌 들어오는 스트림의 이름 및 설정을 반영하는 참조 노드를 추가할 수도 있습니다.

참조 노드는 이동하거나 삭제할 수 없습니다. 그러나 노드를 클릭하면 원래 변환 설정을 수정할 수 있습니다.

데이터 흐름이 참조 노드를 추가하는 경우를 제어하는 UI 규칙은 사용 가능한 공간 및 행 사이의 세로 간격을 기반으로 합니다.
