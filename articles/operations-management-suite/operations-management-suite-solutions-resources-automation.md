---
title: "aaaAzure Automation erőforrások az OMS-megoldások |} Microsoft Docs"
description: "Az OMS megoldások rendszerint az Azure Automation tooautomate folyamatoknak, mint például a összegyűjtése és figyelési adatok feldolgozása runbookok tartalmazza.  Ez a cikk ismerteti, hogyan tooinclude runbookok és a kapcsolódó erőforrások megoldásban."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a><span data-ttu-id="d33a6-104">Azure Automation erőforrások tooan OMS felügyeleti megoldás (előzetes verzió) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d33a6-104">Adding Azure Automation resources tooan OMS management solution (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="d33a6-105">Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="d33a6-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="d33a6-106">Az alábbiakban semmilyen sémát tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="d33a6-106">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="d33a6-107">[Az OMS megoldások](operations-management-suite-solutions.md) runbookok rendszerint tartalmazza az Azure Automation tooautomate folyamatoknak, mint például a összegyűjtése és figyelési adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d33a6-107">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include runbooks in Azure Automation tooautomate processes such as collecting and processing monitoring data.</span></span>  <span data-ttu-id="d33a6-108">Ezenkívül toorunbooks, Automation-fiók tartalmaz erőforrásokat, például a változók és ütemezéseket, amely támogatja a használt hello runbookokat hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="d33a6-108">In addition toorunbooks, Automation accounts includes assets such as variables and schedules that support hello runbooks used in hello solution.</span></span>  <span data-ttu-id="d33a6-109">Ez a cikk ismerteti, hogyan tooinclude runbookok és a kapcsolódó erőforrások megoldásban.</span><span class="sxs-lookup"><span data-stu-id="d33a6-109">This article describes how tooinclude runbooks and their related resources in a solution.</span></span>

> [!NOTE]
> <span data-ttu-id="d33a6-110">hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="d33a6-110">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="d33a6-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d33a6-111">Prerequisites</span></span>
<span data-ttu-id="d33a6-112">Ez a cikk feltételezi, hogy már ismeri a következő információ hello.</span><span class="sxs-lookup"><span data-stu-id="d33a6-112">This article assumes that you're already familiar with hello following information.</span></span>

- <span data-ttu-id="d33a6-113">Hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="d33a6-113">How too[create a management solution](operations-management-suite-solutions-creating.md).</span></span>
- <span data-ttu-id="d33a6-114">hello szerkezete egy [megoldásfájlt](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="d33a6-114">hello structure of a [solution file](operations-management-suite-solutions-solution-file.md).</span></span>
- <span data-ttu-id="d33a6-115">Hogyan túl[Resource Manager-sablonok készítésének](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="d33a6-115">How too[author Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>

## <a name="automation-account"></a><span data-ttu-id="d33a6-116">Automation-fiók</span><span class="sxs-lookup"><span data-stu-id="d33a6-116">Automation account</span></span>
<span data-ttu-id="d33a6-117">Az Azure Automationben összes erőforrás található egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview).</span><span class="sxs-lookup"><span data-stu-id="d33a6-117">All resources in Azure Automation are contained in an [Automation account](../automation/automation-security-overview.md#automation-account-overview).</span></span>  <span data-ttu-id="d33a6-118">A [OMS munkaterületet, és az Automation-fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation-fiók nem tartalmazza a hello felügyeleti megoldás, de már léteznie kell hello megoldás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="d33a6-118">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation account isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="d33a6-119">Ha nem érhető el, majd hello megoldás telepítése sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d33a6-119">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="d33a6-120">az egyes Automation erőforrások hello neve hello az Automation-fiók nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d33a6-120">hello name of each Automation resource includes hello name of its Automation account.</span></span>  <span data-ttu-id="d33a6-121">Hello hello megoldást ezt **accountName** paraméter a következő példa egy runbook erőforrás hello hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="d33a6-121">This is done in hello solution with hello **accountName** parameter as in hello following example of a runbook resource.</span></span>

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a><span data-ttu-id="d33a6-122">Runbookok</span><span class="sxs-lookup"><span data-stu-id="d33a6-122">Runbooks</span></span>
<span data-ttu-id="d33a6-123">Tartalmaznia kell a runbookokat hello megoldás hello megoldásfájl használja, hogy hello megoldás telepítésekor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="d33a6-123">You should include any runbooks used by hello solution in hello solution file so that they're created when hello solution is installed.</span></span>  <span data-ttu-id="d33a6-124">Így kell közzé tenni hello runbook tooa nyilvános helyre ahol hozzáférhetők bármely felhasználó telepíti a megoldás azonban nem tartalmazhatja a hello hello runbook hello sablon törzsét.</span><span class="sxs-lookup"><span data-stu-id="d33a6-124">You cannot contain hello body of hello runbook in hello template though, so you should publish hello runbook tooa public location where it can be accessed by any user installing your solution.</span></span>

<span data-ttu-id="d33a6-125">[Azure Automation-runbook](../automation/automation-runbook-types.md) erőforrások típusa lehet **Microsoft.Automation/automationAccounts/runbooks** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-125">[Azure Automation runbook](../automation/automation-runbook-types.md) resources have a type of **Microsoft.Automation/automationAccounts/runbooks** and have hello following structure.</span></span> <span data-ttu-id="d33a6-126">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-126">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


<span data-ttu-id="d33a6-127">a következő táblázat hello runbookokat hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-127">hello properties for runbooks are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-128">Property</span></span> | <span data-ttu-id="d33a6-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-129">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-130">a kötelező runbookType</span><span class="sxs-lookup"><span data-stu-id="d33a6-130">runbookType</span></span> |<span data-ttu-id="d33a6-131">Meghatározza a hello runbook hello típusait.</span><span class="sxs-lookup"><span data-stu-id="d33a6-131">Specifies hello types of hello runbook.</span></span> <br><br> <span data-ttu-id="d33a6-132">Parancsfájl - PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="d33a6-132">Script - PowerShell script</span></span> <br><span data-ttu-id="d33a6-133">PowerShell - PowerShell munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="d33a6-133">PowerShell - PowerShell workflow</span></span> <br> <span data-ttu-id="d33a6-134">GraphPowerShell - grafikus PowerShell-parancsfájl forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d33a6-134">GraphPowerShell - Graphical PowerShell script runbook</span></span> <br> <span data-ttu-id="d33a6-135">GraphPowerShellWorkflow - grafikus PowerShell-munkafolyamati forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d33a6-135">GraphPowerShellWorkflow - Graphical PowerShell workflow runbook</span></span> |
| <span data-ttu-id="d33a6-136">logProgress</span><span class="sxs-lookup"><span data-stu-id="d33a6-136">logProgress</span></span> |<span data-ttu-id="d33a6-137">Megadja, hogy [rekordok előrehaladás](../automation/automation-runbook-output-and-messages.md) hello runbook elő kell állítani.</span><span class="sxs-lookup"><span data-stu-id="d33a6-137">Specifies whether [progress records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="d33a6-138">logVerbose</span><span class="sxs-lookup"><span data-stu-id="d33a6-138">logVerbose</span></span> |<span data-ttu-id="d33a6-139">Megadja, hogy [részletes rekordok](../automation/automation-runbook-output-and-messages.md) hello runbook elő kell állítani.</span><span class="sxs-lookup"><span data-stu-id="d33a6-139">Specifies whether [verbose records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="d33a6-140">leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-140">description</span></span> |<span data-ttu-id="d33a6-141">Hello runbook megadhat egy leírást.</span><span class="sxs-lookup"><span data-stu-id="d33a6-141">Optional description for hello runbook.</span></span> |
| <span data-ttu-id="d33a6-142">publishContentLink</span><span class="sxs-lookup"><span data-stu-id="d33a6-142">publishContentLink</span></span> |<span data-ttu-id="d33a6-143">Hello hello runbook tartalmának megadása</span><span class="sxs-lookup"><span data-stu-id="d33a6-143">Specifies hello content of hello runbook.</span></span> <br><br><span data-ttu-id="d33a6-144">URI - hello runbook tartalmának Uri toohello.</span><span class="sxs-lookup"><span data-stu-id="d33a6-144">uri - Uri toohello content of hello runbook.</span></span>  <span data-ttu-id="d33a6-145">Ez lesz a PowerShell és a parancsfájl runbookok .ps1 fájl, és az exportált grafikus forgatókönyvnek fájl, a Graph-forgatókönyvek esetében.</span><span class="sxs-lookup"><span data-stu-id="d33a6-145">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="d33a6-146">verzió - hello forgatókönyv saját nyomon követésére verzióját.</span><span class="sxs-lookup"><span data-stu-id="d33a6-146">version - Version of hello runbook for your own tracking.</span></span> |


## <a name="automation-jobs"></a><span data-ttu-id="d33a6-147">Automatizálási feladatok</span><span class="sxs-lookup"><span data-stu-id="d33a6-147">Automation jobs</span></span>
<span data-ttu-id="d33a6-148">Amikor elindít egy forgatókönyvet az Azure Automationben, létrehoz egy automation-feladat.</span><span class="sxs-lookup"><span data-stu-id="d33a6-148">When you start a runbook in Azure Automation, it creates an automation job.</span></span>  <span data-ttu-id="d33a6-149">Hozzáadhat egy automatizálási feladat erőforrás tooyour megoldás tooautomatically start egy runbook hello felügyeleti megoldás telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d33a6-149">You can add an automation job resource tooyour solution tooautomatically start a runbook when hello management solution is installed.</span></span>  <span data-ttu-id="d33a6-150">Ez a metódus általánosan használt toostart runbookokat hello megoldás kezdeti konfiguráció használt.</span><span class="sxs-lookup"><span data-stu-id="d33a6-150">This method is typically used toostart runbooks that are used for initial configuration of hello solution.</span></span>  <span data-ttu-id="d33a6-151">toostart egy runbook rendszeres időközönként, hozzon létre egy [ütemezés](#schedules) és egy [feladat ütemezése](#job-schedules)</span><span class="sxs-lookup"><span data-stu-id="d33a6-151">toostart a runbook at regular intervals, create a [schedule](#schedules) and a [job schedule](#job-schedules)</span></span>

<span data-ttu-id="d33a6-152">Feladat erőforrások típusa lehet **Microsoft.Automation/automationAccounts/jobs** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-152">Job resources have a type of **Microsoft.Automation/automationAccounts/jobs** and have hello following structure.</span></span>  <span data-ttu-id="d33a6-153">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-153">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

<span data-ttu-id="d33a6-154">az automatizálási feladatok hello tulajdonságok hello a következő táblázat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-154">hello properties for automation jobs are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-155">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-155">Property</span></span> | <span data-ttu-id="d33a6-156">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-156">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-157">a runbook</span><span class="sxs-lookup"><span data-stu-id="d33a6-157">runbook</span></span> |<span data-ttu-id="d33a6-158">Egyetlen entitás hello runbook toostart hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d33a6-158">Single name entity with hello name of hello runbook toostart.</span></span> |
| <span data-ttu-id="d33a6-159">paraméterek</span><span class="sxs-lookup"><span data-stu-id="d33a6-159">parameters</span></span> |<span data-ttu-id="d33a6-160">Az entitás minden egyes hello runbook által igényelt paraméterérték.</span><span class="sxs-lookup"><span data-stu-id="d33a6-160">Entity for each parameter value required by hello runbook.</span></span> |

<span data-ttu-id="d33a6-161">hello feladat hello runbook neve és a paramétert értékek toobe toohello runbook küldött tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d33a6-161">hello job includes hello runbook name and any parameter values toobe sent toohello runbook.</span></span>  <span data-ttu-id="d33a6-162">hello feladatot kell [függő](operations-management-suite-solutions-solution-file.md#resources) hello runbook óta hello runbook indítása feladat hello előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="d33a6-162">hello job should [depend on](operations-management-suite-solutions-solution-file.md#resources) hello runbook that it's starting since hello runbook must be created before hello job.</span></span>  <span data-ttu-id="d33a6-163">Ha több runbook el kell indítani, megadhatja a sorrendjük azzal, hogy egy feladat minden más, fusson első feladat függ.</span><span class="sxs-lookup"><span data-stu-id="d33a6-163">If you have multiple runbooks that should be started you can define their order by having a job depend on any other jobs that should be run first.</span></span>

<span data-ttu-id="d33a6-164">egy feladat erőforrás hello nevének tartalmaznia kell egy GUID, amely általában egy paraméter által hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="d33a6-164">hello name of a job resource must contain a GUID which is typically assigned by a parameter.</span></span>  <span data-ttu-id="d33a6-165">További a GUID-paraméterekkel kapcsolatos [megoldások létrehozása az Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="d33a6-165">You can read more about GUID parameters in [Creating solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span></span>  


## <a name="certificates"></a><span data-ttu-id="d33a6-166">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="d33a6-166">Certificates</span></span>
<span data-ttu-id="d33a6-167">[Azure Automation-tanúsítványok](../automation/automation-certificates.md) típusú **Microsoft.Automation/automationAccounts/certificates** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-167">[Azure Automation certificates](../automation/automation-certificates.md) have a type of **Microsoft.Automation/automationAccounts/certificates** and have hello following structure.</span></span> <span data-ttu-id="d33a6-168">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-168">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



<span data-ttu-id="d33a6-169">a következő táblázat hello tanúsítványok erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-169">hello properties for Certificates resources are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-170">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-170">Property</span></span> | <span data-ttu-id="d33a6-171">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-171">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-172">base64Value</span><span class="sxs-lookup"><span data-stu-id="d33a6-172">base64Value</span></span> |<span data-ttu-id="d33a6-173">Base 64 érték hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d33a6-173">Base 64 value for hello certificate.</span></span> |
| <span data-ttu-id="d33a6-174">ujjlenyomat</span><span class="sxs-lookup"><span data-stu-id="d33a6-174">thumbprint</span></span> |<span data-ttu-id="d33a6-175">Hello tanúsítványának ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="d33a6-175">Thumbprint for hello certificate.</span></span> |



## <a name="credentials"></a><span data-ttu-id="d33a6-176">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="d33a6-176">Credentials</span></span>
<span data-ttu-id="d33a6-177">[Azure Automation hitelesítő adataival](../automation/automation-credentials.md) típusú **Microsoft.Automation/automationAccounts/credentials** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-177">[Azure Automation credentials](../automation/automation-credentials.md) have a type of **Microsoft.Automation/automationAccounts/credentials** and have hello following structure.</span></span>  <span data-ttu-id="d33a6-178">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-178">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

<span data-ttu-id="d33a6-179">a következő táblázat hello Credential erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-179">hello properties for Credential resources are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-180">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-180">Property</span></span> | <span data-ttu-id="d33a6-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-182">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="d33a6-182">userName</span></span> |<span data-ttu-id="d33a6-183">Hello hitelesítő felhasználó neve.</span><span class="sxs-lookup"><span data-stu-id="d33a6-183">User name for hello credential.</span></span> |
| <span data-ttu-id="d33a6-184">jelszó</span><span class="sxs-lookup"><span data-stu-id="d33a6-184">password</span></span> |<span data-ttu-id="d33a6-185">Jelszó az hello tartozó hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="d33a6-185">Password for hello credential.</span></span> |


## <a name="schedules"></a><span data-ttu-id="d33a6-186">Ütemezések</span><span class="sxs-lookup"><span data-stu-id="d33a6-186">Schedules</span></span>
<span data-ttu-id="d33a6-187">[Azure Automation-ütemezések](../automation/automation-schedules.md) típusú **Microsoft.Automation/automationAccounts/schedules** , és a következő struktúra hello hello.</span><span class="sxs-lookup"><span data-stu-id="d33a6-187">[Azure Automation schedules](../automation/automation-schedules.md) have a type of **Microsoft.Automation/automationAccounts/schedules** and have hello hello following structure.</span></span> <span data-ttu-id="d33a6-188">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-188">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

<span data-ttu-id="d33a6-189">a következő táblázat hello ütemezés erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-189">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-190">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-190">Property</span></span> | <span data-ttu-id="d33a6-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-191">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-192">leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-192">description</span></span> |<span data-ttu-id="d33a6-193">Hello ütemezés megadhat egy leírást.</span><span class="sxs-lookup"><span data-stu-id="d33a6-193">Optional description for hello schedule.</span></span> |
| <span data-ttu-id="d33a6-194">startTime</span><span class="sxs-lookup"><span data-stu-id="d33a6-194">startTime</span></span> |<span data-ttu-id="d33a6-195">Adja meg az ütemezés kezdő időpontjának hello DateTime objektumként.</span><span class="sxs-lookup"><span data-stu-id="d33a6-195">Specifies hello start time of a schedule as a DateTime object.</span></span> <span data-ttu-id="d33a6-196">Egy karakterlánc is megadható, átalakított tooa használható, ha érvényes a DateTime típusú érték.</span><span class="sxs-lookup"><span data-stu-id="d33a6-196">A string can be provided if it can be converted tooa valid DateTime.</span></span> |
| <span data-ttu-id="d33a6-197">IsEnabled</span><span class="sxs-lookup"><span data-stu-id="d33a6-197">isEnabled</span></span> |<span data-ttu-id="d33a6-198">Meghatározza, hogy engedélyezve van-e a hello ütemezés.</span><span class="sxs-lookup"><span data-stu-id="d33a6-198">Specifies whether hello schedule is enabled.</span></span> |
| <span data-ttu-id="d33a6-199">interval</span><span class="sxs-lookup"><span data-stu-id="d33a6-199">interval</span></span> |<span data-ttu-id="d33a6-200">hello típusa hello ütemezés intervallumát.</span><span class="sxs-lookup"><span data-stu-id="d33a6-200">hello type of interval for hello schedule.</span></span><br><br><span data-ttu-id="d33a6-201">nap</span><span class="sxs-lookup"><span data-stu-id="d33a6-201">day</span></span><br><span data-ttu-id="d33a6-202">óra</span><span class="sxs-lookup"><span data-stu-id="d33a6-202">hour</span></span> |
| <span data-ttu-id="d33a6-203">frequency</span><span class="sxs-lookup"><span data-stu-id="d33a6-203">frequency</span></span> |<span data-ttu-id="d33a6-204">Ütemezés hello gyakorisága a napok vagy órák száma kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="d33a6-204">Frequency that hello schedule should fire in number of days or hours.</span></span> |

<span data-ttu-id="d33a6-205">A kezdő időpont hello nagyobb értéket az aktuális idő ütemezések.</span><span class="sxs-lookup"><span data-stu-id="d33a6-205">Schedules must have a start time with a value greater than hello current time.</span></span>  <span data-ttu-id="d33a6-206">Nem adja meg ezt az értéket egy változóhoz, mert semmilyen módon nem tudhatja, hogy telepítve, amelyet toobe kellene lennie.</span><span class="sxs-lookup"><span data-stu-id="d33a6-206">You cannot provide this value with a variable since you would have no way of knowing when it's going toobe installed.</span></span>

<span data-ttu-id="d33a6-207">Hello ütemezés erőforrások a megoldás használata esetén a következő két stratégiák egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="d33a6-207">Use one of hello following two strategies when using schedule resources in a solution.</span></span>

- <span data-ttu-id="d33a6-208">A paraméter használható hello hello ütemezés kezdési idejét.</span><span class="sxs-lookup"><span data-stu-id="d33a6-208">Use a parameter for hello start time of hello schedule.</span></span>  <span data-ttu-id="d33a6-209">Hello megoldás telepítésekor ez fogja kérni hello felhasználói tooprovide értéket.</span><span class="sxs-lookup"><span data-stu-id="d33a6-209">This will prompt hello user tooprovide a value when they install hello solution.</span></span>  <span data-ttu-id="d33a6-210">Ha egyszerre több ütemezés, használhat egy egyetlen paraméterérték egynél több.</span><span class="sxs-lookup"><span data-stu-id="d33a6-210">If you have multiple schedules, you could use a single parameter value for more than one of them.</span></span>
- <span data-ttu-id="d33a6-211">Hello ütemezések használatával, amely akkor kezdődik, amikor hello megoldás van telepítve runbook létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d33a6-211">Create hello schedules using a runbook that starts when hello solution is installed.</span></span>  <span data-ttu-id="d33a6-212">Ezzel megszünteti a hello követelményt, de nem tartalmazhat hello felhasználói toospecify hello ütemezése a megoldásban, ezért el lesz távolítva, hello megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d33a6-212">This removes hello requirement of hello user toospecify a time, but you can't contain hello schedule in your solution so it will be removed when hello solution is removed.</span></span>


### <a name="job-schedules"></a><span data-ttu-id="d33a6-213">Feladatütemezések</span><span class="sxs-lookup"><span data-stu-id="d33a6-213">Job schedules</span></span>
<span data-ttu-id="d33a6-214">Feladat ütemezése erőforrások ütemezés runbookokhoz hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d33a6-214">Job schedule resources link a runbook with a schedule.</span></span>  <span data-ttu-id="d33a6-215">Olyan típusú rendelkeznek **Microsoft.Automation/automationAccounts/jobSchedules** és a következő struktúra hello hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-215">They have a type of **Microsoft.Automation/automationAccounts/jobSchedules** and have hello hello following structure.</span></span>  <span data-ttu-id="d33a6-216">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-216">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


<span data-ttu-id="d33a6-217">hello a következő táblázat ismerteti a feladatok ütemezésének hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="d33a6-217">hello properties for job schedules are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-218">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-218">Property</span></span> | <span data-ttu-id="d33a6-219">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-219">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-220">az ütemezés nevének</span><span class="sxs-lookup"><span data-stu-id="d33a6-220">schedule name</span></span> |<span data-ttu-id="d33a6-221">Egyetlen **neve** hello ütemterv hello nevű entitás.</span><span class="sxs-lookup"><span data-stu-id="d33a6-221">Single **name** entity with hello name of hello schedule.</span></span> |
| <span data-ttu-id="d33a6-222">Runbook neve</span><span class="sxs-lookup"><span data-stu-id="d33a6-222">runbook name</span></span>  |<span data-ttu-id="d33a6-223">Egyetlen **neve** hello runbook hello nevű entitás.</span><span class="sxs-lookup"><span data-stu-id="d33a6-223">Single **name** entity with hello name of hello runbook.</span></span>  |



## <a name="variables"></a><span data-ttu-id="d33a6-224">Változók</span><span class="sxs-lookup"><span data-stu-id="d33a6-224">Variables</span></span>
<span data-ttu-id="d33a6-225">[Azure Automation-változók](../automation/automation-variables.md) típusú **Microsoft.Automation/automationAccounts/variables** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-225">[Azure Automation variables](../automation/automation-variables.md) have a type of **Microsoft.Automation/automationAccounts/variables** and have hello following structure.</span></span>  <span data-ttu-id="d33a6-226">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-226">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

<span data-ttu-id="d33a6-227">a következő táblázat hello változó erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-227">hello properties for variable resources are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-228">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-228">Property</span></span> | <span data-ttu-id="d33a6-229">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-229">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-230">leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-230">description</span></span> | <span data-ttu-id="d33a6-231">Hello változó megadhat egy leírást.</span><span class="sxs-lookup"><span data-stu-id="d33a6-231">Optional description for hello variable.</span></span> |
| <span data-ttu-id="d33a6-232">isEncrypted</span><span class="sxs-lookup"><span data-stu-id="d33a6-232">isEncrypted</span></span> | <span data-ttu-id="d33a6-233">Megadja a hello változó titkosítani kell-e.</span><span class="sxs-lookup"><span data-stu-id="d33a6-233">Specifies whether hello variable should be encrypted.</span></span> |
| <span data-ttu-id="d33a6-234">type</span><span class="sxs-lookup"><span data-stu-id="d33a6-234">type</span></span> | <span data-ttu-id="d33a6-235">Ezt a tulajdonságot jelenleg nincs hatása.</span><span class="sxs-lookup"><span data-stu-id="d33a6-235">This property currently has no effect.</span></span>  <span data-ttu-id="d33a6-236">hello adattípus hello változó hello kezdeti érték határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d33a6-236">hello data type of hello variable will be determined by hello initial value.</span></span> |
| <span data-ttu-id="d33a6-237">érték</span><span class="sxs-lookup"><span data-stu-id="d33a6-237">value</span></span> | <span data-ttu-id="d33a6-238">Hello változó értékét.</span><span class="sxs-lookup"><span data-stu-id="d33a6-238">Value for hello variable.</span></span> |

> [!NOTE]
> <span data-ttu-id="d33a6-239">Hello **típus** tulajdonság nincs hatással a hello változó létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="d33a6-239">hello **type** property currently has no effect on hello variable being created.</span></span>  <span data-ttu-id="d33a6-240">hello változó adattípust hello hello érték határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d33a6-240">hello data type for hello variable will be determined by hello value.</span></span>  

<span data-ttu-id="d33a6-241">Ha hello kezdeti hello változó értékét, akkor be kell állítani hello megfelelő adattípusú értékként.</span><span class="sxs-lookup"><span data-stu-id="d33a6-241">If you set hello initial value for hello variable, it must be configured as hello correct data type.</span></span>  <span data-ttu-id="d33a6-242">a következő táblázat hello hello különböző adattípusokkal engedélyezett és a szintaxis biztosít.</span><span class="sxs-lookup"><span data-stu-id="d33a6-242">hello following table provides hello different data types allowable and their syntax.</span></span>  <span data-ttu-id="d33a6-243">Vegye figyelembe, hogy a JSON-ban értékei várt tooalways hello ajánlatok belül a különleges karaktereket az idézőjelek.</span><span class="sxs-lookup"><span data-stu-id="d33a6-243">Note that values in JSON are expected tooalways be enclosed in quotes with any special characters within hello quotes.</span></span>  <span data-ttu-id="d33a6-244">Például egy olyan karakterláncértéket szeretné megadni hello karakterlánc idézőjelbe (hello escape-karakter használatával (\\)) egy numerikus érték lenne megadva, az idézőjelek közé foglalt egy készletével.</span><span class="sxs-lookup"><span data-stu-id="d33a6-244">For example, a string value would be specified by quotes around hello string (using hello escape character (\\)) while a numeric value would be specified with one set of quotes.</span></span>

| <span data-ttu-id="d33a6-245">Adattípus</span><span class="sxs-lookup"><span data-stu-id="d33a6-245">Data type</span></span> | <span data-ttu-id="d33a6-246">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-246">Description</span></span> | <span data-ttu-id="d33a6-247">Példa</span><span class="sxs-lookup"><span data-stu-id="d33a6-247">Example</span></span> | <span data-ttu-id="d33a6-248">Oldja fel a túl</span><span class="sxs-lookup"><span data-stu-id="d33a6-248">Resolves too</span></span>|
|:--|:--|:--|:--|
| <span data-ttu-id="d33a6-249">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d33a6-249">string</span></span>   | <span data-ttu-id="d33a6-250">Érték tegye idézőjelek közé foglalt.</span><span class="sxs-lookup"><span data-stu-id="d33a6-250">Enclose value in double quotes.</span></span>  | <span data-ttu-id="d33a6-251">"\"Hello world\""</span><span class="sxs-lookup"><span data-stu-id="d33a6-251">"\"Hello world\""</span></span> | <span data-ttu-id="d33a6-252">"Hello world"</span><span class="sxs-lookup"><span data-stu-id="d33a6-252">"Hello world"</span></span> |
| <span data-ttu-id="d33a6-253">Numerikus</span><span class="sxs-lookup"><span data-stu-id="d33a6-253">numeric</span></span>  | <span data-ttu-id="d33a6-254">Szimpla idézőjelben numerikus értéket.</span><span class="sxs-lookup"><span data-stu-id="d33a6-254">Numeric value with single quotes.</span></span>| <span data-ttu-id="d33a6-255">"64"</span><span class="sxs-lookup"><span data-stu-id="d33a6-255">"64"</span></span> | <span data-ttu-id="d33a6-256">64</span><span class="sxs-lookup"><span data-stu-id="d33a6-256">64</span></span> |
| <span data-ttu-id="d33a6-257">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="d33a6-257">boolean</span></span>  | <span data-ttu-id="d33a6-258">**Igaz** vagy **hamis** idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="d33a6-258">**true** or **false** in quotes.</span></span>  <span data-ttu-id="d33a6-259">Vegye figyelembe, hogy ez az érték kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d33a6-259">Note that this value must be lowercase.</span></span> | <span data-ttu-id="d33a6-260">"true"</span><span class="sxs-lookup"><span data-stu-id="d33a6-260">"true"</span></span> | <span data-ttu-id="d33a6-261">Igaz</span><span class="sxs-lookup"><span data-stu-id="d33a6-261">true</span></span> |
| <span data-ttu-id="d33a6-262">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="d33a6-262">datetime</span></span> | <span data-ttu-id="d33a6-263">A szerializált dátumértéket.</span><span class="sxs-lookup"><span data-stu-id="d33a6-263">Serialized date value.</span></span><br><span data-ttu-id="d33a6-264">Használható hello ConvertTo-Json parancsmag PowerShell toogenerate ezt az értéket egy adott dátumot.</span><span class="sxs-lookup"><span data-stu-id="d33a6-264">You can use hello ConvertTo-Json cmdlet in PowerShell toogenerate this value for a particular date.</span></span><br><span data-ttu-id="d33a6-265">Példa: get-date "5/24/2017 13:14:57" \\</span><span class="sxs-lookup"><span data-stu-id="d33a6-265">Example: get-date "5/24/2017 13:14:57" \\</span></span>| <span data-ttu-id="d33a6-266">ConvertTo-Json</span><span class="sxs-lookup"><span data-stu-id="d33a6-266">ConvertTo-Json</span></span> | <span data-ttu-id="d33a6-267">"\\/Date(1495656897378)\\/"</span><span class="sxs-lookup"><span data-stu-id="d33a6-267">"\\/Date(1495656897378)\\/"</span></span> | <span data-ttu-id="d33a6-268">2017-05-24 13:14:57</span><span class="sxs-lookup"><span data-stu-id="d33a6-268">2017-05-24 13:14:57</span></span> |

## <a name="modules"></a><span data-ttu-id="d33a6-269">Modulok</span><span class="sxs-lookup"><span data-stu-id="d33a6-269">Modules</span></span>
<span data-ttu-id="d33a6-270">A felügyeleti megoldás nem kell toodefine [globális modulok](../automation/automation-integration-modules.md) a runbookok által használt, mert azok mindig elérhető lesz az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="d33a6-270">Your management solution does not need toodefine [global modules](../automation/automation-integration-modules.md) used by your runbooks because they will always be available in your Automation account.</span></span>  <span data-ttu-id="d33a6-271">Bármely más, a runbookok által használt modul tooinclude erőforrás szükséges.</span><span class="sxs-lookup"><span data-stu-id="d33a6-271">You do need tooinclude a resource for any other module used by your runbooks.</span></span>

<span data-ttu-id="d33a6-272">[Integrációs modulok](../automation/automation-integration-modules.md) típusú **Microsoft.Automation/automationAccounts/modules** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d33a6-272">[Integration modules](../automation/automation-integration-modules.md) have a type of **Microsoft.Automation/automationAccounts/modules** and have hello following structure.</span></span>  <span data-ttu-id="d33a6-273">Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d33a6-273">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


<span data-ttu-id="d33a6-274">a következő táblázat hello modul erőforrás hello tulajdonságait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d33a6-274">hello properties for module resources are described in hello following table.</span></span>

| <span data-ttu-id="d33a6-275">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d33a6-275">Property</span></span> | <span data-ttu-id="d33a6-276">Leírás</span><span class="sxs-lookup"><span data-stu-id="d33a6-276">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d33a6-277">contentLink</span><span class="sxs-lookup"><span data-stu-id="d33a6-277">contentLink</span></span> |<span data-ttu-id="d33a6-278">Megadja a hello modul hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d33a6-278">Specifies hello content of hello module.</span></span> <br><br><span data-ttu-id="d33a6-279">URI - hello modul tartalmának Uri toohello.</span><span class="sxs-lookup"><span data-stu-id="d33a6-279">uri - Uri toohello content of hello module.</span></span>  <span data-ttu-id="d33a6-280">Ez lesz a PowerShell és a parancsfájl runbookok .ps1 fájl, és az exportált grafikus forgatókönyvnek fájl, a Graph-forgatókönyvek esetében.</span><span class="sxs-lookup"><span data-stu-id="d33a6-280">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="d33a6-281">verzió - hello modul a saját követési verzióját.</span><span class="sxs-lookup"><span data-stu-id="d33a6-281">version - Version of hello module for your own tracking.</span></span> |

<span data-ttu-id="d33a6-282">hello runbook hello modul erőforrás tooensure előtt hello runbook létrehozva függ.</span><span class="sxs-lookup"><span data-stu-id="d33a6-282">hello runbook should depend on hello module resource tooensure that it's created before hello runbook.</span></span>

### <a name="updating-modules"></a><span data-ttu-id="d33a6-283">Modulok frissítése</span><span class="sxs-lookup"><span data-stu-id="d33a6-283">Updating modules</span></span>
<span data-ttu-id="d33a6-284">Ha egy felügyeleti megoldás, amely tartalmazza a runbook által használt ütemezés szerint frissíti, és a megoldás hello új verziója, hogy a runbook által használt új modul, hello runbook hello modul régi verziója hello használhat.</span><span class="sxs-lookup"><span data-stu-id="d33a6-284">If you update a management solution that includes a runbook that uses a schedule, and hello new version of your solution has a new module used by that runbook, then hello runbook may use hello old version of hello module.</span></span>  <span data-ttu-id="d33a6-285">A megoldás forgatókönyve a következő hello tartalmazza, és hozzon létre egy feladat toorun őket, mielőtt más runbookokat.</span><span class="sxs-lookup"><span data-stu-id="d33a6-285">You should include hello following runbooks in your solution and create a job toorun them before any other runbooks.</span></span>  <span data-ttu-id="d33a6-286">Ezzel biztosíthatja, hogy a modul előtt szükséges hello runbookok be van töltve, frissülnek.</span><span class="sxs-lookup"><span data-stu-id="d33a6-286">This will ensure that any modules are updated as required before hello runbooks are loaded.</span></span>

* <span data-ttu-id="d33a6-287">[Frissítés-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) biztosítja, hogy a megoldás a runbookok által használt hello modulok összes hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="d33a6-287">[Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) will ensure that all of hello modules used by runbooks in your solution are hello latest version.</span></span>  
* <span data-ttu-id="d33a6-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) újraregisztrálása összes hello ütemezés erőforrások tooensure, hogy a runbookokat hello használata hello legújabb modulok toothem társítva.</span><span class="sxs-lookup"><span data-stu-id="d33a6-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) will reregister all of hello schedule resources tooensure that hello runbooks linked toothem with use hello latest modules.</span></span>




## <a name="sample"></a><span data-ttu-id="d33a6-289">Minta</span><span class="sxs-lookup"><span data-stu-id="d33a6-289">Sample</span></span>
<span data-ttu-id="d33a6-290">Az alábbiakban látható egy minta a megoldás, amely tartalmazza, amely tartalmazza a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="d33a6-290">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="d33a6-291">Runbook.</span><span class="sxs-lookup"><span data-stu-id="d33a6-291">Runbook.</span></span>  <span data-ttu-id="d33a6-292">Ez a példa runbook egy nyilvános GitHub-tárházban tárolt.</span><span class="sxs-lookup"><span data-stu-id="d33a6-292">This is a sample runbook stored in a public GitHub repository.</span></span>
- <span data-ttu-id="d33a6-293">Automation-feladat, amely hello runbook kezdődik, amikor hello megoldás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="d33a6-293">Automation job that starts hello runbook when hello solution is installed.</span></span>
- <span data-ttu-id="d33a6-294">Az ütemezés és a feladat ütemezése toostart hello runbook rendszeres időközönként.</span><span class="sxs-lookup"><span data-stu-id="d33a6-294">Schedule and job schedule toostart hello runbook at regular intervals.</span></span>
- <span data-ttu-id="d33a6-295">Tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d33a6-295">Certificate.</span></span>
- <span data-ttu-id="d33a6-296">Hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="d33a6-296">Credential.</span></span>
- <span data-ttu-id="d33a6-297">Változó.</span><span class="sxs-lookup"><span data-stu-id="d33a6-297">Variable.</span></span>
- <span data-ttu-id="d33a6-298">A modul.</span><span class="sxs-lookup"><span data-stu-id="d33a6-298">Module.</span></span>  <span data-ttu-id="d33a6-299">Ez a hello [OMSIngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) írás Analytics adatok tooLog.</span><span class="sxs-lookup"><span data-stu-id="d33a6-299">This is hello [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) for writing data tooLog Analytics.</span></span> 

<span data-ttu-id="d33a6-300">minta használ hello [szokásos megoldást paraméterek](operations-management-suite-solutions-solution-file.md#parameters) változókat, amelyek a megoldás, mint a gyakran használni ellenezte toohardcoding értékek hello erőforrás-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="d33a6-300">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a><span data-ttu-id="d33a6-301">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d33a6-301">Next steps</span></span>
* <span data-ttu-id="d33a6-302">[Nézet tooyour megoldás hozzáadása](operations-management-suite-solutions-resources-views.md) toovisualize összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="d33a6-302">[Add a view tooyour solution](operations-management-suite-solutions-resources-views.md) toovisualize collected data.</span></span>
