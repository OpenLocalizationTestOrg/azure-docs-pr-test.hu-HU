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
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="de8d6-103">Oktatóanyag: Ismerkedés a U-SQL Python kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="de8d6-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="de8d6-104">Az U-SQL Python-bővítmények lehetővé teszi a fejlesztők tooperform nagymértékben párhuzamos végrehajtása Python kódját.</span><span class="sxs-lookup"><span data-stu-id="de8d6-104">Python Extensions for U-SQL enable developers tooperform massively parallel execution of Python code.</span></span> <span data-ttu-id="de8d6-105">a következő példa hello hello alapvető lépéseit mutatja be:</span><span class="sxs-lookup"><span data-stu-id="de8d6-105">hello following example illustrates hello basic steps:</span></span>

* <span data-ttu-id="de8d6-106">Használjon hello `REFERENCE ASSEMBLY` utasítás tooenable Python-bővítmények hello U-SQL-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="de8d6-106">Use hello `REFERENCE ASSEMBLY` statement tooenable Python extensions for hello U-SQL Script</span></span>
* <span data-ttu-id="de8d6-107">Hello segítségével `REDUCE` művelet toopartition hello adja meg a kulcs adatokat</span><span class="sxs-lookup"><span data-stu-id="de8d6-107">Using hello `REDUCE` operation toopartition hello input data on a key</span></span>
* <span data-ttu-id="de8d6-108">hello U-SQL Python-bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.Python.Reducer`) minden egyes hozzárendelt csúcspont toohello nyomáscsökkentő futó Python kódot</span><span class="sxs-lookup"><span data-stu-id="de8d6-108">hello Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned toohello reducer</span></span>
* <span data-ttu-id="de8d6-109">U-SQL parancsfájl hello beágyazott hello Python kódot tartalmaz, amely rendelkezik a hívott függvény `usqlml_main` , amely fogad egy pandas DataFrame bemeneti adatként, és egy pandas DataFrame kimeneteként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="de8d6-109">hello U-SQL script contains hello embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

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

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="de8d6-110">Hogyan integrálható a Python U-SQL</span><span class="sxs-lookup"><span data-stu-id="de8d6-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="de8d6-111">Adattípusok</span><span class="sxs-lookup"><span data-stu-id="de8d6-111">Datatypes</span></span>

* <span data-ttu-id="de8d6-112">Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-Pandas és a U-SQL között</span><span class="sxs-lookup"><span data-stu-id="de8d6-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="de8d6-113">U-SQL NULL értékek Pandas a konvertált tooand `NA` értékek</span><span class="sxs-lookup"><span data-stu-id="de8d6-113">U-SQL Nulls are converted tooand from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="de8d6-114">Sémák</span><span class="sxs-lookup"><span data-stu-id="de8d6-114">Schemas</span></span>

* <span data-ttu-id="de8d6-115">A Pandas index vektorok nem támogatottak a U-SQL.</span><span class="sxs-lookup"><span data-stu-id="de8d6-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="de8d6-116">Az összes bemeneti adatkeretek hello Python függvény mindig rendelkezik egy 64 bites numerikus index 0 és hello száma mínusz 1 sort.</span><span class="sxs-lookup"><span data-stu-id="de8d6-116">All input data frames in hello Python function always have a 64-bit numerical index from 0 through hello number of rows minus 1.</span></span> 
* <span data-ttu-id="de8d6-117">U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="de8d6-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="de8d6-118">U-SQL adatkészletek oszlopnevek, amelyek nem karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="de8d6-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="de8d6-119">Python-verziók</span><span class="sxs-lookup"><span data-stu-id="de8d6-119">Python Versions</span></span>
<span data-ttu-id="de8d6-120">Csak a Python 3.5.1 (Windows fordított) használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="de8d6-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="de8d6-121">Python-modulok</span><span class="sxs-lookup"><span data-stu-id="de8d6-121">Standard Python modules</span></span>
<span data-ttu-id="de8d6-122">Összes hello Python modulban találhatók.</span><span class="sxs-lookup"><span data-stu-id="de8d6-122">All hello standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="de8d6-123">További Python-modulok.</span><span class="sxs-lookup"><span data-stu-id="de8d6-123">Additional Python modules</span></span>
<span data-ttu-id="de8d6-124">Módosításokon kívül a Python szabványos függvénytárak hello, több gyakran használt python könyvtárak jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="de8d6-124">Besides hello standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="de8d6-125">Kivétel üzenetek</span><span class="sxs-lookup"><span data-stu-id="de8d6-125">Exception Messages</span></span>
<span data-ttu-id="de8d6-126">Jelenleg a Python kódban kivétel jeleníti meg hibaként általános csúcspont.</span><span class="sxs-lookup"><span data-stu-id="de8d6-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="de8d6-127">A jövőbeli hello hello U-SQL-feladatot hibaüzenetek hello Python kivétel üzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="de8d6-127">In hello future, hello U-SQL Job error messages will display hello Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="de8d6-128">Bemeneti és kimeneti méretkorlátai</span><span class="sxs-lookup"><span data-stu-id="de8d6-128">Input and Output size limitations</span></span>
<span data-ttu-id="de8d6-129">Minden csomópont számára tooit rendelt memória korlátozott mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="de8d6-129">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="de8d6-130">Ezt a határértéket jelenleg egy AU 6 GB-ot.</span><span class="sxs-lookup"><span data-stu-id="de8d6-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="de8d6-131">Hello bemeneti és kimeneti DataFrames léteznie kell a hello Python kódját a memóriában, mert hello bemeneti és kimeneti hello teljes mérete nem lehet nagyobb, mint 6 GB.</span><span class="sxs-lookup"><span data-stu-id="de8d6-131">Because hello input and output DataFrames must exist in memory in hello Python code, hello total size for hello input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="de8d6-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="de8d6-132">See also</span></span>
* [<span data-ttu-id="de8d6-133">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="de8d6-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="de8d6-134">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="de8d6-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="de8d6-135">U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="de8d6-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

