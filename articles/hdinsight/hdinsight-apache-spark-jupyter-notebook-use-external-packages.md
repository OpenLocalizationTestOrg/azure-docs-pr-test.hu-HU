---
title: "a Spark on Azure HDInsight Jupyter aaaUse egyéni Maven csomagok |} Microsoft Docs"
description: "Hogyan tooconfigure Jupyter notebookok, amely a HDInsight Spark-fürtök toouse egyéni Maven-csomagok részletes ismertetése."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="5cb78-103">Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight</span><span class="sxs-lookup"><span data-stu-id="5cb78-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5cb78-104">Cella magic használatával</span><span class="sxs-lookup"><span data-stu-id="5cb78-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="5cb78-105">Parancsfájl művelet segítségével</span><span class="sxs-lookup"><span data-stu-id="5cb78-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="5cb78-106">Ismerje meg, hogyan tooconfigure Jupyter notebook az Apache Spark-fürt a HDInsight toouse külső, közösségi-hozzájárult **maven** csomagok, amelyek nem szerepelnek hello fürt out-of-az-box.</span><span class="sxs-lookup"><span data-stu-id="5cb78-106">Learn how tooconfigure a Jupyter notebook in Apache Spark cluster on HDInsight toouse external, community-contributed **maven** packages that are not included out-of-the-box in hello cluster.</span></span> 

<span data-ttu-id="5cb78-107">Hello kereshet [Maven-tárház](http://search.maven.org/) hello csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="5cb78-107">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="5cb78-108">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="5cb78-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="5cb78-109">Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="5cb78-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="5cb78-110">Ebből a cikkből megtudhatja, hogyan toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) hello Jupyter notebook számára.</span><span class="sxs-lookup"><span data-stu-id="5cb78-110">In this article, you will learn how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="5cb78-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5cb78-111">Prerequisites</span></span>
<span data-ttu-id="5cb78-112">Hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="5cb78-112">You must have hello following:</span></span>

* <span data-ttu-id="5cb78-113">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="5cb78-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="5cb78-114">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="5cb78-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="5cb78-115">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="5cb78-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="5cb78-116">A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="5cb78-116">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="5cb78-117">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="5cb78-117">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="5cb78-118">Hello Spark-fürt panelén kattintson **Gyorshivatkozások**, és majd a hello **fürt irányítópult** panelen kattintson a **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="5cb78-118">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="5cb78-119">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="5cb78-119">If prompted, enter hello admin credentials for hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5cb78-120">A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="5cb78-120">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="5cb78-121">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="5cb78-121">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="5cb78-122">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="5cb78-122">Create a new notebook.</span></span> <span data-ttu-id="5cb78-123">Kattintson a **új**, és kattintson a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="5cb78-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="5cb78-124">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="5cb78-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="5cb78-125">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="5cb78-125">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="5cb78-126">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="5cb78-126">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="5cb78-127">![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "adjon meg egy nevet hello notebook")</span><span class="sxs-lookup"><span data-stu-id="5cb78-127">![Provide a name for hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="5cb78-128">Hello használandó `%%configure` magic tooconfigure hello notebook toouse egy külső csomagot.</span><span class="sxs-lookup"><span data-stu-id="5cb78-128">You will use hello `%%configure` magic tooconfigure hello notebook toouse an external package.</span></span> <span data-ttu-id="5cb78-129">Külső csomagok használata jegyzetfüzetek, győződjön meg arról, meghívja a hello `%%configure` magic az első kódcella hello.</span><span class="sxs-lookup"><span data-stu-id="5cb78-129">In notebooks that use external packages, make sure you call hello `%%configure` magic in hello first code cell.</span></span> <span data-ttu-id="5cb78-130">Ez biztosítja, hogy hello kernel konfigurált toouse hello csomag hello munkamenet indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="5cb78-130">This ensures that hello kernel is configured toouse hello package before hello session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="5cb78-131">Ha elfelejti tooconfigure hello kernel hello első cellába, használhatja a hello `%%configure` a hello `-f` paraméter, de hello munkamenet újraindul, és az összes folyamatban lévő elvesznek.</span><span class="sxs-lookup"><span data-stu-id="5cb78-131">If you forget tooconfigure hello kernel in hello first cell, you can use hello `%%configure` with hello `-f` parameter, but that will restart hello session and all progress will be lost.</span></span>

    | <span data-ttu-id="5cb78-132">HDInsight-verzió</span><span class="sxs-lookup"><span data-stu-id="5cb78-132">HDInsight version</span></span> | <span data-ttu-id="5cb78-133">Parancs</span><span class="sxs-lookup"><span data-stu-id="5cb78-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="5cb78-134">A HDInsight 3.3-as és a HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="5cb78-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="5cb78-135">A HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="5cb78-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="5cb78-136">a fenti hello részlet hello maven koordináták hello külső package Maven központi tárházban vár.</span><span class="sxs-lookup"><span data-stu-id="5cb78-136">hello snippet above expects hello maven coordinates for hello external package in Maven Central Repository.</span></span> <span data-ttu-id="5cb78-137">Ezt a kódrészletet a `com.databricks:spark-csv_2.10:1.4.0` hello maven koordináta a van **spark-fürt megosztott kötetei szolgáltatás** csomag.</span><span class="sxs-lookup"><span data-stu-id="5cb78-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is hello maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="5cb78-138">Itt látható, hogyan hozható létre csomag hello koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="5cb78-138">Here's how you construct hello coordinates for a package.</span></span>
   
    <span data-ttu-id="5cb78-139">a.</span><span class="sxs-lookup"><span data-stu-id="5cb78-139">a.</span></span> <span data-ttu-id="5cb78-140">Keresse meg hello csomag hello Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="5cb78-140">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="5cb78-141">Ebben az oktatóanyagban használjuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="5cb78-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="5cb78-142">b.</span><span class="sxs-lookup"><span data-stu-id="5cb78-142">b.</span></span> <span data-ttu-id="5cb78-143">Adattárból hello gyűjtse össze a hello értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="5cb78-143">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="5cb78-144">Győződjön meg arról, hogy gyűjtse össze a hello értékek egyeznek-e a fürtön.</span><span class="sxs-lookup"><span data-stu-id="5cb78-144">Make sure that hello values you gather match your cluster.</span></span> <span data-ttu-id="5cb78-145">Ebben az esetben a Scala 2.10 és Spark 1.4.0 csomag használjuk, de szükséges tooselect különböző verziói hello megfelelő Scala vagy Spark-verziót a fürtben.</span><span class="sxs-lookup"><span data-stu-id="5cb78-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need tooselect different versions for hello appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="5cb78-146">Talál hello Scala verzió a fürtön futó `scala.util.Properties.versionString` hello Spark Jupyter kernel vagy Spark elküldése.</span><span class="sxs-lookup"><span data-stu-id="5cb78-146">You can find out hello Scala version on your cluster by running `scala.util.Properties.versionString` on hello Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="5cb78-147">Talál hello Spark verzió a fürtön futó `sc.version` használt Jupyter notebookokban.</span><span class="sxs-lookup"><span data-stu-id="5cb78-147">You can find out hello Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="5cb78-148">![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="5cb78-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="5cb78-149">c.</span><span class="sxs-lookup"><span data-stu-id="5cb78-149">c.</span></span> <span data-ttu-id="5cb78-150">Összefűzésére hello három értékek, egymástól kettősponttal (**:**).</span><span class="sxs-lookup"><span data-stu-id="5cb78-150">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="5cb78-151">Futtassa a hello kódcella hello `%%configure` magic.</span><span class="sxs-lookup"><span data-stu-id="5cb78-151">Run hello code cell with hello `%%configure` magic.</span></span> <span data-ttu-id="5cb78-152">Ezzel konfigurálja a hello alapul szolgáló Livy munkamenet toouse hello csomag megadott.</span><span class="sxs-lookup"><span data-stu-id="5cb78-152">This will configure hello underlying Livy session toouse hello package you provided.</span></span> <span data-ttu-id="5cb78-153">A hello későbbi levő hello jegyzetfüzet, most már használhatja hello csomag alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="5cb78-153">In hello subsequent cells in hello notebook, you can now use hello package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="5cb78-154">Ezt követően futtathatja hello kódtöredékek, például a lent látható módon tooview hello hello dataframe adatait hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="5cb78-154">You can then run hello snippets, like shown below, tooview hello data from hello dataframe you created in hello previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="5cb78-155"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5cb78-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="5cb78-156">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="5cb78-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="5cb78-157">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="5cb78-157">Scenarios</span></span>
* [<span data-ttu-id="5cb78-158">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="5cb78-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="5cb78-159">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="5cb78-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="5cb78-160">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="5cb78-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="5cb78-161">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="5cb78-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="5cb78-162">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="5cb78-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="5cb78-163">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="5cb78-163">Create and run applications</span></span>
* [<span data-ttu-id="5cb78-164">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="5cb78-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="5cb78-165">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="5cb78-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="5cb78-166">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="5cb78-166">Tools and extensions</span></span>

* [<span data-ttu-id="5cb78-167">Külső python-csomagok használata Jupyter notebookok Apache Spark-fürtök HDInsight Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="5cb78-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="5cb78-168">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="5cb78-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="5cb78-169">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="5cb78-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="5cb78-170">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="5cb78-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="5cb78-171">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="5cb78-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="5cb78-172">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="5cb78-172">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="5cb78-173">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="5cb78-173">Manage resources</span></span>
* [<span data-ttu-id="5cb78-174">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="5cb78-174">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="5cb78-175">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="5cb78-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

