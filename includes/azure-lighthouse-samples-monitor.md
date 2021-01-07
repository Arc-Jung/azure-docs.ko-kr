---
title: 포함 파일
description: 포함 파일
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/11/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 0056e18b6cb3aad2a4504bbe20b3b3421793489e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97356273"
---
이러한 샘플은 Azure Monitor를 사용하여 Azure 위임 리소스 관리를 위해 온보드된 구독에 대한 경고를 만드는 방법을 보여 줍니다.

| **템플릿** | **설명** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) | 관리 테넌트에서 지난 1일 동안의 활동을 쿼리하고 [추가되거나 제거된 위임에 대해 보고](../articles/lighthouse/how-to/monitor-delegation-changes.md)(또는 성공하지 못한 시도)합니다.|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/alert-using-actiongroup) | 이 템플릿은 Azure 경고를 만들고 기존 작업 그룹에 연결합니다.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/multiple-loganalytics-alerts) | Kusto 쿼리를 기반으로 여러 로그 경고를 만듭니다.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegation-alert-for-customer) | 사용자가 관리 테넌트에 구독을 위임하면 테넌트에 경고를 배포합니다.|
| [workbook-activitylogs-by-domain](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) | 도메인 이름으로 필터링하는 옵션을 사용하여 구독 간에 Azure 활동 로그를 표시합니다. |
