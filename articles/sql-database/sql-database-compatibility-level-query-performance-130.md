---
title: "Adatbázis-kompatibilitási szint 130 - Azure SQL Database |} Microsoft Docs"
description: "Ez a cikk azt előnyei az Azure SQL Database 130 kompatibilitási szinten fut, és hogy az új lekérdezésoptimalizáló és a lekérdezés processzorfunkciókat előnyeit kihasználva. Azt is eleget a lehetséges-hatással a lekérdezési teljesítményt, a meglévő SQL-alkalmazásokhoz."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Javult a lekérdezési teljesítmény kompatibilitási szint 130 az Azure SQL-adatbázis
Azure SQL-adatbázis fut transzparens módon akár több ezer adatbázis több száz számos különböző kompatibilitási szinten megőrzi, és az összes ügyfél számára a megfelelő verzióra, a Microsoft SQL Server az előző verziókkal való kompatibilitás biztosítása!

Ez a cikk azt előnyei az Azure SQL adatbázismodell 130 kompatibilitási szinten fut, és az új lekérdezésoptimalizáló és a lekérdezés processzorfunkciókat előnyeit kihasználva. Azt is eleget a lehetséges-hatással a lekérdezési teljesítményt, a meglévő SQL-alkalmazásokhoz.

Ne feledje az előzményeket SQL-verziók alapértelmezett kompatibilitási szintre igazítása a következők:

* 100: az SQL Server 2008 és az Azure SQL adatbázis-V11.
* 110: az SQL Server 2012 és az Azure SQL adatbázis-V11.
* 120: az SQL Server 2014 és az Azure SQL-adatbázis 12-es verzió.
* 130: az SQL Server 2016-os és az Azure SQL-adatbázis 12-es verzió.

> [!IMPORTANT]
> Kezdve **2016. június mid**, az Azure SQL-adatbázis, az alapértelmezett kompatibilitási szintje lesz helyett 120 130 **az újonnan létrehozott** adatbázisok.
> 
> 2016. június mid előtt létrehozott adatbázisokat fogja *nem* érinti, és a jelenlegi kompatibilitási szint (100, 110-esre vagy 120) rendelkeznek. V12 Azure SQL Database V11-es verziójából áttelepített adatbázisok fog rendelkezni 100 vagy a 110-es kompatibilitási szintet. 
> 

## <a name="about-compatibility-level-130"></a>130 kompatibilitási szintre vonatkozó
Ha szeretné tudni, hogy az adatbázis jelenlegi kompatibilitási szintjét, először a következő Transact-SQL-utasítás végrehajtása.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Mielőtt szint 130 a módosítás történik az **újonnan** hozott létre adatbázisok, most áttekintheti az ezt a módosítást minden szól bizonyos nagyon alapszintű lekérdezés példákból, és tekintse meg, hogyan bárki is kihasználhatja le azt.

A lekérdezés feldolgozása a relációs adatbázisok nagyon összetett feladat lehet, és nagy mennyiségű számítástechnikai és matematikai megtudhatja, hogy a rejlő tervezési döntések ütköznek azokkal és viselkedéshez vezethet. Ebben a dokumentumban a tartalom szándékosan egyszerűsített, győződjön meg arról, hogy bárki, aki néhány minimális technikai háttér megismerheti a hatását a kompatibilitási szint változás, és határozza meg az alkalmazások használatának előnyei.

Mi a kompatibilitási szint 130 számos lehetőséget kínál a táblát gyorsan át most rendelkezik.  További részletei: [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), de rövid összegzése:

* Az Insert művelet Insert választás utasítás többszálas lehetnek, vagy hogy a párhuzamos terv, mielőtt ez a művelet lett egyszálas során.
* Memória optimalizált tábla és a tábla változók lekérdezések most lehet párhuzamos terveket, mielőtt ez a művelet lett is egyszálas során.
* A Memóriaoptimalizált tábla statisztikája most mintát vehetnek és automatikus frissítése. Lásd: [az adatbázis-kezelő újdonságai: memórián belüli online Tranzakciófeldolgozási](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) további részleteket.
* Az oszlop az Oszlopcentrikus indexek módosítja sor módú kötegelt módban v/s
  * Egy oszlop Oszlopcentrikus indexszel rendelkező táblán rendezi most kötegelt módban van.
  * Leképezési összesítések kötegelt módban, például a TSQL LAG/ÁTFUTÁSI utasítások működni.
  * A distinct záradékban több oszlop tárolási táblákon lekérdezések kötegelt módban működik.
  * DOP mellett futó lekérdezésekhez = 1 vagy soros csomagot is hajtsa végre a kötegelt módban.
* Utolsó számossága becslés fejlesztései ténylegesen érkeznek-es kompatibilitási szintű 120, de azokat (azaz 100 vagy 110), alacsonyabb kompatibilitási szintű futtatásával az áthelyezés kompatibilitási szint 130 is megjelenik a fejlesztéseknek, és ezeket is a lekérdezési teljesítményt, az alkalmazások kihasználják.

## <a name="practicing-compatibility-level-130"></a>Kompatibilitási szint 130 gyakorlás
Első folytassuk az egyes táblák, indexek és az új lehetőségekhez némelyike gyakorlata létrehozott véletlenszerű adatokat. A TSQL-parancsfájl például SQL Server 2016-os, illetve az Azure SQL Database hajtható végre. Azure SQL-adatbázis létrehozása esetén ellenőrizze, hogy választja, a legalább P2 adatbázis, mivel szüksége legalább néhány többszálas engedélyezése, és ezért ezek a funkciók kihasználhassa magszámra vonatkozó követelmény.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Most tegyük a tekintse meg a lekérdezés feldolgozása funkciók hamarosan-es kompatibilitási szintű 130 egyes rendelkezik.

## <a name="parallel-insert"></a>Párhuzamos BESZÚRÁSA
Az INSERT művelet a kompatibilitási szint 120 és 130, melyik rendre egyetlen összefűzött modellben (120), és a többszálas modellben (130) hajtja végre a beszúrási művelet végrehajtása a TSQL-utasításokat az alábbi hajtja végre.

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Kérésével a tényleges a lekérdezésterv, és megnézi a grafikus ábrázolása vagy az XML-tartalmat, mely számossága becslés függvény jelenleg play azt is meghatározhatja. 1. ábra a megnézi a tervek-mellé, egyértelműen láthatja, hogy az oszlop tároló BESZÚRÁSA végrehajtási tartalma a 120 soros párhuzamos 130. Emellett vegye figyelembe, hogy a módosítás a iterátor ikon megjelenítése két párhuzamos nyilat ábrázoló azt a tényt, ami már iterátor végrehajtása 130 tervben valóban párhuzamos. Ha nagy a beszúrási műveletek végrehajtásához, csatolva van az adatbázis, a rendelkezésére core száma párhuzamos végrehajtása javítja a teljesítményt; legfeljebb 100 alkalommal, amely gyorsabb a helyzettől függően!

*1. ábra: INSERT művelet állapotról soros párhuzamos kompatibilitási szint 130.*

![1. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SOROS kötegelt módban
Hasonlóképpen a áthelyezését a kompatibilitási szintet 130 sornyi adatot feldolgozásakor engedélyezi kötegfeldolgozási mód. Először kötegelt módban futó műveletekhez esetén csak érhetők el egy oszlopcentrikus indexszel érvényben van. A kötegelt, általában jelöl ~ 900 sorok, és használja a kód logika multicore Processzor, memória nagyobb átviteli optimalizálva, és közvetlenül lehetővé teszi a tömörített adatok az oszlop-tároló, amikor csak lehetséges. Ebben az SQL Server 2016 is feldolgozni ~ 900 sorok egyszerre, helyett 1 sor időpontjában, és következtében a művelet a teljes általános költség most közösen az egész köteget, csökkenti a költségeket, soronként teljes. A műveletek a oszlop tároló tömörítés alapvetően együtt megosztott mennyisége egy SELECT kötegelt módban műveletben érintett késését csökkentő. Az oszlop-áruházzal kapcsolatos további részletekért található, és kötegelt módban, [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Alább látható, mint betartásával a lekérdezés 2. ábra tervek egymás mellett, láthatja, hogy a feldolgozás módját kompatibilitási szintű megváltozott, és ennek következtében, ha a lekérdezések végrehajtása mindkét kompatibilitási szint regisztrálását, láthatja, hogy a legtöbb a feldolgozási időt töltött sor módú (86 %) képest kötegelt módtól (14 %), ahol 2 kötegek dolgozott. A dataset növeléséhez a juttatás növeli.

*2. ábra: A soros művelet módosítások BEJELÖLÉSÉVEL-es kompatibilitási szintű 130 kötegelt módban.*

![2. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>A rendezési végrehajtási kötegelt módban
Hasonló a fenti, de a rendezési művelet életbe lép az átmenet kötegelt módban (kompatibilitási szint 130) sor üzemmódról (120 kompatibilitási szint) javítja a okok miatt a rendezési művelet.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Látható-mellé a 3. ábra, láthatja, hogy a rendezési művelet sor módú jelöli 81-es %-át, amíg a kötegelt módban csak a 19 %-át (illetve 81-56 % meg magát a rendezés) jelöli.

*3. ábra: Rendezési művelet módosítja sor-es kompatibilitási szintű 130 kötegelt módban.*

![3. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Természetesen ezeket a mintákat csak tartalmazhat tízezreit sorok, vagyis semmi kezelőkonzoljából nézve a legtöbb SQL-kiszolgáló elérhető adatok ezek a napok. Csak ezek alapján több millió sort helyette projektre, és ezt tudja lefordítani a függőben lévő a számítási feladatok jellege naponta kímélni végrehajtás néhány percig.

## <a name="cardinality-estimation-ce-improvements"></a>Számossága becslése (CE) fejlesztései
Az SQL Server 2014-ban bevezetett, bármilyen adatbázis-kompatibilitási szinten 120 vagy újabb rendszeren futó igénybe veszik a új számossága a vizsgálat funkciót. Számossága becslése lényegében az határozza meg, hogyan hajtható végre SQL server egy lekérdezést a becsült költség alapuló logikai. A becslés lekérdezés érintett objektumok statisztika bemenetének használatával történik. Gyakorlatilag, egy magas szintű számossága becslés funkciók hozzávetőlegesek sor száma együtt a különböző érték számát, az értékek kapcsolatos információkat és ismétlődő száma szerepel a táblákat és a lekérdezésben hivatkozott objektumok. Ezek a becslések megfelelő első, vezethet szükségtelen lemez i/o miatt nincs elegendő memória biztosít (azaz a TempDB kijutás), vagy egy soros terv végrehajtási kijelölt keresztül egy párhuzamos terv végrehajtási néhányat említsünk. Megkötését, helytelen becslése egy átfogó teljesítménycsökkenés a lekérdezés-végrehajtás vezethet. A másik oldalon jobb becsléseket, pontosabb becslést vezet jobb lekérdezés végrehajtások!

Ahogy korábban említettük, lekérdezés optimalizálása és becslése egy összetett függetlenül attól, hogy, de ha azt szeretné, további információt a lekérdezésterveket és számosságbecslő, a dokumentum hivatkozhat [a lekérdezésterveket optimalizálja az SQL Server 2014 kulcsattribútumával Négyzetgyökének](https://msdn.microsoft.com/library/dn673537.aspx) mélyebb bemutatója a.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Mely számossága becslés jelenleg használ?
Határozza meg, hogy mely számossága becslése a lekérdezéseket futtat, a most használja az alábbi lekérdezés minták. Vegye figyelembe, hogy ez a példa első kompatibilitási szintjét 110-re, a régi számossága becslés funkciók használatát fognak futni.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Végrehajtási végrehajtása után XML hivatkozásra, és nézze meg az első iterátor tulajdonságainak alább látható módon. Vegye figyelembe a beállított 70 CardinalityEstimationModelVersion nevű tulajdonságnév. Ez nem jelenti azt, hogy az adatbázis kompatibilitási szintje van beállítva az SQL Server 7.0 verziójára (van állítva, akkor a 110-esre látható a fenti TSQL utasításokban, mint), de az érték 70 egyszerűen jelöli a örökölt számossága becslés funkció érhető el az SQL Server 7.0 óta Nincs fő verziók amely addig az SQL Server 2014 (amely 120 kompatibilitási szintje).

*4. ábra: A CardinalityEstimationModelVersion értéke 70 110-esre vagy alatti kompatibilitási szintje használatakor.*

![4. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Azt is megteheti, módosítsa a kompatibilitási szintjét 130, és az új számossága becslés függvény használatának letiltása a LEGACY_CARDINALITY_ESTIMATION beállítása ON a használatával [adatbázis HATÓKÖRŰ konfiguráció ALTER](https://msdn.microsoft.com/library/mt629158.aspx). Ez lesz pontosan megegyezik a 110-re a számosság becslés függvény szempontjából, a legújabb lekérdezés feldolgozása a kompatibilitási szint használata során. Ezáltal kihasználhassa az új lekérdezés feldolgozása a funkciók hamarosan legfrissebb kompatibilitási szintű (azaz kötegelt módban), de továbbra is a régi számossága becslés funkció támaszkodnak szükség esetén.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


A kompatibilitási szint 120 vagy 130 egyszerűen áthelyezése lehetővé teszi, hogy az új számossága becslés funkciókat. Ebben az esetben az alapértelmezett CardinalityEstimationModelVersion lesz beállítva, ennek megfelelően 120 vagy 130, az alább látható.

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*5. ábra: A CardinalityEstimationModelVersion értéke 130 130 kompatibilitási szintje használatakor.*

![5. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a>A számosság becslés különbségek aláírnia
Most tegyük futtassa kissé összetettebb érintő INNER JOIN néhány predikátumok a WHERE záradék a lekérdezés, és vizsgáljuk meg a sor számának becslése a régi számossága becslés függvény első.

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


A lekérdezés végrehajtása hatékonyan sorát adja vissza 200,704, amíg a régi számossága becslés funkciójú sor becsült jogcímek 194,284 sorok. Természetesen, az említett előtt, sor száma keresztillesztésével is függ, milyen gyakran futtatta a korábbi mintákat, amelyek tölti fel a minta táblák minden egyes futtatásához, és újra. Természetesen a lekérdezés predikátumok is vezérelt a tábla alakzat adattartalmat, a tényleges becslés hatással, és hogyan ezek az adatok tényleges összefüggéseket egymással.

*6. ábra: A sor számának becslése a rendszer nem 194,284 vagy 6000 sor a várt 200,704 sorokat.*

![6. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

Ugyanúgy most most végrehajtani ugyanabban a lekérdezésben új számossága becslés funkció.

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Megnézi a lent most látható, hogy a sorok becsült 202,877, vagy sokkal szorosabb és magasabb, mint a régi számossága becslés-e.

*7. ábra: A sor számának becslése már 202,877 194,284 helyett.*

![7. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

A valóságban eredménykészlet 200,704 sorok (de az összes függ, milyen gyakran az előző minták lekérdezések futott, de ennél is fontosabb, a TSQL a RAND() utasítást használja, mert a visszaadott tényleges értékek eltérőek lehetnek egy futni, hanem a következő). A konkrét példában az új számossága becslés nem, mert 202,877 sokkal közelebb 200,704, mint 194,284 sorok számának becslése jobb munkát! Az utolsó, ha módosítja a WHERE záradékban predikátumokat az egyenlő (helyett ">" példány), ez teheti a becslések között a régi és új számossága függvény még több különböző, attól függően, hogy hány megegyezik kérheti le.

Természetesen ebben az esetben ~ 6000 sort éppen ki a tényleges száma nem felel meg nagy mennyiségű adat bizonyos esetekben. Most, ez több millió sort több tábla és összetett lekérdezések, és esetenként a becslés lehet ki több millió sort, és ezért a megfelelő végrehajtási terv fel-vagy a TempDB vezető nincs elég memória biztosít a kért Transzponálás kijutás, és így további i/o sokkal nagyobb.

Ha a lehetőség, ez az összehasonlítás a leggyakoribb lekérdezések és adatkészletek gyakorlat, és tapasztalja meg, mennyi által a régi és új becslések némelyike érint, amíg csak néhány volt válnak a valóságban ki további vagy néhány másoknak közelebb egyszerűen csak az aktuális sor száma, az eredménykészlet ténylegesen vissza. Az összes az alakzat a lekérdezéseket, az Azure SQL adatbázis jellemzőit, típusát és az adatkészleteket és a rájuk vonatkozó statisztikát méretétől függ. Most létrehozott az Azure SQL Database-példányt, ha a lekérdezésoptimalizáló kell teljesen az előző lekérdezés futtatásakor készült statisztika újbóli felhasználása helyett a Tudásbázis létrehozásához. Igen a becslések nagyon környezetfüggő és majdnem adott minden kiszolgáló- és alkalmazástelepítési követelmények állnak. Fontos szem előtt tartani fontos eleme!

## <a name="some-considerations-to-take-into-account"></a>Néhány szempontot figyelembe kell venni
Bár a legtöbb alkalmazás és szolgáltatás előnyös a kompatibilitási szint 130, mielőtt az éles környezetben a kompatibilitási szintje bevezetése alapvetően 3 lehetőség közül választhat:

1. Kompatibilitási szint 130 áthelyezése, és tekintse meg, hogyan dolgot végre. Ha azt észleli, hogy egyes hátrányait, akkor egyszerűen csak a kompatibilitási szint állítsa vissza az eredeti szintre, vagy hagyja 130 és az örökölt üzemmód csak fordított számossága becslése biztonsági (fentiekben leírtak szerint ez nem sikerült a problémát).
2. Alaposan tesztelje a meglévő alkalmazások hasonló üzemi terhelés, finomhangolásra és a teljesítményt, mielőtt a termelési ellenőrzése. Esetlegesen felmerülő problémák ugyanaz, mint a fenti, is mindig lépjen vissza az eredeti kompatibilitási szintjét, vagy egyszerűen megfordíthatja a számosság becslése az örökölt módba.
3. Az utolsó lehetőség, megoldásának ezeket a kérdéseket a legutóbbi módja a Lekérdezéstár ki. Ez az ajánlott beállítás az aktuális! Segít az elemzést, a lekérdezések a kompatibilitási szint 120 vagy alatt 130, és nem javasoljuk, ahhoz, hogy a Lekérdezéstár használja. A Lekérdezéstár érhető el az Azure SQL Database 12-es verziójú legújabb verzióját, és úgy van kialakítva, a lekérdezési teljesítmény hibaelhárítás. A Lekérdezéstár gondol, a felhőszolgáltató közötti átviteléhez rögzítő összegyűjtése és minden lekérdezést előzménymodell részletesen bemutatja az adatbázis számára. Ez jelentősen egyszerűbb teljesítmény Törvényszéki diagnosztizálhatja és problémák megoldására idő csökkentésével. További információt a [Lekérdezéstár: az adatbázis egy felé továbbított adatok író](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

A magas szintű, ha már fut, vagy az alatti kompatibilitási szinttel rendelkező 120 adatbázisok készleteit, és tervezze át őket 130, vagy mert a számítási feladatok automatikusan létesítsen új adatbázisokat, hogy hamarosan beállítani 130 alapértelmezés szerint, fontolja meg a a következőket:

* Mielőtt éles környezetben az új kompatibilitási szint módosítása, engedélyezze a Lekérdezéstárat. Olvassa el a [módosítsa az adatbázis kompatibilitási módja, és használja a Lekérdezéstár](https://msdn.microsoft.com/library/bb895281.aspx) további információt.
* Ezt követően tesztelje jellemző adatok és a lekérdezések egy hasonló környezetet, és hasonlítsa össze a tapasztalt teljesítmény és a Lekérdezéstár által jelentett összes fontos munkaterhelés. Egyes hátrányait tapasztal, az a Lekérdezéstár regressed lekérdezések azonosításához, és a terv kényszerített lehetőséget a Lekérdezéstár használata (más néven csomag rögzítéshez). Ebben az esetben akkor véglegesen a kompatibilitási szint 130 marad, és használja a korábbi lekérdezésterv a Lekérdezéstár által javasolt.
* Ha szeretnék kihasználni az új funkciók és képességek az Azure SQL Database (amelyen az SQL Server 2016-os), de a kompatibilitási szint 130, utolsó lehetőségként által indított módosítások érzékeny érdemes lehet a kompatibilitási szint biztonsági szint kényszerítése az ALTER DATABASE utasítás segítségével, amely megfelel a számítási feladatok. De először, vegye figyelembe, hogy a Lekérdezéstár terv rögzítése beállítás-e a legjobb lehetőség, mivel nem használ 130 alapvetően marad az SQL Server régebbi verziójú működési szinten.
* Ha több adatbázis átfedés több-bérlős alkalmazásokhoz, frissítenie kell az adatbázis konzisztens kompatibilitási szintjének biztosítása közötti összes; létesítési logikai lehet régi és az újonnan telepített néhányat a meglévők közül. Lehet, hogy az alkalmazás munkateljesítményt néhány adatbázis különböző kompatibilitási szinten futnak tény-és nagybetűket, ezért bármely adatbázis kompatibilitási szintjét egységességének lehet szükség ahhoz, hogy ugyanazt a felhasználói élményt biztosít az ügyfelek számára a tábla összes között. Vegye figyelembe, hogy nincs megbízás, valóban attól függ, milyen hatással van az alkalmazás által a kompatibilitási szintet.
* Utolsó, számossága becslése kapcsolatban, és hasonlóan, éles, a folytatás előtt a kompatibilitási szint módosítása javasoljuk, hogy a termelési számítási feladatok határozza meg, ha az alkalmazás nyújtotta új feltételek tesztelése a Számossága becslés fejlesztései.

## <a name="conclusion"></a>Összegzés
Azure SQL Database használata az összes SQL Server 2016 fejlesztések kihasználják a lekérdezés végrehajtások egyértelműen javítja. Ahogy-van! Természetesen mint bármely új szolgáltatás a megfelelő próbaverzióra kell elvégezni a pontos feltételek, amelyek alapján az adatbázis munkaterhelés működik, a legjobb meghatározására. A tapasztalat, hogy a legtöbb munkaterhelés várhatóan legalább futni transzparens módon kompatibilitási szint 130, miközben feldolgozási funkciók és új számossága becslés új lekérdezés használhatja. Amely említett reálisan nézve mindig van néhány kivétel, valamint arról, hogy megfelelő megfelelő gondossággal annak meghatározásához, hogy milyen mértékben kihasználhassa a bővítések fontos értékelését. És ebben az esetben a Lekérdezéstár a munka során a nagy Súgó!

Az SQL Azure fejlődésének a jövőben számíthat 140 kompatibilitási szintet. Megfelelő idő esetén először fog van szó Mi a későbbi kompatibilitási szint 140 be fogja hozni, ugyanúgy, mint a röviden ismertettük itt milyen kompatibilitási szint 130 ma mihamarabb elérhetővé tenni.

Most tegyük feledkezzen, 2016. június-től kezdődően az Azure SQL Database módosul az alapértelmezett kompatibilitási szintje 120 130 az újonnan létrehozott adatbázisok. Ne feledje!

## <a name="references"></a>Referencia
* [Adatbázis-kezelő újdonságai](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blog: A Lekérdezéstár: egy felé továbbított adatok író Borko Novakovic, 8 2016. június az adatbázis számára](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [ALTER DATABASE HATÓKÖRŰ KONFIGURÁCIÓ](https://msdn.microsoft.com/library/mt629158.aspx)
* [Az Azure SQL Database 12-es kompatibilitási szintje 130](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [A lekérdezés optimalizálása-csomagok és az SQL Server 2014 Számosságbecslő](https://msdn.microsoft.com/library/dn673537.aspx)
* [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx)
* [Blog: Jobb teljesítmény-küszöbérték-es kompatibilitási szintű 130 az Azure SQL-adatbázis által Alain Lissoir 6 2016. május](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
