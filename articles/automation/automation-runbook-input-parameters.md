---
title: "a bemeneti paraméterek aaaRunbook |} Microsoft Docs"
description: "Runbook bemeneti paraméterek rugalmasabb hello a runbookok lehetővé teszik, hogy toopass adatok tooa runbook indításakor. Ez a cikk ismerteti a különböző alkalmazási helyzetek, amely bemeneti paraméterek runbookok használják."
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
ms.openlocfilehash: f3abaf92382e7d41019616bafb14af23cf98dd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-input-parameters"></a><span data-ttu-id="ee357-104">Runbook bemeneti paraméterei</span><span class="sxs-lookup"><span data-stu-id="ee357-104">Runbook input parameters</span></span>
<span data-ttu-id="ee357-105">Runbook bemeneti paraméterek rugalmasabb hello a runbookok lehetővé teszik, hogy toopass adatok tooit indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-105">Runbook input parameters increase hello flexibility of runbooks by allowing you toopass data tooit when it is started.</span></span> <span data-ttu-id="ee357-106">hello paraméterek segítségével hello runbook műveletek toobe érvényes az adott forgatókönyvekben és környezetekben.</span><span class="sxs-lookup"><span data-stu-id="ee357-106">hello parameters allow hello runbook actions toobe targeted for specific scenarios and environments.</span></span> <span data-ttu-id="ee357-107">Ez a cikk azt haladhat végig különböző alkalmazási helyzetek runbookok helyének bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ee357-107">In this article, we will walk you through different scenarios where input parameters are used in runbooks.</span></span>

## <a name="configure-input-parameters"></a><span data-ttu-id="ee357-108">A bemeneti paraméterek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee357-108">Configure input parameters</span></span>
<span data-ttu-id="ee357-109">A bemeneti paramétereket a PowerShell, a PowerShell-munkafolyamatok és a grafikus forgatókönyvek konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="ee357-109">Input parameters can be configured in PowerShell, PowerShell Workflow, and graphical runbooks.</span></span> <span data-ttu-id="ee357-110">Egy runbook is rendelkezik különböző adattípusokkal több paramétert, vagy nincsenek paraméterei egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="ee357-110">A runbook can have multiple parameters with different data types, or no parameters at all.</span></span> <span data-ttu-id="ee357-111">Kötelező vagy választható lehet bemeneti paramétereket, és hozzá lehet rendelni egy választható paramétereik alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="ee357-111">Input parameters can be mandatory or optional, and you can assign a default value for optional parameters.</span></span> <span data-ttu-id="ee357-112">Megadhat értéket toohello bemeneti paraméterek egy runbook hello rendelkezésre álló módszerek egyikével indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-112">You can assign values toohello input parameters for a runbook when you start it through one of hello available methods.</span></span> <span data-ttu-id="ee357-113">Ezek a metódusok lehetnek egy runbook indítása hello portal vagy webszolgáltatás futhat.</span><span class="sxs-lookup"><span data-stu-id="ee357-113">These methods include starting  a runbook from hello portal  or a web service.</span></span> <span data-ttu-id="ee357-114">Akkor is elindíthatja, mint amit a gyermekrunbook beágyazottan is nevezett egy másik runbook.</span><span class="sxs-lookup"><span data-stu-id="ee357-114">You can also start one as a child runbook that is called inline in another runbook.</span></span>

## <a name="configure-input-parameters-in-powershell-and-powershell-workflow-runbooks"></a><span data-ttu-id="ee357-115">Konfigurálhatja a bemeneti paramétereket a PowerShell és a PowerShell-munkafolyamati forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="ee357-115">Configure input parameters in PowerShell and PowerShell Workflow runbooks</span></span>
<span data-ttu-id="ee357-116">PowerShell és [PowerShell munkafolyamat-forgatókönyvekről](automation-first-runbook-textual.md) az Azure Automationben támogatja a következő attribútumok hello keresztül meghatározott bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ee357-116">PowerShell and [PowerShell Workflow runbooks](automation-first-runbook-textual.md) in Azure Automation support input parameters that are defined through hello following attributes.</span></span>  

| <span data-ttu-id="ee357-117">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="ee357-117">**Property**</span></span> | <span data-ttu-id="ee357-118">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="ee357-118">**Description**</span></span> |
|:--- |:--- |
| <span data-ttu-id="ee357-119">Típus</span><span class="sxs-lookup"><span data-stu-id="ee357-119">Type</span></span> |<span data-ttu-id="ee357-120">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-120">Required.</span></span> <span data-ttu-id="ee357-121">hello adattípus hello paraméter értéket vár.</span><span class="sxs-lookup"><span data-stu-id="ee357-121">hello data type expected for hello parameter value.</span></span> <span data-ttu-id="ee357-122">A .NET-típus érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="ee357-122">Any .NET type is valid.</span></span> |
| <span data-ttu-id="ee357-123">Név</span><span class="sxs-lookup"><span data-stu-id="ee357-123">Name</span></span> |<span data-ttu-id="ee357-124">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-124">Required.</span></span> <span data-ttu-id="ee357-125">hello hello paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="ee357-125">hello name of hello parameter.</span></span> <span data-ttu-id="ee357-126">Ez kell hello runbookon belüli egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ee357-126">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="ee357-127">Betűvel kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="ee357-127">It must start with a letter.</span></span> |
| <span data-ttu-id="ee357-128">Kötelező</span><span class="sxs-lookup"><span data-stu-id="ee357-128">Mandatory</span></span> |<span data-ttu-id="ee357-129">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-129">Optional.</span></span> <span data-ttu-id="ee357-130">Meghatározza, hogy egy érték hello paramétert meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="ee357-130">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="ee357-131">Ha ez túl**$true**, akkor meg kell adni egy értéket, hello runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-131">If you set this too**$true**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="ee357-132">Ha ez túl**$false**, majd egy értéket nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="ee357-132">If you set this too**$false**, then a value is optional.</span></span> |
| <span data-ttu-id="ee357-133">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ee357-133">Default value</span></span> |<span data-ttu-id="ee357-134">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-134">Optional.</span></span>  <span data-ttu-id="ee357-135">Adja meg egy értéket, amely hello paraméter alkalmazva lesznek, ha az érték nem kerül át a hello runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-135">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="ee357-136">Alapértelmezett érték bármelyik paraméternél állítható be, és automatikusan teszik hello paraméter függetlenül hello kötelező beállítás nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-136">A default value can be set for any parameter and will automatically make hello parameter optional regardless of hello Mandatory setting.</span></span> |

<span data-ttu-id="ee357-137">Windows PowerShell támogatja a bemeneti paraméterek további attribútumok azokat az itt felsorolt, érvényesítése, például aliasokat, és a paraméter állandóként állítja be.</span><span class="sxs-lookup"><span data-stu-id="ee357-137">Windows PowerShell supports more attributes of input parameters than those listed here, like validation, aliases, and parameter sets.</span></span> <span data-ttu-id="ee357-138">Azure Automation jelenleg csak hello bemeneti paraméterek fent felsorolt támogatja.</span><span class="sxs-lookup"><span data-stu-id="ee357-138">However, Azure Automation currently supports only hello input parameters listed above.</span></span>

<span data-ttu-id="ee357-139">A paraméter PowerShell munkafolyamat-forgatókönyvekről van hello következő általános űrlapot, ahol több paraméter vesszővel kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="ee357-139">A parameter definition in PowerShell Workflow runbooks has hello following general form, where multiple parameters are separated by commas.</span></span>

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
> <span data-ttu-id="ee357-140">Ha amely akkor paraméterek, ha nem adja meg a hello **kötelező** attribútum, akkor alapértelmezés szerint hello paraméter megadása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-140">When you're defining parameters, if you don’t specify hello **Mandatory** attribute, then by default, hello parameter is considered optional.</span></span> <span data-ttu-id="ee357-141">Is, ha a paraméter alapértelmezett értékét a PowerShell munkafolyamat-forgatókönyvekről, azt minősül PowerShell, nem kötelező paraméter, függetlenül attól, hello **kötelező** attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="ee357-141">Also, if you set a default value for a parameter in PowerShell Workflow runbooks, it will be treated by PowerShell as an optional parameter, regardless of hello **Mandatory** attribute value.</span></span>
> 
> 

<span data-ttu-id="ee357-142">Tegyük fel pedig konfiguráljuk hello bemeneti paramétereinek egy PowerShell-munkafolyamati forgatókönyv, amely virtuális gépeket, vagy egy virtuális vagy az erőforráscsoporton belül minden virtuális gép adatait.</span><span class="sxs-lookup"><span data-stu-id="ee357-142">As an example, let’s configure hello input parameters for a PowerShell Workflow runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="ee357-143">Ez a forgatókönyv két paraméterrel rendelkezik, ahogy az alábbi képernyőfelvétel a hello: hello nevét, valamint a virtuális gép hello hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="ee357-143">This runbook has two parameters as shown in hello following screenshot: hello name of virtual machine and hello name of hello resource group.</span></span>

![Automatizálás PowerShell-munkafolyamat](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

<span data-ttu-id="ee357-145">Ez a paraméter-definícióban hello paraméterek **$VMName** és **$resourceGroupName** egyszerű paraméterek karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="ee357-145">In this parameter definition, hello parameters **$VMName** and **$resourceGroupName** are simple parameters of type string.</span></span> <span data-ttu-id="ee357-146">Azonban PowerShell és a PowerShell-munkafolyamati forgatókönyvek támogatja az egyszerű típusok és a komplex típusok, például a **objektum** vagy **PSCredential** -bemeneti paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="ee357-146">However, PowerShell and PowerShell Workflow runbooks support all simple types and complex types, such as **object** or **PSCredential** for input parameters.</span></span>

<span data-ttu-id="ee357-147">Ha a runbook rendelkezik az objektum típusú bemeneti paraméter, majd a PowerShell kivonattáblaként (név, érték) párokat toopass értékét.</span><span class="sxs-lookup"><span data-stu-id="ee357-147">If your runbook has an object type input parameter, then use a PowerShell hashtable with (name,value) pairs toopass in a value.</span></span> <span data-ttu-id="ee357-148">Ha például van egy runbook paraméter a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ee357-148">For example, if you have hello following parameter in a runbook:</span></span>

     [Parameter (Mandatory = $true)]
     [object] $FullName

<span data-ttu-id="ee357-149">Akkor is továbbítja a következő érték toohello paraméter hello:</span><span class="sxs-lookup"><span data-stu-id="ee357-149">Then you can pass hello following value toohello parameter:</span></span>

    @{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}


## <a name="configure-input-parameters-in-graphical-runbooks"></a><span data-ttu-id="ee357-150">Grafikus forgatókönyvek bemeneti paramétereinek a konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ee357-150">Configure input parameters in graphical runbooks</span></span>
<span data-ttu-id="ee357-151">túl[konfigurálása egy grafikus forgatókönyvnek](automation-first-runbook-graphical.md) bemeneti paraméterek, hozzon létre egy grafikus forgatókönyv, amely virtuális gépek adatait vagy egy virtuális vagy virtuális gépeinek erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="ee357-151">too[configure a graphical runbook](automation-first-runbook-graphical.md) with input parameters, let’s create a graphical runbook that outputs details about virtual machines, either a single VM or all VMs within a resource group.</span></span> <span data-ttu-id="ee357-152">Egy runbook konfigurálása áll két fő tevékenység az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="ee357-152">Configuring a runbook consists of two major activities, as described below.</span></span>

<span data-ttu-id="ee357-153">[**Azure-beli futtató fiókot a Runbookok hitelesítést** ](automation-sec-configure-azure-runas-account.md) tooauthenticate az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="ee357-153">[**Authenticate Runbooks with Azure Run As account**](automation-sec-configure-azure-runas-account.md) tooauthenticate with Azure.</span></span>

<span data-ttu-id="ee357-154">[**Get-AzureRmVm** ](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello tulajdonságai egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ee357-154">[**Get-AzureRmVm**](https://msdn.microsoft.com/library/mt603718.aspx) tooget hello properties of a virtual machines.</span></span>

<span data-ttu-id="ee357-155">Használhatja a hello [ **Write-Output** ](https://technet.microsoft.com/library/hh849921.aspx) tevékenységek toooutput hello nevének a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="ee357-155">You can use hello [**Write-Output**](https://technet.microsoft.com/library/hh849921.aspx) activity toooutput hello names of virtual machines.</span></span> <span data-ttu-id="ee357-156">tevékenység hello **Get-AzureRmVm** két paramétert fogad, hello **virtuálisgép-nevet** és hello **erőforráscsoport-név**.</span><span class="sxs-lookup"><span data-stu-id="ee357-156">hello activity **Get-AzureRmVm** accepts two parameters, hello **virtual machine name** and hello **resource group name**.</span></span> <span data-ttu-id="ee357-157">Mivel ezek a paraméterek hello runbook minden egyes indításakor igényelhet eltérő értékek tartoznak, a bemeneti paraméterek tooyour runbook is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="ee357-157">Since these parameters could require different values each time you start hello runbook, you can add input parameters tooyour runbook.</span></span> <span data-ttu-id="ee357-158">Az alábbiakban hello lépéseket tooadd bemeneti paraméterek:</span><span class="sxs-lookup"><span data-stu-id="ee357-158">Here are hello steps tooadd input parameters:</span></span>

1. <span data-ttu-id="ee357-159">Válassza ki a hello grafikus forgatókönyvnek hello **Runbookok** panel megnyitásához, és kattintson [ **szerkesztése** ](automation-graphical-authoring-intro.md) azt.</span><span class="sxs-lookup"><span data-stu-id="ee357-159">Select hello graphical runbook from hello **Runbooks** blade and then click [**Edit**](automation-graphical-authoring-intro.md) it.</span></span>
2. <span data-ttu-id="ee357-160">Hello forgatókönyv-szerkesztőben kattintson **bemeneti és kimeneti** tooopen hello **bemeneti és kimeneti** panelen.</span><span class="sxs-lookup"><span data-stu-id="ee357-160">From hello runbook editor, click **Input and output** tooopen hello **Input and output** blade.</span></span>
   
    ![Grafikus Automation-forgatókönyv](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)
3. <span data-ttu-id="ee357-162">Hello **bemeneti és kimeneti** panel hello runbook meghatározott bemeneti paraméterek listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ee357-162">hello **Input and output** blade displays a list of input parameters that are defined for hello runbook.</span></span> <span data-ttu-id="ee357-163">A panel adja hozzá egy új bemeneti paramétert, vagy egy meglévő bemeneti paraméter hello konfigurációjának a szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="ee357-163">On this blade, you can either add a new input parameter or edit hello configuration of an existing input parameter.</span></span> <span data-ttu-id="ee357-164">egy új paraméter hello runbook tooadd kattintson **bemenet hozzáadása** tooopen hello **Runbook bemeneti paraméter** panelen.</span><span class="sxs-lookup"><span data-stu-id="ee357-164">tooadd a new parameter for hello runbook, click **Add input** tooopen hello **Runbook input parameter** blade.</span></span> <span data-ttu-id="ee357-165">Hiba a következő paraméterek hello is konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="ee357-165">There, you can configure hello following parameters:</span></span>
   
   | <span data-ttu-id="ee357-166">**Tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="ee357-166">**Property**</span></span> | <span data-ttu-id="ee357-167">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="ee357-167">**Description**</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="ee357-168">Név</span><span class="sxs-lookup"><span data-stu-id="ee357-168">Name</span></span> |<span data-ttu-id="ee357-169">Kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-169">Required.</span></span>  <span data-ttu-id="ee357-170">hello hello paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="ee357-170">hello name of hello parameter.</span></span> <span data-ttu-id="ee357-171">Ez kell hello runbookon belüli egyedi és is csak betűket, számokat, vagy aláhúzás karaktereket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ee357-171">This must be unique within hello runbook, and can contain only letters, numbers, or underscore characters.</span></span> <span data-ttu-id="ee357-172">Betűvel kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="ee357-172">It must start with a letter.</span></span> |
   | <span data-ttu-id="ee357-173">Leírás</span><span class="sxs-lookup"><span data-stu-id="ee357-173">Description</span></span> |<span data-ttu-id="ee357-174">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-174">Optional.</span></span> <span data-ttu-id="ee357-175">Bemeneti paraméter hello céljának leírása.</span><span class="sxs-lookup"><span data-stu-id="ee357-175">Description about hello purpose of input parameter.</span></span> |
   | <span data-ttu-id="ee357-176">Típus</span><span class="sxs-lookup"><span data-stu-id="ee357-176">Type</span></span> |<span data-ttu-id="ee357-177">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-177">Optional.</span></span> <span data-ttu-id="ee357-178">hello adattípus, amely hello paraméterérték várt.</span><span class="sxs-lookup"><span data-stu-id="ee357-178">hello data type that's expected for hello parameter value.</span></span> <span data-ttu-id="ee357-179">Támogatott paramétertípusa **karakterlánc**, **Int32**, **Int64**, **decimális**, **logikai**,  **Dátum és idő**, és **objektum**.</span><span class="sxs-lookup"><span data-stu-id="ee357-179">Supported parameter types are **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime**, and **Object**.</span></span> <span data-ttu-id="ee357-180">Ha adattípus nincs bejelölve, az alapértelmezés szerinti érték túl**karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="ee357-180">If a data type is not selected, it defaults too**String**.</span></span> |
   | <span data-ttu-id="ee357-181">Kötelező</span><span class="sxs-lookup"><span data-stu-id="ee357-181">Mandatory</span></span> |<span data-ttu-id="ee357-182">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-182">Optional.</span></span> <span data-ttu-id="ee357-183">Meghatározza, hogy egy érték hello paramétert meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="ee357-183">Specifies whether a value must be provided for hello parameter.</span></span> <span data-ttu-id="ee357-184">Ha úgy dönt, **Igen**, akkor meg kell adni egy értéket, hello runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-184">If you choose **yes**, then a value must be provided when hello runbook is started.</span></span> <span data-ttu-id="ee357-185">Ha úgy dönt, **nem**, majd kitöltése nem kötelező hello forgatókönyv indításakor, és egy alapértelmezett érték is beállítható.</span><span class="sxs-lookup"><span data-stu-id="ee357-185">If you choose **no**, then a value is not required when hello runbook is started, and a default value may be set.</span></span> |
   | <span data-ttu-id="ee357-186">Alapértelmezett érték</span><span class="sxs-lookup"><span data-stu-id="ee357-186">Default Value</span></span> |<span data-ttu-id="ee357-187">Választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-187">Optional.</span></span> <span data-ttu-id="ee357-188">Adja meg egy értéket, amely hello paraméter alkalmazva lesznek, ha az érték nem kerül át a hello runbook indításakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-188">Specifies a value that will be used for hello parameter if a value is not passed in when hello runbook is started.</span></span> <span data-ttu-id="ee357-189">Alapértelmezett érték a nem kötelező paraméter állítható be.</span><span class="sxs-lookup"><span data-stu-id="ee357-189">A default value can be set for a parameter that's not mandatory.</span></span> <span data-ttu-id="ee357-190">Válasszon tooset alapértelmezés **egyéni**.</span><span class="sxs-lookup"><span data-stu-id="ee357-190">tooset a default value, choose **Custom**.</span></span> <span data-ttu-id="ee357-191">Ezt az értéket használja, ha egy másik értéket biztosított hello runbook indítását.</span><span class="sxs-lookup"><span data-stu-id="ee357-191">This value is used unless another value is provided when hello runbook is started.</span></span> <span data-ttu-id="ee357-192">Válasszon **nincs** Ha nem szeretné, hogy tooprovide minden alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="ee357-192">Choose **None** if you don’t want tooprovide any default value.</span></span> |
   
    ![Új bemenet hozzáadása](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. <span data-ttu-id="ee357-194">Hozzon létre két paramétert a következő tulajdonságok hello által használt hello **Get-AzureRmVm** tevékenység:</span><span class="sxs-lookup"><span data-stu-id="ee357-194">Create two parameters with hello following properties that will be used by hello **Get-AzureRmVm** activity:</span></span>
   
   * <span data-ttu-id="ee357-195">**Parameter1:**</span><span class="sxs-lookup"><span data-stu-id="ee357-195">**Parameter1:**</span></span>
     
     * <span data-ttu-id="ee357-196">Név – VMName</span><span class="sxs-lookup"><span data-stu-id="ee357-196">Name - VMName</span></span>
     * <span data-ttu-id="ee357-197">-Karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="ee357-197">Type - String</span></span>
     * <span data-ttu-id="ee357-198">Kötelező – nem</span><span class="sxs-lookup"><span data-stu-id="ee357-198">Mandatory - No</span></span>
   * <span data-ttu-id="ee357-199">**2:**</span><span class="sxs-lookup"><span data-stu-id="ee357-199">**Parameter2:**</span></span>
     
     * <span data-ttu-id="ee357-200">Név - erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="ee357-200">Name - resourceGroupName</span></span>
     * <span data-ttu-id="ee357-201">-Karakterlánc típusú</span><span class="sxs-lookup"><span data-stu-id="ee357-201">Type - String</span></span>
     * <span data-ttu-id="ee357-202">Kötelező – nem</span><span class="sxs-lookup"><span data-stu-id="ee357-202">Mandatory - No</span></span>
     * <span data-ttu-id="ee357-203">Alapértelmezett érték – egyéni</span><span class="sxs-lookup"><span data-stu-id="ee357-203">Default value - Custom</span></span>
     * <span data-ttu-id="ee357-204">Egyéni alapértelmezett érték – \<hello virtuális gépeket tartalmazó erőforráscsoport hello neve ></span><span class="sxs-lookup"><span data-stu-id="ee357-204">Custom default value - \<Name of hello resource group that contains hello virtual machines></span></span>
5. <span data-ttu-id="ee357-205">Ha hello paraméterek hozzáadásához kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="ee357-205">Once you add hello parameters, click **OK**.</span></span>  <span data-ttu-id="ee357-206">Most már megtekintheti azokat a hello **bemeneti és kimeneti panel**.</span><span class="sxs-lookup"><span data-stu-id="ee357-206">You can now view them in hello **Input and output blade**.</span></span> <span data-ttu-id="ee357-207">Kattintson a **OK** újra, és kattintson a **mentése** és **közzététel** a runbookban.</span><span class="sxs-lookup"><span data-stu-id="ee357-207">Click **OK** again, and then click **Save** and **Publish** your runbook.</span></span>

## <a name="assign-values-tooinput-parameters-in-runbooks"></a><span data-ttu-id="ee357-208">Értékek társításához tooinput paraméterek a runbookokban</span><span class="sxs-lookup"><span data-stu-id="ee357-208">Assign values tooinput parameters in runbooks</span></span>
<span data-ttu-id="ee357-209">Átadhatók értékek tooinput paraméterek a runbookokat hello a következő forgatókönyvek a.</span><span class="sxs-lookup"><span data-stu-id="ee357-209">You can pass values tooinput parameters in runbooks in hello following scenarios.</span></span>

### <a name="start-a-runbook-and-assign-parameters"></a><span data-ttu-id="ee357-210">Elindítja a runbookot, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-210">Start a runbook and assign parameters</span></span>
<span data-ttu-id="ee357-211">Egy runbook indítható számos szempont: hello Azure-portálon, és olyan webhook, a PowerShell-parancsmagokkal, hello REST API vagy hello SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="ee357-211">A runbook can be started many ways: through hello Azure portal, with a webhook, with PowerShell cmdlets, with hello REST API, or with hello SDK.</span></span> <span data-ttu-id="ee357-212">Az alábbiakban arról lesz szó különböző módszereket a runbook indítása, és rendelje hozzá a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="ee357-212">Below we discuss different methods for starting a runbook and assigning parameters.</span></span>

#### <a name="start-a-published-runbook-by-using-hello-azure-portal-and-assign-parameters"></a><span data-ttu-id="ee357-213">Indítsa el a közzétett runbookok hello Azure portál segítségével, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-213">Start a published runbook by using hello Azure portal and assign parameters</span></span>
<span data-ttu-id="ee357-214">Ha Ön [hello runbook indítása](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **runbook meghívása** panel nyílik meg, és konfigurálhatja az imént létrehozott hello paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="ee357-214">When you [start hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal), hello **Start Runbook** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Indítsa el a hello portál használatával](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

<span data-ttu-id="ee357-216">Hello címke alatt hello beviteli mezőt, a hello attribútumok hello paraméter beállított látható.</span><span class="sxs-lookup"><span data-stu-id="ee357-216">In hello label beneath hello input box, you can see hello attributes that have been set for hello parameter.</span></span> <span data-ttu-id="ee357-217">Attribútumok kötelező vagy választható, típusa és alapértelmezett értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ee357-217">Attributes include mandatory or optional, type, and  default value.</span></span> <span data-ttu-id="ee357-218">A hello súgó buborékban megjelenő következő toohello paraméter neve megtekintheti az összes hello kulcsadatokat bemeneti paraméterértékek toomake döntéseket kell.</span><span class="sxs-lookup"><span data-stu-id="ee357-218">In hello help balloon next toohello parameter name, you can see all hello key information you need toomake decisions about parameter input values.</span></span> <span data-ttu-id="ee357-219">Ezen információk közé tartozik, hogy a paraméter kötelező vagy választható.</span><span class="sxs-lookup"><span data-stu-id="ee357-219">This information includes whether a parameter is mandatory or optional.</span></span> <span data-ttu-id="ee357-220">Hello típusa és alapértelmezett értéket (ha van ilyen) és más hasznos megjegyzéseket is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ee357-220">It also includes hello type and default value (if any), and other helpful notes.</span></span>

![A buborékban megjelenő Súgó](media/automation-runbook-input-parameters/automation-05-helpbaloon.png)

> [!NOTE]
> <span data-ttu-id="ee357-222">Karakterlánc típusú paramétereket támogatási **üres** karakterlánc-értékek.</span><span class="sxs-lookup"><span data-stu-id="ee357-222">String type parameters support **Empty** String values.</span></span>  <span data-ttu-id="ee357-223">Belépés **[EmptyString]** hello bemeneti paraméterben mezőbe egy üres karakterlánc toohello paraméter fogja továbbítani.</span><span class="sxs-lookup"><span data-stu-id="ee357-223">Entering **[EmptyString]** in hello input parameter box will pass an empty string toohello parameter.</span></span> <span data-ttu-id="ee357-224">Ezenkívül nem támogatják a karakterlánc típusú paraméterrel **Null** átadta-e értékeket.</span><span class="sxs-lookup"><span data-stu-id="ee357-224">Also, String type parameters don’t support **Null** values being passed.</span></span> <span data-ttu-id="ee357-225">Ha bármely érték toohello karakterlánc-paramétert nem adhatók át, majd PowerShell értelmezi null értékként.</span><span class="sxs-lookup"><span data-stu-id="ee357-225">If you don’t pass any value toohello String parameter, then PowerShell will interpret it as null.</span></span>
> 
> 

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a><span data-ttu-id="ee357-226">Indítsa el a közzétett runbookok PowerShell-parancsmagok használatával, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-226">Start a published runbook by using PowerShell cmdlets and assign parameters</span></span>
* <span data-ttu-id="ee357-227">**Az Azure Resource Manager parancsmagjainak:** elindíthatja egy hozott létre egy erőforráscsoportot az Automation-runbook [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee357-227">**Azure Resource Manager cmdlets:** You can start an Automation runbook that was created in a resource group by using [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx).</span></span>
  
  <span data-ttu-id="ee357-228">**Példa**</span><span class="sxs-lookup"><span data-stu-id="ee357-228">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”;”resourceGroupeName”=”WSVMClassicSG”}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” –ResourceGroupName $resourceGroupName -Parameters $params
  ```
* <span data-ttu-id="ee357-229">**Azure szolgáltatásfelügyelet parancsmagok:** elindíthatja az automation-forgatókönyv használatával létrehozott egy alapértelmezett erőforráscsoportban [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span><span class="sxs-lookup"><span data-stu-id="ee357-229">**Azure Service Management cmdlets:** You can start an automation runbook that was created in a default resource group by using [Start-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690259.aspx).</span></span>
  
  <span data-ttu-id="ee357-230">**Példa**</span><span class="sxs-lookup"><span data-stu-id="ee357-230">**Example:**</span></span>
  
  ```
  $params = @{“VMName”=”WSVMClassic”; ”ServiceName”=”WSVMClassicSG”}
  
  Start-AzureAutomationRunbook -AutomationAccountName “TestAutomation” -Name “Get-AzureVMGraphical” -Parameters $params
  ```

> [!NOTE]
> <span data-ttu-id="ee357-231">Amikor elindít egy runbookot PowerShell-parancsmagok, egy alapértelmezett paraméter használatával **MicrosoftApplicationManagementStartedBy** jön létre a hello érték **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ee357-231">When you start a runbook by using PowerShell cmdlets, a default parameter, **MicrosoftApplicationManagementStartedBy** is created with hello value **PowerShell**.</span></span> <span data-ttu-id="ee357-232">Ez a paraméter megtekintheti a hello **feladat részletei** panelen.</span><span class="sxs-lookup"><span data-stu-id="ee357-232">You can view this parameter in hello **Job details** blade.</span></span>  
> 
> 

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a><span data-ttu-id="ee357-233">Az SDK használatával indítsa el a runbookot, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-233">Start a runbook by using an SDK and assign parameters</span></span>
* <span data-ttu-id="ee357-234">**Az Azure Resource Manager-metódus:** megkezdése egy runbook hello SDK egy programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="ee357-234">**Azure Resource Manager method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="ee357-235">Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="ee357-235">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="ee357-236">Megtekintheti az összes hello kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="ee357-236">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>  
  
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
* <span data-ttu-id="ee357-237">**Azure szolgáltatásfelügyelet metódus:** megkezdése egy runbook hello SDK egy programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="ee357-237">**Azure Service Management method:** You can start a runbook by using hello SDK of a programming language.</span></span> <span data-ttu-id="ee357-238">Alatt van egy C# kódrészletet a runbook indítása az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="ee357-238">Below is a C# code snippet for starting a runbook in your Automation account.</span></span> <span data-ttu-id="ee357-239">Megtekintheti az összes hello kódot a [GitHub-tárházban](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span><span class="sxs-lookup"><span data-stu-id="ee357-239">You can view all hello code at our [GitHub repository](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).</span></span>
  
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
  
  <span data-ttu-id="ee357-240">toostart ezzel a módszerrel hozzon létre egy szótár toostore hello runbook-paraméterek, **VMName** és **resourceGroupName**, és azok értékeit.</span><span class="sxs-lookup"><span data-stu-id="ee357-240">toostart this method, create a dictionary toostore hello runbook parameters, **VMName** and  **resourceGroupName**, and their values.</span></span> <span data-ttu-id="ee357-241">Majd indítsa el a hello runbook.</span><span class="sxs-lookup"><span data-stu-id="ee357-241">Then start hello runbook.</span></span> <span data-ttu-id="ee357-242">Az alábbiakban van hello C# kódrészletet, amely a fent definiált hello metódus hívásakor.</span><span class="sxs-lookup"><span data-stu-id="ee357-242">Below is hello C# code snippet for calling hello method that's defined above.</span></span>
  
  ```
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters toohello dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call hello StartRunbook method with parameters
  StartRunbook(“Get-AzureVMGraphical”, RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-hello-rest-api-and-assign-parameters"></a><span data-ttu-id="ee357-243">Forgatókönyv indítása hello REST API használatával, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-243">Start a runbook by using hello REST API and assign parameters</span></span>
<span data-ttu-id="ee357-244">A runbook-feladatok létrehozhatók és hello Azure Automation REST API használatába hello segítségével **PUT** metódus a következő hello kérelmi URI.</span><span class="sxs-lookup"><span data-stu-id="ee357-244">A runbook job can be created and started with hello Azure Automation REST API by using hello **PUT** method with hello following request URI.</span></span>

    https://management.core.windows.net/<subscription-id>/cloudServices/<cloud-service-name>/resources/automation/~/automationAccounts/<automation-account-name>/jobs/<job-id>?api-version=2014-12-08`

<span data-ttu-id="ee357-245">Hello kérelem URI-azonosítója cserélje le a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="ee357-245">In hello request URI, replace hello following parameters:</span></span>

* <span data-ttu-id="ee357-246">**előfizetés-azonosító:** az Azure-előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="ee357-246">**subscription-id:** Your Azure subscription ID.</span></span>  
* <span data-ttu-id="ee357-247">**felhő szolgáltatásnév:** hello felhő toowhich hello szolgáltatáskérés hello nevét kell küldeni.</span><span class="sxs-lookup"><span data-stu-id="ee357-247">**cloud-service-name:** hello name of hello cloud service toowhich hello request should be sent.</span></span>  
* <span data-ttu-id="ee357-248">**Automation-fiók-name:** megadott felhőszolgáltatás az automation-fiók, amely hello gazdája hello neve.</span><span class="sxs-lookup"><span data-stu-id="ee357-248">**automation-account-name:** hello name of your automation account that's hosted within hello specified cloud service.</span></span>  
* <span data-ttu-id="ee357-249">**azonosítójú feladat:** GUID hello hello feladat.</span><span class="sxs-lookup"><span data-stu-id="ee357-249">**job-id:** hello GUID for hello job.</span></span> <span data-ttu-id="ee357-250">A PowerShell GUID hello segítségével hozhatók létre **[GUID]::NewGuid(). ToString()** parancsot.</span><span class="sxs-lookup"><span data-stu-id="ee357-250">GUIDs in PowerShell can be created by using hello **[GUID]::NewGuid().ToString()** command.</span></span>

<span data-ttu-id="ee357-251">A sorrend toopass paraméterek toohello runbook-feladat használja a hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="ee357-251">In order toopass parameters toohello runbook job, use hello request body.</span></span> <span data-ttu-id="ee357-252">A következő két tulajdonságait JSON formátumban megadott hello tart:</span><span class="sxs-lookup"><span data-stu-id="ee357-252">It takes hello following two properties provided in JSON format:</span></span>

* <span data-ttu-id="ee357-253">**Runbook neve:** szükséges.</span><span class="sxs-lookup"><span data-stu-id="ee357-253">**Runbook name:** Required.</span></span> <span data-ttu-id="ee357-254">hello neve hello runbook hello feladat toostart.</span><span class="sxs-lookup"><span data-stu-id="ee357-254">hello name of hello runbook for hello job toostart.</span></span>  
* <span data-ttu-id="ee357-255">**Runbook-paraméterek:** nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="ee357-255">**Runbook parameters:** Optional.</span></span> <span data-ttu-id="ee357-256">Hello paraméterlista (név, érték) álló dictionary formátumban, ahol neve karakterlánc típusúnak kell lennie, és értéke lehet bármely érvényes JSON-érték.</span><span class="sxs-lookup"><span data-stu-id="ee357-256">A dictionary of hello parameter list in (name, value) format where name should be of String type and value can be any valid JSON value.</span></span>

<span data-ttu-id="ee357-257">Ha azt szeretné, hogy toostart hello **Get-AzureVMTextual** hoztak létre korábbi runbook **VMName** és **resourceGroupName** paraméterként, használja a következő JSON formátummal hello a hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="ee357-257">If you want toostart hello **Get-AzureVMTextual** runbook that was created earlier with **VMName** and **resourceGroupName** as parameters, use hello following JSON format for hello request body.</span></span>

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

<span data-ttu-id="ee357-258">A 201-es HTTP-állapotkód eredményül, ha a hello feladat sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="ee357-258">A HTTP status code 201 is returned if hello job is successfully created.</span></span> <span data-ttu-id="ee357-259">A válaszfejlécek és hello adott válasz törzsének olvashat bővebben arról, hogyan toohello cikk túl[egy runbook-feladat létrehozása hello REST API használatával.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span><span class="sxs-lookup"><span data-stu-id="ee357-259">For more information on response headers and hello response body, refer toohello article about how too[create a runbook job by using hello REST API.](https://msdn.microsoft.com/library/azure/mt163849.aspx)</span></span>

### <a name="test-a-runbook-and-assign-parameters"></a><span data-ttu-id="ee357-260">Egy runbook tesztelése, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-260">Test a runbook and assign parameters</span></span>
<span data-ttu-id="ee357-261">Ha Ön [teszt hello a runbook vázlatverzióját](automation-testing-runbook.md) hello vizsgálati beállítással hello **tesztelése** panel nyílik meg, és konfigurálhatja az imént létrehozott hello paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="ee357-261">When you [test hello draft version of your runbook](automation-testing-runbook.md) by using hello test option, hello **Test** blade opens and you can configure values for hello parameters that you just created.</span></span>

![Tesztelje, és rendelje hozzá a paraméterek](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-tooa-runbook-and-assign-parameters"></a><span data-ttu-id="ee357-263">Egy ütemezés tooa runbook hivatkozásra, és rendelje hozzá a paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-263">Link a schedule tooa runbook and assign parameters</span></span>
<span data-ttu-id="ee357-264">Is [összekapcsolhat egy ütemezést](automation-schedules.md) tooyour runbook úgy, hogy hello runbook egy adott időpontban kezdődik.</span><span class="sxs-lookup"><span data-stu-id="ee357-264">You can [link a schedule](automation-schedules.md) tooyour runbook so that hello runbook starts at a specific time.</span></span> <span data-ttu-id="ee357-265">Hello ütemezés létrehozása, és hello runbook ezeket az értékeket fogja használni, akkor hello ütemezés szerint rendelhet bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ee357-265">You assign input parameters when you create hello schedule, and hello runbook will use these values when it is started by hello schedule.</span></span> <span data-ttu-id="ee357-266">Hello ütemezés nem menthető, amíg az összes kötelező paraméter az értékek.</span><span class="sxs-lookup"><span data-stu-id="ee357-266">You can’t save hello schedule until all mandatory parameter values are provided.</span></span>

![Az ütemezés és a hozzárendelés paraméterek](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a><span data-ttu-id="ee357-268">A webhook egy runbook létrehozása és hozzárendelése paraméterek</span><span class="sxs-lookup"><span data-stu-id="ee357-268">Create a webhook for a runbook and assign parameters</span></span>
<span data-ttu-id="ee357-269">Létrehozhat egy [webhook](automation-webhooks.md) a runbook és konfigurálhatja a runbook bemeneti paramétereket.</span><span class="sxs-lookup"><span data-stu-id="ee357-269">You can create a [webhook](automation-webhooks.md) for your runbook and configure runbook input parameters.</span></span> <span data-ttu-id="ee357-270">Hello webhook nem menthetők, amíg az összes kötelező paraméter az értékek.</span><span class="sxs-lookup"><span data-stu-id="ee357-270">You can’t save hello webhook until all mandatory parameter values are provided.</span></span>

![Webhook létrehozása és hozzárendelése paraméterek](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

<span data-ttu-id="ee357-272">Amikor egy runbook egy webhook használatával hajtható végre, hello előre meghatározott bemeneti paraméter  **[Webhookdata](automation-webhooks.md#details-of-a-webhook)**  , együtt küldött hello meghatározott bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="ee357-272">When you execute a runbook by using a webhook, hello predefined input parameter **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** is sent, along with hello input parameters that you defined.</span></span> <span data-ttu-id="ee357-273">Kattinthat a tooexpand hello **WebhookData** paraméter további részleteket.</span><span class="sxs-lookup"><span data-stu-id="ee357-273">You can click tooexpand hello **WebhookData** parameter for more details.</span></span>

![WebhookData paraméter](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="next-steps"></a><span data-ttu-id="ee357-275">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee357-275">Next steps</span></span>
* <span data-ttu-id="ee357-276">A forgatókönyv bemeneti és kimeneti további információkért lásd: [Azure Automation: runbook bemeneti, kimeneti és egymásba ágyazott runbookok](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span><span class="sxs-lookup"><span data-stu-id="ee357-276">For more information on runbook input and output, see [Azure Automation: runbook input, output, and nested runbooks](https://azure.microsoft.com/blog/azure-automation-runbook-input-output-and-nested-runbooks/).</span></span>
* <span data-ttu-id="ee357-277">Különböző módokon toostart egy runbookot, lásd: [runbook indítása](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="ee357-277">For details about different ways toostart a runbook, see [Starting a runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="ee357-278">tooedit szöveges forgatókönyvként, tekintse meg a túl[szöveges runbookok szerkesztéséhez](automation-edit-textual-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="ee357-278">tooedit a textual runbook, refer too[Editing textual runbooks](automation-edit-textual-runbook.md).</span></span>
* <span data-ttu-id="ee357-279">egy grafikus forgatókönyvnek tooedit tekintse meg a túl[grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="ee357-279">tooedit a graphical runbook, refer too[Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>

