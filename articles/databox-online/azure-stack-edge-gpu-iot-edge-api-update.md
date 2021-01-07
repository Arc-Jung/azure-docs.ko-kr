---
title: Azure Stack Edge 2021년 1월 업데이트 영향 | Microsoft Docs
description: 이 문서에서는 2021년 1월 업데이트를 설치한 후 IoT Edge 역할 관리에서 Azure Stack Edge 디바이스에 미치는 영향을 설명합니다.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 10/26/2020
ms.author: alkohli
ms.openlocfilehash: f16f33e9aadcc01427602a1bd81f81cb0710e4dd
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578742"
---
# <a name="iot-edge-role-management-changes-for-your-azure-stack-edge-device"></a>Azure Stack Edge 디바이스용 IoT Edge 역할 관리 변경 내용

Azure Stack Edge 디바이스용 Azure IoT Edge 역할 관리에서는 2021년 1월 릴리스로 예약된 업데이트 버전의 API, SDK 및 Azure PowerShell을 사용합니다. 

이 문서에서는 이 최신 버전을 사용하는 경우 변경해야 하는 사항에 대해 자세히 설명합니다.

2021년 1월 업데이트는 Azure Stack Edge Pro – GPU, Azure Stack Edge Pro R 및 Azure Stack Edge Mini R 디바이스에만 사용할 수 있습니다. 이 문서의 정보는 이러한 디바이스에만 적용됩니다.

> [!NOTE]
> 2021년 1월 버전으로 업그레이드할 필요가 없습니다. 현재 버전을 계속 사용하도록 선택하더라도 IoT Edge 역할 관리에 영향을 주지 않습니다. 그러나 새로운 기능을 활용하고 보안 위험을 줄이려면 최신 버전을 설치하는 것이 좋습니다. 

## <a name="iot-edge-role-management-changes"></a>IoT Edge 역할 관리 변경 내용

선택적인 2021년 1월 업데이트가 Azure Stack Edge 디바이스에 설치되면 최신 버전의 API, SDK 및 PowerShell cmdlet을 IoT Edge 역할 관리에 사용해야 합니다.

2021년 1월 업데이트를 적용하는 경우에만 다음과 같이 변경해야 합니다.

- 현재 역할 관리 API 버전&nbsp;2019-08-01을 사용하고 있는 경우 2021년 1월에 릴리스되는 API 버전으로 업그레이드합니다. 
- SDK 버전&nbsp;1.0.0을 통해 역할 관리를 사용하고 있는 경우 2021년 1월에 릴리스되는 버전으로 업그레이드합니다.
- `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` 또는 `Remove-AzStackEdgeRole`과 같은 Azure PowerShell cmdlet(미리 보기)에서 역할 관리를 사용하고 있는 경우 새 cmdlet이 2021년 2월에 릴리스될 때까지 기다려야 합니다.

## <a name="api-usage"></a>API 사용

현재 API를 통해 IoT Edge 역할 관리를 수행하는 경우 나중에 게시되는 새 API 버전 2020-12-01을 사용해야 합니다. 현재 역할 API를 사용하고 있고 예정된 디바이스 소프트웨어 버전이 설치되면 PUT, GET, DELETE Kubernetes 역할로 이동한 다음, PUT IoT 추가 기능 API로 이동해야 합니다.

### <a name="for-the-put-method"></a>PUT 메서드의 경우

#### <a name="the-current-http-request"></a>현재 HTTP 요청 

- API 호출은 'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01 ' URI에서 수행됩니다.

- 요청 본문은 다음과 같습니다.

    ```json
    {
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        }
    }
    ```

#### <a name="the-upcoming-http-request"></a>예정된 HTTP 요청 

- Kubernetes 역할에 대한 API 호출은 다음 URI에서 수행됩니다. 

  'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1?api-version=2020-12-01'

    요청 본문은 다음과 같습니다.

    ```json
    {
        "kind": "Kubernetes",
        "properties": {
            "hostPlatform": "Linux",
            "kubernetesClusterInfo": {
                "version": "v1.17.3"
            },
            "kubernetesRoleResources": {
                "storage": {
                    "endpoints": []
                },
                "compute": {
                    "vmProfile": "DS1_v2"
                }
            }
        }
    }
    ```

- IoT Edge 추가 기능에 대한 API 호출은 다음 URI에서 수행됩니다. 

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

    요청 본문은 다음과 같습니다.

    ```json
    {
        "kind": "IoT",
        "properties": {
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            }
        }
    }
    ```


### <a name="for-the-get-method"></a>GET 메서드의 경우

#### <a name="the-current-http-response"></a>현재 HTTP 응답

- API 호출은 다음 URI에서 수행됩니다. 

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01'

- 응답 본문은 다음과 같습니다.

    ```json
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1",
        "name": "IoTRole1",
        "type": "dataBoxEdgeDevices/roles"
    }    
    ```

#### <a name="the-upcoming-http-response"></a>예정된 HTTP 응답

- API 호출은 다음 URI에서 수행됩니다.  

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

- 응답 본문은 다음과 같습니다. 

    ```json
    {
        "kind": "IoT",
        "properties": {
            "provisioningState": "Creating",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "version": "0.1.0-beta10"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/res1/roles/kubernetesRole/addons/iotName",
        "name": " iotName",
        "type": "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addon",
    }
    ```

### <a name="for-the-delete-method"></a>DELETE 메서드의 경우

### <a name="the-current-api-calls"></a>현재 API 호출

API 호출은 다음 URI에서 수행됩니다.  

'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01'

### <a name="the-upcoming-api-calls"></a>예정된 API 호출

API 호출은 다음 URI에서 수행됩니다.  

'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

## <a name="sdk-usage"></a>SDK 사용

SDK를 사용하고 있고 2021년 1월 업데이트가 설치되면 다음 샘플과 같이 IoT Edge 역할을 설정하는 방법을 변경해야 합니다. 그런 다음, 다음과 같이 예정된 NuGet 패키지를 다운로드하고 설치하여 새 SDK로 이동해야 합니다.

### <a name="the-current-sdk-sample"></a>현재 SDK 샘플

```csharp
var iotRoleStatus = "Enabled";
var iotHostPlatform = "Linux";
var id = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/iotrole"; 
var name = "iotrole";
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var iotRoleName = "iotrole";
var ioTDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeAgentInfo = new IoTEdgeAgentInfo(...);
var shareMappings = new List<MountPointMap>(...);

var role = new IoTRole(roleStatus, 
    hostPlatform, 
    shareMappings, 
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    id, 
    name, 
    type);

DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, iotRoleName, role, resourceGroup);
```


### <a name="the-new-sdk-sample"></a>새 SDK 샘플

```csharp
var k8sRoleStatus = "Enabled";
var k8sHostPlatform = "Linux";
var k8sId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole"; 
var k8sRoleName = "KubernetesRole";
var k8sClusterVersion = "v1.17.3"; //Final values will be updated here around January 2021
var k8sVmProfile = "DS1_v2"; //Final values will be updated here around January 2021
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var k8sRole = new KubernetesRole(
    roleStatus,
    hostPlatform,
    shareMappings,
    k8sClusterVersion,
    k8sVmProfile,
    k8sId,
    k8sRoleName,
    type
);
DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, k8sRoleName, k8sRole, resourceGroup); //Final usage will be updated here around January 2021

var ioTId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole/addons/iotaddon";
var ioTAddonName = "iotaddon";
var ioTAddonType = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addons";
var addon = new IoTAddon(
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    ioTId, 
    ioTAddonName, 
    ioTAddonType);
DataBoxEdgeManagementClient.AddOns.CreateOrUpdate(deviceName, k8sRoleName, addonName, addon, resourceGroup); //Final usage will be updated here around January 2021
```

## <a name="cmdlet-usage"></a>cmdlet 사용

현재 `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` 또는 `Remove-AzStackEdgeRole` cmdlet을 사용하고 있는 경우 2021년 2월 릴리스로 예정된 새 버전을 기다려야 합니다.

## <a name="frequently-asked-questions"></a>자주 묻는 질문

**Azure Stack Edge Pro – FPGA를 사용하고 있습니다. 2021년 1월 업데이트가 FPGA 모델에 영향을 주나요?**

아니요. 2021년 1월 업데이트는 Azure Stack Edge Pro - FPGA, Azure Stack Edge Pro R 및 Azure Stack Edge Mini R 디바이스에만 적용됩니다. Azure Stack Edge Pro – FPGA는 이 업데이트의 영향을 받지 않으며, IoT Edge 역할 관리를 변경할 필요가 없습니다.

**2021년 1월에 Azure Stack Edge Pro - GPU를 새 디바이스 소프트웨어로 업데이트하면 기존 서비스 중 어떤 것이 영향을 받나요?**

아니요. 2021년 1월 디바이스 업데이트가 설치되더라도 구성된 서비스에 영향을 주지 않습니다.

**IoT Edge 관리 API, SDK 또는 cmdlet에 대한 개략적인 변경 내용은 무엇인가요?**

IoT Edge는 Kubernetes 역할의 추가 기능입니다. 이는 먼저 Kubernetes가 구성되어 있는지 확인한 다음, IoT Edge 구성을 수행해야 함을 의미합니다.


## <a name="next-steps"></a>다음 단계

- [업데이트를 적용하는 방법 알아보기](azure-stack-edge-gpu-install-update.md)

