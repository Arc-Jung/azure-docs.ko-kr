---
title: Azure Portal를 사용 하 여 관리 되는 앱 모니터링
description: Azure Portal을 사용하여 관리되는 애플리케이션의 가용성과 경고를 모니터링하는 방법을 설명합니다.
author: tfitzmac
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: afe78dd00ecebdc54b6d73c4c8324729e117d95b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75651749"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>관리되는 애플리케이션의 배포된 인스턴스 모니터링

관리되는 애플리케이션을 Azure 구독에 배포한 후에는 애플리케이션의 상태를 확인할 수 있습니다. 이 문서에서는 상태 확인을 위한 Azure Portal의 옵션에 대해 설명합니다. 관리되는 애플리케이션에서 리소스의 가용성을 모니터링할 수 있으며, 경고를 설정하고 확인할 수도 있습니다.

## <a name="view-resource-health"></a>리소스 상태 보기

1. 관리되는 애플리케이션 인스턴스를 선택합니다.

   ![관리되는 애플리케이션 선택](./media/monitor-managed-application-portal/select-managed-application.png)

1. **Resource Health**를 선택합니다.

   ![Resource Health 선택](./media/monitor-managed-application-portal/select-resource-health.png)

1. 관리되는 애플리케이션에서 리소스의 가용성을 확인합니다.

   ![리소스 상태 보기](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>경고 보기

1. **경고**를 선택합니다.

   ![경고 선택](./media/monitor-managed-application-portal/select-alerts.png)

1. 경고 규칙을 구성한 경우 발생한 경고 관련 정보가 표시됩니다.

   ![경고 보기](./media/monitor-managed-application-portal/view-alerts.png)

1. 경고 규칙을 추가하려면 **+ 새 경고 규칙**을 선택합니다.

   ![경고 만들기](./media/monitor-managed-application-portal/create-new-alert.png)

관리되는 애플리케이션 인스턴스 또는 관리되는 애플리케이션의 리소스에 대해 경고를 만들 수 있습니다. 경고 만들기에 대한 자세한 내용은 [Microsoft Azure의 경고 개요](../../azure-monitor/platform/alerts-overview.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

* 관리되는 애플리케이션 예제는 [Azure 관리되는 애플리케이션의 샘플 프로젝트](sample-projects.md)를 참조하세요.
* 관리되는 애플리케이션을 배포하려면 [Azure Portal을 통해 서비스 카탈로그 앱 배포](deploy-service-catalog-quickstart.md)를 참조하세요.