---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 5ab03b682dd0ed1dc7b198e89c86e7a74c6275cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67457443"
---
| Resource | 대상 |
|----------|---------------|
| 단일 테이블의 최대 크기 | 500TiB |
| 테이블 엔터티의 최대 크기 | 1MiB |
| 테이블 엔터티에서 속성의 최대 수 | 255 세 가지 시스템 속성이 포함 되어 있습니다. PartitionKey, RowKey 및 Timestamp |
| 엔터티의 속성 값의 최대 총 크기 | 1MiB |
| 엔터티의 개별 속성의 최대 총 크기 | 속성 유형에 따라 달라 집니다. 자세한 내용은 **속성 형식을** 에 [Table Service 데이터 모델 이해](/rest/api/storageservices/understanding-the-table-service-data-model)합니다. |
| 테이블 별로 저장 된 액세스 정책 최대 수 | 5 |
| 스토리지 계정당 최대 요청 속도 | 1-(1kib 엔터티 크기로 가정 초당 20,000 개 트랜잭션 |
| (1 (1kib 엔터티)는 단일 테이블 파티션의 목표 처리량 | 초당 최대 2,000 개 엔터티 |