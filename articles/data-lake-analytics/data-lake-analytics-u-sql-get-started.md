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
# <a name="get-started-with-u-sql"></a>Ismerkedés a U-SQL
U-SQL egy nyelv, amely egyesíti a deklaratív SQL C# imperatív toolet az adatok bármilyen léptékben feldolgozása. Hello méretezhető, terjesztett lekérdezés képesség az U-SQL használatával hatékonyan elemezheti adatok relációs áruházak, például az Azure SQL-adatbázis között. U-SQL, a strukturálatlan adatok feldolgozását olvassa el a séma alkalmazása és egyéni logika és a felhasználó által megadott függvények beszúrni. Ezenkívül az U-SQL, amely lehetővé teszi az módjának részletesebb vezérlés bővítési is tooexecute méretekben. 

## <a name="learning-resources"></a>Képzési erőforrást

* Hello [U-SQL-oktatóanyag](http://aka.ms/usqltutorial) egy részletes útmutató hello U-SQL nyelv legtöbb biztosít. Ez a dokumentum olvasása az összes, aki a U-SQL toolearn ajánlott.
* Hello kapcsolatos részletes információkat **U-SQL nyelvi szintaxisát**, lásd: hello [U-SQL nyelvi referencia](http://go.microsoft.com/fwlink/p/?LinkId=691348).
* toounderstand hello **U-SQL-Tervező alapvetően**, hello Visual Studio blogbejegyzésből [bevezetéséről U-SQL – A nyelv, amely megkönnyíti, nagy adatfeldolgozási](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

## <a name="prerequisites"></a>Előfeltételek

Mielőtt továbblépne a U-SQL hello minták ebben a dokumentumban használatával, olvassa el, és végezze el [oktatóanyag: Data Lake Tools for Visual Studio használatával fejlesztése U-SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md). Hogy az oktatóanyag azt ismerteti, hello idejéről U-SQL az Azure Data Lake Tools for Visual Studio használatával.

## <a name="your-first-u-sql-script"></a>Az első U-SQL-szkript

U-SQL-parancsfájl a következő hello egyszerű, és sok szempontból hello U-SQL nyelv vizsgálatát teszi lehetővé.

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

Ez a parancsfájl nem rendelkezik olyan átalakítása lépéseket. Emellett beolvassa is nevezett hello forrásfájl `SearchLog.tsv`, schematizes, és kiírja hello sorhalmaz vissza SearchLog-first-u-sql.csv nevű fájlba.

Figyelje meg hello kérdőjel következő toohello típusú adatok hello `Duration` mező. Ez azt jelenti, hogy hello `Duration` mezője null értékű lehet.

### <a name="key-concepts"></a>Fő fogalmak
* **A sorhalmaz változók**: minden lekérdezési kifejezés, amely létrehozza a sorhalmaz tooa változó lehet hozzárendelni. A következő T-SQL hello változó elnevezési U-SQL (`@searchlog`, például) hello parancsfájlban.
* Hello **KIBONTÁSA** kulcsszó fájlból olvassa be az adatokat, és olvasási hello séma határozza meg. `Extractors.Tsv`a beépített U-SQL kivonatoló lapon tagolt fájlok van. Egyéni vagyis fejleszthet.
* Hello **kimeneti** írja az adatokat egy sorhalmaz tooa fájlból. `Outputters.Csv()`a beépített U-SQL outputter toocreate van egy vesszővel tagolt fájlt. Egyéni outputters fejleszthet.

### <a name="file-paths"></a>Fájlok elérési útja

hello KIVONATOT és a kimeneti utasítás használja az elérési utat. Abszolút vagy relatív elérési útvonalat lehet:

A következő fájl abszolút elérési út hivatkozik egy Data Lake Store nevű fájlban tooa `mystore`:

    adl://mystore.azuredatalakestore.net/Samples/Data/SearchLog.tsv

A következő fájl elérési útját kezdődik-e `"/"`. Hello alapértelmezett Data Lake Store-fiók tooa fájlra hivatkozik:

    /output/SearchLog-first-u-sql.csv

## <a name="use-scalar-variables"></a>Skaláris változót használja.

Akkor használható skaláris változók jól toomake a parancsfájl karbantartás egyszerűbb. hello előző U-SQL parancsfájlt is írhatók mint:

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

## <a name="transform-rowsets"></a>Átalakítás sorkészletek

Használjon **válasszon** tootransform készletek:

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

hello WHERE záradékot használ egy [C# logikai kifejezés](https://msdn.microsoft.com/library/6a71f45d.aspx). Hello C# kifejezés nyelvi toodo használhatja a saját kifejezések és a funkciók. Összetettebb szűrés alkalmazásával logikai kötőszavak (jelölőnégyzetből) és disjunctions (ORs) még akkor is végrehajtható.

hello következő parancsfájl hello DateTime.Parse() metódus, és egy együtt.

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
 >hello második lekérdezés üzemel, a hello első sorhalmaz, hello összetett létrehoz két szűrő hello eredményét. A változó nevének is felhasználhatja, és hello nevek lexically hatóköre.

## <a name="aggregate-rowsets"></a>Összesített sorkészletek
U-SQL ismerős ORDER BY, a GROUP BY és a összesítések hello tesz lehetővé.

hello következő lekérdezés talál hello teljes időtartam régiónként, és ezután jeleníti meg hello felső öt időtartamok sorrendben.

U-SQL sorkészletek nem őrzi meg a következő lekérdezés hello sorrendjét. Egy kimeneti, így tooorder tooadd ORDER BY toohello kimeneti utasítás van szüksége:

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

hello U-SQL ORDER BY záradék szükséges hello FETCH záradék használatával válassza ki a kifejezésben.

hello U-SQL rendelkező záradékban használt toorestrict hello kimeneti toogroups, amelyek megfelelnek a hello HAVING feltétel lehet:

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

Speciális összesítő forgatókönyvek esetén a dokumentációban hello hello U-SQL referencia [összesíteni, elemzési, és hivatkozzon funkciók](https://msdn.microsoft.com/en-us/library/azure/mt621335.aspx)

## <a name="next-steps"></a>Következő lépések
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md)
