---
title: Redis 인스턴스에 대 한 가상 네트워크-프리미엄 계층 Azure 캐시 구성
description: Redis 인스턴스에 대 한 프리미엄 계층 Azure 캐시에 대 한 가상 네트워크 지원을 만들고 관리 하는 방법을 알아봅니다.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.openlocfilehash: 94bbb9bb683f40d44d6649802b66bda6feeee218
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100375275"
---
# <a name="configure-virtual-network-support-for-a-premium-azure-cache-for-redis-instance"></a>Redis 인스턴스에 대 한 프리미엄 Azure 캐시에 대 한 가상 네트워크 지원 구성

[Azure Virtual Network](https://azure.microsoft.com/services/virtual-network/) 배포는 서브넷, 액세스 제어 정책 및 기타 기능을 사용 하 여 액세스를 추가로 제한 하는 향상 된 보안 및 격리 기능을 제공 합니다. Azure Cache for Redis 인스턴스는 가상 네트워크를 사용 하 여 구성 된 경우 공개적으로 주소를 지정할 수 없으며 가상 네트워크 내의 응용 프로그램 및 가상 컴퓨터 에서만 액세스할 수 있습니다. 이 문서에서는 Redis 용 프리미엄 계층 Azure 캐시 인스턴스에 대 한 가상 네트워크 지원을 구성 하는 방법을 설명 합니다.

> [!NOTE]
> Redis 용 Azure Cache는 클래식 배포 모델 및 Azure Resource Manager 가상 네트워크를 모두 지원 합니다.
> 

## <a name="set-up-virtual-network-support"></a>가상 네트워크 지원 설정

**Redis에 대 한 새 Azure cache** 창에서 캐시를 만드는 동안 가상 네트워크 지원이 구성 됩니다.

1. 프리미엄 계층 캐시를 만들려면 [Azure Portal](https://portal.azure.com) 에 로그인 하 고 **리소스 만들기** 를 선택 합니다. Azure Portal에서 캐시를 만드는 것 외에도 리소스 관리자 템플릿, PowerShell 또는 Azure CLI를 사용 하 여 만들 수 있습니다. Redis 인스턴스에 대 한 Azure 캐시를 만드는 방법에 대 한 자세한 내용은 [캐시 만들기](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)를 참조 하세요.

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="리소스 만들기를 보여 주는 스크린샷":::
   
1. **새로 만들기** 페이지에서 **데이터베이스** 를 선택 합니다. 그런 다음 **Azure Cache For Redis를** 선택 합니다.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Redis 용 Azure Cache를 선택 하는 방법을 보여 주는 스크린샷":::

1. **새 Redis Cache** 페이지에서 새 프리미엄 계층 캐시의 설정을 구성 합니다.
   
   | 설정      | 제안 값  | Description |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS 이름** | 전역적으로 고유한 이름을 입력합니다. | 캐시 이름은 1 ~ 007e; 63 자 사이의 문자열 이어야 하며 숫자, 문자 또는 하이픈만 포함 해야 합니다. 이름은 숫자 또는 문자로 시작 하 고 끝나야 하며 연속 하이픈을 포함할 수 없습니다. 캐시 인스턴스의 *호스트 이름* 은 *\<DNS name>.redis.cache.windows.net* 입니다. |
   | **구독** | 드롭다운 목록에서 구독을 선택합니다. | 이 구독 아래에 새 Azure Cache for Redis 인스턴스가 만들어집니다. |
   | **리소스 그룹** | 드롭다운 목록에서 리소스 그룹을 선택 하거나 **새로 만들기** 를 선택 하 고 새 리소스 그룹 이름을 입력 합니다. | 캐시 및 기타 리소스를 만들 리소스 그룹의 이름입니다. 모든 앱 리소스를 하나의 리소스 그룹에 배치하면 앱 리소스를 쉽게 관리하거나 삭제할 수 있습니다. |
   | **위치** | 드롭다운 목록에서 위치를 선택합니다. | 캐시를 사용할 다른 서비스와 가까이 있는 [Azure 지역](https://azure.microsoft.com/regions/)을 선택합니다. |
   | **캐시 유형** |프리미엄 계층 기능을 구성 하려면 드롭다운 목록에서 프리미엄 계층 캐시를 선택 합니다. 자세한 내용은 [Azure Cache for Redis 가격](https://azure.microsoft.com/pricing/details/cache/)을 참조하세요. |  가격 책정 계층은 캐시에 사용 가능한 크기, 성능 및 기능을 결정합니다. 자세한 내용은 [Azure Cache For Redis 개요](cache-overview.md)를 참조 하세요. |

1. **네트워킹** 탭을 선택 하거나 페이지 맨 아래에 있는 **네트워킹** 단추를 선택 합니다.

1. **네트워킹** 탭에서 연결 방법으로 **가상 네트워크** 를 선택 합니다. 새 가상 네트워크를 사용 하려면 먼저 [Azure Portal를 사용 하 여 가상 네트워크 만들기](../virtual-network/manage-virtual-network.md#create-a-virtual-network) 의 단계를 수행 하거나 [Azure Portal를 사용 하 여 가상 네트워크 만들기 (클래식)](/previous-versions/azure/virtual-network/virtual-networks-create-vnet-classic-pportal)의 단계를 수행 하 여 만듭니다. 그런 다음 **Redis에 대 한 새 Azure Cache** 창으로 돌아와 프리미엄 계층 캐시를 만들고 구성 합니다.

   > [!IMPORTANT]
   > Redis 용 Azure Cache를 리소스 관리자 가상 네트워크에 배포 하는 경우 Redis 인스턴스에 대 한 Azure Cache를 제외 하 고 다른 리소스를 포함 하지 않는 전용 서브넷에 캐시가 있어야 합니다. 다른 리소스를 포함 하는 리소스 관리자 가상 네트워크 서브넷에 Redis 용 Azure 캐시 인스턴스를 배포 하려는 경우 배포에 실패 합니다.
   > 
   > 

   | 설정      | 제안 값  | Description |
   | ------------ |  ------- | -------------------------------------------------- |
   | **가상 네트워크** | 드롭다운 목록에서 가상 네트워크를 선택 합니다. | 캐시와 동일한 구독 및 위치에 있는 가상 네트워크를 선택 합니다. |
   | **서브넷** | 드롭다운 목록에서 서브넷을 선택 합니다. | 서브넷의 주소 범위는 CIDR 표기법 (예: 192.168.1.0/24) 이어야 합니다. 가상 네트워크의 주소 공간에 포함되어야 합니다. |
   | **고정 IP 주소** | 필드 고정 IP 주소를 입력 하십시오. | 고정 IP 주소를 지정 하지 않으면 IP 주소가 자동으로 선택 됩니다. |

   > [!IMPORTANT]
   > Azure는 각 서브넷 내의 일부 IP 주소를 예약하며, 이러한 주소는 사용할 수 없습니다. 서브넷의 첫 번째 및 마지막 IP 주소는 Azure 서비스에 사용되는 3개 이상의 주소와 함께 프로토콜 적합성을 위해 예약됩니다. 자세한 내용은 [이러한 서브넷 내에서 IP 주소를 사용하는데 제한 사항이 있습니까?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
   > 
   > Azure 가상 네트워크 인프라에서 사용 하는 IP 주소 외에도, 서브넷의 각 Redis 용 Azure Cache 인스턴스는 분할 당 두 개의 IP 주소와 부하 분산 장치에 대 한 추가 IP 주소 하나를 사용 합니다. 비클러스터형 캐시는 하나의 분할 된 데이터베이스가 있는 것으로 간주 됩니다.
   > 

1. **다음: 고급** 탭을 선택 하거나 페이지 맨 아래에 있는 **다음: 고급** 단추를 선택 합니다.

1. 프리미엄 계층 캐시 인스턴스에 대 한 **고급** 탭에서 TLS가 아닌 포트, 클러스터링 및 데이터 지 속성에 대 한 설정을 구성 합니다.

1. **다음: 태그** 탭을 선택 하거나 페이지 맨 아래에 있는 **다음: 태그** 단추를 선택 합니다.

1. 필요에 따라 리소스를 범주화 하려면 **태그** 탭에서 이름 및 값을 입력 합니다.

1. **검토 + 만들기** 를 선택합니다. Azure에서 구성의 유효성을 검사 하는 **검토 + 만들기** 탭으로 이동 됩니다.

1. 녹색 **유효성 검사 통과** 메시지가 표시 되 면 **만들기** 를 선택 합니다.

캐시를 만드는 데 잠시 시간이 걸립니다. Azure Cache for Redis **개요** 페이지에서 진행률을 모니터링할 수 있습니다. **상태** 가 **실행 중** 으로 표시되면 캐시를 사용할 준비가 된 것입니다. 캐시를 만든 후에는 **리소스** 메뉴에서 **Virtual Network** 를 선택 하 여 가상 네트워크에 대 한 구성을 볼 수 있습니다.

![가상 네트워크][redis-cache-vnet-info]

가상 네트워크를 사용 하는 경우 Azure Cache for Redis 인스턴스에 연결 하려면 다음 예제와 같이 연결 문자열에 캐시의 호스트 이름을 지정 합니다.

```csharp
private static Lazy<ConnectionMultiplexer>
    lazyConnection = new Lazy<ConnectionMultiplexer> (() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}
```

## <a name="azure-cache-for-redis-virtual-network-faq"></a>Azure Cache for Redis 가상 네트워크 FAQ

Azure Cache for Redis 크기 조정에 대해 자주 묻는 질문과 대답이 나와 있는 목록은 다음과 같습니다.

* Redis 및 virtual network에 대 한 Azure Cache의 일반적인 잘못 된 구성 문제는 무엇 인가요?
* [내 캐시가 가상 네트워크에서 작동 하는지 확인 하려면 어떻게 해야 하나요?](#how-can-i-verify-that-my-cache-is-working-in-a-virtual-network)
* 가상 네트워크에서 Azure Cache for Redis 인스턴스에 연결 하려고 할 때 원격 인증서가 유효 하지 않다는 오류가 표시 되는 이유는 무엇 인가요?
* [표준 또는 기본 캐시에서 가상 네트워크를 사용할 수 있나요?](#can-i-use-virtual-networks-with-a-standard-or-basic-cache)
* 일부 서브넷에서 Redis 인스턴스에 대 한 Azure Cache를 만드는 작업이 실패 하는 이유는 무엇 인가요?
* [서브넷 주소 공간 요구 사항은 무엇입니까?](#what-are-the-subnet-address-space-requirements)
* [캐시가 가상 네트워크에서 호스트 되는 경우 모든 캐시 기능이 작동 하나요?](#do-all-cache-features-work-when-a-cache-is-hosted-in-a-virtual-network)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-virtual-networks"></a>Redis 및 virtual network에 대 한 Azure Cache의 일반적인 잘못 된 구성 문제는 무엇 인가요?

Azure Cache for Redis가 가상 네트워크에서 호스트 되는 경우 다음 표의 포트가 사용 됩니다.

>[!IMPORTANT]
>다음 테이블의 포트가 차단 되 면 캐시가 제대로 작동 하지 않을 수 있습니다. 이러한 포트가 하나 이상 차단 되는 것은 가상 네트워크에서 Redis 용 Azure Cache를 사용 하는 경우 가장 일반적인 잘못 된 구성 문제입니다.
> 

- [아웃바운드 포트 요구 사항](#outbound-port-requirements)
- [인바운드 포트 요구 사항](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>아웃바운드 포트 요구 사항

아웃 바운드 포트 요구 사항은 9 가지가 있습니다. 이러한 범위의 아웃 바운드 요청은 캐시가 작동 하는 데 필요한 다른 서비스 또는 노드 간 통신용 Redis 서브넷 내부에 대 한 아웃 바운드입니다. 지리적 복제의 경우 기본 및 복제본 캐시의 서브넷 간 통신을 위한 추가 아웃 바운드 요구 사항이 있습니다.

| 포트 | Direction | 전송 프로토콜 | 용도 | 로컬 IP | 원격 IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |아웃바운드 |TCP |Azure Storage/PKI (인터넷)에 대 한 Redis 종속성 | (Redis 서브넷) |* <sup>3-4</sup> |
| 443 | 아웃바운드 | TCP | Azure Key Vault 및 Azure Monitor에 대 한 Redis 종속성 | (Redis 서브넷) | AzureKeyVault, AzureMonitor <sup>1</sup> |
| 53 |아웃바운드 |TCP/UDP |DNS에 대 한 Redis 종속성 (인터넷/가상 네트워크) | (Redis 서브넷) | 168.63.129.16 및 169.254.169.254 <sup>2</sup> 및 서브넷 <sup>3</sup> 에 대 한 모든 사용자 지정 DNS 서버 |
| 8443 |아웃바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) | (Redis 서브넷) |
| 10221-10231 |아웃바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) | (Redis 서브넷) |
| 20226 |아웃바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) |
| 13000-13999 |아웃바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) |
| 15000-15999 |아웃바운드 |TCP |Redis 및 지역에서 복제에 대 한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) (지역 복제본 피어 서브넷) |
| 6379-6380 |아웃바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) |

<sup>1</sup> nsgs (네트워크 보안 그룹) 리소스 관리자와 함께 AzureKeyVault 및 azuremonitor 서비스 태그를 사용할 수 있습니다.

<sup>2</sup> Microsoft에서 소유 하는 이러한 IP 주소는 Azure DNS 서비스를 제공 하는 호스트 VM을 해결 하는 데 사용 됩니다.

<sup>3</sup> 사용자 지정 dns 서버 또는 사용자 지정 dns를 무시 하는 최신 Redis 캐시가 없는 서브넷에는이 정보가 필요 하지 않습니다.

<sup>4</sup> 자세한 내용은 [추가 가상 네트워크 연결 요구 사항](#additional-virtual-network-connectivity-requirements)을 참조 하세요.

#### <a name="geo-replication-peer-port-requirements"></a>지역 복제 피어 포트 요구 사항

Azure 가상 네트워크에서 캐시 간의 지역에서 복제를 사용 하는 경우 인바운드 *및* 아웃 바운드 방향의 전체 서브넷에 대 한 포트 15000-15999을 두 캐시로 모두 차단 해제 합니다. 이 구성을 사용 하면 이후 지역 장애 조치 (failover)가 있는 경우에도 서브넷의 모든 복제본 구성 요소가 서로 직접 통신할 수 있습니다.

#### <a name="inbound-port-requirements"></a>인바운드 포트 요구 사항

8개의 인바운드 포트 범위 요구 사항이 있습니다. 이러한 범위의 인바운드 요청은 동일한 가상 네트워크에서 호스트 되는 다른 서비스에서 인바운드 또는 Redis 서브넷 통신 내부에 있습니다.

| 포트 | Direction | 전송 프로토콜 | 용도 | 로컬 IP | 원격 IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |인바운드 |TCP |Redis에 대한 클라이언트 통신, Azure 부하 분산 | (Redis 서브넷) | (Redis 서브넷), (클라이언트 서브넷), AzureLoadBalancer <sup>1</sup> |
| 8443 |인바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) |
| 8500 |인바운드 |TCP/UDP |Azure 부하 분산 | (Redis 서브넷) | AzureLoadBalancer |
| 10221-10231 |인바운드 |TCP |Redis 클러스터에 대 한 클라이언트 통신, Redis에 대 한 내부 통신 | (Redis 서브넷) |(Redis 서브넷), AzureLoadBalancer, (클라이언트 서브넷) |
| 13000-13999 |인바운드 |TCP |Redis 클러스터에 대한 클라이언트 통신, Azure 부하 분산 | (Redis 서브넷) | (Redis 서브넷), (클라이언트 서브넷), AzureLoadBalancer |
| 15000-15999 |인바운드 |TCP |Redis 클러스터에 대 한 클라이언트 통신, Azure 부하 분산 및 지역에서 복제 | (Redis 서브넷) | (Redis 서브넷), (클라이언트 서브넷), AzureLoadBalancer, (지리적 복제본 피어 서브넷) |
| 16001 |인바운드 |TCP/UDP |Azure 부하 분산 | (Redis 서브넷) | AzureLoadBalancer |
| 20226 |인바운드 |TCP |Redis에 대한 내부 통신 | (Redis 서브넷) |(Redis 서브넷) |

<sup>1</sup> nsg 규칙을 작성 하기 위해 클래식 배포 모델에 대 한 리소스 관리자 또는 AZURE_LOADBALANCER 서비스 태그 azureloadbalancer를 사용할 수 있습니다.

#### <a name="additional-virtual-network-connectivity-requirements"></a>추가 가상 네트워크 연결 요구 사항

Azure Cache for Redis에 대 한 네트워크 연결 요구 사항은 처음에 가상 네트워크에서 충족 되지 않을 수 있습니다. Redis 용 Azure Cache는 가상 네트워크 내에서 사용 하는 경우 다음 항목이 모두 제대로 작동 해야 합니다.

* 전세계 Azure Storage 엔드포인트에 아웃바운드 네트워크 연결. Redis 인스턴스에 대 한 Azure Cache와 동일한 지역에 있는 끝점과 *다른* Azure 지역에 있는 저장소 끝점도 포함 됩니다. Azure Storage 끝점은 다음 DNS 도메인에서 확인 됩니다. *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* 및 *file.core.windows.net*
* *Ocsp.digicert.com*, *crl4.digicert.com*, *ocsp.msocsp.com*, *mscrl.microsoft.com*, *crl3.digicert.com*, *cacerts.digicert.com*, *oneocsp.microsoft.com* 및 *crl.microsoft.com* 에 대 한 아웃 바운드 네트워크 연결입니다. TLS/SSL 기능을 지원 하려면이 연결이 필요 합니다.
* 가상 네트워크에 대한 DNS 구성은 이전 시점에 언급된 엔드포인트 및 도메인을 모두 해결할 수 있어야 합니다. 유효한 DNS 인프라를 구성하고 가상 네트워크에 유지 관리하여 DNS 요구를 충족할 수 있습니다.
* 다음 Azure Monitor 끝점에 대 한 아웃 바운드 네트워크 연결: *shoebox2-black.shoebox2.metrics.nsatc.net*, *north-prod2.prod2.metrics.nsatc.net*, *azglobal-black.azglobal.metrics.nsatc.net*, *shoebox2-red.shoebox2.metrics.nsatc.net*, *EAST-PROD2.PROD2.METRICS.NSATC.NET*, *azglobal-red.azglobal.metrics.nsatc.net*, *shoebox3.prod.microsoftmetrics.com*, *shoebox3-red.prod.microsoftmetrics.com* 및 *shoebox3-black.prod.microsoftmetrics.com* DNS 도메인에서 확인 됩니다.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-virtual-network"></a>내 캐시가 가상 네트워크에서 작동 하는지 확인 하려면 어떻게 해야 하나요?

>[!IMPORTANT]
>가상 네트워크에서 호스트 되는 Redis 인스턴스에 대 한 Azure 캐시에 연결 하는 경우 캐시 클라이언트는 동일한 Azure 지역 내에서 가상 네트워크 피어 링을 사용 하도록 설정 된 가상 네트워크 또는 동일한 가상 네트워크에 있어야 합니다. 전역 가상 네트워크 피어 링은 현재 지원 되지 않습니다. 이 요구 사항은 모든 테스트 응용 프로그램 또는 진단 ping 도구에 적용 됩니다. 클라이언트 응용 프로그램이 호스트 되는 위치에 관계 없이 클라이언트의 네트워크 트래픽이 Redis 인스턴스에 대 한 Azure 캐시에 도달할 수 있도록 NSGs 또는 기타 네트워크 계층을 구성 해야 합니다.
>

이전 섹션에 설명 된 대로 포트 요구 사항이 구성 된 후 다음 단계를 수행 하 여 캐시가 작동 하는지 확인할 수 있습니다.

- 모든 캐시 노드를 [다시 부팅](cache-administration.md#reboot)합니다. [인바운드 포트 요구](cache-how-to-premium-vnet.md#inbound-port-requirements) 사항 및 [아웃 바운드 포트 요구 사항](cache-how-to-premium-vnet.md#outbound-port-requirements)에 설명 된 대로 필요한 모든 캐시 종속성에 연결할 수 없는 경우 캐시를 다시 시작할 수 없습니다.
- 캐시 노드가 다시 시작 된 후 Azure Portal의 캐시 상태에서 보고 되는 경우 다음 테스트를 수행할 수 있습니다.
  - [Tcping](https://www.elifulkerson.com/projects/tcping.php)을 사용 하 여 캐시와 동일한 가상 네트워크 내에 있는 컴퓨터에서 포트 6380을 사용 하 여 캐시 끝점을 Ping 합니다. 예를 들면 다음과 같습니다.
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    `tcping`이 도구는 포트가 열려 있음을 보고 하는 경우 가상 네트워크에 있는 클라이언트에서의 연결에 대 한 캐시를 사용할 수 있습니다.

  - 또 다른 테스트 방법은 캐시에 연결하고 캐시에서 일부 항목을 추가 및 검색하는 테스트 캐시 클라이언트(StackExchange.Redis를 사용하는 간단한 콘솔 애플리케이션일 수 있음)를 만드는 것입니다. 캐시와 동일한 가상 네트워크에 있는 VM에 샘플 클라이언트 응용 프로그램을 설치 합니다. 그런 다음 실행 하 여 캐시에 대 한 연결을 확인 합니다.


### <a name="when-i-try-to-connect-to-my-azure-cache-for-redis-instance-in-a-virtual-network-why-do-i-get-an-error-stating-the-remote-certificate-is-invalid"></a>가상 네트워크에서 Azure Cache for Redis 인스턴스에 연결 하려고 할 때 원격 인증서가 유효 하지 않다는 오류가 표시 되는 이유는 무엇 인가요?

가상 네트워크에서 Redis 인스턴스에 대 한 Azure 캐시에 연결 하려고 하면 다음과 같은 인증서 유효성 검사 오류가 표시 됩니다.

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

IP 주소를 통해 호스트에 연결 하는 것이 원인일 수 있습니다. 호스트 이름을 사용 하는 것이 좋습니다. 즉, 다음 문자열을 사용 합니다.

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

다음 연결 문자열과 유사한 IP 주소는 사용하지 마세요.

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

DNS 이름을 확인할 수 없는 경우 일부 클라이언트 라이브러리에는 `sslHost` Stackexchange 클라이언트에서 제공 하는와 같은 구성 옵션이 포함 됩니다. 이 옵션을 사용 하면 인증서 유효성 검사에 사용 되는 호스트 이름을 재정의할 수 있습니다. 예를 들면 다음과 같습니다.

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-virtual-networks-with-a-standard-or-basic-cache"></a>표준 또는 기본 캐시에서 가상 네트워크를 사용할 수 있나요?

가상 네트워크는 프리미엄 계층 캐시에만 사용할 수 있습니다.

### <a name="why-does-creating-an-azure-cache-for-redis-instance-fail-in-some-subnets-but-not-others"></a>일부 서브넷에서 Redis 인스턴스에 대 한 Azure Cache를 만드는 작업이 실패 하는 이유는 무엇 인가요?

Azure Cache for Redis 인스턴스를 가상 네트워크에 배포 하는 경우 캐시는 다른 리소스 종류가 포함 되지 않은 전용 서브넷에 있어야 합니다. Redis 용 Azure Cache 인스턴스를 Azure 애플리케이션 게이트웨이 인스턴스 및 아웃 바운드 NAT와 같은 다른 리소스를 포함 하는 리소스 관리자 가상 네트워크 서브넷에 배포 하려고 하면 일반적으로 배포가 실패 합니다. Redis 인스턴스에 대 한 새 Azure Cache를 만들려면 먼저 다른 유형의 기존 리소스를 삭제 해야 합니다.

또한 서브넷에 사용할 수 있는 IP 주소가 충분 해야 합니다.

### <a name="what-are-the-subnet-address-space-requirements"></a>서브넷 주소 공간 요구 사항은 무엇입니까?

Azure는 각 서브넷 내의 일부 IP 주소를 예약하며, 이러한 주소는 사용할 수 없습니다. 서브넷의 첫 번째 및 마지막 IP 주소는 Azure 서비스에 사용되는 3개 이상의 주소와 함께 프로토콜 적합성을 위해 예약됩니다. 자세한 내용은 [이러한 서브넷 내에서 IP 주소를 사용하는데 제한 사항이 있습니까?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure 가상 네트워크 인프라에서 사용 하는 IP 주소 외에도, 서브넷의 각 Redis 인스턴스에 대 한 각 Azure Cache는 클러스터 분할 당 두 개의 IP 주소를 사용 하 고 추가 복제본의 추가 IP 주소를 사용 합니다 (있는 경우). 부하 분산 장치에는 하나의 추가 IP 주소가 사용 됩니다. 비클러스터형 캐시는 하나의 분할 된 데이터베이스가 있는 것으로 간주 됩니다.

### <a name="do-all-cache-features-work-when-a-cache-is-hosted-in-a-virtual-network"></a>캐시가 가상 네트워크에서 호스트 되는 경우 모든 캐시 기능이 작동 하나요?

캐시가 가상 네트워크의 일부인 경우 가상 네트워크의 클라이언트만 캐시에 액세스할 수 있습니다. 결과적으로 다음 캐시 관리 기능은 현재 작동 하지 않습니다.

* **Redis console**: Redis 콘솔은 일반적으로 가상 네트워크에 연결 되지 않은 개발자 컴퓨터에 있는 로컬 브라우저에서 실행 되기 때문에 캐시에 연결할 수 없습니다.

## <a name="use-expressroute-with-azure-cache-for-redis"></a>Azure Cache for Redis에서 ExpressRoute 사용

고객은 [Azure express](https://azure.microsoft.com/services/expressroute/) 경로 회로를 가상 네트워크 인프라에 연결할 수 있습니다. 이러한 방식으로 Azure로 온-프레미스 네트워크를 확장 합니다.

기본적으로 새로 만든 Express 경로 회로는 가상 네트워크에서 강제 터널링 (0.0.0.0/0 기본 경로 보급 알림)을 수행 하지 않습니다. 따라서 아웃 바운드 인터넷 연결은 가상 네트워크에서 직접 허용 됩니다. 클라이언트 응용 프로그램은 Redis 인스턴스에 대 한 Azure Cache를 포함 하는 다른 Azure 끝점에 연결할 수 있습니다.

일반적인 고객 구성은 강제 터널링 (기본 경로 보급)을 사용 하 여 아웃 바운드 인터넷 트래픽이 온-프레미스로 전송 되도록 하는 것입니다. 아웃 바운드 트래픽이 온-프레미스에서 차단 되는 경우이 트래픽 흐름은 Redis 용 Azure 캐시와의 연결을 중단 합니다 .이에 따라 Azure Cache for Redis 인스턴스는 해당 종속성과 통신할 수 없습니다.

해결 방법은 Azure Cache for Redis 인스턴스를 포함 하는 서브넷에서 하나 이상의 UDRs (사용자 정의 경로)를 정의 하는 것입니다. UDR이 정의한 특정 서브넷 경로는 기본 경로대신 적용됩니다.

가능한 경우 다음 구성을 사용합니다.

* Express 경로 구성은 0.0.0.0/0을 보급 하 고 기본적으로 모든 아웃 바운드 트래픽 온-프레미스를 강제로 터널링 합니다.
* Azure Cache for Redis 인스턴스를 포함 하는 서브넷에 적용 된 UDR은 공용 인터넷에 대 한 TCP/IP 트래픽의 작업 경로를 사용 하 여 0.0.0.0/0을 정의 합니다. 예를 들어, 다음 홉 유형을 *인터넷* 으로 설정 합니다.

이러한 단계의 결합 된 효과는 서브넷 수준 UDR이 Redis 인스턴스에 대 한 Azure Cache에서 아웃 바운드 인터넷 액세스를 보장 하는 Express 경로 강제 터널링 보다 우선적으로 적용 된다는 것입니다.

Express 경로를 사용 하 여 온-프레미스 응용 프로그램에서 Redis 인스턴스에 대 한 Azure 캐시에 연결 하는 것은 성능상의 이유로 일반적인 사용 시나리오가 아닙니다. 최상의 성능을 위해 Redis 클라이언트용 Azure Cache는 Redis 용 Azure Cache 인스턴스와 동일한 지역에 있어야 합니다.

>[!IMPORTANT]
>UDR에 정의 된 경로는 Express 경로 구성에서 보급 된 경로 보다 우선 하도록 구체적 이어야 *합니다* . 다음 예제에서는 광범위 한 0.0.0.0/0 주소 범위를 사용 하며,이에 따라 더 구체적인 주소 범위를 사용 하는 경로 알림에서 실수로 재정의할 수 있습니다.

>[!WARNING]
>Redis 용 Azure Cache는 *공용 피어 링 경로에서 개인 피어 링 경로에 대 한 경로를 잘못 교차 보급* 하는 express 경로 구성에서 지원 되지 않습니다. 구성된 공용 피어링이 있는 ExpressRoute 구성은 다양한 Microsoft Azure IP 주소 범위 세트에 대해 Microsoft에서 경로 보급을 받습니다. 이러한 주소 범위를 프라이빗 피어링 경로에서 잘못 교차 보급하는 경우 Azure Cache for Redis 인스턴스의 서브넷에 있는 모든 아웃바운드 네트워크 패킷이 고객의 온-프레미스 네트워크 인프라로 잘못 강제 터널링됩니다. 이 네트워크 흐름으로 인해 Azure Cache for Redis가 중단됩니다. 이 문제를 해결하려면 공용 피어링 경로에서 프라이빗 피어링 경로로 이어진 교차 보급 경로를 중지합니다.

UDRs의 배경 정보는 [가상 네트워크 트래픽 라우팅](../virtual-network/virtual-networks-udr-overview.md)에서 사용할 수 있습니다.

ExpressRoute에 대한 자세한 내용은 [ExpressRoute 기술 개요](../expressroute/expressroute-introduction.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

Azure Cache for Redis 기능에 대해 자세히 알아보세요.

* [Redis Premium 서비스 계층에 대 한 Azure 캐시](cache-overview.md#service-tiers)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png
