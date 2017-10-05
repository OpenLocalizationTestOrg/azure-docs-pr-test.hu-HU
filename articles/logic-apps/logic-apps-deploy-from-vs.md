---
title: "Hozzon létre, elkészítéséhez és központi telepítése a Visual Studio - Azure Logic Apps a logic apps |} Microsoft Docs"
description: "Visual Studio-projektek, tervezése, elkészítéséhez és központi telepítése az Azure Logic Apps létrehozása"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="c2e2a-103">Tervezési, elkészítéséhez és központi telepítése az Azure Logic Apps a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="c2e2a-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="c2e2a-104">Bár a [Azure-portálon](https://portal.azure.com/) létrehozása és kezelése az Azure Logic Apps remek mód kínál, tervezési, kiépítési és a logic Apps alkalmazások telepítése a Visual Studio is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-104">Although the [Azure portal](https://portal.azure.com/) offers a great way for you to create and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="c2e2a-105">A Visual Studio hasonlóan ahhoz, hogy a logic Apps alkalmazások létrehozása, telepítése és az automatizálás sablonok konfigurálása és telepítése bármilyen környezethez a Logic App Designer hatékony eszközöket biztosít.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-105">Visual Studio provides rich tools like the Logic App Designer for you to create logic apps, configure deployment and automation templates, and deploy to any environment.</span></span> 

<span data-ttu-id="c2e2a-106">Ismerkedés az Azure Logic Apps, ismerje meg [az első logikai alkalmazás létrehozása az Azure portálon](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c2e2a-106">To get started with Azure Logic Apps, learn [how to create your first logic app in the Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="c2e2a-107">Telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="c2e2a-107">Installation steps</span></span>

<span data-ttu-id="c2e2a-108">Telepítse és konfigurálja a Visual Studio eszközök az Azure Logic Apps, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-108">To install and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c2e2a-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2e2a-109">Prerequisites</span></span>

* <span data-ttu-id="c2e2a-110">[A Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) vagy a Visual Studio 2015-höz</span><span class="sxs-lookup"><span data-stu-id="c2e2a-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="c2e2a-111">[Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="c2e2a-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="c2e2a-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2e2a-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="c2e2a-113">Ha a beágyazott designer segítségével Internet-hozzáféréssel</span><span class="sxs-lookup"><span data-stu-id="c2e2a-113">Access to the web when using the embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="c2e2a-114">Telepítheti a Visual Studio tools for Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="c2e2a-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="c2e2a-115">Miután telepítette az Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-115">After you install the prerequisites:</span></span>

1. <span data-ttu-id="c2e2a-116">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-116">Open Visual Studio.</span></span> <span data-ttu-id="c2e2a-117">Az a **eszközök** menü **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-117">On the **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="c2e2a-118">Bontsa ki a **Online** kategória online kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-118">Expand the **Online** category so you can search online.</span></span>
3. <span data-ttu-id="c2e2a-119">Keresse meg **Logic Apps** amíg meg nem látja **Azure Logic Apps Tools for Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="c2e2a-120">Töltse le és telepítse a bővítmény kattintson **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-120">To download and install the extension, click **Download**.</span></span>
5. <span data-ttu-id="c2e2a-121">Indítsa újra a Visual Studio telepítése után.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e2a-122">Emellett letöltheti [Azure Logic Apps-eszközök Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) és a [Azure Logic Apps eszközök a Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) közvetlenül a Visual Studio piactérről.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and the [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from the Visual Studio Marketplace.</span></span>

<span data-ttu-id="c2e2a-123">A telepítés befejezése után a Logic App-tervezővel is használhatja az Azure erőforráscsoport-projekt.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-123">After you finish installation, you can use the Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="c2e2a-124">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2e2a-124">Create your project</span></span>

1. <span data-ttu-id="c2e2a-125">Az a **fájl** menüben keresse fel **új**, és válassza ki **projekt**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-125">On the **File** menu, go to **New**, and select **Project**.</span></span> <span data-ttu-id="c2e2a-126">Vagy egy meglévő megoldás projektjéhez adásához **Hozzáadás**, és válassza ki **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-126">Or to add your project to an existing solution, go to **Add**, and select **New Project**.</span></span>

    ![Fájl menü](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="c2e2a-128">Az a **új projekt** ablakban található **felhő**, és válassza ki **Azure erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-128">In the **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="c2e2a-129">Nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-129">Name your project, and click **OK**.</span></span>

    ![Új projekt hozzáadása](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="c2e2a-131">Válassza ki a **logikai alkalmazás** sablont, amely létrehoz egy üres logikai alkalmazás központi telepítési sablont használatára.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-131">Select the **Logic App** template, which creates a blank logic app deployment template for you to use.</span></span> <span data-ttu-id="c2e2a-132">A sablon megadása után kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-132">After you select your template, click **OK**.</span></span>

    ![Válassza ki a Logic App-sablon](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="c2e2a-134">Most már hozzáadott a logic app projektet a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-134">You've now added your logic app project to your solution.</span></span> 
    <span data-ttu-id="c2e2a-135">A Megoldáskezelőben a telepítési fájlt meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-135">In the Solution Explorer, your deployment file should appear.</span></span>

    ![Telepítési fájl](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="c2e2a-137">A Logic App tervezővel a logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2e2a-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="c2e2a-138">Ha egy Azure erőforráscsoport-projekt, amely egy logikai alkalmazást tartalmaz, a Logic App Designer nyithatja meg a Visual Studio, a munkafolyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-138">When you have an Azure Resource Group project that contains a logic app, you can open the Logic App Designer in Visual Studio to create your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="c2e2a-139">A Tervező lekérdezés összekötőkre internetkapcsolat szükséges a rendelkezésre álló tulajdonságok és az adatok.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-139">The designer requires an internet connection to query connectors for available properties and data.</span></span> <span data-ttu-id="c2e2a-140">Például ha a Dynamics CRM Online-összekötő használata esetén a Tervező lekérdezi a CRM példányát a rendelkezésre álló egyéni és az alapértelmezett tulajdonságok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-140">For example, if you use the Dynamics CRM Online connector, the designer queries your CRM instance to show available custom and default properties.</span></span>

1. <span data-ttu-id="c2e2a-141">Kattintson a jobb gombbal a `<template>.json` fájlt, és válassza ki **nyissa meg a Logic App tervezővel**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="c2e2a-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="c2e2a-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="c2e2a-143">Válassza ki az Azure-előfizetés, erőforráscsoportot és helyet a központi telepítési sablon.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2e2a-144">Logikai alkalmazás kialakítása API-kapcsolat erőforrások azokhoz a tulajdonságokhoz lekérdezés során létrehoz Tervező.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="c2e2a-145">A Visual Studio a kiválasztott erőforráscsoporthoz alapján hozza létre ezeket a kapcsolatokat tervezés során.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-145">Visual Studio uses your selected resource group to create those connections during design time.</span></span> <span data-ttu-id="c2e2a-146">Megtekintéséhez, vagy módosítsa a API-kapcsolatokat, nyissa meg az Azure portálra, és tallózással keresse meg **API kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-146">To view or change any API Connections, go to the Azure portal, and browse for **API Connections**.</span></span>

    ![Előfizetés kiválasztása](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="c2e2a-148">A designer használja a definíció a `<template>.json` fájl a megjelenítéshez.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-148">The designer uses the definition in the `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="c2e2a-149">Hozzon létre, és a logikai alkalmazás kialakítása.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-149">Create and design your logic app.</span></span> <span data-ttu-id="c2e2a-150">A módosításokat a központi telepítési sablont frissül.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-150">Your deployment template is updated with your changes.</span></span>

    ![A Visual Studio programot Tervező](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="c2e2a-152">A Visual Studio hozzáadja `Microsoft.Web/connections` a kapcsolatokat erőforrásfájljából erőforrásokat a Logic Apps alkalmazást kell működni.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-152">Visual Studio adds `Microsoft.Web/connections` resources to your resource file for any connections your logic app needs to function.</span></span> <span data-ttu-id="c2e2a-153">A kapcsolati tulajdonságok kell telepítésekor, és állítsa a telepítése után felügyelt **API kapcsolatok** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in the Azure portal.</span></span>

### <a name="switch-to-json-code-view"></a><span data-ttu-id="c2e2a-154">JSON-kód nézetre váltáshoz</span><span class="sxs-lookup"><span data-stu-id="c2e2a-154">Switch to JSON code view</span></span>

<span data-ttu-id="c2e2a-155">A logikai alkalmazásnak a JSON-megjelenítés megjelenítéséhez jelölje ki a **kódnézetben** lapon a designer alján.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-155">To show the JSON representation for your logic app, select the **Code View** tab at the bottom of the designer.</span></span>

<span data-ttu-id="c2e2a-156">Váltson vissza a teljes körű erőforrást JSON, kattintson a jobb gombbal a `<template>.json` fájlt, és válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-156">To switch back to the full resource JSON, right-click the `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a><span data-ttu-id="c2e2a-157">Hivatkozások a tőle függő erőforrások hozzáadása a Visual Studio központi telepítési sablonok</span><span class="sxs-lookup"><span data-stu-id="c2e2a-157">Add references for dependent resources to Visual Studio deployment templates</span></span>

<span data-ttu-id="c2e2a-158">Ha azt szeretné, hogy a logikai alkalmazás függő erőforrásokra kell hivatkoznia, [Azure Resource Manager sablonfüggvényei](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) logic app központi telepítési sablonba.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-158">When you want your logic app to reference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="c2e2a-159">Például érdemes hivatkozhasson rá az Azure-funkció vagy integrációs fiókkal, amely mellett a Logic Apps alkalmazást telepíteni szeretné a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-159">For example, you might want your logic app to reference an Azure Function or integration account that you want to deploy alongside your logic app.</span></span> <span data-ttu-id="c2e2a-160">Kövesse az alábbi irányelveket a központi telepítési sablont a paraméterek használatáról, úgy, hogy a Logic App Designer Renderelés megfelelően.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-160">Follow these guidelines about how to use parameters in your deployment template so that the Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="c2e2a-161">Eseményindítók és műveletek az ilyen típusú logic app paramétereket használhatja:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="c2e2a-162">Alárendelt munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="c2e2a-162">Child workflow</span></span>
*   <span data-ttu-id="c2e2a-163">Függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c2e2a-163">Function app</span></span>
*   <span data-ttu-id="c2e2a-164">APIM hívás</span><span class="sxs-lookup"><span data-stu-id="c2e2a-164">APIM call</span></span>
*   <span data-ttu-id="c2e2a-165">API kapcsolat futásidejű URL-címe</span><span class="sxs-lookup"><span data-stu-id="c2e2a-165">API connection runtime URL</span></span>
*   <span data-ttu-id="c2e2a-166">API-kapcsolati útvonal</span><span class="sxs-lookup"><span data-stu-id="c2e2a-166">API connection path</span></span>

<span data-ttu-id="c2e2a-167">És használhatja a sablont funkciókat, például a paraméterek, változókat, resourceId, concat, stb. Például ez hogyan lecserélheti az Azure-függvény erőforrás-azonosító:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace the Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="c2e2a-168">És ahol paraméterek szeretné használni:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-168">And where you would use parameters:</span></span>

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
<span data-ttu-id="c2e2a-169">Másik példaként a Service Bus küldési üzenet művelet is parametrizálja:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-169">As another example you can parameterize the Service Bus send message operation:</span></span>

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> <span data-ttu-id="c2e2a-170">host.runtimeUrl nem kötelező, és eltávolíthatja a sablon ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="c2e2a-171">A Logic App Designer működéséhez paraméterek használatakor meg kell adni alapértelmezett értéket, például:</span><span class="sxs-lookup"><span data-stu-id="c2e2a-171">For the Logic App Designer to work when you use parameters, you must provide default values, for example:</span></span>
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a><span data-ttu-id="c2e2a-172">A logikai alkalmazás mentése</span><span class="sxs-lookup"><span data-stu-id="c2e2a-172">Save your logic app</span></span>

<span data-ttu-id="c2e2a-173">A Logic Apps alkalmazást bármikor mentéséhez navigáljon **fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-173">To save your logic app at anytime, go to **File** > **Save**.</span></span> <span data-ttu-id="c2e2a-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="c2e2a-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="c2e2a-175">Ha a Logic Apps alkalmazást hibák az alkalmazás mentésekor, azok megjelennek a Visual Studio **kimenetek** ablak.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-175">If your logic app has any errors when you save your app, they appear in the Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="c2e2a-176">A Logic Apps alkalmazást a Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="c2e2a-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="c2e2a-177">Az alkalmazás konfigurálását, telepítheti közvetlenül a Visual Studio csak néhány lépésben.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="c2e2a-178">A Megoldáskezelőben kattintson jobb gombbal a projektre, és navigáljon a **telepítés** > **új központi telepítési...**</span><span class="sxs-lookup"><span data-stu-id="c2e2a-178">In Solution Explorer, right-click your project, and go to **Deploy** > **New Deployment...**</span></span>

    ![Új központi telepítés](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="c2e2a-180">Amikor a rendszer kéri, jelentkezzen be az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-180">When you're prompted, sign in to your Azure subscription.</span></span> 

3. <span data-ttu-id="c2e2a-181">Most már ki kell választania az erőforráscsoportot, ahol a Logic Apps alkalmazást telepíteni szeretné a részletes.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-181">Now you must select the details for the resource group where you want to deploy your logic app.</span></span> <span data-ttu-id="c2e2a-182">Amikor elkészült, válassza ki a **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c2e2a-183">Győződjön meg arról, hogy bejelöli-e a megfelelő sablon és a paraméterek fájlt ahhoz az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-183">Make sure that you select the correct template and parameters file for the resource group.</span></span> <span data-ttu-id="c2e2a-184">Ha szeretné telepíteni az éles környezetben való használatra, például éles paraméterek fájl kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-184">For example, if you want to deploy to a production environment, choose the production parameters file.</span></span>

    ![Erőforráscsoport telepítése](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="c2e2a-186">A telepítés állapota megjelenik a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-186">The deployment status appears in the **Output** window.</span></span> 
    <span data-ttu-id="c2e2a-187">Ki kell választania **Azure kiépítés** a a **megjelenítése kimenetét** listája.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-187">You might have to select **Azure Provisioning** in the **Show output from** list.</span></span>

    ![Központi telepítési állapot kimeneti](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="c2e2a-189">A jövőben a Logic Apps alkalmazást az adatforrás-vezérlő szerkesztése, és új verziók helyezhetők üzembe a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-189">In the future, you can edit your logic app in source control, and use Visual Studio to deploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e2a-190">Az Azure portálon definition közvetlenül módosítja, ezeket a módosításokat a rendszer felülírja központi telepítésekor a Visual Studio eszközből legközelebb.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-190">If you change the definition in the Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a><span data-ttu-id="c2e2a-191">A logikai alkalmazás hozzáadása egy meglévő erőforráscsoport-projekt</span><span class="sxs-lookup"><span data-stu-id="c2e2a-191">Add your logic app to an existing Resource Group project</span></span>

<span data-ttu-id="c2e2a-192">Ha egy meglévő erőforráscsoport-projekt, adhat hozzá a logikai alkalmazás erre a projektre a JSON-vázlat ablak.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-192">If you have an existing Resource Group project, you can add your logic app to that project in the JSON Outline window.</span></span> <span data-ttu-id="c2e2a-193">Egy másik logikai alkalmazást a korábban létrehozott alkalmazás mellett azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-193">You can also add another logic app alongside the app you previously created.</span></span>

1. <span data-ttu-id="c2e2a-194">Nyissa meg az `<template>.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-194">Open the `<template>.json` file.</span></span>

2. <span data-ttu-id="c2e2a-195">A JSON-vázlat ablak megnyitásához lépjen **nézet** > **más Windows** > **JSON-vázlat**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-195">To open the JSON Outline window, go to **View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="c2e2a-196">A sablonfájl egy erőforrás hozzáadásához kattintson **erőforrás hozzáadása** a JSON-vázlat ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-196">To add a resource to the template file, click **Add Resource** at the top of the JSON Outline window.</span></span> <span data-ttu-id="c2e2a-197">A JSON-vázlat ablak, kattintson a jobb gombbal vagy **erőforrások**, és válassza ki **új erőforrás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-197">Or in the JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![JSON-vázlat ablak](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="c2e2a-199">Az a **erőforrás hozzáadása** párbeszédpanel, keresése és kijelölése **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-199">In the **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="c2e2a-200">A logikai alkalmazás neve, és válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c2e2a-200">Name your logic app, and choose **Add**.</span></span>

    ![Erőforrás hozzáadása](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="c2e2a-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2e2a-202">Next Steps</span></span>

* [<span data-ttu-id="c2e2a-203">A Visual Studio Cloud Explorer logic Apps-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="c2e2a-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* [<span data-ttu-id="c2e2a-204">Gyakori példák és felhasználási helyzetek megtekintése</span><span class="sxs-lookup"><span data-stu-id="c2e2a-204">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="c2e2a-205">Az Azure Logic Apps üzleti folyamatok automatizálása</span><span class="sxs-lookup"><span data-stu-id="c2e2a-205">Learn how to automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="c2e2a-206">Útmutató: az Azure Logic Apps rendszerintegráció</span><span class="sxs-lookup"><span data-stu-id="c2e2a-206">Learn how to integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
