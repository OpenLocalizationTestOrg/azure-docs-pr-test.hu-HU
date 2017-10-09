---
title: "aaaBuild és a gépi tanulási modellek használata az SQL Server egy Azure virtuális gépen telepítése |} Microsoft Docs"
description: "Bővített Analitikát folyamat és a technológia, működés közben"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 30ba9a9e3cf65f75015e13f9c7876dcbccc5bc47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a><span data-ttu-id="6ae12-103">hello Team adatok tudományos folyamat működés közben: SQL Server használata</span><span class="sxs-lookup"><span data-stu-id="6ae12-103">hello Team Data Science Process in action: using SQL Server</span></span>
<span data-ttu-id="6ae12-104">Az oktatóanyag ismerteti hello folyamatot, amely létrehozása és telepítése a gépi tanulási modellek használata az SQL Server és a nyilvánosan elérhető dataset – hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="6ae12-104">In this tutorial, you walk through hello process of building and deploying a machine learning model using SQL Server and a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="6ae12-105">hello eljárást követi a szabványos adatelemezési munkafolyamatot: betöltési és hello adatokba, tervezését szolgáltatások toofacilitate tanulási, majd build és a modell rendszerbe állítása.</span><span class="sxs-lookup"><span data-stu-id="6ae12-105">hello procedure follows a standard data science workflow: ingest and explore hello data, engineer features toofacilitate learning, then build and deploy a model.</span></span>

## <span data-ttu-id="6ae12-106"><a name="dataset"></a>NYC Taxi utazás közben adatkészlet leírása</span><span class="sxs-lookup"><span data-stu-id="6ae12-106"><a name="dataset"></a>NYC Taxi Trips Dataset Description</span></span>
<span data-ttu-id="6ae12-107">hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (tömörítetlen ~ 48GB), amely több mint 173 millió egyedi utak és hello turistajegyek esetében minden út kifizette.</span><span class="sxs-lookup"><span data-stu-id="6ae12-107">hello NYC Taxi Trip data is about 20GB of compressed CSV files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="6ae12-108">Minden út rekord tartalmazza hello felvétel és Gyűjtőtár hely és idő, anonimizált rejthetők el (illesztőprogram) engedély száma és medallion (taxi tartozó egyedi azonosító) számát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-108">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="6ae12-109">hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:</span><span class="sxs-lookup"><span data-stu-id="6ae12-109">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="6ae12-110">hello "trip_data" CSV út részleteit, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ae12-110">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="6ae12-111">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="6ae12-111">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="6ae12-112">hello "trip_fare" CSV hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6ae12-112">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="6ae12-113">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="6ae12-113">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="6ae12-114">hello egyedi kulcs toojoin út\_adatok és út\_jegy ára áll hello mezők: medallion, rejthetők el\_engedély és a felvételi\_datetime.</span><span class="sxs-lookup"><span data-stu-id="6ae12-114">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

## <span data-ttu-id="6ae12-115"><a name="mltasks"></a>Előrejelzés feladatok példák</span><span class="sxs-lookup"><span data-stu-id="6ae12-115"><a name="mltasks"></a>Examples of Prediction Tasks</span></span>
<span data-ttu-id="6ae12-116">Azt a három előrejelzés problémák hello alapján fogalmaz *tipp\_összeg*, nevezetesen:</span><span class="sxs-lookup"><span data-stu-id="6ae12-116">We will formulate three prediction problems based on hello *tip\_amount*, namely:</span></span>

1. <span data-ttu-id="6ae12-117">Bináris osztályozás: előre jelezni, függetlenül attól, tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_összeg* $ 0 van egy negatív példa.</span><span class="sxs-lookup"><span data-stu-id="6ae12-117">Binary classification: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="6ae12-118">Multiclass besorolási: hello út fizetett tipp toopredict hello tartományán.</span><span class="sxs-lookup"><span data-stu-id="6ae12-118">Multiclass classification: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="6ae12-119">A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:</span><span class="sxs-lookup"><span data-stu-id="6ae12-119">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="6ae12-120">Regressziós feladat: toopredict hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="6ae12-120">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="6ae12-121"><a name="setup"></a>A beállítás mentése hello az Azure data tudományos környezet speciális elemzésekre</span><span class="sxs-lookup"><span data-stu-id="6ae12-121"><a name="setup"></a>Setting Up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="6ae12-122">A hello látható [a környezet megtervezése](machine-learning-data-science-plan-your-environment.md) az útmutatóban több beállítások toowork hello NYC Taxi való adatváltások számát az adatkészlethez az Azure-ban van:</span><span class="sxs-lookup"><span data-stu-id="6ae12-122">As you can see from hello [Plan Your Environment](machine-learning-data-science-plan-your-environment.md) guide, there are several options toowork with hello NYC Taxi Trips dataset in Azure:</span></span>

* <span data-ttu-id="6ae12-123">Hello adatokat az Azure-blobokat, majd a modellt az Azure Machine Learning használata</span><span class="sxs-lookup"><span data-stu-id="6ae12-123">Work with hello data in Azure blobs then model in Azure Machine Learning</span></span>
* <span data-ttu-id="6ae12-124">Hello adatok betöltése az SQL Server-adatbázist, majd a modellt az Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6ae12-124">Load hello data into a SQL Server database then model in Azure Machine Learning</span></span>

<span data-ttu-id="6ae12-125">Ebben az oktatóanyagban a párhuzamos tömeges importálással hello adatok tooa SQL Server, az adatok feltárása, a szolgáltatás a bemutatjuk mérnöki mintavételi le SQL Server Management Studio használatával, valamint IPython Notebook használatával.</span><span class="sxs-lookup"><span data-stu-id="6ae12-125">In this tutorial we will demonstrate parallel bulk import of hello data tooa SQL Server, data exploration, feature engineering and down sampling using SQL Server Management Studio as well as using IPython Notebook.</span></span> <span data-ttu-id="6ae12-126">[Minta parancsfájlok](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) és [IPython notebookok](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) megosztott a Githubon.</span><span class="sxs-lookup"><span data-stu-id="6ae12-126">[Sample scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) and [IPython notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) are shared in GitHub.</span></span> <span data-ttu-id="6ae12-127">Egy minta IPython notebook toowork hello adatokkal az Azure-blobokat is rendelkezésre áll, a hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="6ae12-127">A sample IPython notebook toowork with hello data in Azure blobs is also available in hello same location.</span></span>

<span data-ttu-id="6ae12-128">az Azure Adattudomány környezet tooset:</span><span class="sxs-lookup"><span data-stu-id="6ae12-128">tooset up your Azure Data Science environment:</span></span>

1. [<span data-ttu-id="6ae12-129">Tárfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-129">Create a storage account</span></span>](../storage/common/storage-create-storage-account.md)
2. [<span data-ttu-id="6ae12-130">Az Azure Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-130">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
3. <span data-ttu-id="6ae12-131">[Rendszerű virtuális gép adatok tudományos](machine-learning-data-science-setup-sql-server-virtual-machine.md), amely biztosítja, hogy egy SQL Server és IPython Notebook kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6ae12-131">[Provision a Data Science Virtual Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), which provides a SQL Server and an IPython Notebook server.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6ae12-132">hello mintaparancsfájlok és IPython notebookok letöltött tooyour Adattudomány virtuális gép lesz hello beállítási folyamata során.</span><span class="sxs-lookup"><span data-stu-id="6ae12-132">hello sample scripts and IPython notebooks will be downloaded tooyour Data Science virtual machine during hello setup process.</span></span> <span data-ttu-id="6ae12-133">Hello VM telepítés utáni parancsfájl befejezése után a virtuális gép dokumentumtár lehet hello minták:</span><span class="sxs-lookup"><span data-stu-id="6ae12-133">When hello VM post-installation script completes, hello samples will be in your VM's Documents library:</span></span>  
   > 
   > * <span data-ttu-id="6ae12-134">Minta parancsfájlok:`C:\Users\<user_name>\Documents\Data Science Scripts`</span><span class="sxs-lookup"><span data-stu-id="6ae12-134">Sample Scripts: `C:\Users\<user_name>\Documents\Data Science Scripts`</span></span>  
   > * <span data-ttu-id="6ae12-135">A minta IPython notebookok:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span><span class="sxs-lookup"><span data-stu-id="6ae12-135">Sample IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span></span>  
   >   <span data-ttu-id="6ae12-136">Ha `<user_name>` a virtuális gép Windows bejelentkezési név.</span><span class="sxs-lookup"><span data-stu-id="6ae12-136">where `<user_name>` is your VM's Windows login name.</span></span> <span data-ttu-id="6ae12-137">Minta mappák toohello hivatkozik **mintaparancsfájlok** és **minta IPython notebookok**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-137">We will refer toohello sample folders as **Sample Scripts** and **Sample IPython Notebooks**.</span></span>
   > 
   > 

<span data-ttu-id="6ae12-138">Hello adatkészlet méretének, adatforrásról és hello Azure célként kiválasztott környezet alapján, ebben a forgatókönyvben hasonlít túl[forgatókönyv \#5: nagy adatkészlet egy helyi fájlok, SQL Server Azure virtuális gép cél](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span><span class="sxs-lookup"><span data-stu-id="6ae12-138">Based on hello dataset size, data source location, and hello selected Azure target environment, this scenario is similar too[Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span></span>

## <span data-ttu-id="6ae12-139"><a name="getdata"></a>Nyilvános forráskódú hello adatok lekérése</span><span class="sxs-lookup"><span data-stu-id="6ae12-139"><a name="getdata"></a>Get hello Data from Public Source</span></span>
<span data-ttu-id="6ae12-140">tooget hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset nyilvános helyéről, segítségével ismertetett hello-metódusok bármelyike [adatok áthelyezése tooand az Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello adatok tooyour új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6ae12-140">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour new virtual machine.</span></span>

<span data-ttu-id="6ae12-141">toocopy hello adatok AzCopy használatával:</span><span class="sxs-lookup"><span data-stu-id="6ae12-141">toocopy hello data using AzCopy:</span></span>

1. <span data-ttu-id="6ae12-142">Jelentkezzen be tooyour virtuális gép (VM)</span><span class="sxs-lookup"><span data-stu-id="6ae12-142">Log in tooyour virtual machine (VM)</span></span>
2. <span data-ttu-id="6ae12-143">Hozzon létre egy új könyvtárat a hello VM adatlemez (Megjegyzés: ne használjon hello ideiglenes lemez, amely virtuális Gépet állítsanak adatlemezt hello).</span><span class="sxs-lookup"><span data-stu-id="6ae12-143">Create a new directory in hello VM's data disk (Note: Do not use hello Temporary Disk which comes with hello VM as a Data Disk).</span></span>
3. <span data-ttu-id="6ae12-144">Egy parancssori ablakot futtassa a következő Azcopy parancssori, < path_to_data_folder > cseréje a data mappán (2) létrehozott hello:</span><span class="sxs-lookup"><span data-stu-id="6ae12-144">In a Command Prompt window, run hello following Azcopy command line, replacing <path_to_data_folder> with your data folder created in (2):</span></span>
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    <span data-ttu-id="6ae12-145">Hello AzCopy befejezését követően a rendszer összesen 24 zip CSV-fájlok (12-út\_adatok és a 12-út\_jegy ára) hello adatok mappában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6ae12-145">When hello AzCopy completes, a total of 24 zipped CSV files (12 for trip\_data and 12 for trip\_fare) should be in hello data folder.</span></span>
4. <span data-ttu-id="6ae12-146">Bontsa ki a letöltött hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-146">Unzip hello downloaded files.</span></span> <span data-ttu-id="6ae12-147">Vegye figyelembe a tömörítetlen hello fájlokat tároló hello mappát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-147">Note hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="6ae12-148">Ez a mappa lesz hivatkozott tooas hello < elérési út\_való\_adatok\_fájlok\>.</span><span class="sxs-lookup"><span data-stu-id="6ae12-148">This folder will be referred tooas hello <path\_to\_data\_files\>.</span></span>

## <span data-ttu-id="6ae12-149"><a name="dbload"></a>A tömeges adatok importálása az SQL Server-adatbázisba</span><span class="sxs-lookup"><span data-stu-id="6ae12-149"><a name="dbload"></a>Bulk Import Data into SQL Server Database</span></span>
<span data-ttu-id="6ae12-150">hello betöltés/átvitele a nagy mennyiségű adatok tooan SQL-adatbázis és a lekérdezések teljesítményét növelhető a *particionált táblák és nézetek*.</span><span class="sxs-lookup"><span data-stu-id="6ae12-150">hello performance of loading/transferring large amounts of data tooan SQL database and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> <span data-ttu-id="6ae12-151">Ebben a szakaszban ismertetett hello utasításokat követi azt [párhuzamos tömeges adatok importálása használatával SQL partíciós táblákba](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate egy új adatbázist és hello az adatok betöltése párhuzamosan particionált táblába.</span><span class="sxs-lookup"><span data-stu-id="6ae12-151">In this section, we will follow hello instructions described in [Parallel Bulk Data Import Using SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate a new database and load hello data into partitioned tables in parallel.</span></span>

1. <span data-ttu-id="6ae12-152">A virtuális gép tooyour bejelentkezve indítsa el a **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-152">While logged in tooyour VM, start **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="6ae12-153">Csatlakozás a Windows-hitelesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="6ae12-153">Connect using Windows Authentication.</span></span>
   
    ![SSMS csatlakozás][12]
3. <span data-ttu-id="6ae12-155">Ha még nem módosított hello SQL Server hitelesítési mód, és létrehozott egy új SQL-bejelentkezési felhasználójának, nyissa meg a nevű hello parancsfájl **módosítása\_auth.sql** a hello **mintaparancsfájlok** mappát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-155">If you have not yet changed hello SQL Server authentication mode and created a new SQL login user, open hello script file named **change\_auth.sql** in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="6ae12-156">Hello alapértelmezett felhasználónév és jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="6ae12-156">Change hello  default user name and password.</span></span> <span data-ttu-id="6ae12-157">Kattintson a **! Végrehajtás** hello eszköztár toorun hello parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-157">Click **!Execute** in hello toolbar toorun hello script.</span></span>
   
    ![Parancsfájlok végrehajtását][13]
4. <span data-ttu-id="6ae12-159">Győződjön meg arról, és/vagy módosításához hello SQL Server alapértelmezett adatbázishoz és naplófájlokhoz mappák tooensure adatbázisok újonnan létrehozott adatlemezt fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="6ae12-159">Verify and/or change hello SQL Server default database and log folders tooensure that newly created databases will be stored in a Data Disk.</span></span> <span data-ttu-id="6ae12-160">hello SQL Server Virtuálisgép-lemezkép datawarehousing terhelések optimalizált előre konfigurált adatainak és naplókönyvtárainak lemezzel.</span><span class="sxs-lookup"><span data-stu-id="6ae12-160">hello SQL Server VM image that is optimized for datawarehousing loads is pre-configured with data and log disks.</span></span> <span data-ttu-id="6ae12-161">Ha a virtuális gép nem tartalmazza az adatlemezt, és a hozzáadott új virtuális merevlemezek hello Virtuálisgép-telepítési folyamat során, módosítsa az alábbiak szerint hello alapértelmezett mappák:</span><span class="sxs-lookup"><span data-stu-id="6ae12-161">If your VM did not include a Data Disk and you added new virtual hard disks during hello VM setup process, change hello default folders as follows:</span></span>
   
   * <span data-ttu-id="6ae12-162">Kattintson a jobb gombbal hello SQL-kiszolgáló neve a bal oldali panelen, és kattintson hello **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-162">Right-click hello SQL Server name in hello left panel and click **Properties**.</span></span>
     
       ![SQL-kiszolgáló tulajdonságai][14]
   * <span data-ttu-id="6ae12-164">Válassza ki **adatbázis beállításainak** a hello **oldal kijelölése** toohello bal oldali listában.</span><span class="sxs-lookup"><span data-stu-id="6ae12-164">Select **Database Settings** from hello **Select a page** list toohello left.</span></span>
   * <span data-ttu-id="6ae12-165">Ellenőrizze és/vagy módosításához a hello **adatbázis alapértelmezett helyek** toohello **adatlemez** az Ön által választott helyen.</span><span class="sxs-lookup"><span data-stu-id="6ae12-165">Verify and/or change hello **Database default locations** toohello **Data Disk** locations of your choice.</span></span> <span data-ttu-id="6ae12-166">Ez az új adatbázisokat tároló Ha hello alapértelmezett helye beállításokkal hozza létre.</span><span class="sxs-lookup"><span data-stu-id="6ae12-166">This is where new databases reside if created with hello default location settings.</span></span>
     
       ![SQL-adatbázis alapértelmezett értéke][15]  
5. <span data-ttu-id="6ae12-168">toocreate egy új adatbázist és a fájlcsoport toohold hello particionált táblák, nyissa meg a hello mintaparancsfájl **létrehozása\_db\_default.sql**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-168">toocreate a new database and a set of filegroups toohold hello partitioned tables, open hello sample script **create\_db\_default.sql**.</span></span> <span data-ttu-id="6ae12-169">hello parancsfájl nevű új adatbázist fog létrehozni **TaxiNYC** és 12 fájlcsoportok hello alapértelmezett adatok helyen.</span><span class="sxs-lookup"><span data-stu-id="6ae12-169">hello script will create a new database named **TaxiNYC** and 12 filegroups in hello default data location.</span></span> <span data-ttu-id="6ae12-170">Minden fájlcsoport tárolására egy hónap út\_adatok és út\_díjszabás adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-170">Each filegroup will hold one month of trip\_data and trip\_fare data.</span></span> <span data-ttu-id="6ae12-171">Ha szükséges, módosítsa a hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="6ae12-171">Modify hello database name, if desired.</span></span> <span data-ttu-id="6ae12-172">Kattintson a **! Végrehajtás** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="6ae12-172">Click **!Execute** toorun hello script.</span></span>
6. <span data-ttu-id="6ae12-173">Ezután hozzon létre két partíció tábla, egy a hello út\_adatokat, majd egy másikat a hello út\_jegy ára.</span><span class="sxs-lookup"><span data-stu-id="6ae12-173">Next, create two partition tables, one for hello trip\_data and another for hello trip\_fare.</span></span> <span data-ttu-id="6ae12-174">Nyissa meg a hello mintaparancsfájl **létrehozása\_particionált\_table.sql**, amelynél:</span><span class="sxs-lookup"><span data-stu-id="6ae12-174">Open hello sample script **create\_partitioned\_table.sql**, which will:</span></span>
   
   * <span data-ttu-id="6ae12-175">Hozzon létre egy partíciós függvény toosplit hello adatok hónap szerint.</span><span class="sxs-lookup"><span data-stu-id="6ae12-175">Create a partition function toosplit hello data by month.</span></span>
   * <span data-ttu-id="6ae12-176">Hozzon létre egy partíciós séma toomap minden hónap tooa különböző adatfájlcsoport.</span><span class="sxs-lookup"><span data-stu-id="6ae12-176">Create a partition scheme toomap each month's data tooa different filegroup.</span></span>
   * <span data-ttu-id="6ae12-177">Hozzon létre két particionált táblák csatlakoztatott toohello partícióséma: **nyctaxi\_út** hello út tárolására\_adatok és **nyctaxi\_jegy ára** hello út tárolására \_díjszabás adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-177">Create two partitioned tables mapped toohello partition scheme: **nyctaxi\_trip** will hold hello trip\_data and **nyctaxi\_fare** will hold hello trip\_fare data.</span></span>
     
     <span data-ttu-id="6ae12-178">Kattintson a **! Végrehajtás** toorun parancsfájl hello és hello particionált táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ae12-178">Click **!Execute** toorun hello script and create hello partitioned tables.</span></span>
7. <span data-ttu-id="6ae12-179">A hello **mintaparancsfájlok** mappa, két minta PowerShell-parancsfájlok megadott toodemonstrate párhuzamos tömeges importálások tooSQL Server adattáblák van.</span><span class="sxs-lookup"><span data-stu-id="6ae12-179">In hello **Sample Scripts** folder, there are two sample PowerShell scripts provided toodemonstrate parallel bulk imports of data tooSQL Server tables.</span></span>
   
   * <span data-ttu-id="6ae12-180">**BCP\_párhuzamos\_generic.ps1** van egy általános parancsfájl tooparallel tömeges adatok importálása egy táblába.</span><span class="sxs-lookup"><span data-stu-id="6ae12-180">**bcp\_parallel\_generic.ps1** is a generic script tooparallel bulk import data into a table.</span></span> <span data-ttu-id="6ae12-181">A parancsfájl tooset hello bemeneti és a cél változók módosítása a hello Megjegyzés sorok hello parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-181">Modify this script tooset hello input and target variables as indicated in hello comment lines in hello script.</span></span>
   * <span data-ttu-id="6ae12-182">**BCP\_párhuzamos\_nyctaxi.ps1** előre konfigurált változata hello általános parancsfájlt, és használt tootooload hello NYC Taxi Utazgatással adatok mindkét táblát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-182">**bcp\_parallel\_nyctaxi.ps1** is a pre-configured version of hello generic script and can be used tootooload both tables for hello NYC Taxi Trips data.</span></span>  
8. <span data-ttu-id="6ae12-183">Kattintson a jobb gombbal hello **bcp\_párhuzamos\_nyctaxi.ps1** parancsfájl nevét, és kattintson **szerkesztése** tooopen azt a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="6ae12-183">Right-click hello **bcp\_parallel\_nyctaxi.ps1** script name and click **Edit** tooopen it in PowerShell.</span></span> <span data-ttu-id="6ae12-184">Felülvizsgálati hello előre változóival, és módosítsa a függően tooyour kijelölt adatbázis neve, a bemeneti adatok mappa, a célmappa napló és a elérési utak toohello minta formátumú fájlok **nyctaxi_trip.xml** és **nyctaxi\_ fare.xml** (hello megadott **mintaparancsfájlok** mappa).</span><span class="sxs-lookup"><span data-stu-id="6ae12-184">Review hello preset variables and modify according tooyour selected database name, input data folder, target log folder, and paths toohello  sample format files **nyctaxi_trip.xml** and **nyctaxi\_fare.xml** (provided in hello **Sample Scripts** folder).</span></span>
   
    ![A tömeges adatok importálása][16]
   
    <span data-ttu-id="6ae12-186">Hello hitelesítési módot is kiválaszthat, alapértelmezett Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="6ae12-186">You may also select hello authentication mode, default is Windows Authentication.</span></span> <span data-ttu-id="6ae12-187">Kattintson a hello eszköztár toorun hello zöld nyílra.</span><span class="sxs-lookup"><span data-stu-id="6ae12-187">Click hello green arrow in hello toolbar toorun.</span></span> <span data-ttu-id="6ae12-188">hello parancsfájl 24 tömeges importálási műveletek párhuzamos, 12 particionált táblák elindul.</span><span class="sxs-lookup"><span data-stu-id="6ae12-188">hello script will launch 24 bulk import operations in parallel, 12 for each partitioned table.</span></span> <span data-ttu-id="6ae12-189">Előfordulhat, hogy figyelheti hello adatok importálása folyamatban fenti beállított hello SQL Server alapértelmezett Adatmappa megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="6ae12-189">You may monitor hello data import progress by opening hello SQL Server default data folder as set above.</span></span>
9. <span data-ttu-id="6ae12-190">hello PowerShell parancsfájl jelentések hello kezdési és befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="6ae12-190">hello PowerShell script reports hello starting and ending times.</span></span> <span data-ttu-id="6ae12-191">Az összes tömeges teljes importálja, befejezési időpont hello jelenteni.</span><span class="sxs-lookup"><span data-stu-id="6ae12-191">When all bulk imports complete, hello ending time is reported.</span></span> <span data-ttu-id="6ae12-192">Hello cél napló mappa tooverify hello tömeges importálása sikerült-e, azaz, hogy ellenőrizze, nincs hiba hello napló célmappában.</span><span class="sxs-lookup"><span data-stu-id="6ae12-192">Check hello target log folder tooverify that hello bulk imports were successful, i.e., no errors reported in hello target log folder.</span></span>
10. <span data-ttu-id="6ae12-193">Az adatbázis készen áll a feltárása, a szolgáltatás tervezés és egyéb műveletek.</span><span class="sxs-lookup"><span data-stu-id="6ae12-193">Your database is now ready for exploration, feature engineering, and other operations as desired.</span></span> <span data-ttu-id="6ae12-194">Mivel hello táblák particionáltak toohello szerint **a felvételi\_dátum és idő** mezőben, a lekérdezések, többek között **a felvételi\_datetime** hello feltételek  **Ha** záradékot használják ki a hello partícióséma előnyeit.</span><span class="sxs-lookup"><span data-stu-id="6ae12-194">Since hello tables are partitioned according toohello **pickup\_datetime** field, queries which include **pickup\_datetime** conditions in hello **WHERE** clause will benefit from hello partition scheme.</span></span>
11. <span data-ttu-id="6ae12-195">A **SQL Server Management Studio**, megismerkedhet a megadott hello mintaparancsfájl **minta\_queries.sql**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-195">In **SQL Server Management Studio**, explore hello provided sample script **sample\_queries.sql**.</span></span> <span data-ttu-id="6ae12-196">hello mintalekérdezések, kiemelési hello bármelyikét sorok lekérdezéséhez, majd kattintson az toorun **! Végrehajtás** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="6ae12-196">toorun any of hello sample queries, highlight hello query lines then click **!Execute** in hello toolbar.</span></span>
12. <span data-ttu-id="6ae12-197">hello NYC Taxi Utazgatással adatok két külön táblázatban be van töltve.</span><span class="sxs-lookup"><span data-stu-id="6ae12-197">hello NYC Taxi Trips data is loaded in two separate tables.</span></span> <span data-ttu-id="6ae12-198">tooimprove összekapcsolási műveletek, lehetőleg tooindex hello táblákat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-198">tooimprove join operations, it is highly recommended tooindex hello tables.</span></span> <span data-ttu-id="6ae12-199">mintaparancsfájl hello **létrehozása\_particionált\_index.sql** particionált indexek létrehozása a hello összetett illesztési kulcs **medallion, rejthetők el\_licenc, és a felvételi\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-199">hello sample script **create\_partitioned\_index.sql** creates partitioned indexes on hello composite join key **medallion, hack\_license, and pickup\_datetime**.</span></span>

## <span data-ttu-id="6ae12-200"><a name="dbexplore"></a>Az adatok feltárása és az SQL Server szolgáltatás manipuláció</span><span class="sxs-lookup"><span data-stu-id="6ae12-200"><a name="dbexplore"></a>Data Exploration and Feature Engineering in SQL Server</span></span>
<span data-ttu-id="6ae12-201">Ebben a szakaszban végezzük el adatok feltárása és a szolgáltatás generálása hello közvetlenül az SQL-lekérdezések futtatásával **SQL Server Management Studio** korábban létrehozott hello SQL Server-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="6ae12-201">In this section, we will perform data exploration and feature generation by running SQL queries directly in hello **SQL Server Management Studio** using hello SQL Server database created earlier.</span></span> <span data-ttu-id="6ae12-202">Egy minta parancsfájlt nevű **minta\_queries.sql** hello megadott **mintaparancsfájlok** mappát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-202">A sample script named **sample\_queries.sql** is provided in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="6ae12-203">Hello parancsfájl toochange hello adatbázis neve, módosítása, ha hello alapértelmezettől eltérő: **TaxiNYC**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-203">Modify hello script toochange hello database name, if it is different from hello default: **TaxiNYC**.</span></span>

<span data-ttu-id="6ae12-204">Ebben a gyakorlatban a következő történik:</span><span class="sxs-lookup"><span data-stu-id="6ae12-204">In this exercise, we will:</span></span>

* <span data-ttu-id="6ae12-205">Csatlakozás túl**SQL Server Management Studio** vagy Windows-hitelesítés használatával, vagy használja az SQL-hitelesítést és hello SQL-bejelentkezési nevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="6ae12-205">Connect too**SQL Server Management Studio** using either Windows Authentication or using SQL Authentication and hello SQL login name and password.</span></span>
* <span data-ttu-id="6ae12-206">Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.</span><span class="sxs-lookup"><span data-stu-id="6ae12-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="6ae12-207">Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.</span><span class="sxs-lookup"><span data-stu-id="6ae12-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="6ae12-208">Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="6ae12-209">Szolgáltatások létrehozása és számítási/összehasonlítása út távolság.</span><span class="sxs-lookup"><span data-stu-id="6ae12-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="6ae12-210">Csatlakozás hello két táblák, és bontsa ki a használt toobuild modellek leendő véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="6ae12-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

<span data-ttu-id="6ae12-211">Amikor készen áll a tooproceed tooAzure gépi tanulás, akkor előfordulhat, hogy vagy:</span><span class="sxs-lookup"><span data-stu-id="6ae12-211">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="6ae12-212">Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] modul az Azure Machine Learning, vagy</span><span class="sxs-lookup"><span data-stu-id="6ae12-212">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="6ae12-213">Hello mintát megmaradnak, és azt tervezi, hogy az adatbázis felépítése modell toouse visszafejtett adatok tábla, és a hello új tábla hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-213">Persist hello sampled and engineered data you plan toouse for model building in a new database table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

<span data-ttu-id="6ae12-214">Ebben a szakaszban hello utolsó lekérdezési tooextract és minta hello adatok menti azt.</span><span class="sxs-lookup"><span data-stu-id="6ae12-214">In this section we will save hello final query tooextract and sample hello data.</span></span> <span data-ttu-id="6ae12-215">második módszer hello mutatják be hello [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-215">hello second method is demonstrated in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span>

<span data-ttu-id="6ae12-216">Sorok és oszlopok a hello hello számának gyors ellenőrzés táblák fel korábban a párhuzamos tömeges importálással segítségével</span><span class="sxs-lookup"><span data-stu-id="6ae12-216">For a quick verification of hello number of rows and columns in hello tables populated earlier using parallel bulk import,</span></span>

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="6ae12-217">Feltárása: Út elosztása medallion szerint</span><span class="sxs-lookup"><span data-stu-id="6ae12-217">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="6ae12-218">Ebben a példában hello medallion (taxi számok) azonosítja a 100-nál több való adatváltások számát egy adott időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="6ae12-218">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="6ae12-219">hello lekérdezés előnyös hello particionált tábla hozzáférés óta, akkor annak hello partíciós sémája által **a felvételi\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-219">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="6ae12-220">Hello teljes adatkészlet lekérdezése is teszi particionált tábla hello használata és/vagy vizsgálat index.</span><span class="sxs-lookup"><span data-stu-id="6ae12-220">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="6ae12-221">Feltárása: Út terjesztési medallion és hack_license</span><span class="sxs-lookup"><span data-stu-id="6ae12-221">Exploration: Trip distribution by medallion and hack_license</span></span>
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="6ae12-222">Adatok vizsgálatának: Helytelen hosszúsági és/vagy szélességi rekordjainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6ae12-222">Data Quality Assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="6ae12-223">Ez a példa pedig megvizsgálja, ha bármelyik hello hosszúsági és/vagy szélességi mezők vagy érvénytelen értéket tartalmaz (radián fok -90 és 90 között kell lennie), vagy (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="6ae12-223">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="6ae12-224">Feltárása: A vs Formabontó. Nem Formabontó Utazgatással terjesztési</span><span class="sxs-lookup"><span data-stu-id="6ae12-224">Exploration: Tipped vs. Not Tipped Trips distribution</span></span>
<span data-ttu-id="6ae12-225">Ez a példa hello száma, amelyek volt Formabontó és időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó) egy adott idő alatt nem Formabontó utak megkeresése.</span><span class="sxs-lookup"><span data-stu-id="6ae12-225">This example finds hello number of trips that were tipped vs. not tipped in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="6ae12-226">Ehhez a terjesztéshez hello bináris címke terjesztési toobe később használatos bináris osztályozási modellezési tükrözi.</span><span class="sxs-lookup"><span data-stu-id="6ae12-226">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="6ae12-227">Feltárása: Az osztály vagy tartomány terjesztési tipp</span><span class="sxs-lookup"><span data-stu-id="6ae12-227">Exploration: Tip Class/Range Distribution</span></span>
<span data-ttu-id="6ae12-228">Ebben a példában egy adott idő alatt hello tipp tartományok terjesztése kiszámítja az időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó).</span><span class="sxs-lookup"><span data-stu-id="6ae12-228">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="6ae12-229">Ez a hello címke osztályokkal rendelkezik, amelyek később használandó multiclass besorolás modellezési hello terjesztése.</span><span class="sxs-lookup"><span data-stu-id="6ae12-229">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="6ae12-230">Feltárása: A számítási, és hasonlítsa össze a út távolság</span><span class="sxs-lookup"><span data-stu-id="6ae12-230">Exploration: Compute and Compare Trip Distance</span></span>
<span data-ttu-id="6ae12-231">Ebben a példában a felvételi és Gyűjtőtár hosszúság hello alakítja át, és tooSQL a földrajzi szélesség mutat, hello út távolság SQL geográfiai pontok különbség használatával kiszámítja és visszaadja az összehasonlításhoz hello eredmények véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="6ae12-231">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="6ae12-232">hello példa eredményekre hello toovalid koordinálja, csak a hello minőségi assessment adatlekérdezés kezelt korábban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-232">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a><span data-ttu-id="6ae12-233">Az SQL-lekérdezések mérnöki szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="6ae12-233">Feature Engineering in SQL Queries</span></span>
<span data-ttu-id="6ae12-234">hello címke generációs és földrajzi átalakítás feltárása lekérdezéseket is használt toogenerate címkék/szolgáltatások része számbavételi hello eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="6ae12-234">hello label generation and geography conversion exploration queries can also be used toogenerate labels/features by removing hello counting part.</span></span> <span data-ttu-id="6ae12-235">A szolgáltatás további mérnöki SQL példák hello szerepelnek [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-235">Additional feature engineering SQL examples are provided in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span> <span data-ttu-id="6ae12-236">Hatékonyabb toorun hello szolgáltatás generációs lekérdezések teljes adatkészlet hello vagy egy nagy részét, közvetlenül a hello SQL Server adatbázis-példány az SQL-lekérdezések használatával is.</span><span class="sxs-lookup"><span data-stu-id="6ae12-236">It is more efficient toorun hello feature generation queries on hello full dataset or a large subset of it using SQL queries which run directly on hello SQL Server database instance.</span></span> <span data-ttu-id="6ae12-237">hello lekérdezéseket is végrehajtható a **SQL Server Management Studio**, IPython Notebook vagy semmilyen eszköz/fejlesztőkörnyezet amely elérheti hello adatbázis helyileg vagy távolról.</span><span class="sxs-lookup"><span data-stu-id="6ae12-237">hello queries may be executed in **SQL Server Management Studio**, IPython Notebook or any development tool/environment which can access hello database locally or remotely.</span></span>

#### <a name="preparing-data-for-model-building"></a><span data-ttu-id="6ae12-238">Adatok előkészítése az modell létrehozásának</span><span class="sxs-lookup"><span data-stu-id="6ae12-238">Preparing Data for Model Building</span></span>
<span data-ttu-id="6ae12-239">hello következő lekérdezés illesztések hello **nyctaxi\_út** és **nyctaxi\_jegy ára** táblák, állít elő, a bináris besorolási címkék **Formabontó**, egy több osztály besorolási címke **tipp\_osztály**, és kinyeri 1 % véletlenszerűen hello teljes illesztett adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="6ae12-239">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a 1% random sample from hello full joined dataset.</span></span> <span data-ttu-id="6ae12-240">Ez a lekérdezés másolja, majd közvetlenül az hello beillesztett [Azure Machine Learning Studio](https://studio.azureml.net) [és adatokat importálhat] [ import-data] közvetlen adatfeldolgozást hello SQL Server adatbázis-modul az Azure-példányt.</span><span class="sxs-lookup"><span data-stu-id="6ae12-240">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL Server database instance in Azure.</span></span> <span data-ttu-id="6ae12-241">hello lekérdezés nem tartalmazza a rekordok helytelen (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="6ae12-241">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <span data-ttu-id="6ae12-242"><a name="ipnb"></a>Az adatok feltárása, a szolgáltatás fejlesztés IPython jegyzetfüzet</span><span class="sxs-lookup"><span data-stu-id="6ae12-242"><a name="ipnb"></a>Data Exploration and Feature Engineering in IPython Notebook</span></span>
<span data-ttu-id="6ae12-243">Ebben a szakaszban végezzük el a Python és az SQL lekérdezések írásában, korábban létrehozott hello SQL Server-adatbázis használata a szolgáltatás generálása és az adatok feltárása.</span><span class="sxs-lookup"><span data-stu-id="6ae12-243">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL Server database created earlier.</span></span> <span data-ttu-id="6ae12-244">Egy minta IPython notebook nevű **machine-Learning-data-science-process-sql-story.ipynb** hello megadott **minta IPython notebookok** mappa.</span><span class="sxs-lookup"><span data-stu-id="6ae12-244">A sample IPython notebook named **machine-Learning-data-science-process-sql-story.ipynb** is provided in hello **Sample IPython Notebooks** folder.</span></span> <span data-ttu-id="6ae12-245">A notebook is rendelkezésre áll, a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span><span class="sxs-lookup"><span data-stu-id="6ae12-245">This notebook is also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span></span>

<span data-ttu-id="6ae12-246">big Data típusú adatok használata ajánlott az feladatütemezési hello hello következő:</span><span class="sxs-lookup"><span data-stu-id="6ae12-246">hello recommended sequence when working with big data is hello following:</span></span>

* <span data-ttu-id="6ae12-247">Olvassa el egy a memóriában levő keretbe hello adatok mintát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-247">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="6ae12-248">Hajtsa végre az egyes képi megjelenítéseket, és segítségével explorations hello mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-248">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="6ae12-249">Összehasonlítást az adatminta szolgáltatás mérnöki hello segítségével a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="6ae12-249">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="6ae12-250">A nagyobb az adatok feltárása adatkezelési és a szolgáltatás mérnöki csapathoz, használjon Python tooissue SQL lekérdezések közvetlenül hello SQL Server-adatbázis hello Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="6ae12-250">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL Server database in hello Azure VM.</span></span>
* <span data-ttu-id="6ae12-251">Döntse el, az Azure Machine Learning modell létrehozásának hello minta mérete toouse.</span><span class="sxs-lookup"><span data-stu-id="6ae12-251">Decide hello sample size toouse for Azure Machine Learning model building.</span></span>

<span data-ttu-id="6ae12-252">Ha készen áll a Machine Learning tooproceed tooAzure, vagy lehet:</span><span class="sxs-lookup"><span data-stu-id="6ae12-252">When ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="6ae12-253">Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-253">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="6ae12-254">Ez a módszer mutatják be hello [épület modellek az Azure Machine Learning](#mlmodel) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-254">This method is demonstrated in hello [Building Models in Azure Machine Learning](#mlmodel) section.</span></span>    
2. <span data-ttu-id="6ae12-255">Hello mintát és azt tervezi, hogy az új adatbázistábla létrehozása modell toouse visszafejtett adatokat, majd használja hello új tábla hello [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="6ae12-255">Persist hello sampled and engineered data you plan toouse for model building in a new database table, then use hello new table in hello [Import Data][import-data] module.</span></span>

<span data-ttu-id="6ae12-256">hello az alábbiakban néhány adatok feltárása, adatábrázolási és példák mérnöki szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ae12-256">hello following are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="6ae12-257">További példákért lásd hello minta SQL IPython jegyzetfüzetet hello **minta IPython notebookok** mappa.</span><span class="sxs-lookup"><span data-stu-id="6ae12-257">For more examples, see hello sample SQL IPython notebook in hello **Sample IPython Notebooks** folder.</span></span>

#### <a name="initialize-database-credentials"></a><span data-ttu-id="6ae12-258">Adatbázis-hitelesítő adatok inicializálása</span><span class="sxs-lookup"><span data-stu-id="6ae12-258">Initialize Database Credentials</span></span>
<span data-ttu-id="6ae12-259">Az adatbázis-kapcsolati beállításokat a következő változók hello inicializálása:</span><span class="sxs-lookup"><span data-stu-id="6ae12-259">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a><span data-ttu-id="6ae12-260">Adatbázis-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-260">Create Database Connection</span></span>
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="6ae12-261">Sorok és oszlopok a tábla nyctaxi_trip jelentés száma</span><span class="sxs-lookup"><span data-stu-id="6ae12-261">Report number of rows and columns in table nyctaxi_trip</span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="6ae12-262">A sorok teljes számát = 173179759</span><span class="sxs-lookup"><span data-stu-id="6ae12-262">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="6ae12-263">Az oszlopok teljes száma = 14</span><span class="sxs-lookup"><span data-stu-id="6ae12-263">Total number of columns = 14</span></span>

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a><span data-ttu-id="6ae12-264">Olvassa el a hello SQL Server-adatbázis egy kisméretű mintát</span><span class="sxs-lookup"><span data-stu-id="6ae12-264">Read-in a small data sample from hello SQL Server Database</span></span>
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="6ae12-265">Idő tooread hello mintatáblázat: 6.492000 másodperc</span><span class="sxs-lookup"><span data-stu-id="6ae12-265">Time tooread hello sample table is 6.492000 seconds</span></span>  
<span data-ttu-id="6ae12-266">A beolvasott sorok és oszlopok száma = (84952, 21)</span><span class="sxs-lookup"><span data-stu-id="6ae12-266">Number of rows and columns retrieved = (84952, 21)</span></span>

#### <a name="descriptive-statistics"></a><span data-ttu-id="6ae12-267">Leíró statisztika</span><span class="sxs-lookup"><span data-stu-id="6ae12-267">Descriptive Statistics</span></span>
<span data-ttu-id="6ae12-268">Most állnak készen tooexplore mintát hello adatok.</span><span class="sxs-lookup"><span data-stu-id="6ae12-268">Now are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="6ae12-269">Először a hello leíró statisztika megnézi **út\_távolság** (vagy bármely más) mezőből:</span><span class="sxs-lookup"><span data-stu-id="6ae12-269">We start with looking at descriptive statistics for hello **trip\_distance** (or any other) field(s):</span></span>

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a><span data-ttu-id="6ae12-270">A képi megjelenítés: Rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="6ae12-270">Visualization: Box Plot Example</span></span>
<span data-ttu-id="6ae12-271">Úgy tekintünk hello Dobozdiagram hello út távolság toovisualize hello quantiles a Tovább gombra</span><span class="sxs-lookup"><span data-stu-id="6ae12-271">Next we look at hello box plot for hello trip distance toovisualize hello quantiles</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Tőzsdei #1][1]

#### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="6ae12-273">A képi megjelenítés: Terjesztési rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="6ae12-273">Visualization: Distribution Plot Example</span></span>
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tőzsdei #2][2]

#### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="6ae12-275">A képi megjelenítés: Sáv és sor előkészítésére</span><span class="sxs-lookup"><span data-stu-id="6ae12-275">Visualization: Bar and Line Plots</span></span>
<span data-ttu-id="6ae12-276">Ebben a példában a Microsoft hello út távolság az öt bins bin és hello dobozolás eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6ae12-276">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="6ae12-277">A Microsoft hello fent egy sávon bin terjesztési tőzsdei vagy sor a következő ábra</span><span class="sxs-lookup"><span data-stu-id="6ae12-277">We can plot hello above bin distribution in a bar or line plot as below</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

#### <a name="visualization-scatterplot-example"></a><span data-ttu-id="6ae12-280">A képi megjelenítés: Scatterplot – példa</span><span class="sxs-lookup"><span data-stu-id="6ae12-280">Visualization: Scatterplot Example</span></span>
<span data-ttu-id="6ae12-281">Pontdiagram rajzot közötti megmutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** toosee bármely korrelációs esetén</span><span class="sxs-lookup"><span data-stu-id="6ae12-281">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

<span data-ttu-id="6ae12-283">Hasonlóképpen azt hello közötti kapcsolat ellenőrzése **arány\_kód** és **út\_távolság**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-283">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="sub-sampling-hello-data-in-sql"></a><span data-ttu-id="6ae12-285">Alárendelt mintavételi hello SQL-adatok</span><span class="sxs-lookup"><span data-stu-id="6ae12-285">Sub-Sampling hello Data in SQL</span></span>
<span data-ttu-id="6ae12-286">Adatok létrehozása modell előkészítésekor [Azure Machine Learning Studio](https://studio.azureml.net), vagy dönthet a hello **SQL lekérdezés toouse közvetlenül az adatok importálása modul hello** vagy hello fejthetők vissza és mintát megőrzése új táblát, amely hello használhat adatok [és adatokat importálhat] [ import-data] modul egy egyszerű **kiválasztása * FROM < a\_új\_tábla\_neve >**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-286">When preparing data for model building in [Azure Machine Learning Studio](https://studio.azureml.net), you may either decide on hello **SQL query toouse directly in hello Import Data module** or persist hello engineered and sampled data in a new table, which you could use in hello [Import Data][import-data] module with a simple **SELECT * FROM <your\_new\_table\_name>**.</span></span>

<span data-ttu-id="6ae12-287">Ebben a szakaszban létrehozunk egy új tábla toohold hello mintát, és fejthetők vissza az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-287">In this section we will create a new table toohold hello sampled and engineered data.</span></span> <span data-ttu-id="6ae12-288">Például egy közvetlen SQL-lekérdezés a modell létrehozásának megadott hello [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-288">An example of a direct SQL query for model building is provided in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a><span data-ttu-id="6ae12-289">Hozzon létre egy minta-tábla és a feltöltési hello csatlakoztatott táblák 1 %.</span><span class="sxs-lookup"><span data-stu-id="6ae12-289">Create a Sample Table and Populate with 1% of hello Joined Tables.</span></span> <span data-ttu-id="6ae12-290">Eldobni a tábla első, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="6ae12-290">Drop Table First if it Exists.</span></span>
<span data-ttu-id="6ae12-291">Ez a szakasz azt csatlakozás hello táblák **nyctaxi\_út** és **nyctaxi\_jegy ára**, bontsa ki a 1 % véletlenszerűen és egy új tábla nevében mintát hello adatok megőrzése  **nyctaxi\_egy\_százalék**:</span><span class="sxs-lookup"><span data-stu-id="6ae12-291">In this section, we join hello tables **nyctaxi\_trip** and **nyctaxi\_fare**, extract a 1% random sample, and persist hello sampled data in a new table name **nyctaxi\_one\_percent**:</span></span>

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="6ae12-292">Az SQL-lekérdezések IPython Notebook használatával az adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="6ae12-292">Data Exploration using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="6ae12-293">Ebben a részben azt megismerkedhet adatok azokat a terjesztéseket használatával hello mintát 1 % adat, amely a fenti létrehozott hello új tábla megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="6ae12-293">In this section, we explore data distributions using hello 1% sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="6ae12-294">Vegye figyelembe, hogy hasonló explorations használatával végezheti el hello eredeti táblák, külön kérésre mintázatot használ **TABLESAMPLE** toolimit hello feltárása minta vagy hello korlátozásával eredmények hello időszakának megadott tooa **felvétel \_datetime** particionálja a hello [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-294">Note that similar explorations can be performed using hello original tables, optionally using **TABLESAMPLE** toolimit hello exploration sample or by limiting hello results tooa given time period using hello **pickup\_datetime** partitions, as illustrated in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="6ae12-295">Feltárása: Való adatváltások számát napi eloszlásáról</span><span class="sxs-lookup"><span data-stu-id="6ae12-295">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="6ae12-296">Feltárása: Utazás per medallion terjesztési</span><span class="sxs-lookup"><span data-stu-id="6ae12-296">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="6ae12-297">Az SQL-lekérdezések használatával IPython jegyzetfüzet szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-297">Feature Generation Using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="6ae12-298">Ez a szakasz az új címkék fog létrehozni, és közvetlenül az SQL-lekérdezések használatával funkciókat, hello 1 % mintatáblázat működő létrehozott hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6ae12-298">In this section we will generate new labels and features directly using SQL queries, operating on hello 1% sample table we created in hello previous section.</span></span>

#### <a name="label-generation-generate-class-labels"></a><span data-ttu-id="6ae12-299">Címke generációs: Osztály címkék létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-299">Label Generation: Generate Class Labels</span></span>
<span data-ttu-id="6ae12-300">A következő példa hello létre azt a modellezési címkék toouse két csoportja:</span><span class="sxs-lookup"><span data-stu-id="6ae12-300">In hello following example, we generate two sets of labels toouse for modeling:</span></span>

1. <span data-ttu-id="6ae12-301">Bináris osztály címkék **Formabontó** (ha tipp kapnak becslése)</span><span class="sxs-lookup"><span data-stu-id="6ae12-301">Binary Class Labels **tipped** (predicting if a tip will be given)</span></span>
2. <span data-ttu-id="6ae12-302">Multiclass címkék **tipp\_osztály** (előrejelzésére hello tipp bin vagy tartomány)</span><span class="sxs-lookup"><span data-stu-id="6ae12-302">Multiclass Labels **tip\_class** (predicting hello tip bin or range)</span></span>
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a><span data-ttu-id="6ae12-303">A szolgáltatás műszaki osztály: Száma szolgáltatások Kategorikus oszlopokhoz</span><span class="sxs-lookup"><span data-stu-id="6ae12-303">Feature Engineering: Count Features for Categorical Columns</span></span>
<span data-ttu-id="6ae12-304">Ebben a példában egy numerikus mezőbe kategorikus mező átalakítja azáltal, hogy minden kategória hello hello adatok az előfordulások száma.</span><span class="sxs-lookup"><span data-stu-id="6ae12-304">This example transforms a categorical field into a numeric field by replacing each category with hello count of its occurrences in hello data.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a><span data-ttu-id="6ae12-305">A szolgáltatás műszaki osztály: Bin szolgáltatások numerikus oszlopokhoz</span><span class="sxs-lookup"><span data-stu-id="6ae12-305">Feature Engineering: Bin features for Numerical Columns</span></span>
<span data-ttu-id="6ae12-306">Ebben a példában egy folyamatos számmező átalakítja az előre definiált kategória tartományokat, azaz átalakító számmező kategorikus mező be azokat.</span><span class="sxs-lookup"><span data-stu-id="6ae12-306">This example transforms a continuous numeric field into preset category ranges, i.e., transform numeric field into a categorical field.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a><span data-ttu-id="6ae12-307">A szolgáltatás műszaki osztály: Hely szolgáltatások kinyerése decimális szélesség/hosszúság</span><span class="sxs-lookup"><span data-stu-id="6ae12-307">Feature Engineering: Extract Location Features from Decimal Latitude/Longitude</span></span>
<span data-ttu-id="6ae12-308">Ebben a példában megszakad a szélességi és/vagy hosszúsági mező decimális ábrázolása hello több régióban mezőibe eltérő a granularitási, többek között ország város, város, letiltása, stb. Vegye figyelembe, hogy hello új földrajzi-mezők nem leképezett tooactual helyét.</span><span class="sxs-lookup"><span data-stu-id="6ae12-308">This example breaks down hello decimal representation of a latitude and/or longitude field into multiple region fields of different granularity, such as, country, city, town, block, etc. Note that hello new geo-fields are not mapped tooactual locations.</span></span> <span data-ttu-id="6ae12-309">Leképezési geocode helyeken további információkért lásd: [Bing Maps REST szolgáltatások](https://msdn.microsoft.com/library/ff701710.aspx).</span><span class="sxs-lookup"><span data-stu-id="6ae12-309">For information on mapping geocode locations, see [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="6ae12-310">Hello végső űrlap hello featurized tábla ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6ae12-310">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

<span data-ttu-id="6ae12-311">Dolgozunk most már készen áll a tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="6ae12-311">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="6ae12-312">hello adatok készen áll a hello előrejelzés probléma azonosított korábbi, nevezetesen:</span><span class="sxs-lookup"><span data-stu-id="6ae12-312">hello data is ready for any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="6ae12-313">Bináris osztályozás: toopredict tipp egy út kifizetett-e.</span><span class="sxs-lookup"><span data-stu-id="6ae12-313">Binary classification: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="6ae12-314">Multiclass besorolási: toopredict hello tartomány tipp fizetős, toohello korábban definiált osztályok szerint.</span><span class="sxs-lookup"><span data-stu-id="6ae12-314">Multiclass classification: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="6ae12-315">Regressziós feladat: toopredict hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="6ae12-315">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="6ae12-316"><a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6ae12-316"><a name="mlmodel"></a>Building Models in Azure Machine Learning</span></span>
<span data-ttu-id="6ae12-317">toobegin hello modellezési a gyakorlatban tooyour Azure Machine Learning-munkaterület bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="6ae12-317">toobegin hello modeling exercise, log in tooyour Azure Machine Learning workspace.</span></span> <span data-ttu-id="6ae12-318">Ha még nem hozott létre machine learning-munkaterület, lásd: [hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="6ae12-318">If you have not yet created a machine learning workspace, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="6ae12-319">az Azure Machine Learning lépései tooget lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="6ae12-319">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="6ae12-320">Jelentkezzen be túl[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="6ae12-320">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="6ae12-321">hello Studio kezdőlap számos olyan információt, videók, oktatóanyagok, hivatkozások toohello modulok hivatkozás és egyéb erőforrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="6ae12-321">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="6ae12-322">Fore Azure Machine Learning kapcsolatos további információkért tekintse meg a hello [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="6ae12-322">Fore more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="6ae12-323">Egy tipikus tanítási kísérletet hello következő tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="6ae12-323">A typical training experiment consists of hello following:</span></span>

1. <span data-ttu-id="6ae12-324">Hozzon létre egy **+ új** kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="6ae12-324">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="6ae12-325">Hello adatok tooAzure Machine Learning beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6ae12-325">Get hello data tooAzure Machine Learning.</span></span>
3. <span data-ttu-id="6ae12-326">Előre feldolgozzák, átalakítás és kezelhetők hello adatok, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="6ae12-326">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="6ae12-327">Szolgáltatások létrehozása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="6ae12-327">Generate features as needed.</span></span>
5. <span data-ttu-id="6ae12-328">Hello adatok felosztása adatkészletek képzési/érvényesítési/tesztelése (külön adatkészleteket, vagy az egyes).</span><span class="sxs-lookup"><span data-stu-id="6ae12-328">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="6ae12-329">Válasszon egy vagy több számítógépet tanulási algoritmus attól függően, hogy a probléma toosolve tanulási hello.</span><span class="sxs-lookup"><span data-stu-id="6ae12-329">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="6ae12-330">Például bináris osztályozási multiclass besorolás, regressziós.</span><span class="sxs-lookup"><span data-stu-id="6ae12-330">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="6ae12-331">Hello képzési adatkészlet egy vagy több modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="6ae12-331">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="6ae12-332">Pontszám hello érvényesítési dataset hello betanított modellek segítségével.</span><span class="sxs-lookup"><span data-stu-id="6ae12-332">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="6ae12-333">Hello modellek toocompute hello vonatkozó metrikáinak probléma tanulási hello kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="6ae12-333">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="6ae12-334">Konfigurálva finomhangolhatják a hello modellek és select hello legjobb modell toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6ae12-334">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="6ae12-335">Ebben a gyakorlatban azt már megismerkedett és fejthetők vissza az SQL Server hello adatokat, és úgy döntött, a hello minta mérete tooingest az Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6ae12-335">In this exercise, we have already explored and engineered hello data in SQL Server, and decided on hello sample size tooingest in Azure Machine Learning.</span></span> <span data-ttu-id="6ae12-336">egy vagy több hello előrejelzési modellek toobuild döntöttünk:</span><span class="sxs-lookup"><span data-stu-id="6ae12-336">toobuild one or more of hello prediction models we decided:</span></span>

1. <span data-ttu-id="6ae12-337">Hello adatok tooAzure Machine Learning beolvasása hello segítségével [és adatokat importálhat] [ import-data] modul hello elérhető **bemeneti és kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ae12-337">Get hello data tooAzure Machine Learning using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="6ae12-338">További információkért lásd: hello [és adatokat importálhat] [ import-data] modul referencialapja.</span><span class="sxs-lookup"><span data-stu-id="6ae12-338">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Az Azure Machine Learning-adatok beolvasása][17]
2. <span data-ttu-id="6ae12-340">Válassza ki **Azure SQL Database** , hello **adatforrás** a hello **tulajdonságok** panel.</span><span class="sxs-lookup"><span data-stu-id="6ae12-340">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="6ae12-341">Adja meg a hello adatbázis DNS-név hello **adatbázis-kiszolgáló neve** mező.</span><span class="sxs-lookup"><span data-stu-id="6ae12-341">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="6ae12-342">Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="6ae12-342">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="6ae12-343">Adja meg a hello **adatbázisnév** hello megfelelő mezőben.</span><span class="sxs-lookup"><span data-stu-id="6ae12-343">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="6ae12-344">Adja meg a hello **SQL felhasználónév** hello a ** felhasználói aqccount kiszolgálónév és a jelszó hello hello **kiszolgáló felhasználói fiók jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-344">Enter hello **SQL user name** in hello **Server user aqccount name, and hello password in hello **Server user account password**.</span></span>
6. <span data-ttu-id="6ae12-345">Ellenőrizze **fogadja el a kiszolgálói tanúsítvány** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6ae12-345">Check **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="6ae12-346">A hello **adatbázis-lekérdezés** szerkesztése területre, és beillesztheti hello szükséges adatbázis-(beleértve a számított mezőket például hello címkék) mezőt, és le kivonatok minták hello adatok szükséges toohello mintaméret hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="6ae12-346">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="6ae12-347">Példa egy bináris osztályozási kísérletet, közvetlenül a hello SQL Server-adatbázis adatainak olvasása: hello az alábbi ábra a.</span><span class="sxs-lookup"><span data-stu-id="6ae12-347">An example of a binary classification experiment reading data directly from hello SQL Server database is in hello figure below.</span></span> <span data-ttu-id="6ae12-348">Hasonló kísérletek multiclass besorolás és a visszavonási lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6ae12-348">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Az Azure Machine Learning vonat][10]

> [!IMPORTANT]
> <span data-ttu-id="6ae12-350">Az adatok kinyerése modellezési, és a korábbi szakaszokban bemutatott lekérdezés példák mintavételi hello **hello három modellezési gyakorlatok az összes felirathoz szerepelnek hello lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-350">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="6ae12-351">Az egyes gyakorlatok modellezési hello (kötelező) fontos lépés túl van**kizárása** hello szükségtelen címkéit hello más két problémákat, és minden egyéb **kiszivárgásának cél**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-351">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="6ae12-352">A például bináris osztályozási használjon hello címke **Formabontó** , és amelyeket hello mezők **tipp\_osztály**, **tipp\_összeg**, és **teljes\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="6ae12-352">For e.g., when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="6ae12-353">Ez utóbbi hello cél kiszivárgásának, mivel azok utalnak hello tipp fizetett.</span><span class="sxs-lookup"><span data-stu-id="6ae12-353">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="6ae12-354">felesleges oszlopok tooexclude és/vagy a cél kiszivárgásának, használhatja a hello [Select Columns in Dataset] [ select-columns] modul vagy hello [szerkesztése metaadatok] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="6ae12-354">tooexclude unnecessary columns and/or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="6ae12-355">További információkért lásd: [Select Columns in Dataset] [ select-columns] és [szerkesztése metaadatok] [ edit-metadata] lapok hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="6ae12-355">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="6ae12-356"><a name="mldeploy"></a>Az Azure Machine Learning modellek telepítéséről</span><span class="sxs-lookup"><span data-stu-id="6ae12-356"><a name="mldeploy"></a>Deploying Models in Azure Machine Learning</span></span>
<span data-ttu-id="6ae12-357">Amikor készen áll a modell, könnyedén telepítheti azt közvetlenül a hello kísérlet webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="6ae12-357">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="6ae12-358">További információ az Azure Machine Learning webszolgáltatások üzembe helyezése: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6ae12-358">For more information about deploying Azure Machine Learning web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="6ae12-359">egy új webszolgáltatás-bővítmény toodeploy, kell:</span><span class="sxs-lookup"><span data-stu-id="6ae12-359">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="6ae12-360">A pontozási kísérlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6ae12-360">Create a scoring experiment.</span></span>
2. <span data-ttu-id="6ae12-361">Hello webes szolgáltatás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="6ae12-361">Deploy hello web service.</span></span>

<span data-ttu-id="6ae12-362">a pontozási kísérletezhet a toocreate egy **befejezett** betanítása kísérletet, kattintson **létrehozása pontozási KÍSÉRLETEZHET** hello alacsonyabb művelet sávon.</span><span class="sxs-lookup"><span data-stu-id="6ae12-362">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Pontozás az Azure][18]

<span data-ttu-id="6ae12-364">Az Azure Machine Learning toocreate egy pontozási kísérletet hello tanítási kísérletet hello összetevői alapján kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="6ae12-364">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="6ae12-365">Különösen a következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="6ae12-365">In particular, it will:</span></span>

1. <span data-ttu-id="6ae12-366">Hello betanított modell mentéséhez, és távolítsa el a hello modell képzési modulok.</span><span class="sxs-lookup"><span data-stu-id="6ae12-366">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="6ae12-367">Azonosítsa a logikai **bemeneti port** toorepresent hello várt bemeneti adatok séma.</span><span class="sxs-lookup"><span data-stu-id="6ae12-367">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="6ae12-368">Azonosítsa a logikai **kimeneti port** toorepresent hello várt webes szolgáltatás kimeneti sémával.</span><span class="sxs-lookup"><span data-stu-id="6ae12-368">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="6ae12-369">Amikor hello pontozási kísérlet jön létre, ellenőrzésre, majd szükség szerint állítsa be.</span><span class="sxs-lookup"><span data-stu-id="6ae12-369">When hello scoring experiment is created, review it and adjust as needed.</span></span> <span data-ttu-id="6ae12-370">Egy tipikus módosítás tooreplace hello bemeneti adatkészlet és/vagy entitáselem, amely kizárja a címke mezők esetén, mert ezek nem lesz elérhető hello szolgáltatás meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="6ae12-370">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="6ae12-371">Akkor is hello célszerű tooreduce hello méretet adjon meg adatkészlet és/vagy lekérdezési tooa néhány rekordjának, éppen elegendő tooindicate hello bemeneti sémát.</span><span class="sxs-lookup"><span data-stu-id="6ae12-371">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="6ae12-372">A hello kimeneti portra, azt közös tooexclude összes beviteli mezőt, és csak terjed hello **pontozott címkék** és **program pontozza a mennyiségeket valószínűség** hello hello használja a [Select Columns in Dataset ] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="6ae12-372">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="6ae12-373">Kísérlet pontozási minta hello az alábbi ábra van.</span><span class="sxs-lookup"><span data-stu-id="6ae12-373">A sample scoring experiment is in hello figure below.</span></span> <span data-ttu-id="6ae12-374">Ha készen áll a toodeploy, kattintson a hello **WEBSZOLGÁLTATÁS közzététele** hello alacsonyabb művelet található gombra.</span><span class="sxs-lookup"><span data-stu-id="6ae12-374">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Az Azure gépi tanulás közzététele][11]

<span data-ttu-id="6ae12-376">toorecap, ez a forgatókönyv az oktatóanyagban hozott létre az Azure data tudományos környezethez, nagy nyilvános adatbázisból származó adatok megszerzése toomodel képzési és az Azure Machine Learning webszolgáltatás üzembe helyezése összes hello módon együttműködik.</span><span class="sxs-lookup"><span data-stu-id="6ae12-376">toorecap, in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset all hello way from data acquisition toomodel training and deploying of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="6ae12-377">Licencinformációk</span><span class="sxs-lookup"><span data-stu-id="6ae12-377">License Information</span></span>
<span data-ttu-id="6ae12-378">Ez a minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft hello MIT licence.</span><span class="sxs-lookup"><span data-stu-id="6ae12-378">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="6ae12-379">Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.</span><span class="sxs-lookup"><span data-stu-id="6ae12-379">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

### <a name="references"></a><span data-ttu-id="6ae12-380">Referencia</span><span class="sxs-lookup"><span data-stu-id="6ae12-380">References</span></span>
<span data-ttu-id="6ae12-381">• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="6ae12-381">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="6ae12-382">• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="6ae12-382">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="6ae12-383">• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="6ae12-383">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
