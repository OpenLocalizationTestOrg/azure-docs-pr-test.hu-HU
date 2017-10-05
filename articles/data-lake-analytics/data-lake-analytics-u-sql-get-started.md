---
title: "Ismerkedés a U-SQL nyelv |} Microsoft Docs"
description: "A U-SQL nyelvű alapismeretei."
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
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 38c4e1b9bd24ef0b8a81f6154620f3f98d3b5ac1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-u-sql"></a><span data-ttu-id="e7d0c-103">Ismerkedés a U-SQL</span><span class="sxs-lookup"><span data-stu-id="e7d0c-103">Get started with U-SQL</span></span>
<span data-ttu-id="e7d0c-104">U-SQL nyelven, hogy feltétlenül szükséges C#, így deklaratív SQL bármilyen léptékben adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-104">U-SQL is a language that combines declarative SQL with imperative C# to let you process data at any scale.</span></span> <span data-ttu-id="e7d0c-105">U-SQL skálázható, elosztott lekérdezés funkció segítségével hatékonyan elemezheti adatok relációs áruházak, például az Azure SQL-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-105">Through the scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="e7d0c-106">U-SQL, a strukturálatlan adatok feldolgozását olvassa el a séma alkalmazása és egyéni logika és a felhasználó által megadott függvények beszúrni.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="e7d0c-107">Ezenkívül a U-SQL, amely lehetővé teszi az részletes szabályozhatják, hogyan hajthat végre léptékű bővítési is.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how to execute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="e7d0c-108">Képzési erőforrást</span><span class="sxs-lookup"><span data-stu-id="e7d0c-108">Learning resources</span></span>

* <span data-ttu-id="e7d0c-109">A [U-SQL-oktatóanyag](http://aka.ms/usqltutorial) egy részletes útmutató a U-SQL nyelv legtöbb biztosít.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-109">The [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of the U-SQL language.</span></span> <span data-ttu-id="e7d0c-110">Ez a dokumentum olvasása az összes, aki a U-SQL további ajánlott.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-110">This document is recommended reading for all developers wanting to learn U-SQL.</span></span>
* <span data-ttu-id="e7d0c-111">További információk a **U-SQL nyelvi szintaxisát**, tekintse meg a [U-SQL nyelvi referencia](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="e7d0c-111">For detailed information about the **U-SQL language syntax**, see the [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="e7d0c-112">Megérteni a **U-SQL-Tervező alapvetően**, a Visual Studio blogbejegyzésből [bevezetéséről U-SQL – A nyelv, amely megkönnyíti, nagy adatfeldolgozási](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="e7d0c-112">To understand the **U-SQL design philosophy**, see the Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7d0c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e7d0c-113">Prerequisites</span></span>

<span data-ttu-id="e7d0c-114">Mielőtt továbblépne ebben a dokumentumban a U-SQL minták keresztül, olvassa el, és végezze el [oktatóanyag: Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e7d0c-114">Before you go through the U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="e7d0c-115">Hogy az oktatóanyag azt ismerteti, beállítás esetén az Azure Data Lake Tools for Visual Studio U-SQL használatával.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-115">That tutorial explains the mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="e7d0c-116">Az első U-SQL-szkript</span><span class="sxs-lookup"><span data-stu-id="e7d0c-116">Your first U-SQL script</span></span>

<span data-ttu-id="e7d0c-117">Az alábbi U-SQL parancsfájl egyszerű, és sok szempontból nyelve a U-SQL vizsgálatát teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-117">The following U-SQL script is simple and lets us explore many aspects the U-SQL language.</span></span>

```
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

OUTPUT @searchlog   
    TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="e7d0c-118">Ez a parancsfájl nem rendelkezik olyan átalakítása lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="e7d0c-119">Emellett beolvassa a forrásfájl neve is `SearchLog.tsv`, schematizes, és kiírja a sorhalmaz vissza SearchLog-first-u-sql.csv nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-119">It reads from the source file called `SearchLog.tsv`, schematizes it, and writes the rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="e7d0c-120">Figyelje meg a kérdéses az adatok mellett írja be a `Duration` mező.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-120">Notice the question mark next to the data type in the `Duration` field.</span></span> <span data-ttu-id="e7d0c-121">Ez azt jelenti, hogy a `Duration` mezője null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-121">It means that the `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="e7d0c-122">Fő fogalmak</span><span class="sxs-lookup"><span data-stu-id="e7d0c-122">Key concepts</span></span>
* <span data-ttu-id="e7d0c-123">**A sorhalmaz változók**: minden lekérdezési kifejezés, amely létrehozza a sorhalmaz változó lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-123">**Rowset variables**: Each query expression that produces a rowset can be assigned to a variable.</span></span> <span data-ttu-id="e7d0c-124">U-SQL-T-SQL változó elnevezési mintát követi (`@searchlog`, például) a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-124">U-SQL follows the T-SQL variable naming pattern (`@searchlog`, for example) in the script.</span></span>
* <span data-ttu-id="e7d0c-125">A **KIBONTÁSA** kulcsszó fájlból olvassa be az adatokat, és határozza meg, olvassa el a séma.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-125">The **EXTRACT** keyword reads data from a file and defines the schema on read.</span></span> <span data-ttu-id="e7d0c-126">`Extractors.Tsv`a beépített U-SQL kivonatoló lapon tagolt fájlok van.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="e7d0c-127">Egyéni vagyis fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="e7d0c-128">A **kimeneti** fájlba írja az adatokat a sorkészlet.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-128">The **OUTPUT** writes data from a rowset to a file.</span></span> <span data-ttu-id="e7d0c-129">`Outputters.Csv()`az egy beépített U-SQL outputter vesszővel tagolt fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-129">`Outputters.Csv()` is a built-in U-SQL outputter to create a comma-separated-value file.</span></span> <span data-ttu-id="e7d0c-130">Egyéni outputters fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="e7d0c-131">Fájlok elérési útja</span><span class="sxs-lookup"><span data-stu-id="e7d0c-131">File paths</span></span>

<span data-ttu-id="e7d0c-132">A KIVONATOT, és a kimeneti utasítás elérési utat használja.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-132">The EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="e7d0c-133">Abszolút vagy relatív elérési útvonalat lehet:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="e7d0c-134">A következő fájl abszolút elérési út egy-egy Data Lake Store nevű fájlra hivatkozik `mystore`:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-134">This following absolute file path refers to a file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="e7d0c-135">A következő fájl elérési útját kezdődik-e `"/"`.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="e7d0c-136">Az alapértelmezett Data Lake Store-fiók egy fájlra vonatkozik:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-136">It refers to a file in the default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="e7d0c-137">Skaláris változót használja.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-137">Use scalar variables</span></span>

<span data-ttu-id="e7d0c-138">Skaláris változót is segítségével egyszerűbbé teszik a parancsfájl karbantartási.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-138">You can use scalar variables as well to make your script maintenance easier.</span></span> <span data-ttu-id="e7d0c-139">Az előző U-SQL-parancsfájlt is írhatók mint:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-139">The previous U-SQL script can also be written as:</span></span>

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a><span data-ttu-id="e7d0c-140">Átalakítás sorkészletek</span><span class="sxs-lookup"><span data-stu-id="e7d0c-140">Transform rowsets</span></span>

<span data-ttu-id="e7d0c-141">Használjon **válasszon** sorkészletek átalakítására:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-141">Use **SELECT** to transform rowsets:</span></span>

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

<span data-ttu-id="e7d0c-142">A WHERE záradékban használ egy [C# logikai kifejezés](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="e7d0c-142">The WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="e7d0c-143">A C# kifejezés nyelv segítségével a saját kifejezések és a funkciók.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-143">You can use the C# expression language to do your own expressions and functions.</span></span> <span data-ttu-id="e7d0c-144">Összetettebb szűrés alkalmazásával logikai kötőszavak (jelölőnégyzetből) és disjunctions (ORs) még akkor is végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="e7d0c-145">Az alábbi parancsfájl a DateTime.Parse() metódus és a együtt használja.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-145">The following script uses the DateTime.Parse() method and a conjunction.</span></span>

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

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 ><span data-ttu-id="e7d0c-146">A második lekérdezés eredménye az első sorhalmaz, és létrehozza a két szűrő összetett működik.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-146">The second query is operating on the result of the first rowset, which creates a composite of the two filters.</span></span> <span data-ttu-id="e7d0c-147">Is felhasználhatja a változó nevét, és a nevek lexically hatóköre.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-147">You can also reuse a variable name, and the names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="e7d0c-148">Összesített sorkészletek</span><span class="sxs-lookup"><span data-stu-id="e7d0c-148">Aggregate rowsets</span></span>
<span data-ttu-id="e7d0c-149">U-SQL lehetővé teszi az ismerős ORDER BY, GROUP BY és összesítések.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-149">U-SQL gives you the familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="e7d0c-150">A következő lekérdezés a teljes időtartam talál régiónként, és ezután jeleníti meg az első öt időtartamok sorrendben.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-150">The following query finds the total duration per region, and then displays the top five durations in order.</span></span>

<span data-ttu-id="e7d0c-151">U-SQL sorkészletek nem őrzi meg a következő lekérdezés sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-151">U-SQL rowsets do not preserve their order for the next query.</span></span> <span data-ttu-id="e7d0c-152">Így egy output sorrendben kell ORDER BY hozzáadása a kimeneti kivonat:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-152">Thus, to order an output, you need to add ORDER BY to the OUTPUT statement:</span></span>

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

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

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="e7d0c-153">A U-SQL ORDER BY záradék van szükség a FETCH záradék használatával válassza ki a kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="e7d0c-153">The U-SQL ORDER BY clause requires using the FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="e7d0c-154">A U-SQL rendelkező záradékot a kimeneti korlátozása csoportokat, amelyek a HAVING feltételnek megfelelő használható:</span><span class="sxs-lookup"><span data-stu-id="e7d0c-154">The U-SQL HAVING clause can be used to restrict the output to groups that satisfy the HAVING condition:</span></span>

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

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
        GROUP BY Region
        HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="e7d0c-155">Speciális összesítő forgatókönyvek esetén dokumentációjában az U-SQL referencia [összesíteni, elemzési, és hivatkozzon funkciók](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="e7d0c-155">For advanced aggregation scenarios, see the The U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7d0c-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7d0c-156">Next steps</span></span>
* [<span data-ttu-id="e7d0c-157">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="e7d0c-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="e7d0c-158">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="e7d0c-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
