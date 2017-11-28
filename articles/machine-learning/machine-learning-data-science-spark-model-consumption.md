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
# <a name="operationalize-spark-built-machine-learning-models"></a><span data-ttu-id="13ec5-103">Azok a Spark-beépített machine learning modellek</span><span class="sxs-lookup"><span data-stu-id="13ec5-103">Operationalize Spark-built machine learning models</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="13ec5-104">Ez a témakör bemutatja, hogyan toooperationalize a mentett gépi tanulási modell (ML) Python használata a HDInsight Spark-fürtök.</span><span class="sxs-lookup"><span data-stu-id="13ec5-104">This topic shows how toooperationalize a saved machine learning model (ML) using Python on HDInsight Spark clusters.</span></span> <span data-ttu-id="13ec5-105">Leírja, hogyan tooload Spark MLlib használatával készített, és az Azure Blob Storage (WASB) tárolt tanulási modelljeit gépre, és hogyan tooscore WASB is megtalálható adatkészletekkel őket.</span><span class="sxs-lookup"><span data-stu-id="13ec5-105">It describes how tooload machine learning models that have been built using Spark MLlib and stored in Azure Blob Storage (WASB), and how tooscore them with datasets that have also been stored in WASB.</span></span> <span data-ttu-id="13ec5-106">Azt illusztrálja, hogyan toopre-folyamat hello bemeneti adatokat, átalakító funkciók hello indexelési és kódolási funkcióinak hello MLlib eszközkészlet, és hogyan toocreate egy címkézett pont objektumot, amelyek változtatás nélkül használhatók bemeneti pontozó hello ML modellekkel.</span><span class="sxs-lookup"><span data-stu-id="13ec5-106">It shows how toopre-process hello input data, transform features using hello indexing and encoding functions in hello MLlib toolkit, and how toocreate a labeled point data object that can be used as input for scoring with hello ML models.</span></span> <span data-ttu-id="13ec5-107">pontozó használt hello modellek lineáris regresszió, logisztikai regresszió, véletlenszerű erdő modellek és átmenetes kiemelése fa modellek tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="13ec5-107">hello models used for scoring include Linear Regression, Logistic Regression, Random Forest Models, and Gradient Boosting Tree Models.</span></span>

## <a name="spark-clusters-and-jupyter-notebooks"></a><span data-ttu-id="13ec5-108">A Spark-fürtök és a Jupyter notebookok</span><span class="sxs-lookup"><span data-stu-id="13ec5-108">Spark clusters and Jupyter notebooks</span></span>
<span data-ttu-id="13ec5-109">A telepítő lépéseket és hello kód toooperationalize az ML-modell-okat a forgatókönyv az 1.6-os Spark on HDInsight-fürt, valamint a Spark 2.0 fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="13ec5-109">Setup steps and hello code toooperationalize an ML model are provided in this walkthrough for using an HDInsight Spark 1.6 cluster as well as a Spark 2.0 cluster.</span></span> <span data-ttu-id="13ec5-110">Ezek az eljárások hello kódját Jupyter notebookok is megtalálható.</span><span class="sxs-lookup"><span data-stu-id="13ec5-110">hello code for these procedures is also provided in Jupyter notebooks.</span></span>

### <a name="notebook-for-spark-16"></a><span data-ttu-id="13ec5-111">A Spark 1.6-os notebook</span><span class="sxs-lookup"><span data-stu-id="13ec5-111">Notebook for Spark 1.6</span></span>
<span data-ttu-id="13ec5-112">Hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook bemutatja, hogyan toooperationalize egy mentett modell Python használata a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="13ec5-112">hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook shows how toooperationalize a saved model using Python on HDInsight clusters.</span></span> 

### <a name="notebook-for-spark-20"></a><span data-ttu-id="13ec5-113">A Spark 2.0 notebook</span><span class="sxs-lookup"><span data-stu-id="13ec5-113">Notebook for Spark 2.0</span></span>
<span data-ttu-id="13ec5-114">toomodify hello Jupyter notebook Spark 1.6 toouse 2.0 a HDInsight Spark-fürttel, cserélje le a hello Python kódját fájl [ezt a fájlt](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span><span class="sxs-lookup"><span data-stu-id="13ec5-114">toomodify hello Jupyter notebook for Spark 1.6 toouse with an HDInsight Spark 2.0 cluster, replace hello Python code file with [this file](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span></span> <span data-ttu-id="13ec5-115">A kód bemutatja, hogyan tooconsume hello modellek hozza létre a Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="13ec5-115">This code shows how tooconsume hello models created in Spark 2.0.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="13ec5-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13ec5-116">Prerequisites</span></span>

1. <span data-ttu-id="13ec5-117">Az Azure-fiók és a Spark 1.6-os (vagy külső 2.0) HDInsight-fürtöt toocomplete Ez a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="13ec5-117">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="13ec5-118">Lásd: hello [áttekintése adatok tudományos Spark on Azure HDInsight használatának](machine-learning-data-science-spark-overview.md) útmutatást toosatisfy ezeket a követelményeket.</span><span class="sxs-lookup"><span data-stu-id="13ec5-118">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="13ec5-119">Témakör hello itt használt NYC 2013 Taxi adatok, valamint útmutatást a hogyan tooexecute hello Spark-fürt a Jupyter notebook a kód leírását is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="13ec5-119">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 
2. <span data-ttu-id="13ec5-120">Akkor is létre kell hoznia hello machine learning modellek toobe program pontozza a mennyiségeket itt által feldolgozása révén hello [adatok feltárása és Spark modellezés](machine-learning-data-science-spark-data-exploration-modeling.md) hello 1.6-os Spark-fürt vagy hello Spark 2.0 notebookok témakör.</span><span class="sxs-lookup"><span data-stu-id="13ec5-120">You must also create hello machine learning models toobe scored here by working through hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic for hello Spark 1.6 cluster or hello Spark 2.0 notebooks.</span></span> 
3. <span data-ttu-id="13ec5-121">hello Spark 2.0 notebookok használata további adatok hello besorolás feladat hello jól ismert légitársaság időben indító dataset 2012 és a 2011.</span><span class="sxs-lookup"><span data-stu-id="13ec5-121">hello Spark 2.0 notebooks use an additional data set for hello classification task, hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="13ec5-122">Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="13ec5-122">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="13ec5-123">Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek.</span><span class="sxs-lookup"><span data-stu-id="13ec5-123">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="13ec5-124">Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható.</span><span class="sxs-lookup"><span data-stu-id="13ec5-124">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="13ec5-125">Beállítása: tárolóhelyek, könyvtárak és hello beállított Spark környezet</span><span class="sxs-lookup"><span data-stu-id="13ec5-125">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="13ec5-126">A Spark az Azure tárolási Blob (WASB) képes tooread és írási tooan.</span><span class="sxs-lookup"><span data-stu-id="13ec5-126">Spark is able tooread and write tooan Azure Storage Blob (WASB).</span></span> <span data-ttu-id="13ec5-127">Így a meglévő tárolt adatok dolgozhatók használata Spark, hello tárolt újra WASB eredménye.</span><span class="sxs-lookup"><span data-stu-id="13ec5-127">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="13ec5-128">toosave modellek vagy WASB fájlokat, a hello elérési utat kell toobe-e megadva.</span><span class="sxs-lookup"><span data-stu-id="13ec5-128">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="13ec5-129">hello alapértelmezett tároló csatolt toohello Spark-fürt lehet hivatkozni egy elérési utat kezdetű használatával: *"wasb / /"*.</span><span class="sxs-lookup"><span data-stu-id="13ec5-129">hello default container attached toohello Spark cluster can be referenced using a path beginning with: *"wasb//"*.</span></span> <span data-ttu-id="13ec5-130">hello alábbi kódminta helyét adja meg hello hello adatok toobe olvassa el és mentett hello hello modell tároló könyvtár toowhich hello modell kimeneti elérési útját.</span><span class="sxs-lookup"><span data-stu-id="13ec5-130">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved.</span></span> 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="13ec5-131">Tárolási helye a WASB set könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="13ec5-131">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="13ec5-132">Modellek lesznek mentve: "wasb: / / / felhasználó/remoteuser/NYCTaxi/modellek".</span><span class="sxs-lookup"><span data-stu-id="13ec5-132">Models are saved in: "wasb:///user/remoteuser/NYCTaxi/Models".</span></span> <span data-ttu-id="13ec5-133">Ha az elérési út nem megfelelően van beállítva, a modellek nem töltődnek pontozó.</span><span class="sxs-lookup"><span data-stu-id="13ec5-133">If this path is not set properly, models are not loaded for scoring.</span></span>

<span data-ttu-id="13ec5-134">hello pontozott eredmények lett mentve: "wasb: / / / felhasználó/remoteuser/NYCTaxi/ScoredResults".</span><span class="sxs-lookup"><span data-stu-id="13ec5-134">hello scored results have been saved in: "wasb:///user/remoteuser/NYCTaxi/ScoredResults".</span></span> <span data-ttu-id="13ec5-135">Hello elérési toofolder nem megfelelő, ha az eredmények nincsenek mentve a mappában.</span><span class="sxs-lookup"><span data-stu-id="13ec5-135">If hello path toofolder is incorrect, results are not saved in that folder.</span></span>   

> [!NOTE]
> <span data-ttu-id="13ec5-136">hello fájl meghajtóútvonal-helyeit másolható és illeszthetők be ezt a kódot a hello kimenet utolsó cella hello a hello hello helyőrzőt **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebookot.</span><span class="sxs-lookup"><span data-stu-id="13ec5-136">hello file path locations can be copied and pasted into hello placeholders in this code from hello output of hello last cell of hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.</span></span>   
> 
> 

<span data-ttu-id="13ec5-137">Íme hello kód tooset directory elérési utak:</span><span class="sxs-lookup"><span data-stu-id="13ec5-137">Here is hello code tooset directory paths:</span></span> 

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

<span data-ttu-id="13ec5-138">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-138">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-139">DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)</span><span class="sxs-lookup"><span data-stu-id="13ec5-139">datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="13ec5-140">Könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="13ec5-140">Import libraries</span></span>
<span data-ttu-id="13ec5-141">Spark környezet beállítása és a következő kód hello szükséges könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="13ec5-141">Set spark context and import necessary libraries with hello following code</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="13ec5-142">Spark és az PySpark magics az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="13ec5-142">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="13ec5-143">Jupyter notebookok kapnak hello PySpark kernelt beállításkészletet kontextusban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="13ec5-143">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="13ec5-144">Így nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon fejleszt hello alkalmazás használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="13ec5-144">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="13ec5-145">Elérhetők az Ön alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="13ec5-145">These are available for you by default.</span></span> <span data-ttu-id="13ec5-146">Ezek a környezetek a következők:</span><span class="sxs-lookup"><span data-stu-id="13ec5-146">These contexts are:</span></span>

* <span data-ttu-id="13ec5-147">sc - a Spark</span><span class="sxs-lookup"><span data-stu-id="13ec5-147">sc - for Spark</span></span> 
* <span data-ttu-id="13ec5-148">az sqlContext - struktúra</span><span class="sxs-lookup"><span data-stu-id="13ec5-148">sqlContext - for Hive</span></span>

<span data-ttu-id="13ec5-149">hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%.</span><span class="sxs-lookup"><span data-stu-id="13ec5-149">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="13ec5-150">Nincsenek két ilyen parancsot a következő kód mintákat használt.</span><span class="sxs-lookup"><span data-stu-id="13ec5-150">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="13ec5-151">**%% helyi** megadott helyileg hajtotta végre az egymás utáni sorok hello kódot.</span><span class="sxs-lookup"><span data-stu-id="13ec5-151">**%%local** Specified that hello code in subsequent lines is executed locally.</span></span> <span data-ttu-id="13ec5-152">Kód érvényes Python-kódot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="13ec5-152">Code must be valid Python code.</span></span>
* <span data-ttu-id="13ec5-153">**%% sql -o<variable name>**</span><span class="sxs-lookup"><span data-stu-id="13ec5-153">**%%sql -o <variable name>**</span></span> 
* <span data-ttu-id="13ec5-154">Végrehajtja a Hive-lekérdezések hello az sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="13ec5-154">Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="13ec5-155">Hello -o paramétert fogad el, ha a hello hello lekérdezés eredménye hello megőrződjenek %%, egy Pandas dataframe helyi Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="13ec5-155">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas dataframe.</span></span>

<span data-ttu-id="13ec5-156">További információ a Jupyter notebookokból és az előre megadott hello hello kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="13ec5-156">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a><span data-ttu-id="13ec5-157">Adatok és megtisztított adatok keret létrehozása</span><span class="sxs-lookup"><span data-stu-id="13ec5-157">Ingest data and create a cleaned data frame</span></span>
<span data-ttu-id="13ec5-158">Ez a szakasz feladatok szükséges tooingest hello adatok toobe program pontozza a mennyiségeket sorozata hello kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="13ec5-158">This section contains hello code for a series of tasks required tooingest hello data toobe scored.</span></span> <span data-ttu-id="13ec5-159">Hello adatok formázása illesztett 0,1 % minta hello taxi út és a jegy ára fájl (.tsv fájlként tárolja), olvasása, és létrehoz egy tiszta adatok keret.</span><span class="sxs-lookup"><span data-stu-id="13ec5-159">Read in a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file), format hello data, and then creates a clean data frame.</span></span>

<span data-ttu-id="13ec5-160">hello taxi út és a jegy ára fájlok alapján volt csatlakoztatva a megadott hello eljárás a: [Team adatok tudományos folyamat hello működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="13ec5-160">hello taxi trip and fare files were joined based on hello procedure provided in the: [hello Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) topic.</span></span>

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

<span data-ttu-id="13ec5-161">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-161">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-162">Idő tooexecute cella fent: 46.37 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-162">Time taken tooexecute above cell: 46.37 seconds</span></span>

## <a name="prepare-data-for-scoring-in-spark"></a><span data-ttu-id="13ec5-163">Készítse elő az adatokat a Spark pontozó</span><span class="sxs-lookup"><span data-stu-id="13ec5-163">Prepare data for scoring in Spark</span></span>
<span data-ttu-id="13ec5-164">Ez a szakasz bemutatja, hogyan tooindex, kódolására, és azokat használni a felügyelt MLlib tanulási algoritmusok besorolási és regressziós kategorikus szolgáltatások tooprepare méretezni.</span><span class="sxs-lookup"><span data-stu-id="13ec5-164">This section shows how tooindex, encode, and scale categorical features tooprepare them for use in MLlib supervised learning algorithms for classification and regression.</span></span>

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a><span data-ttu-id="13ec5-165">Átalakítás funkció: index és modellek pontozó a bemenő kategorikus funkciói kódolása</span><span class="sxs-lookup"><span data-stu-id="13ec5-165">Feature transformation: index and encode categorical features for input into models for scoring</span></span>
<span data-ttu-id="13ec5-166">Ez a szakasz bemutatja, hogyan tooindex kategorikus adatok segítségével egy `StringIndexer` és a funkcióinak kódolása `OneHotEncoder` hello modellek bemeneteként.</span><span class="sxs-lookup"><span data-stu-id="13ec5-166">This section shows how tooindex categorical data using a `StringIndexer` and encode features with `OneHotEncoder` input into hello models.</span></span>

<span data-ttu-id="13ec5-167">Hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) egy karakterlánc-oszlopnak címkék tooa oszlop címke indexek kódolja.</span><span class="sxs-lookup"><span data-stu-id="13ec5-167">hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) encodes a string column of labels tooa column of label indices.</span></span> <span data-ttu-id="13ec5-168">címke gyakoriságot szerint rendezett hello indexet.</span><span class="sxs-lookup"><span data-stu-id="13ec5-168">hello indices are ordered by label frequencies.</span></span> 

<span data-ttu-id="13ec5-169">Hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="13ec5-169">hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="13ec5-170">Ez a kódolás lehetővé teszi, hogy a várt folyamatos fontos funkciók, például logisztikai regresszió algoritmusokat alkalmazott toobe toocategorical szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="13ec5-170">This encoding allows algorithms that expect continuous valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

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

<span data-ttu-id="13ec5-171">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-171">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-172">Idő tooexecute cella fent: 5.37 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-172">Time taken tooexecute above cell: 5.37 seconds</span></span>

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a><span data-ttu-id="13ec5-173">A szolgáltatás-tömböket tartalmazó bemeneti be modellek RDD objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="13ec5-173">Create RDD objects with feature arrays for input into models</span></span>
<span data-ttu-id="13ec5-174">Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adatait, mivel az egy RDD objektumot, és egy közbeni kódolása, azok használt tootrain és tesztelési MLlib logisztikai regresszió és -modellek fa-alapú kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="13ec5-174">This section contains code that shows how tooindex categorical text data as an RDD object and one-hot encode it so it can be used tootrain and test MLlib logistic regression and tree-based models.</span></span> <span data-ttu-id="13ec5-175">hello indexelt adatai [rugalmas elosztott Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objektumok.</span><span class="sxs-lookup"><span data-stu-id="13ec5-175">hello indexed data is stored in [Resilient Distributed Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objects.</span></span> <span data-ttu-id="13ec5-176">Ezek a hello alapvető absztrakciós Spark.</span><span class="sxs-lookup"><span data-stu-id="13ec5-176">These are hello basic abstraction in Spark.</span></span> <span data-ttu-id="13ec5-177">RDD objektum egy, a Spark párhuzamosan is üzemeltetnek elemek nem módosítható, particionált gyűjteményét képviseli.</span><span class="sxs-lookup"><span data-stu-id="13ec5-177">An RDD object represents an immutable, partitioned collection of elements that can be operated on in parallel with Spark.</span></span>

<span data-ttu-id="13ec5-178">Azt is kódot tartalmaz, amely bemutatja, hogyan tooscale adatok hello `StandardScalar` MLlib által előírt lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD), a számos machine learning modellek betanítása népszerű algoritmust használja.</span><span class="sxs-lookup"><span data-stu-id="13ec5-178">It also contains code that shows how tooscale data with hello `StandardScalar` provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of machine learning models.</span></span> <span data-ttu-id="13ec5-179">Hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) használt tooscale hello szolgáltatások toounit variancia.</span><span class="sxs-lookup"><span data-stu-id="13ec5-179">hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) is used tooscale hello features toounit variance.</span></span> <span data-ttu-id="13ec5-180">Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy széles körben folyósított értékekkel funkciók vannak nem a megadott túlzott mérjük hello objektív függvényben.</span><span class="sxs-lookup"><span data-stu-id="13ec5-180">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> 

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

<span data-ttu-id="13ec5-181">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-181">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-182">Idő tooexecute cella fent: 11.72 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-182">Time taken tooexecute above cell: 11.72 seconds</span></span>

## <a name="score-with-hello-logistic-regression-model-and-save-output-tooblob"></a><span data-ttu-id="13ec5-183">A hello logisztikai regressziós modell pontozása, és mentse a kimeneti tooblob</span><span class="sxs-lookup"><span data-stu-id="13ec5-183">Score with hello Logistic Regression Model and save output tooblob</span></span>
<span data-ttu-id="13ec5-184">Ebben a szakaszban hello kód bemutatja, hogyan tooload egy logisztikai regressziós modellt az Azure-ban korábban mentett blob-tároló és taxi utazás toopredict tipp fizetnek-e használni, azt a szabványos besorolás metrikák pontozása, és mentse és hello eredmények tooblob megrajzolásához tárolás.</span><span class="sxs-lookup"><span data-stu-id="13ec5-184">hello code in this section shows how tooload a Logistic Regression Model that has been saved in Azure blob storage and use it toopredict whether or not a tip is paid on a taxi trip, score it with standard classification metrics, and then save and plot hello results tooblob storage.</span></span> <span data-ttu-id="13ec5-185">eredmények pontozni hello RDD objektumok vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="13ec5-185">hello scored results are stored in RDD objects.</span></span> 

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

<span data-ttu-id="13ec5-186">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-186">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-187">Idő tooexecute cella fent: 19.22 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-187">Time taken tooexecute above cell: 19.22 seconds</span></span>

## <a name="score-a-linear-regression-model"></a><span data-ttu-id="13ec5-188">Egy lineáris regressziós modell pontozása</span><span class="sxs-lookup"><span data-stu-id="13ec5-188">Score a Linear Regression Model</span></span>
<span data-ttu-id="13ec5-189">Használtuk [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain egy lineáris regressziós modellt Stochastic átmenetes módszeren (SGD) használatával optimalizálási toopredict hello időtartamig tipp fizetett.</span><span class="sxs-lookup"><span data-stu-id="13ec5-189">We used [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain a linear regression model using Stochastic Gradient Descent (SGD) for optimization toopredict hello amount of tip paid.</span></span> 

<span data-ttu-id="13ec5-190">Ebben a szakaszban hello kód bemutatja, hogyan tooload egy lineáris regressziós modellt az Azure blob storage pontozása méretezett változók használata, és mentse a hello eredményeit, hátsó toohello blob.</span><span class="sxs-lookup"><span data-stu-id="13ec5-190">hello code in this section shows how tooload a Linear Regression Model from Azure blob storage, score using scaled variables, and then save hello results back toohello blob.</span></span>

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


<span data-ttu-id="13ec5-191">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-191">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-192">Idő tooexecute cella fent: 16.63 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-192">Time taken tooexecute above cell: 16.63 seconds</span></span>

## <a name="score-classification-and-regression-random-forest-models"></a><span data-ttu-id="13ec5-193">Besorolás és regressziós véletlenszerű erdő modell pontozása</span><span class="sxs-lookup"><span data-stu-id="13ec5-193">Score classification and regression Random Forest Models</span></span>
<span data-ttu-id="13ec5-194">Ebben a szakaszban hello kód bemutatja, hogyan tooload hello mentett besorolási és regressziós mentése az Azure blob storage véletlenszerű erdő modellek pontozása szabványos osztályozó és regressziós intézkedések teljesítményét, és mentse a hello eredmények hátsó tooblob tároló.</span><span class="sxs-lookup"><span data-stu-id="13ec5-194">hello code in this section shows how tooload hello saved classification and regression Random Forest Models saved in Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span>

<span data-ttu-id="13ec5-195">[Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.</span><span class="sxs-lookup"><span data-stu-id="13ec5-195">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="13ec5-196">Sok döntési fák algoritmus tooreduce hello kockázatát overfitting össze azokat.</span><span class="sxs-lookup"><span data-stu-id="13ec5-196">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="13ec5-197">Véletlenszerű erdők kezelni tud a kategorikus szolgáltatások kiterjesztése toohello multiclass adatbesorolás beállításai, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="13ec5-197">Random forests can handle categorical features, extend toohello multiclass classification setting, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="13ec5-198">Véletlenszerű erdők hello legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.</span><span class="sxs-lookup"><span data-stu-id="13ec5-198">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>

<span data-ttu-id="13ec5-199">[Spark.mllib](http://spark.apache.org/mllib/) támogatja az erdők véletlenszerű multiclass és bináris besorolási regressziós folyamatos és kategorikus funkciókat használ.</span><span class="sxs-lookup"><span data-stu-id="13ec5-199">[spark.mllib](http://spark.apache.org/mllib/) supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.</span></span> 

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

<span data-ttu-id="13ec5-200">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-200">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-201">Idő tooexecute cella fent: 31.07 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-201">Time taken tooexecute above cell: 31.07 seconds</span></span>

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a><span data-ttu-id="13ec5-202">Besorolás és regressziós átmenetes kiemelése fa modell pontozása</span><span class="sxs-lookup"><span data-stu-id="13ec5-202">Score classification and regression Gradient Boosting Tree Models</span></span>
<span data-ttu-id="13ec5-203">Ebben a szakaszban hello kód mutatja be, hogyan tooload besorolási és regressziós átmenetes kiemelése fa modellek az Azure blob storage pontozása szabványos osztályozó és regressziós intézkedések teljesítményét, és mentse a hello eredmények hátsó tooblob tároló.</span><span class="sxs-lookup"><span data-stu-id="13ec5-203">hello code in this section shows how tooload classification and regression Gradient Boosting Tree Models from Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span> 

<span data-ttu-id="13ec5-204">**Spark.mllib** támogatja az GBTs bináris osztályozási regressziós folyamatos és kategorikus funkciókat használ.</span><span class="sxs-lookup"><span data-stu-id="13ec5-204">**spark.mllib** supports GBTs for binary classification and for regression, using both continuous and categorical features.</span></span> 

<span data-ttu-id="13ec5-205">[Átmenet kiemelése fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák.</span><span class="sxs-lookup"><span data-stu-id="13ec5-205">[Gradient Boosting Trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="13ec5-206">GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt.</span><span class="sxs-lookup"><span data-stu-id="13ec5-206">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="13ec5-207">GBTs kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="13ec5-207">GBTs can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="13ec5-208">Is is szerepel a multiclass-adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="13ec5-208">They can also be used in a multiclass-classification setting.</span></span>

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

<span data-ttu-id="13ec5-209">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-209">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-210">Idő tooexecute cella fent: 14.6 másodperc</span><span class="sxs-lookup"><span data-stu-id="13ec5-210">Time taken tooexecute above cell: 14.6 seconds</span></span>

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a><span data-ttu-id="13ec5-211">A memóriából objektumainak eltávolítása, és nyomtassa ki a pontozott Alapkönyvtár</span><span class="sxs-lookup"><span data-stu-id="13ec5-211">Clean up objects from memory and print scored file locations</span></span>
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


<span data-ttu-id="13ec5-212">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="13ec5-212">**OUTPUT:**</span></span>

<span data-ttu-id="13ec5-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span><span class="sxs-lookup"><span data-stu-id="13ec5-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span></span>

<span data-ttu-id="13ec5-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span><span class="sxs-lookup"><span data-stu-id="13ec5-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span></span>

<span data-ttu-id="13ec5-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span><span class="sxs-lookup"><span data-stu-id="13ec5-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span></span>

<span data-ttu-id="13ec5-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span><span class="sxs-lookup"><span data-stu-id="13ec5-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span></span>

<span data-ttu-id="13ec5-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span><span class="sxs-lookup"><span data-stu-id="13ec5-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span></span>

<span data-ttu-id="13ec5-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span><span class="sxs-lookup"><span data-stu-id="13ec5-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span></span>

## <a name="consume-spark-models-through-a-web-interface"></a><span data-ttu-id="13ec5-219">Spark modellek felhasználásához webes felületen keresztül</span><span class="sxs-lookup"><span data-stu-id="13ec5-219">Consume Spark Models through a web interface</span></span>
<span data-ttu-id="13ec5-220">Spark mechanizmust biztosít tooremotely submit kötegelt feladatok vagy interaktív lekérdezések egy REST keresztül kommunikáljanak a Livy összetevőt.</span><span class="sxs-lookup"><span data-stu-id="13ec5-220">Spark provides a mechanism tooremotely submit batch jobs or interactive queries through a REST interface with a component called Livy.</span></span> <span data-ttu-id="13ec5-221">A HDInsight Spark-fürt alapértelmezés szerint engedélyezve van a Livy.</span><span class="sxs-lookup"><span data-stu-id="13ec5-221">Livy is enabled by default on your HDInsight Spark cluster.</span></span> <span data-ttu-id="13ec5-222">Livy további információkért lásd: [távolról a Livy használatával nyújt Spark feladatok](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="13ec5-222">For more information on Livy, see: [Submit Spark jobs remotely using Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span></span> 

<span data-ttu-id="13ec5-223">Tooremotely, amelyeket a batch pontszámok feladat elküldése egy fájlt, amely egy Azure blob tárolja, és ezután ír hello eredmények tooanother blob Livy is használhatja.</span><span class="sxs-lookup"><span data-stu-id="13ec5-223">You can use Livy tooremotely submit a job that batch scores a file that is stored in an Azure blob and then writes hello results tooanother blob.</span></span> <span data-ttu-id="13ec5-224">toodo, a Python-parancsfájl hello feltöltése</span><span class="sxs-lookup"><span data-stu-id="13ec5-224">toodo this, you upload hello Python script from</span></span>  
<span data-ttu-id="13ec5-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob hello Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="13ec5-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob of hello Spark cluster.</span></span> <span data-ttu-id="13ec5-226">Egy eszköz, például használhatja **Microsoft Azure Tártallózó** vagy **AzCopy** toocopy hello parancsfájl toohello fürt blob.</span><span class="sxs-lookup"><span data-stu-id="13ec5-226">You can use a tool like **Microsoft Azure Storage Explorer** or **AzCopy** toocopy hello script toohello cluster blob.</span></span> <span data-ttu-id="13ec5-227">Abban az esetben, ha azt feltöltött hello parancsfájl túl***wasb:///example/python/ConsumeGBNYCReg.py***.</span><span class="sxs-lookup"><span data-stu-id="13ec5-227">In our case we uploaded hello script too***wasb:///example/python/ConsumeGBNYCReg.py***.</span></span>   

> [!NOTE]
> <span data-ttu-id="13ec5-228">meg kell található hello portal hello Spark-fürt társított hello tárfiók tárelérési kulcsokat hello.</span><span class="sxs-lookup"><span data-stu-id="13ec5-228">hello access keys that you need can be found on hello portal for hello storage account associated with hello Spark cluster.</span></span> 
> 
> 

<span data-ttu-id="13ec5-229">Toothis helye a feltöltést követően a parancsfájl hello Spark-fürt elosztott környezetben belül fut.</span><span class="sxs-lookup"><span data-stu-id="13ec5-229">Once uploaded toothis location, this script runs within hello Spark cluster in a distributed context.</span></span> <span data-ttu-id="13ec5-230">Hello modell betölt, és előrejelzéseket futó hello modellen alapuló bemeneti fájlok.</span><span class="sxs-lookup"><span data-stu-id="13ec5-230">It loads hello model and runs predictions on input files based on hello model.</span></span>  

<span data-ttu-id="13ec5-231">Ez a parancsfájl távolról meghívni egy egyszerű HTTPS/REST kérelem hajtsanak végre a Livy.</span><span class="sxs-lookup"><span data-stu-id="13ec5-231">You can invoke this script remotely by making a simple HTTPS/REST request on Livy.</span></span>  <span data-ttu-id="13ec5-232">Ez a curl parancs tooconstruct hello HTTP kérelem tooinvoke hello Python-parancsfájl távolról.</span><span class="sxs-lookup"><span data-stu-id="13ec5-232">Here is a curl command tooconstruct hello HTTP request tooinvoke hello Python script remotely.</span></span> <span data-ttu-id="13ec5-233">CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME cserélje le a Spark-fürt hello megfelelő értékeket.</span><span class="sxs-lookup"><span data-stu-id="13ec5-233">Replace CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME with hello appropriate values for your Spark cluster.</span></span>

    # CURL COMMAND tooINVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

<span data-ttu-id="13ec5-234">Egyetlen nyelven használhatja hello távoli tooinvoke hello Spark rendszerfeladat Livy keresztül a hívás egy egyszerű HTTPS az egyszerű hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="13ec5-234">You can use any language on hello remote system tooinvoke hello Spark job through Livy by making a simple HTTPS call with Basic Authentication.</span></span>   

> [!NOTE]
> <span data-ttu-id="13ec5-235">A HTTP híváshoz kényelmes toouse hello Python kérelmek könyvtár lenne, de jelenleg nincs telepítve az Azure Functions alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="13ec5-235">It would be convenient toouse hello Python Requests library when making this HTTP call, but it is not currently installed by default in Azure Functions.</span></span> <span data-ttu-id="13ec5-236">Így a régebbi HTTP szalagtárak használja helyette.</span><span class="sxs-lookup"><span data-stu-id="13ec5-236">So older HTTP libraries are used instead.</span></span>   
> 
> 

<span data-ttu-id="13ec5-237">Hello HTTP hívás hello Python kódját a következő:</span><span class="sxs-lookup"><span data-stu-id="13ec5-237">Here is hello Python code for hello HTTP call:</span></span>

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


<span data-ttu-id="13ec5-238">Azt is megteheti a Python-kód túl[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger egy Spark feladat elküldése, amely egy blob pontszámaihoz alapján különböző eseményekkel – például egy időzítő, létrehozás vagy frissítés BLOB.</span><span class="sxs-lookup"><span data-stu-id="13ec5-238">You can also add this Python code too[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger a Spark job submission that scores a blob based on various events like a timer, creation, or update of a blob.</span></span> 

<span data-ttu-id="13ec5-239">Kód szabad ügyfélélmény tetszés szerint használja a hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark kötegelt pontozási meghatározhat egy HTTP-műveletet a hello **Logic Apps Designer** és a paraméterek beállítása.</span><span class="sxs-lookup"><span data-stu-id="13ec5-239">If you prefer a code free client experience, use hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark batch scoring by defining an HTTP action on hello **Logic Apps Designer** and setting its parameters.</span></span> 

* <span data-ttu-id="13ec5-240">Azure-portálon hozzon létre egy új logikai alkalmazás kiválasztásával **+ új** -> **Web + mobil** -> **logikai alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="13ec5-240">From Azure portal, create a new Logic App by selecting **+New** -> **Web + Mobile** -> **Logic App**.</span></span> 
* <span data-ttu-id="13ec5-241">hello mentése toobring **Logic Apps Designer**, adja meg a Logic App hello hello nevét és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="13ec5-241">toobring up hello **Logic Apps Designer**, enter hello name of hello Logic App and App Service Plan.</span></span>
* <span data-ttu-id="13ec5-242">Válasszon ki egy HTTP-műveletet, és írja be a következő ábra hello látható hello paramétereket:</span><span class="sxs-lookup"><span data-stu-id="13ec5-242">Select an HTTP action and enter hello parameters shown in hello following figure:</span></span>

![Logic Apps-Tervező](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a><span data-ttu-id="13ec5-244">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="13ec5-244">What's next?</span></span>
<span data-ttu-id="13ec5-245">**Kereszt-ellenőrzési és hyperparameter abszolút**: lásd: [speciális adatok feltárása és Spark modellezés](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek betanítása a kereszt-ellenőrzési és a hyper-paraméter abszolút használatával.</span><span class="sxs-lookup"><span data-stu-id="13ec5-245">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>

