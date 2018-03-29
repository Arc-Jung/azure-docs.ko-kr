---
title: Visual Studio에서 클러스터에 Azure Service Fabric 응용 프로그램 배포 | Microsoft Docs
description: Visual Studio에서 응용 프로그램을 클러스터에 배포하는 방법을 알아봅니다.
services: service-fabric
documentationcenter: .net
-author: mikkelhegn
-manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 1d8f8d903046f1d471f7abbe08a957b81522e391
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-deploy-an-application-to-a-service-fabric-cluster-in-azure"></a>자습서: Azure에서 Service Fabric 클러스터에 응용 프로그램 배포
이 자습서는 시리즈의 2부이며, Visual Studio에서 Azure Service Fabric 응용 프로그램을 Azure의 새 클러스터에 직접 배포하는 방법을 보여 줍니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.
> [!div class="checklist"]
> * Visual Studio에서 클러스터 만들기
> * Visual Studio를 사용하여 원격 클러스터에 응용 프로그램 배포


이 자습서 시리즈에서는 다음 방법에 대해 알아봅니다.
> [!div class="checklist"]
> * [.NET Service Fabric 응용 프로그램 빌드](service-fabric-tutorial-create-dotnet-app.md)
> * 응용 프로그램을 원격 클러스터에 배포
> * [Visual Studio Team Services를 사용하여 CI/CD 구성](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [응용 프로그램에 대한 모니터링 및 진단 설정](service-fabric-tutorial-monitoring-aspnet.md)


## <a name="prerequisites"></a>필수 조건
이 자습서를 시작하기 전에:
- Azure 구독이 없는 경우 [평가판 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.
- [Visual Studio 2017을 설치](https://www.visualstudio.com/)하고 **Azure 개발**과 **ASP.NET 및 웹 개발** 워크로드를 설치합니다.
- [Service Fabric SDK를 설치](service-fabric-get-started.md)합니다.

## <a name="download-the-voting-sample-application"></a>투표 응용 프로그램 샘플 다운로드
[이 자습서 시리즈의 1부](service-fabric-tutorial-create-dotnet-app.md)에서 투표 응용 프로그램 샘플을 빌드하지 않은 경우 다운로드할 수 있습니다. 명령 창에서 다음 명령을 실행하여 로컬 컴퓨터에 샘플 앱 리포지토리를 복제합니다.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="deploy-the-sample-application"></a>샘플 응용 프로그램 배포

### <a name="select-a-service-fabric-cluster-to-which-to-publish"></a>게시할 Service Fabric 클러스터 선택
응용 프로그램이 준비되면 Visual Studio에서 클러스터에 직접 배포할 수 있습니다.

배포 옵션에는 다음 두 가지 옵션이 있습니다.
- Visual Studio에서 클러스터를 만듭니다. 이 옵션을 사용하면 기본 설 구성으로 Visual Studio에서 직접 보안 클러스터정를 만들 수 있습니다. 이 유형의 클러스터는 클러스터를 만든 다음, Visual Studio 내에서 해당 클러스터에 직접 게시할 수 있는 테스트 시나리오에 적합합니다.
- 기존 클러스터를 구독에 게시합니다.

이 자습서에서는 Visual Studio에서 클러스터를 만드는 단계를 수행합니다. 다른 옵션의 경우 연결 엔드포인트를 복사하여 붙여넣거나 구독에서 선택할 수 있습니다.
> [!NOTE]
> 대부분의 서비스에서 역방향 프록시를 사용하여 서로 통신합니다. Visual Studio 및 Party 클러스터에서 만든 클러스터는 기본적으로 역방향 프록시를 사용하도록 설정됩니다.  기존 클러스터를 사용하는 경우 [클러스터에서 역방향 프록시를 사용하도록 설정](service-fabric-reverseproxy.md#setup-and-configuration)해야 합니다.

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Service Fabric 클러스터에 앱 배포
1. [솔루션 탐색기]에서 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭하고 **게시**를 선택합니다.

2. Azure 계정을 사용하여 로그인하면 구독 정보에 액세스할 수 있습니다. Party 클러스터를 사용하는 경우 이 단계는 선택 사항입니다.

3. **연결 엔드포인트**에 대한 드롭다운을 선택하고 "<Create New Cluster...>" 옵션을 선택합니다.
    
    ![[게시] 대화 상자](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)
    
4. "클러스터 만들기" 대화 상자에서 다음 설정을 수정합니다.

    1. "클러스터 이름" 필드에서 클러스터의 이름뿐만 아니라 사용하려는 구독과 위치도 지정합니다.
    2. 선택 사항: 노드 수를 수정할 수 있습니다. 기본적으로 Service Fabric 시나리오를 테스트하는 데 필요한 최소 세 개의 노드가 있습니다.
    3. "인증서" 탭을 선택합니다. 이 탭에서 클러스터의 인증서를 보호하는 데 사용할 암호를 입력합니다. 이 인증서는 클러스터를 안전하게 보호하는 데 도움이 됩니다. 인증서를 저장하려는 위치의 경로를 수정할 수도 있습니다. 또한 응용 프로그램을 클러스터에 게시하는 데 필요한 단계이므로 Visual Studio에서 사용자에 대한 인증서를 가져올 수도 있습니다.
    4. "VM 세부 정보" 탭을 선택합니다. 클러스터를 구성하는 VM(Virtual Machines)에 사용할 암호를 지정합니다. 사용자 이름과 암호를 사용하여 VM에 원격으로 연결할 수 있습니다. 또한 VM 컴퓨터 크기를 선택해야 하며, 필요한 경우 VM 이미지를 변경할 수도 있습니다.
    5. 선택 사항: "고급" 탭에서 클러스터와 함께 만들 부하 분산 장치에서 열려는 포트 목록을 수정할 수 있습니다. 또한 응용 프로그램 로그 파일을 라우팅하는 데 사용할 기존 Application Insights 키를 추가할 수도 있습니다.
    6. 설정 수정을 마쳤으면 "만들기" 단추를 선택합니다. 만들기가 완료되는 데 몇 분이 걸립니다. 클러스터가 완전히 만들어지면 출력 창에 표시됩니다.
    
    ![클러스터 만들기 대화 상자](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

4. 사용하려는 클러스터가 준비되면 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭하고 **게시**를 선택합니다.

    게시가 완료되면 브라우저를 통해 응용 프로그램에 요청을 보낼 수 있습니다.

5. 기본 설정 브라우저를 열고, 클러스터 주소를 입력합니다(포트 정보가 없는 연결 끝점. 예: win1kw5649s.westus.cloudapp.azure.com).

    이제 응용 프로그램을 로컬로 실행할 때와 동일한 결과가 표시됩니다.

    ![클러스터에서 API 응답](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>다음 단계
이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * Visual Studio에서 클러스터 만들기
> * Visual Studio를 사용하여 원격 클러스터에 응용 프로그램 배포

다음 자습서를 진행합니다.
> [!div class="nextstepaction"]
> [Visual Studio Team Services를 사용하여 연속 통합 설정](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
