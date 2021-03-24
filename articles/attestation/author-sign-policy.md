---
title: Azure Attestation 정책을 작성하는 방법
description: 증명 정책을 작성하는 방법의 설명입니다.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 3e36de62b79788e2efdc3e9abf711924c4fba0c4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93341810"
---
# <a name="how-to-author-an-attestation-policy"></a>증명 정책을 작성하는 방법

증명 정책은 Microsoft Azure Attestation에 업로드된 파일입니다. Azure Attestation은 증명 특정 정책 형식으로 정책을 업로드하는 유연성을 제공합니다. 또는 JSON 웹 서명에서 인코딩된 버전의 정책을 업로드할 수도 있습니다. 정책 관리자는 증명 정책을 작성해야 합니다. 대부분의 증명 시나리오에서 신뢰 당사자는 정책 관리자 역할을 합니다. 증명 호출을 수행하는 클라이언트는 증명 증거를 보냅니다. 서비스에서 이를 구문 분석하고 들어오는 클레임(속성, 값 집합)으로 변환합니다. 그런 다음, 서비스는 정책에 정의된 항목을 기반으로 클레임을 처리하고, 계산된 결과를 반환합니다.

정책에는 인증 조건, 속성 및 증명 토큰의 콘텐츠를 결정하는 규칙이 포함되어 있습니다. 샘플 정책 파일은 아래와 같습니다.

```
version=1.0;
authorizationrules
{
   c:[type="secureBootEnables", issuer=="AttestationService"]=> permit()
};

issuancerules
{
  c:[type="secureBootEnables", issuer=="AttestationService"]=> issue(claim=c)
  c:[type="notSafeMode", issuer=="AttestationService"]=> issue(claim=c)
};
```
 
정책 파일에는 위에 표시된 것처럼 3개의 세그먼트가 있습니다.

- **버전**:  버전은 뒤따르는 문법의 버전 번호입니다. 

    ```
    version=MajorVersion.MinorVersion   
    ```

    현재 지원되는 유일한 버전은 1.0입니다.

- **authorizationrules**: Azure Attestation이 **issuancerules** 로 진행되는지 확인하기 위해 먼저 확인되는 클레임 규칙의 컬렉션입니다. 클레임 규칙은 정의된 순서대로 적용됩니다.

- **issuancerules**: 정책에 정의된 대로 증명 결과에 추가 정보를 추가하기 위해 평가되는 클레임 규칙의 컬렉션입니다. 클레임 규칙은 정의된 순서대로 적용되며 선택 사항이기도 합니다.

자세한 내용은 [클레임 및 클레임 규칙](claim-rule-grammar.md)을 참조하세요.
   
## <a name="drafting-the-policy-file"></a>정책 파일 초안 작성

1. 새 파일을 만듭니다.
1. 버전을 파일에 추가합니다.
1. **authorizationrules** 및 **issuancerules** 에 대한 섹션을 추가합니다.

  ```
  version=1.0;
  authorizationrules
  {
  =>deny();
  };
  
  issuancerules
  {
  };
  ```

  권한 부여 규칙에는 아무런 조건 없이 거부() 작업을 포함하여 발급 규칙이 처리되지 않도록 합니다. 또는 권한 부여 규칙이 발급 규칙의 처리를 허용하는 허용() 작업을 포함할 수도 있습니다.
  
4. 권한 부여 규칙에 클레임 규칙 추가

  ```
  version=1.0;
  authorizationrules
  {
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules
  {
  };
  ```

  들어오는 클레임 집합에 유형, 값 및 발급자와 일치하는 클레임이 포함된 경우 허용() 작업은 **issuancerules** 를 처리하도록 정책 엔진에 지시합니다.
  
5. **issuancerules** 에 클레임 규칙을 추가합니다.

  ```
  version=1.0;
  authorizationrules
  {
  [type=="secureBootEnabled", value==true, issuer=="AttestationService"]=>permit();
  };
  
  issuancerules
  {
  => issue(type="SecurityLevelValue", value=100);
  };
  ```
  
  나가는 클레임 집합에는 다음이 포함된 클레임이 포함됩니다.

  ```
  [type="SecurityLevelValue", value=100, valueType="Integer", issuer="AttestationPolicy"]
  ```

  비슷한 방식으로 복잡한 정책을 만들 수 있습니다. 자세한 내용은 [증명 정책 예제](policy-examples.md)를 참조하세요.
  
6. 파일을 저장합니다.

## <a name="creating-the-policy-file-in-json-web-signature-format"></a>JSON 웹 서명 형식으로 정책 파일 만들기

정책 파일을 만든 후에 JWS 형식으로 정책을 업로드하려면 아래 단계를 수행합니다.

1. 페이로드로 정책(utf-8 인코딩)을 사용하여 JWS, RFC 7515를 생성합니다.
     - Base64Url 인코딩된 정책의 페이로드 식별자는 "AttestationPolicy"여야 합니다.
     
     샘플 JWT:
     ```
     Header: {"alg":"none"}
     Payload: {"AttestationPolicy":" Base64Url (policy)"}
     Signature: {}

     JWS format: eyJhbGciOiJub25lIn0.XXXXXXXXX.
     ```

2. (선택 사항) 정책에 서명합니다. Azure Attestation은 다음과 같은 알고리즘을 지원합니다.
     - **없음**: 정책 페이로드에 서명하지 않습니다.
     - **RS256**: 정책 페이로드를 서명하는 데 지원되는 알고리즘

3. JWS를 업로드하고 정책의 유효성을 검사합니다.
     - 정책 파일에 구문 오류가 없는 경우 정책 파일은 서비스에서 허용됩니다.
     - 정책 파일에 구문 오류가 있는 경우 정책 파일은 서비스에서 거부됩니다.

## <a name="next-steps"></a>다음 단계
- [PowerShell을 사용하여 Azure Attestation 설정](quickstart-powershell.md)
- [코드 샘플을 사용하여 SGX enclave 증명](/samples/browse/?expanded=azure&terms=attestation)