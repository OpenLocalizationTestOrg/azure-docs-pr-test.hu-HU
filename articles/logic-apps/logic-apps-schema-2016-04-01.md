---
title: "Séma frissítések 2016. június-1-- Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps JSON-meghatározások létrehozása séma 2016-06-01-es verziójával"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 43df04d6478e44c82c88b17d916cfc9fe4afc03e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="9cbf7-103">Az Azure Logic Apps – 2016. június 1 sémafrissítések</span><span class="sxs-lookup"><span data-stu-id="9cbf7-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="9cbf7-104">Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps megbízhatóbb és könnyebben használható:</span><span class="sxs-lookup"><span data-stu-id="9cbf7-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier to use:</span></span>

* <span data-ttu-id="9cbf7-105">[Hatókörök](#scopes) lehetővé teszik, hogy a csoport- vagy beágyazni műveletek műveletek gyűjteményeként.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="9cbf7-106">[Feltételek és a hurkok](#conditions-loops) első osztályú műveletek is.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="9cbf7-107">Pontosabb rendezést a műveletek futtatása a `runAfter` tulajdonság, cseréje`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="9cbf7-107">More precise ordering for running actions with the `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="9cbf7-108">A logic apps a 2015. augusztus 1 preview séma frissítéséhez a 2016. június 1 séma [tekintse meg a verziófrissítés szakasz](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="9cbf7-108">To upgrade your logic apps from the August 1, 2015 preview schema to the June 1, 2016 schema, [check out the upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="9cbf7-109">Hatókörök</span><span class="sxs-lookup"><span data-stu-id="9cbf7-109">Scopes</span></span>

<span data-ttu-id="9cbf7-110">A séma tartalmaz a hatókörök, amelyek lehetővé teszik, hogy együtt műveleteit, vagy a nest műveletek belül egymással.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="9cbf7-111">Egy feltétel tartalmazhat például egy másik feltétel.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="9cbf7-112">További információ [szintaxis hatókör](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át a alapvető hatókör példa:</span><span class="sxs-lookup"><span data-stu-id="9cbf7-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="9cbf7-113">Feltételek és a hurkok módosítása</span><span class="sxs-lookup"><span data-stu-id="9cbf7-113">Conditions and loops changes</span></span>

<span data-ttu-id="9cbf7-114">Előző séma verziója, a feltételek és a hurkok voltak egyetlen művelettel társított paraméter.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="9cbf7-115">Ebben a sémában liftek ezt a korlátozást, így a feltételek és a hurkok megjelenni művelettípusok.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="9cbf7-116">További információ [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át az egyszerű példában feltétel művelethez:</span><span class="sxs-lookup"><span data-stu-id="9cbf7-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a><span data-ttu-id="9cbf7-117">"runAfter" tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9cbf7-117">'runAfter' property</span></span>

<span data-ttu-id="9cbf7-118">A `runAfter` tulajdonság cserél `dependsOn`, nyújtó további pontosság megadása a futtatási ahhoz, hogy a műveletek előző műveletek állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-118">The `runAfter` property replaces `dependsOn`, providing more precision when you specify the run order for actions based on the status of previous actions.</span></span>

<span data-ttu-id="9cbf7-119">A `dependsOn` azonos a "a művelet futtatása és sikeres volt-e" tulajdonsága, nem számít, hány alkalommal szeretné alapján, hogy az előző művelet sikeres volt, a művelet végrehajtása sikertelen volt, vagy kihagyott.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-119">The `dependsOn` property was synonymous with "the action ran and was successful", no matter how many times you wanted to execute an action, based on whether the previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="9cbf7-120">A `runAfter` tulajdonság olyan objektum, amely után az objektum fut művelet nevét adja meg, hogy rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-120">The `runAfter` property provides that flexibility as an object that specifies all the action names after which the object runs.</span></span> <span data-ttu-id="9cbf7-121">Ez a tulajdonság is megadja elfogadható eseményindítóként használható állapotait tömbjét.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="9cbf7-122">Például ha végezze el a lépést A sikeres és is után B lépésben sikeres vagy sikertelen lesz, hozhat létre a `runAfter` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="9cbf7-122">For example, if you wanted to run after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="9cbf7-123">A séma frissítése</span><span class="sxs-lookup"><span data-stu-id="9cbf7-123">Upgrade your schema</span></span>

<span data-ttu-id="9cbf7-124">Az új séma frissítése csak néhány lépésben vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-124">Upgrading to the new schema only takes a few steps.</span></span> <span data-ttu-id="9cbf7-125">A frissítési folyamat magában foglalja a frissítési parancsprogram futtatása mentése új logikai alkalmazás, és ha azt szeretné, valószínűleg felülírja az előző logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-125">The upgrade process includes running the upgrade script, saving as a new logic app, and if you want, possibly overwriting the previous logic app.</span></span>

1. <span data-ttu-id="9cbf7-126">Nyissa meg a Logic Apps alkalmazást az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-126">In the Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="9cbf7-127">Ugrás a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-127">Go to **Overview**.</span></span> <span data-ttu-id="9cbf7-128">A logic app eszköztáron válassza **frissítés séma**.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-128">On the logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Válassza ki a séma frissítése][1]
   
    <span data-ttu-id="9cbf7-130">A frissített definícióban ad vissza, amelyet másolja és illessze be az erőforrás-definíció szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-130">The upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="9cbf7-131">Azonban azt **erősen ajánlott** választja **Mentés másként** győződjön meg arról, hogy az összes kapcsolat érvényesek a frissített logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-131">However, we **strongly recommend** you choose **Save As** to make sure that all connection references are valid in the upgraded logic app.</span></span>

3. <span data-ttu-id="9cbf7-132">A frissítési panel eszköztáron válassza **Mentés másként**.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-132">In the upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="9cbf7-133">Adja meg a logikai nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-133">Enter the logic name and status.</span></span> <span data-ttu-id="9cbf7-134">A frissített Logic Apps alkalmazást telepíteni, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-134">To deploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="9cbf7-135">Győződjön meg arról, hogy a frissített logikai alkalmazás megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9cbf7-136">Manuális vagy kérelem eseményindítót használ, ha a visszahívás URL-címet az új logikai alkalmazás változik.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-136">If you are using a manual or request trigger, the callback URL changes in your new logic app.</span></span> <span data-ttu-id="9cbf7-137">Teszteléséhez győződjön meg arról, hogy a végpont élmény működik az új URL-címet.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-137">Test the new URL to make sure the end-to-end experience works.</span></span> <span data-ttu-id="9cbf7-138">Előző URL-címek megőrzéséhez a meglévő Logic Apps alkalmazást keresztül tud klónozni.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-138">To preserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="9cbf7-139">*Nem kötelező* felülírja az előző logikai alkalmazást az új verzióval séma, az eszköztáron válassza **Klónozás**mellett található **frissítés séma**.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-139">*Optional* To overwrite your previous logic app with the new schema version, on the toolbar, choose **Clone**, next to **Update Schema**.</span></span> <span data-ttu-id="9cbf7-140">Ez a lépés szükség, csak akkor, ha szeretné megtartani a azonos erőforrás-azonosító, vagy kérje meg a Logic Apps alkalmazást indítási URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-140">This step is necessary only if you want to keep the same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="9cbf7-141">Frissítési eszköz megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9cbf7-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="9cbf7-142">Leképezési feltételek</span><span class="sxs-lookup"><span data-stu-id="9cbf7-142">Mapping conditions</span></span>

<span data-ttu-id="9cbf7-143">A frissített definícióban az eszköz lehetővé teszi egy, a true és false fiókirodai műveletek csoportosítása hatóköreként a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-143">In the upgraded definition, the tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="9cbf7-144">Pontosabban, a Tervező mintáját `@equals(actions('a').status, 'Skipped')` meg kell jelennie egy `else` művelet.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-144">Specifically, the designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="9cbf7-145">Azonban az eszköz felismerhetetlen minták észleli, ha az eszköz lehet, hogy hozzon létre külön feltételek a true és a hamis ág.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-145">However, if the tool detects unrecognizable patterns, the tool might create separate conditions for both the true and the false branch.</span></span> <span data-ttu-id="9cbf7-146">Műveletek leképezheti a frissítés után is, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="9cbf7-147">feltételt tartalmazó "foreach" hurok</span><span class="sxs-lookup"><span data-stu-id="9cbf7-147">'foreach' loop with condition</span></span>

<span data-ttu-id="9cbf7-148">Az új sémával, használhatja a szűrési művelet replikálja a mintát egy `foreach` elemenként, eltérő feltételt tartalmazó hurok, de ezt a változtatást automatikusan történjen, amikor frissít.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-148">In the new schema, you can use the filter action to replicate the pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="9cbf7-149">A feltételnek megfelelő elemek csak egy tömb visszaküldésére használatos a foreach hurok előtt szűrőművelet lesz, és, hogy tömb átad a foreach műveletet.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-149">The condition becomes a filter action before the foreach loop for returning only an array of items that match the condition, and that array is passed into the foreach action.</span></span> <span data-ttu-id="9cbf7-150">Egy vonatkozó példáért lásd: [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="9cbf7-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="9cbf7-151">Az erőforráscímkék</span><span class="sxs-lookup"><span data-stu-id="9cbf7-151">Resource tags</span></span>

<span data-ttu-id="9cbf7-152">A frissítés befejezése után az erőforráscímkék eltávolítja, így azokat a frissített munkafolyamat kell visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-152">After you upgrade, resource tags are removed, so you must reset them for the upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="9cbf7-153">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="9cbf7-153">Other changes</span></span>

### <a name="renamed-manual-trigger-to-request-trigger"></a><span data-ttu-id="9cbf7-154">A "kérelem" eseményindító átnevezett "manual" eseményindító</span><span class="sxs-lookup"><span data-stu-id="9cbf7-154">Renamed 'manual' trigger to 'request' trigger</span></span>

<span data-ttu-id="9cbf7-155">A `manual` indítási típus elavult, és a átnevezésre `request` típusú `http`.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-155">The `manual` trigger type was deprecated and renamed to `request` with type `http`.</span></span> <span data-ttu-id="9cbf7-156">Ez a változás hoz létre további konzisztencia mintát típusú, amely az eseményindító segítségével hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-156">This change creates more consistency for the kind of pattern that the trigger is used to build.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="9cbf7-157">Új "filter" művelet</span><span class="sxs-lookup"><span data-stu-id="9cbf7-157">New 'filter' action</span></span>

<span data-ttu-id="9cbf7-158">Elemek körét le egy nagy tömböt szűrése az új `filter` típus elfogadja a tömb és egy feltétel, az egyes elemekhez tartozó feltétel eredménye és tömböt ad vissza a feltétel értekezlet elemekhez.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-158">To filter a large array down to a smaller set of items, the new `filter` type accepts an array and a condition, evaluates the condition for each item, and returns an array with items meeting the condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="9cbf7-159">A "foreach" és "csak" műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="9cbf7-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="9cbf7-160">A `foreach` és `until` hurok csak egyetlen művelettel.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-160">The `foreach` and `until` loop are restricted to a single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="9cbf7-161">Új "trackedProperties" műveletek</span><span class="sxs-lookup"><span data-stu-id="9cbf7-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="9cbf7-162">Műveletek is most már rendelkezik egy további tulajdonság nevű `trackedProperties`, ez az a testvér a `runAfter` és `type` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-162">Actions can now have an additional property called `trackedProperties`, which is sibling to the `runAfter` and `type` properties.</span></span> <span data-ttu-id="9cbf7-163">Ezt az objektumot határozza meg az egyes művelet bemeneti vagy kimeneti munkafolyamat részeként kibocsátott Azure diagnosztikai telemetriai adatok szerepeltetni kívánt.</span><span class="sxs-lookup"><span data-stu-id="9cbf7-163">This object specifies certain action inputs or outputs that you want to include in the Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="9cbf7-164">Példa:</span><span class="sxs-lookup"><span data-stu-id="9cbf7-164">For example:</span></span>

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="9cbf7-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9cbf7-165">Next Steps</span></span>
* [<span data-ttu-id="9cbf7-166">A logic apps munkafolyamat-meghatározások létrehozása</span><span class="sxs-lookup"><span data-stu-id="9cbf7-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="9cbf7-167">A logic Apps alkalmazások központi telepítési sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9cbf7-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
