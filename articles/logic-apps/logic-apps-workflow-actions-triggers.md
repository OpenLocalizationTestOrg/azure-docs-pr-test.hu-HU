---
title: "Munkafolyamat-műveleteket és eseményindítók - Azure Logic Apps |} Microsoft Docs"
description: 
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bd3f1d225b974ebde889738bb435825658d1e1e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="632b7-102">Munkafolyamat-műveleteket, és az Azure Logic Apps eseményindítók</span><span class="sxs-lookup"><span data-stu-id="632b7-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="632b7-103">A Logic apps eseményindítók és műveletek alkotják.</span><span class="sxs-lookup"><span data-stu-id="632b7-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="632b7-104">Nincsenek eseményindítók hat típusú.</span><span class="sxs-lookup"><span data-stu-id="632b7-104">There are six types of triggers.</span></span> <span data-ttu-id="632b7-105">Minden különböző felület és a különböző viselkedés rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="632b7-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="632b7-106">Részleteinek megtekintésével is olvashat további részleteket a [Munkafolyamatdefiníciós nyelve](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="632b7-106">You can also learn about other details by looking at the details of the [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="632b7-107">Olvassa el a további információk eseményindítók és műveletek és hogyan használhatja azokat az üzleti folyamatokat és a munkafolyamatok javítása érdekében a logic Apps alkalmazásokat készíthet.</span><span class="sxs-lookup"><span data-stu-id="632b7-107">Read on to learn more about triggers and actions and how you might use them to build logic apps to improve your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="632b7-108">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="632b7-108">Triggers</span></span>  

<span data-ttu-id="632b7-109">Egy eseményindító határozza meg a hívások is kezdeményezhető a logic app munkafolyamat futását.</span><span class="sxs-lookup"><span data-stu-id="632b7-109">A trigger specifies the calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="632b7-110">Az alábbiakban kezdeményezheti a munkafolyamatot futtató két különböző módon:</span><span class="sxs-lookup"><span data-stu-id="632b7-110">Here are the two different ways to initiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="632b7-111">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-111">A polling trigger</span></span>  

-   <span data-ttu-id="632b7-112">A leküldéses eseményindító - meghívásával a [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="632b7-112">A push trigger - by calling the [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="632b7-113">Indítók a legfelső szintű elemet tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="632b7-113">All triggers contain these top-level elements:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property to create runs for>",
    "operationOptions": "<operation options on the trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="632b7-114">Az eseményindítók típusai és azok</span><span class="sxs-lookup"><span data-stu-id="632b7-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="632b7-115">Az ilyen típusú eseményindítók használhatja:</span><span class="sxs-lookup"><span data-stu-id="632b7-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="632b7-116">**Kérelem** \- lehetővé teszi a logikai alkalmazást hívja meg a végpont</span><span class="sxs-lookup"><span data-stu-id="632b7-116">**Request** \- Makes the logic app an endpoint for you to call</span></span>  
  
-   <span data-ttu-id="632b7-117">**Ismétlődés** \- következik be a meghatározott ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="632b7-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="632b7-118">**HTTP** \- HTTP webes végpont kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="632b7-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="632b7-119">A HTTP-végpont meg kell felelnie egy meghatározott eseményindító szerződés \- használva egy 202\-aszinkron mintát, vagy egy tömb vissza</span><span class="sxs-lookup"><span data-stu-id="632b7-119">The HTTP endpoint must conform to a specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="632b7-120">**ApiConnection** \- indul el, mint a HTTP szavazások, azonban kihasználja a [Microsoft által felügyelt API-k](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="632b7-120">**ApiConnection** \- Polls like the HTTP trigger, however, it takes advantage of the [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="632b7-121">**HTTPWebhook** \- megnyit egy végpontot, hasonló manuális eseményindító, azonban ez szükségessé teszi a megadott URL-címhez regisztrálásához és</span><span class="sxs-lookup"><span data-stu-id="632b7-121">**HTTPWebhook** \- Opens an endpoint, similar to the Manual trigger, however, it also calls out to a specified URL to register and unregister</span></span>  
  
-   <span data-ttu-id="632b7-122">**ApiConnectionWebhook** \- Operates hasonlóan a HTTPWebhook eseményindító, ha kihasználja a Microsoft által felügyelt API-k</span><span class="sxs-lookup"><span data-stu-id="632b7-122">**ApiConnectionWebhook** \- Operates like the HTTPWebhook trigger by taking advantage of the Microsoft-managed APIs</span></span>       
    <span data-ttu-id="632b7-123">Minden eseményindító más rendelkezik **bemenetek** , amely meghatározza, hogy a viselkedését.</span><span class="sxs-lookup"><span data-stu-id="632b7-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="632b7-124">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-124">Request trigger</span></span>  

<span data-ttu-id="632b7-125">Ehhez az eseményindítóhoz szolgál egy végpontot, amely meghívja a keresztül egy HTTP-kérelem lehet meghívni a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="632b7-125">This trigger serves as an endpoint that you call via an HTTP Request to invoke your logic app.</span></span> <span data-ttu-id="632b7-126">A kérelem eseményindító ebben a példában néz ki:</span><span class="sxs-lookup"><span data-stu-id="632b7-126">A request trigger looks like this example:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type" : "request",
    "kind": "http",
    "inputs" : {
        "schema" : {
            "properties" : {
                "myInputProperty1" : { "type" : "string" },
                "myInputProperty2" : { "type" : "number" }
            },
        "required" : [ "myInputProperty1" ],
        "type" : "object"
        }
    }
} 
```

<span data-ttu-id="632b7-127">Szerepel továbbá egy nem kötelező tulajdonság neve **séma**:</span><span class="sxs-lookup"><span data-stu-id="632b7-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="632b7-128">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-128">Element name</span></span>|<span data-ttu-id="632b7-129">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-129">Required</span></span>|<span data-ttu-id="632b7-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="632b7-131">Séma</span><span class="sxs-lookup"><span data-stu-id="632b7-131">schema</span></span>|<span data-ttu-id="632b7-132">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-132">No</span></span>|<span data-ttu-id="632b7-133">A JSON-séma, amely megvizsgálja a bejövő kérelem.</span><span class="sxs-lookup"><span data-stu-id="632b7-133">A JSON schema that validates the incoming request.</span></span> <span data-ttu-id="632b7-134">Hasznos segítséget nyújt a soron következő munkafolyamat lépéseket tudja, mely tulajdonságokat kell hivatkoznia.</span><span class="sxs-lookup"><span data-stu-id="632b7-134">Useful for helping subsequent workflow steps know which properties to reference.</span></span>|

<span data-ttu-id="632b7-135">Ehhez a végponthoz meghívni, meg kell hívnia a *listCallbackUrl* API.</span><span class="sxs-lookup"><span data-stu-id="632b7-135">To invoke this endpoint, you need to call the *listCallbackUrl* API.</span></span> <span data-ttu-id="632b7-136">Lásd: [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="632b7-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="632b7-137">Ismétlődés eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-137">Recurrence trigger</span></span>  

<span data-ttu-id="632b7-138">Ismétlődés eseményindító egyike, amelyek egy meghatározott ütemezés alapján fut.</span><span class="sxs-lookup"><span data-stu-id="632b7-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="632b7-139">Ez a példa ilyen eseményindító nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="632b7-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="632b7-140">Ahogy látja, akkor egyszerűen egy munkafolyamat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="632b7-140">As you can see, it is a simple way to run a workflow.</span></span>  
  
|<span data-ttu-id="632b7-141">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-141">Element name</span></span>|<span data-ttu-id="632b7-142">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-142">Required</span></span>|<span data-ttu-id="632b7-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="632b7-144">gyakoriság</span><span class="sxs-lookup"><span data-stu-id="632b7-144">frequency</span></span>|<span data-ttu-id="632b7-145">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-145">Yes</span></span>|<span data-ttu-id="632b7-146">Milyen gyakran az eseményindító végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="632b7-146">How often the trigger executes.</span></span> <span data-ttu-id="632b7-147">Használja a lehetséges értékek egyikét: másodperc, perc, óra, nap, hét, hónap vagy év</span><span class="sxs-lookup"><span data-stu-id="632b7-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="632b7-148">időköz</span><span class="sxs-lookup"><span data-stu-id="632b7-148">interval</span></span>|<span data-ttu-id="632b7-149">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-149">Yes</span></span>|<span data-ttu-id="632b7-150">Adott gyakoriságának az ismétlődés időköze</span><span class="sxs-lookup"><span data-stu-id="632b7-150">Interval of the given frequency for the recurrence</span></span>|  
|<span data-ttu-id="632b7-151">startTime</span><span class="sxs-lookup"><span data-stu-id="632b7-151">startTime</span></span>|<span data-ttu-id="632b7-152">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-152">No</span></span>|<span data-ttu-id="632b7-153">A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.</span><span class="sxs-lookup"><span data-stu-id="632b7-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="632b7-154">Időzóna</span><span class="sxs-lookup"><span data-stu-id="632b7-154">timeZone</span></span>|<span data-ttu-id="632b7-155">nem</span><span class="sxs-lookup"><span data-stu-id="632b7-155">no</span></span>|<span data-ttu-id="632b7-156">A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.</span><span class="sxs-lookup"><span data-stu-id="632b7-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="632b7-157">Egy eseményindító végrehajtása egy bizonyos ponton a jövőben el is ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="632b7-157">You can also schedule a trigger to start executing at some point in the future.</span></span> <span data-ttu-id="632b7-158">Például ha el szeretné indítani a heti jelentés minden hétfőn ütemezheti a logikai alkalmazást a következő eseményindító létrehozása minden hétfőn el:</span><span class="sxs-lookup"><span data-stu-id="632b7-158">For example, if you want to start a weekly report every Monday you can schedule the logic app to start every Monday by creating the following trigger:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2015-06-22T00:00:00Z"
    }
}
```

## <a name="http-trigger"></a><span data-ttu-id="632b7-159">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-159">HTTP trigger</span></span>  

<span data-ttu-id="632b7-160">HTTP-eseményindítók kérdezze le a megadott végpont, és ellenőrizze a válasz határozza meg, hogy a munkafolyamat végre kell hajtani.</span><span class="sxs-lookup"><span data-stu-id="632b7-160">HTTP triggers poll a specified endpoint and check the response to determine whether the workflow should be executed.</span></span> <span data-ttu-id="632b7-161">A bemeneti objektum hajtja végre egy HTTP-hívás létrehozásához szükséges paraméterek készletét:</span><span class="sxs-lookup"><span data-stu-id="632b7-161">The inputs object takes the set of parameters required to construct an HTTP call:</span></span>  
  
|<span data-ttu-id="632b7-162">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-162">Element name</span></span>|<span data-ttu-id="632b7-163">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-163">Required</span></span>|<span data-ttu-id="632b7-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-164">Description</span></span>|<span data-ttu-id="632b7-165">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="632b7-166">Módszer</span><span class="sxs-lookup"><span data-stu-id="632b7-166">method</span></span>|<span data-ttu-id="632b7-167">igen</span><span class="sxs-lookup"><span data-stu-id="632b7-167">yes</span></span>|<span data-ttu-id="632b7-168">A következő HTTP-metódus egyike lehet: GET, POST, PUT, DELETE, a javítás vagy a HEAD</span><span class="sxs-lookup"><span data-stu-id="632b7-168">Can be one of the following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="632b7-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-169">String</span></span>|  
|<span data-ttu-id="632b7-170">URI</span><span class="sxs-lookup"><span data-stu-id="632b7-170">uri</span></span>|<span data-ttu-id="632b7-171">igen</span><span class="sxs-lookup"><span data-stu-id="632b7-171">yes</span></span>|<span data-ttu-id="632b7-172">A http vagy https-végpont is nevezett.</span><span class="sxs-lookup"><span data-stu-id="632b7-172">The http or https endpoint that is called.</span></span> <span data-ttu-id="632b7-173">Legfeljebb 2 KB.</span><span class="sxs-lookup"><span data-stu-id="632b7-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="632b7-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-174">String</span></span>|  
|<span data-ttu-id="632b7-175">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-175">queries</span></span>|<span data-ttu-id="632b7-176">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-176">No</span></span>|<span data-ttu-id="632b7-177">Az URL-cím hozzáadása a lekérdezési paraméterek képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="632b7-177">An object representing the query parameters to add to the URL.</span></span> <span data-ttu-id="632b7-178">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|<span data-ttu-id="632b7-179">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-179">Object</span></span>|  
|<span data-ttu-id="632b7-180">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-180">headers</span></span>|<span data-ttu-id="632b7-181">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-181">No</span></span>|<span data-ttu-id="632b7-182">A fejlécek, a kérelemben küldött mindegyikének képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="632b7-182">An object representing each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-183">Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="632b7-183">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="632b7-184">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-184">Object</span></span>|  
|<span data-ttu-id="632b7-185">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-185">body</span></span>|<span data-ttu-id="632b7-186">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-186">No</span></span>|<span data-ttu-id="632b7-187">A tartalom a végpontnak küldött képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="632b7-187">An object representing the payload that is sent to the endpoint.</span></span>|<span data-ttu-id="632b7-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-188">Object</span></span>|  
|<span data-ttu-id="632b7-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="632b7-189">retryPolicy</span></span>|<span data-ttu-id="632b7-190">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-190">No</span></span>|<span data-ttu-id="632b7-191">Egy objektum, amely lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.</span><span class="sxs-lookup"><span data-stu-id="632b7-191">An object that lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="632b7-192">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-192">Object</span></span>|  
|<span data-ttu-id="632b7-193">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="632b7-193">authentication</span></span>|<span data-ttu-id="632b7-194">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-194">No</span></span>|<span data-ttu-id="632b7-195">A metódust, hogy van-e a kérelem hitelesítése jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-195">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="632b7-196">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="632b7-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="632b7-197">Ütemező túl van egy több támogatott tulajdonságot: `authority` alapértelmezés szerint ez az érték van `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="632b7-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="632b7-198">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-198">Object</span></span>|  
  
<span data-ttu-id="632b7-199">A HTTP-eseményindítóval egy adott minta a Logic Apps alkalmazást való ahhoz, hogy a HTTP API szükséges.</span><span class="sxs-lookup"><span data-stu-id="632b7-199">The HTTP trigger requires the HTTP API to conform with a specific pattern to work well with your logic app.</span></span> <span data-ttu-id="632b7-200">A következő mezők szükségesek hozzá:</span><span class="sxs-lookup"><span data-stu-id="632b7-200">It requires the following fields:</span></span>  
  
|<span data-ttu-id="632b7-201">Válasz</span><span class="sxs-lookup"><span data-stu-id="632b7-201">Response</span></span>|<span data-ttu-id="632b7-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="632b7-203">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="632b7-203">Status code</span></span>|<span data-ttu-id="632b7-204">Állapotkód 200 \(OK\) futtató okozza.</span><span class="sxs-lookup"><span data-stu-id="632b7-204">Status code 200 \(OK\) to cause a run.</span></span> <span data-ttu-id="632b7-205">Bármely más állapotkód futtató nem okoz.</span><span class="sxs-lookup"><span data-stu-id="632b7-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="632b7-206">Próbálja meg újra\-fejléc után</span><span class="sxs-lookup"><span data-stu-id="632b7-206">Retry\-after header</span></span>|<span data-ttu-id="632b7-207">Amíg a logic app kérdezze le a végpont újra másodpercek számát.</span><span class="sxs-lookup"><span data-stu-id="632b7-207">Number of seconds until the logic app polls the endpoint again.</span></span>|  
|<span data-ttu-id="632b7-208">Hely fejléc</span><span class="sxs-lookup"><span data-stu-id="632b7-208">Location header</span></span>|<span data-ttu-id="632b7-209">Hívja meg a következő lekérdezési időköz az URL-címe.</span><span class="sxs-lookup"><span data-stu-id="632b7-209">The URL to call on the next polling interval.</span></span> <span data-ttu-id="632b7-210">Ha nincs megadva, az eredeti URL-címet használja.</span><span class="sxs-lookup"><span data-stu-id="632b7-210">If not specified, the original URL is used.</span></span>|  
  
<span data-ttu-id="632b7-211">Íme néhány példa a kérelmek különböző típusú különböző beállításokat:</span><span class="sxs-lookup"><span data-stu-id="632b7-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="632b7-212">Válaszkód</span><span class="sxs-lookup"><span data-stu-id="632b7-212">Response code</span></span>|<span data-ttu-id="632b7-213">Próbálja meg újra\-után</span><span class="sxs-lookup"><span data-stu-id="632b7-213">Retry\-After</span></span>|<span data-ttu-id="632b7-214">Viselkedés</span><span class="sxs-lookup"><span data-stu-id="632b7-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="632b7-215">200</span><span class="sxs-lookup"><span data-stu-id="632b7-215">200</span></span>|<span data-ttu-id="632b7-216">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="632b7-216">\(none\)</span></span>|<span data-ttu-id="632b7-217">Nem érvényes eseményindító, újrapróbálkozási\-után kell megadni, vagy ellenkező esetben a motor soha nem kérdezi le a következő kérelem.</span><span class="sxs-lookup"><span data-stu-id="632b7-217">Not a valid trigger, Retry\-After is required, or else the engine never polls for the next request.</span></span>|  
|<span data-ttu-id="632b7-218">202</span><span class="sxs-lookup"><span data-stu-id="632b7-218">202</span></span>|<span data-ttu-id="632b7-219">60</span><span class="sxs-lookup"><span data-stu-id="632b7-219">60</span></span>|<span data-ttu-id="632b7-220">Nem indítható el, a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="632b7-220">Do not trigger the workflow.</span></span> <span data-ttu-id="632b7-221">A következő kísérlet történik, egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="632b7-221">The next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="632b7-222">200</span><span class="sxs-lookup"><span data-stu-id="632b7-222">200</span></span>|<span data-ttu-id="632b7-223">10</span><span class="sxs-lookup"><span data-stu-id="632b7-223">10</span></span>|<span data-ttu-id="632b7-224">A munkafolyamat futtatása, és újra keressen további tartalmat 10 másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="632b7-224">Run the workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="632b7-225">400</span><span class="sxs-lookup"><span data-stu-id="632b7-225">400</span></span>|<span data-ttu-id="632b7-226">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="632b7-226">\(none\)</span></span>|<span data-ttu-id="632b7-227">Hibás kérés, amelyeken fut a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="632b7-227">Bad request, do not run the workflow.</span></span> <span data-ttu-id="632b7-228">Ha nincs **ismételje meg a házirend** definiálva, akkor az alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="632b7-228">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="632b7-229">Miután a rendszer elérte az újbóli próbálkozások számát, az eseményindító már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="632b7-229">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
|<span data-ttu-id="632b7-230">500</span><span class="sxs-lookup"><span data-stu-id="632b7-230">500</span></span>|<span data-ttu-id="632b7-231">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="632b7-231">\(none\)</span></span>|<span data-ttu-id="632b7-232">Kiszolgálóhiba, a munkafolyamat nem futnak.</span><span class="sxs-lookup"><span data-stu-id="632b7-232">Server error, do not run the workflow.</span></span>  <span data-ttu-id="632b7-233">Ha nincs **ismételje meg a házirend** definiálva, akkor az alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="632b7-233">If there is no **Retry Policy** defined, then the default policy is used.</span></span> <span data-ttu-id="632b7-234">Miután a rendszer elérte az újbóli próbálkozások számát, az eseményindító már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="632b7-234">After the number of retries has been reached, the trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="632b7-235">A HTTP-eseményindítóval kimenetének ebben a példában látható:</span><span class="sxs-lookup"><span data-stu-id="632b7-235">The outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="632b7-236">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-236">Element name</span></span>|<span data-ttu-id="632b7-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-237">Description</span></span>|<span data-ttu-id="632b7-238">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="632b7-239">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-239">headers</span></span>|<span data-ttu-id="632b7-240">A fejléceket a http-válaszok.</span><span class="sxs-lookup"><span data-stu-id="632b7-240">The headers of the http response.</span></span>|<span data-ttu-id="632b7-241">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-241">Object</span></span>|  
|<span data-ttu-id="632b7-242">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-242">body</span></span>|<span data-ttu-id="632b7-243">A HTTP-válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="632b7-243">The body of the http response.</span></span>|<span data-ttu-id="632b7-244">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="632b7-245">API-kapcsolat eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-245">API Connection trigger</span></span>  

<span data-ttu-id="632b7-246">Az API-kapcsolat eseményindító hasonlít az alapszintű funkciókat, hogy a HTTP-eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="632b7-246">The API connection trigger is similar to the HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="632b7-247">A művelet azonosítására szolgáló paraméterek azonban eltérőek.</span><span class="sxs-lookup"><span data-stu-id="632b7-247">However, the parameters for identifying the action are different.</span></span> <span data-ttu-id="632b7-248">Például:</span><span class="sxs-lookup"><span data-stu-id="632b7-248">Here is an example:</span></span>  
  
```json
"dailyReport" : {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            },
        }
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

|<span data-ttu-id="632b7-249">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-249">Element name</span></span>|<span data-ttu-id="632b7-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-250">Required</span></span>|<span data-ttu-id="632b7-251">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-251">Type</span></span>|<span data-ttu-id="632b7-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="632b7-253">állomás</span><span class="sxs-lookup"><span data-stu-id="632b7-253">host</span></span>|<span data-ttu-id="632b7-254">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-254">Yes</span></span>||<span data-ttu-id="632b7-255">A ApiApp átjáró és az azonosítóját tárolja.</span><span class="sxs-lookup"><span data-stu-id="632b7-255">The ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="632b7-256">Módszer</span><span class="sxs-lookup"><span data-stu-id="632b7-256">method</span></span>|<span data-ttu-id="632b7-257">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-257">Yes</span></span>|<span data-ttu-id="632b7-258">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-258">String</span></span>|<span data-ttu-id="632b7-259">A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**</span><span class="sxs-lookup"><span data-stu-id="632b7-259">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="632b7-260">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-260">queries</span></span>|<span data-ttu-id="632b7-261">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-261">No</span></span>|<span data-ttu-id="632b7-262">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-262">Object</span></span>|<span data-ttu-id="632b7-263">A lekérdezési paraméterek fel kell venni az URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-263">Represents the query parameters to be added to the URL.</span></span> <span data-ttu-id="632b7-264">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="632b7-265">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-265">headers</span></span>|<span data-ttu-id="632b7-266">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-266">No</span></span>|<span data-ttu-id="632b7-267">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-267">Object</span></span>|<span data-ttu-id="632b7-268">A fejlécek, a kérelemben küldött mindegyikének jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-268">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-269">Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="632b7-269">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="632b7-270">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-270">body</span></span>|<span data-ttu-id="632b7-271">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-271">No</span></span>|<span data-ttu-id="632b7-272">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-272">Object</span></span>|<span data-ttu-id="632b7-273">A tartalom a végpontnak küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-273">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="632b7-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="632b7-274">retryPolicy</span></span>|<span data-ttu-id="632b7-275">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-275">No</span></span>|<span data-ttu-id="632b7-276">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-276">Object</span></span>|<span data-ttu-id="632b7-277">Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.</span><span class="sxs-lookup"><span data-stu-id="632b7-277">Allows you to customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="632b7-278">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="632b7-278">authentication</span></span>|<span data-ttu-id="632b7-279">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-279">No</span></span>|<span data-ttu-id="632b7-280">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-280">Object</span></span>|<span data-ttu-id="632b7-281">A metódust, hogy van-e a kérelem hitelesítése jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-281">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="632b7-282">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítésre](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="632b7-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="632b7-283">A gazdagép tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="632b7-283">The properties for host are:</span></span>  
  
|<span data-ttu-id="632b7-284">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-284">Element name</span></span>|<span data-ttu-id="632b7-285">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-285">Required</span></span>|<span data-ttu-id="632b7-286">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="632b7-287">API runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="632b7-287">api runtimeUrl</span></span>|<span data-ttu-id="632b7-288">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-288">Yes</span></span>|<span data-ttu-id="632b7-289">A felügyelt API végpontja.</span><span class="sxs-lookup"><span data-stu-id="632b7-289">The endpoint of the managed API.</span></span>|  
|<span data-ttu-id="632b7-290">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="632b7-290">connection name</span></span>||<span data-ttu-id="632b7-291">Egy paraméter, hivatkozást kell `$connection` és a munkafolyamat által használt felügyelt API-kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="632b7-291">Must be a reference to a parameter called `$connection` and is the name of the managed API connection that the workflow uses.</span></span>|
  
<span data-ttu-id="632b7-292">A kimenetek egy API-kapcsolat eseményindító a következők:</span><span class="sxs-lookup"><span data-stu-id="632b7-292">The outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="632b7-293">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-293">Element name</span></span>|<span data-ttu-id="632b7-294">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-294">Type</span></span>|<span data-ttu-id="632b7-295">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="632b7-296">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-296">headers</span></span>|<span data-ttu-id="632b7-297">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-297">Object</span></span>|<span data-ttu-id="632b7-298">A fejléceket a http-válaszok.</span><span class="sxs-lookup"><span data-stu-id="632b7-298">The headers of the http response.</span></span>|  
|<span data-ttu-id="632b7-299">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-299">body</span></span>|<span data-ttu-id="632b7-300">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-300">Object</span></span>|<span data-ttu-id="632b7-301">A HTTP-válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="632b7-301">The body of the http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="632b7-302">HTTPWebhook eseményindító</span><span class="sxs-lookup"><span data-stu-id="632b7-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="632b7-303">HTTPWebhook eseményindító megnyit egy végpontot, a kézi indítási hasonló, de a HTTPWebhook eseményindító is meghívja a regisztrálásához és megadott URL-címhez.</span><span class="sxs-lookup"><span data-stu-id="632b7-303">The HTTPWebhook trigger opens an endpoint, similar to the manual trigger, but the HTTPWebhook trigger also calls out to a specified URL to register and unregister.</span></span> <span data-ttu-id="632b7-304">Íme egy példa hogyan nézhet ki egy HTTPWebhook eseményindító:</span><span class="sxs-lookup"><span data-stu-id="632b7-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

```json
"myappspottrigger": {
    "type": "httpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
    }
```

<span data-ttu-id="632b7-305">Ezek a szakaszok számos nem kötelező, és a Webhook viselkedését függ, mely szakaszok megadva, vagy nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="632b7-305">Many of these sections are optional, and the behavior of the Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="632b7-306">A Webhook tulajdonságainak a következők:</span><span class="sxs-lookup"><span data-stu-id="632b7-306">The properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="632b7-307">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-307">Element name</span></span>|<span data-ttu-id="632b7-308">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-308">Required</span></span>|<span data-ttu-id="632b7-309">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="632b7-310">előfizetés</span><span class="sxs-lookup"><span data-stu-id="632b7-310">subscribe</span></span>|<span data-ttu-id="632b7-311">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-311">No</span></span>|<span data-ttu-id="632b7-312">A kimenő kérelem is nevezett, amikor az eseményindító jön létre, és elvégzi a kezdeti regisztráció.</span><span class="sxs-lookup"><span data-stu-id="632b7-312">The outgoing request that is called when the trigger is created and performs the initial registration.</span></span>|  
|<span data-ttu-id="632b7-313">előfizetés lemondása</span><span class="sxs-lookup"><span data-stu-id="632b7-313">unsubscribe</span></span>|<span data-ttu-id="632b7-314">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-314">No</span></span>|<span data-ttu-id="632b7-315">A kimenő kérelem, az eseményindító törlésekor.</span><span class="sxs-lookup"><span data-stu-id="632b7-315">The outgoing request when the trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="632b7-316">**Előfizetés** a kimenő hívás megkezdeni a figyelést események jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="632b7-316">**Subscribe** is the outgoing call that's made to start listening to events.</span></span> <span data-ttu-id="632b7-317">Ez a hívás kezdődik-e ugyanazokat a paramétereket, amelyek a normál HTTP-műveleteket.</span><span class="sxs-lookup"><span data-stu-id="632b7-317">This call starts with the same set of parameters that the normal HTTP actions do.</span></span> <span data-ttu-id="632b7-318">A kimenő kezdeményezték bármely bármely olyan módon, például a munkafolyamat-módosítások idő, amikor a hitelesítő adatok már megkezdődött, vagy az eseményindító bemeneti paraméterek módosítása.</span><span class="sxs-lookup"><span data-stu-id="632b7-318">This outgoing call is made any time the workflow changes in any way, for example, whenever the credentials are rolled, or the trigger's input parameters change.</span></span>
  
    <span data-ttu-id="632b7-319">Ez a hívás támogatásához van egy új funkció: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="632b7-319">To support this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="632b7-320">Ez a függvény egy egyedi URL-címet az adott eseményindító a munkafolyamat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="632b7-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="632b7-321">Azt jelenti, hogy a szolgáltatás további használó végpontokon egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="632b7-321">It represents the unique identifier for the endpoints that use the Service REST.</span></span>  
  
-   <span data-ttu-id="632b7-322">**Leiratkozhat** van meghívva, amikor egy művelet Renderelés ehhez az eseményindítóhoz érvénytelen, beleértve:</span><span class="sxs-lookup"><span data-stu-id="632b7-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="632b7-323">Törlése vagy az eseményindító letiltása</span><span class="sxs-lookup"><span data-stu-id="632b7-323">Deleting or disabling the trigger</span></span>  
  
    -   <span data-ttu-id="632b7-324">Törlése vagy a munkafolyamat letiltása</span><span class="sxs-lookup"><span data-stu-id="632b7-324">Deleting or disabling the workflow</span></span>  
  
    -   <span data-ttu-id="632b7-325">Törlése vagy az előfizetés letiltása</span><span class="sxs-lookup"><span data-stu-id="632b7-325">Deleting or disabling the subscription</span></span>  
  
    <span data-ttu-id="632b7-326">A logikai alkalmazás automatikusan meghívja a lemondás műveletet.</span><span class="sxs-lookup"><span data-stu-id="632b7-326">The logic app automatically calls the unsubscribe action.</span></span> <span data-ttu-id="632b7-327">A paraméterek, a függvény nem ugyanaz, mint a HTTP-eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="632b7-327">The parameters to this function are the same as the HTTP trigger.</span></span>  
  
    <span data-ttu-id="632b7-328">A kimenetek az HTTPWebhook eseményindító a bejövő kérelem:</span><span class="sxs-lookup"><span data-stu-id="632b7-328">The outputs of the HTTPWebhook trigger are the contents of the incoming request:</span></span>  
  
|<span data-ttu-id="632b7-329">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-329">Element name</span></span>|<span data-ttu-id="632b7-330">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-330">Type</span></span>|<span data-ttu-id="632b7-331">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="632b7-332">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-332">headers</span></span>|<span data-ttu-id="632b7-333">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-333">Object</span></span>|<span data-ttu-id="632b7-334">A HTTP-kérés fejlécébe.</span><span class="sxs-lookup"><span data-stu-id="632b7-334">The headers of the http request.</span></span>|  
|<span data-ttu-id="632b7-335">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-335">body</span></span>|<span data-ttu-id="632b7-336">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-336">Object</span></span>|<span data-ttu-id="632b7-337">A http-kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="632b7-337">The body of the http request.</span></span>|  

<span data-ttu-id="632b7-338">A webhook művelet vonatkozó korlátozások is megadhatók a azonos módon [HTTP aszinkron korlátok](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="632b7-338">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="632b7-339">Feltételek</span><span class="sxs-lookup"><span data-stu-id="632b7-339">Conditions</span></span>  

<span data-ttu-id="632b7-340">Bármely eseményindító egy vagy több feltételt segítségével határozza meg, hogy a munkafolyamat futtasson-e vagy sem.</span><span class="sxs-lookup"><span data-stu-id="632b7-340">For any trigger, you can use one or more conditions to determine whether the workflow should run or not.</span></span> <span data-ttu-id="632b7-341">Példa:</span><span class="sxs-lookup"><span data-stu-id="632b7-341">For example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "conditions": [ {
        "expression": "@parameters('sendReports')"
    } ],
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="632b7-342">Ebben az esetben a jelentés csak eseményindítók során a munkafolyamat `sendReports` paraméter értéke igaz.</span><span class="sxs-lookup"><span data-stu-id="632b7-342">In this case, the report only triggers while the workflow's `sendReports` parameter is set to true.</span></span> <span data-ttu-id="632b7-343">Végezetül feltételek hivatkozhatnak az állapotkód: az eseményindító.</span><span class="sxs-lookup"><span data-stu-id="632b7-343">Finally, conditions may reference the status code of the trigger.</span></span> <span data-ttu-id="632b7-344">Például sikerült indítsa el a munkafolyamat csak akkor, ha a webhely adja vissza egy állapotkód: 500, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="632b7-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="632b7-345">Ha bármely kifejezés hivatkozik az állapotkód: az eseményindító \(valamilyen módon\), az alapértelmezett viselkedés \(eseményindító csak a 200-as \(OK\) \) váltja fel.</span><span class="sxs-lookup"><span data-stu-id="632b7-345">When any expression references the status code of the trigger \(in any way\), the default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="632b7-346">Például, ha azt szeretné, állapotkód: 200-as és a 201-es állapotkód is elindítható, fel kell vennie: `@or(equals(triggers().code, 200),equals(triggers().code,201))` a feltételként.</span><span class="sxs-lookup"><span data-stu-id="632b7-346">For example, if you want to trigger on both status code 200 and status code 201, you have to include: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="632b7-347">Indítsa el a kérelmek több futtatása</span><span class="sxs-lookup"><span data-stu-id="632b7-347">Start multiple runs for a request</span></span>

<span data-ttu-id="632b7-348">Az egy kérelemhez több futtatások indítsa `splitOn` akkor hasznos, például, ha azt szeretné, és egy végpontot, amely több új elemek közötti lekérdezési időközök kérdezze le.</span><span class="sxs-lookup"><span data-stu-id="632b7-348">To kick off multiple runs for a single request, `splitOn` is useful, for example, when you want to poll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="632b7-349">A `splitOn`, a válasz forgalma, amely tartalmazza a tömb elemek, amelyek az eseményindító futtató indításához használni kívánt belül tulajdonság megadása.</span><span class="sxs-lookup"><span data-stu-id="632b7-349">With `splitOn`, you specify the property inside the response payload that contains the array of items, each of which you want to use to start a run of the trigger.</span></span> <span data-ttu-id="632b7-350">Tegyük fel, hogy az API-k, a következő választ ad vissza, amely rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="632b7-350">For example, imagine you have an API that returns the following response:</span></span>  
  
```json
{
    "Status" : "success",
    "Rows" : [
        {  
            "id" : 938109380,
            "name" : "mycoolrow"
        },
        {
            "id" : 938109381,
            "name" : "another row"
        }
    ]
}
```
  
<span data-ttu-id="632b7-351">A Logic Apps alkalmazást csak a sorok tartalom van szüksége, hogyan hozhat létre például ebben a példában az eseményindító:</span><span class="sxs-lookup"><span data-stu-id="632b7-351">Your logic app only needs the Rows content, so you can construct your trigger like this example:</span></span>  
  
```json
"mysplitter" : {
    "type" : "http",
    "recurrence": {
        "frequency": "Minute",
        "interval": "1"
    },
    "intputs" : {
        "uri" : "https://mydomain.com/myAPI",
        "method" : "GET"
    },
    "splitOn" : "@triggerBody()?.Rows"
}
```
  
<span data-ttu-id="632b7-352">Ezt követően a munkafolyamat-definíciót, `@triggerBody().name` adja vissza `mycoolrow` az első alkalommal történő futtatásakor, és `another row` második futtatáshoz.</span><span class="sxs-lookup"><span data-stu-id="632b7-352">Then, in the workflow definition, `@triggerBody().name` returns `mycoolrow` for the first run, and `another row` for the second run.</span></span> <span data-ttu-id="632b7-353">Az eseményindító kimenetek kinézetét ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="632b7-353">The trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="632b7-354">Igen, ha `SplitOn`, nem olvasható be a tulajdonságokat a tömb kívül ebben az esetben a `Status` mező.</span><span class="sxs-lookup"><span data-stu-id="632b7-354">So if you use `SplitOn`, you can't get the properties that are outside the array, in this case, the `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="632b7-355">A jelen példában használjuk a `?` operátor elkerülhető a hibát, ha a `Rows` tulajdonság nincs jelen.</span><span class="sxs-lookup"><span data-stu-id="632b7-355">In this example, we use the `?` operator to be able to avoid a failure if the `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="632b7-356">Egyszeri futtatási példánya</span><span class="sxs-lookup"><span data-stu-id="632b7-356">Single run instance</span></span>

<span data-ttu-id="632b7-357">Beállíthatja, hogy eseményindítókat, amelyek rendelkeznek egy ismétlődési tulajdonság csak akkor működik, amikor az összes aktív futtatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="632b7-357">You can configure triggers that have a recurrence property to only fire if all active runs have completed.</span></span> <span data-ttu-id="632b7-358">Ütemezett ismétlődése következik be, amíg a folyamatban lévő futtassa, ha az eseményindító kihagyja, és megvárja a következő ütemezett ismétlési időköz újra.</span><span class="sxs-lookup"><span data-stu-id="632b7-358">If a scheduled recurrence occurs while there is an in-progress run, the trigger skips and waits until the next scheduled recurrence interval to check again.</span></span>

<span data-ttu-id="632b7-359">A beállítás a művelet lehetőségeit:</span><span class="sxs-lookup"><span data-stu-id="632b7-359">You can configure this setting through the operation options:</span></span>

```json
"triggers": {
    "mytrigger": {
        "type": "http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="types-and-inputs"></a><span data-ttu-id="632b7-360">Típusok és a bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="632b7-360">Types and inputs</span></span>  

<span data-ttu-id="632b7-361">Nincsenek számos különböző típusú műveletek, az egyedi viselkedését.</span><span class="sxs-lookup"><span data-stu-id="632b7-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="632b7-362">Gyűjtemény műveletek önmagába sok más műveleteket is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="632b7-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="632b7-363">Standard műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-363">Standard actions</span></span>  

-   <span data-ttu-id="632b7-364">**HTTP** Ez a művelet HTTP webes végpont hívja.</span><span class="sxs-lookup"><span data-stu-id="632b7-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="632b7-365">**ApiConnection** \- Ez a művelet a HTTP-művelet viselkedik, de a Microsoft által felügyelt API-k.</span><span class="sxs-lookup"><span data-stu-id="632b7-365">**ApiConnection** \- This action behaves like the HTTP action, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="632b7-366">**ApiConnectionWebhook** \- például HTTPWebhook, de a Microsoft által felügyelt API-kat használ.</span><span class="sxs-lookup"><span data-stu-id="632b7-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses the Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="632b7-367">**Válasz** \- Ez a művelet bejövő válasz meghatározása.</span><span class="sxs-lookup"><span data-stu-id="632b7-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="632b7-368">**Várjon, amíg** \- egyszerű művelet várakozik a rögzített méretű idő vagy egy adott időpontig.</span><span class="sxs-lookup"><span data-stu-id="632b7-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="632b7-369">**Munkafolyamat** \- Ez a művelet egy beágyazott munkafolyamat jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="632b7-370">**Függvény** \- Ez a művelet egy Azure-függvény jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="632b7-371">Gyűjtemény műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-371">Collection actions</span></span>

-   <span data-ttu-id="632b7-372">**Hatókör** \- Ez a művelet akkor más műveletek logikai csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="632b7-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="632b7-373">**Az állapot** \- Ez a művelet egy kifejezés kiértékelése, és végrehajtja a megfelelő eredmény ágat.</span><span class="sxs-lookup"><span data-stu-id="632b7-373">**Condition** \- This action evaluates an expression and executes the corresponding result branch.</span></span>

-   <span data-ttu-id="632b7-374">**ForEach** \- ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="632b7-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="632b7-375">**Amíg** \- ismétlési művelet belső műveleteket hajt végre, amíg egy feltétel eredménye igaz.</span><span class="sxs-lookup"><span data-stu-id="632b7-375">**Until** \- This looping action executes inner actions until a condition results to true.</span></span>
  
<span data-ttu-id="632b7-376">Írja be a van más **bemenetek** , amely egy művelet viselkedését határozza meg.</span><span class="sxs-lookup"><span data-stu-id="632b7-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="632b7-377">HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-377">HTTP action</span></span>  

<span data-ttu-id="632b7-378">HTTP-műveletek hívható meg a megadott végpont, és ellenőrizze a válasz határozza meg, hogy a munkafolyamat fusson-e.</span><span class="sxs-lookup"><span data-stu-id="632b7-378">HTTP actions call a specified endpoint and check the response to determine whether the workflow should run.</span></span> <span data-ttu-id="632b7-379">A **bemenetek** objektum veszi a HTTP-hívás létrehozásához szükséges paraméterek készletét:</span><span class="sxs-lookup"><span data-stu-id="632b7-379">The **inputs** object takes the set of parameters required to construct the HTTP call:</span></span>  
  
|<span data-ttu-id="632b7-380">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-380">Element name</span></span>|<span data-ttu-id="632b7-381">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-381">Required</span></span>|<span data-ttu-id="632b7-382">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-382">Type</span></span>|<span data-ttu-id="632b7-383">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="632b7-384">Módszer</span><span class="sxs-lookup"><span data-stu-id="632b7-384">method</span></span>|<span data-ttu-id="632b7-385">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-385">Yes</span></span>|<span data-ttu-id="632b7-386">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-386">String</span></span>|<span data-ttu-id="632b7-387">A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**</span><span class="sxs-lookup"><span data-stu-id="632b7-387">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="632b7-388">URI</span><span class="sxs-lookup"><span data-stu-id="632b7-388">uri</span></span>|<span data-ttu-id="632b7-389">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-389">Yes</span></span>|<span data-ttu-id="632b7-390">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-390">String</span></span>|<span data-ttu-id="632b7-391">A http vagy https-végpont is nevezett.</span><span class="sxs-lookup"><span data-stu-id="632b7-391">The http or https endpoint that is called.</span></span> <span data-ttu-id="632b7-392">Hossza legfeljebb 2 KB.</span><span class="sxs-lookup"><span data-stu-id="632b7-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="632b7-393">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-393">queries</span></span>|<span data-ttu-id="632b7-394">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-394">No</span></span>|<span data-ttu-id="632b7-395">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-395">Object</span></span>|<span data-ttu-id="632b7-396">A lekérdezési paraméterek hozzáadása az URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-396">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="632b7-397">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="632b7-398">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-398">headers</span></span>|<span data-ttu-id="632b7-399">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-399">No</span></span>|<span data-ttu-id="632b7-400">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-400">Object</span></span>|<span data-ttu-id="632b7-401">A fejlécek, a kérelemben küldött mindegyikének jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-401">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-402">Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="632b7-402">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="632b7-403">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-403">body</span></span>|<span data-ttu-id="632b7-404">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-404">No</span></span>|<span data-ttu-id="632b7-405">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-405">Object</span></span>|<span data-ttu-id="632b7-406">A tartalom a végpontnak küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-406">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="632b7-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="632b7-407">retryPolicy</span></span>|<span data-ttu-id="632b7-408">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-408">No</span></span>|<span data-ttu-id="632b7-409">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-409">Object</span></span>|<span data-ttu-id="632b7-410">Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.</span><span class="sxs-lookup"><span data-stu-id="632b7-410">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="632b7-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="632b7-411">operationsOptions</span></span>|<span data-ttu-id="632b7-412">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-412">No</span></span>|<span data-ttu-id="632b7-413">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-413">String</span></span>|<span data-ttu-id="632b7-414">A speciális viselkedés felülbírálásához csoportját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="632b7-414">Defines the set of special behaviors to override.</span></span>|  
|<span data-ttu-id="632b7-415">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="632b7-415">authentication</span></span>|<span data-ttu-id="632b7-416">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-416">No</span></span>|<span data-ttu-id="632b7-417">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-417">Object</span></span>|<span data-ttu-id="632b7-418">A metódust, hogy van-e a kérelem hitelesítése jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-418">Represents the method that the request should be authenticated.</span></span> <span data-ttu-id="632b7-419">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="632b7-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="632b7-420">Ütemező túl van egy több támogatott tulajdonságot: `authority`.</span><span class="sxs-lookup"><span data-stu-id="632b7-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="632b7-421">Alapértelmezés szerint ez a `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="632b7-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="632b7-422">HTTP-műveletek \(és API-kapcsolat\) műveletek támogatási házirendek próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="632b7-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="632b7-423">Újrapróbálkozási házirendje érvényes időszakos hibák, mint a HTTP-állapotkódok jellemzőek 408 429 és 5xx minden csatlakozási kivétel mellett.</span><span class="sxs-lookup"><span data-stu-id="632b7-423">A retry policy applies to intermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition to any connectivity exceptions.</span></span> <span data-ttu-id="632b7-424">Ez a házirend leírását használja a *retryPolicy* objektum megadva, az itt látható:</span><span class="sxs-lookup"><span data-stu-id="632b7-424">This policy is described using the *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="632b7-425">Az újrapróbálkozási időköz az ISO 8601 formátumban van megadva.</span><span class="sxs-lookup"><span data-stu-id="632b7-425">The retry interval is specified in the ISO 8601 format.</span></span> <span data-ttu-id="632b7-426">Ezt az időközt értéke a minimális és az alapértelmezett 20 másodperc, a maximális érték pedig egy óra.</span><span class="sxs-lookup"><span data-stu-id="632b7-426">This interval has a default and minimum value of 20 seconds, while the maximum value is one hour.</span></span> <span data-ttu-id="632b7-427">Az alapértelmezett és a maximális újrapróbálkozási számláló négy óra.</span><span class="sxs-lookup"><span data-stu-id="632b7-427">The default and maximum retry count is four hours.</span></span> <span data-ttu-id="632b7-428">Ha nincs megadva az újrapróbálkozási házirend-definíció, egy `fixed` stratégia használatos az alapértelmezett értékekkel újrapróbálkozási számát és az időközt.</span><span class="sxs-lookup"><span data-stu-id="632b7-428">If the retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="632b7-429">Tiltsa le az újrapróbálkozási házirendet, adja meg a típus `None`.</span><span class="sxs-lookup"><span data-stu-id="632b7-429">To disable the retry policy, set its type to `None`.</span></span>  
  
<span data-ttu-id="632b7-430">Például a következő tevékenységek ismétlése beolvasásához használt a legújabb híreket kétszer, ha nincsenek időszakos hibák, három végrehajtások, 30 másodperces késleltetéssel minden kísérlet között összesen:</span><span class="sxs-lookup"><span data-stu-id="632b7-430">For example, the following action retries fetching the latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
```json
"latestNews" : {
    "type": "http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```
### <a name="asynchronous-patterns"></a><span data-ttu-id="632b7-431">Aszinkron minták</span><span class="sxs-lookup"><span data-stu-id="632b7-431">Asynchronous patterns</span></span>

<span data-ttu-id="632b7-432">Alapértelmezés szerint az összes HTTP-alapú műveletek támogatja a szabványos aszinkron művelet szabályt.</span><span class="sxs-lookup"><span data-stu-id="632b7-432">By default, all HTTP-based actions support the standard asynchronous operation pattern.</span></span> <span data-ttu-id="632b7-433">Igen, ha a távoli kiszolgáló, az azt jelzi, hogy a kérelem elfogadása egy 202 feldolgozásra \(elfogadott\) választ, a Logic Apps motor tartja a lekérdezés egy terminál állapot eléréséig hely válaszfejléc megadott URL- \(nem\-202 válasz\).</span><span class="sxs-lookup"><span data-stu-id="632b7-433">So if the remote server indicates that the request is accepted for processing with a 202 \(Accepted\) response, the Logic Apps engine keeps polling the URL specified in the response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="632b7-434">A korábban leírt aszinkron viselkedés letiltásához állítsa be a `DisableAsyncPattern` lehetősége a művelet bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="632b7-434">To disable the asynchronous behavior previously described, set a `DisableAsyncPattern` option in the action inputs.</span></span> <span data-ttu-id="632b7-435">A művelet kimenetének ebben az esetben a kiszolgáló válaszát kezdeti 202 alapul.</span><span class="sxs-lookup"><span data-stu-id="632b7-435">In this case, the output of the action is based on the initial 202 response from the server.</span></span>  
  
```json
"invokeLongRunningOperation" : {
    "type": "http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```

#### <a name="asynchronous-limits"></a><span data-ttu-id="632b7-436">Aszinkron korlátok</span><span class="sxs-lookup"><span data-stu-id="632b7-436">Asynchronous Limits</span></span>

<span data-ttu-id="632b7-437">Egy aszinkron mintát is korlátozni az egy adott időintervallumban időtartamát.</span><span class="sxs-lookup"><span data-stu-id="632b7-437">An asynchronous pattern can be limited in its duration to a specific time interval.</span></span>  <span data-ttu-id="632b7-438">Ha az időtartam nem érte el a Terminálszolgáltatások állapot eltelte után a művelet állapota lesz megjelölve `Cancelled` egy kóddal `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="632b7-438">If the time interval elapses without reaching a terminal state, the status of the action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="632b7-439">ISO 8601 formátumban megadott korlát időkorlát.</span><span class="sxs-lookup"><span data-stu-id="632b7-439">The limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="632b7-440">Korlátozások a következő szintaxissal adható meg:</span><span class="sxs-lookup"><span data-stu-id="632b7-440">Limits can be specified with the following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="632b7-441">API-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="632b7-441">API Connection</span></span>  

<span data-ttu-id="632b7-442">API-kapcsolat a Microsoft által felügyelt összekötők hivatkozó művelet.</span><span class="sxs-lookup"><span data-stu-id="632b7-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="632b7-443">Ez a művelet érvényes kapcsolat, és az API és a szükséges paraméterek hivatkozás szükséges.</span><span class="sxs-lookup"><span data-stu-id="632b7-443">This action requires a reference to a valid connection, and information on the API and parameters required.</span></span>

|<span data-ttu-id="632b7-444">Elem neve</span><span class="sxs-lookup"><span data-stu-id="632b7-444">Element name</span></span>|<span data-ttu-id="632b7-445">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-445">Required</span></span>|<span data-ttu-id="632b7-446">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-446">Type</span></span>|<span data-ttu-id="632b7-447">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="632b7-448">állomás</span><span class="sxs-lookup"><span data-stu-id="632b7-448">host</span></span>|<span data-ttu-id="632b7-449">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-449">Yes</span></span>|<span data-ttu-id="632b7-450">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-450">Object</span></span>|<span data-ttu-id="632b7-451">Az összekötővel kapcsolatos adatokat, például a runtimeUrl és a kapcsolat objektum hivatkozását jelenti.</span><span class="sxs-lookup"><span data-stu-id="632b7-451">Represents the connector information such as the runtimeUrl and reference to the connection object</span></span>|
|<span data-ttu-id="632b7-452">Módszer</span><span class="sxs-lookup"><span data-stu-id="632b7-452">method</span></span>|<span data-ttu-id="632b7-453">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-453">Yes</span></span>|<span data-ttu-id="632b7-454">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-454">String</span></span>|<span data-ttu-id="632b7-455">A következő HTTP-metódus egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy **HEAD**</span><span class="sxs-lookup"><span data-stu-id="632b7-455">Can be one of the following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="632b7-456">Elérési út</span><span class="sxs-lookup"><span data-stu-id="632b7-456">path</span></span>|<span data-ttu-id="632b7-457">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-457">Yes</span></span>|<span data-ttu-id="632b7-458">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-458">String</span></span>|<span data-ttu-id="632b7-459">Az API-művelet elérési útját.</span><span class="sxs-lookup"><span data-stu-id="632b7-459">The path of the API operation.</span></span>|  
|<span data-ttu-id="632b7-460">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-460">queries</span></span>|<span data-ttu-id="632b7-461">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-461">No</span></span>|<span data-ttu-id="632b7-462">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-462">Object</span></span>|<span data-ttu-id="632b7-463">A lekérdezési paraméterek hozzáadása az URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-463">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="632b7-464">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="632b7-465">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-465">headers</span></span>|<span data-ttu-id="632b7-466">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-466">No</span></span>|<span data-ttu-id="632b7-467">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-467">Object</span></span>|<span data-ttu-id="632b7-468">A fejlécek, a kérelemben küldött mindegyikének jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-468">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-469">Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="632b7-469">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="632b7-470">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-470">body</span></span>|<span data-ttu-id="632b7-471">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-471">No</span></span>|<span data-ttu-id="632b7-472">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-472">Object</span></span>|<span data-ttu-id="632b7-473">A tartalom a végpontnak küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-473">Represents the payload that is sent to the endpoint.</span></span>|  
|<span data-ttu-id="632b7-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="632b7-474">retryPolicy</span></span>|<span data-ttu-id="632b7-475">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-475">No</span></span>|<span data-ttu-id="632b7-476">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-476">Object</span></span>|<span data-ttu-id="632b7-477">Lehetővé teszi testre szabhatja az újrapróbálási viselkedése 4xx vagy 5xx hibákat.</span><span class="sxs-lookup"><span data-stu-id="632b7-477">Lets you customize the retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="632b7-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="632b7-478">operationsOptions</span></span>|<span data-ttu-id="632b7-479">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-479">No</span></span>|<span data-ttu-id="632b7-480">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-480">String</span></span>|<span data-ttu-id="632b7-481">A speciális viselkedés felülbírálásához csoportját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="632b7-481">Defines the set of special behaviors to override.</span></span>|  

```json
"Send_Email": {
    "type": "apiconnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "post",
        "body": {
            "Subject": "New Tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
    }
```

## <a name="api-connection-webhook-action"></a><span data-ttu-id="632b7-482">API-kapcsolat webhook művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-482">API Connection webhook action</span></span>

```json
"Send_approval_email": {
    "type": "apiconnectionwebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

<span data-ttu-id="632b7-483">A webhook művelet vonatkozó korlátozások is megadhatók a azonos módon [HTTP aszinkron korlátok](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="632b7-483">Limits on a webhook action can be specified in the same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="632b7-484">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-484">Response action</span></span>  

<span data-ttu-id="632b7-485">A művelet típusa a teljes válasz forgalma a HTTP-kérelem tartalmazza, és egy statusCode, a törzs és a fejlécek tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="632b7-485">This action type contains the entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
```json
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "runAfter": {}
}
```
  
<span data-ttu-id="632b7-486">A válasz műveletnek speciális egyéb műveletek nem vonatkozó korlátozások.</span><span class="sxs-lookup"><span data-stu-id="632b7-486">The response action has special restrictions that don't apply to other actions.</span></span> <span data-ttu-id="632b7-487">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="632b7-487">Specifically:</span></span>  
  
-   <span data-ttu-id="632b7-488">Sérülésével kapcsolatos válaszműveletek nem lehet a definícióban párhuzamos, mert a bejövő kérelem determinisztikus választ szükség.</span><span class="sxs-lookup"><span data-stu-id="632b7-488">Response actions cannot be parallel in a definition because a deterministic response to the incoming request is required.</span></span>  
  
-   <span data-ttu-id="632b7-489">Ha egy válasz művelet elérése után a bejövő kérelem kapott választ, a művelet nem sikerült tekinthető \(ütközés\), és ennek eredményeképpen a Futtatás `Failed`.</span><span class="sxs-lookup"><span data-stu-id="632b7-489">If a response action is reached after the incoming request has received a response, the action is considered failed \(conflict\), and as a result, the run is `Failed`.</span></span>  
  
-   <span data-ttu-id="632b7-490">A válaszműveleteket egy munkafolyamatban nem lehet `splitOn` a eseményindítójára, mert egy hívás hatására hány futtatása.</span><span class="sxs-lookup"><span data-stu-id="632b7-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="632b7-491">Ennek köszönhetően ez kell ellenőriznie a folyamat PUT és ok egy hibás kérés esetén.</span><span class="sxs-lookup"><span data-stu-id="632b7-491">As a result, this should be validated when the flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="632b7-492">Várjon, amíg a művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-492">Wait action</span></span>  

<span data-ttu-id="632b7-493">A `wait` művelet felfüggeszti a munkafolyamat végrehajtása a megadott intervallumban.</span><span class="sxs-lookup"><span data-stu-id="632b7-493">The `wait` action suspends workflow execution for the specified interval.</span></span> <span data-ttu-id="632b7-494">Például várjon 15 percig, használhatja a kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="632b7-494">For example, to wait 15 minutes, you can use this snippet:</span></span>  
  
```json
"waitForFifteenMinutes" : {
    "type": "wait",
    "inputs": {
        "interval": {
            "unit" : "minute",
            "count" : 15
        }
    }
}
```  
  
<span data-ttu-id="632b7-495">Azt is megteheti várakozási idő az adott néhány percet, használhatja a példa:</span><span class="sxs-lookup"><span data-stu-id="632b7-495">Alternatively, to wait until a specific moment in time, you can use this example:</span></span>  
  
```json
"waitUntilOctober" : {
    "type": "wait",
    "inputs": {
        "until": {
            "timestamp" : "2016-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> <span data-ttu-id="632b7-496">A várakozási időtartama vagy adható a **időköz** objektum vagy a **amíg** objektum, de soha sem egyszerre mindkettőre.</span><span class="sxs-lookup"><span data-stu-id="632b7-496">The wait duration can be either specified using the **interval** object or the **until** object, but not both.</span></span>  
  
|<span data-ttu-id="632b7-497">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-497">Name</span></span>|<span data-ttu-id="632b7-498">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-498">Required</span></span>|<span data-ttu-id="632b7-499">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-499">Type</span></span>|<span data-ttu-id="632b7-500">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-501">időköz</span><span class="sxs-lookup"><span data-stu-id="632b7-501">interval</span></span>|<span data-ttu-id="632b7-502">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-502">No</span></span>|<span data-ttu-id="632b7-503">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-503">Object</span></span>|<span data-ttu-id="632b7-504">A várakozási időtartama idő alapján.</span><span class="sxs-lookup"><span data-stu-id="632b7-504">The wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="632b7-505">időköze</span><span class="sxs-lookup"><span data-stu-id="632b7-505">interval unit</span></span>|<span data-ttu-id="632b7-506">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-506">Yes</span></span>|<span data-ttu-id="632b7-507">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-507">String</span></span>|<span data-ttu-id="632b7-508">Egy intervallumok: másodperc, perc, óra, nap, hét, hónap, év.</span><span class="sxs-lookup"><span data-stu-id="632b7-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="632b7-509">időköz száma</span><span class="sxs-lookup"><span data-stu-id="632b7-509">interval count</span></span>|<span data-ttu-id="632b7-510">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-510">Yes</span></span>|<span data-ttu-id="632b7-511">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-511">String</span></span>|<span data-ttu-id="632b7-512">A megadott belső egység alapján időtartama.</span><span class="sxs-lookup"><span data-stu-id="632b7-512">Duration based on the given internal unit.</span></span>|  
|<span data-ttu-id="632b7-513">amíg</span><span class="sxs-lookup"><span data-stu-id="632b7-513">until</span></span>|<span data-ttu-id="632b7-514">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-514">No</span></span>|<span data-ttu-id="632b7-515">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-515">Object</span></span>|<span data-ttu-id="632b7-516">A várakozási időtartama időben a pontok alapján.</span><span class="sxs-lookup"><span data-stu-id="632b7-516">The wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="632b7-517">amíg időbélyeg</span><span class="sxs-lookup"><span data-stu-id="632b7-517">until timestamp</span></span>|<span data-ttu-id="632b7-518">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-518">Yes</span></span>|<span data-ttu-id="632b7-519">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-519">String</span></span>|<span data-ttu-id="632b7-520">Karakterlánc &#124; A pont UTC formátumban, ha a lejár a várakozási idő.</span><span class="sxs-lookup"><span data-stu-id="632b7-520">String&#124;The point in time in UTC when the wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="632b7-521">Lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-521">Query action</span></span>

<span data-ttu-id="632b7-522">A `query` művelet lehetővé teszi, hogy egy tömb feltétel alapján szűrni.</span><span class="sxs-lookup"><span data-stu-id="632b7-522">The `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="632b7-523">Például válassza ki a 2-nél nagyobb számot, használhatja:</span><span class="sxs-lookup"><span data-stu-id="632b7-523">For example, to select numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="632b7-524">A kimenet a `query` művelete olyan tömb, amely rendelkezik, amelyek megfelelnek a következő feltételt: a bemeneti tömb elemei.</span><span class="sxs-lookup"><span data-stu-id="632b7-524">The output from the `query` action is an array that has elements from the input array that satisfy the condition.</span></span>

> [!NOTE]
> <span data-ttu-id="632b7-525">Ha nincs érték felel meg a `where` , az eredmény feltétele egy üres tömb.</span><span class="sxs-lookup"><span data-stu-id="632b7-525">If no values satisfy the `where` condition, the result is an empty array.</span></span>

|<span data-ttu-id="632b7-526">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-526">Name</span></span>|<span data-ttu-id="632b7-527">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-527">Required</span></span>|<span data-ttu-id="632b7-528">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-528">Type</span></span>|<span data-ttu-id="632b7-529">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="632b7-530">A</span><span class="sxs-lookup"><span data-stu-id="632b7-530">from</span></span>|<span data-ttu-id="632b7-531">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-531">Yes</span></span>|<span data-ttu-id="632b7-532">A tömb</span><span class="sxs-lookup"><span data-stu-id="632b7-532">Array</span></span>|<span data-ttu-id="632b7-533">A forrástömb.</span><span class="sxs-lookup"><span data-stu-id="632b7-533">The source array.</span></span>|
|<span data-ttu-id="632b7-534">Ha</span><span class="sxs-lookup"><span data-stu-id="632b7-534">where</span></span>|<span data-ttu-id="632b7-535">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-535">Yes</span></span>|<span data-ttu-id="632b7-536">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-536">String</span></span>|<span data-ttu-id="632b7-537">A forrás tömb egyes elemei a vonatkozó feltételt.</span><span class="sxs-lookup"><span data-stu-id="632b7-537">The condition to apply to each element of the source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="632b7-538">Kijelölési művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-538">Select action</span></span>

<span data-ttu-id="632b7-539">A `select` művelet lehetővé teszi, hogy a tömb egyes elemei projekt be új értéket.</span><span class="sxs-lookup"><span data-stu-id="632b7-539">The `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="632b7-540">Például egy tömb számok alakítani egy objektumokból álló tömb, használhatja:</span><span class="sxs-lookup"><span data-stu-id="632b7-540">For example, to convert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="632b7-541">A kimenetét a `select` művelete olyan tömb, amely a bemeneti tömb azonos számossága tartozik minden elem által meghatározott átalakítva a `select` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="632b7-541">The output of the `select` action is an array that has the same cardinality as the input array, with each element transformed as defined by the `select` property.</span></span> <span data-ttu-id="632b7-542">Ha a bemeneti érték egy üres tömb, a kimeneti is üres tömb.</span><span class="sxs-lookup"><span data-stu-id="632b7-542">If the input is an empty array, the output is also an empty array.</span></span>

|<span data-ttu-id="632b7-543">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-543">Name</span></span>|<span data-ttu-id="632b7-544">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-544">Required</span></span>|<span data-ttu-id="632b7-545">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-545">Type</span></span>|<span data-ttu-id="632b7-546">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="632b7-547">A</span><span class="sxs-lookup"><span data-stu-id="632b7-547">from</span></span>|<span data-ttu-id="632b7-548">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-548">Yes</span></span>|<span data-ttu-id="632b7-549">A tömb</span><span class="sxs-lookup"><span data-stu-id="632b7-549">Array</span></span>|<span data-ttu-id="632b7-550">A forrástömb.</span><span class="sxs-lookup"><span data-stu-id="632b7-550">The source array.</span></span>|
|<span data-ttu-id="632b7-551">Válassza ki</span><span class="sxs-lookup"><span data-stu-id="632b7-551">select</span></span>|<span data-ttu-id="632b7-552">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-552">Yes</span></span>|<span data-ttu-id="632b7-553">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="632b7-553">Any</span></span>|<span data-ttu-id="632b7-554">A forrás tömb egyes elemei alkalmazandó leképezési.</span><span class="sxs-lookup"><span data-stu-id="632b7-554">The projection to apply to each element of the source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="632b7-555">Leállítási művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-555">Terminate action</span></span>

<span data-ttu-id="632b7-556">A megszakítási művelet munkafolyamat futtatás, bármilyen az üzenetsoroktól műveletek megszakad, és kihagyja a fennmaradó műveletek végrehajtása leáll.</span><span class="sxs-lookup"><span data-stu-id="632b7-556">The Terminate action stops execution of the workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="632b7-557">Ahhoz például, hogy leállítja állapottal futtató **sikertelen**, használhatja a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="632b7-557">For example, to terminate a run with status **Failed**, you can use the following snippet:</span></span>

```json
"HandleUnexpectedResponse" : {
    "type": "terminate",
    "inputs": {
        "runStatus" : "failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response.",
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="632b7-558">A megszakítási művelet nem érinti a művelet már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="632b7-558">Actions already completed are not affected by the terminate action.</span></span>

|<span data-ttu-id="632b7-559">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-559">Name</span></span>|<span data-ttu-id="632b7-560">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-560">Required</span></span>|<span data-ttu-id="632b7-561">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-561">Type</span></span>|<span data-ttu-id="632b7-562">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="632b7-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="632b7-563">runStatus</span></span>|<span data-ttu-id="632b7-564">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-564">Yes</span></span>|<span data-ttu-id="632b7-565">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-565">String</span></span>|<span data-ttu-id="632b7-566">A futtatási állapot cél.</span><span class="sxs-lookup"><span data-stu-id="632b7-566">The target run status.</span></span> <span data-ttu-id="632b7-567">Vagy **sikertelen** vagy **megszakítva**.</span><span class="sxs-lookup"><span data-stu-id="632b7-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="632b7-568">runError</span><span class="sxs-lookup"><span data-stu-id="632b7-568">runError</span></span>|<span data-ttu-id="632b7-569">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-569">No</span></span>|<span data-ttu-id="632b7-570">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-570">Object</span></span>|<span data-ttu-id="632b7-571">A hiba részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="632b7-571">The error details.</span></span> <span data-ttu-id="632b7-572">Csak támogatott, ha **runStatus** értéke **sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="632b7-572">Only supported when **runStatus** is set to **Failed**.</span></span>|
|<span data-ttu-id="632b7-573">runError kód</span><span class="sxs-lookup"><span data-stu-id="632b7-573">runError code</span></span>|<span data-ttu-id="632b7-574">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-574">No</span></span>|<span data-ttu-id="632b7-575">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-575">String</span></span>|<span data-ttu-id="632b7-576">A futtatási hibakód.</span><span class="sxs-lookup"><span data-stu-id="632b7-576">The run error code.</span></span>|
|<span data-ttu-id="632b7-577">runError üzenet</span><span class="sxs-lookup"><span data-stu-id="632b7-577">runError message</span></span>|<span data-ttu-id="632b7-578">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-578">No</span></span>|<span data-ttu-id="632b7-579">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-579">String</span></span>|<span data-ttu-id="632b7-580">A futtatási hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="632b7-580">The run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="632b7-581">A művelet összeállítása</span><span class="sxs-lookup"><span data-stu-id="632b7-581">Compose action</span></span>

<span data-ttu-id="632b7-582">Az új művelet lehetővé teszi, hogy egy tetszőleges objektum hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="632b7-582">The Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="632b7-583">Az új művelet eredménye a bemenet kiértékelése eredményét.</span><span class="sxs-lookup"><span data-stu-id="632b7-583">The output of the compose action is the result of evaluating its inputs.</span></span> <span data-ttu-id="632b7-584">Például a compose művelet segítségével több művelet kimenetének egyesítése:</span><span class="sxs-lookup"><span data-stu-id="632b7-584">For example, you can use the compose action to merge outputs of multiple actions:</span></span>

```json
"composeUserRecord" : {
    "type": "compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="632b7-585">A **Compose** művelet is segítségével hozza létre a kimenetet, többek között az objektumok, tömbök és bármely más típusból XML és bináris hasonlóan a logic apps natívan támogatja.</span><span class="sxs-lookup"><span data-stu-id="632b7-585">The **Compose** action can be used to construct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="632b7-586">Tábla művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-586">Table action</span></span>

<span data-ttu-id="632b7-587">A `table` lehetővé teszi, hogy elemeket tömbje alakíthatja át egy **CSV** vagy **HTML** tábla.</span><span class="sxs-lookup"><span data-stu-id="632b7-587">The `table` allows you to convert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="632b7-588">Tegyük fel, hogy @triggerBodyvan)</span><span class="sxs-lookup"><span data-stu-id="632b7-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="632b7-589">A művelet kell definiálni, így</span><span class="sxs-lookup"><span data-stu-id="632b7-589">And let the action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="632b7-590">A fenti akkor az eredmény</span><span class="sxs-lookup"><span data-stu-id="632b7-590">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="632b7-591">id</span><span class="sxs-lookup"><span data-stu-id="632b7-591">id</span></span></th><th><span data-ttu-id="632b7-592">név</span><span class="sxs-lookup"><span data-stu-id="632b7-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="632b7-593">0</span><span class="sxs-lookup"><span data-stu-id="632b7-593">0</span></span></td><td><span data-ttu-id="632b7-594">almák</span><span class="sxs-lookup"><span data-stu-id="632b7-594">apples</span></span></td></tr><tr><td><span data-ttu-id="632b7-595">1</span><span class="sxs-lookup"><span data-stu-id="632b7-595">1</span></span></td><td><span data-ttu-id="632b7-596">narancs</span><span class="sxs-lookup"><span data-stu-id="632b7-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="632b7-597">"</span><span class="sxs-lookup"><span data-stu-id="632b7-597">"</span></span>

<span data-ttu-id="632b7-598">Ahhoz, hogy a tábla testreszabása, explicit módon megadhatja az oszlopok.</span><span class="sxs-lookup"><span data-stu-id="632b7-598">In order to customize the table, you can specify the columns explicitly.</span></span> <span data-ttu-id="632b7-599">Példa:</span><span class="sxs-lookup"><span data-stu-id="632b7-599">For example:</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [{
          "header": "produce id",
          "value": "@item().id"
        },{
          "header": "description",
          "value": "@concat('fresh ', item().name)"
        }]
    }
}
```

<span data-ttu-id="632b7-600">A fenti akkor az eredmény</span><span class="sxs-lookup"><span data-stu-id="632b7-600">The above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="632b7-601">Készítsen azonosítója</span><span class="sxs-lookup"><span data-stu-id="632b7-601">produce id</span></span></th><th><span data-ttu-id="632b7-602">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="632b7-603">0</span><span class="sxs-lookup"><span data-stu-id="632b7-603">0</span></span></td><td><span data-ttu-id="632b7-604">friss almák</span><span class="sxs-lookup"><span data-stu-id="632b7-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="632b7-605">1</span><span class="sxs-lookup"><span data-stu-id="632b7-605">1</span></span></td><td><span data-ttu-id="632b7-606">friss narancs</span><span class="sxs-lookup"><span data-stu-id="632b7-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="632b7-607">"</span><span class="sxs-lookup"><span data-stu-id="632b7-607">"</span></span>

<span data-ttu-id="632b7-608">Ha a `from` tulajdonság értéke üres tömb, a program üres táblát kimenete.</span><span class="sxs-lookup"><span data-stu-id="632b7-608">If the `from` property value is an empty array, the output is an empty table.</span></span>

|<span data-ttu-id="632b7-609">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-609">Name</span></span>|<span data-ttu-id="632b7-610">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-610">Required</span></span>|<span data-ttu-id="632b7-611">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-611">Type</span></span>|<span data-ttu-id="632b7-612">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="632b7-613">A</span><span class="sxs-lookup"><span data-stu-id="632b7-613">from</span></span>|<span data-ttu-id="632b7-614">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-614">Yes</span></span>|<span data-ttu-id="632b7-615">A tömb</span><span class="sxs-lookup"><span data-stu-id="632b7-615">Array</span></span>|<span data-ttu-id="632b7-616">A forrástömb.</span><span class="sxs-lookup"><span data-stu-id="632b7-616">The source array.</span></span>|
|<span data-ttu-id="632b7-617">Formátumban</span><span class="sxs-lookup"><span data-stu-id="632b7-617">format</span></span>|<span data-ttu-id="632b7-618">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-618">Yes</span></span>|<span data-ttu-id="632b7-619">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-619">String</span></span>|<span data-ttu-id="632b7-620">A formátum vagy **CSV** vagy **HTML**.</span><span class="sxs-lookup"><span data-stu-id="632b7-620">The format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="632b7-621">oszlopok</span><span class="sxs-lookup"><span data-stu-id="632b7-621">columns</span></span>|<span data-ttu-id="632b7-622">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-622">No</span></span>|<span data-ttu-id="632b7-623">A tömb</span><span class="sxs-lookup"><span data-stu-id="632b7-623">Array</span></span>|<span data-ttu-id="632b7-624">Az oszlopok.</span><span class="sxs-lookup"><span data-stu-id="632b7-624">The columns.</span></span> <span data-ttu-id="632b7-625">Lehetővé teszi, hogy a tábla alapértelmezett alakját felülbírálására.</span><span class="sxs-lookup"><span data-stu-id="632b7-625">Allows to override the default shape of the table.</span></span>|
|<span data-ttu-id="632b7-626">Oszlopfejléc</span><span class="sxs-lookup"><span data-stu-id="632b7-626">column header</span></span>|<span data-ttu-id="632b7-627">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-627">No</span></span>|<span data-ttu-id="632b7-628">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-628">String</span></span>|<span data-ttu-id="632b7-629">Az oszlop fejlécében.</span><span class="sxs-lookup"><span data-stu-id="632b7-629">The header of the column.</span></span>|
|<span data-ttu-id="632b7-630">oszlop értéke</span><span class="sxs-lookup"><span data-stu-id="632b7-630">column value</span></span>|<span data-ttu-id="632b7-631">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-631">Yes</span></span>|<span data-ttu-id="632b7-632">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-632">String</span></span>|<span data-ttu-id="632b7-633">Az oszlop értéke.</span><span class="sxs-lookup"><span data-stu-id="632b7-633">The value of the column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="632b7-634">Munkafolyamat-művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-634">Workflow action</span></span>   

|<span data-ttu-id="632b7-635">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-635">Name</span></span>|<span data-ttu-id="632b7-636">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-636">Required</span></span>|<span data-ttu-id="632b7-637">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-637">Type</span></span>|<span data-ttu-id="632b7-638">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-639">állomás azonosítója</span><span class="sxs-lookup"><span data-stu-id="632b7-639">host id</span></span>|<span data-ttu-id="632b7-640">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-640">Yes</span></span>|<span data-ttu-id="632b7-641">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-641">String</span></span>|<span data-ttu-id="632b7-642">A munkafolyamatot, amely a hívni kívánt erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="632b7-642">The resource ID of the workflow that you want to call.</span></span>|  
|<span data-ttu-id="632b7-643">állomás Eseményindító_neve</span><span class="sxs-lookup"><span data-stu-id="632b7-643">host triggerName</span></span>|<span data-ttu-id="632b7-644">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-644">Yes</span></span>|<span data-ttu-id="632b7-645">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-645">String</span></span>|<span data-ttu-id="632b7-646">A meghívni kívánt eseményindító nevét.</span><span class="sxs-lookup"><span data-stu-id="632b7-646">The name of the trigger that you want to invoke.</span></span>|  
|<span data-ttu-id="632b7-647">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-647">queries</span></span>|<span data-ttu-id="632b7-648">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-648">No</span></span>|<span data-ttu-id="632b7-649">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-649">Object</span></span>|<span data-ttu-id="632b7-650">A lekérdezési paraméterek hozzáadása az URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-650">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="632b7-651">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="632b7-652">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-652">headers</span></span>|<span data-ttu-id="632b7-653">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-653">No</span></span>|<span data-ttu-id="632b7-654">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-654">Object</span></span>|<span data-ttu-id="632b7-655">A fejlécek, a kérelemben küldött mindegyikének jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-655">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-656">Ha például a nyelv, és írja be a kérelem:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="632b7-656">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="632b7-657">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-657">body</span></span>|<span data-ttu-id="632b7-658">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-658">No</span></span>|<span data-ttu-id="632b7-659">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-659">Object</span></span>|<span data-ttu-id="632b7-660">A végpontnak küldött hasznos jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-660">Represents the payload sent to the endpoint.</span></span>|  
  
```json
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName " : "mytrigger001"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
    }
```
  
<span data-ttu-id="632b7-661">Hozzáférés-ellenőrzést a munkafolyamat készült \(pontosabban az eseményindító\), ami azt jelenti, hozzá kell férnie a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="632b7-661">An access check is made on the workflow \(more specifically, the trigger\), meaning you need access to the workflow.</span></span>  
  
<span data-ttu-id="632b7-662">A kimeneteinek a `workflow` művelet alapján a meghatározott a `response` alárendelt munkafolyamat műveletét.</span><span class="sxs-lookup"><span data-stu-id="632b7-662">The outputs from the `workflow` action are based on what you defined in the `response` action in the child workflow.</span></span> <span data-ttu-id="632b7-663">Ha nincs megadva, a `response` műveletet, majd a kimenetek üresek.</span><span class="sxs-lookup"><span data-stu-id="632b7-663">If you have not defined any `response` action, then the outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="632b7-664">Függvény művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-664">Function action</span></span>   

|<span data-ttu-id="632b7-665">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-665">Name</span></span>|<span data-ttu-id="632b7-666">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-666">Required</span></span>|<span data-ttu-id="632b7-667">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-667">Type</span></span>|<span data-ttu-id="632b7-668">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-669">függvény azonosítója</span><span class="sxs-lookup"><span data-stu-id="632b7-669">function id</span></span>|<span data-ttu-id="632b7-670">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-670">Yes</span></span>|<span data-ttu-id="632b7-671">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-671">String</span></span>|<span data-ttu-id="632b7-672">A függvény meghívása kívánt erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="632b7-672">The resource ID of the function that you want to invoke.</span></span>|  
|<span data-ttu-id="632b7-673">Módszer</span><span class="sxs-lookup"><span data-stu-id="632b7-673">method</span></span>|<span data-ttu-id="632b7-674">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-674">No</span></span>|<span data-ttu-id="632b7-675">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-675">String</span></span>|<span data-ttu-id="632b7-676">A függvény hívásához használt HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="632b7-676">The HTTP method used to invoke the function.</span></span> <span data-ttu-id="632b7-677">Alapértelmezés szerint van `POST` Ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="632b7-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="632b7-678">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="632b7-678">queries</span></span>|<span data-ttu-id="632b7-679">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-679">No</span></span>|<span data-ttu-id="632b7-680">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-680">Object</span></span>|<span data-ttu-id="632b7-681">A lekérdezési paraméterek hozzáadása az URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-681">Represents the query parameters to add to the URL.</span></span> <span data-ttu-id="632b7-682">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` URL-címét.</span><span class="sxs-lookup"><span data-stu-id="632b7-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` to the URL.</span></span>|  
|<span data-ttu-id="632b7-683">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="632b7-683">headers</span></span>|<span data-ttu-id="632b7-684">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-684">No</span></span>|<span data-ttu-id="632b7-685">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-685">Object</span></span>|<span data-ttu-id="632b7-686">A fejlécek, a kérelemben küldött mindegyikének jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-686">Represents each of the headers that is sent to the request.</span></span> <span data-ttu-id="632b7-687">Ahhoz például, hogy állítsa be a nyelvét és típusát kérelem: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="632b7-687">For example, to set the language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="632b7-688">Törzs</span><span class="sxs-lookup"><span data-stu-id="632b7-688">body</span></span>|<span data-ttu-id="632b7-689">Nem</span><span class="sxs-lookup"><span data-stu-id="632b7-689">No</span></span>|<span data-ttu-id="632b7-690">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-690">Object</span></span>|<span data-ttu-id="632b7-691">A végpontnak küldött hasznos jelöli.</span><span class="sxs-lookup"><span data-stu-id="632b7-691">Represents the payload sent to the endpoint.</span></span>|  

```json
"myfunc" : {
    "type" : "Function",
    "inputs" : {
        "function" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Web/sites/myfuncapp/functions/myfunc"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },  
        "headers" : {
            "x-ms-date" : "@utcnow()"
        },
        "method" : "POST",
    "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "runAfter": {}
}
```

<span data-ttu-id="632b7-692">A logikai alkalmazás mentésekor végezzük a hivatkozott függvény néhány ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="632b7-692">When you save the logic app, we perform some checks on the referenced function:</span></span>
-   <span data-ttu-id="632b7-693">A funkció eléréséhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="632b7-693">You need to have access to the function.</span></span>
-   <span data-ttu-id="632b7-694">Szabványos HTTP-eseményindítóval vagy általános JSON webhook eseményindító csak engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="632b7-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="632b7-695">A megadott útvonal nem tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="632b7-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="632b7-696">Csak "függvény" és "névtelen" jogosultsági szint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="632b7-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="632b7-697">Az eseményindító URL-cím lekérése, gyorsítótárazva, és futásidőben használja.</span><span class="sxs-lookup"><span data-stu-id="632b7-697">The trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="632b7-698">Így bármilyen műveletet érvényteleníti a gyorsítótárazott URL-címe, ha a művelet futásidőben nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="632b7-698">So if any operation invalidates the cached URL, the action fails at runtime.</span></span> <span data-ttu-id="632b7-699">Ez, mentse a logikai alkalmazást újra, és emiatt logikai alkalmazás kérhető le, és az indítási URL-cím gyorsítótárazása újra.</span><span class="sxs-lookup"><span data-stu-id="632b7-699">To work around this, save the logic app again, which will cause logic app to retrieve and cache the trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="632b7-700">Gyűjtemény műveletek (hatókörök és hurkok)</span><span class="sxs-lookup"><span data-stu-id="632b7-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="632b7-701">Néhány művelettípusok műveletek belül maguk is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="632b7-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="632b7-702">A gyűjteményen belül hivatkozás műveletek közvetlenül a gyűjtemény kívül lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="632b7-702">Reference actions within a collection can be referenced directly outside of the collection.</span></span> <span data-ttu-id="632b7-703">Ha az Ön által definiált `http` hatókörben, `@body('http')` a munkafolyamaton belül bárhol továbbra is érvényes.</span><span class="sxs-lookup"><span data-stu-id="632b7-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="632b7-704">A gyűjteményen belül műveletek is `runAfter` belül ugyanaz a gyűjtemény csak más műveleteket.</span><span class="sxs-lookup"><span data-stu-id="632b7-704">Actions within a collection can `runAfter` only other actions within the same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="632b7-705">Hatókör művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-705">Scope action</span></span>

<span data-ttu-id="632b7-706">A `scope` művelet lehetővé teszi, hogy logikailag a munkafolyamat műveleteit.</span><span class="sxs-lookup"><span data-stu-id="632b7-706">The `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="632b7-707">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-707">Name</span></span>|<span data-ttu-id="632b7-708">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-708">Required</span></span>|<span data-ttu-id="632b7-709">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-709">Type</span></span>|<span data-ttu-id="632b7-710">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-711">műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-711">actions</span></span>|<span data-ttu-id="632b7-712">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-712">Yes</span></span>|<span data-ttu-id="632b7-713">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-713">Object</span></span>|<span data-ttu-id="632b7-714">Belső műveletek végrehajtásához a hatókörén belül</span><span class="sxs-lookup"><span data-stu-id="632b7-714">Inner actions to execute within the scope</span></span>|

```json
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```

## <a name="foreach-action"></a><span data-ttu-id="632b7-715">ForEach-művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-715">ForEach action</span></span>

<span data-ttu-id="632b7-716">Ez a ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="632b7-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="632b7-717">Alapértelmezés szerint a foreach hurok végrehajtja a párhuzamos (20 végrehajtások párhuzamosan egyszerre).</span><span class="sxs-lookup"><span data-stu-id="632b7-717">By default, the foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="632b7-718">Végrehajtási szabályok használatával beállíthatja a `operationOptions` paraméter.</span><span class="sxs-lookup"><span data-stu-id="632b7-718">You can set execution rules using the `operationOptions` parameter.</span></span>

|<span data-ttu-id="632b7-719">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-719">Name</span></span>|<span data-ttu-id="632b7-720">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-720">Required</span></span>|<span data-ttu-id="632b7-721">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-721">Type</span></span>|<span data-ttu-id="632b7-722">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-723">műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-723">actions</span></span>|<span data-ttu-id="632b7-724">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-724">Yes</span></span>|<span data-ttu-id="632b7-725">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-725">Object</span></span>|<span data-ttu-id="632b7-726">Belső műveletek végrehajtásához a hurkon belül</span><span class="sxs-lookup"><span data-stu-id="632b7-726">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="632b7-727">foreach</span><span class="sxs-lookup"><span data-stu-id="632b7-727">foreach</span></span>|<span data-ttu-id="632b7-728">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-728">Yes</span></span>|<span data-ttu-id="632b7-729">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-729">string</span></span>|<span data-ttu-id="632b7-730">Az ismétlés a tömb</span><span class="sxs-lookup"><span data-stu-id="632b7-730">The array to iterate over</span></span>|
|<span data-ttu-id="632b7-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="632b7-731">operationOptions</span></span>|<span data-ttu-id="632b7-732">nem</span><span class="sxs-lookup"><span data-stu-id="632b7-732">no</span></span>|<span data-ttu-id="632b7-733">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-733">string</span></span>|<span data-ttu-id="632b7-734">Viselkedés művelet lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="632b7-734">Any operation options for behavior.</span></span> <span data-ttu-id="632b7-735">Jelenleg csak `sequential` ismétlési végrehajtani egymás után (az alapértelmezés lesz párhuzamos)</span><span class="sxs-lookup"><span data-stu-id="632b7-735">Currently only supports `sequential` to execute iterations sequentially (default behavior is parallel)</span></span>|

```json
"forEach_email": {
    "type": "foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter":{
        "email_filter": [ "Succeeded" ]
    }
}
```

## <a name="until-action"></a><span data-ttu-id="632b7-736">Amíg a művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-736">Until action</span></span>

<span data-ttu-id="632b7-737">Ismétlési művelet belső műveleteket hajtja végre, amíg feltétel eredménye igaz.</span><span class="sxs-lookup"><span data-stu-id="632b7-737">This looping action executes inner actions until a condition results to true.</span></span>

|<span data-ttu-id="632b7-738">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-738">Name</span></span>|<span data-ttu-id="632b7-739">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-739">Required</span></span>|<span data-ttu-id="632b7-740">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-740">Type</span></span>|<span data-ttu-id="632b7-741">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-742">műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-742">actions</span></span>|<span data-ttu-id="632b7-743">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-743">Yes</span></span>|<span data-ttu-id="632b7-744">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-744">Object</span></span>|<span data-ttu-id="632b7-745">Belső műveletek végrehajtásához a hurkon belül</span><span class="sxs-lookup"><span data-stu-id="632b7-745">Inner actions to execute within the loop</span></span>|
|<span data-ttu-id="632b7-746">kifejezés</span><span class="sxs-lookup"><span data-stu-id="632b7-746">expression</span></span>|<span data-ttu-id="632b7-747">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-747">Yes</span></span>|<span data-ttu-id="632b7-748">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-748">string</span></span>|<span data-ttu-id="632b7-749">A kifejezés kiértékelése mindegyik iteráció után</span><span class="sxs-lookup"><span data-stu-id="632b7-749">The expression to evaluate after each iteration</span></span>|
|<span data-ttu-id="632b7-750">Korlát</span><span class="sxs-lookup"><span data-stu-id="632b7-750">limit</span></span>|<span data-ttu-id="632b7-751">igen</span><span class="sxs-lookup"><span data-stu-id="632b7-751">yes</span></span>|<span data-ttu-id="632b7-752">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-752">Object</span></span>|<span data-ttu-id="632b7-753">A hurok - legalább egy korlát korlátok kell megadni.</span><span class="sxs-lookup"><span data-stu-id="632b7-753">The limits for the loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="632b7-754">Száma</span><span class="sxs-lookup"><span data-stu-id="632b7-754">count</span></span>|<span data-ttu-id="632b7-755">nem</span><span class="sxs-lookup"><span data-stu-id="632b7-755">no</span></span>|<span data-ttu-id="632b7-756">int</span><span class="sxs-lookup"><span data-stu-id="632b7-756">int</span></span>|<span data-ttu-id="632b7-757">A korlát végrehajtható ismétlések száma</span><span class="sxs-lookup"><span data-stu-id="632b7-757">The limit to the number of iterations that can be performed</span></span>|
|<span data-ttu-id="632b7-758">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="632b7-758">timeout</span></span>|<span data-ttu-id="632b7-759">nem</span><span class="sxs-lookup"><span data-stu-id="632b7-759">no</span></span>|<span data-ttu-id="632b7-760">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-760">string</span></span>|<span data-ttu-id="632b7-761">Mennyi ideig kell a hurok időtúllépés.</span><span class="sxs-lookup"><span data-stu-id="632b7-761">The timeout for how long it should loop.</span></span>  <span data-ttu-id="632b7-762">ISO 8601 formátumban</span><span class="sxs-lookup"><span data-stu-id="632b7-762">ISO 8601 format</span></span>|


```json
 "Until_succeeded": {
    "actions": {
        "Http": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {},
    "type": "Until"
}
```

## <a name="conditions---if-action"></a><span data-ttu-id="632b7-763">Feltételek – Ha a művelet</span><span class="sxs-lookup"><span data-stu-id="632b7-763">Conditions - If Action</span></span>

<span data-ttu-id="632b7-764">A `If` művelet lehetővé teszi, hogy olyan feltétel értékelése, majd hajtsa végre egy ágat, hogy a kifejezés eredménye alapján `true`.</span><span class="sxs-lookup"><span data-stu-id="632b7-764">The `If` action lets you evaluate a condition and execute a branch based on whether the expression evaluates to `true`.</span></span>

|<span data-ttu-id="632b7-765">Név</span><span class="sxs-lookup"><span data-stu-id="632b7-765">Name</span></span>|<span data-ttu-id="632b7-766">Szükséges</span><span class="sxs-lookup"><span data-stu-id="632b7-766">Required</span></span>|<span data-ttu-id="632b7-767">Típus</span><span class="sxs-lookup"><span data-stu-id="632b7-767">Type</span></span>|<span data-ttu-id="632b7-768">Leírás</span><span class="sxs-lookup"><span data-stu-id="632b7-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="632b7-769">műveletek</span><span class="sxs-lookup"><span data-stu-id="632b7-769">actions</span></span>|<span data-ttu-id="632b7-770">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-770">Yes</span></span>|<span data-ttu-id="632b7-771">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-771">Object</span></span>|<span data-ttu-id="632b7-772">Belső műveletek hajtható végre, ha a kifejezés eredménye`true`</span><span class="sxs-lookup"><span data-stu-id="632b7-772">Inner actions to execute when expression evaluates to `true`</span></span>|
|<span data-ttu-id="632b7-773">kifejezés</span><span class="sxs-lookup"><span data-stu-id="632b7-773">expression</span></span>|<span data-ttu-id="632b7-774">Igen</span><span class="sxs-lookup"><span data-stu-id="632b7-774">Yes</span></span>|<span data-ttu-id="632b7-775">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="632b7-775">string</span></span>|<span data-ttu-id="632b7-776">A kiértékelendő kifejezés</span><span class="sxs-lookup"><span data-stu-id="632b7-776">The expression to evaluate</span></span>|
|<span data-ttu-id="632b7-777">más</span><span class="sxs-lookup"><span data-stu-id="632b7-777">else</span></span>|<span data-ttu-id="632b7-778">nem</span><span class="sxs-lookup"><span data-stu-id="632b7-778">no</span></span>|<span data-ttu-id="632b7-779">Objektum</span><span class="sxs-lookup"><span data-stu-id="632b7-779">Object</span></span>|<span data-ttu-id="632b7-780">Belső műveletek hajtható végre, ha a kifejezés eredménye`false`</span><span class="sxs-lookup"><span data-stu-id="632b7-780">Inner actions to execute when expression evaluates to `false`</span></span>|
  
```json
"My_condition": {
    "actions": {
        "If_true": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {},
                "type": "Http"
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {},
    "type": "If"
}
```  
  
<span data-ttu-id="632b7-781">Az alábbi táblázat példákat feltételek használatát kifejezések egy műveletben:</span><span class="sxs-lookup"><span data-stu-id="632b7-781">The following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="632b7-782">JSON-érték</span><span class="sxs-lookup"><span data-stu-id="632b7-782">JSON value</span></span>|<span data-ttu-id="632b7-783">eredménye</span><span class="sxs-lookup"><span data-stu-id="632b7-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="632b7-784">Bármely érték, amely true értékre állítására értékelné ki ezt az állapotot adja át.</span><span class="sxs-lookup"><span data-stu-id="632b7-784">Any value that would evaluate to true causes this condition to pass.</span></span> <span data-ttu-id="632b7-785">Csak a logikai kifejezések használhatók.</span><span class="sxs-lookup"><span data-stu-id="632b7-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="632b7-786">Más típusúra átalakítása logikai érték, használja a függvényt `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="632b7-786">To convert other types to Boolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="632b7-787">Összehasonlító függvény támogatott.</span><span class="sxs-lookup"><span data-stu-id="632b7-787">Comparison functions are supported.</span></span> <span data-ttu-id="632b7-788">Itt például a művelet csak során végrehajt act1 eredménye meghaladja a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="632b7-788">For the example here, the action only executes when the output of act1 is greater than the threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="632b7-789">Logika funkciók is támogatottak a beágyazott logikai kifejezésen létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="632b7-789">Logic functions are also supported to create nested Boolean expressions.</span></span> <span data-ttu-id="632b7-790">Ebben az esetben a művelet hajtja végre, amikor act1 eredménye 100 alatt vagy a küszöbérték felett.</span><span class="sxs-lookup"><span data-stu-id="632b7-790">In this case, the action executes when the output of act1 is above the threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="632b7-791">Tömb funkciók segítségével ellenőrizze, hogy egy tömb rendelkezik-e az elemeket.</span><span class="sxs-lookup"><span data-stu-id="632b7-791">You can use array functions to check if an array has any items.</span></span> <span data-ttu-id="632b7-792">Ebben az esetben a művelet végrehajtása, ha a hibák tömb üres.</span><span class="sxs-lookup"><span data-stu-id="632b7-792">In this case, the action executes when the errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="632b7-793">Hiba – nem egy érvényes, mert @ van szükség a feltétel feltételeket.</span><span class="sxs-lookup"><span data-stu-id="632b7-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="632b7-794">Ha a feltétel sikeresen, a feltétel jelölése `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="632b7-794">If a condition evaluates successfully, the condition is marked as `Succeeded`.</span></span> <span data-ttu-id="632b7-795">Műveletek belül vagy a `actions` vagy `else` objektumok értékelhetők `Succeeded` hajtotta végre, és sikeresen befejeződött, `Failed` hajtotta végre, és nem sikerült, vagy `Skipped` mikor nem hajtotta végre a fiókirodában.</span><span class="sxs-lookup"><span data-stu-id="632b7-795">Actions within either the `actions` or `else` objects evaluate to `Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="632b7-796">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="632b7-796">Next steps</span></span>

[<span data-ttu-id="632b7-797">A munkafolyamat szolgáltatás REST API</span><span class="sxs-lookup"><span data-stu-id="632b7-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
