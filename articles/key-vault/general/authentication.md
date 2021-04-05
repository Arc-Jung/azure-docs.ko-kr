---
title: Azure Key Vault에 인증
description: Azure Key Vault에 인증하는 방법을 알아봅니다.
author: ShaneBala-keyvault
ms.author: sudbalas
ms.date: 08/27/2020
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.openlocfilehash: ed83d52d19df126ef9b0e68f984f88f5dfd40c42
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99492943"
---
# <a name="authenticate-to-azure-key-vault"></a>Azure Key Vault에 인증

Azure Key Vault를 사용하면 비밀을 저장하고 중앙 집중식 보안 클라우드 리포지토리에서 배포를 제어할 수 있으므로 애플리케이션에 자격 증명을 저장할 필요가 없습니다. 애플리케이션은 런타임 시 Key Vault로만 인증하여 해당 비밀에 액세스해야 합니다.

## <a name="app-identity-and-security-principals"></a>앱 ID 및 보안 주체

Key Vault를 사용한 인증은 [Azure AD(Azure Active Directory)](../../active-directory/fundamentals/active-directory-whatis.md)와 함께 작동하며, 이는 지정된 **보안 주체** 의 ID를 인증하는 역할을 담당합니다.

보안 주체는 사용자, 그룹 또는 Azure 리소스에 대한 액세스를 요청하는 애플리케이션을 나타내는 개체입니다. Azure는 모든 보안 주체에 고유한 **개체 ID** 를 할당합니다.

* **사용자** 보안 주체는 Azure Active Directory에 프로필이 있는 개인을 식별합니다.

* **그룹** 보안 주체는 Azure Active Directory에서 만든 사용자 세트를 식별합니다. 그룹에 할당된 모든 역할 또는 사용 권한은 그룹 내의 모든 사용자에 부여됩니다.

* **서비스 주체** 는 애플리케이션 또는 서비스를 식별하는 보안 주체의 유형입니다. 즉, 사용자 또는 그룹이 아닌 코드 조각입니다. 서비스 주체의 개체 ID를 **클라이언트 ID** 라고 하며, 사용자 이름처럼 작동합니다. 서비스 주체의 **클라이언트 암호** 는 암호처럼 작동합니다.

애플리케이션의 경우 서비스 주체를 가져오는 방법에는 두 가지가 있습니다.

* 권장: 애플리케이션에 대한 시스템 할당 **관리 ID** 를 사용하도록 설정합니다.

    관리 ID를 통해 Azure는 애플리케이션의 서비스 주체를 내부적으로 관리하고 다른 Azure 서비스에서 애플리케이션을 자동으로 인증합니다. 관리 ID는 다양한 서비스에 배포된 애플리케이션에 사용할 수 있습니다.

    자세한 내용은 [관리 ID 개요](../../active-directory/managed-identities-azure-resources/overview.md)를 참조하세요. 또한 [관리 ID를 지원하는 Azure 서비스](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)를 참조하세요. 특정 서비스(예: App Service, Azure Functions, Virtual Machines 등)에 대해 관리 ID를 사용하도록 설정하는 방법이 담긴 문서로 연결됩니다.

* 관리 ID를 사용할 수 없는 경우 대신 Azure AD 테넌트를 사용하여 애플리케이션에 **등록** 합니다([빠른 시작: Azure ID 플랫폼에 애플리케이션 등록](../../active-directory/develop/quickstart-register-app.md) 참조). 또한 등록은 모든 테넌트에서 앱을 식별하는 두 번째 애플리케이션 개체를 만듭니다.

## <a name="authorize-a-security-principal-to-access-key-vault"></a>보안 주체에게 Key Vault에 대한 액세스 권한 부여

Key Vault는 두 가지 개별 권한 부여 수준으로 작동합니다.

- **액세스 정책** 은 사용자, 그룹 또는 서비스 주체에게 기존 Key Vault 리소스 *내에 있는* 비밀, 키 및 인증서에 액세스할 수 있는 권한이 있는지 여부를 제어합니다(“데이터 평면” 작업이라고도 함). 액세스 정책은 일반적으로 사용자, 그룹 및 애플리케이션에 부여됩니다.

    액세스 정책을 할당하려면 다음 문서를 참조하세요.

    - [Azure Portal](assign-access-policy-portal.md)
    - [Azure CLI](assign-access-policy-cli.md)
    - [Azure PowerShell](assign-access-policy-portal.md)

- **역할 사용 권한** 은 사용자, 그룹 또는 서비스 주체가 Key Vault 리소스를 만들고, 삭제하고, 관리할 권한이 있는지 여부를 제어합니다(“관리 평면”이라고도 함). 이러한 역할은 대부분 관리자에게만 부여됩니다.
 
    역할을 할당하고 관리하려면 다음 문서를 참조하세요.

    - [Azure Portal](../../role-based-access-control/role-assignments-portal.md)
    - [Azure CLI](../../role-based-access-control/role-assignments-cli.md)
    - [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)

    Key Vault는 현재 Key Vault 리소스에서 관리 작업이 가능한 [기여자](../../role-based-access-control/built-in-roles.md#key-vault-contributor) 역할을 지원합니다. 여러 다른 역할은 현재 미리 보기로 제공됩니다. [Azure 사용자 지정 역할](../../role-based-access-control/custom-roles.md)에 설명된 대로 사용자 지정 역할을 만들 수도 있습니다.

    역할에 대한 전반적인 내용은 [Azure RBAC(역할 기반 액세스 제어)란?](../../role-based-access-control/overview.md)을 참조하세요.


> [!IMPORTANT]
> 가장 강력한 보안을 위해 항상 최소 권한 원칙을 따르고 필요한 가장 구체적인 액세스 정책 및 역할만 부여합니다. 
    
## <a name="configure-the-key-vault-firewall"></a>Key Vault 방화벽 구성

기본적으로 Key Vault는 공용 IP 주소를 통해 리소스에 대한 액세스를 허용합니다. 또한 가장 강력한 보안을 위해 특정 IP 범위, 서비스 엔드포인트, 가상 네트워크 또는 프라이빗 엔드포인트에 대한 액세스를 제한할 수 있습니다.

자세한 내용은 [방화벽 뒤에 있는 Azure Key Vault에 액세스](./access-behind-firewall.md)를 참조하세요.


## <a name="the-key-vault-authentication-flow"></a>Key Vault 인증 흐름

1. 서비스 주체는 Azure AD를 사용하여 인증을 요청합니다. 예제는 다음과 같습니다.
    * 사용자가 사용자 이름과 암호를 사용하여 Azure Portal에 로그인합니다.
    * 애플리케이션이 클라이언트 ID 및 비밀 또는 클라이언트 인증서를 제공하는 Azure REST API를 호출합니다.
    * 관리 ID를 사용하는 가상 머신과 같은 Azure 리소스가 [Azure IMDS(Instance Metadata Service)](../../virtual-machines/windows/instance-metadata-service.md) REST 엔드포인트에 연결하여 액세스 토큰을 가져옵니다.

1. Azure AD를 통한 인증에 성공하면 OAuth 토큰이 서비스 주체에 부여됩니다.

1. 서비스 주체는 Key Vault의 엔드포인트(URI)을 통해 Key Vault REST API를 호출합니다.

1. Key Vault 방화벽은 다음 조건을 확인합니다. 조건을 충족하는 경우 호출이 허용됩니다. 그렇지 않으면 호출이 차단되고 사용 권한 없음 응답이 반환됩니다.

    * 방화벽이 사용하지 않도록 설정되어 있고, 퍼블릭 인터넷에서 Key Vault의 퍼블릭 엔드포인트에 연결할 수 있습니다.
    * 호출자가 [Key Vault 신뢰할 수 있는 서비스](./overview-vnet-service-endpoints.md#trusted-services)로, 방화벽을 우회할 수 있습니다.
    * 호출자가 IP 주소, 가상 네트워크 또는 서비스 엔드포인트별로 방화벽에 나열됩니다.
    * 호출자가 구성된 프라이빗 링크 연결을 통해 Key Vault에 연결할 수 있습니다.    

1. 방화벽에서 호출을 허용하는 경우 Key Vault는 Azure AD를 호출하여 서비스 주체의 액세스 토큰의 유효성을 검사합니다.

1. Key Vault는 서비스 주체에게 요청된 작업에 대한 필수 액세스 정책이 있는지 확인합니다. 그렇지 않은 경우 Key Vault에서 사용 권한 없음 응답을 반환합니다.

1. Key Vault에서 요청된 작업을 수행하고 결과를 반환합니다.

다음 다이어그램에서는 Key Vault “비밀 가져오기” API를 호출하는 애플리케이션에 대한 프로세스를 보여 줍니다.

![Azure Key Vault 인증 흐름](../media/authentication/authentication-flow.png)

> [!NOTE]
> 비밀, 인증서 및 키에 대한 키 자격 증명 모음 SDK 클라이언트는 액세스 토큰 없이 키 자격 증명 모음을 추가로 호출하므로 테넌트 정보를 검색하기 위해 401 응답이 발생합니다. 자세한 내용은 [인증, 요청 및 응답](authentication-requests-and-responses.md)을 참조하세요.

## <a name="code-examples"></a>코드 예제

다음 표는 해당 언어에 대해 Azure SDK 라이브러리를 사용하여 애플리케이션 코드에서 Key Vault와 작업하는 방법을 설명하는 여러 문서가 연결되어 있습니다. Azure CLI 및 Azure Portal과 같은 다른 인터페이스는 편의를 위해 포함되었습니다.

| Key Vault 비밀 | Key Vault 키 | Key Vault 인증서 |
|  --- | --- | --- |
| [Python](../secrets/quick-create-python.md) | [Python](../keys/quick-create-python.md) | [Python](../certificates/quick-create-python.md) | 
| [.NET](../secrets/quick-create-net.md) | [.NET](../keys/quick-create-net.md) | [.NET](../certificates/quick-create-net.md) |
| [Java](../secrets/quick-create-java.md) | [Java](../keys/quick-create-java.md) | [Java](../certificates/quick-create-java.md) |
| [JavaScript](../secrets/quick-create-node.md) | [JavaScript](../keys/quick-create-node.md) | [JavaScript](../certificates/quick-create-node.md) | 
| [Azure Portal](../secrets/quick-create-portal.md) | [Azure Portal](../keys/quick-create-portal.md) | [Azure Portal](../certificates/quick-create-portal.md) |
| [Azure CLI](../secrets/quick-create-cli.md) | [Azure CLI](../keys/quick-create-cli.md) | [Azure CLI](../certificates/quick-create-cli.md) |
| [Azure PowerShell](../secrets/quick-create-powershell.md) | [Azure PowerShell](../keys/quick-create-powershell.md) | [Azure PowerShell](../certificates/quick-create-powershell.md) |
| [ARM 템플릿](../secrets/quick-create-net.md) | -- | -- |

## <a name="next-steps"></a>다음 단계

- [Key Vault 액세스 정책 문제 해결](troubleshooting-access-issues.md)
- [Key Vault REST API 오류 코드](rest-error-codes.md)
- [Key Vault 개발자 가이드](developers-guide.md)
- [Azure RBAC(Azure 역할 기반 액세스 제어)란?](../../role-based-access-control/overview.md)
