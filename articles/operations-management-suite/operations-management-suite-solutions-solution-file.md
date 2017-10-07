---
title: "az Operations Management Suite (OMS) megoldások aaaCreating |} Microsoft Docs"
description: "Megoldások bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir OMS-munkaterület adhat hozzá.  Ez a cikk ismerteti, hogyan hozhat létre felügyeleti megoldások toobe részleteinek a saját környezetben használt, illetve mikor elérhető tooyour ügyfelek."
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
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="1d518-104">A felügyeleti megoldás fájl létrehozása az Operations Management Suite (OMS) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="1d518-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="1d518-105">Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak.</span><span class="sxs-lookup"><span data-stu-id="1d518-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="1d518-106">Az alábbiakban semmilyen sémát tulajdonos toochange.</span><span class="sxs-lookup"><span data-stu-id="1d518-106">Any schema described below is subject toochange.</span></span>  

<span data-ttu-id="1d518-107">Az Operations Management Suite (OMS) megoldások használják, mint [Resource Manager-sablonok](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="1d518-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="1d518-108">Hogyan tooauthor megoldások hogyan túl tanulási tanulási fő feladat hello[sablon szerzői](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1d518-108">hello main task in learning how tooauthor management solutions is learning how too[author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="1d518-109">Ez a cikk részletesen bemutatja a egyedi használt megoldások sablonjainak és hogyan tooconfigure szokásos megoldás erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1d518-109">This article provides unique details of templates used for solutions and how tooconfigure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="1d518-110">Eszközök</span><span class="sxs-lookup"><span data-stu-id="1d518-110">Tools</span></span>

<span data-ttu-id="1d518-111">Bármely szöveg szerkesztő toowork megoldásfájlok használható, de azt javasoljuk, ami hello szolgáltatásai a Visual Studio vagy Visual Studio Code hello a következő cikkekben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="1d518-111">You can use any text editor toowork with solution files, but we recommend leveraging hello features provided in Visual Studio or Visual Studio Code as described in hello following articles.</span></span>

- [<span data-ttu-id="1d518-112">Létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok</span><span class="sxs-lookup"><span data-stu-id="1d518-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="1d518-113">A Visual Studio Code Azure Resource Manager-sablonok használata</span><span class="sxs-lookup"><span data-stu-id="1d518-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="1d518-114">struktúra</span><span class="sxs-lookup"><span data-stu-id="1d518-114">Structure</span></span>
<span data-ttu-id="1d518-115">hello alapszintű struktúrát egy felügyeleti megoldás fájl van hello ugyanaz, mint egy [Resource Manager-sablon](../azure-resource-manager/resource-group-authoring-templates.md#template-format) Ez az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="1d518-115">hello basic structure of a management solution file is hello same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="1d518-116">Egyes hello lentebbi hello legfelső szintű elemeket ismerteti és és azok tartalmát, a megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-116">Each of hello sections below describes hello top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="1d518-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1d518-117">Parameters</span></span>
<span data-ttu-id="1d518-118">[Paraméterek](../azure-resource-manager/resource-group-authoring-templates.md#parameters) értékek, amelyekre szüksége van a hello felhasználó hello felügyeleti megoldás telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="1d518-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from hello user when they install hello management solution.</span></span>  <span data-ttu-id="1d518-119">Standard paramétert, amely minden megoldás fog rendelkezni, és az adott megoldáshoz szükség szerint további paramétereket is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="1d518-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="1d518-120">Hogyan felhasználók nyújtják a paraméterértékek való telepítésekor a megoldás hello adott paraméter és hello megoldás telepítési módjának függ.</span><span class="sxs-lookup"><span data-stu-id="1d518-120">How users will provide parameter values when they install your solution will depend on hello particular parameter and how hello solution is being installed.</span></span>

<span data-ttu-id="1d518-121">Amikor a felhasználó telepíti a felügyeleti megoldás keresztül hello [Azure piactér](operations-management-suite-solutions.md#finding-and-installing-management-solutions) vagy [Azure gyors üzembe helyezési sablonokat](operations-management-suite-solutions.md#finding-and-installing-management-solutions) rákérdezéses tooselect egy [OMS munkaterületet, és az Automation-fiók ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="1d518-121">When a user installs your management solution through hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted tooselect an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="1d518-122">Ezek a használt toopopulate hello értékek egyes hello szabványos paraméterek.</span><span class="sxs-lookup"><span data-stu-id="1d518-122">These are used toopopulate hello values of each of hello standard parameters.</span></span>  <span data-ttu-id="1d518-123">hello nem felhasználótól toodirectly hello szabványos paraméterek értékének megadására, ugyanakkor felszólító tooprovide semmilyen további paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="1d518-123">hello user is not prompted toodirectly provide values for hello standard parameters, but they are prompted tooprovide values for any additional parameters.</span></span>

<span data-ttu-id="1d518-124">Ha hello felhasználó telepíti a megoldás [egy másik módszer](operations-management-suite-solutions.md#finding-and-installing-management-solutions), meg kell adniuk egy értéket az összes szabványos és az összes további paraméterei.</span><span class="sxs-lookup"><span data-stu-id="1d518-124">When hello user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="1d518-125">Az alábbiakban látható egy minta paraméter.</span><span class="sxs-lookup"><span data-stu-id="1d518-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="1d518-126">hello a következő táblázatban a paraméter hello attribútumait ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1d518-126">hello following table describes hello attributes of a parameter.</span></span>

| <span data-ttu-id="1d518-127">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1d518-127">Attribute</span></span> | <span data-ttu-id="1d518-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="1d518-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1d518-129">type</span><span class="sxs-lookup"><span data-stu-id="1d518-129">type</span></span> |<span data-ttu-id="1d518-130">Hello paraméter adattípusa.</span><span class="sxs-lookup"><span data-stu-id="1d518-130">Data type for hello parameter.</span></span> <span data-ttu-id="1d518-131">hello bemeneti vezérlő hello felhasználó számára megjelenített hello adatok típusától függ.</span><span class="sxs-lookup"><span data-stu-id="1d518-131">hello input control displayed for hello user depends on hello data type.</span></span><br><br><span data-ttu-id="1d518-132">logikai érték - a legördülő listából</span><span class="sxs-lookup"><span data-stu-id="1d518-132">bool - Drop down box</span></span><br><span data-ttu-id="1d518-133">karakterlánc - szövegmező</span><span class="sxs-lookup"><span data-stu-id="1d518-133">string - Text box</span></span><br><span data-ttu-id="1d518-134">int - szövegmező</span><span class="sxs-lookup"><span data-stu-id="1d518-134">int - Text box</span></span><br><span data-ttu-id="1d518-135">SecureString - jelszó mező</span><span class="sxs-lookup"><span data-stu-id="1d518-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="1d518-136">category</span><span class="sxs-lookup"><span data-stu-id="1d518-136">category</span></span> |<span data-ttu-id="1d518-137">Nem kötelező kategória hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="1d518-137">Optional category for hello parameter.</span></span>  <span data-ttu-id="1d518-138">Az azonos kategóriába sorolhatók hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="1d518-138">Parameters in hello same category are grouped together.</span></span> |
| <span data-ttu-id="1d518-139">vezérlő</span><span class="sxs-lookup"><span data-stu-id="1d518-139">control</span></span> |<span data-ttu-id="1d518-140">További funkciók karakterlánc-paraméter.</span><span class="sxs-lookup"><span data-stu-id="1d518-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="1d518-141">datetime - Datetime vezérlő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1d518-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="1d518-142">GUID - Guid-érték automatikusan jön létre, és hello paraméter nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1d518-142">guid - Guid value is automatically generated, and hello parameter is not displayed.</span></span> |
| <span data-ttu-id="1d518-143">leírás</span><span class="sxs-lookup"><span data-stu-id="1d518-143">description</span></span> |<span data-ttu-id="1d518-144">Hello paraméter nem kötelező leírása.</span><span class="sxs-lookup"><span data-stu-id="1d518-144">Optional description for hello parameter.</span></span>  <span data-ttu-id="1d518-145">Megjelenik az információkat a buborékban megjelenő következő toohello paraméter.</span><span class="sxs-lookup"><span data-stu-id="1d518-145">Displayed in an information balloon next toohello parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="1d518-146">Szabványos paraméterek</span><span class="sxs-lookup"><span data-stu-id="1d518-146">Standard parameters</span></span>
<span data-ttu-id="1d518-147">hello alábbi táblázat az összes felügyeleti megoldások szabványos paramétereinek hello.</span><span class="sxs-lookup"><span data-stu-id="1d518-147">hello following table lists hello standard parameters for all management solutions.</span></span>  <span data-ttu-id="1d518-148">Ezek az értékek helyett adatkérés el a megoldás hello Azure piactér vagy gyorsindítási sablonok alapján telepítésekor hello felhasználó fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="1d518-148">These values are populated for hello user instead of prompting for them when your solution is installed from hello Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="1d518-149">Ha hello megoldás telepítve van egy másik módszerrel hello felhasználói értékeket kell adnia a számukra.</span><span class="sxs-lookup"><span data-stu-id="1d518-149">hello user must provide values for them if hello solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="1d518-150">hello felhasználói felületének hello Azure piactér és gyorsindítási sablonok által várt paraméterekkel hello paraméterneveknek hello táblában.</span><span class="sxs-lookup"><span data-stu-id="1d518-150">hello user interface in hello Azure Marketplace and Quickstart templates is expecting hello parameter names in hello table.</span></span>  <span data-ttu-id="1d518-151">Ha különböző paraméternevek majd hello kéri a felhasználótól a számukra, és azok nem automatikusan tölti fel.</span><span class="sxs-lookup"><span data-stu-id="1d518-151">If you use different parameter names then hello user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="1d518-152">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1d518-152">Parameter</span></span> | <span data-ttu-id="1d518-153">Típus</span><span class="sxs-lookup"><span data-stu-id="1d518-153">Type</span></span> | <span data-ttu-id="1d518-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="1d518-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="1d518-155">Fióknév</span><span class="sxs-lookup"><span data-stu-id="1d518-155">accountName</span></span> |<span data-ttu-id="1d518-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-156">string</span></span> |<span data-ttu-id="1d518-157">Azure Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="1d518-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="1d518-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="1d518-158">pricingTier</span></span> |<span data-ttu-id="1d518-159">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-159">string</span></span> |<span data-ttu-id="1d518-160">A Naplóelemzési munkaterület- és Azure Automation-fiók tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="1d518-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="1d518-161">regionId</span><span class="sxs-lookup"><span data-stu-id="1d518-161">regionId</span></span> |<span data-ttu-id="1d518-162">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-162">string</span></span> |<span data-ttu-id="1d518-163">Azure Automation-fiók hello területet.</span><span class="sxs-lookup"><span data-stu-id="1d518-163">Region of hello Azure Automation account.</span></span> |
| <span data-ttu-id="1d518-164">Megoldás neve</span><span class="sxs-lookup"><span data-stu-id="1d518-164">solutionName</span></span> |<span data-ttu-id="1d518-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-165">string</span></span> |<span data-ttu-id="1d518-166">Hello megoldás nevét.</span><span class="sxs-lookup"><span data-stu-id="1d518-166">Name of hello solution.</span></span>  <span data-ttu-id="1d518-167">Központi telepítése a megoldás gyorsindítási sablonok keresztül, majd meg kell határozni megoldás neve paraméterként, egy karakterlánc, ehelyett igénylő hello felhasználói toospecify egy definiálhat.</span><span class="sxs-lookup"><span data-stu-id="1d518-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring hello user toospecify one.</span></span> |
| <span data-ttu-id="1d518-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="1d518-168">workspaceName</span></span> |<span data-ttu-id="1d518-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-169">string</span></span> |<span data-ttu-id="1d518-170">Napló Analytics munkaterület neve.</span><span class="sxs-lookup"><span data-stu-id="1d518-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="1d518-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="1d518-171">workspaceRegionId</span></span> |<span data-ttu-id="1d518-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1d518-172">string</span></span> |<span data-ttu-id="1d518-173">Hello Naplóelemzési munkaterület területet.</span><span class="sxs-lookup"><span data-stu-id="1d518-173">Region of hello Log Analytics workspace.</span></span> |


<span data-ttu-id="1d518-174">Az alábbiakban az hello szerkezete hello szabványos paraméterek másolja és illessze be a megoldásfájlt.</span><span class="sxs-lookup"><span data-stu-id="1d518-174">Following is hello structure of hello standard parameters that you can copy and paste into your solution file.</span></span>  

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
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="1d518-175">Tekintse meg a hello megoldás hello szintaxissal egyéb elemeinek értékei tooparameter **paraméter ("név" paraméternek)**.</span><span class="sxs-lookup"><span data-stu-id="1d518-175">You refer tooparameter values in other elements of hello solution with hello syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="1d518-176">Ha például tooaccess hello munkaterület nevét, szeretné használni **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="1d518-176">For example, tooaccess hello workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="1d518-177">Változók</span><span class="sxs-lookup"><span data-stu-id="1d518-177">Variables</span></span>
<span data-ttu-id="1d518-178">[Változók](../azure-resource-manager/resource-group-authoring-templates.md#variables) hello többi hello felügyeleti megoldást használni kívánt érték.</span><span class="sxs-lookup"><span data-stu-id="1d518-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in hello rest of hello management solution.</span></span>  <span data-ttu-id="1d518-179">Ezek az értékek nincsenek kitett toohello felhasználó hello megoldás is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="1d518-179">These values are not exposed toohello user installing hello solution.</span></span>  <span data-ttu-id="1d518-180">Tervezett tooprovide hello Szerző rendelkező meg egy helyet, ahol kezelésére használható többször hello megoldás teljes értékek.</span><span class="sxs-lookup"><span data-stu-id="1d518-180">They are intended tooprovide hello author with a single location where they can manage values that may be used multiple times throughout hello solution.</span></span> <span data-ttu-id="1d518-181">El kell helyezni egy értékek adott tooyour megoldásba változók kódolása őket a hello megakadályozását toohard, **erőforrások** elemet.</span><span class="sxs-lookup"><span data-stu-id="1d518-181">You should put any values specific tooyour solution in variables as opposed toohard coding them in hello **resources** element.</span></span>  <span data-ttu-id="1d518-182">Ez megkönnyíti a hello kód olvashatóbbá, és lehetővé teszi a tooeasily módosítani ezeket az értékeket az újabb verziókban.</span><span class="sxs-lookup"><span data-stu-id="1d518-182">This makes hello code more readable and allows you tooeasily change these values in later versions.</span></span>

<span data-ttu-id="1d518-183">Az alábbiakban látható egy példa egy **változók** elem megoldások használt általános paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="1d518-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="1d518-184">Tekintse meg a toovariable értékek hello megoldással hello szintaxissal **változók ("változó neve")**.</span><span class="sxs-lookup"><span data-stu-id="1d518-184">You refer toovariable values through hello solution with hello syntax **variables('variable name')**.</span></span>  <span data-ttu-id="1d518-185">Ha például tooaccess hello megoldás neve változó, szeretné használni **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="1d518-185">For example, tooaccess hello SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="1d518-186">Azt is megadhatja, komplex változók értékeinek beállítja, hogy több.</span><span class="sxs-lookup"><span data-stu-id="1d518-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="1d518-187">Ezek akkor igazán hasznosak-kezelési megoldásokban ahol több különböző típusú erőforrások tulajdonság meghatározásakor.</span><span class="sxs-lookup"><span data-stu-id="1d518-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="1d518-188">Például sikerült átalakítása hello megoldás változók toohello következő fent látható.</span><span class="sxs-lookup"><span data-stu-id="1d518-188">For example, you could restructure hello solution variables shown above toohello following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="1d518-189">Ebben az esetben tekintse meg az toovariable értékek hello megoldással hello szintaxissal **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="1d518-189">In this case, you refer toovariable values through hello solution with hello syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="1d518-190">Ha például tooaccess hello megoldásnév változó, szeretné használni **variables('Solution'). Név**.</span><span class="sxs-lookup"><span data-stu-id="1d518-190">For example, tooaccess hello Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="1d518-191">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="1d518-191">Resources</span></span>
<span data-ttu-id="1d518-192">[Erőforrások](../azure-resource-manager/resource-group-authoring-templates.md#resources) meghatározása hello különböző erőforrások, amelyek a felügyeleti megoldás telepíteni és konfigurálni fog.</span><span class="sxs-lookup"><span data-stu-id="1d518-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define hello different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="1d518-193">Ez lesz a legnagyobb hello és hello sablon legösszetettebb része.</span><span class="sxs-lookup"><span data-stu-id="1d518-193">This will be hello largest and most complex portion of hello template.</span></span>  <span data-ttu-id="1d518-194">Hello struktúra és a teljes leírását az erőforrás-elemek [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="1d518-194">You can get hello structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="1d518-195">Különböző erőforrások, amelyek általában meghatározzák részletes leírást talál további cikkeit a jelen dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="1d518-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="1d518-196">Függőségek</span><span class="sxs-lookup"><span data-stu-id="1d518-196">Dependencies</span></span>
<span data-ttu-id="1d518-197">Hello **dependsOn** elemek megadja egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) egy másik erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1d518-197">hello **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="1d518-198">Hello megoldás telepítésekor egy erőforrás nem jön létre, amíg az összes függősége létrejött.</span><span class="sxs-lookup"><span data-stu-id="1d518-198">When hello solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="1d518-199">A megoldás lehet például [runbookot](operations-management-suite-solutions-resources-automation.md#runbooks) használatával telepített egy [erőforrás feladat](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="1d518-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="1d518-200">hello feladat erőforrás lenne a függő hello runbook erőforrás toomake meg arról, hogy az adott hello runbook jön létre, mielőtt hello feladat jön létre.</span><span class="sxs-lookup"><span data-stu-id="1d518-200">hello job resource would be dependent on hello runbook resource toomake sure that hello runbook is created before hello job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="1d518-201">OMS-munkaterület és Automation-fiók</span><span class="sxs-lookup"><span data-stu-id="1d518-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="1d518-202">Megoldások szükséges egy [OMS-munkaterület](../log-analytics/log-analytics-manage-access.md) toocontain nézetek és egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview) toocontain runbookok és a kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="1d518-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) toocontain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks and related resources.</span></span>  <span data-ttu-id="1d518-203">Ezek hello hello megoldásban erőforrások jönnek létre, és nem lehet megadni, maga hello megoldásban előtt elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1d518-203">These must be available before hello resources in hello solution are created and should not be defined in hello solution itself.</span></span>  <span data-ttu-id="1d518-204">hello felhasználó fog [adjon meg egy munkaterület és a fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) amikor azok a megoldás üzembe helyezéséhez, de hello Szerző vegye figyelembe a következő pontok hello.</span><span class="sxs-lookup"><span data-stu-id="1d518-204">hello user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as hello author you should consider hello following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="1d518-205">Megoldás erőforrás</span><span class="sxs-lookup"><span data-stu-id="1d518-205">Solution resource</span></span>
<span data-ttu-id="1d518-206">Minden egyes megoldáshoz szükségesek egy erőforrás bejegyzés hello **erőforrások** elem, amely meghatározza a hello megoldás magát.</span><span class="sxs-lookup"><span data-stu-id="1d518-206">Each solution requires a resource entry in hello **resources** element that defines hello solution itself.</span></span>  <span data-ttu-id="1d518-207">Ez lesz az olyan típusú **Microsoft.OperationsManagement/solutions** és a következő struktúra hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1d518-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have hello following structure.</span></span> <span data-ttu-id="1d518-208">Ez magában foglalja [szabványos paraméterek](#parameters) és [változók](#variables) , amelyek általánosan használt toodefine tulajdonságok hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used toodefine properties of hello solution.</span></span>


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




### <a name="dependencies"></a><span data-ttu-id="1d518-209">Függőségek</span><span class="sxs-lookup"><span data-stu-id="1d518-209">Dependencies</span></span>
<span data-ttu-id="1d518-210">hello megoldás erőforrás rendelkeznie kell egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) hello megoldás, mert azokat tooexist hello megoldás létrehozása előtt minden más erőforráson.</span><span class="sxs-lookup"><span data-stu-id="1d518-210">hello solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in hello solution since they need tooexist before hello solution can be created.</span></span>  <span data-ttu-id="1d518-211">Ehhez egy bejegyzést az egyes erőforrások hozzáadása a hello **dependsOn** elemet.</span><span class="sxs-lookup"><span data-stu-id="1d518-211">You do this by adding an entry for each resource in hello **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="1d518-212">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="1d518-212">Properties</span></span>
<span data-ttu-id="1d518-213">hello megoldás erőforrás a következő táblázat hello hello tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1d518-213">hello solution resource has hello properties in hello following table.</span></span>  <span data-ttu-id="1d518-214">Ez magában foglalja a hello erőforrások hivatkozott és hello megoldás, amely meghatározza, hogyan hello erőforrás felügyelt hello megoldás telepítése után által tartalmazott.</span><span class="sxs-lookup"><span data-stu-id="1d518-214">This includes hello resources referenced and contained by hello solution which defines how hello resource is managed after hello solution is installed.</span></span>  <span data-ttu-id="1d518-215">Az egyes erőforrások hello megoldásban szerepelnie kell vagy hello **referencedResources** vagy hello **containedResources** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1d518-215">Each resource in hello solution should be listed in either hello **referencedResources** or hello **containedResources** property.</span></span>

| <span data-ttu-id="1d518-216">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1d518-216">Property</span></span> | <span data-ttu-id="1d518-217">Leírás</span><span class="sxs-lookup"><span data-stu-id="1d518-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1d518-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="1d518-218">workspaceResourceId</span></span> |<span data-ttu-id="1d518-219">Hello formában hello Naplóelemzési munkaterület azonosítója  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Munkaterületnevet\>*.</span><span class="sxs-lookup"><span data-stu-id="1d518-219">ID of hello Log Analytics workspace in hello form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="1d518-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="1d518-220">referencedResources</span></span> |<span data-ttu-id="1d518-221">Az erőforrások listájához a hello megoldás, nem lehet eltávolítani, hello megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="1d518-221">List of resources in hello solution that should not be removed when hello solution is removed.</span></span> |
| <span data-ttu-id="1d518-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="1d518-222">containedResources</span></span> |<span data-ttu-id="1d518-223">Az erőforrások listájához a hello megoldás, amely hello megoldás eltávolításakor el kell távolítani.</span><span class="sxs-lookup"><span data-stu-id="1d518-223">List of resources in hello solution that should be removed when hello solution is removed.</span></span> |

<span data-ttu-id="1d518-224">hello fenti vonatkozik, amely runbook, egy ütemezést, és tekintse meg a megoldást.</span><span class="sxs-lookup"><span data-stu-id="1d518-224">hello example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="1d518-225">hello ütemezés és a runbook *hivatkozott* a hello **tulajdonságok** elem, ezért nem eltávolítva hello megoldás eltávolításakor.</span><span class="sxs-lookup"><span data-stu-id="1d518-225">hello schedule and runbook are *referenced* in hello  **properties**  element so they are not removed when hello solution is removed.</span></span>  <span data-ttu-id="1d518-226">hello nézet *tartalmazott* , az hello megoldás eltávolításakor törlődik.</span><span class="sxs-lookup"><span data-stu-id="1d518-226">hello view is *contained* so it is removed when hello solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="1d518-227">Felkészülés</span><span class="sxs-lookup"><span data-stu-id="1d518-227">Plan</span></span>
<span data-ttu-id="1d518-228">Hello **terv** hello megoldás erőforrás entitás hello tulajdonságokkal rendelkezik a következő táblázat hello.</span><span class="sxs-lookup"><span data-stu-id="1d518-228">hello **plan** entity of hello solution resource has hello properties in hello following table.</span></span>

| <span data-ttu-id="1d518-229">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1d518-229">Property</span></span> | <span data-ttu-id="1d518-230">Leírás</span><span class="sxs-lookup"><span data-stu-id="1d518-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1d518-231">név</span><span class="sxs-lookup"><span data-stu-id="1d518-231">name</span></span> |<span data-ttu-id="1d518-232">Hello megoldás nevét.</span><span class="sxs-lookup"><span data-stu-id="1d518-232">Name of hello solution.</span></span> |
| <span data-ttu-id="1d518-233">Verzió</span><span class="sxs-lookup"><span data-stu-id="1d518-233">version</span></span> |<span data-ttu-id="1d518-234">Hello megoldás hello Szerző alapján verziója.</span><span class="sxs-lookup"><span data-stu-id="1d518-234">Version of hello solution as determined by hello author.</span></span> |
| <span data-ttu-id="1d518-235">A termék</span><span class="sxs-lookup"><span data-stu-id="1d518-235">product</span></span> |<span data-ttu-id="1d518-236">Egyedi karakterlánc tooidentify hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-236">Unique string tooidentify hello solution.</span></span> |
| <span data-ttu-id="1d518-237">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="1d518-237">publisher</span></span> |<span data-ttu-id="1d518-238">A Publisher hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-238">Publisher of hello solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="1d518-239">Minta</span><span class="sxs-lookup"><span data-stu-id="1d518-239">Sample</span></span>
<span data-ttu-id="1d518-240">Megoldásfájlok megoldás erőforrással mintát, az alábbi helyek hello tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="1d518-240">You can view samples of solution files with a solution resource at hello following locations.</span></span>

- [<span data-ttu-id="1d518-241">Automation-erőforrások</span><span class="sxs-lookup"><span data-stu-id="1d518-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="1d518-242">Keresés és a riasztás erőforrások</span><span class="sxs-lookup"><span data-stu-id="1d518-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="1d518-243">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d518-243">Next steps</span></span>
* <span data-ttu-id="1d518-244">[Adja hozzá a mentett keresések és riasztások](operations-management-suite-solutions-resources-searches-alerts.md) tooyour felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) tooyour management solution.</span></span>
* <span data-ttu-id="1d518-245">[Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) tooyour felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-245">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="1d518-246">[Adja hozzá a runbookok és egyéb automatizálási erőforrások](operations-management-suite-solutions-resources-automation.md) tooyour felügyeleti megoldás.</span><span class="sxs-lookup"><span data-stu-id="1d518-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>
* <span data-ttu-id="1d518-247">Ismerje meg, hello részleteit [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1d518-247">Learn hello details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1d518-248">Keresési [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates) példákért különböző Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="1d518-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
