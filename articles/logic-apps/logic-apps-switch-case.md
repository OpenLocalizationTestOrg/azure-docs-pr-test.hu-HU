---
title: "az Azure Logic Apps különféle műveletek aaaSwitch nyilatkozata |} Microsoft Docs"
description: "Adja meg a logic apps kifejezés értékek alapján a switch utasítás használatával különféle műveletek tooperform"
services: logic-apps
keywords: "Switch utasítás"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a><span data-ttu-id="e00ac-104">A logic apps kapcsoló utasítással különböző műveleteket</span><span class="sxs-lookup"><span data-stu-id="e00ac-104">Perform different actions in logic apps with a switch statement</span></span>

<span data-ttu-id="e00ac-105">Amikor egy munkafolyamat szerzői, gyakran előfordul, hogy tootake különféle műveletek objektum vagy kifejezés hello érték alapján.</span><span class="sxs-lookup"><span data-stu-id="e00ac-105">When authoring a workflow, you often have tootake different actions based on hello value of an object or expression.</span></span> <span data-ttu-id="e00ac-106">Érdemes például a logic app toobehave másképp hello állapotkód a HTTP-lekérdezés alapján, vagy egy e-mailben kiválasztott lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e00ac-106">For example, you might want your logic app toobehave differently based on hello status code of an HTTP request, or an option selected in an email.</span></span>

<span data-ttu-id="e00ac-107">Használhatja a switch utasítás tooimplement forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="e00ac-107">You can use a switch statement tooimplement these scenarios.</span></span> <span data-ttu-id="e00ac-108">A Logic Apps alkalmazást egy token vagy kifejezés kiértékelése, és válasszon hello esetet, amelyben hello azonos érték tooexecute hello megadott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="e00ac-108">Your logic app can evaluate a token or expression, and choose hello case with hello same value tooexecute hello specified actions.</span></span> <span data-ttu-id="e00ac-109">Csak egyetlen esetet meg kell felelnie hello switch utasításban.</span><span class="sxs-lookup"><span data-stu-id="e00ac-109">Only one case should match hello switch statement.</span></span>

> [!TIP]
> <span data-ttu-id="e00ac-110">Minden programozási nyelv, például a switch utasítás csak egyenlőségi operátorok támogatja.</span><span class="sxs-lookup"><span data-stu-id="e00ac-110">Like all programming languages, switch statements support only equality operators.</span></span> <span data-ttu-id="e00ac-111">Ha más relációs operátorok kell például a "nagyobb, mint", az állapot utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="e00ac-111">If you need other relational operators, such as "greater than", use a condition statement.</span></span>
> <span data-ttu-id="e00ac-112">tooensure determinisztikus végrehajtási viselkedésének, esetekben dinamikus jogkivonatok vagy kifejezés helyett egyedi és statikus értéket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="e00ac-112">tooensure deterministic execution behavior, cases must contain a unique and static value instead of dynamic tokens or expression.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e00ac-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e00ac-113">Prerequisites</span></span>

- <span data-ttu-id="e00ac-114">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e00ac-114">An active Azure subscription.</span></span> <span data-ttu-id="e00ac-115">Ha nincs egy aktív Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/), vagy próbálja [logikai alkalmazások szabad](https://tryappservice.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e00ac-115">If you don't have an active Azure subscription, [create a free account](https://azure.microsoft.com/free/), or try [Logic Apps for free](https://tryappservice.azure.com/).</span></span>
- [<span data-ttu-id="e00ac-116">A logic apps alapszintű ismerete</span><span class="sxs-lookup"><span data-stu-id="e00ac-116">Basic knowledge about logic apps</span></span>](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a><span data-ttu-id="e00ac-117">A kapcsoló utasítás tooyour munkafolyamat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e00ac-117">Add a switch statement tooyour workflow</span></span>

<span data-ttu-id="e00ac-118">a switch utasítás működése tooshow, ebben a példában, hogy a figyelők fájlok feltöltése tooDropbox logikai alkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e00ac-118">tooshow how a switch statement works, this example creates a logic app that monitors files uploaded tooDropbox.</span></span> <span data-ttu-id="e00ac-119">Ha hello új fájlok feltöltése után hello logikai alkalmazás küld-e e-mailek tooan jóváhagyó úgy dönt, hogy tootransfer ezen fájlok tooSharePoint.</span><span class="sxs-lookup"><span data-stu-id="e00ac-119">When hello new files are uploaded, hello logic app sends email tooan approver who chooses whether tootransfer those files tooSharePoint.</span></span> <span data-ttu-id="e00ac-120">hello alkalmazása használja-e a switch utasítás hajtja végre különböző műveleteket hello érték alapján hello jóváhagyó választja ki.</span><span class="sxs-lookup"><span data-stu-id="e00ac-120">hello app uses a switch statement that performs different actions based on hello value that hello approver selects.</span></span>

1. <span data-ttu-id="e00ac-121">Logikai alkalmazás létrehozása, és válassza ki az ehhez az eseményindítóhoz: **Dropbox - fájl létrehozásakor**.</span><span class="sxs-lookup"><span data-stu-id="e00ac-121">Create a logic app, and select this trigger: **Dropbox - When a file is created**.</span></span>

   ![Használja a Dropbox -, ha egy fájl jön létre eseményindító](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. <span data-ttu-id="e00ac-123">Hello eseményindító, adja hozzá ezt a műveletet: **Outlook.com - jóváhagyási e-mail küldése**</span><span class="sxs-lookup"><span data-stu-id="e00ac-123">Under hello trigger, add this action: **Outlook.com - Send approval email**</span></span>

   > [!TIP]
   > <span data-ttu-id="e00ac-124">A Logic apps egy Office 365 Outlook-fiókot küldő jóváhagyási e-mail lehetőségeket is támogatja.</span><span class="sxs-lookup"><span data-stu-id="e00ac-124">Logic apps also support sending approval email scenarios from an Office 365 Outlook account.</span></span>

   - <span data-ttu-id="e00ac-125">Ha nincs meglévő kapcsolat, megkéri toocreate egyet.</span><span class="sxs-lookup"><span data-stu-id="e00ac-125">If you don't have an existing connection, you're prompted toocreate one.</span></span>
   - <span data-ttu-id="e00ac-126">Hello kötelező mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="e00ac-126">Fill in hello required fields.</span></span> <span data-ttu-id="e00ac-127">Például az **való**, adja meg az üdvözlő e-mail címet hello jóváhagyó e-mailek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="e00ac-127">For example, under **To**, specify hello email address for sending hello approver email.</span></span>
   - <span data-ttu-id="e00ac-128">A **felhasználói beállítások**, adja meg `Approve, Reject`.</span><span class="sxs-lookup"><span data-stu-id="e00ac-128">Under **User Options**, enter `Approve, Reject`.</span></span>

   ![Kapcsolat beállítása](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. <span data-ttu-id="e00ac-130">Adjon hozzá egy kapcsoló utasítást.</span><span class="sxs-lookup"><span data-stu-id="e00ac-130">Add a switch statement.</span></span>

   - <span data-ttu-id="e00ac-131">Válassza ki **+ új lépés** > **... További** > **kapcsoló eset hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e00ac-131">Select **+ New step** > **... More** > **Add a switch case**.</span></span> 
   - <span data-ttu-id="e00ac-132">Most azt szeretnénk, ha tooselect hello művelet tooperform hello alapján `SelectedOptions` hello kimenetét *jóváhagyása e-mailek küldése* művelet.</span><span class="sxs-lookup"><span data-stu-id="e00ac-132">Now we want tooselect hello action tooperform based on hello `SelectedOptions` output from hello *Send approval email* action.</span></span> 
   <span data-ttu-id="e00ac-133">Ez a mező található hello **dinamikus tartalom hozzáadása az** választó.</span><span class="sxs-lookup"><span data-stu-id="e00ac-133">You can find this field in hello **Add dynamic content** selector.</span></span>
   - <span data-ttu-id="e00ac-134">Használjon *1. eset* toohandle, amikor kijelöli hello jóváhagyó `Approve`.</span><span class="sxs-lookup"><span data-stu-id="e00ac-134">Use *Case 1* toohandle when hello approver selects `Approve`.</span></span>
     - <span data-ttu-id="e00ac-135">Jóváhagyás esetén másolja hello eredeti fájl tooSharePoint Online rendelkező hello [ **SharePoint Online - fájl létrehozása** művelet](../connectors/connectors-create-api-sharepointonline.md).</span><span class="sxs-lookup"><span data-stu-id="e00ac-135">If approved, copy hello original file tooSharePoint Online with hello [**SharePoint Online - Create file** action](../connectors/connectors-create-api-sharepointonline.md).</span></span>
     - <span data-ttu-id="e00ac-136">Adja hozzá a hello eset toonotify felhasználókat, hogy egy új fájl áll rendelkezésre a SharePoint-webhelyen belül egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="e00ac-136">Add another action within hello case toonotify users that a new file is available on SharePoint.</span></span>
   - <span data-ttu-id="e00ac-137">Adjon hozzá egy másik eset toohandle, ha a felhasználó kijelöli `Reject`.</span><span class="sxs-lookup"><span data-stu-id="e00ac-137">Add another case toohandle when user selects `Reject`.</span></span>
     - <span data-ttu-id="e00ac-138">Ha vissza kell utasítani, hogy hello fájl elutasítva, és nincs további teendője, amely arról tájékoztatja, hogy más jóváhagyóknak értesítő e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="e00ac-138">If rejected, send a notification email informing other approvers that hello file is rejected and no further action is required.</span></span>
   - <span data-ttu-id="e00ac-139">`SelectedOptions`csak két lehetőséget biztosít, így azt is hagyhatja hello **alapértelmezett** esetben üres.</span><span class="sxs-lookup"><span data-stu-id="e00ac-139">`SelectedOptions` provides only two options, so we can leave hello **Default** case empty.</span></span>

   ![Switch utasítás](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > <span data-ttu-id="e00ac-141">A switch utasítás kell legalább egy eset hozzáadása toohello alapértelmezett esetben.</span><span class="sxs-lookup"><span data-stu-id="e00ac-141">A switch statement needs at least one case in addition toohello default case.</span></span>

4. <span data-ttu-id="e00ac-142">Hello switch utasításban, követően törölje ezt a műveletet hello eredeti feltöltött fájl tooDropbox: **Dropbox - fájl törlése**</span><span class="sxs-lookup"><span data-stu-id="e00ac-142">After hello switch statement, delete hello original file uploaded tooDropbox by adding this action: **Dropbox - Delete file**</span></span>

5. <span data-ttu-id="e00ac-143">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e00ac-143">Save your logic app.</span></span> <span data-ttu-id="e00ac-144">Az alkalmazás tesztelése a fájl tooDropbox feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="e00ac-144">Test your app by uploading a file tooDropbox.</span></span> <span data-ttu-id="e00ac-145">Hamarosan jóváhagyása e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="e00ac-145">You should receive an approval email shortly.</span></span> <span data-ttu-id="e00ac-146">Jelöljön ki egy lehetőséget, és tekintse meg az hello viselkedését.</span><span class="sxs-lookup"><span data-stu-id="e00ac-146">Select an option, and observe hello behavior.</span></span>

   > [!TIP]
   > <span data-ttu-id="e00ac-147">Ellenőrizze, hogyan túl[logikai alkalmazások figyelése](logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e00ac-147">Check out how too[monitor your logic apps](logic-apps-monitor-your-logic-apps.md).</span></span>

## <a name="understand-hello-code-behind-switch-statements"></a><span data-ttu-id="e00ac-148">Hello háttérkódot kapcsoló utasítások megismeréséhez</span><span class="sxs-lookup"><span data-stu-id="e00ac-148">Understand hello code behind switch statements</span></span>

<span data-ttu-id="e00ac-149">Most, hogy sikeresen létrehozta a logikai alkalmazás egy switch utasítás használatával, nézzük hello kód megadása mögött hello switch utasításban.</span><span class="sxs-lookup"><span data-stu-id="e00ac-149">Now that you successfully created a logic app using a switch statement, let's look at hello code definition behind hello switch statement.</span></span>

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* <span data-ttu-id="e00ac-150">`"Switch"`átnevezheti az olvashatóság hello switch utasítás hello neve van.</span><span class="sxs-lookup"><span data-stu-id="e00ac-150">`"Switch"` is hello name of hello switch statement, which you can rename for readability.</span></span> 
* <span data-ttu-id="e00ac-151">`"type": "Switch"`azt jelzi, hogy hello művelet egy switch utasításban.</span><span class="sxs-lookup"><span data-stu-id="e00ac-151">`"type": "Switch"` indicates that hello action is a switch statement.</span></span> 
* <span data-ttu-id="e00ac-152">`"expression"`Ebben a példában a hello jóváhagyó kiválasztott beállítás, és minden esetben később hello definition deklarált képest értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="e00ac-152">`"expression"` is hello approver's selected option in this example and is evaluated against each case declared later in hello definition.</span></span> 
* <span data-ttu-id="e00ac-153">`"cases"`tetszőleges számú esetek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e00ac-153">`"cases"` can contain any number of cases.</span></span> <span data-ttu-id="e00ac-154">A minden esetben `"Case *"` hello alapértelmezett név az olvashatóság átnevezheti hello eset.</span><span class="sxs-lookup"><span data-stu-id="e00ac-154">For each case, `"Case *"` is hello default name of hello case, which you can rename for readability.</span></span> 
<span data-ttu-id="e00ac-155">`"case"`Megadja a hello eset címke, amely hello kapcsoló kifejezést használ az összehasonlításhoz, és állandó és egyedi értéknek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e00ac-155">`"case"` specifies hello case label, which hello switch expression uses for comparison, and must be a constant and unique value.</span></span> <span data-ttu-id="e00ac-156">Ha nincs hello esetekben hello kapcsoló kifejezésnek a feltétel teljesüléséhez, a műveletek `"default"` hajtja végre a rendszer.</span><span class="sxs-lookup"><span data-stu-id="e00ac-156">If none of hello cases match hello switch expression, actions under `"default"` are executed.</span></span>

## <a name="get-help"></a><span data-ttu-id="e00ac-157">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="e00ac-157">Get help</span></span>

<span data-ttu-id="e00ac-158">tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, tekintse meg a Microsoft hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span><span class="sxs-lookup"><span data-stu-id="e00ac-158">tooask questions, answer questions, and see what other Azure Logic Apps users are doing, visit hello [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="e00ac-159">toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).</span><span class="sxs-lookup"><span data-stu-id="e00ac-159">toohelp improve Azure Logic Apps and connectors, vote on or submit ideas at hello [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e00ac-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e00ac-160">Next steps</span></span>

- <span data-ttu-id="e00ac-161">Ismerje meg, hogyan túl[feltételek hozzáadása](logic-apps-use-logic-app-features.md)</span><span class="sxs-lookup"><span data-stu-id="e00ac-161">Learn how too[add conditions](logic-apps-use-logic-app-features.md)</span></span>
- <span data-ttu-id="e00ac-162">További tudnivalók [hiba és kivételkezelést](logic-apps-exception-handling.md)</span><span class="sxs-lookup"><span data-stu-id="e00ac-162">Learn about [error and exception handling](logic-apps-exception-handling.md)</span></span>
- <span data-ttu-id="e00ac-163">Fedezze fel több [munkafolyamat nyelvi képességek](logic-apps-author-definitions.md)</span><span class="sxs-lookup"><span data-stu-id="e00ac-163">Explore more [workflow language capabilities](logic-apps-author-definitions.md)</span></span>