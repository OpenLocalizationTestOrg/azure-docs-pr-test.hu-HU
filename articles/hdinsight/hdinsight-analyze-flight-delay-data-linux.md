---
title: "A HDInsight - Azure Hive repülési késleltetés adatok elemzése |} Microsoft Docs"
description: "Útmutató a Hive használata a Linux-alapú HDInsight felé továbbított adatok elemzésére, majd exportálja az adatokat az SQL Database Sqoop használatával."
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
ms.openlocfilehash: 8cdc19ac8a517b6d8eefabb5476a686aa252a332
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a><span data-ttu-id="ffb03-103">A Linux-alapú HDInsight Hive használatával repülési késleltetés adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="ffb03-103">Analyze flight delay data by using Hive on Linux-based HDInsight</span></span>

<span data-ttu-id="ffb03-104">Útmutató a Hive használata a Linux-alapú HDInsight repülési késleltetés adatok elemzésére, majd exportálja az adatokat az Azure SQL Database Sqoop használatával.</span><span class="sxs-lookup"><span data-stu-id="ffb03-104">Learn how to analyze flight delay data using Hive on Linux-based HDInsight then export the data to Azure SQL Database using Sqoop.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ffb03-105">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="ffb03-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="ffb03-106">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="ffb03-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ffb03-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ffb03-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="prerequisites"></a><span data-ttu-id="ffb03-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ffb03-108">Prerequisites</span></span>

* <span data-ttu-id="ffb03-109">**HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="ffb03-109">**An HDInsight cluster**.</span></span> <span data-ttu-id="ffb03-110">Lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md) vonatkozó lépéseket egy új Linux-alapú HDInsight-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ffb03-110">See [Get started using Hadoop with Hive in HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) for steps on creating a new Linux-based HDInsight cluster.</span></span>

* <span data-ttu-id="ffb03-111">**Az Azure SQL adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="ffb03-111">**Azure SQL Database**.</span></span> <span data-ttu-id="ffb03-112">Azure SQL-adatbázis használata a cél-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="ffb03-112">You use an Azure SQL database as a destination data store.</span></span> <span data-ttu-id="ffb03-113">Ha egy SQL-adatbázis már nem rendelkezik, tekintse meg a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ffb03-113">If you do not have a SQL Database already, see [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span>

* <span data-ttu-id="ffb03-114">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="ffb03-114">**Azure CLI**.</span></span> <span data-ttu-id="ffb03-115">Ha még nem telepítette az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md) a további lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ffb03-115">If you have not installed the Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) for more steps.</span></span>

## <a name="download-the-flight-data"></a><span data-ttu-id="ffb03-116">A felé továbbított adatok letöltése</span><span class="sxs-lookup"><span data-stu-id="ffb03-116">Download the flight data</span></span>

1. <span data-ttu-id="ffb03-117">Keresse meg a [kutatási és innovatív technológia felügyeleti, iroda szállítására statisztikák][rita-website].</span><span class="sxs-lookup"><span data-stu-id="ffb03-117">Browse to [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>

2. <span data-ttu-id="ffb03-118">A lapon válassza ki a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="ffb03-118">On the page, select the following values:</span></span>

   | <span data-ttu-id="ffb03-119">Név</span><span class="sxs-lookup"><span data-stu-id="ffb03-119">Name</span></span> | <span data-ttu-id="ffb03-120">Érték</span><span class="sxs-lookup"><span data-stu-id="ffb03-120">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="ffb03-121">Szűrő év</span><span class="sxs-lookup"><span data-stu-id="ffb03-121">Filter Year</span></span> |<span data-ttu-id="ffb03-122">2013</span><span class="sxs-lookup"><span data-stu-id="ffb03-122">2013</span></span> |
   | <span data-ttu-id="ffb03-123">Időszak szűrése</span><span class="sxs-lookup"><span data-stu-id="ffb03-123">Filter Period</span></span> |<span data-ttu-id="ffb03-124">Január</span><span class="sxs-lookup"><span data-stu-id="ffb03-124">January</span></span> |
   | <span data-ttu-id="ffb03-125">Mezők</span><span class="sxs-lookup"><span data-stu-id="ffb03-125">Fields</span></span> |<span data-ttu-id="ffb03-126">Év FlightDate, UniqueCarrier, vivőjel, FlightNum, OriginAirportID, eredet, OriginCityName, OriginState, DestAirportID, cél, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span><span class="sxs-lookup"><span data-stu-id="ffb03-126">Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay.</span></span> <span data-ttu-id="ffb03-127">Törölje az összes többi mező</span><span class="sxs-lookup"><span data-stu-id="ffb03-127">Clear all other fields</span></span> |

3. <span data-ttu-id="ffb03-128">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ffb03-128">Click **Download**.</span></span>

## <a name="upload-the-data"></a><span data-ttu-id="ffb03-129">Az adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="ffb03-129">Upload the data</span></span>

1. <span data-ttu-id="ffb03-130">Az alábbi parancs segítségével a zip-fájl feltöltése a HDInsight fürt átjárócsomópontjába:</span><span class="sxs-lookup"><span data-stu-id="ffb03-130">Use the following command to upload the zip file to the HDInsight cluster head node:</span></span>

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    <span data-ttu-id="ffb03-131">Cserélje le **Fájlnév** a zip-fájl nevével.</span><span class="sxs-lookup"><span data-stu-id="ffb03-131">Replace **FILENAME** with the name of the zip file.</span></span> <span data-ttu-id="ffb03-132">Cserélje le **felhasználónév** a HDInsight-fürthöz az SSH-bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="ffb03-132">Replace **USERNAME** with the SSH login for the HDInsight cluster.</span></span> <span data-ttu-id="ffb03-133">CLUSTERNAME cserélje le a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="ffb03-133">Replace CLUSTERNAME with the name of the HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ffb03-134">Ha a jelszó segítségével hitelesíti az SSH-bejelentkezéskor, kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="ffb03-134">If you use a password to authenticate your SSH login, you are prompted for the password.</span></span> <span data-ttu-id="ffb03-135">A nyilvános kulcs használatakor esetleg használja a `-i` paraméter és a kapcsolódó titkos kulcs elérési útja.</span><span class="sxs-lookup"><span data-stu-id="ffb03-135">If you used a public key, you may need to use the `-i` parameter and specify the path to the matching private key.</span></span> <span data-ttu-id="ffb03-136">Például: `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span><span class="sxs-lookup"><span data-stu-id="ffb03-136">For example, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.</span></span>

2. <span data-ttu-id="ffb03-137">Miután befejezte a feltöltést, csatlakozzon a fürthöz SSH használatával:</span><span class="sxs-lookup"><span data-stu-id="ffb03-137">Once the upload has completed, connect to the cluster using SSH:</span></span>

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    <span data-ttu-id="ffb03-138">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ffb03-138">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="ffb03-139">Miután csatlakozott, bontsa ki a .zip fájlt használja a következő:</span><span class="sxs-lookup"><span data-stu-id="ffb03-139">Once connected, use the following to unzip the .zip file:</span></span>

    ```
    unzip FILENAME.zip
    ```

    <span data-ttu-id="ffb03-140">Ez a parancs egy CSV-fájlt, amely körülbelül 60 MB bontja ki.</span><span class="sxs-lookup"><span data-stu-id="ffb03-140">This command extracts a .csv file that is roughly 60 MB.</span></span>

4. <span data-ttu-id="ffb03-141">Az alábbi parancs segítségével hozzon létre egy könyvtárat a HDInsight-tárolóba, majd másolja a fájlt a könyvtárba:</span><span class="sxs-lookup"><span data-stu-id="ffb03-141">Use the following command to create a directory on HDInsight storage, and then copy the file to the directory:</span></span>

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a><span data-ttu-id="ffb03-142">Hozzon létre, és a HiveQL futtatása</span><span class="sxs-lookup"><span data-stu-id="ffb03-142">Create and run the HiveQL</span></span>

<span data-ttu-id="ffb03-143">Az alábbi lépések segítségével adatokat importálhat a CSV-fájl nevű Hive tábla **késések**.</span><span class="sxs-lookup"><span data-stu-id="ffb03-143">Use the following steps to import data from the CSV file into a Hive table named **Delays**.</span></span>

1. <span data-ttu-id="ffb03-144">A következő paranccsal hozhat létre és szerkeszthet egy új fájlt **flightdelays.hql**:</span><span class="sxs-lookup"><span data-stu-id="ffb03-144">Use the following command to create and edit a new file named **flightdelays.hql**:</span></span>

    ```
    nano flightdelays.hql
    ```

    <span data-ttu-id="ffb03-145">Ez a fájl tartalmát a következő szöveg használata:</span><span class="sxs-lookup"><span data-stu-id="ffb03-145">Use the following text as the contents of this file:</span></span>

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
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
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
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

2. <span data-ttu-id="ffb03-146">Mentse a fájlt, használja a **Ctrl + X**, majd **Y** .</span><span class="sxs-lookup"><span data-stu-id="ffb03-146">To save the file, use **Ctrl + X**, then **Y** .</span></span>

3. <span data-ttu-id="ffb03-147">Hive elindításához és a **flightdelays.hql** fájlt, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="ffb03-147">To start Hive and run the **flightdelays.hql** file, use the following command:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > <span data-ttu-id="ffb03-148">Ebben a példában `localhost` szolgál, mert a HDInsight-fürt, amely hiveserver2-n futtató átjárócsomópontjához csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="ffb03-148">In this example, `localhost` is used since you are connected to the head node of the HDInsight cluster, which is where HiveServer2 is running.</span></span>

4. <span data-ttu-id="ffb03-149">Egyszer a __flightdelays.hql__ parancsfájlt futtató befejeződik, nyisson meg egy interaktív Beeline-munkamenetet a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="ffb03-149">Once the __flightdelays.hql__ script finishes running, use the following command to open an interactive Beeline session:</span></span>

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. <span data-ttu-id="ffb03-150">Amikor megjelenik a `jdbc:hive2://localhost:10001/>` kérni, használja a következő lekérdezést adatok lekérése az importált repülési késleltetés adatokat.</span><span class="sxs-lookup"><span data-stu-id="ffb03-150">When you receive the `jdbc:hive2://localhost:10001/>` prompt, use the following query to retrieve data from the imported flight delay data.</span></span>

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    <span data-ttu-id="ffb03-151">Ez a lekérdezés lekéri a várost, időjárási késést, az átlagos idő együtt, és mentse azt észlelt listáját `/tutorials/flightdelays/output`.</span><span class="sxs-lookup"><span data-stu-id="ffb03-151">This query retrieves a list of cities that experienced weather delays, along with the average delay time, and save it to `/tutorials/flightdelays/output`.</span></span> <span data-ttu-id="ffb03-152">Később a Sqoop beolvassa az adatokat a következő helyről, és exportálni onnan az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ffb03-152">Later, Sqoop reads the data from this location and export it to Azure SQL Database.</span></span>

6. <span data-ttu-id="ffb03-153">Lépjen ki a Beeline, írja be a következőt `!quit` a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="ffb03-153">To exit Beeline, enter `!quit` at the prompt.</span></span>

## <a name="create-a-sql-database"></a><span data-ttu-id="ffb03-154">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffb03-154">Create a SQL Database</span></span>

<span data-ttu-id="ffb03-155">Ha már rendelkezik egy SQL-adatbázis, ha előbb telepítik azokra a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="ffb03-155">If you already have a SQL Database, you must get the server name.</span></span> <span data-ttu-id="ffb03-156">Megtalálhatja a kiszolgáló nevére a [Azure-portálon](https://portal.azure.com) kiválasztásával **SQL-adatbázisok**, és majd szűrését a használni kívánt adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="ffb03-156">You can find the server name in the [Azure portal](https://portal.azure.com) by selecting **SQL Databases**, and then filtering on the name of the database you wish to use.</span></span> <span data-ttu-id="ffb03-157">A kiszolgálónév szerepel a **SERVER** oszlop.</span><span class="sxs-lookup"><span data-stu-id="ffb03-157">The server name is listed in the **SERVER** column.</span></span>

<span data-ttu-id="ffb03-158">Ha még nem rendelkezik SQL-adatbázis, olvassa el a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md) kattintva létrehozhat egyet.</span><span class="sxs-lookup"><span data-stu-id="ffb03-158">If you do not already have a SQL Database, use the information in [SQL Database tutorial: Create a SQL database in minutes](../sql-database/sql-database-get-started.md) to create one.</span></span> <span data-ttu-id="ffb03-159">Mentse a kiszolgáló nevét az adatbázis használatos.</span><span class="sxs-lookup"><span data-stu-id="ffb03-159">Save the server name used for the database.</span></span>

## <a name="create-a-sql-database-table"></a><span data-ttu-id="ffb03-160">SQL-adatbázis tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="ffb03-160">Create a SQL Database table</span></span>

> [!NOTE]
> <span data-ttu-id="ffb03-161">Számos módon csatlakozzon az SQL Database, és hozzon létre egy táblát.</span><span class="sxs-lookup"><span data-stu-id="ffb03-161">There are many ways to connect to SQL Database and create a table.</span></span> <span data-ttu-id="ffb03-162">Az alábbi lépéseket használata [FreeTDS](http://www.freetds.org/) a a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ffb03-162">The following steps use [FreeTDS](http://www.freetds.org/) from the HDInsight cluster.</span></span>


1. <span data-ttu-id="ffb03-163">Az SSH használata a Linux-alapú HDInsight-fürthöz való csatlakozáshoz, és futtassa a következő lépéseket az SSH-munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="ffb03-163">Use SSH to connect to the Linux-based HDInsight cluster, and run the following steps from the SSH session.</span></span>

2. <span data-ttu-id="ffb03-164">A következő paranccsal telepítse FreeTDS:</span><span class="sxs-lookup"><span data-stu-id="ffb03-164">Use the following command to install FreeTDS:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. <span data-ttu-id="ffb03-165">A telepítés befejezése után a következő paranccsal az SQL Database-kiszolgálóhoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="ffb03-165">Once the install completes, use the following command to connect to the SQL Database server.</span></span> <span data-ttu-id="ffb03-166">Cserélje le **kiszolgálónév** az SQL-adatbázis-kiszolgáló nevével.</span><span class="sxs-lookup"><span data-stu-id="ffb03-166">Replace **serverName** with the SQL Database server name.</span></span> <span data-ttu-id="ffb03-167">Cserélje le **adminLogin** és **adminPassword** SQL-adatbázis a bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="ffb03-167">Replace **adminLogin** and **adminPassword** with the login for SQL Database.</span></span> <span data-ttu-id="ffb03-168">Cserélje le **databaseName** az adatbázis nevével.</span><span class="sxs-lookup"><span data-stu-id="ffb03-168">Replace **databaseName** with the database name.</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="ffb03-169">A kimenet az alábbihoz hasonló jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="ffb03-169">You receive output similar to the following text:</span></span>

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. <span data-ttu-id="ffb03-170">: A `1>` kéri, adja meg a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="ffb03-170">At the `1>` prompt, enter the following lines:</span></span>

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    <span data-ttu-id="ffb03-171">Ha a `GO` utasításban is meg kell adni, az előző utasítások kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="ffb03-171">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="ffb03-172">Ez a lekérdezés egy nevű táblát hoz létre **késések**, a fürtözött index.</span><span class="sxs-lookup"><span data-stu-id="ffb03-172">This query creates a table named **delays**, with a clustered index.</span></span>

    <span data-ttu-id="ffb03-173">A következő lekérdezés segítségével győződjön meg arról, hogy a tábla jött létre:</span><span class="sxs-lookup"><span data-stu-id="ffb03-173">Use the following query to verify that the table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="ffb03-174">A kimenet az alábbi szöveghez hasonló:</span><span class="sxs-lookup"><span data-stu-id="ffb03-174">The output is similar to the following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. <span data-ttu-id="ffb03-175">Adja meg `exit` , a `1>` Rákérdezés a tsql segédprogram kilép.</span><span class="sxs-lookup"><span data-stu-id="ffb03-175">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="export-data-with-sqoop"></a><span data-ttu-id="ffb03-176">A Sqoop adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="ffb03-176">Export data with Sqoop</span></span>

1. <span data-ttu-id="ffb03-177">A következő parancs használatával győződjön meg arról, hogy a Sqoop látja-e az SQL-adatbázis:</span><span class="sxs-lookup"><span data-stu-id="ffb03-177">Use the following command to verify that Sqoop can see your SQL Database:</span></span>

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    <span data-ttu-id="ffb03-178">Ez a parancs adatbázisok, beleértve a késést táblájának korábban létrehozott adatbázis listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ffb03-178">This command returns a list of databases, including the database that you created the delays table in earlier.</span></span>

2. <span data-ttu-id="ffb03-179">Az alábbi parancs segítségével exportál adatokat az hivesampletable a mobiledata tábla:</span><span class="sxs-lookup"><span data-stu-id="ffb03-179">Use the following command to export data from hivesampletable to the mobiledata table:</span></span>

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="ffb03-180">Sqoop a késést táblát tartalmazó adatbázishoz kapcsolódik, és adatokat exportál a `/tutorials/flightdelays/output` könyvtár késések táblához.</span><span class="sxs-lookup"><span data-stu-id="ffb03-180">Sqoop connects to the database containing the delays table, and exports data from the `/tutorials/flightdelays/output` directory to the delays table.</span></span>

3. <span data-ttu-id="ffb03-181">A parancs után használja a TSQL használatával adatbázishoz való kapcsolódáshoz a következőket:</span><span class="sxs-lookup"><span data-stu-id="ffb03-181">After the command completes, use the following to connect to the database using TSQL:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    <span data-ttu-id="ffb03-182">A csatlakozás után a következő utasítások használatával győződjön meg arról, hogy az adatok mobiledata táblához exportált:</span><span class="sxs-lookup"><span data-stu-id="ffb03-182">Once connected, use the following statements to verify that the data was exported to the mobiledata table:</span></span>

    ```
    SELECT * FROM delays
    GO
    ```

    <span data-ttu-id="ffb03-183">Meg kell jelennie a tábla adatainak listáját.</span><span class="sxs-lookup"><span data-stu-id="ffb03-183">You should see a listing of data in the table.</span></span> <span data-ttu-id="ffb03-184">Típus `exit` való kilépéshez a tsql segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="ffb03-184">Type `exit` to exit the tsql utility.</span></span>

## <span data-ttu-id="ffb03-185"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ffb03-185"><a id="nextsteps"></a> Next steps</span></span>

<span data-ttu-id="ffb03-186">További részleteket a hdinsight adatokkal dolgozni, lásd: a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="ffb03-186">To learn more ways to work with data in HDInsight, see the following documents:</span></span>

* <span data-ttu-id="ffb03-187">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="ffb03-187">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="ffb03-188">[Oozie használata a hdinsight eszközzel][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="ffb03-188">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="ffb03-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="ffb03-189">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="ffb03-190">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="ffb03-190">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="ffb03-191">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="ffb03-191">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>
* <span data-ttu-id="ffb03-192">[Python Hadoop-Stream programok HDInsight a fejlesztéséhez][hdinsight-develop-streaming]</span><span class="sxs-lookup"><span data-stu-id="ffb03-192">[Develop Python Hadoop streaming programs for HDInsight][hdinsight-develop-streaming]</span></span>

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
