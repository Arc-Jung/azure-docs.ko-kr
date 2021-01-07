---
title: Azure Portal에서 템플릿 내보내기
description: Azure Portal를 사용 하 여 구독의 리소스에서 Azure Resource Manager 템플릿을 내보냅니다.
ms.topic: conceptual
ms.date: 07/29/2020
ms.openlocfilehash: ee97953a337bbb7cc9a8d1f042a3beae7bccdcae
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185694"
---
# <a name="single-and-multi-resource-export-to-a-template-in-azure-portal"></a>Azure Portal에서 템플릿에 대 한 단일 및 다중 리소스 내보내기

Azure Resource Manager 템플릿을 만드는 데 도움이 되도록 기존 리소스에서 템플릿을 내보낼 수 있습니다. 내보낸 템플릿을 사용 하면 리소스를 배포 하는 JSON 구문 및 속성을 이해할 수 있습니다. 향후 배포를 자동화 하려면 내보낸 템플릿으로 시작 하 고 시나리오에 맞게 수정 합니다.

리소스 관리자를 사용 하면 템플릿으로 내보내기 위해 하나 이상의 리소스를 선택할 수 있습니다. 템플릿에 필요한 리소스에 정확히 집중할 수 있습니다.

이 문서에서는 포털을 통해 템플릿을 내보내는 방법을 보여 줍니다. [Azure CLI](../management/manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](../management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)또는 [REST API](/rest/api/resources/resourcegroups/exporttemplate)를 사용할 수도 있습니다.

## <a name="choose-the-right-export-option"></a>올바른 내보내기 옵션 선택

두 가지 방법으로 템플릿을 내보낼 수 있습니다.

* **리소스 그룹 또는 리소스에서 내보냅니다**. 이 옵션은 기존 리소스에서 새 템플릿을 생성 합니다. 내보낸 템플릿은 리소스 그룹의 현재 상태에 대 한 "스냅숏"입니다. 해당 리소스 그룹 내에서 전체 리소스 그룹 또는 특정 리소스를 내보낼 수 있습니다.

* **배포 전이나 기록을 통해 내보냅니다**. 이 옵션은 배포에 사용 되는 템플릿의 정확한 복사본을 검색 합니다.

선택한 옵션에 따라 내보낸 템플릿의 품질은 다릅니다.

| 리소스 그룹 또는 리소스에서 | 배포 전 또는 기록에서 |
| --------------------- | ----------------- |
| Template은 리소스의 현재 상태에 대 한 스냅숏입니다. 여기에는 배포 후에 수행 된 모든 수동 변경 내용이 포함 됩니다. | 템플릿은 배포 시 리소스의 상태만 표시 합니다. 배포 후에 수행 하는 모든 수동 변경 내용은 포함 되지 않습니다. |
| 리소스 그룹에서 내보낼 리소스를 선택할 수 있습니다. | 특정 배포에 대 한 모든 리소스가 포함 됩니다. 이러한 리소스의 하위 집합을 선택 하거나 다른 시간에 추가 된 리소스를 추가할 수 없습니다. |
| 템플릿에는 일반적으로 배포 중에 설정 하지 않은 일부 속성을 포함 하 여 리소스에 대 한 모든 속성이 포함 됩니다. 템플릿을 다시 사용 하기 전에 이러한 속성을 제거 하거나 정리 하는 것이 좋습니다. | 템플릿에는 배포에 필요한 속성만 포함 됩니다. 템플릿을 바로 사용할 수 있습니다. |
| 템플릿에 다시 사용 해야 하는 모든 매개 변수가 포함 되지 않을 수 있습니다. 대부분의 속성 값은 템플릿에 하드 코드 되어 있습니다. 다른 환경에서 템플릿을 다시 배포 하려면 리소스를 구성 하는 기능을 향상 시키는 매개 변수를 추가 해야 합니다.  매개 변수 **포함** 을 선택 취소 하 여 매개 변수를 직접 작성할 수 있습니다. | 템플릿에는 여러 환경에서 쉽게 다시 배포할 수 있도록 하는 매개 변수가 포함 되어 있습니다. |

다음의 경우 리소스 그룹 또는 리소스에서 템플릿을 내보냅니다.

* 원래 배포 후에 적용 된 리소스에 대 한 변경 내용을 캡처해야 합니다.
* 내보낼 리소스를 선택 하려고 합니다.

배포 전이나 기록에서 다음과 같은 경우 템플릿을 내보냅니다.

* 다시 사용 하기 쉬운 템플릿이 필요 합니다.
* 원래 배포 후에 변경한 내용을 포함할 필요가 없습니다.

## <a name="limitations"></a>제한 사항

리소스 그룹 또는 리소스에서 내보내는 경우 내보낸 템플릿은 각 리소스 유형에 대해 [게시 된 스키마](https://github.com/Azure/azure-resource-manager-schemas/tree/master/schemas) 에서 생성 됩니다. 스키마에 리소스 종류에 대 한 최신 버전이 없는 경우도 있습니다. 내보낸 템플릿을 확인 하 여 필요한 속성이 포함 되어 있는지 확인 합니다. 필요한 경우 내보낸 템플릿을 편집 하 여 필요한 API 버전을 사용 합니다.

템플릿 내보내기 기능은 Azure Data Factory 리소스 내보내기를 지원 하지 않습니다. Data Factory 리소스를 내보내는 방법에 대 한 자세한 내용은 [Azure Data Factory에서 데이터 팩터리 복사 또는 복제](../../data-factory/copy-clone-data-factory.md)를 참조 하세요.

클래식 배포 모델을 통해 만든 리소스를 내보내려면 [리소스 관리자 배포 모델로 마이그레이션해야](../../virtual-machines/migration-classic-resource-manager-overview.md)합니다.

리소스 형식을 내보내지 못했음을 나타내는 템플릿을 내보낼 때 경고가 표시 되 면 해당 리소스에 대 한 속성을 계속 검색할 수 있습니다. 리소스 속성을 볼 수 있는 다양 한 옵션에 대 한 자세한 내용은 [리소스 속성 검색](view-resources.md)을 참조 하세요. 리소스 종류에 대 한 [Azure REST API](/rest/api/azure/) 를 살펴볼 수도 있습니다.

내보낸 템플릿을 만든 리소스 그룹의 리소스는 200 개로 제한 됩니다. 200 개 이상의 리소스가 있는 리소스 그룹을 내보내려는 경우 오류 메시지가 `Export template is not supported for resource groups more than 200 resources` 표시 됩니다.

## <a name="export-template-from-a-resource-group"></a>리소스 그룹에서 템플릿 내보내기

리소스 그룹에서 하나 이상의 리소스를 내보내려면:

1. 내보내려는 리소스를 포함 하는 리소스 그룹을 선택 합니다.

1. 확인란을 선택 하 여 하나 이상의 리소스를 선택 합니다.  모두 선택 하려면 **이름** 왼쪽에 있는 확인란을 선택 합니다. **템플릿 내보내기** 메뉴 항목은 하나 이상의 리소스를 선택한 후에만 사용 하도록 설정 됩니다.

   ![모든 리소스 내보내기](./media/export-template-portal/select-all-resources.png)

    스크린샷에는 저장소 계정만 선택 되어 있습니다.
1. **템플릿 내보내기** 를 선택합니다.

1. 내보낸 템플릿이 표시 되며 다운로드 하 고 배포할 수 있습니다.

   ![템플릿 표시](./media/export-template-portal/show-template.png)

   **매개 변수 포함** 은 기본적으로 선택 되어 있습니다.  이를 선택 하면 템플릿이 생성 될 때 모든 템플릿 매개 변수가 포함 됩니다. 사용자 고유의 매개 변수를 작성 하려는 경우이 확인란을 포함 하지 않으려면이 확인란을 선택/선택 취소 합니다.

## <a name="export-template-from-a-resource"></a>리소스에서 템플릿 내보내기

한 리소스를 내보내려면:

1. 내보내려는 리소스를 포함 하는 리소스 그룹을 선택 합니다.

1. 리소스를 열려면 내보내려는 리소스를 선택 합니다.

1. 해당 리소스에 대해 왼쪽 창에서 **템플릿 내보내기** 를 선택 합니다.

   ![리소스 내보내기](./media/export-template-portal/export-single-resource.png)

1. 내보낸 템플릿이 표시 되며 다운로드 하 고 배포할 수 있습니다. 템플릿에는 단일 리소스만 포함 됩니다. **매개 변수 포함** 은 기본적으로 선택 되어 있습니다.  이를 선택 하면 템플릿이 생성 될 때 모든 템플릿 매개 변수가 포함 됩니다. 사용자 고유의 매개 변수를 작성 하려는 경우이 확인란을 포함 하지 않으려면이 확인란을 선택/선택 취소 합니다.

## <a name="export-template-before-deployment"></a>배포 전 템플릿 내보내기

1. 배포 하려는 Azure 서비스를 선택 합니다.

1. 새 서비스에 대 한 값을 입력 합니다.

1. 유효성 검사를 통과 한 후 배포를 시작 하기 전에 **automation을 위한 템플릿 다운로드** 를 선택 합니다.

   ![템플릿 다운로드](./media/export-template-portal/download-before-deployment.png)

1. 템플릿이 표시 되 고 다운로드 및 배포에 사용할 수 있습니다.


## <a name="export-template-after-deployment"></a>배포 후 템플릿 내보내기

기존 리소스를 배포 하는 데 사용 된 템플릿을 내보낼 수 있습니다. 가져오는 템플릿은 배포에 사용 된 것과 정확히 일치 합니다.

1. 내보내려는 리소스 그룹을 선택 합니다.

1. **배포** 에서 링크를 선택합니다.

   ![배포 기록 선택](./media/export-template-portal/select-deployment-history.png)

1. 배포 기록에서 배포 중 하나를 선택합니다.

   ![배포 선택](./media/export-template-portal/select-details.png)

1. **템플릿** 을 선택 합니다. 이 배포에 사용 되는 템플릿이 표시 되 고 다운로드할 수 있습니다.

   ![템플릿 선택](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>다음 단계

- [Azure CLI](../management/manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](../management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)또는 [REST API](/rest/api/resources/resourcegroups/exporttemplate)를 사용 하 여 템플릿을 내보내는 방법에 대해 알아봅니다.
- 리소스 관리자 템플릿 구문에 대 한 자세한 내용은 [Azure Resource Manager 템플릿의 구조 및 구문 이해](template-syntax.md)를 참조 하세요.
- 템플릿을 개발 하는 방법을 알아보려면 단계별 [자습서](../index.yml)를 참조 하세요.
- Azure Resource Manager 템플릿 스키마를 보려면 [템플릿 참조](/azure/templates/)를 참조 하세요.