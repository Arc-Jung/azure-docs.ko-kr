---
title: Linux용 OpenSSL을 구성하는 방법
titleSuffix: Azure Cognitive Services
description: Linux 용 OpenSSL을 구성 하는 방법에 대해 알아봅니다.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: jhakulin
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a6225fec30a87ca0bbe57e414733bc21489f87ad
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104577447"
---
# <a name="configure-openssl-for-linux"></a>Linux용 OpenSSL 구성

1.9.0 이전에 Speech SDK 버전을 사용 하는 경우 [OpenSSL](https://www.openssl.org) 는 호스트 시스템 버전으로 동적으로 구성 됩니다. 이후 버전의 Speech SDK에서는 OpenSSL (버전 [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html))이 speech sdk의 핵심 라이브러리에 정적으로 연결 되어 있습니다.

연결을 보장 하려면 OpenSSL 인증서가 시스템에 설치 되어 있는지 확인 합니다. 다음 명령을 실행 합니다.
```bash
openssl version -d
```

Ubuntu/Debian 기반 시스템의 출력은 다음과 같아야 합니다.
```
OPENSSLDIR: "/usr/lib/ssl"
```

`certs`OPENSSLDIR 아래에 하위 디렉터리가 있는지 여부를 확인 합니다. 위의 예제에서는 `/usr/lib/ssl/certs` 입니다.

* 이 있고 `/usr/lib/ssl/certs` 개별 인증서 파일 (또는 확장명 포함)이 많으면 `.crt` `.pem` 추가 작업이 필요 하지 않습니다.

* OPENSSLDIR이이 아닌 다른 것 `/usr/lib/ssl` 이거나 여러 개별 파일 대신 단일 인증서 번들 파일이 있는 경우 인증서를 찾을 수 있는 위치를 나타내는 적절 한 SSL 환경 변수를 설정 해야 합니다.

## <a name="examples"></a>예제

- OPENSSLDIR은 `/opt/ssl` 입니다. `certs`또는 파일이 많은 하위 디렉터리가 `.crt` 있습니다 `.pem` .
`SSL_CERT_DIR` `/opt/ssl/certs` Speech SDK를 사용 하는 프로그램을 실행 하기 전에를 가리키도록 환경 변수를 설정 합니다. 예를 들면 다음과 같습니다.
```bash
export SSL_CERT_DIR=/opt/ssl/certs
```

- OPENSSLDIR은 `/etc/pki/tls` (RHEL/CentOS 기반 시스템에서와 같이)입니다. 예를 `certs` 들어 인증서 번들 파일이 있는 하위 디렉터리가 있습니다 `ca-bundle.crt` .
`SSL_CERT_FILE`SPEECH SDK를 사용 하는 프로그램을 실행 하기 전에 해당 파일을 가리키도록 환경 변수를 설정 합니다. 예를 들면 다음과 같습니다.
```bash
export SSL_CERT_FILE=/etc/pki/tls/certs/ca-bundle.crt
```

## <a name="certificate-revocation-checks"></a>인증서 해지 확인
Speech Service에 연결 하는 경우 speech SDK는 Speech Service에서 사용 하는 TLS 인증서가 해지 되지 않았는지 확인 합니다. 이 확인을 수행 하려면 Speech SDK가 Azure에서 사용 하는 인증 기관에 대 한 CRL 배포 지점의 액세스 권한이 필요 합니다. 가능한 CRL 다운로드 위치 목록은 [이 문서](https://docs.microsoft.com/azure/security/fundamentals/tls-certificate-changes)에서 찾을 수 있습니다. 인증서가 해지 되었거나 CRL을 다운로드할 수 없는 경우 Speech SDK가 연결을 중단 하 고 취소 된 이벤트를 발생 시킵니다.

음성 SDK를 사용 하는 네트워크가 CRL 다운로드 위치에 대 한 액세스를 허용 하지 않는 방식으로 구성 된 경우 crl 확인을 사용 하지 않도록 설정 하거나 CRL을 검색할 수 없는 경우 실패로 설정할 수 있습니다. 이 구성은 인식기 개체를 만드는 데 사용 되는 구성 개체를 통해 수행 됩니다.

CRL을 검색할 수 없는 경우 연결을 계속 하려면 속성 OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE 설정 합니다.

::: zone pivot="programming-language-csharp"

```csharp
config.SetProperty("OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE", "true");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
config->SetProperty("OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE", "true");
```

::: zone-end

::: zone pivot="programming-language-java"

```java
config.setProperty("OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE", "true");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
speech_config.set_property_by_name("OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE", "true")?
```

::: zone-end

::: zone pivot="programming-language-more"

```ObjectiveC
[config setPropertyTo:@"true" byName:"OPENSSL_CONTINUE_ON_CRL_DOWNLOAD_FAILURE"];
```

::: zone-end
"True"로 설정 하면 CRL을 검색 하려고 시도 하 고, 검색에 성공 하면 인증서가 해지를 확인 하 고, 검색에 실패 하면 연결을 계속할 수 있게 됩니다.

인증서 해지 검사를 완전히 사용 하지 않도록 설정 하려면 속성 OPENSSL_DISABLE_CRL_CHECK를 "true"로 설정 합니다.
::: zone pivot="programming-language-csharp"

```csharp
config.SetProperty("OPENSSL_DISABLE_CRL_CHECK", "true");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
config->SetProperty("OPENSSL_DISABLE_CRL_CHECK", "true");
```

::: zone-end

::: zone pivot="programming-language-java"

```java
config.setProperty("OPENSSL_DISABLE_CRL_CHECK", "true");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
speech_config.set_property_by_name("OPENSSL_DISABLE_CRL_CHECK", "true")?
```

::: zone-end

::: zone pivot="programming-language-more"

```ObjectiveC
[config setPropertyTo:@"true" byName:"OPENSSL_DISABLE_CRL_CHECK"];
```

::: zone-end


> [!NOTE]
> 또한 일부 Linux 배포판에는 TMP 또는 TMPDIR 환경 변수가 정의 되어 있지 않습니다. 이렇게 하면 해당 CRL이 만료 될 때까지 다시 사용할 수 있도록 디스크에 캐시 하지 않고 매번 음성 SDK가 CRL (인증서 해지 목록)을 다운로드 합니다. 초기 연결 성능을 향상 시키기 위해 [TMPDIR 라는 환경 변수를 만들고 선택한 임시 디렉터리의 경로로 설정할](https://help.ubuntu.com/community/EnvironmentVariables)수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Speech SDK 정보](speech-sdk.md)
