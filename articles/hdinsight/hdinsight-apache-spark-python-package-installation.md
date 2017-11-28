---
title: "Művelet – telepítése Python-csomagokat, az Azure hdinsight Jupyter parancsfájl |} Microsoft Docs"
description: "Lépésenkénti útmutató parancsfájlművelet használatára konfigurálhatja a Jupyter notebookok érhető el a HDInsight Spark-fürtjei külső python-csomagok használata."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 20cf384c96d4ff4eaf064c8880ad128d521fb9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="dd0ad-103">Parancsfájlművelet használata Jupyter notebookok külső Python-csomagokat telepíteni az Apache Spark on hdinsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="dd0ad-103">Use Script Action to install external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="dd0ad-104">Cella magic használatával</span><span class="sxs-lookup"><span data-stu-id="dd0ad-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="dd0ad-105">Parancsfájl művelet segítségével</span><span class="sxs-lookup"><span data-stu-id="dd0ad-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="dd0ad-106">Útmutató Apache Spark-fürt konfigurálása a HDInsight (Linux) használatához a külső, közösségi része volt a Parancsfájlműveletek segítségével **python** csomagok, amelyek nem tartalmazza a fürt, a-kész.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-106">Learn how to use Script Actions to configure an Apache Spark cluster on HDInsight (Linux) to use external, community-contributed **python** packages that are not included out-of-the-box in the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="dd0ad-107">Jupyter notebook használatával is konfigurálhatja `%%configure` külső csomagok használata a Bűvös.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-107">You can also configure a Jupyter notebook by using `%%configure` magic to use external packages.</span></span> <span data-ttu-id="dd0ad-108">Útmutatásért lásd: [külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="dd0ad-109">Kereshet az [csomagindexet](https://pypi.python.org/pypi) csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-109">You can search the [package index](https://pypi.python.org/pypi) for the complete list of packages that are available.</span></span> <span data-ttu-id="dd0ad-110">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="dd0ad-111">Például telepíthet keresztül elérhető csomagok [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) vagy [conda-forge](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="dd0ad-112">Ebből a cikkből megtanulhatja, hogyan telepítheti a [TensorFlow](https://www.tensorflow.org/) csomag parancsfájl Actoin használatával a fürtön, és a Jupyter notebook keresztül használni.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-112">In this article, you will learn how to install the [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via the Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dd0ad-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dd0ad-113">Prerequisites</span></span>
<span data-ttu-id="dd0ad-114">Az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="dd0ad-114">You must have the following:</span></span>

* <span data-ttu-id="dd0ad-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-115">An Azure subscription.</span></span> <span data-ttu-id="dd0ad-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="dd0ad-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="dd0ad-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="dd0ad-119">Ha még nem rendelkezik egy Spark-fürt HDInsight Linux rendszeren, Parancsfájlműveletek futtathatja a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="dd0ad-120">Látogasson el a dokumentációt a [egyéni parancsfájl-műveletek használata](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-120">Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="dd0ad-121">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="dd0ad-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="dd0ad-122">Az [Azure portál](https://portal.azure.com/) kezdőpultján kattintson a Spark-fürthöz tartozó csempére (ha rögzítette azt a kezdőpulton).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-122">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="dd0ad-123">A fürtöt a következő helyről is megkeresheti: **Browse All (Összes tallózása)** > **HDInsight Clusters** (HDInsight-fürtök).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-123">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="dd0ad-124">A Spark-fürt panelén kattintson **Parancsfájlműveletek** alatt **használati**.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-124">From the Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="dd0ad-125">Az egyéni művelet, amely telepíti TensorFlow az átjárócsomópontokkal és a feldolgozó csomópontok.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-125">Run the custom action that installs TensorFlow in the head nodes and the worker nodes.</span></span> <span data-ttu-id="dd0ad-126">A bash parancsfájlok lehet hivatkozni: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh látogassa meg a dokumentáció a [egyéni parancsfájl-műveletek használata](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="dd0ad-126">The bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="dd0ad-127">Nincsenek két python telepítések a fürtben.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-127">There are two python installations in the cluster.</span></span> <span data-ttu-id="dd0ad-128">Spark fogja használni a Anaconda python telepítési helye: `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-128">Spark will use the Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="dd0ad-129">Hivatkozás a telepítést, az egyéni műveletek keresztül a `/usr/bin/anaconda/bin/pip` és `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="dd0ad-130">Nyissa meg a PySpark Jupyter notebook</span><span class="sxs-lookup"><span data-stu-id="dd0ad-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="dd0ad-131">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="dd0ad-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="dd0ad-132">Az új notebook létrejött, és Untitled.pynb néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-132">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="dd0ad-133">A felső részen kattintson a notebook nevére, és adjon meg egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-133">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="dd0ad-134">![Adjon nevet a notebooknak](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Adjon nevet a notebooknak")</span><span class="sxs-lookup"><span data-stu-id="dd0ad-134">![Provide a name for the notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="dd0ad-135">Most fog `import tensorflow` , és futtassa a hello world példa.</span><span class="sxs-lookup"><span data-stu-id="dd0ad-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="dd0ad-136">Kód másolása:</span><span class="sxs-lookup"><span data-stu-id="dd0ad-136">Code to copy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="dd0ad-137">Az eredmény fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="dd0ad-137">The result will look like this:</span></span>
    
    <span data-ttu-id="dd0ad-138">![TensorFlow kód végrehajtása](./media/hdinsight-apache-spark-python-package-installation/execution.png "hajtható végre TensorFlow kódot")</span><span class="sxs-lookup"><span data-stu-id="dd0ad-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="dd0ad-139"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="dd0ad-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="dd0ad-140">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="dd0ad-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="dd0ad-141">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="dd0ad-141">Scenarios</span></span>
* [<span data-ttu-id="dd0ad-142">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="dd0ad-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="dd0ad-143">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="dd0ad-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="dd0ad-144">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="dd0ad-144">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="dd0ad-145">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="dd0ad-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="dd0ad-146">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="dd0ad-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="dd0ad-147">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="dd0ad-147">Create and run applications</span></span>
* [<span data-ttu-id="dd0ad-148">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="dd0ad-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="dd0ad-149">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="dd0ad-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="dd0ad-150">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="dd0ad-150">Tools and extensions</span></span>
* [<span data-ttu-id="dd0ad-151">Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight</span><span class="sxs-lookup"><span data-stu-id="dd0ad-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="dd0ad-152">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="dd0ad-152">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="dd0ad-153">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="dd0ad-153">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="dd0ad-154">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="dd0ad-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="dd0ad-155">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="dd0ad-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="dd0ad-156">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="dd0ad-156">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="dd0ad-157">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="dd0ad-157">Manage resources</span></span>
* [<span data-ttu-id="dd0ad-158">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="dd0ad-158">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="dd0ad-159">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="dd0ad-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
