---
title: "a Termékváltozat aaaChoose vagy IP-címek az Azure Search |} Microsoft Docs"
description: "Az Azure Search is üzembe ezek termékváltozatok: ingyenes, a Basic és standard szintű, ahol Standard lehetőség a különböző erőforrás-konfigurációk és kapacitása szintjének."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: eac9a09266db9da4900766808be3bc3b3ff11519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Termékváltozat vagy tarifacsomag választása az Azure Search szolgáltatáshoz
Az Azure Search egy [szolgáltatás kiépítve](search-create-service-portal.md) egy adott árképzési szint vagy a Termékváltozat. A választható lehetőségek **szabad**, **alapvető**, vagy **szabványos**, ahol **szabványos** több konfigurációk és kapacitások érhető el.

hello Ez a cikk célja toohelp úgy dönt, hogy a réteg. A réteg kapacitása elemről kiderül, hogy toobe túl alacsony, ha egy új szolgáltatás hello magasabb szintű használható tooprovision kell, és töltse be újra az indexek. Nincs azonos szolgáltatást egy SKU tooanother a hello helybeni frissítése.

> [!NOTE]
> A réteg a választott és [egy keresési szolgáltatás kiépítése](search-create-service-portal.md), később növelheti a replika- és partíció adatokra hello szolgáltatáson belül. Útmutatásért lásd: [erőforrás szintek lekérdezési és indexelési munkaterhelések méretezése](search-capacity-planning.md).
>
>

## <a name="how-tooapproach-a-pricing-tier-decision"></a>Hogyan tooapproach egy árképzési szintjüket döntési
Az Azure Search hello réteg kapacitását, és nem a szolgáltatás rendelkezésre állásával határozza meg. Szolgáltatások általában érhető el minden réteghez, beleértve az előzetes verziójú funkciókat. hello egyetlen kivétel ez alól a S3 HD indexelő esetében nem támogatott.

> [!TIP]
> Azt javasoljuk, hogy mindig mértékben egy **szabad** szolgáltatást (egy előfizetésenként lejárat nélkül), hogy passzív projektek a könnyen elérhető. Használjon hello **szabad** hozzon létre egy második számlázható szolgáltatás hello; tesztelés és értékelés szolgáltatás **alapvető** vagy **szabványos** réteget a termelési vagy nagyobb tesztelési célú alkalmazásokat.
>
>

Kapacitás és hello szolgáltatás futtatásával járó költségek nyissa meg kézzel az aktuális. Ebben a cikkben található információk segíthetnek eldönteni, rendszerrel SKU hello egyensúlyt, de bármely azt toobe hasznos, hello következő legalább durva becslést van szüksége:

* Száma és mérete indexek toocreate megtervezése
* Száma és mérete dokumentumok tooupload
* Összeállítottuk lekérdezés kötet, tekintetében lekérdezések egy második (QPS)

Száma és mérete fontosak, mert a legmagasabb keresztül rögzített korlátját hello indexek vagy egy szolgáltatást a dokumentumok száma, vagy hello szolgáltatás által használt erőforrások (tároló- és replikák) elérésekor. hello tényleges a szolgáltatás határa attól lekérdezi használt először: erőforrások vagy objektumokat.

Az aktuális becslések hello a következő lépéseket kell egyszerűsítésére hello:

* **1. lépés** hello SKU leírások toolearn rendelkezésre álló beállításokkal kapcsolatos alábbi áttekintése.
* **2. lépés** alább, egy előzetes döntési tooarrive hello kérdések megválaszolása.
* **3. lépés** döntés véglegesítéséhez szigorú korlátozásokat tároló áttekintése és az árképzés terén.

## <a name="sku-descriptions"></a>Termékváltozat leírása
hello a következő táblázat az egyes rétegek leírását tartalmazza.

| Szint | Elsődleges forgatókönyvek |
| --- | --- |
| **Ingyenes** |Megosztott szolgáltatás, díjmentesen, értékelése, a vizsgálat vagy a kis munkaterhelés használt. Más előfizetők van megosztva, mert lekérdezési átviteli sebesség és a indexelő függ ki másnak hello szolgáltatást használ. A kapacitása kis (50 MB vagy 3 indexek 10 000 fel dokumentumokat egyes). |
| **Basic** |Kis termelési számítási feladatokhoz a dedikált hardveren. Magas rendelkezésre állású. A kapacitása too3 replikák és 1 partíció (2 GB-ot). |
| **S1** |Standard 1 támogatja a partíciók (12) és a replikák (12), medium termelési számítási feladatokhoz a dedikált hardveren használt rugalmas kombinációi. Partíciók és replikák maximális száma 36 számlázható keresési egység által támogatott kombinációkban foglalhatja le. Ezen a szinten partíció található, 25 GB és QPS körülbelül 15 lekérdezések száma másodpercenként. |
| **S2** |Standard 2 fut hello azonos 36 keresési egységek használatával S1 megegyezik, azonban nagyobb méretű partíciókat és a replikák nagyobb termelési számítási feladatokhoz. Ezen a szinten partíció található, a 100 GB és QPS készül 60 lekérdezések száma másodpercenként. |
| **S3** |Szabványos 3 futó arányosan nagyobb termelési számítási feladatokhoz magasabb end rendszerek too12 partíciók vagy 12 replikák beállításait a 36 keresési egység. Ezen a szinten partíció található, a 200 GB és QPS másodpercenként több mint 60 lekérdezések. |
| **S3 HD** |Standard 3 nagy sűrűségű kisebb indexek nagy számú tervezték. Másolatot too3 partíciók, 200 GB lehet. QPS másodpercenként több mint 60 lekérdezések. |

> [!NOTE]
> Replika és a partíció méretkorlát egységként keresési (36 egységek maximális száma a szolgáltatás), mint milyen hello maximális értékét, magában foglalja a hatékony alsó határának előíró számlázzuk ki. Például a toouse hello legfeljebb 12 replikák legfeljebb 3 partíció lehet (12 * 3 = 36 egységek). Ehhez hasonlóan toouse maximális partíciók, csökkentse a replikák too3. Lásd: [erőforrás szintjeinek lekérdezési és indexelési munkaterhelések az Azure Search méretezése](search-capacity-planning.md) megengedett kombinációk diagram.
>
>

## <a name="review-limits-per-tier"></a>Tekintse át a határon belül az egyes réteg
hello következő diagram része a hello essen [az Azure Search szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md). Felsorolja hello tényezők valószínűleg tooimpact SKU döntést hoznak. Az alábbi hello kérdések felülvizsgálatakor olvassa el a toothis diagram.

| Erőforrás | Ingyenes | Alapszintű | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Szolgáltatói szerződés (SLA) |No <sup>1</sup> |Igen |Igen |Igen |Igen |Igen |
| Index korlátok |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| A dokumentum korlátok |10 000 teljes |1 millió szolgáltatás |15 millió partíciónként |60 millió partíciónként |120 millió partíciónként |1 millió index |
| Maximális partíciók |N/A |1 |12 |12 |12 |3 <sup>2</sup> |
| Partíció mérete |Összesen 50 MB |2 GB-ot szolgáltatás |Partíciónként 25 GB |100 GB-ot partíció (felfelé tooa legfeljebb 1.2-es TB-szolgáltatás esetében) |200 GB-ot partíció (felfelé tooa legfeljebb 2,4 TB-szolgáltatás esetében) |200 GB-os (felfelé tooa legfeljebb 600 GB-szolgáltatás esetében) |
| Maximális replikák |N/A |3 |12 |12 |12 |12 |
| Lekérdezések száma másodpercenként |N/A |~3 replikánként |~15 replikánként |~60 replikánként |>60 replikánként |>60 replikánként |

<sup>1</sup> ingyenes szint és az előzetes funkciók nem tartoznak a szolgáltatásszint-szerződések (SLA). Az összes számlázható rétegek SLA-k érvénybe, ha a szolgáltatás megfelelő redundancia kiépítése. Két vagy több replikák szükségesek (olvasás) lekérdezés SLA-t. Három vagy több replikák lekérdezés és az indexelés (írásra) SLA szükségesek. a partíciók számának hello nincs egy SLA-t szempont. 

<sup>2</sup> S3 és S3 HD azonos nagy kapacitású infrastruktúra által támogatott, de azok eléri a maximális korlátját különböző módon. S3 nagyon nagy indexek kevesebb célozza. Mint ilyen, a maximális korlátját az erőforrás-kötött (minden egyes szolgáltatás 2,4 TB). S3 HD nagyon kis indexek nagy számú célozza. S3 HD 1000 indexek eléri a hello formában tárgymutató megkötéseket teljes kapacitásukkal működjenek. Ha egy több mint 1000 indexek szükséges S3 HD ügyfelünk, forduljon a Microsoft Support kapcsolatos információk tooproceed.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Kiküszöbölése termékváltozatok követelményeinek nem megfelelő.
a következő kérdések hello segítséget nyújt a kiszolgálófarmban lévő hello megfelelő SKU döntés a munkaterhelés számára.

1. Rendelkezik **szolgáltatási szint szerződés (SLA)** követelmények? Használhat bármilyen számlázható réteg (alapvető a mentést), de konfigurálnia kell a redundancia érdekében. Két vagy több replikák szükségesek (olvasás) lekérdezés SLA-t. Három vagy több replikák lekérdezés és az indexelés (írásra) SLA szükségesek. a partíciók számának hello nincs egy SLA-t szempont.
2. **Hány indexeli** szükség van-e? Az SKU döntést faktoring hello legnagyobb változók egyik minden SKU által támogatott indexek hello száma. Index támogatási van jelentősen különböző szintjein lévő alacsonyabb hello tarifacsomag szükséges. Indexek száma vonatkozó követelmények egy SKU döntés elsődlegesen meghatározó tényező lehet.
3. **Hány dokumentumok** lesz betöltve minden indexbe? hello számát és méretét, a dokumentumok hello index hello végleges méretét határozza meg. Feltételezve megbecsülheti hello hello index tervezett méretét, összehasonlíthatja, hogy a kívánt hello partíció méretet a Termékváltozat kiterjesztett partíció szükséges toostore hello száma alapján ez a méret index ellen.
4. **Mi az az hello várt lekérdezés terhelését**? Miután tárhellyel kapcsolatos követelmények értendők, fontolja meg a lekérdezés munkaterhelések. S2 és mindkét S3 termékváltozatok biztosítják a közelében egyenértékű átviteli, miközben a szolgáltatásszint-szerződési követelményeit minden előzetes termékváltozatok fog kizárása.
5. Ha hello S2 vagy S3 réteg, szükségességének eldöntése [indexelők](search-indexer-overview.md). Indexelő még nem állnak rendelkezésre hello S3 HD szinthez. Alternatív megoldás, ahol írhat kódot toopush alkalmazás, egy adatkészlet tooan index toouse index frissítések leküldéses modellt.

A legtöbb ügyfél szabály is egy adott SKU bejövő vagy kimenő a fenti kérdések a válaszok toohello alapján. Ha még nem meg arról, hogy mely SKU toogo rendelkező, felteheti kérdéseit tooMSDN StackOverflow fórumok, vagy Azure ügyfélszolgálatához kapcsolatos további iránymutatásért.

## <a name="decision-validation-does-hello-sku-offer-sufficient-storage-and-qps"></a>Döntési érvényesítése: hello SKU elegendő tárhely és QPS ajánlja fel?
Utolsó lépésként le újra hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/search/) és hello [szolgáltatásra vonatkozó korlátozások szolgáltatás és az-index részeiben](search-limits-quotas-capacity.md) toodouble-ellenőrzés a becslések előfizetés és a szolgáltatás korlátozások.

Ha bármelyik hello ár vagy tárolási követelményei értéktartományon kívül esik, érdemes toorefactor hello munkaterhelésének több kisebb szolgáltatások (például). Részletesebb felügyeletét, a indexek toobe kisebb volt Újratervezés és a használata hatékonyabb toomake lekérdezések szűrők.

> [!NOTE]
> Tárolási követelmények túlzott felfújt lehet, ha a dokumentumok oda nem tartozó adatokat tartalmaznak. Ideális esetben a dokumentumok csak a kereshető adatokat vagy a metaadatokat tartalmaznak. Bináris adatok nem kereshető, ezért kell tárolni külön-külön (lehet, hogy az Azure tábla vagy a blob storage) hello index toohold egy URL-cím referenciaadatok külső toohello mezőt. hello maximális egy adott dokumentumot mérete 16 MB (vagy ha kevesebb mint egy kérelem több dokumentum feltöltése tömeges). Lásd: [szolgáltatási korlátait, az Azure Search](search-limits-quotas-capacity.md) további információt.
>
>

## <a name="next-step"></a>Következő lépés
Miután eldöntötte, amely SKU jobbra igazítása hello, folytassa az alábbi lépéseket:

* [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md)
* [A szolgáltatás hello lefoglalása a partíciók és replikák tooscale módosítása](search-capacity-planning.md)
