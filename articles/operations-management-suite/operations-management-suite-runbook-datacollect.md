---
title: "Egy runbookhoz, az Azure Automationben Log Analytics-adatok gyűjtése |} Microsoft Docs"
description: "Lépésenkénti útmutató, amely végigvezeti egy runbook létrehozása az Azure Automation szolgáltatásban, hogy a OMS tárházba elemzés, Log Analyticshez úgy gyűjthet adatokat."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 59f674c9c6404da7f5384539189f41a4ba1a939a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="17c22-103">Egy Azure Automation-runbook Naplóelemzési az adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="17c22-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="17c22-104">Jelentős mennyiségű Naplóelemzési adatokat gyűjteni különböző forrásokból, beleértve a [adatforrások](../log-analytics/log-analytics-data-sources.md) az ügynökökre és is [adatgyűjtés az Azure-ból](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="17c22-105">Nincsenek olyan forgatókönyvek, ha az adatok gyűjtéséhez kell, amely nem érhető el a szabványos forrásokon keresztül.</span><span class="sxs-lookup"><span data-stu-id="17c22-105">There are a scenarios though where you need to collect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="17c22-106">Ebben az esetben is használhatja a [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md) Naplóelemzési bármely REST API-ügyfél adatokat írni.</span><span class="sxs-lookup"><span data-stu-id="17c22-106">In these cases, you can use the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) to write data to Log Analytics from any REST API client.</span></span>  <span data-ttu-id="17c22-107">Az adatgyűjtés elvégzéséhez gyakran használják az Azure Automationben egy runbook használ.</span><span class="sxs-lookup"><span data-stu-id="17c22-107">A common method to perform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="17c22-108">Ez az oktatóanyag végigvezeti a folyamat létrehozásának és az Azure Automationben adatokat írni a Naplóelemzési runbook ütemezése.</span><span class="sxs-lookup"><span data-stu-id="17c22-108">This tutorial walks through the process for creating and scheduling a runbook in Azure Automation to write data to Log Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="17c22-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="17c22-109">Prerequisites</span></span>
<span data-ttu-id="17c22-110">Ebben a forgatókönyvben a következő erőforrások az Azure-előfizetéshez konfigurált igényel.</span><span class="sxs-lookup"><span data-stu-id="17c22-110">This scenario requires the following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="17c22-111">Egy ingyenes fiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="17c22-111">Both can be a free account.</span></span>

- <span data-ttu-id="17c22-112">[A Naplóelemzési munkaterület jelentkezzen](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="17c22-113">[Azure automation-fiók](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="17c22-114">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="17c22-114">Overview of scenario</span></span>
<span data-ttu-id="17c22-115">Ebben az oktatóanyagban automatizálási feladatok adatokat gyűjt a runbook fog írni.</span><span class="sxs-lookup"><span data-stu-id="17c22-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="17c22-116">Az Azure Automation Runbookjai PowerShell használatával valósíthatók meg, írása, és egy parancsfájl tesztelése az Azure Automation-szerkesztőben kezdi.</span><span class="sxs-lookup"><span data-stu-id="17c22-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in the Azure Automation editor.</span></span>  <span data-ttu-id="17c22-117">Miután ellenőrizte, hogy a szükséges adatokat gyűjt, fogja, hogy adatokat írjon Naplóelemzési, és ellenőrizze, az egyéni adattípus.</span><span class="sxs-lookup"><span data-stu-id="17c22-117">Once you verify that you're collecting the required information, you'll write that data to Log Analytics and verify the custom data type.</span></span>  <span data-ttu-id="17c22-118">Végezetül létre fog hozni egy ütemezést, rendszeres időközönként a runbook elindításához.</span><span class="sxs-lookup"><span data-stu-id="17c22-118">Finally, you'll create a schedule to start the runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="17c22-119">Azure Automation-feladat információinak küldendő Log Analytics ezt a runbookot nélkül is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="17c22-119">You can configure Azure Automation to send job information to Log Analytics without this runbook.</span></span>  <span data-ttu-id="17c22-120">Ebben a forgatókönyvben elsősorban az oktatóanyag támogatásához, és elküldheti az adatokat egy tesztelési munkaterület ajánlott.</span><span class="sxs-lookup"><span data-stu-id="17c22-120">This scenario is primarily used to support the tutorial, and it's recommended that you send the data to a test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="17c22-121">1. Data Collector API-modul telepítése</span><span class="sxs-lookup"><span data-stu-id="17c22-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="17c22-122">Minden [kérelem érkezett a HTTP-adatokat gyűjtő API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) megfelelően formázva, és egy authorization fejlécet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="17c22-122">Every [request from the HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="17c22-123">Ehhez a runbookban, de a kódot, amely leegyszerűsíti ezt a folyamatot modul használatával mennyisége csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="17c22-123">You can do this in your runbook, but you can reduce the amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="17c22-124">Egy modul, melyekkel [OMSIngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI) a PowerShell-galériában.</span><span class="sxs-lookup"><span data-stu-id="17c22-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in the PowerShell Gallery.</span></span>

<span data-ttu-id="17c22-125">Használatához a [modul](../automation/automation-integration-modules.md) egy runbookban, telepítenie kell az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="17c22-125">To use a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="17c22-126">Minden runbook ugyanazt a fiókot használhatja a funkciók a modulban.</span><span class="sxs-lookup"><span data-stu-id="17c22-126">Any runbook in the same account can then use the functions in the module.</span></span>  <span data-ttu-id="17c22-127">Új modul választásával telepítheti **eszközök** > **modulok** > **a modul hozzá lesz adva** az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="17c22-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="17c22-128">A PowerShell-galériában azonban lehetővé teszi a gyors kapcsolóval kell telepítenie egy modul közvetlenül az automation-fiók, hogy használhassa ezt a beállítást a jelen oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="17c22-128">The PowerShell Gallery though gives you a quick option to deploy a module directly to your automation account so you can use that option for this tutorial.</span></span>  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="17c22-130">Ugrás a [PowerShell-galériában](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="17c22-130">Go to [PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="17c22-131">Keresse meg **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="17c22-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="17c22-132">Kattintson a **központi telepítése az Azure Automation** gombra.</span><span class="sxs-lookup"><span data-stu-id="17c22-132">Click on the **Deploy to Azure Automation** button.</span></span>
4. <span data-ttu-id="17c22-133">Válassza ki az automation-fiók, és kattintson a **OK** -modul telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="17c22-133">Select your automation account and click **OK** to install the module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="17c22-134">2. Automatizálási változók létrehozása</span><span class="sxs-lookup"><span data-stu-id="17c22-134">2. Create Automation variables</span></span>
<span data-ttu-id="17c22-135">[Automatizálási változók](..\automation\automation-variables.md) az Automation-fiók minden forgatókönyve használt értékek tárolásához.</span><span class="sxs-lookup"><span data-stu-id="17c22-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="17c22-136">Akkor runbookok rugalmasabb azáltal, hogy ezek az értékek módosítása az aktuális runbook szerkesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="17c22-136">They make runbooks more flexible by allowing you to change these values without editing the actual runbook.</span></span> <span data-ttu-id="17c22-137">A HTTP-adatokat gyűjtő API minden kérelemnél szükséges azonosítója és kulcsa az OMS-munkaterület, és a változó eszközök ideálisak ezek az információk tárolására.</span><span class="sxs-lookup"><span data-stu-id="17c22-137">Every request from the HTTP Data Collector API requires the ID and key of the OMS workspace, and variable assets are ideal to store this information.</span></span>  

![Változók](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="17c22-139">Az Azure-portálon lépjen az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="17c22-139">In the Azure portal, navigate to your Automation account.</span></span>
2. <span data-ttu-id="17c22-140">Válassza ki **változók** alatt **megosztott erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="17c22-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="17c22-141">Kattintson a **változó hozzáadása** , és hozzon létre két változót a következő táblázatban.</span><span class="sxs-lookup"><span data-stu-id="17c22-141">Click **Add a variable** and create the two variables in the following table.</span></span>

| <span data-ttu-id="17c22-142">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17c22-142">Property</span></span> | <span data-ttu-id="17c22-143">Munkaterület-azonosító értéke</span><span class="sxs-lookup"><span data-stu-id="17c22-143">Workspace ID Value</span></span> | <span data-ttu-id="17c22-144">Munkaterület-kulcs értéke</span><span class="sxs-lookup"><span data-stu-id="17c22-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="17c22-145">Név</span><span class="sxs-lookup"><span data-stu-id="17c22-145">Name</span></span> | <span data-ttu-id="17c22-146">WorkspaceId</span><span class="sxs-lookup"><span data-stu-id="17c22-146">WorkspaceId</span></span> | <span data-ttu-id="17c22-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="17c22-147">WorkspaceKey</span></span> |
| <span data-ttu-id="17c22-148">Típus</span><span class="sxs-lookup"><span data-stu-id="17c22-148">Type</span></span> | <span data-ttu-id="17c22-149">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="17c22-149">String</span></span> | <span data-ttu-id="17c22-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="17c22-150">String</span></span> |
| <span data-ttu-id="17c22-151">Érték</span><span class="sxs-lookup"><span data-stu-id="17c22-151">Value</span></span> | <span data-ttu-id="17c22-152">Illessze be a Naplóelemzési munkaterület a munkaterület azonosítója.</span><span class="sxs-lookup"><span data-stu-id="17c22-152">Paste in the Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="17c22-153">Illessze be kell jelentkezniük az elsődleges vagy másodlagos kulcsot a Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="17c22-153">Paste in with the Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="17c22-154">Titkosított</span><span class="sxs-lookup"><span data-stu-id="17c22-154">Encrypted</span></span> | <span data-ttu-id="17c22-155">Nem</span><span class="sxs-lookup"><span data-stu-id="17c22-155">No</span></span> | <span data-ttu-id="17c22-156">Igen</span><span class="sxs-lookup"><span data-stu-id="17c22-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="17c22-157">3. Runbook létrehozása</span><span class="sxs-lookup"><span data-stu-id="17c22-157">3. Create runbook</span></span>

<span data-ttu-id="17c22-158">Azure Automation szolgáltatásbeli szerkesztővé rendelkezik a portálon, ahol szerkesztheti, és tesztelje a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="17c22-158">Azure Automation has an editor in the portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="17c22-159">Lehetősége van a parancsprogram-szerkesztő segítségével dolgozhat [PowerShell közvetlenül](../automation/automation-edit-textual-runbook.md) vagy [létrehozhat egy grafikus](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-159">You have the option to use the script editor to work with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="17c22-160">Ebben az oktatóanyagban a PowerShell-parancsfájllal fog működni.</span><span class="sxs-lookup"><span data-stu-id="17c22-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Forgatókönyv szerkesztése](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="17c22-162">Nyissa meg az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="17c22-162">Navigate to your Automation account.</span></span>  
2. <span data-ttu-id="17c22-163">Kattintson a **Runbookok** > **hozzáadása egy runbook** > **hozzon létre egy új runbookot**.</span><span class="sxs-lookup"><span data-stu-id="17c22-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="17c22-164">A runbook nevét, írja be a következőt **gyűjtése-Automation-feladat**.</span><span class="sxs-lookup"><span data-stu-id="17c22-164">For the runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="17c22-165">Válassza ki a runbook típusa **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="17c22-165">For the runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="17c22-166">Kattintson a **létrehozása** hozza létre a runbookot és a szerkesztő elindításához.</span><span class="sxs-lookup"><span data-stu-id="17c22-166">Click **Create** to create the runbook and start the editor.</span></span>
5. <span data-ttu-id="17c22-167">Másolja és illessze be a következő kódot a runbookot.</span><span class="sxs-lookup"><span data-stu-id="17c22-167">Copy and paste the following code into the runbook.</span></span>  <span data-ttu-id="17c22-168">Tekintse meg a megjegyzéseket, a kód ismertetése a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="17c22-168">Refer to the comments in the script for explanation of the code.</span></span>
    
        # Get information required for the automation account from parameter values when the runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate to the Automation account using the Azure connection created when the Automation account was created.
        # Code copied from the runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set the $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set the name of the record type.
        $logType = "AutomationJob"
        
        # Get the jobs from the past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert the job data to json
            $body = $jobs | ConvertTo-Json
        
            # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
            Write-Verbose $body
        
            # Send the data to Log Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="17c22-169">4. Runbook tesztelése</span><span class="sxs-lookup"><span data-stu-id="17c22-169">4. Test runbook</span></span>
<span data-ttu-id="17c22-170">Azure Automation szolgáltatásbeli tartalmaz egy környezet [tesztelje a forgatókönyvet](../automation/automation-testing-runbook.md) közzététel előtt.</span><span class="sxs-lookup"><span data-stu-id="17c22-170">Azure Automation includes an environment to [test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="17c22-171">Vizsgálja meg a runbook által gyűjtött adatokat, és győződjön meg arról, hogy ír Naplóelemzési éles történő közzététel előtt várt módon.</span><span class="sxs-lookup"><span data-stu-id="17c22-171">You can inspect the data collected by the runbook and verify that it writes to Log Analytics as expected before publishing it to production.</span></span> 
 
![Runbook tesztelése](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="17c22-173">Kattintson a **mentése** menteni a runbookot.</span><span class="sxs-lookup"><span data-stu-id="17c22-173">Click **Save** to save the runbook.</span></span>
1. <span data-ttu-id="17c22-174">Kattintson a **teszt ablaktábla** való nyissa meg a runbookot a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="17c22-174">Click **Test pane** to open the runbook in the test environment.</span></span>
3. <span data-ttu-id="17c22-175">A runbook paraméterekkel rendelkezik, mivel érték megadására kéri.</span><span class="sxs-lookup"><span data-stu-id="17c22-175">Since your runbook has parameters, you're prompted to enter values for them.</span></span>  <span data-ttu-id="17c22-176">Adja meg az erőforráscsoport nevét, és az automatizálás figyelembe, hogy a folyamatban lévő feladat adatainak gyűjtéséről.</span><span class="sxs-lookup"><span data-stu-id="17c22-176">Enter the name of the resource group and the automation account that your going to collect job information from.</span></span>
4. <span data-ttu-id="17c22-177">Kattintson a **Start** a Start a runbookot.</span><span class="sxs-lookup"><span data-stu-id="17c22-177">Click **Start** to the start the runbook.</span></span>
3. <span data-ttu-id="17c22-178">A runbook elindul állapotú **várakozik** előtt kerül **futtató**.</span><span class="sxs-lookup"><span data-stu-id="17c22-178">The runbook will start with a status of **Queued** before it goes to **Running**.</span></span>  
3. <span data-ttu-id="17c22-179">A runbook részletes kimenet megjelenjen a feladatok gyűjtött json formátumban.</span><span class="sxs-lookup"><span data-stu-id="17c22-179">The runbook should display verbose output with the jobs collected in json format.</span></span>  <span data-ttu-id="17c22-180">Ha nincsenek feladatok jelenik meg, majd lépett nincsenek feladatok az elmúlt órában az automation-fiókban létrehozni.</span><span class="sxs-lookup"><span data-stu-id="17c22-180">If no jobs are listed, then there may have been no jobs created in the automation account in the last hour.</span></span>  <span data-ttu-id="17c22-181">Minden olyan forgatókönyvben indítsa el az automation-fiókban, és végezze el ismét a vizsgálatot.</span><span class="sxs-lookup"><span data-stu-id="17c22-181">Try starting any runbook in the automation account and then perform the test again.</span></span>
4. <span data-ttu-id="17c22-182">Gondoskodjon arról, hogy a kimeneti szolgáltatáshoz nem mutatja ki a hibákat a post parancsot.</span><span class="sxs-lookup"><span data-stu-id="17c22-182">Ensure that the output doesn't show any errors in the post command to Log Analytics.</span></span>  <span data-ttu-id="17c22-183">A következőhöz hasonló üzenetet kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="17c22-183">You should have a message similar to the following.</span></span>

    ![POST kimeneti](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="17c22-185">5. A Naplóelemzési rekordjainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="17c22-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="17c22-186">Ha a runbook-teszt befejeződött, és ellenőrizte, hogy a kimeneti sikeresen megérkezett, ellenőrizheti, a rekordok használatával lettek létrehozva egy [napló keresés a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-186">Once the runbook has completed in test, and you verified that the output was successfully received, you can verify that the records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Kimenet](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="17c22-188">Válassza ki a Naplóelemzési munkaterület az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="17c22-188">In the Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="17c22-189">Kattintson a **keresési jelentkezzen**.</span><span class="sxs-lookup"><span data-stu-id="17c22-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="17c22-190">Írja be a következő parancsot `Type=AutomationJob_CL` , majd kattintson a Keresés gombra.</span><span class="sxs-lookup"><span data-stu-id="17c22-190">Type the following command `Type=AutomationJob_CL` and click the search button.</span></span> <span data-ttu-id="17c22-191">Vegye figyelembe, hogy a rekordtípust, amely nincs meghatározva a parancsfájl _CL tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="17c22-191">Note that the record type includes _CL that isn't specified in the script.</span></span>  <span data-ttu-id="17c22-192">Adott utótagot automatikusan fűz hozzá a hibanapló-típus azt jelzi, hogy az egy egyéni napló típusa.</span><span class="sxs-lookup"><span data-stu-id="17c22-192">That suffix is automatically appended to the log type to indicate that it's a custom log type.</span></span>
4. <span data-ttu-id="17c22-193">Meg kell jelennie egy vagy több rekordot adott vissza, amely azt jelzi, hogy a várt módon működik-e a runbookot.</span><span class="sxs-lookup"><span data-stu-id="17c22-193">You should see one or more records returned indicating that the runbook is working as expected.</span></span>


## <a name="6-publish-the-runbook"></a><span data-ttu-id="17c22-194">6. A runbook közzététele</span><span class="sxs-lookup"><span data-stu-id="17c22-194">6. Publish the runbook</span></span>
<span data-ttu-id="17c22-195">Miután ellenőrizte, hogy, hogy a runbook megfelelően működik-e, kell közzétenni ahhoz az éles környezetben futtatható.</span><span class="sxs-lookup"><span data-stu-id="17c22-195">Once you've verified that the runbook is working correctly, you need to publish it so you can run it in production.</span></span>  <span data-ttu-id="17c22-196">Továbbra is szerkesztése és a runbook tesztelése a közzétett változatban módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="17c22-196">You can continue to edit and test the runbook without modifying the published version.</span></span>  

![Runbook közzététele](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="17c22-198">Térjen vissza az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="17c22-198">Return to your automation account.</span></span>
2. <span data-ttu-id="17c22-199">Kattintson a **Runbookok** válassza **gyűjtése-Automation-feladat**.</span><span class="sxs-lookup"><span data-stu-id="17c22-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="17c22-200">Kattintson a **szerkesztése** , majd **közzététele**.</span><span class="sxs-lookup"><span data-stu-id="17c22-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="17c22-201">Kattintson a **Igen** Ha kéri, hogy ellenőrizze, hogy szeretné-e felülírja a korábban közzétett verziót.</span><span class="sxs-lookup"><span data-stu-id="17c22-201">Click **Yes** when asked to verify that you want to overwrite the previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="17c22-202">7. Naplózási beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="17c22-202">7. Set logging options</span></span> 
<span data-ttu-id="17c22-203">A vizsgálat sikerült megtekintéséhez [részletes kimenet](../automation/automation-runbook-output-and-messages.md#message-streams) , mert a parancsfájl a $VerbosePreference változó beállítása.</span><span class="sxs-lookup"><span data-stu-id="17c22-203">For test, you were able to view [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set the $VerbosePreference variable in the script.</span></span>  <span data-ttu-id="17c22-204">Termelési környezetben a runbook naplózási tulajdonságainak beállításához, ha meg szeretné jeleníteni a részletes kimenet kell.</span><span class="sxs-lookup"><span data-stu-id="17c22-204">For production, you need to set the logging properties for the runbook if you want to view verbose output.</span></span>  <span data-ttu-id="17c22-205">Az ebben az oktatóanyagban használt runbookot Ez a json-adatokat küldi el a Naplóelemzési jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="17c22-205">For the runbook used in this tutorial, this will display the json data being sent to Log Analytics.</span></span>

![Naplózás és nyomkövetés](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="17c22-207">Válassza ki a runbook tulajdonságok **naplózás és nyomkövetés** alatt **Runbookbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="17c22-207">In the properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="17c22-208">A beállításának módosítása **részletes rekordok naplózására** való **a**.</span><span class="sxs-lookup"><span data-stu-id="17c22-208">Change the setting for **Log verbose records** to **On**.</span></span>
3. <span data-ttu-id="17c22-209">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="17c22-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="17c22-210">8. Runbook ütemezése</span><span class="sxs-lookup"><span data-stu-id="17c22-210">8. Schedule runbook</span></span>
<span data-ttu-id="17c22-211">A leggyakoribb figyelési adatokat összegyűjtő runbook-indítási módja ütemezése úgy, hogy automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="17c22-211">The most common way to start a runbook that collects monitoring data is to schedule it to run automatically.</span></span>  <span data-ttu-id="17c22-212">Hozzon létre ehhez a [Azure Automation ütemezési](../automation/automation-schedules.md) és a runbookban való csatlakoztatás.</span><span class="sxs-lookup"><span data-stu-id="17c22-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it to your runbook.</span></span>

![Runbook ütemezése](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="17c22-214">Válassza ki a runbook tulajdonságok, **ütemezések** alatt **erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="17c22-214">In the properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="17c22-215">Kattintson a **ütemezés hozzáadása** > **ütemezés kapcsolása a forgatókönyvhöz** > **hozzon létre egy új ütemezést**.</span><span class="sxs-lookup"><span data-stu-id="17c22-215">Click **Add a schedule** > **Link a schedule to your runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="17c22-216">Adja meg az ütemezés a következő értékeket, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="17c22-216">Type in the following values for the schedule and click **Create**.</span></span>

| <span data-ttu-id="17c22-217">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="17c22-217">Property</span></span> | <span data-ttu-id="17c22-218">Érték</span><span class="sxs-lookup"><span data-stu-id="17c22-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="17c22-219">Név</span><span class="sxs-lookup"><span data-stu-id="17c22-219">Name</span></span> | <span data-ttu-id="17c22-220">AutomationJobs-óránként</span><span class="sxs-lookup"><span data-stu-id="17c22-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="17c22-221">Indítása</span><span class="sxs-lookup"><span data-stu-id="17c22-221">Starts</span></span> | <span data-ttu-id="17c22-222">Válassza ki, amikor legalább 5 perccel későbbi, mint a jelenlegi időpont.</span><span class="sxs-lookup"><span data-stu-id="17c22-222">Select any time at least 5 minutes past the current time.</span></span> |
| <span data-ttu-id="17c22-223">Ismétlődés</span><span class="sxs-lookup"><span data-stu-id="17c22-223">Recurrence</span></span> | <span data-ttu-id="17c22-224">Ismétlődő</span><span class="sxs-lookup"><span data-stu-id="17c22-224">Recurring</span></span> |
| <span data-ttu-id="17c22-225">Ismétlődik minden</span><span class="sxs-lookup"><span data-stu-id="17c22-225">Recur every</span></span> | <span data-ttu-id="17c22-226">1 óra</span><span class="sxs-lookup"><span data-stu-id="17c22-226">1 hour</span></span> |
| <span data-ttu-id="17c22-227">Készlet lejárati</span><span class="sxs-lookup"><span data-stu-id="17c22-227">Set expiration</span></span> | <span data-ttu-id="17c22-228">Nem</span><span class="sxs-lookup"><span data-stu-id="17c22-228">No</span></span> |

<span data-ttu-id="17c22-229">Az ütemezés létrehozása után kell beállítani, amely minden alkalommal, amikor az ütemezés elindítja a runbookot használjuk paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="17c22-229">Once the schedule is created, you need to set the parameter values that will be used each time this schedule starts the runbook.</span></span>

6. <span data-ttu-id="17c22-230">Kattintson a **konfigurálása paraméterek és futtatási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="17c22-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="17c22-231">Adja meg az értékeket a **ResourceGroupName** és **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="17c22-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="17c22-232">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="17c22-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="17c22-233">9. Ellenőrizze a runbook elindul az ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="17c22-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="17c22-234">Egy runbook indítását, Everytime [feladat jön létre](../automation/automation-runbook-execution.md) és a kimenetet a naplóba.</span><span class="sxs-lookup"><span data-stu-id="17c22-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="17c22-235">Valójában ezek a ugyanazt a feladatot, amely runbook gyűjt.</span><span class="sxs-lookup"><span data-stu-id="17c22-235">In fact, these are the same jobs that the runbook is collecting.</span></span>  <span data-ttu-id="17c22-236">Ellenőrizheti, hogy a runbook által a runbookhoz tartozó feladatok után az ütemezés kezdési idejét megfelelően elindul-e.</span><span class="sxs-lookup"><span data-stu-id="17c22-236">You can verify that the runbook starts as expected by checking the jobs for the runbook after the start time for the schedule has passed.</span></span>

![Feladatok](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="17c22-238">Válassza ki a runbook tulajdonságok, **feladatok** alatt **erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="17c22-238">In the properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="17c22-239">Minden alkalommal, amikor a runbook elindításának feladatok listáját kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="17c22-239">You should see a listing of jobs for each time the runbook was started.</span></span>
3. <span data-ttu-id="17c22-240">Kattintson az egyik feladatához a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="17c22-240">Click on one of the jobs to view its details.</span></span>
4. <span data-ttu-id="17c22-241">Kattintson a **összes napló** a naplók megtekintéséhez, és kimeneti a runbookból.</span><span class="sxs-lookup"><span data-stu-id="17c22-241">Click on **All logs** to view the logs and output from the runbook.</span></span>
5. <span data-ttu-id="17c22-242">Görgessen az alsó hasonló az alábbi képen található bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="17c22-242">Scroll to the bottom to find an entry similar to the image below.</span></span><br>![Részletes](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="17c22-244">Ez a bejegyzés Naplóelemzési küldött json részletes adatainak megtekintéséhez kattintson.</span><span class="sxs-lookup"><span data-stu-id="17c22-244">Click on this entry to view the detailed json data  that was sent to Log Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="17c22-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17c22-245">Next steps</span></span>
- <span data-ttu-id="17c22-246">Használjon [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) a Naplóelemzési tárház összegyűjtött adatokat megjelenítő nézet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="17c22-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) to create a view displaying the data that you've collected to the Log Analytics repository.</span></span>
- <span data-ttu-id="17c22-247">A runbookot a csomag egy [felügyeleti megoldás](operations-management-suite-solutions-creating.md) terjeszteni az ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="17c22-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) to distribute to customers.</span></span>
- <span data-ttu-id="17c22-248">További információ [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="17c22-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="17c22-249">További információ [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="17c22-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="17c22-250">További információ a [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="17c22-250">Learn more about the [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>