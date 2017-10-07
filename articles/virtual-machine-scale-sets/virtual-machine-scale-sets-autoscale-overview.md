---
title: "aaaAutomatic méretezés és a virtuális gép méretezési készletek |} Microsoft Docs"
description: "További tudnivalók a diagnostics és az automatikus skálázás erőforrások tooautomatically méretezési virtuális gépek méretezési csoportban lévő használatával."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 25f54b693e3c991577238242008c262023ed479c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a>Hogyan toouse automatikus skálázás és virtuálisgép-méretezési csoportok
Méretezési csoportban lévő virtuális gépek automatikus skálázás hello létrehozását vagy törlését gépek hello állítja be a szükséges toomatch teljesítményre vonatkozó követelmények. Munka hello mennyiségének növekedésével alkalmazás szükség lehet további erőforrások tooenable azt tooeffectively feladatokat.

Az automatikus skálázás be egy automatikus folyamat, hogy segítséget nyújt megkönnyítik kezelési terhelés mellett. Csökkenti a terhelés, nem kell toocontinually figyelő rendszerteljesítmény vagy döntse el, hogyan toomanage erőforrásokat. Skálázás be egy rugalmas folyamatban. További erőforrásokat a terhelés növekedése hello lehet hozzáadni. Igény szerint csökkenő, mint erőforrásokat is lehet eltávolított toominimize költségek és és teljesítményszintet.

Állítsa be az automatikus skálázás beállítása az Azure Resource Manager sablon, Azure PowerShell, Azure CLI vagy hello Azure-portál használatával méretű.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Erőforrás-kezelő sablonjaival skálázás beállítása
Központi telepítésére és felügyeletére külön-külön az egyes erőforrások, az alkalmazás használata helyett a sablont, amely telepíti az összes erőforrást egyetlen, koordinált műveletben. Hello sablonban definiált alkalmazás-erőforrásokat, és különböző környezetek központi telepítési paraméterek vannak megadva. hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll. toolearn több, nézze meg [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Hello sablonban hello kapacitás elemet adja meg:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

Kapacitás – hello beállítása a virtuális gépek hello száma. Egy másik érték megadásával a sablonok telepítésével hello kapacitás manuálisan módosíthatja. Ha telepít egy sablon tooonly módosítás hello kapacitás, megadhat csak hello SKU elem frissítése hello kapacitással rendelkező átjáróeszközt.

hello kapacitás, a méretezési automatikusan módosítható kombinációjával hello **autoscaleSettings** erőforrás és hello diagnosztika bővítmény.

### <a name="configure-hello-azure-diagnostics-extension"></a>Hello Azure Diagnostics-kiterjesztés konfigurálása
Az automatikus skálázás csak akkor végezhető, ha metrikák gyűjteménye sikeres hello méretezési csoportban lévő összes virtuális gépén. hello Azure Diagnostics bővítmény biztosít hello figyelése és diagnosztikai lehetőségei hello metrikák gyűjtemény hello automatikus skálázás erőforrás szükségleteit képes kielégíteni. Hello bővítmény hello Resource Manager-sablon részeként is telepítheti.

Ez a példa bemutatja a hello változókat, amelyek hello sablon tooconfigure hello diagnosztika bővítményben:

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

Paraméterek vannak megadva hello sablon telepítésekor. Ez például hello hello (adatokat tároló) storage-fiók nevét és hello nevét (Ha adatgyűjtés) hello méretezési rendelkezésre állnak. Emellett a Windows Server példában csak hello szálak száma teljesítményszámláló gyűjtése. Az összes rendelkezésre álló teljesítményszámlálók a Windows hello vagy Linux lehet használt toocollect diagnosztikai információkat, és hello bővítménykonfiguráció tartalmazhat.

Ez a példa bemutatja a hello definition hello bővítmény hello sablonban:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Insights.VMDiagnosticsSettings",
      "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
          "storageAccount": "[variables('diagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
          "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

Hello diagnosztika bővítmény futtatásakor hello adatok tárolásának és egy olyan táblázatában, amely a megadott tárfiók hello gyűjti. A hello **WADPerformanceCounters** táblában található hello gyűjtött adatokat:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a>Hello autoScaleSettings erőforrás konfigurálása
hello autoscaleSettings erőforrás hello diagnosztika bővítmény toodecide hello adatait használja, hogy a virtuális gépek hello méretezési tooincrease vagy csökken hello száma.

Ez a példa bemutatja a hello konfigurációs hello erőforrás hello sablonban:

```json
{
  "type": "Microsoft.Insights/autoscaleSettings",
  "apiVersion": "2015-04-01",
  "name": "[concat(parameters('resourcePrefix'),'as1')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
  ],
  "properties": {
    "enabled": true,
    "name": "[concat(parameters('resourcePrefix'),'as1')]",
    "profiles": [
      {
        "name": "Profile1",
        "capacity": {
          "minimum": "1",
          "maximum": "10",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

Hello a fenti példában a két szabályt hoz létre az automatikus skálázási műveletek hello meghatározása. hello első szabály hello kibővített műveletet határoz meg, és hello második szabály meghatározása hello skálázási művelet. Ezek az értékek hello szabályok szerepelnek:

| Szabály | Leírás |
| ---- | ----------- |
| metricName        | Ez az érték azonos a hello diagnosztika bővítmény hello wadperfcounter változóban meghatározott hello teljesítményszámláló van hello. Hello a fenti példában a hello szálak száma számláló szolgál.    |
| metricResourceUri | Ez az érték hello erőforrás-azonosítót a virtuálisgép-méretezési csoport hello. Ez az azonosító hello hello erőforráscsoport nevét, hello erőforrás-szolgáltató neve hello és hello méretezési készlet tooscale hello nevét tartalmazza. |
| Időkeretben vannak         | Ez az érték hello metrikák összegyűjtött hello granularitása. Példa megelőző hello, az adatgyűjtés a perces időközönként. Az időtartomány értékének ezt az értéket használják. |
| statisztika         | Ez az érték határozza meg, hogyan hello adatok gyűjtése le kombinált tooaccommodate hello automatikus skálázás művelet. hello lehetséges értékek a következők: átlagos, Min, Max. |
| Az időtartomány értékének        | Ez az érték, amely hello amelyben példány adatgyűjtés. 5 perc és 12 óra között kell lennie. |
| timeAggregation   | Ez az érték szabja meg, hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt. hello alapértelmezett érték átlaga. hello lehetséges értékek a következők: átlagos, Minimum, Maximum, utolsó, összesen, száma. |
| Operátor          | Ez az érték használt toocompare hello metrika adatok és hello küszöbérték, hello operátor. hello lehetséges értékek a következők: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual. |
| Küszöbérték         | Ez az érték hello érték, amely elindítja a hello skálázási művelet. Lehet, hogy tooprovide hello küszöbértékei hello elegendő különbségének **kibővített** és **méretezési a** műveletek. Ha mindkét műveletet ugyanazokat az értékeket állít be hello, hello rendszer állandó változásról, amely megakadályozza, hogy a méretezési művelet végrehajtása azt feltételezi. Például az előző példa hello mindkét too600 szálak beállítás nem működik. |
| Iránya         | Ez az érték határozza meg a hello hello küszöbérték érhető el, amikor végrehajtandó műveletet. hello lehetséges értékei növekedése vagy csökkenése. |
| type              | Ez az érték egy végrehajtandó műveletet kell végrehajtani, és be kell állítani tooChangeCount hello típusú. |
| érték             | Ez az érték hello hello méretezési távolítva tooor hozzáadott virtuális gépek száma. Ez az érték 1 vagy nagyobb lehet. |
| cooldown          | Ez az érték érték hello idő toowait óta hello utolsó méretezési művelet előtt hello a következő lépéseket. Ez az érték egy percet, és egy hét között kell lennie. |

Attól függően, hogy hello teljesítményszámláló használja, néhány hello sablon konfigurációs elemeinek hello használata eltér. A fenti példa hello hello teljesítményszámláló szálak száma, hello küszöbérték: 650 kibővített művelethez, pedig hello küszöbérték 550 a hello skálázási művelet. Ha például a processzor kihasználtsága (%) teljesítményszámláló használja, hello küszöbérték értéke határozza meg, hogy a méretezési művelet CPU-használat százalékos toohello.

Kiváltásakor a skálázási művelet, például a nagy terhelés hello kapacitás hello készlet hello érték hello sablon alapján nő. Például egy méretezési készletben ahol hello kapacitás too3 hello skálázásiművelet-értéke van beállítva és too1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Ha a jelenlegi terhelés okok hello átlagos szál száma toogo 650 hello küszöbérték felett hello:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

A **kibővített** művelet akkor váltódik ki, hogy okok hello hello beállítása toobe eggyel megnövelve kapacitásának:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

hello eredménye egy virtuális gép kerül toohello méretezési:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Elteltével cooldown öt perc ha hello átlagos hello gépeken futó szálak száma marad több mint 600, egy másik gép kerül toohello beállítása. Hello átlagos szálak száma 550 alatt marad, ha eggyel csökken hello méretezési hello kapacitás, és a rendszer eltávolítja a gépet a hello beállítása.

## <a name="set-up-scaling-using-azure-powershell"></a>Az Azure PowerShell skálázás beállítása

Nézze meg toosee példák PowerShell tooset be az automatikus skálázást, [Azure figyelő PowerShell quick start minták](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Azure parancssori felület használatával skálázás beállítása

Nézze meg az Azure parancssori felület tooset be az automatikus skálázást, toosee példák [figyelő Azure platformfüggetlen parancssori felület gyors start minták](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-hello-azure-portal"></a>Hello Azure-portál használatával skálázás beállítása

toosee használatának példája hello Azure portál tooset be az automatikus skálázást, keresse meg a [hozzon létre egy virtuálisgép-méretezési csoportban hello Azure-portál használatával](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Vizsgálja meg a skálázási műveletek

* **Azure Portal**  
Jelenleg csak korlátozott mennyiségű információt hello portál használatával érheti el.

* **Azure Resource Explorer**  
Ez az eszköz felderítését lehetővé tevő hello aktuális állapotát, a méretezési legjobb hello. Hajtsa végre ezt az elérési utat, és megtekintheti az hello példányait tartalmazó nézetet hello méretezési beállítása létrehozott:  
**Előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltatók > Microsoft.Compute > virtualMachineScaleSets > {a méretezési} > virtuális gépek vannak**

* **Azure PowerShell**  
Használja a parancs tooget néhány információt:

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* Csatlakoztassa a toohello jumpbox virtuális gépet, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti hello virtuális gépek hello méretezési készlet toomonitor az egyes folyamatok.

## <a name="next-steps"></a>Következő lépések
* Nézze meg [automatikus méretezése a gépet egy virtuálisgép-méretezési csoportban lévő](virtual-machine-scale-sets-windows-autoscale.md) toosee hogyan toocreate egy méretezési az automatikus skálázás konfigurált példát.

* Példák figyelési szolgáltatásokat az Azure-figyelő található [Azure figyelő PowerShell quick start minták](../monitoring-and-diagnostics/insights-powershell-samples.md)

* További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata Azure figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).

* Megtudhatja, hogyan túl[használata auditnaplókat toosend e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

* További tudnivalók [speciális automatikus skálázás](virtual-machine-scale-sets-advanced-autoscale.md).
