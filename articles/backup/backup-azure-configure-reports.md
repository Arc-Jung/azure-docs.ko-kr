---
title: Azure Backup용 보고서 구성
description: Recovery Services 자격 증명 모음을 사용하여 Azure Backup용 Power BI 보고서를 구성하는 방법을 설명합니다.
ms.reviewer: adigan
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: dacurwin
ms.openlocfilehash: 757e12f97f95ef856643820e47113521d840f10d
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74074984"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup 보고서 구성

이 문서에서는 Recovery Services 자격 증명 모음을 사용하여 Azure Backup용 보고서를 구성하기 위해 수행해야 하는 단계를 설명합니다. 또한 Power BI를 사용하여 보고서에 액세스하는 방법도 설명합니다. 이러한 단계를 완료한 후에는 Power BI로 직접 이동하여 보고서를 확인, 사용자 지정 및 작성할 수 있습니다.

> [!IMPORTANT]
> 2018년 11월 1일부터 일부 고객은 Power BI의 Azure Backup 앱에서 데이터를 로드하는 과정에서 “JSON 입력의 끝에 추가 문자가 있습니다. IDataReader 인터페이스에서 예외가 발생했습니다.” 문제를 볼 수 있습니다.
이것은 데이터가 스토리지 계정에 로드되는 형식이 변경되었기 때문입니다.
이 문제를 방지하려면 최신 앱(버전 1.8)을 다운로드하세요.
>
>

## <a name="supported-scenarios"></a>지원되는 시나리오

- Azure Backup 보고서는 Azure Recovery Services 에이전트를 사용하여 클라우드로 Azure 가상 머신 백업 및 파일/폴더 백업을 전송하는 작업용으로 지원됩니다.
- Azure SQL Database, Azure File Shares, Data Protection Manager 및 Azure Backup Server용 보고서는 현재 지원되지 않습니다.
- 각 자격 증명 모음에 대해 동일한 스토리지 계정이 구성된 경우 여러 자격 증명 모음과 구독의 보고서를 볼 수 있습니다. 선택한 스토리지 계정은 Recovery Services 자격 증명 모음과 동일한 지역에 있어야 합니다.
- Power BI에서 보고서에 대해 예약된 새로 고침 빈도는 24시간입니다. Power BI에서 보고서의 주문형 새로 고침을 수행할 수도 있습니다. 이 경우 고객 스토리지 계정의 최신 데이터를 사용하여 보고서를 렌더링합니다.

## <a name="prerequisites"></a>선행 조건

- [Azure Storage 계정](../storage/common/storage-quickstart-create-account.md)을 만들어 보고서에 대해 구성합니다. 이 스토리지 계정은 보고서 관련 데이터를 저장하는 데 사용됩니다.
- [Power BI 계정을 만들어](https://powerbi.microsoft.com/landing/signin/) Power BI 포털을 사용해 보고서를 확인, 사용자 지정 및 생성합니다.
- 리소스 공급자 **Microsoft.insights**를 아직 등록하지 않은 경우 등록합니다. 보고 데이터가 스토리지 계정으로 전송될 수 있도록 Recovery Services 자격 증명 모음 및 스토리지 계정용 구독을 사용합니다. 이 단계를 수행하려면 Azure Portal로 이동하여 **구독** > **리소스 공급자**를 선택한 다음 이 공급자를 선택해서 등록합니다.

## <a name="configure-storage-account-for-reports"></a>보고서에 대한 스토리지 계정 구성

Azure Portal을 사용하여 Recovery Services 자격 증명 모음용 스토리지 계정을 구성하려면 다음 단계를 수행합니다. 이 구성은 한 번만 수행하면 됩니다. 저장소 계정이 구성 된 후 Power BI으로 직접 이동 하 여 템플릿 앱을 보고 보고서를 사용할 수 있습니다.

1. Recovery Services 자격 증명 모음이 이미 열려 있으면 다음 단계로 진행합니다. Recovery Services 자격 증명 모음이 열려 있지 않으면 Azure Portal에서 **모든 서비스**를 선택합니다.

   - 리소스 목록에서 **Recovery Services**를 입력합니다.
   - 입력을 시작하면 입력한 내용을 바탕으로 목록이 필터링됩니다. **Recovery Services 자격 증명 모음**이 표시되면 이를 선택합니다.
   - Recovery Services 자격 증명 모음의 목록이 표시됩니다. Recovery Services 자격 증명 모음의 목록에서 자격 증명 모음을 선택합니다.

     선택한 자격 증명 모음 대시보드가 열립니다.
2. 자격 증명 모음 아래에 나타나는 항목 목록에서 **모니터링 및 보고서** 섹션 아래의 **백업 보고서**를 선택하여 보고서용 스토리지 계정을 구성합니다.

      ![백업 보고서 메뉴 항목 선택 2단계](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. **백업 보고서** 블레이드에서 **진단 설정** 링크를 선택합니다. 이 링크를 선택하면 고객 스토리지 계정에 데이터를 푸시하는 데 사용되는 **진단 설정** UI가 열립니다.

      ![진단 사용 3단계](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. **진단 켜기**를 선택하여 스토리지 계정을 구성하는 데 사용되는 UI를 엽니다.

      ![진단 사용 4단계](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. **이름** 상자에 설정 이름을 입력합니다. 보고 데이터가 스토리지 계정으로 전송될 수 있도록 **스토리지 계정에 보관** 확인란을 선택합니다.

      ![진단 사용 5단계](./media/backup-azure-configure-reports/select-setting-name.png)
6. **스토리지 계정**을 선택하고 보고 데이터를 저장할 관련 구독 및 스토리지 계정을 목록에서 선택한 후에 **확인**을 선택합니다.

      ![스토리지 계정 선택 6단계](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. **로그** 섹션에서 **AzureBackupReport** 확인란을 선택합니다. 슬라이더를 이동하여 이 보고 데이터의 보존 기간을 선택합니다. 스토리지 계정의 보고 데이터는 이 슬라이더를 사용하여 선택한 기간 동안 유지됩니다.

      ![스토리지 계정 저장 7단계](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. 모든 변경 내용을 검토하고 **저장**을 선택합니다. 이 작업을 수행하면 모든 변경 내용이 저장됩니다. 이제 보고 데이터를 저장할 스토리지 계정이 구성되었습니다.

9. 이제 **진단 설정** 테이블에는 자격 증명 모음에 사용하도록 설정된 새 설정이 표시됩니다. 새 설정이 표시되지 않으면 테이블을 새로 고쳐 업데이트된 설정을 표시합니다.

      ![진단 설정 보기 9단계](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> 스토리지 계정을 저장하여 보고서를 구성한 후에는 초기 데이터 푸시가 완료될 때까지 *24시간 동안 대기*합니다. 이 시간이 지난 후에만 Power BI에서 Azure Backup 앱을 가져오세요. 자세한 내용은 [FAQ 섹션](backup-azure-monitor-alert-faq.md)을 참조하세요.
>
>

## <a name="view-reports-in-power-bi"></a>Power BI에서 보고서 보기

Recovery Services 자격 증명 모음을 사용하여 보고서용 스토리지 계정을 구성한 후 보고 데이터 전송이 시작될 때까지는 약 24시간 정도 걸립니다. 스토리지 계정을 설정하고 24시간 후에 Power BI에서 보고서를 보려면 다음 단계를 수행합니다.
보고서를 사용자 지정하고 공유하려면 작업 영역을 만들고 다음 단계를 수행하세요.

1. Power BI에 [로그인](https://powerbi.microsoft.com/landing/signin/)합니다.
2. 앱 > 이동 하 여 **Microsoft Appsource에서 더 많은 앱을 가져옵니다**. [서비스에 연결하는 방법에 대한 Power BI 설명서](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/)의 단계를 따릅니다.

3. **검색** 창에 **Azure Backup**을 입력하고 **지금 설치하기**를 선택합니다.

      ![템플릿 앱 가져오기](./media/backup-azure-configure-reports/template-app-get.png)
4. 위의 5단계에서 구성한 스토리지 계정의 이름을 입력하고 **다음**을 선택합니다.

    ![스토리지 계정 이름 입력](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)
5. 인증 방법 &quot;키&quot;를 사용하여 이 스토리지 계정의 스토리지 계정 키를 입력합니다. [스토리지 액세스 키를 확인하고 복사](../storage/common/storage-account-manage.md#access-keys)하려면 Azure Portal에서 스토리지 계정으로 이동합니다.

     ![스토리지 계정 입력](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>

6. **로그인**을 선택합니다. 정상적으로 로그인되면 데이터 가져오기 알림이 표시됩니다.

    ![콘텐츠 팩 가져오기](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>

    가져오기가 완료되면 **성공** 알림이 표시됩니다. 저장소 계정에 있는 데이터의 양이 클 경우 템플릿 앱을 가져오는 데 약간의 시간이 걸릴 수 있습니다.

    ![콘텐츠 팩 가져오기 성공](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>

7. 데이터 가져오기가 성공적으로 완료 되 면 탐색 창의 **앱** 에 **Azure Backup** 템플릿 앱이 표시 됩니다. 이제 **대시보드**, **보고서**, **데이터 세트** 아래의 목록에 Azure Backup이 표시됩니다.

8. **대시보드**에서 **Azure Backup**을 선택합니다. 그러면 고정 키 보고서 집합이 표시됩니다.

      ![Azure Backup 대시보드](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. 전체 보고서 집합을 보려면 대시보드에서 아무 보고서나 선택합니다.

      ![Azure Backup 작업 상태](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. 보고서의 각 탭을 선택하여 해당 영역의 보고서를 확인합니다.

      ![Azure Backup 보고서 탭](./media/backup-azure-configure-reports/reports-tab-view.png)

## <a name="troubleshooting-errors"></a>문제 해결 오류

| 오류 세부 정보 | 해결 방법 |
| --- | --- |
| 백업 보고서의 스토리지 계정을 설정한 후에도 **스토리지 계정**에 계속 **구성되지 않음**이 표시됩니다. | 스토리지 계정을 정상적으로 구성했다면 이 문제가 발생해도 보고 데이터는 전송됩니다. 이 문제를 해결하려면 Azure Portal로 이동하여 **모든 서비스** > **진단 설정** > **Recovery Services 자격 증명 모음** > **설정 편집**을 선택합니다. 이전에 구성한 설정을 삭제하고 동일한 블레이드에서 새 설정을 만듭니다. 이번에는 **이름** 상자에서 **서비스**를 선택합니다. 그러면 구성된 스토리지 계정이 표시됩니다. |
|Power BI에서 Azure Backup 템플릿 앱을 가져온 후 "404-컨테이너를 찾을 수 없습니다." 라는 오류 메시지가 나타납니다. | 앞에서 설명한 것처럼 Recovery Services 자격 증명 모음에서 보고서를 구성한 후 24시간 동안 기다려야 Power BI에서 해당 보고서가 올바르게 표시됩니다. 24시간이 지나기 전에 보고서에 액세스하려고 하면 유효한 보고서를 표시하는 데 필요한 전체 데이터가 아직 없으므로 이 오류 메시지가 나타납니다. |

## <a name="next-steps"></a>다음 단계

저장소 계정을 구성 하 고 Azure Backup 템플릿 앱을 가져온 후 다음 단계는 보고서를 사용자 지정 하 고 보고 데이터 모델을 사용 하 여 보고서를 만드는 것입니다. 자세한 내용은 다음 문서를 참조하세요.

- [Azure Backup 보고 데이터 모델 사용](backup-azure-reports-data-model.md)
- [Power BI에서 보고서 필터링](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
- [Power BI에서 보고서 만들기](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
