---
title: 스마트 그룹 관리
description: 경고 인스턴스에 대해 생성된 스마트 그룹 관리
ms.topic: conceptual
ms.subservice: alerts
ms.date: 09/24/2018
ms.openlocfilehash: 763bfefcf71b0be43722b99f31641015a5991607
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92105841"
---
# <a name="manage-smart-groups"></a>스마트 그룹 관리

[스마트 그룹](./alerts-smartgroups-overview.md?toc=%252fazure%252fazure-monitor%252ftoc.json)은 기계 학습 알고리즘을 사용하여 동시 발생 또는 유사성을 기준으로 경고를 그룹화하므로, 이제 사용자가 각 경고를 개별적으로 관리하는 대신 스마트 그룹을 관리할 수 있습니다. 이 문서에서는 Azure Monitor에서 스마트 그룹을 액세스하고 사용하는 방법을 안내합니다.

1. 경고 인스턴스에 대해 생성 된 스마트 그룹을 확인 하려면 다음 중 하나를 수행 하면 됩니다.

     1. **경고 요약** 페이지에서 **스마트 그룹** 을 클릭 합니다.    
    ![스마트 그룹이 강조 표시 된 경고 요약 페이지가 스크린샷으로 표시 됩니다.](./media/alerts-managing-smart-groups/sg-alerts-summary.jpg)
    
     1. 모든 경고 페이지에서 스마트 그룹별로 경고를 클릭 합니다.   
     ![스마트 그룹별 경고가 강조 표시 된 모든 경고 페이지가 스크린샷으로 표시 됩니다.](./media/alerts-managing-smart-groups/sg-all-alerts.jpg)

2. 이렇게 하면 경고 인스턴스에 대해 생성된 모든 스마트 그룹의 목록 보기로 이동됩니다. 여러 경고를 살펴보는 대신, 이제 스마트 그룹을 처리할 수 있습니다.   
![모든 경고 페이지가 표시 됩니다.](./media/alerts-managing-smart-groups/sg-list.jpg)

3. 스마트 그룹을 클릭하면 세부 정보 페이지가 열리며, 여기서 멤버 경고와 그룹화 이유를 확인할 수 있습니다. 이 집계를 사용하면 여러 경고를 살펴보는 대신 단일 스마트 그룹을 처리할 수 있습니다.   
![세부 정보 페이지를 보여 주는 스크린샷](./media/alerts-managing-smart-groups/sg-details.jpg)