---
title: Bing Search API의 사용 및 표시 요구 사항
titleSuffix: Azure Cognitive Services
description: 애플리케이션에서 Bing Search API의 검색 결과를 표시하기 위한 요구 사항입니다.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: 93be72f2afcda90dde1b74c5ee317a7ad3350be1
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93075690"
---
# <a name="bing-search-api-use-and-display-requirements"></a>Bing Search API 사용 및 표시 요구 사항

> [!WARNING]
> Bing Search API Cognitive Services에서 Bing Search 서비스로 이동 합니다. **2020 년 10 월 30 일부 터** [여기](https://aka.ms/cogsvcs/bingmove)에 설명 된 프로세스에 따라 Bing Search의 새 인스턴스를 프로 비전 해야 합니다.
> Cognitive Services를 사용 하 여 프로 비전 된 Bing Search API는 향후 3 년 동안 또는 기업계약 종료 될 때까지 먼저 발생 합니다.
> 마이그레이션 지침은 [Bing Search Services](https://aka.ms/cogsvcs/bingmigration)를 참조 하십시오.

이러한 사용 및 표시 요구 사항은 관계, 메타데이터 및 기타 신호를 포함한 다음 Bing Search API의 콘텐츠 및 관련 정보 구현에 적용됩니다.

- Bing 사용자 지정 검색
- Bing Entity Search
- Bing 이미지 검색
- Bing 뉴스 검색
- Bing 비디오 검색
- Bing Visual Search
- Bing Web Search
- Bing 맞춤법 검사
- Bing Autosuggest

## <a name="definitions"></a>정의


|용어  |설명  |
|---------|---------|
|답변     | 응답에서 반환된 결과의 범주를 나타냅니다. 예를 들어 Bing Web Search API의 응답에는 웹 페이지 결과, 이미지, 비디오, 시각적 개체 및 뉴스 범주의 답변이 포함될 수 있습니다. |
|응답     | Search API에 대한 단일 호출에 대응하여 받은 모든 대답 및 관련 데이터를 나타냅니다. |
|결과    | 대답에 있는 정보 항목을 나타냅니다. 예를 들어 하나의 뉴스 기사와 연결된 일단의 데이터는 뉴스 답변의 결과입니다. |
|Search API    | Bing Custom Search, Entity Search, Image Search, News Search, Video Search, Visual Search, Local Business Search 및 Web Search API를 총체적으로 의미합니다. |

## <a name="bing-spell-check-and-bing-autosuggest-api-restrictions"></a>Bing Spell Check 및 Bing Autosuggest API 제한 사항

수행하지 않아야 할 작업:

- Bing Spell Check 또는 Bing Autosuggest API에서 받은 데이터를 복사, 저장 또는 캐시하지 않습니다.
- 기계 학습 또는 유사한 알고리즘 활동의 일부로 Bing Spell Check 또는 Bing Autosuggest API에서 받은 데이터를 사용하지 않습니다. 이 데이터를 사용자 또는 제3자가 제공할 수 있는 새 서비스 또는 기존 서비스를 학습하거나 평가하거나 개선하는 데 사용하지 않습니다.

## <a name="bing-search-apis"></a>Bing Search API

> [!NOTE]
> 이 섹션의 요구 사항은 Bing Spell Check 또는 Bing Autosuggest를 포함하지 않는 Search API에만 적용됩니다. 

### <a name="internet-search-experience-requirements"></a>인터넷 검색 환경 요구 사항

응답으로 반환된 모든 데이터는 인터넷 검색 환경에서만 사용할 수 있습니다. 인터넷 검색 환경은 다음과 같이 표시되는 콘텐츠를 의미합니다. 

- 최종 사용자의 직접 쿼리 또는 사용자의 검색 관심과 의도의 다른 표시(예: 사용자 지정 검색 쿼리)에 관련되고 반응합니다. 

- 사용자가 데이터 원본을 찾고 탐색할 수 있습니다. 예를 들어 응답의 하이퍼링크에서 클릭 가능한 링크를 제공하는 경우입니다.

- 사용자가 선택할 수 있는 여러 결과가 포함됩니다. 

- 사용자가 검색할 수 있도록 배치됩니다.

- 콘텐츠가 인터넷 검색 결과임을 알리는 시각적 표시를 포함합니다. 예를 들어 "웹에서"라는 내용이 있는 표현이 제공됩니다.

- Bing Search API 데이터가 관련 법률을 위반하거나 제3자의 권리를 침해하지 않도록 하기 위한 적절한 기타 조치를 포함합니다. 적절한 조치에 대해 법적 자문이 필요하면 법률 고문에게 문의합니다.

이러한 인터넷 검색 환경 요구 사항에 대한 유일한 예외는 이 문서 뒷부분에 설명된 URL 검색의 경우입니다. 

### <a name="restrictions"></a>제한

수행하지 않아야 할 작업:

- 응답의 데이터는 복사, 저장 또는 캐시하지 않습니다(예외: [서비스 연속성](#continuity-of-service)에서 허용하는 범위 내 보존). 

- 기계 학습 또는 유사한 알고리즘 활동의 일부로 Search API에서 받은 데이터를 사용하지 않습니다. 이 데이터를 사용자 또는 제3자가 제공할 수 있는 새 서비스 또는 기존 서비스를 학습하거나 평가하거나 개선하는 데 사용하지 않습니다.

- 법률이 요구하거나 Microsoft에서 동의하지 않은 경우, 결과의 내용은 수정하지 않습니다(예외: 다른 요구 사항을 위반하지 않는 방식으로 서식 재지정). 

- 결과의 내용과 관련된 특성 정보와 URL은 생략하지 않습니다.

- 법률이 요구하거나 Microsoft에서 동의하지 않은 경우, 순서 또는 순위를 제공할 때 대답에 표시된 결과의 순서를 변경하지 않습니다(누락 포함). 

    > [!NOTE]
    > 이 요구 사항은 Bing Custom Search API 포털을 통해 구현되는 순서 재지정에는 적용되지 않습니다.

- 사용자가 다른 내용을 응답의 일부로 믿도록 유도하는 방식으로 응답의 일부에 다른 내용을 표시하지 않습니다. 

- Microsoft에서 제공하지 않는 광고는 응답의 일부를 표시하는 페이지에 표시하지 않습니다. 

- 페이지에 다음에 의한 응답과 함께 광고를 표시합니다.
    - Bing Image, News Search, Video Search 또는 Visual Search API의 응답
    - 주로(또는 전적으로) 이미지, 뉴스 및/또는 비디오나 비디오 검색 결과로 필터링되거나 제한된 응답

### <a name="notices-and-branding"></a>공지 및 브랜드 
권장 사항:

- 사용자에게 검색 쿼리 입력 기능을 제공하는 UX(사용자 환경)의 각 위치 가까이에 [Microsoft 개인정보처리방침](https://go.microsoft.com/fwlink/?LinkId=521839)에 대한 기능형 하이퍼링크를 잘 보이게 포함합니다. 하이퍼링크 레이블을 **Microsoft 개인정보처리방침** 으로 표시합니다.

- 사용자에게 검색 쿼리 입력 기능을 제공하는 UX의 각 위치 가까이에 [Bing 상표 사용 지침](https://go.microsoft.com/fwlink/?linkid=833278)과 일관되는 Bing 브랜드를 잘 보이게 표시합니다. 이러한 브랜드는 Microsoft가 인터넷 검색 환경을 지원한다는 사실을 사용자에게 명확하게 알릴 수 있어야 합니다.

- Microsoft가 서면으로 다르게 지정하지 않는 한, Bing Web Search, Image Search, News Search, Video Search 및 Visual Search API에서 표시된 각 응답(또는 응답 부분)은 Microsoft에 귀속될 수 있습니다. 이 내용은 [Bing 상표 사용 지침](https://go.microsoft.com/fwlink/?linkid=833278)에 설명되어 있습니다. 

수행하지 않아야 할 작업:

- Microsoft가 특정 사용에 대해 서면으로 다르게 지정하지 않는 한, Bing Custom Search API에서 표시된 응답(또는 응답 부분)은 Microsoft에 귀속되지 않습니다.

### <a name="transferring-responses"></a>응답 전송

사용자가 메시지 앱 또는 소셜 미디어 게시와 같이 다른 사용자에게 검색 API의 응답을 전송할 수 있도록 하는 경우 적용되는 사항은 다음과 같습니다. 

- 전송되는 응답에 적용되는 작업은 다음과 같습니다.
  - 전송하는 사용자에게 표시된 콘텐츠에서 수정되지 않은 콘텐츠로 구성해야 합니다. 서식 변경은 허용됩니다.
  - 메타데이터 형식의 데이터는 포함할 수 없습니다.
  - Bing Web, Image, News, Video 및 Visual API에서 가져온 응답의 경우, 응답이 Bing에서 지원하는 인터넷 검색 환경을 통해 획득되었음을 나타내는 언어를 표시합니다. 예를 들어, "Powered by Bing(Bing 지원)" 또는 "Learn more about this image on Bing(Bing에서 이 이미지에 대해 자세히 알아보기)"과 같은 언어를 표시하거나 Bing 로고를 사용할 수 있습니다.
  - Bing Custom Search API에서 가져온 응답의 경우, 응답이 인터넷 검색 환경을 통해 획득되었음을 나타내는 언어를 표시합니다. 예를 들어, "Learn more about this search result(이 검색 결과에 대해 자세히 알아보기)"와 같은 언어를 표시할 수 있습니다.
  - 응답을 생성하는 데 사용된 전체 쿼리를 눈에 띄게 표시해야 합니다.
  - 직접적으로 또는 검색 엔진(bing.com이나 m.bing.com 또는 사용자 지정 검색 서비스)을 통해 응답의 기본 원본에 대해 눈에 띄는 링크 또는 이와 유사한 특성을 포함해야 합니다.
- 응답 전송은 자동화할 수 없습니다. 전송은 응답을 전송하려는 의도를 명확하게 나타내는 사용자 작업으로 시작해야 합니다.
- 사용자는 사용자의 쿼리 전송에 대한 응답으로 표시된 응답만 전송할 수 있습니다.

### <a name="continuity-of-service"></a>서비스 연속성 

Search API 응답의 데이터는 복사, 저장 또는 캐시하면 안 됩니다. 그러나 서비스 액세스 및 데이터 렌더링의 연속성을 사용하려면 다음과 같은 경우에만 결과를 유지할 수 있습니다.

#### <a name="device"></a>디바이스

다음과 같은 목적으로만 보관된 결과를 사용할 수 있다면 (i) 쿼리 시간으로부터 24시간 미만 동안 또는 (ii) 사용자가 업데이트된 결과에 대해 다른 쿼리를 제출할 때까지 사용자가 해당 결과를 디바이스에서 유지할 수 있습니다.

- 사용자가 해당 디바이스의 해당 사용자에게 이전에 반환된 결과에 액세스할 수 있도록 합니다(예: 서비스 중단의 경우).
- 해당 사용자의 신호(예: 예상되는 서비스 중단의 경우)에 따라 사용자의 요구 사항을 예상하여 개인 설정된 자동 관리 쿼리에 대해 반환된 결과를 저장합니다.

#### <a name="server"></a>서버

다음과 같은 목적에 한하여 한 명의 사용자에게 해당하는 결과를 고객이 제어하는 서버에 안전하게 보관하고 이 보관된 결과를 표시할 수 있습니다.

- 솔루션에서 해당 사용자에게 이전에 반환된 결과의 기록 보고서에 액세스할 수 있도록 하기 위한 목적입니다. 결과는 (i) 최종 사용자의 초기 쿼리 시간으로부터 21일 이상 보존되지 않으며 (ii) 사용자의 새 쿼리 또는 반복 쿼리의 응답으로 표시되지 않을 수 있습니다.
- 해당 사용자의 신호에 따라 사용자의 요구 사항을 예상하여 개인 설정된 자동 관리 쿼리에 대해 반환된 결과를 저장하기 위한 목적입니다. 이러한 결과는 (i) 쿼리 시점부터 24시간 이내 또는 (ii) 사용자가 업데이트된 결과에 대해 다른 쿼리를 제출하는 시점까지 중에서 더 짧은 기간 동안 저장할 수 있습니다.

특정 사용자에 대한 결과가 보존될 경우 다른 사용자의 결과와 혼합될 수 없습니다. 즉, 각 사용자의 결과는 별도로 유지되고 전달되어야 합니다.

### <a name="general"></a>일반 

보관된 결과의 모든 표시는 다음 조건을 충족해야 합니다.

- 쿼리가 전송된 시간을 명확하게 나타내는 정보를 포함합니다.
- 쿼리를 다시 실행하고 업데이트된 결과를 얻을 수 있는 버튼이나 이와 비슷한 방법을 사용자에게 제공합니다. 
- 결과 표시에서 Bing 브랜딩을 유지합니다.
- 지정된 시간 범위 내에 저장된 결과를 삭제합니다(필요한 경우 새 쿼리로 새로 고침).

### <a name="non-display-url-discovery"></a>비표시 URL 검색 

사용자 또는 고객의 쿼리에 응답하는 정보 출처의 URL을 검색하기 위한 용도로만 비인터넷 검색 환경에서 검색 응답을 사용할 수 있습니다. 다음과 같이 보고서 또는 사용자가 제공하는 유사한 응답에서 이러한 URL을 복사할 수 있습니다.

- 해당 쿼리에 대한 응답으로 해당 사용자 또는 고객에게만
- 쿼리와 관련된 중요한 추가 콘텐츠가 포함된 경우에만

이전 섹션의 Search API 사용 및 표시 요구 사항은 다음을 제외하고는 이러한 비표시 사용에 적용되지 않습니다. 

- 앞서 설명한 제한된 URL 복사 외에는 검색 응답에서 데이터 또는 내용을 캐시, 복사 또는 저장하거나 파생시키면 안됩니다.
- Search API에서 받은 데이터(URL 포함)를 사용함으로써 관련 법률이나 제3자의 권리를 침해하지 않는지 확인합니다.
- 검색 인덱스, 기계 학습 또는 이와 유사한 알고리즘 작업의 일부로 Search API에서 받은 데이터(URL 포함)는 사용하지 않습니다. 이 데이터를 사용자 또는 제3자가 제공할 수 있는 서비스를 학습하거나 평가하거나 개선하는 데 사용하지 않습니다.

## <a name="gdpr-compliance"></a>GDPR 규정 준수  

유럽 연합 GDPR(일반 데이터 보호 규정)을 따르며 Search API, Bing Spell Check API 또는 Bing Autosuggest API 호출과 연계해서 처리되는 모든 개인 데이터 주체와 관련해서, 사용자 및 Microsoft가 GDPR에서 독립적인 데이터 통제자임을 이해해야 합니다. 사용자는 GDPR 준수에 대해 별도로 책임을 집니다.  

