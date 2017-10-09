---
title: "aaaExtend U-SQL parancsfájl az Azure Data Lake Analytics Python |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun Python U-SQL-parancsfájlok kódot"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: f051f56f67522d4f2b8e6e54fd21a5c95ce3ba92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a>Oktatóanyag: Ismerkedés a U-SQL Python kiterjesztése

Az U-SQL Python-bővítmények lehetővé teszi a fejlesztők tooperform nagymértékben párhuzamos végrehajtása Python kódját. a következő példa hello hello alapvető lépéseit mutatja be:

* Használjon hello `REFERENCE ASSEMBLY` utasítás tooenable Python-bővítmények hello U-SQL-parancsfájl
* Hello segítségével `REDUCE` művelet toopartition hello adja meg a kulcs adatokat
* hello U-SQL Python-bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.Python.Reducer`) minden egyes hozzárendelt csúcspont toohello nyomáscsökkentő futó Python kódot
* U-SQL parancsfájl hello beágyazott hello Python kódot tartalmaz, amely rendelkezik a hívott függvény `usqlml_main` , amely fogad egy pandas DataFrame bemeneti adatként, és egy pandas DataFrame kimeneteként adja vissza.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        too"/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Hogyan integrálható a Python U-SQL

### <a name="datatypes"></a>Adattípusok

* Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-Pandas és a U-SQL között
* U-SQL NULL értékek Pandas a konvertált tooand `NA` értékek

### <a name="schemas"></a>Sémák

* A Pandas index vektorok nem támogatottak a U-SQL. Az összes bemeneti adatkeretek hello Python függvény mindig rendelkezik egy 64 bites numerikus index 0 és hello száma mínusz 1 sort. 
* U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz
* U-SQL adatkészletek oszlopnevek, amelyek nem karakterlánc. 

### <a name="python-versions"></a>Python-verziók
Csak a Python 3.5.1 (Windows fordított) használata támogatott. 

### <a name="standard-python-modules"></a>Python-modulok
Összes hello Python modulban találhatók.

### <a name="additional-python-modules"></a>További Python-modulok.
Módosításokon kívül a Python szabványos függvénytárak hello, több gyakran használt python könyvtárak jelennek meg:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Kivétel üzenetek
Jelenleg a Python kódban kivétel jeleníti meg hibaként általános csúcspont. A jövőbeli hello hello U-SQL-feladatot hibaüzenetek hello Python kivétel üzenetet jelenít meg.

### <a name="input-and-output-size-limitations"></a>Bemeneti és kimeneti méretkorlátai
Minden csomópont számára tooit rendelt memória korlátozott mennyiségű. Ezt a határértéket jelenleg egy AU 6 GB-ot. Hello bemeneti és kimeneti DataFrames léteznie kell a hello Python kódját a memóriában, mert hello bemeneti és kimeneti hello teljes mérete nem lehet nagyobb, mint 6 GB.

## <a name="see-also"></a>Lásd még:
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok](data-lake-analytics-use-window-functions.md)

