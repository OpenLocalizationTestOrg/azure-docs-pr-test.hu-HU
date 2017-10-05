---
title: "Runbook elindítása az Azure Automationben |} Microsoft Docs"
description: "Összefoglalja a különböző módszereket, amelyek segítségével elindít egy forgatókönyvet az Azure Automation és részleteit az Azure portál és a Windows PowerShell használatával."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6ee756b4-9200-4eb2-9bda-ec156853803b
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 844831b63d5263987ed05370125fbe9f01913ab9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="e063c-103">Runbook elindítása az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="e063c-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="e063c-104">Az alábbi táblázat segítségével meghatározhatja, hogy a runbook indítása az Azure Automationben, amely az adott helyzetnek legmegfelelőbb módszer.</span><span class="sxs-lookup"><span data-stu-id="e063c-104">The following table will help you determine the method to start a runbook in Azure Automation that is most suitable to your particular scenario.</span></span> <span data-ttu-id="e063c-105">Ez a cikk tartalmazza a runbook elindítása és az Azure portál és a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e063c-105">This article includes details on starting a runbook with the Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="e063c-106">Egyéb módszerekkel kapcsolatos részletek szerepelnek a lenti hivatkozásokra kattintva érheti el más dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="e063c-106">Details on the other methods are provided in other documentation that you can access from the links below.</span></span>

| <span data-ttu-id="e063c-107">**MÓDSZER**</span><span class="sxs-lookup"><span data-stu-id="e063c-107">**METHOD**</span></span> | <span data-ttu-id="e063c-108">**JELLEMZŐI**</span><span class="sxs-lookup"><span data-stu-id="e063c-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="e063c-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e063c-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="e063c-110">Az interaktív felhasználói kezelőfelület legegyszerűbb módja.</span><span class="sxs-lookup"><span data-stu-id="e063c-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="e063c-111">Űrlap egyszerű paraméter értékének megadására.</span><span class="sxs-lookup"><span data-stu-id="e063c-111">Form to provide simple parameter values.</span></span><br> <li><span data-ttu-id="e063c-112">Feladat állapotát nyomon.</span><span class="sxs-lookup"><span data-stu-id="e063c-112">Easily track job state.</span></span><br> <li><span data-ttu-id="e063c-113">Az Azure bejelentkezési hitelesített hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e063c-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="e063c-114">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e063c-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="e063c-115">A Windows PowerShell-parancsmagokkal a parancssorból hívható.</span><span class="sxs-lookup"><span data-stu-id="e063c-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="e063c-116">Több lépést automatizált megoldást szerepeljenek.</span><span class="sxs-lookup"><span data-stu-id="e063c-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="e063c-117">A tanúsítvány vagy egyszerű / szolgáltatás OAuth felhasználói kérelem hitelesítésekor egyszerű.</span><span class="sxs-lookup"><span data-stu-id="e063c-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="e063c-118">Adja meg egyszerű és összetett paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="e063c-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="e063c-119">Feladat-állapotok nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="e063c-119">Track job state.</span></span><br> <li><span data-ttu-id="e063c-120">Az ügyfél PowerShell-parancsmagok támogatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="e063c-120">Client required to support PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="e063c-121">Azure Automation szolgáltatásbeli API</span><span class="sxs-lookup"><span data-stu-id="e063c-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="e063c-122">Legrugalmasabb módszer, de a legtöbb komplex is.</span><span class="sxs-lookup"><span data-stu-id="e063c-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="e063c-123">Felelnek meg, és bármilyen egyéni kód használhat, amelyek HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="e063c-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="e063c-124">A kérelem hitelesítése a tanúsítványt, vagy Oauth felhasználó egyszerű / szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="e063c-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="e063c-125">Adja meg egyszerű és összetett paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="e063c-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="e063c-126">Feladat-állapotok nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="e063c-126">Track job state.</span></span> |
| [<span data-ttu-id="e063c-127">Webhook</span><span class="sxs-lookup"><span data-stu-id="e063c-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="e063c-128">Indítsa el a runbook egyetlen HTTP-kérelemből.</span><span class="sxs-lookup"><span data-stu-id="e063c-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="e063c-129">Felhasználók hitelesítése a biztonsági jogkivonat URL-címben.</span><span class="sxs-lookup"><span data-stu-id="e063c-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="e063c-130">Ügyfél nem bírálhatja felül a webhook létrehozásakor megadott paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="e063c-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="e063c-131">Runbook megadhat egyetlen paramétert, amely a HTTP-kérelem részletes adatait a telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="e063c-131">Runbook can define single parameter that is populated with the HTTP request details.</span></span><br> <li><span data-ttu-id="e063c-132">Nem képes a webhook URL-CÍMÉT a feladat-állapotok nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="e063c-132">No ability to track job state through webhook URL.</span></span> |
| [<span data-ttu-id="e063c-133">Az Azure riasztás válaszolni</span><span class="sxs-lookup"><span data-stu-id="e063c-133">Respond to Azure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="e063c-134">Elindít egy forgatókönyvet az Azure riasztás válasz.</span><span class="sxs-lookup"><span data-stu-id="e063c-134">Start a runbook in response to Azure alert.</span></span><br> <li><span data-ttu-id="e063c-135">Konfigurálja webhook runbookhoz és riasztási mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="e063c-135">Configure webhook for runbook and link to alert.</span></span><br> <li><span data-ttu-id="e063c-136">Felhasználók hitelesítése a biztonsági jogkivonat URL-címben.</span><span class="sxs-lookup"><span data-stu-id="e063c-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="e063c-137">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="e063c-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="e063c-138">Óránkénti, napi, heti vagy havi ütemezés szerint automatikusan elindítja a runbookot.</span><span class="sxs-lookup"><span data-stu-id="e063c-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="e063c-139">Azure-portálon, a PowerShell-parancsmagok vagy az Azure API ütemezés módosítására.</span><span class="sxs-lookup"><span data-stu-id="e063c-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="e063c-140">Adja meg az ütemezés használni kívánt paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="e063c-140">Provide parameter values to be used with schedule.</span></span> |
| [<span data-ttu-id="e063c-141">Egy másik runbookból</span><span class="sxs-lookup"><span data-stu-id="e063c-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="e063c-142">Egy runbook használata egy másik runbook egy tevékenységére.</span><span class="sxs-lookup"><span data-stu-id="e063c-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="e063c-143">Akkor hasznos, ha több runbookok által használt funkciók.</span><span class="sxs-lookup"><span data-stu-id="e063c-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="e063c-144">Adja meg a gyermekrunbook paraméter értékét, és kimeneti a szülőrunbook.</span><span class="sxs-lookup"><span data-stu-id="e063c-144">Provide parameter values to child runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="e063c-145">Az alábbi képen egy runbook életciklusának részletes részletes folyamatát mutatja be.</span><span class="sxs-lookup"><span data-stu-id="e063c-145">The following image illustrates detailed step-by-step process in the life cycle of a runbook.</span></span> <span data-ttu-id="e063c-146">Ez magában foglalja a különböző módokon egy runbook indítását az Azure Automationben összetevők szükséges a hibrid forgatókönyv-feldolgozó Azure Automation-forgatókönyv és a különböző összetevők közötti interakció végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="e063c-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker to execute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="e063c-147">Automatizálási runbookok végrehajtása az adatközpontban található kapcsolatos információkért tekintse meg [hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="e063c-147">To learn about executing Automation runbooks in your datacenter, refer to [hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Runbook-architektúra](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-the-azure-portal"></a><span data-ttu-id="e063c-149">Runbook elindítása és az Azure portál</span><span class="sxs-lookup"><span data-stu-id="e063c-149">Starting a runbook with the Azure portal</span></span>
1. <span data-ttu-id="e063c-150">Válassza ki az Azure-portálon **Automation** és majd kattintson az automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e063c-150">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="e063c-151">A központ menüben válassza ki a **Runbookok**.</span><span class="sxs-lookup"><span data-stu-id="e063c-151">On the Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="e063c-152">Az a **Runbookok** panelen válassza ki a runbookot, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="e063c-152">On the **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="e063c-153">Ha a runbook paraméterekkel rendelkezik, kéri szövegmező értékeinek megadására az egyes paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e063c-153">If the runbook has parameters, you will be prompted to provide values with a text box for each parameter.</span></span> <span data-ttu-id="e063c-154">Lásd: [Runbook-paraméterek](#Runbook-parameters) alatt kapcsolatos tovább információkért paraméterek.</span><span class="sxs-lookup"><span data-stu-id="e063c-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="e063c-155">Az a **feladat** panelen megtekintheti a runbook feladatának állapotát.</span><span class="sxs-lookup"><span data-stu-id="e063c-155">On the **Job** blade, you can view the status of the runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="e063c-156">Runbook indítása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e063c-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="e063c-157">Használhatja a [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) runbook indítása a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="e063c-157">You can use the [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) to start a runbook with Windows PowerShell.</span></span> <span data-ttu-id="e063c-158">Az alábbi példakód elindít egy Test-Runbook nevű runbookot.</span><span class="sxs-lookup"><span data-stu-id="e063c-158">The following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="e063c-159">Start-AzureRmAutomationRunbook adja vissza egy feladatobjektumot, amely segítségével nyomon követheti az állapotát, amennyiben a runbook indítását.</span><span class="sxs-lookup"><span data-stu-id="e063c-159">Start-AzureRmAutomationRunbook returns a job object that you can use to track its status once the runbook is started.</span></span> <span data-ttu-id="e063c-160">Ezután használhatja a feladatobjektum rendelkező [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) a feladat állapotának megállapításához és [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) való lekérheti annak kimenetét.</span><span class="sxs-lookup"><span data-stu-id="e063c-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) to determine the status of the job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) to get its output.</span></span> <span data-ttu-id="e063c-161">Az alábbi példakód elindít egy Test-Runbook nevű, megvárja, amíg befejeződött, majd megjeleníti annak kimenetét.</span><span class="sxs-lookup"><span data-stu-id="e063c-161">The following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

```
$runbookName = "Test-Runbook"
$ResourceGroup = "ResourceGroup01"
$AutomationAcct = "MyAutomationAccount"

$job = Start-AzureRmAutomationRunbook –AutomationAccountName $AutomationAcct -Name $runbookName -ResourceGroupName $ResourceGroup

$doLoop = $true
While ($doLoop) {
   $job = Get-AzureRmAutomationJob –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup
   $status = $job.Status
   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped"))
}

Get-AzureRmAutomationJobOutput –AutomationAccountName $AutomationAcct -Id $job.JobId -ResourceGroupName $ResourceGroup –Stream Output
```

<span data-ttu-id="e063c-162">Ha a runbook paramétereket igényel, akkor meg kell adnia azokat egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) amelyben a kivonattábla kulcsa megegyezik a paraméter neve és értéke a paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="e063c-162">If the runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where the key of the hashtable matches the parameter name and the value is the parameter value.</span></span> <span data-ttu-id="e063c-163">A következő példa bemutatja, hogyan két karakterlánc-paraméterrel Utónév és Vezetéknév, egy RepeatCount nevű egész számot és egy Show nevű logikai paraméterrel rendelkező runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="e063c-163">The following example shows how to start a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="e063c-164">A paraméterek további információkért lásd: [Runbook-paraméterek](#Runbook-parameters) alatt.</span><span class="sxs-lookup"><span data-stu-id="e063c-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="e063c-165">Runbook-paraméterek</span><span class="sxs-lookup"><span data-stu-id="e063c-165">Runbook parameters</span></span>
<span data-ttu-id="e063c-166">Amikor elindít egy forgatókönyvet az Azure portál vagy a Windows PowerShell, a parancsfájl az Azure Automation webszolgáltatás keresztül küld el.</span><span class="sxs-lookup"><span data-stu-id="e063c-166">When you start a runbook from the Azure Portal or Windows PowerShell, the instruction is sent through the Azure Automation web service.</span></span> <span data-ttu-id="e063c-167">Ez a szolgáltatás nem támogatja az összetett adattípusú paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e063c-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="e063c-168">Ha meg kell adnia egy értéket egy összetett paraméterhez, majd meg kell hívnia az beágyazott egy másik runbookból a [az Azure Automation runbookjai gyermek](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="e063c-168">If you need to provide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="e063c-169">Az Azure Automation webszolgáltatás bizonyos adattípusokkal a következő szakaszokban ismertetett módon paraméterek speciális funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="e063c-169">The Azure Automation web service will provide special functionality for parameters using certain data types as described in the following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="e063c-170">Névvel ellátott értékek</span><span class="sxs-lookup"><span data-stu-id="e063c-170">Named values</span></span>
<span data-ttu-id="e063c-171">Ha a paraméter adattípusa [objektum], akkor használhatja a következő JSON formátummal küldhet neki egy névvel ellátott értékek listája: *{Név1: 'Érték1', Name2: 'Érték2', név3: "Érték3"}*.</span><span class="sxs-lookup"><span data-stu-id="e063c-171">If the parameter is data type [object], then you can use the following JSON format to send it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="e063c-172">Ezek az értékek csak egyszerű típusok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="e063c-172">These values must be simple types.</span></span> <span data-ttu-id="e063c-173">A runbook fog kapni, a paraméter egy [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) a Tulajdonságok megegyeznek az egyes névvel rendelkező érték.</span><span class="sxs-lookup"><span data-stu-id="e063c-173">The runbook will receive the parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond to each named value.</span></span>

<span data-ttu-id="e063c-174">Tesztrunbook a következő egy felhasználó nevű paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="e063c-174">Consider the following test runbook that accepts a parameter called user.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][object]$user
   )
    $userObject = $user | ConvertFrom-JSON
    if ($userObject.Show) {
        foreach ($i in 1..$userObject.RepeatCount) {
            $userObject.FirstName
            $userObject.LastName
        }
    }
}
```

<span data-ttu-id="e063c-175">A user paraméterhez a következő szöveg használható.</span><span class="sxs-lookup"><span data-stu-id="e063c-175">The following text could be used for the user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="e063c-176">Ez a következő kimenetet eredményezi.</span><span class="sxs-lookup"><span data-stu-id="e063c-176">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="e063c-177">Tömbök</span><span class="sxs-lookup"><span data-stu-id="e063c-177">Arrays</span></span>
<span data-ttu-id="e063c-178">Ha a paraméter egy tömb, például [array] vagy [string []], a következő JSON formátummal küldhet neki értékből álló lista használhatja: *[érték1, érték2, érték3]*.</span><span class="sxs-lookup"><span data-stu-id="e063c-178">If the parameter is an array such as [array] or [string[]], then you can use the following JSON format to send it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="e063c-179">Ezek az értékek csak egyszerű típusok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="e063c-179">These values must be simple types.</span></span>

<span data-ttu-id="e063c-180">Tesztrunbook a következő nevű egy paramétert fogad *felhasználói*.</span><span class="sxs-lookup"><span data-stu-id="e063c-180">Consider the following test runbook that accepts a parameter called *user*.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][array]$user
   )
    if ($user[3]) {
        foreach ($i in 1..$user[2]) {
            $ user[0]
            $ user[1]
        }
    }
}
```

<span data-ttu-id="e063c-181">A user paraméterhez a következő szöveg használható.</span><span class="sxs-lookup"><span data-stu-id="e063c-181">The following text could be used for the user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="e063c-182">Ez a következő kimenetet eredményezi.</span><span class="sxs-lookup"><span data-stu-id="e063c-182">This results in the following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="e063c-183">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="e063c-183">Credentials</span></span>
<span data-ttu-id="e063c-184">Ha a paraméter adattípusa **PSCredential**, akkor megadhatja egy Azure Automation neve [hitelesítőadat-eszköz](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="e063c-184">If the parameter is data type **PSCredential**, then you can provide the name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="e063c-185">A runbook lekéri a megadott nevű hitelesítő adat.</span><span class="sxs-lookup"><span data-stu-id="e063c-185">The runbook will retrieve the credential with the name that you specify.</span></span>

<span data-ttu-id="e063c-186">Tesztrunbook a következő fogad el egy paraméter, hitelesítő adat.</span><span class="sxs-lookup"><span data-stu-id="e063c-186">Consider the following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="e063c-187">A következő szöveg használható a felhasználó paraméter feltéve, hogy létezik egy nevű hitelesítőeszköz *a hitelesítő adatok*.</span><span class="sxs-lookup"><span data-stu-id="e063c-187">The following text could be used for the user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="e063c-188">Feltéve, hogy a hitelesítő adatokat felhasználónév lett *jsmith*, az eredmény a következő kimenet.</span><span class="sxs-lookup"><span data-stu-id="e063c-188">Assuming the username in the credential was *jsmith*, this results in the following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="e063c-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e063c-189">Next steps</span></span>
* <span data-ttu-id="e063c-190">A runbook-architektúra az aktuális cikkben runbookok kezelése az Azure és a hibrid forgatókönyv-feldolgozót a helyszíni erőforrások magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="e063c-190">The runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with the Hybrid Runbook Worker.</span></span>  <span data-ttu-id="e063c-191">Automatizálási runbookok végrehajtása az adatközpontban található kapcsolatos információkért tekintse meg [hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="e063c-191">To learn about executing Automation runbooks in your datacenter, refer to [Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="e063c-192">Az egyedi vagy közös funkciók más runbookok által használható létrehozása moduláris runbookok kapcsolatos további tudnivalókért lásd [Gyermekforgatókönyvek](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="e063c-192">To learn more about the creating modular runbooks to be used by other runbooks for specific or common functions, refer to [Child Runbooks](automation-child-runbooks.md).</span></span>

