---
title: "Az Azure Data Lake méretezhető Adattudomány: egy végpontok közötti forgatókönyv |} Microsoft Docs"
description: "Hogyan toouse Azure Data Lake toodo adatok feltárása és bináris osztályozás a dataset feladatok."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 91a8207f-1e57-4570-b7fc-7c5fa858ffeb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: bradsev;weig
ms.openlocfilehash: 8b05457ae7045a7aaed350a7502469f2247161e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a>Az Azure Data Lake méretezhető Adattudomány: egy végpont forgatókönyv
Ez a bemutató ismerteti, hogyan toouse Azure Data Lake toodo adatok feltárása és a bináris osztályozási feladatok hello NYC mintában taxiköltség út és a jegy ára dataset toopredict tipp egy jegy ára fizeti lesz-e. Az végigvezeti hello a hello [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess), végpontok, az adatok megszerzése toomodel képzési, és egy webszolgáltatás, amelyet hello modell közzéteszi toohello telepítését.

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
Hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) szükséges toomake összes hello képességekkel rendelkezik egyszerűen az adatok kutatók toostore adatok bármely méretét, az alakzat és a sebesség és a tooconduct adatfeldolgozás, speciális elemzés és a machine learning modellezési magas méretezhetőség költséghatékony módon.   Kell fizetnie a feladatonként, csak akkor, ha ténylegesen feldolgozott adatokat. Az Azure Data Lake Analytics U-SQL, az, hogy színátmenet hello deklaratív természetét a C# tooprovide méretezhető hello kifejezőerejével SQL nyelvű elosztott lekérdezés funkció. Tooprocess lehetővé teszi a séma alkalmazásával olvasható, a strukturálatlan adatok beszúrása egyéni logika és a felhasználó által definiált működik (UDF), és tartalmazza a bővítési tooenable finom nyomtatott szabályozhatják, hogyan tooexecute méretekben. toolearn hello tervezési alapvetően mögött U-SQL, kapcsolatos további információkért lásd: [Visual Studio által írt blogbejegyzés](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

A Data Lake Analytics kulcseleme a Cortana Analytics Suite csomagnak is, emellett az Azure SQL Data Warehouse, a Power BI és a Data Factory szolgáltatásokkal is együttműködik. Ez lehetővé teszi egy teljes felhőalapú big Data típusú adatok és a speciális elemzési platformot.

Ez a forgatókönyv megkezdése toocomplete hello szükséges feladatok Data Lake Analytics hello adatok tudományos folyamat alkotó, és milyen erőforrásokat és hello Előfeltételek ismertetésével tooinstall őket. Ezután U-SQL használatával hello adatfeldolgozási lépéseit sorolja fel, és arra a következtetésre jut jelenít meg hogyan toouse Python és az Azure Machine Learning Studio toobuild struktúra és üzembe prediktív modelleket hello. 

### <a name="u-sql-and-visual-studio"></a>U-SQL és a Visual Studio
Ez a forgatókönyv azt javasolja, hogy a Visual Studio tooedit U-SQL parancsfájlok tooprocess hello adatkészlet. hello U-SQL-parancsfájlok ismerteti, és egy különálló fájlban megadott. hello folyamat választásával dolgozhat fel, felfedezése és hello az adatok mintavétele tartalmazza. Azt is bemutatja, hogyan toorun egy U-SQL parancsfájl-alapú hello Azure-portál a feladat. Hive táblák hello adatok egy társított HDInsight fürt toofacilitate hello épület és a központi telepítés egy bináris besorolási modell az Azure Machine Learning Studióban jön létre.  

### <a name="python"></a>Python
Ez a forgatókönyv is egy szakaszt tartalmaz, amely bemutatja, hogyan toobuild és központi telepítése a Python használata az Azure Machine Learning Studio prediktív modellt.  Jupyter notebook hello Python parancsfájlokkal, ezeket a lépéseket, a folyamat a nyújtunk. hello notebook néhány további szolgáltatás mérnöki lépéseit, valamint a például a multiclass besorolást és a modellezési továbbá toohello bináris osztályozási modell itt vázolt regressziós modell konstrukció kódot tartalmazza. hello regressziós feladat érték toopredict hello hello tipp más tip szolgáltatások alapján. 

### <a name="azure-machine-learning"></a>Azure Machine Learning
Az Azure Machine Learning Studio használt toobuild és üzembe prediktív modelleket hello. Ebben az esetben két módszer használatával: első Python-parancsfájlok majd Hive táblák HDInsight (Hadoop) fürtökön.

### <a name="scripts"></a>Parancsprogramok
A bemutatóban szereplő eljárásokat csak hello egyszerű lépéseket. Letöltheti a teljes hello **U-SQL parancsfájl** és **Jupyter Notebook** a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

## <a name="prerequisites"></a>Előfeltételek
Ezek a témakörök megkezdése előtt hello következő kell rendelkeznie:

* Azure-előfizetés. Ha még nem rendelkezik egy, lásd: [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Ajánlott] A Visual Studio 2013 vagy újabb verzió. Ha még nem rendelkezik ilyen verziójú telepítve, akkor egy ingyenes közösségi verziója letölthető a [Visual Studio Community](https://www.visualstudio.com/vs/community/).

> [!NOTE]
> Visual Studio helyett hello Azure Portal toosubmit Azure Data Lake-lekérdezéseket is használhatja. Hogyan toodo, a Visual Studio- és kilépéskor egyaránt hello portal hello szakaszában jelenik meg a Microsoft szükséges műveleteket **feldolgozni az adatokat az U-SQL**. 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Azure Data Lake data tudományos környezet előkészítése
tooprepare hello adatok tudományos környezetben ennél a bemutatónál hozza létre a következő erőforrások hello:

* Az Azure Data Lake Store-(ADLS) 
* Az Azure Data Lake Analytics (ADLA)
* Az Azure Blob storage-fiók
* Azure Machine Learning Studio-fiók
* Az Azure Data Lake Tools for Visual Studio (ajánlott)

Ez a szakasz útmutatás toocreate minden erőforrásainak. Ha úgy dönt, toouse Hive táblák Azure Machine Learning segítségével, Python, a modell toobuild helyett tooprovision (Hadoop) HDInsight-fürtök is szüksége lesz. Az alternatív eljárás hello megfelelő részben leírt.


> [!NOTE]
> Hello **Azure Data Lake Store** is létrehozható, vagy külön-külön vagy hello létrehozásakor **Azure Data Lake Analytics** hello alapértelmezett tárolóként. Ezek külön-külön az alábbi erőforrások mindegyikének létrehozására vonatkozó utasításokat hivatkozott, de hello Data Lake-tárfiókot kell nem kell külön hoz létre.
>
> 

### <a name="create-an-azure-data-lake-store"></a>Hozzon létre egy Azure Data Lake Store


Hozzon létre egy ADLS a hello [Azure Portal](http://portal.azure.com). További információkért lásd: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Meg arról, hogy tooset hello a fürt AAD-identitása hello fel kell **DataSource** panelen található hello **opcionális konfigurációs** panel leírt van. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a>Azure Data Lake Analytics-fiók létrehozása
ADLA-fiók létrehozása a hello [Azure Portal](http://portal.azure.com). További információkért lásd: [oktatóanyag: Ismerkedés az Azure Data Lake Analytics Azure portál használatával](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a>Egy Azure Blob storage-fiók létrehozása
Egy Azure Blob storage-fiók létrehozása hello [Azure Portal](http://portal.azure.com). További információkért lásd: hello hozzon létre egy tárfiókot című [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).

 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a>Az Azure Machine Learning Studio fiók beállítása
Jelentkezzen be- / az Azure Machine Learning Studio a hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) lap. Kattintson a hello **az induláshoz** gombra, majd válassza a "Szabad munkaterület" vagy "Szabványos munkaterület". Ezt követően az Azure ML Studio képes toocreate kísérletek fogja.  

### <a name="install-azure-data-lake-tools-recommended"></a>Telepítheti az Azure Data Lake Tools [ajánlott]
Azure Data Lake Tools telepítése a Visual Studio verziójának [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Hello telepítés sikeres befejezése után nyissa meg a Visual Studio. Hello Data Lake lapon hello menü hello felső kell megjelennie. Az Azure-erőforrások meg kell jelennie a hello bal oldali panelen történő bejelentkezéskor be Azure-fiókjába.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="hello-nyc-taxi-trips-dataset"></a>hello NYC Taxi Utazgatással adatkészlet
hello itt használtuk adatkészlet nyilvánosan elérhető dataset – hello [NYC Taxi Utazgatással dataset](http://www.andresmh.com/nyctaxitrips/). hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (~ 48GB tömörítetlen) áll, több mint 173 millió rögzítése egyéni utak és hello turistajegyek esetében minden út kifizette. Minden út rekord hello felvétel és Gyűjtőtár helyeket tartalmaz, és időpontokat, anonimizált adatokon alapul (illesztőprogram) licencszám ellophatja, és hello medallion (taxi tartozó egyedi azonosító) száma. hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:

* hello "trip_data" CSV út részleteit, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza tartalmazza. Íme néhány példa rekordok:
  
       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
* hello "trip_fare" CSV hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza. Íme néhány példa rekordok:
  
       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

hello egyedi kulcs toojoin út\_adatok és út\_jegy ára tevődnek össze a következő három mezőt hello: medallion, rejthetők el\_licenc és a felvételi\_datetime. egy nyilvános Azure storage-blob hello nyers CSV-fájlok elérhetők. U-SQL parancsfájl hello ezt az összekapcsolást érték a hello [út és a jegy ára csatlakozás](#join) szakasz.

## <a name="process-data-with-u-sql"></a>A U-SQL adatok feldolgozása
hello adatfeldolgozási ebben a szakaszban ismertetett feladatok választásával dolgozhat fel, minőségi ellenőrzése, felfedezése és hello az adatok mintavétele. Is megmutatjuk, hogyan toojoin út és a jegy ára tábla. hello végső szakasz futtatási bemutatja a U-SQL parancsfájl feladatot hello Azure-portálon. Az alábbiakban hivatkozások tooeach alszakasz:

* [Adatfeldolgozást: nyilvános blob adatainak olvasása](#ingest)
* [Adatminőségi ellenőrzése](#quality)
* [Az adatok feltárása](#explore)
* [Csatlakozás út és a jegy ára](#join)
* [Adat-mintavételezésre](#sample)
* [U-SQL feladatok futtatása](#run)

hello U-SQL-parancsfájlok ismerteti, és egy különálló fájlban megadott. Letöltheti a teljes hello **U-SQL-parancsfájlok** a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

tooexecute U-SQL, nyissa meg a Visual Studio, kattintson a **fájl--> Új projekt-->**, válassza a **U-SQL projekt**, a nevet, és mentse tooa mappa.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> Már lehetséges toouse hello Azure Portal tooexecute U-SQL Visual Studio helyett. Keresse meg az Azure Data Lake Analytics-erőforrás toohello hello portálon, és közvetlenül, a következő ábra hello illusztrált lekérdezések.
> 
> 

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Adatfeldolgozást: A nyilvános blob adatainak olvasása
hello adatok Azure blob hello hello helyét hivatkozott  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**  és használatával kiolvasható **Extractors.Csv()**. Helyettesítse a saját tároló nevének és a tárfiók nevét az alábbi parancsfájlok a container_name@blob_storage_account_name hello wasb címét. Mivel ugyanazt a formátumot a hello fájlneveket, használhatjuk **út\_data_ {\*\}.csv** tooread összes 12 út fájl. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Mivel hello első sorában fejlécek, azt kell tooremove hello fejlécek, és oszloptípus átalakítása megfelelő néhányat a meglévők közül. Azt a mentés elvégezhető feldolgozott hello tooAzure Data Lake Storage használatával végzett **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ vagy tooAzure Blob-tároló fiók használatával  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** . 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data tooADL
    OUTPUT @trip   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data tooblob
    OUTPUT @trip   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Hasonlóképpen azt olvasási hello jegy ára adathalmaz. Kattintson a jobb gombbal az Azure Data Lake Store, választhatja az adatok toolook **Azure Portal adatkezelő-->** vagy **Fájlkezelőben** Visual studióban. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)

### <a name="quality"></a>Adatminőségi ellenőrzése
Miután út és a jegy ára az olvasott, adatminőségi ellenőrzése a következő módon hello végezhető. CSV-fájlok eredő hello kimeneti tooAzure Blob-tároló vagy az Azure Data Lake Store lehet. 

Keresse meg medallions hello száma és medallions egyedi száma:

    ///check hello number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Keresse meg azokat, amelyek 100-nál több utazgatással kellett medallions:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Található érvénytelen rekordokat pickup_longitude tekintetében:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Az egyes változók hiányzó értékeinek megkeresése:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Az adatok feltárása
Tehetünk ennek néhány adatok feltárása tooget hello adatok jobb megértése.

Hello terjesztési Formabontó és nem Formabontó utak keresése:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Tipp összeg lezárási értékekkel hello terjesztési található: 0,5,10 és 20 dollár.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Keresse meg a út távolság alapvető statisztikai adatait tartalmazza:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Hello százalékos érték út távolság keresése:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Csatlakozás út és a jegy ára
Út és a jegy ára medallion, hack_license és pickup_time társíthatók.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output tooblob
    OUTPUT @model_data_full   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data tooADL
    OUTPUT @model_data_full   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Az egyes utas száma kiszámítása hello rekordok, átlagos tipp összeg, tipp összeg varianciáját, százalékos aránya Formabontó való adatváltások számát.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Adat-mintavételezésre
Először igazolnia véletlenszerűen választ ki 0,1 % hello adatok hello illesztett táblából:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Majd végezzük rétegzett mintavételi bináris változó tip_class szerint:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output tooblob
    OUTPUT @model_data_stratified_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data tooADL
    OUTPUT @model_data_stratified_sample_1_1000   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>U-SQL feladatok futtatása
Amikor befejezte a U-SQL-parancsfájlok szerkesztésével, elküldheti azokat toohello server az Azure Data Lake Analytics-fiókkal. Kattintson a **Data Lake**, **feladat elküldése**, jelölje be a **Analytics-fiók**, válassza ki **párhuzamossági**, és kattintson a **küldje el a következőt** gombra.  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Amikor hello feladat sikeresen megfelelést, a feladat állapotának hello jelenik meg a Visual Studio figyelésre. Után hello feladat befejezése után történik, akkor még a visszajátszás hello feladat végrehajtási folyamatának, és megtudhatja, hello bottleneck lépéseket tooimprove a feladat hatékonyságát. Portál toocheck hello a U-SQL feladatok állapotának tooAzure is választhatja.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)

Most már ellenőrizheti a hello kimeneti fájlok az Azure Blob-tároló vagy Azure portálon. A következő lépésben hello modellezési műanyaggal rétegezett hello mintaadatok azt fogja használni.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Hozza létre és telepítheti az Azure Machine Learning modellek
Bemutatjuk, két lehetőség érhető el az Ön toopull adatokat az Azure Machine Learning toobuild és 

* Hello első lehetőség, használhatja az Azure Blob tooan írt mintát hello adatokat (a hello **adat-mintavételezésre** fenti lépést) használ a Python toobuild és központi telepítése az Azure Machine Learning modellek. 
* A második lehetőség hello adatait hello Azure Data Lake közvetlenül a Hive-lekérdezések használatával. Ez a beállítás megköveteli, hogy hozzon létre egy új HDInsight-fürtöt, vagy használja egy meglévő HDInsight-fürtre, ahol hello Hive táblák pont toohello NY Taxi adatokat az Azure Data Lake Storage.  Mindkét ezek a beállítások az alábbi arról lesz szó. 

## <a name="option-1-use-python-toobuild-and-deploy-machine-learning-models"></a>1. lehetőség: Használata Python toobuild és központi telepítése a machine learning modellek
toobuild és machine learning modellek pythonos környezetekben, a rendszer a Jupyter Notebook létrehozása, a helyi számítógépen vagy az Azure Machine Learning Studióban. hello Jupyter Notebook megadott [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) tartalmaz hello teljes kód tooexplore, adatok, a szolgáltatás mérnöki csapathoz, a modellezési és a központi telepítés megjelenítése. Ez a cikk megmutatjuk, ebben az esetben a hello modellezési és a központi telepítés. 

### <a name="import-python-libraries"></a>Python könyvtárak importálása
A sorrend toorun hello minta Jupyter Notebook vagy hello Python-parancsfájl, hello következő Python csomagok szükségesek. Hello AzureML Notebook szolgáltatást használja, ha ezeket a csomagokat előre telepített törölték.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-hello-data-from-blob"></a>A blob adatait hello olvasása
* Kapcsolati karakterlánc   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* Szövegként olvasása
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds tooread in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
* Oszlop neveket adhat hozzá és különálló oszlopok
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* Egyes oszlopok toonumeric módosítása
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Machine learning modellek létrehozása
Itt azt hozzon létre egy bináris osztályozási modell toopredict, hogy egy út Formabontó van, vagy nem. A Jupyter Notebook hello található egyéb két modell: multiclass besorolási és regressziós modell.

* Először igazolnia kell toocreate dummy változók scikit használt-modellek tudnivalók
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* Adatok kerete hello modellezési létrehozása
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* Modell betanítására és tesztelésére 60-40 felosztása
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* A gyakorlókészlethez logisztikai regresszió
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* Tesztelési adatkészlet pontozása
  
        Y_test_pred = logit_fit.predict(X_test)
* Értékelési-metrikák kiszámítása
  
        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
  
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
  
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
  
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)
  
       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a>Webszolgáltatási API létrehozása és a Python szokásokra is
Szeretnénk toooperationalize hello gépi tanulási modell, miután be lett építve. Hello bináris logisztikai modell itt példaként használjuk. Győződjön meg arról, hogy hello scikit-további 0.15.1 verziója a helyi gépen. A tooworry nincs Azure ML studio szolgáltatás használatakor.

* A munkaterület hitelesítő adatait az Azure ML studio beállítások megkeresése Az Azure Machine Learning Studióban, kattintson a **beállítások** --> **neve** --> **engedélyezési jogkivonatok**. 
  
    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* Hozzon létre a webszolgáltatás
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* Webes szolgáltatás hitelesítő adatainak lekérése
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* Webszolgáltatási API hívása. Hogy toowait hello előző lépés után 5-10 másodperc.
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>2. lehetőség: Hozzon létre, és közvetlenül az Azure Machine Learning modellek telepítése
Az Azure Machine Learning Studio képes adatokat közvetlenül olvassák be az Azure Data Lake Store majd használt toocreate kell és modellek. Ez a módszer olyan Hive táblát, amely a hello Azure Data Lake Store használja. Ehhez szükséges, hogy egy külön Azure HDInsight-fürt üzembe, mely hello Hive tábla létre van hozva. a következő szakaszok megjelenítése hogyan hello toodo ez. 

### <a name="create-an-hdinsight-linux-cluster"></a>HDInsight Linux-fürtök létrehozása
HDInsight-fürtök (Linux) készíteni hello [Azure Portal](http://portal.azure.com). További információkért lásd: hello **HDInsight-fürtök létrehozása a hozzáférés tooAzure Data Lake Store** szakasz [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Hdinsight Hive tábla létrehozásához
Most már tudjuk létrehozni a Hive táblák toobe hello HDInsight-fürtjéhez hello adataihoz az Azure Data Lake Store hello előző lépésben az Azure Machine Learning Studióban használt. Nyissa meg toohello most hozott létre HDInsight-fürthöz. Kattintson a **beállítások** --> **tulajdonságok** --> **fürt AAD-identitása** --> **ADLS-hozzáférés**, Ellenőrizze, hogy az Azure Data Lake Store-fiók fel van véve hello lista olvasási, írási és végrehajtási jogokat. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

Kattintson a **irányítópult** következő toohello **beállítások** gombra, és egy ablakban jelenik meg. Kattintson a **Hive View** hello jobb felső sarkában hello lapot, és a következő látható: hello **Lekérdezésszerkesztő**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

Illessze be a következő Hive parancsfájlok toocreate hello egy tábla. hello adatforrás helye a ily módon az Azure Data Lake Store-dokumentáció: **adl://data_lake_store_name.azuredatalakestore.net:443/mappanév/Fájlnév**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Hello lekérdezés végeztével ilyen hello eredmény jelenik meg:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Hozza létre és telepítheti az Azure Machine Learning Studióban modellek
Azt most már készen áll a toobuild, és telepítsék a modell, amely képes-e a tipp van fizetős Azure Machine Learning segítségével. hello rétegzett mintaadatok kész, a bináris osztályozási használt toobe (tipp vagy sem) a probléma. hello multiclass besorolásával (tip_class) prediktív modellek és regressziós (tip_amount) is a beépített és az Azure Machine Learning Studio telepített, de itt csak megmutatjuk, hogyan hello bináris osztályozási modell toohandle hello nagybetűk használata.

1. Hello adatok beolvasása az Azure ml hello segítségével **és adatokat importálhat** modul hello elérhető **bemeneti és kimeneti** szakasz. További információkért lásd: hello [és adatokat importálhat modul](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) referencialapja.
2. Válassza ki **Hive-lekérdezések** , hello **adatforrás** a hello **tulajdonságok** panel.
3. A következő Hive parancsfájl hello Beillesztés hello **adatbázis-lekérdezés Hive** szerkesztő
   
        select * from nyc_stratified_sample;
4. Adja meg a hello URI a HDInsight-fürt (Ez található az Azure portálon), a Hadoop hitelesítő adatokat, a hely a kimeneti adatok és az Azure storage fiók nevét vagy kulcstároló neve.
   
   ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Egy bináris osztályozási kísérletet, adatok beolvasása a Hive tábla példa hello az alábbi ábra az.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Hello kísérlet létrehozása után kattintson **webes szolgáltatások beállítása** --> **prediktív webszolgáltatás**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Automatikusan létrehozott futtatási hello pontozási kísérlet, a Befejezés után kattintson **webes szolgáltatás telepítése**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

hello webes szolgáltatás irányítópultját hamarosan fog megjelenni:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a>Összefoglalás
Ez a forgatókönyv végrehajtásával létrehozott egy adatok tudományos környezet méretezhető végpontok közötti megoldások Azure Data Lake készítéséhez. Ebben a környezetben használt tooanalyze véve a hello kanonikus lépéseit az adatgyűjtést keresztül modell betanítási adatok tudományos folyamat, hello a nagy nyilvános adatbázisból lett, és hogy hello toohello telepítését majd modell webszolgáltatásként. U-SQL használt tooprocess volt, és fel az hello adatokat. Python és a Hive az Azure Machine Learning Studio toobuild alkalmazott, és üzembe prediktív modelleket.

## <a name="whats-next"></a>A következő lépések
a képzési hello a [Team adatok tudományos folyamat (TDSP)](http://aka.ms/datascienceprocess) leíró egyes hivatkozások tootopics advanced analytics folyamat hello lépést biztosítja. Nincsenek felsorolva hello a forgatókönyvek több [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) hogyan lapon adott showcase toouse erőforrások és szolgáltatások a prediktív elemzés különféle helyzetekben:

* [hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse](machine-learning-data-science-process-sqldw-walkthrough.md)
* [hello Team adatok tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md)
* [hello csapat az tudományos folyamata: SQL Server használata](machine-learning-data-science-process-sql-walkthrough.md)
* [Az Azure HDInsight Spark hello adatok tudományos folyamat használatának áttekintése](machine-learning-data-science-spark-overview.md)

