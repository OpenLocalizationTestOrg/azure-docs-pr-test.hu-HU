---
title: "aaaHow tooadd műveletek tooan API az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd műveletek tooan API az Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a>Hogyan tooadd műveletek tooan API az Azure API Management
Egy API-t az API Management használata előtt hozzá kell adni műveletek. Ez útmutató bemutatja hogyan tooadd és műveletek tooan API különböző típusú konfigurálhatja az API Management.

## <a name="add-operation"></a>Művelet hozzáadása
Műveletek bekerülnek és tooan API a hello publisher portálon. tooaccess hello publisher portálon kattintson **Publisher portal** a hello Azure portál, az API Management szolgáltatás.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Hello jelölje be a keresett API hello publisher portálon, és jelölje ki hello **műveletek** fülre. 

![Műveletek][api-management-operations]

Kattintson a **művelet hozzáadása** tooadd új művelet. Hello **új művelet** jelenik meg, és hello **aláírás** alapértelmezett kiválasztása lapon.

![A művelet hozzáadása][api-management-add-operation]

Adja meg a hello **HTTP-műveletet** hello legördülő listából válassza ki.

![HTTP-metódus][api-management-http-method]

<a name="url-template"></a>

Sablon hello URL-CÍMÉT, írja be egy vagy több URL-cím elérési út szegmensek és nulla vagy több lekérdezési karakterláncot paraméterek URL-cím töredéket. hello URL-cím sablon hozzáfűzött toohello alap URL-CÍMÉT hello API-t egyetlen HTTP-művelet azonosítja. Tartalmazhat egy vagy több nevű változó részeit, amelyek nincsenek azonosítva kapcsos zárójelek. Változó részei Sablonparaméterek hívják, és dinamikusan kiosztott értékeket kinyert hello kérelem URL-címe, ha hello kérés feldolgozása folyamatban van hello API platformja.

> hello URL-cím sablon helyettesítő mintákat tartalmazhat. Például megadó `/*` előre, hogy HTTP metódus toohello vissza az összes kérelem véget ér szolgáltatás.

![URL-cím sablon][api-management-url-template]

<a name="rewrite-url-template"></a>

Ha szükséges, adja meg a hello **URL-cím újraírása sablon**. Ez lehetővé teszi toouse hello szabványos URL-cím sablon az előtér-hello beérkező kérelmek feldolgozásához, hello háttér-egy toohello szerint konvertált URL-CÍMEN keresztül hívása közben újraírási sablont. Sablonparaméterek hello URL-cím sablonból hello átdolgozás sablonban kell használni. hello következő példa bemutatja, hogyan tartalomtípus, elérésiút-szegmens hello webszolgáltatás hello előző példa megadható egy lekérdezés paramétere hello hello keresztül közzétett API hello URL-cím sablonok segítségével az API Management platform kódolású.

![Sablon átdolgozás URL-címe][api-management-url-template-rewrite]

Hívóknak toohello műveletet fogják használni hello `/customers?customerid=ALFKI` és ez túl rendelendő`/Customers('ALFKI')` amikor meghívták hello háttér-szolgáltatás.

**Megjelenítési** nevét és **leírás** hello művelet leírását adhatja meg, és használt tooprovide dokumentáció toohello fejlesztők számára az API használatával hello developer portálon.

![Leírás][api-management-description]

hello művelet leírását is megadni egyszerű szöveg-vagy HTML hello **leírás** szövegmezőben.

## <a name="operation-caching"></a>Művelet gyorsítótárazása
Válasz gyorsítótárazását csökkenti a késést érzékelt hello API fogyasztó, csökkenti a sávszélesség-használat és csökkenő hello terhelése hello HTTP webes szolgáltatás végrehajtási hello API. 

tooeasily és gyorsan gyorsítótárazása hello a művelethez, jelölje be hello **gyorsítótárazását** fülre és ellenőrizze a hello **engedélyezése** jelölőnégyzetet.

![Gyorsítótárazás][api-management-caching-tab]

**Időtartam** megadja hello időszak során melyik hello művelet válasz hello gyorsítótárában marad. hello alapértelmezett értéke 3600 másodperc, vagyis 1 óra.

Gyorsítótár kulcsai között válaszok használt toodifferentiate, hogy a megfelelő tooeach különböző gyorsítótárkulcshoz hello választ kap a saját külön gyorsítótárazott érték. Szükség esetén adja meg a megadott lekérdezési karakterlánc paraméterek és/vagy a gyorsítótár-értékeit hello számítástechnikai használt HTTP-fejlécek toobe **lekérdezési karakterlánc paraméterei Vary** és **Vary fejléc által** szövegdobozok kulcsattribútumokkal. Ha nincs megadott, teljes kérelem URL-CÍMÉT, és a következő HTTP-fejléc értékei hello használt gyorsítótár Kulcslétrehozási: **elfogadás** és **Accept-Charset**.

> A és a házirendek gyorsítótárazást további információkért lásd: [hogyan toocache művelet eredménye az Azure API Management][How toocache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Kérelem paraméterei
Művelet paramétereit felügyelt hello paraméterek lapon. Hello megadott paraméterek **URL-cím sablon** a hello **aláírás** lapon a rendszer automatikusan hozzáadja, és csak a hello URL-cím sablon szerkesztésével módosíthatja. További paraméterek manuálisan is megadhatók.

Kattintson egy új lekérdezési paraméter tooadd **lekérdezési paraméter hozzáadása** , és írja be a következő információ hello:

* **Név** -paraméter neve.
* **Leírás** -hello paraméter (választható) rövid leírása.
* **Típus** -paramétertípus hello legördülő lista a kijelölt.
* **Értékek** -értékeket, amelyeket toothis paraméter lehet hozzárendelni. Hello értékek egyike lehet állítani alapértelmezettként (nem kötelező).
* **Szükséges** -hello paraméter hello jelölőnégyzet bejelölésével kötelezővé tételéhez. 

![A kérelemben szereplő paraméterek][api-management-request-parameters]

## <a name="request-body"></a>Kérelem törzse
Ha hello művelet lehetővé teszi, hogy (pl. PUT, POST), és előfordulhat, hogy adja meg például az összes hello törzs támogatott ábrázolását formátumokban (pl. json, XML) igényel. 

> hello kérelemtörzset csak dokumentáció célokat szolgál, és nincs érvényesítve.
> 
> 

a kérelem törzsében tooenter kapcsoló toohello **törzs** lapon.

Kattintson a **ábrázolását adja hozzá**, írjon be a kívánt típus neve (például application/json), jelölje ki a legördülő lista hello és beillesztés hello szükséges kérelem törzse példa hello kijelölt formátumban hello szövegmezőbe. 

![Kérés törzsében][api-management-request-body]

A további toorepresentations is megadható egy nem kötelező leírás a hello **leírás** szövegmezőben.

## <a name="responses"></a>Válaszok
Egy célszerű tooprovide példák hello művelet készíthet összes állapotkódokat a válaszokat. Előfordulhat, hogy minden állapotkód egynél több válasz törzsében példa, minden hello támogatott tartalomtípusokat. 

tooadd választ, kattintson a **Hozzáadás** és kezdje el begépelni szükséges hello állapotkódot. Példa hello állapota kódja: **200 OK**. Miután hello legördülő hello kód jelenik meg, válassza ki azt, és hello válaszkód létrehozott és a hozzáadott tooyour műveletet.

![Válaszkód][api-management-response-code]

Kattintson a **ábrázolását adja hozzá**, kezdje el begépelni hello kívánt típus neve (például application/json), és jelölje ki azt a hello legördülő listán.

![Törzs tartalom típusa][api-management-response-body-content-type]

Illessze be hello válasz törzsében példa hello kijelölt formátumban hello szövegmezőben. 

![Választörzs][api-management-response-body]

Ha szükséges, megadhat egy leírást, a hello **leírása** szövegmezőben.

Ha konfigurálva van a hello műveletet, kattintson a **mentése**.

## <a name="next-steps"></a>Következő lépések
Miután hello műveletek bekerülnek a tooan API, hello tovább tooassociate hello API termékkel rendelkező és közzéteszi az, hogy a fejlesztők meghívhatja a műveleteket.

* [Hogyan toocreate és a termék közzététele][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
