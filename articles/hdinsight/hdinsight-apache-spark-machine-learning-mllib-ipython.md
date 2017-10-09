---
title: "Példa a HDInsight - Azure Spark MLlib tartalmazó tanulási aaaMachine |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Spark MLlib toocreate egy machine learning-alkalmazást, amely elemzi a DataSet adatkészlet besorolásával keresztül logisztikai regresszió."
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
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a><span data-ttu-id="a53e9-104">Spark MLlib toobuild használata a machine learning-alkalmazás, és elemezze a DataSet adatkészlet</span><span class="sxs-lookup"><span data-stu-id="a53e9-104">Use Spark MLlib toobuild a machine learning application and analyze a dataset</span></span>

<span data-ttu-id="a53e9-105">Megtudhatja, hogyan toouse Spark **MLlib** toocreate a gépi tanulási alkalmazás toodo egyszerű prediktív elemzési egy megnyitott adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="a53e9-105">Learn how toouse Spark **MLlib** toocreate a machine learning application toodo simple predictive analysis on an open dataset.</span></span> <span data-ttu-id="a53e9-106">A Spark beépített gépi tanulási szalagtárak, a példában az *besorolás* logisztikai regresszió keresztül.</span><span class="sxs-lookup"><span data-stu-id="a53e9-106">From Spark's built-in machine learning libraries, this example uses *classification* through logistic regression.</span></span> 

> [!TIP]
> <span data-ttu-id="a53e9-107">Ebben a példában, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el.</span><span class="sxs-lookup"><span data-stu-id="a53e9-107">This example is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="a53e9-108">hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát.</span><span class="sxs-lookup"><span data-stu-id="a53e9-108">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="a53e9-109">toofollow hello oktatóprogram belül a notebook létrehozása egy Spark fürt és a Jupyter notebook indítási (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span><span class="sxs-lookup"><span data-stu-id="a53e9-109">toofollow hello tutorial from within a notebook, create a Spark cluster and launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span></span> <span data-ttu-id="a53e9-110">Futtassa a hello notebook **Spark Machine Learning - étele ellenőrző adatok MLlib.ipynb a prediktív elemzési** alatt hello **Python** mappa.</span><span class="sxs-lookup"><span data-stu-id="a53e9-110">Then, run hello notebook **Spark Machine Learning - Predictive analysis on food inspection data using MLlib.ipynb** under hello **Python** folder.</span></span>
>
>

<span data-ttu-id="a53e9-111">MLlib egy Spark Alapkönyvtár, amely számos hasznos segédprogramokat biztosít gépi tanulási feladatok, beleértve a segédprogramok:</span><span class="sxs-lookup"><span data-stu-id="a53e9-111">MLlib is a core Spark library that provides many utilities useful for machine learning tasks, including utilities that are suitable for:</span></span>

* <span data-ttu-id="a53e9-112">Besorolás</span><span class="sxs-lookup"><span data-stu-id="a53e9-112">Classification</span></span>
* <span data-ttu-id="a53e9-113">Regressziós</span><span class="sxs-lookup"><span data-stu-id="a53e9-113">Regression</span></span>
* <span data-ttu-id="a53e9-114">Fürtszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a53e9-114">Clustering</span></span>
* <span data-ttu-id="a53e9-115">A témakör modellezési</span><span class="sxs-lookup"><span data-stu-id="a53e9-115">Topic modeling</span></span>
* <span data-ttu-id="a53e9-116">Szinguláris érték felbontás ellen (SVD) és egyszerű összetevő elemzés (PEM)</span><span class="sxs-lookup"><span data-stu-id="a53e9-116">Singular value decomposition (SVD) and principal component analysis (PCA)</span></span>
* <span data-ttu-id="a53e9-117">Alapul, tesztelése és minta számításához</span><span class="sxs-lookup"><span data-stu-id="a53e9-117">Hypothesis testing and calculating sample statistics</span></span>

## <a name="what-are-classification-and-logistic-regression"></a><span data-ttu-id="a53e9-118">Mik azok a besorolás és logisztikai regresszió?</span><span class="sxs-lookup"><span data-stu-id="a53e9-118">What are classification and logistic regression?</span></span>
<span data-ttu-id="a53e9-119">*Besorolási*népszerű Machine learning-feladat, hello folyamat a bemeneti adatok rendezése kategóriákba.</span><span class="sxs-lookup"><span data-stu-id="a53e9-119">*Classification*, a popular machine learning task, is hello process of sorting input data into categories.</span></span> <span data-ttu-id="a53e9-120">A besorolási algoritmus toofigure tudhat meg a tooassign "címkék" tooinput adatok megadandó hello feladat.</span><span class="sxs-lookup"><span data-stu-id="a53e9-120">It is hello job of a classification algorithm toofigure out how tooassign "labels" tooinput data that you provide.</span></span> <span data-ttu-id="a53e9-121">Például egy gépi tanulási algoritmus, amely a bemeneti készlet adatokat fogadja a sikerült véleménye és felosztása hello készlet két kategóriába sorolhatók: kell értékesítő készletek és a készletek, amelyek kell tartania.</span><span class="sxs-lookup"><span data-stu-id="a53e9-121">For example, you could think of a machine learning algorithm that accepts stock information as input and divides hello stock into two categories: stocks that you should sell and stocks that you should keep.</span></span>

<span data-ttu-id="a53e9-122">Logisztikai regresszió hello algoritmus besorolási használó.</span><span class="sxs-lookup"><span data-stu-id="a53e9-122">Logistic regression is hello algorithm that you use for classification.</span></span> <span data-ttu-id="a53e9-123">A Spark logisztikai regresszió API akkor hasznos, ha *bináris osztályozási*, vagy egy két bemeneti adatok besorolása.</span><span class="sxs-lookup"><span data-stu-id="a53e9-123">Spark's logistic regression API is useful for *binary classification*, or classifying input data into one of two groups.</span></span> <span data-ttu-id="a53e9-124">További információ a logisztikai regresszió: [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span><span class="sxs-lookup"><span data-stu-id="a53e9-124">For more information about logistic regressions, see [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span></span>

<span data-ttu-id="a53e9-125">Az összegzés hello logisztikai regresszió illesztése egy *logisztikai függvény* , amely lehet, hogy egy bemeneti vektort tartozik egy csoport vagy más hello használt toopredict hello valószínűség.</span><span class="sxs-lookup"><span data-stu-id="a53e9-125">In summary, hello process of logistic regression produces a *logistic function* that can be used toopredict hello probability that an input vector belongs in one group or hello other.</span></span>  

## <a name="predictive-analysis-example-on-food-inspection-data"></a><span data-ttu-id="a53e9-126">A prediktív elemzés példa étele ellenőrző adatok</span><span class="sxs-lookup"><span data-stu-id="a53e9-126">Predictive analysis example on food inspection data</span></span>
<span data-ttu-id="a53e9-127">Ebben a példában használja Spark tooperform néhány prediktív elemzési étele ellenőrző adatokat (**Food_Inspections1.csv**), amely lett beszerzett hello [város a Chicagói adat portált](https://data.cityofchicago.org/).</span><span class="sxs-lookup"><span data-stu-id="a53e9-127">In this example, you use Spark tooperform some predictive analysis on food inspection data (**Food_Inspections1.csv**) that was acquired through hello [City of Chicago data portal](https://data.cityofchicago.org/).</span></span> <span data-ttu-id="a53e9-128">Ez az adatkészlet chicagóban, beleértve az egyes kialakítása végezték étele kialakítása ellenőrzésekre kapcsolatos információt tartalmazza, hello szabálytalanság található (ha van ilyen), és hello hello vizsgálat eredményeit.</span><span class="sxs-lookup"><span data-stu-id="a53e9-128">This dataset contains information about food establishment inspections that were conducted in Chicago, including information about each establishment, hello violations found (if any), and hello results of hello inspection.</span></span> <span data-ttu-id="a53e9-129">hello fürthöz társított hello tárfiókban már hello CSV adatfájl **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-129">hello CSV data file is already available in hello storage account associated with hello cluster at **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span></span>

<span data-ttu-id="a53e9-130">Hello az alábbi lépések fejlesztése a modell toosee mi toopass tart, vagy a étele ellenőrzése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="a53e9-130">In hello steps below, you develop a model toosee what it takes toopass or fail a food inspection.</span></span>

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a><span data-ttu-id="a53e9-131">Indítsa el a Spark MMLib machine learning-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a53e9-131">Start building a Spark MMLib machine learning app</span></span>
1. <span data-ttu-id="a53e9-132">A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="a53e9-132">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="a53e9-133">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-133">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
1. <span data-ttu-id="a53e9-134">Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-134">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="a53e9-135">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="a53e9-135">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a53e9-136">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="a53e9-136">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="a53e9-137">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="a53e9-137">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. <span data-ttu-id="a53e9-138">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="a53e9-138">Create a notebook.</span></span> <span data-ttu-id="a53e9-139">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="a53e9-139">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="a53e9-140">![A Jupyter notebook létrehozása](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "egy új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="a53e9-140">![Create a Jupyter notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "Create a new Jupyter notebook")</span></span>
1. <span data-ttu-id="a53e9-141">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="a53e9-141">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="a53e9-142">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="a53e9-142">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="a53e9-143">![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "adjon meg egy nevet hello notebook")</span><span class="sxs-lookup"><span data-stu-id="a53e9-143">![Provide a name for hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Provide a name for hello notebook")</span></span>
1. <span data-ttu-id="a53e9-144">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="a53e9-144">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="a53e9-145">hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="a53e9-145">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="a53e9-146">A machine learning alkalmazás ehhez a forgatókönyvhöz szükséges hello típusok importálásával felépítése elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="a53e9-146">You can start building your machine learning application by importing hello types required for this scenario.</span></span> <span data-ttu-id="a53e9-147">toodo tehát kurzorral hello hello cellába, majd nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-147">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a><span data-ttu-id="a53e9-148">Egy bemeneti dataframe szerkezet</span><span class="sxs-lookup"><span data-stu-id="a53e9-148">Construct an input dataframe</span></span>
<span data-ttu-id="a53e9-149">Használhatunk `sqlContext` tooperform átalakítások, a strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="a53e9-149">We can use `sqlContext` tooperform transformations on structured data.</span></span> <span data-ttu-id="a53e9-150">első feladata tooload hello mintaadatok hello ((**Food_Inspections1.csv**)) egy Spark SQL *dataframe*.</span><span class="sxs-lookup"><span data-stu-id="a53e9-150">hello first task is tooload hello sample data ((**Food_Inspections1.csv**)) into a Spark SQL *dataframe*.</span></span>

1. <span data-ttu-id="a53e9-151">Mivel hello nyers adatok CSV formátumban, igazolnia kell toouse hello Spark környezetben toopull hello fájl minden sora a memóriába strukturálatlan szövegként; Ezután használhatja Python a fürt megosztott kötetei szolgáltatás könyvtár tooparse soronként külön-külön.</span><span class="sxs-lookup"><span data-stu-id="a53e9-151">Because hello raw data is in a CSV format, we need toouse hello Spark context toopull every line of hello file into memory as unstructured text; then, you use Python's CSV library tooparse each line individually.</span></span>

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. <span data-ttu-id="a53e9-152">A Microsoft most egy RDD hello CSV-fájl lehet.</span><span class="sxs-lookup"><span data-stu-id="a53e9-152">We now have hello CSV file as an RDD.</span></span>  <span data-ttu-id="a53e9-153">toounderstand hello séma hello adatok, nem beolvasni egy sor hello RDD.</span><span class="sxs-lookup"><span data-stu-id="a53e9-153">toounderstand hello schema of hello data, we retrieve one row from hello RDD.</span></span>

        inspections.take(1)

    <span data-ttu-id="a53e9-154">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-154">You should see an output like hello following:</span></span>

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
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. <span data-ttu-id="a53e9-155">hello előző kimeneti biztosítanak számunkra hello séma hello bemeneti fájl képet.</span><span class="sxs-lookup"><span data-stu-id="a53e9-155">hello preceding output gives us an idea of hello schema of hello input file.</span></span> <span data-ttu-id="a53e9-156">Ez magában foglalja a minden kialakítása, kialakítása, hello címét, és hello ellenőrzések hello helyét, többek között a hello adatok hello típusú hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a53e9-156">It includes hello name of every establishment, hello type of establishment, hello address, hello data of hello inspections, and hello location, among other things.</span></span> <span data-ttu-id="a53e9-157">Most válassza ki, amelyek hasznosak a prediktív elemzési néhány oszlopot, és csoport hello eredménye egy dataframe, mely azt, majd használja toocreate ideiglenes táblából.</span><span class="sxs-lookup"><span data-stu-id="a53e9-157">Let's select a few columns that are useful for our predictive analysis and group hello results as a dataframe, which we then use toocreate a temporary table.</span></span>

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. <span data-ttu-id="a53e9-158">Most már rendelkezik egy *dataframe*, `df` amelyen most a elemzés elvégezni.</span><span class="sxs-lookup"><span data-stu-id="a53e9-158">We now have a *dataframe*, `df` on which we can perform our analysis.</span></span> <span data-ttu-id="a53e9-159">Egy ideiglenes tábla hívás is tudunk **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-159">We also have a temporary table call **CountResults**.</span></span> <span data-ttu-id="a53e9-160">A Microsoft hello dataframe jelentőséggel négy oszlop szerepel: **azonosító**, **neve**, **eredmények**, és **megsértésének**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-160">We've included four columns of interest in hello dataframe: **id**, **name**, **results**, and **violations**.</span></span>

    <span data-ttu-id="a53e9-161">Folytassuk hello adatok mintát:</span><span class="sxs-lookup"><span data-stu-id="a53e9-161">Let's get a small sample of hello data:</span></span>

        df.show(5)

    <span data-ttu-id="a53e9-162">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-162">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a><span data-ttu-id="a53e9-163">Hello adatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="a53e9-163">Understand hello data</span></span>
1. <span data-ttu-id="a53e9-164">Kezdjük tooget egyfajta az adatkészletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a53e9-164">Let's start tooget a sense of what our dataset contains.</span></span> <span data-ttu-id="a53e9-165">Például, melyek hello eltérő értéket hello **eredmények** oszlop?</span><span class="sxs-lookup"><span data-stu-id="a53e9-165">For example, what are hello different values in hello **results** column?</span></span>

        df.select('results').distinct().show()

    <span data-ttu-id="a53e9-166">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-166">You should see an output like hello following:</span></span>

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
1. <span data-ttu-id="a53e9-167">Gyors képi megjelenítés segíthet OK kapcsolatos ezek eredményekkel hello terjesztése.</span><span class="sxs-lookup"><span data-stu-id="a53e9-167">A quick visualization can help us reason about hello distribution of these outcomes.</span></span> <span data-ttu-id="a53e9-168">Egy ideiglenes tábla hello adatok már tudunk **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-168">We already have hello data in a temporary table **CountResults**.</span></span> <span data-ttu-id="a53e9-169">A következő SQL-lekérdezést hello tábla tooget hello szeretné jobban megismerni hello eredmények elosztása hogyan futtathat.</span><span class="sxs-lookup"><span data-stu-id="a53e9-169">You can run hello following SQL query against hello table tooget a better understanding of how hello results are distributed.</span></span>

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    <span data-ttu-id="a53e9-170">Hello `%%sql` magic követ `-o countResultsdf` biztosítja, hogy hello lekérdezés kimenetét hello hello (általában hello headnode hello fürt) Jupyter kiszolgálón helyileg megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="a53e9-170">hello `%%sql` magic followed by `-o countResultsdf` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="a53e9-171">hello kimeneti tárolva a [Pandas](http://pandas.pydata.org/) dataframe hello a megadott név **countResultsdf**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-171">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **countResultsdf**.</span></span>

    <span data-ttu-id="a53e9-172">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-172">You should see an output like hello following:</span></span>

    <span data-ttu-id="a53e9-173">![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL-lekérdezés kimenete")</span><span class="sxs-lookup"><span data-stu-id="a53e9-173">![SQL query output](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL query output")</span></span>

    <span data-ttu-id="a53e9-174">Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="a53e9-174">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
1. <span data-ttu-id="a53e9-175">Matplotlib is használható marad, a szalagtár tooconstruct képi megjelenítés adatainak, toocreate rajzot használja.</span><span class="sxs-lookup"><span data-stu-id="a53e9-175">You can also use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="a53e9-176">Mert a rajzolási hello hello helyileg kell létrehozni a megőrzött **countResultsdf** dataframe, hello kódrészletet a következővel kell kezdődnie hello `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="a53e9-176">Because hello plot must be created from hello locally persisted **countResultsdf** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="a53e9-177">Ez biztosítja, hogy hello kód hello Jupyter kiszolgálón helyileg futnak.</span><span class="sxs-lookup"><span data-stu-id="a53e9-177">This ensures that hello code is run locally on hello Jupyter server.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="a53e9-178">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-178">You should see an output like hello following:</span></span>

    <span data-ttu-id="a53e9-179">![Spark gépi tanulási a alkalmazás kimeneti - öt különböző vizsgálat eredményekkel kördiagram](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark gépi tanulási a eredmény kimenete")</span><span class="sxs-lookup"><span data-stu-id="a53e9-179">![Spark machine learning application output - pie chart with five distinct inspection results](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning result output")</span></span>
1. <span data-ttu-id="a53e9-180">Láthatja, hogy vannak-e 5 különböző eredményeket, amelyeken az ellenőrzés:</span><span class="sxs-lookup"><span data-stu-id="a53e9-180">You can see that there are 5 distinct results that an inspection can have:</span></span>

   * <span data-ttu-id="a53e9-181">Nem található üzleti</span><span class="sxs-lookup"><span data-stu-id="a53e9-181">Business not located</span></span>
   * <span data-ttu-id="a53e9-182">Sikertelen</span><span class="sxs-lookup"><span data-stu-id="a53e9-182">Fail</span></span>
   * <span data-ttu-id="a53e9-183">Fázis</span><span class="sxs-lookup"><span data-stu-id="a53e9-183">Pass</span></span>
   * <span data-ttu-id="a53e9-184">Feltételek keresztüli rendelkező PSS</span><span class="sxs-lookup"><span data-stu-id="a53e9-184">Pss w/ conditions</span></span>
   * <span data-ttu-id="a53e9-185">Üzleti kívül</span><span class="sxs-lookup"><span data-stu-id="a53e9-185">Out of Business</span></span>

     <span data-ttu-id="a53e9-186">Ossza meg velünk fejlesztése modell, amely kitalálni a hello étele hálózatfelügyeleti, az adott hello megsértésének eredménye.</span><span class="sxs-lookup"><span data-stu-id="a53e9-186">Let us develop a model that can guess hello outcome of a food inspection, given hello violations.</span></span> <span data-ttu-id="a53e9-187">Mivel logisztikai regresszió egy bináris osztályozási módszer, így logika toogroup adataink két kategóriába sorolhatók: **sikertelen** és **átadni**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-187">Since logistic regression is a binary classification method, it makes sense toogroup our data into two categories: **Fail** and **Pass**.</span></span> <span data-ttu-id="a53e9-188">Egy "átadni keresztüli rendelkező feltételek" még nem állt le a hozzáférési, így amikor hello modell betanításához azt adatraktárakban hello két eredmények egyenértékű.</span><span class="sxs-lookup"><span data-stu-id="a53e9-188">A "Pass w/ Conditions" is still a Pass, so when we train hello model, we consider hello two results equivalent.</span></span> <span data-ttu-id="a53e9-189">Adatok a hello más eredmények ("Üzleti nem található" vagy "Out üzleti") esetén nem lehet hasznos, hogy távolítsa el azokat a gyakorlókészlethez.</span><span class="sxs-lookup"><span data-stu-id="a53e9-189">Data with hello other results ("Business Not Located" or "Out of Business") are not useful so we remove them from our training set.</span></span> <span data-ttu-id="a53e9-190">Ennek ellenére kell, óta ezek két kategóriába mégis alkotó hello eredmények nagyon kis százalékát.</span><span class="sxs-lookup"><span data-stu-id="a53e9-190">This should be okay since these two categories make up a very small percentage of hello results anyway.</span></span>
1. <span data-ttu-id="a53e9-191">Ossza meg velünk lépjen tovább, és alakítsa át a meglévő dataframe (`df`) egy új dataframe, ahol minden egyes ellenőrzés címke-megsértésének párban szerepel-e be.</span><span class="sxs-lookup"><span data-stu-id="a53e9-191">Let us go ahead and convert our existing dataframe(`df`) into a new dataframe where each inspection is represented as a label-violations pair.</span></span> <span data-ttu-id="a53e9-192">Ebben az esetben a címke `0.0` jelenti. a hiba, a címkék `1.0` sikeres, és a címke jelöli `-1.0` egyes eredményeket e két mellett jelöli.</span><span class="sxs-lookup"><span data-stu-id="a53e9-192">In our case, a label of `0.0` represents a failure, a label of `1.0` represents a success, and a label of `-1.0` represents some results besides those two.</span></span> <span data-ttu-id="a53e9-193">A Microsoft kiszűrhetők az egyéb eredmények hello új adatok keret számításakor.</span><span class="sxs-lookup"><span data-stu-id="a53e9-193">We filter those other results out when computing hello new data frame.</span></span>

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    <span data-ttu-id="a53e9-194">toosee milyen hello feliratú adatok tűnik, most beolvasni egy sort.</span><span class="sxs-lookup"><span data-stu-id="a53e9-194">toosee what hello labeled data looks like, let's retrieve one row.</span></span>

        labeledData.take(1)

    <span data-ttu-id="a53e9-195">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-195">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a><span data-ttu-id="a53e9-196">A bemeneti dataframe hello logisztikai regresszió modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a53e9-196">Create a logistic regression model from hello input dataframe</span></span>
<span data-ttu-id="a53e9-197">A végső feladat adatok címkével olyan formátumra, amely logisztikai regresszió által elemezhető tooconvert hello.</span><span class="sxs-lookup"><span data-stu-id="a53e9-197">Our final task is tooconvert hello labeled data into a format that can be analyzed by logistic regression.</span></span> <span data-ttu-id="a53e9-198">hello bemeneti tooa logisztikai regresszió algoritmus kell lenniük, amelyek *címke-funkció vektoros párok*, ahol a "szolgáltatás vektoros" hello hello bemeneti pontot jelölő számok vektor.</span><span class="sxs-lookup"><span data-stu-id="a53e9-198">hello input tooa logistic regression algorithm should be a set of *label-feature vector pairs*, where hello "feature vector" is a vector of numbers representing hello input point.</span></span> <span data-ttu-id="a53e9-199">Igen szükséges tooconvert hello "megsértésének" oszlop, amely félig strukturált és szabad szöveges, tooan tömb valós szám, amely a gép könnyen értelmezhető formára sok megjegyzéseket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a53e9-199">So, we need tooconvert hello "violations" column, which is semi-structured and contains many comments in free-text, tooan array of real numbers that a machine could easily understand.</span></span>

<span data-ttu-id="a53e9-200">Feldolgozási természetes nyelvű egy szabványos machine learning megközelítés tooassign minden különálló egy "index" szó, és akkor továbbítja a vektoros toohello a gépi tanulási algoritmus, úgy, hogy minden egyes indexértéket tartalmaz hello relatív gyakoriságot hello szöveges szó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a53e9-200">One standard machine learning approach for processing natural language is tooassign each distinct word an "index", and then pass a vector toohello machine learning algorithm such that each index's value contains hello relative frequency of that word in hello text string.</span></span>

<span data-ttu-id="a53e9-201">MLlib biztosít egy egyszerűen tooperform ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a53e9-201">MLlib provides an easy way tooperform this operation.</span></span> <span data-ttu-id="a53e9-202">Első lépésként "tokenize" minden megsértésének karakterlánc tooget hello a szöveg minden karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a53e9-202">First, "tokenize" each violations string tooget hello individual words in each string.</span></span> <span data-ttu-id="a53e9-203">Ezt követően egy `HashingTF` be a szolgáltatás vektor, amely majd átadott toohello logisztikai regresszió algoritmus tooconstruct jogkivonatok egyes készlete tooconvert egy modell.</span><span class="sxs-lookup"><span data-stu-id="a53e9-203">Then, use a `HashingTF` tooconvert each set of tokens into a feature vector that can then be passed toohello logistic regression algorithm tooconstruct a model.</span></span> <span data-ttu-id="a53e9-204">Azt végezze el az összes lépés sorrendben használja a "futószalag".</span><span class="sxs-lookup"><span data-stu-id="a53e9-204">We conduct all of these steps in sequence using a "pipeline".</span></span>

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a><span data-ttu-id="a53e9-205">Egy külön vizsgálat adatkészlethez hello modell kiértékelése</span><span class="sxs-lookup"><span data-stu-id="a53e9-205">Evaluate hello model on a separate test dataset</span></span>
<span data-ttu-id="a53e9-206">Korábban létrehozott hello modell használhatjuk túl*előrejelzése* milyen hello új ellenőrzések eredményeinek lesz, a megfigyelt hello szabálysértések alapján.</span><span class="sxs-lookup"><span data-stu-id="a53e9-206">We can use hello model we created earlier too*predict* what hello results of new inspections will be, based on hello violations that were observed.</span></span> <span data-ttu-id="a53e9-207">Jelenleg ez a modell hello adatkészlethez betanítása **Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-207">We trained this model on hello dataset **Food_Inspections1.csv**.</span></span> <span data-ttu-id="a53e9-208">Ossza meg velünk használjon egy második dataset **Food_Inspections2.csv**, túl*kiértékelése* ebben a modellben az új adatok erősségével hello.</span><span class="sxs-lookup"><span data-stu-id="a53e9-208">Let us use a second dataset, **Food_Inspections2.csv**, too*evaluate* hello strength of this model on new data.</span></span> <span data-ttu-id="a53e9-209">A második készlet (**Food_Inspections2.csv**) kell lennie a hello hello-fürthöz tartozó alapértelmezett tárolókat.</span><span class="sxs-lookup"><span data-stu-id="a53e9-209">This second data set (**Food_Inspections2.csv**) should already be in hello default storage container associated with hello cluster.</span></span>

1. <span data-ttu-id="a53e9-210">hello következő kódrészlettel létrehoz egy új dataframe **predictionsDf** tartalmazó hello előrejelzés hello modell által generált.</span><span class="sxs-lookup"><span data-stu-id="a53e9-210">hello following snippet creates a new dataframe, **predictionsDf** that contains hello prediction generated by hello model.</span></span> <span data-ttu-id="a53e9-211">hello részlet is létrehoz egy ideiglenes táblát **előrejelzéseket** hello dataframe alapján.</span><span class="sxs-lookup"><span data-stu-id="a53e9-211">hello snippet also creates a temporary table called **Predictions** based on hello dataframe.</span></span>

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    <span data-ttu-id="a53e9-212">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-212">You should see an output like hello following:</span></span>

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
1. <span data-ttu-id="a53e9-213">Tekintse meg hello előrejelzéseket egyikét.</span><span class="sxs-lookup"><span data-stu-id="a53e9-213">Look at one of hello predictions.</span></span> <span data-ttu-id="a53e9-214">Futtassa ezt a kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="a53e9-214">Run this snippet:</span></span>

        predictionsDf.take(1)

   <span data-ttu-id="a53e9-215">Első bejegyzés hello hello teszt adatkészlet előrejelzéshez van.</span><span class="sxs-lookup"><span data-stu-id="a53e9-215">There is a prediction for hello first entry in hello test data set.</span></span>
1. <span data-ttu-id="a53e9-216">Hello `model.transform()` módhoz hello azonos átalakítása tooany új adatok hello ugyanazon séma, és hogyan tooclassify hello adatok előrejelzése érkeznie.</span><span class="sxs-lookup"><span data-stu-id="a53e9-216">hello `model.transform()` method applies hello same transformation tooany new data with hello same schema, and arrive at a prediction of how tooclassify hello data.</span></span> <span data-ttu-id="a53e9-217">Tehetünk ennek néhány egyszerű statisztika tooget egyfajta hogyan pontos az előrejelzés volt:</span><span class="sxs-lookup"><span data-stu-id="a53e9-217">We can do some simple statistics tooget a sense of how accurate our predictions were:</span></span>

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    <span data-ttu-id="a53e9-218">hello kimeneti következőhöz hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="a53e9-218">hello output looks like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    <span data-ttu-id="a53e9-219">Logisztikai regresszió használata Spark lehetőséget ad olyan pontos modell hello kapcsolat megsértésének leírását angol nyelven, és hogy egy adott üzleti volna továbbítja, vagy a étele ellenőrzése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="a53e9-219">Using logistic regression with Spark gives us an accurate model of hello relationship between violations descriptions in English and whether a given business would pass or fail a food inspection.</span></span>

## <a name="create-a-visual-representation-of-hello-prediction"></a><span data-ttu-id="a53e9-220">Hello előrejelzés vizuális ábrázolását létrehozása</span><span class="sxs-lookup"><span data-stu-id="a53e9-220">Create a visual representation of hello prediction</span></span>
<span data-ttu-id="a53e9-221">Most azt állíthat össze egy, az ebben a tesztben hello eredmény okból végső képi megjelenítés toohelp.</span><span class="sxs-lookup"><span data-stu-id="a53e9-221">We can now construct a final visualization toohelp us reason about hello results of this test.</span></span>

1. <span data-ttu-id="a53e9-222">Először hello különböző előrejelzéseket beolvasásával és eredményeinek hello **előrejelzéseket** korábban létrehozott ideiglenes tábla.</span><span class="sxs-lookup"><span data-stu-id="a53e9-222">We start by extracting hello different predictions and results from hello **Predictions** temporary table created earlier.</span></span> <span data-ttu-id="a53e9-223">hello következő lekérdezések külön hello kimeneti adatok *true_positive*, *false_positive*, *true_negative*, és *false_negative*.</span><span class="sxs-lookup"><span data-stu-id="a53e9-223">hello following queries separate hello output as *true_positive*, *false_positive*, *true_negative*, and *false_negative*.</span></span> <span data-ttu-id="a53e9-224">Az alábbi hello lekérdezésekben, azt kapcsolja ki a képi megjelenítés használatával `-q` hello kimeneti is mentheti, és (használatával `-o`), majd használható a hello dataframes `%%local` magic.</span><span class="sxs-lookup"><span data-stu-id="a53e9-224">In hello queries below, we turn off visualization by using `-q` and also save hello output (by using `-o`) as dataframes that can be then used with hello `%%local` magic.</span></span>

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. <span data-ttu-id="a53e9-225">Végül, használja a következő kódrészletet toogenerate hello rajzot használatával hello **Matplotlib**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-225">Finally, use hello following snippet toogenerate hello plot using **Matplotlib**.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="a53e9-226">A következő kimeneti hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a53e9-226">You should see hello following output:</span></span>

    <span data-ttu-id="a53e9-227">![Spark gépi tanulási alkalmazás kimeneti - Kördiagram százalékos sikertelen étele ellenőrzések. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark gépi tanulási a eredmény kimenete")</span><span class="sxs-lookup"><span data-stu-id="a53e9-227">![Spark machine learning application output - pie chart percentages of failed food inspections.](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning result output")</span></span>

    <span data-ttu-id="a53e9-228">Ezen a diagramon "pozitív" nem sikerült toohello étele hálózatfelügyeleti, hivatkozik, amíg negatív eredmény hálózatfelügyeleti átadott tooa hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a53e9-228">In this chart, a "positive" result refers toohello failed food inspection, while a negative result refers tooa passed inspection.</span></span>

## <a name="shut-down-hello-notebook"></a><span data-ttu-id="a53e9-229">Hello notebook leállítása</span><span class="sxs-lookup"><span data-stu-id="a53e9-229">Shut down hello notebook</span></span>
<span data-ttu-id="a53e9-230">Miután befejezte a hello alkalmazást futtat, állítsa le a hello notebook toorelease hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a53e9-230">After you have finished running hello application, you should shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="a53e9-231">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="a53e9-231">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="a53e9-232">Ezzel leállítja és bezárja a notebookot hello.</span><span class="sxs-lookup"><span data-stu-id="a53e9-232">This shuts down and closes hello notebook.</span></span>

## <span data-ttu-id="a53e9-233"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a53e9-233"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="a53e9-234">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="a53e9-234">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="a53e9-235">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="a53e9-235">Scenarios</span></span>
* [<span data-ttu-id="a53e9-236">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="a53e9-236">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="a53e9-237">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="a53e9-237">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="a53e9-238">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="a53e9-238">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="a53e9-239">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="a53e9-239">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="a53e9-240">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="a53e9-240">Create and run applications</span></span>
* [<span data-ttu-id="a53e9-241">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="a53e9-241">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="a53e9-242">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="a53e9-242">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="a53e9-243">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="a53e9-243">Tools and extensions</span></span>
* [<span data-ttu-id="a53e9-244">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a53e9-244">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="a53e9-245">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="a53e9-245">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="a53e9-246">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="a53e9-246">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="a53e9-247">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="a53e9-247">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="a53e9-248">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="a53e9-248">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="a53e9-249">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="a53e9-249">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="a53e9-250">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="a53e9-250">Manage resources</span></span>
* [<span data-ttu-id="a53e9-251">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="a53e9-251">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="a53e9-252">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="a53e9-252">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
