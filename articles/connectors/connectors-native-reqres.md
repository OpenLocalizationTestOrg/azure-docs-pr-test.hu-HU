---
title: "Kérelem és válasz műveletek használata |} Microsoft Docs"
description: "A kérelem-válasz eseményindítója és tevékenysége fel az Azure logic App áttekintése"
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
ms.openlocfilehash: e45b07d709927af64cfba28dfb0d8ee9cb8893b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-request-and-response-components"></a><span data-ttu-id="6cc1c-103">Első lépések a kérelem-válasz összetevői</span><span class="sxs-lookup"><span data-stu-id="6cc1c-103">Get started with the request and response components</span></span>
<span data-ttu-id="6cc1c-104">A logikai alkalmazás kérelem-válasz összetevővel reagálhat valós idejű eseményekre.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-104">With the request and response components in a logic app, you can respond in real time to events.</span></span>

<span data-ttu-id="6cc1c-105">Megteheti például a következőt:</span><span class="sxs-lookup"><span data-stu-id="6cc1c-105">For example, you can:</span></span>

* <span data-ttu-id="6cc1c-106">Válaszol a HTTP-kérelem adatokkal keresztül logikai alkalmazás a helyi adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-106">Respond to an HTTP request with data from an on-premises database through a logic app.</span></span>
* <span data-ttu-id="6cc1c-107">Indítás, a logikai alkalmazást egy külső webhook eseményből.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-107">Trigger a logic app from an external webhook event.</span></span>
* <span data-ttu-id="6cc1c-108">Hívja meg a logikai alkalmazás belül egy másik logikai alkalmazás a kérelem-válasz művelettel.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-108">Call a logic app with a request and response action from within another logic app.</span></span>

<span data-ttu-id="6cc1c-109">Első lépések egy logikai alkalmazás a kérelem-válasz műveletek használatával, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6cc1c-109">To get started using the request and response actions in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-http-request-trigger"></a><span data-ttu-id="6cc1c-110">Használja a HTTP-kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="6cc1c-110">Use the HTTP Request trigger</span></span>
<span data-ttu-id="6cc1c-111">Egy eseményindító egy eseményt, amely segítségével indítsa el a munkafolyamatot, amely a logikai alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-111">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="6cc1c-112">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6cc1c-112">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="6cc1c-113">Íme egy parancssorozat-példa bemutatja, hogyan állítson be egy HTTP-kérelem a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-113">Here’s an example sequence of how to set up an HTTP request in the Logic App Designer.</span></span>

1. <span data-ttu-id="6cc1c-114">Adja hozzá az eseményindító **kérelem - amikor egy HTTP-kérelem érkezik** a a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-114">Add the trigger **Request - When an HTTP request is received** in your logic app.</span></span> <span data-ttu-id="6cc1c-115">Is megadhat egy JSON-séma (például egy olyan eszközzel [JSONSchema.net](http://jsonschema.net)) vonatkozó kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-115">You can optionally provide a JSON schema (by using a tool like [JSONSchema.net](http://jsonschema.net)) for the request body.</span></span> <span data-ttu-id="6cc1c-116">Ez lehetővé teszi, hogy a Tervező jogkivonatokat a tulajdonságok a HTTP-kérelmet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-116">This allows the designer to generate tokens for properties in the HTTP request.</span></span>
2. <span data-ttu-id="6cc1c-117">Egy másik művelet hozzáadása, hogy a logikai alkalmazást is mentheti.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-117">Add another action so that you can save the logic app.</span></span>
3. <span data-ttu-id="6cc1c-118">A logikai alkalmazást a mentés után letölthető a HTTP-kérelem URL-CÍMÉT a kérelem kártya.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-118">After saving the logic app, you can get the HTTP request URL from the request card.</span></span>
4. <span data-ttu-id="6cc1c-119">Egy HTTP POST (használhatja, hogy egy eszköz, például [Postman](https://www.getpostman.com/)) URL-címre akkor váltja ki a logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-119">An HTTP POST (you can use a tool like [Postman](https://www.getpostman.com/)) to the URL triggers the logic app.</span></span>

> [!NOTE]
> <span data-ttu-id="6cc1c-120">Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` választ a rendszer azonnal visszaérkezik a hívóhoz.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-120">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span> <span data-ttu-id="6cc1c-121">A válasz művelet segítségével testre szabhatja a választ.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-121">You can use the response action to customize a response.</span></span>
> 
> 

![Válasz eseményindító](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a><span data-ttu-id="6cc1c-123">A HTTP-válasz művelettel</span><span class="sxs-lookup"><span data-stu-id="6cc1c-123">Use the HTTP Response action</span></span>
<span data-ttu-id="6cc1c-124">A HTTP-válasz művelet csak akkor érvényes, ha egy munkafolyamatban, amely egy HTTP-kérés használhatja.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-124">The HTTP Response action is only valid when you use it in a workflow that is triggered by an HTTP request.</span></span> <span data-ttu-id="6cc1c-125">Ha nem adja meg egy válasz művelet egy `202 ACCEPTED` választ a rendszer azonnal visszaérkezik a hívóhoz.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-125">If you don't define a response action, a `202 ACCEPTED` response is immediately returned to the caller.</span></span>  <span data-ttu-id="6cc1c-126">Hozzáadhat egy válasz művelet bármely lépése a munkafolyamaton belül.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-126">You can add a response action at any step within the workflow.</span></span> <span data-ttu-id="6cc1c-127">A logikai alkalmazást csak fenntartja a bejövő kérelem választ egy percig.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-127">The logic app only keeps the incoming request open for one minute for a response.</span></span>  <span data-ttu-id="6cc1c-128">Ha nincs válasz érkezett a munkafolyamat (és a definíciója van a válasz művelet), egy perc után egy `504 GATEWAY TIMEOUT` van visszaérkezik a hívóhoz.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-128">After one minute, if no response was sent from the workflow (and a response action exists in the definition), a `504 GATEWAY TIMEOUT` is returned to the caller.</span></span>

<span data-ttu-id="6cc1c-129">Megtudhatja, hogyan HTTP-válasz művelet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="6cc1c-129">Here's how to add an HTTP Response action:</span></span>

1. <span data-ttu-id="6cc1c-130">Válassza ki a **új lépés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-130">Select the **New Step** button.</span></span>
2. <span data-ttu-id="6cc1c-131">Válasszon **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-131">Choose **Add an action**.</span></span>
3. <span data-ttu-id="6cc1c-132">A művelet a keresőmezőbe írja be **válasz** a válasz művelet listázásához.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-132">In the action search box, type **response** to list the Response action.</span></span>
   
    ![Válassza ki a válasz](./media/connectors-native-reqres/using-action-1.png)
4. <span data-ttu-id="6cc1c-134">Adja hozzá a HTTP-válaszüzenetnek szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-134">Add in any parameters that are required for the HTTP response message.</span></span>
   
    ![A válasz a művelet](./media/connectors-native-reqres/using-action-2.png)
5. <span data-ttu-id="6cc1c-136">Kattintson az eszköztáron menteni a bal felső sarkában, és a Logic Apps alkalmazást mentése és közzététele (aktiválása).</span><span class="sxs-lookup"><span data-stu-id="6cc1c-136">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="request-trigger"></a><span data-ttu-id="6cc1c-137">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="6cc1c-137">Request trigger</span></span>
<span data-ttu-id="6cc1c-138">Az alábbiak az eseményindító, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-138">Here are the details for the trigger that this connector supports.</span></span> <span data-ttu-id="6cc1c-139">Nincs egyetlen kérelem eseményindítót.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-139">There is a single request trigger.</span></span>

| <span data-ttu-id="6cc1c-140">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="6cc1c-140">Trigger</span></span> | <span data-ttu-id="6cc1c-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc1c-141">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6cc1c-142">Kérés</span><span class="sxs-lookup"><span data-stu-id="6cc1c-142">Request</span></span> |<span data-ttu-id="6cc1c-143">Akkor következik be, amikor egy HTTP-kérelem érkezik</span><span class="sxs-lookup"><span data-stu-id="6cc1c-143">Occurs when an HTTP request is received</span></span> |

## <a name="response-action"></a><span data-ttu-id="6cc1c-144">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="6cc1c-144">Response action</span></span>
<span data-ttu-id="6cc1c-145">Az alábbiak a művelet, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-145">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="6cc1c-146">Nincs olyan egyetlen válasz művelet, amely csak akkor használható, amikor egy kérelem eseményindító kíséri.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-146">There is a single response action that can only be used when it is accompanied by a request trigger.</span></span>

| <span data-ttu-id="6cc1c-147">Műveletek</span><span class="sxs-lookup"><span data-stu-id="6cc1c-147">Action</span></span> | <span data-ttu-id="6cc1c-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc1c-148">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6cc1c-149">Válasz</span><span class="sxs-lookup"><span data-stu-id="6cc1c-149">Response</span></span> |<span data-ttu-id="6cc1c-150">A korrelált HTTP-kérelem választ ad vissza</span><span class="sxs-lookup"><span data-stu-id="6cc1c-150">Returns a response to the correlated HTTP request</span></span> |

### <a name="trigger-and-action-details"></a><span data-ttu-id="6cc1c-151">Eseményindítója és tevékenysége részletei</span><span class="sxs-lookup"><span data-stu-id="6cc1c-151">Trigger and action details</span></span>
<span data-ttu-id="6cc1c-152">Az alábbi táblázatban láthatók a beviteli mezők az eseményindító és a művelet, és a megfelelő kimeneti részleteit.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-152">The following tables describe the input fields for the trigger and action, and the corresponding output details.</span></span>

#### <a name="request-trigger"></a><span data-ttu-id="6cc1c-153">Kérelem eseményindító</span><span class="sxs-lookup"><span data-stu-id="6cc1c-153">Request trigger</span></span>
<span data-ttu-id="6cc1c-154">A bejövő HTTP-kérelem az eseményindító egy beviteli mezőt a következő:</span><span class="sxs-lookup"><span data-stu-id="6cc1c-154">The following is an input field for the trigger from an incoming HTTP request.</span></span>

| <span data-ttu-id="6cc1c-155">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="6cc1c-155">Display name</span></span> | <span data-ttu-id="6cc1c-156">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6cc1c-156">Property name</span></span> | <span data-ttu-id="6cc1c-157">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc1c-157">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cc1c-158">JSON-séma</span><span class="sxs-lookup"><span data-stu-id="6cc1c-158">JSON Schema</span></span> |<span data-ttu-id="6cc1c-159">Séma</span><span class="sxs-lookup"><span data-stu-id="6cc1c-159">schema</span></span> |<span data-ttu-id="6cc1c-160">A JSON-séma, a HTTP-kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="6cc1c-160">The JSON schema of the HTTP request body</span></span> |

<br>

<span data-ttu-id="6cc1c-161">**Kimeneti részletei**</span><span class="sxs-lookup"><span data-stu-id="6cc1c-161">**Output details**</span></span>

<span data-ttu-id="6cc1c-162">Az alábbiakban a kérelem részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-162">The following are output details for the request.</span></span>

| <span data-ttu-id="6cc1c-163">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6cc1c-163">Property name</span></span> | <span data-ttu-id="6cc1c-164">Adattípus</span><span class="sxs-lookup"><span data-stu-id="6cc1c-164">Data type</span></span> | <span data-ttu-id="6cc1c-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc1c-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cc1c-166">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="6cc1c-166">Headers</span></span> |<span data-ttu-id="6cc1c-167">Objektum</span><span class="sxs-lookup"><span data-stu-id="6cc1c-167">object</span></span> |<span data-ttu-id="6cc1c-168">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="6cc1c-168">Request headers</span></span> |
| <span data-ttu-id="6cc1c-169">Törzs</span><span class="sxs-lookup"><span data-stu-id="6cc1c-169">Body</span></span> |<span data-ttu-id="6cc1c-170">Objektum</span><span class="sxs-lookup"><span data-stu-id="6cc1c-170">object</span></span> |<span data-ttu-id="6cc1c-171">Kérelem objektum</span><span class="sxs-lookup"><span data-stu-id="6cc1c-171">Request object</span></span> |

#### <a name="response-action"></a><span data-ttu-id="6cc1c-172">Válasz művelet</span><span class="sxs-lookup"><span data-stu-id="6cc1c-172">Response action</span></span>
<span data-ttu-id="6cc1c-173">Az alábbiakban a beviteli mezők a HTTP-válasz művelethez.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-173">The following are input fields for the HTTP Response action.</span></span> <span data-ttu-id="6cc1c-174">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="6cc1c-174">A * means that it is a required field.</span></span>

| <span data-ttu-id="6cc1c-175">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="6cc1c-175">Display name</span></span> | <span data-ttu-id="6cc1c-176">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="6cc1c-176">Property name</span></span> | <span data-ttu-id="6cc1c-177">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cc1c-177">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cc1c-178">Állapot kód *</span><span class="sxs-lookup"><span data-stu-id="6cc1c-178">Status Code*</span></span> |<span data-ttu-id="6cc1c-179">statusCode</span><span class="sxs-lookup"><span data-stu-id="6cc1c-179">statusCode</span></span> |<span data-ttu-id="6cc1c-180">A HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="6cc1c-180">The HTTP status code</span></span> |
| <span data-ttu-id="6cc1c-181">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="6cc1c-181">Headers</span></span> |<span data-ttu-id="6cc1c-182">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="6cc1c-182">headers</span></span> |<span data-ttu-id="6cc1c-183">Bármely válaszfejlécek felvenni, egy JSON-objektum</span><span class="sxs-lookup"><span data-stu-id="6cc1c-183">A JSON object of any response headers to include</span></span> |
| <span data-ttu-id="6cc1c-184">Törzs</span><span class="sxs-lookup"><span data-stu-id="6cc1c-184">Body</span></span> |<span data-ttu-id="6cc1c-185">Törzs</span><span class="sxs-lookup"><span data-stu-id="6cc1c-185">body</span></span> |<span data-ttu-id="6cc1c-186">A választörzs</span><span class="sxs-lookup"><span data-stu-id="6cc1c-186">The response body</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6cc1c-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6cc1c-187">Next steps</span></span>
<span data-ttu-id="6cc1c-188">Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="6cc1c-188">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="6cc1c-189">Az egyéb rendelkezésre álló összekötők logic Apps alkalmazások felmérésével felfedezheti a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="6cc1c-189">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

