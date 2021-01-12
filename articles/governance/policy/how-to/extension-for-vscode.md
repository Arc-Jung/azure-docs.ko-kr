---
title: Visual Studio Code용 Azure Policy 확장
description: Visual Studio Code Azure Policy 확장을 사용 하 여 Azure Resource Manager 별칭을 조회 하는 방법에 대해 알아봅니다.
ms.date: 01/11/2021
ms.topic: how-to
ms.openlocfilehash: 4c4ba0eeb0506179ff92ead0ee86f048600d157e
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98107942"
---
# <a name="use-azure-policy-extension-for-visual-studio-code"></a>Visual Studio Code Azure Policy 확장 사용

> Azure Policy 확장 버전 **0.1.1** 이상에 적용 됩니다.

Visual Studio Code에 대 한 Azure Policy 확장을 사용 하 여 [별칭](../concepts/definition-structure.md#aliases)을 조회 하 고, 리소스 및 정책을 검토 하 고, 개체를 내보내고, 정책 정의를 평가 하는 방법을 알아봅니다. 먼저 Visual Studio Code에서 Azure Policy 확장을 설치 하는 방법을 설명 합니다. 그런 다음 별칭을 조회 하는 방법을 안내 합니다.

Visual Studio Code에 대 한 Azure Policy 확장은 Windows에 설치할 수 있습니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서의 단계를 완료하려면 다음 항목이 필요합니다.

- Azure 구독 Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/)을 만듭니다.
- [Visual Studio Code](https://code.visualstudio.com)

## <a name="install-and-configure-the-azure-policy-extension"></a>Azure Policy 확장 설치 및 구성

필수 구성 요소를 충족 한 후에는 다음 단계를 수행 하 여 Visual Studio Code에 대 한 Azure Policy 확장을 설치할 수 있습니다.

1. Visual Studio Code를 엽니다.
1. 메뉴 모음에서 확장 **보기** 로 이동  >  합니다.
1. 검색 상자에 **Azure Policy** 을 입력 합니다.
1. 검색 결과에서 **Azure Policy** 를 선택한 다음 **설치** 를 선택 합니다.
1. 필요한 경우 **다시 로드** 를 선택합니다.

국가별 클라우드 사용자의 경우 다음 단계에 따라 Azure 환경을 먼저 설정 합니다.

1. **File\Preferences\Settings** 를 선택 합니다.
1. 다음 문자열을 검색 합니다. _Azure: Cloud_
1. 목록에서 국가 클라우드를 선택 합니다.

   :::image type="content" source="../media/extension-for-vscode/set-default-azure-cloud-sign-in.png" alt-text="Visual Studio Code에 대 한 Azure cloud 로그인 국가를 선택 하는 스크린샷" border="false":::

## <a name="using-the-policy-extension"></a>정책 확장 사용

> [!NOTE]
> Visual Studio Code에 대 한 Azure Policy 확장에 표시 된 정책에서 로컬로 변경한 내용은 Azure와 동기화 되지 않습니다.

### <a name="connect-to-an-azure-account"></a>Azure 계정에 연결

리소스 및 조회 별칭을 평가 하려면 Azure 계정에 연결 해야 합니다. Visual Studio Code에서 Azure에 연결 하려면 다음 단계를 따르세요.

1. Azure Policy 확장 또는 명령 팔레트에서 Azure에 로그인 합니다.

   - Azure Policy 확장

     Azure Policy 확장에서 **Azure에 로그인** 을 선택 합니다.

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-policy-extension.png" alt-text="Visual Studio Code의 스크린샷 및 Azure Policy 확장에 대 한 아이콘입니다." border="false":::

   - 명령 팔레트

     메뉴 모음에서 **보기**  >  **명령 팔레트** 로 이동 하 고 **Azure: 로그인** 을 입력 합니다.

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-command-palette.png" alt-text="명령 팔레트의 Visual Studio Code에 대 한 Azure cloud 로그인 옵션의 스크린샷" border="false":::

1. 로그인 지침에 따라 Azure에 로그인 합니다. 연결 되 면 Azure 계정 이름이 Visual Studio Code 창의 아래쪽에 있는 상태 표시줄에 표시 됩니다.

### <a name="select-subscriptions"></a>구독 선택

처음 로그인 하면 Azure Policy 확장에 의해 기본 구독 리소스 및 정책만 로드 됩니다. 리소스 및 정책 표시에서 구독을 추가 하거나 제거 하려면 다음 단계를 수행 합니다.

1. 명령 팔레트 또는 창 바닥글에서 subscription 명령을 시작 합니다.

   - 명령 팔레트:

     메뉴 모음에서 **보기** > **명령 팔레트** 로 이동 하 고 **Azure: 구독 선택** 을 입력 합니다.

   - 창 바닥글

     화면 아래쪽의 창 바닥글에서 **Azure \<your account\>** 와 일치 하는 세그먼트를 선택 합니다.

1. 필터 상자를 사용 하 여 이름을 기준으로 신속 하 게 구독을 찾을 수 있습니다. 그런 다음 각 구독에서 확인을 확인 하거나 제거 하 여 Azure Policy 확장에 표시 된 구독을 설정 합니다. 표시할 구독을 추가 하거나 제거 하는 작업이 완료 되 면 **확인** 을 선택 합니다.

### <a name="search-for-and-view-resources"></a>리소스 검색 및 보기

Azure Policy 확장에는 리소스 공급자 및 리소스 그룹에서 선택한 구독에 대 한 리소스가 **리소스 창에** 나열 됩니다. Treeview는 선택한 구독 내 또는 구독 수준에서 다음과 같은 리소스 그룹을 포함 합니다.

- **리소스 공급자**
  - 정책 별칭이 있는 리소스 및 관련 자식 리소스를 사용 하 여 등록 된 각 리소스 공급자
- **리소스 그룹**
  - 속한 리소스 그룹의 모든 리소스

기본적으로 확장은 정책 별칭이 있는 리소스 및 기존 리소스를 기준으로 ' 리소스 공급자 '의 일부를 필터링 합니다.   >    >  필터링 하지 않고 모든 리소스 공급자를 보려면 **Azure Policy** 설정 확장에서이 동작을 변경 합니다.

단일 구독에서 수백 또는 수천 개의 리소스를 사용 하는 고객은 검색 가능한 방법을 사용 하 여 리소스를 찾을 수 있습니다. Azure Policy 확장을 사용 하면 다음 단계를 통해 특정 리소스를 검색할 수 있습니다.

1. Azure Policy 확장 프로그램 또는 명령 팔레트에서 검색 인터페이스를 시작 합니다.

   - Azure Policy 확장

     Azure Policy 확장에서 **리소스** 패널을 마우스로 가리키고 줄임표를 선택한 다음 **리소스 검색** 을 선택 합니다.

   - 명령 팔레트:

     메뉴 모음에서 **보기** > **명령 팔레트** 로 이동 하 고 **리소스 검색** 을 입력 합니다.

1. 표시 하기 위해 둘 이상의 구독을 선택 하는 경우 필터를 사용 하 여 검색할 구독을 선택 합니다.

1. 필터를 사용 하 여 이전에 선택한 구독의 일부인 검색할 리소스 그룹을 선택 합니다.

1. 필터를 사용 하 여 표시할 리소스를 선택 합니다. 필터는 리소스 이름과 리소스 유형 모두에 대해 작동 합니다.

### <a name="discover-aliases-for-resource-properties"></a>리소스 속성에 대 한 별칭 검색

검색 인터페이스를 통해 또는 treeview에서 리소스를 선택 하 여 리소스를 선택 하는 경우 Azure Policy 확장은 해당 리소스 및 모든 Azure Resource Manager 속성 값을 나타내는 JSON 파일을 엽니다.

리소스가 열리면 리소스 관리자 속성 이름 또는 값을 마우스로 가리키면 Azure Policy 별칭이 표시 됩니다 (있는 경우). 이 예제에서 리소스는 `Microsoft.Compute/virtualMachines` 리소스 형식이 고 **imageReference** 속성은 가리킴 속성입니다. 가리키기는 일치 하는 별칭을 표시 합니다.

:::image type="content" source="../media/extension-for-vscode/extension-hover-shows-property-alias.png" alt-text="별칭 이름을 표시 하는 속성을 Visual Studio Code 가리키기 위한 Azure Policy 확장의 스크린샷" border="false":::

> [!NOTE]
> VS Code 확장은 리소스 관리자 모드 속성의 평가만 지원 합니다. 모드에 대 한 자세한 내용은 [모드 정의](../concepts/definition-structure.md#mode)를 참조 하세요.

### <a name="search-for-and-view-policies-and-assignments"></a>정책 및 할당 검색 및 보기

Azure Policy 확장 **에는 정책 창에** 표시 하기 위해 선택한 구독에 대 한 treeview로 정책 형식 및 정책 할당이 나열 됩니다. 단일 구독에서 수백 또는 수천 개의 정책이 나 할당을 사용 하는 고객은 검색 가능한 방법을 사용 하 여 해당 정책 또는 할당을 찾을 수 있습니다. Azure Policy 확장을 사용 하면 다음 단계를 통해 특정 정책이 나 할당을 검색할 수 있습니다.

1. Azure Policy 확장 프로그램 또는 명령 팔레트에서 검색 인터페이스를 시작 합니다.

   - Azure Policy 확장

     Azure Policy 확장에서 **정책** 패널을 마우스로 가리키고 줄임표를 선택한 다음 **검색 정책** 을 선택 합니다.

   - 명령 팔레트:

     메뉴 모음에서 **보기** > **명령 팔레트** 로 이동 하 여 **정책: 검색 정책** 을 입력 합니다.

1. 표시 하기 위해 둘 이상의 구독을 선택 하는 경우 필터를 사용 하 여 검색할 구독을 선택 합니다.

1. 필터를 사용 하 여 이전에 선택한 구독의 일부인 검색할 정책 유형 또는 할당을 선택 합니다.

1. 필터를 사용 하 여 표시할 정책 또는 표시를 선택 합니다. 필터는 정책 정의 또는 정책 할당의 _displayName_ 에 대해 작동 합니다.

검색 인터페이스를 통해 또는 treeview에서이를 선택 하 여 정책 또는 할당을 선택 하는 경우 Azure Policy 확장은 정책 또는 할당과 모든 리소스 관리자 속성 값을 나타내는 JSON을 엽니다. 확장은 열린 Azure Policy JSON 스키마의 유효성을 검사할 수 있습니다.

### <a name="export-objects"></a>개체 내보내기

구독의 개체를 로컬 JSON 파일로 내보낼 수 있습니다. **리소스** 또는 **정책** 창에서 내보낼 수 있는 개체를 가리키거나 선택 합니다. 강조 표시 된 행의 끝에서 저장 아이콘을 선택 하 고 해당 리소스를 저장할 폴더를 선택 합니다.

다음 개체는 로컬로 내보낼 수 있습니다.

- 리소스 창
  - 리소스 그룹
  - 개별 리소스 (리소스 그룹 또는 리소스 공급자 아래에 있는)
- 정책 창
  - 정책 할당
  - 기본 제공 정책 정의
  - 사용자 지정 정책 정의
  - Initiatives

### <a name="on-demand-evaluation-scan"></a>주문형 평가 검사

평가 검사는 Visual Studio Code에 대 한 Azure Policy 확장을 사용 하 여 시작할 수 있습니다. 평가를 시작 하려면 리소스, 정책 정의 및 정책 할당과 같은 각 개체를 선택 하 고 고정 합니다.

1. 각 개체를 고정 하려면 **리소스** 창이 나 **정책** 창에서 찾아서 편집 탭에 고정 아이콘을 선택 합니다. 개체를 고정 하면 확장의 **평가** 창에 추가 됩니다.
1. **평가** 창에서 각 개체 중 하나를 선택 하 고 평가에 대해 선택 아이콘을 사용 하 여 평가에 포함 된 것으로 표시 합니다.
1. **평가** 창 맨 위에서 실행 평가 아이콘을 선택 합니다. 결과 평가 세부 정보가 JSON 형식으로 Visual Studio Code 새 창이 열립니다.

> [!NOTE]
> [AuditIfNotExists](../concepts/effects.md#auditifnotexists) 또는 [Deployifnotexists](../concepts/effects.md#deployifnotexists) 정책 정의의 경우 **계산** 창의 더하기 아이콘을 사용 하 여 존재 확인을 위한 _관련_ 리소스를 선택 합니다.

평가 결과에는 정책 정의 및 정책 할당에 대 한 정보와 함께 **policyEvaluations. evaluationResult** 속성이 제공 됩니다. 출력은 다음 예제와 같이 표시됩니다.

```json
{
    "policyEvaluations": [
        {
            "policyInfo": {
                ...
            },
            "evaluationResult": "Compliant",
            "effectDetails": {
                "policyEffect": "Audit",
                "existenceScope": "None"
            }
        }
    ]
}
```

> [!NOTE]
> VS Code 확장은 리소스 관리자 모드 속성의 평가만 지원 합니다. 모드에 대 한 자세한 내용은 [모드 정의](../concepts/definition-structure.md#mode)를 참조 하세요.
>
> 평가 기능은 macOS 및 Linux 확장 설치에서 작동 하지 않습니다.

### <a name="sign-out"></a>로그아웃

메뉴 모음에서 **보기**  >  **명령 팔레트** 로 이동한 다음 **Azure: 로그 아웃** 을 입력 합니다.

## <a name="next-steps"></a>다음 단계

- [Azure Policy 샘플](../samples/index.md)에서 예제를 검토합니다.
- [Azure Policy 정의 구조](../concepts/definition-structure.md)를 검토합니다.
- [정책 효과 이해](../concepts/effects.md)를 검토합니다.
- [프로그래밍 방식으로 정책을 생성](programmatically-create.md)하는 방법을 이해합니다.
- [규정 비준수 리소스를 수정](remediate-resources.md)하는 방법을 알아봅니다.
- [Azure 관리 그룹으로 리소스 구성](../../management-groups/overview.md)을 포함하는 관리 그룹을 검토합니다.
