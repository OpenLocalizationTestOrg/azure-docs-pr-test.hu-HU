---
title: "A különböző műveletek utasítás kapcsoló Azure Logic Apps |} Microsoft Docs"
description: "Válassza ki a különböző műveleteket hajthat végre a logic Apps alkalmazások kifejezés értékek alapján a switch utasítás használatával"
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
ms.openlocfilehash: 338b6a5b549d7bf81186550295608438ac4aee32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a><span data-ttu-id="ddcd3-104">A logic apps kapcsoló utasítással különböző műveleteket</span><span class="sxs-lookup"><span data-stu-id="ddcd3-104">Perform different actions in logic apps with a switch statement</span></span>

<span data-ttu-id="ddcd3-105">Szerzői munkafolyamat, ha gyakran kell más műveletek objektum vagy kifejezés értéke alapján.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-105">When authoring a workflow, you often have to take different actions based on the value of an object or expression.</span></span> <span data-ttu-id="ddcd3-106">Például érdemes a Logic Apps alkalmazást úgy kezd viselkedni HTTP-kérelem, vagy egy e-mailben kiválasztott lehetőségnek állapotkódját menübeállításoktól függően.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-106">For example, you might want your logic app to behave differently based on the status code of an HTTP request, or an option selected in an email.</span></span>

<span data-ttu-id="ddcd3-107">A switch utasítás használatával ezek a forgatókönyvek megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-107">You can use a switch statement to implement these scenarios.</span></span> <span data-ttu-id="ddcd3-108">A Logic Apps alkalmazást egy token vagy kifejezés kiértékelése, és válassza ki a kódaláírásokra is ugyanazt az értéket a megadott műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-108">Your logic app can evaluate a token or expression, and choose the case with the same value to execute the specified actions.</span></span> <span data-ttu-id="ddcd3-109">Csak egyetlen esetet meg kell felelnie az switch utasításban.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-109">Only one case should match the switch statement.</span></span>

> [!TIP]
> <span data-ttu-id="ddcd3-110">Minden programozási nyelv, például a switch utasítás csak egyenlőségi operátorok támogatja.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-110">Like all programming languages, switch statements support only equality operators.</span></span> <span data-ttu-id="ddcd3-111">Ha más relációs operátorok kell például a "nagyobb, mint", az állapot utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-111">If you need other relational operators, such as "greater than", use a condition statement.</span></span>
> <span data-ttu-id="ddcd3-112">Ahhoz, hogy determinisztikus végrehajtási viselkedésének, esetekben dinamikus jogkivonatok vagy kifejezés helyett egyedi és statikus értéket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-112">To ensure deterministic execution behavior, cases must contain a unique and static value instead of dynamic tokens or expression.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddcd3-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ddcd3-113">Prerequisites</span></span>

- <span data-ttu-id="ddcd3-114">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-114">An active Azure subscription.</span></span> <span data-ttu-id="ddcd3-115">Ha nincs egy aktív Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/), vagy próbálja [logikai alkalmazások szabad](https://tryappservice.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ddcd3-115">If you don't have an active Azure subscription, [create a free account](https://azure.microsoft.com/free/), or try [Logic Apps for free](https://tryappservice.azure.com/).</span></span>
- [<span data-ttu-id="ddcd3-116">A logic apps alapszintű ismerete</span><span class="sxs-lookup"><span data-stu-id="ddcd3-116">Basic knowledge about logic apps</span></span>](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-to-your-workflow"></a><span data-ttu-id="ddcd3-117">A switch utasítás hozzá a munkafolyamathoz</span><span class="sxs-lookup"><span data-stu-id="ddcd3-117">Add a switch statement to your workflow</span></span>

<span data-ttu-id="ddcd3-118">A switch utasítás működése megjelenítéséhez az alábbi példa létrehoz egy logikai alkalmazás, amely figyeli a dropbox alkalmazásba feltöltött fájlok.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-118">To show how a switch statement works, this example creates a logic app that monitors files uploaded to Dropbox.</span></span> <span data-ttu-id="ddcd3-119">Az új fájlok feltöltése után a logikai alkalmazást úgy dönt, hogy ezek a fájlok átvitele a SharePoint-e jóváhagyó e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-119">When the new files are uploaded, the logic app sends email to an approver who chooses whether to transfer those files to SharePoint.</span></span> <span data-ttu-id="ddcd3-120">Az alkalmazás használ egy kapcsoló utasítást, ami a jóváhagyó értéke alapján különböző műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-120">The app uses a switch statement that performs different actions based on the value that the approver selects.</span></span>

1. <span data-ttu-id="ddcd3-121">Logikai alkalmazás létrehozása, és válassza ki az ehhez az eseményindítóhoz: **Dropbox - fájl létrehozásakor**.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-121">Create a logic app, and select this trigger: **Dropbox - When a file is created**.</span></span>

   ![Használja a Dropbox -, ha egy fájl jön létre eseményindító](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. <span data-ttu-id="ddcd3-123">Az eseményindító, adja hozzá ezt a műveletet: **Outlook.com - jóváhagyási e-mail küldése**</span><span class="sxs-lookup"><span data-stu-id="ddcd3-123">Under the trigger, add this action: **Outlook.com - Send approval email**</span></span>

   > [!TIP]
   > <span data-ttu-id="ddcd3-124">A Logic apps egy Office 365 Outlook-fiókot küldő jóváhagyási e-mail lehetőségeket is támogatja.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-124">Logic apps also support sending approval email scenarios from an Office 365 Outlook account.</span></span>

   - <span data-ttu-id="ddcd3-125">Ha nincs meglévő kapcsolat, felszólítást hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-125">If you don't have an existing connection, you're prompted to create one.</span></span>
   - <span data-ttu-id="ddcd3-126">Töltse ki a kötelező mezőket.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-126">Fill in the required fields.</span></span> <span data-ttu-id="ddcd3-127">Például az **való**, adja meg az e-mail címet a jóváhagyó e-mailek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-127">For example, under **To**, specify the email address for sending the approver email.</span></span>
   - <span data-ttu-id="ddcd3-128">A **felhasználói beállítások**, adja meg `Approve, Reject`.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-128">Under **User Options**, enter `Approve, Reject`.</span></span>

   ![Kapcsolat beállítása](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. <span data-ttu-id="ddcd3-130">Adjon hozzá egy kapcsoló utasítást.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-130">Add a switch statement.</span></span>

   - <span data-ttu-id="ddcd3-131">Válassza ki **+ új lépés** > **... További** > **kapcsoló eset hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-131">Select **+ New step** > **... More** > **Add a switch case**.</span></span> 
   - <span data-ttu-id="ddcd3-132">Most azt szeretnénk, a végrehajtandó művelet kiválasztásához alapján a `SelectedOptions` kimenetét a *jóváhagyása e-mailek küldése* művelet.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-132">Now we want to select the action to perform based on the `SelectedOptions` output from the *Send approval email* action.</span></span> 
   <span data-ttu-id="ddcd3-133">Ez a mező található az **dinamikus tartalom hozzáadása az** választó.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-133">You can find this field in the **Add dynamic content** selector.</span></span>
   - <span data-ttu-id="ddcd3-134">Használjon *1. eset* kezelésére, amikor kijelöli a jóváhagyó `Approve`.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-134">Use *Case 1* to handle when the approver selects `Approve`.</span></span>
     - <span data-ttu-id="ddcd3-135">Jóváhagyás esetén másolja az eredeti fájlt a SharePoint Online-hoz való a [ **SharePoint Online - fájl létrehozása** művelet](../connectors/connectors-create-api-sharepointonline.md).</span><span class="sxs-lookup"><span data-stu-id="ddcd3-135">If approved, copy the original file to SharePoint Online with the [**SharePoint Online - Create file** action](../connectors/connectors-create-api-sharepointonline.md).</span></span>
     - <span data-ttu-id="ddcd3-136">Adja hozzá a eset értesítse a felhasználókat, hogy egy új fájl áll rendelkezésre a SharePoint-webhelyen belül egy másik művelet.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-136">Add another action within the case to notify users that a new file is available on SharePoint.</span></span>
   - <span data-ttu-id="ddcd3-137">Adja hozzá szeretné kezelni a felhasználó kiválaszt egy másik helyzet `Reject`.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-137">Add another case to handle when user selects `Reject`.</span></span>
     - <span data-ttu-id="ddcd3-138">Ha vissza kell utasítani, hogy a fájl nem fogadja el, és nincs további teendője, amely arról tájékoztatja, hogy más jóváhagyóknak értesítő e-mail küldése.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-138">If rejected, send a notification email informing other approvers that the file is rejected and no further action is required.</span></span>
   - <span data-ttu-id="ddcd3-139">`SelectedOptions`csak két lehetőséget biztosít, így azt is hagyhatja a **alapértelmezett** esetben üres.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-139">`SelectedOptions` provides only two options, so we can leave the **Default** case empty.</span></span>

   ![Switch utasítás](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > <span data-ttu-id="ddcd3-141">A switch utasítás kell legalább egy eset az alapértelmezett eset mellett.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-141">A switch statement needs at least one case in addition to the default case.</span></span>

4. <span data-ttu-id="ddcd3-142">A switch utasítás után törli az eredeti fájlt adja hozzá ezt a műveletet a dropbox alkalmazásba feltöltött: **Dropbox - fájl törlése**</span><span class="sxs-lookup"><span data-stu-id="ddcd3-142">After the switch statement, delete the original file uploaded to Dropbox by adding this action: **Dropbox - Delete file**</span></span>

5. <span data-ttu-id="ddcd3-143">Mentse a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-143">Save your logic app.</span></span> <span data-ttu-id="ddcd3-144">Tesztelje az alkalmazás feltölteni a fájlt a dropbox alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-144">Test your app by uploading a file to Dropbox.</span></span> <span data-ttu-id="ddcd3-145">Hamarosan jóváhagyása e-mailt kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-145">You should receive an approval email shortly.</span></span> <span data-ttu-id="ddcd3-146">Jelöljön ki egy lehetőséget, és tekintse meg a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-146">Select an option, and observe the behavior.</span></span>

   > [!TIP]
   > <span data-ttu-id="ddcd3-147">Ellenőrizze, hogy miként [logikai alkalmazások figyelése](logic-apps-monitor-your-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="ddcd3-147">Check out how to [monitor your logic apps](logic-apps-monitor-your-logic-apps.md).</span></span>

## <a name="understand-the-code-behind-switch-statements"></a><span data-ttu-id="ddcd3-148">A kapcsoló utasítások háttérkódot ismertetése</span><span class="sxs-lookup"><span data-stu-id="ddcd3-148">Understand the code behind switch statements</span></span>

<span data-ttu-id="ddcd3-149">Most, hogy sikeresen létrehozta a logikai alkalmazás egy switch utasítás használatával, a kód megadása a switch utasítás mögött vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-149">Now that you successfully created a logic app using a switch statement, let's look at the code definition behind the switch statement.</span></span>

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

* <span data-ttu-id="ddcd3-150">`"Switch"`a kapcsoló utasítást, ami az olvashatóság átnevezheti neve van.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-150">`"Switch"` is the name of the switch statement, which you can rename for readability.</span></span> 
* <span data-ttu-id="ddcd3-151">`"type": "Switch"`azt jelzi, hogy a művelet egy switch utasításban.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-151">`"type": "Switch"` indicates that the action is a switch statement.</span></span> 
* <span data-ttu-id="ddcd3-152">`"expression"`Ebben a példában a jóváhagyó a kijelölt beállítást, és minden esetben később a definíció deklarált képest értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-152">`"expression"` is the approver's selected option in this example and is evaluated against each case declared later in the definition.</span></span> 
* <span data-ttu-id="ddcd3-153">`"cases"`tetszőleges számú esetek is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-153">`"cases"` can contain any number of cases.</span></span> <span data-ttu-id="ddcd3-154">A minden esetben `"Case *"` az esetében, amelyek átnevezheti az olvashatóság alapértelmezett neve.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-154">For each case, `"Case *"` is the default name of the case, which you can rename for readability.</span></span> 
<span data-ttu-id="ddcd3-155">`"case"`a case címke, amely a kapcsolókifejezés használja az összehasonlításhoz, határozza meg és állandó és egyedi értéknek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-155">`"case"` specifies the case label, which the switch expression uses for comparison, and must be a constant and unique value.</span></span> <span data-ttu-id="ddcd3-156">Ha nem tartozik a kifejezéssel kapcsoló, a műveletek `"default"` hajtja végre a rendszer.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-156">If none of the cases match the switch expression, actions under `"default"` are executed.</span></span>

## <a name="get-help"></a><span data-ttu-id="ddcd3-157">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="ddcd3-157">Get help</span></span>

<span data-ttu-id="ddcd3-158">Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-158">To ask questions, answer questions, and see what other Azure Logic Apps users are doing, visit the [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).</span></span>

<span data-ttu-id="ddcd3-159">Ha szeretné segíteni az Azure Logic Apps és összekötők fejlesztését, szavazzon az ötletekre az [Azure Logic Apps felhasználói visszajelzések oldalán](http://aka.ms/logicapps-wish), vagy küldje be saját javaslatait.</span><span class="sxs-lookup"><span data-stu-id="ddcd3-159">To help improve Azure Logic Apps and connectors, vote on or submit ideas at the [Azure Logic Apps user feedback site](http://aka.ms/logicapps-wish).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ddcd3-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ddcd3-160">Next steps</span></span>

- <span data-ttu-id="ddcd3-161">Megtudhatja, hogyan [feltételek hozzáadása](logic-apps-use-logic-app-features.md)</span><span class="sxs-lookup"><span data-stu-id="ddcd3-161">Learn how to [add conditions](logic-apps-use-logic-app-features.md)</span></span>
- <span data-ttu-id="ddcd3-162">További tudnivalók [hiba és kivételkezelést](logic-apps-exception-handling.md)</span><span class="sxs-lookup"><span data-stu-id="ddcd3-162">Learn about [error and exception handling](logic-apps-exception-handling.md)</span></span>
- <span data-ttu-id="ddcd3-163">Fedezze fel több [munkafolyamat nyelvi képességek](logic-apps-author-definitions.md)</span><span class="sxs-lookup"><span data-stu-id="ddcd3-163">Explore more [workflow language capabilities](logic-apps-author-definitions.md)</span></span>