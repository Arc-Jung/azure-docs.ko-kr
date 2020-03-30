---
title: Azure 마켓플레이스에 대한 일반적인 SAS URL 문제 및 수정 사항
description: 공유 액세스 서명 URI 및 가능한 솔루션의 사용과 관련된 일반적인 문제를 나열합니다.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/27/2018
ms.author: dsindona
ms.openlocfilehash: 47702959474a352a8e13710ec850f789dee4d517
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278164"
---
# <a name="common-sas-url-issues-and-fixes"></a>일반적인 SAS URL 문제 및 수정

다음 표에는 공유 액세스 서명(업로드된 솔루션용 VHD를 식별하고 공유하는 데 사용됨)을 사용할 때 발생하는 몇 가지 일반적인 문제가 제안된 해결 방법과 함께 나와 있습니다.

| **문제** | **오류 메시지** | **수정** | 
| --------- | ------------------- | ------- | 
| &emsp;  *이미지 복사 중 오류* |  |  |
| "?"가 SAS URL에 없습니다 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 권장되는 도구를 사용하여 SAS URL을 업데이트합니다. |
| SAS URL에 없는 "st" 및 "se" 매개 변수 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | SAS URL을 적절한 **시작 날짜**와 **종료 날짜** 값으로 업데이트합니다. | 
| SAS URL에 없는 "sp=rl" | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | SAS URL을 `Read` 및 `List`로 설정된 권한으로 업데이트합니다. | 
| SAS URL의 VHD 이름에 공백이 있습니다. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 공백을 제거하도록 SAS URL을 업데이트합니다. |
| SAS URL 권한 부여 오류 | `Failure: Copying Images. Not able to download blob due to authorization error` | SAS URI 형식을 검토하고 수정합니다. 필요한 경우 다시 생성합니다. |
| "st" 및 "se" SAS URL 매개 변수에 전체 날짜/시간 사양이 없습니다. | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL 시작 날짜 및`st` 종료 `se` **날짜** 매개 변수(및 하위 문자열)는 `11-02-2017T00:00:00Z`와 같은 전체 datetime 형식이 필요합니다. **End Date** 약식 버전은 유효하지 않습니다. (Azure CLI의 일부 명령은 기본적으로 약식 값을 생성할 수 있습니다.) | 
|  |  |  |

자세한 내용은 [SAS(공유 액세스 서명) 사용](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)을 참조하세요.
