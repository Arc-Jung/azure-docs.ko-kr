---
title: Azure Dev Spaces 공유 하는 방법
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure에서 컨테이너 및 마이크로 서비스가 있는 Kubernetes 개발 환경을 빠르게 만듭니다.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s '
ms.openlocfilehash: 2c85625a4f61b701bc1e8b0a4a06f71dc0989ce0
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279986"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev Spaces 공유

Azure Dev Spaces를 사용하여 팀의 다른 사용자와 개발 공간을 공유할 수 있습니다. 각 개발자는 다른 사용자를 중단시킬 걱정 없이 자체 공간에서 작업할 수 있습니다. 또한 하나의 공간에서 함께 작업하면 모의 개체를 만들거나 종속성을 시뮬레이션할 필요 없이 엔드투엔드 코드를 테스트할 수 있습니다. 자세한 내용은 [팀 개발 알아보기](../team-development-nodejs.md) 가이드를 참조하세요.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>여러 개발자를 위한 개발 공간 설정

1. Azure에서 Dev Space를 만듭니다. [.NET Core 및 VS Code](../get-started-netcore.md), [.NET Core 및 Visual Studio](../get-started-netcore-visualstudio.md) 또는 [Node.js 및 VS Code](../get-started-nodejs.md)를 선택합니다. 선택한 Azure 구독에 대한 소유자 또는 기여자 액세스 권한이 있어야 합니다.
1. Azure Dev Space의 **리소스 그룹**을 각 팀 멤버에 대해 [기여자 액세스 권한을 부여](/azure/active-directory/role-based-access-control-configure)하도록 구성합니다. `azds list-up` 명령을 실행하여 개발 공간의 리소스 그룹을 확인할 수 있습니다.
1. 개발 공간에서 개발할 수 있도록 **개발 공간을 선택**하라고 팀 멤버에게 요청합니다.
   * **명령줄 또는 VS Code**: 액세스 권한이 있는 기존 Azure Dev Spaces를 보려면 `azds space list` 명령을 사용합니다. 개발 공간을 선택하려면 `azds space select` 명령을 사용합니다.
   * **Visual Studio IDE**: Visual Studio에서 프로젝트를 열고, 시작 설정 드롭다운에서 **Azure Dev Spaces**를 선택합니다. 열리는 대화 상자에서 기존 클러스터를 선택합니다.

     ![Visual Studio 시작 설정 드롭다운](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>다음 단계

자세한 내용은 [팀 개발 알아보기](../team-development-nodejs.md)를 참조하세요.
