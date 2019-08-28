---
title: 로컬 파일에서 Azure Media Services 작업 입력 만들기 | Microsoft Docs
description: 이 항목에서는 로컬 파일에서 작업 입력을 만드는 방법을 보여줍니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/18/2019
ms.author: juliako
ms.openlocfilehash: 94ac332867d46deb426672fbc40cc72cc121fe64
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69998146"
---
# <a name="create-a-job-input-from-a-local-file"></a>로컬 파일에서 작업 입력 만들기

Media Services v3에서는, 비디오를 처리할 작업을 제출할 때 입력 비디오를 찾을 위치를 Media Services에 알려줘야 합니다. 입력 비디오는 Media Service 자산으로 저장할 수 있으며 이 경우 파일(로컬 또는 Azure Blob Storage에 저장됨)을 기반으로 입력 자산을 만듭니다. 이 항목에서는 로컬 파일에서 작업 입력을 만드는 방법을 보여줍니다. 전체 예제를 보려면 [GitHub 샘플](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs)을 참조하세요.

## <a name="net-sample"></a>.NET 샘플

다음 코드는 입력 자산을 만들어서 작업에 대한 입력으로 사용하는 방법을 보여줍니다. CreateInputAsset 함수는 다음 작업을 수행합니다.

* 자산 만들기
* [스토리지에 있는 자산 컨테이너](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container)에 대해 쓰기가 가능한 [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) 가져오기
* SAS URL을 사용하여 스토리지의 컨테이너에 파일 업로드

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

다음 코드 조각에서는 출력 자산이 아직 없는 경우 새로 만듭니다.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAssetAsync)]

다음 코드 조각은 인코딩 작업을 제출 합니다.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>작업 오류 코드

[오류 코드](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)를 참조하세요.

## <a name="next-steps"></a>다음 단계

[HTTP URL에서 작업 입력 만들기](job-input-from-http-how-to.md)
