---
title: Azure AD Connect "구성 안 함"을 사용 하는 경우 PTA를 사용 하지 않도록 설정 합니다. | Microsoft Docs
description: 이 문서에서는 Azure AD Connect "구성 안 함" 기능을 사용 하 여 PTA를 사용 하지 않도록 설정 하는 방법을 설명 합니다.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 04/20/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26112b1e799cbde3145e7137c686b4b336db4bab
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98919938"
---
# <a name="disable-pta-when-using-azure-ad-connect"></a>Azure AD Connect를 사용할 때 PTA 사용 안 함

Azure AD Connect에서 통과 인증을 사용 하는 경우 **"구성** 안 함"으로 설정 하면 사용 하지 않도록 설정할 수 있습니다. 

>[!NOTE]
>이미 PHS를 사용 하도록 설정 하면 PTA를 사용 하지 않도록 설정 하면 테 넌 트 대체가 PHS로 바뀝니다.

PTA 비활성화는 다음 cmdlet을 사용 하 여 수행할 수 있습니다. 

## <a name="prerequisites"></a>필수 구성 요소
필요한 필수 구성 요소는 다음과 같습니다.
- PTA 에이전트가 설치 된 모든 windows 컴퓨터 
- 에이전트는 1.5.1742.0 버전 이상 이어야 합니다. 
- Azure 전역 관리자 계정-PowerShell cmdlet을 실행 하 여 PTA를 사용 하지 않도록 설정 합니다.

>[!NOTE]
> 에이전트가 오래 된 경우이 작업을 완료 하는 데 필요한 cmdlet이 없을 수 있습니다. Azure Portal에서 새 에이전트를 가져와 windows 컴퓨터에 설치 하 고 관리자 자격 증명을 제공할 수 있습니다. 에이전트를 설치 해도 클라우드의 PTA 상태는 영향을 받지 않습니다.

> [!IMPORTANT]
> Azure Government 클라우드를 사용 하는 경우 다음 값을 사용 하 여 환경 이름 매개 변수를 전달 해야 합니다. 
>
>| 환경 이름 | 클라우드 |
>| - | - |
>| AzureUSGovernment | US Gov|


## <a name="to-disable-pta"></a>PTA를 사용 하지 않도록 설정 하려면
PowerShell 세션 내에서 다음을 사용 하 여 PTA를 사용 하지 않도록 설정 합니다.
1. PS C:\Program Files\Microsoft Azure AD Connect 인증 에이전트> `Import-Module .\Modules\PassthroughAuthPSModule`
2. `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth` 또는 `Get-PassthroughAuthenticationEnablementStatus -Feature PassthroughAuth -EnvironmentName <identifier>`
3. `Disable-PassthroughAuthentication  -Feature PassthroughAuth` 또는 `Disable-PassthroughAuthentication -Feature PassthroughAuth -EnvironmentName <identifier>`

## <a name="if-you-dont-have-access-to-an-agent"></a>에이전트에 액세스할 수 없는 경우

에이전트 컴퓨터가 없는 경우 다음 명령을 사용 하 여 에이전트를 설치할 수 있습니다.

1. Portal.azure.com에서 최신 인증 에이전트를 다운로드 합니다.
2. 기능 `.\AADConnectAuthAgentSetup.exe` 을 설치 합니다. `.\AADConnectAuthAgentSetup.exe ENVIRONMENTNAME=<identifier>`


## <a name="next-steps"></a>다음 단계

- [Azure Active Directory 통과 인증으로 사용자 로그인](how-to-connect-pta.md)
