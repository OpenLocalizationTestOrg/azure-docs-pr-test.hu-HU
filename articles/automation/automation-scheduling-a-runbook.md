---
title: "Az Azure Automationben runbook ütemezése |} Microsoft Docs"
description: "Ismerteti, hogyan ütemezés létrehozása az Azure Automationben, így képes automatikusan elindít egy runbookot egy adott időpontban vagy egy ismétlődő ütemezés szerint."
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
ms.openlocfilehash: 52f1d55f141bb1b3948e3b7039cfc131a5e407b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a><span data-ttu-id="78040-103">Runbook ütemezése az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="78040-103">Scheduling a runbook in Azure Automation</span></span>
<span data-ttu-id="78040-104">A megadott időben elindítani Azure Automation forgatókönyv ütemezése, csatolható egy vagy több ütemezés.</span><span class="sxs-lookup"><span data-stu-id="78040-104">To schedule a runbook in Azure Automation to start at a specified time, you link it to one or more schedules.</span></span> <span data-ttu-id="78040-105">Ütemezés beállítható úgy, hogy a runbookok a klasszikus Azure portálon, és az Azure-portálon a runbookok egyszeri és egy ismétlődés óránkénti futtatási vagy napi ütemezés, továbbá ütemezheti őket heti, havi, meghatározott napokat a hét vagy a mont napjain h, vagy a hónap adott napja.</span><span class="sxs-lookup"><span data-stu-id="78040-105">A schedule can be configured to either run once or on a reoccurring hourly or daily schedule for runbooks in the Azure classic portal and for runbooks in the Azure portal,  you can additionally schedule them for weekly, monthly, specific days of the week or days of the month, or a particular day of the month.</span></span>  <span data-ttu-id="78040-106">Egy runbook több ütemezéssel is lehet társítani, és egy ütemezés szerint lehet kapcsolni több runbook.</span><span class="sxs-lookup"><span data-stu-id="78040-106">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span>

## <a name="creating-a-schedule"></a><span data-ttu-id="78040-107">Ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="78040-107">Creating a schedule</span></span>
<span data-ttu-id="78040-108">A runbookok új ütemtervet hozhat létre az Azure portálon, a klasszikus portálon, vagy a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="78040-108">You can create a new schedule for runbooks in the Azure portal, in the classic portal, or with Windows PowerShell.</span></span> <span data-ttu-id="78040-109">Új ütemezés létrehozására, ha egy runbook egy ütemezést az Azure klasszikus vagy az Azure portál használatával is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="78040-109">You also have the option of creating a new schedule when you link a runbook to a schedule using the Azure classic or Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="78040-110">Ütemezés hozzárendelése egy runbookhoz, ha Automation tárolja a fiókját a modulok aktuális verziója, és csatolja őket, hogy az ütemezést.</span><span class="sxs-lookup"><span data-stu-id="78040-110">When you associate a schedule with a runbook, Automation stores the current versions of the modules in your account and links them to that schedule.</span></span>  <span data-ttu-id="78040-111">Ez azt jelenti, hogy ha egy modul 1.0-s verzió fiókjában ütemezés létrehozása, és a 2.0-s verzióját frissíti a modul, az ütemezés továbbra is 1.0 használja.</span><span class="sxs-lookup"><span data-stu-id="78040-111">This means that if you had a module with version 1.0 in your account when you created a schedule and then update the module to version 2.0, the schedule will continue to use 1.0.</span></span>  <span data-ttu-id="78040-112">A frissített verziója használatához létre kell hoznia egy új ütemezést.</span><span class="sxs-lookup"><span data-stu-id="78040-112">In order to use the updated module version, you must create a new schedule.</span></span> 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a><span data-ttu-id="78040-113">Új ütemezés létrehozása a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="78040-113">To create a new schedule in the Azure classic portal</span></span>
1. <span data-ttu-id="78040-114">A klasszikus Azure portálon válassza ki az Automation, és válassza majd automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="78040-114">In the Azure classic portal, select Automation and then then select the name of an automation account.</span></span>
2. <span data-ttu-id="78040-115">Válassza ki a **eszközök** fülre.</span><span class="sxs-lookup"><span data-stu-id="78040-115">Select the **Assets** tab.</span></span>
3. <span data-ttu-id="78040-116">Az ablak alján kattintson **beállítás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="78040-116">At the bottom of the window, click **Add Setting**.</span></span>
4. <span data-ttu-id="78040-117">Kattintson a **ütemezés hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="78040-117">Click **Add Schedule**.</span></span>
5. <span data-ttu-id="78040-118">Adjon meg egy **neve** és opcionálisan egy **leírás** az új schedule.your az ütemezés futtatásához **egyszer**, **óránkénti**, **napi**, **heti**, vagy **havi**.</span><span class="sxs-lookup"><span data-stu-id="78040-118">Type a **Name** and optionally a **Description** for the new schedule.your schedule will run **One Time**, **Hourly**, **Daily**, **Weekly**, or **Monthly**.</span></span>
6. <span data-ttu-id="78040-119">Adjon meg egy **kezdete** és egyéb beállítások kiválasztott ütemezéstípustól függően.</span><span class="sxs-lookup"><span data-stu-id="78040-119">Specify a **Start Time** and other options depending on the type of schedule that you selected.</span></span>

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a><span data-ttu-id="78040-120">Új ütemezés létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="78040-120">To create a new schedule in the Azure portal</span></span>
1. <span data-ttu-id="78040-121">Az Azure portálon, az automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="78040-121">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="78040-122">Kattintson a **ütemezések** csempére kattintva nyissa meg a **ütemezések** panelen.</span><span class="sxs-lookup"><span data-stu-id="78040-122">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="78040-123">Kattintson a **ütemezés hozzáadása** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="78040-123">Click **Add a schedule** at the top of the blade.</span></span>
4. <span data-ttu-id="78040-124">Az a **új ütemezés** panelen adjon meg egy **neve** és opcionálisan egy **leírás** az új ütemezés.</span><span class="sxs-lookup"><span data-stu-id="78040-124">On the **New schedule** blade, type a **Name** and optionally a **Description** for the new schedule.</span></span>
5. <span data-ttu-id="78040-125">Válassza ki, hogy az ütemezés futtatásához egy alkalommal vagy feladatról ütemezés kiválasztásával **egyszer** vagy **ismétlődési**.</span><span class="sxs-lookup"><span data-stu-id="78040-125">Select whether the schedule will run one time, or on a reoccurring schedule by selecting **Once** or **Recurrence**.</span></span>  <span data-ttu-id="78040-126">Választásakor **egyszer** adjon meg egy **kezdési időpont** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="78040-126">If you select **Once** specify a **Start time** and then click **Create**.</span></span>  <span data-ttu-id="78040-127">Ha **ismétlődési**, adja meg egy **kezdési időpont** és a gyakoriság, milyen gyakran szeretné a runbook ismételje meg a-az **óra**, **nap**, **hét**, vagy **hónap**.</span><span class="sxs-lookup"><span data-stu-id="78040-127">If you select **Recurrence**, specify a **Start time** and the frequency for how often you want the runbook to repeat - by **hour**, **day**, **week**, or by **month**.</span></span>  <span data-ttu-id="78040-128">Ha **hét** vagy **hónap** a legördülő listából a **ismétlődési beállítást** fog megjelenni a panelen és kiválasztáskor, a **ismétlődési beállítást** panel számára jelenik meg, és kiválaszthatja a hét napja, ha a kiválasztott **hét**.</span><span class="sxs-lookup"><span data-stu-id="78040-128">If you select **week** or **month** from the drop-down list, the **Recurrence option** will appear in the blade and upon selection, the **Recurrence option** blade will be presented and you can select the day of week if you selected **week**.</span></span>  <span data-ttu-id="78040-129">Ha a kiválasztott **hónap**, szerint is választhat **hét napjai** vagy a naptáron a hónap adott napjaira és végezetül szeretné futtatni a hónap utolsó napján, vagy nem, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="78040-129">If you selected **month**, you can choose by **week days** or specific days of the month on the calendar and finally, do you want to run it on the last day of the month or not and then click **OK**.</span></span>   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a><span data-ttu-id="78040-130">Új ütemezés létrehozása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="78040-130">To create a new schedule with Windows PowerShell</span></span>
<span data-ttu-id="78040-131">Használhatja a [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) parancsmag új ütemezés létrehozása az Azure Automationben klasszikus runbookok vagy [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) parancsmaggal a runbookok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="78040-131">You can use the [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) cmdlet to create a new schedule in Azure Automation for classic runbooks, or [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="78040-132">Meg kell adnia az ütemezés és a gyakoriság fusson kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="78040-132">You must specify the start time for the schedule and the frequency it should run.</span></span>

<span data-ttu-id="78040-133">Az alábbi Példaparancsok szemléltetik hozzon létre egy új ütemezést, amely a minden nap 3:30 PM 2015. január 20 kezdődően az Azure szolgáltatásfelügyelet parancsmagjával.</span><span class="sxs-lookup"><span data-stu-id="78040-133">The following sample commands show how to create a new schedule that runs each day at 3:30 PM starting on January 20, 2015 with an Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

<span data-ttu-id="78040-134">Az alábbi Példaparancsok bemutatja, hogyan hozzon létre egy ütemezést, a 15. és az Azure Resource Manager parancsmagjával havonta 30.</span><span class="sxs-lookup"><span data-stu-id="78040-134">The following sample commands shows how to create a schedule for the 15th and 30th of every month using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a><span data-ttu-id="78040-135">Ütemezés összekapcsolása runbookkal</span><span class="sxs-lookup"><span data-stu-id="78040-135">Linking a schedule to a runbook</span></span>
<span data-ttu-id="78040-136">Egy runbook több ütemezéssel is lehet társítani, és egy ütemezés szerint lehet kapcsolni több runbook.</span><span class="sxs-lookup"><span data-stu-id="78040-136">A runbook can be linked to multiple schedules, and a schedule can have multiple runbooks linked to it.</span></span> <span data-ttu-id="78040-137">Ha a runbook paraméterekkel rendelkezik, majd is értékeket ad meg a számukra.</span><span class="sxs-lookup"><span data-stu-id="78040-137">If a runbook has parameters, then you can provide values for them.</span></span> <span data-ttu-id="78040-138">Adjon meg értéket minden kötelező paraméterhez, és előfordulhat, hogy adjon meg értékeket a választható paramétereket.</span><span class="sxs-lookup"><span data-stu-id="78040-138">You must provide values for any mandatory parameters and may provide values for any optional parameters.</span></span>  <span data-ttu-id="78040-139">Ezeket az értékeket fogja használni minden alkalommal, amikor a runbook az ütemezés szerint elindult.</span><span class="sxs-lookup"><span data-stu-id="78040-139">These values will be used each time the runbook is started by this schedule.</span></span>  <span data-ttu-id="78040-140">Ugyanaz a runbook egy másik ütemezés csatolja, és adjon meg másik paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="78040-140">You can attach the same runbook to another schedule and specify different parameter values.</span></span>

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a><span data-ttu-id="78040-141">Az ütemezés összekapcsolása runbookkal a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="78040-141">To link a schedule to a runbook with the Azure classic portal</span></span>
1. <span data-ttu-id="78040-142">A klasszikus Azure portálon, válassza ki a **Automation** és majd kattintson az automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="78040-142">In the Azure classic portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="78040-143">Válassza ki a **Runbookok** fülre.</span><span class="sxs-lookup"><span data-stu-id="78040-143">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="78040-144">Kattintson az ütemezni kívánt runbook nevére.</span><span class="sxs-lookup"><span data-stu-id="78040-144">Click on the name of the runbook to schedule.</span></span>
4. <span data-ttu-id="78040-145">Kattintson a **ütemezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="78040-145">Click the **Schedule** tab.</span></span>
5. <span data-ttu-id="78040-146">Ha a runbook jelenleg nem kapcsolódik egy ütemezéshez, akkor nyílik lehetőség **hivatkozásra egy új ütemezést** vagy **meglévő ütemezés mutató hivatkozás**.</span><span class="sxs-lookup"><span data-stu-id="78040-146">If the runbook is not currently linked to a schedule, then you will be given the option to **Link to a New Schedule** or **Link to an Existing Schedule**.</span></span>  <span data-ttu-id="78040-147">Ha a runbook jelenleg hozzá van kapcsolva egy ütemezést, kattintson a **hivatkozás** ezek a beállítások eléréséhez az ablak alján.</span><span class="sxs-lookup"><span data-stu-id="78040-147">If the runbook is currently linked to a schedule, click **Link** at the bottom of the window to access these options.</span></span>
6. <span data-ttu-id="78040-148">Ha a runbook paraméterekkel rendelkezik, a rendszer kéri, ezek értékeinek megadására.</span><span class="sxs-lookup"><span data-stu-id="78040-148">If the runbook has parameters, you will be prompted for their values.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a><span data-ttu-id="78040-149">Az ütemezés összekapcsolása runbookkal a az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="78040-149">To link a schedule to a runbook with the Azure portal</span></span>
1. <span data-ttu-id="78040-150">Az Azure portálon, az automation-fiók, kattintson a **Runbookok** csempére kattintva nyissa meg a **Runbookok** panelen.</span><span class="sxs-lookup"><span data-stu-id="78040-150">In the Azure portal, from your automation account, click the **Runbooks** tile to open the **Runbooks** blade.</span></span>
2. <span data-ttu-id="78040-151">Kattintson az ütemezni kívánt runbook nevére.</span><span class="sxs-lookup"><span data-stu-id="78040-151">Click on the name of the runbook to schedule.</span></span>
3. <span data-ttu-id="78040-152">Ha a runbook jelenleg nem kapcsolódik egy ütemezést, majd nyílik létrehozhat egy új ütemezést, vagy meglévő ütemezés mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="78040-152">If the runbook is not currently linked to a schedule, then you will be given the option to create a new schedule or link to an existing schedule.</span></span>  
4. <span data-ttu-id="78040-153">Ha a runbook paraméterekkel rendelkezik, válassza a beállítás **(alapértelmezett: Azure) futtatási beállítások módosítása** és a **paraméterek** panel oszlik, ahol megadhatja a ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="78040-153">If the runbook has parameters, you can select the option **Modify run settings (Default:Azure)** and the **Parameters** blade is presented where you can enter the information accordingly.</span></span>  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a><span data-ttu-id="78040-154">Ütemezés összekapcsolása runbookkal a Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="78040-154">To link a schedule to a runbook with Windows PowerShell</span></span>
<span data-ttu-id="78040-155">Használhatja a [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) összekapcsolhat egy ütemezést egy klasszikus runbookhoz vagy [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) parancsmaggal a runbookok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="78040-155">You can use the [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) to link a schedule to a classic runbook or [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) cmdlet for runbooks in the Azure portal.</span></span>  <span data-ttu-id="78040-156">A paraméterek paraméterrel megadhatja a gyermekrunbook paramétereinek értékeit.</span><span class="sxs-lookup"><span data-stu-id="78040-156">You can specify values for the runbook’s parameters with the Parameters parameter.</span></span> <span data-ttu-id="78040-157">Lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) a paraméterértékek meghatározásáról a további információt.</span><span class="sxs-lookup"><span data-stu-id="78040-157">See [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md) for more information on specifying parameter values.</span></span>

<span data-ttu-id="78040-158">A következő mintaparancsok bemutatják, miként kapcsolhat össze egy ütemezést egy Azure szolgáltatásfelügyeleti parancsmaggal paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="78040-158">The following sample commands show how to link a schedule using an Azure Service Management cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

<span data-ttu-id="78040-159">A következő mintaparancsok bemutatják, hogyan kapcsolhat össze egy ütemezést egy runbookhoz, az Azure Resource Manager parancsmaggal paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="78040-159">The following sample commands show how to link a schedule to a runbook using an Azure Resource Manager cmdlet with parameters.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a><span data-ttu-id="78040-160">Ütemezés letiltása</span><span class="sxs-lookup"><span data-stu-id="78040-160">Disabling a schedule</span></span>
<span data-ttu-id="78040-161">Ha letilt egy ütemezést, minden hozzá kapcsolt forgatókönyvre nem fog működni, hogy ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="78040-161">When you disable a schedule, any runbooks linked to it will no longer run on that schedule.</span></span> <span data-ttu-id="78040-162">Manuálisan ütemezésének letiltása, vagy az ütemezések gyakorisággal lejárati idő beállítása a létrehozott.</span><span class="sxs-lookup"><span data-stu-id="78040-162">You can manually disable a schedule or set an expiration time for schedules with a frequency when you create them.</span></span> <span data-ttu-id="78040-163">A lejárati idő elérésekor a ütemezés letiltásra kerül.</span><span class="sxs-lookup"><span data-stu-id="78040-163">When the expiration time is reached, the schedule will be disabled.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a><span data-ttu-id="78040-164">A klasszikus Azure portálon ütemezésének letiltása</span><span class="sxs-lookup"><span data-stu-id="78040-164">To disable a schedule from the Azure classic portal</span></span>
<span data-ttu-id="78040-165">Az ütemezés részleteit megjelenítő oldalon az ütemezés a klasszikus Azure portálon ütemezés letilthatja.</span><span class="sxs-lookup"><span data-stu-id="78040-165">You can disable a schedule in the Azure classic portal from the Schedule Details page for the schedule.</span></span>

1. <span data-ttu-id="78040-166">A klasszikus Azure portálon válassza ki az Automation, és kattintson majd automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="78040-166">In the Azure classic portal, select Automation and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="78040-167">Válassza az eszközök lapot.</span><span class="sxs-lookup"><span data-stu-id="78040-167">Select the Assets tab.</span></span>
3. <span data-ttu-id="78040-168">Kattintson a nevére, nyissa meg annak információs lapját a kívánt ütemezést.</span><span class="sxs-lookup"><span data-stu-id="78040-168">Click the name of a schedule to open its detail page.</span></span>
4. <span data-ttu-id="78040-169">Változás **engedélyezett** való **nem**.</span><span class="sxs-lookup"><span data-stu-id="78040-169">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-from-the-azure-portal"></a><span data-ttu-id="78040-170">Azure-portálról ütemezésének letiltása</span><span class="sxs-lookup"><span data-stu-id="78040-170">To disable a schedule from the Azure portal</span></span>
1. <span data-ttu-id="78040-171">Az Azure portálon, az automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.</span><span class="sxs-lookup"><span data-stu-id="78040-171">In the Azure portal, from your automation account, click the **Assets** tile to open the **Assets** blade.</span></span>
2. <span data-ttu-id="78040-172">Kattintson a **ütemezések** csempére kattintva nyissa meg a **ütemezések** panelen.</span><span class="sxs-lookup"><span data-stu-id="78040-172">Click the **Schedules** tile to open the **Schedules** blade.</span></span>
3. <span data-ttu-id="78040-173">Kattintson a Részletek panel megnyitásához ütemezés nevét.</span><span class="sxs-lookup"><span data-stu-id="78040-173">Click the name of a schedule to open the details blade.</span></span>
4. <span data-ttu-id="78040-174">Változás **engedélyezett** való **nem**.</span><span class="sxs-lookup"><span data-stu-id="78040-174">Change **Enabled** to **No**.</span></span>

### <a name="to-disable-a-schedule-with-windows-powershell"></a><span data-ttu-id="78040-175">A Windows PowerShell-lel ütemezésének letiltása</span><span class="sxs-lookup"><span data-stu-id="78040-175">To disable a schedule with Windows PowerShell</span></span>
<span data-ttu-id="78040-176">Használhatja a [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) parancsmag klasszikus runbook meglévő ütemezés tulajdonságainak módosításához vagy [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) parancsmaggal a runbookok az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="78040-176">You can use the [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) cmdlet to change the properties of an existing schedule for a classic runbook or [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) cmdlet for runbooks in the Azure portal.</span></span> <span data-ttu-id="78040-177">Az ütemezés letiltásához adja meg a **hamis** a a **IsEnabled** paraméter.</span><span class="sxs-lookup"><span data-stu-id="78040-177">To disable the schedule, specify **false** for the **IsEnabled** parameter.</span></span>

<span data-ttu-id="78040-178">Az alábbi Példaparancsok szemléltetik az Azure szolgáltatásfelügyelet parancsmaggal ütemezésének letiltása.</span><span class="sxs-lookup"><span data-stu-id="78040-178">The following sample commands show how to disable a schedule using the Azure Service Management cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

<span data-ttu-id="78040-179">Az alábbi Példaparancsok szemléltetik egy runbook az Azure Resource Manager parancsmagjával ütemezésének letiltása.</span><span class="sxs-lookup"><span data-stu-id="78040-179">The following sample commands show how to disable a schedule for a runbook using an Azure Resource Manager cmdlet.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a><span data-ttu-id="78040-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78040-180">Next steps</span></span>
* <span data-ttu-id="78040-181">Ütemezések használatával kapcsolatos további tudnivalókért lásd: [Azure Automation szolgáltatásbeli ütemezési eszközök](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span><span class="sxs-lookup"><span data-stu-id="78040-181">To learn more about working with schedules, see [Schedule Assets in Azure Automation](http://msdn.microsoft.com/library/azure/dn940016.aspx)</span></span>
* <span data-ttu-id="78040-182">Ismerkedés az Azure Automation runbookjai, lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="78040-182">To get started with runbooks in Azure Automation, see [Starting a Runbook in Azure Automation](automation-starting-a-runbook.md)</span></span> 

