---
title: "gyorsítótárazás tooimprove teljesítmény az Azure API Management aaaAdd |} Microsoft Docs"
description: "Ismerje meg, hogyan tölthető be a tooimprove hello késés, sávszélesség-használat és web service API-kezelés szolgáltatás hívások."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 056ab7cf788218327e30bd5c028b76e3b1977fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a><span data-ttu-id="557a7-103">Adja hozzá a gyorsítótárazási tooimprove teljesítmény az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="557a7-103">Add caching tooimprove performance in Azure API Management</span></span>
<span data-ttu-id="557a7-104">Az API Management műveleteit konfigurálni lehet a válaszok gyorsítótárazásához.</span><span class="sxs-lookup"><span data-stu-id="557a7-104">Operations in API Management can be configured for response caching.</span></span> <span data-ttu-id="557a7-105">A válaszok gyorsítótárazása jelentősen csökkentheti az API-k késleltetését, sávszélesség-használatát és a webszolgáltatások terhelését olyan adatok esetén, amelyek nem változnak gyakran.</span><span class="sxs-lookup"><span data-stu-id="557a7-105">Response caching can significantly reduce API latency, bandwidth consumption, and web service load for data that does not change frequently.</span></span>

<span data-ttu-id="557a7-106">Ez az útmutató bemutatja, hogyan tooadd válasz az API-gyorsítótárazás és a hello minta Echo API műveletek házirendek konfigurálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="557a7-106">This guide shows you how tooadd response caching for your API and configure policies for hello sample Echo API operations.</span></span> <span data-ttu-id="557a7-107">Majd hívása hello művelet hello developer portálon tooverify gyorsítótárazásának működés közben.</span><span class="sxs-lookup"><span data-stu-id="557a7-107">You can then call hello operation from hello developer portal tooverify caching in action.</span></span>

> [!NOTE]
> <span data-ttu-id="557a7-108">További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="557a7-108">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="557a7-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="557a7-109">Prerequisites</span></span>
<span data-ttu-id="557a7-110">Mielőtt következő hello a jelen útmutató lépéseit, rendelkeznie kell egy API-t és a konfigurált termék API-kezelés szolgáltatás példány.</span><span class="sxs-lookup"><span data-stu-id="557a7-110">Before following hello steps in this guide, you must have an API Management service instance with an API and a product configured.</span></span> <span data-ttu-id="557a7-111">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="557a7-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

## <span data-ttu-id="557a7-112"><a name="configure-caching"></a>Művelet konfigurálása gyorsítótárazáshoz</span><span class="sxs-lookup"><span data-stu-id="557a7-112"><a name="configure-caching"> </a>Configure an operation for caching</span></span>
<span data-ttu-id="557a7-113">Ebben a lépésben gyorsítótárazási beállítások a hello hello felülvizsgálja **(gyorsítótárazott), első erőforrás** hello minta Echo API működését.</span><span class="sxs-lookup"><span data-stu-id="557a7-113">In this step, you will review hello caching settings of hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

> [!NOTE]
> <span data-ttu-id="557a7-114">Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak.</span><span class="sxs-lookup"><span data-stu-id="557a7-114">Each API Management service instance comes preconfigured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="557a7-115">További információkért lásd: [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="557a7-115">For more information, see [Get started with Azure API Management][Get started with Azure API Management].</span></span>
> 
> 

<span data-ttu-id="557a7-116">tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="557a7-116">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span> <span data-ttu-id="557a7-117">Ezzel megnyitná toohello API Management publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="557a7-117">This takes you toohello API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="557a7-119">Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="557a7-119">Click **APIs** from hello **API Management** menu on hello left, and then click **Echo API**.</span></span>

![Echo API][api-management-echo-api]

<span data-ttu-id="557a7-121">Hello kattintson **műveletek** fülre, majd hello **(gyorsítótárazott), első erőforrás** hello műveletet **műveletek** listája.</span><span class="sxs-lookup"><span data-stu-id="557a7-121">Click hello **Operations** tab, and then click hello **GET Resource (cached)** operation from hello **Operations** list.</span></span>

![Echo API műveletek][api-management-echo-api-operations]

<span data-ttu-id="557a7-123">Kattintson a hello **gyorsítótárazását** lapon tooview hello gyorsítótárazási beállítások ehhez a művelethez.</span><span class="sxs-lookup"><span data-stu-id="557a7-123">Click hello **Caching** tab tooview hello caching settings for this operation.</span></span>

![Gyorsítótárazás lap][api-management-caching-tab]

<span data-ttu-id="557a7-125">művelet, jelölje be hello gyorsítótárazás tooenable **engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="557a7-125">tooenable caching for an operation, select hello **Enable** check box.</span></span> <span data-ttu-id="557a7-126">Ebben a példában engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="557a7-126">In this example, caching is enabled.</span></span>

<span data-ttu-id="557a7-127">Minden egyes művelet válasz kulccsal, hello hello értékei alapján **lekérdezési karakterlánc paraméterei Vary** és **Vary fejléc által** mezőket.</span><span class="sxs-lookup"><span data-stu-id="557a7-127">Each operation response is keyed, based on hello values in hello **Vary by query string parameters** and **Vary by headers** fields.</span></span> <span data-ttu-id="557a7-128">Ha azt szeretné, hogy a lekérdezési karakterlánc paramétereket vagy fejlécek alapján több válaszok toocache, konfigurálhatja azokat a következő két mező.</span><span class="sxs-lookup"><span data-stu-id="557a7-128">If you want toocache multiple responses based on query string parameters or headers, you can configure them in these two fields.</span></span>

<span data-ttu-id="557a7-129">**Időtartam** hello lejárati időköz hello gyorsítótárazott válaszok.</span><span class="sxs-lookup"><span data-stu-id="557a7-129">**Duration** specifies hello expiration interval of hello cached responses.</span></span> <span data-ttu-id="557a7-130">Ebben a példában a hello időköz: **3600** másodperc, amely egyenértékű tooone óra.</span><span class="sxs-lookup"><span data-stu-id="557a7-130">In this example, hello interval is **3600** seconds, which is equivalent tooone hour.</span></span>

<span data-ttu-id="557a7-131">Ebben a példában a konfigurációs gyorsítótár hello használva hello első kérelem toohello **(gyorsítótárazott), első erőforrás** művelet hello háttérszolgáltatást választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="557a7-131">Using hello caching configuration in this example, hello first request toohello **GET Resource (cached)** operation returns a response from hello backend service.</span></span> <span data-ttu-id="557a7-132">Ez a válasz a gyorsítótárba fognak kerülni, által megadott hello kulccsal fejlécek és a lekérdezési karakterlánc-paraméter.</span><span class="sxs-lookup"><span data-stu-id="557a7-132">This response will be cached, keyed by hello specified headers and query string parameters.</span></span> <span data-ttu-id="557a7-133">Toohello műveletet a megfelelő paramétereket, lesz hello későbbi hívások gyorsítótárazott választ adott vissza, amíg hello időköz időtartam lejárt.</span><span class="sxs-lookup"><span data-stu-id="557a7-133">Subsequent calls toohello operation, with matching parameters, will have hello cached response returned, until hello cache duration interval has expired.</span></span>

## <span data-ttu-id="557a7-134"><a name="caching-policies"></a>Felülvizsgálati hello házirendek gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="557a7-134"><a name="caching-policies"> </a>Review hello caching policies</span></span>
<span data-ttu-id="557a7-135">Ebben a lépésben megtekintheti hello beállításainak gyorsítótárazás hello **(gyorsítótárazott), első erőforrás** hello minta Echo API működését.</span><span class="sxs-lookup"><span data-stu-id="557a7-135">In this step, you review hello caching settings for hello **GET Resource (cached)** operation of hello sample Echo API.</span></span>

<span data-ttu-id="557a7-136">Ha gyorsítótárazási beállításai a hello művelet **gyorsítótárazását** lap gyorsítótárazás házirendek kerülnek hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="557a7-136">When caching settings are configured for an operation on hello **Caching** tab, caching policies are added for hello operation.</span></span> <span data-ttu-id="557a7-137">Ezek a házirendek tekinthetők meg és hello Helyicsoportházirend-szerkesztőben szerkeszti.</span><span class="sxs-lookup"><span data-stu-id="557a7-137">These policies can be viewed and edited in hello policy editor.</span></span>

<span data-ttu-id="557a7-138">Kattintson a **házirendek** hello a **API Management** hello bal, majd válassza a menü **Echo API / ERŐFORRÁSKÉSZLET (gyorsítótárazott)** hello a **művelet**legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="557a7-138">Click **Policies** from hello **API Management** menu on hello left, and then select **Echo API / GET Resource (cached)** from hello **Operation** drop-down list.</span></span>

![Házirend-hatókör művelet][api-management-operation-dropdown]

<span data-ttu-id="557a7-140">A Helyicsoportházirend-szerkesztő hello hello házirendek ehhez a művelethez jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="557a7-140">This displays hello policies for this operation in hello policy editor.</span></span>

![API Management házirendszerkesztő][api-management-policy-editor]

<span data-ttu-id="557a7-142">házirend-definíció hello a művelethez tartalmaz hello házirendekre konfigurációs gyorsítótárazás hello meghatározó elvégezték hello segítségével **gyorsítótárazását** hello előző lépésben fülre.</span><span class="sxs-lookup"><span data-stu-id="557a7-142">hello policy definition for this operation includes hello policies that define hello caching configuration that were reviewed using hello **Caching** tab in hello previous step.</span></span>

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
        </cache-lookup>
        <rewrite-uri template="/resource" />
    </inbound>
    <outbound>
        <base />
        <cache-store caching-mode="cache-on" duration="3600" />
    </outbound>
</policies>
```

> [!NOTE]
> <span data-ttu-id="557a7-143">Házirendek a Helyicsoportházirend-szerkesztő hello gyorsítótárazás végrehajtott módosítások toohello hello meg fog jelenni **gyorsítótárazását** lapján egy műveletet, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="557a7-143">Changes made toohello caching policies in hello policy editor will be reflected on hello **Caching** tab of an operation, and vice-versa.</span></span>
> 
> 

## <span data-ttu-id="557a7-144"><a name="test-operation"></a>Hívható meg egy műveletet, és tesztelje a hello gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="557a7-144"><a name="test-operation"> </a>Call an operation and test hello caching</span></span>
<span data-ttu-id="557a7-145">toosee hello gyorsítótárazási működés közben is nevezzük hello művelet hello developer portálról.</span><span class="sxs-lookup"><span data-stu-id="557a7-145">toosee hello caching in action, we can call hello operation from hello developer portal.</span></span> <span data-ttu-id="557a7-146">Kattintson a **fejlesztői portálján** hello jobb felső menüjében található.</span><span class="sxs-lookup"><span data-stu-id="557a7-146">Click **Developer portal** in hello top right menu.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="557a7-148">Kattintson a **API-k** a felső menüben hello, és válassza ki **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="557a7-148">Click **APIs** in hello top menu, and then select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> <span data-ttu-id="557a7-150">Ha csak egy API konfigurálva van, vagy látható tooyour fiókot, majd kattintson az API-k viszi közvetlenül toohello műveletek, hogy az API-hoz.</span><span class="sxs-lookup"><span data-stu-id="557a7-150">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="557a7-151">Jelölje be hello **(gyorsítótárazott), első erőforrás** műveletet, és kattintson **nyissa meg a konzolt**.</span><span class="sxs-lookup"><span data-stu-id="557a7-151">Select hello **GET Resource (cached)** operation, and then click **Open Console**.</span></span>

![Konzol megnyitása][api-management-open-console]

<span data-ttu-id="557a7-153">hello konzol lehetővé teszi tooinvoke műveletek közvetlenül a hello fejlesztői portálján.</span><span class="sxs-lookup"><span data-stu-id="557a7-153">hello console allows you tooinvoke operations directly from hello developer portal.</span></span>

![Konzol][api-management-console]

<span data-ttu-id="557a7-155">Tartsa hello alapértelmezett értékeinek **param1** és **paraméter2**.</span><span class="sxs-lookup"><span data-stu-id="557a7-155">Keep hello default values for **param1** and **param2**.</span></span>

<span data-ttu-id="557a7-156">Válassza ki a kívánt kulcsot az hello hello **előfizetés-kulcs** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="557a7-156">Select hello desired key from hello **subscription-key** drop-down list.</span></span> <span data-ttu-id="557a7-157">Ha a fiókja csak egyetlen előfizetéssel rendelkezik, akkor az alapból ki lesz választva.</span><span class="sxs-lookup"><span data-stu-id="557a7-157">If your account has only one subscription, it will already be selected.</span></span>

<span data-ttu-id="557a7-158">Adja meg **sampleheader:value1** a hello **kérelem fejlécei** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="557a7-158">Enter **sampleheader:value1** in hello **Request headers** text box.</span></span>

<span data-ttu-id="557a7-159">Kattintson a **HTTP Get** és jegyezze fel a hello response fejlécekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="557a7-159">Click **HTTP Get** and make a note of hello response headers.</span></span>

<span data-ttu-id="557a7-160">Adja meg **sampleheader:value2** a hello **kérelem fejlécei** szövegmezőbe, és kattintson **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="557a7-160">Enter **sampleheader:value2** in hello **Request headers** text box, and then click **HTTP Get**.</span></span>

<span data-ttu-id="557a7-161">Vegye figyelembe, hogy hello értékének **sampleheader** továbbra is **érték1** hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="557a7-161">Note that hello value of **sampleheader** is still **value1** in hello response.</span></span> <span data-ttu-id="557a7-162">Néhány más értékeivel, és vegye figyelembe, hogy a hello első hívás gyorsítótárazott válasz hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="557a7-162">Try some different values and note that hello cached response from hello first call is returned.</span></span>

<span data-ttu-id="557a7-163">Adja meg **25** történő hello **paraméter2** mezőben, majd kattintson a **HTTP Get**.</span><span class="sxs-lookup"><span data-stu-id="557a7-163">Enter **25** into hello **param2** field, and then click **HTTP Get**.</span></span>

<span data-ttu-id="557a7-164">Vegye figyelembe, hogy hello értékének **sampleheader** hello a válasz mostantól **érték2**.</span><span class="sxs-lookup"><span data-stu-id="557a7-164">Note that hello value of **sampleheader** in hello response is now **value2**.</span></span> <span data-ttu-id="557a7-165">A művelet eredménye hello lekérdezési karakterlánc szerinti kulccsal vannak ellátva, mert hello előző gyorsítótárazott válasz nem adott vissza.</span><span class="sxs-lookup"><span data-stu-id="557a7-165">Because hello operation results are keyed by query string, hello previous cached response was not returned.</span></span>

## <span data-ttu-id="557a7-166"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="557a7-166"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="557a7-167">További információ a házirendek gyorsítótárazás: [házirendek gyorsítótárazás] [ Caching policies] a hello [API-felügyeleti szabályzatainak ismertetése][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="557a7-167">For more information about caching policies, see [Caching policies][Caching policies] in hello [API Management policy reference][API Management policy reference].</span></span>
* <span data-ttu-id="557a7-168">További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="557a7-168">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review hello caching policies]: #caching-policies
[Call an operation and test hello caching]: #test-operation
[Next steps]: #next-steps
