---
title: "aaaBuild Apache Spark gépi tanulási alkalmazások az Azure HDInsight |} Microsoft Docs"
description: "Lépésenként hogyan toobuild Apache Spark gépi tanulási HDInsight Spark alkalmazás fürtök Jupyter notebook használatával"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 332bd89876f7ebf178f7573d6018d064edfe9a8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Az Apache Spark on machine learning-alkalmazások Azure hdinsight létrehozása

Ismerje meg, hogyan toobuild egy Spark használó Apache Spark machine learning alkalmazások fürt a hdinsight platformon. Ez a cikk bemutatja, hogyan toouse hello Jupyter notebook hello fürt toobuild érhető el, és az alkalmazás teszteléséhez. hello alkalmazás hello HVAC.csv mintaadatok elérhető alapértelmezés szerint minden fürt használja.

**Előfeltételek:**

Hello következő kell rendelkeznie:

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Hello adatkészlet ismertetése
Előtt először hello alkalmazás felépítése, ossza meg velünk ismerje meg, amelyhez azt létre hello és hello típusú műveleteket végezzük el elemzést hello adatokon hello adatok hello szerkezete. 

Ebben a cikkben hello mintát használjuk **HVAC.csv** adatfájl elérhető hello hello HDInsight-fürthöz társított Azure Storage-fiók. Hello tárfiókot, belül hello fájl jelenleg **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Töltse le és nyissa meg a hello CSV-fájl tooget hello adatok pillanatképe.  

![Spark machine learning például használt adatok pillanatképe](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark machine learning például használt adatok pillanatképe")

hello adatok hello cél hőmérséklet és a tényleges hőmérséklet hello HVAC-rendszerek telepítése épület jeleníti meg. Tegyük fel hello **rendszer** oszlop felel hello Rendszerazonosító és hello **SystemAge** oszlop felel hello számú éven belülre esik hello HVAC rendszer korábban már hello épület helyén.

Az adatok toopredict e épület lesz hotter vagy colder alapján hello cél hőmérséklet, a rendszer Azonosítót és a rendszer kora használjuk.

## <a name="app"></a>Spark machine learning alkalmazás Spark MLlib
Ez az alkalmazás egy Spark ML adatcsatorna tooperform dokumentum besorolást használjuk. Hello sorban azt hello dokumentum felosztása szavakat, hello szavak átalakítása egy numerikus szolgáltatás vektor, és végül a egy előrejelzési modell hello szolgáltatás vektorok és címkék létrehozása. Hajtsa végre a következő lépéseket toocreate hello alkalmazás hello.

1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   
2. Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.
   
   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Hozzon létre új notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.
   
    ![Spark machine learning például Jupyter notebook létrehozása](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Spark machine learning például Jupyter notebook létrehozása")
4. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.
   
    ![Adja meg a notebook neve Spark machine learning például](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "notebook nevezze el a Spark machine learning – példa")
5. Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor. Ehhez a forgatókönyvhöz szükséges típusok hello importálásával megkezdése. Illessze be az alábbi részlet egy üres cellába hello, és nyomja le az **SHIFT + ENTER**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Ki kell betölteni hello adatait (hvac.csv), elemzéséhez, és újra tootrain hello modellt használja azt. Ehhez adja meg egy függvényt, amely ellenőrzi, hogy hello tényleges hőmérséklet hello épület hello cél hőmérséklet-nál nagyobb. Hello tényleges hőmérséklet nagyobb, hello épület akkor forró hello értékkel megjelölt **1.0**. Hello tényleges hőmérséklet kisebb, hello épület akkor cold, hello értékkel megjelölt **0,0**. 
   
    A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.

        # List hello structure of data for better understanding. Because hello data will be
        # loaded as an array, this structure makes it easy toounderstand what each element
        # in hello array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses hello raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load hello raw HVAC.csv file, parse it using hello function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfigurálja a hello Spark gépi tanulás feldolgozási folyamat három szakaszból áll: jogkivonatokat létrehozó hashingTF és lr. Mi az, hogy egy folyamat és annak működéséről lásd: További információ <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning adatcsatorna</a>.
   
    A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Hello adatcsatorna toohello képzési dokumentum oszlopnál. A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.
   
        model = pipeline.fit(training)
3. Ellenőrizze a hello képzési dokumentum toocheckpoint hello alkalmazással az előrehaladást. A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.
   
        training.show()
   
    Hello kimeneti hasonló toohello következő adja meg:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Lépjen vissza, és ellenőrizze a hello kimeneti hello nyers CSV-fájl alapján. Hello első sor hello CSV-fájl például ezek az adatok rendelkezik:

    ![Kimeneti Spark machine learning például pillanatkép adatai](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "kimeneti adatok pillanatképének elkészítéséhez, Spark machine learning például")

    Figyelje meg, hogyan hello tényleges hőmérséklete kisebb, mint hello cél hőmérséklet hello épület javasolhat cold. Ezért hello képzési kimenet, a következő hello **címke** a hello első sor a **0,0**, ami azt jelenti, hogy hello épület nincs gyakran használt adatok.

1. Készítsen elő egy adatkészlet toorun hello betanított modell ellen. toodo Igen, azt volna adja át a rendszer Azonosítót és a rendszer kora (jelölése **SystemInfo** hello képzési kimenet), és hello modell előrejelzése volna e hello felépítése az adott rendszer azonosítója és a rendszer kora kellene lennie hotter (kimaradásával 1.0) vagy a hűtőre () helyén 0,0).
   
   A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Végezetül előrejelzéseket készítsen a hello tesztadatokat. A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Egy kimeneti hasonló toohello következő kell megjelennie:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Hello első sorában hello előrejelzés, láthatja, hogy a 20-as Azonosítójú és 25 évre rendszer korát HVAC rendszer esetében hello épület lesz működés közbeni (**előrejelzés = 1.0**). hello DenseVector (0.49999) első értéke megfelel-e toohello előrejelzés 0,0-nál, és hello szükséges második értéke (0.5001) toohello előrejelzés 1.0 felel meg. A hello kimeneti, annak ellenére, hogy hello második értéke csak kis mértékben magasabb hello modell mutatja **előrejelzés = 1.0**.
4. Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**. Ezzel leállítja és Bezárás hello notebookot.

## <a name="anaconda"></a>Használjon Anaconda scikit – ismerje meg a könyvtár a Spark gépi tanulás
Az Apache Spark on hdinsight fürtök Anaconda-könyvtárakkal rendelkeznek. Ebbe beleértendő az is hello **scikit-további** könyvtár a gépi tanulás. hello könyvtár is használható toobuild alkalmazásokat közvetlenül a Jupyter notebook különböző adatkészletek. A példák segítségével hello scikit-könyvtár ismerje meg, lásd: [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
