---
title: 개념-Id 및 액세스
description: Azure VMware 솔루션의 id 및 액세스 개념에 대해 알아보기
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 68f4ce9136cca1cf9bf0824395e31704d8ed1a17
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100364888"
---
# <a name="azure-vmware-solution-identity-concepts"></a>Azure VMware 솔루션 id 개념

Azure VMware 솔루션 사설 클라우드는 vCenter server 및 NSX Manager를 사용 하 여 프로 비전 됩니다. VCenter를 사용 하 여 VM (가상 컴퓨터) 워크 로드를 관리 합니다. NSX-T 관리자를 사용 하 여 사설 클라우드를 확장 합니다.

액세스 및 id 관리에서는 NSX-T 관리자에 대 한 vCenter 및 제한 된 관리자 권한으로 CloudAdmin 그룹 권한을 사용 합니다. 이를 통해 사설 클라우드 플랫폼은 최신 기능과 패치를 사용 하 여 자동으로 업그레이드 됩니다.  자세한 내용은 [사설 클라우드 업그레이드 개념 문서][concepts-upgrades]를 참조 하세요.

## <a name="vcenter-access-and-identity"></a>vCenter 액세스 및 id

CloudAdmin 그룹은 vCenter에서 권한을 제공 합니다. VCenter에서 로컬로 그룹을 관리 합니다. 또 다른 옵션은 vCenter LDAP Single Sign-On Azure Active Directory 통합 하는 것입니다. 사설 클라우드를 배포한 후이 통합을 사용 하도록 설정 합니다. 

이 표에는 **cloudadmin** 및 **cloudglobaladmin** 권한이 나와 있습니다.

|  권한 집합           | CloudAdmin | CloudGlobalAdmin | 주석 |
| :---                     |    :---:   |       :---:      |   :--:  |
|  알람                  | CloudAdmin 사용자는 Compute-ResourcePool 및 Vm에서 알람에 대 한 모든 경보 권한을 가집니다.     |          --        |  -- |
|  자동 배포             |  --  |        --        |  Microsoft는 호스트 관리를 수행 합니다.  |
|  인증서            |  --  |        --       |  Microsoft는 인증서 관리를 수행 합니다.  |
|  콘텐츠 라이브러리         | CloudAdmin 사용자에 게는 콘텐츠 라이브러리에서 파일을 만들고 사용할 수 있는 권한이 있습니다.    |         SSO를 사용 하도록 설정 합니다.         |  Microsoft는 콘텐츠 라이브러리의 파일을 ESXi 호스트에 배포 합니다.  |
|  데이터 센터              |  --  |        --          |  Microsoft는 모든 데이터 센터 작업을 수행 합니다.  |
|  데이터 저장소               | AllocateSpace, 데이터 저장소. 찾아보기, Datastore.Config, DeleteFile, FileManagement, 데이터 저장소. UpdateVirtualMachineMetadata     |    --    |   -- |
|  ESX 에이전트 관리자       |  --  |         --       |  Microsoft에서 모든 작업을 수행 합니다.  |
|  폴더                  |  CloudAdmin 사용자에 게는 모든 폴더 권한이 있습니다.     |  --  |  --  |
|  전역                  |  전역 CancelTask, Global.asax 태그, Global.asax, Global. LogEvent, ManageCustomFields, ServiceManagers, 전역 SetCustomField, Global.SystemTag         |                  |    |
|  호스트                    |  호스트. Hbr. HbrManagement      |        --          |  Microsoft는 다른 모든 호스트 작업을 수행 합니다.  |
|  InventoryService        |  InventoryService      |        --          |  --  |
|  네트워크                 |  Network.Assign    |                  |  Microsoft는 다른 모든 네트워크 작업을 수행 합니다.  |
|  사용 권한             |  --  |        --       |  Microsoft는 모든 권한 작업을 수행 합니다.  |
|  프로필 기반 저장소  |  --  |        --       |  Microsoft는 모든 프로필 작업을 수행 합니다.  |
|  리소스                |  CloudAdmin 사용자에 게 모든 리소스 권한이 있습니다.        |      --       | --   |
|  예약된 태스크          |  CloudAdmin 사용자는 모든 ScheduleTask 권한을 가집니다.   |   --   | -- |
|  세션                |  세션. GlobalMessage, 세션 ValidateSession      |   --   |  Microsoft는 다른 모든 세션 작업을 수행 합니다.  |
|  저장소 뷰           |  StorageViews. 뷰   |        --          |  Microsoft는 다른 모든 저장소 보기 작업 (서비스 구성)을 수행 합니다.  |
|  작업                   |  --  |  --   |  Microsoft는 작업을 관리 하는 확장을 관리 합니다.  |
|  Vapp과                    |  CloudAdmin 사용자에 게는 모든 vApp 권한이 있습니다.  |  --  |  --  |
|  Virtual Machine         |  CloudAdmin 사용자는 모든 VirtualMachine 권한을 가집니다.  |  --  |  --  |
|  vService                |  CloudAdmin 사용자에 게는 모든 vService 권한이 있습니다.  |  --  |  --  |

## <a name="nsx-t-manager-access-and-identity"></a>NSX-T 관리자 액세스 및 id

*관리자* 계정을 사용 하 여 NSX Manager에 액세스 합니다. 모든 권한을 가지 며 계층-1 (T1) 게이트웨이, 세그먼트 (논리 스위치) 및 모든 서비스를 만들고 관리할 수 있습니다. 권한은 NSX (T0) 게이트웨이에 대 한 액세스를 제공 합니다. T0 게이트웨이를 변경 하면 네트워크 성능이 저하 되거나 사설 클라우드 액세스가 저하 될 수 있습니다. Azure Portal에서 지원 요청을 열어 NSX-T T0 게이트웨이에 대 한 변경 내용을 요청 합니다.
  
## <a name="next-steps"></a>다음 단계

이제 Azure VMware 솔루션 액세스 및 id 개념을 살펴보았습니다. 다음에 대해 알아볼 수 있습니다.

- [사설 클라우드 업그레이드 개념](concepts-upgrades.md).
- [Azure VMware 솔루션에 대 한 Vsphere 역할 기반 액세스 제어](concepts-role-based-access-control.md).
- [Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법](enable-azure-vmware-solution.md)

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-upgrades]: ./concepts-upgrades.md