---
title: Azure 스토리지 환경으로 데이터 로드 - Team Data Science Process
description: 데이터가 저장 되 고 처리 되는 다양 한 대상 환경으로 데이터를 수집 하는 방법에 대해 알아봅니다.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 515decfafe46ad0c1b5b90743688abc26a975903
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053257"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>분석용 스토리지 환경에 데이터 로드

팀 데이터 과학 프로세스는 프로세스의 각 단계에서 가장 적절한 방법으로 처리되거나 분석되도록 데이터가 다양한 스토리지 환경에 수집되거나 로드되어야 합니다. 처리하기 위해 일반적으로 사용하는 데이터 대상에는 Azure Blob Storage, SQL Azure 데이터베이스, Azure VM의 SQL Server, HDInsight(Hadoop) 및 Azure Machine Learning이 포함됩니다. 

다음 문서에서는 데이터가 저장되고 처리되는 다양한 대상 환경으로 데이터를 수집하는 방법을 설명합니다.

* [Azure Blob Storage](move-azure-blob.md) 간 수집
* [Azure VM의 SQL Server](move-sql-server-virtual-machine.md)로 수집
* [Azure SQL 데이터베이스](move-sql-azure.md)로 수집
* [Hive 테이블](move-hive-tables.md)로 수집
* [SQL 분할된 테이블](parallel-load-sql-partitioned-tables.md)로 수집
* [온-프레미스 SQL Server](move-sql-azure-adf.md)에서 수집

데이터의 처음 위치, 형식 및 크기는 물론 기술 및 비즈니스 요구에 따라 분석의 목표를 달성하기 위해 데이터를 수집해야 하는 대상 환경이 결정됩니다. 예측 모델을 구성하는 데 필요한 다양한 작업을 구현하기 위해 데이터가 여러 환경 사이를 이동해야 하는 시나리오는 일반적이지 않습니다. 이 작업 시퀀스에는 데이터 탐색, 사전 처리, 정리, 다운 샘플링, 모델 학습 등이 포함될 수 있습니다.
