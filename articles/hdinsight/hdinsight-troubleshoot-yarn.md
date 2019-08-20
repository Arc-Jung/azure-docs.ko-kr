---
title: Azure HDInsight의 YARN 문제 해결
description: Apache Hadoop YARN 및 Azure HDInsight 작업에 대한 일반적인 질문에 답합니다.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: 8bfe249b0295bc860cf17a006c3787ff8afa676b
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69573715"
---
# <a name="troubleshoot-apache-hadoop-yarn-by-using-azure-hdinsight"></a>Azure HDInsight를 사용하여 Apache Hadoop YARN 문제를 해결합니다.

Apache Ambari에서 Apache Hadoop YARN 페이로드를 사용할 때의 주요 문제 및 해결 방법을 알아봅니다.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>클러스터에서 새 YARN 큐를 만들려면 어떻게 하나요?

### <a name="resolution-steps"></a>해결 단계:

Ambari에서 다음 단계를 사용하여 새 YARN 큐를 만들고 모든 큐에 용량이 균형 있게 할당되도록 조정합니다.

이 예제에서는 기존 큐 두 개(**default** 및 **thriftsvr**) 모두 새 큐(spark)를 50% 용량으로 하여 50% 용량에서 25% 용량으로 변경됩니다.

| 큐 | 용 | 최대 용량 |
| --- | --- | --- |
| 기본 | 25% | 50% |
| thrftsvr | 25% | 50% |
| spark | 50% | 50% |

1. **Ambari 뷰** 아이콘을 선택한 다음, 그리드 패턴을 선택합니다. 다음으로, **YARN 큐 관리자**를 선택합니다.

    ![Ambari 뷰 아이콘 선택](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. **default** 큐를 선택합니다.

    ![default 큐 선택](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. **default** 큐의 경우 **용량**을 50%에서 25%로 변경합니다. **thriftsvr** 큐의 경우 **용량**을 25%로 변경합니다.

    ![default 및 thriftsvr 큐에 대해 용량을 25%로 변경](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. 새 큐를 만들려면 **큐 추가**를 선택합니다.

    ![큐 추가 선택](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. 새 큐 이름을 지정합니다.

    ![큐 이름을 Spark로 지정](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. **용량** 값을 50%로 두고 **작업** 단추를 선택합니다.

    ![작업 단추 선택](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. **큐 저장 및 새로 고침**을 선택합니다.

    ![큐 저장 및 새로 고침 선택](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

이러한 변경 내용은 YARN Scheduler UI에 즉시 표시됩니다.

### <a name="additional-reading"></a>추가 참조 항목

- [Apache Hadoop YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)

## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>클러스터에서 YARN 로그를 다운로드하려면 어떻게 하나요?

### <a name="resolution-steps"></a>해결 단계: 

1. SSH(Secure Shell) 클라이언트를 사용하여 HDInsight 클러스터에 연결합니다. 자세한 내용은 [더 보기](#additional-reading-2)를 참조하세요.

1. 현재 실행 중인 YARN 애플리케이션의 모든 애플리케이션 ID를 나열하려면 다음 명령을 사용합니다.

    ```apache
    yarn top
    ```

    ID가 **APPLICATIONID** 열에 나열됩니다. **APPLICATIONID** 열에서 로그를 다운로드할 수 있습니다.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

1. 모든 애플리케이션 마스터에 대한 YARN 컨테이너 로그를 다운로드하려면 다음 명령을 사용합니다.

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    amlogs.txt라는 로그 파일이 만들어집니다.

1. 최신 애플리케이션 마스터에 대한 YARN 컨테이너 로그만 다운로드하려면 다음 명령을 사용합니다.

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    latestamlogs.txt라는 로그 파일이 만들어집니다.

1. 처음 두 개 애플리케이션 마스터에 대한 YARN 컨테이너 로그를 다운로드하려면 다음 명령을 사용합니다.

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt
    ```

    first2amlogs.txt라는 로그 파일이 만들어집니다.

1. 모든 YARN 컨테이너 로그를 다운로드하려면 다음 명령을 사용합니다.

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    logs.txt라는 로그 파일이 만들어집니다.

1. 특정 컨테이너에 대한 YARN 컨테이너 로그를 다운로드하려면 다음 명령을 사용합니다.

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt
    ```

    containerlogs.txt라는 로그 파일이 만들어집니다.

### <a name="additional-reading-2"></a>추가 참조 자료

- [SSH를 사용하여 HDInsight(Apache Hadoop)에 연결](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN 개념 및 애플리케이션](https://hadoop.apache.org/docs/r2.7.4/hadoop-yarn/hadoop-yarn-site/WritingYarnApplications.html#Concepts_and_Flow)

## <a name="next-steps"></a>다음 단계

문제가 표시되지 않거나 문제를 해결할 수 없는 경우 다음 채널 중 하나를 방문하여 추가 지원을 받으세요.

- Azure [커뮤니티 지원을](https://azure.microsoft.com/support/community/)통해 azure 전문가 로부터 답변을 받으세요.

- 을 사용 [@AzureSupport](https://twitter.com/azuresupport) 하 여 연결-고객 환경을 개선 하기 위한 공식 Microsoft Azure 계정입니다. Azure 커뮤니티를 적절 한 리소스 (답변, 지원 및 전문가)에 연결 합니다.

- 도움이 더 필요한 경우 [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)에서 지원 요청을 제출할 수 있습니다. 메뉴 모음에서 **지원** 을 선택 하거나 **도움말 + 지원** 허브를 엽니다. 자세한 내용은 [Azure 지원 요청을 만드는 방법](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)을 참조 하세요. 구독 관리 및 청구 지원에 대 한 액세스는 Microsoft Azure 구독에 포함 되며, [Azure 지원 계획](https://azure.microsoft.com/support/plans/)중 하나를 통해 기술 지원이 제공 됩니다.
