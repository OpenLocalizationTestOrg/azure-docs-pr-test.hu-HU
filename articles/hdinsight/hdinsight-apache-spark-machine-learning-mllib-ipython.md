---
title: "Machine learning például a hdinsight - Azure Spark MLlib |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy machine learning-alkalmazást, amely elemzi a DataSet adatkészlet besorolásával keresztül logisztikai regresszió Spark MLlib segítségével."
keywords: "Spark gépi tanulási, spark machine learning – példa"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: e563d4f51c9e27b20df47eca6d3eb00ac79e854f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-spark-mllib-to-build-a-machine-learning-application-and-analyze-a-dataset"></a><span data-ttu-id="4ffaf-104">Spark MLlib segítségével a machine learning-alkalmazás létrehozása és elemzése a DataSet adatkészlet</span><span class="sxs-lookup"><span data-stu-id="4ffaf-104">Use Spark MLlib to build a machine learning application and analyze a dataset</span></span>

<span data-ttu-id="4ffaf-105">Használata Spark **MLlib** machine learning-alkalmazás egy megnyitott adatkészlethez egyszerű prediktív elemzési létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-105">Learn how to use Spark **MLlib** to create a machine learning application to do simple predictive analysis on an open dataset.</span></span> <span data-ttu-id="4ffaf-106">A Spark beépített gépi tanulási szalagtárak, a példában az *besorolás* logisztikai regresszió keresztül.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-106">From Spark's built-in machine learning libraries, this example uses *classification* through logistic regression.</span></span> 

> [!TIP]
> <span data-ttu-id="4ffaf-107">Ebben a példában, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-107">This example is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="4ffaf-108">A notebook élmény lehetővé teszi a Python kódtöredékek futtassa a notebook magát.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-108">The notebook experience lets you run the Python snippets from the notebook itself.</span></span> <span data-ttu-id="4ffaf-109">Hajtsa végre az oktatóprogram a notebook belül, Spark-fürt létrehozása, és indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span><span class="sxs-lookup"><span data-stu-id="4ffaf-109">To follow the tutorial from within a notebook, create a Spark cluster and launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span></span> <span data-ttu-id="4ffaf-110">Ezután futtassa a notebook **Spark Machine Learning - étele ellenőrző adatok MLlib.ipynb a prediktív elemzési** alatt a **Python** mappát.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-110">Then, run the notebook **Spark Machine Learning - Predictive analysis on food inspection data using MLlib.ipynb** under the **Python** folder.</span></span>
>
>

<span data-ttu-id="4ffaf-111">MLlib egy Spark Alapkönyvtár, amely számos hasznos segédprogramokat biztosít gépi tanulási feladatok, beleértve a segédprogramok:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-111">MLlib is a core Spark library that provides many utilities useful for machine learning tasks, including utilities that are suitable for:</span></span>

* <span data-ttu-id="4ffaf-112">Besorolás</span><span class="sxs-lookup"><span data-stu-id="4ffaf-112">Classification</span></span>
* <span data-ttu-id="4ffaf-113">Regressziós</span><span class="sxs-lookup"><span data-stu-id="4ffaf-113">Regression</span></span>
* <span data-ttu-id="4ffaf-114">Fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4ffaf-114">Clustering</span></span>
* <span data-ttu-id="4ffaf-115">A témakör modellezési</span><span class="sxs-lookup"><span data-stu-id="4ffaf-115">Topic modeling</span></span>
* <span data-ttu-id="4ffaf-116">Szinguláris érték felbontás ellen (SVD) és egyszerű összetevő elemzés (PEM)</span><span class="sxs-lookup"><span data-stu-id="4ffaf-116">Singular value decomposition (SVD) and principal component analysis (PCA)</span></span>
* <span data-ttu-id="4ffaf-117">Alapul, tesztelése és minta számításához</span><span class="sxs-lookup"><span data-stu-id="4ffaf-117">Hypothesis testing and calculating sample statistics</span></span>

## <a name="what-are-classification-and-logistic-regression"></a><span data-ttu-id="4ffaf-118">Mik azok a besorolás és logisztikai regresszió?</span><span class="sxs-lookup"><span data-stu-id="4ffaf-118">What are classification and logistic regression?</span></span>
<span data-ttu-id="4ffaf-119">*Besorolási*népszerű Machine learning-feladat, az a folyamat a bemeneti adatok rendezése kategóriákba.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-119">*Classification*, a popular machine learning task, is the process of sorting input data into categories.</span></span> <span data-ttu-id="4ffaf-120">A feladat is található egy osztályozó algoritmus hozzárendelése "címkék" adatokat, hogy a felhasználótól, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-120">It is the job of a classification algorithm to figure out how to assign "labels" to input data that you provide.</span></span> <span data-ttu-id="4ffaf-121">Sikerült a például a gépi tanulási algoritmus, amely fogadja bemeneti adatként készlet információkat, és elosztja a készlet két kategóriába sorolhatók véleménye: kell értékesítő készletek és a készletek, amelyek kell tartania.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-121">For example, you could think of a machine learning algorithm that accepts stock information as input and divides the stock into two categories: stocks that you should sell and stocks that you should keep.</span></span>

<span data-ttu-id="4ffaf-122">Logisztikai regresszió a besorolási használt algoritmus.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-122">Logistic regression is the algorithm that you use for classification.</span></span> <span data-ttu-id="4ffaf-123">A Spark logisztikai regresszió API akkor hasznos, ha *bináris osztályozási*, vagy egy két bemeneti adatok besorolása.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-123">Spark's logistic regression API is useful for *binary classification*, or classifying input data into one of two groups.</span></span> <span data-ttu-id="4ffaf-124">További információ a logisztikai regresszió: [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span><span class="sxs-lookup"><span data-stu-id="4ffaf-124">For more information about logistic regressions, see [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span></span>

<span data-ttu-id="4ffaf-125">Összefoglalva, a logisztikai regresszió illesztése egy *logisztikai függvény* , amely használható a valószínűsége annak, hogy egy bemeneti vektort tartozik egy csoport vagy egyéb előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-125">In summary, the process of logistic regression produces a *logistic function* that can be used to predict the probability that an input vector belongs in one group or the other.</span></span>  

## <a name="predictive-analysis-example-on-food-inspection-data"></a><span data-ttu-id="4ffaf-126">A prediktív elemzés példa étele ellenőrző adatok</span><span class="sxs-lookup"><span data-stu-id="4ffaf-126">Predictive analysis example on food inspection data</span></span>
<span data-ttu-id="4ffaf-127">Ebben a példában használható Spark néhány prediktív elemzési étele ellenőrző adatokon (**Food_Inspections1.csv**) keresztül szerzett része, amely a [város a Chicagói adat portált](https://data.cityofchicago.org/).</span><span class="sxs-lookup"><span data-stu-id="4ffaf-127">In this example, you use Spark to perform some predictive analysis on food inspection data (**Food_Inspections1.csv**) that was acquired through the [City of Chicago data portal](https://data.cityofchicago.org/).</span></span> <span data-ttu-id="4ffaf-128">Ez az adatkészlet chicagóban, beleértve az egyes létrehozását, a szabálytalanság található (ha van ilyen) és az ellenőrzés eredményét végezték étele kialakítása ellenőrzésekre információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-128">This dataset contains information about food establishment inspections that were conducted in Chicago, including information about each establishment, the violations found (if any), and the results of the inspection.</span></span> <span data-ttu-id="4ffaf-129">Az adatok CSV-fájl már megtalálható a fürthöz rendelt tárolási fiók **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-129">The CSV data file is already available in the storage account associated with the cluster at **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span></span>

<span data-ttu-id="4ffaf-130">Az alábbi lépéseket, a most kialakított egy modell megtekintéséhez, mi tart, vagy a étele ellenőrzése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-130">In the steps below, you develop a model to see what it takes to pass or fail a food inspection.</span></span>

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a><span data-ttu-id="4ffaf-131">Indítsa el a Spark MMLib machine learning-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ffaf-131">Start building a Spark MMLib machine learning app</span></span>
1. <span data-ttu-id="4ffaf-132">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="4ffaf-132">From the [Azure portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="4ffaf-133">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="4ffaf-133">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
1. <span data-ttu-id="4ffaf-134">A Spark-fürt panelén kattintson a **Fürt irányítópultja Dashboard** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-134">From the Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="4ffaf-135">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-135">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="4ffaf-136">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-136">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="4ffaf-137">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-137">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. <span data-ttu-id="4ffaf-138">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-138">Create a notebook.</span></span> <span data-ttu-id="4ffaf-139">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-139">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="4ffaf-140">![A Jupyter notebook létrehozása](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "egy új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="4ffaf-140">![Create a Jupyter notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "Create a new Jupyter notebook")</span></span>
1. <span data-ttu-id="4ffaf-141">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-141">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="4ffaf-142">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-142">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="4ffaf-143">![Adjon nevet a notebooknak](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Adjon nevet a notebooknak")</span><span class="sxs-lookup"><span data-stu-id="4ffaf-143">![Provide a name for the notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Provide a name for the notebook")</span></span>
1. <span data-ttu-id="4ffaf-144">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-144">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="4ffaf-145">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-145">The Spark and Hive contexts are automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="4ffaf-146">A machine learning alkalmazás ehhez a forgatókönyvhöz szükséges típusok importálása felépítése elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-146">You can start building your machine learning application by importing the types required for this scenario.</span></span> <span data-ttu-id="4ffaf-147">Ehhez az szükséges, vigye a kurzort a cella, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-147">To do so, place the cursor in the cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a><span data-ttu-id="4ffaf-148">Egy bemeneti dataframe szerkezet</span><span class="sxs-lookup"><span data-stu-id="4ffaf-148">Construct an input dataframe</span></span>
<span data-ttu-id="4ffaf-149">Használhatunk `sqlContext` csatlakoztatva átalakításokat hajthattak végre a strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-149">We can use `sqlContext` to perform transformations on structured data.</span></span> <span data-ttu-id="4ffaf-150">Az első feladata a mintaadatok betöltése ((**Food_Inspections1.csv**)) egy Spark SQL *dataframe*.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-150">The first task is to load the sample data ((**Food_Inspections1.csv**)) into a Spark SQL *dataframe*.</span></span>

1. <span data-ttu-id="4ffaf-151">Mivel a nyers adatok CSV formátumban, igazolnia kell a Spark-környezet használata való lekérésére a fájl minden sora a memóriába strukturálatlan szövegként; Ezt követően használja a Python a fürt megosztott kötetei szolgáltatás könyvtár minden sort külön-külön elemzése.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-151">Because the raw data is in a CSV format, we need to use the Spark context to pull every line of the file into memory as unstructured text; then, you use Python's CSV library to parse each line individually.</span></span>

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. <span data-ttu-id="4ffaf-152">Most már tudunk a CSV-fájlt, egy RDD.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-152">We now have the CSV file as an RDD.</span></span>  <span data-ttu-id="4ffaf-153">Szeretné megtudni, az adatok a séma, nem beolvasni egy sort a RDD.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-153">To understand the schema of the data, we retrieve one row from the RDD.</span></span>

        inspections.take(1)

    <span data-ttu-id="4ffaf-154">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-154">You should see an output like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. <span data-ttu-id="4ffaf-155">Az előző kimeneti biztosítanak számunkra a bemeneti fájl séma képet.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-155">The preceding output gives us an idea of the schema of the input file.</span></span> <span data-ttu-id="4ffaf-156">Ez magában foglalja a minden létrehozását, a kialakítása, a címet, és az ellenőrzések, a helyre, többek között az adatok nevét.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-156">It includes the name of every establishment, the type of establishment, the address, the data of the inspections, and the location, among other things.</span></span> <span data-ttu-id="4ffaf-157">Néhány oszlopot, amely a prediktív elemzési során hasznosak, és csoportosítsa az eredményeket egy dataframe, amely majd használatával hozzon létre egy ideiglenes táblát, most kiválasztása</span><span class="sxs-lookup"><span data-stu-id="4ffaf-157">Let's select a few columns that are useful for our predictive analysis and group the results as a dataframe, which we then use to create a temporary table.</span></span>

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. <span data-ttu-id="4ffaf-158">Most már rendelkezik egy *dataframe*, `df` amelyen most a elemzés elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-158">We now have a *dataframe*, `df` on which we can perform our analysis.</span></span> <span data-ttu-id="4ffaf-159">Egy ideiglenes tábla hívás is tudunk **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-159">We also have a temporary table call **CountResults**.</span></span> <span data-ttu-id="4ffaf-160">Azt a dataframe jelentőséggel négy oszlop szerepel: **azonosító**, **neve**, **eredmények**, és **megsértésének**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-160">We've included four columns of interest in the dataframe: **id**, **name**, **results**, and **violations**.</span></span>

    <span data-ttu-id="4ffaf-161">Folytassuk az adatok mintát:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-161">Let's get a small sample of the data:</span></span>

        df.show(5)

    <span data-ttu-id="4ffaf-162">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-162">You should see an output like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a><span data-ttu-id="4ffaf-163">Az adatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="4ffaf-163">Understand the data</span></span>
1. <span data-ttu-id="4ffaf-164">Kezdjük, hogy az az adatkészletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-164">Let's start to get a sense of what our dataset contains.</span></span> <span data-ttu-id="4ffaf-165">Például a különböző értékeket Mik a **eredmények** oszlop?</span><span class="sxs-lookup"><span data-stu-id="4ffaf-165">For example, what are the different values in the **results** column?</span></span>

        df.select('results').distinct().show()

    <span data-ttu-id="4ffaf-166">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-166">You should see an output like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. <span data-ttu-id="4ffaf-167">Gyors képi megjelenítés segíthet OK információ ezen eredmények a terjesztéséről.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-167">A quick visualization can help us reason about the distribution of these outcomes.</span></span> <span data-ttu-id="4ffaf-168">Egy ideiglenes tábla az adatok már tudunk **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-168">We already have the data in a temporary table **CountResults**.</span></span> <span data-ttu-id="4ffaf-169">A következő SQL-lekérdezés futtatható jobb megértése, hogyan jutnak el az eredmények eléréséhez a táblázaton.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-169">You can run the following SQL query against the table to get a better understanding of how the results are distributed.</span></span>

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    <span data-ttu-id="4ffaf-170">A `%%sql` magic követ `-o countResultsdf` biztosítja, hogy a lekérdezés kimenetét a Jupyter kiszolgálón (általában a fürt headnode) helyileg megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-170">The `%%sql` magic followed by `-o countResultsdf` ensures that the output of the query is persisted locally on the Jupyter server (typically the headnode of the cluster).</span></span> <span data-ttu-id="4ffaf-171">A kimeneti tárolva a [Pandas](http://pandas.pydata.org/) a megadott nevű dataframe **countResultsdf**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-171">The output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with the specified name **countResultsdf**.</span></span>

    <span data-ttu-id="4ffaf-172">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-172">You should see an output like the following:</span></span>

    <span data-ttu-id="4ffaf-173">![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL-lekérdezés kimenete")</span><span class="sxs-lookup"><span data-stu-id="4ffaf-173">![SQL query output](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL query output")</span></span>

    <span data-ttu-id="4ffaf-174">A `%%sql` funkcióval, illetve a PySpark kernellel elérhető egyéb funkciókkal kapcsolatos további információkat [A Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-174">For more information about the `%%sql` magic, and other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
1. <span data-ttu-id="4ffaf-175">Matplotlib, a szalagtár segítségével hozza létre az adatok, a képi megjelenítés segítségével rajzot.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-175">You can also use Matplotlib, a library used to construct visualization of data, to create a plot.</span></span> <span data-ttu-id="4ffaf-176">Mert a rajzolási létre kell hozni a helyileg tárolt **countResultsdf** dataframe, a kódrészletet a következővel kell kezdődnie az `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-176">Because the plot must be created from the locally persisted **countResultsdf** dataframe, the code snippet must begin with the `%%local` magic.</span></span> <span data-ttu-id="4ffaf-177">Ez biztosítja, hogy a kód a Jupyter kiszolgálón helyileg futnak.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-177">This ensures that the code is run locally on the Jupyter server.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="4ffaf-178">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-178">You should see an output like the following:</span></span>

    <span data-ttu-id="4ffaf-179">![Spark gépi tanulási a alkalmazás kimeneti - öt különböző vizsgálat eredményekkel kördiagram](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark gépi tanulási a eredmény kimenete")</span><span class="sxs-lookup"><span data-stu-id="4ffaf-179">![Spark machine learning application output - pie chart with five distinct inspection results](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning result output")</span></span>
1. <span data-ttu-id="4ffaf-180">Láthatja, hogy vannak-e 5 különböző eredményeket, amelyeken az ellenőrzés:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-180">You can see that there are 5 distinct results that an inspection can have:</span></span>

   * <span data-ttu-id="4ffaf-181">Nem található üzleti</span><span class="sxs-lookup"><span data-stu-id="4ffaf-181">Business not located</span></span>
   * <span data-ttu-id="4ffaf-182">Sikertelen</span><span class="sxs-lookup"><span data-stu-id="4ffaf-182">Fail</span></span>
   * <span data-ttu-id="4ffaf-183">Fázis</span><span class="sxs-lookup"><span data-stu-id="4ffaf-183">Pass</span></span>
   * <span data-ttu-id="4ffaf-184">Feltételek keresztüli rendelkező PSS</span><span class="sxs-lookup"><span data-stu-id="4ffaf-184">Pss w/ conditions</span></span>
   * <span data-ttu-id="4ffaf-185">Üzleti kívül</span><span class="sxs-lookup"><span data-stu-id="4ffaf-185">Out of Business</span></span>

     <span data-ttu-id="4ffaf-186">Ossza meg velünk fejlesztése modell, amely kitalálni a egy étele ellenőrző eredményeit a megsértésének megadott.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-186">Let us develop a model that can guess the outcome of a food inspection, given the violations.</span></span> <span data-ttu-id="4ffaf-187">Mivel logisztikai regresszió egy bináris osztályozási módszer, így az adatok csoportosításához két kategóriába sorolhatók: **sikertelen** és **átadni**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-187">Since logistic regression is a binary classification method, it makes sense to group our data into two categories: **Fail** and **Pass**.</span></span> <span data-ttu-id="4ffaf-188">Egy "átadni keresztüli rendelkező feltételek" még nem állt le a hozzáférési, ha azt a modell betanítását, javasolt a két eredményt egyenértékű.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-188">A "Pass w/ Conditions" is still a Pass, so when we train the model, we consider the two results equivalent.</span></span> <span data-ttu-id="4ffaf-189">Az eredmények ("Üzleti nem található" vagy "Out üzleti") adatok, hogy távolítsa el azokat a gyakorlókészlethez nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-189">Data with the other results ("Business Not Located" or "Out of Business") are not useful so we remove them from our training set.</span></span> <span data-ttu-id="4ffaf-190">Ez kell lennie rendben, mivel ezek két kategóriába mégis jött létre az eredmények nagyon kis százalékát.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-190">This should be okay since these two categories make up a very small percentage of the results anyway.</span></span>
1. <span data-ttu-id="4ffaf-191">Ossza meg velünk lépjen tovább, és alakítsa át a meglévő dataframe (`df`) egy új dataframe, ahol minden egyes ellenőrzés címke-megsértésének párban szerepel-e be.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-191">Let us go ahead and convert our existing dataframe(`df`) into a new dataframe where each inspection is represented as a label-violations pair.</span></span> <span data-ttu-id="4ffaf-192">Ebben az esetben a címke `0.0` jelenti. a hiba, a címkék `1.0` sikeres, és a címke jelöli `-1.0` egyes eredményeket e két mellett jelöli.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-192">In our case, a label of `0.0` represents a failure, a label of `1.0` represents a success, and a label of `-1.0` represents some results besides those two.</span></span> <span data-ttu-id="4ffaf-193">A Microsoft kiszűrhetők az egyéb eredmények az új adatok keret számításakor.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-193">We filter those other results out when computing the new data frame.</span></span>

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    <span data-ttu-id="4ffaf-194">Milyen a címkézett adatok tűnik, hogy most egy olyan sor beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-194">To see what the labeled data looks like, let's retrieve one row.</span></span>

        labeledData.take(1)

    <span data-ttu-id="4ffaf-195">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-195">You should see an output like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]

## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a><span data-ttu-id="4ffaf-196">A bemeneti dataframe logisztikai regressziós modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ffaf-196">Create a logistic regression model from the input dataframe</span></span>
<span data-ttu-id="4ffaf-197">A végső feladata a teljesítményadatokká alakítani a címkézett logisztikai regresszió által elemezhető formátumú.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-197">Our final task is to convert the labeled data into a format that can be analyzed by logistic regression.</span></span> <span data-ttu-id="4ffaf-198">A bemeneti logisztikai regresszió algoritmushoz kell lenniük, amelyek *címke-funkció vektoros párok*, ahol a "szolgáltatás"vektor a bemeneti pontot jelölő számok vektor.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-198">The input to a logistic regression algorithm should be a set of *label-feature vector pairs*, where the "feature vector" is a vector of numbers representing the input point.</span></span> <span data-ttu-id="4ffaf-199">Igen igazolnia kell alakítani az "megsértésének", amely félig strukturált és a szabad szöveges, a gép könnyen értelmezhető formára valós szám tömbjének sok megjegyzéseket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-199">So, we need to convert the "violations" column, which is semi-structured and contains many comments in free-text, to an array of real numbers that a machine could easily understand.</span></span>

<span data-ttu-id="4ffaf-200">Tanulási megközelítés természetes nyelvű feldolgozása egy szabványos számítógép, hogy minden distinct szó hozzárendelése egy "index", és akkor továbbítja a vektoros a gépi tanulási algoritmus úgy, hogy minden egyes súgóindex-értéket tartalmaz a relatív gyakoriságot karakterláncban szó.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-200">One standard machine learning approach for processing natural language is to assign each distinct word an "index", and then pass a vector to the machine learning algorithm such that each index's value contains the relative frequency of that word in the text string.</span></span>

<span data-ttu-id="4ffaf-201">MLlib leegyszerűsíti a művelet elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-201">MLlib provides an easy way to perform this operation.</span></span> <span data-ttu-id="4ffaf-202">Első lépésként "tokenize" minden megsértésének karakterlánc beolvasása az egyes szavak minden karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-202">First, "tokenize" each violations string to get the individual words in each string.</span></span> <span data-ttu-id="4ffaf-203">Ezt követően egy `HashingTF` jogkivonatok minden készlete alakítani egy szolgáltatás vektort, majd adhatók át a logisztikai regresszió algoritmus a modell összeállításához.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-203">Then, use a `HashingTF` to convert each set of tokens into a feature vector that can then be passed to the logistic regression algorithm to construct a model.</span></span> <span data-ttu-id="4ffaf-204">Azt végezze el az összes lépés sorrendben használja a "futószalag".</span><span class="sxs-lookup"><span data-stu-id="4ffaf-204">We conduct all of these steps in sequence using a "pipeline".</span></span>

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-the-model-on-a-separate-test-dataset"></a><span data-ttu-id="4ffaf-205">Értékelje ki a modellt a külön tesztelési adatkészletre</span><span class="sxs-lookup"><span data-stu-id="4ffaf-205">Evaluate the model on a separate test dataset</span></span>
<span data-ttu-id="4ffaf-206">A korábban létrehozott modell használhatjuk *előrejelzése* mely új ellenőrzések eredményeinek kell, a megfigyelt szabálysértések alapján.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-206">We can use the model we created earlier to *predict* what the results of new inspections will be, based on the violations that were observed.</span></span> <span data-ttu-id="4ffaf-207">Ebben a modellben az adatkészlettel betanítása azt **Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-207">We trained this model on the dataset **Food_Inspections1.csv**.</span></span> <span data-ttu-id="4ffaf-208">Ossza meg velünk használjon egy második dataset **Food_Inspections2.csv**található *kiértékelése* ebben a modellben az új adatok erősségével.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-208">Let us use a second dataset, **Food_Inspections2.csv**, to *evaluate* the strength of this model on new data.</span></span> <span data-ttu-id="4ffaf-209">A második készlet (**Food_Inspections2.csv**) már a fürthöz tartozó alapértelmezett tárolót kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-209">This second data set (**Food_Inspections2.csv**) should already be in the default storage container associated with the cluster.</span></span>

1. <span data-ttu-id="4ffaf-210">A következő kódrészletet hoz létre egy új dataframe **predictionsDf** , amely tartalmazza a modell által generált előrejelzését.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-210">The following snippet creates a new dataframe, **predictionsDf** that contains the prediction generated by the model.</span></span> <span data-ttu-id="4ffaf-211">A kódrészletben is létrehoz egy ideiglenes táblát **előrejelzéseket** a dataframe alapján.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-211">The snippet also creates a temporary table called **Predictions** based on the dataframe.</span></span>

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    <span data-ttu-id="4ffaf-212">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-212">You should see an output like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. <span data-ttu-id="4ffaf-213">Tekintse meg az előrejelzés egyikét.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-213">Look at one of the predictions.</span></span> <span data-ttu-id="4ffaf-214">Futtassa ezt a kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-214">Run this snippet:</span></span>

        predictionsDf.take(1)

   <span data-ttu-id="4ffaf-215">Nincs olyan előrejelzésre lévő teszt az első bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-215">There is a prediction for the first entry in the test data set.</span></span>
1. <span data-ttu-id="4ffaf-216">A `model.transform()` metódus ugyanazt az átalakítást vonatkozik semmilyen új adatot az azonos sémával, és hogyan adatok előrejelzése érkeznek.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-216">The `model.transform()` method applies the same transformation to any new data with the same schema, and arrive at a prediction of how to classify the data.</span></span> <span data-ttu-id="4ffaf-217">Hogyan pontos volt az előrejelzéseket, hogy néhány egyszerű statisztikai tehetünk ennek:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-217">We can do some simple statistics to get a sense of how accurate our predictions were:</span></span>

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    <span data-ttu-id="4ffaf-218">A kimenet a következőképpen néznek:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-218">The output looks like the following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    <span data-ttu-id="4ffaf-219">Logisztikai regresszió használata Spark lehetőséget ad olyan pontos modell megsértésének leírását angol nyelven, és hogy egy adott üzleti volna továbbítja, vagy a étele ellenőrzése sikertelen közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-219">Using logistic regression with Spark gives us an accurate model of the relationship between violations descriptions in English and whether a given business would pass or fail a food inspection.</span></span>

## <a name="create-a-visual-representation-of-the-prediction"></a><span data-ttu-id="4ffaf-220">Az előrejelzés vizuális ábrázolását létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ffaf-220">Create a visual representation of the prediction</span></span>
<span data-ttu-id="4ffaf-221">A Microsoft hogyan most hozhat létre és segítsen végső képi megjelenítés OK Ez a vizsgálat eredményei.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-221">We can now construct a final visualization to help us reason about the results of this test.</span></span>

1. <span data-ttu-id="4ffaf-222">Először a különböző előrejelzéseket és az eredmények kivonja a a **előrejelzéseket** korábban létrehozott ideiglenes tábla.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-222">We start by extracting the different predictions and results from the **Predictions** temporary table created earlier.</span></span> <span data-ttu-id="4ffaf-223">A következő lekérdezések külön a kimeneti adatok *true_positive*, *false_positive*, *true_negative*, és *false_negative*.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-223">The following queries separate the output as *true_positive*, *false_positive*, *true_negative*, and *false_negative*.</span></span> <span data-ttu-id="4ffaf-224">Az alábbi lekérdezésekben azt kapcsolja ki a képi megjelenítés használatával `-q` , és a kimeneti (használatával `-o`), majd használható a dataframes a `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-224">In the queries below, we turn off visualization by using `-q` and also save the output (by using `-o`) as dataframes that can be then used with the `%%local` magic.</span></span>

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. <span data-ttu-id="4ffaf-225">Végezetül a következő kódrészletet használja a rajzolási történő létrehozásához **Matplotlib**.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-225">Finally, use the following snippet to generate the plot using **Matplotlib**.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="4ffaf-226">A következő kimenetet kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-226">You should see the following output:</span></span>

    <span data-ttu-id="4ffaf-227">![Spark gépi tanulási alkalmazás kimeneti - Kördiagram százalékos sikertelen étele ellenőrzések. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark gépi tanulási a eredmény kimenete")</span><span class="sxs-lookup"><span data-stu-id="4ffaf-227">![Spark machine learning application output - pie chart percentages of failed food inspections.](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning result output")</span></span>

    <span data-ttu-id="4ffaf-228">Ezen a diagramon "pozitív" hivatkozik sikertelen étele ellenőrző negatív eredmény hivatkozik a átadott ellenőrzése közben.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-228">In this chart, a "positive" result refers to the failed food inspection, while a negative result refers to a passed inspection.</span></span>

## <a name="shut-down-the-notebook"></a><span data-ttu-id="4ffaf-229">Állítsa le a notebook</span><span class="sxs-lookup"><span data-stu-id="4ffaf-229">Shut down the notebook</span></span>
<span data-ttu-id="4ffaf-230">Miután befejezte az alkalmazás fut, állítsa le a notebook az erőforrások kijelölése.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-230">After you have finished running the application, you should shut down the notebook to release the resources.</span></span> <span data-ttu-id="4ffaf-231">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-231">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="4ffaf-232">Ezzel leállítja és bezárja a notebookot.</span><span class="sxs-lookup"><span data-stu-id="4ffaf-232">This shuts down and closes the notebook.</span></span>

## <span data-ttu-id="4ffaf-233"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4ffaf-233"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="4ffaf-234">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="4ffaf-234">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="4ffaf-235">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="4ffaf-235">Scenarios</span></span>
* [<span data-ttu-id="4ffaf-236">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="4ffaf-236">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="4ffaf-237">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="4ffaf-237">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="4ffaf-238">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="4ffaf-238">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="4ffaf-239">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="4ffaf-239">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="4ffaf-240">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="4ffaf-240">Create and run applications</span></span>
* [<span data-ttu-id="4ffaf-241">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="4ffaf-241">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="4ffaf-242">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="4ffaf-242">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="4ffaf-243">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="4ffaf-243">Tools and extensions</span></span>
* [<span data-ttu-id="4ffaf-244">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="4ffaf-244">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="4ffaf-245">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="4ffaf-245">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="4ffaf-246">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="4ffaf-246">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="4ffaf-247">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="4ffaf-247">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="4ffaf-248">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="4ffaf-248">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="4ffaf-249">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="4ffaf-249">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="4ffaf-250">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="4ffaf-250">Manage resources</span></span>
* [<span data-ttu-id="4ffaf-251">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4ffaf-251">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="4ffaf-252">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4ffaf-252">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
