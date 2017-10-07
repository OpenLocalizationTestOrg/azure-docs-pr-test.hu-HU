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
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Az API-k védelme sebességkorlátokkal az Azure API Management használatával
Ez az útmutató bemutatja, milyen egyszerűen a háttér-API tooadd védelmét az Azure API Management arány korlátozás és a kvóta házirendek konfigurálásával.

Ebben az oktatóanyagban létrehoz egy "Ingyenes" API-terméket, amely lehetővé teszi a fejlesztők toomake too10 hívások száma percenként és mentése tooa legfeljebb 200 hívásszám hét tooyour API használatával hello [korlát hívás arány előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [ Set memóriahasználati kvóta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendek. Majd közzéteszi hello API és hello arány korlát házirend tesztelése.

További speciális forgatókönyveket hello használatával szabályozás [arány-korlát-által-kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a kulcs-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek, lásd: [speciális kérelmet az Azure API Management-szabályozás](api-management-sample-flexible-throttling.md).

## <a name="create-product"></a>toocreate termék
Ebben a lépésben létrehoz egy Ingyenes próbaverzió terméket, amely nem igényel jóváhagyott előfizetést.

> [!NOTE]
> Ha már konfigurált egy termék, és szeretné toouse az ehhez az oktatóanyaghoz, de is ugorhat azokat, amelyek túl[konfigurálása hívás arány korlátozás és a kvóta házirendek] [ Configure call rate limit and quota policies] és az oktatóanyag hello ott használhatná a terméket helyett hello ingyenes próbaverziója.
> 
> 

tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [kezelése az Azure API Management az első API] [ Manage your first API in Azure API Management] oktatóanyag.
> 
> 

Kattintson a **termékek** a hello **API Management** hello bal oldali toodisplay hello menüjének **termékek** lap.

![Termék hozzáadása][api-management-add-product]

Kattintson a **Hozzáadás termék** toodisplay hello **hozzáadása új termék** párbeszédpanel megnyitásához.

![Új termék hozzáadása][api-management-new-product-window]

A hello **cím** mezőbe írja be **ingyenes**.

A hello **leírás** mezőbe, a következő szöveg típusú hello: **előfizetők lesz képes toorun 10 hívások/perces tooa legfeljebb 200 hívások/hét után, amely a hozzáférés megtagadva.**

Az API Management termékei védettek vagy nyitottak lehetnek. Védett termékek kell előfizetett toobefore is használhatók. A nyitott termékeket előfizetés nélkül is lehet használni. Győződjön meg arról, hogy **előfizetés szükséges** kijelölt toocreate van egy védett terméket, amely a előfizetést igényel. Ez az hello alapértelmezett beállítása.

Ha szeretné, hogy egy rendszergazda tooreview, és fogadja el vagy utasítsa el az előfizetés toothis termék kísérletek, válassza ki a **előfizetés jóváhagyás szükséges**. Ha hello jelölőnégyzet nincs bejelölve, a előfizetés kísérletek lesz automatikusan jóváhagyta. Ebben a példában előfizetések automatikus jóváhagyása, így hello be nem jelöli be.

tooallow fejlesztői fiókok toosubscribe többször toohello új terméket, jelölje be hello **lehetővé teszi több egyidejű előfizetések** jelölőnégyzetet. Ez az oktatóanyag nem használ több egyidejű előfizetést, ezért ne jelölje ezt be.

Miután az összes érték lett megadva, kattintson **mentése** toocreate hello termék.

![Hozzáadott termék][api-management-product-added]

Alapértelmezés szerint új termékeket a hello látható toousers **rendszergazdák** csoport. Tooadd hello fogjuk **fejlesztők** csoport. Kattintson a **ingyenes**, és kattintson a hello **látható** fülre.

> Az API Management csoportok olyan termékek toodevelopers használt toomanage hello láthatóságát. Termékek adjon látható toogroups és fejlesztők tekintheti meg és előfizetés toohello terméket, amelyre látható toohello csoportok, amelyben tartoznak. További információkért lásd: [hogyan toocreate és -felhasználási csoportosítja az Azure API Management][How toocreate and use groups in Azure API Management].
> 
> 

![Fejlesztői csoport hozzáadása][api-management-add-developers-group]

Jelölje be hello **fejlesztők** jelölőnégyzetet, majd kattintson a **mentése**.

## <a name="add-api"></a>tooadd az API-k toohello termék
Ebben a lépésben hello oktatóanyag adunk hozzá hello Echo API toohello új ingyenes próbaverziója.

> Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak. További információkért lásd: [Az első API kezelése az Azure API Management szolgáltatásban][Manage your first API in Azure API Management].
> 
> 

Kattintson a **termékek** a hello **API Management** hello maradt, és válassza a menü **ingyenes** tooconfigure hello termék.

![Termék konfigurálása][api-management-configure-product]

Kattintson a **hozzáadása API tooproduct**.

![API tooproduct hozzáadása][api-management-add-api]

Válassza ki az **Echo API** elemet, majd kattintson a **Mentés** gombra.

![Echo API hozzáadása][api-management-add-echo-api]

## <a name="policies"></a>tooconfigure hívás arány korlátozás és a kvóta házirendek
Sebességhatárok és kvóták hello Helyicsoportházirend-szerkesztő vannak konfigurálva. Ebben az oktatóanyagban bővítjük hello két házirendek nincsenek hello [korlát hívás arány előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [Set memóriahasználati kvóta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendek. Ezek a házirendek hello termék hatókörben kell alkalmazni.

Kattintson a **házirendek** alatt hello **API Management** hello bal oldali menüben. A hello **termék** listában, kattintson **ingyenes**.

![Termékházirend][api-management-product-policy]

Kattintson a **házirend hozzáadása** tooimport Házirendsablon hello és hello arány korlátozás és a kvóta házirendek létrehozásának megkezdéséhez.

![Házirend hozzáadása][api-management-add-policy]

Sebesség korlátozás és a kvóta házirendek olyan bejövő házirendek, így pozíció hello kurzor hello bejövő elemben.

![Házirendszerkesztő][api-management-policy-editor-inbound]

Görgessen végig hello házirendek listájában, és keresse meg a hello **korlát hívás arány előfizetésenként** házirend bejegyzést.

![Házirend-utasítások][api-management-limit-policies]

Hello után létrejön a hello kurzor **bejövő** házirend elem hello melletti nyílra **korlát hívás arány előfizetésenként** tooinsert a sablont.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

Hello kódrészletben láthatja, hello házirend korlátozása hello termék API-k, illetve műveletek köszönhetően. Az oktatóanyag azt fogja nem használja ezt a funkciót, így törlése hello **api** és **művelet** hello elemek **sebességhatár** elemben, úgy, hogy csak a külső hello**sebességhatár** elem marad, ahogy az alábbi példa hello.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

A hello ingyenes termék hello maximális megengedhető hívás sebesség 10 hívások száma percenként, kell megadni, **10** hello hello értékként **hívások** attribútum, és **60** a hello **megújítási időszak** attribútum.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

tooconfigure hello **Set memóriahasználati kvóta előfizetésenként** házirend, pozíció hello alatt közvetlenül a kurzor újonnan hozzáadott **sebességhatár** hello elemet **bejövő** elemet, majd keresse meg és hello nyíl toohello bal oldalán kattintson **Set memóriahasználati kvóta előfizetésenként**.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

Hasonlóképpen toohello **Set memóriahasználati kvóta előfizetésenként** házirend, **Set memóriahasználati kvóta előfizetésenként** házirend lehetővé teszi, hogy a caps beállítás hello termék API-k és a műveletek. Az oktatóanyag azt fogja nem használja ezt a funkciót, így törlése hello **api** és **művelet** hello elemek **kvóta** elem, ahogy az alábbi példa hello.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Kvóták alapulhat hello eseményenkénti hívásszám időköz, sávszélesség vagy mindkettőt. Az oktatóanyag azt vannak nem alapján sávszélesség szabályozása, ezért törölje a hello **sávszélesség** attribútum.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

A hello ingyenes termék hello kvótát hetente 200 hívások. Adja meg **200** hello hello értékként **hívások** attribútumot, és adja meg **604800** hello hello értékként **megújítási időszak** az attribútum.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> A házirendidőközök másodpercekben vannak megadva. egy hét toocalculate hello intervallumát is szorzási hello hány napig (7) által hello száma óra szerint egy óra alatt (60) egy perc alatt (60) másodperc hello számú percek száma hello naponta (24): 7 * 24 * 60 * 60 = 604800.
> 
> 

Hello házirend konfigurálása után, akkor meg kell felelnie a következő példa hello.

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

Miután hello szükséges házirendek hogyan vannak konfigurálva, kattintson **mentése**.

![Házirend mentése][api-management-policy-save]

## <a name="publish-product"></a> toopublish hello termék
Most, hogy hello hello API-k hozzáadásával és hello házirendek hogyan vannak konfigurálva, hello termék közzé kell tenni, hogy a fejlesztők használhatják. Kattintson a **termékek** a hello **API Management** hello maradt, és válassza a menü **ingyenes** tooconfigure hello termék.

![Termék konfigurálása][api-management-configure-product]

Kattintson a **közzététel**, és kattintson a **Igen, azt közzé** tooconfirm.

![Termék közzététele][api-management-publish-product]

## <a name="subscribe-account"></a>toosubscribe toohello termék fejlesztői fiók
Most, hogy hello termék közzé van téve, a fejlesztők által használt előfizetett elérhető toobe tooand is.

> Az API Management-példány a rendszergazdák automatikusan előfizetett tooevery termék. Útmutató ezt a lépést azt fogja előfizetés hello fejlesztői nem rendszergazdai fiókok toohello ingyenes termék egyikét. Ha a fejlesztői fiókba hello Rendszergazdák szerepkör tagja, majd kövesse ezt a lépést, valamint annak ellenére, hogy már előfizetett.
> 
> 

Kattintson a **felhasználók** a hello **API Management** hello menüjének maradt, és kattintson a hello a developer-fiók nevét. A jelen példában használjuk hello **Clayton Gragg** fejlesztői fiókjába.

![Fejlesztő konfigurálása][api-management-configure-developer]

Kattintson az **Előfizetés hozzáadása** lehetőségre.

![Előfizetés hozzáadása][api-management-add-subscription-menu]

Válassza ki az **Ingyenes próbaverzió** elemet, majd kattintson az **Előfizetés** lehetőségre.

![Előfizetés hozzáadása][api-management-add-subscription]

> [!NOTE]
> Ebben az oktatóanyagban több egyidejű előfizetések nem engedélyezettek hello ingyenes próbaverziója. Ha azok, lehetővé válik felszólító tooname hello előfizetés, ahogy az alábbi példa hello.
> 
> 

![Előfizetés hozzáadása][api-management-add-subscription-multiple]

Miután rákattintott **előfizetés**, hello termék megjelenik a hello **előfizetés** hello felhasználó listáját.

![Előfizetés hozzáadva][api-management-subscription-added]

## <a name="test-rate-limit"></a>toocall egy művelet és a vizsgálati hello sávszélesség-korlátjának
Hello ingyenes próbaverziója van konfigurálva, és közzétett, azt hívja az egyes műveletek, és hello arány korlát tesztszabályzat.
Kapcsoló toohello fejlesztői portálján kattintva **fejlesztői portálján** hello jobb felső menüjében.

![Fejlesztői portál][api-management-developer-portal-menu]

Kattintson a **API-k** a felső menüben hello, és kattintson a **Echo API**.

![Fejlesztői portál][api-management-developer-portal-api-menu]

Kattintson a **GET Resource** elemre, majd kattintson a **Kipróbálom** gombra.

![Konzol megnyitása][api-management-open-console]

Tartsa hello alapértelmezett paraméterértékek, és válassza ki az Előfizetés kulcs hello ingyenes termék.

![Előfizetői azonosító][api-management-select-key]

> [!NOTE]
> Ha több előfizetéssel rendelkezik, lehet, hogy tooselect hello kulcsa **ingyenes**, vagy más hello házirendek hello előző lépésben beállított nem lesz érvényben.
> 
> 

Kattintson a **küldése**, majd tekintse meg hello válasz. Megjegyzés: hello **válaszállapot** a **200 OK**.

![A művelet eredményei][api-management-http-get-results]

Kattintson a **küldése** hello arány korlát házirend 10 percenként intézett hívások nagyobb sebességgel. Miután hello arány korlát házirend túllépi, állapotú **429-es jelű túl sok kérelem** adja vissza.

![A művelet eredményei][api-management-http-get-429]

Hello **válasz tartalom** jelzi hello fennmaradó időköz, mielőtt újrapróbálkozások sikeres lesz.

Ha hello sebessége korlát házirend 10 percenként intézett hívások érvényben van, ezt sikertelen lesz, a hello 60 másodperc elteltével első hello 10 sikeres hívás toohello termék előtt hello arány korlát túllépése. Ebben a példában a fennmaradó időköz hello: 54 másodperc.

## <a name="next-steps"></a>Következő lépések
* Tekintse meg a bemutatója a következő videó hello sebességhatárok és a kvóták beállítását.

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
