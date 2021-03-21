---
title: Azure 파일 동기화 에이전트 배포
description: Azure 파일 동기화 에이전트를 배포 합니다. 마이그레이션 문서 전체에서 공유 되는 일반 텍스트 블록
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: f038392f03b94aa2c2450531c9da4a11d9900295
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93043120"
---
이 섹션에서는 Windows Server 인스턴스에 Azure 파일 동기화 에이전트를 설치 합니다.

[배포 가이드](../articles/storage/files/storage-sync-files-deployment-guide.md) 에서는 **Internet Explorer 보안 강화 구성을** 해제 해야 함을 보여 줍니다. 이 보안 조치는 Azure 파일 동기화에는 적용 되지 않습니다. 이 기능을 해제 하면 문제 없이 Azure에 인증할 수 있습니다.

PowerShell을 열고 다음 명령을 사용 하 여 필요한 PowerShell 모듈을 설치 합니다. 메시지가 표시 되 면 전체 모듈 및 NuGet 공급자를 설치 해야 합니다.

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

서버에서 인터넷에 연결 하는 데 문제가 있는 경우 지금 해결할 시간입니다. Azure 파일 동기화은 인터넷에 대 한 사용 가능한 네트워크 연결을 사용 합니다. 프록시 서버를 인터넷에 연결 해야 하는 것도 지원 됩니다. 에이전트를 설치 하는 동안 시스템 차원 프록시를 지금 구성 하거나 Azure 파일 동기화만 사용할 프록시를 지정할 수 있습니다.

프록시를 구성 하는 경우이 서버에 대 한 방화벽을 열어야 하므로 해당 방법이 허용 될 수 있습니다. 서버를 설치 하 고 나면 서버 등록을 완료 한 후 네트워크 연결 보고서에 Azure에서 사용자가 선택한 지역에 대해와 통신 해야 하 Azure 파일 동기화는 정확한 끝점 Url이 표시 됩니다. 또한이 보고서는 통신이 필요한 이유를 알려 줍니다. 이 보고서를 사용 하 여이 서버 주변의 방화벽을 특정 Url로 잠글 수 있습니다.

방화벽 전체를 열지 않고 상위 수준 DNS 네임 스페이스와 통신 하도록 서버를 제한 하는 보다 보수적인 접근 방식을 따를 수도 있습니다. 자세한 내용은 [프록시 및 방화벽 설정 Azure 파일 동기화](../articles/storage/files/storage-sync-files-firewall-and-proxy.md)을 참조 하세요. 사용자 고유의 네트워킹 모범 사례를 따르세요.

서버 설치 마법사가 끝날 때 서버 등록 마법사가 열립니다. 이전에서 저장소 동기화 서비스의 Azure 리소스에 서버를 등록 합니다.

이러한 단계는 배포 가이드에 자세히 설명 되어 있습니다. 여기에는 먼저 설치 해야 하는 PowerShell 모듈이 포함 됩니다. [에이전트 설치를 Azure 파일 동기화](../articles/storage/files/storage-sync-files-deployment-guide.md)합니다.

최신 에이전트를 사용 합니다. Microsoft 다운로드 센터: [Azure 파일 동기화 에이전트](https://aka.ms/AFS/agent "Azure 파일 동기화 에이전트 다운로드")에서 다운로드할 수 있습니다.

성공적으로 설치 하 고 서버를 등록 한 후에는이 단계가 성공적으로 완료 되었는지 확인할 수 있습니다. Azure Portal의 저장소 동기화 서비스 리소스로 이동 합니다. 왼쪽 메뉴에서 **등록 된 서버** 로 이동 합니다. 여기에 나열 된 서버가 표시 됩니다.
