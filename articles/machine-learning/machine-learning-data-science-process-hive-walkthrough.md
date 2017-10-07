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
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>hello Team adatok tudományos folyamat működés közben: használata Azure HDInsight Hadoop-fürtök
Ez a forgatókönyv használjuk hello [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md) egy végpont forgatókönyv használatával egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) toostore, vizsgálatát, és nyilvánosan funkció hello visszafejtés adatait rendelkezésre álló [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset és toodown az hello adatokat. Hello adatok modelljei épülnek Azure Machine Learning toohandle multiclass és bináris osztályozás és regressziós prediktív feladatok.

Ez a forgatókönyv bemutatja, hogyan toohandle nagyobb (1 terabájtnál) adatkészletre vonatkozó hasonló forgatókönyvet használata a HDInsight Hadoop-fürtök az adatok feldolgozásához, lásd: [Team adatok tudományos folyamat - használata Azure HDInsight Hadoop-fürtök az 1 TB-os dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Akkor is lehetséges toouse egy IPython notebook tooaccomplish hello hello 1 TB-os adatkészlet bemutatott hello forgatókönyv feladatokat. Felhasználók, akik volna, ez a megközelítés konzultáljon tootry például hello [Criteo forgatókönyv Hive ODBC-kapcsolat használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakör.

## <a name="dataset"></a>NYC Taxi Utazgatással adatkészlet leírása
hello NYC Taxi út adatok körülbelül 20GB tömörített vesszővel tagolt (CSV) fájl (tömörítetlen ~ 48GB), amely több mint 173 millió egyedi utak és hello turistajegyek esetében minden út kifizette. Minden út rekord tartalmazza hello felvétel és Gyűjtőtár hely és idő, anonimizált rejthetők el (illesztőprogram) engedély száma és medallion (taxi tartozó egyedi azonosító) számát. hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:

1. hello "trip_data" CSV-fájlok tartalmazzák a út részletek, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza. Íme néhány példa rekordok:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello "trip_fare" CSV-fájlok hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza. Íme néhány példa rekordok:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

hello egyedi kulcs toojoin út\_adatok és út\_jegy ára áll hello mezők: medallion, rejthetők el\_engedély és a felvételi\_datetime.

minden tooget hello részletek vonatkozó tooa adott út, három kulccsal rendelkező elegendő toojoin: "medallion" hello "ellophatja\_licenc" és "a felvételi\_datetime".

Néhány további részleteket hello adatok azt írják le, ha tároljuk őket a Hive táblák hamarosan.

## <a name="mltasks"></a>Előrejelzés feladatok példák
Majdnem elérte az adatokat, ha azt szeretné, hogy az elemzés alapján toomake előrejelzéseket hello típusú meghatározása segít elmagyarázza, hogy a folyamat tooinclude kell hello feladatok.
A forgatókönyv hello azon alapul, amelynek létrehozását oldjuk előrejelzés problémák három példa *tipp\_összeg*:

1. **Bináris osztályozási**: előre jelezni, függetlenül attól, tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_Összeg* $ 0 egy negatív példában látható.
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. **Multiclass besorolási**: toopredict hello tartomány tipp díjak kifizette hello út. A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regressziós feladat**: toopredict hello hello tipp fizetni útnak.  

## <a name="setup"></a>Állítson be egy HDInsight Hadoop-fürt speciális elemzésekre
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

Speciális elemzésekre három lépést a HDInsight-fürtök által környezetet az Azure állíthat be:

1. [Hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md): ezt a tárfiókot az Azure Blob Storage adatok tárolására szolgál. a HDInsight-fürtök hello adatait is itt található.
2. [Testre szabhatja az Azure HDInsight Hadoop fürtök a hello Advanced Analytics folyamat és a technológia](machine-learning-data-science-customize-hadoop-cluster.md). Ebben a lépésben létrehoz egy Azure HDInsight Hadoop 64 bites Anaconda Python 2.7 a fürt összes csomópontján telepíteni. Nincsenek két fontos lépések az tooremember a HDInsight-fürt testreszabása közben.
   
   * Ne felejtse el toolink hello tárfiók létrehozásakor az 1. lépésben a HDInsight-fürthöz létrehozott. Ez a tárfiók használt tooaccess adatok feldolgozott hello fürtön belül.
   * Hello fürt létrehozása után engedélyezze a távoli elérés toohello átjárócsomópont hello fürt. Keresse meg a toohello **konfigurációs** fülre, és kattintson **távoli engedélyezése**. Ez a lépés hello felhasználói hitelesítő adatok a távoli bejelentkezési határozza meg.
3. [Hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md): az Azure Machine Learning munkaterület használt toobuild gépi tanulási modellek. Ez a feladat címzettjei egy kezdeti adatfeltárás befejezése után, és a mintavételi hello HDInsight-fürt használatával le.

## <a name="getdata"></a>Egy nyilvános forrásból hello adatok beolvasása
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

tooget hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset nyilvános helyéről, segítségével ismertetett hello-metódusok bármelyike [adatok áthelyezése tooand az Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello adatok tooyour gép.

Itt azt ismertetik, hogyan AzCopy tootransfer hello használata-adatokat tartalmazó fájlokat. toodownload és az AzCopy telepítési útmutatás alapján hello: [Ismerkedés az AzCopy parancssori segédprogram hello](../storage/common/storage-use-azcopy.md).

1. Egy parancssori ablakot a hello AzCopy parancsok követően, hogy ki *< path_to_data_folder >* hello kívánt cél:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Hello Másolás befejezése után összesen 24 tömörített fájlok kiválasztott hello adatok mappában találhatók. Bontsa ki a letöltött hello fájlok toohello ugyanabban a könyvtárban a helyi számítógépen. Jegyezze fel a tömörítetlen hello fájlokat tároló hello mappát. Ez a mappa lesz hivatkozott tooas hello *< elérési út\_való\_unzipped_data\_fájlok\>*  van következik.

## <a name="upload"></a>Töltse fel a hello adatok toohello alapértelmezett tároló Azure HDInsight Hadoop-fürt
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

Hello következő AzCopy parancsok, cserélje le a következő értékekkel hello tényleges hello Hadoop-fürt létrehozásakor megadott paraméterek és hello adatfájlok kicsomagolás hello.

* ***&#60; path_to_data_folder >*** hello könyvtár (együtt elérési utat) a hello unzipped adatfájljait tartalmazó számítógépre  
* ***&#60; Hadoop-fürt tárfióknév >*** hello a HDInsight-fürthöz társított storage-fiók
* ***&#60; alapértelmezett tároló Hadoop-fürt >*** hello alapértelmezett tároló a fürt által használt. Vegye figyelembe, hogy hello neve hello alapértelmezett tároló az általában hello azonos nevet maga hello fürtként. Például ha hello fürt "abc123.azurehdinsight.net" nevezik, hello alapértelmezett tároló abc123.
* ***&#60; tárfiók kulcsa >*** hello kulcsának hello a fürt által használt

A parancssorba vagy egy Windows PowerShell-ablakot a gépen futtassa a következő két AzCopy parancs hello.

Ez a parancs feltölt hello út adatok túl***nyctaxitripraw*** könyvtárban lévő hello alapértelmezett tároló hello Hadoop-fürt.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Ez a parancs feltölt hello jegy ára adatok túl***nyctaxifareraw*** könyvtárban lévő hello alapértelmezett tároló hello Hadoop-fürt.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

hello adatok mostantól az Azure Blob Storage és készen áll a toobe hello HDInsight-fürtön belül kell.

## <a name="#download-hql-files"></a>Jelentkezzen be a Hadoop-fürt átjárócsomópontjához hello és és a felderítő adatelemzés előkészítése
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

hello fürt felderítő adatelemzés és a mintavételi hello adatok le tooaccess hello átjárócsomópont eljárással hello leírt [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

Ez a forgatókönyv elsősorban használjuk írt lekérdezések [Hive](https://hive.apache.org/), egy SQL-szerű lekérdező nyelv, tooperform előzetes adatok explorations. hello Hive-lekérdezések .hql fájlok tárolják. A Microsoft majd lefelé minta az adatok toobe belül Azure Machine Learning modellek létrehozására használt.

tooprepare hello fürt felderítő adatelemzéshez, letöltődnek hello .hql fájlokat tartalmazó hello vonatkozó Hive parancsfájlokat a [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa helyi könyvtárába (C:\temp) hello átjárócsomópont. toodo a, nyissa meg hello **parancssor** belül hello a fürt és a probléma a következő két parancsot hello hello átjárócsomópontjához:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

A két parancsok töltik le a forgatókönyv toohello helyi könyvtárban szükséges fájlokat .hql ***C:\temp &#92;*** hello központi csomópontban.

## <a name="#hive-db-tables"></a>Hive-adatbázis és hónaponként particionált táblák létrehozása
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

Dolgozunk most már készen áll a toocreate Hive táblák a következőt: taxi adatkészlethez.
Hello Hadoop-fürt átjárócsomópontjához hello, nyissa meg hello ***Hadoop parancssori*** a hello hello átjárócsomópont asztalát, és írja be a hello Hive directory hello parancs megadásával

    cd %hive_home%\bin

> [!NOTE]
> **Ez a forgatókönyv minden Hive parancsot futtatja hello fent Hive bin / directory kérdés. Ez az elérési út probléma merül fel automatikusan kezeli. "Struktúra directory prompt" hello kifejezéseket használjuk a "struktúra bin / directory prompt", és a "Hadoop parancssori" azonos értelemben ebben a bemutatóban.**
> 
> 

Hello Hive directory parancssorába írja be a következő parancsot a Hadoop parancssori hello átjárócsomópont toosubmit hello Hive lekérdezés toocreate Hive-adatbázis és táblák hello:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Íme hello hello tartalmának ***C:\temp\sample\_hive\_létrehozása\_db\_és\_tables.hql*** fájl, amelyhez Hive adatbázist hoz létre ***nyctaxidb *** és táblák ***út*** és ***jegy ára***.

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

A Hive parancsfájl hoz létre két tábla:

* hello "út" tábla minden egyes menethelyzetben (illesztőprogram adatai, felvételi idő, út távolság és időpontok) út részleteit tartalmazza.
* hello "jegy ára" tábla (jegy ára, tipp összegre, autópályadíjak és pótdíjak) jegy ára részleteit tartalmazza.

Ha ezekkel az eljárásokkal semmilyen további segítségre van szüksége, vagy tooinvestigate alternatív néhányat a meglévők közül, című hello [elküldeni a Hive-lekérdezések közvetlenül a hello Hadoop parancssori ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Partíció tooHive adattáblák betöltése
> [!NOTE]
> Ez a művelet rendszerint egy **Admin** feladat.
> 
> 

hello NYC taxi dataset adatkészletben, a természetes particionálás havonta, amelyek tooenable feldolgozásával és lekérdezéseivel gyorsabb használatára. az alábbi PowerShell-parancsok hello (hello segítségével hello Hive könyvtárból kiadott **Hadoop parancssori**) adatok toohello "út" és "jegy ára" Hive táblák havonta particionálva betölteni.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Hello *minta\_hive\_betölteni\_adatok\_által\_partitions.hql* fájl tartalmazza-e hello alábbi **betöltése** parancsok.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Vegye figyelembe, hogy egy használjuk itt hello feltárási folyamata a Hive-lekérdezések száma is magában foglalja, csupán egyetlen partícióra vagy csak néhány partíciókat. De ezeket a lekérdezéseket futtathat hello teljes adatok között.

### <a name="#show-db"></a>A HDInsight Hadoop-fürt hello adatbázisok megjelenítése
a HDInsight Hadoop-fürt hello Hadoop parancssori ablakban futtassa a következő parancsot a Hadoop parancssori hello belül létrehozott tooshow hello adatbázisok:

    hive -e "show databases;"

### <a name="#show-tables"></a>Hello nyctaxidb adatbázis hello Hive táblák megjelenítése
tooshow hello táblák hello nyctaxidb adatbázisban, futtassa a következő parancsot a Hadoop parancssori hello:

    hive -e "show tables in nyctaxidb;"

Azt is meggyőződhet arról, hogy hello táblák hello az alábbi parancsot a vannak-e particionálni:

    hive -e "show partitions nyctaxidb.trip;"

hello várható kimenete az alább látható:

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

Ehhez hasonlóan azt is győződjön meg arról, hogy hello jegy ára tábla particionálva van hello az alábbi parancsot a:

    hive -e "show partitions nyctaxidb.fare;"

hello várható kimenete az alább látható:

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

## <a name="#explore-hive"></a>Az adatok feltárása és Hive mérnöki szolgáltatás
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

az adatok feltárása hello és mérnöki feladatok adatok hello Hive táblák töltve a Hive-lekérdezések használatával valósítható hello szolgáltatás. Az alábbiakban például olyan feladatokat, hogy azt végigvezetik Önt ebben a szakaszban:

* A két tábla hello felső 10 rekordok megtekintése
* Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.
* Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.
* Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.
* Szolgáltatások elő számítástechnikai hello közvetlen út távolság.

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a>Feltárása: Tábla út hello felső 10 rekordok megtekintése
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

milyen hello adatok a következőképpen néz toosee, azt vizsgálja meg a 10 rögzíti az összes táblából. Futtassa a következő két lekérdezések külön-külön parancssorból hello Hive directory rekordokban hello Hadoop parancssori konzol tooinspect hello hello.

tooget hello felső 10 rekordok hello tábla "út" a hónap első hello:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

tooget hello felső 10 rekordot hello tábla "díjszabás" hello az első hónapja:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Ez a legtöbbször hasznos toosave hello rekordok tooa fájl kényelmes megtekintésre. Ezt egy kis változást toohello fent lekérdezés éri el:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a>Feltárása: Hello rekordok számának megjelenítése az egyes hello 12 partíciók
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Fontos van hello hogyan utazgatással hello száma változik hello naptári év során. Havonta csoportosítási lehetővé teszi toosee ehhez a terjesztéshez utak néz.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Ez biztosítanak számunkra hello kimenete:

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

Itt hello első oszlop hello hónap pedig hello második utazgatással hello száma az adott hónapban.

Azt is is száma hello rekordok teljes számát a út adatkészlet a következő parancs hello Hive könyvtár parancssorába hello kiállításával.

    hive -e "select count(*) from nyctaxidb.trip;"

Ez eredményez:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Parancsok hasonló toothose hello út adatkészletnél látható azt kiadhatnak Hive-lekérdezések hello Hive directory parancssorból hello jegy ára adatkészlet toovalidate hello rekordok száma.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Ez biztosítanak számunkra hello kimenete:

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

Figyelje meg, hogy mindkét adatkészletek visszaküldött hello pontos azonos számú havonta való adatváltások számát. Ez lehetővé teszi hello első ellenőrzése, hogy megfelelően lett betöltve adatok hello.

Hello jegy ára adatkészlet-rekordok teljes száma hello számbavételi végezhető paranccsal hello alatt hello Hive directory parancssorból:

    hive -e "select count(*) from nyctaxidb.fare;"

Ez eredményez:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

a két tábla rekordok teljes számát hello is hello azonos. Ez lehetővé teszi a második ellenőrzési adott hello adatok helyesen betöltése.

### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Út elosztása medallion szerint
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Ebben a példában hello medallion (taxi számok) azonosítja a 100-nál több való adatváltások számát egy adott időtartamon belül. hello lekérdezés számos előnyt biztosít az hello particionált tábla hozzáférés óta, akkor annak hello partíció változó által **hónap**. hello lekérdezési eredmények nyelven íródtak tooa helyi fájl queryoutput.tsv `C:\temp` hello központi csomóponton.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Íme hello tartalmának *minta\_hive\_út\_száma\_által\_medallion.hql* fájl a vizsgálathoz.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

hello medallion hello NYC taxi adathalmaz egyedi cab azonosítja. Azt is azonosíthatja, hogy mely kabinetfájlok "foglalt" azzal, hogy melyik végzett több mint egy bizonyos számú való adatváltások számát egy adott időszakban. hello alábbi példa azonosítja kabinetfájlok végrehajtott egynél több száz helymódosításra utazgatással hello első három hónap, és menti a lekérdezési eredmények tooa helyi fájl esetén C:\temp\queryoutput.tsv hello.

Íme hello tartalmának *minta\_hive\_út\_száma\_által\_medallion.hql* fájl a vizsgálathoz.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

A hello struktúra directory parancssorba probléma hello parancs az alábbi:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Út terjesztési medallion és hack_license
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Ha dataset felfedezése, gyakran szeretnénk co-csoportokat előfordulásainak száma tooexamine hello. Ebben a szakaszban adja meg, hogyan toodo ezt kabinetfájlokban és illesztőprogramok egy példát.

Hello *minta\_hive\_út\_száma\_által\_medallion\_license.hql* fájlcsoportok hello jegy ára adatok megadása "medallion" és "hack_license" és az egyes kombinációkban számát adja vissza. Az alábbiakban annak tartalmát.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

A lekérdezés által visszaadott cab-fájl és az adott illesztőprogram kombinációját utazgatással csökkenő száma alapján rendezve.

A hello Hive directory kérdés, futtassa:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

hello lekérdezési eredmények tooa helyi fájl C:\temp\queryoutput.tsv készültek.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Feltárása: Értékeli az adatminőségi érvénytelen hosszúság vagy szélességi rekordok ellenőrzésével
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

A felderítő adatelemzés közös célja tooweed kimenő érvénytelen vagy hibás rögzíti. hello jelen szakaszban ismertetett példa meghatározza, hogy vagy hello hosszúsági és szélességi mezőket tartalmaz értéket sokkal hello NYC területen kívül. Valószínű, hogy az ilyen bejegyzések rendelkezik-e egy hibás hosszúság-szélességi értékeknek, mivel azt szeretnénk, ha azokat olyan adatot, amely toobe modellezési használt tooeliminate.

Íme hello tartalmának *minta\_hive\_minőségi\_assessment.hql* fájl a vizsgálathoz.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


A hello Hive directory kérdés, futtassa:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Hello *-S* argumentum, a parancsban benne mellőzi hello állapota képernyőn nyomtatás hello térkép/csökkentse Hive-feladatok. Ez akkor hasznos, mert így hello képernyő nyomtatása hello Hive lekérdezés kimeneti olvashatóbb.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Feltárása: Út tippek bináris osztály disztribúciók
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Hello bináris osztályozási problémához hello leírt [előrejelzés feladatok példái](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakaszban is hasznos tooknow tipp vagy nem kapott-e. A terjesztés tippek az bináris:

* Tipp megadott (osztály 1, tipp\_összeg > $0)  
* nincs ötlet (osztály: 0, tipp\_összeg = 0).

Hello *minta\_hive\_Formabontó\_frequencies.hql* fájlsablont ezt végzi.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

A hello Hive directory kérdés, futtassa:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a>Feltárása: Hello multiclass beállításban osztály disztribúciók
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Hello multiclass osztályozási problémához hello leírt [előrejelzés feladatok példái](machine-learning-data-science-process-hive-walkthrough.md#mltasks) szakasz adatkészlet is adatmodelljeinek tooa természetes besorolás, ahol szeretnénk megadott hello tippek toopredict hello mennyiségét. Használhatunk bins toodefine tipp tartományok hello lekérdezésben. tooget hello osztály terjesztéseket hello a különböző tipp tartományokat, hello használjuk *minta\_hive\_tipp\_tartomány\_frequencies.hql* fájlt. Az alábbiakban annak tartalmát.

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

Futtassa a parancsot követő Hadoop parancssori konzolról hello:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Feltárása: Számítási közvetlen hosszúság-szélesség két hely közötti távolság szerint
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Amelyek mérték hello közvetlen távolság lehetővé teszi ki hello eltérést és hello közötti toofind tényleges út távolságot. Ez a funkció jelenleg rávenni jelzésével utas lehet kisebb, valószínűleg tootip, ha azok mérje fel, hogy hello illesztőprogram szándékosan tartott őket egy sokkal hosszabb útvonalanként.

toosee hello tényleges út távolság és összehasonlítása hello [Haversine távolság](http://en.wikipedia.org/wiki/Haversine_formula) két hosszúság-szélesség pontja (hello "nagy kör" távolság), használjuk hello trigonometriai elérhető belül struktúra, így:

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

A fenti hello lekérdezés R miles a föld hello hello sugara, továbbá pi konvertált tooradians. Ne feledje, hogy hello hosszúság-szélesség pontok hello NYC terület messze "szűrt" tooremove értékeket.

Ebben az esetben az eredmények tooa könyvtárat "queryoutputdir" azt írni. hello parancssorrend alább látható először hoz létre a kimeneti könyvtárhoz, és hello Hive parancsot futtatja, majd.

A hello Hive directory kérdés, futtassa:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


hello lekérdezési eredmények írt too9 Azure-blobok ***queryoutputdir/000000\_0*** túl ***queryoutputdir/000008\_0*** hello alapértelmezett tároló hello Hadoop-fürt alatt.

hello egyes blobok toosee hello méretét, Futtatás hello parancs hello Hive directory parancssorból a következő:

    hdfs dfs -ls wasb:///queryoutputdir

a megadott fájl tartalmát toosee hello 000000 mondja ki\_0, használjuk a Hadoop által `copyToLocal` parancsban, így.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`nagyon nagy méretű fájlok esetén lassú lehet, és nem ajánlott azokat való használatra.  
> 
> 

A kulcs az adatok találhatók az Azure blob előnye, hogy azt is adatokba hello Azure Machine Learning segítségével hello belül [és adatokat importálhat] [ import-data] modul.

## <a name="#downsample"></a>Adatok és -buildek mintákat az Azure Machine Learning le
> [!NOTE]
> Ez a művelet rendszerint egy **adatok tudósok** feladat.
> 
> 

Hello felderítő adatok elemzési fázis után dolgozunk most már készen áll a toodown hello adatot az Azure Machine Learning modellek készítéséhez. Ebben a szakaszban megmutatjuk, hogyan toouse egy Hive lekérdezés toodown hello mintaadatok, amely majd elérése hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.

### <a name="down-sampling-hello-data"></a>Lefelé hello az adatok mintavétele
Az alábbi eljárással két lépésben történik. Hello csatlakoztassa azt **nyctaxidb.trip** és **nyctaxidb.fare** táblák összes rekord található három kulcs: "medallion", "ellophatja\_licenc", és "a felvételi\_datetime". A Microsoft létrehozza a bináris besorolási címkék **Formabontó** és több osztály besorolási címkék **tipp\_osztály**.

toobe képes toouse hello le a mintaadatokat közvetlenül a hello [és adatokat importálhat] [ import-data] modul az Azure Machine Learning szükséges toostore hello eredményeit hello lekérdezés tooan belső struktúra táblázat felett. A következő azt egy belső Hive tábla létrehozásához és tölthet tartalmának csatlakoztatott hello és le a mintaadatokat.

hello lekérdezés vonatkozik közvetlenül toogenerate hello óra, nap, hét év, hétköznap (hétfő jelző 1 és 7 jelző vasárnap) hello a szabványos Hive funkciók "felvétel\_dátum és idő" mező, és hello felvétel és dropoff közvetlen távolságát hello helyek. Felhasználók túl hivatkozhat[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) ilyen funkciók teljes listáját.

lekérdezés hello és le minták hello adatokat, hogy a lekérdezés eredményei hello elfér hello Azure Machine Learning Studio. Hello eredeti adathalmazból csak körülbelül 1 %-át hello Studio importálta.

Az alábbiakban hello tartalmát *minta\_hive\_előkészítése\_a\_aml\_full.hql* fájlt, amely adatokat előkészíti a modell létrehozása az Azure Machine Learning.

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

toorun Ez a lekérdezés hello Hive könyvtárból kérni:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Most már van egy belső tábla "nyctaxidb.nyctaxi_downsampled_dataset" hello is elérhetők [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban. Továbbá a Machine Learning modellek létrehozása ehhez az adatkészlethez előfordulhat, hogy használjuk.  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a>Az Azure Machine Learning tooaccess hello le a mintaadatokat hello adatokat importálhat modul használata
Hive-lekérdezéseket a hello kiállító előfeltételként [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban, igazolnia kell tooan Azure Machine Learning-munkaterület hozzáférési és hozzáférési hello toohello hitelesítő adatait fürt és a kapcsolódó tárfiók.

Egyes adatok hello [és adatokat importálhat] [ import-data] modul és hello paraméterek tooinput:

**HCatalog kiszolgáló URI azonosítója**: Ha hello fürtnév abc123, akkor ez az egyszerűen: https://abc123.azurehdinsight.net

**Hadoop felhasználói fiók nevét** : hello fürthöz kiválasztott hello felhasználónevet (**nem** hello távelérési felhasználónév)

**Hadoop ser fiók jelszava** : hello fürt választott hello jelszóra (**nem** hello távelérési jelszó)

**Az kimeneti adatok** : Ez toobe Azure van kiválasztva.

**Az Azure storage-fiók neve** : hello hello-fürthöz tartozó alapértelmezett a tárfióknak nevét.

**Az Azure Tárolónév** : Ez hello alapértelmezett tároló hello fürt nevét, és van általában hello azonos hello fürt neveként. A fürt "abc123" nevű ez pedig csak abc123.

> [!IMPORTANT]
> **Bármely táblájának használatával hello tooquery jelenítsük [és adatokat importálhat] [ import-data] modul az Azure Machine Learning egy belső tábla kell lennie.** Tipp: a meghatározhatja, hogy a tábla T D.db adatbázisban egy belső tábla a következőképpen történik.
> 
> 

A hello struktúra directory parancssorba probléma hello parancsot:

    hdfs dfs -ls wasb:///D.db/T

Ha hello tábla egy belső tábla és a telepítéskor, a tartalmát itt kell mutatnia. Egy másik módja toodetermine, hogy-e egy táblát egy belső tábla toouse hello Azure Tártallózó. Toonavigate toohello alapértelmezett tároló hello fürt nevét használja, és majd szűrés hello tábla neve. Ha hello tábla és annak tartalmát jelenik meg, Ez megerősíti, hogy egy belső tábla.

Íme egy pillanatkép hello Hive-lekérdezések és hello [és adatokat importálhat] [ import-data] modul:

![Adatokat importálhat modul Hive-lekérdezések](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Vegye figyelembe, hogy a mintaadatokat hello alapértelmezett tárolóban található le, mert hello eredményül kapott Hive-lekérdezést az Azure Machine Learning nagyon egyszerű, csak a "Válasszon * a nyctaxidb.nyctaxi\_felbontáscsökkentésének\_adatok".

hello adatkészlet most hello kiindulási pontjaként gépi tanulási modellek készítéséhez használható.

### <a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása
Azt is képes tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net). hello adatok készen áll a us toouse fent azonosított hello előrejelzés problémák megoldásához:

**1. Bináris osztályozási**: toopredict tipp egy út kifizetett-e.

**Használt tanuló:** két osztályú logisztikai regresszió

a. A probléma az cél (vagy osztály) címke van "Formabontó". Az eredeti le mintát adatkészletet rendelkezik néhány az oszlopokat, amelyek a cél kiszivárgásának a besorolási kísérleti fázisú funkciókat. Különösen: tipp\_osztály, tipp\_mennyiségét, és a végösszeg\_összeg hello címkét, amely nem érhető el a tesztelési idő fedik fel információt. Ezek az oszlopok eltávolítása hello használata azon [Select Columns in Dataset] [ select-columns] modul.

az alábbi hello pillanatkép jeleníti meg a kísérlet toopredict tipp egy adott út kifizetett-e.

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Ehhez a kísérlethez a cél címke azokat a terjesztéseket volt körülbelül 1:1.

az alábbi hello pillanatkép hello terjesztési tipp osztály címkék hello bináris osztályozási problémához jeleníti meg.

![Tipp osztály címkék terjesztése](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Ennek eredményeképpen azt szerezzen be egy AUC 0.987 a, az alábbi hello ábrán látható módon.

![AUC érték](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. Multiclass besorolási**: toopredict hello tartomány hello út hello használatával korábban fizetett tipp díjak meghatározott osztályának.

**Használt tanuló:** Multiclass logisztikai regresszió

a. A probléma van az cél (vagy osztály) címke "Tipp\_osztály" amely (0,1,2,3,4) öt érték valamelyikét hajthatja végre. Hello bináris osztályozási esetében, mint van néhány az oszlopokat, amelyek a cél kiszivárgásának a kísérleti fázisú funkciókat. Különösen: Formabontó, tipp\_teljes összeg\_összeg hello címkét, amely nem érhető el a tesztelési idő fedik fel információt. Ezekben az oszlopokban hello eltávolítása [Select Columns in Dataset] [ select-columns] modul.

hello az alábbi pillanatkép készítése a rendeltetési tipp valószínűleg toofall kísérlet toopredict (osztály: 0: tipp = 0, 1 osztály: > $0 és tipp tipp < $5, osztály 2 =: > $5 és tipp tipp < = 10 $ osztály 3: > $10 és tipp tipp < = $20 4. osztály: tipp > $20)

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Most megjelenítése a tényleges vizsgálati osztály terjesztési néz. Látható, hogy amíg osztály 0 és 1-osztály nem elterjedt, hello más osztályok ritkák.

![Osztály terjesztési tesztelése](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Ehhez a kísérlethez az előrejelzési pontosság egy zavart mátrix toolook használhatja azt. Ez az alábbiakban látható.

![Zavart mátrix](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Vegye figyelembe, hogy az osztály pontosság hello elterjedt osztályokon pedig elég jó hello modell nem "tanulás" szép munka meg hello ritkább osztályok.

**3. Regressziós feladat**: toopredict hello tipp fizetni útnak.

**Használt tanuló:** Boosted döntési fája

a. A probléma van az cél (vagy osztály) címke "Tipp\_összeg". A cél kiszivárgásának ebben az esetben is: Formabontó, tipp\_osztály, teljes\_összeg; minden változókhoz hello tipp mennyiségét, amely általában nem érhető el a tesztelési idő információk felfedése. Ezekben az oszlopokban hello eltávolítása [Select Columns in Dataset] [ select-columns] modul.

hello pillanatkép belows a megadott tipp hello kísérlet toopredict hello összegét mutatja.

![Kísérlet pillanatkép](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regressziós problémák az előrejelzési hello pontosság mérjük négyzet hello hiba a hello előrejelzéseket megtekintésével, hello együttható, és például hello. Megmutatjuk, ezek az alábbi.

![Előrejelzés statisztikai](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Azt látja, hogy kapcsolatos hello együttható 0.709, a modell együttható magyarázza készül 71 % hello eltérés utalás.

> [!IMPORTANT]
> További információk az Azure Machine Learning toolearn és hogyan tooaccess és használni, tekintse meg a túl[Mi az Machine Learning?](machine-learning-what-is-machine-learning.md). A Machine Learning kísérleteket Azure Machine Learning szolgáltatásban egy csoportját játszik nagyon hasznos erőforrás hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). hello gyűjtemény egy kísérletek skáláját ismerteti, és az Azure Machine Learning képességek hello tartományba alapos bevezetést tartalmaz.
> 
> 

## <a name="license-information"></a>Licencinformációk
Ez a minta a forgatókönyv és a hozzá tartozó szkriptek által megosztott Microsoft hello MIT licence. Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.

## <a name="references"></a>Referencia
• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
