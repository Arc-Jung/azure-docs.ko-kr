---
title: 포함 파일
description: 포함 파일
services: storage
author: twooley
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: twooley
ms.custom: include file
ms.openlocfilehash: 7098f23e9b5b6f56fbbe761335afc65375aea680
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509228"
---
**Azure Data Lake Storage Gen2** 는 전용 서비스 또는 스토리지 계정 유형이 아닙니다. 빅 데이터 분석을 전담하는 기능의 최신 릴리스입니다.  이러한 기능은 범용 v2 또는 BlockBlobStorage 스토리지 계정에서 사용할 수 있으며 계정의 **계층 구조 네임스페이스** 기능을 사용하도록 설정하여 가져올 수 있습니다. 스케일링 대상은 다음 문서를 참조하세요. 

- [Blob Storage의 스케일링 대상](../articles/storage/blobs/scalability-targets.md#scale-targets-for-blob-storage)
- [표준 스토리지 계정의 스케일링 대상](../articles/storage/common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#scale-targets-for-standard-storage-accounts)

**Azure Data Lake Storage Gen1** 은 전용 서비스입니다. 빅 데이터 분석 작업을 위한 엔터프라이즈급 하이퍼스케일 리포지토리입니다. Data Lake Storage Gen1을 사용하면 운영 및 예비 분석을 위해 단일 위치에서 원하는 크기, 유형 및 수집 속도의 데이터를 캡처할 수 있습니다. Data Lake Storage Gen1 계정에 저장할 수 있는 데이터 양에는 제한이 없습니다.

| **리소스** | **제한** | **설명** |
| --- | --- | --- |
| 구독당, 지역당, Data Lake Storage Gen1 계정의 최대 수 |10 | 이 한도 증가를 요청하려면 고객 지원팀에 문의하세요. |
| 파일 또는 폴더당 최대 액세스 ACL 수 |32 | 이것은 하드 한도입니다. 항목 수가 적은 액세스를 관리하려면 그룹을 사용하세요. |
| 파일 또는 폴더당 최대 기본 ACL 수 |32 | 이것은 하드 한도입니다. 항목 수가 적은 액세스를 관리하려면 그룹을 사용하세요. |