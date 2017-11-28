---
title: "aaaWorkflow műveletek és eseményindítók - Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 857927b7d7df3fc9cdc4931ffdb613efde0db9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="workflow-actions-and-triggers-for-azure-logic-apps"></a><span data-ttu-id="1b078-102">Munkafolyamat-műveleteket, és az Azure Logic Apps eseményindítók</span><span class="sxs-lookup"><span data-stu-id="1b078-102">Workflow actions and triggers for Azure Logic Apps</span></span>

<span data-ttu-id="1b078-103">A Logic apps eseményindítók és műveletek alkotják.</span><span class="sxs-lookup"><span data-stu-id="1b078-103">Logic apps consist of triggers and actions.</span></span> <span data-ttu-id="1b078-104">Nincsenek eseményindítók hat típusú.</span><span class="sxs-lookup"><span data-stu-id="1b078-104">There are six types of triggers.</span></span> <span data-ttu-id="1b078-105">Minden különböző felület és a különböző viselkedés rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1b078-105">Each type has different interface and different behavior.</span></span> <span data-ttu-id="1b078-106">Hello hello részleteinek megtekintésével is olvashat egyéb részletek [Munkafolyamatdefiníciós nyelve](logic-apps-workflow-definition-language.md).</span><span class="sxs-lookup"><span data-stu-id="1b078-106">You can also learn about other details by looking at hello details of hello [Workflow Definition Language](logic-apps-workflow-definition-language.md).</span></span>  
  
<span data-ttu-id="1b078-107">További információ az eseményindítók és műveletek és hogyan használhatja őket toobuild logic apps tooimprove toolearn olvasni az üzleti folyamatokat és munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="1b078-107">Read on toolearn more about triggers and actions and how you might use them toobuild logic apps tooimprove your business processes and workflows.</span></span>  
  
### <a name="triggers"></a><span data-ttu-id="1b078-108">Eseményindítók</span><span class="sxs-lookup"><span data-stu-id="1b078-108">Triggers</span></span>  

<span data-ttu-id="1b078-109">Egy eseményindító is kezdeményezhető a logic app munkafolyamatot futtató hello hívások határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1b078-109">A trigger specifies hello calls that can initiate a run of your logic app workflow.</span></span> <span data-ttu-id="1b078-110">Az alábbiakban hello két különböző módon tooinitiate a munkafolyamatot futtató:</span><span class="sxs-lookup"><span data-stu-id="1b078-110">Here are hello two different ways tooinitiate a run of your workflow:</span></span>  
  
-   <span data-ttu-id="1b078-111">A lekérdezési eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-111">A polling trigger</span></span>  

-   <span data-ttu-id="1b078-112">A leküldéses eseményindító - hívó hello által [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span><span class="sxs-lookup"><span data-stu-id="1b078-112">A push trigger - by calling hello [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows)</span></span>  
  
<span data-ttu-id="1b078-113">Indítók a legfelső szintű elemet tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="1b078-113">All triggers contain these top-level elements:</span></span>  
  
```json
"<name-of-the-trigger>" : {
    "type": "<type-of-trigger>",
    "inputs": { <settings-for-the-call> },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Week|Month|Year",
        "interval": "<recurrence interval in units of frequency>"
    },
    "conditions": [ <array-of-required-conditions > ],
    "splitOn" : "<property toocreate runs for>",
    "operationOptions": "<operation options on hello trigger>"
}
```

### <a name="trigger-types-and-their-inputs"></a><span data-ttu-id="1b078-114">Az eseményindítók típusai és azok</span><span class="sxs-lookup"><span data-stu-id="1b078-114">Trigger types and their inputs</span></span>  

<span data-ttu-id="1b078-115">Az ilyen típusú eseményindítók használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b078-115">You can use these types of triggers:</span></span>
  
-   <span data-ttu-id="1b078-116">**Kérelem** \- teszi hello logic app, toocall végpont</span><span class="sxs-lookup"><span data-stu-id="1b078-116">**Request** \- Makes hello logic app an endpoint for you toocall</span></span>  
  
-   <span data-ttu-id="1b078-117">**Ismétlődés** \- következik be a meghatározott ütemezés szerint</span><span class="sxs-lookup"><span data-stu-id="1b078-117">**Recurrence** \- Fires based on a defined schedule</span></span>  
  
-   <span data-ttu-id="1b078-118">**HTTP** \- HTTP webes végpont kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="1b078-118">**HTTP** \- Polls an HTTP web endpoint.</span></span> <span data-ttu-id="1b078-119">hello HTTP-végpont meg kell felelnie a tooa meghatározott eseményindító szerződés \- használva egy 202\-aszinkron mintát, vagy egy tömb vissza</span><span class="sxs-lookup"><span data-stu-id="1b078-119">hello HTTP endpoint must conform tooa specific triggering contract \- either by using a 202\-async pattern, or by returning an array</span></span>  
  
-   <span data-ttu-id="1b078-120">**ApiConnection** \- indul el, például hello HTTP szavazások, azonban kihasználja hello [Microsoft által felügyelt API-k](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="1b078-120">**ApiConnection** \- Polls like hello HTTP trigger, however, it takes advantage of hello [Microsoft-managed APIs](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>  
  
-   <span data-ttu-id="1b078-121">**HTTPWebhook** \- megnyit egy végpontot, hasonló toohello kézi indítási, azonban ez szükségessé teszi ki tooa megadott URL-cím tooregister és regisztrációjának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="1b078-121">**HTTPWebhook** \- Opens an endpoint, similar toohello Manual trigger, however, it also calls out tooa specified URL tooregister and unregister</span></span>  
  
-   <span data-ttu-id="1b078-122">**ApiConnectionWebhook** \- működik, mint ha kihasználja a Microsoft által felügyelt API-k hello HTTPWebhook eseményindító hello</span><span class="sxs-lookup"><span data-stu-id="1b078-122">**ApiConnectionWebhook** \- Operates like hello HTTPWebhook trigger by taking advantage of hello Microsoft-managed APIs</span></span>       
    <span data-ttu-id="1b078-123">Minden eseményindító más rendelkezik **bemenetek** , amely meghatározza, hogy a viselkedését.</span><span class="sxs-lookup"><span data-stu-id="1b078-123">Each trigger type has a different set of **inputs** that defines its behavior.</span></span>  
  
## <a name="request-trigger"></a><span data-ttu-id="1b078-124">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-124">Request trigger</span></span>  

<span data-ttu-id="1b078-125">Ehhez az eseményindítóhoz szolgál egy végpontot, amely meghívja a egy HTTP-kérelem tooinvoke keresztül a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1b078-125">This trigger serves as an endpoint that you call via an HTTP Request tooinvoke your logic app.</span></span> <span data-ttu-id="1b078-126">A kérelem eseményindító ebben a példában néz ki:</span><span class="sxs-lookup"><span data-stu-id="1b078-126">A request trigger looks like this example:</span></span>  
  
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

<span data-ttu-id="1b078-127">Szerepel továbbá egy nem kötelező tulajdonság neve **séma**:</span><span class="sxs-lookup"><span data-stu-id="1b078-127">There is also an optional property called **schema**:</span></span>  
  
|<span data-ttu-id="1b078-128">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-128">Element name</span></span>|<span data-ttu-id="1b078-129">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-129">Required</span></span>|<span data-ttu-id="1b078-130">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-130">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="1b078-131">Séma</span><span class="sxs-lookup"><span data-stu-id="1b078-131">schema</span></span>|<span data-ttu-id="1b078-132">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-132">No</span></span>|<span data-ttu-id="1b078-133">A JSON-séma, amely megvizsgálja a bejövő kérelem hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-133">A JSON schema that validates hello incoming request.</span></span> <span data-ttu-id="1b078-134">Útmutatás nyújtása a soron következő munkafolyamat lépéseket tudja, milyen tulajdonságok tooreference hasznos.</span><span class="sxs-lookup"><span data-stu-id="1b078-134">Useful for helping subsequent workflow steps know which properties tooreference.</span></span>|

<span data-ttu-id="1b078-135">tooinvoke ezt a végpontot kell toocall hello *listCallbackUrl* API.</span><span class="sxs-lookup"><span data-stu-id="1b078-135">tooinvoke this endpoint, you need toocall hello *listCallbackUrl* API.</span></span> <span data-ttu-id="1b078-136">Lásd: [munkafolyamat szolgáltatás REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span><span class="sxs-lookup"><span data-stu-id="1b078-136">See [Workflow Service REST API](https://docs.microsoft.com/rest/api/logic/workflows).</span></span>  
  
## <a name="recurrence-trigger"></a><span data-ttu-id="1b078-137">Ismétlődés eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-137">Recurrence trigger</span></span>  

<span data-ttu-id="1b078-138">Ismétlődés eseményindító egyike, amelyek egy meghatározott ütemezés alapján fut.</span><span class="sxs-lookup"><span data-stu-id="1b078-138">A Recurrence trigger is one that runs based on a defined schedule.</span></span> <span data-ttu-id="1b078-139">Ez a példa ilyen eseményindító nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="1b078-139">Such a trigger might look like this example:</span></span>  

```json
"dailyReport" : {
    "type": "recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": "1"
    }
}
```

<span data-ttu-id="1b078-140">Ahogy látja, akkor egy egyszerű módon toorun munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="1b078-140">As you can see, it is a simple way toorun a workflow.</span></span>  
  
|<span data-ttu-id="1b078-141">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-141">Element name</span></span>|<span data-ttu-id="1b078-142">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-142">Required</span></span>|<span data-ttu-id="1b078-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-143">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="1b078-144">frequency</span><span class="sxs-lookup"><span data-stu-id="1b078-144">frequency</span></span>|<span data-ttu-id="1b078-145">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-145">Yes</span></span>|<span data-ttu-id="1b078-146">Milyen gyakran hello eseményindító végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="1b078-146">How often hello trigger executes.</span></span> <span data-ttu-id="1b078-147">Használja a lehetséges értékek egyikét: másodperc, perc, óra, nap, hét, hónap vagy év</span><span class="sxs-lookup"><span data-stu-id="1b078-147">Use only one of these possible values: second, minute, hour, day, week, month, or year</span></span>|  
|<span data-ttu-id="1b078-148">interval</span><span class="sxs-lookup"><span data-stu-id="1b078-148">interval</span></span>|<span data-ttu-id="1b078-149">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-149">Yes</span></span>|<span data-ttu-id="1b078-150">Hello tartozó hello Ismétlődési gyakoriság megadott időköz</span><span class="sxs-lookup"><span data-stu-id="1b078-150">Interval of hello given frequency for hello recurrence</span></span>|  
|<span data-ttu-id="1b078-151">startTime</span><span class="sxs-lookup"><span data-stu-id="1b078-151">startTime</span></span>|<span data-ttu-id="1b078-152">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-152">No</span></span>|<span data-ttu-id="1b078-153">A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.</span><span class="sxs-lookup"><span data-stu-id="1b078-153">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
|<span data-ttu-id="1b078-154">Időzóna</span><span class="sxs-lookup"><span data-stu-id="1b078-154">timeZone</span></span>|<span data-ttu-id="1b078-155">nem</span><span class="sxs-lookup"><span data-stu-id="1b078-155">no</span></span>|<span data-ttu-id="1b078-156">A startTime nélkül a UTC eltolás valósul meg, ha a timeZone szolgál.</span><span class="sxs-lookup"><span data-stu-id="1b078-156">If a startTime is provided without a UTC offset, this timeZone is used.</span></span>|  
  
<span data-ttu-id="1b078-157">Egy eseményindító toostart végrehajtása a jövőbeli hello valamikor is ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="1b078-157">You can also schedule a trigger toostart executing at some point in hello future.</span></span> <span data-ttu-id="1b078-158">Például ha azt szeretné, hogy a heti jelentés minden hétfőn toostart ütemezhet hello logic app toostart minden hétfőn eseményindítót követő hello létrehozásával:</span><span class="sxs-lookup"><span data-stu-id="1b078-158">For example, if you want toostart a weekly report every Monday you can schedule hello logic app toostart every Monday by creating hello following trigger:</span></span>  

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

## <a name="http-trigger"></a><span data-ttu-id="1b078-159">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-159">HTTP trigger</span></span>  

<span data-ttu-id="1b078-160">HTTP-eseményindítók kérdezze le a megadott végpont és hello válasz toodetermine ellenőrizze, hogy hello munkafolyamat végre kell hajtani.</span><span class="sxs-lookup"><span data-stu-id="1b078-160">HTTP triggers poll a specified endpoint and check hello response toodetermine whether hello workflow should be executed.</span></span> <span data-ttu-id="1b078-161">hello bemenetek objektum paraméterek szükséges tooconstruct egy HTTP-hívás hello készlete hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="1b078-161">hello inputs object takes hello set of parameters required tooconstruct an HTTP call:</span></span>  
  
|<span data-ttu-id="1b078-162">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-162">Element name</span></span>|<span data-ttu-id="1b078-163">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-163">Required</span></span>|<span data-ttu-id="1b078-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-164">Description</span></span>|<span data-ttu-id="1b078-165">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-165">Type</span></span>|  
|----------------|------------|---------------|--------|  
|<span data-ttu-id="1b078-166">Módszer</span><span class="sxs-lookup"><span data-stu-id="1b078-166">method</span></span>|<span data-ttu-id="1b078-167">igen</span><span class="sxs-lookup"><span data-stu-id="1b078-167">yes</span></span>|<span data-ttu-id="1b078-168">A következő HTTP-metódus hello egyike lehet: GET, POST, PUT, DELETE, a javítás vagy a HEAD</span><span class="sxs-lookup"><span data-stu-id="1b078-168">Can be one of hello following HTTP methods: GET, POST, PUT, DELETE, PATCH, or HEAD</span></span>|<span data-ttu-id="1b078-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-169">String</span></span>|  
|<span data-ttu-id="1b078-170">URI</span><span class="sxs-lookup"><span data-stu-id="1b078-170">uri</span></span>|<span data-ttu-id="1b078-171">igen</span><span class="sxs-lookup"><span data-stu-id="1b078-171">yes</span></span>|<span data-ttu-id="1b078-172">hello http vagy https-végpont is nevezett.</span><span class="sxs-lookup"><span data-stu-id="1b078-172">hello http or https endpoint that is called.</span></span> <span data-ttu-id="1b078-173">Legfeljebb 2 KB.</span><span class="sxs-lookup"><span data-stu-id="1b078-173">Maximum of 2 kilobytes.</span></span>|<span data-ttu-id="1b078-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-174">String</span></span>|  
|<span data-ttu-id="1b078-175">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-175">queries</span></span>|<span data-ttu-id="1b078-176">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-176">No</span></span>|<span data-ttu-id="1b078-177">Hello lekérdezési paraméterek tooadd toohello URL-CÍMRE képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="1b078-177">An object representing hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="1b078-178">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-178">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|<span data-ttu-id="1b078-179">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-179">Object</span></span>|  
|<span data-ttu-id="1b078-180">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-180">headers</span></span>|<span data-ttu-id="1b078-181">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-181">No</span></span>|<span data-ttu-id="1b078-182">Egyes toohello kérelmet küldött hello fejlécek képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="1b078-182">An object representing each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-183">Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="1b078-183">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|<span data-ttu-id="1b078-184">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-184">Object</span></span>|  
|<span data-ttu-id="1b078-185">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-185">body</span></span>|<span data-ttu-id="1b078-186">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-186">No</span></span>|<span data-ttu-id="1b078-187">Hello hasznos toohello végpont küldött képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="1b078-187">An object representing hello payload that is sent toohello endpoint.</span></span>|<span data-ttu-id="1b078-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-188">Object</span></span>|  
|<span data-ttu-id="1b078-189">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="1b078-189">retryPolicy</span></span>|<span data-ttu-id="1b078-190">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-190">No</span></span>|<span data-ttu-id="1b078-191">Egy objektum, amely lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.</span><span class="sxs-lookup"><span data-stu-id="1b078-191">An object that lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|<span data-ttu-id="1b078-192">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-192">Object</span></span>|  
|<span data-ttu-id="1b078-193">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b078-193">authentication</span></span>|<span data-ttu-id="1b078-194">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-194">No</span></span>|<span data-ttu-id="1b078-195">Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik.</span><span class="sxs-lookup"><span data-stu-id="1b078-195">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="1b078-196">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="1b078-196">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="1b078-197">Ütemező túl van egy több támogatott tulajdonságot: `authority` alapértelmezés szerint ez az érték van `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="1b078-197">Beyond scheduler, there is one more supported property: `authority` By default, this value is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|<span data-ttu-id="1b078-198">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-198">Object</span></span>|  
  
<span data-ttu-id="1b078-199">hello HTTP-eseményindítóval hello HTTP API tooconform az egy adott minta toowork jól igazodik a Logic Apps alkalmazást igényli.</span><span class="sxs-lookup"><span data-stu-id="1b078-199">hello HTTP trigger requires hello HTTP API tooconform with a specific pattern toowork well with your logic app.</span></span> <span data-ttu-id="1b078-200">A következő mezők hello szükségesek hozzá:</span><span class="sxs-lookup"><span data-stu-id="1b078-200">It requires hello following fields:</span></span>  
  
|<span data-ttu-id="1b078-201">Válasz</span><span class="sxs-lookup"><span data-stu-id="1b078-201">Response</span></span>|<span data-ttu-id="1b078-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-202">Description</span></span>|  
|------------|---------------|  
|<span data-ttu-id="1b078-203">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="1b078-203">Status code</span></span>|<span data-ttu-id="1b078-204">Állapotkód 200 \(OK\) toocause futását.</span><span class="sxs-lookup"><span data-stu-id="1b078-204">Status code 200 \(OK\) toocause a run.</span></span> <span data-ttu-id="1b078-205">Bármely más állapotkód futtató nem okoz.</span><span class="sxs-lookup"><span data-stu-id="1b078-205">Any other status code doesn't cause a run.</span></span>|  
|<span data-ttu-id="1b078-206">Próbálja meg újra\-fejléc után</span><span class="sxs-lookup"><span data-stu-id="1b078-206">Retry\-after header</span></span>|<span data-ttu-id="1b078-207">Hány másodperc múlva hello logikai alkalmazás kérdezze le az hello végpont újra.</span><span class="sxs-lookup"><span data-stu-id="1b078-207">Number of seconds until hello logic app polls hello endpoint again.</span></span>|  
|<span data-ttu-id="1b078-208">Hely fejléc</span><span class="sxs-lookup"><span data-stu-id="1b078-208">Location header</span></span>|<span data-ttu-id="1b078-209">hello URL-cím toocall a hello következő lekérdezési időközt.</span><span class="sxs-lookup"><span data-stu-id="1b078-209">hello URL toocall on hello next polling interval.</span></span> <span data-ttu-id="1b078-210">Ha nincs megadva, hello eredeti URL-címet használja.</span><span class="sxs-lookup"><span data-stu-id="1b078-210">If not specified, hello original URL is used.</span></span>|  
  
<span data-ttu-id="1b078-211">Íme néhány példa a kérelmek különböző típusú különböző beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1b078-211">Here are some examples of different behaviors for different types of requests:</span></span>  
  
|<span data-ttu-id="1b078-212">Válaszkód</span><span class="sxs-lookup"><span data-stu-id="1b078-212">Response code</span></span>|<span data-ttu-id="1b078-213">Próbálja meg újra\-után</span><span class="sxs-lookup"><span data-stu-id="1b078-213">Retry\-After</span></span>|<span data-ttu-id="1b078-214">Viselkedés</span><span class="sxs-lookup"><span data-stu-id="1b078-214">Behavior</span></span>|  
|-----------------|----------------|------------|  
|<span data-ttu-id="1b078-215">200</span><span class="sxs-lookup"><span data-stu-id="1b078-215">200</span></span>|<span data-ttu-id="1b078-216">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="1b078-216">\(none\)</span></span>|<span data-ttu-id="1b078-217">Nem érvényes eseményindító, újrapróbálkozási\-után van, kötelező, vagy más hello motor soha nem hello következő kérés lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="1b078-217">Not a valid trigger, Retry\-After is required, or else hello engine never polls for hello next request.</span></span>|  
|<span data-ttu-id="1b078-218">202</span><span class="sxs-lookup"><span data-stu-id="1b078-218">202</span></span>|<span data-ttu-id="1b078-219">60</span><span class="sxs-lookup"><span data-stu-id="1b078-219">60</span></span>|<span data-ttu-id="1b078-220">Nem indítható el, hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="1b078-220">Do not trigger hello workflow.</span></span> <span data-ttu-id="1b078-221">hello következő kísérlet történik, egy perc alatt.</span><span class="sxs-lookup"><span data-stu-id="1b078-221">hello next attempt happens in one minute.</span></span>|  
|<span data-ttu-id="1b078-222">200</span><span class="sxs-lookup"><span data-stu-id="1b078-222">200</span></span>|<span data-ttu-id="1b078-223">10</span><span class="sxs-lookup"><span data-stu-id="1b078-223">10</span></span>|<span data-ttu-id="1b078-224">Hello munkafolyamat futtatásához, és újra keressen további tartalmat 10 másodperc múlva.</span><span class="sxs-lookup"><span data-stu-id="1b078-224">Run hello workflow, and check again for more content in 10 seconds.</span></span>|  
|<span data-ttu-id="1b078-225">400</span><span class="sxs-lookup"><span data-stu-id="1b078-225">400</span></span>|<span data-ttu-id="1b078-226">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="1b078-226">\(none\)</span></span>|<span data-ttu-id="1b078-227">Hibás kérés, hello munkafolyamat nem futnak.</span><span class="sxs-lookup"><span data-stu-id="1b078-227">Bad request, do not run hello workflow.</span></span> <span data-ttu-id="1b078-228">Ha nincs **ismételje meg a házirend** definiált, majd hello alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="1b078-228">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="1b078-229">Az újrapróbálkozások számát hello elérése után hello eseményindító már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="1b078-229">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
|<span data-ttu-id="1b078-230">500</span><span class="sxs-lookup"><span data-stu-id="1b078-230">500</span></span>|<span data-ttu-id="1b078-231">\(egyik sem\)</span><span class="sxs-lookup"><span data-stu-id="1b078-231">\(none\)</span></span>|<span data-ttu-id="1b078-232">Kiszolgálóhiba, akkor futtassa a hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="1b078-232">Server error, do not run hello workflow.</span></span>  <span data-ttu-id="1b078-233">Ha nincs **ismételje meg a házirend** definiált, majd hello alapértelmezett házirend.</span><span class="sxs-lookup"><span data-stu-id="1b078-233">If there is no **Retry Policy** defined, then hello default policy is used.</span></span> <span data-ttu-id="1b078-234">Az újrapróbálkozások számát hello elérése után hello eseményindító már nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="1b078-234">After hello number of retries has been reached, hello trigger is no longer valid.</span></span>|  
  
<span data-ttu-id="1b078-235">egy HTTP-eseményindítóval hello kimenetének ebben a példában látható:</span><span class="sxs-lookup"><span data-stu-id="1b078-235">hello outputs of an HTTP trigger look like this example:</span></span>  
  
|<span data-ttu-id="1b078-236">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-236">Element name</span></span>|<span data-ttu-id="1b078-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-237">Description</span></span>|<span data-ttu-id="1b078-238">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-238">Type</span></span>|  
|----------------|---------------|--------|  
|<span data-ttu-id="1b078-239">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-239">headers</span></span>|<span data-ttu-id="1b078-240">hello fejlécek hello http-válaszok.</span><span class="sxs-lookup"><span data-stu-id="1b078-240">hello headers of hello http response.</span></span>|<span data-ttu-id="1b078-241">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-241">Object</span></span>|  
|<span data-ttu-id="1b078-242">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-242">body</span></span>|<span data-ttu-id="1b078-243">hello hello HTTP-válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="1b078-243">hello body of hello http response.</span></span>|<span data-ttu-id="1b078-244">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-244">Object</span></span>|  
  
## <a name="api-connection-trigger"></a><span data-ttu-id="1b078-245">API-kapcsolat eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-245">API Connection trigger</span></span>  

<span data-ttu-id="1b078-246">hello API-kapcsolat eseményindító hasonló toohello HTTP-eseményindítóval alapvető funkciókat, hogy a rendszer.</span><span class="sxs-lookup"><span data-stu-id="1b078-246">hello API connection trigger is similar toohello HTTP trigger in its basic functionality.</span></span> <span data-ttu-id="1b078-247">Hello paraméterek hello művelet azonosítására szolgáló azonban eltérőek.</span><span class="sxs-lookup"><span data-stu-id="1b078-247">However, hello parameters for identifying hello action are different.</span></span> <span data-ttu-id="1b078-248">Például:</span><span class="sxs-lookup"><span data-stu-id="1b078-248">Here is an example:</span></span>  
  
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

|<span data-ttu-id="1b078-249">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-249">Element name</span></span>|<span data-ttu-id="1b078-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-250">Required</span></span>|<span data-ttu-id="1b078-251">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-251">Type</span></span>|<span data-ttu-id="1b078-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-252">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="1b078-253">állomás</span><span class="sxs-lookup"><span data-stu-id="1b078-253">host</span></span>|<span data-ttu-id="1b078-254">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-254">Yes</span></span>||<span data-ttu-id="1b078-255">hello ApiApp átjáró és az azonosítóját tárolja.</span><span class="sxs-lookup"><span data-stu-id="1b078-255">hello ApiApp hosted gateway and id.</span></span>|  
|<span data-ttu-id="1b078-256">Módszer</span><span class="sxs-lookup"><span data-stu-id="1b078-256">method</span></span>|<span data-ttu-id="1b078-257">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-257">Yes</span></span>|<span data-ttu-id="1b078-258">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-258">String</span></span>|<span data-ttu-id="1b078-259">A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="1b078-259">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="1b078-260">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-260">queries</span></span>|<span data-ttu-id="1b078-261">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-261">No</span></span>|<span data-ttu-id="1b078-262">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-262">Object</span></span>|<span data-ttu-id="1b078-263">Jelöli hello lekérdezési paraméterek toobe hozzáadott toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-263">Represents hello query parameters toobe added toohello URL.</span></span> <span data-ttu-id="1b078-264">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-264">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="1b078-265">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-265">headers</span></span>|<span data-ttu-id="1b078-266">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-266">No</span></span>|<span data-ttu-id="1b078-267">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-267">Object</span></span>|<span data-ttu-id="1b078-268">Egyes toohello kérelmet küldött hello fejlécek jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-268">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-269">Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="1b078-269">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="1b078-270">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-270">body</span></span>|<span data-ttu-id="1b078-271">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-271">No</span></span>|<span data-ttu-id="1b078-272">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-272">Object</span></span>|<span data-ttu-id="1b078-273">Hello hasznos toohello végpont küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-273">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="1b078-274">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="1b078-274">retryPolicy</span></span>|<span data-ttu-id="1b078-275">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-275">No</span></span>|<span data-ttu-id="1b078-276">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-276">Object</span></span>|<span data-ttu-id="1b078-277">Lehetővé teszi a toocustomize hello újrapróbálási viselkedése 4xx vagy 5xx hibákat.</span><span class="sxs-lookup"><span data-stu-id="1b078-277">Allows you toocustomize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="1b078-278">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b078-278">authentication</span></span>|<span data-ttu-id="1b078-279">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-279">No</span></span>|<span data-ttu-id="1b078-280">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-280">Object</span></span>|<span data-ttu-id="1b078-281">Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik.</span><span class="sxs-lookup"><span data-stu-id="1b078-281">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="1b078-282">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítésre](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span><span class="sxs-lookup"><span data-stu-id="1b078-282">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication)</span></span>|  
  
<span data-ttu-id="1b078-283">a gazdagép hello tulajdonságok a következők:</span><span class="sxs-lookup"><span data-stu-id="1b078-283">hello properties for host are:</span></span>  
  
|<span data-ttu-id="1b078-284">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-284">Element name</span></span>|<span data-ttu-id="1b078-285">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-285">Required</span></span>|<span data-ttu-id="1b078-286">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-286">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="1b078-287">API runtimeUrl</span><span class="sxs-lookup"><span data-stu-id="1b078-287">api runtimeUrl</span></span>|<span data-ttu-id="1b078-288">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-288">Yes</span></span>|<span data-ttu-id="1b078-289">hello hello végpontja felügyelt API-t.</span><span class="sxs-lookup"><span data-stu-id="1b078-289">hello endpoint of hello managed API.</span></span>|  
|<span data-ttu-id="1b078-290">Kapcsolat neve</span><span class="sxs-lookup"><span data-stu-id="1b078-290">connection name</span></span>||<span data-ttu-id="1b078-291">Meg kell a hivatkozás tooa paraméter hívni `$connection` és hello felügyelt API-kapcsolat, amely a munkafolyamat által használt hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="1b078-291">Must be a reference tooa parameter called `$connection` and is hello name of hello managed API connection that hello workflow uses.</span></span>|
  
<span data-ttu-id="1b078-292">az API-kapcsolat eseményindító hello kimenetének a következők:</span><span class="sxs-lookup"><span data-stu-id="1b078-292">hello outputs of an API connection trigger are:</span></span>
  
|<span data-ttu-id="1b078-293">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-293">Element name</span></span>|<span data-ttu-id="1b078-294">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-294">Type</span></span>|<span data-ttu-id="1b078-295">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-295">Description</span></span>|  
|----------------|--------|---------------|  
|<span data-ttu-id="1b078-296">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-296">headers</span></span>|<span data-ttu-id="1b078-297">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-297">Object</span></span>|<span data-ttu-id="1b078-298">hello fejlécek hello http-válaszok.</span><span class="sxs-lookup"><span data-stu-id="1b078-298">hello headers of hello http response.</span></span>|  
|<span data-ttu-id="1b078-299">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-299">body</span></span>|<span data-ttu-id="1b078-300">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-300">Object</span></span>|<span data-ttu-id="1b078-301">hello hello HTTP-válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="1b078-301">hello body of hello http response.</span></span>|  
  
## <a name="httpwebhook-trigger"></a><span data-ttu-id="1b078-302">HTTPWebhook eseményindító</span><span class="sxs-lookup"><span data-stu-id="1b078-302">HTTPWebhook trigger</span></span>  

<span data-ttu-id="1b078-303">hello HTTPWebhook eseményindító megnyit egy végpontot, a hasonló toohello kézi indítási, de a hello HTTPWebhook eseményindító is meghívja a kimenő tooa megadott URL-cím tooregister és regisztrációjának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="1b078-303">hello HTTPWebhook trigger opens an endpoint, similar toohello manual trigger, but hello HTTPWebhook trigger also calls out tooa specified URL tooregister and unregister.</span></span> <span data-ttu-id="1b078-304">Íme egy példa hogyan nézhet ki egy HTTPWebhook eseményindító:</span><span class="sxs-lookup"><span data-stu-id="1b078-304">Here's an example of what an HTTPWebhook trigger might look like:</span></span>  

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

<span data-ttu-id="1b078-305">Ezek a szakaszok számos nem kötelező, és hello Webhook hello viselkedését függ, mely szakaszok megadva, vagy nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="1b078-305">Many of these sections are optional, and hello behavior of hello Webhook depends on which sections are provided or omitted.</span></span>  
<span data-ttu-id="1b078-306">a Webhook tulajdonságainak hello a következők:</span><span class="sxs-lookup"><span data-stu-id="1b078-306">hello properties of a Webhook are as follows:</span></span>  
  
|<span data-ttu-id="1b078-307">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-307">Element name</span></span>|<span data-ttu-id="1b078-308">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-308">Required</span></span>|<span data-ttu-id="1b078-309">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-309">Description</span></span>|  
|----------------|------------|---------------|  
|<span data-ttu-id="1b078-310">előfizetés</span><span class="sxs-lookup"><span data-stu-id="1b078-310">subscribe</span></span>|<span data-ttu-id="1b078-311">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-311">No</span></span>|<span data-ttu-id="1b078-312">a kimenő kérelem is nevezett hello eseményindító jön létre, és elvégzi a kezdeti regisztrációs hello hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-312">hello outgoing request that is called when hello trigger is created and performs hello initial registration.</span></span>|  
|<span data-ttu-id="1b078-313">előfizetés lemondása</span><span class="sxs-lookup"><span data-stu-id="1b078-313">unsubscribe</span></span>|<span data-ttu-id="1b078-314">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-314">No</span></span>|<span data-ttu-id="1b078-315">hello kimenő kérelem hello eseményindító törlésekor.</span><span class="sxs-lookup"><span data-stu-id="1b078-315">hello outgoing request when hello trigger is deleted.</span></span>|  
  
-   <span data-ttu-id="1b078-316">**Előfizetés** hello kimenő hívás toostart figyelő tooevents jogosultságokat.</span><span class="sxs-lookup"><span data-stu-id="1b078-316">**Subscribe** is hello outgoing call that's made toostart listening tooevents.</span></span> <span data-ttu-id="1b078-317">Ez a hívás ugyanazokat a paramétereket, normál HTTP-műveletek hello tegye hello kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1b078-317">This call starts with hello same set of parameters that hello normal HTTP actions do.</span></span> <span data-ttu-id="1b078-318">A kimenő kezdeményezték bármely idő hello munkafolyamat-módosítások bármely olyan módon, például amikor hello hitelesítő adatok már megkezdődött, vagy hello eseményindító bemeneti paraméterek módosítása.</span><span class="sxs-lookup"><span data-stu-id="1b078-318">This outgoing call is made any time hello workflow changes in any way, for example, whenever hello credentials are rolled, or hello trigger's input parameters change.</span></span>
  
    <span data-ttu-id="1b078-319">toosupport ez hívja, van egy új funkció: `@listCallbackUrl()`.</span><span class="sxs-lookup"><span data-stu-id="1b078-319">toosupport this call, there is a new function: `@listCallbackUrl()`.</span></span> <span data-ttu-id="1b078-320">Ez a függvény egy egyedi URL-címet az adott eseményindító a munkafolyamat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1b078-320">This function returns a unique URL for this specific trigger in this workflow.</span></span> <span data-ttu-id="1b078-321">Azt jelenti, hogy hello szolgáltatás REST használó végpontokon hello hello egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1b078-321">It represents hello unique identifier for hello endpoints that use hello Service REST.</span></span>  
  
-   <span data-ttu-id="1b078-322">**Leiratkozhat** van meghívva, amikor egy művelet Renderelés ehhez az eseményindítóhoz érvénytelen, beleértve:</span><span class="sxs-lookup"><span data-stu-id="1b078-322">**Unsubscribe** is called when an operation renders this trigger invalid, including:</span></span>  
  
    -   <span data-ttu-id="1b078-323">Törlése vagy hello eseményindító letiltása</span><span class="sxs-lookup"><span data-stu-id="1b078-323">Deleting or disabling hello trigger</span></span>  
  
    -   <span data-ttu-id="1b078-324">Törlése vagy hello munkafolyamat letiltása</span><span class="sxs-lookup"><span data-stu-id="1b078-324">Deleting or disabling hello workflow</span></span>  
  
    -   <span data-ttu-id="1b078-325">Törlése vagy hello előfizetés letiltása</span><span class="sxs-lookup"><span data-stu-id="1b078-325">Deleting or disabling hello subscription</span></span>  
  
    <span data-ttu-id="1b078-326">hello logikai alkalmazás automatikusan meghívja a hello leiratkozhat művelet.</span><span class="sxs-lookup"><span data-stu-id="1b078-326">hello logic app automatically calls hello unsubscribe action.</span></span> <span data-ttu-id="1b078-327">hello paraméterek toothis függvény van mint a HTTP-eseményindítóval hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="1b078-327">hello parameters toothis function are hello same as hello HTTP trigger.</span></span>  
  
    <span data-ttu-id="1b078-328">hello hello HTTPWebhook eseményindító kimenetének hello bejövő kérelem hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="1b078-328">hello outputs of hello HTTPWebhook trigger are hello contents of hello incoming request:</span></span>  
  
|<span data-ttu-id="1b078-329">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-329">Element name</span></span>|<span data-ttu-id="1b078-330">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-330">Type</span></span>|<span data-ttu-id="1b078-331">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-331">Description</span></span>|  
|-----------------|--------|---------------|  
|<span data-ttu-id="1b078-332">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-332">headers</span></span>|<span data-ttu-id="1b078-333">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-333">Object</span></span>|<span data-ttu-id="1b078-334">hello fejlécek hello HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="1b078-334">hello headers of hello http request.</span></span>|  
|<span data-ttu-id="1b078-335">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-335">body</span></span>|<span data-ttu-id="1b078-336">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-336">Object</span></span>|<span data-ttu-id="1b078-337">hello hello http-kérelem törzse.</span><span class="sxs-lookup"><span data-stu-id="1b078-337">hello body of hello http request.</span></span>|  

<span data-ttu-id="1b078-338">A webhook művelet vonatkozó korlátozások is megadhatók hello megegyező módon [HTTP aszinkron korlátok](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="1b078-338">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  

## <a name="conditions"></a><span data-ttu-id="1b078-339">Feltételek</span><span class="sxs-lookup"><span data-stu-id="1b078-339">Conditions</span></span>  

<span data-ttu-id="1b078-340">Bármely eseményindító használhatja egy vagy több feltételek toodetermine e hello munkafolyamat futtasson-e vagy sem.</span><span class="sxs-lookup"><span data-stu-id="1b078-340">For any trigger, you can use one or more conditions toodetermine whether hello workflow should run or not.</span></span> <span data-ttu-id="1b078-341">Példa:</span><span class="sxs-lookup"><span data-stu-id="1b078-341">For example:</span></span>  

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

<span data-ttu-id="1b078-342">Ebben az esetben a jelentés csak eseményindítók hello munkafolyamat közben hello `sendReports` paraméter értéke tootrue.</span><span class="sxs-lookup"><span data-stu-id="1b078-342">In this case, hello report only triggers while hello workflow's `sendReports` parameter is set tootrue.</span></span> <span data-ttu-id="1b078-343">Végezetül feltételek hivatkozhatnak hello állapotkód: hello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="1b078-343">Finally, conditions may reference hello status code of hello trigger.</span></span> <span data-ttu-id="1b078-344">Például sikerült indítsa el a munkafolyamat csak akkor, ha a webhely adja vissza egy állapotkód: 500, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1b078-344">For example, you could kick off a workflow only when your website returns a status code 500, as follows:</span></span>
  
```  
"conditions": [  
        {  
          "expression": "@equals(triggers().code, 'InternalServerError')"  
        }  
      ]  
```  
  
> [!NOTE]  
> <span data-ttu-id="1b078-345">Ha bármely kifejezés hivatkozik hello állapotkód: hello eseményindító \(valamilyen módon\), alapértelmezett viselkedést hello \(eseményindító csak a 200-as \(OK\) \) váltja fel.</span><span class="sxs-lookup"><span data-stu-id="1b078-345">When any expression references hello status code of hello trigger \(in any way\), hello default behavior \(trigger only on 200 \(OK\)\) is replaced.</span></span> <span data-ttu-id="1b078-346">Például, ha azt szeretné, hogy a állapotkód 200-as és a 201-es állapotkód tootrigger, hogy tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` a feltételként.</span><span class="sxs-lookup"><span data-stu-id="1b078-346">For example, if you want tootrigger on both status code 200 and status code 201, you have tooinclude: `@or(equals(triggers().code, 200),equals(triggers().code,201))` as your condition.</span></span>  
  
## <a name="start-multiple-runs-for-a-request"></a><span data-ttu-id="1b078-347">Indítsa el a kérelmek több futtatása</span><span class="sxs-lookup"><span data-stu-id="1b078-347">Start multiple runs for a request</span></span>

<span data-ttu-id="1b078-348">egyetlen kérelem több futtatások ki tookick `splitOn` akkor hasznos, például, ha azt szeretné, hogy egy végpontot, amely több új elemek közötti lekérdezési időközök toopoll.</span><span class="sxs-lookup"><span data-stu-id="1b078-348">tookick off multiple runs for a single request, `splitOn` is useful, for example, when you want toopoll an endpoint that can have multiple new items between polling intervals.</span></span>
  
<span data-ttu-id="1b078-349">A `splitOn`, belül, amely tartalmazza az elemet, amelyek mindegyike hello tömbje hello válasz hasznos hello tulajdonság megadása toouse toostart hello eseményindító futását.</span><span class="sxs-lookup"><span data-stu-id="1b078-349">With `splitOn`, you specify hello property inside hello response payload that contains hello array of items, each of which you want toouse toostart a run of hello trigger.</span></span> <span data-ttu-id="1b078-350">Tegyük fel, hogy rendelkezik egy, a válasz a következő hello visszaadó API:</span><span class="sxs-lookup"><span data-stu-id="1b078-350">For example, imagine you have an API that returns hello following response:</span></span>  
  
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
  
<span data-ttu-id="1b078-351">A Logic Apps alkalmazást csak a kell hello sorok tartalom, hogyan hozhat létre például ebben a példában az eseményindító:</span><span class="sxs-lookup"><span data-stu-id="1b078-351">Your logic app only needs hello Rows content, so you can construct your trigger like this example:</span></span>  
  
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
  
<span data-ttu-id="1b078-352">Ezt követően a hello munkafolyamat-definíciót, `@triggerBody().name` adja vissza `mycoolrow` a hello először futtatja, és `another row` a hello második futtató.</span><span class="sxs-lookup"><span data-stu-id="1b078-352">Then, in hello workflow definition, `@triggerBody().name` returns `mycoolrow` for hello first run, and `another row` for hello second run.</span></span> <span data-ttu-id="1b078-353">hello eseményindító kimenetek kinézetét ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="1b078-353">hello trigger outputs look like this example:</span></span>  
  
```json
{
    "body" : {
        "id" : 938109381,
        "name" : "another row"
    }
}
```

<span data-ttu-id="1b078-354">Igen, ha `SplitOn`, nem olvasható be, amelyek túlmutatnak hello tömb, ebben az esetben hello hello tulajdonságok `Status` mező.</span><span class="sxs-lookup"><span data-stu-id="1b078-354">So if you use `SplitOn`, you can't get hello properties that are outside hello array, in this case, hello `Status` field.</span></span>  
  
> [!NOTE]  
> <span data-ttu-id="1b078-355">A jelen példában használjuk hello `?` operátor toobe képes tooavoid egy hiba, ha hello `Rows` tulajdonság nincs jelen.</span><span class="sxs-lookup"><span data-stu-id="1b078-355">In this example, we use hello `?` operator toobe able tooavoid a failure if hello `Rows` property is not present.</span></span> 
  
## <a name="single-run-instance"></a><span data-ttu-id="1b078-356">Egyszeri futtatási példánya</span><span class="sxs-lookup"><span data-stu-id="1b078-356">Single run instance</span></span>

<span data-ttu-id="1b078-357">Konfigurálhatja az eseményindítók rendelkező tooonly tűz ismétlődési tulajdonság, ha az összes aktív futtatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1b078-357">You can configure triggers that have a recurrence property tooonly fire if all active runs have completed.</span></span> <span data-ttu-id="1b078-358">Ütemezett ismétlődése következik be, amíg a folyamatban lévő futtassa, ha a hello eseményindító kihagyja, és megvárja a hello következő ütemezett ismétlődési időköz toocheck újra.</span><span class="sxs-lookup"><span data-stu-id="1b078-358">If a scheduled recurrence occurs while there is an in-progress run, hello trigger skips and waits until hello next scheduled recurrence interval toocheck again.</span></span>

<span data-ttu-id="1b078-359">A beállítás hello művelet lehetőségeit:</span><span class="sxs-lookup"><span data-stu-id="1b078-359">You can configure this setting through hello operation options:</span></span>

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

## <a name="types-and-inputs"></a><span data-ttu-id="1b078-360">Típusok és a bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="1b078-360">Types and inputs</span></span>  

<span data-ttu-id="1b078-361">Nincsenek számos különböző típusú műveletek, az egyedi viselkedését.</span><span class="sxs-lookup"><span data-stu-id="1b078-361">There are many types of actions, each with unique behavior.</span></span> <span data-ttu-id="1b078-362">Gyűjtemény műveletek önmagába sok más műveleteket is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1b078-362">Collection actions may contain many other actions within itself.</span></span>

### <a name="standard-actions"></a><span data-ttu-id="1b078-363">Standard műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-363">Standard actions</span></span>  

-   <span data-ttu-id="1b078-364">**HTTP** Ez a művelet HTTP webes végpont hívja.</span><span class="sxs-lookup"><span data-stu-id="1b078-364">**HTTP** This action calls an HTTP web endpoint.</span></span>  
  
-   <span data-ttu-id="1b078-365">**ApiConnection** \- , de ez a művelet úgy viselkedik, mint a HTTP-művelet hello, használja a Microsoft által felügyelt API-k hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-365">**ApiConnection** \- This action behaves like hello HTTP action, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="1b078-366">**ApiConnectionWebhook** \- például HTTPWebhook, de a Microsoft által felügyelt API-k által használt hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-366">**ApiConnectionWebhook** \- Like HTTPWebhook, but uses hello Microsoft-managed APIs.</span></span>  
  
-   <span data-ttu-id="1b078-367">**Válasz** \- Ez a művelet bejövő válasz meghatározása.</span><span class="sxs-lookup"><span data-stu-id="1b078-367">**Response** \- This action defines a response for an incoming call.</span></span>  
  
-   <span data-ttu-id="1b078-368">**Várjon, amíg** \- egyszerű művelet várakozik a rögzített méretű idő vagy egy adott időpontig.</span><span class="sxs-lookup"><span data-stu-id="1b078-368">**Wait** \- This simple action waits a fixed amount of time or until a specific time.</span></span>  
  
-   <span data-ttu-id="1b078-369">**Munkafolyamat** \- Ez a művelet egy beágyazott munkafolyamat jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-369">**Workflow** \- This action represents a nested workflow.</span></span>  

-   <span data-ttu-id="1b078-370">**Függvény** \- Ez a művelet egy Azure-függvény jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-370">**Function** \- This action represents an Azure Function.</span></span>

### <a name="collection-actions"></a><span data-ttu-id="1b078-371">Gyűjtemény műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-371">Collection actions</span></span>

-   <span data-ttu-id="1b078-372">**Hatókör** \- Ez a művelet akkor más műveletek logikai csoportosítása.</span><span class="sxs-lookup"><span data-stu-id="1b078-372">**Scope** \- This action is a logical grouping of other actions.</span></span>

-   <span data-ttu-id="1b078-373">**Az állapot** \- Ez a művelet egy kifejezés kiértékelése és hello megfelelő eredmény fiókirodai végrehajtja.</span><span class="sxs-lookup"><span data-stu-id="1b078-373">**Condition** \- This action evaluates an expression and executes hello corresponding result branch.</span></span>

-   <span data-ttu-id="1b078-374">**ForEach** \- ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1b078-374">**ForEach** \- This looping action iterates through an array and performs inner actions for each item.</span></span>

-   <span data-ttu-id="1b078-375">**Amíg** \- ismétlési művelet belső műveleteket hajt végre, amíg egy feltétel eredménye tootrue.</span><span class="sxs-lookup"><span data-stu-id="1b078-375">**Until** \- This looping action executes inner actions until a condition results tootrue.</span></span>
  
<span data-ttu-id="1b078-376">Írja be a van más **bemenetek** , amely egy művelet viselkedését határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1b078-376">Each type of action has a different set of **inputs** that define an action's behavior.</span></span>  
  
## <a name="http-action"></a><span data-ttu-id="1b078-377">HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-377">HTTP action</span></span>  

<span data-ttu-id="1b078-378">HTTP-műveletek a megadott végpont hívja, és hello válasz toodetermine ellenőrizze, hogy hello munkafolyamat futtatásának.</span><span class="sxs-lookup"><span data-stu-id="1b078-378">HTTP actions call a specified endpoint and check hello response toodetermine whether hello workflow should run.</span></span> <span data-ttu-id="1b078-379">Hello **bemenetek** objektum veszi hello set paraméterek szükséges tooconstruct hello HTTP-hívás:</span><span class="sxs-lookup"><span data-stu-id="1b078-379">hello **inputs** object takes hello set of parameters required tooconstruct hello HTTP call:</span></span>  
  
|<span data-ttu-id="1b078-380">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-380">Element name</span></span>|<span data-ttu-id="1b078-381">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-381">Required</span></span>|<span data-ttu-id="1b078-382">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-382">Type</span></span>|<span data-ttu-id="1b078-383">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-383">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="1b078-384">Módszer</span><span class="sxs-lookup"><span data-stu-id="1b078-384">method</span></span>|<span data-ttu-id="1b078-385">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-385">Yes</span></span>|<span data-ttu-id="1b078-386">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-386">String</span></span>|<span data-ttu-id="1b078-387">A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="1b078-387">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="1b078-388">URI</span><span class="sxs-lookup"><span data-stu-id="1b078-388">uri</span></span>|<span data-ttu-id="1b078-389">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-389">Yes</span></span>|<span data-ttu-id="1b078-390">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-390">String</span></span>|<span data-ttu-id="1b078-391">hello http vagy https-végpont is nevezett.</span><span class="sxs-lookup"><span data-stu-id="1b078-391">hello http or https endpoint that is called.</span></span> <span data-ttu-id="1b078-392">Hossza legfeljebb 2 KB.</span><span class="sxs-lookup"><span data-stu-id="1b078-392">Maximum length is 2 kilobytes.</span></span>|  
|<span data-ttu-id="1b078-393">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-393">queries</span></span>|<span data-ttu-id="1b078-394">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-394">No</span></span>|<span data-ttu-id="1b078-395">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-395">Object</span></span>|<span data-ttu-id="1b078-396">Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-396">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="1b078-397">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-397">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="1b078-398">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-398">headers</span></span>|<span data-ttu-id="1b078-399">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-399">No</span></span>|<span data-ttu-id="1b078-400">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-400">Object</span></span>|<span data-ttu-id="1b078-401">Egyes toohello kérelmet küldött hello fejlécek jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-401">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-402">Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="1b078-402">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="1b078-403">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-403">body</span></span>|<span data-ttu-id="1b078-404">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-404">No</span></span>|<span data-ttu-id="1b078-405">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-405">Object</span></span>|<span data-ttu-id="1b078-406">Hello hasznos toohello végpont küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-406">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="1b078-407">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="1b078-407">retryPolicy</span></span>|<span data-ttu-id="1b078-408">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-408">No</span></span>|<span data-ttu-id="1b078-409">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-409">Object</span></span>|<span data-ttu-id="1b078-410">Lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.</span><span class="sxs-lookup"><span data-stu-id="1b078-410">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="1b078-411">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="1b078-411">operationsOptions</span></span>|<span data-ttu-id="1b078-412">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-412">No</span></span>|<span data-ttu-id="1b078-413">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-413">String</span></span>|<span data-ttu-id="1b078-414">Különleges viselkedések toooverride hello csoportját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1b078-414">Defines hello set of special behaviors toooverride.</span></span>|  
|<span data-ttu-id="1b078-415">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1b078-415">authentication</span></span>|<span data-ttu-id="1b078-416">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-416">No</span></span>|<span data-ttu-id="1b078-417">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-417">Object</span></span>|<span data-ttu-id="1b078-418">Jelöli hello módszer, amelynek a hello kérelem hitelesítése történik.</span><span class="sxs-lookup"><span data-stu-id="1b078-418">Represents hello method that hello request should be authenticated.</span></span> <span data-ttu-id="1b078-419">Ezen az objektumon, lásd: [Feladatütemező kimenő hitelesítési](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span><span class="sxs-lookup"><span data-stu-id="1b078-419">For details on this object, see [Scheduler Outbound Authentication](https://docs.microsoft.com/azure/scheduler/scheduler-outbound-authentication).</span></span> <span data-ttu-id="1b078-420">Ütemező túl van egy több támogatott tulajdonságot: `authority`.</span><span class="sxs-lookup"><span data-stu-id="1b078-420">Beyond scheduler, there is one more supported property: `authority`.</span></span> <span data-ttu-id="1b078-421">Alapértelmezés szerint ez a `https://login.windows.net` Ha nincs megadva, de használhat például egy másik célközönség`https://login.windows\-ppe.net`</span><span class="sxs-lookup"><span data-stu-id="1b078-421">By default, this is `https://login.windows.net` when not specified, but you can use a different audience like `https://login.windows\-ppe.net`</span></span>|  
  
<span data-ttu-id="1b078-422">HTTP-műveletek \(és API-kapcsolat\) műveletek támogatási házirendek próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="1b078-422">HTTP actions \(and API Connection\) actions support retry policies.</span></span> <span data-ttu-id="1b078-423">Újrapróbálkozási házirendje érvényes toointermittent hibák, mint a HTTP-állapot jellemzőek 408 429 és 5xx, továbbá tooany csatlakozási kivétel a kódok.</span><span class="sxs-lookup"><span data-stu-id="1b078-423">A retry policy applies toointermittent failures, characterized as HTTP status codes 408, 429, and 5xx, in addition tooany connectivity exceptions.</span></span> <span data-ttu-id="1b078-424">Ez a házirend leírását hello segítségével *retryPolicy* objektum megadva, az itt látható:</span><span class="sxs-lookup"><span data-stu-id="1b078-424">This policy is described using hello *retryPolicy* object defined as shown here:</span></span>
  
```json
"retryPolicy" : {
    "type": "<type-of-retry-policy>",
    "interval": <retry-interval>,
    "count": <number-of-retry-attempts>
}
```
  
<span data-ttu-id="1b078-425">hello újrapróbálkozási időköz hello ISO 8601 formátumban van megadva.</span><span class="sxs-lookup"><span data-stu-id="1b078-425">hello retry interval is specified in hello ISO 8601 format.</span></span> <span data-ttu-id="1b078-426">Ez az időtartam alatt alapértelmezett és 20 másodperc, minimális érték rendelkezik hello maximális értéke pedig egy óra.</span><span class="sxs-lookup"><span data-stu-id="1b078-426">This interval has a default and minimum value of 20 seconds, while hello maximum value is one hour.</span></span> <span data-ttu-id="1b078-427">hello alapértelmezett és a maximális újrapróbálkozások száma négy óra.</span><span class="sxs-lookup"><span data-stu-id="1b078-427">hello default and maximum retry count is four hours.</span></span> <span data-ttu-id="1b078-428">Ha nincs megadva hello újrapróbálkozási házirend-definíció, egy `fixed` stratégia használatos az alapértelmezett értékekkel újrapróbálkozási számát és az időközt.</span><span class="sxs-lookup"><span data-stu-id="1b078-428">If hello retry policy definition is not specified, a `fixed` strategy is used with default retry count and interval values.</span></span> <span data-ttu-id="1b078-429">toodisable hello újrapróbálkozási házirendje, állítsa be a típusa túl`None`.</span><span class="sxs-lookup"><span data-stu-id="1b078-429">toodisable hello retry policy, set its type too`None`.</span></span>  
  
<span data-ttu-id="1b078-430">Például hello következő művelet újrapróbálja lekérdezésekor hello legfrissebb hírek kétszer, ha nincsenek időszakos hibák, három végrehajtások, 30 másodperces késleltetéssel minden kísérlet között összesen:</span><span class="sxs-lookup"><span data-stu-id="1b078-430">For example, hello following action retries fetching hello latest news two times, if there are intermittent failures, for a total of three executions, with a 30-second delay between each attempt:</span></span>  
  
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
### <a name="asynchronous-patterns"></a><span data-ttu-id="1b078-431">Aszinkron minták</span><span class="sxs-lookup"><span data-stu-id="1b078-431">Asynchronous patterns</span></span>

<span data-ttu-id="1b078-432">Alapértelmezés szerint az összes HTTP-alapú műveletek támogatja hello szabványos aszinkron művelet szabályt.</span><span class="sxs-lookup"><span data-stu-id="1b078-432">By default, all HTTP-based actions support hello standard asynchronous operation pattern.</span></span> <span data-ttu-id="1b078-433">Így ha hello távoli kiszolgáló azt jelzi, hogy hello kérelem elfogadható egy 202 feldolgozásra \(elfogadott\) választ, a hello Logic Apps motor tartja a lekérdezés egy terminált eléréséig hello válasz hely fejlécben megadott hello URL-cím állapot \(nem\-202 válasz\).</span><span class="sxs-lookup"><span data-stu-id="1b078-433">So if hello remote server indicates that hello request is accepted for processing with a 202 \(Accepted\) response, hello Logic Apps engine keeps polling hello URL specified in hello response's location header until reaching a terminal state \(a non\-202 response\).</span></span>  
  
<span data-ttu-id="1b078-434">toodisable hello aszinkron viselkedés korábban leírt, állítsa be a `DisableAsyncPattern` hello művelet bemeneti lehetősége.</span><span class="sxs-lookup"><span data-stu-id="1b078-434">toodisable hello asynchronous behavior previously described, set a `DisableAsyncPattern` option in hello action inputs.</span></span> <span data-ttu-id="1b078-435">Ebben az esetben hello kimeneti hello művelet hello kezdeti 202 hello kiszolgáló válasza alapján történik.</span><span class="sxs-lookup"><span data-stu-id="1b078-435">In this case, hello output of hello action is based on hello initial 202 response from hello server.</span></span>  
  
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

#### <a name="asynchronous-limits"></a><span data-ttu-id="1b078-436">Aszinkron korlátok</span><span class="sxs-lookup"><span data-stu-id="1b078-436">Asynchronous Limits</span></span>

<span data-ttu-id="1b078-437">Egy aszinkron mintát lehet korlátozni a duration tooa adott időintervallumban.</span><span class="sxs-lookup"><span data-stu-id="1b078-437">An asynchronous pattern can be limited in its duration tooa specific time interval.</span></span>  <span data-ttu-id="1b078-438">Ha hello alatt az időtartam alatt nem érte el a Terminálszolgáltatások állapot eltelte után hello művelet hello állapota lesz megjelölve `Cancelled` egy kóddal `ActionTimedOut`.</span><span class="sxs-lookup"><span data-stu-id="1b078-438">If hello time interval elapses without reaching a terminal state, hello status of hello action will be marked `Cancelled` with a code of `ActionTimedOut`.</span></span>  <span data-ttu-id="1b078-439">ISO 8601 formátumban megadott hello korlát időkorlát.</span><span class="sxs-lookup"><span data-stu-id="1b078-439">hello limit timeout is specified in ISO 8601 format.</span></span>  <span data-ttu-id="1b078-440">Korlátozások a szintaxis a következő hello adható meg:</span><span class="sxs-lookup"><span data-stu-id="1b078-440">Limits can be specified with hello following syntax:</span></span>

``` json
"<action-name>": {
    "type": "workflow|webhook|http|apiconnectionwebhook|apiconnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="api-connection"></a><span data-ttu-id="1b078-441">API-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="1b078-441">API Connection</span></span>  

<span data-ttu-id="1b078-442">API-kapcsolat a Microsoft által felügyelt összekötők hivatkozó művelet.</span><span class="sxs-lookup"><span data-stu-id="1b078-442">API Connection is an action that references a Microsoft-managed connector.</span></span>
<span data-ttu-id="1b078-443">Ez a művelet hivatkozás tooa érvényes kapcsolat, és a hello API és a szükséges paraméterek szükséges.</span><span class="sxs-lookup"><span data-stu-id="1b078-443">This action requires a reference tooa valid connection, and information on hello API and parameters required.</span></span>

|<span data-ttu-id="1b078-444">Elem neve</span><span class="sxs-lookup"><span data-stu-id="1b078-444">Element name</span></span>|<span data-ttu-id="1b078-445">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-445">Required</span></span>|<span data-ttu-id="1b078-446">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-446">Type</span></span>|<span data-ttu-id="1b078-447">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-447">Description</span></span>|  
|----------------|------------|--------|---------------|  
|<span data-ttu-id="1b078-448">állomás</span><span class="sxs-lookup"><span data-stu-id="1b078-448">host</span></span>|<span data-ttu-id="1b078-449">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-449">Yes</span></span>|<span data-ttu-id="1b078-450">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-450">Object</span></span>|<span data-ttu-id="1b078-451">Hello összekötővel kapcsolatos adatokat, például a hello runtimeUrl és a hivatkozás toohello kapcsolatobjektumot jelenti.</span><span class="sxs-lookup"><span data-stu-id="1b078-451">Represents hello connector information such as hello runtimeUrl and reference toohello connection object</span></span>|
|<span data-ttu-id="1b078-452">Módszer</span><span class="sxs-lookup"><span data-stu-id="1b078-452">method</span></span>|<span data-ttu-id="1b078-453">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-453">Yes</span></span>|<span data-ttu-id="1b078-454">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-454">String</span></span>|<span data-ttu-id="1b078-455">A következő HTTP-metódus hello egyike lehet: **beolvasása**, **POST**, **PUT**, **törlése**, **javítás**, vagy  **HEAD**</span><span class="sxs-lookup"><span data-stu-id="1b078-455">Can be one of hello following HTTP methods: **GET**, **POST**, **PUT**, **DELETE**, **PATCH**, or **HEAD**</span></span>|  
|<span data-ttu-id="1b078-456">Elérési út</span><span class="sxs-lookup"><span data-stu-id="1b078-456">path</span></span>|<span data-ttu-id="1b078-457">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-457">Yes</span></span>|<span data-ttu-id="1b078-458">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-458">String</span></span>|<span data-ttu-id="1b078-459">hello API művelet hello elérési útját.</span><span class="sxs-lookup"><span data-stu-id="1b078-459">hello path of hello API operation.</span></span>|  
|<span data-ttu-id="1b078-460">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-460">queries</span></span>|<span data-ttu-id="1b078-461">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-461">No</span></span>|<span data-ttu-id="1b078-462">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-462">Object</span></span>|<span data-ttu-id="1b078-463">Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-463">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="1b078-464">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-464">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="1b078-465">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-465">headers</span></span>|<span data-ttu-id="1b078-466">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-466">No</span></span>|<span data-ttu-id="1b078-467">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-467">Object</span></span>|<span data-ttu-id="1b078-468">Egyes toohello kérelmet küldött hello fejlécek jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-468">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-469">Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="1b078-469">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="1b078-470">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-470">body</span></span>|<span data-ttu-id="1b078-471">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-471">No</span></span>|<span data-ttu-id="1b078-472">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-472">Object</span></span>|<span data-ttu-id="1b078-473">Hello hasznos toohello végpont küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-473">Represents hello payload that is sent toohello endpoint.</span></span>|  
|<span data-ttu-id="1b078-474">retryPolicy</span><span class="sxs-lookup"><span data-stu-id="1b078-474">retryPolicy</span></span>|<span data-ttu-id="1b078-475">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-475">No</span></span>|<span data-ttu-id="1b078-476">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-476">Object</span></span>|<span data-ttu-id="1b078-477">Lehetővé teszi testre szabhatja a hello újrapróbálási viselkedése 4xx vagy 5xx hiba.</span><span class="sxs-lookup"><span data-stu-id="1b078-477">Lets you customize hello retry behavior for 4xx or 5xx errors.</span></span>|  
|<span data-ttu-id="1b078-478">operationsOptions</span><span class="sxs-lookup"><span data-stu-id="1b078-478">operationsOptions</span></span>|<span data-ttu-id="1b078-479">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-479">No</span></span>|<span data-ttu-id="1b078-480">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-480">String</span></span>|<span data-ttu-id="1b078-481">Különleges viselkedések toooverride hello csoportját határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1b078-481">Defines hello set of special behaviors toooverride.</span></span>|  

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

## <a name="api-connection-webhook-action"></a><span data-ttu-id="1b078-482">API-kapcsolat webhook művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-482">API Connection webhook action</span></span>

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

<span data-ttu-id="1b078-483">A webhook művelet vonatkozó korlátozások is megadhatók hello megegyező módon [HTTP aszinkron korlátok](#asynchronous-limits).</span><span class="sxs-lookup"><span data-stu-id="1b078-483">Limits on a webhook action can be specified in hello same manner as [HTTP Asynchronous Limits](#asynchronous-limits).</span></span>
  
## <a name="response-action"></a><span data-ttu-id="1b078-484">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-484">Response action</span></span>  

<span data-ttu-id="1b078-485">A művelet típusa hello teljes válasz hasznos a HTTP-kérelem tartalmazza, és egy statusCode, a törzs és a fejlécek tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1b078-485">This action type contains hello entire response payload from an HTTP request and includes a statusCode, body, and headers:</span></span>  
  
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
  
<span data-ttu-id="1b078-486">hello válasz műveletnek speciális tooother műveletek nem vonatkozó korlátozások.</span><span class="sxs-lookup"><span data-stu-id="1b078-486">hello response action has special restrictions that don't apply tooother actions.</span></span> <span data-ttu-id="1b078-487">Konkrétan:</span><span class="sxs-lookup"><span data-stu-id="1b078-487">Specifically:</span></span>  
  
-   <span data-ttu-id="1b078-488">Sérülésével kapcsolatos válaszműveletek nem lehet a definícióban párhuzamos, mert a mérvadó válasz toohello bejövő kérelmet.</span><span class="sxs-lookup"><span data-stu-id="1b078-488">Response actions cannot be parallel in a definition because a deterministic response toohello incoming request is required.</span></span>  
  
-   <span data-ttu-id="1b078-489">Ha egy válasz művelet elérése után hello bejövő kérelmet kapott választ hello művelet nem sikerült tekinthető \(ütközés\), és emiatt futtatása hello `Failed`.</span><span class="sxs-lookup"><span data-stu-id="1b078-489">If a response action is reached after hello incoming request has received a response, hello action is considered failed \(conflict\), and as a result, hello run is `Failed`.</span></span>  
  
-   <span data-ttu-id="1b078-490">A válaszműveleteket egy munkafolyamatban nem lehet `splitOn` a eseményindítójára, mert egy hívás hatására hány futtatása.</span><span class="sxs-lookup"><span data-stu-id="1b078-490">A workflow with Response actions cannot have `splitOn` in its trigger because one call causes many runs.</span></span> <span data-ttu-id="1b078-491">Ennek köszönhetően ez érvényesítendő hello folyamata PUT és ok egy hibás kérés esetén.</span><span class="sxs-lookup"><span data-stu-id="1b078-491">As a result, this should be validated when hello flow is PUT and cause a Bad Request.</span></span>  
  
## <a name="wait-action"></a><span data-ttu-id="1b078-492">Várjon, amíg a művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-492">Wait action</span></span>  

<span data-ttu-id="1b078-493">Hello `wait` művelet felfüggeszti a munkafolyamat-végrehajtási hello megadott időtartam alatt.</span><span class="sxs-lookup"><span data-stu-id="1b078-493">hello `wait` action suspends workflow execution for hello specified interval.</span></span> <span data-ttu-id="1b078-494">Például a toowait 15 perc, a részlet is használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b078-494">For example, toowait 15 minutes, you can use this snippet:</span></span>  
  
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
  
<span data-ttu-id="1b078-495">Másik lehetőségként toowait időben adott néhány percet, amíg ez a példa használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b078-495">Alternatively, toowait until a specific moment in time, you can use this example:</span></span>  
  
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
> <span data-ttu-id="1b078-496">hello várakozási időtartama vagy szövegtípusához hello **időköz** objektum vagy hello **amíg** objektum, de soha sem egyszerre mindkettőre.</span><span class="sxs-lookup"><span data-stu-id="1b078-496">hello wait duration can be either specified using hello **interval** object or hello **until** object, but not both.</span></span>  
  
|<span data-ttu-id="1b078-497">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-497">Name</span></span>|<span data-ttu-id="1b078-498">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-498">Required</span></span>|<span data-ttu-id="1b078-499">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-499">Type</span></span>|<span data-ttu-id="1b078-500">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-500">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-501">interval</span><span class="sxs-lookup"><span data-stu-id="1b078-501">interval</span></span>|<span data-ttu-id="1b078-502">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-502">No</span></span>|<span data-ttu-id="1b078-503">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-503">Object</span></span>|<span data-ttu-id="1b078-504">hello várakozás időtartama idő alapján.</span><span class="sxs-lookup"><span data-stu-id="1b078-504">hello wait duration based on amount of time.</span></span>|  
|<span data-ttu-id="1b078-505">időköze</span><span class="sxs-lookup"><span data-stu-id="1b078-505">interval unit</span></span>|<span data-ttu-id="1b078-506">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-506">Yes</span></span>|<span data-ttu-id="1b078-507">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-507">String</span></span>|<span data-ttu-id="1b078-508">Egy intervallumok: másodperc, perc, óra, nap, hét, hónap, év.</span><span class="sxs-lookup"><span data-stu-id="1b078-508">One of these intervals: second, minute, hour, day, week, month, year.</span></span>|  
|<span data-ttu-id="1b078-509">időköz száma</span><span class="sxs-lookup"><span data-stu-id="1b078-509">interval count</span></span>|<span data-ttu-id="1b078-510">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-510">Yes</span></span>|<span data-ttu-id="1b078-511">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-511">String</span></span>|<span data-ttu-id="1b078-512">A megadott belső egységgel hello alapján időtartama.</span><span class="sxs-lookup"><span data-stu-id="1b078-512">Duration based on hello given internal unit.</span></span>|  
|<span data-ttu-id="1b078-513">amíg</span><span class="sxs-lookup"><span data-stu-id="1b078-513">until</span></span>|<span data-ttu-id="1b078-514">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-514">No</span></span>|<span data-ttu-id="1b078-515">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-515">Object</span></span>|<span data-ttu-id="1b078-516">hello várakozás időtartama időben a pontok alapján.</span><span class="sxs-lookup"><span data-stu-id="1b078-516">hello wait duration based on a point in time.</span></span>|  
|<span data-ttu-id="1b078-517">amíg időbélyeg</span><span class="sxs-lookup"><span data-stu-id="1b078-517">until timestamp</span></span>|<span data-ttu-id="1b078-518">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-518">Yes</span></span>|<span data-ttu-id="1b078-519">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-519">String</span></span>|<span data-ttu-id="1b078-520">Karakterlánc &#124; időpontja (UTC) hello várakozási lejártakor hello pontot.</span><span class="sxs-lookup"><span data-stu-id="1b078-520">String&#124;hello point in time in UTC when hello wait expires.</span></span>|  

## <a name="query-action"></a><span data-ttu-id="1b078-521">Lekérdezési művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-521">Query action</span></span>

<span data-ttu-id="1b078-522">Hello `query` művelet lehetővé teszi, hogy egy tömb feltétel alapján szűrni.</span><span class="sxs-lookup"><span data-stu-id="1b078-522">hello `query` action lets you filter an array based on a condition.</span></span> <span data-ttu-id="1b078-523">2-nél nagyobb tooselect számokat, például a következő szempontokat is használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b078-523">For example, tooselect numbers greater than 2, you can use:</span></span>

```json
"FilterNumbers" : {
    "type": "query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

<span data-ttu-id="1b078-524">hello kimenete hello `query` művelete olyan tömb, amely rendelkezik, amelyek megfelelnek a hello feltétel hello bemeneti tömb elemei.</span><span class="sxs-lookup"><span data-stu-id="1b078-524">hello output from hello `query` action is an array that has elements from hello input array that satisfy hello condition.</span></span>

> [!NOTE]
> <span data-ttu-id="1b078-525">Ha nincs érték kielégítéséhez hello `where` feltétel, hello eredménye üres tömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-525">If no values satisfy hello `where` condition, hello result is an empty array.</span></span>

|<span data-ttu-id="1b078-526">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-526">Name</span></span>|<span data-ttu-id="1b078-527">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-527">Required</span></span>|<span data-ttu-id="1b078-528">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-528">Type</span></span>|<span data-ttu-id="1b078-529">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-529">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="1b078-530">A</span><span class="sxs-lookup"><span data-stu-id="1b078-530">from</span></span>|<span data-ttu-id="1b078-531">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-531">Yes</span></span>|<span data-ttu-id="1b078-532">Tömb</span><span class="sxs-lookup"><span data-stu-id="1b078-532">Array</span></span>|<span data-ttu-id="1b078-533">hello forrástömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-533">hello source array.</span></span>|
|<span data-ttu-id="1b078-534">Ha</span><span class="sxs-lookup"><span data-stu-id="1b078-534">where</span></span>|<span data-ttu-id="1b078-535">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-535">Yes</span></span>|<span data-ttu-id="1b078-536">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-536">String</span></span>|<span data-ttu-id="1b078-537">hello feltétel tooapply tooeach eleme hello forrástömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-537">hello condition tooapply tooeach element of hello source array.</span></span>|

## <a name="select-action"></a><span data-ttu-id="1b078-538">Kijelölési művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-538">Select action</span></span>

<span data-ttu-id="1b078-539">Hello `select` művelet lehetővé teszi, hogy a tömb egyes elemei projekt be új értéket.</span><span class="sxs-lookup"><span data-stu-id="1b078-539">hello `select` action lets you project each element of an array into a new value.</span></span>
<span data-ttu-id="1b078-540">Például egy tömb a azokat egy tömb számú objektum, tooconvert használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b078-540">For example, tooconvert an array of numbers into an array of objects, you can use:</span></span>

```json
"SelectNumbers" : {
    "type": "select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

<span data-ttu-id="1b078-541">hello kimenete hello `select` művelete olyan tömb, amely rendelkezik hello azonos számossága, a bemeneti tömb egyes elemei át legyenek-e a hello definiált hello `select` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1b078-541">hello output of hello `select` action is an array that has hello same cardinality as hello input array, with each element transformed as defined by hello `select` property.</span></span> <span data-ttu-id="1b078-542">Hello bemenet üres tömb, hello kimeneti akkor is üres tömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-542">If hello input is an empty array, hello output is also an empty array.</span></span>

|<span data-ttu-id="1b078-543">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-543">Name</span></span>|<span data-ttu-id="1b078-544">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-544">Required</span></span>|<span data-ttu-id="1b078-545">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-545">Type</span></span>|<span data-ttu-id="1b078-546">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-546">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="1b078-547">A</span><span class="sxs-lookup"><span data-stu-id="1b078-547">from</span></span>|<span data-ttu-id="1b078-548">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-548">Yes</span></span>|<span data-ttu-id="1b078-549">Tömb</span><span class="sxs-lookup"><span data-stu-id="1b078-549">Array</span></span>|<span data-ttu-id="1b078-550">hello forrástömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-550">hello source array.</span></span>|
|<span data-ttu-id="1b078-551">Válassza ki</span><span class="sxs-lookup"><span data-stu-id="1b078-551">select</span></span>|<span data-ttu-id="1b078-552">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-552">Yes</span></span>|<span data-ttu-id="1b078-553">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="1b078-553">Any</span></span>|<span data-ttu-id="1b078-554">hello leképezése tooapply tooeach eleme hello forrástömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-554">hello projection tooapply tooeach element of hello source array.</span></span>|

## <a name="terminate-action"></a><span data-ttu-id="1b078-555">Leállítási művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-555">Terminate action</span></span>

<span data-ttu-id="1b078-556">hello megszakítási művelet leáll hello a munkafolyamat végrehajtásának futtatja, az üzenetsoroktól műveletek megszakad, és semmilyen további műveletet kihagyja.</span><span class="sxs-lookup"><span data-stu-id="1b078-556">hello Terminate action stops execution of hello workflow run, aborting any in-flight actions, and skipping any remaining actions.</span></span> <span data-ttu-id="1b078-557">Például tooterminate állapotú futtató **sikertelen**, használhatja a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="1b078-557">For example, tooterminate a run with status **Failed**, you can use hello following snippet:</span></span>

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
> <span data-ttu-id="1b078-558">Már elvégzett műveletek nem érinti a hello leállítási művelet.</span><span class="sxs-lookup"><span data-stu-id="1b078-558">Actions already completed are not affected by hello terminate action.</span></span>

|<span data-ttu-id="1b078-559">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-559">Name</span></span>|<span data-ttu-id="1b078-560">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-560">Required</span></span>|<span data-ttu-id="1b078-561">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-561">Type</span></span>|<span data-ttu-id="1b078-562">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-562">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="1b078-563">runStatus</span><span class="sxs-lookup"><span data-stu-id="1b078-563">runStatus</span></span>|<span data-ttu-id="1b078-564">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-564">Yes</span></span>|<span data-ttu-id="1b078-565">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-565">String</span></span>|<span data-ttu-id="1b078-566">hello cél futtatási állapot.</span><span class="sxs-lookup"><span data-stu-id="1b078-566">hello target run status.</span></span> <span data-ttu-id="1b078-567">Vagy **sikertelen** vagy **megszakítva**.</span><span class="sxs-lookup"><span data-stu-id="1b078-567">Either **Failed** or **Cancelled**.</span></span>|
|<span data-ttu-id="1b078-568">runError</span><span class="sxs-lookup"><span data-stu-id="1b078-568">runError</span></span>|<span data-ttu-id="1b078-569">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-569">No</span></span>|<span data-ttu-id="1b078-570">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-570">Object</span></span>|<span data-ttu-id="1b078-571">hello hiba részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="1b078-571">hello error details.</span></span> <span data-ttu-id="1b078-572">Csak támogatott, ha **runStatus** értéke túl**sikertelen**.</span><span class="sxs-lookup"><span data-stu-id="1b078-572">Only supported when **runStatus** is set too**Failed**.</span></span>|
|<span data-ttu-id="1b078-573">runError kód</span><span class="sxs-lookup"><span data-stu-id="1b078-573">runError code</span></span>|<span data-ttu-id="1b078-574">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-574">No</span></span>|<span data-ttu-id="1b078-575">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-575">String</span></span>|<span data-ttu-id="1b078-576">Hiba a kódra hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-576">hello run error code.</span></span>|
|<span data-ttu-id="1b078-577">runError üzenet</span><span class="sxs-lookup"><span data-stu-id="1b078-577">runError message</span></span>|<span data-ttu-id="1b078-578">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-578">No</span></span>|<span data-ttu-id="1b078-579">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-579">String</span></span>|<span data-ttu-id="1b078-580">Futtassa a hibaüzenet a következő hello.</span><span class="sxs-lookup"><span data-stu-id="1b078-580">hello run error message.</span></span>|

## <a name="compose-action"></a><span data-ttu-id="1b078-581">A művelet összeállítása</span><span class="sxs-lookup"><span data-stu-id="1b078-581">Compose action</span></span>

<span data-ttu-id="1b078-582">hello Compose művelet lehetővé teszi, hogy egy tetszőleges objektum hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="1b078-582">hello Compose action lets you construct an arbitrary object.</span></span> <span data-ttu-id="1b078-583">hello hello kimenete írása műveleti bemenet kiértékelése hello eredménye.</span><span class="sxs-lookup"><span data-stu-id="1b078-583">hello output of hello compose action is hello result of evaluating its inputs.</span></span> <span data-ttu-id="1b078-584">Például használhatja hello művelet toomerge kimenetének több művelet állítható össze:</span><span class="sxs-lookup"><span data-stu-id="1b078-584">For example, you can use hello compose action toomerge outputs of multiple actions:</span></span>

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
> <span data-ttu-id="1b078-585">Hello **Compose** művelet lehet használt tooconstruct kimenetet, többek között az objektumok, tömbök és bármely más típusból XML és bináris hasonlóan a logic apps natívan támogatja.</span><span class="sxs-lookup"><span data-stu-id="1b078-585">hello **Compose** action can be used tooconstruct any output, including objects, arrays, and any other type natively supported by logic apps like XML and binary.</span></span>

## <a name="table-action"></a><span data-ttu-id="1b078-586">Tábla művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-586">Table action</span></span>

<span data-ttu-id="1b078-587">Hello `table` lehetővé teszi egy elemeket tömbje tooconvert egy **CSV** vagy **HTML** tábla.</span><span class="sxs-lookup"><span data-stu-id="1b078-587">hello `table` allows you tooconvert an array of items into a **CSV** or **HTML** table.</span></span>

<span data-ttu-id="1b078-588">Tegyük fel, hogy @triggerBodyvan)</span><span class="sxs-lookup"><span data-stu-id="1b078-588">Suppose @triggerBody() is</span></span>

```json
[{
  "id": 0,
  "name": "apples"
},{
  "id": 1, 
  "name": "oranges"
}]
```

<span data-ttu-id="1b078-589">Hello művelet kell definiálni, így</span><span class="sxs-lookup"><span data-stu-id="1b078-589">And let hello action be defined as</span></span>

```json
"ConvertToTable" : {
    "type": "table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

<span data-ttu-id="1b078-590">a fenti hello eredményezne.</span><span class="sxs-lookup"><span data-stu-id="1b078-590">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="1b078-591">id</span><span class="sxs-lookup"><span data-stu-id="1b078-591">id</span></span></th><th><span data-ttu-id="1b078-592">név</span><span class="sxs-lookup"><span data-stu-id="1b078-592">name</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="1b078-593">0</span><span class="sxs-lookup"><span data-stu-id="1b078-593">0</span></span></td><td><span data-ttu-id="1b078-594">almák</span><span class="sxs-lookup"><span data-stu-id="1b078-594">apples</span></span></td></tr><tr><td><span data-ttu-id="1b078-595">1</span><span class="sxs-lookup"><span data-stu-id="1b078-595">1</span></span></td><td><span data-ttu-id="1b078-596">narancs</span><span class="sxs-lookup"><span data-stu-id="1b078-596">oranges</span></span></td></tr></tbody></table><span data-ttu-id="1b078-597">"</span><span class="sxs-lookup"><span data-stu-id="1b078-597">"</span></span>

<span data-ttu-id="1b078-598">Rendelés toocustomize hello tábla explicit módon megadhat hello oszlopok.</span><span class="sxs-lookup"><span data-stu-id="1b078-598">In order toocustomize hello table, you can specify hello columns explicitly.</span></span> <span data-ttu-id="1b078-599">Példa:</span><span class="sxs-lookup"><span data-stu-id="1b078-599">For example:</span></span>

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

<span data-ttu-id="1b078-600">a fenti hello eredményezne.</span><span class="sxs-lookup"><span data-stu-id="1b078-600">hello above would produce</span></span>

<table><thead><tr><th><span data-ttu-id="1b078-601">Készítsen azonosítója</span><span class="sxs-lookup"><span data-stu-id="1b078-601">produce id</span></span></th><th><span data-ttu-id="1b078-602">leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-602">description</span></span></th></tr></thead><tbody><tr><td><span data-ttu-id="1b078-603">0</span><span class="sxs-lookup"><span data-stu-id="1b078-603">0</span></span></td><td><span data-ttu-id="1b078-604">friss almák</span><span class="sxs-lookup"><span data-stu-id="1b078-604">fresh apples</span></span></td></tr><tr><td><span data-ttu-id="1b078-605">1</span><span class="sxs-lookup"><span data-stu-id="1b078-605">1</span></span></td><td><span data-ttu-id="1b078-606">friss narancs</span><span class="sxs-lookup"><span data-stu-id="1b078-606">fresh oranges</span></span></td></tr></tbody></table><span data-ttu-id="1b078-607">"</span><span class="sxs-lookup"><span data-stu-id="1b078-607">"</span></span>

<span data-ttu-id="1b078-608">Ha hello `from` tulajdonság értéke üres tömb, hello eredménye a program üres táblát.</span><span class="sxs-lookup"><span data-stu-id="1b078-608">If hello `from` property value is an empty array, hello output is an empty table.</span></span>

|<span data-ttu-id="1b078-609">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-609">Name</span></span>|<span data-ttu-id="1b078-610">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-610">Required</span></span>|<span data-ttu-id="1b078-611">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-611">Type</span></span>|<span data-ttu-id="1b078-612">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-612">Description</span></span>|
|--------|------------|--------|---------------|
|<span data-ttu-id="1b078-613">A</span><span class="sxs-lookup"><span data-stu-id="1b078-613">from</span></span>|<span data-ttu-id="1b078-614">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-614">Yes</span></span>|<span data-ttu-id="1b078-615">Tömb</span><span class="sxs-lookup"><span data-stu-id="1b078-615">Array</span></span>|<span data-ttu-id="1b078-616">hello forrástömb.</span><span class="sxs-lookup"><span data-stu-id="1b078-616">hello source array.</span></span>|
|<span data-ttu-id="1b078-617">Formátumban</span><span class="sxs-lookup"><span data-stu-id="1b078-617">format</span></span>|<span data-ttu-id="1b078-618">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-618">Yes</span></span>|<span data-ttu-id="1b078-619">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-619">String</span></span>|<span data-ttu-id="1b078-620">formátumú, vagy hello **CSV** vagy **HTML**.</span><span class="sxs-lookup"><span data-stu-id="1b078-620">hello format, either **CSV** or **HTML**.</span></span>|
|<span data-ttu-id="1b078-621">oszlopok</span><span class="sxs-lookup"><span data-stu-id="1b078-621">columns</span></span>|<span data-ttu-id="1b078-622">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-622">No</span></span>|<span data-ttu-id="1b078-623">Tömb</span><span class="sxs-lookup"><span data-stu-id="1b078-623">Array</span></span>|<span data-ttu-id="1b078-624">hello oszlopok.</span><span class="sxs-lookup"><span data-stu-id="1b078-624">hello columns.</span></span> <span data-ttu-id="1b078-625">Lehetővé teszi, hogy toooverride hello alapértelmezett hello tábla alakját.</span><span class="sxs-lookup"><span data-stu-id="1b078-625">Allows toooverride hello default shape of hello table.</span></span>|
|<span data-ttu-id="1b078-626">Oszlopfejléc</span><span class="sxs-lookup"><span data-stu-id="1b078-626">column header</span></span>|<span data-ttu-id="1b078-627">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-627">No</span></span>|<span data-ttu-id="1b078-628">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-628">String</span></span>|<span data-ttu-id="1b078-629">hello hello oszlop fejlécében.</span><span class="sxs-lookup"><span data-stu-id="1b078-629">hello header of hello column.</span></span>|
|<span data-ttu-id="1b078-630">oszlop értéke</span><span class="sxs-lookup"><span data-stu-id="1b078-630">column value</span></span>|<span data-ttu-id="1b078-631">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-631">Yes</span></span>|<span data-ttu-id="1b078-632">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-632">String</span></span>|<span data-ttu-id="1b078-633">hello hello oszlop értéke.</span><span class="sxs-lookup"><span data-stu-id="1b078-633">hello value of hello column.</span></span>|

## <a name="workflow-action"></a><span data-ttu-id="1b078-634">Munkafolyamat-művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-634">Workflow action</span></span>   

|<span data-ttu-id="1b078-635">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-635">Name</span></span>|<span data-ttu-id="1b078-636">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-636">Required</span></span>|<span data-ttu-id="1b078-637">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-637">Type</span></span>|<span data-ttu-id="1b078-638">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-638">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-639">állomás azonosítója</span><span class="sxs-lookup"><span data-stu-id="1b078-639">host id</span></span>|<span data-ttu-id="1b078-640">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-640">Yes</span></span>|<span data-ttu-id="1b078-641">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-641">String</span></span>|<span data-ttu-id="1b078-642">hello erőforrás-azonosító, amelyet az toocall hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="1b078-642">hello resource ID of hello workflow that you want toocall.</span></span>|  
|<span data-ttu-id="1b078-643">állomás Eseményindító_neve</span><span class="sxs-lookup"><span data-stu-id="1b078-643">host triggerName</span></span>|<span data-ttu-id="1b078-644">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-644">Yes</span></span>|<span data-ttu-id="1b078-645">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-645">String</span></span>|<span data-ttu-id="1b078-646">hello neve, amelyet az tooinvoke hello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="1b078-646">hello name of hello trigger that you want tooinvoke.</span></span>|  
|<span data-ttu-id="1b078-647">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-647">queries</span></span>|<span data-ttu-id="1b078-648">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-648">No</span></span>|<span data-ttu-id="1b078-649">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-649">Object</span></span>|<span data-ttu-id="1b078-650">Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-650">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="1b078-651">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-651">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="1b078-652">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-652">headers</span></span>|<span data-ttu-id="1b078-653">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-653">No</span></span>|<span data-ttu-id="1b078-654">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-654">Object</span></span>|<span data-ttu-id="1b078-655">Egyes toohello kérelmet küldött hello fejlécek jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-655">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-656">Például a tooset hello nyelvét és típusát kérelem lehetőséget:`"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span><span class="sxs-lookup"><span data-stu-id="1b078-656">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us",  "Content-Type": "application/json" }`</span></span>|  
|<span data-ttu-id="1b078-657">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-657">body</span></span>|<span data-ttu-id="1b078-658">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-658">No</span></span>|<span data-ttu-id="1b078-659">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-659">Object</span></span>|<span data-ttu-id="1b078-660">Hello hasznos toohello végpont küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-660">Represents hello payload sent toohello endpoint.</span></span>|  
  
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
  
<span data-ttu-id="1b078-661">Hozzáférés-ellenőrzést készült hello munkafolyamat \(pontosabban hello eseményindító\), ami azt jelenti, hogy toohello munkafolyamat kell hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="1b078-661">An access check is made on hello workflow \(more specifically, hello trigger\), meaning you need access toohello workflow.</span></span>  
  
<span data-ttu-id="1b078-662">hello kiírja a hello `workflow` művelet alapján hello definiált `response` hello alárendelt munkafolyamat műveletét.</span><span class="sxs-lookup"><span data-stu-id="1b078-662">hello outputs from hello `workflow` action are based on what you defined in hello `response` action in hello child workflow.</span></span> <span data-ttu-id="1b078-663">Ha nincs megadva, a `response` műveletet, majd hello kimenetek üresek.</span><span class="sxs-lookup"><span data-stu-id="1b078-663">If you have not defined any `response` action, then hello outputs are empty.</span></span>  

## <a name="function-action"></a><span data-ttu-id="1b078-664">Függvény művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-664">Function action</span></span>   

|<span data-ttu-id="1b078-665">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-665">Name</span></span>|<span data-ttu-id="1b078-666">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-666">Required</span></span>|<span data-ttu-id="1b078-667">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-667">Type</span></span>|<span data-ttu-id="1b078-668">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-668">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-669">függvény azonosítója</span><span class="sxs-lookup"><span data-stu-id="1b078-669">function id</span></span>|<span data-ttu-id="1b078-670">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-670">Yes</span></span>|<span data-ttu-id="1b078-671">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-671">String</span></span>|<span data-ttu-id="1b078-672">hello erőforrás-azonosító, amelyet az tooinvoke hello függvény.</span><span class="sxs-lookup"><span data-stu-id="1b078-672">hello resource ID of hello function that you want tooinvoke.</span></span>|  
|<span data-ttu-id="1b078-673">Módszer</span><span class="sxs-lookup"><span data-stu-id="1b078-673">method</span></span>|<span data-ttu-id="1b078-674">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-674">No</span></span>|<span data-ttu-id="1b078-675">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-675">String</span></span>|<span data-ttu-id="1b078-676">HTTP-metódus hello tooinvoke hello funkció használata.</span><span class="sxs-lookup"><span data-stu-id="1b078-676">hello HTTP method used tooinvoke hello function.</span></span> <span data-ttu-id="1b078-677">Alapértelmezés szerint van `POST` Ha nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="1b078-677">By default, it is `POST` when not specified.</span></span>|  
|<span data-ttu-id="1b078-678">Lekérdezések</span><span class="sxs-lookup"><span data-stu-id="1b078-678">queries</span></span>|<span data-ttu-id="1b078-679">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-679">No</span></span>|<span data-ttu-id="1b078-680">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-680">Object</span></span>|<span data-ttu-id="1b078-681">Hello lekérdezési paraméterek tooadd toohello URL-címet jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-681">Represents hello query parameters tooadd toohello URL.</span></span> <span data-ttu-id="1b078-682">Például `"queries" : { "api-version": "2015-02-01" }` hozzáadja `?api-version=2015-02-01` toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1b078-682">For example, `"queries" : { "api-version": "2015-02-01" }` adds `?api-version=2015-02-01` toohello URL.</span></span>|  
|<span data-ttu-id="1b078-683">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1b078-683">headers</span></span>|<span data-ttu-id="1b078-684">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-684">No</span></span>|<span data-ttu-id="1b078-685">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-685">Object</span></span>|<span data-ttu-id="1b078-686">Egyes toohello kérelmet küldött hello fejlécek jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-686">Represents each of hello headers that is sent toohello request.</span></span> <span data-ttu-id="1b078-687">Például a tooset hello nyelvet és a kérelem típusa: `"headers" : { "Accept-Language": "en-us" }`.</span><span class="sxs-lookup"><span data-stu-id="1b078-687">For example, tooset hello language and type on a request: `"headers" : { "Accept-Language": "en-us" }`.</span></span>|  
|<span data-ttu-id="1b078-688">Törzs</span><span class="sxs-lookup"><span data-stu-id="1b078-688">body</span></span>|<span data-ttu-id="1b078-689">Nem</span><span class="sxs-lookup"><span data-stu-id="1b078-689">No</span></span>|<span data-ttu-id="1b078-690">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-690">Object</span></span>|<span data-ttu-id="1b078-691">Hello hasznos toohello végpont küldött jelöli.</span><span class="sxs-lookup"><span data-stu-id="1b078-691">Represents hello payload sent toohello endpoint.</span></span>|  

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

<span data-ttu-id="1b078-692">Hello logikai alkalmazás mentésekor a hivatkozott hello függvény néhány ellenőrzést végezzük:</span><span class="sxs-lookup"><span data-stu-id="1b078-692">When you save hello logic app, we perform some checks on hello referenced function:</span></span>
-   <span data-ttu-id="1b078-693">Toohave hozzáférés toohello függvény van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1b078-693">You need toohave access toohello function.</span></span>
-   <span data-ttu-id="1b078-694">Szabványos HTTP-eseményindítóval vagy általános JSON webhook eseményindító csak engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="1b078-694">Only standard HTTP trigger or generic JSON webhook trigger is allowed.</span></span>
-   <span data-ttu-id="1b078-695">A megadott útvonal nem tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1b078-695">It should not have any route defined.</span></span>
-   <span data-ttu-id="1b078-696">Csak "függvény" és "névtelen" jogosultsági szint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="1b078-696">Only "function" and "anonymous" authorization level is allowed.</span></span>

<span data-ttu-id="1b078-697">hello indítási URL-cím lekérése, gyorsítótárazva, és futásidőben használja.</span><span class="sxs-lookup"><span data-stu-id="1b078-697">hello trigger URL is retrieved, cached, and used at runtime.</span></span> <span data-ttu-id="1b078-698">Így az összes műveletet érvényteleníti gyorsítótárazott hello URL-címe, ha futásidőben hello művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="1b078-698">So if any operation invalidates hello cached URL, hello action fails at runtime.</span></span> <span data-ttu-id="1b078-699">toowork kerülheti, mentés hello logikai alkalmazás, amely miatt a logic app tooretrieve és hello indítási URL-cím gyorsítótárazása újra.</span><span class="sxs-lookup"><span data-stu-id="1b078-699">toowork around this, save hello logic app again, which will cause logic app tooretrieve and cache hello trigger URL again.</span></span>

## <a name="collection-actions-scopes-and-loops"></a><span data-ttu-id="1b078-700">Gyűjtemény műveletek (hatókörök és hurkok)</span><span class="sxs-lookup"><span data-stu-id="1b078-700">Collection actions (scopes and loops)</span></span>

<span data-ttu-id="1b078-701">Néhány művelettípusok műveletek belül maguk is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1b078-701">Some action types can contain actions within themselves.</span></span> <span data-ttu-id="1b078-702">A gyűjteményen belül hivatkozás műveletek közvetlenül kívül hello gyűjtemény lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="1b078-702">Reference actions within a collection can be referenced directly outside of hello collection.</span></span> <span data-ttu-id="1b078-703">Ha az Ön által definiált `http` hatókörben, `@body('http')` a munkafolyamaton belül bárhol továbbra is érvényes.</span><span class="sxs-lookup"><span data-stu-id="1b078-703">If you defined `http` in a scope, `@body('http')` is still valid anywhere in a workflow.</span></span> <span data-ttu-id="1b078-704">A gyűjteményen belül műveletek is `runAfter` belül csak más műveletek hello ugyanahhoz a gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="1b078-704">Actions within a collection can `runAfter` only other actions within hello same collection.</span></span>

## <a name="scope-action"></a><span data-ttu-id="1b078-705">Hatókör művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-705">Scope action</span></span>

<span data-ttu-id="1b078-706">Hello `scope` művelet lehetővé teszi, hogy logikailag a munkafolyamat műveleteit.</span><span class="sxs-lookup"><span data-stu-id="1b078-706">hello `scope` action lets you logically group actions in a workflow.</span></span>

|<span data-ttu-id="1b078-707">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-707">Name</span></span>|<span data-ttu-id="1b078-708">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-708">Required</span></span>|<span data-ttu-id="1b078-709">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-709">Type</span></span>|<span data-ttu-id="1b078-710">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-710">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-711">műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-711">actions</span></span>|<span data-ttu-id="1b078-712">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-712">Yes</span></span>|<span data-ttu-id="1b078-713">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-713">Object</span></span>|<span data-ttu-id="1b078-714">Belső műveletek tooexecute hello hatókörén belül</span><span class="sxs-lookup"><span data-stu-id="1b078-714">Inner actions tooexecute within hello scope</span></span>|

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

## <a name="foreach-action"></a><span data-ttu-id="1b078-715">ForEach-művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-715">ForEach action</span></span>

<span data-ttu-id="1b078-716">Ez a ismétlési művelet tömb telepítéseket, és minden elemhez belső műveleteket hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1b078-716">This looping action iterates through an array and performs inner actions for each item.</span></span> <span data-ttu-id="1b078-717">Alapértelmezés szerint a hello foreach hurok párhuzamos (20 végrehajtások párhuzamosan egyszerre) hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="1b078-717">By default, hello foreach loop executes in parallel (20 executions in parallel at a time).</span></span> <span data-ttu-id="1b078-718">Végrehajtási szabályainak használatával hello beállítása `operationOptions` paraméter.</span><span class="sxs-lookup"><span data-stu-id="1b078-718">You can set execution rules using hello `operationOptions` parameter.</span></span>

|<span data-ttu-id="1b078-719">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-719">Name</span></span>|<span data-ttu-id="1b078-720">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-720">Required</span></span>|<span data-ttu-id="1b078-721">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-721">Type</span></span>|<span data-ttu-id="1b078-722">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-722">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-723">műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-723">actions</span></span>|<span data-ttu-id="1b078-724">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-724">Yes</span></span>|<span data-ttu-id="1b078-725">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-725">Object</span></span>|<span data-ttu-id="1b078-726">Belső műveletek tooexecute hello hurkon belül</span><span class="sxs-lookup"><span data-stu-id="1b078-726">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="1b078-727">foreach</span><span class="sxs-lookup"><span data-stu-id="1b078-727">foreach</span></span>|<span data-ttu-id="1b078-728">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-728">Yes</span></span>|<span data-ttu-id="1b078-729">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-729">string</span></span>|<span data-ttu-id="1b078-730">hello tömb tooiterate keresztül</span><span class="sxs-lookup"><span data-stu-id="1b078-730">hello array tooiterate over</span></span>|
|<span data-ttu-id="1b078-731">operationOptions</span><span class="sxs-lookup"><span data-stu-id="1b078-731">operationOptions</span></span>|<span data-ttu-id="1b078-732">nem</span><span class="sxs-lookup"><span data-stu-id="1b078-732">no</span></span>|<span data-ttu-id="1b078-733">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-733">string</span></span>|<span data-ttu-id="1b078-734">Viselkedés művelet lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="1b078-734">Any operation options for behavior.</span></span> <span data-ttu-id="1b078-735">Jelenleg csak támogatja `sequential` tooexecute ismétlési egymás után (az alapértelmezés lesz párhuzamos)</span><span class="sxs-lookup"><span data-stu-id="1b078-735">Currently only supports `sequential` tooexecute iterations sequentially (default behavior is parallel)</span></span>|

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

## <a name="until-action"></a><span data-ttu-id="1b078-736">Amíg a művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-736">Until action</span></span>

<span data-ttu-id="1b078-737">Ismétlési művelet belső műveleteket hajt végre, amíg feltétel eredménye tootrue.</span><span class="sxs-lookup"><span data-stu-id="1b078-737">This looping action executes inner actions until a condition results tootrue.</span></span>

|<span data-ttu-id="1b078-738">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-738">Name</span></span>|<span data-ttu-id="1b078-739">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-739">Required</span></span>|<span data-ttu-id="1b078-740">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-740">Type</span></span>|<span data-ttu-id="1b078-741">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-741">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-742">műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-742">actions</span></span>|<span data-ttu-id="1b078-743">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-743">Yes</span></span>|<span data-ttu-id="1b078-744">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-744">Object</span></span>|<span data-ttu-id="1b078-745">Belső műveletek tooexecute hello hurkon belül</span><span class="sxs-lookup"><span data-stu-id="1b078-745">Inner actions tooexecute within hello loop</span></span>|
|<span data-ttu-id="1b078-746">kifejezés</span><span class="sxs-lookup"><span data-stu-id="1b078-746">expression</span></span>|<span data-ttu-id="1b078-747">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-747">Yes</span></span>|<span data-ttu-id="1b078-748">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-748">string</span></span>|<span data-ttu-id="1b078-749">Mindegyik iteráció után hello kifejezés tooevaluate</span><span class="sxs-lookup"><span data-stu-id="1b078-749">hello expression tooevaluate after each iteration</span></span>|
|<span data-ttu-id="1b078-750">Korlát</span><span class="sxs-lookup"><span data-stu-id="1b078-750">limit</span></span>|<span data-ttu-id="1b078-751">igen</span><span class="sxs-lookup"><span data-stu-id="1b078-751">yes</span></span>|<span data-ttu-id="1b078-752">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-752">Object</span></span>|<span data-ttu-id="1b078-753">hello korlátok hello hurok - legalább egy korlátot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="1b078-753">hello limits for hello loop - at least one limit must be defined</span></span>|
|<span data-ttu-id="1b078-754">Száma</span><span class="sxs-lookup"><span data-stu-id="1b078-754">count</span></span>|<span data-ttu-id="1b078-755">nem</span><span class="sxs-lookup"><span data-stu-id="1b078-755">no</span></span>|<span data-ttu-id="1b078-756">int</span><span class="sxs-lookup"><span data-stu-id="1b078-756">int</span></span>|<span data-ttu-id="1b078-757">hello korlát toohello végrehajtható ismétlések száma</span><span class="sxs-lookup"><span data-stu-id="1b078-757">hello limit toohello number of iterations that can be performed</span></span>|
|<span data-ttu-id="1b078-758">timeout</span><span class="sxs-lookup"><span data-stu-id="1b078-758">timeout</span></span>|<span data-ttu-id="1b078-759">nem</span><span class="sxs-lookup"><span data-stu-id="1b078-759">no</span></span>|<span data-ttu-id="1b078-760">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-760">string</span></span>|<span data-ttu-id="1b078-761">mennyi ideig kell a hurok hello időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="1b078-761">hello timeout for how long it should loop.</span></span>  <span data-ttu-id="1b078-762">ISO 8601 formátumban</span><span class="sxs-lookup"><span data-stu-id="1b078-762">ISO 8601 format</span></span>|


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

## <a name="conditions---if-action"></a><span data-ttu-id="1b078-763">Feltételek – Ha a művelet</span><span class="sxs-lookup"><span data-stu-id="1b078-763">Conditions - If Action</span></span>

<span data-ttu-id="1b078-764">Hello `If` művelet lehetővé teszi, hogy olyan feltétel értékelése, és hogy hello kifejezés túl értéke alapján ág végrehajtható`true`.</span><span class="sxs-lookup"><span data-stu-id="1b078-764">hello `If` action lets you evaluate a condition and execute a branch based on whether hello expression evaluates too`true`.</span></span>

|<span data-ttu-id="1b078-765">Név</span><span class="sxs-lookup"><span data-stu-id="1b078-765">Name</span></span>|<span data-ttu-id="1b078-766">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1b078-766">Required</span></span>|<span data-ttu-id="1b078-767">Típus</span><span class="sxs-lookup"><span data-stu-id="1b078-767">Type</span></span>|<span data-ttu-id="1b078-768">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b078-768">Description</span></span>|  
|--------|------------|--------|---------------|  
|<span data-ttu-id="1b078-769">műveletek</span><span class="sxs-lookup"><span data-stu-id="1b078-769">actions</span></span>|<span data-ttu-id="1b078-770">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-770">Yes</span></span>|<span data-ttu-id="1b078-771">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-771">Object</span></span>|<span data-ttu-id="1b078-772">Ha a kifejezés eredménye túl belső műveletek tooexecute`true`</span><span class="sxs-lookup"><span data-stu-id="1b078-772">Inner actions tooexecute when expression evaluates too`true`</span></span>|
|<span data-ttu-id="1b078-773">kifejezés</span><span class="sxs-lookup"><span data-stu-id="1b078-773">expression</span></span>|<span data-ttu-id="1b078-774">Igen</span><span class="sxs-lookup"><span data-stu-id="1b078-774">Yes</span></span>|<span data-ttu-id="1b078-775">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1b078-775">string</span></span>|<span data-ttu-id="1b078-776">hello kifejezés tooevaluate</span><span class="sxs-lookup"><span data-stu-id="1b078-776">hello expression tooevaluate</span></span>|
|<span data-ttu-id="1b078-777">más</span><span class="sxs-lookup"><span data-stu-id="1b078-777">else</span></span>|<span data-ttu-id="1b078-778">nem</span><span class="sxs-lookup"><span data-stu-id="1b078-778">no</span></span>|<span data-ttu-id="1b078-779">Objektum</span><span class="sxs-lookup"><span data-stu-id="1b078-779">Object</span></span>|<span data-ttu-id="1b078-780">Ha a kifejezés eredménye túl belső műveletek tooexecute`false`</span><span class="sxs-lookup"><span data-stu-id="1b078-780">Inner actions tooexecute when expression evaluates too`false`</span></span>|
  
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
  
<span data-ttu-id="1b078-781">hello következő táblázat példákat mutat a feltételek használatát kifejezések egy műveletben:</span><span class="sxs-lookup"><span data-stu-id="1b078-781">hello following table shows examples of how conditions can use expressions in an action:</span></span>  
  
|<span data-ttu-id="1b078-782">JSON-érték</span><span class="sxs-lookup"><span data-stu-id="1b078-782">JSON value</span></span>|<span data-ttu-id="1b078-783">eredménye</span><span class="sxs-lookup"><span data-stu-id="1b078-783">Result</span></span>|  
|--------------|----------|  
|`"expression": "@parameters('hasSpecialAction')"`|<span data-ttu-id="1b078-784">Bármely érték, amely tootrue értékelné ki, ez az állapot toopass okoz.</span><span class="sxs-lookup"><span data-stu-id="1b078-784">Any value that would evaluate tootrue causes this condition toopass.</span></span> <span data-ttu-id="1b078-785">Csak a logikai kifejezések használhatók.</span><span class="sxs-lookup"><span data-stu-id="1b078-785">Only Boolean expressions are supported.</span></span> <span data-ttu-id="1b078-786">más tooconvert típusokat tooBoolean, függvények használata `empty`, `equals`.</span><span class="sxs-lookup"><span data-stu-id="1b078-786">tooconvert other types tooBoolean, use functions `empty`, `equals`.</span></span>|  
|`"expression": "@greater(actions('act1').output.value, parameters('threshold'))"`|<span data-ttu-id="1b078-787">Összehasonlító függvény támogatott.</span><span class="sxs-lookup"><span data-stu-id="1b078-787">Comparison functions are supported.</span></span> <span data-ttu-id="1b078-788">Például hello itt hello művelet csak akkor megy végbe hello küszöbértéknél nagyobb act1 hello kimenete esetén.</span><span class="sxs-lookup"><span data-stu-id="1b078-788">For hello example here, hello action only executes when hello output of act1 is greater than hello threshold.</span></span>|  
|`"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"`|<span data-ttu-id="1b078-789">Támogatott toocreate beágyazott logikai kifejezésen logika funkciók is.</span><span class="sxs-lookup"><span data-stu-id="1b078-789">Logic functions are also supported toocreate nested Boolean expressions.</span></span> <span data-ttu-id="1b078-790">Ebben az esetben hello művelet hajtja végre, amikor hello act1 eredménye hello küszöbérték fölött vagy alatt 100.</span><span class="sxs-lookup"><span data-stu-id="1b078-790">In this case, hello action executes when hello output of act1 is above hello threshold or below 100.</span></span>|  
|`"expression": "@equals(length(actions('act1').outputs.errors), 0))"`|<span data-ttu-id="1b078-791">Tömb funkciók toocheck is használja, ha egy tömb van elemeket.</span><span class="sxs-lookup"><span data-stu-id="1b078-791">You can use array functions toocheck if an array has any items.</span></span> <span data-ttu-id="1b078-792">Ebben az esetben hello műveletet hajt végre, ha hello hibák tömb üres.</span><span class="sxs-lookup"><span data-stu-id="1b078-792">In this case, hello action executes when hello errors array is empty.</span></span>| 
|`"expression": "parameters('hasSpecialAction')"`|<span data-ttu-id="1b078-793">Hiba – nem egy érvényes, mert @ van szükség a feltétel feltételeket.</span><span class="sxs-lookup"><span data-stu-id="1b078-793">Error - not a valid condition because @ is required for conditions.</span></span>|  
  
<span data-ttu-id="1b078-794">Ha a feltétel sikeresen, hello feltétel jelölése `Succeeded`.</span><span class="sxs-lookup"><span data-stu-id="1b078-794">If a condition evaluates successfully, hello condition is marked as `Succeeded`.</span></span> <span data-ttu-id="1b078-795">Műveletek belül vagy hello `actions` vagy `else` objektumok kiértékelése túl`Succeeded` hajtotta végre, és sikeresen befejeződött, `Failed` hajtotta végre, és nem sikerült, vagy `Skipped` mikor nem hajtotta végre a fiókirodában.</span><span class="sxs-lookup"><span data-stu-id="1b078-795">Actions within either hello `actions` or `else` objects evaluate too`Succeeded` when executed and succeeded, `Failed` when executed and failed, or `Skipped` when that branch is not executed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b078-796">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b078-796">Next steps</span></span>

[<span data-ttu-id="1b078-797">A munkafolyamat szolgáltatás REST API</span><span class="sxs-lookup"><span data-stu-id="1b078-797">Workflow Service REST API</span></span>](https://docs.microsoft.com/rest/api/logic/workflows)
