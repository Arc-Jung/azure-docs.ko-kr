---
title: 템플릿 함수-날짜
description: 날짜 작업에 Azure Resource Manager 템플릿 (ARM 템플릿)에서 사용할 수 있는 함수에 대해 설명 합니다.
ms.topic: conceptual
ms.date: 11/18/2020
ms.openlocfilehash: abff5b86ad1e10042596b11f613cdb594e307209
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104889929"
---
# <a name="date-functions-for-arm-templates"></a>ARM 템플릿에 대 한 날짜 함수

리소스 관리자는 Azure Resource Manager 템플릿 (ARM 템플릿)에서 날짜를 사용 하기 위한 다음 함수를 제공 합니다.

* [dateTimeAdd](#datetimeadd)
* [utcNow](#utcnow)

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="datetimeadd"></a>dateTimeAdd

`dateTimeAdd(base, duration, [format])`

기본 값에 기간을 추가 합니다. ISO 8601 형식이 필요 합니다.

### <a name="parameters"></a>매개 변수

| 매개 변수 | 필수 | Type | Description |
|:--- |:--- |:--- |:--- |
| base | 예 | 문자열 | 더하기의 시작 날짜/시간 값입니다. [ISO 8601 타임 스탬프 형식을](https://en.wikipedia.org/wiki/ISO_8601)사용 합니다. |
| duration | 예 | 문자열 | 밑에 더할 시간 값입니다. 음수 값일 수 있습니다. [ISO 8601 기간 형식을](https://en.wikipedia.org/wiki/ISO_8601#Durations)사용 합니다. |
| format | 예 | 문자열 | 날짜/시간 결과의 출력 형식입니다. 지정 하지 않으면 기준 값의 형식이 사용 됩니다. [표준 형식 문자열](/dotnet/standard/base-types/standard-date-and-time-format-strings) 또는 [사용자 지정 형식 문자열](/dotnet/standard/base-types/custom-date-and-time-format-strings)을 사용 합니다. |

### <a name="return-value"></a>반환 값

기준 값에 duration 값을 더하여 생성 되는 datetime 값입니다.

### <a name="examples"></a>예제

다음 예제 템플릿에서는 시간 값을 추가 하는 여러 가지 방법을 보여 줍니다.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "baseTime": {
      "type": "string",
      "defaultValue": "[utcNow('u')]"
    }
  },
  "variables": {
    "add3Years": "[dateTimeAdd(parameters('baseTime'), 'P3Y')]",
    "subtract9Days": "[dateTimeAdd(parameters('baseTime'), '-P9D')]",
    "add1Hour": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
  },
  "resources": [],
  "outputs": {
    "add3YearsOutput": {
      "value": "[variables('add3Years')]",
      "type": "string"
    },
    "subtract9DaysOutput": {
      "value": "[variables('subtract9Days')]",
      "type": "string"
    },
    "add1HourOutput": {
      "value": "[variables('add1Hour')]",
      "type": "string"
    },
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param baseTime string = utcNow('u')

var add3Years = dateTimeAdd(baseTime, 'P3Y')
var subtract9Days = dateTimeAdd(baseTime, '-P9D')
var add1Hour = dateTimeAdd(baseTime, 'PT1H')

output add3YearsOutput string = add3Years
output subtract9DaysOutput string = subtract9Days
output add1HourOutput string = add1Hour
```

---

위의 템플릿이 기본 시간을 사용 하 여 배포 된 경우 `2020-04-07 14:53:14Z` 출력은 다음과 같습니다.

| 이름 | Type | 값 |
| ---- | ---- | ----- |
| add3YearsOutput | String | 오후 4/7/2023 2:53:14 |
| subtract9DaysOutput | String | 오후 3/29/2020 2:53:14 |
| add1HourOutput | String | 오후 4/7/2020 3:53:14 |

다음 예제 템플릿에서는 자동화 일정의 시작 시간을 설정 하는 방법을 보여 줍니다.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "omsAutomationAccountName": {
      "type": "string",
      "defaultValue": "demoAutomation",
      "metadata": {
        "description": "Use an existing Automation account."
      }
    },
    "scheduleName": {
      "type": "string",
      "defaultValue": "demoSchedule1",
      "metadata": {
        "description": "Name of the new schedule."
      }
    },
    "baseTime": {
      "type": "string",
      "defaultValue": "[utcNow('u')]",
      "metadata": {
        "description": "Schedule will start one hour from this time."
      }
    }
  },
  "variables": {
    "startTime": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Automation/automationAccounts/schedules",
      "apiVersion": "2015-10-31",
      "name": "[concat(parameters('omsAutomationAccountName'), '/', parameters('scheduleName'))]",

      "properties": {
        "description": "Demo Scheduler",
        "startTime": "[variables('startTime')]",
        "interval": 1,
        "frequency": "Hour"
      }
    }
  ],
  "outputs": {
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param omsAutomationAccountName string = 'demoAutomation'
param scheduleName string = 'demSchedule1'
param baseTime string = utcNow('u')

var startTime = dateTimeAdd(baseTime, 'PT1H')

...

resource scheduler 'Microsoft.Automation/automationAccounts/schedules@2015-10-31' = {
  name: concat(omsAutomationAccountName, '/', scheduleName)
  properties: {
    description: 'Demo Scheduler'
    startTime: startTime
    interval: 1
    frequency: 'Hour'
  }
}
```

---

## <a name="utcnow"></a>utcNow

`utcNow(format)`

지정 된 형식의 현재 (UTC) datetime 값을 반환 합니다. 형식이 제공 되지 않으면 ISO 8601 ( `yyyyMMddTHHmmssZ` ) 형식이 사용 됩니다. **이 함수는 매개 변수의 기본값에만 사용할 수 있습니다.**

### <a name="parameters"></a>매개 변수

| 매개 변수 | 필수 | Type | Description |
|:--- |:--- |:--- |:--- |
| format |예 |문자열 |문자열로 변환할 URI 인코딩 값입니다. [표준 형식 문자열](/dotnet/standard/base-types/standard-date-and-time-format-strings) 또는 [사용자 지정 형식 문자열](/dotnet/standard/base-types/custom-date-and-time-format-strings)을 사용 합니다. |

### <a name="remarks"></a>설명

이 함수는 매개 변수의 기본값에 대해서만 식 내에서 사용할 수 있습니다. 템플릿의 다른 위치에서이 함수를 사용 하면 오류가 반환 됩니다. 함수는 호출 될 때마다 다른 값을 반환 하므로 템플릿의 다른 부분에서는 허용 되지 않습니다. 동일한 매개 변수를 사용 하 여 동일한 템플릿을 배포 하는 것은 안정적으로 동일한 결과를 생성 하지 않습니다.

이전에 성공한 배포에 [대해 오류 발생 시 롤백 옵션](rollback-on-error.md) 을 사용 하는 경우 이전 배포에 utcNow를 사용 하는 매개 변수가 포함 된 경우 매개 변수는 다시 평가 되지 않습니다. 대신 이전 배포의 매개 변수 값이 롤백 배포에서 자동으로 다시 사용 됩니다.

기본값에 대해 utcNow 함수를 사용 하는 템플릿을 다시 배포 해야 합니다. 다시 배포 하는 경우 매개 변수에 대 한 값을 제공 하지 않으면 함수가 재평가 됩니다. 새 리소스를 만드는 대신 기존 리소스를 업데이트 하려면 이전 배포에서 매개 변수 값을 전달 합니다.

### <a name="return-value"></a>반환 값

현재 UTC 날짜/시간 값입니다.

### <a name="examples"></a>예제

다음 예제 템플릿에서는 datetime 값에 대 한 다양 한 형식을 보여 줍니다.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "utcValue": {
      "type": "string",
      "defaultValue": "[utcNow()]"
    },
    "utcShortValue": {
      "type": "string",
      "defaultValue": "[utcNow('d')]"
    },
    "utcCustomValue": {
      "type": "string",
      "defaultValue": "[utcNow('M d')]"
    }
  },
  "resources": [
  ],
  "outputs": {
    "utcOutput": {
      "type": "string",
      "value": "[parameters('utcValue')]"
    },
    "utcShortOutput": {
      "type": "string",
      "value": "[parameters('utcShortValue')]"
    },
    "utcCustomOutput": {
      "type": "string",
      "value": "[parameters('utcCustomValue')]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param utcValue string = utcNow()
param utcShortValue string = utcNow('d')
param utcCustomValue string = utcNow('M d')

output utcOutput string = utcValue
output utcShortOutput string = utcShortValue
output utcCustomOutput string = utcCustomValue
```

---

이전 예제의 출력은 각 배포에 따라 다르지만 다음과 유사 합니다.

| 이름 | Type | 값 |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 2019/03/05 |
| utcCustomOutput | string | 3 5 |

다음 예제에서는 태그 값을 설정할 때 함수의 값을 사용 하는 방법을 보여 줍니다.

# <a name="json"></a>[JSON](#tab/json)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "utcShort": {
      "type": "string",
      "defaultValue": "[utcNow('d')]"
    },
    "rgName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2020-10-01",
      "name": "[parameters('rgName')]",
      "location": "westeurope",
      "tags": {
        "createdDate": "[parameters('utcShort')]"
      },
      "properties": {}
    }
  ],
  "outputs": {
    "utcShortOutput": {
      "type": "string",
      "value": "[parameters('utcShort')]"
    }
  }
}
```

# <a name="bicep"></a>[Bicep](#tab/bicep)

```bicep
param utcShort string = utcNow('d')
param rgName string

resource myRg 'Microsoft.Resources/resourceGroups@2020-10-01' = {
  name: rgName
  location: 'westeurope'
  tags: {
    createdDate: utcShort
  }
}

output utcShortOutput string = utcShort
```

---

## <a name="next-steps"></a>다음 단계

* ARM 템플릿의 섹션에 대 한 설명은 [arm 템플릿의 구조 및 구문 이해](template-syntax.md)를 참조 하세요.
