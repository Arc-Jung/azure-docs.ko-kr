---
title: Synapse Studio에서 Apache Spark 응용 프로그램을 모니터링 하는 방법
description: Synapse Studio를 사용 하 여 Apache Spark 응용 프로그램을 모니터링 하는 방법을 알아봅니다.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 11/30/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 5f9733866e85d79bdb85b8a24d1878e1169c2479
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97586043"
---
# <a name="how-to-use-synapse-studio-to-monitor-your-apache-spark-applications"></a>Synapse Studio를 사용 하 여 Apache Spark 응용 프로그램을 모니터링 하는 방법

Azure Synapse Analytics를 사용 하면 Spark 풀의 작업 영역에서 Spark를 사용 하 여 노트북, 작업 및 기타 종류의 응용 프로그램을 실행할 수 있습니다.

이 문서에서는 Apache Spark 응용 프로그램을 모니터링 하 여 최신 상태, 문제 및 진행 상황을 파악할 수 있도록 하는 방법을 설명 합니다.

## <a name="access-apache-spark-applications-list"></a>액세스 Apache Spark 응용 프로그램 목록

작업 영역에서 Apache Spark 응용 프로그램 목록을 보려면 먼저 [Synapse Studio를 열고](https://web.azuresynapse.net/) 작업 영역을 선택 합니다.

![작업 영역에 로그인](./media/common/login-workspace.png)

작업 영역을 연 후 왼쪽의 **모니터** 섹션을 선택 합니다.

![모니터 허브 선택](./media/common/left-nav.png)

응용 프로그램 **Apache Spark** 를 선택 하 여 Apache Spark 응용 프로그램 목록을 확인 합니다.

 ![Spark 응용 프로그램 선택](./media/how-to-monitor-spark-applications/monitor-hub-nav-spark-applications.png)

## <a name="filter-your-apache-spark-applications"></a>Apache Spark 응용 프로그램 필터링

원하는 응용 프로그램에 대 한 Apache Spark 응용 프로그램 목록을 필터링 할 수 있습니다. 화면 위쪽의 필터를 사용 하 여 필터링 할 필드를 지정할 수 있습니다.

예를 들어 뷰를 필터링 하 여 이름이 "sales" 인 Apache Spark 응용 프로그램만 볼 수 있습니다.

![샘플 필터](./media/how-to-monitor-spark-applications/filter-example.png)

## <a name="view-details-about-a-specific-apache-spark-application"></a>특정 Apache Spark 응용 프로그램에 대 한 세부 정보 보기

Apache Spark 응용 프로그램 중 하나에 대 한 세부 정보를 보려면 Apache Spark 응용 프로그램을 선택 하 고 세부 정보를 확인 합니다. Apache Spark 응용 프로그램이 계속 실행 중인 경우 진행률을 모니터링할 수 있습니다. [자세히 알아봅니다](apache-spark-applications.md).

## <a name="next-steps"></a>다음 단계

파이프라인 실행을 모니터링 하는 방법에 대 한 자세한 내용은 [Monitor 파이프라인 실행 Synapse Studio](how-to-monitor-pipeline-runs.md) 문서를 참조 하세요. 

Apache Spark 응용 프로그램 디버깅에 대 한 자세한 내용은 [Synapse Studio에서 Apache Spark 응용 프로그램 모니터링](apache-spark-applications.md) 문서를 참조 하세요.