---
title: "Adatok elemzése az Azure Data Lake Store az Apache Spark használatával |} Microsoft Docs"
description: "Azure Data Lake Store-ban tárolt adatok elemzése a Spark-feladatok futtatása"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 66f115c4f348ccaeb8855568ba1ad50faa442173
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a><span data-ttu-id="d8b5c-103">A Data Lake Store adatok elemzése a HDInsight Spark-fürt használatával</span><span class="sxs-lookup"><span data-stu-id="d8b5c-103">Use HDInsight Spark cluster to analyze data in Data Lake Store</span></span>

<span data-ttu-id="d8b5c-104">Ebben az oktatóanyagban Jupyter notebook érhető el a HDInsight Spark-fürtjei futtatásához használt egy feladatot, amely egy Data Lake Store-fiók olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters to run a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8b5c-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d8b5c-105">Prerequisites</span></span>

* <span data-ttu-id="d8b5c-106">Azure Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="d8b5c-107">Kövesse [Az Azure Data Lake Store használatának első lépései az Azure Portal használatával](../data-lake-store/data-lake-store-get-started-portal.md) című témakör utasításait.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-107">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="d8b5c-108">Az Azure HDInsight Spark-fürt a Data Lake Store tárolóként</span><span class="sxs-lookup"><span data-stu-id="d8b5c-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="d8b5c-109">Kövesse az utasításokat, [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d8b5c-109">Follow the instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-the-data"></a><span data-ttu-id="d8b5c-110">Adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="d8b5c-110">Prepare the data</span></span>

> [!NOTE]
> <span data-ttu-id="d8b5c-111">Nem kell végrehajtania ezt a lépést, ha a Data Lake Store alapértelmezett tárolóként hozott létre a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-111">You do not need to perform this step if you have created the HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="d8b5c-112">A fürt csoportlétrehozási folyamatait néhány adatot hozzáadja a fürt létrehozásakor megadott Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-112">The cluster creation processes adds some sample data in the Data Lake Store account that you specify while creating the cluster.</span></span> <span data-ttu-id="d8b5c-113">Váltson át a szakasz [használata a HDInsight Spark-fürt a Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="d8b5c-113">Skip to the section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="d8b5c-114">HDInsight-fürtök Data Lake Store további tárhely és az Azure Storage-Blobba alapértelmezett tárolóként hozta létre, ha először másolja át néhány adatot a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data to the Data Lake Store account.</span></span> <span data-ttu-id="d8b5c-115">A minta a HDInsight-fürthöz kapcsolódó adatokat az Azure Storage-Blobból is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-115">You can use the sample data from the Azure Storage Blob associated with the HDInsight cluster.</span></span> <span data-ttu-id="d8b5c-116">Használhatja a [ADLCopy eszköz](http://aka.ms/downloadadlcopy) ehhez.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-116">You can use the [ADLCopy tool](http://aka.ms/downloadadlcopy) to do so.</span></span> <span data-ttu-id="d8b5c-117">Töltse le és telepítse az eszközt a hivatkozásból.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-117">Download and install the tool from the link.</span></span>

1. <span data-ttu-id="d8b5c-118">Nyisson meg egy parancssort, és keresse meg azt a könyvtárat, AdlCopy futtató, általában `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-118">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="d8b5c-119">A következő parancsot egy adott blob átmásolja a forrás-tároló egy Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-119">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="d8b5c-120">Másolás a **HVAC.csv** minta adatfájl **/HdiSamples/HdiSamples/SensorSampleData/hvac/** az Azure Data Lake Store-fiókba.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-120">Copy the **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** to the Azure Data Lake Store account.</span></span> <span data-ttu-id="d8b5c-121">A kódrészletet hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-121">The code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="d8b5c-122">Ellenőrizze, hogy a megfelelő esetben van a fájl elérési útja és neve.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-122">Make sure you the file and path names are in the proper case.</span></span>
   >
   >
3. <span data-ttu-id="d8b5c-123">A rendszer bekéri a hitelesítő adatok megadása az Azure-előfizetés alapján, amely rendelkezik a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-123">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="d8b5c-124">Egy a következőhöz hasonló kimenetet fog látni:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-124">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="d8b5c-125">Az adatfájl (**HVAC.csv**) egy mappában kerülnek **/hvac** a Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-125">The data file (**HVAC.csv**) will be copied under a folder **/hvac** in the Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="d8b5c-126">A Data Lake Store HDInsight Spark-fürt használatára</span><span class="sxs-lookup"><span data-stu-id="d8b5c-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="d8b5c-127">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="d8b5c-127">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="d8b5c-128">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="d8b5c-128">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="d8b5c-129">A Spark-fürt panelén kattintson a **Quick Links** (Gyorshivatkozások) lehetőségre, majd a **Cluster Dashboard** (Fürt irányítópultja) panelen a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-129">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="d8b5c-130">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-130">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d8b5c-131">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-131">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="d8b5c-132">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-132">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="d8b5c-133">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-133">Create a new notebook.</span></span> <span data-ttu-id="d8b5c-134">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="d8b5c-135">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d8b5c-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="d8b5c-136">Mivel a notebook PySpark kernel használatával jött létre, explicit módon semmilyen tartalmat nem kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-136">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="d8b5c-137">Az első kódcella futtatásakor a Spark- és Hive-környezetek automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-137">The Spark and Hive contexts will be automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="d8b5c-138">Ennek az első lépése a jelen forgatókönyvhöz szükséges típusok importálása.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-138">You can start by importing the types required for this scenario.</span></span> <span data-ttu-id="d8b5c-139">Ehhez illessze be a következő kódrészletet a cellába, majd nyomja le a **SHIFT + ENTER** billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-139">To do so, paste the following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="d8b5c-140">Minden alkalommal, amikor a Jupyterben feladatot futtat, a webböngésző ablakának címsorában **(Foglalt)** állapot jelenik meg a notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="d8b5c-141">A jobb felső sarokban lévő **PySpark** felirat mellett ekkor egy teli kör is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-141">You will also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="d8b5c-142">A feladat befejezését követően ez a jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-142">After the job is completed, this will change to a hollow circle.</span></span>

     <span data-ttu-id="d8b5c-143">![A Jupyter notebook feladat állapota](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "A Jupyter notebook feladat állapota")</span><span class="sxs-lookup"><span data-stu-id="d8b5c-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="d8b5c-144">Mintaadatok betöltése az egy ideiglenes táblát használ a **HVAC.csv** fájl másolása a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-144">Load sample data into a temporary table using the **HVAC.csv** file you copied to the Data Lake Store account.</span></span> <span data-ttu-id="d8b5c-145">Az adatok a Data Lake Store-fiókot a következő URL-cím minta használatával érheti el.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-145">You can access the data in the Data Lake Store account using the following URL pattern.</span></span>

    * <span data-ttu-id="d8b5c-146">Ha a Data Lake Store alapértelmezett tárolóként, HVAC.csv az elérési útját, a következő URL-cím lesz:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-146">If you have Data Lake Store as default storage, HVAC.csv will be at the path similar to the following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="d8b5c-147">Vagy is használhatja a rövidített formátumban, például a következőket:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-147">Or, you could also use a shortened format such as the following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="d8b5c-148">Ha Data Lake Store további tárolóként, HVAC.csv lesz a helyen, ahol másolta, például:</span><span class="sxs-lookup"><span data-stu-id="d8b5c-148">If you have Data Lake Store as additional storage, HVAC.csv will be at the location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="d8b5c-149">A cella üres, illessze be a következő kódrészlet példa, hogy lecseréli **MYDATALAKESTORE** a Data Lake Store-fiók nevét, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-149">In an empty cell, paste the following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="d8b5c-150">Ez a kódpélda az adatokat a **hvac** nevű ideiglenes táblába regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-150">This code example registers the data into a temporary table called **hvac**.</span></span>

            # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="d8b5c-151">Mivel PySpark kernelt használ, most közvetlenül futtathat SQL-lekérdezést az imént létrehozott **hvac** ideiglenes táblán, a `%%sql` funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-151">Because you are using a PySpark kernel, you can now directly run a SQL query on the temporary table **hvac** that you just created by using the `%%sql` magic.</span></span> <span data-ttu-id="d8b5c-152">A `%%sql` funkcióval, illetve a PySpark kernellel elérhető egyéb funkciókkal kapcsolatos további információkat [A Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic) című részben talál.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-152">For more information about the `%%sql` magic, as well as other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="d8b5c-153">A feladat sikeres végrehajtását követően alapértelmezés szerint az alábbi táblázatos kimenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-153">Once the job is completed successfully, the following tabular output is displayed by default.</span></span>

      <span data-ttu-id="d8b5c-154">![A lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "A lekérdezési eredmény táblázati kimenete")</span><span class="sxs-lookup"><span data-stu-id="d8b5c-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="d8b5c-155">Az eredményeket egyéb megjelenítési formákban is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-155">You can also see the results in other visualizations as well.</span></span> <span data-ttu-id="d8b5c-156">Az azonos kimenethez tartozó területgrafikon például az alábbihoz hasonlóan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-156">For example, an area graph for the same output would look like the following.</span></span>

     <span data-ttu-id="d8b5c-157">![A lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "A lekérdezési eredmény területgrafikonja")</span><span class="sxs-lookup"><span data-stu-id="d8b5c-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="d8b5c-158">Az alkalmazás futtatását követően állítsa le a notebookot az erőforrások felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-158">After you have finished running the application, you should shutdown the notebook to release the resources.</span></span> <span data-ttu-id="d8b5c-159">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-159">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="d8b5c-160">Ezzel leállítja és bezárja a notebookot.</span><span class="sxs-lookup"><span data-stu-id="d8b5c-160">This will shutdown and close the notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d8b5c-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8b5c-161">Next steps</span></span>

* [<span data-ttu-id="d8b5c-162">Önálló Scala alkalmazás futtatását a Apache Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8b5c-162">Create a standalone Scala application to run on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d8b5c-163">Azure eszköztára IntelliJ HDInsight Tools használatával Spark-alkalmazások HDInsight Spark Linux-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8b5c-163">Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="d8b5c-164">Az Eclipse Azure eszközkészlet a HDInsight Tools használatával Spark-alkalmazások HDInsight Spark Linux-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8b5c-164">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
