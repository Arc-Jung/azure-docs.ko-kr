---
title: 영역 & 끝점 게시-LUIS
description: Azure Portal에 지정 된 지역은 LUIS 앱을 게시할 때와 동일한 지역에 대해 끝점 URL이 생성 됩니다.
ms.service: cognitive-services
ms.subservice: language-understanding
author: aahill
ms.author: aahi
ms.topic: reference
ms.date: 01/21/2021
ms.custom: references_regions
ms.openlocfilehash: 8b43fc472f3247a93414a0b18d9098c6dfb94917
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98681610"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>작성 및 게시 지역과 관련 키

LUIS 제작 지역은 LUIS 포털에서 지원 됩니다. LUIS 앱을 둘 이상의 지역에 게시하려면 지역당 하나 이상의 키가 필요합니다.

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUIS 제작 지역

[!INCLUDE [portal consolidation](includes/portal-consolidation.md)]

LUIS에는 지역, [www.luis.ai](https://www.luis.ai)에 관계 없이 사용할 수 있는 포털이 하나 있습니다. 동일한 지역에서 아직 제작 하 고 게시 해야 합니다.

제작 지역에 [페어링된 장애 조치 (failover) 지역이](../../best-practices-availability-paired-regions.md) 있습니다.

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>지역 및 Azure 리소스 게시

앱은 LUIS 포털에 추가된 LUIS 리소스와 관련된 모든 지역에 게시됩니다. 예를 들어 [www.luis.ai][www.luis.ai]에서 만든 앱의 경우 **westus** 에서 luis 또는 인식 서비스 리소스를 만들고 [리소스로 앱에 추가](luis-how-to-azure-subscription.md)하는 경우 해당 지역에 앱이 게시 됩니다.

## <a name="public-apps"></a>공개 앱
공용 앱은 모든 지역에 게시되므로 지역 기반 LUIS 리소스 키를 가진 사용자가 해당 리소스 키와 연결된 지역에서 앱에 액세스할 수 있습니다.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>게시 지역은 지역 제작에 연결 됩니다.

작성 지역 앱은 해당 게시 지역에만 게시할 수 있습니다. 현재 앱이 잘못된 작성 지역에 있는 경우 앱을 내보내고 게시 지역의 올바른 작성 지역으로 가져옵니다.

> [!NOTE]
> 이제에서 만든 LUIS apps https://www.luis.ai 를 [유럽](#publishing-to-europe) 및 [오스트레일리아](#publishing-to-australia) 지역을 포함 하는 모든 끝점에 게시할 수 있습니다.

## <a name="publishing-to-europe"></a>유럽에 게시

 글로벌 지역 | API 영역 제작 | 게시 및 쿼리 지역<br>`API region name`   |  엔드포인트 URL 형식   |
|-----|------|------|------|
| 유럽 | `westeurope`| 프랑스 중부<br>`francecentral`     | `https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| 유럽 | `westeurope`| 북유럽<br>`northeurope`     | `https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| 유럽 | `westeurope`| 서유럽<br>`westeurope`    |  `https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| 유럽 | `westeurope`| 영국 남부<br>`uksouth`    |  `https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-australia"></a>오스트레일리아에 게시

 글로벌 지역 | API 영역 제작 | 게시 및 쿼리 지역<br>`API region name`   |  엔드포인트 URL 형식   |
|-----|------|------|------|
| 오스트레일리아 | `australiaeast` | 오스트레일리아 동부<br>`australiaeast`     |  `https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="other-publishing-regions"></a>기타 게시 영역

 글로벌 지역 | API 영역 제작 | 게시 및 쿼리 지역<br>`API region name`   |  엔드포인트 URL 형식   |
|-----|------|------|------|
| 아프리카 | `westus`<br>[www.luis.ai][www.luis.ai]| 남아프리카 북부<br>`southafricanorth` |  `https://southafricanorth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 인도 중부<br>`centralindia` |  `https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 동아시아<br>`eastasia`     |  `https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 일본 동부<br>`japaneast`     |   `https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 일본 서부<br>`japanwest`     |   `https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 한국 중부<br>`koreacentral`     |   `https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 동남 아시아<br>`southeastasia`     |   `https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 아시아 | `westus`<br>[www.luis.ai][www.luis.ai]| 북부 아랍에미리트<br>`northuae`     |   `https://northuae.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 |`westus`<br>[www.luis.ai][www.luis.ai] | 캐나다 중부<br>`canadacentral`     |   `https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 |`westus`<br>[www.luis.ai][www.luis.ai] | 미국 중부<br>`centralus`     |   `https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 |`westus`<br>[www.luis.ai][www.luis.ai] | 미국 동부<br>`eastus`      |  `https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 | `westus`<br>[www.luis.ai][www.luis.ai] | 미국 동부 2<br>`eastus2`     |  `https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 | `westus`<br>[www.luis.ai][www.luis.ai] | 미국 중북부<br>`northcentralus`  |  `https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 | `westus`<br>[www.luis.ai][www.luis.ai] | 미국 중남부<br>`southcentralus`  |  `https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 |`westus`<br>[www.luis.ai][www.luis.ai] | 미국 중서부<br>`westcentralus`    |  `https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 | `westus`<br>[www.luis.ai][www.luis.ai] | 미국 서부<br>`westus`  |   `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 북아메리카 |`westus`<br>[www.luis.ai][www.luis.ai] | 미국 서부 2<br>`westus2`    |  `https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |
| 남아메리카 | `westus`<br>[www.luis.ai][www.luis.ai] | 브라질 남부<br>`brazilsouth`    |  `https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY` |

## <a name="endpoints"></a>엔드포인트

[제작 및 예측 끝점](developer-reference-resource.md)에 대해 자세히 알아보세요.

## <a name="failover-regions"></a>장애 조치(failover) 지역

각 지역에는 장애 조치할 보조 지역이 있습니다. 유럽 및 오스트레일리아 내에서 유럽 장애 조치 (failover)는 오스트레일리아 내부에서 장애 조치 합니다.

제작 지역에는 [페어링된 장애 조치 지역이](../../best-practices-availability-paired-regions.md)있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [미리 빌드된 엔터티 참조](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai