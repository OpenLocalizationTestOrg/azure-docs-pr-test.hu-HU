---
title: "az SQL Server virtuális gépet az Azure aaaExplore adatok |} Microsoft Docs"
description: "Hogyan tooexplore Azure SQL Server virtuális gépen tárolt adatokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="097bd-103">Az SQL Server virtuális gépen tárolt adatok megismerése az Azure rendszerben</span><span class="sxs-lookup"><span data-stu-id="097bd-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="097bd-104">Ez a dokumentum hogyan tooexplore Azure SQL Server virtuális gépen tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="097bd-104">This document covers how tooexplore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="097bd-105">Ezt adatok wrangling SQL használatával, és Python hasonló programozási nyelv használatával is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="097bd-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="097bd-106">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics.</span><span class="sxs-lookup"><span data-stu-id="097bd-106">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="097bd-107">Ez a feladat Ez a lépés hello Cortana Analytics folyamat (CAP).</span><span class="sxs-lookup"><span data-stu-id="097bd-107">This task is a step in hello Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="097bd-108">Ez a dokumentum hello minta SQL-utasítások feltételezik, hogy adatokat az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="097bd-108">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="097bd-109">Ha nem, olvassa el az toohello felhő adatok tudományos folyamat térkép toolearn hogyan toomove az adatok tooSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="097bd-109">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="097bd-110"><a name="sql-dataexploration"></a>Az SQL-parancsfájlok SQL adatokba</span><span class="sxs-lookup"><span data-stu-id="097bd-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="097bd-111">Íme néhány példa SQL-parancsfájlok, amely az SQL Server használt tooexplore adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="097bd-111">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

1. <span data-ttu-id="097bd-112">Napi megfigyeléseket hello számbavétele</span><span class="sxs-lookup"><span data-stu-id="097bd-112">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="097bd-113">Hello szintek kategorikus oszlopban beolvasása</span><span class="sxs-lookup"><span data-stu-id="097bd-113">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="097bd-114">Hello szintek száma kombinációja kategorikus kétoszlopos lekérése</span><span class="sxs-lookup"><span data-stu-id="097bd-114">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="097bd-115">Hello terjesztési numerikus oszlopok az beszerzése</span><span class="sxs-lookup"><span data-stu-id="097bd-115">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="097bd-116">Gyakorlati például használhatja hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.</span><span class="sxs-lookup"><span data-stu-id="097bd-116">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="097bd-117"><a name="python"></a>Python SQL adatokba</span><span class="sxs-lookup"><span data-stu-id="097bd-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="097bd-118">Python tooexplore adatokkal, és generáljon szolgáltatások Ha hello adatok SQL Server olyan hasonló tooprocessing adatok pythonos környezetekben, az Azure BLOB dokumentált módon [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="097bd-118">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="097bd-119">hello adatok hello adatbázisból betölti a pandas DataFrame toobe kell, és majd dolgozhatók további.</span><span class="sxs-lookup"><span data-stu-id="097bd-119">hello data needs toobe loaded from hello database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="097bd-120">Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello DataFrame ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="097bd-120">We document hello process of connecting toohello database and loading hello data into hello DataFrame in this section.</span></span>

<span data-ttu-id="097bd-121">a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, a felhasználónevet és jelszót az adott értékek) használatával lehet:</span><span class="sxs-lookup"><span data-stu-id="097bd-121">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="097bd-122">Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz.</span><span class="sxs-lookup"><span data-stu-id="097bd-122">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="097bd-123">hello alábbira olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:</span><span class="sxs-lookup"><span data-stu-id="097bd-123">hello following code reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="097bd-124">Most is dolgozhat hello Pandas DataFrame hello a témakörben ismertetett módon [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="097bd-124">Now you can work with hello Pandas DataFrame as covered in hello topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="097bd-125">A művelet példa Cortana Analytics folyamat</span><span class="sxs-lookup"><span data-stu-id="097bd-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="097bd-126">Hello Cortana Analytics folyamat egy nyilvános adatkészlet-végpontok közötti bemutató példa: [Team adatok tudományos folyamat hello működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="097bd-126">For an end-to-end walkthrough example of hello Cortana Analytics Process using a public dataset, see [hello Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

