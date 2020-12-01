---
title: Azure NetApp Files를 사용 하는 솔루션 아키텍처 | Microsoft Docs
description: Azure NetApp Files를 사용 하는 솔루션 아키텍처에 대 한 모범 사례에 대 한 참조를 제공 합니다.
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
ms.date: 11/30/2020
ms.author: b-juche
ms.openlocfilehash: 44553d7b88d6b911769c7449d71d71418aaeba6e
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96434892"
---
# <a name="solution-architectures-using-azure-netapp-files"></a>Azure NetApp Files를 사용하는 솔루션 아키텍처
이 문서에서는 Azure NetApp Files 사용을 위한 솔루션 아키텍처를 이해 하는 데 도움이 되는 모범 사례에 대 한 참조를 제공 합니다.  

다음 다이어그램은 Azure NetApp Files에서 제공 하는 솔루션 아키텍처의 범주를 요약 합니다.

![솔루션 아키텍처 범주](../media/azure-netapp-files/solution-architecture-categories.png)

## <a name="linux-oss-apps-and-database-solutions"></a>Linux OSS 앱 및 데이터베이스 솔루션

이 섹션에서는 Linux OSS 응용 프로그램 및 데이터베이스의 솔루션에 대 한 참조를 제공 합니다. 

### <a name="oracle"></a>Oracle

* [Azure NetApp Files 단일 볼륨의 Oracle 데이터베이스 성능](performance-oracle-single-volumes.md)
* [Azure 배포 모범 사례 가이드의 Oracle Azure NetApp Files 사용](https://www.netapp.com/us/media/tr-4780.pdf)
* [Oracle VM 이미지 및 Microsoft Azure에 대 한 배포: 공유 저장소 구성 옵션](../virtual-machines/workloads/oracle/oracle-vm-solutions.md#shared-storage-configuration-options)
* [Oracle Database에서 Azure NetApp Files를 사용할 경우의 이점](solutions-benefits-azure-netapp-files-oracle-database.md)

## <a name="windows-apps-and-sql-server-solutions"></a>Windows 앱 및 SQL Server 솔루션

이 섹션에서는 Windows 응용 프로그램 및 SQL Server 솔루션에 대 한 참조를 제공 합니다.

### <a name="file-sharing-and-global-file-caching"></a>파일 공유 및 전역 파일 캐싱

* [사용자 고유의 Azure NFS 빌드 Wrestling Linux 파일 공유를 클라우드로](https://cloud.netapp.com/blog/ma-anf-blg-build-your-own-linux-nfs-file-shares)
* [전역 파일 캐시/Azure NetApp Files 배포](https://youtu.be/91LKb1qsLIM)
* [Azure NetApp Files에 대 한 클라우드 규정 준수](https://cloud.netapp.com/hubfs/Cloud%20Compliance%20for%20Azure%20NetApp%20Files%20-%20November%202020.pdf)

### <a name="sql-server"></a>SQL Server

* [Azure NetApp Files를 사용 하 여 SMB를 통해 SQL Server 배포](https://www.youtube.com/watch?v=x7udfcYbibs)
<!-- * [Deploy SQL Server Always-On Failover Cluster over SMB with Azure NetApp Files](https://www.youtube.com/watch?v=zuNJ5E07e8Q) --> 
<!-- * [Deploy Always-On Availability Groups with Azure NetApp Files](https://www.youtube.com/watch?v=y3VQmzzeyvc) --> 

## <a name="sap-on-azure-solutions"></a>Azure의 SAP 솔루션

이 섹션에서는 Azure 솔루션의 SAP에 대 한 참조를 제공 합니다. 

### <a name="generic-sap-and-sap-netweaver"></a>일반 SAP 및 SAP Netweaver 

* [Azure NetApp Files를 사용 하는 Microsoft Azure의 SAP 응용 프로그램](https://www.netapp.com/us/media/tr-4746.pdf)
* [SAP 애플리케이션용 Azure NetApp Files를 사용하여 SUSE Linux Enterprise Server에서 Azure VM의 SAP NetWeaver 고가용성 실현](../virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files.md)
* [SAP 응용 프로그램용 Azure NetApp Files를 사용 하 Red Hat Enterprise Linux의 Azure Vm에서 SAP NetWeaver에 대 한 고가용성](../virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files.md)
* [SAP 응용 프로그램용 SMB (Azure NetApp Files)를 사용 하는 Windows의 Azure Vm에서 SAP NetWeaver에 대 한 고가용성](../virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb.md)
* [SAP 응용 프로그램에 대 한 Red Hat Enterprise Linux Azure Vm의 SAP NetWeaver에 대 한 고가용성-다중 SID 가이드](../virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid.md)

### <a name="sap-hana"></a>SAP HANA 

* [SAP HANA Azure 가상 머신 스토리지 구성](../virtual-machines/workloads/sap/hana-vm-operations-storage.md)
* [Red Hat Enterprise Linux Azure NetApp Files를 사용 하 여 SAP HANA 확장의 고가용성](../virtual-machines/workloads/sap/sap-hana-high-availability-netapp-files-red-hat.md)
* [SUSE Linux Enterprise Server Azure NetApp Files를 사용 하 여 Azure Vm에서 대기 노드로 확장 SAP HANA](../virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse.md)
* [Red Hat Enterprise Linux Azure NetApp Files를 사용 하 여 Azure Vm에서 대기 노드로 확장 SAP HANA](../virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel.md)

### <a name="sap-iq-nls"></a>SAP IQ-NLS
*   [SUSE Linux Enterprise Server에서 Azure NetApp Files를 사용 하 여 SAP IQ-NLS HA 솔루션 배포](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/deploy-sap-iq-nls-ha-solution-using-azure-netapp-files-on-suse/ba-p/1651172#.X2tDfpNzBh4.linkedin)

### <a name="sap-tech-community-and-blog-posts"></a>SAP tech 커뮤니티 및 블로그 게시물 

* [Azure NetApp Files – SAP HANA 백업 (초)](https://blog.netapp.com/azure-netapp-files-sap-hana-backup-in-seconds/)
* [Azure NetApp Files – 스냅숏 백업에서 HANA 데이터베이스 복원](https://blog.netapp.com/azure-netapp-files-backup-sap-hana)
* [Azure NetApp Files – 클라우드 동기화를 사용 하 여 백업 오프 로드 SAP HANA](https://blog.netapp.com/azure-netapp-files-sap-hana)
* [Azure NetApp Files를 사용 하 여 SAP HANA 시스템 복사본 속도 향상](https://blog.netapp.com/sap-hana-faster-using-azure-netapp-files/)
* [클라우드 볼륨 ONTAP 및 Azure NetApp Files: SAP HANA 시스템 마이그레이션을 손쉽게 수행할 수 있습니다.](https://blog.netapp.com/cloud-volumes-ontap-and-azure-netapp-files-sap-hana-system-migration-made-easy/)

## <a name="virtual-desktop-infrastructure-solutions"></a>가상 데스크톱 인프라 솔루션

이 섹션에서는 가상 데스크톱 인프라 솔루션에 대 한 참조를 제공 합니다.

### <a name="windows-virtual-desktop"></a>Windows Virtual Desktop

* [Windows Virtual Desktop에서 Azure NetApp Files를 사용할 경우의 이점](solutions-windows-virtual-desktop.md)
* [Windows 가상 데스크톱의 FSLogix 프로필 컨테이너에 대 한 저장소 옵션](../virtual-desktop/store-fslogix-profile.md#azure-platform-details)
* [Azure NetApp Files를 사용 하 여 호스트 풀의 FSLogix 프로필 컨테이너 만들기](../virtual-desktop/create-fslogix-profile-container.md)
* [엔터프라이즈 규모의 Windows Virtual Desktop](/azure/architecture/example-scenario/wvd/windows-virtual-desktop)
* [엔터프라이즈 Azure NetApp Files 모범 사례에 대 한 Microsoft FSLogix](/azure/architecture/example-scenario/wvd/windows-virtual-desktop-fslogix#azure-netapp-files-best-practices)

## <a name="hpc-solutions"></a>HPC 솔루션

이 섹션에서는 HPC (고성능 컴퓨팅) 솔루션에 대 한 참조를 제공 합니다. 

### <a name="generic-hpc"></a>일반 HPC

* [Azure NetApp Files: 클라우드 저장소를 최대한 활용](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf)
* [Azure Batch 및 Azure NetApp Files를 사용 하 여 MPI 작업 실행](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
* [Azure Cycle Cloud: Azure NetApp Files에서 HPC 환경 CycleCloud](/azure/cyclecloud/overview)

### <a name="oil-and-gas"></a>석유 및 가스

* [HPC (고성능 컴퓨팅): Azure에서 석유 및 가스](https://techcommunity.microsoft.com/t5/azure-global/high-performance-computing-hpc-oil-and-gas-in-azure/ba-p/824926)
* [Azure에서 저수지 시뮬레이션 소프트웨어 실행](/azure/architecture/example-scenario/infrastructure/reservoir-simulation)

### <a name="electronic-design-automation-eda"></a>.EDA (Electronic design automation)

* [전자 디자인 자동화에 Azure NetApp Files 사용의 이점](solutions-benefits-azure-netapp-files-electronic-design-automation.md)
* [Azure CycleCloud: Azure NetApp Files을 사용 하는 .EDA HPC 랩](https://github.com/Azure/cyclecloud-hands-on-labs/blob/master/EDA/README.md)
* [반도체 업계를 위한 Azure](https://azurecomcdn.azureedge.net/cvt-f40f39cd9de2d875ab0c198a8d7b186350cf0bca161e80d7896941389685d012/mediahandler/files/resourcefiles/azure-for-the-semiconductor-industry/Azure_for_the_Semiconductor_Industry.pdf)

### <a name="analytics"></a>분석

* [Azure NetApp Files: Microsoft Azure의 SAS 표와 함께 사용할 새 공유 파일 시스템](https://communities.sas.com/t5/Architecture/Azure-NetApp-Files-A-new-shared-file-system-to-use-with-SAS-Grid/m-p/606978)
* [SAS®에서 Microsoft Azure 사용에 대 한 모범 사례](https://communities.sas.com/t5/Administration-and-Deployment/Best-Practices-for-Using-Microsoft-Azure-with-SAS/m-p/676833#M19680)

## <a name="azure-platform-services-solutions"></a>Azure platform services 솔루션

이 섹션에서는 Azure 플랫폼 서비스에 대 한 솔루션을 제공 합니다. 

### <a name="azure-kubernetes-services-and-kubernetes"></a>Azure Kubernetes Services 및 Kubernetes

* [Azure Kubernetes Service와 Azure NetApp Files 통합](../aks/azure-netapp-files.md)
* [Azure NetApp Files를 사용 하는 Azure의이 세계 Kubernetes 성능](https://cloud.netapp.com/blog/ma-anf-blg-configure-kubernetes-openshift)
* [Trident-컨테이너에 대 한 저장소 Orchestrator](https://netapp-trident.readthedocs.io/en/stable-v20.04/kubernetes/operations/tasks/backends/anf.html)

### <a name="azure-batch"></a>Azure Batch

* [Azure Batch 및 Azure NetApp Files를 사용 하 여 MPI 작업 실행](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
