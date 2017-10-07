---
title: "a Hadoop aaaExplore adatok fürt, és az Azure Machine Learning modellek létrehozása |} Microsoft Docs"
description: "Hello Team adatok tudományos folyamat használja egy végpont forgatókönyv egy HDInsight Hadoop alkalmazó toobuild fürt, és nyilvánosan elérhető adatkészlet a modell rendszerbe állítása."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: a371032e356ffc366af0d6fbe364af281b6efd19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a><span data-ttu-id="294c1-103">hello Team adatok tudományos folyamat működés közben: használata Azure HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="294c1-103">hello Team Data Science Process in action: Use Azure HDInsight Hadoop clusters</span></span>
<span data-ttu-id="294c1-104">Ez a forgatókönyv használjuk hello [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md) egy végpont forgatókönyv használatával egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) toostore, vizsgálatát, és nyilvánosan funkció hello visszafejtés adatait rendelkezésre álló [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset és toodown az hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="294c1-104">In this walkthrough, we use hello [Team Data Science Process (TDSP)](data-science-process-overview.md) in an end-to-end scenario using an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore and feature engineer data from hello publicly available [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset, and toodown sample hello data.</span></span> <span data-ttu-id="294c1-105">Hello adatok modelljei épülnek Azure Machine Learning toohandle multiclass és bináris osztályozás és regressziós prediktív feladatok.</span><span class="sxs-lookup"><span data-stu-id="294c1-105">Models of hello data are built with Azure Machine Learning toohandle binary and multiclass classification and regression predictive tasks.</span></span>

<span data-ttu-id="294c1-106">Ez a forgatókönyv bemutatja, hogyan toohandle nagyobb (1 terabájtnál) adatkészletre vonatkozó hasonló forgatókönyvet használata a HDInsight Hadoop-fürtök az adatok feldolgozásához, lásd: [Team adatok tudományos folyamat - használata Azure HDInsight Hadoop-fürtök az 1 TB-os dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="294c1-106">For a walkthrough that shows how toohandle a larger (1 terabyte) dataset for a similar scenario using HDInsight Hadoop clusters for data processing, see [Team Data Science Process - Using Azure HDInsight Hadoop Clusters on a 1 TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span></span>

<span data-ttu-id="294c1-107">Akkor is lehetséges toouse egy IPython notebook tooaccomplish hello hello 1 TB-os adatkészlet bemutatott hello forgatókönyv feladatokat.</span><span class="sxs-lookup"><span data-stu-id="294c1-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented hello walkthrough using hello 1 TB dataset.</span></span> <span data-ttu-id="294c1-108">Felhasználók, akik volna, ez a megközelítés konzultáljon tootry például hello [Criteo forgatókönyv Hive ODBC-kapcsolat használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakör.</span><span class="sxs-lookup"><span data-stu-id="294c1-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="294c1-109"><a name="dataset"></a>NYC Taxi Utazgatással adatkészlet leírása</span><span class="sxs-lookup"><span data-stu-id="294c1-109"><a name="dataset"></a>NYC Taxi Trips Dataset description</span></span>
<span data-ttu-id="294c1-110">hello NYC Taxi út adatok körülbelül 20GB tömörített vesszővel tagolt (CSV) fájl (tömörítetlen ~ 48GB), amely több mint 173 millió egyedi utak és hello turistajegyek esetében minden út kifizette.</span><span class="sxs-lookup"><span data-stu-id="294c1-110">hello NYC Taxi Trip data is about 20GB of compressed comma-separated values (CSV) files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="294c1-111">Minden út rekord tartalmazza hello felvétel és Gyűjtőtár hely és idő, anonimizált rejthetők el (illesztőprogram) engedély száma és medallion (taxi tartozó egyedi azonosító) számát.</span><span class="sxs-lookup"><span data-stu-id="294c1-111">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="294c1-112">hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:</span><span class="sxs-lookup"><span data-stu-id="294c1-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="294c1-113">hello "trip_data" CSV-fájlok tartalmazzák a út részletek, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza.</span><span class="sxs-lookup"><span data-stu-id="294c1-113">hello 'trip_data' CSV files contain trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="294c1-114">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="294c1-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="294c1-115">hello "trip_fare" CSV-fájlok hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="294c1-115">hello 'trip_fare' CSV files contain details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="294c1-116">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="294c1-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="294c1-117">hello egyedi kulcs toojoin út\_adatok és út\_jegy ára áll hello mezők: medallion, rejthetők el\_engedély és a felvételi\_datetime.</span><span class="sxs-lookup"><span data-stu-id="294c1-117">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

<span data-ttu-id="294c1-118">minden tooget hello részletek vonatkozó tooa adott út, három kulccsal rendelkező elegendő toojoin: "medallion" hello "ellophatja\_licenc" és "a felvételi\_datetime".</span><span class="sxs-lookup"><span data-stu-id="294c1-118">tooget all of hello details relevant tooa particular trip, it is sufficient toojoin with three keys: hello "medallion", "hack\_license" and "pickup\_datetime".</span></span>

<span data-ttu-id="294c1-119">Néhány további részleteket hello adatok azt írják le, ha tároljuk őket a Hive táblák hamarosan.</span><span class="sxs-lookup"><span data-stu-id="294c1-119">We describe some more details of hello data when we store them into Hive tables shortly.</span></span>

## <span data-ttu-id="294c1-120"><a name="mltasks"></a>Előrejelzés feladatok példák</span><span class="sxs-lookup"><span data-stu-id="294c1-120"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="294c1-121">Majdnem elérte az adatokat, ha azt szeretné, hogy az elemzés alapján toomake előrejelzéseket hello típusú meghatározása segít elmagyarázza, hogy a folyamat tooinclude kell hello feladatok.</span><span class="sxs-lookup"><span data-stu-id="294c1-121">When approaching data, determining hello kind of predictions you want toomake based on its analysis helps clarify hello tasks that you will need tooinclude in your process.</span></span>
<span data-ttu-id="294c1-122">A forgatókönyv hello azon alapul, amelynek létrehozását oldjuk előrejelzés problémák három példa *tipp\_összeg*:</span><span class="sxs-lookup"><span data-stu-id="294c1-122">Here are three examples of prediction problems that we address in this walkthrough whose formulation is based on hello *tip\_amount*:</span></span>

1. <span data-ttu-id="294c1-123">**Bináris osztályozási**: előre jelezni, függetlenül attól, tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_Összeg* $ 0 egy negatív példában látható.</span><span class="sxs-lookup"><span data-stu-id="294c1-123">**Binary classification**: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. <span data-ttu-id="294c1-124">**Multiclass besorolási**: toopredict hello tartomány tipp díjak kifizette hello út.</span><span class="sxs-lookup"><span data-stu-id="294c1-124">**Multiclass classification**: toopredict hello range of tip amounts paid for hello trip.</span></span> <span data-ttu-id="294c1-125">A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:</span><span class="sxs-lookup"><span data-stu-id="294c1-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="294c1-126">**Regressziós feladat**: toopredict hello hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="294c1-126">**Regression task**: toopredict hello amount of hello tip paid for a trip.</span></span>  

## <span data-ttu-id="294c1-127"><a name="setup"></a>Állítson be egy HDInsight Hadoop-fürt speciális elemzésekre</span><span class="sxs-lookup"><span data-stu-id="294c1-127"><a name="setup"></a>Set up an HDInsight Hadoop cluster for advanced analytics</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-128">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-128">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-129">Speciális elemzésekre három lépést a HDInsight-fürtök által környezetet az Azure állíthat be:</span><span class="sxs-lookup"><span data-stu-id="294c1-129">You can set up an Azure environment for advanced analytics that employs an HDInsight cluster in three steps:</span></span>

1. <span data-ttu-id="294c1-130">[Hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md): ezt a tárfiókot az Azure Blob Storage adatok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="294c1-130">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used for storing data in Azure Blob Storage.</span></span> <span data-ttu-id="294c1-131">a HDInsight-fürtök hello adatait is itt található.</span><span class="sxs-lookup"><span data-stu-id="294c1-131">hello data used in HDInsight clusters also resides here.</span></span>
2. <span data-ttu-id="294c1-132">[Testre szabhatja az Azure HDInsight Hadoop fürtök a hello Advanced Analytics folyamat és a technológia](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="294c1-132">[Customize Azure HDInsight Hadoop clusters for hello Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md).</span></span> <span data-ttu-id="294c1-133">Ebben a lépésben létrehoz egy Azure HDInsight Hadoop 64 bites Anaconda Python 2.7 a fürt összes csomópontján telepíteni.</span><span class="sxs-lookup"><span data-stu-id="294c1-133">This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="294c1-134">Nincsenek két fontos lépések az tooremember a HDInsight-fürt testreszabása közben.</span><span class="sxs-lookup"><span data-stu-id="294c1-134">There are two important steps tooremember while customizing your HDInsight cluster.</span></span>
   
   * <span data-ttu-id="294c1-135">Ne felejtse el toolink hello tárfiók létrehozásakor az 1. lépésben a HDInsight-fürthöz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="294c1-135">Remember toolink hello storage account created in step 1 with your HDInsight cluster when creating it.</span></span> <span data-ttu-id="294c1-136">Ez a tárfiók használt tooaccess adatok feldolgozott hello fürtön belül.</span><span class="sxs-lookup"><span data-stu-id="294c1-136">This storage account is used tooaccess data that is processed within hello cluster.</span></span>
   * <span data-ttu-id="294c1-137">Hello fürt létrehozása után engedélyezze a távoli elérés toohello átjárócsomópont hello fürt.</span><span class="sxs-lookup"><span data-stu-id="294c1-137">After hello cluster is created, enable Remote Access toohello head node of hello cluster.</span></span> <span data-ttu-id="294c1-138">Keresse meg a toohello **konfigurációs** fülre, és kattintson **távoli engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="294c1-138">Navigate toohello **Configuration** tab and click **Enable Remote**.</span></span> <span data-ttu-id="294c1-139">Ez a lépés hello felhasználói hitelesítő adatok a távoli bejelentkezési határozza meg.</span><span class="sxs-lookup"><span data-stu-id="294c1-139">This step specifies hello user credentials used for remote login.</span></span>
3. <span data-ttu-id="294c1-140">[Hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md): az Azure Machine Learning munkaterület használt toobuild gépi tanulási modellek.</span><span class="sxs-lookup"><span data-stu-id="294c1-140">[Create an Azure Machine Learning workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used toobuild machine learning models.</span></span> <span data-ttu-id="294c1-141">Ez a feladat címzettjei egy kezdeti adatfeltárás befejezése után, és a mintavételi hello HDInsight-fürt használatával le.</span><span class="sxs-lookup"><span data-stu-id="294c1-141">This task is addressed after completing an initial data exploration and down sampling using hello HDInsight cluster.</span></span>

## <span data-ttu-id="294c1-142"><a name="getdata"></a>Egy nyilvános forrásból hello adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="294c1-142"><a name="getdata"></a>Get hello data from a public source</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-143">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-143">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-144">tooget hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset nyilvános helyéről, segítségével ismertetett hello-metódusok bármelyike [adatok áthelyezése tooand az Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello adatok tooyour gép.</span><span class="sxs-lookup"><span data-stu-id="294c1-144">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour machine.</span></span>

<span data-ttu-id="294c1-145">Itt azt ismertetik, hogyan AzCopy tootransfer hello használata-adatokat tartalmazó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="294c1-145">Here we describe how use AzCopy tootransfer hello files containing data.</span></span> <span data-ttu-id="294c1-146">toodownload és az AzCopy telepítési útmutatás alapján hello: [Ismerkedés az AzCopy parancssori segédprogram hello](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="294c1-146">toodownload and install AzCopy follow hello instructions at [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

1. <span data-ttu-id="294c1-147">Egy parancssori ablakot a hello AzCopy parancsok követően, hogy ki *< path_to_data_folder >* hello kívánt cél:</span><span class="sxs-lookup"><span data-stu-id="294c1-147">From a Command Prompt window, issue hello following AzCopy commands, replacing *<path_to_data_folder>* with hello desired destination:</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. <span data-ttu-id="294c1-148">Hello Másolás befejezése után összesen 24 tömörített fájlok kiválasztott hello adatok mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="294c1-148">When hello copy completes, a total of 24 zipped files are in hello data folder chosen.</span></span> <span data-ttu-id="294c1-149">Bontsa ki a letöltött hello fájlok toohello ugyanabban a könyvtárban a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="294c1-149">Unzip hello downloaded files toohello same directory on your local machine.</span></span> <span data-ttu-id="294c1-150">Jegyezze fel a tömörítetlen hello fájlokat tároló hello mappát.</span><span class="sxs-lookup"><span data-stu-id="294c1-150">Make a note of hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="294c1-151">Ez a mappa lesz hivatkozott tooas hello *< elérési út\_való\_unzipped_data\_fájlok\>*  van következik.</span><span class="sxs-lookup"><span data-stu-id="294c1-151">This folder will be referred tooas hello *<path\_to\_unzipped_data\_files\>* is what follows.</span></span>

## <span data-ttu-id="294c1-152"><a name="upload"></a>Töltse fel a hello adatok toohello alapértelmezett tároló Azure HDInsight Hadoop-fürt</span><span class="sxs-lookup"><span data-stu-id="294c1-152"><a name="upload"></a>Upload hello data toohello default container of Azure HDInsight Hadoop cluster</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-153">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-153">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-154">Hello következő AzCopy parancsok, cserélje le a következő értékekkel hello tényleges hello Hadoop-fürt létrehozásakor megadott paraméterek és hello adatfájlok kicsomagolás hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-154">In hello following AzCopy commands, replace hello following parameters with hello actual values that you specified when creating hello Hadoop cluster and unzipping hello data files.</span></span>

* <span data-ttu-id="294c1-155">***&#60; path_to_data_folder >*** hello könyvtár (együtt elérési utat) a hello unzipped adatfájljait tartalmazó számítógépre</span><span class="sxs-lookup"><span data-stu-id="294c1-155">***&#60;path_to_data_folder>*** hello directory (along with path) on your machine that contain hello unzipped data files</span></span>  
* <span data-ttu-id="294c1-156">***&#60; Hadoop-fürt tárfióknév >*** hello a HDInsight-fürthöz társított storage-fiók</span><span class="sxs-lookup"><span data-stu-id="294c1-156">***&#60;storage account name of Hadoop cluster>*** hello storage account associated with your HDInsight cluster</span></span>
* <span data-ttu-id="294c1-157">***&#60; alapértelmezett tároló Hadoop-fürt >*** hello alapértelmezett tároló a fürt által használt.</span><span class="sxs-lookup"><span data-stu-id="294c1-157">***&#60;default container of Hadoop cluster>*** hello default container used by your cluster.</span></span> <span data-ttu-id="294c1-158">Vegye figyelembe, hogy hello neve hello alapértelmezett tároló az általában hello azonos nevet maga hello fürtként.</span><span class="sxs-lookup"><span data-stu-id="294c1-158">Note that hello name of hello default container is usually hello same name as hello cluster itself.</span></span> <span data-ttu-id="294c1-159">Például ha hello fürt "abc123.azurehdinsight.net" nevezik, hello alapértelmezett tároló abc123.</span><span class="sxs-lookup"><span data-stu-id="294c1-159">For example, if hello cluster is called "abc123.azurehdinsight.net", hello default container is abc123.</span></span>
* <span data-ttu-id="294c1-160">***&#60; tárfiók kulcsa >*** hello kulcsának hello a fürt által használt</span><span class="sxs-lookup"><span data-stu-id="294c1-160">***&#60;storage account key>*** hello key for hello storage account used by your cluster</span></span>

<span data-ttu-id="294c1-161">A parancssorba vagy egy Windows PowerShell-ablakot a gépen futtassa a következő két AzCopy parancs hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-161">From a Command Prompt or a Windows PowerShell window in your machine, run hello following two AzCopy commands.</span></span>

<span data-ttu-id="294c1-162">Ez a parancs feltölt hello út adatok túl***nyctaxitripraw*** könyvtárban lévő hello alapértelmezett tároló hello Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="294c1-162">This command uploads hello trip data too***nyctaxitripraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

<span data-ttu-id="294c1-163">Ez a parancs feltölt hello jegy ára adatok túl***nyctaxifareraw*** könyvtárban lévő hello alapértelmezett tároló hello Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="294c1-163">This command uploads hello fare data too***nyctaxifareraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

<span data-ttu-id="294c1-164">hello adatok mostantól az Azure Blob Storage és készen áll a toobe hello HDInsight-fürtön belül kell.</span><span class="sxs-lookup"><span data-stu-id="294c1-164">hello data should now in Azure Blob Storage and ready toobe consumed within hello HDInsight cluster.</span></span>

## <span data-ttu-id="294c1-165"><a name="#download-hql-files"></a>Jelentkezzen be a Hadoop-fürt átjárócsomópontjához hello és és a felderítő adatelemzés előkészítése</span><span class="sxs-lookup"><span data-stu-id="294c1-165"><a name="#download-hql-files"></a>Log into hello head node of Hadoop cluster and and prepare for exploratory data analysis</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-166">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-166">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-167">hello fürt felderítő adatelemzés és a mintavételi hello adatok le tooaccess hello átjárócsomópont eljárással hello leírt [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="294c1-167">tooaccess hello head node of hello cluster for exploratory data analysis and down sampling of hello data, follow hello procedure outlined in [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

<span data-ttu-id="294c1-168">Ez a forgatókönyv elsősorban használjuk írt lekérdezések [Hive](https://hive.apache.org/), egy SQL-szerű lekérdező nyelv, tooperform előzetes adatok explorations.</span><span class="sxs-lookup"><span data-stu-id="294c1-168">In this walkthrough, we primarily use queries written in [Hive](https://hive.apache.org/), a SQL-like query language, tooperform preliminary data explorations.</span></span> <span data-ttu-id="294c1-169">hello Hive-lekérdezések .hql fájlok tárolják.</span><span class="sxs-lookup"><span data-stu-id="294c1-169">hello Hive queries are stored in .hql files.</span></span> <span data-ttu-id="294c1-170">A Microsoft majd lefelé minta az adatok toobe belül Azure Machine Learning modellek létrehozására használt.</span><span class="sxs-lookup"><span data-stu-id="294c1-170">We then down sample this data toobe used within Azure Machine Learning for building models.</span></span>

<span data-ttu-id="294c1-171">tooprepare hello fürt felderítő adatelemzéshez, letöltődnek hello .hql fájlokat tartalmazó hello vonatkozó Hive parancsfájlokat a [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa helyi könyvtárába (C:\temp) hello átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="294c1-171">tooprepare hello cluster for exploratory data analysis, we download hello .hql files containing hello relevant Hive scripts from [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa local directory (C:\temp) on hello head node.</span></span> <span data-ttu-id="294c1-172">toodo a, nyissa meg hello **parancssor** belül hello a fürt és a probléma a következő két parancsot hello hello átjárócsomópontjához:</span><span class="sxs-lookup"><span data-stu-id="294c1-172">toodo this, open hello **Command Prompt** from within hello head node of hello cluster and issue hello following two commands:</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="294c1-173">A két parancsok töltik le a forgatókönyv toohello helyi könyvtárban szükséges fájlokat .hql ***C:\temp &#92;*** hello központi csomópontban.</span><span class="sxs-lookup"><span data-stu-id="294c1-173">These two commands will download all .hql files needed in this walkthrough toohello local directory ***C:\temp&#92;*** in hello head node.</span></span>

## <span data-ttu-id="294c1-174"><a name="#hive-db-tables"></a>Hive-adatbázis és hónaponként particionált táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="294c1-174"><a name="#hive-db-tables"></a>Create Hive database and tables partitioned by month</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-175">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-175">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-176">Dolgozunk most már készen áll a toocreate Hive táblák a következőt: taxi adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="294c1-176">We are now ready toocreate Hive tables for our NYC taxi dataset.</span></span>
<span data-ttu-id="294c1-177">Hello Hadoop-fürt átjárócsomópontjához hello, nyissa meg hello ***Hadoop parancssori*** a hello hello átjárócsomópont asztalát, és írja be a hello Hive directory hello parancs megadásával</span><span class="sxs-lookup"><span data-stu-id="294c1-177">In hello head node of hello Hadoop cluster, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="294c1-178">**Ez a forgatókönyv minden Hive parancsot futtatja hello fent Hive bin / directory kérdés. Ez az elérési út probléma merül fel automatikusan kezeli. "Struktúra directory prompt" hello kifejezéseket használjuk a "struktúra bin / directory prompt", és a "Hadoop parancssori" azonos értelemben ebben a bemutatóban.**</span><span class="sxs-lookup"><span data-stu-id="294c1-178">**Run all Hive commands in this walkthrough from hello above Hive bin/ directory prompt. This will take care of any path issues automatically. We use hello terms "Hive directory prompt", "Hive bin/ directory prompt",  and "Hadoop Command Line" interchangeably in this walkthrough.**</span></span>
> 
> 

<span data-ttu-id="294c1-179">Hello Hive directory parancssorába írja be a következő parancsot a Hadoop parancssori hello átjárócsomópont toosubmit hello Hive lekérdezés toocreate Hive-adatbázis és táblák hello:</span><span class="sxs-lookup"><span data-stu-id="294c1-179">From hello Hive directory prompt, enter hello following command in Hadoop Command Line of hello head node toosubmit hello Hive query toocreate Hive database and tables:</span></span>

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

<span data-ttu-id="294c1-180">Íme hello hello tartalmának ***C:\temp\sample\_hive\_létrehozása\_db\_és\_tables.hql*** fájl, amelyhez Hive adatbázist hoz létre ***nyctaxidb *** és táblák ***út*** és ***jegy ára***.</span><span class="sxs-lookup"><span data-stu-id="294c1-180">Here is hello content of hello ***C:\temp\sample\_hive\_create\_db\_and\_tables.hql*** file which creates Hive database ***nyctaxidb*** and tables ***trip*** and ***fare***.</span></span>

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

<span data-ttu-id="294c1-181">A Hive parancsfájl hoz létre két tábla:</span><span class="sxs-lookup"><span data-stu-id="294c1-181">This Hive script creates two tables:</span></span>

* <span data-ttu-id="294c1-182">hello "út" tábla minden egyes menethelyzetben (illesztőprogram adatai, felvételi idő, út távolság és időpontok) út részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="294c1-182">hello "trip" table contains trip details of each ride (driver details, pickup time, trip distance and times)</span></span>
* <span data-ttu-id="294c1-183">hello "jegy ára" tábla (jegy ára, tipp összegre, autópályadíjak és pótdíjak) jegy ára részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="294c1-183">hello "fare" table contains fare details (fare amount, tip amount, tolls and surcharges).</span></span>

<span data-ttu-id="294c1-184">Ha ezekkel az eljárásokkal semmilyen további segítségre van szüksége, vagy tooinvestigate alternatív néhányat a meglévők közül, című hello [elküldeni a Hive-lekérdezések közvetlenül a hello Hadoop parancssori ](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="294c1-184">If you need any additional assistance with these procedures or want tooinvestigate alternative ones, see hello section [Submit Hive queries directly from hello Hadoop Command Line ](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="294c1-185"><a name="#load-data"></a>Partíció tooHive adattáblák betöltése</span><span class="sxs-lookup"><span data-stu-id="294c1-185"><a name="#load-data"></a>Load Data tooHive tables by partitions</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-186">Ez a művelet rendszerint egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-186">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="294c1-187">hello NYC taxi dataset adatkészletben, a természetes particionálás havonta, amelyek tooenable feldolgozásával és lekérdezéseivel gyorsabb használatára.</span><span class="sxs-lookup"><span data-stu-id="294c1-187">hello NYC taxi dataset has a natural partitioning by month, which we use tooenable faster processing and query times.</span></span> <span data-ttu-id="294c1-188">az alábbi PowerShell-parancsok hello (hello segítségével hello Hive könyvtárból kiadott **Hadoop parancssori**) adatok toohello "út" és "jegy ára" Hive táblák havonta particionálva betölteni.</span><span class="sxs-lookup"><span data-stu-id="294c1-188">hello PowerShell commands below (issued from hello Hive directory using hello **Hadoop Command Line**) load data toohello "trip" and "fare" Hive tables partitioned by month.</span></span>

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

<span data-ttu-id="294c1-189">Hello *minta\_hive\_betölteni\_adatok\_által\_partitions.hql* fájl tartalmazza-e hello alábbi **betöltése** parancsok.</span><span class="sxs-lookup"><span data-stu-id="294c1-189">hello *sample\_hive\_load\_data\_by\_partitions.hql* file contains hello following **LOAD** commands.</span></span>

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

<span data-ttu-id="294c1-190">Vegye figyelembe, hogy egy használjuk itt hello feltárási folyamata a Hive-lekérdezések száma is magában foglalja, csupán egyetlen partícióra vagy csak néhány partíciókat.</span><span class="sxs-lookup"><span data-stu-id="294c1-190">Note that a number of Hive queries we use here in hello exploration process involve looking at just a single partition or at only a couple of partitions.</span></span> <span data-ttu-id="294c1-191">De ezeket a lekérdezéseket futtathat hello teljes adatok között.</span><span class="sxs-lookup"><span data-stu-id="294c1-191">But these queries could be run across hello entire data.</span></span>

### <span data-ttu-id="294c1-192"><a name="#show-db"></a>A HDInsight Hadoop-fürt hello adatbázisok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="294c1-192"><a name="#show-db"></a>Show databases in hello HDInsight Hadoop cluster</span></span>
<span data-ttu-id="294c1-193">a HDInsight Hadoop-fürt hello Hadoop parancssori ablakban futtassa a következő parancsot a Hadoop parancssori hello belül létrehozott tooshow hello adatbázisok:</span><span class="sxs-lookup"><span data-stu-id="294c1-193">tooshow hello databases created in HDInsight Hadoop cluster inside hello Hadoop Command Line window, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show databases;"

### <span data-ttu-id="294c1-194"><a name="#show-tables"></a>Hello nyctaxidb adatbázis hello Hive táblák megjelenítése</span><span class="sxs-lookup"><span data-stu-id="294c1-194"><a name="#show-tables"></a>Show hello Hive tables in hello nyctaxidb database</span></span>
<span data-ttu-id="294c1-195">tooshow hello táblák hello nyctaxidb adatbázisban, futtassa a következő parancsot a Hadoop parancssori hello:</span><span class="sxs-lookup"><span data-stu-id="294c1-195">tooshow hello tables in hello nyctaxidb database, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show tables in nyctaxidb;"

<span data-ttu-id="294c1-196">Azt is meggyőződhet arról, hogy hello táblák hello az alábbi parancsot a vannak-e particionálni:</span><span class="sxs-lookup"><span data-stu-id="294c1-196">We can confirm that hello tables are partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.trip;"

<span data-ttu-id="294c1-197">hello várható kimenete az alább látható:</span><span class="sxs-lookup"><span data-stu-id="294c1-197">hello expected output is shown below:</span></span>

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

<span data-ttu-id="294c1-198">Ehhez hasonlóan azt is győződjön meg arról, hogy hello jegy ára tábla particionálva van hello az alábbi parancsot a:</span><span class="sxs-lookup"><span data-stu-id="294c1-198">Similarly, we can ensure that hello fare table is partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.fare;"

<span data-ttu-id="294c1-199">hello várható kimenete az alább látható:</span><span class="sxs-lookup"><span data-stu-id="294c1-199">hello expected output is shown below:</span></span>

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <span data-ttu-id="294c1-200"><a name="#explore-hive"></a>Az adatok feltárása és Hive mérnöki szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="294c1-200"><a name="#explore-hive"></a>Data exploration and feature engineering in Hive</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-201">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-201">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-202">az adatok feltárása hello és mérnöki feladatok adatok hello Hive táblák töltve a Hive-lekérdezések használatával valósítható hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="294c1-202">hello data exploration and feature engineering tasks for hello data loaded into hello Hive tables can be accomplished using Hive queries.</span></span> <span data-ttu-id="294c1-203">Az alábbiakban például olyan feladatokat, hogy azt végigvezetik Önt ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="294c1-203">Here are examples of such tasks that we walk you through in this section:</span></span>

* <span data-ttu-id="294c1-204">A két tábla hello felső 10 rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="294c1-204">View hello top 10 records in both tables.</span></span>
* <span data-ttu-id="294c1-205">Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.</span><span class="sxs-lookup"><span data-stu-id="294c1-205">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="294c1-206">Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.</span><span class="sxs-lookup"><span data-stu-id="294c1-206">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="294c1-207">Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="294c1-207">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="294c1-208">Szolgáltatások elő számítástechnikai hello közvetlen út távolság.</span><span class="sxs-lookup"><span data-stu-id="294c1-208">Generate features by computing hello direct trip distances.</span></span>

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a><span data-ttu-id="294c1-209">Feltárása: Tábla út hello felső 10 rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="294c1-209">Exploration: View hello top 10 records in table trip</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-210">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-210">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-211">milyen hello adatok a következőképpen néz toosee, azt vizsgálja meg a 10 rögzíti az összes táblából.</span><span class="sxs-lookup"><span data-stu-id="294c1-211">toosee what hello data looks like, we examine 10 records from each table.</span></span> <span data-ttu-id="294c1-212">Futtassa a következő két lekérdezések külön-külön parancssorból hello Hive directory rekordokban hello Hadoop parancssori konzol tooinspect hello hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-212">Run hello following two queries separately from hello Hive directory prompt in hello Hadoop Command Line console tooinspect hello records.</span></span>

<span data-ttu-id="294c1-213">tooget hello felső 10 rekordok hello tábla "út" a hónap első hello:</span><span class="sxs-lookup"><span data-stu-id="294c1-213">tooget hello top 10 records in hello table "trip" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

<span data-ttu-id="294c1-214">tooget hello felső 10 rekordot hello tábla "díjszabás" hello az első hónapja:</span><span class="sxs-lookup"><span data-stu-id="294c1-214">tooget hello top 10 records in hello table "fare" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

<span data-ttu-id="294c1-215">Ez a legtöbbször hasznos toosave hello rekordok tooa fájl kényelmes megtekintésre.</span><span class="sxs-lookup"><span data-stu-id="294c1-215">It is often useful toosave hello records tooa file for convenient viewing.</span></span> <span data-ttu-id="294c1-216">Ezt egy kis változást toohello fent lekérdezés éri el:</span><span class="sxs-lookup"><span data-stu-id="294c1-216">A small change toohello above query accomplishes this:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a><span data-ttu-id="294c1-217">Feltárása: Hello rekordok számának megjelenítése az egyes hello 12 partíciók</span><span class="sxs-lookup"><span data-stu-id="294c1-217">Exploration: View hello number of records in each of hello 12 partitions</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-218">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-218">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-219">Fontos van hello hogyan utazgatással hello száma változik hello naptári év során.</span><span class="sxs-lookup"><span data-stu-id="294c1-219">Of interest is hello how hello number of trips varies during hello calendar year.</span></span> <span data-ttu-id="294c1-220">Havonta csoportosítási lehetővé teszi toosee ehhez a terjesztéshez utak néz.</span><span class="sxs-lookup"><span data-stu-id="294c1-220">Grouping by month allows us toosee what this distribution of trips looks like.</span></span>

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

<span data-ttu-id="294c1-221">Ez biztosítanak számunkra hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="294c1-221">This gives us hello output :</span></span>

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

<span data-ttu-id="294c1-222">Itt hello első oszlop hello hónap pedig hello második utazgatással hello száma az adott hónapban.</span><span class="sxs-lookup"><span data-stu-id="294c1-222">Here, hello first column is hello month and hello second is hello number of trips for that month.</span></span>

<span data-ttu-id="294c1-223">Azt is is száma hello rekordok teljes számát a út adatkészlet a következő parancs hello Hive könyvtár parancssorába hello kiállításával.</span><span class="sxs-lookup"><span data-stu-id="294c1-223">We can also count hello total number of records in our trip data set by issuing hello following command at hello Hive directory prompt.</span></span>

    hive -e "select count(*) from nyctaxidb.trip;"

<span data-ttu-id="294c1-224">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="294c1-224">This yields:</span></span>

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

<span data-ttu-id="294c1-225">Parancsok hasonló toothose hello út adatkészletnél látható azt kiadhatnak Hive-lekérdezések hello Hive directory parancssorból hello jegy ára adatkészlet toovalidate hello rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="294c1-225">Using commands similar toothose shown for hello trip data set, we can issue Hive queries from hello Hive directory prompt for hello fare data set toovalidate hello number of records.</span></span>

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

<span data-ttu-id="294c1-226">Ez biztosítanak számunkra hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="294c1-226">This gives us hello output:</span></span>

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

<span data-ttu-id="294c1-227">Figyelje meg, hogy mindkét adatkészletek visszaküldött hello pontos azonos számú havonta való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="294c1-227">Note that hello exact same number of trips per month is returned for both data sets.</span></span> <span data-ttu-id="294c1-228">Ez lehetővé teszi hello első ellenőrzése, hogy megfelelően lett betöltve adatok hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-228">This provides hello first validation that hello data has been loaded correctly.</span></span>

<span data-ttu-id="294c1-229">Hello jegy ára adatkészlet-rekordok teljes száma hello számbavételi végezhető paranccsal hello alatt hello Hive directory parancssorból:</span><span class="sxs-lookup"><span data-stu-id="294c1-229">Counting hello total number of records in hello fare data set can be done using hello command below from hello Hive directory prompt:</span></span>

    hive -e "select count(*) from nyctaxidb.fare;"

<span data-ttu-id="294c1-230">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="294c1-230">This yields :</span></span>

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

<span data-ttu-id="294c1-231">a két tábla rekordok teljes számát hello is hello azonos.</span><span class="sxs-lookup"><span data-stu-id="294c1-231">hello total number of records in both tables is also hello same.</span></span> <span data-ttu-id="294c1-232">Ez lehetővé teszi a második ellenőrzési adott hello adatok helyesen betöltése.</span><span class="sxs-lookup"><span data-stu-id="294c1-232">This provides a second validation that hello data has been loaded correctly.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="294c1-233">Feltárása: Út elosztása medallion szerint</span><span class="sxs-lookup"><span data-stu-id="294c1-233">Exploration: Trip distribution by medallion</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-234">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-234">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-235">Ebben a példában hello medallion (taxi számok) azonosítja a 100-nál több való adatváltások számát egy adott időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="294c1-235">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="294c1-236">hello lekérdezés számos előnyt biztosít az hello particionált tábla hozzáférés óta, akkor annak hello partíció változó által **hónap**.</span><span class="sxs-lookup"><span data-stu-id="294c1-236">hello query benefits from hello partitioned table access since it is conditioned by hello partition variable **month**.</span></span> <span data-ttu-id="294c1-237">hello lekérdezési eredmények nyelven íródtak tooa helyi fájl queryoutput.tsv `C:\temp` hello központi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="294c1-237">hello query results are written tooa local file queryoutput.tsv in `C:\temp` on hello head node.</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="294c1-238">Íme hello tartalmának *minta\_hive\_út\_száma\_által\_medallion.hql* fájl a vizsgálathoz.</span><span class="sxs-lookup"><span data-stu-id="294c1-238">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="294c1-239">hello medallion hello NYC taxi adathalmaz egyedi cab azonosítja.</span><span class="sxs-lookup"><span data-stu-id="294c1-239">hello medallion in hello NYC taxi data set identifies a unique cab.</span></span> <span data-ttu-id="294c1-240">Azt is azonosíthatja, hogy mely kabinetfájlok "foglalt" azzal, hogy melyik végzett több mint egy bizonyos számú való adatváltások számát egy adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="294c1-240">We can identify which cabs are "busy" by asking which ones made more than a certain number of trips in a particular time period.</span></span> <span data-ttu-id="294c1-241">hello alábbi példa azonosítja kabinetfájlok végrehajtott egynél több száz helymódosításra utazgatással hello első három hónap, és menti a lekérdezési eredmények tooa helyi fájl esetén C:\temp\queryoutput.tsv hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-241">hello following example identifies cabs that made more than a hundred trips in hello first three months, and saves hello query results tooa local file, C:\temp\queryoutput.tsv.</span></span>

<span data-ttu-id="294c1-242">Íme hello tartalmának *minta\_hive\_út\_száma\_által\_medallion.hql* fájl a vizsgálathoz.</span><span class="sxs-lookup"><span data-stu-id="294c1-242">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="294c1-243">A hello struktúra directory parancssorba probléma hello parancs az alábbi:</span><span class="sxs-lookup"><span data-stu-id="294c1-243">From hello Hive directory prompt, issue hello command below :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="294c1-244">Feltárása: Út terjesztési medallion és hack_license</span><span class="sxs-lookup"><span data-stu-id="294c1-244">Exploration: Trip distribution by medallion and hack_license</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-245">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-245">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-246">Ha dataset felfedezése, gyakran szeretnénk co-csoportokat előfordulásainak száma tooexamine hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-246">When exploring a dataset, we frequently want tooexamine hello number of co-occurences of groups of values.</span></span> <span data-ttu-id="294c1-247">Ebben a szakaszban adja meg, hogyan toodo ezt kabinetfájlokban és illesztőprogramok egy példát.</span><span class="sxs-lookup"><span data-stu-id="294c1-247">This section provide an example of how toodo this for cabs and drivers.</span></span>

<span data-ttu-id="294c1-248">Hello *minta\_hive\_út\_száma\_által\_medallion\_license.hql* fájlcsoportok hello jegy ára adatok megadása "medallion" és "hack_license" és az egyes kombinációkban számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="294c1-248">hello *sample\_hive\_trip\_count\_by\_medallion\_license.hql* file groups hello fare data set on "medallion" and "hack_license" and returns counts of each combination.</span></span> <span data-ttu-id="294c1-249">Az alábbiakban annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="294c1-249">Below are its contents.</span></span>

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

<span data-ttu-id="294c1-250">A lekérdezés által visszaadott cab-fájl és az adott illesztőprogram kombinációját utazgatással csökkenő száma alapján rendezve.</span><span class="sxs-lookup"><span data-stu-id="294c1-250">This query returns cab and particular driver combinations ordered by descending number of trips.</span></span>

<span data-ttu-id="294c1-251">A hello Hive directory kérdés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="294c1-251">From hello Hive directory prompt, run :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="294c1-252">hello lekérdezési eredmények tooa helyi fájl C:\temp\queryoutput.tsv készültek.</span><span class="sxs-lookup"><span data-stu-id="294c1-252">hello query results are written tooa local file C:\temp\queryoutput.tsv.</span></span>

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a><span data-ttu-id="294c1-253">Feltárása: Értékeli az adatminőségi érvénytelen hosszúság vagy szélességi rekordok ellenőrzésével</span><span class="sxs-lookup"><span data-stu-id="294c1-253">Exploration: Assessing data quality by checking for invalid longitude/latitude records</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-254">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-254">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-255">A felderítő adatelemzés közös célja tooweed kimenő érvénytelen vagy hibás rögzíti.</span><span class="sxs-lookup"><span data-stu-id="294c1-255">A common objective of exploratory data analysis is tooweed out invalid or bad records.</span></span> <span data-ttu-id="294c1-256">hello jelen szakaszban ismertetett példa meghatározza, hogy vagy hello hosszúsági és szélességi mezőket tartalmaz értéket sokkal hello NYC területen kívül.</span><span class="sxs-lookup"><span data-stu-id="294c1-256">hello example in this section determines whether either hello longitude or latitude fields contain a value far outside hello NYC area.</span></span> <span data-ttu-id="294c1-257">Valószínű, hogy az ilyen bejegyzések rendelkezik-e egy hibás hosszúság-szélességi értékeknek, mivel azt szeretnénk, ha azokat olyan adatot, amely toobe modellezési használt tooeliminate.</span><span class="sxs-lookup"><span data-stu-id="294c1-257">Since it is likely that such records have an erroneous longitude-latitude values, we want tooeliminate them from any data that is toobe used for modeling.</span></span>

<span data-ttu-id="294c1-258">Íme hello tartalmának *minta\_hive\_minőségi\_assessment.hql* fájl a vizsgálathoz.</span><span class="sxs-lookup"><span data-stu-id="294c1-258">Here is hello content of *sample\_hive\_quality\_assessment.hql* file for inspection.</span></span>

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


<span data-ttu-id="294c1-259">A hello Hive directory kérdés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="294c1-259">From hello Hive directory prompt, run :</span></span>

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

<span data-ttu-id="294c1-260">Hello *-S* argumentum, a parancsban benne mellőzi hello állapota képernyőn nyomtatás hello térkép/csökkentse Hive-feladatok.</span><span class="sxs-lookup"><span data-stu-id="294c1-260">hello *-S* argument included in this command suppresses hello status screen printout of hello Hive Map/Reduce jobs.</span></span> <span data-ttu-id="294c1-261">Ez akkor hasznos, mert így hello képernyő nyomtatása hello Hive lekérdezés kimeneti olvashatóbb.</span><span class="sxs-lookup"><span data-stu-id="294c1-261">This is useful because it makes hello screen print of hello Hive query output more readable.</span></span>

### <a name="exploration-binary-class-distributions-of-trip-tips"></a><span data-ttu-id="294c1-262">Feltárása: Út tippek bináris osztály disztribúciók</span><span class="sxs-lookup"><span data-stu-id="294c1-262">Exploration: Binary class distributions of trip tips</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-263">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-263">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-264">Hello bináris osztályozási problémához hello leírt [előrejelzés feladatok példái](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakaszban is hasznos tooknow tipp vagy nem kapott-e.</span><span class="sxs-lookup"><span data-stu-id="294c1-264">For hello binary classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section, it is useful tooknow whether a tip was given or not.</span></span> <span data-ttu-id="294c1-265">A terjesztés tippek az bináris:</span><span class="sxs-lookup"><span data-stu-id="294c1-265">This distribution of tips is binary:</span></span>

* <span data-ttu-id="294c1-266">Tipp megadott (osztály 1, tipp\_összeg > $0)</span><span class="sxs-lookup"><span data-stu-id="294c1-266">tip given(Class 1, tip\_amount > $0)</span></span>  
* <span data-ttu-id="294c1-267">nincs ötlet (osztály: 0, tipp\_összeg = 0).</span><span class="sxs-lookup"><span data-stu-id="294c1-267">no tip (Class 0, tip\_amount = $0).</span></span>

<span data-ttu-id="294c1-268">Hello *minta\_hive\_Formabontó\_frequencies.hql* fájlsablont ezt végzi.</span><span class="sxs-lookup"><span data-stu-id="294c1-268">hello *sample\_hive\_tipped\_frequencies.hql* file shown below does this.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

<span data-ttu-id="294c1-269">A hello Hive directory kérdés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="294c1-269">From hello Hive directory prompt, run:</span></span>

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a><span data-ttu-id="294c1-270">Feltárása: Hello multiclass beállításban osztály disztribúciók</span><span class="sxs-lookup"><span data-stu-id="294c1-270">Exploration: Class distributions in hello multiclass setting</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-271">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-271">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-272">Hello multiclass osztályozási problémához hello leírt [előrejelzés feladatok példái](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakasz adatkészlet is adatmodelljeinek tooa természetes besorolás, ahol szeretnénk megadott hello tippek toopredict hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="294c1-272">For hello multiclass classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section this data set also lends itself tooa natural classification where we would like toopredict hello amount of hello tips given.</span></span> <span data-ttu-id="294c1-273">Használhatunk bins toodefine tipp tartományok hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="294c1-273">We can use bins toodefine tip ranges in hello query.</span></span> <span data-ttu-id="294c1-274">tooget hello osztály terjesztéseket hello a különböző tipp tartományokat, hello használjuk *minta\_hive\_tipp\_tartomány\_frequencies.hql* fájlt.</span><span class="sxs-lookup"><span data-stu-id="294c1-274">tooget hello class distributions for hello various tip ranges, we use hello *sample\_hive\_tip\_range\_frequencies.hql* file.</span></span> <span data-ttu-id="294c1-275">Az alábbiakban annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="294c1-275">Below are its contents.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

<span data-ttu-id="294c1-276">Futtassa a parancsot követő Hadoop parancssori konzolról hello:</span><span class="sxs-lookup"><span data-stu-id="294c1-276">Run hello following command from Hadoop Command Line console:</span></span>

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a><span data-ttu-id="294c1-277">Feltárása: Számítási közvetlen hosszúság-szélesség két hely közötti távolság szerint</span><span class="sxs-lookup"><span data-stu-id="294c1-277">Exploration: Compute Direct Distance Between Two Longitude-Latitude Locations</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-278">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-278">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-279">Amelyek mérték hello közvetlen távolság lehetővé teszi ki hello eltérést és hello közötti toofind tényleges út távolságot.</span><span class="sxs-lookup"><span data-stu-id="294c1-279">Having a measure of hello direct distance allows us toofind out hello discrepancy between it and hello actual trip distance.</span></span> <span data-ttu-id="294c1-280">Ez a funkció jelenleg rávenni jelzésével utas lehet kisebb, valószínűleg tootip, ha azok mérje fel, hogy hello illesztőprogram szándékosan tartott őket egy sokkal hosszabb útvonalanként.</span><span class="sxs-lookup"><span data-stu-id="294c1-280">We motivate this feature by pointing out that a passenger might be less likely tootip if they figure out that hello driver has intentionally taken them by a much longer route.</span></span>

<span data-ttu-id="294c1-281">toosee hello tényleges út távolság és összehasonlítása hello [Haversine távolság](http://en.wikipedia.org/wiki/Haversine_formula) két hosszúság-szélesség pontja (hello "nagy kör" távolság), használjuk hello trigonometriai elérhető belül struktúra, így:</span><span class="sxs-lookup"><span data-stu-id="294c1-281">toosee hello comparison between actual trip distance and hello [Haversine distance](http://en.wikipedia.org/wiki/Haversine_formula) between two longitude-latitude points (hello "great circle" distance), we use hello trigonometric functions available within Hive, thus :</span></span>

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

<span data-ttu-id="294c1-282">A fenti hello lekérdezés R miles a föld hello hello sugara, továbbá pi konvertált tooradians.</span><span class="sxs-lookup"><span data-stu-id="294c1-282">In hello query above, R is hello radius of hello Earth in miles, and pi is converted tooradians.</span></span> <span data-ttu-id="294c1-283">Ne feledje, hogy hello hosszúság-szélesség pontok hello NYC terület messze "szűrt" tooremove értékeket.</span><span class="sxs-lookup"><span data-stu-id="294c1-283">Note that hello longitude-latitude points are "filtered" tooremove values that are far from hello NYC area.</span></span>

<span data-ttu-id="294c1-284">Ebben az esetben az eredmények tooa könyvtárat "queryoutputdir" azt írni.</span><span class="sxs-lookup"><span data-stu-id="294c1-284">In this case, we write our results tooa directory called "queryoutputdir".</span></span> <span data-ttu-id="294c1-285">hello parancssorrend alább látható először hoz létre a kimeneti könyvtárhoz, és hello Hive parancsot futtatja, majd.</span><span class="sxs-lookup"><span data-stu-id="294c1-285">hello sequence of commands shown below first creates this output directory, and then runs hello Hive command.</span></span>

<span data-ttu-id="294c1-286">A hello Hive directory kérdés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="294c1-286">From hello Hive directory prompt, run:</span></span>

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


<span data-ttu-id="294c1-287">hello lekérdezési eredmények írt too9 Azure-blobok ***queryoutputdir/000000\_0*** túl ***queryoutputdir/000008\_0*** hello alapértelmezett tároló hello Hadoop-fürt alatt.</span><span class="sxs-lookup"><span data-stu-id="294c1-287">hello query results are written too9 Azure blobs ***queryoutputdir/000000\_0*** too ***queryoutputdir/000008\_0*** under hello default container of hello Hadoop cluster.</span></span>

<span data-ttu-id="294c1-288">hello egyes blobok toosee hello méretét, Futtatás hello parancs hello Hive directory parancssorból a következő:</span><span class="sxs-lookup"><span data-stu-id="294c1-288">toosee hello size of hello individual blobs, we run hello following command from hello Hive directory prompt :</span></span>

    hdfs dfs -ls wasb:///queryoutputdir

<span data-ttu-id="294c1-289">a megadott fájl tartalmát toosee hello 000000 mondja ki\_0, használjuk a Hadoop által `copyToLocal` parancsban, így.</span><span class="sxs-lookup"><span data-stu-id="294c1-289">toosee hello contents of a given file, say 000000\_0, we use Hadoop's `copyToLocal` command, thus.</span></span>

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> <span data-ttu-id="294c1-290">`copyToLocal`nagyon nagy méretű fájlok esetén lassú lehet, és nem ajánlott azokat való használatra.</span><span class="sxs-lookup"><span data-stu-id="294c1-290">`copyToLocal` can be very slow for large files, and is not recommended for use with them.</span></span>  
> 
> 

<span data-ttu-id="294c1-291">A kulcs az adatok találhatók az Azure blob előnye, hogy azt is adatokba hello Azure Machine Learning segítségével hello belül [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="294c1-291">A key advantage of having this data reside in an Azure blob is that we may explore hello data within Azure Machine Learning using hello [Import Data][import-data] module.</span></span>

## <span data-ttu-id="294c1-292"><a name="#downsample"></a>Adatok és -buildek mintákat az Azure Machine Learning le</span><span class="sxs-lookup"><span data-stu-id="294c1-292"><a name="#downsample"></a>Down sample data and build models in Azure Machine Learning</span></span>
> [!NOTE]
> <span data-ttu-id="294c1-293">Ez a művelet rendszerint egy **adatok tudósok** feladat.</span><span class="sxs-lookup"><span data-stu-id="294c1-293">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="294c1-294">Hello felderítő adatok elemzési fázis után dolgozunk most már készen áll a toodown hello adatot az Azure Machine Learning modellek készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="294c1-294">After hello exploratory data analysis phase, we are now ready toodown sample hello data for building models in Azure Machine Learning.</span></span> <span data-ttu-id="294c1-295">Ebben a szakaszban megmutatjuk, hogyan toouse egy Hive lekérdezés toodown hello mintaadatok, amely majd elérése hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="294c1-295">In this section, we show how toouse a Hive query toodown sample hello data, which is then accessed from hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

### <a name="down-sampling-hello-data"></a><span data-ttu-id="294c1-296">Lefelé hello az adatok mintavétele</span><span class="sxs-lookup"><span data-stu-id="294c1-296">Down sampling hello data</span></span>
<span data-ttu-id="294c1-297">Az alábbi eljárással két lépésben történik.</span><span class="sxs-lookup"><span data-stu-id="294c1-297">There are two steps in this procedure.</span></span> <span data-ttu-id="294c1-298">Hello csatlakoztassa azt **nyctaxidb.trip** és **nyctaxidb.fare** táblák összes rekord található három kulcs: "medallion", "ellophatja\_licenc", és "a felvételi\_datetime".</span><span class="sxs-lookup"><span data-stu-id="294c1-298">First we join hello **nyctaxidb.trip** and **nyctaxidb.fare** tables on three keys that are present in all records : "medallion", "hack\_license", and "pickup\_datetime".</span></span> <span data-ttu-id="294c1-299">A Microsoft létrehozza a bináris besorolási címkék **Formabontó** és több osztály besorolási címkék **tipp\_osztály**.</span><span class="sxs-lookup"><span data-stu-id="294c1-299">We then generate a binary classification label **tipped** and a multi-class classification label **tip\_class**.</span></span>

<span data-ttu-id="294c1-300">toobe képes toouse hello le a mintaadatokat közvetlenül a hello [és adatokat importálhat] [ import-data] modul az Azure Machine Learning szükséges toostore hello eredményeit hello lekérdezés tooan belső struktúra táblázat felett.</span><span class="sxs-lookup"><span data-stu-id="294c1-300">toobe able toouse hello down sampled data directly from hello [Import Data][import-data] module in Azure Machine Learning, it is necessary toostore hello results of hello above query tooan internal Hive table.</span></span> <span data-ttu-id="294c1-301">A következő azt egy belső Hive tábla létrehozásához és tölthet tartalmának csatlakoztatott hello és le a mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="294c1-301">In what follows, we create an internal Hive table and populate its contents with hello joined and down sampled data.</span></span>

<span data-ttu-id="294c1-302">hello lekérdezés vonatkozik közvetlenül toogenerate hello óra, nap, hét év, hétköznap (hétfő jelző 1 és 7 jelző vasárnap) hello a szabványos Hive funkciók "felvétel\_dátum és idő" mező, és hello felvétel és dropoff közvetlen távolságát hello helyek.</span><span class="sxs-lookup"><span data-stu-id="294c1-302">hello query applies standard Hive functions directly toogenerate hello hour of day, week of year, weekday (1 stands for Monday, and 7 stands for Sunday) from hello "pickup\_datetime" field,  and hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="294c1-303">Felhasználók túl hivatkozhat[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) ilyen funkciók teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="294c1-303">Users can refer too[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for a complete list of such functions.</span></span>

<span data-ttu-id="294c1-304">lekérdezés hello és le minták hello adatokat, hogy a lekérdezés eredményei hello elfér hello Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="294c1-304">hello query then down samples hello data so that hello query results can fit into hello Azure Machine Learning Studio.</span></span> <span data-ttu-id="294c1-305">Hello eredeti adathalmazból csak körülbelül 1 %-át hello Studio importálta.</span><span class="sxs-lookup"><span data-stu-id="294c1-305">Only about 1% of hello original dataset is imported into hello Studio.</span></span>

<span data-ttu-id="294c1-306">Az alábbiakban hello tartalmát *minta\_hive\_előkészítése\_a\_aml\_full.hql* fájlt, amely adatokat előkészíti a modell létrehozása az Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="294c1-306">Below are hello contents of *sample\_hive\_prepare\_for\_aml\_full.hql* file that prepares data for model building in Azure Machine Learning.</span></span>

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of hello join into hello above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

<span data-ttu-id="294c1-307">toorun Ez a lekérdezés hello Hive könyvtárból kérni:</span><span class="sxs-lookup"><span data-stu-id="294c1-307">toorun this query, from hello Hive directory prompt :</span></span>

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

<span data-ttu-id="294c1-308">Most már van egy belső tábla "nyctaxidb.nyctaxi_downsampled_dataset" hello is elérhetők [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="294c1-308">We now have an internal table "nyctaxidb.nyctaxi_downsampled_dataset" which can be accessed using hello [Import Data][import-data] module from Azure Machine Learning.</span></span> <span data-ttu-id="294c1-309">Továbbá a Machine Learning modellek létrehozása ehhez az adatkészlethez előfordulhat, hogy használjuk.</span><span class="sxs-lookup"><span data-stu-id="294c1-309">Furthermore, we may use this dataset for building Machine Learning models.</span></span>  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a><span data-ttu-id="294c1-310">Az Azure Machine Learning tooaccess hello le a mintaadatokat hello adatokat importálhat modul használata</span><span class="sxs-lookup"><span data-stu-id="294c1-310">Use hello Import Data module in Azure Machine Learning tooaccess hello down sampled data</span></span>
<span data-ttu-id="294c1-311">Hive-lekérdezéseket a hello kiállító előfeltételként [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban, igazolnia kell tooan Azure Machine Learning-munkaterület hozzáférési és hozzáférési hello toohello hitelesítő adatait fürt és a kapcsolódó tárfiók.</span><span class="sxs-lookup"><span data-stu-id="294c1-311">As prerequisites for issuing Hive queries in hello [Import Data][import-data] module of Azure Machine Learning, we need access tooan Azure Machine Learning workspace and access toohello credentials of hello cluster and its associated storage account.</span></span>

<span data-ttu-id="294c1-312">Egyes adatok hello [és adatokat importálhat] [ import-data] modul és hello paraméterek tooinput:</span><span class="sxs-lookup"><span data-stu-id="294c1-312">Some details on hello [Import Data][import-data] module and hello parameters tooinput :</span></span>

<span data-ttu-id="294c1-313">**HCatalog kiszolgáló URI azonosítója**: Ha hello fürtnév abc123, akkor ez az egyszerűen: https://abc123.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="294c1-313">**HCatalog server URI**: If hello cluster name is abc123, then this is simply : https://abc123.azurehdinsight.net</span></span>

<span data-ttu-id="294c1-314">**Hadoop felhasználói fiók nevét** : hello fürthöz kiválasztott hello felhasználónevet (**nem** hello távelérési felhasználónév)</span><span class="sxs-lookup"><span data-stu-id="294c1-314">**Hadoop user account name** : hello user name chosen for hello cluster (**not** hello remote access user name)</span></span>

<span data-ttu-id="294c1-315">**Hadoop ser fiók jelszava** : hello fürt választott hello jelszóra (**nem** hello távelérési jelszó)</span><span class="sxs-lookup"><span data-stu-id="294c1-315">**Hadoop ser account password** : hello password chosen for hello cluster (**not** hello remote access password)</span></span>

<span data-ttu-id="294c1-316">**Az kimeneti adatok** : Ez toobe Azure van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="294c1-316">**Location of output data** : This is chosen toobe Azure.</span></span>

<span data-ttu-id="294c1-317">**Az Azure storage-fiók neve** : hello hello-fürthöz tartozó alapértelmezett a tárfióknak nevét.</span><span class="sxs-lookup"><span data-stu-id="294c1-317">**Azure storage account name** : Name of hello default storage account associated with hello cluster.</span></span>

<span data-ttu-id="294c1-318">**Az Azure Tárolónév** : Ez hello alapértelmezett tároló hello fürt nevét, és van általában hello azonos hello fürt neveként.</span><span class="sxs-lookup"><span data-stu-id="294c1-318">**Azure container name** : This is hello default container name for hello cluster, and is typically hello same as hello cluster name.</span></span> <span data-ttu-id="294c1-319">A fürt "abc123" nevű ez pedig csak abc123.</span><span class="sxs-lookup"><span data-stu-id="294c1-319">For a cluster called "abc123", this is just abc123.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="294c1-320">**Bármely táblájának használatával hello tooquery jelenítsük [és adatokat importálhat] [ import-data] modul az Azure Machine Learning egy belső tábla kell lennie.**</span><span class="sxs-lookup"><span data-stu-id="294c1-320">**Any table we wish tooquery using hello [Import Data][import-data] module in Azure Machine Learning must be an internal table.**</span></span> <span data-ttu-id="294c1-321">Tipp: a meghatározhatja, hogy a tábla T D.db adatbázisban egy belső tábla a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="294c1-321">A tip for determining if a table T in a database D.db is an internal table is as follows.</span></span>
> 
> 

<span data-ttu-id="294c1-322">A hello struktúra directory parancssorba probléma hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="294c1-322">From hello Hive directory prompt, issue hello command :</span></span>

    hdfs dfs -ls wasb:///D.db/T

<span data-ttu-id="294c1-323">Ha hello tábla egy belső tábla és a telepítéskor, a tartalmát itt kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="294c1-323">If hello table is an internal table and it is populated, its contents must show here.</span></span> <span data-ttu-id="294c1-324">Egy másik módja toodetermine, hogy-e egy táblát egy belső tábla toouse hello Azure Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="294c1-324">Another way toodetermine whether a table is an internal table is toouse hello Azure Storage Explorer.</span></span> <span data-ttu-id="294c1-325">Toonavigate toohello alapértelmezett tároló hello fürt nevét használja, és majd szűrés hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="294c1-325">Use it toonavigate toohello default container name of hello cluster, and then filter by hello table name.</span></span> <span data-ttu-id="294c1-326">Ha hello tábla és annak tartalmát jelenik meg, Ez megerősíti, hogy egy belső tábla.</span><span class="sxs-lookup"><span data-stu-id="294c1-326">If hello table and its contents show up, this confirms that it is an internal table.</span></span>

<span data-ttu-id="294c1-327">Íme egy pillanatkép hello Hive-lekérdezések és hello [és adatokat importálhat] [ import-data] modul:</span><span class="sxs-lookup"><span data-stu-id="294c1-327">Here is a snapshot of hello Hive query and hello [Import Data][import-data] module:</span></span>

![Adatokat importálhat modul Hive-lekérdezések](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

<span data-ttu-id="294c1-329">Vegye figyelembe, hogy a mintaadatokat hello alapértelmezett tárolóban található le, mert hello eredményül kapott Hive-lekérdezést az Azure Machine Learning nagyon egyszerű, csak a "Válasszon * a nyctaxidb.nyctaxi\_felbontáscsökkentésének\_adatok".</span><span class="sxs-lookup"><span data-stu-id="294c1-329">Note that since our down sampled data resides in hello default container, hello resulting Hive query from Azure Machine Learning is very simple and is just a "SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data".</span></span>

<span data-ttu-id="294c1-330">hello adatkészlet most hello kiindulási pontjaként gépi tanulási modellek készítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="294c1-330">hello dataset may now be used as hello starting point for building Machine Learning models.</span></span>

### <span data-ttu-id="294c1-331"><a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="294c1-331"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="294c1-332">Azt is képes tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="294c1-332">We are now able tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="294c1-333">hello adatok készen áll a us toouse fent azonosított hello előrejelzés problémák megoldásához:</span><span class="sxs-lookup"><span data-stu-id="294c1-333">hello data is ready for us toouse in addressing hello prediction problems identified above:</span></span>

<span data-ttu-id="294c1-334">**1. Bináris osztályozási**: toopredict tipp egy út kifizetett-e.</span><span class="sxs-lookup"><span data-stu-id="294c1-334">**1. Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>

<span data-ttu-id="294c1-335">**Használt tanuló:** két osztályú logisztikai regresszió</span><span class="sxs-lookup"><span data-stu-id="294c1-335">**Learner used:** Two-class logistic regression</span></span>

<span data-ttu-id="294c1-336">a.</span><span class="sxs-lookup"><span data-stu-id="294c1-336">a.</span></span> <span data-ttu-id="294c1-337">A probléma az cél (vagy osztály) címke van "Formabontó".</span><span class="sxs-lookup"><span data-stu-id="294c1-337">For this problem, our target (or class) label is "tipped".</span></span> <span data-ttu-id="294c1-338">Az eredeti le mintát adatkészletet rendelkezik néhány az oszlopokat, amelyek a cél kiszivárgásának a besorolási kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="294c1-338">Our original down-sampled dataset has a few columns that are target leaks for this classification experiment.</span></span> <span data-ttu-id="294c1-339">Különösen: tipp\_osztály, tipp\_mennyiségét, és a végösszeg\_összeg hello címkét, amely nem érhető el a tesztelési idő fedik fel információt.</span><span class="sxs-lookup"><span data-stu-id="294c1-339">In particular : tip\_class, tip\_amount, and total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="294c1-340">Ezek az oszlopok eltávolítása hello használata azon [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="294c1-340">We remove these columns from consideration using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="294c1-341">az alábbi hello pillanatkép jeleníti meg a kísérlet toopredict tipp egy adott út kifizetett-e.</span><span class="sxs-lookup"><span data-stu-id="294c1-341">hello snapshot below shows our experiment toopredict whether or not a tip was paid for a given trip.</span></span>

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

<span data-ttu-id="294c1-343">b.</span><span class="sxs-lookup"><span data-stu-id="294c1-343">b.</span></span> <span data-ttu-id="294c1-344">Ehhez a kísérlethez a cél címke azokat a terjesztéseket volt körülbelül 1:1.</span><span class="sxs-lookup"><span data-stu-id="294c1-344">For this experiment, our target label distributions were roughly 1:1.</span></span>

<span data-ttu-id="294c1-345">az alábbi hello pillanatkép hello terjesztési tipp osztály címkék hello bináris osztályozási problémához jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="294c1-345">hello snapshot below shows hello distribution of tip class labels for hello binary classification problem.</span></span>

![Tipp osztály címkék terjesztése](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

<span data-ttu-id="294c1-347">Ennek eredményeképpen azt szerezzen be egy AUC 0.987 a, az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="294c1-347">As a result, we obtain an AUC of 0.987 as shown in hello figure below.</span></span>

![AUC érték](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

<span data-ttu-id="294c1-349">**2. Multiclass besorolási**: toopredict hello tartomány hello út hello használatával korábban fizetett tipp díjak meghatározott osztályának.</span><span class="sxs-lookup"><span data-stu-id="294c1-349">**2. Multiclass classification**: toopredict hello range of tip amounts paid for hello trip, using hello previously defined classes.</span></span>

<span data-ttu-id="294c1-350">**Használt tanuló:** Multiclass logisztikai regresszió</span><span class="sxs-lookup"><span data-stu-id="294c1-350">**Learner used:** Multiclass logistic regression</span></span>

<span data-ttu-id="294c1-351">a.</span><span class="sxs-lookup"><span data-stu-id="294c1-351">a.</span></span> <span data-ttu-id="294c1-352">A probléma van az cél (vagy osztály) címke "Tipp\_osztály" amely (0,1,2,3,4) öt érték valamelyikét hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="294c1-352">For this problem, our target (or class) label is "tip\_class" which can take one of five values (0,1,2,3,4).</span></span> <span data-ttu-id="294c1-353">Hello bináris osztályozási esetében, mint van néhány az oszlopokat, amelyek a cél kiszivárgásának a kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="294c1-353">As in hello binary classification case, we have a few columns that are target leaks for this experiment.</span></span> <span data-ttu-id="294c1-354">Különösen: Formabontó, tipp\_teljes összeg\_összeg hello címkét, amely nem érhető el a tesztelési idő fedik fel információt.</span><span class="sxs-lookup"><span data-stu-id="294c1-354">In particular : tipped, tip\_amount, total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="294c1-355">Ezekben az oszlopokban hello eltávolítása [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="294c1-355">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="294c1-356">hello az alábbi pillanatkép készítése a rendeltetési tipp valószínűleg toofall kísérlet toopredict (osztály: 0: tipp = 0, 1 osztály: > $0 és tipp tipp < $5, osztály 2 =: > $5 és tipp tipp < = 10 $ osztály 3: > $10 és tipp tipp < = $20 4. osztály: tipp > $20)</span><span class="sxs-lookup"><span data-stu-id="294c1-356">hello snapshot below shows our experiment toopredict in which bin a tip is likely toofall ( Class 0: tip = $0, class 1 : tip > $0 and tip <= $5, Class 2 : tip > $5 and tip <= $10, Class 3 : tip > $10 and tip <= $20, Class 4 : tip > $20)</span></span>

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

<span data-ttu-id="294c1-358">Most megjelenítése a tényleges vizsgálati osztály terjesztési néz.</span><span class="sxs-lookup"><span data-stu-id="294c1-358">We now show what our actual test class distribution looks like.</span></span> <span data-ttu-id="294c1-359">Látható, hogy amíg osztály 0 és 1-osztály nem elterjedt, hello más osztályok ritkák.</span><span class="sxs-lookup"><span data-stu-id="294c1-359">We see that while Class 0 and Class 1 are prevalent, hello other classes are rare.</span></span>

![Osztály terjesztési tesztelése](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

<span data-ttu-id="294c1-361">b.</span><span class="sxs-lookup"><span data-stu-id="294c1-361">b.</span></span> <span data-ttu-id="294c1-362">Ehhez a kísérlethez az előrejelzési pontosság egy zavart mátrix toolook használhatja azt.</span><span class="sxs-lookup"><span data-stu-id="294c1-362">For this experiment, we use a confusion matrix toolook at our prediction accuracies.</span></span> <span data-ttu-id="294c1-363">Ez az alábbiakban látható.</span><span class="sxs-lookup"><span data-stu-id="294c1-363">This is shown below.</span></span>

![Zavart mátrix](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

<span data-ttu-id="294c1-365">Vegye figyelembe, hogy az osztály pontosság hello elterjedt osztályokon pedig elég jó hello modell nem "tanulás" szép munka meg hello ritkább osztályok.</span><span class="sxs-lookup"><span data-stu-id="294c1-365">Note that while our class accuracies on hello prevalent classes is quite good, hello model does not do a good job of "learning" on hello rarer classes.</span></span>

<span data-ttu-id="294c1-366">**3. Regressziós feladat**: toopredict hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="294c1-366">**3. Regression task**: toopredict hello amount of tip paid for a trip.</span></span>

<span data-ttu-id="294c1-367">**Használt tanuló:** Boosted döntési fája</span><span class="sxs-lookup"><span data-stu-id="294c1-367">**Learner used:** Boosted decision tree</span></span>

<span data-ttu-id="294c1-368">a.</span><span class="sxs-lookup"><span data-stu-id="294c1-368">a.</span></span> <span data-ttu-id="294c1-369">A probléma van az cél (vagy osztály) címke "Tipp\_összeg".</span><span class="sxs-lookup"><span data-stu-id="294c1-369">For this problem, our target (or class) label is "tip\_amount".</span></span> <span data-ttu-id="294c1-370">A cél kiszivárgásának ebben az esetben is: Formabontó, tipp\_osztály, teljes\_összeg; minden változókhoz hello tipp mennyiségét, amely általában nem érhető el a tesztelési idő információk felfedése.</span><span class="sxs-lookup"><span data-stu-id="294c1-370">Our target leaks in this case are : tipped, tip\_class, total\_amount ; all these variables reveal information about hello tip amount that is typically unavailable at testing time.</span></span> <span data-ttu-id="294c1-371">Ezekben az oszlopokban hello eltávolítása [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="294c1-371">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="294c1-372">hello pillanatkép belows a megadott tipp hello kísérlet toopredict hello összegét mutatja.</span><span class="sxs-lookup"><span data-stu-id="294c1-372">hello snapshot belows shows our experiment toopredict hello amount of hello given tip.</span></span>

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

<span data-ttu-id="294c1-374">b.</span><span class="sxs-lookup"><span data-stu-id="294c1-374">b.</span></span> <span data-ttu-id="294c1-375">Regressziós problémák az előrejelzési hello pontosság mérjük négyzet hello hiba a hello előrejelzéseket megtekintésével, hello együttható, és például hello.</span><span class="sxs-lookup"><span data-stu-id="294c1-375">For regression problems, we measure hello accuracies of our prediction by looking at hello squared error in hello predictions, hello coefficient of determination, and hello like.</span></span> <span data-ttu-id="294c1-376">Megmutatjuk, ezek az alábbi.</span><span class="sxs-lookup"><span data-stu-id="294c1-376">We show these below.</span></span>

![Előrejelzés statisztikai](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

<span data-ttu-id="294c1-378">Azt látja, hogy kapcsolatos hello együttható 0.709, a modell együttható magyarázza készül 71 % hello eltérés utalás.</span><span class="sxs-lookup"><span data-stu-id="294c1-378">We see that about hello coefficient of determination is 0.709, implying about 71% of hello variance is explained by our model coefficients.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="294c1-379">További információk az Azure Machine Learning toolearn és hogyan tooaccess és használni, tekintse meg a túl[Mi az Machine Learning?](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="294c1-379">toolearn more about Azure Machine Learning and how tooaccess and use it, please refer too[What's Machine Learning?](machine-learning-what-is-machine-learning.md).</span></span> <span data-ttu-id="294c1-380">A Machine Learning kísérleteket Azure Machine Learning szolgáltatásban egy csoportját játszik nagyon hasznos erőforrás hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="294c1-380">A very useful resource for playing with a bunch of Machine Learning experiments on Azure Machine Learning is hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span></span> <span data-ttu-id="294c1-381">hello gyűjtemény egy kísérletek skáláját ismerteti, és az Azure Machine Learning képességek hello tartományba alapos bevezetést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="294c1-381">hello Gallery covers a gamut of experiments and provides a thorough introduction into hello range of capabilities of Azure Machine Learning.</span></span>
> 
> 

## <a name="license-information"></a><span data-ttu-id="294c1-382">Licencinformációk</span><span class="sxs-lookup"><span data-stu-id="294c1-382">License Information</span></span>
<span data-ttu-id="294c1-383">Ez a minta a forgatókönyv és a hozzá tartozó szkriptek által megosztott Microsoft hello MIT licence.</span><span class="sxs-lookup"><span data-stu-id="294c1-383">This sample walkthrough and its accompanying scripts are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="294c1-384">Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.</span><span class="sxs-lookup"><span data-stu-id="294c1-384">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="294c1-385">Referencia</span><span class="sxs-lookup"><span data-stu-id="294c1-385">References</span></span>
<span data-ttu-id="294c1-386">• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="294c1-386">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="294c1-387">• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="294c1-387">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="294c1-388">• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="294c1-388">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
