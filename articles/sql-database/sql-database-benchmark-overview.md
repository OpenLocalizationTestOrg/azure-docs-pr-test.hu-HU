---
title: "aaaAzure SQL-adatbázis referenciaalap áttekintésében"
description: "Ez a témakör ismerteti a hello Azure SQL adatbázis teljesítményteszt használni a hello Azure SQL Database teljesítményét."
services: sql-database
documentationcenter: na
author: jan-eng
manager: jhubbard
editor: monicar
ms.assetid: e26f8a66-2c12-49d7-8297-45b4d48a5c01
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/21/2016
ms.author: janeng
ms.openlocfilehash: 1024e9ada511935f911cb1345b4dc5508997c702
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-benchmark-overview"></a>Az Azure SQL Database teljesítménytesztek áttekintése
## <a name="overview"></a>Áttekintés
A Microsoft Azure SQL Database nyújt három [szolgáltatásszintek](sql-database-service-tiers.md) különböző teljesítményszintek. Minden egyes teljesítményszintet biztosít egy készletét erőforrásokhoz, vagy a "power" tervezett toodeliver egyre nagyobb teljesítmény növelése.

Fontos toobe képes tooquantify hogyan egyes növekvő hatványa hello fordítja le nagyobb adatbázis-teljesítményt is. a Microsoft kifejlesztette toodo hello Azure SQL adatbázis Referenciaalap (ASDB). hello teljesítményteszt gyakorolja vegyesen található összes OLTP-munkaterhelések az alapvető műveleteket. Mérjük hello átviteli teljesíteni az egyes futó adatbázisok.

hello erőforrások és a tápkábelek minden egyes szolgáltatást és teljesítményszintet szint szerint van megadva a [adatbázis-tranzakciós egységek (dtu-k)](sql-database-what-is-a-dtu.md). Dtu-k toodescribe hello relatív kapacitását a teljesítményszintet Processzor, memória, kevert mértéke alapján és olvasási és írási sebesség egyes által kínált adja meg. Így dupla hello DTU minősítés adatbázis csatlakozás toodoubling hello adatbázis power. hello teljesítményteszt lehetővé teszi tooassess hello hatása az adatbázis teljesítményének növelése által tényleges adatbázis-művelet gyakorló arányban adatbázis mérete, a felhasználók számát, és tranzakciós díjakat biztosítanak méretezés közben egyes által kínált power hello toohello biztosított erőforrások toohello adatbázis.

Által hello átviteli hello alapvető szolgáltatásréteg tranzakciók-óránként használatával kifejező, hello szabványos szolgáltatásréteg tranzakciók perc és hello prémium szolgáltatásszintet tranzakciók másodpercenkénti, teszi, hogy könnyebben tooquickly használatával hello vonatkoznak minden egyes réteg toohello követelményei az alkalmazások teljesítményének lehetséges.

## <a name="correlating-benchmark-results-tooreal-world-database-performance"></a>Adatok teljesítményteszt eredmények tooreal globális adatbázis teljesítménye
Ez fontos toounderstand adott ASDB, például az összes referenciaalapokhoz képest, a reprezentatív és tájékoztató csak. hello tranzakció hello teljesítményteszt alkalmazással elért díjszabás fog nem lehet hello ugyanúgy, előfordulhat, hogy érhető el, egyéb alkalmazásokkal. hello teljesítményteszt típusok futtatni egy séma tartalmazó táblák és adattípusokat számos különböző tranzakció gyűjteményét tartalmazza. Hello teljesítményteszt gyakorlatok hello alapszintű műveletek közös tooall az OLTP-munkaterhelések, amíg nem jelent az adatbázis vagy az alkalmazás bármely adott osztály. hello hello teljesítményteszt célja tooprovide egy ésszerű útmutató toohello relatív várható amikor felfelé vagy lefelé skálázás teljesítményszintek között adatbázis teljesítménye. A valóságban adatbázisok különböző méretű és összetettségét, munkaterhelések különböző keverékei előforduló, és különböző módokon reagál. Például egy IO-igényes alkalmazás előfordulhat, hogy hamarabb találati IO küszöbértékeket, vagy egy processzorigényes alkalmazás előfordulhat, hogy hamarabb találati CPU-korlátok. Nem biztos, hogy bármely adott adatbázis méretezni a azonos módon növelésével hello alapként betöltése hello van.

hello teljesítményteszt és annak módszereket ismertetik részletesebben az alábbi.

## <a name="benchmark-summary"></a>Teljesítményteszt összegzése
ASDB méri az online tranzakció-feldolgozási (OLTP) munkaterhelések leggyakrabban előforduló alapszintű adatbázis-műveletek vegyesen hello teljesítményét. Bár hello teljesítményteszt célja a felhőalapú informatika szem előtt, hello adatbázisséma, az adatok feltöltése és a tranzakciók voltak tervezett toobe széleskörűen képviseli az OLTP-munkaterhelések leggyakrabban használt alapvető elemei hello.

## <a name="schema"></a>Séma
hello séma tervezett toohave van elegendő különböző és a bonyolultságot toosupport műveletek széles körét. hello teljesítményteszt fut egy adatbázist hat táblák áll. hello táblák három kategóriába sorolhatók: rögzített méretű, méretezés és növekedését. Két rögzített méretű táblák; három méretezési táblák; és egy egyre bővülő tábla. Rögzített méretű sorok számának állandó kell. Méretezési arányos toodatabase teljesítmény, de nem módosítja a hello teljesítménytesztek során számosságuk kell. például egy méretezési táblázat első betöltéskor tábla növekvő hello mérete, de majd hello számossága módosításait hello állomásokon futó hello teljesítményteszt sort beszúrni, és törölni.

hello séma tartalmaz vegyesen adattípusok, beleértve az egész szám, numerikus, a karakter, és a dátum/idő. hello séma tartalmaz az elsődleges és másodlagos kulcsok, de nem idegen kulcsokkal – Ez azt jelenti, hogy nincsenek hivatkozási integritási megkötés táblák között.

Adatok generálása program hello adatokat hello kezdeti adatbázis állít elő. Az egész és numerikus adatokat a különböző stratégiák jön létre. Bizonyos esetekben az értékek vannak osztva véletlenszerűen tartományban. Más esetekben az értékek egy halmazát, hogy megőrződjön-e egy adott terjesztési véletlenszerűen a(z) tooensure. Szövegmező szavak tooproduce reális kinézetű adatok súlyozott listájából jönnek létre.

hello adatbázis mérete alapján egy "Mérettényező." hello méretezési tényező (KB rövidítése) a méretezés és táblák növekvő hello hello számossága határozza meg. Az alábbiakban leírtak hello szakasz felhasználók és Pacing, hello adatbázis mérete, a felhasználók számát, és minden egyéb arányban tooeach méretezése a legjobb teljesítmény.

## <a name="transactions"></a>Tranzakciók
hello munkaterhelés áll kilenc tranzakciótípusok, ahogy az az alábbi táblázat hello. Minden tranzakció tervezett toohighlight egy meghatározott hello adatbázis-motor és a rendszer hardver jellemzői, kontrasztos megjelenítés a a hello más tranzakciók. Ez a megközelítés lehetővé teszi különböző összetevői toooverall teljesítmény könnyebb tooassess hello hatása. Például "Olvasás nehéz" hello tranzakció eredménye olyan jelentős lemezről az olvasási műveletek.

| Tranzakció típusa | Leírás |
| --- | --- |
| Olvassa el a Lite |VÁLASSZA KI A; a memóriában; csak olvasható |
| Olvasási közepes |VÁLASSZA KI A; legtöbbször a memóriában; csak olvasható |
| Olvasási gyakori |VÁLASSZA KI A; többnyire nincs a memóriában; csak olvasható |
| A frissítés Lite |FRISSÍTÉS; a memóriában; olvasási és írási |
| Gyakori frissítése |FRISSÍTÉS; többnyire nincs a memóriában; olvasási és írási |
| INSERT Lite |INSERT; a memóriában; olvasási és írási |
| Gyakori beszúrása |INSERT; többnyire nincs a memóriában; olvasási és írási |
| Törlés |TÖRLÉS; a memóriában, és nem memóriában; olvasási és írási |
| CPU gyakori |VÁLASSZA KI A; a memóriában; CPU-terhelés viszonylag nagy mennyiségű; csak olvasható |

## <a name="workload-mix"></a>Munkaterhelés-vegyes
Tranzakciók vannak véletlenszerűen kiválasztott egy súlyozott terjesztési hello a következő általános kettőn együtt. hello általános vegyes van egy olvasási/írási arányt körülbelül 2:1.

| Tranzakció típusa | Vegyes % |
| --- | --- |
| Olvassa el a Lite |35 |
| Olvasási közepes |20 |
| Olvasási gyakori |5 |
| A frissítés Lite |20 |
| Gyakori frissítése |3 |
| INSERT Lite |3 |
| Gyakori beszúrása |2 |
| Törlés |2 |
| CPU gyakori |10 |

## <a name="users-and-pacing"></a>Felhasználók és pacing
hello teljesítményteszt munkaterhelés célja a egy eszköz, amellyel a tranzakciók kapcsolatok toosimulate hello viselkedését számos egyidejű felhasználók csoportja között. Annak ellenére, hogy az összes hello kapcsolatok és a tranzakciók gép jön létre, az egyszerűség kedvéért irányítjuk toothese kapcsolatok felhasználóként"." Bár minden felhasználó működése független a többi felhasználó, a minden felhasználó hello hajtsa végre az alábbi lépéseket azonos ciklus:

1. Egy adatbázis-kapcsolat létrehozásához.
2. Ismételje meg a jelzett tooexit amíg:
   * Véletlenszerű (a súlyozott terjesztési) egy olyan tranzakciót választott.
   * Hajtsa végre a kijelölt hello tranzakció és mérték hello válaszidő.
   * Várjon, amíg pacing késést.
3. Zárja be az adatbázis-kapcsolat hello.
4. Kilépés.

késleltetése (2/c. lépés) pacing hello véletlenszerűen kiválasztott-e, de a terjesztési, amely rendelkezik 1.0-ás másodpercenként átlagosan. Így minden felhasználóhoz, átlagosan generálhat másodpercenként legfeljebb egy tranzakciót.

## <a name="scaling-rules"></a>Skálázási szabályok
felhasználók száma hello hello adatbázis mérete (Mérettényező egységekben) határozza meg. Nincs minden öt Mérettényező egységek egy felhasználó. Hello pacing késleltetés, mert egy felhasználó hozhat létre a másodpercenként legfeljebb egy tranzakciót átlagosan.

Például egy méretezési tényezős 500 (KB = 500) adatbázis 100 felhasználót lesz, és a 100 TP-k maximális sebesség érhető el. a magasabb TP-k mértékben toodrive szükséges további felhasználók és egy nagyobb adatbázist.

az alábbi táblázat hello felhasználók ténylegesen az egyes szolgáltatási szint és a teljesítmény szűk hello számát jeleníti meg.

| Szolgáltatási réteg (teljesítményszint szükséges) | Felhasználók | Adatbázisméret |
| --- | --- | --- |
| Basic |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (s2 –) |50 |7.1 GB |
| Prémium (P1) |100 |14 GB |
| Prémium (P2) |200 |28 GB |
| Prémium (P6/P3) |800 |114 GB |

## <a name="measurement-duration"></a>Mérési időtartama
Egy érvényes teljesítményteszt futtatásához igényel a stabil állapot mérési időtartama legalább egy órát.

## <a name="metrics"></a>Mérőszámok
alapvető metrikákat hello hello teljesítményteszt olyan átviteli sebesség és a válaszidőt.

* Átviteli sebesség hello alapvető teljesítménymérés hello teljesítményteszt. Átviteli sebesség jelentésekről--időegység, minden tranzakciótípus számbavételi tranzakciókat.
* Válaszidő teljesítmény kiszámíthatóságot mérőszáma. Válasz időkorlát hello szolgáltatást, magasabb osztályok szigorúbb válasz idő követelmény, hogy alább látható módon szolgáltatás osztály függ.

| Szolgáltatás osztálya | Átviteli sebesség mérték | Válasz idő követelmény |
| --- | --- | --- |
| Prémium |Másodpercenkénti tranzakciók |95. százalékos érték 0,5 másodperc: |
| Standard |Percenkénti tranzakciók |90. százalékos érték 1.0-ás másodperc: |
| Basic |Óránkénti tranzakciók |80 PERCENTILIS 2.0 másodperc: |

## <a name="conclusion"></a>Összegzés
hello Azure SQL adatbázis teljesítményteszt relatív teljesítménye hello Azure SQL Database keresztül elérhető szolgáltatásszintek és teljesítményszintek hello számos méri. hello teljesítményteszt alapszintű adatbázis-műveletek, az online tranzakció-feldolgozási (OLTP) munkaterhelések leggyakrabban előforduló vegyesen gyakorolja. Aktuális teljesítménye mérésével hello teljesítményteszt Ez a témakör a részletesebb elemzés hello hatással lehet az átviteli sebességének módosítása hello teljesítményszint szükséges, mint ha csak az egyes szinteken, például a Processzor sebessége, a memóriaméret és a IOPS hello erőforrásokat listázása . Hello jövőbeli, a rendszer továbbra is tooevolve hello teljesítményteszt toobroaden hatókörében és bontsa ki a megadott hello adatokat.

## <a name="resources"></a>Erőforrások
[Bevezetés tooSQL adatbázis](sql-database-technical-overview.md)

[Szolgáltatásszintek és teljesítményszintek](sql-database-service-tiers.md)

[Az önálló adatbázisok teljesítményének útmutató](sql-database-performance-guidance.md)
