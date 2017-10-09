---
title: "az SQL Data Warehouse aaaDistributing táblák |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblák terjesztése."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 65093eeaeb00fef85aaa6070da2c976fed3f4bbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse táblák terjesztése
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Adattípusok][Data Types]
> * [Terjesztése][Distribute]
> * [Index][Index]
> * [Partíció][Partition]
> * [Statisztika][Statistics]
> * [Ideiglenes][Temporary]
>
>

Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozási (MPP) elosztott adatbázisrendszerre épül.  Az adatok és a feldolgozási képesség több csomópont közötti elosztásával az SQL Data Warehouse olyan rendkívüli méretezhetőséget kínál, amely messze meghaladja bármilyen különálló rendszer lehetőségeit.  Annak eldöntése, hogyan toodistribute az adatokat az SQL Data warehouse az egyik legfontosabb hello tényezők tooachieving optimális teljesítményét.   hello kulcs toooptimal teljesítmény van kis méretre adatmozgás, vagy pedig hello kulcs toominimizing adatmozgás hello megfelelő telepítési stratégia jelöl.

## <a name="understanding-data-movement"></a>Adatátvitel ismertetése
Minden tábla hello adatait az MPP rendszer több alapul szolgáló adatbázisok között oszlik meg.  hello legtöbb optimalizált lekérdezések az MPP rendszeren is egyszerűen átadható tooexecute hello a beavatkozás nélküli egyes elosztott adatbázisok közötti hello más adatbázisok.  Például tételezzük fel az értékesítési adatait, amely tartalmazza a két tábla, értékesítéssel és az ügyfelek olyan adatbázist.  Ha egy lekérdezést, amelyet a toojoin az értékesítési tooyour felhasználói tábla, és hogy nullával az értékesítési és a felhasználói táblák mentése ügyfélszámot, külön adatbázisban mindegyik ügyfél üzembe, a csatlakozás az értékesítés és a felhasználói lekérdezések belül minden megoldhatók nincsenek saját ismeretei az adatbázis hello más adatbázisok.  Ezzel szemben, ha az értékesítési adatok osztva számát és az ügyféladatok ügyfél száma szerint, majd bármely adott adatbázis nem lesz hello tartozó adatokat minden ügyfél esetében, ezért ha toojoin az értékesítési adatokat tooyour ügyféladatok, kellene tooget hello adatok a hello minden ügyfél esetében más adatbázisok.  A második példában adatmozgás kellene toooccur toomove hello adatok toohello értékesítési ügyféladatok, úgy, hogy a két hello táblák társíthatók.  

Adatátvitel nem mindig egy rossz dolgot tegyenek, egyes esetekben szükséges toosolve lekérdezést.  De ha ezt a kiegészítő lépést el kell kerülni, természetesen a lekérdezés fog futni gyorsabb.  Adatátvitel leggyakrabban akkor keletkezik, ha a táblák tartományhoz csatlakoztatott vagy Összesítés hajtja végre.  Gyakran kell toodo mindkét, így bár bizonyára tudni toooptimize egy forgatókönyvet, például a csatlakozzon, Ön továbbra is igényel, oldja meg az adatok mozgása toohelp hello más forgatókönyv, például egy összesítő.  hello trükk rendszer tudja, amelyek értéke kevesebb munka.  A legtöbb esetben nagy ténytáblák általában illesztett oszlopon kiosztása van hello hello csökkenti a leghatékonyabb módszer legtöbb adatátvitelt jelölik.  Az illesztési oszlopok adatok terjesztése egy sokkal közös metódus tooreduce adatmozgás mint összesítést részt vevő oszlopokat adatok terjesztése.

## <a name="select-distribution-method"></a>Elosztási módszer kiválasztása
Hello háttérben SQL Data Warehouse felosztja az adatok 60 adatbázisok.  Minden egyes adatbázis hivatkozott tooas egy **terjesztési**.  Adatok betöltése az egyes táblanevek, az SQL Data Warehouse rendelkezik tooknow hogyan toodivide az adatok így vannak elrendezve a 60 terjesztéseket.  

hello elosztási módszer hello tábla szintjén van meghatározva, és jelenleg két választási lehetőség:

1. **Ciklikus multiplexelés** egyenletesen, de véletlenszerűen elosztják adatokat.
2. **Elosztott kivonatoló** osztja el a kivonatolás csak egy oszlop értékeinek alapján

Alapértelmezés szerint nem határoznak meg a telepítési módszert, ha a tábla eloszlik a hello segítségével **ciklikus multiplexelés** elosztási módszer.  Azonban ha már kifinomultabb a megvalósításában, érdemes tooconsider használatával **elosztott kivonatoló** táblák toominimize adatmozgás, ami viszont optimalizálja a lekérdezések teljesítményét.

### <a name="round-robin-tables"></a>Ciklikus multiplexelés táblák
Ciklikus multiplexelés metódus adatok elosztása hello használata túlságosan azok hogyan úgy tűnik, hogy.  Ahogy az az adatok be van töltve, minden egyes sorára egyszerűen toohello következő terjesztési küld.  Ez a módszer elosztása hello adatokat fog mindig véletlenszerűen hello adatok nagyon egyenletes elosztása összes hello terjesztéseket.  Ez azt jelenti, hogy nincs rendezés kész során hello round robin folyamattal, amely az adatokat helyezi.  Ciklikus multiplexelés terjesztési ezért nevezik véletlenszerű kivonatát.  Ciklikus multiplexelés elosztott tábla nincs szükség toounderstand hello adat.  Emiatt ciklikus multiplexelés gyakran táblázatokkal jó betöltése célokat.

Alapértelmezés szerint nincs elosztási módszer választása esetén hello ciklikus multiplexelés elosztási módszer használható.  Azonban amíg ciklikus multiplexelés táblák könnyen toouse, mert az adatok véletlenszerűen hello rendszer, az azt jelenti, hogy a rendszer hello nem garantálható, hogy mely terjesztési pontjain. mindegyik sor van.  Eredményeként hello rendszer néha igények tooinvoke egy adatok adatátviteli művelet toobetter rendszerezi az adatokat ahhoz, hogy feloldja egy lekérdezést.  Ezt a kiegészítő lépést lelassíthatja a lekérdezéseket.

Vegye figyelembe, hogy a táblázat a forgatókönyv a következő hello ciklikus multiplexelés eloszlás használatával:

* Ha egyszerű kiindulási pontként első lépések
* Ha nincs egyértelmű csatlakozó kulcs
* Ha nem jó jelölt oszlop a kivonat hello tábla terjesztése
* Ha hello tábla nem osztja meg egy közös illesztési kulcs más táblák
* Ha hello illesztési kevésbé fontos, mint más illesztések hello lekérdezés
* Egy ideiglenes átmeneti tárolási tábla hello tábla esetén

A példák is létrehoz egy ciklikus multiplexelés táblát:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Ciklikus multiplexelés pedig hello alapértelmezett táblatípus alatt a DDL-t a explicit tekinthető az ajánlott eljárás, hogy a table elrendezést hello céljaira egyértelmű tooothers.
>
>

### <a name="hash-distributed-tables"></a>Elosztott táblák kivonatoló
Használja a **elosztott kivonatoló** algoritmus toodistribute a táblák a jobb teljesítmény érdekében számos forgatókönyv az adatmozgás csökkentése a lekérdezés idejét.  Elosztott táblák listáját táblákat, amelyek között hello kivonatoló elosztott adatbázisok csak egy oszlop, amely választ a kivonatolási algoritmust használja.  hello terjesztési oszlop, mi határozza meg, hogyan hello adatok felosztásának az elosztott adatbázisok között.  hello kivonatoló funkció hello terjesztési oszlop tooassign sorok toodistributions.  hello kivonatoló algoritmust, és eredményül kapott terjesztési determinisztikus.  Vagyis hello hello ugyanannál az adattípusnál lesz mindig azonos értéket tartalmaz toohello azonos terjesztési.    

Ez a példa létrehoz egy táblát, a terjesztés azonosítója:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Válassza ki a terjesztési oszlop
Ha úgy dönt, túl**kivonatoló terjesztése** egy tábla tooselect egyetlen terjesztési oszlop szüksége lesz.  Jelöljön ki egy terjesztési oszlopot, amikor nincsenek három fő tényezők tooconsider.  

Válassza ki a csak egy oszlop, amelyek:

1. Nem frissíthető
2. Adatok, egyenletes elosztása elkerülhető az a döntés adatok
3. Adatátvitel minimalizálása érdekében

### <a name="select-distribution-column-which-will-not-be-updated"></a>Válassza ki a terjesztési oszlop, amely nem frissíthető
Terjesztési oszlopok nem frissíthető, ezért, válasszon statikus értékeket tartalmazó oszlop.  Egy oszlop toobe frissíteni kell, esetén általában nem a megfelelő terjesztési jelölt.  Ha egy olyan esetben, ha frissítenie kell egy terjesztési oszlop, ez végezhető először a hello sor törlése, és ezután az új sorok beszúrásához.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Válassza ki a terjesztési oszlop, amely egyenletes elosztása adatok
Elosztott rendszer csak olyan gyorsan, mint a leglassabb kiosztás hajt végre, mert azt nem fontos toodivide hello munkahelyi egyenletesen hello terjesztéseket rendelés elosztott terhelésű tooachieve végrehajtása hello rendszer között.  hello munkahelyi oszlik elosztott rendszerek hello módon ahol hello adatokat az egyes terjesztési él alapul.  Ennek köszönhetően nagyon fontos tooselect hello megfelelő terjesztési oszlop hello adatok terjesztéséhez, hogy minden terjesztési egyenlő munkahelyi és a rendszer hajtsa végre a megfelelő hello azonos idő toocomplete hello munkahelyi részét.  Munkahelyi jól oszlik hello rendszer között, amikor hello adatok kiegyensúlyozott hello terjesztéseket között.  Adatok nem egyenletesen eloszlik, amikor ez nevezzük **adatok döntés**.  

toodivide adatok egyenletesen és eltérésére adatok elkerüléséhez hello következő megfontolandó a terjesztési oszlop kiválasztása:

1. Válasszon ki egy oszlopot, amely számos különböző értékeket tartalmazza.
2. Ne elosztása néhány különböző értékeket tartalmazó oszlopok adatokat.
3. Kerülje a NULL értékek nagy gyakorisággal oszlopokon adatok terjesztése.
4. Kerülje a dátumoszlopot adatok terjesztése.

Mivel minden egyes érték 60 azokat a terjesztéseket, kivonatolt too1, még akkor is, terjesztési tooachieve érdemes tooselect egy oszlopot, amely magas egyedi, és több mint 60 egyedi értékeket tartalmaz.  tooillustrate, fontolja meg egy olyan esetben, ahol egy oszlop csak rendelkezik 40 egyedi értékeket.  Ebben az oszlopban hello terjesztési kulcs volt kiválasztva, ha, hogy a tábla adatai hello volna mindkét 40 azokat a terjesztéseket, legfeljebb 20 terjesztéseket nincsenek adatok, és nem feldolgozási toodo hagyja.  Ezzel ellentétben hello más 40 terjesztéseket kellene, hogy ha hello adatok lett egyenlően elosztva több mint 60 terjesztéseket további munkahelyi toodo.  Ebben a forgatókönyvben az adatok eltérésére egy példája.

MPP rendszerben minden egyes lekérdezés lépés megvárja-e a minden terjesztéseket toocomplete hello munka a megosztást.  Ha egy terjesztési végez műveletet több mint mások hello, akkor a hello erőforrás hello más terjesztéseket vannak lényegében kihasználatlan kell hello foglalt terjesztési csak vár.  Ha munkahelyi nem egyenlően elosztva összes azokat a terjesztéseket, ez nevezzük **eltérésére feldolgozási**.  Feldolgozási döntés lekérdezések toorun lassabb, mint ha hello munkaterhelés is lehet egyenlően elosztva hello terjesztéseket miatt.  Adatok döntés eltérésére tooprocessing irányítja.

Elkerülése hello null értékek összes megnyílik a hello azonos módon magas nullázható oszlopában terjesztése terjesztési. A dátum oszlop terjesztése is eredményezheti, hogy eltérésére feldolgozása, mert egy adott dátumhoz tartozó összes adatot megnyílik a hello azonos terjesztési. Ha több felhasználó is feldolgozás alatt álló összes szűrési lekérdezések hello azonos dátum, akkor csak a hello 60 terjesztéseket 1 lesz aki összes hello munkahelyi mivel egy adott időpontban csak egy terjesztési. Ebben a forgatókönyvben hello lekérdezések valószínűleg lassabb, mint ha hello adatok lettek egyaránt eloszlatva összes hello terjesztéseket 60 alkalommal futtathatja.

Ha nem jó jelölt oszlop létezik, majd vegye fontolóra a ciklikus multiplexelés hello elosztási módszer használatát.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Válassza ki a terjesztési oszlop, ami minimálisra csökkentheti adatátvitel
Adatátvitel minimalizálja a hello megfelelő terjesztési oszlop kiválasztásával egyike hello legfontosabb stratégiák az SQL Data Warehouse teljesítményének optimalizálásához.  Adatátvitel leggyakrabban akkor keletkezik, ha a táblák tartományhoz csatlakoztatott vagy Összesítés hajtja végre.  Használt az oszlopok `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` és `HAVING` készítése az összes záradékok **jó** terjesztési jelöltek kivonat.

Ugyanakkor, hello oszlopai a hello `WHERE` záradék do **nem** gondoskodjon megfelelő kivonatoló oszlop jelöltek mert azok korlátozza, mely azokat a terjesztéseket hello lekérdezés feldolgozása, amely részt döntés.  Egy oszlop, amely lehet, tempting toodistribute, de gyakran okozhat a feldolgozási eltérésére jó példa a dátum oszlop.

Általánosan fogalmazva Ha két nagy ténytáblák gyakran érintett szerepel, érheti el hello legtöbb teljesítményét azáltal, hogy mindkét tábla egyik hello illesztési oszlop.  Ha egy táblázatot, amely soha nem csatlakoztatott tooanother nagy ténytábla, majd keresse meg, amelyek gyakran hello toocolumns `GROUP BY` záradékban.

Van néhány főbb feltételeket, amelyek az illesztés során fennállnak tooavoid adatmozgás kell lennie:

1. hello hello illesztési érintett táblákhoz kell lennie a terjesztés kivonatoló **egy** hello oszlopok hello illesztési részt.
2. hello illesztési oszlopok adattípusai hello meg kell egyeznie a két tábla között.
3. az egyenlő operátor szerinti szűrése hello oszlopok kell csatlakoznia.
4. hello illesztési típus nem lehet egy `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Hibaelhárítási adatokat eltérésére
Amikor a tábla adatai hello kivonatoló telepítési módszerrel van egy lehetőségét, hogy az egyes terjesztési lesz toohave aránytalanul több adat, mint a többire válik egyenetlenné. Eltérésére túl sok adatot befolyásolhatja a lekérdezési teljesítményt, mert hello végső eredménye egy elosztott lekérdezést kell várnia, hogy hello leghosszabb futó terjesztési toofinish. Attól függően, hello adatok eltérésére szükség lehet a tooaddress hello fokú azt.

### <a name="identifying-skew"></a>Döntés azonosítása
Egy egyszerű módon tooidentify válik egyenetlenné táblázattá toouse `DBCC PDW_SHOWSPACEUSED`.  Ez az egy igen gyors és egyszerű módon toosee hello minden hello 60 disztribúciók az adatbázis tárolt tábla sorok száma.  Ne feledje, hogy a legtöbb kiegyensúlyozott hello teljesítmény érdekében az elosztott tábla sorainak hello kell kell elosztva egyenletesen valamennyi hello felosztást.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Azonban ha lekérdezést hajt végre hello Azure SQL Data Warehouse dinamikus felügyeleti nézetekkel (DMV) végezhet részletesebb elemzését.  toostart, hello nézet létrehozása [dbo.vTableSizes] [ dbo.vTableSizes] használatával megtekintheti az SQL hello [tábla áttekintése] [ Overview] cikk.  Hello nézet létrehozása után futtassa a lekérdezés tooidentify táblák rendelkezhet több mint 10 % adatok döntés.

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Adatok eltérésére feloldása
Nem minden eltérésére van elegendő toowarrant egy javítást.  Bizonyos esetekben néhány lekérdezést egy olyan táblázatában hello teljesítményét is járó hello kárt eltérésére adatok.  Ha oldja fel az adatok toodecide döntés egy tábla tisztában kell lennie hello adatok mennyiségéről és lekérdezések lehetőség szerint a terhelést a.   Döntés hello hatását a egyirányú toolook toouse hello lépéseit hello [lekérdezés figyelési] [ Query Monitoring] toomonitor hello hatását a lekérdezési teljesítmény eltérésére a következő cikket, és kifejezetten hello hatás toohow hosszú lekérdezések hello egyedi terjesztéseket toocomplete igénybe.

Adatok terjesztése, hogy a rendszer hello egyensúlyt a minimalizálja a adatok döntés és minimalizálja az adatmozgás között. Ezek is ellentétes célok, és egyes esetekben érdemes tookeep adatok rendelés tooreduce adatátvitelt jelölik a döntés. Például ha hello terjesztési oszlop gyakran összekapcsolásokhoz és az aggregációhoz megosztott oszlopa hello, akkor lesz kell minimalizálása adatátvitelt jelölik. hello hello minimális adatmozgás előnyös lehet, hogy járó döntés adatok hello hatását.

hello jellemző módon tooresolve adatok döntés toore-hello tábla létrehozása egy másik terjesztési oszloppal. Mivel nincs mód toochange hello terjesztési oszlop egy olyan tábla, hello módon toochange hello terjesztési táblázat azt toorecreate azt egy [CTAS] [].  Az alábbiakban a két példa bemutatja, hogyan oldhatók fel az adatok eltérésére:

### <a name="example-1-re-create-hello-table-with-a-new-distribution-column"></a>1. példa: Hozza létre egy új terjesztési oszlop hello tábla
Ez a példa [CTAS] [] toore-hozzon létre egy táblát egy másik kivonatoló terjesztési oszlop.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] too[FactInternetSales];
```

### <a name="example-2-re-create-hello-table-using-round-robin-distribution"></a>2. példa: Hozza létre a ciklikus multiplexelés terjesztés hello tábla
Ez a példa [CTAS] [] toore-ciklikus multiplexelés kivonatoló terjesztési helyett hozzon létre egy táblát. Ez a változás a művelet létrehoz még akkor is, adatok terjesztési hello költségekkel megnövekedett adat mozgása.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename hello tables
RENAME OBJECT [dbo].[FactInternetSales] too[FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] too[FactInternetSales];
```

## <a name="next-steps"></a>Következő lépések
toolearn Táblatervezés, kapcsolatos további információkért lásd: hello [elosztás][Distribute], [Index][Index], [partíció] [ Partition], [Adattípusok][Data Types], [statisztika] [ Statistics] és [ideiglenes táblák] [ Temporary] cikkeket.

Ajánlott eljárások áttekintését lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
