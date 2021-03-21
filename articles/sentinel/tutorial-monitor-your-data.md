---
title: Azure Sentinel에서 Azure Monitor 통합 문서를 사용하여 데이터 시각화 | Microsoft Docs
description: 이 자습서를 사용하여 Azure Sentinel에서 통합 문서를 통해 데이터를 시각화하는 방법을 알아봅니다.
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
ms.date: 05/04/2020
ms.author: yelevin
ms.openlocfilehash: 048a089209ef7c5f20c96f77593e2cf39590147e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104600529"
---
# <a name="tutorial-visualize-and-monitor-your-data"></a>자습서: 데이터 시각화 및 모니터링



[데이터 원본을](quickstart-onboard.md) azure 센티널에 연결 하면 사용자 지정 대시보드를 만들 때 다양 한 기능을 제공 하는 Azure Monitor 통합 문서에 대 한 azure 센티널 채택을 사용 하 여 데이터를 시각화 하 고 모니터링할 수 있습니다. 통합 문서가 Azure Sentinel에 다르게 표시되는 반면, [Azure Monitor 통합 문서를 사용하여 대화형 보고서를 만드는](../azure-monitor/visualize/workbooks-overview.md) 방법을 확인하는 것이 유용할 수 있습니다. Azure Sentinel을 사용하면 데이터에 대한 사용자 지정 통합 문서를 만들 수 있으며 데이터 원본을 연결하는 즉시 데이터를 신속하게 파악할 수 있는 기본 제공 통합 문서 템플릿도 제공됩니다.


이 자습서는 Azure Sentinel에서 데이터를 시각화하는 데 도움이 됩니다.
> [!div class="checklist"]
> * 기본 제공 통합 문서 사용
> * 새 통합 문서 만들기

## <a name="prerequisites"></a>사전 요구 사항

- Azure Sentinel 작업 영역의 리소스 그룹에 대한 통합 문서 판독기 또는 통합 문서 기여자 권한이 있어야 합니다.

> [!NOTE]
> Azure Sentinel에서 볼 수 있는 통합 문서는 Azure Sentinel 작업 영역의 리소스 그룹 내에 저장되고 생성된 작업 영역에 의해 태그가 지정됩니다.

## <a name="use-built-in-workbooks"></a>기본 제공 통합 문서 사용

1. **통합 문서** 로 이동한 다음, **템플릿** 을 선택하여 Azure Sentinel 기본 제공 통합 문서의 전체 목록을 확인합니다. 연결한 데이터 형식과 관련된 항목을 확인하기 위해 관련 데이터를 Azure Sentinel로 이미 스트리밍한 경우 각 통합 문서의 **필수 데이터 형식** 필드에 녹색 확인 표시 옆에 있는 데이터 형식이 나열됩니다.
  ![통합 문서로 이동](./media/tutorial-monitor-data/access-workbooks.png)
1. 템플릿 **보기** 를 클릭 하 여 데이터로 채워진 템플릿을 확인 합니다.
  
1. 통합 문서를 편집 하려면 **저장** 을 선택한 다음 템플릿에 대 한 JSON 파일을 저장할 위치를 선택 합니다. 

   > [!NOTE]
   > 그러면 관련 템플릿을 기반으로 하는 Azure 리소스가 생성 되 고 데이터가 아닌 통합 문서의 JSON 파일이 저장 됩니다.


1. **저장 된 통합 문서 보기** 를 선택 합니다. 그런 다음, 위쪽에 있는 **편집** 단추를 클릭합니다. 이제 통합 문서를 편집하고 필요에 따라 사용자 지정할 수 있습니다. 통합 문서를 사용자 지정하는 방법에 대한 자세한 내용은 [Azure Monitor 통합 문서를 사용하여 대화형 보고서를 만드는](../azure-monitor/visualize/workbooks-overview.md) 방법을 참조하세요.
![통합 문서 보기](./media/tutorial-monitor-data/workbook-graph.png)
1. 변경을 수행한 후 통합 문서를 저장할 수 있습니다. 

1. 통합 문서를 복제할 수도 있습니다. **편집** 을 선택한 다음, **다른 이름으로 저장** 을 선택하여 동일한 구독 및 리소스 그룹 아래에 다른 이름으로 저장합니다. 이러한 복제 된 통합 문서는 **내 통합 문서** 탭에 표시 됩니다.


## <a name="create-new-workbook"></a>새 통합 문서 만들기

1. **통합 문서** 로 이동한 다음, **통합 문서 추가** 를 선택하여 새 통합 문서를 처음부터 만듭니다.
  ![새 통합 문서 화면을 보여 주는 스크린샷](./media/tutorial-monitor-data/create-workbook.png)

1. 통합 문서를 편집하려면 **편집** 을 선택한 다음, 필요에 따라 텍스트, 쿼리 및 매개 변수를 추가합니다. 통합 문서를 사용자 지정하는 방법에 대한 자세한 내용은 [Azure Monitor 통합 문서를 사용하여 대화형 보고서를 만드는](../azure-monitor/visualize/workbooks-overview.md) 방법을 참조하세요. 

1. 쿼리를 작성 하는 경우 **데이터 원본이** **로그** 로 설정 되 고 **리소스 종류** 가 **Log Analytics** 로 설정 되었는지 확인 한 다음 관련 작업 영역을 선택 합니다. 

1. 통합 문서를 만든 후 통합 문서를 저장 하 고 Azure 센티널 작업 영역의 구독 및 리소스 그룹에 저장 합니다.

1. 조직의 다른 사용자가 통합 문서를 사용하도록 하려면 **저장 위치** 에서 **공유 보고서** 를 선택합니다. 이 통합 문서를 사용자만 사용할 수 있도록 하려면 **내 보고서** 를 선택합니다.

1. 작업 영역에서 통합 문서를 전환 하려면  ![ 통합 문서 열기에 대 한 아이콘 열기를 선택할 수 있습니다. ](./media/tutorial-monitor-data/switch.png) 통합 문서의 위쪽 창에서 오른쪽에 열리는 창에서 통합 문서 간에 전환합니다.

   ![통합 문서 전환](./media/tutorial-monitor-data/switch-workbooks.png)


## <a name="print-a-workbook-or-save-as-pdf"></a>통합 문서 인쇄 또는 PDF로 저장

통합 문서를 인쇄 하거나 PDF로 저장 하려면 통합 문서 제목 오른쪽에 있는 옵션 메뉴를 사용 합니다.

1. :::image type="icon" source="media/whats-new/print-icon.png" border="false"::: **콘텐츠 > 인쇄** 옵션을 선택 합니다. 
2. 인쇄 화면에서 필요에 따라 인쇄 설정을 조정 하거나 **PDF로 저장** 을 선택 하 여 로컬에 저장 합니다.

예를 들면 다음과 같습니다.

:::image type="content" source="media/whats-new/print-workbook.png" alt-text="통합 문서를 인쇄 하거나 PDF로 저장 합니다.":::

## <a name="how-to-delete-workbooks"></a>통합 문서를 삭제하는 방법

저장 된 통합 문서 (저장 된 템플릿 또는 사용자 지정 통합 문서)를 삭제 하려면 통합 문서 페이지에서 삭제 하려는 저장 된 통합 문서를 선택 하 고 **삭제** 를 선택 합니다. 그러면 저장된 통합 문서가 제거됩니다.

> [!NOTE]
> 그러면 통합 문서 리소스 뿐만 아니라 템플릿에 대 한 모든 변경 내용이 제거 됩니다. 원래 템플릿은 사용 가능한 상태로 유지됩니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 Azure 통합 문서를 사용 하 여 Azure 센티널에서 데이터를 시각화 하는 방법을 알아보았습니다.

위협에 대한 응답을 자동화하는 방법에 대해 알아보려면 [Azure Sentinel에서 자동화된 위협 응답 설정](tutorial-respond-threats-playbook.md)을 참조하세요.
