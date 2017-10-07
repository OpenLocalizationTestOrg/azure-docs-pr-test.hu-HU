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
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a>Adja hozzá a gyorsítótárazási tooimprove teljesítmény az Azure API Management
Az API Management műveleteit konfigurálni lehet a válaszok gyorsítótárazásához. A válaszok gyorsítótárazása jelentősen csökkentheti az API-k késleltetését, sávszélesség-használatát és a webszolgáltatások terhelését olyan adatok esetén, amelyek nem változnak gyakran.

Ez az útmutató bemutatja, hogyan tooadd válasz az API-gyorsítótárazás és a hello minta Echo API műveletek házirendek konfigurálása érdekében. Majd hívása hello művelet hello developer portálon tooverify gyorsítótárazásának működés közben.

> [!NOTE]
> További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Mielőtt következő hello a jelen útmutató lépéseit, rendelkeznie kell egy API-t és a konfigurált termék API-kezelés szolgáltatás példány. Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.

## <a name="configure-caching"></a>Művelet konfigurálása gyorsítótárazáshoz
Ebben a lépésben gyorsítótárazási beállítások a hello hello felülvizsgálja **(gyorsítótárazott), első erőforrás** hello minta Echo API működését.

> [!NOTE]
> Minden API Management service-példány az Echo API-k, melyek a használt tooexperiment és API-kezeléssel kapcsolatos további tudnivalók az előre konfigurált származnak. További információkért lásd: [Ismerkedés az Azure API Management szolgáltatással][Get started with Azure API Management].
> 
> 

tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás. Ezzel megnyitná toohello API Management publisher portálon.

![Közzétevő portál][api-management-management-console]

Kattintson a **API-k** a hello **API Management** hello maradt, és válassza a menü **Echo API**.

![Echo API][api-management-echo-api]

Hello kattintson **műveletek** fülre, majd hello **(gyorsítótárazott), első erőforrás** hello műveletet **műveletek** listája.

![Echo API műveletek][api-management-echo-api-operations]

Kattintson a hello **gyorsítótárazását** lapon tooview hello gyorsítótárazási beállítások ehhez a művelethez.

![Gyorsítótárazás lap][api-management-caching-tab]

művelet, jelölje be hello gyorsítótárazás tooenable **engedélyezése** jelölőnégyzetet. Ebben a példában engedélyezve van a gyorsítótárazás.

Minden egyes művelet válasz kulccsal, hello hello értékei alapján **lekérdezési karakterlánc paraméterei Vary** és **Vary fejléc által** mezőket. Ha azt szeretné, hogy a lekérdezési karakterlánc paramétereket vagy fejlécek alapján több válaszok toocache, konfigurálhatja azokat a következő két mező.

**Időtartam** hello lejárati időköz hello gyorsítótárazott válaszok. Ebben a példában a hello időköz: **3600** másodperc, amely egyenértékű tooone óra.

Ebben a példában a konfigurációs gyorsítótár hello használva hello első kérelem toohello **(gyorsítótárazott), első erőforrás** művelet hello háttérszolgáltatást választ ad vissza. Ez a válasz a gyorsítótárba fognak kerülni, által megadott hello kulccsal fejlécek és a lekérdezési karakterlánc-paraméter. Toohello műveletet a megfelelő paramétereket, lesz hello későbbi hívások gyorsítótárazott választ adott vissza, amíg hello időköz időtartam lejárt.

## <a name="caching-policies"></a>Felülvizsgálati hello házirendek gyorsítótárazása
Ebben a lépésben megtekintheti hello beállításainak gyorsítótárazás hello **(gyorsítótárazott), első erőforrás** hello minta Echo API működését.

Ha gyorsítótárazási beállításai a hello művelet **gyorsítótárazását** lap gyorsítótárazás házirendek kerülnek hello a művelethez. Ezek a házirendek tekinthetők meg és hello Helyicsoportházirend-szerkesztőben szerkeszti.

Kattintson a **házirendek** hello a **API Management** hello bal, majd válassza a menü **Echo API / ERŐFORRÁSKÉSZLET (gyorsítótárazott)** hello a **művelet**legördülő listából.

![Házirend-hatókör művelet][api-management-operation-dropdown]

A Helyicsoportházirend-szerkesztő hello hello házirendek ehhez a művelethez jeleníti meg.

![API Management házirendszerkesztő][api-management-policy-editor]

házirend-definíció hello a művelethez tartalmaz hello házirendekre konfigurációs gyorsítótárazás hello meghatározó elvégezték hello segítségével **gyorsítótárazását** hello előző lépésben fülre.

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
> Házirendek a Helyicsoportházirend-szerkesztő hello gyorsítótárazás végrehajtott módosítások toohello hello meg fog jelenni **gyorsítótárazását** lapján egy műveletet, és fordítva.
> 
> 

## <a name="test-operation"></a>Hívható meg egy műveletet, és tesztelje a hello gyorsítótárazása
toosee hello gyorsítótárazási működés közben is nevezzük hello művelet hello developer portálról. Kattintson a **fejlesztői portálján** hello jobb felső menüjében található.

![Fejlesztői portál][api-management-developer-portal-menu]

Kattintson a **API-k** a felső menüben hello, és válassza ki **Echo API**.

![Echo API][api-management-apis-echo-api]

> Ha csak egy API konfigurálva van, vagy látható tooyour fiókot, majd kattintson az API-k viszi közvetlenül toohello műveletek, hogy az API-hoz.
> 
> 

Jelölje be hello **(gyorsítótárazott), első erőforrás** műveletet, és kattintson **nyissa meg a konzolt**.

![Konzol megnyitása][api-management-open-console]

hello konzol lehetővé teszi tooinvoke műveletek közvetlenül a hello fejlesztői portálján.

![Konzol][api-management-console]

Tartsa hello alapértelmezett értékeinek **param1** és **paraméter2**.

Válassza ki a kívánt kulcsot az hello hello **előfizetés-kulcs** legördülő listából. Ha a fiókja csak egyetlen előfizetéssel rendelkezik, akkor az alapból ki lesz választva.

Adja meg **sampleheader:value1** a hello **kérelem fejlécei** szövegmezőben.

Kattintson a **HTTP Get** és jegyezze fel a hello response fejlécekkel együtt.

Adja meg **sampleheader:value2** a hello **kérelem fejlécei** szövegmezőbe, és kattintson **HTTP Get**.

Vegye figyelembe, hogy hello értékének **sampleheader** továbbra is **érték1** hello válaszként. Néhány más értékeivel, és vegye figyelembe, hogy a hello első hívás gyorsítótárazott válasz hello adja vissza.

Adja meg **25** történő hello **paraméter2** mezőben, majd kattintson a **HTTP Get**.

Vegye figyelembe, hogy hello értékének **sampleheader** hello a válasz mostantól **érték2**. A művelet eredménye hello lekérdezési karakterlánc szerinti kulccsal vannak ellátva, mert hello előző gyorsítótárazott válasz nem adott vissza.

## <a name="next-steps"></a>Következő lépések
* További információ a házirendek gyorsítótárazás: [házirendek gyorsítótárazás] [ Caching policies] a hello [API-felügyeleti szabályzatainak ismertetése][API Management policy reference].
* További információ az elemeknek a házirend-kifejezések kulcsával történő gyorsítótárazásáról: [Egyéni gyorsítótárazás az Azure API Management szolgáltatásban](api-management-sample-cache-by-key.md).

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
