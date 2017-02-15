---
title: "DocumentDB 디자인 패턴: 소셜 미디어 앱 | Microsoft Docs"
description: "DocumentDB 및 기타 Azure 서비스의 저장소 유연성을 활용하여 소셜 네트워크에 대한 디자인 패턴을 알아봅니다."
keywords: "소셜 미디어 앱"
services: documentdb
author: ealsur
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 2dbf83a7-512a-4993-bf1b-ea7d72e095d9
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: d9f6c8c73cb7803547053ec495812f993eb44c43
ms.openlocfilehash: b2f8683be1dea938cba84766efe32287eeebb712


---
# <a name="going-social-with-documentdb"></a>DocumentDB를 사용하여 소셜 네트워크 디자인
광범위하게 상호 연결된 사회에서 살고 있다는 것은 삶의 어느 시점에서 **소셜 네트워크**의 일부가 된다는 것을 의미합니다. 우리는 소셜 네트워크를 사용하여 친구, 동료, 가족 등과 연락하거나, 때로는 공통의 관심사를 가진 사람들과 열정을 공유합니다.

엔지니어 또는 개발자로서 우리는 이러한 네트워크가 데이터를 저장하고 상호 연결하는 방법 또는 특정 틈새 시장을 위한 새로운 소셜 네트워크를 만들거나 설계하는 데 활용되어 왔을 수 있는 방법이 궁금할 수 있습니다. 특히 이 모든 데이터가 어떻게 저장되는지가 매우 궁금할 때 더욱 그렇습니다.

사용자가 사진, 동영상 또는 음악과 같은 관련 미디어와 함께 문서를 게시할 수 있는 새롭고 참신한 소셜 네트워크를 만든다고 가정해 보겠습니다. 사용자는 게시물에 의견을 달고 평점을 매길 수 있습니다. 기본 웹 사이트 방문 페이지에는 사용자가 보고 상호 작용할 수 있는 게시물 피드가 있을 것입니다. 처음에는 실제로 복잡하게 들리지 않겠지만 간단한 설명을 위해 이에 대한 내용은 생략하겠습니다(관계별 영향을 받는 사용자 지정 사용자 피드를 자세히 살펴볼 수 있지만 이는 이 문서의 목표를 벗어남).

그렇다면 데이터는 어디에 어떻게 저장될까요?

여러분 대다수는 SQL 데이터베이스에 대한 경험이 있거나 적어도 [데이터의 관계형 모델링](https://en.wikipedia.org/wiki/Relational_model) 에 대한 개념을 알고 있을 것이므로 다음과 같은 다이어그램을 그리기 시작할 수 있을 것입니다.

![상대 관계형 모델을 보여 주는 다이어그램](./media/documentdb-social-media-apps/social-media-apps-sql.png) 

완전히 정규화되고 세련된 데이터 구조는... 확장되지 않습니다. 

저에 대해 오해하지 마세요. 저는 SQL 데이터베이스를 평생 사용해 왔으며, SQL 데이터베이스는 정말 대단하지만 모든 패턴, 방식 및 소프트웨어 플랫폼과 마찬가지로 모든 시나리오에 완벽한 것은 아닙니다.

이 시나리오에서 SQL이 최선의 선택이 아닌 이유는 무엇일까요? 단일 게시물의 구조를 살펴봅시다. 웹사이트나 응용 프로그램에 이 게시물을 나타내려면 테이블 조인 8개(!)로 쿼리를 수행해야 합니다. 단지 게시물 하나를 나타내기 위해서 말입니다. 이제 동적으로 로드되어 화면에 나타나는 게시물 스트림을 그려보면 어디를 향하고 있는지 볼 수 있습니다.

물론 이 많은 조인으로 수천 개의 쿼리를 해결할 정도의 방대한 SQL 인스턴스를 사용하여 콘텐츠를 제공할 수 있지만 보다 간단한 솔루션이 있는 데 그래야 하는 이유가 있을까요?

## <a name="the-nosql-road"></a>NoSQL
[Azure](http://neo4j.com/developer/guide-cloud-deployment/#_windows_azure) 에서 실행할 수 있지만 저렴하지 않고 IaaS 서비스(Infrastructure-as-a-Service, 주로 가상 컴퓨터) 및 유지 관리가 필요한 특수 그래프 데이터베이스가 있습니다. 이 문서의 목적은 대부분의 시나리오에 적합하고 Azure의 NoSQL 데이터베이스 [DocumentDB](https://azure.microsoft.com/services/documentdb/)에서 실행되는 보다 저렴한 솔루션을 소개하는 것입니다. [NoSQL](https://en.wikipedia.org/wiki/NoSQL) 접근 방식을 사용하여 데이터를 JSON 형식으로 저장하고 [역정규화](https://en.wikipedia.org/wiki/Denormalization)를 적용하면 이전의 복잡한 게시물을 단일 [문서](https://en.wikipedia.org/wiki/Document-oriented_database)로 변환할 수 있습니다.

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

또한 조인 없이 단일 쿼리로 이를 실현할 수 있습니다. 이는 훨씬 간단하고 저렴하며, 보다 적은 리소스로 더 나은 결과를 얻을 수 있는 방법입니다.

Azure DocumentDB는 모든 속성이 자체 [자동 인덱싱](documentdb-indexing.md)을 통해 인덱싱되도록 하며, 이를 [사용자 지정](documentdb-indexing-policies.md)할 수도 있습니다. 스키마 없는 접근 방식을 통해 다양한 동적 구조로 문서를 저장할 수 있으며, 향후에는 게시물과 관련된 해시 태그 또는 범주 목록을 함께 유지할 수 있을 것입니다. DocumentDB는 추가 작업 없이 새 문서를 추가된 특성과 함께 처리합니다.

부모 속성을 사용하여 게시물에 대한 의견을 다른 게시물로 처리할 수 있습니다. 이는 개체 매핑을 간소화합니다. 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

또한 모든 소셜 상호 작용을 별도의 개체에 카운터로 저장할 수 있습니다.

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

피드를 만들려면 주어진 연관성 순으로 게시물 id 목록을 유지할 수 있는 문서를 만들기만 하면 됩니다.

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

만든 날짜 순으로 게시물이 정렬된 “최신” 스트림을 유지하거나, 지난 24시간 이내에 좋아요가 많이 추가된 순으로 게시물이 정렬된 “인기” 스트림을 유지할 수 있습니다. 또한 팔로워와 관심사 같은 논리에 따라 각 사용자에 대한 사용자 지정 스트림을 구현할 수도 있으며, 이는 여전히 게시물 목록으로 유지됩니다. 이러한 목록을 빌드하는 방법이 중요하지만 읽기 성능이 그대로 유지되어야 합니다. 이러한 목록 중 하나를 가져온 후에는 여러 페이지의 게시물을 한 번에 가져오기 위해 [IN 연산자](documentdb-sql-query.md#where-clause)를 사용하여 DocumentDB에 대한 단일 쿼리를 실행합니다.

피드 스트림은 [Azure App Service](https://azure.microsoft.com/services/app-service/)의 백그라운드 프로세스인 [Webjobs](../app-service-web/web-sites-create-web-jobs.md)를 사용하여 빌드할 수 있습니다. 게시물을 만든 후 [Azure Webjobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)를 통해 트리거되고 고유한 사용자 지정 논리를 기반으로 스트림 내에 게시물 전파를 구현하는 [Azure Storage](https://azure.microsoft.com/services/storage/), [큐](../storage/storage-dotnet-how-to-use-queues.md) 및 Webjobs를 사용하여 백그라운드 처리를 트리거할 수 있습니다. 

이 동일한 기술을 사용하여 궁극적으로 일관된 환경을 만들어 게시물에 대한 평점 및 좋아요를 지연된 방식으로 처리할 수 있습니다.

팔로워는 더 복잡합니다. DocumentDB에는 최대 문서 크기 제한이 있으며 크기가 큰 문서의 읽기/쓰기는 응용 프로그램의 확장성에 영향을 줄 수 있습니다. 다음 구조를 사용하여 팔로워를 저장하는 방법을 고려할 수 있습니다.

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

이 방식은 수천 명의 팔로워를 지닌 사용자에게 적합하지만 일부 유명인이 랭크에 조인하면 결국 이 접근 방식은 큰 문서에 도달해 문서 크기 용량이 부족하게 될 수 있습니다.

이 문제를 해결하기 위해 혼합된 접근 방식을 사용할 수 있습니다. 사용자 통계 문서의 일부로 팔로워의 수를 저장할 수 있습니다.

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

또한 팔로워의 실제 그래프는 간단한 "A 다음에 B"를 저장하고 검색하도록 허용하는 [확장](https://github.com/richorama/AzureStorageExtensions#azuregraphstore) 을 사용하여 Azure 저장소 테이블에 저장할 수 있습니다. 이렇게 Azure 저장소 테이블에 대한 정확한 팔로워 목록(필요한 경우)을 검색하는 프로세스를 삭제할 수 있지만 단축 번호 조회의 경우 DocumentDB를 계속 사용합니다.

## <a name="the-ladder-pattern-and-data-duplication"></a>"사다리" 패턴 및 데이터 중복
게시물을 참조하는 JSON 문서에서 볼 수 있듯이 하나의 사용자가 여러 번 발생합니다. 이는 이러한 역정규화가 주어진 경우 사용자를 나태는 정보가 여러 곳에 표시될 수 있음을 의미합니다.

데이터 중복이 발생하도록 둔 것은 더 빠른 쿼리를 허용하기 위해서입니다. 그 부작용으로 인한 문제는 일부 작업으로 인해 사용자의 데이터가 변경된 경우 해당 사용자가 지금까지 수행한 모든 활동을 찾아서 모두 업데이트해야 한다는 점입니다. 그다지 실용적으로 들리지 않죠, 그렇죠?

그래프 데이터베이스는 이 문제를 고유한 방식으로 해결합니다. 각 활동에 대해 응용 프로그램에 표시하는 사용자의 주요 특성을 식별하여 문제를 해결할 수 있습니다. 게시물을 응용 프로그램에 시각적으로 표시하고 만든 사람의 이름과 사진만 표시했을 뿐인데 "createdBy" 특성에 해당 사용자의 모든 데이터가 저장되는 이유는 무엇일까요? 각 의견에 대해 사용자의 사진만 표시하면 나머지 정보는 필요 없습니다. 바로 여기에 "사다리 패턴"이 적용됩니다.

다음 사용자 정보를 예로 들어 보겠습니다.

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

이 정보를 보면 중요한 정보와 중요하지 않은 정보를 신속하게 감지할 수 있으므로 다음과 같은 “사다리”가 만들어집니다.

![사다리 패턴의 다이어그램](./media/documentdb-social-media-apps/social-media-apps-ladder.png)

가장 작은 단계는 사용자를 식별하는 정보의 최소 조각인 UserChunk로서, 데이터 중복에 사용됩니다. 중복된 데이터의 크기를 “표시”할 정보로만 줄이면 대량 업데이트의 가능성이 줄어듭니다.

중간 단계는 user입니다. 이는 DocumentDB에 대한 대부분의 성능 종속 쿼리에 사용되고 가장 자주 액세스되며 중요한 전체 데이터입니다. 여기에는 UserChunk가 나타내는 정보가 포함됩니다.

가장 큰 단계는 Extended User입니다. 여기에는 중요한 모든 사용자 정보와 실제로 빠르게 읽을 필요가 없으며 마지막에 사용되는(예: 로그인 프로세스) 기타 데이터가 포함됩니다. 이 데이터를 DocumentDB 외부, Azure SQL 데이터베이스 또는 Azure 저장소 테이블에 저장할 수 있습니다.

사용자를 분할하고 심지어 이 정보를 여러 곳에 저장하는 이유는 무엇일까요? DocumentDB의 저장소 공간은 [무한하지 않으며](documentdb-limits.md) 성능 면에서 문서가 클수록 쿼리 비용이 많이 들기 때문입니다. 소셜 네트워크에 대한 모든 성능 종속 쿼리를 수행하는 데 적합한 정보로 문서를 간소하게 유지하고 전체 프로필 편집, 로그인, 사용량 분석 및 빅 데이터 이니셔티브를 위한 데이터 마이닝 등 최종적인 시나리오에 대한 기타 추가 정보를 저장하세요. 사용자 환경이 빠르고 간소하게 유지된다면 데이터 마이닝을 위한 데이터 수집이 Azure SQL 데이터베이스에서 실행되기 때문에 느려지는 것은 중요하지 않습니다. 사용자는 DocumentDB에 다음과 같이 저장됩니다.

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

그리고 게시물은 다음과 유사합니다.

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

청크의 특성 중 하나가 영향을 받는 곳에서 편집 작업이 수행된 경우 인덱싱된 특성(SELECT * FROM posts p WHERE p.createdBy.id == “edited_user_id”)을 가리키는 쿼리를 사용한 후 청크를 업데이트하여 영향을 받는 문서를 쉽게 찾을 수 있습니다.

## <a name="the-search-box"></a>검색 상자
다행히 사용자는 많은 콘텐츠를 생성합니다. 우리는 콘텐츠 스트림에 없을 수 있는 콘텐츠를 검색하고 찾을 수 있는 기능을 제공할 수 있어야 합니다. 만든 사람을 추적하지 않거나 6개월 전에 게시한 오래된 게시물을 찾으려고 할 수 있기 때문입니다.

다행히 Azure DocumentDB를 사용하기 때문에 단일 코드 줄을 입력하지 않고도 [Azure 검색](https://azure.microsoft.com/services/search/) 을 통해 몇 분 이내에 검색 엔진을 쉽게 구현할 수 있습니다(검색 프로세스 및 UI가 아님).

이 작업이 이렇게 쉬운 이유는 무엇일까요?

Azure Search는 데이터 리포지토리에 후크되는 백그라운드 프로세스인 [인덱서](https://msdn.microsoft.com/library/azure/dn946891.aspx)를 호출하여 인덱스에서 개체를 자동으로 추가, 업데이트 또는 제거하는 기능을 구현하기 때문입니다. Azure Search는 [Azure SQL Database 인덱서](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure Blob 인덱서](../search/search-howto-indexing-azure-blob-storage.md) 그리고 다행이도 [Azure DocumentDB 인덱서](documentdb-search-indexer.md)를 지원합니다. DocumentDB에서 Azure Search로 정보를 전환하는 것은 간단합니다. 둘 다 정보를 JSON 형식으로 저장하기 때문에 [인덱스를 만들고](../search/search-create-index-portal.md) 문서에서 인덱싱할 특성을 매핑하기만 하면 됩니다. 그러면 몇 분(데이터 크기에 따라 다름) 이내에 클라우드 인프라에서 제공되는 최고의 SaaS(Search-as-a-Service) 솔루션을 통해 모든 콘텐츠를 검색할 수 있게 됩니다. 

Azure 검색에 대한 자세한 내용은 [Hitchhiker의 검색 가이드](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/)를 참조하세요.

## <a name="the-underlying-knowledge"></a>기본 지식
매일 증가하는 이 모든 콘텐츠를 저장한 후에는 이 모든 사용자 정보 스트림으로 수행할 수 있는 작업이 무엇인지 궁금할 수 있습니다.

대답은 간단합니다. 사용할 수 있도록 구성한 후 학습하는 것입니다.

그렇다면 무엇을 배울 수 있을까요? 몇 가지 쉬운 예를 들면 [정서 분석](https://en.wikipedia.org/wiki/Sentiment_analysis), 사용자의 선호도에 따른 콘텐츠 추천 또는 소셜 네트워크에서 게시된 모든 콘텐츠가 가족에게 안전하도록 보장하는 자동화된 콘텐츠 중재자 등이 있습니다.

이제 간단한 데이터베이스 및 파일에서 이러한 패턴과 정보를 추출하려면 수학 박사가 필요하다고 생각하겠지만 그렇지 않습니다.

[Cortana Intelligence 제품군](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx)의 일부인 [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/)은 완전히 관리되는 클라우드 서비스로서, 간단한 끌어서 놓기 인터페이스에서 알고리즘을 사용하여 워크플로를 만들거나, [R](https://en.wikipedia.org/wiki/R_\(programming_language\))에서 사용자 고유의 알고리즘을 코딩하거나, 이미 빌드되고 즉시 사용 가능한 API(예: [텍스트 분석](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [Content Moderator](https://www.microsoft.com/moderator) 또는 [추천](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2))를 사용할 수 있도록 해줍니다.

이러한 Machine Learning 시나리오를 달성하려면 [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/)를 사용하여 다양 한 원본에서 정보를 수집하고 [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/)을 사용하여 정보를 처리하고 Azure Machine Learning에서 처리할 수 있는 출력을 생성할 수 있습니다.

또 다른 사용 가능한 옵션은 [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services)를 사용하여 사용자의 콘텐츠를 분석하는 것입니다. 이를 통해 보다 잘 이해할 수 있을 뿐만 아니라([Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)로 작성한 것을 분석하여) 원치 않거나 성숙한 콘텐츠를 검색하고 [Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api)를 사용하여 그에 따라 동작할 수 있습니다. Cognitive Services는 사용하기 위해 Machine Learning의 지식이 필요하지 않은 많은 기본 제공 솔루션을 포함합니다.

## <a name="conclusion"></a>결론
이 문서에서는 완전히 Azure에서 저렴한 서비스를 사용하여 소셜 네트워크를 만들고, 다중 계층 저장소 솔루션 및 “사다리”라는 데이터 분산을 사용하도록 권장하여 뛰어난 결과를 제공하는 대안에 대해 살펴보았습니다.

![소셜 네트워킹에 대한 Azure 서비스 간 상호 작용의 다이어그램](./media/documentdb-social-media-apps/social-media-apps-azure-solution.png)

이러한 종류의 시나리오에 대한 묘책은 없습니다. 이는 뛰어난 경험을 구축할 수 있도록 해주는 유용한 서비스의 조합으로 생성되는 시너지입니다. 예를 들어 뛰어난 소셜 응용 프로그램을 제공하는 Azure DocumentDB의 속도와 자유로움, Azure 검색과 같은 최고 수준의 검색 솔루션에 숨은 인텔리전스, 언어에 관계없는 응용 프로그램뿐 아니라 강력한 백그라운드 프로세스를 호스트하는 Azure 앱 서비스의 유연성, 방대한 양의 데이터를 저장하는 확장 가능한 Azure 저장소 및 Azure SQL 데이터베이스, 프로세스에 대한 피드백을 제공할 수 있는 지식과 인텔리전스를 만들고 올바른 사용자에게 올바른 콘텐츠를 제공하도록 도와주는 Azure 기계 학습의 분석 기능 등이 조합된 결과입니다.

## <a name="next-steps"></a>다음 단계
데이터 모델링에 대한 자세한 내용은 [DocumentDB의 데이터 모델링](documentdb-modeling-data.md) 문서를 참조하세요. DocumentDB의 다른 사용 사례에 관심이 있는 경우 [일반적인 DocumentDB 사용 사례](documentdb-use-cases.md)를 참조하세요.

또는 [DocumentDB 학습 경로](https://azure.microsoft.com/documentation/learning-paths/documentdb/)를 수행하여 DocumentDB에 대해 자세히 알아보세요.




<!--HONumber=Dec16_HO2-->


