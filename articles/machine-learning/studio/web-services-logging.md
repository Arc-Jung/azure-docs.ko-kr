---
title: 'ML Studio (클래식): 웹 서비스 로깅 사용-Azure'
description: Machine Learning Studio (클래식) 웹 서비스에 대 한 로깅을 사용 하도록 설정 하는 방법을 알아봅니다. 로깅은 API 문제를 해결하는 데 도움이 되는 추가 정보를 제공합니다.
services: machine-learning
author: likebupt
ms.author: keli19
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
ms.date: 06/15/2017
ms.openlocfilehash: b879fac7482b000e1b92f2fcc3f7586ae7dae321
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87430198"
---
# <a name="enable-logging-for-azure-machine-learning-studio-classic-web-services"></a>Azure Machine Learning Studio (클래식) 웹 서비스에 대 한 로깅 사용

**적용 대상:** ![ 예 ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (클래식) ![ 아니요](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../compare-azure-ml-to-studio-classic.md)  


이 문서에서는 Machine Learning Studio (클래식) 웹 서비스의 로깅 기능에 대 한 정보를 제공 합니다. 로깅은 오류 번호 및 메시지 외에도 Machine Learning Studio (클래식) Api에 대 한 호출 문제를 해결 하는 데 도움이 될 수 있는 추가 정보를 제공 합니다.  

## <a name="how-to-enable-logging-for-a-web-service"></a>웹 서비스에서 로깅을 사용하도록 설정하는 방법

[Azure Machine Learning Studio (클래식) 웹 서비스](https://services.azureml.net) 포털에서 로깅을 사용 하도록 설정 합니다. 

1. 에서 Azure Machine Learning Studio (클래식) 웹 서비스 포털에 로그인 [https://services.azureml.net](https://services.azureml.net) 합니다. 클래식 웹 서비스의 경우 Studio (클래식)의 Machine Learning Studio (클래식) 웹 서비스 페이지에서 **새 웹 서비스 환경** 을 클릭 하 여 포털로 이동할 수도 있습니다.

   ![새 웹 서비스 환경 링크](./media/web-services-logging/new-web-services-experience-link.png)

2. 상단 메뉴 모음에서 새 웹 서비스에 대한 **웹 서비스**를 클릭하거나 클래식 웹 서비스에 대한 **클래식 웹 서비스**를 클릭합니다.

   ![새로운 또는 클래식 웹 서비스 선택](./media/web-services-logging/select-web-service.png)

3. 새 웹 서비스의 경우 웹 서비스 이름을 클릭합니다. 클래식 웹 서비스의 경우 웹 서비스 이름을 클릭하고, 다음 페이지에서 적절한 엔드포인트를 클릭합니다.

4. 최상위 메뉴 모음에서 **구성**을 클릭합니다.

5. **로깅 사용** 옵션을 *오류*(오류만 로그) 또는 *모두*(전체 로깅의 경우)로 설정합니다.

   ![로깅 수준 선택](./media/web-services-logging/enable-logging.png)

6. **저장**을 클릭합니다.

7. 클래식 웹 서비스의 경우 **ml-diagnostics** 컨테이너를 만듭니다.

   모든 웹 서비스 로그는 웹 서비스와 연결된 스토리지 계정에 있는 **ml-diagnostics**이라는 blob 컨테이너에 저장됩니다. 새 웹 서비스의 경우 이 컨테이너는 처음 웹 서비스에 액세스할 때 만들어집니다. 클래식 웹 서비스의 경우 이 컨테이너가 없는 경우 하나 만들어야 합니다. 

   1. [Azure Portal](https://portal.azure.com)에서 웹 서비스와 연결된 스토리지 계정으로 이동합니다.

   2. **Blob Service**에서 **컨테이너**를 클릭 합니다.

   3. 컨테이너 **ml-diagnostics**가 없는 경우 **+컨테이너**를 클릭하여 컨테이너 이름을 “ml-diagnostics”로 지정하고, **액세스 형식**을 “Blob”으로 선택합니다. **확인**을 클릭합니다.

      ![진단 로그를 저장할 새 컨테이너 만들기](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> 기존 웹 서비스의 경우 Machine Learning Studio (클래식)의 웹 서비스 대시보드에 로깅을 사용 하도록 설정 하는 스위치가 있습니다. 그러나 이제 로깅은 웹 서비스 포털을 통해 관리되므로 이 문서에 설명된 대로 포털을 통해 로깅을 사용하도록 설정해야 합니다. 이미 Studio (클래식)에서 로깅을 사용 하도록 설정한 경우에는 웹 서비스 포털에서 로깅을 사용 하지 않도록 설정 하 고 다시 사용 하도록 설정 합니다.


## <a name="the-effects-of-enabling-logging"></a>로깅 활성화의 효과
로깅을 사용 하도록 설정 하면 웹 서비스 끝점의 진단 및 오류가 사용자 작업 영역과 연결 된 Azure Storage 계정의 **ml 진단** blob 컨테이너에 기록 됩니다. 이 컨테이너는 이 스토리지 계정과 연결된 모든 작업 영역의 모든 웹 서비스 엔드포인트에 대한 모든 진단 정보를 포함합니다.

Azure Storage 계정을 살펴보는 데 사용할 수 있는 여러 도구 중 하나를 통해 로그를 볼 수 있습니다. 가장 쉬운 방법은 Azure Portal에서 스토리지 계정으로 이동한 다음, **컨테이너**를 클릭하고 컨테이너 **ml-diagnostics**를 클릭하는 것입니다.  

## <a name="log-blob-detail-information"></a>Blob 세부 정보 기록
컨테이너의 각 blob은 정확히 다음 작업 중 하나에 대한 진단 정보를 포함합니다.

* Batch 실행 메서드 실행  
* 요청-응답 메서드 실행  
* 요청-응답 컨테이너 초기화

각 Blob의 이름에는 다음과 같은 형식의 접두사가 있습니다. 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


_로그 형식_은 다음 값 중 하나를 사용합니다.  

* 일괄 처리  
* score/requests  
* score/init  

