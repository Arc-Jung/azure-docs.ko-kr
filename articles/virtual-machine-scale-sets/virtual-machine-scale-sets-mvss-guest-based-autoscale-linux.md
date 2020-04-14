---
title: Linux 규모 집합 템플릿에서 게스트 메트릭과 함께 Azure 자동 크기 조정 사용
description: Linux Virtual Machine Scale Set 템플릿에서 게스트 메트릭을 사용하여 자동으로 크기를 조정하는 방법을 알아봅니다.
author: mimckitt
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: mimckitt
ms.openlocfilehash: 8021b7b8feb6dc06fb2e48bc4e825200a1baad33
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273650"
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Linux 확장 집합 템플릿에서 게스트 메트릭을 사용한 자동 크기 조정

Azure에는 VM 및 배율 집합에서 수집되는 두 가지 유형의 메트릭이 있습니다: 호스트 메트릭 및 게스트 메트릭. 높은 수준에서 표준 CPU, 디스크 및 네트워크 메트릭을 사용하려는 경우 호스트 메트릭이 적합합니다. 그러나 더 많은 측정항목이 필요한 경우 게스트 측정항목을 살펴봐야 합니다.

호스트 메트릭은 호스트 VM에서 수집되므로 추가 설정이 필요하지 않지만 게스트 메트릭에서는 [Windows 진단 확장](../virtual-machines/windows/extensions-diagnostics-template.md) 또는 [Linux Azure 진단 확장을](../virtual-machines/linux/diagnostic-extension.md) 게스트 VM에 설치해야 합니다. 호스트 메트릭 대신 게스트 메트릭을 사용하는 한 가지 일반적인 원인은 게스트 메트릭에서 제공되는 메트릭이 호스트 메트릭보다 더 많기 때문입니다. 이러한 예로 메모리 소비 메트릭을 들 수 있습니다. 이 메트릭은 게스트 메트릭을 통해서만 사용할 수 있습니다. 지원되는 호스트 메트릭의 목록은 [여기](../azure-monitor/platform/metrics-supported.md)에 있으며, 흔히 사용되는 게스트 메트릭의 목록은 [여기](../azure-monitor/platform/autoscale-common-metrics.md)에 있습니다. 이 문서에서는 Linux 규모 집합에 대한 게스트 메트릭을 기반으로 자동 크기 조정 규칙을 사용하도록 [기본 실행 가능한 축척 집합 템플릿을](virtual-machine-scale-sets-mvss-start.md) 수정하는 방법을 보여 주습니다.

## <a name="change-the-template-definition"></a>템플릿 정의 변경

이전 [문서에서는](virtual-machine-scale-sets-mvss-start.md) 기본 축척 집합 템플릿을 만들었습니다. 이제 이전 템플릿을 사용하고 이를 수정하여 게스트 메트릭 기반 자동 크기 조정으로 Linux 스케일 집합을 배포하는 템플릿을 만듭니다.

먼저, `storageAccountName` 및 `storageAccountSasToken`의 매개 변수를 추가합니다. 진단 에이전트가 이 스토리지 계정의 [테이블](../cosmos-db/table-storage-how-to-use-dotnet.md)에 메트릭 데이터를 보관합니다. Linux 진단 에이전트 버전 3.0부터는 스토리지 액세스 키를 사용하는 것이 지원되지 않습니다. 대신 [SAS 토큰](../storage/common/storage-dotnet-shared-access-signature-part-1.md)을 사용합니다.

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

다음으로, 진단 확장을 포함하도록 `extensionProfile` 확장 집합을 수정합니다. 이 구성에서는 메트릭 수집 출처인 확장 집합의 리소스 ID뿐만 아니라 이 메트릭을 저장하는 데 사용할 스토리지 계정 및 SAS 토큰도 지정합니다. 메트릭이 집계되는 빈도(이 경우에는 1분마다)와 추적할 메트릭(이 경우에는 사용된 메모리의 비율)을 지정합니다. 이 구성 및 사용된 메모리 비율 외의 다른 메트릭에 대한 자세한 내용은 [이 설명서](../virtual-machines/linux/diagnostic-extension.md)를 참조하세요.

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

마지막으로, 이러한 메트릭을 기반으로 한 자동 크기 조정을 구성하도록 `autoscaleSettings` 리소스를 추가합니다. 이 리소스에는 확장 집합을 참조하여 자동 크기 조정을 시도하기 전에 먼저 확장 집합이 존재하는지 확인하는 `dependsOn` 절이 포함되어 있습니다. 자동 크기 조정의 기반으로 삼을 다른 메트릭을 선택하는 경우는 진단 확장 구성의 `counterSpecifier`를 자동 크기 조정 구성의 `metricName`으로 사용합니다. 자동 크기 조정 구성에 대한 자세한 내용은 [자동 크기 조정 모범 사례](../azure-monitor/platform/autoscale-best-practices.md) 및 [Azure Monitor REST API 참조 설명서](/rest/api/monitor/autoscalesettings)를 참조하세요.

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
