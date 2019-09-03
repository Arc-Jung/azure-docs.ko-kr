---
title: Azure 큐 소개 - Azure Storage
description: Azure 큐 소개
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/07/2019
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 86bbff167a2653fd8d89b566b551c4c53dd3614e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70124560"
---
# <a name="what-are-azure-queues"></a>Azure 큐란?

Azure Queue Storage는 대량의 메시지를 저장하기 위한 서비스입니다. 전 세계 어디서나 인증된 호출을 통해 HTTP 또는 HTTPS를 사용하여 메시지에 액세스할 수 있습니다. 큐 메시지의 크기는 최대 64KB입니다. 큐는 스토리지 계정의 용량 제한에 도달할 때까지 수백만 개의 메시지를 포함할 수 있습니다. 큐는 비동기적으로 처리할 작업의 백로그를 만드는 데 일반적으로 사용됩니다.

## <a name="queue-service-concepts"></a>큐 서비스 개념

큐 서비스에는 다음 구성 요소가 포함됩니다.

![큐 개념](./media/storage-queues-introduction/queue1.png)

* **URL 형식:** 다음 URL 형식을 사용하여 큐에 주소를 지정할 수 있습니다.

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    다음 URL은 다이어그램에 있는 큐의 주소를 지정합니다.  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **스토리지 계정:** Azure Storage에 대한 모든 액세스는 Storage 계정을 통해 수행됩니다. Storage 계정 용량에 대한 자세한 내용은 [Azure Storage 확장성 및 성능 목표](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (영문)를 참조하십시오.

* **큐:** 큐에는 메시지 세트가 포함됩니다. 큐 이름은 **반드시** 모두 소문자여야 합니다. 큐의 명명에 대한 자세한 내용은 [큐 및 메타데이터 명명](https://msdn.microsoft.com/library/azure/dd179349.aspx)을 참조하세요.

* **메시지:** 최대 64KB인 임의 형식의 메시지입니다. 2017-07-29 미만 버전에서 허용되는 최대 TTL(Time to Live)은 7일입니다. 2017-07-29 이상 버전에서 허용되는 최대 TTL(Time to Live)은 모든 양수 또는 메시지가 만료되지 않는 -1입니다. 이 매개 변수를 생략하면 기본 TTL(Time to Live)은 7일입니다.

## <a name="next-steps"></a>다음 단계

* [스토리지 계정을 만드는](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [.NET을 사용하여 큐 시작하기](storage-dotnet-how-to-use-queues.md)
