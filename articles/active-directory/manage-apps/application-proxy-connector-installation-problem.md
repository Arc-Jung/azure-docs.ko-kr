---
title: 애플리케이션 프록시 에이전트 커넥터를 설치할 때 문제 발생 | Microsoft Docs
description: 애플리케이션 프록시 에이전트 커넥터를 설치할 때 발생하는 문제를 해결하는 방법
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: a35558b81d064680981bcf403a3584e3a3d00e4f
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2019
ms.locfileid: "72311752"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>애플리케이션 프록시 에이전트 커넥터를 설치할 때 문제 발생

Microsoft AAD 애플리케이션 프록시 커넥터는 아웃바운드 연결을 사용하여 클라우드 사용 가능 엔드포인트에서 내부 도메인으로의 연결을 설정하는 내부 도메인 구성 요소입니다.

## <a name="general-problem-areas-with-connector-installation"></a>커넥터 설치에 대한 일반적인 문제 영역

커넥터 설치가 실패한 경우 근본 원인은 대개 다음 영역 중 하나입니다.

1.  **연결** – 성공적인 설치를 완료하려면 새 커넥터를 등록하고 향후 트러스트 속성을 설정해야 합니다. 이 작업은 AAD 애플리케이션 프록시 클라우드 서비스에 연결하여 수행됩니다.

2.  **트러스트 설정** – 새 커넥터는 자체 서명된 인증서를 만들고 클라우드 서비스에 등록합니다.

3.  **관리자 인증** – 설치 중 사용자가 커넥터 설치를 완료하려면 관리자 자격 증명을 제공해야 합니다.

> [!NOTE]
> 커넥터 설치 로그 는% TEMP% 폴더에서 찾을 수 있으며 설치 오류의 원인이 되는 항목에 대 한 추가 정보를 제공 하는 데 도움이 됩니다.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>클라우드 애플리케이션 프록시 서비스 및 Microsoft 로그인 페이지에 대한 연결 확인

**목표:** 커넥터 머신에서 Microsoft 로그인 페이지뿐만 아니라 AAD 애플리케이션 프록시 등록 엔드포인트에도 연결할 수 있는지 확인합니다.

1.  커넥터 서버에서 [텔넷](https://docs.microsoft.com/windows-server/administration/windows-commands/telnet) 또는 기타 포트 테스트 도구를 사용 하 여 포트 443 및 80이 열려 있는지 확인 하는 방식으로 포트 테스트를 실행 합니다.

2.  이러한 포트 중 하나라도 실패 하면 방화벽 또는 백 엔드 프록시가 필요한 도메인 및 포트에 액세스할 수 있는지 확인 하 고 [온-프레미스 환경 준비](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment)를 참조 하세요.

3.  브라우저(별도 탭)를 열고 <https://login.microsoftonline.com>을 방문하여 해당 페이지에 로그인할 수 있는지 확인합니다.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>머신 및 백 엔드 구성 요소가 애플리케이션 프록시 트러스트 인증서를 지원하는지 확인

**목표:** 커넥터 머신, 백 엔드 프록시 및 방화벽이 향후 트러스트를 위해 커넥터에서 만든 인증서를 지원할 수 있는지 확인합니다.

>[!NOTE]
>커넥터는 TLS1.2에서 지원하는 SHA512 인증서를 만들려고 합니다. 컴퓨터 또는 백 엔드 방화벽과 프록시가 TLS 1.2를 지원 하지 않는 경우 설치가 실패 합니다.
>
>

**이 문제를 해결하려면:**

1.  컴퓨터가 TLS1.2를 지원하는지 확인합니다. 2012 R2 이후의 모든 Windows 버전은 TLS1.2를 지원합니다. 커넥터 컴퓨터가 2012 R2 또는 이전 버전인 경우 컴퓨터에 <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2> 기술 자료 문서가 설치되어 있는지 확인합니다.

2.  네트워크 관리자에게 문의하여 백 엔드 프록시와 방화벽이 나가는 트래픽에 대해 SHA512를 차단하지 않는지 확인합니다.

## <a name="verify-admin-is-used-to-install-the-connector"></a>관리자가 커넥터를 설치했는지 확인합니다.

**목표:** 커넥터를 설치하려고 하는 사용자가 올바른 자격 증명을 가진 관리자인지 확인합니다. 현재 설치를 완료 하려면 사용자가 적어도 응용 프로그램 관리자 여야 합니다.

**자격 증명이 올바른지 확인하려면:**

<https://login.microsoftonline.com>에 연결하고 동일한 자격 증명을 사용합니다. 로그인이 성공하는지 확인합니다. **Azure Active Directory** -&gt; **사용자 및 그룹** -&gt; **모든 사용자**로 이동하여 사용자 역할을 확인할 수 있습니다. 

사용자 계정을 선택한 다음 결과 메뉴에서 "디렉터리 역할"을 선택합니다. 선택한 역할이 "응용 프로그램 관리자" 인지 확인 합니다. 이러한 단계에서 페이지에 액세스할 수 없는 경우 필수 역할이 없습니다.

## <a name="next-steps"></a>다음 단계
[Azure AD 애플리케이션 프록시 커넥터 이해](application-proxy-connectors.md)
