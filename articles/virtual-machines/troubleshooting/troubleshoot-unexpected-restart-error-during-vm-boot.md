---
title: OS 시작-컴퓨터가 예기치 않게 다시 시작 되거나 예기치 않은 오류가 발생 했습니다.
description: 이 문서에서는 Windows를 설치 하는 동안 VM이 예기치 않게 다시 시작 되거나 오류가 발생 하는 문제를 해결 하는 단계를 제공 합니다.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 1d249a4e-71ba-475d-8461-31ff13e57811
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/22/2020
ms.author: v-mibufo
ms.openlocfilehash: cfeb040893ae2be5842959ed8458bd713bebe6ee
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512140"
---
# <a name="os-start-up--computer-restarted-unexpectedly-or-encountered-an-unexpected-error"></a>OS 시작-컴퓨터가 예기치 않게 다시 시작 되거나 예기치 않은 오류가 발생 했습니다.

이 문서에서는 Windows를 설치 하는 동안 VM (가상 컴퓨터)에 예기치 않은 다시 시작 또는 오류가 발생 하는 문제를 해결 하는 단계를 제공 합니다.

## <a name="symptom"></a>증상

[부팅 진단을](./boot-diagnostics.md) 사용 하 여 VM의 스크린샷을 볼 때 다음 오류와 함께 Windows 설치가 실패 하는 것을 볼 수 있습니다.

**컴퓨터가 예기치 않게 다시 시작 되었거나 예기치 않은 오류가 발생 했습니다. Windows 설치를 계속할 수 없습니다. Windows를 설치 하려면 "확인"을 클릭 하 여 컴퓨터를 다시 시작한 다음 설치를 다시 시작 합니다.**

![Windows 설치가 진행 되는 동안 오류가 발생 했습니다. 컴퓨터가 예기치 않게 다시 시작 되었거나 예기치 않은 오류가 발생 했습니다. Windows 설치를 계속할 수 없습니다. Windows를 설치 하려면 "확인"을 클릭 하 여 컴퓨터를 다시 시작한 다음 설치를 다시 시작 합니다.](./media/unexpected-restart-error-during-vm-boot/1.png)
 
![Windows 설치 설치 프로그램에서 서비스를 시작 하는 동안 오류 발생: 컴퓨터가 예기치 않게 다시 시작 되었거나 예기치 않은 오류가 발생 했습니다. Windows 설치를 계속할 수 없습니다. Windows를 설치 하려면 "확인"을 클릭 하 여 컴퓨터를 다시 시작한 다음 설치를 다시 시작 합니다.](./media/unexpected-restart-error-during-vm-boot/2.png)

## <a name="cause"></a>원인

컴퓨터가 [일반화 된 이미지](/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation)의 초기 부팅을 시도 하지만 사용자 지정 응답 파일 (Unattend.xml)이 처리 되는 동안 문제가 발생 했습니다. **사용자 지정 응답 파일은 Azure에서 지원 되지 않습니다**. 

응답 파일은 Windows Server 운영 체제를 설치 하는 동안 자동화 하려는 구성 설정에 대 한 정의 및 값 설정을 포함 하는 특수 XML 파일입니다. 구성 옵션에는 디스크를 분할 하는 방법, 설치할 Windows 이미지를 찾을 수 있는 위치, 적용할 제품 키, 실행할 기타 명령을 포함 하는 지침이 포함 됩니다.

또한 사용자 지정 응답 파일은 Azure에서 지원 되지 않습니다. 따라서 Azure에서 사용할 수 있도록 이미지를 준비 했지만 다음 명령과 유사한 플래그와 함께 **SYSPREP** 을 사용 하 여 사용자 지정 Unattend.xml 파일을 지정 했을 때 이러한 상황이 발생 합니다.

`sysprep /oobe /generalize /unattend:<your file’s name> /shutdown`

Azure에서 **시스템 준비 도구 GUI** 에서 **시스템 oobe (첫 실행 경험)** 옵션을 사용 하거나 `sysprep /oobe` Unattend.xml 파일 대신을 사용 합니다.

이 문제는 일반적으로 온-프레미스 VM과 함께 sysprep을 사용 하 여 일반화 된 VM을 Azure에 업로드 하는 동안 생성 됩니다. 이 경우 일반화 된 VM을 적절 하 게 업로드 하는 방법에도 관심이 있을 수 있습니다.

## <a name="solution"></a>해결 방법

### <a name="do-not-use-unattendxml"></a>Unattend.xml 사용 안 함

이 문제를 해결 하려면 [이미지 준비/캡처](../windows/upload-generalized-managed.md) 및 새로운 일반화 된 이미지 준비에 대 한 Azure 지침을 따르세요. Sysprep을 실행 하는 동안 **`/unattend:<your file’s name>` 플래그를 사용 하지 마십시오**. 대신 아래 플래그만 사용 합니다.

`sysprep /oobe /generalize /shutdown`

- OOBE (기본 제공 환경)는 Azure Vm에 대해 지원 되는 설정입니다.

아래에 표시 된 옵션을 선택 하 여 **시스템 준비 도구 GUI** 를 사용 하 여 위의 명령과 동일한 작업을 수행할 수도 있습니다.

- 기본 제공 환경 입력
- 일반화
- Shutdown
 
![시스템 준비 도구 창 (OOBE, 일반화 및 종료 옵션 선택).](./media/unexpected-restart-error-during-vm-boot/3.png)
