---
title: "az Azure HDInsight Spark kognitív eszközkészlet aaaMicrosoft mély biztonságával |} Microsoft Docs"
description: "Ismerje meg, hogyan képzett Microsoft kognitív eszközkészletre mély tanulási modell alkalmazott tooa dataset hello Python API Spark on Azure HDInsight Spark-fürtben lévő használatával lehet."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a><span data-ttu-id="0762b-103">Microsoft kognitív eszközkészlet mély tanulása Azure HDInsight Spark-fürt használatára</span><span class="sxs-lookup"><span data-stu-id="0762b-103">Use Microsoft Cognitive Toolkit deep learning model with Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="0762b-104">Ebben a cikkben szereplő lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="0762b-104">In this article, you do hello following steps.</span></span>

1. <span data-ttu-id="0762b-105">Egy egyéni parancsfájl tooinstall Microsoft kognitív eszközkészlet futtasson egy Azure HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="0762b-105">Run a custom script tooinstall Microsoft Cognitive Toolkit on an Azure HDInsight Spark cluster.</span></span>

2. <span data-ttu-id="0762b-106">Töltse fel a Jupyter notebook toohello Spark-fürt toosee hogyan tooapply képzett Microsoft kognitív eszközkészletre mély tanulási modell toofiles egy Azure Blob Storage-fiók hello segítségével a [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span><span class="sxs-lookup"><span data-stu-id="0762b-106">Upload a Jupyter notebook toohello Spark cluster toosee how tooapply a trained Microsoft Cognitive Toolkit deep learning model toofiles in an Azure Blob Storage Account using hello [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0762b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0762b-107">Prerequisites</span></span>

* <span data-ttu-id="0762b-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="0762b-108">**An Azure subscription**.</span></span> <span data-ttu-id="0762b-109">Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="0762b-109">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="0762b-110">Lásd: [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="0762b-110">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

* <span data-ttu-id="0762b-111">**Az Azure HDInsight Spark-fürt**.</span><span class="sxs-lookup"><span data-stu-id="0762b-111">**Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="0762b-112">Ez a cikk a Spark 2.0 fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0762b-112">For this article, create a Spark 2.0 cluster.</span></span> <span data-ttu-id="0762b-113">Útmutatásért lásd: [Azure HDInsight létrehozása Apache Spark-fürt](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="0762b-113">For instructions, see [Create Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="how-does-this-solution-flow"></a><span data-ttu-id="0762b-114">Hogyan nem flow ehhez a megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="0762b-114">How does this solution flow?</span></span>

<span data-ttu-id="0762b-115">Ebben a megoldásban ez a cikk és ebben az oktatóanyagban részeként feltöltött Jupyter notebook között oszlik meg.</span><span class="sxs-lookup"><span data-stu-id="0762b-115">This solution is divided between this article and a Jupyter notebook that you upload as part of this tutorial.</span></span> <span data-ttu-id="0762b-116">A cikkben végezze el a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="0762b-116">In this article, you complete hello following steps:</span></span>

* <span data-ttu-id="0762b-117">Futtasson egy parancsfájl műveletet egy HDInsight Spark-fürt tooinstall Microsoft kognitív eszközkészlet és Python-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="0762b-117">Run a script action on an HDInsight Spark cluster tooinstall Microsoft Cognitive Toolkit and Python packages.</span></span>
* <span data-ttu-id="0762b-118">Hello Jupyter notebook hello megoldás toohello HDInsight Spark-fürt futtató feltöltése.</span><span class="sxs-lookup"><span data-stu-id="0762b-118">Upload hello Jupyter notebook that runs hello solution toohello HDInsight Spark cluster.</span></span>

<span data-ttu-id="0762b-119">hello következő fennmaradó lépéseit lásd: hello Jupyter notebook.</span><span class="sxs-lookup"><span data-stu-id="0762b-119">hello following remaining steps are covered in hello Jupyter notebook.</span></span>

- <span data-ttu-id="0762b-120">Egy Spark Resiliant elosztott adatkészlet vagy RDD minta képek betöltése</span><span class="sxs-lookup"><span data-stu-id="0762b-120">Load sample images into a Spark Resiliant Distributed Dataset or RDD</span></span>
   - <span data-ttu-id="0762b-121">Modulok be, és a készletek meghatározása</span><span class="sxs-lookup"><span data-stu-id="0762b-121">Load modules and define presets</span></span>
   - <span data-ttu-id="0762b-122">Töltse le a hello adatkészletre helyileg a hello Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="0762b-122">Download hello dataset locally on hello Spark cluster</span></span>
   - <span data-ttu-id="0762b-123">Egy RDD hello adatkészlethez konvertálása</span><span class="sxs-lookup"><span data-stu-id="0762b-123">Convert hello dataset into an RDD</span></span>
- <span data-ttu-id="0762b-124">Pontszám hello képek kognitív eszközkészlet betanított modell segítségével</span><span class="sxs-lookup"><span data-stu-id="0762b-124">Score hello images using a trained Cognitive Toolkit model</span></span>
   - <span data-ttu-id="0762b-125">Töltse le a betanítása hello kognitív eszközkészlet modell toohello Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="0762b-125">Download hello trained Cognitive Toolkit model toohello Spark cluster</span></span>
   - <span data-ttu-id="0762b-126">Adja meg a munkavégző csomópontokhoz által használt funkciók toobe</span><span class="sxs-lookup"><span data-stu-id="0762b-126">Define functions toobe used by worker nodes</span></span>
   - <span data-ttu-id="0762b-127">A feldolgozó csomópontok pontszám hello lemezképek</span><span class="sxs-lookup"><span data-stu-id="0762b-127">Score hello images on worker nodes</span></span>
   - <span data-ttu-id="0762b-128">Értékelje ki a modell pontosságát</span><span class="sxs-lookup"><span data-stu-id="0762b-128">Evaluate model accuracy</span></span>


## <a name="install-microsoft-cognitive-toolkit"></a><span data-ttu-id="0762b-129">Microsoft kognitív eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="0762b-129">Install Microsoft Cognitive Toolkit</span></span>

<span data-ttu-id="0762b-130">Telepítheti a Microsoft kognitív eszközkészlet parancsfájlművelet használata Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="0762b-130">You can install Microsoft Cognitive Toolkit on a Spark cluster using script action.</span></span> <span data-ttu-id="0762b-131">Parancsfájlművelet egyéni parancsfájlok tooinstall összetevőket, amelyek nincsenek alapértelmezés szerint elérhető hello fürtön használja.</span><span class="sxs-lookup"><span data-stu-id="0762b-131">Script action uses custom scripts tooinstall components on hello cluster that are not available by default.</span></span> <span data-ttu-id="0762b-132">Hello hello Azure portál, egyéni parancsfájlt használhatja a HDInsight .NET SDK használatával, vagy az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="0762b-132">You can use hello custom script from hello Azure Portal, by using HDInsight .NET SDK, or by using Azure PowerShell.</span></span> <span data-ttu-id="0762b-133">Hello parancsfájl tooinstall hello eszközkészlet részeként a fürt létrehozása, vagy miután hello fürt megfelelően működik, és vagy is használható.</span><span class="sxs-lookup"><span data-stu-id="0762b-133">You can also use hello script tooinstall hello toolkit either as part of cluster creation, or after hello cluster is up and running.</span></span> 

<span data-ttu-id="0762b-134">Ebben a cikkben használjuk hello portál tooinstall hello eszközkészlet hello fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="0762b-134">In this article, we use hello portal tooinstall hello toolkit, after hello cluster has been created.</span></span> <span data-ttu-id="0762b-135">Más módokon toorun hello egyéni parancsfájl, lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0762b-135">For other ways toorun hello custom script, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="0762b-136">Hello Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="0762b-136">Using hello Azure Portal</span></span>

<span data-ttu-id="0762b-137">Hogyan toouse hello Azure Portal toorun parancsfájl-e a művelet, lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span><span class="sxs-lookup"><span data-stu-id="0762b-137">For instructions on how toouse hello Azure Portal toorun script action, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span></span> <span data-ttu-id="0762b-138">Ellenőrizze, hogy a következő bemenet tooinstall kognitív eszközkészlet Microsoft hello megadnia.</span><span class="sxs-lookup"><span data-stu-id="0762b-138">Make sure you provide hello following inputs tooinstall Microsoft Cognitive Toolkit.</span></span>

* <span data-ttu-id="0762b-139">Adjon meg egy értéket hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="0762b-139">Provide a value for hello script action name.</span></span>

* <span data-ttu-id="0762b-140">A **Bash parancsfájlok URI**, adja meg `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span><span class="sxs-lookup"><span data-stu-id="0762b-140">For **Bash script URI**, enter `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span></span>

* <span data-ttu-id="0762b-141">Ellenőrizze, hogy futtatja hello parancsfájl csak hello head és feldolgozó csomópontokat, és törölje az összes hello más jelölőnégyzeteket.</span><span class="sxs-lookup"><span data-stu-id="0762b-141">Make sure you run hello script only on hello head and worker nodes and clear all hello other checkboxes.</span></span>

* <span data-ttu-id="0762b-142">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0762b-142">Click **Create**.</span></span>

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a><span data-ttu-id="0762b-143">Töltse fel a hello Jupyter notebook tooAzure HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="0762b-143">Upload hello Jupyter notebook tooAzure HDInsight Spark cluster</span></span>

<span data-ttu-id="0762b-144">Microsoft kognitív eszközkészlet toouse hello hello Azure HDInsight Spark-fürt, be kell tölteni a hello Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="0762b-144">toouse hello Microsoft Cognitive Toolkit with hello Azure HDInsight Spark cluster, you must load hello Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="0762b-145">A notebook érhető el a Githubon: [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="0762b-145">This notebook is available on GitHub at [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span>

1. <span data-ttu-id="0762b-146">Klónozás hello GitHub-tárházban [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="0762b-146">Clone hello GitHub repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span> <span data-ttu-id="0762b-147">További utasítások tooclone: [egy tárház klónozása](https://help.github.com/articles/cloning-a-repository/).</span><span class="sxs-lookup"><span data-stu-id="0762b-147">For instructions tooclone, see [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).</span></span>

2. <span data-ttu-id="0762b-148">Az Azure portál hello, nyissa meg a hello Spark-fürt panelén, amelyre már kiépített, **fürt irányítópult**, és kattintson a **Jupyter notebook**.</span><span class="sxs-lookup"><span data-stu-id="0762b-148">From hello Azure Portal, open hello Spark cluster blade that you already provisioned, click **Cluster Dashboard**, and then click **Jupyter notebook**.</span></span>

    <span data-ttu-id="0762b-149">Is elindíthatja a hello Jupyter notebook toohello URL-cím megnyitásával `https://<clustername>.azurehdinsight.net/jupyter/`.</span><span class="sxs-lookup"><span data-stu-id="0762b-149">You can also launch hello Jupyter notebook by going toohello URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span></span> <span data-ttu-id="0762b-150">Cserélje le \<clustername > hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0762b-150">Replace \<clustername> with hello name of your HDInsight cluster.</span></span>

3. <span data-ttu-id="0762b-151">A hello Jupyter notebook, kattintson az **feltöltése** a hello jobb felső sarokban, és navigáljon a toohello helyre, ahol klónozott hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="0762b-151">From hello Jupyter notebook, click **Upload** in hello top-right corner and then navigate toohello location where you cloned hello GitHub repository.</span></span>

    <span data-ttu-id="0762b-152">![Töltse fel a Jupyter notebook tooAzure HDInsight Spark-fürt](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "feltöltése Jupyter notebook tooAzure HDInsight Spark-fürt")</span><span class="sxs-lookup"><span data-stu-id="0762b-152">![Upload Jupyter notebook tooAzure HDInsight Spark cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Upload Jupyter notebook tooAzure HDInsight Spark cluster")</span></span>

4. <span data-ttu-id="0762b-153">Kattintson a **feltöltése** újra.</span><span class="sxs-lookup"><span data-stu-id="0762b-153">Click **Upload** again.</span></span>

5. <span data-ttu-id="0762b-154">Után hello notebook töltheti fel, hello notebook neve hello kattintson, és majd utasítások hello hello jegyzetfüzet maga tooload hello adatkészlet és a hello az oktatóanyag végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0762b-154">After hello notebook is uploaded, click hello name of hello notebook and then follow hello instructions in hello notebook itself on how tooload hello data set and perform hello tutorial.</span></span>

## <a name="see-also"></a><span data-ttu-id="0762b-155">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0762b-155">See also</span></span>
* [<span data-ttu-id="0762b-156">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="0762b-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="0762b-157">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="0762b-157">Scenarios</span></span>
* [<span data-ttu-id="0762b-158">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="0762b-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="0762b-159">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="0762b-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="0762b-160">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="0762b-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="0762b-161">Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására</span><span class="sxs-lookup"><span data-stu-id="0762b-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="0762b-162">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="0762b-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="0762b-163">Az Application Insights telemetriai adatainak elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="0762b-163">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="0762b-164">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="0762b-164">Create and run applications</span></span>
* [<span data-ttu-id="0762b-165">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="0762b-165">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="0762b-166">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="0762b-166">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="0762b-167">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="0762b-167">Tools and extensions</span></span>
* [<span data-ttu-id="0762b-168">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0762b-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="0762b-169">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="0762b-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="0762b-170">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="0762b-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="0762b-171">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="0762b-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="0762b-172">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="0762b-172">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="0762b-173">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="0762b-173">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="0762b-174">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="0762b-174">Manage resources</span></span>
* [<span data-ttu-id="0762b-175">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="0762b-175">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="0762b-176">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="0762b-176">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
