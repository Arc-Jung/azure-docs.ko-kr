---
title: 정기적 백업으로 Azure Cosmos DB 계정 구성
description: 이 문서에서는 백업 간격을 사용 하 여 정기적 백업으로 Azure Cosmos DB 계정을 구성 하는 방법을 설명 합니다. 보존. 또한 지원 담당자에 게 연락 하 여 데이터를 복원 하는 방법을 설명 합니다.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/13/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 69a9f0a82f5c19504564825e47f69ab8414e0909
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102565838"
---
# <a name="configure-azure-cosmos-db-account-with-periodic-backup"></a>정기적 백업으로 Azure Cosmos DB 계정 구성
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB는 자동으로 데이터의 백업을 정기적으로 수행합니다. 자동 백업은 데이터베이스 작업의 성능이나 가용성에 영향을 주지 않고 수행됩니다. 모든 백업은 스토리지 서비스에서 개별적으로 저장되고 이러한 백업은 지역 재해에 대비한 복원을 위해 전역적으로 복제됩니다. Azure Cosmos DB를 사용하면 데이터뿐만 아니라 데이터의 백업도 지역 재해에 대해 중복성 및 복원력이 뛰어납니다. 다음 단계에서는 Azure Cosmos DB 데이터 백업을 수행 하는 방법을 보여 줍니다.

* Azure Cosmos DB는 4 시간 마다 데이터베이스의 전체 백업을 자동으로 수행 하며, 언제 든 지 최신 두 백업만 기본적으로 저장 됩니다. 기본 간격이 작업에 충분 하지 않은 경우 Azure Portal에서 백업 간격과 보존 기간을 변경할 수 있습니다. Azure Cosmos 계정을 만든 후 또는 이후에 백업 구성을 변경할 수 있습니다. 컨테이너 또는 데이터베이스를 삭제 한 경우에는 지정 된 컨테이너 또는 데이터베이스의 기존 스냅숏이 30 일 동안 유지 Azure Cosmos DB.

* Azure Cosmos DB은 이러한 백업을 Azure Blob 저장소에 저장 하는 반면 실제 데이터는 Azure Cosmos DB 내에 로컬로 상주 합니다.

* 짧은 대기 시간을 보장 하기 위해 백업의 스냅숏은 Azure Blob storage에 현재 쓰기 지역과 동일한 지역 (또는 다중 지역 쓰기 구성이 있는 경우 쓰기 지역 중 **하나** )에 저장 됩니다. 지역적 재해에 대비한 복구를 위해 Azure Blob Storage에 있는 백업 데이터의 각 스냅샷은 GRS(지역 중복 스토리지)를 통해 다른 지역으로 다시 복제됩니다. 백업이 복제되는 지역은 원본 지역 및 원본 지역에 연결된 지역 쌍을 기반으로 합니다. 자세한 내용은 [Azure 지역의 지역 중복 쌍 목록](../best-practices-availability-paired-regions.md) 문서를 참조하세요. 이 백업에 직접 액세스할 수 없습니다. Azure Cosmos DB 팀은 지원 요청을 통해 요청하는 경우 백업을 복원합니다.

  다음 이미지는 미국 서부의 기본 실제 파티션 3개를 모두 포함하는 Azure Cosmos 컨테이너가 미국 서부의 원격 Azure Blob Storage 계정에 백업된 다음, 미국 동부에 복제되는 과정을 보여줍니다.

  :::image type="content" source="./media/configure-periodic-backup-restore/automatic-backup.png" alt-text="GRS Azure Storage에 있는 모든 Cosmos DB 엔터티의 정기적 전체 백업" lightbox="./media/configure-periodic-backup-restore/automatic-backup.png" border="false":::

* 백업은 애플리케이션의 성능이나 가용성에 영향을 주지 않고 수행됩니다. Azure Cosmos DB는 추가로 프로 비전 된 처리량 (RUs)을 사용 하거나 데이터베이스의 성능 및 가용성에 영향을 주지 않고 백그라운드에서 데이터 백업을 수행 합니다.

## <a name="modify-the-backup-interval-and-retention-period"></a><a id="configure-backup-interval-retention"></a>백업 간격 및 보존 기간 수정

Azure Cosmos DB는 4 시간 마다 데이터의 전체 백업을 자동으로 수행 하 고, 언제 든 지 최신 두 개의 백업이 저장 됩니다. 이 구성은 기본 옵션이 며 추가 비용 없이 제공 됩니다. Azure Cosmos 계정을 만드는 중이거나 계정을 만든 후에 이러한 기본 백업 간격과 보존 기간을 변경할 수 있습니다. 백업 구성은 Azure Cosmos 계정 수준에서 설정되며, 각 계정에 대해 구성해야 합니다. 계정에 대 한 백업 옵션을 구성한 후에는 해당 계정 내의 모든 컨테이너에 적용 됩니다. 현재는 Azure Portal에서만 백업 옵션을 변경할 수 있습니다.

실수로 데이터를 삭제 하거나 손상 한 경우 **데이터 복원에 대 한 지원 요청을 만들기 전에 계정에 대 한 백업 보존 기간을 7 일 이상으로 늘려야 합니다. 이 이벤트의 8 시간 이내에 보존 기간을 늘리는 것이 가장 좋습니다.** 이렇게 하면 Azure Cosmos DB 팀이 계정을 복원할 충분한 시간을 갖게 됩니다.

다음 단계를 사용 하 여 기존 Azure Cosmos 계정에 대 한 기본 백업 옵션을 변경 합니다.

1. Azure Portal에 로그인 [합니다.](https://portal.azure.com/)
1. Azure Cosmos 계정으로 이동 하 여 **Backup & 복원** 창을 엽니다. 필요에 따라 백업 간격 및 백업 보존 기간을 업데이트 합니다.

   * **백업 간격** -Azure Cosmos DB 데이터의 백업을 수행 하려고 시도 하는 간격입니다. 백업에는 0이 아닌 시간이 사용 되며, 경우에 따라 다운스트림 종속성으로 인해 오류가 발생할 수 있습니다. Azure Cosmos DB는 구성 된 간격으로 백업을 수행 하는 것이 가장 좋습니다. 그러나 백업이 해당 시간 간격 내에 완료 되는 것을 보장 하지는 않습니다. 이 값은 몇 시간 또는 몇 분 이내에 구성할 수 있습니다. 백업 간격은 1 시간 보다 작고 24 시간 보다 클 수 없습니다. 이 간격을 변경 하면 마지막 백업이 수행 된 시간부터 새 간격이 적용 됩니다.

   * **백업 보존** -각 백업이 유지 되는 기간을 나타냅니다. 몇 시간 또는 며칠 내에 구성할 수 있습니다. 최소 보존 기간은 백업 간격 (시간)의 두 배 미만일 수 없으며 720 시간 보다 클 수 없습니다.

   * **보존 된 데이터 복사본** -기본적으로 데이터의 백업 복사본 두 개는 무료로 제공 됩니다. 복사본이 두 개 이상 필요한 경우에는 추가 요금이 부과 됩니다. 추가 사본에 대 한 정확한 가격은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/) 에서 사용 된 저장소 섹션을 참조 하세요.

   :::image type="content" source="./media/configure-periodic-backup-restore/configure-backup-interval-retention.png" alt-text="기존 Azure Cosmos 계정에 대 한 백업 간격 및 보존을 구성 합니다." border="true":::

계정을 만드는 동안 백업 옵션을 구성 하는 경우 **정기적** 으로 또는 **연속** 되는 **백업 정책을** 구성할 수 있습니다. 정기 정책을 사용 하면 백업 간격 및 백업 보존을 구성할 수 있습니다. 현재 연속 정책은 등록 전용으로 사용할 수 있습니다. Azure Cosmos DB 팀은 워크 로드를 평가 하 고 요청을 승인 합니다.

:::image type="content" source="./media/configure-periodic-backup-restore/configure-periodic-continuous-backup-policy.png" alt-text="새 Azure Cosmos 계정에 대 한 주기적 또는 연속 백업 정책을 구성 합니다." border="true":::

## <a name="request-data-restore-from-a-backup"></a><a id="request-restore"></a>백업에서 데이터 복원 요청

데이터베이스나 컨테이너를 실수로 삭제 한 경우 [지원 티켓을](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 제출 하거나 [Azure 지원에 문의](https://azure.microsoft.com/support/options/) 하 여 자동 온라인 백업에서 데이터를 복원할 수 있습니다. Azure 지원은 **표준**, **개발자** 및 계획 보다 높은 요금제와 같은 선택 된 계획에만 사용할 수 있습니다. Azure 지원은 **Basic** 플랜에는 사용할 수 없습니다. 여러 지원 플랜에 대해 자세히 알아보려면 [Azure 지원 플랜](https://azure.microsoft.com/support/plans/) 페이지를 참조하세요.

백업의 특정 스냅숏을 복원 하려면 해당 스냅숏에 대 한 백업 주기 동안 데이터를 사용할 수 있어야 Azure Cosmos DB.
복원을 요청하려면 다음 세부 정보가 필요합니다.

* 구독 ID를 준비합니다.

* 데이터가 어떻게 실수로 삭제되거나 수정되었는지에 따라 추가 정보를 준비해야 합니다. 일부 시간에 구애 받는 경우 불리할 수 있는 전후 과정을 최소화하기 위해 사전에 사용 가능한 정보를 확보하는 것이 좋습니다.

* 전체 Azure Cosmos DB 계정이 삭제된 경우 삭제된 계정의 이름을 제공해야 합니다. 삭제된 계정과 동일한 이름을 가진 다른 계정을 만들 경우 올바른 계정을 선택하는 데 도움이 되므로 이를 지원팀과 공유하세요. 복원 상태에 대 한 혼동을 최소화 하기 때문에 삭제 된 각 계정에 대해 다른 지원 티켓을 파일 하는 것이 좋습니다.

* 하나 이상의 데이터베이스를 삭제 하는 경우 Azure Cosmos 계정 및 Azure Cosmos 데이터베이스 이름을 제공 하 고 이름이 같은 새 데이터베이스가 있는지를 지정 해야 합니다.

* 하나 이상의 컨테이너가 삭제된 경우 Azure Cosmos 계정 이름, 데이터베이스 이름 및 컨테이너 이름을 제공해야 합니다. 또한 같은 이름의 컨테이너가 존재하는지 여부를 지정합니다.

* 실수로 데이터를 삭제 하거나 손상 한 경우 8 시간 이내에 [Azure 지원](https://azure.microsoft.com/support/options/) 에 문의 하 여 Azure Cosmos DB 팀이 백업에서 데이터를 복원 하는 데 도움을 받을 수 있도록 해야 합니다. **데이터 복원에 대 한 지원 요청을 만들기 전에 계정에 대 한 [백업 보존](#configure-backup-interval-retention) 기간을 7 일 이상으로 늘려야 합니다. 이 이벤트의 8 시간 이내에 보존 기간을 늘리는 것이 가장 좋습니다.** 이러한 방식으로 Azure Cosmos DB 지원 팀은 계정을 복원 하는 데 충분 한 시간을 갖게 됩니다.

Azure Cosmos 계정 이름, 데이터베이스 이름, 컨테이너 이름 외에도 데이터를 복원할 수 있는 시점을 지정 해야 합니다. 가능한 한 정확하게 해야 해당 시점에 가장 적합한 백업을 결정할 수 있습니다. **또한 시간을 UTC로 지정하는 것도 중요합니다.**

다음 스크린샷은 Azure Portal을 사용하여 데이터를 복원하기 위해 컨테이너(컬렉션/그래프/테이블)에 대한 지원 요청을 만드는 방법을 보여줍니다. 데이터 유형, 복원 목적, 데이터를 삭제 한 시간 등 요청에 우선 순위를 지정 하는 데 도움이 되는 기타 세부 정보를 제공 합니다.

:::image type="content" source="./media/configure-periodic-backup-restore/backup-support-request-portal.png" alt-text="Azure Portal를 사용 하 여 백업 지원 요청을 만듭니다." border="true":::

## <a name="considerations-for-restoring-the-data-from-a-backup"></a>백업에서 데이터를 복원할 때의 고려 사항

다음 시나리오 중 하나에서 실수로 데이터를 삭제 하거나 수정할 수 있습니다.  

* 전체 Azure Cosmos 계정을 삭제 합니다.

* 하나 이상의 Azure Cosmos 데이터베이스를 삭제 합니다.

* 하나 이상의 Azure Cosmos 컨테이너를 삭제 합니다.

* 컨테이너 내에서 Azure Cosmos 항목 (예: 문서)을 삭제 하거나 수정 합니다. 이 특정 사례는 일반적으로 데이터 손상 이라고 합니다.

* 공유 제공 데이터베이스 내에 있는 공유 제공 데이터베이스 또는 컨테이너가 삭제 되었거나 손상 되었습니다.

Azure Cosmos DB는 위의 모든 시나리오에서 데이터를 복원할 수 있습니다. 복원 하는 경우 복원 된 데이터를 저장 하기 위해 새 Azure Cosmos 계정이 만들어집니다. 지정 되지 않은 경우 새 계정의 이름은 형식이 지정 됩니다 `<Azure_Cosmos_account_original_name>-restored1` . 여러 복원을 시도 하면 마지막 자릿수가 증가 합니다. 미리 만든 Azure Cosmos 계정으로 데이터를 복원할 수 없습니다.

Azure Cosmos 계정을 실수로 삭제 하면 계정 이름이 사용 되 고 있지 않은 경우 동일한 이름의 새 계정으로 데이터를 복원할 수 있습니다. 따라서 계정을 삭제 한 후에는 다시 만들지 않는 것이 좋습니다. 복원 된 데이터는 동일한 이름을 사용 하는 것을 방지 하는 것이 아니라 적절 한 계정을 검색 하 여 어려움 없이 복원할 수 있습니다.

Azure Cosmos 데이터베이스를 실수로 삭제 하는 경우 해당 데이터베이스 내의 전체 데이터베이스 또는 컨테이너의 하위 집합을 복원할 수 있습니다. 또한 데이터베이스에서 특정 컨테이너를 선택 하 고 새 Azure Cosmos 계정으로 복원할 수 있습니다.

컨테이너 내에서 실수로 하나 이상의 항목을 삭제 하거나 수정 하는 경우 (데이터 손상 사례) 복원할 시간을 지정 해야 합니다. 데이터 손상이 있는 경우 시간이 중요 합니다. 컨테이너가 라이브 상태 이기 때문에 백업이 계속 실행 되므로 보존 기간 (기본값은 8 시간)을 초과 하 여 대기 하는 경우 백업을 덮어쓰게 됩니다. 백업을 덮어쓰는 것을 방지 하기 위해 계정에 대 한 백업 보존 기간을 7 일 이상으로 늘립니다. 데이터 손상 으로부터 8 시간 이내에 보존 기간을 늘리는 것이 가장 좋습니다.

실수로 데이터를 삭제 하거나 손상 한 경우 8 시간 이내에 [Azure 지원](https://azure.microsoft.com/support/options/) 에 문의 하 여 Azure Cosmos DB 팀이 백업에서 데이터를 복원 하는 데 도움을 받을 수 있도록 해야 합니다. 이러한 방식으로 Azure Cosmos DB 지원 팀은 계정을 복원 하는 데 충분 한 시간을 갖게 됩니다.

> [!NOTE]
> 데이터를 복원한 후에는 일부 원본 기능 또는 설정이 복원 된 계정으로 전달 되지 않습니다. 다음 설정은 새 계정으로 전달 되지 않습니다.
> * VNET 액세스 제어 목록
> * 저장 프로시저, 트리거 및 사용자 정의 함수
> * 다중 지역 설정  

데이터베이스 수준에서 처리량을 프로 비전 하는 경우이 경우 백업 및 복원 프로세스는 개별 컨테이너 수준이 아니라 전체 데이터베이스 수준에서 발생 합니다. 이러한 경우에는 복원할 컨테이너의 하위 집합을 선택할 수 없습니다.

## <a name="required-permissions-to-change-retention-or-restore-from-the-portal"></a>포털에서 보존 또는 복원을 변경 하는 데 필요한 권한
[CosmosdbBackupOperator](../role-based-access-control/built-in-roles.md#cosmosbackupoperator), 소유자 또는 참가자 역할의 일부인 보안 주체는 복원을 요청 하거나 보존 기간을 변경할 수 있습니다.

## <a name="understanding-costs-of-extra-backups"></a>추가 백업의 비용 이해
두 개의 백업은 무료로 제공 되며 [백업 저장소 가격 책정](https://azure.microsoft.com/en-us/pricing/details/cosmos-db/)에 설명 된 백업 저장소에 대 한 지역 기반 가격 책정에 따라 추가 백업이 청구 됩니다. 예를 들어 백업 보존이 240 시간, 10 일, 백업 간격을 24 시간으로 구성 된 경우입니다. 이는 백업 데이터의 복사본 10 개를 의미 합니다. 미국 서 부에 1TB의 데이터가 있다고 가정 하면, 해당 월의 백업 저장소에 대 한 비용은 0.12 * 1000 * 8이 됩니다. 


## <a name="options-to-manage-your-own-backups"></a>사용자 고유의 백업을 관리하기 위한 옵션

Azure Cosmos DB SQL API 계정을 통해 다음 방법 중 하나를 사용하면 백업을 직접 유지할 수도 있습니다.

* [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)를 사용하여 데이터를 주기적으로 선택한 스토리지로 이동합니다.

* Azure Cosmos DB [변경 피드](change-feed.md) 를 사용 하 여 전체 백업 또는 증분 변경에 대 한 데이터를 주기적으로 읽거나 증분 변경 내용을 저장소에 저장 합니다.

## <a name="post-restore-actions"></a>복원 후 작업

데이터 복원의 기본 목표는 실수로 삭제 하거나 수정한 데이터를 복구 하는 것입니다. 따라서 예상하는 것을 포함하도록 하려면 복구된 데이터의 콘텐츠를 먼저 검사하는 것이 좋습니다. 모든 항목이 양호 하면 다시 기본 계정으로 데이터를 마이그레이션할 수 있습니다. 복원 된 계정을 새 활성 계정으로 사용할 수 있지만 프로덕션 워크 로드가 있는 경우에는 권장 되는 옵션이 아닙니다. 

데이터를 복원한 후 새 계정 이름(일반적으로 `<original-name>-restored1` 형식임) 및 계정이 복구된 시간에 대한 알림을 받습니다. 복원된 계정은 프로비저닝된 처리량과 인덱싱 정책이 동일하며 원래 계정과 동일한 영역에 있습니다. 구독 관리자 또는 공동 관리자인 사용자는 복원된 계정을 볼 수 있습니다.

### <a name="migrate-data-to-the-original-account"></a>원본 계정으로 데이터 마이그레이션

다음은 원래 계정으로 데이터를 다시 마이그레이션하는 다양 한 방법입니다.

* [Azure Cosmos DB 데이터 마이그레이션 도구](import-data.md)를 사용 합니다.
* [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)를 사용 합니다.
* Azure Cosmos DB에서 [변경 피드](change-feed.md) 를 사용 합니다.
* 사용자 고유의 사용자 지정 코드를 작성할 수 있습니다.

데이터를 마이그레이션하는 즉시 컨테이너 또는 데이터베이스를 삭제하는 것이 좋습니다. 복원된 데이터베이스 또는 컨테이너를 삭제하지 않으면 요청 단위, 스토리지 및 송신 관련 비용이 발생합니다.

## <a name="next-steps"></a>다음 단계

* 복원 요청을 수행 하려면 Azure 지원에 문의 하 고 [Azure Portal 티켓을](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)제출 합니다.
* [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md), [CLI](continuous-backup-restore-command-line.md)또는 [Azure Resource Manager](continuous-backup-restore-template.md)를 사용 하 여 연속 백업을 구성 하 고 관리 합니다.
* 연속 백업 모드를 사용 하 여 데이터를 복원 하는 데 필요한 [권한을 관리](continuous-backup-restore-permissions.md) 합니다.
