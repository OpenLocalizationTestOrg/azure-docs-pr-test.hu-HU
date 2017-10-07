---
title: "aaaCreate, elkészítéséhez és központi telepítése a Visual Studio - Azure Logic Apps a logic apps |} Microsoft Docs"
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
ms.openlocfilehash: 5154cb05f9a48e9f0f2381a6953947217f7bb114
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a><span data-ttu-id="d09fe-103">Tervezési, elkészítéséhez és központi telepítése az Azure Logic Apps a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="d09fe-103">Design, build, and deploy Azure Logic Apps in Visual Studio</span></span>

<span data-ttu-id="d09fe-104">Bár hello [Azure-portálon](https://portal.azure.com/) nagyszerű lehetőséget nyújt az Ön toocreate és kezelése az Azure Logic Apps, tervezési, kiépítési és a logic Apps alkalmazások telepítése a Visual Studio is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d09fe-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toocreate and manage Azure Logic Apps, you can use Visual Studio for designing, building, and deploying your logic apps.</span></span> <span data-ttu-id="d09fe-105">A Visual Studio eszközöket biztosít a gazdag hello Logic App Designer például akkor toocreate a logic apps, telepítés és az automatizálás sablonok konfigurálása és telepítése tooany környezetben.</span><span class="sxs-lookup"><span data-stu-id="d09fe-105">Visual Studio provides rich tools like hello Logic App Designer for you toocreate logic apps, configure deployment and automation templates, and deploy tooany environment.</span></span> 

<span data-ttu-id="d09fe-106">További lépések az Azure Logic Apps tooget [hogyan toocreate hello Azure-portálon első logika alkalmazását](logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="d09fe-106">tooget started with Azure Logic Apps, learn [how toocreate your first logic app in hello Azure portal](logic-apps-create-a-logic-app.md).</span></span>

## <a name="installation-steps"></a><span data-ttu-id="d09fe-107">Telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="d09fe-107">Installation steps</span></span>

<span data-ttu-id="d09fe-108">tooinstall és a Visual Studio eszközök konfigurálása az Azure Logic Apps, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d09fe-108">tooinstall and configure Visual Studio tools for Azure Logic Apps, follow these steps.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d09fe-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d09fe-109">Prerequisites</span></span>

* <span data-ttu-id="d09fe-110">[A Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) vagy a Visual Studio 2015-höz</span><span class="sxs-lookup"><span data-stu-id="d09fe-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) or Visual Studio 2015</span></span>
* <span data-ttu-id="d09fe-111">[Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="d09fe-111">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="d09fe-112">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d09fe-112">Azure PowerShell</span></span>](https://github.com/Azure/azure-powershell#installation)
* <span data-ttu-id="d09fe-113">Hozzáférés toohello webes hello beágyazott designer használata esetén</span><span class="sxs-lookup"><span data-stu-id="d09fe-113">Access toohello web when using hello embedded designer</span></span>

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a><span data-ttu-id="d09fe-114">Telepítheti a Visual Studio tools for Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d09fe-114">Install Visual Studio tools for Azure Logic Apps</span></span>

<span data-ttu-id="d09fe-115">Miután telepítette a hello Előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="d09fe-115">After you install hello prerequisites:</span></span>

1. <span data-ttu-id="d09fe-116">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="d09fe-116">Open Visual Studio.</span></span> <span data-ttu-id="d09fe-117">A hello **eszközök** menü **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-117">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="d09fe-118">Bontsa ki a hello **Online** kategória online kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="d09fe-118">Expand hello **Online** category so you can search online.</span></span>
3. <span data-ttu-id="d09fe-119">Keresse meg **Logic Apps** amíg meg nem látja **Azure Logic Apps Tools for Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-119">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="d09fe-120">toodownload és a telepítés hello kiterjesztéssel, kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-120">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="d09fe-121">Indítsa újra a Visual Studio telepítése után.</span><span class="sxs-lookup"><span data-stu-id="d09fe-121">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="d09fe-122">Emellett letöltheti [Azure Logic Apps-eszközök Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) és hello [Azure Logic Apps eszközök a Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) hello Visual Studio Marketplace-ről.</span><span class="sxs-lookup"><span data-stu-id="d09fe-122">You can also download [Azure Logic Apps Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) and hello [Azure Logic Apps Tools for Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) directly from hello Visual Studio Marketplace.</span></span>

<span data-ttu-id="d09fe-123">A telepítés befejezése után a hello Azure erőforráscsoport-projekt Logic App-tervezővel is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d09fe-123">After you finish installation, you can use hello Azure Resource Group project with Logic App Designer.</span></span>

## <a name="create-your-project"></a><span data-ttu-id="d09fe-124">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d09fe-124">Create your project</span></span>

1. <span data-ttu-id="d09fe-125">A hello **fájl** menü Ugrás túl**új**, és válassza ki **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-125">On hello **File** menu, go too**New**, and select **Project**.</span></span> <span data-ttu-id="d09fe-126">Vagy tooadd túl meglévő megoldás, nyissa meg a projekt-tooan**Hozzáadás**, és válassza ki **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-126">Or tooadd your project tooan existing solution, go too**Add**, and select **New Project**.</span></span>

    ![Fájl menü](./media/logic-apps-deploy-from-vs/filemenu.png)

2. <span data-ttu-id="d09fe-128">A hello **új projekt** ablakban található **felhő**, és válassza ki **Azure erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-128">In hello **New Project** window, find **Cloud**, and select **Azure Resource Group**.</span></span> <span data-ttu-id="d09fe-129">Nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-129">Name your project, and click **OK**.</span></span>

    ![Új projekt hozzáadása](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. <span data-ttu-id="d09fe-131">Jelölje be hello **logikai alkalmazás** sablon, amely létrehozza a központi telepítési sablont üres logic app, toouse.</span><span class="sxs-lookup"><span data-stu-id="d09fe-131">Select hello **Logic App** template, which creates a blank logic app deployment template for you toouse.</span></span> <span data-ttu-id="d09fe-132">A sablon megadása után kattintson az **OK**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-132">After you select your template, click **OK**.</span></span>

    ![Válassza ki a Logic App-sablon](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    <span data-ttu-id="d09fe-134">Most már felvett a logic app projektet tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="d09fe-134">You've now added your logic app project tooyour solution.</span></span> 
    <span data-ttu-id="d09fe-135">A Solution Explorer hello a telepítési fájlt meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="d09fe-135">In hello Solution Explorer, your deployment file should appear.</span></span>

    ![Telepítési fájl](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a><span data-ttu-id="d09fe-137">A Logic App tervezővel a logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d09fe-137">Create your logic app with Logic App Designer</span></span>

<span data-ttu-id="d09fe-138">Ha egy Azure erőforráscsoport-projekt, amely tartalmazza a logic app, megnyithatja hello Logic App Designer a Visual Studio toocreate a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="d09fe-138">When you have an Azure Resource Group project that contains a logic app, you can open hello Logic App Designer in Visual Studio toocreate your workflow.</span></span> 

> [!NOTE]
> <span data-ttu-id="d09fe-139">hello Tervező szükséges az internetkapcsolat túl összekötők rendelkezésre álló tulajdonságok és az adatok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="d09fe-139">hello designer requires an internet connection too query connectors for available properties and data.</span></span> <span data-ttu-id="d09fe-140">Például ha hello Dynamics CRM Online-összekötő használata esetén hello designer lekérdezi a CRM példány tooshow rendelkezésre álló egyéni és az alapértelmezett tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="d09fe-140">For example, if you use hello Dynamics CRM Online connector, hello designer queries your CRM instance tooshow available custom and default properties.</span></span>

1. <span data-ttu-id="d09fe-141">Kattintson a jobb gombbal a `<template>.json` fájlt, és válassza ki **nyissa meg a Logic App tervezővel**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-141">Right-click your `<template>.json` file, and select **Open with Logic App Designer**.</span></span> <span data-ttu-id="d09fe-142">(`Ctrl+L`)</span><span class="sxs-lookup"><span data-stu-id="d09fe-142">(`Ctrl+L`)</span></span>

2. <span data-ttu-id="d09fe-143">Válassza ki az Azure-előfizetés, erőforráscsoportot és helyet a központi telepítési sablon.</span><span class="sxs-lookup"><span data-stu-id="d09fe-143">Choose your Azure subscription, resource group, and location for your deployment template.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d09fe-144">Logikai alkalmazás kialakítása API-kapcsolat erőforrások azokhoz a tulajdonságokhoz lekérdezés során létrehoz Tervező.</span><span class="sxs-lookup"><span data-stu-id="d09fe-144">Designing a logic app creates API Connection resources that query for properties during design.</span></span> <span data-ttu-id="d09fe-145">A Visual Studio ezeket a kapcsolatokat használ a kiválasztott erőforrás csoport toocreate tervezés során.</span><span class="sxs-lookup"><span data-stu-id="d09fe-145">Visual Studio uses your selected resource group toocreate those connections during design time.</span></span> <span data-ttu-id="d09fe-146">tooview vagy módosítsa a API-kapcsolatokat nyissa meg toohello Azure-portálon, és tallózással keresse meg **API kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-146">tooview or change any API Connections, go toohello Azure portal, and browse for **API Connections**.</span></span>

    ![Előfizetés kiválasztása](./media/logic-apps-deploy-from-vs/designer_picker.png)

    <span data-ttu-id="d09fe-148">hello designer hello definition használja a hello `<template>.json` fájl a megjelenítéshez.</span><span class="sxs-lookup"><span data-stu-id="d09fe-148">hello designer uses hello definition in hello `<template>.json` file for rendering.</span></span>

4. <span data-ttu-id="d09fe-149">Hozzon létre, és a logikai alkalmazás kialakítása.</span><span class="sxs-lookup"><span data-stu-id="d09fe-149">Create and design your logic app.</span></span> <span data-ttu-id="d09fe-150">A módosításokat a központi telepítési sablont frissül.</span><span class="sxs-lookup"><span data-stu-id="d09fe-150">Your deployment template is updated with your changes.</span></span>

    ![A Visual Studio programot Tervező](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

<span data-ttu-id="d09fe-152">A Visual Studio hozzáadja `Microsoft.Web/connections` erőforrások túl az erőforrás fájlt a kapcsolatokat a Logic Apps alkalmazást kell toofunction.</span><span class="sxs-lookup"><span data-stu-id="d09fe-152">Visual Studio adds `Microsoft.Web/connections` resources too your resource file for any connections your logic app needs toofunction.</span></span> <span data-ttu-id="d09fe-153">A kapcsolati tulajdonságok kell telepítésekor, és állítsa a telepítése után felügyelt **API kapcsolatok** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d09fe-153">These connection properties can be set when you deploy, and managed after you deploy in **API Connections** in hello Azure portal.</span></span>

### <a name="switch-toojson-code-view"></a><span data-ttu-id="d09fe-154">TooJSON kód nézetre váltani</span><span class="sxs-lookup"><span data-stu-id="d09fe-154">Switch tooJSON code view</span></span>

<span data-ttu-id="d09fe-155">a Logic Apps alkalmazást, jelölje be hello JSON-megjelenítés tooshow hello **kódnézetben** hello designer hello alján fülre.</span><span class="sxs-lookup"><span data-stu-id="d09fe-155">tooshow hello JSON representation for your logic app, select hello **Code View** tab at hello bottom of hello designer.</span></span>

<span data-ttu-id="d09fe-156">tooswitch toohello teljes körű erőforrást JSON biztonsági, kattintson a jobb gombbal a hello `<template>.json` fájlt, és válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-156">tooswitch back toohello full resource JSON, right-click hello `<template>.json` file, and select **Open**.</span></span>

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a><span data-ttu-id="d09fe-157">Mutató hivatkozásokat adhat a tőle függő erőforrások tooVisual Studio központi telepítési sablonok</span><span class="sxs-lookup"><span data-stu-id="d09fe-157">Add references for dependent resources tooVisual Studio deployment templates</span></span>

<span data-ttu-id="d09fe-158">Ha azt szeretné, hogy a logic app tooreference tőle függő erőforrások, [Azure Resource Manager sablonfüggvényei](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) logic app központi telepítési sablonba.</span><span class="sxs-lookup"><span data-stu-id="d09fe-158">When you want your logic app tooreference dependent resources, you can use [Azure Resource Manager template functions](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) in your logic app deployment template.</span></span> <span data-ttu-id="d09fe-159">Például érdemes a logic app tooreference egy Azure-funkció vagy integrációs kívánt fiók toodeploy mellett a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d09fe-159">For example, you might want your logic app tooreference an Azure Function or integration account that you want toodeploy alongside your logic app.</span></span> <span data-ttu-id="d09fe-160">Az alábbiakra kapcsolatos hogyan toouse paramétereit a központi telepítési sablont úgy, hogy a Logic App Designer hello megfelelően képezi le.</span><span class="sxs-lookup"><span data-stu-id="d09fe-160">Follow these guidelines about how toouse parameters in your deployment template so that hello Logic App Designer renders correctly.</span></span> 

<span data-ttu-id="d09fe-161">Eseményindítók és műveletek az ilyen típusú logic app paramétereket használhatja:</span><span class="sxs-lookup"><span data-stu-id="d09fe-161">You can use logic app parameters in these kinds of triggers and actions:</span></span>

*   <span data-ttu-id="d09fe-162">Alárendelt munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="d09fe-162">Child workflow</span></span>
*   <span data-ttu-id="d09fe-163">Függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="d09fe-163">Function app</span></span>
*   <span data-ttu-id="d09fe-164">APIM hívás</span><span class="sxs-lookup"><span data-stu-id="d09fe-164">APIM call</span></span>
*   <span data-ttu-id="d09fe-165">API kapcsolat futásidejű URL-címe</span><span class="sxs-lookup"><span data-stu-id="d09fe-165">API connection runtime URL</span></span>
*   <span data-ttu-id="d09fe-166">API-kapcsolati útvonal</span><span class="sxs-lookup"><span data-stu-id="d09fe-166">API connection path</span></span>

<span data-ttu-id="d09fe-167">És használhatja a sablont funkciókat, például a paraméterek, változókat, resourceId, concat, stb. Például ez hogyan lecserélheti hello Azure-függvény erőforrás-azonosító:</span><span class="sxs-lookup"><span data-stu-id="d09fe-167">And you can use template functions such as parameters, variables, resourceId, concat, etc. For example, here's how you can replace hello Azure Function resource ID:</span></span>

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

<span data-ttu-id="d09fe-168">És ahol paraméterek szeretné használni:</span><span class="sxs-lookup"><span data-stu-id="d09fe-168">And where you would use parameters:</span></span>

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
<span data-ttu-id="d09fe-169">Egy másik példa Service Bus küldési üzenet művelethez hello is parametrizálja:</span><span class="sxs-lookup"><span data-stu-id="d09fe-169">As another example you can parameterize hello Service Bus send message operation:</span></span>

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
> <span data-ttu-id="d09fe-170">host.runtimeUrl nem kötelező, és eltávolíthatja a sablon ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="d09fe-170">host.runtimeUrl is optional and can be removed from your template if present.</span></span>
> 


> [!NOTE] 
> <span data-ttu-id="d09fe-171">Hello Logic App Designer toowork paraméterek, használatakor meg kell adnia az alapértelmezett értékeket, például:</span><span class="sxs-lookup"><span data-stu-id="d09fe-171">For hello Logic App Designer toowork when you use parameters, you must provide default values, for example:</span></span>
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

### <a name="save-your-logic-app"></a><span data-ttu-id="d09fe-172">A logikai alkalmazás mentése</span><span class="sxs-lookup"><span data-stu-id="d09fe-172">Save your logic app</span></span>

<span data-ttu-id="d09fe-173">toosave a Logic Apps alkalmazást bármikor, nyissa meg túl**fájl** > **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-173">toosave your logic app at anytime, go too**File** > **Save**.</span></span> <span data-ttu-id="d09fe-174">(`Ctrl+S`)</span><span class="sxs-lookup"><span data-stu-id="d09fe-174">(`Ctrl+S`)</span></span> 

<span data-ttu-id="d09fe-175">Ha a Logic Apps alkalmazást hibák az alkalmazás mentésekor, megjelennek a Visual Studio hello **kimenetek** ablak.</span><span class="sxs-lookup"><span data-stu-id="d09fe-175">If your logic app has any errors when you save your app, they appear in hello Visual Studio **Outputs** window.</span></span>

## <a name="deploy-your-logic-app-from-visual-studio"></a><span data-ttu-id="d09fe-176">A Logic Apps alkalmazást a Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="d09fe-176">Deploy your logic app from Visual Studio</span></span>

<span data-ttu-id="d09fe-177">Az alkalmazás konfigurálását, telepítheti közvetlenül a Visual Studio csak néhány lépésben.</span><span class="sxs-lookup"><span data-stu-id="d09fe-177">After configuring your app, you can deploy directly from Visual Studio in just a couple steps.</span></span> 

1. <span data-ttu-id="d09fe-178">A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a túl**telepítés** > **új központi telepítési...**</span><span class="sxs-lookup"><span data-stu-id="d09fe-178">In Solution Explorer, right-click your project, and go too**Deploy** > **New Deployment...**</span></span>

    ![Új központi telepítés](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. <span data-ttu-id="d09fe-180">Amikor a rendszer kéri, jelentkezzen be Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="d09fe-180">When you're prompted, sign in tooyour Azure subscription.</span></span> 

3. <span data-ttu-id="d09fe-181">Most már ki kell választania, ahová toodeploy a Logic Apps alkalmazást hello erőforráscsoport hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="d09fe-181">Now you must select hello details for hello resource group where you want toodeploy your logic app.</span></span> <span data-ttu-id="d09fe-182">Amikor elkészült, válassza ki a **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-182">When you're done, select **Deploy**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d09fe-183">Győződjön meg arról, hogy ki kell választania hello megfelelő sablon és paraméterfájl hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="d09fe-183">Make sure that you select hello correct template and parameters file for hello resource group.</span></span> <span data-ttu-id="d09fe-184">Ha azt szeretné, hogy toodeploy tooa éles környezetben, például válassza a hello éles paraméterek fájl.</span><span class="sxs-lookup"><span data-stu-id="d09fe-184">For example, if you want toodeploy tooa production environment, choose hello production parameters file.</span></span>

    ![Tooresource csoport központi telepítése](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    <span data-ttu-id="d09fe-186">hello központi telepítési állapot jelenik meg hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="d09fe-186">hello deployment status appears in hello **Output** window.</span></span> 
    <span data-ttu-id="d09fe-187">Előfordulhat, hogy tooselect **Azure kiépítés** a hello **megjelenítése kimenetét** lista.</span><span class="sxs-lookup"><span data-stu-id="d09fe-187">You might have tooselect **Azure Provisioning** in hello **Show output from** list.</span></span>

    ![Központi telepítési állapot kimeneti](./media/logic-apps-deploy-from-vs/output.png)

<span data-ttu-id="d09fe-189">Jövőbeli hello szerkessze a Logic Apps alkalmazást a verziókövetési rendszerrel, és használja a Visual Studio toodeploy új verziók.</span><span class="sxs-lookup"><span data-stu-id="d09fe-189">In hello future, you can edit your logic app in source control, and use Visual Studio toodeploy new versions.</span></span>

> [!NOTE]
> <span data-ttu-id="d09fe-190">Közvetlenül hello definíciójában hello Azure-portálon módosítja, ezeket a módosításokat a rendszer felülírja központi telepítésekor a Visual Studio eszközből legközelebb.</span><span class="sxs-lookup"><span data-stu-id="d09fe-190">If you change hello definition in hello Azure portal directly, those changes are overwritten when you deploy from Visual Studio next time.</span></span> 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a><span data-ttu-id="d09fe-191">Adja hozzá a logic app tooan meglévő erőforráscsoport-projekt</span><span class="sxs-lookup"><span data-stu-id="d09fe-191">Add your logic app tooan existing Resource Group project</span></span>

<span data-ttu-id="d09fe-192">Ha egy meglévő erőforráscsoport-projekt, adhat hozzá a logic app toothat projekt hello JSON-vázlat ablak.</span><span class="sxs-lookup"><span data-stu-id="d09fe-192">If you have an existing Resource Group project, you can add your logic app toothat project in hello JSON Outline window.</span></span> <span data-ttu-id="d09fe-193">Egy másik logikai alkalmazást a korábban létrehozott hello alkalmazás mellett azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="d09fe-193">You can also add another logic app alongside hello app you previously created.</span></span>

1. <span data-ttu-id="d09fe-194">Nyissa meg hello `<template>.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d09fe-194">Open hello `<template>.json` file.</span></span>

2. <span data-ttu-id="d09fe-195">tooopen hello JSON-vázlat ablak, nyissa meg túl**nézet** > **más Windows** > **JSON-vázlat**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-195">tooopen hello JSON Outline window, go too**View** > **Other Windows** > **JSON Outline**.</span></span>

3. <span data-ttu-id="d09fe-196">egy erőforrás toohello sablonfájl tooadd kattintson **erőforrás hozzáadása** el hello hello JSON-vázlat ablak tetején.</span><span class="sxs-lookup"><span data-stu-id="d09fe-196">tooadd a resource toohello template file, click **Add Resource** at hello top of hello JSON Outline window.</span></span> <span data-ttu-id="d09fe-197">Vagy hello JSON-vázlat ablak, kattintson a jobb gombbal **erőforrások**, és válassza ki **új erőforrás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-197">Or in hello JSON Outline window, right-click **resources**, and select **Add New Resource**.</span></span>

    ![JSON-vázlat ablak](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. <span data-ttu-id="d09fe-199">A hello **erőforrás hozzáadása** párbeszédpanel, keresése és kijelölése **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-199">In hello **Add Resource** dialog box, find and select **Logic App**.</span></span> <span data-ttu-id="d09fe-200">A logikai alkalmazás neve, és válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d09fe-200">Name your logic app, and choose **Add**.</span></span>

    ![Erőforrás hozzáadása](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a><span data-ttu-id="d09fe-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d09fe-202">Next Steps</span></span>

* [<span data-ttu-id="d09fe-203">A Visual Studio Cloud Explorer logic Apps-alkalmazások kezelése</span><span class="sxs-lookup"><span data-stu-id="d09fe-203">Manage logic apps with Visual Studio Cloud Explorer</span></span>](logic-apps-manage-from-vs.md)
* [<span data-ttu-id="d09fe-204">Gyakori példák és felhasználási helyzetek megtekintése</span><span class="sxs-lookup"><span data-stu-id="d09fe-204">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="d09fe-205">Ismerje meg, hogyan tooautomate üzleti dolgozza fel az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d09fe-205">Learn how tooautomate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="d09fe-206">Megtudhatja, hogyan toointegrate a rendszer az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="d09fe-206">Learn how toointegrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
