---
title: "a Hive-lekérdezésekkel Hadoop-fürtben lévő adatok aaaCreate szolgáltatások |} Microsoft Docs"
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
ms.openlocfilehash: 686282bf0fb84ea82758d3c5b7de2bd90f0cf159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="f8667-103">Funkciók létrehozása az adatokhoz egy Hadoop-fürtben Hive-lekérdezések segítségével</span><span class="sxs-lookup"><span data-stu-id="f8667-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="f8667-104">Ez a dokumentum látható, hogyan toocreate szolgáltatások adatok az Azure HDInsight Hadoop-fürt Hive-lekérdezésekkel tárol.</span><span class="sxs-lookup"><span data-stu-id="f8667-104">This document shows how toocreate features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="f8667-105">A Hive-lekérdezéseket beágyazott Hive felhasználó által megadott funkciókat (UDF) használatához hello parancsfájlok, amelynek vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="f8667-105">These Hive queries use embedded Hive User Defined Functions (UDFs), hello scripts for which are provided.</span></span>

<span data-ttu-id="f8667-106">hello szükséges műveletek toocreate szolgáltatások memóriaigényes lehet.</span><span class="sxs-lookup"><span data-stu-id="f8667-106">hello operations needed toocreate features can be memory intensive.</span></span> <span data-ttu-id="f8667-107">Hive-lekérdezések teljesítményét hello kritikus fontosságú ebben az esetben lesz, és bizonyos paraméterek hangolása javítja.</span><span class="sxs-lookup"><span data-stu-id="f8667-107">hello performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="f8667-108">hello végső szakaszban tárgyalt hello beállítása a következő paraméterek közül.</span><span class="sxs-lookup"><span data-stu-id="f8667-108">hello tuning of these parameters is discussed in hello final section.</span></span>

<span data-ttu-id="f8667-109">Hello lekérdezések, amelyek bemutatják többek között az adott toohello [NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="f8667-109">Examples of hello queries that are presented are specific toohello [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="f8667-110">Ezeket a lekérdezéseket már adatok séma van megadva, és készen áll a toobe benyújtott toorun.</span><span class="sxs-lookup"><span data-stu-id="f8667-110">These queries already have data schema specified and are ready toobe submitted toorun.</span></span> <span data-ttu-id="f8667-111">Hello a végső szakaszban paramétereket, így növelhető a Hive-lekérdezések teljesítményét hello észlelheti a felhasználók is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f8667-111">In hello final section, parameters that users can tune so that hello performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="f8667-112">Ez **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toocreate-adatok különböző környezetekben funkcióit tootopics.</span><span class="sxs-lookup"><span data-stu-id="f8667-112">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="f8667-113">Ez a feladat ez hello lépés [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="f8667-113">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8667-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8667-114">Prerequisites</span></span>
<span data-ttu-id="f8667-115">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="f8667-115">This article assumes that you have:</span></span>

* <span data-ttu-id="f8667-116">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8667-116">Created an Azure storage account.</span></span> <span data-ttu-id="f8667-117">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="f8667-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="f8667-118">A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve.</span><span class="sxs-lookup"><span data-stu-id="f8667-118">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="f8667-119">Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f8667-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="f8667-120">hello már feltöltött tooHive táblák az Azure HDInsight Hadoop-fürtök.</span><span class="sxs-lookup"><span data-stu-id="f8667-120">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="f8667-121">Ha még nem, kövesse [létrehozása és a betöltés tooHive adattáblák](machine-learning-data-science-move-hive-tables.md) tooupload adatok tooHive először táblázatot.</span><span class="sxs-lookup"><span data-stu-id="f8667-121">If it has not, please follow [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="f8667-122">Engedélyezett távoli hozzáférési toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="f8667-122">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="f8667-123">Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="f8667-123">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="f8667-124"><a name="hive-featureengineering"></a>Szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8667-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="f8667-125">Ez a szakasz számos példát, amelyben funkciókat is lehet generálása Hive-lekérdezésekkel hello módszereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f8667-125">In this section, several examples of hello ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="f8667-126">Miután létrehozta a további szolgáltatásokat, oszlopok toohello meglévő táblaként adja hozzá, vagy hozzon létre egy új tábla hello további funkciók és elsődleges kulcs, amely majd lehetne illeszteni hello eredeti táblázatban.</span><span class="sxs-lookup"><span data-stu-id="f8667-126">Once you have generated additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, which can then be joined with hello original table.</span></span> <span data-ttu-id="f8667-127">Az alábbiakban bemutatott hello példák:</span><span class="sxs-lookup"><span data-stu-id="f8667-127">Here are hello examples presented:</span></span>

1. [<span data-ttu-id="f8667-128">Gyakoriság alapján a szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8667-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="f8667-129">A bináris osztályozási Kategorikus változók kockázata</span><span class="sxs-lookup"><span data-stu-id="f8667-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="f8667-130">Bontsa ki a szolgáltatásokat a DateTime típusú mező</span><span class="sxs-lookup"><span data-stu-id="f8667-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="f8667-131">Szolgáltatások kinyerése szövegmező</span><span class="sxs-lookup"><span data-stu-id="f8667-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="f8667-132">Kiszámíthatja GPS-koordináták közötti távolság</span><span class="sxs-lookup"><span data-stu-id="f8667-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="f8667-133"><a name="hive-frequencyfeature"></a>Gyakoriság alapján a szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8667-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="f8667-134">Ez a legtöbbször hasznos toocalculate hello gyakoriságát hello szintek kategorikus változó, vagy több kategorikus változók szintjét egyes kombinációi hello gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="f8667-134">It is often useful toocalculate hello frequencies of hello levels of a categorical variable, or hello frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="f8667-135">Felhasználók az e használja a következő parancsfájl toocalculate hello:</span><span class="sxs-lookup"><span data-stu-id="f8667-135">Users can use hello following script toocalculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="f8667-136"><a name="hive-riskfeature"></a>A bináris osztályozási Kategorikus változók kockázata</span><span class="sxs-lookup"><span data-stu-id="f8667-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="f8667-137">A bináris osztályozási igazolnia kell tooconvert nem numerikus kategorikus változók numerikus funkciók be hello modellek használt csak numerikus szolgáltatások kerül.</span><span class="sxs-lookup"><span data-stu-id="f8667-137">In binary classification, we need tooconvert non-numeric categorical variables into numeric features when hello models being used only take numeric features.</span></span> <span data-ttu-id="f8667-138">Ez történik, azáltal, hogy minden nem numerikus szinthez a numerikus kockázata.</span><span class="sxs-lookup"><span data-stu-id="f8667-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="f8667-139">Ebben a szakaszban megmutatjuk, néhány általános hello kockázati értékek (napló valószínűleg) kategorikus változó kiszámításához Hive-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="f8667-139">In this section, we show some generic Hive queries that calculate hello risk values (log odds) of a categorical variable.</span></span>

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

<span data-ttu-id="f8667-140">Ebben a példában, változók `smooth_param1` és `smooth_param2` set toosmooth hello kockázati értékek kiszámítása hello adatokból.</span><span class="sxs-lookup"><span data-stu-id="f8667-140">In this example, variables `smooth_param1` and `smooth_param2` are set toosmooth hello risk values calculated from hello data.</span></span> <span data-ttu-id="f8667-141">Kockázatok hogy a tartomány -Inf és Inf között.</span><span class="sxs-lookup"><span data-stu-id="f8667-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="f8667-142">A kockázatok > 0 azt jelzi, hogy hello valószínűsége, hogy a célkiszolgáló hello egyenlő too1 0,5-nél nagyobb.</span><span class="sxs-lookup"><span data-stu-id="f8667-142">A risks > 0 indicates that hello probability that hello target is equal too1 is greater than 0.5.</span></span>

<span data-ttu-id="f8667-143">Hello kockázat tábla kiszámítása, miután felhasználók kockázati értékek tooa tábla hozzárendeléséhez csatlakoztatná hello kockázat táblával.</span><span class="sxs-lookup"><span data-stu-id="f8667-143">After hello risk table is calculated, users can assign risk values tooa table by joining it with hello risk table.</span></span> <span data-ttu-id="f8667-144">az előző szakaszban hello Hive csatlakozó lekérdezés lett megadva.</span><span class="sxs-lookup"><span data-stu-id="f8667-144">hello Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="f8667-145"><a name="hive-datefeatures"></a>Bontsa ki a szolgáltatásokat a Datetime mezők</span><span class="sxs-lookup"><span data-stu-id="f8667-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="f8667-146">Hive tartalmaz egy felhasználó által megadott függvények a datetime mezők feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="f8667-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="f8667-147">A Hive, hello alapértelmezett dátum és idő formátuma: "éééé-hh-nn 00:00:00" ("1970-01-01 12:21:32" például).</span><span class="sxs-lookup"><span data-stu-id="f8667-147">In Hive, hello default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="f8667-148">Ebben a szakaszban megmutatjuk, bontsa ki a hónap, a DateTime típusú mező hello hónap napja hello példákat és egyéb példák, amelyek a dátum/idő karakterlánc formátuma eltérő hello alapértelmezett formátum tooa dátum/idő karakterláncot az alapértelmezett formázni.</span><span class="sxs-lookup"><span data-stu-id="f8667-148">In this section, we show examples that extract hello day of a month, hello month from a datetime field, and other examples that convert a datetime string in a format other than hello default format tooa datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="f8667-149">A Hive-lekérdezést feltételezi, hogy hello *&#60; DateTime típusú mező >* hello alapértelmezett dátum és idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="f8667-149">This Hive query assumes that hello *&#60;datetime field>* is in hello default datetime format.</span></span>

<span data-ttu-id="f8667-150">Ha egy DateTime típusú mező nincs a hello alapértelmezett formátuma, először a Unix időbélyeg szüksége tooconvert hello DateTime típusú mező, majd a konvertálás hello Unix idő stamp tooa dátum/idő karakterlánc, amely hello alapértelmezett formátumban.</span><span class="sxs-lookup"><span data-stu-id="f8667-150">If a datetime field is not in hello default format, you need tooconvert hello datetime field into Unix time stamp first, and then convert hello Unix time stamp tooa datetime string that is in hello default format.</span></span> <span data-ttu-id="f8667-151">Hello dátum és idő formátuma alapértelmezett, amikor a felhasználók alkalmazhatja a beágyazott hello dátum és idő felhasználó által megadott függvények tooextract funkciót.</span><span class="sxs-lookup"><span data-stu-id="f8667-151">When hello datetime is in default format, users can apply hello embedded datetime UDFs tooextract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="f8667-152">Ebben a lekérdezésben, ha hello *&#60; DateTime típusú mező >* hello minta like rendelkezik *2015-03/26 12:04:39*, hello *"&#60; hello DateTime típusú mező mintát >"* kell lennie `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="f8667-152">In this query, if hello *&#60;datetime field>* has hello pattern like *03/26/2015 12:04:39*, hello *'&#60;pattern of hello datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="f8667-153">tootest, felhasználók futtatható</span><span class="sxs-lookup"><span data-stu-id="f8667-153">tootest it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="f8667-154">Hello *hivesampletable* ebben a lekérdezésben előre telepítve van az összes Azure HDInsight Hadoop-fürtök alapértelmezés szerint hello fürtök kiosztásakor.</span><span class="sxs-lookup"><span data-stu-id="f8667-154">hello *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when hello clusters are provisioned.</span></span>

### <span data-ttu-id="f8667-155"><a name="hive-textfeatures"></a>Szolgáltatások kinyerése szövegmezők tartalma</span><span class="sxs-lookup"><span data-stu-id="f8667-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="f8667-156">Amikor hello Hive tábla egy szövegmezőben, hogy szóközök határolja karakterláncot tartalmazó, hello következő lekérdezés kibontása hello hello karakterlánc, és hello szavak száma a hello karakterlánc hossza.</span><span class="sxs-lookup"><span data-stu-id="f8667-156">When hello Hive table has a text field that contains a string of words that are delimited by spaces, hello following query extracts hello length of hello string, and hello number of words in hello string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="f8667-157"><a name="hive-gpsdistance"></a>Kiszámíthatja GPS koordinátákat közötti távolság</span><span class="sxs-lookup"><span data-stu-id="f8667-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="f8667-158">Ebben a szakaszban megadott hello lekérdezés közvetlenül alkalmazott toohello NYC Taxi út adatok lehet.</span><span class="sxs-lookup"><span data-stu-id="f8667-158">hello query given in this section can be directly applied toohello NYC Taxi Trip Data.</span></span> <span data-ttu-id="f8667-159">hello Ez a lekérdezés célja tooshow hogyan egy beágyazott tooapply matematikai funkciók Hive toogenerate funkciói.</span><span class="sxs-lookup"><span data-stu-id="f8667-159">hello purpose of this query is tooshow how tooapply an embedded mathematical functions in Hive toogenerate features.</span></span>

<span data-ttu-id="f8667-160">hello ebben a lekérdezésben használt mezők szerepelnek a felvételi és dropoff helyeket nevű hello GPS-koordinátáit *a felvételi\_hosszúság*, *a felvételi\_szélesség*,  *dropoff\_hosszúság*, és *dropoff\_szélesség*.</span><span class="sxs-lookup"><span data-stu-id="f8667-160">hello fields that are used in this query are hello GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="f8667-161">hello közvetlen közötti távolság szerint hello felvétel és dropoff koordináták számító hello lekérdezéseket a következők:</span><span class="sxs-lookup"><span data-stu-id="f8667-161">hello queries that calculate hello direct distance between hello pickup and dropoff coordinates are:</span></span>

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

<span data-ttu-id="f8667-162">hello Matematikai egyenleteket két GPS-koordináták hello távolságát számító hello található <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type parancsfájlok</a> hely Peter Lapisu által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f8667-162">hello mathematical equations that calculate hello distance between two GPS coordinates can be found on hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="f8667-163">A Javascript, a függvény hello `toRad()` csak van *lat_or_lon*pi/180 *, fok tooradians alakítja át, amely.</span><span class="sxs-lookup"><span data-stu-id="f8667-163">In his Javascript, hello function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees tooradians.</span></span> <span data-ttu-id="f8667-164">Itt *lat_or_lon* hello szélességi és hosszúsági van.</span><span class="sxs-lookup"><span data-stu-id="f8667-164">Here, *lat_or_lon* is hello latitude or longitude.</span></span> <span data-ttu-id="f8667-165">Mivel struktúra nem ad hello függvény `atan2`, de hello függvény `atan`, hello `atan2` függvény megvalósítja `atan` függvény a fent megadott hello definition Hive-lekérdezés hello <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="f8667-165">Since Hive does not provide hello function `atan2`, but provides hello function `atan`, hello `atan2` function is implemented by `atan` function in hello above Hive query using hello definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="f8667-167">A Hive hello található beágyazott felhasználó által megadott függvények teljes listáját **beépített funkciók** szakaszt, hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="f8667-167">A full list of Hive embedded UDFs can be found in hello **Built-in Functions** section on hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="f8667-168"><a name="tuning"></a>Speciális témakörök: hangolási Hive paraméterek tooImprove lekérdezés sebesség</span><span class="sxs-lookup"><span data-stu-id="f8667-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters tooImprove Query Speed</span></span>
<span data-ttu-id="f8667-169">hello Hive fürt beállítások nem feltétlenül alkalmas hello Hive-lekérdezéseket és hello lekérdezések feldolgozás alatt hello adatok alapértelmezett paraméter.</span><span class="sxs-lookup"><span data-stu-id="f8667-169">hello default parameter settings of Hive cluster might not be suitable for hello Hive queries and hello data that hello queries are processing.</span></span> <span data-ttu-id="f8667-170">Ebben a szakaszban arról lesz szó néhány paramétereket, amelyek a felhasználók észlelheti a Hive-lekérdezések hello teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="f8667-170">In this section, we discuss some parameters that users can tune that improve hello performance of Hive queries.</span></span> <span data-ttu-id="f8667-171">A felhasználóknak kell tooadd hello paraméter hangolása lekérdezések hello lekérdezések adatok feldolgozása előtt.</span><span class="sxs-lookup"><span data-stu-id="f8667-171">Users need tooadd hello parameter tuning queries before hello queries of processing data.</span></span>

1. <span data-ttu-id="f8667-172">**Java halommemória terület**: lekérdezések nagy adatkészletek csatlakozni, vagy hosszú rekordjának feldolgozásáért **elegendő szabad terület halommemória** hello közös hiba egyike.</span><span class="sxs-lookup"><span data-stu-id="f8667-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of hello common error.</span></span> <span data-ttu-id="f8667-173">Ez a paraméter beállításával szabályozható *mapreduce.map.java.opts* és *mapreduce.task.io.sort.mb* toodesired értékeket.</span><span class="sxs-lookup"><span data-stu-id="f8667-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* toodesired values.</span></span> <span data-ttu-id="f8667-174">Például:</span><span class="sxs-lookup"><span data-stu-id="f8667-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="f8667-175">Ez a paraméter lefoglalt halommemória tooJava 4GB memória, és is teszi rendezés hatékonyabb több memóriát oszt ki azt.</span><span class="sxs-lookup"><span data-stu-id="f8667-175">This parameter allocates 4GB memory tooJava heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="f8667-176">Azt nem egy jó ötlet tooplay a megoldást, ha a feladat sikertelen hibák kapcsolódó tooheap terület.</span><span class="sxs-lookup"><span data-stu-id="f8667-176">It is a good idea tooplay with these allocations if there are any job failure errors related tooheap space.</span></span>

1. <span data-ttu-id="f8667-177">**Az elosztott Fájlrendszerbeli blokkméret** : Ez a paraméter állandóként állítja be a hello legkisebb egység fájl rendszer tárolók hello adatok.</span><span class="sxs-lookup"><span data-stu-id="f8667-177">**DFS block size** : This parameter sets hello smallest unit of data that hello file system stores.</span></span> <span data-ttu-id="f8667-178">Tegyük fel hello elosztott Fájlrendszerbeli blokk mérete 128MB majd mérete adatot kevesebb mint, illetve a hierarchiában felfelé too128MB tárolódnak a egyetlen blokkot tartalmaz, amely nagyobb, mint 128MB számára engedélyezett a felesleges blokkok adatainak közben.</span><span class="sxs-lookup"><span data-stu-id="f8667-178">As an example, if hello DFS block size is 128MB, then any data of size less than and up too128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="f8667-179">Nagy terhek nagyon kis blokkméret kiválasztása hatására a Hadoop, mivel hello neve csomópont csak a tooprocess számos további kérelmeket toofind hello megfelelő blokk vonatkozó toohello fájl.</span><span class="sxs-lookup"><span data-stu-id="f8667-179">Choosing a very small block size causes large overheads in Hadoop since hello name node has tooprocess many more requests toofind hello relevant block pertaining toohello file.</span></span> <span data-ttu-id="f8667-180">A javasolt beállítás foglalkozó gigabájt (vagy nagyobb) adat:</span><span class="sxs-lookup"><span data-stu-id="f8667-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="f8667-181">**Hive join művelet optimalizálása** : közben hello térkép/csökkentése keretrendszer összekapcsolási műveletek általában kerül sor a hello fázisban, néha csökkentéséhez hatalmas növekedését elérhető illesztések ütemezésével hello térkép fázisban (más néven "mapjoins").</span><span class="sxs-lookup"><span data-stu-id="f8667-181">**Optimizing join operation in Hive** : While join operations in hello map/reduce framework typically take place in hello reduce phase, sometimes, enormous gains can be achieved by scheduling joins in hello map phase (also called "mapjoins").</span></span> <span data-ttu-id="f8667-182">toodirect struktúra toodo ez amikor csak lehetséges, azt állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="f8667-182">toodirect Hive toodo this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="f8667-183">**Mappers tooHive hello számát megadó** : közben Hadoop lehetővé teszi, hogy a hello felhasználói tooset hello száma szűkítő, hello száma mappers általában hello felhasználó nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="f8667-183">**Specifying hello number of mappers tooHive** : While Hadoop allows hello user tooset hello number of reducers, hello number of mappers is typically not be set by hello user.</span></span> <span data-ttu-id="f8667-184">Amely lehetővé teszi, hogy ez a szám a vezérlő bizonyos fokú körben toochoose hello Hadoop változók *mapred.min.split.size* és *mapred.max.split.size* minden térkép hello méreteként feladat határozza meg:</span><span class="sxs-lookup"><span data-stu-id="f8667-184">A trick that allows some degree of control on this number is toochoose hello Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as hello size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="f8667-185">Általában az alapértelmezett érték hello *mapred.min.split.size* 0, az *mapred.max.split.size* van **Long.MAX** és az *dfs.block.size* 64 MB.</span><span class="sxs-lookup"><span data-stu-id="f8667-185">Typically, hello default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="f8667-186">Láthatja, adott hello adatok mérete, mert ezek a paraméterek "beállítása" hangolása őket lehetővé teszi használt mappers tootune hello száma.</span><span class="sxs-lookup"><span data-stu-id="f8667-186">As we can see, given hello data size, tuning these parameters by "setting" them allows us tootune hello number of mappers used.</span></span>
4. <span data-ttu-id="f8667-187">Még más néhány **speciális beállítások** Hive optimalizálási teljesítmény alatt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="f8667-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="f8667-188">Ezek lehetővé teszik tooset hello lefoglalt memória toomap és csökkentse a feladatok, és elősegíti a teljesítmény tökéletesítse.</span><span class="sxs-lookup"><span data-stu-id="f8667-188">These allow you tooset hello memory allocated toomap and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="f8667-189">Ne feledje, hogy hello *mapreduce.reduce.memory.mb* nem lehet nagyobb, mint az egyes feldolgozó csomópontok hello Hadoop-fürt hello fizikai memória méretét.</span><span class="sxs-lookup"><span data-stu-id="f8667-189">Please keep in mind that hello *mapreduce.reduce.memory.mb* cannot be greater than hello physical memory size of each worker node in hello Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

