---
title: "Azure Automation-fiók Log Analyticshez való leválasztása |} Microsoft Docs"
description: "Ez a cikk áttekintést az Azure Automation-fiók az OMS-munkaterület választható módjáról."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 02/07/2017
ms.author: magoedte
ms.openlocfilehash: 56b09c2cfc14813b5efcb364c580787fec1bf639
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="e0b2a-103">Az Automation-fiók a Naplóelemzési munkaterület választható hogyan</span><span class="sxs-lookup"><span data-stu-id="e0b2a-103">How to unlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="e0b2a-104">Azure Automation szolgáltatásbeli integrálható a Log Analyticshez nem csak a proaktív figyeléshez, a runbook-feladatok az Automation-fiók összes támogatására, de is szükség a Naplóelemzési függő következő megoldások importálásakor:</span><span class="sxs-lookup"><span data-stu-id="e0b2a-104">Azure Automation integrates with Log Analytics to not only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import the following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="e0b2a-105">Frissítéskezelés</span><span class="sxs-lookup"><span data-stu-id="e0b2a-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="e0b2a-106">Változáskövetés</span><span class="sxs-lookup"><span data-stu-id="e0b2a-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="e0b2a-107">Indítása/leállítása virtuális gépek munkaidőn kívüli során</span><span class="sxs-lookup"><span data-stu-id="e0b2a-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="e0b2a-108">Ha úgy dönt, hogy már nem szeretne az Automation-fiók integrálása Naplóelemzési, megszüntetheti a fiók közvetlenül az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-108">If you decide you no longer wish to integrate your Automation account with Log Analytics, you can unlink your account directly from the Azure portal.</span></span>  <span data-ttu-id="e0b2a-109">Mielőtt továbblépne, akkor először a fentebb említett megoldások eltávolítása, ellenkező esetben ez a folyamat nem lesz lehetséges a folytatás.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-109">Before you proceed, you will first need to remove the solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="e0b2a-110">Tekintse át a témakör az adott megoldás importálását megérteni, hogy eltávolítsa a szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-110">Review the topic for the particular solution you have imported to understand the steps required to remove it.</span></span>  

<span data-ttu-id="e0b2a-111">Ezek a megoldások eltávolítása után az Automation-fiók leválasztása a következő lépésekkel végezheti el.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-111">After you remove these solutions you can perform the following steps to unlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="e0b2a-112">Munkaterület választható</span><span class="sxs-lookup"><span data-stu-id="e0b2a-112">Unlink workspace</span></span>

1. <span data-ttu-id="e0b2a-113">Azure-portálról, nyissa meg az Automation-fiók, és az Automation-fiók panelen, a fiók panelen válassza ki a **munkaterület választható**.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-113">From the Azure portal, open your Automation account, and in the Automation account blade, in the account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="e0b2a-114">![Munkaterület beállítás leválasztása](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="e0b2a-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="e0b2a-115">Szétkapcsolás munkaterület paneljén kattintson **worksapce leválasztása**.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-115">On the Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="e0b2a-116">![Panel munkaterület választható](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="e0b2a-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="e0b2a-117">A rendszer felkéri, hogy erősítse meg, valóban folytani kívánja-e.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-117">You will receive a prompt verifying you wish to proceed.</span></span><br><br>
3. <span data-ttu-id="e0b2a-118">Azure Automation kísérli meg a fiók a Naplóelemzési munkaterület választható, amíg a folyamat állapotát követheti **értesítések** a menüből.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-118">While Azure Automation attempts to unlink the account your Log Analytics workspace, you can track the progress under **Notifications** from the menu.</span></span>

<span data-ttu-id="e0b2a-119">Ha a frissítés felügyeleti megoldás, opcionálisan előfordulhat, hogy szeretné eltávolítani a következő elemek, a megoldás eltávolítása után már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-119">If you used the Update Management solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="e0b2a-120">Ütemezés frissítésére.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-120">Update schedules.</span></span>  <span data-ttu-id="e0b2a-121">Minden egyes lesz melyek nevei egyeznek a létrehozott központi telepítések)</span><span class="sxs-lookup"><span data-stu-id="e0b2a-121">Each will have names that match the update deployments you created)</span></span>

* <span data-ttu-id="e0b2a-122">Hibrid dolgozó csoportok létrehozása a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-122">Hybrid worker groups created for the solution.</span></span>  <span data-ttu-id="e0b2a-123">Minden egyes lesznek elnevezve hasonlóan a machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="e0b2a-123">Each will be named similarly to  machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="e0b2a-124">Munkaidőn kívüli megoldás során használt virtuális gépek indítása/leállítása, ha szükség lehet, hogy szeretné eltávolítani a következő elemek, a megoldás eltávolítása után már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e0b2a-124">If you used the Start/Stop VMs during off-hours solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="e0b2a-125">Elindítása és leállítása a virtuális gép runbook ütemezése</span><span class="sxs-lookup"><span data-stu-id="e0b2a-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="e0b2a-126">Virtuális gép runbookok elindítása és leállítása</span><span class="sxs-lookup"><span data-stu-id="e0b2a-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="e0b2a-127">Változók</span><span class="sxs-lookup"><span data-stu-id="e0b2a-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="e0b2a-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0b2a-128">Next steps</span></span>

<span data-ttu-id="e0b2a-129">Konfigurálja újra az Automation-fiók OMS Naplóelemzési integrálása, lásd: [továbbítása feladat állapotát és a feladat adatfolyamok Automation való Naplóelemzés (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="e0b2a-129">To reconfigure your Automation account to integrate with OMS Log Analytics, see [Forward job status and job streams from Automation to Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 