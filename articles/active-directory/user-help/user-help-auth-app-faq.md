---
title: Microsoft Authenticator 앱에 대한 질문과 대답 - Azure AD
description: Microsoft Authenticator 앱 및 2단계 인증에 대한 FAQ(질문과 대답)입니다.
services: active-directory
author: curtand
manager: daveba
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 01/28/2021
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: 369ec11050fa7dbc09159d88793685ad5fbdb3e5
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99053963"
---
# <a name="frequently-asked-questions-faq-about-the-microsoft-authenticator-app"></a>Microsoft Authenticator 앱에 대 한 FAQ (질문과 대답)

이 문서에서는 Microsoft Authenticator 앱에 대한 일반적인 질문에 대답합니다. 질문에 대한 답변을 찾을 수 없는 경우 [Microsoft Authenticator 앱 포럼](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp)으로 이동하세요.

Microsoft Authenticator 앱은 Azure Authenticator 앱을 대체 했으며 Azure AD Multi-Factor Authentication를 사용 하는 경우 권장 되는 앱입니다. [Android](https://app.adjust.com/e3rxkc_7lfdtm?fallback=https%3A%2F%2Fplay.google.com%2Fstore%2Fapps%2Fdetails%3Fid%3Dcom.azure.authenticator) 및 [iOS](https://app.adjust.com/e3rxkc_7lfdtm?fallback=https%3A%2F%2Fitunes.apple.com%2Fus%2Fapp%2Fmicrosoft-authenticator%2Fid983156458) 디바이스의 경우 Microsoft Authenticator 앱을 사용할 수 있습니다.

## <a name="frequently-asked-questions"></a>질문과 대답

### <a name="registering-a-device"></a>디바이스 등록

**Q**: 회사 또는 서비스에 장치에 대 한 액세스 권한을 부여 하는 것에 동의 하는 장치를 등록 하 고 있습니까?

**A**: 장치를 등록 하면 조직의 서비스에 액세스할 수 있으며 조직에서 장치에 액세스할 수 없습니다.

### <a name="error-adding-account"></a>계정 추가 중 오류 발생

**Q**: 내 계정을 추가 하려고 하면 "추가 하려는 계정이 현재 유효 하지 않음" 이라는 오류 메시지가 표시 됩니다. 이 문제를 해결 하려면 관리자에 게 문의 하십시오 (고유성 유효성 검사). " 어떻게 해야 합니까?

**A**: 관리자에 게 연락 하 여 고유성 유효성 검사 문제로 인해 인증자에 게 계정을 추가할 수 없습니다. 관리자가 조직에서 볼 수 있도록 로그인 사용자 이름을 제공 해야 합니다.

### <a name="legacy-apns-support-deprecated"></a>레거시 APNs 지원 사용 되지 않음

**Q**: Apple Push Notification service에 대 한 레거시 이진 인터페이스가 11 월 2020에서 더 이상 사용 되지 않기 때문에 Microsoft Authenticator/Phone 요인을 계속 사용 하 여 로그인 하려면 어떻게 해야 하나요?

**A**: [Apple](https://developer.apple.com/news/?id=11042019a) 은 휴대폰 요소에 사용 되는 것과 같이 iOS 장치용 이진 인터페이스를 사용 하는 푸시 알림의 사용 중단을 발표 했습니다. 푸시 알림을 계속 받으려면 사용자가 인증자 앱을 최신 버전의 앱으로 업데이트 하는 것이 좋습니다. 그 동안에는 인증자 앱에서 알림을 수동으로 확인 하 여 해결할 수 있습니다.

### <a name="app-lock-feature"></a>앱 잠금 기능

**Q**: 앱 잠금의 정의 및이를 사용 하 여 더 안전 하 게 유지 하는 데 도움이 되는 방법

**A**: 앱 잠금은 일회성 인증 코드, 앱 정보 및 앱 설정을 보다 안전 하 게 유지 하는 데 도움이 됩니다. 앱 잠금이 설정 된 경우 인증자를 열 때마다 장치 PIN 또는 생체 인식을 사용 하 여 인증 하 라는 메시지가 표시 됩니다. 앱 잠금은 로그인 알림을 승인할 때마다 PIN 또는 생체 인식 메시지를 표시 하 여 알림을 승인할 수 있는 유일한 사용자 인지 확인 하는 데도 도움이 됩니다. 인증자 설정 페이지에서 앱 잠금을 설정 하거나 해제할 수 있습니다. 기본적으로 장치에서 PIN 또는 생체 인식을 설정 하면 앱 잠금이 켜 집니다.<br><br>아쉽게도 앱 잠금으로 인해 누군가가 인증자에 액세스 하지 못하도록 할 수 있는 것은 아닙니다. 장치 등록은 Android 계정 설정 또는 회사 포털 앱 등 인증자 외부의 다른 위치에서 발생할 수 있기 때문입니다.

### <a name="windows-mobile-retired"></a>Windows Mobile 사용 중지

**Q**: windows mobile 장치를 사용 하 고 있으며 windows mobile에서 Microsoft Authenticator는 사용 되지 않습니다. 앱을 사용 하 여 계속 인증할 수 있나요?

**A**: Windows Mobile에서 Microsoft Authenticator를 사용 하는 모든 인증은 2020 년 7 월 15 일 후에 사용이 중지 됩니다. 계정에서 잠기는 것을 방지 하려면 대체 인증 방법을 사용 하는 것이 좋습니다.<br>엔터프라이즈 사용자를 위한 대체 옵션은 다음과 같습니다.<br><ul><li>[Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) 또는 [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)에 대 한 Microsoft Authenticator 설정</li><li>확인 코드를 수신 하도록 [SMS 설정](multi-factor-authentication-setup-phone-number.md)</li><li>전화 통화를 수신 하는 전화 번호를 설정 [하 여 id를 확인](multi-factor-authentication-setup-office-phone.md)합니다.</li></ul><br>개인 Microsoft 계정 사용자를 위한 대체 옵션은 다음과 같습니다.<br><ul><li>[Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) 또는 [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)에 대 한 Microsoft Authenticator 설정</li><li>[Microsoft 계정 보안 페이지](https://account.microsoft.com/security/)에서 보안 정보를 업데이트 하 여 대체 로그인 방법 (SMS 또는 전자 메일)을 설정 합니다.</li></ul>

### <a name="android-screenshots"></a>Android 스크린샷

**Q**: Android AUTHENTICATOR에서 OTP (일회성 암호) 코드의 스크린샷을 사용할 수 있나요?

**A**: authenticator Android의 릴리스 6.2003.1704부터 인증자의 스크린샷 생성 될 때마다 기본적으로 모든 OTP 코드가 숨겨집니다. 스크린샷에서 OTP 코드를 보거나 다른 앱이 인증자 화면을 캡처하도록 허용 하려면를 사용 합니다. 인증자에서 **화면 캡처** 설정만 켜고 앱을 다시 시작 합니다.

### <a name="delete-stored-data"></a>저장 된 데이터 삭제

**Q**: 인증자에서 사용자를 대신 하 여 어떤 데이터를 삭제할 수 있나요?

**A**: Authenticator 앱은 다음과 같은 세 가지 유형의 정보를 수집 합니다.

- 사용자 계정을 추가할 때 제공하는 계정 정보 계정을 제거하여 이 데이터를 제거할 수 있습니다.
- 앱의 최상위 메뉴에서 **사용자 의견을 보내** Microsoft에 로그를 보낼 때까지 앱에만 남아 있는 진단 로그 데이터입니다. 이러한 로그에는 전자 메일 주소, 서버 주소, IP 주소 등의 개인 데이터가 포함 될 수 있습니다. 장치 이름 및 운영 체제 버전과 같은 장치 데이터를 포함할 수도 있습니다. 수집 된 모든 개인 데이터는 앱 문제를 해결 하는 데 필요한 정보로 제한 됩니다. 언제 든 지 앱에서 이러한 로그 파일을 탐색 하 여 수집 되는 정보를 확인할 수 있습니다. 로그 파일을 보내는 경우 인증 앱 엔지니어는 고객이 보고 한 문제를 해결 하는 데만 사용 합니다.
- "계정 흐름 추가 시작됨/성공적으로 계정을 추가함" 또는 "알림 승인됨"과 같은 비-개인 식별이 가능한 사용량 데이터. 이 데이터는 엔지니어링 결정의 필수적인 부분입니다. 사용량에 따라 사용자에 게 중요 한 방식으로 앱을 개선할 수 있는 위치를 결정 하는 데 도움이 됩니다. 앱을 처음 사용 하는 경우이 데이터 컬렉션에 대 한 알림이 표시 됩니다. 앱의 설정 페이지에서이 기능을 해제할 수 있음을 알려 줍니다 ****   . 언제 든 지이 설정을 켜거나 끌 수 있습니다.

### <a name="codes-in-the-app"></a>앱의 코드

**Q**: 앱의 코드는 무엇 인가요?

**A**: Authenticator를 열면 추가 된 계정이 타일로 표시 됩니다. 회사 또는 학교 계정 및 개인 Microsoft 계정에는 계정의 전체 화면 보기에 6 자리 또는 8 자리 숫자가 표시 됩니다 (계정 타일을 눌러 액세스). 다른 계정의 경우 앱의 **계정** 페이지에 6 자리 또는 8 자리 숫자가 표시 됩니다.<br>이러한 코드를 단일 사용 암호로 사용 하 여 사용자의 신원을 확인 합니다. 사용자 이름 및 암호를 사용하여 로그인한 후 해당 계정과 연결된 확인 코드를 입력합니다. 예를 들어 Contoso 계정에 Katy 로그인 하는 경우 계정 타일을 탭 한 다음 확인 코드 895823를 사용 합니다. Outlook 계정의 경우 동일한 단계를 수행 합니다.<br>Contoso 계정 타일을 탭 합니다.<br>![Authenticator 앱의 계정 타일](media/user-help-auth-app-faq/katy-signin.png)<br>Contoso 계정 타일을 탭 하면 확인 코드가 전체 화면에 표시 됩니다.<br>![인증자의 계정 타일에 있는 확인 코드](media/user-help-auth-app-faq/verification-code.png)

### <a name="countdown-timer"></a>카운트다운 타이머

**Q**: 코드 옆의 숫자가 계속 해 서 계산 되는 이유는 무엇 인가요?

**A**: 활성 확인 코드 옆에 30 초 타이머를 계산 하는 것을 볼 수 있습니다. 이 타이머는 같은 코드를 두 번 사용하여 로그인하는 일이 없도록 방지하기 위한 것입니다. 암호와는 달리, 이 숫자를 기억할 필요가 없습니다. 휴대폰에 액세스할 수 있는 사람만이 확인 코드를 알 수 있다는 점에 착안하였습니다.

### <a name="grayed-account-tile"></a>회색 계정 타일

**Q**: 내 계정 타일이 회색으로 표시 되는 이유는 무엇 인가요?

**A**: 일부 조직에서는 Single Sign-On와 함께 작동 하 고 조직 리소스를 보호 하는 인증 자가 필요 합니다. 이 경우 계정이 2단계 인증에 사용되지 않고 회색 또는 비활성 상태로 표시됩니다. 이러한 종류의 계정을 일반적으로 "브로커" 계정이라고 부릅니다.

### <a name="device-registration"></a>디바이스 등록

**Q**: 장치 등록 이란?
**A**: 조직에서는 파일 및 앱과 같은 보안 리소스에 대 한 액세스를 추적 하기 위해 장치를 등록 하도록 요구할 수 있습니다. 또한 조건부 액세스를 설정하여 해당 리소스가 원치 않는 방식으로 액세스될 위험을 줄일 수 있습니다. **설정** 에서 디바이스 등록을 취소할 수 있지만, Outlook의 이메일과 OneDrive의 파일에 액세스할 수 없게 될 수 있고 휴대폰 로그인 기능을 잃게 됩니다.

### <a name="verification-codes-when-connected"></a>연결 된 경우의 확인 코드

**Q**: 확인 코드를 가져오고 사용 하려면 인터넷 또는 내 네트워크에 연결 해야 하나요?

**A**: 코드에서 인터넷에 연결 하거나 데이터에 연결 하지 않아도 되기 때문에 로그인 할 때에는 전화 서비스가 필요 하지 않습니다. 또한 앱을 닫는 즉시 앱 실행이 중지되므로 배터리가 소모되지 않습니다.

### <a name="no-notifications-when-app-is-closed"></a>앱이 닫히면 알림이 없습니다.

**Q**: 앱이 열려 있는 경우에만 알림을 받으려면 어떻게 해야 하나요? 앱이 닫히면 알림이 수신 되지 않습니다.

**A**: 알림이 표시 되지만 경고가 아닌 경우에도 앱 설정을 확인 해야 합니다. 알림이 도착할 때 소리를 내거나 진동이 울리도록 앱을 설정해야 합니다. 알림이 전혀 수신 되지 않으면 다음 조건을 확인 해야 합니다.<ul><li>휴대폰이 방해 금지 또는 자동 모드인가요? 이 모드에서는 앱이 알림을 보내지 않을 수 있습니다.</li><li>다른 앱에서 알림을 받을 수 있나요? 받을 수 없으면 휴대폰의 네트워크 연결 문제이거나 Android 또는 Apple의 알림 채널에 문제가 있는 것일 수 있습니다. 전화 설정을 통해 네트워크 연결을 확인할 수 있습니다. Android 또는 Apple 알림 채널을 사용 하 여 도움을 받을 수 있도록 서비스 공급자에 게 문의 해야 할 수 있습니다.</li><li>앱에서 일부 계정의 알림을 받을 수 있고 나머지 계정의 알림을 받을 수 없나요? 그렇다면 앱에서 문제가 있는 계정을 제거했다가 다시 추가하고 알림을 허용한 다음, 문제가 해결되었는지 확인하세요.</li></ul>이러한 단계를 모두 시도한 후에도 문제가 지속되면 로그 파일을 보내주세요. 앱을 열고 앱의 최상위 메뉴로 이동한 다음 **사용자 의견 보내기** 를 선택 합니다. 그런 다음 [Microsoft Authenticator 앱 포럼](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) 으로 이동 하 여 표시 하는 문제와 시도한 단계를 Microsoft에 알려 주세요.

### <a name="switch-to-push-notifications"></a>푸시 알림으로 전환

**Q**: 앱에서 확인 코드를 사용 하 고 있지만 푸시 알림으로 전환 하려면 어떻게 해야 하나요?

**A**: 회사 또는 학교 계정 (관리자가 허용 하는 경우) 또는 개인 Microsoft 계정에 대 한 알림을 설정할 수 있습니다. Google 또는 Facebook과 같은 타사 계정에 대해서는 알림이 작동 하지 않습니다.<br>개인 계정을 알림으로 전환 하려면 계정을 사용 하 여 장치를 다시 등록 해야 합니다. 이렇게 하려면 **계정 추가** 로 이동하여 **개인 Microsoft 계정** 을 선택한 다음, 사용자 이름과 암호를 사용하여 로그인합니다.<br>조직에서는 회사 또는 학교 계정에 대해 한 번 클릭 알림 허용 여부를 결정 합니다.

### <a name="notifications-for-other-accounts"></a>다른 계정에 대 한 알림

**Q**: Microsoft 계정이 아닌 계정에 대 한 알림이 작동 하나요?

**A**: 아니요, 알림은 Microsoft 계정 및 Azure Active Directory 계정 에서만 작동 합니다. 회사 또는 학교에서 Azure AD 계정을 사용 하는 경우이 기능을 해제할 수 있습니다.

### <a name="backup-and-recovery"></a>Backup 및 복구

**Q**: 새 장치를가지고 있거나 백업에서 장치를 복원 했습니다. 인증자에서 내 계정을 다시 설정 어떻게 할까요??

**A**: 이전 장치에서 **클라우드 백업을** 설정한 경우 이전 백업을 사용 하 여 새 iOS 또는 Android 장치에서 계정 자격 증명을 복구할 수 있습니다. 자세한 내용은 Authenticator를 사용 하 [여 계정 자격 증명 백업 및 복구](user-help-auth-app-backup-recovery.md) 문서를 참조 하세요.

### <a name="lost-device"></a>장치 분실

**Q**: 장치를 분실 했거나 새 장치로 이동 했습니다. 알림이 계속해서 기존 디바이스로 이동하지 않게 하려면 어떻게 해야 하나요?

**A**: 새 장치에 인증자를 추가 해도 이전 장치에서 앱이 자동으로 제거 되지 않습니다. 기존 디바이스에서 앱을 삭제하는 것만으로는 충분하지 않습니다. 이전 장치에서 앱을 삭제 하 고 Microsoft 또는 조직에 이전 장치의 등록을 취소 하 고 등록을 취소 해야 합니다.<ul><li>**개인 Microsoft 계정을 사용하여 디바이스에서 앱을 제거하려면:** [계정 보안](https://account.microsoft.com/security) 페이지의 2단계 확인 영역으로 이동하여 기존 디바이스의 확인 기능을 끕니다.</li><li>**회사 또는 학교 Microsoft 계정을 사용하여 디바이스에서 앱을 제거하려면:** [Myapps 페이지](https://myapps.microsoft.com/) 또는 조직의 사용자 지정 포털의 2 단계 인증 영역으로 이동 하 여 이전 장치에 대 한 확인을 해제 합니다.</li></ul>

### <a name="remove-account-from-app"></a>앱에서 계정 제거

**Q**: 앱에서 계정을 제거할 어떻게 할까요? 있나요?

**A**: 계정 전체 화면을 보려면 앱에서 제거 하려는 계정에 대 한 계정 타일을 탭 합니다. **계정 제거** 를 탭 하 여 앱에서 계정을 제거 합니다.<br>조직에 등록 된 장치가 있는 경우 계정을 제거 하는 추가 단계가 필요할 수 있습니다. 이러한 장치에서 인증자는 자동으로 장치 관리자로 등록 됩니다. 앱을 완전히 제거하려면 먼저 앱 설정에서 앱 등록을 취소해야 합니다.

### <a name="too-many-permissions"></a>권한이 너무 많음

**Q**: 앱에서 많은 권한을 요청 하는 이유는 무엇 인가요?

**A**: 요청 될 수 있는 사용 권한의 전체 목록 및 앱에서 사용 하는 방법은 다음과 같습니다. 표시되는 특정 사용 권한은 사용하는 휴대폰의 종류에 따라 달라집니다.<ul><li>**위치**. 경우에 따라 특정 리소스에 액세스할 수 있도록 하기 전에 조직에서 사용자의 위치를 알고 싶을 수 있습니다. 앱은 조직에 위치가 필요한 정책이 있는 경우에만이 권한을 요청 합니다.</li><li>**생체 인식 하드웨어를 사용합니다.** 일부 회사 및 학교 계정에는 ID를 인증할 때마다 추가 PIN이 필요합니다. 앱을 사용하려면 PIN을 입력하는 대신 생체 인식 또는 얼굴 인식을 사용하는 데 동의해야 합니다.</li><li>**카메라.** 회사 계정, 학교 계정 또는 Microsoft 이외의 계정을 추가할 때 QR 코드를 검사하는 데 사용됩니다.</li><li>**연락처 및 휴대폰.** 휴대폰에서 회사 또는 학교 Microsoft 계정을 검색 하 고 앱에 추가 하려면 앱에이 권한이 필요 합니다.</li><li>**SMS.** 처음으로 개인 Microsoft 계정에 로그인 할 때 전화 번호가 레코드 번호와 일치 하는지 확인 하는 데 사용 됩니다. 6-8 자리 확인 코드를 포함 하는 앱을 설치한 휴대폰으로 문자 메시지를 보냅니다. 이 코드를 찾을 필요 없이 인증 자가 문자 메시지에서 자동으로 해당 코드를 검색 하기 때문에 입력 합니다.</li><li>**다른 앱을 통해 가져오기.** Id를 확인 하는 알림은 실행 중인 다른 앱에도 표시 됩니다.</li><li>**인터넷의 데이터 수신.** 알림을 보내기 위해 이 사용 권한이 필요합니다.</li><li>**휴대폰 절전 모드 전환 방지.** 디바이스를 조직에 등록하면 조직에서는 휴대폰에서 이 정책을 변경할 수 있습니다.</li><li>**진동 제어.** ID를 인증하는 알림을 수신할 때마다 진동을 울릴지 여부를 선택할 수 있습니다.</li><li>**지문 하드웨어 사용.** 일부 회사 및 학교 계정에는 ID를 인증할 때마다 추가 PIN이 필요합니다. 프로세스를 보다 쉽게 수행하려면 PIN을 입력하는 대신 지문을 사용할 수 있습니다.</li><li> **네트워크 연결 보기.** Microsoft 계정을 추가할 때 앱에는 네트워크/인터넷 연결이 필요합니다.</li><li>**스토리지의 내용 읽기.** 앱 설정을 통해 기술 문제를 보고하는 경우에만 이 사용 권한을 사용하게 됩니다. 스토리지의 일부 정보를 수집하여 문제를 진단합니다.</li><li>**전체 네트워크 액세스.** ID를 인증하는 알림을 보내는 데 이 사용 권한이 필요합니다.</li><li>**시작 시 실행.** 전화를 다시 시작하는 경우 이 사용 권한을 사용하면 ID를 인증하는 알림을 계속 수신할 수 있습니다.</li></ul>

### <a name="approve-requests-without-unlocking"></a>잠금 해제 하지 않고 요청 승인

**Q**: 인증자를 사용 하 여 장치를 잠금 해제 하지 않고 요청을 승인 하는 이유는 무엇 인가요?

**A**: 확인 해야 하는 것은 휴대폰을 보유 하 고 있기 때문에 확인 요청을 승인 하기 위해 장치 잠금을 해제할 필요가 없습니다. 2 단계 인증을 위해서는 사용자가 알고 있는 것과 사용자에 게 두 가지 사항을 입증 해야 합니다. 사용자가 알고 있는 것은 암호입니다. 사용자가 갖고 있는 것은 휴대폰 (인증을 사용 하 여 설정 하 고 multi-factor authentication 증명으로 등록)입니다. 따라서 전화를 하 고 요청을 승인 하면 두 번째 인증 단계에 대 한 조건을 충족 합니다.

### <a name="activity-notifications"></a>작업 알림

**Q**: 내 계정 활동에 대 한 알림이 표시 되는 이유는 무엇 인가요?

**A**: 활동 알림은 개인 Microsoft 계정이 변경 될 때마다 즉시 Authenticator에 전송 되므로 더 안전 하 게 유지할 수 있습니다. 이전에는 전자 메일과 SMS를 통해서만 이러한 알림을 보냈습니다. 이러한 작업 알림에 대한 자세한 내용은 [비정상적인 계정 로그인이 있는 경우 어떻게 되나요?](https://support.microsoft.com/help/13967/microsoft-account-unusual-sign-in)를 참조하세요. 알림을 받는 위치를 변경하려면 계정의 [단순 계정 알림과 관련하여 연락할 수 있는 메일 주소를 입력하세요.](https://account.live.com/SecurityNotifications/Update) 페이지에 로그인합니다.

### <a name="one-time-passcodes"></a>일회성 암호

**Q**: 내 일회성 암호 작동 하지 않습니다. 어떻게 해야 합니까?

**A**: 장치의 날짜와 시간이 올바르고 자동으로 동기화 되 고 있는지 확인 합니다. 날짜 및 시간이 잘못되었거나 동기화되지 않으면 코드가 작동하지 않습니다.

### <a name="windows-10-mobile"></a>Windows 10 Mobile

**Q**: Windows 10 모바일 운영 체제는 12 월 2019에 사용 되지 않습니다. Windows Mobile 운영 체제의 Microsoft Authenticator는 더 이상 사용되지 않습니다.

**A**: 2020 년 2 월 28 일 이후에는 모든 Windows 모바일 운영 체제의 인증 자가 지원 되지 않습니다. 사용자는 앞서 언급한 앱 게시물에 대한 새 업데이트를 받지 못합니다. 2020년 2월 28일 이후부터 모든 Windows Mobile 운영 체제에서 현재 Microsoft Authenticator를 사용하여 인증을 지원하는 Microsoft 서비스는 지원이 중지됩니다. Microsoft 서비스에서 인증을 받으려면 모든 사용자는 이 날짜 이전에 대체 인증 메커니즘으로 전환하는 것이 좋습니다.

### <a name="default-mail-app"></a>기본 메일 앱

**Q**: iOS와 함께 제공 되는 기본 메일 앱을 사용 하 여 내 회사 또는 학교 계정에 로그인 하는 동안 보안 인증 정보를 제공 하 라는 메시지가 표시 됩니다. 해당 정보를 입력하고 메일 앱으로 돌아가면 오류가 발생합니다. 어떻게 해야 하나요?

**A**:이는 두 개의 서로 다른 앱에서 로그인 및 메일 앱이 발생 하 여 초기 백그라운드 로그인 프로세스의 작동이 중지 되 고 실패 하는 경우에 발생할 수 있습니다. 이 문제를 해결하려면 메일 앱에 로그인하는 동안 화면의 오른쪽 아래에서 **Safari** 아이콘을 선택하는 것이 좋습니다. Safari로 이동하면 전체 로그인 프로세스가 단일 앱에서 발생하므로 앱에 성공적으로 로그인할 수 있습니다.

### <a name="apple-watch-watchos-7"></a>Apple Watch watchOS 7

**Q**: watchOS 7에서 Apple Watch 하는 데 문제가 있는 이유는 무엇 인가요?

**A**: watchOS 7에 대 한 알림을 승인 하는 데 문제가 있습니다 .이 문제를 해결 하기 위해 Apple에서 작업 하 고 있습니다. 그 동안에는 Microsoft Authenticator watchOS 앱이 필요한 모든 알림을 휴대폰에서 승인 해야 합니다.

### <a name="apple-watch-doesnt-show-accounts"></a>계정이 표시 되지 Apple Watch

**Q**: 내 Apple Watch에서 인증자를 열 때 일부 계정이 표시 되지 않는 이유는 무엇 인가요?

**A**: 인증자는 Apple Watch 동반 앱에서 푸시 알림이 있는 Microsoft 개인 또는 학교 또는 회사 계정만 지원 합니다. Google 또는 Facebook과 같은 다른 계정에 대해 휴대폰에서 인증 앱을 열어 확인 코드를 확인 해야 합니다.

### <a name="apple-watch-notifications"></a>Apple Watch 알림

**Q**: 내 Apple Watch에 대 한 알림을 승인 하거나 거부할 수 없는 이유는 무엇입니까?

**A**: 먼저 IPhone에서 Authenticator 버전 6.0.0 이상으로 업그레이드 했는지 확인 합니다. 그 후 Apple Watch에서 Microsoft Authenticator 도우미 앱을 열고 아래에 **설정** 단추가 있는 계정을 찾습니다. 해당 계정에 대 한 알림을 승인 하려면 설치 프로세스를 완료 합니다.

### <a name="apple-watch-communication-error"></a>Apple Watch 통신 오류

**Q**: Apple Watch와 내 전화 간에 통신 오류가 발생 했습니다. 어떻게 하면 문제를 해결할 수 있나요?

**A**:이 오류는 시계 화면이 전화 통신을 완료 하기 전에 절전 모드로 전환 될 때 발생 합니다.<br><b>설치 하는 동안 오류가 발생 하는 경우:</b><br>설치를 다시 실행하고 프로세스가 완료될 때가지 Watch가 활성화 상태를 유지하도록 합니다. 동시에, 휴대폰에서 앱을 열고 표시되는 모든 메시지에 응답합니다.<br>전화와 감시가 여전히 통신 하지 않는 경우 다음 작업을 시도할 수 있습니다.<ol><li>Microsoft Authenticator 휴대폰 앱을 강제로 종료하여 iPhone에서 다시 엽니다.</li><li>Apple Watch 도우미 앱을 강제로 종료합니다.<ol><li> Watch에서 Microsoft Authenticator 도우미 앱을 엽니다.</li><li>**종료** 화면이 나타날 때까지 옆면의 단추를 길게 누릅니다.</li><li>옆면의 단추를 놓고 Digital Crown을 길게 눌러 활성 앱을 강제로 종료합니다.</li></ol></li><li>휴대폰과 Watch에서 Bluetooth와 Wi-Fi를 모두 껐다가 다시 켭니다.</li><li>iPhone 및 Watch를 다시 시작합니다.</li></ol><b>알림을 승인 하려고 할 때 오류가 발생 하는 경우:</b><br>다음 번에 Apple Watch에서 알림을 승인하려고 할 때, 요청이 완료되고 성공적으로 완료되었음을 알리는 소리가 들릴 때까지 화면을 활성화 상태로 유지합니다.

### <a name="apple-watch-companion-app-not-syncing"></a>Apple Watch 자매 앱이 동기화 되지 않음

**Q**: Microsoft Authenticator 자매 앱이 Apple Watch 동기화 하거나 내 Watch에 표시 되지 않는 이유는 무엇 인가요?

**A**: 앱이 Watch에 표시 되지 않는 경우 다음 작업을 시도 합니다. <ol><li>Watch가 watchOS 4.0 이상을 실행 중인지 확인합니다.</li><li>Watch를 다시 동기화합니다.</li></ol>

### <a name="apple-watch-companion-app-crashed"></a>Apple Watch 자매 앱이 손상 되었습니다.

**Q**: 내 Apple Watch 도우미 앱이 충돌 합니다. 크래시 로그를 보내서 조사할 수 있나요?

**A**: 먼저 분석을 microsoft와 공유 하도록 선택 했는지 확인 해야 합니다. TestFlight 사용자인 경우 이미 등록되어 있습니다. 아닌 경우 **설정 > 개인 정보 > 분석** 으로 이동하여 **iPhone 및 Watch 분석 공유** 및 **앱 개발자와 공유** 옵션을 모두 선택합니다.<br>가입한 후 크래시를 재현하면 자동으로 크래시 로그가 Microsoft로 전송됩니다. 하지만 크래시를 재현할 수 없는 경우 로그 파일을 수동으로 복사해서 보내주시면 됩니다.<ol><li>휴대폰에서 Watch 앱을 열고 **설정 > 일반** 으로 이동한 다음, **Watch 분석 복사** 를 클릭합니다.</li><li>**설정 > 개인 정보 > 분석 > 분석 데이터** 아래에서 해당 크래시를 찾은 다음, 전체 텍스트를 수동으로 복사합니다.</li><li>휴대폰에서 Authenticator를 열고  **사용자 의견 보내기** 페이지에서 **문제가 발생 하** 는 경우 발생 하는  **문제를 설명** 합니다. 상자에 복사한 텍스트를 붙여넣습니다. </li></ol>

## <a name="autofill-with-authenticator"></a>인증자를 사용 하 여 자동 채우기

**Q**: 인증자를 사용 하 여 자동 채우기는 어떻게 되나요?

**A**: 인증 앱은 이제 휴대폰에서 방문한 앱 및 웹 사이트에 안전 하 게 저장 하 고 암호를 자동으로 채웁니다. 자동 채우기를 사용 하 여 iOS 및 Android 장치에서 암호를 동기화 하 고 자동 채우기를 수행할 수 있습니다. 휴대폰에서 인증자 앱을 자동 채우기 공급자로 설정한 후에는 사이트 또는 앱 로그인 페이지에 암호를 입력할 때 암호를 저장 하는 기능을 제공 합니다. 암호는 [개인 Microsoft 계정](https://account.microsoft.com/account) 의 일부로 저장 되며 개인 Microsoft 계정를 사용 하 여 Microsoft Edge에 로그인 하는 경우에도 사용할 수 있습니다.

**Q**: Authenticator에서 자동으로 자동 채우기를 수행할 수 있는 정보는 무엇 인가요?

**A**: 인증자는 휴대폰에서 방문 하는 사이트 및 앱에 대 한 사용자 이름 및 암호를 자동으로 입력할 수 있습니다.

**Q**: 내 휴대폰의 인증자에서 암호 자동 채우기를 켤 어떻게 할까요? 있나요?

**A**: 다음 단계를 수행 합니다.

1. Authenticator 앱을 엽니다.
1. 인증자의 **암호** 탭에서 **Microsoft에 로그인** 를 선택 하 고 [Microsoft 계정](https://account.microsoft.com/account)를 사용 하 여 로그인 합니다. 이 기능은 현재 Microsoft 계정만 지원 하며 회사 또는 학교 계정을 아직 지원 하지 않습니다.

**Q**: 내 휴대폰에서 인증자를 기본 자동 채우기 공급자로 설정 어떻게 할까요?

**A**: 다음 단계를 수행 합니다.

1. Authenticator 앱을 엽니다.
1. 앱 내의 **암호** 탭에서 **Microsoft에 로그인** 를 선택 하 고 [Microsoft 계정](https://account.microsoft.com/account)를 사용 하 여 로그인 합니다.
1. 다음 중 하나를 수행합니다.

   - IOS의 **설정** 에서 자동 채우기 설정 섹션에서 **자동 채우기 설정 방법** 을 선택 하 여 기본 자동 채우기 공급자로 인증자를 설정 하는 방법을 알아봅니다.
   - Android의 **설정** 에서 자동 채우기 설정 섹션에서 자동 **채우기 공급자로 설정** 을 선택 합니다.

**Q**: 설정에서 자동 **채우기** 스위치를 사용할 수 없는 경우는 어떻게 되나요?

**A**: 인증자에서 자동 채우기를 사용할 수 없는 경우 조직 또는 계정 유형에 대해 자동 채우기가 아직 허용 되지 않았기 때문일 수 있습니다. 회사 또는 학교 계정이 추가 되지 않은 장치에서이 기능을 사용할 수 있습니다. 조직에서 자동 채우기를 허용 하는 방법에 대 한 자세한 내용은 [IT 관리자를 위한 자동 채우기](#autofill-for-it-admins)를 참조 하세요.

**Q**: 암호 동기화를 중지 어떻게 할까요??

**A**: Authenticator 앱에서 암호 동기화를 중지 하려면 설정 자동   >  **채우기 설정**  >  **동기화 계정** 을 엽니다. 다음 화면에서 **동기화 중지 및 모든 자동 채우기 데이터 제거** 를 선택할 수 있습니다. 그러면 장치에서 암호 및 기타 자동 채우기 데이터가 제거 됩니다. 자동 채우기 데이터를 제거 해도 multi-factor authentication에는 영향을 주지 않습니다.

**Q**: 인증 앱에서 암호를 어떻게 보호 하나요?

**A**: Authenticator 앱은 이미 multi-factor authentication 및 계정 관리에 높은 수준의 보안을 제공 하 고, 동일한 높은 보안 막대를 통해 암호를 관리 하도록 확장 되었습니다.

- **Authenticator 앱에는 강력한 인증이 필요** 합니다. 인증자에 로그인 하려면 두 번째 요소가 필요 합니다. 즉, 누군가가 Microsoft 계정 암호를가지고 있는 경우에도 인증자 앱 내에서 암호를 보호 합니다.
- 자동 **채우기 데이터는 생체 인식 및 암호를 사용 하 여 보호 됩니다**. 앱 또는 사이트에서 암호를 자동으로 채우기 전에 인증자에 게 생체 인식 또는 장치 암호가 필요 합니다. 이렇게 하면 다른 사람이 장치에 액세스할 수 있는 경우에도 생체 인식 또는 장치 PIN 입력을 제공할 수 없기 때문에 암호를 채우거 나 볼 수 없도록 하는 추가 보안을 추가할 수 있습니다. 또한 사용자는 앱 설정에서 앱 잠금을 해제 하는 경우에도 생체 인식 또는 PIN을 제공 하지 않는 한 암호 페이지를 열 수 없습니다.
- **장치의 암호화 된 암호**: 장치의 암호는 암호화 되 고, 암호화/암호 해독 키는 저장 되지 않으며 필요할 때 항상 생성 됩니다. 암호는 사용자가 입력 하는 경우, 즉 자동 채우기 중에 또는 사용자가 암호를 확인 하려는 경우에만 해독 됩니다 .이 경우에는 모두 생체 인식 또는 PIN이 필요 합니다.
- **클라우드 및 네트워크 보안**: 클라우드의 암호는 장치에 도달 하는 경우에만 암호화 및 암호 해독 됩니다. 암호는 SSL로 보호 된 HTTPS 연결을 통해 동기화 되므로 공격자가 중요 한 데이터를 동기화 할 때 도청 하지 않도록 방지할 수 있습니다. 암호화 해시 된 함수 (특히 해시 기반 메시지 인증 코드)를 사용 하 여 네트워크를 통해 동기화 되는 데이터의 온전성도 확인 합니다.

## <a name="autofill-for-it-admins"></a>IT 관리자를 위한 자동 채우기

**Q**: 내 직원 또는 학생이 Authenticator 앱에서 암호 자동 채우기를 사용 하나요?

**A**: 예, 자동 채우기는 회사 또는 학교 계정이 인증자 앱에 추가 된 경우에도 대부분의 엔터프라이즈 사용자에 대해 작동 합니다. 양식을 작성 하 여 조직에 대 한 자동 채우기를 구성 (허용 또는 거부) 하 고 [인증자 팀에 보낼](https://aka.ms/ConfigureAutofillInAuthenticator)수 있습니다.

**Q**: 내 사용자의 회사 또는 학교 계정 암호를 자동으로 동기화 하나요?

**A**: 아니요. 암호 자동 채우기는 사용자의 회사 또는 학교 계정 암호를 동기화 하지 않습니다. 사용자가 사이트 또는 앱을 방문 하면 인증 자가 해당 사이트나 앱에 대 한 암호를 저장 하 고, 사용자가를 선택 하는 경우에만 암호가 저장 됩니다.
  
**Q**: 내 조직의 특정 사용자만 자동 채우기를 allowlist 수 있나요?

**A**: 아니요. 현재 기업에서는이 시점에서 직원 중 일부 또는 모두에 대해 암호 자동 채우기를 사용 하도록 설정할 수 있습니다.

**Q**: 직원 또는 학생에 게 여러 회사 또는 학교 계정이 있는 경우 어떻게 되나요? 예를 들어 직원의 Microsoft Authenticator에는 여러 기업 또는 학교 계정이 있습니다.

**A**: authenticator 앱에 추가 된 모든 기업 또는 학교는 앱 소유자가 사용할 수 있도록 인증자에서 자동 채우기를 허용 하도록 허용 되어야 합니다. 이러한 제한에 대 한 한 가지 예외는 직원 또는 학생이 [외부 또는 타사 계정](user-help-auth-app-add-non-ms-account.md)으로 Microsoft 클라우드 기반 multi-factor authentication에 회사 또는 학교 계정을 추가 하는 경우입니다.

## <a name="next-steps"></a>다음 단계

- 개인 Microsoft 계정에 대한 확인 코드를 가져오는 데 문제가 있는 경우 **Microsoft 계정 보안 정보 및 확인 코드** 문서에서 [ 확인 코드 문제 해결](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes) 섹션을 참조하세요.

- 2단계 인증에 대해 더 자세히 알아보려면 [2단계 인증에 내 계정 설정](multi-factor-authentication-end-user-first-time.md)을 참조하세요.

- 보안 정보에 대한 자세한 내용은 [보안 정보(미리 보기) 개요](./security-info-setup-signin.md)를 참조하세요.

- 궁금한 내용에 대한 답변이 여기에 없으면 직접 질문을 남겨주세요. [Microsoft Authenticator 앱 포럼](https://social.technet.microsoft.com/Forums/en-us/home?forum=MicrosoftAuthenticatorApp)으로 이동하여 질문을 게시하고, 커뮤니티의 도움을 받고, 이 페이지에 의견을 남기세요.
