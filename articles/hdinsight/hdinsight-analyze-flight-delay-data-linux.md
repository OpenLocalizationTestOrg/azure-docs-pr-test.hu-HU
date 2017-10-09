---
title: "a HDInsight - Azure Hive aaaAnalyze repülési késleltetés adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse tooanalyze felé továbbított adatok a Linux-alapú HDInsight Hive, majd exportálja hello adatok tooSQL adatbázis-Sqoop használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 7830457a7100880dff1c647dde1b4d203bfea3c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="23a9f-103">A Linux-alapú HDInsight Hive használatával repülési késleltetés adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="23a9f-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="23a9f-104">Ismerje meg, hogyan tooanalyze repülési késleltetés Hive használata a Linux-alapú HDInsight majd adatexportálás hello adatok tooAzure SQL-adatbázis Sqoop használatával.</span><span class="sxs-lookup"><span data-stu-id="23a9f-104">Learn how tooanalyze flight delay data using Hive on Linux-based HDInsight then export hello data tooAzure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="23a9f-105">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="23a9f-105">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="23a9f-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="23a9f-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="23a9f-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="23a9f-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="23a9f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23a9f-108">Prerequisites</span></span>

* <span data-ttu-id="23a9f-109">**HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="23a9f-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="23a9f-110">Lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md) vonatkozó lépéseket egy új Linux-alapú HDInsight-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="23a9f-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="23a9f-111">**Azure SQL Database**</span><span class="sxs-lookup"><span data-stu-id="23a9f-111">**Azure SQL Database**.</span></span> <span data-ttu-id="23a9f-112">Azure SQL-adatbázis használata a cél-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="23a9f-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="23a9f-113">Ha egy SQL-adatbázis már nem rendelkezik, tekintse meg a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="23a9f-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="23a9f-114">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="23a9f-114">**Azure CLI**.</span></span> <span data-ttu-id="23a9f-115">Ha még nem telepítette az Azure parancssori felület hello, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) a további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="23a9f-115">If you have not installed hello Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-hello-flight-data"></a><span data-ttu-id="23a9f-116">Hello felé továbbított adatok letöltése</span><span class="sxs-lookup"><span data-stu-id="23a9f-116">Download hello flight data</span></span>

1. <span data-ttu-id="23a9f-117">Keresse meg a túl[kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda][rita-website].</span><span class="sxs-lookup"><span data-stu-id="23a9f-117">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="23a9f-118">Hello lapon jelölje be a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-118">On hello page, select hello following values:</span></span>

   | <span data-ttu-id="23a9f-119">Név</span><span class="sxs-lookup"><span data-stu-id="23a9f-119">Name</span></span> | <span data-ttu-id="23a9f-120">Érték</span><span class="sxs-lookup"><span data-stu-id="23a9f-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="23a9f-121">Szűrő év</span><span class="sxs-lookup"><span data-stu-id="23a9f-121">Filter Year</span></span> |<span data-ttu-id="23a9f-122">2013</span><span class="sxs-lookup"><span data-stu-id="23a9f-122">2013</span></span> |
   | <span data-ttu-id="23a9f-123">Időszak szűrése</span><span class="sxs-lookup"><span data-stu-id="23a9f-123">Filter Period</span></span> |<span data-ttu-id="23a9f-124">Január</span><span class="sxs-lookup"><span data-stu-id="23a9f-124">January</span></span> |
   | <span data-ttu-id="23a9f-125">Mezők</span><span class="sxs-lookup"><span data-stu-id="23a9f-125">Fields</span></span> |<span data-ttu-id="23a9f-126">Év FlightDate, UniqueCarrier, vivőjel, FlightNum, OriginAirportID, eredet, OriginCityName, OriginState, DestAirportID, cél, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="23a9f-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="23a9f-127">Törölje az összes többi mező</span><span class="sxs-lookup"><span data-stu-id="23a9f-127">Clear all other fields</span></span> |

3. <span data-ttu-id="23a9f-128">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="23a9f-128">Click **Download**.</span></span>

## <a name="upload-hello-data"></a><span data-ttu-id="23a9f-129">Hello adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="23a9f-129">Upload hello data</span></span>

1. <span data-ttu-id="23a9f-130">A következő parancs tooupload hello zip fájlt toohello HDInsight fürt átjárócsomópontjába hello használata:</span><span class="sxs-lookup"><span data-stu-id="23a9f-130">Use hello following command tooupload hello zip file toohello HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="23a9f-131">Cserélje le **Fájlnév** hello nevű hello zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="23a9f-131">Replace **FILENAME** with hello name of hello zip file.</span></span> <span data-ttu-id="23a9f-132">Cserélje le **felhasználónév** hello SSH-bejelentkezéskor hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="23a9f-132">Replace **USERNAME** with hello SSH login for hello HDInsight cluster.</span></span> <span data-ttu-id="23a9f-133">Cserélje le a FÜRTNÉV hello nevű hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="23a9f-133">Replace CLUSTERNAME with hello name of hello HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="23a9f-134">A jelszó tooauthenticate használatakor az SSH-bejelentkezéskor kéri hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="23a9f-134">If you use a password tooauthenticate your SSH login, you are prompted for hello password.</span></span> <span data-ttu-id="23a9f-135">Ha a használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter, és adja meg a hello elérési toohello titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="23a9f-135">If you used a public key, you may need toouse hello `-i` parameter and specify hello path toohello matching private key.</span></span> <span data-ttu-id="23a9f-136">Például: `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="23a9f-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="23a9f-137">Hello feltöltés befejezése után csatlakoztassa az SSH használatával toohello fürtöt:</span><span class="sxs-lookup"><span data-stu-id="23a9f-137">Once hello upload has completed, connect toohello cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="23a9f-138">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="23a9f-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="23a9f-139">Miután csatlakozott, használja a következő toounzip hello .zip fájl hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-139">Once connected, use hello following toounzip hello .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="23a9f-140">Ez a parancs egy CSV-fájlt, amely körülbelül 60 MB bontja ki.</span><span class="sxs-lookup"><span data-stu-id="23a9f-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="23a9f-141">HDInsight tároló parancs toocreate könyvtár következő hello használja, és másolja hello toohello könyvtára:</span><span class="sxs-lookup"><span data-stu-id="23a9f-141">Use hello following command toocreate a directory on HDInsight storage, and then copy hello file toohello directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a><span data-ttu-id="23a9f-142">Hozzon létre és futtasson hello HiveQL</span><span class="sxs-lookup"><span data-stu-id="23a9f-142">Create and run hello HiveQL</span></span>

<span data-ttu-id="23a9f-143">Az elnevezett Hive táblát használja hello alábbi lépéseit az hello CSV-fájlból tooimport adatok **késések**.</span><span class="sxs-lookup"><span data-stu-id="23a9f-143">Use hello following steps tooimport data from hello CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="23a9f-144">Használjon hello következő toocreate parancsot, és egy új fájlt szerkesztése **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="23a9f-144">Use hello following command toocreate and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="23a9f-145">Szöveg hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="23a9f-145">Use hello following text as hello contents of this file:</span></span>

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over hello csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- hello following lines describe hello format and location of hello file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop hello delays table if it exists
    DROP TABLE delays;
    -- Create hello delays table and populate it with data
    -- pulled in from hello CSV file (via hello external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. <span data-ttu-id="23a9f-146">toosave hello fájl használata **Ctrl + X**, majd **Y** .</span><span class="sxs-lookup"><span data-stu-id="23a9f-146">toosave hello file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="23a9f-147">toostart struktúra és futtatási hello **flightdelays.hql** fájlt, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="23a9f-147">toostart Hive and run hello **flightdelays.hql** file, use hello following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="23a9f-148">Ebben a példában `localhost` szolgál, mivel Ön hello HDInsight-fürt, amely hiveserver2-n futtató csatlakoztatott toohello átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="23a9f-148">In this example, `localhost` is used since you are connected toohello head node of hello HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="23a9f-149">Egyszer hello __flightdelays.hql__ parancsfájlt futtató befejeződik, használja hello következő parancsot egy interaktív Beeline munkamenet tooopen:</span><span class="sxs-lookup"><span data-stu-id="23a9f-149">Once hello __flightdelays.hql__ script finishes running, use hello following command tooopen an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="23a9f-150">Amikor megjelenik a hello `jdbc:hive2://localhost:10001/>` kérni, használja a következő tooretrieve adatait importált hello repülési késleltetés adatokból hello.</span><span class="sxs-lookup"><span data-stu-id="23a9f-150">When you receive hello `jdbc:hive2://localhost:10001/>` prompt, use hello following query tooretrieve data from hello imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="23a9f-151">Ez a lekérdezés lekéri a város, hogy a tapasztalt időjárási késéseket, valamint hello átlagos idő késleltetés, és mentse túl listáját`/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="23a9f-151">This query retrieves a list of cities that experienced weather delays, along with hello average delay time, and save it too`/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="23a9f-152">Később Sqoop hello adatokat olvas a következő helyről, és exportálni onnan tooAzure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="23a9f-152">Later, Sqoop reads hello data from this location and export it tooAzure SQL Database.</span></span>

6. <span data-ttu-id="23a9f-153">Adja meg a tooexit Beeline, `!quit` hello a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="23a9f-153">tooexit Beeline, enter `!quit` at hello prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="23a9f-154">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="23a9f-154">Create a SQL Database</span></span>

<span data-ttu-id="23a9f-155">Ha már rendelkezik egy SQL-adatbázis, ha előbb telepítik azokra a hello kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="23a9f-155">If you already have a SQL Database, you must get hello server name.</span></span> <span data-ttu-id="23a9f-156">Hello kiszolgálónév hello található [Azure-portálon](https://portal.azure.com) kiválasztásával **SQL-adatbázisok**, majd szűrést hello hello nevét az adatbázis-, és toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="23a9f-156">You can find hello server name in hello [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on hello name of hello database you wish toouse.</span></span> <span data-ttu-id="23a9f-157">hello kiszolgálónév szerepel hello **SERVER** oszlop.</span><span class="sxs-lookup"><span data-stu-id="23a9f-157">hello server name is listed in hello **SERVER** column.</span></span>

<span data-ttu-id="23a9f-158">Ha még nem rendelkezik SQL-adatbázis, használja a hello adatokat [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md) toocreate egy.</span><span class="sxs-lookup"><span data-stu-id="23a9f-158">If you do not already have a SQL Database, use hello information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) toocreate one.</span></span> <span data-ttu-id="23a9f-159">Mentse a hello adatbázis hello kiszolgálójának nevét.</span><span class="sxs-lookup"><span data-stu-id="23a9f-159">Save hello server name used for hello database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="23a9f-160">SQL-adatbázis tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="23a9f-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="23a9f-161">Számos módon tooconnect tooSQL adatbázisban van, és hozzon létre egy táblát.</span><span class="sxs-lookup"><span data-stu-id="23a9f-161">There are many ways tooconnect tooSQL Database and create a table.</span></span> <span data-ttu-id="23a9f-162">a következő lépéseket használata hello [FreeTDS](http://www.freetds.org/) hello HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="23a9f-162">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="23a9f-163">SSH tooconnect toohello Linux-alapú HDInsight-fürtöt, és hello SSH-munkamenetből a lépéseket követve futtatási hello használata.</span><span class="sxs-lookup"><span data-stu-id="23a9f-163">Use SSH tooconnect toohello Linux-based HDInsight cluster, and run hello following steps from hello SSH session.</span></span>

2. <span data-ttu-id="23a9f-164">A következő parancs tooinstall FreeTDS hello használata:</span><span class="sxs-lookup"><span data-stu-id="23a9f-164">Use hello following command tooinstall FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="23a9f-165">Hello telepítés befejezése után használja a következő parancs tooconnect toohello SQL adatbázis-kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="23a9f-165">Once hello install completes, use hello following command tooconnect toohello SQL Database server.</span></span> <span data-ttu-id="23a9f-166">Cserélje le **kiszolgálónév** hello SQL adatbázis-kiszolgáló nevével.</span><span class="sxs-lookup"><span data-stu-id="23a9f-166">Replace **serverName** with hello SQL Database server name.</span></span> <span data-ttu-id="23a9f-167">Cserélje le **adminLogin** és **adminPassword** hello bejelentkezéskor SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="23a9f-167">Replace **adminLogin** and **adminPassword** with hello login for SQL Database.</span></span> <span data-ttu-id="23a9f-168">Cserélje le **databaseName** hello adatbázisnévvel.</span><span class="sxs-lookup"><span data-stu-id="23a9f-168">Replace **databaseName** with hello database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="23a9f-169">Kimeneti hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="23a9f-169">You receive output similar toohello following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. <span data-ttu-id="23a9f-170">A hello `1>` kéri, adja meg az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-170">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="23a9f-171">Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="23a9f-171">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="23a9f-172">Ez a lekérdezés egy nevű táblát hoz létre **késések**, a fürtözött index.</span><span class="sxs-lookup"><span data-stu-id="23a9f-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="23a9f-173">A következő lekérdezés tooverify, amelyek a tábla hello használata hello jött létre:</span><span class="sxs-lookup"><span data-stu-id="23a9f-173">Use hello following query tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="23a9f-174">a kimeneti hello hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="23a9f-174">hello output is similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="23a9f-175">Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.</span><span class="sxs-lookup"><span data-stu-id="23a9f-175">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="23a9f-176">A Sqoop adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="23a9f-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="23a9f-177">A következő parancs tooverify, hogy a Sqoop látja-e az SQL-adatbázis hello használata:</span><span class="sxs-lookup"><span data-stu-id="23a9f-177">Use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="23a9f-178">Ez a parancs adatbázisok, többek között a hello adatbázis hello késések tábla a korábban létrehozott listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="23a9f-178">This command returns a list of databases, including hello database that you created hello delays table in earlier.</span></span>

2. <span data-ttu-id="23a9f-179">Használjon hello következő parancsot a hivesampletable toohello mobiledata tábla tooexport adatait:</span><span class="sxs-lookup"><span data-stu-id="23a9f-179">Use hello following command tooexport data from hivesampletable toohello mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="23a9f-180">Sqoop hello késések tábla tartalmazó toohello adatbázis csatlakozik, valamint adatokat exportál hello `/tutorials/flightdelays/output` könyvtár toohello késések táblában.</span><span class="sxs-lookup"><span data-stu-id="23a9f-180">Sqoop connects toohello database containing hello delays table, and exports data from hello `/tutorials/flightdelays/output` directory toohello delays table.</span></span>

3. <span data-ttu-id="23a9f-181">Hello parancs után használja a következő tooconnect toohello adatbázis TSQL használatával hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-181">After hello command completes, use hello following tooconnect toohello database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="23a9f-182">A csatlakozás után használja a következő, hogy hello adatok volt-e az exportált toohello mobiledata table utasítások tooverify hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-182">Once connected, use hello following statements tooverify that hello data was exported toohello mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="23a9f-183">Meg kell jelennie hello tábla listáját.</span><span class="sxs-lookup"><span data-stu-id="23a9f-183">You should see a listing of data in hello table.</span></span> <span data-ttu-id="23a9f-184">Típus `exit` tooexit hello tsql segédprogram.</span><span class="sxs-lookup"><span data-stu-id="23a9f-184">Type `exit` tooexit hello tsql utility.</span></span>

## <span data-ttu-id="23a9f-185"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23a9f-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="23a9f-186">toolearn további módszereket toowork adatokkal a Hdinsightban, lásd a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="23a9f-186">toolearn more ways toowork with data in HDInsight, see hello following documents:</span></span>

* <span data-ttu-id="23a9f-187">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="23a9f-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="23a9f-188">[Oozie használata a hdinsight eszközzel][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="23a9f-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="23a9f-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="23a9f-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="23a9f-190">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="23a9f-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="23a9f-191">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="23a9f-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="23a9f-192">[Python Hadoop-Stream programok HDInsight a fejlesztéséhez][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="23a9f-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
