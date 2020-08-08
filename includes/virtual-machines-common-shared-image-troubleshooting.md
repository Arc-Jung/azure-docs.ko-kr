---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 06/15/2020
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 52569f3cec26432970606b31fe831bb6459839d6
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88011025"
---
공유 이미지 갤러리, 이미지 정의 및 이미지 버전에 대한 작업을 수행하는 동안 문제가 발생하는 경우 실패한 명령을 디버그 모드에서 다시 실행합니다. 디버그 모드는 `--debug` CLI 및 PowerShell로 스위치를 전달 하 여 활성화 됩니다 `-Debug` . 오류를 찾은 후에는 이 문서의 지침에 따라 오류를 해결합니다.


## <a name="unable-to-create-a-shared-image-gallery"></a>공유 이미지 갤러리를 만들 수 없음

가능한 원인:

*갤러리 이름을 올바르지 않습니다.*

갤러리 이름에 허용되는 문자는 대문자 또는 소문자, 숫자, 점 및 마침표입니다. 갤러리 이름에 대시를 사용할 수 없습니다. 갤러리 이름을 변경하고 다시 시도하세요. 

*갤러리 이름이 구독 내에서 고유하지 않습니다.*

다른 갤러리 이름을 선택하고 다시 시도하세요.


## <a name="unable-to-create-an-image-definition"></a>이미지 정의를 만들 수 없음 

가능한 원인:

*이미지 정의 이름이 올바르지 않습니다.*

이미지 정의에 허용 되는 문자는 대/소문자, 숫자, 점, 대시 및 마침표입니다. 이미지 정의 이름을 변경하고 다시 시도하세요.

*이미지 정의를 만들기 위한 필수 속성이 채워지지 않았습니다.*

이름, 게시자, 제품, sku, OS 종류 등의 속성은 필수입니다. 모든 속성이 전달되었는지 확인하세요.

이미지 정의의 **OSType**(Linux 또는 Windows)가 이미지 버전을 만드는 데 사용 하는 원본과 동일한 지 확인 합니다. 


## <a name="unable-to-create-an-image-version"></a>이미지 버전을 만들 수 없음 

가능한 원인:

*이미지 버전 이름이 올바르지 않습니다.*

이미지 버전에 허용되는 문자는 숫자 및 마침표입니다. 숫자는 32비트 정수 범위 내에 포함되어야 합니다. 형식: *MinorVersion*. 이미지 버전 이름을 변경하고 다시 시도하세요.

*이미지 버전이 만들어지는 원본 관리 이미지가 없습니다.* 

이미지 버전과 동일한 지역에 원본 이미지가 있는지 확인하세요.

*관리 이미지가 프로비전되지 않았습니다.*

원본 관리 이미지의 프로비전 상태가 **성공함**인지 확인하세요.

*대상 지역 목록에는 원본 지역이 포함 되지 않습니다.*

대상 지역 목록은 이미지 버전의 원본 지역을 포함해야 합니다. Azure가 이미지 버전을 복제할 대상 지역 목록에 원존 지역이 포함되었는지 확인하세요.

*모든 대상 지역에 복제하는 작업이 완료되지 않습니다.*

**--expand ReplicationStatus** 플래그를 사용하여 지정된 모든 대상 지역에 복제하는 작업이 완료되었는지 확인하세요. 아직 완료되지 않은 경우 작업이 완료될 때까지 최대 6시간을 기다려야 합니다. 작업이 실패하면 명령을 다시 실행하여 이미지 버전을 만들고 복제하세요. 이미지 버전이 복제되는 대상 지역의 수가 많은 경우 복제를 단계적으로 수행하는 방안을 고려해 보세요.

## <a name="unable-to-create-a-vm-or-a-scale-set"></a>VM 또는 확장 집합을 만들 수 없음 

가능한 원인:

*VM 또는 가상 머신 확장 집합을 만들려고 시도하는 사용자가 이미지 버전에 대한 읽기 권한을 갖고 있지 않습니다.*

구독 소유자에 게 문의 하 고 azure [역할 기반 액세스 제어 (AZURE RBAC)](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)를 통해 이미지 버전 또는 부모 리소스 (예: 공유 이미지 갤러리 또는 이미지 정의)에 대 한 읽기 액세스 권한을 부여 하도록 요청 합니다. 

*이미지 버전이 없습니다.*

VM 또는 가상 머신 확장 집합을 만들려는 지역이 이미지 버전의 대상 지역 목록에 포함되어 있는지 확인하세요. 해당 지역이 이미 대상 지역 목록에 포함되어 있으면 복제 작업이 완료되었는지 확인하세요. **-ReplicationStatus** 플래그를 사용하여 지정된 모든 대상 지역에 복제하는 작업이 완료되었는지 확인할 수 있습니다. 

*VM 또는 가상 머신 확장 집합을 만드는 시간이 깁니다.*

VM 또는 가상 머신 확장 집합을 만들려는 이미지 버전의 **OSType** 에 이미지 버전을 만드는 데 사용한 것과 동일한 원본 **OSType** 있는지 확인 합니다. 

## <a name="unable-to-share-resources"></a>리소스를 공유할 수 없음

Azure [역할 기반 액세스 제어 (AZURE RBAC)](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)를 사용 하 여 구독 간에 공유 이미지 갤러리, 이미지 정의 및 이미지 버전 리소스를 공유할 수 있습니다. 

## <a name="replication-is-slow"></a>복제 속도가 느림

**--expand ReplicationStatus** 플래그를 사용하여 지정된 모든 대상 지역에 복제하는 작업이 완료되었는지 확인하세요. 아직 완료되지 않은 경우 작업이 완료될 때까지 최대 6시간을 기다려야 합니다. 작업이 실패하면 명령을 다시 트리거하여 이미지 버전을 만들고 복제하세요. 이미지 버전이 복제되는 대상 지역의 수가 많은 경우 복제를 단계적으로 수행하는 방안을 고려해 보세요.

## <a name="azure-limits-and-quotas"></a>Azure 한도 및 할당량 

[Azure 한도 및 할당량](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)은 모든 공유 이미지 갤러리, 이미지 정의 및 이미지 버전 리소스에 적용됩니다. 구독의 한도 내에 있는지 확인하세요. 
