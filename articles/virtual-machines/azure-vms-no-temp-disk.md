---
title: 로컬 임시 디스크가 없는 Azure VM 크기 FAQ
description: 이 문서에서는 로컬 임시 디스크를 포함 하지 않는 Microsoft Azure VM 크기에 대 한 FAQ (질문과 대답)를 제공 합니다.
author: brbell
ms.service: virtual-machines
ms.topic: conceptual
ms.author: brbell
ms.reviewer: mimckitt
ms.date: 06/15/2020
ms.openlocfilehash: 4dd078205989872179b0b2474974a29cf6b88dad
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102507843"
---
# <a name="azure-vm-sizes-with-no-local-temporary-disk"></a>로컬 임시 디스크가 없는 Azure VM 크기 
이 문서에서는 로컬 임시 디스크가 없는 Azure VM 크기에 대 한 FAQ (질문과 대답)를 제공 합니다 (예: 로컬 임시 디스크 없음). 이러한 VM 크기에 대 한 자세한 내용은 [Dv4 및 Dsv4에 대 한 사양 (범용 워크 로드)](dv4-dsv4-series.md) 또는 [Ev4 및 Esv4 시리즈 사양 (메모리 액세스에 최적화 된 작업)](ev4-esv4-series.md)을 참조 하세요.

## <a name="what-does-no-local-temp-disk-mean"></a>로컬 임시 디스크는 무엇을 의미 하나요? 
일반적으로 작은 로컬 디스크 (예: D: 드라이브)를 포함 하는 VM 크기 (예: Standard_D2s_v3, Standard_E48_v3)가 있습니다. 이제 이러한 새 VM 크기를 사용 하 여 작은 로컬 디스크가 더 이상 존재 하지 않습니다. 그러나 표준 HDD, 프리미엄 SSD 또는 울트라 SSD 연결할 수 있습니다.

## <a name="what-if-i-still-want-a-local-temp-disk"></a>여전히 로컬 임시 디스크를 원하는 경우 어떻게 하나요?
워크 로드에 로컬 임시 디스크가 필요한 경우 새로운 [Ddv4 및 Ddsv4](ddv4-ddsv4-series.md) , [Edv4 및 Edsv4](edv4-edsv4-series.md) VM 크기를 사용할 수도 있습니다. 이러한 크기는 이전 v3 크기와 비교 하 여 50% 더 큰 임시 디스크를 제공 합니다.

> [!NOTE]
> 로컬 임시 디스크는 영구적이 지 않습니다. 데이터를 영구적으로 유지 하려면 표준 HDD, 프리미엄 SSD 또는 울트라 SSD 옵션을 사용 하세요. 

## <a name="what-are-the-differences-between-these-new-vm-sizes-and-the-general-purpose-dv3dsv3-or-the-memory-optimized-ev3esv3-vm-sizes-that-i-am-used-to"></a>이러한 새 VM 크기와 범용 Dv3/Dsv3 또는 사용 되는 메모리 액세스에 최적화 된 Ev3/Esv3 VM 크기 간의 차이점은 무엇 인가요? 
| 로컬 임시 디스크를 사용 하는 v3 VM 제품군   | 로컬 임시 디스크를 사용 하는 새 v4 제품군 | 로컬 임시 디스크가 없는 새 v4 패밀리 |
|---|---|---|
| Dv3   | Ddv4 | Dv4 |
| Dsv3 | Ddsv4  | Dsv4 |
| Ev3   | Edv4  | Ev4 |
| Esv3 | Edsv4 |    Esv4 | 

## <a name="can-i-resize-a-vm-size-that-has-a-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>로컬 임시 디스크가 있는 vm 크기를 로컬 임시 디스크가 없는 VM 크기로 조정할 수 있나요?  
아니요. 크기 조정에 사용할 수 있는 조합은 다음과 같습니다. 

1. VM (로컬 임시 디스크 사용)-> VM (로컬 임시 디스크 포함); 하거나 
2. VM (로컬 임시 디스크 없음)-> VM (로컬 임시 디스크 없음) 

해결 방법에 관심이 있는 경우 다음 질문을 참조 하세요.

> [!NOTE]
> 이미지가 리소스 디스크에 종속 되어 있거나 페이지 파일 또는 스왑 파일이 로컬 임시 디스크에 있는 경우 디스크 없는 이미지는 작동 하지 않습니다. 대신 ' 디스크에 사용 ' 대안을 사용 하십시오. 

## <a name="how-do-i-migrate-from-a-vm-size-with-local-temp-disk-to-a-vm-size-with-no-local-temp-disk"></a>로컬 임시 디스크를 사용 하는 VM 크기에서 로컬 임시 디스크가 없는 VM 크기로 마이그레이션할 어떻게 할까요? 있나요?  
다음 단계를 수행 하 여 마이그레이션할 수 있습니다. 

1. 로컬 임시 디스크가 있는 가상 컴퓨터 (예: D: 드라이브)에 로컬 관리자로 연결 합니다.
2. [WINDOWS VM에서 d: 드라이브를 데이터 드라이브로 사용](./windows/change-drive-letter.md) 의 "일시적으로 pagefile.sys를 c 드라이브로 이동" 섹션의 지침에 따라 페이지 파일을 로컬 임시 디스크 (D: 드라이브)에서 C: 드라이브로 이동 합니다.

   > [!NOTE]
   > "C 드라이브로 일시적으로 pagefile.sys 이동" 섹션의 지침에 따라 로컬 임시 디스크 (D: 드라이브)에서 C: 드라이브로 페이지 파일을 이동 하려면 Windows VM의 데이터 드라이브로 D: 드라이브를 사용 합니다. **설명 된 단계와의 편차를 통해 오류 메시지가 표시 됩니다. "리소스 디스크에서 비 리소스 디스크 VM 크기로 변경 하 여 VM의 크기를 조정할 수 없습니다.**

3. [포털을 사용 하 여 스냅숏 만들기 또는 Azure CLI](./linux/snapshot-copy-managed-disk.md)에 설명 된 단계를 수행 하 여 VM의 스냅숏을 만듭니다. 
4. CLI를 사용 하 [여 스냅숏에서 가상 컴퓨터 만들기](./scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md)에 설명 된 단계에 따라 스냅숏을 사용 하 여 새 디스크 없는 VM (예: Dv4, Dsv4, Ev4, Esv4 series)을 만듭니다. 

## <a name="do-these-vm-sizes-support-both-linux-and-windows-operating-systems-os"></a>이러한 VM 크기는 Linux 및 Windows 운영 체제 (OS)를 모두 지원 하나요?
예.

## <a name="will-this-break-my-custom-scripts-custom-images-or-os-images-that-have-scratch-files-or-page-files-on-a-local-temp-disk"></a>이는 로컬 임시 디스크에 스크래치 파일이 나 페이지 파일이 있는 사용자 지정 스크립트, 사용자 지정 이미지 또는 OS 이미지를 중단 하나요?
사용자 지정 OS 이미지가 로컬 임시 디스크를 가리키는 경우이 디스크 없는 크기로 이미지가 제대로 작동 하지 않을 수 있습니다.

## <a name="have-questions-or-feedback"></a>질문이나 의견이 있으신가요?
[피드백 양식을]( https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRUMzdCQkw0OVVRTldFUUtXSTlLQVBPUkVHSy4u)작성 합니다. 

## <a name="next-steps"></a>다음 단계 
이 문서에서는 로컬 임시 디스크 없이 Azure Vm과 관련 하 여 가장 자주 묻는 질문에 대해 자세히 알아보았습니다. 이러한 VM 크기에 대 한 자세한 내용은 다음 문서를 참조 하세요.

- [Dv4 및 Dsv4 시리즈에 대 한 사양 (범용 워크 로드)](dv4-dsv4-series.md)
- [Ev4 및 Esv4 시리즈 사양 (메모리 액세스에 최적화 된 작업)](ev4-esv4-series.md)
