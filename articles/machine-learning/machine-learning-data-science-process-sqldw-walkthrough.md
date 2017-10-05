---
title: "Az Team tudományos folyamat működés közben: az SQL Data Warehouse |} Microsoft Docs"
description: "Bővített Analitikát folyamat és a technológia, működés közben"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;hangzh;weig
ms.openlocfilehash: ce7de48af0f2f21576c66a962b88635a0f9f8333
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a><span data-ttu-id="f4e8b-103">Az Team tudományos folyamat működés közben: az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f4e8b-103">The Team Data Science Process in action: using SQL Data Warehouse</span></span>
<span data-ttu-id="f4e8b-104">Az oktatóanyag azt ismerteti létrehozása és telepítése a gépi tanulási modellek az SQL Data Warehouse (SQL DW) keresztül egy nyilvánosan elérhető adatkészlet--a [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-104">In this tutorial, we walk you through building and deploying a machine learning model using SQL Data Warehouse (SQL DW) for a publicly available dataset -- the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="f4e8b-105">A bináris osztályozási modell összeállított képes-e tipp fizetnek útnak, és multiclass besorolás és regressziós modell, amely a terjesztési megjósolható a fizetős tipp adatmennyiség is ismertetése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-105">The binary classification model constructed predicts whether or not a tip is paid for a trip, and models for multiclass classification and regression are also discussed that predict the distribution for the tip amounts paid.</span></span>

<span data-ttu-id="f4e8b-106">Az eljárást követi a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-106">The procedure follows the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) workflow.</span></span> <span data-ttu-id="f4e8b-107">Megmutatjuk, hogyan állíthatja egy tudományos környezetben, az adatok betöltése az SQL DW, és hogyan használják az SQL DW vagy egy IPython Notebook Fedezze fel az adatokat és a visszafejtés funkciói szorulnak modell.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-107">We show how to setup a data science environment, how to load the data into SQL DW, and how use either SQL DW or an IPython Notebook to explore the data and engineer features to model.</span></span> <span data-ttu-id="f4e8b-108">Ezután megmutatjuk, hogyan lehet elkészíteni a modell Azure Machine Learning segítségével telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-108">We then show how to build and deploy a model with Azure Machine Learning.</span></span>

## <span data-ttu-id="f4e8b-109"><a name="dataset"></a>A NYC Taxi Utazgatással adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f4e8b-109"><a name="dataset"></a>The NYC Taxi Trips dataset</span></span>
<span data-ttu-id="f4e8b-110">A NYC Taxi út tartalmaz körülbelül 20GB tömörített CSV-fájlok (tömörítetlen ~ 48GB), minden út kifizette több mint 173 millió egyedi való adatváltások számát és a vitel rögzítése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-110">The NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="f4e8b-111">Minden út rekord tartalmazza a felvétel és Gyűjtőtár helyeket és időpontokat, anonimizált rejthetők el (illesztőprogram) licencszám, és a medallion (taxi tartozó egyedi azonosító) számát.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-111">Each trip record includes the pickup and drop-off locations and times, anonymized hack (driver's) license number, and the medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="f4e8b-112">Az adatok minden való adatváltások számát ismerteti az év 2013, és minden hónap a következő két adatkészletet találhatók:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-112">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="f4e8b-113">A **trip_data.csv** fájl tartalmazza út részleteit, például az utasok, a felvételi és dropoff pontok, út időtartama és út hossza száma.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-113">The **trip_data.csv** file contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="f4e8b-114">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="f4e8b-115">A **trip_fare.csv** fájl a jegy ára kifizette minden út, például a fizetési mód, jegy ára összeg, emelt díjas és adókat, tippeket és autópályadíjak, és a teljes összeg fizetős részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-115">The **trip_fare.csv** file contains details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="f4e8b-116">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="f4e8b-117">A **egyedi kulcs** történő csatlakozás út\_adatok és út\_jegy ára a következő három mezőből áll:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-117">The **unique key** used to join trip\_data and trip\_fare is composed of the following three fields:</span></span>

* <span data-ttu-id="f4e8b-118">medallion,</span><span class="sxs-lookup"><span data-stu-id="f4e8b-118">medallion,</span></span>
* <span data-ttu-id="f4e8b-119">ellophatja\_licenc és</span><span class="sxs-lookup"><span data-stu-id="f4e8b-119">hack\_license and</span></span>
* <span data-ttu-id="f4e8b-120">a felvételi\_datetime.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-120">pickup\_datetime.</span></span>

## <span data-ttu-id="f4e8b-121"><a name="mltasks"></a>Három típusú előrejelzés feladatok cím</span><span class="sxs-lookup"><span data-stu-id="f4e8b-121"><a name="mltasks"></a>Address three types of prediction tasks</span></span>
<span data-ttu-id="f4e8b-122">Azt a három előrejelzés problémák alapján állítson össze a *tipp\_összeg* három különböző feladatok modellezési mutatja be:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-122">We formulate three prediction problems based on the *tip\_amount* to illustrate three kinds of modeling tasks:</span></span>

1. <span data-ttu-id="f4e8b-123">**Bináris osztályozási**: előre jelezni-e tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_Összeg* $ 0 egy negatív példában látható.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-123">**Binary classification**: To predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="f4e8b-124">**Multiclass besorolási**: út kifizette tipp számos előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-124">**Multiclass classification**: To predict the range of tip paid for the trip.</span></span> <span data-ttu-id="f4e8b-125">Azt a osztani a *tipp\_összeg* öt bins vagy osztályok:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-125">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="f4e8b-126">**Regressziós feladat**: megjósolható a fizetős útnak tipp mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-126">**Regression task**: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="f4e8b-127"><a name="setup"></a>A speciális elemzésekre az Azure data tudományos környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-127"><a name="setup"></a>Set up the Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="f4e8b-128">Állítsa be a Azure Adattudomány környezetet, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-128">To set up your Azure Data Science environment, follow these steps.</span></span>

<span data-ttu-id="f4e8b-129">**A saját Azure blob storage-fiók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-129">**Create your own Azure blob storage account**</span></span>

* <span data-ttu-id="f4e8b-130">Ha a saját Azure blob-tároló, válassza ki az Azure blob storage a vagy a lehető legközelebb a földrajzi helymeghatározás **déli középső Régiójában**, vagyis a következőt: Taxi adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-130">When you provision your own Azure blob storage, choose a geo-location for your Azure blob storage in or as close as possible to **South Central US**, which is where the NYC Taxi data is stored.</span></span> <span data-ttu-id="f4e8b-131">Az adatok tárolót a nyilvános blob storage tárolóból AzCopy használatával saját tárfiókba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-131">The data will be copied using AzCopy from the public blob storage container to a container in your own storage account.</span></span> <span data-ttu-id="f4e8b-132">Minél közelebb az Azure blob storage déli középső Régiójában, minél gyorsabban (4. lépés) Ez a feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-132">The closer your Azure blob storage is to South Central US, the faster this task (Step 4) will be completed.</span></span>
* <span data-ttu-id="f4e8b-133">A saját Azure storage-fiók létrehozásához kövesse a következő részben ismertetett lépések [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-133">To create your own Azure storage account, follow the steps outlined at [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="f4e8b-134">Ne felejtse el megjegyzések adja meg a következő tárfiók hitelesítő adatainak értékeit, akkor ez az útmutató későbbi részében szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-134">Be sure to make notes on the values for following storage account credentials as they will be needed later in this walkthrough.</span></span>
  
  * <span data-ttu-id="f4e8b-135">**A tárfiók neve**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-135">**Storage Account Name**</span></span>
  * <span data-ttu-id="f4e8b-136">**Tárfiók kulcsa**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-136">**Storage Account Key**</span></span>
  * <span data-ttu-id="f4e8b-137">**Tároló neve** (amely az adatokat az Azure blob Storage tárolóban tárolni kívánt)</span><span class="sxs-lookup"><span data-stu-id="f4e8b-137">**Container Name** (which you want the data to be stored in the Azure blob storage)</span></span>

<span data-ttu-id="f4e8b-138">**Az Azure SQL DW-példányt kiépítéséhez.**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-138">**Provision your Azure SQL DW instance.**</span></span>
<span data-ttu-id="f4e8b-139">Kövesse a dokumentációban a [SQL Data Warehouse létrehozása](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) egy SQL Data Warehouse-példányokhoz kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-139">Follow the documentation at [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) to provision a SQL Data Warehouse instance.</span></span> <span data-ttu-id="f4e8b-140">Győződjön meg arról, hogy, hogy a következő SQL-adatraktár hitelesítő adatok a későbbi lépésekben használt jelölések.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-140">Make sure that you make notations on the following SQL Data Warehouse credentials which will be used in later steps.</span></span>

* <span data-ttu-id="f4e8b-141">**Kiszolgálónév**: <server Name>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="f4e8b-141">**Server Name**: <server Name>.database.windows.net</span></span>
* <span data-ttu-id="f4e8b-142">**SQLDW (adatbázis) neve**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-142">**SQLDW (Database) Name**</span></span>
* <span data-ttu-id="f4e8b-143">**Felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-143">**Username**</span></span>
* <span data-ttu-id="f4e8b-144">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-144">**Password**</span></span>

<span data-ttu-id="f4e8b-145">**Telepítse a Visual Studio és az SQL Server Data Tools összetevővel.**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-145">**Install Visual Studio and SQL Server Data Tools.**</span></span> <span data-ttu-id="f4e8b-146">Útmutatásért lásd: [Visual Studio 2015 telepítése és/vagy az SSDT (SQL Server Data Tools) az SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-146">For instructions, see [Install Visual Studio 2015 and/or SSDT (SQL Server Data Tools) for SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span></span>

<span data-ttu-id="f4e8b-147">**Csatlakozás az Azure SQL DW a Visual Studio.**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-147">**Connect to your Azure SQL DW with Visual Studio.**</span></span> <span data-ttu-id="f4e8b-148">Útmutatásért lásd: 1 és 2 lépéseket [csatlakozás az Azure SQL Data Warehouse a Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-148">For instructions, see steps 1 & 2 in [Connect to Azure SQL Data Warehouse with Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-149">Futtassa a következő SQL lekérdezést az adatbázis az SQL Data Warehouse (és nem a lekérdezés a connect témakör 3. lépésében megadott) létrehozott a **hozzon létre egy főkulcsot**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-149">Run the following SQL query on the database you created in your SQL Data Warehouse (instead of the query provided in step 3 of the connect topic,) to **create a master key**.</span></span>
> 
> 

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

<span data-ttu-id="f4e8b-150">**Hozzon létre egy Azure Machine Learning munkaterülettel alatt az Azure-előfizetéshez.**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-150">**Create an Azure Machine Learning workspace under your Azure subscription.**</span></span> <span data-ttu-id="f4e8b-151">Útmutatásért lásd: [hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-151">For instructions, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

## <span data-ttu-id="f4e8b-152"><a name="getdata"></a>Az adatok betöltése az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f4e8b-152"><a name="getdata"></a>Load the data into SQL Data Warehouse</span></span>
<span data-ttu-id="f4e8b-153">Nyisson meg egy Windows PowerShell-parancs konzolt.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-153">Open a Windows PowerShell command console.</span></span> <span data-ttu-id="f4e8b-154">Futtassa a következő PowerShell-parancsok futtatásával töltse le a példában SQL parancsfájlok, amelyek osztjuk meg Önnel a Githubon az paraméterrel megadott helyi könyvtárba *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-154">Run the following PowerShell commands to download the example SQL script files that we share with you on GitHub to a local directory that you specify with the parameter *-DestDir*.</span></span> <span data-ttu-id="f4e8b-155">Módosíthatja a paraméter értékének *- DestDir* bármely helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-155">You can change the value of parameter *-DestDir* to any local directory.</span></span> <span data-ttu-id="f4e8b-156">Ha *- DestDir* nem létezik, a rendszer automatikusan létrehozza a PowerShell parancsfájlhoz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-156">If *-DestDir* does not exist, it will be created by the PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-157">Szükség lehet **Futtatás rendszergazdaként** a következő PowerShell-parancsfájl végrehajtásakor, ha a *DestDir* directory létrehozásához vagy íráshoz rendszergazdai jogosultság szükséges.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-157">You might need to **Run as Administrator** when executing the following PowerShell script if your *DestDir* directory needs Administrator privilege to create or to write to it.</span></span>
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

<span data-ttu-id="f4e8b-158">Sikeres végrehajtása után az aktuális munkakönyvtárban vált *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-158">After successful execution, your current working directory changes to *-DestDir*.</span></span> <span data-ttu-id="f4e8b-159">Képernyőn láthatja el alatt, például:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-159">You should be able to see screen like below:</span></span>

![][19]

<span data-ttu-id="f4e8b-160">Az a *- DestDir*, a következő PowerShell-parancsfájl végrehajtása felügyeleti üzemmódban:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-160">In your *-DestDir*, execute the following PowerShell script in administrator mode:</span></span>

    ./SQLDW_Data_Import.ps1

<span data-ttu-id="f4e8b-161">Ha a PowerShell-parancsfájl futtatása először, a rendszer kéri, hogy adjon meg az Azure SQL DW és az Azure blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-161">When the PowerShell script runs for the first time, you will be asked to input the information from your Azure SQL DW and your Azure blob storage account.</span></span> <span data-ttu-id="f4e8b-162">A PowerShell parancsfájl befejeződésekor először, a hitelesítő adatok futtató meg bemeneti fog készült SQLDW.conf konfigurációs fájlt a jelenlegi munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-162">When this PowerShell script completes running for the first time, the credentials you input will have been written to a configuration file SQLDW.conf in the present working directory.</span></span> <span data-ttu-id="f4e8b-163">A jövőbeli Futtatás a PowerShell parancsfájl az összes szükséges paraméterek a konfigurációs fájlból olvasható lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-163">The future run of this PowerShell script file has the option to read all needed parameters from this configuration file.</span></span> <span data-ttu-id="f4e8b-164">Egyes paraméterek módosítani szeretné, ha paraméterei a kérés esetén a képernyő törölni a konfigurációs fájlt, és a bevitel a paraméterek értékét, a rendszer kéri a felhasználótól, vagy választhatja a paraméterértékeket a aSQLDW.conffájlszerkesztésévelmódosíthatja*- DestDir* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-164">If you need to change some parameters, you can choose to input the parameters on the screen upon prompt by deleting this configuration file and inputting the parameters values as prompted or to change the parameter values by editing the SQLDW.conf file in your *-DestDir* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-165">Ahhoz, hogy a séma neve ütközik az alábbiakhoz már létezik az Azure SQL DW, ha Fájlolvasási paraméterek közvetlenül a SQLDW.conf elkerülése érdekében 3 számjegyű véletlenszerűen kerül a séma nevének a SQLDW.conf fájlból minden egyes futtatásához alapértelmezett séma neveként.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-165">In order to avoid schema name conflicts with those that already exist in your Azure SQL DW, when reading parameters directly from the SQLDW.conf file, a 3-digit random number is added to the schema name from the SQLDW.conf file as the default schema name for each run.</span></span> <span data-ttu-id="f4e8b-166">A PowerShell parancsfájl kérheti a séma nevének: felhasználói belátása neve adható meg.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-166">The PowerShell script may prompt you for a schema name: the name may be specified at user discretion.</span></span>
> 
> 

<span data-ttu-id="f4e8b-167">Ez **PowerShell-parancsfájl** fájl a következő feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-167">This **PowerShell script** file completes the following tasks:</span></span>

* <span data-ttu-id="f4e8b-168">**Letölti és telepíti az AzCopy**, ha az AzCopy nincs telepítve</span><span class="sxs-lookup"><span data-stu-id="f4e8b-168">**Downloads and installs AzCopy**, if AzCopy is not already installed</span></span>
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* <span data-ttu-id="f4e8b-169">**Másolja az adatokat a személyes blob storage-fiók** az AzCopy nyilvános blob</span><span class="sxs-lookup"><span data-stu-id="f4e8b-169">**Copies data to your private blob storage account** from the public blob with AzCopy</span></span>
  
        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* <span data-ttu-id="f4e8b-170">**A Polybase (LoadDataToSQLDW.sql végrehajtásával) segítségével az Azure SQL DW adatokat tölt** az az alábbi parancsokkal a titkos blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-170">**Loads data using Polybase (by executing LoadDataToSQLDW.sql) to your Azure SQL DW** from your private blob storage account with the following commands.</span></span>
  
  * <span data-ttu-id="f4e8b-171">Hozzon létre egy séma</span><span class="sxs-lookup"><span data-stu-id="f4e8b-171">Create a schema</span></span>
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * <span data-ttu-id="f4e8b-172">Egy adatbázishoz kötődő hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-172">Create a database scoped credential</span></span>
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * <span data-ttu-id="f4e8b-173">Az Azure storage-blob egy külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-173">Create an external data source for an Azure storage blob</span></span>
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * <span data-ttu-id="f4e8b-174">Hozzon létre egy csv-fájl egy külső fájlformátum.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-174">Create an external file format for a csv file.</span></span> <span data-ttu-id="f4e8b-175">Adatok tömörítetlen és mezőket a függőleges vonal választják el egymástól.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-175">Data is uncompressed and fields are separated with the pipe character.</span></span>
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * <span data-ttu-id="f4e8b-176">Külső jegy ára és NYC taxi adatkészlet út táblák létrehozása az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-176">Create external fare and trip tables for NYC taxi dataset in Azure blob storage.</span></span>
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - <span data-ttu-id="f4e8b-177">Adatok betöltése az Azure blob storage külső táblát az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f4e8b-177">Load data from external tables in Azure blob storage to SQL Data Warehouse</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - <span data-ttu-id="f4e8b-178">Hozzon létre egy minta-adattábla (NYCTaxi_Sample), és adatok beszúrása azt az SQL-lekérdezések a út és a jegy ára táblák kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-178">Create a sample data table (NYCTaxi_Sample) and insert data to it from selecting SQL queries on the trip and fare tables.</span></span> <span data-ttu-id="f4e8b-179">(Ez az útmutató néhány lépése kell használnia a mintatáblát.)</span><span class="sxs-lookup"><span data-stu-id="f4e8b-179">(Some steps of this walkthrough needs to use this sample table.)</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

<span data-ttu-id="f4e8b-180">A storage-fiókok földrajzi helye a betöltési idők hatással van.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-180">The geographic location of your storage accounts affects load times.</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-181">Attól függően, hogy a titkos blob storage-fiók földrajzi elhelyezkedését, a folyamat egy nyilvános blob titkos tárfiókja adatok másolása körülbelül 15 percig is eltarthat, vagy akár hosszabb, és a korábbi tárfiókban lévő adatok betöltése az Azure-bA folyamata SQL DW 20 percet vehet igénybe, vagy hosszabb.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-181">Depending on the geographical location of your private blob storage account, the process of copying data from a public blob to your private storage account can take about 15 minutes, or even longer,and the process of loading data from your storage account to your Azure SQL DW could take 20 minutes or longer.</span></span>  
> 
> 

<span data-ttu-id="f4e8b-182">Döntse el, hogy milyen Ha ismétlődő a forrás és cél fájlokat fog.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-182">You will have to decide what do if you have duplicate source and destination files.</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-183">Ha a .csv-fájlokat másolni a nyilvános blob storage a saját blob storage-fiók már szerepel a titkos blob storage-fiók, AzCopy megkérdezi, hogy meg szeretné felülír.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-183">If the .csv files to be copied from the public blob storage to your private blob storage account already exist in your private blob storage account, AzCopy will ask you whether you want to overwrite them.</span></span> <span data-ttu-id="f4e8b-184">Ha nem szeretné, hogy felülírja, bemeneti  **n**  megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-184">If you do not want to overwrite them, input **n** when prompted.</span></span> <span data-ttu-id="f4e8b-185">Ha szeretné felülírni **összes** , bemeneti **egy** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-185">If you want to overwrite **all** of them, input **a** when prompted.</span></span> <span data-ttu-id="f4e8b-186">Is szövegszerkesztőben **y** .csv fájlok egyenként felülírásához.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-186">You can also input **y** to overwrite .csv files individually.</span></span>
> 
> 

![#21. megrajzolásához][21]

<span data-ttu-id="f4e8b-188">Használhatja a saját adatait.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-188">You can use your own data.</span></span> <span data-ttu-id="f4e8b-189">Ha az adatok a valós életben alkalmazásban a helyszíni gépen, AzCopy továbbra is használhatja a helyszíni adatok feltöltése a saját Azure blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-189">If your data is in your on-premises machine in your real life application, you can still use AzCopy to upload on-premises data to your private Azure blob storage.</span></span> <span data-ttu-id="f4e8b-190">Csak módosítani szeretné a **forrás** helyét, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, az AzCopy parancs a helyi könyvtárba, amely tartalmazza az adatokat a PowerShell parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-190">You only need to change the **Source** location, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in the AzCopy command of the PowerShell script file to the local directory that contains your data.</span></span>

> [!TIP]
> <span data-ttu-id="f4e8b-191">Ha az adatok a valós életben alkalmazás már a saját Azure blob Storage tárolóban, az AzCopy hagyja ki a PowerShell parancsfájl, és közvetlenül az adatokat az Azure SQL DW feltölteni.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-191">If your data is already in your private Azure blob storage in your real life application, you can skip the AzCopy step in the PowerShell script and directly upload the data to Azure SQL DW.</span></span> <span data-ttu-id="f4e8b-192">Ezt a beállítást a parancsfájl személyessé tétele érdekében, az adatok formátumának további műveleteket.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-192">This will require additional edits of the script to tailor it to the format of your data.</span></span>
> 
> 

<span data-ttu-id="f4e8b-193">A Powershell-parancsfájlt is csatlakozik az Azure SQL DW-információkat az adatok feltárása például fájlok SQLDW_Explorations.sql SQLDW_Explorations.ipynb és SQLDW_Explorations_Scripts.py, hogy ezek a fájlok kell próbált azonnal után készen áll a PowerShell parancsfájl lefutott.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-193">This Powershell script also plugs in the Azure SQL DW information into the data exploration example files SQLDW_Explorations.sql, SQLDW_Explorations.ipynb, and SQLDW_Explorations_Scripts.py so that these three files are ready to be tried out instantly after the PowerShell script completes.</span></span>

<span data-ttu-id="f4e8b-194">Egy sikeres végrehajtása után képernyőn megjelenik alá, például:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-194">After a successful execution, you will see screen like below:</span></span>

![][20]

## <span data-ttu-id="f4e8b-195"><a name="dbexplore"></a>Az adatok feltárása, a szolgáltatás fejlesztés az Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f4e8b-195"><a name="dbexplore"></a>Data exploration and feature engineering in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="f4e8b-196">Ebben a szakaszban szemben Azure SQL DW közvetlenül az SQL-lekérdezések futtatásával végezzük adatok feltárása és a szolgáltatás generálása **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-196">In this section, we perform data exploration and feature generation by running SQL queries against Azure SQL DW directly using **Visual Studio Data Tools**.</span></span> <span data-ttu-id="f4e8b-197">Ebben a szakaszban használt összes SQL-lekérdezések itt található: a minta parancsfájlt nevű *SQLDW_Explorations.sql*.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-197">All SQL queries used in this section can be found in the sample script named *SQLDW_Explorations.sql*.</span></span> <span data-ttu-id="f4e8b-198">Ez a fájl már letöltött a helyi könyvtárba a PowerShell parancsfájlhoz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-198">This file has already been downloaded to your local directory by the PowerShell script.</span></span> <span data-ttu-id="f4e8b-199">Azt is megtalálja a [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-199">You can also retrieve it from [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span></span> <span data-ttu-id="f4e8b-200">De a fájl a Githubban nincsenek-e a hálózathoz csatlakoztatva működik a Azure SQL DW-adatok.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-200">But the file in GitHub does not have the Azure SQL DW information plugged in.</span></span>

<span data-ttu-id="f4e8b-201">Az Azure SQL DW SQL DW-bejelentkezési nevet és jelszót a Visual Studio használatával csatlakozhat, és nyissa meg a **SQL Object Explorer** annak ellenőrzéséhez, hogy az adatbázis és a táblák importálta.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-201">Connect to your Azure SQL DW using Visual Studio with the SQL DW login name and password and open up the **SQL Object Explorer** to confirm the database and tables have been imported.</span></span> <span data-ttu-id="f4e8b-202">Beolvasni a *SQLDW_Explorations.sql* fájlt.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-202">Retrieve the *SQLDW_Explorations.sql* file.</span></span>

> [!NOTE]
> <span data-ttu-id="f4e8b-203">A Parallel Data Warehouse (PDW) lekérdezés-szerkesztő megnyitásához használja a **új lekérdezés** a PDW kiválasztása a parancsot a **SQL Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-203">To open a Parallel Data Warehouse (PDW) query editor, use the **New Query** command while your PDW is selected in the **SQL Object Explorer**.</span></span> <span data-ttu-id="f4e8b-204">A szokásos SQL lekérdezés-szerkesztő PDW által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-204">The standard SQL query editor is not supported by PDW.</span></span>
> 
> 

<span data-ttu-id="f4e8b-205">Az alábbiakban a típusú adatok feltárása és a szolgáltatás generálása feladatok végre ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-205">Here are the type of data exploration and feature generation tasks performed in this section:</span></span>

* <span data-ttu-id="f4e8b-206">Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="f4e8b-207">Vizsgálja meg a szélességi és hosszúsági mezők adatok minőségét.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-207">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="f4e8b-208">Multiclass és bináris besorolási címkék alapján készítése a **tipp\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-208">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="f4e8b-209">Szolgáltatások létrehozása és számítási/összehasonlítása út távolság.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="f4e8b-210">Csatlakozás a két tábla, és bontsa ki a modellek létrehozásához használt véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-210">Join the two tables and extract a random sample that will be used to build models.</span></span>

### <a name="data-import-verification"></a><span data-ttu-id="f4e8b-211">Az importálás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f4e8b-211">Data import verification</span></span>
<span data-ttu-id="f4e8b-212">Ezeket a lekérdezéseket, adja meg a sorok száma a gyors ellenőrzése és oszlopok a táblázatokban fel korábban segítségével a Polybase által párhuzamos tömeges importálásához</span><span class="sxs-lookup"><span data-stu-id="f4e8b-212">These queries provide a quick verification of the number of rows and columns in the tables populated earlier using Polybase's parallel bulk import,</span></span>

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

<span data-ttu-id="f4e8b-213">**Kimenet:** 173,179,759 sorok és oszlopok 14 kapja meg.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-213">**Output:** You should get 173,179,759 rows and 14 columns.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="f4e8b-214">Feltárása: Út elosztása medallion szerint</span><span class="sxs-lookup"><span data-stu-id="f4e8b-214">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="f4e8b-215">Ez a példa lekérdezés 100-nál több való adatváltások számát egy adott időszakon belül elvégzett medallions (taxi számok) azonosítja.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-215">This example query identifies the medallions (taxi numbers) that completed more than 100 trips within a specified time period.</span></span> <span data-ttu-id="f4e8b-216">A lekérdezés előnyös a particionált tábla hozzáférés óta, akkor annak partíciós sémája által **a felvételi\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-216">The query would benefit from the partitioned table access since it is conditioned by the partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="f4e8b-217">A teljes adatkészlet lekérdezése is teszi a particionált tábla használja, és/vagy a vizsgálat index.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-217">Querying the full dataset will also make use of the partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

<span data-ttu-id="f4e8b-218">**Kimenet:** a lekérdezés visszaadja-e egy tábla sorait a 13,369 medallions (taxikra) és egyéb 2013 befejezte út száma.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-218">**Output:** The query should return a table with rows specifying the 13,369 medallions (taxis) and the number of trip completed by them in 2013.</span></span> <span data-ttu-id="f4e8b-219">Az utolsó oszlopban befejeződött utak számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-219">The last column contains the count of the number of trips completed.</span></span>

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="f4e8b-220">Feltárása: Út terjesztési medallion és hack_license</span><span class="sxs-lookup"><span data-stu-id="f4e8b-220">Exploration: Trip distribution by medallion and hack_license</span></span>
<span data-ttu-id="f4e8b-221">Ebben a példában a medallions (taxi számok) azonosítja, és hack_license számok (-illesztőprogramok) az elvégzett több mint 100 való adatváltások számát egy adott időszakon belül.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-221">This example identifies the medallions (taxi numbers) and hack_license numbers (drivers) that completed more than 100 trips within a specified time period.</span></span>

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

<span data-ttu-id="f4e8b-222">**Kimenet:** a lekérdezés megadásával több befejeződött, hogy 100 utazás közben 2013 13,369 autó/illesztőprogram azonosítók 13,369 sorokból kell visszaadnia egy tábla.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-222">**Output:** The query should return a table with 13,369 rows specifying the 13,369 car/driver IDs that have completed more that 100 trips in 2013.</span></span> <span data-ttu-id="f4e8b-223">Az utolsó oszlopban befejeződött utak számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-223">The last column contains the count of the number of trips completed.</span></span>

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="f4e8b-224">Adatok vizsgálatának: helytelen hosszúsági és/vagy szélességi rekordjainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f4e8b-224">Data quality assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="f4e8b-225">Ez a példa pedig megvizsgálja, ha a hosszúsági és/vagy szélességi mezőinek vagy érvénytelen értéket tartalmazza (radián fok -90 és 90 között kell lennie), vagy (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-225">This example investigates if any of the longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

<span data-ttu-id="f4e8b-226">**Kimenet:** a lekérdezés érvénytelen hosszúsági és/vagy szélességi mezői lehetnek 837,467 való adatváltások számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-226">**Output:** The query returns 837,467 trips that have invalid longitude and/or latitude fields.</span></span>

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="f4e8b-227">Feltárása: Nem Formabontó utazgatással terjesztési és Formabontó</span><span class="sxs-lookup"><span data-stu-id="f4e8b-227">Exploration: Tipped vs. not tipped trips distribution</span></span>
<span data-ttu-id="f4e8b-228">Ez a példa megkeresi való adatváltások számát, amely volt Formabontó és a szám, amely nem volt Formabontó, egy adott időszakra vonatkozó (vagy a teljes adatkészletet, ha a teljes évre vonatkozó szerint van beállítva itt) száma.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-228">This example finds the number of trips that were tipped vs. the number that were not tipped in a specified time period (or in the full dataset if covering the full year as it is set up here).</span></span> <span data-ttu-id="f4e8b-229">Ehhez a terjesztéshez a bináris címke terjesztési használandó később bináris osztályozási modellezési tükrözi.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-229">This distribution reflects the binary label distribution to be later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

<span data-ttu-id="f4e8b-230">**Kimenet:** a lekérdezés visszaadja-e a következő tipp gyakoriságokat 2013: 90,447,622 Formabontó évben és 82,264,709 nem Formabontó.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-230">**Output:** The query should return the following tip frequencies for the year 2013: 90,447,622 tipped and 82,264,709 not-tipped.</span></span>

### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="f4e8b-231">Feltárása: Tipp osztály/tartomány terjesztési</span><span class="sxs-lookup"><span data-stu-id="f4e8b-231">Exploration: Tip class/range distribution</span></span>
<span data-ttu-id="f4e8b-232">Ebben a példában kiszámítja a terjesztési tipp tartományait egy adott időszakban (vagy a teljes adatkészletet, ha a teljes évre vonatkozó).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-232">This example computes the distribution of tip ranges in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="f4e8b-233">Ez az a terjesztési később használandó multiclass besorolás modellezési címke osztályok.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-233">This is the distribution of the label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

<span data-ttu-id="f4e8b-234">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="f4e8b-234">**Output:**</span></span>

| <span data-ttu-id="f4e8b-235">tip_class</span><span class="sxs-lookup"><span data-stu-id="f4e8b-235">tip_class</span></span> | <span data-ttu-id="f4e8b-236">tip_freq</span><span class="sxs-lookup"><span data-stu-id="f4e8b-236">tip_freq</span></span> |
| --- | --- |
| <span data-ttu-id="f4e8b-237">1</span><span class="sxs-lookup"><span data-stu-id="f4e8b-237">1</span></span> |<span data-ttu-id="f4e8b-238">82230915</span><span class="sxs-lookup"><span data-stu-id="f4e8b-238">82230915</span></span> |
| <span data-ttu-id="f4e8b-239">2</span><span class="sxs-lookup"><span data-stu-id="f4e8b-239">2</span></span> |<span data-ttu-id="f4e8b-240">6198803</span><span class="sxs-lookup"><span data-stu-id="f4e8b-240">6198803</span></span> |
| <span data-ttu-id="f4e8b-241">3</span><span class="sxs-lookup"><span data-stu-id="f4e8b-241">3</span></span> |<span data-ttu-id="f4e8b-242">1932223</span><span class="sxs-lookup"><span data-stu-id="f4e8b-242">1932223</span></span> |
| <span data-ttu-id="f4e8b-243">0</span><span class="sxs-lookup"><span data-stu-id="f4e8b-243">0</span></span> |<span data-ttu-id="f4e8b-244">82264625</span><span class="sxs-lookup"><span data-stu-id="f4e8b-244">82264625</span></span> |
| <span data-ttu-id="f4e8b-245">4</span><span class="sxs-lookup"><span data-stu-id="f4e8b-245">4</span></span> |<span data-ttu-id="f4e8b-246">85765</span><span class="sxs-lookup"><span data-stu-id="f4e8b-246">85765</span></span> |

### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="f4e8b-247">Feltárása: Számítási, és hasonlítsa össze a út távolság</span><span class="sxs-lookup"><span data-stu-id="f4e8b-247">Exploration: Compute and compare trip distance</span></span>
<span data-ttu-id="f4e8b-248">Ebben a példában a felvétel és Gyűjtőtár hosszúság alakítja át, és SQL a földrajzi szélesség mutat, út távolság SQL geográfiai pontok különbség használatával kiszámítja és visszaadja az eredmények összehasonlítása a véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-248">This example converts the pickup and drop-off longitude and latitude to SQL geography points, computes the trip distance using SQL geography points difference, and returns a random sample of the results for comparison.</span></span> <span data-ttu-id="f4e8b-249">A példa az eredményeket a minőségi assessment lekérdezést kezelt korábban csak a érvényes koordináták korlátozza.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-249">The example limits the results to valid coordinates only using the data quality assessment query covered earlier.</span></span>

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a><span data-ttu-id="f4e8b-250">A szolgáltatás műszaki osztály SQL függvények használata</span><span class="sxs-lookup"><span data-stu-id="f4e8b-250">Feature engineering using SQL functions</span></span>
<span data-ttu-id="f4e8b-251">SQL-függvény néha lehet a hatékony beállítás a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-251">Sometimes SQL functions can be an efficient option for feature engineering.</span></span> <span data-ttu-id="f4e8b-252">Ebben a bemutatóban meghatározott egy SQL-függvény a felvétel és dropoff helyek közötti távolság szerint közvetlen kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-252">In this walkthrough, we defined a SQL function to calculate the direct distance between the pickup and dropoff locations.</span></span> <span data-ttu-id="f4e8b-253">A következő SQL-parancsfájlok futtathat **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-253">You can run the following SQL scripts in **Visual Studio Data Tools**.</span></span>

<span data-ttu-id="f4e8b-254">Ez az SQL-parancsfájlt, amely meghatározza a távolságskála függvény.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-254">Here is the SQL script that defines the distance function.</span></span>

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

<span data-ttu-id="f4e8b-255">Íme egy példa a szolgáltatások létrehozni az SQL-lekérdezés függvény meghívásához:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-255">Here is an example to call this function to generate features in your SQL query:</span></span>

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="f4e8b-256">**Kimenet:** Ez a lekérdezés a felvétel és dropoff földrajzi szélesség (2,803,538 sorok) tábla állít elő, és hosszúságot és a megfelelő közvetlen miles megadott.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-256">**Output:** This query generates a table (with 2,803,538 rows) with pickup and dropoff latitudes and longitudes and the corresponding direct distances in miles.</span></span> <span data-ttu-id="f4e8b-257">Az alábbiakban az eredményeket az első 3 sort:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-257">Here are the results for first 3 rows:</span></span>

|  | <span data-ttu-id="f4e8b-258">pickup_latitude</span><span class="sxs-lookup"><span data-stu-id="f4e8b-258">pickup_latitude</span></span> | <span data-ttu-id="f4e8b-259">pickup_longitude</span><span class="sxs-lookup"><span data-stu-id="f4e8b-259">pickup_longitude</span></span> | <span data-ttu-id="f4e8b-260">dropoff_latitude</span><span class="sxs-lookup"><span data-stu-id="f4e8b-260">dropoff_latitude</span></span> | <span data-ttu-id="f4e8b-261">dropoff_longitude</span><span class="sxs-lookup"><span data-stu-id="f4e8b-261">dropoff_longitude</span></span> | <span data-ttu-id="f4e8b-262">DirectDistance</span><span class="sxs-lookup"><span data-stu-id="f4e8b-262">DirectDistance</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f4e8b-263">1</span><span class="sxs-lookup"><span data-stu-id="f4e8b-263">1</span></span> |<span data-ttu-id="f4e8b-264">40.731804</span><span class="sxs-lookup"><span data-stu-id="f4e8b-264">40.731804</span></span> |<span data-ttu-id="f4e8b-265">-74.001083</span><span class="sxs-lookup"><span data-stu-id="f4e8b-265">-74.001083</span></span> |<span data-ttu-id="f4e8b-266">40.736622</span><span class="sxs-lookup"><span data-stu-id="f4e8b-266">40.736622</span></span> |<span data-ttu-id="f4e8b-267">-73.988953</span><span class="sxs-lookup"><span data-stu-id="f4e8b-267">-73.988953</span></span> |<span data-ttu-id="f4e8b-268">.7169601222</span><span class="sxs-lookup"><span data-stu-id="f4e8b-268">.7169601222</span></span> |
| <span data-ttu-id="f4e8b-269">2</span><span class="sxs-lookup"><span data-stu-id="f4e8b-269">2</span></span> |<span data-ttu-id="f4e8b-270">40.715794</span><span class="sxs-lookup"><span data-stu-id="f4e8b-270">40.715794</span></span> |<span data-ttu-id="f4e8b-271">-74,010635</span><span class="sxs-lookup"><span data-stu-id="f4e8b-271">-74,010635</span></span> |<span data-ttu-id="f4e8b-272">40.725338</span><span class="sxs-lookup"><span data-stu-id="f4e8b-272">40.725338</span></span> |<span data-ttu-id="f4e8b-273">-74.00399</span><span class="sxs-lookup"><span data-stu-id="f4e8b-273">-74.00399</span></span> |<span data-ttu-id="f4e8b-274">.7448343721</span><span class="sxs-lookup"><span data-stu-id="f4e8b-274">.7448343721</span></span> |
| <span data-ttu-id="f4e8b-275">3</span><span class="sxs-lookup"><span data-stu-id="f4e8b-275">3</span></span> |<span data-ttu-id="f4e8b-276">40.761456</span><span class="sxs-lookup"><span data-stu-id="f4e8b-276">40.761456</span></span> |<span data-ttu-id="f4e8b-277">-73.999886</span><span class="sxs-lookup"><span data-stu-id="f4e8b-277">-73.999886</span></span> |<span data-ttu-id="f4e8b-278">40.766544</span><span class="sxs-lookup"><span data-stu-id="f4e8b-278">40.766544</span></span> |<span data-ttu-id="f4e8b-279">-73.988228</span><span class="sxs-lookup"><span data-stu-id="f4e8b-279">-73.988228</span></span> |<span data-ttu-id="f4e8b-280">0.7037227967</span><span class="sxs-lookup"><span data-stu-id="f4e8b-280">0.7037227967</span></span> |

### <a name="prepare-data-for-model-building"></a><span data-ttu-id="f4e8b-281">Adatok előkészítése a modell létrehozásának</span><span class="sxs-lookup"><span data-stu-id="f4e8b-281">Prepare data for model building</span></span>
<span data-ttu-id="f4e8b-282">Az alábbi lekérdezés illesztések a **nyctaxi\_út** és **nyctaxi\_jegy ára** táblák, állít elő, a bináris besorolási címkék **Formabontó**, egy több osztály besorolási címke **tipp\_osztály**, és egy minta kibontja a teljes illesztett adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-282">The following query joins the **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a sample from the full joined dataset.</span></span> <span data-ttu-id="f4e8b-283">A mintavétel lekérésével egy részét a felvételi ideje alapján való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-283">The sampling is done by retrieving a subset of the trips based on pickup time.</span></span>  <span data-ttu-id="f4e8b-284">Ez a lekérdezés másolja, majd a beillesztett közvetlenül a [Azure Machine Learning Studio](https://studio.azureml.net) [és adatokat importálhat] [ import-data] modul a közvetlen adatfeldolgozást az SQL adatbázis példányából Azure.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-284">This query can be copied then pasted directly in the [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from the SQL database instance in Azure.</span></span> <span data-ttu-id="f4e8b-285">A lekérdezés nem tartalmazza a rekordok helytelen (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-285">The query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="f4e8b-286">Ha készen áll az Azure Machine Learning folytatja, akkor előfordulhat, hogy vagy:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-286">When you are ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="f4e8b-287">A végső SQL lekérdezés kibontása és az az adatokat és a másolás-beillesztés közvetlenül a lekérdezés mentéséhez egy [és adatokat importálhat] [ import-data] modul az Azure Machine Learning, vagy</span><span class="sxs-lookup"><span data-stu-id="f4e8b-287">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="f4e8b-288">A modell egy új SQL DW-tábla létrehozásához használni kívánt mintavételi és visszafejtett adatok megmaradnak, és az új táblázat a [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-288">Persist the sampled and engineered data you plan to use for model building in a new SQL DW table and use the new table in the [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="f4e8b-289">A korábbi lépésben a PowerShell parancsfájl végzett ezt meg.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-289">The PowerShell script in earlier step has done this for you.</span></span> <span data-ttu-id="f4e8b-290">Elolvashatja, közvetlenül az adatok importálása a modulban a táblából.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-290">You can read directly from this table in the Import Data module.</span></span>

## <span data-ttu-id="f4e8b-291"><a name="ipnb"></a>Az adatok feltárása, a szolgáltatás fejlesztés IPython jegyzetfüzet</span><span class="sxs-lookup"><span data-stu-id="f4e8b-291"><a name="ipnb"></a>Data exploration and feature engineering in IPython notebook</span></span>
<span data-ttu-id="f4e8b-292">Ebben a szakaszban végezzük el az adatok feltárása és a szolgáltatás generálása mindkét pythonos környezetekben, és az SQL-Adatraktár SQL lekérdezések korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-292">In this section, we will perform data exploration and feature generation using both Python and SQL queries against the SQL DW created earlier.</span></span> <span data-ttu-id="f4e8b-293">Egy minta IPython notebook nevű **SQLDW_Explorations.ipynb** és a Python-parancsfájl **SQLDW_Explorations_Scripts.py** vannak töltve a helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-293">A sample IPython notebook named **SQLDW_Explorations.ipynb** and a Python script file **SQLDW_Explorations_Scripts.py** have been downloaded to your local directory.</span></span> <span data-ttu-id="f4e8b-294">Akkor is elérhetők a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-294">They are also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span></span> <span data-ttu-id="f4e8b-295">Ezeket a fájlokat két Python parancsfájlok azonosak.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-295">These two files are identical in Python scripts.</span></span> <span data-ttu-id="f4e8b-296">A Python-parancsfájl megadott meg abban az esetben, ha nem rendelkezik egy IPython Notebook kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-296">The Python script file is provided to you in case you do not have an IPython Notebook server.</span></span> <span data-ttu-id="f4e8b-297">Ez a két minta úgy vannak beállítva, a Python **Python 2.7**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-297">These two sample Python files are designed under **Python 2.7**.</span></span>

<span data-ttu-id="f4e8b-298">A szükséges Azure SQL DW információk minta IPython jegyzetfüzet és a Python-parancsfájl letöltése a helyi számítógépre van lett csatlakoztatva a PowerShell-parancsfájl által korábban.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-298">The needed Azure SQL DW information in the sample IPython Notebook and the Python script file downloaded to your local machine has been plugged in by the PowerShell script previously.</span></span> <span data-ttu-id="f4e8b-299">Azok a módosítás nélkül végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-299">They are executable without any modification.</span></span>

<span data-ttu-id="f4e8b-300">Ha már beállított egy AzureML-munkaterületet, közvetlenül a minta IPython Notebook feltölteni az AzureML IPython Notebook szolgáltatás és elindultak, azt.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-300">If you have already set up an AzureML workspace, you can directly upload the sample IPython Notebook to the AzureML IPython Notebook service and start running it.</span></span> <span data-ttu-id="f4e8b-301">AzureML IPython Notebook szolgáltatás feltöltésére lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-301">Here are the steps to upload to AzureML IPython Notebook service:</span></span>

1. <span data-ttu-id="f4e8b-302">Jelentkezzen be az AzureML-munkaterületen, a lap tetején kattintson az "Studio" és "NOTEBOOKOK" lap bal oldalán kattintson.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-302">Log in to your AzureML workspace, click "Studio" at the top, and click "NOTEBOOKS" on the left side of the web page.</span></span>
   
    ![#22 ábrázolása][22]
2. <span data-ttu-id="f4e8b-304">A weblap bal alsó sarokban kattintson az "Új", és jelölje ki "Python 2".</span><span class="sxs-lookup"><span data-stu-id="f4e8b-304">Click "NEW" on the left bottom corner of the web page, and select "Python 2".</span></span> <span data-ttu-id="f4e8b-305">Ezt követően adjon meg egy nevet a notebook, és kattintson a pipa jelre az új üres IPython Notebook létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-305">Then, provide a name to the notebook and click the check mark to create the new blank IPython Notebook.</span></span>
   
    ![#23 ábrázolása][23]
3. <span data-ttu-id="f4e8b-307">Kattintson a "Jupyter" szimbólumra az új IPython Notebook bal felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-307">Click the "Jupyter" symbol on the left top corner of the new IPython Notebook.</span></span>
   
    ![#24 ábrázolása][24]
4. <span data-ttu-id="f4e8b-309">A minta IPython Notebook áthúzása számára a **fa** az AzureML IPython Notebook szolgáltatást, majd kattintson a lap **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-309">Drag and drop the sample IPython Notebook to the **tree** page of your AzureML IPython Notebook service, and click **Upload**.</span></span> <span data-ttu-id="f4e8b-310">Ezt követően a minta IPython Notebook lesz feltöltve az AzureML IPython Notebook szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-310">Then, the sample IPython Notebook will be uploaded to the AzureML IPython Notebook service.</span></span>
   
    ![#25 ábrázolása][25]

<span data-ttu-id="f4e8b-312">A minta futtatásához IPython Notebook vagy a Python parancsfájlt, a következő csomagok szükségesek Python.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-312">In order to run the sample IPython Notebook or the Python script file, the following Python packages are needed.</span></span> <span data-ttu-id="f4e8b-313">Az AzureML IPython Notebook szolgáltatást használja, ha ezeket a csomagokat előre telepített törölték.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-313">If you are using the AzureML IPython Notebook service, these packages have been pre-installed.</span></span>

    - <span data-ttu-id="f4e8b-314">pandas</span><span class="sxs-lookup"><span data-stu-id="f4e8b-314">pandas</span></span>
    - <span data-ttu-id="f4e8b-315">numpy</span><span class="sxs-lookup"><span data-stu-id="f4e8b-315">numpy</span></span>
    - <span data-ttu-id="f4e8b-316">matplotlib</span><span class="sxs-lookup"><span data-stu-id="f4e8b-316">matplotlib</span></span>
    - <span data-ttu-id="f4e8b-317">pyodbc</span><span class="sxs-lookup"><span data-stu-id="f4e8b-317">pyodbc</span></span>
    - <span data-ttu-id="f4e8b-318">PyTables</span><span class="sxs-lookup"><span data-stu-id="f4e8b-318">PyTables</span></span>

<span data-ttu-id="f4e8b-319">Ha speciális elemzési megoldások kialakításának AzureML nagy adatokkal ajánlott sorrendje a következő:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-319">The recommended sequence when building advanced analytical solutions on AzureML with large data is the following:</span></span>

* <span data-ttu-id="f4e8b-320">Olvassa el az adatok kis mintában egy a memóriában levő keretbe.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-320">Read in a small sample of the data into an in-memory data frame.</span></span>
* <span data-ttu-id="f4e8b-321">Néhány képi megjelenítések és explorations a mintaadatokat használatával hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-321">Perform some visualizations and explorations using the sampled data.</span></span>
* <span data-ttu-id="f4e8b-322">Szolgáltatás műszaki osztály használata a mintaadatokat kísérletezhet.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-322">Experiment with feature engineering using the sampled data.</span></span>
* <span data-ttu-id="f4e8b-323">Nagyobb az adatok feltárása, adatkezelési és a szolgáltatás mérnöki csapathoz, a Python segítségével közvetlenül az SQL-Adatraktár SQL lekérdezések ki.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-323">For larger data exploration, data manipulation and feature engineering, use Python to issue SQL Queries directly against the SQL DW.</span></span>
* <span data-ttu-id="f4e8b-324">Döntse el, az Azure Machine Learning modell létrehozásának alkalmasnak mintájának méretét.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-324">Decide the sample size to be suitable for Azure Machine Learning model building.</span></span>

<span data-ttu-id="f4e8b-325">A következőket: néhány adatok feltárása, adatábrázolási és példák mérnöki szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-325">The followings are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="f4e8b-326">További adatok explorations megtalálhatók a minta IPython jegyzetfüzet és Python fájlra.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-326">More data explorations can be found in the sample IPython Notebook and the sample Python script file.</span></span>

### <a name="initialize-database-credentials"></a><span data-ttu-id="f4e8b-327">Adatbázis-hitelesítő adatok inicializálása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-327">Initialize database credentials</span></span>
<span data-ttu-id="f4e8b-328">Az adatbázis-kapcsolati beállításokat a következő változók inicializálása:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-328">Initialize your database connection settings in the following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a><span data-ttu-id="f4e8b-329">Adatbázis-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-329">Create database connection</span></span>
<span data-ttu-id="f4e8b-330">Ez a kapcsolati karakterláncot, amely hoz létre a kapcsolat az adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-330">Here is the connection string that creates the connection to the database.</span></span>

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="f4e8b-331">Sorok és oszlopok a tábla < nyctaxi_trip > jelentés száma</span><span class="sxs-lookup"><span data-stu-id="f4e8b-331">Report number of rows and columns in table <nyctaxi_trip></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="f4e8b-332">A sorok teljes számát = 173179759</span><span class="sxs-lookup"><span data-stu-id="f4e8b-332">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="f4e8b-333">Az oszlopok teljes száma = 14</span><span class="sxs-lookup"><span data-stu-id="f4e8b-333">Total number of columns = 14</span></span>

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a><span data-ttu-id="f4e8b-334">Sorok és oszlopok a tábla < nyctaxi_fare > jelentés száma</span><span class="sxs-lookup"><span data-stu-id="f4e8b-334">Report number of rows and columns in table <nyctaxi_fare></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="f4e8b-335">A sorok teljes számát = 173179759</span><span class="sxs-lookup"><span data-stu-id="f4e8b-335">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="f4e8b-336">Az oszlopok teljes száma = 11</span><span class="sxs-lookup"><span data-stu-id="f4e8b-336">Total number of columns = 11</span></span>

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a><span data-ttu-id="f4e8b-337">Olvassa el a kisméretű minta az SQL Data Warehouse adatbázisból</span><span class="sxs-lookup"><span data-stu-id="f4e8b-337">Read-in a small data sample from the SQL Data Warehouse Database</span></span>
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="f4e8b-338">Idő olvasási a mintatáblázat 14.096495 másodperc.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-338">Time to read the sample table is 14.096495 seconds.</span></span>  
<span data-ttu-id="f4e8b-339">A beolvasott sorok és oszlopok száma = (1000, 21).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-339">Number of rows and columns retrieved = (1000, 21).</span></span>

### <a name="descriptive-statistics"></a><span data-ttu-id="f4e8b-340">Leíró statisztika</span><span class="sxs-lookup"><span data-stu-id="f4e8b-340">Descriptive statistics</span></span>
<span data-ttu-id="f4e8b-341">Most már készen áll a mintaadatokat felfedezése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-341">Now you are ready to explore the sampled data.</span></span> <span data-ttu-id="f4e8b-342">Először néhány leíró statisztikája megnézi a **út\_távolság** (vagy bármely más mezők úgy dönt, hogy adja meg).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-342">We start with looking at some descriptive statistics for the **trip\_distance** (or any other fields you choose to specify).</span></span>

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a><span data-ttu-id="f4e8b-343">A képi megjelenítés: Rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="f4e8b-343">Visualization: Box plot example</span></span>
<span data-ttu-id="f4e8b-344">Ezután úgy tekintünk, a Dobozdiagram a megjelenítéséhez a quantiles út távolság.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-344">Next we look at the box plot for the trip distance to visualize the quantiles.</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Tőzsdei #1][1]

### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="f4e8b-346">A képi megjelenítés: Terjesztési rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="f4e8b-346">Visualization: Distribution plot example</span></span>
<span data-ttu-id="f4e8b-347">Amely a telepítési és a mintában szereplő út távolság a hisztogram megjelenítése előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-347">Plots that visualize the distribution and a histogram for the sampled trip distances.</span></span>

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tőzsdei #2][2]

### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="f4e8b-349">A képi megjelenítés: Sáv megnyitásához, majd sor ábra</span><span class="sxs-lookup"><span data-stu-id="f4e8b-349">Visualization: Bar and line plots</span></span>
<span data-ttu-id="f4e8b-350">Ebben a példában azt az öt bins út távolság bin és a binning eredményeinek képi megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-350">In this example, we bin the trip distance into five bins and visualize the binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="f4e8b-351">A Microsoft megrajzolásához a fenti bin terjesztési egy sávon, vagy a rajzolási. sor:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-351">We can plot the above bin distribution in a bar or line plot with:</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

<span data-ttu-id="f4e8b-353">és</span><span class="sxs-lookup"><span data-stu-id="f4e8b-353">and</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

### <a name="visualization-scatterplot-examples"></a><span data-ttu-id="f4e8b-355">A képi megjelenítés: Scatterplot példák</span><span class="sxs-lookup"><span data-stu-id="f4e8b-355">Visualization: Scatterplot examples</span></span>
<span data-ttu-id="f4e8b-356">Pontdiagram rajzot közötti megmutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** van-e bármilyen korrelációs</span><span class="sxs-lookup"><span data-stu-id="f4e8b-356">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** to see if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

<span data-ttu-id="f4e8b-358">Hasonlóképpen azt is ellenőrizze közötti kapcsolat **arány\_kód** és **út\_távolság**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-358">Similarly we can check the relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="f4e8b-360">Az adatok feltárása a mintaadatokat az SQL-lekérdezések IPython notebook használatával</span><span class="sxs-lookup"><span data-stu-id="f4e8b-360">Data exploration on sampled data using SQL queries in IPython notebook</span></span>
<span data-ttu-id="f4e8b-361">Ebben a részben azt megismerkedhet a mintában szereplő adatait, amely a fenti létrehozott új tábla megőrződjenek használatával adatokat terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-361">In this section, we explore data distributions using the sampled data which is persisted in the new table we created above.</span></span> <span data-ttu-id="f4e8b-362">Vegye figyelembe, hogy hasonló explorations használatával végezheti el az eredeti táblázatban.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-362">Note that similar explorations can be performed using the original tables.</span></span>

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a><span data-ttu-id="f4e8b-363">Feltárása: A jelentés sorok és oszlopok száma a mintában szereplő táblázatban</span><span class="sxs-lookup"><span data-stu-id="f4e8b-363">Exploration: Report number of rows and columns in the sampled table</span></span>
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a><span data-ttu-id="f4e8b-364">Feltárása: A Formabontó/nem terjesztési azokat kioldják</span><span class="sxs-lookup"><span data-stu-id="f4e8b-364">Exploration: Tipped/not tripped Distribution</span></span>
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a><span data-ttu-id="f4e8b-365">Feltárása: Tipp osztály terjesztési</span><span class="sxs-lookup"><span data-stu-id="f4e8b-365">Exploration: Tip class distribution</span></span>
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a><span data-ttu-id="f4e8b-366">Feltárása: A tipp terjesztési megrajzolásához osztály</span><span class="sxs-lookup"><span data-stu-id="f4e8b-366">Exploration: Plot the tip distribution by class</span></span>
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 ábrázolása][26]

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="f4e8b-368">Feltárása: Való adatváltások számát napi eloszlásáról</span><span class="sxs-lookup"><span data-stu-id="f4e8b-368">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="f4e8b-369">Feltárása: Utazás per medallion terjesztési</span><span class="sxs-lookup"><span data-stu-id="f4e8b-369">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a><span data-ttu-id="f4e8b-370">Feltárása: Út terjesztési medallion és rejthetők el licenc által</span><span class="sxs-lookup"><span data-stu-id="f4e8b-370">Exploration: Trip distribution by medallion and hack license</span></span>
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a><span data-ttu-id="f4e8b-371">Feltárása: Út idő terjesztése</span><span class="sxs-lookup"><span data-stu-id="f4e8b-371">Exploration: Trip time distribution</span></span>
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a><span data-ttu-id="f4e8b-372">Feltárása: Út távolság terjesztési</span><span class="sxs-lookup"><span data-stu-id="f4e8b-372">Exploration: Trip distance distribution</span></span>
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a><span data-ttu-id="f4e8b-373">Feltárása: Fizetési típus terjesztési</span><span class="sxs-lookup"><span data-stu-id="f4e8b-373">Exploration: Payment type distribution</span></span>
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a><span data-ttu-id="f4e8b-374">Ellenőrizze a featurized tábla végleges formájában</span><span class="sxs-lookup"><span data-stu-id="f4e8b-374">Verify the final form of the featurized table</span></span>
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <span data-ttu-id="f4e8b-375"><a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4e8b-375"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="f4e8b-376">Azt készen áll a modell létrehozásának és a modell telepítési [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-376">We are now ready to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="f4e8b-377">Az adatok nevezetesen korábban azonosított előrejelzés problémák valamelyikében használatra kész:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-377">The data is ready to be used in any of the prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="f4e8b-378">**Bináris osztályozási**: előre jelezni-e tipp kifizetett egy út.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-378">**Binary classification**: To predict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="f4e8b-379">**Multiclass besorolási**: megjósolható a fizetős, a korábban meghatározott osztályba tipp tartományán.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-379">**Multiclass classification**: To predict the range of tip paid, according to the previously defined classes.</span></span>
3. <span data-ttu-id="f4e8b-380">**Regressziós feladat**: megjósolható a fizetős útnak tipp mennyisége.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-380">**Regression task**: To predict the amount of tip paid for a trip.</span></span>  

<span data-ttu-id="f4e8b-381">A modellezési gyakorlására megkezdéséhez jelentkezzen be a **Azure Machine Learning** munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-381">To begin the modeling exercise, log in to your **Azure Machine Learning** workspace.</span></span> <span data-ttu-id="f4e8b-382">Ha még nem hozott létre machine learning-munkaterület, lásd: [hozzon létre egy Azure ML munkaterületet](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-382">If you have not yet created a machine learning workspace, see [Create an Azure ML workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="f4e8b-383">Ismerkedés az Azure Machine Learning, lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="f4e8b-383">To get started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="f4e8b-384">Jelentkezzen be [Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-384">Log in to [Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="f4e8b-385">A Studio kezdőlap modulok hivatkozást, és más erőforrások számos olyan információt, videók, oktatóanyagok, valamint hivatkozásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-385">The Studio Home page provides a wealth of information, videos, tutorials, links to the Modules Reference, and other resources.</span></span> <span data-ttu-id="f4e8b-386">Azure Machine Learning kapcsolatos további információkért tekintse meg a [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-386">For more information about Azure Machine Learning, consult the [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="f4e8b-387">Egy tipikus tanítási kísérletet a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-387">A typical training experiment consists of the following steps:</span></span>

1. <span data-ttu-id="f4e8b-388">Hozzon létre egy **+ új** kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-388">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="f4e8b-389">Az adatok beolvasása az Azure ml.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-389">Get the data into Azure ML.</span></span>
3. <span data-ttu-id="f4e8b-390">Előre feldolgozzák, átalakítására, és szükség esetén az adatok kezelését.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-390">Pre-process, transform and manipulate the data as needed.</span></span>
4. <span data-ttu-id="f4e8b-391">Szolgáltatások létrehozása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-391">Generate features as needed.</span></span>
5. <span data-ttu-id="f4e8b-392">Az adatok felosztása adatkészletek képzési/érvényesítési/tesztelése (külön adatkészleteket, vagy az egyes).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-392">Split the data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="f4e8b-393">Válasszon egy vagy több gépi tanulási algoritmusok attól függően, hogy a tanulási probléma megoldására.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-393">Select one or more machine learning algorithms depending on the learning problem to solve.</span></span> <span data-ttu-id="f4e8b-394">Például bináris osztályozási multiclass besorolás, regressziós.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-394">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="f4e8b-395">A képzési adatkészlet egy vagy több modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-395">Train one or more models using the training dataset.</span></span>
8. <span data-ttu-id="f4e8b-396">Az érvényesítési adatkészletet a betanított modellek segítségével pontozása.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-396">Score the validation dataset using the trained model(s).</span></span>
9. <span data-ttu-id="f4e8b-397">Értékelje ki a megfelelő metrikákat a tanulási probléma kiszámításához modellek.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-397">Evaluate the model(s) to compute the relevant metrics for the learning problem.</span></span>
10. <span data-ttu-id="f4e8b-398">Konfigurálva finomhangolhatják a modellek, és válassza ki a legjobb központilag telepítendő modell.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-398">Fine tune the model(s) and select the best model to deploy.</span></span>

<span data-ttu-id="f4e8b-399">Ebben a gyakorlatban azt már megismerkedett és fejthetők vissza az adatokat az SQL Data Warehouse, és a mintájának méretét az Azure ml betöltési lezárását.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-399">In this exercise, we have already explored and engineered the data in SQL Data Warehouse, and decided on the sample size to ingest in Azure ML.</span></span> <span data-ttu-id="f4e8b-400">A folyamat során legalább egy, az előrejelzési modellek létrehozásához a rendszer:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-400">Here is the procedure to build one or more of the prediction models:</span></span>

1. <span data-ttu-id="f4e8b-401">Az Azure gépi tanulás használatával az adatok beolvasása a [és adatokat importálhat] [ import-data] modul érhető el a **bemeneti és kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-401">Get the data into Azure ML using the [Import Data][import-data] module, available in the **Data Input and Output** section.</span></span> <span data-ttu-id="f4e8b-402">További információkért lásd: a [és adatokat importálhat] [ import-data] modul referencialapja.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-402">For more information, see the [Import Data][import-data] module reference page.</span></span>
   
    ![Azure ML importálási adatok][17]
2. <span data-ttu-id="f4e8b-404">Válassza ki **Azure SQL Database** , a **adatforrás** a a **tulajdonságok** panel.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-404">Select **Azure SQL Database** as the **Data source** in the **Properties** panel.</span></span>
3. <span data-ttu-id="f4e8b-405">Az adatbázis DNS-nevét adja meg a **adatbázis-kiszolgáló neve** mező.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-405">Enter the database DNS name in the **Database server name** field.</span></span> <span data-ttu-id="f4e8b-406">Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="f4e8b-406">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="f4e8b-407">Adja meg a **adatbázisnév** a megfelelő mezőben.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-407">Enter the **Database name** in the corresponding field.</span></span>
5. <span data-ttu-id="f4e8b-408">Adja meg a *SQL felhasználónév* a a **kiszolgáló felhasználói fiók nevét**, és a *jelszó* a a **kiszolgáló felhasználói fiók jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-408">Enter the *SQL user name* in the **Server user account name**, and the *password* in the **Server user account password**.</span></span>
6. <span data-ttu-id="f4e8b-409">Ellenőrizze a **fogadja el a kiszolgálói tanúsítvány** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-409">Check the **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="f4e8b-410">Az a **adatbázis-lekérdezés** szöveg terület szerkesztése, illessze be a lekérdezést, amely a szükséges adatbázis-mezők (beleértve a számított mezőket a címkéket például) és régebbi minták a kívánt mintájának méretét az adatokat.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-410">In the **Database query** edit text area, paste the query which extracts the necessary database fields (including any computed fields such as the labels) and down samples the data to the desired sample size.</span></span>

<span data-ttu-id="f4e8b-411">Az alábbi ábra a lehet például egy bináris osztályozási kísérletet, adatok beolvasása közvetlenül az SQL Data Warehouse-adatbázis (ne felejtse el lecserélni a tábla neve nyctaxi_trip és nyctaxi_fare a séma és a tábla nevének használta, a forgatókönyv).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-411">An example of a binary classification experiment reading data directly from the SQL Data Warehouse database is in the figure below (remember to replace the table names nyctaxi_trip and nyctaxi_fare by the schema name and the table names you used in your walkthrough).</span></span> <span data-ttu-id="f4e8b-412">Hasonló kísérletek multiclass besorolás és a visszavonási lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-412">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Az Azure ML vonat][10]

> [!IMPORTANT]
> <span data-ttu-id="f4e8b-414">A modellezési adatok kinyerése és a mintavételi lekérdezés példák korábbi szakaszokban találhatók **összes címkéket a három modellezési gyakorlatok szerepelnek a lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-414">In the modeling data extraction and sampling query examples provided in previous sections, **all labels for the three modeling exercises are included in the query**.</span></span> <span data-ttu-id="f4e8b-415">Minden a modellezési gyakorlatok (kötelező) fontos lépés, hogy **kizárása** a szükségtelen címkék a más két problémákat, és bármely más **céloz kiszivárgásának**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-415">An important (required) step in each of the modeling exercises is to **exclude** the unnecessary labels for the other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="f4e8b-416">Például bináris osztályozási használatakor használjon a címke **Formabontó** , és amelyeket a mezők **tipp\_osztály**, **tipp\_összeg**, és **teljes\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-416">For example, when using binary classification, use the label **tipped** and exclude the fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="f4e8b-417">Az utóbbi cél kiszivárgásának óta azt tartalmazzák a tipp fizetett.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-417">The latter are target leaks since they imply the tip paid.</span></span>
> 
> <span data-ttu-id="f4e8b-418">Felesleges oszlopok kizárásához, vagy a cél kiszivárgásának, használhatja a [Select Columns in Dataset] [ select-columns] modul vagy a [szerkesztése metaadatok][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="f4e8b-418">To exclude any unnecessary columns or target leaks, you may use the [Select Columns in Dataset][select-columns] module or the [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="f4e8b-419">További információkért lásd: [Select Columns in Dataset] [ select-columns] és [szerkesztése metaadatok] [ edit-metadata] lapok hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-419">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="f4e8b-420"><a name="mldeploy"></a>Az Azure Machine Learning modellek telepítése</span><span class="sxs-lookup"><span data-stu-id="f4e8b-420"><a name="mldeploy"></a>Deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="f4e8b-421">Amikor készen áll a modell, könnyedén telepítheti azt egy webszolgáltatás-ről a kísérlet.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-421">When your model is ready, you can easily deploy it as a web service directly from the experiment.</span></span> <span data-ttu-id="f4e8b-422">Azure ML web services telepítésével kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="f4e8b-422">For more information about deploying Azure ML web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="f4e8b-423">Egy új webszolgáltatás-bővítmény telepítéséhez kell:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-423">To deploy a new web service, you need to:</span></span>

1. <span data-ttu-id="f4e8b-424">A pontozási kísérlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-424">Create a scoring experiment.</span></span>
2. <span data-ttu-id="f4e8b-425">A webszolgáltatás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-425">Deploy the web service.</span></span>

<span data-ttu-id="f4e8b-426">Egy pontozási kísérletet a létrehozásához egy **befejezett** betanítása kísérletet, kattintson a **létrehozása pontozási KÍSÉRLETEZHET** alacsonyabb műveletsávon.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-426">To create a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in the lower action bar.</span></span>

![Pontozás az Azure][18]

<span data-ttu-id="f4e8b-428">Az Azure Machine Learning megpróbál létrehozni egy pontozási kísérletet, a tanítási kísérletet összetevői alapján.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-428">Azure Machine Learning will attempt to create a scoring experiment based on the components of the training experiment.</span></span> <span data-ttu-id="f4e8b-429">Különösen a következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f4e8b-429">In particular, it will:</span></span>

1. <span data-ttu-id="f4e8b-430">A betanított modell elmentésével, és távolítsa el a modell képzési modulok.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-430">Save the trained model and remove the model training modules.</span></span>
2. <span data-ttu-id="f4e8b-431">Azonosítsa a logikai **bemeneti port** a szükséges bemeneti adatok séma képviseli.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-431">Identify a logical **input port** to represent the expected input data schema.</span></span>
3. <span data-ttu-id="f4e8b-432">Azonosítsa a logikai **kimeneti port** képviselő a várt webes szolgáltatás kimeneti sémával.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-432">Identify a logical **output port** to represent the expected web service output schema.</span></span>

<span data-ttu-id="f4e8b-433">A pontozási kísérlet jön létre, amikor, tekintse át, és ellenőrizze, hogy szükség szerint állítsa be.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-433">When the scoring experiment is created, review it and make adjust as needed.</span></span> <span data-ttu-id="f4e8b-434">Egy tipikus módosítása, hogy cserélje le a bemeneti adatkészlet és/vagy lekérdezési egyet, amely kizárja a címke mezők, mivel ezek nem lesz elérhető a szolgáltatás meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-434">A typical adjustment is to replace the input dataset and/or query with one which excludes label fields, as these will not be available when the service is called.</span></span> <span data-ttu-id="f4e8b-435">Azt is néhány rögzíti, hogy a bemeneti adatkészlet és/vagy lekérdezési méretének csökkentése érdekében célszerű éppen elegendő mértékű jelzi a bemeneti sémát.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-435">It is also a good practice to reduce the size of the input dataset and/or query to a few records, just enough to indicate the input schema.</span></span> <span data-ttu-id="f4e8b-436">A kimeneti port esetében gyakori, hogy kizárja az összes beviteli mezőt, és csak a **pontozott címkék** és **program pontozza a mennyiségeket valószínűség** a kimeneti használatával a [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-436">For the output port, it is common to exclude all input fields and only include the **Scored Labels** and **Scored Probabilities** in the output using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="f4e8b-437">Az alábbi ábra a kísérletben pontozási minta valósul meg.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-437">A sample scoring experiment is provided in the figure below.</span></span> <span data-ttu-id="f4e8b-438">Amikor készen áll a központi telepítése, kattintson a **WEBSZOLGÁLTATÁS közzététele** alacsonyabb műveletsávon gombra.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-438">When ready to deploy, click the **PUBLISH WEB SERVICE** button in the lower action bar.</span></span>

![Az Azure ML közzététele][11]

## <a name="summary"></a><span data-ttu-id="f4e8b-440">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="f4e8b-440">Summary</span></span>
<span data-ttu-id="f4e8b-441">Recap mi azt megtette az oktatóanyag forgatókönyv, hogy hozott létre az Azure data tudományos környezethez, a nagy nyilvános dataset dolgozott véve a folyamatot, a csapat adatok tudományos, egészen az adatgyűjtést modell betanítási, majd a az Azure Machine Learning webszolgáltatás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-441">To recap what we have done in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset, taking it through the Team Data Science Process, all the way from data acquisition to model training, and then to the deployment of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="f4e8b-442">Licencinformációk</span><span class="sxs-lookup"><span data-stu-id="f4e8b-442">License information</span></span>
<span data-ttu-id="f4e8b-443">Ez a minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft alatt a MIT licenccel.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-443">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="f4e8b-444">Ellenőrizze a LICENSE.txt fájlt további részletekért a Githubon mintakódot a címtárban.</span><span class="sxs-lookup"><span data-stu-id="f4e8b-444">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="f4e8b-445">Referencia</span><span class="sxs-lookup"><span data-stu-id="f4e8b-445">References</span></span>
<span data-ttu-id="f4e8b-446">• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="f4e8b-446">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="f4e8b-447">• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="f4e8b-447">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="f4e8b-448">• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="f4e8b-448">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
