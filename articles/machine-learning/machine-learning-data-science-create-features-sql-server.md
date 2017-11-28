---
title: "Az adatok funkciók létrehozása az SQL Server SQL és Python |} Microsoft Docs"
description: "Folyamat adatokat az SQL Azure-ból"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: f0ac2799e2d8f18b2dd5b633555bfca08a44ba27
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a><span data-ttu-id="6a2c3-103">Funkciók létrehozása az adatokhoz az SQL Serveren SQL és Python használatával</span><span class="sxs-lookup"><span data-stu-id="6a2c3-103">Create features for data in SQL Server using SQL and Python</span></span>
<span data-ttu-id="6a2c3-104">Ez a dokumentum bemutatja, hogyan létrehozni az SQL Server virtuális gép az Azure-on tárolt adatok, amelyek segítenek az adatokból hatékonyabban további algoritmusok szolgáltatásai.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-104">This document shows how to generate features for data stored in a SQL Server VM on Azure that help algorithms learn more efficiently from the data.</span></span> <span data-ttu-id="6a2c3-105">Ez végezhető SQL vagy Python, amelyek mindegyikét egy itt hasonló programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-105">This can be done by using SQL or by using a programming language like Python, both of which are demonstrated here.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="6a2c3-106">Ez **menü** szolgáltatások adatok létrehozása a különböző környezetek leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-106">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="6a2c3-107">Ez a feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-107">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

> [!NOTE]
> <span data-ttu-id="6a2c3-108">Gyakorlati például részleteket a [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-108">For a practical example, you can consult the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="6a2c3-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6a2c3-109">Prerequisites</span></span>
<span data-ttu-id="6a2c3-110">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-110">This article assumes that you have:</span></span>

* <span data-ttu-id="6a2c3-111">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-111">Created an Azure storage account.</span></span> <span data-ttu-id="6a2c3-112">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="6a2c3-112">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="6a2c3-113">SQL Server adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-113">Stored your data in SQL Server.</span></span> <span data-ttu-id="6a2c3-114">Ha nem, olvassa el [adatok áthelyezése az Azure SQL Database az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) útmutatást nem helyezi át az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-114">If you have not, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) for instructions on how to move the data there.</span></span>

## <span data-ttu-id="6a2c3-115"><a name="sql-featuregen"></a>Az SQL szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a2c3-115"><a name="sql-featuregen"></a>Feature Generation with SQL</span></span>
<span data-ttu-id="6a2c3-116">Ez a szakasz azt módokat SQL funkciók generálása mutatják be:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-116">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="6a2c3-117">Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a2c3-117">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="6a2c3-118">A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a2c3-118">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="6a2c3-119">A szolgáltatások csak egy oszlop működés közbeni</span><span class="sxs-lookup"><span data-stu-id="6a2c3-119">Rolling out the features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="6a2c3-120">Létrehozhat további szolgáltatásokat, ha oszlopokként vegye fel a meglévő táblázat, vagy hozzon létre egy új táblázat további funkciók és elsődleges kulcs, az eredeti táblázatban lehetne illeszteni.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-120">Once you generate additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, that can be joined with the original table.</span></span>
> 
> 

### <span data-ttu-id="6a2c3-121"><a name="sql-countfeature"></a>Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a2c3-121"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="6a2c3-122">Ez a dokumentum bemutatja kétféleképpen száma szolgáltatások generálására.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-122">This document demonstrates two ways of generating count features.</span></span> <span data-ttu-id="6a2c3-123">Az első módszer Feltételes összegzés pedig a második metódust használja a "where" záradék.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-123">The first method uses conditional sum and the second method uses the 'where\` clause.</span></span> <span data-ttu-id="6a2c3-124">Ezek ezután össze lehet kapcsolni és az eredeti tábla (elsődleges kulcs oszlopok) mellett az eredeti adatok száma szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-124">These can then be joined with the original table (using primary key columns) to have count features alongside the original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <span data-ttu-id="6a2c3-125"><a name="sql-binningfeature"></a>A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a2c3-125"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="6a2c3-126">A következő példa bemutatja, hogyan által (5 bins használatával) a dobozolás binned szolgáltatások létrehozásához egy numerikus oszlopot, amely a ehelyett szolgáltatásként használható:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-126">The following example shows how to generate binned features by binning (using 5 bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="6a2c3-127"><a name="sql-featurerollout"></a>A szolgáltatások csak egy oszlop működés közbeni</span><span class="sxs-lookup"><span data-stu-id="6a2c3-127"><a name="sql-featurerollout"></a>Rolling out the features from a single column</span></span>
<span data-ttu-id="6a2c3-128">Ebben a szakaszban a bemutatjuk, hogyan arra csak egy oszlop a tábla létrehozásához további szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-128">In this section, we demonstrate how to roll-out a single column in a table to generate additional features.</span></span> <span data-ttu-id="6a2c3-129">A példa feltételezi, hogy nincs-e a szélességi és hosszúsági oszlop a tábla, amelyből szolgáltatások létrehozni kívánt.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-129">The example assumes that there is a latitude or longitude column in the table from which you are trying to generate features.</span></span>

<span data-ttu-id="6a2c3-130">Íme egy rövid ismertetése a szélesség/hosszúság helyadatok (a stackoverflow forrásokat `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-130">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span></span> <span data-ttu-id="6a2c3-131">Ez az hogy featurizing előtt tájékozódjon a location mező:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-131">This is useful to understand before featurizing the location field:</span></span>

* <span data-ttu-id="6a2c3-132">A bejelentkezési be van állítva, hogy dolgozunk Észak vagy Dél, keleti vagy nyugati a világ.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-132">The sign tells us whether we are north or south, east or west on the globe.</span></span>
* <span data-ttu-id="6a2c3-133">Egy nem nulla több száz számjegy közli a gép hosszúsági és szélességi nem használunk!</span><span class="sxs-lookup"><span data-stu-id="6a2c3-133">A nonzero hundreds digit tells us we're using longitude, not latitude!</span></span>
* <span data-ttu-id="6a2c3-134">A több számjegyet körülbelül 1000 kilométerben helyzetben biztosít.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-134">The tens digit gives a position to about 1,000 kilometers.</span></span> <span data-ttu-id="6a2c3-135">Milyen kontinensen vagy óceáni dolgozunk a hasznos információkat nyújt nekünk.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-135">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="6a2c3-136">Az egységek számjegy (egy decimális fok) biztosít egy helyen legfeljebb 111 kilométerben (60 tengeri miles, körülbelül 69 miles).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-136">The units digit (one decimal degree) gives a position up to 111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="6a2c3-137">Azt is adja meg, nagyjából milyen nagy állapot vagy a dolgozunk ország.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-137">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="6a2c3-138">Akár 11.1 km-érdemes van tizedes jegyre: azt is különbözteti meg a szomszédos nagy város egy nagy város pozícióját.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-138">The first decimal place is worth up to 11.1 km: it can distinguish the position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="6a2c3-139">A második tizedes van akár az 1.1-es km-érdemes: azt is egy falu eltérő, külön a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-139">The second decimal place is worth up to 1.1 km: it can separate one village from the next.</span></span>
* <span data-ttu-id="6a2c3-140">A harmadik tizedes ér legfeljebb 110 m: nagy mezőgazdasági mező vagy intézményi egyetemi is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-140">The third decimal place is worth up to 110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="6a2c3-141">A negyedik tizedes ér legfeljebb 11 m: egy is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-141">The fourth decimal place is worth up to 11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="6a2c3-142">Már nem zavarja a nem javított GPS egység tipikus pontosságának összehasonlítható.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-142">It is comparable to the typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="6a2c3-143">Az ötödik tizedes ér legfeljebb 1.1 m: azt fák megkülönböztetni egymástól.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-143">The fifth decimal place is worth up to 1.1 m: it distinguish trees from each other.</span></span> <span data-ttu-id="6a2c3-144">A kereskedelmi GPS-egységekhez szintre pontossága különbözeti helyesbítéssel, csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-144">Accuracy to this level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="6a2c3-145">A hatodik tizedes ér értéke legfeljebb 0,11 m: is használhatja ezt a részletes tájak, tervezéséhez struktúrák elrendezése utak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-145">The sixth decimal place is worth up to 0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="6a2c3-146">Több mint elég jó glaciers és folyókat követési kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-146">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="6a2c3-147">Ez megvalósítható a GPS, például a differentially javított GPS painstaking intézkedéseket.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-147">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="6a2c3-148">A Tartózkodásihely-adatok is lehet featurized következőképpen, régió, a hely és a várost elválasztó.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-148">The location information can can be featurized as follows, separating out region, location and city information.</span></span> <span data-ttu-id="6a2c3-149">Vegye figyelembe, hogy a többször is meghívhatja a REST-végpont például a Bing térképek API érhető el `https://msdn.microsoft.com/library/ff701710.aspx` a régió/körzeti adatainak megszerzése.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-149">Note that once can also call a REST end point such as Bing Maps API available at `https://msdn.microsoft.com/library/ff701710.aspx` to get the region/district information.</span></span>

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

<span data-ttu-id="6a2c3-150">A fenti helyét alapján szolgáltatása további használható további száma szolgáltatások létrehozásához, a fentebb leírt módon.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-150">The above location based features can be further used to generate additional count features as described earlier.</span></span>

> [!TIP]
> <span data-ttu-id="6a2c3-151">A választott nyelv a rekordok programozott módon is beszúrhat.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-151">You can programmatically insert the records using your language of choice.</span></span> <span data-ttu-id="6a2c3-152">Helyezze be az adatok írási hatékonyság növelése érdekében adattömbök szeretne [látogasson el a példa bemutatja, hogyan ehhez itt használatával pyodbc](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-152">You may need to insert the data in chunks to improve write efficiency [Check out the example of how to do this using pyodbc here](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span></span>
> <span data-ttu-id="6a2c3-153">Egy másik lehetőség, hogy az adatbázis használatával adatokat beszúrni [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx)</span><span class="sxs-lookup"><span data-stu-id="6a2c3-153">Another alternative is to insert data in the database using [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx)</span></span>
> 
> 

### <span data-ttu-id="6a2c3-154"><a name="sql-aml"></a>Csatlakozás az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="6a2c3-154"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="6a2c3-155">Az újonnan létrehozott szolgáltatás fel van véve egy oszlop a meglévő tábla vagy tárolható egy új tábla, és az eredeti táblázatban a machine Learning szolgáltatáshoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-155">The newly generated feature can be added as a column to an existing table or stored in a new table and joined with the original table for machine learning.</span></span> <span data-ttu-id="6a2c3-156">Szolgáltatások jön létre, vagy használatával érhető el, ha már létrehozott, a [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul Azure ml alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-156">Features can be generated or accessed if already created, using the [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module in Azure ML as shown below:</span></span>

![az azureml-olvasók](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <span data-ttu-id="6a2c3-158"><a name="python"></a>Például a Python-programozási nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="6a2c3-158"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="6a2c3-159">Python szolgáltatások létrehozásához, amikor az adatok az SQL Server használata hasonló adatfeldolgozás dokumentált Python használatával az Azure BLOB [Azure Blobadatok folyamat akkor adatok tudományos környezetben](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-159">Using Python to generate features when the data is in SQL Server is similar to processing data in Azure blob using Python as documented in [Process Azure Blob data in you data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="6a2c3-160">Az adatok betöltve kell lennie az adatbázisból pandas adatok keretbe, és majd dolgozhatók további.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-160">The data needs to be loaded from the database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="6a2c3-161">A folyamat az adatbázishoz csatlakozással, és az adatok betöltését az adatok keret ebben a szakaszban a dokumentum azt.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-161">We document the process of connecting to the database and loading the data into the data frame in this section.</span></span>

<span data-ttu-id="6a2c3-162">A következő kapcsolati karakterlánc-formátum használható Python pyodbc (csere kiszolgálónév, dbname, felhasználónevet és jelszót az adott értékek) használatával az SQL Server-adatbázis csatlakozni:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-162">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="6a2c3-163">A [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6a2c3-163">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="6a2c3-164">Az alábbi kódot beolvassa az eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:</span><span class="sxs-lookup"><span data-stu-id="6a2c3-164">The code below reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="6a2c3-165">Most a témakörök kiterjed a Pandas adatok keret dolgozhat [létrehozása az Azure blob storage adatok Panda szolgáltatásai](machine-learning-data-science-create-features-blob.md).</span><span class="sxs-lookup"><span data-stu-id="6a2c3-165">Now you can work with the Pandas data frame as covered in topics [Create features for Azure blob storage data using Panda](machine-learning-data-science-create-features-blob.md).</span></span>

