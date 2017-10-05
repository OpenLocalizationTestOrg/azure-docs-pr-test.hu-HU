---
title: "Egyéni Maven-csomagok használata Jupyter a Spark on Azure HDInsight |} Microsoft Docs"
description: "Lépésenkénti útmutató HDInsight Spark-fürtjei Jupyter notebookok elérhető konfigurálása egyéni Maven-csomagok használata."
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
ms.openlocfilehash: 0bcfe220e60e34937c667c7b416065d5f3dc8d63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="2b558-103">Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b558-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2b558-104">Cella magic használatával</span><span class="sxs-lookup"><span data-stu-id="2b558-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="2b558-105">Parancsfájl művelet segítségével</span><span class="sxs-lookup"><span data-stu-id="2b558-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="2b558-106">Jupyter notebook beállításáról az Apache Spark-fürt a HDInsight használatához a külső, közösségi hozzájárult **maven** csomagok, amelyek nem tartalmazza a fürt, a-kész.</span><span class="sxs-lookup"><span data-stu-id="2b558-106">Learn how to configure a Jupyter notebook in Apache Spark cluster on HDInsight to use external, community-contributed **maven** packages that are not included out-of-the-box in the cluster.</span></span> 

<span data-ttu-id="2b558-107">Kereshet az [Maven-tárház](http://search.maven.org/) csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="2b558-107">You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available.</span></span> <span data-ttu-id="2b558-108">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="2b558-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="2b558-109">Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="2b558-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="2b558-110">Ebből a cikkből megtanulhatja, hogyan használható a [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) a Jupyter notebookkal csomag.</span><span class="sxs-lookup"><span data-stu-id="2b558-110">In this article, you will learn how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="2b558-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b558-111">Prerequisites</span></span>
<span data-ttu-id="2b558-112">Az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="2b558-112">You must have the following:</span></span>

* <span data-ttu-id="2b558-113">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="2b558-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="2b558-114">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="2b558-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="2b558-115">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="2b558-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="2b558-116">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="2b558-116">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="2b558-117">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="2b558-117">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="2b558-118">A Spark-fürt panelén kattintson a **Quick Links** (Gyorshivatkozások) lehetőségre, majd a **Cluster Dashboard** (Fürt irányítópultja) panelen a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="2b558-118">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="2b558-119">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2b558-119">If prompted, enter the admin credentials for the cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2b558-120">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="2b558-120">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="2b558-121">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="2b558-121">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="2b558-122">Hozzon létre új notebookot.</span><span class="sxs-lookup"><span data-stu-id="2b558-122">Create a new notebook.</span></span> <span data-ttu-id="2b558-123">Kattintson a **új**, és kattintson a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="2b558-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="2b558-124">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="2b558-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="2b558-125">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="2b558-125">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="2b558-126">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="2b558-126">Click the notebook name at the top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="2b558-127">![Adjon nevet a notebooknak](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Adjon nevet a notebooknak")</span><span class="sxs-lookup"><span data-stu-id="2b558-127">![Provide a name for the notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="2b558-128">Használhatja a `%%configure` magic a notebook egy külső csomag konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2b558-128">You will use the `%%configure` magic to configure the notebook to use an external package.</span></span> <span data-ttu-id="2b558-129">Külső csomagok használata jegyzetfüzetek, győződjön meg arról, meghívja a `%%configure` magic az első kódcella a.</span><span class="sxs-lookup"><span data-stu-id="2b558-129">In notebooks that use external packages, make sure you call the `%%configure` magic in the first code cell.</span></span> <span data-ttu-id="2b558-130">Ez biztosítja, hogy a rendszermag a munkamenet akkor kezdődik, a csomag használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2b558-130">This ensures that the kernel is configured to use the package before the session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="2b558-131">Ha elfelejti a kernel konfigurálása az első cellába, használhatja a `%%configure` rendelkező a `-f` paraméter, de a munkamenet újraindul, és az összes folyamatban lévő elvesznek.</span><span class="sxs-lookup"><span data-stu-id="2b558-131">If you forget to configure the kernel in the first cell, you can use the `%%configure` with the `-f` parameter, but that will restart the session and all progress will be lost.</span></span>

    | <span data-ttu-id="2b558-132">HDInsight-verzió</span><span class="sxs-lookup"><span data-stu-id="2b558-132">HDInsight version</span></span> | <span data-ttu-id="2b558-133">Parancs</span><span class="sxs-lookup"><span data-stu-id="2b558-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="2b558-134">A HDInsight 3.3-as és a HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="2b558-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="2b558-135">A HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="2b558-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="2b558-136">A fenti kódrészletet a maven koordinátáit a külső package Maven központi tárházban vár.</span><span class="sxs-lookup"><span data-stu-id="2b558-136">The snippet above expects the maven coordinates for the external package in Maven Central Repository.</span></span> <span data-ttu-id="2b558-137">Ezt a kódrészletet a `com.databricks:spark-csv_2.10:1.4.0` van a maven koordináta **spark-fürt megosztott kötetei szolgáltatás** csomag.</span><span class="sxs-lookup"><span data-stu-id="2b558-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is the maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="2b558-138">Itt látható, hogyan hozható létre csomag a koordináták százalékosan.</span><span class="sxs-lookup"><span data-stu-id="2b558-138">Here's how you construct the coordinates for a package.</span></span>
   
    <span data-ttu-id="2b558-139">a.</span><span class="sxs-lookup"><span data-stu-id="2b558-139">a.</span></span> <span data-ttu-id="2b558-140">Keresse meg a csomagot a Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="2b558-140">Locate the package in the Maven Repository.</span></span> <span data-ttu-id="2b558-141">Ebben az oktatóanyagban használjuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="2b558-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="2b558-142">b.</span><span class="sxs-lookup"><span data-stu-id="2b558-142">b.</span></span> <span data-ttu-id="2b558-143">A tárház gyűjtse össze a értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="2b558-143">From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="2b558-144">Győződjön meg arról, hogy gyűjtse össze az értékek megegyeznek-e a fürt.</span><span class="sxs-lookup"><span data-stu-id="2b558-144">Make sure that the values you gather match your cluster.</span></span> <span data-ttu-id="2b558-145">Ebben az esetben a Scala 2.10 és Spark 1.4.0 csomag használjuk, de szükség lehet a fürt különböző verzióinak a megfelelő Scala vagy Spark verzió kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="2b558-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need to select different versions for the appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="2b558-146">Talál a Scala verzió a fürtön futó `scala.util.Properties.versionString` a Spark Jupyter kernel vagy Spark elküldése.</span><span class="sxs-lookup"><span data-stu-id="2b558-146">You can find out the Scala version on your cluster by running `scala.util.Properties.versionString` on the Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="2b558-147">Talál a Spark-verzió a fürtön futó `sc.version` használt Jupyter notebookokban.</span><span class="sxs-lookup"><span data-stu-id="2b558-147">You can find out the Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="2b558-148">![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")</span><span class="sxs-lookup"><span data-stu-id="2b558-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="2b558-149">c.</span><span class="sxs-lookup"><span data-stu-id="2b558-149">c.</span></span> <span data-ttu-id="2b558-150">A következő három érték, egymástól kettősponttal összefűzésére (**:**).</span><span class="sxs-lookup"><span data-stu-id="2b558-150">Concatenate the three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="2b558-151">Futtassa a kód cellába, amelynek a `%%configure` magic.</span><span class="sxs-lookup"><span data-stu-id="2b558-151">Run the code cell with the `%%configure` magic.</span></span> <span data-ttu-id="2b558-152">Ezzel az alapul szolgáló Livy munkamenet a megadott csomag konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2b558-152">This will configure the underlying Livy session to use the package you provided.</span></span> <span data-ttu-id="2b558-153">A notebook későbbi celláiban most már használhatja a csomag alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="2b558-153">In the subsequent cells in the notebook, you can now use the package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="2b558-154">Ezt követően futtathatja részletek, például alább látható módon, az előző lépésben létrehozott dataframe származó adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b558-154">You can then run the snippets, like shown below, to view the data from the dataframe you created in the previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="2b558-155"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2b558-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="2b558-156">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="2b558-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="2b558-157">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2b558-157">Scenarios</span></span>
* [<span data-ttu-id="2b558-158">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="2b558-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="2b558-159">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="2b558-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="2b558-160">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="2b558-160">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="2b558-161">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="2b558-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="2b558-162">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="2b558-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="2b558-163">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="2b558-163">Create and run applications</span></span>
* [<span data-ttu-id="2b558-164">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="2b558-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="2b558-165">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="2b558-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="2b558-166">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="2b558-166">Tools and extensions</span></span>

* [<span data-ttu-id="2b558-167">Külső python-csomagok használata Jupyter notebookok Apache Spark-fürtök HDInsight Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="2b558-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="2b558-168">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="2b558-168">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="2b558-169">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="2b558-169">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="2b558-170">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="2b558-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="2b558-171">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="2b558-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="2b558-172">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="2b558-172">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="2b558-173">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="2b558-173">Manage resources</span></span>
* [<span data-ttu-id="2b558-174">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="2b558-174">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="2b558-175">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="2b558-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

