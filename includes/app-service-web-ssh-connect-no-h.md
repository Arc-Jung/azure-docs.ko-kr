---
title: 포함 파일
description: 포함 파일
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2020
ms.locfileid: "67182682"
---
컨테이너와 직접 SSH 세션을 열려면 앱을 실행해야 합니다.

브라우저에 다음 URL을 붙여넣고 \<app-name>을 앱 이름으로 바꿉니다.

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

아직 인증을 받지 못한 경우 연결하기 위해서는 Azure 구독에서 인증을 받아야 합니다. 인증되면 컨테이너 내부에서 명령을 실행할 수 있는 브라우저 내부 셸을 확인합니다.

![SSH 연결](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
