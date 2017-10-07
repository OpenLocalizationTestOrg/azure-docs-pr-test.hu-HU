---
title: "az SQL Data Warehouse táblák aaaManaging statisztikák |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblákon statisztika."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Az SQL Data Warehouse táblák statisztikák kezelése
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

hello további SQL Data Warehouse ismer az adatok hello gyorsabban azt is lekérdezheti az adatokat.  Hello, amelyek biztosítják az SQL Data Warehouse szolgál az adatokról módja statisztikája az adatok gyűjtésével.  Statisztika az adatokról, akkor az egyik legfontosabb dolog hello teheti toooptimize a lekérdezéseket.  Statisztika SQL Data Warehouse hello legoptimálisabb terv a lekérdezések létrehozása érdekében.  Ennek az az oka optimalizáló költségekkel hello SQL Data Warehouse lekérdezés alapú optimalizáló.  Ez azt jelenti, hogy összehasonlítja a különböző lekérdezésterveket hello költségét, és majd úgy dönt, hello terv hello legalacsonyabb költség, amely hello terv, amely végrehajtja a leggyorsabb hello kell.

Statisztika is létrehozható, csak egy oszlop, több oszlop vagy táblázat index.  Statisztika, amely rögzíti a hello tartomány- és értékek kiválasztásánál hisztogram tárolódnak.  Ez akkor különösen fontos, ha hello optimalizáló kell tooevaluate illesztéseket, GROUP BY, HAVING és a WHERE záradék a lekérdezésben.  Például ha hello optimalizáló becslése, hogy jelenleg korlátozza a lekérdezés hello dátum 1 sort ad vissza, nagyon eltérő tervezze meg, mint ha az választhatják, azok dátum becslése kijelölt lesz 1 millió sort adja vissza.  Míg rendkívül fontos a statisztikák létrehozása, is ugyanilyen fontos, hogy statisztika *pontosan* hello tábla hello aktuális állapotát tükrözik.  Naprakész statisztika biztosítja, hogy egy jó terv hello optimalizáló van-e kiválasztva.  hello optimalizáló hello tervekhez csak olyan megegyezik az hello statisztikai adatait.

hello folyamat létrehozása, és frissítse a statisztikai adatokat egy kézi művelet jelenleg azonban nagyon egyszerű toodo.  Ez nem az SQL Server, mely automatikusan létrehozza, és egyetlen oszlopok és indexek statisztikák frissíti.  Az alábbi részleteket hello segítségével nagy mértékben automatizálható hello felügyeleti hello statisztikák az adatokon. 

## <a name="getting-started-with-statistics"></a>Ismerkedés a statisztikák
 Mintavételi statisztikák létrehozása minden oszlop egy egyszerűen tooget parancsot statisztika.  Mivel ez egyaránt fontos tookeep statisztika naprakész, előfordulhat, hogy a konzervatív megközelítést kell tooupdate a statisztika naponta vagy minden egyes betöltés után. Mindig akadnak teljesítmény- és hello költség toocreate és frissítési statisztikái közötti kompromisszumot.  Ha tart túl hosszú toomaintain összes a statisztika, érdemes lehet több szelektív oszlopok statisztika rendelkezik és mely oszlopok tootry toobe kell gyakori frissítése.  Érdemes például tooupdate dátumoszlopának dátumtulajdonságai, naponta, új értékeket vehetők helyett minden betöltés után. Ebben az esetben érheti el hello legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztéseket, GROUP BY, HAVING és a WHERE záradék.  Ha sok tábla hello csak használt oszlopok záradék válassza ki, nem segíthetnek, ezen oszlopokon statisztika, és egy kis további elérhető tooidentify költségeik csak az adott statisztika segít, hello oszlopok csökkentheti hello idő toomaintain a statisztika .

## <a name="multi-column-statistics"></a>Több oszlop statisztikai adatainak
Ezenkívül toocreating statisztika egyetlen oszlopokon, előfordulhat, hogy a lekérdezések ki a előnyeit több oszlop statisztikai adatainak.  Több oszlop statisztikákat statisztikákat létrehozni az oszlopok listája.  Egyetlen oszlop statisztikai adatainak hello első oszlop hello listában tartalmaznak, valamint bizonyos kereszt-oszlop korrelációs adatokat nevű sűrűség.  Például ha egy táblázatot, amelyhez csatlakozik tooanother két oszlopokon, előfordulhat, hogy az SQL Data Warehouse jobban is optimalizálhatja hello terv, támogatja a hello kapcsolat két oszlop között.   Több oszlop statisztikai adatainak javíthatja a lekérdezések teljesítményét az egyes műveletek, például összetett illesztések és a csoportosítás alapját.

## <a name="updating-statistics"></a>Frissítse a statisztikai adatokat
Frissítse a statisztikai adatokat az adatbázis-felügyeleti rutin fontos részét képezi.  Hello terjesztési hello adatbázis adatok megváltozásakor statisztika kell toobe frissítése.  Elévült statisztikát toosub optimális lekérdezési teljesítmény irányítja.

Egy ajánlott dátumoszlopának dátumtulajdonságai tooupdate statisztikák minden nap új dátumok hozzáadása.  Minden alkalommal új sorok töltődnek be a hello data warehouse-ba, új betöltése vagy tranzakció kerülnek. Ezek hello adatok terjesztési módosítsa, majd ellenőrizze hello statisztika elavult. Ezzel szemben ország oszlop egy felhasználói tábla statisztikai előfordulhat, hogy soha nem kell toobe frissítve, hello terjesztési értékek általában nem változik. Feltéve, hogy hello terjesztési állandó, az ügyfelek között, hozzáadása új sorok toohello tábla változat nem fog toochange hello adatok terjesztési. Azonban ha az adatraktár csak tartalmaz egy országon, és az adatok egy új országból kapcsolná, az adatok tárolását, több országokból származó akkor mindenképpen szüksége tooupdate statisztikák hello ország oszlop.

Hello első kérdések tooask lekérdezés hibaelhárítás esetén egyik "naprakészek hello statisztika?"

Ezt a kérdést egyike nem tud válaszolni hello kor hello adatok. Lehet, hogy nagyon régi, ha nincs jelentős változás toohello alapjául szolgáló adatok naprakészek toodate statisztika objektum. Ha a sorok számát hello jelentősen módosult, vagy nincs az adott oszlop értékeinek hello elosztása jelentős változás *majd* tooupdate statisztikája.  

Referenciaként **SQL Server** (nem az SQL Data Warehouse) automatikusan frissíti a statisztikákat ilyen helyzetekben:

* Ha egyetlen sor hello táblázatban szereplő sorok hozzáadásakor, fog kapni a statisztikák automatikus frissítés
* Sorok tooa 500-nál több tábla kezdődő, és kevesebb, mint 500 sorok hozzáadásakor (például a start rendelkezik 499, és adja meg az 500 sorok tooa összesen 999 sorok), az automatikus frissítés jelenik meg 
* 500 sorok közben fog tooadd 500 további sorokat + 20 %-át hello tábla hello méretét a hello statisztikák láthatja az automatikus frissítés előtt

Mivel nincs nincs DMV toodetermine Ha hello táblázatban levő adatok hello utolsó idő statisztika frissítése óta megváltozott, a statisztika hello korát ismerete adja meg a hello kép része.  A következő lekérdezés toodetermine hello legutóbbi a statisztika hello is használhatja, ahol minden táblában frissített.  

> [!NOTE]
> Ne feledje, hogy az adott oszlop értékeinek eloszlását hello jelentős változás esetén frissítse függetlenül hello statisztika legutóbbi frissítése megtörtént.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Egy adatraktár dátumoszlopának dátumtulajdonságai például általában kell gyakori statisztikai adatok frissítése. Minden alkalommal új sorok töltődnek be a hello data warehouse-ba, új betöltése vagy tranzakció kerülnek. Ezek hello adatok terjesztési módosítsa, majd ellenőrizze hello statisztika elavult.  Ezzel szemben az ügyfél táblán nemét oszlop statisztikai előfordulhat, hogy soha nem kell toobe frissítése. Feltéve, hogy hello terjesztési állandó, az ügyfelek között, hozzáadása új sorok toohello tábla változat nem fog toochange hello adatok terjesztési. Azonban ha az adatraktár csak tartalmaz egy nemét, és több genders egy új követelményt eredményezi majd mindenképpen kell tooupdate statisztikák hello nemét oszlop.

További ismertetése [statisztika] [ Statistics] az MSDN Webhelyén.

## <a name="implementing-statistics-management"></a>Végrehajtási statisztika kezelése
Gyakran egy jó ötlet tooextend az adatok betöltése a frissített statisztika folyamat tooensure hello hello terhelés végét. hello adatbetöltés akkor, ha a táblák leggyakrabban módosítása a méretük és/vagy a terjesztési értékek. Ezért ez az a logikai hely tooimplement egyes felügyeleti folyamatokat.

Néhány alapelvek alább a statisztika frissítési hello betöltése során:

* Győződjön meg arról, hogy rendelkezik-e frissíteni kell legalább egy statisztika objektum minden egyes betöltött táblákon. A frissítések hello táblák (sorok számát és oldalszám) információi hello statisztikák frissítés részeként.
* Összpontosítson az ILLESZTÉS, GROUP BY, ORDER BY és DISTINCT záradékban részt vevő oszlopokat
* Fontolja meg "kulcs növekvő" oszlopok például tranzakció dátumok gyakrabban, mivel ezek az értékek nem fog szerepelni hello statisztika hisztogram.
* Érdemes lehet, statikus terjesztési oszlopok gyakran frissíteni.
* Ne feledje, hogy minden statisztikai adat objektum sorozat frissül. Egyszerűen végrehajtási `UPDATE STATISTICS <TABLE_NAME>` nem mindig ideális megoldás – különösen a statisztika objektumok sok nagy táblák esetében.

> [!NOTE]
> [Növekvő kulcs] vonatkozó részletes információért tekintse meg az SQL Server 2014 toohello számossága becslés modell tanulmány.
> 
> 

További ismertetése [számossága becslés] [ Cardinality Estimation] az MSDN Webhelyén.

## <a name="examples-create-statistics"></a>Példák: Statisztikák létrehozása
Ezek a példák azt szemléltetik, hogyan toouse statisztika létrehozásának különböző beállításait. használhatja az egyes oszlopok hello-beállítások az adatok jellemzői hello és hello oszlop a lekérdezésekben használt hogyan függ.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Egy oszlop statisztikák létrehozása az alapértelmezett beállításokkal
egy oszlop toocreate statisztikák egyszerűen adjon hello statisztika objektum nevét hello hello oszlop neve.

Ez a szintaxis hello alapértelmezett beállításokat használja. Alapértelmezés szerint az SQL Data Warehouse hello tábla 20 százalékát minták, amikor létrehozza statisztika.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Példa:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Minden sor megvizsgálásával egyoszlopos statisztikát létrehozása
hello alapértelmezett mintavételi 20 százalékos aránya a legtöbb esetben elegendő. Hello mintavételi ráta azonban módosíthatja.

teljes toosample hello table, a következő szintaxist használja:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Példa:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a>C. Hozza létre a egyoszlopos statisztikát hello mintaméret megadásával
Másik lehetőségként százalékban hello mintaméret adhat meg:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a>D. Egyoszlopos statisztikát csak egyes hello sorok létrehozása
Egy másik lehetőség, létrehozhat statisztika hello sorok része a táblában. Ezt nevezik a szűrt statisztikai.

Használhat például szűrt statisztikákat egy adott partícióra egy nagy particionált tábla tooquery tervezése során. Létrehozásával statisztikák csak hello partíció értékek, hello statisztika hello pontossága fog javítása, és ezért javíthatja a lekérdezések teljesítményét.

Ez a Példa statisztika értéktartománya hoz létre. hello értékek könnyen lehet meghatározott értékek tartományán toomatch hello partíció.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Hello lekérdezés optimalizáló tooconsider szűrt statisztikákat használja, akkor azt úgy dönt, hogy hello elosztott lekérdezésterv, a hello lekérdezés hello hello statisztika objektum meghatározását kell férnie. Hello előző példában hello lekérdezés amikor záradékban kell toospecify Oszlop1 értékek között 2000101 és 20001231 használatával.
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a>E. Hozzon létre egyoszlopos statisztikát összes hello-beállítások
Természetesen kombinálhatja hello beállítások együtt. az alábbi példa hello egy szűrt statisztikákat objektum egyéni mintaméret hoz létre:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Hello teljes referenciáért lásd: [CREATE statistics UTASÍTÁSHOZ] [ CREATE STATISTICS] az MSDN Webhelyén.

### <a name="f-create-multi-column-statistics"></a>F. Több oszlop statisztikai adatainak létrehozása
toocreate többoszlopos statisztika, egyszerűen hello előző példák használja, de további oszlopok megadása.

> [!NOTE]
> hello hisztogram, amely csak akkor hello lekérdezési eredményhez, sorainak száma tooestimate hello hello statisztika Objektumdefiníció szereplő első oszlop.
> 
> 

Ebben a példában hello hisztogram van *termék\_kategória*. Kereszt-oszlop statisztikai adatainak kiszámítása *termék\_kategória* és *termék\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Mivel közötti összefüggés *termék\_kategória* és *termék\_sub\_kategória*, a többoszlopos stat akkor lehet hasznos, ha ezekben az oszlopokban érhetők el a hello ugyanannyi időt vesz igénybe.

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a>G. Egy táblázat összes hello oszlopa statisztikák létrehozása
Egyirányú toocreate statisztika tooissues CREATE statistics UTASÍTÁSHOZ parancsok hello tábla létrehozása után.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a>H. A tárolt eljárás toocreate statisztika használatát egy adatbázis összes oszlopa
Az SQL Data Warehouse nem rendelkezik túl a rendszer tárolt eljárás megfelelőjét az SQL Server [sp_create_stats] [-]. Ez a tárolt eljárás objektumot hoz létre egy oszlop statisztika hello adatbázis, amely még nem rendelkezik statisztikai minden oszlop alapján.

Ez segítséget nyújt az adatbázis tervét az első lépései. Szabad tooadapt látja azt tooyour kell.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Ezzel az eljárással hello táblázat összes oszlopa toocreate statisztikák egyszerűen hello eljárás hívása.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Példák: statisztika frissítése
tooupdate statisztika, a következőket teheti:

1. Egy statisztikai objektum frissítése. Adja meg a statisztika objektum tooupdate kívánja hello hello nevét.
2. Egy tábla összes statisztika objektumok frissítése. Adja meg egy adott statisztika objektum helyett hello tábla hello nevét.

### <a name="a-update-one-specific-statistics-object"></a>A. Egy adott statisztika objektum frissítése
A következő szintaxist tooupdate egy adott statisztika objektum hello használata:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Példa:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Adott statisztika objektumok frissítésével hello időt és erőforrásokat szükséges toomanage statisztika minimalizálása érdekében. Ehhez az egyes-re, azonban toochoose hello legjobb statisztika objektumok tooupdate.

### <a name="b-update-all-statistics-on-a-table"></a>B. Egy tábla összes statisztika frissítése
Ez azt jelenti, egy egyszerű módszer egy tábla összes hello statisztika objektumok frissítése.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Példa:

```sql
UPDATE STATISTICS dbo.table1;
```

A jelen nyilatkozat könnyen toouse. Ne feledje azonban ez frissíti az összes statisztika hello táblán, és ezért előfordulhat, hogy hajtsa végre a szükségesnél több munkát. Ha hello teljesítmény darabolása nem okoz problémát, ez az egyértelműen hello legegyszerűbb és leghatékonyabb módon tooguarantee statisztika naprakészek legyenek.

> [!NOTE]
> Egy tábla összes statisztika frissítése, az SQL Data Warehouse egy vizsgálat toosample hello tábla minden egyes statistics hajtja végre. Hello tábla túl nagy, ha sok oszlopot, és sok statisztika, előfordulhat, hogy hatékonyabb tooupdate egyedi statisztika igények alapján.
> 
> 

Egy végrehajtásához egy `UPDATE STATISTICS` eljárást lásd: hello [az ideiglenes táblák] [ Temporary] cikk. hello megvalósítási módja kissé eltérő toohello `CREATE STATISTICS` fent leírt lépéseket, de hello végeredménynek van hello azonos.

Hello teljes szintaxisát, lásd: [Update Statistics] [ Update Statistics] az MSDN Webhelyén.

## <a name="statistics-metadata"></a>Statisztika metaadatok
Több rendszernézet és, hogy használható-e toofind információkat statisztikai függvények is van. Például láthatja, ha egy statisztika objektum elavult lehet hello statisztikák-date függvény toosee használatával statisztika volt utoljára létrehozásakor vagy frissítésekor.

### <a name="catalog-views-for-statistics"></a>A statisztika katalógusnézetekre
Ezek a rendszer nézetek statisztika ismertetik:

| Katalógusnézet használatával derítheti ki | Leírás |
|:--- |:--- |
| [sys.Columns][sys.columns] |Egy sor minden egyes oszlophoz. |
| [sys.Objects][sys.objects] |Egy sor minden objektum hello adatbázisban. |
| [sys.schemas][sys.schemas] |Egy sor minden hello adatbázis-séma. |
| [sys.stats][sys.stats] |Egy sor minden egyes statisztika objektumhoz. |
| [sys.stats_columns][sys.stats_columns] |Egy sor hello statisztika objektum minden egyes oszlophoz. Hivatkozások biztonsági toosys.columns. |
| [sys.Tables][sys.tables] |Egy sor minden táblához (külső táblát tartalmazza). |
| [sys.table_types][sys.table_types] |Egy sor egyes adattípusok esetében. |

### <a name="system-functions-for-statistics"></a>A statisztika rendszer funkciók
A rendszer függvények hasznosak statisztika használata:

| Rendszer-funkció | Leírás |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Utolsó frissítés dátuma hello statisztika objektum. |
| [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Hello terjesztési értékek összefoglaló szintű és részletes információt nyújt hello statisztika objektum tudja értelmezni. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Statisztika oszlopok és funkciók egyesítése egy nézet
Ez a nézet során az oszlopok ki az egymáshoz kapcsolódó toostatistics, és az eredmények hello [STATS_DATE()] [] függvényből együtt.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() példák
DBCC SHOW_STATISTICS() statisztika objektumon belül tárolt hello adatainak megjelenítése. Ezek az adatok származnak három részből áll.

1. Fejléc
2. Sűrűség vektoros
3. Hisztogram

hello fejléc metaadatok hello statisztika. hello hisztogram hello terjesztési értékek hello első hello statisztika objektum kulcsoszlop jeleníti meg. hello sűrűség vektoros kereszt-oszlop korrelációs méri. SQLDW kiszámítja hello adatok hello statisztika objektum egyik számossága becslése.

### <a name="show-header-density-and-histogram"></a>Fejléc, sűrűség és hisztogram megjelenítése
Az egyszerű példában statisztika objektum összes három részből.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Példa:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>DBCC SHOW_STATISTICS(); egy vagy több részei megjelenítése
Ha érdekli csak megtekintés részét, használja a hello `WITH` záradék, és adja meg, mely részeit toosee szeretné:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Példa:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() különbségek
DBCC SHOW_STATISTICS() szigorúbban vezettek be az SQL Data Warehouse képest tooSQL kiszolgáló.

1. Nem dokumentált funkciók nem támogatottak.
2. Nem használható a Stats_stream
3. Nem tudja csatlakoztatni meghatározott fájlcsoportokat statisztikai adatok eredményeit pl. (STAT_HEADER ILLESZTÉSI DENSITY_VECTOR)
4. Üzenet tiltási NO_INFOMSGS nem állítható be
5. Statisztika neveket burkoló szögletes zárójelek között nem használható.
6. Oszlop nevek tooidentify statisztika objektumok nem használható.
7. Egyéni hiba 2767 nem támogatott

## <a name="next-steps"></a>Következő lépések
További részletekért lásd: [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] az MSDN Webhelyén.  toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [tábla particionáló] [ Partition] és [ Az ideiglenes táblák][Temporary].  Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].  

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

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
