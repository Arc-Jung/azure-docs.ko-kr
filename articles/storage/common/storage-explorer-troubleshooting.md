---
title: Azure Storage Explorer 문제 해결 가이드 | Microsoft Docs
description: Azure Storage 탐색기에 대 한 디버깅 기술 개요
services: storage
author: Deland-Han
manager: dcscontentpm
ms.service: storage
ms.topic: troubleshooting
ms.date: 07/28/2020
ms.author: delhan
ms.openlocfilehash: 9a20db58846ca48afb4fb256adae58e1fccdff3a
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98875739"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Explorer 문제 해결 가이드

Microsoft Azure Storage Explorer는 Windows, macOS 및 Linux에서 Azure Storage 데이터를 쉽게 사용할 수 있게 하는 독립 실행형 앱입니다. 앱은 Azure, National Clouds 및 Azure Stack에서 호스트되는 스토리지 계정에 연결할 수 있습니다.

이 가이드에는 Storage 탐색기에서 일반적으로 발생 하는 문제에 대 한 해결 방법이 요약 되어 있습니다.

## <a name="azure-rbac-permissions-issues"></a>Azure RBAC 사용 권한 문제

Azure 역할 기반 access control [AZURE RBAC](../../role-based-access-control/overview.md) 는 권한 집합을 _역할로_ 결합 하 여 azure 리소스의 매우 세부적인 액세스 관리를 가능 하 게 합니다. Storage 탐색기에서 가장 적합 한 Azure RBAC를 얻기 위한 몇 가지 전략이 있습니다.

### <a name="how-do-i-access-my-resources-in-storage-explorer"></a>Storage 탐색기에서 내 리소스에 액세스 어떻게 할까요??

Azure RBAC를 통해 저장소 리소스에 액세스 하는 데 문제가 발생 하는 경우 적절 한 역할이 할당 되지 않았을 수 있습니다. 다음 섹션에서는 현재 저장소 리소스에 액세스 하는 데 필요한 권한을 Storage 탐색기 설명 합니다. 적절 한 역할 또는 권한이 있는지 확실 하지 않은 경우 Azure 계정 관리자에 게 문의 하세요.

#### <a name="read-listget-storage-accounts-permissions-issue"></a>"읽기: 저장소 계정 나열/가져오기" 권한 문제

저장소 계정을 나열할 수 있는 권한이 있어야 합니다. 이 권한을 얻으려면 _읽기 권한자_ 역할을 할당 받아야 합니다.

#### <a name="list-storage-account-keys"></a>스토리지 계정 키 나열

또한 Storage 탐색기는 계정 키를 사용 하 여 요청을 인증할 수 있습니다. _참가자_ 역할과 같은 보다 강력한 역할을 통해 계정 키에 대 한 액세스 권한을 얻을 수 있습니다.

> [!NOTE]
> 액세스 키는이를 보유 한 모든 사용자에 게 무제한 권한을 부여 합니다. 따라서 이러한 키를 계정 사용자에 게 전달 하지 않는 것이 좋습니다. 액세스 키를 취소 해야 하는 경우에는 [Azure Portal](https://portal.azure.com/)에서 다시 생성할 수 있습니다.

#### <a name="data-roles"></a>데이터 역할

리소스에서 데이터를 읽을 수 있는 액세스 권한을 부여 하는 하나 이상의 역할을 할당 해야 합니다. 예를 들어 blob을 나열 하거나 다운로드 하려면 적어도 _저장소 Blob 데이터 판독기_ 역할이 필요 합니다.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Storage 탐색기 내 리소스를 보려면 관리 계층 역할이 필요한 이유는 무엇 인가요?

Azure Storage에는 _관리_ 및 _데이터_ 의 두 가지 액세스 계층이 있습니다. 구독 및 저장소 계정은 관리 계층을 통해 액세스 됩니다. 컨테이너, blob 및 기타 데이터 리소스는 데이터 계층을 통해 액세스 됩니다. 예를 들어 Azure에서 저장소 계정 목록을 가져오려는 경우 관리 끝점에 요청을 보냅니다. 계정에 blob 컨테이너 목록을 원하는 경우 적절 한 서비스 끝점에 요청을 보냅니다.

Azure 역할은 관리 또는 데이터 계층 액세스 권한을 부여할 수 있습니다. 예를 들어 판독기 역할은 관리 계층 리소스에 대 한 읽기 전용 액세스 권한을 부여 합니다.

엄격히 말하면 읽기 권한자 역할은 데이터 계층 사용 권한을 제공 하지 않으며 데이터 계층에 액세스 하는 데 필요 하지 않습니다.

Storage 탐색기를 통해 Azure 리소스에 연결 하는 데 필요한 정보를 수집 하 여 리소스에 쉽게 액세스할 수 있습니다. 예를 들어 blob 컨테이너를 표시 하기 위해 Storage 탐색기는 "컨테이너 목록" 요청을 blob service 끝점으로 보냅니다. 이 끝점을 가져오기 위해 Storage 탐색기는 사용자가 액세스할 수 있는 구독 및 저장소 계정 목록을 검색 합니다. 구독 및 저장소 계정을 찾으려면 Storage 탐색기 관리 계층에 대 한 액세스 권한도 필요 합니다.

관리 계층 사용 권한을 부여 하는 역할이 없으면 데이터 계층에 연결 하는 데 필요한 정보를 가져올 수 Storage 탐색기.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>관리자에 게 필요한 관리 계층 권한을 얻을 수 없는 경우 어떻게 하나요?

Blob 컨테이너 또는 큐에 액세스 하려는 경우 Azure 자격 증명을 사용 하 여 해당 리소스에 연결할 수 있습니다.

1. 연결 대화 상자를 엽니다.
2. "Azure Active Directory를 통해 리소스 추가 (Azure AD)"를 선택 합니다. 다음을 선택합니다.
3. 연결 하려는 리소스와 연결 된 사용자 계정 및 테 넌 트를 선택 합니다. 다음을 선택합니다.
4. 리소스 종류를 선택 하 고 리소스에 대 한 URL을 입력 한 다음 연결에 대 한 고유한 표시 이름을 입력 합니다. 다음, 연결을 차례로 선택 합니다.

다른 리소스 종류의 경우 현재 Azure RBAC 관련 솔루션이 없습니다. 이 문제를 해결 하려면 [리소스에 연결할](../../vs-azure-tools-storage-manage-with-storage-explorer.md?tabs=linux#use-a-shared-access-signature-uri)SAS URI를 요청할 수 있습니다.

### <a name="recommended-azure-built-in-roles"></a>권장 되는 Azure 기본 제공 역할

Storage 탐색기를 사용 하는 데 필요한 권한을 제공할 수 있는 몇 가지 Azure 기본 제공 역할이 있습니다. 이러한 역할 중 일부는 다음과 같습니다.
- [Owner](../../role-based-access-control/built-in-roles.md#owner): 리소스에 대 한 액세스를 포함 하 여 모든 것을 관리 합니다.
- [참가자](../../role-based-access-control/built-in-roles.md#contributor): 리소스에 대 한 액세스를 제외한 모든 항목을 관리 합니다.
- [Reader](../../role-based-access-control/built-in-roles.md#reader): 리소스를 읽고 나열 합니다.
- [Storage 계정 참가자](../../role-based-access-control/built-in-roles.md#storage-account-contributor): 저장소 계정에 대 한 전체 관리
- [저장소 Blob 데이터 소유자](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Azure Storage Blob 컨테이너 및 데이터에 대 한 모든 권한입니다.
- [저장소 Blob 데이터 참여자](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): 컨테이너 및 blob Azure Storage 읽기, 쓰기 및 삭제
- [저장소 Blob 데이터 판독기](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): 컨테이너 및 blob Azure Storage 읽고 나열 합니다.

> [!NOTE]
> 소유자, 참가자 및 저장소 계정 기여자 역할은 계정 키 액세스를 부여 합니다.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>오류: 인증서 체인의 자체 서명 된 인증서 (및 유사한 오류)

인증서 오류는 일반적으로 다음과 같은 경우에 발생 합니다.

- 앱이 _투명 프록시_ 를 통해 연결 됩니다. 즉, 서버 (예: 회사 서버)는 HTTPS 트래픽을 가로채 고, 암호를 해독 한 다음 자체 서명 된 인증서를 사용 하 여 암호화 하는 것을 의미 합니다.
- 받은 HTTPS 메시지에 자체 서명 된 TLS/SSL 인증서를 삽입 하는 응용 프로그램을 실행 하 고 있습니다. 인증서를 삽입 하는 응용 프로그램의 예로는 바이러스 백신 및 네트워크 트래픽 검사 소프트웨어가 있습니다.

Storage 탐색기 자체 서명 된 인증서 또는 신뢰할 수 없는 인증서가 표시 되 면 수신 된 HTTPS 메시지의 변경 여부를 더 이상 알 수 없습니다. 자체 서명 된 인증서의 복사본이 있는 경우 다음 단계에 따라 해당 인증서를 신뢰 하도록 Storage 탐색기에 지시할 수 있습니다.

1. Base-64 인코딩된 x.509 (.cer) 인증서 복사본을 가져옵니다.
2.   >  **SSL 인증서** 편집  >  **인증서 가져오기** 로 이동한 다음 파일 선택기를 사용 하 여 .cer 파일을 찾고 선택 하 고 엽니다.

여러 인증서 (루트 및 중간)가 있는 경우에도이 문제가 발생할 수 있습니다. 이 오류를 해결 하려면 두 인증서를 모두 추가 해야 합니다.

인증서가 들어오는 위치를 모를 경우 다음 단계를 수행 하 여 인증서를 찾으십시오.

1. OpenSSL을 설치합니다.
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html): 모든 조명 버전이 면 충분 합니다.
    * Mac 및 Linux: 운영 체제에 포함 되어야 합니다.
2. OpenSSL를 실행 합니다.
    * Windows: 설치 디렉터리를 열고 **/st/** 를 선택한 다음 **openssl.exe** 를 두 번 클릭 합니다.
    * Mac 및 Linux: `openssl` 터미널에서를 실행 합니다.
3. `s_client -showcerts -connect microsoft.com:443`를 실행합니다.
4. 자체 서명된 인증서를 찾습니다. 자체 서명 된 인증서를 모를 경우 주제와 발급자의 모든 위치를 기록해 둡니다 `("s:")` `("i:")` .
5. 자체 서명 된 인증서를 찾았으면 각 인증서에 대해를 복사 하 여에 포함 된 모든 항목을 `-----BEGIN CERTIFICATE-----` `-----END CERTIFICATE-----` 새 .cer 파일에 붙여넣습니다.
6. Storage 탐색기를 열고   >  **SSL 인증서** 편집  >  **인증서 가져오기** 로 이동 합니다. 그런 다음 파일 선택기를 사용 하 여 만든 .cer 파일을 찾고 선택 하 고 엽니다.

이러한 단계를 수행 하 여 자체 서명 된 인증서를 찾을 수 없는 경우 피드백 도구를 통해 microsoft에 문의 하세요. 플래그를 사용 하 여 명령줄에서 Storage 탐색기를 열 수도 있습니다 `--ignore-certificate-errors` . 이 플래그를 사용 하 여 열면 Storage 탐색기 인증서 오류가 무시 됩니다.

## <a name="sign-in-issues"></a>로그인 문제

### <a name="blank-sign-in-dialog-box"></a>빈 로그인 대화 상자

빈 로그인 대화 상자는 Active Directory Federation Services (AD FS) 메시지를 Storage 탐색기 표시 하 여 전자에서 지원 하지 않는 리디렉션을 수행 하는 경우 가장 자주 발생 합니다. 이 문제를 해결 하기 위해 로그인에 장치 코드 흐름을 사용 하려고 할 수 있습니다. 이렇게 하려면 다음 단계를 따르십시오.

1. 왼쪽 세로 도구 모음에서 **설정** 을 엽니다. 설정 패널에서 **응용 프로그램**  >  **로그인** 으로 이동 합니다. **장치 코드 흐름 로그인 사용** 을 사용 하도록 설정 합니다.
2. 왼쪽 세로 막대에 있는 플러그 아이콘을 통해 또는 계정 패널에서 **계정 추가** 를 선택 하 여 **연결** 대화 상자를 엽니다.
3. 로그인 할 환경을 선택 합니다.
4. **로그인** 을 선택합니다.
5. 다음 패널의 지침을 따릅니다.

기본 브라우저가 이미 다른 계정에 로그인 되어 있으므로 사용 하려는 계정에 로그인 할 수 없는 경우 다음 중 하나를 수행 합니다.

- 링크 및 코드를 브라우저의 프라이빗 세션에 수동으로 복사합니다.
- 링크 및 코드를 다른 브라우저에 수동으로 복사합니다.

### <a name="reauthentication-loop-or-upn-change"></a>재인증 루프 또는 UPN 변경

재인증 루프에 있거나 계정 중 하나의 UPN이 변경 된 경우 다음 단계를 수행 합니다.

1. 모든 계정을 제거한 후 Storage 탐색기를 닫습니다.
2. 컴퓨터에서 .IdentityService 폴더를 삭제합니다. Windows에서 폴더는 `C:\users\<username>\AppData\Local`에 있습니다. Mac 및 Linux의 경우 사용자 디렉토리의 루트에서 폴더를 찾을 수 있습니다.
3. Mac 또는 Linux를 실행 하는 경우 운영 체제의 키 저장소에서 IdentityService 항목을 삭제 해야 합니다. Mac에서 키 저장소은 *Gnome 키 집합* 응용 프로그램입니다. Linux에서 응용 프로그램은 일반적으로 인증 _키 라고 하지만_ 배포에 따라 이름이 다를 수 있습니다.

### <a name="conditional-access"></a>조건부 액세스

Storage 탐색기에서 사용 하는 Azure AD 라이브러리의 제한으로 인해 Windows 10, Linux 또는 macOS에서 Storage 탐색기를 사용 하는 경우에는 조건부 액세스가 지원 되지 않습니다.

## <a name="mac-keychain-errors"></a>Mac 키 집합 오류

MacOS 키 집합은 Storage 탐색기 인증 라이브러리에 대 한 문제를 일으키는 상태를 입력 하는 경우도 있습니다. 이 상태에서 키 집합을 가져오려면 다음 단계를 수행 합니다.

1. Storage Explorer를 닫습니다.
2. 키 집합을 열고 (명령 + 스페이스바를 누르고 키 **집합** 을 입력 한 다음 enter 키를 누릅니다.)
3. "로그인" 키 집합을 선택 합니다.
4. 자물쇠 아이콘을 선택 하 여 키 집합을 잠급니다. (프로세스가 완료 되 면 자물쇠가 잠깁니다. 열려 있는 앱에 따라 몇 초 정도 걸릴 수 있습니다.

    ![자물쇠 아이콘](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Storage Explorer를 엽니다.
6. "서비스 허브가 키 집합에 액세스 하려고 합니다."와 같은 메시지가 표시 됩니다. Mac 관리자 계정 암호를 입력 하 고 **항상 허용** 을 선택 합니다 (또는 **항상 허용** 을 사용할 수 없는 경우 **허용** ).
7. 로그인을 시도합니다.

### <a name="general-sign-in-troubleshooting-steps"></a>일반적인 로그인 문제 해결 단계

* MacOS를 사용할 때 **인증 대기** 대화 상자에 로그인 창이 표시 되지 않으면 [다음 단계](#mac-keychain-errors)를 수행 합니다.
* Storage 탐색기를 다시 시작 합니다.
* 인증 창이 비어 있는 경우 1분 이상 기다렸다가 인증 대화 상자를 닫습니다.
* 사용자 컴퓨터와 Storage 탐색기에 대해 프록시 및 인증서 설정이 적절히 구성 되어 있는지 확인 합니다.
* Windows를 실행 하는 경우 동일한 컴퓨터와 로그인 자격 증명에 Visual Studio 2019에 대 한 액세스 권한을 보유 한 경우 Visual Studio 2019에 로그인 해 보세요. Visual Studio 2019에 성공적으로 로그인 하면 Storage 탐색기 열고 계정 패널에서 계정을 볼 수 있습니다.

이러한 방법으로 문제가 해결 되지 않으면 [GitHub에서 문제를 엽니다](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>누락된 구독 및 손상된 테넌트

성공적으로 로그인 한 후 구독을 검색할 수 없는 경우 다음 문제 해결 방법을 시도해 보세요.

* 해당 계정이 원하는 구독에 액세스할 수 있는지 확인합니다. 사용 하려는 Azure 환경에 대 한 포털에 로그인 하 여 액세스를 확인할 수 있습니다.
* 올바른 Azure 환경 (Azure, Azure 중국 21Vianet, Azure 독일, Azure 미국 정부 또는 사용자 지정 환경)을 통해 로그인 했는지 확인 합니다.
* 프록시 서버 뒤에 있는 경우 Storage 탐색기 프록시가 올바르게 구성 되었는지 확인 합니다.
* 계정을 제거 하 고 다시 추가 해 보세요.
* "추가 정보" 링크가 있는 경우 실패 하는 테 넌 트에 대해 보고 되는 오류 메시지를 확인 합니다. 오류 메시지에 응답 하는 방법을 잘 모르는 경우 [GitHub에서 문제](https://github.com/Microsoft/AzureStorageExplorer/issues)를 자유롭게 열 수 있습니다.

## <a name="cant-remove-an-attached-account-or-storage-resource"></a>연결 된 계정 또는 저장소 리소스를 제거할 수 없습니다.

UI를 통해 연결 된 계정 또는 저장소 리소스를 제거할 수 없는 경우 다음 폴더를 삭제 하 여 연결 된 모든 리소스를 수동으로 삭제할 수 있습니다.

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> 이러한 폴더를 삭제 하기 전에 Storage 탐색기을 닫습니다.

> [!NOTE]
> SSL 인증서를 가져온 적이 있는 경우 디렉터리의 콘텐츠를 백업 `certs` 합니다. 다음에는 백업을 사용하여 SSL 인증서를 다시 가져올 수 있습니다.

## <a name="proxy-issues"></a>프록시 문제

Storage 탐색기는 프록시 서버를 통해 Azure Storage 리소스에 대 한 연결을 지원 합니다. 프록시를 통해 Azure에 연결 하는 데 문제가 발생 하는 경우 몇 가지 제안 사항이 있습니다.

> [!NOTE]
> Storage 탐색기는 프록시 서버에 대 한 기본 인증만 지원 합니다. NTLM과 같은 다른 인증 방법은 지원 되지 않습니다.

> [!NOTE]
> Storage 탐색기에서 프록시 설정을 구성 하는 데 프록시 자동 구성 파일을 지원 하지 않습니다.

### <a name="verify-storage-explorer-proxy-settings"></a>Storage 탐색기 프록시 설정 확인

**응용 프로그램 → 프록시 → 프록시 구성** 설정에 따라 프록시 구성을 가져오는 원본 Storage 탐색기 결정 됩니다.

"환경 변수 사용"을 선택 하는 경우 또는 환경 변수를 설정 해야 합니다 `HTTPS_PROXY` `HTTP_PROXY` . 환경 변수는 대/소문자를 구분 하므로 올바른 변수를 설정 해야 합니다. 이러한 변수가 정의 되어 있지 않거나 잘못 된 경우 Storage 탐색기 프록시를 사용 하지 않습니다. 환경 변수를 수정한 후 Storage 탐색기를 다시 시작 합니다.

"앱 프록시 설정 사용"을 선택 하는 경우 앱 내 프록시 설정이 올바른지 확인 하세요.

### <a name="steps-for-diagnosing-issues"></a>문제를 진단 하는 단계

여전히 문제가 발생 하는 경우 다음과 같은 문제 해결 방법을 시도해 보세요.

1. 프록시를 사용 하지 않고 인터넷에 연결할 수 있는 경우 프록시 설정을 사용 하지 않고 Storage 탐색기 작동 하는지 확인 합니다. Storage 탐색기 성공적으로 연결 되 면 프록시 서버에 문제가 있을 수 있습니다. 문제를 식별 하려면 관리자에 게 문의 하세요.
2. 프록시 서버를 사용 하는 다른 응용 프로그램이 예상 대로 작동 하는지 확인 합니다.
3. 사용 하려는 Azure 환경의 포털에 연결할 수 있는지 확인 합니다.
4. 서비스 엔드포인트에서 응답을 수신할 수 있는지 확인합니다. 엔드포인트 URL 중 하나를 브라우저에 입력합니다. 연결할 수 있는 경우 `InvalidQueryParameterValue` 또는 유사한 XML 응답을 받게 됩니다.
5. 동일한 프록시 서버에서 Storage 탐색기를 사용 하는 다른 사용자가 연결할 수 있는지 확인 합니다. 가능 하면 프록시 서버 관리자에 게 문의 해야 할 수 있습니다.

### <a name="tools-for-diagnosing-issues"></a>문제를 진단하기 위한 도구

Fiddler와 같은 네트워킹 도구는 문제를 진단 하는 데 도움이 될 수 있습니다.

1. 로컬 호스트에서 실행 되는 프록시 서버로 네트워킹 도구를 구성 합니다. 실제 프록시 뒤에서 작업을 계속 해야 하는 경우 프록시를 통해 연결 하도록 네트워킹 도구를 구성 해야 할 수 있습니다.
2. 네트워킹 도구에서 사용하는 포트 번호를 확인합니다.
3. 로컬 호스트 및 네트워킹 도구의 포트 번호 (예: "localhost: 8888")를 사용 하도록 프록시 설정을 Storage 탐색기 구성 합니다.
 
올바르게 설정 된 경우 네트워킹 도구는 Storage 탐색기에서 만든 네트워크 요청을 관리 및 서비스 끝점에 기록 합니다.
 
네트워킹 도구가 Storage 탐색기 트래픽을 로깅하는 것 같지 않은 경우 다른 응용 프로그램을 사용 하 여 도구를 테스트 해 보세요. 예를 들어 웹 브라우저에서 저장소 리소스 중 하나 (예:)에 대 한 끝점 URL을 입력 `https://contoso.blob.core.windows.net/` 하면 다음과 유사한 응답이 수신 됩니다.

  ![코드 샘플](./media/storage-explorer-troubleshooting/4022502_en_2.png)

  응답에는 액세스할 수 없는 경우에도 리소스가 있음을 알 수 있습니다.

네트워킹 도구에 다른 응용 프로그램의 트래픽만 표시 되는 경우 Storage 탐색기에서 프록시 설정을 조정 해야 할 수 있습니다. 그렇지 않으면 도구 설정을 조정 해야 합니다.

### <a name="contact-proxy-server-admin"></a>프록시 서버 관리자 문의

프록시 설정이 올바르면 프록시 서버 관리자에 게 다음을 문의 해야 할 수 있습니다.

* 프록시가 Azure 관리 또는 리소스 끝점에 대 한 트래픽을 차단 하지 않는지 확인 합니다.
* 프록시 서버에서 사용하는 인증 프로토콜을 확인합니다. Storage 탐색기는 기본 인증 프로토콜만 지원 합니다. Storage 탐색기는 NTLM 프록시를 지원 하지 않습니다.

## <a name="unable-to-retrieve-children-error-message"></a>"하위 항목을 검색할 수 없음" 오류 메시지

프록시를 통해 Azure에 연결 하는 경우 프록시 설정이 올바른지 확인 합니다.

구독 또는 계정의 소유자에 게 리소스에 대 한 액세스 권한이 부여 된 경우 해당 리소스에 대 한 읽기 또는 목록 권한이 있는지 확인 합니다.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>연결 문자열에 전체 구성 설정이 없습니다.

이 오류 메시지가 표시 되 면 저장소 계정에 대 한 키를 가져오는 데 필요한 권한이 없을 수 있습니다. 이 경우에 해당 하는지 확인 하려면 포털로 이동 하 여 저장소 계정을 찾습니다. 저장소 계정에 대 한 노드를 마우스 오른쪽 단추로 클릭 하 고 **포털에서 열기** 를 선택 하 여이 작업을 수행할 수 있습니다. 그런 다음 **액세스 키** 블레이드로 이동 합니다. 키를 볼 수 있는 권한이 없으면 "액세스할 수 없습니다." 라는 메시지가 표시 됩니다. 이 문제를 해결 하려면 다른 사용자의 계정 키를 가져와서 이름 및 키를 통해 연결 하거나, 다른 사용자에 게 저장소 계정에 대 한 SAS를 요청 하 고이를 사용 하 여 저장소 계정을 연결할 수 있습니다.

계정 키가 표시 되 면 문제를 해결 하는 데 도움이 될 수 있도록 GitHub에서 문제를 파일 합니다.

## <a name="error-occurred-while-adding-new-connection-typeerror-cannot-read-property-version-of-undefined"></a>새 연결을 추가 하는 동안 오류가 발생 했습니다. TypeError: 정의 되지 않은 ' version ' 속성을 읽을 수 없습니다.

사용자 지정 연결을 추가 하려고 할 때이 오류 메시지가 표시 되 면 로컬 자격 증명 관리자에 저장 된 연결 데이터가 손상 될 수 있습니다. 이 문제를 해결 하려면 손상 된 로컬 연결을 삭제 한 다음 다시 추가 하십시오.

1. Storage 탐색기를 시작 합니다. 메뉴에서 **도움말**  >  **설정/해제 개발자 도구** 로 이동 합니다.
2. 열린 창의 **응용 프로그램** 탭에서 **로컬 저장소** (왼쪽) > **file://** 로 이동 합니다.
3. 문제가 발생 하는 연결 유형에 따라 해당 키를 찾은 다음 해당 값을 텍스트 편집기에 복사 합니다. 값은 다음과 같이 사용자 지정 연결 이름으로 이루어진 배열입니다.
    * Storage 계정
        * `StorageExplorer_CustomConnections_Accounts_v1`
    * Blob 컨테이너
        * `StorageExplorer_CustomConnections_Blobs_v1`
        * `StorageExplorer_CustomConnections_Blobs_v2`
    * 파일 공유
        * `StorageExplorer_CustomConnections_Files_v1`
    * 큐
        * `StorageExplorer_CustomConnections_Queues_v1`
    * 테이블
        * `StorageExplorer_CustomConnections_Tables_v1`
4. 현재 연결 이름을 저장 한 후 개발자 도구의 값을로 설정 `[]` 합니다.

손상 되지 않은 연결을 유지 하려는 경우 다음 단계를 사용 하 여 손상 된 연결을 찾을 수 있습니다. 기존 연결을 모두 잃지 않는 경우 이러한 단계를 건너뛰고 플랫폼별 지침에 따라 연결 데이터를 지울 수 있습니다.

1. 텍스트 편집기에서 각 연결 이름을 개발자 도구에 다시 추가한 다음 연결이 계속 작동 하는지 확인 합니다.
2. 연결이 제대로 작동 하는 경우 손상 되지 않으므로 안전 하 게 유지할 수 있습니다. 연결이 작동 하지 않는 경우 개발자 도구에서 해당 값을 제거 하 고 나중에 다시 추가할 수 있도록 기록 합니다.
3. 모든 연결을 검사할 때까지 반복 합니다.

모든 연결을 이동한 후에는 다시 추가 되지 않는 모든 연결 이름에 대해 손상 된 데이터 (있는 경우)를 지우고 Storage 탐색기의 표준 단계를 사용 하 여 다시 추가 해야 합니다.

# <a name="windows"></a>[Windows](#tab/Windows)

1. **시작** 메뉴에서 **자격 증명 관리자** 를 검색 하 여 엽니다.
2. **Windows 자격 증명** 으로 이동 합니다.
3. **일반 자격 증명** 에서 키가 있는 항목을 찾습니다 `<connection_type_key>/<corrupted_connection_name>` (예: `StorageExplorer_CustomConnections_Accounts_v1/account1` ).
4. 이러한 항목을 삭제 하 고 연결을 다시 추가 합니다.

# <a name="macos"></a>[macOS](#tab/macOS)

1. 스포트라이트 (명령 + 스페이스바)를 열고 키 **집합 액세스** 를 검색 합니다.
2. 키가 있는 항목을 찾습니다 `<connection_type_key>/<corrupted_connection_name>` (예: `StorageExplorer_CustomConnections_Accounts_v1/account1` ).
3. 이러한 항목을 삭제 하 고 연결을 다시 추가 합니다.

# <a name="linux"></a>[Linux](#tab/Linux)

로컬 자격 증명 관리는 Linux 배포에 따라 다릅니다. Linux 배포판에서 로컬 자격 증명 관리에 대 한 기본 제공 GUI 도구를 제공 하지 않는 경우 타사 도구를 설치 하 여 로컬 자격 증명을 관리할 수 있습니다. 예를 들어 Linux 로컬 자격 증명을 관리 하기 위한 오픈 소스 GUI 도구인 [Seahorse](https://wiki.gnome.org/Apps/Seahorse/)를 사용할 수 있습니다.

1. 로컬 자격 증명 관리 도구를 열고 저장 된 자격 증명을 찾습니다.
2. 키가 있는 항목을 찾습니다 `<connection_type_key>/<corrupted_connection_name>` (예: `StorageExplorer_CustomConnections_Accounts_v1/account1` ).
3. 이러한 항목을 삭제 하 고 연결을 다시 추가 합니다.
---

이러한 단계를 실행 한 후에도이 오류가 발생 하거나 손상 된 것으로 의심 되는 항목을 공유 하려면 GitHub 페이지에서 [문제를 여세요](https://github.com/microsoft/AzureStorageExplorer/issues) .

## <a name="issues-with-sas-url"></a>SAS URL 문제

SAS URL을 통해 서비스에 연결 하 고 오류가 발생 하는 경우:

* URL이 리소스를 읽거나 나열하는 데 필요한 권한을 제공하는지 확인합니다.
* URL이 만료되지 않았는지 확인합니다.
* SAS URL이 액세스 정책을 기반으로 하는 경우 액세스 정책이 철회되지 않았는지 확인합니다.

잘못 된 SAS URL을 사용 하 여 실수로 연결 하 고 분리할 수 없는 경우 다음 단계를 수행 합니다.

1. Storage 탐색기를 실행 하는 경우 F12 키를 눌러 개발자 도구 창을 엽니다.
2. **응용 프로그램** 탭의 왼쪽 트리에서 **Local Storage**  >  **file://** 를 선택 합니다.
3. 문제가 있는 SAS URI의 서비스 유형과 연결된 키를 찾습니다. 예를 들어 잘못된 SAS URI가 Blob 컨테이너에 대한 것이면 `StorageExplorer_AddStorageServiceSAS_v1_blob`으로 명명된 키를 찾아봅니다.
4. 키의 값은 JSON 배열이어야 합니다. 잘못 된 URI와 연결 된 개체를 찾은 다음 삭제 합니다.
5. Storage Explorer를 다시 로드하려면 Ctrl+R 키를 누릅니다.

## <a name="linux-dependencies"></a>Linux 종속성

### <a name="snap"></a>Snap

Storage 탐색기 1.10.0 이상은 Snap 저장소에서 스냅으로 사용할 수 있습니다. Storage 탐색기 snap는 모든 종속성을 자동으로 설치 하며, 새 버전의 snap를 사용할 수 있게 되 면 업데이트 됩니다. 권장 설치 방법은 Storage 탐색기 snap를 설치 하는 것입니다.

Storage 탐색기를 사용 하려면 암호 관리자를 사용 해야 합니다. 암호 관리자는 Storage 탐색기 제대로 작동 하려면 수동으로 연결 해야 할 수도 있습니다. 다음 명령을 실행 하 여 시스템의 암호 관리자에 Storage 탐색기를 연결할 수 있습니다.

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

### <a name="targz-file"></a>release.tar.gz 파일

응용 프로그램을 release.tar.gz 파일로 다운로드할 수도 있지만 종속성을 수동으로 설치 해야 합니다.

Release.tar.gz 다운로드에 제공 된 Storage 탐색기 다음 버전의 Ubuntu에만 지원 됩니다. Storage 탐색기는 다른 Linux 배포판에서 작동할 수 있지만 공식적으로는 지원 되지 않습니다.

- Ubuntu 20.04 x64
- Ubuntu 18.04 x64
- Ubuntu 16.04 x64

Storage 탐색기를 사용 하려면 .NET Core가 시스템에 설치 되어 있어야 합니다. .NET Core 2.1를 사용 하는 것이 좋지만 Storage 탐색기 2.2 에서도 작동 합니다.

> [!NOTE]
> Storage 탐색기 버전 1.7.0 및 이전 버전에서는 .NET Core 2.0이 필요 합니다. 최신 버전의 .NET Core가 설치 되어 있는 경우 [Storage 탐색기 패치](#patching-storage-explorer-for-newer-versions-of-net-core)를 제공 해야 합니다. Storage 탐색기 1.8.0 이상 버전을 실행 하는 경우 .NET Core 2.1 이상이 필요 합니다.

# <a name="ubuntu-2004"></a>[Ubuntu 20.04](#tab/2004)

1. Storage 탐색기 release.tar.gz 파일을 다운로드 합니다.
2. [.Net Core 런타임](/dotnet/core/install/linux)설치:
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```

# <a name="ubuntu-1804"></a>[Ubuntu 18.04](#tab/1804)

1. Storage 탐색기 release.tar.gz 파일을 다운로드 합니다.
2. [.Net Core 런타임](/dotnet/core/install/linux)설치:
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```

# <a name="ubuntu-1604"></a>[Ubuntu 16.04](#tab/1604)

1. Storage 탐색기 release.tar.gz 파일을 다운로드 합니다.
2. [.Net Core 런타임](/dotnet/core/install/linux)설치:
   ```bash
   wget https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb; \
     sudo dpkg -i packages-microsoft-prod.deb; \
     sudo apt-get update; \
     sudo apt-get install -y apt-transport-https && \
     sudo apt-get update && \
     sudo apt-get install -y dotnet-runtime-2.1
   ```
---

Storage 탐색기에 필요한 많은 라이브러리가 표준 Ubuntu의 표준 설치와 함께 미리 설치 되어 제공 됩니다. 사용자 지정 환경에 이러한 라이브러리 중 일부가 없을 수 있습니다. Storage 탐색기를 시작 하는 데 문제가 있는 경우 다음 패키지가 시스템에 설치 되어 있는지 확인 하는 것이 좋습니다.

- iproute2
- libasound2
- libatm1
- libgconf2-4
- libnspr4
- libnss3
- libpulse0
- 기능 비밀-1-0
- libx11-xcb1
- libxss1
- libxtables11
- libxtst6
- xdg-유틸리티

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>최신 버전의 .NET Core에 대 한 Storage 탐색기 패치

Storage 탐색기 1.7.0 이전 버전의 경우 Storage 탐색기에서 사용 하는 .NET Core 버전을 패치 해야 할 수 있습니다.

1. [NuGet에서](https://www.nuget.org/packages/StreamJsonRpc/1.5.43)StreamJsonRpc의 1.5.43 버전을 다운로드 합니다. 페이지의 오른쪽에 있는 "패키지 다운로드" 링크를 찾습니다.
2. 패키지를 다운로드 한 후 파일 확장명을에서으로 변경 `.nupkg` `.zip` 합니다.
3. 패키지의 압축을 풉니다.
4. `streamjsonrpc.1.5.43/lib/netstandard1.1/` 폴더를 엽니다.
5. `StreamJsonRpc.dll`Storage 탐색기 폴더의 다음 위치에 복사 합니다.
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-the-azure-portal-doesnt-work"></a>Azure Portal에서 "탐색기에서 열기"가 작동 하지 않음

Azure Portal에서 **탐색기에서 열기** 단추가 작동 하지 않으면 호환 되는 브라우저를 사용 하 고 있는지 확인 합니다. 다음 브라우저는 호환성을 테스트 했습니다.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>다음 단계

이러한 해결 방법이 없는 경우 [GitHub에서 문제를 엽니다](https://github.com/Microsoft/AzureStorageExplorer/issues). 왼쪽 아래 모서리에 있는 **GitHub에 문제 보고** 단추를 선택 하 여이 작업을 수행할 수도 있습니다.

![사용자 의견](./media/storage-explorer-troubleshooting/feedback-button.PNG)