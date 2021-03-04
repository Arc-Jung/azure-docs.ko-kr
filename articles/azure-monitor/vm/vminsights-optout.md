---
title: VM insights에서 모니터링 사용 안 함
description: 이 문서에서는 VM 정보에서 virtual machines 모니터링을 중지 하는 방법을 설명 합니다.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 2de0dcd52745ebadb02ab8dbb563e28abf2822dc
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102046485"
---
# <a name="disable-monitoring-of-your-vms-in-vm-insights"></a>VM insights에서 Vm 모니터링 사용 안 함

Vm (가상 머신) 모니터링을 사용 하도록 설정한 후 나중에 VM insights에서 모니터링을 사용 하지 않도록 선택할 수 있습니다. 이 문서에서는 하나 이상의 Vm에 대 한 모니터링을 사용 하지 않도록 설정 하는 방법을 보여 줍니다.  

현재 VM 정보는 VM 모니터링의 선택적 비활성화를 지원 하지 않습니다. Log Analytics 작업 영역에서 VM 정보 및 기타 솔루션을 지원할 수 있습니다. 다른 모니터링 데이터를 수집할 수도 있습니다. Log Analytics 작업 영역에서 이러한 서비스를 제공 하는 경우 시작 하기 전에 모니터링을 사용 하지 않도록 설정 하는 효과와 방법을 이해 해야 합니다.

VM insights는 다음 구성 요소에 의존 하 여 환경을 제공 합니다.

* Vm 및 기타 원본의 모니터링 데이터를 저장 하는 Log Analytics 작업 영역입니다.
* 작업 영역에 구성 된 성능 카운터의 컬렉션입니다. 컬렉션은 작업 영역에 연결 된 모든 Vm의 모니터링 구성을 업데이트 합니다.
* `VMInsights`-작업 영역에서 구성 된 모니터링 솔루션입니다. 이 솔루션은 작업 영역에 연결 된 모든 Vm의 모니터링 구성을 업데이트 합니다.
* `MicrosoftMonitoringAgent``DependencyAgent`AZURE VM 확장 인 및 이러한 확장은 데이터를 수집 하 고 작업 영역으로 보냅니다.

Vm 모니터링을 사용 하지 않도록 준비할 때 다음 사항을 염두에 두어야 합니다.

* 단일 VM을 사용 하 여 평가 하 고 미리 선택 된 기본 Log Analytics 작업 영역을 사용한 경우 VM에서 종속성 에이전트를 제거 하 고이 작업 영역에서 Log Analytics 에이전트의 연결을 해제 하 여 모니터링을 사용 하지 않도록 설정할 수 있습니다. 이 방법은 다른 용도로 VM을 사용 하 고 나중에 다른 작업 영역에 다시 연결 하도록 결정 하는 경우에 적합 합니다.
* 다른 원본에서 다른 모니터링 솔루션 및 데이터 컬렉션을 지 원하는 기존 Log Analytics 작업 영역을 선택한 경우 작업 영역을 중단 하거나 영향을 주지 않고 작업 영역에서 솔루션 구성 요소를 제거할 수 있습니다.  

>[!NOTE]
> 작업 영역에서 솔루션 구성 요소를 제거한 후에도 계속 해 서 Azure Vm에 대 한 성능 및 지도 데이터를 확인할 수 있습니다. 데이터는 궁극적으로 **성능** 및 **지도** 보기에 나타나지 않습니다. 선택 된 Azure VM에서 **사용** 옵션을 사용할 수 있으므로 나중에 모니터링을 다시 사용 하도록 설정할 수 있습니다.  

## <a name="remove-vm-insights-completely"></a>VM 정보를 완전히 제거

Log Analytics 작업 영역이 여전히 필요한 경우 다음 단계에 따라 VM 정보를 완전히 제거 합니다. `VMInsights`작업 영역에서 솔루션을 제거 합니다.  

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. Azure Portal에서 **모든 서비스** 를 선택합니다. 리소스 목록에서 **Log Analytics** 를 입력합니다. 입력을 시작 하면 목록에서 입력을 기준으로 제안을 필터링 합니다. **Log Analytics** 를 선택합니다.
3. Log Analytics 작업 영역 목록에서 VM insights를 사용 하도록 설정할 때 선택한 작업 영역을 선택 합니다.
4. 왼쪽에서 **솔루션** 을 선택 합니다.  
5. 솔루션 목록에서 **VMInsights (작업 영역 이름)** 을 선택 합니다. 솔루션에 대 한 **개요** 페이지에서 **삭제** 를 선택 합니다. 확인 하 라는 메시지가 표시 되 면 **예** 를 선택 합니다.

## <a name="disable-monitoring-and-keep-the-workspace"></a>모니터링 사용 안 함 및 작업 영역 유지  

Log Analytics 작업 영역에서 여전히 다른 원본의 모니터링을 지원 해야 하는 경우 다음 단계에 따라 VM 정보를 평가 하는 데 사용한 VM에서 모니터링을 사용 하지 않도록 설정 합니다. Azure Vm의 경우 VM에서 직접 Windows 또는 Linux 용 종속성 에이전트 VM 확장 및 Log Analytics 에이전트 VM 확장을 제거 합니다. 

>[!NOTE]
>다음 경우 Log Analytics 에이전트를 제거 하지 마세요. 
>
> * Azure Automation는 VM을 관리 하 여 프로세스를 오케스트레이션 하거나 구성 또는 업데이트를 관리 합니다. 
> * Azure Security Center는 보안 및 위협 검색을 위해 VM을 관리 합니다. 
>
> Log Analytics 에이전트를 제거 하면 해당 서비스 및 솔루션이 VM을 사전에 관리 하지 못하게 됩니다. 

1. [Azure Portal](https://portal.azure.com)에 로그인합니다. 
2. Azure Portal에서 **Virtual Machines** 를 선택합니다. 
3. 목록에서 VM을 선택합니다. 
4. 왼쪽에서 **확장** 을 선택 합니다. **확장** 페이지에서 **DependencyAgent** 를 선택 합니다.
5. 확장 속성 페이지에서 **제거** 를 선택 합니다.
6. **확장** 페이지에서 **MicrosoftMonitoringAgent** 를 선택 합니다. 확장 속성 페이지에서 **제거** 를 선택 합니다.  
