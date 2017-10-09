---
title: "az SQL Serverről az Azure SQL Data warehouse-ba (bcp) aaaLoad adatok |} Microsoft Docs"
description: "Kisebb adatméret esetében a bcp tooexport adatait az SQL Server tooflat fájlok és használja hello és adatokat importálhat közvetlenül az Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a03b5403d123e8814ae73a7cce8e6851c8b264a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Adatok betöltése az SQL Serverből az Azure SQL Data Warehouse-ba (egybesimított fájlok)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Kisebb adatkészletek használja az SQL Server hello bcp parancssori segédprogram tooexport adatait, és ezután töltse be tooAzure SQL Data Warehouse közvetlen.

Az oktatóanyag során a következőket hajtja végre a bcp segítségével:

* Egy tábla exportálása az SQL Server hello bcp out paranccsal használatával (vagy hozzon létre egy egyszerű fájl)
* Hello tábla importálása egy egybesimított fájl tooSQL Data warehouse-bA.
* Statisztikák létrehozása hello betöltött adatokról.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Előkészületek
### <a name="prerequisites"></a>Előfeltételek
az oktatóanyag teljesítéséhez toostep, lesz szüksége:

* Egy SQL Data Warehouse-adatbázis
* hello telepített bcp parancssori segédprogram
* hello telepített sqlcmd parancssori segédprogram

Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Adatok ASCII vagy UTF-16 formátumban
Ha ez az oktatóanyag a saját adataival próbálja, az adatok kell toouse hello ASCII vagy UTF-16 kódolást, mert a bcp nem támogatja az UTF-8. 

A PolyBase támogatja az UTF-8 formátumot, de az UTF-16 formátumot még nem. Vegye figyelembe, hogy ha azt szeretné, hogy a polybase-zel toocombine bcp szüksége lesz tootransform hello adatok tooUTF-8 után az SQL-kiszolgálóról. 

## <a name="1-create-a-destination-table"></a>1. Céltábla létrehozása
Adjon meg egy táblát az SQL Data Warehouse hello céltábla hello terhelést el. hello tábla oszlopainak hello toohello adatfájl egyes soraiban szereplő adatoknak kell megfelelnie.

toocreate tábla, nyisson meg egy parancssort, és az sqlcmd.exe toorun hello a következő parancsot használja:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Forrásadatfájlok létrehozása
Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt. Ezek az adatok ASCII formátumban vannak.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Választható) tooexport a saját adatait egy SQL Server-adatbázisból, nyisson meg egy parancssort, és futtassa a következő parancs hello. Cserélje le a TableName, ServerName, DatabaseName, Username és Password adatokat a saját adataira.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-hello-data"></a>3. Hello adatok betöltése
tooload hello adatok, nyisson meg egy parancssort, és futtassa a következő parancsot, a kiszolgáló nevét, a adatbázis nevét, a felhasználónév és a jelszó hello értékeket a saját cserélje le hello.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

A parancs tooverify hello megfelelően be adatok használata

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

hello eredményeket kell kinéznie:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="4-create-statistics"></a>4. Statisztika létrehozása
Az SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását vagy automatikus frissítését. tooget hello legjobb lekérdezési teljesítményt, akkor fontos toocreate statisztikákat a táblák összes oszlopához hello első betöltés után, vagy minden lényeges módosítását követően hello adataiban. A statisztika részletes ismertetése, [statisztika][Statistics]. 

Futtatási hello következő parancsot a toocreate statisztikák az újonnan betöltött táblákon.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Adatok exportálása az SQL Data Warehouse-ból
A Szórakozás exportálhatja a hello adatokat, vissza az SQL Data Warehouse kívül betöltött.  hello parancs tooexport pontosan hello ugyanaz, mint az SQL Server exportálása.

Van azonban hello eredmények eltérőek lesznek. Mivel a hello adatok rendszer elosztott helyeken az SQL Data Warehouse, az adatok exportálásakor minden számítási csomópont böngészőbe írja azt toohello kimeneti adatfájlt. hello hello hello kimeneti fájl adatainak sorrendje valószínűleg toobe hello hello hello bemeneti fájl adatainak sorrendje eltér.

### <a name="export-a-table-and-compare-exported-results"></a>Tábla exportálása és az exportált eredmények összehasonlítása
toosee hello tegye elérhetővé az adatokat, nyisson meg egy parancssort, és futtassa a parancsot a saját paramétereivel. A kiszolgálónév annak az Azure logikai SQL Server hello neve.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Hello adatok helyes exportálását hello új fájl megnyitásával ellenőrizheti. hello fájl hello adatait meg kell felelnie az alábbi hello szöveg, de valószínűleg rendezhető más sorrendben:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-hello-results-of-a-query"></a>A lekérdezés eredményeinek hello exportálása
Használhatja a hello **queryout** bcp tooexport hello hello egész tábla exportálása helyett a lekérdezés eredményeinek funkcióját. 

## <a name="next-steps"></a>Következő lépések
A betöltés áttekintése: [Adatok betöltése az SQL Data Warehouse-ba][Load data into SQL Data Warehouse].
További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].
Lásd: [tábla áttekintése] [ Table Overview] vagy [CREATE TABLE szintaxis] [ CREATE TABLE syntax] SQL Data Warehouse a táblázatok létrehozásáról további információt.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
