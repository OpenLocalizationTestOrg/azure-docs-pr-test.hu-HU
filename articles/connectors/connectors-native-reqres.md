---
title: "kérelem és válasz műveletek aaaUse |} Microsoft Docs"
description: "Hello kérelem-válasz eseményindítója és tevékenysége fel az Azure logic App áttekintése"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 24c378cc12d5f3f65116d5e59278236186a99662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-request-and-response-components"></a><span data-ttu-id="c0e93-103">Első lépések hello kérés- és összetevői</span><span class="sxs-lookup"><span data-stu-id="c0e93-103">Get started with hello request and response components</span></span>
<span data-ttu-id="c0e93-104">Hello kérelem-válasz összetevőket logikai alkalmazás reagálhat a valós idejű tooevents.</span><span class="sxs-lookup"><span data-stu-id="c0e93-104">With hello request and response components in a logic app, you can respond in real time tooevents.</span></span>

<span data-ttu-id="c0e93-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="c0e93-105">For example, you can:</span></span>

* <span data-ttu-id="c0e93-106">Válaszol a HTTP-kérelem tooan adatokkal keresztül logikai alkalmazás a helyi adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="c0e93-106">Respond tooan HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="c0e93-107">Indítás, a logikai alkalmazást egy külső webhook eseményből.</span><span class="sxs-lookup"><span data-stu-id="c0e93-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="c0e93-108">Hívja meg a logikai alkalmazás belül egy másik logikai alkalmazás a kérelem-válasz művelettel.</span><span class="sxs-lookup"><span data-stu-id="c0e93-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="c0e93-109">tooget használatának hello kérelem-válasz műveletek a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c0e93-109">tooget started using hello request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-request-trigger"></a><span data-ttu-id="c0e93-110">HTTP-kérelem eseményindító hello használata</span><span class="sxs-lookup"><span data-stu-id="c0e93-110">Use hello HTTP Request trigger</span></span>
<span data-ttu-id="c0e93-111">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="c0e93-111">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="c0e93-112">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c0e93-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="c0e93-113">Íme egy parancssorozat-példa hogyan tooset be HTTP hello Logic App Designer kérheti.</span><span class="sxs-lookup"><span data-stu-id="c0e93-113">Here’s an example sequence of how tooset up an HTTP request in hello Logic App Designer.</span></span>

1. <span data-ttu-id="c0e93-114">Adja hozzá a hello eseményindító **kérelem - amikor egy HTTP-kérelem érkezik** a Logic Apps alkalmazást a.</span><span class="sxs-lookup"><span data-stu-id="c0e93-114">Add hello trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="c0e93-115">Is megadhat egy JSON-séma (például egy olyan eszközzel [JSONSchema.net](http://jsonschema.net)) a hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="c0e93-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for hello request body.</span></span> <span data-ttu-id="c0e93-116">Ez lehetővé teszi tulajdonságok hello Tervező toogenerate jogkivonatainak hello HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="c0e93-116">This allows hello designer toogenerate tokens for properties in hello HTTP request.</span></span>
2. <span data-ttu-id="c0e93-117">Adja hozzá egy másik művelet, így hello logikai alkalmazás mentheti.</span><span class="sxs-lookup"><span data-stu-id="c0e93-117">Add another action so that you can save hello logic app.</span></span>
3. <span data-ttu-id="c0e93-118">Hello logikai alkalmazást a mentés után a hello HTTP-kérelem URL-CÍMÉT az hello kérelem kártya kérheti le.</span><span class="sxs-lookup"><span data-stu-id="c0e93-118">After saving hello logic app, you can get hello HTTP request URL from hello request card.</span></span>
4. <span data-ttu-id="c0e93-119">Egy HTTP POST (például egy eszközzel [Postman](https://www.getpostman.com/)) toohello URL-cím eseményindítók hello logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c0e93-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) toohello URL triggers hello logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="c0e93-120">Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` azonnal érkezik válasz toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="c0e93-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span> <span data-ttu-id="c0e93-121">Hello válasz művelet toocustomize választ is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c0e93-121">You can use hello response action toocustomize a response.</span></span>
> 
> 

![Válasz eseményindító](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-hello-http-response-action"></a><span data-ttu-id="c0e93-123">HTTP-válasz művelet hello használata</span><span class="sxs-lookup"><span data-stu-id="c0e93-123">Use hello HTTP Response action</span></span>
<span data-ttu-id="c0e93-124">hello HTTP-válasz művelet csak akkor érvényes, ha egy munkafolyamatban, amely egy HTTP-kérés használhatja.</span><span class="sxs-lookup"><span data-stu-id="c0e93-124">hello HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="c0e93-125">Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` azonnal érkezik válasz toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="c0e93-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned toohello caller.</span></span>  <span data-ttu-id="c0e93-126">A válasz művelet hello munkafolyamaton belül bármely lépése is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="c0e93-126">You can add a response action at any step within hello workflow.</span></span> <span data-ttu-id="c0e93-127">hello logikai alkalmazás csak fenntartja hello bejövő kérelem választ egy percig.</span><span class="sxs-lookup"><span data-stu-id="c0e93-127">hello logic app only keeps hello incoming request open for one minute for a response.</span></span>  <span data-ttu-id="c0e93-128">Ha nem választ küldött hello munkafolyamat (és hello definíciója van a válasz művelet), egy perc után egy `504 GATEWAY TIMEOUT` toohello hívó adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c0e93-128">After one minute, if no response was sent from hello workflow (and a response action exists in hello definition), a `504 GATEWAY TIMEOUT` is returned toohello caller.</span></span>

<span data-ttu-id="c0e93-129">Itt hogyan tooadd egy HTTP-válasz műveletet:</span><span class="sxs-lookup"><span data-stu-id="c0e93-129">Here's how tooadd an HTTP Response action:</span></span>

1. <span data-ttu-id="c0e93-130">Jelölje be hello **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="c0e93-130">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="c0e93-131">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c0e93-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="c0e93-132">Hello művelet keresési mezőbe, írja be a **válasz** toolist hello válasz művelet.</span><span class="sxs-lookup"><span data-stu-id="c0e93-132">In hello action search box, type **response** toolist hello Response action.</span></span>
   
    ![Válassza ki a hello válasz művelet](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="c0e93-134">Adja hozzá a HTTP-válaszüzenetnek hello szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c0e93-134">Add in any parameters that are required for hello HTTP response message.</span></span>
   
    ![Teljes hello válasz művelet](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="c0e93-136">Kattintson a bal felső sarkának hello hello eszköztár toosave, és a logic app fog egyaránt mentés, és tegye közzé (aktiválása).</span><span class="sxs-lookup"><span data-stu-id="c0e93-136">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="c0e93-137">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="c0e93-137">Request trigger</span></span>
<span data-ttu-id="c0e93-138">Az alábbiakban hello részletek hello eseményindító, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="c0e93-138">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="c0e93-139">Nincs egyetlen kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="c0e93-139">There is a single request trigger.</span></span>

| <span data-ttu-id="c0e93-140">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="c0e93-140">Trigger</span></span> | <span data-ttu-id="c0e93-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0e93-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c0e93-142">Kérés</span><span class="sxs-lookup"><span data-stu-id="c0e93-142">Request</span></span> |<span data-ttu-id="c0e93-143">Akkor következik be, amikor egy HTTP-kérelem érkezik</span><span class="sxs-lookup"><span data-stu-id="c0e93-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="c0e93-144">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="c0e93-144">Response action</span></span>
<span data-ttu-id="c0e93-145">Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="c0e93-145">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="c0e93-146">Nincs olyan egyetlen válasz művelet, amely csak akkor használható, amikor egy kérelem eseményindító kíséri.</span><span class="sxs-lookup"><span data-stu-id="c0e93-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="c0e93-147">Műveletek</span><span class="sxs-lookup"><span data-stu-id="c0e93-147">Action</span></span> | <span data-ttu-id="c0e93-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0e93-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c0e93-149">Válasz</span><span class="sxs-lookup"><span data-stu-id="c0e93-149">Response</span></span> |<span data-ttu-id="c0e93-150">Visszaadja egy válasz toohello korrelált HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="c0e93-150">Returns a response toohello correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="c0e93-151">Eseményindítója és tevékenysége részletei</span><span class="sxs-lookup"><span data-stu-id="c0e93-151">Trigger and action details</span></span>
<span data-ttu-id="c0e93-152">hello táblázatokban leíró hello bemeneti mezőinek hello eseményindítója és tevékenysége fel, és megfelelő kimeneti részletek hello.</span><span class="sxs-lookup"><span data-stu-id="c0e93-152">hello following tables describe hello input fields for hello trigger and action, and hello corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="c0e93-153">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="c0e93-153">Request trigger</span></span>
<span data-ttu-id="c0e93-154">hello egy beviteli mezőt a bejövő HTTP-kérelem hello eseményindító látható.</span><span class="sxs-lookup"><span data-stu-id="c0e93-154">hello following is an input field for hello trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="c0e93-155">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="c0e93-155">Display name</span></span> | <span data-ttu-id="c0e93-156">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="c0e93-156">Property name</span></span> | <span data-ttu-id="c0e93-157">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0e93-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c0e93-158">JSON-séma</span><span class="sxs-lookup"><span data-stu-id="c0e93-158">JSON Schema</span></span> |<span data-ttu-id="c0e93-159">Séma</span><span class="sxs-lookup"><span data-stu-id="c0e93-159">schema</span></span> |<span data-ttu-id="c0e93-160">hello JSON-séma hello HTTP-kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="c0e93-160">hello JSON schema of hello HTTP request body</span></span> |

<br>

<span data-ttu-id="c0e93-161">**Kimeneti részletei**</span><span class="sxs-lookup"><span data-stu-id="c0e93-161">**Output details**</span></span>

<span data-ttu-id="c0e93-162">Az alábbiakban hello kimeneti részletes hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="c0e93-162">hello following are output details for hello request.</span></span>

| <span data-ttu-id="c0e93-163">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="c0e93-163">Property name</span></span> | <span data-ttu-id="c0e93-164">Adattípus</span><span class="sxs-lookup"><span data-stu-id="c0e93-164">Data type</span></span> | <span data-ttu-id="c0e93-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0e93-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c0e93-166">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="c0e93-166">Headers</span></span> |<span data-ttu-id="c0e93-167">Objektum</span><span class="sxs-lookup"><span data-stu-id="c0e93-167">object</span></span> |<span data-ttu-id="c0e93-168">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="c0e93-168">Request headers</span></span> |
| <span data-ttu-id="c0e93-169">Törzs</span><span class="sxs-lookup"><span data-stu-id="c0e93-169">Body</span></span> |<span data-ttu-id="c0e93-170">Objektum</span><span class="sxs-lookup"><span data-stu-id="c0e93-170">object</span></span> |<span data-ttu-id="c0e93-171">Kérelem objektum</span><span class="sxs-lookup"><span data-stu-id="c0e93-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="c0e93-172">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="c0e93-172">Response action</span></span>
<span data-ttu-id="c0e93-173">Az alábbiakban hello hello HTTP-válasz művelet a beviteli mezők.</span><span class="sxs-lookup"><span data-stu-id="c0e93-173">hello following are input fields for hello HTTP Response action.</span></span> <span data-ttu-id="c0e93-174">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="c0e93-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="c0e93-175">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="c0e93-175">Display name</span></span> | <span data-ttu-id="c0e93-176">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="c0e93-176">Property name</span></span> | <span data-ttu-id="c0e93-177">Leírás</span><span class="sxs-lookup"><span data-stu-id="c0e93-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c0e93-178">Állapot kód *</span><span class="sxs-lookup"><span data-stu-id="c0e93-178">Status Code*</span></span> |<span data-ttu-id="c0e93-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="c0e93-179">statusCode</span></span> |<span data-ttu-id="c0e93-180">hello HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="c0e93-180">hello HTTP status code</span></span> |
| <span data-ttu-id="c0e93-181">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="c0e93-181">Headers</span></span> |<span data-ttu-id="c0e93-182">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="c0e93-182">headers</span></span> |<span data-ttu-id="c0e93-183">Minden válasz fejlécek tooinclude, egy JSON-objektum</span><span class="sxs-lookup"><span data-stu-id="c0e93-183">A JSON object of any response headers tooinclude</span></span> |
| <span data-ttu-id="c0e93-184">Törzs</span><span class="sxs-lookup"><span data-stu-id="c0e93-184">Body</span></span> |<span data-ttu-id="c0e93-185">Törzs</span><span class="sxs-lookup"><span data-stu-id="c0e93-185">body</span></span> |<span data-ttu-id="c0e93-186">hello választörzs</span><span class="sxs-lookup"><span data-stu-id="c0e93-186">hello response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c0e93-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0e93-187">Next steps</span></span>
<span data-ttu-id="c0e93-188">Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c0e93-188">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="c0e93-189">Ismerje meg a hello más rendelkezésre álló összekötők logic Apps alkalmazások felmérésével a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c0e93-189">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

