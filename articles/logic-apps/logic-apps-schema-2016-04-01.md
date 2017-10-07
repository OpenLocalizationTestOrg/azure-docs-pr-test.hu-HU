---
title: "frissítések 2016. június-1-- Azure Logic Apps aaaSchema |} Microsoft Docs"
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
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a><span data-ttu-id="c8af6-103">Az Azure Logic Apps – 2016. június 1 sémafrissítések</span><span class="sxs-lookup"><span data-stu-id="c8af6-103">Schema updates for Azure Logic Apps - June 1, 2016</span></span>

<span data-ttu-id="c8af6-104">Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps több megbízható és egyszerűbb toouse:</span><span class="sxs-lookup"><span data-stu-id="c8af6-104">This new schema and API version for Azure Logic Apps includes key improvements that make logic apps more reliable and easier toouse:</span></span>

* <span data-ttu-id="c8af6-105">[Hatókörök](#scopes) lehetővé teszik, hogy a csoport- vagy beágyazni műveletek műveletek gyűjteményeként.</span><span class="sxs-lookup"><span data-stu-id="c8af6-105">[Scopes](#scopes) let you group or nest actions as a collection of actions.</span></span>
* <span data-ttu-id="c8af6-106">[Feltételek és a hurkok](#conditions-loops) első osztályú műveletek is.</span><span class="sxs-lookup"><span data-stu-id="c8af6-106">[Conditions and loops](#conditions-loops) are now first-class actions.</span></span>
* <span data-ttu-id="c8af6-107">Pontosabb rendezést műveletek futtatása a hello `runAfter` tulajdonság, cseréje`dependsOn`</span><span class="sxs-lookup"><span data-stu-id="c8af6-107">More precise ordering for running actions with hello `runAfter` property, replacing `dependsOn`</span></span>

<span data-ttu-id="c8af6-108">az a logic apps 2015. augusztus 1 hello tooupgrade előzetes séma toohello 2016. június 1 séma [tekintse meg a verziófrissítés szakasz hello](##upgrade-your-schema).</span><span class="sxs-lookup"><span data-stu-id="c8af6-108">tooupgrade your logic apps from hello August 1, 2015 preview schema toohello June 1, 2016 schema, [check out hello upgrade section](##upgrade-your-schema).</span></span>

<a name="scopes"></a>
## <a name="scopes"></a><span data-ttu-id="c8af6-109">Hatókörök</span><span class="sxs-lookup"><span data-stu-id="c8af6-109">Scopes</span></span>

<span data-ttu-id="c8af6-110">A séma tartalmaz a hatókörök, amelyek lehetővé teszik, hogy együtt műveleteit, vagy a nest műveletek belül egymással.</span><span class="sxs-lookup"><span data-stu-id="c8af6-110">This schema includes scopes, which let you group actions together, or nest actions inside each other.</span></span> <span data-ttu-id="c8af6-111">Egy feltétel tartalmazhat például egy másik feltétel.</span><span class="sxs-lookup"><span data-stu-id="c8af6-111">For example, a condition can contain another condition.</span></span> <span data-ttu-id="c8af6-112">További információ [szintaxis hatókör](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át a alapvető hatókör példa:</span><span class="sxs-lookup"><span data-stu-id="c8af6-112">Learn more about [scope syntax](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic scope example:</span></span>

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
## <a name="conditions-and-loops-changes"></a><span data-ttu-id="c8af6-113">Feltételek és a hurkok módosítása</span><span class="sxs-lookup"><span data-stu-id="c8af6-113">Conditions and loops changes</span></span>

<span data-ttu-id="c8af6-114">Előző séma verziója, a feltételek és a hurkok voltak egyetlen művelettel társított paraméter.</span><span class="sxs-lookup"><span data-stu-id="c8af6-114">In previous schema versions, conditions and loops were parameters associated with a single action.</span></span> <span data-ttu-id="c8af6-115">Ebben a sémában liftek ezt a korlátozást, így a feltételek és a hurkok megjelenni művelettípusok.</span><span class="sxs-lookup"><span data-stu-id="c8af6-115">This schema lifts this limitation, so conditions and loops now appear as action types.</span></span> <span data-ttu-id="c8af6-116">További információ [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át az egyszerű példában feltétel művelethez:</span><span class="sxs-lookup"><span data-stu-id="c8af6-116">Learn more about [loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md), or review this basic example for a condition action:</span></span>

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
## <a name="runafter-property"></a><span data-ttu-id="c8af6-117">"runAfter" tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c8af6-117">'runAfter' property</span></span>

<span data-ttu-id="c8af6-118">Hello `runAfter` tulajdonság cserél `dependsOn`, nyújtó pontosabb futtatása hello ahhoz, hogy a műveletek megadásakor korábbi műveletek hello állapota alapján.</span><span class="sxs-lookup"><span data-stu-id="c8af6-118">hello `runAfter` property replaces `dependsOn`, providing more precision when you specify hello run order for actions based on hello status of previous actions.</span></span>

<span data-ttu-id="c8af6-119">Hello `dependsOn` tulajdonságának azonos a "hello művelet futtatott és sikeres volt-e" volt, függetlenül attól, hogy hányszor tooexecute művelet, hogy hello előző művelet sikeres volt, alapján nem sikerült, vagy kihagytuk kívánta.</span><span class="sxs-lookup"><span data-stu-id="c8af6-119">hello `dependsOn` property was synonymous with "hello action ran and was successful", no matter how many times you wanted tooexecute an action, based on whether hello previous action was successful, failed, or skipped.</span></span> <span data-ttu-id="c8af6-120">Hello `runAfter` tulajdonság olyan objektum, amely megadja az összes hello művelet nevek elteltével hello objektum fut, hogy rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="c8af6-120">hello `runAfter` property provides that flexibility as an object that specifies all hello action names after which hello object runs.</span></span> <span data-ttu-id="c8af6-121">Ez a tulajdonság is megadja elfogadható eseményindítóként használható állapotait tömbjét.</span><span class="sxs-lookup"><span data-stu-id="c8af6-121">This property also defines an array of statuses that are acceptable as triggers.</span></span> <span data-ttu-id="c8af6-122">Például ha toorun után lépést A sikeres és is után B lépésben sikeres vagy sikertelen lesz, hozhat létre a `runAfter` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="c8af6-122">For example, if you wanted toorun after step A succeeds and also after step B succeeds or fails, you construct this `runAfter` property:</span></span>

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a><span data-ttu-id="c8af6-123">A séma frissítése</span><span class="sxs-lookup"><span data-stu-id="c8af6-123">Upgrade your schema</span></span>

<span data-ttu-id="c8af6-124">Toohello frissítése új séma mindössze néhány lépést.</span><span class="sxs-lookup"><span data-stu-id="c8af6-124">Upgrading toohello new schema only takes a few steps.</span></span> <span data-ttu-id="c8af6-125">hello frissítési folyamat tartalmaz hello frissítési parancsprogram, futtatása mentése új logikai alkalmazás, és ha azt szeretné, valószínűleg felülírja a hello előző logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8af6-125">hello upgrade process includes running hello upgrade script, saving as a new logic app, and if you want, possibly overwriting hello previous logic app.</span></span>

1. <span data-ttu-id="c8af6-126">Hello Azure-portálon nyissa meg a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8af6-126">In hello Azure portal, open your logic app.</span></span>

2. <span data-ttu-id="c8af6-127">Nyissa meg túl**áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="c8af6-127">Go too**Overview**.</span></span> <span data-ttu-id="c8af6-128">Hello logic app eszköztáron válassza **frissítés séma**.</span><span class="sxs-lookup"><span data-stu-id="c8af6-128">On hello logic app toolbar, choose **Update Schema**.</span></span>
   
    ![Válassza ki a séma frissítése][1]
   
    <span data-ttu-id="c8af6-130">hello frissített definíciós ad vissza, amelyet másolja és illessze be az erőforrás-definíció szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="c8af6-130">hello upgraded definition is returned, which you can copy and paste into a resource definition if necessary.</span></span> 
    <span data-ttu-id="c8af6-131">Azonban azt **erősen ajánlott** választja **Mentés másként** toomake meg arról, hogy az összes kapcsolat hivatkozások érvényes hello a logikai alkalmazás frissítése.</span><span class="sxs-lookup"><span data-stu-id="c8af6-131">However, we **strongly recommend** you choose **Save As** toomake sure that all connection references are valid in hello upgraded logic app.</span></span>

3. <span data-ttu-id="c8af6-132">Hello frissítési panel eszköztárán válassza **Mentés másként**.</span><span class="sxs-lookup"><span data-stu-id="c8af6-132">In hello upgrade blade toolbar, choose **Save As**.</span></span>

4. <span data-ttu-id="c8af6-133">Adja meg a hello logika nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="c8af6-133">Enter hello logic name and status.</span></span> <span data-ttu-id="c8af6-134">toodeploy a frissített Logic Apps alkalmazást, válassza a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c8af6-134">toodeploy your upgraded logic app, choose **Create**.</span></span>

5. <span data-ttu-id="c8af6-135">Győződjön meg arról, hogy a frissített logikai alkalmazás megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="c8af6-135">Confirm that your upgraded logic app works as expected.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c8af6-136">Ha manuális vagy kérelem eseményindítót használ, hello visszahívási URL-címet az új logikai alkalmazás változik.</span><span class="sxs-lookup"><span data-stu-id="c8af6-136">If you are using a manual or request trigger, hello callback URL changes in your new logic app.</span></span> <span data-ttu-id="c8af6-137">Teszt hello új URL-cím toomake meg arról, hogy hello-végpont működik tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="c8af6-137">Test hello new URL toomake sure hello end-to-end experience works.</span></span> <span data-ttu-id="c8af6-138">toopreserve előző URL-címek, klónozhat keresztül a meglévő Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8af6-138">toopreserve previous URLs, you can clone over your existing logic app.</span></span>

6. <span data-ttu-id="c8af6-139">*Nem kötelező* toooverwrite a korábbi Logic Apps alkalmazást hello új séma verzióra, a hello eszköztárán válassza **Klónozás**, tovább túl**frissítés séma**.</span><span class="sxs-lookup"><span data-stu-id="c8af6-139">*Optional* toooverwrite your previous logic app with hello new schema version, on hello toolbar, choose **Clone**, next too**Update Schema**.</span></span> <span data-ttu-id="c8af6-140">Ez a lépés nem szükséges csak akkor, ha azt szeretné, tookeep hello azonos erőforrás azonosítója, vagy kérjen indítási URL-CÍMÉT a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8af6-140">This step is necessary only if you want tookeep hello same resource ID or request trigger URL of your logic app.</span></span>

### <a name="upgrade-tool-notes"></a><span data-ttu-id="c8af6-141">Frissítési eszköz megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c8af6-141">Upgrade tool notes</span></span>

#### <a name="mapping-conditions"></a><span data-ttu-id="c8af6-142">Leképezési feltételek</span><span class="sxs-lookup"><span data-stu-id="c8af6-142">Mapping conditions</span></span>

<span data-ttu-id="c8af6-143">Hello frissített definícióban hello eszköz lehetővé teszi egy, a true és false fiókirodai műveletek csoportosítása hatóköreként a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="c8af6-143">In hello upgraded definition, hello tool makes a best effort at grouping true and false branch actions together as a scope.</span></span> <span data-ttu-id="c8af6-144">Pontosabban hello Tervező mintáját `@equals(actions('a').status, 'Skipped')` meg kell jelennie egy `else` művelet.</span><span class="sxs-lookup"><span data-stu-id="c8af6-144">Specifically, hello designer pattern of `@equals(actions('a').status, 'Skipped')` should appear as an `else` action.</span></span> <span data-ttu-id="c8af6-145">Azonban hello eszköz felismerhetetlen minták észleli, ha hello eszköz előfordulhat, hogy hozzon létre külön feltételek hello IGAZ és hamis ág hello.</span><span class="sxs-lookup"><span data-stu-id="c8af6-145">However, if hello tool detects unrecognizable patterns, hello tool might create separate conditions for both hello true and hello false branch.</span></span> <span data-ttu-id="c8af6-146">Műveletek leképezheti a frissítés után is, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8af6-146">You can remap actions after upgrading, if necessary.</span></span>

#### <a name="foreach-loop-with-condition"></a><span data-ttu-id="c8af6-147">feltételt tartalmazó "foreach" hurok</span><span class="sxs-lookup"><span data-stu-id="c8af6-147">'foreach' loop with condition</span></span>

<span data-ttu-id="c8af6-148">Hello új sémában, használhatja a hello szűrő művelet tooreplicate hello mintáját a `foreach` elemenként, eltérő feltételt tartalmazó hurok, de ezt a változtatást automatikusan történjen, amikor frissít.</span><span class="sxs-lookup"><span data-stu-id="c8af6-148">In hello new schema, you can use hello filter action tooreplicate hello pattern of a `foreach` loop with a condition per item, but this change should automatically happen when you upgrade.</span></span> <span data-ttu-id="c8af6-149">hello feltétel előtt hello foreach hurok hello feltételének elemek csak egy tömb visszaküldésére használatos szűrőművelet válik, és, hogy tömb átad a hello foreach művelet.</span><span class="sxs-lookup"><span data-stu-id="c8af6-149">hello condition becomes a filter action before hello foreach loop for returning only an array of items that match hello condition, and that array is passed into hello foreach action.</span></span> <span data-ttu-id="c8af6-150">Egy vonatkozó példáért lásd: [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="c8af6-150">For an example, see [Loops and scopes](../logic-apps/logic-apps-loops-and-scopes.md).</span></span>

#### <a name="resource-tags"></a><span data-ttu-id="c8af6-151">Az erőforráscímkék</span><span class="sxs-lookup"><span data-stu-id="c8af6-151">Resource tags</span></span>

<span data-ttu-id="c8af6-152">A frissítés befejezése után az erőforráscímkék eltávolítja, így kell visszaállítani őket frissített hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="c8af6-152">After you upgrade, resource tags are removed, so you must reset them for hello upgraded workflow.</span></span>

## <a name="other-changes"></a><span data-ttu-id="c8af6-153">Az egyéb módosítások</span><span class="sxs-lookup"><span data-stu-id="c8af6-153">Other changes</span></span>

### <a name="renamed-manual-trigger-toorequest-trigger"></a><span data-ttu-id="c8af6-154">Átnevezett "manual" eseményindító too'request "eseményindító</span><span class="sxs-lookup"><span data-stu-id="c8af6-154">Renamed 'manual' trigger too'request' trigger</span></span>

<span data-ttu-id="c8af6-155">Hello `manual` indítási típus elavult, és a neve túl`request` típusú `http`.</span><span class="sxs-lookup"><span data-stu-id="c8af6-155">hello `manual` trigger type was deprecated and renamed too`request` with type `http`.</span></span> <span data-ttu-id="c8af6-156">Ez a módosítás több konzisztencia hoz létre, a mintát, amely eseményindító hello hello típusú használt toobuild.</span><span class="sxs-lookup"><span data-stu-id="c8af6-156">This change creates more consistency for hello kind of pattern that hello trigger is used toobuild.</span></span>

### <a name="new-filter-action"></a><span data-ttu-id="c8af6-157">Új "filter" művelet</span><span class="sxs-lookup"><span data-stu-id="c8af6-157">New 'filter' action</span></span>

<span data-ttu-id="c8af6-158">egy nagy tömböt le meg tooa kevesebb elemek, új hello toofilter `filter` típusú tömb és egy feltétel fogad, hello feltétel minden elemhez kiértékeli, és hello feltétel értekezlet elemekhez tömböt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="c8af6-158">toofilter a large array down tooa smaller set of items, hello new `filter` type accepts an array and a condition, evaluates hello condition for each item, and returns an array with items meeting hello condition.</span></span>

### <a name="restrictions-for-foreach-and-until-actions"></a><span data-ttu-id="c8af6-159">A "foreach" és "csak" műveletek korlátozása</span><span class="sxs-lookup"><span data-stu-id="c8af6-159">Restrictions for 'foreach' and 'until' actions</span></span>

<span data-ttu-id="c8af6-160">Hello `foreach` és `until` hurok korlátozott tooa egyetlen művelettel vannak.</span><span class="sxs-lookup"><span data-stu-id="c8af6-160">hello `foreach` and `until` loop are restricted tooa single action.</span></span>

### <a name="new-trackedproperties-for-actions"></a><span data-ttu-id="c8af6-161">Új "trackedProperties" műveletek</span><span class="sxs-lookup"><span data-stu-id="c8af6-161">New 'trackedProperties' for actions</span></span>

<span data-ttu-id="c8af6-162">Műveletek is most már rendelkezik egy további tulajdonság nevű `trackedProperties`, vagyis testvér toohello `runAfter` és `type` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="c8af6-162">Actions can now have an additional property called `trackedProperties`, which is sibling toohello `runAfter` and `type` properties.</span></span> <span data-ttu-id="c8af6-163">Ezen az objektumon bizonyos művelet bemeneti határozza meg, és kiírja, hogy szeretné-e a hello Azure diagnosztikai telemetriai, egy munkafolyamat részeként kibocsátott tooinclude.</span><span class="sxs-lookup"><span data-stu-id="c8af6-163">This object specifies certain action inputs or outputs that you want tooinclude in hello Azure Diagnostic telemetry, emitted as part of a workflow.</span></span> <span data-ttu-id="c8af6-164">Példa:</span><span class="sxs-lookup"><span data-stu-id="c8af6-164">For example:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c8af6-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8af6-165">Next Steps</span></span>
* [<span data-ttu-id="c8af6-166">A logic apps munkafolyamat-meghatározások létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8af6-166">Create workflow definitions for logic apps</span></span>](../logic-apps/logic-apps-author-definitions.md)
* [<span data-ttu-id="c8af6-167">A logic Apps alkalmazások központi telepítési sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8af6-167">Create deployment templates for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
