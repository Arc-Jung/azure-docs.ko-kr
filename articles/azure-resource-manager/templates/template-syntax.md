---
title: 템플릿 구조 및 구문
description: 선언적 JSON 구문을 사용 하 여 Azure Resource Manager 템플릿 (ARM 템플릿)의 구조 및 속성을 설명 합니다.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: ce36d725b3844fcd4c8d43a9f044423611d44fbd
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96497880"
---
# <a name="understand-the-structure-and-syntax-of-arm-templates"></a>ARM 템플릿의 구조 및 구문 이해

이 문서에서는 Azure Resource Manager 템플릿 (ARM 템플릿)의 구조에 대해 설명 합니다. 여기서는 템플릿의 다른 섹션 및 해당 섹션에서 사용할 수 있는 속성을 보여 줍니다.

이 문서는 ARM 템플릿에 대해 잘 알고 있는 사용자를 위한 것입니다. 템플릿의 구조에 대 한 자세한 정보를 제공 합니다. 템플릿 만들기 프로세스를 안내하는 단계별 자습서는 [자습서: 첫 번째 ARM 템플릿 만들기 및 배포](template-tutorial-create-first-template.md)를 참조하세요.

## <a name="template-format"></a>템플릿 형식

가장 간단한 구조의 템플릿에 포함되는 요소는 다음과 같습니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| 요소 이름 | 필수 | 설명 |
|:--- |:--- |:--- |
| $schema |예 |템플릿 언어의 버전을 설명하는 JSON 스키마 파일의 위치입니다. 사용할 버전 번호는 배포 범위 및 JSON 편집기에 따라 다릅니다.<br><br>[Azure Resource Manager 도구 확장과 함께 VS Code](quickstart-create-templates-use-visual-studio-code.md)를 사용 하는 경우 리소스 그룹 배포에 대 한 최신 버전을 사용 합니다.<br>`https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#`<br><br>다른 편집기 (Visual Studio 포함)가이 스키마를 처리 하지 못할 수 있습니다. 이러한 편집기의 경우 다음을 사용 합니다.<br>`https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>구독 배포의 경우 다음을 사용 합니다.<br>`https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`<br><br>관리 그룹 배포의 경우 다음을 사용 합니다.<br>`https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#`<br><br>테 넌 트 배포의 경우 다음을 사용 합니다.<br>`https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#` |
| contentVersion |예 |템플릿의 버전입니다(예: 1.0.0.0). 이 요소에 값을 제공할 수 있습니다. 이 값을 사용하여 템플릿에서 중요한 변경 내용을 문서화할 수 있습니다. 템플릿을 사용하여 리소스를 배포할 때 이 값을 사용하면 정확한 템플릿이 사용되도록 할 수 있습니다. |
| apiProfile |아니요 | 리소스 종류에 대 한 API 버전 컬렉션으로 사용 되는 API 버전입니다. 템플릿의 각 리소스에 대해 API 버전을 지정 하지 않으려면이 값을 사용 합니다. API 프로필 버전을 지정 하 고 리소스 종류에 대 한 API 버전을 지정 하지 않는 경우 리소스 관리자는 프로필에 정의 된 해당 리소스 유형에 대 한 API 버전을 사용 합니다.<br><br>API profile 속성은 Azure Stack, 글로벌 Azure 등의 다양 한 환경에 템플릿을 배포 하는 경우에 특히 유용 합니다. API 프로필 버전을 사용 하 여 템플릿에서 두 환경 모두에서 지원 되는 버전을 자동으로 사용 하는지 확인 합니다. 프로필에 정의 된 현재 API 프로필 버전 및 리소스 API 버전 목록은 [API 프로필](https://github.com/Azure/azure-rest-api-specs/tree/master/profile)을 참조 하세요.<br><br>자세한 내용은 [API 프로필을 사용 하 여 버전 추적](templates-cloud-consistency.md#track-versions-using-api-profiles)을 참조 하세요. |
| [parameters](#parameters) |아니요 |배포를 실행하여 리소스 배포를 사용자 지정할 때 제공되는 값입니다. |
| [variables](#variables) |아니요 |템플릿에서 템플릿 언어 식을 단순화하는 JSON 조각으로 사용되는 값입니다. |
| [역함수](#functions) |아니요 |템플릿 내에서 사용할 수 있는 사용자 정의 함수입니다. |
| [인사](#resources) |예 |리소스 그룹 또는 구독에 배포되거나 업데이트되는 리소스 종류입니다. |
| [outputs](#outputs) |아니요 |배포 후 반환되는 값입니다. |

각 요소에는 사용자가 설정할 수 있는 속성이 있습니다. 이 기사에서는 템플릿의 섹션에 대해 자세히 설명합니다.

## <a name="data-types"></a>데이터 형식

ARM 템플릿 내에서 다음 데이터 형식을 사용할 수 있습니다.

* 문자열
* securestring
* int
* bool
* object
* secureObject
* array

다음 템플릿에서는 데이터 형식에 대 한 형식을 보여 줍니다. 각 형식의 기본값은 올바른 형식입니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "stringParameter": {
      "type": "string",
      "defaultValue": "option 1"
    },
    "intParameter": {
      "type": "int",
      "defaultValue": 1
    },
    "boolParameter": {
        "type": "bool",
        "defaultValue": true
    },
    "objectParameter": {
      "type": "object",
      "defaultValue": {
        "one": "a",
        "two": "b"
      }
    },
    "arrayParameter": {
      "type": "array",
      "defaultValue": [ 1, 2, 3 ]
    }
  },
  "resources": [],
  "outputs": {}
}
```

보안 문자열은 문자열과 동일한 형식을 사용 하 고 보안 개체는 개체와 동일한 형식을 사용 합니다. 매개 변수를 보안 문자열 또는 보안 개체에 설정 하면 매개 변수 값이 배포 기록에 저장 되지 않고 기록 되지 않습니다. 그러나 보안 값을 예상 하지 않는 속성에 해당 보안 값을 설정 하면 값이 보호 되지 않습니다. 예를 들어 보안 문자열을 태그로 설정 하면 해당 값이 일반 텍스트로 저장 됩니다. 암호 및 암호에 보안 문자열을 사용 합니다.

인라인 매개 변수로 전달 된 정수의 경우 배포에 사용 하는 SDK 또는 명령줄 도구를 사용 하 여 값 범위를 제한할 수 있습니다. 예를 들어 PowerShell을 사용 하 여 템플릿을 배포 하는 경우 정수 형식의 범위는-2147483648 ~ 2147483647 일 수 있습니다. 이러한 제한을 방지 하려면 [매개 변수 파일](parameter-files.md)에 큰 정수 값을 지정 합니다. 리소스 형식은 정수 속성에 대해 고유한 제한을 적용 합니다.

템플릿에서 부울 및 정수 값을 지정 하는 경우 값을 따옴표로 묶지 마세요. 큰따옴표를 사용 하 여 문자열 값을 시작 하 고 끝에 표시 합니다.

개체는 왼쪽 중괄호로 시작 하 고 오른쪽 중괄호로 끝납니다. 배열은 왼쪽 대괄호를 사용 하 여 시작 하 고 오른쪽 괄호를 사용 하 여 끝납니다.

## <a name="parameters"></a>매개 변수

템플릿의 parameters 섹션에서 리소스를 배포할 때 입력할 수 있는 값을 지정합니다. 템플릿에서 매개 변수는 256개로 제한됩니다. 여러 속성을 포함 하는 개체를 사용 하 여 매개 변수 수를 줄일 수 있습니다.

매개 변수에 사용할 수 있는 속성은 다음과 같습니다.

```json
"parameters": {
  "<parameter-name>" : {
    "type" : "<type-of-parameter-value>",
    "defaultValue": "<default-value-of-parameter>",
    "allowedValues": [ "<array-of-allowed-values>" ],
    "minValue": <minimum-value-for-int>,
    "maxValue": <maximum-value-for-int>,
    "minLength": <minimum-length-for-string-or-array>,
    "maxLength": <maximum-length-for-string-or-array-parameters>,
    "metadata": {
      "description": "<description-of-the parameter>"
    }
  }
}
```

| 요소 이름 | 필수 | 설명 |
|:--- |:--- |:--- |
| 매개 변수-이름 |예 |매개 변수의 이름입니다. 유효한 JavaScript 식별자여야 합니다. |
| 형식 |예 |매개 변수 값의 유형입니다. 허용되는 유형 및 값은 **string**, **securestring**, **int**, **bool**, **object**, **secureObject** 및 **array** 입니다. [데이터 형식](#data-types)을 참조 하세요. |
| defaultValue |아니요 |매개 변수 값을 제공하지 않는 경우 매개 변수의 기본값입니다. |
| allowedValues |아니요 |올바른 값을 제공하도록 매개 변수에 대해 허용되는 값의 배열입니다. |
| minValue |아니요 |Int 형식 매개 변수의 최소값이며, 이 값이 포함됩니다. |
| maxValue |아니요 |Int 형식 매개 변수의 최대값이며, 이 값이 포함됩니다. |
| minLength |아니요 |string, securestring 및 array 형식 매개 변수의 최소 길이이며, 이 값이 포함됩니다. |
| maxLength |아니요 |string, securestring 및 array 형식 매개 변수의 최대 길이이며, 이 값이 포함됩니다. |
| description |아니요 |포털에서 사용자에게 표시되는 매개 변수의 설명입니다. 자세한 내용은 [템플릿의 주석](#comments)을 참조하세요. |

매개 변수를 사용 하는 방법에 대 한 예제는 [ARM 템플릿의 매개 변수](template-parameters.md)를 참조 하세요.

## <a name="variables"></a>변수

변수 섹션에서 템플릿을 통해 사용할 수 있는 값을 생성합니다. 변수를 정의할 필요는 없지만 종종 변수를 통해 복잡한 식을 줄이면 템플릿이 단순화됩니다. 각 변수의 형식은 [데이터 형식](#data-types)중 하 나와 일치 합니다.

다음 예에서는 변수를 정의 하는 데 사용할 수 있는 옵션을 보여 줍니다.

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": {
    <variable-complex-type-value>
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

를 사용 하 여 `copy` 변수에 대 한 여러 값을 만드는 방법에 대 한 자세한 내용은 [변수 반복](copy-variables.md)을 참조 하세요.

변수를 사용 하는 방법에 대 한 예제는 [ARM 템플릿의 변수](template-variables.md)를 참조 하세요.

## <a name="functions"></a>함수

템플릿 내에서 함수를 직접 만들 수 있습니다. 이러한 함수는 템플릿에서 사용할 수 있습니다. 일반적으로 템플릿 전체에서 반복 하지 않으려는 복잡 한 식을 정의 합니다. 템플릿에서 지원되는 식 및 [함수](template-functions.md)에서 사용자 정의 함수를 만듭니다.

사용자 함수를 정의할 때는 다음과 같은 몇 가지 제한 사항이 있습니다.

* 함수는 변수에 액세스할 수 없습니다.
* 함수는 함수에 정의된 매개 변수만 사용할 수 있습니다. 사용자 정의 함수 내에서 [parameters 함수](template-functions-deployment.md#parameters) 를 사용 하면 해당 함수에 대 한 매개 변수로 제한 됩니다.
* 함수는 다른 사용자 정의 함수를 호출할 수 없습니다.
* 함수는 [참조 함수](template-functions-resource.md#reference)를 사용할 수 없습니다.
* 함수의 매개 변수는 기본값을 가질 수 없습니다.

```json
"functions": [
  {
    "namespace": "<namespace-for-functions>",
    "members": {
      "<function-name>": {
        "parameters": [
          {
            "name": "<parameter-name>",
            "type": "<type-of-parameter-value>"
          }
        ],
        "output": {
          "type": "<type-of-output-value>",
          "value": "<function-return-value>"
        }
      }
    }
  }
],
```

| 요소 이름 | 필수 | 설명 |
|:--- |:--- |:--- |
| namespace |예 |사용자 지정 함수에 대 한 네임 스페이스입니다. 템플릿 함수와의 이름 충돌을 방지 하는 데 사용 합니다. |
| 함수 이름 |예 |사용자 지정 함수의 이름입니다. 함수를 호출할 때 함수 이름을 네임 스페이스와 결합 합니다. 예를 들어 contoso 네임 스페이스에서 uniqueName 이라는 함수를 호출 하려면를 사용 `"[contoso.uniqueName()]"` 합니다. |
| 매개 변수-이름 |아니요 |사용자 지정 함수 내에서 사용 되는 매개 변수의 이름입니다. |
| 매개 변수-값 |아니요 |매개 변수 값의 유형입니다. 허용되는 유형 및 값은 **string**, **securestring**, **int**, **bool**, **object**, **secureObject** 및 **array** 입니다. |
| 출력 형식 |예 |출력 값의 유형입니다. 출력 값은 함수 입력 매개 변수와 동일한 형식을 지원 합니다. |
| 출력-값 |예 |함수에서 계산 되 고 반환 되는 템플릿 언어 식입니다. |

사용자 지정 함수를 사용 하는 방법에 대 한 예제는 [ARM 템플릿의 사용자 정의 함수](template-user-defined-functions.md)를 참조 하세요.

## <a name="resources"></a>리소스

리소스 섹션에서 배포되거나 업데이트되는 리소스를 정의합니다.

다음과 같은 구조를 사용하여 리소스를 정의합니다.

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "apiVersion": "<api-version-of-resource>",
      "name": "<name-of-the-resource>",
      "comments": "<your-reference-notes>",
      "location": "<location-of-resource>",
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| 요소 이름 | 필수 | 설명 |
|:--- |:--- |:--- |
| condition(조건) | 아니요 | 리소스가 이 배포 중 프로비전되는지 여부를 나타내는 부울 값입니다. `true`인 경우 리소스는 배포하는 동안 만들어집니다. `false`인 경우 리소스는 이 배포에 대해 건너뛰어집니다. [조건](conditional-resource-deployment.md)을 참조 하세요. |
| 형식 |예 |리소스 유형입니다. 이 값은 리소스 공급자의 네임 스페이스와 리소스 유형 (예: **Microsoft. Storage/storageAccounts**)의 조합입니다. 사용 가능한 값을 확인 하려면 [템플릿 참조](/azure/templates/)를 참조 하세요. 자식 리소스의 경우 형식의 형식은 부모 리소스 내에 중첩 되어 있는지, 부모 리소스 외부에 정의 되는지에 따라 달라 집니다. [자식 리소스에 대한 이름 및 형식 설정](child-resource-name-type.md)을 참조하세요. |
| apiVersion |예 |리소스를 만들 때 사용하는 REST API의 버전입니다. 새 템플릿을 만들 때이 값을 배포 중인 리소스의 최신 버전으로 설정 합니다. 템플릿이 필요에 따라 작동 하는 동안 동일한 API 버전을 계속 사용 합니다. 동일한 API 버전을 계속 사용 하면 새 API 버전이 템플릿의 작동 방식을 변경 하는 위험을 최소화할 수 있습니다. 최신 버전에 도입 된 새 기능을 사용 하려는 경우에만 API 버전을 업데이트 하는 것이 좋습니다. 사용 가능한 값을 확인 하려면 [템플릿 참조](/azure/templates/)를 참조 하세요. |
| name |예 |리소스의 이름입니다. 이 이름은 RFC3986에 정의된 URI 구성 요소 제한을 따라야 합니다. 외부 파티에 리소스 이름을 노출 하는 Azure 서비스는 이름 유효성을 검사 하 여 다른 id를 스푸핑 하려고 하지 않았는지 확인 합니다. 자식 리소스의 경우 이름의 형식은 부모 리소스 내에 중첩 되어 있는지, 부모 리소스 외부에 정의 되는지에 따라 달라 집니다. [자식 리소스에 대한 이름 및 형식 설정](child-resource-name-type.md)을 참조하세요. |
| comments |아니요 |템플릿에서 리소스를 문서화하는 내용에 대한 참고입니다. 자세한 내용은 [템플릿의 주석](template-syntax.md#comments)을 참조하세요. |
| 위치 |상황에 따라 다름 |제공된 리소스의 지역적 위치를 지원합니다. 사용 가능한 위치 중 하나를 선택할 수 있지만 대개는 사용자에게 가까운 하나를 선택하는 것이 좋습니다. 일반적으로 동일한 지역에서 서로 상호 작용하도록 리소스를 배치하는 것도 좋습니다. 대부분의 리소스 종류에는 위치가 필요하지만 일부 종류(예: 역할 할당)에는 위치가 필요하지 않습니다. [리소스 위치 설정](resource-location.md)을 참조 하세요. |
| dependsOn |아니요 |이 리소스를 배포하기 전에 배포해야 하는 리소스입니다. Resource Manager는 리소스 간의 종속성을 평가한 후 올바른 순서에 따라 리소스를 배포합니다. 리소스는 서로 종속되지 않을 경우, 병렬로 배포됩니다. 이 값은 리소스 이름 또는 리소스 고유 식별자의 쉼표로 구분된 목록입니다. 이 템플릿에 배포된 리소스만 나열합니다. 이 템플릿에 정의되지 않은 리소스는 이미 존재해야 합니다. 불필요한 종속성은 배포 속도를 느리게 만들고 순환 종속성을 만들기 때문에 추가하지 않습니다. 종속성 설정에 대 한 지침은 [ARM 템플릿에서 리소스를 배포 하는 순서 정의](define-resource-dependency.md)를 참조 하세요. |
| tags |예 |리소스와 연결된 태그입니다. 태그를 적용하여 구독에서 리소스를 논리적으로 구성합니다. |
| sku | 예 | 일부 리소스에서는 SKU를 정의하는 값을 허용합니다. 예를 들어 스토리지 계정에 대한 중복 유형을 지정할 수 있습니다. |
| kind | 아니요 | 일부 리소스에서는 배포하는 리소스 종류를 정의하는 값을 허용합니다. 예를 들어 만들 Cosmos DB 종류를 지정할 수 있습니다. |
| copy |아니요 |인스턴스가 둘 이상 필요한 경우 만드는 리소스의 수입니다. 기본 모드는 병렬입니다. 모든 리소스를 동시에 배포하지 않으려면 직렬 모드를 지정합니다. 자세한 내용은 [Azure Resource Manager에서 리소스의 여러 인스턴스 만들기](copy-resources.md)를 참조하세요. |
| 계획 | 아니요 | 일부 리소스에서는 배포할 계획을 정의하는 값을 허용합니다. 예를 들어 가상 머신에 대한 마켓플레이스 이미지를 지정할 수 있습니다. |
| properties |아니요 |리소스별 구성 설정입니다. 속성의 값은 리소스를 만들기 위해 REST API 작업(PUT 메서드)에 대한 요청 본문에 제공하는 값과 동일합니다. 복사 배열을 지정하여 속성의 여러 인스턴스를 만들 수도 있습니다. 사용 가능한 값을 확인 하려면 [템플릿 참조](/azure/templates/)를 참조 하세요. |
| 리소스 |예 |정의 중인 리소스에 종속되는 하위 리소스입니다. 부모 리소스의 스키마에서 허용되는 리소스 유형만 제공합니다. 부모 리소스에 대한 종속성은 암시되지 않습니다. 해당 종속성을 명시적으로 정의해야 합니다. [자식 리소스에 대한 이름 및 형식 설정](child-resource-name-type.md)을 참조하세요. |

## <a name="outputs"></a>출력

Outputs 섹션에서, 배포에서 반환되는 값을 지정합니다. 일반적으로 배포 된 리소스에서 값을 반환 합니다.

다음 예제에서는 출력 정의의 구조를 보여줍니다.

```json
"outputs": {
  "<output-name>": {
    "condition": "<boolean-value-whether-to-output-value>",
    "type": "<type-of-output-value>",
    "value": "<output-value-expression>",
    "copy": {
      "count": <number-of-iterations>,
      "input": <values-for-the-variable>
    }
  }
}
```

| 요소 이름 | 필수 | 설명 |
|:--- |:--- |:--- |
| 출력-이름 |예 |출력 값의 이름입니다. 유효한 JavaScript 식별자여야 합니다. |
| condition(조건) |아니요 | 이 출력 값의 반환 여부를 나타내는 부울 값입니다. `true`이면 해당 값이 배포의 출력에 포함됩니다. `false`이면 이 배포에 대한 출력 값을 건너뜁니다. 지정하지 않으면 기본값은 `true`입니다. |
| 형식 |예 |출력 값의 유형입니다. 출력 값은 템플릿 입력 매개 변수와 동일한 유형을 지원합니다. 출력 유형에 대해 **securestring** 을 지정 하는 경우 값은 배포 기록에 표시 되지 않으며 다른 템플릿에서 검색할 수 없습니다. 둘 이상의 템플릿에서 비밀 값을 사용 하려면 Key Vault에 비밀을 저장 하 고 매개 변수 파일에서 비밀을 참조 합니다. 자세한 내용은 [Azure Key Vault를 사용하여 배포 중에 보안 매개 변수 값 전달](key-vault-parameter.md)을 참조하세요. |
| 값 |아니요 |출력 값으로 계산되어 반환되는 템플릿 언어 식입니다. **값** 또는 **복사** 를 지정 합니다. |
| copy |아니요 | 출력에 대해 둘 이상의 값을 반환 하는 데 사용 됩니다. **값** 또는 **복사** 를 지정 합니다. 자세한 내용은 [ARM 템플릿의 출력 반복](copy-outputs.md)을 참조 하세요. |

출력을 사용 하는 방법에 대 한 예제는 [ARM 템플릿의 출력](template-outputs.md)을 참조 하세요.

<a id="comments"></a>

## <a name="comments-and-metadata"></a>주석 및 메타데이터

템플릿에 주석 및 메타데이터를 추가하는 몇 가지 옵션이 있습니다.

### <a name="comments"></a>의견

인라인 주석의 경우 또는 중 하나를 사용할 `//` 수 `/* ... */` 있지만이 구문은 모든 도구에서 작동 하지 않습니다. 이 주석 스타일을 추가하는 경우 사용하는 도구가 반드시 인라인 JSON 주석을 지원해야 합니다.

> [!NOTE]
> 2.3.0 또는 이전 버전의 Azure CLI을 사용 하 여 주석을 사용 하 여 템플릿을 배포 하려면 스위치를 사용 해야 합니다 `--handle-extended-json-format` .

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "dependsOn": [ /* storage account and network interface must be deployed first */
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Visual Studio Code에서 [Azure Resource Manager 도구 확장](quickstart-create-templates-use-visual-studio-code.md) 은 ARM 템플릿을 자동으로 검색 하 고 언어 모드를 변경할 수 있습니다. VS Code의 오른쪽 아래 모서리에 **Azure Resource Manager 템플릿이** 표시 되는 경우 인라인 주석을 사용할 수 있습니다. 그러면 인라인 주석이 더 이상 유효하지 않음으로 표시되지 않습니다.

![Visual Studio Code Azure Resource Manager 템플릿 모드](./media/template-syntax/resource-manager-template-editor-mode.png)

### <a name="metadata"></a>메타데이터

`metadata` 개체는 템플릿의 거의 모든 위치에 추가할 수 있습니다. Azure Resource Manager는 해당 개체를 무시하지만 JSON 편집기가 해당 속성이 유효하지 않음을 경고할 수 있습니다. 개체 내에 필요한 속성을 정의합니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

**매개 변수** 에 대해 `description` 속성과 함께 `metadata` 개체를 추가합니다.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

포털을 통해 템플릿을 배포하는 경우 설명에 제공하는 텍스트가 자동으로 해당 매개 변수에 대한 팁으로 사용됩니다.

![매개 변수 팁 표시](./media/template-syntax/show-parameter-tip.png)

**리소스** 에 대해 `comments` 요소 또는 메타데이터 개체를 추가합니다. 다음 예제에서는 주석 요소와 메타데이터 개체를 모두 보여 줍니다.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2018-07-01",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "comments": "Storage account used to store VM disks",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

**outputs** 의 경우 출력 값에 메타데이터 개체를 추가합니다.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

사용자 정의 함수에 메타데이터 개체를 추가할 수 없습니다.

## <a name="multi-line-strings"></a>여러 줄 문자열

문자열을 여러 줄로 나눌 수 있습니다. 예를 들어, 다음 JSON 예제에서 위치 속성 및 주석 중 하나를 참조 하세요.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

2.3.0 또는 이전 버전의 Azure CLI을 사용 하 여 여러 줄 문자열을 사용 하 여 템플릿을 배포 하려면 스위치를 사용 해야 합니다 `--handle-extended-json-format` .

## <a name="next-steps"></a>다음 단계

* 다양한 유형의 솔루션에 대한 전체 템플릿을 보려면 [Azure 빠른 시작 템플릿](https://azure.microsoft.com/documentation/templates/)을 참조하세요.
* 템플릿 내에서 사용할 수 있는 함수에 대 한 자세한 내용은 [ARM 템플릿 함수](template-functions.md)를 참조 하세요.
* 배포 하는 동안 여러 템플릿을 결합 하려면 [Azure 리소스를 배포할 때 연결 된 템플릿 및 중첩 된 템플릿 사용](linked-templates.md)을 참조 하세요.
* 템플릿을 만드는 방법에 대 한 권장 사항은 [ARM 템플릿 모범 사례](template-best-practices.md)를 참조 하세요.
* 일반적인 질문에 대 한 대답은 [ARM 템플릿에 대 한](frequently-asked-questions.md)질문과 대답을 참조 하세요.
