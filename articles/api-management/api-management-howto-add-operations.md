---
title: "aaaHow tooadd műveletek tooan API az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd műveletek tooan API az Azure API Management."
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
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a><span data-ttu-id="a497f-103">Hogyan tooadd műveletek tooan API az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a497f-103">How tooadd operations tooan API in Azure API Management</span></span>
<span data-ttu-id="a497f-104">Egy API-t az API Management használata előtt hozzá kell adni műveletek.</span><span class="sxs-lookup"><span data-stu-id="a497f-104">Before an API in API Management can be used, operations must be added.</span></span> <span data-ttu-id="a497f-105">Ez útmutató bemutatja hogyan tooadd és műveletek tooan API különböző típusú konfigurálhatja az API Management.</span><span class="sxs-lookup"><span data-stu-id="a497f-105">This guide shows how tooadd and configure different types of operations tooan API in API Management.</span></span>

## <span data-ttu-id="a497f-106"><a name="add-operation"></a>Művelet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a497f-106"><a name="add-operation"> </a>Add an operation</span></span>
<span data-ttu-id="a497f-107">Műveletek bekerülnek és tooan API a hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="a497f-107">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="a497f-108">tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a497f-108">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="a497f-110">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="a497f-110">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="a497f-111">Hello jelölje be a keresett API hello publisher portálon, és jelölje ki hello **műveletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="a497f-111">Select hello desired API in hello publisher portal and then select hello **Operations** tab.</span></span> 

![Műveletek][api-management-operations]

<span data-ttu-id="a497f-113">Kattintson a **művelet hozzáadása** tooadd új művelet.</span><span class="sxs-lookup"><span data-stu-id="a497f-113">Click **Add Operation** tooadd a new operation.</span></span> <span data-ttu-id="a497f-114">Hello **új művelet** jelenik meg, és hello **aláírás** alapértelmezett kiválasztása lapon.</span><span class="sxs-lookup"><span data-stu-id="a497f-114">hello **New operation** will be displayed and hello **Signature** tab will be selected by default.</span></span>

![A művelet hozzáadása][api-management-add-operation]

<span data-ttu-id="a497f-116">Adja meg a hello **HTTP-műveletet** hello legördülő listából válassza ki.</span><span class="sxs-lookup"><span data-stu-id="a497f-116">Specify hello **HTTP verb** by choosing from hello drop-down list.</span></span>

![HTTP-metódus][api-management-http-method]

<a name="url-template"></a>

<span data-ttu-id="a497f-118">Sablon hello URL-CÍMÉT, írja be egy vagy több URL-cím elérési út szegmensek és nulla vagy több lekérdezési karakterláncot paraméterek URL-cím töredéket.</span><span class="sxs-lookup"><span data-stu-id="a497f-118">Define hello URL template by typing in a URL fragment consisting of one or more URL path segments and zero or more query string parameters.</span></span> <span data-ttu-id="a497f-119">hello URL-cím sablon hozzáfűzött toohello alap URL-CÍMÉT hello API-t egyetlen HTTP-művelet azonosítja.</span><span class="sxs-lookup"><span data-stu-id="a497f-119">hello URL template, appended toohello base URL of hello API, identifies a single HTTP operation.</span></span> <span data-ttu-id="a497f-120">Tartalmazhat egy vagy több nevű változó részeit, amelyek nincsenek azonosítva kapcsos zárójelek.</span><span class="sxs-lookup"><span data-stu-id="a497f-120">It may contain one or more named variable parts that are identified by curly braces.</span></span> <span data-ttu-id="a497f-121">Változó részei Sablonparaméterek hívják, és dinamikusan kiosztott értékeket kinyert hello kérelem URL-címe, ha hello kérés feldolgozása folyamatban van hello API platformja.</span><span class="sxs-lookup"><span data-stu-id="a497f-121">These variable parts are called template parameters and are dynamically assigned values extracted from hello request's URL when hello request is being processed by hello API Management platform.</span></span>

> <span data-ttu-id="a497f-122">hello URL-cím sablon helyettesítő mintákat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a497f-122">hello URL template can include wildcard patterns.</span></span> <span data-ttu-id="a497f-123">Például megadó `/*` előre, hogy HTTP metódus toohello vissza az összes kérelem véget ér szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a497f-123">For example, specifying `/*` will forward all requests for that HTTP method toohello back end service.</span></span>

![URL-cím sablon][api-management-url-template]

<a name="rewrite-url-template"></a>

<span data-ttu-id="a497f-125">Ha szükséges, adja meg a hello **URL-cím újraírása sablon**.</span><span class="sxs-lookup"><span data-stu-id="a497f-125">If desired, specify hello **Rewrite URL template**.</span></span> <span data-ttu-id="a497f-126">Ez lehetővé teszi toouse hello szabványos URL-cím sablon az előtér-hello beérkező kérelmek feldolgozásához, hello háttér-egy toohello szerint konvertált URL-CÍMEN keresztül hívása közben újraírási sablont.</span><span class="sxs-lookup"><span data-stu-id="a497f-126">This allows you toouse hello standard URL template for processing incoming requests on hello front-end, while calling hello back-end via a converted URL according toohello rewrite template.</span></span> <span data-ttu-id="a497f-127">Sablonparaméterek hello URL-cím sablonból hello átdolgozás sablonban kell használni.</span><span class="sxs-lookup"><span data-stu-id="a497f-127">Template parameters from hello URL template should be used in hello rewrite template.</span></span> <span data-ttu-id="a497f-128">hello következő példa bemutatja, hogyan tartalomtípus, elérésiút-szegmens hello webszolgáltatás hello előző példa megadható egy lekérdezés paramétere hello hello keresztül közzétett API hello URL-cím sablonok segítségével az API Management platform kódolású.</span><span class="sxs-lookup"><span data-stu-id="a497f-128">hello following example shows how content type encoded as path segment in hello web service from hello previous example can be provided as a query parameter in hello API published via hello API Management platform using hello URL templates.</span></span>

![Sablon átdolgozás URL-címe][api-management-url-template-rewrite]

<span data-ttu-id="a497f-130">Hívóknak toohello műveletet fogják használni hello `/customers?customerid=ALFKI` és ez túl rendelendő`/Customers('ALFKI')` amikor meghívták hello háttér-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a497f-130">Callers toohello operation will use hello format `/customers?customerid=ALFKI` and this will be mapped too`/Customers('ALFKI')` when hello back-end service is invoked.</span></span>

<span data-ttu-id="a497f-131">**Megjelenítési** nevét és **leírás** hello művelet leírását adhatja meg, és használt tooprovide dokumentáció toohello fejlesztők számára az API használatával hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="a497f-131">**Display** name and **Description** provide a description of hello operation and are used tooprovide documentation toohello developers using this API in hello developer portal.</span></span>

![Leírás][api-management-description]

<span data-ttu-id="a497f-133">hello művelet leírását is megadni egyszerű szöveg-vagy HTML hello **leírás** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="a497f-133">hello operation description can be specified as plain text or HTML in hello **Description** text box.</span></span>

## <span data-ttu-id="a497f-134"><a name="operation-caching"></a>Művelet gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="a497f-134"><a name="operation-caching"> </a>Operation caching</span></span>
<span data-ttu-id="a497f-135">Válasz gyorsítótárazását csökkenti a késést érzékelt hello API fogyasztó, csökkenti a sávszélesség-használat és csökkenő hello terhelése hello HTTP webes szolgáltatás végrehajtási hello API.</span><span class="sxs-lookup"><span data-stu-id="a497f-135">Response caching reduces latency perceived by hello API consumers, lowers bandwidth consumption and decreases hello load on hello HTTP web service implementing hello API.</span></span> 

<span data-ttu-id="a497f-136">tooeasily és gyorsan gyorsítótárazása hello a művelethez, jelölje be hello **gyorsítótárazását** fülre és ellenőrizze a hello **engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="a497f-136">tooeasily and quickly enable caching for hello operation, select hello **Caching** tab and check hello **Enable** checkbox.</span></span>

![Gyorsítótárazás][api-management-caching-tab]

<span data-ttu-id="a497f-138">**Időtartam** megadja hello időszak során melyik hello művelet válasz hello gyorsítótárában marad.</span><span class="sxs-lookup"><span data-stu-id="a497f-138">**Duration** specifies hello time period during which hello operation response remains in hello cache.</span></span> <span data-ttu-id="a497f-139">hello alapértelmezett értéke 3600 másodperc, vagyis 1 óra.</span><span class="sxs-lookup"><span data-stu-id="a497f-139">hello default value is 3600 seconds or 1 hour.</span></span>

<span data-ttu-id="a497f-140">Gyorsítótár kulcsai között válaszok használt toodifferentiate, hogy a megfelelő tooeach különböző gyorsítótárkulcshoz hello választ kap a saját külön gyorsítótárazott érték.</span><span class="sxs-lookup"><span data-stu-id="a497f-140">Cache keys are used toodifferentiate between responses so that hello response corresponding tooeach different cache key will get its own separate cached value.</span></span> <span data-ttu-id="a497f-141">Szükség esetén adja meg a megadott lekérdezési karakterlánc paraméterek és/vagy a gyorsítótár-értékeit hello számítástechnikai használt HTTP-fejlécek toobe **lekérdezési karakterlánc paraméterei Vary** és **Vary fejléc által** szövegdobozok kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="a497f-141">Optionally, enter specific query string parameters and/or HTTP headers toobe used in computing cache key values in hello **Vary by query string parameters** and **Vary by headers** text boxes respectively.</span></span> <span data-ttu-id="a497f-142">Ha nincs megadott, teljes kérelem URL-CÍMÉT, és a következő HTTP-fejléc értékei hello használt gyorsítótár Kulcslétrehozási: **elfogadás** és **Accept-Charset**.</span><span class="sxs-lookup"><span data-stu-id="a497f-142">When none are specified, full request URL and hello following HTTP header values are used in cache key generation: **Accept** and **Accept-Charset**.</span></span>

> <span data-ttu-id="a497f-143">A és a házirendek gyorsítótárazást további információkért lásd: [hogyan toocache művelet eredménye az Azure API Management][How toocache operation results in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="a497f-143">For more information on caching and caching policies, see [How toocache operation results in Azure API Management][How toocache operation results in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="a497f-144"><a name="request-parameters"></a>Kérelem paraméterei</span><span class="sxs-lookup"><span data-stu-id="a497f-144"><a name="request-parameters"> </a>Request parameters</span></span>
<span data-ttu-id="a497f-145">Művelet paramétereit felügyelt hello paraméterek lapon. Hello megadott paraméterek **URL-cím sablon** a hello **aláírás** lapon a rendszer automatikusan hozzáadja, és csak a hello URL-cím sablon szerkesztésével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a497f-145">Operation parameters are managed on hello Parameters tab. Parameters specified in hello **URL Template** on hello **Signature** tab are added automatically and can be changed only by editing hello URL template.</span></span> <span data-ttu-id="a497f-146">További paraméterek manuálisan is megadhatók.</span><span class="sxs-lookup"><span data-stu-id="a497f-146">Additional parameters can be entered manually.</span></span>

<span data-ttu-id="a497f-147">Kattintson egy új lekérdezési paraméter tooadd **lekérdezési paraméter hozzáadása** , és írja be a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="a497f-147">tooadd a new query parameter, click **Add Query Parameter** and enter hello following information:</span></span>

* <span data-ttu-id="a497f-148">**Név** -paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="a497f-148">**Name** - parameter name.</span></span>
* <span data-ttu-id="a497f-149">**Leírás** -hello paraméter (választható) rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="a497f-149">**Description** - a brief description of hello parameter (optional).</span></span>
* <span data-ttu-id="a497f-150">**Típus** -paramétertípus hello legördülő lista a kijelölt.</span><span class="sxs-lookup"><span data-stu-id="a497f-150">**Type** - parameter type, selected in hello drop down.</span></span>
* <span data-ttu-id="a497f-151">**Értékek** -értékeket, amelyeket toothis paraméter lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="a497f-151">**Values** - values that can be assigned toothis parameter.</span></span> <span data-ttu-id="a497f-152">Hello értékek egyike lehet állítani alapértelmezettként (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="a497f-152">One of hello values can be marked as default (optional).</span></span>
* <span data-ttu-id="a497f-153">**Szükséges** -hello paraméter hello jelölőnégyzet bejelölésével kötelezővé tételéhez.</span><span class="sxs-lookup"><span data-stu-id="a497f-153">**Required** - make hello parameter mandatory by checking hello checkbox.</span></span> 

![A kérelemben szereplő paraméterek][api-management-request-parameters]

## <span data-ttu-id="a497f-155"><a name="request-body"></a>Kérelem törzse</span><span class="sxs-lookup"><span data-stu-id="a497f-155"><a name="request-body"> </a>Request body</span></span>
<span data-ttu-id="a497f-156">Ha hello művelet lehetővé teszi, hogy (pl. PUT, POST), és előfordulhat, hogy adja meg például az összes hello törzs támogatott ábrázolását formátumokban (pl. json, XML) igényel.</span><span class="sxs-lookup"><span data-stu-id="a497f-156">If hello operation allows (e.g. PUT, POST) and requires a body you may provide an example of it in all of hello supported representation formats (e.g. json, XML).</span></span> 

> <span data-ttu-id="a497f-157">hello kérelemtörzset csak dokumentáció célokat szolgál, és nincs érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="a497f-157">hello request body is used for documentation purposes only and is not validated.</span></span>
> 
> 

<span data-ttu-id="a497f-158">a kérelem törzsében tooenter kapcsoló toohello **törzs** lapon.</span><span class="sxs-lookup"><span data-stu-id="a497f-158">tooenter a request body, switch toohello **Body** tab.</span></span>

<span data-ttu-id="a497f-159">Kattintson a **ábrázolását adja hozzá**, írjon be a kívánt típus neve (például application/json), jelölje ki a legördülő lista hello és beillesztés hello szükséges kérelem törzse példa hello kijelölt formátumban hello szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="a497f-159">Click **Add Representation**, start typing desired content type name (e.g. application/json), select it in hello drop-down, and paste hello desired request body example in hello selected format into hello text box.</span></span> 

![Kérés törzsében][api-management-request-body]

<span data-ttu-id="a497f-161">A további toorepresentations is megadható egy nem kötelező leírás a hello **leírás** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="a497f-161">In additional toorepresentations, you can also specify an optional text description in hello **Description** text box.</span></span>

## <span data-ttu-id="a497f-162"><a name="responses"></a>Válaszok</span><span class="sxs-lookup"><span data-stu-id="a497f-162"><a name="responses"> </a>Responses</span></span>
<span data-ttu-id="a497f-163">Egy célszerű tooprovide példák hello művelet készíthet összes állapotkódokat a válaszokat.</span><span class="sxs-lookup"><span data-stu-id="a497f-163">It is a good practice tooprovide examples of responses for all status codes that hello operation may produce.</span></span> <span data-ttu-id="a497f-164">Előfordulhat, hogy minden állapotkód egynél több válasz törzsében példa, minden hello támogatott tartalomtípusokat.</span><span class="sxs-lookup"><span data-stu-id="a497f-164">Each status code may have more than one response body example, one for each of hello supported content types.</span></span> 

<span data-ttu-id="a497f-165">tooadd választ, kattintson a **Hozzáadás** és kezdje el begépelni szükséges hello állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="a497f-165">tooadd a response, click **Add** and start typing hello desired status code.</span></span> <span data-ttu-id="a497f-166">Példa hello állapota kódja: **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="a497f-166">In this example hello status code is **200 OK**.</span></span> <span data-ttu-id="a497f-167">Miután hello legördülő hello kód jelenik meg, válassza ki azt, és hello válaszkód létrehozott és a hozzáadott tooyour műveletet.</span><span class="sxs-lookup"><span data-stu-id="a497f-167">Once hello code is displayed in hello drop-down, select it, and hello response code is created and added tooyour operation.</span></span>

![Válaszkód][api-management-response-code]

<span data-ttu-id="a497f-169">Kattintson a **ábrázolását adja hozzá**, kezdje el begépelni hello kívánt típus neve (például application/json), és jelölje ki azt a hello legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="a497f-169">Click **Add Representation**, start typing hello desired content type name (e.g. application/json) and then select it in hello drop down.</span></span>

![Törzs tartalom típusa][api-management-response-body-content-type]

<span data-ttu-id="a497f-171">Illessze be hello válasz törzsében példa hello kijelölt formátumban hello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="a497f-171">Paste hello response body example in hello selected format into hello text box.</span></span> 

![Választörzs][api-management-response-body]

<span data-ttu-id="a497f-173">Ha szükséges, megadhat egy leírást, a hello **leírása** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="a497f-173">If desired, add an optional description into hello **Description** text box.</span></span>

<span data-ttu-id="a497f-174">Ha konfigurálva van a hello műveletet, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a497f-174">Once hello operation is configured, click **Save**.</span></span>

## <span data-ttu-id="a497f-175"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a497f-175"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="a497f-176">Miután hello műveletek bekerülnek a tooan API, hello tovább tooassociate hello API termékkel rendelkező és közzéteszi az, hogy a fejlesztők meghívhatja a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a497f-176">Once hello operations are added tooan API, hello next step is tooassociate hello API with a product and publish it so that developers can call its operations.</span></span>

* <span data-ttu-id="a497f-177">[Hogyan toocreate és a termék közzététele][How toocreate and publish a product]</span><span class="sxs-lookup"><span data-stu-id="a497f-177">[How toocreate and publish a product][How toocreate and publish a product]</span></span>

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

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
