---
title: "aaaAdd feltételeket, és indítsa el az Azure Logic Apps-munkafolyamatok – |} Microsoft Docs"
description: "Szabályozza, hogyan munkafolyamatok futnak, az Azure Logic Apps feltételes logikához, eseményindítók, műveletek és paraméterek hozzáadásával."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 76d5e44590ffa14cf70d7a93b99a241d286d555b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-logic-apps-features"></a><span data-ttu-id="9e9d3-103">Logic Apps funkcióinak használata</span><span class="sxs-lookup"><span data-stu-id="9e9d3-103">Use Logic Apps features</span></span>

<span data-ttu-id="9e9d3-104">Az egy [előző témakörben](../logic-apps/logic-apps-create-a-logic-app.md), az első logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-104">In a [previous topic](../logic-apps/logic-apps-create-a-logic-app.md), you created your first logic app.</span></span> <span data-ttu-id="9e9d3-105">toocontrol a Logic Apps alkalmazást munkafolyamat, adja meg a logic app toorun elérési útja eltér, és hogyan túl fel az adatokat a tömböket, gyűjtemények és kötegek.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-105">toocontrol your logic app's workflow, you can specify different paths for your logic app toorun and how too process data in arrays, collections, and batches.</span></span> <span data-ttu-id="9e9d3-106">Ezeket az elemeket a logic app munkafolyamat alábbiakból állhat:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-106">You can include these elements in your logic app workflow:</span></span>

* <span data-ttu-id="9e9d3-107">Feltételek és [utasítások kapcsoló](../logic-apps/logic-apps-switch-case.md) lehetővé teszik a Logic Apps alkalmazást, hogy meghatározott feltételek alapján más műveletek futtatása.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-107">Conditions and [switch statements](../logic-apps/logic-apps-switch-case.md) let your logic app run different actions based on whether specific conditions are met.</span></span>

* <span data-ttu-id="9e9d3-108">[Hurkok](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé teszik a Logic Apps alkalmazást ismételten lépések futtatásával.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-108">[Loops](../logic-apps/logic-apps-loops-and-scopes.md) let your logic app run steps repeatedly.</span></span> <span data-ttu-id="9e9d3-109">Például megismétlésével műveletek keresztül tömb használatakor egy **For_each** hurok.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-109">For example, you can repeat actions over an array when you use a **For_each** loop.</span></span> <span data-ttu-id="9e9d3-110">Vagy a művelet megismétlésével egy feltétel teljesüléséig, használatakor egy **amíg** hurok.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-110">Or you can repeat actions until a condition is met when you use an **Until** loop.</span></span>

* <span data-ttu-id="9e9d3-111">[Hatókörök](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé csoportosítása műveletsorozattal együtt, például tooimplement kivételkezelést.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-111">[Scopes](../logic-apps/logic-apps-loops-and-scopes.md) let you group series of actions together, for example, tooimplement exception handling.</span></span>

* <span data-ttu-id="9e9d3-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lehetővé teszi, hogy a logikai alkalmazás indul elemek külön munkafolyamatok tömbben el hello **SplitOn** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-112">[Debatching](../logic-apps/logic-apps-loops-and-scopes.md) lets your logic app start separate workflows for items in an array when you use hello **SplitOn** command.</span></span>

<span data-ttu-id="9e9d3-113">Ez a témakör bemutatja a más fogalmak összeállítani saját logikai alkalmazását:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-113">This topic introduces other concepts for building your logic app:</span></span>

* <span data-ttu-id="9e9d3-114">Egy meglévő logikai alkalmazás nézet tooedit kód</span><span class="sxs-lookup"><span data-stu-id="9e9d3-114">Code view tooedit an existing logic app</span></span>
* <span data-ttu-id="9e9d3-115">Egy munkafolyamat beállítások</span><span class="sxs-lookup"><span data-stu-id="9e9d3-115">Options for starting a workflow</span></span>

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a><span data-ttu-id="9e9d3-116">Feltételek: Lépéseket csak egy feltétel teljesítése után futtassa.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-116">Conditions: Run steps only after meeting a condition</span></span>

<span data-ttu-id="9e9d3-117">a Logic Apps alkalmazást lépések futtatásával csak akkor, amikor adatokat meghatározott feltételeknek, adhat hozzá egy feltételt, amely összehasonlítja az adatok adott mezők vagy értékek hello munkafolyamat toohave.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-117">toohave your logic app run steps only when data meets specific criteria, you can add a condition that compares data in hello workflow against specific fields or values.</span></span>

<span data-ttu-id="9e9d3-118">Tegyük fel például, hogy egy webhely RSS-hírcsatorna a POST túl sok e-mailt küld logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-118">For example, suppose you have a logic app that sends you too many emails for posts on a website's RSS feed.</span></span> <span data-ttu-id="9e9d3-119">Hozzáadhat egy feltétel, hogy a logikai alkalmazás e-mailt küld, csak akkor, ha új hello utáni tartozik tooa adott konkrét kategóriával.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-119">You can add a condition so that your logic app sends email only when hello new post belongs tooa specific category.</span></span>

1. <span data-ttu-id="9e9d3-120">A hello [Azure-portálon](https://portal.azure.com), található, és nyissa meg a Logic Apps alkalmazást a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-120">In hello [Azure portal](https://portal.azure.com), find and open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="9e9d3-121">Adja hozzá a feltétel toohello munkafolyamat helyet használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-121">Add a condition toohello workflow location that you want.</span></span> 

   <span data-ttu-id="9e9d3-122">tooadd hello feltétel hello logic app munkafolyamat, meglévő lépések között helyezze hello egérmutatót hello nyíl kívánt tooadd hello feltétel.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-122">tooadd hello condition between existing steps in hello logic app workflow, move hello pointer over hello arrow where you want tooadd hello condition.</span></span> 
   <span data-ttu-id="9e9d3-123">Válassza ki a hello **pluszjel** (**+**), majd válassza a **feltétel hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-123">Choose hello **plus sign** (**+**), then choose **Add a condition**.</span></span> <span data-ttu-id="9e9d3-124">Példa:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-124">For example:</span></span>

   ![Az állapot toologic alkalmazás hozzáadása](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > <span data-ttu-id="9e9d3-126">Ha egy feltétel tooadd a jelenlegi munkafolyamatot hello végén, nyissa meg a Logic Apps alkalmazást toohello aljára, és válassza ki **+ új lépés**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-126">If you want tooadd a condition at hello end of your current workflow, go toohello bottom of your logic app, and choose **+ New step**.</span></span>

3. <span data-ttu-id="9e9d3-127">Most meghatározhatja hello feltételét.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-127">Now define hello condition.</span></span> <span data-ttu-id="9e9d3-128">Adja meg, amelyet tooevaluate, hello művelet tooperform, és hello célérték vagy mező hello mezőjének.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-128">Specify hello source field that you want tooevaluate, hello operation tooperform, and hello target value or field.</span></span> <span data-ttu-id="9e9d3-129">tooadd meglévő mezők tooyour feltétel hello választhat **hozzáadása a dinamikus tartalom listába**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-129">tooadd existing fields tooyour condition, choose from hello **Add dynamic content list**.</span></span>

   <span data-ttu-id="9e9d3-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-130">For example:</span></span>

   ![Alapszintű módban feltétel szerkesztése](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   <span data-ttu-id="9e9d3-132">Az alábbiakban hello teljes feltétel:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-132">Here's hello complete condition:</span></span>

   ![Teljes feltétel](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > <span data-ttu-id="9e9d3-134">a kódban, toodefine hello feltétel kiválasztása **speciális módban szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-134">toodefine hello condition in code, choose **Edit in advanced mode**.</span></span> <span data-ttu-id="9e9d3-135">Példa:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-135">For example:</span></span>
   > 
   > ![A kód feltétel szerkesztése](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. <span data-ttu-id="9e9d3-137">A **IF Igen** és **IF nem**, adja hozzá a hello lépéseket tooperform alapján hello feltétel teljesülését vizsgálja.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-137">Under **IF YES** and **IF NO**, add hello steps tooperform based on whether hello condition is met.</span></span>

   <span data-ttu-id="9e9d3-138">Példa:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-138">For example:</span></span>

   ![Igen, és nincsenek elérési utak feltételei](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > <span data-ttu-id="9e9d3-140">Meglévő műveletek húzza hello **IF Igen** és **IF nem** elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-140">You can drag existing actions into hello **IF YES** and **IF NO** paths.</span></span>

5. <span data-ttu-id="9e9d3-141">Amikor elkészült, mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-141">When you're done, save your logic app.</span></span>

<span data-ttu-id="9e9d3-142">Most kapott e-mailek csak hello bejegyzéseket felel meg a feltételnek.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-142">Now you get emails only when hello posts meet your condition.</span></span>

## <a name="repeat-actions-over-a-list-with-foreach"></a><span data-ttu-id="9e9d3-143">Ismételje meg a műveletek során a forEach</span><span class="sxs-lookup"><span data-stu-id="9e9d3-143">Repeat actions over a list with forEach</span></span>

<span data-ttu-id="9e9d3-144">hello forEach hurok egy tömb toorepeat művelet keresztül határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-144">hello forEach loop specifies an array toorepeat an action over.</span></span> <span data-ttu-id="9e9d3-145">Ha nem tömb, hello folyamat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-145">If it is not an array, hello flow fails.</span></span> <span data-ttu-id="9e9d3-146">Például action1 üzenetek tömbjét adja kimenetként, amely rendelkezik, és minden üzenetet toosend szeretnénk, ha is felvehet a forEach-utasítás tulajdonságainak hello a művelethez:`forEach : "@action('action1').outputs.messages"`</span><span class="sxs-lookup"><span data-stu-id="9e9d3-146">For example, if you have action1 that outputs an array of messages, and you want toosend each message, you can include this forEach statement in hello properties of your action: `forEach : "@action('action1').outputs.messages"`</span></span>

## <a name="edit-hello-code-definition-for-a-logic-app"></a><span data-ttu-id="9e9d3-147">Hello kód megadása a logikai alkalmazás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="9e9d3-147">Edit hello code definition for a logic app</span></span>

<span data-ttu-id="9e9d3-148">Bár a Logic App Designer hello, hello kódot, amely meghatározza a logikai alkalmazás közvetlenül szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-148">Although you have hello Logic App Designer, you can directly edit hello code that defines a logic app.</span></span>

1. <span data-ttu-id="9e9d3-149">A hello parancssávon válassza **nézet Code**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-149">On hello command bar, choose **Code view**.</span></span>

    <span data-ttu-id="9e9d3-150">A teljes szerkesztő megnyílik, és szerkesztett hello definition jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-150">A full editor opens and shows hello definition you edited.</span></span>

    ![Kód nézetre](media/logic-apps-use-logic-app-features/codeview.png)

    <span data-ttu-id="9e9d3-152">Hello szövegszerkesztőben, másolja és illessze be a tetszőleges számú műveletet hello belül azonos logikai alkalmazás vagy a logic Apps alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-152">In hello text editor, you can copy and paste any number of actions within hello same logic app or between logic apps.</span></span> 
    <span data-ttu-id="9e9d3-153">Is egyszerűen adja hozzá vagy távolítsa el a teljes szakaszok hello definícióból, és másokkal is megoszthat definíciókat.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-153">You can also easily add or remove entire sections from hello definition, and you can also share definitions with others.</span></span>

2. <span data-ttu-id="9e9d3-154">toosave a módosításokat, válassza a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-154">toosave your edits, choose **Save**.</span></span>

## <a name="parameters"></a><span data-ttu-id="9e9d3-155">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="9e9d3-155">Parameters</span></span>

<span data-ttu-id="9e9d3-156">Egyes Logic Apps tulajdonságai csak a kód nézetre, például a paraméterek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-156">Some Logic Apps capabilities are available only in code view, for example, parameters.</span></span> <span data-ttu-id="9e9d3-157">Paraméterek révén könnyen tooreuse értékét a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-157">Parameters make it easy tooreuse values throughout your logic app.</span></span> <span data-ttu-id="9e9d3-158">Például ha egy e-mail címet, amelyet számos műveletet használja, meg kell határozni, hogy e-mail cím paraméterként.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-158">For example, if you have an email address that you want use in several actions, you should define that email address as a parameter.</span></span>

<span data-ttu-id="9e9d3-159">A paraméterekre ideális húzza ki valószínűleg toochange sokkal értékeket.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-159">Parameters are good for pulling out values that you are likely toochange a lot.</span></span> <span data-ttu-id="9e9d3-160">Különösen akkor hasznos, ha különböző környezetekben toooverride paraméterek van szükség.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-160">They are especially useful when you need toooverride parameters in different environments.</span></span> <span data-ttu-id="9e9d3-161">Hogyan toooverride paraméterek környezet alapján: toolearn [logikai alkalmazás definícióiról szerzői](../logic-apps/logic-apps-author-definitions.md) és [REST API-dokumentáció](https://docs.microsoft.com/rest/api/logic).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-161">toolearn how toooverride parameters based on environment, see [Author logic app definitions](../logic-apps/logic-apps-author-definitions.md) and [REST API documentation](https://docs.microsoft.com/rest/api/logic).</span></span>

<span data-ttu-id="9e9d3-162">A példa bemutatja, hogyan tooupdate a meglévő Logic Apps alkalmazást, hogy a paraméter használható hello lekérdezési kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-162">This example shows how tooupdate your existing logic app so that you can use parameters for hello query term.</span></span>

1. <span data-ttu-id="9e9d3-163">A kód nézetre, megkeresi a hello `parameters : {}` objektumot, és adjon hozzá egy `currentFeedUrl` objektum:</span><span class="sxs-lookup"><span data-stu-id="9e9d3-163">In code view, find hello `parameters : {}` object, and add a `currentFeedUrl` object:</span></span>

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. <span data-ttu-id="9e9d3-164">Nyissa meg toohello `When_a_feed-item_is_published` művelet, a keresés hello `queries` szakaszt, és cserélje le hello lekérdezés értékét:`"feedUrl": "#@{parameters('currentFeedUrl')}"`</span><span class="sxs-lookup"><span data-stu-id="9e9d3-164">Go toohello `When_a_feed-item_is_published` action, find hello `queries` section, and replace hello query value with : `"feedUrl": "#@{parameters('currentFeedUrl')}"`</span></span> 

    <span data-ttu-id="9e9d3-165">toojoin két vagy több karakterláncot, használhatja a hello `concat` függvény.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-165">toojoin two or more strings, you can also use hello `concat` function.</span></span> 
    <span data-ttu-id="9e9d3-166">Például `"@concat('#',parameters('currentFeedUrl'))"` works hello ugyanaz, mint a fenti hello.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-166">For example, `"@concat('#',parameters('currentFeedUrl'))"` works hello same as hello above.</span></span>

3.  <span data-ttu-id="9e9d3-167">Amikor elkészült, válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-167">When you're done, choose **Save**.</span></span> 

    <span data-ttu-id="9e9d3-168">Most hello webhely RSS-hírcsatorna úgy, hogy egy másik URL-címet hello keresztül módosítható `currentFeedURL` objektum.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-168">Now you can change hello website's RSS feed by passing a different URL through hello `currentFeedURL` object.</span></span>

<span data-ttu-id="9e9d3-169">További információ [hogyan tooauthor logikai alkalmazás definícióiról](../logic-apps/logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-169">Learn more about [how tooauthor logic app definitions](../logic-apps/logic-apps-author-definitions.md).</span></span>

## <a name="start-logic-app-workflows"></a><span data-ttu-id="9e9d3-170">Indítsa el a logic app munkafolyamatok</span><span class="sxs-lookup"><span data-stu-id="9e9d3-170">Start logic app workflows</span></span>

<span data-ttu-id="9e9d3-171">A Logic Apps alkalmazást definiált hello munkafolyamat indítása különböző lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-171">You have different options for starting hello workflow defined in your logic app.</span></span> <span data-ttu-id="9e9d3-172">Egy munkafolyamat-igény szerinti mindig indítható hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="9e9d3-172">You can always start a workflow on-demand in hello [Azure portal].</span></span>

### <a name="recurrence-triggers"></a><span data-ttu-id="9e9d3-173">Ismétlődés eseményindítók</span><span class="sxs-lookup"><span data-stu-id="9e9d3-173">Recurrence triggers</span></span>

<span data-ttu-id="9e9d3-174">Ismétlődés eseményindító megadott időközönként fut.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-174">A recurrence trigger runs at an interval that you specify.</span></span> <span data-ttu-id="9e9d3-175">Hello eseményindító feltételes logikához rendelkezik, a hello eseményindító meghatározza, hogy hello munkafolyamat toorun kell-e.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-175">When hello trigger has conditional logic, hello trigger determines whether hello workflow needs toorun.</span></span> <span data-ttu-id="9e9d3-176">Egy eseményindító azt jelzi, hogy hello munkafolyamat futtatásának vissza egy `200` állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-176">A trigger indicates hello workflow should run by returning a `200` status code.</span></span> <span data-ttu-id="9e9d3-177">Hello munkafolyamat toorun nem szükséges, ha hello eseményindító adja vissza egy `202` állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-177">When hello workflow doesn't need toorun, hello trigger returns a `202` status code.</span></span>

### <a name="callback-using-rest-apis"></a><span data-ttu-id="9e9d3-178">A visszahívási REST API-k használatával</span><span class="sxs-lookup"><span data-stu-id="9e9d3-178">Callback using REST APIs</span></span>

<span data-ttu-id="9e9d3-179">szolgáltatások toostart munkafolyamat, meghívhatja a logic app végpont.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-179">toostart a workflow, services can call a logic app endpoint.</span></span> <span data-ttu-id="9e9d3-180">toostart logic app igény, az ilyen válasszon **futtatása most** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="9e9d3-180">toostart this kind of logic app on-demand, choose **Run now** on hello command bar.</span></span> <span data-ttu-id="9e9d3-181">Lásd: [indítsa el a munkafolyamatok logic app végpontok eseményindítóként használható meghívásával](../logic-apps/logic-apps-http-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="9e9d3-181">See [Start workflows by calling logic app endpoints as triggers](../logic-apps/logic-apps-http-endpoint.md).</span></span> 

<!-- Shared links -->
[Azure-portálon]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="9e9d3-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9e9d3-183">Next steps</span></span>

* [<span data-ttu-id="9e9d3-184">Kapcsoló utasítások</span><span class="sxs-lookup"><span data-stu-id="9e9d3-184">Switch statements</span></span>](../logic-apps/logic-apps-switch-case.md) 
* [<span data-ttu-id="9e9d3-185">Hurkok, hatókörök és kibontás</span><span class="sxs-lookup"><span data-stu-id="9e9d3-185">Loops, scopes, and debatching</span></span>](../logic-apps/logic-apps-loops-and-scopes.md)
* [<span data-ttu-id="9e9d3-186">Logikaialkalmazás-definíciók készítése</span><span class="sxs-lookup"><span data-stu-id="9e9d3-186">Author logic app definitions</span></span>](../logic-apps/logic-apps-author-definitions.md)