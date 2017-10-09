---
title: "aaaVertically méretezési Azure virtuálisgép-méretezési csoportok |} Microsoft Docs"
description: "Hogyan toovertically méretezése válasz toomonitoring értesítések az Azure Automation szolgáltatásban, a virtuális gép"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 1cc35a805b6a5742252a89c21588ca451ff547a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a><span data-ttu-id="e3111-103">Virtuálisgép-méretezési csoportok függőleges skálázva</span><span class="sxs-lookup"><span data-stu-id="e3111-103">Vertical autoscale with Virtual Machine Scale sets</span></span>
<span data-ttu-id="e3111-104">Ez a cikk ismerteti, hogyan toovertically méretezése Azure [virtuálisgép-méretezési csoportok](https://azure.microsoft.com/services/virtual-machine-scale-sets/) vagy reprovisioning nélkül.</span><span class="sxs-lookup"><span data-stu-id="e3111-104">This article describes how toovertically scale Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) with or without reprovisioning.</span></span> <span data-ttu-id="e3111-105">Virtuális gépek, amelyek nem szerepelnek a méretezési készlet függőleges skálázás, tekintse meg túl[függőleges skálázása az Azure virtuális gép az Azure Automation szolgáltatásban](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e3111-105">For vertical scaling of VMs which are not in scale sets, refer too[Vertically scale Azure virtual machine with Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="e3111-106">Függőleges skálázás, más néven *vertikális felskálázás* és *csökkentheti*, azt jelenti, hogy növelésével vagy csökkentésével válasz tooa terheléseknél engedélyezett virtuális gépek (VM) méretét.</span><span class="sxs-lookup"><span data-stu-id="e3111-106">Vertical scaling, also known as *scale up* and *scale down*, means increasing or decreasing virtual machine (VM) sizes in response tooa workload.</span></span> <span data-ttu-id="e3111-107">Hasonlítsa össze a [horizontális skálázás](virtual-machine-scale-sets-autoscale-overview.md), más néven tooas *horizontális felskálázás* és *méretezése*, ahol a virtuális gépek száma hello van-e módosítva az hello munkaterhelés függően.</span><span class="sxs-lookup"><span data-stu-id="e3111-107">Compare this with [horizontal scaling](virtual-machine-scale-sets-autoscale-overview.md), also referred tooas *scale out* and *scale in*, where hello number of VMs is altered depending on hello workload.</span></span>

<span data-ttu-id="e3111-108">Reprovisioning azt jelenti, hogy egy meglévő virtuális gép eltávolításakor, és cserélje le a egy újat.</span><span class="sxs-lookup"><span data-stu-id="e3111-108">Reprovisioning means removing an existing VM and replacing it with a new one.</span></span> <span data-ttu-id="e3111-109">Vagy a Virtuálisgép-méretezési csoportban lévő virtuális gépek hello méretének csökkentése, egyes esetekben azt szeretné, hogy a meglévő virtuális gépek tooresize és megőrizni az adatokat, amíg más esetekben a toodeploy kell új virtuális gépeinek hello új méretét.</span><span class="sxs-lookup"><span data-stu-id="e3111-109">When you increase or decrease hello size of VMs in a VM Scale Set, in some cases you want tooresize existing VMs and retain your data, while in other cases you need toodeploy new VMs of hello new size.</span></span> <span data-ttu-id="e3111-110">Ez a dokumentum ismerteti a mindkét esetben.</span><span class="sxs-lookup"><span data-stu-id="e3111-110">This document covers both cases.</span></span>

<span data-ttu-id="e3111-111">Függőleges skálázás esetekben lehet hasznos:</span><span class="sxs-lookup"><span data-stu-id="e3111-111">Vertical scaling can be useful when:</span></span>

* <span data-ttu-id="e3111-112">Egy szolgáltatás, amely a virtuális gépek szükség szerint (például a hétvégéket).</span><span class="sxs-lookup"><span data-stu-id="e3111-112">A service built on virtual machines is under-utilized (for example at weekends).</span></span> <span data-ttu-id="e3111-113">Virtuálisgép-méretet hello csökkentése csökkentheti a havi költségeket.</span><span class="sxs-lookup"><span data-stu-id="e3111-113">Reducing hello VM size can reduce monthly costs.</span></span>
* <span data-ttu-id="e3111-114">Virtuális gép mérete toocope a nagyobb igény szerinti növelésével további virtuális gépek létrehozása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e3111-114">Increasing VM size toocope with larger demand without creating additional VMs.</span></span>

<span data-ttu-id="e3111-115">Függőleges állíthat be a metrika alapján riasztások a Virtuálisgép-méretezési csoportban indított toobe skálázás alapján.</span><span class="sxs-lookup"><span data-stu-id="e3111-115">You can set up vertical scaling toobe triggered based on metric based alerts from your VM Scale Set.</span></span> <span data-ttu-id="e3111-116">Hello riasztás aktiválásakor akkor következik be egy webhook, hogy a skála méretezhető, amely runbook beállítása felfelé vagy lefelé eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="e3111-116">When hello alert is activated it fires a webhook that triggers a runbook which can scale your scale set up or down.</span></span> <span data-ttu-id="e3111-117">Függőleges skálázás konfigurálhatja ezeket a lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="e3111-117">Vertical scaling can be configured by following these steps:</span></span>

1. <span data-ttu-id="e3111-118">Hozzon létre egy Azure Automation-fiók futtató funkció.</span><span class="sxs-lookup"><span data-stu-id="e3111-118">Create an Azure Automation account with run-as capability.</span></span>
2. <span data-ttu-id="e3111-119">Azure Automation függőleges méretezési runbookok a Virtuálisgép-méretezési készlet importálja az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e3111-119">Import Azure Automation Vertical Scale runbooks for VM Scale Sets into your subscription.</span></span>
3. <span data-ttu-id="e3111-120">Adjon hozzá egy webhook tooyour runbookot.</span><span class="sxs-lookup"><span data-stu-id="e3111-120">Add a webhook tooyour runbook.</span></span>
4. <span data-ttu-id="e3111-121">Adjon hozzá egy riasztási tooyour Virtuálisgép-méretezési csoportban webhook értesítést használ.</span><span class="sxs-lookup"><span data-stu-id="e3111-121">Add an alert tooyour VM Scale Set using a webhook notification.</span></span>

> [!NOTE]
> <span data-ttu-id="e3111-122">Függőleges automatikus skálázás csak akkor kerül sor a VM-méretek egyes tartományokon belül.</span><span class="sxs-lookup"><span data-stu-id="e3111-122">Vertical autoscaling can only take place within certain ranges of VM sizes.</span></span> <span data-ttu-id="e3111-123">Hasonlítsa össze a hello specifikációk minden méretű előtt tooscale egy tooanother a legmegfelelőbb (nagyobb szám nem mindig utalhat, hogy nagyobb virtuális gép mérete).</span><span class="sxs-lookup"><span data-stu-id="e3111-123">Compare hello specifications of each size before deciding tooscale from one tooanother (higher number does not always indicate bigger VM size).</span></span> <span data-ttu-id="e3111-124">Választhat, hogy tooscale hello párok méretű a következő között:</span><span class="sxs-lookup"><span data-stu-id="e3111-124">You can choose tooscale between hello following pairs of sizes:</span></span>
> 
> | <span data-ttu-id="e3111-125">Virtuálisgép-méretek pár skálázás</span><span class="sxs-lookup"><span data-stu-id="e3111-125">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="e3111-126">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="e3111-126">Standard_A0</span></span> |<span data-ttu-id="e3111-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="e3111-127">Standard_A11</span></span> |
> | <span data-ttu-id="e3111-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="e3111-128">Standard_D1</span></span> |<span data-ttu-id="e3111-129">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="e3111-129">Standard_D14</span></span> |
> | <span data-ttu-id="e3111-130">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="e3111-130">Standard_DS1</span></span> |<span data-ttu-id="e3111-131">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="e3111-131">Standard_DS14</span></span> |
> | <span data-ttu-id="e3111-132">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="e3111-132">Standard_D1v2</span></span> |<span data-ttu-id="e3111-133">Standard_D15v2</span><span class="sxs-lookup"><span data-stu-id="e3111-133">Standard_D15v2</span></span> |
> | <span data-ttu-id="e3111-134">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="e3111-134">Standard_G1</span></span> |<span data-ttu-id="e3111-135">Standard szintű, G5</span><span class="sxs-lookup"><span data-stu-id="e3111-135">Standard_G5</span></span> |
> | <span data-ttu-id="e3111-136">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="e3111-136">Standard_GS1</span></span> |<span data-ttu-id="e3111-137">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="e3111-137">Standard_GS5</span></span> |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a><span data-ttu-id="e3111-138">Hozzon létre egy Azure Automation-fiók futtató funkció</span><span class="sxs-lookup"><span data-stu-id="e3111-138">Create an Azure Automation Account with run-as capability</span></span>
<span data-ttu-id="e3111-139">hello elsőként toodo szüksége, hozzon létre egy Azure Automation-fiók hello használt runbookok tooscale hello Virtuálisgép-méretezési csoport példányait futtató.</span><span class="sxs-lookup"><span data-stu-id="e3111-139">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="e3111-140">Nemrég [Azure Automation](https://azure.microsoft.com/services/automation/) hello "Futtatás mint fiók" funkció automatikusan hello runbookok futtatásáért egy felhasználó nevében nagyon egyszerű szolgáltatásnevet hello beállítása így vezetett be.</span><span class="sxs-lookup"><span data-stu-id="e3111-140">Recently [Azure Automation](https://azure.microsoft.com/services/automation/) introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on a user's behalf very easy.</span></span> <span data-ttu-id="e3111-141">További a hello a cikkben az alábbi:</span><span class="sxs-lookup"><span data-stu-id="e3111-141">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="e3111-142">Runbookok hitelesítése Azure-beli futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="e3111-142">Authenticate Runbooks with Azure Run As account</span></span>](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="e3111-143">Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="e3111-143">Import Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="e3111-144">runbookokat hello szükséges toovertically méretezési hello Azure Automation-Runbook gyűjteménye már közzé a Virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="e3111-144">hello runbooks needed toovertically scale your VM Scale Sets are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="e3111-145">tooimport őket az előfizetés hello kövesse ebben a cikkben ismertetett visszaállítási lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e3111-145">tooimport them into your subscription follow hello steps in this article:</span></span>

* [<span data-ttu-id="e3111-146">Az Azure Automation forgatókönyv- és minták</span><span class="sxs-lookup"><span data-stu-id="e3111-146">Runbook and module galleries for Azure Automation</span></span>](../automation/automation-runbook-gallery.md)

<span data-ttu-id="e3111-147">Válassza ki a hello Tallózás gyűjtemény beállítást Runbookokat hello menüből:</span><span class="sxs-lookup"><span data-stu-id="e3111-147">Choose hello Browse Gallery option from hello Runbooks menu:</span></span>

![Importált Runbookok toobe][runbooks]

<span data-ttu-id="e3111-149">importált toobe igénylő runbookokat hello jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e3111-149">hello runbooks that need toobe imported are shown.</span></span> <span data-ttu-id="e3111-150">Válassza ki azt kívánja-e függőleges vagy anélkül reprovisioning skálázás alapján hello runbookot:</span><span class="sxs-lookup"><span data-stu-id="e3111-150">Select hello runbook based on whether you want vertical scaling with or without reprovisioning:</span></span>

![Runbookok gyűjteménye][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="e3111-152">A webhook tooyour runbook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3111-152">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="e3111-153">Miután importált runbookokat hello szüksége lesz tooadd webhook toohello runbook úgy is elindítható a Virtuálisgép-méretezési csoportban a riasztások alapján.</span><span class="sxs-lookup"><span data-stu-id="e3111-153">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a VM Scale Set.</span></span> <span data-ttu-id="e3111-154">a webhook létrehozása a runbook hello részleteit ebben a cikkben ismertetett:</span><span class="sxs-lookup"><span data-stu-id="e3111-154">hello details of creating a webhook for your Runbook are described in this article:</span></span>

* [<span data-ttu-id="e3111-155">Azure Automation-webhook</span><span class="sxs-lookup"><span data-stu-id="e3111-155">Azure Automation webhooks</span></span>](../automation/automation-webhooks.md)

> [!NOTE]
> <span data-ttu-id="e3111-156">Ellenőrizze, hogy hello webhook URI hello webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban hello előtt másolja át.</span><span class="sxs-lookup"><span data-stu-id="e3111-156">Make sure you copy hello webhook URI before closing hello webhook dialog as you will need this in hello next section.</span></span>
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a><span data-ttu-id="e3111-157">Egy Virtuálisgép-méretezési csoportban riasztási tooyour hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3111-157">Add an alert tooyour VM Scale Set</span></span>
<span data-ttu-id="e3111-158">Az alábbiakban a PowerShell parancsfájl mely bemutatja, hogyan tooadd egy riasztási tooa Virtuálisgép-méretezési csoportban.</span><span class="sxs-lookup"><span data-stu-id="e3111-158">Below is a PowerShell script which shows how tooadd an alert tooa VM Scale Set.</span></span> <span data-ttu-id="e3111-159">Tekintse meg a cikk tooget hello hello metrika toofire hello riasztás nevét következő toohello: [Azure figyelő automatikus skálázás közös metrikák](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e3111-159">Refer toohello following article tooget hello name of hello metric toofire hello alert on: [Azure Monitor autoscaling common metrics](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).</span></span>

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> <span data-ttu-id="e3111-160">Az ajánlott tooconfigure egy ésszerű időkerete hello riasztással rendelés tooavoid kiváltó függőleges méretezés és szolgáltatáskiesést, túl gyakran az esetleg hozzá tartozó.</span><span class="sxs-lookup"><span data-stu-id="e3111-160">It is recommended tooconfigure a reasonable time window for hello alert in order tooavoid triggering vertical scaling, and any associated service interruption, too often.</span></span> <span data-ttu-id="e3111-161">Fontolja meg egy legalább 20-30 perc vagy több ablakot.</span><span class="sxs-lookup"><span data-stu-id="e3111-161">Consider a window of least 20-30 minutes or more.</span></span> <span data-ttu-id="e3111-162">Vegye figyelembe a horizontális skálázás, ha tooavoid megszakításának van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e3111-162">Consider horizontal scaling if you need tooavoid any interruption.</span></span>
> 
> 

<span data-ttu-id="e3111-163">További információ a hogyan toocreate riasztások tekintse meg a következő cikkek toohello:</span><span class="sxs-lookup"><span data-stu-id="e3111-163">For more information on how toocreate alerts refer toohello following articles:</span></span>

* [<span data-ttu-id="e3111-164">A figyelő PowerShell Azure gyors üzembe helyezési-minták</span><span class="sxs-lookup"><span data-stu-id="e3111-164">Azure Monitor PowerShell quick start samples</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [<span data-ttu-id="e3111-165">Figyelő platformfüggetlen parancssori felület Azure gyors üzembe helyezési-minták</span><span class="sxs-lookup"><span data-stu-id="e3111-165">Azure Monitor Cross-platform CLI quick start samples</span></span>](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a><span data-ttu-id="e3111-166">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e3111-166">Summary</span></span>
<span data-ttu-id="e3111-167">Ez a cikk bemutatta egyszerű függőleges méretezési példákat.</span><span class="sxs-lookup"><span data-stu-id="e3111-167">This article showed simple vertical scaling examples.</span></span> <span data-ttu-id="e3111-168">Ezeket az építőelemeket - Automation-fiók, a runbookok, webhookokkal, riasztások - események gazdag számos műveletek testreszabott számú csatlakoztathatja.</span><span class="sxs-lookup"><span data-stu-id="e3111-168">With these building blocks - Automation account, runbooks, webhooks, alerts - you can connect a rich variety of events with a customized set of actions.</span></span>

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
