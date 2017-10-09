---
title: "az adatok feltárása aaaAdvanced és a Spark modellezési |} Microsoft Docs"
description: "HDInsight Spark toodo adatfeltárás használja, és a kereszt-ellenőrzési és hyperparameter optimalizálással bináris osztályozás és regressziós modell betanításához."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f90d9a80-4eaf-437b-a914-23514390cd60
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 055c342857fd732633cec9810de69cee61db973d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Speciális adatáttekintés és modellezés a Spark segítségével
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ez az útmutató a HDInsight Spark toodo feltárására és vonat bináris adatosztályozási és kereszt-ellenőrzési használatával regressziós modellt használja, és hyperparameter optimalizálása mintán hello NYC út taxiköltség és 2013 dataset díjszabás. Az végigvezeti hello a hello [adatok tudományos folyamat](http://aka.ms/datascienceprocess), -végpontok közötti, egy HDInsight Spark használó fürt feldolgozásra, és Azure-blobok toostore hello adatok és hello modellek. hello folyamat felderíti és az Azure Storage-Blobból adatok visualizes, és ezután előkészíti az hello adatok toobuild prediktív modelleket. Python lett használt toocode hello megoldás és tooshow hello vonatkozó előkészítésére. Ezek a modellek hello Spark MLlib eszközkészlet toodo bináris osztályozás és feladatok modellezési regressziós buildre. 

* Hello **bináris osztályozási** feladata toopredict tipp fizetett hello út van-e. 
* Hello **regressziós** feladata toopredict hello összeg hello tipp más tip szolgáltatások alapján. 

hello modellezési lépéseket is hogyan tootrain, értékelje ki és mentse a modellt különböző típusú kódot tartalmaznak. hello a témakör néhány ugyanaz, mint a hello szabad hello [adatok feltárása és Spark modellezés](machine-learning-data-science-spark-data-exploration-modeling.md) témakör. De azt a több "kiemelt" abban, hogy a kereszt-ellenőrzési az abszolút tootrain optimális pontos besorolási és regressziós modell hyperparameter is használja. 

**Kereszt-ellenőrzési (KtgE)** egy technika, amely értékeli a milyen mértékben a modellek betanítása adatokat egy ismert csoportján használatúvá toopredicting hello részeit, amelyen az még nincs betanítva adatkészletek.  Egy közös megvalósított toodivide K modellrészt az adatkészletet, és ezután a ciklikus multiplexeléssel a egy hello modellrészt hello modell betanításához. hello modell tooprediction pontosan elleni hello független adatkészlet a nem használt modellrészek tootrain hello modellben tesztelésekor hello képességét megfelelőségét ellenőrizni kell.

**Hyperparameter optimalizálási** hello probléma általában a hello célja egy független adatkészlet hello algoritmus teljesítményének biztosítása optimalizálása a tanulási algoritmus hiperparamétereket készlete kiválasztása. **Hiperparamétereket** olyan értékek, amelyek kívül hello modell betanítási eljárást meg kell adni. Ezek az értékek feltételezéseket befolyásolhatja a hello rugalmasságot és hello modellek pontosságát. Döntési fák például rendelkezik hiperparamétereket, például a hello szükséges mélységében és hello fában leaves száma. Támogatási vektoros gépeknél (SVMs) szükség van a téves besorolás szövegminősítési kifejezés beállítását. 

A közös módon tooperform hyperparameter optimalizálása itt használt rács keresést, vagy egy **paraméter ismétlés**. Ez a tanulási algoritmus hello hyperparameter terület megadott részhalmazának keresztül hello értékek részletes keresést végrehajtásához áll. Keresztellenőrzési megadhatja a teljesítmény metrika toosort hello optimális eredmények hello rács keresési algoritmus által előállított ki. KtgE használt hyperparameter elvégzésekor mindig segít korlátot problémák, például overfitting egy modell tootraining adatokat, így hello modell olyan hello kapacitás tooapply toohello általános adatok készletét, mely hello betanítási adatok ki lett olvasni.

hello modellek használjuk logisztikai és lineáris regressziós, véletlenszerű erdők és átmenetes súlyozott fák tartalmazza:

* [A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modellt, amely Stochastic átmenetes módszeren (SGD) módszerrel és optimalizálási, valamint a szolgáltatás toopredict hello tipp összegek skálázás fizetett. 
* [A LBFGS logisztikai regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regresszió egy regressziós modell kategorikus toodo adatbesorolást hello függő változó esetén használható. LBFGS egy látszólagos Newton optimalizálási algoritmus, amely megközelíti hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus csak korlátozott mennyiségű memóriát használ, és a gépi tanulás széles körben használt.
* [Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.  Sok döntési fák algoritmus tooreduce hello kockázatát overfitting össze azokat. Véletlenszerű erdők regressziós és besorolási használ, és kezelni tud a kategorikus funkciók és bővíthető toohello multiclass adatbesorolás beállításai. Nincs szükségük szolgáltatás méretezés és a képes toocapture nemlinearitás és a szolgáltatás kapcsolatait. Véletlenszerű erdők hello legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.
* [Színátmenet súlyozott fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák. GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt. GBTs regressziós és besorolási használ és kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció. Is is szerepel a multiclass-adatbesorolás beállításai.

Példák KtgE és Hyperparameter modellezési ismétlés hello bináris osztályozási problémához láthatók. Egyszerűbb (nélkül paraméter el) be hello fő témakört regressziós feladatokhoz. De hello függelékben használata rugalmas net lineáris regressziós és KtgE az véletlenszerű erdő regressziós használatával ismétlés paraméter érvényesítése is ismertet. Hello **nettó rugalmas** rendeződik regressziós metódus az illesztés lineáris regressziós modellek, amelyek lineárisan egyesíti az 1. és 2. szintű metrikák hello hello az eljárást, [szabadkézi](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) és [peremmel](https://en.wikipedia.org/wiki/Tikhonov_regularization) módszerek.   

> [!NOTE]
> Bár hello Spark MLlib eszközkészlet tervezett toowork a nagy adatkészleteket, viszonylag kis minta (KB. 30 Mb használatával 170K sorok, hello eredeti NYC adatkészlet hamarosan 0,1 %) használható itt kényelmét szolgálja. az alábbi hello a gyakorlatban 2 munkavégző csomópontokhoz a HDInsight-fürtök a hatékony (KB) a fut. hello ugyanazt a kódot, kisebb módosításokkal lehet használt tooprocess nagyobb-adatmennyiség memóriájában lévő adatok gyorsítótárazása és hello fürt méretének módosítása megfelelő módosításával.
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a>A telepítő: A Spark-fürtök és notebookok
Beállítási lépéseket és kód okat ebben a forgatókönyvben egy HDInsight Spark 1.6 használatával. De Jupyter notebookok HDInsight Spark 1.6-os és a Spark 2.0 fürtök rendelkeznek. Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban. Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek. Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható. Kényelmi célokat szolgál az alábbiakban hello hivatkozások toohello Jupyter notebookok Spark 1.6-os és futtatása a Jupyter Notebook server hello hello pyspark kernel 2.0 toobe:

### <a name="spark-16-notebooks"></a>Spark 1.6-os notebookok

[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): #1 jegyzetfüzet és modell fejlesztési hyperparameter hangolása és kereszt-ellenőrzési témaköröket tartalmazza.

### <a name="spark-20-notebooks"></a>Spark 2.0 notebookok

[Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok feltárása, modellezés és Spark 2.0 pontozási fürtök.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a>Beállítása: tárolóhelyek, könyvtárak és hello beállított Spark környezet
A Spark az képes tooread és írási tooAzure tárolási Blob (más néven WASB). Így a meglévő tárolt adatok dolgozhatók használata Spark, hello tárolt újra WASB eredménye.

toosave modellek vagy WASB fájlokat, a hello elérési utat kell toobe-e megadva. hello alapértelmezett tároló csatolt toohello Spark-fürt lehet hivatkozni egy elérési utat kezdetű használatával: "wasb: / / /". Más helyeken által hivatkozott "wasb: / /".

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Tárolási helye a WASB set könyvtár elérési útja
hello alábbi kódminta helyét adja meg hello hello adatok toobe olvassa el, és mentett hello hello modell tároló könyvtár toowhich hello modell kimeneti elérési útja:

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**KIMENETI**

DateTime.DateTime (2016, 4, 18., 17, 36, 27 és 832799)

### <a name="import-libraries"></a>Könyvtárak importálása
Importálás a következő kód hello szükséges könyvtárak:

    # LOAD PYSPARK LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Spark és az PySpark magics az adott néven beállítás
Jupyter notebookok kapnak hello PySpark kernelt beállításkészletet kontextusban rendelkezik. Így nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon fejleszt hello alkalmazás használatának megkezdése előtt. Ezek a környezetek érhetők el, alapértelmezés szerint. Ezek a környezetek a következők:

* sc - a Spark 
* az sqlContext - struktúra

hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%. Nincsenek két ilyen parancsot a következő kód mintákat használt.

* **%% helyi** Megadja, hogy egymás utáni sorok hello kód helyben végrehajtott toobe. Kód érvényes Python-kódot kell lennie.
* **%% sql -o <variable name>**  végrehajtja a Hive-lekérdezések hello az sqlContext ellen. Hello -o paramétert fogad el, ha a hello hello lekérdezés eredménye hello megőrződjenek %%, egy Pandas DataFrame helyi Python-környezetben.

További információ a Jupyter notebookokból és az előre megadott hello hello kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="data-ingestion-from-public-blob"></a>A nyilvános blob adatfeldolgozást:
hello hello adatok tudományos folyamat első lépéseként tooingest hello adatok toobe helyét az adatok feltárása és modellezési környezetbe forrásokból elemzése. Ebben a környezetben a forgatókönyv külső. Ez a szakasz hello kód toocomplete feladatok sorozatát:

* betöltési hello adatok minta toobe modellezése
* olvassa el a hello bemeneti adatkészletet (.tsv fájlként tárolja)
* formátum és tiszta hello adatok
* Hozzon létre és objektumok (RDDs vagy adatkeretek) a memóriában gyorsítótárazása
* regisztrálja az SQL-környezetben temp-táblázatként.

Itt található adatfeldolgozást hello kódját.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES hello DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

Idő tooexecute cella fent: 276.62 másodperc

## <a name="data-exploration--visualization"></a>Az adatok feltárása & képi megjelenítés
Miután hello adatok Spark üzembe, hello hello adatok tudományos folyamat következő lépésének nem toogain bemutatják hello az adatok feltárása és -megjelenítésre. Ez a szakasz azt hello taxi adatokat, az SQL-lekérdezések és a rajzolási hello cél változók és a potenciális szolgáltatások visual hálózatfelügyeleti vizsgálja meg. Pontosabban azt megrajzolásához utas számát taxi utakat, hello gyakoriságát tipp díjak, és hogyan tippek változhat fizetési mennyiségét, és írja be a hello gyakoriságát.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a>A hisztogram hello mintában taxi utak utas száma gyakoriságértékek listáját ábrázolása
A kód és a későbbi kódtöredékek SQL magic tooquery hello minta és helyi magic tooplot hello adatok használni.

* **SQL magic (`%%sql`)** hello HDInsight PySpark kernel lekérdezéseket támogat, könnyen beágyazott HiveQL hello az sqlContext ellen. hello (-o VARIABLE_NAME) argumentum hello kimeneti hello SQL-lekérdezés egy Pandas DataFrame hello Jupyter kiszolgálón, továbbra is fennáll.. Ez azt jelenti, hogy hello helyi módban érhető el.
* Hello  **`%%local` magic** van toorun kód helyileg használt hello Jupyter kiszolgálón, amely hello headnode hello HDInsight-fürt. Általában akkor használják `%%local` magic után hello `%%sql -o` magic használt toorun lekérdezést. hello -o paraméter szeretné megőrizni a hello SQL lekérdezés helyileg hello kimenetét. Majd hello `%%local` magic eseményindítók hello kód kódtöredékek toorun helyileg elleni hello kimeneti hello SQL lekérdezések helyileg megőrzött következő készletét. hello kimeneti automatikusan történik, hello kód futtatása után.

Ez a lekérdezés lekéri a hello utazgatással utas száma szerint. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Ez a kód létrehoz egy helyi adatok-keret hello lekérdezés kimeneti és tevékenységtérkép hello adatok. Hello `%%local` magic létrehoz egy helyi adatok-keret, `sqlResults`, a matplotlib ábrázolásához használható. 

> [!NOTE]
> A PySpark magic ebben a bemutatóban több alkalommal van használva. Nagy adatmennyiség hello esetén meg kell minta toocreate elfér a helyi memória adatok-keret.
> 
> 

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Íme hello kód tooplot hello utazgatással utas száma

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**KIMENETI**

![Gyakoriság utak utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Hello segítségével számos különböző típusú megjelenítések (táblázat, torta, vonal, terület vagy sáv) választható **típus** hello Jegyzetfüzet menü gombokat. hello sáv rajzot itt jelenik meg.

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Tipp összegek és hogyan függ a tipp összeg utas számát és a jegy ára összegek hisztogram ábrázolásához.
Egy SQL-lekérdezés toosample adatokat használja.

    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25


Ez a kód cella hello SQL lekérdezés toocreate három előkészítésére hello adatokat használ.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


**A KIMENETRE:** 

![Tipp összeg terjesztési](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp összeg jegy ára mennyiség szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>A funkció modellezési mérnöki csapathoz, átalakítás és adatok előkészítése
Ez a szakasz ismerteti és eljárások hello kódját használt tooprepare adatokat ML modellezési használható. Azt illusztrálja, hogyan toodo hello a következő feladatokat:

* Hozzon létre egy új szolgáltatás üzemideje (óra) a forgalom idő bins particionálás
* Index és a közbeni kódolása kategorikus szolgáltatások
* Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása
* Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési
* A szolgáltatás skálázás
* A memóriában objektumok

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a>Hozzon létre egy új szolgáltatás a bins forgalom alkalommal particionálás
Ez a kód bemutatja, hogyan toocreate forgalom felosztásával új szolgáltatása alkalommal fordult elő a bins, majd hogyan toocache hello eredményül kapott adatok keret a memóriában. Gyorsítótárazás vezet tooimproved végrehajtási idő ahol elosztott rugalmas adatkészleteket (RDDs) és adatkeretek használják ismételten. Igen azt a gyorsítótár RDDs és adatkeretek, a forgatókönyv több lépésből áll.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # hello .COUNT() GOES THROUGH hello ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES hello COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**KIMENETI**

126050

### <a name="index-and-one-hot-encode-categorical-features"></a>Index és egy közbeni kódolása kategorikus szolgáltatások
Ez a szakasz bemutatja, hogyan tooindex vagy funkciók modellezési hello a bemenő kategorikus funkciói kódolása. hello modellezési és MLlib feladatai szükséges szolgáltatások kategorikus bemeneti adatokkal indexelt vagy kódolású előrejelzése előzetes toouse. 

Hello modelltől függően tooindex kell, vagy más módon kódolás. Például Logistic és lineáris regressziós modell igényelnek egy közbeni kódolását, ha, például három kategóriába szolgáltatás minden egyes tartalmazó 0 vagy 1 attól függően, hogy egy megfigyelési hello kategória három funkció oszlopokba bővíthetők. MLlib biztosít [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) toodo egy közbeni kódolás működik. A kódoló leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz. Ez a kódolás lehetővé teszi, hogy a várt numerikus fontos funkciók, például logisztikai regresszió algoritmusokat alkalmazott toobe toocategorical szolgáltatásokat.

Itt található kód tooindex hello és kódolása kategorikus szolgáltatások:

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

Idő tooexecute cella fent: 3.14 másodpercben

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása
Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adattípushoz címkézett pont adatokat, és hogyan kód tooencode azt. Ez felkészíti használt toobe tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell. Címkézett pont objektum rugalmas elosztott adatkészletek (RDD) formázott oly módon, hogy a bemeneti adatként ML algoritmusok MLlib a legtöbb van szükség. A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.

Itt található kód tooindex hello és bináris osztályozási funkciói szöveg kódolása.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Ez hello kód tooencode és index kategorikus szöveg szolgáltatások lineáris regressziós elemzés céljából.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a>Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési
Ez a kód hoz létre egy véletlenszerű mintavételi hello adatok (25 % használata). Bár nem szükséges ehhez a példához hello dataset toohello mérete miatt, bemutatjuk, hogyan lehet mintát itt hello adatok. Akkor tudja hogyan toouse a saját probléma, ha szükséges. Ha minták nagy, ez is tanítási modell jelentős időt takaríthat meg. Ezután azt felosztása hello minta képzési része (Itt 75 %) és egy tesztelési részét (25 %-os itt) toouse besorolási és regressziós modellezéshez hatékony.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI**

Idő tooexecute cella fent: 0.31 másodperc

### <a name="feature-scaling"></a>A szolgáltatás skálázás
Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy széles körben folyósított értékekkel funkciók vannak nem a megadott túlzott mérjük hello objektív függvényben. hello szolgáltatás méretezéshez használ a hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello szolgáltatások toounit eltérése. Ez biztosítja MLlib lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD) használható. SGD egy népszerű más gépi tanulási modellek például rendeződik regresszió vagy támogatási vektoros gépek (SVM) számos különféle képzési algoritmus.   

> [!TIP]
> Találtunk, amelyeknek hello LinearRegressionWithSGD algoritmus toobe bizalmas toofeature méretezés.   
> 
> 

Itt található hello kód tooscale változók rendeződik hello lineáris SGD algoritmus való használatra.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI**

Idő tooexecute cella fent: 11.67 másodperc

### <a name="cache-objects-in-memory"></a>A memóriában objektumok
hello idő betanításra és tesztelésre ML algoritmusok hello bemeneti adatok keret objektumok az osztályozás, regressziós és szolgáltatásokat méretezni gyorsítótárazásával csökkenthető.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI** 

Idő tooexecute cella fent: 0.13 másodperc

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Függetlenül attól, tipp bináris osztályozási modellekkel való fizetnek előrejelzése
Ez a szakasz bemutatja, hogyan használható három modelleket használnak hello bináris osztályozási feladatát előrejelzésére tipp taxi útnak fizetnek-e. hello modell jelenik meg a következő:

* Logisztikai regresszió 
* Véletlenszerű erdő
* Átmenet kiemelési fák

Egyes modellek kód szakasz felépítése lépéseket oszlik: 

1. **A modell betanítási** egy paraméterhalmaz adatok
2. **A modell kiértékelése** a metrikák a teszt adatkészlet
3. **Modell mentése** BLOB a jövőbeni felhasználásához

Megmutatjuk, hogyan toodo kereszt-ellenőrzési (KtgE) két módon abszolút paraméterrel:

1. Használatával **általános** algoritmusban beállítja egyéni kód, amely lehet alkalmazott tooany algoritmus MLlib és tooany paraméterben. 
2. Hello segítségével **pySpark CrossValidator csővezeték függvény**. Vegye figyelembe, hogy CrossValidator Spark 1.5.0 néhány korlátozásai: 
   
   * Adatcsatorna modellek nem lehet menteni, megőrzött állapotú későbbi felhasználásra.
   * Nem használható a modellben minden paraméterhez.
   * Nem minden MLlib algoritmus használható.

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-hello-logistic-regression-algorithm-for-binary-classification"></a>Érvényesítési és a hyperparameter abszolút bináris osztályozási hello logisztikai regresszió algoritmussal használt általános
Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentse egy logisztikai regresszió modell [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak. hello modell betanítása közötti érvényesítési (KtgE) és hyperparameter abszolút egyéni kód, amely a tanulási algoritmus a MLlib hello alkalmazott tooany megvalósítva.   

> [!NOTE]
> az egyéni KtgE kód hello végrehajtása több percig is eltarthat.
> 
> 

**Hello logisztikai regresszió tanítási KtgE és hyperparameter abszolút használatával**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS tooSWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

Együttható: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

INTERCEPT:-0.0111216486893

Idő tooexecute cella fent: 14.43 másodperc

**Standard metrikák hello bináris osztályozási modell kiértékelése**

Ebben a szakaszban hello kód bemutatja, hogyan tooevaluate logisztikai regresszió modellhez tartozó adatok-TesztKészlet, beleértve a hello: ROC-görbe rajzot ellen.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

Törlés a ka területen = 0.985336538462

ROC területen = 0.983383274312

Összesített statisztikák

Pontosság = 0.984174341679

Visszahívása = 0.984174341679

F1 Pontozása = 0.984174341679

Idő tooexecute cella fent: 2.67 másodpercben

**Hello: ROC-görbe ábrázolásához.**

Hello *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, hello előző cella. *tmp_results* is használt toodo lekérdezések és eredmények kimeneti keretbe hello sqlResults adatok-ábrázolásához. Itt található hello kódot.

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Ez hello kód toomake előrejelzéseket és rajzot hello: ROC-görbe.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**KIMENETI**

![Logisztikai regresszió: ROC-görbe általános módszer](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

**A jövőbeni felhasználásához blob modellt megőrzése**

Ebben a szakaszban hello kód bemutatja, hogyan toosave hello logisztikai regresszió modell felhasználásra.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";


**KIMENETI**

Idő tooexecute cella fent: 34.57 másodperc

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>MLlib tartozó CrossValidator csővezeték függvény használata logisztikai regresszió (rugalmas regresszió) modell
Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentse egy logisztikai regresszió modell [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak. hello modell betanítása keresztellenőrzési (KtgE) használatával, és hyperparameter abszolút megvalósítva a hello MLlib CrossValidator csővezeték függvény a KtgE paraméter ismétlés.   

> [!NOTE]
> a MLlib KtgE kód hello végrehajtása több percig is eltarthat.
> 
> 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**KIMENETI**

Idő tooexecute cella fent: 107.98 másodperc

**Hello: ROC-görbe ábrázolásához.**

Hello *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, hello előző cella. *tmp_results* is használt toodo lekérdezések és eredmények kimeneti keretbe hello sqlResults adatok-ábrázolásához. Itt található hello kódot.

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Itt található hello kód tooplot hello: ROC-görbe.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**KIMENETI**

![Logisztikai regresszió: ROC-görbe MLlib tartozó CrossValidator használatával](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a>Véletlenszerű erdő besorolás
Ebben a szakaszban hello kód mutatja be, hogyan tootrain, értékelje ki, és mentse egy véletlenszerű erdő regressziós, amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

ROC területen = 0.985336538462

Idő tooexecute cella fent: 26.72 másodperc

### <a name="gradient-boosting-trees-classification"></a>Átmenet kiemelési fák besorolás
Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és átmenetes kiemelési fák modell, amely képes-e tipp hello NYC taxi út és a jegy ára adatkészlet útnak fizetnek mentése.

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI**

ROC területen = 0.985336538462

Idő tooexecute cella fent: 28.13 másodpercben

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Tipp összeg előrejelzése regressziós modellek (nem használ KtgE)
Ez a szakasz bemutatja, hogyan használható három modellek hello regressziós feladathoz: hello tipp kifizetett taxi útnak más tip szolgáltatások alapján előre jelezni. hello modell jelenik meg a következő:

* Lineáris regressziós rendeződik
* Véletlenszerű erdő
* Átmenet kiemelési fák

Ezek a modellek leírt hello bevezetés. Egyes modellek kód szakasz felépítése lépéseket oszlik: 

1. **A modell betanítási** egy paraméterhalmaz adatok
2. **A modell kiértékelése** a metrikák a teszt adatkészlet
3. **Modell mentése** BLOB a jövőbeni felhasználásához   

> Az AZURE Megjegyzés: Kereszt-ellenőrzési nem használható hello három regressziós modellt ebben a szakaszban mivel ennek részletes hello logisztikai regresszió modellek látható volt. Egy példa látható, hogyan toouse a rugalmas nettó KtgE lineáris regressziós a hello függelék – az ebben a témakörben találhatók.
> 
> AZURE Megjegyzés: Az a tapasztalat lehet LinearRegressionWithSGD modellek konvergencia problémákat, és a paraméterek kell toobe megváltozott/optimalizált gondosan érvényes modellt kéréséhez. A változók jelentősen skálázás elősegíti a konvergencia. Rugalmas nettó regressziós hello függelék toothis témakörben látható LinearRegressionWithSGD helyett is használható.
> 
> 

### <a name="linear-regression-with-sgd"></a>A SGD lineáris regressziós
hello kód ebben a szakaszban bemutatja, hogyan toouse méretezhető szolgáltatások tootrain egy lineáris regressziós stochastic átmenetes módszeren (SGD) használó optimális, és hogyan tooscore, értékelje ki, és mentse hello modellt az Azure Blob Storage (WASB).

> [!TIP]
> Az a tapasztalat hello konvergencia LinearRegressionWithSGD modellek problémái lehetnek, és paraméterekkel kell toobe megváltozott/optimalizált gondosan kéréséhez érvényes modellt. A változók jelentősen skálázás elősegíti a konvergencia.
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES tooTRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI**

Együttható: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

INTERCEPT: 0.854507624459

GYÖKÁTLAGOS = 1.23485131376

R-sqr = 0.597963951127

Idő tooexecute cella fent: 38.62 másodperc

### <a name="random-forest-regression"></a>Véletlenszerű erdő regressziós
Ebben a szakaszban hello kód mutatja be, hogyan tootrain, értékelje ki, és mentse egy véletlenszerű erdő modellt, amely képes tipp hello NYC taxi út adatok mennyiségét.   

> [!NOTE]
> Kereszt-ellenőrzési abszolút egyéni kód használatával paraméterrel hello függelékben valósul meg.
> 
> 

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**KIMENETI**

GYÖKÁTLAGOS = 0.931981967875

R-sqr = 0.733445485802

Idő tooexecute cella fent: 25.98 másodperc

### <a name="gradient-boosting-trees-regression"></a>Átmenet kiemelési fák regressziós
Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentése átmenetes kiemelési fák modell, amely képes tipp hello NYC taxi út adatok mennyiségét.

** Betanítása és kiértékelése **

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

GYÖKÁTLAGOS = 0.928172197114

R-sqr = 0.732680354389

Idő tooexecute cella fent: 20.9 másodperc

**Ábrázolása**

*tmp_results* hello előző cella Hive tábla néven van regisztrálva. A hello kimeneti eredményei hello táblából *sqlResults* adatok-keret ábrázolásához. Íme hello kódot

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Itt az hello kód tooplot hello adatai hello Jupyter kiszolgáló használatával.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Tényleges-vs-előre jelezni-tipp-díjak](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>A függelék: További regressziós feladatok keresztellenőrzési használata paraméter el
E függelék tartalmaz kód ábrázoló hogyan toodo KtgE rugalmas net használata lineáris regresszió, és hogyan toodo KtgE paraméterrel sweep véletlenszerű erdő regressziós az egyéni kód használatával.

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Kereszt-nettó rugalmas használatát lineáris regressziós érvényesítése
Ebben a szakaszban hello kód mutatja be, hogyan toodo kereszt-érvényesítési lineáris regressziós nettó rugalmas használ, és hogyan tooevaluate hello modell vizsgálati adatok alapján.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY hello MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses hello best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT tooDF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

Idő tooexecute cella fent: 161.21 másodperc

**R-SQR metrikájú kiértékelése**

*tmp_results* hello előző cella Hive tábla néven van regisztrálva. A hello kimeneti eredményei hello táblából *sqlResults* adatok-keret ábrázolásához. Íme hello kódot

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Itt található hello kód toocalculate R-sqr.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**KIMENETI**

R-sqr = 0.619184907088

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Kereszt-ellenőrzési az egyéni kód használatával a véletlenszerű erdő regressziós paraméter ismétlés
Ebben a szakaszban hello kód mutatja be, hogyan toodo kereszt-ellenőrzési paraméter ismétlés egyéni kód használatával véletlenszerű erdő visszavonták, és hogyan tooevaluate hello modell vizsgálati adatok alapján.

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY tooHOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**KIMENETI**

GYÖKÁTLAGOS = 0.906972198262

R-sqr = 0.740751197012

Idő tooexecute cella fent: 69.17 másodperc

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Memória és a nyomtatási modell helyekről objektumainak eltávolítása
Használjon `unpersist()` jelenleg a memóriában lévő toodelete objektumok.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


**KIMENETI**

[122] PythonRDD RDD PythonRDD.scala a következő: 43

** Nyomtatás elérési toomodel fájlok toobe hello fogyasztás notebook szerepel. ** tooconsume és pontszám független-adatkészlet, toocopy kell, és illessze be a következő fájl neve a "Használat notebook" hello.

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**KIMENETI**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>A következő lépések
Most, hogy a korábban létrehozott regressziós és besorolási modell hello Spark MlLib, áll készen toolearn hogyan tooscore és ezek a modellek kiértékeléséhez.

**Modellhez tartozó felhasználás:** toolearn hogyan tooscore és értékelje ki a létrehozott ebben a témakörben hello besorolási és regressziós modell, lásd: [pontszám és értékelje ki a Spark-beépített machine learning modellek](machine-learning-data-science-spark-model-consumption.md).

