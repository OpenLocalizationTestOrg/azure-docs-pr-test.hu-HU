---
title: "Műveletek felvétele az Azure API Management API |} Microsoft Docs"
description: "Útmutató tevékenységek hozzáadása az Azure API Management API."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 105fc51c2d1152a40a5757985da47330e0b7b8cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a><span data-ttu-id="d69b4-103">Műveletek hozzáadása egy API-t az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d69b4-103">How to add operations to an API in Azure API Management</span></span>
<span data-ttu-id="d69b4-104">Egy API-t az API Management használata előtt hozzá kell adni műveletek.</span><span class="sxs-lookup"><span data-stu-id="d69b4-104">Before an API in API Management can be used, operations must be added.</span></span> <span data-ttu-id="d69b4-105">Ez az útmutató bemutatja, hogyan hozzáadása és konfigurálása a különböző típusú műveletek az API-k az API Management.</span><span class="sxs-lookup"><span data-stu-id="d69b4-105">This guide shows how to add and configure different types of operations to an API in API Management.</span></span>

## <span data-ttu-id="d69b4-106"><a name="add-operation"></a>Művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d69b4-106"><a name="add-operation"> </a>Add an operation</span></span>
<span data-ttu-id="d69b4-107">Műveletek felvétele, illetve egy API-t, a közzétevő portálon konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="d69b4-107">Operations are added and configured to an API in the publisher portal.</span></span> <span data-ttu-id="d69b4-108">A közzétevő portál eléréséhez kattintson **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d69b4-108">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="d69b4-110">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="d69b4-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="d69b4-111">Válassza ki a kívánt API a közzétevő portálon, és válassza ki a **műveletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="d69b4-111">Select the desired API in the publisher portal and then select the **Operations** tab.</span></span> 

![Műveletek][api-management-operations]

<span data-ttu-id="d69b4-113">Kattintson a **művelet hozzáadása** hozzáadása egy új művelet.</span><span class="sxs-lookup"><span data-stu-id="d69b4-113">Click **Add Operation** to add a new operation.</span></span> <span data-ttu-id="d69b4-114">A **új művelet** jelenik meg, és a **aláírás** alapértelmezett kiválasztása lapon.</span><span class="sxs-lookup"><span data-stu-id="d69b4-114">The **New operation** will be displayed and the **Signature** tab will be selected by default.</span></span>

![A művelet hozzáadása][api-management-add-operation]

<span data-ttu-id="d69b4-116">Adja meg a **HTTP-műveletet** válassza ki a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="d69b4-116">Specify the **HTTP verb** by choosing from the drop-down list.</span></span>

![HTTP-metódus][api-management-http-method]

<a name="url-template"></a>

<span data-ttu-id="d69b4-118">Az URL-cím sablon URL-cím elérési út egy vagy több szegmensek és nulla vagy több lekérdezési karakterláncot paraméterek töredékkel URL-CÍMÉT, írja be.</span><span class="sxs-lookup"><span data-stu-id="d69b4-118">Define the URL template by typing in a URL fragment consisting of one or more URL path segments and zero or more query string parameters.</span></span> <span data-ttu-id="d69b4-119">Az URL-cím sablon, az alap URL-címet a API fűzött azonosítja egy egyetlen HTTP-műveletből.</span><span class="sxs-lookup"><span data-stu-id="d69b4-119">The URL template, appended to the base URL of the API, identifies a single HTTP operation.</span></span> <span data-ttu-id="d69b4-120">Tartalmazhat egy vagy több nevű változó részeit, amelyek nincsenek azonosítva kapcsos zárójelek.</span><span class="sxs-lookup"><span data-stu-id="d69b4-120">It may contain one or more named variable parts that are identified by curly braces.</span></span> <span data-ttu-id="d69b4-121">Változó részei Sablonparaméterek hívják, és dinamikusan kiosztott értékeket kinyert a kérelem URL-címe, ha a kérés feldolgozása folyamatban van az API Management platformon.</span><span class="sxs-lookup"><span data-stu-id="d69b4-121">These variable parts are called template parameters and are dynamically assigned values extracted from the request's URL when the request is being processed by the API Management platform.</span></span>

> <span data-ttu-id="d69b4-122">Az URL-cím sablon helyettesítő mintákat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d69b4-122">The URL template can include wildcard patterns.</span></span> <span data-ttu-id="d69b4-123">Például megadó `/*` előre minden belépési kérelmet, hogy a biztonsági HTTP-metódus véget ér szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d69b4-123">For example, specifying `/*` will forward all requests for that HTTP method to the back end service.</span></span>

![URL-cím sablon][api-management-url-template]

<a name="rewrite-url-template"></a>

<span data-ttu-id="d69b4-125">Ha szükséges, adja meg a **URL-cím újraírása sablon**.</span><span class="sxs-lookup"><span data-stu-id="d69b4-125">If desired, specify the **Rewrite URL template**.</span></span> <span data-ttu-id="d69b4-126">Ez lehetővé teszi, hogy a szabványos URL-cím sablon használata az előtér-a bejövő kérelmeket feldolgozó a háttér-keresztül konvertált URL-címet a átdolgozás sablon alapján hívása közben.</span><span class="sxs-lookup"><span data-stu-id="d69b4-126">This allows you to use the standard URL template for processing incoming requests on the front-end, while calling the back-end via a converted URL according to the rewrite template.</span></span> <span data-ttu-id="d69b4-127">Az URL-cím sablonból sablon paramétereket a átdolgozás sablonban kell használni.</span><span class="sxs-lookup"><span data-stu-id="d69b4-127">Template parameters from the URL template should be used in the rewrite template.</span></span> <span data-ttu-id="d69b4-128">A következő példa bemutatja, hogyan tartalomtípus elérésiút-szegmens az webszolgáltatás az előző példából az API Management platform, az URL-sablonokkal keresztül közzétett API egy lekérdezési paraméterben meghatározott kódolású.</span><span class="sxs-lookup"><span data-stu-id="d69b4-128">The following example shows how content type encoded as path segment in the web service from the previous example can be provided as a query parameter in the API published via the API Management platform using the URL templates.</span></span>

![Sablon átdolgozás URL-címe][api-management-url-template-rewrite]

<span data-ttu-id="d69b4-130">A művelet hívók fogják használni a `/customers?customerid=ALFKI` és ez lesz rendelve `/Customers('ALFKI')` amikor meghívták a háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d69b4-130">Callers to the operation will use the format `/customers?customerid=ALFKI` and this will be mapped to `/Customers('ALFKI')` when the back-end service is invoked.</span></span>

<span data-ttu-id="d69b4-131">**Megjelenítési** nevét és **leírás** adja meg a művelet leírását, és biztosítja a dokumentáció a a fejlesztők számára az API használatával a fejlesztői portálra.</span><span class="sxs-lookup"><span data-stu-id="d69b4-131">**Display** name and **Description** provide a description of the operation and are used to provide documentation to the developers using this API in the developer portal.</span></span>

![Leírás][api-management-description]

<span data-ttu-id="d69b4-133">A művelet leírást adhat meg egyszerű szöveges vagy a HTML a **leírás** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="d69b4-133">The operation description can be specified as plain text or HTML in the **Description** text box.</span></span>

## <span data-ttu-id="d69b4-134"><a name="operation-caching"></a>Művelet gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="d69b4-134"><a name="operation-caching"> </a>Operation caching</span></span>
<span data-ttu-id="d69b4-135">Válasz gyorsítótárazását csökkenti a késést API fogyasztók, csökkenti a sávszélesség-használat és csökken a terhelés HTTP webes szolgáltatás végrehajtási az API által érzékelt.</span><span class="sxs-lookup"><span data-stu-id="d69b4-135">Response caching reduces latency perceived by the API consumers, lowers bandwidth consumption and decreases the load on the HTTP web service implementing the API.</span></span> 

<span data-ttu-id="d69b4-136">Egyszerűen és gyorsan gyorsítótárazása a művelethez, válassza ki a **gyorsítótárazását** fülre és ellenőrizze a **engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d69b4-136">To easily and quickly enable caching for the operation, select the **Caching** tab and check the **Enable** checkbox.</span></span>

![Gyorsítótárazás][api-management-caching-tab]

<span data-ttu-id="d69b4-138">**Időtartam** megadja az időtartamot, amelynek során a művelet választ a gyorsítótárban marad.</span><span class="sxs-lookup"><span data-stu-id="d69b4-138">**Duration** specifies the time period during which the operation response remains in the cache.</span></span> <span data-ttu-id="d69b4-139">Az alapértelmezett érték 3600 másodperc, vagyis 1 óra.</span><span class="sxs-lookup"><span data-stu-id="d69b4-139">The default value is 3600 seconds or 1 hour.</span></span>

<span data-ttu-id="d69b4-140">Gyorsítótár-kulcsok segítségével válaszok különböztetheti meg, hogy a választ, minden más gyorsítótárkulcshoz megfelelő saját külön gyorsítótárazott értéket kap.</span><span class="sxs-lookup"><span data-stu-id="d69b4-140">Cache keys are used to differentiate between responses so that the response corresponding to each different cache key will get its own separate cached value.</span></span> <span data-ttu-id="d69b4-141">Szükség esetén adja meg a megadott lekérdezési karakterlánc paraméterek és/vagy HTTP-fejlécek gyorsítótár értékeit számítástechnikai használandó a **lekérdezési karakterlánc paraméterei Vary** és **Vary fejléc által** szövegdobozok kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="d69b4-141">Optionally, enter specific query string parameters and/or HTTP headers to be used in computing cache key values in the **Vary by query string parameters** and **Vary by headers** text boxes respectively.</span></span> <span data-ttu-id="d69b4-142">Ha nincs megadott, teljes kérelem URL-CÍMÉT, és a következő HTTP-fejléc értékei használt a gyorsítótár Kulcslétrehozási: **elfogadás** és **Accept-Charset**.</span><span class="sxs-lookup"><span data-stu-id="d69b4-142">When none are specified, full request URL and the following HTTP header values are used in cache key generation: **Accept** and **Accept-Charset**.</span></span>

> <span data-ttu-id="d69b4-143">A és a házirendek gyorsítótárazást további információkért lásd: [gyorsítótárazásának művelet eredménye az Azure API Management][How to cache operation results in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="d69b4-143">For more information on caching and caching policies, see [How to cache operation results in Azure API Management][How to cache operation results in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="d69b4-144"><a name="request-parameters"></a>Kérelem paraméterei</span><span class="sxs-lookup"><span data-stu-id="d69b4-144"><a name="request-parameters"> </a>Request parameters</span></span>
<span data-ttu-id="d69b4-145">A Paraméterek lapon felügyelt művelet paramétereit.</span><span class="sxs-lookup"><span data-stu-id="d69b4-145">Operation parameters are managed on the Parameters tab.</span></span> <span data-ttu-id="d69b4-146">A megadott paraméterek a **URL-cím sablon** a a **aláírás** lapon a rendszer automatikusan hozzáadja, és csak az URL-cím sablon szerkesztésével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d69b4-146">Parameters specified in the **URL Template** on the **Signature** tab are added automatically and can be changed only by editing the URL template.</span></span> <span data-ttu-id="d69b4-147">További paraméterek manuálisan is megadhatók.</span><span class="sxs-lookup"><span data-stu-id="d69b4-147">Additional parameters can be entered manually.</span></span>

<span data-ttu-id="d69b4-148">Egy új lekérdezési paraméter hozzáadásához kattintson **lekérdezési paraméter hozzáadása** , és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="d69b4-148">To add a new query parameter, click **Add Query Parameter** and enter the following information:</span></span>

* <span data-ttu-id="d69b4-149">**Név** -paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="d69b4-149">**Name** - parameter name.</span></span>
* <span data-ttu-id="d69b4-150">**Leírás** -rövid leírását tartalmazza (választható) paraméter.</span><span class="sxs-lookup"><span data-stu-id="d69b4-150">**Description** - a brief description of the parameter (optional).</span></span>
* <span data-ttu-id="d69b4-151">**Típus** -paraméter típusa, az vetett kiválasztott le.</span><span class="sxs-lookup"><span data-stu-id="d69b4-151">**Type** - parameter type, selected in the drop down.</span></span>
* <span data-ttu-id="d69b4-152">**Értékek** -értékeket, amelyeket ez a paraméter lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="d69b4-152">**Values** - values that can be assigned to this parameter.</span></span> <span data-ttu-id="d69b4-153">Az értékek egyike lehet állítani alapértelmezettként (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="d69b4-153">One of the values can be marked as default (optional).</span></span>
* <span data-ttu-id="d69b4-154">**Szükséges** -paraméter kötelezővé teheti a jelölőnégyzet bejelölésével.</span><span class="sxs-lookup"><span data-stu-id="d69b4-154">**Required** - make the parameter mandatory by checking the checkbox.</span></span> 

![A kérelemben szereplő paraméterek][api-management-request-parameters]

## <span data-ttu-id="d69b4-156"><a name="request-body"></a>Kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="d69b4-156"><a name="request-body"> </a>Request body</span></span>
<span data-ttu-id="d69b4-157">Ha a művelet lehetővé teszi, hogy (pl. PUT, POST) és egy szervezet például az összes támogatott ábrázolását formátumot (pl. json, XML) által biztosított igényel.</span><span class="sxs-lookup"><span data-stu-id="d69b4-157">If the operation allows (e.g. PUT, POST) and requires a body you may provide an example of it in all of the supported representation formats (e.g. json, XML).</span></span> 

> <span data-ttu-id="d69b4-158">A kérelem törzsében csak dokumentáció célokat szolgál, és nincs érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="d69b4-158">The request body is used for documentation purposes only and is not validated.</span></span>
> 
> 

<span data-ttu-id="d69b4-159">Adjon meg egy kérelemtörzset, térjen át a **törzs** fülre.</span><span class="sxs-lookup"><span data-stu-id="d69b4-159">To enter a request body, switch to the **Body** tab.</span></span>

<span data-ttu-id="d69b4-160">Kattintson a **ábrázolását adja hozzá**, elkezdi beírni kívánt tartalom típusa nevét (például application/json), a legördülő listán válassza ki és illessze be a kívánt kérelem törzse példa a kijelölt formátumú szöveget.</span><span class="sxs-lookup"><span data-stu-id="d69b4-160">Click **Add Representation**, start typing desired content type name (e.g. application/json), select it in the drop-down, and paste the desired request body example in the selected format into the text box.</span></span> 

![Kérés törzsében][api-management-request-body]

<span data-ttu-id="d69b4-162">A további képviseletein, azt is megadhatja a egy nem kötelező leírás a **leírás** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="d69b4-162">In additional to representations, you can also specify an optional text description in the **Description** text box.</span></span>

## <span data-ttu-id="d69b4-163"><a name="responses"></a>Válaszok</span><span class="sxs-lookup"><span data-stu-id="d69b4-163"><a name="responses"> </a>Responses</span></span>
<span data-ttu-id="d69b4-164">Ajánlott a művelet készíthet összes állapotkódokat válaszok példákat.</span><span class="sxs-lookup"><span data-stu-id="d69b4-164">It is a good practice to provide examples of responses for all status codes that the operation may produce.</span></span> <span data-ttu-id="d69b4-165">Előfordulhat, hogy az egyes állapotkód egynél több válasz törzsében példa, minden támogatott tartalomtípusokat.</span><span class="sxs-lookup"><span data-stu-id="d69b4-165">Each status code may have more than one response body example, one for each of the supported content types.</span></span> 

<span data-ttu-id="d69b4-166">A válasz hozzáadásához kattintson **Hozzáadás** és kezdje el begépelni az kívánt állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="d69b4-166">To add a response, click **Add** and start typing the desired status code.</span></span> <span data-ttu-id="d69b4-167">Ebben a példában az állapotkód akkor **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="d69b4-167">In this example the status code is **200 OK**.</span></span> <span data-ttu-id="d69b4-168">Követően a kód jelenik meg a legördülő listán, válassza ki azt, és ez a válaszkód létrehozza és hozzáadja a művelethez.</span><span class="sxs-lookup"><span data-stu-id="d69b4-168">Once the code is displayed in the drop-down, select it, and the response code is created and added to your operation.</span></span>

![Válaszkód][api-management-response-code]

<span data-ttu-id="d69b4-170">Kattintson a **ábrázolását adja hozzá**elkezdi beírni a kívánt tartalom típusa nevét (például application/json), majd válassza ki az vetett le.</span><span class="sxs-lookup"><span data-stu-id="d69b4-170">Click **Add Representation**, start typing the desired content type name (e.g. application/json) and then select it in the drop down.</span></span>

![Törzs tartalom típusa][api-management-response-body-content-type]

<span data-ttu-id="d69b4-172">Illessze be a válasz törzsében példa a kijelölt formátumú szöveg.</span><span class="sxs-lookup"><span data-stu-id="d69b4-172">Paste the response body example in the selected format into the text box.</span></span> 

![Választörzs][api-management-response-body]

<span data-ttu-id="d69b4-174">Tetszés szerint megadhat egy leírást, azokat a **leírás** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="d69b4-174">If desired, add an optional description into the **Description** text box.</span></span>

<span data-ttu-id="d69b4-175">Ha a művelet már konfigurálva van, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d69b4-175">Once the operation is configured, click **Save**.</span></span>

## <span data-ttu-id="d69b4-176"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d69b4-176"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="d69b4-177">Miután a műveletek bekerülnek az API-k, a következő lépés, hogy rendelje hozzá az API-t a termék és közzéteszi az, hogy a fejlesztők meghívhatja a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d69b4-177">Once the operations are added to an API, the next step is to associate the API with a product and publish it so that developers can call its operations.</span></span>

* <span data-ttu-id="d69b4-178">[Hogyan hozhat létre, és a termék közzététele][How to create and publish a product]</span><span class="sxs-lookup"><span data-stu-id="d69b4-178">[How to create and publish a product][How to create and publish a product]</span></span>

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
