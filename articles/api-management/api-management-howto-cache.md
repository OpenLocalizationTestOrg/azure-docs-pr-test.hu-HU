---
title: "Gyorsítótárazás hozzáadása az Azure API Management teljesítményének javításához | Microsoft Docs"
description: "Megtudhatja, hogyan javíthat az API Management szolgáltatáshívások késleltetésén, sávszélesség-használatán és a webszolgáltatások terhelésén."
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
ms.openlocfilehash: 59c595f0d5ce849f44c46fdb6cab0b44d35fffa0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a><span data-ttu-id="9a8ba-103">Gyorsítótárazás hozzáadása az Azure API Management teljesítményének javításához</span><span class="sxs-lookup"><span data-stu-id="9a8ba-103">Add caching to improve performance in Azure API Management</span></span>
<span data-ttu-id="9a8ba-104">Az API Management műveleteit konfigurálni lehet a válaszok gyorsítótárazásához.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-104">Operations in API Management can be configured for response caching.</span></span> <span data-ttu-id="9a8ba-105">A válaszok gyorsítótárazása jelentősen csökkentheti az API-k késleltetését, sávszélesség-használatát és a webszolgáltatások terhelését olyan adatok esetén, amelyek nem változnak gyakran.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-105">Response caching can significantly reduce API latency, bandwidth consumption, and web service load for data that does not change frequently.</span></span>

<span data-ttu-id="9a8ba-106">Jelen útmutató ismerteti, hogyan adhatja hozzá az API-hoz a válaszok gyorsítótárazását, és hogyan konfigurálhat házirendeket a minta Echo API-műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-106">This guide shows you how to add response caching for your API and configure policies for the sample Echo API operations.</span></span> <span data-ttu-id="9a8ba-107">Ezt követően meghívhatja a műveletet a fejlesztői portálról a gyorsítótárazás működés közbeni ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-107">You can then call the operation from the developer portal to verify caching in action.</span></span>

> [!NOTE]
> <span data-ttu-id="9a8ba-108">További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="9a8ba-108">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="9a8ba-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9a8ba-109">Prerequisites</span></span>
<span data-ttu-id="9a8ba-110">Mielőtt belekezdene az útmutató lépéseibe, rendelkeznie kell egy API Management szolgáltatáspéldánnyal, amelyhez konfigurálva van egy API és egy termék.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-110">Before following the steps in this guide, you must have an API Management service instance with an API and a product configured.</span></span> <span data-ttu-id="9a8ba-111">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>

## <span data-ttu-id="9a8ba-112"><a name="configure-caching"> </a>Művelet konfigurálása gyorsítótárazáshoz</span><span class="sxs-lookup"><span data-stu-id="9a8ba-112"><a name="configure-caching"> </a>Configure an operation for caching</span></span>
<span data-ttu-id="9a8ba-113">Ebben a lépésben a minta Echo API **GET Resource (cached)** műveletének gyorsítótárazási beállításait fogja ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-113">In this step, you will review the caching settings of the **GET Resource (cached)** operation of the sample Echo API.</span></span>

> [!NOTE]
> <span data-ttu-id="9a8ba-114">Minden API Management szolgáltatáspéldányhoz előre konfigurálva van egy kipróbálható Echo API, amely segít megismerni az API Management szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-114">Each API Management service instance comes preconfigured with an Echo API that can be used to experiment with and learn about API Management.</span></span> <span data-ttu-id="9a8ba-115">További információkért lásd: [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="9a8ba-115">For more information, see [Get started with Azure API Management][Get started with Azure API Management].</span></span>
> 
> 

<span data-ttu-id="9a8ba-116">Első lépésként kattintson a **Közzétevő portál** elemre az API Management szolgáltatás Azure Portalján.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-116">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span> <span data-ttu-id="9a8ba-117">Ezzel továbblép az API Management közzétevő portáljára.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-117">This takes you to the API Management publisher portal.</span></span>

![Közzétevő portál][api-management-management-console]

<span data-ttu-id="9a8ba-119">Kattintson a bal oldali **API Management** menü **API-k** elemére, majd kattintson az **Echo API** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-119">Click **APIs** from the **API Management** menu on the left, and then click **Echo API**.</span></span>

![Echo API][api-management-echo-api]

<span data-ttu-id="9a8ba-121">Kattintson a **Műveletek** lapra, majd kattintson a **Műveletek** lista **GET Resource (cached)** műveletére.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-121">Click the **Operations** tab, and then click the **GET Resource (cached)** operation from the **Operations** list.</span></span>

![Echo API műveletek][api-management-echo-api-operations]

<span data-ttu-id="9a8ba-123">Kattintson a **Gyorsítótárazás** lapra a művelet gyorsítótárazási beállításainak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-123">Click the **Caching** tab to view the caching settings for this operation.</span></span>

![Gyorsítótárazás lap][api-management-caching-tab]

<span data-ttu-id="9a8ba-125">Ha engedélyezni szeretné a gyorsítótárazást egy művelet számára, jelölje be az **Engedélyezés** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-125">To enable caching for an operation, select the **Enable** check box.</span></span> <span data-ttu-id="9a8ba-126">Ebben a példában engedélyezve van a gyorsítótárazás.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-126">In this example, caching is enabled.</span></span>

<span data-ttu-id="9a8ba-127">Minden művelet válasza egy kulccsal van ellátva a **Változtatás a lekérdezési karakterlánc paraméterei alapján** és **Változtatás a fejlécek alapján** mezőkben megadott értékek szerint.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-127">Each operation response is keyed, based on the values in the **Vary by query string parameters** and **Vary by headers** fields.</span></span> <span data-ttu-id="9a8ba-128">Ha gyorsítótárazni szeretné a többszöri válaszadást a lekérdezési karakterlánc paraméterei vagy a fejlécek alapján, ebben a két mezőben konfigurálhatja őket.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-128">If you want to cache multiple responses based on query string parameters or headers, you can configure them in these two fields.</span></span>

<span data-ttu-id="9a8ba-129">Az **Időtartam** megadja a gyorsítótárazott válaszok lejárati időközét.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-129">**Duration** specifies the expiration interval of the cached responses.</span></span> <span data-ttu-id="9a8ba-130">Ebben a példában az időköz **3600** másodperc, ami egy órának felel meg.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-130">In this example, the interval is **3600** seconds, which is equivalent to one hour.</span></span>

<span data-ttu-id="9a8ba-131">Ha a példa gyorsítótárazási konfigurációját használja, a **GET Resource (cached)** műveletre irányuló első kérés a háttérszolgáltatásból küld vissza választ.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-131">Using the caching configuration in this example, the first request to the **GET Resource (cached)** operation returns a response from the backend service.</span></span> <span data-ttu-id="9a8ba-132">Ez a válasz gyorsítótárazva lesz, és egy kulccsal lesz ellátva a megadott fejlécek és lekérdezési karakterlánc paraméterek alapján.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-132">This response will be cached, keyed by the specified headers and query string parameters.</span></span> <span data-ttu-id="9a8ba-133">A művelet későbbi, egyező paraméterekkel rendelkező hívásai a gyorsítótárazott választ küldik vissza, egészen addig, amíg a gyorsítótárazás időköze le nem jár.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-133">Subsequent calls to the operation, with matching parameters, will have the cached response returned, until the cache duration interval has expired.</span></span>

## <span data-ttu-id="9a8ba-134"><a name="caching-policies"> </a>A gyorsítótárazási házirendek áttekintése</span><span class="sxs-lookup"><span data-stu-id="9a8ba-134"><a name="caching-policies"> </a>Review the caching policies</span></span>
<span data-ttu-id="9a8ba-135">Ebben a lépésben a minta Echo API **GET Resource (cached)** műveletének gyorsítótárazási beállításait ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-135">In this step, you review the caching settings for the **GET Resource (cached)** operation of the sample Echo API.</span></span>

<span data-ttu-id="9a8ba-136">Amikor a **Gyorsítótárazás** lapon konfigurálják egy művelet gyorsítótárazási beállításait, a rendszer gyorsítótárazási házirendeket ad művelethez.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-136">When caching settings are configured for an operation on the **Caching** tab, caching policies are added for the operation.</span></span> <span data-ttu-id="9a8ba-137">Ezeket a házirendeket a házirendszerkesztőben lehet megtekinteni és szerkeszteni.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-137">These policies can be viewed and edited in the policy editor.</span></span>

<span data-ttu-id="9a8ba-138">Kattintson a bal oldali **API Management** menü **Házirendek** elemére, majd válassza ki az **Echo API / GET Resource (cached)** lehetőséget a **Művelet** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-138">Click **Policies** from the **API Management** menu on the left, and then select **Echo API / GET Resource (cached)** from the **Operation** drop-down list.</span></span>

![Házirend-hatókör művelet][api-management-operation-dropdown]

<span data-ttu-id="9a8ba-140">Ez megjeleníti a művelet házirendjeit a házirendszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-140">This displays the policies for this operation in the policy editor.</span></span>

![API Management házirendszerkesztő][api-management-policy-editor]

<span data-ttu-id="9a8ba-142">A művelet házirend-definíciójába beletartoznak azok a házirendek is, amelyek az előző lépésben a **Gyorsítótárazás** lappal ellenőrzött gyorsítótárazási konfigurációt definiálják.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-142">The policy definition for this operation includes the policies that define the caching configuration that were reviewed using the **Caching** tab in the previous step.</span></span>

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
> <span data-ttu-id="9a8ba-143">A gyorsítótárazási házirendek házirendszerkesztőben végzett módosításai meg fognak jelenni a művelet **Gyorsítótárazás** oldalán, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-143">Changes made to the caching policies in the policy editor will be reflected on the **Caching** tab of an operation, and vice-versa.</span></span>
> 
> 

## <span data-ttu-id="9a8ba-144"><a name="test-operation"> </a>Művelet meghívása és a gyorsítótárazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="9a8ba-144"><a name="test-operation"> </a>Call an operation and test the caching</span></span>
<span data-ttu-id="9a8ba-145">A gyorsítótárazás működés közbeni megtekintéséhez meghívhatjuk a műveletet a fejlesztői portálról.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-145">To see the caching in action, we can call the operation from the developer portal.</span></span> <span data-ttu-id="9a8ba-146">Kattintson a **Fejlesztői portál** lehetőségre a jobb felső menüben.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-146">Click **Developer portal** in the top right menu.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="9a8ba-148">Kattintson az **API-k** elemre a felső menüben, majd válassza az **Echo API** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-148">Click **APIs** in the top menu, and then select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> <span data-ttu-id="9a8ba-150">Ha csak egy API van konfigurálva, vagy csak egy API látható a fiókja számára, és rákattint az API-k elemre, az közvetlenül az API-hoz tartozó művelethez fogja vinni.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-150">If you have only one API configured or visible to your account, then clicking APIs takes you directly to the operations for that API.</span></span>
> 
> 

<span data-ttu-id="9a8ba-151">Válassza ki a **GET Resource (cached)** műveletet, majd kattintson a **Konzol megnyitása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-151">Select the **GET Resource (cached)** operation, and then click **Open Console**.</span></span>

![Konzol megnyitása][api-management-open-console]

<span data-ttu-id="9a8ba-153">A konzol lehetővé teszi, hogy műveleteket hívjon meg közvetlenül a fejlesztői portálról.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-153">The console allows you to invoke operations directly from the developer portal.</span></span>

![Konzol][api-management-console]

<span data-ttu-id="9a8ba-155">Tartsa meg a **param1** és a **param2** alapértelmezett értékeit.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-155">Keep the default values for **param1** and **param2**.</span></span>

<span data-ttu-id="9a8ba-156">Válassza ki a kívánt kulcsot a **subscription-key** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-156">Select the desired key from the **subscription-key** drop-down list.</span></span> <span data-ttu-id="9a8ba-157">Ha a fiókja csak egyetlen előfizetéssel rendelkezik, akkor az alapból ki lesz választva.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-157">If your account has only one subscription, it will already be selected.</span></span>

<span data-ttu-id="9a8ba-158">Írja be a **sampleheader:value1** kifejezést a **Kérésfejlécek** szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-158">Enter **sampleheader:value1** in the **Request headers** text box.</span></span>

<span data-ttu-id="9a8ba-159">Kattintson a **HTTP Get** lehetőségre, és jegyezze fel a válaszfejléceket.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-159">Click **HTTP Get** and make a note of the response headers.</span></span>

<span data-ttu-id="9a8ba-160">Írja be a **sampleheader:value2** kifejezést a **Kérésfejlécek** szövegmezőbe, majd kattintson a **HTTP Get** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-160">Enter **sampleheader:value2** in the **Request headers** text box, and then click **HTTP Get**.</span></span>

<span data-ttu-id="9a8ba-161">Vegye figyelembe, hogy a **sampleheader** értéke a válaszban is **value1**.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-161">Note that the value of **sampleheader** is still **value1** in the response.</span></span> <span data-ttu-id="9a8ba-162">Próbáljon ki különböző értékeket, és jegyezze fel az első visszaküldött hívás gyorsítótárazott válaszát.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-162">Try some different values and note that the cached response from the first call is returned.</span></span>

<span data-ttu-id="9a8ba-163">Írja be a **25** értéket a **param2** mezőbe, majd kattintson a **HTTP Get** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-163">Enter **25** into the **param2** field, and then click **HTTP Get**.</span></span>

<span data-ttu-id="9a8ba-164">Vegye figyelembe, hogy a **sampleheader** értéke a válaszban most már **value2**.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-164">Note that the value of **sampleheader** in the response is now **value2**.</span></span> <span data-ttu-id="9a8ba-165">Mivel a művelet eredményei egy kulccsal vannak ellátva a lekérdezési karakterlánc alapján, a rendszer nem küldte vissza az előző gyorsítótárazott választ.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-165">Because the operation results are keyed by query string, the previous cached response was not returned.</span></span>

## <span data-ttu-id="9a8ba-166"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a8ba-166"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="9a8ba-167">További információt a gyorsítótárazási házirendekről az [API Management házirend-referencia][API Management policy reference] oktatóanyag [Gyorsítótárazási házirendek][Caching policies] szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="9a8ba-167">For more information about caching policies, see [Caching policies][Caching policies] in the [API Management policy reference][API Management policy reference].</span></span>
* <span data-ttu-id="9a8ba-168">További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).</span><span class="sxs-lookup"><span data-stu-id="9a8ba-168">For information on caching items by key using policy expressions, see [Custom caching in Azure API Management](api-management-sample-cache-by-key.md).</span></span>

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


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
