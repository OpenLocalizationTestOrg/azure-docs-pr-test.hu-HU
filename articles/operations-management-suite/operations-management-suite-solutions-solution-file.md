---
title: "Létrehozása kezelési megoldásai Operations Management Suite (OMS) |} Microsoft Docs"
description: "Megoldások bővítése Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, amelyek az ügyfelek az OMS-munkaterület adhat hozzá.  Ez a cikk részletesen hogyan hozhat létre a saját környezetében használható felügyeleti megoldás, vagy szeretné elérhetővé tenni az ügyfelek számára."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee3462c13101d18921dc488b08c79e1e4e02ff3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="d2d81-104">A felügyeleti megoldás fájl létrehozása az Operations Management Suite (OMS) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="d2d81-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="d2d81-105">Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="d2d81-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="d2d81-106">Az alábbiakban a séma van változhat.</span><span class="sxs-lookup"><span data-stu-id="d2d81-106">Any schema described below is subject to change.</span></span>  

<span data-ttu-id="d2d81-107">Az Operations Management Suite (OMS) megoldások használják, mint [Resource Manager-sablonok](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d2d81-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="d2d81-108">A fő feladat hozhatnak létre megoldások hogyan van tanulási hogyan [sablon szerzői](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d2d81-108">The main task in learning how to author management solutions is learning how to [author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="d2d81-109">Ez a cikk ismerteti egyedi megoldások és a szokásos megoldás erőforrások konfigurálása használt sablonokat.</span><span class="sxs-lookup"><span data-stu-id="d2d81-109">This article provides unique details of templates used for solutions and how to configure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="d2d81-110">Eszközök</span><span class="sxs-lookup"><span data-stu-id="d2d81-110">Tools</span></span>

<span data-ttu-id="d2d81-111">Megoldás fájlok szövegszerkesztőben használhatja, de javasoljuk, hogy a következő cikkekben ismertetett módon a Visual Studio vagy Visual Studio Code nyújtott szolgáltatásokat kihasználva.</span><span class="sxs-lookup"><span data-stu-id="d2d81-111">You can use any text editor to work with solution files, but we recommend leveraging the features provided in Visual Studio or Visual Studio Code as described in the following articles.</span></span>

- [<span data-ttu-id="d2d81-112">Létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok</span><span class="sxs-lookup"><span data-stu-id="d2d81-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="d2d81-113">A Visual Studio Code Azure Resource Manager-sablonok használata</span><span class="sxs-lookup"><span data-stu-id="d2d81-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="d2d81-114">struktúra</span><span class="sxs-lookup"><span data-stu-id="d2d81-114">Structure</span></span>
<span data-ttu-id="d2d81-115">A felügyeleti megoldás fájl alapvető szerkezete megegyezik egy [Resource Manager-sablon](../azure-resource-manager/resource-group-authoring-templates.md#template-format) Ez az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="d2d81-115">The basic structure of a management solution file is the same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="d2d81-116">A legfelső szintű elemeket ismerteti az alábbi szakaszok mindegyikének és és azok tartalmát, a megoldás.</span><span class="sxs-lookup"><span data-stu-id="d2d81-116">Each of the sections below describes the top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="d2d81-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d2d81-117">Parameters</span></span>
<span data-ttu-id="d2d81-118">[Paraméterek](../azure-resource-manager/resource-group-authoring-templates.md#parameters) értékek, amelyekre szüksége van a felhasználótól a felügyeleti megoldás telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from the user when they install the management solution.</span></span>  <span data-ttu-id="d2d81-119">Standard paramétert, amely minden megoldás fog rendelkezni, és az adott megoldáshoz szükség szerint további paramétereket is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d2d81-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="d2d81-120">Hogyan felhasználók nyújtják a paraméterértékek való telepítésekor a megoldás függ az adott paraméter és a megoldás telepítésének módját.</span><span class="sxs-lookup"><span data-stu-id="d2d81-120">How users will provide parameter values when they install your solution will depend on the particular parameter and how the solution is being installed.</span></span>

<span data-ttu-id="d2d81-121">Amikor a felhasználó telepíti a felügyeleti megoldás keresztül a [Azure piactér](operations-management-suite-solutions.md#finding-and-installing-management-solutions) vagy [Azure gyors üzembe helyezési sablonokat](operations-management-suite-solutions.md#finding-and-installing-management-solutions) megadását kéri az [OMS munkaterületet, és az Automation-fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="d2d81-121">When a user installs your management solution through the [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted to select an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="d2d81-122">Ezek használhatók a szabványos paraméterek értékeit feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="d2d81-122">These are used to populate the values of each of the standard parameters.</span></span>  <span data-ttu-id="d2d81-123">Nem kéri a felhasználót, hogy közvetlenül a szabványos paraméterek értékének megadására, de meg kell minden további paraméterek értékének megadására.</span><span class="sxs-lookup"><span data-stu-id="d2d81-123">The user is not prompted to directly provide values for the standard parameters, but they are prompted to provide values for any additional parameters.</span></span>

<span data-ttu-id="d2d81-124">Amikor a felhasználó telepíti a megoldás [egy másik módszer](operations-management-suite-solutions.md#finding-and-installing-management-solutions), meg kell adniuk egy értéket az összes szabványos és az összes további paraméterei.</span><span class="sxs-lookup"><span data-stu-id="d2d81-124">When the user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="d2d81-125">Az alábbiakban látható egy minta paraméter.</span><span class="sxs-lookup"><span data-stu-id="d2d81-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="d2d81-126">Az alábbi táblázatban a paraméter attribútumait.</span><span class="sxs-lookup"><span data-stu-id="d2d81-126">The following table describes the attributes of a parameter.</span></span>

| <span data-ttu-id="d2d81-127">Attribútum</span><span class="sxs-lookup"><span data-stu-id="d2d81-127">Attribute</span></span> | <span data-ttu-id="d2d81-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2d81-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d2d81-129">type</span><span class="sxs-lookup"><span data-stu-id="d2d81-129">type</span></span> |<span data-ttu-id="d2d81-130">A paraméter adattípusa.</span><span class="sxs-lookup"><span data-stu-id="d2d81-130">Data type for the parameter.</span></span> <span data-ttu-id="d2d81-131">A bemeneti vezérlő jelenik meg a felhasználói adatok típusától függ.</span><span class="sxs-lookup"><span data-stu-id="d2d81-131">The input control displayed for the user depends on the data type.</span></span><br><br><span data-ttu-id="d2d81-132">logikai érték - a legördülő listából</span><span class="sxs-lookup"><span data-stu-id="d2d81-132">bool - Drop down box</span></span><br><span data-ttu-id="d2d81-133">karakterlánc - szövegmező</span><span class="sxs-lookup"><span data-stu-id="d2d81-133">string - Text box</span></span><br><span data-ttu-id="d2d81-134">int - szövegmező</span><span class="sxs-lookup"><span data-stu-id="d2d81-134">int - Text box</span></span><br><span data-ttu-id="d2d81-135">SecureString - jelszó mező</span><span class="sxs-lookup"><span data-stu-id="d2d81-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="d2d81-136">category</span><span class="sxs-lookup"><span data-stu-id="d2d81-136">category</span></span> |<span data-ttu-id="d2d81-137">A paraméter nem kötelező kategóriát.</span><span class="sxs-lookup"><span data-stu-id="d2d81-137">Optional category for the parameter.</span></span>  <span data-ttu-id="d2d81-138">Paraméterek ugyanabba a kategóriába sorolhatók.</span><span class="sxs-lookup"><span data-stu-id="d2d81-138">Parameters in the same category are grouped together.</span></span> |
| <span data-ttu-id="d2d81-139">vezérlő</span><span class="sxs-lookup"><span data-stu-id="d2d81-139">control</span></span> |<span data-ttu-id="d2d81-140">További funkciók karakterlánc-paraméter.</span><span class="sxs-lookup"><span data-stu-id="d2d81-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="d2d81-141">datetime - Datetime vezérlő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d2d81-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="d2d81-142">GUID - Guid-érték automatikusan jön létre, és a paraméter nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d2d81-142">guid - Guid value is automatically generated, and the parameter is not displayed.</span></span> |
| <span data-ttu-id="d2d81-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2d81-143">description</span></span> |<span data-ttu-id="d2d81-144">A paraméter nem kötelező leírása.</span><span class="sxs-lookup"><span data-stu-id="d2d81-144">Optional description for the parameter.</span></span>  <span data-ttu-id="d2d81-145">Megjelenik az adatokat a buborékban megjelenő mellett a paraméter.</span><span class="sxs-lookup"><span data-stu-id="d2d81-145">Displayed in an information balloon next to the parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="d2d81-146">Szabványos paraméterek</span><span class="sxs-lookup"><span data-stu-id="d2d81-146">Standard parameters</span></span>
<span data-ttu-id="d2d81-147">Az alábbi táblázat a minden felügyeleti megoldások szabványos paraméterek.</span><span class="sxs-lookup"><span data-stu-id="d2d81-147">The following table lists the standard parameters for all management solutions.</span></span>  <span data-ttu-id="d2d81-148">Ezek az értékek fel van töltve, a felhasználó helyett adatkérés el a megoldás az Azure piactér vagy gyorsindítási sablonok telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-148">These values are populated for the user instead of prompting for them when your solution is installed from the Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="d2d81-149">Ha a megoldás telepítve van egy másik módszerrel a felhasználó értékeket kell adnia a számukra.</span><span class="sxs-lookup"><span data-stu-id="d2d81-149">The user must provide values for them if the solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="d2d81-150">A felhasználói felület az Azure piactér és gyorsindítási sablonok által várt paraméterekkel a paraméterek nevei a táblában.</span><span class="sxs-lookup"><span data-stu-id="d2d81-150">The user interface in the Azure Marketplace and Quickstart templates is expecting the parameter names in the table.</span></span>  <span data-ttu-id="d2d81-151">Ha különböző paraméternevek használja majd a felhasználót a rendszer kéri a számukra, és azok nem automatikusan tölti fel.</span><span class="sxs-lookup"><span data-stu-id="d2d81-151">If you use different parameter names then the user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="d2d81-152">Paraméter</span><span class="sxs-lookup"><span data-stu-id="d2d81-152">Parameter</span></span> | <span data-ttu-id="d2d81-153">Típus</span><span class="sxs-lookup"><span data-stu-id="d2d81-153">Type</span></span> | <span data-ttu-id="d2d81-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2d81-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="d2d81-155">Fióknév</span><span class="sxs-lookup"><span data-stu-id="d2d81-155">accountName</span></span> |<span data-ttu-id="d2d81-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-156">string</span></span> |<span data-ttu-id="d2d81-157">Azure Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="d2d81-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="d2d81-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="d2d81-158">pricingTier</span></span> |<span data-ttu-id="d2d81-159">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-159">string</span></span> |<span data-ttu-id="d2d81-160">A Naplóelemzési munkaterület- és Azure Automation-fiók tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="d2d81-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="d2d81-161">regionId</span><span class="sxs-lookup"><span data-stu-id="d2d81-161">regionId</span></span> |<span data-ttu-id="d2d81-162">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-162">string</span></span> |<span data-ttu-id="d2d81-163">Az Azure Automation-fiók területet.</span><span class="sxs-lookup"><span data-stu-id="d2d81-163">Region of the Azure Automation account.</span></span> |
| <span data-ttu-id="d2d81-164">Megoldás neve</span><span class="sxs-lookup"><span data-stu-id="d2d81-164">solutionName</span></span> |<span data-ttu-id="d2d81-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-165">string</span></span> |<span data-ttu-id="d2d81-166">A megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="d2d81-166">Name of the solution.</span></span>  <span data-ttu-id="d2d81-167">Ha a megoldás gyorsindítási sablonok keresztül telepíti, majd meg kell határozni megoldás neve paraméterként úgy határozhatja meg kell adnia egy felhasználói helyette igénylő karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d2d81-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring the user to specify one.</span></span> |
| <span data-ttu-id="d2d81-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="d2d81-168">workspaceName</span></span> |<span data-ttu-id="d2d81-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-169">string</span></span> |<span data-ttu-id="d2d81-170">Napló Analytics munkaterület neve.</span><span class="sxs-lookup"><span data-stu-id="d2d81-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="d2d81-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="d2d81-171">workspaceRegionId</span></span> |<span data-ttu-id="d2d81-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="d2d81-172">string</span></span> |<span data-ttu-id="d2d81-173">A Naplóelemzési munkaterület területet.</span><span class="sxs-lookup"><span data-stu-id="d2d81-173">Region of the Log Analytics workspace.</span></span> |


<span data-ttu-id="d2d81-174">Az alábbiakban olvashatja a szabványos paraméterek, másolja és illessze be a megoldásfájlt szerkezete.</span><span class="sxs-lookup"><span data-stu-id="d2d81-174">Following is the structure of the standard parameters that you can copy and paste into your solution file.</span></span>  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="d2d81-175">Tekintse meg a paraméterértékeket a megoldás a szintaxissal más elemei **paraméter ("név" paraméternek)**.</span><span class="sxs-lookup"><span data-stu-id="d2d81-175">You refer to parameter values in other elements of the solution with the syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="d2d81-176">Például a munkaterület neve eléréséhez használja **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="d2d81-176">For example, to access the workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="d2d81-177">Változók</span><span class="sxs-lookup"><span data-stu-id="d2d81-177">Variables</span></span>
<span data-ttu-id="d2d81-178">[Változók](../azure-resource-manager/resource-group-authoring-templates.md#variables) szüksége lesz a megoldásról a további értékek.</span><span class="sxs-lookup"><span data-stu-id="d2d81-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in the rest of the management solution.</span></span>  <span data-ttu-id="d2d81-179">Ezek az értékek nem érhetők el a felhasználó telepíti a megoldást.</span><span class="sxs-lookup"><span data-stu-id="d2d81-179">These values are not exposed to the user installing the solution.</span></span>  <span data-ttu-id="d2d81-180">A szerző biztosítania meg egy helyet, ahol értékeket, amelyeket az lehet, hogy több alkalommal a megoldás teljes kezelésére szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="d2d81-180">They are intended to provide the author with a single location where they can manage values that may be used multiple times throughout the solution.</span></span> <span data-ttu-id="d2d81-181">El kell helyezni minden olyan értéket adott változók figyelésekor rögzített kódolási azokat a megoldáshoz a **erőforrások** elemet.</span><span class="sxs-lookup"><span data-stu-id="d2d81-181">You should put any values specific to your solution in variables as opposed to hard coding them in the **resources** element.</span></span>  <span data-ttu-id="d2d81-182">Ez a kód olvashatóbbá teszi és könnyen módosíthatja ezeket az értékeket az újabb verziókban.</span><span class="sxs-lookup"><span data-stu-id="d2d81-182">This makes the code more readable and allows you to easily change these values in later versions.</span></span>

<span data-ttu-id="d2d81-183">Az alábbiakban látható egy példa egy **változók** elem megoldások használt általános paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="d2d81-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="d2d81-184">Tekintse meg a változó a megoldással a szintaxissal **változók ("változó neve")**.</span><span class="sxs-lookup"><span data-stu-id="d2d81-184">You refer to variable values through the solution with the syntax **variables('variable name')**.</span></span>  <span data-ttu-id="d2d81-185">Például a megoldás neve változó eléréséhez használja **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="d2d81-185">For example, to access the SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="d2d81-186">Azt is megadhatja, komplex változók értékeinek beállítja, hogy több.</span><span class="sxs-lookup"><span data-stu-id="d2d81-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="d2d81-187">Ezek akkor igazán hasznosak-kezelési megoldásokban ahol több különböző típusú erőforrások tulajdonság meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="d2d81-188">Például sikerült átalakítása a megoldás változók a következőhöz fent látható.</span><span class="sxs-lookup"><span data-stu-id="d2d81-188">For example, you could restructure the solution variables shown above to the following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="d2d81-189">Ebben az esetben hivatkozik, a megoldással a szintaxissal változók értékeinek **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="d2d81-189">In this case, you refer to variable values through the solution with the syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="d2d81-190">Például a megoldás neve változó eléréséhez használja **variables('Solution'). Név**.</span><span class="sxs-lookup"><span data-stu-id="d2d81-190">For example, to access the Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="d2d81-191">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="d2d81-191">Resources</span></span>
<span data-ttu-id="d2d81-192">[Erőforrások](../azure-resource-manager/resource-group-authoring-templates.md#resources) határozza meg a különböző erőforrások, amelyek a felügyeleti megoldás telepíteni és konfigurálni fog.</span><span class="sxs-lookup"><span data-stu-id="d2d81-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define the different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="d2d81-193">Ez a sablon, a legnagyobb, és a legösszetettebb része lesz.</span><span class="sxs-lookup"><span data-stu-id="d2d81-193">This will be the largest and most complex portion of the template.</span></span>  <span data-ttu-id="d2d81-194">A struktúra és a teljes leírását az erőforrás-elemek [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="d2d81-194">You can get the structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="d2d81-195">Különböző erőforrások, amelyek általában meghatározzák részletes leírást talál további cikkeit a jelen dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="d2d81-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="d2d81-196">Függőségek</span><span class="sxs-lookup"><span data-stu-id="d2d81-196">Dependencies</span></span>
<span data-ttu-id="d2d81-197">A **dependsOn** elemek megadja egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) egy másik erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d2d81-197">The **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="d2d81-198">A megoldás telepítésekor egy erőforrás nem jön létre, amíg az összes függősége létrejött.</span><span class="sxs-lookup"><span data-stu-id="d2d81-198">When the solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="d2d81-199">A megoldás lehet például [runbookot](operations-management-suite-solutions-resources-automation.md#runbooks) használatával telepített egy [erőforrás feladat](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="d2d81-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="d2d81-200">A feladat erőforrás lenne erőforrástól függ a runbook győződjön meg arról, hogy a runbook létrehozása, a feladat létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="d2d81-200">The job resource would be dependent on the runbook resource to make sure that the runbook is created before the job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="d2d81-201">OMS-munkaterület és Automation-fiók</span><span class="sxs-lookup"><span data-stu-id="d2d81-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="d2d81-202">Megoldások szükséges egy [OMS-munkaterület](../log-analytics/log-analytics-manage-access.md) nézeteket tartalmaz, és egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview) magában foglalja a runbookok és kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d2d81-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) to contain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) to contain runbooks and related resources.</span></span>  <span data-ttu-id="d2d81-203">Ezek előtt elérhetőnek kell lennie a megoldás az erőforrások jönnek létre, és nem lehet megadni, a megoldás magát.</span><span class="sxs-lookup"><span data-stu-id="d2d81-203">These must be available before the resources in the solution are created and should not be defined in the solution itself.</span></span>  <span data-ttu-id="d2d81-204">A felhasználó fog [adjon meg egy munkaterület és a fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) amikor azok a megoldás üzembe helyezéséhez, de a szerző vegye figyelembe a következő szempontokat.</span><span class="sxs-lookup"><span data-stu-id="d2d81-204">The user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as the author you should consider the following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="d2d81-205">Megoldás erőforrás</span><span class="sxs-lookup"><span data-stu-id="d2d81-205">Solution resource</span></span>
<span data-ttu-id="d2d81-206">Minden egyes megoldáshoz szükségesek egy erőforrás bejegyzés a **erőforrások** elem, amely meghatározza a megoldás magát.</span><span class="sxs-lookup"><span data-stu-id="d2d81-206">Each solution requires a resource entry in the **resources** element that defines the solution itself.</span></span>  <span data-ttu-id="d2d81-207">Ez lesz az olyan típusú **Microsoft.OperationsManagement/solutions** és az alábbi szerkezettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d2d81-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have the following structure.</span></span> <span data-ttu-id="d2d81-208">Ez magában foglalja [szabványos paraméterek](#parameters) és [változók](#variables) , amely általában meghatározásához használják a megoldás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="d2d81-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used to define properties of the solution.</span></span>


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a><span data-ttu-id="d2d81-209">Függőségek</span><span class="sxs-lookup"><span data-stu-id="d2d81-209">Dependencies</span></span>
<span data-ttu-id="d2d81-210">A megoldás erőforrás rendelkeznie kell egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) minden más erőforráshoz, a megoldás, mert azokat léteznie kell a megoldás hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="d2d81-210">The solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in the solution since they need to exist before the solution can be created.</span></span>  <span data-ttu-id="d2d81-211">Az egyes erőforrásokra vonatkozó bejegyzés hozzáadásával ehhez a **dependsOn** elemet.</span><span class="sxs-lookup"><span data-stu-id="d2d81-211">You do this by adding an entry for each resource in the **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="d2d81-212">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="d2d81-212">Properties</span></span>
<span data-ttu-id="d2d81-213">A megoldás erőforrás tulajdonságokkal rendelkezik, az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="d2d81-213">The solution resource has the properties in the following table.</span></span>  <span data-ttu-id="d2d81-214">Ez magában foglalja az erőforrások hivatkozik, és szerepelnie kell a megoldás, amely meghatározza, hogyan kezeli az erőforrás a megoldás telepítése után.</span><span class="sxs-lookup"><span data-stu-id="d2d81-214">This includes the resources referenced and contained by the solution which defines how the resource is managed after the solution is installed.</span></span>  <span data-ttu-id="d2d81-215">A megoldás az egyes erőforrások kell szerepelnie, akár a **referencedResources** vagy a **containedResources** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d2d81-215">Each resource in the solution should be listed in either the **referencedResources** or the **containedResources** property.</span></span>

| <span data-ttu-id="d2d81-216">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d2d81-216">Property</span></span> | <span data-ttu-id="d2d81-217">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2d81-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d2d81-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="d2d81-218">workspaceResourceId</span></span> |<span data-ttu-id="d2d81-219">A Naplóelemzési munkaterület formájában azonosító * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Munkaterületnevet\>*.</span><span class="sxs-lookup"><span data-stu-id="d2d81-219">ID of the Log Analytics workspace in the form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="d2d81-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="d2d81-220">referencedResources</span></span> |<span data-ttu-id="d2d81-221">Az erőforrások listájához a a megoldás, nem lehet eltávolítani, a megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-221">List of resources in the solution that should not be removed when the solution is removed.</span></span> |
| <span data-ttu-id="d2d81-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="d2d81-222">containedResources</span></span> |<span data-ttu-id="d2d81-223">Az erőforrások listájához a a megoldás, amely el kell távolítani a megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-223">List of resources in the solution that should be removed when the solution is removed.</span></span> |

<span data-ttu-id="d2d81-224">A fenti példa egy megoldást egy runbookot, egy ütemezést, és tekintse meg a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d2d81-224">The example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="d2d81-225">Az ütemezés és a runbook *hivatkozott* a a **tulajdonságok** elem, ezért nem eltávolítva a megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-225">The schedule and runbook are *referenced* in the  **properties**  element so they are not removed when the solution is removed.</span></span>  <span data-ttu-id="d2d81-226">A nézet *tartalmazott* , a rendszer eltávolítja a megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="d2d81-226">The view is *contained* so it is removed when the solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="d2d81-227">Felkészülés</span><span class="sxs-lookup"><span data-stu-id="d2d81-227">Plan</span></span>
<span data-ttu-id="d2d81-228">A **terv** entitás a megoldás erőforrás tulajdonságokkal rendelkezik, az alábbi táblázatban.</span><span class="sxs-lookup"><span data-stu-id="d2d81-228">The **plan** entity of the solution resource has the properties in the following table.</span></span>

| <span data-ttu-id="d2d81-229">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d2d81-229">Property</span></span> | <span data-ttu-id="d2d81-230">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2d81-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d2d81-231">név</span><span class="sxs-lookup"><span data-stu-id="d2d81-231">name</span></span> |<span data-ttu-id="d2d81-232">A megoldás neve.</span><span class="sxs-lookup"><span data-stu-id="d2d81-232">Name of the solution.</span></span> |
| <span data-ttu-id="d2d81-233">Verzió</span><span class="sxs-lookup"><span data-stu-id="d2d81-233">version</span></span> |<span data-ttu-id="d2d81-234">A megoldás a szerző által meghatározott verziója.</span><span class="sxs-lookup"><span data-stu-id="d2d81-234">Version of the solution as determined by the author.</span></span> |
| <span data-ttu-id="d2d81-235">A termék</span><span class="sxs-lookup"><span data-stu-id="d2d81-235">product</span></span> |<span data-ttu-id="d2d81-236">A megoldás azonosításához egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="d2d81-236">Unique string to identify the solution.</span></span> |
| <span data-ttu-id="d2d81-237">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="d2d81-237">publisher</span></span> |<span data-ttu-id="d2d81-238">A megoldás közzétevője.</span><span class="sxs-lookup"><span data-stu-id="d2d81-238">Publisher of the solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="d2d81-239">Minta</span><span class="sxs-lookup"><span data-stu-id="d2d81-239">Sample</span></span>
<span data-ttu-id="d2d81-240">A következő helyeken megoldás erőforrással megoldásfájlok mintáit tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="d2d81-240">You can view samples of solution files with a solution resource at the following locations.</span></span>

- [<span data-ttu-id="d2d81-241">Automation-erőforrások</span><span class="sxs-lookup"><span data-stu-id="d2d81-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="d2d81-242">Keresés és a riasztás erőforrások</span><span class="sxs-lookup"><span data-stu-id="d2d81-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="d2d81-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2d81-243">Next steps</span></span>
* <span data-ttu-id="d2d81-244">[Adja hozzá a mentett keresések és riasztások](operations-management-suite-solutions-resources-searches-alerts.md) a kezelési megoldással.</span><span class="sxs-lookup"><span data-stu-id="d2d81-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) to your management solution.</span></span>
* <span data-ttu-id="d2d81-245">[Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) a kezelési megoldással.</span><span class="sxs-lookup"><span data-stu-id="d2d81-245">[Add views](operations-management-suite-solutions-resources-views.md) to your management solution.</span></span>
* <span data-ttu-id="d2d81-246">[Adja hozzá a runbookok és egyéb automatizálási erőforrások](operations-management-suite-solutions-resources-automation.md) a kezelési megoldással.</span><span class="sxs-lookup"><span data-stu-id="d2d81-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) to your management solution.</span></span>
* <span data-ttu-id="d2d81-247">További részleteit [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d2d81-247">Learn the details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="d2d81-248">Keresési [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates) példákért különböző Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="d2d81-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
