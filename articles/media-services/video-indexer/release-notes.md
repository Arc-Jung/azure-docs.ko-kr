---
title: Azure Media Services Video Indexer 릴리스 정보 | Microsoft Docs
description: 최신 개발을 최신 상태로 유지 하기 위해이 문서에서는 Azure Media Services Video Indexer 최신 업데이트를 제공 합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 02/16/2021
ms.author: juliako
ms.openlocfilehash: 618617d3602e45ebb15314c7cc5f6898a73bb71f
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203728"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer 릴리스 정보

>이 URL(`https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+Video+Indexer+release+notes%22&locale=en-us`)을 RSS 피드 판독기에 복사하고 붙여넣어 업데이트를 위해 이 페이지를 다시 방문해야 하는 시기에 대한 알림을 받습니다.

최신 개발 정보를 확인할 수 있도록 이 문서에서는 다음과 같은 정보를 제공합니다.

* 최신 릴리스
* 알려진 문제
* 버그 수정
* 사용되지 않는 기능

## <a name="march-2021"></a>2021년 3월

이제 다른 가격대의 새로운 오디오 기능 번들에서 오디오 분석을 사용할 수 있습니다. 새로운 **기본 오디오** 분석 사전 설정은 음성 기록, 번역 및 서식 출력 캡션과 자막만 추출 하는 저렴 한 옵션을 제공 합니다. **기본 오디오** 사전 설정은 기록에 대 한 선과 캡션 및 부제목 서식 지정을 위한 별도의 줄을 포함 하 여 청구서에 두 개의 개별 미터를 생성 합니다. 가격 책정에 대 한 자세한 내용은 [Media Services 가격 책정](https://azure.microsoft.com/pricing/details/media-services/) 페이지를 참조 하세요.

새로 추가 된 번들은   ->  **비디오 + 오디오 인덱싱** 드롭다운 상자 아래에서 고급 옵션 **기본 오디오** 사전 설정을 선택 하 여 파일을 인덱싱 또는 다시 인덱싱할 때 사용할 수 있습니다.

## <a name="february-2021"></a>2021년 2월

### <a name="multiple-account-owners"></a>여러 계정 소유자 

계정 소유자 역할이 Video Indexer에 추가 되었습니다. 사용자를 추가, 변경 및 제거할 수 있습니다. 해당 역할을 변경 합니다. 계정을 공유 하는 방법에 대 한 자세한 내용은 [사용자 초대](invite-users.md)를 참조 하세요.

### <a name="audio-event-detection-public-preview"></a>오디오 이벤트 검색 (공개 미리 보기)

> [!NOTE]
> 이 기능은 평가판 계정 에서만 사용할 수 있습니다. 

Video Indexer는 이제 콘텐츠의 음성이 아닌 세그먼트에서 다음과 같은 오디오 효과를 검색 합니다. gunshot, 투명 분할, 경보, siren, 폭발, dog 짖, 흥미, 웃음,, 반응, 응원, 박수) 및 무음. 

새로 추가 된 오디오에 영향을 주는 기능은 **고급 옵션인**  ->  **고급 오디오** 사전 설정 (비디오 + 오디오 인덱싱 아래)을 선택 하 여 파일을 인덱싱할 때 사용할 수 있습니다. 표준 인덱싱에는 **대기** 중인 **반응** 만 포함 됩니다. 

이전 오디오 효과 모델에 포함 된 **박수** 이벤트 유형은 이제 가장 된 **반응** 이벤트 유형의 일부로 추출 됩니다.

[Video Indexer](https://www.videoindexer.ai/) 웹 사이트에서 비디오에 대 한 **정보** 를 볼 때 오디오 효과가 페이지에 표시 됩니다.

:::image type="content" source="./media/release-notes/audio-detection.png" alt-text="오디오 이벤트 감지":::

### <a name="named-entities-enhancement"></a>명명 된 엔터티 기능 향상  

추출한 사용자 및 위치 목록이 일반적으로 확장 되 고 업데이트 되었습니다. 

또한 모델에는 비디오에서 ' Sam ' 또는 ' Home '과 같이 유명 하지 않은 사람 및 위치가 포함 되어 있습니다. 

## <a name="january-2021"></a>2021년 1월

### <a name="video-indexer-is-deployed-on-us-government-cloud"></a>Video Indexer 미국 정부 클라우드에 배포 됨 

이제 버지니아 및 애리조나 지역에서 미국 정부 클라우드의 Video Indexer 유료 계정을 만들 수 있습니다. Video Indexer 무료 평가판 제품을 언급 된 지역에서 사용할 수 없습니다. 자세한 내용은 Video Indexer 설명서를 참조 하세요. 

### <a name="video-indexer-deployed-in-the-india-central-region"></a>인도 중부 지역에 배포 Video Indexer 

이제 인도 중부 지역에서 Video Indexer 유료 계정을 만들 수 있습니다. 

### <a name="new-dark-mode-for-the-video-indexer-website-experience"></a>Video Indexer 웹 사이트 환경의 새 어둡게 모드

이제는 Video Indexer 웹 사이트 환경을 어둡게 모드로 사용할 수 있습니다. 어둡게 모드를 사용 하도록 설정 하려면 설정 패널을 열고 **어둡게 모드** 옵션을 설정/해제 합니다. 

:::image type="content" source="./media/release-notes/dark-mode.png" alt-text="어두운 모드 설정":::

## <a name="december-2020"></a>2020년 12월

### <a name="video-indexer-deployed-in-the-switzerland-west-and-switzerland-north"></a>스위스 서부 및 스위스 북부에 배포 Video Indexer

이제 스위스 서부 및 스위스 북부 지역에서 Video Indexer 유료 계정을 만들 수 있습니다.

## <a name="october-2020"></a>2020년 10월

### <a name="animated-character-identification-improvements"></a>문자 id의 애니메이션 향상  

Video Indexer은 Cognitive Services 사용자 지정 비전과의 통합을 통해 애니메이션 된 콘텐츠의 문자 검색, 그룹화 및 인식을 지원 합니다. 검색 및 문자 인식에서이 AI 알고리즘에 대해 크게 향상 된 기능을 추가 했습니다. 결과 정보 정확도 및 식별 된 문자가 크게 향상 됩니다.

### <a name="planned-video-indexer-website-authenticatication-changes"></a>계획 된 Video Indexer 웹 사이트 authenticatication 변경 내용

2021 년 3 월 1 일부 터는 더 이상 Facebook 또는 LinkedIn을 사용 하 여 [Video Indexer 웹 사이트](https://www.videoindexer.ai/) [개발자 포털](video-indexer-use-apis.md) 에 등록 하 고 로그인 할 수 없습니다.

Azure AD, Microsoft, Google 등의 공급자 중 하나를 사용 하 여 등록 하 고 로그인 할 수 있습니다.

> [!NOTE]
> LinkedIn 및 Facebook에 연결 된 Video Indexer 계정은 2021 년 3 월 1 일 이후에는 액세스할 수 없습니다. 
> 
> 사용자가 소유 하 고 있는 Azure AD, Microsoft 또는 Google 전자 메일을 Video Indexer 계정에 [초대](invite-users.md) 하 여 여전히 액세스할 수 있도록 해야 합니다. [초대](invite-users.md)의 설명에 따라 지원 되는 공급자의 추가 소유자를 추가할 수 있습니다. <br/>
> 또는 유료 계정을 만들고 데이터를 마이그레이션할 수 있습니다.

## <a name="august-2020"></a>2020년 8월

### <a name="mobile-design-for-the-video-indexer-website"></a>Video Indexer 웹 사이트용 모바일 디자인

이제 Video Indexer 웹 사이트 환경에서 모바일 장치를 지원 합니다. 사용자 환경이 모바일 화면 크기에 맞게 조정 됩니다 (사용자 지정 Ui 제외). 

### <a name="accessibility-improvements-and-bug-fixes"></a>내게 필요한 옵션 개선 사항 및 버그 수정 

WCAG (웹 콘텐츠 접근성 지침)의 일환으로 Video Indexer 웹 사이트 환경은 Microsoft 접근성 표준의 일부로 등급 C에 맞춰 정렬 됩니다. 키보드 탐색, 프로그래밍 방식 액세스 및 화면 판독기와 관련 된 몇 가지 버그와 개선 사항이 해결 되었습니다. 

## <a name="july-2020"></a>2020년 7월

### <a name="ga-for-multi-language-identification"></a>다중 언어 식별을 위한 GA

다중 언어 id는 미리 보기에서 GA로 이동 하 여 생산적으로 사용할 준비가 되었습니다.

"GA로 미리 보기" 전환과 관련 된 가격에는 영향을 주지 않습니다.

### <a name="video-indexer-website-improvements"></a>Video Indexer 웹 사이트 개선 사항

#### <a name="adjustments-in-the-video-gallery"></a>비디오 갤러리의 조정

추가 필터링 기능이 있는 심층 통찰력 검색에 대 한 새 검색 표시줄이 추가 되었습니다. 검색 결과도 향상 되었습니다.

여러 파일을 사용 하 여 비디오 보관 파일을 정렬 및 관리할 수 있는 새 목록 보기입니다.

#### <a name="new-panel-for-easy-selection-and-configuration"></a>간편한 선택 및 구성을 위한 새 패널

간편한 선택 및 사용자 구성에 대 한 보조 패널이 추가 되어 간단 하 고 빠른 계정 만들기 및 공유와 구성을 설정할 수 있습니다.

사이드 패널은 사용자 기본 설정 및 도움말에도 사용 됩니다.

## <a name="june-2020"></a>2020년 6월

### <a name="search-by-topics"></a>항목으로 검색

이제 검색 API를 사용 하 여 특정 토픽으로 비디오를 검색할 수 있습니다 (API에만 해당).

토픽은 `textScope` (선택적 매개 변수)의 일부로 추가 됩니다. 자세한 내용은 [API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Search-Videos) 를 참조 하세요.  

### <a name="labels-enhancement"></a>레이블 기능 향상

레이블 태거가 업그레이드 되었으며 이제 식별할 수 있는 더 많은 시각적 레이블이 포함 되어 있습니다.

## <a name="may-2020"></a>2020년 5월

### <a name="video-indexer-deployed-in-the-east-us"></a>미국 동부에 배포 Video Indexer

이제 미국 동부 지역에서 Video Indexer 유료 계정을 만들 수 있습니다.
 
### <a name="video-indexer-url"></a>Video Indexer URL

Video Indexer 지역 끝점은 모두 www로만 시작 되도록 통합 되었습니다. 작업 항목은 필요 하지 않습니다.

이제는 위젯을 포함 하거나 Video Indexer 웹 응용 프로그램에 로그인 하는 것이 www.videoindexer.ai에 도달 했습니다.

또한 wus.videoindexer.ai는 www로 리디렉션됩니다. 추가 정보는 [앱의 Embed Video Indexer](video-indexer-embed-widgets.md)위젯에 있습니다.

## <a name="april-2020"></a>2020년 4월

### <a name="new-widget-parameters-capabilities"></a>새 위젯 매개 변수 기능

**Insights** 위젯에는 새 매개 변수 및가 포함 되어 있습니다. `language` `control`

**플레이어** 위젯에는 새 `locale` 매개 변수가 있습니다. `locale`및 `language` 매개 변수는 플레이어의 언어를 제어 합니다.

자세한 내용은 [위젯 형식](video-indexer-embed-widgets.md#widget-types) 섹션을 참조 하세요. 

### <a name="new-player-skin"></a>새 플레이어 스킨

업데이트 된 설계로 시작 된 새 플레이어 스킨입니다.

### <a name="prepare-for-upcoming-changes"></a>예정 된 변경 내용 준비

* 현재 다음 Api는 계정 개체를 반환 합니다.

    * [유료-계정](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Paid-Account)
    * [계정 가져오기](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Account)
    * [가져오기-계정-권한 부여](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Accounts-Authorization)
    * [계정-토큰 포함](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Accounts-With-Token)
 
    계정 개체에는 `Url` [Video Indexer 웹 사이트](https://www.videoindexer.ai/)의 위치를 가리키는 필드가 있습니다.
유료 계정의 경우 `Url` 이 필드는 현재 공용 웹 사이트가 아닌 내부 URL을 가리킵니다.
몇 주 후에는이를 변경 하 고 모든 계정에 대 한 [Video Indexer 웹 사이트](https://www.videoindexer.ai/) URL (평가판 및 유료)을 반환 합니다.

    내부 Url을 사용 하지 마세요. [Video Indexer 공용 api](https://api-portal.videoindexer.ai/)를 사용 해야 합니다.
* 응용 프로그램에 Video Indexer Url을 포함 하는 경우 url이 [Video Indexer 웹 사이트나](https://www.videoindexer.ai/) Video Indexer API 끝점 ()을 가리키지 `https://api.videoindexer.ai` 않고 지역 끝점 (예:)이 아닌 경우 `https://wus2.videoindexer.ai` url을 다시 생성 합니다.

   다음 중 하나를 수행 하 여이 작업을 수행할 수 있습니다.

    * URL을 Video Indexer 위젯 Api (예: [insights 위젯](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Get-Video-Insights-Widget))를 가리키는 url로 바꿉니다.
    * Video Indexer 웹 사이트를 사용 하 여 새 포함 된 URL 생성:
         
         **Play** 를 눌러 비디오 페이지로 이동 합니다. > **&lt; / &gt; 포함** 단추를 클릭 하 > URL을 응용 프로그램에 복사 합니다.
   
    지역 Url은 지원 되지 않으며, 몇 주 후에 차단 됩니다.

## <a name="january-2020"></a>2020년 1월
 
### <a name="custom-language-support-for-additional-languages"></a>추가 언어에 대 한 사용자 지정 언어 지원

이제 Video Indexer에서, 및에 대 한 사용자 지정 언어 모델 `ar-SY` 을 지원 `en-UK` `en-AU` 합니다 (API에만 해당).
 
### <a name="delete-account-timeframe-action-update"></a>계정 기간 삭제 작업 업데이트

계정 삭제 작업은 이제 48 시간 대신 90 일 이내에 계정을 삭제 합니다.
 
### <a name="new-video-indexer-github-repository"></a>새 Video Indexer GitHub 리포지토리

이제 여러 프로젝트를 포함 하는 새로운 Video Indexer GitHub, 시작 가이드 및 코드 샘플을 사용할 수 있습니다. https://github.com/Azure-Samples/media-services-video-indexer
 
### <a name="swagger-update"></a>Swagger 업데이트

단일 [Video Indexer OpenAPI 사양 (swagger)](https://api-portal.videoindexer.ai/docs/services/Operations/export?DocumentFormat=OpenApiJson)으로 통합 **인증** 및 **작업** 을 Video Indexer 합니다. 개발자는 [Video Indexer 개발자 포털](https://api-portal.videoindexer.ai/)에서 api를 찾을 수 있습니다.

## <a name="december-2019"></a>2019년 12월

### <a name="update-transcript-with-the-new-api"></a>새 API로 성적 증명서 업데이트

[업데이트-비디오-인덱스](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?&pattern=update) API를 사용 하 여 기록의 특정 섹션을 업데이트 합니다.

### <a name="fix-account-configuration-from-the-video-indexer-portal"></a>Video Indexer 포털에서 계정 구성 수정

이제 다음과 같은 문제를 쉽게 해결할 수 있도록 Media Services 연결 구성을 업데이트할 수 있습니다. 

* 잘못 된 Azure Media Services 리소스
* 암호 변경
* Media Services 리소스가 구독 간에 이동 됨  

계정 구성을 수정 하려면 Video Indexer 포털에서 설정 > 계정 탭 (소유자로)으로 이동 합니다.

### <a name="configure-the-custom-vision-account"></a>사용자 지정 비전 계정 구성

Video Indexer 포털을 사용 하 여 유료 계정에서 사용자 지정 비전 계정을 구성 합니다 (이전에는 API 에서만 지원 됨). 이렇게 하려면 Video Indexer 포털에 로그인 하 고 모델 사용자 지정 > 애니메이션 문자 > 구성을 선택 합니다. 

### <a name="scenes-shots-and-keyframes--now-in-one-insight-pane"></a>장면, 샷 및 키 프레임-이제 하나의 통찰력 창

이제 장면, 샷 및 키 프레임은 더 쉽게 사용 하 고 탐색할 수 있도록 하나의 통찰력으로 병합 됩니다. 원하는 장면을 선택 하면 구성 된 샷 및 키 프레임을 볼 수 있습니다. 

### <a name="notification-about-a-long-video-name"></a>긴 비디오 이름에 대 한 알림

비디오 이름이 80 자 보다 길면 Video Indexer 업로드 시 설명 오류를 표시 합니다.

### <a name="streaming-endpoint-is-disabled-notification"></a>스트리밍 끝점을 사용할 수 없습니다. 알림

스트리밍 끝점이 사용 하지 않도록 설정 된 경우 플레이어 페이지에 설명 오류가 Video Indexer 표시 됩니다.

### <a name="error-handling-improvement"></a>오류 처리 향상

상태 코드 409은 이제 비디오를 인덱싱할 때 비디오를 [다시 인덱싱하고](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?https://api-portal.videoindexer.ai/docs/services/Operations/operations/Re-Index-Video?) 비디오 인덱스 Api를 [업데이트](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Index?) 하는 방식으로 반환 됩니다 .이는 현재 다시 인덱스 변경 내용을 실수로 재정의 하지 않도록 하기 위해 비디오를 활발히 인덱싱하고 있습니다.

## <a name="november-2019"></a>2019년 11월
 
* 한국어 사용자 지정 언어 모델 지원

    이제 Video 인덱서 `ko-KR` 는 API와 포털 모두에서 한국어 ()의 사용자 지정 언어 모델을 지원 합니다. 
* 음성 텍스트에 대해 지원 되는 새 언어 (STT)

    Video Indexer Api는 이제 아랍어 Levantine (ar SY), 영어 영국 언어 (en-us) 및 영어 오스트레일리아 언어 (en-us)로 STT을 지원 합니다.
    
    비디오 업로드의 경우 zh-cn-HANS를 zh-cn로 대체 했습니다. 둘 다 지원 되지만 zh-cn는 권장 되며 더 정확 합니다.
    
## <a name="october-2019"></a>2019년 10월
 
* 갤러리에서 애니메이션 문자 검색

    애니메이션 문자를 인덱싱할 때 이제 계정의 비디오 교정쇄에서 검색할 수 있습니다. 자세한 내용은 [문자 인식](animated-characters-recognition.md)(영문)을 참조 하세요.

## <a name="september-2019"></a>2019년 9월
 
IBC 2019에서 여러 가지 기능이 발표 되었습니다.
 
* 문자 인식 애니메이션 (공개 미리 보기)

    사용자 지정 비전과의 통합을 통해 애니메이션 된 콘텐츠에서 그룹 ad 인식 문자를 검색 하는 기능입니다. 자세한 내용은 [애니메이션 캐릭터 검색](animated-characters-recognition.md)을 참조하세요.
* 다국어 id (공개 미리 보기)

    오디오 트랙에서 여러 언어로 된 세그먼트를 검색 하 고이를 기반으로 다국어 성적 증명서를 만듭니다. 초기 지원: 영어, 스페인어, 독일어 및 프랑스어 자세한 내용은 [자동으로 다국어 콘텐츠 식별 및 전사](multi-language-identification-transcription.md)를 참조하세요.
* 사용자 및 위치에 대 한 명명 된 엔터티 추출

    NLP (자연어 처리)를 통해 음성 및 시각적 텍스트에서 브랜드, 위치 및 사람을 추출 합니다.
* 편집 샷 유형 분류

    종가, 중간 샷, 두 샷, 실내, 실외 등의 편집 형식으로 샷의 태그를 지정 합니다. 자세한 내용은 [편집 샷 유형 검색](scenes-shots-keyframes.md#editorial-shot-type-detection)을 참조 하세요.
* 토픽 추론 개선-이제 수준 2 포함
    
    추론 model 항목은 이제 IPTC 분류의 세부적인 세분성을 지원 합니다. [새 AI 지원 혁신을 Azure Media Services](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/)에 대 한 자세한 내용을 읽으십시오.

## <a name="august-2019"></a>2019년 8월
 
### <a name="video-indexer-deployed-in-uk-south"></a>영국 남부에 배포 된 Video Indexer

이제 영국 남부 지역에서 Video Indexer 유료 계정을 만들 수 있습니다.

### <a name="new-editorial-shot-type-insights-available"></a>새 편집 샷 유형 정보 사용 가능

비디오 샷에 추가 된 새 태그는 매우 확대/확대, 확대, 중간, 두 샷, 실외, 실내, 왼쪽 및 오른쪽 (JSON에서 사용 가능)과 같은 콘텐츠 생성 워크플로에 사용 되는 일반적인 편집 문구를 사용 하 여 해당 항목을 식별 하는 편집 "샷 유형"을 제공 합니다.

### <a name="new-people-and-locations-entities-extraction-available"></a>새 사용자 및 위치 엔터티 추출 사용 가능

Video Indexer는 비디오의 OCR 및 기록에서 NLP (자연어 처리)를 통해 명명 된 위치 및 사용자를 식별 합니다. Video Indexer는 기계 학습 알고리즘을 사용 하 여 특정 위치 (예: Eiffel 타워) 나 사람 (예: John Doe)을 비디오에서 호출 하는 경우를 인식 합니다.

### <a name="keyframes-extraction-in-native-resolution"></a>네이티브 해상도의 키 프레임 추출

Video Indexer에 의해 추출 된 키프레임은 비디오의 원래 해상도에서 사용할 수 있습니다.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>이미지에서 사용자 지정 얼굴 모델을 학습 하기 위한 GA

미리 보기 모드에서 GA (API 및 포털을 통해 사용 가능)로 이동 된 이미지의 교육 얼굴입니다.

> [!NOTE]
> "GA로 미리 보기" 전환과 관련 된 가격에는 영향을 주지 않습니다.

### <a name="hide-gallery-toggle-option"></a>갤러리 표시/숨기기 토글 옵션

사용자는 포털에서 갤러리 탭을 숨기도록 선택할 수 있습니다 (샘플 탭 숨기기와 유사).
 
### <a name="maximum-url-size-increased"></a>최대 URL 크기 증가

비디오를 인덱싱할 때 URL 쿼리 문자열 4096 (2048 대신)을 지원 합니다.
 
### <a name="support-for-multi-lingual-projects"></a>다국어 프로젝트 지원

이제 다른 언어로 인덱싱되는 비디오 (API만 해당)를 기반으로 프로젝트를 만들 수 있습니다.

## <a name="july-2019"></a>2019년 7월

### <a name="editor-as-a-widget"></a>Widget 편집기

이제는 Video Indexer AI 편집기를 고객 응용 프로그램에 포함 시킬 수 있는 위젯로 사용할 수 있습니다.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>포털에서 자막 파일의 사용자 지정 언어 모델 업데이트

고객은 포털의 사용자 지정 페이지에서 VTT, SRT 및 TTML 파일 형식을 언어 모델에 대 한 입력으로 제공할 수 있습니다.

## <a name="june-2019"></a>2019년 6월

### <a name="video-indexer-deployed-to-japan-east"></a>일본 동부에 배포 Video Indexer

이제 일본 동부 지역에서 Video Indexer 유료 계정을 만들 수 있습니다.

### <a name="create-and-repair-account-api-preview"></a>계정 API 만들기 및 복구 (미리 보기)

[Azure Media Service 연결 끝점 또는 키를 업데이트할](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag)수 있는 새 API가 추가 되었습니다.

### <a name="improve-error-handling-on-upload"></a>업로드 시 오류 처리 향상 

설명 메시지는 기본 Azure Media Services 계정의 잘못 된 구성의 경우 반환 됩니다.

### <a name="player-timeline-keyframes-preview"></a>플레이어 타임 라인 키 프레임 미리 보기 

이제 플레이어의 타임 라인에서 각 시간에 대 한 이미지 미리 보기를 볼 수 있습니다.

### <a name="editor-semi-select"></a>편집기 세미 선택

이제 편집기에서 특정 통찰력을 선택한 결과로 선택 된 모든 정보에 대 한 미리 보기를 볼 수 있습니다.

## <a name="may-2019"></a>2019년 5월

### <a name="update-custom-language-model-from-closed-caption-file"></a>닫힌 캡션 파일에서 사용자 지정 언어 모델 업데이트

[사용자 지정 언어 모델 만들기](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) 및 [사용자 지정 언어 모델 업데이트](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) api는 이제 VTT, SRT 및 TTML 파일 형식을 언어 모델에 대 한 입력으로 지원 합니다.

[비디오 성적 업데이트 API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript)를 호출 하면 자동으로 기록이 추가 됩니다. 비디오와 연결 된 학습 모델도 자동으로 업데이트 됩니다. 언어 모델을 사용자 지정 하 고 학습 하는 방법에 대 한 자세한 내용은 [Video Indexer를 사용 하 여 언어 모델 사용자 지정](customize-language-model-overview.md)을 참조 하세요.

### <a name="new-download-transcript-formats--txt-and-csv"></a>새 다운로드 기록 형식 – TXT 및 CSV

이미 지원 되는 (SRT, VTT 및 TTML Video Indexer) 폐쇄 캡션 형식 외에도 이제는 TXT 및 CSV 형식으로 성적 증명서 다운로드를 지원 합니다.

## <a name="next-steps"></a>다음 단계

[개요](video-indexer-overview.md)
