---
title: "aaaOperationalize Spark-beépített gépi tanulási modellek |} Microsoft Docs"
description: "Hogyan tooload tanulási modellek pontszám tárolja és az Azure Blob Storage (WASB) Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 626305a2-0abf-4642-afb0-dad0f6bd24e9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: c5fadcb13257b94dcb28a522be454f6e03dfa991
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a>Azok a Spark-beépített machine learning modellek
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ez a témakör bemutatja, hogyan toooperationalize a mentett gépi tanulási modell (ML) Python használata a HDInsight Spark-fürtök. Leírja, hogyan tooload Spark MLlib használatával készített, és az Azure Blob Storage (WASB) tárolt tanulási modelljeit gépre, és hogyan tooscore WASB is megtalálható adatkészletekkel őket. Azt illusztrálja, hogyan toopre-folyamat hello bemeneti adatokat, átalakító funkciók hello indexelési és kódolási funkcióinak hello MLlib eszközkészlet, és hogyan toocreate egy címkézett pont objektumot, amelyek változtatás nélkül használhatók bemeneti pontozó hello ML modellekkel. pontozó használt hello modellek lineáris regresszió, logisztikai regresszió, véletlenszerű erdő modellek és átmenetes kiemelése fa modellek tartalmaznak.

## <a name="spark-clusters-and-jupyter-notebooks"></a>A Spark-fürtök és a Jupyter notebookok
A telepítő lépéseket és hello kód toooperationalize az ML-modell-okat a forgatókönyv az 1.6-os Spark on HDInsight-fürt, valamint a Spark 2.0 fürt használatával. Ezek az eljárások hello kódját Jupyter notebookok is megtalálható.

### <a name="notebook-for-spark-16"></a>A Spark 1.6-os notebook
Hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook bemutatja, hogyan toooperationalize egy mentett modell Python használata a HDInsight-fürtök. 

### <a name="notebook-for-spark-20"></a>A Spark 2.0 notebook
toomodify hello Jupyter notebook Spark 1.6 toouse 2.0 a HDInsight Spark-fürttel, cserélje le a hello Python kódját fájl [ezt a fájlt](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py). A kód bemutatja, hogyan tooconsume hello modellek hozza létre a Spark 2.0.


## <a name="prerequisites"></a>Előfeltételek

1. Az Azure-fiók és a Spark 1.6-os (vagy külső 2.0) HDInsight-fürtöt toocomplete Ez a forgatókönyv. Lásd: hello [áttekintése adatok tudományos Spark on Azure HDInsight használatának](machine-learning-data-science-spark-overview.md) útmutatást toosatisfy ezeket a követelményeket. Témakör hello itt használt NYC 2013 Taxi adatok, valamint útmutatást a hogyan tooexecute hello Spark-fürt a Jupyter notebook a kód leírását is tartalmazza. 
2. Akkor is létre kell hoznia hello machine learning modellek toobe program pontozza a mennyiségeket itt által feldolgozása révén hello [adatok feltárása és Spark modellezés](machine-learning-data-science-spark-data-exploration-modeling.md) hello 1.6-os Spark-fürt vagy hello Spark 2.0 notebookok témakör. 
3. hello Spark 2.0 notebookok használata további adatok hello besorolás feladat hello jól ismert légitársaság időben indító dataset 2012 és a 2011. Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban. Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek. Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható. 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a>Beállítása: tárolóhelyek, könyvtárak és hello beállított Spark környezet
A Spark az Azure tárolási Blob (WASB) képes tooread és írási tooan. Így a meglévő tárolt adatok dolgozhatók használata Spark, hello tárolt újra WASB eredménye.

toosave modellek vagy WASB fájlokat, a hello elérési utat kell toobe-e megadva. hello alapértelmezett tároló csatolt toohello Spark-fürt lehet hivatkozni egy elérési utat kezdetű használatával: *"wasb / /"*. hello alábbi kódminta helyét adja meg hello hello adatok toobe olvassa el és mentett hello hello modell tároló könyvtár toowhich hello modell kimeneti elérési útját. 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Tárolási helye a WASB set könyvtár elérési útja
Modellek lesznek mentve: "wasb: / / / felhasználó/remoteuser/NYCTaxi/modellek". Ha az elérési út nem megfelelően van beállítva, a modellek nem töltődnek pontozó.

hello pontozott eredmények lett mentve: "wasb: / / / felhasználó/remoteuser/NYCTaxi/ScoredResults". Hello elérési toofolder nem megfelelő, ha az eredmények nincsenek mentve a mappában.   

> [!NOTE]
> hello fájl meghajtóútvonal-helyeit másolható és illeszthetők be ezt a kódot a hello kimenet utolsó cella hello a hello hello helyőrzőt **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebookot.   
> 
> 

Íme hello kód tooset directory elérési utak: 

    # LOCATION OF DATA tooBE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR hello MODELS tooBE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**A KIMENETRE:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)

### <a name="import-libraries"></a>Könyvtárak importálása
Spark környezet beállítása és a következő kód hello szükséges könyvtárak importálása

    #IMPORT LIBRARIES
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
Jupyter notebookok kapnak hello PySpark kernelt beállításkészletet kontextusban rendelkezik. Így nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon fejleszt hello alkalmazás használatának megkezdése előtt. Elérhetők az Ön alapértelmezés szerint. Ezek a környezetek a következők:

* sc - a Spark 
* az sqlContext - struktúra

hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%. Nincsenek két ilyen parancsot a következő kód mintákat használt.

* **%% helyi** megadott helyileg hajtotta végre az egymás utáni sorok hello kódot. Kód érvényes Python-kódot kell lennie.
* **%% sql -o<variable name>** 
* Végrehajtja a Hive-lekérdezések hello az sqlContext ellen. Hello -o paramétert fogad el, ha a hello hello lekérdezés eredménye hello megőrződjenek %%, egy Pandas dataframe helyi Python-környezetben.

További információ a Jupyter notebookokból és az előre megadott hello hello kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Adatok és megtisztított adatok keret létrehozása
Ez a szakasz feladatok szükséges tooingest hello adatok toobe program pontozza a mennyiségeket sorozata hello kódját tartalmazza. Hello adatok formázása illesztett 0,1 % minta hello taxi út és a jegy ára fájl (.tsv fájlként tárolja), olvasása, és létrehoz egy tiszta adatok keret.

hello taxi út és a jegy ára fájlok alapján volt csatlakoztatva a megadott hello eljárás a: [Team adatok tudományos folyamat hello működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md) témakör.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_test_file.first()
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

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**A KIMENETRE:**

Idő tooexecute cella fent: 46.37 másodperc

## <a name="prepare-data-for-scoring-in-spark"></a>Készítse elő az adatokat a Spark pontozó
Ez a szakasz bemutatja, hogyan tooindex, kódolására, és azokat használni a felügyelt MLlib tanulási algoritmusok besorolási és regressziós kategorikus szolgáltatások tooprepare méretezni.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Átalakítás funkció: index és modellek pontozó a bemenő kategorikus funkciói kódolása
Ez a szakasz bemutatja, hogyan tooindex kategorikus adatok segítségével egy `StringIndexer` és a funkcióinak kódolása `OneHotEncoder` hello modellek bemeneteként.

Hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) egy karakterlánc-oszlopnak címkék tooa oszlop címke indexek kódolja. címke gyakoriságot szerint rendezett hello indexet. 

Hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz. Ez a kódolás lehetővé teszi, hogy a várt folyamatos fontos funkciók, például logisztikai regresszió algoritmusokat alkalmazott toobe toocategorical szolgáltatásokat.

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**A KIMENETRE:**

Idő tooexecute cella fent: 5.37 másodperc

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>A szolgáltatás-tömböket tartalmazó bemeneti be modellek RDD objektumok létrehozása
Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adatait, mivel az egy RDD objektumot, és egy közbeni kódolása, azok használt tootrain és tesztelési MLlib logisztikai regresszió és -modellek fa-alapú kódját tartalmazza. hello indexelt adatai [rugalmas elosztott Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objektumok. Ezek a hello alapvető absztrakciós Spark. RDD objektum egy, a Spark párhuzamosan is üzemeltetnek elemek nem módosítható, particionált gyűjteményét képviseli.

Azt is kódot tartalmaz, amely bemutatja, hogyan tooscale adatok hello `StandardScalar` MLlib által előírt lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD), a számos machine learning modellek betanítása népszerű algoritmust használja. Hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) használt tooscale hello szolgáltatások toounit variancia. Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy széles körben folyósított értékekkel funkciók vannak nem a megadott túlzott mérjük hello objektív függvényben. 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**A KIMENETRE:**

Idő tooexecute cella fent: 11.72 másodperc

## <a name="score-with-hello-logistic-regression-model-and-save-output-tooblob"></a>A hello logisztikai regressziós modell pontozása, és mentse a kimeneti tooblob
Ebben a szakaszban hello kód bemutatja, hogyan tooload egy logisztikai regressziós modellt az Azure-ban korábban mentett blob-tároló és taxi utazás toopredict tipp fizetnek-e használni, azt a szabványos besorolás metrikák pontozása, és mentse és hello eredmények tooblob megrajzolásához tárolás. eredmények pontozni hello RDD objektumok vannak tárolva. 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) tooBLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**A KIMENETRE:**

Idő tooexecute cella fent: 19.22 másodperc

## <a name="score-a-linear-regression-model"></a>Egy lineáris regressziós modell pontozása
Használtuk [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain egy lineáris regressziós modellt Stochastic átmenetes módszeren (SGD) használatával optimalizálási toopredict hello időtartamig tipp fizetett. 

Ebben a szakaszban hello kód bemutatja, hogyan tooload egy lineáris regressziós modellt az Azure blob storage pontozása méretezett változók használata, és mentse a hello eredményeit, hátsó toohello blob.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


**A KIMENETRE:**

Idő tooexecute cella fent: 16.63 másodperc

## <a name="score-classification-and-regression-random-forest-models"></a>Besorolás és regressziós véletlenszerű erdő modell pontozása
Ebben a szakaszban hello kód bemutatja, hogyan tooload hello mentett besorolási és regressziós mentése az Azure blob storage véletlenszerű erdő modellek pontozása szabványos osztályozó és regressziós intézkedések teljesítményét, és mentse a hello eredmények hátsó tooblob tároló.

[Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.  Sok döntési fák algoritmus tooreduce hello kockázatát overfitting össze azokat. Véletlenszerű erdők kezelni tud a kategorikus szolgáltatások kiterjesztése toohello multiclass adatbesorolás beállításai, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció. Véletlenszerű erdők hello legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.

[Spark.mllib](http://spark.apache.org/mllib/) támogatja az erdők véletlenszerű multiclass és bináris besorolási regressziós folyamatos és kategorikus funkciókat használ. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

**A KIMENETRE:**

Idő tooexecute cella fent: 31.07 másodperc

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Besorolás és regressziós átmenetes kiemelése fa modell pontozása
Ebben a szakaszban hello kód mutatja be, hogyan tooload besorolási és regressziós átmenetes kiemelése fa modellek az Azure blob storage pontozása szabványos osztályozó és regressziós intézkedések teljesítményét, és mentse a hello eredmények hátsó tooblob tároló. 

**Spark.mllib** támogatja az GBTs bináris osztályozási regressziós folyamatos és kategorikus funkciókat használ. 

[Átmenet kiemelése fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák. GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt. GBTs kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció. Is is szerepel a multiclass-adatbesorolás beállításai.

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    #LOAD AND SCORE hello MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

**A KIMENETRE:**

Idő tooexecute cella fent: 14.6 másodperc

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>A memóriából objektumainak eltávolítása, és nyomtassa ki a pontozott Alapkönyvtár
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH tooSCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**A KIMENETRE:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt

## <a name="consume-spark-models-through-a-web-interface"></a>Spark modellek felhasználásához webes felületen keresztül
Spark mechanizmust biztosít tooremotely submit kötegelt feladatok vagy interaktív lekérdezések egy REST keresztül kommunikáljanak a Livy összetevőt. A HDInsight Spark-fürt alapértelmezés szerint engedélyezve van a Livy. Livy további információkért lásd: [távolról a Livy használatával nyújt Spark feladatok](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Tooremotely, amelyeket a batch pontszámok feladat elküldése egy fájlt, amely egy Azure blob tárolja, és ezután ír hello eredmények tooanother blob Livy is használhatja. toodo, a Python-parancsfájl hello feltöltése  
[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob hello Spark-fürt. Egy eszköz, például használhatja **Microsoft Azure Tártallózó** vagy **AzCopy** toocopy hello parancsfájl toohello fürt blob. Abban az esetben, ha azt feltöltött hello parancsfájl túl***wasb:///example/python/ConsumeGBNYCReg.py***.   

> [!NOTE]
> meg kell található hello portal hello Spark-fürt társított hello tárfiók tárelérési kulcsokat hello. 
> 
> 

Toothis helye a feltöltést követően a parancsfájl hello Spark-fürt elosztott környezetben belül fut. Hello modell betölt, és előrejelzéseket futó hello modellen alapuló bemeneti fájlok.  

Ez a parancsfájl távolról meghívni egy egyszerű HTTPS/REST kérelem hajtsanak végre a Livy.  Ez a curl parancs tooconstruct hello HTTP kérelem tooinvoke hello Python-parancsfájl távolról. CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME cserélje le a Spark-fürt hello megfelelő értékeket.

    # CURL COMMAND tooINVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Egyetlen nyelven használhatja hello távoli tooinvoke hello Spark rendszerfeladat Livy keresztül a hívás egy egyszerű HTTPS az egyszerű hitelesítéssel.   

> [!NOTE]
> A HTTP híváshoz kényelmes toouse hello Python kérelmek könyvtár lenne, de jelenleg nincs telepítve az Azure Functions alapértelmezés szerint. Így a régebbi HTTP szalagtárak használja helyette.   
> 
> 

Hello HTTP hívás hello Python kódját a következő:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF hello REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY hello PYTHON SCRIPT tooRUN ON hello SPARK CLUSTER
    # IN hello FILE PARAMETER OF hello JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Azt is megteheti a Python-kód túl[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger egy Spark feladat elküldése, amely egy blob pontszámaihoz alapján különböző eseményekkel – például egy időzítő, létrehozás vagy frissítés BLOB. 

Kód szabad ügyfélélmény tetszés szerint használja a hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark kötegelt pontozási meghatározhat egy HTTP-műveletet a hello **Logic Apps Designer** és a paraméterek beállítása. 

* Azure-portálon hozzon létre egy új logikai alkalmazás kiválasztásával **+ új** -> **Web + mobil** -> **logikai alkalmazás**. 
* hello mentése toobring **Logic Apps Designer**, adja meg a Logic App hello hello nevét és az App Service-csomag.
* Válasszon ki egy HTTP-műveletet, és írja be a következő ábra hello látható hello paramétereket:

![Logic Apps-Tervező](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a>A következő lépések
**Kereszt-ellenőrzési és hyperparameter abszolút**: lásd: [speciális adatok feltárása és Spark modellezés](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek betanítása a kereszt-ellenőrzési és a hyper-paraméter abszolút használatával.

