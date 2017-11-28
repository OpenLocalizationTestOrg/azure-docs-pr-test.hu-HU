---
title: az Azure API Management aaaProduct sablonok |} Microsoft Docs
description: "Ismerje meg, hogyan toocustomize hello tartalom hello termék lapok: hello Azure API Management developer portálon."
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
ms.openlocfilehash: 60600299287aad87f9b621782ab5ceb866601d03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="product-templates-in-azure-api-management"></a><span data-ttu-id="788c4-103">A termék sablonok az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="788c4-103">Product templates in Azure API Management</span></span>
<span data-ttu-id="788c4-104">Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának.</span><span class="sxs-lookup"><span data-stu-id="788c4-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="788c4-105">Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="788c4-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="788c4-106">Ebben a szakaszban hello sablonok lehetővé teszik hello termék lapok tartalmát toocustomize hello hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="788c4-106">hello templates in this section allow you toocustomize hello content of hello product pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="788c4-107">Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="788c4-107">Product list</span></span>](#ProductList)  
  
-   [<span data-ttu-id="788c4-108">A termék</span><span class="sxs-lookup"><span data-stu-id="788c4-108">Product</span></span>](#Product)  
  
> [!NOTE]
>  <span data-ttu-id="788c4-109">Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt.</span><span class="sxs-lookup"><span data-stu-id="788c4-109">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="788c4-110">Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="788c4-110">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="788c4-111">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="788c4-111">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="788c4-112"><a name="ProductList"></a>Termékek listáját</span><span class="sxs-lookup"><span data-stu-id="788c4-112"><a name="ProductList"></a> Product list</span></span>  
 <span data-ttu-id="788c4-113">Hello **termékek listáját** sablon teszi hello termék lista lap toocustomize hello törzsében hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="788c4-113">hello **Product list** template allows you toocustomize hello body of hello product list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="788c4-114">![Termékek listáját](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span><span class="sxs-lookup"><span data-stu-id="788c4-114">![Products list](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="788c4-115">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="788c4-115">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="788c4-116">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="788c4-116">Controls</span></span>  
 <span data-ttu-id="788c4-117">Hello `Product list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="788c4-117">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="788c4-118">lapozófájl-vezérlő</span><span class="sxs-lookup"><span data-stu-id="788c4-118">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="788c4-119">keresési-vezérlő</span><span class="sxs-lookup"><span data-stu-id="788c4-119">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="788c4-120">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="788c4-120">Data model</span></span>  
  
|<span data-ttu-id="788c4-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="788c4-121">Property</span></span>|<span data-ttu-id="788c4-122">Típus</span><span class="sxs-lookup"><span data-stu-id="788c4-122">Type</span></span>|<span data-ttu-id="788c4-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="788c4-123">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="788c4-124">Lapozás</span><span class="sxs-lookup"><span data-stu-id="788c4-124">Paging</span></span>|<span data-ttu-id="788c4-125">[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.</span><span class="sxs-lookup"><span data-stu-id="788c4-125">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="788c4-126">hello lapozás információ hello termékek gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="788c4-126">hello paging information for hello products collection.</span></span>|  
|<span data-ttu-id="788c4-127">Szűrés</span><span class="sxs-lookup"><span data-stu-id="788c4-127">Filtering</span></span>|<span data-ttu-id="788c4-128">[Szűrés](api-management-template-data-model-reference.md#Filtering) entitás.</span><span class="sxs-lookup"><span data-stu-id="788c4-128">[Filtering](api-management-template-data-model-reference.md#Filtering) entity.</span></span>|<span data-ttu-id="788c4-129">hello szűrő információt hello termékek lap felsorolja a listában.</span><span class="sxs-lookup"><span data-stu-id="788c4-129">hello filtering information for hello products list page.</span></span>|  
|<span data-ttu-id="788c4-130">Termékek</span><span class="sxs-lookup"><span data-stu-id="788c4-130">Products</span></span>|<span data-ttu-id="788c4-131">A gyűjtemény [termék](api-management-template-data-model-reference.md#Product) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="788c4-131">Collection of [Product](api-management-template-data-model-reference.md#Product) entities.</span></span>|<span data-ttu-id="788c4-132">hello termékek látható toohello aktuális felhasználó.</span><span class="sxs-lookup"><span data-stu-id="788c4-132">hello products visible toohello current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="788c4-133">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="788c4-133">Sample template data</span></span>  
  
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
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="788c4-134"><a name="Product"></a>A termék</span><span class="sxs-lookup"><span data-stu-id="788c4-134"><a name="Product"></a> Product</span></span>  
 <span data-ttu-id="788c4-135">Hello **termék** sablon teszi toocustomize hello törzsét, termékoldala hello hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="788c4-135">hello **Product** template allows you toocustomize hello body of hello product page in hello developer portal.</span></span>  
  
 <span data-ttu-id="788c4-136">![Fejlesztői portál termékoldalára](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span><span class="sxs-lookup"><span data-stu-id="788c4-136">![Developer portal product page](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="788c4-137">Alapértelmezett sablon</span><span class="sxs-lookup"><span data-stu-id="788c4-137">Default template</span></span>  
  
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
  
### <a name="controls"></a><span data-ttu-id="788c4-138">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="788c4-138">Controls</span></span>  
 <span data-ttu-id="788c4-139">Hello `Product list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="788c4-139">hello `Product list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="788c4-140">előfizetés gomb</span><span class="sxs-lookup"><span data-stu-id="788c4-140">subscribe-button</span></span>](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a><span data-ttu-id="788c4-141">Adatmodell</span><span class="sxs-lookup"><span data-stu-id="788c4-141">Data model</span></span>  
  
|<span data-ttu-id="788c4-142">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="788c4-142">Property</span></span>|<span data-ttu-id="788c4-143">Típus</span><span class="sxs-lookup"><span data-stu-id="788c4-143">Type</span></span>|<span data-ttu-id="788c4-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="788c4-144">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="788c4-145">Product</span><span class="sxs-lookup"><span data-stu-id="788c4-145">Product</span></span>|[<span data-ttu-id="788c4-146">A termék</span><span class="sxs-lookup"><span data-stu-id="788c4-146">Product</span></span>](api-management-template-data-model-reference.md#Product)|<span data-ttu-id="788c4-147">megadott termék hello.</span><span class="sxs-lookup"><span data-stu-id="788c4-147">hello specified product.</span></span>|  
|<span data-ttu-id="788c4-148">IsDeveloperSubscribed</span><span class="sxs-lookup"><span data-stu-id="788c4-148">IsDeveloperSubscribed</span></span>|<span data-ttu-id="788c4-149">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="788c4-149">boolean</span></span>|<span data-ttu-id="788c4-150">Hello aktuális felhasználónak-e előfizetett toothis termék.</span><span class="sxs-lookup"><span data-stu-id="788c4-150">Whether hello current user is subscribed toothis product.</span></span>|  
|<span data-ttu-id="788c4-151">SubscriptionState</span><span class="sxs-lookup"><span data-stu-id="788c4-151">SubscriptionState</span></span>|<span data-ttu-id="788c4-152">Szám</span><span class="sxs-lookup"><span data-stu-id="788c4-152">number</span></span>|<span data-ttu-id="788c4-153">hello előfizetés hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="788c4-153">hello state of hello subscription.</span></span> <span data-ttu-id="788c4-154">Lehetséges állapota van:</span><span class="sxs-lookup"><span data-stu-id="788c4-154">Possible states are:</span></span><br /><br /> <span data-ttu-id="788c4-155">-   `0 - suspended`– hello előfizetés le van tiltva, és hello előfizető hello termék bármely API nem hívható meg.</span><span class="sxs-lookup"><span data-stu-id="788c4-155">-   `0 - suspended` – hello subscription is blocked, and hello subscriber cannot call any APIs of hello product.</span></span><br /><span data-ttu-id="788c4-156">-   `1 - active`– hello előfizetés már aktív.</span><span class="sxs-lookup"><span data-stu-id="788c4-156">-   `1 - active` – hello subscription is active.</span></span><br /><span data-ttu-id="788c4-157">-   `2 - expired`– hello előfizetés elérte a lejárat és inaktiváltuk.</span><span class="sxs-lookup"><span data-stu-id="788c4-157">-   `2 - expired` – hello subscription reached its expiration date and was deactivated.</span></span><br /><span data-ttu-id="788c4-158">-   `3 - submitted`– hello előfizetés kérelem hello fejlesztő történt, de még nem jóváhagyták vagy elutasították.</span><span class="sxs-lookup"><span data-stu-id="788c4-158">-   `3 - submitted` – hello subscription request has been made by hello developer, but has not yet been approved or rejected.</span></span><br /><span data-ttu-id="788c4-159">-   `4 - rejected`– hello előfizetés kérelem rendszergazda meg lett tagadva.</span><span class="sxs-lookup"><span data-stu-id="788c4-159">-   `4 - rejected` – hello subscription request has been denied by an administrator.</span></span><br /><span data-ttu-id="788c4-160">-   `5 - cancelled`– hello előfizetés hello fejlesztői vagy a rendszergazda megszakította.</span><span class="sxs-lookup"><span data-stu-id="788c4-160">-   `5 - cancelled` – hello subscription has been cancelled by hello developer or administrator.</span></span>|  
|<span data-ttu-id="788c4-161">Korlátok</span><span class="sxs-lookup"><span data-stu-id="788c4-161">Limits</span></span>|<span data-ttu-id="788c4-162">A tömb</span><span class="sxs-lookup"><span data-stu-id="788c4-162">array</span></span>|<span data-ttu-id="788c4-163">Ez a tulajdonság elavult, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="788c4-163">This property is deprecated and should not be used.</span></span>|  
|<span data-ttu-id="788c4-164">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="788c4-164">DelegatedSubscriptionEnabled</span></span>|<span data-ttu-id="788c4-165">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="788c4-165">boolean</span></span>|<span data-ttu-id="788c4-166">E [delegálás](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) engedélyezve van ennél az előfizetésnél.</span><span class="sxs-lookup"><span data-stu-id="788c4-166">Whether [delegation](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is enabled for this subscription.</span></span>|  
|<span data-ttu-id="788c4-167">DelegatedSubscriptionUrl</span><span class="sxs-lookup"><span data-stu-id="788c4-167">DelegatedSubscriptionUrl</span></span>|<span data-ttu-id="788c4-168">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="788c4-168">string</span></span>|<span data-ttu-id="788c4-169">Ha engedélyezve van a delegálást, hello meghatalmazott előfizetés URL-címet.</span><span class="sxs-lookup"><span data-stu-id="788c4-169">If delegation is enabled, hello delegated subscription URL.</span></span>|  
|<span data-ttu-id="788c4-170">IsAgreed</span><span class="sxs-lookup"><span data-stu-id="788c4-170">IsAgreed</span></span>|<span data-ttu-id="788c4-171">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="788c4-171">boolean</span></span>|<span data-ttu-id="788c4-172">Ha hello termék feltételeket, hogy hello aktuális felhasználó hozzájárult toohello feltételeit.</span><span class="sxs-lookup"><span data-stu-id="788c4-172">If hello product has terms, whether hello current user has agreed toohello terms.</span></span>|  
|<span data-ttu-id="788c4-173">Előfizetések</span><span class="sxs-lookup"><span data-stu-id="788c4-173">Subscriptions</span></span>|<span data-ttu-id="788c4-174">A gyűjtemény [előfizetés összegzés](api-management-template-data-model-reference.md#SubscriptionSummary) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="788c4-174">Collection of [Subscription summary](api-management-template-data-model-reference.md#SubscriptionSummary) entities.</span></span>|<span data-ttu-id="788c4-175">hello előfizetések toohello termék.</span><span class="sxs-lookup"><span data-stu-id="788c4-175">hello subscriptions toohello product.</span></span>|  
|<span data-ttu-id="788c4-176">API-k</span><span class="sxs-lookup"><span data-stu-id="788c4-176">Apis</span></span>|<span data-ttu-id="788c4-177">A gyűjtemény [API](api-management-template-data-model-reference.md#API) entitásokat.</span><span class="sxs-lookup"><span data-stu-id="788c4-177">Collection of [API](api-management-template-data-model-reference.md#API) entities.</span></span>|<span data-ttu-id="788c4-178">hello API-k fel a termékkel.</span><span class="sxs-lookup"><span data-stu-id="788c4-178">hello APIs in this product.</span></span>|  
|<span data-ttu-id="788c4-179">CannotAddBecauseSubscriptionNumberLimitReached</span><span class="sxs-lookup"><span data-stu-id="788c4-179">CannotAddBecauseSubscriptionNumberLimitReached</span></span>|<span data-ttu-id="788c4-180">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="788c4-180">boolean</span></span>|<span data-ttu-id="788c4-181">Hello aktuális felhasználónak-e jogosult toosubscribe toothis termék legutóbb toohello előfizetési korlátját.</span><span class="sxs-lookup"><span data-stu-id="788c4-181">Whether hello current user is eligible toosubscribe toothis product with regard toohello subscription limit.</span></span>|  
|<span data-ttu-id="788c4-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span><span class="sxs-lookup"><span data-stu-id="788c4-182">CannotAddBecauseMultipleSubscriptionsNotAllowed</span></span>|<span data-ttu-id="788c4-183">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="788c4-183">boolean</span></span>|<span data-ttu-id="788c4-184">Hello aktuális felhasználónak-e jogosult toosubscribe toothis termék legutóbb toomultiple előfizetésekkel, vagy nem megengedett.</span><span class="sxs-lookup"><span data-stu-id="788c4-184">Whether hello current user is eligible toosubscribe toothis product with regard toomultiple subscriptions being allowed or not.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="788c4-185">Mintaadatokat sablon</span><span class="sxs-lookup"><span data-stu-id="788c4-185">Sample template data</span></span>  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",  
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

## <a name="next-steps"></a><span data-ttu-id="788c4-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="788c4-186">Next steps</span></span>
<span data-ttu-id="788c4-187">A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="788c4-187">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
