---
title: "Ismerkedés a U-SQL nyelv |} Microsoft Docs"
description: Ismerje meg a U-SQL nyelv hello hello alapjait.
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
ms.openlocfilehash: 5edee0e0d85211e84b3d47895c53d71f0a19f083
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-u-sql"></a><span data-ttu-id="61c08-103">Ismerkedés a U-SQL</span><span class="sxs-lookup"><span data-stu-id="61c08-103">Get started with U-SQL</span></span>
<span data-ttu-id="61c08-104">U-SQL egy nyelv, amely egyesíti a deklaratív SQL C# imperatív toolet az adatok bármilyen léptékben feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="61c08-104">U-SQL is a language that combines declarative SQL with imperative C# toolet you process data at any scale.</span></span> <span data-ttu-id="61c08-105">Hello méretezhető, terjesztett lekérdezés képesség az U-SQL használatával hatékonyan elemezheti adatok relációs áruházak, például az Azure SQL-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="61c08-105">Through hello scalable, distributed-query capability of U-SQL, you can efficiently analyze data across relational stores such as Azure SQL Database.</span></span> <span data-ttu-id="61c08-106">U-SQL, a strukturálatlan adatok feldolgozását olvassa el a séma alkalmazása és egyéni logika és a felhasználó által megadott függvények beszúrni.</span><span class="sxs-lookup"><span data-stu-id="61c08-106">With U-SQL, you can process unstructured data by applying schema on read and inserting custom logic and UDFs.</span></span> <span data-ttu-id="61c08-107">Ezenkívül az U-SQL, amely lehetővé teszi az módjának részletesebb vezérlés bővítési is tooexecute méretekben.</span><span class="sxs-lookup"><span data-stu-id="61c08-107">Additionally, U-SQL includes extensibility that gives you fine-grained control over how tooexecute at scale.</span></span> 

## <a name="learning-resources"></a><span data-ttu-id="61c08-108">Képzési erőforrást</span><span class="sxs-lookup"><span data-stu-id="61c08-108">Learning resources</span></span>

* <span data-ttu-id="61c08-109">Hello [U-SQL-oktatóanyag](http://aka.ms/usqltutorial) egy részletes útmutató hello U-SQL nyelv legtöbb biztosít.</span><span class="sxs-lookup"><span data-stu-id="61c08-109">hello [U-SQL Tutorial](http://aka.ms/usqltutorial) provides a guided walkthrough of most of hello U-SQL language.</span></span> <span data-ttu-id="61c08-110">Ez a dokumentum olvasása az összes, aki a U-SQL toolearn ajánlott.</span><span class="sxs-lookup"><span data-stu-id="61c08-110">This document is recommended reading for all developers wanting toolearn U-SQL.</span></span>
* <span data-ttu-id="61c08-111">Hello kapcsolatos részletes információkat **U-SQL nyelvi szintaxisát**, lásd: hello [U-SQL nyelvi referencia](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="61c08-111">For detailed information about hello **U-SQL language syntax**, see hello [U-SQL Language Reference](http://go.microsoft.com/fwlink/p/?LinkId=691348).</span></span>
* <span data-ttu-id="61c08-112">toounderstand hello **U-SQL-Tervező alapvetően**, hello Visual Studio blogbejegyzésből [bevezetéséről U-SQL – A nyelv, amely megkönnyíti, nagy adatfeldolgozási](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="61c08-112">toounderstand hello **U-SQL design philosophy**, see hello Visual Studio blog post [Introducing U-SQL – A Language that makes Big Data Processing Easy](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61c08-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="61c08-113">Prerequisites</span></span>

<span data-ttu-id="61c08-114">Mielőtt továbblépne a U-SQL hello minták ebben a dokumentumban használatával, olvassa el, és végezze el [oktatóanyag: Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="61c08-114">Before you go through hello U-SQL samples in this document, read and complete [Tutorial: Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="61c08-115">Hogy az oktatóanyag azt ismerteti, hello idejéről U-SQL az Azure Data Lake Tools for Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="61c08-115">That tutorial explains hello mechanics of using U-SQL with Azure Data Lake Tools for Visual Studio.</span></span>

## <a name="your-first-u-sql-script"></a><span data-ttu-id="61c08-116">Az első U-SQL-szkript</span><span class="sxs-lookup"><span data-stu-id="61c08-116">Your first U-SQL script</span></span>

<span data-ttu-id="61c08-117">U-SQL-parancsfájl a következő hello egyszerű, és sok szempontból hello U-SQL nyelv vizsgálatát teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="61c08-117">hello following U-SQL script is simple and lets us explore many aspects hello U-SQL language.</span></span>

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
    too"/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="61c08-118">Ez a parancsfájl nem rendelkezik olyan átalakítása lépéseket.</span><span class="sxs-lookup"><span data-stu-id="61c08-118">This script doesn't have any transformation steps.</span></span> <span data-ttu-id="61c08-119">Emellett beolvassa is nevezett hello forrásfájl `SearchLog.tsv`, schematizes, és kiírja hello sorhalmaz vissza SearchLog-first-u-sql.csv nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="61c08-119">It reads from hello source file called `SearchLog.tsv`, schematizes it, and writes hello rowset back into a file called SearchLog-first-u-sql.csv.</span></span>

<span data-ttu-id="61c08-120">Figyelje meg hello kérdőjel következő toohello típusú adatok hello `Duration` mező.</span><span class="sxs-lookup"><span data-stu-id="61c08-120">Notice hello question mark next toohello data type in hello `Duration` field.</span></span> <span data-ttu-id="61c08-121">Ez azt jelenti, hogy hello `Duration` mezője null értékű lehet.</span><span class="sxs-lookup"><span data-stu-id="61c08-121">It means that hello `Duration` field could be null.</span></span>

### <a name="key-concepts"></a><span data-ttu-id="61c08-122">Fő fogalmak</span><span class="sxs-lookup"><span data-stu-id="61c08-122">Key concepts</span></span>
* <span data-ttu-id="61c08-123">**A sorhalmaz változók**: minden lekérdezési kifejezés, amely létrehozza a sorhalmaz tooa változó lehet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="61c08-123">**Rowset variables**: Each query expression that produces a rowset can be assigned tooa variable.</span></span> <span data-ttu-id="61c08-124">A következő T-SQL hello változó elnevezési U-SQL (`@searchlog`, például) hello parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="61c08-124">U-SQL follows hello T-SQL variable naming pattern (`@searchlog`, for example) in hello script.</span></span>
* <span data-ttu-id="61c08-125">Hello **KIBONTÁSA** kulcsszó fájlból olvassa be az adatokat, és olvasási hello séma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="61c08-125">hello **EXTRACT** keyword reads data from a file and defines hello schema on read.</span></span> <span data-ttu-id="61c08-126">`Extractors.Tsv`a beépített U-SQL kivonatoló lapon tagolt fájlok van.</span><span class="sxs-lookup"><span data-stu-id="61c08-126">`Extractors.Tsv` is a built-in U-SQL extractor for tab-separated-value files.</span></span> <span data-ttu-id="61c08-127">Egyéni vagyis fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="61c08-127">You can develop custom extractors.</span></span>
* <span data-ttu-id="61c08-128">Hello **kimeneti** írja az adatokat egy sorhalmaz tooa fájlból.</span><span class="sxs-lookup"><span data-stu-id="61c08-128">hello **OUTPUT** writes data from a rowset tooa file.</span></span> <span data-ttu-id="61c08-129">`Outputters.Csv()`a beépített U-SQL outputter toocreate van egy vesszővel tagolt fájlt.</span><span class="sxs-lookup"><span data-stu-id="61c08-129">`Outputters.Csv()` is a built-in U-SQL outputter toocreate a comma-separated-value file.</span></span> <span data-ttu-id="61c08-130">Egyéni outputters fejleszthet.</span><span class="sxs-lookup"><span data-stu-id="61c08-130">You can develop custom outputters.</span></span>

### <a name="file-paths"></a><span data-ttu-id="61c08-131">Fájlok elérési útja</span><span class="sxs-lookup"><span data-stu-id="61c08-131">File paths</span></span>

<span data-ttu-id="61c08-132">hello KIVONATOT és a kimeneti utasítás használja az elérési utat.</span><span class="sxs-lookup"><span data-stu-id="61c08-132">hello EXTRACT and OUTPUT statements use file paths.</span></span> <span data-ttu-id="61c08-133">Abszolút vagy relatív elérési útvonalat lehet:</span><span class="sxs-lookup"><span data-stu-id="61c08-133">File paths can be absolute or relative:</span></span>

<span data-ttu-id="61c08-134">A következő fájl abszolút elérési út hivatkozik egy Data Lake Store nevű fájlban tooa `mystore`:</span><span class="sxs-lookup"><span data-stu-id="61c08-134">This following absolute file path refers tooa file in a Data Lake Store named `mystore`:</span></span>

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

<span data-ttu-id="61c08-135">A következő fájl elérési útját kezdődik-e `"/"`.</span><span class="sxs-lookup"><span data-stu-id="61c08-135">This following file path starts with `"/"`.</span></span> <span data-ttu-id="61c08-136">Hello alapértelmezett Data Lake Store-fiók tooa fájlra hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="61c08-136">It refers tooa file in hello default Data Lake Store account:</span></span>

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a><span data-ttu-id="61c08-137">Skaláris változót használja.</span><span class="sxs-lookup"><span data-stu-id="61c08-137">Use scalar variables</span></span>

<span data-ttu-id="61c08-138">Akkor használható skaláris változók jól toomake a parancsfájl karbantartás egyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="61c08-138">You can use scalar variables as well toomake your script maintenance easier.</span></span> <span data-ttu-id="61c08-139">hello előző U-SQL parancsfájlt is írhatók mint:</span><span class="sxs-lookup"><span data-stu-id="61c08-139">hello previous U-SQL script can also be written as:</span></span>

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
        too@out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a><span data-ttu-id="61c08-140">Átalakítás sorkészletek</span><span class="sxs-lookup"><span data-stu-id="61c08-140">Transform rowsets</span></span>

<span data-ttu-id="61c08-141">Használjon **válasszon** tootransform készletek:</span><span class="sxs-lookup"><span data-stu-id="61c08-141">Use **SELECT** tootransform rowsets:</span></span>

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
        too"/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

<span data-ttu-id="61c08-142">hello WHERE záradékot használ egy [C# logikai kifejezés](https://msdn.microsoft.com/library/6a71f45d.aspx).</span><span class="sxs-lookup"><span data-stu-id="61c08-142">hello WHERE clause uses a [C# Boolean expression](https://msdn.microsoft.com/library/6a71f45d.aspx).</span></span> <span data-ttu-id="61c08-143">Hello C# kifejezés nyelvi toodo használhatja a saját kifejezések és a funkciók.</span><span class="sxs-lookup"><span data-stu-id="61c08-143">You can use hello C# expression language toodo your own expressions and functions.</span></span> <span data-ttu-id="61c08-144">Összetettebb szűrés alkalmazásával logikai kötőszavak (jelölőnégyzetből) és disjunctions (ORs) még akkor is végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="61c08-144">You can even perform more complex filtering by combining them with logical conjunctions (ANDs) and disjunctions (ORs).</span></span>

<span data-ttu-id="61c08-145">hello következő parancsfájl hello DateTime.Parse() metódus, és egy együtt.</span><span class="sxs-lookup"><span data-stu-id="61c08-145">hello following script uses hello DateTime.Parse() method and a conjunction.</span></span>

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
        too"/output/SearchLog-transform-datetime.csv"
        USING Outputters.Csv();

 >[!NOTE]
 ><span data-ttu-id="61c08-146">hello második lekérdezés üzemel, a hello első sorhalmaz, hello összetett létrehoz két szűrő hello eredményét.</span><span class="sxs-lookup"><span data-stu-id="61c08-146">hello second query is operating on hello result of hello first rowset, which creates a composite of hello two filters.</span></span> <span data-ttu-id="61c08-147">A változó nevének is felhasználhatja, és hello nevek lexically hatóköre.</span><span class="sxs-lookup"><span data-stu-id="61c08-147">You can also reuse a variable name, and hello names are scoped lexically.</span></span>

## <a name="aggregate-rowsets"></a><span data-ttu-id="61c08-148">Összesített sorkészletek</span><span class="sxs-lookup"><span data-stu-id="61c08-148">Aggregate rowsets</span></span>
<span data-ttu-id="61c08-149">U-SQL ismerős ORDER BY, a GROUP BY és a összesítések hello tesz lehetővé.</span><span class="sxs-lookup"><span data-stu-id="61c08-149">U-SQL gives you hello familiar ORDER BY, GROUP BY, and aggregations.</span></span>

<span data-ttu-id="61c08-150">hello következő lekérdezés talál hello teljes időtartam régiónként, és ezután jeleníti meg hello felső öt időtartamok sorrendben.</span><span class="sxs-lookup"><span data-stu-id="61c08-150">hello following query finds hello total duration per region, and then displays hello top five durations in order.</span></span>

<span data-ttu-id="61c08-151">U-SQL sorkészletek nem őrzi meg a következő lekérdezés hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="61c08-151">U-SQL rowsets do not preserve their order for hello next query.</span></span> <span data-ttu-id="61c08-152">Egy kimeneti, így tooorder tooadd ORDER BY toohello kimeneti utasítás van szüksége:</span><span class="sxs-lookup"><span data-stu-id="61c08-152">Thus, tooorder an output, you need tooadd ORDER BY toohello OUTPUT statement:</span></span>

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
        too@out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

    OUTPUT @res
        too@out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="61c08-153">hello U-SQL ORDER BY záradék szükséges hello FETCH záradék használatával válassza ki a kifejezésben.</span><span class="sxs-lookup"><span data-stu-id="61c08-153">hello U-SQL ORDER BY clause requires using hello FETCH clause in a SELECT expression.</span></span>

<span data-ttu-id="61c08-154">hello U-SQL rendelkező záradékban használt toorestrict hello kimeneti toogroups, amelyek megfelelnek a hello HAVING feltétel lehet:</span><span class="sxs-lookup"><span data-stu-id="61c08-154">hello U-SQL HAVING clause can be used toorestrict hello output toogroups that satisfy hello HAVING condition:</span></span>

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
        too"/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

<span data-ttu-id="61c08-155">Speciális összesítő forgatókönyvek esetén a dokumentációban hello hello U-SQL referencia [összesíteni, elemzési, és hivatkozzon funkciók](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span><span class="sxs-lookup"><span data-stu-id="61c08-155">For advanced aggregation scenarios, see hello hello U-SQL reference documentation for [aggregate, analytic, and reference functions](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="61c08-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61c08-156">Next steps</span></span>
* [<span data-ttu-id="61c08-157">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="61c08-157">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="61c08-158">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="61c08-158">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
