---
title: Azure Monitor Log Analytics에서 고객 관리형 스토리지 계정 사용
description: Log Analytics 시나리오에 고유한 저장소 계정 사용
ms.subservice: logs
ms.topic: conceptual
author: noakup
ms.author: noakuper
ms.date: 09/03/2020
ms.openlocfilehash: 706392d95e371fe303bb9f2c18f59e4a224d83c0
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201062"
---
# <a name="using-customer-managed-storage-accounts-in-azure-monitor-log-analytics"></a>Azure Monitor Log Analytics에서 고객 관리형 스토리지 계정 사용

Log Analytics은 다양 한 시나리오에서 Azure Storage에 의존 합니다. 이 사용은 일반적으로 자동으로 관리 됩니다. 그러나 경우에 따라 고객 관리 저장소 계정이 라고도 하는 고유한 저장소 계정을 제공 하 고 관리 해야 하는 경우도 있습니다. 이 문서에서는 WAD/꼬마 로그, 개인 링크 및 CMK (고객 관리 키) 암호화를 위해 고객이 관리 하는 저장소를 사용 하는 방법을 설명 합니다. 

> [!NOTE]
> 서식 지정 및 콘텐츠가 변경 될 수 있는 경우 고객 관리 저장소에 업로드 Log Analytics 내용에 의존 하지 않는 것이 좋습니다.

## <a name="ingesting-azure-diagnostics-extension-logs-wadlad"></a>수집 Azure 진단 확장 로그 (WAD/꼬마)
Azure 진단 확장 에이전트 (Windows 및 Linux 에이전트에 대 한 WAD이 라고도 하는)는 다양 한 운영 체제 로그를 수집 하 여 고객 관리 저장소 계정에 저장 합니다. 그런 다음 이러한 로그를 Log Analytics으로 수집 하 여 검토 하 고 분석할 수 있습니다.
### <a name="how-to-collect-azure-diagnostics-extension-logs-from-your-storage-account"></a>저장소 계정에서 Azure 진단 확장 로그를 수집 하는 방법
[Azure Portal를](./diagnostics-extension-logs.md#collect-logs-from-azure-storage) 사용 하거나 [storage Insights API](/rest/api/loganalytics/storage%20insights/createorupdate)를 호출 하 여 저장소 계정을 Log Analytics 작업 영역에 저장소 데이터 원본으로 연결 합니다.

지원되는 데이터 형식은
* syslog
* Windows 이벤트
* Service Fabric
* ETW 이벤트
* IIS 로그

## <a name="using-private-links"></a>개인 링크 사용
고객 관리 저장소 계정은 개인 링크를 사용 하 여 Azure Monitor 리소스에 연결할 때 사용자 지정 로그 또는 IIS 로그를 수집 하는 데 사용 됩니다. 이러한 데이터 형식의 수집 프로세스는 먼저 로그를 중간 Azure Storage 계정에 업로드 한 다음 작업 영역에 수집 합니다. 

### <a name="using-a-customer-managed-storage-account-over-a-private-link"></a>개인 링크를 통해 고객이 관리 하는 저장소 계정 사용
#### <a name="workspace-requirements"></a>작업 영역 요구 사항
개인 링크를 통해 Azure Monitor에 연결 하는 경우 Log Analytics 에이전트는 개인 링크를 통해 액세스할 수 있는 작업 영역에만 로그를 보낼 수 있습니다. 이 요구 사항은 다음을 의미 합니다.
* Azure Monitor 개인 링크 범위 (AMPLS) 개체를 구성 합니다.
* 작업 영역에 연결
* 개인 링크를 통해 AMPLS을 네트워크에 연결 합니다. 

AMPLS 구성 절차에 대 한 자세한 내용은 [Azure 개인 링크를 사용 하 여 네트워크를 Azure Monitor에 안전 하 게 연결을](./private-link-security.md)참조 하세요. 

#### <a name="storage-account-requirements"></a>스토리지 계정 요구 사항
저장소 계정이 개인 링크에 성공적으로 연결 하려면 다음을 수행 해야 합니다.
* VNet 또는 피어 링 네트워크에 있고 개인 링크를 통해 VNet에 연결 되어 있어야 합니다.
* 연결 된 작업 영역과 동일한 영역에 있어야 합니다.
* Azure Monitor에서 저장소 계정에 액세스할 수 있도록 허용 합니다. 선택한 네트워크만 저장소 계정에 액세스할 수 있도록 선택한 경우 "신뢰할 수 있는 Microsoft 서비스가이 저장소 계정에 액세스할 수 있도록 허용" 예외를 선택 해야 합니다.
![저장소 계정 신뢰 MS services 이미지](./media/private-storage/storage-trust.png)
* 작업 영역에서 다른 네트워크의 트래픽만 처리 하는 경우 관련 네트워크/인터넷에서 들어오는 트래픽을 허용 하도록 저장소 계정을 구성 해야 합니다.

### <a name="using-a-customer-managed-storage-account-for-cmk-data-encryption"></a>CMK 데이터 암호화에 고객이 관리 하는 저장소 계정 사용
Azure Storage는 저장소 계정에서 미사용 데이터를 모두 암호화 합니다. 기본적으로 Microsoft 관리 키 (MMK)를 사용 하 여 데이터를 암호화 합니다. 그러나 Azure Storage를 사용 하면 Azure Key vault의 CMK를 사용 하 여 저장소 데이터를 암호화할 수도 있습니다. 사용자 고유의 키를 Azure Key Vault로 가져오거나 Azure Key Vault Api를 사용 하 여 키를 생성할 수 있습니다.
#### <a name="cmk-scenarios-that-require-a-customer-managed-storage-account"></a>고객이 관리 하는 저장소 계정이 필요한 CMK 시나리오
* 로그 암호화-CMK를 사용 하 여 경고 쿼리
* CMK를 사용 하 여 저장 된 쿼리 암호화

#### <a name="how-to-apply-cmk-to-customer-managed-storage-accounts"></a>고객 관리 저장소 계정에 CMK를 적용 하는 방법
##### <a name="storage-account-requirements"></a>스토리지 계정 요구 사항
스토리지 계정 및 키 자격 증명 모음은 동일한 지역에 있어야 하지만 서로 다른 구독에 있을 수도 있습니다. 암호화 및 키 관리 Azure Storage에 대 한 자세한 내용은 [미사용 데이터에 대 한 Azure Storage 암호화](../../storage/common/storage-service-encryption.md)를 참조 하세요.

##### <a name="apply-cmk-to-your-storage-accounts"></a>저장소 계정에 CMK 적용
Azure Key Vault에서 CMK를 사용 하도록 Azure Storage 계정을 구성 하려면 [Azure Portal](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json), [PowerShell](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json)또는 [CLI](../../storage/common/customer-managed-keys-configure-key-vault.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json)를 사용 합니다. 

## <a name="link-storage-accounts-to-your-log-analytics-workspace"></a>Log Analytics 작업 영역에 저장소 계정 연결
### <a name="using-the-azure-portal"></a>Azure Portal 사용
Azure Portal에서 작업 영역 메뉴를 열고 *연결 된 저장소 계정* 을 선택 합니다. 블레이드가 열리고 위에서 언급 한 사용 사례에 의해 연결 된 저장소 계정이 표시 됩니다 (개인 링크를 통해 수집, 저장 된 쿼리에 CMK 적용 또는 경고에 적용).
![연결 된 저장소 계정 블레이드 이미지 ](./media/private-storage/all-linked-storage-accounts.png) 테이블에서 항목을 선택 하면 해당 저장소 계정 세부 정보가 열립니다. 여기서이 형식에 대해 연결 된 저장소 계정을 설정 하거나 업데이트할 수 있습니다. 
![저장소 계정 블레이드 이미지 링크를 ](./media/private-storage/link-a-storage-account-blade.png) 선호 하는 경우 다른 사용 사례에 대해 동일한 계정을 사용할 수 있습니다.

### <a name="using-the-azure-cli-or-rest-api"></a>Azure CLI 또는 REST API 사용
[Azure CLI](/cli/azure/monitor/log-analytics/workspace/linked-storage) 또는 [REST API](/rest/api/loganalytics/linkedstorageaccounts)를 통해 저장소 계정을 작업 영역에 연결할 수도 있습니다.

해당 하는 dataSourceType 값은 다음과 같습니다.
* CustomLogs – 사용자 지정 로그 및 IIS 로그 수집에 저장소 계정을 사용 합니다.
* 쿼리-저장소 계정을 사용 하 여 저장 된 쿼리를 저장 합니다 (CMK 암호화에 필요).
* 경고-저장소 계정을 사용 하 여 로그 기반 경고를 저장 하려면 (CMK 암호화에 필요)


## <a name="managing-linked-storage-accounts"></a>연결 된 저장소 계정 관리

### <a name="create-or-modify-a-link"></a>링크 만들기 또는 수정
저장소 계정을 작업 영역에 연결 하면 서비스에서 소유 하는 저장소 계정 대신 해당 계정을 사용 하 여 Log Analytics 됩니다. 이 문서의 설명에 따라 Azure Automation Hybrid Runbook Worker를 제거할 수 있습니다. 
* 여러 저장소 계정을 등록 하 여 로그의 부하 분산
* 여러 작업 영역에 동일한 저장소 계정 다시 사용

### <a name="unlink-a-storage-account"></a>스토리지 계정 연결 해제
저장소 계정 사용을 중지 하려면 작업 영역에서 저장소의 연결을 해제 합니다. 작업 영역에서 모든 저장소 계정 연결을 해제 하면 Log Analytics 서비스 관리 저장소 계정에 의존 하는 것을 의미 합니다. 네트워크에서 인터넷에 대 한 액세스가 제한 된 경우 이러한 저장소를 사용 하지 못할 수 있으며 저장소에 의존 하는 모든 시나리오가 실패 합니다.

### <a name="replace-a-storage-account"></a>스토리지 계정 교체
수집에 사용 되는 저장소 계정을 교체 하려면
1.  **새 저장소 계정에 대 한 링크를 만듭니다.** 로깅 에이전트는 업데이트 된 구성을 가져오고 데이터를 새 저장소로 보내기 시작 합니다. 이 프로세스는 몇 분 정도 걸릴 수 있습니다.
2.  **그런 다음 에이전트가 제거 된 계정에 쓰기를 중지 하도록 이전 저장소 계정의 연결을 해제 합니다.** 수집 프로세스는 모두 수집될 때까지 이 계정의 데이터를 계속 읽습니다. 모든 로그가 수집될 때까지 스토리지 계정을 삭제하지 마세요.

### <a name="maintaining-storage-accounts"></a>저장소 계정 유지 관리
#### <a name="manage-log-retention"></a>로그 보존 관리
사용자 고유의 저장소 계정을 사용 하는 경우에는 사용자가 보유 합니다. Log Analytics은 개인 저장소에 저장 된 로그를 삭제 하지 않습니다. 대신, 기본 설정에 따라 부하를 처리 하는 정책을 설정 해야 합니다.

#### <a name="consider-load"></a>로드 고려
저장소 계정은 제한 요청을 시작 하기 전에 특정 로드의 읽기 및 쓰기 요청을 처리할 수 있습니다. 자세한 내용은 [Blob 저장소의 확장성 및 성능 목표](../../storage/common/scalability-targets-standard-account.md)를 참조 하세요. 제한은 로그를 수집 하는 데 걸리는 시간에 영향을 줍니다. 저장소 계정이 오버 로드 된 경우 추가 저장소 계정을 등록 하 여 부하를 분산 합니다. 저장소 계정의 용량 및 성능을 모니터링 하려면 [Azure Portal의 정보]( https://docs.microsoft.com/azure/azure-monitor/insights/storage-insights-overview)를 검토 하세요.

### <a name="related-charges"></a>관련 요금
저장소 계정은 저장 된 데이터의 볼륨, 저장소 유형 및 중복성 유형에 따라 청구 됩니다. 자세한 내용은 [블록 Blob 가격 책정](https://azure.microsoft.com/pricing/details/storage/blobs) 및 [Table Storage 가격 책정](https://azure.microsoft.com/pricing/details/storage/tables)을 참조하세요.


## <a name="next-steps"></a>다음 단계

- [Azure 개인 링크를 사용 하 여 네트워크를 Azure Monitor에 안전](private-link-security.md) 하 게 연결 하는 방법을 알아봅니다.
- [고객 관리 키 Azure Monitor](customer-managed-keys.md) 에 대해 알아보기
