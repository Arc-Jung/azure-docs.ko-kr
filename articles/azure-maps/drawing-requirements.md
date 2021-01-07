---
title: Microsoft Azure Maps 작성자의 패키지 요구 사항 그리기 (미리 보기)
description: 기능 디자인 파일을 지도 데이터로 변환 하기 위한 그리기 패키지 요구 사항에 대해 알아봅니다.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philMea
ms.openlocfilehash: 26b6273b4dd2371790025515e35b71d1fc863ebe
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96903465"
---
# <a name="drawing-package-requirements"></a>그리기 패키지 요구 사항


> [!IMPORTANT]
> Azure Maps 작성자 서비스는 현재 공개 미리 보기로 제공 됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다. 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

[Azure Maps 변환 서비스](/rest/api/maps/conversion)를 사용 하 여 업로드 된 그리기 패키지를 맵 데이터로 변환할 수 있습니다. 이 문서에서는 Conversion API의 그리기 패키지 요구 사항에 대해 설명합니다. 패키지 샘플을 보려면 [그리기 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples) 샘플을 다운로드하면 됩니다.

## <a name="prerequisites"></a>사전 요구 사항

그리기 패키지에는 Autodesk의 AutoCAD® 소프트웨어에 대 한 네이티브 파일 형식인 DWG 형식으로 저장 된 드로잉이 포함 되어 있습니다.

드로잉 패키지에서 드로잉을 생성할 CAD 소프트웨어를 선택할 수 있습니다.  

[Azure Maps Conversion 서비스](/rest/api/maps/conversion)는 그리기 패키지를 맵 데이터로 변환합니다. 변환 서비스는 AutoCAD DWG 파일 형식으로 작동 합니다. `AC1032` 는 DWG 파일의 내부 형식 버전 이며 `AC1032` 내부 dwg 파일 형식 버전에 대해 선택 하는 것이 좋습니다.  

## <a name="glossary-of-terms"></a>용어집

쉽게 참조할 수 있도록이 문서를 읽을 때 중요 한 몇 가지 용어 및 정의는 다음과 같습니다.

| 용어  | 정의 |
|:-------|:------------|
| 계층 | AutoCAD DWG 레이어입니다.|
| Level | 설정된 고도에 있는 건물의 영역입니다. 예를 들어 건물의 층입니다. |
| Xref  |주 그리기에 외부 참조로 연결 된 AutoCAD DWG 파일 형식 (. i n t)의 파일입니다.  |
| 기능 | 기하 도형을 추가 메타데이터 정보와 결합하는 개체입니다. |
| 기능 클래스 | 기능에 대한 일반적인 청사진입니다. 예를 들어, *단위* 는 기능 클래스이 고 *office* 는 기능입니다. |

## <a name="drawing-package-structure"></a>패키지 구조 그리기

그리기 패키지는 다음 파일이 포함된 .zip 보관 파일입니다.

* AutoCAD DWG 파일 형식의 DWG 파일
* 단일 시설에 대한 _manifest.json_ 파일

폴더 내에서 다른 방식으로 DWG 파일을 구성할 수 있지만 매니페스트 파일은 폴더의 루트 디렉터리에 있어야 합니다. .Zip 확장명을 사용 하 여 단일 보관 파일에 폴더를 압축 해야 합니다. 다음 섹션에서는 DWG 파일, 매니페스트 파일 및 이러한 파일의 내용에 대 한 요구 사항을 자세히 설명 합니다.  

## <a name="dwg-files-requirements"></a>DWG 파일 요구 사항

각 시설 수준에는 단일 DWG 파일이 필요합니다. 수준의 데이터는 단일 DWG 파일에 포함되어야 합니다. 외부 참조(_xrefs_)는 부모 그리기에 바인딩되어야 합니다. 또한 각 DWG 파일은 다음과 같습니다.

* _실외_ 및 _단위_ 레이어를 정의해야 합니다. 필요에 따라 _벽_, _도어_, 단위 _레이블_, _영역_ 및 _ZoneLabel_ 등의 선택적 계층을 정의할 수 있습니다.
* 여러 수준의 기능을 포함할 수 없습니다.
* 여러 시설의 기능을 포함할 수 없습니다.

[Azure Maps Conversion 서비스](/rest/api/maps/conversion)는 DWG 파일에서 다음 기능 클래스를 추출할 수 있습니다.

* Levels
* Units
* 영역
* 통로
* 벽
* 세로 penetrations

모든 변환 작업을 수행하면 기본 범주의 최소 세트인 room, structure.wall, opening.door, zone 및 facility가 생성됩니다. 추가 범주는 개체에서 참조 하는 각 범주 이름에 대 한 것입니다.  

DWG 레이어에는 단일 클래스의 기능이 포함되어야 합니다. 클래스는 레이어를 공유하지 않아야 합니다. 예를 들어 단위와 벽은 레이어를 공유할 수 없습니다.

또한 DWG 레이어가 따라야 하는 조건은 다음과 같습니다.

* 모든 DWG 파일의 그리기 원점은 동일한 위도와 경도에 맞춰야 합니다.
* 각 수준은 다른 수준과 동일한 방향이어야 합니다.
* 자체 교차 다각형이 자동으로 복구 되 고 [Azure Maps 변환 서비스](/rest/api/maps/conversion) 에서 경고가 발생 합니다. 예상한 결과와 일치 하지 않을 수 있으므로 복구 된 결과를 수동으로 검사 해야 합니다.

모든 레이어 엔터티는 선, 다중선, 다각형, 원호, 원 또는 텍스트 (한 줄) 형식 중 하나 여야 합니다. 다른 모든 엔터티 형식은 무시 됩니다.

다음 표에서는 각 계층에 대해 지원 되는 엔터티 형식 및 지원 되는 기능을 간략하게 설명 합니다. 계층에 지원 되지 않는 엔터티 형식이 포함 된 경우 [Azure Maps 변환 서비스](/rest/api/maps/conversion) 는 이러한 엔터티를 무시 합니다.  

| 계층 | 엔터티 형식 | 기능 |
| :----- | :-------------------| :-------
| [실외](#exterior-layer) | 다각형, 폴리라인(닫힌), 원 | Levels
| [단위](#unit-layer) |  다각형, 폴리라인(닫힌), 원 | 세로 penetrations, 단위
| [벽](#wall-layer)  | 다각형, 폴리라인(닫힌), 원 | 해당 사항 없음 자세한 내용은 [벽 레이어](#wall-layer)를 참조하세요.
| [문](#door-layer) | 다각형, 폴리라인(닫힌), 선, 원호(CircularArc), 원 | 통로
| [영역](#zone-layer) | 다각형, 폴리라인(닫힌), 원 | 영역
| [UnitLabel(단위 레이블)](#unitlabel-layer) | 텍스트(단일선) | 해당 사항 없음 이 레이어는 속성을 단위 레이어의 단위 기능에만 추가할 수 있습니다. 자세한 내용은 [UnitLabel 레이어](#unitlabel-layer)를 참조하세요.
| [ZoneLabel(영역 레이블)](#zonelabel-layer) | 텍스트(단일선) | 해당 사항 없음 이 레이어는 속성을 ZonesLayer의 영역 기능에만 추가할 수 있습니다. 자세한 내용은 [ZoneLabel 계층](#zonelabel-layer)을 참조 하세요.

다음 섹션에서는 각 레이어의 요구 사항에 대해 자세히 설명합니다.

### <a name="exterior-layer"></a>실외 레이어

각 수준에 대한 DWG 파일에는 해당 수준의 경계를 정의하는 레이어가 포함되어야 합니다. 이 계층을 *외부* 계층 이라고 합니다. 예를 들어 두 개의 수준이 시설에 포함된 경우 각 파일에 대한 실외 레이어가 포함된 두 개의 DWG 파일이 있어야 합니다.

외부 계층에 있는 엔터티 그리기의 수에 관계 없이 [결과 기능 데이터 집합](tutorial-creator-indoor-maps.md#create-a-feature-stateset) 은 각 DWG 파일에 대해 하나의 수준 기능만 포함 합니다. 추가 필수 구성 요소:

* Exteriors는 다각형, 폴리라인 (닫힌) 또는 원으로 그려야 합니다.
* Exteriors은 겹칠 수 있지만 하나의 기 하 도형으로 사라지고 됩니다.

레이어에 겹치는 여러 폴리라인가 포함 되어 있으면 폴리라인 단일 수준 기능으로 사라지고 됩니다. 또는 레이어에 겹치지 않는 여러 폴리라인가 포함 되어 있으면 결과 수준 기능에 다중 다각형 표현이 있습니다.

외부 계층의 예제는 [샘플 드로잉 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)의 개요 계층으로 볼 수 있습니다.

### <a name="unit-layer"></a>단위 레이어

각 수준에 대 한 DWG 파일은 단위를 포함 하는 계층을 정의 합니다. 단위는 사무실, 복도, 계단 및 엘리베이터와 같은 건물 내에서 탐색 가능한 공간입니다. 단위 레이어는 다음 요구 사항을 따라야 합니다.

* 단위는 다각형, 다중선 (닫힌) 또는 원으로 그려야 합니다.
* 단위는 시설 외부 경계의 범위 내에 있어야 합니다.
* 단위는 부분적으로 겹칠 수 없습니다.
* 단위는 자체 교차 기하 도형을 포함할 수 없습니다.

Unit 레이블 계층에서 텍스트 개체를 만든 다음 단위의 범위 내에 개체를 추가 하 여 단위의 이름을로 지정한 다음 자세한 내용은 [UnitLabel 레이어](#unitlabel-layer)를 참조하세요.

[샘플 드로잉 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)에서 단위 계층의 예를 볼 수 있습니다.

### <a name="wall-layer"></a>벽 레이어

각 수준의 DWG 파일에는 벽, 열 및 기타 빌딩 구조의 물리적 익스텐트를 정의 하는 계층이 포함 될 수 있습니다.

* 벽은 다각형, 다중선 (닫힌) 또는 원으로 그려야 합니다.
* 벽 계층은 빌딩 구조체로 해석 된 기 하 도형만 포함 해야 합니다.

[샘플 드로잉 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)에서 벽 레이어의 예를 볼 수 있습니다.

### <a name="door-layer"></a>문 레이어

문이 포함 된 DWG 계층을 포함할 수 있습니다. 각 도어가 단위 계층에서 단위의 가장자리와 겹쳐야 합니다.

Azure Maps 데이터 집합의 도어는 여러 단위 경계를 겹치는 한 줄 세그먼트로 표현 됩니다. 다음 이미지는 도어 계층의 기 하 도형을 변환 하 여 데이터 집합의 기능을 여는 방법을 보여 줍니다.

![입구를 생성 하는 단계를 보여 주는 4 개의 그래픽](./media/drawing-requirements/opening-steps.png)

### <a name="zone-layer"></a>영역 레이어

각 수준의 DWG 파일에는 영역의 실제 익스텐트를 정의 하는 영역 계층이 포함 될 수 있습니다. 영역은 실내 빈 공간 또는 뒷마당일 수 있습니다.

* 영역을 다각형, 폴리라인 (닫힌) 또는 원으로 그려야 합니다.
* 영역이 겹칠 수 있습니다.
* 영역은 시설의 외부 경계 내부 또는 외부에 있을 수 있습니다.

ZoneLabel 계층에서 텍스트 개체를 만들고 영역 범위 내에 텍스트 개체를 배치 하 여 영역 이름을로 합니다. 자세한 내용은 [ZoneLabel 레이어](#zonelabel-layer)를 참조하세요.

[샘플 그리기 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)에서 영역 계층의 예를 볼 수 있습니다.

### <a name="unitlabel-layer"></a>UnitLabel 레이어

각 수준의 DWG 파일에는 사업부 레이블 계층이 포함 될 수 있습니다. Unit 레이블 계층은 Unit 계층에서 추출 된 단위에 이름 속성을 추가 합니다. 이름 속성이 있는 단위에는 매니페스트 파일에 지정된 추가 세부 정보가 있을 수 있습니다.

* 단위 레이블은 한 줄 텍스트 엔터티여야 합니다.
* 단위 레이블은 해당 단위의 범위 내에 있어야 합니다.
* 단위는 Unit 레이블 계층에 여러 텍스트 엔터티를 포함 해서는 안 됩니다.

[샘플 드로잉 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)에서 단위 레이블 계층의 예를 볼 수 있습니다.

### <a name="zonelabel-layer"></a>ZoneLabel 레이어

각 수준의 DWG 파일에는 ZoneLabel 계층이 포함 될 수 있습니다. 이 레이어는 이름 속성을 영역 레이어에서 추출된 영역에 추가합니다. 이름 속성이 있는 영역에는 매니페스트 파일에 지정된 추가 세부 정보가 있을 수 있습니다.

* 영역 레이블은 한 줄 텍스트 엔터티여야 합니다.
* 영역 레이블은 해당 영역의 범위 내에 있어야 합니다.
* 영역에는 ZoneLabel 계층의 여러 텍스트 엔터티가 포함 되지 않아야 합니다.

[샘플 드로잉 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)에서 ZoneLabel 계층의 예를 볼 수 있습니다.

## <a name="manifest-file-requirements"></a>매니페스트 파일 요구 사항

zip 폴더는 디렉터리의 루트 수준에 있는 매니페스트 파일을 포함해야 하며, 파일 이름은 **manifest.json** 이어야 합니다. 이는 [Azure Maps Conversion 서비스](/rest/api/maps/conversion)에서 콘텐츠를 구문 분석할 수 있도록 하는 DWG 파일에 대해 설명합니다. 매니페스트에서 식별 된 파일만 수집 됩니다. Zip 폴더에 있지만 매니페스트에 제대로 나열 되지 않은 파일은 무시 됩니다.

매니페스트 파일의 개체에 있는 파일 경로는 `buildingLevels` zip 폴더의 루트에 대 한 상대 경로 여야 합니다. DWG 파일 이름은 시설 수준의 이름과 정확히 일치해야 합니다. 예를 들어 "지하실" 수준의 DWG 파일은 "지하실"입니다. 수준 2에 대 한 DWG 파일의 이름은 "level_2. DWG"로 지정 됩니다. 수준 이름에 공백이 있는 경우 밑줄을 사용합니다.

매니페스트 개체를 사용 하는 경우에는 요구 사항이 있지만 일부 개체는 필요 하지 않습니다. 다음 표에서는 [Azure Maps 변환 서비스](/rest/api/maps/conversion)버전 1.1에 대 한 필수 및 선택적 개체를 보여 줍니다.

| Object | 필수 | Description |
| :----- | :------- | :------- |
| `version` | true |매니페스트 스키마 버전입니다. 현재 버전 1.1만 지원 됩니다.|
| `directoryInfo` | true | 시설 지리적 정보 및 연락처 정보를 간략히 설명합니다. 또한 거주자 지리적 정보 및 연락처 정보를 설명하는 데에도 사용할 수 있습니다. |
| `buildingLevels` | true | 건물 수준 및 수준 디자인이 포함된 파일 수준을 지정합니다. |
| `georeference` | true | 시설 그리기에 대한 숫자 지리적 정보를 포함합니다. |
| `dwgLayers` | true | 레이어의 이름을 나열하고, 각 레이어에는 자체 기능의 이름이 나열됩니다. |
| `unitProperties` | false | 단위 기능에 대한 추가 메타데이터를 삽입하는 데 사용할 수 있습니다. |
| `zoneProperties` | false | 영역 기능에 대한 추가 메타데이터를 삽입하는 데 사용할 수 있습니다. |

다음 섹션에서는 각 개체의 요구 사항에 대해 자세히 설명합니다.

### `directoryInfo`

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
| `name`      | 문자열 | true   |  건물의 이름입니다. |
| `streetAddress`|    문자열 |    false    | 건물의 주소입니다. |
|`unit`     | 문자열    |  false    |  건물의 단위입니다. |
| `locality` |    문자열 |    false |    영역, 인접 영역 또는 지역의 이름입니다. 예를 들어 "호반" 또는 "중심 지구"가 있습니다. 구/군/시는 우편 주소의 일부가 아닙니다. |
| `adminDivisions` |    문자열의 JSON 배열 |    false     | (국가, 주, 시) 또는 (국가, 도/현, 시, 읍) 주소 명칭을 포함하는 배열입니다. ISO 3166 국가 코드 및 ISO 3166-2 주/지역 코드를 사용합니다. |
| `postalCode` |    문자열    | false    | 메일 분류 코드입니다. |
| `hoursOfOperation` |    문자열 |     false | [OSM Opening Hours](https://wiki.openstreetmap.org/wiki/Key:opening_hours/specification) 형식을 따릅니다. |
| `phone`    | 문자열 |    false |    건물과 연결된 전화 번호입니다. 국가 코드를 포함해야 합니다. |
| `website`    | 문자열 |    false    | 건물과 연결된 웹 사이트입니다. Http 또는 https로 시작 해야 합니다. |
| `nonPublic` |    bool    | false | 건물이 대중에게 공개되는지 여부를 지정하는 플래그입니다. |
| `anchorLatitude` | numeric |    false | 시설 앵커(압정)의 위도입니다. |
| `anchorLongitude` | numeric |    false | 시설 앵커(압정)의 경도입니다. |
| `anchorHeightAboveSeaLevel`  | numeric | false | 시설의 1층 높이입니다(해수면 기준, 미터 단위). |
| `defaultLevelVerticalExtent` | numeric | false | 수준의 `verticalExtent`가 정의되지 않은 경우 사용할 이 시설 수준의 기본 높이(두께)입니다. |

### `buildingLevels`

`buildingLevels` 개체에는 건물 수준의 JSON 배열이 포함됩니다.

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
|`levelName`    |문자열    |true |    설명이 포함된 수준 이름입니다. 예: 층 1, 로비, Blue 주차장 또는 지하실.|
|`ordinal` | integer |    true | 수준 세로 순서를 결정 합니다. 모든 시설에는 서수가 0인 수준이 있어야 합니다. |
|`heightAboveFacilityAnchor` | numeric | false |    앵커의 수준 높이 (미터)입니다. |
| `verticalExtent` | numeric | false | 미터 단위의 상한 높이 (두께)입니다. |
|`filename` |    문자열 |    true |    건물 수준에 대한 CAD 그리기의 파일 시스템 경로입니다. 건물의 zip 파일의 루트에 대한 상대 경로여야 합니다. |

### `georeference`

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
|`lat`    | numeric |    true |    시설 그리기의 원점에 대한 위도의 10진수 표현입니다. 원점 좌표는 WGS84 Web Mercator(`EPSG:3857`)에 있어야 합니다.|
|`lon`    |numeric|    true|    시설 그리기의 원점에 대한 경도의 10진수 표현입니다. 원점 좌표는 WGS84 Web Mercator(`EPSG:3857`)에 있어야 합니다. |
|`angle`|    numeric|    true|   진정한 북쪽 및 드로잉의 세로 (Y) 축 사이의 시계 방향 각도 (도)입니다.   |

### `dwgLayers`

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
|`exterior`    |문자열 배열|    true|    외부 빌딩 프로필을 정의 하는 계층의 이름입니다.|
|`unit`|    문자열 배열|    true|    단위를 정의 하는 레이어의 이름입니다.|
|`wall`|    문자열 배열    |false|    벽을 정의 하는 레이어의 이름입니다.|
|`door`    |문자열 배열|    false   | 문을 정의 하는 계층의 이름입니다.|
|`unitLabel`    |문자열 배열|    false    |단위의 이름을 정의 하는 레이어의 이름입니다.|
|`zone` | 문자열 배열    | false    | 영역을 정의 하는 계층의 이름입니다.|
|`zoneLabel` | 문자열 배열 |     false |    영역 이름을 정의 하는 레이어의 이름입니다.|

### `unitProperties`

`unitProperties` 개체에는 단위 속성의 JSON 배열이 포함됩니다.

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
|`unitName`    |문자열    |true    |이 `unitProperty` 레코드와 연결할 단위의 이름입니다. 이 레코드는 계층에서 레이블 일치를 찾은 경우에만 유효 합니다 `unitName` `unitLabel` . |
|`categoryName`|    문자열|    false    |범주 이름입니다. 전체 범주 목록은 [범주](https://aka.ms/pa-indoor-spacecategories)를 참조하세요. |
|`navigableBy`| 문자열 배열 |    false    |단위를 트래버스할 수 있는 탐색 에이전트의 유형을 나타냅니다. 이 속성은 wayfinding 기능을 알립니다. 허용 되는 값은,,,,,,,,,, `pedestrian` `wheelchair` `machine` `bicycle` `automobile` `hiredAuto` `bus` `railcar` `emergency` `ferry` `boat` 및 `disallowed` 입니다.|
|`routeThroughBehavior`|    문자열|    false    |단위에 대한 경로 통과 동작입니다. 허용되는 값은 `disallowed`, `allowed` 및 `preferred`입니다. 기본값은 `allowed`입니다.|
|`occupants`    |System.io.directoryinfo 개체의 배열 |false    |단위의 거주자 목록입니다. |
|`nameAlt`|    문자열|    false|    단위의 대체 이름입니다. |
|`nameSubtitle`|    문자열    |false|    단위의 부제목입니다. |
|`addressRoomNumber`|    문자열|    false|    단위의 방, 유닛, 아파트 또는 도구 모음 번호입니다.|
|`verticalPenetrationCategory`|    문자열|    false| 이 속성이 정의 된 경우 생성 되는 기능은 단위가 아닌 수직 침투 (VRT)입니다. Vrt를 사용 하 여 상위 또는 그 아래의 수준에서 다른 VRT 기능으로 이동할 수 있습니다. 수직 침투는 [범주](https://aka.ms/pa-indoor-spacecategories) 이름입니다. 이 속성이 정의 된 경우 `categoryName` 속성이로 재정의 됩니다 `verticalPenetrationCategory` . |
|`verticalPenetrationDirection`|    문자열|    false    |`verticalPenetrationCategory`가 정의되면 필요에 따라 유효한 이동 방향을 정의합니다. 허용 되는 값은 `lowToHigh` , `highToLow` , `both` 및 `closed` 입니다. 기본값은 `both`입니다.|
| `nonPublic` | bool | false | 단위가 대중에게 공개되는지 여부를 나타냅니다. |
| `isRoutable` | bool | false | 이 속성이로 설정 된 경우 `false` 또는 단위를 통해 이동할 수 없습니다. 기본값은 `true`입니다. |
| `isOpenArea` | bool | false | 단위에 연결 된 열을 요구 하지 않고 탐색 에이전트가 단위를 입력할 수 있습니다. 기본적으로이 값은 여가 `true` 없는 단위에 대해로 설정 되 고, 여는 단위에 대해로 설정 됩니다 `false` . `isOpenArea`을 (를) 시작 `false` 하지 않고 단위에서 수동으로로 설정 하면 경고가 발생 합니다. 그 이유는 탐색 에이전트가 결과 단위에 연결할 수 없기 때문입니다.|

### `zoneProperties`

`zoneProperties` 개체에는 영역 속성의 JSON 배열이 포함됩니다.

| 속성  | Type | 필수 | Description |
|-----------|------|----------|-------------|
|zoneName        |문자열    |true    |`zoneProperty` 레코드와 연결할 영역의 이름입니다. `zoneName`과 일치하는 레이블이 영역의 `zoneLabel` 레이어에 있는 경우에만 이 레코드가 유효합니다.  |
|categoryName|    문자열|    false    |범주 이름입니다. 전체 범주 목록은 [범주](https://aka.ms/pa-indoor-spacecategories)를 참조하세요. |
|zoneNameAlt|    문자열|    false    |영역의 대체 이름입니다.  |
|zoneNameSubtitle|    문자열 |    false    |영역의 부제목입니다. |
|zoneSetId|    문자열 |    false    | 그룹으로 쿼리하거나 선택할 수 있도록 여러 영역 간에 관계를 설정 하려면 ID를 설정 합니다. 예를 들어 여러 수준에 걸쳐 있는 영역이 있습니다. |

### <a name="sample-drawing-package-manifest"></a>그리기 패키지 매니페스트 샘플

다음은 샘플 드로잉 패키지에 대 한 샘플 매니페스트 파일입니다. 전체 패키지를 다운로드 하려면 [샘플 그리기 패키지](https://github.com/Azure-Samples/am-creator-indoor-data-examples)를 참조 하세요.

#### <a name="manifest-file"></a>매니페스트 파일

```JSON
{
    "version": "1.1", 
    "directoryInfo": { 
        "name": "Contoso Building", 
        "streetAddress": "Contoso Way", 
        "unit": "1", 
        "locality": "Contoso eastside", 
        "postalCode": "98052", 
        "adminDivisions": [ 
            "Contoso city", 
            "Contoso state", 
            "Contoso country" 
        ], 
        "hoursOfOperation": "Mo-Fr 08:00-17:00 open", 
        "phone": "1 (425) 555-1234", 
        "website": "www.contoso.com", 
        "nonPublic": false, 
        "anchorLatitude": 47.636152, 
        "anchorLongitude": -122.132600, 
        "anchorHeightAboveSeaLevel": 1000, 
        "defaultLevelVerticalExtent": 3  
    }, 
    "buildingLevels": { 
        "levels": [ 
            { 
                "levelName": "Basement", 
                "ordinal": -1, 
                "filename": "./Basement.dwg" 
            }, { 
                "levelName": "Ground", 
                "ordinal": 0, 
                "verticalExtent": 5, 
                "filename": "./Ground.dwg" 
            }, { 
                "levelName": "Level 2", 
                "ordinal": 1, 
                "heightAboveFacilityAnchor": 3.5, 
                "filename": "./Level_2.dwg" 
            } 
        ] 
    }, 
    "georeference": { 
        "lat": 47.636152, 
        "lon": -122.132600, 
        "angle": 0 
    }, 
    "dwgLayers": { 
        "exterior": [ 
            "OUTLINE", "WINDOWS" 
        ], 
        "unit": [ 
            "UNITS" 
        ], 
        "wall": [ 
            "WALLS" 
        ], 
        "door": [ 
            "DOORS" 
        ], 
        "unitLabel": [ 
            "UNITLABELS" 
        ], 
        "zone": [ 
            "ZONES" 
        ], 
        "zoneLabel": [ 
            "ZONELABELS" 
        ] 
    }, 
    "unitProperties": [ 
        { 
            "unitName": "B01", 
            "categoryName": "room.office", 
            "navigableBy": ["pedestrian", "wheelchair", "machine"], 
            "routeThroughBehavior": "disallowed", 
            "occupants": [ 
                { 
                    "name": "Joe's Office", 
                    "phone": "1 (425) 555-1234" 
                } 
            ], 
            "nameAlt": "Basement01", 
            "nameSubtitle": "01", 
            "addressRoomNumber": "B01", 
            "nonPublic": true, 
            "isRoutable": true, 
            "isOpenArea": true 
        }, 
        { 
            "unitName": "B02" 
        }, 
        { 
            "unitName": "B05", 
            "categoryName": "room.office" 
        }, 
        { 
            "unitName": "STRB01", 
            "verticalPenetrationCategory": "verticalPenetration.stairs", 
            "verticalPenetrationDirection": "both" 
        }, 
        { 
            "unitName": "ELVB01", 
            "verticalPenetrationCategory": "verticalPenetration.elevator", 
            "verticalPenetrationDirection": "high_to_low" 
        } 
    ], 
    "zoneProperties": 
    [ 
        { 
            "zoneName": "WifiB01", 
            "categoryName": "Zone", 
            "zoneNameAlt": "MyZone", 
            "zoneNameSubtitle": "Wifi", 
            "zoneSetId": "1234" 
        }, 
        { 
            "zoneName": "Wifi101",
            "categoryName": "Zone",
            "zoneNameAlt": "MyZone",
            "zoneNameSubtitle": "Wifi",
            "zoneSetId": "1234"
        }
    ]
}
```

## <a name="next-steps"></a>다음 단계

그리기 패키지가 요구 사항을 충족 하는 경우 [Azure Maps 변환 서비스](/rest/api/maps/conversion) 를 사용 하 여 패키지를 맵 데이터 집합으로 변환할 수 있습니다. 그런 다음, 데이터 집합을 사용 하 여 실내 지도 모듈을 통해 실내 지도를 생성할 수 있습니다.

> [!div class="nextstepaction"]
>[실내 지도의 작성자 (미리 보기)](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [자습서: 작성자 (미리 보기) 실내 지도 만들기](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [실내 지도 동적 스타일 지정](indoor-map-dynamic-styling.md)