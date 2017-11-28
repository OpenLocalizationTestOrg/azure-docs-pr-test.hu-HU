---
title: "hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse |} Microsoft Docs"
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
ms.openlocfilehash: b1b6371583a023d32e33db59464cafd8c3b767d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a><span data-ttu-id="c7b0f-103">hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c7b0f-103">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>
<span data-ttu-id="c7b0f-104">Az oktatóanyag azt ismerteti létrehozása és telepítése a gépi tanulási modellek az SQL Data Warehouse (SQL DW) keresztül egy nyilvánosan elérhető adatkészlet – hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-104">In this tutorial, we walk you through building and deploying a machine learning model using SQL Data Warehouse (SQL DW) for a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="c7b0f-105">hello bináris osztályozási modell összeállított képes-e tipp fizetnek útnak, és multiclass besorolás és regressziós modell, amely hello terjesztési megjósolható a fizetős hello tipp adatmennyiség is ismertetése.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-105">hello binary classification model constructed predicts whether or not a tip is paid for a trip, and models for multiclass classification and regression are also discussed that predict hello distribution for hello tip amounts paid.</span></span>

<span data-ttu-id="c7b0f-106">hello eljárást követi hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-106">hello procedure follows hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) workflow.</span></span> <span data-ttu-id="c7b0f-107">Megmutatjuk, hogyan toosetup egy tudományos környezetben, hogyan tooload adatok hello SQL DW-be, és hogyan használja az SQL DW vagy egy IPython Notebook tooexplore hello adatok és a visszafejtés szolgáltatások toomodel.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-107">We show how toosetup a data science environment, how tooload hello data into SQL DW, and how use either SQL DW or an IPython Notebook tooexplore hello data and engineer features toomodel.</span></span> <span data-ttu-id="c7b0f-108">Ezután megmutatjuk, hogyan toobuild és a modell Azure Machine Learning segítségével telepíthet.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-108">We then show how toobuild and deploy a model with Azure Machine Learning.</span></span>

## <span data-ttu-id="c7b0f-109"><a name="dataset"></a>hello NYC Taxi Utazgatással adatkészlet</span><span class="sxs-lookup"><span data-stu-id="c7b0f-109"><a name="dataset"></a>hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="c7b0f-110">hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (~ 48GB tömörítetlen) áll, több mint 173 millió rögzítése egyéni utak és hello turistajegyek esetében minden út kifizette.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-110">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="c7b0f-111">Minden út rekord hello felvétel és Gyűjtőtár helyeket tartalmaz, és időpontokat, anonimizált adatokon alapul (illesztőprogram) licencszám ellophatja, és hello medallion (taxi tartozó egyedi azonosító) száma.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-111">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="c7b0f-112">hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="c7b0f-113">Hello **trip_data.csv** fájl tartalmazza út részleteit, például az utasok, a felvételi és dropoff pontok, út időtartama és út hossza száma.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-113">hello **trip_data.csv** file contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="c7b0f-114">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="c7b0f-115">Hello **trip_fare.csv** fájl hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-115">hello **trip_fare.csv** file contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="c7b0f-116">Íme néhány példa rekordok:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="c7b0f-117">Hello **egyedi kulcs** toojoin út használt\_adatok és út\_jegy ára hello a következő három mezőből áll:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-117">hello **unique key** used toojoin trip\_data and trip\_fare is composed of hello following three fields:</span></span>

* <span data-ttu-id="c7b0f-118">medallion,</span><span class="sxs-lookup"><span data-stu-id="c7b0f-118">medallion,</span></span>
* <span data-ttu-id="c7b0f-119">ellophatja\_licenc és</span><span class="sxs-lookup"><span data-stu-id="c7b0f-119">hack\_license and</span></span>
* <span data-ttu-id="c7b0f-120">a felvételi\_datetime.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-120">pickup\_datetime.</span></span>

## <span data-ttu-id="c7b0f-121"><a name="mltasks"></a>Három típusú előrejelzés feladatok cím</span><span class="sxs-lookup"><span data-stu-id="c7b0f-121"><a name="mltasks"></a>Address three types of prediction tasks</span></span>
<span data-ttu-id="c7b0f-122">A Microsoft hello alapján három előrejelzés problémák állítson össze *tipp\_összeg* tooillustrate három típusú feladatok modellezési:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-122">We formulate three prediction problems based on hello *tip\_amount* tooillustrate three kinds of modeling tasks:</span></span>

1. <span data-ttu-id="c7b0f-123">**Bináris osztályozási**: toopredict-e tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_Összeg* $ 0 egy negatív példában látható.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-123">**Binary classification**: toopredict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="c7b0f-124">**Multiclass besorolási**: hello út fizetett tipp toopredict hello tartományán.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-124">**Multiclass classification**: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="c7b0f-125">A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="c7b0f-126">**Regressziós feladat**: toopredict hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-126">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="c7b0f-127"><a name="setup"></a>Hello az Azure data tudományos környezet speciális elemzésekre beállítása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-127"><a name="setup"></a>Set up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="c7b0f-128">az Azure Adattudomány környezet tooset kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-128">tooset up your Azure Data Science environment, follow these steps.</span></span>

<span data-ttu-id="c7b0f-129">**A saját Azure blob storage-fiók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-129">**Create your own Azure blob storage account**</span></span>

* <span data-ttu-id="c7b0f-130">Ha a saját Azure blob-tároló, válassza az Azure blob storage a vagy a lehető legközelebb a földrajzi helymeghatározás túl**déli középső Régiójában**, vagyis hello NYC Taxi adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-130">When you provision your own Azure blob storage, choose a geo-location for your Azure blob storage in or as close as possible too**South Central US**, which is where hello NYC Taxi data is stored.</span></span> <span data-ttu-id="c7b0f-131">hello adatok hello nyilvános blob storage tárolóban tooa tárolót az AzCopy használatával saját tárfiókba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-131">hello data will be copied using AzCopy from hello public blob storage container tooa container in your own storage account.</span></span> <span data-ttu-id="c7b0f-132">hello közelebb az Azure blob storage tooSouth USA középső RÉGIÓJA, hello gyorsabb (4. lépés) Ez a feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-132">hello closer your Azure blob storage is tooSouth Central US, hello faster this task (Step 4) will be completed.</span></span>
* <span data-ttu-id="c7b0f-133">toocreate az Azure-tárolóban fiókot, a leírt lépéseket hajtsa végre hello [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-133">toocreate your own Azure storage account, follow hello steps outlined at [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="c7b0f-134">Lehet, hogy toomake tudnivalók a következő tárfiók hitelesítő adatainak hello értékeit, akkor ez az útmutató későbbi részében szükség lesz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-134">Be sure toomake notes on hello values for following storage account credentials as they will be needed later in this walkthrough.</span></span>
  
  * <span data-ttu-id="c7b0f-135">**A tárfiók neve**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-135">**Storage Account Name**</span></span>
  * <span data-ttu-id="c7b0f-136">**Tárfiók kulcsa**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-136">**Storage Account Key**</span></span>
  * <span data-ttu-id="c7b0f-137">**Tároló neve** (amelybe hello adatok toobe hello Azure blob Storage tárolóban tárolt)</span><span class="sxs-lookup"><span data-stu-id="c7b0f-137">**Container Name** (which you want hello data toobe stored in hello Azure blob storage)</span></span>

<span data-ttu-id="c7b0f-138">**Az Azure SQL DW-példányt kiépítéséhez.**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-138">**Provision your Azure SQL DW instance.**</span></span>
<span data-ttu-id="c7b0f-139">Hajtsa végre a hello dokumentációját a [SQL Data Warehouse létrehozása](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision egy SQL Data Warehouse-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-139">Follow hello documentation at [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision a SQL Data Warehouse instance.</span></span> <span data-ttu-id="c7b0f-140">Győződjön meg arról, hogy, hogy az SQL Data Warehouse hitelesítő adataikat, amelyek a későbbi lépésekben használható a következő hello jelölések.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-140">Make sure that you make notations on hello following SQL Data Warehouse credentials which will be used in later steps.</span></span>

* <span data-ttu-id="c7b0f-141">**Kiszolgálónév**: <server Name>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="c7b0f-141">**Server Name**: <server Name>.database.windows.net</span></span>
* <span data-ttu-id="c7b0f-142">**SQLDW (adatbázis) neve**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-142">**SQLDW (Database) Name**</span></span>
* <span data-ttu-id="c7b0f-143">**Felhasználónév**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-143">**Username**</span></span>
* <span data-ttu-id="c7b0f-144">**Jelszó**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-144">**Password**</span></span>

<span data-ttu-id="c7b0f-145">**Telepítse a Visual Studio és az SQL Server Data Tools összetevővel.**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-145">**Install Visual Studio and SQL Server Data Tools.**</span></span> <span data-ttu-id="c7b0f-146">Útmutatásért lásd: [Visual Studio 2015 telepítése és/vagy az SSDT (SQL Server Data Tools) az SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-146">For instructions, see [Install Visual Studio 2015 and/or SSDT (SQL Server Data Tools) for SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span></span>

<span data-ttu-id="c7b0f-147">**Kapcsolódás Azure SQL DW tooyour a Visual Studio.**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-147">**Connect tooyour Azure SQL DW with Visual Studio.**</span></span> <span data-ttu-id="c7b0f-148">Útmutatásért lásd: 1 és 2 lépéseket [csatlakozzon az SQL Data Warehouse tooAzure Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-148">For instructions, see steps 1 & 2 in [Connect tooAzure SQL Data Warehouse with Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-149">Futtatási hello következő SQL-lekérdezést a hello adatbázis az SQL Data Warehouse létrehozott (hello lekérdezés hello 3. lépésében megadott helyett csatlakozzon a témakörben) túl**hozzon létre egy főkulcsot**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-149">Run hello following SQL query on hello database you created in your SQL Data Warehouse (instead of hello query provided in step 3 of hello connect topic,) too**create a master key**.</span></span>
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

<span data-ttu-id="c7b0f-150">**Hozzon létre egy Azure Machine Learning munkaterülettel alatt az Azure-előfizetéshez.**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-150">**Create an Azure Machine Learning workspace under your Azure subscription.**</span></span> <span data-ttu-id="c7b0f-151">Útmutatásért lásd: [hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-151">For instructions, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

## <span data-ttu-id="c7b0f-152"><a name="getdata"></a>Hello adatok betöltése az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c7b0f-152"><a name="getdata"></a>Load hello data into SQL Data Warehouse</span></span>
<span data-ttu-id="c7b0f-153">Nyisson meg egy Windows PowerShell-parancs konzolt.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-153">Open a Windows PowerShell command console.</span></span> <span data-ttu-id="c7b0f-154">Futtassa a következő hello PowerShell parancsokkal toodownload hello példa SQL parancsfájlok, amelyek osztjuk meg Önnel GitHub tooa helyi könyvtárban hello paraméterrel megadott *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-154">Run hello following PowerShell commands toodownload hello example SQL script files that we share with you on GitHub tooa local directory that you specify with hello parameter *-DestDir*.</span></span> <span data-ttu-id="c7b0f-155">Módosíthatja a paraméter értékének hello *- DestDir* tooany helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-155">You can change hello value of parameter *-DestDir* tooany local directory.</span></span> <span data-ttu-id="c7b0f-156">Ha *- DestDir* nem létezik, a rendszer automatikusan létrehozza hello PowerShell-parancsfájl által.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-156">If *-DestDir* does not exist, it will be created by hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-157">Szükség lehet túl**Futtatás rendszergazdaként** hello, ha a következő PowerShell-parancsfájl végrehajtása közben a *DestDir* directory rendszergazdai jogosultság toocreate vagy toowrite tooit kell.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-157">You might need too**Run as Administrator** when executing hello following PowerShell script if your *DestDir* directory needs Administrator privilege toocreate or toowrite tooit.</span></span>
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

<span data-ttu-id="c7b0f-158">Sikeres végrehajtása után az aktuális munkakönyvtárban módosítja közé tartoznak túl*- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-158">After successful execution, your current working directory changes too*-DestDir*.</span></span> <span data-ttu-id="c7b0f-159">Hozzá kell férnie toosee például az alábbi képernyőképen:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-159">You should be able toosee screen like below:</span></span>

![][19]

<span data-ttu-id="c7b0f-160">Az a *- DestDir*, hajtsa végre a következő rendszergazdai jogú PowerShell-parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-160">In your *-DestDir*, execute hello following PowerShell script in administrator mode:</span></span>

    ./SQLDW_Data_Import.ps1

<span data-ttu-id="c7b0f-161">Ha hello PowerShell-parancsfájl futtatása hello először, kérni fogja tooinput hello információk az Azure SQL DW és az Azure blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-161">When hello PowerShell script runs for hello first time, you will be asked tooinput hello information from your Azure SQL DW and your Azure blob storage account.</span></span> <span data-ttu-id="c7b0f-162">A PowerShell parancsfájl befejeződésekor hello hello hitelesítő adatok első alkalommal fut, bemeneti fog készült tooa konfigurációs fájl SQLDW.conf hello jelen munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-162">When this PowerShell script completes running for hello first time, hello credentials you input will have been written tooa configuration file SQLDW.conf in hello present working directory.</span></span> <span data-ttu-id="c7b0f-163">hello jövőbeli Futtatás a PowerShell parancsfájl hello beállítás tooread valamennyi szükséges paraméterekkel rendelkezik a konfigurációs fájlból.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-163">hello future run of this PowerShell script file has hello option tooread all needed parameters from this configuration file.</span></span> <span data-ttu-id="c7b0f-164">Ha egyes paramétereket kell toochange, dönthet úgy tooinput üdvözlő képernyőt törölni a konfigurációs fájlt, és a bevitel hello paraméterek értékét, kért kérdés vagy toochange hello paraméterértékek hello paraméterek hello SQLDW.conf fájl szerkesztésével az a *- DestDir* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-164">If you need toochange some parameters, you can choose tooinput hello parameters on hello screen upon prompt by deleting this configuration file and inputting hello parameters values as prompted or toochange hello parameter values by editing hello SQLDW.conf file in your *-DestDir* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-165">A sorrend tooavoid séma neve ütközik a már meglévő az Azure SQL DW paraméterek közvetlenül a hello SQLDW.conf fájl olvasásakor 3 számjegyű véletlenszerűen meg van adva toohello sémanév hello SQLDW.conf fájlból hello alapértelmezett séma neve minden egyes futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-165">In order tooavoid schema name conflicts with those that already exist in your Azure SQL DW, when reading parameters directly from hello SQLDW.conf file, a 3-digit random number is added toohello schema name from hello SQLDW.conf file as hello default schema name for each run.</span></span> <span data-ttu-id="c7b0f-166">PowerShell parancsfájl hello kérheti a séma nevének: felhasználói belátása hello neve adható meg.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-166">hello PowerShell script may prompt you for a schema name: hello name may be specified at user discretion.</span></span>
> 
> 

<span data-ttu-id="c7b0f-167">Ez **PowerShell-parancsfájl** fájl befejeződött a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-167">This **PowerShell script** file completes hello following tasks:</span></span>

* <span data-ttu-id="c7b0f-168">**Letölti és telepíti az AzCopy**, ha az AzCopy nincs telepítve</span><span class="sxs-lookup"><span data-stu-id="c7b0f-168">**Downloads and installs AzCopy**, if AzCopy is not already installed</span></span>
  
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
* <span data-ttu-id="c7b0f-169">**Másolja át az adatokat tooyour titkos blob storage-fiók** az AzCopy hello nyilvános blobból</span><span class="sxs-lookup"><span data-stu-id="c7b0f-169">**Copies data tooyour private blob storage account** from hello public blob with AzCopy</span></span>
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* <span data-ttu-id="c7b0f-170">**Polybase (LoadDataToSQLDW.sql végrehajtásával) tooyour Azure SQL DW használatával adatokat tölt** a következő parancsok hello a titkos blob tároló fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-170">**Loads data using Polybase (by executing LoadDataToSQLDW.sql) tooyour Azure SQL DW** from your private blob storage account with hello following commands.</span></span>
  
  * <span data-ttu-id="c7b0f-171">Hozzon létre egy séma</span><span class="sxs-lookup"><span data-stu-id="c7b0f-171">Create a schema</span></span>
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * <span data-ttu-id="c7b0f-172">Egy adatbázishoz kötődő hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-172">Create a database scoped credential</span></span>
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * <span data-ttu-id="c7b0f-173">Az Azure storage-blob egy külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-173">Create an external data source for an Azure storage blob</span></span>
    
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
  * <span data-ttu-id="c7b0f-174">Hozzon létre egy csv-fájl egy külső fájlformátum.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-174">Create an external file format for a csv file.</span></span> <span data-ttu-id="c7b0f-175">Adatok tömörítetlen és mezők hello függőleges vonal választják el egymástól.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-175">Data is uncompressed and fields are separated with hello pipe character.</span></span>
    
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
  * <span data-ttu-id="c7b0f-176">Külső jegy ára és NYC taxi adatkészlet út táblák létrehozása az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-176">Create external fare and trip tables for NYC taxi dataset in Azure blob storage.</span></span>
    
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

    - <span data-ttu-id="c7b0f-177">Adatok betöltése az Azure blob storage tooSQL adatraktár külső táblák</span><span class="sxs-lookup"><span data-stu-id="c7b0f-177">Load data from external tables in Azure blob storage tooSQL Data Warehouse</span></span>

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

    - <span data-ttu-id="c7b0f-178">Hozzon létre egy minta-adattábla (NYCTaxi_Sample), és adatok tooit beszúrása kiválasztása az SQL-lekérdezések hello út és a jegy ára táblákon.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-178">Create a sample data table (NYCTaxi_Sample) and insert data tooit from selecting SQL queries on hello trip and fare tables.</span></span> <span data-ttu-id="c7b0f-179">(Ez az útmutató néhány lépése kell toouse meg ez a minta a táblázat.)</span><span class="sxs-lookup"><span data-stu-id="c7b0f-179">(Some steps of this walkthrough needs toouse this sample table.)</span></span>

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

<span data-ttu-id="c7b0f-180">a storage-fiókok hello földrajzi helye a betöltési idők hatással van.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-180">hello geographic location of your storage accounts affects load times.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-181">Attól függően, hogy a titkos blob storage-fiók földrajzi elhelyezkedése hello, hello folyamat adatok másolása egy nyilvános blob tooyour titkos tárfiókot is körülbelül 15 percet vesz igénybe, vagy akár már és hello feldolgozni az adatokat tölt be a tárfiók Azure SQL DW tooyour 20 percet vehet igénybe, vagy hosszabb.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-181">Depending on hello geographical location of your private blob storage account, hello process of copying data from a public blob tooyour private storage account can take about 15 minutes, or even longer,and hello process of loading data from your storage account tooyour Azure SQL DW could take 20 minutes or longer.</span></span>  
> 
> 

<span data-ttu-id="c7b0f-182">Hogy toodecide milyen műveleteket, ha ismétlődő a forrás és cél fájlok.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-182">You will have toodecide what do if you have duplicate source and destination files.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-183">Ha hello .csv fájlok toobe átmásolva hello nyilvános blob storage tooyour titkos blob storage-fiók már létezik a személyes blob storage-fiók, AzCopy rákérdez, hogy szeretné-e toooverwrite őket.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-183">If hello .csv files toobe copied from hello public blob storage tooyour private blob storage account already exist in your private blob storage account, AzCopy will ask you whether you want toooverwrite them.</span></span> <span data-ttu-id="c7b0f-184">Ha nem szeretné, hogy toooverwrite őket, bemeneti  **n**  megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-184">If you do not want toooverwrite them, input **n** when prompted.</span></span> <span data-ttu-id="c7b0f-185">Ha azt szeretné, hogy toooverwrite **összes** , bemeneti **egy** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-185">If you want toooverwrite **all** of them, input **a** when prompted.</span></span> <span data-ttu-id="c7b0f-186">Is szövegszerkesztőben **y** toooverwrite .csv fájlok külön-külön.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-186">You can also input **y** toooverwrite .csv files individually.</span></span>
> 
> 

![#21. megrajzolásához][21]

<span data-ttu-id="c7b0f-188">Használhatja a saját adatait.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-188">You can use your own data.</span></span> <span data-ttu-id="c7b0f-189">Ha az adatok a valós életben alkalmazásban a helyszíni gépen, AzCopy tooupload a helyszíni adatok tooyour titkos Azure blob storage továbbra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-189">If your data is in your on-premises machine in your real life application, you can still use AzCopy tooupload on-premises data tooyour private Azure blob storage.</span></span> <span data-ttu-id="c7b0f-190">Csak akkor kell toochange hello **forrás** helyét, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, az AzCopy parancs hello PowerShell parancsfájl fájl toohello helyi könyvtárat, az adatokat tartalmazó hello.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-190">You only need toochange hello **Source** location, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in hello AzCopy command of hello PowerShell script file toohello local directory that contains your data.</span></span>

> [!TIP]
> <span data-ttu-id="c7b0f-191">Ha az adatok már a saját Azure blob Storage a valós életben alkalmazásban, kihagyhatja hello AzCopy hello PowerShell-parancsfájl lépést, és közvetlenül a hello adatok tooAzure SQL DW feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-191">If your data is already in your private Azure blob storage in your real life application, you can skip hello AzCopy step in hello PowerShell script and directly upload hello data tooAzure SQL DW.</span></span> <span data-ttu-id="c7b0f-192">Ehhez szükséges további szerkeszti a hello parancsfájl tootailor azt toohello adatformátum.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-192">This will require additional edits of hello script tootailor it toohello format of your data.</span></span>
> 
> 

<span data-ttu-id="c7b0f-193">A Powershell-parancsfájlt is csatlakoztatja az hello adatfájlokba hello feltárása példa SQLDW_Explorations.sql, SQLDW_Explorations.ipynb és SQLDW_Explorations_Scripts.py Azure SQL DW-információkat, hogy ezek a fájlok próbált azonnal készen toobe PowerShell parancsfájl hello után.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-193">This Powershell script also plugs in hello Azure SQL DW information into hello data exploration example files SQLDW_Explorations.sql, SQLDW_Explorations.ipynb, and SQLDW_Explorations_Scripts.py so that these three files are ready toobe tried out instantly after hello PowerShell script completes.</span></span>

<span data-ttu-id="c7b0f-194">Egy sikeres végrehajtása után képernyőn megjelenik alá, például:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-194">After a successful execution, you will see screen like below:</span></span>

![][20]

## <span data-ttu-id="c7b0f-195"><a name="dbexplore"></a>Az adatok feltárása, a szolgáltatás fejlesztés az Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="c7b0f-195"><a name="dbexplore"></a>Data exploration and feature engineering in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="c7b0f-196">Ebben a szakaszban szemben Azure SQL DW közvetlenül az SQL-lekérdezések futtatásával végezzük adatok feltárása és a szolgáltatás generálása **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-196">In this section, we perform data exploration and feature generation by running SQL queries against Azure SQL DW directly using **Visual Studio Data Tools**.</span></span> <span data-ttu-id="c7b0f-197">Ebben a szakaszban használt összes SQL-lekérdezések nevű hello mintaparancsfájl található *SQLDW_Explorations.sql*.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-197">All SQL queries used in this section can be found in hello sample script named *SQLDW_Explorations.sql*.</span></span> <span data-ttu-id="c7b0f-198">Ez a fájl már letöltött tooyour helyi könyvtár hello PowerShell-parancsprogram.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-198">This file has already been downloaded tooyour local directory by hello PowerShell script.</span></span> <span data-ttu-id="c7b0f-199">Azt is megtalálja a [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-199">You can also retrieve it from [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span></span> <span data-ttu-id="c7b0f-200">De hello fájl a Githubban nincs csatlakoztatva hello Azure SQL DW-információ.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-200">But hello file in GitHub does not have hello Azure SQL DW information plugged in.</span></span>

<span data-ttu-id="c7b0f-201">Csatlakozás tooyour Azure SQL DW hello SQL DW-bejelentkezési nevet és jelszót a Visual Studio használatával, és nyissa meg a hello **SQL Object Explorer** tooconfirm hello adatbázis és tábla nincs importálva.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-201">Connect tooyour Azure SQL DW using Visual Studio with hello SQL DW login name and password and open up hello **SQL Object Explorer** tooconfirm hello database and tables have been imported.</span></span> <span data-ttu-id="c7b0f-202">Hello beolvasása *SQLDW_Explorations.sql* fájlt.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-202">Retrieve hello *SQLDW_Explorations.sql* file.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b0f-203">a Parallel Data Warehouse (PDW) Lekérdezésszerkesztő tooopen hello használata **új lekérdezés** parancsot a PDW kiválasztása a hello **SQL Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-203">tooopen a Parallel Data Warehouse (PDW) query editor, use hello **New Query** command while your PDW is selected in hello **SQL Object Explorer**.</span></span> <span data-ttu-id="c7b0f-204">szabványos SQL Lekérdezésszerkesztő hello PDW által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-204">hello standard SQL query editor is not supported by PDW.</span></span>
> 
> 

<span data-ttu-id="c7b0f-205">Az alábbiakban hello típusú adatok feltárása és a szolgáltatás generálása feladatok végre ebben a szakaszban:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-205">Here are hello type of data exploration and feature generation tasks performed in this section:</span></span>

* <span data-ttu-id="c7b0f-206">Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="c7b0f-207">Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="c7b0f-208">Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="c7b0f-209">Szolgáltatások létrehozása és számítási/összehasonlítása út távolság.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="c7b0f-210">Csatlakozás hello két táblák, és bontsa ki a használt toobuild modellek leendő véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

### <a name="data-import-verification"></a><span data-ttu-id="c7b0f-211">Az importálás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-211">Data import verification</span></span>
<span data-ttu-id="c7b0f-212">Ezeket a lekérdezéseket, adja meg a sorok és oszlopok a táblák fel korábban segítségével a Polybase által párhuzamos tömeges importálásához hello hello száma gyors ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-212">These queries provide a quick verification of hello number of rows and columns in hello tables populated earlier using Polybase's parallel bulk import,</span></span>

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

<span data-ttu-id="c7b0f-213">**Kimenet:** 173,179,759 sorok és oszlopok 14 kapja meg.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-213">**Output:** You should get 173,179,759 rows and 14 columns.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="c7b0f-214">Feltárása: Út elosztása medallion szerint</span><span class="sxs-lookup"><span data-stu-id="c7b0f-214">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="c7b0f-215">Ez a példa lekérdezés hello medallions (taxi számok), 100-nál több való adatváltások számát egy adott időszakon belül elvégzett azonosítja.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-215">This example query identifies hello medallions (taxi numbers) that completed more than 100 trips within a specified time period.</span></span> <span data-ttu-id="c7b0f-216">hello lekérdezés előnyös hello particionált tábla hozzáférés óta, akkor annak hello partíciós sémája által **a felvételi\_datetime**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-216">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="c7b0f-217">Hello teljes adatkészlet lekérdezése is teszi particionált tábla hello használata és/vagy vizsgálat index.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-217">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

<span data-ttu-id="c7b0f-218">**Kimenet:** hello lekérdezés kell egy olyan tábla visszaadása hello 13,369 medallions (taxikra) megadása sorokból és hello 2013 befejezte út száma.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-218">**Output:** hello query should return a table with rows specifying hello 13,369 medallions (taxis) and hello number of trip completed by them in 2013.</span></span> <span data-ttu-id="c7b0f-219">hello utolsó oszlop hello számát hello befejeződött való adatváltások számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-219">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="c7b0f-220">Feltárása: Út terjesztési medallion és hack_license</span><span class="sxs-lookup"><span data-stu-id="c7b0f-220">Exploration: Trip distribution by medallion and hack_license</span></span>
<span data-ttu-id="c7b0f-221">Ez a példa hello medallions (taxi számok) azonosítja, és hack_license számok (-illesztőprogramok) az elvégzett több mint 100 való adatváltások számát egy adott időszakon belül.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-221">This example identifies hello medallions (taxi numbers) and hack_license numbers (drivers) that completed more than 100 trips within a specified time period.</span></span>

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

<span data-ttu-id="c7b0f-222">**Kimenet:** hello lekérdezés visszaadja-e 13,369 sorait hello 13,369 autó/illesztőprogram elvégző több adott 100 utazgatással 2013 azonosítók megadása egy tábla.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-222">**Output:** hello query should return a table with 13,369 rows specifying hello 13,369 car/driver IDs that have completed more that 100 trips in 2013.</span></span> <span data-ttu-id="c7b0f-223">hello utolsó oszlop hello számát hello befejeződött való adatváltások számát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-223">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="c7b0f-224">Adatok vizsgálatának: helytelen hosszúsági és/vagy szélességi rekordjainak ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-224">Data quality assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="c7b0f-225">Ez a példa pedig megvizsgálja, ha bármelyik hello hosszúsági és/vagy szélességi mezők vagy érvénytelen értéket tartalmaz (radián fok -90 és 90 között kell lennie), vagy (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-225">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

<span data-ttu-id="c7b0f-226">**Kimenet:** hello lekérdezés érvénytelen hosszúsági és/vagy szélességi mezői lehetnek 837,467 való adatváltások számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-226">**Output:** hello query returns 837,467 trips that have invalid longitude and/or latitude fields.</span></span>

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="c7b0f-227">Feltárása: Nem Formabontó utazgatással terjesztési és Formabontó</span><span class="sxs-lookup"><span data-stu-id="c7b0f-227">Exploration: Tipped vs. not tipped trips distribution</span></span>
<span data-ttu-id="c7b0f-228">Ebben a példában hello száma való adatváltások számát, amely volt Formabontó és hello számát, amely nem volt Formabontó, egy adott időszakra vonatkozó (vagy a teljes adatkészlet hello Ha kiterjedő hello teljes év van beállítva itt) talál.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-228">This example finds hello number of trips that were tipped vs. hello number that were not tipped in a specified time period (or in hello full dataset if covering hello full year as it is set up here).</span></span> <span data-ttu-id="c7b0f-229">Ehhez a terjesztéshez hello bináris címke terjesztési toobe később használatos bináris osztályozási modellezési tükrözi.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-229">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

<span data-ttu-id="c7b0f-230">**Kimenet:** hello lekérdezés kell visszatérési hello következő tipp gyakoriságot hello év 2013: 90,447,622 Formabontó és nem Formabontó 82,264,709.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-230">**Output:** hello query should return hello following tip frequencies for hello year 2013: 90,447,622 tipped and 82,264,709 not-tipped.</span></span>

### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="c7b0f-231">Feltárása: Tipp osztály/tartomány terjesztési</span><span class="sxs-lookup"><span data-stu-id="c7b0f-231">Exploration: Tip class/range distribution</span></span>
<span data-ttu-id="c7b0f-232">Ebben a példában egy adott idő alatt hello tipp tartományok terjesztése kiszámítja az időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-232">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="c7b0f-233">Ez a hello címke osztályokkal rendelkezik, amelyek később használandó multiclass besorolás modellezési hello terjesztése.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-233">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

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

<span data-ttu-id="c7b0f-234">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="c7b0f-234">**Output:**</span></span>

| <span data-ttu-id="c7b0f-235">tip_class</span><span class="sxs-lookup"><span data-stu-id="c7b0f-235">tip_class</span></span> | <span data-ttu-id="c7b0f-236">tip_freq</span><span class="sxs-lookup"><span data-stu-id="c7b0f-236">tip_freq</span></span> |
| --- | --- |
| <span data-ttu-id="c7b0f-237">1</span><span class="sxs-lookup"><span data-stu-id="c7b0f-237">1</span></span> |<span data-ttu-id="c7b0f-238">82230915</span><span class="sxs-lookup"><span data-stu-id="c7b0f-238">82230915</span></span> |
| <span data-ttu-id="c7b0f-239">2</span><span class="sxs-lookup"><span data-stu-id="c7b0f-239">2</span></span> |<span data-ttu-id="c7b0f-240">6198803</span><span class="sxs-lookup"><span data-stu-id="c7b0f-240">6198803</span></span> |
| <span data-ttu-id="c7b0f-241">3</span><span class="sxs-lookup"><span data-stu-id="c7b0f-241">3</span></span> |<span data-ttu-id="c7b0f-242">1932223</span><span class="sxs-lookup"><span data-stu-id="c7b0f-242">1932223</span></span> |
| <span data-ttu-id="c7b0f-243">0</span><span class="sxs-lookup"><span data-stu-id="c7b0f-243">0</span></span> |<span data-ttu-id="c7b0f-244">82264625</span><span class="sxs-lookup"><span data-stu-id="c7b0f-244">82264625</span></span> |
| <span data-ttu-id="c7b0f-245">4</span><span class="sxs-lookup"><span data-stu-id="c7b0f-245">4</span></span> |<span data-ttu-id="c7b0f-246">85765</span><span class="sxs-lookup"><span data-stu-id="c7b0f-246">85765</span></span> |

### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="c7b0f-247">Feltárása: Számítási, és hasonlítsa össze a út távolság</span><span class="sxs-lookup"><span data-stu-id="c7b0f-247">Exploration: Compute and compare trip distance</span></span>
<span data-ttu-id="c7b0f-248">Ebben a példában a felvételi és Gyűjtőtár hosszúság hello alakítja át, és tooSQL a földrajzi szélesség mutat, hello út távolság SQL geográfiai pontok különbség használatával kiszámítja és visszaadja az összehasonlításhoz hello eredmények véletlenszerű minta.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-248">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="c7b0f-249">hello példa eredményekre hello toovalid koordinálja, csak a hello minőségi assessment adatlekérdezés kezelt korábban.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-249">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function toocalculate hello direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
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

### <a name="feature-engineering-using-sql-functions"></a><span data-ttu-id="c7b0f-250">A szolgáltatás műszaki osztály SQL függvények használata</span><span class="sxs-lookup"><span data-stu-id="c7b0f-250">Feature engineering using SQL functions</span></span>
<span data-ttu-id="c7b0f-251">SQL-függvény néha lehet a hatékony beállítás a szolgáltatás mérnöki csapathoz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-251">Sometimes SQL functions can be an efficient option for feature engineering.</span></span> <span data-ttu-id="c7b0f-252">Ebben a bemutatóban meghatározott SQL függvény toocalculate hello közvetlen távolság hello felvétel és a dropoff között.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-252">In this walkthrough, we defined a SQL function toocalculate hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="c7b0f-253">Futtathatja a következő SQL-parancsfájlok a hello **Visual Studio Data Tools**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-253">You can run hello following SQL scripts in **Visual Studio Data Tools**.</span></span>

<span data-ttu-id="c7b0f-254">Itt található, amely meghatározza a hello távolság függvény hello SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-254">Here is hello SQL script that defines hello distance function.</span></span>

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate hello direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

<span data-ttu-id="c7b0f-255">Íme egy példa toocall a függvény toogenerate szolgáltatás az SQL-lekérdezésben:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-255">Here is an example toocall this function toogenerate features in your SQL query:</span></span>

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="c7b0f-256">**Kimenet:** Ez a lekérdezés az (2,803,538 sorok) tábla a felvétel és dropoff földrajzi szélesség hoz létre, és a hosszúságot, a megfelelő hello közvetlen miles megadott.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-256">**Output:** This query generates a table (with 2,803,538 rows) with pickup and dropoff latitudes and longitudes and hello corresponding direct distances in miles.</span></span> <span data-ttu-id="c7b0f-257">Az alábbiakban az első 3 sort az hello eredménye:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-257">Here are hello results for first 3 rows:</span></span>

|  | <span data-ttu-id="c7b0f-258">pickup_latitude</span><span class="sxs-lookup"><span data-stu-id="c7b0f-258">pickup_latitude</span></span> | <span data-ttu-id="c7b0f-259">pickup_longitude</span><span class="sxs-lookup"><span data-stu-id="c7b0f-259">pickup_longitude</span></span> | <span data-ttu-id="c7b0f-260">dropoff_latitude</span><span class="sxs-lookup"><span data-stu-id="c7b0f-260">dropoff_latitude</span></span> | <span data-ttu-id="c7b0f-261">dropoff_longitude</span><span class="sxs-lookup"><span data-stu-id="c7b0f-261">dropoff_longitude</span></span> | <span data-ttu-id="c7b0f-262">DirectDistance</span><span class="sxs-lookup"><span data-stu-id="c7b0f-262">DirectDistance</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="c7b0f-263">1</span><span class="sxs-lookup"><span data-stu-id="c7b0f-263">1</span></span> |<span data-ttu-id="c7b0f-264">40.731804</span><span class="sxs-lookup"><span data-stu-id="c7b0f-264">40.731804</span></span> |<span data-ttu-id="c7b0f-265">-74.001083</span><span class="sxs-lookup"><span data-stu-id="c7b0f-265">-74.001083</span></span> |<span data-ttu-id="c7b0f-266">40.736622</span><span class="sxs-lookup"><span data-stu-id="c7b0f-266">40.736622</span></span> |<span data-ttu-id="c7b0f-267">-73.988953</span><span class="sxs-lookup"><span data-stu-id="c7b0f-267">-73.988953</span></span> |<span data-ttu-id="c7b0f-268">.7169601222</span><span class="sxs-lookup"><span data-stu-id="c7b0f-268">.7169601222</span></span> |
| <span data-ttu-id="c7b0f-269">2</span><span class="sxs-lookup"><span data-stu-id="c7b0f-269">2</span></span> |<span data-ttu-id="c7b0f-270">40.715794</span><span class="sxs-lookup"><span data-stu-id="c7b0f-270">40.715794</span></span> |<span data-ttu-id="c7b0f-271">-74,010635</span><span class="sxs-lookup"><span data-stu-id="c7b0f-271">-74,010635</span></span> |<span data-ttu-id="c7b0f-272">40.725338</span><span class="sxs-lookup"><span data-stu-id="c7b0f-272">40.725338</span></span> |<span data-ttu-id="c7b0f-273">-74.00399</span><span class="sxs-lookup"><span data-stu-id="c7b0f-273">-74.00399</span></span> |<span data-ttu-id="c7b0f-274">.7448343721</span><span class="sxs-lookup"><span data-stu-id="c7b0f-274">.7448343721</span></span> |
| <span data-ttu-id="c7b0f-275">3</span><span class="sxs-lookup"><span data-stu-id="c7b0f-275">3</span></span> |<span data-ttu-id="c7b0f-276">40.761456</span><span class="sxs-lookup"><span data-stu-id="c7b0f-276">40.761456</span></span> |<span data-ttu-id="c7b0f-277">-73.999886</span><span class="sxs-lookup"><span data-stu-id="c7b0f-277">-73.999886</span></span> |<span data-ttu-id="c7b0f-278">40.766544</span><span class="sxs-lookup"><span data-stu-id="c7b0f-278">40.766544</span></span> |<span data-ttu-id="c7b0f-279">-73.988228</span><span class="sxs-lookup"><span data-stu-id="c7b0f-279">-73.988228</span></span> |<span data-ttu-id="c7b0f-280">0.7037227967</span><span class="sxs-lookup"><span data-stu-id="c7b0f-280">0.7037227967</span></span> |

### <a name="prepare-data-for-model-building"></a><span data-ttu-id="c7b0f-281">Adatok előkészítése a modell létrehozásának</span><span class="sxs-lookup"><span data-stu-id="c7b0f-281">Prepare data for model building</span></span>
<span data-ttu-id="c7b0f-282">hello következő lekérdezés illesztések hello **nyctaxi\_út** és **nyctaxi\_jegy ára** táblák, állít elő, a bináris besorolási címkék **Formabontó**, egy több osztály besorolási címke **tipp\_osztály**, és egy minta kiolvassa a hello teljes illesztett adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-282">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a sample from hello full joined dataset.</span></span> <span data-ttu-id="c7b0f-283">hello mintavétel felvételi ideje alapján hello utazgatással részhalmazának lekérésével.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-283">hello sampling is done by retrieving a subset of hello trips based on pickup time.</span></span>  <span data-ttu-id="c7b0f-284">Ez a lekérdezés másolja, majd közvetlenül az hello beillesztett [Azure Machine Learning Studio](https://studio.azureml.net) [és adatokat importálhat] [ import-data] hello SQL database-példányt a közvetlen adatfeldolgozást-modul az Azure.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-284">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL database instance in Azure.</span></span> <span data-ttu-id="c7b0f-285">hello lekérdezés nem tartalmazza a rekordok helytelen (0, 0) koordinátarendszerében.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-285">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

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

<span data-ttu-id="c7b0f-286">Amikor készen áll a tooproceed tooAzure gépi tanulás, akkor előfordulhat, hogy vagy:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-286">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="c7b0f-287">Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] modul az Azure Machine Learning, vagy</span><span class="sxs-lookup"><span data-stu-id="c7b0f-287">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="c7b0f-288">Hello mintát megmaradnak, és azt tervezi, hogy az új SQL-Adatraktár létrehozása modell toouse visszafejtett adatok tábla, és a hello új tábla hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-288">Persist hello sampled and engineered data you plan toouse for model building in a new SQL DW table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="c7b0f-289">PowerShell-parancsfájl a korábbi lépésben hello végzett ezt meg.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-289">hello PowerShell script in earlier step has done this for you.</span></span> <span data-ttu-id="c7b0f-290">Ez a táblázat hello adatokat importálhat modul közvetlenül a olvashatja.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-290">You can read directly from this table in hello Import Data module.</span></span>

## <span data-ttu-id="c7b0f-291"><a name="ipnb"></a>Az adatok feltárása, a szolgáltatás fejlesztés IPython jegyzetfüzet</span><span class="sxs-lookup"><span data-stu-id="c7b0f-291"><a name="ipnb"></a>Data exploration and feature engineering in IPython notebook</span></span>
<span data-ttu-id="c7b0f-292">Ebben a szakaszban végezzük el az adatok feltárása és a szolgáltatás generálása mindkét pythonos környezetekben, és a korábban létrehozott SQL DW hello SQL-lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-292">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL DW created earlier.</span></span> <span data-ttu-id="c7b0f-293">Egy minta IPython notebook nevű **SQLDW_Explorations.ipynb** és a Python-parancsfájl **SQLDW_Explorations_Scripts.py** letöltött tooyour helyi könyvtárba lett volna.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-293">A sample IPython notebook named **SQLDW_Explorations.ipynb** and a Python script file **SQLDW_Explorations_Scripts.py** have been downloaded tooyour local directory.</span></span> <span data-ttu-id="c7b0f-294">Akkor is elérhetők a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-294">They are also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span></span> <span data-ttu-id="c7b0f-295">Ezeket a fájlokat két Python parancsfájlok azonosak.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-295">These two files are identical in Python scripts.</span></span> <span data-ttu-id="c7b0f-296">hello Python-parancsfájl tooyou valósul meg, abban az esetben, ha nem rendelkezik egy IPython Notebook kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-296">hello Python script file is provided tooyou in case you do not have an IPython Notebook server.</span></span> <span data-ttu-id="c7b0f-297">Ez a két minta úgy vannak beállítva, a Python **Python 2.7**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-297">These two sample Python files are designed under **Python 2.7**.</span></span>

<span data-ttu-id="c7b0f-298">hello hello mintában IPython Notebook szükséges Azure SQL DW-adatokat, és a Python-parancsfájl fájl letöltött tooyour helyi számítógép rendelkezik lett csatlakoztatva hello PowerShell-parancsfájl által korábban hello.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-298">hello needed Azure SQL DW information in hello sample IPython Notebook and hello Python script file downloaded tooyour local machine has been plugged in by hello PowerShell script previously.</span></span> <span data-ttu-id="c7b0f-299">Azok a módosítás nélkül végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-299">They are executable without any modification.</span></span>

<span data-ttu-id="c7b0f-300">Ha már az AzureML-munkaterülethez, közvetlenül hello minta IPython Notebook toohello AzureML IPython Notebook szolgáltatás feltöltése és elindultak, azt.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-300">If you have already set up an AzureML workspace, you can directly upload hello sample IPython Notebook toohello AzureML IPython Notebook service and start running it.</span></span> <span data-ttu-id="c7b0f-301">Az alábbiakban hello lépéseket tooupload tooAzureML IPython Notebook szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-301">Here are hello steps tooupload tooAzureML IPython Notebook service:</span></span>

1. <span data-ttu-id="c7b0f-302">Jelentkezzen be tooyour AzureML munkaterület, hello tetején kattintson az "Studio" és "NOTEBOOKOK" hello hello weblap bal oldalán kattintson.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-302">Log in tooyour AzureML workspace, click "Studio" at hello top, and click "NOTEBOOKS" on hello left side of hello web page.</span></span>
   
    ![#22 ábrázolása][22]
2. <span data-ttu-id="c7b0f-304">Kattintson az "Új" hello weblap hello bal alsó sarkában, és jelölje ki "Python 2".</span><span class="sxs-lookup"><span data-stu-id="c7b0f-304">Click "NEW" on hello left bottom corner of hello web page, and select "Python 2".</span></span> <span data-ttu-id="c7b0f-305">Ezt követően adjon meg egy nevet toohello notebook, és hello pipa toocreate hello új üres IPython Notebook kattintson.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-305">Then, provide a name toohello notebook and click hello check mark toocreate hello new blank IPython Notebook.</span></span>
   
    ![#23 ábrázolása][23]
3. <span data-ttu-id="c7b0f-307">Kattintson a bal felső sarkának hello hello hello "Jupyter" szimbólum IPython új Notebookot.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-307">Click hello "Jupyter" symbol on hello left top corner of hello new IPython Notebook.</span></span>
   
    ![#24 ábrázolása][24]
4. <span data-ttu-id="c7b0f-309">Hello minta IPython Notebook toohello áthúzása **fa** az AzureML IPython Notebook szolgáltatást, majd kattintson a lap **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-309">Drag and drop hello sample IPython Notebook toohello **tree** page of your AzureML IPython Notebook service, and click **Upload**.</span></span> <span data-ttu-id="c7b0f-310">Ezt követően a hello minta IPython Notebook feltöltött toohello AzureML IPython Notebook szolgáltatás lesz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-310">Then, hello sample IPython Notebook will be uploaded toohello AzureML IPython Notebook service.</span></span>
   
    ![#25 ábrázolása][25]

<span data-ttu-id="c7b0f-312">A sorrend toorun hello minta IPython Notebook vagy hello Python-parancsfájl, hello következő Python csomagok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-312">In order toorun hello sample IPython Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="c7b0f-313">Ha hello AzureML IPython Notebook szolgáltatást használja, ezeket a csomagokat manuálisan telepítette.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-313">If you are using hello AzureML IPython Notebook service, these packages have been pre-installed.</span></span>

    - <span data-ttu-id="c7b0f-314">pandas</span><span class="sxs-lookup"><span data-stu-id="c7b0f-314">pandas</span></span>
    - <span data-ttu-id="c7b0f-315">numpy</span><span class="sxs-lookup"><span data-stu-id="c7b0f-315">numpy</span></span>
    - <span data-ttu-id="c7b0f-316">matplotlib</span><span class="sxs-lookup"><span data-stu-id="c7b0f-316">matplotlib</span></span>
    - <span data-ttu-id="c7b0f-317">pyodbc</span><span class="sxs-lookup"><span data-stu-id="c7b0f-317">pyodbc</span></span>
    - <span data-ttu-id="c7b0f-318">PyTables</span><span class="sxs-lookup"><span data-stu-id="c7b0f-318">PyTables</span></span>

<span data-ttu-id="c7b0f-319">ajánlott az sorozat építve speciális elemzési megoldásokat AzureML nagy adatokkal hello hello következő:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-319">hello recommended sequence when building advanced analytical solutions on AzureML with large data is hello following:</span></span>

* <span data-ttu-id="c7b0f-320">Olvassa el egy a memóriában levő keretbe hello adatok mintát.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-320">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="c7b0f-321">Hajtsa végre az egyes képi megjelenítéseket, és segítségével explorations hello mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-321">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="c7b0f-322">Összehasonlítást az adatminta szolgáltatás mérnöki hello segítségével a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-322">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="c7b0f-323">A nagyobb az adatok feltárása a adatkezelési és a szolgáltatás mérnöki csapathoz, Python tooissue SQL lekérdezések közvetlenül hello SQL DW használjon.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-323">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL DW.</span></span>
* <span data-ttu-id="c7b0f-324">Döntse el, hello minta mérete toobe Azure Machine Learning modell létrehozásának alkalmas.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-324">Decide hello sample size toobe suitable for Azure Machine Learning model building.</span></span>

<span data-ttu-id="c7b0f-325">hello következőket a következők: néhány adatok feltárása, adatábrázolási és a szolgáltatás mérnöki példák.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-325">hello followings are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="c7b0f-326">További adatok explorations hello minta IPython jegyzetfüzet és hello minta Python-parancsfájl találhatók.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-326">More data explorations can be found in hello sample IPython Notebook and hello sample Python script file.</span></span>

### <a name="initialize-database-credentials"></a><span data-ttu-id="c7b0f-327">Adatbázis-hitelesítő adatok inicializálása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-327">Initialize database credentials</span></span>
<span data-ttu-id="c7b0f-328">Az adatbázis-kapcsolati beállításokat a következő változók hello inicializálása:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-328">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a><span data-ttu-id="c7b0f-329">Adatbázis-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-329">Create database connection</span></span>
<span data-ttu-id="c7b0f-330">Itt található hello kapcsolati karakterlánc, amely hello kapcsolat toohello adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-330">Here is hello connection string that creates hello connection toohello database.</span></span>

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="c7b0f-331">Sorok és oszlopok a tábla < nyctaxi_trip > jelentés száma</span><span class="sxs-lookup"><span data-stu-id="c7b0f-331">Report number of rows and columns in table <nyctaxi_trip></span></span>
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

* <span data-ttu-id="c7b0f-332">A sorok teljes számát = 173179759</span><span class="sxs-lookup"><span data-stu-id="c7b0f-332">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="c7b0f-333">Az oszlopok teljes száma = 14</span><span class="sxs-lookup"><span data-stu-id="c7b0f-333">Total number of columns = 14</span></span>

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a><span data-ttu-id="c7b0f-334">Sorok és oszlopok a tábla < nyctaxi_fare > jelentés száma</span><span class="sxs-lookup"><span data-stu-id="c7b0f-334">Report number of rows and columns in table <nyctaxi_fare></span></span>
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

* <span data-ttu-id="c7b0f-335">A sorok teljes számát = 173179759</span><span class="sxs-lookup"><span data-stu-id="c7b0f-335">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="c7b0f-336">Az oszlopok teljes száma = 11</span><span class="sxs-lookup"><span data-stu-id="c7b0f-336">Total number of columns = 11</span></span>

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a><span data-ttu-id="c7b0f-337">Olvassa el a hello SQL-adatraktár-adatbázis egy kisméretű mintát</span><span class="sxs-lookup"><span data-stu-id="c7b0f-337">Read-in a small data sample from hello SQL Data Warehouse Database</span></span>
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
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="c7b0f-338">Idő tooread hello mintatáblázat: 14.096495 másodperc.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-338">Time tooread hello sample table is 14.096495 seconds.</span></span>  
<span data-ttu-id="c7b0f-339">A beolvasott sorok és oszlopok száma = (1000, 21).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-339">Number of rows and columns retrieved = (1000, 21).</span></span>

### <a name="descriptive-statistics"></a><span data-ttu-id="c7b0f-340">Leíró statisztika</span><span class="sxs-lookup"><span data-stu-id="c7b0f-340">Descriptive statistics</span></span>
<span data-ttu-id="c7b0f-341">Most már áll készen tooexplore hello mintavételezett adatokat.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-341">Now you are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="c7b0f-342">Először néhány leíró statisztikája hello megnézi **út\_távolság** (vagy bármely más mezők kiválasztása toospecify).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-342">We start with looking at some descriptive statistics for hello **trip\_distance** (or any other fields you choose toospecify).</span></span>

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a><span data-ttu-id="c7b0f-343">A képi megjelenítés: Rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="c7b0f-343">Visualization: Box plot example</span></span>
<span data-ttu-id="c7b0f-344">Ezután úgy tekintünk hello Dobozdiagram hello út távolság toovisualize hello quantiles számára.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-344">Next we look at hello box plot for hello trip distance toovisualize hello quantiles.</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Tőzsdei #1][1]

### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="c7b0f-346">A képi megjelenítés: Terjesztési rajzot – példa</span><span class="sxs-lookup"><span data-stu-id="c7b0f-346">Visualization: Distribution plot example</span></span>
<span data-ttu-id="c7b0f-347">Felvétel, megjelenítheti a hello terjesztési és hello a hisztogram út távolság mintát.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-347">Plots that visualize hello distribution and a histogram for hello sampled trip distances.</span></span>

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tőzsdei #2][2]

### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="c7b0f-349">A képi megjelenítés: Sáv megnyitásához, majd sor ábra</span><span class="sxs-lookup"><span data-stu-id="c7b0f-349">Visualization: Bar and line plots</span></span>
<span data-ttu-id="c7b0f-350">Ebben a példában a Microsoft hello út távolság az öt bins bin és hello dobozolás eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-350">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="c7b0f-351">A Microsoft hello fent egy sávon bin terjesztési megrajzolásához, vagy a rajzolási. sor:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-351">We can plot hello above bin distribution in a bar or line plot with:</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

<span data-ttu-id="c7b0f-353">és</span><span class="sxs-lookup"><span data-stu-id="c7b0f-353">and</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

### <a name="visualization-scatterplot-examples"></a><span data-ttu-id="c7b0f-355">A képi megjelenítés: Scatterplot példák</span><span class="sxs-lookup"><span data-stu-id="c7b0f-355">Visualization: Scatterplot examples</span></span>
<span data-ttu-id="c7b0f-356">Pontdiagram rajzot közötti megmutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** toosee bármely korrelációs esetén</span><span class="sxs-lookup"><span data-stu-id="c7b0f-356">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

<span data-ttu-id="c7b0f-358">Hasonlóképpen azt hello közötti kapcsolat ellenőrzése **arány\_kód** és **út\_távolság**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-358">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="c7b0f-360">Az adatok feltárása a mintaadatokat az SQL-lekérdezések IPython notebook használatával</span><span class="sxs-lookup"><span data-stu-id="c7b0f-360">Data exploration on sampled data using SQL queries in IPython notebook</span></span>
<span data-ttu-id="c7b0f-361">Ebben a részben azt megismerkedhet adatok azokat a terjesztéseket használatával mintát hello adat, amely a fenti létrehozott hello új tábla megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-361">In this section, we explore data distributions using hello sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="c7b0f-362">Vegye figyelembe, hogy hasonló explorations végrehajtható hello eredeti táblák használata.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-362">Note that similar explorations can be performed using hello original tables.</span></span>

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a><span data-ttu-id="c7b0f-363">Feltárása: Sorok és oszlopok hello a jelentés száma mintát venni tábla</span><span class="sxs-lookup"><span data-stu-id="c7b0f-363">Exploration: Report number of rows and columns in hello sampled table</span></span>
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a><span data-ttu-id="c7b0f-364">Feltárása: A Formabontó/nem terjesztési azokat kioldják</span><span class="sxs-lookup"><span data-stu-id="c7b0f-364">Exploration: Tipped/not tripped Distribution</span></span>
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a><span data-ttu-id="c7b0f-365">Feltárása: Tipp osztály terjesztési</span><span class="sxs-lookup"><span data-stu-id="c7b0f-365">Exploration: Tip class distribution</span></span>
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a><span data-ttu-id="c7b0f-366">Feltárása: Hello tipp terjesztési megrajzolásához osztály</span><span class="sxs-lookup"><span data-stu-id="c7b0f-366">Exploration: Plot hello tip distribution by class</span></span>
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 ábrázolása][26]

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="c7b0f-368">Feltárása: Való adatváltások számát napi eloszlásáról</span><span class="sxs-lookup"><span data-stu-id="c7b0f-368">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="c7b0f-369">Feltárása: Utazás per medallion terjesztési</span><span class="sxs-lookup"><span data-stu-id="c7b0f-369">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a><span data-ttu-id="c7b0f-370">Feltárása: Út terjesztési medallion és rejthetők el licenc által</span><span class="sxs-lookup"><span data-stu-id="c7b0f-370">Exploration: Trip distribution by medallion and hack license</span></span>
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a><span data-ttu-id="c7b0f-371">Feltárása: Út idő terjesztése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-371">Exploration: Trip time distribution</span></span>
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a><span data-ttu-id="c7b0f-372">Feltárása: Út távolság terjesztési</span><span class="sxs-lookup"><span data-stu-id="c7b0f-372">Exploration: Trip distance distribution</span></span>
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a><span data-ttu-id="c7b0f-373">Feltárása: Fizetési típus terjesztési</span><span class="sxs-lookup"><span data-stu-id="c7b0f-373">Exploration: Payment type distribution</span></span>
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="c7b0f-374">Hello végső űrlap hello featurized tábla ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-374">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <span data-ttu-id="c7b0f-375"><a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c7b0f-375"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="c7b0f-376">Dolgozunk most már készen áll a tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-376">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="c7b0f-377">hello adata készen áll, nevezetesen korábban azonosított hello előrejelzés problémák valamelyikében használt toobe:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-377">hello data is ready toobe used in any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="c7b0f-378">**Bináris osztályozási**: toopredict tipp egy út kifizetett-e.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-378">**Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="c7b0f-379">**Multiclass besorolási**: toopredict hello tartomány tipp fizetős, toohello korábban definiált osztályok szerint.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-379">**Multiclass classification**: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="c7b0f-380">**Regressziós feladat**: toopredict hello tipp fizetni útnak.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-380">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

<span data-ttu-id="c7b0f-381">toobegin hello modellezési gyakorlat, jelentkezzen be tooyour **Azure Machine Learning** munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-381">toobegin hello modeling exercise, log in tooyour **Azure Machine Learning** workspace.</span></span> <span data-ttu-id="c7b0f-382">Ha még nem hozott létre machine learning-munkaterület, lásd: [hozzon létre egy Azure ML munkaterületet](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-382">If you have not yet created a machine learning workspace, see [Create an Azure ML workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="c7b0f-383">az Azure Machine Learning lépései tooget lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="c7b0f-383">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="c7b0f-384">Jelentkezzen be túl[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-384">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="c7b0f-385">hello Studio kezdőlap számos olyan információt, videók, oktatóanyagok, hivatkozások toohello modulok hivatkozás és egyéb erőforrásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-385">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="c7b0f-386">Azure Machine Learning kapcsolatos további információkért tekintse meg a hello [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-386">For more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="c7b0f-387">Egy tipikus tanítási kísérletet hello lépéseket foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-387">A typical training experiment consists of hello following steps:</span></span>

1. <span data-ttu-id="c7b0f-388">Hozzon létre egy **+ új** kipróbálásához.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-388">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="c7b0f-389">Azure ml hello adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-389">Get hello data into Azure ML.</span></span>
3. <span data-ttu-id="c7b0f-390">Előre feldolgozzák, átalakítás és kezelhetők hello adatok, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-390">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="c7b0f-391">Szolgáltatások létrehozása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-391">Generate features as needed.</span></span>
5. <span data-ttu-id="c7b0f-392">Hello adatok felosztása adatkészletek képzési/érvényesítési/tesztelése (külön adatkészleteket, vagy az egyes).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-392">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="c7b0f-393">Válasszon egy vagy több számítógépet tanulási algoritmus attól függően, hogy a probléma toosolve tanulási hello.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-393">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="c7b0f-394">Például bináris osztályozási multiclass besorolás, regressziós.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-394">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="c7b0f-395">Hello képzési adatkészlet egy vagy több modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-395">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="c7b0f-396">Pontszám hello érvényesítési dataset hello betanított modellek segítségével.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-396">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="c7b0f-397">Hello modellek toocompute hello vonatkozó metrikáinak probléma tanulási hello kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-397">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="c7b0f-398">Konfigurálva finomhangolhatják a hello modellek és select hello legjobb modell toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-398">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="c7b0f-399">Ebben a gyakorlatban azt már megismerkedett és fejthetők vissza az SQL Data Warehouse hello adatokat, és úgy döntött, a hello minta mérete tooingest Azure ml.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-399">In this exercise, we have already explored and engineered hello data in SQL Data Warehouse, and decided on hello sample size tooingest in Azure ML.</span></span> <span data-ttu-id="c7b0f-400">Hello eljárás toobuild Ez egy vagy több hello előrejelzési modellek:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-400">Here is hello procedure toobuild one or more of hello prediction models:</span></span>

1. <span data-ttu-id="c7b0f-401">Hello adatok beolvasása az Azure ml hello segítségével [és adatokat importálhat] [ import-data] modul hello elérhető **bemeneti és kimeneti** szakasz.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-401">Get hello data into Azure ML using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="c7b0f-402">További információkért lásd: hello [és adatokat importálhat] [ import-data] modul referencialapja.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-402">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Azure ML importálási adatok][17]
2. <span data-ttu-id="c7b0f-404">Válassza ki **Azure SQL Database** , hello **adatforrás** a hello **tulajdonságok** panel.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-404">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="c7b0f-405">Adja meg a hello adatbázis DNS-név hello **adatbázis-kiszolgáló neve** mező.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-405">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="c7b0f-406">Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="c7b0f-406">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="c7b0f-407">Adja meg a hello **adatbázisnév** hello megfelelő mezőben.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-407">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="c7b0f-408">Adja meg a hello *SQL felhasználónév* a hello **kiszolgáló felhasználói fiók nevét**, és hello *jelszó* hello a **kiszolgáló felhasználói fiók jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-408">Enter hello *SQL user name* in hello **Server user account name**, and hello *password* in hello **Server user account password**.</span></span>
6. <span data-ttu-id="c7b0f-409">Ellenőrizze a hello **fogadja el a kiszolgálói tanúsítvány** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-409">Check hello **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="c7b0f-410">A hello **adatbázis-lekérdezés** szerkesztése területre, és beillesztheti hello szükséges adatbázis-(beleértve a számított mezőket például hello címkék) mezőt, és le kivonatok minták hello adatok szükséges toohello mintaméret hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-410">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="c7b0f-411">Példa egy bináris osztályozási kísérletet, közvetlenül a hello SQL Data Warehouse-adatbázis adatainak olvasása: hello az alábbi ábra a (ne feledje tooreplace hello táblázatok neve nyctaxi_trip és nyctaxi_fare hello séma által használt nevét és hello táblanevek a a forgatókönyv).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-411">An example of a binary classification experiment reading data directly from hello SQL Data Warehouse database is in hello figure below (remember tooreplace hello table names nyctaxi_trip and nyctaxi_fare by hello schema name and hello table names you used in your walkthrough).</span></span> <span data-ttu-id="c7b0f-412">Hasonló kísérletek multiclass besorolás és a visszavonási lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-412">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Az Azure ML vonat][10]

> [!IMPORTANT]
> <span data-ttu-id="c7b0f-414">Az adatok kinyerése modellezési, és a korábbi szakaszokban bemutatott lekérdezés példák mintavételi hello **hello három modellezési gyakorlatok az összes felirathoz szerepelnek hello lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-414">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="c7b0f-415">Az egyes gyakorlatok modellezési hello (kötelező) fontos lépés túl van**kizárása** hello szükségtelen címkéit hello más két problémákat, és minden egyéb **kiszivárgásának cél**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-415">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="c7b0f-416">Például bináris osztályozási használatakor használjon hello címke **Formabontó** , és amelyeket hello mezők **tipp\_osztály**, **tipp\_összeg**, és **teljes\_összeg**.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-416">For example, when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="c7b0f-417">Ez utóbbi hello cél kiszivárgásának, mivel azok utalnak hello tipp fizetett.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-417">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="c7b0f-418">tooexclude bármely felesleges oszlopok vagy a cél kiszivárgásának hello segítségével [Select Columns in Dataset] [ select-columns] modul vagy hello [szerkesztése metaadatok] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="c7b0f-418">tooexclude any unnecessary columns or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="c7b0f-419">További információkért lásd: [Select Columns in Dataset] [ select-columns] és [szerkesztése metaadatok] [ edit-metadata] lapok hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-419">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="c7b0f-420"><a name="mldeploy"></a>Az Azure Machine Learning modellek telepítése</span><span class="sxs-lookup"><span data-stu-id="c7b0f-420"><a name="mldeploy"></a>Deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="c7b0f-421">Amikor készen áll a modell, könnyedén telepítheti azt közvetlenül a hello kísérlet webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-421">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="c7b0f-422">Azure ML web services telepítésével kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="c7b0f-422">For more information about deploying Azure ML web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="c7b0f-423">egy új webszolgáltatás-bővítmény toodeploy, kell:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-423">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="c7b0f-424">A pontozási kísérlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-424">Create a scoring experiment.</span></span>
2. <span data-ttu-id="c7b0f-425">Hello webes szolgáltatás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-425">Deploy hello web service.</span></span>

<span data-ttu-id="c7b0f-426">a pontozási kísérletezhet a toocreate egy **befejezett** betanítása kísérletet, kattintson **létrehozása pontozási KÍSÉRLETEZHET** hello alacsonyabb művelet sávon.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-426">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Pontozás az Azure][18]

<span data-ttu-id="c7b0f-428">Az Azure Machine Learning toocreate egy pontozási kísérletet hello tanítási kísérletet hello összetevői alapján kísérli meg.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-428">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="c7b0f-429">Különösen a következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="c7b0f-429">In particular, it will:</span></span>

1. <span data-ttu-id="c7b0f-430">Hello betanított modell mentéséhez, és távolítsa el a hello modell képzési modulok.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-430">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="c7b0f-431">Azonosítsa a logikai **bemeneti port** toorepresent hello várt bemeneti adatok séma.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-431">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="c7b0f-432">Azonosítsa a logikai **kimeneti port** toorepresent hello várt webes szolgáltatás kimeneti sémával.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-432">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="c7b0f-433">Kísérlet pontozási hello létrehozásakor, tekintse át, és ellenőrizze, hogy szükség szerint állítsa be.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-433">When hello scoring experiment is created, review it and make adjust as needed.</span></span> <span data-ttu-id="c7b0f-434">Egy tipikus módosítás tooreplace hello bemeneti adatkészlet és/vagy entitáselem, amely kizárja a címke mezők esetén, mert ezek nem lesz elérhető hello szolgáltatás meghívásakor.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-434">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="c7b0f-435">Akkor is hello célszerű tooreduce hello méretet adjon meg adatkészlet és/vagy lekérdezési tooa néhány rekordjának, éppen elegendő tooindicate hello bemeneti sémát.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-435">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="c7b0f-436">A hello kimeneti portra, azt közös tooexclude összes beviteli mezőt, és csak terjed hello **pontozott címkék** és **program pontozza a mennyiségeket valószínűség** hello hello használja a [Select Columns in Dataset ] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-436">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="c7b0f-437">Kísérlet pontozási minta hello az alábbi ábra valósul meg.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-437">A sample scoring experiment is provided in hello figure below.</span></span> <span data-ttu-id="c7b0f-438">Ha készen áll a toodeploy, kattintson a hello **WEBSZOLGÁLTATÁS közzététele** hello alacsonyabb művelet található gombra.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-438">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Az Azure ML közzététele][11]

## <a name="summary"></a><span data-ttu-id="c7b0f-440">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c7b0f-440">Summary</span></span>
<span data-ttu-id="c7b0f-441">toorecap mi azt megtette az bemutató oktatóanyag, hozott létre az Azure data tudományos környezethez, a nagy nyilvános dataset dolgozott véve a csapat az tudományos folyamata, beszerzési toomodel betanítási adatok, az összes hello módon hello keresztül, majd az Azure Machine Learning webszolgáltatás üzembe helyezése toohello.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-441">toorecap what we have done in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset, taking it through hello Team Data Science Process, all hello way from data acquisition toomodel training, and then toohello deployment of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="c7b0f-442">Licencinformációk</span><span class="sxs-lookup"><span data-stu-id="c7b0f-442">License information</span></span>
<span data-ttu-id="c7b0f-443">Ez a minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft hello MIT licence.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-443">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="c7b0f-444">Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.</span><span class="sxs-lookup"><span data-stu-id="c7b0f-444">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="c7b0f-445">Referencia</span><span class="sxs-lookup"><span data-stu-id="c7b0f-445">References</span></span>
<span data-ttu-id="c7b0f-446">• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="c7b0f-446">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="c7b0f-447">• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="c7b0f-447">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="c7b0f-448">• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="c7b0f-448">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

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
