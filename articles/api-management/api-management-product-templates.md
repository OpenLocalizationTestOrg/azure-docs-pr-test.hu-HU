---
title: "A termék sablonok az Azure API Management |} Microsoft Docs"
description: "Ismerje meg, hogyan szabhatja testre a tartalom az Azure API Management fejlesztői portálján a termék oldalát."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 9ddbb9860b437cb3e7334bdf5891f2fba1cffb76
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="product-templates-in-azure-api-management"></a><span data-ttu-id="84048-103">A termék sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="84048-103">Product templates in Azure API Management</span></span>
<span data-ttu-id="84048-104">Az Azure API Management lehetővé teszi a tartalom developer portálon lapok használatával konfigurálhatja a tartalom-sablonok testreszabása.</span><span class="sxs-lookup"><span data-stu-id="84048-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="84048-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és az Ön által választott szerkesztőben, mint [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), konfigurálja a tartalmat, a lapok, ahogyan szeretné ezeket a sablonokat használ nagy rugalmasságot biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="84048-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="84048-106">Ebben a szakaszban a sablonok engedélyezi, hogy a tartalom a fejlesztői portálján a termék oldalát testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="84048-106">The templates in this section allow you to customize the content of the product pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="84048-107">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="84048-107">Product list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="84048-108">A termék</span><span class="sxs-lookup"><span data-stu-id="84048-108">Product</span></span>](#Product)  
  
> [!NOTE]
>  <span data-ttu-id="84048-109">Minta alapértelmezett sablonok az alábbi dokumentáció szerepelnek, de folyamatos fejlesztéseket miatt változhat.</span><span class="sxs-lookup"><span data-stu-id="84048-109">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="84048-110">Megtekintheti az élő alapértelmezett sablonok a fejlesztői portálra nyissa meg a kívánt egyéni sablonokat.</span><span class="sxs-lookup"><span data-stu-id="84048-110">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="84048-111">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="84048-111">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="84048-112"><a name="ProductList"></a>Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="84048-112"><a name="ProductList"></a> Product list</span></span>  
 <span data-ttu-id="84048-113">A **termékek listáját** sablon lehetővé teszi a fejlesztői portálra a termék lista lap törzsében testreszabását.</span><span class="sxs-lookup"><span data-stu-id="84048-113">The **Product list** template allows you to customize the body of the product list page in the developer portal.</span></span>  
  
 <span data-ttu-id="84048-114">![Termékek listáját](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span><span class="sxs-lookup"><span data-stu-id="84048-114">![Products list](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="84048-115">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="84048-115">Default template</span></span>  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if products.size > 0 %}  
    <ul class="list-unstyled">  
    {% for product in products %}  
        <li>  
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>  
            {{product.description}}  
        </li>     
    {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="84048-116">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="84048-116">Controls</span></span>  
 <span data-ttu-id="84048-117">A `Product list` sablon előfordulhat, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="84048-117">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="84048-118">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="84048-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="84048-119">keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="84048-119">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="84048-120">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="84048-120">Data model</span></span>  
  
|<span data-ttu-id="84048-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="84048-121">Property</span></span>|<span data-ttu-id="84048-122">Típus</span><span class="sxs-lookup"><span data-stu-id="84048-122">Type</span></span>|<span data-ttu-id="84048-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="84048-123">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="84048-124">Lapozás</span><span class="sxs-lookup"><span data-stu-id="84048-124">Paging</span></span>|<span data-ttu-id="84048-125">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="84048-125">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="84048-126">A termékek gyűjtemény lapozás adatait.</span><span class="sxs-lookup"><span data-stu-id="84048-126">The paging information for the products collection.</span></span>|  
|<span data-ttu-id="84048-127">Szűrés</span><span class="sxs-lookup"><span data-stu-id="84048-127">Filtering</span></span>|<span data-ttu-id="84048-128">[Szűrés](api-management-template-data-model-reference.md#Filtering) entitás.</span><span class="sxs-lookup"><span data-stu-id="84048-128">[Filtering](api-management-template-data-model-reference.md#Filtering) entity.</span></span>|<span data-ttu-id="84048-129">A termékek lista lap kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="84048-129">The filtering information for the products list page.</span></span>|  
|<span data-ttu-id="84048-130">Termékek</span><span class="sxs-lookup"><span data-stu-id="84048-130">Products</span></span>|<span data-ttu-id="84048-131">A gyűjtemény [termék](api-management-template-data-model-reference.md#Product) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="84048-131">Collection of [Product](api-management-template-data-model-reference.md#Product) entities.</span></span>|<span data-ttu-id="84048-132">A termékek, az aktuális felhasználó számára látható.</span><span class="sxs-lookup"><span data-stu-id="84048-132">The products visible to the current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="84048-133">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="84048-133">Sample template data</span></span>  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 2,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Filtering": {  
        "Pattern": null,  
        "Placeholder": "Search products"  
    },  
    "Products": [  
        {  
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="84048-134"><a name="Product"></a>A termék</span><span class="sxs-lookup"><span data-stu-id="84048-134"><a name="Product"></a> Product</span></span>  
 <span data-ttu-id="84048-135">A **termék** sablon lehetővé teszi a termék oldalát a fejlesztői portálra törzsét testreszabását.</span><span class="sxs-lookup"><span data-stu-id="84048-135">The **Product** template allows you to customize the body of the product page in the developer portal.</span></span>  
  
 <span data-ttu-id="84048-136">![Fejlesztői portál termékoldalára](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span><span class="sxs-lookup"><span data-stu-id="84048-136">![Developer portal product page](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="84048-137">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="84048-137">Default template</span></span>  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a><span data-ttu-id="84048-138">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="84048-138">Controls</span></span>  
 <span data-ttu-id="84048-139">A `Product list` sablon előfordulhat, hogy használja a következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="84048-139">The `Product list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="84048-140">előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="84048-140">subscribe-button</span></span>](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a><span data-ttu-id="84048-141">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="84048-141">Data model</span></span>  
  
|<span data-ttu-id="84048-142">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="84048-142">Property</span></span>|<span data-ttu-id="84048-143">Típus</span><span class="sxs-lookup"><span data-stu-id="84048-143">Type</span></span>|<span data-ttu-id="84048-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="84048-144">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="84048-145">Product</span><span class="sxs-lookup"><span data-stu-id="84048-145">Product</span></span>|[<span data-ttu-id="84048-146">A termék</span><span class="sxs-lookup"><span data-stu-id="84048-146">Product</span></span>](api-management-template-data-model-reference.md#Product)|<span data-ttu-id="84048-147">A meghatározott termék.</span><span class="sxs-lookup"><span data-stu-id="84048-147">The specified product.</span></span>|  
|<span data-ttu-id="84048-148">IsDeveloperSubscribed</span><span class="sxs-lookup"><span data-stu-id="84048-148">IsDeveloperSubscribed</span></span>|<span data-ttu-id="84048-149">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="84048-149">boolean</span></span>|<span data-ttu-id="84048-150">Hogy az aktuális felhasználó számára a termék elő van fizetve.</span><span class="sxs-lookup"><span data-stu-id="84048-150">Whether the current user is subscribed to this product.</span></span>|  
|<span data-ttu-id="84048-151">SubscriptionState</span><span class="sxs-lookup"><span data-stu-id="84048-151">SubscriptionState</span></span>|<span data-ttu-id="84048-152">Szám</span><span class="sxs-lookup"><span data-stu-id="84048-152">number</span></span>|<span data-ttu-id="84048-153">Az előfizetés állapotát.</span><span class="sxs-lookup"><span data-stu-id="84048-153">The state of the subscription.</span></span> <span data-ttu-id="84048-154">Lehetséges állapota van:</span><span class="sxs-lookup"><span data-stu-id="84048-154">Possible states are:</span></span><br /><br /> <span data-ttu-id="84048-155">-   `0 - suspended`– az előfizetés le van tiltva, és az előfizető nem hívható meg a termék bármely API-k.</span><span class="sxs-lookup"><span data-stu-id="84048-155">-   `0 - suspended` – the subscription is blocked, and the subscriber cannot call any APIs of the product.</span></span><br /><span data-ttu-id="84048-156">-   `1 - active`– az előfizetés nem aktív.</span><span class="sxs-lookup"><span data-stu-id="84048-156">-   `1 - active` – the subscription is active.</span></span><br /><span data-ttu-id="84048-157">-   `2 - expired`– az előfizetés elérte a lejárat és inaktiváltuk.</span><span class="sxs-lookup"><span data-stu-id="84048-157">-   `2 - expired` – the subscription reached its expiration date and was deactivated.</span></span><br /><span data-ttu-id="84048-158">-   `3 - submitted`– az előfizetési kérelem a fejlesztő történt, de még nem jóváhagyták vagy elutasították.</span><span class="sxs-lookup"><span data-stu-id="84048-158">-   `3 - submitted` – the subscription request has been made by the developer, but has not yet been approved or rejected.</span></span><br /><span data-ttu-id="84048-159">-   `4 - rejected`– az előfizetési kérelem rendszergazda meg lett tagadva.</span><span class="sxs-lookup"><span data-stu-id="84048-159">-   `4 - rejected` – the subscription request has been denied by an administrator.</span></span><br /><span data-ttu-id="84048-160">-   `5 - cancelled`– az előfizetés a fejlesztői vagy a rendszergazda megszakította.</span><span class="sxs-lookup"><span data-stu-id="84048-160">-   `5 - cancelled` – the subscription has been cancelled by the developer or administrator.</span></span>|  
|<span data-ttu-id="84048-161">Korlátok</span><span class="sxs-lookup"><span data-stu-id="84048-161">Limits</span></span>|<span data-ttu-id="84048-162">A tömb</span><span class="sxs-lookup"><span data-stu-id="84048-162">array</span></span>|<span data-ttu-id="84048-163">Ez a tulajdonság elavult, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="84048-163">This property is deprecated and should not be used.</span></span>|  
|<span data-ttu-id="84048-164">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="84048-164">DelegatedSubscriptionEnabled</span></span>|<span data-ttu-id="84048-165">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="84048-165">boolean</span></span>|<span data-ttu-id="84048-166">E [delegálás](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) engedélyezve van ennél az előfizetésnél.</span><span class="sxs-lookup"><span data-stu-id="84048-166">Whether [delegation](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is enabled for this subscription.</span></span>|  
|<span data-ttu-id="84048-167">DelegatedSubscriptionUrl</span><span class="sxs-lookup"><span data-stu-id="84048-167">DelegatedSubscriptionUrl</span></span>|<span data-ttu-id="84048-168">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="84048-168">string</span></span>|<span data-ttu-id="84048-169">Ha delegálás engedélyezve van, a meghatalmazott előfizetés URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="84048-169">If delegation is enabled, the delegated subscription URL.</span></span>|  
|<span data-ttu-id="84048-170">IsAgreed</span><span class="sxs-lookup"><span data-stu-id="84048-170">IsAgreed</span></span>|<span data-ttu-id="84048-171">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="84048-171">boolean</span></span>|<span data-ttu-id="84048-172">Ha a termék feltételeket, hogy az aktuális felhasználó elfogadta a feltételeket.</span><span class="sxs-lookup"><span data-stu-id="84048-172">If the product has terms, whether the current user has agreed to the terms.</span></span>|  
|<span data-ttu-id="84048-173">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="84048-173">Subscriptions</span></span>|<span data-ttu-id="84048-174">A gyűjtemény [előfizetés összegzés](api-management-template-data-model-reference.md#SubscriptionSummary) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="84048-174">Collection of [Subscription summary](api-management-template-data-model-reference.md#SubscriptionSummary) entities.</span></span>|<span data-ttu-id="84048-175">A termék az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="84048-175">The subscriptions to the product.</span></span>|  
|<span data-ttu-id="84048-176">API-k</span><span class="sxs-lookup"><span data-stu-id="84048-176">Apis</span></span>|<span data-ttu-id="84048-177">A gyűjtemény [API](api-management-template-data-model-reference.md#API) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="84048-177">Collection of [API](api-management-template-data-model-reference.md#API) entities.</span></span>|<span data-ttu-id="84048-178">Az API-k ennek a terméknek.</span><span class="sxs-lookup"><span data-stu-id="84048-178">The APIs in this product.</span></span>|  
|<span data-ttu-id="84048-179">CannotAddBecauseSubscriptionNumberLimitReached</span><span class="sxs-lookup"><span data-stu-id="84048-179">CannotAddBecauseSubscriptionNumberLimitReached</span></span>|<span data-ttu-id="84048-180">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="84048-180">boolean</span></span>|<span data-ttu-id="84048-181">Az aktuális felhasználónak-e előfizetni tekintetében az előfizetési határértéket a termék jogosult.</span><span class="sxs-lookup"><span data-stu-id="84048-181">Whether the current user is eligible to subscribe to this product with regard to the subscription limit.</span></span>|  
|<span data-ttu-id="84048-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span><span class="sxs-lookup"><span data-stu-id="84048-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span></span>|<span data-ttu-id="84048-183">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="84048-183">boolean</span></span>|<span data-ttu-id="84048-184">Az aktuális felhasználónak-e előfizetni tekintetében több előfizetést, vagy nem megengedett a termék jogosult.</span><span class="sxs-lookup"><span data-stu-id="84048-184">Whether the current user is eligible to subscribe to this product with regard to multiple subscriptions being allowed or not.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="84048-185">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="84048-185">Sample template data</span></span>  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a><span data-ttu-id="84048-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84048-186">Next steps</span></span>
<span data-ttu-id="84048-187">A sablonok használatának kapcsolatos további információkért lásd: [hogyan szabhatja testre a sablonok segítségével az API Management fejlesztői portálján](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="84048-187">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>