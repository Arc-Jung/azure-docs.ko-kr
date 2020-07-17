---
title: Azure PowerShell 스크립트 샘플 - 사용자 지정 항목 구독 | Microsoft Docs
description: 이 문서에서는 사용자 지정 항목에 대한 Event Grid 이벤트를 구독하는 방법을 보여주는 샘플 Azure PowerShell 스크립트를 제공합니다.
ms.devlang: powershell
ms.topic: sample
ms.date: 07/08/2020
ms.openlocfilehash: 89917554da3d5b338ae17332ea8847d6313ca534
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171179"
---
# <a name="subscribe-to-events-for-a-custom-topic-with-powershell"></a>PowerShell을 사용하여 사용자 지정 항목에 대한 이벤트 구독

이 스크립트는 사용자 지정 항목에 대한 이벤트에 대한 Event Grid 구독을 만듭니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

미리 보기 샘플 스크립트에는 Event Grid 모듈이 필요합니다. 설치하려면 `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`를 실행합니다.

## <a name="sample-script---stable"></a>샘플 스크립트 - 안정적

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-custom-topic/subscribe-to-custom-topic.ps1 "Subscribe to custom topic")]

## <a name="sample-script---preview-module"></a>샘플 스크립트 - 미리 보기 모듈

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-custom-topic-preview/subscribe-to-custom-topic-preview.ps1 "Subscribe to custom topic")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 이벤트 구독을 만듭니다. 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Event Grid 구독을 만듭니다. |

## <a name="next-steps"></a>다음 단계

* 관리되는 애플리케이션에 대한 소개는 [Azure Managed Application 개요](../overview.md)를 참조하세요.
* PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/azure/get-started-azureps)를 참조하세요.
