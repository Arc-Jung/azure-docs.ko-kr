---
title: 보안 에이전트 인증 방법
description: IoT 서비스용 Defender를 사용할 때 사용할 수 있는 다양 한 인증 방법에 대해 알아봅니다.
ms.topic: conceptual
ms.date: 01/24/2021
ms.openlocfilehash: bd4d89d8664dae2421ff7f268b3710b5a906427b
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104784614"
---
# <a name="security-agent-authentication-methods"></a>보안 에이전트 인증 방법

이 문서에서는 AzureIoTSecurity agent와 함께 사용 하 여 IoT Hub 인증할 수 있는 다양 한 인증 방법을 설명 합니다.

IoT Hub에서 IoT 용 각 장치를 등록 하려면 Defender-마이크로 에이전트가 필요 합니다. 장치를 인증 하려면 IoT 용 Defender에서 두 가지 방법 중 하나를 사용할 수 있습니다. 기존 IoT 솔루션에 가장 적합 한 방법을 선택 합니다.

- SecurityModule 옵션
- 장치 옵션

## <a name="authentication-methods"></a>인증 방법

Defender에서 IoT AzureIoTSecurity agent에 대 한 두 가지 방법으로 인증을 수행할 수 있습니다.

- **Defender-마이크로 에이전트** 인증 모드<br>
에이전트는 장치 id와 독립적으로 Defender-IoT-마이크로 에이전트 id를 사용 하 여 인증 됩니다.
보안 에이전트가 Defender-마이크로 에이전트 (대칭 키만)를 통해 전용 인증 방법을 사용 하도록 하려면이 인증 유형을 사용 합니다.

- **장치** 인증 모드<br>
이 메서드에서 보안 에이전트는 먼저 장치 id를 사용 하 여 인증 합니다. 초기 인증 후에 IoT 에이전트의 Defender는 장치의 인증 데이터와 REST API를 사용 하 여 IoT Hub에 대 한 **REST** 호출을 수행 합니다. IoT 에이전트의 Defender는 IoT Hub에서 Defender-마이크로 에이전트 인증 방법과 데이터를 요청 합니다. 마지막 단계에서 IoT 용 Defender 에이전트는 IoT 용 Defender 모듈에 대 한 인증을 수행 합니다.

보안 에이전트에서 기존 장치 인증 방법 (자체 서명 된 인증서 또는 대칭 키)을 다시 사용 하려는 경우이 인증 유형을 사용 합니다.

를 구성 하는 방법을 알아보려면 [보안 에이전트 설치 매개 변수](#security-agent-installation-parameters) 를 참조 하세요.

## <a name="authentication-methods-known-limitations"></a>인증 방법 알려진 제한 사항

- **Securitymodule** 인증 모드에서는 대칭 키 인증만 지원 합니다.
- CA-Signed 인증서는 **장치** 인증 모드에서 지원 되지 않습니다.

## <a name="security-agent-installation-parameters"></a>보안 에이전트 설치 매개 변수

[보안 에이전트를 배포 하는](how-to-deploy-agent.md)경우 인증 정보를 인수로 제공 해야 합니다.
이러한 인수는 다음 표에 설명 되어 있습니다.

|Linux 매개 변수 이름 | Windows 매개 변수 이름 | 줄임 매개 변수 |Description|옵션|
|---------------------|---------------|---------|---------------|---------------|
|인증-id|AuthenticationIdentity|안 함|인증 id| **Securitymodule** 또는 **장치**|
|authentication-method|AuthenticationMethod|온 m|인증 방법|**SymmetricKey** 또는 **new-selfsignedcertificate**|
|파일 경로|FilePath|f|인증서 또는 대칭 키를 포함 하는 파일의 절대 전체 경로입니다.| |
|호스트 이름|HostName|hn|IoT Hub의 FQDN|예: ContosoIotHub.azure-devices.net|
|장치 id|DeviceId|di|디바이스 ID|예: MyDevice1|
|인증서-위치-종류|CertificateLocationKind|cl.exe|인증서 저장소 위치|**Localfile** 또는 **Store**|
|

보안 에이전트 설치 스크립트를 사용 하는 경우 다음 구성이 자동으로 수행 됩니다. 보안 에이전트 인증을 수동으로 편집 하려면 구성 파일을 편집 합니다.

## <a name="change-authentication-method-after-deployment"></a>배포 후 인증 방법 변경

설치 스크립트를 사용 하 여 보안 에이전트를 배포 하는 경우 구성 파일이 자동으로 생성 됩니다.

배포 후 인증 방법을 변경 하려면 구성 파일을 수동으로 편집 해야 합니다.

### <a name="c-based-security-agent"></a>C # 기반 보안 에이전트

다음 매개 변수를 사용 하 여 _Authentication.config_ 를 편집 합니다.

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>C 기반 보안 에이전트

다음 매개 변수를 사용 하 여 _LocalConfiguration.js를_ 편집 합니다.

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>참고 항목

- [보안 에이전트 개요](security-agent-architecture.md)
- [보안 에이전트 배포](how-to-deploy-agent.md)
- [원시 보안 데이터 액세스](how-to-security-data-access.md)
