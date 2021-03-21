---
title: Microsoft Azure 맵에서 지도의 데이터 원본 만들기
description: 지도의 데이터 원본을 만드는 방법에 대해 알아보세요. Azure Maps 웹 SDK에서 사용 하는 데이터 원본에 대해 알아봅니다. GeoJSON 원본 및 벡터 타일.
author: rbrundritt
ms.author: richbrun
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen, devx-track-js
ms.openlocfilehash: 9964c99ddfb59811fc67df634b41cede5847ede0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97678849"
---
# <a name="create-a-data-source"></a>데이터 소스 만들기

Azure Maps 웹 SDK는 데이터 원본에 데이터를 저장 합니다. 데이터 원본을 사용 하 여 쿼리 및 렌더링을 위한 데이터 작업을 최적화 합니다. 현재 다음과 같은 두 가지 유형의 데이터 원본이 있습니다.

* **GeoJSON source**: 로컬에서 GeoJSON 형식의 원시 위치 데이터를 관리 합니다. 중소 규모의 데이터 집합에 적합 합니다 (수천 개의 셰이프).
* **벡터 타일 원본**: 지도 바둑판식 배열 시스템을 기반으로 현재 지도 보기의 벡터 타일로 형식이 지정 된 데이터를 로드 합니다. 대규모 데이터 집합 (수백만 또는 수십억 개의 도형)에 이상적입니다.

## <a name="geojson-data-source"></a>GeoJSON 데이터 원본

GeoJSON 기반 데이터 소스는 클래스를 사용 하 여 데이터를 로컬에서 로드 하 고 저장 `DataSource` 합니다. GeoJSON 네임 스페이스의 도우미 클래스를 사용 하 여 수동으로 데이터를 만들거나 만들 수 [있습니다.](/javascript/api/azure-maps-control/atlas.data) `DataSource`클래스는 로컬 또는 원격 GeoJSON 파일을 가져오기 위한 함수를 제공 합니다. 원격 GeoJSON 파일은 CORs 사용 끝점에서 호스팅되어야 합니다. `DataSource`클래스는 클러스터링 지점 데이터에 대 한 기능을 제공 합니다. 그리고 클래스를 사용 하 여 데이터를 쉽게 추가, 제거 및 업데이트할 수 있습니다 `DataSource` . 다음 코드는 Azure Maps에서 GeoJSON 데이터를 만드는 방법을 보여 줍니다.

```javascript
//Create raw GeoJSON object.
var rawGeoJson = {
     "type": "Feature",
     "geometry": {
         "type": "Point",
         "coordinates": [-100, 45]
     },
     "properties": {
         "custom-property": "value"
     }
};

//Create GeoJSON using helper classes (less error prone and less typing).
var geoJsonClass = new atlas.data.Feature(new atlas.data.Point([-100, 45]), {
    "custom-property": "value"
}); 
```

데이터 원본을 만든 후에는 `map.sources` [sourcemanager](/javascript/api/azure-maps-control/atlas.sourcemanager)인 속성을 통해 지도에 데이터 원본을 추가할 수 있습니다. 다음 코드에서는를 만들어 맵에 추가 하는 방법을 보여 줍니다 `DataSource` .

```javascript
//Create a data source and add it to the map.
var source = new atlas.source.DataSource();
map.sources.add(source);
```

다음 코드는 GeoJSON 데이터를에 추가할 수 있는 여러 가지 방법을 보여 줍니다 `DataSource` .

```javascript
//GeoJsonData in the following code can be a single or array of GeoJSON features or geometries, a GeoJSON feature colleciton, or a single or array of atlas.Shape objects.

//Add geoJSON object to data source. 
source.add(geoJsonData);

//Load geoJSON data from URL. URL should be on a CORs enabled endpoint.
source.importDataFromUrl(geoJsonUrl);

//Overwrite all data in data source.
source.setShapes(geoJsonData);
```

> [!TIP]
> 의 모든 데이터를 덮어쓰도록 할 수 있습니다 `DataSource` . Then 함수를 호출 하면 `clear` `add` 맵이 두 번 다시 렌더링 되어 약간의 지연이 발생할 수 있습니다. 대신 함수를 사용 하 여 `setShapes` 데이터 원본의 모든 데이터를 제거 하 고 바꾸고 지도의 단일 다시 렌더링만 트리거합니다.

## <a name="vector-tile-source"></a>벡터 타일 원본

벡터 타일 소스는 벡터 타일 계층에 액세스 하는 방법을 설명 합니다. [VectorTileSource](/javascript/api/azure-maps-control/atlas.source.vectortilesource) 클래스를 사용 하 여 벡터 타일 소스를 인스턴스화합니다. 벡터 타일 계층은 타일 계층과 비슷하지만 동일 하지는 않습니다. 타일 계층은 래스터 이미지입니다. 벡터 타일 계층은 압축 파일 ( **Pf** 형식)입니다. 이 압축 파일은 벡터 맵 데이터 및 하나 이상의 계층을 포함 합니다. 각 계층의 스타일에 따라 파일을 클라이언트에서 렌더링 하 고 스타일을 지정할 수 있습니다. 벡터 타일의 데이터에는 요소, 선 및 다각형 형식의 지리적 기능이 포함 되어 있습니다. 래스터 타일 계층 대신 벡터 타일 계층을 사용할 경우 다음과 같은 몇 가지 이점이 있습니다.

* 벡터 타일의 파일 크기는 일반적으로 해당 하는 래스터 타일 보다 훨씬 작습니다. 따라서 대역폭이 줄어듭니다. 낮은 대기 시간, 더 빠른 맵 및 더 나은 사용자 환경을 의미 합니다.
* 벡터 타일은 클라이언트에서 렌더링 되므로 표시 되는 장치의 해상도에 맞게 조정 됩니다. 따라서 렌더링 된 맵은 명확 하 고 명확한 레이블이 있는 잘 정의 된 것으로 나타납니다.
* 클라이언트에 새 스타일을 적용할 수 있으므로 벡터 맵에서 데이터의 스타일을 변경 하는 경우 데이터를 다시 다운로드할 필요가 없습니다. 반면 래스터 타일 계층의 스타일을 변경 하는 경우 일반적으로 서버에서 타일을 로드 한 다음 새 스타일을 적용 해야 합니다.
* 데이터가 벡터 형식으로 전달 되므로 데이터를 준비 하는 데 필요한 서버 쪽 처리가 줄어듭니다. 따라서 최신 데이터를 더 빨리 사용할 수 있습니다.

Azure Maps는 [Mapbox Vector 타일 사양](https://github.com/mapbox/vector-tile-spec)(개방형 표준)을 준수 합니다. Azure Maps는 플랫폼의 일부로 다음 벡터 타일 서비스를 제공 합니다.

- 도로 타일 [설명서](/rest/api/maps/renderv2/getmaptilepreview)  |  [데이터 형식 세부 정보](https://developer.tomtom.com/maps-api/maps-api-documentation-vector/tile)
- 트래픽 인시던트 [설명서](/rest/api/maps/traffic/gettrafficincidenttile)  |  [데이터 형식 세부 정보](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-incidents/vector-incident-tiles)
- 트래픽 흐름 [설명서](/rest/api/maps/traffic/gettrafficflowtile)  |  [데이터 형식 세부 정보](https://developer.tomtom.com/traffic-api/traffic-api-documentation-traffic-flow/vector-flow-tiles)
- Azure Maps Creator (미리 보기)를 사용 하 여 사용자 지정 벡터 타일을 만들고, [Get 타일 렌더링 V2](/rest/api/maps/renderv2/getmaptilepreview) 를 통해 액세스할 수도 있습니다.

> [!TIP]
> 웹 SDK를 사용 하 여 Azure Maps render service에서 벡터 또는 래스터 이미지 타일을 사용 하는 경우를 `atlas.microsoft.com` 자리 표시자로 바꿀 수 있습니다 `{azMapsDomain}` . 이 자리 표시자는 맵에 사용 되는 동일한 도메인으로 바뀌고 동일한 인증 세부 정보도 자동으로 추가 됩니다. 이렇게 하면 Azure Active Directory 인증을 사용 하는 경우 렌더링 서비스에 대 한 인증이 매우 간단해 집니다.

지도에 벡터 타일 원본의 데이터를 표시 하려면 데이터 렌더링 계층 중 하나에 원본을 연결 합니다. 벡터 원본을 사용 하는 모든 계층은 옵션에 값을 지정 해야 합니다 `sourceLayer` . 다음 코드는 Azure Maps traffic flow vector 타일 서비스를 벡터 타일 원본으로 로드 한 다음 선 계층을 사용 하 여 지도에 표시 합니다. 이 벡터 타일 원본에는 원본 계층에서 "트래픽 흐름" 이라는 단일 데이터 집합이 있습니다. 이 데이터 집합의 줄 데이터에는 `traffic_level` 이 코드에서 색을 선택 하 고 선의 크기를 조정 하는 데 사용 되는 라는 속성이 있습니다.

```javascript
//Create a vector tile source and add it to the map.
var source = new atlas.source.VectorTileSource(null, {
    tiles: ['https://{azMapsDomain}/traffic/flow/tile/pbf?api-version=1.0&style=relative&zoom={z}&x={x}&y={y}'],
    maxZoom: 22
});
map.sources.add(source);

//Create a layer for traffic flow lines.
var flowLayer = new atlas.layer.LineLayer(source, null, {
    //The name of the data layer within the data source to pass into this rendering layer.
    sourceLayer: 'Traffic flow',

    //Color the roads based on the traffic_level property. 
    strokeColor: [
        'interpolate',
        ['linear'],
        ['get', 'traffic_level'],
        0, 'red',
        0.33, 'orange',
        0.66, 'green'
    ],

    //Scale the width of roads based on the traffic_level property. 
    strokeWidth: [
        'interpolate',
        ['linear'],
        ['get', 'traffic_level'],
        0, 6,
        1, 1
    ]
});

//Add the traffic flow layer below the labels to make the map clearer.
map.layers.add(flowLayer, 'labels');
```

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="벡터 타일 선 계층" src="https://codepen.io/azuremaps/embed/wvMXJYJ?height=500&theme-id=default&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen의 Azure Maps ()로 펜 <a href='https://codepen.io/azuremaps/pen/wvMXJYJ'>벡터 타일 선 계층</a> 을 참조 하세요 <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'></a>
</iframe>

<br/>

## <a name="connecting-a-data-source-to-a-layer"></a>계층에 데이터 원본 연결

데이터는 렌더링 레이어를 사용 하 여 맵에서 렌더링 됩니다. 하나 이상의 렌더링 레이어에서 단일 데이터 원본을 참조할 수 있습니다. 다음 렌더링 계층에는 데이터 원본이 필요 합니다.

* [거품형 계층](map-add-bubble-layer.md) -지도에서 점 데이터를 크기가 조정 된 원으로 렌더링 합니다.
* [기호 계층](map-add-pin.md) -지점 데이터를 아이콘이 나 텍스트로 렌더링 합니다.
* [열 지도 계층](map-add-heat-map-layer.md) -점 데이터를 밀도 열 맵으로 렌더링 합니다.
* [선 계층](map-add-shape.md) -선을 렌더링 하거나 다각형의 윤곽선을 렌더링 합니다. 
* [다각형 계층](map-add-shape.md) -단색 또는 이미지 패턴으로 다각형 영역을 채웁니다.

다음 코드에서는 데이터 원본을 만들고 지도에 추가한 다음 거품형 계층에 연결 하는 방법을 보여 줍니다. 그런 다음 원격 위치에서 데이터 원본으로 GeoJSON point 데이터를 가져옵니다. 

```javascript
//Create a data source and add it to the map.
var source = new atlas.source.DataSource();
map.sources.add(source);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(source));

//Load the earthquake data.
source.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

이러한 데이터 소스에 연결 하지 않는 추가 렌더링 계층이 있지만 렌더링을 위해 데이터를 직접 로드 합니다. 

* [이미지 계층](map-add-image-layer.md) -지도 위에 단일 이미지를 오버레이 하 고 해당 모퉁이를 지정 된 좌표 집합에 바인딩합니다.
* [타일 계층](map-add-tile-layer.md) -수퍼는 지도 위에 래스터 타일 계층을 적용 합니다.

## <a name="one-data-source-with-multiple-layers"></a>여러 계층이 있는 하나의 데이터 원본

여러 레이어는 단일 데이터 원본에 연결할 수 있습니다. 이 옵션이 유용 하 게 사용할 수 있는 여러 가지 시나리오가 있습니다. 예를 들어 사용자가 다각형을 그리는 시나리오를 생각해 보겠습니다. 사용자가 지도에 점이 추가 될 때 다각형 영역을 렌더링 하 고 채워야 합니다. 스타일이 지정 된 선을 추가 하 여 다각형을 윤곽선으로 만들면 사용자가 그릴 때 다각형의 가장자리를 더 쉽게 볼 수 있습니다. 다각형의 개별 위치를 편리 하 게 편집 하기 위해 각 위치 위에 핀 이나 표식 같은 핸들을 추가할 수 있습니다.

![단일 데이터 원본에서 데이터를 렌더링 하는 여러 레이어를 보여 주는 지도](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

대부분의 매핑 플랫폼에서 다각형 개체, 선 개체 및 다각형의 각 위치에 대 한 pin이 필요 합니다. 다각형이 수정 될 때 줄과 핀을 수동으로 업데이트 해야 하며,이는 빠르게 복잡 해질 수 있습니다.

Azure Maps를 사용 하는 경우 아래 코드와 같이 데이터 소스에 단일 다각형이 필요 합니다.

```javascript
//Create a data source and add it to the map.
var source = new atlas.source.DataSource();
map.sources.add(source);

//Create a polygon and add it to the data source.
source.add(new atlas.data.Polygon([[[/* Coordinates for polygon */]]]));

//Create a polygon layer to render the filled in area of the polygon.
var polygonLayer = new atlas.layer.PolygonLayer(source, 'myPolygonLayer', {
     fillColor: 'rgba(255,165,0,0.2)'
});

//Create a line layer for greater control of rendering the outline of the polygon.
var lineLayer = new atlas.layer.LineLayer(source, 'myLineLayer', {
     color: 'orange',
     width: 2
});

//Create a bubble layer to render the vertices of the polygon as scaled circles.
var bubbleLayer = new atlas.layer.BubbleLayer(source, 'myBubbleLayer', {
     color: 'orange',
     radius: 5,
     strokeColor: 'white',
     strokeWidth: 2
});

//Add all layers to the map.
map.layers.add([polygonLayer, lineLayer, bubbleLayer]);
```

> [!TIP]
> 함수를 사용 하 여 지도에 레이어를 추가 하는 경우 `map.layers.add` 기존 계층의 ID 또는 인스턴스를 두 번째 매개 변수로 전달할 수 있습니다. 그러면 기존 계층 아래에 추가 되는 새 계층이 삽입 됩니다. 계층 ID를 전달 하는 것 외에도이 메서드는 다음 값도 지원 합니다.
>
> * `"labels"` -지도 레이블 계층 아래에 새 계층을 삽입 합니다.
> * `"transit"` -지도 이동 및 전송 계층 아래에 새 계층을 삽입 합니다.

## <a name="next-steps"></a>다음 단계

이 문서에서 사용된 클래스 및 메서드에 대해 자세히 알아봅니다.

> [!div class="nextstepaction"]
> [DataSource](/javascript/api/azure-maps-control/atlas.source.datasource)

> [!div class="nextstepaction"]
> [DataSourceOptions](/javascript/api/azure-maps-control/atlas.datasourceoptions)

> [!div class="nextstepaction"]
> [VectorTileSource](/javascript/api/azure-maps-control/atlas.source.vectortilesource)

> [!div class="nextstepaction"]
> [VectorTileSourceOptions](/javascript/api/azure-maps-control/atlas.vectortilesourceoptions)

맵에 추가할 더 많은 코드 예제를 보려면 다음 문서를 참조하세요.

> [!div class="nextstepaction"]
> [팝업 추가](map-add-popup.md)

> [!div class="nextstepaction"]
> [데이터 기반 스타일 식 사용](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [기호 계층 추가](map-add-pin.md)

> [!div class="nextstepaction"]
> [거품형 계층 추가](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [선 계층 추가](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [다각형 계층 추가](map-add-shape.md)

> [!div class="nextstepaction"]
> [열 지도 추가](map-add-heat-map-layer.md)

> [!div class="nextstepaction"]
> [코드 샘플](/samples/browse/?products=azure-maps)