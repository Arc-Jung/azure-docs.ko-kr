---
title: Blob 저장소에 대 한 요청에 암호화 키 제공
titleSuffix: Azure Storage
description: Azure Blob 저장소에 대 한 요청을 수행 하는 클라이언트에는 요청 별로 암호화 키를 제공 하는 옵션이 있습니다. 요청에 암호화 키를 포함 하면 Blob 저장소 작업의 암호화 설정에 대 한 세부적인 제어 기능을 제공 합니다.
services: storage
author: tamram
ms.service: storage
ms.date: 12/14/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: fcc5c02c4a37e205622470260d3c620ad76d07d8
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97694702"
---
# <a name="provide-an-encryption-key-on-a-request-to-blob-storage"></a>Blob 저장소에 대 한 요청에 암호화 키 제공

Azure Blob 저장소에 대 한 요청을 수행 하는 클라이언트는 요청 별로 AES-256 암호화 키를 제공 하는 옵션을 제공 합니다. 요청에 암호화 키를 포함 하면 Blob 저장소 작업의 암호화 설정에 대 한 세부적인 제어 기능을 제공 합니다. 고객이 제공한 키는 Azure Key Vault 또는 다른 키 저장소에 저장할 수 있습니다.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="encrypting-read-and-write-operations"></a>읽기 및 쓰기 작업 암호화

클라이언트 응용 프로그램에서 요청에 대 한 암호화 키를 제공 하는 경우 Azure Storage는 blob 데이터를 읽고 쓰는 동안 암호화 및 암호 해독을 투명 하 게 수행 합니다. Azure Storage는 blob 내용과 함께 암호화 키의 SHA-256 해시를 작성 합니다. 해시는 blob에 대 한 모든 후속 작업이 동일한 암호화 키를 사용 하는지 확인 하는 데 사용 됩니다.

Azure Storage는 클라이언트가 요청과 함께 보내는 암호화 키를 저장 하거나 관리 하지 않습니다. 암호화 또는 암호 해독 프로세스가 완료되는 즉시 키가 안전하게 삭제됩니다.

클라이언트가 요청에 대해 고객이 제공한 키를 사용 하 여 blob을 만들거나 업데이트 하는 경우 해당 blob에 대 한 후속 읽기 및 쓰기 요청에도 키가 제공 되어야 합니다. 이미 고객이 제공한 키로 암호화 된 blob에 대 한 요청에서 키를 제공 하지 않으면 요청이 실패 하 고 오류 코드 409 (충돌)이 발생 합니다.

클라이언트 응용 프로그램에서 요청에 대 한 암호화 키를 보내고 저장소 계정도 Microsoft 관리 키 또는 고객이 관리 하는 키를 사용 하 여 암호화 되는 경우 Azure Storage는 암호화 및 암호 해독을 위해 요청에 제공 된 키를 사용 합니다.

요청의 일부로 암호화 키를 보내려면 클라이언트는 HTTPS를 사용 하 여 Azure Storage에 대 한 보안 연결을 설정 해야 합니다.

각 blob 스냅숏에는 자체 암호화 키가 있을 수 있습니다.

## <a name="request-headers-for-specifying-customer-provided-keys"></a>고객이 제공한 키를 지정 하기 위한 요청 헤더

REST 호출의 경우 클라이언트는 다음 헤더를 사용 하 여 요청에 대 한 암호화 키 정보를 Blob 저장소에 안전 하 게 전달할 수 있습니다.

|요청 헤더 | Description |
|---------------|-------------|
|`x-ms-encryption-key` |쓰기 및 읽기 요청 모두에 필요 합니다. B A s e 64로 인코딩된 AES-256 암호화 키 값입니다. |
|`x-ms-encryption-key-sha256`| 쓰기 및 읽기 요청 모두에 필요 합니다. 암호화 키의 b a s e 64로 인코딩된 SHA256입니다. |
|`x-ms-encryption-algorithm` | 쓰기 요청에 필요 하며 읽기 요청의 경우 선택 사항입니다. 지정 된 키를 사용 하 여 데이터를 암호화할 때 사용할 알고리즘을 지정 합니다.  이 헤더의 값은 이어야 합니다 `AES256` . |

요청에 암호화 키를 지정 하는 것은 선택 사항입니다. 그러나 쓰기 작업을 위해 위에 나열 된 헤더 중 하나를 지정 하는 경우 모든 헤더를 지정 해야 합니다.

## <a name="blob-storage-operations-supporting-customer-provided-keys"></a>고객이 제공한 키를 지 원하는 Blob storage 작업

다음 Blob 저장소 작업은 요청에 고객이 제공한 암호화 키를 보내는 작업을 지원 합니다.

- [Blob 배치](/rest/api/storageservices/put-blob)
- [블록 목록 배치](/rest/api/storageservices/put-block-list)
- [블록 배치](/rest/api/storageservices/put-block)
- [URL에서 블록 배치](/rest/api/storageservices/put-block-from-url)
- [페이지 배치](/rest/api/storageservices/put-page)
- [URL에서 페이지 배치](/rest/api/storageservices/put-page-from-url)
- [추가 블록](/rest/api/storageservices/append-block)
- [Blob 속성 설정](/rest/api/storageservices/set-blob-properties)
- [Blob 메타데이터 설정](/rest/api/storageservices/set-blob-metadata)
- [Blob 가져오기](/rest/api/storageservices/get-blob)
- [Blob 속성 가져오기](/rest/api/storageservices/get-blob-properties)
- [Blob 메타데이터 가져오기](/rest/api/storageservices/get-blob-metadata)
- [Blob 스냅샷](/rest/api/storageservices/snapshot-blob)

## <a name="rotate-customer-provided-keys"></a>고객이 제공한 키 회전

Blob을 암호화 하는 데 사용 된 암호화 키를 회전 하려면 blob을 다운로드 한 다음 새 암호화 키를 사용 하 여 다시 업로드 합니다.

> [!IMPORTANT]
> 요청에 제공 된 키로 암호화 된 컨테이너 또는 blob에 대 한 읽기 또는 쓰기에 Azure Portal를 사용할 수 없습니다.
>
> Azure Key Vault와 같은 보안 키 저장소의 Blob 저장소에 대 한 요청에 제공 하는 암호화 키를 보호 해야 합니다. 암호화 키 없이 컨테이너 또는 blob에 대해 쓰기 작업을 시도 하면 작업이 실패 하 고 개체에 대 한 액세스 권한이 손실 됩니다.

## <a name="next-steps"></a>다음 단계

- [.NET을 사용 하 여 Blob 저장소에 대 한 요청에 고객이 제공한 키를 지정 합니다.](storage-blob-customer-provided-key.md)
- [미사용 데이터에 대한 Azure Storage 암호화](../common/storage-service-encryption.md)
