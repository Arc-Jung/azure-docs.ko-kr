---
title: Azure Backup Server를 사용 하 여 Azure VMware 솔루션 Vm 백업
description: Azure Backup Server를 사용 하 여 가상 컴퓨터를 백업 하도록 Azure VMware 솔루션 환경을 구성 합니다.
ms.topic: how-to
ms.date: 02/04/2021
ms.openlocfilehash: 163065556b7dbc979d68613744ea827c209d9fda
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102618919"
---
# <a name="back-up-azure-vmware-solution-vms-with-azure-backup-server"></a>Azure Backup Server를 사용 하 여 Azure VMware 솔루션 Vm 백업

이 문서에서는 Azure Backup Server를 사용 하 여 Azure VMware 솔루션에서 실행 되는 VMware Vm (가상 머신)을 백업 합니다. 먼저 [Azure VMware 솔루션에 대 한 Microsoft Azure Backup 서버 설정](set-up-backup-server-for-azure-vmware-solution.md)을 철저히 진행 합니다.

그런 다음, 다음을 수행 하는 데 필요한 모든 절차를 안내 합니다.

> [!div class="checklist"] 
> * Azure Backup Server가 HTTPS를 통해 VMware 서버와 통신할 수 있도록 보안 채널을 설정합니다. 
> * Azure Backup Server에 계정 자격 증명을 추가 합니다. 
> * Azure Backup Server에 vCenter를 추가 합니다. 
> * 백업할 VMware VM을 포함하는 보호 그룹을 설정하고 백업 설정을 지정한 다음, 백업을 예약합니다.

## <a name="create-a-secure-connection-to-the-vcenter-server"></a>VCenter server에 대 한 보안 연결 만들기

기본적으로 Azure Backup Server는 HTTPS를 통해 VMware 서버와 통신합니다. HTTPS 연결을 설정 하려면 VMware CA (인증 기관) 인증서를 다운로드 하 여 Azure Backup Server에서 가져옵니다.

### <a name="set-up-the-certificate"></a>인증서 설정

1. 브라우저의 Azure Backup Server 컴퓨터에서 vSphere 웹 클라이언트 URL을 입력 합니다.

   > [!NOTE] 
   > VMware **시작** 페이지가 표시 되지 않으면 연결 및 브라우저 프록시 설정을 확인 하 고 다시 시도 하세요.

1. VMware **시작** 페이지에서 **신뢰할 수 있는 루트 CA 인증서 다운로드** 를 선택 합니다.

   :::image type="content" source="../backup/media/backup-azure-backup-server-vmware/vsphere-web-client.png" alt-text="vSphere Web Client":::

1. **download.zip** 파일을 Azure Backup Server 컴퓨터에 저장 한 다음를 포함 하는 **인증서** 폴더에 해당 콘텐츠를 추출 합니다.

   - 0 및. 1과 같이 번호가 매겨진 시퀀스로 시작 하는 확장명을 포함 하는 루트 인증서 파일입니다.
   - 확장명이 r 0 또는 r1과 같은 시퀀스로 시작 하는 CRL 파일

1. **인증서** 폴더에서 루트 인증서 파일을 마우스 오른쪽 단추로 클릭 하 고 **이름 바꾸기** 를 선택 하 여 확장명을 **.crt** 로 변경 합니다.

   파일 아이콘이 루트 인증서를 나타내는 아이콘으로 변경됩니다.

1. 루트 인증서를 마우스 오른쪽 단추로 클릭 하 고 **인증서 설치** 를 선택 합니다.

1. **인증서 가져오기 마법사** 에서 **로컬 컴퓨터** 를 인증서의 대상으로 선택 하 고 **다음** 을 선택 합니다.

   ![마법사 시작 페이지](../backup/media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

   > [!NOTE] 
   > 메시지가 표시 되 면 컴퓨터에 대 한 변경을 허용할 것인지 확인 합니다.

1. **모든 인증서를 다음 저장소에** 저장을 선택 하 고 **찾아보기** 를 선택 하 여 인증서 저장소를 선택 합니다.

   ![인증서 스토리지](../backup/media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

1. 대상 폴더로 **신뢰할 수 있는 루트 인증 기관** 을 선택 하 고 **확인** 을 선택 합니다.

1. 설정을 검토 하 고 **마침** 을 선택 하 여 인증서 가져오기를 시작 합니다.

   ![인증서가 적절한 폴더에 있는지 확인](../backup/media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

1. 인증서 가져오기가 확인 된 후 vCenter 서버에 로그인 하 여 연결이 안전한 지 확인 합니다.

### <a name="enable-tls-12-on-azure-backup-server"></a>Azure Backup Server에서 TLS 1.2 사용

VMware 6.7는 TLS를 통신 프로토콜로 사용 하도록 설정 했습니다. 

1. 다음 레지스트리 설정을 복사 하 여 메모장에 붙여 넣습니다. 그런 다음 파일을 TLS로 저장 합니다. .Txt 확장명이 없는 REG.

   ```
   
   Windows Registry Editor Version 5.00
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   ```

1. TLS를 마우스 오른쪽 단추로 클릭 합니다. REG 파일을 선택 하 고 **병합** 또는 **열기** 를 선택 하 여 설정을 레지스트리에 추가 합니다.


## <a name="add-the-account-on-azure-backup-server"></a>Azure Backup Server에 계정 추가

1. Azure Backup Server를 열고 Azure Backup Server 콘솔에서 **관리**  >  **프로덕션 서버**  >  **VMware 관리** 를 선택 합니다.

   ![Azure Backup Server 콘솔](../backup/media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

1. **자격 증명 관리** 대화 상자에서 **추가** 를 선택 합니다.

   ![자격 증명 관리 대화 상자에서 추가를 선택 합니다.](../backup/media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

1. **자격 증명 추가** 대화 상자에서 새 자격 증명에 대한 이름 및 설명을 입력합니다. VMware 서버에서 정의한 사용자 이름 및 암호를 지정 합니다.

   > [!NOTE] 
   > VMware 서버 및 Azure Backup Server 같은 도메인에 없는 경우 **사용자 이름** 상자에 도메인을 지정 합니다.

   ![Azure Backup Server 자격 증명 추가 대화 상자](../backup/media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

1. **추가** 를 선택 하 여 새 자격 증명을 추가 합니다.

   ![스크린샷 새 자격 증명을 표시 하는 Azure Backup Server 자격 증명 관리 대화 상자를 표시 합니다.](../backup/media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

## <a name="add-the-vcenter-server-to-azure-backup-server"></a>Azure Backup Server에 vCenter 서버 추가

1. Azure Backup Server 콘솔에서 **관리**  >  **프로덕션 서버**  >  **추가** 를 선택 합니다.

   ![프로덕션 서버 추가 마법사 열기](../backup/media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

1. **VMware 서버** 를 선택 하 고 **다음** 을 선택 합니다.

   ![프로덕션 서버 추가 마법사](../backup/media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

1. VCenter의 IP 주소를 지정 합니다.

   ![VMware 서버 지정](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

1. **SSL 포트** 상자에 vCenter와 통신 하는 데 사용 되는 포트를 입력 합니다.

   > [!TIP] 
   > 포트 443는 기본 포트 이지만 vCenter가 다른 포트에서 수신 대기 하는 경우이 포트를 변경할 수 있습니다.

1. **자격 증명 지정** 상자에서 이전 섹션에서 만든 자격 증명을 선택 합니다.

1. **추가** 를 선택 하 여 vCenter를 서버 목록에 추가 하 고 **다음** 을 선택 합니다.

   ![VMware 서버 및 자격 증명 추가](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

1. **요약** 페이지에서 **추가** 를 선택 하 여 Azure Backup Server에 vCenter를 추가 합니다.

   새 서버가 즉시 추가 됩니다. vCenter는 에이전트가 필요 하지 않습니다.

   ![Azure Backup Server에 VMware 서버 추가](../backup/media/backup-azure-backup-server-vmware/tasks-screen.png)

1. **마침** 페이지에서 설정을 검토 하 고 **닫기** 를 선택 합니다.

   ![마침 페이지](../backup/media/backup-azure-backup-server-vmware/summary-screen.png)

   다음을 사용 하 여 **프로덕션 서버** 에 나열 된 vCenter 서버가 표시 됩니다.
   - **VMware 서버로** 입력 
   - 에이전트 상태를 **정상** 으로 
   
      **에이전트 상태가** **알 수 없음** 으로 표시 되 면 **새로 고침** 을 선택 합니다.

## <a name="configure-a-protection-group"></a>보호 그룹 구성

보호 그룹은 여러 VM을 수집하고 동일한 데이터 보존 및 백업 설정을 그룹의 모든 VM에 적용합니다.

1. Azure Backup Server 콘솔에서 **보호**  >  **새로 만들기** 를 선택 합니다.

   ![새 보호 그룹 만들기 마법사 열기](../backup/media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. **새 보호 그룹 만들기** 마법사 시작 페이지에서 **다음** 을 선택 합니다.

   ![새 보호 그룹 만들기 마법사 대화 상자](../backup/media/backup-azure-backup-server-vmware/protection-wizard.png)

1. **보호 그룹 형식 선택** 페이지에서 **서버** 를 선택하고 **다음** 을 선택합니다. **그룹 구성원 선택** 페이지가 나타납니다.

1. **그룹 구성원 선택** 페이지에서 백업 하려는 vm (또는 vm 폴더)을 선택 하 고 **다음** 을 선택 합니다.

   > [!NOTE]
   > 폴더 또는 Vm을 선택 하면 해당 폴더 내의 폴더도 백업 하도록 선택 됩니다. 백업하지 않으려는 폴더 또는 VM을 선택 취소할 수 있습니다. VM 또는 폴더가 이미 백업 중인 경우 vm에 대해 중복 복구 지점이 생성 되지 않도록 선택할 수 없습니다.

   ![그룹 구성원 선택](../backup/media/backup-azure-backup-server-vmware/server-add-selected-members.png)

1. **데이터 보호 방법 선택** 페이지에서 보호 그룹 및 보호 설정의 이름을 입력 합니다. 

1. 단기 보호를 **디스크** 에 설정 하 고 온라인 보호를 사용 하도록 설정한 후 **다음** 을 선택 합니다.

   ![데이터 보호 방법 선택](../backup/media/backup-azure-backup-server-vmware/name-protection-group.png)

1. 데이터를 디스크에 백업 하는 데 사용할 기간을 지정 합니다.

   - **보존 범위**: 디스크 복구 지점이 유지 되는 일 수입니다.
   - **빠른 전체 백업**: 디스크 복구 지점이 수행 되는 빈도입니다. 단기 백업이 발생 하는 시간 또는 날짜를 변경 하려면 **수정** 을 선택 합니다.

   :::image type="content" source="media/azure-vmware-solution-backup/new-protection-group-specify-short-term-goals.png" alt-text="디스크 기반 보호의 단기 목표 지정":::

1. **디스크 저장소 할당 검토** 페이지에서 VM 백업용으로 제공 된 디스크 공간을 검토 합니다.

   - 권장되는 디스크 할당은 지정된 보존 범위, 워크로드의 형식 및 보호된 데이터의 크기를 기반으로 합니다. 필요한 항목을 변경한 후 **다음** 을 선택 합니다.
   - **데이터 크기:** 보호 그룹의 데이터 크기입니다.
   - **디스크 공간:** 보호 그룹에 권장 되는 디스크 공간의 양입니다. 이 설정을 수정 하려면 각 데이터 원본의 예상 크기 보다 약간 더 큰 공간을 선택 합니다.
   - **저장소 풀 세부 정보:** 총 및 남은 디스크 크기를 포함 하는 저장소 풀의 상태를 표시 합니다.

   :::image type="content" source="media/azure-vmware-solution-backup/review-disk-allocation.png" alt-text="저장소 풀에 지정 된 디스크 공간을 검토 합니다.":::

   > [!NOTE]
   > 일부 시나리오에서는 보고 된 데이터 크기가 실제 VM 크기 보다 높습니다. 문제를 인식 하 고 현재 조사 하 고 있습니다.

1. **복제본 만들기 방법 선택** 페이지에서 초기 백업을 수행할 방법을 지정 하 고 **다음** 을 선택 합니다.

   - 기본 설정은 **네트워크를 통해 자동으로** 및 **지금** 입니다. 기본값을 사용 하는 경우 사용량이 적은 시간을 지정 합니다. **나중** 에를 선택 하는 경우 날짜 및 시간을 지정 합니다.
   - 대용량 데이터이거나 네트워크 상태가 최적화되지 않은 경우 이동식 미디어를 사용하여 데이터를 오프라인으로 복제하는 것을 고려하세요.

   ![복제본 만들기 방법 선택](../backup/media/backup-azure-backup-server-vmware/replica-creation.png)

1. **일관성 확인 옵션** 의 경우 일관성 확인을 자동화할 방법 및 시기를 선택 하 고 **다음** 을 선택 합니다.

   - 복제 데이터가 일관성을 잃은 경우 또는 설정된 일정에 따라 일관성 확인을 실행할 수 있습니다.
   - 자동 일관성 확인을 구성 하지 않으려는 경우 보호 그룹을 마우스 오른쪽 단추로 클릭 하 고 **일관성 확인을 수행** 하 여 수동 확인을 실행할 수 있습니다.

1. **온라인 보호 데이터 지정** 페이지에서 백업 하려는 VM 또는 vm 폴더를 선택 하 고 **다음** 을 선택 합니다. 

   > [!TIP]
   > 멤버를 개별적으로 선택 하거나 모두 **선택** 을 선택 하 여 모든 멤버를 선택할 수 있습니다.

   ![온라인 보호 데이터 지정](../backup/media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. **온라인 백업 일정 지정** 페이지에서 로컬 저장소에서 Azure로 데이터를 백업할 빈도를 지정 합니다. 

   - 일정에 따라 생성 되는 데이터에 대 한 클라우드 복구 지점이 필요 합니다. 
   - 복구 지점이 생성 되 면 Azure에서 Recovery Services 자격 증명 모음으로 전송 됩니다.

   ![온라인 백업 일정 지정](../backup/media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. **온라인 보존 정책 지정** 페이지에서 백업에서 만들어진 복구 지점이 Azure에 유지 되는 기간을 지정 합니다.

   - Azure에서 데이터를 유지할 수 있는 기간에 대한 시간 제한은 없습니다.
   - 유일한 제한은 보호 된 인스턴스당 9999 개의 복구 지점이 될 수 없다는 것입니다. 이 예제에서 보호되는 인스턴스는 VMware 서버입니다.

   ![온라인 보존 정책 지정](../backup/media/backup-azure-backup-server-vmware/retention-policy.png)

1. **요약** 페이지에서 설정을 검토 한 후 **그룹 만들기** 를 선택 합니다.

   ![보호 그룹 구성원 및 설정 요약](../backup/media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="monitor-with-the-azure-backup-server-console"></a>Azure Backup Server 콘솔로 모니터링

Azure VMware 솔루션 Vm을 백업 하도록 보호 그룹을 구성한 후 Azure Backup Server 콘솔을 사용 하 여 백업 작업 및 경고의 상태를 모니터링할 수 있습니다. 모니터링할 수 있는 항목은 다음과 같습니다.

- **모니터링** 작업 영역에서 다음을 수행 합니다.
   - **경고** 에서 오류, 경고 및 일반 정보를 모니터링할 수 있습니다.  활성 및 비활성 경고를 보고 전자 메일 알림을 설정할 수 있습니다.
   - **작업** 아래에서 보호 된 특정 데이터 원본 또는 보호 그룹에 대 한 Azure Backup Server 의해 시작 된 작업을 볼 수 있습니다. 작업 진행률을 따르거나 작업에서 사용 하는 리소스를 확인할 수 있습니다.
- **보호** 작업 영역에서 보호 그룹의 볼륨 및 공유 상태를 확인할 수 있습니다. 복구 설정, 디스크 할당, 백업 일정 등의 구성 설정을 확인할 수도 있습니다.
- **관리** 작업 영역에서 **디스크, 온라인** 및 **에이전트** 탭을 보고 저장소 풀의 디스크 상태, AZURE에 등록 및 배포 된 DPM 에이전트 상태를 확인할 수 있습니다.

:::image type="content" source="media/azure-vmware-solution-backup/monitor-backup-jobs.png" alt-text="Azure Backup Server에서 백업 작업의 상태를 모니터링 합니다.":::

## <a name="restore-vmware-virtual-machines"></a>VMware 가상 컴퓨터 복원

Azure Backup Server 관리자 콘솔에는 복구 가능한 데이터를 찾는 두 가지 방법이 있습니다. 검색 하거나 찾아볼 수 있습니다. 데이터를 복구할 때 데이터 또는 VM을 동일한 위치에 복원 하는 것이 좋습니다. 따라서 Azure Backup Server는 VMware VM 백업에 대 한 세 가지 복구 옵션을 지원 합니다.

- **Olr (원래 위치 복구)**: olr을 사용 하 여 보호 된 VM을 원래 위치로 복원 합니다. 백업이 발생 한 후에 추가 되거나 삭제 된 디스크가 없는 경우에만 VM을 원래 위치로 복원할 수 있습니다. 디스크를 추가 하거나 삭제 한 경우에는 대체 위치 복구를 사용 해야 합니다.
- **ALR (대체 위치 복구)**: 원래 vm이 없거나 원래 vm을 방해 하지 않으려는 경우에 사용 합니다. ESXi 호스트, 리소스 풀, 폴더 및 저장소 데이터 저장소 및 경로의 위치를 제공 합니다. 복원 된 VM을 원래 VM에서 구분 하려면 VM 이름에 *"-복구 됨"* 을 추가 Azure Backup Server 합니다.
- **ILR (개별 파일 위치 복구)**: 보호 된 Vm이 WINDOWS Server vm 인 경우 AZURE BACKUP SERVER의 ILR 기능을 사용 하 여 vm 내의 개별 파일 또는 폴더를 복구할 수 있습니다. 개별 파일을 복구하려면 이 문서의 뒤에 나오는 절차를 참조하세요. VM에서 개별 파일을 복원 하는 것은 Windows VM 및 디스크 복구 지점만 사용할 수 있습니다.

### <a name="restore-a-recovery-point"></a>복구 지점 복원

1. Azure Backup Server 관리자 콘솔에서 **복구** 보기를 선택 합니다. 

1. **찾아보기** 창에서 찾아보기 또는 필터를 사용하여 복구할 VM을 찾습니다. VM 또는 폴더를 선택한 후 * * 복구 지점은 사용 가능한 복구 지점이 표시 됩니다.

   ![사용 가능한 복구 위치](../backup/media/restore-azure-backup-server-vmware/recovery-points.png)

1. 복구 **지점에 대 한** 창에서 복구 지점을 만든 날짜를 선택 합니다. 굵게 표시된 달력 날짜에는 사용 가능한 복구 지점이 있습니다. 또는 VM을 마우스 오른쪽 단추로 클릭 하 고 **모든 복구 지점 표시** 를 선택한 다음 목록에서 복구 지점을 선택할 수 있습니다.

   > [!NOTE] 
   > 단기 보호의 경우 빠른 복구를 위해 디스크 기반 복구 지점을 선택 합니다. 단기 복구 지점이 만료 된 후 복구 하는 **온라인** 복구 지점만 표시 됩니다.

1. 온라인 복구 지점에서 복구 하기 전에 준비 위치에 복구 하려는 VM의 압축 되지 않은 전체 크기를 저장할 수 있는 충분 한 여유 공간이 있는지 확인 합니다. **구독 설정 구성 마법사** 를 실행 하 여 스테이징 위치를 보거나 변경할 수 있습니다.

   :::image type="content" source="media/azure-vmware-solution-backup/mabs-recovery-folder-settings.png" alt-text="Azure Backup Server 복구 폴더 설정":::

1. 복구 **를 선택 하** 여 **복구 마법사** 를 엽니다.

   ![복구 마법사, 복구 선택 사항 확인 페이지](../backup/media/restore-azure-backup-server-vmware/recovery-wizard.png)

1. **다음** 을 선택 하 여 **복구 옵션 지정** 화면으로 이동 합니다. **다음** 을 다시 선택 하 여 **복구 유형 선택** 화면으로 이동 합니다. 

   > [!NOTE]
   > VMware 워크 로드는 네트워크 대역폭 제한 사용을 지원 하지 않습니다.

1. **복구 유형 선택** 페이지에서 원본 인스턴스 또는 새 위치에 복구를 선택 합니다.

   - **원본 인스턴스에 복구** 를 선택하면 마법사에서 추가 항목을 선택할 필요가 없습니다. 원본 인스턴스에 대한 데이터가 사용됩니다.
   - **임의의 호스트에 가상 머신으로 복구** 를 선택한 경우 **대상 지정** 화면에서 **ESXi 호스트**, **리소스 풀**, **폴더** 및 **경로** 에 대한 정보를 제공합니다.

   ![복구 유형 선택 페이지](../backup/media/restore-azure-backup-server-vmware/recovery-type.png)

1. **요약** 페이지에서 설정을 검토 하 **고 복구를 선택 하** 여 복구 프로세스를 시작 합니다. 

   **복구 상태** 화면에 복구 작업의 진행 상태가 표시됩니다.

### <a name="restore-an-individual-file-from-a-vm"></a>VM에서 개별 파일 복원

보호된 VM 복구 지점에서 개별 파일을 복원할 수 있습니다. 이 기능은 Windows Server VM에만 사용할 수 있습니다. 개별 파일을 복원 하는 것은 .VMDK으로 이동 하 여 복구 프로세스를 시작 하기 전에 원하는 파일을 찾는 점을 제외 하 고 전체 VM을 복원 하는 것과 비슷합니다. 

> [!NOTE]
> VM에서 개별 파일을 복원 하는 것은 Windows VM 및 디스크 복구 지점만 사용할 수 있습니다.

1. Azure Backup Server 관리자 콘솔에서 **복구** 보기를 선택 합니다.

1. **찾아보기** 창에서 찾아보기 또는 필터를 사용하여 복구할 VM을 찾습니다. VM 또는 폴더를 선택한 후 * * 복구 지점은 사용 가능한 복구 지점이 표시 됩니다.

   ![사용할 수 있는 복구 위치](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk.png)

1. **복구 지점의 대상** 창에서 달력을 사용 하 여 원하는 복구 지점이 포함 된 날짜를 선택 합니다. 백업 정책이 구성 된 방식에 따라 날짜에는 복구 지점이 둘 이상 있을 수 있습니다. 

1. 복구 지점을 만든 날짜를 선택한 후에는 올바른 **복구 시간** 을 선택 했는지 확인 합니다. 

   > [!NOTE]
   > 선택한 날짜에 복구 지점이 여러 개 있는 경우 **복구 시간** 드롭다운 메뉴에서 복구 지점을 선택합니다. 

   복구 지점을 선택 하면 복구 가능한 항목 목록이 **경로** 창에 표시 됩니다.

1. 복구 하려는 파일을 찾으려면 **경로** 창에서 **복구 가능한 항목** 열의 항목을 두 번 클릭 하 여 엽니다. 그런 다음 복구 하려는 파일을 하나 이상 선택 합니다. 여러 항목을 선택 하려면 **Ctrl** 키를 선택한 상태에서 각 항목을 선택 합니다. **경로** 창을 사용 하 여 **복구 가능한 항목** 열에 표시 되는 파일 또는 폴더 목록을 검색할 수 있습니다.
    
   > [!NOTE]
   > **아래 검색 목록은** 하위 폴더로 검색 하지 않습니다. 하위 폴더를 검색하려면 폴더를 두 번 클릭합니다. **위로** 단추를 사용하여 자식 폴더에서 부모 폴더로 이동합니다. 여러 항목(파일 및 폴더)을 선택할 수 있지만 항목이 동일한 부모 폴더에 있어야 합니다. 동일한 복구 작업에서 여러 폴더의 항목을 복구할 수 없습니다.

   ![복구 선택 사항 확인](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk-ilr-2.png)

1. 복구할 항목을 선택한 경우 관리자 콘솔 도구 리본 **에서 복구를 선택 하** 여 **복구 마법사** 를 엽니다. 복구 **마법사** 에서 **복구 선택 사항 확인** 화면에는 복구할 선택 된 항목이 표시 됩니다.

1. **복구 옵션 지정** 화면에서 다음 단계 중 하나를 수행 합니다.

   - 네트워크 대역폭 제한을 사용 하려면 **수정** 을 선택 합니다. **제한** 대화 상자에서 **네트워크 대역폭 사용 제한 사용** 을 선택 하 여 설정 합니다. 사용하도록 설정된 후 **설정** 및 **근무 일정** 을 구성합니다.
   - 네트워크 제한을 사용 하지 않도록 설정 하려면 **다음** 을 선택 합니다.

1. **복구 유형 선택** 화면에서 **다음** 을 선택 합니다. 파일 또는 폴더만 네트워크 폴더에 복구할 수 있습니다.

1. **대상 지정** 화면에서 **찾아보기** 를 선택 하 여 파일 또는 폴더에 대 한 네트워크 위치를 찾습니다. Azure Backup Server 복구 된 항목이 모두 복사 되는 폴더를 만듭니다. 폴더 이름에 MABS_day-month-year 접두사가 있습니다. 복구 된 파일이 나 폴더의 위치를 선택 하면 해당 위치에 대 한 세부 정보가 제공 됩니다.

   ![파일을 복구할 위치 지정](../backup/media/restore-azure-backup-server-vmware/specify-destination.png)

1. **복구 옵션 지정** 화면에서 적용할 보안 설정을 선택합니다. 네트워크 대역폭 사용량 제한을 수정하도록 선택할 수 있지만, 제한은 기본적으로 사용되지 않습니다. 또한 **SAN 복구** 및 **알림이** 사용 하도록 설정 되어 있지 않습니다.

1. **요약** 화면에서 설정을 검토 하 **고 복구를 선택 하** 여 복구 프로세스를 시작 합니다. **복구 상태** 화면에 복구 작업의 진행 상태가 표시됩니다.

## <a name="next-steps"></a>다음 단계

이제 Azure Backup Server를 사용 하 여 Azure VMware 솔루션 Vm을 백업 했으므로 다음에 대해 알아볼 수 있습니다. 

- [Azure Backup Server에서 백업을 설정할 때의 문제 해결](../backup/backup-azure-mabs-troubleshoot.md)
- [Azure VMware 솔루션 vm의 수명 주기 관리](lifecycle-management-of-azure-vmware-solution-vms.md).
