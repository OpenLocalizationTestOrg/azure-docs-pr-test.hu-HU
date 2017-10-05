---
title: "Hive-lekérdezésekkel Hadoop-fürtben lévő adatok funkciók létrehozása |} Microsoft Docs"
description: "Példák a szolgáltatások készítése az Azure HDInsight Hadoop-fürt tárolt adatokat a Hive-lekérdezéseket."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e027a6ffcb63868be13432870e484c5cbf2eef4b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="385ad-103">Funkciók létrehozása az adatokhoz egy Hadoop-fürtben Hive-lekérdezések segítségével</span><span class="sxs-lookup"><span data-stu-id="385ad-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="385ad-104">Ez a dokumentum bemutatja, hogyan hozzon létre egy Azure HDInsight Hadoop-fürt Hive-lekérdezésekkel tárolt adatok funkciói.</span><span class="sxs-lookup"><span data-stu-id="385ad-104">This document shows how to create features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="385ad-105">A Hive-lekérdezéseket beágyazott Hive felhasználó által megadott funkciókat (UDF), amelynek parancsfájlokat használja.</span><span class="sxs-lookup"><span data-stu-id="385ad-105">These Hive queries use embedded Hive User Defined Functions (UDFs), the scripts for which are provided.</span></span>

<span data-ttu-id="385ad-106">A szolgáltatások létrehozásához szükséges műveleteket memóriaigényes lehet.</span><span class="sxs-lookup"><span data-stu-id="385ad-106">The operations needed to create features can be memory intensive.</span></span> <span data-ttu-id="385ad-107">A Hive-lekérdezések teljesítményét kritikus fontosságú ebben az esetben lesz, és bizonyos paraméterek hangolása javítja.</span><span class="sxs-lookup"><span data-stu-id="385ad-107">The performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="385ad-108">Ezek a paraméterek beállítása az utolsó szakaszban tárgyalt.</span><span class="sxs-lookup"><span data-stu-id="385ad-108">The tuning of these parameters is discussed in the final section.</span></span>

<span data-ttu-id="385ad-109">A lekérdezések, amelyek bemutatják példák jellemzőek a [NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="385ad-109">Examples of the queries that are presented are specific to the [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="385ad-110">Ezeket a lekérdezéseket már rendelkezik az adatok séma van megadva, és készen áll elküldésre váró futtatásához.</span><span class="sxs-lookup"><span data-stu-id="385ad-110">These queries already have data schema specified and are ready to be submitted to run.</span></span> <span data-ttu-id="385ad-111">A végső szakaszban paramétereket, így növelhető a Hive-lekérdezések teljesítményét észlelheti a felhasználók is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="385ad-111">In the final section, parameters that users can tune so that the performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="385ad-112">Ez **menü** szolgáltatások adatok létrehozása a különböző környezetek leíró témakörök hivatkozásait.</span><span class="sxs-lookup"><span data-stu-id="385ad-112">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="385ad-113">Ez a feladat Ez a lépés a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="385ad-113">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="385ad-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="385ad-114">Prerequisites</span></span>
<span data-ttu-id="385ad-115">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="385ad-115">This article assumes that you have:</span></span>

* <span data-ttu-id="385ad-116">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="385ad-116">Created an Azure storage account.</span></span> <span data-ttu-id="385ad-117">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="385ad-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="385ad-118">A HDInsight szolgáltatásban egy testreszabott Hadoop-fürt üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="385ad-118">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span>  <span data-ttu-id="385ad-119">Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="385ad-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="385ad-120">Az adatok az Azure HDInsight Hadoop-fürtök Hive táblák fel lett töltve.</span><span class="sxs-lookup"><span data-stu-id="385ad-120">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="385ad-121">Ha még nem, kövesse [létrehozása és az adatok betöltése a Hive táblák](machine-learning-data-science-move-hive-tables.md) feltölteni az adatokat a Hive táblák először.</span><span class="sxs-lookup"><span data-stu-id="385ad-121">If it has not, please follow [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="385ad-122">Engedélyezve van a fürt távoli eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="385ad-122">Enabled remote access to the cluster.</span></span> <span data-ttu-id="385ad-123">Ha módosítania kell az utasításokat, lásd: [a Head csomópont a Hadoop-fürt eléréséhez](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="385ad-123">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="385ad-124"><a name="hive-featureengineering"></a>Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="385ad-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="385ad-125">Ez a szakasz néhány példa a módszereket, amelyben funkciókat is kell generálása Hive-lekérdezésekkel ismerteti.</span><span class="sxs-lookup"><span data-stu-id="385ad-125">In this section, several examples of the ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="385ad-126">Miután létrehozta a további szolgáltatásokat, oszlopokként vegye fel a meglévő tábla, vagy hozzon létre egy új táblázat további funkciók és elsődleges kulcs, majd az eredeti táblázatban lehetne illeszteni.</span><span class="sxs-lookup"><span data-stu-id="385ad-126">Once you have generated additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, which can then be joined with the original table.</span></span> <span data-ttu-id="385ad-127">Az alábbiakban bemutatott példák:</span><span class="sxs-lookup"><span data-stu-id="385ad-127">Here are the examples presented:</span></span>

1. [<span data-ttu-id="385ad-128">Gyakoriság alapján a szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="385ad-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="385ad-129">A bináris osztályozási Kategorikus változók kockázata</span><span class="sxs-lookup"><span data-stu-id="385ad-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="385ad-130">Bontsa ki a szolgáltatásokat a DateTime típusú mező</span><span class="sxs-lookup"><span data-stu-id="385ad-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="385ad-131">Szolgáltatások kinyerése szövegmező</span><span class="sxs-lookup"><span data-stu-id="385ad-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="385ad-132">Kiszámíthatja GPS-koordináták közötti távolság</span><span class="sxs-lookup"><span data-stu-id="385ad-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="385ad-133"><a name="hive-frequencyfeature"></a>Gyakoriság alapján a szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="385ad-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="385ad-134">Érdemes gyakran kategorikus változó szintjének gyakoriságot, vagy több kategorikus változók szintjét egyes kombinációi gyakoriságát kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="385ad-134">It is often useful to calculate the frequencies of the levels of a categorical variable, or the frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="385ad-135">Felhasználók a következő parancsfájl segítségével az e számítása:</span><span class="sxs-lookup"><span data-stu-id="385ad-135">Users can use the following script to calculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="385ad-136"><a name="hive-riskfeature"></a>A bináris osztályozási Kategorikus változók kockázata</span><span class="sxs-lookup"><span data-stu-id="385ad-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="385ad-137">A bináris osztályozási igazolnia kell a nem numerikus kategorikus változók átalakítása numerikus szolgáltatások, a modellek használt csak numerikus szolgáltatások kerül.</span><span class="sxs-lookup"><span data-stu-id="385ad-137">In binary classification, we need to convert non-numeric categorical variables into numeric features when the models being used only take numeric features.</span></span> <span data-ttu-id="385ad-138">Ez történik, azáltal, hogy minden nem numerikus szinthez a numerikus kockázata.</span><span class="sxs-lookup"><span data-stu-id="385ad-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="385ad-139">Ebben a szakaszban megmutatjuk, néhány általános kockázati értékek (napló valószínűleg) kategorikus változó kiszámításához Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="385ad-139">In this section, we show some generic Hive queries that calculate the risk values (log odds) of a categorical variable.</span></span>

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

<span data-ttu-id="385ad-140">Ebben a példában, változók `smooth_param1` és `smooth_param2` a kockázati értékek az adatokból számított sima értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="385ad-140">In this example, variables `smooth_param1` and `smooth_param2` are set to smooth the risk values calculated from the data.</span></span> <span data-ttu-id="385ad-141">Kockázatok hogy a tartomány -Inf és Inf között.</span><span class="sxs-lookup"><span data-stu-id="385ad-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="385ad-142">A kockázatok > 0 azt jelzi, hogy a valószínűsége, hogy a cél 1-nél nagyobb, mint 0,5.</span><span class="sxs-lookup"><span data-stu-id="385ad-142">A risks > 0 indicates that the probability that the target is equal to 1 is greater than 0.5.</span></span>

<span data-ttu-id="385ad-143">A kockázat után tábla kiszámítása, felhasználókat rendelhet kockázati értékek tábla csatlakoztatná a kockázat táblával.</span><span class="sxs-lookup"><span data-stu-id="385ad-143">After the risk table is calculated, users can assign risk values to a table by joining it with the risk table.</span></span> <span data-ttu-id="385ad-144">Az előző szakaszban megadott csatlakozó Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="385ad-144">The Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="385ad-145"><a name="hive-datefeatures"></a>Bontsa ki a szolgáltatásokat a Datetime mezők</span><span class="sxs-lookup"><span data-stu-id="385ad-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="385ad-146">Hive tartalmaz egy felhasználó által megadott függvények a datetime mezők feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="385ad-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="385ad-147">A Hive, az alapértelmezett dátum és idő formátuma: "éééé-hh-nn 00:00:00" ("1970-01-01 12:21:32" például).</span><span class="sxs-lookup"><span data-stu-id="385ad-147">In Hive, the default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="385ad-148">Ebben a szakaszban megmutatjuk, bontsa ki a hónap, a DateTime típusú mező a hónap napját példákat és egyéb példák, amelyek a dátum/idő karakterlánc formátuma nem az alapértelmezett formátum dátum/idő karakterláncot az alapértelmezett formázása.</span><span class="sxs-lookup"><span data-stu-id="385ad-148">In this section, we show examples that extract the day of a month, the month from a datetime field, and other examples that convert a datetime string in a format other than the default format to a datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="385ad-149">A Hive-lekérdezést feltételezi, hogy a *&#60; DateTime típusú mező >* az alapértelmezett dátum és idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="385ad-149">This Hive query assumes that the *&#60;datetime field>* is in the default datetime format.</span></span>

<span data-ttu-id="385ad-150">Egy DateTime típusú mező nem az alapértelmezett formátumban van, ha először a DateTime típusú mező átalakítása Unix időbélyegzőjét, és majd alakíthatja át a Unix időbélyeg dátum/idő karakterlánc, amely az alapértelmezett formátumban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="385ad-150">If a datetime field is not in the default format, you need to convert the datetime field into Unix time stamp first, and then convert the Unix time stamp to a datetime string that is in the default format.</span></span> <span data-ttu-id="385ad-151">A dátum és idő formátuma alapértelmezett, ha felhasználók alkalmazhatja a beágyazott datetime felhasználó által megadott függvények szolgáltatások kibontásához.</span><span class="sxs-lookup"><span data-stu-id="385ad-151">When the datetime is in default format, users can apply the embedded datetime UDFs to extract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="385ad-152">Ebben a lekérdezésben Ha a *&#60; DateTime típusú mező >* rendelkezik a minta like *2015-03/26 12:04:39*, a *"&#60; a mintában a DateTime típusú mező >"* kell `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="385ad-152">In this query, if the *&#60;datetime field>* has the pattern like *03/26/2015 12:04:39*, the *'&#60;pattern of the datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="385ad-153">Tesztelheti, hogy a felhasználók futtathatják</span><span class="sxs-lookup"><span data-stu-id="385ad-153">To test it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="385ad-154">A *hivesampletable* ebben a lekérdezésben előre telepítve van az összes Azure HDInsight Hadoop-fürtök alapértelmezés szerint a fürtök kiosztásakor.</span><span class="sxs-lookup"><span data-stu-id="385ad-154">The *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when the clusters are provisioned.</span></span>

### <span data-ttu-id="385ad-155"><a name="hive-textfeatures"></a>Szolgáltatások kinyerése szövegmezők tartalma</span><span class="sxs-lookup"><span data-stu-id="385ad-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="385ad-156">A Hive tábla egy szövegmezőben, hogy szóközök határolja karakterláncot tartalmazó rendelkezik, a következő lekérdezés kicsomagolja a karakterláncot, és szót a karakterlánc hosszát.</span><span class="sxs-lookup"><span data-stu-id="385ad-156">When the Hive table has a text field that contains a string of words that are delimited by spaces, the following query extracts the length of the string, and the number of words in the string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="385ad-157"><a name="hive-gpsdistance"></a>Kiszámíthatja GPS koordinátákat közötti távolság</span><span class="sxs-lookup"><span data-stu-id="385ad-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="385ad-158">A jelen szakaszban megadott lekérdezést közvetlenül a következőt: Taxi út adatok alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="385ad-158">The query given in this section can be directly applied to the NYC Taxi Trip Data.</span></span> <span data-ttu-id="385ad-159">A lekérdezés célja egy beágyazott matematikai függvény a szolgáltatások létrehozásához Hive alkalmazásáról megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="385ad-159">The purpose of this query is to show how to apply an embedded mathematical functions in Hive to generate features.</span></span>

<span data-ttu-id="385ad-160">Az ebben a lekérdezésben használt mezőket a felvételi és dropoff helyeket nevű GPS koordinátáit *a felvételi\_hosszúság*, *a felvételi\_szélesség*, *dropoff\_hosszúság*, és *dropoff\_szélesség*.</span><span class="sxs-lookup"><span data-stu-id="385ad-160">The fields that are used in this query are the GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="385ad-161">A felvétel és dropoff koordináták közvetlen távolságát számító lekérdezéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="385ad-161">The queries that calculate the direct distance between the pickup and dropoff coordinates are:</span></span>

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

<span data-ttu-id="385ad-162">A két GPS-koordináták közötti távolság számító Matematikai egyenleteket található meg a <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type parancsfájlok</a> hely Peter Lapisu által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="385ad-162">The mathematical equations that calculate the distance between two GPS coordinates can be found on the <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="385ad-163">A Javascript, a függvény a `toRad()` csak van *lat_or_lon*pi/180 *, amely radiánban megadott szög fok alakítja át.</span><span class="sxs-lookup"><span data-stu-id="385ad-163">In his Javascript, the function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees to radians.</span></span> <span data-ttu-id="385ad-164">Itt *lat_or_lon* a szélesség vagy hosszúság.</span><span class="sxs-lookup"><span data-stu-id="385ad-164">Here, *lat_or_lon* is the latitude or longitude.</span></span> <span data-ttu-id="385ad-165">Mivel struktúra nem ad a függvény `atan2`, de a függvény `atan`, a `atan2` függvény megvalósítja `atan` a fenti Hive-lekérdezés segítségével a definícióját a megadott függvény <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="385ad-165">Since Hive does not provide the function `atan2`, but provides the function `atan`, the `atan2` function is implemented by `atan` function in the above Hive query using the definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="385ad-167">A beágyazott felhasználó által megadott függvények található Hive teljes listáját a **beépített funkciók** a szakasz a <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="385ad-167">A full list of Hive embedded UDFs can be found in the **Built-in Functions** section on the <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="385ad-168"><a name="tuning"></a>Speciális témakörök: hangolási Hive paraméterek lekérdezési teljesítmény javítása</span><span class="sxs-lookup"><span data-stu-id="385ad-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters to Improve Query Speed</span></span>
<span data-ttu-id="385ad-169">Előfordulhat, hogy az alapértelmezett paraméterbeállítások Hive fürt nem alkalmas a Hive-lekérdezéseket és az adatokat, amelyek a lekérdezések feldolgozás alatt.</span><span class="sxs-lookup"><span data-stu-id="385ad-169">The default parameter settings of Hive cluster might not be suitable for the Hive queries and the data that the queries are processing.</span></span> <span data-ttu-id="385ad-170">Ebben a szakaszban arról lesz szó néhány paraméter, amely észlelheti a felhasználók, amelyek javítják a Hive-lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="385ad-170">In this section, we discuss some parameters that users can tune that improve the performance of Hive queries.</span></span> <span data-ttu-id="385ad-171">Felhasználók kell hozzáadnia a lekérdezések a lekérdezéseket az adatok feldolgozása előtt hangolása paraméter.</span><span class="sxs-lookup"><span data-stu-id="385ad-171">Users need to add the parameter tuning queries before the queries of processing data.</span></span>

1. <span data-ttu-id="385ad-172">**Java halommemória terület**: lekérdezések nagy adatkészletek csatlakozni, vagy hosszú rekordjának feldolgozásáért **elegendő szabad terület halommemória** Ez egy általános hiba.</span><span class="sxs-lookup"><span data-stu-id="385ad-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of the common error.</span></span> <span data-ttu-id="385ad-173">Ez a paraméter beállításával szabályozható *mapreduce.map.java.opts* és *mapreduce.task.io.sort.mb* kívánt értékekre.</span><span class="sxs-lookup"><span data-stu-id="385ad-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* to desired values.</span></span> <span data-ttu-id="385ad-174">Például:</span><span class="sxs-lookup"><span data-stu-id="385ad-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="385ad-175">Ez a paraméter Java halommemória területre 4GB memóriát foglal le, majd is teszi rendezés hatékonyabb több memóriát oszt ki azt.</span><span class="sxs-lookup"><span data-stu-id="385ad-175">This parameter allocates 4GB memory to Java heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="385ad-176">Célszerű lejátszás a megoldást, ha bármely halommemória terület kapcsolatos hiba hibák állnak fenn.</span><span class="sxs-lookup"><span data-stu-id="385ad-176">It is a good idea to play with these allocations if there are any job failure errors related to heap space.</span></span>

1. <span data-ttu-id="385ad-177">**Az elosztott Fájlrendszerbeli blokkméret** : Ez a paraméter állandóként állítja be, a fájlrendszer által tárolt adatokat a legkisebb egysége.</span><span class="sxs-lookup"><span data-stu-id="385ad-177">**DFS block size** : This parameter sets the smallest unit of data that the file system stores.</span></span> <span data-ttu-id="385ad-178">Például ha az elosztott Fájlrendszerbeli blokkméret 128MB, majd mérete adatot legalább és legfeljebb 128MB tárolódik egyetlen blokkot tartalmaz, amely nagyobb, mint 128MB számára engedélyezett a felesleges blokkok adatainak közben.</span><span class="sxs-lookup"><span data-stu-id="385ad-178">As an example, if the DFS block size is 128MB, then any data of size less than and up to 128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="385ad-179">Egy nagyon kis blokkméretet kiválasztása hatására nagy terhek Hadoop, mert a név csomópont található a megfelelő blokkot, a fájl vonatkozó számos további kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="385ad-179">Choosing a very small block size causes large overheads in Hadoop since the name node has to process many more requests to find the relevant block pertaining to the file.</span></span> <span data-ttu-id="385ad-180">A javasolt beállítás foglalkozó gigabájt (vagy nagyobb) adat:</span><span class="sxs-lookup"><span data-stu-id="385ad-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="385ad-181">**Hive join művelet optimalizálása** : közben összekapcsolási műveletek térkép/csökkentse keretében általában kerül sor a csökkentse fázisban, egyes esetekben hatalmas növekedését elérhető illesztések ütemezésével a térkép fázisban (más néven "mapjoins").</span><span class="sxs-lookup"><span data-stu-id="385ad-181">**Optimizing join operation in Hive** : While join operations in the map/reduce framework typically take place in the reduce phase, sometimes, enormous gains can be achieved by scheduling joins in the map phase (also called "mapjoins").</span></span> <span data-ttu-id="385ad-182">Közvetlen Hive ehhez, amikor csak lehetséges, hogy állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="385ad-182">To direct Hive to do this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="385ad-183">**A Hive mappers számát** : közben Hadoop lehetővé teszi a felhasználónak szűkítő adjon meg, a száma mappers általában a felhasználó nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="385ad-183">**Specifying the number of mappers to Hive** : While Hadoop allows the user to set the number of reducers, the number of mappers is typically not be set by the user.</span></span> <span data-ttu-id="385ad-184">Amely lehetővé teszi, hogy ez a szám a vezérlő bizonyos fokú körben, hogy válassza körültekintően a Hadoop változók *mapred.min.split.size* és *mapred.max.split.size* minden leképezés méretének feladat határozza meg:</span><span class="sxs-lookup"><span data-stu-id="385ad-184">A trick that allows some degree of control on this number is to choose the Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as the size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="385ad-185">Általában az alapértelmezett érték *mapred.min.split.size* 0, az *mapred.max.split.size* van **Long.MAX** és az *dfs.block.size* 64 MB.</span><span class="sxs-lookup"><span data-stu-id="385ad-185">Typically, the default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="385ad-186">Ahogyan azt láthatja, megadott adatok mérete hangolása ezeket a paramétereket "beállítása" őket lehetővé teszi hangolására használható mappers száma.</span><span class="sxs-lookup"><span data-stu-id="385ad-186">As we can see, given the data size, tuning these parameters by "setting" them allows us to tune the number of mappers used.</span></span>
4. <span data-ttu-id="385ad-187">Még más néhány **speciális beállítások** Hive optimalizálási teljesítmény alatt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="385ad-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="385ad-188">Ezek lehetővé teszik rendelve, és csökkentheti a feladatok számára fenntartott memória mérete, és elősegíti a teljesítmény tökéletesítse.</span><span class="sxs-lookup"><span data-stu-id="385ad-188">These allow you to set the memory allocated to map and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="385ad-189">Ellenőrizze a következőket kell figyelembe venni, hogy a *mapreduce.reduce.memory.mb* nem lehet nagyobb, mint a Hadoop-fürt egyes feldolgozó csomópontok fizikai memória méretét.</span><span class="sxs-lookup"><span data-stu-id="385ad-189">Please keep in mind that the *mapreduce.reduce.memory.mb* cannot be greater than the physical memory size of each worker node in the Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

