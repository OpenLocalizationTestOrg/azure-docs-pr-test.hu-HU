---
title: "aaaVertically scale Azure virtuális gép az Azure Automation szolgáltatásban |} Microsoft Docs"
description: "Hogyan toovertically méretezése válasz toomonitoring értesítések az Azure Automation szolgáltatásban, a Linux virtuális gépek"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee4c1c33a588bd907d107f1828380a8afdaa725e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a><span data-ttu-id="f313a-103">Függőleges skálázása az Azure Linux virtuális gép az Azure Automation szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="f313a-103">Vertically scale Azure Linux virtual machine with Azure Automation</span></span>
<span data-ttu-id="f313a-104">Függőleges skálázás rendszer hello növelésével vagy csökkentésével hello erőforrások a gépek válasz toohello terheléseknél engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f313a-104">Vertical scaling is hello process of increasing or decreasing hello resources of a machine in response toohello workload.</span></span> <span data-ttu-id="f313a-105">Az Azure-ban ehhez hello virtuális gép méretét hello módosításával.</span><span class="sxs-lookup"><span data-stu-id="f313a-105">In Azure this can be accomplished by changing hello size of hello Virtual Machine.</span></span> <span data-ttu-id="f313a-106">Ez segítheti a következő forgatókönyvek hello</span><span class="sxs-lookup"><span data-stu-id="f313a-106">This can help in hello following scenarios</span></span>

* <span data-ttu-id="f313a-107">Ha a virtuális gép hello gyakran nincs használatban, átméretezhető tooa kisebb méretű tooreduce le a havi költségeket</span><span class="sxs-lookup"><span data-stu-id="f313a-107">If hello Virtual Machine is not being used frequently, you can resize it down tooa smaller size tooreduce your monthly costs</span></span>
* <span data-ttu-id="f313a-108">Ha a virtuális gép hello a csúcsterhelés lát, is lehet átméretezett tooa nagyobb méretű tooincrease a kapacitás</span><span class="sxs-lookup"><span data-stu-id="f313a-108">If hello Virtual Machine is seeing a peak load, it can be resized tooa larger size tooincrease its capacity</span></span>

<span data-ttu-id="f313a-109">hello vázlat hello lépéseket tooaccomplish ez van, mint az alábbi</span><span class="sxs-lookup"><span data-stu-id="f313a-109">hello outline for hello steps tooaccomplish this is as below</span></span>

1. <span data-ttu-id="f313a-110">Azure Automation tooaccess beállítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f313a-110">Setup Azure Automation tooaccess your Virtual Machines</span></span>
2. <span data-ttu-id="f313a-111">Azure Automation függőleges méretezési runbookokat hello importálnia kell az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="f313a-111">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
3. <span data-ttu-id="f313a-112">A webhook tooyour runbook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f313a-112">Add a webhook tooyour runbook</span></span>
4. <span data-ttu-id="f313a-113">Egy riasztás tooyour virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f313a-113">Add an alert tooyour Virtual Machine</span></span>

> [!NOTE]
> <span data-ttu-id="f313a-114">Hello hello mérete miatt korlátozott lehet első virtuális gépen, akkor is méretezhető, hello mérete miatt hello toohello rendelkezésre állását más méretek hello fürt aktuális virtuális gép üzemel.</span><span class="sxs-lookup"><span data-stu-id="f313a-114">Because of hello size of hello first Virtual Machine, hello sizes it can be scaled to, may be limited due toohello availability of hello other sizes in hello cluster current Virtual Machine is deployed in.</span></span> <span data-ttu-id="f313a-115">Hello közzé azt ebben az esetben mi gondoskodunk és a virtuális gép mérete párok alatt hello belül csak méret cikkben használt automation-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="f313a-115">In hello published automation runbooks used in this article we take care of this case and only scale within hello below VM size pairs.</span></span> <span data-ttu-id="f313a-116">Ez azt jelenti, hogy egy Standard_D1v2 virtuális gép lesz nem hirtelen tooStandard_G5 kiterjesztett vagy méretezhető tooBasic_A0 le.</span><span class="sxs-lookup"><span data-stu-id="f313a-116">This means that a Standard_D1v2 Virtual Machine will not suddenly be scaled up tooStandard_G5 or scaled down tooBasic_A0.</span></span>
> 
> | <span data-ttu-id="f313a-117">Virtuálisgép-méretek pár skálázás</span><span class="sxs-lookup"><span data-stu-id="f313a-117">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="f313a-118">Basic_A0</span><span class="sxs-lookup"><span data-stu-id="f313a-118">Basic_A0</span></span> |<span data-ttu-id="f313a-119">Basic_A4</span><span class="sxs-lookup"><span data-stu-id="f313a-119">Basic_A4</span></span> |
> | <span data-ttu-id="f313a-120">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="f313a-120">Standard_A0</span></span> |<span data-ttu-id="f313a-121">Standard_A4</span><span class="sxs-lookup"><span data-stu-id="f313a-121">Standard_A4</span></span> |
> | <span data-ttu-id="f313a-122">Standard_A5</span><span class="sxs-lookup"><span data-stu-id="f313a-122">Standard_A5</span></span> |<span data-ttu-id="f313a-123">Standard_A7</span><span class="sxs-lookup"><span data-stu-id="f313a-123">Standard_A7</span></span> |
> | <span data-ttu-id="f313a-124">Standard_A8</span><span class="sxs-lookup"><span data-stu-id="f313a-124">Standard_A8</span></span> |<span data-ttu-id="f313a-125">Standard_A9</span><span class="sxs-lookup"><span data-stu-id="f313a-125">Standard_A9</span></span> |
> | <span data-ttu-id="f313a-126">Standard_A10</span><span class="sxs-lookup"><span data-stu-id="f313a-126">Standard_A10</span></span> |<span data-ttu-id="f313a-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="f313a-127">Standard_A11</span></span> |
> | <span data-ttu-id="f313a-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="f313a-128">Standard_D1</span></span> |<span data-ttu-id="f313a-129">Standard_D4</span><span class="sxs-lookup"><span data-stu-id="f313a-129">Standard_D4</span></span> |
> | <span data-ttu-id="f313a-130">Standard_D11</span><span class="sxs-lookup"><span data-stu-id="f313a-130">Standard_D11</span></span> |<span data-ttu-id="f313a-131">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="f313a-131">Standard_D14</span></span> |
> | <span data-ttu-id="f313a-132">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="f313a-132">Standard_DS1</span></span> |<span data-ttu-id="f313a-133">Standard_DS4</span><span class="sxs-lookup"><span data-stu-id="f313a-133">Standard_DS4</span></span> |
> | <span data-ttu-id="f313a-134">Standard_DS11</span><span class="sxs-lookup"><span data-stu-id="f313a-134">Standard_DS11</span></span> |<span data-ttu-id="f313a-135">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="f313a-135">Standard_DS14</span></span> |
> | <span data-ttu-id="f313a-136">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="f313a-136">Standard_D1v2</span></span> |<span data-ttu-id="f313a-137">Standard_D5v2</span><span class="sxs-lookup"><span data-stu-id="f313a-137">Standard_D5v2</span></span> |
> | <span data-ttu-id="f313a-138">Standard_D11v2</span><span class="sxs-lookup"><span data-stu-id="f313a-138">Standard_D11v2</span></span> |<span data-ttu-id="f313a-139">Standard_D14v2</span><span class="sxs-lookup"><span data-stu-id="f313a-139">Standard_D14v2</span></span> |
> | <span data-ttu-id="f313a-140">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="f313a-140">Standard_G1</span></span> |<span data-ttu-id="f313a-141">Standard szintű, G5</span><span class="sxs-lookup"><span data-stu-id="f313a-141">Standard_G5</span></span> |
> | <span data-ttu-id="f313a-142">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="f313a-142">Standard_GS1</span></span> |<span data-ttu-id="f313a-143">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="f313a-143">Standard_GS5</span></span> |
> 
> 

## <a name="setup-azure-automation-tooaccess-your-virtual-machines"></a><span data-ttu-id="f313a-144">Azure Automation tooaccess beállítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f313a-144">Setup Azure Automation tooaccess your Virtual Machines</span></span>
<span data-ttu-id="f313a-145">hello elsőként toodo szüksége, hozzon létre egy Azure Automation-fiók hello használt runbookok tooscale hello Virtuálisgép-méretezési csoport példányait futtató.</span><span class="sxs-lookup"><span data-stu-id="f313a-145">hello first thing you need toodo is create an Azure Automation account that will host hello runbooks used tooscale hello VM Scale Set instances.</span></span> <span data-ttu-id="f313a-146">Hello Automation szolgáltatás új hello "Futtatás mint fiók" funkció automatikusan futtatása hello runbookokat hello felhasználó nevében nagyon egyszerű szolgáltatásnevet hello beállítása így.</span><span class="sxs-lookup"><span data-stu-id="f313a-146">Recently hello Automation service introduced hello "Run As account" feature which makes setting up hello Service Principal for automatically running hello runbooks on hello user's behalf very easy.</span></span> <span data-ttu-id="f313a-147">További a hello a cikkben az alábbi:</span><span class="sxs-lookup"><span data-stu-id="f313a-147">You can read more about this in hello article below:</span></span>

* [<span data-ttu-id="f313a-148">Runbookok hitelesítése Azure-beli futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="f313a-148">Authenticate Runbooks with Azure Run As account</span></span>](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-hello-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="f313a-149">Azure Automation függőleges méretezési runbookokat hello importálnia kell az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="f313a-149">Import hello Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="f313a-150">Függőleges a virtuális gép már közzé hello Azure Automation-Runbook gyűjteménye méretezéshez szükséges runbookokat hello.</span><span class="sxs-lookup"><span data-stu-id="f313a-150">hello runbooks that are needed for Vertically Scaling your Virtual Machine are already published in hello Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="f313a-151">Tooimport kell őket az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="f313a-151">You will need tooimport them into your subscription.</span></span> <span data-ttu-id="f313a-152">Megtudhatja, hogyan olvasásával tooimport runbookokat hello következő cikket.</span><span class="sxs-lookup"><span data-stu-id="f313a-152">You can learn how tooimport runbooks by reading hello following article.</span></span>

* [<span data-ttu-id="f313a-153">Az Azure Automation forgatókönyv- és minták</span><span class="sxs-lookup"><span data-stu-id="f313a-153">Runbook and module galleries for Azure Automation</span></span>](../../automation/automation-runbook-gallery.md)

<span data-ttu-id="f313a-154">importált toobe igénylő runbookokat hello hello az alábbi képen látható</span><span class="sxs-lookup"><span data-stu-id="f313a-154">hello runbooks that need toobe imported are shown in hello image below</span></span>

![Runbookok importálása](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-tooyour-runbook"></a><span data-ttu-id="f313a-156">A webhook tooyour runbook hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f313a-156">Add a webhook tooyour runbook</span></span>
<span data-ttu-id="f313a-157">Miután importált runbookokat hello szüksége lesz tooadd webhook toohello runbook úgy is elindítható a virtuális gép riasztások alapján.</span><span class="sxs-lookup"><span data-stu-id="f313a-157">Once you've imported hello runbooks you'll need tooadd a webhook toohello runbook so it can be triggered by an alert from a Virtual Machine.</span></span> <span data-ttu-id="f313a-158">a webhook létrehozása a runbook hello részleteit itt olvasható</span><span class="sxs-lookup"><span data-stu-id="f313a-158">hello details of creating a webhook for your Runbook can be read here</span></span>

* [<span data-ttu-id="f313a-159">Azure Automation-webhook</span><span class="sxs-lookup"><span data-stu-id="f313a-159">Azure Automation webhooks</span></span>](../../automation/automation-webhooks.md)

<span data-ttu-id="f313a-160">Ellenőrizze, hogy hello webhook hello webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban hello előtt másolja át.</span><span class="sxs-lookup"><span data-stu-id="f313a-160">Make sure you copy hello webhook before closing hello webhook dialog as you will need this in hello next section.</span></span>

## <a name="add-an-alert-tooyour-virtual-machine"></a><span data-ttu-id="f313a-161">Egy riasztás tooyour virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f313a-161">Add an alert tooyour Virtual Machine</span></span>
1. <span data-ttu-id="f313a-162">Válassza ki a virtuális gép beállításait</span><span class="sxs-lookup"><span data-stu-id="f313a-162">Select Virtual Machine settings</span></span>
2. <span data-ttu-id="f313a-163">Válassza ki a "Riasztási szabályok"</span><span class="sxs-lookup"><span data-stu-id="f313a-163">Select "Alert rules"</span></span>
3. <span data-ttu-id="f313a-164">Válassza a "Riasztás hozzáadása"</span><span class="sxs-lookup"><span data-stu-id="f313a-164">Select "Add alert"</span></span>
4. <span data-ttu-id="f313a-165">Jelölje be a metrika toofire hello riasztások a</span><span class="sxs-lookup"><span data-stu-id="f313a-165">Select a metric toofire hello alert on</span></span>
5. <span data-ttu-id="f313a-166">Válassza ki azt a feltételt, amely teljesítése lesz hello riasztási toofire okozhat</span><span class="sxs-lookup"><span data-stu-id="f313a-166">Select a condition, which when fulfilled will cause hello alert toofire</span></span>
6. <span data-ttu-id="f313a-167">5. lépésben válassza ki a hello feltétel küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="f313a-167">Select a threshold for hello condition in Step 5.</span></span> <span data-ttu-id="f313a-168">toobe teljesítése</span><span class="sxs-lookup"><span data-stu-id="f313a-168">toobe fulfilled</span></span>
7. <span data-ttu-id="f313a-169">Lépéseket 5 és 6 hello feltétel és a küszöbérték felett mely hello szolgáltatás ellenőrzi időszak kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f313a-169">Select a period over which hello monitoring service will check for hello condition and threshold in Steps 5 & 6</span></span>
8. <span data-ttu-id="f313a-170">Illessze be a hello előző szakaszából másolt hello webhook.</span><span class="sxs-lookup"><span data-stu-id="f313a-170">Paste in hello webhook you copied from hello previous section.</span></span>

![Riasztási tooVirtual gép 1 hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Riasztási tooVirtual gép 2 hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-2.png)

