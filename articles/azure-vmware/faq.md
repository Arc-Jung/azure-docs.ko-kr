---
title: 질문과 대답
description: Azure VMware 솔루션에 대 한 일반적인 질문에 대 한 답변을 제공 합니다.
ms.topic: conceptual
ms.date: 12/22/2020
ms.openlocfilehash: 941708003558dda601aa43459bc83133788687fd
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97835196"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution"></a>Azure VMware 솔루션에 대 한 질문과 대답

이 문서에서는 Azure VMware 솔루션에 대 한 자주 묻는 질문과 대답을 제공 합니다.

## <a name="general"></a>일반

#### <a name="what-is-azure-vmware-solution"></a>Azure VMware Solution이란?

기업은 IT 현대화 전략을 활용 하 여 비즈니스 민첩성을 개선 하 고 비용을 절감 하며 혁신을 가속화 하는 하이브리드 클라우드 플랫폼은 고객의 디지털 변환의 핵심 핵심으로 등장 했습니다. Azure VMware 솔루션은 VMware의 SDDC (Software-Defined 데이터 센터) 소프트웨어를 Microsoft의 Azure 글로벌 클라우드 서비스 에코 시스템과 결합 합니다. Azure VMware 솔루션은 성능, 가용성, 보안 및 규정 준수 요구 사항을 충족 하도록 관리 됩니다.

## <a name="azure-vmware-solution-service"></a>Azure VMware 솔루션 서비스

#### <a name="where-is-azure-vmware-solution-available-today"></a>현재 Azure VMware 솔루션은 어디에서 사용할 수 있나요?

서비스는 계속 해 서 새 지역에 추가 되므로 [최신 서비스 가용성 정보](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) 를 확인 하 여 자세한 내용을 확인 하세요. 

#### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>Azure VMware 솔루션 인스턴스에서 실행 되는 워크 로드는 Azure 서비스를 사용 하거나 통합할 수 있나요?

모든 Azure 서비스는 Azure VMware 솔루션 고객에 게 제공 됩니다. 서비스의 성능 및 가용성 제한은 서비스마다 다르게 접근해야 합니다.

#### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>현재 프라이빗 클라우드 리소스를 관리하는 데 사용하는 것과 동일한 도구를 사용하나요?

예. Azure Portal은 배포 및 여러 관리 작업에 사용 됩니다. vSphere 및 NSX-T 리소스 관리에는 vCenter 및 NSX Manager가 사용됩니다.

#### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>온-프레미스 vCenter를 사용하여 프라이빗 클라우드를 관리할 수 있나요?

Azure VMware 솔루션은 시작 시 온-프레미스 및 사설 클라우드 환경에서 단일 관리 환경을 지원 하지 않습니다. 프라이빗 클라우드 클러스터는 프라이빗 클라우드에 로컬인 vCenter 및 NSX Manager를 통해 관리됩니다.

#### <a name="can-i-use-vrealize-suite-running-on-premises"></a>온-프레미스에서 실행되는 vRealize Suite를 사용할 수 있나요? 

구체적인 통합 및 사용 사례는 건별로 평가할 수 있습니다.

#### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>온-프레미스 환경에서 Azure VMware 솔루션 사설 클라우드로 vSphere Vm을 마이그레이션할 수 있나요?

예. 표준 cross vCenter [vMotion 요구 사항이](https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=2106952) 충족 되는 경우 vm 마이그레이션 및 vMotion를 사용 하 여 사설 클라우드로 vm을 이동할 수 있습니다.

#### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>온-프레미스 환경에서 특정 버전의 vSphere가 필요한가요?

모든 클라우드 환경은 vMotion에 대 한 온-프레미스 환경에서 VMware HCX, vSphere 5.5 이상으로 제공 됩니다.

#### <a name="what-does-the-change-control-process-look-like"></a>변경 제어 프로세스는 어떻게 생겼나요?

서비스 자체에 대 한 업데이트는 Microsoft Azure의 표준 변경 관리 프로세스를 따릅니다. 고객은 워크로드 관리 작업 및 관련된 변경 관리 프로세스를 담당합니다.

#### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>Azure VMware Solution by CloudSimple과 어떻게 다른가요?

새 Azure VMware Solution과 관련하여 Microsoft와 VMware는 직접 클라우드 공급자 파트너십을 체결했습니다. 새로운 솔루션은 Microsoft에서 완전히 설계, 구축 및 지원 되며 VMware에 의해 보증 됩니다. 아키텍처는 전용 Azure 인프라에서 실행 되는 VMware 기술 스택을 사용 하 여 일관 된 솔루션입니다.

#### <a name="can-azure-vmware-solution-vms-be-managed-by-vmrc"></a>VMRC를 통해 Azure VMware 솔루션 Vm을 관리할 수 있나요?
예. 가 설치 된 시스템에서 사설 클라우드 vCenter에 액세스할 수 있으며 공용 DNS를 사용 하 여 ESXi 호스트 이름을 확인 합니다.

#### <a name="are-there-special-instructions-for-installing-and-using-vmrc-with-azure-vmware-solution-vms"></a>Azure VMware 솔루션 Vm과 함께 VMRC를 설치 하 고 사용 하기 위한 특별 지침이 있나요?
아니요. VM 필수 구성 요소를 충족 하려면 [VMware에서 제공](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-89E7E8F0-DB2B-437F-8F70-BA34C505053F.html)하는 지침을 따르세요. 

#### <a name="is-vmware-hcx-supported-on-vpns"></a>Vpn에서 VMware HCX가 지원 되나요?
아니요, 대역폭 및 대기 시간 요구 사항으로 인해 발생 합니다.

#### <a name="can-azure-bastion-be-used-for-connecting-to-azure-vmware-solution-vms"></a>Azure가 Azure VMware 솔루션 Vm에 연결 하는 데 사용할 수 있나요?
Azure는 Azure VMware 솔루션을 인터넷에 노출 하지 않도록 하기 위해 점프 상자에 연결 하는 서비스입니다. Azure 가상 사용자는 azure IaaS 개체가 아니므로 azure 방호를 사용 하 여 azure VMware 솔루션 Vm에 연결할 수 없습니다.

#### <a name="can-azure-load-balancer-internal-be-used-for-azure-vmware-solution-vms"></a>내부 Azure Load Balancer Azure VMware 솔루션 Vm에 사용할 수 있나요?
아니요. Azure Load Balancer 내부 전용은 Azure IaaS Vm만 지원 합니다. Azure Load Balancer는 IP 기반 백 엔드 풀을 지원 하지 않습니다. azure VMware 솔루션 Vm이 azure 개체가 아닌 Azure Vm 또는 가상 머신 확장 집합 개체만.

#### <a name="can-an-existing-expressroute-gateway-be-used-to-connect-to-azure-vmware-solution"></a>기존 Express 경로 게이트웨이를 Azure VMware 솔루션에 연결 하는 데 사용할 수 있나요?
예. 가상 네트워크 당 4 개의 Express 경로 회로 제한을 초과 하지 않는 한 기존 Express 경로 게이트웨이를 사용 하 여 Azure VMware 솔루션에 연결 합니다. Express 경로를 통해 온-프레미스에서 Azure VMware 솔루션에 액세스 하려면 Express 경로 게이트웨이가 연결 된 회로 간에 전이적 라우팅을 제공 하지 않으므로 Express 경로 Global Reach 있어야 합니다.

## <a name="compute-network-storage-and-backup"></a>계산, 네트워크, 저장소 및 백업

#### <a name="is-there-more-than-one-type-of-host-available"></a>사용 가능한 호스트 유형이 2개 이상 있나요?

사용할 수 있는 호스트 유형은 하나 뿐입니다.

#### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>각 호스트 유형의 CPU 사양이 어떻게 되나요?

서버에는 듀얼 18코어 2.3GHz Intel CPU를 사용합니다.

#### <a name="how-much-memory-is-in-each-host"></a>각 호스트의 메모리는 얼마인가요?

서버의 RAM은 576GB입니다.

#### <a name="what-is-the-storage-capacity-of-each-host"></a>각 호스트의 스토리지 용량은 얼마인가요?

각 ESXi 호스트에는 15.2 TB의 용량 계층을 포함 하는 두 개의 vSAN diskgroups와 3.2-TB NVMe 캐시 계층 (각 diskgroups의 1.6 TB)이 있습니다.

#### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>각 ESXi 호스트에서 사용할 수 있는 네트워크 대역폭은 얼마나 되나요?

Azure VMware 솔루션의 각 ESXi 호스트는 4 25-Gbps Nic, ESXi 시스템 트래픽에 대해 프로 비전 된 두 개의 Nic, 워크 로드 트래픽에 대해 프로 비전 된 두 개의 Nic를 사용 하 여 구성 됩니다. 

#### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>미사용 암호화 된 vSAN 데이터 저장소에 저장 된 데이터 입니까?

예, 모든 vSAN 데이터는 Azure Key Vault에 저장 된 키를 사용 하 여 기본적으로 암호화 됩니다.

####  <a name="what-independent-software-vendors-isvs-backup-solutions-work-with-azure-vmware-solution"></a>Azure VMware 솔루션에서 사용할 Isv (독립 소프트웨어 공급 업체) 백업 솔루션은 무엇 인가요?

Commvault, Veritas 및 Veeam은 Azure VMware 솔루션을 사용 하도록 백업 솔루션을 확장 했습니다.  그러나 HotAdd 전송 모드에서 VMware VADP를 사용 하는 모든 백업 솔루션은 Azure VMware 솔루션에서 바로 사용할 수 있습니다.

#### <a name="what-about-support-for-isv-backup-solutions"></a>ISV 백업 솔루션에 대 한 지원 이란 무엇 인가요?

이러한 백업 솔루션은 고객에 의해 설치 되 고 관리 되므로 지원 하기 위해 각 ISV에 게 연락할 수 있습니다. 

#### <a name="what-is-the-correct-storage-policy-for-the-dedupe-setup"></a>중복 제거 설치에 대 한 올바른 저장소 정책은 무엇 인가요?

VM 템플릿에 대 한 *thin_provision* 저장소 정책을 사용 합니다.  기본값은 *thick_provision* 입니다.

#### <a name="are-the-snmp-infrastructure-logs-shared"></a>SNMP 인프라 로그가 공유 되나요?

아니요.

## <a name="hosts-clusters-and-private-clouds"></a>호스트, 클러스터 및 프라이빗 클라우드

#### <a name="is-the-underlying-infrastructure-shared"></a>기본 인프라를 공유하나요?

아니요, 프라이빗 클라우드 호스트와 클러스터는 전용이며 사용 전과 후에 안전하게 삭제됩니다.

#### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>클러스터당 최소/최대 호스트 수는 얼마인가요?

ESXi 호스트 3~16개 사이에서 클러스터를 스케일링할 수 있습니다. 평가판 클러스터는 호스트 3개로 제한됩니다.

#### <a name="can-i-scale-my-private-cloud-clusters"></a>프라이빗 클라우드 클러스터를 스케일링할 수 있나요?

예, 클러스터는 최소 및 최대 ESXi 호스트 수 사이에서 크기를 조정 합니다. 평가판 클러스터는 호스트 3개로 제한됩니다.

#### <a name="what-are-trial-clusters"></a>평가판 클러스터란 무엇인가요?

평가판 클러스터는 Azure VMware 솔루션 사설 클라우드의 1 개월 평가에 사용 되는 세 개의 호스트 클러스터입니다.

#### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>평가판 클러스터에 고성능 호스트를 사용할 수 있나요?

아니요. 고성능 ESXi 호스트는 프로덕션 클러스터에 사용하도록 예약되어 있습니다.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware 솔루션 및 VMware 소프트웨어

#### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>프라이빗 클라우드에 사용되는 VMware 소프트웨어 버전은 무엇인가요?

[!INCLUDE [vmware-software-versions](includes/vmware-software-versions.md)]


#### <a name="do-private-clouds-use-vmware-nsx"></a>프라이빗 클라우드는 VMware NSX를 사용하나요?

예, NSX-T 2.5은 Azure VMware 솔루션 사설 클라우드의 소프트웨어 정의 네트워킹에 사용 됩니다.

#### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>프라이빗 클라우드에서 VMware NSX-V를 사용할 수 있나요?

아니요. 현재 지원되는 유일한 NSX 버전은 NSX-T입니다.

#### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>사설 클라우드에 연결 하는 온-프레미스 환경 또는 네트워크에서 NSX 필요 한가요?

아니요, 온-프레미스에서는 NSX를 사용할 필요가 없습니다.

#### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>프라이빗 클라우드에서 VMware 소프트웨어를 업그레이드 및 업데이트하는 일정이 어떻게 되나요?

사설 클라우드 소프트웨어 번들 업그레이드는 VMware에서 최신 소프트웨어 번들 릴리스의 한 버전에 있는 소프트웨어를 유지 합니다. 사설 클라우드 소프트웨어 버전은 개별 소프트웨어 구성 요소의 최신 버전 (ESXi, NSX, vCenter, vSAN)과 다를 수 있습니다.

#### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>프라이빗 클라우드 소프트웨어 스택은 얼마나 자주 업데이트되나요?

사설 클라우드 소프트웨어는 VMware에서 소프트웨어 번들의 릴리스를 추적 하는 일정에 따라 업그레이드 됩니다. 프라이빗 클라우드는 업그레이드로 인한 가동 중지 시간이 없습니다.

## <a name="connectivity"></a>연결

#### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>프라이빗 클라우드를 온-프레미스 환경과 통합하는 데 필요한 네트워크 IP 주소 계획은 무엇인가요?

Azure VMware 솔루션 사설 클라우드를 배포 하려면 개인 네트워크/22 주소 공간이 필요 합니다. 이 개인 주소 공간은 구독의 다른 가상 네트워크 또는 온-프레미스 네트워크와 겹치지 않아야 합니다.
 
#### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>온-프레미스 환경에서 Azure VMware 솔루션 사설 클라우드로 연결 어떻게 할까요??

다음 두 가지 방법 중 하나로 서비스에 연결할 수 있습니다. 

- ExpressRoute를 통해 프라이빗 클라우드에 피어링되는 Azure 가상 네트워크에 배포된 VM 또는 애플리케이션 게이트웨이를 사용합니다.
- Express 경로를 통해 온-프레미스 데이터 센터에서 Azure Express 경로 회로로 Global Reach 합니다.

#### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>워크로드 VM을 인터넷 또는 Azure 서비스 엔드포인트에 연결하려면 어떻게 할까요?

Azure Portal에서 프라이빗 클라우드에 인터넷 연결을 사용하도록 설정합니다. NSX-T 관리자를 사용하여 NSX-T T1 라우터와 논리 스위치를 만듭니다. 그런 다음, vCenter를 사용하여 논리 스위치가 정의한 네트워크 세그먼트에 VM을 배포합니다. 해당 VM은 인터넷 및 Azure 서비스에 대 한 네트워크 액세스 권한을 가집니다.

#### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>프라이빗 클라우드의 논리 네트워크에 있는 VM에 대한 인터넷 액세스를 제한해야 하나요?

아니요. 인터넷에서 사설 클라우드로 직접 들어오는 네트워크 트래픽은 기본적으로 허용 되지 않습니다.  그러나 Azure VMware 솔루션 사설 클라우드의 Azure Portal에서 [공용 IP](public-ip-usage.md) 옵션을 통해 Azure Vmware 솔루션 vm을 인터넷에 노출할 수 있습니다.

#### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>논리 네트워크의 VM에서 인터넷으로 액세스하는 것을 제한해야 하나요?

예. NSX manager를 사용 하 여 방화벽을 만들어 인터넷에 대 한 VM 액세스를 제한 해야 합니다.


#### <a name="can-azure-vmware-solution-use-azure-virtual-wan-hosted-expressroute-gateways"></a>Azure VMware 솔루션은 Azure 가상 WAN 호스트 된 Express 경로 게이트웨이를 사용할 수 있나요?
예.

#### <a name="can-transit-connectivity-be-established-between-on-premises-and-azure-vmware-solution-through-azure-virtual-wan-over-expressroute-global-reach"></a>Express 경로 Global Reach를 통해 Azure 가상 WAN을 통해 온-프레미스와 Azure VMware 솔루션 간에 전송 연결을 설정할 수 있나요?
Azure 가상 WAN은 연결 된 두 Express 경로 회로와 비가상 WAN Express 경로 게이트웨이 간의 전이적 라우팅을 제공 하지 않습니다. Express 경로 Global Reach를 사용 하 여 온-프레미스와 Azure VMware 솔루션 간의 연결을 허용 하지만 가상 WAN 허브 대신 Microsoft의 글로벌 네트워크를 통해 이동 합니다.

#### <a name="could-i-use-hcx-through-public-internet-communications-as-a-workaround-for-the-non-supportability-of-hcx-when-using-vpn-s2s-with-vwan-for-on-premises-communications"></a>온-프레미스 통신을 위해 vWAN에서 VPN S2S를 사용 하는 경우 HCX의 지원 되지 않는 문제에 대 한 해결 방법으로 공용 인터넷 통신을 통해 HCX를 사용할 수 있나요?

현재 HCX에서 유일 하 게 지원 되는 방법은 Express 경로를 통해서만 지원 됩니다.

## <a name="accounts-and-privileges"></a>계정 및 권한

#### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>새 Azure VMware 솔루션 사설 클라우드를 사용 하 여 얻을 수 있는 계정과 권한은 무엇 인가요?

vCenter의 cloudadmin 사용자에 대한 자격 증명과 NSX-T 관리자에 대한 관리자 액세스 권한이 제공됩니다. Azure Active Directory를 통합하는 데 사용할 수 있는 CloudAdmin 그룹도 있습니다. 자세한 내용은 [액세스 및 ID 개념](concepts-identity.md)을 참조하세요.

#### <a name="can-have-administrator-access-to-esxi-hosts"></a>ESXi 호스트에 관리자로 액세스할 수 있나요?

아니요, 솔루션의 보안 요구 사항을 충족하기 위해 ESXi에 대한 관리자 권한 액세스는 제한되어 있습니다.

#### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>vCenter에서 어떤 권한을 얻게 되나요?

CloudAdmin 그룹 권한을 얻게 됩니다. 자세한 내용은 [액세스 및 ID 개념](concepts-identity.md)을 참조하세요.

#### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>NSX-T 관리자에서 어떤 권한을 얻게 되나요?

NSX에 대 한 모든 관리자 권한을 보유 하 고 NSX 데이터 센터 온-프레미스와 마찬가지로 vSphere 역할 기반 액세스 제어를 관리할 수 있습니다. 자세한 내용은 [액세스 및 ID 개념](concepts-identity.md)을 참조하세요.

> [!NOTE]
> 프라이빗 클라우드 배포의 한 과정으로 T0 라우터가 생성 및 구성됩니다. 이 논리 라우터 또는 NSX-T 에지 노드 VM을 수정하면 프라이빗 클라우드에 연결하는 데 영향을 줄 수 있습니다.

## <a name="billing-and-support"></a>청구 및 지원

#### <a name="how-will-pricing-be-structured-for-azure-vmware-solution"></a>Azure VMware 솔루션에 대 한 가격 책정은 어떻게 구성 되나요?

가격 책정에 대 한 일반적인 질문은 Azure VMware 솔루션 [가격 책정](https://azure.microsoft.com/pricing/details/azure-vmware) 페이지를 참조 하세요. 

#### <a name="can-azure-vmware-solution-be-purchased-through-a-microsoft-csp"></a>Microsoft CSP를 통해 Azure VMware 솔루션을 구매할 수 있나요?

예, 고객은 CSP에서 관리 하는 Azure 구독 내에서 Azure VMware 솔루션을 배포할 수 있습니다.

#### <a name="who-supports-azure-vmware-solution"></a>누가 Azure VMware 솔루션을 지원 하나요?

Microsoft는 Azure VMware 솔루션에 대 한 지원을 제공 합니다. [지원 요청](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)을 제출할 수 있습니다. 

CSP 관리 구독의 경우 첫 번째 지원 수준에서는 다른 Azure 서비스에 대 한 CSP와 동일한 방식으로 솔루션 공급자를 제공 합니다.

#### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>Azure VMware 솔루션 사설 클라우드를 만들려면 어떤 계정이 필요 한가요?

Azure 구독의 Azure 계정이 필요합니다.

#### <a name="are-red-hat-solutions-supported-on-azure-vmware-solution"></a>Red Hat 솔루션은 Azure VMware 솔루션에서 지원 되나요?

Microsoft 및 Red Hat는 Azure 플랫폼에서 실행 되는 Red Hat 에코 시스템에 대 한 통합 연결 지점을 제공 하는 통합 된 공동 배치 지원 팀을 공유 합니다.  Red Hat Enterprise Linux를 사용 하는 다른 Azure platform 서비스와 마찬가지로 Azure VMware 솔루션은 클라우드 액세스 및 통합 지원 파라솔에 속합니다. Red Hat Enterprise Linux는 Azure 내의 Azure VMware 솔루션을 기반으로 실행 하기 위해 지원 됩니다.

#### <a name="is-vmware-hcx-enterprise-available-and-if-so-how-much-does-it-cost"></a>VMware HCX Enterprise를 사용할 수 있으며, 그렇다면 비용은 얼마나 되나요?

VMware HCX Enterprise는 *미리 보기* 기능/서비스로 Azure vmware 솔루션에서 사용할 수 있습니다. Azure VMware 솔루션에 대 한 VMware HCX Enterprise는 미리 보기 상태 이며, 무료 기능/서비스 이며 미리 보기 서비스 사용 약관에 적용 됩니다. VMware HCX Enterprise 서비스가 GA로 전환 되 면 요금이 청구 되는 30 일 알림이 표시 됩니다. 서비스를 끄거나 옵트아웃 (opt out) 할 수 있습니다.

#### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>Azure VMware 솔루션에 대 한 호스트 할당량 증가를 요청 어떻게 할까요??

CSP 관리 구독의 경우 고객은 파트너에 게 요청을 제출 해야 합니다. 파트너 팀은 Microsoft에 참여 하 여 구독에 대 한 할당량을 늘립니다. 자세한 내용은 [Azure VMware 솔루션 리소스를 사용 하도록 설정 하는 방법 문서](enable-azure-vmware-solution.md) 를 참조 하세요. 

EA 구독의 경우 다음 절차를 사용 합니다. 먼저 다음이 필요 합니다.

* Microsoft를 사용 하는 [Azure 기업계약 (EA)](../cost-management-billing/manage/ea-portal-agreements.md) .
* Azure 구독의 Azure 계정.

Azure VMware 솔루션 리소스를 만들기 전에 호스트를 할당 하도록 지원 티켓을 제출 합니다. 요청을 확인 하 고 충족 하는 데 최대 5 영업일까지 소요 됩니다. 기존 Azure VMware Solution 프라이빗 클라우드가 있고 더 많은 호스트를 할당하려는 경우에도 동일한 프로세스를 진행합니다.

1. Azure Portal의 **도움말 + 지원** 에서 **[새 지원 요청](https://rc.portal.azure.com/#create/Microsoft.Support)** 을 만들고 티켓에 대 한 다음 정보를 제공 합니다.
   - **문제 유형:** 기술
   - **구독:** 구독 선택
   - **서비스:** Azure VMware 솔루션 > 모든 서비스
   - **리소스:** 일반 질문 
   - **요약:** 용량 필요
   - **문제 유형:** 용량 관리 문제
   - **문제 하위 유형:** 호스트 할당량/용량을 추가하기 위한 고객 요청

1. 지원 티켓에 대 한 **설명** 의 **세부 정보** 탭에서 다음을 입력 합니다.

   - POC 또는 프로덕션 
   - 지역 이름
   - 호스트 수
   - 기타 세부 정보

   >[!NOTE]
   >Azure VMware 솔루션은 사설 클라우드를 실행 하 고 중복성 N + 1 호스트에 대해 최소 세 개의 호스트를 권장 합니다. 

1. **검토 + 만들기** 를 선택 하 여 요청을 제출 합니다.

   지원 담당자가 요청을 확인 하는 데 최대 5 영업일 소요 됩니다.

   >[!IMPORTANT] 
   >기존 Azure VMware 솔루션이 이미 있고 추가 호스트를 요청 하는 경우 호스트를 할당 하는 데 영업일 5 일이 필요 합니다. 

1. 호스트를 프로 비전 하기 전에 Azure Portal에서 **MICROSOFT AVS** 리소스 공급자를 등록 했는지 확인 합니다.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   리소스 공급자를 등록 하는 방법에 대 한 자세한 내용은 [Azure 리소스 공급자 및 형식](../azure-resource-manager/management/resource-providers-and-types.md)을 참조 하세요. 

#### <a name="are-reserved-instances-available-for-purchasing-through-the-cloud-solution-provider-csp-program"></a>CSP (클라우드 솔루션 공급자) 프로그램을 통해 구매할 수 있는 예약 된 인스턴스 인가요?

예. CSP는 고객에 대 한 예약 인스턴스를 구입할 수 있습니다. 자세한 내용은 [예약 된 인스턴스로 비용](reserved-instance.md)절감을 참조 하세요. 

#### <a name="does-azure-vmware-solution-offer-multi-tenancy-for-hosting-csp-partners"></a>Azure VMware 솔루션은 CSP 파트너 호스팅을 위한 다중 테 넌 트를 제공 하나요?

아니요. 현재 Azure VMware 솔루션은 다중 테 넌 트를 제공 하지 않습니다.

#### <a name="will-traffic-between-on-premises-and-azure-vmware-solution-over-expressroute-incur-any-outbound-data-transfer-charge-in-the-metered-data-plan"></a>Express 경로를 통해 온-프레미스와 Azure VMware 솔루션 간의 트래픽이 요금제 데이터 요금제의 아웃 바운드 데이터 전송 요금을 초래 하나요?

Azure VMware 솔루션 Express 경로 회로의 트래픽은 어떤 방식으로든 측정 되지 않습니다. Azure에 온-프레미스에 연결 하는 Express 경로 회로의 트래픽은 Express 경로 요금제에 따라 요금이 청구 됩니다.


## <a name="customer-communication"></a>고객 통신

#### <a name="how-can-i-receive-an-alert-when-azure-sends-service-health-notifications-to-my-azure-subscription"></a>Azure 구독에 서비스 상태 알림을 보낼 때 경고를 받으려면 어떻게 해야 하나요?

서비스 문제, 계획 된 유지 관리, 상태 권고, 보안 권고 알림은 Azure Portal **Service Health** 을 통해 게시 됩니다.  이러한 알림에 대해 활동 로그 경고를 설정할 때 시기 적절 한 조치를 취할 수 있습니다. 자세한 내용은 [Azure Portal를 사용 하 여 서비스 상태 경고 만들기](../service-health/alerts-activity-log-service-notifications-portal.md#create-service-health-alert-using-azure-portal)를 참조 하세요.

:::image type="content" source="media/service-health.png" alt-text="Service Health 알림 스크린샷":::



<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952?lang=en_US&queryTerm=21069522

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md
