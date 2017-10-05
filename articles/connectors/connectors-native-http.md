---
title: "A tetszőleges végpontot HTTP - Azure Logic Apps protokollt használó kommunikációra |} Microsoft Docs"
description: "Hozzon létre a logic apps, amely képes kommunikálni a tetszőleges végpontot HTTP Protokollon keresztül"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan; LADocs
ms.openlocfilehash: d422a07a27ffa62a673bd2d471ae4fc837251dee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-http-action"></a><span data-ttu-id="1149f-103">Ismerkedjen meg a HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="1149f-103">Get started with the HTTP action</span></span>

<span data-ttu-id="1149f-104">A HTTP-művelettel munkafolyamatok kiterjesztheti a szervezet és a tetszőleges végpontot HTTP protokollt használó kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="1149f-104">With the HTTP action, you can extend workflows for your organization and communicate to any endpoint over HTTP.</span></span>

<span data-ttu-id="1149f-105">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="1149f-105">You can:</span></span>

* <span data-ttu-id="1149f-106">Hozzon létre programot (trigger) aktiválása, ha leáll a webhelyet, ahol kezelheti az alkalmazás munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="1149f-106">Create logic app workflows that activate (trigger) when a website that you manage goes down.</span></span>
* <span data-ttu-id="1149f-107">A tetszőleges végpontot HTTP protokollt használó kommunikációra a munkafolyamatok kiterjeszti egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="1149f-107">Communicate to any endpoint over HTTP to extend your workflows into other services.</span></span>

<span data-ttu-id="1149f-108">Első lépések egy logikai alkalmazás a HTTP-művelet használatával, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1149f-108">To get started using the HTTP action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-http-trigger"></a><span data-ttu-id="1149f-109">A HTTP-eseményindítóval használata</span><span class="sxs-lookup"><span data-stu-id="1149f-109">Use the HTTP trigger</span></span>
<span data-ttu-id="1149f-110">Egy eseményindító egy eseményt, amely segítségével indítsa el a munkafolyamatot, amely a logikai alkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="1149f-110">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="1149f-111">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1149f-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="1149f-112">Íme egy parancssorozat-példa bemutatja, hogyan állíthatja be a HTTP-eseményindítóval a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="1149f-112">Here’s an example sequence of how to set up the HTTP trigger in the Logic App Designer.</span></span>

1. <span data-ttu-id="1149f-113">Adja hozzá a HTTP-eseményindítóval a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1149f-113">Add the HTTP trigger in your logic app.</span></span>
2. <span data-ttu-id="1149f-114">Töltse ki a HTTP-végpont kívánt kérdezze le a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="1149f-114">Fill in the parameters for the HTTP endpoint that you want to poll.</span></span>
3. <span data-ttu-id="1149f-115">Az ismétlődési időköz az, hogy milyen gyakran le kell kérdeznie.</span><span class="sxs-lookup"><span data-stu-id="1149f-115">Modify the recurrence interval on how frequently it should poll.</span></span>

   <span data-ttu-id="1149f-116">A logikai alkalmazás most már minden egyes ellenőrzése során visszaküldött tartalommal következik be.</span><span class="sxs-lookup"><span data-stu-id="1149f-116">The logic app now fires with any content that is returned during each check.</span></span>

   ![HTTP eseményindító](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a><span data-ttu-id="1149f-118">A HTTP-eseményindítóval működése</span><span class="sxs-lookup"><span data-stu-id="1149f-118">How the HTTP trigger works</span></span>

<span data-ttu-id="1149f-119">A HTTP-eseményindítóval küld egy ismétlődési időköz HTTP-végpont hívásakor.</span><span class="sxs-lookup"><span data-stu-id="1149f-119">The HTTP trigger sends a call to HTTP endpoint on a recurring interval.</span></span> <span data-ttu-id="1149f-120">Alapértelmezés szerint a HTTP-válaszkód, amely kisebb, mint 300 hatására a logikai alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1149f-120">By default, any HTTP response code that is lower than 300 causes a logic app to run.</span></span> <span data-ttu-id="1149f-121">Adja meg, hogy a logikai alkalmazást kell érvényesítést, szerkessze a logikai alkalmazást a kód nézetre, és adja hozzá a HTTP-hívása után kiértékelésére szolgáló feltétel.</span><span class="sxs-lookup"><span data-stu-id="1149f-121">To specify whether the logic app should fire, you can edit the logic app in code view, and add a condition that evaluates after the HTTP call.</span></span> <span data-ttu-id="1149f-122">Íme egy példa egy HTTP-eseményindítóval, amely akkor következik be, ha az eredményül adott állapotkód: nagyobb vagy egyenlő `400`.</span><span class="sxs-lookup"><span data-stu-id="1149f-122">Here's an example of an HTTP trigger that fires when the returned status code is greater than or equal to `400`.</span></span>

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

<span data-ttu-id="1149f-123">Részletes információ a HTTP-trigger paraméterek érhetők el a [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span><span class="sxs-lookup"><span data-stu-id="1149f-123">Full details about the HTTP trigger parameters are available on [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span></span>

## <a name="use-the-http-action"></a><span data-ttu-id="1149f-124">Használja a HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="1149f-124">Use the HTTP action</span></span>

<span data-ttu-id="1149f-125">Egy művelet során, amely a logikai alkalmazás definiált munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="1149f-125">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> 
<span data-ttu-id="1149f-126">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1149f-126">[Learn more about actions](connectors-overview.md).</span></span>

1. <span data-ttu-id="1149f-127">Válasszon **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1149f-127">Choose **New Step** > **Add an action**.</span></span>
3. <span data-ttu-id="1149f-128">A művelet a keresőmezőbe írja be **http** a HTTP-műveletek listáját.</span><span class="sxs-lookup"><span data-stu-id="1149f-128">In the action search box, type **http** to list the HTTP actions.</span></span>
   
    ![Válassza ki a HTTP-művelet](./media/connectors-native-http/using-action-1.png)

4. <span data-ttu-id="1149f-130">Adja hozzá a HTTP-hívás a szükséges paramétereket.</span><span class="sxs-lookup"><span data-stu-id="1149f-130">Add any required parameters for the HTTP call.</span></span>
   
    ![Fejezze be a HTTP-művelet](./media/connectors-native-http/using-action-2.png)

5. <span data-ttu-id="1149f-132">A designer eszköztáron kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1149f-132">On the designer toolbar, click **Save**.</span></span> <span data-ttu-id="1149f-133">A Logic Apps alkalmazást mentése és egy időben (aktivált) közzététele.</span><span class="sxs-lookup"><span data-stu-id="1149f-133">Your logic app is saved and published (activated) at the same time.</span></span>

## <a name="http-trigger"></a><span data-ttu-id="1149f-134">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="1149f-134">HTTP trigger</span></span>
<span data-ttu-id="1149f-135">Az alábbiak az eseményindító, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="1149f-135">Here are the details for the trigger that this connector supports.</span></span> <span data-ttu-id="1149f-136">A HTTP-összekötő egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="1149f-136">The HTTP connector has one trigger.</span></span>

| <span data-ttu-id="1149f-137">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="1149f-137">Trigger</span></span> | <span data-ttu-id="1149f-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-138">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1149f-139">HTTP</span><span class="sxs-lookup"><span data-stu-id="1149f-139">HTTP</span></span> |<span data-ttu-id="1149f-140">Egy HTTP-hívást, és a válasz tartalmat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1149f-140">Makes an HTTP call and returns the response content.</span></span> |

## <a name="http-action"></a><span data-ttu-id="1149f-141">HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="1149f-141">HTTP action</span></span>
<span data-ttu-id="1149f-142">Az alábbiak a művelet, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="1149f-142">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="1149f-143">A HTTP-összekötő lehetséges egy-egy művelettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1149f-143">The HTTP connector has one possible action.</span></span>

| <span data-ttu-id="1149f-144">Műveletek</span><span class="sxs-lookup"><span data-stu-id="1149f-144">Action</span></span> | <span data-ttu-id="1149f-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-145">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1149f-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="1149f-146">HTTP</span></span> |<span data-ttu-id="1149f-147">Egy HTTP-hívást, és a válasz tartalmat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1149f-147">Makes an HTTP call and returns the response content.</span></span> |

## <a name="http-details"></a><span data-ttu-id="1149f-148">HTTP-részletek</span><span class="sxs-lookup"><span data-stu-id="1149f-148">HTTP details</span></span>
<span data-ttu-id="1149f-149">A következő táblázatok ismertetik, hogy a művelet és a megfelelő kimeneti részletek művelettel társított szükséges és választható bemeneti mező.</span><span class="sxs-lookup"><span data-stu-id="1149f-149">The following tables describe the required and optional input fields for the action and the corresponding output details that are associated with using the action.</span></span>

#### <a name="http-request"></a><span data-ttu-id="1149f-150">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="1149f-150">HTTP request</span></span>
<span data-ttu-id="1149f-151">A művelet, amely a kimenő HTTP-kérelem a beviteli mezők a következők:</span><span class="sxs-lookup"><span data-stu-id="1149f-151">The following are input fields for the action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="1149f-152">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="1149f-152">A * means that it is a required field.</span></span>

| <span data-ttu-id="1149f-153">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="1149f-153">Display name</span></span> | <span data-ttu-id="1149f-154">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1149f-154">Property name</span></span> | <span data-ttu-id="1149f-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1149f-156">Módszer *</span><span class="sxs-lookup"><span data-stu-id="1149f-156">Method*</span></span> |<span data-ttu-id="1149f-157">Módszer</span><span class="sxs-lookup"><span data-stu-id="1149f-157">method</span></span> |<span data-ttu-id="1149f-158">A HTTP-műveletet használata</span><span class="sxs-lookup"><span data-stu-id="1149f-158">The HTTP verb to use</span></span> |
| <span data-ttu-id="1149f-159">URI *</span><span class="sxs-lookup"><span data-stu-id="1149f-159">URI*</span></span> |<span data-ttu-id="1149f-160">URI</span><span class="sxs-lookup"><span data-stu-id="1149f-160">uri</span></span> |<span data-ttu-id="1149f-161">A HTTP-kérelem URI-JÁNAK</span><span class="sxs-lookup"><span data-stu-id="1149f-161">The URI for the HTTP request</span></span> |
| <span data-ttu-id="1149f-162">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1149f-162">Headers</span></span> |<span data-ttu-id="1149f-163">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1149f-163">headers</span></span> |<span data-ttu-id="1149f-164">A HTTP-fejlécek felvenni egy JSON-objektum</span><span class="sxs-lookup"><span data-stu-id="1149f-164">A JSON object of HTTP headers to include</span></span> |
| <span data-ttu-id="1149f-165">Törzs</span><span class="sxs-lookup"><span data-stu-id="1149f-165">Body</span></span> |<span data-ttu-id="1149f-166">Törzs</span><span class="sxs-lookup"><span data-stu-id="1149f-166">body</span></span> |<span data-ttu-id="1149f-167">A HTTP-kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="1149f-167">The HTTP request body</span></span> |
| <span data-ttu-id="1149f-168">Authentication</span><span class="sxs-lookup"><span data-stu-id="1149f-168">Authentication</span></span> |<span data-ttu-id="1149f-169">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-169">authentication</span></span> |<span data-ttu-id="1149f-170">A részletek a [hitelesítési](#authentication) szakasz</span><span class="sxs-lookup"><span data-stu-id="1149f-170">Details in the [Authentication](#authentication) section</span></span> |

<br>

#### <a name="output-details"></a><span data-ttu-id="1149f-171">Kimeneti részletei</span><span class="sxs-lookup"><span data-stu-id="1149f-171">Output details</span></span>
<span data-ttu-id="1149f-172">A HTTP-válasz kimeneti részleteit az alábbiakban.</span><span class="sxs-lookup"><span data-stu-id="1149f-172">The following are output details for the HTTP response.</span></span>

| <span data-ttu-id="1149f-173">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1149f-173">Property name</span></span> | <span data-ttu-id="1149f-174">Adattípus</span><span class="sxs-lookup"><span data-stu-id="1149f-174">Data type</span></span> | <span data-ttu-id="1149f-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-175">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1149f-176">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="1149f-176">Headers</span></span> |<span data-ttu-id="1149f-177">Objektum</span><span class="sxs-lookup"><span data-stu-id="1149f-177">object</span></span> |<span data-ttu-id="1149f-178">Válaszfejlécek</span><span class="sxs-lookup"><span data-stu-id="1149f-178">Response headers</span></span> |
| <span data-ttu-id="1149f-179">Törzs</span><span class="sxs-lookup"><span data-stu-id="1149f-179">Body</span></span> |<span data-ttu-id="1149f-180">Objektum</span><span class="sxs-lookup"><span data-stu-id="1149f-180">object</span></span> |<span data-ttu-id="1149f-181">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="1149f-181">Response object</span></span> |
| <span data-ttu-id="1149f-182">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="1149f-182">Status Code</span></span> |<span data-ttu-id="1149f-183">int</span><span class="sxs-lookup"><span data-stu-id="1149f-183">int</span></span> |<span data-ttu-id="1149f-184">HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="1149f-184">HTTP status code</span></span> |

## <a name="authentication"></a><span data-ttu-id="1149f-185">Authentication</span><span class="sxs-lookup"><span data-stu-id="1149f-185">Authentication</span></span>
<span data-ttu-id="1149f-186">A Logic Apps szolgáltatás lehetővé teszi a különböző típusú HTTP-végpontokról hitelesítést használja.</span><span class="sxs-lookup"><span data-stu-id="1149f-186">The Logic Apps feature allows you to use different types of authentication against HTTP endpoints.</span></span> <span data-ttu-id="1149f-187">Használhatja a hitelesítés a **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, és  **[HTTP Webhook](connectors-native-webhook.md)**  összekötők.</span><span class="sxs-lookup"><span data-stu-id="1149f-187">You can use this authentication with the **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)**, and **[HTTP Webhook](connectors-native-webhook.md)** connectors.</span></span> <span data-ttu-id="1149f-188">A következő hitelesítési típusokat, amelyek konfigurálhatók:</span><span class="sxs-lookup"><span data-stu-id="1149f-188">The following types of authentication are configurable:</span></span>

* [<span data-ttu-id="1149f-189">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-189">Basic authentication</span></span>](#basic-authentication)
* [<span data-ttu-id="1149f-190">Ügyféltanúsítvány-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-190">Client certificate authentication</span></span>](#client-certificate-authentication)
* [<span data-ttu-id="1149f-191">Az Azure Active Directory (Azure AD) OAuth-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-191">Azure Active Directory (Azure AD) OAuth authentication</span></span>](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a><span data-ttu-id="1149f-192">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-192">Basic authentication</span></span>

<span data-ttu-id="1149f-193">A következő hitelesítési objektumot az alapszintű hitelesítés van szükség.</span><span class="sxs-lookup"><span data-stu-id="1149f-193">The following authentication object is needed for basic authentication.</span></span>
<span data-ttu-id="1149f-194">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="1149f-194">A * means that it is a required field.</span></span>

| <span data-ttu-id="1149f-195">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1149f-195">Property name</span></span> | <span data-ttu-id="1149f-196">Adattípus</span><span class="sxs-lookup"><span data-stu-id="1149f-196">Data type</span></span> | <span data-ttu-id="1149f-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1149f-198">Típus *</span><span class="sxs-lookup"><span data-stu-id="1149f-198">Type*</span></span> |<span data-ttu-id="1149f-199">type</span><span class="sxs-lookup"><span data-stu-id="1149f-199">type</span></span> |<span data-ttu-id="1149f-200">Hitelesítés típusa (lehet `Basic` az egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="1149f-200">Type of authentication (must be `Basic` for basic authentication)</span></span> |
| <span data-ttu-id="1149f-201">Felhasználónév *</span><span class="sxs-lookup"><span data-stu-id="1149f-201">Username*</span></span> |<span data-ttu-id="1149f-202">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="1149f-202">username</span></span> |<span data-ttu-id="1149f-203">Felhasználónév hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="1149f-203">User name to authenticate</span></span> |
| <span data-ttu-id="1149f-204">Jelszó *</span><span class="sxs-lookup"><span data-stu-id="1149f-204">Password*</span></span> |<span data-ttu-id="1149f-205">jelszó</span><span class="sxs-lookup"><span data-stu-id="1149f-205">password</span></span> |<span data-ttu-id="1149f-206">Jelszó a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="1149f-206">Password to authenticate</span></span> |

> [!TIP]
> <span data-ttu-id="1149f-207">Ha szeretné használni, melyek nem kérhető le a definíció, használja a jelszó egy `securestring` paraméter és a `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="1149f-207">If you want to use a password that cannot be retrieved from the definition, use a `securestring` parameter and the `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="1149f-208">Példa:</span><span class="sxs-lookup"><span data-stu-id="1149f-208">For example:</span></span>

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a><span data-ttu-id="1149f-209">Ügyféltanúsítvány-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-209">Client certificate authentication</span></span>

<span data-ttu-id="1149f-210">A következő hitelesítési objektum ügyféltanúsítvány-alapú hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="1149f-210">The following authentication object is needed for client certificate authentication.</span></span> <span data-ttu-id="1149f-211">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="1149f-211">A * means that it is a required field.</span></span>

| <span data-ttu-id="1149f-212">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1149f-212">Property name</span></span> | <span data-ttu-id="1149f-213">Adattípus</span><span class="sxs-lookup"><span data-stu-id="1149f-213">Data type</span></span> | <span data-ttu-id="1149f-214">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-214">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1149f-215">Típus *</span><span class="sxs-lookup"><span data-stu-id="1149f-215">Type*</span></span> |<span data-ttu-id="1149f-216">type</span><span class="sxs-lookup"><span data-stu-id="1149f-216">type</span></span> |<span data-ttu-id="1149f-217">A hitelesítés típusát (kell `ClientCertificate` az ügyfél SSL-tanúsítványok)</span><span class="sxs-lookup"><span data-stu-id="1149f-217">The type of authentication (must be `ClientCertificate` for SSL client certificates)</span></span> |
| <span data-ttu-id="1149f-218">PFX *</span><span class="sxs-lookup"><span data-stu-id="1149f-218">PFX*</span></span> |<span data-ttu-id="1149f-219">PFX</span><span class="sxs-lookup"><span data-stu-id="1149f-219">pfx</span></span> |<span data-ttu-id="1149f-220">A Base64-kódolású tartalmak a személyes információcseréhez kapcsolódó (PFX) fájl</span><span class="sxs-lookup"><span data-stu-id="1149f-220">The Base64-encoded contents of the Personal Information Exchange (PFX) file</span></span> |
| <span data-ttu-id="1149f-221">Jelszó *</span><span class="sxs-lookup"><span data-stu-id="1149f-221">Password*</span></span> |<span data-ttu-id="1149f-222">jelszó</span><span class="sxs-lookup"><span data-stu-id="1149f-222">password</span></span> |<span data-ttu-id="1149f-223">A jelszó megadásával érheti el a PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="1149f-223">The password to access the PFX file</span></span> |

> [!TIP]
> <span data-ttu-id="1149f-224">Egy paramétert, amely olvashatók a logic app mentését követően a definíció nem lesz használható a `securestring` paraméter és a `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="1149f-224">To use a parameter that won't be readable in the definition after saving the logic app, you can use a `securestring` parameter and the `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="1149f-225">Példa:</span><span class="sxs-lookup"><span data-stu-id="1149f-225">For example:</span></span>

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a><span data-ttu-id="1149f-226">Az Azure AD OAuth-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1149f-226">Azure AD OAuth authentication</span></span>
<span data-ttu-id="1149f-227">A következő hitelesítési objektum Azure AD OAuth-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="1149f-227">The following authentication object is needed for Azure AD OAuth authentication.</span></span> <span data-ttu-id="1149f-228">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="1149f-228">A * means that it is a required field.</span></span>

| <span data-ttu-id="1149f-229">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1149f-229">Property name</span></span> | <span data-ttu-id="1149f-230">Adattípus</span><span class="sxs-lookup"><span data-stu-id="1149f-230">Data type</span></span> | <span data-ttu-id="1149f-231">Leírás</span><span class="sxs-lookup"><span data-stu-id="1149f-231">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1149f-232">Típus *</span><span class="sxs-lookup"><span data-stu-id="1149f-232">Type*</span></span> |<span data-ttu-id="1149f-233">type</span><span class="sxs-lookup"><span data-stu-id="1149f-233">type</span></span> |<span data-ttu-id="1149f-234">A hitelesítés típusát (kell `ActiveDirectoryOAuth` az Azure AD OAuth)</span><span class="sxs-lookup"><span data-stu-id="1149f-234">The type of authentication (must be `ActiveDirectoryOAuth` for Azure AD OAuth)</span></span> |
| <span data-ttu-id="1149f-235">Bérlői *</span><span class="sxs-lookup"><span data-stu-id="1149f-235">Tenant*</span></span> |<span data-ttu-id="1149f-236">Bérlői</span><span class="sxs-lookup"><span data-stu-id="1149f-236">tenant</span></span> |<span data-ttu-id="1149f-237">A bérlő azonosítóját az Azure AD-bérlő</span><span class="sxs-lookup"><span data-stu-id="1149f-237">The tenant identifier for the Azure AD tenant</span></span> |
| <span data-ttu-id="1149f-238">A célközönség *</span><span class="sxs-lookup"><span data-stu-id="1149f-238">Audience*</span></span> |<span data-ttu-id="1149f-239">Célközönség</span><span class="sxs-lookup"><span data-stu-id="1149f-239">audience</span></span> |<span data-ttu-id="1149f-240">Az engedélyezési használatára a kért erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1149f-240">The resource you are requesting authorization to use.</span></span> <span data-ttu-id="1149f-241">Például:`https://management.core.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="1149f-241">For example: `https://management.core.windows.net/`</span></span> |
| <span data-ttu-id="1149f-242">Ügyfél-azonosító *</span><span class="sxs-lookup"><span data-stu-id="1149f-242">Client ID*</span></span> |<span data-ttu-id="1149f-243">clientId</span><span class="sxs-lookup"><span data-stu-id="1149f-243">clientId</span></span> |<span data-ttu-id="1149f-244">Az ügyfél az Azure AD-alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="1149f-244">The client identifier for the Azure AD application</span></span> |
| <span data-ttu-id="1149f-245">Titkos kulcs *</span><span class="sxs-lookup"><span data-stu-id="1149f-245">Secret*</span></span> |<span data-ttu-id="1149f-246">titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="1149f-246">secret</span></span> |<span data-ttu-id="1149f-247">A titkos kulcsot a jogkivonatot kérő ügyfél</span><span class="sxs-lookup"><span data-stu-id="1149f-247">The secret of the client that is requesting the token</span></span> |

> [!TIP]
> <span data-ttu-id="1149f-248">Használhatja a `securestring` paraméter és a `@parameters()` [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs) egy paraméterrel, amely a kezdeti mentést követően a definíció nem olvasható.</span><span class="sxs-lookup"><span data-stu-id="1149f-248">You can use a `securestring` parameter and the `@parameters()` [workflow definition function](http://aka.ms/logicappdocs) to use a parameter that won't be readable in the definition after saving.</span></span>
> 
> 

<span data-ttu-id="1149f-249">Példa:</span><span class="sxs-lookup"><span data-stu-id="1149f-249">For example:</span></span>

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a><span data-ttu-id="1149f-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1149f-250">Next steps</span></span>
<span data-ttu-id="1149f-251">Most, próbálja ki a platformot és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1149f-251">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="1149f-252">Megismerheti az egyéb rendelkezésre álló összekötők Logic Apps, ha megnézi a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1149f-252">You can explore the other available connectors in Logic Apps by looking at our [APIs list](apis-list.md).</span></span>

