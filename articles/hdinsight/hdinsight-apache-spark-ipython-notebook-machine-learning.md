---
title: "Az Apache Spark on machine learning-alkalmazások Azure hdinsight létrehozása |} Microsoft Docs"
description: "Lépésenkénti útmutató Apache Spark gépi tanulás alkalmazás elkészítése a HDInsight Spark-fürtök Jupyter notebook használatával"
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
ms.openlocfilehash: 158ade4612104020e0231794e7123ea5cad6c459
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a><span data-ttu-id="5eb9e-103">Az Apache Spark on machine learning-alkalmazások Azure hdinsight létrehozása</span><span class="sxs-lookup"><span data-stu-id="5eb9e-103">Build Apache Spark machine learning applications on Azure HDInsight</span></span>

<span data-ttu-id="5eb9e-104">Ismerje meg, hogyan hozhat létre egy Apache Spark machine learning, hdinsight Spark-fürt használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-104">Learn how to build an Apache Spark machine learning application using a Spark cluster on HDInsight.</span></span> <span data-ttu-id="5eb9e-105">Ez a cikk bemutatja, hogyan használható a Jupyter notebook érhető el a fürt és az alkalmazás teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-105">This article shows how to use the Jupyter notebook available with the cluster to build and test this application.</span></span> <span data-ttu-id="5eb9e-106">Az alkalmazás alapértelmezés szerint minden fürtön található HVAC.csv mintaadatokat használja.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-106">The application uses the sample HVAC.csv data that is available on all clusters by default.</span></span>

<span data-ttu-id="5eb9e-107">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="5eb9e-107">**Prerequisites:**</span></span>

<span data-ttu-id="5eb9e-108">Az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-108">You must have the following:</span></span>

* <span data-ttu-id="5eb9e-109">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-109">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="5eb9e-110">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-110">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> 

## <span data-ttu-id="5eb9e-111"><a name="data"></a>Az adatkészlet ismertetése</span><span class="sxs-lookup"><span data-stu-id="5eb9e-111"><a name="data"></a>Understand the data set</span></span>
<span data-ttu-id="5eb9e-112">Először az alkalmazás összeállítása, mielőtt ossza meg velünk, ismerje meg, amelynek azt létrehozása az alkalmazás és az elemzés végezzük el az adatokat az adatok szerkezete.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-112">Before we start building the application, let us understand the structure of the data for which we build the application and the kind of analysis we will do on the data.</span></span> 

<span data-ttu-id="5eb9e-113">Ebben a cikkben a mintát használjuk **HVAC.csv** adatfájlt, amely a HDInsight-fürthöz társított Azure Storage-fiók érhető el.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-113">In this article, we use the sample **HVAC.csv** data file that is available in the Azure Storage account that you associated with the HDInsight cluster.</span></span> <span data-ttu-id="5eb9e-114">A tárfiókon belül a fájl jelenleg **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-114">Within the storage account, the file is at **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span> <span data-ttu-id="5eb9e-115">Töltse le és nyissa meg a CSV-fájl lekérése az adatok pillanatképe.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-115">Download and open the CSV file to get a snapshot of the data.</span></span>  

<span data-ttu-id="5eb9e-116">![Spark machine learning például használt adatok pillanatképe](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Spark machine learning például használt adatok pillanatképe")</span><span class="sxs-lookup"><span data-stu-id="5eb9e-116">![Snapshot of data used for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Snapshot of data used for Spark machine learning example")</span></span>

<span data-ttu-id="5eb9e-117">Az adatok a cél hőmérséklet és a tényleges hőmérséklet HVAC-rendszerek telepítése épület jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-117">The data shows the target temperature and the actual temperature of a building that has HVAC systems installed.</span></span> <span data-ttu-id="5eb9e-118">Tételezzük fel, hogy a **rendszer** oszlop felel meg a rendszer Azonosítót és a **SystemAge** oszlop felel meg a HVAC rendszer korábban már az épület helyén évek száma.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-118">Let's assume the **System** column represents the system ID and the **SystemAge** column represents the number of years the HVAC system has been in place at the building.</span></span>

<span data-ttu-id="5eb9e-119">Ezek az adatok előre jelezni, hogy egy épületet hotter vagy lesz colder alapján a cél hőmérséklet, a rendszer azonosítója és a rendszer élettartamát a használjuk.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-119">We use this data to predict whether a building will be hotter or colder based on the target temperature, given a system ID and system age.</span></span>

## <span data-ttu-id="5eb9e-120"><a name="app"></a>Spark machine learning alkalmazás Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="5eb9e-120"><a name="app"></a>Write a Spark machine learning application using Spark MLlib</span></span>
<span data-ttu-id="5eb9e-121">Ebben az alkalmazásban a Spark ML folyamat használatával hajtja végre a dokumentum besorolását használjuk.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-121">In this application we use a Spark ML pipeline to perform a document classification.</span></span> <span data-ttu-id="5eb9e-122">A folyamat azt a dokumentum felosztása szavakat, a szavakat átalakítása egy numerikus szolgáltatás vektoros és végül a egy előrejelzési modell szolgáltatás vektorok és címkék létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-122">In the pipeline, we split the document into words, convert the words into a numerical feature vector, and finally build a prediction model using the feature vectors and labels.</span></span> <span data-ttu-id="5eb9e-123">A következő lépésekkel hozhat létre az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-123">Perform the following steps to create the application.</span></span>

1. <span data-ttu-id="5eb9e-124">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-124">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="5eb9e-125">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-125">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="5eb9e-126">A Spark-fürt panelén kattintson a **Fürt irányítópultja Dashboard** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-126">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="5eb9e-127">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-127">If prompted, enter the admin credentials for the cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5eb9e-128">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-128">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="5eb9e-129">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-129">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. <span data-ttu-id="5eb9e-130">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-130">Create a new notebook.</span></span> <span data-ttu-id="5eb9e-131">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-131">Click **New**, and then click **PySpark**.</span></span>
   
    <span data-ttu-id="5eb9e-132">![Spark machine learning például Jupyter notebook létrehozása](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Spark machine learning például Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="5eb9e-132">![Create a Jupyter notebook for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Create a Jupyter notebook for Spark machine learning example")</span></span>
4. <span data-ttu-id="5eb9e-133">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-133">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="5eb9e-134">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-134">Click the notebook name at the top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="5eb9e-135">![Adja meg a notebook neve Spark machine learning például](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "notebook nevezze el a Spark machine learning – példa")</span><span class="sxs-lookup"><span data-stu-id="5eb9e-135">![Provide a notebook name for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Provide a notebook name for Spark machine learning example")</span></span>
5. <span data-ttu-id="5eb9e-136">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-136">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="5eb9e-137">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-137">The Spark and Hive contexts will be automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="5eb9e-138">Ehhez a forgatókönyvhöz szükséges típusok importálása megkezdése.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-138">You can start by importing the types that are required for this scenario.</span></span> <span data-ttu-id="5eb9e-139">Illessze be a következő kódrészletet a cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-139">Paste the following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span> 
   
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
6. <span data-ttu-id="5eb9e-140">Meg kell most betölteni az adatokat (hvac.csv), elemzéséhez és a modell betanításához segítségével.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-140">You must now load the data (hvac.csv), parse it, and use it to train the model.</span></span> <span data-ttu-id="5eb9e-141">Ehhez adja meg egy függvényt, amely ellenőrzi, hogy a tényleges hőmérséklet az épület nagyobb, mint a célként megadott hőmérsékletet.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-141">For this, you define a function that checks whether the actual temperature of the building is greater than the target temperature.</span></span> <span data-ttu-id="5eb9e-142">A tényleges hőmérséklet nagyobb, az épület akkor gyakran használt adatok, a értékkel megjelölt **1.0**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-142">If the actual temperature is greater, the building is hot, denoted by the value **1.0**.</span></span> <span data-ttu-id="5eb9e-143">A tényleges hőmérséklet kevesebb, az épület akkor cold, a értékkel megjelölt **0,0**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-143">If the actual temperature is lesser, the building is cold, denoted by the value **0.0**.</span></span> 
   
    <span data-ttu-id="5eb9e-144">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-144">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>

        # List the structure of data for better understanding. Because the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. <span data-ttu-id="5eb9e-145">Konfigurálja a Spark machine learning-feldolgozási folyamat három szakaszból áll: jogkivonatokat létrehozó hashingTF és lr.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-145">Configure the Spark machine learning pipeline that consists of three stages: tokenizer, hashingTF, and lr.</span></span> <span data-ttu-id="5eb9e-146">Mi az, hogy egy folyamat és annak működéséről lásd: További információ <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning adatcsatorna</a>.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-146">For more information about what is a pipeline and how it works see <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.</span></span>
   
    <span data-ttu-id="5eb9e-147">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-147">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. <span data-ttu-id="5eb9e-148">A folyamat a képzés dokumentumhoz oszlopnál.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-148">Fit the pipeline to the training document.</span></span> <span data-ttu-id="5eb9e-149">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-149">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        model = pipeline.fit(training)
3. <span data-ttu-id="5eb9e-150">Ellenőrizze a képzés dokumentum ellenőrzőpont az előrehaladást, az alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-150">Verify the training document to checkpoint your progress with the application.</span></span> <span data-ttu-id="5eb9e-151">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-151">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        training.show()
   
    <span data-ttu-id="5eb9e-152">Adja meg a kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-152">This should give the output similar to the following:</span></span>
   
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

    <span data-ttu-id="5eb9e-153">Lépjen vissza, és ellenőrizze a kimeneti szemben a nyers CSV-fájlt.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-153">Go back and verify the output against the raw CSV file.</span></span> <span data-ttu-id="5eb9e-154">Az első sor a CSV-fájl például ezek az adatok rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-154">For example, the first row the CSV file has this data:</span></span>

    <span data-ttu-id="5eb9e-155">![Kimeneti Spark machine learning például pillanatkép adatai](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "kimeneti adatok pillanatképének elkészítéséhez, Spark machine learning például")</span><span class="sxs-lookup"><span data-stu-id="5eb9e-155">![Output data snapshot for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Output data snapshot for Spark machine learning example")</span></span>

    <span data-ttu-id="5eb9e-156">Figyelje meg, hogyan tényleges hőmérséklete kisebb, mint a cél hőmérséklet javasolhat az épület cold.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-156">Notice how the actual temperature is less than the target temperature suggesting the building is cold.</span></span> <span data-ttu-id="5eb9e-157">Ezért a következő kimeneti, képzés **címke** az első sorban van **0,0**, ami azt jelenti, hogy az épület nincs gyakran használt adatok.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-157">Hence in the training output, the value for **label** in the first row is **0.0**, which means the building is not hot.</span></span>

1. <span data-ttu-id="5eb9e-158">Készítse el adatok a betanított modell tanítását futtatásához.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-158">Prepare a data set to run the trained model against.</span></span> <span data-ttu-id="5eb9e-159">Ehhez az szükséges, azt szeretné továbbítani egy rendszer-Azonosítót és a rendszer kora (jelölése **SystemInfo** képzési kimenet), és a modell volna előre jelezni, hogy az adott rendszer-azonosító és a rendszer kor az épület hotter lenne (1.0 jelölik) vagy (kimaradásával hűvösebben 0,0).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-159">To do so, we would pass on a system ID and system age (denoted as **SystemInfo** in the training output), and the model would predict whether the building with that system ID and system age would be hotter (denoted by 1.0) or cooler (denoted by 0.0).</span></span>
   
   <span data-ttu-id="5eb9e-160">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-160">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. <span data-ttu-id="5eb9e-161">Végezetül előrejelzéseket készítsen a a tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-161">Finally, make predictions on the test data.</span></span> <span data-ttu-id="5eb9e-162">Illessze be az alábbi részlet egy cella üres, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-162">Paste the following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. <span data-ttu-id="5eb9e-163">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-163">You should see an output similar to the following:</span></span>
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   <span data-ttu-id="5eb9e-164">Az első sorból a előrejelzését, láthatja, hogy a 20-as Azonosítójú és 25 évre rendszer korát HVAC rendszer esetében az épület lesz működés közbeni (**előrejelzés = 1.0**).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-164">From the first row in the prediction, you can see that for an HVAC system with ID 20 and system age of 25 years, the building will be hot (**prediction=1.0**).</span></span> <span data-ttu-id="5eb9e-165">Az előrejelzés 0,0 DenseVector (0.49999) első értéke megfelel-e, és a második érték (0.5001) 1.0 előrejelzését felel meg.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-165">The first value for DenseVector (0.49999) corresponds to the  prediction 0.0 and the second value (0.5001) corresponds to the prediction 1.0.</span></span> <span data-ttu-id="5eb9e-166">A kimenetben, annak ellenére, hogy a második értéke csak kis mértékben nagyobb, a modell bemutatja **előrejelzés = 1.0**.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-166">In the output, even though the second value is only marginally higher, the model shows **prediction=1.0**.</span></span>
4. <span data-ttu-id="5eb9e-167">Az alkalmazás futtatását követően állítsa le a notebookot az erőforrások felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-167">After you have finished running the application, you should shutdown the notebook to release the resources.</span></span> <span data-ttu-id="5eb9e-168">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-168">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="5eb9e-169">Ezzel leállítja és bezárja a notebookot.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-169">This will shutdown and close the notebook.</span></span>

## <span data-ttu-id="5eb9e-170"><a name="anaconda"></a>Használjon Anaconda scikit – ismerje meg a könyvtár a Spark gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="5eb9e-170"><a name="anaconda"></a>Use Anaconda scikit-learn library for Spark machine learning</span></span>
<span data-ttu-id="5eb9e-171">Az Apache Spark on hdinsight fürtök Anaconda-könyvtárakkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-171">Apache Spark clusters on HDInsight include Anaconda libraries.</span></span> <span data-ttu-id="5eb9e-172">Ez magában foglalja a **scikit-további** könyvtár a machine Learning szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-172">This also includes the **scikit-learn** library for machine learning.</span></span> <span data-ttu-id="5eb9e-173">A könyvtár is tartalmaz, amelyek közvetlenül a Jupyter notebook minta alkalmazásokat hozhatnak létre különböző adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="5eb9e-173">The library also includes various data sets that you can use to build sample applications directly from a Jupyter notebook.</span></span> <span data-ttu-id="5eb9e-174">A scikit használatát bemutató példák-könyvtár ismerje meg, lásd: [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span><span class="sxs-lookup"><span data-stu-id="5eb9e-174">For examples on using the scikit-learn library, see [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span></span>

## <span data-ttu-id="5eb9e-175"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5eb9e-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="5eb9e-176">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="5eb9e-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="5eb9e-177">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5eb9e-177">Scenarios</span></span>
* [<span data-ttu-id="5eb9e-178">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="5eb9e-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="5eb9e-179">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="5eb9e-179">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="5eb9e-180">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="5eb9e-180">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="5eb9e-181">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="5eb9e-181">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="5eb9e-182">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="5eb9e-182">Create and run applications</span></span>
* [<span data-ttu-id="5eb9e-183">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="5eb9e-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="5eb9e-184">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="5eb9e-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="5eb9e-185">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="5eb9e-185">Tools and extensions</span></span>
* [<span data-ttu-id="5eb9e-186">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="5eb9e-186">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="5eb9e-187">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="5eb9e-187">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="5eb9e-188">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="5eb9e-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="5eb9e-189">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="5eb9e-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="5eb9e-190">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="5eb9e-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="5eb9e-191">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="5eb9e-191">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="5eb9e-192">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="5eb9e-192">Manage resources</span></span>
* [<span data-ttu-id="5eb9e-193">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="5eb9e-193">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="5eb9e-194">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="5eb9e-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
