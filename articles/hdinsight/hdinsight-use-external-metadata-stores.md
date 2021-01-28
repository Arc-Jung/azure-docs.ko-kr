---
title: 외부 메타데이터 저장소 사용 - Azure HDInsight
description: Azure HDInsight 클러스터에서 외부 메타 데이터 저장소를 사용 합니다.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 08/06/2020
ms.openlocfilehash: d36c8f1f592bbe714a9e31cad8131523049f29ad
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98931367"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure HDInsight에서 외부 메타데이터 저장소 사용

HDInsight를 사용 하면 외부 데이터 저장소를 사용 하 여 데이터 및 메타 데이터를 제어할 수 있습니다. 이 기능은 [Apache Hive metastore](#custom-metastore), [apache Oozie Metastore](#apache-oozie-metastore)및 [apache Ambari 데이터베이스](#custom-ambari-db)에서 사용할 수 있습니다.

HDInsight의 Apache Hive 메타스토어는 Apache Hadoop 아키텍처의 핵심 부분입니다. Metastore는 중앙 스키마 리포지토리입니다. Metastore은 Apache Spark, LLAP (Interactive Query), Presto 또는 Apache Pig와 같은 기타 빅 데이터 액세스 도구에서 사용 됩니다. HDInsight는 Azure SQL Database를 Hive 메타스토어로 사용합니다.

![HDInsight Hive 메타데이터 저장소 아키텍처](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

두 가지 방법을 이용하여 HDInsight 클러스터에 대해 metastore를 설정할 수 있습니다.

* [기본 metastore](#default-metastore)
* [사용자 지정 metastore](#custom-metastore)

## <a name="default-metastore"></a>기본 metastore

기본적으로 HDInsight는 모든 클러스터 유형과 함께 metastore를 만듭니다. 사용자 지정 metastore를 지정할 수 있습니다. 기본 metastore에 대한 고려 사항은 다음과 같습니다.

* 추가 비용이 없습니다. HDInsight는 모든 클러스터 유형과 함께 metastore를 만들며, 추가 비용은 없습니다.

* 각 기본 metastore는 클러스터 수명 주기의 일부입니다. 클러스터를 삭제하면 해당하는 metastore와 메타데이터도 삭제됩니다.

* 기본 metastore을 다른 클러스터와 공유할 수 없습니다.

* 기본 metastore 간단한 워크 로드에만 권장 됩니다. 여러 클러스터가 필요 하지 않으며 클러스터의 수명 주기 이상으로 유지 되는 메타 데이터가 필요 하지 않은 워크 로드입니다.

> [!IMPORTANT]
> 기본 metastore **기본 계층 5 DTU 제한 (업그레이드할 수 없음)** 을 사용 하는 Azure SQL Database를 제공 합니다. 기본 테스트 목적으로 적합 합니다. 대량 또는 프로덕션 워크 로드의 경우 외부 metastore 마이그레이션하는 것이 좋습니다.

## <a name="custom-metastore"></a>사용자 지정 metastore

HDInsight는 프로덕션 클러스터에 권장되는 사용자 지정 metastore도 지원합니다.

* 사용자 고유의 Azure SQL Database를 metastore로 지정합니다.

* Metastore의 수명 주기는 클러스터 수명 주기에 연결 되지 않으므로 메타 데이터 손실 없이 클러스터를 만들고 삭제할 수 있습니다. HDInsight 클러스터를 삭제하고 다시 만든 후에도 Hive 스키마와 같은 메타데이터가 유지됩니다.

* 사용자 지정 metastore를 사용하면 해당 metastore에 여러 클러스터 및 클러스터 유형을 첨부할 수 있습니다. 예를 들어 HDInsight의 여러 대화형 쿼리, Hive 및 Spark 클러스터에서 단일 metastore를 공유할 수 있습니다.

* 선택한 성능 수준에 따라 metastore (Azure SQL Database) 비용에 대 한 비용을 지불 합니다.

* 필요에 따라 metastore를 확장할 수 있습니다.

* 클러스터와 외부 metastore는 동일한 지역에 호스팅해야 합니다.

![HDInsight Hive 메타데이터 저장소 사용 사례](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

### <a name="create-and-config-azure-sql-database-for-the-custom-metastore"></a>사용자 지정 metastore 만들기 및 구성 Azure SQL Database

HDInsight 클러스터에 대 한 사용자 지정 Hive metastore를 설정 하기 전에 기존 Azure SQL Database을 만들거나 만듭니다.  자세한 내용은 [빠른 시작: Azure SQL Database에서 단일 데이터베이스 만들기](../azure-sql/database/single-database-create-quickstart.md?tabs=azure-portal)를 참조 하세요.

클러스터를 만드는 동안 HDInsight 서비스는 외부 metastore에 연결 하 여 자격 증명을 확인 해야 합니다. Azure 서비스 및 리소스에서 서버에 액세스할 수 있도록 Azure SQL Database 방화벽 규칙을 구성 합니다. **서버 방화벽 설정** 을 선택 하 여 Azure Portal에서이 옵션을 사용 하도록 설정 합니다. 그런 다음 **공용 네트워크 액세스 거부** 아래에서 **아니요** 를 **선택 하 고,** **Azure 서비스 및 리소스에서** Azure SQL Database에 대 한이 서버에 액세스 하도록 허용 합니다 .를 선택 합니다. 자세한 내용은 [IP 방화벽 규칙 만들기 및 관리](../azure-sql/database/firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules) 를 참조 하세요.

SQL 저장소에 대 한 개인 끝점은 resourceproviderconnection을 사용 하 여 만든 클러스터 에서만 지원 됩니다 `outbound` . 자세히 알아보려면이 [documentationa](./hdinsight-private-link.md)를 참조 하세요.

![서버 방화벽 설정 단추](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![azure 서비스 액세스 허용](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>클러스터를 만드는 동안 사용자 지정 metastore 선택

언제 든 지 클러스터에서 이전에 만든 Azure SQL Database를 가리킬 수 있습니다. 포털을 통해 클러스터를 만들 때 옵션은 **저장소 > Metastore 설정** 에서 지정 합니다.

![HDInsight Hive 메타데이터 저장소 Azure Portal](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-guidelines"></a>Hive metastore 지침

> [!NOTE]
> 컴퓨팅 리소스(실행 중인 클러스터)와 metastore에 저장된 메타데이터를 구분할 수 있도록 가능하면 항상 사용자 지정 metastore를 사용합니다. 50 DTU 및 250 GB의 저장소를 제공 하는 S2 계층으로 시작 합니다. 병목 상태가 발생하는 경우 데이터베이스를 확장할 수 있습니다.

* 여러 HDInsight 클러스터가 별개의 데이터에 액세스하도록 하려는 경우에는 각 클러스터의 metastore용으로 별도의 데이터베이스를 사용합니다. 여러 HDInsight 클러스터에서 metastore 하나를 공유하는 경우 클러스터는 같은 메타데이터 및 기본 사용자 데이터 파일을 사용합니다.

* 사용자 지정 metastore를 정기적으로 백업합니다. Azure SQL Database는 백업을 자동으로 생성하지만 백업 보존 기간은 각기 다릅니다. 자세한 내용은 [자동 SQL 데이터베이스 백업에 대해 알아보기](../azure-sql/database/automated-backups-overview.md)를 참조하세요.

* 동일한 지역에서 metastore 및 HDInsight 클러스터를 찾습니다. 이 구성은 최고 성능과 가장 낮은 네트워크 송신 요금을 제공 합니다.

* Azure SQL Database 모니터링 도구 또는 Azure Monitor 로그를 사용 하 여 성능 및 가용성에 대 한 metastore를 모니터링 합니다.

* 기존 사용자 지정 metastore 데이터베이스에 대 한 새 버전의 Azure HDInsight를 만든 경우 시스템에서 metastore의 스키마를 업그레이드 합니다. 백업에서 데이터베이스를 복원 하지 않고 업그레이드를 취소할 수 있습니다.

* 여러 클러스터 간에 metastore 하나를 공유하는 경우 모든 클러스터의 HDInsight 버전이 같은지 확인해야 합니다. 각 Hive 버전이 사용하는 metastore 데이터베이스 스키마는 서로 다릅니다. 예를 들어 Hive 2.1 및 Hive 3.1 버전이 지정 된 클러스터에서 metastore를 공유할 수 없습니다.

* HDInsight 4.0에서 Spark 및 Hive는 SparkSQL 또는 Hive 테이블에 액세스하기 위해 독립적인 카탈로그를 사용합니다. Spark에서 만든 테이블은 Spark 카탈로그에 있습니다. Hive에서 만든 테이블은 Hive 카탈로그에 있습니다. 이 동작은 Hive 및 Spark 공유 공통 카탈로그가 있는 HDInsight 3.6과는 다릅니다. HDInsight 4.0의 Hive 및 Spark 통합은 HWC(Hive Warehouse Connector)에 의존합니다. HWC는 Spark와 Hive 간에 브리지 역할을 합니다. [Hive 웨어하우스 커넥터에 대해 알아봅니다](../hdinsight/interactive-query/apache-hive-warehouse-connector.md).

* HDInsight 4.0에서 Hive와 Spark 간에 metastore을 공유하려는 경우 Spark 클러스터에서 metastore.catalog.default 속성을 hive로 변경하면 됩니다. Ambari 고급 spark2-hive-site-override에서 이 속성을 찾을 수 있습니다. metastore의 공유는 외부 hive 테이블에서만 작동합니다. 내부/관리형 hive 테이블 또는 ACID 테이블이 있는 경우에는 이 공유가 작동하지 않습니다.  

## <a name="apache-oozie-metastore"></a>Apache Oozie metastore

Apache Oozie는 Hadoop 작업을 관리하는 워크플로 조정 시스템입니다. Oozie는 Apache MapReduce, Pig, Hive 등에 대한 Hadoop 작업을 지원합니다.  Oozie는 metastore를 사용 하 여 워크플로에 대 한 세부 정보를 저장 합니다. Oozie 사용 시 성능을 높이기 위해 Azure SQL Database를 사용자 지정 metastore로 사용할 수 있습니다. Metastore은 클러스터를 삭제 한 후 Oozie 작업 데이터에 대 한 액세스를 제공 합니다.

Azure SQL Database를 사용하여 Oozie metastore를 만드는 방법에 대한 지침은 [워크플로에 대해 Apache Oozie 사용](hdinsight-use-oozie-linux-mac.md)을 참조하세요.

## <a name="custom-ambari-db"></a>사용자 지정 Ambari DB

HDInsight의 Apache Ambari에서 고유한 외부 데이터베이스를 사용 하려면 [사용자 지정 Apache Ambari 데이터베이스](hdinsight-custom-ambari-db.md)를 참조 하세요.

## <a name="next-steps"></a>다음 단계

* [Apache Hadoop, Apache Spark, Apache Kafka 등을 사용하여 HDInsight에서 클러스터 설정](./hdinsight-hadoop-provision-linux-clusters.md)