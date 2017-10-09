---
title: "Azure Automation - PowerShell munkafolyamat aaaStarting és leállításával virtuális gépek |} Microsoft Docs"
description: "Grafikus verzióját, beleértve a runbookok toostart, majd szüntesse meg a klasszikus virtuális gépek Azure Automation-forgatókönyv."
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
redirect_document_id: False
ms.openlocfilehash: 273631c7fc5ddb989b3bbdc82b470ac3af6ee482
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a><span data-ttu-id="23216-103">Azure Automation forgatókönyv - elindítása és leállítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="23216-103">Azure Automation scenario - starting and stopping virtual machines</span></span>
<span data-ttu-id="23216-104">Az Azure Automation-forgatókönyv runbookok toostart, majd szüntesse meg a klasszikus virtuális gépeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="23216-104">This Azure Automation scenario includes runbooks toostart and stop classic virtual machines.</span></span>  <span data-ttu-id="23216-105">Ebben a forgatókönyvben hello következő használható:</span><span class="sxs-lookup"><span data-stu-id="23216-105">You can use this scenario for any of hello following:</span></span>  

* <span data-ttu-id="23216-106">A saját környezetben runbookokat hello a módosítás nélkül használható.</span><span class="sxs-lookup"><span data-stu-id="23216-106">Use hello runbooks without modification in your own environment.</span></span>
* <span data-ttu-id="23216-107">Módosítsa a hello runbookok tooperform testreszabott funkcióit.</span><span class="sxs-lookup"><span data-stu-id="23216-107">Modify hello runbooks tooperform customized functionality.</span></span>  
* <span data-ttu-id="23216-108">Runbookokat hello hívható meg egy másik runbookból egy teljes megoldás részeként.</span><span class="sxs-lookup"><span data-stu-id="23216-108">Call hello runbooks from another runbook as part of an overall solution.</span></span>
* <span data-ttu-id="23216-109">Szerzői alapfogalmak oktatóanyagok toolearn runbook runbookokat hello használják.</span><span class="sxs-lookup"><span data-stu-id="23216-109">Use hello runbooks as tutorials toolearn runbook authoring concepts.</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="23216-110">Grafikus</span><span class="sxs-lookup"><span data-stu-id="23216-110">Graphical</span></span>](automation-solution-startstopvm-graphical.md)
> * [<span data-ttu-id="23216-111">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="23216-111">PowerShell Workflow</span></span>](automation-solution-startstopvm-psworkflow.md)
> 
> 

<span data-ttu-id="23216-112">Ez a hello ebben a forgatókönyvben PowerShell-munkafolyamati forgatókönyv verzióját.</span><span class="sxs-lookup"><span data-stu-id="23216-112">This is hello PowerShell Workflow runbook version of this scenario.</span></span> <span data-ttu-id="23216-113">Akkor is elérhető használatával [grafikus forgatókönyvek](automation-solution-startstopvm-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="23216-113">It is also available using [graphical runbooks](automation-solution-startstopvm-graphical.md).</span></span>

## <a name="getting-hello-scenario"></a><span data-ttu-id="23216-114">Első hello forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="23216-114">Getting hello scenario</span></span>
<span data-ttu-id="23216-115">Ebben a forgatókönyvben két PowerShell-munkafolyamati forgatókönyvek tölthet le a következő hivatkozások hello áll.</span><span class="sxs-lookup"><span data-stu-id="23216-115">This scenario consists of two PowerShell Workflow runbooks that you can download from hello following links.</span></span>  <span data-ttu-id="23216-116">Lásd: hello [grafikus verzió](automation-solution-startstopvm-graphical.md) az ebben a forgatókönyvben hivatkozások toohello grafikus forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="23216-116">See hello [graphical version](automation-solution-startstopvm-graphical.md) of this scenario for links toohello graphical runbooks.</span></span>

| <span data-ttu-id="23216-117">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="23216-117">Runbook</span></span> | <span data-ttu-id="23216-118">Hivatkozás</span><span class="sxs-lookup"><span data-stu-id="23216-118">Link</span></span> | <span data-ttu-id="23216-119">Típus</span><span class="sxs-lookup"><span data-stu-id="23216-119">Type</span></span> | <span data-ttu-id="23216-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="23216-120">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="23216-121">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-121">Start-AzureVMs</span></span> |[<span data-ttu-id="23216-122">Az Azure klasszikus virtuális gépek elindítása</span><span class="sxs-lookup"><span data-stu-id="23216-122">Start Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) |<span data-ttu-id="23216-123">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="23216-123">PowerShell Workflow</span></span> |<span data-ttu-id="23216-124">Indítja el az összes klasszikus virtuális gép egy Azure subscriptionor az összes virtuális gép egy adott szolgáltatáshoz nevű.</span><span class="sxs-lookup"><span data-stu-id="23216-124">Starts all classic virtual machines in an Azure subscriptionor all virtual machines with a particular service name.</span></span> |
| <span data-ttu-id="23216-125">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-125">Stop-AzureVMs</span></span> |[<span data-ttu-id="23216-126">Az Azure klasszikus virtuális gépek leállítása</span><span class="sxs-lookup"><span data-stu-id="23216-126">Stop Azure Classic VMs</span></span>](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) |<span data-ttu-id="23216-127">PowerShell-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="23216-127">PowerShell Workflow</span></span> |<span data-ttu-id="23216-128">Automation-fiók összes virtuális gép vagy egy adott szolgáltatáshoz névvel rendelkező összes virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="23216-128">Stops all virtual machines in an automation account or all virtual machines with a particular service name.</span></span> |

## <a name="installing-and-configuring-hello-scenario"></a><span data-ttu-id="23216-129">Telepítése és konfigurálása hello forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="23216-129">Installing and configuring hello scenario</span></span>
### <a name="1-install-hello-runbooks"></a><span data-ttu-id="23216-130">1. Runbookokat hello telepítése</span><span class="sxs-lookup"><span data-stu-id="23216-130">1. Install hello runbooks</span></span>
<span data-ttu-id="23216-131">Runbookokat hello a letöltés után importálhatja azokat a hello eljárással [olyan Runbookot importálna](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span><span class="sxs-lookup"><span data-stu-id="23216-131">After downloading hello runbooks, you can import them using hello procedure in [Importing a Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).</span></span>

### <a name="2-review-hello-description-and-requirements"></a><span data-ttu-id="23216-132">2. Felülvizsgálati hello leírása és követelményei</span><span class="sxs-lookup"><span data-stu-id="23216-132">2. Review hello description and requirements</span></span>
<span data-ttu-id="23216-133">runbookokat hello megjegyzésként súgószöveg, amely tartalmazza a szükséges eszközök és leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="23216-133">hello runbooks include commented help text that includes a description and required assets.</span></span>  <span data-ttu-id="23216-134">Is megkapható hello ugyanazokat az információkat a cikkből.</span><span class="sxs-lookup"><span data-stu-id="23216-134">You can also get hello same information from this article.</span></span>

### <a name="3-configure-assets"></a><span data-ttu-id="23216-135">3. Eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23216-135">3. Configure assets</span></span>
<span data-ttu-id="23216-136">runbookokat hello eszközöket, létre kell hoznia és rendelt megfelelő értékeket a következő hello igényelnek.</span><span class="sxs-lookup"><span data-stu-id="23216-136">hello runbooks require hello following assets that you must create and populate with appropriate values.</span></span>

| <span data-ttu-id="23216-137">Eszköz típusa</span><span class="sxs-lookup"><span data-stu-id="23216-137">Asset Type</span></span> | <span data-ttu-id="23216-138">Eszköz neve</span><span class="sxs-lookup"><span data-stu-id="23216-138">Asset Name</span></span> | <span data-ttu-id="23216-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="23216-139">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="23216-140">Hitelesítő adat</span><span class="sxs-lookup"><span data-stu-id="23216-140">Credential</span></span> |<span data-ttu-id="23216-141">AzureCredential</span><span class="sxs-lookup"><span data-stu-id="23216-141">AzureCredential</span></span> |<span data-ttu-id="23216-142">Egy Azure-előfizetés hello hatóság toostart és állítsa le a virtuális gépek rendelkező fiók hitelesítő adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="23216-142">Contains credentials for an account that has authority toostart and stop virtual machines in hello Azure subscription.</span></span>  <span data-ttu-id="23216-143">Másik lehetőségként megadhat egy másik hitelesítőadat-eszköz hello **Credential** hello paramétere **Add-AzureAccount** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="23216-143">Alternatively, you can specify another credential asset in hello **Credential** parameter of hello **Add-AzureAccount** activity.</span></span> |
| <span data-ttu-id="23216-144">Változó</span><span class="sxs-lookup"><span data-stu-id="23216-144">Variable</span></span> |<span data-ttu-id="23216-145">AzureSubscriptionId</span><span class="sxs-lookup"><span data-stu-id="23216-145">AzureSubscriptionId</span></span> |<span data-ttu-id="23216-146">Hello előfizetés-azonosító az Azure-előfizetés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="23216-146">Contains hello subscription ID of your Azure subscription.</span></span> |

## <a name="using-hello-scenario"></a><span data-ttu-id="23216-147">Hello forgatókönyv használata</span><span class="sxs-lookup"><span data-stu-id="23216-147">Using hello scenario</span></span>
### <a name="parameters"></a><span data-ttu-id="23216-148">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="23216-148">Parameters</span></span>
<span data-ttu-id="23216-149">hello runbookokat hello a következő paraméterekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="23216-149">hello runbooks each have hello following parameters.</span></span>  <span data-ttu-id="23216-150">Adjon meg értéket minden kötelező paraméterhez, és opcionálisan szükséges más paramétereket a követelményeitől függően.</span><span class="sxs-lookup"><span data-stu-id="23216-150">You must provide values for any mandatory parameters and can optionally provide values for other parameters depending on your requirements.</span></span>

| <span data-ttu-id="23216-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="23216-151">Parameter</span></span> | <span data-ttu-id="23216-152">Típus</span><span class="sxs-lookup"><span data-stu-id="23216-152">Type</span></span> | <span data-ttu-id="23216-153">Kötelező</span><span class="sxs-lookup"><span data-stu-id="23216-153">Mandatory</span></span> | <span data-ttu-id="23216-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="23216-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="23216-155">Szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="23216-155">ServiceName</span></span> |<span data-ttu-id="23216-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="23216-156">string</span></span> |<span data-ttu-id="23216-157">Nem</span><span class="sxs-lookup"><span data-stu-id="23216-157">No</span></span> |<span data-ttu-id="23216-158">Ha egy érték áll rendelkezésre, majd az összes virtuális gép szolgáltatás nevű elindításakor vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="23216-158">If a value is provided, then all virtual machines with that service name are started or stopped.</span></span>  <span data-ttu-id="23216-159">Ha nincs érték megadva, majd minden hello Azure-előfizetéssel klasszikus virtuális gépek elindításakor vagy leállt.</span><span class="sxs-lookup"><span data-stu-id="23216-159">If no value is provided, then all classic virtual machines in hello Azure subscription are started or stopped.</span></span> |
| <span data-ttu-id="23216-160">AzureSubscriptionIdAssetName</span><span class="sxs-lookup"><span data-stu-id="23216-160">AzureSubscriptionIdAssetName</span></span> |<span data-ttu-id="23216-161">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="23216-161">string</span></span> |<span data-ttu-id="23216-162">Nem</span><span class="sxs-lookup"><span data-stu-id="23216-162">No</span></span> |<span data-ttu-id="23216-163">Hello hello nevét tartalmazza [változóeszköz](#installing-and-configuring-the-scenario) , amely tartalmazza az Azure-előfizetéshez hello előfizetés-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="23216-163">Contains hello name of hello [variable asset](#installing-and-configuring-the-scenario) that contains hello subscription ID of your Azure subscription.</span></span>  <span data-ttu-id="23216-164">Ha nem adja meg egy értéket *AzureSubscriptionId* szolgál.</span><span class="sxs-lookup"><span data-stu-id="23216-164">If you don't specify a value, *AzureSubscriptionId* is used.</span></span> |
| <span data-ttu-id="23216-165">AzureCredentialAssetName</span><span class="sxs-lookup"><span data-stu-id="23216-165">AzureCredentialAssetName</span></span> |<span data-ttu-id="23216-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="23216-166">string</span></span> |<span data-ttu-id="23216-167">Nem</span><span class="sxs-lookup"><span data-stu-id="23216-167">No</span></span> |<span data-ttu-id="23216-168">Hello hello nevét tartalmazza [hitelesítőadat-eszköz](#installing-and-configuring-the-scenario) tartalmazó hello runbook toouse hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="23216-168">Contains hello name of hello [credential asset](#installing-and-configuring-the-scenario) that contains hello credentials for hello runbook toouse.</span></span>  <span data-ttu-id="23216-169">Ha nem adja meg egy értéket *AzureCredential* szolgál.</span><span class="sxs-lookup"><span data-stu-id="23216-169">If you don't specify a value, *AzureCredential* is used.</span></span> |

### <a name="starting-hello-runbooks"></a><span data-ttu-id="23216-170">Runbookokat hello indítása</span><span class="sxs-lookup"><span data-stu-id="23216-170">Starting hello runbooks</span></span>
<span data-ttu-id="23216-171">Hello módszerek bármelyikét használhatja [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) toostart runbookokat hello a ebben a forgatókönyvben egyik sem.</span><span class="sxs-lookup"><span data-stu-id="23216-171">You can use any of hello methods in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md) toostart either of hello runbooks in this scenario.</span></span>

<span data-ttu-id="23216-172">a következő Példaparancsok hello használja a Windows PowerShell toorun **StartAzureVMs** toostart hello szolgáltatás névvel rendelkező összes virtuális gép *MyVMService*.</span><span class="sxs-lookup"><span data-stu-id="23216-172">hello following sample commands uses Windows PowerShell toorun **StartAzureVMs** toostart all virtual machines with hello service name *MyVMService*.</span></span>

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a><span data-ttu-id="23216-173">Kimenet</span><span class="sxs-lookup"><span data-stu-id="23216-173">Output</span></span>
<span data-ttu-id="23216-174">runbookokat hello fog [kimeneti üzenet](automation-runbook-output-and-messages.md) minden virtuális gép, amely azt jelzi-e hello indítási vagy leállítási utasítás sikeresen el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="23216-174">hello runbooks will [output a message](automation-runbook-output-and-messages.md) for each virtual machine indicating whether or not hello start or stop instruction was successfully submitted.</span></span>  <span data-ttu-id="23216-175">Kereshet az hello kimeneti toodetermine hello eredmények minden runbook egy adott karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="23216-175">You can look for a specific string in hello output toodetermine hello result for each runbook.</span></span>  <span data-ttu-id="23216-176">a következő táblázat hello hello lehetséges kimeneti karakterláncok listáját.</span><span class="sxs-lookup"><span data-stu-id="23216-176">hello possible output strings are listed in hello following table.</span></span>

| <span data-ttu-id="23216-177">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="23216-177">Runbook</span></span> | <span data-ttu-id="23216-178">Az állapot</span><span class="sxs-lookup"><span data-stu-id="23216-178">Condition</span></span> | <span data-ttu-id="23216-179">Üzenet</span><span class="sxs-lookup"><span data-stu-id="23216-179">Message</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="23216-180">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-180">Start-AzureVMs</span></span> |<span data-ttu-id="23216-181">Virtuális gép már fut.</span><span class="sxs-lookup"><span data-stu-id="23216-181">Virtual machine is already running</span></span> |<span data-ttu-id="23216-182">MyVM már fut.</span><span class="sxs-lookup"><span data-stu-id="23216-182">MyVM is already running</span></span> |
| <span data-ttu-id="23216-183">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-183">Start-AzureVMs</span></span> |<span data-ttu-id="23216-184">Kérelem küldése sikeres volt a virtuális gép indítása</span><span class="sxs-lookup"><span data-stu-id="23216-184">Start request for virtual machine successfully submitted</span></span> |<span data-ttu-id="23216-185">MyVM el lett indítva.</span><span class="sxs-lookup"><span data-stu-id="23216-185">MyVM has been started</span></span> |
| <span data-ttu-id="23216-186">Start-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-186">Start-AzureVMs</span></span> |<span data-ttu-id="23216-187">Nem sikerült a virtuális gép indítási kérésre</span><span class="sxs-lookup"><span data-stu-id="23216-187">Start request for virtual machine failed</span></span> |<span data-ttu-id="23216-188">Nem sikerült MyVM toostart</span><span class="sxs-lookup"><span data-stu-id="23216-188">MyVM failed toostart</span></span> |
| <span data-ttu-id="23216-189">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-189">Stop-AzureVMs</span></span> |<span data-ttu-id="23216-190">Virtuális gép már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="23216-190">Virtual machine is already stopped</span></span> |<span data-ttu-id="23216-191">MyVM már le van állítva.</span><span class="sxs-lookup"><span data-stu-id="23216-191">MyVM is already stopped</span></span> |
| <span data-ttu-id="23216-192">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-192">Stop-AzureVMs</span></span> |<span data-ttu-id="23216-193">Kérelem küldése sikeres volt a virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="23216-193">Stop request for virtual machine successfully submitted</span></span> |<span data-ttu-id="23216-194">MyVM le lett állítva.</span><span class="sxs-lookup"><span data-stu-id="23216-194">MyVM has been stopped</span></span> |
| <span data-ttu-id="23216-195">STOP-AzureVMs</span><span class="sxs-lookup"><span data-stu-id="23216-195">Stop-AzureVMs</span></span> |<span data-ttu-id="23216-196">A virtuális gép leállítási kérelem sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="23216-196">Stop request for virtual machine failed</span></span> |<span data-ttu-id="23216-197">Nem sikerült MyVM toostop</span><span class="sxs-lookup"><span data-stu-id="23216-197">MyVM failed toostop</span></span> |

<span data-ttu-id="23216-198">Például következő kódrészlet egy runbookból hello kísérletek toostart hello szolgáltatás névvel rendelkező összes virtuális gép *MyServiceName*.</span><span class="sxs-lookup"><span data-stu-id="23216-198">For example, hello following code snippet from a runbook attempts toostart all virtual machines with hello service name *MyServiceName*.</span></span>  <span data-ttu-id="23216-199">Ha bármelyik hello indítsa el a kérelmek sikertelenek, majd hiba műveleteket lehessen állítani.</span><span class="sxs-lookup"><span data-stu-id="23216-199">If any of hello start requests fail, then error actions can be taken.</span></span>

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action tootake in case of success.
        }
        else {
            # Action tootake in case of error.
        }
    }


## <a name="detailed-breakdown"></a><span data-ttu-id="23216-200">Részletes információkat</span><span class="sxs-lookup"><span data-stu-id="23216-200">Detailed breakdown</span></span>
<span data-ttu-id="23216-201">Az alábbiakban látható az ebben a forgatókönyvben hello runbookok részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="23216-201">Following is a detailed breakdown of hello runbooks in this scenario.</span></span>  <span data-ttu-id="23216-202">Ezen információk tooeither runbookokat hello vagy ezekből a saját automatizálási esetekben tartalomkészítéshez csak toolearn testreszabása.</span><span class="sxs-lookup"><span data-stu-id="23216-202">You can use this information tooeither customize hello runbooks or just toolearn from them for authoring your own automation scenarios.</span></span>

### <a name="parameters"></a><span data-ttu-id="23216-203">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="23216-203">Parameters</span></span>
    param (
        [Parameter(Mandatory=$false)]
        [String]  $AzureCredentialAssetName = 'AzureCredential',

        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)]
        [String] $ServiceName
    )

<span data-ttu-id="23216-204">hello munkafolyamat való első hello értékek beolvasása hello [bemeneti paraméterek](#using-the-scenario).</span><span class="sxs-lookup"><span data-stu-id="23216-204">hello workflow starts by getting hello values for hello [input parameters](#using-the-scenario).</span></span>  <span data-ttu-id="23216-205">Ha hello eszköz neve nincs megadva alapértelmezett neveket használnak.</span><span class="sxs-lookup"><span data-stu-id="23216-205">If hello asset names are not provided then default names are used.</span></span>

### <a name="output"></a><span data-ttu-id="23216-206">Kimenet</span><span class="sxs-lookup"><span data-stu-id="23216-206">Output</span></span>
    # Returns strings with status messages
    [OutputType([String])]

<span data-ttu-id="23216-207">A sor deklarálja, hogy hello kimeneti hello runbook lesz-e egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="23216-207">This line declares that hello output of hello runbook will be a string.</span></span>  <span data-ttu-id="23216-208">Ez nincs szükség, de hello runbook használatakor az ajánlott eljárás egy [gyermekrunbook](automation-child-runbooks.md) , hogy megtudják, hogy a szülő runbook hello kimeneti tooexpect írja be.</span><span class="sxs-lookup"><span data-stu-id="23216-208">This is not required but is a best practice for when hello runbook is used as a [child runbook](automation-child-runbooks.md) so that a parent runbook will know hello output type tooexpect.</span></span>

### <a name="authentication"></a><span data-ttu-id="23216-209">Authentication</span><span class="sxs-lookup"><span data-stu-id="23216-209">Authentication</span></span>
    # Connect tooAzure and select hello subscription toowork against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

<span data-ttu-id="23216-210">hello következő sorok hello beállítása [hitelesítő adatok](automation-credentials.md) és az Azure-előfizetéssel, amely jelzi a hello rest hello runbook.</span><span class="sxs-lookup"><span data-stu-id="23216-210">hello next lines set hello [credentials](automation-credentials.md) and Azure subscription that will be used for hello rest of hello runbook.</span></span>
<span data-ttu-id="23216-211">Először használjuk **Get-AutomationPSCredential** tooget hello eszköz, amely az Azure-előfizetés hello toostart, majd szüntesse meg a virtuális gépek hello hitelesítő adatok elérésével tárolja.</span><span class="sxs-lookup"><span data-stu-id="23216-211">First we use **Get-AutomationPSCredential** tooget hello asset that holds hello credentials with access toostart and stop virtual machines in hello Azure subscription.</span></span> <span data-ttu-id="23216-212">**Adja hozzá AzureAccount** az eszköz tooset hello megadott hitelesítő adatokat használja majd.</span><span class="sxs-lookup"><span data-stu-id="23216-212">**Add-AzureAccount** then uses this asset tooset hello credentials.</span></span>  <span data-ttu-id="23216-213">hello kimeneti tooa típusú változó van hozzárendelve, így azt nem tartalmazza a runbook-kimenet hello.</span><span class="sxs-lookup"><span data-stu-id="23216-213">hello output is assigned tooa dummy variable so that it isn't included in hello runbook output.</span></span>  

<span data-ttu-id="23216-214">hello változóeszköz hello előfizetés azonosítója majd beolvasni a **Get-AutomationVariable** és a beállított hello előfizetés **válasszon-AzureSubscription**.</span><span class="sxs-lookup"><span data-stu-id="23216-214">hello variable asset with hello subscription ID is then retrieved with **Get-AutomationVariable** and hello subscription set with **Select-AzureSubscription**.</span></span>

### <a name="get-vms"></a><span data-ttu-id="23216-215">Virtuális gépek beolvasása</span><span class="sxs-lookup"><span data-stu-id="23216-215">Get VMs</span></span>
    # If there is a specific cloud service, then get all VMs in hello service,
    # otherwise get all VMs in hello subscription.
    if ($ServiceName)
    {
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else
    {
        $VMs = Get-AzureVM
    }

<span data-ttu-id="23216-216">**Get-AzureVM** van használt tooretrieve hello virtuális gépek hello runbook működni fog-e.</span><span class="sxs-lookup"><span data-stu-id="23216-216">**Get-AzureVM** is used tooretrieve hello virtual machines hello runbook will work with.</span></span>  <span data-ttu-id="23216-217">Ha értéket megadva a hello **szolgáltatásnév** bemeneti a rendszer beolvassa a változót, akkor csak hello virtuális gépek, a szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="23216-217">If a value is provided in hello **ServiceName** input variable, then only hello virtual machines with that service name are retrieved.</span></span>  <span data-ttu-id="23216-218">Ha **szolgáltatásnév** üres, akkor a rendszer beolvassa az összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="23216-218">If **ServiceName** is empty, then all virtual machines are retrieved.</span></span>

### <a name="startstop-virtual-machines-and-send-output"></a><span data-ttu-id="23216-219">Virtuális gépek indítása/leállítása és kimenetként</span><span class="sxs-lookup"><span data-stu-id="23216-219">Start/Stop virtual machines and send output</span></span>
    # Start each of hello stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # hello VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # hello VM needs toobe started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # hello VM failed toostart, so send notice
                Write-Output ($VM.InstanceName + " failed toostart")
            }
            else
            {
                # hello VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

<span data-ttu-id="23216-220">hello következő sorok lépésenként minden virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="23216-220">hello next lines step through each virtual machine.</span></span>  <span data-ttu-id="23216-221">Először hello **PowerState** hello, a virtuális gép bejelölt toosee is, ha már fut vagy leállt, attól függően, hogy hello runbook.</span><span class="sxs-lookup"><span data-stu-id="23216-221">First hello **PowerState** of hello virtual machine is checked toosee if it is already running or stopped, depending on hello runbook.</span></span>  <span data-ttu-id="23216-222">Ha már hello cél állapotban, majd egy üzenetet kap toooutput, és hello runbook akkor ér véget.</span><span class="sxs-lookup"><span data-stu-id="23216-222">If it is already in hello target state, then a message is sent toooutput, and hello runbook ends.</span></span>  <span data-ttu-id="23216-223">Ha nem, majd **Start-AzureVM** vagy **Stop-AzureVM** használt tooattempt toostart vagy stop hello virtuális gép hello kérelem tárolt tooa változó hello eredménnyel.</span><span class="sxs-lookup"><span data-stu-id="23216-223">If not, then **Start-AzureVM** or **Stop-AzureVM** is used tooattempt toostart or stop hello virtual machine with hello result of hello request stored tooa variable.</span></span>  <span data-ttu-id="23216-224">Egy üzenetet küldi el a rendszer toooutput megadó e hello kérelem toostart vagy leállítása sikeresen el lett küldve.</span><span class="sxs-lookup"><span data-stu-id="23216-224">A message is then sent toooutput specifying whether hello request toostart or stop was submitted successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23216-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23216-225">Next steps</span></span>
* <span data-ttu-id="23216-226">toolearn több gyermekrunbookot, használatával kapcsolatban lásd: [gyermek az Azure Automation runbookjai](automation-child-runbooks.md)</span><span class="sxs-lookup"><span data-stu-id="23216-226">toolearn more about working with child runbooks, see [Child runbooks in Azure Automation](automation-child-runbooks.md)</span></span>
* <span data-ttu-id="23216-227">További részletek toolearn a runbook végrehajtása és a naplózás toohelp állapotüzenetei hibaelhárítás című témakörben [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="23216-227">toolearn more about output messages during runbook execution and logging toohelp troubleshoot, see [Runbook output and messages in Azure Automation](automation-runbook-output-and-messages.md)</span></span>

