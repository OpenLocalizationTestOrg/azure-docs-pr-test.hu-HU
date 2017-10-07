---
title: "aaaHow toocreate és a termék közzététele az Azure API Management"
description: "Megtudhatja, hogyan toocreate és termékek közzététele az Azure API Management."
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
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a><span data-ttu-id="e2869-103">Hogyan toocreate és a termék közzététele az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="e2869-103">How toocreate and publish a product in Azure API Management</span></span>
<span data-ttu-id="e2869-104">Az Azure API Management a termék egy vagy több API-k, valamint a használati kvóta és hello használati feltételeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e2869-104">In Azure API Management, a product contains one or more APIs as well as a usage quota and hello terms of use.</span></span> <span data-ttu-id="e2869-105">Miután közzétette a termék, a fejlesztők toohello termék előfizetés és toouse hello termék API-k megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2869-105">Once a product is published, developers can subscribe toohello product and begin toouse hello product's APIs.</span></span> <span data-ttu-id="e2869-106">hello a témakör egy útmutató toocreating termék, az API-k hozzáadása, és a közzététel a fejlesztők számára.</span><span class="sxs-lookup"><span data-stu-id="e2869-106">hello topic provides a guide toocreating a product, adding an API, and publishing it for developers.</span></span>

## <span data-ttu-id="e2869-107"><a name="create-product"></a>Termék létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2869-107"><a name="create-product"> </a>Create a product</span></span>
<span data-ttu-id="e2869-108">Műveletek bekerülnek és tooan API a hello publisher portálon.</span><span class="sxs-lookup"><span data-stu-id="e2869-108">Operations are added and configured tooan API in hello publisher portal.</span></span> <span data-ttu-id="e2869-109">tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e2869-109">tooaccess hello publisher portal, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="e2869-111">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e2869-111">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="e2869-112">Kattintson a **termékek** hello menüben található hello bal oldali toodisplay hello **termékek** lapon, majd kattintson **termék hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e2869-112">Click on **Products** in hello menu on hello left toodisplay hello **Products** page, and click **Add Product**.</span></span>

![Termékek][api-management-products]

![Új termék][api-management-add-new-product]

<span data-ttu-id="e2869-115">Adjon meg egy leíró nevet a hello termék hello **neve** mező és hello hello termék leírása **leírás** mező.</span><span class="sxs-lookup"><span data-stu-id="e2869-115">Enter a descriptive name for hello product in hello **Name** field and a description of hello product in hello **Description** field.</span></span>

<span data-ttu-id="e2869-116">Az API Management termékek lehet **nyitott** vagy **védett**.</span><span class="sxs-lookup"><span data-stu-id="e2869-116">Products in API Management can be **Open** or **Protected**.</span></span> <span data-ttu-id="e2869-117">Védett termékek kell is használhatók, miközben a Megnyitás előfizetett toobefore termékek előfizetés nélkül is használható.</span><span class="sxs-lookup"><span data-stu-id="e2869-117">Protected products must be subscribed toobefore they can be used, while open products can be used without a subscription.</span></span> <span data-ttu-id="e2869-118">Ellenőrizze **előfizetés szükséges** toocreate egy védett terméket, amely a előfizetést igényel.</span><span class="sxs-lookup"><span data-stu-id="e2869-118">Check **Require subscription** toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="e2869-119">Ez az hello alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="e2869-119">This is hello default setting.</span></span>

<span data-ttu-id="e2869-120">Ellenőrizze **előfizetés jóváhagyás szükséges** Ha szeretné, hogy egy rendszergazda tooreview, és fogadja el vagy utasítsa el az előfizetés kísérletek toothis termék.</span><span class="sxs-lookup"><span data-stu-id="e2869-120">Check **Require subscription approval** if you want an administrator tooreview and accept or reject subscription attempts toothis product.</span></span> <span data-ttu-id="e2869-121">Ha hello jelölőnégyzet nincs bejelölve, a előfizetés kísérletek lesz automatikusan jóváhagyta.</span><span class="sxs-lookup"><span data-stu-id="e2869-121">If hello box is unchecked, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="e2869-122">Az előfizetések további információkért lásd: [nézet előfizetők tooa termék][View subscribers tooa product].</span><span class="sxs-lookup"><span data-stu-id="e2869-122">For more information on subscriptions, see [View subscribers tooa product][View subscribers tooa product].</span></span>

<span data-ttu-id="e2869-123">tooallow fejlesztői fiókok toosubscribe többször toohello terméket, ellenőrizze a hello **több előfizetés engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="e2869-123">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box.</span></span> <span data-ttu-id="e2869-124">Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal toohello termék kérhet le.</span><span class="sxs-lookup"><span data-stu-id="e2869-124">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

![Korlátlan több előfizetéssel][api-management-unlimited-multiple-subscriptions]

<span data-ttu-id="e2869-126">több egyidejű előfizetéssel toolimit hello száma ellenőrizze hello **egyidejű előfizetések számának korlátozása** jelölje be a jelölőnégyzetet, és írja be a hello előfizetési korlátját.</span><span class="sxs-lookup"><span data-stu-id="e2869-126">toolimit hello count of multiple simultaneous subscriptions, check hello **Limit number of simultaneous subscriptions to** check box and enter hello subscription limit.</span></span> <span data-ttu-id="e2869-127">A következő példa hello egyidejű előfizetések fejlesztői fiókonként korlátozott toofour.</span><span class="sxs-lookup"><span data-stu-id="e2869-127">In hello following example, simultaneous subscriptions are limited toofour per developer account.</span></span>

![Négy több előfizetéssel][api-management-four-multiple-subscriptions]

<span data-ttu-id="e2869-129">Ha minden új termék beállításainak konfigurálása után kattintson **mentése** toocreate hello új terméket.</span><span class="sxs-lookup"><span data-stu-id="e2869-129">Once all new product options are configured, click **Save** toocreate hello new product.</span></span>

![Termékek][api-management-products-page]

> <span data-ttu-id="e2869-131">Alapértelmezés szerint új termékek közzé nem tett, és látható csak toohello **rendszergazdák** csoport.</span><span class="sxs-lookup"><span data-stu-id="e2869-131">By default new products are unpublished, and are visible only toohello  **Administrators** group.</span></span>
> 
> 

<span data-ttu-id="e2869-132">tooconfigure termék, kattintson a hello termék nevére a hello **termékek** fülre.</span><span class="sxs-lookup"><span data-stu-id="e2869-132">tooconfigure a product, click on hello product name in hello **Products** tab.</span></span>

## <span data-ttu-id="e2869-133"><a name="add-apis"></a>API-k hozzáadása tooa termék</span><span class="sxs-lookup"><span data-stu-id="e2869-133"><a name="add-apis"> </a>Add APIs tooa product</span></span>
<span data-ttu-id="e2869-134">Hello **termékek** lapon négy hivatkozása a konfigurációhoz: **összegzés**, **beállítások**, **látható**, és  **Előfizetők**.</span><span class="sxs-lookup"><span data-stu-id="e2869-134">hello **Products** page contains four links for configuration: **Summary**, **Settings**, **Visibility**, and **Subscribers**.</span></span> <span data-ttu-id="e2869-135">Hello **összegzés** lapon látható, ahol akkor is vegye fel az API-k és közzétételét és a termék.</span><span class="sxs-lookup"><span data-stu-id="e2869-135">hello **Summary** tab is where you can add APIs and publish or unpublish a product.</span></span>

![Összefoglalás][api-management-new-product-summary]

<span data-ttu-id="e2869-137">A termék közzététele előtt kell tooadd egy vagy több API-k.</span><span class="sxs-lookup"><span data-stu-id="e2869-137">Before publishing your product you need tooadd one or more APIs.</span></span> <span data-ttu-id="e2869-138">toodo, kattintson **hozzáadása API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="e2869-138">toodo this, click **Add API tooproduct**.</span></span>

![API-k hozzáadása][api-management-add-apis-to-product]

<span data-ttu-id="e2869-140">Jelölje be hello szükséges API-t, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e2869-140">Select hello desired APIs and click **Save**.</span></span>

## <span data-ttu-id="e2869-141"><a name="add-description"></a>Hozzáadás leíró tooa termék</span><span class="sxs-lookup"><span data-stu-id="e2869-141"><a name="add-description"> </a>Add descriptive information tooa product</span></span>
<span data-ttu-id="e2869-142">Hello **beállítások** a lapon adhatók meg tooprovide részletes információkat hello termék például mindössze egyetlen célra szorítkoznak, hello hozzáférést biztosít a API-k és más hasznos információkat.</span><span class="sxs-lookup"><span data-stu-id="e2869-142">hello **Settings** tab allows you tooprovide detailed information about hello product such as its purpose, hello APIs it provides access to, and other useful information.</span></span> <span data-ttu-id="e2869-143">hello tartalom hello API hívása lesz, és egyszerű szöveg- vagy HTML-kódot is beírhatók hello a fejlesztők irányul.</span><span class="sxs-lookup"><span data-stu-id="e2869-143">hello content is targeted at hello developers that will be calling hello API and can be written in plain text or HTML markup.</span></span>

![A termék beállításai][api-management-product-settings]

<span data-ttu-id="e2869-145">Ellenőrizze **előfizetés szükséges** toocreate egy előfizetés toobe használt, vagy törölje a jelet igénylő védett termék hello jelölőnégyzet toocreate egy megnyitott terméket, amely a előfizetés nélkül meghívható.</span><span class="sxs-lookup"><span data-stu-id="e2869-145">Check **Require subscription** toocreate a protected product that requires a subscription toobe used, or clear hello checkbox toocreate an open product that can be called without a subscription.</span></span>

<span data-ttu-id="e2869-146">Válassza ki **előfizetés jóváhagyás szükséges** Ha azt szeretné, toomanually összes termék előfizetési kérelmek jóváhagyása.</span><span class="sxs-lookup"><span data-stu-id="e2869-146">Select **Require subscription approval** if you want toomanually approve all product subscription requests.</span></span> <span data-ttu-id="e2869-147">Alapértelmezés szerint az összes termék előfizetések automatikusan kapnak.</span><span class="sxs-lookup"><span data-stu-id="e2869-147">By default all product subscriptions are granted automatically.</span></span>

<span data-ttu-id="e2869-148">tooallow fejlesztői fiókok toosubscribe többször toohello terméket, ellenőrizze a hello **több előfizetés engedélyezése** jelölje be a jelölőnégyzetet, és opcionálisan adja meg a határértéket.</span><span class="sxs-lookup"><span data-stu-id="e2869-148">tooallow developer accounts toosubscribe multiple times toohello product, check hello **Allow multiple subscriptions** check box and optionally specify a limit.</span></span> <span data-ttu-id="e2869-149">Ha a négyzet nincs bejelölve, minden developer-fiók csak egy alkalommal toohello termék kérhet le.</span><span class="sxs-lookup"><span data-stu-id="e2869-149">If this box is not checked, each developer account can subscribe only a single time toohello product.</span></span>

<span data-ttu-id="e2869-150">Nem kötelező kitölteni hello **használati feltételek** hello termék, amely előfizetők el kell fogadnia ahhoz toouse hello termékben a hello használati feltételeit leíró mező.</span><span class="sxs-lookup"><span data-stu-id="e2869-150">Optionally fill in hello **Terms of use** field describing hello terms of use for hello product which subscribers must accept in order toouse hello product.</span></span>

## <span data-ttu-id="e2869-151"><a name="publish-product"></a>Termék közzététele</span><span class="sxs-lookup"><span data-stu-id="e2869-151"><a name="publish-product"> </a>Publish a product</span></span>
<span data-ttu-id="e2869-152">A termék API-k hello hívása előtt hello termék közzé kell tenni.</span><span class="sxs-lookup"><span data-stu-id="e2869-152">Before hello APIs in a product can be called, hello product must be published.</span></span> <span data-ttu-id="e2869-153">A hello **összegzés** hello termék lapra, majd **közzététel**, és kattintson a **Igen, azt közzé** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="e2869-153">On hello **Summary** tab for hello product, click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span> <span data-ttu-id="e2869-154">Kattintson egy olyan, korábban kiadott termék magánhálózat toomake **Közzététel megszüntetése**.</span><span class="sxs-lookup"><span data-stu-id="e2869-154">toomake a previously published product private, click **Unpublish**.</span></span>

![Termék közzététele][api-management-publish-product]

## <span data-ttu-id="e2869-156"><a name="make-visible"></a>Ellenőrizze a termék látható toodevelopers</span><span class="sxs-lookup"><span data-stu-id="e2869-156"><a name="make-visible"> </a>Make a product visible toodevelopers</span></span>
<span data-ttu-id="e2869-157">Hello **látható** lapon meghatározhatja, melyik szerepkörök képes toosee hello termék hello developer portálon, és toohello termék előfizetés toochoose.</span><span class="sxs-lookup"><span data-stu-id="e2869-157">hello **Visibility** tab allows you toochoose which roles are able toosee hello product on hello developer portal and subscribe toohello product.</span></span>

![A termék látható][api-management-product-visiblity]

<span data-ttu-id="e2869-159">tooenable vagy tiltsa le látható-e a termék hello fejlesztők számára az egy csoportba, ellenőrizze, vagy hello csoport hello jelölőnégyzeteket, törölje a jelet, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e2869-159">tooenable or disable visibility of a product for hello developers in a group, check or uncheck hello check box beside hello group and then click **Save**.</span></span>

> <span data-ttu-id="e2869-160">További információkért lásd: [hogyan toocreate és -felhasználási csoportok toomanage fejlesztői fiókok az Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e2869-160">For more information, see [How toocreate and use groups toomanage developer accounts in Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].</span></span>
> 
> 

## <span data-ttu-id="e2869-161"><a name="view-subscribers"></a>Előfizetők tooa termék megtekintése</span><span class="sxs-lookup"><span data-stu-id="e2869-161"><a name="view-subscribers"> </a>View subscribers tooa product</span></span>
<span data-ttu-id="e2869-162">Hello **előfizetők** lap felsorolja az előfizetett toohello termék hello fejlesztőknek.</span><span class="sxs-lookup"><span data-stu-id="e2869-162">hello **Subscribers** tab lists hello developers who have subscribed toohello product.</span></span> <span data-ttu-id="e2869-163">hello részleteit és beállításait minden fejlesztői tekintheti meg a fejlesztői hello nevére kattintva.</span><span class="sxs-lookup"><span data-stu-id="e2869-163">hello details and settings for each developer can be viewed by clicking on hello developer's name.</span></span> <span data-ttu-id="e2869-164">Ebben a példában nincs fejlesztők még előfizetett toohello termék.</span><span class="sxs-lookup"><span data-stu-id="e2869-164">In this example no developers have yet subscribed toohello product.</span></span>

![Fejlesztők][api-management-developer-list]

## <span data-ttu-id="e2869-166"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2869-166"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="e2869-167">Egyszer hello szükséges API-k és hello termék közzétett, a fejlesztők is toohello termék előfizetés és toocall hello API-k megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="e2869-167">Once hello desired APIs are added and hello product published, developers can subscribe toohello product and begin toocall hello APIs.</span></span> <span data-ttu-id="e2869-168">Lásd: mutatja be, ezeket a cikkeket, valamint a speciális termék konfigurációs [hogyan létrehozása, és speciális termékbeállítások konfigurálása az Azure API Management][How create and configure advanced product settings in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="e2869-168">For a tutorial that demonstrates these items as well as advanced product configuration see [How create and configure advanced product settings in Azure API Management][How create and configure advanced product settings in Azure API Management].</span></span>

<span data-ttu-id="e2869-169">Működő termékekkel kapcsolatos további információkért tekintse meg a következő videó hello.</span><span class="sxs-lookup"><span data-stu-id="e2869-169">For more information about working with products, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
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


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
