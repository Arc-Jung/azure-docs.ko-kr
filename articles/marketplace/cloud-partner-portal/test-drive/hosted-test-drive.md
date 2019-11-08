---
title: 호스트 된 테스트 드라이브 | Azure Marketplace
description: Marketplace 호스트형 시험 사용을 설정하고 유지 관리하는 방법을 설명합니다.
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 2ce43402fb283cce09687bc7226770731f1bae0d
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827526"
---
# <a name="hosted-test-drive"></a>호스트형 시험 사용

호스트형 시험 사용을 진행하면 시험 사용 사용자 프로비저닝 및 프로비전 해제를 수행하는 서비스를 Microsoft에서 호스팅하고 유지 관리하므로 복잡한 설정을 수행할 필요가 없습니다. 이 문서에서는 AppSource에서 제품을 판매하거나 새 제품을 빌드하여 Dynamics 365 for Customer Engagement, Dynamics 365 for Finance and Operations 또는 Dynamics 365 Business Central instance와 연결되는 호스트형 시험 사용을 제공하려는 게시자를 위한 정보를 제공합니다.

## <a name="how-to-publish-a-test-drive"></a>시험 사용 게시 방법

기존 제품으로 이동하거나 새 제품을 만듭니다.

옆쪽 메뉴에서 시험 사용 옵션을 선택합니다.

\'시험 사용 사용\' 옵션에서 \'예\'를 선택합니다.

\'세부 정보\' 섹션에서 다음 필드에 정보를 입력합니다.

- **설명**: 시험 사용의 개요를 입력합니다. 이 텍스트는 시험 사용을 프로비전하는 동안 사용자에게 표시됩니다. 서식 있는 콘텐츠를 제공하려는 경우 이 필드는 HTML을 지원합니다.
- **사용자 매뉴얼**: 시험 사용 사용자가 앱 사용 방법을 이해하는 데 도움이 되는 세부 사용자 매뉴얼(.pdf 파일 형식)을 업로드합니다.
- **시험 사용 데모 비디오**: 필요에 따라 앱을 보여 주는 비디오를 업로드합니다.

[여기](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md)에 나와 있는 지침에 따라 테넌트의 시험 사용 사용자를 프로비전/프로비전 해제할 수 있는 권한을 AppSource에 부여합니다.

이 단계에서는 아래에 나와 있는 \'Azure AD 앱 ID\' 및 \'Azure AD 앱 키\'를 생성합니다.

\'기술 구성\' 섹션에서 다음 필드에 정보를 입력합니다.

- **시험 사용 유형**: \'Microsoft 호스팅(예: Dynamics 365 for Customer Engagement)' 옵션을 선택합니다. 이 옵션은 시험 사용 사용자 프로비저닝 및 프로비전 해제를 수행하는 서비스를 Microsoft에서 호스팅하고 유지 관리함을 나타냅니다.
- **최대 동시 시험 사용 수**: 이 필드의 값은 지정된 시점에 시험 사용을 활성화할 수 있는 동시 사용자의 수로 설정합니다. 각 사용자는 시험 사용을 활성화할 때 Dynamics 라이선스를 사용하므로 여기서 설정하는 시험 사용 사용자 수 이상의 Dynamics 라이선스가 있는지 확인해야 합니다. 권장 값은 3~5입니다.
- **시험 사용 기간(시간)** : 이 필드의 값은 사용자의 시험 사용이 활성 상태로 유지되는 시간으로 설정합니다. 이 시간이 지나면 사용자가 테넌트에서 프로비전 해제됩니다. 권장 값은 앱의 복잡도에 따라 2~24시간입니다. 시간이 만료된 후 시험 사용에 다시 액세스하려는 사용자는 언제든지 시험 사용을 또다시 요청할 수 있습니다.
- **인스턴스 URL**: 시험 사용 사용자가 시험 사용을 시작할 때 처음으로 이동할 URL을 입력하합니다. 일반적으로는 앱과 샘플 데이터가 설치된 Dynamics 365 인스턴스의 URL입니다. 값 예: https:\//testdrive.crm.dynamics.com
- **Azure AD 테넌트 ID**: Dynamics 365 인스턴스용 Azure 테넌트의 ID를 입력합니다. 이 값을 검색하려면 Azure Portal에 로그인하여 \'Azure Active Directory\' -\> 메뉴 블레이드에서 속성 선택 -\> 디렉터리 ID를 복사합니다. 예제 값은 72f988bf-86f1-41af-91ab-2d7cd0111234와 같습니다.
- **Azure AD 앱 ID**: 7단계에서 만든 Azure AD 앱의 ID입니다. 예제 값은 53852862-a2ae-4e43-9461-faa49650a096과 같습니다.
- **Azure AD 앱 키**: 7단계에서 만든 Azure AD 앱의 비밀입니다. 예제 값은 IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=와 같습니다.
- **Azure AD 테넌트 이름**: Dynamics 365 인스턴스용 Azure 테넌트의 이름을 입력합니다. \<tenantname.\>onmicrosoft.com 형식을 사용합니다. 예제 값은 testdrive.onmicrosoft.com과 같습니다.
- **인스턴스 웹 API URL**: Dynamics 365 인스턴스의 웹 API URL을 입력합니다. Microsoft Dynamics 365 인스턴스에 로그인하여 설정 -\> 사용자 지정 -\> 개발자 리소스 -\> 인스턴스 웹 API로 이동하면 이 값을 검색할 수 있습니다(인스턴스 웹 API URL을 복사해야 함). 값 예: https:\//testdrive.crm.dynamics.com/api/data/v9.0 
- **역할 이름**: 시험 사용을 위해 만든 사용자 지정 Dynamics 365 보안 역할의 이름을 입력합니다. 시험 사용 중에 사용자에게 이 역할이 할당됩니다. 예제 값: testdriverole

## <a name="next-steps"></a>다음 단계

앱이 인증을 통과하여 제품을 **게시**할 준비가 되면 제품 **미리 보기**가 표시됩니다. UI에서 시험 사용을 시작하여 시험 사용이 올바르게 실행되는지 확인합니다. 미리 보기 제품에 문제가 없는 것으로 확인되면 제품을 **라이브로 제공**할 수 있습니다.
