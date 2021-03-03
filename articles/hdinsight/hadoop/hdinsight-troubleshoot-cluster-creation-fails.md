---
title: Azure HDInsight를 사용 하 여 클러스터 만들기 오류 문제 해결
description: Azure HDInsight에 대 한 Apache 클러스터 생성 문제를 해결 하는 방법을 알아봅니다.
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 04/14/2020
ms.openlocfilehash: f909f5714a049f63032c8a23ca1731777a40d332
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101702864"
---
# <a name="troubleshoot-cluster-creation-failures-with-azure-hdinsight"></a>Azure HDInsight를 사용 하 여 클러스터 만들기 오류 문제 해결

다음 문제는 클러스터를 만드는 데 실패 하는 가장 일반적인 근본 원인입니다.

- 권한 문제
- 리소스 정책 제한
- 방화벽
- 리소스 잠금
- 지원 되지 않는 구성 요소 버전
- 스토리지 계정 이름 제한 사항
- 서비스 중단

## <a name="permissions-issues"></a>권한 이슈

Azure Data Lake Storage Gen2를 사용 하 고 "" 오류를 수신 하는 경우 `AmbariClusterCreationFailedErrorCode` :::no-loc text="Internal server error occurred while processing the request. Please retry the request or contact support."::: Azure Portal를 열고 저장소 계정으로 이동한 다음 Access Control (IAM)에서 **저장소 blob 데이터 참가자** 또는 **저장소 blob 데이터 소유자** 역할에 구독에 **할당 된 사용자 관리 id** 에 대 한 액세스 권한을 할당 했는지 확인 합니다. 자세한 지침은 [Data Lake Storage Gen2에서 관리 되는 id에 대 한 사용 권한 설정](../hdinsight-hadoop-use-data-lake-storage-gen2-portal.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2) 을 참조 하세요.

Azure Data Lake Storage Gen1를 사용 하는 경우 설치 및 구성 지침 [Azure HDInsight 클러스터와 Azure Data Lake Storage Gen1 사용](../hdinsight-hadoop-use-data-lake-storage-gen1.md)을 참조 하세요. Data Lake Storage Gen1은 HBase 클러스터에 대해 지원 되지 않으며 HDInsight 버전 4.0에서 지원 되지 않습니다.

Azure Storage 사용 하는 경우 클러스터를 만드는 동안 저장소 계정 이름이 유효한 지 확인 합니다.

## <a name="resource-policy-restrictions"></a>리소스 정책 제한

구독 기반 Azure 정책은 공용 IP 주소 만들기를 거부할 수 있습니다. HDInsight 클러스터를 생성하려면 두 개의 공용 IP가 필요합니다.  

일반적으로 다음 정책은 클러스터 생성에 영향을 줄 수 있습니다.

* 정책으로 인해 구독 내에서 부하 분산 장치 & IP 주소를 만들 수 없습니다.
* 저장소 계정을 만들지 못하게 하는 정책입니다.
* 네트워킹 리소스 (IP 주소/부하 분산 장치)의 삭제를 방지 하는 정책입니다.

## <a name="firewalls"></a>방화벽

가상 네트워크 또는 저장소 계정의 방화벽은 HDInsight 관리 IP 주소와의 통신을 거부할 수 있습니다.

아래 표에 있는 IP 주소의 트래픽을 허용 합니다.

| 원본 IP 주소 | 대상 | 방향 |
|---|---|---|
| 168.61.49.99 | *:443 | 인바운드 |
| 23.99.5.239 | *:443 | 인바운드 |
| 168.61.48.131 | *:443 | 인바운드 |
| 138.91.141.162 | *:443 | 인바운드 |

또한 클러스터를 만든 지역과 관련 된 IP 주소도 추가 합니다. 각 Azure 지역에 대 한 주소 목록은 [HDInsight 관리 IP 주소](../hdinsight-management-ip-addresses.md) 를 참조 하세요.

Express 경로 또는 사용자 지정 DNS 서버를 사용 하는 경우 [Azure HDInsight에 대 한 가상 네트워크 계획-여러 네트워크 연결](../hdinsight-plan-virtual-network-deployment.md#multinet)을 참조 하세요.

## <a name="resources-locks"></a>리소스 잠금  

[가상 네트워크 및 리소스 그룹에 대 한 잠금이](../../azure-resource-manager/management/lock-resources.md)없는지 확인 합니다. 리소스 그룹이 잠겨 있으면 클러스터를 만들거나 삭제할 수 없습니다. 

## <a name="unsupported-component-versions"></a>지원 되지 않는 구성 요소 버전

솔루션에서 [지원 되는 버전의 Azure HDInsight 및 Apache Hadoop 구성 요소](../hdinsight-component-versioning.md) 를 사용 하 고 있는지 확인 합니다.  

## <a name="storage-account-name-restrictions"></a>스토리지 계정 이름 제한 사항

스토리지 계정 이름은 24자를 초과할 수 없으며 특수 문자가 있으면 안 됩니다. 이 제한 사항은 스토리지 계정의 기본 컨테이너 이름에도 적용됩니다.

클러스터를 만드는 경우에도 다른 명명 제한이 적용 됩니다. 자세한 내용은 [클러스터 이름 제한](../hdinsight-hadoop-provision-linux-clusters.md#cluster-name)을 참조 하세요.

## <a name="service-outages"></a>서비스 중단

잠재적인 중단 또는 서비스 문제에 대 한 [Azure 상태](https://status.azure.com) 를 확인 합니다.

## <a name="next-steps"></a>다음 단계

* [Azure Virtual Network를 사용하여 Azure HDInsight 확장](../hdinsight-plan-virtual-network-deployment.md)
* [Azure HDInsight 클러스터에 Azure Data Lake Storage Gen2 사용](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Azure HDInsight 클러스터에서 Azure Storage 사용](../hdinsight-hadoop-use-blob-storage.md)
* [Apache Hadoop, Apache Spark, Apache Kafka 등을 사용하여 HDInsight에서 클러스터 설정](../hdinsight-hadoop-provision-linux-clusters.md)
