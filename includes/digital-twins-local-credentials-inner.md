---
author: baanders
description: 포함 파일 - Azure Digital Twins 샘플에서 DefaultAzureCredential에 대한 로컬 인증 설정(소개 없음)
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: fb148551db798207a52bd7aef629da79dd3341e1
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102212820"
---
`DefaultAzureCredential`을 사용하면 샘플에서 로컬 [Azure CLI](/cli/azure/install-azure-cli) 또는 Visual Studio 또는 Visual Studio Code의 Azure 로그인과 같은 로컬 환경에서 자격 증명을 검색합니다. 이러한 이유로 샘플에 대한 자격 증명을 설정하려면 이러한 메커니즘 중 하나를 통해 *Azure에 로컬로 로그인* 해야 합니다.

Visual Studio 또는 Visual Studio Code를 사용하여 코드 샘플을 실행하는 경우 Azure Digital Twins 인스턴스에 액세스하는 데 사용하려는 것과 동일한 Azure 자격 증명을 사용하여 해당 편집기에 로그인했는지 확인합니다.

그렇지 않으면 [로컬 Azure CLI를 설치](/cli/azure/install-azure-cli)하고, 머신에서 명령 프롬프트를 시작하고, `az login` 명령을 실행하여 Azure 계정에 로그인할 수 있습니다. 로그인한 후 코드 샘플을 실행하면 자동으로 로그인됩니다.