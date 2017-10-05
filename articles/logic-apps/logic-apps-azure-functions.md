---
title: "Az Azure Logic Apps az Azure Functions egyéni kód |} Microsoft Docs"
description: "Hozzon létre és futtasson egyéni kód az Azure Logic Apps az Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18442c87b049200fac5ed41cc7034ba7a848b8d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="aca85-103">Adja hozzá, és egyéni kódot a logic Apps alkalmazásokat futtasson Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="aca85-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="aca85-104">C# vagy node.js egyéni kódtöredékek futtatásához a logic apps, az Azure Functions egyéni funkciókhoz is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="aca85-104">To run custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="aca85-105">[Az Azure Functions](../azure-functions/functions-overview.md) kínál a kiszolgáló-mentes számítógép-használatról a Microsoft Azure és a feladatok végrehajtásához használt hasznos:</span><span class="sxs-lookup"><span data-stu-id="aca85-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="aca85-106">Speciális formázás vagy számítási a logic apps mezők</span><span class="sxs-lookup"><span data-stu-id="aca85-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="aca85-107">A munkafolyamat számításokat.</span><span class="sxs-lookup"><span data-stu-id="aca85-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="aca85-108">Bővíthetők a logic app C# vagy node.js által támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="aca85-108">Extend the logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="aca85-109">A logic Apps alkalmazásokat a felhasználói függvények létrehozása</span><span class="sxs-lookup"><span data-stu-id="aca85-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="aca85-110">Azt javasoljuk, hogy az Azure Functions portálon függvény létrehozása a **általános Webhook - csomópont** vagy **általános Webhook - C#** sablonok.</span><span class="sxs-lookup"><span data-stu-id="aca85-110">We recommend that you create a function in the Azure Functions portal, from the **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="aca85-111">Az eredmény létrehoz egy automatikus feltöltve a sablont, amely elfogadja `application/json` a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="aca85-111">The result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="aca85-112">Függvények, amelyek ezeket a sablonokat hoz létre a rendszer automatikusan észleli és jelennek meg a Logic App Designer alatt **Azure Functions a régióban.**</span><span class="sxs-lookup"><span data-stu-id="aca85-112">Functions that you create from these templates are automatically detected and appear in the Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="aca85-113">Az Azure portálon a a **integráció** ablaktábla a függvénynél, a sablon jelenítsen meg, amely **mód** beállítása **Webhook** és **Webhook típus**értéke **általános JSON**.</span><span class="sxs-lookup"><span data-stu-id="aca85-113">In the Azure portal, on the **Integrate** pane for your function, your template should show that **Mode** set to **Webhook** and **Webhook type** is set to **Generic JSON**.</span></span> 

<span data-ttu-id="aca85-114">Webhook funkciók fogadja el a kérelmet, és adja át az keresztül metódusnak egy `data` változó.</span><span class="sxs-lookup"><span data-stu-id="aca85-114">Webhook functions accept a request and pass it into the method via a `data` variable.</span></span> <span data-ttu-id="aca85-115">Érhető el a tartalom tulajdonságait pontjelöléssel például `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="aca85-115">You can access the properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="aca85-116">Például egy egyszerű JavaScript függvény DateTime típusú értékké alakít át egy dátumkarakterlánc néz ki a következő példa:</span><span class="sxs-lookup"><span data-stu-id="aca85-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like the following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="aca85-117">Az Azure Functions hívható meg a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="aca85-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="aca85-118">Az előfizetésében szereplő tárolókhoz listában, és válassza ki a függvényt, amely felhívja a Logic App tervezőben, kattintson a **műveletek** menüt, és válassza ki a **Azure Functions régiómban**.</span><span class="sxs-lookup"><span data-stu-id="aca85-118">To list the containers in your subscription and select the function that you want to call, in Logic App Designer, click the **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="aca85-119">Miután kiválasztotta a funkciót, a rendszer felkéri bemeneti forgalma objektum.</span><span class="sxs-lookup"><span data-stu-id="aca85-119">After you select the function, you are asked to specify an input payload object.</span></span> <span data-ttu-id="aca85-120">Ez az objektum az az üzenet, hogy a logikai alkalmazást a függvény küld, és JSON-objektumnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aca85-120">This object is the message that the logic app sends to the function and must be a JSON object.</span></span> <span data-ttu-id="aca85-121">Például, ha a átadni kívánt a **utolsó módosítás** dátuma a Salesforce-eseményindítóval, a függvény forgalma nézhet ki ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="aca85-121">For example, if you want to pass in the **Last Modified** date from a Salesforce trigger, the function payload might look like this example:</span></span>

![Utolsó módosítás dátuma][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="aca85-123">Egy függvény eseményindító logikai alkalmazások</span><span class="sxs-lookup"><span data-stu-id="aca85-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="aca85-124">Indíthat el egy függvényen belül egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aca85-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="aca85-125">Lásd: [a Logic apps hívható végpontként](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="aca85-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="aca85-126">Kézi indítási rendelkező logikai alkalmazás létrehozása, majd a a függvényen belül készítése egy HTTP POST URL-cím elé a tartalom kívánt küldi a logikai alkalmazás manuális eseményindító.</span><span class="sxs-lookup"><span data-stu-id="aca85-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST to the manual trigger URL with the payload that you want sent to the logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="aca85-127">A Logic App Designer függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="aca85-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="aca85-128">A Tervező is létrehozhat egy node.js webhook függvény.</span><span class="sxs-lookup"><span data-stu-id="aca85-128">You can also create a node.js webhook function from the designer.</span></span> <span data-ttu-id="aca85-129">Első lépésként válassza ki **régiómban, az Azure Functions** , és válassza ki a függvény tárolója.</span><span class="sxs-lookup"><span data-stu-id="aca85-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="aca85-130">Ha még nem rendelkezik a tárolóhoz, szeretné-e létrehozhat egy a [Azure Functions portálon](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="aca85-130">If you don't yet have a container, you need to create one from the [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="aca85-131">Válassza ki **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="aca85-131">Then select **Create New**.</span></span>  

<span data-ttu-id="aca85-132">Egy sablon számítási kívánt adatok alapján történő létrehozásához adja meg a környezeti objektumot, amely azt tervezi, hogy egy függvény továbbítja.</span><span class="sxs-lookup"><span data-stu-id="aca85-132">To generate a template based on the data that you want to compute, specify the context object that you plan to pass into a function.</span></span> <span data-ttu-id="aca85-133">Ez az objektum JSON objektumnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="aca85-133">This object must be a JSON object.</span></span> <span data-ttu-id="aca85-134">Például ha egy FTP-művelet a teljesíti a fájl tartalma, a környezet forgalma a következőképpen néz ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="aca85-134">For example, if you pass in the file content from an FTP action, the context payload looks like this example:</span></span>

![A környezetben hasznos][2]

> [!NOTE]
> <span data-ttu-id="aca85-136">Ez az objektum nem alakítható karakterláncként, mert a tartalmat közvetlenül a JSON-adattartalmat kerül.</span><span class="sxs-lookup"><span data-stu-id="aca85-136">Because this object wasn't cast as a string, the content is added directly to the JSON payload.</span></span> <span data-ttu-id="aca85-137">Azonban a hiba akkor jelentkezik, ha az objektum nincs a JSON-jogkivonat (Ez azt jelenti, hogy egy karakterlánc- vagy JSON objektum vagy tömb).</span><span class="sxs-lookup"><span data-stu-id="aca85-137">However, an error occurs if the object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="aca85-138">Konvertálja az objektumot egy karakterlánc, vegye fel a idézőjelek között, ebben a cikkben az első ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="aca85-138">To cast the object as a string, add quotes as shown in the first illustration in this article.</span></span>
> 

<span data-ttu-id="aca85-139">A Tervező akkor is létrehozhat beágyazott függvény sablont hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aca85-139">The designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="aca85-140">Változók előre az környezetet, amelyre a függvény át szeretne alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="aca85-140">Variables are pre-created based on the context that you plan to pass into the function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
