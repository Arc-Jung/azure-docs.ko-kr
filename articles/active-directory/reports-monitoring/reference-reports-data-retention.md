---
title: Azure AD 저장소에서 데이터를 보고 하는 기간은 얼마 인가요? | Microsoft 문서
description: Azure에서 다양 한 유형의 보고 데이터를 저장 하는 기간을 알아봅니다.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 03/24/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54636600c208f8f5df9fa2e25460c63dd9f46e85
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80239556"
---
# <a name="how-long-does-azure-ad-store-reporting-data"></a>Azure AD 저장소에서 데이터를 보고 하는 기간은 얼마 인가요?


이 문서에서는 Azure Active Directory의 다른 작업 보고서에 대한 데이터 보존 정책에 대해 알아봅니다. 

### <a name="when-does-azure-ad-start-collecting-data"></a>Azure AD는 언제 데이터 수집을 시작하나요?

| Azure AD 버전 | 수집 시작 |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | 구독을 위한 가입 시기 |
| Azure AD Free| 처음으로 [Azure Active Directory 블레이드](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview)를 열거나 [보고 API](https://aka.ms/aadreports)를 사용할 때  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>Azure Portal에서 활동 데이터를 사용할 수 있는 시기는 언제입니까?

- **즉시** -Azure Portal에서 보고서를 이미 작업 한 경우
- **2시간 이내** - Azure Portal에서 보고 기능을 설정하지 않은 경우

---

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Premium 라이선스를 받은 후 활동 데이터를 확인할 수 있을 때까지는 얼마나 걸리나요?

무료 라이선스를 통해 수집한 활동 데이터가 이미 있다면 업그레이드 시 해당 데이터를 즉시 확인할 수 있습니다. 데이터가 없는 경우에는 Premium 라이선스로 업그레이드한 후 보고서에 데이터가 나타나기까지 1~2일 정도 걸립니다.

---

### <a name="can-i-see-last-months-data-after-getting-an-azure-ad-premium-license"></a>Azure AD Premium 라이선스를 받은 후에 지난 달의 데이터를 확인할 수 있나요?

최근에 Premium 버전(평가판 버전을 포함)으로 전환한 경우 처음에는 최대 7일 동안의 데이터를 확인할 수 있습니다. 데이터가 누적되면 지난 30일간의 데이터를 볼 수 있습니다.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>Azure AD는 언제 보안 신호 데이터 수집을 시작하나요?  

보안 신호의 경우 **ID 보호 센터**를 사용하도록 옵트인할 때 수집 프로세스가 시작됩니다. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>Azure AD는 데이터를 얼마나 오래 저장하나요?

**작업 보고서**    

| 보고서                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| 감사 로그             | 7 일        | 30일             | 30일             |
| 로그인               | 7 일        | 30일             | 30일             |
| Azure MFA 사용        | 30일       | 30일             | 30일             |

Azure Monitor를 사용하여 스토리지 계정으로 라우팅하여 위에서 설명한 기본 보존 기간보다 오랫동안 감사 및 로그인 활동 데이터를 유지할 수 있습니다. 자세한 내용은 [Azure 스토리지 계정에 Azure AD 로그 보관](quickstart-azure-monitor-route-logs-to-storage-account.md)을 참조하세요.

**보안 신호**

| 보고서         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| 위험에 노출된 사용자  | 7 일        | 30일             | 90일             |
| 위험한 로그인 | 7 일        | 30일             | 90일             |

---
