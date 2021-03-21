---
title: Windows 가상 데스크톱 (클래식)에서 기본 제공 앱 게시-Azure
description: Windows 가상 데스크톱 (클래식)에서 기본 제공 앱을 게시 하는 방법입니다.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 1b179ac555ea86aff381c1217e834b8d0aa85e8c
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102561707"
---
# <a name="publish-built-in-apps-in-windows-virtual-desktop-classic"></a>Windows 가상 데스크톱에서 기본 제공 앱 게시 (클래식)

>[!IMPORTANT]
>이 콘텐츠는 Azure Resource Manager Windows Virtual Desktop 개체를 지원하지 않는 Windows Virtual Desktop(클래식)에 적용됩니다. Azure Resource Manager Windows Virtual Desktop 개체를 관리하려는 경우 [이 문서](../publish-apps.md)를 참조하세요.

이 문서에서는 Windows 가상 데스크톱 환경에서 앱을 게시 하는 방법을 설명 합니다.

## <a name="publish-built-in-apps"></a>기본 제공 앱 게시

기본 제공 앱을 게시 하려면:

1. 호스트 풀의 가상 컴퓨터 중 하나에 연결 합니다.
2. [이 문서의](/powershell/module/appx/get-appxpackage)지침에 따라 게시 하려는 앱의 **PackageFamilyName** 를 가져옵니다.
3. 마지막으로, `<PackageFamilyName>` 이전 단계에서 찾은 **PackageFamilyName** 로 교체 하 여 다음 cmdlet을 실행 합니다.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:appsFolder\<PackageFamilyName>!App"
   ```

>[!NOTE]
> Windows 가상 데스크톱은로 시작 하는 설치 위치로 앱 게시만 지원 `C:\Program Files\Windows Apps` 합니다.

## <a name="update-app-icons"></a>앱 아이콘 업데이트

앱을 게시 한 후에는 일반 아이콘 그림 대신 기본 Windows 앱 아이콘이 표시 됩니다. 아이콘을 일반 아이콘으로 변경 하려면 원하는 아이콘 이미지를 네트워크 공유에 배치 합니다. 지원 되는 이미지 형식은 PNG, BMP, GIF, JPG, JPEG 및 ICO입니다.

## <a name="publish-microsoft-edge"></a>Microsoft Edge 게시

Microsoft Edge를 게시 하는 데 사용 하는 프로세스는 다른 앱의 게시 프로세스와 약간 다릅니다. 기본 홈 페이지를 사용 하 여 Microsoft Edge를 게시 하려면이 cmdlet을 실행 합니다.

```powershell
New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -FriendlyName <remoteappname> -FilePath "shell:Appsfolder\Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge"
```

## <a name="next-steps"></a>다음 단계

- 피드를 구성 하 여 [Windows 가상 데스크톱 사용자의 피드 사용자 지정](customize-feed-virtual-desktop-users-2019.md)에서 사용자에 대해 앱이 표시 되는 방식을 구성 하는 방법에 대해 알아봅니다.
- Msix 앱 연결 [설정](../app-attach.md)에서 msix 앱 연결 기능에 대해 알아봅니다.

