---
title: Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법
description: 지원 요청을 제출 하 여 Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법을 알아봅니다. 기존 Azure VMware 솔루션 사설 클라우드에서 더 많은 호스트를 요청할 수도 있습니다.
ms.topic: how-to
ms.date: 11/12/2020
ms.openlocfilehash: 6d614dffc4ab3127e1e6740b1a8773e5fd7c23ff
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97630890"
---
# <a name="how-to-enable-azure-vmware-solution-resource"></a>Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법
지원 요청을 제출 하 여 [Azure VMware 솔루션](introduction.md) 리소스를 사용 하도록 설정 하는 방법을 알아봅니다. 기존 Azure VMware 솔루션 사설 클라우드에서 더 많은 호스트를 요청할 수도 있습니다.

## <a name="eligibility-criteria"></a>자격 기준

Azure 구독의 Azure 계정이 필요합니다. Azure 구독은 다음 조건 중 하나를 준수 해야 합니다.

* Microsoft에서 [Azure 기업계약 (EA)](../cost-management-billing/manage/ea-portal-agreements.md) 를 사용 하는 구독입니다.
* Azure에서 제공 하는 기존 CSP의 CSP (클라우드 솔루션 공급자) 관리 구독은 계약 또는 Azure 계획을 제공 합니다.


## <a name="enable-azure-vmware-solution-for-ea-customers"></a>EA 고객용 Azure VMware 솔루션 사용
Azure VMware 솔루션 리소스를 만들기 전에 호스트를 할당 하기 위해 지원 티켓을 제출 해야 합니다. 지원 팀에서 요청을 받으면 요청을 확인하고 호스트를 할당하는 데 최대 5일(영업일 기준)이 걸립니다. 기존 Azure VMware Solution 프라이빗 클라우드가 있고 더 많은 호스트를 할당하려는 경우에도 동일한 프로세스를 진행합니다.


1. Azure Portal의 **도움말 + 지원** 에서 **[새 지원 요청](https://rc.portal.azure.com/#create/Microsoft.Support)** 을 만들고 티켓에 대 한 다음 정보를 제공 합니다.
   - **문제 유형:** 기술
   - **구독:** 구독 선택
   - **서비스:** Azure VMware 솔루션 > 모든 서비스
   - **리소스:** 일반 질문 
   - **요약:** 용량 필요
   - **문제 유형:** 용량 관리 문제
   - **문제 하위 유형:** 호스트 할당량/용량을 추가하기 위한 고객 요청

1. 지원 티켓에 대 한 **설명** 의 **세부 정보** 탭에서 다음을 입력 합니다.

   - POC 또는 프로덕션 
   - 지역 이름
   - 호스트 수
   - 기타 세부 정보

   >[!NOTE]
   >Azure VMware 솔루션은 사설 클라우드를 실행 하 고 중복성 N + 1 호스트에 대해 최소 세 개의 호스트를 권장 합니다. 

1. **검토 + 만들기** 를 선택 하 여 요청을 제출 합니다.

   지원 담당자가 요청을 확인 하는 데 최대 5 영업일 소요 됩니다.

   >[!IMPORTANT] 
   >기존 Azure VMware 솔루션이 이미 있고 추가 호스트를 요청 하는 경우 5 영업일 동안 호스트를 할당 해야 합니다. 

1. 호스트를 프로 비전 하기 전에 Azure Portal에서 **MICROSOFT AVS** 리소스 공급자를 등록 했는지 확인 합니다.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   리소스 공급자를 등록하는 추가 방법은 [리소스 공급자 및 유형](../azure-resource-manager/management/resource-providers-and-types.md)을 참조하세요.

## <a name="enable-azure-vmware-solution-for-csp-customers"></a>CSP 고객을 위해 Azure VMware 솔루션 사용 

Csp는 [Microsoft 파트너 센터](https://partner.microsoft.com) 를 사용 하 여 고객에 대해 Azure VMware 솔루션을 사용 하도록 설정 해야 합니다. 이 문서에서는 파트너에 대 한 구매 절차를 보여 주기 위해 [CSP Azure 요금제](/partner-center/azure-plan-lp) 를 예로 사용 합니다.

   >[!IMPORTANT] 
   >Azure VMware 솔루션 서비스는 다중 테 넌 트를 제공 하지 않습니다. 이를 필요로 하는 호스팅 파트너는 지원 되지 않습니다. 

1. **파트너 센터** 에서 **CSP** 를 선택 하 여 **고객** 영역에 액세스 합니다.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-customers-screen.png" alt-text="Microsoft 파트너 센터 고객 영역" lightbox="media/enable-azure-vmware-solution/csp-customers-screen.png":::

1. 고객을 선택 하 고 **제품 추가** 를 선택 합니다.

   :::image type="content" source="media/enable-azure-vmware-solution/csp-partner-center.png" alt-text="Microsoft 파트너 센터" lightbox="media/enable-azure-vmware-solution/csp-partner-center.png":::

1. **Azure 계획** 을 선택한 다음 **카트에 추가를** 선택 합니다. 

1. 고객에 대 한 Azure 계획 구독의 일반적인 설정을 검토 하 고 완료 합니다. 자세한 내용은 [Microsoft 파트너 센터 설명서](/partner-center/azure-plan-manage)를 참조 하세요.

Azure 계획을 구성 하 고 필요한 [AZURE RBAC 권한이](/partner-center/azure-plan-manage) 구독에 대해 설정 된 후에 azure 계획 구독에 대 한 할당량을 사용 하도록 설정 하는 Microsoft에 참여 합니다. (Aobo) 프로시저를 **대신** 하 여 관리자를 사용 하 여 [Microsoft 파트너 센터](https://partner.microsoft.com) 에서 Azure Portal에 액세스 합니다.

1. [파트너 센터](https://partner.microsoft.com)에 로그인합니다.

1. **CSP** 를 선택 하 여 **고객** 영역에 액세스 합니다.

1. 고객 세부 정보를 확장 하 고 **Microsoft Azure 관리 포털** 를 선택 합니다.

1. Azure Portal의 **도움말 + 지원** 에서 **[새 지원 요청](https://rc.portal.azure.com/#create/Microsoft.Support)** 을 만들고 티켓에 대 한 다음 정보를 제공 합니다.
   - **문제 유형:** 기술
   - **구독:** 구독 선택
   - **서비스:** Azure VMware 솔루션 > 모든 서비스
   - **리소스:** 일반 질문 
   - **요약:** 용량 필요
   - **문제 유형:** 용량 관리 문제
   - **문제 하위 유형:** 호스트 할당량/용량을 추가하기 위한 고객 요청

1. 지원 티켓에 대 한 **설명** 의 **세부 정보** 탭에서 다음을 입력 합니다.

   - POC 또는 프로덕션 
   - 지역 이름
   - 호스트 수
   - 기타 세부 정보
   - 여러 고객을 호스트할 예정 인가요?

   >[!NOTE]
   >Azure VMware 솔루션은 사설 클라우드를 실행 하 고 중복성 N + 1 호스트에 대해 최소 세 개의 호스트를 권장 합니다. 

1. **검토 + 만들기** 를 선택 하 여 요청을 제출 합니다.

   지원 담당자가 요청을 확인 하는 데 최대 5 영업일 소요 됩니다.

   >[!IMPORTANT] 
   >기존 Azure VMware 솔루션이 이미 있고 추가 호스트를 요청 하는 경우 5 영업일 동안 호스트를 할당 해야 합니다. 

1. 구독이 서비스 공급자에 의해 관리 되는 경우 관리 팀은 파트너 센터에서 (aobo)를 대신 하 여 다시 관리자를 사용 하 여 Azure Portal **에** 액세스 해야 합니다. 하나 Azure Portal [Cloud Shell](../cloud-shell/overview.md) 인스턴스를 시작 하 고 **Microsoft AVS** 리소스 공급자를 등록 하 고 Azure VMware 솔루션 사설 클라우드의 배포를 진행 합니다.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   리소스 공급자를 등록하는 추가 방법은 [리소스 공급자 및 유형](../azure-resource-manager/management/resource-providers-and-types.md)을 참조하세요.

1. 고객이 구독을 직접 관리 하는 경우 구독에 대 한 충분 한 권한이 있는 사용자가 **MICROSOFT AVS** 리소스 공급자를 등록 해야 합니다. 자세한 내용은 [Azure 리소스 공급자 및 유형](../azure-resource-manager/management/resource-providers-and-types.md) 을 참조 하 고 리소스 공급자를 등록 하는 방법을 참조 하세요. 


## <a name="next-steps"></a>다음 단계

Azure VMware 솔루션 리소스를 사용 하도록 설정 하 고 적절 한 네트워킹이 준비 되 면 [사설 클라우드를 만들](tutorial-create-private-cloud.md)수 있습니다.
