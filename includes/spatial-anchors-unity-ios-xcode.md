---
author: ramonarguelles
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 08/14/2020
ms.author: rgarcia
ms.openlocfilehash: b93243a537fafce6d865ec207b12dc2654cafd20
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89536268"
---
**빌드**를 선택합니다. 열리는 대화 상자에서 Xcode 프로젝트를 내보낼 폴더를 선택합니다.

내보내기가 완료되면 내보낸 Xcode 프로젝트가 포함된 폴더가 표시됩니다.

> [!NOTE]
> 바꿀 것인지 아니면 추가할 것인지 물어보는 창이 표시되면 **추가**를 선택하는 것이 좋습니다. 추가가 더 빠르기 때문입니다. 장면의 자산을 변경하려는 경우에는 **바꾸기**를 선택해야 합니다. (예: 부모/자식 관계를 추가, 제거 또는 변경하거나 속성을 추가, 제거 또는 변경하는 경우) 소스 코드만 변경하려는 경우에는 **추가**를 선택하는 것으로 충분합니다.

## <a name="open-the-xcode-project"></a>Xcode 프로젝트 열기

이제 Xcode에서 `Unity-iPhone.xcodeproj`를 열 수 있습니다. Xcode를 시작하고 내보낸 `Unity-iPhone.xcodeproj` 프로젝트를 열거나 프로젝트를 내보낸 위치에서 다음 명령을 실행하여 Xcode에서 프로젝트를 시작할 수 있습니다.

```bash
open ./Unity-iPhone.xcodeproj
```

루트 **Unity-iPhone** 노드를 선택하여 프로젝트 설정을 살펴본 다음, **일반** 탭을 선택합니다.

**서명** 아래에서 **Automatically manage signing**(서명 자동 관리)를 사용하도록 설정되었는지 확인합니다. 설정되지 않았으면 설정한 다음, 나타나는 대화 상자에서 **Enable Automatic**(자동 사용)을 선택하여 빌드 설정을 다시 설정합니다.

**배포 정보**에서 **배포 대상**이 `11.0`으로 설정되어 있는지 확인합니다.

## <a name="deploy-the-app-to-your-ios-device"></a>iOS 디바이스에 앱 배포

iOS 디바이스를 Mac에 연결하고 **활성 스키마**를 iOS 디바이스로 설정합니다.

![디바이스 선택](./media/spatial-anchors-unity/select-device.png)

**빌드를 선택한 다음, 현재 스키마를 실행**합니다.

![배포 및 실행](./media/spatial-anchors-unity/deploy-run.png)
