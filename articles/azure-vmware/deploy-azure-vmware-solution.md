---
title: Azure VMware Solution 배포 및 구성
description: 계획 단계에서 수집된 정보를 사용하여 Azure VMware Solution 프라이빗 클라우드를 배포하고 구성하는 방법을 알아봅니다.
ms.topic: tutorial
ms.custom: contperf-fy21q3
ms.date: 02/17/2021
ms.openlocfilehash: 48b6927407a95d41603c3032f298ffc28def9693
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103462459"
---
# <a name="deploy-and-configure-azure-vmware-solution"></a>Azure VMware Solution 배포 및 구성

이 문서에서는 [계획 섹션](production-ready-deployment-steps.md)의 정보를 사용하여 Azure VMware Solution을 배포하고 구성합니다. 

>[!IMPORTANT]
>정보가 아직 정의되지 않은 경우 계속하기 전에 [계획 섹션](production-ready-deployment-steps.md)으로 돌아가세요.


## <a name="create-an-azure-vmware-solution-private-cloud"></a>Azure VMware Solution 프라이빗 클라우드 만들기

[Azure VMware Solution 프라이빗 클라우드 만들기](tutorial-create-private-cloud.md) 자습서의 필수 구성 요소 및 단계를 따릅니다. Azure VMware Solution 프라이빗 클라우드는 [Azure Portal](tutorial-create-private-cloud.md#azure-portal) 또는 [Azure CLI](tutorial-create-private-cloud.md#azure-cli)를 사용하여 만들 수 있습니다.  

>[!NOTE]
>이 단계의 엔드투엔드 개요는 [Azure VMware Solution: 배포](https://www.youtube.com/embed/gng7JjxgayI) 비디오를 참조하세요.

## <a name="create-the-jump-box"></a>점프 상자 만들기

>[!IMPORTANT]
>**프라이빗 클라우드 만들기** 화면의 초기 프로비저닝 단계에서 **Virtual Network** 옵션을 비워 둔 경우 이 섹션을 진행하기 **전에** [VMware 프라이빗 클라우드에 대한 네트워킹 구성](tutorial-configure-networking.md) 자습서를 완료하세요.  

Azure VMware Solution을 배포한 후 vCenter 및 NSX에 연결하는 가상 네트워크의 점프 상자를 만듭니다. ExpressRoute 회로 및 ExpressRoute Global Reach가 구성되었으면 점프 상자가 필요하지 않습니다.  그러나 Azure VMware Solution에서 vCenter 및 NSX에 도달하는 것이 편리합니다.  

:::image type="content" source="media/pre-deployment/jump-box-diagram.png" alt-text="Azure VMware Solution 점프 상자 만들기" border="false" lightbox="media/pre-deployment/jump-box-diagram.png":::

[배포 프로세스의 일부로 확인하거나 만든](production-ready-deployment-steps.md#attach-azure-virtual-network-to-azure-vmware-solution) 가상 네트워크에서 VM(가상 머신)을 만들려면 다음 지침을 따릅니다. 

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-a-virtual-network-with-expressroute"></a>ExpressRoute를 사용하여 가상 네트워크에 연결

>[!IMPORTANT]
>Azure의 배포 화면에서 가상 네트워크가 이미 정의된 경우 다음 섹션으로 건너뜁니다.

배포 단계에서 가상 네트워크가 정의되지 않았고 Azure VMware Solution의 ExpressRoute를 기존 ExpressRoute 게이트웨이에 연결하려는 경우 다음 단계를 따릅니다.

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]

## <a name="verify-network-routes-advertised"></a>보급된 네트워크 경로 확인

점프 상자는 Azure VMware Solution에서 ExpressRoute 회로를 통해 연결하는 가상 네트워크에 있습니다.  Azure에서 점프 상자의 네트워크 인터페이스로 이동하여 [유효한 경로를 확인](../virtual-network/manage-route-table.md#view-effective-routes)합니다.

유효한 경로 목록에는 Azure VMware Solution 배포의 일부로 만들어진 네트워크가 표시됩니다. [프라이빗 클라우드를 만들](#create-an-azure-vmware-solution-private-cloud) 때 [정의한 `/22` 네트워크](production-ready-deployment-steps.md#ip-address-segment-for-private-cloud-management)에서 파생된 여러 네트워크가 표시됩니다.  

:::image type="content" source="media/pre-deployment/azure-vmware-solution-effective-routes.png" alt-text="Azure VMware Solution에서 Azure Virtual Network로 보급된 네트워크 경로 확인" lightbox="media/pre-deployment/azure-vmware-solution-effective-routes.png":::

이 예제에서 배포 중에 입력된 10.74.72.0/22 네트워크는 /24 네트워크를 파생합니다.  비슷한 항목이 표시되면 Azure VMware Solution에서 vCenter에 연결할 수 있습니다.

## <a name="connect-and-sign-in-to-vcenter-and-nsx-t"></a>vCenter 및 NSX-T에 연결 및 로그인

이전 단계에서 만든 점프 상자에 로그인합니다. 로그인되면 웹 브라우저를 열고, vCenter 및 NSX-T Manager로 이동하여 로그인합니다.  

Azure Portal에서 vCenter 및 NSX-T Manager 콘솔의 IP 주소와 자격 증명을 식별할 수 있습니다.  프라이빗 클라우드를 선택한 다음, **관리** > **ID** 를 선택합니다.

>[!TIP]
>**새 암호 생성** 을 선택하여 새 vCenter 및 NSX-T 암호를 생성합니다.

:::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="프라이빗 클라우드 vCenter와 NSX 관리자의 URL 및 자격 증명을 표시합니다." border="true":::



## <a name="create-a-network-segment-on-azure-vmware-solution"></a>Azure VMware Solution에서 네트워크 세그먼트 만들기

NSX-T Manager를 사용하여 Azure VMware Solution 환경에 새 네트워크 세그먼트를 만듭니다.  [계획 섹션](production-ready-deployment-steps.md)에서 만들려는 네트워크가 정의되었습니다.  아직 정의되지 않은 경우 계속하기 전에 [계획 섹션](production-ready-deployment-steps.md)으로 돌아가세요.

>[!IMPORTANT]
>정의한 CIDR 네트워크 주소 블록이 Azure 또는 온-프레미스 환경의 주소 블록과 겹치지 않는지 확인합니다.  

[Azure VMware Solution에서 NSX-T 네트워크 세그먼트 만들기](tutorial-nsx-t-network-segment.md) 자습서에 따라 Azure VMware Solution에서 NSX-T 네트워크 세그먼트를 만듭니다.

## <a name="verify-advertised-nsx-t-segment"></a>보급된 NSX-T 세그먼트 확인

[보급된 네트워크 경로 확인](#verify-network-routes-advertised) 단계로 돌아갑니다. 이전 단계에서 만든 네트워크 세그먼트를 나타내는 다른 경로가 목록에 표시됩니다.  

가상 머신의 경우 [Azure VMware Solution에서 네트워크 세그먼트 만들기](#create-a-network-segment-on-azure-vmware-solution) 단계에서 만든 세그먼트를 할당합니다.  

DNS가 필요하므로 사용하려는 DNS 서버를 확인합니다.  

- ExpressRoute Global Reach가 구성되어 있으면 온-프레미스에서 사용할 DNS 서버를 사용합니다.  
- Azure에 DNS 서버가 있으면 해당 서버를 사용합니다.  
- 둘 중 하나가 없으면 점프 상자에서 사용하는 DNS 서버를 사용합니다.

>[!NOTE]
>이 단계는 DNS 서버를 확인하는 것이며, 구성은 이 단계에서 수행되지 않습니다.

## <a name="optional-provide-dhcp-services-to-nsx-t-network-segment"></a>(선택 사항) NSX-T 네트워크 세그먼트에 DHCP 서비스 제공

NSX-T 세그먼트에서 DHCP를 사용하려면 이 섹션을 계속 진행합니다. 그렇지 않으면 [NSX-T 네트워크 세그먼트에 VM 추가](#add-a-vm-on-the-nsx-t-network-segment) 섹션으로 건너뜁니다.  

NSX-T 네트워크 세그먼트를 만들었으므로 이제 다음 두 가지 방법으로 Azure VMware Solution에서 DHCP를 만들고 관리할 수 있습니다.

* NSX-T를 사용하여 DHCP 서버를 호스트하는 경우 [DHCP 서버를 생성](manage-dhcp.md#create-a-dhcp-server)하고 [해당 서버에 릴레이](manage-dhcp.md#create-dhcp-relay-service)해야 합니다. 
* 네트워크에서 타사의 외부 DHCP 서버를 사용하는 경우 [DHCP 릴레이 서비스를 생성](manage-dhcp.md#create-dhcp-relay-service)해야 합니다.  이 옵션의 경우 [릴레이 구성만 수행](manage-dhcp.md#create-dhcp-relay-service)합니다.


## <a name="add-a-vm-on-the-nsx-t-network-segment"></a>NSX-T 네트워크 세그먼트에 VM 추가

Azure VMware Solution vCenter에서 VM을 배포하고, 이를 사용하여 Azure VMware Solution 네트워크에서 다음에 대한 연결을 확인합니다.

- 인터넷
- Azure Virtual Networks
- 온-프레미스  

모든 vSphere 환경에서와 같이 VM을 배포합니다.  VM을 이전에 NSX-T에서 만든 네트워크 세그먼트 중 하나에 연결합니다.  

>[!NOTE]
>DHCP 서버를 설정하는 경우 여기서 VM에 대한 네트워크 구성을 가져옵니다(범위를 설정하는 것을 잊지 마세요).  고정으로 구성하려는 경우 일반적인 방법으로 구성합니다.

## <a name="test-the-nsx-t-segment-connectivity"></a>NSX-T 세그먼트 연결 테스트

이전 단계에서 만든 VM에 로그인하고, 연결을 확인합니다.

1. 인터넷에서 IP를 ping합니다.
2. 웹 브라우저에서 인터넷 사이트로 이동합니다.
3. Azure Virtual Network에 있는 점프 상자를 ping합니다.

이제 Azure VMware Solution이 가동 중이며, Azure Virtual Network 및 인터넷과의 연결을 성공적으로 설정했습니다.

## <a name="next-steps"></a>다음 단계

다음 섹션에서는 ExpressRoute를 통해 Azure VMware Solution을 온-프레미스 네트워크에 연결합니다.
> [!div class="nextstepaction"]
> [온-프레미스 환경에 Azure VMware Solution 연결](azure-vmware-solution-on-premises.md)
