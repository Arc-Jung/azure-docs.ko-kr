---
title: 자습서 - 코드 커밋 시 이미지 빌드
description: 이 자습서에서는 소스 코드를 Git 리포지토리에 커밋할 때 클라우드에서 컨테이너 이미지 빌드를 자동으로 트리거하도록 Azure Container Registry 작업을 구성하는 방법을 알아봅니다.
ms.topic: tutorial
ms.date: 05/04/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: 8af8daa4233fe6461b4e129f56a063e7cc212245
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74454746"
---
# <a name="tutorial-automate-container-image-builds-in-the-cloud-when-you-commit-source-code"></a>자습서: 소스 코드를 커밋할 때 클라우드에서 컨테이너 이미지 빌드 자동화

[빠른 작업](container-registry-tutorial-quick-task.md) 외에, ACR 작업은 소스 코드를 Git 리포지토리로 커밋할 때 클라우드에서 자동화된 Docker 컨테이너 이미지 빌드를 지원합니다.

이 자습서의 ACR 작업은 소스 코드를 Git 리포지토리로 커밋할 때 Dockerfile에 지정된 단일 컨테이너 이미지를 빌드하고 푸시합니다. YAML 파일을 사용하여 코드 커밋 시 여러 컨테이너를 빌드하고, 푸시하고, 필요에 따라 테스트하는 단계를 정의하는 [다단계 작업](container-registry-tasks-multi-step.md)을 만들려면 [자습서: 소스 코드를 커밋할 때 클라우드에서 다단계 컨테이너 워크플로 실행](container-registry-tutorial-multistep-task.md)을 참조하세요. ACR 작업 개요에 대해서는 [ACR 작업을 사용하여 OS 및 프레임워크 패치 자동화](container-registry-tasks-overview.md)를 참조하세요.

자습서 내용

> [!div class="checklist"]
> * 작업을 만듭니다.
> * 작업 테스트
> * 태스크 상태 보기
> * 코드 커밋을 사용하여 작업 트리거

이 자습서에서는 [이전 자습서](container-registry-tutorial-quick-task.md)의 단계를 이미 완료했다고 가정합니다. 아직 완료하지 않은 경우 계속 진행하기 전에 이전 자습서의 [필수 조건](container-registry-tutorial-quick-task.md#prerequisites) 섹션에 있는 단계를 완료하세요.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI를 로컬로 사용하려면 Azure CLI 버전 **2.0.46** 이상이 설치되어 있고, [az login][az-login]을 사용하여 로그인해야 합니다. `az --version`을 실행하여 버전을 찾습니다. CLI를 설치하거나 업그레이드해야 하는 경우 [Azure CLI 설치][azure-cli]를 참조하세요.

[!INCLUDE [container-registry-task-tutorial-prereq.md](../../includes/container-registry-task-tutorial-prereq.md)]

## <a name="create-the-build-task"></a>빌드 작업 만들기

이제 ACR 작업에서 커밋 상태를 읽고 리포지토리에 웹후크를 만들 수 있도록 설정하는 데 필요한 단계를 완료했으므로 리포지토리에 커밋할 때 컨테이너 이미지 빌드를 트리거하는 작업을 만들 수 있습니다.

먼저 이러한 셸 환경 변수를 환경에 적합한 값으로 채웁니다. 이 단계는 반드시 필요한 것이 아니지만, 여러 줄로 된 Azure CLI 명령을 이 자습서에서 좀 더 쉽게 실행할 수 있게 합니다. 이러한 환경 변수를 채우지 않는 경우 명령 예제에 나타나는 각 값을 수동으로 바꿔야 합니다.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section
```

이제 다음 [az acr task create][az-acr-task-create] 명령을 실행하여 작업을 만듭니다.

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> 이전에 미리 보기 중에 `az acr build-task` 명령을 사용하여 작업을 만든 경우 [az acr task][az-acr-task] 명령을 사용하여 해당 작업을 다시 만들어야 합니다.

이 작업은 `--context`에 지정된 리포지토리의 *마스터* 분기에 모든 시간 코드를 커밋하도록 지정하고, ACR 작업은 해당 분기의 코드에서 컨테이너 이미지를 빌드합니다. 리포지토리 루트의 `--file`에 지정된 Dockerfile은 이미지를 빌드하는 데 사용됩니다. `--image` 인수는 이미지 태그의 버전 부분에 대해 `{{.Run.ID}}`의 매개 변수화된 값을 지정하여 빌드된 이미지가 특정 빌드와 상호 연결되고 태그가 고유하게 지정되도록 합니다.

성공적인 [az acr task create][az-acr-task-create] 명령의 출력은 다음과 비슷합니다.

```console
{
  "agentConfiguration": {
    "cpu": 2
  },
  "creationDate": "2018-09-14T22:42:32.972298+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myregistry/providers/Microsoft.ContainerRegistry/registries/myregistry/tasks/taskhelloworld",
  "location": "westcentralus",
  "name": "taskhelloworld",
  "platform": {
    "architecture": "amd64",
    "os": "Linux",
    "variant": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myregistry",
  "status": "Enabled",
  "step": {
    "arguments": [],
    "baseImageDependencies": null,
    "contextPath": "https://github.com/gituser/acr-build-helloworld-node",
    "dockerFilePath": "Dockerfile",
    "imageNames": [
      "helloworld:{{.Run.ID}}"
    ],
    "isPushEnabled": true,
    "noCache": false,
    "type": "Docker"
  },
  "tags": null,
  "timeout": 3600,
  "trigger": {
    "baseImageTrigger": {
      "baseImageTriggerType": "Runtime",
      "name": "defaultBaseimageTriggerName",
      "status": "Enabled"
    },
    "sourceTriggers": [
      {
        "name": "defaultSourceTriggerName",
        "sourceRepository": {
          "branch": "master",
          "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
          "sourceControlAuthProperties": null,
          "sourceControlType": "GitHub"
        },
        "sourceTriggerEvents": [
          "commit"
        ],
        "status": "Enabled"
      }
    ]
  },
  "type": "Microsoft.ContainerRegistry/registries/tasks"
}
```

## <a name="test-the-build-task"></a>빌드 작업 테스트

이제 빌드를 정의하는 작업이 있습니다. 빌드 파이프라인을 테스트하려면 [az acr task run][az-acr-task-run] 명령을 실행하여 빌드를 수동으로 트리거합니다.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

기본적으로 `az acr task run` 명령이 실행되면 로그 출력이 콘솔에 스트림됩니다.

```console
$ az acr task run --registry $ACR_NAME --name taskhelloworld

2018/09/17 22:51:00 Using acb_vol_9ee1f28c-4fd4-43c8-a651-f0ed027bbf0e as the home volume
2018/09/17 22:51:00 Setting up Docker configuration...
2018/09/17 22:51:02 Successfully set up Docker configuration
2018/09/17 22:51:02 Logging in to registry: myregistry.azurecr.io
2018/09/17 22:51:03 Successfully logged in
2018/09/17 22:51:03 Executing step: build
2018/09/17 22:51:03 Obtaining source code and scanning for dependencies...
2018/09/17 22:51:05 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  23.04kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
Status: Image is up to date for node:9-alpine
 ---> a56170f59699
Step 2/5 : COPY . /src
 ---> 5f574fcf5816
Step 3/5 : RUN cd /src && npm install
 ---> Running in b1bca3b5f4fc
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.078s
Removing intermediate container b1bca3b5f4fc
 ---> 44457db20dac
Step 4/5 : EXPOSE 80
 ---> Running in 9e6f63ec612f
Removing intermediate container 9e6f63ec612f
 ---> 74c3e8ea0d98
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 7382eea2a56a
Removing intermediate container 7382eea2a56a
 ---> e33cd684027b
Successfully built e33cd684027b
Successfully tagged myregistry.azurecr.io/helloworld:da2
2018/09/17 22:51:11 Executing step: push
2018/09/17 22:51:11 Pushing image: myregistry.azurecr.io/helloworld:da2, attempt 1
The push refers to repository [myregistry.azurecr.io/helloworld]
4a853682c993: Preparing
[...]
4a853682c993: Pushed
[...]
da2: digest: sha256:c24e62fd848544a5a87f06ea60109dbef9624d03b1124bfe03e1d2c11fd62419 size: 1366
2018/09/17 22:51:21 Successfully pushed image: myregistry.azurecr.io/helloworld:da2
2018/09/17 22:51:21 Step id: build marked as successful (elapsed time in seconds: 7.198937)
2018/09/17 22:51:21 Populating digests for step id: build...
2018/09/17 22:51:22 Successfully populated digests for step id: build
2018/09/17 22:51:22 Step id: push marked as successful (elapsed time in seconds: 10.180456)
The following dependencies were found:
- image:
    registry: myregistry.azurecr.io
    repository: helloworld
    tag: da2
    digest: sha256:c24e62fd848544a5a87f06ea60109dbef9624d03b1124bfe03e1d2c11fd62419
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 68cdf2a37cdae0873b8e2f1c4d80ca60541029bf


Run ID: da2 was successful after 27s
```

## <a name="trigger-a-build-with-a-commit"></a>커밋을 사용하여 빌드 트리거

이제 작업을 수동으로 실행하여 테스트했으므로 소스 코드가 변경되면 해당 작업을 자동으로 트리거합니다.

먼저 [리포지토리][sample-repo]의 로컬 복제본이 포함된 디렉터리에 있는지 확인합니다.

```azurecli-interactive
cd acr-build-helloworld-node
```

다음으로, 다음 명령을 실행하여 새 파일을 만들고, 커밋하고, GitHub에 있는 리포지토리의 포크에 푸시합니다.

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master
```

`git push` 명령을 실행할 때 GitHub 자격 증명을 제공하라는 메시지가 표시될 수 있습니다. GitHub 사용자 이름을 제공하고, 이전에 암호에 대해 만든 PAT(개인용 액세스 토큰)를 입력합니다.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

커밋이 리포지토리에 푸시되면 ACR 작업에서 만든 웹후크가 실행되어 Azure Container Registry에서 빌드를 시작합니다. 현재 실행 중인 작업에 대한 로그를 표시하여 빌드 진행 상황을 확인하고 모니터링합니다.

```azurecli-interactive
az acr task logs --registry $ACR_NAME
```

출력은 다음과 비슷하며, 현재 실행 중이거나 마지막으로 실행된 작업을 보여 줍니다.

```console
$ az acr task logs --registry $ACR_NAME
Showing logs of the last created run.
Run ID: da4

[...]

Run ID: da4 was successful after 38s
```

## <a name="list-builds"></a>빌드 나열

ACR 작업에서 레지스트리에 대해 완료한 작업 실행 목록을 보려면 [az acr task list-runs][az-acr-task-list-runs] 명령을 실행합니다.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

이 명령의 출력은 다음과 비슷하게 표시됩니다. ACR 작업에서 실행한 실행이 표시되고, 가장 최근 작업에 대한 TRIGGER 열에 "Git Commit"(Git 커밋)이 표시됩니다.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="next-steps"></a>다음 단계

이 자습서에서는 소스 코드를 Git 리포지토리에 커밋할 때 작업을 사용하여 Azure에서 컨테이너 이미지 빌드를 자동으로 트리거하는 방법을 알아보았습니다. 컨테이너 이미지의 기본 이미지가 업데이트될 때 빌드를 트리거하는 작업을 만드는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

> [!div class="nextstepaction"]
> [기본 이미지 업데이트 시 빌드 자동화](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-login]: /cli/azure/reference-index#az-login



