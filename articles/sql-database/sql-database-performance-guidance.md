---
title: "aaaAzure SQL-adatbázis teljesítményének hangolása útmutató |} Microsoft Docs"
description: "Ez a cikk segítségével meghatározhatja, hogy melyik szolgáltatás réteg toochoose az alkalmazáshoz. Azt is javasolja módon tootune az alkalmazás tooget hello a legtöbbet tudja kihozni az Azure SQL Database."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: dd8d95fa-24b2-4233-b3f1-8e8952a7a22b
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/09/2017
ms.author: carlrab
ms.openlocfilehash: 2699f755391e94ab488ac1e6acedd30f8aec4488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-performance-in-azure-sql-database"></a>Az Azure SQL-adatbázis teljesítményének hangolása

Az Azure SQL Database is tartalmaz [javaslatok](sql-database-advisor.md) , hogy az adatbázis teljesítményét tooimprove is használhatja, vagy hagyhatja, hogy az Azure SQL Database [tooyour alkalmazás automatikusan igazítja](sql-database-automatic-tuning.md) és a módosítások alkalmazásához a számítási feladatok teljesítményére, amely javítja.

Az Ön nem rendelkezik megfelelő javaslatokkal és teljesítménnyel kapcsolatos problémák továbbra is fennáll, használhatja a következő módszerek tooimprove teljesítményének hello:
1. Növelje [szolgáltatásszintek](sql-database-service-tiers.md) , és adja meg a további erőforrások tooyour adatbázis.
2. Az alkalmazás hangolás, és alkalmazni, néhány ajánlott eljárás, amely növelheti a teljesítményt. 
3. Hello adatbázis hangolására indexek és a lekérdezések toomore hatékonyan dolgozni adatokkal történő módosításával.

Ezek a kézi módszert, mert szüksége toodecide mi [szolgáltatásszintek](sql-database-service-tiers.md) kellene kiválasztania vagy toorewrite alkalmazás vagy az adatbázis-kód szükséges, és deply hello módosításokat.

## <a name="increasing-performance-tier-of-your-database"></a>Az adatbázis teljesítményének szintjének növelése

Az Azure SQL Database kínál négy [szolgáltatásszintek](sql-database-service-tiers.md) , amelyek közül választhat: Basic, Standard, Premium és prémium RS (teljesítményének mérése történik az adatbázis-átviteli egységek, vagy [dtu-k](sql-database-what-is-a-dtu.md). Egyes szolgáltatásszinteken szigorúan elkülöníti hello erőforrásokat, hogy használható-e az SQL-adatbázis, és biztosítja, hogy a szolgáltatási szint kiszámítható teljesítményt. Ez a cikk útmutatást, amelyek segítségével válassza ki az alkalmazás hello szolgáltatási rétegben fel. Azt is ismertetik, hogy az alkalmazás tooget hello a legtöbbet tudja kihozni az Azure SQL Database észlelheti.

> [!NOTE]
> Ez a cikk foglalkozik, a teljesítmény útmutatást az önálló adatbázisok Azure SQL-adatbázisban. Teljesítmény útmutató kapcsolódó tooelastic-készletek: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool-guidance.md). Vegye figyelembe azonban, hogy ez a cikk toodatabases rugalmas készlethez javaslatait hangolása hello számos, és alkalmazhatunk hasonló teljesítmény előnyök.
> 

* **Alapszintű**: hello alapszintű service réteg ajánlatokat a megfelelő teljesítmény kiszámíthatóságot az egyes adatbázisok, akár óráig óra. Egy alapszintű adatbázis elegendő erőforrással támogatják a több egyidejű kérelmet nem rendelkező kisméretű adatbázisban a megfelelő teljesítmény. Ha alapszintű szolgáltatásréteg szeretné használni a tipikus használati esetek a következők:
  * **Ön éppen használatának megkezdéséhez az Azure SQL Database**. Alkalmazások, amelyek a fejlesztési gyakran nagy teljesítményű szintek nem szükséges. Alapszintű adatbázisok adatbázis fejlesztési vagy tesztelési, egy alacsony ár ponton ideális környezetben futnak.
  * **Egyetlen felhasználó van egy adatbázis**. Alkalmazásokat, amelyek általában egy-egy felhasználóhoz társítása egy adatbázis nem rendelkezik magas egyidejűségi és teljesítménybeli követelményeit. Ezeket az alkalmazásokat a hello alapvető szolgáltatásszint-zel.
* **Standard**: hello szabványos szolgáltatásréteg kínál javítja a teljesítményt, kiszámíthatóságot és adatbázisok esetén, amelyek több egyidejű kérés, például a munkacsoport és webalkalmazások kiváló teljesítményt nyújt. Ha úgy dönt, a Standard service réteg adatbázis, az adatbázis-alkalmazás, kiszámítható teljesítményt alapján méretét percben percben.
  * **Az adatbázis még több egyidejű kérelmet**. Alkalmazások, amelyeket a szolgáltatás több felhasználó általában egyszerre kell magasabb teljesítményszintet. Például munkacsoportokhoz vagy webalkalmazásokhoz az alkalmazásokat, amelyek alacsony toomedium IO forgalom követelményeinek támogatása több egyidejű lekérdezések esetén hello szabványos szolgáltatási réteg esetén használható jól az.
* **Prémium szintű**: hello prémium szolgáltatásszintet kiszámítható teljesítményt biztosít, az egyes Premium adatbázisok második keresztül. Ha úgy dönt, hogy hello prémium szolgáltatásszintet, az adatbázis-alkalmazás az adatbázishoz tartozó hello csúcsterhelés alapján méretét. hello terv teljesítmény variancia azt okozhatja kis lekérdezések tootake hosszabb, mint a késésre érzékeny műveletek várt esetekben eltávolítja. Ez a modell jelentősen egyszerűsítheti hello fejlesztési és a termék érvényesítési ciklusok toomake erős utasítások csúcs erőforrásigényeivel, a teljesítmény variancia vagy a lekérdezés-késleltetés igénylő alkalmazásokhoz. Prémium szintű szolgáltatási réteg használata legtöbbször egy vagy több, a következő jellemzőkkel rendelkezik:
  * **Magas csúcsterhelés**. Az alkalmazás jelentős Processzor, a memória vagy a bemeneti/kimeneti (I/O) toocomplete egy dedikált, nagy teljesítményű szintet igényel, a műveleteket. Például egy adatbázis-művelet tooconsume ismert több processzormag hosszú ideig hello prémium szolgáltatásszintet jelöltségét ellenőrző.
  * **Sok egyidejű kérés**. Egyes adatbázis-alkalmazások például szolgáltatás sok egyidejű kérés, amikor a szolgáltató egy webhely, amely rendelkezik egy nagy forgalma. Basic és Standard szolgáltatásszintek egyidejű kérelmek / adatbázis hello számának korlátozása. Több kapcsolatot igénylő alkalmazások toochoose egy megfelelő foglalási méret toohandle hello kérelmek maximális számát szükséges lenne szükség.
  * **Kis késés**. Egyes alkalmazások tooguarantee hello adatbázis válaszára minimális idő van szükség. Egy adott tárolt eljárás hívása esetén, egy szélesebb körű felhasználói művelet részeként, lehetséges, hogy egy követelmény toohave egy adott hívás visszatérési legfeljebb 20 ezredmásodpercben, 99 százalékához kiindulási hello idő. Ilyen típusú alkalmazás számos előnyt biztosít az hello prémium szolgáltatásszintet érhető el róla, hogy hello szükséges számítástechnikai power toomake.
* **Prémium szintű RS**: prémium RS réteg hello célja az IO-igényes munkaterhelések nem igénylő hello legmagasabb rendelkezésre állását garantálja. Például nagy teljesítményű munkaterhelések, vagy egy analitikai munkaterhelés, ahol hello adatbázis nincs rekord hello rendszer tesztelése.

hello szolgáltatási szint, amelyekre szüksége van az SQL-adatbázis hello csúcs Adatbetöltési követelményeinek minden erőforrás dimenzió függ. Egyes alkalmazások egyetlen trivial méretű használata, de más erőforrások jelentős követelmények vonatkoznak.

### <a name="service-tier-capabilities-and-limits"></a>Szolgáltatás szolgáltatásszintek lehetőségei és korlátai

: Az egyes szolgáltatásszinteken be hello teljesítményszint szükséges, így csak a szükséges kapacitást hello hello rugalmasságot toopay kell. Is [a kapacitás](sql-database-service-tiers.md), felfelé vagy lefelé, mint a munkaterhelési változások. Például ha hello vissza-iskolai bevásárlási időszak alatt az adatbázisban munkaterhelés magas, növelheti hello teljesítményszintet hello adatbázis beállított ideje, július szeptember keresztül. A maximális időszak végén csökkentheti. Minimalizálhatja a felhő környezet toohello szezonalitás értékének üzleti optimalizálásával fizet. Ez a modell szoftver termék kiadási ciklusok esetén is működik. A teszt csoport kapacitás foglal le, amíg a teszt futtatása, és a kapacitás majd engedje a tesztelés befejezése. A kapacitás kérelem modellben kell fizetnie kapacitás szükség lenne rá, és előfordulhat, hogy ritkán használt dedikált erőforrások kell.

### <a name="why-service-tiers"></a>Miért szolgáltatásszintek?
Bár egyes adatbázis munkaterhelések eltérőek lehetnek, hello szolgáltatásszintek célja tooprovide teljesítmény kiszámíthatóságot teljesítmény különböző szinteken. Nagy méretű adatbázist erőforrás-követelményekkel rendelkező ügyfelek a több dedikált számítógépes környezetekben is működik.

## <a name="tune-your-application"></a>Az alkalmazás hangolása
A hagyományos helyszínen SQL Server a kezdeti kapacitástervezés hello folyamat gyakran elkülönül egy alkalmazást éles környezetben futó hello folyamat. Először hardver- és termék licencet vásárolt, és ezt követően teljesítményhangolás történik. Azure SQL Database használata esetén a rendszer az alkalmazás fut, és azt hangolása egy jó ötlet toointerweave hello folyamat. Az igény szerinti kapacitást fizető hello modelljében észlelheti a az alkalmazás toouse hello minimális szükséges erőforrásokat, a jövőbeli növekedésre tervek az alkalmazás, amelyek gyakran helytelen Találgatások alapján hardveren elhelyezésétől helyett. Egyes ügyfelek előfordulhat, hogy nem tootune kérelmet és helyette kiválasztható toooverprovision hardver-erőforrások. Ez a megközelítés előfordulhat, hogy lehet hasznos, ha nem szeretné, hogy toochange a kulcsfontosságú alkalmazások foglalt időszakon belül. De, egy alkalmazás hangolása érdekében minimalizálhatja erőforrás-követelmények és alacsonyabb havi számla hello szolgáltatási szinteket az Azure SQL Database használatakor.

### <a name="application-characteristics"></a>Alkalmazás jellemzői
Annak ellenére, hogy az Azure SQL Database szolgáltatási szinteket tervezett tooimprove teljesítmény stabilitás és kiszámíthatóságot az alkalmazáshoz, néhány ajánlott eljárás segítségével finomhangolhatják a az alkalmazás toobetter előnyeit hello erőforrásokat érhet el egy teljesítményszint szükséges. Jóllehet számos alkalmazás teljesítménynövekedéshez, egyszerűen tooa váltás magasabb teljesítményszintre vagy szolgáltatás réteg, néhány további, a szolgáltatás magasabb szintű toobenefit hangolása alkalmazások szükséges. A nagyobb teljesítmény elérése érdekében fontolja meg a további alkalmazás hangolása alkalmazásokat, amelyek a következő jellemzőkkel rendelkezik:

* **Az alkalmazásokat, amelyek teljesítménycsökkenést "chatty" viselkedés miatt**. Chatty alkalmazások, amelyek bizalmas toonetwork késése túl sok adatelérési műveletek ellenőrizze. Szükség lehet toomodify az ilyen típusú alkalmazások tooreduce hello száma data access műveletek toohello SQL-adatbázis. Például az alkalmazások teljesítményének javíthatja technikák alkalmi lekérdezések kötegelés vagy áthelyezése hello lekérdezi toostored eljárások segítségével. További információkért lásd: [Batch-lekérdezések](#batch-queries).
* **Egy számításigényes munkaterhelést, amely által egy teljes egyetlen számítógépen nem támogatott az adatbázisok**. Adatbázisok, amelyek mérete meghaladja a legmagasabb prémium teljesítményszintet hello hello erőforrásainak előnye származhat hello munkaterhelés kiterjesztése. További információkért lásd: [adatbázisok közötti horizontális](#cross-database-sharding) és [működési particionálás](#functional-partitioning).
* **Az alkalmazásokat, amelyek optimálisnál lekérdezések**. Alkalmazások, különösen a hello az adatelérési réteg, rosszul hangolt lekérdezések, előfordulhat, hogy nem tudják igénybe venni egy magasabb teljesítményszintre. Ez magában foglalja a lekérdezések, amely nem rendelkezik a WHERE záradék, a hiányzó indexek vagy statisztika elavult. Ezek az alkalmazások ki a normál lekérdezési teljesítményének hangolása módszerek előnyeit. További információkért lásd: [hiányzó indexek](#identifying-and-adding-missing-indexes) és [hangolása és leképezésére lekérdezése](#query-tuning-and-hinting).
* **Az alkalmazásokat, amelyek optimálisnál adatok hozzáférési tervezési**. Az alkalmazásokat, amelyek rejlő adatok hozzáférési párhuzamossági problémák, például deadlocking, előfordulhat, hogy nem tudják igénybe venni egy magasabb teljesítményszintre. Érdemes csökkenteni az adatváltások elleni hello Azure SQL Database által hello ügyféloldali gyorsítótárazási adatok hello szolgáltatás Azure gyorsítótárazása az vagy egy másik gyorsítótárazási technológiát. Lásd: [alkalmazás réteg gyorsítótárazás](#application-tier-caching).

## <a name="tune-your-database"></a>Az adatbázis hangolása
Ebben a szakaszban úgy tekintünk, hogy tootune Azure SQL Database toogain hello legjobb teljesítmény érdekében használjon az alkalmazáshoz, és futtassa hello legalacsonyabb szintű lehetséges teljesítményének néhány technikák is. Néhány, az alábbi eljárások egyezik, ajánlott eljárások hangolása hagyományos SQL Server, de más adott tooAzure SQL-adatbázis. Bizonyos esetekben megvizsgálja egy adatbázis toofind területek toofurther hangolási hello felhasznált erőforrásokat, és a hagyományos SQL Server technikák toowork az Azure SQL Database kiterjesztése.

### <a name="identify-performance-issues-using-azure-portal"></a>Azure-portál használatával teljesítményproblémák azonosítása
hello Azure-portálon az eszközök a következő hello segítségével elemezheti és hárítsa el az SQL database teljesítménnyel kapcsolatos problémák:

* [Lekérdezési terheléselemző](sql-database-query-performance.md)
* [SQL Database Advisor](sql-database-advisor.md)

hello Azure portálon találhat további információt mindkét eszköz, és hogyan toouse őket. tooefficiently diagnosztizálhatja és problémák megoldása, ajánlott hello eszközök hello Azure-portálon az első meg. Azt javasoljuk, hogy használja-e, amely arról lesz szó ezt követően a hiányzó indexek és bizonyos esetekben a lekérdezés hangolási módszerek hangolása hello manuális.

További információ az Azure SQL Database problémákat azonosítása a [teljesítményfigyelés](sql-database-single-database-monitor.md) cikk.

### <a name="identifying-and-adding-missing-indexes"></a>Azonosítja és a hiányzó indexek hozzáadása
Az OLTP adatbázis teljesítményének gyakori probléma toohello fizikai adatbázis tervezési vonatkozik. Gyakran adatbázis-sémák tervezhetők meg és tesztelése (vagy a terhelés vagy adatmennyiség) léptékű nélkül. Sajnos lekérdezéstervet hello teljesítményének előfordulhat, hogy elfogadható kis méretű, de jelentősen csökkentheti a termelési szint az adatkötetek alatt. hello leggyakoribb probléma forrása megfelelő indexek toosatisfy szűrők és egyéb korlátozások lekérdezésben hello hiánya. Gyakran hiányzó indexek jegyzékfájlokat táblázatként vizsgálata, ha egy index seek volt elegendő.

Ebben a példában hello kijelölt lekérdezésterv a vizsgálat használja, ha a keresés elegendő lenne:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![A hiányzó indexek lekérdezéstervet](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Az Azure SQL Database segítségével, keresése és a javítás közös hiányzó index feltételek. Tekintse meg az Azure SQL Database beépített dinamikus felügyeleti nézetek index jelentősen csökkenne meg hello becsült költség toorun lekérdezés lekérdezés fordítások. Lekérdezés végrehajtása során SQL-adatbázis nyomon követi, milyen gyakran minden lekérdezésterv végrehajtja a rendszer, és nyomon követi hello becsült lekérdezésterv és hello hello közötti résnek kezdőlapja egy ahol már létezett, hogy az index. A dinamikus felügyeleti nézetek tooquickly becslés milyen módosításokat tooyour fizikai adatbázis tervezési javíthatja az adatbázis és a valós alkalmazások és szolgáltatások teljes munkaterhelés költségét is használhatja.

A lekérdezés tooevaluate lehetőségeket kínál a hiányzó indexek használhatja:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

Ebben a példában hello lekérdezés eredményezett, ez a javaslat:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Létrehozását követően, hogy ugyanazon SELECT utasítás szerzi másik csomagot, amely használ, a keresés nem egy vizsgálatot, és végrehajtja hatékonyabban hello terv:

![Javított indexű lekérdezéstervet](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

hello kulcs insight adott hello egy megosztott i/o-kapacitás, a hagyományos rendszer korlátozottabb, mint az egy dedikált kiszolgálói számítógép. Nincs a prémium minimalizálja a szükségtelen i/o tootake legjobban hello rendszerben az egyes teljesítményszintjének hello Azure SQL Database szolgáltatási szinteket hello DTU. Megfelelő fizikai adatbázis tervezési döntések ütköznek azokkal is jelentősen javítása egyéni lekérdezések hello késését, javításához hello átviteli sebességgel egyidejű kérelmek skálázási egység kezeli, illetve hello költségek szükséges toosatisfy hello lekérdezés minimalizálása érdekében. Hiányzó index dinamikus felügyeleti nézetek hello kapcsolatos további információkért lásd: [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Lekérdezés hangolása és leképezésére
az Azure SQL Database hello lekérdezésoptimalizáló hasonló toohello hagyományos SQL Server lekérdezésoptimalizáló. A legtöbb hello gyakorlati tanácsok a lekérdezések és hello lekérdezésoptimalizáló modell korlátozásai is mintafelismerési ismertetése hello hangolása tooAzure SQL-adatbázis alkalmazni. Ha az Azure SQL Database lekérdezések hangolás kaphat hello további előnye, hogy csökkenti a összesített erőforrás iránti igények kielégítése érdekében. Az alkalmazás képes toorun mint untuned egyenértékű alacsonyabb költségekkel lehet, mert azt egy alacsonyabb teljesítményszintre futtatható.

Példa, amely az SQL Server általános, és amely akkor is érvényes SQL-adatbázis tooAzure, hogy hogyan hello lekérdezési paraméterek optimalizáló "bitaláírásait". Fordításkor hello lekérdezésoptimalizáló hello aktuális értéke, amely egy paraméter toodetermine kiértékeli, hogy hozhat létre a több optimális készíteni. Bár ezt a stratégiát gyakran vezethet, amely jelentősen gyorsabb, mint a ismert paraméterértékek nélkül lefordított tervek tooa lekérdezésterv, jelenleg működik imperfectly mind az SQL Server és az Azure SQL-adatbázis. Néha a hello paraméter nem felszippantásra, és néha hello paraméter felszippantásra van, de hello előállított terv az optimálisnál gyengébb hello paraméterértékeket a munkaterhelés a teljes készlete esetében. Microsoft tartalmaz lekérdezésmutatók (irányelvek) több szándékosan megadhatja a célt és bírálja felül a paraméter elemzés hello alapértelmezett viselkedését. Gyakran például használatakor megoldhatja esetekben, ahol hello alapértelmezett SQL Server vagy az Azure SQL Database viselkedés hiányos egy adott ügyfélhez munkaterhelés számára.

hello következő példa bemutatja, hogyan hozhat létre hello lekérdezésfeldolgozó egy tervet, amely optimálisnál, mind a teljesítmény- és erőforrás-követelményei. Ez a példa is mutatja, hogy a lekérdezés emlékeztető használatakor csökkentése érdekében lekérdezés futtatása idő és erőforrás-követelményei az SQL-adatbázis:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

hello beállítási kód táblázatot hoz létre, amely rendelkezik válik egyenetlenné adatok terjesztési. hello optimális lekérdezési eltér a terv alapján mely paraméter van kiválasztva. Sajnos gyorsítótárazásának hello terv nem mindig fordítsa újra hello lekérdezés hello leggyakoribb paraméter értéke alapján. Igen akkor lehet egy optimálisnál terv toobe a gyorsítótárban, és sok értéket, akkor is, ha egy másik csomagot lehet, hogy jobb terv választás átlagosan használt a. Ezután hello lekérdezésterv hoz létre két tárolt eljárások, amelyek azonos, azzal a különbséggel, hogy egy speciális lekérdezés javaslat.

**A példában 1. rész**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times tooshow hello performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Példa, 2. rész**

(Javasoljuk, várjon legalább 10 percig hello példa, 2. lépés megkezdése előtt, hogy hello eredmények különbözőek a telemetriai adatok eredő hello.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Ebben a példában részét képező összes megpróbál egy paraméteres insert utasítás toorun 1000 alkalommal (toogenerate egy megfelelő terhelés toouse vizsgálati adatok készletként). Végrehajtja a tárolt eljárások, hello lekérdezésfeldolgozó megvizsgálja a hello paraméterérték átadott toohello eljárás során az első fordítási (paraméter "elemzés"). hello processzor hello eredményül kapott terv gyorsítótárazza, és használja azt a későbbi indítások, még akkor is, ha hello paraméter értéke eltérő. hello optimális terv minden esetben előfordulhat, hogy nem használható. Néha kell tooguide hello optimalizáló toopick egy tervet, amely a, amikor először fordították hello lekérdezés hello adott esetben helyett hello átlagos esetet jobb. Ebben a példában hello kezdeti terv, hogy az olvasások az összes sort toofind minden egyező hello paraméter értéke "ellenőrzés" terv állít elő:

![A vizsgálati csomag használatával hangolása lekérdezése](./media/sql-database-performance-guidance/query_tuning_1.png)

Hello eljárás hajtja hello érték 1, mert a hello eredményül kapott terv lett optimális hello érték 1, de nem optimálisnál értéket minden más helyen hello táblában. hello eredmény valószínűleg nem mi alapvetően szükség, ha egyes megtervezése véletlenszerűen, mert hello terv lassabban hajt végre, és több erőforrást toopick.

Hello vizsgálat futtatásakor `SET STATISTICS IO` túl beállítása`ON`, ebben a példában a logikai vizsgálat munkahelyi hello hello háttérben történik. Láthatja, hogy vannak-e (ez nem hatékony, ha hello átlagos eset tooreturn csak egyetlen sor) hello terv által végzett 1,148 olvasások:

![A logikai vizsgálat használatával hangolása lekérdezése](./media/sql-database-performance-guidance/query_tuning_2.png)

hello példa második része hello hello fordítás során lekérdezés mutató tootell hello optimalizáló toouse egy adott értéket használja. Ebben az esetben kényszerül hello lekérdezés processzor tooignore hello átadott érték hello paraméterként, és ehelyett tooassume `UNKNOWN`. Tooa érték hello átlagos gyakorisága (figyelmen kívül hagyva döntés) hello tábla utal. hello eredményül kapott terv egy keresési-alapú tervet készíteni, amely gyorsabb és kevesebb erőforrást használ, átlagosan hello terv az ebben a példában 1. rész:

![Lekérdezés lekérdezés mutató használatával hangolása](./media/sql-database-performance-guidance/query_tuning_3.png)

Megtekintheti a hello hello hatása **sys.resource_stats** tábla (késik hello teszt tölti fel amikor hello adatok hello tábla végrehajtása hello idő). Ez például 1. rész hello 22:25:00 időszak során futtatni, és 2. rész 22:35:00 végre. hello korábbi időkerete használt további erőforrások, mint (miatt terv hatékonyságát fejlesztései) későbbire hello időablakban.

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Lekérdezés eredménye. példa hangolási.](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Bár ebben a példában hello kötet szándékosan kis, optimálisnál paraméterek hello hatása lehet jelentős, különösen a nagyobb méretű adatbázisokhoz. hello különbség szélsőséges esetben gyors esetekben másodpercig és lassú adódó óra közötti lehet.
> 
> 

Ellenőrizheti **sys.resource_stats** toodetermine e hello erőforrás vizsgálat erőforrást használ, több vagy kevesebb mint egy másik tesztelése. Adatok összehasonlításakor külön hello időzítési tesztet hajt végre, hogy azok nem a hello hello azonos 5 perces ablakában **sys.resource_stats** nézet. hello hello a gyakorlatban célja toominimize hello teljes használt erőforrásokat, és nem toominimize hello csúcs erőforrásokat. Általában olyan kódrészletek, késésének optimalizálása is csökkenti hálózatierőforrás-fogyasztás. Győződjön meg arról, hogy hello módosítások tooan alkalmazása szükséges, és hogy hello módosítások nem negatívan befolyásoló hello felhasználói élmény mások, akik esetleg használja a lekérdezésmutatók hello alkalmazásban.

Ha egy munkaterhelés lekérdezések ismétlődő készlete, gyakran logika toocapture és az a terv a választott hello optimalizálási érvényesíteni, mert azt meghajtók hello minimális erőforrás mérete egység szükséges toohost hello adatbázis. Után érvényesíti, időnként újra a hello csomagok toohelp, győződjön meg arról, hogy azok rendelkeznek csökkent. További tudnivalók [mutatók (Transact-SQL) lekérdezés](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Adatbázisok közötti horizontális
Mivel az Azure SQL Database fut, a hagyományos hardvereken, hello kapacitáskorlátait egyetlen adatbázis alacsonyabb, mint a hagyományos helyszíni SQL Server telepítése a rendszer. Egyes ügyfelek felhasználhatja a horizontális technikák toospread Helyadatbázis-műveletekhez több adatbázis hello műveletek nem férnek el az Azure SQL Database egy önálló adatbázis hello határain belül. A legtöbb felhasználói horizontális technikák az Azure SQL Database-e az adatok egy dimenziót osztani több adatbázis. Ez a megközelítés van szüksége, hogy a OLTP-alkalmazások gyakran hajtsa végre az tooonly egy sor- vagy tooa kis csoport hello séma sorok vonatkozó tranzakciók toounderstand.

> [!NOTE]
> SQL-adatbázis most már biztosít egy könyvtár tooassist a horizontális. További információkért lásd: [Elastic Database ügyféloldali kódtárának ismertetése](sql-database-elastic-database-client-library.md).
> 
> 

Például ha egy adatbázis ügyfél neve, a sorrendje, illetve a rendelés részleteit (például az SQL Server alkalmazáshoz tartozó hagyományos példa Northwind adatbázist hello), akkor lehetett ossza fel ezeket az adatokat több adatbázis rendelkező hello csoportosításával kapcsolódó order és rendelés részletei információ. Garantálható, hogy hello ügyféladatok egyetlen adatbázisban marad. hello alkalmazás szeretné-e osztani különböző ügyfelektől adatbázisok, hatékonyan terjednek hello terhelés több adatbázis között. Horizontális, az ügyfelek nem csak elkerülése érdekében hello adatbázis maximális méretkorlátot, de az Azure SQL Database is képes a munkaterheléseket, amelyek jelentősen nagyobb, mint hello határértékeinek hello különböző teljesítményszintek, mindaddig, amíg minden egyes adatbázis hogyan illeszkedik a DTU.

Bár az adatbázis horizontális nem csökkenthető a megoldás hello összesített erőforrás-kapacitást, hatékonyan van elosztva több adatbázis nagyon nagy megoldások támogatása. Az egyes adatbázisok egy szint toosupport nagyon nagy méretű, magas erőforrás-követelmények "hatékony" adatbázisokkal más-más teljesítménybeli parancs.

### <a name="functional-partitioning"></a>Funkcionális particionálás
SQL Server-felhasználók gyakran egyesítése egyetlen adatbázisban számos funkciót. Például ha egy alkalmazás az áruházbeli logika toomanage leltárban, hogy az adatbázis lehet készlet megrendelések, tárolt eljárások és indexelt vagy materializált nézetek, amely a hónap végi jelentéskészítéshez követési társított logikát. Ezzel a módszerrel teszi, hogy könnyebben tooadminister hello adatbázis műveletek, például a biztonsági másolat, de Ön toosize hello hardver toohandle hello csúcsterhelés között az alkalmazás összes funkciójának is igényel.

Az Azure SQL Database kibővített architektúrák használatára, ha egy jó ötlet toosplit a különböző funkciók egy másik adatbázisba alkalmazás is. Ez a módszer segítségével minden alkalmazás egymástól függetlenül méretezi. Egy alkalmazás válik busier (és hello adatbázis növekszik terhelése hello), a hello rendszergazda megadhatja az egyes függvények független teljesítményszintet hello alkalmazásban. Hello a határt, ezzel az architektúrával alkalmazás lehet nagyobb, mint egy hagyományos egyetlen gép kezelni tud, mert több számítógép között megoszlik hello betöltése.

### <a name="batch-queries"></a>Kötegelt lekérdezések
A nagy mennyiségű adatok elérő alkalmazások esetében gyakori, az ad hoc kérdez le, válaszidő jelentős mennyiségű töltött hello alkalmazás réteg és hello Azure SQL adatbázis-rétegből közötti hálózati kommunikáció. Akkor is, ha mindkét hello alkalmazás és az Azure SQL Database van hello ugyanabban az adatközpontban, közötti hello két hello hálózati késés előfordulhat, hogy nagyítható, adatelérési műveletek nagy száma. tooreduce hello hálózati kerek hello adatok utazás közben hozzáférési műveleteket, érdemes lehet hello beállítás tooeither kötegelt hello ad hoc lekérdezéseket vagy toocompile tárolt eljárásként őket. Kötegelt hello ad hoc lekérdezéseket, ha több lekérdezés egy egyetlen út tooAzure SQL-adatbázis egy nagy köteg elküldheti. Ha ad hoc lekérdezéseket egy tárolt eljárás lefordításához el lehet érni hello ugyanaz, mintha azokat kötegelt vezethet. Az eljárás tárolt eljárással is biztosít, akkor hello hello veszélyét annak, hogy az Azure SQL Database hello terveket gyorsítótárazás, hogy használhassa a hello növelésével járó előnyök újra tárolja.

Egyes alkalmazások írási igényű. Egyes esetekben hello teljes i/o egy adatbázis terhelését csökkentik annak eldöntéséhez, hogy hogyan toobatch együtt írja. Ez általában más dolga, mint az explicit tranzakciók helyett a tárolt eljárások és alkalmi kötegek automatikus véglegesítésű tranzakciók. Különböző módszereket használhat értékelése, lásd: [Azure SQL adatbázis-alkalmazások technikák kötegelés](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). A saját munkaterhelés toofind hello jobb modelljét kötegelés kísérletezhet. Különböző tranzakciós konzisztencia biztosítja, amelyek egy modell lehet, hogy toounderstand némileg lehet. Hello jobb munkaterhelés erőforrás használata a lehető legkevesebb keresése szükséges konzisztencia és a teljesítmény kompromisszumot alakítson ki a megfelelő kombinációja hello keresése.

### <a name="application-tier-caching"></a>Alkalmazás szintű gyorsítótárazása
Egyes adatbázis-alkalmazások olvasási műveleteket munkaterhelésekkel rendelkeznek. Rétegek gyorsítótárazás csökkentheti a hello adatbázis hello terhelését, és potenciálisan csökkentheti a hello teljesítmény szint szükséges toosupport adatbázis Azure SQL Database segítségével. A [Azure Redis Cache](https://azure.microsoft.com/services/cache/), ha egy olvasási műveleteket végez, olvasható hello adatok egyszer (vagy lehet, hogy minden alkalmazás szintű machine konfigurációjától függően), majd helyezze el az adatok kívül az SQL-adatbázis. Ez a módszer tooreduce adatbázis terhelést (Processzor- és olvasási i/o-), de nincs hatással lévő tranzakciós konzisztencia, mivel előfordulhat, hogy a hello adatokat olvas be hello gyorsítótár nincs szinkronban a hello adatbázis hello adatait. Bár számos alkalmazás bizonyos fokú inkonzisztenciát elfogadható, hogy igaz nem munkaterhelések. Egy alkalmazás szintű gyorsítótárazási stratégia megvalósítása előtt, teljes mértékben ismernie kell bármely alkalmazás követelményeinek.

## <a name="next-steps"></a>Következő lépések
* Szolgáltatásrétegeiben használt funkciókkal kapcsolatos további információkért lásd: [SQL Database beállításai és teljesítménye](sql-database-service-tiers.md)
* További információ a rugalmas készletek: [Mi az Azure rugalmas készletek?](sql-database-elastic-pool.md)
* Teljesítmény és a rugalmas készletek kapcsolatos információkért lásd: [amikor tooconsider rugalmas készletek](sql-database-elastic-pool-guidance.md)

