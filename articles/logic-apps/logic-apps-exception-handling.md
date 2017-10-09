---
title: "aaaError & kivételkezelést - Azure Logic Apps |} Microsoft Docs"
description: "Hiba és az Azure Logic Apps kivételkezelő minták"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 326a252310c8dfb154e583f91c9421675e448d1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a><span data-ttu-id="888d3-103">Hibákat és kivételeket az Azure Logic Apps alkalmazásokat kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="888d3-103">Handle errors and exceptions in Azure Logic Apps</span></span>

<span data-ttu-id="888d3-104">Az Azure Logic Apps hatékony eszközöket biztosít, és minták toohelp, győződjön meg arról a Integrációk hatékony és rugalmas-hibákkal szemben.</span><span class="sxs-lookup"><span data-stu-id="888d3-104">Azure Logic Apps provides rich tools and patterns toohelp you make sure your integrations are robust and resilient against failures.</span></span> <span data-ttu-id="888d3-105">Minden integrációs architektúra hello dokumentációkat az, hogy tooappropriately leíró állásidőt jelent, vagy a függő rendszerekről kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="888d3-105">Any integration architecture poses hello challenge of making sure tooappropriately handle downtime or issues from dependent systems.</span></span> <span data-ttu-id="888d3-106">Logic Apps remek hibák kezelése a kiváló felhasználói élmény, felkínálva hello eszközökkel tooact kivételeket és a hibák esetén a munkafolyamatokban.</span><span class="sxs-lookup"><span data-stu-id="888d3-106">Logic Apps makes handling errors a first-class experience, giving you hello tools you need tooact on exceptions and errors in your workflows.</span></span>

## <a name="retry-policies"></a><span data-ttu-id="888d3-107">Ismételje meg a házirendek</span><span class="sxs-lookup"><span data-stu-id="888d3-107">Retry policies</span></span>

<span data-ttu-id="888d3-108">Újrapróbálkozási házirendje egy kivétel és hibakezelés legalapvetőbb típusú hello.</span><span class="sxs-lookup"><span data-stu-id="888d3-108">A retry policy is hello most basic type of exception and error handling.</span></span> <span data-ttu-id="888d3-109">Ha az eredeti kérelem túllépte az időkorlátot, vagy sikertelen volt (a kérelmet, amely egy 429 eredményez vagy 5XX típusú válasz érkezett), ez a házirend határozza meg, hogy újra kell-e a hello művelet.</span><span class="sxs-lookup"><span data-stu-id="888d3-109">If an initial request timed out or failed (any request that results in a 429 or 5xx response), this policy defines whether hello action should retry.</span></span> <span data-ttu-id="888d3-110">Alapértelmezés szerint minden további 4 alkalommal keresztül 20 másodperces időközönként próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="888d3-110">By default, all actions retry 4 additional times over 20-second intervals.</span></span> <span data-ttu-id="888d3-111">Igen, ha hello első kérést kap egy `500 Internal Server Error` választ, 20 másodperc szünet hello munkafolyamat-motor és kísérletek hello kérelem újra.</span><span class="sxs-lookup"><span data-stu-id="888d3-111">So if hello first request receives a `500 Internal Server Error` response, hello workflow engine pauses for 20 seconds, and attempts hello request again.</span></span> <span data-ttu-id="888d3-112">Ha az összes újrapróbálkozás után hello művelet még mindig egy kivétel vagy sikertelen volt, hello munkafolyamat továbbra is fennáll, és jelek hello művelet állapotának `Failed`.</span><span class="sxs-lookup"><span data-stu-id="888d3-112">If after all retries, hello response is still an exception or failure, hello workflow continues and marks hello action status as `Failed`.</span></span>

<span data-ttu-id="888d3-113">Újrapróbálkozási házirendeket konfigurálhat a hello **bemenetek** művelet esetén.</span><span class="sxs-lookup"><span data-stu-id="888d3-113">You can configure retry policies in hello **inputs** for a particular action.</span></span> <span data-ttu-id="888d3-114">Például konfigurálása egy újrapróbálkozási házirend tootry 4 alkalommal akár 1 órás időközönként keresztül</span><span class="sxs-lookup"><span data-stu-id="888d3-114">For example, you can configure a retry policy tootry as many as 4 times over 1-hour intervals.</span></span> <span data-ttu-id="888d3-115">Teljes bemeneti tulajdonságai, lásd: [munkafolyamat-műveleteket és eseményindítók][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="888d3-115">For full details about input properties, see [Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

<span data-ttu-id="888d3-116">Ha a HTTP-művelet tooretry 4 alkalommal kívánta, és várjon 10 percet egyes kísérletek közötti, használja a következő definícióját hello:</span><span class="sxs-lookup"><span data-stu-id="888d3-116">If you wanted your HTTP action tooretry 4 times and wait 10 minutes between each attempt, you would use hello following definition:</span></span>

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

<span data-ttu-id="888d3-117">A támogatott szintaxis további információkért lásd: hello [újrapróbálkozási-házirend a szakasz a munkafolyamat-műveleteket és eseményindítók][retryPolicyMSDN].</span><span class="sxs-lookup"><span data-stu-id="888d3-117">For more information on supported syntax, see hello [retry-policy section in Workflow Actions and Triggers][retryPolicyMSDN].</span></span>

## <a name="catch-failures-with-hello-runafter-property"></a><span data-ttu-id="888d3-118">A tényleges hibákat is hello RunAfter tulajdonság</span><span class="sxs-lookup"><span data-stu-id="888d3-118">Catch failures with hello RunAfter property</span></span>

<span data-ttu-id="888d3-119">Minden logikai alkalmazás művelet kijelenti, hogy milyen műveleteket csak befejezése előtt hello művelet, például a munkafolyamat hello lépéseket rendezés.</span><span class="sxs-lookup"><span data-stu-id="888d3-119">Each logic app action declares which actions must finish before hello action starts, like ordering hello steps in your workflow.</span></span> <span data-ttu-id="888d3-120">Hello művelet definícióban, ebben a rendezése az úgynevezett hello `runAfter` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="888d3-120">In hello action definition, this ordering is known as hello `runAfter` property.</span></span> <span data-ttu-id="888d3-121">Ez a tulajdonság olyan objektum, amely leírja, milyen műveleteket és a művelet állapotok hello művelet hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="888d3-121">This property is an object that describes which actions and action statuses execute hello action.</span></span> <span data-ttu-id="888d3-122">Alapértelmezés szerint hozzáadva hello Logic App Designer minden művelet túl van beállítva`runAfter` hello előző lépést, ha hello előző lépésben `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="888d3-122">By default, all actions added through hello Logic App Designer are set too`runAfter` hello previous step if hello previous step `Succeeded`.</span></span> <span data-ttu-id="888d3-123">Azonban testre szabhatja a érték toofire műveletek során előző műveleteit `Failed`, `Skipped`, vagy egy lehetséges ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="888d3-123">However, you can customize this value toofire actions when previous actions have `Failed`, `Skipped`, or a possible set of these values.</span></span> <span data-ttu-id="888d3-124">Ha tooadd egy elem tooa Service Bus-témakörbe kijelölt egy adott művelet után `Insert_Row` meghiúsul, használhatja a következő hello `runAfter` konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="888d3-124">If you wanted tooadd an item tooa designated Service Bus topic after a specific action `Insert_Row` fails, you could use hello following `runAfter` configuration:</span></span>

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

<span data-ttu-id="888d3-125">Értesítés hello `runAfter` tulajdonság értéke toofire, ha hello `Insert_Row` művelet `Failed`.</span><span class="sxs-lookup"><span data-stu-id="888d3-125">Notice hello `runAfter` property is set toofire if hello `Insert_Row` action is `Failed`.</span></span> <span data-ttu-id="888d3-126">Ha hello művelet állapota toorun hello művelet `Succeeded`, `Failed`, vagy `Skipped`, a következő szintaxist használja:</span><span class="sxs-lookup"><span data-stu-id="888d3-126">toorun hello action if hello action status is `Succeeded`, `Failed`, or `Skipped`, use this syntax:</span></span>

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> <span data-ttu-id="888d3-127">Műveletek futtatásához, és sikeresen befejeződik, miután a fenti művelet sikertelen volt, fel van tüntetve `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="888d3-127">Actions that run and complete successfully after a preceding action has failed, are marked as `Succeeded`.</span></span> <span data-ttu-id="888d3-128">A viselkedés azt jelenti, hogy azt a munkafolyamat sikeresen általános hibák hello futtassa saját magát a jelölés szerint `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="888d3-128">This behavior means that if you successfully catch all failures in a workflow, hello run itself is marked as `Succeeded`.</span></span>

## <a name="scopes-and-results-tooevaluate-actions"></a><span data-ttu-id="888d3-129">Hatókörök és az eredmények tooevaluate műveletek</span><span class="sxs-lookup"><span data-stu-id="888d3-129">Scopes and results tooevaluate actions</span></span>

<span data-ttu-id="888d3-130">Egyes műveletek után futtathatja hasonló toohow, is csoportosíthatja műveletek belül egy [hatókör](../logic-apps/logic-apps-loops-and-scopes.md), amely összekötőként műveletek logikai csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="888d3-130">Similar toohow you can run after individual actions, you can also group actions together inside a [scope](../logic-apps/logic-apps-loops-and-scopes.md), which act as a logical grouping of actions.</span></span> <span data-ttu-id="888d3-131">Hatókörök hasznosak a logic app műveletek sorolására, mind a hatókör hello állapot összesített értékelések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="888d3-131">Scopes are useful both for organizing your logic app actions, and for performing aggregate evaluations on hello status of a scope.</span></span> <span data-ttu-id="888d3-132">hello hatókör magát egy állapot kap, a hatókör összes művelet után.</span><span class="sxs-lookup"><span data-stu-id="888d3-132">hello scope itself receives a status after all actions in a scope have finished.</span></span> <span data-ttu-id="888d3-133">hello hatókör határozza meg a hello ugyanazok a feltételek egy Futtatás mint.</span><span class="sxs-lookup"><span data-stu-id="888d3-133">hello scope status is determined with hello same criteria as a run.</span></span> <span data-ttu-id="888d3-134">Ha hello végső egy végrehajtási fiókirodai művelete `Failed` vagy `Aborted`, hello állapota `Failed`.</span><span class="sxs-lookup"><span data-stu-id="888d3-134">If hello final action in an execution branch is `Failed` or `Aborted`, hello status is `Failed`.</span></span>

<span data-ttu-id="888d3-135">az összes hiba történt a hello hatókörén belül toofire bizonyos műveleteket, használhatja `runAfter` van megjelölve hatókörrel rendelkező `Failed`.</span><span class="sxs-lookup"><span data-stu-id="888d3-135">toofire specific actions for any failures that happened within hello scope, you can use `runAfter` with a scope that is marked `Failed`.</span></span> <span data-ttu-id="888d3-136">Ha *bármely* hello hatókörében művelet sikertelen, a hatókör sikertelen megadható, hogy fut létrehozni egy egyetlen művelettel toocatch hibák.</span><span class="sxs-lookup"><span data-stu-id="888d3-136">If *any* actions in hello scope fail, running after a scope fails lets you create a single action toocatch failures.</span></span>

### <a name="getting-hello-context-of-failures-with-results"></a><span data-ttu-id="888d3-137">Hibák az eredményekkel beolvasásakor hello környezetben</span><span class="sxs-lookup"><span data-stu-id="888d3-137">Getting hello context of failures with results</span></span>

<span data-ttu-id="888d3-138">Bár a hatókörből hibák elfogja akkor hasznos, érdemes lehet környezet toohelp, hogy tudomásul veszi, hogy pontosan mely műveletek nem sikerült, és az esetleges hibákat vagy a visszaadott eredményobjektumokban tárolt állapotkódok.</span><span class="sxs-lookup"><span data-stu-id="888d3-138">Although catching failures from a scope is useful, you might also want context toohelp you understand exactly which actions failed, and any errors or status codes that were returned.</span></span> <span data-ttu-id="888d3-139">Hello `@result()` munkafolyamat-funkció biztosít a hatókör összes művelet eredménye hello kapcsolatos környezetben.</span><span class="sxs-lookup"><span data-stu-id="888d3-139">hello `@result()` workflow function provides context about hello result of all actions in a scope.</span></span>

<span data-ttu-id="888d3-140">`@result()`egy egyetlen paramétert, a hatókör neve időt vesz igénybe, és minden hello művelet eredményeinek hatókörön belüli tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="888d3-140">`@result()` takes a single parameter, scope name, and returns an array of all hello action results from within that scope.</span></span> <span data-ttu-id="888d3-141">E művelet objektumok például ugyanaz, mint hello attribútumok hello `@actions()` objektum, például művelet kezdési ideje, a művelet befejezése, a művelet állapota, a művelet bemeneti, a művelet közötti összefüggést szemléltető azonosítók és a művelet kimenetében.</span><span class="sxs-lookup"><span data-stu-id="888d3-141">These action objects include hello same attributes as hello `@actions()` object, including action start time, action end time, action status, action inputs, action correlation IDs, and action outputs.</span></span> <span data-ttu-id="888d3-142">bármely művelet, amelyet nem sikerült egy hatókörön belüli toosend környezetében, hogy könnyen összepárosíthassa egy `@result()` működik egy `runAfter`.</span><span class="sxs-lookup"><span data-stu-id="888d3-142">toosend context of any actions that failed within a scope, you can easily pair an `@result()` function with a `runAfter`.</span></span>

<span data-ttu-id="888d3-143">tooexecute művelet *minden* művelet hatókörben, amely `Failed`, eredmények tooactions szűrő hello tömbje, amely nem sikerült, akkor párosítható `@result()` rendelkező egy  **[szűrő tömb](../connectors/connectors-native-query.md)**  műveletet és egy  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  hurok.</span><span class="sxs-lookup"><span data-stu-id="888d3-143">tooexecute an action *for each* action in a scope that `Failed`, filter hello array of results tooactions that failed, you can pair `@result()` with a **[Filter Array](../connectors/connectors-native-query.md)** action and a **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)** loop.</span></span> <span data-ttu-id="888d3-144">Hello szűrt eredmény tömb igénybe vehet, és egy műveletet hello használata minden egyes hibához tartozó **ForEach** hurok.</span><span class="sxs-lookup"><span data-stu-id="888d3-144">You can take hello filtered result array and perform an action for each failure using hello **ForEach** loop.</span></span> <span data-ttu-id="888d3-145">Íme egy példa, majd részletesen ismerteti, hello adott válasz törzsének bármely művelet, amelyet nem sikerült a HTTP POST kérést küldő hello hatókörén belül `My_Scope`.</span><span class="sxs-lookup"><span data-stu-id="888d3-145">Here's an example, followed by a detailed explanation, that sends an HTTP POST request with hello response body of any actions that failed within hello scope `My_Scope`.</span></span>

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

<span data-ttu-id="888d3-146">A részletes forgatókönyv toodescribe, mi történik, a következő:</span><span class="sxs-lookup"><span data-stu-id="888d3-146">Here's a detailed walkthrough toodescribe what happens:</span></span>

1. <span data-ttu-id="888d3-147">belül minden művelet eredményét tooget hello `My_Scope`, hello **szűrő tömb** művelet szűrők `@result('My_Scope')`.</span><span class="sxs-lookup"><span data-stu-id="888d3-147">tooget hello result of all actions within `My_Scope`, hello **Filter Array** action filters `@result('My_Scope')`.</span></span>

2. <span data-ttu-id="888d3-148">feltétel hello **szűrő tömb** tetszőleges `@result()` állapota egyenlő túl elem`Failed`.</span><span class="sxs-lookup"><span data-stu-id="888d3-148">hello condition for **Filter Array** is any `@result()` item that has status equal too`Failed`.</span></span> <span data-ttu-id="888d3-149">Ez a feltétel az összes művelet eredményeinek hello tömb szűrők `My_Scope` csak tooan tömb nem sikerült a művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="888d3-149">This condition filters hello array with all action results from `My_Scope` tooan array with only failed action results.</span></span>

3. <span data-ttu-id="888d3-150">Hajtsa végre a **minden** hello művelete **szűrt tömb** kimenete.</span><span class="sxs-lookup"><span data-stu-id="888d3-150">Perform a **For Each** action on hello **Filtered Array** outputs.</span></span> <span data-ttu-id="888d3-151">Ezt a lépést végrehajt egy műveletet *minden* korábban szűrt művelet eredménye nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="888d3-151">This step performs an action *for each* failed action result that was previously filtered.</span></span>

    <span data-ttu-id="888d3-152">Ha hello hatókörében egyetlen művelet sikertelen volt, a hello műveletek hello `foreach` csak egyszer futnak le.</span><span class="sxs-lookup"><span data-stu-id="888d3-152">If a single action in hello scope failed, hello actions in hello `foreach` run only once.</span></span> 
    <span data-ttu-id="888d3-153">Sok sikertelen műveletek miatt egy művelet hibája /.</span><span class="sxs-lookup"><span data-stu-id="888d3-153">Many failed actions cause one action per failure.</span></span>

4. <span data-ttu-id="888d3-154">Küldjön egy HTTP POST hello `foreach` adott válasz törzsének, elem vagy `@item()['outputs']['body']`.</span><span class="sxs-lookup"><span data-stu-id="888d3-154">Send an HTTP POST on hello `foreach` item response body, or `@item()['outputs']['body']`.</span></span> <span data-ttu-id="888d3-155">Hello `@result()` elem alakzat van hello ugyanaz, mint hello `@actions()` alakul, és a program értelmezni tudja hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="888d3-155">hello `@result()` item shape is hello same as hello `@actions()` shape, and can be parsed hello same way.</span></span>

5. <span data-ttu-id="888d3-156">Közé tartoznak a két egyéni fejlécek hello sikertelen művelet nevű `@item()['name']` hello sikertelen volt, mert a nyomkövetési azonosító futtatási ügyfél `@item()['clientTrackingId']`.</span><span class="sxs-lookup"><span data-stu-id="888d3-156">Include two custom headers with hello failed action name `@item()['name']` and hello failed run client tracking ID `@item()['clientTrackingId']`.</span></span>

<span data-ttu-id="888d3-157">Referenciaként Íme egy példa `@result()` elemet ábrázoló hello `name`, `body`, és `clientTrackingId` hello előző példában szereplő elemzésének tulajdonságokhoz.</span><span class="sxs-lookup"><span data-stu-id="888d3-157">For reference, here's an example of a single `@result()` item, showing hello `name`, `body`, and `clientTrackingId` properties that are parsed in hello previous example.</span></span> <span data-ttu-id="888d3-158">Kívüli egy `foreach`, `@result()` ezek az objektumok tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="888d3-158">Outside of a `foreach`, `@result()` returns an array of these objects.</span></span>

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

<span data-ttu-id="888d3-159">tooperform különböző kivételkezelést kombinációját, előzőleg bemutatott hello kifejezéseket is használhat.</span><span class="sxs-lookup"><span data-stu-id="888d3-159">tooperform different exception handling patterns, you can use hello expressions shown previously.</span></span> <span data-ttu-id="888d3-160">Előfordulhat, hogy válassza ki a tooexecute egyetlen kivételkezelő, amely fogadja a hibák szűrt tömb teljes hello hello hatókörén kívüli művelet, és távolítsa el a hello `foreach`.</span><span class="sxs-lookup"><span data-stu-id="888d3-160">You might choose tooexecute a single exception handling action outside hello scope that accepts hello entire filtered array of failures, and remove hello `foreach`.</span></span> <span data-ttu-id="888d3-161">Hello más fontos tulajdonságait is használható `@result()` előzőleg bemutatott válasz.</span><span class="sxs-lookup"><span data-stu-id="888d3-161">You can also include other useful properties from hello `@result()` response shown previously.</span></span>

## <a name="azure-diagnostics-and-telemetry"></a><span data-ttu-id="888d3-162">Az Azure Diagnostics- és telemetriabevitelt</span><span class="sxs-lookup"><span data-stu-id="888d3-162">Azure Diagnostics and telemetry</span></span>

<span data-ttu-id="888d3-163">hello előző mintától kiváló módja toohandle hibákat és kivételeket futtató belül, de is azonosíthatja, és futtassa maga hello független tooerrors válaszol.</span><span class="sxs-lookup"><span data-stu-id="888d3-163">hello previous patterns are great way toohandle errors and exceptions within a run, but you can also identify and respond tooerrors independent of hello run itself.</span></span> 
<span data-ttu-id="888d3-164">[Az Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) biztosít egy egyszerű módon toosend összes munkafolyamat (beleértve az összes futtató és a művelet állapotának) események tooan Azure Storage-fiók vagy egy Azure Event hubs Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="888d3-164">[Azure Diagnostics](../logic-apps/logic-apps-monitor-your-logic-apps.md) provides a simple way toosend all workflow events (including all run and action statuses) tooan Azure Storage account or an Azure Event Hub.</span></span> <span data-ttu-id="888d3-165">tooevaluate állapotok futtassa, hello naplók és a metrikák figyelése, vagy közzététele inkább bármely felügyeleti eszközt.</span><span class="sxs-lookup"><span data-stu-id="888d3-165">tooevaluate run statuses, you can monitor hello logs and metrics, or publish them into any monitoring tool you prefer.</span></span> <span data-ttu-id="888d3-166">Egy lehetséges beállítás esetén toostream keresztül Azure Event hubs Eseményközpontot, az összes hello események [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="888d3-166">One potential option is toostream all hello events through Azure Event Hub into [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> <span data-ttu-id="888d3-167">A Stream Analytics írhat élő lekérdezések ki bármely rendellenességek észlelését, átlagok vagy hibák hello a diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="888d3-167">In Stream Analytics, you can write live queries off any anomalies, averages, or failures from hello diagnostic logs.</span></span> <span data-ttu-id="888d3-168">A Stream Analytics könnyen kimenetre küldheti például várólisták, témakörök, SQL, Azure Cosmos DB és a Power BI tooother adatforrások.</span><span class="sxs-lookup"><span data-stu-id="888d3-168">Stream Analytics can easily output tooother data sources like queues, topics, SQL, Azure Cosmos DB, and Power BI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="888d3-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="888d3-169">Next Steps</span></span>

* [<span data-ttu-id="888d3-170">Tekintse meg, hogyan ügyfél buildek hiba történt az Azure Logic Apps kezelése</span><span class="sxs-lookup"><span data-stu-id="888d3-170">See how a customer builds error handling with Azure Logic Apps</span></span>](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [<span data-ttu-id="888d3-171">További Logic Apps példák és forgatókönyvek keresése</span><span class="sxs-lookup"><span data-stu-id="888d3-171">Find more Logic Apps examples and scenarios</span></span>](../logic-apps/logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="888d3-172">Ismerje meg, hogyan toocreate automatikus logic Apps alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="888d3-172">Learn how toocreate automated deployments for logic apps</span></span>](../logic-apps/logic-apps-create-deploy-template.md)
* [<span data-ttu-id="888d3-173">Logikai alkalmazások létrehozása és üzembe helyezése a Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="888d3-173">Build and deploy logic apps with Visual Studio</span></span>](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
