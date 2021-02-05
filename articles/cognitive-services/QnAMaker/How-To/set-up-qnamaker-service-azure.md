---
title: QnA Maker 서비스 설정-QnA Maker
description: QnA Maker 기술 자료를 만들려면 먼저 Azure에서 QnA Maker 서비스를 설정해야 합니다. 구독에 새 리소스를 만들 수 있는 권한이 있으면 누구든지 QnA Maker 서비스를 설정할 수 있습니다.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: beb45d0d650b07f6106a3307d2d3a955095ee8b1
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99592265"
---
# <a name="manage-qna-maker-resources"></a>QnA Maker 리소스 관리

QnA Maker 기술 자료를 만들려면 먼저 Azure에서 QnA Maker 서비스를 설정해야 합니다. 구독에 새 리소스를 만들 수 있는 권한이 있으면 누구든지 QnA Maker 서비스를 설정할 수 있습니다.

다음 개념에 대 한 확실 한 이해는 리소스를 만들기 전에 유용 합니다.

* [QnA Maker 리소스](../Concepts/azure-resources.md)
* [키 작성 및 게시](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>새 QnA Maker 서비스 만들기

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker 일반 공급(안정적인 릴리스)](#tab/v1)

이 절차는 기술 자료 콘텐츠를 관리 하는 데 필요한 Azure 리소스를 만듭니다. 이러한 단계를 완료 한 후에는 Azure Portal 리소스의 **키** 페이지에서 _구독_ 키를 찾을 수 있습니다.

1. Azure Portal에 로그인 하 여 QnA Maker 리소스를 [만듭니다](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. 사용 약관을 읽은 후 **만들기** 를 선택 합니다.

    ![새 QnA Maker 서비스 만들기](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **QnA Maker** 에서 적절 한 계층 및 지역을 선택 합니다.

    ![새 QnA Maker 서비스 만들기 - 가격 책정 계층 및 지역](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * **이름** 필드에이 QnA Maker 서비스를 식별 하는 고유한 이름을 입력 합니다. 또한이 이름은 기술 자료가 연결 될 QnA Maker 끝점을 식별 합니다.
    * QnA Maker 리소스를 배포할 **구독** 을 선택 합니다.
    * QnA Maker 관리 서비스 (포털 및 관리 Api)에 대 한 **가격 책정 계층** 을 선택 합니다. [SKU 가격 책정에 대 한 자세한 내용을](https://aka.ms/qnamaker-pricing)참조 하세요.
    * 새 리소스 그룹을 만들거나 (권장)이 QnA Maker 리소스를 배포할 기존 **리소스 그룹** 을 사용 합니다. QnA Maker는 여러 Azure 리소스를 만듭니다. 이러한 리소스를 보유 하는 리소스 그룹을 만들 때 리소스 그룹 이름으로 이러한 리소스를 쉽게 찾고 관리 하 고 삭제할 수 있습니다.
    * **리소스 그룹 위치** 를 선택 합니다.
    * Azure Cognitive Search 서비스의 **검색 가격 책정 계층** 을 선택 합니다. 무료 계층 옵션을 사용할 수 없는 경우 (흐리게 표시 됨) 이미 구독을 통해 배포 된 무료 서비스가 있음을 의미 합니다. 이 경우 기본 계층으로 시작 해야 합니다. [Azure Cognitive Search 가격 정보](https://azure.microsoft.com/pricing/details/search/)를 참조 하세요.
    * Azure Cognitive Search 인덱스를 배포할 **검색 위치** 를 선택 합니다. 고객 데이터를 저장 해야 하는 위치에 대 한 제한 사항은 Azure Cognitive Search에 대해 선택한 위치를 결정 하는 데 도움이 됩니다.
    * **앱 이름** 필드에 Azure App Service 인스턴스의 이름을 입력 합니다.
    * 기본적으로 App Service 기본값은 표준 (S1) 계층입니다. 계획을 만든 후에 변경할 수 있습니다. [App Service 가격 책정](https://azure.microsoft.com/pricing/details/app-service/)에 대해 자세히 알아보세요.
    * App Service를 배포할 **웹 사이트 위치** 를 선택 합니다.

        > [!NOTE]
        > **검색 위치** 는 **웹 사이트 위치** 와 다를 수 있습니다.

    * **Application Insights** 사용 하도록 설정할지 여부를 선택 합니다. **Application Insights** 를 사용하도록 설정하면 QnA Maker는 트래픽, 채팅 로그 및 오류에 대한 원격 분석을 수집합니다.
    * Application Insights 리소스를 배포할 **앱 정보 위치** 를 선택 합니다.
    * 비용 절감 측정값의 경우, QnA Maker용으로 생성된 일부(전체는 아님) Azure 리소스를 [공유](#configure-qna-maker-to-use-different-cognitive-search-resource)할 수 있습니다.

1. 모든 필드의 유효성을 검사 한 후 **만들기** 를 선택 합니다. 이 프로세스를 완료하는 데 몇 분이 걸릴 수 있습니다.

1. 배포가 완료 되 면 구독에 다음과 같은 리소스가 생성 됩니다.

   ![QnA Maker 서비스를 새로 만든 리소스](../media/qnamaker-how-to-setup-service/resources-created.png)

    _Cognitive Services_ 형식의 리소스에는 _구독_ 키가 있습니다.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker 관리형(미리 보기 릴리스)](#tab/v2)

이 절차는 기술 자료 콘텐츠를 관리 하는 데 필요한 Azure 리소스를 만듭니다. 이러한 단계를 완료 한 후에는 Azure Portal 리소스의 **키** 페이지에서 *구독* 키를 찾을 수 있습니다.

1. Azure Portal에 로그인 하 여 QnA Maker 리소스를 [만듭니다](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) .

1. 사용 약관을 읽은 후 **만들기** 를 선택 합니다.

    ![새 QnA Maker 서비스 만들기](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. **QnA Maker** 에서 관리 (미리 보기) 확인란을 선택 하 고 적절 한 계층 및 지역을 선택 합니다.

    ![새 QnA Maker 관리 서비스-가격 책정 계층 및 지역 만들기](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png)

    * QnA Maker 리소스를 배포할 **구독** 을 선택 합니다.
    * 새 리소스 그룹을 만들거나 (권장)이 QnA Maker 관리 (미리 보기) 리소스를 배포할 기존 **리소스 그룹** 을 사용 합니다. QnA Maker 관리 (미리 보기)는 몇 가지 Azure 리소스를 만듭니다. 이러한 리소스를 보유 하는 리소스 그룹을 만들 때 리소스 그룹 이름으로 이러한 리소스를 쉽게 찾고 관리 하 고 삭제할 수 있습니다.
    * **이름** 필드에이 QnA Maker 관리 (미리 보기) 서비스를 식별 하는 고유한 이름을 입력 합니다. 
    * QnA Maker 관리 (미리 보기) 서비스를 배포할 **위치** 를 선택 합니다. 관리 Api 및 서비스 끝점은이 위치에서 호스팅됩니다. 
    * QnA Maker 관리 (미리 보기) 서비스의 **가격 책정 계층** 을 선택 합니다 (미리 보기에 사용 가능). [SKU 가격 책정에 대 한 자세한 내용을](https://aka.ms/qnamaker-pricing)참조 하세요.
    * Azure Cognitive Search 인덱스를 배포할 **검색 위치** 를 선택 합니다. 고객 데이터를 저장 해야 하는 위치에 대 한 제한 사항은 Azure Cognitive Search에 대해 선택한 위치를 결정 하는 데 도움이 됩니다.
    * Azure Cognitive Search 서비스의 **검색 가격 책정 계층** 을 선택 합니다. 무료 계층 옵션을 사용할 수 없는 경우 (흐리게 표시 됨) 이미 구독을 통해 배포 된 무료 서비스가 있음을 의미 합니다. 이 경우 기본 계층으로 시작 해야 합니다. [Azure Cognitive Search 가격 정보](https://azure.microsoft.com/pricing/details/search/)를 참조 하세요.

1. 모든 필드의 유효성을 검사 한 후 **검토 + 만들기** 를 선택 합니다. 이 프로세스를 완료하는 데 몇 분이 걸릴 수 있습니다.

1. 배포가 완료 되 면 구독에 다음과 같은 리소스가 생성 됩니다.

    ![리소스가 새로운 QnA Maker 관리 (미리 보기) 서비스를 만듦](../media/qnamaker-how-to-setup-service/resources-created-v2.png)

    _Cognitive Services_ 형식의 리소스에는 _구독_ 키가 있습니다.
    
---

## <a name="recommended-settings-for-network-isolation"></a>네트워크 격리에 대 한 권장 설정

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker 일반 공급(안정적인 릴리스)](#tab/v1)

1. [가상 네트워크를 구성](../../cognitive-services-virtual-networks.md?tabs=portal)하 여 공용 액세스에서 인지 서비스 리소스를 보호 합니다.
2. 공용 액세스에서 App Service (QnA Runtime)를 보호 합니다.

   ##### <a name="add-ips-to-app-service-allowlist"></a>App Service allowlist에 Ip 추가

    * 인지 서비스 Ip의 트래픽만 허용 합니다. 이러한 설정은 서비스 태그에 이미 포함 되어 `CognitiveServicesManagement` 있습니다. 이는 app service를 호출 하 고 Azure Search 서비스를 업데이트 하는 Api (만들기/업데이트 KB)를 작성 하는 데 필요 합니다. [서비스 태그에 대 한 자세한 정보를](../../../virtual-network/service-tags-overview.md) 확인 하세요.
    * 또한 Bot service, QnA Maker portal (corpnet 일 수 있음) 등의 다른 진입점을 허용 하 고, 예측 "GenerateAnswer" API 액세스를 위한 것입니다.
    * IP 주소 범위를 allowlist에 추가 하려면 다음 단계를 수행 하세요.

      * [모든 서비스 태그의 IP 범위를](https://www.microsoft.com/download/details.aspx?id=56519)다운로드 합니다.
      * "CognitiveServicesManagement"의 Ip를 선택 합니다.
      * App Service 리소스의 네트워킹 섹션으로 이동 하 고 "액세스 제한 구성" 옵션을 클릭 하 여 allowlist에 Ip를 추가 합니다.

    또한 App Service에 대해 동일한 작업을 수행 하는 자동화 된 스크립트가 있습니다. GitHub에서 allowlist을 [구성 하는 PowerShell 스크립트](https://github.com/pchoudhari/QnAMakerBackupRestore/blob/master/AddRestrictedIPAzureAppService.ps1) 를 찾을 수 있습니다. 구독 id, 리소스 그룹 및 실제 App Service 이름을 스크립트 매개 변수로 입력 해야 합니다. 스크립트를 실행 하면 App Service allowlist에 Ip가 자동으로 추가 됩니다.

    ##### <a name="configure-app-service-environment-to-host-qna-maker-app-service"></a>QnA Maker를 호스트 App Service Environment 구성 App Service
    ASE (App Service Environment)를 사용 하 여 QnA Maker App Service를 호스트할 수 있습니다. 다음 단계를 따르세요.

    1. App Service Environment 만들고 "external"으로 표시 합니다. 지침은 [자습서](../../../app-service/environment/create-external-ase.md) 를 따르세요.
    2.  App Service Environment 내에서 App service를 만듭니다.
        * App service에 대 한 구성을 확인 하 고 응용 프로그램 설정으로 ' PrimaryEndpointKey '를 추가 합니다. ' PrimaryEndpointKey '의 값을 " \<app-name\> -primaryendpointkey"로 설정 해야 합니다. 앱 이름은 App service URL에 정의 됩니다. 예를 들어 App service URL이 "mywebsite.myase.p.azurewebsite.net" 인 경우 응용 프로그램 이름은 "mywebsite"입니다. 이 경우 ' PrimaryEndpointKey '의 값을 "mywebsite-PrimaryEndpointKey"로 설정 해야 합니다.
        * Azure search 서비스를 만듭니다.
        * Azure Search 및 앱 설정이 적절히 구성 되어 있는지 확인 합니다. 
          이 [자습서](../reference-app-service.md?tabs=v1#app-service)를 수행 하세요.
    3.  App Service Environment 연결 된 네트워크 보안 그룹을 업데이트 합니다.
        * 요구 사항에 따라 미리 만든 인바운드 보안 규칙을 업데이트 합니다.
        * 소스를 ' Service Tag '로, 소스 서비스 태그를 ' CognitiveServicesManagement '로 사용 하 여 새 인바운드 보안 규칙을 추가 합니다.
    4.  QnA Maker 끝점을 위에서 만든 App Service 끝점 (https://mywebsite.myase.p.azurewebsite.net)으로 설정 해야 하는 Azure Resource Manager를 사용 하 여 QnA Maker 인식 서비스 인스턴스 (Cognitiveservices account/accounts)를 만듭니다.
    
3. VNET 내의 개인 끝점으로 Cognitive Search 구성

    QnA Maker 리소스를 만드는 동안 검색 인스턴스가 생성 되는 경우 Cognitive Search을 강제 적용 하 여 고객의 VNet 내에서 완전히 생성 된 개인 끝점 구성을 지원할 수 있습니다.

    모든 리소스를 동일한 지역에 만들어 개인 끝점을 사용 해야 합니다.

    * QnA Maker 리소스
    * 새 Cognitive Search 리소스
    * 새 Virtual Network 리소스

    [Azure Portal](https://portal.azure.com)에서 다음 단계를 완료 합니다.

    1. [QnA Maker 리소스](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)를 만듭니다.
    1. 끝점 연결 (데이터)을 _개인_ 으로 설정 하 여 새 Cognitive Search 리소스를 만듭니다. 1 단계에서 만든 QnA Maker 리소스와 동일한 지역에 리소스를 만듭니다. [Cognitive Search 리소스를 만드는](../../../search/search-create-service-portal.md)방법에 대해 자세히 알아보고이 링크를 사용 하 여 [리소스의 만들기 페이지로](https://ms.portal.azure.com/#create/Microsoft.Search)직접 이동 합니다.
    1. 새 [Virtual Network 리소스](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)를 만듭니다.
    1. 이 절차의 1 단계에서 만든 App service 리소스에서 VNET을 구성 합니다.
        1. 2 단계에서 만든 새 Cognitive Search 리소스에 대해 VNET에 새 DNS 항목을 만듭니다. Cognitive Search IP 주소입니다.
    1. 2 단계에서 만든 [새 Cognitive Search 리소스에 App service를 연결](#configure-qna-maker-to-use-different-cognitive-search-resource) 합니다. 그런 다음 1 단계에서 만든 원래 Cognitive Search 리소스를 삭제할 수 있습니다.

    [QnA Maker 포털](https://www.qnamaker.ai/)에서 첫 번째 기술 자료를 만듭니다.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker 관리형(미리 보기 릴리스)](#tab/v2)

1. [가상 네트워크를 구성](../../cognitive-services-virtual-networks.md?tabs=portal)하 여 공용 액세스에서 인지 서비스 리소스를 보호 합니다.
2. Azure Search 리소스에 대 한 [개인 끝점을 만듭니다](../reference-private-endpoint.md) .

---

## <a name="upgrade-azure-resources"></a>Azure 리소스 업그레이드

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker 일반 공급(안정적인 릴리스)](#tab/v1)

### <a name="upgrade-qna-maker-sku"></a>QnA Maker SKU 업그레이드

현재 계층을 벗어나 기술 자료에서 더 많은 질문과 대답을 원하는 경우 QnA Maker 서비스 가격 책정 계층을 업그레이드 하세요.

QnA Maker 관리 SKU를 업그레이드하려면

1. Azure Portal의 QnA Maker 리소스로 이동한 다음, **가격 책정 계층** 을 선택합니다.

    ![QnA Maker 리소스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. 적절한 SKU를 선택하고 **선택** 을 누릅니다.

    ![QnA Maker 가격 책정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)
    
### <a name="upgrade-app-service"></a>업그레이드 App Service

기술 자료가 클라이언트 앱에서 더 많은 요청을 처리 해야 하는 경우 App Service 가격 책정 계층을 업그레이드 합니다.

App Service를 [강화 하거나 규모](../../../app-service/manage-scale-up.md) 를 확장할 수 있습니다.

Azure Portal에서 App Service 리소스로 이동 하 고 필요에 따라 **수직 확장** 또는 **수평 확장** 옵션을 선택 합니다.

![QnA Maker App Service 크기 조정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Cognitive Search 서비스 업그레이드

많은 기술 자료를 포함할 계획인 경우 Azure Cognitive Search 서비스 가격 책정 계층을 업그레이드하세요.

현재 Azure search SKU의 전체 업그레이드를 수행할 수 없습니다. 그러나 원하는 SKU로 새 Azure Search 리소스를 만들고, 데이터를 새 리소스로 복원한 다음, QnA Maker 스택에 연결할 수 있습니다. 이렇게 하려면 다음 단계를 수행하세요.

1. Azure Portal에서 새 Azure search 리소스를 만들고 원하는 SKU를 선택 합니다.

    ![QnA Maker Azure Search 리소스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. 원래 Azure Search 리소스의 인덱스를 새 리소스로 복원합니다. [백업 복원 샘플 코드](https://github.com/pchoudhari/QnAMakerBackupRestore)를 참조하세요.

1. 데이터가 복원 된 후에는 새 Azure search 리소스로 이동 하 고, **키** 를 선택 하 고, **이름** 및 **관리자 키** 를 적어둡니다.

    ![QnA Maker Azure Search 키](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. 새 Azure search 리소스를 QnA Maker 스택에 연결 하려면 QnA Maker App Service 인스턴스로 이동 합니다.

    ![QnA Maker App Service 인스턴스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. **애플리케이션 설정** 을 선택하고 3단계의 **AzureSearchName** 및 **AzureSearchAdminKey** 필드의 설정을 수정합니다.

    ![QnA Maker App Service 설정](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. App Service 인스턴스를 다시 시작합니다.

    ![QnA Maker App Service 인스턴스 다시 시작](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)
    
### <a name="inactivity-policy-for-free-search-resources"></a>무료 검색 리소스에 대 한 비활성 정책

QnA maker 리소스를 사용 하지 않는 경우 모든 리소스를 제거 해야 합니다. 사용 하지 않는 리소스를 제거 하지 않으면 무료 검색 리소스를 만든 경우 기술 자료가 작동 하지 않습니다.

무료 검색 리소스는 API 호출을 받지 않고 90 일 후에 삭제 됩니다.
    
# <a name="qna-maker-managed-preview-release"></a>[QnA Maker 관리형(미리 보기 릴리스)](#tab/v2)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Azure Cognitive Search 서비스 업그레이드

많은 기술 자료를 포함할 계획인 경우 Azure Cognitive Search 서비스 가격 책정 계층을 업그레이드하세요.

현재 Azure search SKU의 전체 업그레이드를 수행할 수 없습니다. 그러나 원하는 SKU로 새 Azure Search 리소스를 만들고, 데이터를 새 리소스로 복원한 다음, QnA Maker 스택에 연결할 수 있습니다. 이렇게 하려면 다음 단계를 수행하세요.

1. Azure Portal에서 새 Azure search 리소스를 만들고 원하는 SKU를 선택 합니다.

    ![QnA Maker Azure Search 리소스](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. 원래 Azure Search 리소스의 인덱스를 새 리소스로 복원합니다. [백업 복원 샘플 코드](https://github.com/pchoudhari/QnAMakerBackupRestore)를 참조하세요.

1. 새 Azure search 리소스를 QnA Maker 관리 (미리 보기) 서비스에 연결 하려면 아래 항목을 참조 하세요.

### <a name="inactivity-policy-for-free-search-resources"></a>무료 검색 리소스에 대 한 비활성 정책

QnA maker 리소스를 사용 하지 않는 경우 모든 리소스를 제거 해야 합니다. 사용 하지 않는 리소스를 제거 하지 않으면 무료 검색 리소스를 만든 경우 기술 자료가 작동 하지 않습니다.

무료 검색 리소스는 API 호출을 받지 않고 90 일 후에 삭제 됩니다.

---

## <a name="configure-azure-resources"></a>Azure 리소스 구성

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker 일반 공급(안정적인 릴리스)](#tab/v1)

### <a name="configure-qna-maker-to-use-different-cognitive-search-resource"></a>다른 Cognitive Search 리소스를 사용 하도록 QnA Maker 구성

포털을 통해 QnA 서비스 및 해당 종속성 (예: 검색)을 만드는 경우 검색 서비스가 만들어지고 QnA Maker 서비스에 연결 됩니다. 이러한 리소스를 만든 후 이전에 기존 검색 서비스를 사용 하도록 App Service 설정을 업데이트 하 고 방금 만든 검색 서비스를 제거할 수 있습니다.

QnA Maker의 **App Service** 리소스는 Cognitive Search 리소스를 사용 합니다. QnA Maker에서 사용 하는 Cognitive Search 리소스를 변경 하려면 Azure Portal에서 설정을 변경 해야 합니다.

1. 사용할 QnA Maker Cognitive Search 리소스의 **관리자 키** 와 **이름을** 가져옵니다.

1. [Azure Portal](https://portal.azure.com) 에 로그인 하 고 QnA Maker 리소스와 연결 된 **App Service** 를 찾습니다. 둘 다 동일한 이름을 갖습니다.

1. **설정**, **구성** 을 차례로 선택 합니다. 그러면 QnA Maker의 App Service에 대 한 모든 기존 설정이 표시 됩니다.

    > [!div class="mx-imgBorder"]
    > ![App Service 구성 설정을 보여 주는 Azure Portal의 스크린샷](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-app-service-configuration.png)

1. 다음 키에 대 한 값을 변경 합니다.

    * **AzureSearchAdminKey**
    * **AzureSearchName**

1. 새 설정을 사용 하려면 App service를 다시 시작 해야 합니다. **개요** 를 선택한 다음 **다시 시작** 을 선택 합니다.

    > [!div class="mx-imgBorder"]
    > ![구성 설정이 변경 된 후 App Service를 다시 시작 하 Azure Portal 스크린샷](../media/qnamaker-how-to-upgrade-qnamaker/screenshot-azure-portal-restart-app-service.png)

Azure Resource Manager 템플릿을 통해 QnA 서비스를 만드는 경우 모든 리소스를 만들고 기존 검색 서비스를 사용 하는 App Service 만들기를 제어할 수 있습니다.

App Service [응용 프로그램 설정을](../../../app-service/configure-common.md#configure-app-settings)구성 하는 방법에 대해 자세히 알아보세요.

### <a name="get-the-latest-runtime-updates"></a>최신 런타임 업데이트 가져오기

QnAMaker 런타임은 Azure Portal에서 [QnAMaker 서비스를 만들](./set-up-qnamaker-service-azure.md) 때 배포 되는 Azure App Service 인스턴스의 일부입니다. 런타임은 주기적으로 업데이트됩니다. QnA Maker App Service 인스턴스는 4 월 2019 사이트 확장 릴리스 (버전 5 이상) 이후 자동 업데이트 모드에 있습니다. 이 업데이트는 업그레이드 하는 동안 가동 중지 시간을 0으로 처리 하도록 설계 되었습니다.

에서 현재 버전을 확인할 수 있습니다 https://www.qnamaker.ai/UserSettings . 버전이 버전 5.x 보다 이전 버전인 경우 App Service를 다시 시작 하 여 최신 업데이트를 적용 해야 합니다.

1. [Azure Portal](https://portal.azure.com)에서 QnAMaker 서비스 (리소스 그룹)로 이동 합니다.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker Azure 리소스 그룹](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

1. App Service 인스턴스를 선택 하 고 **개요** 섹션을 엽니다.

    > [!div class="mx-imgBorder"]
    > ![QnAMaker App Service 인스턴스](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)


1. App Service를 다시 시작 합니다. 업데이트 프로세스는 몇 초 내에 완료 되어야 합니다. 이 QnAMaker 서비스를 사용 하는 모든 종속 응용 프로그램 또는 봇은이 재시작 기간 동안 최종 사용자가 사용할 수 없습니다.

    ![QnAMaker App Service 인스턴스 다시 시작](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

### <a name="configure-app-service-idle-setting-to-avoid-timeout"></a>시간 초과를 방지 하도록 App service 유휴 설정 구성

게시 된 기술 자료에 대 한 QnA Maker 예측 런타임에 사용 되는 app service는 유휴 시간 제한 구성을 포함 하며,이는 서비스가 유휴 상태 이면 기본적으로 시간 초과 됩니다. QnA Maker의 경우,이는 트래픽이 없는 기간 후에도 예측 런타임 generateAnswer API가 때때로 시간 초과 되는 것을 의미 합니다.

트래픽이 없는 경우에도 예측 끝점 앱이 로드 되도록 하려면 유휴 상태를 always on으로 설정 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. QnA Maker 리소스의 app service를 검색 하 고 선택 합니다. QnA Maker 리소스와 동일한 이름을 갖지만 다른 **유형의** App Service를 갖게 됩니다.
1. **설정을** 찾은 후 **구성** 을 선택 합니다.
1. 구성 창에서 **일반 설정** 을 선택 하 고 **Always on** 을 찾은 다음 값을 **선택 합니다** .

    > [!div class="mx-imgBorder"]
    > ![구성 창에서 * * 일반 설정 * *을 선택 하 고 * * Always on * *을 찾은 다음 값으로 * *를 선택 합니다.](../media/qnamaker-how-to-upgrade-qnamaker/configure-app-service-idle-timeout.png)

1. **저장** 을 선택 하 여 구성을 저장 합니다.
1. 새 설정을 사용 하도록 앱을 다시 시작할지 묻는 메시지가 표시 됩니다. **계속** 을 선택합니다.

App Service [일반 설정을](../../../app-service/configure-common.md#configure-general-settings)구성 하는 방법에 대해 자세히 알아보세요.

### <a name="business-continuity-with-traffic-manager"></a>Traffic manager를 통한 비즈니스 연속성

비즈니스 연속성 계획의 주요 목표는 Bot 또는 이를 사용하는 애플리케이션에 대한 작동 중지 시간이 없도록 보장하는 복원력 있는 기술 자료 엔드포인트를 만드는 것입니다.

> [!div class="mx-imgBorder"]
> ![QnA Maker bcp 계획](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

위에 표시된 것처럼 대략적인 아이디어는 다음과 같습니다.

1. [Azure 쌍을 이루는 지역](../../../best-practices-availability-paired-regions.md)에서 두 개의 병렬 [QnA Maker 서비스](set-up-qnamaker-service-azure.md)를 설정합니다.

1. 기본 QnA Maker App service를 [백업](../../../app-service/manage-backup.md) 하 고 보조 설정에서 [복원](../../../app-service/web-sites-restore.md) 합니다. 이렇게 하면 두 설정이 동일한 호스트 이름 및 키로 작동 합니다.

1. 기본 및 보조 Azure 검색 인덱스를 동기화 된 상태로 유지 합니다. Azure 인덱스를 백업 하는 방법을 보려면 [여기](https://github.com/pchoudhari/QnAMakerBackupRestore) 에서 GitHub 샘플을 사용 하세요.

1. [연속 내보내기](../../../azure-monitor/app/export-telemetry.md)를 사용하여 Application Insights를 백업합니다.

1. 기본 및 보조 스택이 설치되면 [트래픽 관리자](../../../traffic-manager/traffic-manager-overview.md)를 사용하여 두 개의 엔드포인트를 구성하고 라우팅 메서드를 설정합니다.

1. Traffic manager 끝점에 대해 이전에는 TLS (전송 계층 보안), 이전에는 SSL (SSL(Secure Sockets Layer)) 인증서를 만들어야 합니다. App service에서 [TLS/SSL 인증서를 바인딩합니다](../../../app-service/configure-ssl-bindings.md) .

1. 마지막으로, 봇 또는 앱에서 트래픽 관리자 엔드포인트를 사용합니다.

# <a name="qna-maker-managed-preview-release"></a>[QnA Maker 관리형(미리 보기 릴리스)](#tab/v2)

### <a name="configure-qna-maker-managed-preview-service-to-use-different-cognitive-search-resource"></a>다른 Cognitive Search 리소스를 사용 하도록 QnA Maker 관리 (미리 보기) 서비스 구성

포털을 통해 관리 되는 QnA 서비스 (미리 보기) 및 해당 종속성 (예: 검색)을 만드는 경우 검색 서비스가 만들어지고 QnA Maker 관리 (미리 보기) 서비스에 연결 됩니다. 이러한 리소스를 만든 후 **구성** 탭에서 검색 서비스를 업데이트할 수 있습니다.

1. Azure Portal에서 QnA Maker 관리 (미리 보기) 서비스로 이동 합니다.

1. **구성** 을 선택 하 고 QnA Maker 관리 (미리 보기) 서비스와 연결 하려는 Azure Cognitive Search 서비스를 선택 합니다.

    ![QnA Maker 관리 (미리 보기) 구성 페이지의 스크린샷](../media/qnamaker-how-to-upgrade-qnamaker/change-search-service-configuration.png)

1. **저장** 을 클릭합니다.

> [!NOTE]
> QnA Maker와 연결 된 Azure Search 서비스를 변경 하면 이미 있는 모든 기술 자료에 대 한 액세스 권한이 손실 됩니다. Azure Search 서비스를 변경 하기 전에 기존 기술 자료를 내보내야 합니다.

---

## <a name="delete-azure-resources"></a>Azure 리소스 삭제

QnA Maker 기술 자료에 사용되는 Azure 리소스를 삭제하는 경우 기술 자료는 더 이상 작동하지 않습니다. 모든 리소스를 삭제하기 전에 **설정** 페이지에서 기술 자료를 내보냈는지 확인합니다.

## <a name="next-steps"></a>다음 단계

[App service](../../../app-service/index.yml) 및 [Search 서비스](../../../search/index.yml)에 대해 자세히 알아보세요.

> [!div class="nextstepaction"]
> [다른 사용자와 작성 하는 방법 알아보기](../index.yml)
