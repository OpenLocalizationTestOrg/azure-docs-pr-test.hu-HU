---
title: "Tudományos aaaData Scala és Spark használata az Azure-on |} Microsoft Docs"
description: "Hogyan toouse Scala a felügyelt gépi tanulási feladatok hello Spark méretezhető MLlib és Spark ML-csomagokat a egy Azure HDInsight Spark-fürt."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: e32ebd0b91417183fe48ee10ebc7929fd9605762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a>Adatelemzés a Scala és a Spark használatával az Azure rendszerben
Ez a cikk bemutatja, hogyan toouse Scala felügyelt gépi tanulási feladatok hello Spark méretezhető MLlib és Spark ML csomagok egy Azure HDInsight Spark-fürtön. Az bemutatja, hogyan hello feladatok hello alkotó [Adattudomány folyamat](http://aka.ms/datascienceprocess): adatfeldolgozást és a feltárása, a képi megjelenítés, a szolgáltatás mérnöki csapathoz, a modellezési és a model felhasználás. hello cikkben hello modellek között logisztikai és lineáris regressziós, véletlenszerű erdők és színátmenetes súlyozott fák (GBTs), továbbá tootwo közös felügyelt gépi tanulási feladatok:

* Regressziós probléma: hello tipp összeg (SPN) taxi útnak előrejelzését
* Bináris osztályozás: előrejelzését tipp vagy taxi útnak nincs ötlet (1 vagy 0)

folyamat modellezési hello betanítása és kiértékelése egy teszt adatkészlet és a megfelelő pontossága metrikák igényel. Ebből a cikkből megtudhatja hogyan toostore ezek a modellek az Azure Blob Storage tárolóban, és hogyan tooscore és értékelje a prediktív teljesítményét. Ez a cikk is magában foglalja a hello fejlettebb, témakörök, hogyan toooptimize modellek kereszt-ellenőrzési és a hyper-paraméter abszolút használatával. használt hello adatok látható egy minta hello 2013 NYC taxi út és a jegy ára adatkészlet elérhető a Githubon.

[Scala](http://www.scala-lang.org/), hello Java virtuális gép, a nyelvet integrálja az objektumorientált és funkcionális nyelvi fogalmak. Feldolgozás alatt álló hello felhő kiválóan alkalmas toodistributed, és az Azure Spark-fürtjei fut méretezhető nyelven.

[Spark](http://spark.apache.org/) egy nyílt forráskódú párhuzamos feldolgozási keretrendszer, amely támogatja a memórián belüli feldolgozását végzi big data elemzés alkalmazások tooboost hello teljesítményét. hello Spark program sebességét, a könnyű, valamint a kifinomult analytics lett tervezve. A Spark memóriában elosztott tárolt számítási képességei jól funkcionálnak a iteratív algoritmusaival a machine learning és a graph számítások. Hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) csomag egységes teszi lehetővé a magas szintű API-k adatokat, melyek segíthetnek keretek létrehozása és gyakorlati gépi tanulási a folyamatok hangolására platformra épül. [MLlib](http://spark.apache.org/mllib/) a Spark méretezhető gépi tanulás függvénytár, amely modellezési képességekkel toothis elosztott környezetekben.

[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) hello Azure által üzemeltetett elérhető nyílt forráskódú Spark van. Is támogatja a Jupyter Scala notebookok hello Spark-fürtön, és futtatása Spark SQL interaktív lekérdezések tootransform szűrheti, és az Azure Blob storage szolgáltatásban tárolt adatainak megjelenítése. hello Scala kódtöredékek ebben a cikkben, amely hello megoldást nyújtanak, és megfelelő előkészítésére hello toovisualize hello adatok megjelenítése futtatása Jupyter notebookok hello Spark-fürtjei telepítve. hello modellezési lépéseket a következő témakörök rendelkezik code, hogy bemutatja, hogyan tootrain, értékelje ki, mentse és felhasználását egyes beírta modell.

hello beállítási lépéseket és kód ebben a cikkben az Azure HDInsight 3.4 Spark 1.6-os vannak. Ebben a cikkben és a hello kód azonban hello [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) általánosak, és a Spark-fürt működnek. hello fürt beállítása és felügyeleti lépéseket előfordulhat, hogy mi is megjelennek ebben a cikkben nem használata a HDInsight Spark némileg eltérő.

> [!NOTE]
> Ez a témakör azt ismerteti, hogyan toouse Scala helyett Python toocomplete feladatokat egy végpontok közötti Adattudomány folyamat, lásd: [Spark on Azure HDInsight használatának Adattudomány](machine-learning-data-science-spark-overview.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetéssel kell rendelkeznie. Ha még nem rendelkezik egy, [egy Azure ingyenes próbaverzió beszerzése](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Az alábbi eljárásokat Azure HDInsight 3.4 Spark 1.6-os fürt toocomplete hello van szüksége. a fürt toocreate hello utasításait lásd: [első lépések: hozzon létre Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Hello fürt típusa és verzió beállítása hello **fürt típusának kiválasztása** menü.

![A HDInsight fürt típus konfigurálása](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

Hello NYC taxi vissza adatokat, valamint útmutatást a hogyan tooexecute hello Spark-fürt a Jupyter notebook a kód leírását hello megfelelő szakaszaiban talál [áttekintése adatok tudományos Spark on Azure HDInsight használatának](machine-learning-data-science-spark-overview.md).  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>A Spark-fürt hello Jupyter notebook hajt végre Scala kódot
Az Azure-portálon hello Jupyter notebook indíthatja el. Spark-fürt hello az irányítópulton található, és kattintson az tooenter hello felügyelet lapon a fürt számára. Ezután kattintson **fürt irányítópultok**, és kattintson a **Jupyter Notebook** tooopen hello notebook társított hello Spark-fürt.

![Fürt-irányítópult és a Jupyter notebookok](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Jupyter notebookok: https:// is hozzáférhet&lt;clustername&gt;.azurehdinsight.net/jupyter. Cserélje le *clustername* hello néven a fürt. A rendszergazdai fiók tooaccess hello Jupyter notebookok hello jelszó szükséges.

![Nyissa meg tooJupyter notebookok hello fürt neve](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Válassza ki **Scala** toosee egy könyvtárat, amely rendelkezik néhány példa a előre csomagolt notebookok adott használata hello PySpark API. Ennek a programcsomagnak Spark témakörökből áll rendelkezésre a hello mintakódok tartalmazó Scala.ipynb notebook használatával feltárása modellezéséhez és pontozás hello [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).

Közvetlenül a Githubból toohello Jupyter Notebook server hello notebook feltöltheti a Spark-fürtön. A Jupyter kezdőlapján kattintson a hello **feltöltése** gombra. A Fájlkezelőben hello hello Scala jegyzetfüzet hello GitHub (nyers tartalom) URL-cím illeszthető be, és kattintson **nyitott**. a következő URL-cím hello hello Scala notebook érhető el:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Telepítő: Előre definiált Spark és Hive-környezetek, Spark magics és Spark-függvénytárak
### <a name="preset-spark-and-hive-contexts"></a>Spark és Hive-környezetek az adott néven beállítás
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


a Jupyter notebookok biztosított hello Spark mag beállításkészletet kontextusban rendelkezik. Set hello Spark- vagy Hive környezetek tooexplicitly nincs szükség a fejlesztői hello alkalmazás használatának megkezdése előtt. hello beállításkészletet kontextusban a következők:

* `sc`a SparkContext
* `sqlContext`a HiveContext

### <a name="spark-magics"></a>Spark magics
hello Spark kernel tartalmaz néhány előre definiált "magics", amelyeket meghívhatja a különleges parancsok `%%`. Ezek a parancsok közül kettő a következő mintakódok hello használnak.

* `%%local`Meghatározza, hogy egymás utáni sorok hello kód végrehajtja helyileg. hello kód érvényes Scala-kódot kell lennie.
* `%%sql -o <variable name>`végrehajtja a Hive-lekérdezések elleni `sqlContext`. Ha hello `-o` paramétert, a hello megőrződjenek hello hello lekérdezés eredménye `%%local` adatok keretként Spark Scala környezetben.

A Jupyter notebookokból és az előre definiált hello kernelek kapcsolatos további információk "magics", amely meghívja a `%%` (például `%%local`), lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a fürtök HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).

### <a name="import-libraries"></a>Könyvtárak importálása
Importálja a hello Spark, MLlib és egyéb szalagtárak, szüksége lesz a következő kód hello segítségével.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Adatfeldolgozás
hello hello Adattudomány folyamat első lépése az tooingest hello adatai, amelyet az tooanalyze. Kapcsolása hello adatok külső forrásból vagy rendszerek helyét az adatok feltárása és modellezési környezetbe. Ebben a cikkben hello meg a betöltési adata illesztett 0,1 % minta hello taxi út és a jegy ára fájl (.tsv fájlként tárolja). hello adatok feltárása és modellezési környezete Spark. Ez a szakasz hello kód toocomplete hello a következő lépéseket:

1. Adatok és a modell tárolás a könyvtár elérési útvonalak beállítása.
2. Olvassa el a hello bemeneti adatkészletben (.tsv fájlként tárolja).
3. Adja meg a hello és tiszta hello adatainak sémát.
4. Megtisztított adatok keret létrehozása, és a gyorsítótár, a memória.
5. Hello adatok regisztrálható az SQLContext ideiglenes táblájába.
6. Hello tábla lekérdezése, és importálja a hello eredmények adatok keretbe.

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Az Azure Blob storage tárolási helyek könyvtár elérési útja beállítása
Spark és írhat tooAzure Blob Storage tárolóban. Spark tooprocess használja, a meglévő adatok valamelyikét, és tárolására is használható majd hello eredmények újra a Blob Storage tárolóban.

toosave modellek vagy a Blob Storage tárolóban fájlok tooproperly kell hello elérési utat adjon meg. Hivatkozás hello alapértelmezett tároló csatolt toohello Spark-fürt kezdődő elérési úttal `wasb:///`. Egyéb helyek hivatkozni használatával `wasb://`.

hello alábbi kódminta helyét adja meg hello hello bemeneti adatok toobe olvassa el és hello elérési tooBlob tárolókhoz csatolt toohello Spark-fürt hello modell menteni.

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a>Adatok importálása, hozzon létre egy RDD, és olyan adatokat, keret toohello séma szerint
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello SCHEMA BASED ON hello HEADER OF hello FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING toohello SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE hello CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER hello DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 8 másodperc.

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a>Hello tábla lekérdezése, és adatok keretben eredmények importálása
Mellett, hello lekérdezéstábla jegy ára, utas és tipp adatok; szűrő sérült, és a külső adatai; és nyomtathatják ki több sort.

    # QUERY hello DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY hello TOP THREE ROWS
    sqlResultsDF.show(3)

**A kimenetre:**

| fare_amount | passenger_count | tip_amount | Formabontó |
| --- | --- | --- | --- |
|        13.5 |1.0 |2.9 |1.0 |
|        16.0 |2.0 |3.4 |1.0 |
|        10.5 |2.0 |1.0 |1.0 |

## <a name="data-exploration-and-visualization"></a>Az adatok feltárása és -megjelenítésre
Hello adatok beolvasása a Spark, miután hello hello Adattudomány folyamat következő lépése toogain bemutatják hello az adatok feltárása és -megjelenítésre. Ebben a szakaszban hello taxi adatokat az SQL-lekérdezések használatával ellenőrizze. Ezt követően importálása hello eredményeit azokat egy adatok keret tooplot hello cél változók és a visual hálózatfelügyeleti potenciális funkcióit az Jupyter hello automatikus-képi megjelenítés szolgáltatásával.

### <a name="use-local-and-sql-magic-tooplot-data"></a>Helyi és az SQL magic tooplot adatainak használata
Alapértelmezés szerint hello bármely kódrészletet, amely futtatja a Jupyter notebook eredménye hello környezetben hello munkamenet fennállásának hello munkavégző csomóponton belül érhető el. Ha toosave egy út toohello munkavégző csomópontokhoz minden számításhoz, és minden hello adatokat, amelyekre szüksége van a számítási áll rendelkezésre helyi kiszolgáló-csomóponton hello Jupyter (amely hello átjárócsomópont), ha használható hello `%%local` magic toorun hello kódot részlet hello Jupyter kiszolgálón.

* **SQL magic** (`%%sql`). HDInsight Spark kernel hello az SQLContext könnyen beágyazott HiveQL lekérdezéseket támogatja. hello (`-o VARIABLE_NAME`) argumentum hello SQL-lekérdezés kimenetét hello hello Jupyter kiszolgálón Pandas adatok keretként továbbra is fennáll.. Ez azt jelenti, hogy lesz hello helyi módban érhető el.
* `%%local`**magic**. Hello `%%local` magic hello kód futtatása helyben hello Jupyter kiszolgálón, amely hello hello HDInsight-fürt átjárócsomópontjához. Általában akkor használják `%%local` magic hello együtt `%%sql` a hello magic `-o` paraméter. Hello `-o` paraméter szeretné megőrizni a hello kimeneti helyileg, a hello SQL lekérdezést, majd `%%local` magic kiváltották hello következő készlete kód részlet toorun helyileg hello kimeneti hello SQL lekérdezések helyileg fennállásának ellen.

### <a name="query-hello-data-by-using-sql"></a>Hello adatait SQL használatával
Ez a lekérdezés lekéri a jegy ára, utas száma, és tipp által hello taxi való adatváltások számát.

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

A következő kód hello, hello `%%local` magic létrehozza a helyi adatok, sqlResults. SqlResults tooplot matplotlib segítségével is használhatók.

> [!TIP]
> Helyi magic ebben a cikkben több alkalommal van használva. Ha az adatkészlet túl nagy, adjon minta toocreate elfér a helyi memória adatok keret.
> 
> 

### <a name="plot-hello-data"></a>Hello adatok ábrázolása
Dolgozunk is után hello adatok keret helyi környezetben Pandas adatok keretként Python kód használatával.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 hello Spark kernel automatikusan visualizes hello (HiveQL) az SQL-lekérdezések eredményének, hello kód futtatása után. Számos különböző típusú megjelenítések közül választhat:

* Tábla
* Torta
* Vonal
* Terület
* Sáv

Itt az hello kód tooplot hello adatai:

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**A kimenetre:**

![Tipp összeg hisztogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tipp jegy ára mennyiséggel összeg](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Funkciók létrehozása és átalakítási szolgáltatások és funkciók modellezési a bemeneti adatok majd előkészítése
Fa-alapú modellezési funkciók Spark ML és MLlib meg kell tooprepare cél és a szolgáltatások módszerek, például a dobozolás indexelő, egy közbeni kódolás vagy vectorization használatával. Ebben a szakaszban az alábbiakban hello eljárások toofollow:

1. Hozzon létre egy új szolgáltatás által **dobozolás** üzemideje (óra) a forgalom gyűjtők idő.
2. Alkalmazása **indexelő és egy közbeni kódolás** toocategorical szolgáltatásokat.
3. **A minta és a megosztott adatkészlet hello** tanítási és tesztelési törtek be.
4. **Adja meg a képzés változó és a szolgáltatások**, és majd létre indexelt vagy egy közbeni kódolású betanítása és rugalmas bemeneti címkézett pont tesztelés elosztott adatkészletek (RDDs) vagy adatkeretek.
5. Automatikusan **kategorizálását és szolgáltatásait és célok vectorize** toouse machine learning modellek bemeneteként.

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Hozzon létre egy új szolgáltatás a forgalom idő gyűjtők dobozolás óránként
Ez a kód bemutatja, hogyan toocreate egy új szolgáltatás által óra dobozolás forgalom idő az időszakok, és hogyan toocache hello eredményül kapott adatok keret a memóriában. Amennyiben RDDs és az adatok keretek ismételten használnak, tooimproved végrehajtásának lassúságát gyorsítótárazás vezet. Ennek megfelelően RDDs és az adatok keretek lesz gyorsítótár a következő eljárások hello több szakaszaiban.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE hello DATA FRAME IN MEMORY AND MATERIALIZE hello DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indexelő és egy közbeni kódolás kategorikus funkciók
hello modellezési, és előre jelezni MLlib feladatai szükséges a bemeneti adatok kategorikus toobe funkcióinak indexelt, vagy korábbi toouse kódolású. Ez a szakasz bemutatja, hogyan tooindex vagy funkciók modellezési hello a bemenő kategorikus funkciói kódolása.

A modellek kódolása hello modelltől függően különböző módon vagy tooindex szükséges. Logisztikai és lineáris regressziós modellt megkövetelni például, egy közbeni kódolást. Például három kategóriába szolgáltatás három funkció oszlopokba bővíthetők. Egyes oszlopok 0 vagy 1 attól függően, hogy egy megfigyelési hello kategória tartalmazna. MLlib biztosít hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) egy közbeni kódolási funkció. A kódoló leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz. A kódolás numerikus fontos funkciók, például logisztikai regresszió várt algoritmusokat lehet alkalmazott toocategorical szolgáltatások.

Itt csak négy változók tooshow példák, amelyek karakterláncok alakít át. Más változók, például a hét napja, numerikus érték, mint kategorikus változók által képviselt is indexelheti.

Az indexelő, használja `StringIndexer()`, és egy közbeni kódolását, használja a `OneHotEncoder()` MLlib funkciókat. Itt található kód tooindex hello és kódolása kategorikus szolgáltatások:

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 4 másodperc.

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a>A minta és a megosztott adatkészlet hello a tanítási és tesztelési törtek
Ez a kód hoz létre egy véletlenszerű mintavételi hello adatok (25 %, ebben a példában). Mintavételi, de nem szükséges ehhez a példához hello adatkészlet toohello mérete miatt hello a cikk bemutatja, hogyan lehet mintát a megállapításához, hogy hogyan toouse azt a saját problémákat, amikor szükséges. Nagy minták esetén ez jelentős időt takaríthat meg a modellek betanítása közben. A következő hello minta felosztása tanítási része (ebben a példában a 75 %) és egy tesztelési része (25 %, ebben a példában) toouse besorolási és regressziós modellezéshez hatékony.

Egy sort ad hozzá (0 és 1) közé eső véletlenszerű szám tooeach (a "VÉL" oszlop), amelyek betanítás során használt tooselect kereszt-ellenőrzési modellrészt lehetnek.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT hello SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 2 másodperc.

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Adja meg a képzés változó és a szolgáltatások, és hozza létre indexelt vagy egy közbeni kódolású a modell betanítására és tesztelésére bemeneti pont RDDs vagy adatok keretek címkével
Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adatait, mivel a címkézett adattípus mutasson, és így tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell használhatja kódolása kódját tartalmazza. Címkézett pont objektum RDDs formázott oly módon, hogy a legtöbb gépi tanulási algoritmusok MLlib a bemeneti adatként van szükség. A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.

Ez a kód hello (függő) célváltozó és hello szolgáltatások toouse tootrain modellek kell megadni. Ezután létrehoz indexelt vagy egy közbeni kódolású a modell betanítására és tesztelésére bemeneti pont RDDs vagy adatok keretek címkével.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY hello TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 4 másodperc.

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a>Automatikusan kategorizálását és szolgáltatásait és célok toouse vectorize machine learning modellek bemeneteként
Fa-alapú modellezési funkciók használata Spark ML toocategorize hello cél és a szolgáltatások toouse. hello kód két feladatokat hajtja végre:

* Egy bináris osztályozás cél értéke csak 0 vagy 1 tooeach adatpont 0 és 1 közötti hozzárendelése egy küszöbértéket 0,5 használatával hoz létre.
* Automatikusan kategorizálja szolgáltatásokat. Ha az összes olyan szolgáltatás különböző numerikus értékeket hello száma kisebb, mint 32, adott szolgáltatás kategorizálta.

Ez a két feladatok hello kódja.

    # CATEGORIZE FEATURES AND BINARIZE hello TARGET FOR hello BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR hello REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Bináris osztályozási modell: előre jelezni, hogy kell fordítani tipp:
Ebben a szakaszban hozzon létre három típusú-e tipp kell fordítani a bináris osztályozási modellek toopredict:

* A **logisztikai regresszió modell** hello Spark ML használatával `LogisticRegression()` függvény
* A **véletlenszerű erdő besorolási modell** hello Spark ML használatával `RandomForestClassifier()` függvény
* A **átmenetes kiemelési fa besorolási modell** hello MLlib használatával `GradientBoostedTrees()` függvény

### <a name="create-a-logistic-regression-model"></a>Logisztikai regressziós modell létrehozása
Ezután hozzon létre egy logisztikai regresszió modell hello Spark ML használatával `LogisticRegression()` függvény. A lépések egy sorozatát kód felépítése hello modell hoz létre:

1. **Hello tanítási** adatok egy paraméter-készlettel.
2. **Hello modell kiértékelése** a metrikák a teszt adatkészlet.
3. **Hello modell mentése** Blob Storage a jövőbeni felhasználásához.
4. **Pontszám hello modell** vizsgálati adatok alapján.
5. **Hello eredmények ábrázolhatók** a jellemző (ROC) görbék működő fogadó.

Ezek az eljárások hello kódját a következő:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON hello TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE hello MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Betöltése, pontozása és hello-eredményeket menteni.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD hello SAVED MODEL AND SCORE hello TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON hello TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC RESULTS
    println("ROC on test data = " + ROC)


**A kimenetre:**

A tesztadatokat ROC = 0.9827381497557599

Helyi Pandas adatok keretek tooplot hello: ROC-görbe Python használja.

    # QUERY hello RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT hello ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT hello ROC CURVE
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


**A kimenetre:**

![Tipp vagy nincs tipp: ROC-görbe](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a>Hozzon létre egy véletlenszerű besorolás erdőmodell
Ezután hozzon létre egy véletlenszerű erdő besorolási modell hello Spark ML használatával `RandomForestClassifier()` működik, és értékelje ki hello modell a tesztadatokat.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE hello RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT hello MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE hello MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**A kimenetre:**

A tesztadatokat ROC = 0.9847103571552683

### <a name="create-a-gbt-classification-model"></a>GBT besorolási modell létrehozása
Ezután hozzon létre egy GBT besorolási modell MLlib tartozó használatával `GradientBoostedTrees()` működik, és értékelje ki hello modell a tesztadatokat.

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN hello MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE hello MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE hello MODEL ON TEST INSTANCES AND hello COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS tooEVALUATE hello MODEL ON hello TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**A kimenetre:**

: ROC-görbe alatt: 0.9846895479241554

## <a name="regression-model-predict-tip-amount"></a>Regressziós modell: tipp összeg előrejelzése
Ebben a szakaszban hozzon létre két típusú regressziós modell toopredict hello tipp összeg:

* A **rendeződik lineáris regressziós modell** hello Spark ML használatával `LinearRegression()` függvény. Hello modell mentése lesz, és értékelje ki a hello modell a tesztadatokat.
* A **színátmenetes kiemelése fa regressziós modell** hello Spark ML használatával `GBTRegressor()` függvény.

### <a name="create-a-regularized-linear-regression-model"></a>Rendeződik lineáris regressziós modell létrehozása
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING hello SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT hello MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE hello MODEL OVER hello TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE hello MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT hello COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 13 másodperc.

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("R-sqr on test data = " + r2)


**A kimenetre:**

R-sqr a tesztadatokat = 0.5960320470835743

A következő lekérdezés hello a vizsgálati eredmények adatok keret és AutoVizWidget és matplotlib toovisualize használja azt.

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

hello kód létrehoz egy helyi adatok keret hello lekérdezés kimeneti és tevékenységtérkép hello adatok. Hello `%%local` magic létrehoz egy helyi adatok keret `sqlResults`, mely tooplot matplotlib használható.

> [!NOTE]
> A Spark magic ebben a cikkben több alkalommal van használva. Nagy adatmennyiség hello esetén meg kell minta toocreate elfér a helyi memória adatok keret.
> 
> 

Hozzon létre előkészítésére Python matplotlib.

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT hello RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**A kimenetre:**

![Tipp összeg: tényleges és előre jelzett](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a>GBT regressziós modell létrehozása
Hozzon létre egy GBT regressziós modell hello Spark ML `GBTRegressor()` működik, és értékelje ki hello modell a tesztadatokat.

[Fák színátmenetes súlyozott](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák. GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt. Használhat GBTs regressziós és besorolás. Azok kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés funkció és nonlinearities és a szolgáltatás kapcsolati. Is használhatja őket a multiclass-besorolás beállításban.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("Test R-sqr is: " + Test_R2);


**A kimenetre:**

Teszt R-sqr van: 0.7655383534596654

## <a name="advanced-modeling-utilities-for-optimization"></a>Speciális modellezési segédprogramok energiaoptimalizálás
Ebben a szakaszban használhatja a machine learning segédprogramok használó fejlesztők gyakran modell optimalizálásra vonatkozóan. Pontosabban optimalizálhatja a machine learning modellek három különböző módon paraméter abszolút és kereszt-ellenőrzési használatával:

* Hello adatok felosztása tanítási és érvényesítési beállítása hello modell használatával hyper paraméter abszolút egy gyakorlókészlethez optimalizálása és értékelődik ki az érvényesítési készletének (lineáris regresszió)
* Hello modell optimalizálhatja a kereszt-ellenőrzési és hyper paraméter abszolút Spark ML CrossValidator függvény (bináris osztályozás) használatával.
* Gépi tanulás funkció és paraméter beállítása (lineáris regresszió) használatával egyéni kereszt-ellenőrzési és paraméter abszolút kód toouse hello modell optimalizálása

**Kereszt-ellenőrzési** egy technika, amely értékeli a milyen mértékben a modellek betanítása adatokat egy ismert csoportján fog generalize toopredict hello részeit, amelyen az még nincs betanítva adatkészletek. hello általános ezzel a technikával mögött lényege, hogy egy modell betanítása ismert adatok a megfelelő adatokat, és majd hello az előrejelzés pontosságát lett tesztelve egy független adatkészlet ellen. A közös megvalósítása toodivide az adatkészlet *k*-modellrészt, és ezután a ciklikus multiplexeléssel a egy hello modellrészt hello modell betanításához.

**Hyper-paraméter optimalizálási** hello probléma lépett fel a hyper-paraméterek készletét a tanulási algoritmus, általában a hello célja egy független adatkészlet hello algoritmus teljesítményének biztosítása optimalizálása kiválasztása. A hyper-paraméter értéke kívül hello modell betanítási eljárást kell megadnia. A hyper-paraméterértékek feltételezéseket hatással lehet a hello rugalmasságot és hello modell pontosságát. Döntési fák például rendelkezik hyper-paraméterek, például a hello szükséges mélységében és hello fában leaves száma. Meg kell adni egy téves besorolás szövegminősítési kifejezés támogatási vektoros gépek (SVM).

A közös módon tooperform hyper paraméter optimalizálása toouse rács keresés – más néven egy **paraméter ismétlés**. A rács keresés a részletes keresést hello értékek hello hyper paraméter terület tanulási algoritmus megadott alhálózatát keresztül történik. Kereszt-ellenőrzési megadhatja a teljesítmény metrika toosort hello optimális eredmények hello rács keresési algoritmus által előállított ki. Korlát problémák tootraining modelladatok overfitting például segíthet a kereszt-ellenőrzési hyper paraméter abszolút használatakor. Ezzel a módszerrel hello modell olyan hello kapacitás tooapply toohello általános adatok készletét, mely hello betanítási adatok ki lett olvasni.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>A hyper-paraméter abszolút egy lineáris regressziós modellt optimalizálása
A következő adatok felosztása tanítási és érvényesítési beállítása, használja a hyper-paraméter egy képzési abszolút toooptimize hello modell beállítva, és értékelje érvényesítési megfelelő (lineáris regresszió).

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE hello ESTIMATOR FUNCTION: `hello LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE hello PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET), AND THEN hello SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**A kimenetre:**

Teszt R-sqr van: 0.6226484708501209

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>A kereszt-ellenőrzési és a hyper-paraméter abszolút hello bináris osztályozási modell optimalizálása
Ez a szakasz bemutatja, hogyan toooptimize bináris osztályozás modell kereszt-ellenőrzési és a hyper-paraméter abszolút használatával. Ez a Spark ML hello használja `CrossValidator` függvény.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS tooUSE WITH hello TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE hello ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY hello NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE hello TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE hello TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


**A kimenetre:**

Idő toorun hello cella: 33 másodperc.

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Egyéni kereszt-ellenőrzési és paraméter abszolút kód használatával hello lineáris regressziós modell optimalizálása
A következő hello modell optimalizálása egyéni kód használatával, és hello legjobb Modellparaméterek azonosíthatja a legmagasabb pontossági hello feltétel. Ezután hozzon létre hello végső modell, hello modell a tesztadatokat kiértékeléséhez és hello modell mentése a Blob Storage tárolóban. Végezetül hello modell betöltése, vizsgálati adatok pontozása és értékeléséhez pontosságát.

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello PARAMETER GRID AND hello NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY hello NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND hello PARAMETER GRID tooGET AND IDENTIFY hello BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 too(nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 too(numModels-1)) {
            for (nParams <- 0 too(numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET hello BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 too(numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE hello BEST MODEL WITH hello BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE hello BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON hello TRAINING SET WITH hello BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


    # LOAD hello MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST hello MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**A kimenetre:**

Idő toorun hello cella: 61 másodperc.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Spark-beépített machine learning modellek automatikusan Scala felhasználása
Témakörök, amelyek végigvezetik Önt az Azure-ban hello Adattudomány folyamat alkotó hello feladatok áttekintéséhez lásd: [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess).

[Vonja össze az adatokat tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) ismerteti más végpont forgatókönyvek, amelyek hello hello Team adatok tudományos folyamat az adott forgatókönyveket szükséges lépések bemutatása. hello forgatókönyvek is bemutatják, hogyan toocombine felhőalapú és helyszíni eszközök és szolgáltatások a munkafolyamatok és folyamat toocreate intelligens kérelmet.

[Spark-beépített gépi tanulási modell pontozása](machine-learning-data-science-spark-model-consumption.md) bemutatja, hogyan toouse Scala kód tooautomatically betölteni, és új adatkészletek pontozása gépi tanulási modellek Spark a beépített és az Azure Blob storage mentve. Nincs megadott hello utasításokat, és egyszerűen cserélje le hello Python kódját Scala kódra ebben a cikkben a automatizált felhasználásához.

