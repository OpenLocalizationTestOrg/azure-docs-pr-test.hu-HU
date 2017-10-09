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
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>A Linux-alapú HDInsight Hive használatával repülési késleltetés adatok elemzése

Ismerje meg, hogyan tooanalyze repülési késleltetés Hive használata a Linux-alapú HDInsight majd adatexportálás hello adatok tooAzure SQL-adatbázis Sqoop használatával.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="prerequisites"></a>Előfeltételek

* **HDInsight-fürtök**. Lásd: [Hadoop használatának megkezdésében a Hive HDInsight Linux rendszeren](hdinsight-hadoop-linux-tutorial-get-started.md) vonatkozó lépéseket egy új Linux-alapú HDInsight-fürtöt hoz létre.

* **Azure SQL Database** Azure SQL-adatbázis használata a cél-tárolóban. Ha egy SQL-adatbázis már nem rendelkezik, tekintse meg a [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).

* **Azure parancssori felület (CLI)**. Ha még nem telepítette az Azure parancssori felület hello, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) a további lépéseket.

## <a name="download-hello-flight-data"></a>Hello felé továbbított adatok letöltése

1. Keresse meg a túl[kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda][rita-website].

2. Hello lapon jelölje be a következő értékek hello:

   | Név | Érték |
   | --- | --- |
   | Szűrő év |2013 |
   | Időszak szűrése |Január |
   | Mezők |Év FlightDate, UniqueCarrier, vivőjel, FlightNum, OriginAirportID, eredet, OriginCityName, OriginState, DestAirportID, cél, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Törölje az összes többi mező |

3. Kattintson a **Letöltés** gombra.

## <a name="upload-hello-data"></a>Hello adatok feltöltése

1. A következő parancs tooupload hello zip fájlt toohello HDInsight fürt átjárócsomópontjába hello használata:

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    Cserélje le **Fájlnév** hello nevű hello zip-fájl. Cserélje le **felhasználónév** hello SSH-bejelentkezéskor hello HDInsight-fürthöz. Cserélje le a FÜRTNÉV hello nevű hello HDInsight-fürt.

   > [!NOTE]
   > A jelszó tooauthenticate használatakor az SSH-bejelentkezéskor kéri hello jelszót. Ha a használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter, és adja meg a hello elérési toohello titkos kulccsal. Például: `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Hello feltöltés befejezése után csatlakoztassa az SSH használatával toohello fürtöt:

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Miután csatlakozott, használja a következő toounzip hello .zip fájl hello:

    ```
    unzip FILENAME.zip
    ```

    Ez a parancs egy CSV-fájlt, amely körülbelül 60 MB bontja ki.

4. HDInsight tároló parancs toocreate könyvtár következő hello használja, és másolja hello toohello könyvtára:

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-hello-hiveql"></a>Hozzon létre és futtasson hello HiveQL

Az elnevezett Hive táblát használja hello alábbi lépéseit az hello CSV-fájlból tooimport adatok **késések**.

1. Használjon hello következő toocreate parancsot, és egy új fájlt szerkesztése **flightdelays.hql**:

    ```
    nano flightdelays.hql
    ```

    Szöveg hello a fájl tartalmát, a következő hello használata:

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

2. toosave hello fájl használata **Ctrl + X**, majd **Y** .

3. toostart struktúra és futtatási hello **flightdelays.hql** fájlt, a következő parancs hello használata:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

   > [!NOTE]
   > Ebben a példában `localhost` szolgál, mivel Ön hello HDInsight-fürt, amely hiveserver2-n futtató csatlakoztatott toohello átjárócsomópont.

4. Egyszer hello __flightdelays.hql__ parancsfájlt futtató befejeződik, használja hello következő parancsot egy interaktív Beeline munkamenet tooopen:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Amikor megjelenik a hello `jdbc:hive2://localhost:10001/>` kérni, használja a következő tooretrieve adatait importált hello repülési késleltetés adatokból hello.

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Ez a lekérdezés lekéri a város, hogy a tapasztalt időjárási késéseket, valamint hello átlagos idő késleltetés, és mentse túl listáját`/tutorials/flightdelays/output`. Később Sqoop hello adatokat olvas a következő helyről, és exportálni onnan tooAzure SQL-adatbázis.

6. Adja meg a tooexit Beeline, `!quit` hello a parancssorból.

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása

Ha már rendelkezik egy SQL-adatbázis, ha előbb telepítik azokra a hello kiszolgáló nevét. Hello kiszolgálónév hello található [Azure-portálon](https://portal.azure.com) kiválasztásával **SQL-adatbázisok**, majd szűrést hello hello nevét az adatbázis-, és toouse kívánja. hello kiszolgálónév szerepel hello **SERVER** oszlop.

Ha még nem rendelkezik SQL-adatbázis, használja a hello adatokat [SQL Database oktatóanyag: SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md) toocreate egy. Mentse a hello adatbázis hello kiszolgálójának nevét.

## <a name="create-a-sql-database-table"></a>SQL-adatbázis tábla létrehozása

> [!NOTE]
> Számos módon tooconnect tooSQL adatbázisban van, és hozzon létre egy táblát. a következő lépéseket használata hello [FreeTDS](http://www.freetds.org/) hello HDInsight-fürtök.


1. SSH tooconnect toohello Linux-alapú HDInsight-fürtöt, és hello SSH-munkamenetből a lépéseket követve futtatási hello használata.

2. A következő parancs tooinstall FreeTDS hello használata:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Hello telepítés befejezése után használja a következő parancs tooconnect toohello SQL adatbázis-kiszolgáló hello. Cserélje le **kiszolgálónév** hello SQL adatbázis-kiszolgáló nevével. Cserélje le **adminLogin** és **adminPassword** hello bejelentkezéskor SQL-adatbázis. Cserélje le **databaseName** hello adatbázisnévvel.

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Kimeneti hasonló toohello a következő szöveg jelenik meg:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set toosqooptest
    1>
    ```

4. A hello `1>` kéri, adja meg az alábbi hello:

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése. Ez a lekérdezés egy nevű táblát hoz létre **késések**, a fürtözött index.

    A következő lekérdezés tooverify, amelyek a tábla hello használata hello jött létre:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    a kimeneti hello hasonló toohello a következő szöveget:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.

## <a name="export-data-with-sqoop"></a>A Sqoop adatok exportálása

1. A következő parancs tooverify, hogy a Sqoop látja-e az SQL-adatbázis hello használata:

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Ez a parancs adatbázisok, többek között a hello adatbázis hello késések tábla a korábban létrehozott listáját adja vissza.

2. Használjon hello következő parancsot a hivesampletable toohello mobiledata tábla tooexport adatait:

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop hello késések tábla tartalmazó toohello adatbázis csatlakozik, valamint adatokat exportál hello `/tutorials/flightdelays/output` könyvtár toohello késések táblában.

3. Hello parancs után használja a következő tooconnect toohello adatbázis TSQL használatával hello:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    A csatlakozás után használja a következő, hogy hello adatok volt-e az exportált toohello mobiledata table utasítások tooverify hello:

    ```
    SELECT * FROM delays
    GO
    ```

    Meg kell jelennie hello tábla listáját. Típus `exit` tooexit hello tsql segédprogram.

## <a id="nextsteps"></a> Következő lépések

toolearn további módszereket toowork adatokkal a Hdinsightban, lásd a következő dokumentumok hello:

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
