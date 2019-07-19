---
title: Azure NetApp Files에 대 한 NFS 볼륨 만들기 | Microsoft Docs
description: Azure NetApp Files에 대 한 NFS 볼륨을 만드는 방법을 설명 합니다.
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
ms.topic: conceptual
ms.date: 7/9/2019
ms.author: b-juche
ms.openlocfilehash: 06733103980086fad0975514ae3489c3652e428a
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846738"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Azure NetApp Files에 대한 NFS 볼륨 만들기

Azure NetApp Files는 NFS 및 SMBv3 볼륨을 지원 합니다. 볼륨의 용량 소비는 해당 풀의 프로비전된 용량에 대해 계산됩니다. 이 문서에서는 NFS 볼륨을 만드는 방법을 보여 줍니다. SMB 볼륨을 만들려면 [Azure NetApp Files에 대 한 smb 볼륨 만들기](azure-netapp-files-create-volumes-smb.md)를 참조 하세요. 

## <a name="before-you-begin"></a>시작하기 전 주의 사항 
용량 풀을 설정해야 합니다.   
[용량 풀 설정](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files에 서브넷을 위임해야 합니다.  
[Azure NetApp Files에 서브넷 위임](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>NFS 볼륨 만들기

1.  용량 풀 블레이드에서 **볼륨** 블레이드를 클릭 합니다. 

    ![볼륨으로 이동](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  **+ 볼륨 추가**를 클릭하여 볼륨을 만듭니다.  
    볼륨 만들기 창이 나타납니다.

3.  볼륨 만들기 창에서 **만들기** 를 클릭 하 고 다음 필드에 대 한 정보를 제공 합니다.   
    * **볼륨 이름**      
        만들고 있는 볼륨의 이름을 지정합니다.   

        볼륨 이름은 각 용량 풀 내에서 고유 해야 합니다. 3자 이상이어야 합니다. 모든 영숫자 문자를 사용할 수 있습니다.

    * **용량 풀**  
        볼륨을 만들 용량 풀을 지정 합니다.

    * **할당량**  
        볼륨에 할당되는 논리 저장소의 크기를 지정합니다.  

        **사용 가능한 할당량** 필드는 새 볼륨을 만들 때 사용할 수 있는 선택한 용량 풀에서 사용되지 않은 공간의 양을 보여줍니다. 새 볼륨의 크기는 사용 가능한 할당량을 초과해서는 안 됩니다.  

    * **가상 네트워크**  
        볼륨에 액세스하려는 Azure Vnet(가상 네트워크)을 지정합니다.  

        지정하는 VNet에는 Azure NetApp Files에 위임된 서브넷이 있어야 합니다. Azure NetApp Files 서비스는 동일한 VNet 또는 VNet 피어링을 통해 볼륨과 동일한 영역에 있는 VNet에서만 액세스할 수 있습니다. Express 경로를 통해 온-프레미스 네트워크에서 볼륨에 액세스할 수도 있습니다.   

    * **서브넷**  
        볼륨에 사용할 서브넷을 지정합니다.  
        지정하는 서브넷은 Azure NetApp Files에 위임되어야 합니다. 
        
        서브넷을 위임하지 않은 경우에는 볼륨 만들기 페이지에서 **새로 만들기**를 클릭할 수 있습니다. 그런 다음, 서브넷 만들기 페이지에서 서브넷 정보를 지정하고 **Microsoft.NetApp/volumes**를 선택하여 Azure NetApp Files의 서브넷을 위임합니다. 각 Vnet에서 하나의 서브넷만 Azure NetApp Files로 위임할 수 있습니다.   
 
        ![볼륨 만들기](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![서브넷 만들기](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. **프로토콜**을 클릭한 다음, **NFS**를 볼륨에 대한 프로토콜 유형으로 선택합니다.   
    * 새 볼륨의 내보내기 경로를 만드는 데 사용 될 **파일 경로** 를 지정 합니다. 내보내기 경로는 볼륨을 탑재하고 액세스하는 데 사용됩니다.

        파일 경로 이름에는 문자, 숫자 및 하이픈("-")만 포함할 수 있습니다. 이름은 16자~40자여야 합니다. 

        파일 경로는 각 구독 및 각 지역 내에서 고유 해야 합니다. 

    * 필요에 따라 [NFS 볼륨에 대 한 내보내기 정책을 구성 합니다](azure-netapp-files-configure-export-policy.md) .

    ![NFS 프로토콜 지정](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. **검토 + 만들기** 를 클릭 하 여 볼륨 세부 정보를 검토 합니다.  그런 다음 **만들기** 를 클릭 하 여 NFS 볼륨을 만듭니다.

    만든 볼륨이 볼륨 페이지에 표시 됩니다. 
 
    볼륨은 해당 용량 풀에서 구독, 리소스 그룹, 위치 특성을 상속합니다. 볼륨 배포 상태를 모니터링하려면 알림 탭을 사용할 수 있습니다.


## <a name="next-steps"></a>다음 단계  

* [Windows 또는 Linux 가상 머신에 대 한 볼륨 탑재 또는 분리](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [NFS 볼륨에 대한 내보내기 정책 구성](azure-netapp-files-configure-export-policy.md)
* [Azure NetApp Files에 대한 리소스 제한](azure-netapp-files-resource-limits.md)
* [Azure 서비스에 대한 가상 네트워크 통합에 대해 알아보기](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
