---
title: Azure Key Vault 인증서를 사용 하는 TLS 종료
description: HTTPS 지원 수신기에 연결된 서버 인증서를 위해 Azure Application Gateway를 Key Vault와 통합하는 방법을 알아봅니다.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: victorh
ms.openlocfilehash: 8a64956deb7849568e70e94c9b58170df60db1e3
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104775744"
---
# <a name="tls-termination-with-key-vault-certificates"></a>Key Vault 인증서를 사용한 TLS 종료

[Azure Key Vault](../key-vault/general/overview.md)는 비밀, 키 및 TLS/SSL 인증서를 보호하는 데 사용할 수 있는 플랫폼 관리 비밀 저장소입니다. Azure Application Gateway는 HTTPS 지원 수신기에 연결된 서버 인증서에 대해 Key Vault와의 통합을 지원합니다. 이 지원은 Application Gateway의 v2 SKU로 제한 됩니다.

Key Vault 통합은 TLS 종료를 위한 두 가지 모델을 제공 합니다.

- 수신기에 연결 된 TLS/SSL 인증서를 명시적으로 제공할 수 있습니다. 이 모델은 tls 종료를 위해 Application Gateway에 TLS/SSL 인증서를 전달 하는 일반적인 방법입니다.
- HTTPS 사용 수신기를 만들 때 필요에 따라 기존 Key Vault 인증서 또는 암호에 대 한 참조를 제공할 수 있습니다.

Key Vault와 Application Gateway 통합 하면 다음과 같은 여러 이점이 제공 됩니다.

- TLS/SSL 인증서가 응용 프로그램 개발 팀에서 직접 처리 되지 않으므로 보안이 강화 됩니다. 통합을 통해 별도의 보안 팀이 다음을 수행할 수 있습니다.
  * 응용 프로그램 게이트웨이를 설정 합니다.
  * Application gateway 수명 주기를 제어 합니다.
  * 키 자격 증명 모음에 저장 된 인증서에 액세스할 수 있도록 선택한 응용 프로그램 게이트웨이에 대 한 사용 권한을 부여 합니다.
- 주요 자격 증명 모음으로 기존 인증서 가져오기 지원 또는 Key Vault Api를 사용 하 여 신뢰할 수 있는 Key Vault 파트너와 함께 새 인증서를 만들고 관리할 수 있습니다.
- 키 자격 증명 모음에 저장 된 인증서의 자동 갱신을 지원 합니다.

Application Gateway는 현재 소프트웨어 유효성 검사 인증서만 지원 합니다. HSM (하드웨어 보안 모듈)-유효성이 검사 된 인증서는 지원 되지 않습니다. Application Gateway Key Vault 인증서를 사용 하도록 구성 된 경우 해당 인스턴스는 Key Vault에서 인증서를 검색 하 고 TLS 종료를 위해 로컬로 설치 합니다. 또한 인스턴스는 갱신 된 버전의 인증서 (있는 경우)를 검색 하기 위해 4 시간 간격으로 Key Vault를 폴링합니다. 업데이트 된 인증서가 있는 경우 현재 HTTPS 수신기와 연결 된 TLS/SSL 인증서가 자동으로 순환 됩니다.

> [!NOTE]
> Azure Portal는 비밀이 아닌 KeyVault 인증서만 지원 합니다. Application Gateway는 여전히 KeyVault의 비밀을 참조할 수 있지만 PowerShell, CLI, API, ARM 템플릿 등과 같은 비 포털 리소스를 통해서만 지원 됩니다. 

## <a name="how-integration-works"></a>통합의 작동 방식

Key Vault와 통합 Application Gateway 하려면 3 단계 구성 프로세스가 필요 합니다.

1. **사용자 할당 관리 ID 만들기**

   사용자를 대신 하 여 Key Vault에서 인증서를 검색 하는 데 사용 Application Gateway 하는 기존 사용자 할당 관리 id를 만들거나 다시 사용 합니다. 자세한 내용은 [Azure Portal를 사용 하 여 사용자 할당 관리 id에 역할 만들기, 나열, 삭제 또는 할당](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)을 참조 하세요. 이 단계에서는 Azure Active Directory 테 넌 트에 새 id를 만듭니다. Id를 만드는 데 사용 되는 구독에서 id를 신뢰 합니다.

1. **주요 자격 증명 모음 구성**

   그런 다음 기존 인증서를 가져오거나 키 자격 증명 모음에 새 인증서를 만듭니다. 인증서는 응용 프로그램 게이트웨이를 통해 실행 되는 응용 프로그램에서 사용 됩니다. 이 단계에서는 암호 없는 64로 인코딩된 PFX 파일을 저장할 수 있는 Key Vault 암호를 사용할 수도 있습니다. Key Vault에서이 유형의 개체를 사용할 수 있는 자동 갱신 기능 때문에 "인증서" 유형을 사용 하는 것이 좋습니다. 인증서 또는 암호를 만든 후에는 Key Vault에서 액세스 정책을 정의 하 여 id에 암호에 대 한 액세스 권한을 부여할 수 있도록 해야 합니다.
   
   > [!IMPORTANT]
   > 2021 년 3 월 15 일부 터 Key Vault는 신뢰할 수 있는 서비스 중 하나로 Azure 애플리케이션 게이트웨이를 인식 하므로 Azure에서 보안 네트워크 경계를 구축할 수 있습니다. 이렇게 하면 Key Vault에 대 한 모든 네트워크 (인터넷 트래픽 포함)의 트래픽에 대 한 액세스를 거부할 수 있지만 구독 하는 Application Gateway 리소스에 액세스할 수 있게 됩니다. 

   > 다음과 같은 방법으로 제한 된 Key Vault 네트워크에 Application Gateway를 구성할 수 있습니다. <br />
   > a) Key Vault의 네트워킹 블레이드 <br />
   > b) "방화벽 및 가상 네트워크" 탭에서 개인 끝점 및 선택한 네트워크를 선택 합니다. <br/>
   > c) 가상 네트워크를 사용 하 여 Application Gateway의 가상 네트워크 및 서브넷을 추가 합니다. 이 과정에서 확인란을 선택 하 여 ' Microsoft. KeyVault ' 서비스 엔드포인트도 구성 합니다. <br/>
   > d) 마지막으로, "예"를 선택 하 여 신뢰할 수 있는 서비스가 Key Vault의 방화벽을 우회 하도록 허용 합니다. <br/>
   > 
   > ![Key Vault 방화벽](media/key-vault-certs/key-vault-firewall.png)


   > [!NOTE]
   > Azure CLI 또는 PowerShell을 사용 하거나 Azure Portal에서 배포 된 Azure 응용 프로그램을 통해 ARM 템플릿을 통해 응용 프로그램 게이트웨이를 배포 하는 경우, SSL 인증서는 키 자격 증명 모음에 base64 인코딩 PFX 파일로 저장 됩니다. [배포 중에 보안 매개 변수 값을 전달 하려면 Azure Key Vault 사용](../azure-resource-manager/templates/key-vault-parameter.md)의 단계를 완료 해야 합니다. 
   >
   > 을로 설정 하는 것이 특히 중요 `enabledForTemplateDeployment` `true` 합니다. 인증서가 암호를 적거나 암호를 포함할 수 있습니다. 암호를 사용 하는 인증서의 경우 다음 예제에서는 `sslCertificates` `properties` 앱 게이트웨이의 ARM 템플릿 구성에 대 한의 항목에 대 한 가능한 구성을 보여 줍니다. 및의 값 `appGatewaySSLCertificateData` 은 `appGatewaySSLCertificatePassword` [동적 ID로 암호 참조](../azure-resource-manager/templates/key-vault-parameter.md#reference-secrets-with-dynamic-id)섹션에 설명 된 대로 key vault에서 조회 됩니다. 에서 뒤로 참조를 따라 조회를 수행 하는 `parameters('secretName')` 방법을 확인 합니다. 인증서가 암호 보다 낮으면 항목을 생략 `password` 합니다.
   >   
   > ```
   > "sslCertificates": [
   >     {
   >         "name": "appGwSslCertificate",
   >         "properties": {
   >             "data": "[parameters('appGatewaySSLCertificateData')]",
   >             "password": "[parameters('appGatewaySSLCertificatePassword')]"
   >         }
   >     }
   > ]
   > ```

1. **응용 프로그램 게이트웨이 구성**

   위의 두 단계를 완료 한 후에는 사용자 할당 관리 id를 사용 하도록 기존 응용 프로그램 게이트웨이를 설정 하거나 수정할 수 있습니다. 자세한 내용은 [AzApplicationGatewayIdentity](/powershell/module/az.network/set-azapplicationgatewayidentity)를 참조 하세요.

   Key Vault 인증서 또는 암호 ID의 전체 URI를 가리키도록 HTTP 수신기의 TLS/SSL 인증서를 구성할 수도 있습니다.

   ![주요 자격 증명 모음 인증서](media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>다음 단계

[Azure PowerShell를 사용 하 여 Key Vault 인증서로 TLS 종료 구성](configure-keyvault-ps.md)
