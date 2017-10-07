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
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a><span data-ttu-id="43f2b-103">Hogyan toouse automatikus skálázás és virtuálisgép-méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="43f2b-103">How toouse automatic scaling and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="43f2b-104">Méretezési csoportban lévő virtuális gépek automatikus skálázás hello létrehozását vagy törlését gépek hello állítja be a szükséges toomatch teljesítményre vonatkozó követelmények.</span><span class="sxs-lookup"><span data-stu-id="43f2b-104">Automatic scaling of virtual machines in a scale set is hello creation or deletion of machines in hello set as needed toomatch performance requirements.</span></span> <span data-ttu-id="43f2b-105">Munka hello mennyiségének növekedésével alkalmazás szükség lehet további erőforrások tooenable azt tooeffectively feladatokat.</span><span class="sxs-lookup"><span data-stu-id="43f2b-105">As hello volume of work grows, an application may require additional resources tooenable it tooeffectively perform tasks.</span></span>

<span data-ttu-id="43f2b-106">Az automatikus skálázás be egy automatikus folyamat, hogy segítséget nyújt megkönnyítik kezelési terhelés mellett.</span><span class="sxs-lookup"><span data-stu-id="43f2b-106">Automatic scaling is an automated process that helps ease management overhead.</span></span> <span data-ttu-id="43f2b-107">Csökkenti a terhelés, nem kell toocontinually figyelő rendszerteljesítmény vagy döntse el, hogyan toomanage erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="43f2b-107">By reducing overhead, you don't need toocontinually monitor system performance or decide how toomanage resources.</span></span> <span data-ttu-id="43f2b-108">Skálázás be egy rugalmas folyamatban.</span><span class="sxs-lookup"><span data-stu-id="43f2b-108">Scaling is an elastic process.</span></span> <span data-ttu-id="43f2b-109">További erőforrásokat a terhelés növekedése hello lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="43f2b-109">More resources can be added as hello load increases.</span></span> <span data-ttu-id="43f2b-110">Igény szerint csökkenő, mint erőforrásokat is lehet eltávolított toominimize költségek és és teljesítményszintet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-110">And as demand decreases, resources can be removed toominimize costs and maintain performance levels.</span></span>

<span data-ttu-id="43f2b-111">Állítsa be az automatikus skálázás beállítása az Azure Resource Manager sablon, Azure PowerShell, Azure CLI vagy hello Azure-portál használatával méretű.</span><span class="sxs-lookup"><span data-stu-id="43f2b-111">Set up automatic scaling on a scale set by using an Azure Resource Manager template, Azure PowerShell, Azure CLI, or hello Azure portal.</span></span>

## <a name="set-up-scaling-by-using-resource-manager-templates"></a><span data-ttu-id="43f2b-112">Erőforrás-kezelő sablonjaival skálázás beállítása</span><span class="sxs-lookup"><span data-stu-id="43f2b-112">Set up scaling by using Resource Manager templates</span></span>
<span data-ttu-id="43f2b-113">Központi telepítésére és felügyeletére külön-külön az egyes erőforrások, az alkalmazás használata helyett a sablont, amely telepíti az összes erőforrást egyetlen, koordinált műveletben.</span><span class="sxs-lookup"><span data-stu-id="43f2b-113">Rather than deploying and managing each resource of your application separately, use a template that deploys all resources in a single, coordinated operation.</span></span> <span data-ttu-id="43f2b-114">Hello sablonban definiált alkalmazás-erőforrásokat, és különböző környezetek központi telepítési paraméterek vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="43f2b-114">In hello template, application resources are defined and deployment parameters are specified for different environments.</span></span> <span data-ttu-id="43f2b-115">hello sablon JSON és, hogy tooconstruct értékeket is használhat az üzembe helyezéshez kifejezések áll.</span><span class="sxs-lookup"><span data-stu-id="43f2b-115">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="43f2b-116">toolearn több, nézze meg [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-116">toolearn more, look at [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="43f2b-117">Hello sablonban hello kapacitás elemet adja meg:</span><span class="sxs-lookup"><span data-stu-id="43f2b-117">In hello template, you specify hello capacity element:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

<span data-ttu-id="43f2b-118">Kapacitás – hello beállítása a virtuális gépek hello száma.</span><span class="sxs-lookup"><span data-stu-id="43f2b-118">Capacity identifies hello number of virtual machines in hello set.</span></span> <span data-ttu-id="43f2b-119">Egy másik érték megadásával a sablonok telepítésével hello kapacitás manuálisan módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="43f2b-119">You can manually change hello capacity by deploying a template with a different value.</span></span> <span data-ttu-id="43f2b-120">Ha telepít egy sablon tooonly módosítás hello kapacitás, megadhat csak hello SKU elem frissítése hello kapacitással rendelkező átjáróeszközt.</span><span class="sxs-lookup"><span data-stu-id="43f2b-120">If you are deploying a template tooonly change hello capacity, you can include only hello SKU element with hello updated capacity.</span></span>

<span data-ttu-id="43f2b-121">hello kapacitás, a méretezési automatikusan módosítható kombinációjával hello **autoscaleSettings** erőforrás és hello diagnosztika bővítmény.</span><span class="sxs-lookup"><span data-stu-id="43f2b-121">hello capacity of your scale set can be automatically adjusted by using a combination of hello **autoscaleSettings** resource and hello diagnostics extension.</span></span>

### <a name="configure-hello-azure-diagnostics-extension"></a><span data-ttu-id="43f2b-122">Hello Azure Diagnostics-kiterjesztés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43f2b-122">Configure hello Azure Diagnostics extension</span></span>
<span data-ttu-id="43f2b-123">Az automatikus skálázás csak akkor végezhető, ha metrikák gyűjteménye sikeres hello méretezési csoportban lévő összes virtuális gépén.</span><span class="sxs-lookup"><span data-stu-id="43f2b-123">Automatic scaling can only be done if metrics collection is successful on each virtual machine in hello scale set.</span></span> <span data-ttu-id="43f2b-124">hello Azure Diagnostics bővítmény biztosít hello figyelése és diagnosztikai lehetőségei hello metrikák gyűjtemény hello automatikus skálázás erőforrás szükségleteit képes kielégíteni.</span><span class="sxs-lookup"><span data-stu-id="43f2b-124">hello Azure Diagnostics Extension provides hello monitoring and diagnostics capabilities that meets hello metrics collection needs of hello autoscale resource.</span></span> <span data-ttu-id="43f2b-125">Hello bővítmény hello Resource Manager-sablon részeként is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="43f2b-125">You can install hello extension as part of hello Resource Manager template.</span></span>

<span data-ttu-id="43f2b-126">Ez a példa bemutatja a hello változókat, amelyek hello sablon tooconfigure hello diagnosztika bővítményben:</span><span class="sxs-lookup"><span data-stu-id="43f2b-126">This example shows hello variables that are used in hello template tooconfigure hello diagnostics extension:</span></span>

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

<span data-ttu-id="43f2b-127">Paraméterek vannak megadva hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="43f2b-127">Parameters are provided when hello template is deployed.</span></span> <span data-ttu-id="43f2b-128">Ez például hello hello (adatokat tároló) storage-fiók nevét és hello nevét (Ha adatgyűjtés) hello méretezési rendelkezésre állnak.</span><span class="sxs-lookup"><span data-stu-id="43f2b-128">In this example, hello name of hello storage account (where data is stored) and hello name of hello scale set (where data is collected) are provided.</span></span> <span data-ttu-id="43f2b-129">Emellett a Windows Server példában csak hello szálak száma teljesítményszámláló gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="43f2b-129">Also in this Windows Server example, only hello Thread Count performance counter is collected.</span></span> <span data-ttu-id="43f2b-130">Az összes rendelkezésre álló teljesítményszámlálók a Windows hello vagy Linux lehet használt toocollect diagnosztikai információkat, és hello bővítménykonfiguráció tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="43f2b-130">All hello available performance counters in Windows or Linux can be used toocollect diagnostics information and can be included in hello extension configuration.</span></span>

<span data-ttu-id="43f2b-131">Ez a példa bemutatja a hello definition hello bővítmény hello sablonban:</span><span class="sxs-lookup"><span data-stu-id="43f2b-131">This example shows hello definition of hello extension in hello template:</span></span>

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

<span data-ttu-id="43f2b-132">Hello diagnosztika bővítmény futtatásakor hello adatok tárolásának és egy olyan táblázatában, amely a megadott tárfiók hello gyűjti.</span><span class="sxs-lookup"><span data-stu-id="43f2b-132">When hello diagnostics extension runs, hello data is stored and collected in a table, in hello storage account that you specify.</span></span> <span data-ttu-id="43f2b-133">A hello **WADPerformanceCounters** táblában található hello gyűjtött adatokat:</span><span class="sxs-lookup"><span data-stu-id="43f2b-133">In hello **WADPerformanceCounters** table, you can find hello collected data:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a><span data-ttu-id="43f2b-134">Hello autoScaleSettings erőforrás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43f2b-134">Configure hello autoScaleSettings resource</span></span>
<span data-ttu-id="43f2b-135">hello autoscaleSettings erőforrás hello diagnosztika bővítmény toodecide hello adatait használja, hogy a virtuális gépek hello méretezési tooincrease vagy csökken hello száma.</span><span class="sxs-lookup"><span data-stu-id="43f2b-135">hello autoscaleSettings resource uses hello information from hello diagnostics extension toodecide whether tooincrease or decrease hello number of virtual machines in hello scale set.</span></span>

<span data-ttu-id="43f2b-136">Ez a példa bemutatja a hello konfigurációs hello erőforrás hello sablonban:</span><span class="sxs-lookup"><span data-stu-id="43f2b-136">This example shows hello configuration of hello resource in hello template:</span></span>

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

<span data-ttu-id="43f2b-137">Hello a fenti példában a két szabályt hoz létre az automatikus skálázási műveletek hello meghatározása.</span><span class="sxs-lookup"><span data-stu-id="43f2b-137">In hello example above, two rules are created for defining hello automatic scaling actions.</span></span> <span data-ttu-id="43f2b-138">hello első szabály hello kibővített műveletet határoz meg, és hello második szabály meghatározása hello skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-138">hello first rule defines hello scale-out action and hello second rule defines hello scale-in action.</span></span> <span data-ttu-id="43f2b-139">Ezek az értékek hello szabályok szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="43f2b-139">These values are provided in hello rules:</span></span>

| <span data-ttu-id="43f2b-140">Szabály</span><span class="sxs-lookup"><span data-stu-id="43f2b-140">Rule</span></span> | <span data-ttu-id="43f2b-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="43f2b-141">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="43f2b-142">metricName</span><span class="sxs-lookup"><span data-stu-id="43f2b-142">metricName</span></span>        | <span data-ttu-id="43f2b-143">Ez az érték azonos a hello diagnosztika bővítmény hello wadperfcounter változóban meghatározott hello teljesítményszámláló van hello.</span><span class="sxs-lookup"><span data-stu-id="43f2b-143">This value is hello same as hello performance counter that you defined in hello wadperfcounter variable for hello diagnostics extension.</span></span> <span data-ttu-id="43f2b-144">Hello a fenti példában a hello szálak száma számláló szolgál.</span><span class="sxs-lookup"><span data-stu-id="43f2b-144">In hello example above, hello Thread Count counter is used.</span></span>    |
| <span data-ttu-id="43f2b-145">metricResourceUri</span><span class="sxs-lookup"><span data-stu-id="43f2b-145">metricResourceUri</span></span> | <span data-ttu-id="43f2b-146">Ez az érték hello erőforrás-azonosítót a virtuálisgép-méretezési csoport hello.</span><span class="sxs-lookup"><span data-stu-id="43f2b-146">This value is hello resource identifier of hello virtual machine scale set.</span></span> <span data-ttu-id="43f2b-147">Ez az azonosító hello hello erőforráscsoport nevét, hello erőforrás-szolgáltató neve hello és hello méretezési készlet tooscale hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="43f2b-147">This identifier contains hello name of hello resource group, hello name of hello resource provider, and hello name of hello scale set tooscale.</span></span> |
| <span data-ttu-id="43f2b-148">Időkeretben vannak</span><span class="sxs-lookup"><span data-stu-id="43f2b-148">timeGrain</span></span>         | <span data-ttu-id="43f2b-149">Ez az érték hello metrikák összegyűjtött hello granularitása.</span><span class="sxs-lookup"><span data-stu-id="43f2b-149">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="43f2b-150">Példa megelőző hello, az adatgyűjtés a perces időközönként.</span><span class="sxs-lookup"><span data-stu-id="43f2b-150">In hello preceding example, data is collected on an interval of one minute.</span></span> <span data-ttu-id="43f2b-151">Az időtartomány értékének ezt az értéket használják.</span><span class="sxs-lookup"><span data-stu-id="43f2b-151">This value is used with timeWindow.</span></span> |
| <span data-ttu-id="43f2b-152">statisztika</span><span class="sxs-lookup"><span data-stu-id="43f2b-152">statistic</span></span>         | <span data-ttu-id="43f2b-153">Ez az érték határozza meg, hogyan hello adatok gyűjtése le kombinált tooaccommodate hello automatikus skálázás művelet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-153">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="43f2b-154">hello lehetséges értékek a következők: átlagos, Min, Max.</span><span class="sxs-lookup"><span data-stu-id="43f2b-154">hello possible values are: Average, Min, Max.</span></span> |
| <span data-ttu-id="43f2b-155">Az időtartomány értékének</span><span class="sxs-lookup"><span data-stu-id="43f2b-155">timeWindow</span></span>        | <span data-ttu-id="43f2b-156">Ez az érték, amely hello amelyben példány adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="43f2b-156">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="43f2b-157">5 perc és 12 óra között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43f2b-157">It must be between 5 minutes and 12 hours.</span></span> |
| <span data-ttu-id="43f2b-158">timeAggregation</span><span class="sxs-lookup"><span data-stu-id="43f2b-158">timeAggregation</span></span>   | <span data-ttu-id="43f2b-159">Ez az érték szabja meg, hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="43f2b-159">This value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="43f2b-160">hello alapértelmezett érték átlaga.</span><span class="sxs-lookup"><span data-stu-id="43f2b-160">hello default value is Average.</span></span> <span data-ttu-id="43f2b-161">hello lehetséges értékek a következők: átlagos, Minimum, Maximum, utolsó, összesen, száma.</span><span class="sxs-lookup"><span data-stu-id="43f2b-161">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span> |
| <span data-ttu-id="43f2b-162">Operátor</span><span class="sxs-lookup"><span data-stu-id="43f2b-162">operator</span></span>          | <span data-ttu-id="43f2b-163">Ez az érték használt toocompare hello metrika adatok és hello küszöbérték, hello operátor.</span><span class="sxs-lookup"><span data-stu-id="43f2b-163">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="43f2b-164">hello lehetséges értékek a következők: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="43f2b-164">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span> |
| <span data-ttu-id="43f2b-165">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="43f2b-165">threshold</span></span>         | <span data-ttu-id="43f2b-166">Ez az érték hello érték, amely elindítja a hello skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-166">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="43f2b-167">Lehet, hogy tooprovide hello küszöbértékei hello elegendő különbségének **kibővített** és **méretezési a** műveletek.</span><span class="sxs-lookup"><span data-stu-id="43f2b-167">Be sure tooprovide a sufficient difference between hello threshold values for hello **scale-out** and **scale-in** actions.</span></span> <span data-ttu-id="43f2b-168">Ha mindkét műveletet ugyanazokat az értékeket állít be hello, hello rendszer állandó változásról, amely megakadályozza, hogy a méretezési művelet végrehajtása azt feltételezi.</span><span class="sxs-lookup"><span data-stu-id="43f2b-168">If you set hello same values for both actions, hello system anticipates constant change, which prevents it from implementing a scaling action.</span></span> <span data-ttu-id="43f2b-169">Például az előző példa hello mindkét too600 szálak beállítás nem működik.</span><span class="sxs-lookup"><span data-stu-id="43f2b-169">For example, setting both too600 threads in hello preceding example doesn't work.</span></span> |
| <span data-ttu-id="43f2b-170">Iránya</span><span class="sxs-lookup"><span data-stu-id="43f2b-170">direction</span></span>         | <span data-ttu-id="43f2b-171">Ez az érték határozza meg a hello hello küszöbérték érhető el, amikor végrehajtandó műveletet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-171">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="43f2b-172">hello lehetséges értékei növekedése vagy csökkenése.</span><span class="sxs-lookup"><span data-stu-id="43f2b-172">hello possible values are Increase or Decrease.</span></span> |
| <span data-ttu-id="43f2b-173">type</span><span class="sxs-lookup"><span data-stu-id="43f2b-173">type</span></span>              | <span data-ttu-id="43f2b-174">Ez az érték egy végrehajtandó műveletet kell végrehajtani, és be kell állítani tooChangeCount hello típusú.</span><span class="sxs-lookup"><span data-stu-id="43f2b-174">This value is hello type of action that should occur and must be set tooChangeCount.</span></span> |
| <span data-ttu-id="43f2b-175">érték</span><span class="sxs-lookup"><span data-stu-id="43f2b-175">value</span></span>             | <span data-ttu-id="43f2b-176">Ez az érték hello hello méretezési távolítva tooor hozzáadott virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="43f2b-176">This value is hello number of virtual machines that are added tooor removed from hello scale set.</span></span> <span data-ttu-id="43f2b-177">Ez az érték 1 vagy nagyobb lehet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-177">This value must be 1 or greater.</span></span> |
| <span data-ttu-id="43f2b-178">cooldown</span><span class="sxs-lookup"><span data-stu-id="43f2b-178">cooldown</span></span>          | <span data-ttu-id="43f2b-179">Ez az érték érték hello idő toowait óta hello utolsó méretezési művelet előtt hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="43f2b-179">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="43f2b-180">Ez az érték egy percet, és egy hét között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43f2b-180">This value must be between one minute and one week.</span></span> |

<span data-ttu-id="43f2b-181">Attól függően, hogy hello teljesítményszámláló használja, néhány hello sablon konfigurációs elemeinek hello használata eltér.</span><span class="sxs-lookup"><span data-stu-id="43f2b-181">Depending on hello performance counter, you are using, some of hello elements in hello template configuration are used differently.</span></span> <span data-ttu-id="43f2b-182">A fenti példa hello hello teljesítményszámláló szálak száma, hello küszöbérték: 650 kibővített művelethez, pedig hello küszöbérték 550 a hello skálázási művelet.</span><span class="sxs-lookup"><span data-stu-id="43f2b-182">In hello preceding example, hello performance counter is Thread Count, hello threshold value is 650 for a scale-out action, and hello threshold value is 550 for hello scale-in action.</span></span> <span data-ttu-id="43f2b-183">Ha például a processzor kihasználtsága (%) teljesítményszámláló használja, hello küszöbérték értéke határozza meg, hogy a méretezési művelet CPU-használat százalékos toohello.</span><span class="sxs-lookup"><span data-stu-id="43f2b-183">If you use a counter such as %Processor Time, hello threshold value is set toohello percentage of CPU usage that determines a scaling action.</span></span>

<span data-ttu-id="43f2b-184">Kiváltásakor a skálázási művelet, például a nagy terhelés hello kapacitás hello készlet hello érték hello sablon alapján nő.</span><span class="sxs-lookup"><span data-stu-id="43f2b-184">When a scaling action is triggered, such as a high load, hello capacity of hello set is increased based on hello value in hello template.</span></span> <span data-ttu-id="43f2b-185">Például egy méretezési készletben ahol hello kapacitás too3 hello skálázásiművelet-értéke van beállítva és too1:</span><span class="sxs-lookup"><span data-stu-id="43f2b-185">For example, in a scale set where hello capacity is set too3 and hello scale action value is set too1:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

<span data-ttu-id="43f2b-186">Ha a jelenlegi terhelés okok hello átlagos szál száma toogo 650 hello küszöbérték felett hello:</span><span class="sxs-lookup"><span data-stu-id="43f2b-186">When hello current load causes hello average thread count toogo above hello threshold of 650:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

<span data-ttu-id="43f2b-187">A **kibővített** művelet akkor váltódik ki, hogy okok hello hello beállítása toobe eggyel megnövelve kapacitásának:</span><span class="sxs-lookup"><span data-stu-id="43f2b-187">A **scale-out** action is triggered that causes hello capacity of hello set toobe increased by one:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

<span data-ttu-id="43f2b-188">hello eredménye egy virtuális gép kerül toohello méretezési:</span><span class="sxs-lookup"><span data-stu-id="43f2b-188">hello result is a virtual machine is added toohello scale set:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

<span data-ttu-id="43f2b-189">Elteltével cooldown öt perc ha hello átlagos hello gépeken futó szálak száma marad több mint 600, egy másik gép kerül toohello beállítása.</span><span class="sxs-lookup"><span data-stu-id="43f2b-189">After a cooldown period of five minutes, if hello average number of threads on hello machines stays over 600, another machine is added toohello set.</span></span> <span data-ttu-id="43f2b-190">Hello átlagos szálak száma 550 alatt marad, ha eggyel csökken hello méretezési hello kapacitás, és a rendszer eltávolítja a gépet a hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="43f2b-190">If hello average thread count stays below 550, hello capacity of hello scale set is reduced by one and a machine is removed from hello set.</span></span>

## <a name="set-up-scaling-using-azure-powershell"></a><span data-ttu-id="43f2b-191">Az Azure PowerShell skálázás beállítása</span><span class="sxs-lookup"><span data-stu-id="43f2b-191">Set up scaling using Azure PowerShell</span></span>

<span data-ttu-id="43f2b-192">Nézze meg toosee példák PowerShell tooset be az automatikus skálázást, [Azure figyelő PowerShell quick start minták](../monitoring-and-diagnostics/insights-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-192">toosee examples of using PowerShell tooset up autoscaling, look at [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md).</span></span>

## <a name="set-up-scaling-using-azure-cli"></a><span data-ttu-id="43f2b-193">Azure parancssori felület használatával skálázás beállítása</span><span class="sxs-lookup"><span data-stu-id="43f2b-193">Set up scaling using Azure CLI</span></span>

<span data-ttu-id="43f2b-194">Nézze meg az Azure parancssori felület tooset be az automatikus skálázást, toosee példák [figyelő Azure platformfüggetlen parancssori felület gyors start minták](../monitoring-and-diagnostics/insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-194">toosee examples of using Azure CLI tooset up autoscaling, look at [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md).</span></span>

## <a name="set-up-scaling-using-hello-azure-portal"></a><span data-ttu-id="43f2b-195">Hello Azure-portál használatával skálázás beállítása</span><span class="sxs-lookup"><span data-stu-id="43f2b-195">Set up scaling using hello Azure portal</span></span>

<span data-ttu-id="43f2b-196">toosee használatának példája hello Azure portál tooset be az automatikus skálázást, keresse meg a [hozzon létre egy virtuálisgép-méretezési csoportban hello Azure-portál használatával](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-196">toosee an example of using hello Azure portal tooset up autoscaling, look at [Create a Virtual Machine Scale Set using hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="investigate-scaling-actions"></a><span data-ttu-id="43f2b-197">Vizsgálja meg a skálázási műveletek</span><span class="sxs-lookup"><span data-stu-id="43f2b-197">Investigate scaling actions</span></span>

* <span data-ttu-id="43f2b-198">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="43f2b-198">**Azure portal**</span></span>  
<span data-ttu-id="43f2b-199">Jelenleg csak korlátozott mennyiségű információt hello portál használatával érheti el.</span><span class="sxs-lookup"><span data-stu-id="43f2b-199">You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="43f2b-200">**Azure Resource Explorer**</span><span class="sxs-lookup"><span data-stu-id="43f2b-200">**Azure Resource Explorer**</span></span>  
<span data-ttu-id="43f2b-201">Ez az eszköz felderítését lehetővé tevő hello aktuális állapotát, a méretezési legjobb hello.</span><span class="sxs-lookup"><span data-stu-id="43f2b-201">This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="43f2b-202">Hajtsa végre ezt az elérési utat, és megtekintheti az hello példányait tartalmazó nézetet hello méretezési beállítása létrehozott:</span><span class="sxs-lookup"><span data-stu-id="43f2b-202">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>  
<span data-ttu-id="43f2b-203">**Előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltatók > Microsoft.Compute > virtualMachineScaleSets > {a méretezési} > virtuális gépek vannak**</span><span class="sxs-lookup"><span data-stu-id="43f2b-203">**Subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Compute > virtualMachineScaleSets > {your scale set} > virtualMachines**</span></span>

* <span data-ttu-id="43f2b-204">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="43f2b-204">**Azure PowerShell**</span></span>  
<span data-ttu-id="43f2b-205">Használja a parancs tooget néhány információt:</span><span class="sxs-lookup"><span data-stu-id="43f2b-205">Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* <span data-ttu-id="43f2b-206">Csatlakoztassa a toohello jumpbox virtuális gépet, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti hello virtuális gépek hello méretezési készlet toomonitor az egyes folyamatok.</span><span class="sxs-lookup"><span data-stu-id="43f2b-206">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43f2b-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43f2b-207">Next Steps</span></span>
* <span data-ttu-id="43f2b-208">Nézze meg [automatikus méretezése a gépet egy virtuálisgép-méretezési csoportban lévő](virtual-machine-scale-sets-windows-autoscale.md) toosee hogyan toocreate egy méretezési az automatikus skálázás konfigurált példát.</span><span class="sxs-lookup"><span data-stu-id="43f2b-208">Look at [Automatically scale machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-autoscale.md) toosee an example of how toocreate a scale set with automatic scaling configured.</span></span>

* <span data-ttu-id="43f2b-209">Példák figyelési szolgáltatásokat az Azure-figyelő található [Azure figyelő PowerShell quick start minták](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="43f2b-209">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>

* <span data-ttu-id="43f2b-210">További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata Azure figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-210">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span></span>

* <span data-ttu-id="43f2b-211">Megtudhatja, hogyan túl[használata auditnaplókat toosend e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="43f2b-211">Learn about how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

* <span data-ttu-id="43f2b-212">További tudnivalók [speciális automatikus skálázás](virtual-machine-scale-sets-advanced-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="43f2b-212">Learn about [advanced autoscale scenarios](virtual-machine-scale-sets-advanced-autoscale.md).</span></span>
