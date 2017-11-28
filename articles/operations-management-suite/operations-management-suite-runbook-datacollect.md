---
title: Log Analytics-adatok egy runbookhoz, az Azure Automationben aaaCollecting |} Microsoft Docs
description: "Lépésenkénti útmutató, amely végigvezeti egy runbook létrehozása az Azure Automation toocollect adatok történő hello OMS tárházba Naplóelemzési analízis."
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
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a><span data-ttu-id="7d2a1-103">Egy Azure Automation-runbook Naplóelemzési az adatok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-103">Collect data in Log Analytics with an Azure Automation runbook</span></span>
<span data-ttu-id="7d2a1-104">Jelentős mennyiségű Naplóelemzési adatokat gyűjteni különböző forrásokból, beleértve a [adatforrások](../log-analytics/log-analytics-data-sources.md) az ügynökökre és is [adatgyűjtés az Azure-ból](../log-analytics/log-analytics-azure-storage.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-104">You can collect a significant amount of data in Log Analytics from a variety of sources including [data sources](../log-analytics/log-analytics-data-sources.md) on agents and also [data collected from Azure](../log-analytics/log-analytics-azure-storage.md).</span></span>  <span data-ttu-id="7d2a1-105">Vannak olyan helyzetek toocollect adatokat kell, amely nem érhető el a szabványos források keresztül, ha.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-105">There are a scenarios though where you need toocollect data that isn't accessible through these standard sources.</span></span>  <span data-ttu-id="7d2a1-106">Ebben az esetben használhatja hello [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md) toowrite adatok tooLog Analytics bármely REST API-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-106">In these cases, you can use hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog Analytics from any REST API client.</span></span>  <span data-ttu-id="7d2a1-107">Egy általános metódus tooperform ezen adatok gyűjtése az Azure Automationben használ egy runbookot.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-107">A common method tooperform this data collection is using a runbook in Azure Automation.</span></span>   

<span data-ttu-id="7d2a1-108">Ez az oktatóanyag bemutatja, hogyan létrehozásához és az Azure Automation toowrite adatok tooLog Analytics runbook ütemezése hello folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-108">This tutorial walks through hello process for creating and scheduling a runbook in Azure Automation toowrite data tooLog Analytics.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7d2a1-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d2a1-109">Prerequisites</span></span>
<span data-ttu-id="7d2a1-110">Ebben a forgatókönyvben a következő erőforrások az Azure-előfizetéshez konfigurált hello igényel.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-110">This scenario requires hello following resources configured in your Azure subscription.</span></span>  <span data-ttu-id="7d2a1-111">Egy ingyenes fiókot is lehet.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-111">Both can be a free account.</span></span>

- <span data-ttu-id="7d2a1-112">[A Naplóelemzési munkaterület jelentkezzen](../log-analytics/log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-112">[Log Analytics workspace](../log-analytics/log-analytics-get-started.md).</span></span>
- <span data-ttu-id="7d2a1-113">[Azure automation-fiók](../automation/automation-offering-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-113">[Azure automation account](../automation/automation-offering-get-started.md).</span></span>

## <a name="overview-of-scenario"></a><span data-ttu-id="7d2a1-114">Forgatókönyv áttekintése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-114">Overview of scenario</span></span>
<span data-ttu-id="7d2a1-115">Ebben az oktatóanyagban automatizálási feladatok adatokat gyűjt a runbook fog írni.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-115">For this tutorial, you'll write a runbook that collects information about Automation jobs.</span></span>  <span data-ttu-id="7d2a1-116">Az Azure Automation Runbookjai PowerShell használatával valósíthatók meg, írása, és egy parancsfájl tesztelése hello Azure Automation-szerkesztőben kezdi.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-116">Runbooks in Azure Automation are implemented with PowerShell, so you'll start by writing and testing a script in hello Azure Automation editor.</span></span>  <span data-ttu-id="7d2a1-117">Miután ellenőrizte, hogy hello szükséges információkat gyűjt, fogja, hogy adatokat tooLog Analytics írása, és ellenőrizze, hello egyéni adattípus.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-117">Once you verify that you're collecting hello required information, you'll write that data tooLog Analytics and verify hello custom data type.</span></span>  <span data-ttu-id="7d2a1-118">Végezetül a rendszeres időközönként létre fog hozni egy ütemezés toostart hello runbookot.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-118">Finally, you'll create a schedule toostart hello runbook at regular intervals.</span></span>

> [!NOTE]
> <span data-ttu-id="7d2a1-119">Azure Automation toosend feladat információk tooLog Analytics ezt a runbookot nélkül is konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-119">You can configure Azure Automation toosend job information tooLog Analytics without this runbook.</span></span>  <span data-ttu-id="7d2a1-120">Ebben a forgatókönyvben elsősorban a használt toosupport hello oktatóanyag, és javasolt, hogy küldjön hello adatok tooa teszt munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-120">This scenario is primarily used toosupport hello tutorial, and it's recommended that you send hello data tooa test workspace.</span></span>  


## <a name="1-install-data-collector-api-module"></a><span data-ttu-id="7d2a1-121">1. Data Collector API-modul telepítése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-121">1. Install Data Collector API module</span></span>
<span data-ttu-id="7d2a1-122">Minden [hello HTTP adatait gyűjtője API-kéréseket](../log-analytics/log-analytics-data-collector-api.md#create-a-request) megfelelően formázva, és egy authorization fejlécet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-122">Every [request from hello HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md#create-a-request) must be formatted appropriately and include an authorization header.</span></span>  <span data-ttu-id="7d2a1-123">Ehhez a runbookban, de a kódot, amely leegyszerűsíti ezt a folyamatot modul használatával hello mennyisége csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-123">You can do this in your runbook, but you can reduce hello amount of code required by using a module that simplifies this process.</span></span>  <span data-ttu-id="7d2a1-124">Egy modul, melyekkel [OMSIngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI) a PowerShell-galériában hello.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-124">One module that you can use is [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI) in hello PowerShell Gallery.</span></span>

<span data-ttu-id="7d2a1-125">toouse egy [modul](../automation/automation-integration-modules.md) egy runbookban, telepítenie kell az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-125">toouse a [module](../automation/automation-integration-modules.md) in a runbook, it must be installed in your Automation account.</span></span>  <span data-ttu-id="7d2a1-126">Minden olyan forgatókönyvben hello ugyanazt a fiókot használhatja a hello funkciók hello modulban.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-126">Any runbook in hello same account can then use hello functions in hello module.</span></span>  <span data-ttu-id="7d2a1-127">Új modul választásával telepítheti **eszközök** > **modulok** > **a modul hozzá lesz adva** az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-127">You can install a new module by selecting **Assets** > **Modules** > **Add a module** in your Automation account.</span></span>  

<span data-ttu-id="7d2a1-128">PowerShell-galériában hello azonban lehetővé teszi egy gyors beállítás toodeploy modul közvetlen tooyour automatizálási fiókot, hogy használhassa ezt a beállítást a jelen oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-128">hello PowerShell Gallery though gives you a quick option toodeploy a module directly tooyour automation account so you can use that option for this tutorial.</span></span>  

![OMSIngestionAPI modul](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. <span data-ttu-id="7d2a1-130">Nyissa meg túl[PowerShell-galériában](https://www.powershellgallery.com/).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-130">Go too[PowerShell Gallery](https://www.powershellgallery.com/).</span></span>
2. <span data-ttu-id="7d2a1-131">Keresse meg **OMSIngestionAPI**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-131">Search for **OMSIngestionAPI**.</span></span>
3. <span data-ttu-id="7d2a1-132">Kattintson a hello **tooAzure automatizálás telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-132">Click on hello **Deploy tooAzure Automation** button.</span></span>
4. <span data-ttu-id="7d2a1-133">Válassza ki az automation-fiók, és kattintson a **OK** tooinstall hello modul.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-133">Select your automation account and click **OK** tooinstall hello module.</span></span>


## <a name="2-create-automation-variables"></a><span data-ttu-id="7d2a1-134">2. Automatizálási változók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d2a1-134">2. Create Automation variables</span></span>
<span data-ttu-id="7d2a1-135">[Automatizálási változók](..\automation\automation-variables.md) az Automation-fiók minden forgatókönyve használt értékek tárolásához.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-135">[Automation variables](..\automation\automation-variables.md) hold values that can be used by all runbooks in your Automation account.</span></span>  <span data-ttu-id="7d2a1-136">Akkor runbookok több rugalmas lehetővé tételével toochange ezeket az értékeket szerkesztése nélkül hello tényleges runbook.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-136">They make runbooks more flexible by allowing you toochange these values without editing hello actual runbook.</span></span> <span data-ttu-id="7d2a1-137">Hello HTTP adatait gyűjtője API származó kérelmek hello azonosítója és kulcsa hello OMS-munkaterület szükséges, és változó eszközök ideális toostore ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-137">Every request from hello HTTP Data Collector API requires hello ID and key of hello OMS workspace, and variable assets are ideal toostore this information.</span></span>  

![Változók](media/operations-management-suite-runbook-datacollect/variables.png)

1. <span data-ttu-id="7d2a1-139">Hello Azure-portálon lépjen a tooyour Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-139">In hello Azure portal, navigate tooyour Automation account.</span></span>
2. <span data-ttu-id="7d2a1-140">Válassza ki **változók** alatt **megosztott erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-140">Select **Variables** under **Shared Resources**.</span></span>
2. <span data-ttu-id="7d2a1-141">Kattintson a **változó hozzáadása** és hello két változó létrehozása a következő táblázat hello.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-141">Click **Add a variable** and create hello two variables in hello following table.</span></span>

| <span data-ttu-id="7d2a1-142">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7d2a1-142">Property</span></span> | <span data-ttu-id="7d2a1-143">Munkaterület-azonosító értéke</span><span class="sxs-lookup"><span data-stu-id="7d2a1-143">Workspace ID Value</span></span> | <span data-ttu-id="7d2a1-144">Munkaterület-kulcs értéke</span><span class="sxs-lookup"><span data-stu-id="7d2a1-144">Workspace Key Value</span></span> |
|:--|:--|:--|
| <span data-ttu-id="7d2a1-145">Név</span><span class="sxs-lookup"><span data-stu-id="7d2a1-145">Name</span></span> | <span data-ttu-id="7d2a1-146">WorkspaceId</span><span class="sxs-lookup"><span data-stu-id="7d2a1-146">WorkspaceId</span></span> | <span data-ttu-id="7d2a1-147">WorkspaceKey</span><span class="sxs-lookup"><span data-stu-id="7d2a1-147">WorkspaceKey</span></span> |
| <span data-ttu-id="7d2a1-148">Típus</span><span class="sxs-lookup"><span data-stu-id="7d2a1-148">Type</span></span> | <span data-ttu-id="7d2a1-149">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d2a1-149">String</span></span> | <span data-ttu-id="7d2a1-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d2a1-150">String</span></span> |
| <span data-ttu-id="7d2a1-151">Érték</span><span class="sxs-lookup"><span data-stu-id="7d2a1-151">Value</span></span> | <span data-ttu-id="7d2a1-152">Illessze be a munkaterület azonosítója a Naplóelemzési munkaterület hello.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-152">Paste in hello Workspace ID of your Log Analytics workspace.</span></span> | <span data-ttu-id="7d2a1-153">Illessze be kell jelentkezniük az hello elsődleges vagy másodlagos kulcsot a Naplóelemzési munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-153">Paste in with hello Primary or Secondary Key of your Log Analytics workspace.</span></span> |
| <span data-ttu-id="7d2a1-154">Titkosított</span><span class="sxs-lookup"><span data-stu-id="7d2a1-154">Encrypted</span></span> | <span data-ttu-id="7d2a1-155">Nem</span><span class="sxs-lookup"><span data-stu-id="7d2a1-155">No</span></span> | <span data-ttu-id="7d2a1-156">Igen</span><span class="sxs-lookup"><span data-stu-id="7d2a1-156">Yes</span></span> |



## <a name="3-create-runbook"></a><span data-ttu-id="7d2a1-157">3. Runbook létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d2a1-157">3. Create runbook</span></span>

<span data-ttu-id="7d2a1-158">Azure Automation szolgáltatásbeli szerkesztővé rendelkezik hello portálon, ahol szerkesztheti, és tesztelje a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-158">Azure Automation has an editor in hello portal where you can edit and test your runbook.</span></span>  <span data-ttu-id="7d2a1-159">A hello beállítás toouse hello parancsfájl szerkesztő toowork van [PowerShell közvetlenül](../automation/automation-edit-textual-runbook.md) vagy [létrehozhat egy grafikus](../automation/automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-159">You have hello option toouse hello script editor toowork with [PowerShell directly](../automation/automation-edit-textual-runbook.md) or [create a graphical runbook](../automation/automation-graphical-authoring-intro.md).</span></span>  <span data-ttu-id="7d2a1-160">Ebben az oktatóanyagban a PowerShell-parancsfájllal fog működni.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-160">For this tutorial, you will work with a PowerShell script.</span></span> 

![Forgatókönyv szerkesztése](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. <span data-ttu-id="7d2a1-162">Keresse meg a tooyour Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-162">Navigate tooyour Automation account.</span></span>  
2. <span data-ttu-id="7d2a1-163">Kattintson a **Runbookok** > **hozzáadása egy runbook** > **hozzon létre egy új runbookot**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-163">Click **Runbooks** > **Add a runbook** > **Create a new runbook**.</span></span>
3. <span data-ttu-id="7d2a1-164">Hello runbook neve, írja be a következőt **gyűjtése-Automation-feladat**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-164">For hello runbook name, type **Collect-Automation-jobs**.</span></span>  <span data-ttu-id="7d2a1-165">Hello runbooktípusba, válassza a **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-165">For hello runbook type, select **PowerShell**.</span></span>
4. <span data-ttu-id="7d2a1-166">Kattintson a **létrehozása** toocreate hello runbook és kezdő hello szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-166">Click **Create** toocreate hello runbook and start hello editor.</span></span>
5. <span data-ttu-id="7d2a1-167">Másolja és illessze be a következő kódot a hello runbook hello.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-167">Copy and paste hello following code into hello runbook.</span></span>  <span data-ttu-id="7d2a1-168">Tekintse meg a toohello megjegyzések hello parancsfájlban hello kód ismertetése.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-168">Refer toohello comments in hello script for explanation of hello code.</span></span>
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a><span data-ttu-id="7d2a1-169">4. Runbook tesztelése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-169">4. Test runbook</span></span>
<span data-ttu-id="7d2a1-170">Azure Automation szolgáltatásbeli tartalmaz egy olyan környezetben túl[tesztelje a forgatókönyvet](../automation/automation-testing-runbook.md) közzététel előtt.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-170">Azure Automation includes an environment too[test your runbook](../automation/automation-testing-runbook.md) before you publish it.</span></span>  <span data-ttu-id="7d2a1-171">Vizsgálja meg a hello runbook által gyűjtött hello adatokat, és ellenőrizze a közzététel előtt elvárt Analytics tooLog tooproduction azt írja.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-171">You can inspect hello data collected by hello runbook and verify that it writes tooLog Analytics as expected before publishing it tooproduction.</span></span> 
 
![Runbook tesztelése](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. <span data-ttu-id="7d2a1-173">Kattintson a **mentése** toosave hello runbook.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-173">Click **Save** toosave hello runbook.</span></span>
1. <span data-ttu-id="7d2a1-174">Kattintson a **teszt ablaktábla** tooopen hello runbook hello tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-174">Click **Test pane** tooopen hello runbook in hello test environment.</span></span>
3. <span data-ttu-id="7d2a1-175">A runbook paraméterekkel rendelkezik, mivel Ön felszólító tooenter értékeinek őket.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-175">Since your runbook has parameters, you're prompted tooenter values for them.</span></span>  <span data-ttu-id="7d2a1-176">Adja meg a hello hello erőforráscsoport nevét, és hello automatizálási fiókot, amellyel a folyamatos toocollect feladat adatait.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-176">Enter hello name of hello resource group and hello automation account that your going toocollect job information from.</span></span>
4. <span data-ttu-id="7d2a1-177">Kattintson a **Start** toohello hello runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-177">Click **Start** toohello start hello runbook.</span></span>
3. <span data-ttu-id="7d2a1-178">hello runbook elindul állapotú **várakozik** előtt túl kerül**futtató**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-178">hello runbook will start with a status of **Queued** before it goes too**Running**.</span></span>  
3. <span data-ttu-id="7d2a1-179">részletes kimenet hello runbook megjelenjen-e a json formátumban gyűjtött hello feladatok.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-179">hello runbook should display verbose output with hello jobs collected in json format.</span></span>  <span data-ttu-id="7d2a1-180">Ha nincsenek feladatok jelenik meg, majd lépett nincsenek feladatok létrehozott hello automation-fiók hello az elmúlt egy óra.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-180">If no jobs are listed, then there may have been no jobs created in hello automation account in hello last hour.</span></span>  <span data-ttu-id="7d2a1-181">Indítsa el minden olyan forgatókönyvben hello automation-fiókban, és utána végezze el újra hello tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-181">Try starting any runbook in hello automation account and then perform hello test again.</span></span>
4. <span data-ttu-id="7d2a1-182">Győződjön meg arról, hello kimeneti hello szereplő hibák utáni elemzés parancs tooLog nem mutatja.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-182">Ensure that hello output doesn't show any errors in hello post command tooLog Analytics.</span></span>  <span data-ttu-id="7d2a1-183">Egy üzenet hasonló toohello következő kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-183">You should have a message similar toohello following.</span></span>

    ![POST kimeneti](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a><span data-ttu-id="7d2a1-185">5. A Naplóelemzési rekordjainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-185">5. Verify records in Log Analytics</span></span>
<span data-ttu-id="7d2a1-186">Miután hello runbook vizsgálat befejeződött, és ellenőrizte, hogy hello kimeneti sikeresen megérkezett, hello rekordok használatával lettek létrehozva ellenőrizheti egy [napló keresés a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-186">Once hello runbook has completed in test, and you verified that hello output was successfully received, you can verify that hello records were created using a [log search in Log Analytics](../log-analytics/log-analytics-log-searches.md).</span></span>

![Kimenet](media/operations-management-suite-runbook-datacollect/log-output.png)

1. <span data-ttu-id="7d2a1-188">Hello Azure-portálon válassza ki a Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-188">In hello Azure portal, select your Log Analytics workspace.</span></span>
2. <span data-ttu-id="7d2a1-189">Kattintson a **keresési jelentkezzen**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-189">Click on **Log Search**.</span></span>
3. <span data-ttu-id="7d2a1-190">A következő parancs típusa hello `Type=AutomationJob_CL` hello Keresés gombra.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-190">Type hello following command `Type=AutomationJob_CL` and click hello search button.</span></span> <span data-ttu-id="7d2a1-191">Vegye figyelembe, hogy hello rekordtípus tartalmaz, amely nincs meghatározva hello parancsfájl _CL.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-191">Note that hello record type includes _CL that isn't specified in hello script.</span></span>  <span data-ttu-id="7d2a1-192">Névutótagot automatikusan hozzáfűzött toohello napló típusa tooindicate, hogy a rendszer egy egyéni napló típusa.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-192">That suffix is automatically appended toohello log type tooindicate that it's a custom log type.</span></span>
4. <span data-ttu-id="7d2a1-193">Meg kell jelennie egy vagy több rekordot adott vissza, jelezve, hogy hello runbook a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-193">You should see one or more records returned indicating that hello runbook is working as expected.</span></span>


## <a name="6-publish-hello-runbook"></a><span data-ttu-id="7d2a1-194">6. Hello runbook közzététele</span><span class="sxs-lookup"><span data-stu-id="7d2a1-194">6. Publish hello runbook</span></span>
<span data-ttu-id="7d2a1-195">Miután ellenőrizte, hogy adott hello runbook megfelelően működik, toopublish kell, így éles környezetben is futtatható.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-195">Once you've verified that hello runbook is working correctly, you need toopublish it so you can run it in production.</span></span>  <span data-ttu-id="7d2a1-196">Tooedit folytatja, és hello runbook tesztelése hello közzétett verzió módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-196">You can continue tooedit and test hello runbook without modifying hello published version.</span></span>  

![Runbook közzététele](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. <span data-ttu-id="7d2a1-198">Térjen vissza a tooyour automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-198">Return tooyour automation account.</span></span>
2. <span data-ttu-id="7d2a1-199">Kattintson a **Runbookok** válassza **gyűjtése-Automation-feladat**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-199">Click on **Runbooks** and select **Collect-Automation-jobs**.</span></span>
3. <span data-ttu-id="7d2a1-200">Kattintson a **szerkesztése** , majd **közzététele**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-200">Click **Edit** and then **Publish**.</span></span>
4. <span data-ttu-id="7d2a1-201">Kattintson a **Igen** verziója, amelyet toooverwrite hello korábban ismételt tooverify közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-201">Click **Yes** when asked tooverify that you want toooverwrite hello previously published version.</span></span>

## <a name="7-set-logging-options"></a><span data-ttu-id="7d2a1-202">7. Naplózási beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="7d2a1-202">7. Set logging options</span></span> 
<span data-ttu-id="7d2a1-203">A teszteket, képes tooview volt [részletes kimenet](../automation/automation-runbook-output-and-messages.md#message-streams) mert hello parancsfájl hello $VerbosePreference változó beállítása.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-203">For test, you were able tooview [verbose output](../automation/automation-runbook-output-and-messages.md#message-streams) because you set hello $VerbosePreference variable in hello script.</span></span>  <span data-ttu-id="7d2a1-204">A végleges kell tooset hello naplózási tulajdonságait: hello runbook Ha azt szeretné, hogy tooview részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-204">For production, you need tooset hello logging properties for hello runbook if you want tooview verbose output.</span></span>  <span data-ttu-id="7d2a1-205">Ebben az oktatóanyagban használt hello runbook hello json küldött adatok mennyisége tooLog Analytics ekkor jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-205">For hello runbook used in this tutorial, this will display hello json data being sent tooLog Analytics.</span></span>

![Naplózás és nyomkövetés](media/operations-management-suite-runbook-datacollect/logging.png)

1. <span data-ttu-id="7d2a1-207">Válassza ki a runbook tulajdonságainak hello **naplózás és nyomkövetés** alatt **Runbookbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-207">In hello properties for your runbook select **Logging and tracing** under **Runbook Settings**.</span></span>
2. <span data-ttu-id="7d2a1-208">Hello beállításának módosítása **részletes rekordok naplózására** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-208">Change hello setting for **Log verbose records** too**On**.</span></span>
3. <span data-ttu-id="7d2a1-209">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-209">Click **Save**.</span></span>

## <a name="8-schedule-runbook"></a><span data-ttu-id="7d2a1-210">8. Runbook ütemezése</span><span class="sxs-lookup"><span data-stu-id="7d2a1-210">8. Schedule runbook</span></span>
<span data-ttu-id="7d2a1-211">hello leggyakoribb módja toostart figyelési adatokat összegyűjtő runbook tooschedule azt toorun automatikusan.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-211">hello most common way toostart a runbook that collects monitoring data is tooschedule it toorun automatically.</span></span>  <span data-ttu-id="7d2a1-212">Hozzon létre ehhez a [Azure Automation ütemezési](../automation/automation-schedules.md) és tooyour runbook csatlakoztatás.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-212">You do this by creating a [schedule in Azure Automation](../automation/automation-schedules.md) and attaching it tooyour runbook.</span></span>

![Runbook ütemezése](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. <span data-ttu-id="7d2a1-214">Válassza ki a runbook tulajdonságainak hello, **ütemezések** alatt **erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-214">In hello properties for your runbook, select **Schedules** under **Resources**.</span></span>
2. <span data-ttu-id="7d2a1-215">Kattintson a **ütemezés hozzáadása** > **ütemezés tooyour runbook hivatkozás** > **hozzon létre egy új ütemezést**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-215">Click **Add a schedule** > **Link a schedule tooyour runbook** > **Create a new schedule**.</span></span>
5. <span data-ttu-id="7d2a1-216">A hello értékek hello ütemezést, majd kattintson a következő típus **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-216">Type in hello following values for hello schedule and click **Create**.</span></span>

| <span data-ttu-id="7d2a1-217">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="7d2a1-217">Property</span></span> | <span data-ttu-id="7d2a1-218">Érték</span><span class="sxs-lookup"><span data-stu-id="7d2a1-218">Value</span></span> |
|:--|:--|
| <span data-ttu-id="7d2a1-219">Név</span><span class="sxs-lookup"><span data-stu-id="7d2a1-219">Name</span></span> | <span data-ttu-id="7d2a1-220">AutomationJobs-óránként</span><span class="sxs-lookup"><span data-stu-id="7d2a1-220">AutomationJobs-Hourly</span></span> |
| <span data-ttu-id="7d2a1-221">Indítása</span><span class="sxs-lookup"><span data-stu-id="7d2a1-221">Starts</span></span> | <span data-ttu-id="7d2a1-222">Válassza ki a bármikor legalább 5 perccel korábbi hello aktuális idő.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-222">Select any time at least 5 minutes past hello current time.</span></span> |
| <span data-ttu-id="7d2a1-223">Ismétlődés</span><span class="sxs-lookup"><span data-stu-id="7d2a1-223">Recurrence</span></span> | <span data-ttu-id="7d2a1-224">Ismétlődő</span><span class="sxs-lookup"><span data-stu-id="7d2a1-224">Recurring</span></span> |
| <span data-ttu-id="7d2a1-225">Ismétlődik minden</span><span class="sxs-lookup"><span data-stu-id="7d2a1-225">Recur every</span></span> | <span data-ttu-id="7d2a1-226">1 óra</span><span class="sxs-lookup"><span data-stu-id="7d2a1-226">1 hour</span></span> |
| <span data-ttu-id="7d2a1-227">Készlet lejárati</span><span class="sxs-lookup"><span data-stu-id="7d2a1-227">Set expiration</span></span> | <span data-ttu-id="7d2a1-228">Nem</span><span class="sxs-lookup"><span data-stu-id="7d2a1-228">No</span></span> |

<span data-ttu-id="7d2a1-229">Hello ütemezés létrehozása után kell tooset hello paraméter értékeket, amelyeket a rendszer minden alkalommal, amikor az ütemezés hello runbook elindul.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-229">Once hello schedule is created, you need tooset hello parameter values that will be used each time this schedule starts hello runbook.</span></span>

6. <span data-ttu-id="7d2a1-230">Kattintson a **konfigurálása paraméterek és futtatási beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-230">Click **Configure parameters and run settings**.</span></span>
7. <span data-ttu-id="7d2a1-231">Adja meg az értékeket a **ResourceGroupName** és **AutomationAccountName**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-231">Fill in values for your **ResourceGroupName** and **AutomationAccountName**.</span></span>
8. <span data-ttu-id="7d2a1-232">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-232">Click **OK**.</span></span> 

## <a name="9-verify-runbook-starts-on-schedule"></a><span data-ttu-id="7d2a1-233">9. Ellenőrizze a runbook elindul az ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="7d2a1-233">9. Verify runbook starts on schedule</span></span>
<span data-ttu-id="7d2a1-234">Egy runbook indítását, Everytime [feladat jön létre](../automation/automation-runbook-execution.md) és a kimenetet a naplóba.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-234">Everytime a runbook is started, [a job is created](../automation/automation-runbook-execution.md) and any output logged.</span></span>  <span data-ttu-id="7d2a1-235">Valójában ezek a hello ugyanazt a feladatot, amely hello runbook gyűjt.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-235">In fact, these are hello same jobs that hello runbook is collecting.</span></span>  <span data-ttu-id="7d2a1-236">Ellenőrizheti, hogy hello runbook hello feladatok hello runbook ellenőrzésével hello ütemezések kezdő időpontja hello letelte után megfelelően elindul.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-236">You can verify that hello runbook starts as expected by checking hello jobs for hello runbook after hello start time for hello schedule has passed.</span></span>

![Feladatok](media/operations-management-suite-runbook-datacollect/jobs.png)

1. <span data-ttu-id="7d2a1-238">Válassza ki a runbook tulajdonságainak hello, **feladatok** alatt **erőforrások**.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-238">In hello properties for your runbook, select **Jobs** under **Resources**.</span></span>
2. <span data-ttu-id="7d2a1-239">Meg kell jelennie, minden alkalommal hello runbook feladatok listáját lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-239">You should see a listing of jobs for each time hello runbook was started.</span></span>
3. <span data-ttu-id="7d2a1-240">Kattintson a hello feladatok tooview egyik hozzá tartozó részletek.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-240">Click on one of hello jobs tooview its details.</span></span>
4. <span data-ttu-id="7d2a1-241">Kattintson a **összes napló** tooview hello naplókat, valamint a kimeneti hello runbookból.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-241">Click on **All logs** tooview hello logs and output from hello runbook.</span></span>
5. <span data-ttu-id="7d2a1-242">Görgessen toohello alsó toofind egy bejegyzés hasonló toohello az alábbi képen.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-242">Scroll toohello bottom toofind an entry similar toohello image below.</span></span><br>![Részletes](media/operations-management-suite-runbook-datacollect/verbose.png)
6. <span data-ttu-id="7d2a1-244">Kattintson arra a bejegyzésre tooview hello részletes tooLog Analytics elküldött json-adatokat.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-244">Click on this entry tooview hello detailed json data  that was sent tooLog Analytics.</span></span>



## <a name="next-steps"></a><span data-ttu-id="7d2a1-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d2a1-245">Next steps</span></span>
- <span data-ttu-id="7d2a1-246">Használjon [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) egy nézet megjelenítése toocreate hello, hogy toohello Naplóelemzési tárház korábban összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-246">Use [View Designer](../log-analytics/log-analytics-view-designer.md) toocreate a view displaying hello data that you've collected toohello Log Analytics repository.</span></span>
- <span data-ttu-id="7d2a1-247">A runbookot a csomag egy [felügyeleti megoldás](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span><span class="sxs-lookup"><span data-stu-id="7d2a1-247">Package your runbook in a [management solution](operations-management-suite-solutions-creating.md) toodistribute toocustomers.</span></span>
- <span data-ttu-id="7d2a1-248">További információ [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-248">Learn more about [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).</span></span>
- <span data-ttu-id="7d2a1-249">További információ [Azure Automation](https://docs.microsoft.com/azure/automation/).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-249">Learn more about [Azure Automation](https://docs.microsoft.com/azure/automation/).</span></span>
- <span data-ttu-id="7d2a1-250">További tudnivalók hello [HTTP adatait gyűjtője API](../log-analytics/log-analytics-data-collector-api.md).</span><span class="sxs-lookup"><span data-stu-id="7d2a1-250">Learn more about hello [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md).</span></span>
