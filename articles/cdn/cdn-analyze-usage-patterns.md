---
title: "aaaAnalyze Azure CDN használati minták |} Microsoft Docs"
description: "Tekintse meg a használati minták a CDN használatával a következő jelentések hello: sávszélesség, adatokat továbbít, a találatok, gyorsítótár állapotok, gyorsítótári találati aránya, IPV4/IPV6 adatokat továbbít."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 5a0d9018-8bdb-48ff-84df-23648ebcf763
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d27e6f60acaed66abb27d860c3a3e2e81c9f60cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN használati minták elemzése

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

hello útmutató az alábbi végig kell vinnie hello lépéseket tooview hello core jelentések Verizon profilok hello kezelése portálon. Core analytics adatok toostorage, az event hubs vagy Verizon és a Akamai naplóelemzés (oms) is exportálhat [hello azure portálon keresztül](cdn-log-analysis.md).

Tekintse meg a használati minták a CDN a következő jelentések hello használata:

* Sávszélesség
* Átvitt adatok
* A találatok
* Gyorsítótár-állapotok
* Gyorsítótár találati aránya
* Átvitt adatok IPv4/IPV6

## <a name="accessing-core-reports"></a>Core jelentések használata
1. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-reports/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
2. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **Core jelentések** menü.  Kattintson a kívánt hello jelentés hello menüben.
   
    ![CDN management portal - Core jelentések menü](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Sávszélesség
hello sávszélesség jelentés egy grafikonon és az adatok táblázatot hello a sávszélesség-használat a HTTP és HTTPS adott idő alatt áll. Hello sávszélesség az összes CDN POP vagy egy adott POP keresztül tekintheti meg. Ez lehetővé teszi, tooview hello forgalom teljesítményt és a terjesztési CDN POP MB/s mértékegységben.

* Válasszon csomópontjaihoz peremhálózati toosee forgalom minden csomópont, vagy egy adott régióban csomópont hello legördülő listából válassza.
* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb. vagy adjon meg egyéni dátumok, majd kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.
* Exportálhatja és hello kattintva hello adatok letöltése mellett található lap ikonra excel túl "lépjen".

hello jelentés 5 percenként frissül.

![Sávszélesség-jelentés](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Átvitt adatok
Ez a jelentés egy grafikonon és az adatok táblázatot hello forgalom használati HTTP és HTTPS adott idő alatt áll. Az összes CDN POP vagy egy adott POP hello forgalom használati tekintheti meg. Ez lehetővé teszi, tooview hello forgalom teljesítményt és a terjesztési CDN POP GB-ban.

* Minden megjegyzések csomópontjaihoz peremhálózati toosee forgalom válasszon, vagy egy adott régióban csomópont hello legördülő listából válassza.
* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb. vagy adjon meg egyéni dátumok, majd kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.
* Exportálhatja és hello kattintva hello adatok letöltése mellett található lap ikonra excel túl "lépjen".

hello jelentés 5 percenként frissül.

![Az átvitt adatmennyiség jelentés](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Találatok (állapotkód esetében)
Ez a jelentés kérelem állapotkódjai a tartalom terjesztése hello ismerteti. A tartalom minden kérelemnél hoz létre a HTTP-állapotkódot. hello állapotkód: ismerteti, hogyan történik a peremhálózati POP hello kérelem kezelése. Például 2xx állapotkódok jelző hello kérés sikeresen kiszolgálásának tooa ügyfelet, amíg egy 4xx állapotkód: azt jelzi, hogy hiba történt. HTTP-állapotkód kapcsolatos további tudnivalókért lásd: [állapotkódok](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb. vagy adjon meg egyéni dátumok, majd kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.
* Exportálhatja és hello kattintva hello adatok letöltése mellett található lap az excel túl "lépjen".

![A találatok jelentés](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Gyorsítótárak allapota
Ez a jelentés hello terjesztési találatot eredményező gyorsítótárbeli kereséseinek és az ügyfél kérésében gyorsítótárbeli ismerteti. Hello leggyorsabb teljesítmény találatot eredményező gyorsítótárbeli kereséseinek származik, mert adatok kézbesítési sebességű minimalizálja a gyorsítótárbeli és lejárt találatok is optimalizálhatja. Az eredeti kiszolgáló tooavoid "no-cache" válaszfejlécek hozzárendelése konfigurálásával, lehetőleg ne a lekérdezési karakterlánc gyorsítótárazás kivéve, ha feltétlenül szükséges és nem gyorsítótárazható válaszkódot elkerülésével gyorsítótárbeli csökkenteni lehet. Lejárt a gyorsítótár találatok elkerülhetők azáltal, hogy az eszköz maximális-életkora mindaddig, amíg lehetséges toominimize hello kérelmek toohello eredeti kiszolgálóra száma.

![Gyorsítótár-állapotok jelentés](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Fő gyorsítótár állapotok a következők:
* TCP_HIT: Kiszolgált szélétől. hello objektum a gyorsítótárban volt, és nem nagyobb a maximális életkora volt.
* TCP_MISS: Kiszolgált a forrásból. hello objektum nem a gyorsítótárban és hello válasz hátsó tooorigin.
* TCP_EXPIRED _MISS: kiszolgált forrásból eredete ismételt érvényesítése után. hello objektum a gyorsítótárban, de nagyobb a maximális életkora volt. A forrás egy ismételt érvényesítése hello gyorsítótár-objektumának származási új válaszára helyébe eredményezett.
* TCP_EXPIRED _HIT: kiszolgált szélétől eredete ismételt érvényesítése után. hello objektum a gyorsítótárban, de nagyobb a maximális életkora volt. Egy ismételt érvényesítése hello származási kiszolgálóval hello gyorsítótár-objektumának a módosítani kívánt eredményezett.
* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb. vagy adjon meg egyéni dátumok, majd kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.
* Exportálhatja és hello kattintva hello adatok letöltése mellett található lap ikonra excel túl "lépjen".

### <a name="full-list-of-cache-statuses"></a>Gyorsítótár-állapotok teljes listája
* TCP_HIT - kérést közvetlenül a hello POP toohello ügyfél kiszolgált van küldött ezt az állapotot. Egy eszköz azonnal kiszolgált hello POP legközelebbi toohello ügyfél vannak gyorsítótárazva, és van egy érvényes élő idő POP vagy TTL-t. A következő válaszfejlécek hello TTL határozza meg:
  
  * A Cache-Control: s-maxage
  * A Cache-Control: maximális-kor
  * Lejár
* TCP_MISS - azt jelzi, hogy a kért eszköz hello gyorsítótárazott verzió nem található a hello POP legközelebbi toohello ügyfél. hello eszköz kérni fogja az eredeti kiszolgálóra vagy egy eredeti pajzs kiszolgálóra. Ha hello eredeti kiszolgálóra vagy hello pajzs forráskiszolgáló adja vissza egy eszköz, toohello ügyfél kiszolgált és hello ügyfél és a hello peremhálózati kiszolgáló gyorsítótárában. Ellenkező esetben nem 200 állapotkódot (pl. 403 Tiltott, 404 nem található, stb.) adja vissza.
* TCP_EXPIRED _HIT - ezt az állapotot jelentett a kérelmeket, amelyek egy-egy lejárt TTL-t, például amikor hello eszköz maximális-életkora érvényessége lejárt, az eszköz megcélzott kiszolgálásának hello POP toohello ügyfél közvetlenül a Ha.
  
    Lejárt kérelmet általában egy ismételt érvényesítése kérelem toohello eredeti kiszolgálóra eredményez. Ahhoz, hogy egy TCP_EXPIRED _HIT toooccur hello származási kiszolgálónak jeleznie kell, hogy egy újabb verziója hello eszköz nem létezik. Ilyen helyzet általában frissíti ezt az eszközt a Cache-Control és Expires fejléc.
* TCP_EXPIRED _MISS - ezt az állapotot jelentett amikor hello POP toohello ügyfélről kiszolgált egy lejárt gyorsítótárazott eszköz egy újabb verziója. Ez akkor fordulhat elő, ha egy gyorsítótárazott eszköz TTL hello lejárt (pl. lejárt maximális-életkora) és hello származási adja meg, hogy az eszköz egy újabb verziója. Az új verzió hello eszköz szolgáltató toohello ügyfél hello gyorsítótárazott helyett. Továbbá azt a gyorsítótárban tartja hello peremhálózati kiszolgáló- és hello.
* CONFIG_NOCACHE - azt jelzi, hogy egy ügyfél-konfigurációs az oldal POP akadályoznia hello eszköz a gyorsítótárba.
* Nincs – Ez az állapot azt jelzi, hogy a tartalom frissesség ellenőrzése nem történt meg.
* TCP_ CLIENT_REFRESH _MISS - ezt az állapotot jelentett, amikor egy HTTP-ügyfél (pl. webböngésző) hello forráskiszolgálóról arra kényszeríti az edge POP tooretrieve egy új verziója elavult eszköz.
  
    Alapértelmezés szerint a kiszolgálóink megakadályozhatják, hogy egy HTTP-ügyfél kényszerítése a peremhálózati kiszolgáló tooretrieve hello eszközök új verziójának hello forráskiszolgálóról.
* TCP_ PARTIAL_HIT - Ez az állapot jelent, ha a bájttartománykéréseket eredményezi a részlegesen gyorsítótárazott eszköz találat. hello kért bájttartomány azonnal kiszolgált hello POP toohello ügyfélről.
* UNCACHEABLE – Ha egy eszköz a Cache-Control és Expires fejléc jelzi, hogy azt nem gyorsítótárazza a POP vagy hello HTTP-ügyfél által jelentett ezt az állapotot. Az ilyen típusú kérelmek szolgáltatott hello forráskiszolgálóról

## <a name="cache-hit-ratio"></a>Gyorsítótár találati aránya
Ez a jelentés azt jelzi, hogy közvetlenül a gyorsítótárból volt szolgáltatott gyorsítótárazott kérések hello aránya.

hello a jelentés tartalmazza a következő adatok hello:

* a kért hello a hello POP legközelebbi toohello kérelmező a tartalom gyorsítótárazva lett.
* hello kérés kiszolgálásának közvetlenül hello a hálózat széléről.
* hello kérelem nem volt szükség hello származási kiszolgálóval ismételt érvényesítése.

hello jelentés nem tartalmaz:

* Toocountry szűrési beállítások miatt elutasított kérelmek.
* Eszközök fejlécekhez jelzi, hogy azok nem gyorsítótárazza a kérelmeket. Például a Cache-Control: saját, a Cache-Control: no-cache vagy Pragma: no-cache fejlécek megakadályozza, hogy egy eszköz a gyorsítótárba.
* Tartomány kérelmek bájt részlegesen gyorsítótárazott tartalom.

hello képlet: (TCP_ TALÁLATI / (TCP_ TALÁLAT + TCP_MISS)) * 100

* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb. vagy adjon meg egyéni dátumok, majd kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.
* Exportálhatja és hello kattintva hello adatok letöltése mellett található lap ikonra excel túl "lépjen".

![Gyorsítótár találati arány jelentés](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4-/ IPV6 esetén az átvitt adatmennyiség
Ez a jelentés az IPV4 és IPV6 hello használati forgalomeloszlás tartalmazza.

![IPv4-/ IPV6 esetén az átvitt adatmennyiség](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Válassza ki a dátum tartomány tooview adatainak ma ezen a héten/ebben a hónapban, stb., vagy adjon meg egyéni dátumok.
* Kattintson az "Ugrás" toomake meg arról, hogy a kiválasztott frissül.

## <a name="considerations"></a>Megfontolandó szempontok
Jelentések csak hozható létre belül hello utolsó 18 hónap.

