---
title: Linux 확장 집합 템플릿에서 게스트 메트릭을 사용 하 여 Azure 자동 크기 조정 사용
description: Linux Virtual Machine Scale Set 템플릿에서 게스트 메트릭을 사용하여 자동으로 크기를 조정하는 방법을 알아봅니다.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: autoscale
ms.date: 04/26/2019
ms.reviewer: avverma
ms.custom: avverma
ms.openlocfilehash: 0605780651e1a3c54ae53d13a3f99e1124fa76db
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100585000"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Linux 확장 집합 템플릿에서 게스트 메트릭을 사용한 자동 크기 조정

Azure에는 Vm 및 크기 집합에서 수집 되는 두 가지 광범위 한 유형인 호스트 메트릭 및 게스트 메트릭이 있습니다. 높은 수준에서 표준 CPU, 디스크 및 네트워크 메트릭을 사용 하려는 경우 호스트 메트릭이 적합 합니다. 그러나 더 큰 메트릭을 선택 해야 하는 경우에는 게스트 메트릭을 검토 해야 합니다.

호스트 메트릭은 호스트 VM에서 수집 되기 때문에 추가 설정이 필요 하지 않습니다. 반면 게스트 메트릭은 게스트 VM에 [Windows Azure 진단 확장](../virtual-machines/extensions/diagnostics-template.md) 또는 [Linux Azure 진단 확장](../virtual-machines/extensions/diagnostics-linux.md) 을 설치 해야 합니다. 호스트 메트릭 대신 게스트 메트릭을 사용하는 한 가지 일반적인 원인은 게스트 메트릭에서 제공되는 메트릭이 호스트 메트릭보다 더 많기 때문입니다. 이러한 예로 메모리 소비 메트릭을 들 수 있습니다. 이 메트릭은 게스트 메트릭을 통해서만 사용할 수 있습니다. 지원되는 호스트 메트릭의 목록은 [여기](../azure-monitor/essentials/metrics-supported.md)에 있으며, 흔히 사용되는 게스트 메트릭의 목록은 [여기](../azure-monitor/autoscale/autoscale-common-metrics.md)에 있습니다. 이 문서에서는 Linux 확장 집합에 대 한 게스트 메트릭을 기반으로 자동 크기 조정 규칙을 사용 하도록 기본 실행 가능한 [확장 집합 템플릿을](virtual-machine-scale-sets-mvss-start.md) 수정 하는 방법을 보여 줍니다.

## <a name="change-the-template-definition"></a>템플릿 정의 변경

[이전 문서](virtual-machine-scale-sets-mvss-start.md)에서 기본 확장 집합 템플릿을 만들었습니다. 이제 이전 템플릿을 사용 하 고 수정 하 여 게스트 메트릭 기반 자동 크기 조정으로 Linux 확장 집합을 배포 하는 템플릿을 만듭니다.

먼저, `storageAccountName` 및 `storageAccountSasToken`의 매개 변수를 추가합니다. 진단 에이전트가 이 스토리지 계정의 [테이블](../cosmos-db/tutorial-develop-table-dotnet.md)에 메트릭 데이터를 보관합니다. Linux 진단 에이전트 버전 3.0부터는 스토리지 액세스 키를 사용하는 것이 지원되지 않습니다. 대신 [SAS 토큰](../storage/common/storage-sas-overview.md)을 사용합니다.

```diff
     },
     "adminPassword": {
       "type": "securestring"
+    },
+    "storageAccountName": {
+      "type": "string"
+    },
+    "storageAccountSasToken": {
+      "type": "securestring"
     }
   },
```

다음으로, 진단 확장을 포함하도록 `extensionProfile` 확장 집합을 수정합니다. 이 구성에서는 메트릭 수집 출처인 확장 집합의 리소스 ID뿐만 아니라 이 메트릭을 저장하는 데 사용할 스토리지 계정 및 SAS 토큰도 지정합니다. 메트릭이 집계되는 빈도(이 경우에는 1분마다)와 추적할 메트릭(이 경우에는 사용된 메모리의 비율)을 지정합니다. 이 구성 및 사용된 메모리 비율 외의 다른 메트릭에 대한 자세한 내용은 [이 설명서](../virtual-machines/extensions/diagnostics-linux.md)를 참조하세요.

```diff
                 }
               }
             ]
+          },
+          "extensionProfile": {
+            "extensions": [
+              {
+                "name": "LinuxDiagnosticExtension",
+                "properties": {
+                  "publisher": "Microsoft.Azure.Diagnostics",
+                  "type": "LinuxDiagnostic",
+                  "typeHandlerVersion": "3.0",
+                  "settings": {
+                    "StorageAccount": "[parameters('storageAccountName')]",
+                    "ladCfg": {
+                      "diagnosticMonitorConfiguration": {
+                        "performanceCounters": {
+                          "sinks": "WADMetricJsonBlob",
+                          "performanceCounterConfiguration": [
+                            {
+                              "unit": "percent",
+                              "type": "builtin",
+                              "class": "memory",
+                              "counter": "percentUsedMemory",
+                              "counterSpecifier": "/builtin/memory/percentUsedMemory",
+                              "condition": "IsAggregate=TRUE"
+                            }
+                          ]
+                        },
+                        "metrics": {
+                          "metricAggregation": [
+                            {
+                              "scheduledTransferPeriod": "PT1M"
+                            }
+                          ],
+                          "resourceId": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]"
+                        }
+                      }
+                    }
+                  },
+                  "protectedSettings": {
+                    "storageAccountName": "[parameters('storageAccountName')]",
+                    "storageAccountSasToken": "[parameters('storageAccountSasToken')]",
+                    "sinksConfig": {
+                      "sink": [
+                        {
+                          "name": "WADMetricJsonBlob",
+                          "type": "JsonBlob"
+                        }
+                      ]
+                    }
+                  }
+                }
+              }
+            ]
           }
         }
       }
```

마지막으로, 이러한 메트릭을 기반으로 한 자동 크기 조정을 구성하도록 `autoscaleSettings` 리소스를 추가합니다. 이 리소스에는 확장 집합을 참조하여 자동 크기 조정을 시도하기 전에 먼저 확장 집합이 존재하는지 확인하는 `dependsOn` 절이 포함되어 있습니다. 자동 크기 조정의 기반으로 삼을 다른 메트릭을 선택하는 경우는 진단 확장 구성의 `counterSpecifier`를 자동 크기 조정 구성의 `metricName`으로 사용합니다. 자동 크기 조정 구성에 대한 자세한 내용은 [자동 크기 조정 모범 사례](../azure-monitor/autoscale/autoscale-best-practices.md) 및 [Azure Monitor REST API 참조 설명서](/rest/api/monitor/autoscalesettings)를 참조하세요.

```diff
+    },
+    {
+      "type": "Microsoft.Insights/autoscaleSettings",
+      "apiVersion": "2015-04-01",
+      "name": "guestMetricsAutoscale",
+      "location": "[resourceGroup().location]",
+      "dependsOn": [
+        "Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
+      ],
+      "properties": {
+        "name": "guestMetricsAutoscale",
+        "targetResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+        "enabled": true,
+        "profiles": [
+          {
+            "name": "Profile1",
+            "capacity": {
+              "minimum": "1",
+              "maximum": "10",
+              "default": "3"
+            },
+            "rules": [
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "GreaterThan",
+                  "threshold": 60
+                },
+                "scaleAction": {
+                  "direction": "Increase",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              },
+              {
+                "metricTrigger": {
+                  "metricName": "/builtin/memory/percentUsedMemory",
+                  "metricNamespace": "",
+                  "metricResourceUri": "[resourceId('Microsoft.Compute/virtualMachineScaleSets', 'myScaleSet')]",
+                  "timeGrain": "PT1M",
+                  "statistic": "Average",
+                  "timeWindow": "PT5M",
+                  "timeAggregation": "Average",
+                  "operator": "LessThan",
+                  "threshold": 30
+                },
+                "scaleAction": {
+                  "direction": "Decrease",
+                  "type": "ChangeCount",
+                  "value": "1",
+                  "cooldown": "PT1M"
+                }
+              }
+            ]
+          }
+        ]
+      }
     }
   ]
 }
```





## <a name="next-steps"></a>다음 단계

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
