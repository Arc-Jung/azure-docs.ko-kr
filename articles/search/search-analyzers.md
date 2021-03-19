---
title: 언어 및 텍스트 처리를 위한 분석기
titleSuffix: Azure Cognitive Search
description: 인덱스의 검색 가능한 텍스트 필드에 분석기를 할당하여 사용자 지정, 미리 정의 또는 언어 특정 대안으로 기본 표준 Lucene을 바꿉니다.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/17/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: d40dd0b91f9dcfb7bf5b6e8f084f25ee4f90d780
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104596555"
---
# <a name="analyzers-for-text-processing-in-azure-cognitive-search"></a>Azure Cognitive Search에서 텍스트 처리를 위한 분석기

*분석기* 는 쿼리 문자열과 인덱싱된 문서의 텍스트를 처리하는 [전체 텍스트 검색](search-lucene-query-architecture.md) 구성 요소입니다. 텍스트 처리 (어휘 분석이 라고도 함)는 변화 다음과 같은 동작을 통해 쿼리 문자열을 수정 합니다.

+ 필수적이 지 않은 단어 (중지 단어) 및 문장 부호 제거
+ 구와 하이픈을 넣은 단어를 구성 요소 부분으로 분할
+ 소문자-대문자 단어
+ 저장 효율성을 위해 단어를 기본 루트 형식으로 줄인 후 시제에 관계 없이 일치 항목을 찾을 수 있습니다.

분석 `Edm.String` 은 전체 텍스트 검색을 나타내는 "검색 가능"으로 표시 된 필드에 적용 됩니다. 

이 구성을 사용 하는 필드의 경우에는 토큰을 만든 다음 쿼리를 구문 분석할 때 쿼리를 실행 하는 동안 분석을 수행 하 고 일치 하는 토큰을 검색 합니다. 일치는 동일한 분석기가 인덱싱 및 쿼리에 모두 사용 되는 경우 발생할 가능성이 높습니다. 요구 사항에 따라 각 워크 로드에 대 한 분석기를 독립적으로 설정할 수 있습니다.

필터 또는 유사 항목 검색과 같은 전체 텍스트 검색이 *아닌* 쿼리 형식은 쿼리 쪽의 분석 단계를 거치지 않습니다. 대신, 파서는 일치의 기준으로 제공 하는 패턴을 사용 하 여 해당 문자열을 검색 엔진으로 직접 보냅니다. 일반적으로 이러한 쿼리 형식에는 패턴 일치 작업을 수행 하기 위해 전체 문자열 토큰이 필요 합니다. 인덱싱 중에 전체 용어 토큰을 확인 하려면 [사용자 지정 분석기](index-add-custom-analyzers.md)가 필요할 수 있습니다. 쿼리 용어를 분석 하는 시기와 이유에 대 한 자세한 내용은 [Azure Cognitive Search의 전체 텍스트 검색](search-lucene-query-architecture.md)을 참조 하세요.

어휘 분석에 대 한 자세한 배경 정보는 다음 비디오 클립에서 간략하게 설명 합니다.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=132&end=189]

## <a name="default-analyzer"></a>기본 분석기  

Azure Cognitive Search 쿼리에서는 검색 가능으로 표시 된 모든 문자열 필드에서 분석기가 자동으로 호출 됩니다. 

기본적으로 Azure Cognitive Search는 [Apache Lucene 표준 분석기 (표준 Lucene)](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html)를 사용 합니다 .이 분석기는 ["유니코드 텍스트 조각화"](https://unicode.org/reports/tr29/) 규칙에 따라 텍스트를 요소로 분리 합니다. 또한 표준 분석기에서는 모든 문자를 소문자 형식으로 변환합니다. 인덱싱 및 쿼리 처리 중에는 인덱싱된 문서와 검색 용어 둘 다에 대해 분석을 수행합니다.  

필드별로 기본 분석기를 재정의할 수 있습니다. 대체 분석기는 언어 처리, [사용자 지정 분석기](index-add-custom-analyzers.md)또는 [사용 가능한 분석기 목록](index-add-custom-analyzers.md#built-in-analyzers)에서 제공 하는 기본 제공 분석기에 대 한 [언어 분석기](index-add-language-analyzers.md) 일 수 있습니다.

## <a name="types-of-analyzers"></a>분석기 유형

다음 목록에는 Azure Cognitive Search에서 사용할 수 있는 분석기에 대 한 설명이 나와 있습니다.

| 범주 | 설명 |
|----------|-------------|
| [표준 Lucene 분석기](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | 기본값 사양 또는 구성이 필요하지 않습니다. 이 범용 분석기는 다양 한 언어 및 시나리오에 적합 합니다.|
| 기본 제공 분석기 | 있는 그대로 사용 되며 이름으로 참조 됩니다. 언어와 언어에 관계 없이 두 가지 유형이 있습니다. </br></br>[특수(언어 중립적) 분석기](index-add-custom-analyzers.md#built-in-analyzers)는 텍스트 입력에 특수 처리 또는 최소한의 처리가 필요할 때 사용됩니다. 이 범주에 포함 된 분석기의 예로는 **Asciifolding**, **키워드**, **패턴**, **단순**, **중지**, **공백** 등이 있습니다. </br></br>[언어 분석기](index-add-language-analyzers.md)는 개별 언어에 대해 풍부한 언어 지원이 필요할 때 사용됩니다. Azure Cognitive Search는 35 Lucene 언어 분석기 및 50 Microsoft 자연어 처리 분석기를 지원 합니다. |
|[사용자 지정 분석기](/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | 기존 요소를 결합한 사용자 정의 구성을 말하며, 토크나이저(필수) 하나와 여러 선택적 필터(char 또는 토큰)로 구성됩니다.|

**패턴** 또는 **중지** 와 같은 몇 가지 기본 제공 분석기는 제한 된 구성 옵션 집합을 지원 합니다. 이러한 옵션을 설정 하려면 기본 제공 분석기와 [기본 제공](index-add-custom-analyzers.md#built-in-analyzers)분석기에 설명 된 대체 옵션 중 하나로 구성 된 사용자 지정 분석기를 만듭니다. 다른 사용자 지정 구성과 마찬가지로, Lucene 패턴 분석기와 구분할 수 있도록 구성의 이름(예: *myPatternAnalyzer*)을 입력합니다.

## <a name="how-to-specify-analyzers"></a>분석기를 지정하는 방법

분석기 설정은 선택 사항입니다. 일반적으로 기본 표준 Lucene 분석기를 사용 하 여 수행 방법을 확인 하세요. 쿼리가 예상한 결과를 반환 하지 못하는 경우 다른 분석기로 전환 하는 것이 적합 한 솔루션입니다.

1. [인덱스](/rest/api/searchservice/create-index)에서 필드 정의를 만들 때 "analyzer" 속성을 **키워드** 와 같은 [기본 제공 분석기](index-add-custom-analyzers.md#built-in-analyzers) ,와 같은 [언어 분석기](index-add-language-analyzers.md) `en.microsoft` 또는 동일한 인덱스 스키마에 정의 된 사용자 지정 분석기 중 하나로 설정 합니다.  
 
   ```json
     "fields": [
    {
      "name": "Description",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": "en.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null
    },
   ```

   [언어 분석기](index-add-language-analyzers.md)를 사용 하는 경우 "analyzer" 속성을 사용 하 여 지정 해야 합니다. "SearchAnalyzer" 및 "indexAnalyzer" 속성은 언어 분석기에 적용 되지 않습니다.

1. 또는 각 워크 로드에 대 한 분석기를 변경 하려면 "indexAnalyzer" 및 "searchAnalyzer"를 설정 합니다. 이러한 속성을 함께 설정 하 고 "analyzer" 속성을 대체 합니다 .이 속성은 null 이어야 합니다. 이러한 작업 중 하나에 다른 분석기가 필요 하지 않은 특정 변환이 필요한 경우 인덱싱 및 쿼리에 대해 서로 다른 분석기를 사용할 수 있습니다.

   ```json
     "fields": [
    {
      "name": "ProductGroup",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": null,
      "indexAnalyzer": "keyword",
      "searchAnalyzer": "standard"
    },
   ```

1. 사용자 지정 분석기의 경우 인덱스의 **[분석기]** 섹션에 항목을 만든 다음 앞의 두 단계 중 하나에 따라 사용자 지정 분석기를 필드 정의에 할당 합니다. 자세한 내용은 [인덱스 만들기](/rest/api/searchservice/create-index) 및 [사용자 지정 분석기 추가](index-add-custom-analyzers.md)를 참조하세요.

## <a name="when-to-add-analyzers"></a>분석기를 추가하는 시기

분석기를 추가하고 할당하기에 가장 좋은 시기는 인덱스를 삭제하고 다시 만드는 작업이 일상적으로 진행되는 능동적 개발 단계입니다.

분석기는 용어를 토큰화 하는 데 사용 되므로 필드를 만들 때 분석기를 할당 해야 합니다. 실제로 이미 생성 된 필드에 analyzer 또는 indexAnalyzer를 할당 하는 것은 허용 되지 않습니다 (인덱스에 영향을 주지 않고 언제 든 지 searchAnalyzer 속성을 변경할 수 있음).

기존 필드의 분석기를 변경 하려면 [인덱스를 완전히 다시 작성](search-howto-reindex.md) 해야 합니다. 개별 필드는 다시 작성할 수 없습니다. 프로덕션에 있는 인덱스의 경우 새 분석기 할당을 사용 하 여 새 필드를 만들고 이전에 사용 하기 시작 하 여 다시 빌드를 연기할 수 있습니다. [인덱스 업데이트](/rest/api/searchservice/update-index)를 사용하여 새 필드를 통합하고 [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents)를 사용하여 새 필드를 채웁니다. 나중에 계획된 인덱스 서비스의 일환으로 인덱스를 정리하여 오래된 필드를 제거할 수 있습니다.

기존 인덱스에 새 필드를 추가 하려면 [인덱스 업데이트](/rest/api/searchservice/update-index) 를 호출 하 여 필드를 추가 하 고 [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents) 를 채웁니다.

사용자 지정 분석기를 기존 인덱스에 추가 하려면이 오류를 방지 하려면 [업데이트 인덱스](/rest/api/searchservice/update-index) 에서 "allowIndexDowntime 중지 시간" 플래그를 전달 합니다.

*"인덱스 업데이트는 가동 중지 시간이 발생할 수 있으므로 허용 되지 않습니다. 새 분석기, 토크 나이저, 토큰 필터 또는 문자 필터를 기존 인덱스에 추가 하려면 인덱스 업데이트 요청에서 ' allowIndexDowntime 중지 시간 ' 쿼리 매개 변수를 ' t r u e '로 설정 합니다. 이 작업을 수행 하면 최소한 몇 초 동안 인덱스를 오프 라인 상태로 전환 하므로 인덱싱 및 쿼리 요청이 실패 합니다. 인덱스를 업데이트 한 후 몇 분 동안 또는 매우 큰 인덱스의 경우에는 인덱스의 성능 및 쓰기 가용성이 손상 될 수 있습니다. "*

## <a name="recommendations-for-working-with-analyzers"></a>분석기 사용에 대한 추천

이 섹션에서는 분석기 작동 방식에 대한 조언을 제공합니다.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>특정 요구 사항이 없는 경우 읽기 쓰기에 대해 하나의 분석기

Azure Cognitive Search를 사용 하 여 인덱싱 및 검색에 대해 다른 분석기를 지정 하 고 추가 indexAnalyzer와 searchAnalyzer 필드 속성을 사용할 수 있습니다. 분석기를 지정하지 않으면 analyzer 속성을 통해 설정된 분석기가 인덱싱과 검색에 모두 사용됩니다. 분석기가 지정 되지 않은 경우 기본 표준 Lucene 분석기가 사용 됩니다.

특정 요구 사항이 따로 지정되지 않으면 인덱싱 및 쿼리 모두에 대해 동일한 분석기를 사용하는 것이 일반적인 규칙입니다. 철저하게 테스트해야 합니다. 검색 및 인덱싱 시간에서 텍스트 처리가 다른 경우 검색 및 인덱싱 분석기 구성이 일치하지 않으면 쿼리 용어와 인덱싱된 용어 사이에 불일치가 발생할 위험을 떠안게 됩니다.

### <a name="test-during-active-development"></a>활성 개발 중 테스트

표준 분석기를 재정의하려면 인덱스를 다시 빌드해야 합니다. 가능하면 프로덕션 환경에 인덱스를 이동하기 전에 활성 개발 중 사용할 분석기를 결정합니다.

### <a name="inspect-tokenized-terms"></a>토큰화된 용어 검사

검색에서 예상되는 결과가 반환되지 않은 경우, 쿼리에 입력된 용어와 인덱스의 토큰화된 용어의 토큰이 같지 않기 때문일 가능성이 큽니다. 토큰이 같지 않으면 일치하는 결과가 반환되지 않습니다. 토크나이저 출력을 검사할 때는 조사 도구로 [분석 API](/rest/api/searchservice/test-analyzer)를 사용하는 것이 좋습니다. 응답은 해당 분석기에 의해 생성되는 토큰으로 구성됩니다.

<a name="examples"></a>

## <a name="rest-examples"></a>REST 예제

다음 예제에서는 몇 가지 주요 시나리오에 대한 분석기 정의를 보여줍니다.

+ [사용자 지정 분석기 예제](#Custom-analyzer-example)
+ [필드에 분석기 할당 예제](#Per-field-analyzer-assignment-example)
+ [인덱싱 및 검색에 분석기 혼용](#Mixing-analyzers-for-indexing-and-search-operations)
+ [언어 분석기 예제](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>사용자 지정 분석기 예제

이 예제에서는 사용자 지정 옵션을 사용하는 분석기 정을 보여줍니다. char 필터, 토크나이저 및 토큰 필터에 대한 사용자 지정 옵션은 명명된 구문으로 개별 지정되고 분석기 정의에서 참조됩니다. 미리 정의된 요소도 그대로 사용되며 이름별로 참조하기만 하면 됩니다.

이 예제를 진행합니다.

+ 분석기는 검색 가능한 필드에 대한 필드 클래스의 속성입니다.

+ 사용자 지정 분석기는 인덱스 정의의 일부입니다. 가볍게 사용자 지정(예: 한 필터에서 단일 옵션 사용자 지정)되거나 여러 위치에서 사용자 지정될 수도 있습니다.

+ 이 경우에 사용자 지정 분석기는 "my_analyzer"이며, 사용자 지정된 표준 토크나이저 "my_standard_tokenizer" 및 소문자 필터와 사용자 지정된 asciifolding 필터 "my_asciifolding"라는 두 개의 토큰 필터를 차례로 사용합니다.

+ 또한 2개의 사용자 지정 문자 필터 "map_dash" 및 "remove_whitespace"를 정의합니다. 첫 번째 필터는 모든 대시를 밑줄로 바꾸고 두 번째 필터는 모든 공백을 제거합니다. 공백은 매핑 규칙에서 UTF-8로 인코딩되어야 합니다. 문자 필터는 토큰화 전에 적용되며 결과 토큰에 영향을 미칩니다(표준 토크나이저는 대시 및 공백으로 분리되지만 밑줄은 사용하지 않습니다).

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
```

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>필드별 분석기 할당 예제

표준 분석기는 기본값입니다. 패턴 분석기와 같은 다른 미리 정의된 분석기로 기본값을 바꾸려 한다고 가정합니다. 사용자 지정 옵션을 설정하지 않은 경우 필드 정의에서 이름으로 지정하기만 하면 됩니다.

"analyzer" 요소에서 필드 기준으로 표준 분석기를 재정의합니다. 전역 재정의가 없습니다. 이 예제에서 `text1`는 패턴 분석기를 사용하고 분석기를 지정하지 않는 `text2`는 기본값을 사용합니다.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
```

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>인덱싱 및 검색 작업에 분석기 혼용

API는 인덱싱 및 검색에 대해 다른 분석기를 지정하기 위한 추가 인덱스 특성을 포함합니다. searchAnalyzer 및 indexAnalyzer 특성은 쌍으로 지정해야 하며, 단일 analyzer 특성을 대체합니다.


```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
```

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>언어 분석기 예제

다른 언어의 문자열을 포함하는 필드는 언어 분석기를 사용할 수 있지만 다른 필드는 기본값을 그대로 둡니다(또는 다른 미리 정의되거나 사용자 지정된 분석기 사용). 언어 분석기를 사용하는 경우 인덱싱 및 검색 작업에 사용해야 합니다. 언어 분석기를 사용하는 필드는 인덱싱 및 검색에 다른 분석기를 가질 수 없습니다.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
```

## <a name="c-examples"></a>C # 예제

.NET SDK 코드 샘플을 사용 하는 경우 다음 예제를 추가 하 여 분석기를 사용 하거나 구성할 수 있습니다.

+ [기본 제공 분석기 할당](#Assign-a-language-analyzer)
+ [분석기 구성](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>언어 분석기 할당

구성 없이 있는 그대로 사용 되는 모든 분석기는 필드 정의에 지정 됩니다. 인덱스의 **[분석기]** 섹션에 항목을 만들 필요는 없습니다. 

언어 분석기는 있는 그대로 사용됩니다. 이를 사용 하려면 [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer)를 호출 하 고, Azure Cognitive Search에서 지원 되는 텍스트 분석기를 제공 하는 [LexicalAnalyzerName](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) 유형을 지정 합니다.

사용자 지정 분석기는 필드 정의에 유사 하 게 지정 되지만이 작업을 수행 하려면 다음 섹션에서 설명 하는 것 처럼 인덱스 정의에 분석기를 지정 해야 합니다.

```csharp
    public partial class Hotel
    {
       . . . 
        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
        public string Description { get; set; }

        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
        [JsonPropertyName("Description_fr")]
        public string DescriptionFr { get; set; }

        [SearchableField(AnalyzerName = "url-analyze")]
        public string Url { get; set; }
      . . .
    }
```

<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>사용자 지정 분석기 정의

사용자 지정 또는 구성이 필요한 경우에는 분석기 구문을 인덱스에 추가 합니다. 정의한 후에는 앞의 예제에서 설명한 대로 필드 정의에 추가할 수 있습니다.

[CustomAnalyzer](/dotnet/api/azure.search.documents.indexes.models.customanalyzer) 개체를 만듭니다. 사용자 지정 분석기는 알려진 토크, 0 개 이상의 토큰 필터 및 0 개 이상의 문자 필터 이름으로 이루어진 사용자 정의 조합입니다.

+ [CustomAnalyzer](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer)
+ [CustomAnalyzer 필터](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenfilters)
+ [CustomAnalyzer 필터](/dotnet/api/microsoft.azure.search.models.customanalyzer.charfilters)

다음 예제에서는 [uax_url_email 토크](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer) 및 [소문자 토큰 필터](/dotnet/api/microsoft.azure.search.models.tokenfiltername.lowercase)를 사용 하는 "url-분석" 이라는 사용자 지정 분석기를 만듭니다.

```csharp
private static void CreateIndex(string indexName, SearchIndexClient adminClient)
{
   FieldBuilder fieldBuilder = new FieldBuilder();
   var searchFields = fieldBuilder.Build(typeof(Hotel));

   var analyzer = new CustomAnalyzer("url-analyze", "uax_url_email")
   {
         TokenFilters = { TokenFilterName.Lowercase }
   };

   var definition = new SearchIndex(indexName, searchFields);

   definition.Analyzers.Add(analyzer);

   adminClient.CreateOrUpdateIndex(definition);
}
```

더 많은 예제는 [CustomAnalyzerTests](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Microsoft.Azure.Search/tests/Tests/CustomAnalyzerTests.cs)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

쿼리 실행에 대 한 자세한 설명은 [Azure Cognitive Search의 전체 텍스트 검색](search-lucene-query-architecture.md)에서 찾을 수 있습니다. 이 문서에서는 예제를 사용 하 여 화면에서 직관적으로 보일 수 있는 동작을 설명 합니다.

분석기에 대해 자세히 알아보려면 다음 문서를 참조 하세요.

+ [언어 분석기](index-add-language-analyzers.md)
+ [사용자 지정 분석기](index-add-custom-analyzers.md)
+ [검색 인덱스 만들기](search-what-is-an-index.md)
+ [다중 언어 인덱스 만들기](search-language-support.md)