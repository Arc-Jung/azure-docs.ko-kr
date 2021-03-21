---
title: 코드 모범 사례에 해당 하는 Azure Service Fabric 인프라
description: Azure Service Fabric를 코드로 관리 하기 위한 모범 사례 및 디자인 고려 사항입니다.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: b765d92778df40caec0864dc6f547324216fdb07
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102611983"
---
# <a name="infrastructure-as-code"></a>코드 제공 인프라(Infrastructure as code)

프로덕션 시나리오에서 Resource Manager 템플릿을 사용하여 Azure Service Fabric 클러스터를 만듭니다. Resource Manager 템플릿은 리소스 속성을 더 효율적으로 제어하고 일관된 리소스 모델을 갖출 수 있도록 합니다.

Windows 및 Linux용 Resource Manager 템플릿 샘플은 [GitHub의 Azure 샘플](https://github.com/Azure-Samples/service-fabric-cluster-templates)에서 사용할 수 있습니다. 이러한 템플릿은 클러스터 템플릿의 시작점으로 사용할 수 있습니다. `azuredeploy.json` 및 `azuredeploy.parameters.json`을 다운로드하여 사용자 지정 요구 사항에 맞게 편집합니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

위에서 다운로드한 `azuredeploy.json` 및 `azuredeploy.parameters.json` 템플릿을 배포하려면 다음 Azure CLI 명령을 사용합니다.

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az deployment group create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

PowerShell을 사용하여 리소스 만들기

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzResourceGroup -Name $ResourceGroupName -Location $Location
New-AzResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Azure Service Fabric 리소스

Azure Resource Manager를 통해 Service Fabric 클러스터에 애플리케이션 및 서비스를 배포할 수 있습니다. 자세한 내용은 [애플리케이션 및 서비스를 Azure Resource Manager 리소스로 관리](./service-fabric-application-arm-resource.md)를 참조하세요. Resource Manager 템플릿 리소스에 포함할 Service Fabric 애플리케이션 특정 리소스에 대한 추천 사례는 다음과 같습니다.

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Azure Resource Manager를 사용하여 애플리케이션을 배포하려면 먼저 [Sfpkg](./service-fabric-package-apps.md#create-an-sfpkg) Service Fabric 애플리케이션 패키지를 만들어야 합니다. sfpkg를 만드는 방법을 보여 주는 Python 스크립트의 예제는 다음과 같습니다.

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(
    self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(
            root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="azure-virtual-machine-operating-system-automatic-upgrade-configuration"></a>Azure 가상 컴퓨터 운영 체제 자동 업그레이드 구성 
가상 컴퓨터를 업그레이드 하는 작업은 사용자가 시작한 작업 이므로 [가상 컴퓨터 확장 집합](service-fabric-patch-orchestration-application.md) 을 사용 하 여 Azure Service Fabric 클러스터의 자동 운영 체제 업그레이드 호스트 패치 관리를 사용 하는 것이 좋습니다. 패치 오케스트레이션 응용 프로그램은 azure에서 호스트 되는 경우 azure에서 poa를 호스트 하는 오버 헤드로 poa를 통해 가상 컴퓨터 운영 체제 자동 업그레이드를 선호 하는 일반적인 이유를 사용 하 여 azure에서 호스트 되는 경우에 사용할 수 있는 대체 솔루션입니다. 자동 OS 업그레이드를 사용 하도록 설정 하는 계산 가상 머신 확장 집합 리소스 관리자 템플릿 속성은 다음과 같습니다.

```json
"upgradePolicy": {
   "mode": "Automatic",
   "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade": true,
        "disableAutomaticRollback": false
    }
},
```
Service Fabric와 함께 자동 OS 업그레이드를 사용 하는 경우 새 OS 이미지는 Service Fabric에서 실행 되는 서비스의 고가용성을 유지 하기 위해 한 번에 하나의 업데이트 도메인에 롤오버 됩니다. Service Fabric에서 자동 OS 업그레이드를 활용하려면 실버 내구성 계층 이상을 사용하도록 클러스터가 구성되어야 합니다.

Windows 호스트 컴퓨터가 조정 되지 않은 업데이트: HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU를 시작 하지 않도록 하려면 다음 레지스트리 키가 false로 설정 되어 있는지 확인 합니다.

Windowsupdate.log 레지스트리 키를 false로 설정 하는 계산 가상 머신 확장 집합 리소스 관리자 템플릿 속성은 다음과 같습니다.
```json
"osProfile": {
        "computerNamePrefix": "{vmss-name}",
        "adminUsername": "{your-username}",
        "secrets": [],
        "windowsConfiguration": {
          "provisionVMAgent": true,
          "enableAutomaticUpdates": false
        }
      },
```

## <a name="azure-service-fabric-cluster-upgrade-configuration"></a>Azure Service Fabric 클러스터 업그레이드 구성
자동 업그레이드를 사용 하도록 설정 하는 Service Fabric 클러스터 리소스 관리자 템플릿 속성은 다음과 같습니다.
```json
"upgradeMode": "Automatic",
```
클러스터를 수동으로 업그레이드 하려면 클러스터 가상 컴퓨터에 cab/deb 배포를 다운로드 한 후 다음 PowerShell을 호출 합니다.
```powershell
Copy-ServiceFabricClusterPackage -Code -CodePackagePath <"local_VM_path_to_msi"> -CodePackagePathInImageStore ServiceFabric.msi -ImageStoreConnectionString "fabric:ImageStore"
Register-ServiceFabricClusterPackage -Code -CodePackagePath "ServiceFabric.msi"
Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <"msi_code_version">
```

## <a name="next-steps"></a>다음 단계

* Windows Server를 실행하는 VM 또는 컴퓨터에서 클러스터 만들기: [Windows Server용 서비스 패브릭 클러스터 만들기](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Linux를 실행하는 VM 또는 컴퓨터에서 클러스터 만들기: [Linux 클러스터 만들기](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* [Service Fabric 지원 옵션](service-fabric-support.md) 에 대 한 자세한 정보
