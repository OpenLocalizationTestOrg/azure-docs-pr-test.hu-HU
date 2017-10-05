---
title: "Python az Azure Data Lake Analytics U-SQL-parancsfájlok kiterjesztése |} Microsoft Docs"
description: "Megtudhatja, hogyan Python kódját a U-SQL-parancsfájlok futtatása"
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
ms.openlocfilehash: d18ef1f747aee2fa01cef9891432d0461031ee4c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="2b633-103">Oktatóanyag: Ismerkedés a U-SQL Python kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="2b633-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="2b633-104">U-SQL Python-bővítmények a fejlesztők hajtsa végre a Python kód nagymértékben párhuzamos végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2b633-104">Python Extensions for U-SQL enable developers to perform massively parallel execution of Python code.</span></span> <span data-ttu-id="2b633-105">A következő példa bemutatja az alapvető lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2b633-105">The following example illustrates the basic steps:</span></span>

* <span data-ttu-id="2b633-106">Használja a `REFERENCE ASSEMBLY` utasítás a U-SQL parancsfájl Python bővítmények engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2b633-106">Use the `REFERENCE ASSEMBLY` statement to enable Python extensions for the U-SQL Script</span></span>
* <span data-ttu-id="2b633-107">Használja a `REDUCE` művelet, a kulcs a bemeneti adatok particionálása</span><span class="sxs-lookup"><span data-stu-id="2b633-107">Using the `REDUCE` operation to partition the input data on a key</span></span>
* <span data-ttu-id="2b633-108">A U-SQL Python-bővítmények közé tartozik egy beépített nyomáscsökkentő (`Extension.Python.Reducer`) futó Python kódját a nyomáscsökkentő rendelt minden csomópont</span><span class="sxs-lookup"><span data-stu-id="2b633-108">The Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned to the reducer</span></span>
* <span data-ttu-id="2b633-109">A U-SQL parancsfájl tartalmazza a beágyazott Python-kódot, amely rendelkezik a hívott függvény `usqlml_main` , amely fogad egy pandas DataFrame bemeneti adatként, és egy pandas DataFrame kimeneteként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2b633-109">The U-SQL script contains the embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

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
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="2b633-110">Hogyan integrálható a Python U-SQL</span><span class="sxs-lookup"><span data-stu-id="2b633-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="2b633-111">Adattípusok</span><span class="sxs-lookup"><span data-stu-id="2b633-111">Datatypes</span></span>

* <span data-ttu-id="2b633-112">Konvertálja a karakterláncot és a numerikus oszlopot a U-SQL-Pandas és a U-SQL között</span><span class="sxs-lookup"><span data-stu-id="2b633-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="2b633-113">U-SQL nullák alakítja a Pandas érkező vagy oda irányuló `NA` értékek</span><span class="sxs-lookup"><span data-stu-id="2b633-113">U-SQL Nulls are converted to and from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="2b633-114">Sémák</span><span class="sxs-lookup"><span data-stu-id="2b633-114">Schemas</span></span>

* <span data-ttu-id="2b633-115">A Pandas index vektorok nem támogatottak a U-SQL.</span><span class="sxs-lookup"><span data-stu-id="2b633-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="2b633-116">A Python függvény összes bemeneti adatkeretek mindig rendelkezik egy 64 bites numerikus index 0 és mínusz 1 sorok száma.</span><span class="sxs-lookup"><span data-stu-id="2b633-116">All input data frames in the Python function always have a 64-bit numerical index from 0 through the number of rows minus 1.</span></span> 
* <span data-ttu-id="2b633-117">U-SQL adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="2b633-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="2b633-118">U-SQL adatkészletek oszlopnevek, amelyek nem karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2b633-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="2b633-119">Python-verziók</span><span class="sxs-lookup"><span data-stu-id="2b633-119">Python Versions</span></span>
<span data-ttu-id="2b633-120">Csak a Python 3.5.1 (Windows fordított) használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="2b633-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="2b633-121">Python-modulok</span><span class="sxs-lookup"><span data-stu-id="2b633-121">Standard Python modules</span></span>
<span data-ttu-id="2b633-122">Összes a Python modulban találhatók.</span><span class="sxs-lookup"><span data-stu-id="2b633-122">All the standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="2b633-123">További Python-modulok.</span><span class="sxs-lookup"><span data-stu-id="2b633-123">Additional Python modules</span></span>
<span data-ttu-id="2b633-124">A szabványos Python-függvénytárak mellett számos általánosan használt python-könyvtárak jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="2b633-124">Besides the standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="2b633-125">Kivétel üzenetek</span><span class="sxs-lookup"><span data-stu-id="2b633-125">Exception Messages</span></span>
<span data-ttu-id="2b633-126">Jelenleg a Python kódban kivétel jeleníti meg hibaként általános csúcspont.</span><span class="sxs-lookup"><span data-stu-id="2b633-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="2b633-127">A jövőben a U-SQL-feladatot hibaüzenetek a Python kivétel üzenetet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="2b633-127">In the future, the U-SQL Job error messages will display the Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="2b633-128">Bemeneti és kimeneti méretkorlátai</span><span class="sxs-lookup"><span data-stu-id="2b633-128">Input and Output size limitations</span></span>
<span data-ttu-id="2b633-129">Minden csomópont csak korlátozott mennyiségű memória rendelve van.</span><span class="sxs-lookup"><span data-stu-id="2b633-129">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="2b633-130">Ezt a határértéket jelenleg egy AU 6 GB-ot.</span><span class="sxs-lookup"><span data-stu-id="2b633-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="2b633-131">A bemeneti és kimeneti DataFrames már léteznie kell a Python kódját a memória, mert a bemeneti és kimeneti teljes mérete nem haladhatja meg a 6 GB.</span><span class="sxs-lookup"><span data-stu-id="2b633-131">Because the input and output DataFrames must exist in memory in the Python code, the total size for the input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="2b633-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2b633-132">See also</span></span>
* [<span data-ttu-id="2b633-133">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="2b633-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="2b633-134">U-SQL-parancsfájlok fejlesztése a Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="2b633-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="2b633-135">U-SQL ablak függvények használata az Azure Data Lake Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="2b633-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

