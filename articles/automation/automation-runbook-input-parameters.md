---
title: "Runbook-bemeneti paraméterekhez |} Microsoft Docs"
description: "Forgatókönyv bemeneti paraméterei a rugalmasabb, a runbookok a azáltal, hogy mechanizmusok adatok átadására a runbook, amikor elindul. Ez a cikk ismerteti a különböző alkalmazási helyzetek, amely bemeneti paraméterek runbookok használják."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 4d3dff2c-1f55-498d-9a0e-eee497e5bedb
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sngun
ms.openlocfilehash: 1ebf32338f5242e72eb2e2daa2da50d231f4b683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="f1d09-104">Runbook bemeneti paraméterei</span><span class="sxs-lookup"><span data-stu-id="f1d09-104">Runbook input parameters</span></span>
<span data-ttu-id="f1d09-105">Forgatókönyv bemeneti paraméterei a rugalmasabb, a runbookok a azáltal, hogy mechanizmusok adatok átadására rá, amikor elindul.</span><span class="sxs-lookup"><span data-stu-id="f1d09-105">Runbook input parameters increase the flexibility of runbooks by allowing you to pass data to it when it is started.</span></span> <span data-ttu-id="f1d09-106">A paraméterek lehetővé teszik, hogy a runbook műveleteket, meghatározott forgatókönyvek és környezetekhez felvenne.</span><span class="sxs-lookup"><span data-stu-id="f1d09-106">The parameters allow the runbook actions to be targeted for specific scenarios and environments.</span></span> <span data-ttu-id="f1d09-107">Ez a cikk azt haladhat végig különböző alkalmazási helyzetek runbookok helyének bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="f1d09-108">A bemeneti paraméterek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d09-108">Configure input parameters</span></span>
<span data-ttu-id="f1d09-109">A bemeneti paramétereket a PowerShell, a PowerShell-munkafolyamatok és a grafikus forgatókönyvek konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="f1d09-110">Egy runbook is rendelkezik különböző adattípusokkal több paramétert, vagy nincsenek paraméterei egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="f1d09-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="f1d09-111">Kötelező vagy választható lehet bemeneti paramétereket, és hozzá lehet rendelni egy választható paramétereik alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="f1d09-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="f1d09-112">A rendelkezésre álló módszerek egyikével indításakor, értékek rendelhet egy runbookhoz tartozó bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-112">You can assign values to the input parameters for a runbook when you start it through one of the available methods.</span></span> <span data-ttu-id="f1d09-113">Ezek a metódusok lehetnek egy runbook indítása a portál vagy webszolgáltatás futhat.</span><span class="sxs-lookup"><span data-stu-id="f1d09-113">These methods include starting  a runbook from the portal  or a web service.</span></span> <span data-ttu-id="f1d09-114">Akkor is elindíthatja, mint amit a gyermekrunbook beágyazottan is nevezett egy másik runbook.</span><span class="sxs-lookup"><span data-stu-id="f1d09-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="f1d09-115">Konfigurálhatja a bemeneti paramétereket a PowerShell és a PowerShell-munkafolyamati forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="f1d09-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="f1d09-116">PowerShell és [PowerShell munkafolyamat-forgatókönyvekről](automation-first-runbook-textual.md) az Azure Automationben támogatja a következő attribútumok keresztül meghatározott bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through the following attributes.</span></span>  

| <span data-ttu-id="f1d09-117">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="f1d09-117">**Property**</span></span> | <span data-ttu-id="f1d09-118">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="f1d09-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="f1d09-119">Típus</span><span class="sxs-lookup"><span data-stu-id="f1d09-119">Type</span></span> |<span data-ttu-id="f1d09-120">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1d09-120">Required.</span></span> <span data-ttu-id="f1d09-121">A paraméter értéke a várt adattípus.</span><span class="sxs-lookup"><span data-stu-id="f1d09-121">The data type expected for the parameter value.</span></span> <span data-ttu-id="f1d09-122">A .NET-típus érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f1d09-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="f1d09-123">Név</span><span class="sxs-lookup"><span data-stu-id="f1d09-123">Name</span></span> |<span data-ttu-id="f1d09-124">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1d09-124">Required.</span></span> <span data-ttu-id="f1d09-125">A paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="f1d09-125">The name of the parameter.</span></span> <span data-ttu-id="f1d09-126">Ez a runbookon belül egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f1d09-126">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="f1d09-127">Betűvel kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="f1d09-127">It must start with a letter.</span></span> |
| <span data-ttu-id="f1d09-128">Kötelező</span><span class="sxs-lookup"><span data-stu-id="f1d09-128">Mandatory</span></span> |<span data-ttu-id="f1d09-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-129">Optional.</span></span> <span data-ttu-id="f1d09-130">Meghatározza, hogy értéket kell megadni a paraméterben.</span><span class="sxs-lookup"><span data-stu-id="f1d09-130">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="f1d09-131">Ha ez **$true**, akkor meg kell adni egy értéket, a runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="f1d09-131">If you set this to **$true**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="f1d09-132">Ha ez **$false**, majd egy értéket nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="f1d09-132">If you set this to **$false**, then a value is optional.</span></span> |
| <span data-ttu-id="f1d09-133">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1d09-133">Default value</span></span> |<span data-ttu-id="f1d09-134">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-134">Optional.</span></span>  <span data-ttu-id="f1d09-135">Adja meg egy értéket, amely a paraméter alkalmazva lesznek, ha az érték nem kerül át, a runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="f1d09-135">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="f1d09-136">Alapértelmezett érték bármelyik paraméternél állítható be, és automatikusan teszik a paraméter nem kötelező, függetlenül a kötelező beállítás.</span><span class="sxs-lookup"><span data-stu-id="f1d09-136">A default value can be set for any parameter and will automatically make the parameter optional regardless of the Mandatory setting.</span></span> |

<span data-ttu-id="f1d09-137">Windows PowerShell támogatja a bemeneti paraméterek további attribútumok azokat az itt felsorolt, érvényesítése, például aliasokat, és a paraméter állandóként állítja be.</span><span class="sxs-lookup"><span data-stu-id="f1d09-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="f1d09-138">Azure Automation jelenleg csak a fent felsorolt bemeneti paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="f1d09-138">However, Azure Automation currently supports only the input parameters listed above.</span></span>

<span data-ttu-id="f1d09-139">A paraméterek definícióját a PowerShell-munkafolyamati forgatókönyvek formátuma a következő általános, ahol több paraméter vesszővel kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="f1d09-139">A parameter definition in PowerShell Workflow runbooks has the following general form, where multiple parameters are separated by commas.</span></span>

   ```
     Param
     (
         [Parameter (Mandatory= $true/$false)]
         [Type] Name1 = <Default value>,

         [Parameter (Mandatory= $true/$false)]
         [Type] Name2 = <Default value>
     )
   ```

> [!NOTE]
> <span data-ttu-id="f1d09-140">Ha amely akkor paraméterek, ha nem adja meg a **kötelező** attribútum, akkor alapértelmezés szerint a paraméter nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="f1d09-140">When you're defining parameters, if you don’t specify the **Mandatory** attribute, then by default, the parameter is considered optional.</span></span> <span data-ttu-id="f1d09-141">Is, ha beállít egy paraméter alapértelmezett értékét a PowerShell munkafolyamat-forgatókönyvekről, azt tekintendők PowerShell egy nem kötelező paraméter, függetlenül attól, hogy a **kötelező** attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="f1d09-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of the **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="f1d09-142">Tegyük fel pedig konfiguráljuk egy PowerShell-munkafolyamati forgatókönyv, amely virtuális gépeket, vagy egy virtuális vagy az erőforráscsoporton belül minden virtuális gép adatait a bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-142">As an example, let’s configure the input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="f1d09-143">Ez a forgatókönyv két paraméterrel rendelkezik, az alábbi képernyőfelvételen látható módon: a nevét, a virtuális gép és az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-143">This runbook has two parameters as shown in the following screenshot: the name of virtual machine and the name of the resource group.</span></span>

![Automatizálás PowerShell-munkafolyamat](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="f1d09-145">Ez a paraméter-definícióban a paraméterek **$VMName** és **$resourceGroupName** egyszerű paraméterek karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="f1d09-145">In this parameter definition, the parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="f1d09-146">Azonban PowerShell és a PowerShell-munkafolyamati forgatókönyvek támogatja az egyszerű típusok és a komplex típusok, például a **objektum** vagy **PSCredential** -bemeneti paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="f1d09-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="f1d09-147">Ha a runbookban egy objektum típusú bemeneti paraméter, majd használja a PowerShell kivonattáblaként (név, érték) párral értékét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs to pass in a value.</span></span> <span data-ttu-id="f1d09-148">Például ha egy runbook a következő paramétert:</span><span class="sxs-lookup"><span data-stu-id="f1d09-148">For example, if you have the following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="f1d09-149">A következő érték akkor a paraméter is továbbítja:</span><span class="sxs-lookup"><span data-stu-id="f1d09-149">Then you can pass the following value to the parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="f1d09-150">Grafikus forgatókönyvek bemeneti paramétereinek a konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1d09-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="f1d09-151">A [konfigurálása egy grafikus forgatókönyvnek](automation-first-runbook-graphical.md) bemeneti paraméterek, hozzon létre egy grafikus forgatókönyv, amely virtuális gépek adatait vagy egy virtuális vagy virtuális gépeinek erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="f1d09-151">To [configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="f1d09-152">Egy runbook konfigurálása áll két fő tevékenység az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="f1d09-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="f1d09-153">[**Azure-beli futtató fiókot a Runbookok hitelesítést** ](automation-sec-configure-azure-runas-account.md) Azure szolgáltatással való hitelesítésre.</span><span class="sxs-lookup"><span data-stu-id="f1d09-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) to authenticate with Azure.</span></span>

<span data-ttu-id="f1d09-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) a virtuális gépek tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f1d09-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) to get the properties of a virtual machines.</span></span>

<span data-ttu-id="f1d09-155">Használhatja a [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) tevékenység kimeneti virtuális gépek nevét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-155">You can use the [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity to output the names of virtual machines.</span></span> <span data-ttu-id="f1d09-156">A tevékenység **Get-AzureRmVm** két paramétert fogad a **virtuálisgép-nevet** és a **erőforráscsoport-név**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-156">The activity **Get-AzureRmVm** accepts two parameters, the **virtual machine name** and the **resource group name**.</span></span> <span data-ttu-id="f1d09-157">Mivel ezek a paraméterek a runbook minden egyes indításakor igényelhet eltérő értékek tartoznak, a runbookban is hozzáadhat bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-157">Since these parameters could require different values each time you start the runbook, you can add input parameters to your runbook.</span></span> <span data-ttu-id="f1d09-158">A bemeneti paraméterek hozzáadása lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="f1d09-158">Here are the steps to add input parameters:</span></span>

1. <span data-ttu-id="f1d09-159">Válassza ki a grafikus forgatókönyvnek a **Runbookok** panel megnyitásához, és kattintson [ **szerkesztése** ](automation-graphical-authoring-intro.md) azt.</span><span class="sxs-lookup"><span data-stu-id="f1d09-159">Select the graphical runbook from the **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="f1d09-160">A forgatókönyv-szerkesztőben kattintson **bemeneti és kimeneti** megnyitásához a **bemeneti és kimeneti** panelen.</span><span class="sxs-lookup"><span data-stu-id="f1d09-160">From the runbook editor, click **Input and output** to open the **Input and output** blade.</span></span>
   
    ![Grafikus Automation-forgatókönyv](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="f1d09-162">A **bemeneti és kimeneti** panel a runbook meghatározott bemeneti paraméterek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f1d09-162">The **Input and output** blade displays a list of input parameters that are defined for the runbook.</span></span> <span data-ttu-id="f1d09-163">A panel új bemeneti paraméter hozzáadása vagy szerkesztése meglévő bemeneti paraméter konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f1d09-163">On this blade, you can either add a new input parameter or edit the configuration of an existing input parameter.</span></span> <span data-ttu-id="f1d09-164">A runbook egy új paraméter hozzáadásához kattintson **bemenet hozzáadása** megnyitásához a **Runbook bemeneti paraméter** panelen.</span><span class="sxs-lookup"><span data-stu-id="f1d09-164">To add a new parameter for the runbook, click **Add input** to open the **Runbook input parameter** blade.</span></span> <span data-ttu-id="f1d09-165">Konfigurálhatja, a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="f1d09-165">There, you can configure the following parameters:</span></span>
   
   | <span data-ttu-id="f1d09-166">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="f1d09-166">**Property**</span></span> | <span data-ttu-id="f1d09-167">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="f1d09-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="f1d09-168">Név</span><span class="sxs-lookup"><span data-stu-id="f1d09-168">Name</span></span> |<span data-ttu-id="f1d09-169">Szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1d09-169">Required.</span></span>  <span data-ttu-id="f1d09-170">A paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="f1d09-170">The name of the parameter.</span></span> <span data-ttu-id="f1d09-171">Ez a runbookon belül egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f1d09-171">This must be unique within the runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="f1d09-172">Betűvel kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="f1d09-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="f1d09-173">Leírás</span><span class="sxs-lookup"><span data-stu-id="f1d09-173">Description</span></span> |<span data-ttu-id="f1d09-174">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-174">Optional.</span></span> <span data-ttu-id="f1d09-175">Leírás a bemeneti paraméter céljáról.</span><span class="sxs-lookup"><span data-stu-id="f1d09-175">Description about the purpose of input parameter.</span></span> |
   | <span data-ttu-id="f1d09-176">Típus</span><span class="sxs-lookup"><span data-stu-id="f1d09-176">Type</span></span> |<span data-ttu-id="f1d09-177">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-177">Optional.</span></span> <span data-ttu-id="f1d09-178">Az adattípus, amely a paraméterérték várt.</span><span class="sxs-lookup"><span data-stu-id="f1d09-178">The data type that's expected for the parameter value.</span></span> <span data-ttu-id="f1d09-179">Támogatott paramétertípusa **karakterlánc**, **Int32**, **Int64**, **decimális**, **logikai**,  **Dátum és idő**, és **objektum**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="f1d09-180">Ha adattípus nincs megadva, alapértelmezés szerint a **karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-180">If a data type is not selected, it defaults to **String**.</span></span> |
   | <span data-ttu-id="f1d09-181">Kötelező</span><span class="sxs-lookup"><span data-stu-id="f1d09-181">Mandatory</span></span> |<span data-ttu-id="f1d09-182">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-182">Optional.</span></span> <span data-ttu-id="f1d09-183">Meghatározza, hogy értéket kell megadni a paraméterben.</span><span class="sxs-lookup"><span data-stu-id="f1d09-183">Specifies whether a value must be provided for the parameter.</span></span> <span data-ttu-id="f1d09-184">Ha úgy dönt, **Igen**, akkor meg kell adni egy értéket, a runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="f1d09-184">If you choose **yes**, then a value must be provided when the runbook is started.</span></span> <span data-ttu-id="f1d09-185">Ha úgy dönt, **nem**, majd kitöltése nem kötelező a forgatókönyv indításakor, és egy alapértelmezett érték is beállítható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-185">If you choose **no**, then a value is not required when the runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="f1d09-186">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="f1d09-186">Default Value</span></span> |<span data-ttu-id="f1d09-187">Választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-187">Optional.</span></span> <span data-ttu-id="f1d09-188">Adja meg egy értéket, amely a paraméter alkalmazva lesznek, ha az érték nem kerül át, a runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="f1d09-188">Specifies a value that will be used for the parameter if a value is not passed in when the runbook is started.</span></span> <span data-ttu-id="f1d09-189">Alapértelmezett érték a nem kötelező paraméter állítható be.</span><span class="sxs-lookup"><span data-stu-id="f1d09-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="f1d09-190">Alapértelmezett érték beállításához válassza **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-190">To set a default value, choose **Custom**.</span></span> <span data-ttu-id="f1d09-191">Ezt az értéket használja, ha egy másik érték lett megadva, a runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="f1d09-191">This value is used unless another value is provided when the runbook is started.</span></span> <span data-ttu-id="f1d09-192">Válasszon **nincs** Ha nem kívánja minden alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-192">Choose **None** if you don’t want to provide any default value.</span></span> |
   
    ![Új bemenet hozzáadása](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="f1d09-194">Hozzon létre két paramétert a következő tulajdonságokkal, amelyek használják a **Get-AzureRmVm** tevékenység:</span><span class="sxs-lookup"><span data-stu-id="f1d09-194">Create two parameters with the following properties that will be used by the **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="f1d09-195">**Parameter1:**</span><span class="sxs-lookup"><span data-stu-id="f1d09-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="f1d09-196">Név – VMName</span><span class="sxs-lookup"><span data-stu-id="f1d09-196">Name - VMName</span></span>
     * <span data-ttu-id="f1d09-197">-Karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="f1d09-197">Type - String</span></span>
     * <span data-ttu-id="f1d09-198">Kötelező – nem</span><span class="sxs-lookup"><span data-stu-id="f1d09-198">Mandatory - No</span></span>
   * <span data-ttu-id="f1d09-199">**2:**</span><span class="sxs-lookup"><span data-stu-id="f1d09-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="f1d09-200">Név - erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="f1d09-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="f1d09-201">-Karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="f1d09-201">Type - String</span></span>
     * <span data-ttu-id="f1d09-202">Kötelező – nem</span><span class="sxs-lookup"><span data-stu-id="f1d09-202">Mandatory - No</span></span>
     * <span data-ttu-id="f1d09-203">Alapértelmezett érték – egyéni</span><span class="sxs-lookup"><span data-stu-id="f1d09-203">Default value - Custom</span></span>
     * <span data-ttu-id="f1d09-204">Egyéni alapértelmezett érték – \<a virtuális gépeket tartalmazó erőforráscsoport neve ></span><span class="sxs-lookup"><span data-stu-id="f1d09-204">Custom default value - \<Name of the resource group that contains the virtual machines></span></span>
5. <span data-ttu-id="f1d09-205">Miután hozzáadta a paramétereket, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-205">Once you add the parameters, click **OK**.</span></span>  <span data-ttu-id="f1d09-206">Most már megtekintheti azokat a **bemeneti és kimeneti panel**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-206">You can now view them in the **Input and output blade**.</span></span> <span data-ttu-id="f1d09-207">Kattintson a **OK** újra, és kattintson a **mentése** és **közzététel** a runbookban.</span><span class="sxs-lookup"><span data-stu-id="f1d09-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-to-input-parameters-in-runbooks"></a><span data-ttu-id="f1d09-208">A bemeneti paraméterek runbookok megadása</span><span class="sxs-lookup"><span data-stu-id="f1d09-208">Assign values to input parameters in runbooks</span></span>
<span data-ttu-id="f1d09-209">Átadhatók értékek a felhasználótól a következő esetekben a runbookok paraméterei.</span><span class="sxs-lookup"><span data-stu-id="f1d09-209">You can pass values to input parameters in runbooks in the following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="f1d09-210">Elindítja a runbookot, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="f1d09-211">Egy runbook indítható számos módja: az Azure-portálon, és olyan webhook, a PowerShell-parancsmagokkal, a REST API-t vagy az SDK-val.</span><span class="sxs-lookup"><span data-stu-id="f1d09-211">A runbook can be started many ways: through the Azure portal, with a webhook, with PowerShell cmdlets, with the REST API, or with the SDK.</span></span> <span data-ttu-id="f1d09-212">Az alábbiakban arról lesz szó különböző módszereket a runbook indítása, és rendelje hozzá a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a><span data-ttu-id="f1d09-213">Az Azure-portál használatával indítsa el a közzétett runbookok, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-213">Start a published runbook by using the Azure portal and assign parameters</span></span>
<span data-ttu-id="f1d09-214">Ha Ön [elindítja a runbookot](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), a **runbook meghívása** panel nyílik meg, és beállíthatja, hogy az imént létrehozott paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-214">When you [start the runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), the **Start Runbook** blade opens and you can configure values for the parameters that you just created.</span></span>

![A portál használatának megkezdése](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="f1d09-216">A címke alatt a beviteli mezőbe lásd: a paraméter beállított attribútumait.</span><span class="sxs-lookup"><span data-stu-id="f1d09-216">In the label beneath the input box, you can see the attributes that have been set for the parameter.</span></span> <span data-ttu-id="f1d09-217">Attribútumok kötelező vagy választható, típusa és alapértelmezett értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f1d09-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="f1d09-218">A paraméter neve melletti súgó buborék megtekintheti a kulcs adatainak meg kell győződnie döntéseket hozzanak arról, hogy a paraméter bemeneti értékeket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-218">In the help balloon next to the parameter name, you can see all the key information you need to make decisions about parameter input values.</span></span> <span data-ttu-id="f1d09-219">Ezen információk közé tartozik, hogy a paraméter kötelező vagy választható.</span><span class="sxs-lookup"><span data-stu-id="f1d09-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="f1d09-220">A típus és alapértelmezett értéket (ha van ilyen) és más hasznos megjegyzéseket is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f1d09-220">It also includes the type and default value (if any), and other helpful notes.</span></span>

![A buborékban megjelenő Súgó](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="f1d09-222">Karakterlánc típusú paramétereket támogatási **üres** karakterlánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="f1d09-223">Belépés **[EmptyString]** a bemeneti paraméter a mező üres karakterláncra a paraméter fogja továbbítani.</span><span class="sxs-lookup"><span data-stu-id="f1d09-223">Entering **[EmptyString]** in the input parameter box will pass an empty string to the parameter.</span></span> <span data-ttu-id="f1d09-224">Ezenkívül nem támogatják a karakterlánc típusú paraméterrel **Null** átadta-e értékeket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="f1d09-225">Ha bármely érték nem adhatók át a karakterlánc-paramétert, majd PowerShell értelmezi null értékként.</span><span class="sxs-lookup"><span data-stu-id="f1d09-225">If you don’t pass any value to the String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="f1d09-226">Indítsa el a közzétett runbookok PowerShell-parancsmagok használatával, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="f1d09-227">**Az Azure Resource Manager parancsmagjainak:** elindíthatja egy hozott létre egy erőforráscsoportot az Automation-runbook [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1d09-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="f1d09-228">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f1d09-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="f1d09-229">**Azure szolgáltatásfelügyelet parancsmagok:** elindíthatja az automation-forgatókönyv használatával létrehozott egy alapértelmezett erőforráscsoportban [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1d09-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="f1d09-230">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f1d09-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="f1d09-231">Amikor elindít egy runbookot PowerShell-parancsmagok, egy alapértelmezett paraméter használatával **MicrosoftApplicationManagementStartedBy** jön létre a értékű **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="f1d09-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with the value **PowerShell**.</span></span> <span data-ttu-id="f1d09-232">Megtekintheti a paraméter a **feladat részletei** panelen.</span><span class="sxs-lookup"><span data-stu-id="f1d09-232">You can view this parameter in the **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="f1d09-233">Az SDK használatával indítsa el a runbookot, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="f1d09-234">**Az Azure Resource Manager-metódus:** runbook elindíthatja az SDK-t egy programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="f1d09-234">**Azure Resource Manager method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="f1d09-235">Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="f1d09-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="f1d09-236">Megtekintheti, a kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="f1d09-236">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
  ```
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```
* <span data-ttu-id="f1d09-237">**Azure szolgáltatásfelügyelet metódus:** runbook elindíthatja az SDK-t egy programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="f1d09-237">**Azure Service Management method:** You can start a runbook by using the SDK of a programming language.</span></span> <span data-ttu-id="f1d09-238">Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="f1d09-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="f1d09-239">Megtekintheti, a kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="f1d09-239">You can view all the code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
  ```      
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```
  
  <span data-ttu-id="f1d09-240">Ez a metódus indításához a runbook paramétereinek tárolására szótár **VMName** és **resourceGroupName**, és azok értékeit.</span><span class="sxs-lookup"><span data-stu-id="f1d09-240">To start this method, create a dictionary to store the runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="f1d09-241">Ezután elindítja a runbookot.</span><span class="sxs-lookup"><span data-stu-id="f1d09-241">Then start the runbook.</span></span> <span data-ttu-id="f1d09-242">A C# kódrészletet a fent megadott metódust az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="f1d09-242">Below is the C# code snippet for calling the method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a><span data-ttu-id="f1d09-243">Runbook indítása a REST API használatával, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-243">Start a runbook by using the REST API and assign parameters</span></span>
<span data-ttu-id="f1d09-244">A runbook-feladatok létrehozhatók és lépések az Azure Automation REST API használatával a **PUT** metódus a következő kérelem URI-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="f1d09-244">A runbook job can be created and started with the Azure Automation REST API by using the **PUT** method with the following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="f1d09-245">A kérelem URI-azonosítója cserélje le a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="f1d09-245">In the request URI, replace the following parameters:</span></span>

* <span data-ttu-id="f1d09-246">**előfizetés-azonosító:** az Azure-előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="f1d09-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="f1d09-247">**felhő szolgáltatásnév:** a nevét, a felhőalapú szolgáltatás, amelyre a kérelmet küldeni kell.</span><span class="sxs-lookup"><span data-stu-id="f1d09-247">**cloud-service-name:** The name of the cloud service to which the request should be sent.</span></span>  
* <span data-ttu-id="f1d09-248">**Automation-fiók-name:** , amely a megadott felhő-szolgáltatáson belül az automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-248">**automation-account-name:** The name of your automation account that's hosted within the specified cloud service.</span></span>  
* <span data-ttu-id="f1d09-249">**azonosítójú feladat:** a feladathoz tartozó GUID.</span><span class="sxs-lookup"><span data-stu-id="f1d09-249">**job-id:** The GUID for the job.</span></span> <span data-ttu-id="f1d09-250">GUID azonosítók a PowerShell használatával hozhatók létre a **[GUID]::NewGuid(). ToString()** parancsot.</span><span class="sxs-lookup"><span data-stu-id="f1d09-250">GUIDs in PowerShell can be created by using the **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="f1d09-251">Ahhoz, hogy a paraméterek átadása a runbook-feladat, használja a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="f1d09-251">In order to pass parameters to the runbook job, use the request body.</span></span> <span data-ttu-id="f1d09-252">A következő két tulajdonságait JSON formátumban megadott tart:</span><span class="sxs-lookup"><span data-stu-id="f1d09-252">It takes the following two properties provided in JSON format:</span></span>

* <span data-ttu-id="f1d09-253">**Runbook neve:** szükséges.</span><span class="sxs-lookup"><span data-stu-id="f1d09-253">**Runbook name:** Required.</span></span> <span data-ttu-id="f1d09-254">A feladat elindítása a runbook neve.</span><span class="sxs-lookup"><span data-stu-id="f1d09-254">The name of the runbook for the job to start.</span></span>  
* <span data-ttu-id="f1d09-255">**Runbook-paraméterek:** nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="f1d09-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="f1d09-256">A paraméterlista (név, érték) álló dictionary formátumban, ahol neve karakterlánc típusúnak kell lennie, és értéke lehet bármely érvényes JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="f1d09-256">A dictionary of the parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="f1d09-257">Ha el szeretné indítani a **Get-AzureVMTextual** hoztak létre korábbi runbook **VMName** és **resourceGroupName** paraméterek, használja a következő JSON formátummal a a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="f1d09-257">If you want to start the **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use the following JSON format for the request body.</span></span>

   ```
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WSVMClassic",
         "resourceGroupName":”WSCS1”}
        }
    }
   ```

<span data-ttu-id="f1d09-258">A 201-es HTTP-állapotkód eredményül, ha a feladat sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="f1d09-258">A HTTP status code 201 is returned if the job is successfully created.</span></span> <span data-ttu-id="f1d09-259">Válaszfejlécek és az adott válasz törzsének további információkért tekintse meg a cikk kapcsolatos [egy runbook-feladat létrehozása a REST API használatával.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="f1d09-259">For more information on response headers and the response body, refer to the article about how to [create a runbook job by using the REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="f1d09-260">Egy runbook tesztelése, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="f1d09-261">Ha Ön [tesztelése a runbook vázlatverzióját](automation-testing-runbook.md) a vizsgálat lehetőséget a **tesztelése** panel nyílik meg, és beállíthatja, hogy az imént létrehozott paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="f1d09-261">When you [test the draft version of your runbook](automation-testing-runbook.md) by using the test option, the **Test** blade opens and you can configure values for the parameters that you just created.</span></span>

![Tesztelje, és rendelje hozzá a paraméterek](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a><span data-ttu-id="f1d09-263">Ütemezés összekapcsolása runbookkal, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-263">Link a schedule to a runbook and assign parameters</span></span>
<span data-ttu-id="f1d09-264">Is [összekapcsolhat egy ütemezést](automation-schedules.md) a forgatókönyvhöz való úgy, hogy a runbook egy adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="f1d09-264">You can [link a schedule](automation-schedules.md) to your runbook so that the runbook starts at a specific time.</span></span> <span data-ttu-id="f1d09-265">Ha az ütemezés létrehozása, és a runbook ezeket az értékeket fogja használni, amikor elindul az ütemezés szerint rendelhet bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-265">You assign input parameters when you create the schedule, and the runbook will use these values when it is started by the schedule.</span></span> <span data-ttu-id="f1d09-266">Az ütemezés nem menthető, amíg az összes kötelező paraméter az értékek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-266">You can’t save the schedule until all mandatory parameter values are provided.</span></span>

![Az ütemezés és a hozzárendelés paraméterek](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="f1d09-268">A webhook egy runbook létrehozása és hozzárendelése paraméterek</span><span class="sxs-lookup"><span data-stu-id="f1d09-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="f1d09-269">Létrehozhat egy [webhook](automation-webhooks.md) a runbook és konfigurálhatja a runbook bemeneti paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="f1d09-270">A webhook nem menthetők, amíg az összes kötelező paraméter az értékek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-270">You can’t save the webhook until all mandatory parameter values are provided.</span></span>

![Webhook létrehozása és hozzárendelése paraméterek](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="f1d09-272">Ha olyan webhook, az előre meghatározott bemeneti paraméter használatával runbookot végrehajtani  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  , együtt küldött, amelyet a bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f1d09-272">When you execute a runbook by using a webhook, the predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with the input parameters that you defined.</span></span> <span data-ttu-id="f1d09-273">Kattintva bontsa ki a **WebhookData** paraméter további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f1d09-273">You can click to expand the **WebhookData** parameter for more details.</span></span>

![WebhookData paraméter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="f1d09-275">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1d09-275">Next steps</span></span>
* <span data-ttu-id="f1d09-276">A forgatókönyv bemeneti és kimeneti további információkért lásd: [Azure Automation: runbook bemeneti, kimeneti és egymásba ágyazott runbookok](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="f1d09-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="f1d09-277">Runbook-indítási módok részletekért lásd: [runbook indítása](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f1d09-277">For details about different ways to start a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="f1d09-278">Szöveges forgatókönyvként szerkesztéséhez hivatkoznak [szöveges runbookok szerkesztéséhez](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f1d09-278">To edit a textual runbook, refer to [Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="f1d09-279">Grafikus runbook szerkesztése, tekintse meg [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="f1d09-279">To edit a graphical runbook, refer to [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

