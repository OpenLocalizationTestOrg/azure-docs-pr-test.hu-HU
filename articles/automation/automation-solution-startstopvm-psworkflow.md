---
title: "Indítása és leállítása a virtuális gépek az Azure Automation - PowerShell munkafolyamat |} Microsoft Docs"
description: "Többek között a runbookok elindítására és leállítására klasszikus virtuális gépek Azure Automation-forgatókönyv grafikus verzióját."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: d380bd43-d45d-45af-a5b2-78e7f66263c3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2016
ms.author: magoedte;bwren
redirect_url: https://docs.microsoft.com/azure/automation/automation-solution-vm-management
redirect_document_id: FALSE
ms.openlocfilehash: 95a7b02b0d11bf18c398daea48d16e0ead30b543
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="ba724-103">Azure Automation forgatókönyv - elindítása és leállítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ba724-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="ba724-104">Az Azure Automation-forgatókönyv runbookok elindítása és leállítása a klasszikus virtuális gépeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ba724-104">This Azure Automation scenario includes runbooks to start and stop classic virtual machines.</span></span>  <span data-ttu-id="ba724-105">Ebben a forgatókönyvben a következő használható:</span><span class="sxs-lookup"><span data-stu-id="ba724-105">You can use this scenario for any of the following:</span></span>  

* <span data-ttu-id="ba724-106">A saját környezetben használható a runbookok módosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="ba724-106">Use the runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="ba724-107">Módosítsa a runbookok testreszabott funkcióinak végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="ba724-107">Modify the runbooks to perform customized functionality.</span></span>  
* <span data-ttu-id="ba724-108">Hívja a runbookok másik runbookból egy teljes megoldás részeként.</span><span class="sxs-lookup"><span data-stu-id="ba724-108">Call the runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="ba724-109">A runbookok használják oktatóanyagok további runbook fogalmak szerzői.</span><span class="sxs-lookup"><span data-stu-id="ba724-109">Use the runbooks as tutorials to learn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ba724-110">Grafikus</span><span class="sxs-lookup"><span data-stu-id="ba724-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="ba724-111">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="ba724-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="ba724-112">Ez az ebben a forgatókönyvben PowerShell-munkafolyamati forgatókönyv verziója.</span><span class="sxs-lookup"><span data-stu-id="ba724-112">This is the PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="ba724-113">Akkor is elérhető használatával [grafikus forgatókönyvek](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="ba724-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="ba724-114">A forgatókönyv beszerzése</span><span class="sxs-lookup"><span data-stu-id="ba724-114">Getting the scenario</span></span>
<span data-ttu-id="ba724-115">Ebben a forgatókönyvben két PowerShell-munkafolyamati forgatókönyvek tölthet le a következő hivatkozások áll.</span><span class="sxs-lookup"><span data-stu-id="ba724-115">This scenario consists of two PowerShell Workflow runbooks that you can download from the following links.</span></span>  <span data-ttu-id="ba724-116">Tekintse meg a [grafikus verzió](automation-solution-startstopvm-graphical.md) az ebben a forgatókönyvben a grafikus forgatókönyvek mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ba724-116">See the [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links to the graphical runbooks.</span></span>

| <span data-ttu-id="ba724-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ba724-117">Runbook</span></span> | <span data-ttu-id="ba724-118">Hivatkozás</span><span class="sxs-lookup"><span data-stu-id="ba724-118">Link</span></span> | <span data-ttu-id="ba724-119">Típus</span><span class="sxs-lookup"><span data-stu-id="ba724-119">Type</span></span> | <span data-ttu-id="ba724-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="ba724-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ba724-121">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-121">Start-AzureVMs</span></span> |[<span data-ttu-id="ba724-122">Az Azure klasszikus virtuális gépek elindítása</span><span class="sxs-lookup"><span data-stu-id="ba724-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="ba724-123">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="ba724-123">PowerShell Workflow</span></span> |<span data-ttu-id="ba724-124">Indítja el az összes klasszikus virtuális gép egy Azure subscriptionor az összes virtuális gép egy adott szolgáltatáshoz nevű.</span><span class="sxs-lookup"><span data-stu-id="ba724-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="ba724-125">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="ba724-126">Az Azure klasszikus virtuális gépek leállítása</span><span class="sxs-lookup"><span data-stu-id="ba724-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="ba724-127">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="ba724-127">PowerShell Workflow</span></span> |<span data-ttu-id="ba724-128">Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="ba724-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-the-scenario"></a><span data-ttu-id="ba724-129">Telepítése és konfigurálása a forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ba724-129">Installing and configuring the scenario</span></span>
### <a name="1-install-the-runbooks"></a><span data-ttu-id="ba724-130">1. A runbookok telepítése</span><span class="sxs-lookup"><span data-stu-id="ba724-130">1. Install the runbooks</span></span>
<span data-ttu-id="ba724-131">A runbookok a letöltés után importálhatja azokat a [olyan Runbookot importálna](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="ba724-131">After downloading the runbooks, you can import them using the procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-the-description-and-requirements"></a><span data-ttu-id="ba724-132">2. Tekintse át a leírása és követelményei</span><span class="sxs-lookup"><span data-stu-id="ba724-132">2. Review the description and requirements</span></span>
<span data-ttu-id="ba724-133">A runbookok megjegyzésként súgószöveg, amely tartalmazza a szükséges eszközök és leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ba724-133">The runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="ba724-134">A cikkből is megkapható ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="ba724-134">You can also get the same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="ba724-135">3. Eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ba724-135">3. Configure assets</span></span>
<span data-ttu-id="ba724-136">A runbookok a következő eszközök, létre kell hoznia és megfelelő értékekkel feltöltéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="ba724-136">The runbooks require the following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="ba724-137">Eszköz típusa</span><span class="sxs-lookup"><span data-stu-id="ba724-137">Asset Type</span></span> | <span data-ttu-id="ba724-138">Eszköz neve</span><span class="sxs-lookup"><span data-stu-id="ba724-138">Asset Name</span></span> | <span data-ttu-id="ba724-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="ba724-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ba724-140">Hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="ba724-140">Credential</span></span> |<span data-ttu-id="ba724-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="ba724-141">AzureCredential</span></span> |<span data-ttu-id="ba724-142">Egy olyan fiók, amely rendelkezik a szolgáltató indítása és leállítása a virtuális gépek az Azure-előfizetés hitelesítő adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ba724-142">Contains credentials for an account that has authority to start and stop virtual machines in the Azure subscription.</span></span>  <span data-ttu-id="ba724-143">Azt is megteheti, megadhat egy másik, a hitelesítőadat-eszköz a **hitelesítő adatok** paramétere a **Add-AzureAccount** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ba724-143">Alternatively, you can specify another credential asset in the **Credential** parameter of the **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="ba724-144">Változó</span><span class="sxs-lookup"><span data-stu-id="ba724-144">Variable</span></span> |<span data-ttu-id="ba724-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="ba724-145">AzureSubscriptionId</span></span> |<span data-ttu-id="ba724-146">Az előfizetés-azonosító az Azure-előfizetés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ba724-146">Contains the subscription ID of your Azure subscription.</span></span> |

## <a name="using-the-scenario"></a><span data-ttu-id="ba724-147">A forgatókönyv használata</span><span class="sxs-lookup"><span data-stu-id="ba724-147">Using the scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="ba724-148">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ba724-148">Parameters</span></span>
<span data-ttu-id="ba724-149">A runbookok egyes az alábbi paraméterekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ba724-149">The runbooks each have the following parameters.</span></span>  <span data-ttu-id="ba724-150">Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.</span><span class="sxs-lookup"><span data-stu-id="ba724-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="ba724-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ba724-151">Parameter</span></span> | <span data-ttu-id="ba724-152">Típus</span><span class="sxs-lookup"><span data-stu-id="ba724-152">Type</span></span> | <span data-ttu-id="ba724-153">Kötelező</span><span class="sxs-lookup"><span data-stu-id="ba724-153">Mandatory</span></span> | <span data-ttu-id="ba724-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="ba724-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ba724-155">Szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="ba724-155">ServiceName</span></span> |<span data-ttu-id="ba724-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba724-156">string</span></span> |<span data-ttu-id="ba724-157">Nem</span><span class="sxs-lookup"><span data-stu-id="ba724-157">No</span></span> |<span data-ttu-id="ba724-158">Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="ba724-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="ba724-159">Ha nincs érték megadva, majd az Azure-előfizetés összes klasszikus virtuális gépek elindításakor vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="ba724-159">If no value is provided, then all classic virtual machines in the Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="ba724-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="ba724-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="ba724-161">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba724-161">string</span></span> |<span data-ttu-id="ba724-162">Nem</span><span class="sxs-lookup"><span data-stu-id="ba724-162">No</span></span> |<span data-ttu-id="ba724-163">A neve tartalmazza a [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az előfizetés-azonosítója az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ba724-163">Contains the name of the [variable asset](#installing-and-configuring-the-scenario) that contains the subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="ba724-164">Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál.</span><span class="sxs-lookup"><span data-stu-id="ba724-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="ba724-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="ba724-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="ba724-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba724-166">string</span></span> |<span data-ttu-id="ba724-167">Nem</span><span class="sxs-lookup"><span data-stu-id="ba724-167">No</span></span> |<span data-ttu-id="ba724-168">A neve tartalmazza a [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) , amely tartalmazza a runbook használandó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="ba724-168">Contains the name of the [credential asset](#installing-and-configuring-the-scenario) that contains the credentials for the runbook to use.</span></span>  <span data-ttu-id="ba724-169">Ha nem adja meg egy értéket *AzureCredential* szolgál.</span><span class="sxs-lookup"><span data-stu-id="ba724-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-the-runbooks"></a><span data-ttu-id="ba724-170">A runbookok elindítása</span><span class="sxs-lookup"><span data-stu-id="ba724-170">Starting the runbooks</span></span>
<span data-ttu-id="ba724-171">A módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) ebben a forgatókönyvben a runbookok indítása.</span><span class="sxs-lookup"><span data-stu-id="ba724-171">You can use any of the methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) to start either of the runbooks in this scenario.</span></span>

<span data-ttu-id="ba724-172">Az alábbi Példaparancsok Windows PowerShell használatával futtassa **StartAzureVMs** az összes virtuális gép elindítása a szolgáltatás neve *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="ba724-172">The following sample commands uses Windows PowerShell to run **StartAzureVMs** to start all virtual machines with the service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="ba724-173">Kimenet</span><span class="sxs-lookup"><span data-stu-id="ba724-173">Output</span></span>
<span data-ttu-id="ba724-174">A runbookok fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden egyes virtuális géphez, vagy nem sikerült elküldeni a indítási vagy leállítási utasítás jelző.</span><span class="sxs-lookup"><span data-stu-id="ba724-174">The runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not the start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="ba724-175">Kereshet egy adott karakterláncot a kimenetben minden runbook eredmény meghatározására.</span><span class="sxs-lookup"><span data-stu-id="ba724-175">You can look for a specific string in the output to determine the result for each runbook.</span></span>  <span data-ttu-id="ba724-176">A lehetséges kimeneti karakterláncok a következő táblázatban láthatók.</span><span class="sxs-lookup"><span data-stu-id="ba724-176">The possible output strings are listed in the following table.</span></span>

| <span data-ttu-id="ba724-177">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="ba724-177">Runbook</span></span> | <span data-ttu-id="ba724-178">Az állapot</span><span class="sxs-lookup"><span data-stu-id="ba724-178">Condition</span></span> | <span data-ttu-id="ba724-179">Üzenet</span><span class="sxs-lookup"><span data-stu-id="ba724-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="ba724-180">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-180">Start-AzureVMs</span></span> |<span data-ttu-id="ba724-181">Virtuális gép már fut.</span><span class="sxs-lookup"><span data-stu-id="ba724-181">Virtual machine is already running</span></span> |<span data-ttu-id="ba724-182">MyVM már fut.</span><span class="sxs-lookup"><span data-stu-id="ba724-182">MyVM is already running</span></span> |
| <span data-ttu-id="ba724-183">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-183">Start-AzureVMs</span></span> |<span data-ttu-id="ba724-184">Kérelem küldése sikeres volt a virtuális gép indítása</span><span class="sxs-lookup"><span data-stu-id="ba724-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="ba724-185">MyVM el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="ba724-185">MyVM has been started</span></span> |
| <span data-ttu-id="ba724-186">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-186">Start-AzureVMs</span></span> |<span data-ttu-id="ba724-187">Nem sikerült a virtuális gép indítási kérésre</span><span class="sxs-lookup"><span data-stu-id="ba724-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="ba724-188">Nem sikerült elindítani a MyVM</span><span class="sxs-lookup"><span data-stu-id="ba724-188">MyVM failed to start</span></span> |
| <span data-ttu-id="ba724-189">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-189">Stop-AzureVMs</span></span> |<span data-ttu-id="ba724-190">Virtuális gép már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="ba724-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="ba724-191">MyVM már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="ba724-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="ba724-192">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-192">Stop-AzureVMs</span></span> |<span data-ttu-id="ba724-193">Kérelem küldése sikeres volt a virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="ba724-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="ba724-194">MyVM le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="ba724-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="ba724-195">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="ba724-195">Stop-AzureVMs</span></span> |<span data-ttu-id="ba724-196">A virtuális gép leállítási kérelem sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="ba724-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="ba724-197">Nem sikerült leállítani a MyVM</span><span class="sxs-lookup"><span data-stu-id="ba724-197">MyVM failed to stop</span></span> |

<span data-ttu-id="ba724-198">Például a következő kódrészletet egy runbookból megkísérli elindítani az összes virtuális gép szolgáltatásnévvel *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="ba724-198">For example, the following code snippet from a runbook attempts to start all virtual machines with the service name *MyServiceName*.</span></span>  <span data-ttu-id="ba724-199">Ha vannak ilyenek a start kérelmek sikertelenek, majd hiba műveleteket lehessen állítani.</span><span class="sxs-lookup"><span data-stu-id="ba724-199">If any of the start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="ba724-200">Részletes információkat</span><span class="sxs-lookup"><span data-stu-id="ba724-200">Detailed breakdown</span></span>
<span data-ttu-id="ba724-201">Az alábbiakban látható az ebben a forgatókönyvben a runbookok részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="ba724-201">Following is a detailed breakdown of the runbooks in this scenario.</span></span>  <span data-ttu-id="ba724-202">Ez az információ segítségével testre szabhatja a runbookokat, vagy csak a saját automatizálási esetekben tartalomkészítéshez azokat a további.</span><span class="sxs-lookup"><span data-stu-id="ba724-202">You can use this information to either customize the runbooks or just to learn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="ba724-203">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ba724-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="ba724-204">A munkafolyamat által az értékek első elindul a [bemeneti paraméterek](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="ba724-204">The workflow starts by getting the values for the [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="ba724-205">Ha az eszköz neve nincs megadva alapértelmezett neveket használnak.</span><span class="sxs-lookup"><span data-stu-id="ba724-205">If the asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="ba724-206">Kimenet</span><span class="sxs-lookup"><span data-stu-id="ba724-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="ba724-207">A sor deklarálja, hogy a runbook kimenete lesz-e egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="ba724-207">This line declares that the output of the runbook will be a string.</span></span>  <span data-ttu-id="ba724-208">Ez nincs szükség, de a runbook használatakor az ajánlott eljárás egy [gyermekrunbook](automation-child-runbooks.md) , hogy a szülő runbook fogja tudni a várt kimeneti típus.</span><span class="sxs-lookup"><span data-stu-id="ba724-208">This is not required but is a best practice for when the runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know the output type to expect.</span></span>

### <a name="authentication"></a><span data-ttu-id="ba724-209">Authentication</span><span class="sxs-lookup"><span data-stu-id="ba724-209">Authentication</span></span>
    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="ba724-210">A következő sor beállítása a [hitelesítő adatok](automation-credentials.md) és jelzi a runbook a többi Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ba724-210">The next lines set the [credentials](automation-credentials.md) and Azure subscription that will be used for the rest of the runbook.</span></span>
<span data-ttu-id="ba724-211">Először használjuk **Get-AutomationPSCredential** , amely tárolja a hitelesítő adatok indítása és leállítása a virtuális gépek Azure-előfizetést a hozzáférést az eszköz eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ba724-211">First we use **Get-AutomationPSCredential** to get the asset that holds the credentials with access to start and stop virtual machines in the Azure subscription.</span></span> <span data-ttu-id="ba724-212">**Adja hozzá AzureAccount** Ez az eszköz használatával állítsa be a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ba724-212">**Add-AzureAccount** then uses this asset to set the credentials.</span></span>  <span data-ttu-id="ba724-213">A kimeneti van rendelve egy üres változó, így azt nem található meg a runbook-kimenet.</span><span class="sxs-lookup"><span data-stu-id="ba724-213">The output is assigned to a dummy variable so that it isn't included in the runbook output.</span></span>  

<span data-ttu-id="ba724-214">Az azonosító visszakeresése az előfizetéshez a változóeszköz **Get-AutomationVariable** és állítható be az előfizetés **válasszon-AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="ba724-214">The variable asset with the subscription ID is then retrieved with **Get-AutomationVariable** and the subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="ba724-215">Virtuális gépek beolvasása</span><span class="sxs-lookup"><span data-stu-id="ba724-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="ba724-216">**Get-AzureVM** segítségével kérhető le a virtuális gépeket, a runbook működni fog-e.</span><span class="sxs-lookup"><span data-stu-id="ba724-216">**Get-AzureVM** is used to retrieve the virtual machines the runbook will work with.</span></span>  <span data-ttu-id="ba724-217">Ha az érték megtalálható a **szolgáltatásnév** bemeneti változót, akkor a rendszer beolvassa a szolgáltatás neve csak a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ba724-217">If a value is provided in the **ServiceName** input variable, then only the virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="ba724-218">Ha **szolgáltatásnév** üres, akkor a rendszer beolvassa az összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ba724-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="ba724-219">Virtuális gépek indítása/leállítása és kimenetként</span><span class="sxs-lookup"><span data-stu-id="ba724-219">Start/Stop virtual machines and send output</span></span>
    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="ba724-220">A következő sorokban minden egyes virtuális gép lépéseit.</span><span class="sxs-lookup"><span data-stu-id="ba724-220">The next lines step through each virtual machine.</span></span>  <span data-ttu-id="ba724-221">Első a **PowerState** a virtuális gép be van jelölve meg, ha már fut vagy leállt, attól függően, hogy a runbook.</span><span class="sxs-lookup"><span data-stu-id="ba724-221">First the **PowerState** of the virtual machine is checked to see if it is already running or stopped, depending on the runbook.</span></span>  <span data-ttu-id="ba724-222">Ha már van a cél állapotban, majd egy üzenettel történő megjelenítéshez és a runbook véget ér.</span><span class="sxs-lookup"><span data-stu-id="ba724-222">If it is already in the target state, then a message is sent to output, and the runbook ends.</span></span>  <span data-ttu-id="ba724-223">Ha nem, majd **Start-AzureVM** vagy **Stop-AzureVM** próbálja újból elindítani vagy leállítani a virtuális gépet, a kérelem egy változóhoz tárolt eredményével szolgál.</span><span class="sxs-lookup"><span data-stu-id="ba724-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used to attempt to start or stop the virtual machine with the result of the request stored to a variable.</span></span>  <span data-ttu-id="ba724-224">Egy üzenet majd kimeneti megadása, hogy indítása vagy leállítása a kérelem sikeresen elküldve.</span><span class="sxs-lookup"><span data-stu-id="ba724-224">A message is then sent to output specifying whether the request to start or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba724-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba724-225">Next steps</span></span>
* <span data-ttu-id="ba724-226">Gyermek runbookok használatával kapcsolatos további információkért lásd: [gyermek az Azure Automation runbookjai](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="ba724-226">To learn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="ba724-227">További információt a kimeneti üzenetek során a runbook végrehajtása és a hibaelhárítás elősegítése érdekében naplózása, lásd: [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="ba724-227">To learn more about output messages during runbook execution and logging to help troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

