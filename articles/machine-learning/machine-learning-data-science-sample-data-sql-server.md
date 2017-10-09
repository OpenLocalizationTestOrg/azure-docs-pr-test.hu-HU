---
title: az Azure SQL Server adatainak aaaSample |} Microsoft Docs
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
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="3d156-103"><a name="heading"></a>Mintaadatok az SQL Server az Azure-on</span><span class="sxs-lookup"><span data-stu-id="3d156-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="3d156-104">Ez a dokumentum bemutatja, hogyan toosample adatok SQL Server az Azure-on tárolt SQL vagy hello Python programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="3d156-104">This document shows how toosample data stored in SQL Server on Azure using either SQL or hello Python programming language.</span></span> <span data-ttu-id="3d156-105">Azt is bemutatja, hogyan toomove összehasonlítást az adatminta az Azure Machine Learning úgy, hogy elmenti azt tooa fájl feltöltése az Azure blob tooan, és olvassa Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3d156-105">It also shows how toomove sampled data into Azure Machine Learning by saving it tooa file, uploading it tooan Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="3d156-106">hello Python mintavételi használ hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC könyvtár tooconnect tooSQL Azure és a hello Server [Pandas](http://pandas.pydata.org/) könyvtár toodo hello mintavételi.</span><span class="sxs-lookup"><span data-stu-id="3d156-106">hello Python sampling uses hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC library tooconnect tooSQL Server on Azure and hello [Pandas](http://pandas.pydata.org/) library toodo hello sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="3d156-107">SQL mintakód hello ebben a dokumentumban azt feltételezi, hogy egy SQL Server Azure van adatok hello.</span><span class="sxs-lookup"><span data-stu-id="3d156-107">hello sample SQL code in this document assumes that hello data is in a SQL Server on Azure.</span></span> <span data-ttu-id="3d156-108">Ha nem, olvassa el túl[adatok tooSQL kiszolgáló áthelyezése az Azure-on](machine-learning-data-science-move-sql-server-virtual-machine.md) témakör útmutatást toomove az adatok tooSQL Azure-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3d156-108">If it is not, please refer too[Move data tooSQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how toomove your data tooSQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="3d156-109">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik tootopics hogyan toosample adatait tároló különböző környezetekben.</span><span class="sxs-lookup"><span data-stu-id="3d156-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="3d156-110">**Miért érdemes az az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="3d156-110">**Why sample your data?**</span></span>
<span data-ttu-id="3d156-111">Ha azt tervezi, hogy tooanalyze hello adatkészlet túl nagy, a rendszer általában egy jó ötlet toodown-minta hello adatok tooreduce azt tooa kisebb, de reprezentatív és könnyebben kezelhető méretét.</span><span class="sxs-lookup"><span data-stu-id="3d156-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="3d156-112">Ez lehetővé teszi az adatok ismertetése, feltárása és a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="3d156-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="3d156-113">Szerepét a hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) tooenable gyors prototípusának hello adatfeldolgozási funkciók és a gépi tanulási modellek van.</span><span class="sxs-lookup"><span data-stu-id="3d156-113">Its role in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="3d156-114">Ez a mintavételi feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="3d156-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="3d156-115"><a name="SQL"></a>SQL használatával</span><span class="sxs-lookup"><span data-stu-id="3d156-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="3d156-116">Ez a szakasz hello adatbázis SQL tooperform egyszerű véletlenszerű mintavétel hello adatok alapján használatával több módszerét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="3d156-116">This section describes several methods using SQL tooperform simple random sampling against hello data in hello database.</span></span> <span data-ttu-id="3d156-117">Válassza ki az adatok mérete és a kiosztás alapuló módszer.</span><span class="sxs-lookup"><span data-stu-id="3d156-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="3d156-118">az alábbi két elemek hello jelennek meg, hogyan toouse newid az SQL Server tooperform hello mintavételi.</span><span class="sxs-lookup"><span data-stu-id="3d156-118">hello two items below show how toouse newid in SQL Server tooperform hello sampling.</span></span> <span data-ttu-id="3d156-119">hello módszertől függ hogyan véletlenszerű szeretnénk hello minta toobe (az alábbi hello mintakód pk_id feltételezett toobe egy automatikusan létrehozott elsődleges kulcs).</span><span class="sxs-lookup"><span data-stu-id="3d156-119">hello method you choose depends on how random you want hello sample toobe (pk_id in hello sample code below is assumed toobe an auto-generated primary key).</span></span>

1. <span data-ttu-id="3d156-120">Kevésbé szigorú véletlenszerű minta</span><span class="sxs-lookup"><span data-stu-id="3d156-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="3d156-121">Több véletlenszerű minta</span><span class="sxs-lookup"><span data-stu-id="3d156-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="3d156-122">Tablesample is lehet mintavételek, valamint alábbi.</span><span class="sxs-lookup"><span data-stu-id="3d156-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="3d156-123">Ez lehet jobb megközelítés az adatok mérete nagy esetén (feltéve, hogy az adatok különböző oldalain nem korrelált) és a hello lekérdezés toocomplete elfogadható időn belül.</span><span class="sxs-lookup"><span data-stu-id="3d156-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for hello query toocomplete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="3d156-124">Vizsgálatát, és a mintaadatokat egy új tábla elhelyezésével szolgáltatások generálása</span><span class="sxs-lookup"><span data-stu-id="3d156-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="3d156-125"><a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="3d156-125"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="3d156-126">Hello mintalekérdezések fent közvetlenül használható hello Azure Machine Learning [és adatokat importálhat] [ import-data] modul toodown-minta hello adatok hello keresnie, és vonja az Azure Machine Learning kísérlet.</span><span class="sxs-lookup"><span data-stu-id="3d156-126">You can directly  use hello sample queries above in hello Azure Machine Learning [Import Data][import-data] module toodown-sample hello data on hello fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="3d156-127">Alább látható képernyőfelvétel hello olvasó modul tooread mintát hello adatok használatával:</span><span class="sxs-lookup"><span data-stu-id="3d156-127">A screen shot of using hello reader module tooread hello sampled data is shown below:</span></span>

![olvasó sql][1]

## <span data-ttu-id="3d156-129"><a name="python"></a>Hello Python programozási nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="3d156-129"><a name="python"></a>Using hello Python programming language</span></span>
<span data-ttu-id="3d156-130">Ez a szakasz azt mutatja be, hello segítségével [pyodbc könyvtár](https://code.google.com/p/pyodbc/) tooestablish az ODBC-csatlakozás tooa SQL server-adatbázis a Python.</span><span class="sxs-lookup"><span data-stu-id="3d156-130">This section demonstrates using hello [pyodbc library](https://code.google.com/p/pyodbc/) tooestablish an ODBC connect tooa SQL server database in Python.</span></span> <span data-ttu-id="3d156-131">hello adatbázis-kapcsolati karakterláncot a következőképpen történik: (cserélje kiszolgálónév, dbname, felhasználónevet és jelszót a konfiguráció):</span><span class="sxs-lookup"><span data-stu-id="3d156-131">hello database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="3d156-132">Hello [Pandas](http://pandas.pydata.org/) a Python kódtár adatkezelési Python programozási széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket biztosít.</span><span class="sxs-lookup"><span data-stu-id="3d156-132">hello [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="3d156-133">hello kódot hello adatok 0,1 % minta beolvassa az Azure SQL-adatbázis egy táblából egy Pandas adatokat:</span><span class="sxs-lookup"><span data-stu-id="3d156-133">hello code below reads a 0.1% sample of hello data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="3d156-134">Most már hello Pandas adatok keretében mintát hello adatokkal dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="3d156-134">You can now work with hello sampled data in hello Pandas data frame.</span></span> 

### <span data-ttu-id="3d156-135"><a name="python-aml"></a>Csatlakozás tooAzure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="3d156-135"><a name="python-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="3d156-136">A következő kód toosave hello le mintát tooa mintaadatfájlokat hello használja, és töltse fel az Azure blob tooan.</span><span class="sxs-lookup"><span data-stu-id="3d156-136">You can use hello following sample code toosave hello down-sampled data tooa file and upload it tooan Azure blob.</span></span> <span data-ttu-id="3d156-137">hello adatokat hello BLOB közvetlenül elolvashatja az Azure Machine Learning kísérlet hello segítségével [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="3d156-137">hello data in hello blob can be directly read into an Azure Machine Learning Experiment using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="3d156-138">hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="3d156-138">hello steps are as follows:</span></span> 

1. <span data-ttu-id="3d156-139">Hello pandas adatok keret tooa helyi fájl írása</span><span class="sxs-lookup"><span data-stu-id="3d156-139">Write hello pandas data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="3d156-140">Helyi fájl tooAzure blob feltöltése</span><span class="sxs-lookup"><span data-stu-id="3d156-140">Upload local file tooAzure blob</span></span>
   
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
3. <span data-ttu-id="3d156-141">Adatokat olvasni az Azure Machine Learning segítségével az Azure blob [és adatokat importálhat] [ import-data] modul látható hello képernyő fogd az alábbi módon:</span><span class="sxs-lookup"><span data-stu-id="3d156-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in hello screen grab below:</span></span>

![olvasó blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a><span data-ttu-id="3d156-143">hello Team adatok tudományos folyamat művelet példa</span><span class="sxs-lookup"><span data-stu-id="3d156-143">hello Team Data Science Process in Action example</span></span>
<span data-ttu-id="3d156-144">Hello Team adatok tudományos folyamat végpont bemutató példa használata a nyilvános dataset, lásd: [Team adatok tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3d156-144">For an end-to-end walkthrough example of hello Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
