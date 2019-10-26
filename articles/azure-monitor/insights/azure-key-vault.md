---
title: Azure Monitor에서 Azure Key Vault 솔루션 Microsoft Docs
description: Azure Monitor에서 Azure Key Vault 솔루션을 사용 하 여 Azure Key Vault 로그를 검토할 수 있습니다.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/27/2019
ms.openlocfilehash: 8863280407de5d02b53a203b2b6385477aa9f8ae
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899205"
---
# <a name="azure-key-vault-analytics-solution-in-azure-monitor"></a>Azure Monitor의 Azure Key Vault 분석 솔루션

![Key Vault 기호](media/azure-key-vault/key-vault-analytics-symbol.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor에서 Azure Key Vault 솔루션을 사용 하 여 Azure Key Vault AuditEvent 로그를 검토할 수 있습니다.

솔루션을 사용하려면 Azure Key Vault 진단 로깅을 사용하도록 설정하고 진단을 Log Analytics 작업 영역으로 보내야 합니다. Azure Blob Storage에 로그를 쓸 필요는 없습니다.

> [!NOTE]
> 2017년 1월 Key Vault에서 Log Analytics로 로그를 보내도록 지원하는 방법이 변경되었습니다. 사용 중인 Key Vault 솔루션에서 제목에 *(사용되지 않음)* 을 표시하는 경우 수행해야 할 단계는 [이전 Key Vault 솔루션에서 마이그레이션](#migrating-from-the-old-key-vault-solution)을 참조하세요.
>
>

## <a name="install-and-configure-the-solution"></a>솔루션 설치 및 구성
다음 지침을 사용하여 Azure Key Vault 솔루션을 설치 및 구성합니다.

1. [솔루션 갤러리에서 Azure Monitor 솔루션 추가](../../azure-monitor/insights/solutions.md) 에 설명 된 프로세스를 사용 하 여 Log Analytics 작업 영역에 Azure Key Vault 솔루션을 추가 합니다.
2. [포털](#enable-key-vault-diagnostics-in-the-portal) 또는 [PowerShell](#enable-key-vault-diagnostics-using-powershell)을 사용하여 모니터링할 Key Vault 리소스에 대한 진단 로깅을 사용하도록 설정합니다.

### <a name="enable-key-vault-diagnostics-in-the-portal"></a>포털에서 Key Vault 진단 사용 설정

1. Azure Portal에서 모니터링할 Key Vault 리소스로 이동합니다.
2. *진단 설정* 을 선택 하 여 다음 페이지를 엽니다.

   ![Azure Key Vault 타일 이미지](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics01.png)
3. *진단 사용*을 클릭하여 다음 페이지를 엽니다.

   ![Azure Key Vault 타일 이미지](media/azure-key-vault/log-analytics-keyvault-enable-diagnostics02.png)
4. 진단 설정에 이름을 지정 합니다.
5. *Log Analytics로 보내기* 확인란을 클릭합니다.
6. 기존 Log Analytics 작업 영역을 선택하거나 작업 영역을 만듭니다.
7. *AuditEvent* 로그를 사용하도록 설정하려면 [로그] 아래의 확인란을 클릭합니다.
8. *저장* 을 클릭 하 여 Log Analytics 작업 영역에 대 한 진단 로깅을 사용 하도록 설정 합니다.

### <a name="enable-key-vault-diagnostics-using-powershell"></a>PowerShell을 사용하여 Key Vault 진단 사용 설정
다음 PowerShell 스크립트는 `Set-AzDiagnosticSetting`을 사용하여 Key Vault에 진단 로깅을 사용하도록 설정하는 방법에 대한 예제를 제공합니다.
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'

Set-AzDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Azure Key Vault 데이터 수집 상세 정보를 검토합니다.
Azure Key Vault 솔루션은 Key Vault에서 직접 진단 로그를 수집합니다.
Azure Blob Storage에 로그를 작성하지 않아도 되며 데이터 수집에 에이전트가 필요하지 않습니다.

다음 표에서는 데이터 수집 방법 및 Azure Key Vault에 대해 데이터가 수집되는 방식에 대한 기타 세부 정보를 보여 줍니다.

| 플랫폼 | 직접 에이전트 | Systems Center Operations Manager 에이전트 | Azure | Operations Manager 필요 여부 | 관리 그룹을 통해 전송되는 Operations Manager 에이전트 데이터 | 수집 빈도 |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | 도착 시 |

## <a name="use-azure-key-vault"></a>Azure Key Vault 사용
솔루션을 [설치한](https://azuremarketplace.microsoft.com/en-usrketplace/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview)후 Azure Monitor **개요** 페이지에서 **Key Vault 분석** 타일을 클릭 하 여 Key Vault 데이터를 봅니다. **인사이트** 섹션에서 **기타**를 클릭하여 **Azure Monitor** 메뉴에서 이 페이지를 엽니다. 

![Azure Key Vault 타일 이미지](media/azure-key-vault/log-analytics-keyvault-tile.png)

**Key Vault 분석** 타일을 클릭 한 후 로그 요약을 확인 한 후 다음 범주에 대 한 세부 정보를 드릴 다운할 수 있습니다.

* 시간 경과에 따른 모든 Key Vault 작업 크기
* 시간 경과에 따른 실패 작업
* 작업별 평균 작업 대기 시간
* 1000ms 이상이 소요되는 작업 수 및 1000ms 이상이 소요되는 작업 목록을 포함한 작업 서비스 품질

![Azure Key Vault 대시보드 이미지](media/azure-key-vault/log-analytics-keyvault01.png)

![Azure Key Vault 대시보드 이미지](media/azure-key-vault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>모든 작업에 대한 세부 사항을 보려면
1. **개요** 페이지에서 **Key Vault 분석** 타일을 클릭 합니다.
2. **Azure Key Vault** 대시보드에서 블레이드 중 하나에서 요약 정보를 검토한 다음 하나를 클릭하여 로그 검색 페이지에서 항목에 대한 세부 정보를 봅니다.

    로그 검색 페이지에서, 시간별 결과, 자세한 결과 및 로그 검색 기록을 볼 수 있습니다. 패싯으로 필터링하여 결과 범위를 좁힐 수 있습니다.

## <a name="azure-monitor-log-records"></a>Azure Monitor 로그 레코드
Azure Key Vault 솔루션은 Azure Diagnostics에서 [AuditEvent logs](../../key-vault/key-vault-logging.md)에서 수집된 **KeyVaults** 형식의 레코드를 분석합니다.  이러한 레코드의 속성은 다음 표에 있습니다.  

| 자산 | 설명 |
|:--- |:--- |
| `Type` |*AzureDiagnostics* |
| `SourceSystem` |*Azure* |
| `CallerIpAddress` |요청한 클라이언트의 IP 주소입니다. |
| `Category` | *AuditEvent* |
| `CorrelationId` |클라이언트가 서비스 쪽(키 자격 증명 모음) 로그를 사용하여 클라이언트 쪽 로그 상관 관계를 지정하도록 전달할 수 있는 선택적 GUID입니다. |
| `DurationMs` |밀리초 단위로 REST API 요청을 처리하는 데 걸린 시간입니다. 이 시간에는 네트워크 대기 시간이 포함되지 않으므로 클라이언트 쪽에서 측정한 시간이 이 시간과 일치하지 않을 수도 있습니다. |
| `httpStatusCode_d` |요청에서 반환한 HTTP 상태 코드(예: *200*)입니다. |
| `id_s` |요청의 고유 ID |
| `identity_claim_appid_g` | 응용 프로그램 ID의 GUID입니다. |
| `OperationName` |[Azure Key Vault 로깅](../../key-vault/key-vault-logging.md)에 설명된 대로 작업의 이름입니다. |
| `OperationVersion` |클라이언트에서 요청한 REST API 버전(예: *2015-06-01*)입니다. |
| `requestUri_s` |요청의 URI입니다. |
| `Resource` |Key Vault의 이름입니다. |
| `ResourceGroup` |Key Vault의 리소스 그룹입니다. |
| `ResourceId` |Azure 리소스 관리자 리소스 ID. Key Vault 로그의 경우 이는 Key Vault 리소스 ID입니다. |
| `ResourceProvider` |*MICROSOFT.KEYVAULT* |
| `ResourceType` | *VAULTS* |
| `ResultSignature` |HTTP 상태(예: *확인*) |
| `ResultType` |REST API 요청의 결과(예: *성공*) |
| `SubscriptionId` |Key Vault를 포함하는 구독의 Azure 구독 ID입니다. |

## <a name="migrating-from-the-old-key-vault-solution"></a>이전 Key Vault 솔루션에서 마이그레이션
2017년 1월 Key Vault에서 Log Analytics로 로그를 보내도록 지원하는 방법이 변경되었습니다. 이러한 변경은 다음과 같은 이점을 제공합니다.
+ 로그는 저장소 계정을 사용 하지 않고도 Log Analytics 작업 영역에 직접 기록 됩니다.
+ Log Analytics에서 사용할 수 있는 로그가 생성된 이후의 대기 시간 감소
+ 구성 단계 감소
+ 모든 유형의 Azure 진단을 위한 공통 형식

업데이트된 솔루션을 사용하려면 다음을 수행합니다.

1. [진단을 구성 하 여 Key Vault에서 Log Analytics 작업 영역으로 직접 전송](#enable-key-vault-diagnostics-in-the-portal)  
2. [솔루션 갤러리에서 Azure Monitor 솔루션 추가](../../azure-monitor/insights/solutions.md) 에 설명 된 프로세스를 사용 하 여 Azure Key Vault 솔루션을 사용 하도록 설정 합니다.
3. 새 데이터 형식을 사용하도록 저장된 쿼리, 대시보드 또는 경고를 업데이트합니다.
   + KeyVaults에서 AzureDiagnostics로 형식을 변경합니다. ResourceType을 사용하여 Key Vault 로그로 필터링할 수 있습니다.
   + `KeyVaults` 대신 `AzureDiagnostics | where ResourceType'=="VAULTS"`를 사용합니다.
   + 필드: (필드 이름은 대/소문자를 구분함)
   + 이름에 \_s, \_d 또는 \_g 접미사가 있는 필드의 경우 첫 번째 문자를 소문자로 변경합니다.
   + 이름에 \_o 접미사가 있는 모든 필드의 경우 데이터는 중첩된 필드 이름에 기반하여 개별 필드로 분할합니다. 예를 들어 호출자의 UPN은 `identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s` 필드에 저장됩니다.
   + CallerIpAddress 필드를 CallerIPAddress로 변경합니다.
   + RemoteIPCountry 필드는 더 이상 존재하지 않습니다.
4. *Key Vault 분석(사용되지 않음)* 솔루션을 제거합니다. PowerShell을 사용하는 경우 `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`를 사용합니다.

변경 전에 수집된 데이터는 새 솔루션에서 볼 수 없습니다. 이전 형식 및 필드 이름을 사용하여 이 데이터를 계속 쿼리할 수 있습니다.

## <a name="troubleshooting"></a>문제 해결
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>다음 단계
* [Azure Monitor의 로그 쿼리](../../azure-monitor/log-query/log-query-overview.md) 를 사용 하 여 자세한 Azure Key Vault 데이터를 볼 수 있습니다.
