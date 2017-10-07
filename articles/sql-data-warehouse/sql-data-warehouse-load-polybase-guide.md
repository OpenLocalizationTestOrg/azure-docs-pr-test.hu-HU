---
title: az SQL Data Warehouse PolyBase az aaaGuide |} Microsoft Docs
description: "Irányelvek és javaslatok a PolyBase az SQL Data Warehouse forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 6/5/2016
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: b05e4c5d528f2fe1c60d6855b5333065f0c908ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Útmutató az SQL Data Warehouse PolyBase használatával
Ez az útmutató az SQL Data Warehouse PolyBase használatára vonatkozó gyakorlati információkat biztosít.

tooget indult el, lásd: hello [adatok betöltése a PolyBase] [ Load data with PolyBase] oktatóanyag.

## <a name="rotating-storage-keys"></a>Tárolási kulcsok elforgatása
Az idő tootime érdemes biztonsági okokból toochange hello hozzáférési kulcs tooyour blob-tároló.

Ez a feladat a "hello kulcsok elforgatása" néven ismert folyamat toofollow legtöbb elegáns módon tooperform hello. Talán észrevette, hogy két kulccsal is rendelkezik tárolási a blob storage-fiók esetében. Ez azért, hogy, hogy térjen át

Az Azure tárfiókkulcsok elforgatása rendkívül egyszerű három lépést folyamat

1. Második adatbázishoz kötődő hitelesítő adatok alapján hello másodlagos tárelérési kulcs létrehozása
2. Ki az új hitelesítőadat-alapú második külső adatforrás létrehozása
3. Dobja el és létrehozni a hello külső toohello új külső adatforrás mutat

Ha áttelepítette az összes külső táblák toohello új külső adatforrás majd végezhet hello feladatok törlése:

1. Első külső adatforrásból eldobási
2. Első adatbázis kötődő hitelesítő adatok hello elsődleges tárelérési kulcs alapján
3. Jelentkezzen be Azure, majd újra létrehozza a készen áll a hello hello elsődleges elérési kulcsot következő indításakor

## <a name="query-azure-blob-storage-data"></a>Az Azure blob storage-adatok lekérdezése
Külső táblák lekérdezéseket egyszerűen használja hello táblanév, mintha egy relációs tábla volt.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [!NOTE]
> A külső tábla is sikertelenek hello hiba *"lekérdezés végrehajtása megszakadt – hello maximális elutasítás küszöbérték elérte a külső forrásból történő beolvasás során"*. Ez azt jelzi, hogy a külső adatokat tartalmaz *inkonzisztencia* rögzíti. Rekord "hibás" tekintendő, ha hello tényleges adatokat típusok/oszlopok száma nem egyezik a hello oszlopdefiníciók hello külső tábla, vagy ha hello adatai nem felelnek meg a megadott külső fájlformátum toohello. toofix, győződjön meg arról, hogy a külső tábla és a külső fájlformátum-meghatározások helyességéről, valamint a külső adatokat megfelel toothese definíciókat. Abban az esetben, ha egy külső rekordok részét inkonzisztencia, kiválaszthatja tooreject ezeket a rekordokat a lekérdezések a külső tábla létrehozása DDL hello elutasítás beállítások használatával.
> 
> 

## <a name="load-data-from-azure-blob-storage"></a>Adatok betöltése az Azure Blob Storage-ből
Ez a példa adatokat tölt az Azure blob storage tooSQL az operatív adatbázisból.

Adattárolás közvetlenül eltávolítja hello adatátviteli idő lekérdezések. Adattárolás egy oszloptárindexet tartalmazó növeli a lekérdezési teljesítmény elemzési lekérdezések által too10x fel.

A példa hello CREATE TABLE AS SELECT utasítás tooload adatokat. Új tábla hello örökli hello hello lekérdezésben szereplő oszlopokat. Ezek az oszlopok adattípusai hello örököl hello külső tábla definíciójában.

CREATE TABLE AS SELECT a magas performant Transact-SQL utasítást, amely a párhuzamos tooall hello hello adatokat tölt számítási csomópontok az SQL Data warehouse.  A hello nagymértékben párhuzamos feldolgozási (MPP) motor az Analytics Platform System eredetileg, és jelenleg az SQL Data Warehouse.

```sql
-- Load data from Azure blob storage tooSQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Lásd: [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

## <a name="create-statistics-on-newly-loaded-data"></a>Statisztika létrehozása újonnan betöltött adatokról
Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.  A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő.  A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében.  Az alábbiakban látható egy gyors példa hogyan előterjesztett hello toocreate statisztikák ebben a példában betöltött.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-tooazure-blob-storage"></a>Adatok tooAzure blob-tároló exportálása
Ez a szakasz bemutatja, hogyan tooexport adatait az SQL Data Warehouse tooAzure blob-tároló. A példa létrehozása külső tábla AS válassza ki azt a magas performant Transact-SQL utasítás tooexport hello adatokat az összes hello számítási csomópont párhuzamosan.

hello alábbi példa létrehoz egy külső tábla Weblogs2014 oszlopdefiníciók és dbo adatait. Webes naplók tábla. hello külső tábla definíciójának SQL Data Warehouse tárolja, és hello hello SELECT utasítás eredményei exportált toohello hello adatforrás által meghatározott hello blob tároló "/ / log2014/archiválására" könyvtárat. hello adatok exportálása hello megadott szöveg formátumban.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Különítse el a felhasználók betöltésekor
Nincs gyakran egy szükséges toohave adatok betölthetnek egy SQL DW több felhasználó. Mivel hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] VEZÉRLÉSI engedélyekkel kell rendelkeznie a hello adatbázis kat, az elérés több felhasználóval rendelkező összes keresztül. toolimit, hello ellenőrzési MEGTAGADÁSI utasítással.

Példa: Fontolja meg adatbázis sémák schema_A az osztály A, és a részleg B segítségével adatbázis felhasználók user_A schema_B és user_B felhasználóknak PolyBase betöltése az osztály A és B, illetve a. Mindkettő rendelkezik vezérlő adatbázis-engedélyek.
hello creators séma A és B most zárolását, hogy a sémáikat MEGTAGADÁS használatával:

```sql
   DENY CONTROL ON SCHEMA :: schema_A toouser_B;
   DENY CONTROL ON SCHEMA :: schema_B toouser_A;
```   
 Ennek, user_A és user_B most zárolja a hello más osztály sémáját.
 


## <a name="next-steps"></a>Következő lépések
toolearn áthelyezése adatok tooSQL Data Warehouse kapcsolatos további információkért lásd: hello [adatok áttelepítése – áttekintés][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
