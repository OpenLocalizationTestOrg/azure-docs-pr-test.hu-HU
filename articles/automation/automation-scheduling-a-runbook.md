---
title: "Azure Automation forgatókönyv aaaScheduling |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy Azure Automation ütemezési, hogy automatikusan vagy indítható el egy runbook egy adott időpontban egy ismétlődő ütemezés szerint."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: c215b7ff6aa200466f3be566facba3c0cffcc924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="b7487-103">Runbook ütemezése az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="b7487-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="b7487-104">egy runbook tooschedule az Azure Automation toostart egy megadott időpontban, csatolás tooone vagy további ütemezéseket.</span><span class="sxs-lookup"><span data-stu-id="b7487-104">tooschedule a runbook in Azure Automation toostart at a specified time, you link it tooone or more schedules.</span></span> <span data-ttu-id="b7487-105">Ütemezés szerint lehet konfigurált tooeither, egyszer futnak le, vagy a feladatról óránként vagy naponta ütemezés runbookokat hello a klasszikus Azure portálon lévő és a runbookokat hello Azure-portálon a, továbbá ütemezheti őket hello hét heti, havi, meghatározott napjain vagy napjain hello hónapban, vagy egy adott hello hónap napja.</span><span class="sxs-lookup"><span data-stu-id="b7487-105">A schedule can be configured tooeither run once or on a reoccurring hourly or daily schedule for runbooks in hello Azure classic portal and for runbooks in hello Azure portal,  you can additionally schedule them for weekly, monthly, specific days of hello week or days of hello month, or a particular day of hello month.</span></span>  <span data-ttu-id="b7487-106">Egy runbook csatolt toomultiple ütemezéseket, és ütemezés rendelkezhet több hozzá kapcsolt forgatókönyvből tooit.</span><span class="sxs-lookup"><span data-stu-id="b7487-106">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="b7487-107">Ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7487-107">Creating a schedule</span></span>
<span data-ttu-id="b7487-108">Hello Azure-portálon a runbookok új ütemtervet hozhat létre a klasszikus portálon hello, vagy a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="b7487-108">You can create a new schedule for runbooks in hello Azure portal, in hello classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="b7487-109">Akkor is hello lehetőséget egy új ütemezést, ha egy runbook tooa ütemezéshez hello klasszikus Azure vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b7487-109">You also have hello option of creating a new schedule when you link a runbook tooa schedule using hello Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="b7487-110">Ütemezés hozzárendelése egy runbookhoz, ha Automation hello modulok aktuális verziói hello tárol a fiókját, és csatolja őket toothat ütemezés.</span><span class="sxs-lookup"><span data-stu-id="b7487-110">When you associate a schedule with a runbook, Automation stores hello current versions of hello modules in your account and links them toothat schedule.</span></span>  <span data-ttu-id="b7487-111">Ez azt jelenti, hogy ha egy modul 1.0-s verziójával rendelkezett a fiókjában, amikor a feladatütemezés létrehozása, majd frissíti a hello modul tooversion 2.0, hello ütemezés továbbra is toouse 1.0.</span><span class="sxs-lookup"><span data-stu-id="b7487-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update hello module tooversion 2.0, hello schedule will continue toouse 1.0.</span></span>  <span data-ttu-id="b7487-112">A sorrend toouse hello frissített verziója egy új ütemezést kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="b7487-112">In order toouse hello updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a><span data-ttu-id="b7487-113">egy új ütemezést, a klasszikus Azure portálon hello toocreate</span><span class="sxs-lookup"><span data-stu-id="b7487-113">toocreate a new schedule in hello Azure classic portal</span></span>
1. <span data-ttu-id="b7487-114">A klasszikus Azure portálon hello válassza ki az Automation, és válassza majd hello automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="b7487-114">In hello Azure classic portal, select Automation and then then select hello name of an automation account.</span></span>
2. <span data-ttu-id="b7487-115">Jelölje be hello **eszközök** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7487-115">Select hello **Assets** tab.</span></span>
3. <span data-ttu-id="b7487-116">Hello ablak hello alul kattintson **beállítás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b7487-116">At hello bottom of hello window, click **Add Setting**.</span></span>
4. <span data-ttu-id="b7487-117">Kattintson a **ütemezés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="b7487-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="b7487-118">Adjon meg egy **neve** és opcionálisan egy **leírás** hello új schedule.your az ütemezés futtatásához **egyszer**, **óránkénti**, **Napi**, **heti**, vagy **havi**.</span><span class="sxs-lookup"><span data-stu-id="b7487-118">Type a **Name** and optionally a **Description** for hello new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="b7487-119">Adjon meg egy **kezdete** és egyéb beállítások hello típusú kiválasztott ütemezéstípustól függően.</span><span class="sxs-lookup"><span data-stu-id="b7487-119">Specify a **Start Time** and other options depending on hello type of schedule that you selected.</span></span>

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a><span data-ttu-id="b7487-120">toocreate egy új ütemezést a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7487-120">toocreate a new schedule in hello Azure portal</span></span>
1. <span data-ttu-id="b7487-121">Kattintson az automation-fiók, az Azure-portálon hello hello **eszközök** csempe tooopen hello **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7487-121">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="b7487-122">Kattintson a hello **ütemezések** csempe tooopen hello **ütemezések** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7487-122">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="b7487-123">Kattintson a **ütemezés hozzáadása** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="b7487-123">Click **Add a schedule** at hello top of hello blade.</span></span>
4. <span data-ttu-id="b7487-124">A hello **új ütemezés** panelen adjon meg egy **neve** és opcionálisan egy **leírás** hello új ütemezés.</span><span class="sxs-lookup"><span data-stu-id="b7487-124">On hello **New schedule** blade, type a **Name** and optionally a **Description** for hello new schedule.</span></span>
5. <span data-ttu-id="b7487-125">Válassza ki az e hello ütemezése egyszer, vagy feladatról ütemezés kiválasztásával **egyszer** vagy **ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="b7487-125">Select whether hello schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="b7487-126">Választásakor **egyszer** adjon meg egy **kezdési időpont** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b7487-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="b7487-127">Választásakor **ismétlődési**, adjon meg egy **kezdési időpont** és milyen gyakran hello runbook toorepeat - érdemes, az hello gyakoriságát **óra**, **nap**, **hét**, illetve ha **hónap**.</span><span class="sxs-lookup"><span data-stu-id="b7487-127">If you select **Recurrence**, specify a **Start time** and hello frequency for how often you want hello runbook toorepeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="b7487-128">Ha **hét** vagy **hónap** hello legördülő listából hello **ismétlődési beállítást** fog megjelenni hello panelen és kiválasztáskor, hello **ismétlődési a beállítás** panel számára jelenik meg, és hello hét napja, választhat, ha a kiválasztott **hét**.</span><span class="sxs-lookup"><span data-stu-id="b7487-128">If you select **week** or **month** from hello drop-down list, hello **Recurrence option** will appear in hello blade and upon selection, hello **Recurrence option** blade will be presented and you can select hello day of week if you selected **week**.</span></span>  <span data-ttu-id="b7487-129">Ha a kiválasztott **hónap**, szerint is választhat **hét napjai** vagy hello hónap adott napjaira hello naptár, és végül szeretné, hogy toorun azt a hello hónap utolsó napján hello, vagy nem, és kattintson a **OK** .</span><span class="sxs-lookup"><span data-stu-id="b7487-129">If you selected **month**, you can choose by **week days** or specific days of hello month on hello calendar and finally, do you want toorun it on hello last day of hello month or not and then click **OK**.</span></span>   

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="b7487-130">a Windows PowerShell használatával új ütemezés toocreate</span><span class="sxs-lookup"><span data-stu-id="b7487-130">toocreate a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="b7487-131">Használhatja a hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) parancsmag toocreate egy új ütemezést, az Azure Automationben klasszikus runbookok vagy [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) parancsmaggal runbookokat hello Azure a portál.</span><span class="sxs-lookup"><span data-stu-id="b7487-131">You can use hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet toocreate a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="b7487-132">Meg kell adnia hello kezdési idejét hello ütemezése és hello gyakorisága fusson.</span><span class="sxs-lookup"><span data-stu-id="b7487-132">You must specify hello start time for hello schedule and hello frequency it should run.</span></span>

<span data-ttu-id="b7487-133">a következő Példaparancsok hello megjelenítése hogyan toocreate egy új ütemezést, amely fut minden nap du. 3:30 2015. január 20 kezdődően az Azure szolgáltatásfelügyelet parancsmagjával.</span><span class="sxs-lookup"><span data-stu-id="b7487-133">hello following sample commands show how toocreate a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="b7487-134">hello következő minta parancsok bemutatja hogyan toocreate hello ütemezésének 15 és az Azure Resource Manager parancsmagjával havonta 30.</span><span class="sxs-lookup"><span data-stu-id="b7487-134">hello following sample commands shows how toocreate a schedule for hello 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-tooa-runbook"></a><span data-ttu-id="b7487-135">Egy ütemezés tooa runbook csatolása</span><span class="sxs-lookup"><span data-stu-id="b7487-135">Linking a schedule tooa runbook</span></span>
<span data-ttu-id="b7487-136">Egy runbook csatolt toomultiple ütemezéseket, és ütemezés rendelkezhet több hozzá kapcsolt forgatókönyvből tooit.</span><span class="sxs-lookup"><span data-stu-id="b7487-136">A runbook can be linked toomultiple schedules, and a schedule can have multiple runbooks linked tooit.</span></span> <span data-ttu-id="b7487-137">Ha a runbook paraméterekkel rendelkezik, majd is értékeket ad meg a számukra.</span><span class="sxs-lookup"><span data-stu-id="b7487-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="b7487-138">Adjon meg értéket minden kötelező paraméterhez, és előfordulhat, hogy adjon meg értékeket a választható paramétereket.</span><span class="sxs-lookup"><span data-stu-id="b7487-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="b7487-139">Ezeket az értékeket minden alkalommal, amikor az ütemezés szerint elindult hello runbook fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b7487-139">These values will be used each time hello runbook is started by this schedule.</span></span>  <span data-ttu-id="b7487-140">Csatolhat hello ugyanazon runbook tooanother ütemezést, és adja meg a különböző paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="b7487-140">You can attach hello same runbook tooanother schedule and specify different parameter values.</span></span>

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a><span data-ttu-id="b7487-141">a klasszikus Azure portálon hello ütemezés tooa runbookokhoz toolink</span><span class="sxs-lookup"><span data-stu-id="b7487-141">toolink a schedule tooa runbook with hello Azure classic portal</span></span>
1. <span data-ttu-id="b7487-142">Hello a klasszikus Azure portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.</span><span class="sxs-lookup"><span data-stu-id="b7487-142">In hello Azure classic portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="b7487-143">Jelölje be hello **Runbookok** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7487-143">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="b7487-144">Kattintson a hello runbook tooschedule hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b7487-144">Click on hello name of hello runbook tooschedule.</span></span>
4. <span data-ttu-id="b7487-145">Kattintson a hello **ütemezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="b7487-145">Click hello **Schedule** tab.</span></span>
5. <span data-ttu-id="b7487-146">Ha hello runbook nem jelenleg csatolt tooa ütemezést, akkor az aktiválási hello beállítás túl**tooa új ütemezés hivatkozás** vagy **tooan meglévő ütemezés hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="b7487-146">If hello runbook is not currently linked tooa schedule, then you will be given hello option too**Link tooa New Schedule** or **Link tooan Existing Schedule**.</span></span>  <span data-ttu-id="b7487-147">Ha hello a runbook jelenleg csatolt tooa ütemezést, kattintson a **hivatkozás** : hello alsó részén hello ablak tooaccess ezeket a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b7487-147">If hello runbook is currently linked tooa schedule, click **Link** at hello bottom of hello window tooaccess these options.</span></span>
6. <span data-ttu-id="b7487-148">Ha hello runbook paraméterekkel rendelkezik, felkéri ezek értékeinek megadására.</span><span class="sxs-lookup"><span data-stu-id="b7487-148">If hello runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a><span data-ttu-id="b7487-149">toolink ütemezés tooa runbookkal hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7487-149">toolink a schedule tooa runbook with hello Azure portal</span></span>
1. <span data-ttu-id="b7487-150">Kattintson az automation-fiók, az Azure-portálon hello hello **Runbookok** csempe tooopen hello **Runbookok** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7487-150">In hello Azure portal, from your automation account, click hello **Runbooks** tile tooopen hello **Runbooks** blade.</span></span>
2. <span data-ttu-id="b7487-151">Kattintson a hello runbook tooschedule hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b7487-151">Click on hello name of hello runbook tooschedule.</span></span>
3. <span data-ttu-id="b7487-152">Ha hello runbook nem jelenleg csatolt tooa ütemezést, majd hoz adott hello beállítás toocreate kell egy új ütemezést vagy meglévő ütemezés tooan hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b7487-152">If hello runbook is not currently linked tooa schedule, then you will be given hello option toocreate a new schedule or link tooan existing schedule.</span></span>  
4. <span data-ttu-id="b7487-153">Ha hello runbook paraméterekkel rendelkezik, kiválaszthatja a hello beállítást **(alapértelmezett: Azure) futtatási beállítások módosítása** és hello **paraméterek** panel oszlik be hello információ ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b7487-153">If hello runbook has parameters, you can select hello option **Modify run settings (Default:Azure)** and hello **Parameters** blade is presented where you can enter hello information accordingly.</span></span>  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a><span data-ttu-id="b7487-154">a Windows PowerShell-lel ütemezés tooa runbook toolink</span><span class="sxs-lookup"><span data-stu-id="b7487-154">toolink a schedule tooa runbook with Windows PowerShell</span></span>
<span data-ttu-id="b7487-155">Használhatja a hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink ütemezés tooa klasszikus runbook vagy [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) parancsmaggal runbookokat hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="b7487-155">You can use hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink a schedule tooa classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in hello Azure portal.</span></span>  <span data-ttu-id="b7487-156">Hello paraméterek paraméterrel megadhatja hello gyermekrunbook paramétereinek értékeit.</span><span class="sxs-lookup"><span data-stu-id="b7487-156">You can specify values for hello runbook’s parameters with hello Parameters parameter.</span></span> <span data-ttu-id="b7487-157">Lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) a paraméterértékek meghatározásáról a további információt.</span><span class="sxs-lookup"><span data-stu-id="b7487-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="b7487-158">hello következő minta parancsok megjelenítése hogyan toolink egy Azure szolgáltatásfelügyeleti parancsmaggal paraméterekkel ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b7487-158">hello following sample commands show how toolink a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="b7487-159">hello következő minta parancsok megjelenítése hogyan toolink az Azure Resource Manager parancsmaggal paraméterekkel ütemezés tooa runbook.</span><span class="sxs-lookup"><span data-stu-id="b7487-159">hello following sample commands show how toolink a schedule tooa runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="b7487-160">Ütemezés letiltása</span><span class="sxs-lookup"><span data-stu-id="b7487-160">Disabling a schedule</span></span>
<span data-ttu-id="b7487-161">Ha letilt egy ütemezést, minden hozzá kapcsolt forgatókönyvből tooit nem fog működni, hogy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b7487-161">When you disable a schedule, any runbooks linked tooit will no longer run on that schedule.</span></span> <span data-ttu-id="b7487-162">Manuálisan ütemezésének letiltása, vagy az ütemezések gyakorisággal lejárati idő beállítása a létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b7487-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="b7487-163">Hello lejárati idő elérésekor hello ütemezés letiltásra kerül.</span><span class="sxs-lookup"><span data-stu-id="b7487-163">When hello expiration time is reached, hello schedule will be disabled.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a><span data-ttu-id="b7487-164">egy ütemezés, a klasszikus Azure portálon hello toodisable</span><span class="sxs-lookup"><span data-stu-id="b7487-164">toodisable a schedule from hello Azure classic portal</span></span>
<span data-ttu-id="b7487-165">Letilthatja a klasszikus Azure portálon hello ütemezés részleteit megjelenítő oldalon hello ütemezés hello ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b7487-165">You can disable a schedule in hello Azure classic portal from hello Schedule Details page for hello schedule.</span></span>

1. <span data-ttu-id="b7487-166">A klasszikus Azure portálon hello válassza ki az Automation, és kattintson majd hello automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="b7487-166">In hello Azure classic portal, select Automation and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="b7487-167">Hello eszközök lapon válassza ki.</span><span class="sxs-lookup"><span data-stu-id="b7487-167">Select hello Assets tab.</span></span>
3. <span data-ttu-id="b7487-168">Kattintson egy ütemezés tooopen hello nevét annak információs lapját.</span><span class="sxs-lookup"><span data-stu-id="b7487-168">Click hello name of a schedule tooopen its detail page.</span></span>
4. <span data-ttu-id="b7487-169">Változás **engedélyezve** túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="b7487-169">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a><span data-ttu-id="b7487-170">toodisable egy ütemezéshez hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7487-170">toodisable a schedule from hello Azure portal</span></span>
1. <span data-ttu-id="b7487-171">Kattintson az automation-fiók, az Azure-portálon hello hello **eszközök** csempe tooopen hello **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7487-171">In hello Azure portal, from your automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
2. <span data-ttu-id="b7487-172">Kattintson a hello **ütemezések** csempe tooopen hello **ütemezések** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7487-172">Click hello **Schedules** tile tooopen hello **Schedules** blade.</span></span>
3. <span data-ttu-id="b7487-173">Kattintson egy ütemezés tooopen hello részleteit megjelenítő panelen hello nevére.</span><span class="sxs-lookup"><span data-stu-id="b7487-173">Click hello name of a schedule tooopen hello details blade.</span></span>
4. <span data-ttu-id="b7487-174">Változás **engedélyezve** túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="b7487-174">Change **Enabled** too**No**.</span></span>

### <a name="toodisable-a-schedule-with-windows-powershell"></a><span data-ttu-id="b7487-175">a Windows PowerShell-lel ütemezés toodisable</span><span class="sxs-lookup"><span data-stu-id="b7487-175">toodisable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="b7487-176">Használhatja a hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) parancsmag toochange hello klasszikus runbook meglévő ütemezés tulajdonságainak vagy [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) parancsmaggal runbookokat hello Azure a portál.</span><span class="sxs-lookup"><span data-stu-id="b7487-176">You can use hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet toochange hello properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in hello Azure portal.</span></span> <span data-ttu-id="b7487-177">toodisable hello ütemezése, adja meg **hamis** a hello **IsEnabled** paraméter.</span><span class="sxs-lookup"><span data-stu-id="b7487-177">toodisable hello schedule, specify **false** for hello **IsEnabled** parameter.</span></span>

<span data-ttu-id="b7487-178">a következő Példaparancsok hello bemutatják, hogyan egy ütemezés használatával toodisable hello Azure szolgáltatásfelügyelet parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b7487-178">hello following sample commands show how toodisable a schedule using hello Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="b7487-179">hello következő minta parancsok megjelenítése hogyan toodisable annak ütemezését, hogy egy runbook az Azure Resource Manager parancsmagjával.</span><span class="sxs-lookup"><span data-stu-id="b7487-179">hello following sample commands show how toodisable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="b7487-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7487-180">Next steps</span></span>
* <span data-ttu-id="b7487-181">toolearn több ütemezések használatával kapcsolatban lásd: [Azure Automation szolgáltatásbeli ütemezési eszközök](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="b7487-181">toolearn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="b7487-182">Lásd az Azure Automation runbookjai használatába tooget [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="b7487-182">tooget started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

