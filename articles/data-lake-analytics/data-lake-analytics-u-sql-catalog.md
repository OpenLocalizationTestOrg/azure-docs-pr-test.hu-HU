---
title: "Ismerkedés az hello U-SQL catalog szolgáltatással |} Microsoft Docs"
description: "Ismerje meg, hogyan katalógus toouse hello U-SQL-tooshare kódja és adatai."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 57143396-ab86-47dd-b6f8-613ba28c28d2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/09/2017
ms.author: edmaca
ms.openlocfilehash: 559bb7a3879031eb290a3e82946d7bf42ac9f553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-u-sql-catalog"></a>Ismerkedés a U-SQL Catalog hello

## <a name="create-a-tvf"></a>Hozzon létre egy TVF

Hello előző U-SQL-parancsfájlt, az ismétlődő hello a KIVONATOT tooread hello használata ugyanazon forrásfájl. A U-SQL hello táblaértékű függvényt (TVF) hello adatokat jövőbeni használatra foglalják magukban.  

hello következő parancsfájlt hoz létre egy TVF nevű `Searchlog()` hello alapértelmezett adatbázis és a sémában:

```
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

a következő parancsfájl hello bemutatja, hogyan toouse hello hello előző parancsfájlban megadott TVF:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/SerachLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Nézetek létrehozása

Ha egy lekérdezési kifejezésben, a TVF helyett használhatja a U-SQL-NÉZET tooencapsulate adott kifejezés.

hello következő parancsfájlt hoz létre egy nevű nézetet `SearchlogView` hello alapértelmezett adatbázis és a sémában:

```
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

a következő parancsfájl hello hello használati hello meghatározott nézet mutatja be:

```
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    too"/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Táblázatok létrehozása
Mivel relációs adatbázis táblákkal, U-SQL választhat egy előre meghatározott séma hozzon létre egy táblát, vagy hozzon létre egy táblát, amely arra következtet hello séma (más néven CREATE TABLE AS SELECT vagy CTAS) hello tábla feltöltő hello lekérdezésből.

Hozzon létre egy adatbázist és a két tábla hello parancsfájl a következő használatával:

```
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Lekérdezés táblák
Lekérdezheti a táblák, például a létrehozott hello előző parancsfájl, a hello azonos módon, hogy a hello adatfájlok lekérdezése. KIVONAT használatával hoz létre egy sorhalmaz, helyett most olvassa el a toohello tábla neve.

hello táblázatokból tooread hello átalakító parancsfájl, amely korábban használt módosítása:

```
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    too"/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >Jelenleg nem futtatható egy jelöljön ki azonos hello egy parancsfájl hello táblához hello tábla létrehozására.

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md)
* [Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
