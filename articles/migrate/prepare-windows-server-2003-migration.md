---
title: Azure Migrate를 사용 하 여 마이그레이션하기 위해 Windows Server 2003 서버 준비
description: Azure Migrate를 사용 하 여 마이그레이션하기 위해 Windows Server 2003 서버를 준비 하는 방법을 알아봅니다.
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: f8d3dea970d07d951467a44661e12000ba413f72
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96753753"
---
# <a name="prepare-windows-server-2003-machines-for-migration"></a>마이그레이션을 위해 Windows Server 2003 컴퓨터 준비

이 문서에서는 Azure로 마이그레이션하기 위해 Windows Server 2003를 실행 하는 컴퓨터를 준비 하는 방법을 설명 합니다. 


> [!NOTE]
> [Windows Server 2003 연장 지원은](/troubleshoot/azure/virtual-machines/run-win-server-2003#microsoft-windows-server-2003-end-of-support) 2015 년 7 월 14 일에 종료 되었습니다.  Azure 지원 팀은 Azure에서 Windows Server 2003를 실행 하는 문제를 해결 하는 데 도움이 됩니다. 그러나이 지원은 OS 수준 문제 해결 또는 패치가 필요 하지 않은 문제로 제한 됩니다. 최신 버전의 Windows Server를 실행 하는 Azure 인스턴스로 응용 프로그램을 마이그레이션하는 것이 Azure 클라우드의 유연성과 안정성을 효과적으로 활용 하는 데 권장 되는 방법입니다. 그러나 Windows Server 2003를 Azure로 마이그레이션하도록 선택 하는 경우 Windows Server가 VMware 또는 Hyper-v에서 실행 되는 VM 인 경우 Azure Migrate: 서버 마이그레이션 도구를 사용할 수 있습니다.


- 에이전트 없는 마이그레이션을 사용 하 여 [Hyper-v vm](tutorial-migrate-hyper-v.md) 및 [VMware vm](tutorial-migrate-vmware.md) 을 Azure로 마이그레이션할 수 있습니다.
- 마이그레이션 후 Azure Vm에 연결 하기 위해 Azure VM에 Hyper-v Integration Services를 설치 해야 합니다. Windows Server 2003 컴퓨터에는 기본적으로 설치 되어 있지 않습니다.
- Hyper-v Integration Services를 설치 하기 위한 직접 다운로드 링크가 없으므로 다음을 수행 해야 합니다.
    - Hyper-v Vm이 설치 되어 있지 않은 Hyper-v Vm의 경우 Hyper-v 역할을 사용 하 여 Windows Server 2012 R2/Windows Server 2012를 실행 하는 컴퓨터에서 Integration Services에 대 한 설치 파일을 추출 하 고 설치 관리자를 Windows Server 2003 컴퓨터에 복사 합니다. 설치 파일은 Windows Server 2016를 실행 하는 컴퓨터에서 사용할 수 없습니다.
    - VMware Vm의 경우 마이그레이션 후 Azure VM이 시작 될 때 Integration Services를 설치 하는 시작 작업을 만듭니다.


## <a name="install-on-hyper-v-vms"></a>Hyper-v Vm에 설치

마이그레이션하기 전에 Hyper-v Integration Services 설치 되어 있는지 확인 하 고 필요한 경우 설치 합니다.

1. [이러한 지침](/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#turn-an-integration-service-on-or-off-using-hyper-v-manager) 에 따라 설치 여부를 확인 합니다.
2. 설치 되지 않은 경우 Hyper-v 역할을 사용 하 여 Windows Server 2012 R2/Windows Server 2012를 실행 하는 컴퓨터에 로그인 합니다.
3. **C:\Windows\System32\vmguest.iso** 에서 설치 파일로 이동 하 여 파일을 탑재 합니다.
2. Windows Server 2003 컴퓨터에 설치 폴더를 복사 하 고 Integration Services를 설치 합니다.
4. 설치 후에는 기본 설정을 Integration Services 그대로 둘 수 있습니다. 

## <a name="install-on-vmware-vms"></a>VMware Vm에 설치

1. Hyper-v 역할을 사용 하 여 Windows Server 2012 R2/Windows Server 2012를 실행 하는 컴퓨터에 로그인 합니다.
2. **C:\Windows\System32\vmguest.iso** 에서 설치 파일로 이동 하 여 파일을 탑재 합니다.
3. VMware VM에 설치 폴더를 복사 합니다.
4. VM의 명령줄에서를 실행 ```gpedit.msc``` 합니다.
5. **컴퓨터 구성**  >  **Windows 설정**  >  **스크립트 (시작/종료)** 를 엽니다.
6. **시작**  >    >  **스크립트 이름** 추가에 setup.exe 주소를 입력 합니다.
7. Azure로 마이그레이션한 후에 Azure VM이 처음 시작 될 때 스크립트가 실행 됩니다.
8. Azure VM을 수동으로 다시 시작 합니다. 부팅 진단에는 다시 시작이 필요 함을 나타내는 팝업이 있습니다.
9. 스크립트가 실행 되 고 Azure VM에 Hyper-v Integration Services 설치 된 후 시작에서 해당 스크립트를 제거할 수 있습니다.
10. 설치 후에는 기본 설정을 Integration Services 그대로 둘 수 있습니다. 

## <a name="next-steps"></a>다음 단계

- [VMware](migrate-support-matrix-vmware-migration.md) 및 [hyper-v](migrate-support-matrix-hyper-v-migration.md) vm에 대 한 마이그레이션 요구 사항을 검토 합니다.
- [VMware](server-migrate-overview.md) 및 [hyper-v](tutorial-migrate-hyper-v.md) vm을 마이그레이션합니다.
