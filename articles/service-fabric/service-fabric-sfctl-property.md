---
title: Azure Service Fabric CLI-sfctl 속성
description: Sfctl, Azure Service Fabric 명령줄 인터페이스에 대해 알아봅니다. 속성을 저장 하 고 쿼리 하는 명령 목록을 포함 합니다.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a9bd75e0b7f8bfceb50a71ca83b60ff1e7b45508
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905822"
---
# <a name="sfctl-property"></a>sfctl property
Service Fabric 이름 아래에 있는 저장소 및 쿼리 속성입니다.

## <a name="commands"></a>명령

|명령|Description|
| --- | --- |
| delete | 지정된 Service Fabric 속성을 삭제합니다. |
| get | 지정된 Service Fabric 속성을 가져옵니다. |
| list | 지정된 이름에서 모든 Service Fabric 속성에 대한 정보를 가져옵니다. |
| put | Service Fabric 속성을 만들거나 업데이트합니다. |

## <a name="sfctl-property-delete"></a>sfctl property delete
지정된 Service Fabric 속성을 삭제합니다.

지정된 이름에서 지정된 Service Fabric 속성을 삭제합니다. 속성을 삭제하려면 먼저 만들어야 합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --name-id       [필수] | 'fabric\:' URI 체계가 없는 Service Fabric 이름입니다. |
| --property-name [필수] | 가져올 속성의 이름을 지정합니다. |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

## <a name="sfctl-property-get"></a>sfctl property get
지정된 Service Fabric 속성을 가져옵니다.

지정된 이름에서 지정된 Service Fabric 속성을 가져옵니다. 값 및 메타데이터 모두를 항상 반환합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --name-id       [필수] | 'fabric\:' URI 체계가 없는 Service Fabric 이름입니다. |
| --property-name [필수] | 가져올 속성의 이름을 지정합니다. |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

## <a name="sfctl-property-list"></a>sfctl property list
지정된 이름에서 모든 Service Fabric 속성에 대한 정보를 가져옵니다.

Service Fabric 이름에는 사용자 지정 정보를 저장하는 하나 이상의 명명된 속성이 있을 수 있습니다. 이 작업은 페이지가 지정된 목록에서 이러한 속성에 대한 정보를 가져옵니다. 정보는 각 속성에 대한 이름, 값 및 메타데이터를 포함합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --name-id [필수] | 'fabric\:' URI 체계가 없는 Service Fabric 이름입니다. |
| --continuation-token | 연속 토큰 매개 변수는 다음 결과 집합을 얻는 데 사용됩니다. 공백 값이 아닌 연속 토큰은 시스템의 결과가 단일 응답에 맞지 않는 경우 API의 응답에 포함됩니다. 이 값이 다음 API 호출에 전달되면 API는 다음 결과 집합을 반환합니다. 결과가 더 이상 없으면 연속 토큰에 값이 포함되지 않습니다. 이 매개 변수의 값은 URL 인코딩이 되지 말아야 합니다. |
| --include-values | 반환되는 속성의 값을 포함할지 여부를 지정할 수 있습니다. 메타데이터와 함께 값이 반환되어야 하는 경우 True이고, 속성 메타데이터만을 반환하는 경우 False입니다. |
| --timeout -t | 작업을 수행 하기 위한 서버 제한 시간 (초)입니다. 이 시간 제한은 요청 된 작업이 완료 될 때까지 클라이언트가 대기 하는 기간을 지정 합니다. 이 매개 변수의 기본값은 60 초입니다.  기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |

## <a name="sfctl-property-put"></a>sfctl property put
Service Fabric 속성을 만들거나 업데이트합니다.

지정된 이름에서 지정된 Service Fabric 속성을 만들거나 업데이트합니다.

### <a name="arguments"></a>인수

|인수|Description|
| --- | --- |
| --name-id       [필수] | 'fabric\:' URI 체계가 없는 Service Fabric 이름입니다. |
| --property-name [필수] | Service Fabric 속성의 이름입니다. |
| --value         [필수] | Service Fabric 속성 값을 설명합니다. JSON 문자열입니다. <br><br> Json 문자열에는 데이터의 ' 종류 '와 데이터의 ' 데이터 '로 입력 된 값의 두 필드가 있습니다. 'Kind' 값은 JSON 문자열에 나타나는 데 첫 번째 항목이어야 하며, 'Binary', 'Int64', 'Double', 'String' 또는 'Guid' 값일 수 있습니다. 값은 지정된 형식으로 직렬화될 수 있어야 합니다. 'Kind'와 'Data' 값은 문자열로 제공되어야 합니다. |
| --custom-id-type | 속성의 사용자 지정 형식 ID입니다. 이 속성을 사용하여 사용자는 속성 값의 형식을 태그 지정할 수 있습니다. |
| --timeout -t | 기본값\: 60. |

### <a name="global-arguments"></a>전역 인수

|인수|Description|
| --- | --- |
| --debug | 로깅의 자세한 정도를 늘려 모든 디버그 로그를 표시합니다. |
| --help -h | 이 도움말 메시지를 표시하고 종료합니다. |
| --output -o | 출력 형식입니다.  허용되는 값\: json, jsonc, table, tsv.  기본값\: json. |
| --query | JMESPath 쿼리 문자열입니다. 자세한 내용 및 예제는 http\://jmespath.org/를 참조하세요. |
| --verbose | 로깅의 자세한 정도를 늘립니다. 전체 디버그 로그를 표시하려면 --debug를 사용합니다. |


## <a name="next-steps"></a>다음 단계
- Service Fabric CLI [설정](service-fabric-cli.md)
- [샘플 스크립트](/azure/service-fabric/scripts/sfctl-upgrade-application)를 사용하여 Microsoft Azure Service Fabric CLI를 사용하는 방법에 대해 알아봅니다.