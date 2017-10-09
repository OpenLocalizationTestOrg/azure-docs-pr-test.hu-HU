---
title: "Azure Automation-fiók a Naplóelemzési aaaUnlink |} Microsoft Docs"
description: "A cikkben megtudhatja, hogyan toounlink az Azure Automation szolgáltatásbeli fiók az OMS-munkaterület áttekintését."
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
ms.openlocfilehash: 3ec0745673f6a872538d33023a7a1d2bb1d18ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="dccf1-103">Hogyan toounlink az automatizálási fiók egy Naplóelemzési munkaterületet</span><span class="sxs-lookup"><span data-stu-id="dccf1-103">How toounlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="dccf1-104">Azure Automation szolgáltatásbeli Naplóelemzési toonot csak támogatja az előrejelzéses figyeléssel a runbook-feladatok az Automation-fiók összes integrálható, de is szükség a következő megoldások Naplóelemzési függő hello importálásakor:</span><span class="sxs-lookup"><span data-stu-id="dccf1-104">Azure Automation integrates with Log Analytics toonot only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import hello following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="dccf1-105">Frissítéskezelés</span><span class="sxs-lookup"><span data-stu-id="dccf1-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="dccf1-106">Változáskövetés</span><span class="sxs-lookup"><span data-stu-id="dccf1-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="dccf1-107">Indítása/leállítása virtuális gépek munkaidőn kívüli során</span><span class="sxs-lookup"><span data-stu-id="dccf1-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="dccf1-108">Ha úgy dönt, hogy már nem kívánja toointegrate az Automation-fiók a Naplóelemzési, megszüntetheti a fiók közvetlenül a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="dccf1-108">If you decide you no longer wish toointegrate your Automation account with Log Analytics, you can unlink your account directly from hello Azure portal.</span></span>  <span data-ttu-id="dccf1-109">Mielőtt továbblép, akkor először a korábban említett tooremove hello megoldások, ellenkező esetben ez a folyamat nem lesz lehetséges a folytatás.</span><span class="sxs-lookup"><span data-stu-id="dccf1-109">Before you proceed, you will first need tooremove hello solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="dccf1-110">Tekintse át a hello témakör hello adott megoldás toounderstand hello lépéseit importálta tooremove azt.</span><span class="sxs-lookup"><span data-stu-id="dccf1-110">Review hello topic for hello particular solution you have imported toounderstand hello steps required tooremove it.</span></span>  

<span data-ttu-id="dccf1-111">Ezek a megoldások eltávolítása után végezheti el a következő lépéseket toounlink hello az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="dccf1-111">After you remove these solutions you can perform hello following steps toounlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="dccf1-112">Munkaterület választható</span><span class="sxs-lookup"><span data-stu-id="dccf1-112">Unlink workspace</span></span>

1. <span data-ttu-id="dccf1-113">Hello Azure-portálon, nyissa meg az Automation-fiók, és hello Automation-fiók panelen hello-fiók panelen válassza ki a **munkaterület választható**.</span><span class="sxs-lookup"><span data-stu-id="dccf1-113">From hello Azure portal, open your Automation account, and in hello Automation account blade, in hello account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="dccf1-114">![Munkaterület beállítás leválasztása](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="dccf1-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="dccf1-115">Hello szétkapcsolás munkaterület paneljén kattintson **worksapce leválasztása**.</span><span class="sxs-lookup"><span data-stu-id="dccf1-115">On hello Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="dccf1-116">![Panel munkaterület választható](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="dccf1-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="dccf1-117">A kérdés ellenőrzése tooproceed kívánja fog kapni.</span><span class="sxs-lookup"><span data-stu-id="dccf1-117">You will receive a prompt verifying you wish tooproceed.</span></span><br><br>
3. <span data-ttu-id="dccf1-118">Azure Automation próbál toounlink hello fiók Naplóelemzési munkaterületet, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="dccf1-118">While Azure Automation attempts toounlink hello account your Log Analytics workspace, you can track hello progress under **Notifications** from hello menu.</span></span>

<span data-ttu-id="dccf1-119">Ha hello frissítés felügyeleti megoldás, opcionálisan érdemes lehet tooremove hello hello megoldás eltávolítása után már nem szükséges elemeket a következő.</span><span class="sxs-lookup"><span data-stu-id="dccf1-119">If you used hello Update Management solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="dccf1-120">Ütemezés frissítésére.</span><span class="sxs-lookup"><span data-stu-id="dccf1-120">Update schedules.</span></span>  <span data-ttu-id="dccf1-121">Minden egyes lesz melyek nevei egyeznek hello központi telepítést hozott létre)</span><span class="sxs-lookup"><span data-stu-id="dccf1-121">Each will have names that match hello update deployments you created)</span></span>

* <span data-ttu-id="dccf1-122">Hibrid feldolgozó csoportja hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="dccf1-122">Hybrid worker groups created for hello solution.</span></span>  <span data-ttu-id="dccf1-123">Minden egyes lesznek elnevezve hasonlóképpen túl machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="dccf1-123">Each will be named similarly too machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="dccf1-124">Ha munkaidőn kívüli megoldás során használt hello indítása/leállítása virtuális gépeket, opcionálisan érdemes lehet tooremove hello hello megoldás eltávolítása után már nem szükséges elemeket a következő.</span><span class="sxs-lookup"><span data-stu-id="dccf1-124">If you used hello Start/Stop VMs during off-hours solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="dccf1-125">Elindítása és leállítása a virtuális gép runbook ütemezése</span><span class="sxs-lookup"><span data-stu-id="dccf1-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="dccf1-126">Virtuális gép runbookok elindítása és leállítása</span><span class="sxs-lookup"><span data-stu-id="dccf1-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="dccf1-127">Változók</span><span class="sxs-lookup"><span data-stu-id="dccf1-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="dccf1-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dccf1-128">Next steps</span></span>

<span data-ttu-id="dccf1-129">tooreconfigure az Automation-fiók toointegrate az OMS szolgáltatáshoz, lásd: [feladat állapotát és a feladat adatfolyam továbbítása Automation tooLog szolgáltatás (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="dccf1-129">tooreconfigure your Automation account toointegrate with OMS Log Analytics, see [Forward job status and job streams from Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 
