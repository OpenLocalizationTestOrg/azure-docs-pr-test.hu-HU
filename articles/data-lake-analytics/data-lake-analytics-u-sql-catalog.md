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
# <a name="get-started-with-hello-u-sql-catalog"></a><span data-ttu-id="9ceba-103">Ismerkedés a U-SQL Catalog hello</span><span class="sxs-lookup"><span data-stu-id="9ceba-103">Get started with hello U-SQL Catalog</span></span>

## <a name="create-a-tvf"></a><span data-ttu-id="9ceba-104">Hozzon létre egy TVF</span><span class="sxs-lookup"><span data-stu-id="9ceba-104">Create a TVF</span></span>

<span data-ttu-id="9ceba-105">Hello előző U-SQL-parancsfájlt, az ismétlődő hello a KIVONATOT tooread hello használata ugyanazon forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="9ceba-105">In hello previous U-SQL script, you repeated hello use of EXTRACT tooread from hello same source file.</span></span> <span data-ttu-id="9ceba-106">A U-SQL hello táblaértékű függvényt (TVF) hello adatokat jövőbeni használatra foglalják magukban.</span><span class="sxs-lookup"><span data-stu-id="9ceba-106">With hello U-SQL table-valued function (TVF), you can encapsulate hello data for future reuse.</span></span>  

<span data-ttu-id="9ceba-107">hello következő parancsfájlt hoz létre egy TVF nevű `Searchlog()` hello alapértelmezett adatbázis és a sémában:</span><span class="sxs-lookup"><span data-stu-id="9ceba-107">hello following script creates a TVF called `Searchlog()` in hello default database and schema:</span></span>

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

<span data-ttu-id="9ceba-108">a következő parancsfájl hello bemutatja, hogyan toouse hello hello előző parancsfájlban megadott TVF:</span><span class="sxs-lookup"><span data-stu-id="9ceba-108">hello following script shows you how toouse hello TVF that was defined in hello previous script:</span></span>

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

## <a name="create-views"></a><span data-ttu-id="9ceba-109">Nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ceba-109">Create views</span></span>

<span data-ttu-id="9ceba-110">Ha egy lekérdezési kifejezésben, a TVF helyett használhatja a U-SQL-NÉZET tooencapsulate adott kifejezés.</span><span class="sxs-lookup"><span data-stu-id="9ceba-110">If you have a single query expression, instead of a TVF you can use a U-SQL VIEW tooencapsulate that expression.</span></span>

<span data-ttu-id="9ceba-111">hello következő parancsfájlt hoz létre egy nevű nézetet `SearchlogView` hello alapértelmezett adatbázis és a sémában:</span><span class="sxs-lookup"><span data-stu-id="9ceba-111">hello following script creates a view called `SearchlogView` in hello default database and schema:</span></span>

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

<span data-ttu-id="9ceba-112">a következő parancsfájl hello hello használati hello meghatározott nézet mutatja be:</span><span class="sxs-lookup"><span data-stu-id="9ceba-112">hello following script demonstrates hello use of hello defined view:</span></span>

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

## <a name="create-tables"></a><span data-ttu-id="9ceba-113">Táblázatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ceba-113">Create tables</span></span>
<span data-ttu-id="9ceba-114">Mivel relációs adatbázis táblákkal, U-SQL választhat egy előre meghatározott séma hozzon létre egy táblát, vagy hozzon létre egy táblát, amely arra következtet hello séma (más néven CREATE TABLE AS SELECT vagy CTAS) hello tábla feltöltő hello lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="9ceba-114">As with relational database tables, with U-SQL you can create a table with a predefined schema or create a table that infers hello schema from hello query that populates hello table (also known as CREATE TABLE AS SELECT or CTAS).</span></span>

<span data-ttu-id="9ceba-115">Hozzon létre egy adatbázist és a két tábla hello parancsfájl a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="9ceba-115">Create a database and two tables by using hello following script:</span></span>

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

## <a name="query-tables"></a><span data-ttu-id="9ceba-116">Lekérdezés táblák</span><span class="sxs-lookup"><span data-stu-id="9ceba-116">Query tables</span></span>
<span data-ttu-id="9ceba-117">Lekérdezheti a táblák, például a létrehozott hello előző parancsfájl, a hello azonos módon, hogy a hello adatfájlok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="9ceba-117">You can query tables, such as those created in hello previous script, in hello same way that you query hello data files.</span></span> <span data-ttu-id="9ceba-118">KIVONAT használatával hoz létre egy sorhalmaz, helyett most olvassa el a toohello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="9ceba-118">Instead of creating a rowset by using EXTRACT, you now can refer toohello table name.</span></span>

<span data-ttu-id="9ceba-119">hello táblázatokból tooread hello átalakító parancsfájl, amely korábban használt módosítása:</span><span class="sxs-lookup"><span data-stu-id="9ceba-119">tooread from hello tables, modify hello transform script that you used previously:</span></span>

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
 ><span data-ttu-id="9ceba-120">Jelenleg nem futtatható egy jelöljön ki azonos hello egy parancsfájl hello táblához hello tábla létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9ceba-120">Currently, you cannot run a SELECT on a table in hello same script as hello one where you created hello table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ceba-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ceba-121">Next Steps</span></span>
* [<span data-ttu-id="9ceba-122">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="9ceba-122">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="9ceba-123">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="9ceba-123">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="9ceba-124">Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="9ceba-124">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
