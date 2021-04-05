---
title: 자습서 - Azure Service Fabric Mesh에서 실행되는 앱 업그레이드
description: 이 자습서에서는 Service Fabric Mesh에서 실행되는 Service Fabric 애플리케이션을 업그레이드하는 방법을 알아봅니다.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter, devx-track-azurecli
ms.openlocfilehash: 8a71e854f03bee75b757e0a0aa02e7aa2c24469b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99821314"
---
# <a name="tutorial-upgrade-a-service-fabric-application-running-in-service-fabric-mesh"></a>자습서: Service Fabric Mesh에서 실행되는 Service Fabric 애플리케이션 업그레이드

> [!IMPORTANT]
> Azure Service Fabric Mesh의 미리 보기가 사용 중지되었습니다. 새 배포는 더이상 Service Fabric Mesh API를 통해 허용되지 않습니다. 기존 배포에 대한 지원은 2021년 4월 28일까지 계속됩니다.
> 
> 자세한 내용은 [Azure Service Fabric Mesh 미리 보기 사용 중지](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/)를 참조하세요.

이 자습서는 시리즈의 3부입니다. 할당된 CPU 리소스를 늘려서 [Service Fabric Mesh에 이전에 배포한](service-fabric-mesh-tutorial-template-deploy-app.md) Service Fabric 애플리케이션을 업그레이드하는 방법을 알아봅니다.  이 과정을 완료하면 웹 프런트 엔드 서비스가 늘어난 CPU 리소스로 실행됩니다.

시리즈 3부에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 애플리케이션 구성 변경
> * Service Fabric Mesh에서 실행되는 애플리케이션 업그레이드

이 자습서 시리즈에서는 다음 방법에 대해 알아봅니다.
> [!div class="checklist"]
> * [템플릿을 사용하여 Service Fabric Mesh에 애플리케이션 배포](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Service Fabric Mesh에서 실행 중인 애플리케이션 크기 조정](service-fabric-mesh-tutorial-template-scale-services.md)
> * Service Fabric Mesh에서 실행되는 애플리케이션 업그레이드
> * [애플리케이션 제거](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 시작하기 전에:

* Azure 구독이 아직 없으면 시작하기 전에 [무료 계정을 만들](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 수 있습니다.

* [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md)을 열거나 [Azure CLI 및 Service Fabric Mesh CLI를 로컬에 설치](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli)합니다.

## <a name="upgrade-application-configurations"></a>애플리케이션 구성 업그레이드

Service Fabric Mesh에 애플리케이션을 배포하여 얻을 수 있는 주요 이점 중 하나는 애플리케이션 구성을 쉽게 업데이트할 수 있는 기능입니다.  예를 들어, 서비스의 CPU 또는 메모리 리소스를 쉽게 업데이트할 수 있습니다.

이 자습서에서는 [이전에 배포되어](service-fabric-mesh-tutorial-template-deploy-app.md) 현재 실행 중인 할 일 목록 샘플을 예제로 사용합니다. 애플리케이션에 WebFrontEnd 및 ToDoService라는 두 가지 서비스가 있습니다. 각 서비스는 처음에 CPU 리소스 0.5로 배포되었습니다.  WebFrontEnd 서비스에 대한 CPU 리소스를 보려면 다음을 실행합니다.

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

애플리케이션 리소스에 대한 배포 템플릿에서, 각 서비스에는 요청된 CPU 리소스를 설정하는 데 사용할 수 있는 *cpu* 속성이 있습니다. 애플리케이션은 함께 배포되고 관리되는 여러 서비스로 구성될 수 있으며, 각 서비스에는 고유한 *cpu* 설정이 있습니다. 웹 프런트 엔드 서비스의 CPU 리소스를 늘리려면 배포 템플릿이나 매개 변수 파일에서 *cpue* 값을 수정합니다.  그런 다음, 애플리케이션을 업그레이드합니다.

### <a name="modify-the-deployment-template-parameters"></a>배포 템플릿 매개 변수 수정

애플리케이션을 배포한 후에 템플릿의 값이 변경될 것으로 예상되거나 배포별로 값을 변경하는 옵션을 포함하려는 경우(다른 배포에 이 템플릿을 다시 사용하려는 경우) 가장 좋은 방법은 값을 매개 변수화하는 것입니다.

이전에 [mesh_rp.windows.json 배포 템플릿](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json)과 [mesh_rp.windows.parameter.json 매개 변수](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) 파일을 사용하여 애플리케이션을 배포했습니다.

로컬에서 [mesh_rp.windows.parameter.json 매개 변수](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) 파일을 열고 *frontEndCpu* 값을 1로 설정합니다.

```json
      "frontEndCpu":{
        "value": "1"
      }
```

변경 내용을 매개 변수 파일에 저장합니다.  

*frontEndCpu* 매개 변수는 [mesh_rp.windows.json 배포 템플릿](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json)의 *parameters* 섹션에 선언되어 있습니다.

```json
"frontEndCpu": {
    "defaultValue": "0.5",
    "type": "string",
    "metadata": {
        "description": "The CPU resources for the front end web service."
    }
}
```

WebFrontEnd 서비스 *codePackages->resources->requests->cpu* 속성은 *frontEndCpu* 매개 변수를 참조합니다.

```json
    "services": [
          {
            "name": "WebFrontEnd",
            "properties": {
              "description": "WebFrontEnd description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "WebFrontEnd",
                  "image": "[parameters('frontEndImage')]",
                  ...
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              ...
```

### <a name="upgrade-your-application"></a>애플리케이션 업그레이드

애플리케이션이 실행되는 동안 템플릿 및 업데이트된 매개 변수 파일을 다시 배포하여 애플리케이션을 업그레이드할 수 있습니다.

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

그러면 애플리케이션에 대한 롤링 업그레이드가 시작되고 몇 분 안에 CPU 리소스가 증가하는 것을 볼 수 있습니다.  WebFrontEnd 서비스에 대한 CPU 리소스를 보려면 다음을 실행합니다.

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

## <a name="next-steps"></a>다음 단계

자습서의 이 부분에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 애플리케이션 구성 변경
> * Service Fabric Mesh에서 실행되는 애플리케이션 업그레이드

다음 자습서를 진행합니다.
> [!div class="nextstepaction"]
> [Service Fabric Mesh 애플리케이션 제거](service-fabric-mesh-tutorial-template-remove-app.md)
