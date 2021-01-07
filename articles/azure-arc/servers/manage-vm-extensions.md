---
title: Azure Arc 사용 서버를 사용 하 여 VM 확장 관리
description: Azure Arc 사용 서버는 Azure가 아닌 Vm을 사용 하 여 배포 후 구성 및 자동화 작업을 제공 하는 가상 머신 확장의 배포를 관리할 수 있습니다.
ms.date: 12/14/2020
ms.topic: conceptual
ms.openlocfilehash: 55e21f9c6bcd2dfe5f995093034773f2a87d9b03
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97504511"
---
# <a name="virtual-machine-extension-management-with-azure-arc-enabled-servers"></a>Azure Arc 사용 서버를 사용 하 여 가상 머신 확장 관리

VM (가상 컴퓨터) 확장은 Azure Vm에서 배포 후 구성 및 자동화 작업을 제공 하는 작은 응용 프로그램입니다. 예를 들어 가상 컴퓨터에서 소프트웨어 설치, 바이러스 백신 보호를 요구 하거나 스크립트를 실행 하려는 경우 VM 확장을 사용할 수 있습니다.

Azure Arc 사용 서버를 사용 하면 azure VM 확장을 비 Azure Windows 및 Linux Vm에 배포 하 여 해당 수명 주기 동안 하이브리드 컴퓨터의 관리를 간소화할 수 있습니다. 가상 컴퓨터 또는 Arc 사용 서버에서 관리 하는 서버에서 다음 방법을 사용 하 여 VM 확장을 관리할 수 있습니다.

- [Azure Portal](manage-vm-extensions-portal.md)
- [Azure CLI](manage-vm-extensions-cli.md)
- [Azure PowerShell](manage-vm-extensions-powershell.md)
- Azure [리소스 관리자 템플릿](manage-vm-extensions-template.md)

## <a name="key-benefits"></a>주요 이점

Azure Arc 사용 서버 VM 확장 지원은 다음과 같은 주요 이점을 제공 합니다.

- [Azure Automation 상태 구성을](../../automation/automation-dsc-overview.md) 사용 하 여 중앙에서 구성을 저장 하 고 DSC VM 확장을 통해 사용 하도록 설정 된 하이브리드 연결 컴퓨터의 원하는 상태를 유지 관리 합니다.

- Log Analytics 에이전트 VM 확장을 통해 사용 하도록 설정 된 [Azure Monitor의 로그](../../azure-monitor/platform/data-platform-logs.md) 를 분석 하기 위해 로그 데이터를 수집 합니다. 이는 다양 한 종류의 원본에서 데이터에 대해 복잡 한 분석을 수행 하는 데 유용 합니다.

- [VM용 Azure Monitor](../../azure-monitor/insights/vminsights-overview.md)를 사용 하 여 Windows 및 Linux vm의 성능을 분석 하 고 다른 리소스 및 외부 프로세스에 대 한 프로세스 및 종속성을 모니터링 합니다. 이는 Log Analytics 에이전트와 종속성 에이전트 VM 확장을 모두 사용 하도록 설정 하 여 수행 됩니다.

- 사용자 지정 스크립트 확장을 사용 하 여 하이브리드 연결 컴퓨터에서 스크립트를 다운로드 하 여 실행 합니다. 이 확장은 배포 후 구성, 소프트웨어 설치 또는 기타 구성 또는 관리 작업에 유용합니다.

- [Azure Key Vault](../../key-vault/general/overview.md)에 저장 된 인증서를 자동으로 새로 고칩니다.

## <a name="availability"></a>가용성

VM 확장 기능은 지원 되는 [지역](overview.md#supported-regions)목록 에서만 사용할 수 있습니다. 이러한 지역 중 하나에서 컴퓨터를 등록 했는지 확인 합니다.

## <a name="extensions"></a>확장

이 릴리스에서는 Windows 및 Linux 컴퓨터에서 다음 VM 확장을 지원 합니다.

|확장 |OS |Publisher |추가 정보 |
|----------|---|----------|-----------------------|
|CustomScriptExtension |Windows |Microsoft.Compute |[Windows 사용자 지정 스크립트 확장](../../virtual-machines/extensions/custom-script-windows.md)|
|DSC |Windows |Microsoft. PowerShell|[Windows PowerShell DSC 확장](../../virtual-machines/extensions/dsc-windows.md)|
|Log Analytics 에이전트 |Windows |Microsoft.EnterpriseCloud.Monitoring |[Windows 용 Log Analytics VM 확장](../../virtual-machines/extensions/oms-windows.md)|
|Microsoft 종속성 에이전트 | Windows |Microsoft.Compute | [Windows 용 종속성 에이전트 가상 컴퓨터 확장](../../virtual-machines/extensions/agent-dependency-windows.md)|
|Key Vault | Windows | Microsoft.Compute | [Windows용 Key Vault 가상 머신 확장](../../virtual-machines/extensions/key-vault-windows.md) |
|CustomScript|Linux |Microsoft. Azure 확장명 |[Linux 사용자 지정 스크립트 확장 버전 2](../../virtual-machines/extensions/custom-script-linux.md) |
|DSC |Linux |Microsoft.OSTCExtensions |[Linux 용 PowerShell DSC 확장](../../virtual-machines/extensions/dsc-linux.md) |
|Log Analytics 에이전트 |Linux |Microsoft.EnterpriseCloud.Monitoring |[Linux 용 Log Analytics VM 확장](../../virtual-machines/extensions/oms-linux.md) |
|Microsoft 종속성 에이전트 | Linux |Microsoft.Compute | [Linux 용 종속성 에이전트 가상 머신 확장](../../virtual-machines/extensions/agent-dependency-linux.md) |
|Key Vault | Linux | Microsoft.Compute | [Linux용 Key Vault 가상 머신 확장](../../virtual-machines/extensions/key-vault-linux.md) |

Azure 연결 된 컴퓨터 에이전트 패키지 및 확장 에이전트 구성 요소에 대 한 자세한 내용은 [에이전트 개요](agent-overview.md#agent-component-details)를 참조 하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 기능은 구독에 있는 다음 Azure 리소스 공급자에 따라 달라 집니다.

- **Microsoft.HybridCompute**
- **Microsoft.GuestConfiguration**

아직 등록 하지 않은 경우 [Azure 리소스 공급자 등록](agent-overview.md#register-azure-resource-providers)의 단계를 따릅니다.

위의 표에 참조 된 각 VM 확장에 대 한 설명서를 검토 하 여 네트워크 또는 시스템 요구 사항이 있는지 이해 해야 합니다. 이를 통해 해당 VM 확장을 사용 하는 Azure 서비스 또는 기능의 연결 문제가 발생 하지 않도록 방지할 수 있습니다.

### <a name="log-analytics-vm-extension"></a>Log Analytics VM 확장

Linux 용 Log Analytics 에이전트 VM 확장에는 대상 컴퓨터에 Python 2.x가 설치 되어 있어야 합니다. 

### <a name="azure-key-vault-vm-extension-preview"></a>Azure Key Vault VM 확장 (미리 보기)

Key Vault VM 확장 (미리 보기)은 다음 Linux 운영 체제를 지원 하지 않습니다.

- CentOS Linux 7(x64)
- RHEL(Red Hat Enterprise Linux) 7(x64)
- Amazon Linux 2(x64)

Key Vault VM 확장 (미리 보기) 배포는 다음을 통해서만 지원 됩니다.

- Azure CLI
- Azure PowerShell
- Azure Resource Manager 템플릿

확장을 배포 하기 전에 다음을 완료 해야 합니다.

1. [자격 증명 모음 및 인증서](../../key-vault/certificates/quick-create-portal.md) (자체 서명 또는 가져오기)를 만듭니다.

2. Azure Arc 사용 서버에 인증서 암호에 대 한 액세스 권한을 부여 합니다. [RBAC 미리 보기](../../key-vault/general/rbac-guide.md)를 사용 하는 경우 Azure Arc 리소스의 이름을 검색 하 고 **Key Vault 비밀 사용자 (미리 보기)** 역할에 할당 합니다. [Key Vault 액세스 정책을](../../key-vault/general/assign-access-policy-portal.md)사용 하는 경우 Azure Arc 리소스의 시스템 할당 Id에 Secret **Get** 권한을 할당 합니다.

### <a name="connected-machine-agent"></a>Connected Machine 에이전트

컴퓨터가 Azure 연결 된 컴퓨터 에이전트에 대해 [지원 되는 버전](agent-overview.md#supported-operating-systems) 의 Windows 및 Linux 운영 체제와 일치 하는지 확인 합니다.

Windows 및 Linux에서이 기능으로 지원 되는 연결 된 컴퓨터 에이전트의 최소 버전은 1.0 릴리스입니다.

필요한 에이전트 버전으로 컴퓨터를 업그레이드 하려면 [에이전트 업그레이드](manage-agent.md#upgrading-agent)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

[Azure Portal](manage-vm-extensions-portal.md)또는 [Azure Resource Manager 템플릿에서](manage-vm-extensions-template.md) [Azure CLI](manage-vm-extensions-cli.md), [Azure PowerShell](manage-vm-extensions-powershell.md)를 사용 하 여 VM 확장을 배포, 관리 및 제거할 수 있습니다.
