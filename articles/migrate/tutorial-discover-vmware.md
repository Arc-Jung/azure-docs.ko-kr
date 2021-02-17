---
title: Azure Migrate 서버 평가를 사용하여 VMware VM 검색
description: Azure Migrate 서버 평가 도구를 사용하여 온-프레미스 VMware VM을 검색하는 방법을 알아봅니다.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 9/14/2020
ms.custom: mvc
ms.openlocfilehash: 0e06d82c30743a4084cfc5ff856b4a9c8d548146
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98566947"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>자습서: 서버 평가를 사용하여 VMware VM 검색

Azure로 마이그레이션하는 과정의 일환으로 온-프레미스 인벤토리 및 워크로드를 검색합니다.

이 자습서에서는 간단한 Azure Migrate 어플라이언스를 사용하는 Azure Migrate: 서버 평가 도구를 사용하여 온-프레미스 VMware VM(가상 머신)을 검색하는 방법을 보여 줍니다. 어플라이언스를 VMware VM으로 배포하여 VM 및 해당 성능 메타데이터, VM에서 실행되는 애플리케이션 및 VM 종속성을 지속적으로 검색합니다.

이 자습서에서는 다음 작업 방법을 알아봅니다.

> [!div class="checklist"]
> * Azure 계정을 설정합니다.
> * 검색을 위해 VMware 환경을 준비합니다.
> * Azure Migrate 프로젝트를 만듭니다.
> * Azure Migrate 어플라이언스를 설정합니다.
> * 연속 검색을 시작합니다.

> [!NOTE]
> 이 자습서에서는 시나리오를 시도할 수 있는 가장 빠른 경로를 보여 주며, 가능한 경우 기본 옵션을 사용합니다.  

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/pricing/free-trial/)을 만듭니다.


## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 시작하기 전에 다음과 같은 사전 요구 사항이 충족되는지 확인합니다.


**요구 사항** | **세부 정보**
--- | ---
**vCenter Server/ESXi 호스트** | 버전 5.5, 6.0, 6.5 또는 6.7을 실행하는 vCenter Server가 필요합니다.<br/><br/> VM은 버전 5.5 이상을 실행하는 ESXi 호스트에서 호스팅되어야 합니다.<br/><br/> 어플라이언스에서 구성 및 성능 메타데이터를 수집할 수 있도록 vCenter Server에서 443 TCP 포트의 인바운드 연결을 허용합니다.<br/><br/> 어플라이언스는 기본적으로 포트 443의 vCenter에 연결됩니다. vCenter Server가 다른 포트에서 수신 대기하는 경우 어플라이언스 구성 관리자에서 vCenter Server 세부 정보를 제공할 때 포트를 수정할 수 있습니다.<br/><br/> VM을 호스팅하는 ESXi 서버에서 VM 및 VM 종속성에 설치된 애플리케이션을 검색하기 위해 443 TCP 포트에서 인바운드 액세스가 허용되는지 확인합니다.
**어플라이언스** | vCenter Server에는 Azure Migrate 어플라이언스에 대한 VM을 할당하기 위해 다음 리소스가 필요합니다.<br/><br/> - 32GB RAM, 8개 vCPU 및 약 80GB 디스크 스토리지<br/><br/> - 외부 가상 스위치 및 어플라이언스 VM에서 직접 또는 프록시를 통한 인터넷 액세스
**VM** | 모든 Windows 및 Linux OS 버전이 구성 및 성능 메타데이터를 검색하고 VM에 설치된 애플리케이션을 검색하는 데 지원됩니다. <br/><br/> 에이전트 없는 종속성 분석에 지원되는 OS 버전은 [여기](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)서 확인하세요.<br/><br/> 설치된 애플리케이션 및 VM 종속성을 검색하려면 VMware 도구(10.2.0 이상)를 VM에 설치하고 실행해야 하며, Windows VM에는 PowerShell 버전 2.0 이상이 설치되어 있어야 합니다.


## <a name="prepare-an-azure-user-account"></a>Azure 사용자 계정 준비

Azure Migrate 프로젝트를 만들고 Azure Migrate 어플라이언스를 등록하려면 다음이 포함된 계정이 필요합니다.
- Azure 구독에 대한 기여자 또는 소유자 권한
- AAD(Azure Active Directory) 앱을 등록할 수 있는 권한
- 에이전트 없는 VMware 마이그레이션 중에 사용되는 Key Vault를 만들기 위한 Azure 구독에 대한 소유자 또는 기여자 및 사용자 액세스 관리자 권한

Azure 체험 계정을 방금 만든 경우 자신이 구독에 대한 소유자입니다. 구독 소유자가 아닌 경우 다음과 같이 소유자와 협력하여 권한을 할당합니다.

1. Azure Portal에서 "구독"을 검색하여 **서비스** 에서 **구독** 을 선택합니다.

    ![Azure 구독을 검색하는 검색 상자](./media/tutorial-discover-vmware/search-subscription.png)

2. **구독** 페이지에서 Azure Migrate 프로젝트를 만들려는 구독을 선택합니다. 
3. 구독에서 **액세스 제어(IAM)**  > **액세스 확인** 을 선택합니다.
4. **액세스 확인** 에서 관련 사용자 계정을 검색합니다.
5. **역할 할당 추가** 에서 **추가** 를 클릭합니다.

    ![사용자 계정을 검색하여 액세스를 확인하고 역할을 할당합니다.](./media/tutorial-discover-vmware/azure-account-access.png)

6. **역할 할당 추가** 에서 기여자 또는 소유자 역할을 선택하고 계정(이 예제에서는 azmigrateuser)을 선택합니다. 그런 다음 **Save** 를 클릭합니다.

    ![계정에 역할을 할당하는 역할 할당 추가 페이지를 엽니다.](./media/tutorial-discover-vmware/assign-role.png)

1. 어플라이언스를 등록하려면 Azure 계정에 **AAD 앱을 등록할 수 있는 권한** 이 필요합니다.
1. Azure Portal에서 **Azure Active Directory** > **사용자** > **사용자 설정** 으로 차례로 이동합니다.
1. **사용자 설정** 에서 Azure AD 사용자가 애플리케이션을 등록할 수 있는지 확인합니다(기본적으로 **예** 로 설정됨).

    ![사용자 설정에서 사용자가 Active Directory 앱을 등록할 수 있는지 확인](./media/tutorial-discover-vmware/register-apps.png)

9. '앱 등록' 설정이 '아니요'로 설정된 경우 테넌트/전역 관리자에게 필요한 권한을 할당하도록 요청합니다. 또는 테넌트/전역 관리자가 **애플리케이션 개발자** 역할을 계정에 할당하여 AAD 앱 등록을 허용할 수 있습니다. [자세히 알아보기](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-vmware"></a>VMware 준비

vCenter Server에서 계정에 OVA 파일을 사용하여 VM을 만들 수 있는 권한이 있는지 확인합니다. OVA 파일을 사용하여 Azure Migrate 어플라이언스를 VMware VM으로 배포할 때 필요합니다.

서버 평가에는 VMware VM의 검색 및 평가를 위해 vCenter Server 읽기 전용 계정이 필요합니다. 설치된 애플리케이션 및 VM 종속성도 검색하려면 계정에 **가상 머신 > 게스트 작업** 에 사용하도록 설정된 권한이 필요합니다.

### <a name="create-an-account-to-access-vcenter"></a>vCenter에 액세스하는 계정 만들기

vSphere Web Client에서 계정을 다음과 같이 설정합니다.

1. 관리자 권한이 있는 계정을 사용하여 vSphere Web Client에서 **관리** 를 선택합니다.
2. **액세스** 에서 **SSO 사용자 및 그룹** 을 선택합니다.
3. **사용자** 에서 새 사용자를 추가합니다.
4. **새 사용자** 에서 계정 세부 정보를 입력합니다. 그런 후 **OK** 를 클릭합니다.
5. **전역 권한** 에서 사용자 계정을 선택하고, **읽기 전용** 역할을 계정에 할당합니다. 그런 후 **OK** 를 클릭합니다.
6. 설치된 애플리케이션 및 VM 종속성도 검색하려면 **역할** 로 이동하여 **읽기 전용** 역할을 선택하고, **권한** 에서 **게스트 작업** 을 선택합니다. "자식에 전파" 확인란을 선택하여 vCenter Server 아래의 모든 개체에 권한을 전파할 수 있습니다.
 
    ![읽기 전용 역할에서 게스트 작업을 허용하는 확인란](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>VM에 액세스하는 계정 만들기

설치된 애플리케이션 및 VM 종속성을 검색하려면 VM에 대한 필요한 권한이 있는 사용자 계정이 필요합니다. 사용자 계정은 어플라이언스 구성 관리자에서 제공할 수 있습니다. 어플라이언스는 에이전트를 VM에 설치하지 않습니다.

1. Windows VM의 경우 VM에 대한 관리 권한이 있는 계정(로컬 또는 도메인)을 만듭니다.
2. Linux VM의 경우 루트 권한이 있는 계정을 만듭니다. 또는 /bin/netstat 및 /bin/ls 파일에 대해 CAP_DAC_READ_SEARCH 및 CAP_SYS_PTRACE 권한이 있는 계정을 만들 수 있습니다.

> [!NOTE]
> 현재 Azure Migrate는 설치된 애플리케이션 및 VM 종속성을 검색하기 위해 어플라이언스에서 제공할 수 있는 Windows VM에 대한 하나의 사용자 계정과 Linux VM에 대한 하나의 사용자 계정을 지원합니다.


## <a name="set-up-a-project"></a>프로젝트 설정

새 Azure Migrate 프로젝트를 설정합니다.

1. Azure Portal > **모든 서비스** 에서 **Azure Migrate** 를 검색합니다.
2. **서비스** 아래에서 **Azure Migrate** 를 선택합니다.
3. **개요** 에서 **프로젝트 만들기** 를 선택합니다.
5. **프로젝트 만들기** 에서 Azure 구독 및 리소스 그룹을 선택합니다. 리소스 그룹이 없는 경우 리소스 그룹을 만듭니다.
6. **프로젝트 세부 정보** 에서 프로젝트 이름과 이 프로젝트를 만들려는 지역을 지정합니다. [퍼블릭](migrate-support-matrix.md#supported-geographies-public-cloud) 및 [정부 클라우드](migrate-support-matrix.md#supported-geographies-azure-government)에 대해 지원되는 지역을 검토합니다.

   ![프로젝트 이름 및 지역 상자](./media/tutorial-discover-vmware/new-project.png)

7. **만들기** 를 선택합니다.
8. Azure Migrate 프로젝트가 배포될 때까지 몇 분 정도 기다립니다. **Azure Migrate: 서버 평가** 도구가 기본적으로 새 프로젝트에 추가됩니다.

![기본적으로 추가된 서버 평가 도구를 보여주는 페이지](./media/tutorial-discover-vmware/added-tool.png)

> [!NOTE]
> 이미 프로젝트를 만든 경우 더 많은 VM을 검색하고 평가하기 위해 동일한 프로젝트를 사용하여 추가 어플라이언스를 등록할 수 있습니다. [자세한 정보](create-manage-projects.md#find-a-project)

## <a name="set-up-the-appliance"></a>어플라이언스 설정

Azure Migrate: 서버 평가는 간단한 Azure Migrate 어플라이언스를 사용합니다. 어플라이언스는 VM 검색을 수행하고, VM 구성 및 성능 메타데이터를 Azure Migrate에 보냅니다. 어플라이언스는 Azure Migrate 프로젝트에서 다운로드할 수 있는 OVA 템플릿을 배포하여 설정할 수 있습니다.

> [!NOTE]
> 어떤 이유로 템플릿을 사용하여 어플라이언스를 설정할 수 없는 경우 기존 Windows Server 2016 서버에서 PowerShell 스크립트를 사용하여 설정할 수 있습니다. [자세히 알아보기](deploy-appliance-script.md#set-up-the-appliance-for-vmware).


### <a name="deploy-with-ova"></a>OVA를 사용하여 배포

OVA 템플릿을 사용하여 어플라이언스를 설정하려면 다음을 수행합니다.
1. 포털에서 어플라이언스 이름을 제공하고, Azure Migrate 프로젝트 키를 생성합니다.
1. OVA 템플릿 파일을 다운로드하여 vCenter Server로 가져옵니다. OVA가 안전한지 확인하세요.
1. 어플라이언스를 만들고, Azure Migrate 서버 평가에 연결할 수 있는지 확인합니다.
1. 어플라이언스를 처음으로 구성하고 Azure Migrate 프로젝트 키를 사용하여 Azure Migrate 프로젝트에 등록합니다.

### <a name="1-generate-the-azure-migrate-project-key"></a>1. Azure Migrate 프로젝트 키 생성

1. **마이그레이션 목표** > **서버** > **Azure Migrate: 서버 평가** 에서 **검색** 을 선택합니다.
2. **머신 검색** > **머신이 가상화되어 있습니까?** 에서 **예, VMware vSphere 하이퍼바이저 사용** 을 선택합니다.
3. **1: Azure Migrate 프로젝트 키 생성** 에서 VMware VM을 검색하도록 설정할 Azure Migrate 어플라이언스에 대한 이름을 제공합니다. 이름은 14자 이하의 영숫자여야 합니다.
1. **키 생성** 을 클릭하여 필요한 Azure 리소스 만들기를 시작합니다. 리소스를 만드는 동안 [컴퓨터 검색] 페이지를 닫지 마세요.
1. Azure 리소스가 성공적으로 만들어지면 **Azure Migrate 프로젝트 키** 가 생성됩니다.
1. 어플라이언스를 구성하는 동안 어플라이언스 등록을 완료하는 데 필요하므로 키를 복사합니다.

### <a name="2-download-the-ova-template"></a>2. OVA 템플릿 다운로드

**2: Azure Migrate 어플라이언스 다운로드** 에서 .OVA 파일을 선택하고, **다운로드** 를 클릭합니다.

### <a name="verify-security"></a>보안 확인

배포하기 전에 OVA 파일이 안전한지 확인합니다.

1. 파일을 다운로드한 컴퓨터에서 관리자 명령 창을 엽니다.
2. 다음 명령을 실행하여 OVA 파일에 대한 해시를 생성합니다.
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   사용 예: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. 최신 어플라이언스 버전 및 해시 값을 확인합니다.

    - Azure 퍼블릭 클라우드의 경우:
    
        **알고리즘** | **다운로드** | **SHA256**
        --- | --- | ---
        VMware(11.9GB) | [최신 버전](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74

    - Azure Government의 경우:
    
        **알고리즘** | **다운로드** | **SHA256**
        --- | --- | ---
        VMware(85.8MB) | [최신 버전](https://go.microsoft.com/fwlink/?linkid=2140337) | 2daaa2a59302bf911e8ef195f8add7d7c8352de77a9af0b860e2a627979085ca

### <a name="3-create-the-appliance-vm"></a>3. 어플라이언스 VM 만들기

다운로드한 파일을 가져오고 VM을 만듭니다.

1. vSphere Client 콘솔에서 **파일** > **OVF 템플릿 배포** 를 클릭합니다.
2. OVF 템플릿 배포 마법사 > **원본** 에서 OVA 파일의 위치를 지정합니다.
3. **이름** 및 **위치** 에서 친숙한 VM 이름을 지정합니다. VM을 호스팅할 인벤토리 개체를 선택합니다.
5. **호스트/클러스터** 에서 VM이 실행될 호스트 또는 클러스터를 지정합니다.
6. **스토리지** 에서 VM에 대한 스토리지 대상을 지정합니다.
7. **디스크 형식** 에서 디스크 유형 및 크기를 지정합니다.
8. **네트워크 매핑** 에서 VM이 연결할 네트워크를 지정합니다. 메타데이터를 Azure Migrate 서버 평가에 보내려면 네트워크에 인터넷 연결이 필요합니다.
9. 설정을 검토 및 확인한 다음 **마침** 을 클릭합니다.


### <a name="verify-appliance-access-to-azure"></a>Azure에 대한 어플라이언스 액세스 확인

어플라이언스 VM에서 [퍼블릭](migrate-appliance.md#public-cloud-urls) 및 [정부](migrate-appliance.md#government-cloud-urls) 클라우드의 Azure URL에 연결할 수 있는지 확인합니다.


### <a name="4-configure-the-appliance"></a>4. 어플라이언스 구성

어플라이언스를 처음으로 설정합니다.

> [!NOTE]
> 다운로드한 OVA 대신 [PowerShell 스크립트](deploy-appliance-script.md)를 사용하여 어플라이언스를 설정하는 경우 이 절차의 처음 두 단계는 관련이 없습니다.

1. vSphere Client 콘솔에서 마우스 오른쪽 단추로 VM을 클릭한 다음, **콘솔 열기** 를 선택합니다.
2. 어플라이언스에 대한 언어, 표준 시간대 및 암호를 제공합니다.
3. VM에 연결할 수 있는 모든 머신에서 브라우저를 열고, 어플라이언스 웹앱의 URL(**https://*어플라이언스 이름 또는 IP 주소*: 44368**)을 엽니다.

   또는 앱 바로 가기를 선택하여 어플라이언스 데스크톱에서 앱을 열 수 있습니다.
1. **사용 조건** 에 동의하고 타사 정보를 읽습니다.
1. 웹앱 > **필수 구성 요소 설정** 에서 다음을 수행합니다.
   - **연결**: 앱에서 VM이 인터넷에 액세스할 수 있는지 확인합니다. VM에서 프록시를 사용하는 경우:
     - **프록시 설정** 을 클릭하여 프록시 주소(http://ProxyIPAddress 또는 http://ProxyFQDN) 형식) 및 수신 대기 포트를 지정합니다.
     - 프록시에 인증이 필요한 경우 자격 증명을 지정합니다.
     - HTTP 프록시만 지원됩니다.
     - 프록시 세부 정보를 추가하거나 프록시 및/또는 인증을 사용하지 않도록 설정한 경우 **저장** 을 클릭하여 연결 확인을 다시 트리거합니다.
   - **시간 동기화**: 검색이 제대로 작동하려면 어플라이언스의 시간이 인터넷 시간과 동기화되어야 합니다.
   - **업데이트 설치**: 어플라이언스에서 최신 업데이트가 설치되어 있는지 확인합니다. 확인이 완료되면 **어플라이언스 서비스 보기** 를 클릭하여 어플라이언스에서 실행되는 구성 요소의 상태와 버전을 확인할 수 있습니다.
   - **VDDK 설치**: 어플라이언스에서 VMware vSphere VDDK(Virtual Disk Development Kit)가 설치되어 있는지 확인합니다. 설치되지 않은 경우 **설치 지침** 에서 제공한 대로 VMware에서 VDDK 6.7을 다운로드하고, 다운로드한 zip 콘텐츠를 어플라이언스의 지정된 위치에 추출합니다.

     Azure Migrate 서버 마이그레이션에서 VDDK를 사용하여 Azure로 마이그레이션하는 동안 머신을 복제합니다. 
1. 원하는 경우 어플라이언스를 구성하는 동안 언제든지 **필수 구성 요소를 다시 실행** 하여 어플라이언스에서 모든 필수 구성 요소를 충족하는지 확인할 수 있습니다.

### <a name="register-the-appliance-with-azure-migrate"></a>Azure Migrate를 사용하여 어플라이언스 등록

1. 포털에서 복사한 **Azure Migrate 프로젝트 키** 를 붙여넣습니다. 키가 없는 경우 **서버 평가 > 검색 > 기존 어플라이언스 관리** 로 차례로 이동하여 키 생성 시 제공한 어플라이언스 이름을 선택하고, 해당 키를 복사합니다.
1. Azure로 인증하려면 디바이스 코드가 필요합니다. **로그인** 을 클릭하면 아래와 같이 디바이스 코드가 포함된 모달이 열립니다.

    ![디바이스 코드를 보여주는 모달](./media/tutorial-discover-vmware/device-code.png)

1. **코드 복사 및 로그인** 을 클릭하여 디바이스 코드를 복사하고 새 브라우저 탭에서 Azure 로그인 프롬프트를 엽니다. 표시되지 않으면 브라우저에서 팝업 차단을 사용하지 않도록 설정했는지 확인합니다.
1. 새 탭에서 디바이스 코드를 붙여넣고 Azure 사용자 이름과 암호를 사용하여 로그인합니다.
   
   PIN을 사용한 로그인은 지원되지 않습니다.
3. 로그인 탭을 실수로 로그인하지 않고 닫은 경우에는 어플라이언스 구성 관리자의 브라우저 탭을 새로 고쳐 로그인 단추를 다시 사용하도록 설정해야 합니다.
1. 성공적으로 로그인한 후 어플라이언스 구성 관리자를 사용하여 이전 탭으로 돌아갑니다.
1. 로깅에 사용되는 Azure 사용자 계정에 키 생성 시 만든 Azure 리소스에 대한 올바른 권한이 있는 경우 어플라이언스 등록이 시작됩니다.
1. 어플라이언스가 성공적으로 등록되면 **세부 정보 보기** 를 클릭하여 등록 세부 정보를 확인할 수 있습니다.



## <a name="start-continuous-discovery"></a>연속 검색 시작

어플라이언스는 VM의 구성 및 성능 데이터를 검색하기 위해 vCenter Server에 연결해야 합니다.

1. **1단계: vCenter Server 자격 증명 제공** 에서 **자격 증명 추가** 를 클릭하여 자격 증명 이름을 지정하고, 어플라이언스가 vCenter Server 인스턴스에서 VM을 검색하는 데 사용할 vCenter Server 계정에 대한 **사용자 이름** 및 **암호** 를 추가합니다.
    - 이전 자습서에서 필요한 권한이 있는 계정을 설정했어야 합니다.
    - 검색 범위를 특정 VMware 개체(vCenter Server 데이터 센터, 클러스터, 클러스터 폴더, 호스트, 호스트 폴더 또는 개별 VM)로 지정하려면 [이 문서](set-discovery-scope.md)의 지침을 검토하여 Azure Migrate에서 사용하는 계정을 제한합니다.
1. **2단계: vCenter Server 세부 정보 제공** 에서 **검색 원본 추가** 를 클릭하여 드롭다운에서 자격 증명 이름을 선택하고, vCenter Server 인스턴스의 **IP 주소/FQDN** 을 지정합니다. **포트** 를 기본값(443)으로 그대로 두거나 vCenter Server에서 수신 대기하는 사용자 지정 포트를 지정하고, **저장** 을 클릭할 수 있습니다.
1. [저장]을 클릭하면 어플라이언스에서 제공된 자격 증명을 사용하여 vCenter Server에 대한 연결의 유효성을 검사하고, vCenter Server IP 주소/FQDN에 대한 **유효성 검사 상태** 를 테이블에 표시합니다.
1. 검색을 시작하기 전에 언제든지 vCenter Server에 대한 연결의 **유효성을 다시 검사** 할 수 있습니다.
1. **3단계: 설치된 애플리케이션을 검색하고 에이전트 없는 종속성 매핑을 수행하기 위한 VM 자격 증명 제공** 에서 **자격 증명 추가** 를 클릭하여 자격 증명을 제공하는 운영 체제, 자격 증명 이름, **사용자 이름** 및 **암호** 를 지정합니다. 그런 다음, **저장** 을 클릭합니다.

    - [애플리케이션 검색](how-to-discover-applications.md) 또는 [에이전트 없는 종속성 분석](how-to-create-group-machine-dependencies-agentless.md)에 사용할 계정을 만든 경우 필요에 따라 자격 증명을 여기에 추가합니다.
    - 이러한 기능을 사용하지 않으려면 슬라이더를 클릭하여 단계를 건너뛸 수 있습니다. 나중에 언제든지 의도를 되돌릴 수 있습니다.
    - [애플리케이션 검색](migrate-support-matrix-vmware.md#application-discovery-requirements) 또는 [에이전트 없는 종속성 분석](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)을 위해 계정에 필요한 권한을 검토합니다.

5. **검색 시작** 을 선택하여 VM 검색을 시작합니다. 검색이 성공적으로 시작되면 테이블에서 vCenter Server IP 주소/FQDN에 대한 검색 상태를 확인할 수 있습니다.

검색은 다음과 같이 작동합니다.
- 검색된 VM 메타데이터가 포털에 표시되는 데 약 15분이 걸립니다.
- 설치된 애플리케이션, 역할 및 기능을 검색하는 데 시간이 걸립니다. 기간은 검색되는 VM 수에 따라 달라집니다. 500개 VM의 경우 애플리케이션 인벤토리가 Azure Migrate 포털에 표시되는 데 약 1시간이 걸립니다.
- VM 검색이 완료되면 포털에서 원하는 VM에 대한 에이전트 없는 종속성 분석을 사용하도록 설정할 수 있습니다.


## <a name="next-steps"></a>다음 단계

- Azure VM으로 마이그레이션할 [VMware VM을 평가](./tutorial-assess-vmware-azure-vm.md)합니다.
- 검색 중에 어플라이언스에서 수집하는 [데이터를 검토](migrate-appliance.md#collected-data---vmware)합니다.