---
title: "Azure Automation függőleges méretezése a Windows virtuális gépek használata |} Microsoft Docs"
description: "Függőleges méretezhető figyelési riasztásokhoz adható az Azure Automation szolgáltatásban válaszul egy Windows rendszerű virtuális gép"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea5169c1a95f00e78ae3f5f177812466eb7a0deb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a><span data-ttu-id="94971-103">Függőleges méretezése a Windows-alapú virtuális gépek az Azure Automation szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="94971-103">Vertically scale Windows VMs with Azure Automation</span></span>

<span data-ttu-id="94971-104">Függőleges skálázás bővítése vagy csökkentése az erőforrásokat, a munkaterhelés válaszul gép során a rendszer.</span><span class="sxs-lookup"><span data-stu-id="94971-104">Vertical scaling is the process of increasing or decreasing the resources of a machine in response to the workload.</span></span> <span data-ttu-id="94971-105">Az Azure-ban ehhez a virtuális gép méretének módosításával.</span><span class="sxs-lookup"><span data-stu-id="94971-105">In Azure this can be accomplished by changing the size of the Virtual Machine.</span></span> <span data-ttu-id="94971-106">Ez segítheti a következő esetekben</span><span class="sxs-lookup"><span data-stu-id="94971-106">This can help in the following scenarios</span></span>

* <span data-ttu-id="94971-107">Ha a virtuális gép nem gyakran használják, méretét, a havi költségek csökkentése érdekében kis méretben le</span><span class="sxs-lookup"><span data-stu-id="94971-107">If the Virtual Machine is not being used frequently, you can resize it down to a smaller size to reduce your monthly costs</span></span>
* <span data-ttu-id="94971-108">Ha a virtuális gép egy csúcsterhelés lát, azt is átméretezhetők nagyobb méretűre a kapacitás növelése</span><span class="sxs-lookup"><span data-stu-id="94971-108">If the Virtual Machine is seeing a peak load, it can be resized to a larger size to increase its capacity</span></span>

<span data-ttu-id="94971-109">Ehhez a lépéseket a Vázlat van, mint alatt</span><span class="sxs-lookup"><span data-stu-id="94971-109">The outline for the steps to accomplish this is as below</span></span>

1. <span data-ttu-id="94971-110">A virtuális gépek eléréséhez Azure Automation beállítása</span><span class="sxs-lookup"><span data-stu-id="94971-110">Setup Azure Automation to access your Virtual Machines</span></span>
2. <span data-ttu-id="94971-111">Az Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="94971-111">Import the Azure Automation Vertical Scale runbooks into your subscription</span></span>
3. <span data-ttu-id="94971-112">A webhook hozzáadása a runbookhoz</span><span class="sxs-lookup"><span data-stu-id="94971-112">Add a webhook to your runbook</span></span>
4. <span data-ttu-id="94971-113">Adjon hozzá egy riasztást a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="94971-113">Add an alert to your Virtual Machine</span></span>

> [!NOTE]
> <span data-ttu-id="94971-114">Az első virtuális gépen, az azt is méretezhető, méretek mérete miatt előfordulhat, hogy korlátozva lesz, mert a rendelkezésre állási a fürt más méretű aktuális virtuális gép üzemel.</span><span class="sxs-lookup"><span data-stu-id="94971-114">Because of the size of the first Virtual Machine, the sizes it can be scaled to, may be limited due to the availability of the other sizes in the cluster current Virtual Machine is deployed in.</span></span> <span data-ttu-id="94971-115">A közzétett automatizálási runbookok a cikk ezt használja azt ebben az esetben mi gondoskodunk, és csak méretezése belül a virtuális gép mérete párok alatt.</span><span class="sxs-lookup"><span data-stu-id="94971-115">In the published automation runbooks used in this article we take care of this case and only scale within the below VM size pairs.</span></span> <span data-ttu-id="94971-116">Ez azt jelenti, hogy, hogy egy Standard_D1v2 virtuális gép lesz hirtelen nem standard szintű, G5 akár méretezhető vagy Basic_A0 méretezhető.</span><span class="sxs-lookup"><span data-stu-id="94971-116">This means that a Standard_D1v2 Virtual Machine will not suddenly be scaled up to Standard_G5 or scaled down to Basic_A0.</span></span>
> 
> | <span data-ttu-id="94971-117">Virtuálisgép-méretek pár skálázás</span><span class="sxs-lookup"><span data-stu-id="94971-117">VM sizes scaling pair</span></span> |  |
> | --- | --- |
> | <span data-ttu-id="94971-118">Basic_A0</span><span class="sxs-lookup"><span data-stu-id="94971-118">Basic_A0</span></span> |<span data-ttu-id="94971-119">Basic_A4</span><span class="sxs-lookup"><span data-stu-id="94971-119">Basic_A4</span></span> |
> | <span data-ttu-id="94971-120">Standard_A0</span><span class="sxs-lookup"><span data-stu-id="94971-120">Standard_A0</span></span> |<span data-ttu-id="94971-121">Standard_A4</span><span class="sxs-lookup"><span data-stu-id="94971-121">Standard_A4</span></span> |
> | <span data-ttu-id="94971-122">Standard_A5</span><span class="sxs-lookup"><span data-stu-id="94971-122">Standard_A5</span></span> |<span data-ttu-id="94971-123">Standard_A7</span><span class="sxs-lookup"><span data-stu-id="94971-123">Standard_A7</span></span> |
> | <span data-ttu-id="94971-124">Standard_A8</span><span class="sxs-lookup"><span data-stu-id="94971-124">Standard_A8</span></span> |<span data-ttu-id="94971-125">Standard_A9</span><span class="sxs-lookup"><span data-stu-id="94971-125">Standard_A9</span></span> |
> | <span data-ttu-id="94971-126">Standard_A10</span><span class="sxs-lookup"><span data-stu-id="94971-126">Standard_A10</span></span> |<span data-ttu-id="94971-127">Standard_A11</span><span class="sxs-lookup"><span data-stu-id="94971-127">Standard_A11</span></span> |
> | <span data-ttu-id="94971-128">Standard_D1</span><span class="sxs-lookup"><span data-stu-id="94971-128">Standard_D1</span></span> |<span data-ttu-id="94971-129">Standard_D4</span><span class="sxs-lookup"><span data-stu-id="94971-129">Standard_D4</span></span> |
> | <span data-ttu-id="94971-130">Standard_D11</span><span class="sxs-lookup"><span data-stu-id="94971-130">Standard_D11</span></span> |<span data-ttu-id="94971-131">Standard_D14</span><span class="sxs-lookup"><span data-stu-id="94971-131">Standard_D14</span></span> |
> | <span data-ttu-id="94971-132">Standard_DS1</span><span class="sxs-lookup"><span data-stu-id="94971-132">Standard_DS1</span></span> |<span data-ttu-id="94971-133">Standard_DS4</span><span class="sxs-lookup"><span data-stu-id="94971-133">Standard_DS4</span></span> |
> | <span data-ttu-id="94971-134">Standard_DS11</span><span class="sxs-lookup"><span data-stu-id="94971-134">Standard_DS11</span></span> |<span data-ttu-id="94971-135">Standard_DS14</span><span class="sxs-lookup"><span data-stu-id="94971-135">Standard_DS14</span></span> |
> | <span data-ttu-id="94971-136">Standard_D1v2</span><span class="sxs-lookup"><span data-stu-id="94971-136">Standard_D1v2</span></span> |<span data-ttu-id="94971-137">Standard_D5v2</span><span class="sxs-lookup"><span data-stu-id="94971-137">Standard_D5v2</span></span> |
> | <span data-ttu-id="94971-138">Standard_D11v2</span><span class="sxs-lookup"><span data-stu-id="94971-138">Standard_D11v2</span></span> |<span data-ttu-id="94971-139">Standard_D14v2</span><span class="sxs-lookup"><span data-stu-id="94971-139">Standard_D14v2</span></span> |
> | <span data-ttu-id="94971-140">Standard_G1</span><span class="sxs-lookup"><span data-stu-id="94971-140">Standard_G1</span></span> |<span data-ttu-id="94971-141">Standard szintű, G5</span><span class="sxs-lookup"><span data-stu-id="94971-141">Standard_G5</span></span> |
> | <span data-ttu-id="94971-142">Standard_GS1</span><span class="sxs-lookup"><span data-stu-id="94971-142">Standard_GS1</span></span> |<span data-ttu-id="94971-143">Standard_GS5</span><span class="sxs-lookup"><span data-stu-id="94971-143">Standard_GS5</span></span> |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a><span data-ttu-id="94971-144">A virtuális gépek eléréséhez Azure Automation beállítása</span><span class="sxs-lookup"><span data-stu-id="94971-144">Setup Azure Automation to access your Virtual Machines</span></span>
<span data-ttu-id="94971-145">Először is végre kell hajtani, hozzon létre egy Azure Automation-fiók, amely a virtuális gépek méretezési használt runbookok tárolni fogja.</span><span class="sxs-lookup"><span data-stu-id="94971-145">The first thing you need to do is create an Azure Automation account that will host the runbooks used to scale a Virtual Machine.</span></span> <span data-ttu-id="94971-146">Az Automation szolgáltatás új a "Futtatás mint fiók" szolgáltatás, amely lehetővé teszi a beállítás a szolgáltatás egyszerű fel automatikusan a runbookok futtatásáért a felhasználó nevében nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="94971-146">Recently the Automation service introduced the "Run As account" feature which makes setting up the Service Principal for automatically running the runbooks on the user's behalf very easy.</span></span> <span data-ttu-id="94971-147">További információt az alábbi cikkben:</span><span class="sxs-lookup"><span data-stu-id="94971-147">You can read more about this in the article below:</span></span>

* [<span data-ttu-id="94971-148">Runbookok hitelesítése Azure-beli futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="94971-148">Authenticate Runbooks with Azure Run As account</span></span>](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a><span data-ttu-id="94971-149">Az Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="94971-149">Import the Azure Automation Vertical Scale runbooks into your subscription</span></span>
<span data-ttu-id="94971-150">A runbookok, amelyek szükségesek a virtuális gép függőleges skálázás vannak már közzé van téve az Azure Automation-Runbook katalógus.</span><span class="sxs-lookup"><span data-stu-id="94971-150">The runbooks that are needed for Vertically Scaling your Virtual Machine are already published in the Azure Automation Runbook Gallery.</span></span> <span data-ttu-id="94971-151">Szüksége lesz az előfizetés importálja azokat.</span><span class="sxs-lookup"><span data-stu-id="94971-151">You will need to import them into your subscription.</span></span> <span data-ttu-id="94971-152">Megismerheti a runbookok importálása a következő cikk elolvasása.</span><span class="sxs-lookup"><span data-stu-id="94971-152">You can learn how to import runbooks by reading the following article.</span></span>

* [<span data-ttu-id="94971-153">Az Azure Automation forgatókönyv- és minták</span><span class="sxs-lookup"><span data-stu-id="94971-153">Runbook and module galleries for Azure Automation</span></span>](../../automation/automation-runbook-gallery.md)

<span data-ttu-id="94971-154">A runbookokat, importálandók kell az alábbi képen látható</span><span class="sxs-lookup"><span data-stu-id="94971-154">The runbooks that need to be imported are shown in the image below</span></span>

![Runbookok importálása](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a><span data-ttu-id="94971-156">A webhook hozzáadása a runbookhoz</span><span class="sxs-lookup"><span data-stu-id="94971-156">Add a webhook to your runbook</span></span>
<span data-ttu-id="94971-157">Miután importált a runbookok kell hozzáadása egy webhook a runbookhoz úgy is elindítható a virtuális gép riasztások alapján.</span><span class="sxs-lookup"><span data-stu-id="94971-157">Once you've imported the runbooks you'll need to add a webhook to the runbook so it can be triggered by an alert from a Virtual Machine.</span></span> <span data-ttu-id="94971-158">A webhook létrehozása a runbook részleteit itt olvasható</span><span class="sxs-lookup"><span data-stu-id="94971-158">The details of creating a webhook for your Runbook can be read here</span></span>

* [<span data-ttu-id="94971-159">Azure Automation-webhook</span><span class="sxs-lookup"><span data-stu-id="94971-159">Azure Automation webhooks</span></span>](../../automation/automation-webhooks.md)

<span data-ttu-id="94971-160">Ellenőrizze, hogy a webhook másolja át a webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban előtt.</span><span class="sxs-lookup"><span data-stu-id="94971-160">Make sure you copy the webhook before closing the webhook dialog as you will need this in the next section.</span></span>

## <a name="add-an-alert-to-your-virtual-machine"></a><span data-ttu-id="94971-161">Adjon hozzá egy riasztást a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="94971-161">Add an alert to your Virtual Machine</span></span>
1. <span data-ttu-id="94971-162">Válassza ki a virtuális gép beállításait</span><span class="sxs-lookup"><span data-stu-id="94971-162">Select Virtual Machine settings</span></span>
2. <span data-ttu-id="94971-163">Válassza ki a "Riasztási szabályok"</span><span class="sxs-lookup"><span data-stu-id="94971-163">Select "Alert rules"</span></span>
3. <span data-ttu-id="94971-164">Válassza a "Riasztás hozzáadása"</span><span class="sxs-lookup"><span data-stu-id="94971-164">Select "Add alert"</span></span>
4. <span data-ttu-id="94971-165">Jelöljön ki egy metrikát az érvényesítést a riasztás a</span><span class="sxs-lookup"><span data-stu-id="94971-165">Select a metric to fire the alert on</span></span>
5. <span data-ttu-id="94971-166">Válassza ki azt a feltételt, amely teljesítése lesz miatt a riasztás az érvényesítést</span><span class="sxs-lookup"><span data-stu-id="94971-166">Select a condition, which when fulfilled will cause the alert to fire</span></span>
6. <span data-ttu-id="94971-167">Válassza ki a feltétel küszöbértéket az 5. lépés.</span><span class="sxs-lookup"><span data-stu-id="94971-167">Select a threshold for the condition in Step 5.</span></span> <span data-ttu-id="94971-168">teljesítendő</span><span class="sxs-lookup"><span data-stu-id="94971-168">to be fulfilled</span></span>
7. <span data-ttu-id="94971-169">A feltétel és a küszöbérték felett, amelyek a figyelési szolgáltatás ellenőrzi időszak kiválasztása lépéseket 5 és 6</span><span class="sxs-lookup"><span data-stu-id="94971-169">Select a period over which the monitoring service will check for the condition and threshold in Steps 5 & 6</span></span>
8. <span data-ttu-id="94971-170">Illessze be a webhook előző szakaszából másolt.</span><span class="sxs-lookup"><span data-stu-id="94971-170">Paste in the webhook you copied from the previous section.</span></span>

![1 virtuális gép értesítés hozzáadása](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Riasztás hozzáadása a virtuális gép 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

