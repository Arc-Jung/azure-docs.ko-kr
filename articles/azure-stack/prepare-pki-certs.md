---
title: Azure 스택 통합 시스템 배포에 대 한 Azure 스택 공개 키 인프라 인증서 준비 | Microsoft Docs
description: Azure 스택 통합 시스템에 대 한 Azure 스택 PKI 인증서를 준비 하는 방법에 설명 합니다.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dadb443f8b7739e3a18c0d3beb558d8c42e9d19c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Azure 스택 PKI 인증서 배포 준비
인증서 파일 [선택한 해당 CA에서 가져온](azure-stack-get-pki-certs.md) 가져오고 Azure 스택 인증서 요구 사항 일치 하는 속성을 사용 하 여 내보낸 해야 합니다.


## <a name="prepare-certificates-for-deployment"></a>배포에 대 한 인증서를 준비 합니다.
다음이 단계를 사용 하 여 준비 하 고 Azure 스택 PKI 인증서의 유효성을 검사 합니다. 

1.  원래 인증서 버전을 복사할 [선택한 해당 CA에서 가져온](azure-stack-get-pki-certs.md) 배포 호스트에 있는 디렉터리에 있습니다. 
  > [!WARNING]
  > CA에서 직접 제공 하는 파일에서 어떤 방식으로든에서 이미, 내보낼 가져오거나 변경 하는 파일을 복사 하지 마십시오.

2.  로컬 컴퓨터 인증서 저장소에 인증서를 가져옵니다.

    a.  선택한 인증서를 마우스 오른쪽 단추로 클릭 **PFX 설치**합니다.

    나.  에 **인증서 가져오기 마법사**선택, **로컬 컴퓨터** 가져오기 위치도 합니다. **다음**을 선택합니다.

    ![로컬 컴퓨터 가져오기 위치](.\media\prepare-pki-certs\1.png)

    다.  선택 **다음** 에 **가져올 파일 선택** 페이지.

    d.  에 **개인 키 보호** 페이지에서 인증서 파일에 대 한 암호를 입력 한 후 사용 하도록 설정 된 **이 키를 내보낼 수 있도록 표시 합니다. 백업 또는 나중에 키를 전송할 수 있습니다** 옵션입니다. **다음**을 선택합니다.

    ![키를 내보낼 수 있도록 표시](.\media\prepare-pki-certs\2.png)

    e.  선택 **모든 인증서를 다음 저장소에 위치** 선택한 후 **엔터프라이즈 신뢰** 위치로 합니다. 클릭 **확인** 를 인증서 저장소 선택 대화 상자를 닫으려면 다음 **다음**합니다.

    ![인증서 저장소 구성](.\media\prepare-pki-certs\3.png)

  f.    클릭 **마침** 인증서 가져오기 마법사를 완료 합니다.

  g.    배포에 대해 제공 하는 모든 인증서에 대 한 프로세스를 반복 합니다.

3. Azure 스택 요구 사항이 있는 PFX 파일 형식으로 인증서를 내보냅니다.

  a.    인증서 관리자 MMC 콘솔을 열고 로컬 컴퓨터 인증서 저장소에 연결 합니다.

  나.    이동 하 여 **엔터프라이즈 신뢰** 디렉터리입니다.

  다.    위의 2 단계에서 가져온 인증서 중 하나를 선택 합니다.

  d.    작업 표시줄의 인증서 관리자 콘솔에서 선택 **동작** > **모든 작업** > **내보내기**합니다.

  e.    **다음**을 선택합니다.

  f.    선택 **예, 개인 키 내보내기**, 클릭 하 고 **다음**합니다.

  g.    파일 내보내기 형식 섹션에서 선택 **확장 된 속성 모두 내보내기** 클릭 하 고 **다음**합니다.

  h.    선택 **암호** 인증서에 대 한 암호를 지정 합니다. 배포 매개 변수로 사용 중 이므로이 암호를 기억해 둡니다. **다음**을 선택합니다.

  i.    파일 이름 및 pfx 파일을 내보낼 위치를 선택 합니다. **다음**을 선택합니다.

  j.    **마침**을 선택합니다.

  k.    모든 위의 2 단계에서 배포 하기 위해 가져온 인증서에 대 한이 프로세스를 반복 합니다.

## <a name="next-steps"></a>다음 단계
[PKI 인증서의 유효성을 검사합니다](validate-pki-certs.md)