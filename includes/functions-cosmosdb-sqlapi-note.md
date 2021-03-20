---
title: 파일 포함
description: 포함 파일
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/22/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f7495c52e36bdc207145f17ec72eb57b7ebf1784
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "76279619"
---
Azure Cosmos DB 바인딩은 SQL API에서만 사용할 수 있습니다. 다른 모든 Azure Cosmos DB API의 경우 [Azure Cosmos DB의 MongoDB용 API](../articles/cosmos-db/mongodb-introduction.md), [Cassandra API](../articles/cosmos-db/cassandra-introduction.md), [Gremlin API](../articles/cosmos-db/graph-introduction.md) 및 [Table API](../articles/cosmos-db/table-introduction.md)를 비롯한 API에 정적 클라이언트를 사용하여 함수에서 데이터베이스에 액세스해야 합니다.
