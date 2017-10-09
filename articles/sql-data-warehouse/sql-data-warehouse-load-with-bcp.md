---
title: aaaUse bcp tooload adatokat az SQL Data Warehouse |} Microsoft Docs
description: "Ismerje meg a bcp-t, és hogyan toouse az adatraktározási forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 09a2980585097644924c71899f9e74fb32fbc26d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-bcp"></a>Adatok betöltése a bcp használatával
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

**[BCP] [ bcp]**  parancssori tömeges betöltési segédprogram, amely lehetővé teszi a toocopy adatok SQL Server, az adatfájlok és az SQL Data Warehouse között. Használja a bcp tooimport nagyszámú sorok SQL Data Warehouse tábláiba, vagy az SQL Server tábláiból adatfájlokba tooexport adatokat. Amikor hello queryout kapcsolóval használja, kivéve a bcp nem ismeri a Transact-SQL van szükség.

BCP-t egy gyorsan és egyszerűen elvégezhető toomove kisebb adatkészletek esetében bejövő és kimenő egy SQL Data Warehouse-adatbázis. hello tooload/kinyerésre ajánlott adatok pontos mennyisége függvényében kapcsolat toohello Azure-adatközponthoz csatlakozó hálózati.  Általában a dimenziótáblák könnyen betölthetők és kinyerhetők, ennek ellenére a bcp nem ajánlott nagy adatmennyiségek betöltéséhez vagy kinyeréséhez.  Polybase az ajánlott eszköz betöltéséhez és kinyeréséhez a nagy mennyiségű adat, mint az SQL Data Warehouse hello nagymértékben párhuzamos feldolgozási architektúráját, ami jobb munkát hello.

A bcp-vel a következőket teheti:

* Egy egyszerű parancssori segédprogrammal tooload adatokat használja az SQL Data Warehouse.
* Az SQL Data Warehouse egy egyszerű parancssori segédprogrammal tooextract adatokat használja.

Ez az oktatóanyag a következőket mutatja be:

* Adatok importálása egy táblába a bcp hello parancsban
* Adatok exportálása egy tábla előrejelzése hello bcp out paranccsal

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Előfeltételek
az oktatóanyag teljesítéséhez toostep, lesz szüksége:

* Egy SQL Data Warehouse-adatbázis
* hello telepített bcp parancssori segédprogram
* hello telepített SQLCMD parancssori segédprogram

> [!NOTE]
> Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Adatok importálása az SQL Data Warehouse-ba
Ebben az oktatóanyagban létrehoz egy táblát az Azure SQL Data warehouse-ba, és adatimportálás hello táblába.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>1. lépés: Tábla létrehozása az Azure SQL Data Warehouse-ban
A parancssorból használja az sqlcmd toorun hello lekérdezés toocreate egy táblát a példányon a következő:

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

> [!NOTE]
> Lásd: [tábla áttekintése] [ Table Overview] vagy [CREATE TABLE szintaxis] [ CREATE TABLE syntax] a táblázatok létrehozásáról az SQL Data Warehouse és hello további információ  hello WITH záradékkal használható lehetőségekről.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>2. lépés: Forrásadatfájlok létrehozása
Nyissa meg a Jegyzettömbben, és másolja hello az alábbi adatsorokat egy új szöveges fájlba, és mentse a fájlt tooyour helyi ideiglenes könyvtárba C:\Temp\DimDate2.txt.

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

> [!NOTE]
> Fontos tooremember, hogy a bcp.exe nem támogatja a hello UTF-8 kódolása. A bcp.exe használatakor használjon ASCII fájlokat vagy UTF-16 kódolást.
> 
> 

### <a name="step-3-connect-and-import-hello-data"></a>3. lépés: Csatlakozás és hello adatok importálása
A bcp csatlakozhat, és használja a következő parancs tagjára hello értékeket szükség szerint hello hello adatok importálása:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Hello adatok lett betöltve, a következő lekérdezést az sqlcmd segítségével hello futtatásával ellenőrizheti:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Ez a következő eredmények hello kell visszaadnia:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>4. lépés: Statisztikák létrehozása az újonnan betöltött adatokról
Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését. A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos létrehozni statisztikákat a táblák összes oszlopához hello első betöltés után, vagy hello adatok minden lényeges módosítását fordul elő. A statisztika részletes ismertetése, lásd: hello [statisztika] [ Statistics] hello fejlesztés témakörcsoport témakörében. Az alábbiakban látható egy gyors példa hogyan előterjesztett hello toocreate statisztikák ebben a példában betöltött

Hajtsa végre az alábbi CREATE STATISTICS utasításokat egy sqlcmd parancssorból hello:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Adatok exportálása az SQL Data Warehouse-ból
Ebben az oktatóanyagban létrehoz egy adatfájlt egy SQL Data Warehouse-táblából. A fenti tooa új, dimdate2_export.txt adatfájlba létrehozott hello adatokat fog exportálása.

### <a name="step-1-export-hello-data"></a>1. lépés: Hello adatok exportálása
Hello bcp segédprogram használatával csatlakozhat és exportálhatja az adatokat a következő parancs tagjára hello értékeket szükség szerint hello használata:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Hello adatok helyes exportálását hello új fájl megnyitásával ellenőrizheti. hello fájl hello adatait meg kell felelnie az alábbi hello szöveg:

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

> [!NOTE]
> Miatt elosztott rendszerek jellemzői toohello hello adatok sorrendje nem lehet azonos az SQL Data Warehouse az adatbázisok közötti hello. Másik lehetőség is toouse hello **queryout** függvény a bcp toowrite lekérdezés kibontása helyett hello egész tábla exportálása.
> 
> 

## <a name="next-steps"></a>Következő lépések
A betöltés áttekintése: [Adatok betöltése az SQL Data Warehouse-ba][Load data into SQL Data Warehouse].
További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].

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
