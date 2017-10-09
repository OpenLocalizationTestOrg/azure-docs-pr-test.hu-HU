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
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a><span data-ttu-id="8e872-103">Az Apache Spark on machine learning-alkalmazások Azure hdinsight létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e872-103">Build Apache Spark machine learning applications on Azure HDInsight</span></span>

<span data-ttu-id="8e872-104">Ismerje meg, hogyan toobuild egy Spark használó Apache Spark machine learning alkalmazások fürt a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="8e872-104">Learn how toobuild an Apache Spark machine learning application using a Spark cluster on HDInsight.</span></span> <span data-ttu-id="8e872-105">Ez a cikk bemutatja, hogyan toouse hello Jupyter notebook hello fürt toobuild érhető el, és az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8e872-105">This article shows how toouse hello Jupyter notebook available with hello cluster toobuild and test this application.</span></span> <span data-ttu-id="8e872-106">hello alkalmazás hello HVAC.csv mintaadatok elérhető alapértelmezés szerint minden fürt használja.</span><span class="sxs-lookup"><span data-stu-id="8e872-106">hello application uses hello sample HVAC.csv data that is available on all clusters by default.</span></span>

<span data-ttu-id="8e872-107">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="8e872-107">**Prerequisites:**</span></span>

<span data-ttu-id="8e872-108">Hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="8e872-108">You must have hello following:</span></span>

* <span data-ttu-id="8e872-109">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="8e872-109">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="8e872-110">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="8e872-110">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> 

## <span data-ttu-id="8e872-111"><a name="data"></a>Hello adatkészlet ismertetése</span><span class="sxs-lookup"><span data-stu-id="8e872-111"><a name="data"></a>Understand hello data set</span></span>
<span data-ttu-id="8e872-112">Előtt először hello alkalmazás felépítése, ossza meg velünk ismerje meg, amelyhez azt létre hello és hello típusú műveleteket végezzük el elemzést hello adatokon hello adatok hello szerkezete.</span><span class="sxs-lookup"><span data-stu-id="8e872-112">Before we start building hello application, let us understand hello structure of hello data for which we build hello application and hello kind of analysis we will do on hello data.</span></span> 

<span data-ttu-id="8e872-113">Ebben a cikkben hello mintát használjuk **HVAC.csv** adatfájl elérhető hello hello HDInsight-fürthöz társított Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="8e872-113">In this article, we use hello sample **HVAC.csv** data file that is available in hello Azure Storage account that you associated with hello HDInsight cluster.</span></span> <span data-ttu-id="8e872-114">Hello tárfiókot, belül hello fájl jelenleg **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="8e872-114">Within hello storage account, hello file is at **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span> <span data-ttu-id="8e872-115">Töltse le és nyissa meg a hello CSV-fájl tooget hello adatok pillanatképe.</span><span class="sxs-lookup"><span data-stu-id="8e872-115">Download and open hello CSV file tooget a snapshot of hello data.</span></span>  

<span data-ttu-id="8e872-116">![Spark machine learning például használt adatok pillanatképe](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark machine learning például használt adatok pillanatképe")</span><span class="sxs-lookup"><span data-stu-id="8e872-116">![Snapshot of data used for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Snapshot of data used for Spark machine learning example")</span></span>

<span data-ttu-id="8e872-117">hello adatok hello cél hőmérséklet és a tényleges hőmérséklet hello HVAC-rendszerek telepítése épület jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8e872-117">hello data shows hello target temperature and hello actual temperature of a building that has HVAC systems installed.</span></span> <span data-ttu-id="8e872-118">Tegyük fel hello **rendszer** oszlop felel hello Rendszerazonosító és hello **SystemAge** oszlop felel hello számú éven belülre esik hello HVAC rendszer korábban már hello épület helyén.</span><span class="sxs-lookup"><span data-stu-id="8e872-118">Let's assume hello **System** column represents hello system ID and hello **SystemAge** column represents hello number of years hello HVAC system has been in place at hello building.</span></span>

<span data-ttu-id="8e872-119">Az adatok toopredict e épület lesz hotter vagy colder alapján hello cél hőmérséklet, a rendszer Azonosítót és a rendszer kora használjuk.</span><span class="sxs-lookup"><span data-stu-id="8e872-119">We use this data toopredict whether a building will be hotter or colder based on hello target temperature, given a system ID and system age.</span></span>

## <span data-ttu-id="8e872-120"><a name="app"></a>Spark machine learning alkalmazás Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="8e872-120"><a name="app"></a>Write a Spark machine learning application using Spark MLlib</span></span>
<span data-ttu-id="8e872-121">Ez az alkalmazás egy Spark ML adatcsatorna tooperform dokumentum besorolást használjuk.</span><span class="sxs-lookup"><span data-stu-id="8e872-121">In this application we use a Spark ML pipeline tooperform a document classification.</span></span> <span data-ttu-id="8e872-122">Hello sorban azt hello dokumentum felosztása szavakat, hello szavak átalakítása egy numerikus szolgáltatás vektor, és végül a egy előrejelzési modell hello szolgáltatás vektorok és címkék létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8e872-122">In hello pipeline, we split hello document into words, convert hello words into a numerical feature vector, and finally build a prediction model using hello feature vectors and labels.</span></span> <span data-ttu-id="8e872-123">Hajtsa végre a következő lépéseket toocreate hello alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="8e872-123">Perform hello following steps toocreate hello application.</span></span>

1. <span data-ttu-id="8e872-124">A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="8e872-124">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="8e872-125">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="8e872-125">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="8e872-126">Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="8e872-126">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="8e872-127">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="8e872-127">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8e872-128">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="8e872-128">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="8e872-129">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="8e872-129">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. <span data-ttu-id="8e872-130">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="8e872-130">Create a new notebook.</span></span> <span data-ttu-id="8e872-131">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="8e872-131">Click **New**, and then click **PySpark**.</span></span>
   
    <span data-ttu-id="8e872-132">![Spark machine learning például Jupyter notebook létrehozása](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Spark machine learning például Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="8e872-132">![Create a Jupyter notebook for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Create a Jupyter notebook for Spark machine learning example")</span></span>
4. <span data-ttu-id="8e872-133">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="8e872-133">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="8e872-134">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="8e872-134">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="8e872-135">![Adja meg a notebook neve Spark machine learning például](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "notebook nevezze el a Spark machine learning – példa")</span><span class="sxs-lookup"><span data-stu-id="8e872-135">![Provide a notebook name for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Provide a notebook name for Spark machine learning example")</span></span>
5. <span data-ttu-id="8e872-136">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="8e872-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="8e872-137">hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="8e872-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="8e872-138">Ehhez a forgatókönyvhöz szükséges típusok hello importálásával megkezdése.</span><span class="sxs-lookup"><span data-stu-id="8e872-138">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="8e872-139">Illessze be az alábbi részlet egy üres cellába hello, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-139">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span> 
   
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
6. <span data-ttu-id="8e872-140">Ki kell betölteni hello adatait (hvac.csv), elemzéséhez, és újra tootrain hello modellt használja azt.</span><span class="sxs-lookup"><span data-stu-id="8e872-140">You must now load hello data (hvac.csv), parse it, and use it tootrain hello model.</span></span> <span data-ttu-id="8e872-141">Ehhez adja meg egy függvényt, amely ellenőrzi, hogy hello tényleges hőmérséklet hello épület hello cél hőmérséklet-nál nagyobb.</span><span class="sxs-lookup"><span data-stu-id="8e872-141">For this, you define a function that checks whether hello actual temperature of hello building is greater than hello target temperature.</span></span> <span data-ttu-id="8e872-142">Hello tényleges hőmérséklet nagyobb, hello épület akkor forró hello értékkel megjelölt **1.0**.</span><span class="sxs-lookup"><span data-stu-id="8e872-142">If hello actual temperature is greater, hello building is hot, denoted by hello value **1.0**.</span></span> <span data-ttu-id="8e872-143">Hello tényleges hőmérséklet kisebb, hello épület akkor cold, hello értékkel megjelölt **0,0**.</span><span class="sxs-lookup"><span data-stu-id="8e872-143">If hello actual temperature is lesser, hello building is cold, denoted by hello value **0.0**.</span></span> 
   
    <span data-ttu-id="8e872-144">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-144">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>

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


1. <span data-ttu-id="8e872-145">Konfigurálja a hello Spark gépi tanulás feldolgozási folyamat három szakaszból áll: jogkivonatokat létrehozó hashingTF és lr.</span><span class="sxs-lookup"><span data-stu-id="8e872-145">Configure hello Spark machine learning pipeline that consists of three stages: tokenizer, hashingTF, and lr.</span></span> <span data-ttu-id="8e872-146">Mi az, hogy egy folyamat és annak működéséről lásd: További információ <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning adatcsatorna</a>.</span><span class="sxs-lookup"><span data-stu-id="8e872-146">For more information about what is a pipeline and how it works see <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.</span></span>
   
    <span data-ttu-id="8e872-147">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-147">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. <span data-ttu-id="8e872-148">Hello adatcsatorna toohello képzési dokumentum oszlopnál.</span><span class="sxs-lookup"><span data-stu-id="8e872-148">Fit hello pipeline toohello training document.</span></span> <span data-ttu-id="8e872-149">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-149">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        model = pipeline.fit(training)
3. <span data-ttu-id="8e872-150">Ellenőrizze a hello képzési dokumentum toocheckpoint hello alkalmazással az előrehaladást.</span><span class="sxs-lookup"><span data-stu-id="8e872-150">Verify hello training document toocheckpoint your progress with hello application.</span></span> <span data-ttu-id="8e872-151">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-151">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        training.show()
   
    <span data-ttu-id="8e872-152">Hello kimeneti hasonló toohello következő adja meg:</span><span class="sxs-lookup"><span data-stu-id="8e872-152">This should give hello output similar toohello following:</span></span>
   
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

    <span data-ttu-id="8e872-153">Lépjen vissza, és ellenőrizze a hello kimeneti hello nyers CSV-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="8e872-153">Go back and verify hello output against hello raw CSV file.</span></span> <span data-ttu-id="8e872-154">Hello első sor hello CSV-fájl például ezek az adatok rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8e872-154">For example, hello first row hello CSV file has this data:</span></span>

    <span data-ttu-id="8e872-155">![Kimeneti Spark machine learning például pillanatkép adatai](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "kimeneti adatok pillanatképének elkészítéséhez, Spark machine learning például")</span><span class="sxs-lookup"><span data-stu-id="8e872-155">![Output data snapshot for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Output data snapshot for Spark machine learning example")</span></span>

    <span data-ttu-id="8e872-156">Figyelje meg, hogyan hello tényleges hőmérséklete kisebb, mint hello cél hőmérséklet hello épület javasolhat cold.</span><span class="sxs-lookup"><span data-stu-id="8e872-156">Notice how hello actual temperature is less than hello target temperature suggesting hello building is cold.</span></span> <span data-ttu-id="8e872-157">Ezért hello képzési kimenet, a következő hello **címke** a hello első sor a **0,0**, ami azt jelenti, hogy hello épület nincs gyakran használt adatok.</span><span class="sxs-lookup"><span data-stu-id="8e872-157">Hence in hello training output, hello value for **label** in hello first row is **0.0**, which means hello building is not hot.</span></span>

1. <span data-ttu-id="8e872-158">Készítsen elő egy adatkészlet toorun hello betanított modell ellen.</span><span class="sxs-lookup"><span data-stu-id="8e872-158">Prepare a data set toorun hello trained model against.</span></span> <span data-ttu-id="8e872-159">toodo Igen, azt volna adja át a rendszer Azonosítót és a rendszer kora (jelölése **SystemInfo** hello képzési kimenet), és hello modell előrejelzése volna e hello felépítése az adott rendszer azonosítója és a rendszer kora kellene lennie hotter (kimaradásával 1.0) vagy a hűtőre () helyén 0,0).</span><span class="sxs-lookup"><span data-stu-id="8e872-159">toodo so, we would pass on a system ID and system age (denoted as **SystemInfo** in hello training output), and hello model would predict whether hello building with that system ID and system age would be hotter (denoted by 1.0) or cooler (denoted by 0.0).</span></span>
   
   <span data-ttu-id="8e872-160">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-160">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. <span data-ttu-id="8e872-161">Végezetül előrejelzéseket készítsen a hello tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="8e872-161">Finally, make predictions on hello test data.</span></span> <span data-ttu-id="8e872-162">A következő kódrészletet egy cella üres, és nyomja meg a Beillesztés hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="8e872-162">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. <span data-ttu-id="8e872-163">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8e872-163">You should see an output similar toohello following:</span></span>
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   <span data-ttu-id="8e872-164">Hello első sorában hello előrejelzés, láthatja, hogy a 20-as Azonosítójú és 25 évre rendszer korát HVAC rendszer esetében hello épület lesz működés közbeni (**előrejelzés = 1.0**).</span><span class="sxs-lookup"><span data-stu-id="8e872-164">From hello first row in hello prediction, you can see that for an HVAC system with ID 20 and system age of 25 years, hello building will be hot (**prediction=1.0**).</span></span> <span data-ttu-id="8e872-165">hello DenseVector (0.49999) első értéke megfelel-e toohello előrejelzés 0,0-nál, és hello szükséges második értéke (0.5001) toohello előrejelzés 1.0 felel meg.</span><span class="sxs-lookup"><span data-stu-id="8e872-165">hello first value for DenseVector (0.49999) corresponds toohello  prediction 0.0 and hello second value (0.5001) corresponds toohello prediction 1.0.</span></span> <span data-ttu-id="8e872-166">A hello kimeneti, annak ellenére, hogy hello második értéke csak kis mértékben magasabb hello modell mutatja **előrejelzés = 1.0**.</span><span class="sxs-lookup"><span data-stu-id="8e872-166">In hello output, even though hello second value is only marginally higher, hello model shows **prediction=1.0**.</span></span>
4. <span data-ttu-id="8e872-167">Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8e872-167">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="8e872-168">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="8e872-168">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="8e872-169">Ezzel leállítja és Bezárás hello notebookot.</span><span class="sxs-lookup"><span data-stu-id="8e872-169">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="8e872-170"><a name="anaconda"></a>Használjon Anaconda scikit – ismerje meg a könyvtár a Spark gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="8e872-170"><a name="anaconda"></a>Use Anaconda scikit-learn library for Spark machine learning</span></span>
<span data-ttu-id="8e872-171">Az Apache Spark on hdinsight fürtök Anaconda-könyvtárakkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="8e872-171">Apache Spark clusters on HDInsight include Anaconda libraries.</span></span> <span data-ttu-id="8e872-172">Ebbe beleértendő az is hello **scikit-további** könyvtár a gépi tanulás.</span><span class="sxs-lookup"><span data-stu-id="8e872-172">This also includes hello **scikit-learn** library for machine learning.</span></span> <span data-ttu-id="8e872-173">hello könyvtár is használható toobuild alkalmazásokat közvetlenül a Jupyter notebook különböző adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="8e872-173">hello library also includes various data sets that you can use toobuild sample applications directly from a Jupyter notebook.</span></span> <span data-ttu-id="8e872-174">A példák segítségével hello scikit-könyvtár ismerje meg, lásd: [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span><span class="sxs-lookup"><span data-stu-id="8e872-174">For examples on using hello scikit-learn library, see [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span></span>

## <span data-ttu-id="8e872-175"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8e872-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="8e872-176">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="8e872-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="8e872-177">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="8e872-177">Scenarios</span></span>
* [<span data-ttu-id="8e872-178">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="8e872-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="8e872-179">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="8e872-179">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="8e872-180">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="8e872-180">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="8e872-181">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="8e872-181">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="8e872-182">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="8e872-182">Create and run applications</span></span>
* [<span data-ttu-id="8e872-183">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="8e872-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="8e872-184">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="8e872-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="8e872-185">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="8e872-185">Tools and extensions</span></span>
* [<span data-ttu-id="8e872-186">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="8e872-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="8e872-187">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="8e872-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="8e872-188">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="8e872-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="8e872-189">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="8e872-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="8e872-190">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="8e872-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="8e872-191">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="8e872-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="8e872-192">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="8e872-192">Manage resources</span></span>
* [<span data-ttu-id="8e872-193">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="8e872-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="8e872-194">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="8e872-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
