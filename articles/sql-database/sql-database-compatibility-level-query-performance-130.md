---
title: "aaaDatabase kompatibilitási szint 130 - Azure SQL Database |} Microsoft Docs"
description: "Ez a cikk azt megismerkedhet az Azure SQL Database 130 kompatibilitási szinten fut, és új lekérdezésoptimalizáló hello hello előnyeit kihasználva hello előnyei és lekérdezni a processzor funkcióit. Azt is eleget hello lehetséges mellékhatásokkal a lekérdezési teljesítmény hello hello meglévő SQL-alkalmazásokhoz."
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
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Javult a lekérdezési teljesítmény kompatibilitási szint 130 az Azure SQL-adatbázis
Az Azure SQL Database fut transzparens módon akár több ezer adatbázis több száz számos különböző kompatibilitási szinten megőrzi az és hello előző verziókkal való kompatibilitás toohello megfelelő verzióját a Microsoft SQL Server biztosítva az ügyfelek a!

Ez a cikk azt megismerkedhet az Azure SQL adatbázismodell 130 kompatibilitási szinten fut, és új lekérdezésoptimalizáló hello hello előnyeit kihasználva hello előnyei és lekérdezni a processzor funkcióit. Azt is eleget hello lehetséges mellékhatásokkal a lekérdezési teljesítmény hello hello meglévő SQL-alkalmazásokhoz.

Ne feledje az előzményeket SQL verziók toodefault kompatibilitási szintnek hello igazítását a következők:

* 100: az SQL Server 2008 és az Azure SQL adatbázis-V11.
* 110: az SQL Server 2012 és az Azure SQL adatbázis-V11.
* 120: az SQL Server 2014 és az Azure SQL-adatbázis 12-es verzió.
* 130: az SQL Server 2016-os és az Azure SQL-adatbázis 12-es verzió.

> [!IMPORTANT]
> Kezdve **2016. június mid**, az Azure SQL Database, a hello alapértelmezett kompatibilitási szintje lesz helyett 120 130 **az újonnan létrehozott** adatbázisok.
> 
> 2016. június mid előtt létrehozott adatbázisokat fogja *nem* érinti, és a jelenlegi kompatibilitási szint (100, 110-esre vagy 120) rendelkeznek. Azure SQL Database V11 tooV12 verziójából áttelepített adatbázisok fog rendelkezni 100 vagy a 110-es kompatibilitási szintet. 
> 

## <a name="about-compatibility-level-130"></a>130 kompatibilitási szintre vonatkozó
Először Ha azt szeretné, hogy tooknow hello jelenlegi kompatibilitási szintjét az adatbázis, a következő Transact-SQL-utasítás hello hajtható végre.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Ezen változtatás előtt toolevel 130 történik, a **újonnan** hozott létre adatbázisok, most áttekintheti az ezt a módosítást minden szól bizonyos nagyon alapszintű lekérdezés példákból, és tekintse meg, hogyan bárki is kihasználhatja le azt.

A lekérdezés feldolgozása a relációs adatbázisok nagyon összetett feladat lehet, és toolots számítógép tudományos és matematikai toounderstand hello rejlő tervezési döntések ütköznek azokkal és viselkedéshez vezethet. Ebben a dokumentumban hello tartalom lett szándékosan egyszerűsített tooensure, hogy bárki, aki néhány minimális technikai háttér hello kompatibilitási szint módosítása hello hatásának megismerése, és határozza meg az alkalmazások előnyeiről.

Most rendelkeznie milyen hello kompatibilitási szint 130 számos lehetőséget kínál, hello táblát gyorsan át.  További részletei: [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), de rövid összegzése:

* hello Insert választás utasítás beszúrási művelet lehet többszálas vagy, hogy a párhuzamos terv, mielőtt ez a művelet lett egyszálas során.
* Memória optimalizált tábla és a tábla változók lekérdezések most lehet párhuzamos terveket, mielőtt ez a művelet lett is egyszálas során.
* A Memóriaoptimalizált tábla statisztikája most mintát vehetnek és automatikus frissítése. Lásd: [az adatbázis-kezelő újdonságai: memórián belüli online Tranzakciófeldolgozási](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) további részleteket.
* Az oszlop az Oszlopcentrikus indexek módosítja sor módú kötegelt módban v/s
  * Egy oszlop Oszlopcentrikus indexszel rendelkező táblán rendezi most kötegelt módban van.
  * Leképezési összesítések kötegelt módban, például a TSQL LAG/ÁTFUTÁSI utasítások működni.
  * A distinct záradékban több oszlop tárolási táblákon lekérdezések kötegelt módban működik.
  * DOP mellett futó lekérdezésekhez = 1 vagy soros csomagot is hajtsa végre a kötegelt módban.
* Utolsó, számossága becslés fejlesztései ténylegesen érkeznek-es kompatibilitási szintű 120, de azok az Ön alacsonyabb kompatibilitási szintű fut (azaz 100 vagy 110), hello áthelyezés toocompatibility szint 130 fog is visszaállítja ezek a fejlesztések, és ezeket is az alkalmazások teljesítményének hello lekérdezés előnyeit.

## <a name="practicing-compatibility-level-130"></a>Kompatibilitási szint 130 gyakorlás
Első folytassuk egyes táblák, indexek és létrehozott véletlenszerű adatokat toopractice néhány az új lehetőségekhez. SQL Server 2016-os, illetve az Azure SQL Database hello TSQL parancsfájl példák hajtható végre. Azonban ha egy Azure SQL-adatbázis létrehozása győződjön meg arról, hogy adjon meg egy P2 adatbázist, mert a szükséges minimális hello többszálú magok tooallow legalább néhány, és ezért kihasználhatja le ezeket a szolgáltatásokat.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

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


Most tegyük rendelkezik egy hely toosome hello lekérdezés feldolgozása funkciók hamarosan-es kompatibilitási szintű 130.

## <a name="parallel-insert"></a>Párhuzamos BESZÚRÁSA
Hello kompatibilitási szintje 120 és 130, amely egyetlen összefűzött modellben (120), és a többszálas modellben (130) rendre végrehajtja a beszúrási művelet hello alapján végrehajtott beszúrási művelet végrehajtása hello TSQL utasításokat az alábbi hajtja végre.

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Hello tényleges hello lekérdezésterv, a grafikus ábrázolása és az XML-tartalom kérésével mely számossága becslés függvény jelenleg play azt is meghatározhatja. Az 1. ábra megnézi hello csomagok-mellé, egyértelműen láthatja, hogy hello oszlop tároló BESZÚRÁSA végrehajtási kerül a soros a 120 tooparallel 130. Megjegyzés: Ez hello a módosítás hello iterátor ikon hello 130 terv, amely mutatja, két párhuzamos nyilat ábrázoló hello tényt, hogy most hello iterátor végrehajtási valóban párhuzamos. Ha nagy beszúrási műveletek toocomplete, hello párhuzamos futtatáshoz, Ön rendelkezésére a hello adatbázis core csatolt toohello száma javítja a teljesítményt; 100 alkalommal gyorsabb a helyzettől függően tooa mentése!

*1. ábra: BESZÚRÁSA soros tooparallel-es kompatibilitási szintű 130 művelet módosításokat.*

![1. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SOROS kötegelt módban
Hasonlóképpen a toocompatibility szint 130 áthelyezése sornyi adatot feldolgozásakor engedélyezi kötegfeldolgozási mód. Először kötegelt módban futó műveletekhez esetén csak érhetők el egy oszlopcentrikus indexszel érvényben van. Ezután egy kötegelt általában ~ 900 sorok jelöli, és használja a kód logika multicore Processzor, memória nagyobb átviteli optimalizálva és közvetlenül használja hello tömörített adatai hello oszlop tároló, amikor csak lehetséges. Ebben az SQL Server 2016 tud feldolgozni ~ 900 sorok egyszerre, 1 sor hello időpontban helyett, valamint következtében hello hello művelet teljes általános költség most megosztott hello teljes kötegelt hello általános soronként költségeket csökkenti. Alapvetően együtt hello oszlop tároló tömörítési műveletek megosztott ennyi csökkenti a hello késést részt vesz egy SELECT kötegelt módban működik. Hello oszlop-áruházzal kapcsolatos további részletekért található, és kötegelt módban, [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Alább látható, mint betartásával hello lekérdezés tervek-mellé a 2. ábra azt mutatják, hogy hello feldolgozási mód hello kompatibilitási szinttel rendelkező megváltozott, és következtében végrehajtásakor hello lekérdezések mindkét kompatibilitási szint regisztrálását, láthatja, hogy a legtöbb hello feldolgozási időt töltött sor mód (86 %) képest toohello kötegelt módban (14 %), ahol 2 kötegek feldolgozott. Hello dataset növeléséhez hello juttatás növeli.

*2. ábra: Művelet módosítások-es kompatibilitási szintű 130 soros toobatch mód kiválasztása*

![2. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>A rendezési végrehajtási kötegelt módban
A fenti hasonló toohello, de a rendezési művelet alkalmazott tooa, hello való áttérés sor mód (kompatibilitási szint 120) toobatch mód (kompatibilitási szint 130) javítja hello hello a rendezési művelet hello teljesítményét okok miatt.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Látható-mellé a 3. ábra, láthatja, hogy a hello rendezési művelet sor módú 81-es %-a költség, amíg hello kötegelt módban csak jelöli 19 % hello költség (illetve 81-56 % hello rendezési magát a) hello jelöli.

*3. ábra: A sor toobatch módú-es kompatibilitási szintű 130 változik rendezési művelet.*

![3. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Természetesen ezeket a mintákat csak tartalmazhat tízezreit sorok, vagyis semmi kezelőkonzoljából nézve legtöbb SQL-kiszolgáló elérhető hello adatok ezek a napok. Csak ezek alapján több millió sort helyette projektre, és ez kímélni minden nap függőben lévő hello jellegét a számítási feladatok végrehajtása több percig is fordítása.

## <a name="cardinality-estimation-ce-improvements"></a>Számossága becslése (CE) fejlesztései
Az SQL Server 2014-ban bevezetett, bármilyen adatbázis-kompatibilitási szinten 120 vagy újabb rendszeren futó megkönnyítő hello új számossága becslés funkciók használatára. Alapvetően számossága becslés hello logika toodetermine SQL server hogyan hajtható végre a lekérdezés a becsült költség alapul. hello becslés lekérdezés érintett objektumok statisztika bemenetének használatával történik. Gyakorlatilag, egy magas szintű számossága becslés funkciók hozzávetőlegesek sor száma információ hello terjesztési hello értékek eltérő érték számát, valamint és ismétlődő számok lévő táblák és hello lekérdezésben hivatkozott objektumok hello. Első hibás, ezek a becslések vezethet toounnecessary lemez i/o tooinsufficient memória biztosít (azaz a TempDB kijutás) miatt, vagy tooa kiválasztását párhuzamos keresztül egy soros terv végrehajtási terv végrehajtási, tooname néhány. Megkötését, helytelen becslése vezethet tooan hello lekérdezés-végrehajtás általános teljesítménycsökkenést. Hello a másik oldalon pontosabb becslést, pontosabb becslést, érdeklődők toobetter lekérdezés végrehajtások!

Ahogy korábban említettük, lekérdezés optimalizálása és becslése egy összetett függetlenül attól, hogy, de ha azt szeretné, hogy további terveket és számosságbecslő toolearn, olvassa el a dokumentumot toohello [optimalizálása a lekérdezés terveket hello SQL Server 2014 Számosságbecslő](https://msdn.microsoft.com/library/dn673537.aspx) mélyebb bemutatója a.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Mely számossága becslés jelenleg használ?
mely számossága becslése a lekérdezéseket futtat, a most csak használata hello lekérdezés minták alábbi toodetermine. Vegye figyelembe, hogy az első példában kompatibilitási szintjét 110-esre, hello hello régi számossága becslés funkciók használatát úgy fognak futni.

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


Ha végrehajtása befejeződött, kattintson a hello XML-hivatkozás, és hello első iterátor hello tulajdonságainak tekintse meg alább látható módon. Megjegyzés: a beállított 70 CardinalityEstimationModelVersion nevű hello tulajdonság neve. Azt nem jelenti azt, hogy hello adatbázis kompatibilitási szintje toohello SQL Server 7.0 verzió (van állítva, akkor a 110-re, mint látható a fenti hello TSQL-utasításokban), de hello érték 70 egyszerűen annak hello örökölt számossága becslés funkció érhető el SQL óta Server 7.0, amelyet nem fő verziók addig az SQL Server 2014 (amely 120 kompatibilitási szintje).

*4. ábra: hello CardinalityEstimationModelVersion értéke too70 110-esre vagy alatti kompatibilitási szintje használatakor.*

![4. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Azt is megteheti, hello kompatibilitási szint too130 módosítsa, majd tiltsa le a hello új számossága becslés függvény hello használata LEGACY_CARDINALITY_ESTIMATION beállítása a tooON hello segítségével [ALTER DATABASE HATÓKÖRŰ CONFIGURATION utasítás](https://msdn.microsoft.com/library/mt629158.aspx). Ez fogja kell pontosan hello ugyanaz, mint a Cardinality becslés függvény szempontjából, a 110 használatával hello legújabb lekérdezés feldolgozása a kompatibilitási szint használata során. Így igénybe vehesse az hello új lekérdezés feldolgozása a funkciók hamarosan hello legfrissebb kompatibilitási szintű (azaz kötegelt módban), de továbbra is támaszkodjon hello régi számossága becslés funkció szükség esetén.

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


Egyszerűen áthelyezése toohello kompatibilitási szint 120 vagy 130 lehetővé teszi, hogy a hello új számossága becslés funkciót. Ebben az esetben hello alapértelmezett CardinalityEstimationModelVersion lesz beállítva, ennek megfelelően too120 vagy 130 látható, az alábbi.

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


*5. ábra: hello CardinalityEstimationModelVersion értéke too130 130 kompatibilitási szintje használatakor.*

![5. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a>Aláírnia hello számossága becslés különbségek
Most tegyük futtassa kissé összetettebb érintő INNER JOIN néhány predikátumok a WHERE záradék a lekérdezés, és nézzük hello sor számának becslése hello régi számossága becslés függvény első.

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


Hello sor becsült funkciójú hello régi számossága becslés jogcímek 194,284 sorok, 200,704 sorok, a lekérdezés végrehajtása hatékonyan adja vissza. Természetesen, az említett előtt, sor száma keresztillesztésével is függ, milyen gyakran futtatta hello előző mintát, amely feltölti hello minta táblák minden egyes futtatásához, és újra. A lekérdezés hello predikátumok természetesen is hello tényleges becslés vezérelt hello tábla alakzat, az adatok tartalmának és hogyan ezek az adatok tényleges összefüggéseket egymással hatással.

*6. ábra: hello sor számának becslése a rendszer nem 194,284 vagy 6000 sor a hello 200,704 sorok várt.*

![6. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

A hello ugyanúgy, most végrehajtani a hello azonos funkciójú hello új számossága becslés lekérdezése.

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


Az alábbi hello megnézi, azt most meg hello sor becslést 202,877, vagy sokkal szorosabb és magasabb, mint a régi számossága becslés hello.

*7. ábra: hello sor számának becslése már 202,877 194,284 helyett.*

![7. ábra](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

A valóságban hello eredménykészlet 200,704 sorok (de hello hello lekérdezések futott milyen gyakran az előző minták függ az összes, de ennél is fontosabb, hello TSQL hello RAND() utasítást használja, mert hello visszaadott tényleges értékek eltérhetnek egy futtatási toohello mellett). A konkrét példában hello új számossága becslés nem: hello sorok számának becslése, mert 202,877 sokkal szorosabb too200, 704, mint 194,284 jobb munkát! Az utolsó, ha módosítja a WHERE záradék predikátumokat tooequality hello (helyett ">" példány), ez akár több különböző sikerült hello becslése hello régi és új számossága függvény között, attól függően, hogy hány megegyezik kaphat.

Természetesen ebben az esetben ~ 6000 sort éppen ki a tényleges száma nem felel meg nagy mennyiségű adat bizonyos esetekben. Most, tenni a sorok toomillions több táblák és összetett lekérdezések között, és esetenként hello becsült sorok milliói ki kell, és ezért a fel-hello helytelen végrehajtási terv, vagy nincs elég memória a kért biztosít bevezető kockázatát hello tooTempDB kijutás, és így további i/o sokkal nagyobb.

Ha hello lehetőség van, ez az összehasonlítás a leggyakoribb lekérdezések és adatkészletek konfigurációkat, és ismerje által mennyi néhány hello régi és új becslése érint, míg néhány csak válhat a hello valóságban ki további vagy néhány mások csak egyszerűen szorosabb toohello tényleges sorban álló hello eredményhalmazt ténylegesen vissza. Az összes hello alakzat lekérdezések, hello Azure SQL adatbázis jellemzőit, hello természetét és a adatkészleteket, és rendelkezésére hello statisztika hello méretétől függ. Ha az imént létrehozott az Azure SQL Database-példányt, hello lekérdezés optimalizáló toobuild kell a Tudásbázis helyett hello előző lekérdezés tett újbóli felhasználás statisztika teljesen új fut. Igen hello becslése nagyon környezetfüggő és majdnem adott tooevery kiszolgáló és az alkalmazás. Fontos szem előtt egy fontos tookeep!

## <a name="some-considerations-tootake-into-account"></a>Bizonyos szempontok tootake figyelembe
Bár a legtöbb alkalmazás és szolgáltatás hello kompatibilitási szint 130, mielőtt az éles környezetben hello kompatibilitási szintje bevezetése előnyös, alapvetően 3 lehetőség közül választhat:

1. Helyezze át a toocompatibility szint 130, és tekintse meg, hogyan dolgot végre. Abban az esetben, ha egyes hátrányait figyelje meg, akkor egyszerűen csak hello kompatibilitási szint hátsó tooits eredeti szintje, vagy tartsa 130, és csak fordított hello számossága becslés hátsó toohello örökölt üzemmódú (fentiekben Elmagyarázott, ez nem sikerült hiba megoldása érdekében hello).
2. Alaposan tesztelje a meglévő alkalmazások hasonló üzemi terhelés, finomhangolásra és hello teljesítményének folyamatos tooproduction előtt ellenőrzi. Esetlegesen felmerülő problémák ugyanaz, mint a fenti, is mindig vissza toohello eredeti kompatibilitási szintet, vagy egyszerűen megfordíthatja hello számossága becslés hátsó toohello örökölt üzemmódú.
3. A végső lehetőséget, és hello legutóbbi módon tooaddress tooleverage hello Lekérdezéstár ezek kérdésekre, lesz. Ez az ajánlott beállítás az aktuális! tooassist hello elemzése a lekérdezéseket a kompatibilitási szint 120 vagy alatt 130, és nem javasoljuk, elég toouse Lekérdezéstár. A Lekérdezéstár hello legújabb Azure SQL Database 12-es verziójában elérhető, és úgy van kialakítva, toohelp, a lekérdezési teljesítmény hibaelhárításban. A Lekérdezéstár hello gondol a felhőszolgáltató közötti átviteléhez rögzítő az adatbázis összegyűjtése és minden lekérdezést előzménymodell részletesen bemutatja. Ez jelentősen leegyszerűsíti a teljesítmény Törvényszéki hello idő toodiagnose csökkentésével, és problémák megoldására. További információt a [Lekérdezéstár: az adatbázis egy felé továbbított adatok író](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

A magas szintű, ha már fut, vagy az alatti kompatibilitási szinttel rendelkező 120 adatbázisok készleteit, és tervezze meg toomove hello némelyikük too130, vagy mert a számítási feladatok automatikusan létesítsen új adatbázisokat, hogy rövidesen lehetőség alapértelmezett too130 által beállított, fontolja meg az hello a következőket:

* Toohello új kompatibilitási szintjének éles környezetben módosítása előtt engedélyezze a Lekérdezéstárat. Olvassa el a túl[hello adatbázis kompatibilitási módja és -felhasználási hello Lekérdezéstár módosítása](https://msdn.microsoft.com/library/bb895281.aspx) további információt.
* Ezt követően tesztelje összes fontos munkaterhelés jellemző adatok és a lekérdezések egy hasonló környezetet, és hasonlítsa össze hello teljesítmény észlelt, és a Lekérdezéstár jelentett adatokat. Egyes hátrányait tapasztal, azonosíthatja a Lekérdezéstár hello közleményében szerepelt lekérdezések hello és kényszerített lehetőséget a Lekérdezéstár hello csomag használata (más néven csomag rögzítéshez). Ebben az esetben akkor véglegesen hello kompatibilitási szintű 130 maradnak, és használja hello korábbi lekérdezésterv hello a Lekérdezéstár által javasolt.
* Ha szeretné, hogy az Azure SQL Database (amelyen az SQL Server 2016) tooleverage új funkcióira és képességeire, de bizalmas toochanges alá hello kompatibilitási szint 130, utolsó lehetőségként, érdemes lehet hello kompatibilitási szint vissza kényszerítése toohello szintje, amely megfelel a számítási feladatok ALTER DATABASE utasítás segítségével. De először, vegye figyelembe beállítás rögzítési hello Lekérdezéstár terv a legjobb lehetőség, mert nem használ 130 alapvetően marad az SQL Server régebbi verziójú hello működési szinten.
* Ha több adatbázis átfedés több-bérlős alkalmazásokhoz, lehet szükséges tooupdate hello az adatbázisok tooensure egy egységes kompatibilitási szint elvét kiépítés közötti összes; régi és az újonnan telepített néhányat a meglévők közül. Lehet, hogy a munkaterhelés alkalmazásteljesítmény bizalmas toohello tényt, hogy az egyes adatbázisok különböző kompatibilitási szinten futnak, ezért bármely adatbázis kompatibilitási szintjét egységességének szükség lehet ahhoz tooprovide hello azonos felhasználói élmény tooyour ügyfelek összes hello tábla között. Vegye figyelembe, hogy nincs megbízás, valóban attól függ, milyen hatással van az alkalmazás által hello kompatibilitási szintet.
* Utolsó, hello számossága becslés kapcsolatban, és hasonlóan hello kompatibilitási szint, éles, a folytatás előtt célszerű ajánlott a hello új feltételek toodetermine alatt a termelési számítási feladatok tootest Ha az alkalmazás nyújtotta hello számossága becslés fejlesztései.

## <a name="conclusion"></a>Összegzés
Azure SQL Database használata az összes SQL Server 2016 fejlesztések toobenefit egyértelműen javíthatja a lekérdezés végrehajtások. Ahogy-van! Természetesen mint bármely új szolgáltatás a megfelelő próbaverzióra elvégzendő toodetermine hello pontos feltételek alapján, amely az adatbázis munkaterhelés működik hello legjobban. Felhasználói élmény azt mutatja, hogy a legtöbb munkaterhelés várt tooat legalább futni transzparens módon kompatibilitási szint 130, miközben feldolgozási funkciók és új számossága becslés új lekérdezés használhatja. Amely említett reálisan nézve mindig van néhány kivétel, valamint arról, hogy megfelelő megfelelő gondossággal egy fontos assessment toodetermine milyen mértékben kihasználhassa a bővítések. És ebben az esetben hasznos lehet a hello Lekérdezéstár kiváló segít a munka során!

Az SQL Azure fejlődésének számíthat jövőbeli hello a kompatibilitási szintet 140. Megfelelő idő esetén először fog van szó Mi a későbbi kompatibilitási szint 140 be fogja hozni, ugyanúgy, mint a röviden ismertettük itt milyen kompatibilitási szint 130 ma mihamarabb elérhetővé tenni.

Most tegyük feledkezzen, 2016. június-től kezdődően az Azure SQL Database állapotúról hello alapértelmezett kompatibilitási szintje 120 too130 az újonnan létrehozott adatbázisok. Ne feledje!

## <a name="references"></a>Referencia
* [Adatbázis-kezelő újdonságai](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blog: A Lekérdezéstár: egy felé továbbított adatok író Borko Novakovic, 8 2016. június az adatbázis számára](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [ALTER adatbázis kompatibilitási szintje (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [ALTER DATABASE HATÓKÖRŰ KONFIGURÁCIÓ](https://msdn.microsoft.com/library/mt629158.aspx)
* [Az Azure SQL Database 12-es kompatibilitási szintje 130](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [A lekérdezés tervek optimalizálja az SQL Server 2014 Számosságbecslő hello](https://msdn.microsoft.com/library/dn673537.aspx)
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
