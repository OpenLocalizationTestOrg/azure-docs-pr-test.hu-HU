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
# <a name="product-templates-in-azure-api-management"></a>A termék sablonok az Azure API Management
Az Azure API Management biztosít, akkor hello képességét toocustomize hello fejlesztői portál lapok használatával konfigurálhatja a tartalom-sablonok tartalmának. Használatával [DotLiquid](http://dotliquidmarkup.org/) szintaxisát és hello szerkesztő az Ön által választott, például a [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), és a megadott készlete honosított [karakterlánc-erőforrások](api-management-template-resources.md#strings), [ A betűkép-erőforrások](api-management-template-resources.md#glyphs), és [vezérlők lapon](api-management-page-controls.md), rugalmas lehetőségeket biztosítanak tooconfigure hello hello lapok tartalmát rendelkezik, ezeket a sablonokat igényei szerint.  
  
 Ebben a szakaszban hello sablonok lehetővé teszik hello termék lapok tartalmát toocustomize hello hello developer portálon.  
  
-   [Termékek listáját](#ProductList)  
  
-   [A termék](#Product)  
  
> [!NOTE]
>  Minta alapértelmezett sablonok a következő dokumentáció hello szerepelnek, de tulajdonos toochange toocontinuous fejlesztései miatt. Navigáljon a szükséges toohello egyéni sablonok hello élő alapértelmezett sablonok a hello fejlesztői portálján tekintheti meg. A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
##  <a name="ProductList"></a>Termékek listáját  
 Hello **termékek listáját** sablon teszi hello termék lista lap toocustomize hello törzsében hello developer portálon.  
  
 ![Termékek listáját](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Alapértelmezett sablon  
  
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
  
### <a name="controls"></a>Vezérlők  
 Hello `Product list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).  
  
-   [lapozófájl-vezérlő](api-management-page-controls.md#paging-control)  
  
-   [keresési-vezérlő](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Adatmodell  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Lapozás|[Lapozás](api-management-template-data-model-reference.md#Paging) entitás.|hello lapozás információ hello termékek gyűjtemény.|  
|Szűrés|[Szűrés](api-management-template-data-model-reference.md#Filtering) entitás.|hello szűrő információt hello termékek lap felsorolja a listában.|  
|Termékek|A gyűjtemény [termék](api-management-template-data-model-reference.md#Product) entitásokat.|hello termékek látható toohello aktuális felhasználó.|  
  
### <a name="sample-template-data"></a>Mintaadatokat sablon  
  
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
  
##  <a name="Product"></a>A termék  
 Hello **termék** sablon teszi toocustomize hello törzsét, termékoldala hello hello developer portálon.  
  
 ![Fejlesztői portál termékoldalára](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Alapértelmezett sablon  
  
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
  
### <a name="controls"></a>Vezérlők  
 Hello `Product list` sablon használhat hello következő [vezérlők lapon](api-management-page-controls.md).  
  
-   [előfizetés gomb](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Adatmodell  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Product|[A termék](api-management-template-data-model-reference.md#Product)|megadott termék hello.|  
|IsDeveloperSubscribed|Logikai érték|Hello aktuális felhasználónak-e előfizetett toothis termék.|  
|SubscriptionState|Szám|hello előfizetés hello állapotát. Lehetséges állapota van:<br /><br /> -   `0 - suspended`– hello előfizetés le van tiltva, és hello előfizető hello termék bármely API nem hívható meg.<br />-   `1 - active`– hello előfizetés már aktív.<br />-   `2 - expired`– hello előfizetés elérte a lejárat és inaktiváltuk.<br />-   `3 - submitted`– hello előfizetés kérelem hello fejlesztő történt, de még nem jóváhagyták vagy elutasították.<br />-   `4 - rejected`– hello előfizetés kérelem rendszergazda meg lett tagadva.<br />-   `5 - cancelled`– hello előfizetés hello fejlesztői vagy a rendszergazda megszakította.|  
|Korlátok|A tömb|Ez a tulajdonság elavult, és nem használható.|  
|DelegatedSubscriptionEnabled|Logikai érték|E [delegálás](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) engedélyezve van ennél az előfizetésnél.|  
|DelegatedSubscriptionUrl|Karakterlánc|Ha engedélyezve van a delegálást, hello meghatalmazott előfizetés URL-címet.|  
|IsAgreed|Logikai érték|Ha hello termék feltételeket, hogy hello aktuális felhasználó hozzájárult toohello feltételeit.|  
|Előfizetések|A gyűjtemény [előfizetés összegzés](api-management-template-data-model-reference.md#SubscriptionSummary) entitásokat.|hello előfizetések toohello termék.|  
|API-k|A gyűjtemény [API](api-management-template-data-model-reference.md#API) entitásokat.|hello API-k fel a termékkel.|  
|CannotAddBecauseSubscriptionNumberLimitReached|Logikai érték|Hello aktuális felhasználónak-e jogosult toosubscribe toothis termék legutóbb toohello előfizetési korlátját.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|Logikai érték|Hello aktuális felhasználónak-e jogosult toosubscribe toothis termék legutóbb toomultiple előfizetésekkel, vagy nem megengedett.|  
  
### <a name="sample-template-data"></a>Mintaadatokat sablon  
  
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

## <a name="next-steps"></a>Következő lépések
A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).
