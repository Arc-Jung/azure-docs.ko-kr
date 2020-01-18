---
title: Azure Maps Search Service를 사용 하 여 위치 검색 | Microsoft Azure 맵
description: 이 문서에서는 Microsoft Azure Maps Search Service를 사용 하 여 위치를 검색 하는 방법을 알아봅니다.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 53856b4157afa5976947c451952fc26eefcdd0ea
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264189"
---
# <a name="find-an-address-using-the-azure-maps-search-service"></a>Azure Maps 검색 서비스를 사용하여 주소 찾기

지도 검색 서비스는 개발자를 위해 설계 된 RESTful Api 집합입니다. 서비스는 주소, 위치, 관심 영역, 비즈니스 목록 및 기타 지리 정보를 검색할 수 있습니다. 다음 각각에는 위도 및 경도 값 (특정 주소, 교차 주소, 지리적 기능 또는 POI (관심 지점))이 있습니다. 쿼리에서 반환 된 위도 및 경도 값을 다른 Map services의 매개 변수로 사용할 수 있습니다. 예를 들어, 반환 되는 값은 경로 서비스 또는 traffic flow 서비스에 대 한 매개 변수가 될 수 있습니다. 

다음 방법에 대해 알아보겠습니다.

* [유사 항목 검색 API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) 를 사용 하 여 주소 검색
* 속성 및 좌표와 함께 주소 검색
* 주소 검색을 위한 [역방향 주소 검색](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 만들기
* [검색 주소 역방향 교차 주소 API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet) 를 사용 하 여 교차 주소 검색

## <a name="prerequisites"></a>필수 조건

Maps 서비스 Api에 대 한 호출을 수행 하려면 Maps 계정 및 키가 필요 합니다. Azure Maps 계정을 만들려면 [계정 만들기](quick-demo-map-app.md#create-an-account-with-azure-maps)의 지침을 따르세요. 기본 키 가져오기에 대 한 도움이 필요한 경우 [기본 키 가져오기](quick-demo-map-app.md#get-the-primary-key-for-your-account)의 단계를 수행 합니다. Azure Maps의 인증에 대 한 자세한 내용은 [Azure Maps 인증 관리](./how-to-manage-authentication.md)를 참조 하세요.

이 문서에서는 [우체부 앱](https://www.getpostman.com/apps)을 사용하여 REST 호출을 빌드합니다. 선호하는 모든 API 개발 환경을 사용할 수 있습니다.

## <a name="using-fuzzy-search"></a>유사 항목 검색 사용

검색 서비스에 대 한 기본 API는 [유사 항목 검색](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)입니다. 이 서비스는 검색 쿼리에서 사용자 입력 형식이 확실 하지 않은 경우에 유용 합니다. API는 POI 검색과 지오코딩을 정식 '한 줄 검색'으로 결합합니다. 예를 들어 API는 모든 주소 또는 POI 토큰 조합의 입력을 처리할 수 있습니다. 또한 컨텍스트 위치 (/lon.)를 사용 하 여 가중치를 지정할 수 있습니다. 쌍)에 가중치를 부여하거나, 좌표와 반경으로 완전히 제한하거나, 지리적으로 편향된(geo biasing) 앵커 지점 없이 더 일반적으로 실행할 수도 있습니다.

대부분의 검색 쿼리는 성능을 달성하고 비정상적인 결과를 줄이기 위해 기본적으로 `maxFuzzyLevel=1`로 설정됩니다. 이 기본값은 요청에 따라 필요한 경우 쿼리 매개 변수에 `maxFuzzyLevel=2` 또는 `3`을 전달하여 재정의할 수 있습니다.

### <a name="search-for-an-address-using-fuzzy-search"></a>유사 항목 검색을 사용하여 주소 검색

1. Postman 앱을 열고 새로 만들기 새로 만들기를 클릭하고 **GET 요청**을 선택합니다. **유사 항목 검색**의 요청 이름을 입력하고 이를 저장할 컬렉션 또는 폴더를 선택하고 **저장**을 클릭합니다.

2. 작성기 탭에서 **GET** HTTP 메서드를 선택하고 API 엔드포인트에 대한 요청 URL을 입력합니다.

    ![유사 항목 검색](./media/how-to-search-for-address/fuzzy_search_url.png)

    | 매개 변수 | 제안 값 |
    |---------------|------------------------------------------------|
    | HTTP 메서드 | GET |
    | 요청 URL | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | 권한 부여 | 인증 없음 |

    URL 경로의 **json** 특성은 응답 형식을 결정합니다. 이 문서에서는 사용 편의성과 가독성을 위해 json을 사용 합니다. 사용 가능한 응답 형식은 [Maps 기능 API 참조](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)의 **유사 항목 검색 가져오기** 정의에서 찾을 수 있습니다.

3. **Params**를 클릭하고 요청 URL에 쿼리 또는 경로 매개 변수로 사용할 다음 키 / 값 쌍을 입력합니다.

    ![유사 항목 검색](./media/how-to-search-for-address/fuzzy_search_params.png)

    | 키 | 값 |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | 구독 키 | \<사용자의 Azure 맵 키\> |
    | Query | 피자 |

4. **보내기**를 클릭하고 응답 본문을 검토합니다.

    "피자"의 모호한 쿼리 문자열은 "피자" 및 "식당" 범주에서 10 개의 [이율 결과](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (poi)를 반환 했습니다. 각 결과는 해당 위치에 대 한 주소, 위도 및 경도 값, 뷰 포트 및 진입점을 반환 합니다.
  
    결과는 특정 기준 위치에 연결되지 않고 이 쿼리에 대해 변화됩니다. **CountrySet** 매개 변수를 사용 하 여 응용 프로그램에 적용 해야 하는 국가/지역만 지정할 수 있습니다. 기본 동작은 전체 세계를 검색 하 여 불필요 한 결과를 반환 하는 것입니다.

5. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |------------------|-------------------------|
    | countrySet | 미국 |
  
    이제 결과는 국가 코드로 경계가 구분되며 쿼리는 미국의 피자 음식점을 반환합니다.
  
    위치에 대한 결과를 제공하려면 관심 지점을 쿼리하고 반환된 위도와 경도 값을 유사 항목 검색 서비스에 대한 호출에 사용할 수 있습니다. 이 경우 검색 서비스를 사용하여 시애틀의 스페이스 니들(Space Needle) 탑의 위치를 반환하고 위도 / 경도를 사용했습니다. 검색 방향을 지정하는 값입니다.
  
6. 매개 변수에서 다음 키 / 값 쌍을 입력하고 **보내기**를 클릭합니다.

    ![유사 항목 검색](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | 키 | 값 |
    |-----|------------|
    | lat | 47.620525 |
    | lon | -122.349274 |

## <a name="search-for-address-properties-and-coordinates"></a>주소 속성 및 좌표 검색

전체 또는 부분 주소를 검색 주소 API에 전달할 수 있습니다. 자세한 주소 속성을 포함 하는 응답을 받게 됩니다. 자세한 주소 속성은 고도 및 경도, 지방 자치 체 또는 하위 분류의 위치 값과 같은 값입니다.

1. Postman에서 **새 요청** | **GET 요청**을 클릭하고 이름을 **주소 검색**으로 지정합니다.
2. 작성기 탭에서 **GET** HTTP 메서드를 선택하고 API 엔드포인트에 대한 요청 URL을 입력한 다음 권한 부여 프로토콜이 있는 경우 해당 프로토콜을 선택합니다.

    ![주소 검색](./media/how-to-search-for-address/address_search_url.png)
  
    | 매개 변수 | 제안 값 |
    |---------------|------------------------------------------------|
    | HTTP 메서드 | GET |
    | 요청 URL | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) |
    | 권한 부여 | 인증 없음 |

3. **Params**를 클릭하고 요청 URL에 쿼리 또는 경로 매개 변수로 사용할 다음 키 / 값 쌍을 입력합니다.
  
    ![주소 검색](./media/how-to-search-for-address/address_search_params.png)
  
    | 키 | 값 |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | 구독 키 | \<사용자의 Azure 맵 키\> |
    | Query | 400 한 광범위 한 St, 시애틀, WA 98109 |
  
4. **보내기**를 클릭하고 응답 본문을 검토합니다.
  
    이 경우 전체 주소 쿼리를 지정했으며 응답 본문에 단일 결과를 받습니다.
  
5. 매개 변수에 다음 값에 대한 쿼리 문자열을 편집합니다.
    ```plaintext
        400 Broad, Seattle
    ```

6. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |-----|------------|
    | typeahead | true |

    **typeahead** 플래그는 주소 검색 API에 쿼리를 부분 입력으로 처리하고 예측 값의 배열을 반환하도록 지시합니다.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>역방향 주소 검색을 사용하여 주소 검색

1. Postman에서 **새 요청** | **GET 요청**을 클릭하고 **역 주소 검색**으로 이름 지정합니다.

2. 작성기 탭에서 **GET** HTTP 메서드를 선택하고 API 엔드포인트에 대한 요청 URL을 입력합니다.
  
    ![역 주소 검색 URL](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | 매개 변수 | 제안 값 |
    |---------------|------------------------------------------------|
    | HTTP 메서드 | GET |
    | 요청 URL | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | 권한 부여 | 인증 없음 |
  
3. **Params**를 클릭하고 요청 URL에 쿼리 또는 경로 매개 변수로 사용할 다음 키 / 값 쌍을 입력합니다.
  
    ![역 주소 검색 매개 변수](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | 키 | 값 |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | 구독 키 | \<사용자의 Azure 맵 키\> |
    | Query | 47.591180,-122.332700 |
  
4. **보내기**를 클릭하고 응답 본문을 검토합니다.

    응답에는 Safeco Field에 대한 주요 주소 정보가 포함됩니다.
  
5. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |-----|------------|
    | number | true |

    요청을 사용 하 여 [number](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 쿼리 매개 변수를 보내는 경우 응답에는 거리 (왼쪽 또는 오른쪽)와 해당 숫자의 오프셋 위치가 포함 될 수 있습니다.
  
6. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |-----|------------|
    | returnSpeedLimit | true |
  
    [ReturnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 쿼리 매개 변수를 설정 하면 응답에서 게시 된 속도 제한을 반환 합니다.

7. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |-----|------------|
    | returnRoadUse | true |

    [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 쿼리 매개 변수가 설정되면 응답에서 도로 수준의 역방향 지역 코드에 대한 도로 사용 배열을 반환합니다.

8. **Params** 섹션에 다음 키/값 쌍을 추가하고 **보내기**를 클릭합니다.

    | 키 | 값 |
    |-----|------------|
    | roadUse | true |

    [RoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 쿼리 매개 변수를 사용 하 여 역방향 geocode 쿼리를 특정 유형의 이동으로 제한할 수 있습니다.
  
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>역 주소 교차로 검색을 사용하여 교차로 검색

1. Postman에서 **새 요청** | **GET 요청**을 클릭하고 **역 주소 교차로 검색**으로 이름 지정합니다.

2. 작성기 탭에서 **GET** HTTP 메서드를 선택하고 API 엔드포인트에 대한 요청 URL을 입력합니다.
  
    ![역 주소 교차로 검색](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | 매개 변수 | 제안 값 |
    |---------------|------------------------------------------------|
    | HTTP 메서드 | GET |
    | 요청 URL | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | 권한 부여 | 인증 없음 |
  
3. **Params**를 클릭하고 요청 URL에 쿼리 또는 경로 매개 변수로 사용할 다음 키 / 값 쌍을 입력합니다.
  
    | 키 | 값 |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | 구독 키 | \<사용자의 Azure 맵 키\> |
    | Query | 47.591180,-122.332700 |
  
4. **보내기**를 클릭하고 응답 본문을 검토합니다.

## <a name="next-steps"></a>다음 단계

- [Azure Maps 검색 서비스](https://docs.microsoft.com/rest/api/maps/search) API 설명서를 살펴봅니다.
