---
title: "az Azure SQL Server virtuális gépen aaaExplore adatok |} Microsoft Docs"
description: "Adatokba és szolgáltatások létrehozása az Azure SQL Server virtuális gépen"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="1a483-103"><a name="heading"></a>Az SQL Server virtuális gépet az Azure adatfeldolgozásra</span><span class="sxs-lookup"><span data-stu-id="1a483-103"><a name="heading"></a>Process Data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="1a483-104">Ez a dokumentum hogyan tooexplore adatok és az SQL Server virtuális gép az Azure-on tárolt adatok szolgáltatásai generálásához.</span><span class="sxs-lookup"><span data-stu-id="1a483-104">This document covers how tooexplore data and generate features for data stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="1a483-105">Ezt adatok wrangling SQL használatával, és Python hasonló programozási nyelv használatával is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="1a483-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

> [!NOTE]
> <span data-ttu-id="1a483-106">Ez a dokumentum hello minta SQL-utasítások feltételezik, hogy adatokat az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1a483-106">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="1a483-107">Ha nem, olvassa el az toohello felhő adatok tudományos folyamat térkép toolearn hogyan toomove az adatok tooSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="1a483-107">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="1a483-108"><a name="SQL"></a>SQL használatával</span><span class="sxs-lookup"><span data-stu-id="1a483-108"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="1a483-109">Azt írja le a hello wrangling feladatok ebben a szakaszban az SQL a következő:</span><span class="sxs-lookup"><span data-stu-id="1a483-109">We describe hello following data wrangling tasks in this section using SQL:</span></span>

1. [<span data-ttu-id="1a483-110">Az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="1a483-110">Data Exploration</span></span>](#sql-dataexploration)
2. [<span data-ttu-id="1a483-111">Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-111">Feature Generation</span></span>](#sql-featuregen)

### <span data-ttu-id="1a483-112"><a name="sql-dataexploration"></a>Az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="1a483-112"><a name="sql-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="1a483-113">Íme néhány példa SQL-parancsfájlok, amely az SQL Server használt tooexplore adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="1a483-113">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="1a483-114">Gyakorlati például használhatja hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) , majd tekintse át a toohello című IPNB [NYC adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) egy végpontok közötti segédlet az.</span><span class="sxs-lookup"><span data-stu-id="1a483-114">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

1. <span data-ttu-id="1a483-115">Napi megfigyeléseket hello számbavétele</span><span class="sxs-lookup"><span data-stu-id="1a483-115">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="1a483-116">Hello szintek kategorikus oszlopban beolvasása</span><span class="sxs-lookup"><span data-stu-id="1a483-116">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="1a483-117">Hello szintek száma kombinációja kategorikus kétoszlopos lekérése</span><span class="sxs-lookup"><span data-stu-id="1a483-117">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="1a483-118">Hello terjesztési numerikus oszlopok az beszerzése</span><span class="sxs-lookup"><span data-stu-id="1a483-118">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <span data-ttu-id="1a483-119"><a name="sql-featuregen"></a>Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-119"><a name="sql-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="1a483-120">Ez a szakasz azt módokat SQL funkciók generálása mutatják be:</span><span class="sxs-lookup"><span data-stu-id="1a483-120">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="1a483-121">Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-121">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="1a483-122">A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-122">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="1a483-123">Működés közbeni hello szolgáltatások csak egy oszlop</span><span class="sxs-lookup"><span data-stu-id="1a483-123">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="1a483-124">Létrehozhat további szolgáltatásokat, ha oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, hello eredeti tábla lehetne illeszteni.</span><span class="sxs-lookup"><span data-stu-id="1a483-124">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span> 
> 
> 

### <span data-ttu-id="1a483-125"><a name="sql-countfeature"></a>Count alapú szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-125"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="1a483-126">hello alábbi példák bemutatják, kétféle módon száma szolgáltatások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1a483-126">hello following examples demonstrate two ways of generating count features.</span></span> <span data-ttu-id="1a483-127">hello első módszer Feltételes összegzés és hello második módszert használja a "where" záradék hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1a483-127">hello first method uses conditional sum and hello second method uses hello 'where' clause.</span></span> <span data-ttu-id="1a483-128">Ezek ezután társíthatók hello eredeti (elsődleges kulcs oszlopokat használó) tábla toohave száma szolgáltatásokkal együtt hello eredeti adatok.</span><span class="sxs-lookup"><span data-stu-id="1a483-128">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <span data-ttu-id="1a483-129"><a name="sql-binningfeature"></a>A dobozolás szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a483-129"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="1a483-130">hello következő példa bemutatja, hogyan toogenerate binned szolgáltatások által a dobozolás (használatával öt bins) egy numerikus oszlopot, amely a ehelyett szolgáltatásként használható:</span><span class="sxs-lookup"><span data-stu-id="1a483-130">hello following example shows how toogenerate binned features by binning (using five bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="1a483-131"><a name="sql-featurerollout"></a>Működés közbeni hello szolgáltatások csak egy oszlop</span><span class="sxs-lookup"><span data-stu-id="1a483-131"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="1a483-132">Ebben a szakaszban a bemutatjuk, hogyan tooroll csak egy oszlop a tábla toogenerate további szolgáltatások ki.</span><span class="sxs-lookup"><span data-stu-id="1a483-132">In this section, we demonstrate how tooroll out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="1a483-133">hello példa feltételezi, hogy van-e a szélességi és hosszúsági oszlop, amelyből toogenerate szolgáltatások kívánt hello tábla.</span><span class="sxs-lookup"><span data-stu-id="1a483-133">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="1a483-134">Íme egy rövid ismertetése a szélesség/hosszúság helyadatok (a stackoverflow forrásokat [hogyan toomeasure hello szélességi és hosszúsági pontosságát?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span><span class="sxs-lookup"><span data-stu-id="1a483-134">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow [How toomeasure hello accuracy of latitude and longitude?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span></span> <span data-ttu-id="1a483-135">Ez az featurizing hello a location mező előtt hasznos toounderstand:</span><span class="sxs-lookup"><span data-stu-id="1a483-135">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="1a483-136">hello bejelentkezési közli nekünk e dolgozunk, déli vagy északi keleti vagy nyugati a hello földgolyó méretét.</span><span class="sxs-lookup"><span data-stu-id="1a483-136">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="1a483-137">Egy nem nulla több száz számjegy közli velünk, hogy használjuk-e hosszúsági és szélességi nem!</span><span class="sxs-lookup"><span data-stu-id="1a483-137">A nonzero hundreds digit tells us that we're using longitude, not latitude!</span></span>
* <span data-ttu-id="1a483-138">hello több számjegyet biztosít a pozíció tooabout 1000 kilométerben.</span><span class="sxs-lookup"><span data-stu-id="1a483-138">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="1a483-139">Milyen kontinensen vagy óceáni dolgozunk a hasznos információkat nyújt nekünk.</span><span class="sxs-lookup"><span data-stu-id="1a483-139">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="1a483-140">hello egységek számjegy (egy decimális fok) biztosít egy feljebb too111 kilométerben (60 tengeri miles, körülbelül 69 miles).</span><span class="sxs-lookup"><span data-stu-id="1a483-140">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="1a483-141">Azt is adja meg, nagyjából milyen nagy állapot vagy a dolgozunk ország.</span><span class="sxs-lookup"><span data-stu-id="1a483-141">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="1a483-142">hello tizedes jegyre érdemes működik-e too11.1 km: azt is különbözteti meg a szomszédos nagy város egy nagy város hello pozícióját.</span><span class="sxs-lookup"><span data-stu-id="1a483-142">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="1a483-143">hello második tizedes érdemes működik-e too1.1 km: azt is egy falu eltérő, külön hello mellett.</span><span class="sxs-lookup"><span data-stu-id="1a483-143">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="1a483-144">hello harmadik tizedes működik-érdemes e too110 m: nagy mezőgazdasági mező vagy intézményi egyetemi is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="1a483-144">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="1a483-145">hello negyedik tizedes működik-érdemes e too11 m: egy is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="1a483-145">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="1a483-146">Hasonló toohello tipikus pontossága nem zavarja a nem javított GPS egység.</span><span class="sxs-lookup"><span data-stu-id="1a483-146">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="1a483-147">hello ötödik tizedes működik-érdemes e too1.1 m: azt fák különbözteti meg egymástól.</span><span class="sxs-lookup"><span data-stu-id="1a483-147">hello fifth decimal place is worth up too1.1 m: it distinguishes trees from each other.</span></span> <span data-ttu-id="1a483-148">Pontosság toothis szintjét az kereskedelmi GPS-egységekhez csak különbözeti helyesbítéssel lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="1a483-148">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="1a483-149">hello hatodik tizedes érdemes működik-e too0.11 m: is használhatja ezt a részletes tájak, tervezéséhez struktúrák elrendezése utak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1a483-149">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="1a483-150">Több mint elég jó glaciers és folyókat követési kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1a483-150">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="1a483-151">Ez megvalósítható a GPS, például a differentially javított GPS painstaking intézkedéseket.</span><span class="sxs-lookup"><span data-stu-id="1a483-151">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="1a483-152">hello helyére vonatkozó információkat is a következők featurized, régió, a hely és a várost elválasztó.</span><span class="sxs-lookup"><span data-stu-id="1a483-152">hello location information can be featurized as follows, separating out region, location, and city information.</span></span> <span data-ttu-id="1a483-153">Vegye figyelembe, hogy Ön is meghívhatja a REST-végpont például a Bing térképek API érhető el [pont hely keresése](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello régió/körzeti információkat.</span><span class="sxs-lookup"><span data-stu-id="1a483-153">Note that you can also call a REST end point such as Bing Maps API available at [Find a Location by Point](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello region/district information.</span></span>

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

<span data-ttu-id="1a483-154">Ezeket a helyalapú szolgáltatásokat lehet további használt toogenerate további száma szolgáltatások ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="1a483-154">These location-based features can be further used toogenerate additional count features as described earlier.</span></span> 

> [!TIP]
> <span data-ttu-id="1a483-155">Programozott módon szúrhat be a választott nyelven hello rögzíti.</span><span class="sxs-lookup"><span data-stu-id="1a483-155">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="1a483-156">Szükség lehet az adattömbök tooimprove írási hatékonyságát tooinsert hello adatokat (a példa bemutatja, hogyan toodo a pyodbc használata, lásd: [A HelloWorld minta tooaccess SQLServer python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span><span class="sxs-lookup"><span data-stu-id="1a483-156">You may need tooinsert hello data in chunks tooimprove write efficiency (for an example of how toodo this using pyodbc, see [A HelloWorld sample tooaccess SQLServer with python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span></span> <span data-ttu-id="1a483-157">Egy másik lehetőség az tooinsert adatai hello adatbázisban hello használatával [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a483-157">Another alternative is tooinsert data in hello database using hello [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx).</span></span>
> 
> 

### <span data-ttu-id="1a483-158"><a name="sql-aml"></a>Csatlakozás tooAzure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="1a483-158"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="1a483-159">újonnan létrehozott hello szolgáltatás felvehető táblázatként oszlop tooan meglévő vagy új táblában tárolt és hello eredeti táblázatban a machine Learning szolgáltatáshoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="1a483-159">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="1a483-160">Szolgáltatások jön létre, vagy használatával érhető el, ha már létrehozott, hello [és adatokat importálhat] [ import-data] modul az Azure Machine Learning alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="1a483-160">Features can be generated or accessed if already created, using hello [Import Data][import-data] module in Azure Machine Learning as shown below:</span></span>

![az azureml-olvasók][1] 

## <span data-ttu-id="1a483-162"><a name="python"></a>Például a Python-programozási nyelv használatával</span><span class="sxs-lookup"><span data-stu-id="1a483-162"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="1a483-163">Python tooexplore adatokkal, és generáljon szolgáltatásokat, ha hello adatok SQL Server olyan hasonló tooprocessing adatok dokumentált Python használatával az Azure BLOB [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="1a483-163">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="1a483-164">hello adatok pandas adatok keretbe hello adatbázisból betöltött toobe kell, és majd dolgozhatók további.</span><span class="sxs-lookup"><span data-stu-id="1a483-164">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="1a483-165">Azt a dokumentum hello folyamat toohello adatbázis csatlakoztatása és hello adatok betöltését hello adatok keret ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1a483-165">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="1a483-166">a következő kapcsolati karakterlánc-formátum hello használt tooconnect tooa SQL Server-adatbázis a Python pyodbc (csere kiszolgálónév, dbname, a felhasználónevet és jelszót az adott értékek) használatával lehet:</span><span class="sxs-lookup"><span data-stu-id="1a483-166">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="1a483-167">Hello [Pandas könyvtár](http://pandas.pydata.org/) Python szolgáltatás széles választékának adatstruktúrák és adatok elemzésére szolgáló eszközöket ad adatkezelési Python programozáshoz.</span><span class="sxs-lookup"><span data-stu-id="1a483-167">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="1a483-168">az alábbi kód hello olvassa hello eredményt Pandas adatok keretbe egy SQL Server-adatbázist vissza:</span><span class="sxs-lookup"><span data-stu-id="1a483-168">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="1a483-169">Most, együttműködhet hello Pandas adatok keret hello cikkben szereplő [Azure Blobadatok folyamat adatok tudományos környezetében](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="1a483-169">Now you can work with hello Pandas data frame as covered in hello article [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="azure-data-science-in-action-example"></a><span data-ttu-id="1a483-170">Az Azure Data tudományos művelet példa</span><span class="sxs-lookup"><span data-stu-id="1a483-170">Azure Data Science in Action Example</span></span>
<span data-ttu-id="1a483-171">Hello Azure adatok tudományos folyamat egy nyilvános adatkészlet-végpontok közötti bemutató példa: [Azure adatok tudományos folyamat működés közben](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="1a483-171">For an end-to-end walkthrough example of hello Azure Data Science Process using a public dataset, see [Azure Data Science Process in Action](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

