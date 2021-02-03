---
title: Azure API Management 인증 정책 | Microsoft Docs
description: Azure API Management에 사용할 수 있는 인증 정책에 대해 알아봅니다.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/27/2021
ms.author: apimpm
ms.openlocfilehash: 22d2960801cac2222f868c384a55b4bf436bc75b
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99492606"
---
# <a name="api-management-authentication-policies"></a>API Management 인증 정책
이 문서에서는 다음 API Management 정책에 대한 참조를 제공합니다. 정책의 추가 및 구성에 대한 자세한 내용은 [API Management 정책](./api-management-policies.md)을 참조하세요.

##  <a name="authentication-policies"></a><a name="AuthenticationPolicies"></a> 인증 정책

-   [기본 사용 인증](api-management-authentication-policies.md#Basic) - 기본 인증을 사용하여 백 엔드 서비스를 인증합니다.

-   [클라이언트 인증서 사용 인증](api-management-authentication-policies.md#ClientCertificate) - 클라이언트 인증서를 사용하여 백 엔드 서비스를 인증합니다.

-   [관리 id로 인증](api-management-authentication-policies.md#ManagedIdentity) -API Management 서비스에 대 한 [관리 id](../active-directory/managed-identities-azure-resources/overview.md) 를 사용 하 여 인증 합니다.

##  <a name="authenticate-with-basic"></a><a name="Basic"></a> 기본 사용 인증
 `authentication-basic` 정책을 사용하여 기본 인증을 사용하는 백 엔드 서비스를 인증합니다. 이 정책은 HTTP 권한 부여 헤더를 정책에서 제공한 자격 증명에 해당하는 값으로 효과적으로 설정합니다.

### <a name="policy-statement"></a>정책 문

```xml
<authentication-basic username="username" password="password" />
```

### <a name="example"></a>예제

```xml
<authentication-basic username="testuser" password="testpassword" />
```

### <a name="elements"></a>요소

|이름|Description|필수|
|----------|-----------------|--------------|
|인증-기본|루트 요소입니다.|예|

### <a name="attributes"></a>특성

|속성|Description|필수|기본값|
|----------|-----------------|--------------|-------------|
|사용자 이름|기본 자격 증명의 사용자 이름을 지정합니다.|예|해당 없음|
|password|기본 자격 증명의 비밀번호를 지정합니다.|예|해당 없음|

### <a name="usage"></a>사용량
 이 정책은 다음과 같은 정책 [섹션](./api-management-howto-policies.md#sections) 및 [범위](./api-management-howto-policies.md#scopes)에서 사용할 수 있습니다.

-   **정책 섹션:** inbound

-   **정책 범위:** 모든 범위

##  <a name="authenticate-with-client-certificate"></a><a name="ClientCertificate"></a> 클라이언트 인증서 사용 인증
 `authentication-certificate`클라이언트 인증서를 사용 하 여 백 엔드 서비스로 인증 하려면 정책을 사용 합니다. 인증서를 [API Management 먼저 설치](./api-management-howto-mutual-certificates.md) 하 고 해당 손도장 또는 인증서 ID (리소스 이름)로 식별 해야 합니다. 

> [!CAUTION]
> 인증서가 Azure Key Vault에 저장 된 인증서를 참조 하는 경우 인증서 ID를 사용 하 여 식별 합니다. 키 자격 증명 모음 인증서를 회전 하면 API Management의 지문이 변경 되 고, 지 문으로 식별 되는 경우 정책에서 새 인증서를 확인 하지 않습니다.

### <a name="policy-statement"></a>정책 문

```xml
<authentication-certificate thumbprint="thumbprint" certificate-id="resource name"/>
```

### <a name="examples"></a>예제

이 예제에서 클라이언트 인증서는 인증서 ID로 식별 됩니다.

```xml  
<authentication-certificate certificate-id="544fe9ddf3b8f30fb490d90f" />  
``` 

이 예제에서 클라이언트 인증서는 지 문으로 식별 됩니다.

```xml
<authentication-certificate thumbprint="CA06F56B258B7A0D4F2B05470939478651151984" />
```
이 예제에서는 클라이언트 인증서가 기본 제공 인증서 저장소에서 검색 되지 않고 정책에 설정 됩니다.

```xml
<authentication-certificate body="@(context.Variables.GetValueOrDefault<byte[]>("byteCertificate"))" password="optional-certificate-password" />
```

### <a name="elements"></a>요소  
  
|이름|Description|필수|  
|----------|-----------------|--------------|  
|인증-인증서|루트 요소입니다.|예|  
  
### <a name="attributes"></a>특성  
  
|속성|Description|필수|기본값|  
|----------|-----------------|--------------|-------------|  
|thumbprint|클라이언트 인증서에 대한 지문입니다.|또는 중 하나를 `thumbprint` `certificate-id` 제공 해야 합니다.|해당 없음|
|인증서 id|인증서 리소스 이름입니다.|또는 중 하나를 `thumbprint` `certificate-id` 제공 해야 합니다.|해당 없음|
|본문|클라이언트 인증서 (바이트 배열)입니다.|아니요|해당 없음|
|password|클라이언트 인증서의 암호입니다.|에 지정 된 인증서 `body` 가 암호로 보호 된 경우 사용 됩니다.|해당 없음|
  
### <a name="usage"></a>사용량  
 이 정책은 다음과 같은 정책 [섹션](./api-management-howto-policies.md#sections) 및 [범위](./api-management-howto-policies.md#scopes)에서 사용할 수 있습니다.  
  
-   **정책 섹션:** inbound  
  
-   **정책 범위:** 모든 범위  

##  <a name="authenticate-with-managed-identity"></a><a name="ManagedIdentity"></a> 관리 id를 사용 하 여 인증  
 `authentication-managed-identity`관리 id를 사용 하 여 백 엔드 서비스로 인증 하려면 정책을 사용 합니다. 이 정책은 기본적으로 관리 id를 사용 하 여 Azure Active Directory에서 지정 된 리소스에 액세스 하기 위한 액세스 토큰을 가져옵니다. 토큰을 성공적으로 가져오면 정책에서 `Authorization` 스키마를 사용 하 여 헤더의 토큰 값을 설정 합니다 `Bearer` .

시스템 할당 id와 여러 사용자 할당 id를 모두 사용 하 여 토큰을 요청할 수 있습니다. `client-id`를 제공 하지 않으면 시스템 할당 id로 간주 됩니다. `client-id`변수가 제공 된 경우 해당 사용자 할당 id에 대 한 토큰이 요청 됩니다 Azure Active Directory
  
### <a name="policy-statement"></a>정책 문  
  
```xml  
<authentication-managed-identity resource="resource" client-id="clientid of user-assigned identity" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>예제  
#### <a name="use-managed-identity-to-authenticate-with-a-backend-service"></a>관리 id를 사용 하 여 백 엔드 서비스 인증
```xml  
<authentication-managed-identity resource="https://graph.microsoft.com"/> 
```
```xml  
<authentication-managed-identity resource="https://management.azure.com/"/> <!--Azure Resource Manager-->
```
```xml  
<authentication-managed-identity resource="https://vault.azure.net"/> <!--Azure Key Vault-->
```
```xml  
<authentication-managed-identity resource="https://servicebus.azure.net/"/> <!--Azure Service Bus-->
```
```xml  
<authentication-managed-identity resource="https://storage.azure.com/"/> <!--Azure Blob Storage-->
```
```xml  
<authentication-managed-identity resource="https://database.windows.net/"/> <!--Azure SQL-->
```

```xml
<authentication-managed-identity resource="api://Client_id_of_Backend"/> <!--Your own Azure AD Application-->
```

#### <a name="use-managed-identity-and-set-header-manually"></a>관리 id 사용 및 수동으로 헤더 설정

```xml
<authentication-managed-identity resource="api://Client_id_of_Backend"
   output-token-variable-name="msi-access-token" ignore-error="false" /> <!--Your own Azure AD Application-->
<set-header name="Authorization" exists-action="override">
   <value>@("Bearer " + (string)context.Variables["msi-access-token"])</value>
</set-header>
```

#### <a name="use-managed-identity-in-send-request-policy"></a>전송 요청 정책에서 관리 id 사용
```xml  
<send-request mode="new" timeout="20" ignore-error="false">
    <set-url>https://example.com/</set-url>
    <set-method>GET</set-method>
    <authentication-managed-identity resource="ResourceID"/>
</send-request>
```

### <a name="elements"></a>요소  
  
|이름|Description|필수|  
|----------|-----------------|--------------|  
|인증 관리-id |루트 요소입니다.|예|  
  
### <a name="attributes"></a>특성  
  
|속성|Description|필수|기본값|  
|----------|-----------------|--------------|-------------|  
|리소스|문자열입니다. Azure Active Directory에 있는 대상 web API (보안 리소스)의 앱 ID입니다.|예|해당 없음|
|클라이언트 id|문자열입니다. Azure Active Directory에서 사용자 할당 id의 앱 ID입니다.|아니요|시스템이 할당 한 id|
|출력-토큰 변수-이름|문자열입니다. 토큰 값을 개체 형식으로 수신 하는 컨텍스트 변수의 이름입니다 `string` . |아니요|해당 없음|  
|ignore-error|부울. 로 설정 하면 `true` 액세스 토큰을 가져올 수 없는 경우에도 정책 파이프라인이 계속 실행 됩니다.|아니요|false|  
  
### <a name="usage"></a>사용량  
 이 정책은 다음과 같은 정책 [섹션](./api-management-howto-policies.md#sections) 및 [범위](./api-management-howto-policies.md#scopes)에서 사용할 수 있습니다.  
  
-   **정책 섹션:** inbound  
  
-   **정책 범위:** 모든 범위

## <a name="next-steps"></a>다음 단계
정책으로 작업하는 방법에 대한 자세한 내용은 다음을 참조하세요.

+ [API Management의 정책](api-management-howto-policies.md)
+ [API 변환](transform-api.md)
+ [정책 참조](./api-management-policies.md)(정책 문 및 해당 설정에 대한 전체 목록)
+ [정책 샘플](./policy-reference.md)