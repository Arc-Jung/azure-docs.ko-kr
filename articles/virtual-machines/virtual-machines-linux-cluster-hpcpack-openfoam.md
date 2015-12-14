<properties
 pageTitle="Linux VM에서 HPC 팩으로 OpenFOAM 실행 | Microsoft Azure"
 description="Azure에서 Microsoft HPC Pack 클러스터를 배포하고 RDMA 네트워크 간의 여러 Linux 계산 노드에서 OpenFOAM 작업을 실행합니다."
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="11/25/2015"
 ms.author="danlep"/>

# Azure의 Linux RDMA 클러스터에서 Microsoft HPC 팩을 사용하여 OpenFoam 실행

이 문서에서는 Azure에 Microsoft HPC 팩 클러스터를 배포하고 Azure RDMA(원격 직접 메모리 액세스) 네트워크 간을 연결하는 여러 Linux 계산 노드에서 Intel MPI를 사용하여 [OpenFoam](http://openfoam.com/) 작업을 실행하는 방법을 보여 줍니다.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]리소스 관리자 모델.

OpenFOAM(오픈 필드 작업 및 조작에 대한)은 상업용 및 연구용 조직의 공학 및 과학에서 널리 사용되는 무료로 사용할 수 있는 오픈 소스 컴퓨팅 유체 역학(CFD) 소프트웨어 패키지입니다. Meshing, 특히 snappyHexMesh, 복잡한 CAD 기하학 및 전처리 및 후처리에 대한 병렬화된 메셔에 대한 도구를 포함합니다. 거의 모든 프로세스는 마음껏 컴퓨터 하드웨어를 모두 활용하도록 병렬로 실행합니다.

Microsoft HPC 팩에서는 MPI 응용 프로그램을 포함한 다양한 대규모 HPC 및 병렬 응용 프로그램을 Microsoft Azure 가상 컴퓨터의 클러스터에서 실행하는 기능을 제공합니다. Microsoft HPC 팩 2012 R2 업데이트 2부터는 HPC 팩에서 HPC 팩 클러스터에 배포된 Linux 계산 노드 VM에서 Linux HPC 응용 프로그램의 실행도 지원합니다. HPC 팩으로 Linux 계산 노드 사용에 대한 소개는 [Azure에서 HPC 팩 클러스터의 Linux 계산 노드 시작](virtual-machines-linux-cluster-hpcpack.md)을 참조하세요.

>[AZURE.NOTE]이 문서는 Linux 시스템 관리 및 Linux HPC 클러스터에서 실행 중인 MPI 작업에 대해 잘 알고 있다고 가정합니다.

## 필수 조건

*   **Linux 계산 노드가 포함된 HPC 팩 클러스터** - Azure 마켓플레이스에서 Azure PowerShell 스크립트 및 HPC 팩 이미지를 사용하여 Azure에서 Linux 계산 노드와 함께 HPC 팩 클러스터를 배포하는 필수 조건 및 단계는 [Azure에서 HPC 팩 클러스터의 Linux 계산 노드 시작](virtual-machines-linux-cluster-hpcpack.md)을 참조하세요. A8 계산 집약적 인스턴스를 사용하여 Azure RDMA 네트워크에 액세스하기 위한 추가 고려 사항에 대해 [A8, A9, A10 및 A11 계산 집약적 인스턴스 정보](virtual-machines-a8-a9-a10-a11-specs.md)를 참조하세요.

    다음은 A8 크기 Windows Server 2012 R2 헤드 노드 1개 및 A8 크기 SUSE Linux Enterprise Server 12 계산 노드 2개로 구성되는 Azure 기반 HPC 팩 클러스터를 배포하는 스크립트와 함께 사용하는 샘플 XML 구성 파일입니다. 구독 및 서비스 이름을 적절한 값으로 대체합니다.

    >[AZURE.NOTE]현재 Azure의 Linux RDMA 네트워킹은 Azure 마켓플레이스의 RDMA를 사용하는 SUSE Linux Enterprise Server 12 이미지에서 만든 VM에 대해서만 지원됩니다(b4590d9e3ed742e4a1d46e5424aa335e\_\_suse-sles-12-hpc-v20150708).

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionName>Subscription-1</SubscriptionName>
        <StorageAccount>allvhdsje</StorageAccount>
      </Subscription>
      <Location>Japan East</Location>  
      <VNet>
        <VNetName>suse12rdmavnet</VNetName>
        <SubnetName>SUSE12RDMACluster</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpclab.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>SUSE12RDMA-HN</VMName>
        <ServiceName>suse12rdma-je</ServiceName>
        <VMSize>A8</VMSize>
        <EnableRESTAPI />
        <EnableWebPortal />
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
        <ServiceName>suse12rdma-je</ServiceName>
        <VMSize>A8</VMSize>
        <NodeCount>2</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>
```

    **알아야 할 추가 사항**

    *   노드 간에 RDMA 네트워크 연결을 사용하도록 한 클라우드 서비스 내에서 모든 Linux 계산 노드를 배포합니다.

    *   Linux 노드를 배포한 후 추가 관리 작업을 수행하도록 SSH로 연결해야 하는 경우 Azure 포털에서 각 Linux VM에 대한 SSH 연결 상세 정보를 찾습니다.
        
*   **Intel MPI** - Azure의 Linux 계산 노드에서 OpenFOAM를 실행하려면 [Intel.com 사이트](https://software.intel.com/ko-KR/intel-mpi-library/)에서 Intel MPI Library 5 런타임이 필요합니다. 이후 단계에서 Linux 계산 노트에 Intel MPI를 설치합니다. 이를 준비하려면 Intel에 등록한 후 확인 전자 메일의 링크를 따라 관련 웹 페이지로 이동하여 적절한 Intel MPI 버전의 .tgz 파일에 대한 다운로드 링크를 복사합니다. 이 문서는 Intel MPI 5.0.3.048 버전을 기반으로 합니다.

*   **OpenFOAM 소스 팩** - [OpenFOAM Foundation 사이트](http://www.openfoam.org/download/source.php)에서 Linux용 OpenFOAM 소스 팩 소프트웨어를 다운로드합니다. 이 문서는 OpenFOAM-2.3.1.tgz로 다운로드할 수 있는 소스 팩 버전 2.3.1을 기반으로 합니다. Linux 계산 노드에서 OpenFOAM의 압축을 풀고 컴파일하려면 이 문서의 뒷부분에 나오는 지침을 따릅니다.

*   **EnSight**(선택 사항) - OpenFOAM 시뮬레이션의 결과를 확인하려면 HPC Pack 클러스터의 헤드 노드에서 [EnSight](https://www.ceisoftware.com/download/) 시각화 및 분석 프로그램의 Windows 버전을 다운로드 및 설치합니다. 라이선스 및 다운로드 정보는 EnSight 사이트에 있습니다.


## 계산 노드 간 상호 트러스트 설정

여러 Linux 노드에서 크로스 노드 작업을 실행하려면 노드가 서로 신뢰해야 합니다(**rsh** 또는 **ssh** 사용). Microsoft HPC 팩 IaaS 배포 스크립트로 HPC 팩 클러스터를 만드는 경우 스크립트에서 사용자가 지정한 관리자 계정에 대해 영구 상호 트러스트를 자동으로 설정합니다. 클러스터의 도메인에 만든 관리자가 아닌 사용자의 경우 작업이 할당될 때 노드 간에 임시 상호 트러스트를 설정하고 작업이 완료된 후 관계를 삭제해야 합니다. 사용자마다 이 작업을 수행하려면 HPC 팩에서 트러스트 관계를 설정하는 데 사용할 클러스터에 RSA 키 쌍을 제공합니다.

### RSA 키 쌍 생성

Linux **ssh-keygen** 명령을 실행하여 공개 키 및 개인 키를 포함하는 RSA 키 쌍을 간편하게 생성할 수 있습니다.

1.	Linux 컴퓨터에 로그온합니다.

2.	다음 명령을 실행합니다.

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE]명령이 완료될 때까지 기본 설정을 사용하려면 **Enter** 키를 누릅니다. 여기에 암호를 입력하지 마십시오. 암호를 묻는 메시지가 표시되면 **Enter** 키만 누르십시오.

    ![RSA 키 쌍 생성][keygen]

3.	디렉터리를 ~/.ssh 디렉터리로 변경합니다. 개인 키는 id\_rsa에 저장되고 공개 키는 id\_rsa.pub에 저장됩니다.

    ![개인 및 공용 키][keys]

### HPC 팩 클러스터에 키 쌍 추가
1.	HPC 팩 관리자 계정(배포 스크립트를 실행 할 때 설정한 관리자 계정)으로 헤드 노드에 원격 데스크톱 연결을 만듭니다.

2. 표준 Windows Server 절차를 사용하여 클러스터의 Active Directory 도메인에 도메인 사용자 계정을 만듭니다. 예를 들어 헤드 노드에서 Active Directory 사용자 및 컴퓨터 도구를 사용합니다. 이 문서의 예에서는 hpclab\\hpcuser라는 이름의 도메인 사용자를 만든다고 가정합니다.

3.	C:\\cred.xml이라는 이름의 파일을 만들고 RSA 키 데이터를 여기에 복사합니다. 이 파일에 대한 예는 이 문서 끝에 있는 샘플 파일에서 확인할 수 있습니다.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.	명령 프롬프트를 열고 다음 명령을 입력하여 hpclab\\hpcuser 계정에 대한 자격 증명 데이터를 설정합니다. 키 데이터를 위해 만든 C:\\cred.xml 파일의 이름을 전달하는 데 **extendeddata** 매개 변수를 사용합니다.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    이 명령은 출력 없이 성공적으로 완료됩니다. 작업을 실행하는 데 필요한 사용자의 자격 증명을 설정한 후에 cred.xml 파일을 안전한 위치에 저장하거나 삭제합니다.

5.	Linux 노드 중 하나에서 RSA 키 쌍을 생성한 경우 사용이 끝난 후 키를 삭제해야 합니다. HPC 팩에서는 기존 id\_rsa 파일 또는 id\_rsa.pub 파일이 발견된 경우 상호 트러스트를 설정하지 않습니다.

>[AZURE.IMPORTANT]공유 클러스터에서 Linux 작업을 클러스터 관리자로 실행하지 않는 것이 좋습니다. 관리자가 제출한 작업이 Linux 노드에서 루트 계정으로 실행되기 때문입니다. 관리자가 아닌 사용자가 제출한 작업은 작업 사용자와 동일한 이름의 로컬 Linux 사용자 계정으로 실행되며 HPC 팩은 작업에 할당된 모든 노드에 걸쳐 이 Linux 사용자에 대해 상호 트러스트를 설정합니다. 작업을 실행하기 전에 Linux 노드에서 Linux 사용자를 수동으로 설정할 수 있습니다. 또는 HPC 팩에서 작업이 제출될 때 사용자를 자동으로 만듭니다. HPC 팩에서 사용자를 만드는 경우 HPC 팩은 작업이 완료된 후 사용자를 삭제합니다. 노드에서 작업이 완료된 후 키가 제거되므로 보안 위협이 줄어듭니다.

## 사용자가 액세스할 파일 공유를 설정합니다.

이제 헤드 노드에서 폴더에 대한 표준 SMB 공유를 설정하고 모든 Linux 노드에서 공유 폴더를 탑재하여 Linux 노드에서 일반 경로로 응용 프로그램 파일에 액세스할 수 있도록 합니다. 원하는 경우 여러 시나리오에서 권장되는 Azure 파일 공유 또는 NFS 공유와 같은 다른 파일 공유 옵션을 사용할 수 있습니다. [Azure에서 HPC 팩 클러스터의 Linux 계산 노드 시작](virtual-machines-linux-cluster-hpcpack.md)의 파일 공유 정보 및 자세한 단계를 참조하세요.

1.	헤드 노드에서 폴더를 만들고 읽기/쓰기 권한을 설정하여 모든 사용자에게 공유합니다. 예를 들어 헤드 노드의 C:\\OpenFOAM을 \\SUSE12RDMA-HN\\OpenFOAM으로 공유합니다. 여기에서 *SUSE12RDMA-HN*은 헤드 노드의 호스트 이름입니다.

2.	Windows PowerShell 창을 열고 다음 명령을 실행하여 공유 폴더를 탑재합니다.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>’`,dir_mode=0777`,file_mode=0777
    ```

첫 번째 명령은 LinuxNodes 그룹의 모든 노드에 /openfoam이라는 폴더를 만듭니다. 두 번째 명령은 dir\_mode 및 file\_mode 비트를 777로 설정하여 공유 폴더 //SUSE12RDMA-HN/OpenFOAM을 Linux 노드에 탑재합니다. 명령의 *username*과 *password*는 헤드 노드 사용자의 자격 증명이어야 합니다.

>[AZURE.NOTE]두 번째 명령의 "`" 기호는 PowerShell의 이스케이프 기호입니다. "`,"는 ","(쉼표)가 명령의 일부임을 의미합니다.

## MPI 및 OpenFOAM 설치

RDMA 네트워크에 MPI 작업으로 OpenFOAM을 실행하려면 Intel MPI 라이브러리를 사용하여 OpenFOAM을 컴파일해야 합니다.

먼저 여러 **clusrun** 명령을 실행하여 모든 Linux 노드에 Intel MPI 라이브러리 및 OpenFOAM을 설치합니다. 이전에 구성된 헤드 노드 공유를 사용하여 Linux 노드 간에 설치 파일을 공유합니다.

>[AZURE.IMPORTANT]이러한 설치 및 컴파일 단계는 예제이며 Linux 시스템 관리 특히 종속 컴파일러 및 라이브러리가 올바르게 설치되었는지 확인하도록 일부 지식이 필요합니다. 특정 환경 변수 또는 Intel MPI 및 OpenFOAM 버전에 필요한 기타 설정을 수정해야 합니다. 자세한 내용은 [Linux 설치 가이드용 Intel MPI Library](http://scc.ustc.edu.cn/zlsc/tc4600/intel/impi/INSTALL.html) 및 [OpenFOAM 소스 팩 설치](http://www.openfoam.org/download/source.php)를 참조하세요.


### Intel MPI 설치

Intel MPI에 대해 다운로드된 설치 패키지(이 예제에서 l\_mpi\_p\_5.0.3.048.tgz)를 헤드 노드의 C:\\OpenFoam에 저장하여 Linux 노드가 /openfoam에서 이 파일에 액세스할 수 있도록 합니다. 그런 다음 **clusrun**을 실행하여 모든 Linux 노드에 Intel MPI Library를 설치합니다.

1.  다음 명령은 설치 패키지를 복사하고 각 노드의 /opt/intel에 압축을 풉니다.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Intel MPI Library를 자동으로 설치하려면 silent.cfg 파일을 사용합니다. 이 문서의 끝에 있는 샘플 파일에서 예제를 확인할 수 있습니다. 이 파일을 공유 폴더 /openfoam에 넣습니다. silent.cfg 파일에 대한 자세한 내용은 [Linux 설치 가이드용 Intel MPI Library - 자동 설치](http://scc.ustc.edu.cn/zlsc/tc4600/intel/impi/INSTALL.html#silentinstall)를 참조하세요.

    >[AZURE.TIP]silent.cfg 파일을 Linux 줄 끝(CR LF가 아닌 LF만)을 사용하여 텍스트 파일로 저장합니다. 이렇게 해야 Linux 노드에서 제대로 실행됩니다.

3.  자동 모드에서 Intel MPI Library를 설치합니다.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### MPI 구성

테스트를 위해 각 Linux 노드에서 /etc/security/limits.conf에 다음 줄을 추가해야 합니다.

```
*               hard    memlock         unlimited
*               soft    memlock         unlimited
```

limits.conf 파일을 업데이트한 후 Linux 노드를 다시 시작합니다. 예를 들어 다음 **clusrun** 명령을 사용합니다.

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

다시 시작한 후 공유 폴더가 /openfoam으로 탑재되었는지 확인합니다.

### OpenFOAM 컴파일 및 설치

OpenFOAM 소스 팩에 대해 다운로드된 설치 패키지(이 예제에서 OpenFOAM-2.3.1.tgz)를 헤드 노드의 C:\\OpenFoam에 저장하여 Linux 노드가 /openfoam에서 이 파일에 액세스할 수 있도록 합니다. 그런 다음 **clusrun**을 실행하여 모든 Linux 노드의 OpenFOAM을 컴파일합니다.


1.  각 Linux 노드에서 폴더 /opt/OpenFOAM을 만들고 이 폴더에 소스 패키지를 복사하고 거기에 압축을 풉니다.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Intel MPI Library와 함께 OpenFOAM을 컴파일하려면 먼저 Intel MPI 및 OpenFOAM에 대한 몇 가지 환경 변수를 설정합니다. settings.sh라는 bash 스크립트를 사용하여 이 작업을 수행합니다. 이 문서의 끝에 있는 샘플 파일에서 예제를 확인할 수 있습니다. 공유 폴더 /openfoam에 이 파일(Linux 줄 끝으로 저장됨)을 저장합니다. 이 파일에는 OpenFOAM 작업을 실행하기 위해 나중에 사용하는 MPI 및 OpenFOAM 런타임에 대한 설정이 포함되어 있습니다.

3. OpenFOAM을 컴파일하는 데 필요한 종속 패키지를 설치합니다. Linux 배포에 따라 리포지토리를 먼저 추가해야 할 수 있습니다. 다음과 유사하게 **clusrun** 명령을 실행합니다.

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    필요한 경우 제대로 실행하는지 확인하도록 각 Linux 노드에 ssh를 사용하여 명령을 실행하는 것이 좋습니다.

4.  다음 명령을 실행하여 OpenFOAM을 컴파일합니다. 컴파일 프로세스를 완료하는 데 시간이 걸리며 표준 출력으로 많은 양의 로그 정보를 생성하므로 **/interleaved** 옵션을 사용하여 출력 인터리브를 표시합니다.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]명령의 "'" 기호는 PowerShell의 이스케이프 기호입니다. “`&”는 “&”가 명령의 일부임을 의미합니다.

## OpenFOAM 작업 실행 준비

이제 2개의 Linux 노드에서 OpenFoam 샘플 중 하나인 sloshingTank3D라는 MPI 작업을 실행할 준비를 합니다.

### 런타임 환경 설정

모든 Linux 노드에서 MPI 및 OpenFOAM에 대한 런타임 환경을 설정하려면 헤드 노드의 Windows PowerShell 창에서 다음 명령을 실행합니다. (이 명령은 SUSE Linux에 대해서만 유효합니다.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### 샘플 데이터 준비

이전에 구성된 헤드 노드 공유를 사용하여 Linux 노드 간에 파일을 공유합니다(/openfoam으로 탑재됨).

1.  SSH에서 Linux 계산 노드 중 하나로.

2.  아직 수행하지 않은 경우 다음 명령을 실행하여 OpenFOAM 런타임 환경 설정을 설정합니다.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  sloshingTank3D 샘플을 공유 폴더에 복사하고 공유 폴더로 이동합니다.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  이 샘플의 기본 매개 변수를 사용하면 실행하는 데 많은 시간이 걸리므로 일부 매개 변수를 수정하여 더 빨리 실행하도록 할 수 있습니다. 하나의 간단한 선택은 시간 및 읽기 및 쓰기 솔루션 데이터의 제어와 관련된 모든 입력 데이터를 저장하는 시스템/controlDict 파일의 시간 단계 변수 deltaT 및 writeInterval을 수정하는 것입니다. 예를 들어 deltaT의 값을 0.05에서 0.5로 writeInterval의 값을 0.05에서 0.5로 변경할 수 있습니다.

    ![단계 변수 수정][step_variables]

5.  시스템/decomposeParDict 파일에서 변수에 대해 원하는 값을 지정합니다. 이 예제에서는 8개의 코어로 각각 2개의 Linux 노드를 사용하므로 numberOfSubdomains를 16으로 hierarchicalCoeffs의 n을 (1 1 16)으로 설정합니다. 이는 16개의 프로세스와 함께 병렬로 OpenFOAM을 실행하는 것을 의미합니다. OpenFOAM을 병렬로 실행하는 방법에 대한 자세한 내용은 [OpenFOAM 사용자 가이드: 3.4 병렬로 응용 프로그램 실행](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4)을 참조하세요.

    ![프로세스 분해][decompose]

6.  sloshingTank3D 디렉터리에서 다음 명령을 실행하여 샘플 데이터를 준비합니다.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  헤드 노드에 샘플 데이터 파일이 C:\\OpenFoam\\sloshingTank3D로 복사된 것을 확인할 수 있습니다. (C:\\OpenFoam은 헤드 노드의 공유 폴더입니다.)

    ![헤드 노드의 데이터 파일][data_files]

### mpirun에 대한 호스트 파일

이 단계에서 **mpirun** 명령이 사용하는 호스트 파일(계산 노드 목록)을 만듭니다.

1.	Linux 노드 중 하나에서 /openfoam 아래에 hostfile이라는 새 파일을 만들면 이 파일은 모든 Linux 노드에서 /openfoam/hostfile에 연결할 수 있습니다.

2.	Linux 노드 이름을 이 파일에 작성합니다. 이 예제에서 파일은 다음과 같이 표시됩니다.
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]또한 이 파일을 헤드 노드의 C:\\OpenFoam\\hostfile에 만들 수 있습니다. 이를 실행하는 경우 Linux 줄 끝(CR LF가 아닌 LF만)을 사용하여 텍스트 파일로 저장합니다. 이렇게 해야 Linux 노드에서 제대로 실행됩니다.

    **Bash 스크립트 래퍼**

    많은 Linux 노드가 있고 그 중 일부에 대해 작업이 실행되는 경우 어떤 노드가 작업에 할당되는지 모르므로 고정된 호스트 파일을 사용하는 것은 좋지 않습니다. 이 경우 **mpirun**에 대한 bash 스크립트 래퍼를 작성하여 호스트 파일을 자동으로 만듭니다. 이 문서 끝에 있는 샘플 파일에서 hpcimpirun.sh라는 예제 bash 스크립트 래퍼를 찾고 /openfoam/hpcimpirun.sh로 저장할 수 있습니다. 이 예제 스크립트에서는 다음을 수행합니다.

    1.	**mpirun** 및 일부 추가 명령 매개 변수에 대한 환경 변수를 설정하여 RDMA 네트워크를 통해 MPI 작업을 실행합니다. 이 경우 다음을 설정합니다.

        *	I\_MPI\_FABRICS=shm:dapl
        *	I\_MPI\_DAPL\_PROVIDER=ofa-v2-ib0
        *	I\_MPI\_DYNAMIC\_CONNECTION=0

    2.	작업이 활성화될 때 HPC 헤드 노드로 설정되는 환경 변수 $CCP\_NODES\_CORES에 따라 호스트 파일을 만듭니다.

        $CCP\_NODES\_CORES의 형식은 이 패턴을 따릅니다.

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        여기서,

        * `<Number of nodes>`: 이 작업에 할당된 노드 수입니다.  
        
        * `<Name of node_n_...>`: 이 작업에 할당된 각 노드의 이름입니다.
        
        * `<Cores of node_n_...>`: 이 작업에 할당된 노드의 코어 수입니다.

        예를 들어 작업에 실행할 노드가 2개 필요한 경우 $CCP\_NODES\_CORES는 다음과 유사합니다.
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.	**mpirun** 명령을 호출하고 명령줄에 2개의 매개 변수를 추가합니다.

        * `--hostfile <hostfilepath>: <hostfilepath>` - 스크립트가 만드는 호스트 파일의 경로입니다.

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - 이 작업에 할당된 총 코어 수를 저장하는 HPC 팩 헤드 노드로 설정된 환경 변수입니다. 이 경우 **mpirun**에 대한 프로세스의 수를 지정합니다.


## OpenFOAM 작업 제출

이제 HPC 클러스터 관리자에서 작업을 제출할 수 있습니다. 작업 중 일부에 대한 명령줄의 스크립트 hpcimpirun.sh를 전달해야 합니다.

1. 클러스터 헤드 노드에 연결하고 HPC 클러스터 관리자를 시작합니다.

2. **리소스 관리에서** Linux 계산 노드가 **온라인** 상태인지 확인합니다. 온라인 상태가 아닌 경우 노드를 선택하고 **온라인 상태로 전환**을 클릭합니다.

3.  **작업 관리**에서 **새 작업**을 클릭합니다.

4.  작업 이름(예: _sloshingTank3D_)을 입력합니다.

    ![작업 세부 정보][job_details]

5.	**작업 리소스**에서 리소스 유형을 “노드”로 선택하고 최소를 2로 설정합니다. 이 예에서는 Linux 노드 2개에 작업을 실행하며 각각은 코어 8개를 포함합니다.

    ![작업 리소스][job_resources]

6.	작업에 대해 다음 명령줄 및 설정을 사용하여 작업에 4개의 작업을 추가합니다.

    >[AZURE.NOTE]`source /openfoam/settings.sh` 실행으로 OpenFOAM 및 MPI 런타임 환경을 설정하면 다음 각 작업은 OpenFOAM 명령 전에 호출합니다.

    *   **작업 1**. **decomposePar**를 실행하여 병렬로 **interDyMFoam** 실행에 대한 데이터 파일을 생성합니다.
    
        *   작업에 하나의 노드 할당

        *   **명령줄** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **작업 디렉터리** -/ openfoam/sloshingTank3D
        
        다음 그림을 참조하세요. 나머지 작업을 비슷하게 구성합니다.

        ![작업 1 세부 정보][task_details1]

    *   **작업 2**. 병렬로 **interDyMFoam**을 실행하여 샘플을 계산합니다.

        *   작업에 2개의 노드 할당

        *   **명령줄** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **작업 디렉터리** -/ openfoam/sloshingTank3D

    *   **작업 3**. **reconstructPar**를 실행하여 각 processor\_N\_ 디렉터리에서 시간 디렉터리 단일 집합으로 시간 디렉터리의 집합을 병합합니다.

        *   작업에 하나의 노드 할당

        *   **명령줄** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **작업 디렉터리** -/ openfoam/sloshingTank3D

    *   **작업 4**. 병렬로 **foamToEnsight**를 실행하여 OpenFOAM 결과 파일을 EnSight 형식으로 변환하고 사례 디렉터리의 EnSight라는 디렉터리에 EnSight 파일을 놓습니다.

        *   작업에 2개의 노드 할당

        *   **명령줄** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **작업 디렉터리** -/ openfoam/sloshingTank3D

6.	오름차순 작업 순서로 이러한 작업에 종속성을 추가합니다.

    ![작업 종속성][task_dependencies]

7.	**제출**을 클릭하여 이 작업을 실행합니다.

    기본적으로 HPC 팩은 현재 로그온한 사용자 계정으로 작업을 제출합니다. **제출**을 클릭한 후 사용자 이름 및 암호를 입력하라는 대화 상자가 표시될 수 있습니다.

    ![작업 자격 증명][creds]

    일부 조건에서 HPC 팩이 사용자가 이전에 입력한 사용자 정보를 저장해 두어 이 대화 상자가 표시되지 않습니다. HPC 팩에서 이 대화 상자를 다시 표시하려면 명령 프롬프트 창에 다음을 입력한 후 작업을 제출합니다.

    ```
    hpccred delcreds
    ```

8.	샘플에 대해 설정한 매개 변수에 따라 수십 분에서 몇 시간까지 소요됩니다. 열 지도에 2개의 Linux 노드에서 실행 중인 작업이 표시됩니다.

    ![열 지도][heat_map]

    각 노드에서 8개의 프로세스가 시작됩니다.

    ![Linux 프로세스][linux_processes]

9.  작업이 완료되면 C:\\OpenFoam\\sloshingTank3D 아래의 폴더에서 작업 결과 및 C:\\OpenFoam에서 로그 파일을 찾습니다.


## EnSight에서 결과 보기

필요에 따라 [EnSight](https://www.ceisoftware.com/)를 사용하여 OpenFOAM 작업의 결과를 시각화하고 분석합니다. EnSight에서 시각화 및 애니메이션에 대한 자세한 내용은 이 [비디오 가이드](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html)를 참조하세요.

1.  헤드 노드에서 EnSight를 설치한 후 시작합니다.

2.  C:\\OpenFoam\\sloshingTank3D\\EnSight\\sloshingTank3D.case를 엽니다.

    뷰어에서 탱크를 볼 수 있습니다.

    ![EnSight의 탱크][tank]

3.	**internalMesh**에서 **Isosurface**를 만든 다음 변수 **alpha\_water**를 선택합니다.

    ![isosurface 만들기][isosurface]

4.	이전 단계에서 만든 **Isosurface\_part**에 대한 색을 설정합니다. 예를 들어 워터 블루로 설정합니다.

    ![Isosurface 색상 편집][isosurface_color]

5.  **부품** 패널에서 **벽**을 선택하여 **벽**에서 **Iso-volume**을 만들고 도구 모음에서 **Isosurfaces** 단추를 클릭합니다.

6.	대화 상자에서 **Isovolume**으로 **형식**을 선택하고 **Isovolume 범위**의 최소를 0.5로 설정합니다. **Create with selected parts**(선택한 부품으로 만들기)를 클릭하여 isovolume을 만듭니다.

7.	이전 단계에서 만든 **Iso\_volume\_part**에 대한 색을 설정합니다. 예를 들어 딥 워터 블루로 설정합니다.

8.	**벽**에 대한 색을 설정합니다. 예를 들어 투명 흰색으로 설정합니다.

9. 이제 **재생**을 클릭하여 시뮬레이션의 결과를 확인합니다.

    ![탱크 결과][tank_result]

## 샘플 파일


### 샘플 cred.xml 파일

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### 샘플 silent.cfg 파일

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no

```

### 샘플 settings.sh 스크립트

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###샘플 hpcimpirun.sh 스크립트

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
	# CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
	${MPIRUN} $*
else
	# Create the hostfile file
	NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

	# Get every node name and write into the hostfile file
	I=1
	while [ ${I} -lt ${COUNT} ]
	do
		echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
		let "I=${I}+2"
	done

	# Run the mpirun with hostfile arg
	${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

	RTNSTS=$?
	rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/linux_processes.png

<!---HONumber=AcomDC_1203_2015-->