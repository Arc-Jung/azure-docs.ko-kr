---
title: 사용자 지정 정책에서 OAuth2 기술 프로필 정의
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C의 사용자 지정 정책에서 OAuth2 기술 프로필을 정의합니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 86ec7a5745a58546faf6f0ff15d6dc5f452baa88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184046"
---
# <a name="define-an-oauth2-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C 사용자 지정 정책에서 OAuth2 기술 프로필 정의

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C(Azure AD B2C)는 OAuth2 프로토콜 ID 공급자에 대한 지원을 제공합니다. OAuth2는 권한 부여 및 위임된 인증을 위한 기본 프로토콜입니다. 자세한 내용은 [RFC 6749 OAuth 2.0 권한 부여 프레임워크](https://tools.ietf.org/html/rfc6749)를 참조하세요. OAuth2 기술 프로필을 사용하면 Facebook과 같은 OAuth2 기반 ID 공급자와 페더럴할 수 있습니다. ID 공급자와 페더레이션을 사용하면 사용자가 기존 소셜 또는 엔터프라이즈 ID로 로그인할 수 있습니다.

## <a name="protocol"></a>프로토콜

**Protocol** 요소의 **Name** 특성은 `OAuth2`로 설정해야 합니다. 예를 들어 **Facebook-OAUTH** 기술 프로필의 프로토콜은 `OAuth2`입니다.

```XML
<TechnicalProfile Id="Facebook-OAUTH">
  <DisplayName>Facebook</DisplayName>
  <Protocol Name="OAuth2" />
  ...
```

## <a name="input-claims"></a>입력 클레임

**InputClaims** 및 **InputClaimsTransformations** 요소는 필요하지 않습니다. 그러나 ID 공급자에게 추가 매개 변수를 보내는 것이 좋습니다. 다음 예제는 값이 `contoso.com`인 **domain_hint** 쿼리 문자열 매개 변수를 권한 부여 요청에 추가합니다.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="domain_hint" DefaultValue="contoso.com" />
</InputClaims>
```

## <a name="output-claims"></a>출력 클레임

**OutputClaims** 요소는 OAuth2 ID 공급자가 반환한 클레임 목록을 포함합니다. 정책에 정의된 클레임 이름을 ID 공급자에 정의된 이름에 매핑해야 할 수도 있습니다. `DefaultValue` 특성만 설정하면, ID 공급자가 반환하지 않은 클레임도 포함할 수 있습니다.

**OutputClaimsTransformations** 요소는 출력 클레임을 수정하거나 새 출력 클레임을 생성하는 데 사용되는 **OutputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

다음 예제는 Facebook ID 공급자가 반환한 클레임을 보여 줍니다.

- **first_name** 클레임은 **givenName** 클레임에 매핑됩니다.
- **last** 클레임은 **surname** 클레임에 매핑됩니다.
- 이름 매핑없이 **표시 이름** 클레임입니다.
- 이름 매핑이 없는 **email** 클레임입니다.

기술 프로필은 ID 공급자가 반환하지 않은 클레임도 반환합니다.

- ID 공급자의 이름을 포함하는 **identityProvider** 클레임입니다.
- 기본값이 **socialIdpAuthentication**인 **authenticationSource** 클레임입니다.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="facebook.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>메타데이터

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| client_id | yes | ID 공급자의 애플리케이션 식별자입니다. |
| IdTokenAudience | 예 | id_token의 대상 그룹입니다. 지정된 경우 Azure AD B2C는 토큰이 ID 공급자에서 반환된 클레임에 있고 지정된 토큰과 같은지 확인합니다. |
| authorization_endpoint | yes | RFC 6749에 따른 권한 부여 엔드포인트의 URL입니다. |
| AccessTokenEndpoint | yes | RFC 6749에 따른 토큰 엔드포인트의 URL입니다. |
| ClaimsEndpoint | yes | RFC 6749에 따른 사용자 정보 엔드포인트의 URL입니다. |
| AccessTokenResponseFormat | 예 | 액세스 토큰 엔드포인트 호출의 형식입니다. 예를 들어 Facebook에는 HTTP GET 메서드가 필요하지만 액세스 토큰 응답은 JSON 형식입니다. |
| AdditionalRequestQueryParameters | 예 | 추가 요청 쿼리 매개 변수입니다. 예를 들어 ID 공급자에 추가 매개 변수를 보내려고 할 수 있습니다. 쉼표 구분 기호를 사용하여 여러 매개 변수를 포함할 수 있습니다. |
| ClaimsEndpointAccessTokenName | 예 | 액세스 토큰 쿼리 문자열 매개 변수의 이름입니다. 일부 ID 공급자의 클레임 엔드포인트가 GET HTTP 요청을 지원합니다. 이 경우 전달자 토큰은 권한 부여 헤더 대신 쿼리 문자열 매개 변수를 사용하여 전송됩니다. |
| ClaimsEndpointFormatName | 예 | 형식 쿼리 문자열 매개 변수의 이름입니다. 예를 들어 이 LinkedIn 클레임 엔드포인트 `https://api.linkedin.com/v1/people/~?format=json`에서 이름을 `format`으로 설정할 수 있습니다. |
| ClaimsEndpointFormat | 예 | 형식 쿼리 문자열 매개 변수의 값입니다. 예를 들어 이 LinkedIn 클레임 엔드포인트 `https://api.linkedin.com/v1/people/~?format=json`에서 값을 `json`으로 설정할 수 있습니다. |
| ProviderName | 예 | ID 공급자의 이름입니다. |
| response_mode | 예 | ID 공급자가 결과를 다시 Azure AD B2C에 보내는 데 사용하는 방법입니다. 가능한 값은 `query`, `form_post`(기본값) 또는 `fragment`입니다. |
| scope | 예 | OAuth2 ID 공급자 사양에 따라 정의된 요청의 범위입니다. 예를 들어 `openid`, `profile` 및 `email`입니다. |
| HttpBinding | 예 | 액세스 토큰 및 클레임 토큰 엔드포인트에 필요한 HTTP 바인딩입니다. 가능한 값은 `GET` 또는 `POST`입니다.  |
| ResponseErrorCodeParamName | 예 | HTTP 200 (Ok)을 통해 반환된 오류 메시지를 포함하는 매개 변수의 이름입니다. |
| ExtraParamsInAccessTokenEndpointResponse | 예 | 일부 ID 공급자가 **AccessTokenEndpoint**의 응답으로 반환할 수 있는 추가 매개 변수를 포함합니다. 예를 들어 **AccessTokenEndpoint**의 응답에는 **ClaimsEndpoint** 요청 쿼리 문자열에서 access_token 이외의 필수 매개 변수인 `openid`와 같은 추가 매개 변수가 포함됩니다. 여러 매개 변수 이름은 이스케이프되고 쉼표 ',' 구분 기호로 구분되어야 합니다. |
| ExtraParamsInClaimsEndpointRequest | 예 | 일부 ID 공급자가 **ClaimsEndpoint** 요청으로 반환할 수 있는 추가 매개 변수를 포함합니다. 여러 매개 변수 이름은 이스케이프되고 쉼표 ',' 구분 기호로 구분되어야 합니다. |
| 클레임 해결내 처리 포함  | 예 | 입력 및 출력 클레임의 경우 [클레임 해결이](claim-resolver-overview.md) 기술 프로필에 포함되는지 여부를 지정합니다. 가능한 값: `true` `false`  " 또는 (기본값) . 기술 프로필에서 클레임 확인자를 사용하려면 이 것을 `true`로 설정합니다. |
| 리졸브JsonPathsInJson토큰  | 예 | 기술 프로파일이 JSON 경로를 해결하는지 여부를 나타냅니다. 가능한 값: `true` `false` " 또는 (기본값) . 이 메타데이터를 사용하여 중첩된 JSON 요소의 데이터를 읽습니다. [OutputClaim에서](technicalprofiles.md#outputclaims) `PartnerClaimType` 출력할 JSON 경로 요소로 설정합니다. 예를 들어: `firstName.localized` `data.0.to.0.email`또는 .|

## <a name="cryptographic-keys"></a>암호화 키

**CryptographicKeys** 요소에는 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| client_secret | yes | ID 공급자 애플리케이션의 클라이언트 암호입니다. 암호화 키는 **response_types** 메타데이터가 `code`로 설정된 경우에만 필요합니다. 이 경우 Azure AD B2C는 액세스 토큰에 대한 인증 코드를 교환하는 다른 호출을 수행합니다. 메타데이터가 `id_token`로 설정된 경우 암호화 키를 생략할 수 있습니다. |

## <a name="redirect-uri"></a>리디렉션 URI

ID 공급자의 리디렉션 URL을 구성할 때 `https://login.microsoftonline.com/te/tenant/policyId/oauth2/authresp`를 입력합니다. **tenant**를 테넌트의 이름(예: contosob2c.onmicrosoft.com)으로 바꾸고 **policyId**를 정책 식별자(예: b2c_1_policy)로 바꿔야 합니다. 리디렉션 URI는 모두 소문자여야 합니다.

**login.microsoftonline.com** 대신 **b2clogin.com** 도메인을 사용하는 경우 login.microsoftonline.com 대신 b2clogin.com을 사용해야 합니다.

예제:

- [사용자 지정 정책을 사용하여 OAuth2 ID 공급자로 Google+ 추가](identity-provider-google-custom.md)













