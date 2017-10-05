---
title: "Hogyan hozhat létre, és a termék közzététele az Azure API Management"
description: "Ismerje meg, hogyan hozhat létre és termékek közzététele az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73bf4451ba1b71807e22440beecc73a7e8045c5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a><span data-ttu-id="6a8e1-103">Hogyan hozhat létre, és a termék közzététele az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="6a8e1-103">How to create and publish a product in Azure API Management</span></span>
<span data-ttu-id="6a8e1-104">Az Azure API Management a termék egy vagy több API-k, valamint a memóriahasználati kvóta és a használati feltételeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-104">In Azure API Management, a product contains one or more APIs as well as a usage quota and the terms of use.</span></span> <span data-ttu-id="6a8e1-105">Miután közzétette a termék, a fejlesztők a termék előfizetni és a termék API-k használatának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-105">Once a product is published, developers can subscribe to the product and begin to use the product's APIs.</span></span> <span data-ttu-id="6a8e1-106">A témakör való termék létrehozása, az API-k hozzáadása, és közzéteheti azt a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-106">The topic provides a guide to creating a product, adding an API, and publishing it for developers.</span></span>

## <span data-ttu-id="6a8e1-107"><a name="create-product"></a>Termék létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a8e1-107"><a name="create-product"> </a>Create a product</span></span>
<span data-ttu-id="6a8e1-108">Műveletek felvétele, illetve egy API-t, a közzétevő portálon konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-108">Operations are added and configured to an API in the publisher portal.</span></span> <span data-ttu-id="6a8e1-109">A közzétevő portál eléréséhez kattintson **Publisher portal** az Azure portálon az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-109">To access the publisher portal, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="6a8e1-111">Ha még nem hozott létre API Management szolgáltatáspéldányt, tekintse meg az [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management] oktatóanyag [API Management szolgáltatáspéldány létrehozása][Create an API Management service instance] című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="6a8e1-112">Kattintson a **termékek** megjelenítéséhez a bal oldali menüben a **termékek** lapon, majd kattintson **termék hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-112">Click on **Products** in the menu on the left to display the **Products** page, and click **Add Product**.</span></span>

![Termékek][api-management-products]

![Új termék][api-management-add-new-product]

<span data-ttu-id="6a8e1-115">Adjon meg egy leíró nevet a termékből a **neve** mező, és a termék leírása a **leírás** mező.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-115">Enter a descriptive name for the product in the **Name** field and a description of the product in the **Description** field.</span></span>

<span data-ttu-id="6a8e1-116">Az API Management termékek lehet **nyitott** vagy **védett**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-116">Products in API Management can be **Open** or **Protected**.</span></span> <span data-ttu-id="6a8e1-117">A védett termékeket csak előfizetők használhatják, míg a nyílt termékeket előfizetés nélkül is lehet használni.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-117">Protected products must be subscribed to before they can be used, while open products can be used without a subscription.</span></span> <span data-ttu-id="6a8e1-118">Ellenőrizze **előfizetés szükséges** előfizetést igénylő védett termék létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-118">Check **Require subscription** to create a protected product that requires a subscription.</span></span> <span data-ttu-id="6a8e1-119">Ez az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-119">This is the default setting.</span></span>

<span data-ttu-id="6a8e1-120">Ellenőrizze **előfizetés jóváhagyás szükséges** Ha azt szeretné, hogy tekintse át és fogadja el vagy utasítsa el a termék előfizetés megkísérli a rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-120">Check **Require subscription approval** if you want an administrator to review and accept or reject subscription attempts to this product.</span></span> <span data-ttu-id="6a8e1-121">Ha a jelölőnégyzet nincs bejelölve, a előfizetés kísérletek lesz automatikusan jóváhagyta.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-121">If the box is unchecked, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="6a8e1-122">Az előfizetések további információkért lásd: [megtekintheti a termék-előfizetők][View subscribers to a product].</span><span class="sxs-lookup"><span data-stu-id="6a8e1-122">For more information on subscriptions, see [View subscribers to a product][View subscribers to a product].</span></span>

<span data-ttu-id="6a8e1-123">Engedélyezze a fejlesztői fiókok többször előfizetni a terméket, ellenőrizze a **több előfizetés engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-123">To allow developer accounts to subscribe multiple times to the product, check the **Allow multiple subscriptions** check box.</span></span> <span data-ttu-id="6a8e1-124">Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal a termék kérhet le.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-124">If this box is not checked, each developer account can subscribe only a single time to the product.</span></span>

![Korlátlan több előfizetéssel][api-management-unlimited-multiple-subscriptions]

<span data-ttu-id="6a8e1-126">Több egyidejű előfizetések számának korlátozása érdekében ellenőrizze a **egyidejű előfizetések számának korlátozása** jelölje be a jelölőnégyzetet, és írja be az előfizetési határértéket.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-126">To limit the count of multiple simultaneous subscriptions, check the **Limit number of simultaneous subscriptions to** check box and enter the subscription limit.</span></span> <span data-ttu-id="6a8e1-127">A következő példában egyidejű előfizetések korlátozódnak négy fejlesztői fiókonként.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-127">In the following example, simultaneous subscriptions are limited to four per developer account.</span></span>

![Négy több előfizetéssel][api-management-four-multiple-subscriptions]

<span data-ttu-id="6a8e1-129">Minden új termék beállítások konfigurálása után kattintson **mentése** az új termék létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-129">Once all new product options are configured, click **Save** to create the new product.</span></span>

![Termékek][api-management-products-page]

> <span data-ttu-id="6a8e1-131">Alapértelmezés szerint új termékek közzé nem tett, és csak a **rendszergazdák** csoport.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-131">By default new products are unpublished, and are visible only to the  **Administrators** group.</span></span>
> 
> 

<span data-ttu-id="6a8e1-132">A termék konfigurálásához kattintson a termék nevére a a **termékek** fülre.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-132">To configure a product, click on the product name in the **Products** tab.</span></span>

## <span data-ttu-id="6a8e1-133"><a name="add-apis"></a>API-k hozzáadása egy termékre</span><span class="sxs-lookup"><span data-stu-id="6a8e1-133"><a name="add-apis"> </a>Add APIs to a product</span></span>
<span data-ttu-id="6a8e1-134">A **termékek** lapon négy hivatkozása a konfigurációhoz: **összegzés**, **beállítások**, **látható**, és  **Előfizetők**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-134">The **Products** page contains four links for configuration: **Summary**, **Settings**, **Visibility**, and **Subscribers**.</span></span> <span data-ttu-id="6a8e1-135">A **összegzés** lapon látható, ahol akkor is vegye fel az API-k és közzétételét és a termék.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-135">The **Summary** tab is where you can add APIs and publish or unpublish a product.</span></span>

![Összefoglalás][api-management-new-product-summary]

<span data-ttu-id="6a8e1-137">A termék közzététele előtt kell hozzáadnia egy vagy több API-k.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-137">Before publishing your product you need to add one or more APIs.</span></span> <span data-ttu-id="6a8e1-138">Ehhez kattintson **API vegye fel a termék**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-138">To do this, click **Add API to product**.</span></span>

![API-k hozzáadása][api-management-add-apis-to-product]

<span data-ttu-id="6a8e1-140">Válassza ki a kívánt API-kat, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-140">Select the desired APIs and click **Save**.</span></span>

## <span data-ttu-id="6a8e1-141"><a name="add-description"></a>Leíró adatokat hozzáadni a termék</span><span class="sxs-lookup"><span data-stu-id="6a8e1-141"><a name="add-description"> </a>Add descriptive information to a product</span></span>
<span data-ttu-id="6a8e1-142">A **beállítások** lapon adhatja meg a termék, például a célja, hozzáférést biztosít az API-k és más hasznos információk részletes információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-142">The **Settings** tab allows you to provide detailed information about the product such as its purpose, the APIs it provides access to, and other useful information.</span></span> <span data-ttu-id="6a8e1-143">A tartalom célja a fejlesztők számára, amely hívja az API-t kell lesz, és egyszerű szöveg- vagy HTML-kódot is beírhatók.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-143">The content is targeted at the developers that will be calling the API and can be written in plain text or HTML markup.</span></span>

![A termék beállításai][api-management-product-settings]

<span data-ttu-id="6a8e1-145">Ellenőrizze **előfizetés szükséges** védett termék, amely használható, vagy törölje a jelölőnégyzet bejelölésével hozzon létre egy megnyitott terméket, amely a előfizetés nélkül meghívható előfizetését igényli létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-145">Check **Require subscription** to create a protected product that requires a subscription to be used, or clear the checkbox to create an open product that can be called without a subscription.</span></span>

<span data-ttu-id="6a8e1-146">Válassza ki **előfizetés jóváhagyás szükséges** Ha manuálisan jóváhagyja az összes termék előfizetési kérelmek.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-146">Select **Require subscription approval** if you want to manually approve all product subscription requests.</span></span> <span data-ttu-id="6a8e1-147">Alapértelmezés szerint az összes termék előfizetések automatikusan kapnak.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-147">By default all product subscriptions are granted automatically.</span></span>

<span data-ttu-id="6a8e1-148">Engedélyezze a fejlesztői fiókok többször előfizetni a terméket, ellenőrizze a **több előfizetés engedélyezése** jelölje be a jelölőnégyzetet, és opcionálisan adja meg a határértéket.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-148">To allow developer accounts to subscribe multiple times to the product, check the **Allow multiple subscriptions** check box and optionally specify a limit.</span></span> <span data-ttu-id="6a8e1-149">Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal a termék kérhet le.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-149">If this box is not checked, each developer account can subscribe only a single time to the product.</span></span>

<span data-ttu-id="6a8e1-150">Nem kötelező kitölteni a **használati feltételek** mezőt, amely leírja a termék, mely előfizetők a termék használatához el kell fogadnia a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-150">Optionally fill in the **Terms of use** field describing the terms of use for the product which subscribers must accept in order to use the product.</span></span>

## <span data-ttu-id="6a8e1-151"><a name="publish-product"></a>Termék közzététele</span><span class="sxs-lookup"><span data-stu-id="6a8e1-151"><a name="publish-product"> </a>Publish a product</span></span>
<span data-ttu-id="6a8e1-152">A termék API-k hívása előtt közzé kell tenni a terméket.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-152">Before the APIs in a product can be called, the product must be published.</span></span> <span data-ttu-id="6a8e1-153">A a **összegzés** termék lapra, majd **közzététel**, és kattintson a **Igen, azt közzé** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-153">On the **Summary** tab for the product, click **Publish**, and then click **Yes, publish it** to confirm.</span></span> <span data-ttu-id="6a8e1-154">A korábban közzétett termék titkos kattintson **Közzététel megszüntetése**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-154">To make a previously published product private, click **Unpublish**.</span></span>

![Termék közzététele][api-management-publish-product]

## <span data-ttu-id="6a8e1-156"><a name="make-visible"></a>a fejlesztők számára láthatóvá termék</span><span class="sxs-lookup"><span data-stu-id="6a8e1-156"><a name="make-visible"> </a>Make a product visible to developers</span></span>
<span data-ttu-id="6a8e1-157">A **látható** lap lehetővé teszi annak meghatározását, mely szerepkörök képesek a termék megjelenik a fejlesztői portálján, és a termék előfizetni.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-157">The **Visibility** tab allows you to choose which roles are able to see the product on the developer portal and subscribe to the product.</span></span>

![A termék látható][api-management-product-visiblity]

<span data-ttu-id="6a8e1-159">Letiltása és engedélyezése látható-e a termék a fejlesztők számára az egy csoport, ellenőrizze, vagy törölje a jelet az a csoport melletti jelölőnégyzetet, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-159">To enable or disable visibility of a product for the developers in a group, check or uncheck the check box beside the group and then click **Save**.</span></span>

> <span data-ttu-id="6a8e1-160">További információkért lásd: [létrehozásáról és csoportoknak a segítségével az Azure API Management fejlesztői fiókok kezelése][How to create and use groups to manage developer accounts in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="6a8e1-160">For more information, see [How to create and use groups to manage developer accounts in Azure API Management][How to create and use groups to manage developer accounts in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="6a8e1-161"><a name="view-subscribers"></a>Termék-előfizetők megtekintése</span><span class="sxs-lookup"><span data-stu-id="6a8e1-161"><a name="view-subscribers"> </a>View subscribers to a product</span></span>
<span data-ttu-id="6a8e1-162">A **előfizetők** lap felsorolja a fejlesztők számára a termék előfizetett.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-162">The **Subscribers** tab lists the developers who have subscribed to the product.</span></span> <span data-ttu-id="6a8e1-163">A részletek és minden fejlesztői beállításait a a fejlesztői nevére kattintva is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-163">The details and settings for each developer can be viewed by clicking on the developer's name.</span></span> <span data-ttu-id="6a8e1-164">Ebben a példában nincs fejlesztők előfizetett még a termék.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-164">In this example no developers have yet subscribed to the product.</span></span>

![Fejlesztők][api-management-developer-list]

## <span data-ttu-id="6a8e1-166"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a8e1-166"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="6a8e1-167">Miután a kívánt API-k hozzáadásával, és a termék közzétett, a fejlesztők előfizetni a termék és az API-k hívására megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-167">Once the desired APIs are added and the product published, developers can subscribe to the product and begin to call the APIs.</span></span> <span data-ttu-id="6a8e1-168">Lásd: mutatja be, ezeket a cikkeket, valamint a speciális termék konfigurációs [hogyan létrehozása, és speciális termékbeállítások konfigurálása az Azure API Management][How create and configure advanced product settings in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="6a8e1-168">For a tutorial that demonstrates these items as well as advanced product configuration see [How create and configure advanced product settings in Azure API Management][How create and configure advanced product settings in Azure API Management].</span></span>

<span data-ttu-id="6a8e1-169">Termékek kezelésével kapcsolatos további információkért tekintse meg a következő videó.</span><span class="sxs-lookup"><span data-stu-id="6a8e1-169">For more information about working with products, see the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[View subscribers to a product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How to create and use groups to manage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
