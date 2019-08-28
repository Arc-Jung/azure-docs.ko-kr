---
title: Apache Hadoop과 Apache Sqoop - Azure HDInsight
description: Apache Sqoop을 사용하여 HDInsight의 Hadoop과 Azure SQL Database 간에 가져오기 및 내보내기를 수행하는 방법을 알아봅니다.
keywords: hadoop sqoop,sqoop
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: c839aeae77d7e75fb30d82c410c331d21f5868ae
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406035"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop을 사용하여 HDInsight의 Hadoop과 SQL Database 간에 데이터 가져오기 및 내보내기

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Apache Sqoop을 사용하여 Azure HDInsight의 Hadoop 클러스터와 Azure SQL Database 또는 Microsoft SQL Server 데이터베이스 사이에서 가져오기 및 내보내기를 수행하는 방법을 알아봅니다. 이 문서의 단계에서는 Hadoop 클러스터의 헤드에서 직접 `sqoop` 명령을 사용합니다. SSH를 사용하여 헤드 노드에 연결하고 이 문서의 명령을 실행합니다. 이 문서는 [HDInsight에서 Hadoop과 함께 Apache Sqoop 사용](./hdinsight-use-sqoop.md)의 연속입니다.

## <a name="prerequisites"></a>필수 구성 요소

* [HDInsight에서 Hadoop과 함께 Apache Sqoop 사용](./hdinsight-use-sqoop.md)에서 [테스트 환경 설정](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) 완료

* Azure SQL database를 쿼리 하는 클라이언트입니다. [SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) 또는 [Visual Studio Code](../../sql-database/sql-database-connect-query-vscode.md)를 사용 하는 것이 좋습니다.

* SSH 클라이언트. 자세한 내용은 [SSH를 사용하여 HDInsight(Apache Hadoop)에 연결](../hdinsight-hadoop-linux-use-ssh-unix.md)을 참조하세요.

## <a name="sqoop-export"></a>Sqoop 내보내기

Hive에서 SQL Server 합니다.

1. SSH를 사용하여 HDInsight 클러스터에 연결합니다. 을 `CLUSTERNAME` 클러스터의 이름으로 바꾸고 다음 명령을 입력 합니다.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. 을 `MYSQLSERVER` SQL Server 이름으로 바꿉니다. Sqoop가 SQL Database를 볼 수 있는지 확인 하려면 열려 있는 SSH 연결에서 아래 명령을 입력 합니다. 메시지가 표시 되 면 SQL Server 로그인에 대 한 암호를 입력 합니다. 이 명령은 데이터베이스 목록을 반환 합니다.

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433 --username sqluser -P
    ```

3. 을 `MYSQLSERVER` SQLServer`MYDATABASE` 이름으로 바꾸고을 SQL 데이터베이스의 이름으로 바꿉니다. Hive `hivesampletable` 테이블`mobiledata` 의 데이터를 SQL Database의 테이블로 내보내려면 open SSH 연결에 아래 명령을 입력 합니다. 메시지가 표시 되 면 SQL Server 로그인에 대 한 암호를 입력 합니다.

    ```bash
    sqoop export --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P -table 'mobiledata' --hcatalog-table hivesampletable
    ```

4. 데이터를 내보낼지 확인 하려면 SQL 클라이언트에서 다음 쿼리를 사용 하 여 내보낸 데이터를 확인 합니다.

    ```sql
    SELECT COUNT(*) FROM [dbo].[mobiledata] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[mobiledata] WITH (NOLOCK);
    ```

## <a name="sqoop-import"></a>Sqoop 가져오기

SQL Server에서 Azure storage로.

1. 을 `MYSQLSERVER` SQLServer`MYDATABASE` 이름으로 바꾸고을 SQL 데이터베이스의 이름으로 바꿉니다. `mobiledata` SQLDatabase`wasb:///tutorials/usesqoop/importeddata` 에서 테이블의 데이터를 HDInsight의 디렉터리로 가져오려면 SSH 연결 열기에 아래 명령을 입력 합니다. 메시지가 표시 되 면 SQL Server 로그인에 대 한 암호를 입력 합니다. 데이터의 필드는 탭 문자로 구분되어 있으며 줄은 줄 바꿈 문자로 종료됩니다.

    ```bash
    sqoop import --connect 'jdbc:sqlserver://MYSQLSERVER.database.windows.net:1433;database=MYDATABASE' --username sqluser -P --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

2. 가져오기가 완료 되 면 열려 있는 SSH 연결에서 다음 명령을 입력 하 여 새 디렉터리의 데이터를 나열 합니다.

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="limitations"></a>제한 사항

* 대량 내보내기 - Linux 기반 HDInsight와 함께 Microsoft SQL Server 또는 Azure SQL Database에 데이터를 내보내는 데 사용된 Sqoop 커넥터도 대량 삽입을 지원하지 않습니다.

* Batch - Linux 기반 HDInsight에서 삽입을 수행할 때 `-batch` 스위치를 사용하는 경우 Sqoop은 삽입 작업을 일괄 처리하는 대신 여러 번의 삽입 작업을 수행합니다.

## <a name="important-considerations"></a>중요 고려 사항

* HDInsight와 SQL Server가 모두 동일한 Azure Virtual Network에 있어야 합니다.

    예제를 보려면 [온-프레미스 네트워크에 HDInsight 연결](./../connect-on-premises-network.md) 문서를 참조하세요.

    HDInsight와 함께 Azure Virtual Network를 사용하는 방법에 대한 자세한 내용은 [Azure Virtual Network를 사용하여 HDInsight 확장](../hdinsight-plan-virtual-network-deployment.md) 문서를 참조하세요. Azure Virtual Network에 대한 자세한 내용은 [가상 네트워크 개요](../../virtual-network/virtual-networks-overview.md) 문서를 참조하세요.

* SQL Server는 SQL 인증을 허용하도록 구성되어야 합니다. 자세한 내용은 [인증 모드 선택](https://msdn.microsoft.com/ms144284.aspx) 문서를 참조하세요.

* SQL Server를 원격 연결을 허용하도록 구성해야 할 수 있습니다. 자세한 내용은 [SQL Server 데이터베이스 엔진에 연결하는 문제를 해결하는 방법](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)(영문)을 참조하십시오.

## <a name="next-steps"></a>다음 단계

이제 Sqoop을 사용하는 방법에 대해 알아봤습니다. 자세한 내용은 다음을 참조하세요.

* [HDInsight에서 Apache Oozie 사용](../hdinsight-use-oozie-linux-mac.md): Oozie 워크플로에서 Sqoop 작업을 사용합니다.
* [HDInsight를 사용하여 비행 지연 데이터 분석](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): 대화형 쿼리를 사용 하 여 비행 지연 데이터를 분석 한 다음 Sqoop을 사용 하 여 Azure SQL database로 데이터를 내보냅니다.
* [HDInsight에 데이터 업로드](../hdinsight-upload-data.md): HDInsight/Azure Blob Storage에 데이터를 업로드하는 다른 방법을 찾습니다.
