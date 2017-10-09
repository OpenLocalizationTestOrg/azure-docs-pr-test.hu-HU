---
title: "SQL és a Python, SQL Server adatainak aaaCreate szolgáltatások |} Microsoft Docs"
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
ms.openlocfilehash: 223352edb0377a159333e7528ad03c43902e6f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a><span data-ttu-id="a6d53-103">Funkciók létrehozása az adatokhoz az SQL Serveren SQL és Python használatával</span><span class="sxs-lookup"><span data-stu-id="a6d53-103">Create features for data in SQL Server using SQL and Python</span></span>
<span data-ttu-id="a6d53-104">Ez a dokumentum bemutatja, hogyan toogenerate szolgáltatásokat az SQL Server virtuális gép az Azure-on tárolt adatok, algoritmusok segítő további hatékonyabban hello adatokból.</span><span class="sxs-lookup"><span data-stu-id="a6d53-104">This document shows how toogenerate features for data stored in a SQL Server VM on Azure that help algorithms learn more efficiently from hello data.</span></span> <span data-ttu-id="a6d53-105">Ez végezhető SQL vagy Python, amelyek mindegyikét egy itt hasonló programozási nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="a6d53-105">This can be done by using SQL or by using a programming language like Python, both of which are demonstrated here.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="a6d53-106">Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics.</span><span class="sxs-lookup"><span data-stu-id="a6d53-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="a6d53-107">Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="a6d53-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

> [!NOTE]
> <span data-ttu-id="a6d53-108">Gyakorlati például további részleteket hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.</span><span class="sxs-lookup"><span data-stu-id="a6d53-108">For a practical example, you can consult hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a6d53-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6d53-109">Prerequisites</span></span>
<span data-ttu-id="a6d53-110">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a6d53-110">This article assumes that you have:</span></span>

* <span data-ttu-id="a6d53-111">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a6d53-111">Created an Azure storage account.</span></span> <span data-ttu-id="a6d53-112">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="a6d53-112">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="a6d53-113">SQL Server adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="a6d53-113">Stored your data in SQL Server.</span></span> <span data-ttu-id="a6d53-114">Ha nem, olvassa el [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) hogyan toomove hello hiba adatok kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="a6d53-114">If you have not, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) for instructions on how toomove hello data there.</span></span>

## <span data-ttu-id="a6d53-115"><a name="sql-featuregen"></a>Az SQL szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6d53-115"><a name="sql-featuregen"></a>Feature Generation with SQL</span></span>
<span data-ttu-id="a6d53-116">Ez a szakasz azt módokat SQL funkciók generálása mutatják be:</span><span class="sxs-lookup"><span data-stu-id="a6d53-116">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="a6d53-117">Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6d53-117">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="a6d53-118">A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6d53-118">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="a6d53-119">Működés közbeni hello szolgáltatások csak egy oszlop</span><span class="sxs-lookup"><span data-stu-id="a6d53-119">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="a6d53-120">Létrehozhat további szolgáltatásokat, ha oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, hello eredeti tábla lehetne illeszteni.</span><span class="sxs-lookup"><span data-stu-id="a6d53-120">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span>
> 
> 

### <span data-ttu-id="a6d53-121"><a name="sql-countfeature"></a>Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6d53-121"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="a6d53-122">Ez a dokumentum bemutatja kétféleképpen száma szolgáltatások generálására.</span><span class="sxs-lookup"><span data-stu-id="a6d53-122">This document demonstrates two ways of generating count features.</span></span> <span data-ttu-id="a6d53-123">hello első módszer Feltételes összegzés és hello második módszert használja a "where" záradék hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a6d53-123">hello first method uses conditional sum and hello second method uses hello 'where\` clause.</span></span> <span data-ttu-id="a6d53-124">Ezek ezután társíthatók hello eredeti (elsődleges kulcs oszlopokat használó) tábla toohave száma szolgáltatásokkal együtt hello eredeti adatok.</span><span class="sxs-lookup"><span data-stu-id="a6d53-124">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <span data-ttu-id="a6d53-125"><a name="sql-binningfeature"></a>A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6d53-125"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="a6d53-126">hello következő példa bemutatja, hogyan toogenerate binned szolgáltatások által a dobozolás (5 bins használatával) egy numerikus oszlopot, amely a ehelyett szolgáltatásként használható:</span><span class="sxs-lookup"><span data-stu-id="a6d53-126">hello following example shows how toogenerate binned features by binning (using 5 bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="a6d53-127"><a name="sql-featurerollout"></a>Működés közbeni hello szolgáltatások csak egy oszlop</span><span class="sxs-lookup"><span data-stu-id="a6d53-127"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="a6d53-128">Ebben a szakaszban a bemutatjuk, hogyan tooroll kibővített egyetlen oszlop a tábla toogenerate további szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="a6d53-128">In this section, we demonstrate how tooroll-out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="a6d53-129">hello példa feltételezi, hogy van-e a szélességi és hosszúsági oszlop, amelyből toogenerate szolgáltatások kívánt hello tábla.</span><span class="sxs-lookup"><span data-stu-id="a6d53-129">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="a6d53-130">Íme egy rövid ismertetése a szélesség/hosszúság helyadatok (a stackoverflow forrásokat `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span><span class="sxs-lookup"><span data-stu-id="a6d53-130">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span></span> <span data-ttu-id="a6d53-131">Ez az featurizing hello a location mező előtt hasznos toounderstand:</span><span class="sxs-lookup"><span data-stu-id="a6d53-131">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="a6d53-132">hello bejelentkezési közli nekünk e dolgozunk, déli vagy északi keleti vagy nyugati a hello földgolyó méretét.</span><span class="sxs-lookup"><span data-stu-id="a6d53-132">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="a6d53-133">Egy nem nulla több száz számjegy közli a gép hosszúsági és szélességi nem használunk!</span><span class="sxs-lookup"><span data-stu-id="a6d53-133">A nonzero hundreds digit tells us we're using longitude, not latitude!</span></span>
* <span data-ttu-id="a6d53-134">hello több számjegyet biztosít a pozíció tooabout 1000 kilométerben.</span><span class="sxs-lookup"><span data-stu-id="a6d53-134">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="a6d53-135">Milyen kontinensen vagy óceáni dolgozunk a hasznos információkat nyújt nekünk.</span><span class="sxs-lookup"><span data-stu-id="a6d53-135">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="a6d53-136">hello egységek számjegy (egy decimális fok) biztosít egy feljebb too111 kilométerben (60 tengeri miles, körülbelül 69 miles).</span><span class="sxs-lookup"><span data-stu-id="a6d53-136">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="a6d53-137">Azt is adja meg, nagyjából milyen nagy állapot vagy a dolgozunk ország.</span><span class="sxs-lookup"><span data-stu-id="a6d53-137">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="a6d53-138">hello tizedes jegyre érdemes működik-e too11.1 km: azt is különbözteti meg a szomszédos nagy város egy nagy város hello pozícióját.</span><span class="sxs-lookup"><span data-stu-id="a6d53-138">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="a6d53-139">hello második tizedes érdemes működik-e too1.1 km: azt is egy falu eltérő, külön hello mellett.</span><span class="sxs-lookup"><span data-stu-id="a6d53-139">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="a6d53-140">hello harmadik tizedes működik-érdemes e too110 m: nagy mezőgazdasági mező vagy intézményi egyetemi is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="a6d53-140">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="a6d53-141">hello negyedik tizedes működik-érdemes e too11 m: egy is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="a6d53-141">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="a6d53-142">Hasonló toohello tipikus pontossága nem zavarja a nem javított GPS egység.</span><span class="sxs-lookup"><span data-stu-id="a6d53-142">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="a6d53-143">hello ötödik tizedes működik-érdemes e too1.1 m: azt fák megkülönböztetni egymástól.</span><span class="sxs-lookup"><span data-stu-id="a6d53-143">hello fifth decimal place is worth up too1.1 m: it distinguish trees from each other.</span></span> <span data-ttu-id="a6d53-144">Pontosság toothis szintjét az kereskedelmi GPS-egységekhez csak különbözeti helyesbítéssel lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="a6d53-144">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="a6d53-145">hello hatodik tizedes érdemes működik-e too0.11 m: is használhatja ezt a részletes tájak, tervezéséhez struktúrák elrendezése utak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a6d53-145">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="a6d53-146">Több mint elég jó glaciers és folyókat követési kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6d53-146">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="a6d53-147">Ez megvalósítható a GPS, például a differentially javított GPS painstaking intézkedéseket.</span><span class="sxs-lookup"><span data-stu-id="a6d53-147">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="a6d53-148">hello helyére vonatkozó információkat is lehet featurized következőképpen, régió, a hely és a várost elválasztó.</span><span class="sxs-lookup"><span data-stu-id="a6d53-148">hello location information can can be featurized as follows, separating out region, location and city information.</span></span> <span data-ttu-id="a6d53-149">Vegye figyelembe, hogy a többször is meghívhatja a REST-végpont például a Bing térképek API érhető el `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello régió/körzeti információkat.</span><span class="sxs-lookup"><span data-stu-id="a6d53-149">Note that once can also call a REST end point such as Bing Maps API available at `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello region/district information.</span></span>

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

<span data-ttu-id="a6d53-150">hello fenti alapú szolgáltatások lehet további helyre toogenerate további száma funkciókat használ, a fentebb leírt módon.</span><span class="sxs-lookup"><span data-stu-id="a6d53-150">hello above location based features can be further used toogenerate additional count features as described earlier.</span></span>

> [!TIP]
> <span data-ttu-id="a6d53-151">Programozott módon szúrhat be a választott nyelven hello rögzíti.</span><span class="sxs-lookup"><span data-stu-id="a6d53-151">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="a6d53-152">Szükség lehet a tooinsert hello adatok adattömbök tooimprove írási hatékonyságát [hello példa bemutatja, hogyan kivétele toodo a pyodbc itt használatával](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span><span class="sxs-lookup"><span data-stu-id="a6d53-152">You may need tooinsert hello data in chunks tooimprove write efficiency [Check out hello example of how toodo this using pyodbc here](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span></span>
> <span data-ttu-id="a6d53-153">Egy másik lehetőség az tooinsert adatai hello adatbázis használatával [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx)</span><span class="sxs-lookup"><span data-stu-id="a6d53-153">Another alternative is tooinsert data in hello database using [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx)</span></span>
> 
> 

### <span data-ttu-id="a6d53-154"><a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="a6d53-154"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="a6d53-155">újonnan létrehozott hello szolgáltatás felvehető táblázatként oszlop tooan meglévő vagy új táblában tárolt és hello eredeti táblázatban a machine Learning szolgáltatáshoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="a6d53-155">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="a6d53-156">Szolgáltatások jön létre, vagy használatával érhető el, ha már létrehozott, hello [és adatokat importálhat](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul Azure ml alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="a6d53-156">Features can be generated or accessed if already created, using hello [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module in Azure ML as shown below:</span></span>

![az azureml-olvasók](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <span data-ttu-id="a6d53-158"><a name="python"></a>Például a Python-programozási nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="a6d53-158"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="a6d53-159">Python toogenerate funkciókat használ, amikor hello adatok SQL Server hasonló tooprocessing adatok Azure blob dokumentált Python használatával [Azure Blobadatok folyamat akkor adatok tudományos környezetben](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="a6d53-159">Using Python toogenerate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in you data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="a6d53-160">hello adatok pandas adatok keretbe hello adatbázisból betöltött toobe kell, és majd dolgozhatók további.</span><span class="sxs-lookup"><span data-stu-id="a6d53-160">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="a6d53-161">Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello adatok keret ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="a6d53-161">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="a6d53-162">a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, felhasználónevet és jelszót az adott értékek) használatával lehet:</span><span class="sxs-lookup"><span data-stu-id="a6d53-162">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="a6d53-163">Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz.</span><span class="sxs-lookup"><span data-stu-id="a6d53-163">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="a6d53-164">az alábbi kód hello olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:</span><span class="sxs-lookup"><span data-stu-id="a6d53-164">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="a6d53-165">Most a témakörök kiterjed hello Pandas adatok keret dolgozhat [létrehozása az Azure blob storage adatok Panda szolgáltatásai](machine-learning-data-science-create-features-blob.md).</span><span class="sxs-lookup"><span data-stu-id="a6d53-165">Now you can work with hello Pandas data frame as covered in topics [Create features for Azure blob storage data using Panda](machine-learning-data-science-create-features-blob.md).</span></span>

