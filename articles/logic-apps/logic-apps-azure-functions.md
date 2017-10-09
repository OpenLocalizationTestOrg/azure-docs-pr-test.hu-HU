---
title: "az Azure Functions Azure Logic Apps aaaCustom kódját |} Microsoft Docs"
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
ms.openlocfilehash: 18b3821ed7e434feb568b9b96e9a5a2189dba3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a><span data-ttu-id="981a8-103">Adja hozzá, és egyéni kódot a logic Apps alkalmazásokat futtasson Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="981a8-103">Add and run custom code for logic apps through Azure Functions</span></span>

<span data-ttu-id="981a8-104">toorun egyéni kódtöredékek C# vagy node.js a logic apps, az Azure Functions egyéni funkciókhoz is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="981a8-104">toorun custom snippets of C# or node.js in logic apps, you can create custom functions through Azure Functions.</span></span> 
<span data-ttu-id="981a8-105">[Az Azure Functions](../azure-functions/functions-overview.md) kínál a kiszolgáló-mentes számítógép-használatról a Microsoft Azure és a feladatok végrehajtásához használt hasznos:</span><span class="sxs-lookup"><span data-stu-id="981a8-105">[Azure Functions](../azure-functions/functions-overview.md) offers server-free computing in Microsoft Azure and are useful for performing these tasks:</span></span>

* <span data-ttu-id="981a8-106">Speciális formázás vagy számítási a logic apps mezők</span><span class="sxs-lookup"><span data-stu-id="981a8-106">Advanced formatting or compute of fields in logic apps</span></span>
* <span data-ttu-id="981a8-107">A munkafolyamat számításokat.</span><span class="sxs-lookup"><span data-stu-id="981a8-107">Perform calculations in a workflow.</span></span>
* <span data-ttu-id="981a8-108">Bővíthetők hello logic app C# vagy node.js által támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="981a8-108">Extend hello logic app functionality with functions that are supported in C# or node.js</span></span>

## <a name="create-custom-functions-for-your-logic-apps"></a><span data-ttu-id="981a8-109">A logic Apps alkalmazásokat a felhasználói függvények létrehozása</span><span class="sxs-lookup"><span data-stu-id="981a8-109">Create custom functions for your logic apps</span></span>

<span data-ttu-id="981a8-110">Azt javasoljuk, hogy hozzon létre egy olyan függvényt hello Azure Functions portálon, hello **általános Webhook - csomópont** vagy **általános Webhook - C#** sablonok.</span><span class="sxs-lookup"><span data-stu-id="981a8-110">We recommend that you create a function in hello Azure Functions portal, from hello **Generic Webhook - Node** or **Generic Webhook - C#** templates.</span></span> <span data-ttu-id="981a8-111">hello eredmény létrehoz egy automatikus feltöltve a sablont, amely elfogadja `application/json` a logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="981a8-111">hello result creates an auto-populated a template that accepts `application/json` from a logic app.</span></span> <span data-ttu-id="981a8-112">Függvények, amelyek ezeket a sablonokat hoz létre a rendszer automatikusan észleli és hello jelennek meg a Logic App Designer **Azure Functions a régióban.**</span><span class="sxs-lookup"><span data-stu-id="981a8-112">Functions that you create from these templates are automatically detected and appear in hello Logic App Designer under **Azure Functions in my region.**</span></span>

<span data-ttu-id="981a8-113">Az Azure portál, a hello hello **integráció** ablaktábla a függvénynél, a sablon jelenítsen meg, amely **mód** túl beállítása**Webhook** és **Webhook típus** értéke túl**általános JSON**.</span><span class="sxs-lookup"><span data-stu-id="981a8-113">In hello Azure portal, on hello **Integrate** pane for your function, your template should show that **Mode** set too**Webhook** and **Webhook type** is set too**Generic JSON**.</span></span> 

<span data-ttu-id="981a8-114">Webhook funkciók fogadja el a kérelmet, és adja át az keresztül hello metódusnak egy `data` változó.</span><span class="sxs-lookup"><span data-stu-id="981a8-114">Webhook functions accept a request and pass it into hello method via a `data` variable.</span></span> <span data-ttu-id="981a8-115">Érhető el a tartalom hello tulajdonságok pontjelöléssel például `data.function-name`.</span><span class="sxs-lookup"><span data-stu-id="981a8-115">You can access hello properties of your payload by using dot notation like `data.function-name`.</span></span> <span data-ttu-id="981a8-116">Például egy egyszerű JavaScript függvény DateTime típusú értékké alakít át egy dátumkarakterlánc néz ki a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="981a8-116">For example, a simple JavaScript function that converts a DateTime value into a date string looks like hello following example:</span></span>

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a><span data-ttu-id="981a8-117">Az Azure Functions hívható meg a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="981a8-117">Call Azure Functions from logic apps</span></span>

<span data-ttu-id="981a8-118">toolist hello tárolókat az előfizetését és select hello függvény, amelyet az toocall Logic App tervezőben kattintson hello **műveletek** menüt, és válassza ki a **Azure Functions régiómban**.</span><span class="sxs-lookup"><span data-stu-id="981a8-118">toolist hello containers in your subscription and select hello function that you want toocall, in Logic App Designer, click hello **Actions** menu, and select from **Azure Functions in my Region**.</span></span>

<span data-ttu-id="981a8-119">Hello függvény kiválasztása után áll ismételt toospecify bemeneti forgalma objektum.</span><span class="sxs-lookup"><span data-stu-id="981a8-119">After you select hello function, you are asked toospecify an input payload object.</span></span> <span data-ttu-id="981a8-120">Ez az objektum üdvözlőüzenetére hello logic app küld toohello függvény, és JSON-objektumnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="981a8-120">This object is hello message that hello logic app sends toohello function and must be a JSON object.</span></span> <span data-ttu-id="981a8-121">Például, ha azt szeretné, hogy a hello toopass **utolsó módosítás** dátuma a Salesforce-eseményindítóval hello függvény hasznos nézhet ki ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="981a8-121">For example, if you want toopass in hello **Last Modified** date from a Salesforce trigger, hello function payload might look like this example:</span></span>

![Utolsó módosítás dátuma][1]

## <a name="trigger-logic-apps-from-a-function"></a><span data-ttu-id="981a8-123">Egy függvény eseményindító logikai alkalmazások</span><span class="sxs-lookup"><span data-stu-id="981a8-123">Trigger logic apps from a function</span></span>

<span data-ttu-id="981a8-124">Indíthat el egy függvényen belül egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="981a8-124">You can trigger a logic app from inside a function.</span></span> <span data-ttu-id="981a8-125">Lásd: [a Logic apps hívható végpontként](logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="981a8-125">See [Logic apps as callable endpoints](logic-apps-http-endpoint.md).</span></span> <span data-ttu-id="981a8-126">Kézi indítási rendelkező logikai alkalmazás létrehozása, majd a a függvényen belül készítése egy HTTP POST toohello kézi indítási URL-CÍMÉT a használni kívánt toohello logikai alkalmazás küldött hello tartalom.</span><span class="sxs-lookup"><span data-stu-id="981a8-126">Create a logic app that has a manual trigger, then from inside your function, generate an HTTP POST toohello manual trigger URL with hello payload that you want sent toohello logic app.</span></span>

### <a name="create-a-function-from-logic-app-designer"></a><span data-ttu-id="981a8-127">A Logic App Designer függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="981a8-127">Create a function from Logic App Designer</span></span>

<span data-ttu-id="981a8-128">A node.js webhook függvény hello designer is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="981a8-128">You can also create a node.js webhook function from hello designer.</span></span> <span data-ttu-id="981a8-129">Első lépésként válassza ki **régiómban, az Azure Functions** , és válassza ki a függvény tárolója.</span><span class="sxs-lookup"><span data-stu-id="981a8-129">First, select **Azure Functions in my Region,** and then choose a container for your function.</span></span> <span data-ttu-id="981a8-130">Ha még nem rendelkezik a tárolóhoz, szükség van-e toocreate a hello [Azure Functions portálon](https://functions.azure.com/signin).</span><span class="sxs-lookup"><span data-stu-id="981a8-130">If you don't yet have a container, you need toocreate one from hello [Azure Functions portal](https://functions.azure.com/signin).</span></span> <span data-ttu-id="981a8-131">Válassza ki **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="981a8-131">Then select **Create New**.</span></span>  

<span data-ttu-id="981a8-132">egy sablon, amelyet az toocompute, hello adatok alapján toogenerate hello környezeti objektumot toopass tervezi egy függvénynek adja meg.</span><span class="sxs-lookup"><span data-stu-id="981a8-132">toogenerate a template based on hello data that you want toocompute, specify hello context object that you plan toopass into a function.</span></span> <span data-ttu-id="981a8-133">Ez az objektum JSON objektumnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="981a8-133">This object must be a JSON object.</span></span> <span data-ttu-id="981a8-134">Például ha egy FTP-művelet a teljesíti a hello fájl tartalma a, hello környezetben hasznos a következőképpen néz ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="981a8-134">For example, if you pass in hello file content from an FTP action, hello context payload looks like this example:</span></span>

![A környezetben hasznos][2]

> [!NOTE]
> <span data-ttu-id="981a8-136">Ez az objektum nem alakítható karakterláncként, mert hello tartalom kerüljön közvetlenül toohello JSON-adattartalmat.</span><span class="sxs-lookup"><span data-stu-id="981a8-136">Because this object wasn't cast as a string, hello content is added directly toohello JSON payload.</span></span> <span data-ttu-id="981a8-137">Hiba történik, ha hello objektum nem egy JSON-jogkivonatot (Ez azt jelenti, hogy egy karakterlánc- vagy JSON objektum vagy tömb).</span><span class="sxs-lookup"><span data-stu-id="981a8-137">However, an error occurs if hello object is not a JSON token (that is, a string or a JSON object/array).</span></span> <span data-ttu-id="981a8-138">toocast hello objektum karakterláncként, adjon hozzá idézőjelek között, ebben a cikkben hello első ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="981a8-138">toocast hello object as a string, add quotes as shown in hello first illustration in this article.</span></span>
> 

<span data-ttu-id="981a8-139">hello designer akkor is létrehozhat beágyazott függvény sablont hoz létre.</span><span class="sxs-lookup"><span data-stu-id="981a8-139">hello designer then generates a function template that you can create inline.</span></span> <span data-ttu-id="981a8-140">Változók előre toopass tervezi hello függvénynek hello környezet alapján hozzák létre.</span><span class="sxs-lookup"><span data-stu-id="981a8-140">Variables are pre-created based on hello context that you plan toopass into hello function.</span></span>

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
