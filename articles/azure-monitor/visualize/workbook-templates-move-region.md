---
title: 통합 문서 템플릿 Azure Monitor-영역 이동
description: 통합 문서 템플릿을 다른 지역으로 이동 하는 방법
services: azure-monitor
author: gardnerjr
manager: acearun
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 08/12/2020
ms.author: jgardner
ms.openlocfilehash: 9787118b1912a5697c02e116be99b1f396748937
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100613804"
---
# <a name="move-an-azure-workbook-template-to-another-region"></a>Azure 통합 문서 템플릿을 다른 지역으로 이동

이 문서에서는 Azure 통합 문서 템플릿 리소스를 다른 Azure 지역으로 이동 하는 방법을 설명 합니다. 여러 가지 이유로 리소스를 다른 지역으로 이동할 수 있습니다. 예를 들어, 새 Azure 지역을 활용 하 여 특정 지역 에서만 사용할 수 있는 기능 또는 서비스를 배포 하거나, 내부 정책 및 거 버 넌 스 요구 사항을 충족 하거나, 용량 계획 요구 사항에 대 한 응답으로 배포할 수 있습니다.

현재 통합 문서 템플릿 리소스를 만드는 데는 포털 UI가 없습니다. 이러한 리소스를 만드는 유일한 방법은 [Azure Resource Manager 템플릿 (ARM 템플릿) 배포를 통해](../visualize/workbooks-automate.md)서는 것입니다. 따라서 템플릿을 이동 하는 가장 쉬운 방법은 이전 ARM 템플릿을 다시 사용 하 여 새 지역에 배포 하도록 업데이트 하는 것입니다.

## <a name="prerequisites"></a>필수 구성 요소

* 대상 지역에서 통합 문서 템플릿이 지원 되는지 확인 합니다.

## <a name="prepare"></a>준비

* 통합 문서 템플릿에 대해 이전에 사용한 ARM 템플릿을 식별 합니다.

## <a name="move"></a>이동

1. 새 지역을 참조 하도록 이전에 사용한 템플릿을 업데이트 합니다.

   > [!NOTE]
   > 중복 된 이름을 방지 하려면 통합 문서 템플릿에 새 이름을 사용 해야 할 수도 있습니다.

2. ARM 템플릿 배포를 통해 업데이트 된 템플릿을 배포 하 여 원하는 지역에 새 통합 문서 템플릿을 만듭니다.

## <a name="verify"></a>확인

Azure 통합 문서 찾아보기 UI를 사용 하 여 새로 배포 된 통합 문서 템플릿을 찾습니다. 위치가 대상 위치 인지 확인 합니다.

## <a name="clean-up"></a>정리

새 지역에서 통합 문서 템플릿을 만든 후에는 이전 지역에서 원래 통합 문서 템플릿을 삭제 합니다.
1. Azure 통합 문서 찾아보기 UI에서 통합 문서 템플릿을 찾습니다.
2. 삭제할 통합 문서 템플릿을 선택 합니다.
3. "삭제" 명령을 선택 합니다.

새 지역으로 가져오기 위해 통합 문서 템플릿의 이름을 바꾼 경우 Azure 통합 문서 템플릿 리소스 보기에서 "이름 바꾸기" 명령을 사용 하 여 원본 항목이 삭제 된 후 통합 문서 템플릿의 이름을 이전 이름으로 바꿀 수 있습니다.

## <a name="next-steps"></a>다음 단계

템플릿 대신 통합 문서를 이동 해야 하나요? [Azure 통합 문서를 다른 지역으로 이동](./workbooks-move-region.md)하는 방법을 참조 하세요.

