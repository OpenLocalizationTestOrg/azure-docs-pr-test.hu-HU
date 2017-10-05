---
title: Az adatokat az SQL Server az Azure-on |} Microsoft Docs
description: A mintaadatok az SQL Server az Azure-on
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 1bdcc7175dac325de1144d805e977264524b3fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="649a2-103"><a name="heading"></a>Mintaadatok az SQL Server az Azure-on</span><span class="sxs-lookup"><span data-stu-id="649a2-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="649a2-104">Ez a dokumentum bemutatja, hogyan az SQL Server az Azure-on tárolt adatokat SQL vagy a Python programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="649a2-104">This document shows how to sample data stored in SQL Server on Azure using either SQL or the Python programming language.</span></span> <span data-ttu-id="649a2-105">Azt is bemutatja, hogyan mintaadatokat áthelyezi az Azure Machine Learning fájlba menti, feltölti az Azure blob, és olvassa Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="649a2-105">It also shows how to move sampled data into Azure Machine Learning by saving it to a file, uploading it to an Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="649a2-106">A Python mintavételi használatát a [pyodbc](https://code.google.com/p/pyodbc/) ODBC könyvtár az Azure SQL-kiszolgálóhoz való csatlakozáshoz és a [Pandas](http://pandas.pydata.org/) könyvtár tennie, hogy a mintavétel.</span><span class="sxs-lookup"><span data-stu-id="649a2-106">The Python sampling uses the [pyodbc](https://code.google.com/p/pyodbc/) ODBC library to connect to SQL Server on Azure and the [Pandas](http://pandas.pydata.org/) library to do the sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="649a2-107">Ebben a dokumentumban SQL példakód azt feltételezi, hogy az adatok egy SQL Server az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="649a2-107">The sample SQL code in this document assumes that the data is in a SQL Server on Azure.</span></span> <span data-ttu-id="649a2-108">Ha nem, olvassa el [adatok áthelyezése az SQL Server Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) témakör útmutatást az adatok áthelyezése az SQL Server az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="649a2-108">If it is not, please refer to [Move data to SQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how to move your data to SQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="649a2-109">A következő **menü** az adatokat a különböző tárolási környezetekben módját leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="649a2-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="649a2-110">**Miért érdemes az az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="649a2-110">**Why sample your data?**</span></span>
<span data-ttu-id="649a2-111">Ha azt tervezi, hogy elemezheti az adatkészlet túl nagy, akkor általában down kétmintás az adatokat, hogy az kisebb, de reprezentatív és könnyebben kezelhető méretű jó ötlet.</span><span class="sxs-lookup"><span data-stu-id="649a2-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="649a2-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="649a2-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="649a2-113">Szerepét a a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) az adatok feldolgozása funkciók és a gépi tanulási modellek gyors prototípusának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="649a2-113">Its role in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="649a2-114">Ez a mintavételi feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="649a2-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="649a2-115"><a name="SQL"></a>SQL használatával</span><span class="sxs-lookup"><span data-stu-id="649a2-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="649a2-116">Ez a szakasz az adatok alapján egyszerű véletlenszerű mintavétel végrehajtásához az adatbázisban az SQL több módszerét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="649a2-116">This section describes several methods using SQL to perform simple random sampling against the data in the database.</span></span> <span data-ttu-id="649a2-117">Válassza ki az adatok mérete és a kiosztás alapuló módszer.</span><span class="sxs-lookup"><span data-stu-id="649a2-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="649a2-118">Az alábbi két elemek használatát mutatják be newid az SQL Server a mintavételi végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="649a2-118">The two items below show how to use newid in SQL Server to perform the sampling.</span></span> <span data-ttu-id="649a2-119">Módszertől függ a minta szeretnénk hogyan véletlenszerű (az alábbi példakód pk_id feltételezett, hogy egy automatikusan létrehozott elsődleges kulcsot kell).</span><span class="sxs-lookup"><span data-stu-id="649a2-119">The method you choose depends on how random you want the sample to be (pk_id in the sample code below is assumed to be an auto-generated primary key).</span></span>

1. <span data-ttu-id="649a2-120">Kevésbé szigorú véletlenszerű minta</span><span class="sxs-lookup"><span data-stu-id="649a2-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="649a2-121">Több véletlenszerű minta</span><span class="sxs-lookup"><span data-stu-id="649a2-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="649a2-122">Tablesample is lehet mintavételek, valamint alábbi.</span><span class="sxs-lookup"><span data-stu-id="649a2-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="649a2-123">Jobb megközelítés erre akkor lehet, ha az adatok mérete nagy (feltéve, hogy az adatok különböző oldalain nem korrelált), és a lekérdezés elfogadható időn belül végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="649a2-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for the query to complete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="649a2-124">Vizsgálatát, és a mintaadatokat egy új tábla elhelyezésével szolgáltatások generálása</span><span class="sxs-lookup"><span data-stu-id="649a2-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="649a2-125"><a name="sql-aml"></a>Csatlakozás az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="649a2-125"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="649a2-126">A fenti mintalekérdezések közvetlenül használható az Azure Machine Learning [és adatokat importálhat] [ import-data] lefelé-minta menet közben az adatok és az érdekében, hogy az Azure Machine Learning kísérlet a modult.</span><span class="sxs-lookup"><span data-stu-id="649a2-126">You can directly  use the sample queries above in the Azure Machine Learning [Import Data][import-data] module to down-sample the data on the fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="649a2-127">Alább látható képernyőfelvétel a mintában szereplő adatokat olvasni az olvasó modullal:</span><span class="sxs-lookup"><span data-stu-id="649a2-127">A screen shot of using the reader module to read the sampled data is shown below:</span></span>

![olvasó sql][1]

## <span data-ttu-id="649a2-129"><a name="python"></a>A Python programozási nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="649a2-129"><a name="python"></a>Using the Python programming language</span></span>
<span data-ttu-id="649a2-130">Ez a szakasz azt mutatja be, használja a [pyodbc könyvtár](https://code.google.com/p/pyodbc/) egy ODBC csatlakozni a Python egy SQL server-adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="649a2-130">This section demonstrates using the [pyodbc library](https://code.google.com/p/pyodbc/) to establish an ODBC connect to a SQL server database in Python.</span></span> <span data-ttu-id="649a2-131">Adatbázis-kapcsolati karakterláncot a következőképpen történik: (cserélje kiszolgálónév, dbname, felhasználónevet és jelszót a konfiguráció):</span><span class="sxs-lookup"><span data-stu-id="649a2-131">The database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="649a2-132">A [Pandas](http://pandas.pydata.org/) a Python kódtár adatkezelési Python programozási széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket biztosít.</span><span class="sxs-lookup"><span data-stu-id="649a2-132">The [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="649a2-133">Az alábbi kódot beolvassa az adatok 0,1 % minta az Azure SQL-adatbázis egy táblából egy Pandas adatokat:</span><span class="sxs-lookup"><span data-stu-id="649a2-133">The code below reads a 0.1% sample of the data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="649a2-134">Most már a Pandas adatok keretében a mintában szereplő adatokkal dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="649a2-134">You can now work with the sampled data in the Pandas data frame.</span></span> 

### <span data-ttu-id="649a2-135"><a name="python-aml"></a>Csatlakozás az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="649a2-135"><a name="python-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="649a2-136">Az alábbi példakód segítségével le mintát adatok mentése fájlba, és töltse fel az Azure-blobot.</span><span class="sxs-lookup"><span data-stu-id="649a2-136">You can use the following sample code to save the down-sampled data to a file and upload it to an Azure blob.</span></span> <span data-ttu-id="649a2-137">A blob adatait az Azure Machine Learning kísérlet közvetlenül olvasható használatával a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="649a2-137">The data in the blob can be directly read into an Azure Machine Learning Experiment using the [Import Data][import-data] module.</span></span> <span data-ttu-id="649a2-138">A lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="649a2-138">The steps are as follows:</span></span> 

1. <span data-ttu-id="649a2-139">A pandas adatok keret írni egy helyi fájlba</span><span class="sxs-lookup"><span data-stu-id="649a2-139">Write the pandas data frame to a local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="649a2-140">Helyi fájl feltöltése az Azure-blobba</span><span class="sxs-lookup"><span data-stu-id="649a2-140">Upload local file to Azure blob</span></span>
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. <span data-ttu-id="649a2-141">Az Azure Machine Learning segítségével az Azure blob-adatok olvasása [és adatokat importálhat] [ import-data] modul, ahogy az az alábbi képernyő fogd:</span><span class="sxs-lookup"><span data-stu-id="649a2-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in the screen grab below:</span></span>

![olvasó blob][2]

## <a name="the-team-data-science-process-in-action-example"></a><span data-ttu-id="649a2-143">A művelet a példában az Team tudományos folyamat</span><span class="sxs-lookup"><span data-stu-id="649a2-143">The Team Data Science Process in Action example</span></span>
<span data-ttu-id="649a2-144">A adatok tudományos folyamatának végpont forgatókönyv példa használata a nyilvános adatkészlet: [Team adatok tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="649a2-144">For an end-to-end walkthrough example of the Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
