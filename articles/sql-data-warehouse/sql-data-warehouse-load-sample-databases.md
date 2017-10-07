---
title: az SQL Data Warehouse aaaLoad mintaadatok |} Microsoft Docs
description: "Mintaadatok betöltése az SQL Data Warehouse adatbázisba"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 3459c42f3aae51c27fd35db7874faf99e1e577e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Mintaadatok betöltése az SQL Data Warehouse adatbázisba
Kövesse ezeket a egyszerű lépéseket tooload és hello Adventure Works vállalat adatbázis lekérdezése. Ezek a parancsfájlok először az sqlcmd toorun SQL, amely táblák és nézetek fog létrehozni. Táblák létrehozása után, hello parancsfájlok bcp tooload adatokat fogja használni.  Ha még nem rendelkezik sqlcmd és telepített bcp, az alábbi hivatkozásokat követve túl[bcp telepítése] [ install bcp] és túl[sqlcmd telepítése][install sqlcmd].

## <a name="load-sample-data"></a>Mintaadatok betöltése
1. Töltse le a hello [Adventure Works mintaparancsfájlok az SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip-fájl.
2. Hello fájlt csomagolja ki a helyi számítógépen a letöltött zip tooa könyvtárból.
3. Kibontott hello fájl aw_create.bat szerkesztése, és állítsa be a következő változók hello fájl hello tetején találhatók hello.  Lehet, hogy tooleave hello "=" és hello paraméter között nem állhat csak szóközökből.  Az alábbiakban példákat hogyan nézhet ki a módosításokat.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. A Windows parancssort szerkeszteni hello aw_create.bat futtassa.  Győződjön meg arról, hogy hello könyvtárban, ahová menteni szeretné a aw_create.bat módosított verzióját.
   Ezt a parancsfájlt a rendszer...
   
   * Az Adventure Works táblát és nézetet, az adatbázisban már meglévő DROP
   * Hello Adventure Works táblák és nézetek létrehozása
   * A bcp Adventure Works táblákra betöltése
   * Ellenőrizze az Adventure Works mindegyikhez hello sorok száma
   * Statisztika gyűjtése az Adventure Works mindegyikhez minden egyes oszlophoz

## <a name="query-sample-data"></a>Lekérdezés mintaadatokat
Ha néhány adatot az SQL Data Warehouse-bA már betöltött, gyorsan néhány lekérdezéseket is futtathat.  toorun lekérdezés, csatlakoztassa az újonnan létrehozott tooyour Adventure Works adatbázisa a Visual Studio és az SSDT, Azure SQL DW, hello leírtaknak [lekérdezése a Visual Studio] [ query with Visual Studio] dokumentum.

Az egyszerű példában válassza ki a utasítás tooget összes hello adatai hello alkalmazottak:

```sql
SELECT * FROM DimEmployee;
```

Egy összetettebb lekérdezés használata szerkezetek GROUP BY toolook például a teljes összeg hello az összes értékesítés naponta példát:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

A WHERE záradék toofilter ki egy bizonyos időpont előtt rendelést a SELECT példát:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Az SQL Data Warehouse szinte minden, amely támogatja az SQL Server T-SQL szerkezetek támogatja.  Bármely különbségek vannak dokumentálva a [telepítse át a kódot] [ migrate code] dokumentációját.

## <a name="next-steps"></a>Következő lépések
Most, hogy egy alkalommal tootry már volt néhány lekérdezést mintaadatokkal, ellenőrizze, hogyan túl[fejlesztése][develop], [betölteni][load], vagy [ Telepítse át] [ migrate] tooSQL Data warehouse-bA.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
