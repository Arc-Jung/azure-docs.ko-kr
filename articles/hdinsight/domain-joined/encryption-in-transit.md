---
title: 전송 중인 Azure HDInsight 암호화
description: Azure HDInsight 클러스터에 대 한 전송 암호화를 제공 하는 보안 기능에 대해 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: ba1542d1bb10933edb34b697f1c81cc5e3e7f1c9
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2020
ms.locfileid: "97705373"
---
# <a name="ipsec-encryption-in-transit-for-azure-hdinsight"></a>Azure HDInsight에 대 한 전송에서 IPSec 암호화

이 문서에서는 Azure HDInsight 클러스터 노드 간 통신을 위한 전송 암호화 구현에 대해 설명 합니다.

## <a name="background"></a>배경

Azure HDInsight는 엔터프라이즈 데이터를 보호 하기 위한 다양 한 보안 기능을 제공 합니다. 이러한 솔루션은 경계 보안, 인증, 권한 부여, 감사, 암호화 및 규정 준수의 핵심 요소로 그룹화 되어 있습니다. 암호화는 미사용 및 전송 중에 데이터에 적용할 수 있습니다.

미사용 암호화는 HDInsight 클러스터의 일부인 Azure Vm의 디스크 암호화 뿐만 아니라 Azure storage 계정에서 서버 쪽 암호화를 통해 적용 됩니다.

HDInsight에서 전송 중인 데이터의 암호화는 클러스터 게이트웨이와 클러스터 노드 간에 [IPSec (인터넷 프로토콜 보안)](https://wikipedia.org/wiki/IPsec) 에 액세스 하기 위한 [TLS (전송 계층 보안](../transport-layer-security.md) )를 통해 수행 됩니다. IPSec은 모든 헤드 노드, 작업자 노드,에 지 노드 및 아웃 들 노드 사이에서 선택적으로 사용할 수 있습니다. Windows 기반 Vm 및 클러스터의 다른 linux 기반 노드인 게이트웨이 또는 [id 브로커](./identity-broker.md) 노드 간의 트래픽에 대해서는 사용할 수 없습니다.

## <a name="enable-encryption-in-transit"></a>전송 중 암호화 사용

### <a name="azure-portal"></a>Azure portal

Azure Portal를 사용 하 여 전송 중 암호화가 설정 된 새 클러스터를 만들려면 다음 단계를 수행 합니다.

1. 정상적인 클러스터 만들기 프로세스를 시작 합니다. 초기 클러스터 생성 단계 [는 Azure Portal을 사용 하 여 HDInsight에서 Linux 기반 클러스터 만들기](../hdinsight-hadoop-create-linux-clusters-portal.md) 를 참조 하세요.
1. **기본 사항** 및 **저장소** 탭을 완료 합니다. **보안 + 네트워킹** 탭으로 이동 합니다.

    :::image type="content" source="media/encryption-in-transit/create-cluster-security-networking-tab.png" alt-text="클러스터-보안 및 네트워킹 탭을 만듭니다.":::

1. **보안 + 네트워킹** 탭에서 **전송 중 암호화 사용** 확인란을 선택 합니다.

    :::image type="content" source="media/encryption-in-transit/enable-encryption-in-transit.png" alt-text="클러스터 만들기-전송 중 암호화를 사용 합니다.":::

### <a name="create-a-cluster-with-encryption-in-transit-enabled-through-the-azure-cli"></a>Azure CLI를 통해 전송에서 암호화를 사용 하 여 클러스터 만들기

속성을 사용 하 여 전송 중인 암호화를 사용할 수 `isEncryptionInTransitEnabled` 있습니다.

[샘플 템플릿 및 매개 변수 파일을 다운로드할](https://github.com/Azure-Samples/hdinsight-enterprise-security)수 있습니다. 템플릿 및 Azure CLI 코드 조각을 사용 하기 전에 다음 자리 표시자를 올바른 값으로 바꿉니다.

| 자리 표시자 | Description |
|---|---|
| `<SUBSCRIPTION_ID>` | Azure 구독의 ID입니다. |
| `<RESOURCE_GROUP>` | 새 클러스터 및 저장소 계정을 만들 리소스 그룹입니다. |
| `<STORAGEACCOUNTNAME>` | 클러스터와 함께 사용 해야 하는 기존 저장소 계정입니다. 이름은 형식 이어야 합니다. `ACCOUNTNAME.blob.core.windows.net` |
| `<CLUSTERNAME>` | HDInsight 클러스터의 이름입니다. |
| `<PASSWORD>` | SSH 및 Ambari 대시보드를 사용 하 여 클러스터에 로그인 할 때 선택한 암호입니다. |
| `<VNET_NAME>` | 클러스터가 배포 될 가상 네트워크입니다. |

아래 코드 조각은 다음과 같은 초기 단계를 수행 합니다.

1. Azure 계정에 로그인 합니다.
1. 만들기 작업을 수행할 활성 구독을 설정 합니다.
1. 새 배포 작업에 대 한 새 리소스 그룹을 만듭니다.
1. 템플릿을 배포 하 여 새 클러스터를 만듭니다.

```azurecli
az login
az account set --subscription <SUBSCRIPTION_ID>

# Create resource group
az group create --name <RESOURCEGROUPNAME> --location eastus2

az deployment group create --name HDInsightEnterpriseSecDeployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file hdinsight-enterprise-security.json \
    --parameters parameters.json
```

## <a name="next-steps"></a>다음 단계

* [Azure HDInsight의 엔터프라이즈 보안 개요](hdinsight-security-overview.md)
* [Azure Active Directory 사용자를 HDInsight 클러스터와 동기화](../disk-encryption.md)합니다.
