---
title: "a tetszőleges végpontot HTTP - Azure Logic Apps aaaCommunicate |} Microsoft Docs"
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
ms.openlocfilehash: 9793601839437a2b880bdb81e15881270cacc963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-http-action"></a><span data-ttu-id="86d80-103">Ismerkedés a hello HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="86d80-103">Get started with hello HTTP action</span></span>

<span data-ttu-id="86d80-104">Hello HTTP-művelet, a munkafolyamatok kiterjesztheti a szervezet és tooany végpont HTTP protokollt használó kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="86d80-104">With hello HTTP action, you can extend workflows for your organization and communicate tooany endpoint over HTTP.</span></span>

<span data-ttu-id="86d80-105">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="86d80-105">You can:</span></span>

* <span data-ttu-id="86d80-106">Hozzon létre programot (trigger) aktiválása, ha leáll a webhelyet, ahol kezelheti az alkalmazás munkafolyamatok.</span><span class="sxs-lookup"><span data-stu-id="86d80-106">Create logic app workflows that activate (trigger) when a website that you manage goes down.</span></span>
* <span data-ttu-id="86d80-107">Tooany végpont protokollt használó kommunikációra HTTP tooextend a munkafolyamatokat az más szolgáltatásaiba.</span><span class="sxs-lookup"><span data-stu-id="86d80-107">Communicate tooany endpoint over HTTP tooextend your workflows into other services.</span></span>

<span data-ttu-id="86d80-108">tooget használatának hello HTTP-művelet a logikai alkalmazás, lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="86d80-108">tooget started using hello HTTP action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-http-trigger"></a><span data-ttu-id="86d80-109">Hello HTTP-eseményindító használata</span><span class="sxs-lookup"><span data-stu-id="86d80-109">Use hello HTTP trigger</span></span>
<span data-ttu-id="86d80-110">Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény.</span><span class="sxs-lookup"><span data-stu-id="86d80-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="86d80-111">[További tudnivalók az eseményindítók](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86d80-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="86d80-112">Íme egy parancssorozat-példa hogyan tooset hello HTTP be trigger hello Logic App Tervező.</span><span class="sxs-lookup"><span data-stu-id="86d80-112">Here’s an example sequence of how tooset up hello HTTP trigger in hello Logic App Designer.</span></span>

1. <span data-ttu-id="86d80-113">Adja hozzá a logikai alkalmazás hello HTTP-eseményindítóval.</span><span class="sxs-lookup"><span data-stu-id="86d80-113">Add hello HTTP trigger in your logic app.</span></span>
2. <span data-ttu-id="86d80-114">Töltse ki a megjeleníteni kívánt toopoll hello HTTP-végpont hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="86d80-114">Fill in hello parameters for hello HTTP endpoint that you want toopoll.</span></span>
3. <span data-ttu-id="86d80-115">Milyen gyakran le kell kérdeznie a hello ismétlődési időköz módosítása.</span><span class="sxs-lookup"><span data-stu-id="86d80-115">Modify hello recurrence interval on how frequently it should poll.</span></span>

   <span data-ttu-id="86d80-116">hello logikai alkalmazás most már minden egyes ellenőrzése során visszaküldött tartalommal következik be.</span><span class="sxs-lookup"><span data-stu-id="86d80-116">hello logic app now fires with any content that is returned during each check.</span></span>

   ![HTTP eseményindító](./media/connectors-native-http/using-trigger.png)

### <a name="how-hello-http-trigger-works"></a><span data-ttu-id="86d80-118">Hello HTTP-eseményindítóval működése</span><span class="sxs-lookup"><span data-stu-id="86d80-118">How hello HTTP trigger works</span></span>

<span data-ttu-id="86d80-119">hello HTTP-eseményindítóval küld egy hívás tooHTTP végpont ismétlődési időköz.</span><span class="sxs-lookup"><span data-stu-id="86d80-119">hello HTTP trigger sends a call tooHTTP endpoint on a recurring interval.</span></span> <span data-ttu-id="86d80-120">Alapértelmezés szerint a HTTP-válaszkód, amely kisebb, mint 300 hatására a logic app toorun.</span><span class="sxs-lookup"><span data-stu-id="86d80-120">By default, any HTTP response code that is lower than 300 causes a logic app toorun.</span></span> <span data-ttu-id="86d80-121">toospecify hello logikai alkalmazás érvényesítést kell, hogy szerkesztheti hello logikai alkalmazást a kód nézetre, és adja hozzá a HTTP-hívása után hello kiértékelésére szolgáló feltétel.</span><span class="sxs-lookup"><span data-stu-id="86d80-121">toospecify whether hello logic app should fire, you can edit hello logic app in code view, and add a condition that evaluates after hello HTTP call.</span></span> <span data-ttu-id="86d80-122">Íme egy példa egy HTTP-eseményindítóval, amely akkor következik be, ha a hello adja vissza. állapotkód: nagyobb vagy egyenlő túl`400`.</span><span class="sxs-lookup"><span data-stu-id="86d80-122">Here's an example of an HTTP trigger that fires when hello returned status code is greater than or equal too`400`.</span></span>

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

<span data-ttu-id="86d80-123">Részletes információ hello HTTP-trigger paraméterek érhetők el a [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span><span class="sxs-lookup"><span data-stu-id="86d80-123">Full details about hello HTTP trigger parameters are available on [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).</span></span>

## <a name="use-hello-http-action"></a><span data-ttu-id="86d80-124">Hello HTTP művelettel</span><span class="sxs-lookup"><span data-stu-id="86d80-124">Use hello HTTP action</span></span>

<span data-ttu-id="86d80-125">Egy művelet során, amely logikai alkalmazás definiált hello munkafolyamat végzi.</span><span class="sxs-lookup"><span data-stu-id="86d80-125">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> 
<span data-ttu-id="86d80-126">[További információ a műveletek](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="86d80-126">[Learn more about actions](connectors-overview.md).</span></span>

1. <span data-ttu-id="86d80-127">Válasszon **új lépés** > **művelet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="86d80-127">Choose **New Step** > **Add an action**.</span></span>
3. <span data-ttu-id="86d80-128">Hello művelet keresési mezőbe, írja be a **http** toolist hello HTTP-műveletek.</span><span class="sxs-lookup"><span data-stu-id="86d80-128">In hello action search box, type **http** toolist hello HTTP actions.</span></span>
   
    ![Válassza ki a hello HTTP-művelet](./media/connectors-native-http/using-action-1.png)

4. <span data-ttu-id="86d80-130">Adja hozzá a szükséges paramétereket hello HTTP hívásához.</span><span class="sxs-lookup"><span data-stu-id="86d80-130">Add any required parameters for hello HTTP call.</span></span>
   
    ![Teljes hello HTTP-művelet](./media/connectors-native-http/using-action-2.png)

5. <span data-ttu-id="86d80-132">A hello designer eszköztáron kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="86d80-132">On hello designer toolbar, click **Save**.</span></span> <span data-ttu-id="86d80-133">A Logic Apps alkalmazást mentése és hello (aktivált) közzétett ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="86d80-133">Your logic app is saved and published (activated) at hello same time.</span></span>

## <a name="http-trigger"></a><span data-ttu-id="86d80-134">HTTP eseményindító</span><span class="sxs-lookup"><span data-stu-id="86d80-134">HTTP trigger</span></span>
<span data-ttu-id="86d80-135">Az alábbiakban hello részletek hello eseményindító, amely támogatja ezt az összekötőt.</span><span class="sxs-lookup"><span data-stu-id="86d80-135">Here are hello details for hello trigger that this connector supports.</span></span> <span data-ttu-id="86d80-136">hello HTTP összekötő egy eseményindító tartozik.</span><span class="sxs-lookup"><span data-stu-id="86d80-136">hello HTTP connector has one trigger.</span></span>

| <span data-ttu-id="86d80-137">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="86d80-137">Trigger</span></span> | <span data-ttu-id="86d80-138">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-138">Description</span></span> |
| --- | --- |
| <span data-ttu-id="86d80-139">HTTP</span><span class="sxs-lookup"><span data-stu-id="86d80-139">HTTP</span></span> |<span data-ttu-id="86d80-140">Egy HTTP-hívást, és hello válasz tartalmat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="86d80-140">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-action"></a><span data-ttu-id="86d80-141">HTTP-művelet</span><span class="sxs-lookup"><span data-stu-id="86d80-141">HTTP action</span></span>
<span data-ttu-id="86d80-142">Az alábbiakban hello részleteit, amely támogatja ezt az összekötőt hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="86d80-142">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="86d80-143">hello HTTP összekötő lehetséges egy-egy művelettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="86d80-143">hello HTTP connector has one possible action.</span></span>

| <span data-ttu-id="86d80-144">Műveletek</span><span class="sxs-lookup"><span data-stu-id="86d80-144">Action</span></span> | <span data-ttu-id="86d80-145">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-145">Description</span></span> |
| --- | --- |
| <span data-ttu-id="86d80-146">HTTP</span><span class="sxs-lookup"><span data-stu-id="86d80-146">HTTP</span></span> |<span data-ttu-id="86d80-147">Egy HTTP-hívást, és hello válasz tartalmat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="86d80-147">Makes an HTTP call and returns hello response content.</span></span> |

## <a name="http-details"></a><span data-ttu-id="86d80-148">HTTP-részletek</span><span class="sxs-lookup"><span data-stu-id="86d80-148">HTTP details</span></span>
<span data-ttu-id="86d80-149">hello alábbi táblázatok bemutatják az hello szükséges és választható beviteli mezők hello művelet és hello megfelelő kimeneti részletekért társított hello műveletével.</span><span class="sxs-lookup"><span data-stu-id="86d80-149">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

#### <a name="http-request"></a><span data-ttu-id="86d80-150">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="86d80-150">HTTP request</span></span>
<span data-ttu-id="86d80-151">Az alábbiakban hello hello művelet, amely a kimenő HTTP-kérelem a beviteli mezők.</span><span class="sxs-lookup"><span data-stu-id="86d80-151">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="86d80-152">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="86d80-152">A * means that it is a required field.</span></span>

| <span data-ttu-id="86d80-153">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="86d80-153">Display name</span></span> | <span data-ttu-id="86d80-154">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="86d80-154">Property name</span></span> | <span data-ttu-id="86d80-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-155">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="86d80-156">Módszer *</span><span class="sxs-lookup"><span data-stu-id="86d80-156">Method*</span></span> |<span data-ttu-id="86d80-157">Módszer</span><span class="sxs-lookup"><span data-stu-id="86d80-157">method</span></span> |<span data-ttu-id="86d80-158">hello HTTP-művelet toouse</span><span class="sxs-lookup"><span data-stu-id="86d80-158">hello HTTP verb toouse</span></span> |
| <span data-ttu-id="86d80-159">URI *</span><span class="sxs-lookup"><span data-stu-id="86d80-159">URI*</span></span> |<span data-ttu-id="86d80-160">URI</span><span class="sxs-lookup"><span data-stu-id="86d80-160">uri</span></span> |<span data-ttu-id="86d80-161">hello URI hello HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="86d80-161">hello URI for hello HTTP request</span></span> |
| <span data-ttu-id="86d80-162">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="86d80-162">Headers</span></span> |<span data-ttu-id="86d80-163">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="86d80-163">headers</span></span> |<span data-ttu-id="86d80-164">A HTTP-fejlécek tooinclude egy JSON-objektum</span><span class="sxs-lookup"><span data-stu-id="86d80-164">A JSON object of HTTP headers tooinclude</span></span> |
| <span data-ttu-id="86d80-165">Törzs</span><span class="sxs-lookup"><span data-stu-id="86d80-165">Body</span></span> |<span data-ttu-id="86d80-166">Törzs</span><span class="sxs-lookup"><span data-stu-id="86d80-166">body</span></span> |<span data-ttu-id="86d80-167">hello HTTP-kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="86d80-167">hello HTTP request body</span></span> |
| <span data-ttu-id="86d80-168">Authentication</span><span class="sxs-lookup"><span data-stu-id="86d80-168">Authentication</span></span> |<span data-ttu-id="86d80-169">Hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-169">authentication</span></span> |<span data-ttu-id="86d80-170">Hello részleteket [hitelesítési](#authentication) szakasz</span><span class="sxs-lookup"><span data-stu-id="86d80-170">Details in hello [Authentication](#authentication) section</span></span> |

<br>

#### <a name="output-details"></a><span data-ttu-id="86d80-171">Kimeneti részletei</span><span class="sxs-lookup"><span data-stu-id="86d80-171">Output details</span></span>
<span data-ttu-id="86d80-172">Az alábbiakban hello kimeneti részletes hello HTTP-válasz.</span><span class="sxs-lookup"><span data-stu-id="86d80-172">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="86d80-173">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="86d80-173">Property name</span></span> | <span data-ttu-id="86d80-174">Adattípus</span><span class="sxs-lookup"><span data-stu-id="86d80-174">Data type</span></span> | <span data-ttu-id="86d80-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-175">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="86d80-176">Fejlécek</span><span class="sxs-lookup"><span data-stu-id="86d80-176">Headers</span></span> |<span data-ttu-id="86d80-177">Objektum</span><span class="sxs-lookup"><span data-stu-id="86d80-177">object</span></span> |<span data-ttu-id="86d80-178">Válaszfejlécek</span><span class="sxs-lookup"><span data-stu-id="86d80-178">Response headers</span></span> |
| <span data-ttu-id="86d80-179">Törzs</span><span class="sxs-lookup"><span data-stu-id="86d80-179">Body</span></span> |<span data-ttu-id="86d80-180">Objektum</span><span class="sxs-lookup"><span data-stu-id="86d80-180">object</span></span> |<span data-ttu-id="86d80-181">Válasz objektum</span><span class="sxs-lookup"><span data-stu-id="86d80-181">Response object</span></span> |
| <span data-ttu-id="86d80-182">Állapotkód</span><span class="sxs-lookup"><span data-stu-id="86d80-182">Status Code</span></span> |<span data-ttu-id="86d80-183">int</span><span class="sxs-lookup"><span data-stu-id="86d80-183">int</span></span> |<span data-ttu-id="86d80-184">HTTP-állapotkód:</span><span class="sxs-lookup"><span data-stu-id="86d80-184">HTTP status code</span></span> |

## <a name="authentication"></a><span data-ttu-id="86d80-185">Authentication</span><span class="sxs-lookup"><span data-stu-id="86d80-185">Authentication</span></span>
<span data-ttu-id="86d80-186">hello Logic Apps szolgáltatás lehetővé teszi toouse különböző hitelesítési HTTP-végpontokról ellen.</span><span class="sxs-lookup"><span data-stu-id="86d80-186">hello Logic Apps feature allows you toouse different types of authentication against HTTP endpoints.</span></span> <span data-ttu-id="86d80-187">Ez a hitelesítés használatával hello **HTTP**,  **[HTTP + Swagger](connectors-native-http-swagger.md)**, és  **[HTTP Webhook](connectors-native-webhook.md)**  összekötők.</span><span class="sxs-lookup"><span data-stu-id="86d80-187">You can use this authentication with hello **HTTP**, **[HTTP + Swagger](connectors-native-http-swagger.md)**, and **[HTTP Webhook](connectors-native-webhook.md)** connectors.</span></span> <span data-ttu-id="86d80-188">a következő típusú hitelesítés hello amelyek konfigurálhatók:</span><span class="sxs-lookup"><span data-stu-id="86d80-188">hello following types of authentication are configurable:</span></span>

* [<span data-ttu-id="86d80-189">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-189">Basic authentication</span></span>](#basic-authentication)
* [<span data-ttu-id="86d80-190">Ügyféltanúsítvány-alapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-190">Client certificate authentication</span></span>](#client-certificate-authentication)
* [<span data-ttu-id="86d80-191">Az Azure Active Directory (Azure AD) OAuth-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-191">Azure Active Directory (Azure AD) OAuth authentication</span></span>](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a><span data-ttu-id="86d80-192">Az egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-192">Basic authentication</span></span>

<span data-ttu-id="86d80-193">a következő hitelesítési objektumot hello az egyszerű hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="86d80-193">hello following authentication object is needed for basic authentication.</span></span>
<span data-ttu-id="86d80-194">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="86d80-194">A * means that it is a required field.</span></span>

| <span data-ttu-id="86d80-195">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="86d80-195">Property name</span></span> | <span data-ttu-id="86d80-196">Adattípus</span><span class="sxs-lookup"><span data-stu-id="86d80-196">Data type</span></span> | <span data-ttu-id="86d80-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-197">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="86d80-198">Típus *</span><span class="sxs-lookup"><span data-stu-id="86d80-198">Type*</span></span> |<span data-ttu-id="86d80-199">type</span><span class="sxs-lookup"><span data-stu-id="86d80-199">type</span></span> |<span data-ttu-id="86d80-200">Hitelesítés típusa (lehet `Basic` az egyszerű hitelesítés)</span><span class="sxs-lookup"><span data-stu-id="86d80-200">Type of authentication (must be `Basic` for basic authentication)</span></span> |
| <span data-ttu-id="86d80-201">Felhasználónév *</span><span class="sxs-lookup"><span data-stu-id="86d80-201">Username*</span></span> |<span data-ttu-id="86d80-202">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="86d80-202">username</span></span> |<span data-ttu-id="86d80-203">Felhasználói név tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="86d80-203">User name tooauthenticate</span></span> |
| <span data-ttu-id="86d80-204">Jelszó *</span><span class="sxs-lookup"><span data-stu-id="86d80-204">Password*</span></span> |<span data-ttu-id="86d80-205">jelszó</span><span class="sxs-lookup"><span data-stu-id="86d80-205">password</span></span> |<span data-ttu-id="86d80-206">Jelszó tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="86d80-206">Password tooauthenticate</span></span> |

> [!TIP]
> <span data-ttu-id="86d80-207">Ha azt szeretné, hogy egy jelszót, amely nem kérhető le hello definition toouse, egy `securestring` paraméter és hello `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="86d80-207">If you want toouse a password that cannot be retrieved from hello definition, use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="86d80-208">Példa:</span><span class="sxs-lookup"><span data-stu-id="86d80-208">For example:</span></span>

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a><span data-ttu-id="86d80-209">Ügyféltanúsítvány-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-209">Client certificate authentication</span></span>

<span data-ttu-id="86d80-210">hello következő hitelesítési objektum szükséges ügyféltanúsítvány-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="86d80-210">hello following authentication object is needed for client certificate authentication.</span></span> <span data-ttu-id="86d80-211">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="86d80-211">A * means that it is a required field.</span></span>

| <span data-ttu-id="86d80-212">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="86d80-212">Property name</span></span> | <span data-ttu-id="86d80-213">Adattípus</span><span class="sxs-lookup"><span data-stu-id="86d80-213">Data type</span></span> | <span data-ttu-id="86d80-214">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-214">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="86d80-215">Típus *</span><span class="sxs-lookup"><span data-stu-id="86d80-215">Type*</span></span> |<span data-ttu-id="86d80-216">type</span><span class="sxs-lookup"><span data-stu-id="86d80-216">type</span></span> |<span data-ttu-id="86d80-217">hello típusú hitelesítés (kell `ClientCertificate` az ügyfél SSL-tanúsítványok)</span><span class="sxs-lookup"><span data-stu-id="86d80-217">hello type of authentication (must be `ClientCertificate` for SSL client certificates)</span></span> |
| <span data-ttu-id="86d80-218">PFX *</span><span class="sxs-lookup"><span data-stu-id="86d80-218">PFX*</span></span> |<span data-ttu-id="86d80-219">PFX</span><span class="sxs-lookup"><span data-stu-id="86d80-219">pfx</span></span> |<span data-ttu-id="86d80-220">hello Base64-kódolású hello személyes információcseréhez kapcsolódó (PFX) fájl tartalma</span><span class="sxs-lookup"><span data-stu-id="86d80-220">hello Base64-encoded contents of hello Personal Information Exchange (PFX) file</span></span> |
| <span data-ttu-id="86d80-221">Jelszó *</span><span class="sxs-lookup"><span data-stu-id="86d80-221">Password*</span></span> |<span data-ttu-id="86d80-222">jelszó</span><span class="sxs-lookup"><span data-stu-id="86d80-222">password</span></span> |<span data-ttu-id="86d80-223">hello jelszó tooaccess hello PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="86d80-223">hello password tooaccess hello PFX file</span></span> |

> [!TIP]
> <span data-ttu-id="86d80-224">egy paraméter, amely nem lesz olvasható hello definícióban hello logikai alkalmazás mentését követően toouse, használhatja a `securestring` paraméter és hello `@parameters()`  
>  [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs).</span><span class="sxs-lookup"><span data-stu-id="86d80-224">toouse a parameter that won't be readable in hello definition after saving hello logic app, you can use a `securestring` parameter and hello `@parameters()` 
> [workflow definition function](http://aka.ms/logicappdocs).</span></span>

<span data-ttu-id="86d80-225">Példa:</span><span class="sxs-lookup"><span data-stu-id="86d80-225">For example:</span></span>

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a><span data-ttu-id="86d80-226">Az Azure AD OAuth-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="86d80-226">Azure AD OAuth authentication</span></span>
<span data-ttu-id="86d80-227">a következő hitelesítési objektumot hello Azure AD OAuth-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="86d80-227">hello following authentication object is needed for Azure AD OAuth authentication.</span></span> <span data-ttu-id="86d80-228">A * azt jelenti, hogy mezőt kötelező kitölteni.</span><span class="sxs-lookup"><span data-stu-id="86d80-228">A * means that it is a required field.</span></span>

| <span data-ttu-id="86d80-229">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="86d80-229">Property name</span></span> | <span data-ttu-id="86d80-230">Adattípus</span><span class="sxs-lookup"><span data-stu-id="86d80-230">Data type</span></span> | <span data-ttu-id="86d80-231">Leírás</span><span class="sxs-lookup"><span data-stu-id="86d80-231">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="86d80-232">Típus *</span><span class="sxs-lookup"><span data-stu-id="86d80-232">Type*</span></span> |<span data-ttu-id="86d80-233">type</span><span class="sxs-lookup"><span data-stu-id="86d80-233">type</span></span> |<span data-ttu-id="86d80-234">hello típusú hitelesítés (kell `ActiveDirectoryOAuth` az Azure AD OAuth)</span><span class="sxs-lookup"><span data-stu-id="86d80-234">hello type of authentication (must be `ActiveDirectoryOAuth` for Azure AD OAuth)</span></span> |
| <span data-ttu-id="86d80-235">Bérlői *</span><span class="sxs-lookup"><span data-stu-id="86d80-235">Tenant*</span></span> |<span data-ttu-id="86d80-236">Bérlői</span><span class="sxs-lookup"><span data-stu-id="86d80-236">tenant</span></span> |<span data-ttu-id="86d80-237">hello bérlői azonosító hello Azure AD-bérlő számára</span><span class="sxs-lookup"><span data-stu-id="86d80-237">hello tenant identifier for hello Azure AD tenant</span></span> |
| <span data-ttu-id="86d80-238">A célközönség *</span><span class="sxs-lookup"><span data-stu-id="86d80-238">Audience*</span></span> |<span data-ttu-id="86d80-239">Célközönség</span><span class="sxs-lookup"><span data-stu-id="86d80-239">audience</span></span> |<span data-ttu-id="86d80-240">a kért engedélyezési toouse hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="86d80-240">hello resource you are requesting authorization toouse.</span></span> <span data-ttu-id="86d80-241">Például:`https://management.core.windows.net/`</span><span class="sxs-lookup"><span data-stu-id="86d80-241">For example: `https://management.core.windows.net/`</span></span> |
| <span data-ttu-id="86d80-242">Ügyfél-azonosító *</span><span class="sxs-lookup"><span data-stu-id="86d80-242">Client ID*</span></span> |<span data-ttu-id="86d80-243">clientId</span><span class="sxs-lookup"><span data-stu-id="86d80-243">clientId</span></span> |<span data-ttu-id="86d80-244">hello hello Azure AD alkalmazás ügyfél-azonosítója</span><span class="sxs-lookup"><span data-stu-id="86d80-244">hello client identifier for hello Azure AD application</span></span> |
| <span data-ttu-id="86d80-245">Titkos kulcs *</span><span class="sxs-lookup"><span data-stu-id="86d80-245">Secret*</span></span> |<span data-ttu-id="86d80-246">titkos kulcs</span><span class="sxs-lookup"><span data-stu-id="86d80-246">secret</span></span> |<span data-ttu-id="86d80-247">hello titkos hello jogkivonatot kérő hello ügyfél</span><span class="sxs-lookup"><span data-stu-id="86d80-247">hello secret of hello client that is requesting hello token</span></span> |

> [!TIP]
> <span data-ttu-id="86d80-248">Használhatja a `securestring` paraméter és hello `@parameters()` [munkafolyamat-definíció funkció](http://aka.ms/logicappdocs) toouse egy paraméter, amely nem lesz olvasható, a mentés után hello-definícióban.</span><span class="sxs-lookup"><span data-stu-id="86d80-248">You can use a `securestring` parameter and hello `@parameters()` [workflow definition function](http://aka.ms/logicappdocs) toouse a parameter that won't be readable in hello definition after saving.</span></span>
> 
> 

<span data-ttu-id="86d80-249">Példa:</span><span class="sxs-lookup"><span data-stu-id="86d80-249">For example:</span></span>

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a><span data-ttu-id="86d80-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86d80-250">Next steps</span></span>
<span data-ttu-id="86d80-251">Most, próbálja ki hello platform és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="86d80-251">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="86d80-252">Ismerje meg a más rendelkezésre álló összekötők Logic Apps hello megnézzük a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="86d80-252">You can explore hello other available connectors in Logic Apps by looking at our [APIs list](apis-list.md).</span></span>

