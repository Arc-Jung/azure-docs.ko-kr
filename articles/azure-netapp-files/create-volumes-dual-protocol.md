---
title: Azure NetApp Files에 대 한 이중 프로토콜 (NFSv3 및 SMB) 볼륨 만들기 Microsoft Docs
description: LDAP 사용자 매핑을 지 원하는 NFSv3 및 SMB의 이중 프로토콜을 사용 하는 볼륨을 만드는 방법에 대해 설명 합니다.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/28/2020
ms.author: b-juche
ms.openlocfilehash: 725f15d72694534da1fd924b3b6ffe223722fd3d
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100104470"
---
# <a name="create-a-dual-protocol-nfsv3-and-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files에 대 한 이중 프로토콜 (NFSv3 및 SMB) 볼륨 만들기

Azure NetApp Files에서는 NFS (NFSv3 및 NFSv 4.1), SMB3 또는 이중 프로토콜을 사용 하 여 볼륨을 만들 수 있습니다. 이 문서에서는 LDAP 사용자 매핑을 지 원하는 NFSv3 및 SMB의 이중 프로토콜을 사용 하는 볼륨을 만드는 방법을 보여 줍니다.  


## <a name="before-you-begin"></a>시작하기 전에 

* 용량 풀이 이미 만들어져 있어야 합니다.  
    [용량 풀 설정을](azure-netapp-files-set-up-capacity-pool.md)참조 하세요.   
* Azure NetApp Files에 서브넷을 위임해야 합니다.  
    [Azure NetApp Files에 서브넷 위임을](azure-netapp-files-delegate-subnet.md)참조 하세요.

## <a name="considerations"></a>고려 사항

* [Active Directory 연결에 대 한 요구 사항을](azure-netapp-files-create-volumes-smb.md#requirements-for-active-directory-connections)충족 하는지 확인 합니다. 
* DNS 서버에 역방향 조회 영역을 만든 다음 해당 역방향 조회 영역에 AD 호스트 컴퓨터의 포인터 (PTR) 레코드를 추가 합니다. 그렇지 않으면 이중 프로토콜 볼륨 만들기가 실패 합니다.
* NFS 클라이언트가 최신 상태이며 운영 체제에 대한 최신 업데이트를 실행 중인지 확인합니다.
* Ad (Active Directory) LDAP 서버가 AD에서 실행 중인지 확인 합니다. AD 컴퓨터에서 [Active Directory LDS(Lightweight Directory Services) (AD LDS)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831593(v=ws.11)) 역할을 설치 하 고 구성 하 여이 작업을 수행할 수 있습니다.
* 이중 프로토콜 볼륨은 현재 Azure Active Directory Domain Services (AADDS)를 지원 하지 않습니다.  
* 이중 프로토콜 볼륨에서 사용 하는 NFS 버전은 NFSv3입니다. 따라서 다음과 같은 고려 사항이 적용 됩니다.
    * 이중 프로토콜은 NFS 클라이언트의 Windows ACL 확장 특성을 지원 하지 않습니다 `set/get` .
    * NFS 클라이언트는 NTFS 보안 스타일에 대 한 권한을 변경할 수 없으며 Windows 클라이언트는 UNIX 스타일의 이중 프로토콜 볼륨에 대 한 권한을 변경할 수 없습니다.   

    다음 표에서는 보안 스타일 및 그에 대 한 영향을 설명 합니다.  
    
    | 보안 스타일    | 사용 권한을 수정할 수 있는 클라이언트   | 클라이언트에서 사용할 수 있는 사용 권한  | 결과 유효 보안 스타일    | 파일에 액세스할 수 있는 클라이언트     |
    |-  |-  |-  |-  |-  |
    | `Unix`    | NFS   | NFSv3 모드 비트   | UNIX  | NFS 및 Windows   |
    | `Ntfs`    | Windows   | NTFS Acl     | NTFS  |NFS 및 Windows|
* NFS를 사용 하 여 NTFS 보안 스타일 볼륨을 탑재 하는 UNIX 사용자는 `root` unix `root` 및 `pcuser` 기타 모든 사용자로 인증 됩니다. NFS를 사용 하는 경우 볼륨을 탑재 하기 전에 Active Directory에 이러한 사용자 계정이 존재 하는지 확인 합니다. 
* 토폴로지가 크고 `Unix` 확장 그룹이 있는 LDAP 또는 이중 프로토콜 볼륨이 포함 된 보안 스타일을 사용 하는 경우 Azure NetApp Files 토폴로지에서 모든 서버에 액세스 하지 못할 수 있습니다.  이러한 상황이 발생 하면 계정 팀에 문의 하 여 도움을 받으세요.  <!-- NFSAAS-15123 --> 
* 이중 프로토콜 볼륨을 만들기 위한 서버 루트 CA 인증서가 필요 하지 않습니다. TLS를 통한 LDAP를 사용 하는 경우에만 필요 합니다.


## <a name="create-a-dual-protocol-volume"></a>이중 프로토콜 볼륨 만들기

1.  용량 풀 블레이드에서 **볼륨** 블레이드를 클릭합니다. **+ 볼륨 추가** 를 클릭하여 볼륨을 만듭니다. 

    ![볼륨으로 이동](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png) 

2.  볼륨 만들기 창에서 **만들기** 를 클릭 하 고 기본 사항 탭에서 다음 필드에 대 한 정보를 제공 합니다.   
    * **볼륨 이름**      
        만들고 있는 볼륨의 이름을 지정합니다.   

        볼륨 이름은 각 용량 풀 내에서 고유 해야 합니다. 3자 이상이어야 합니다. 영숫자 문자를 사용할 수 있습니다.   

        `default`또는를 `bin` 볼륨 이름으로 사용할 수 없습니다.

    * **용량 풀**  
        볼륨을 만들 용량 풀을 지정합니다.

    * **할당량**  
        볼륨에 할당되는 논리 스토리지의 크기를 지정합니다.  

        **사용 가능한 할당량** 필드는 새 볼륨을 만들 때 사용할 수 있는 선택한 용량 풀에서 사용되지 않은 공간의 양을 보여줍니다. 새 볼륨의 크기는 사용 가능한 할당량을 초과해서는 안 됩니다.  

    * **처리량 (MiB/S)**   
        볼륨이 수동 QoS 용량 풀에 생성 되 면 볼륨에 대해 원하는 처리량을 지정 합니다.   

        볼륨이 자동 QoS 용량 풀에 생성 되는 경우이 필드에 표시 되는 값은 (할당량 x 서비스 수준 처리량)입니다.   

    * **가상 네트워크**  
        볼륨에 액세스하려는 Microsoft Azure Virtual Network(VNet)를 지정합니다.  

        지정하는 VNet에는 Azure NetApp Files에 위임된 서브넷이 있어야 합니다. Azure NetApp Files 서비스는 동일한 VNet 또는 VNet 피어링을 통해 볼륨과 동일한 영역에 있는 VNet에서만 액세스할 수 있습니다. Express Route를 통해 온-프레미스 네트워크에서 볼륨에 액세스할 수도 있습니다.   

    * **서브넷**  
        볼륨에 사용할 서브넷을 지정합니다.  
        지정하는 서브넷은 Azure NetApp Files에 위임되어야 합니다. 
        
        서브넷을 위임하지 않은 경우에는 볼륨 만들기 페이지에서 **새로 만들기** 를 클릭할 수 있습니다. 그런 다음, 서브넷 만들기 페이지에서 서브넷 정보를 지정하고 **Microsoft.NetApp/volumes** 를 선택하여 Azure NetApp Files의 서브넷을 위임합니다. 각 Vnet에서 하나의 서브넷만 Azure NetApp Files로 위임할 수 있습니다.   
 
        ![볼륨 만들기](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![서브넷 만들기](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * 볼륨에 기존 스냅숏 정책을 적용 하려면 **고급 섹션 표시** 를 클릭 하 여 확장 하 고, 스냅숏 경로를 숨길지 여부를 지정 하 고, 풀 다운 메뉴에서 스냅숏 정책을 선택 합니다. 

        스냅숏 정책을 만드는 방법에 대 한 자세한 내용은 [스냅숏 정책 관리](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies)를 참조 하세요.

        ![고급 선택 표시](../media/azure-netapp-files/volume-create-advanced-selection.png)

3. **프로토콜** 을 클릭한 후, 다음 작업을 완료합니다.  
    * 볼륨에 대 한 프로토콜 유형으로 **이중 프로토콜 (NFSv3 및 SMB)** 을 선택 합니다.   

    * 볼륨에 대 한 **볼륨 경로** 를 지정 합니다.   
    이 볼륨 경로는 공유 볼륨의 이름입니다. 이름은 알파벳 문자로 시작 해야 하 고 각 구독 및 각 지역 내에서 고유 해야 합니다.  

    * 사용할 **보안 스타일** 을 NTFS (기본값) 또는 UNIX로 지정 합니다.

    * 필요에 따라 [볼륨에 대 한 내보내기 정책을 구성](azure-netapp-files-configure-export-policy.md)합니다.

    ![이중 프로토콜 지정](../media/azure-netapp-files/create-volume-protocol-dual.png)

4. **검토 + 만들기** 를 클릭하여 볼륨 정보를 검토합니다. 그런 다음 **만들기** 를 클릭 하 여 볼륨을 만듭니다.

    만든 볼륨이 볼륨 페이지에 표시됩니다. 
 
    볼륨은 해당 용량 풀에서 구독, 리소스 그룹, 위치 특성을 상속합니다. 볼륨 배포 상태를 모니터링하려면 알림 탭을 사용할 수 있습니다.

## <a name="manage-ldap-posix-attributes"></a>LDAP POSIX 특성 관리

Active Directory 사용자 및 컴퓨터 MMC 스냅인을 사용 하 여 UID, 홈 디렉터리 및 기타 값과 같은 POSIX 특성을 관리할 수 있습니다.  다음 예에서는 Active Directory 특성 편집기를 보여 줍니다.  

![Active Directory 특성 편집기](../media/azure-netapp-files/active-directory-attribute-editor.png) 

LDAP 사용자 및 LDAP 그룹에 대해 다음 특성을 설정 해야 합니다. 
* LDAP 사용자에 대 한 필수 특성:   
    `uid`: Alice, `uidNumber` : 139, `gidNumber` : 555, `objectClass` : posixAccount
* LDAP 그룹에 필요한 특성:   
    `objectClass`: "posixGroup", `gidNumber` : 555

## <a name="configure-the-nfs-client"></a>NFS 클라이언트 구성 

Nfs 클라이언트를 구성 하려면 [Azure NetApp Files에 대 한 nfs 클라이언트 구성](configure-nfs-clients.md) 의 지침을 따르세요.  

## <a name="next-steps"></a>다음 단계  

* [Azure NetApp Files에 대한 NFS 클라이언트 구성](configure-nfs-clients.md)
* [SMB 또는 이중 프로토콜 볼륨 문제 해결](troubleshoot-dual-protocol-volumes.md)
