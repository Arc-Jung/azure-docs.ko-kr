---
title: azcopy 작업 다시 시작 | Microsoft Docs
description: 이 문서에서는 azcopy jobs 다시 시작 명령에 대 한 참조 정보를 제공 합니다.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 30c0a31cc58ee6f1bbe78af017be42ae7d410fe0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98878412"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

지정 된 작업 ID를 사용 하 여 기존 작업을 다시 시작 합니다.

## <a name="synopsis"></a>개요

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>관련 개념 문서

- [AzCopy 시작](storage-use-azcopy-v10.md)
- [AzCopy 및 Blob 저장소를 사용 하 여 데이터 전송](./storage-use-azcopy-v10.md#transfer-data)
- [AzCopy 및 File Storage를 사용하여 데이터 전송](storage-use-azcopy-files.md)
- [AzCopy 구성, 최적화 및 문제 해결](storage-use-azcopy-configure.md)

## <a name="options"></a>옵션

|옵션|설명|
|--|--|
|--대상-sas 문자열|지정 된 작업 ID에 대 한 대상의 대상 SAS입니다.|
|--문자열 제외|필터: 작업을 다시 시작할 때 이러한 전송 실패를 제외 합니다. 파일은 '; '으로 구분 해야 합니다.|
|-h, --help|Resume 명령에 대 한 도움말 콘텐츠를 표시 합니다.|
|--문자열 포함|필터: 작업을 다시 시작할 때 이러한 실패 한 전송만 포함 합니다. 파일은 '; '으로 구분 해야 합니다.|
|--소스-sas 문자열 |지정 된 작업 ID의 원본 SAS입니다.|

## <a name="options-inherited-from-parent-commands"></a>부모 명령에서 상속 된 옵션

|옵션|설명|
|---|---|
|--0mbps float|전송 률 (메가 비트/초)을 대문자로 처리 합니다. 순간 처리량은 cap와 약간 다를 수 있습니다. 이 옵션을 0으로 설정 하거나 생략 하면 처리량이 생략 되지 않습니다.|
|--출력 형식 문자열|명령의 출력 형식입니다. 텍스트, json 등을 선택할 수 있습니다. 기본값은 "text"입니다.|
|--신뢰할 수 있는 microsoft 접미사 문자열   |Azure Active Directory 로그인 토큰이 전송 될 수 있는 추가 도메인 접미사를 지정 합니다.  기본값은 '*. core.windows.net;* 입니다. core.chinacloudapi.cn; *. core.cloudapi.de;*. core.usgovcloudapi.net '. 여기에 나열 된 Any는 기본값에 추가 됩니다. 보안을 위해 여기에 Microsoft Azure 도메인만 배치 해야 합니다. 여러 항목을 세미콜론으로 구분 합니다.|

## <a name="see-also"></a>참고 항목

- [azcopy jobs](storage-ref-azcopy-jobs.md)
