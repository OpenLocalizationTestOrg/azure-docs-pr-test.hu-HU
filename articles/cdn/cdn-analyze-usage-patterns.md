---
title: "Azure CDN használati minták elemzése |} Microsoft Docs"
description: "Tekintse meg a használati minták használatával az alábbi jelentések a CDN: sávszélesség, adatokat továbbít, a találatok, gyorsítótár állapotok, gyorsítótári találati aránya, IPV4/IPV6 adatokat továbbít."
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
ms.openlocfilehash: aadbe872dd3384c8d337b432fb3be69422ca322b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN használati minták elemzése

[!INCLUDE[cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Az alábbi útmutató végighalad a lépéseket, a kezelés portálon Verizon profilok core-jelentések megtekintéséhez. Előfordulhat, hogy is tárhelyre, az event hubs alapvető analitikai adatok exportál vagy analytics (oms) keresse meg a Verizon és a Akamai [az azure portálon keresztül](cdn-log-analysis.md).

Tekintse meg a használati minták a CDN használata a következő:

* Sávszélesség
* Átvitt adatok
* A találatok
* Gyorsítótár-állapotok
* Gyorsítótár találati aránya
* Átvitt adatok IPv4/IPV6

## <a name="accessing-core-reports"></a>Core jelentések használata
1. A CDN-profil panelje, kattintson a **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-reports/cdn-manage-btn.png)
   
    Megnyitja a CDN-felügyeleti portálon.
2. Vigye a **Analytics** lapra, és vigye a **Core jelentések** menü.  Kattintson a kívánt jelentést, a menüben.
   
    ![CDN management portal - Core jelentések menü](./media/cdn-reports/cdn-core-reports.png)

## <a name="bandwidth"></a>Sávszélesség
A sávszélesség a jelentés egy grafikonon és az adatok táblázatot, amely tartalmazza a sávszélesség-használat a HTTP és HTTPS adott idő alatt áll. A sávszélesség-használat összes CDN POP vagy egy adott POP keresztül tekintheti meg. Ez lehetővé teszi, hogy a forgalom teljesítményt és a terjesztési megtekintése CDN POP a MB/s között.

* Minden peremhálózati csomópont összes csomópontjának hálózati forgalom, vagy válasszon egy adott régióban csomópont a legördülő listából válassza ki.
* Válassza ki a dátumtartományt megtekintése a mai ezen a héten/e havi adatokat stb. vagy adjon meg egyéni dátumok, majd kattintson a "" Győződjön meg arról, hogy a kiválasztott frissül.
* Exportálni, és töltse le az adatokat az "Ugrás" melletti excel lap ikonra kattintva.

A jelentés 5 percenként frissül.

![Sávszélesség-jelentés](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Átvitt adatok
Ez a jelentés egy grafikonon és az adatok táblázatot a forgalom használati HTTP és HTTPS adott idő alatt áll. A forgalom használata az összes CDN POP vagy egy adott POP keresztül tekintheti meg. Ez lehetővé teszi, hogy a forgalom teljesítményt és a terjesztési megtekintése között CDN POP GB-ban.

* Válassza ki az összes jegyzetet hálózati forgalom, vagy a legördülő listából válassza ki a régió/csomópont csomópontjaihoz peremhálózati.
* Válassza ki a dátumtartományt megtekintése a mai ezen a héten/e havi adatokat stb. vagy adjon meg egyéni dátumok, majd kattintson a "" Győződjön meg arról, hogy a kiválasztott frissül.
* Exportálni, és töltse le az adatokat az "Ugrás" melletti excel lap ikonra kattintva.

A jelentés 5 percenként frissül.

![Az átvitt adatmennyiség jelentés](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Találatok (állapotkód esetében)
Ez a jelentés a kérelem állapotkódok a tartalom terjesztési ismerteti. A tartalom minden kérelemnél hoz létre a HTTP-állapotkódot. Az állapotkód: ismerteti, hogyan történik a peremhálózati POP a kérelem kezelése. Például 2xx állapotkódok jelzi, hogy a kérés sikeresen kiszolgálásának ügyfélnek, amíg egy 4xx állapotkód: azt jelzi, hogy hiba történt. HTTP-állapotkód kapcsolatos további tudnivalókért lásd: [állapotkódok](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

* Válassza ki a dátumtartományt megtekintése a mai ezen a héten/e havi adatokat stb. vagy adjon meg egyéni dátumok, majd kattintson a "" Győződjön meg arról, hogy a kiválasztott frissül.
* Exportálni, és töltse le az adatokat az "Ugrás" melletti excel-táblában.

![A találatok jelentés](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Gyorsítótárak allapota
Ez a jelentés a terjesztési találatot eredményező gyorsítótárbeli kereséseinek és az ügyfél kérésében gyorsítótárbeli ismerteti. Mivel a leggyorsabb teljesítmény találatot eredményező gyorsítótárbeli kereséseinek származik, adatok kézbesítési sebességű minimalizálja a gyorsítótárbeli és lejárt találatok optimalizálható. A forrás kiszolgálón, hogy ne rendelje hozzá a "no-cache" válaszfejlécek konfigurálásával, lehetőleg ne a lekérdezési karakterlánc gyorsítótárazás kivéve, ha feltétlenül szükséges és nem gyorsítótárazható válaszkódot elkerülésével gyorsítótárbeli csökkenteni lehet. Találatok elkerülhetők azáltal, hogy egy eszköz lejárt gyorsítótár maximális-életkora mindaddig, amíg az eredeti kiszolgálóra kérelmek számának minimalizálása érdekében a lehető végpontjára.

![Gyorsítótár-állapotok jelentés](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Fő gyorsítótár állapotok a következők:
* TCP_HIT: Kiszolgált szélétől. Az objektum a gyorsítótárban volt, és nem nagyobb a maximális életkora volt.
* TCP_MISS: Kiszolgált a forrásból. Az objektum nem a gyorsítótárban, és a válasz vissza az eredeti volt.
* TCP_EXPIRED _MISS: kiszolgált forrásból eredete ismételt érvényesítése után. Az objektum a gyorsítótárban volt, de nagyobb a maximális életkora volt. A forrás egy ismételt érvényesítése a felváltja a forrásból új választ gyorsítótár-objektumának eredményezett.
* TCP_EXPIRED _HIT: kiszolgált szélétől eredete ismételt érvényesítése után. Az objektum a gyorsítótárban volt, de nagyobb a maximális életkora volt. A forrás-kiszolgálóval a ismételt érvényesítése a módosítani kívánt gyorsítótár-objektumának eredményezett.
* Válassza ki a dátumtartományt megtekintése a mai ezen a héten/e havi adatokat stb. vagy adjon meg egyéni dátumok, majd kattintson a "" Győződjön meg arról, hogy a kiválasztott frissül.
* Exportálni, és töltse le az adatokat az "Ugrás" melletti excel lap ikonra kattintva.

### <a name="full-list-of-cache-statuses"></a>Gyorsítótár-állapotok teljes listája
* TCP_HIT – amikor egy kérelem kiszolgált a POP-ről az ügyfél ezt az állapotot jelentett. Egy eszköz azonnal kiszolgált POP, amikor az ügyfél legközelebb POP a gyorsítótárban van és van-e egy érvényes élő idő vagy TTL-t. A következő válaszfejlécek TTL határozza meg:
  
  * A Cache-Control: s-maxage
  * A Cache-Control: maximális-kor
  * Lejár
* TCP_MISS - azt jelzi, hogy a kért eszköz gyorsítótárazott verzió nem található meg az ügyfél legközelebb POP. Az eszköz kérni fogja az eredeti kiszolgálóra vagy egy eredeti pajzs kiszolgálóra. Ha az eredeti kiszolgálóra vagy a forráskiszolgáló pajzs adja vissza egy eszköz, az ügyfélprogram és az ügyfél és a peremhálózati kiszolgáló gyorsítótárazza. Ellenkező esetben nem 200 állapotkódot (pl. 403 Tiltott, 404 nem található, stb.) adja vissza.
* TCP_EXPIRED _HIT - Ez az állapot jelent, ha a kérelmeket, amelyek egy-egy lejárt TTL-t, ha az eszköz maximális-életkora érvényessége lejárt, például az eszköz megcélzott állítása és kiszolgálása között a POP-ről az ügyfélnek.
  
    Lejárt kérelmet általában annak az eredménye ismételt érvényesítése kérelemben az eredeti kiszolgálóra. Ahhoz, hogy egy TCP_EXPIRED _HIT megtörténik a forrás kiszolgálónak jeleznie kell, hogy az eszköz egy újabb verziója nem létezik. Ilyen helyzet általában frissíti ezt az eszközt a Cache-Control és Expires fejléc.
* TCP_EXPIRED _MISS – amikor egy lejárt gyorsítótárazott eszköz egy újabb verziója az ügyfélnek a POP-ra a kiszolgált ezt az állapotot jelentett. Ez akkor fordulhat elő, ha egy gyorsítótárazott eszköz Élettartama lejárt (pl. lejárt maximális-életkora) és az eredeti kiszolgálóra adja vissza egy adott eszközre újabb verziója. Az eszköz ezen új verziójával szolgáltató az ügyfélnek a gyorsítótárazott verzió helyett. Továbbá azt a gyorsítótárba fognak kerülni a peremhálózati kiszolgáló és az ügyfélen.
* CONFIG_NOCACHE - azt jelzi, hogy egy ügyfél-konfigurációs az oldal POP miatt nem sikerült az eszköz a gyorsítótárba helyezésből.
* Nincs – Ez az állapot azt jelzi, hogy a tartalom frissesség ellenőrzése nem történt meg.
* TCP_ CLIENT_REFRESH _MISS – amikor egy HTTP-ügyfél (pl. webböngésző) kényszeríti a forráskiszolgálóról egy új verziója elavult eszköz beolvasandó POP él ezt az állapotot jelentett.
  
    Alapértelmezés szerint a kiszolgálóink megakadályozhatják, hogy egy HTTP-ügyfél kényszerítése a peremhálózati kiszolgálóinak beolvasása az eszköz új verziójának a forráskiszolgálóról.
* TCP_ PARTIAL_HIT - Ez az állapot jelent, ha a bájttartománykéréseket eredményezi a részlegesen gyorsítótárazott eszköz találat. A kért bájttartomány azonnal kiszolgált a a csatlakozási pont az ügyfélhez.
* UNCACHEABLE – Ha egy eszköz a Cache-Control és Expires fejléc jelzi, hogy azt nem gyorsítótárazza a POP- vagy HTTP-ügyfél által jelentett ezt az állapotot. Az ilyen típusú kérelmek szolgáltatott a forráskiszolgálóról

## <a name="cache-hit-ratio"></a>Gyorsítótár találati aránya
Ez a jelentés azt jelzi, hogy közvetlenül a gyorsítótárból volt szolgáltatott gyorsítótárazott kérelem azon százaléka.

A jelentés tartalmazza a következő adatokat:

* A kért tartalom a legközelebbi igénylő POP gyorsítótárazva lett.
* A kérés kiszolgálásának közvetlenül a hálózati szélétől.
* A kérelem nem volt szükség a forrás-kiszolgálóval ismételt érvényesítése.

A jelentés nem tartalmaz:

* Ország szűrési beállítások miatt elutasított kérelmek.
* Eszközök fejlécekhez jelzi, hogy azok nem gyorsítótárazza a kérelmeket. Például a Cache-Control: saját, a Cache-Control: no-cache vagy Pragma: no-cache fejlécek megakadályozza, hogy egy eszköz a gyorsítótárba.
* Tartomány kérelmek bájt részlegesen gyorsítótárazott tartalom.

A képlet: (TCP_ TALÁLATI / (TCP_ TALÁLAT + TCP_MISS)) * 100

* Válassza ki a dátumtartományt megtekintése a mai ezen a héten/e havi adatokat stb. vagy adjon meg egyéni dátumok, majd kattintson a "" Győződjön meg arról, hogy a kiválasztott frissül.
* Exportálni, és töltse le az adatokat az "Ugrás" melletti excel lap ikonra kattintva.

![Gyorsítótár találati arány jelentés](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4-/ IPV6 esetén az átvitt adatmennyiség
Ez a jelentés az IPV4 és IPv6-alapú forgalom használati eloszlását mutatja.

![IPv4-/ IPV6 esetén az átvitt adatmennyiség](./media/cdn-reports/cdn-ipv4-ipv6.png)

* Jelölje ki a mai ezen a héten/e havi adatokat stb megtekintéséhez, vagy adjon meg egyéni dátumok dátumtartományt.
* Kattintson az "Ugrás" Győződjön meg arról, hogy a kiválasztott frissül.

## <a name="considerations"></a>Megfontolandó szempontok
Jelentések csak az utolsó 18 hónapon belül hozható létre.

