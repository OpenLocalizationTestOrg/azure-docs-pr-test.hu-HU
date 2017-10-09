---
title: "Az Azure automatikus skálázási használata Linux méretezési készlet sablonban Vendég metrikák |} Microsoft Docs"
description: "Megtudhatja, hogyan tooautoscale Vendég mérőszámok használatát a Linux virtuális gépek méretezési csoportján sablon"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: na
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: negat
ms.openlocfilehash: 7afbef943a5f15c7a72dcf7114f46d521c504424
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a>Sablon Vendég mérőszámok használatát a Linux méretezési automatikus skálázás beállítása

Az Azure-ban mérőszámok, amelyek a virtuális gépek használata során, a méretezés beállítása két típusa van: néhány származnak hello a virtuális gép üzemeltetésére és mások származhat hello Vendég virtuális Gépen. Gazdagép-metrikák nem igényel további beállítást vannak összegyűjtött hello állomás virtuális gép, mert Vendég metrikák tooinstall hello igényelnek, mivel [Windows Azure diagnosztikai bővítmény](../virtual-machines/windows/extensions-diagnostics-template.md) vagy hello [Linux Azure Diagnostics bővítmény](../virtual-machines/linux/diagnostic-extension.md) hello a Vendég virtuális Gépet. Egy közös OK toouse Vendég metrikák gazdagép metrikák helyett az, hogy a Vendég metrikák metrikák gazdagép metrikák-nál nagyobb kijelölt biztosítson. Ilyen például a memória-felhasználás metrikákat, amelyek csak vendég metrikák keresztül elérhető. hello támogatott gazdagép-metrikák felsorolt [Itt](../monitoring-and-diagnostics/monitoring-supported-metrics.md), és a gyakran használt Vendég metrikák felsorolt [Itt](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toouse automatikus skálázási szabályok alapján Vendég metrikáinak Linux méretezési készlet.

## <a name="change-hello-template-definition"></a>Hello sablonleírásnak módosítása

A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello Linux méretezési készletben a Vendég-alapú automatikus skálázás látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json). Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:

Először adja hozzá azt az paramétereinek `storageAccountName` és `storageAccountSasToken`. hello diagnosztikai ügynök a metrika értékét fogja tárolni egy [tábla](../cosmos-db/table-storage-how-to-use-dotnet.md) az ezt a tárfiókot. Frissítésétől hello Linux diagnosztikai ügynök 3.0-s verziója a tárelérési kulcs használata már nem támogatott. Igazolnia kell használnia egy [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

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

A következő azt módosítása hello méretezési `extensionProfile` tooinclude hello diagnosztika bővítmény. Ebben a konfigurációban azt adja meg a hello erőforráskészletet hello skála azonosítója a toocollect metrikák, valamint tárfiók és a SAS-token toouse toostore hello metrikák hello. Azt is megadhatja, milyen gyakran (ebben az esetben percenként) hello metrikák összesítése és mely metrikák tootrack (az az eset százalékos használt memória). További információk a ezt a konfigurációt, és a használt memória százalékos eltérő metrikákat, lásd: [ebben a dokumentációban](../virtual-machines/linux/diagnostic-extension.md).

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

Végül azt adja hozzá egy `autoscaleSettings` erőforrás tooconfigure automatikus skálázási a metrikák alapján. Ehhez az erőforráshoz a `dependsOn` záradékban hivatkozott hello méretezési tooensure hello méretezési létező tooautoscale megkísérlése előtt állítsa be azt. Ha egy másik metrika tooautoscale a választjuk, akkor használjuk hello `counterSpecifier` hello diagnosztika bővítmény konfigurációjából hello, `metricName` hello automatikus skálázás konfigurációban. Az automatikus skálázás konfigurációs további információkért lásd: hello [automatikus skálázás gyakorlati tanácsok](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) és hello [Azure figyelő REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/dn931928.aspx).

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





## <a name="next-steps"></a>Következő lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
