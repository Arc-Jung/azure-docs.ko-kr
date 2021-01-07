---
title: CI/CD를 사용하여 컨테이너 애플리케이션 배포
description: 이 자습서에서는 Visual Studio Azure DevOps를 사용하여 Azure Service Fabric 컨테이너 애플리케이션에 대한 지속적인 통합 및 배포를 설정하는 방법을 알아봅니다.
ms.topic: tutorial
ms.date: 08/29/2018
ms.custom: mvc
ms.openlocfilehash: bb0eb9226a99f139ff10a8da12a1e22017536c67
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96018854"
---
# <a name="tutorial-deploy-a-container-application-with-cicd-to-a-service-fabric-cluster"></a>자습서: Service Fabric 클러스터에 CI/CD로 컨테이너 애플리케이션 배포

이 자습서는 시리즈의 2부로, Visual Studio 및 Azure DevOps를 사용하여 Azure Service Fabric 컨테이너 애플리케이션에 대한 지속적인 통합 및 배포를 설정하는 방법을 설명합니다.  기존 Service Fabric 애플리케이션이 필요하며 [Azure Service Fabric에 Windows 컨테이너로 .NET 애플리케이션 배포](service-fabric-host-app-in-a-container.md)에서 만든 애플리케이션을 예제로 사용합니다.

시리즈 2부에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 프로젝트에 소스 제어 추가
> * Visual Studio 팀 탐색기에서 빌드 정의 만들기
> * Visual Studio 팀 탐색기에서 릴리스 정의 만들기
> * 애플리케이션 자동 배포 및 업그레이드

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 시작하기 전에:

* Azure에 클러스터가 있거나, [이 자습서에 따라 새로 만들어야](service-fabric-tutorial-create-vnet-and-windows-cluster.md) 합니다.
* [컨테이너화된 애플리케이션을 배포](service-fabric-host-app-in-a-container.md)해야 합니다.

## <a name="prepare-a-publish-profile"></a>게시 프로필 준비

이제 [컨테이너 애플리케이션을 배포](service-fabric-host-app-in-a-container.md)했으므로 연속 통합을 설정할 준비가 되었습니다.  먼저, Azure DevOps 내에서 실행할 배포 프로세스에 사용할 게시 프로필을 애플리케이션 내에서 준비하는 것입니다.  게시 프로필은 이전에 만든 클러스터를 대상으로 지정하도록 구성해야 합니다.  Visual Studio를 시작하고 기존 Service Fabric 애플리케이션 프로젝트를 엽니다.  **솔루션 탐색기** 에서 애플리케이션을 마우스 오른쪽 단추로 클릭하고 **게시...** 를 선택합니다.

연속 통합 워크플로로 사용할 대상 프로필을 애플리케이션 프로젝트 내에서 선택합니다(예: 클라우드).  클러스터 연결 엔드포인트를 지정합니다.  **애플리케이션 업그레이드** 확인란을 선택하여 Azure DevOps의 각 배포에 대해 애플리케이션이 업그레이드되도록 합니다.  **저장** 하이퍼링크를 클릭하여 설정을 게시 프로필에 저장한 후 **취소** 를 클릭하여 대화 상자를 닫습니다.

![푸시 프로필][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-azure-devops-git-repo"></a>새 Azure DevOps Git 리포지토리에 Visual Studio 솔루션 공유

애플리케이션 소스 파일을 Azure DevOps의 팀 프로젝트에 공유하여 빌드를 생성할 수 있습니다.

Visual Studio의 오른쪽 하단의 상태 표시줄에서 **소스 제어에 추가** -> **Git** 을 선택하여 프로젝트 대한 새 로컬 Git 리포지토리를 만듭니다.

**팀 탐색기** 의 **푸시** 보기에서 **Azure DevOps에 푸시** 아래에 있는 **Git 리포지토리 게시** 단추를 선택합니다.

![Visual Studio의 팀 탐색기 - 동기화 창 스크린샷. Azure DevOps로 푸시 아래에서 Publish to Git Repo 단추가 강조 표시됩니다.][push-git-repo]

사용자의 이메일을 확인하고 **계정** 드롭다운에서 조직을 선택합니다. 아직 없는 경우 조직을 설정해야 합니다. 리포지토리 이름을 입력하고 **리포지토리 게시** 를 선택합니다.

![Azure DevOps로 푸시 창의 스크린샷. 이메일, 계정, 리포지토리 이름 및 리포지토리 게시 단추에 대한 설정이 강조 표시됩니다.][publish-code]

리포지토리를 게시하면 사용자 계정에 로컬 리포지토리와 같은 이름으로 새 팀 프로젝트가 만들어집니다. 기존 팀 프로젝트에서 리포지토리를 만들려면 **리포지토리** 이름 옆에서 **고급** 을 클릭하고 팀 프로젝트를 선택합니다. **See it on the web(웹에서 보기)** 을 선택하여 웹에서 코드를 볼 수 있습니다.

## <a name="configure-continuous-delivery-with-azure-pipelines"></a>Azure Pipelines를 사용한 지속적인 배달 구성

Azure DevOps 빌드 정의는 순차적으로 실행되는 빌드 단계 집합으로 구성된 워크플로를 설명합니다. Service Fabric 애플리케이션 패키지 및 기타 아티팩트를 생성하는 빌드 정의를 만들어 Service Fabric 클러스터를 배포합니다. Azure DevOps [빌드 정의](https://www.visualstudio.com/docs/build/define/create)에 대해 자세히 알아봅니다. 

Azure DevOps 릴리스 정의에서는 애플리케이션 패키지를 클러스터에 배포하는 워크플로를 설명합니다. 빌드 정의와 릴리스 정의를 함께 사용할 경우 소스 파일로 시작하여 클러스터에서 실행 중인 애플리케이션에서 종료할 때까지 전체 워크플로를 실행합니다. Azure DevOps [릴리스 정의](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition)에 대해 자세히 알아봅니다.

### <a name="create-a-build-definition"></a>빌드 정의 만들기

웹 브라우저에서 https://dev.azure.com 으로 이동하고 조직, 새 프로젝트를 차례로 선택하여 새로운 팀 프로젝트를 엽니다. 

왼쪽 패널에서 **파이프라인** 옵션을 선택한 다음, **새 파이프라인** 을 클릭합니다.

>[!NOTE]
>빌드 정의 템플릿이 표시되지 않으면 **새 YAML 파이프라인 생성 환경** 이 해제되어 있는지 확인합니다. 이 기능은 DevOps 계정의 **미리 보기 기능** 섹션 내에서 구성됩니다.

![새 파이프라인][new-pipeline]

소스로 **Azure Repos Git** 을 선택하고, 팀 프로젝트 이름, 프로젝트 리포지토리 및 **마스터** 기본 분기 또는 수동 및 예약 빌드를 선택합니다.  그런 후 **계속** 을 클릭합니다.

**템플릿 선택** 에서 **Docker 지원을 사용하는 Azure Service Fabric 애플리케이션** 템플릿을 선택하고 **적용** 을 클릭합니다.

![빌드 템플릿 선택][select-build-template]

**태스크** 에서 **에이전트 풀** 로 **Hosted VS2017** 을 선택합니다.

![태스크 선택][task-agent-pool]

**태그 이미지** 를 클릭합니다.

**Container Registry 유형** 에서 **Azure Container Registry** 를 선택합니다. **Azure 구독** 을 선택한 후 **권한 부여** 를 클릭합니다. **Azure Container Registry** 를 선택합니다.

![Docker 태그 이미지 선택][select-tag-images]

**이미지 푸시** 를 클릭합니다.

**Container Registry 유형** 에서 **Azure Container Registry** 를 선택합니다. **Azure 구독** 을 선택한 후 **권한 부여** 를 클릭합니다. **Azure Container Registry** 를 선택합니다.

![Docker 푸시 이미지 선택][select-push-images]

**트리거** 탭 아래에서 **지속적인 통합 사용** 을 선택하여 지속적인 통합을 사용하도록 설정합니다. **분기 필터** 내에서 **+ 추가** 를 클릭하면 **분기 사양** 이 기본값인 **마스터** 로 지정됩니다.

**빌드 파이프라인 및 큐 저장 대화 상자** 에서 **저장 및 큐에 넣기** 를 클릭하여 빌드를 수동으로 시작합니다.

![트리거 선택][save-and-queue]

푸시 또는 체크 인 시 트리거도 빌드합니다. 빌드 진행률을 확인하려면 **빌드** 탭으로 전환합니다.  빌드가 성공적으로 실행되는지 확인한 후 애플리케이션을 클러스터에 배포하는 릴리스 정의를 정의합니다.

### <a name="create-a-release-definition"></a>릴리스 정의 만들기

왼쪽 패널에서 **파이프라인** 옵션을 선택한 다음, **릴리스**, **+ 새 파이프라인** 을 차례로 선택합니다.  **템플릿 선택** 에서 목록의 **Azure Service Fabric 배포** 템플릿을 선택하고 **적용** 을 선택합니다.

![릴리스 템플릿 선택][select-release-template]

**태스크** 를 선택하고 **환경 1** 을 선택한 후 **+새로 만들기** 를 선택하여 새 클러스터 연결을 추가합니다.

![클러스터 연결 추가][add-cluster-connection]

**새 Service Fabric 연결 추가** 보기에서 **인증서 기반** 또는 **Azure Active Directory** 인증을 선택합니다.  "Mysftestcluster" 및 "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000"의 클러스터 엔드포인트(또는 배포 중인 클러스터의 엔드포인트)의 연결 이름을 지정합니다.

인증서 기반 인증의 경우 클러스터를 만드는 데 사용된 서버 인증서의 **서버 인증서 지문** 을 추가합니다.  **클라이언트 인증서** 에서 base-64 인코딩된 클라이언트 인증서 파일을 추가합니다. base-64 인코딩된 해당 인증서의 표현을 가져오는 방법에 대한 정보는 해당 필드에서 도움말 팝업을 참조하세요. 인증서의 **암호** 도 추가합니다.  별도 클라이언트 인증서가 없는 경우 클러스터 또는 서버 인증서를 사용할 수 있습니다.

Azure Active Directory 자격 증명의 경우 사용하려는 클러스터 및 자격 증명을 만드는 데 사용된 서버 인증서의 **서버 인증서 지문** 을 추가하여 **사용자 이름** 및 **암호** 필드에서 클러스터에 연결합니다.

**추가** 를 클릭하여 클러스터 연결을 저장합니다.



에이전트 단계 아래에서 **Service Fabric 애플리케이션 배포** 를 클릭합니다.
**Docker 설정** 을 클릭하고 **Docker 설정 구성** 을 클릭합니다. **레지스트리 자격 증명 소스** 에서 **Azure Resource Manager 서비스 연결** 을 선택합니다. 그런 후 **Azure 구독** 을 선택합니다.

![릴리스 파이프라인 에이전트][release-pipeline-agent]

다음으로 릴리스 정의가 빌드에서 출력을 찾을 수 있도록 파이프라인에 빌드 아티팩트를 추가합니다. **파이프라인** 및 **아티팩트**-> **+추가** 를 선택합니다.  **원본(빌드 정의)** 에서 이전에 만든 빌드 정의를 선택합니다.  **추가** 를 클릭하여 빌드 아티팩트를 저장합니다.

![아티팩트 추가][add-artifact]

빌드가 완료될 때 릴리스가 자동으로 생성하도록 지속적인 배포 트리거를 사용하도록 설정합니다. 아티팩트에서 번개 아이콘을 클릭하고, 트리거를 사용하고, **저장** 을 클릭하여 릴리스 정의를 저장합니다.

![트리거 사용][enable-trigger]

**+ 릴리스** -> **릴리스 만들기** -> **만들기** 를 선택하여 릴리스를 수동으로 만듭니다. **릴리스** 탭에서 릴리스 진행률을 모니터링할 수 있습니다.

배포에 성공했고 클러스터에서 애플리케이션이 실행 중인지 확인합니다.  웹 브라우저를 열고 `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`로 이동합니다.  애플리케이션 버전을 확인합니다. 이 예제에서는 “1.0.0.20170616.3”입니다.

## <a name="commit-and-push-changes-trigger-a-release"></a>변경 내용 커밋 및 푸시, 릴리스 트리거

Azure DevOps의 일부 코드 변경을 체크 인하여 연속 통합 파이프라인이 작동하는지 확인합니다.

코드를 작성하면 Visual Studio에서 변경 내용을 자동으로 추적합니다. 오른쪽 하단의 상태 표시줄에서 보류 중인 변경 내용 아이콘(![보류 중인 변경 내용 아이콘은 연필 및 숫자를 표시합니다.][pending])을 선택하여 로컬 Git 리포지토리로 변경 내용을 커밋합니다.

팀 탐색기의 **변경 내용** 보기에서 업데이트를 설명하는 메시지를 추가하고 변경 내용을 커밋합니다.

![모두 커밋][changes]

팀 탐색기에서 게시 취소된 변경 내용 상태 표시줄 아이콘(![게시 취소된 변경 내용][unpublished-changes]) 또는 동기화 보기를 선택합니다. **푸시** 를 선택하여 Azure DevOps에서 코드를 업데이트합니다.

![변경 내용 푸시][push]

Azure DevOps에 변경 내용을 푸시하면 빌드가 자동으로 트리거됩니다.  빌드 정의가 성공적으로 완료되면 릴리스가 자동으로 만들어지고 클러스터에서 애플리케이션 업그레이드가 시작됩니다.

빌드 진행률을 확인하려면 Visual Studio의 **팀 탐색기** 에서 **빌드** 탭으로 전환합니다.  빌드가 성공적으로 실행되는지 확인한 후 애플리케이션을 클러스터에 배포하는 릴리스 정의를 정의합니다.

배포에 성공했고 클러스터에서 애플리케이션이 실행 중인지 확인합니다.  웹 브라우저를 열고 `http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/`로 이동합니다.  애플리케이션 버전을 확인합니다. 이 예제에서는 &quot;1.0.0.20170815.3&quot;입니다.

![Service Fabric Explorer의 투표 앱 스크린샷. Essentials 탭에서 앱 버전 "1.0.0.20170815.3"이 강조 표시됩니다.][sfx1]

## <a name="update-the-application"></a>애플리케이션 업데이트

애플리케이션에서 코드를 변경합니다.  이전 단계에 따라 변경 내용을 저장 및 커밋합니다.

애플리케이션 업그레이드가 시작되면 Service Fabric Explorer에서 업그레이드 진행률을 확인할 수 있습니다.

![Service Fabric Explorer의 투표 앱 스크린샷. "업그레이드 진행 중" 메시지가 강조 표시되고 앱 상태가 "업그레이드"로 표시됩니다.][sfx2]

애플리케이션 업그레이드에는 몇 분 정도 걸릴 수 있습니다. 업그레이드가 완료되면 애플리케이션이 다음 버전으로 실행됩니다.  이 예제에서는 "1.0.0.20170815.4"입니다.

![Service Fabric Explorer의 투표 앱 스크린샷. Essentials 탭에서 업데이트된 앱 버전 "1.0.0.20170815.4"가 강조 표시됩니다.][sfx3]

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 작업 방법을 알아보았습니다.

> [!div class="checklist"]
> * 프로젝트에 소스 제어 추가
> * 빌드 정의 만들기
> * 릴리스 정의 만들기
> * 애플리케이션 자동 배포 및 업그레이드

자습서의 다음 부분에서는 [컨테이너에 대한 모니터링](service-fabric-tutorial-monitoring-wincontainers.md)을 설정하는 방법을 알아봅니다.

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishCode.png
[new-pipeline]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewPipeline.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectBuildTemplate.png
[task-agent-pool]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/TaskAgentPool.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SaveAndQueue.png
[select-tag-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerTagImages.png
[select-push-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerPushImages.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddClusterConnection.png
[release-pipeline-agent]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/ReleasePipelineAgent.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpointDialog.png
