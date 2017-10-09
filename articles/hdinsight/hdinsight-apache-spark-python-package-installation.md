---
title: "aaaScript művelet – telepítése Python-csomagokat, az Azure hdinsight Jupyter |} Microsoft Docs"
description: "Lépésenként hogyan toouse parancsfájl művelet tooconfigure Jupyter notebookok érhető el a HDInsight Spark-fürtök toouse külső python-csomagokat."
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
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="e3b2c-103">Parancsfájlművelet tooinstall külső Python-csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight</span><span class="sxs-lookup"><span data-stu-id="e3b2c-103">Use Script Action tooinstall external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e3b2c-104">Cella magic használatával</span><span class="sxs-lookup"><span data-stu-id="e3b2c-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="e3b2c-105">Parancsfájl művelet segítségével</span><span class="sxs-lookup"><span data-stu-id="e3b2c-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="e3b2c-106">Ismerje meg, hogyan toouse Parancsfájlműveletek tooconfigure Apache Spark-fürt a HDInsight (Linux) toouse külső, közösségi-hozzájárult **python** csomagok, amelyek nem szerepelnek hello fürt out-of-az-box.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-106">Learn how toouse Script Actions tooconfigure an Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed **python** packages that are not included out-of-the-box in hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="e3b2c-107">Jupyter notebook használatával is konfigurálhatja `%%configure` magic toouse külső csomagok.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-107">You can also configure a Jupyter notebook by using `%%configure` magic toouse external packages.</span></span> <span data-ttu-id="e3b2c-108">Útmutatásért lásd: [külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="e3b2c-109">Hello kereshet [csomagindexet](https://pypi.python.org/pypi) hello csomagok rendelkezésre álló teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-109">You can search hello [package index](https://pypi.python.org/pypi) for hello complete list of packages that are available.</span></span> <span data-ttu-id="e3b2c-110">Más forrásból is megkapható elérhető csomagok listáját.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="e3b2c-111">Például telepíthet keresztül elérhető csomagok [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) vagy [conda-forge](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="e3b2c-112">Ebből a cikkből megtudhatja, hogyan tooinstall hello [TensorFlow](https://www.tensorflow.org/) csomag parancsfájl Actoin használatával a fürtön, és keresztül hello Jupyter notebook használni.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-112">In this article, you will learn how tooinstall hello [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via hello Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3b2c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e3b2c-113">Prerequisites</span></span>
<span data-ttu-id="e3b2c-114">Hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="e3b2c-114">You must have hello following:</span></span>

* <span data-ttu-id="e3b2c-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-115">An Azure subscription.</span></span> <span data-ttu-id="e3b2c-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="e3b2c-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="e3b2c-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="e3b2c-119">Ha még nem rendelkezik egy Spark-fürt HDInsight Linux rendszeren, Parancsfájlműveletek futtathatja a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="e3b2c-120">Keresse fel a hello dokumentáció [hogyan toouse egyéni parancsfájl-műveletek](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-120">Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="e3b2c-121">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="e3b2c-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="e3b2c-122">A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-122">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="e3b2c-123">Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-123">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="e3b2c-124">Hello Spark-fürt panelén kattintson **Parancsfájlműveletek** alatt **használati**.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-124">From hello Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="e3b2c-125">Futtassa a következő egyéni művelet hello telepítésére TensorFlow hello átjárócsomópontokkal és hello munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-125">Run hello custom action that installs TensorFlow in hello head nodes and hello worker nodes.</span></span> <span data-ttu-id="e3b2c-126">hello bash parancsfájlok lehet hivatkozni: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh látogasson el hello dokumentációja [hogyan toouse egyéni parancsfájl-műveletek](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="e3b2c-126">hello bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="e3b2c-127">Nincsenek két python hello fürt telepítése.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-127">There are two python installations in hello cluster.</span></span> <span data-ttu-id="e3b2c-128">Spark fogja használni a hello Anaconda python telepítési helye: `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-128">Spark will use hello Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="e3b2c-129">Hivatkozás a telepítést, az egyéni műveletek keresztül a `/usr/bin/anaconda/bin/pip` és `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="e3b2c-130">Nyissa meg a PySpark Jupyter notebook</span><span class="sxs-lookup"><span data-stu-id="e3b2c-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="e3b2c-131">![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Új Jupyter notebook létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e3b2c-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="e3b2c-132">Új notebook létrejött, és Untitled.pynb hello nevű.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-132">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="e3b2c-133">Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-133">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="e3b2c-134">![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "adjon meg egy nevet hello notebook")</span><span class="sxs-lookup"><span data-stu-id="e3b2c-134">![Provide a name for hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="e3b2c-135">Most fog `import tensorflow` , és futtassa a hello world példa.</span><span class="sxs-lookup"><span data-stu-id="e3b2c-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="e3b2c-136">Kód toocopy:</span><span class="sxs-lookup"><span data-stu-id="e3b2c-136">Code toocopy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="e3b2c-137">hello eredményt fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="e3b2c-137">hello result will look like this:</span></span>
    
    <span data-ttu-id="e3b2c-138">![TensorFlow kód végrehajtása](./media/hdinsight-apache-spark-python-package-installation/execution.png "hajtható végre TensorFlow kódot")</span><span class="sxs-lookup"><span data-stu-id="e3b2c-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="e3b2c-139"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e3b2c-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="e3b2c-140">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="e3b2c-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="e3b2c-141">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e3b2c-141">Scenarios</span></span>
* [<span data-ttu-id="e3b2c-142">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="e3b2c-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="e3b2c-143">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="e3b2c-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="e3b2c-144">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="e3b2c-144">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="e3b2c-145">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="e3b2c-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="e3b2c-146">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="e3b2c-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="e3b2c-147">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="e3b2c-147">Create and run applications</span></span>
* [<span data-ttu-id="e3b2c-148">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="e3b2c-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="e3b2c-149">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="e3b2c-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="e3b2c-150">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="e3b2c-150">Tools and extensions</span></span>
* [<span data-ttu-id="e3b2c-151">Külső csomagok használata Jupyter notebookok Apache Spark-fürt a HDInsight</span><span class="sxs-lookup"><span data-stu-id="e3b2c-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="e3b2c-152">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e3b2c-152">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="e3b2c-153">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="e3b2c-153">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="e3b2c-154">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="e3b2c-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="e3b2c-155">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="e3b2c-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="e3b2c-156">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="e3b2c-156">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="e3b2c-157">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="e3b2c-157">Manage resources</span></span>
* [<span data-ttu-id="e3b2c-158">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="e3b2c-158">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="e3b2c-159">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e3b2c-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
