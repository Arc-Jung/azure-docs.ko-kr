---
title: Azure의 VM에 대한 부팅 진단 | Microsoft Docs
description: Azure의 가상 머신에 대한 두 가지 디버깅 기능 개요
services: virtual-machines
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: delhan
ms.openlocfilehash: fe2427d008b49daa6222ca981994f0dc2fbea355
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476589"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>부팅 진단을 사용하여 Azure의 가상 머신 문제를 해결하는 방법

가상 머신이 부팅할 수 없는 상태가 되는 이유에는 여러 가지가 있을 수 있습니다. Resource Manager 배포 모델을 사용하여 만든 가상 컴퓨터의 문제를 해결하려면 Azure 가상 컴퓨터에 대한 콘솔 출력 및 스크린샷 지원과 같은 디버깅 기능을 사용할 수 있습니다. 

Linux 가상 머신의 경우 포털에서 콘솔 로그의 출력을 볼 수 있습니다. Windows 및 Linux 가상 머신의 경우 Azure를 사용하면 하이퍼바이저에서 VM의 스크린샷을 볼 수 있습니다. 두 가지 기능이 모든 지역의 Azure 가상 머신에서 지원됩니다. 스크린샷과 출력은 저장소 계정에 표시되려면 최대 10분이 걸릴 수 있습니다.

**부트 진단** 옵션을 선택하여 로그 및 스크린샷을 볼 수 있습니다.

![리소스 관리자](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)

## <a name="common-boot-errors"></a>일반적인 부팅 오류

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [운영 체제를 찾지 못함](https://support.microsoft.com/help/4010142)
- [부팅 오류 또는 INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-virtual-machine-created-using-the-azure-portal"></a>Azure Portal을 사용하여 만든 가상 머신에서 진단 사용

다음 절차는 Resource Manager 배포 모델을 사용하여 만든 가상 머신을 위한 것입니다.

**관리** 탭의 **모니터링** 섹션에서 **부트 진단**이 켜져 있는지 확인합니다. **진단 스토리지 계정** 드롭다운 목록에서 진단 파일을 배치할 스토리지 계정을 선택합니다.
 
![VM 만들기](./media/virtual-machines-common-boot-diagnostics/enable-boot-diagnostics-vm.png)

> [!NOTE]
> 부팅 진단 기능은 프리미엄 스토리지 계정을 지원하지 않습니다. 부팅 진단에 프리미엄 스토리지 계정을 사용하는 경우 VM을 시작할 때 StorageAccountTypeNotSupported 오류가 발생할 수 있습니다.
>

### <a name="deploying-from-an-azure-resource-manager-template"></a>Azure Resource Manager 템플릿에서 배포

Azure Resource Manager 템플릿에서 배포하는 경우 가상 머신 리소스로 이동하고 진단 프로필 섹션을 추가합니다. API 버전 헤더를 "2015-06-15" 이상으로 설정합니다. 최신 버전은 "2018-10-01"입니다.

```json
{
  "apiVersion": "2018-10-01",
  "type": "Microsoft.Compute/virtualMachines",
  … 
```

진단 프로필을 사용하면 이러한 로그를 저장할 스토리지 계정을 선택할 수 있습니다.

```json
    "diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[concat('https://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
    }
    }
    }
}
```

템플릿을 사용하여 리소스 배포에 대한 자세한 내용은 [Azure 포털을 사용하여 Azure 리소스 관리자 템플릿 만들기 및 배포를](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)참조하세요.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>기존 가상 머신에서 부트 진단 사용 

기존 가상 머신에서 부팅 진단을 활성화하려면 다음 단계를 따릅니다.

1. [Azure Portal](https://portal.azure.com)에 로그인한 다음, 가상 머신을 선택합니다.
2. **지원 + 문제 해결** 섹션에서 **부트 진단**을 선택한 후 **설정** 탭을 선택합니다.
3. **부트 진단** 설정에서 상태를 **켜기**로 변경하고 **스토리지 계정** 드롭다운 목록에서 스토리지 계정을 선택합니다. 
4. 변경 내용을 저장합니다.

    ![기본 VM 업데이트](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

변경 내용을 적용하려면 가상 머신을 다시 시작해야 합니다.

### <a name="enable-boot-diagnostics-using-the-azure-cli"></a>Azure CLI를 사용하여 부트 진단 사용

Azure CLI를 사용하여 기존 Azure Virtual Machine에서 부트 진단을 사용하도록 설정할 수 있습니다. 자세한 내용은 [az vm boot-diagnostics](
https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest)를 참조하세요.
