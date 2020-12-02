---
title: Azure IoT SDK 이해 | Microsoft Docs
description: 개발자 가이드 - 디바이스 및 백 엔드 앱을 빌드하는데 사용할 수 있는 다양한 Azure IoT 디바이스 및 서비스 SDK에 대한 링크 정보입니다.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/14/2020
ms.custom:
- mqtt
- 'Role: IoT Device'
- 'Role: Cloud Development'
ms.openlocfilehash: 4e914b7b49393964f17cc6d9515cb2f4a3bebd2e
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499988"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Azure IoT Hub SDK 이해 및 사용

IoT Hub를 사용하기 위한 SDK(소프트웨어 개발 키트)의 범주는 두 가지입니다.

* **IoT Hub 장치 sdk** 를 사용 하면 장치 클라이언트 또는 모듈 클라이언트를 사용 하 여 IoT 장치에서 실행 되는 앱을 빌드할 수 있습니다. 이러한 앱은 IoT Hub로 원격 분석 데이터를 보내고, 필요에 따라 IoT Hub에서 메시지, 작업, 메서드, 쌍 업데이트를 받습니다.  모듈 클라이언트를 사용하여 [Azure IoT Edge 런타임](../iot-edge/about-iot-edge.md)용 [모듈](../iot-edge/iot-edge-modules.md)을 작성할 수도 있습니다.

* **IoT Hub 서비스 sdk** 를 사용 하면 iot Hub를 관리 하는 백 엔드 응용 프로그램을 구축 하 고, 선택적으로 메시지를 보내거나 작업을 예약 하 고, 직접 메서드를 호출 하거나, 원하는 속성 업데이트를 iot 장치 또는 모듈에 보낼 수 있습니다.

또한 [디바이스 프로비저닝 서비스](../iot-dps/about-iot-dps.md)와 함께 작동하는 SDK 세트도 제공됩니다.

* **프로비저닝 디바이스 SDK** 를 사용하면 IoT 디바이스에서 실행되는 앱을 빌드하여 디바이스 프로비저닝 서비스와 통신할 수 있습니다.

* **프로비저닝 서비스 SDK** 를 사용하면 백엔드 애플리케이션을 빌드하여 디바이스 프로비저닝 서비스의 등록을 관리할 수 있습니다.

[Azure IoT SDK를 사용한 개발의 이점](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/)을 알아봅니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="os-platform-and-hardware-compatibility"></a>OS 플랫폼 및 하드웨어 호환성

SDK에 지원되는 플랫폼은 [Azure IoT SDK 플랫폼 지원](iot-hub-device-sdk-platform-support.md)에서 찾을 수 있습니다.

특정 하드웨어 디바이스와 SDK 호환성에 대한 자세한 내용은 [IoT용 Azure Certified 디바이스 카탈로그](https://catalog.azureiotsolutions.com/) 또는 개별 리포지토리를 참조하세요.

## <a name="azure-iot-hub-device-sdks"></a>Azure IoT Hub 디바이스 SDK

Microsoft Azure IoT 디바이스 SDK에는 Azure IoT Hub 서비스에 연결되고 Azure IoT Hub 서비스에서 관리하는 애플리케이션의 빌드를 용이하게 하는 코드가 포함되어 있습니다.

.NET용 Azure IoT Hub 디바이스 SDK: 

* [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)에서 다운로드 합니다.  네임스페이스는 IoT Hub 디바이스 클라이언트(DeviceClient, ModuleClient)를 포함하는 Microsoft.Azure.Devices.Clients입니다.
* [소스 코드](https://github.com/Azure/azure-iot-sdk-csharp)
* [API 참조](/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [모듈 참조](/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)


임베디드 C에 대 한 Azure IoT Hub 장치 SDK (ANSI C-C99):
* [임베디드 C SDK 빌드](https://github.com/Azure/azure-sdk-for-c/tree/master/sdk/docs/iot#build)
* [소스 코드](https://github.com/Azure/azure-sdk-for-c)
* 제한 된 장치에 대 한 [차트 크기 조정](https://github.com/Azure/azure-sdk-for-c/tree/master/sdk/docs/iot#size-chart)
* [API 참조](https://azuresdkdocs.blob.core.windows.net/$web/c/docs/1.0.0/index.html)


C(ANSI C - C99)용 Azure IoT Hub 디바이스 SDK:

* [apt-get, MBED, Arduino IDE 또는 iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)에서 설치
* [소스 코드](https://github.com/Azure/azure-iot-sdk-c)
* [C 디바이스 SDK 컴파일](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compiling-the-c-device-sdk)
* [API 참조](/azure/iot-hub/iot-c-sdk-ref/)
* [모듈 참조](/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h)
* [다른 플랫폼에 C SDK 이식](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
* 교차 컴파일, 여러 플랫폼에서 시작하기 등에 대한 정보를 제공하는 [개발자 설명서](https://github.com/Azure/azure-iot-sdk-c/tree/master/doc)
* [Azure IoT Hub C SDK 리소스 소비 정보](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md)

Java용 Azure IoT Hub 디바이스 SDK:

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) 프로젝트에 추가
* [소스 코드](https://github.com/Azure/azure-iot-sdk-java)
* [API 참조](/java/api/com.microsoft.azure.sdk.iot.device)
* [모듈 참조](/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)

Node.js용 Azure IoT Hub 디바이스 SDK:

* [npm](https://www.npmjs.com/package/azure-iot-device)에서 설치
* [소스 코드](https://github.com/Azure/azure-iot-sdk-node)
* [API 참조](/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [모듈 참조](/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Python용 Azure IoT Hub 디바이스 SDK:

* [pip](https://pypi.org/project/azure-iot-device/)에서 설치
* [소스 코드](https://github.com/Azure/azure-iot-sdk-python)
* [API 참조](/python/api/azure-iot-device)

iOS용 Azure IoT Hub 디바이스 SDK:

* [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)에서 설치
* [샘플](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API 참조: [C API 참조](/azure/iot-hub/iot-c-sdk-ref/) 참조

## <a name="azure-iot-hub-service-sdks"></a>Azure IoT Hub 서비스 SDK

Azure IoT 서비스 SDK에는 디바이스와 보안을 관리하기 위해 IoT Hub 서비스를 직접 조작하는 애플리케이션의 빌드를 용이하게 하는 코드가 포함되어 있습니다.

.NET용 Azure IoT Hub 서비스 SDK:

* [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices/)에서 다운로드 합니다.  네임스페이스는 IoT Hub 서비스 클라이언트(RegistryManager, ServiceClients)를 포함하는 Microsoft.Azure.Devices입니다.
* [소스 코드](https://github.com/Azure/azure-iot-sdk-csharp)
* [API 참조](/dotnet/api/microsoft.azure.devices)

Java용 Azure IoT Hub 서비스 SDK:

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) 프로젝트에 추가
* [소스 코드](https://github.com/Azure/azure-iot-sdk-java)
* [API 참조](/java/api/com.microsoft.azure.sdk.iot.service)

Node.js용 Azure IoT Hub 서비스 SDK:

* [npm](https://www.npmjs.com/package/azure-iothub)에서 다운로드
* [소스 코드](https://github.com/Azure/azure-iot-sdk-node)
* [API 참조](/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Python용 Azure IoT Hub 서비스 SDK:

* [pip](https://pypi.python.org/pypi/azure-iot-hub/)에서 다운로드
* [소스 코드](https://github.com/Azure/azure-iot-sdk-python/tree/master)
* [API 참조](/python/api/azure-iot-hub)

C용 Azure IoT Hub 서비스 SDK:

C 용 Azure IoT 서비스 SDK는 더 이상 활성 개발에 적용 되지 않습니다.
충돌, 데이터 손상 및 보안 취약성과 같은 중요 한 버그를 계속 수정 합니다. 그러나 중요 하지 않은 새 기능은 추가 하거나 버그를 수정 하지는 않습니다.

Azure IoT 서비스 SDK 지원은 고급 언어 ([c #](https://github.com/Azure/azure-iot-sdk-csharp), [Java](https://github.com/Azure/azure-iot-sdk-java), [Node](https://github.com/Azure/azure-iot-sdk-node), [Python](https://github.com/Azure/azure-iot-sdk-python))에서 사용할 수 있습니다.

* [Apt-get, MBED, ARDUINO IDE 또는 NuGet](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md) 에서 다운로드
* [소스 코드](https://github.com/Azure/azure-iot-sdk-c)

iOS용 Azure IoT Hub 서비스 SDK:

* [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)에서 설치
* [샘플](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> 언어 및 플랫폼 특정 패키지 관리자를 사용하여 개발 컴퓨터에서 이진 파일 및 종속성을 설치하는 방법에 대한 정보는 GitHub 리포지토리의 추가 정보 파일을 참조하세요.

## <a name="microsoft-azure-provisioning-sdks"></a>Microsoft Azure 프로비저닝 SDK

**Microsoft Azure 프로비저닝 SDK** 를 사용하면 [디바이스 프로비저닝 서비스](../iot-dps/about-iot-dps.md)를 통해 디바이스를 IoT Hub에 프로비전할 수 있습니다.

C#용 Azure 프로비저닝 디바이스 및 서비스 SDK:

* NuGet에서 [디바이스 SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Client/) 및 [서비스 SDK](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/)를 다운로드하세요.
* [소스 코드](https://github.com/Azure/azure-iot-sdk-csharp/)
* [API 참조](/dotnet/api/microsoft.azure.devices.provisioning.client?view=azure-dotnet)

C용 Azure 프로비저닝 디바이스 및 서비스 SDK:

* [apt-get, MBED, Arduino IDE 또는 iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)에서 설치
* [소스 코드](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [API 참조](/azure/iot-hub/iot-c-sdk-ref/)

Java용 Azure 프로비저닝 디바이스 및 서비스 SDK:

* [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) 프로젝트에 추가
* [소스 코드](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning)
* [API 참조](/java/api/com.microsoft.azure.sdk.iot.provisioning.device?view=azure-java-stable)

Node.js용 Azure 프로비저닝 디바이스 및 서비스 SDK:

* [소스 코드](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning)
* [API 참조](/javascript/api/overview/azure/iothubdeviceprovisioning?view=azure-node-latest)
* npm에서 [디바이스 SDK](https://badge.fury.io/js/azure-iot-provisioning-device) 및 [서비스 SDK](https://badge.fury.io/js/azure-iot-provisioning-service) 다운로드

Python용 Azure 프로비저닝 디바이스 및 서비스 SDK:

* [소스 코드](https://github.com/Azure/azure-iot-sdk-python)
* pip에서 [디바이스 SDK](https://pypi.org/project/azure-iot-device/) 및 [서비스 SDK](https://pypi.org/project/azure-iothub-provisioningserviceclient/) 다운로드

## <a name="next-steps"></a>다음 단계

Azure IoT SDK는 개발을 도와주는 도구 집합도 제공합니다.

* [iothub 진단](https://github.com/Azure/iothub-diagnostics): IoT Hub 연결 관련 문제를 진단할 수 있는 플랫폼 간 명령줄 도구입니다.
* [azure-iot-탐색기](https://github.com/Azure/azure-iot-explorer): IoT Hub에 연결 하 고 iot 장치를 추가/관리/통신 하기 위한 플랫폼 간 데스크톱 응용 프로그램입니다.

Azure IoT SDK를 사용한 개발과 관련된 문서

* IoT Hub 디바이스 SDK를 사용하여 [연결 및 신뢰할 수 있는 메시지를 관리하는 방법](iot-hub-reliability-features-in-sdks.md)에 대해 알아보세요.
* iOS 및 Android와 같은 [모바일 플랫폼용 개발](iot-hub-how-to-develop-for-mobile-devices.md) 방법에 대해 알아보세요.
* [Azure IoT SDK 플랫폼 지원](iot-hub-device-sdk-platform-support.md)

이 IoT Hub 개발자 가이드의 다른 참조 자료:

* [IoT Hub 엔드포인트](iot-hub-devguide-endpoints.md)
* [디바이스 쌍, 작업 및 메시지 라우팅에 대한 IoT Hub 쿼리 언어](iot-hub-devguide-query-language.md)
* [할당량 및 제한](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT 지원](iot-hub-mqtt-support.md)
* [IoT Hub REST API 참조](/rest/api/iothub/)
