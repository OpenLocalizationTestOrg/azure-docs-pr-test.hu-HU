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
# <a name="autoscale-using-guest-metrics-in-a-linux-scale-set-template"></a><span data-ttu-id="a693c-103">Sablon Vendég mérőszámok használatát a Linux méretezési automatikus skálázás beállítása</span><span class="sxs-lookup"><span data-stu-id="a693c-103">Autoscale using guest metrics in a Linux scale set template</span></span>

<span data-ttu-id="a693c-104">Az Azure-ban mérőszámok, amelyek a virtuális gépek használata során, a méretezés beállítása két típusa van: néhány származnak hello a virtuális gép üzemeltetésére és mások származhat hello Vendég virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="a693c-104">There are two types of metrics in Azure that are gathered from VMs and scale sets: some come from hello host VM and others come from hello guest VM.</span></span> <span data-ttu-id="a693c-105">Gazdagép-metrikák nem igényel további beállítást vannak összegyűjtött hello állomás virtuális gép, mert Vendég metrikák tooinstall hello igényelnek, mivel [Windows Azure diagnosztikai bővítmény](../virtual-machines/windows/extensions-diagnostics-template.md) vagy hello [Linux Azure Diagnostics bővítmény](../virtual-machines/linux/diagnostic-extension.md) hello a Vendég virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a693c-105">Host metrics do not require additional setup because they are collected by hello host VM, whereas guest metrics require us tooinstall hello [Windows Azure Diagnostics extension](../virtual-machines/windows/extensions-diagnostics-template.md) or hello [Linux Azure Diagnostics extension](../virtual-machines/linux/diagnostic-extension.md) in hello guest VM.</span></span> <span data-ttu-id="a693c-106">Egy közös OK toouse Vendég metrikák gazdagép metrikák helyett az, hogy a Vendég metrikák metrikák gazdagép metrikák-nál nagyobb kijelölt biztosítson.</span><span class="sxs-lookup"><span data-stu-id="a693c-106">One common reason toouse guest metrics instead of host metrics is that guest metrics provide a larger selection of metrics than host metrics.</span></span> <span data-ttu-id="a693c-107">Ilyen például a memória-felhasználás metrikákat, amelyek csak vendég metrikák keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="a693c-107">One such example is memory-consumption metrics, which are only available via guest metrics.</span></span> <span data-ttu-id="a693c-108">hello támogatott gazdagép-metrikák felsorolt [Itt](../monitoring-and-diagnostics/monitoring-supported-metrics.md), és a gyakran használt Vendég metrikák felsorolt [Itt](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="a693c-108">hello supported host metrics are listed [here](../monitoring-and-diagnostics/monitoring-supported-metrics.md), and commonly used guest metrics are listed [here](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span> <span data-ttu-id="a693c-109">Ez a cikk bemutatja, hogyan toomodify hello [minimális életképes méretezési sablon](./virtual-machine-scale-sets-mvss-start.md) toouse automatikus skálázási szabályok alapján Vendég metrikáinak Linux méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="a693c-109">This article shows how toomodify hello [minimum viable scale set template](./virtual-machine-scale-sets-mvss-start.md) toouse autoscale rules based on guest metrics for Linux scale sets.</span></span>

## <a name="change-hello-template-definition"></a><span data-ttu-id="a693c-110">Hello sablonleírásnak módosítása</span><span class="sxs-lookup"><span data-stu-id="a693c-110">Change hello template definition</span></span>

<span data-ttu-id="a693c-111">A minimális életképes méretezési sablon beállítása látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), és a sablon telepítéséhez hello Linux méretezési készletben a Vendég-alapú automatikus skálázás látható [Itt](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a693c-111">Our minimum viable scale set template can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json), and our template for deploying hello Linux scale set with guest-based autoscale can be seen [here](https://raw.githubusercontent.com/gatneil/mvss/guest-based-autoscale-linux/azuredeploy.json).</span></span> <span data-ttu-id="a693c-112">Ez a sablon hello használt különbözeti toocreate vizsgáljuk meg (`git diff minimum-viable-scale-set existing-vnet`) adat által adat:</span><span class="sxs-lookup"><span data-stu-id="a693c-112">Let's examine hello diff used toocreate this template (`git diff minimum-viable-scale-set existing-vnet`) piece by piece:</span></span>

<span data-ttu-id="a693c-113">Először adja hozzá azt az paramétereinek `storageAccountName` és `storageAccountSasToken`.</span><span class="sxs-lookup"><span data-stu-id="a693c-113">First, we add parameters for `storageAccountName` and `storageAccountSasToken`.</span></span> <span data-ttu-id="a693c-114">hello diagnosztikai ügynök a metrika értékét fogja tárolni egy [tábla](../cosmos-db/table-storage-how-to-use-dotnet.md) az ezt a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a693c-114">hello diagnostics agent will store metric data in a [table](../cosmos-db/table-storage-how-to-use-dotnet.md) in this storage account.</span></span> <span data-ttu-id="a693c-115">Frissítésétől hello Linux diagnosztikai ügynök 3.0-s verziója a tárelérési kulcs használata már nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a693c-115">As of hello Linux Diagnostics Agent version 3.0, using a storage access key is no longer supported.</span></span> <span data-ttu-id="a693c-116">Igazolnia kell használnia egy [SAS-Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="a693c-116">We must use a [SAS Token](../storage/common/storage-dotnet-shared-access-signature-part-1.md).</span></span>

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

<span data-ttu-id="a693c-117">A következő azt módosítása hello méretezési `extensionProfile` tooinclude hello diagnosztika bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a693c-117">Next, we modify hello scale set `extensionProfile` tooinclude hello diagnostics extension.</span></span> <span data-ttu-id="a693c-118">Ebben a konfigurációban azt adja meg a hello erőforráskészletet hello skála azonosítója a toocollect metrikák, valamint tárfiók és a SAS-token toouse toostore hello metrikák hello.</span><span class="sxs-lookup"><span data-stu-id="a693c-118">In this configuration, we specify hello resource ID of hello scale set toocollect metrics from, as well as hello storage account and SAS token toouse toostore hello metrics.</span></span> <span data-ttu-id="a693c-119">Azt is megadhatja, milyen gyakran (ebben az esetben percenként) hello metrikák összesítése és mely metrikák tootrack (az az eset százalékos használt memória).</span><span class="sxs-lookup"><span data-stu-id="a693c-119">We also specify how frequently hello metrics are aggregated (in this case every minute) and which metrics tootrack (in this case percent used memory).</span></span> <span data-ttu-id="a693c-120">További információk a ezt a konfigurációt, és a használt memória százalékos eltérő metrikákat, lásd: [ebben a dokumentációban](../virtual-machines/linux/diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="a693c-120">For more detailed information on this configuration and metrics other than percent used memory, see [this documentation](../virtual-machines/linux/diagnostic-extension.md).</span></span>

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

<span data-ttu-id="a693c-121">Végül azt adja hozzá egy `autoscaleSettings` erőforrás tooconfigure automatikus skálázási a metrikák alapján.</span><span class="sxs-lookup"><span data-stu-id="a693c-121">Finally, we add an `autoscaleSettings` resource tooconfigure autoscale based on these metrics.</span></span> <span data-ttu-id="a693c-122">Ehhez az erőforráshoz a `dependsOn` záradékban hivatkozott hello méretezési tooensure hello méretezési létező tooautoscale megkísérlése előtt állítsa be azt.</span><span class="sxs-lookup"><span data-stu-id="a693c-122">This resource has a `dependsOn` clause that references hello scale set tooensure that hello scale set exists before attempting tooautoscale it.</span></span> <span data-ttu-id="a693c-123">Ha egy másik metrika tooautoscale a választjuk, akkor használjuk hello `counterSpecifier` hello diagnosztika bővítmény konfigurációjából hello, `metricName` hello automatikus skálázás konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="a693c-123">If we choose a different metric tooautoscale on, we would use hello `counterSpecifier` from hello diagnostics extension configuration as hello `metricName` in hello autoscale configuration.</span></span> <span data-ttu-id="a693c-124">Az automatikus skálázás konfigurációs további információkért lásd: hello [automatikus skálázás gyakorlati tanácsok](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) és hello [Azure figyelő REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span><span class="sxs-lookup"><span data-stu-id="a693c-124">For more information on autoscale configuration, see hello [autoscale best practices](..//monitoring-and-diagnostics/insights-autoscale-best-practices.md) and hello [Azure Monitor REST API reference documentation](https://msdn.microsoft.com/library/azure/dn931928.aspx).</span></span>

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





## <a name="next-steps"></a><span data-ttu-id="a693c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a693c-125">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
