---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: c6666f4417cde9e0f77cc965ded1d6bdb5dced34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67182161"
---
## <a name="start-your-powershell-session"></a>PowerShell 세션 시작
 

[**Connect-Az Account**](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount) cmdlet을 실행하면 자격 증명을 입력할 수 있는 로그인 화면이 나타납니다. Azure 포털에 로그인하는 데 사용하는 동일한 자격 증명을 사용합니다.

```powershell
Connect-AzAccount
```

여러 구독이 있는 경우 [**Set-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet을 사용하여 PowerShell 세션이 사용해야 하는 구독을 선택합니다. 현재 PowerShell 세션이 사용 중인 구독을 보려면 [**Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext)를 실행합니다. 모든 구독을 보려면 [**Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)을 실행합니다.

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

