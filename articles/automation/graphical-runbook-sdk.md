---
title: Azure 자동화 그래픽 런북 SDK 사용
description: 이 문서에서는 Azure 자동화 그래픽 runbook SDK를 사용하는 방법을 설명합니다.
services: automation
ms.subservice: process-automation
ms.date: 07/20/2018
ms.topic: conceptual
ms.openlocfilehash: 21f6ff8078d5a1db88b2fde33c9063a56b3ee43a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682915"
---
# <a name="use-the-azure-automation-graphical-runbook-sdk"></a>Azure 자동화 그래픽 런북 SDK 사용

[그래픽 런북은](automation-graphical-authoring-intro.md) 기본 Windows PowerShell 또는 PowerShell 워크플로 코드의 복잡성을 관리하는 데 도움이 됩니다. Microsoft Azure 자동화 그래픽 작성 SDK를 사용하면 개발자가 Azure 자동화와 함께 사용할 그래픽 런북을 만들고 편집할 수 있습니다. 이 문서에서는 코드에서 그래픽 런북을 만드는 기본 단계를 설명합니다.

## <a name="prerequisites"></a>사전 요구 사항

패키지를 `Microsoft.Azure.Automation.GraphicalRunbook.Model` 프로젝트로 가져옵니다.

## <a name="create-a-runbook-object-instance"></a>Runbook 개체 인스턴스 만들기

`Orchestrator.GraphRunbook.Model` 어셈블리를 참조하고 `Orchestrator.GraphRunbook.Model.GraphRunbook` 클래스의 인스턴스를 만듭니다.

```csharp
using Orchestrator.GraphRunbook.Model;
using Orchestrator.GraphRunbook.Model.ExecutableView;

var runbook = new GraphRunbook();
```

## <a name="add-runbook-parameters"></a>Runbook 매개 변수 추가

`Orchestrator.GraphRunbook.Model.Parameter` 개체를 인스턴스화하고 Runbook에 추가합니다.

```csharp
runbook.AddParameter(
 new Parameter("YourName") {
  TypeName = typeof(string).FullName,
   DefaultValue = "World",
   Optional = true
 });

runbook.AddParameter(
 new Parameter("AnotherParameter") {
  TypeName = typeof(int).FullName, ...
 });
```

## <a name="add-activities-and-links"></a>작업 및 링크 추가

적절한 유형의 작업을 인스턴스화하고 Runbook에 추가합니다.

```csharp
var writeOutputActivityType = new CommandActivityType {
 CommandName = "Write-Output",
  ModuleName = "Microsoft.PowerShell.Utility",
 InputParameterSets = new [] {
  new ParameterSet {
   Name = "Default",
    new [] {
     new Parameter("InputObject"), ...
    }
  },
  ...
 }
};

var outputName = runbook.AddActivity(
 new CommandActivity("Output name", writeOutputActivityType) {
  ParameterSetName = "Default",
   Parameters = new ActivityParameters {
    {
     "InputObject",
     new RunbookParameterValueDescriptor("YourName")
    }
   },
   PositionX = 0,
   PositionY = 0
 });

var initializeRunbookVariable = runbook.AddActivity(
 new WorkflowScriptActivity("Initialize variable") {
  Process = "$a = 0",
   PositionX = 0,
   PositionY = 100
 });
```

활동은 `Orchestrator.GraphRunbook.Model` 네임스페이스의 다음 클래스에 의해 구현됩니다.

|클래스  |작업  |
|---------|---------|
|CommandActivity     | PowerShell 명령(cmdlet, 함수 등)을 호출합니다.        |
|InvokeRunbookActivity     | 다른 Runbook 인라인을 호출합니다.        |
|JunctionActivity     | 들어오는 모든 분기가 완료될 때까지 기다립니다.        |
|WorkflowScriptActivity     | Runbook의 컨텍스트에서 PowerShell 또는 PowerShell 워크플로 코드의 블록을 실행합니다(Runbook 유형에 따라). 강력한 도구이지만 과도하게 사용하지 마십시오. UI는 이 스크립트 블록을 텍스트로 표시합니다. 실행 엔진은 제공된 블록을 검은색 상자로 처리하고 기본 구문 검사를 제외하고 해당 콘텐츠를 분석하는 시도를 하지 않습니다. 단일 PowerShell 명령을 호출해야 하는 경우 CommandActivity를 사용합니다.        |

> [!NOTE]
> 제공된 클래스에서 사용자 고유의 활동을 파생하지 마십시오. Azure Automation은 사용자 지정 활동 유형이 있는 Runbook을 사용할 수 없습니다.

직접 값이 `CommandActivity` 아닌 값 설명자로 매개 변수를 제공해야 합니다. `InvokeRunbookActivity` 값 설명자는 실제 매개 변수 값을 생성하는 방법을 지정합니다. 다음 값 설명자가 현재 제공됩니다.


|설명자  |정의  |
|---------|---------|
|ConstantValueDescriptor     | 하드 코드된 상수 값을 나타냅니다.        |
|RunbookParameterValueDescriptor     | 이름으로 Runbook 매개 변수를 나타냅니다.        |
|ActivityOutputValueDescriptor     | 하나의 작업이 다른 작업에 의해 생성된 데이터를 "구독"하도록 허용하는 업스트림 작업 출력을 나타냅니다.        |
|AutomationVariableValueDescriptor     | 이름으로 Automation 변수 자산을 나타냅니다.         |
|AutomationCredentialValueDescriptor     | 이름으로 Automation 인증서 자산을 나타냅니다.        |
|AutomationConnectionValueDescriptor     | 이름으로 Automation 연결 자산을 나타냅니다.        |
|PowerShellExpressionValueDescriptor     | 작업을 호출하기 직전에 평가될 자유 양식 PowerShell 식을 지정합니다.  <br/>강력한 도구이지만 과도하게 사용하지 마십시오. UI는 이 식을 텍스트로 표시합니다. 실행 엔진은 제공된 블록을 검은색 상자로 처리하고 기본 구문 검사를 제외하고 해당 콘텐츠를 분석하는 시도를 하지 않습니다. 가능한 경우 보다 구체적인 값 설명자를 사용합니다.      |

> [!NOTE]
> 제공된 클래스에서 사용자 고유의 값 설명자도 파생하지 마십시오. Azure Automation은 사용자 지정 값 설명자 형식이 있는 Runbook을 사용할 수 없습니다.

작업을 연결하는 링크를 인스턴스화하고 Runbook에 추가합니다.

```csharp
runbook.AddLink(new Link(activityA, activityB, LinkType.Sequence));

runbook.AddLink(
 new Link(activityB, activityC, LinkType.Pipeline) {
  Condition = string.Format("$ActivityOutput['{0}'] -gt 0", activityB.Name)
 });
```

## <a name="save-the-runbook-to-a-file"></a>파일에 Runbook을 저장합니다.

`Orchestrator.GraphRunbook.Model.Serialization.RunbookSerializer`를 사용하여 문자열에 Runbook을 직렬화합니다.

```csharp
var serialized = RunbookSerializer.Serialize(runbook);
```

이 문자열을 **.graphrunbook** 확장이 있는 파일에 저장할 수 있습니다. 해당 Runbook을 Azure 자동화로 가져올 수 있습니다.
직렬화된 형식은 이후 버전에서 `Orchestrator.GraphRunbook.Model.dll`변경될 수 있습니다. 이전 버전과의 호환성을 약속 드립니다. `Orchestrator.GraphRunbook.Model.dll`의 이전 버전으로 직렬화된 모든 Runbook은 모든 새 버전으로 역직렬화될 수 있습니다. 이후 버전과의 호환성은 보장되지 않습니다. 최신 버전으로 직렬화된 Runbook을 이전 버전으로 역직렬화할 수 없습니다.

## <a name="next-steps"></a>다음 단계

Azure 자동화의 그래픽 런북에 대한 자세한 내용은 [그래픽 작성 소개를](automation-graphical-authoring-intro.md)참조하십시오.