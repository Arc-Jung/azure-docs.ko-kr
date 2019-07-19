---
title: Azure Container Instances에서 비밀 볼륨 탑재
description: Container Instances에서 액세스할 수 있도록 중요한 정보를 저장하기 위해 비밀 볼륨을 탑재하는 방법을 알아봅니다.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: danlep
ms.openlocfilehash: 2e96ef73c3ff89fd7941fa14a8a1e53e6d4d8593
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325418"
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Azure Container Instances에서 비밀 볼륨 탑재

*비밀* 볼륨을 사용하여 중요한 정보를 컨테이너 그룹의 컨테이너에 제공할 수 있습니다. *비밀* 볼륨은 컨테이너 그룹의 컨테이너가 액세스할 수 있는 볼륨 내 파일에 비밀을 저장합니다. *비밀* 볼륨에 비밀을 저장하면 SSH 키 또는 데이터베이스 자격 증명 같은 중요한 데이터가 애플리케이션 코드에 추가되는 일을 방지할 수 있습니다.

모든 *비밀* 볼륨은 [tmpfs][tmpfs], RAM 지원 파일 시스템에 의해 지원 됩니다. 해당 내용은 비휘발성 저장소에 기록 되지 않습니다.

> [!NOTE]
> *비밀* 볼륨은 현재 Linux 컨테이너로 제한됩니다. [환경 변수 설정](container-instances-environment-variables.md)에서 Windows 및 Linux 컨테이너 모두에 대한 안전한 환경 변수를 전달하는 방법을 알아봅니다. Windows 컨테이너에 모든 기능을 제공 하기 위해 작업 하는 동안 [개요](container-instances-overview.md#linux-and-windows-containers)에서 현재 플랫폼 차이를 찾을 수 있습니다.

## <a name="mount-secret-volume---azure-cli"></a>비밀 볼륨 탑재 - Azure CLI

Azure CLI를 사용 하 여 하나 이상의 비밀이 있는 컨테이너를 배포 하려면 [az container create][az-container-create] 명령 `--secrets-mount-path` 에 `--secrets` 및 매개 변수를 포함 합니다. 이 예제에서는 `/mnt/secrets`에서 두 비밀 "mysecret1" 및 "mysecret2"로 구성된 *비밀* 볼륨을 탑재합니다.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name secret-volume-demo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --secrets mysecret1="My first secret FOO" mysecret2="My second secret BAR" \
    --secrets-mount-path /mnt/secrets
```

다음 [az container exec][az-container-exec] 출력은 실행 중인 컨테이너에서 셸을 열고, 비밀 볼륨 내에 파일을 나열 하 고, 콘텐츠를 표시 하는 방법을 보여 줍니다.

```console
$ az container exec --resource-group myResourceGroup --name secret-volume-demo --exec-command "/bin/sh"
/usr/src/app # ls -1 /mnt/secrets
mysecret1
mysecret2
/usr/src/app # cat /mnt/secrets/mysecret1
My first secret FOO
/usr/src/app # cat /mnt/secrets/mysecret2
My second secret BAR
/usr/src/app # exit
Bye.
```

## <a name="mount-secret-volume---yaml"></a>비밀 볼륨 탑재 - YAML

Azure CLI 및 [YAML 템플릿](container-instances-multi-container-yaml.md)을 사용하여 컨테이너 그룹을 배포할 수도 있습니다. 여러 컨테이너로 구성된 컨테이너 그룹을 배포할 때에는 YAML 템플릿을 사용하여 배포하는 것이 좋습니다.

YAML 템플릿을 사용하여 배포하는 경우 비밀 값은 템플릿에서 **Base64로 인코딩**되어야 합니다. 그러나 비밀 값은 컨테이너의 파일 내에서 일반 텍스트로 표시됩니다.

다음 YAML 템플릿은 `/mnt/secrets`에서 *비밀* 볼륨을 탑재하는 컨테이너 하나가 포함된 컨테이너 그룹을 정의합니다. 비밀 볼륨에는 두 개의 비밀 "mysecret1" 및 "mysecret2"가 있습니다.

```yaml
apiVersion: '2018-10-01'
location: eastus
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

Yaml 템플릿을 사용 하 여 배포 하려면 이전 yaml을 이라는 `deploy-aci.yaml`파일에 저장 한 다음 `--file` 매개 변수를 사용 하 여 [az container create][az-container-create] 명령을 실행 합니다.

```azurecli-interactive
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

## <a name="mount-secret-volume---resource-manager"></a>비밀 볼륨 탑재 - Resource Manager

CLI 및 YAML 배포 외에도, Azure [Resource Manager 템플릿](/azure/templates/microsoft.containerinstance/containergroups)을 사용하여 컨테이너 그룹을 배포할 수 있습니다.

먼저 템플릿의 `volumes`컨테이너 그룹의 배열`properties` 섹션을 채웁니다. Resource Manager 템플릿을 사용하여 배포하는 경우 비밀 값은 템플릿에서 **Base64로 인코딩**되어야 합니다. 그러나 비밀 값은 컨테이너의 파일 내에서 일반 텍스트로 표시됩니다.

다음으로 *secret* 볼륨을 탑재하려는 컨테이너 그룹에 있는 각 컨테이너의 경우 컨테이너 정의의 `properties` 섹션에서 `volumeMounts` 배열을 채웁니다.

다음 Resource Manager 템플릿은 `/mnt/secrets`에서 *비밀* 볼륨을 탑재하는 컨테이너 하나가 포함된 컨테이너 그룹을 정의합니다. 비밀 볼륨에는 두 개의 비밀 "mysecret1" 및 "mysecret2"가 있습니다.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->
[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

리소스 관리자 템플릿을 사용 하 여 배포 하려면 앞의 JSON을 이라는 `deploy-aci.json`파일에 저장 한 다음 `--template-file` 매개 변수를 사용 하 여 [az group deployment create][az-group-deployment-create] 명령을 실행 합니다.

```azurecli-interactive
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```

## <a name="next-steps"></a>다음 단계

### <a name="volumes"></a>볼륨

Azure Container Instances에서 다른 볼륨 유형을 탑재하는 방법을 알아봅니다.

* [Azure Container Instances를 사용하여 Azure 파일 공유 탑재](container-instances-volume-azure-files.md)
* [Azure Container Instances에서 emptyDir 볼륨 탑재](container-instances-volume-emptydir.md)
* [Azure Container Instances에서 gitRepo 볼륨 탑재](container-instances-volume-gitrepo.md)

### <a name="secure-environment-variables"></a>보안 환경 변수

컨테이너(Windows 컨테이너 포함)에 중요한 정보를 제공하는 또 다른 방법은 [보안 환경 변수](container-instances-environment-variables.md#secure-values)를 사용하는 것입니다.

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
