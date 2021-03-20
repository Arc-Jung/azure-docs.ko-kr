---
title: Azure Stream Analytics의 Azure Synapse Analytics 출력
description: 이 문서에서는 Azure Stream Analytics에 대 한 출력으로 Azure Synapse Analytics에 대해 설명 합니다.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 7e85df8ae67624a253a9fb617629d7355109c210
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98019604"
---
# <a name="azure-synapse-analytics-output-from-azure-stream-analytics"></a>Azure Stream Analytics의 Azure Synapse Analytics 출력

[Azure Synapse analytics](https://azure.microsoft.com/services/synapse-analytics) 는 엔터프라이즈 데이터 웨어하우징 및 빅 데이터 분석을 통합 하는 무제한 분석 서비스입니다. 

Azure Stream Analytics 작업은 Azure Synapse Analytics의 전용 SQL 풀 테이블로 출력 될 수 있으며 최대 200MB/초의 처리량 속도를 처리할 수 있습니다. 이는 보고 및 일점 보 딩과 같은 워크 로드에 대 한 가장 까다로운 실시간 분석 및 핫 경로 데이터 처리 요구를 지원 합니다.  

전용 SQL 풀 테이블이 있어야 Stream Analytics 작업에 출력으로 추가할 수 있습니다. 테이블 스키마는 작업 출력의 필드 및 해당 형식과 일치해야 합니다. 

Azure Synapse를 출력으로 사용하려면 스토리지 계정이 구성되어 있는지 확인해야 합니다. 스토리지 계정 설정으로 이동하여 스토리지 계정을 구성합니다. 테이블을 지원하는 스토리지 계정 유형(범용 V2 및 범용 V1)만 허용됩니다. 표준 계층만 선택합니다. 프리미엄 계층은 지원되지 않습니다.

## <a name="output-configuration"></a>출력 구성

다음 표에는 Azure Synapse Analytics 출력을 만들기 위한 속성 이름 및 해당 설명이 나와 있습니다.

|속성 이름|Description|
|-|-|
|출력 별칭 |쿼리 출력을 이 데이터베이스로 보내기 위해 쿼리에서 사용되는 식별 이름입니다. |
|데이터베이스 |출력을 전송 하는 전용 SQL 풀 이름입니다. |
|서버 이름 |Azure Synapse 서버 이름입니다.  |
|사용자 이름 |데이터베이스에 대한 쓰기 액세스 권한이 있는 사용자 이름입니다. Stream Analytics는 SQL 인증만 지원합니다. |
|암호 |데이터베이스에 연결하는 암호입니다. |
|테이블  | 출력을 기록되는 테이블 이름입니다. 테이블 이름은 대/소문자를 구분합니다. 이 테이블의 스키마는 작업 출력에서 생성하는 필드 수 및 해당 형식과 정확히 일치해야 합니다.|

## <a name="next-steps"></a>다음 단계

* [관리 되는 id를 사용 하 여 Azure Stream Analytics 작업에서 Azure SQL Database 또는 Azure Synapse Analytics에 액세스 (미리 보기)](sql-database-output-managed-identity.md)
* [빠른 시작: Azure Portal을 사용하여 Stream Analytics 작업 만들기](stream-analytics-quick-create-portal.md)