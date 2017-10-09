---
title: "Azure Automation forgatókönyv aaaStarting |} Microsoft Docs"
description: "Hello többféle módszerrel is használt toostart egy runbookot, az Azure Automationben, valamint az információk is hello Azure-portál és a Windows PowerShell biztosít foglalja össze."
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
ms.openlocfilehash: e44bce5b56b8e803f9247fbb4f3d4db7ab35c913
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="starting-a-runbook-in-azure-automation"></a><span data-ttu-id="9b41c-103">Runbook elindítása az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="9b41c-103">Starting a runbook in Azure Automation</span></span>
<span data-ttu-id="9b41c-104">hello a következő táblázat segítségével meghatározhatja, hogy hello metódus toostart az Azure Automationben legmegfelelőbb tooyour adott forgatókönyv, amely runbook.</span><span class="sxs-lookup"><span data-stu-id="9b41c-104">hello following table will help you determine hello method toostart a runbook in Azure Automation that is most suitable tooyour particular scenario.</span></span> <span data-ttu-id="9b41c-105">Ez a cikk tartalmazza a runbook indítása a hello Azure-portálon, és a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9b41c-105">This article includes details on starting a runbook with hello Azure portal and with Windows PowerShell.</span></span> <span data-ttu-id="9b41c-106">Részletek a hello más módszerekkel igénybe más dokumentáció, amely hello a lenti hivatkozásokra kattintva érheti el.</span><span class="sxs-lookup"><span data-stu-id="9b41c-106">Details on hello other methods are provided in other documentation that you can access from hello links below.</span></span>

| <span data-ttu-id="9b41c-107">**MÓDSZER**</span><span class="sxs-lookup"><span data-stu-id="9b41c-107">**METHOD**</span></span> | <span data-ttu-id="9b41c-108">**JELLEMZŐI**</span><span class="sxs-lookup"><span data-stu-id="9b41c-108">**CHARACTERISTICS**</span></span> |
| --- | --- |
| [<span data-ttu-id="9b41c-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b41c-109">Azure Portal</span></span>](#starting-a-runbook-with-the-azure-portal) |<li><span data-ttu-id="9b41c-110">Az interaktív felhasználói kezelőfelület legegyszerűbb módja.</span><span class="sxs-lookup"><span data-stu-id="9b41c-110">Simplest method with interactive user interface.</span></span><br> <li><span data-ttu-id="9b41c-111">Űrlap tooprovide egyszerű paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="9b41c-111">Form tooprovide simple parameter values.</span></span><br> <li><span data-ttu-id="9b41c-112">Feladat állapotát nyomon.</span><span class="sxs-lookup"><span data-stu-id="9b41c-112">Easily track job state.</span></span><br> <li><span data-ttu-id="9b41c-113">Az Azure bejelentkezési hitelesített hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="9b41c-113">Access authenticated with Azure logon.</span></span> |
| [<span data-ttu-id="9b41c-114">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b41c-114">Windows PowerShell</span></span>](https://msdn.microsoft.com/library/dn690259.aspx) |<li><span data-ttu-id="9b41c-115">A Windows PowerShell-parancsmagokkal a parancssorból hívható.</span><span class="sxs-lookup"><span data-stu-id="9b41c-115">Call from command line with Windows PowerShell cmdlets.</span></span><br> <li><span data-ttu-id="9b41c-116">Több lépést automatizált megoldást szerepeljenek.</span><span class="sxs-lookup"><span data-stu-id="9b41c-116">Can be included in automated solution with multiple steps.</span></span><br> <li><span data-ttu-id="9b41c-117">A tanúsítvány vagy egyszerű / szolgáltatás OAuth felhasználói kérelem hitelesítésekor egyszerű.</span><span class="sxs-lookup"><span data-stu-id="9b41c-117">Request is authenticated with certificate or OAuth user principal / service principal.</span></span><br> <li><span data-ttu-id="9b41c-118">Adja meg egyszerű és összetett paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="9b41c-118">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="9b41c-119">Feladat-állapotok nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="9b41c-119">Track job state.</span></span><br> <li><span data-ttu-id="9b41c-120">Ügyfél toosupport PowerShell-parancsmagok szükséges.</span><span class="sxs-lookup"><span data-stu-id="9b41c-120">Client required toosupport PowerShell cmdlets.</span></span> |
| [<span data-ttu-id="9b41c-121">Azure Automation szolgáltatásbeli API</span><span class="sxs-lookup"><span data-stu-id="9b41c-121">Azure Automation API</span></span>](https://msdn.microsoft.com/library/azure/mt662285.aspx) |<li><span data-ttu-id="9b41c-122">Legrugalmasabb módszer, de a legtöbb komplex is.</span><span class="sxs-lookup"><span data-stu-id="9b41c-122">Most flexible method but also most complex.</span></span><br> <li><span data-ttu-id="9b41c-123">Felelnek meg, és bármilyen egyéni kód használhat, amelyek HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="9b41c-123">Call from any custom code that can make HTTP requests.</span></span><br> <li><span data-ttu-id="9b41c-124">A kérelem hitelesítése a tanúsítványt, vagy Oauth felhasználó egyszerű / szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="9b41c-124">Request authenticated with certificate, or Oauth user principal / service principal.</span></span><br> <li><span data-ttu-id="9b41c-125">Adja meg egyszerű és összetett paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="9b41c-125">Provide simple and complex parameter values.</span></span><br> <li><span data-ttu-id="9b41c-126">Feladat-állapotok nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="9b41c-126">Track job state.</span></span> |
| [<span data-ttu-id="9b41c-127">Webhook</span><span class="sxs-lookup"><span data-stu-id="9b41c-127">Webhooks</span></span>](automation-webhooks.md) |<li><span data-ttu-id="9b41c-128">Indítsa el a runbook egyetlen HTTP-kérelemből.</span><span class="sxs-lookup"><span data-stu-id="9b41c-128">Start runbook from single HTTP request.</span></span><br> <li><span data-ttu-id="9b41c-129">Felhasználók hitelesítése a biztonsági jogkivonat URL-címben.</span><span class="sxs-lookup"><span data-stu-id="9b41c-129">Authenticated with security token in URL.</span></span><br> <li><span data-ttu-id="9b41c-130">Ügyfél nem bírálhatja felül a webhook létrehozásakor megadott paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="9b41c-130">Client cannot override parameter values specified when webhook created.</span></span> <span data-ttu-id="9b41c-131">Runbook megadhat egyetlen paramétert, amely hello HTTP kérelem részletes adatait a telepítéskor.</span><span class="sxs-lookup"><span data-stu-id="9b41c-131">Runbook can define single parameter that is populated with hello HTTP request details.</span></span><br> <li><span data-ttu-id="9b41c-132">Nincs lehetősége tootrack feladat állapota a webhook URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9b41c-132">No ability tootrack job state through webhook URL.</span></span> |
| [<span data-ttu-id="9b41c-133">Válaszoljon tooAzure riasztás</span><span class="sxs-lookup"><span data-stu-id="9b41c-133">Respond tooAzure Alert</span></span>](../log-analytics/log-analytics-alerts.md) |<li><span data-ttu-id="9b41c-134">Elindít egy forgatókönyvet az adott válasz tooAzure riasztás.</span><span class="sxs-lookup"><span data-stu-id="9b41c-134">Start a runbook in response tooAzure alert.</span></span><br> <li><span data-ttu-id="9b41c-135">Konfigurálja webhook runbook és tooalert hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="9b41c-135">Configure webhook for runbook and link tooalert.</span></span><br> <li><span data-ttu-id="9b41c-136">Felhasználók hitelesítése a biztonsági jogkivonat URL-címben.</span><span class="sxs-lookup"><span data-stu-id="9b41c-136">Authenticated with security token in URL.</span></span> |
| [<span data-ttu-id="9b41c-137">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="9b41c-137">Schedule</span></span>](automation-schedules.md) |<li><span data-ttu-id="9b41c-138">Óránkénti, napi, heti vagy havi ütemezés szerint automatikusan elindítja a runbookot.</span><span class="sxs-lookup"><span data-stu-id="9b41c-138">Automatically start runbook on hourly, daily, weekly, or monthly schedule.</span></span><br> <li><span data-ttu-id="9b41c-139">Azure-portálon, a PowerShell-parancsmagok vagy az Azure API ütemezés módosítására.</span><span class="sxs-lookup"><span data-stu-id="9b41c-139">Manipulate schedule through Azure portal, PowerShell cmdlets, or Azure API.</span></span><br> <li><span data-ttu-id="9b41c-140">Adja meg a paramétert értékek toobe használt ütemezéssel.</span><span class="sxs-lookup"><span data-stu-id="9b41c-140">Provide parameter values toobe used with schedule.</span></span> |
| [<span data-ttu-id="9b41c-141">Egy másik runbookból</span><span class="sxs-lookup"><span data-stu-id="9b41c-141">From Another Runbook</span></span>](automation-child-runbooks.md) |<li><span data-ttu-id="9b41c-142">Egy runbook használata egy másik runbook egy tevékenységére.</span><span class="sxs-lookup"><span data-stu-id="9b41c-142">Use a runbook as an activity in another runbook.</span></span><br> <li><span data-ttu-id="9b41c-143">Akkor hasznos, ha több runbookok által használt funkciók.</span><span class="sxs-lookup"><span data-stu-id="9b41c-143">Useful for functionality used by multiple runbooks.</span></span><br> <li><span data-ttu-id="9b41c-144">Adja meg a paramétert értékek toochild runbookot, és kimeneti a szülőrunbook.</span><span class="sxs-lookup"><span data-stu-id="9b41c-144">Provide parameter values toochild runbook and use output in parent runbook.</span></span> |

<span data-ttu-id="9b41c-145">hello alábbi kép ábrázolja részletes, lépésenkénti folyamat hello életciklusának egy runbook.</span><span class="sxs-lookup"><span data-stu-id="9b41c-145">hello following image illustrates detailed step-by-step process in hello life cycle of a runbook.</span></span> <span data-ttu-id="9b41c-146">Ez magában foglalja a különböző módokon egy runbook indítását az Azure Automationben hibrid forgatókönyv-feldolgozó tooexecute Azure Automation-forgatókönyv és a különböző összetevők közötti interakció szükséges összetevőket.</span><span class="sxs-lookup"><span data-stu-id="9b41c-146">It includes different ways a runbook is started in Azure Automation, components required for Hybrid Runbook Worker tooexecute Azure Automation runbooks and interactions between different components.</span></span> <span data-ttu-id="9b41c-147">Automation-forgatókönyveket az adatközpontban található végrehajtás alatt álló toolearn tekintse meg a túl[hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md)</span><span class="sxs-lookup"><span data-stu-id="9b41c-147">toolearn about executing Automation runbooks in your datacenter, refer too[hybrid runbook workers](automation-hybrid-runbook-worker.md)</span></span>

![Runbook-architektúra](media/automation-starting-runbook/runbooks-architecture.png)

## <a name="starting-a-runbook-with-hello-azure-portal"></a><span data-ttu-id="9b41c-149">Runbook indítása a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9b41c-149">Starting a runbook with hello Azure portal</span></span>
1. <span data-ttu-id="9b41c-150">Hello Azure-portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.</span><span class="sxs-lookup"><span data-stu-id="9b41c-150">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="9b41c-151">Hello központ menüben válassza ki a **Runbookok**.</span><span class="sxs-lookup"><span data-stu-id="9b41c-151">On hello Hub menu, select **Runbooks**.</span></span>
3. <span data-ttu-id="9b41c-152">A hello **Runbookok** panelen válassza ki a runbookot, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="9b41c-152">On hello **Runbooks** blade, select a runbook, and then click **Start**.</span></span>
4. <span data-ttu-id="9b41c-153">Ha hello runbook paraméterekkel rendelkezik, mindegyik paraméterhez szövegmezőben felszólító tooprovide értékeket fogja.</span><span class="sxs-lookup"><span data-stu-id="9b41c-153">If hello runbook has parameters, you will be prompted tooprovide values with a text box for each parameter.</span></span> <span data-ttu-id="9b41c-154">Lásd: [Runbook-paraméterek](#Runbook-parameters) alatt kapcsolatos tovább információkért paraméterek.</span><span class="sxs-lookup"><span data-stu-id="9b41c-154">See [Runbook Parameters](#Runbook-parameters) below for further details on parameters.</span></span>
5. <span data-ttu-id="9b41c-155">A hello **feladat** panelen megtekintheti hello hello runbook-feladat állapotának.</span><span class="sxs-lookup"><span data-stu-id="9b41c-155">On hello **Job** blade, you can view hello status of hello runbook job.</span></span>

## <a name="starting-a-runbook-with-windows-powershell"></a><span data-ttu-id="9b41c-156">Runbook indítása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9b41c-156">Starting a runbook with Windows PowerShell</span></span>
<span data-ttu-id="9b41c-157">Használhatja a hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart egy runbookot, a Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9b41c-157">You can use hello [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) toostart a runbook with Windows PowerShell.</span></span> <span data-ttu-id="9b41c-158">hello alábbi példakód elindít egy Test-Runbook nevű runbookot.</span><span class="sxs-lookup"><span data-stu-id="9b41c-158">hello following sample code starts a runbook called Test-Runbook.</span></span>

```
Start-AzureRmAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -ResourceGroupName "ResourceGroup01"
```

<span data-ttu-id="9b41c-159">Az objektum kezdő-AzureRmAutomationRunbook értéket ad vissza, a feladat használható tootrack állapotának hello runbook indítását követően.</span><span class="sxs-lookup"><span data-stu-id="9b41c-159">Start-AzureRmAutomationRunbook returns a job object that you can use tootrack its status once hello runbook is started.</span></span> <span data-ttu-id="9b41c-160">Ezután használhatja a feladatobjektum rendelkező [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) hello feladat állapotának toodetermine hello és [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget annak kimenetét.</span><span class="sxs-lookup"><span data-stu-id="9b41c-160">You can then use this job object with [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) toodetermine hello status of hello job and [Get-AzureRmAutomationJobOutput](https://msdn.microsoft.com/library/mt603476.aspx) tooget its output.</span></span> <span data-ttu-id="9b41c-161">a következő példakód hello elindít egy Test-Runbook nevű, megvárja, amíg befejeződött, majd megjeleníti annak kimenetét.</span><span class="sxs-lookup"><span data-stu-id="9b41c-161">hello following sample code starts a runbook called Test-Runbook, waits until it has completed, and then displays its output.</span></span>

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

<span data-ttu-id="9b41c-162">Ha hello runbookhoz paraméterek szükségesek, akkor meg kell adnia azokat egy [hashtable](http://technet.microsoft.com/library/hh847780.aspx) Ha hello hello kivonattábla kulcsa megegyezik-e a hello nevét és értékét hello hello paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="9b41c-162">If hello runbook requires parameters, then you must provide them as a [hashtable](http://technet.microsoft.com/library/hh847780.aspx) where hello key of hello hashtable matches hello parameter name and hello value is hello parameter value.</span></span> <span data-ttu-id="9b41c-163">hello következő példa bemutatja, hogyan toostart egy karakterlánc-paraméterrel rendelkező runbook nevű Utónév és Vezetéknév, egy RepeatCount nevű egész számot és egy Show nevű logikai paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="9b41c-163">hello following example shows how toostart a runbook with two string parameters named FirstName and LastName, an integer named RepeatCount, and a boolean parameter named Show.</span></span> <span data-ttu-id="9b41c-164">A paraméterek további információkért lásd: [Runbook-paraméterek](#Runbook-parameters) alatt.</span><span class="sxs-lookup"><span data-stu-id="9b41c-164">For additional information on parameters, see [Runbook Parameters](#Runbook-parameters) below.</span></span>

```
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -ResourceGroupName "ResourceGroup01" –Parameters $params
```

## <a name="runbook-parameters"></a><span data-ttu-id="9b41c-165">Runbook-paraméterek</span><span class="sxs-lookup"><span data-stu-id="9b41c-165">Runbook parameters</span></span>
<span data-ttu-id="9b41c-166">Amikor elindít egy runbookot hello Azure portál vagy a Windows PowerShell, hello utasítás hello Azure Automation webszolgáltatás keresztül küld el.</span><span class="sxs-lookup"><span data-stu-id="9b41c-166">When you start a runbook from hello Azure Portal or Windows PowerShell, hello instruction is sent through hello Azure Automation web service.</span></span> <span data-ttu-id="9b41c-167">Ez a szolgáltatás nem támogatja az összetett adattípusú paramétereket.</span><span class="sxs-lookup"><span data-stu-id="9b41c-167">This service does not support parameters with complex data types.</span></span> <span data-ttu-id="9b41c-168">Ha egy összetett paraméterhez tooprovide érték van szüksége, akkor meg kell hívnia az beágyazott egy másik runbookból a [az Azure Automation runbookjai gyermek](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="9b41c-168">If you need tooprovide a value for a complex parameter, then you must call it inline from another runbook as described in [Child runbooks in Azure Automation](automation-child-runbooks.md).</span></span>

<span data-ttu-id="9b41c-169">Azure Automation webszolgáltatás hello paramétereket a bizonyos adattípusokkal hello a következő részekben leírtak szerint a speciális funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="9b41c-169">hello Azure Automation web service will provide special functionality for parameters using certain data types as described in hello following sections.</span></span>

### <a name="named-values"></a><span data-ttu-id="9b41c-170">Névvel ellátott értékek</span><span class="sxs-lookup"><span data-stu-id="9b41c-170">Named values</span></span>
<span data-ttu-id="9b41c-171">Ha hello paraméter adattípusa [objektum], akkor is használhatja a következő JSON formátumban toosend azt listája nevű értékek hello: *{Név1: 'Érték1', Name2: 'Érték2', név3: "Érték3"}*.</span><span class="sxs-lookup"><span data-stu-id="9b41c-171">If hello parameter is data type [object], then you can use hello following JSON format toosend it a list of named values: *{Name1:'Value1', Name2:'Value2', Name3:'Value3'}*.</span></span> <span data-ttu-id="9b41c-172">Ezek az értékek csak egyszerű típusok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="9b41c-172">These values must be simple types.</span></span> <span data-ttu-id="9b41c-173">hello runbook kap hello paraméterként egy [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) érték nevű tooeach megfelelő tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="9b41c-173">hello runbook will receive hello parameter as a [PSCustomObject](https://msdn.microsoft.com/library/system.management.automation.pscustomobject%28v=vs.85%29.aspx) with properties that correspond tooeach named value.</span></span>

<span data-ttu-id="9b41c-174">Fontolja meg a teszt runbook, amely fogad egy felhasználó paraméter a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9b41c-174">Consider hello following test runbook that accepts a parameter called user.</span></span>

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

<span data-ttu-id="9b41c-175">hello következő szöveg használható hello felhasználói paraméter.</span><span class="sxs-lookup"><span data-stu-id="9b41c-175">hello following text could be used for hello user parameter.</span></span>

```
{FirstName:'Joe',LastName:'Smith',RepeatCount:'2',Show:'True'}
```

<span data-ttu-id="9b41c-176">Az eredmény kimenete a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9b41c-176">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="arrays"></a><span data-ttu-id="9b41c-177">Tömbök</span><span class="sxs-lookup"><span data-stu-id="9b41c-177">Arrays</span></span>
<span data-ttu-id="9b41c-178">Ha hello paraméter egy tömb, például [array] vagy [string []], akkor hello JSON formátumban toosend követően az értékek listájának: *[érték1, érték2, érték3]*.</span><span class="sxs-lookup"><span data-stu-id="9b41c-178">If hello parameter is an array such as [array] or [string[]], then you can use hello following JSON format toosend it a list of values: *[Value1,Value2,Value3]*.</span></span> <span data-ttu-id="9b41c-179">Ezek az értékek csak egyszerű típusok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="9b41c-179">These values must be simple types.</span></span>

<span data-ttu-id="9b41c-180">Fontolja meg a teszt runbook, amely fogad egy paraméter, a következő hello *felhasználói*.</span><span class="sxs-lookup"><span data-stu-id="9b41c-180">Consider hello following test runbook that accepts a parameter called *user*.</span></span>

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

<span data-ttu-id="9b41c-181">hello következő szöveg használható hello felhasználói paraméter.</span><span class="sxs-lookup"><span data-stu-id="9b41c-181">hello following text could be used for hello user parameter.</span></span>

```
["Joe","Smith",2,true]
```

<span data-ttu-id="9b41c-182">Az eredmény kimenete a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9b41c-182">This results in hello following output.</span></span>

```
Joe
Smith
Joe
Smith
```

### <a name="credentials"></a><span data-ttu-id="9b41c-183">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="9b41c-183">Credentials</span></span>
<span data-ttu-id="9b41c-184">Ha hello paraméter adattípusa **PSCredential**, akkor megadhatja egy Azure Automation hello neve [hitelesítőadat-eszköz](automation-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="9b41c-184">If hello parameter is data type **PSCredential**, then you can provide hello name of an Azure Automation [credential asset](automation-credentials.md).</span></span> <span data-ttu-id="9b41c-185">hello runbook lekéri hello hitelesítő adat hello névvel.</span><span class="sxs-lookup"><span data-stu-id="9b41c-185">hello runbook will retrieve hello credential with hello name that you specify.</span></span>

<span data-ttu-id="9b41c-186">Fontolja meg a teszt runbook, amely fogad egy paraméter, hitelesítő adat a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9b41c-186">Consider hello following test runbook that accepts a parameter called credential.</span></span>

```
Workflow Test-Parameters
{
   param (
      [Parameter(Mandatory=$true)][PSCredential]$credential
   )
   $credential.UserName
}
```

<span data-ttu-id="9b41c-187">hello következő szöveg használható hello felhasználói paraméter, feltéve, hogy létezik egy nevű hitelesítőeszköz *a hitelesítő adatok*.</span><span class="sxs-lookup"><span data-stu-id="9b41c-187">hello following text could be used for hello user parameter assuming that there was a credential asset called *My Credential*.</span></span>

```
My Credential
```

<span data-ttu-id="9b41c-188">Feltéve, hello felhasználónév hello hitelesítő adatok a *jsmith*, az eredmény kimenete a következő hello.</span><span class="sxs-lookup"><span data-stu-id="9b41c-188">Assuming hello username in hello credential was *jsmith*, this results in hello following output.</span></span>

```
jsmith
```

## <a name="next-steps"></a><span data-ttu-id="9b41c-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b41c-189">Next steps</span></span>
* <span data-ttu-id="9b41c-190">hello runbook architektúra aktuális cikkben runbookok kezelése Azure és a helyszíni az hello hibrid forgatókönyv-feldolgozó erőforrások magas szintű áttekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="9b41c-190">hello runbook architecture in current article provides a high-level overview of runbooks managing resources in Azure and on-premises with hello Hybrid Runbook Worker.</span></span>  <span data-ttu-id="9b41c-191">Automation-forgatókönyveket az adatközpontban található végrehajtás alatt álló toolearn tekintse meg a túl[hibrid forgatókönyv-feldolgozók](automation-hybrid-runbook-worker.md).</span><span class="sxs-lookup"><span data-stu-id="9b41c-191">toolearn about executing Automation runbooks in your datacenter, refer too[Hybrid Runbook Workers](automation-hybrid-runbook-worker.md).</span></span>
* <span data-ttu-id="9b41c-192">toolearn hello más runbookok által használt egyedi vagy közös funkciók, moduláris runbookok toobe létrehozásával kapcsolatos további információkért tekintse meg a túl[Gyermekforgatókönyvek](automation-child-runbooks.md).</span><span class="sxs-lookup"><span data-stu-id="9b41c-192">toolearn more about hello creating modular runbooks toobe used by other runbooks for specific or common functions, refer too[Child Runbooks](automation-child-runbooks.md).</span></span>

