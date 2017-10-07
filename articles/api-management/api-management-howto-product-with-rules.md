---
title: "aaaProtect az API-t az Azure API Management szolgáltatással |} Microsoft Docs"
description: "Megtudhatja, hogyan tooprotect kvóták és sávszélesség-szabályozás (sebességhatárolt) házirendek az API-t."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3113fd277d434da0c051b8b90fd629a102bf4867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a><span data-ttu-id="012db-103">Az API-k védelme sebességkorlátokkal az Azure API Management használatával</span><span class="sxs-lookup"><span data-stu-id="012db-103">Protect your API with rate limits using Azure API Management</span></span>
<span data-ttu-id="012db-104">Ez az útmutató bemutatja, milyen egyszerűen a háttér-API tooadd védelmét az Azure API Management arány korlátozás és a kvóta házirendek konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="012db-104">This guide shows you how easy it is tooadd protection for your backend API by configuring rate limit and quota policies with Azure API Management.</span></span>

<span data-ttu-id="012db-105">Ebben az oktatóanyagban létrehoz egy "Ingyenes" API-terméket, amely lehetővé teszi a fejlesztők toomake too10 hívások száma percenként és mentése tooa legfeljebb 200 hívásszám hét tooyour API használatával hello [korlát hívás arány előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [ Set memóriahasználati kvóta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendek.</span><span class="sxs-lookup"><span data-stu-id="012db-105">In this tutorial, you will create a "Free Trial" API product that allows developers toomake up too10 calls per minute and up tooa maximum of 200 calls per week tooyour API using hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="012db-106">Majd közzéteszi hello API és hello arány korlát házirend tesztelése.</span><span class="sxs-lookup"><span data-stu-id="012db-106">You will then publish hello API and test hello rate limit policy.</span></span>

<span data-ttu-id="012db-107">További speciális forgatókönyveket hello használatával szabályozás [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek, lásd: [speciális kérelmet az Azure API Management-szabályozás](api-management-sample-flexible-throttling.md).</span><span class="sxs-lookup"><span data-stu-id="012db-107">For more advanced throttling scenarios using hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies, see [Advanced request throttling with Azure API Management](api-management-sample-flexible-throttling.md).</span></span>

## <span data-ttu-id="012db-108"><a name="create-product"></a>toocreate termék</span><span class="sxs-lookup"><span data-stu-id="012db-108"><a name="create-product"> </a>toocreate a product</span></span>
<span data-ttu-id="012db-109">Ebben a lépésben létrehoz egy Ingyenes próbaverzió terméket, amely nem igényel jóváhagyott előfizetést.</span><span class="sxs-lookup"><span data-stu-id="012db-109">In this step, you will create a Free Trial product that does not require subscription approval.</span></span>

> [!NOTE]
> <span data-ttu-id="012db-110">Ha már konfigurált egy termék, és szeretné toouse az ehhez az oktatóanyaghoz, de is ugorhat azokat, amelyek túl[konfigurálása hívás arány korlátozás és a kvóta házirendek] [ Configure call rate limit and quota policies] és az oktatóanyag hello ott használhatná a terméket helyett hello ingyenes próbaverziója.</span><span class="sxs-lookup"><span data-stu-id="012db-110">If you already have a product configured and want toouse it for this tutorial, you can jump ahead too[Configure call rate limit and quota policies][Configure call rate limit and quota policies] and follow hello tutorial from there using your product in place of hello Free Trial product.</span></span>
> 
> 

<span data-ttu-id="012db-111">tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="012db-111">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Közzétevő portál][api-management-management-console]

> <span data-ttu-id="012db-113">Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [kezelése az Azure API Management az első API] [ Manage your first API in Azure API Management] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="012db-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API in Azure API Management][Manage your first API in Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="012db-114">Kattintson a **termékek** a hello **API Management** hello bal oldali toodisplay hello menüjének **termékek** lap.</span><span class="sxs-lookup"><span data-stu-id="012db-114">Click **Products** in hello **API Management** menu on hello left toodisplay hello **Products** page.</span></span>

![Termék hozzáadása][api-management-add-product]

<span data-ttu-id="012db-116">Kattintson a **Hozzáadás termék** toodisplay hello **hozzáadása új termék** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="012db-116">Click **Add product** toodisplay hello **Add new product** dialog box.</span></span>

![Új termék hozzáadása][api-management-new-product-window]

<span data-ttu-id="012db-118">A hello **cím** mezőbe írja be **ingyenes**.</span><span class="sxs-lookup"><span data-stu-id="012db-118">In hello **Title** box, type **Free Trial**.</span></span>

<span data-ttu-id="012db-119">A hello **leírás** mezőbe, a következő szöveg típusú hello: **előfizetők lesz képes toorun 10 hívások/perces tooa legfeljebb 200 hívások/hét után, amely a hozzáférés megtagadva.**</span><span class="sxs-lookup"><span data-stu-id="012db-119">In hello **Description** box, type hello following text: **Subscribers will be able toorun 10 calls/minute up tooa maximum of 200 calls/week after which access is denied.**</span></span>

<span data-ttu-id="012db-120">Az API Management termékei védettek vagy nyitottak lehetnek.</span><span class="sxs-lookup"><span data-stu-id="012db-120">Products in API Management can be protected or open.</span></span> <span data-ttu-id="012db-121">Védett termékek kell előfizetett toobefore is használhatók.</span><span class="sxs-lookup"><span data-stu-id="012db-121">Protected products must be subscribed toobefore they can be used.</span></span> <span data-ttu-id="012db-122">A nyitott termékeket előfizetés nélkül is lehet használni.</span><span class="sxs-lookup"><span data-stu-id="012db-122">Open products can be used without a subscription.</span></span> <span data-ttu-id="012db-123">Győződjön meg arról, hogy **előfizetés szükséges** kijelölt toocreate van egy védett terméket, amely a előfizetést igényel.</span><span class="sxs-lookup"><span data-stu-id="012db-123">Ensure that **Require subscription** is selected toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="012db-124">Ez az hello alapértelmezett beállítása.</span><span class="sxs-lookup"><span data-stu-id="012db-124">This is hello default setting.</span></span>

<span data-ttu-id="012db-125">Ha szeretné, hogy egy rendszergazda tooreview, és fogadja el vagy utasítsa el az előfizetés toothis termék kísérletek, válassza ki a **előfizetés jóváhagyás szükséges**.</span><span class="sxs-lookup"><span data-stu-id="012db-125">If you want an administrator tooreview and accept or reject subscription attempts toothis product, select **Require subscription approval**.</span></span> <span data-ttu-id="012db-126">Ha hello jelölőnégyzet nincs bejelölve, a előfizetés kísérletek lesz automatikusan jóváhagyta.</span><span class="sxs-lookup"><span data-stu-id="012db-126">If hello check box is not selected, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="012db-127">Ebben a példában előfizetések automatikus jóváhagyása, így hello be nem jelöli be.</span><span class="sxs-lookup"><span data-stu-id="012db-127">In this example, subscriptions are automatically approved, so do not select hello box.</span></span>

<span data-ttu-id="012db-128">tooallow fejlesztői fiókok toosubscribe többször toohello új terméket, jelölje be hello **lehetővé teszi több egyidejű előfizetések** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="012db-128">tooallow developer accounts toosubscribe multiple times toohello new product, select hello **Allow multiple simultaneous subscriptions** check box.</span></span> <span data-ttu-id="012db-129">Ez az oktatóanyag nem használ több egyidejű előfizetést, ezért ne jelölje ezt be.</span><span class="sxs-lookup"><span data-stu-id="012db-129">This tutorial does not utilize multiple simultaneous subscriptions, so leave it unchecked.</span></span>

<span data-ttu-id="012db-130">Miután az összes érték lett megadva, kattintson **mentése** toocreate hello termék.</span><span class="sxs-lookup"><span data-stu-id="012db-130">After all values are entered, click **Save** toocreate hello product.</span></span>

![Hozzáadott termék][api-management-product-added]

<span data-ttu-id="012db-132">Alapértelmezés szerint új termékeket a hello látható toousers **rendszergazdák** csoport.</span><span class="sxs-lookup"><span data-stu-id="012db-132">By default, new products are visible toousers in hello **Administrators** group.</span></span> <span data-ttu-id="012db-133">Tooadd hello fogjuk **fejlesztők** csoport.</span><span class="sxs-lookup"><span data-stu-id="012db-133">We are going tooadd hello **Developers** group.</span></span> <span data-ttu-id="012db-134">Kattintson a **ingyenes**, és kattintson a hello **látható** fülre.</span><span class="sxs-lookup"><span data-stu-id="012db-134">Click **Free Trial**, and then click hello **Visibility** tab.</span></span>

> <span data-ttu-id="012db-135">Az API Management csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát.</span><span class="sxs-lookup"><span data-stu-id="012db-135">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="012db-136">Termékek adjon látható toogroups és fejlesztők tekintheti meg és előfizetés toohello terméket, amelyre látható toohello csoportok, amelyben tartoznak.</span><span class="sxs-lookup"><span data-stu-id="012db-136">Products grant visibility toogroups, and developers can view and subscribe toohello products that are visible toohello groups in which they belong.</span></span> <span data-ttu-id="012db-137">További információkért lásd: [hogyan toocreate és -felhasználási csoportosítja az Azure API Management][How toocreate and use groups in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="012db-137">For more information, see [How toocreate and use groups in Azure API Management][How toocreate and use groups in Azure API Management].</span></span>
> 
> 

![Fejlesztői csoport hozzáadása][api-management-add-developers-group]

<span data-ttu-id="012db-139">Jelölje be hello **fejlesztők** jelölőnégyzetet, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="012db-139">Select hello **Developers** check box, and then click **Save**.</span></span>

## <span data-ttu-id="012db-140"><a name="add-api"></a>tooadd az API-k toohello termék</span><span class="sxs-lookup"><span data-stu-id="012db-140"><a name="add-api"> </a>tooadd an API toohello product</span></span>
<span data-ttu-id="012db-141">Ebben a lépésben hello oktatóanyag adunk hozzá hello Echo API toohello új ingyenes próbaverziója.</span><span class="sxs-lookup"><span data-stu-id="012db-141">In this step of hello tutorial, we will add hello Echo API toohello new Free Trial product.</span></span>

> <span data-ttu-id="012db-142">Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak.</span><span class="sxs-lookup"><span data-stu-id="012db-142">Each API Management service instance comes pre-configured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="012db-143">További információkért lásd: [Az első API kezelése az Azure API Management szolgáltatásban][Manage your first API in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="012db-143">For more information, see [Manage your first API in Azure API Management][Manage your first API in Azure API Management].</span></span>
> 
> 

<span data-ttu-id="012db-144">Kattintson a **termékek** a hello **API Management** hello maradt, és válassza a menü **ingyenes** tooconfigure hello termék.</span><span class="sxs-lookup"><span data-stu-id="012db-144">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Termék konfigurálása][api-management-configure-product]

<span data-ttu-id="012db-146">Kattintson a **hozzáadása API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="012db-146">Click **Add API tooproduct**.</span></span>

![API tooproduct hozzáadása][api-management-add-api]

<span data-ttu-id="012db-148">Válassza ki az **Echo API** elemet, majd kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="012db-148">Select **Echo API**, and then click **Save**.</span></span>

![Echo API hozzáadása][api-management-add-echo-api]

## <span data-ttu-id="012db-150"><a name="policies"></a>tooconfigure hívás arány korlátozás és a kvóta házirendek</span><span class="sxs-lookup"><span data-stu-id="012db-150"><a name="policies"> </a>tooconfigure call rate limit and quota policies</span></span>
<span data-ttu-id="012db-151">Sebességhatárok és kvóták hello Helyicsoportházirend-szerkesztő vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="012db-151">Rate limits and quotas are configured in hello policy editor.</span></span> <span data-ttu-id="012db-152">Ebben az oktatóanyagban bővítjük hello két házirendek nincsenek hello [korlát hívás arány előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [Set memóriahasználati kvóta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendek.</span><span class="sxs-lookup"><span data-stu-id="012db-152">hello two policies we will be adding in this tutorial are hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="012db-153">Ezek a házirendek hello termék hatókörben kell alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="012db-153">These policies must be applied at hello product scope.</span></span>

<span data-ttu-id="012db-154">Kattintson a **házirendek** alatt hello **API Management** hello bal oldali menüben.</span><span class="sxs-lookup"><span data-stu-id="012db-154">Click **Policies** under hello **API Management** menu on hello left.</span></span> <span data-ttu-id="012db-155">A hello **termék** listában, kattintson **ingyenes**.</span><span class="sxs-lookup"><span data-stu-id="012db-155">In hello **Product** list, click **Free Trial**.</span></span>

![Termékházirend][api-management-product-policy]

<span data-ttu-id="012db-157">Kattintson a **házirend hozzáadása** tooimport Házirendsablon hello és hello arány korlátozás és a kvóta házirendek létrehozásának megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="012db-157">Click **Add Policy** tooimport hello policy template and begin creating hello rate limit and quota policies.</span></span>

![Házirend hozzáadása][api-management-add-policy]

<span data-ttu-id="012db-159">Sebesség korlátozás és a kvóta házirendek olyan bejövő házirendek, így pozíció hello kurzor hello bejövő elemben.</span><span class="sxs-lookup"><span data-stu-id="012db-159">Rate limit and quota policies are inbound policies, so position hello cursor in hello inbound element.</span></span>

![Házirendszerkesztő][api-management-policy-editor-inbound]

<span data-ttu-id="012db-161">Görgessen végig hello házirendek listájában, és keresse meg a hello **korlát hívás arány előfizetésenként** házirend bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="012db-161">Scroll through hello list of policies and locate hello **Limit call rate per subscription** policy entry.</span></span>

![Házirend-utasítások][api-management-limit-policies]

<span data-ttu-id="012db-163">Hello után létrejön a hello kurzor **bejövő** házirend elem hello melletti nyílra **korlát hívás arány előfizetésenként** tooinsert a sablont.</span><span class="sxs-lookup"><span data-stu-id="012db-163">After hello cursor is positioned in hello **inbound** policy element, click hello arrow beside **Limit call rate per subscription** tooinsert its policy template.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

<span data-ttu-id="012db-164">Hello kódrészletben láthatja, hello házirend korlátozása hello termék API-k, illetve műveletek köszönhetően.</span><span class="sxs-lookup"><span data-stu-id="012db-164">As you can see from hello snippet, hello policy allows setting limits for hello product's APIs and operations.</span></span> <span data-ttu-id="012db-165">Az oktatóanyag azt fogja nem használja ezt a funkciót, így törlése hello **api** és **művelet** hello elemek **sebességhatár** elemben, úgy, hogy csak a külső hello**sebességhatár** elem marad, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="012db-165">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **rate-limit** element, such that only hello outer **rate-limit** element remains, as shown in hello following example.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

<span data-ttu-id="012db-166">A hello ingyenes termék hello maximális megengedhető hívás sebesség 10 hívások száma percenként, kell megadni, **10** hello hello értékként **hívások** attribútum, és **60** a hello **megújítási időszak** attribútum.</span><span class="sxs-lookup"><span data-stu-id="012db-166">In hello Free Trial product, hello maximum allowable call rate is 10 calls per minute, so type **10** as hello value for hello **calls** attribute, and **60** for hello **renewal-period** attribute.</span></span>

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

<span data-ttu-id="012db-167">tooconfigure hello **Set memóriahasználati kvóta előfizetésenként** házirend, pozíció hello alatt közvetlenül a kurzor újonnan hozzáadott **sebességhatár** hello elemet **bejövő** elemet, majd keresse meg és hello nyíl toohello bal oldalán kattintson **Set memóriahasználati kvóta előfizetésenként**.</span><span class="sxs-lookup"><span data-stu-id="012db-167">tooconfigure hello **Set usage quota per subscription** policy, position your cursor immediately below hello newly added **rate-limit** element within hello **inbound** element, and then locate and click hello arrow toohello left of **Set usage quota per subscription**.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

<span data-ttu-id="012db-168">Hasonlóképpen toohello **Set memóriahasználati kvóta előfizetésenként** házirend, **Set memóriahasználati kvóta előfizetésenként** házirend lehetővé teszi, hogy a caps beállítás hello termék API-k és a műveletek.</span><span class="sxs-lookup"><span data-stu-id="012db-168">Similarly toohello **Set usage quota per subscription** policy, **Set usage quota per subscription** policy allows setting caps for on hello product's APIs and operations.</span></span> <span data-ttu-id="012db-169">Az oktatóanyag azt fogja nem használja ezt a funkciót, így törlése hello **api** és **művelet** hello elemek **kvóta** elem, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="012db-169">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **quota** element, as shown in hello following example.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

<span data-ttu-id="012db-170">Kvóták alapulhat hello eseményenkénti hívásszám időköz, sávszélesség vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="012db-170">Quotas can be based on hello number of calls per interval, bandwidth, or both.</span></span> <span data-ttu-id="012db-171">Az oktatóanyag azt vannak nem alapján sávszélesség szabályozása, ezért törölje a hello **sávszélesség** attribútum.</span><span class="sxs-lookup"><span data-stu-id="012db-171">In this tutorial, we are not throttling based on bandwidth, so delete hello **bandwidth** attribute.</span></span>

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

<span data-ttu-id="012db-172">A hello ingyenes termék hello kvótát hetente 200 hívások.</span><span class="sxs-lookup"><span data-stu-id="012db-172">In hello Free Trial product, hello quota is 200 calls per week.</span></span> <span data-ttu-id="012db-173">Adja meg **200** hello hello értékként **hívások** attribútumot, és adja meg **604800** hello hello értékként **megújítási időszak** az attribútum.</span><span class="sxs-lookup"><span data-stu-id="012db-173">Specify **200** as hello value for hello **calls** attribute, and then specify **604800** as hello value for hello **renewal-period** attribute.</span></span>

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> <span data-ttu-id="012db-174">A házirendidőközök másodpercekben vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="012db-174">Policy intervals are specified in seconds.</span></span> <span data-ttu-id="012db-175">egy hét toocalculate hello intervallumát is szorzási hello hány napig (7) által hello száma óra szerint egy óra alatt (60) egy perc alatt (60) másodperc hello számú percek száma hello naponta (24): 7 * 24 * 60 * 60 = 604800.</span><span class="sxs-lookup"><span data-stu-id="012db-175">toocalculate hello interval for a week, you can multiply hello number of days (7) by hello number of hours in a day (24) by hello number of minutes in an hour (60) by hello number of seconds in a minute (60): 7 * 24 * 60 * 60 = 604800.</span></span>
> 
> 

<span data-ttu-id="012db-176">Hello házirend konfigurálása után, akkor meg kell felelnie a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="012db-176">When you have finished configuring hello policy, it should match hello following example.</span></span>

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

<span data-ttu-id="012db-177">Miután hello szükséges házirendek hogyan vannak konfigurálva, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="012db-177">After hello desired policies are configured, click **Save**.</span></span>

![Házirend mentése][api-management-policy-save]

## <span data-ttu-id="012db-179"><a name="publish-product"></a> toopublish hello termék</span><span class="sxs-lookup"><span data-stu-id="012db-179"><a name="publish-product"> </a> toopublish hello product</span></span>
<span data-ttu-id="012db-180">Most, hogy hello hello API-k hozzáadásával és hello házirendek hogyan vannak konfigurálva, hello termék közzé kell tenni, hogy a fejlesztők használhatják.</span><span class="sxs-lookup"><span data-stu-id="012db-180">Now that hello hello APIs are added and hello policies are configured, hello product must be published so that it can be used by developers.</span></span> <span data-ttu-id="012db-181">Kattintson a **termékek** a hello **API Management** hello maradt, és válassza a menü **ingyenes** tooconfigure hello termék.</span><span class="sxs-lookup"><span data-stu-id="012db-181">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Termék konfigurálása][api-management-configure-product]

<span data-ttu-id="012db-183">Kattintson a **közzététel**, és kattintson a **Igen, azt közzé** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="012db-183">Click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span>

![Termék közzététele][api-management-publish-product]

## <span data-ttu-id="012db-185"><a name="subscribe-account"></a>toosubscribe toohello termék fejlesztői fiók</span><span class="sxs-lookup"><span data-stu-id="012db-185"><a name="subscribe-account"> </a>toosubscribe a developer account toohello product</span></span>
<span data-ttu-id="012db-186">Most, hogy hello termék közzé van téve, a fejlesztők által használt előfizetett elérhető toobe tooand is.</span><span class="sxs-lookup"><span data-stu-id="012db-186">Now that hello product is published, it is available toobe subscribed tooand used by developers.</span></span>

> <span data-ttu-id="012db-187">Az API Management-példány a rendszergazdák automatikusan előfizetett tooevery termék.</span><span class="sxs-lookup"><span data-stu-id="012db-187">Administrators of an API Management instance are automatically subscribed tooevery product.</span></span> <span data-ttu-id="012db-188">Útmutató ezt a lépést azt fogja előfizetés hello fejlesztői nem rendszergazdai fiókok toohello ingyenes termék egyikét.</span><span class="sxs-lookup"><span data-stu-id="012db-188">In this tutorial step, we will subscribe one of hello non-administrator developer accounts toohello Free Trial product.</span></span> <span data-ttu-id="012db-189">Ha a fejlesztői fiókba hello Rendszergazdák szerepkör tagja, majd kövesse ezt a lépést, valamint annak ellenére, hogy már előfizetett.</span><span class="sxs-lookup"><span data-stu-id="012db-189">If your developer account is part of hello Administrators role, then you can follow along with this step, even though you are already subscribed.</span></span>
> 
> 

<span data-ttu-id="012db-190">Kattintson a **felhasználók** a hello **API Management** hello menüjének maradt, és kattintson a hello a developer-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="012db-190">Click **Users** on hello **API Management** menu on hello left, and then click hello name of your developer account.</span></span> <span data-ttu-id="012db-191">A jelen példában használjuk hello **Clayton Gragg** fejlesztői fiókjába.</span><span class="sxs-lookup"><span data-stu-id="012db-191">In this example, we are using hello **Clayton Gragg** developer account.</span></span>

![Fejlesztő konfigurálása][api-management-configure-developer]

<span data-ttu-id="012db-193">Kattintson az **Előfizetés hozzáadása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="012db-193">Click **Add Subscription**.</span></span>

![Előfizetés hozzáadása][api-management-add-subscription-menu]

<span data-ttu-id="012db-195">Válassza ki az **Ingyenes próbaverzió** elemet, majd kattintson az **Előfizetés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="012db-195">Select **Free Trial**, and then click **Subscribe**.</span></span>

![Előfizetés hozzáadása][api-management-add-subscription]

> [!NOTE]
> <span data-ttu-id="012db-197">Ebben az oktatóanyagban több egyidejű előfizetések nem engedélyezettek hello ingyenes próbaverziója.</span><span class="sxs-lookup"><span data-stu-id="012db-197">In this tutorial, multiple simultaneous subscriptions are not enabled for hello Free Trial product.</span></span> <span data-ttu-id="012db-198">Ha azok, lehetővé válik felszólító tooname hello előfizetés, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="012db-198">If they were, you would be prompted tooname hello subscription, as shown in hello following example.</span></span>
> 
> 

![Előfizetés hozzáadása][api-management-add-subscription-multiple]

<span data-ttu-id="012db-200">Miután rákattintott **előfizetés**, hello termék megjelenik a hello **előfizetés** hello felhasználó listáját.</span><span class="sxs-lookup"><span data-stu-id="012db-200">After clicking **Subscribe**, hello product appears in hello **Subscription** list for hello user.</span></span>

![Előfizetés hozzáadva][api-management-subscription-added]

## <span data-ttu-id="012db-202"><a name="test-rate-limit"></a>toocall egy művelet és a vizsgálati hello sávszélesség-korlátjának</span><span class="sxs-lookup"><span data-stu-id="012db-202"><a name="test-rate-limit"> </a>toocall an operation and test hello rate limit</span></span>
<span data-ttu-id="012db-203">Hello ingyenes próbaverziója van konfigurálva, és közzétett, azt hívja az egyes műveletek, és hello arány korlát tesztszabályzat.</span><span class="sxs-lookup"><span data-stu-id="012db-203">Now that hello Free Trial product is configured and published, we can call some operations and test hello rate limit policy.</span></span>
<span data-ttu-id="012db-204">Kapcsoló toohello fejlesztői portálján kattintva **fejlesztői portálján** hello jobb felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="012db-204">Switch toohello developer portal by clicking **Developer portal** in hello upper-right menu.</span></span>

![Fejlesztői portál][api-management-developer-portal-menu]

<span data-ttu-id="012db-206">Kattintson a **API-k** a felső menüben hello, és kattintson a **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="012db-206">Click **APIs** in hello top menu, and then click **Echo API**.</span></span>

![Fejlesztői portál][api-management-developer-portal-api-menu]

<span data-ttu-id="012db-208">Kattintson a **GET Resource** elemre, majd kattintson a **Kipróbálom** gombra.</span><span class="sxs-lookup"><span data-stu-id="012db-208">Click **GET Resource**, and then click **Try it**.</span></span>

![Konzol megnyitása][api-management-open-console]

<span data-ttu-id="012db-210">Tartsa hello alapértelmezett paraméterértékek, és válassza ki az Előfizetés kulcs hello ingyenes termék.</span><span class="sxs-lookup"><span data-stu-id="012db-210">Keep hello default parameter values, and then select your subscription key for hello Free Trial product.</span></span>

![Előfizetői azonosító][api-management-select-key]

> [!NOTE]
> <span data-ttu-id="012db-212">Ha több előfizetéssel rendelkezik, lehet, hogy tooselect hello kulcsa **ingyenes**, vagy más hello házirendek hello előző lépésben beállított nem lesz érvényben.</span><span class="sxs-lookup"><span data-stu-id="012db-212">If you have multiple subscriptions, be sure tooselect hello key for **Free Trial**, or else hello policies that were configured in hello previous steps won't be in effect.</span></span>
> 
> 

<span data-ttu-id="012db-213">Kattintson a **küldése**, majd tekintse meg hello válasz.</span><span class="sxs-lookup"><span data-stu-id="012db-213">Click **Send**, and then view hello response.</span></span> <span data-ttu-id="012db-214">Megjegyzés: hello **válaszállapot** a **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="012db-214">Note hello **Response status** of **200 OK**.</span></span>

![A művelet eredményei][api-management-http-get-results]

<span data-ttu-id="012db-216">Kattintson a **küldése** hello arány korlát házirend 10 percenként intézett hívások nagyobb sebességgel.</span><span class="sxs-lookup"><span data-stu-id="012db-216">Click **Send** at a rate greater than hello rate limit policy of 10 calls per minute.</span></span> <span data-ttu-id="012db-217">Miután hello arány korlát házirend túllépi, állapotú **429-es jelű túl sok kérelem** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="012db-217">After hello rate limit policy is exceeded, a response status of **429 Too Many Requests** is returned.</span></span>

![A művelet eredményei][api-management-http-get-429]

<span data-ttu-id="012db-219">Hello **válasz tartalom** jelzi hello fennmaradó időköz, mielőtt újrapróbálkozások sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="012db-219">hello **Response content** indicates hello remaining interval before retries will be successful.</span></span>

<span data-ttu-id="012db-220">Ha hello sebessége korlát házirend 10 percenként intézett hívások érvényben van, ezt sikertelen lesz, a hello 60 másodperc elteltével első hello 10 sikeres hívás toohello termék előtt hello arány korlát túllépése.</span><span class="sxs-lookup"><span data-stu-id="012db-220">When hello rate limit policy of 10 calls per minute is in effect, subsequent calls will fail until 60 seconds have elapsed from hello first of hello 10 successful calls toohello product before hello rate limit was exceeded.</span></span> <span data-ttu-id="012db-221">Ebben a példában a fennmaradó időköz hello: 54 másodperc.</span><span class="sxs-lookup"><span data-stu-id="012db-221">In this example, hello remaining interval is 54 seconds.</span></span>

## <span data-ttu-id="012db-222"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="012db-222"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="012db-223">Tekintse meg a bemutatója a következő videó hello sebességhatárok és a kvóták beállítását.</span><span class="sxs-lookup"><span data-stu-id="012db-223">Watch a demo of setting rate limits and quotas in hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How toocreate and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers tooa product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API toohello product]: #add-api
[Publish hello product]: #publish-product
[Subscribe a developer account toohello product]: #subscribe-account
[Call an operation and test hello rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
