---
title: "fürt megosztott kötetei szolgáltatás aaaLoad adatait fájlt az Azure SQL Database (bcp) |} Microsoft Docs"
description: "Kisebb adatméret esetében a bcp tooimport adatokat használja az Azure SQL-adatbázisba."
services: sql-database
documentationcenter: NA
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 875f9b8d-f1a1-4895-b717-f45570fb7f80
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 9350e459aa844223820fbbd849a830cf0354d4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-csv-into-azure-sql-database-flat-files"></a>Adatok betöltése CSV-fájlból az Azure SQL Database-be (egybesimított fájlok)
Az Azure SQL-adatbázisba használható hello bcp parancssori segédprogram tooimport adatok CSV-fájlból.

## <a name="before-you-begin"></a>Előkészületek
### <a name="prerequisites"></a>Előfeltételek
toocomplete hello ebben a cikkben ismertetett visszaállítási lépésekkel, a szükséges:

* Egy Azure SQL Database logikai kiszolgáló és adatbázis
* hello telepített bcp parancssori segédprogram
* hello telepített sqlcmd parancssori segédprogram

Hello bcp és Sqlcmd parancssori segédeszközöket letöltheti hello [Microsoft Download Center][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Adatok ASCII vagy UTF-16 formátumban
Ha ez az oktatóanyag a saját adataival próbálja, az adatok kell toouse hello ASCII vagy UTF-16 kódolást, mert a bcp nem támogatja az UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Céltábla létrehozása
Adjon meg egy táblát az SQL-adatbázis hello cél táblázatként. hello tábla oszlopainak hello toohello adatfájl egyes soraiban szereplő adatoknak kell megfelelnie.

toocreate tábla, nyisson meg egy parancssort, és az sqlcmd.exe toorun hello a következő parancsot használja:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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

```bcp
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t , 
```

## <a name="3-load-hello-data"></a>3. Hello adatok betöltése
tooload hello adatok, nyisson meg egy parancssort, és futtassa a következő parancsot, a kiszolgáló nevét, a adatbázis nevét, a felhasználónév és a jelszó hello értékeket a saját cserélje le hello.

```bcp
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ,
```

A parancs tooverify hello megfelelően be adatok használata

```bcp
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

## <a name="next-steps"></a>Következő lépések
egy SQL Server-adatbázis toomigrate lásd [SQL Server-adatbázis áttelepítése](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
