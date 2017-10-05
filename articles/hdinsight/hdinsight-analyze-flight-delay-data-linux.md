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
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>A Linux-alapú HDInsight Hive használatával repülési késleltetés adatok elemzése

Útmutató a Hive használata a Linux-alapú HDInsight repülési késleltetés adatok elemzésére, majd exportálja az adatokat az Azure SQL Database Sqoop használatával.

> [!IMPORTANT]
> A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="prerequisites"></a>Előfeltételek

* **HDInsight-fürtök**. Lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md) vonatkozó lépéseket egy új Linux-alapú HDInsight-fürtöt hoz létre.

* **Az Azure SQL adatbázis**. Azure SQL-adatbázis használata a cél-tárolóban. Ha egy SQL-adatbázis már nem rendelkezik, tekintse meg a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).

* **Azure parancssori felület (CLI)**. Ha még nem telepítette az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md) a további lépéseket.

## <a name="download-the-flight-data"></a>A felé továbbított adatok letöltése

1. Keresse meg a [kutatási és innovatív technológia felügyeleti, iroda szállítására statisztikák][rita-website].

2. A lapon válassza ki a következő értékeket:

   | Név | Érték |
   | --- | --- |
   | Szűrő év |2013 |
   | Időszak szűrése |Január |
   | Mezők |Év FlightDate, UniqueCarrier, vivőjel, FlightNum, OriginAirportID, eredet, OriginCityName, OriginState, DestAirportID, cél, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Törölje az összes többi mező |

3. Kattintson a **Letöltés** gombra.

## <a name="upload-the-data"></a>Az adatok feltöltése

1. Az alábbi parancs segítségével a zip-fájl feltöltése a HDInsight fürt átjárócsomópontjába:

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    Cserélje le **Fájlnév** a zip-fájl nevével. Cserélje le **felhasználónév** a HDInsight-fürthöz az SSH-bejelentkezéskor. CLUSTERNAME cserélje le a HDInsight-fürt nevére.

   > [!NOTE]
   > Ha a jelszó segítségével hitelesíti az SSH-bejelentkezéskor, kéri a jelszót. A nyilvános kulcs használatakor esetleg használja a `-i` paraméter és a kapcsolódó titkos kulcs elérési útja. Például: `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Miután befejezte a feltöltést, csatlakozzon a fürthöz SSH használatával:

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Miután csatlakozott, bontsa ki a .zip fájlt használja a következő:

    ```
    unzip FILENAME.zip
    ```

    Ez a parancs egy CSV-fájlt, amely körülbelül 60 MB bontja ki.

4. Az alábbi parancs segítségével hozzon létre egy könyvtárat a HDInsight-tárolóba, majd másolja a fájlt a könyvtárba:

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a>Hozzon létre, és a HiveQL futtatása

Az alábbi lépések segítségével adatokat importálhat a CSV-fájl nevű Hive tábla **késések**.

1. A következő paranccsal hozhat létre és szerkeszthet egy új fájlt **flightdelays.hql**:

    ```
    nano flightdelays.hql
    ```

    Ez a fájl tartalmát a következő szöveg használata:

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

2. Mentse a fájlt, használja a **Ctrl + X**, majd **Y** .

3. Hive elindításához és a **flightdelays.hql** fájlt, a következő paranccsal:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > Ebben a példában `localhost` szolgál, mert a HDInsight-fürt, amely hiveserver2-n futtató átjárócsomópontjához csatlakozik.

4. Egyszer a __flightdelays.hql__ parancsfájlt futtató befejeződik, nyisson meg egy interaktív Beeline-munkamenetet a következő paranccsal:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Amikor megjelenik a `jdbc:hive2://localhost:10001/>` kérni, használja a következő lekérdezést adatok lekérése az importált repülési késleltetés adatokat.

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Ez a lekérdezés lekéri a várost, időjárási késést, az átlagos idő együtt, és mentse azt észlelt listáját `/tutorials/flightdelays/output`. Később a Sqoop beolvassa az adatokat a következő helyről, és exportálni onnan az Azure SQL Database.

6. Lépjen ki a Beeline, írja be a következőt `!quit` a parancssorba.

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

Ha már rendelkezik egy SQL-adatbázis, ha előbb telepítik azokra a kiszolgáló nevét. Megtalálhatja a kiszolgáló nevére a [Azure-portálon](https://portal.azure.com) kiválasztásával **SQL-adatbázisok**, és majd szűrését a használni kívánt adatbázis nevét. A kiszolgálónév szerepel a **SERVER** oszlop.

Ha még nem rendelkezik SQL-adatbázis, olvassa el a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md) kattintva létrehozhat egyet. Mentse a kiszolgáló nevét az adatbázis használatos.

## <a name="create-a-sql-database-table"></a>SQL-adatbázis tábla létrehozása

> [!NOTE]
> Számos módon csatlakozzon az SQL Database, és hozzon létre egy táblát. Az alábbi lépéseket használata [FreeTDS](http://www.freetds.org/) a a HDInsight-fürthöz.


1. Az SSH használata a Linux-alapú HDInsight-fürthöz való csatlakozáshoz, és futtassa a következő lépéseket az SSH-munkamenetből.

2. A következő paranccsal telepítse FreeTDS:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. A telepítés befejezése után a következő paranccsal az SQL Database-kiszolgálóhoz való kapcsolódáshoz. Cserélje le **kiszolgálónév** az SQL-adatbázis-kiszolgáló nevével. Cserélje le **adminLogin** és **adminPassword** SQL-adatbázis a bejelentkezéskor. Cserélje le **databaseName** az adatbázis nevével.

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    A kimenet az alábbihoz hasonló jelenhet meg:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. : A `1>` kéri, adja meg a következő sorokat:

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Ha a `GO` utasításban is meg kell adni, az előző utasítások kiértékelése. Ez a lekérdezés egy nevű táblát hoz létre **késések**, a fürtözött index.

    A következő lekérdezés segítségével győződjön meg arról, hogy a tábla jött létre:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    A kimenet az alábbi szöveghez hasonló:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Adja meg `exit` , a `1>` Rákérdezés a tsql segédprogram kilép.

## <a name="export-data-with-sqoop"></a>A Sqoop adatok exportálása

1. A következő parancs használatával győződjön meg arról, hogy a Sqoop látja-e az SQL-adatbázis:

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Ez a parancs adatbázisok, beleértve a késést táblájának korábban létrehozott adatbázis listáját adja vissza.

2. Az alábbi parancs segítségével exportál adatokat az hivesampletable a mobiledata tábla:

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop a késést táblát tartalmazó adatbázishoz kapcsolódik, és adatokat exportál a `/tutorials/flightdelays/output` könyvtár késések táblához.

3. A parancs után használja a TSQL használatával adatbázishoz való kapcsolódáshoz a következőket:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    A csatlakozás után a következő utasítások használatával győződjön meg arról, hogy az adatok mobiledata táblához exportált:

    ```
    SELECT * FROM delays
    GO
    ```

    Meg kell jelennie a tábla adatainak listáját. Típus `exit` való kilépéshez a tsql segédprogramot.

## <a id="nextsteps"></a> Következő lépések

További részleteket a hdinsight adatokkal dolgozni, lásd: a következő dokumentumokat:

* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [Oozie használata a hdinsight eszközzel][hdinsight-use-oozie]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]
* [Python Hadoop-Stream programok HDInsight a fejlesztéséhez][hdinsight-develop-streaming]

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
