---
title: "Áttekintheti az adatokat az SQL Server virtuális gép az Azure-on |} Microsoft Docs"
description: "Az SQL Server virtuális gép az Azure-on tárolt adatokba módjáról."
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
ms.openlocfilehash: a2be21ef15b9209db1e97150e0297558fa69a7be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="aab12-103">Az SQL Server virtuális gépen tárolt adatok megismerése az Azure rendszerben</span><span class="sxs-lookup"><span data-stu-id="aab12-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="aab12-104">Ez a dokumentum bemutatja, hogyan adhat az SQL Server virtuális gép az Azure-on tárolt adatokba.</span><span class="sxs-lookup"><span data-stu-id="aab12-104">This document covers how to explore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="aab12-105">Ezt adatok wrangling SQL használatával, és Python hasonló programozási nyelv használatával is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="aab12-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="aab12-106">A következő **menü** eszközök segítségével áttekintheti az különböző tárolási környezetekben adatokat leíró témakörökre mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aab12-106">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span> <span data-ttu-id="aab12-107">Ez a feladat egy lépés a Cortana Analytics folyamat (CAP).</span><span class="sxs-lookup"><span data-stu-id="aab12-107">This task is a step in the Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="aab12-108">Ebben a dokumentumban a minta SQL-utasítások feltételezik, hogy adatokat az SQL Server kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="aab12-108">The sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="aab12-109">Ha nem, nézze meg a felhő adatok tudományos folyamat leképezés megtudhatja, hogyan helyezze át az adatokat az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aab12-109">If it isn't, refer to the cloud data science process map to learn how to move your data to SQL Server.</span></span>
> 
> 

## <span data-ttu-id="aab12-110"><a name="sql-dataexploration"></a>Az SQL-parancsfájlok SQL adatokba</span><span class="sxs-lookup"><span data-stu-id="aab12-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="aab12-111">Íme néhány példa SQL-parancsfájlok, amelyek segítségével megismerheti az SQL Server adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="aab12-111">Here are a few sample SQL scripts that can be used to explore data stores in SQL Server.</span></span>

1. <span data-ttu-id="aab12-112">Napi megfigyeléseket szám</span><span class="sxs-lookup"><span data-stu-id="aab12-112">Get the count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="aab12-113">A szintek kategorikus oszlopban beolvasása</span><span class="sxs-lookup"><span data-stu-id="aab12-113">Get the levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="aab12-114">A szintek számának beolvasása a kombinációja kategorikus kétoszlopos</span><span class="sxs-lookup"><span data-stu-id="aab12-114">Get the number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="aab12-115">A numerikus oszlopok terjesztése beolvasása</span><span class="sxs-lookup"><span data-stu-id="aab12-115">Get the distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="aab12-116">Gyakorlati például használhatja a [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.</span><span class="sxs-lookup"><span data-stu-id="aab12-116">For a practical example, you can use the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="aab12-117"><a name="python"></a>Python SQL adatokba</span><span class="sxs-lookup"><span data-stu-id="aab12-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="aab12-118">Python adatokba és szolgáltatások létrehozása, amikor az adatok az SQL Server használata esetén hasonló adatfeldolgozás pythonos környezetekben, az Azure BLOB, ahogy [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="aab12-118">Using Python to explore data and generate features when the data is in SQL Server is similar to processing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="aab12-119">Az adatok betöltve kell lennie az adatbázisból történő egy pandas DataFrame, és majd dolgozhatók további.</span><span class="sxs-lookup"><span data-stu-id="aab12-119">The data needs to be loaded from the database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="aab12-120">Azt a dokumentum az adatbázishoz való kapcsolódás, illetve az adatok betöltését az ebben a szakaszban DataFrame folyamatán.</span><span class="sxs-lookup"><span data-stu-id="aab12-120">We document the process of connecting to the database and loading the data into the DataFrame in this section.</span></span>

<span data-ttu-id="aab12-121">A következő kapcsolati karakterlánc-formátum használható Python pyodbc (csere kiszolgálónév, dbname, a felhasználónevet és jelszót az adott értékek) használatával az SQL Server-adatbázis csatlakozni:</span><span class="sxs-lookup"><span data-stu-id="aab12-121">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="aab12-122">A [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz.</span><span class="sxs-lookup"><span data-stu-id="aab12-122">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="aab12-123">Az alábbi kód beolvassa az eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:</span><span class="sxs-lookup"><span data-stu-id="aab12-123">The following code reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="aab12-124">Most már használhatja a Pandas DataFrame, a témakörben tárgyalt [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="aab12-124">Now you can work with the Pandas DataFrame as covered in the topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="aab12-125">A művelet példa Cortana Analytics folyamat</span><span class="sxs-lookup"><span data-stu-id="aab12-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="aab12-126">A Cortana Analytics folyamat egy nyilvános adatkészlet-végpontok közötti forgatókönyv például, [a csapat adatok tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="aab12-126">For an end-to-end walkthrough example of the Cortana Analytics Process using a public dataset, see [The Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

