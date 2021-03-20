---
title: API Management에서 클라이언트 인증서 인증을 사용 하 여 Api 보안
titleSuffix: Azure API Management
description: 클라이언트 인증서를 사용 하 여 Api에 대 한 액세스를 보호 하는 방법을 알아봅니다. 정책 식을 사용 하 여 들어오는 인증서의 유효성을 검사할 수 있습니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: apimpm
ms.openlocfilehash: 553b4527796db3e5d0f430afd6c5e614626187e5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99988891"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>API Management에서 클라이언트 인증서 인증을 사용하여 API를 보호하는 방법

API Management에서는 클라이언트 인증서를 사용하여 API에 대한 액세스(예: API Management에 대한 클라이언트)를 보호하는 기능을 제공합니다. 정책 식을 사용 하 여 들어오는 인증서의 유효성을 검사 하 고 원하는 값에 대해 인증서 속성을 확인할 수 있습니다.

클라이언트 인증서 (예: 백 엔드에 API Management)를 사용 하 여 API의 백 엔드 서비스에 대 한 액세스를 보호 하는 방법에 대 한 자세한 내용은 [클라이언트 인증서 인증을 사용 하 여 백 엔드 서비스를 보호 하는 방법](./api-management-howto-mutual-certificates.md) 을 참조 하세요.

> [!IMPORTANT]
> 개발자, 기본, 표준 또는 프리미엄 계층에서 HTTP/2를 통해 클라이언트 인증서를 수신 하 고 확인 하려면 아래와 같이 "사용자 지정 도메인" 블레이드에서 "클라이언트 인증서 협상" 설정을 켜야 합니다.

![클라이언트 인증서 협상](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

> [!IMPORTANT]
> 소비 계층에서 클라이언트 인증서를 수신 하 고 확인 하려면 아래와 같이 "사용자 지정 도메인" 블레이드에서 "클라이언트 인증서 요청" 설정을 켜야 합니다.

![클라이언트 인증서 요청](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>발급자 및 주체 확인

클라이언트 인증서의 발급자 및 주체를 확인하도록 아래 정책을 구성할 수 있습니다.

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> 인증서 해지 목록 확인을 사용 하지 않도록 설정 하려면 대신를 사용 `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` 합니다.
> 클라이언트 인증서가 자체 서명 된 경우 루트 또는 중간 CA 인증서를 [업로드](api-management-howto-ca-certificates.md) 하 여 API Management 하 `context.Request.Certificate.Verify()` 고 `context.Request.Certificate.VerifyNoRevocation()` 작동 해야 합니다.

## <a name="checking-the-thumbprint"></a>지문 확인

클라이언트 인증서의 지문을 확인하도록 아래 정책을 구성할 수 있습니다.

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "DESIRED-THUMBPRINT-IN-UPPER-CASE")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> 인증서 해지 목록 확인을 사용 하지 않도록 설정 하려면 대신를 사용 `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` 합니다.
> 클라이언트 인증서가 자체 서명 된 경우 루트 또는 중간 CA 인증서를 [업로드](api-management-howto-ca-certificates.md) 하 여 API Management 하 `context.Request.Certificate.Verify()` 고 `context.Request.Certificate.VerifyNoRevocation()` 작동 해야 합니다.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>API Management에 업로드된 인증서에 대해 지문 확인

다음 예제에서는 API Management에 업로드된 인증서에 대해 클라이언트 인증서의 지문을 확인하는 방법을 보여 줍니다.

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> 인증서 해지 목록 확인을 사용 하지 않도록 설정 하려면 대신를 사용 `context.Request.Certificate.VerifyNoRevocation()` `context.Request.Certificate.Verify()` 합니다.
> 클라이언트 인증서가 자체 서명 된 경우 루트 또는 중간 CA 인증서를 [업로드](api-management-howto-ca-certificates.md) 하 여 API Management 하 `context.Request.Certificate.Verify()` 고 `context.Request.Certificate.VerifyNoRevocation()` 작동 해야 합니다.

> [!TIP]
> 이 [문서](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) 에서 설명 하는 클라이언트 인증서 교착 상태 문제는 여러 가지 방법으로 매니페스트 될 수 있습니다. 예를 들어, 요청 중지, `403 Forbidden` 시간 초과 후 상태 코드의 요청 결과 `context.Request.Certificate` 는가 `null` 됩니다. 이 문제는 일반적 `POST` 으로 `PUT` 콘텐츠 길이가 약 60kb 이상인 요청에 영향을 줍니다.
> 이 문제가 발생 하지 않도록 하려면이 문서의 첫 번째 이미지에 표시 된 것 처럼 "사용자 지정 도메인" 블레이드에서 원하는 호스트 이름에 대 한 "클라이언트 인증서 협상" 설정을 설정 합니다. 소비 계층에서는이 기능을 사용할 수 없습니다.

## <a name="certificate-validation-in-self-hosted-gateway"></a>자체 호스팅 게이트웨이의 인증서 유효성 검사

기본 API Management [자체 호스팅 게이트웨이](self-hosted-gateway-overview.md) 이미지는 API Management 인스턴스에 업로드 된 [CA 루트 인증서](api-management-howto-ca-certificates.md) 를 사용 하 여 서버 및 클라이언트 인증서의 유효성 검사를 지원 하지 않습니다. 자체 호스팅 게이트웨이에 사용자 지정 인증서를 제공 하는 클라이언트에서 응답 속도가 느려질 수 있습니다. CRL (인증서 해지 목록) 유효성 검사는 게이트웨이에서 시간이 초과 되는 데 시간이 오래 걸릴 수 있기 때문입니다. 

게이트웨이를 실행 하는 경우에 대 한 해결 방법으로, API Management 인스턴스가 아닌 localhost 주소 (127.0.0.1)를 가리키도록 PKI IP 주소를 구성할 수 있습니다. 이로 인해 게이트웨이에서 클라이언트 인증서의 유효성을 검사 하려고 하면 CRL 유효성 검사가 신속 하 게 실패 합니다. 게이트웨이를 구성 하려면 컨테이너에 있는 파일의 localhost로 확인할 API Management 인스턴스에 대 한 DNS 항목을 추가 `/etc/hosts` 합니다. 게이트웨이 배포 중에이 항목을 추가할 수 있습니다.
 
* Docker 배포의 경우 `--add-host <hostname>:127.0.0.1` 매개 변수를 명령에 추가 `docker run` 합니다. 자세한 내용은 [컨테이너 호스트 파일에 항목 추가](https://docs.docker.com/engine/reference/commandline/run/#add-entries-to-container-hosts-file---add-host) 를 참조 하세요.
 
* Kubernetes 배포의 경우- `hostAliases` 구성 파일에 사양을 추가 `myGateway.yaml` 합니다. 자세한 내용은 [호스트 별칭을 사용 하 여 Pod/etc/hosts에 항목 추가](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/)를 참조 하세요.




## <a name="next-steps"></a>다음 단계

-   [클라이언트 인증서 인증을 사용하여 백 엔드 서비스를 보호하는 방법](./api-management-howto-mutual-certificates.md)
-   [인증서 업로드 방법](./api-management-howto-mutual-certificates.md)
