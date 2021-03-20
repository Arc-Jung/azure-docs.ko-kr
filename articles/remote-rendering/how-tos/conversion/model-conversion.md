---
title: 모델 변환
description: 렌더링을 위해 모델을 변환 하는 프로세스를 설명 합니다.
author: jakrams
ms.author: jakras
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: e899b249261ea3238695a2e2be6001cb6a9bc763
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91318061"
---
# <a name="convert-models"></a>모델 변환

Azure 원격 렌더링을 사용 하면 매우 복잡 한 모델을 렌더링할 수 있습니다. 성능을 최대화 하려면 데이터를 전처리 하 여 최적의 형식을 지정 해야 합니다. 데이터 양에 따라이 단계는 시간이 오래 걸릴 수 있습니다. 모델을 로드 하는 동안이 시간이 소요 되는 경우에는 실용적이 지 않습니다. 또한 여러 세션에 대해이 프로세스를 반복 하는 것은 불필요 합니다. 이러한 이유로 ARR 서비스는 미리 실행할 수 있는 전용 *변환 서비스* 를 제공 합니다.
변환 되 면 Azure Storage 계정에서 모델을 로드할 수 있습니다.

## <a name="supported-source-formats"></a>지원 되는 원본 형식

변환 서비스는 다음과 같은 형식을 지원 합니다.

- **FBX**  (버전 2011 ~ 2020)
- **글 tf** / **글** 2 (버전 2.x)

[모델 형식의 재질 매핑](../../reference/material-mapping.md)에 나열 된 것 처럼 재질 속성 변환과 관련 된 형식 간에는 약간의 차이가 있습니다.

## <a name="the-conversion-process"></a>변환 프로세스

1. [두 개의 Azure Blob Storage 컨테이너 준비](blob-storage.md): 입력에 대해 하나, 하나는 출력용입니다.
1. 모델을 입력 컨테이너에 업로드 합니다 (선택적으로 하위 경로 아래에 있습니다).
1. [모델 변환을](conversion-rest-api.md) 통해 변환 프로세스를 트리거합니다 REST API
1. 변환 진행률에 대해 서비스를 폴링합니다.
1. 완료 되 면 모델을 로드 합니다.
    - 연결 된 저장소 계정에서 (저장소 계정을 연결 하는 [계정 만들기](../create-an-account.md#link-storage-accounts) 의 "링크 저장소 계정" 단계 참조)
    - 또는 *SAS (공유 액세스 서명)* 를 제공 합니다.

모든 모델 데이터 (입력 및 출력)는 사용자가 제공 하는 Azure blob 저장소에 저장 됩니다. Azure 원격 렌더링은 자산 관리를 완벽 하 게 제어할 수 있도록 합니다.

## <a name="pricing"></a>가격

변환 가격 책정에 대 한 자세한 내용은 [원격 렌더링 가격 책정](https://azure.microsoft.com/pricing/details/remote-rendering) 페이지를 참조 하세요.


## <a name="conversion-parameters"></a>변환 매개 변수

다양 한 변환 옵션에 대 한 자세한 내용은 [이 챕터](configure-model-conversion.md)를 참조 하십시오.

## <a name="examples"></a>예제

- [빠른 시작: 렌더링을 위해 모델 변환](../../quickstarts/convert-model.md) 은 모델을 변환 하는 방법에 대 한 단계별 소개입니다.
- 변환 서비스의 사용을 보여 주는 [예제 PowerShell 스크립트](../../samples/powershell-example-scripts.md)는 *스크립트* 폴더의 [ARR 샘플 리포지토리](https://github.com/Azure/azure-remote-rendering) 에서 찾을 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [모델 변환에 Azure Blob Storage 사용](blob-storage.md)
- [모델 변환 REST API](conversion-rest-api.md)
- [모델 변환 구성](configure-model-conversion.md)
- [변환용 파일 레이아웃](layout-files-for-conversion.md)
- [모델 형식에 대한 재질 매핑](../../reference/material-mapping.md)
