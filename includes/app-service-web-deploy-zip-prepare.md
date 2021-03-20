---
title: 파일 포함
description: 파일 포함
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/14/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 035399924216434de85865102db8838ea3fa15a3
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "85570222"
---
## <a name="create-a-project-zip-file"></a>프로젝트 ZIP 파일 만들기

>[!NOTE]
> 파일을 ZIP 파일로 다운로드한 경우 먼저 파일을 추출합니다. 예를 들어 GitHub에서 ZIP 파일을 다운로드한 경우 해당 파일을 그대로 배포할 수 없습니다. GitHub에서는 App Service에서 작동하지 않는 추가적인 중첩된 디렉터리를 추가합니다. 
>

로컬 터미널 창에서 앱 프로젝트의 루트 디렉터리로 이동합니다. 

이 디렉터리는 _index.html_, _index.php_ 및 _app.js_ 와 같은 웹앱에 대한 입력 파일을 포함해야 합니다. 또한 _project.json_, _composer.json_, _package.json_, _bower.json_ 및 _requirements.txt_ 와 같은 패키지 관리 파일을 포함할 수 있습니다.

App Service 배포 자동화를 실행 하려면 모든 빌드 작업 (예:,,, 및)을 실행 하 `npm` `bower` `gulp` `composer` `pip` 고 앱을 실행 하는 데 필요한 모든 파일이 있는지 확인 합니다. 이 단계는 [패키지를 직접 실행](../articles/app-service/deploy-run-package.md)하려는 경우에 필요 합니다.

프로젝트의 모든 것에 대한 ZIP 아카이브를 만듭니다. 프로젝트의 경우 `dotnet` 이 폴더는 명령의 출력 폴더입니다 `dotnet publish` . 다음 명령은 터미널의 기본 도구를 사용합니다.

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

