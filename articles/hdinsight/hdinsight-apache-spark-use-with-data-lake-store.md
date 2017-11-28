---
title: az Apache Spark on tooanalyze adatokat az Azure Data Lake Store aaaUse |} Microsoft Docs
description: "A Spark-feladatok futtatása az Azure Data Lake Store-ban tárolt tooanalyze adatok"
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
ms.openlocfilehash: 3b7f628f7a8114d2ca6f3f9219ce107905f1c818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a><span data-ttu-id="d0bfb-103">HDInsight Spark-fürt tooanalyze adatok használata a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="d0bfb-103">Use HDInsight Spark cluster tooanalyze data in Data Lake Store</span></span>

<span data-ttu-id="d0bfb-104">Ebben az oktatóanyagban használata Jupyter notebook elérhető HDInsight Spark-fürtök toorun egy feladatot, amely egy Data Lake Store-fiók olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters toorun a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0bfb-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d0bfb-105">Prerequisites</span></span>

* <span data-ttu-id="d0bfb-106">Azure Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="d0bfb-107">Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d0bfb-107">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="d0bfb-108">Az Azure HDInsight Spark-fürt a Data Lake Store tárolóként</span><span class="sxs-lookup"><span data-stu-id="d0bfb-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="d0bfb-109">Hajtsa végre a hello található utasítások segítségével: [HDInsight-fürtök létrehozása az Azure portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d0bfb-109">Follow hello instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-hello-data"></a><span data-ttu-id="d0bfb-110">Hello adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="d0bfb-110">Prepare hello data</span></span>

> [!NOTE]
> <span data-ttu-id="d0bfb-111">Nem kell tooperform ebben a lépésben Ha hello HDInsight-fürt hozott létre a Data Lake Store alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-111">You do not need tooperform this step if you have created hello HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="d0bfb-112">hello fürt csoportlétrehozási folyamatait néhány mintaadatokkal hello fürt létrehozásakor megadott hello Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-112">hello cluster creation processes adds some sample data in hello Data Lake Store account that you specify while creating hello cluster.</span></span> <span data-ttu-id="d0bfb-113">Hagyja ki a szakaszt toohello [használata a HDInsight Spark-fürt a Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="d0bfb-113">Skip toohello section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="d0bfb-114">HDInsight-fürtök Data Lake Store további tárhely és az Azure Storage-Blobba alapértelmezett tárolóként hozta létre, ha először másolja át néhány minta adatok toohello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data toohello Data Lake Store account.</span></span> <span data-ttu-id="d0bfb-115">Hello HDInsight-fürthöz társított Azure Storage-Blobba hello hello minta adatait is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-115">You can use hello sample data from hello Azure Storage Blob associated with hello HDInsight cluster.</span></span> <span data-ttu-id="d0bfb-116">Használhatja a hello [ADLCopy eszköz](http://aka.ms/downloadadlcopy) toodo így.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-116">You can use hello [ADLCopy tool](http://aka.ms/downloadadlcopy) toodo so.</span></span> <span data-ttu-id="d0bfb-117">Töltse le, és telepítse hello eszköz hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-117">Download and install hello tool from hello link.</span></span>

1. <span data-ttu-id="d0bfb-118">Nyisson meg egy parancssort, és keresse meg a toohello rendszert tartalmazó könyvtár AdlCopy, általában `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-118">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="d0bfb-119">Futtassa a következő parancs toocopy hello egy adott blob hello forrás tároló tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-119">Run hello following command toocopy a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="d0bfb-120">Másolás hello **HVAC.csv** minta adatfájl **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-120">Copy hello **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store account.</span></span> <span data-ttu-id="d0bfb-121">hello kódrészletet hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-121">hello code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="d0bfb-122">Győződjön meg arról, hogy hello fájl, és útvonalneveket hello megfelelő esetben.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-122">Make sure you hello file and path names are in hello proper case.</span></span>
   >
   >
3. <span data-ttu-id="d0bfb-123">Fogja felszólító tooenter hello hitelesítő adatai hello, amely alatt a Data Lake Store-fiók rendelkezik Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-123">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="d0bfb-124">Egy kimeneti hasonló toohello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-124">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="d0bfb-125">hello adatfájl (**HVAC.csv**) egy mappában kerülnek **/hvac** a hello Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-125">hello data file (**HVAC.csv**) will be copied under a folder **/hvac** in hello Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="d0bfb-126">A Data Lake Store HDInsight Spark-fürt használatára</span><span class="sxs-lookup"><span data-stu-id="d0bfb-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="d0bfb-127">A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="d0bfb-127">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="d0bfb-128">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-128">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="d0bfb-129">Hello Spark-fürt panelén kattintson **Gyorshivatkozások**, és majd a hello **fürt irányítópult** panelen kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-129">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="d0bfb-130">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-130">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d0bfb-131">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-131">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="d0bfb-132">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-132">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="d0bfb-133">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-133">Create a new notebook.</span></span> <span data-ttu-id="d0bfb-134">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="d0bfb-135">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d0bfb-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="d0bfb-136">Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="d0bfb-137">hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="d0bfb-138">Ehhez a forgatókönyvhöz szükséges hello típusok importálásával megkezdése.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-138">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="d0bfb-139">toodo Igen, illessze be a következő kódrészletet a cellába, majd nyomja le az hello **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-139">toodo so, paste hello following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="d0bfb-140">Minden alkalommal, amikor a Jupyter futtat egy feladatot, a webböngésző ablakának címsorában megjelenik egy **(foglalt)** állapot hello notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="d0bfb-141">Ezenkívül megjelenik egy teli kör következő toohello **PySpark** hello jobb felső sarokban lévő szöveg.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-141">You will also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="d0bfb-142">Hello feladat befejezése után ez tooa jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-142">After hello job is completed, this will change tooa hollow circle.</span></span>

     <span data-ttu-id="d0bfb-143">![A Jupyter notebook feladat állapota](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "A Jupyter notebook feladat állapota")</span><span class="sxs-lookup"><span data-stu-id="d0bfb-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="d0bfb-144">Mintaadatokat tölthet be egy ideiglenes táblába hello segítségével **HVAC.csv** toohello Data Lake Store-fiók másolt fájl.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-144">Load sample data into a temporary table using hello **HVAC.csv** file you copied toohello Data Lake Store account.</span></span> <span data-ttu-id="d0bfb-145">Hello adatok hello Data Lake Store-fiók URL-minta a következő hello segítségével érheti el.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-145">You can access hello data in hello Data Lake Store account using hello following URL pattern.</span></span>

    * <span data-ttu-id="d0bfb-146">Ha a Data Lake Store alapértelmezett tárolóként, HVAC.csv hello elérési hasonló toohello URL-cím a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-146">If you have Data Lake Store as default storage, HVAC.csv will be at hello path similar toohello following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="d0bfb-147">Vagy egy rövidített formátumban, például hello következő is használhatja:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-147">Or, you could also use a shortened format such as hello following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="d0bfb-148">Ha Data Lake Store további tárolóként, HVAC.csv lesz hello helyét, ahová másolta, például:</span><span class="sxs-lookup"><span data-stu-id="d0bfb-148">If you have Data Lake Store as additional storage, HVAC.csv will be at hello location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="d0bfb-149">Cserélje le egy üres cellába, az alábbi kódpéldát, Beillesztés hello **MYDATALAKESTORE** a Data Lake Store-fiók nevét, és nyomja le az **SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-149">In an empty cell, paste hello following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="d0bfb-150">Ez a Kódpélda nevű ideiglenes táblába regisztrálja a hello adatok **hvac**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-150">This code example registers hello data into a temporary table called **hvac**.</span></span>

            # Load hello data. hello path below assumes Data Lake Store is default storage for hello Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create hello schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse hello data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register hello data fram as a table toorun queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="d0bfb-151">Mivel PySpark kernelt használ, akkor most közvetlenül futtathat SQL-lekérdezést hello ideiglenes táblán **hvac** imént létrehozott hello segítségével `%%sql` magic.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-151">Because you are using a PySpark kernel, you can now directly run a SQL query on hello temporary table **hvac** that you just created by using hello `%%sql` magic.</span></span> <span data-ttu-id="d0bfb-152">További információ a hello `%%sql` magic, valamint hello PySpark kernellel elérhető egyéb magics lásd: [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="d0bfb-152">For more information about hello `%%sql` magic, as well as other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="d0bfb-153">Ha hello feladat sikeresen befejeződött, a következő táblázatos kimenet hello alapértelmezés szerint megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-153">Once hello job is completed successfully, hello following tabular output is displayed by default.</span></span>

      <span data-ttu-id="d0bfb-154">![A lekérdezési eredmény táblázati kimenete](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "A lekérdezési eredmény táblázati kimenete")</span><span class="sxs-lookup"><span data-stu-id="d0bfb-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="d0bfb-155">Hello eredményeket egyéb megjelenítési formákban is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="d0bfb-156">Például tartozó területgrafikon hello azonos kimenethez hello következő jelenne meg.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-156">For example, an area graph for hello same output would look like hello following.</span></span>

     <span data-ttu-id="d0bfb-157">![A lekérdezési eredmény területgrafikonja](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "A lekérdezési eredmény területgrafikonja")</span><span class="sxs-lookup"><span data-stu-id="d0bfb-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="d0bfb-158">Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-158">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="d0bfb-159">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="d0bfb-160">Ezzel leállítja és Bezárás hello notebookot.</span><span class="sxs-lookup"><span data-stu-id="d0bfb-160">This will shutdown and close hello notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d0bfb-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0bfb-161">Next steps</span></span>

* [<span data-ttu-id="d0bfb-162">Hozzon létre egy önálló Scala alkalmazás toorun az Apache Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="d0bfb-162">Create a standalone Scala application toorun on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d0bfb-163">Az IntelliJ toocreate Spark-alkalmazások HDInsight Spark Linux-fürt eszköztára Azure HDInsight eszközök használata</span><span class="sxs-lookup"><span data-stu-id="d0bfb-163">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="d0bfb-164">HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások HDInsight Spark Linux-fürt Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="d0bfb-164">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
