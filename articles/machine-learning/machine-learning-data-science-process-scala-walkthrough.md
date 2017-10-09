---
title: "Tudományos aaaData Scala és Spark használata az Azure-on |} Microsoft Docs"
description: "Hogyan toouse Scala a felügyelt gépi tanulási feladatok hello Spark méretezhető MLlib és Spark ML-csomagokat a egy Azure HDInsight Spark-fürt."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: e32ebd0b91417183fe48ee10ebc7929fd9605762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a><span data-ttu-id="a7b32-103">Adatelemzés a Scala és a Spark használatával az Azure rendszerben</span><span class="sxs-lookup"><span data-stu-id="a7b32-103">Data Science using Scala and Spark on Azure</span></span>
<span data-ttu-id="a7b32-104">Ez a cikk bemutatja, hogyan toouse Scala felügyelt gépi tanulási feladatok hello Spark méretezhető MLlib és Spark ML csomagok egy Azure HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="a7b32-104">This article shows you how toouse Scala for supervised machine learning tasks with hello Spark scalable MLlib and Spark ML packages on an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="a7b32-105">Az bemutatja, hogyan hello feladatok hello alkotó [Adattudomány folyamat](http://aka.ms/datascienceprocess): adatfeldolgozást és a feltárása, a képi megjelenítés, a szolgáltatás mérnöki csapathoz, a modellezési és a model felhasználás.</span><span class="sxs-lookup"><span data-stu-id="a7b32-105">It walks you through hello tasks that constitute hello [Data Science process](http://aka.ms/datascienceprocess): data ingestion and exploration, visualization, feature engineering, modeling, and model consumption.</span></span> <span data-ttu-id="a7b32-106">hello cikkben hello modellek között logisztikai és lineáris regressziós, véletlenszerű erdők és színátmenetes súlyozott fák (GBTs), továbbá tootwo közös felügyelt gépi tanulási feladatok:</span><span class="sxs-lookup"><span data-stu-id="a7b32-106">hello models in hello article include logistic and linear regression, random forests, and gradient-boosted trees (GBTs), in addition tootwo common supervised machine learning tasks:</span></span>

* <span data-ttu-id="a7b32-107">Regressziós probléma: hello tipp összeg (SPN) taxi útnak előrejelzését</span><span class="sxs-lookup"><span data-stu-id="a7b32-107">Regression problem: Prediction of hello tip amount ($) for a taxi trip</span></span>
* <span data-ttu-id="a7b32-108">Bináris osztályozás: előrejelzését tipp vagy taxi útnak nincs ötlet (1 vagy 0)</span><span class="sxs-lookup"><span data-stu-id="a7b32-108">Binary classification: Prediction of tip or no tip (1/0) for a taxi trip</span></span>

<span data-ttu-id="a7b32-109">folyamat modellezési hello betanítása és kiértékelése egy teszt adatkészlet és a megfelelő pontossága metrikák igényel.</span><span class="sxs-lookup"><span data-stu-id="a7b32-109">hello modeling process requires training and evaluation on a test data set and relevant accuracy metrics.</span></span> <span data-ttu-id="a7b32-110">Ebből a cikkből megtudhatja hogyan toostore ezek a modellek az Azure Blob Storage tárolóban, és hogyan tooscore és értékelje a prediktív teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="a7b32-110">In this article, you can learn how toostore these models in Azure Blob storage and how tooscore and evaluate their predictive performance.</span></span> <span data-ttu-id="a7b32-111">Ez a cikk is magában foglalja a hello fejlettebb, témakörök, hogyan toooptimize modellek kereszt-ellenőrzési és a hyper-paraméter abszolút használatával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-111">This article also covers hello more advanced topics of how toooptimize models by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="a7b32-112">használt hello adatok látható egy minta hello 2013 NYC taxi út és a jegy ára adatkészlet elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="a7b32-112">hello data used is a sample of hello 2013 NYC taxi trip and fare data set available on GitHub.</span></span>

<span data-ttu-id="a7b32-113">[Scala](http://www.scala-lang.org/), hello Java virtuális gép, a nyelvet integrálja az objektumorientált és funkcionális nyelvi fogalmak.</span><span class="sxs-lookup"><span data-stu-id="a7b32-113">[Scala](http://www.scala-lang.org/), a language based on hello Java virtual machine, integrates object-oriented and functional language concepts.</span></span> <span data-ttu-id="a7b32-114">Feldolgozás alatt álló hello felhő kiválóan alkalmas toodistributed, és az Azure Spark-fürtjei fut méretezhető nyelven.</span><span class="sxs-lookup"><span data-stu-id="a7b32-114">It's a scalable language that is well suited toodistributed processing in hello cloud, and runs on Azure Spark clusters.</span></span>

<span data-ttu-id="a7b32-115">[Spark](http://spark.apache.org/) egy nyílt forráskódú párhuzamos feldolgozási keretrendszer, amely támogatja a memórián belüli feldolgozását végzi big data elemzés alkalmazások tooboost hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="a7b32-115">[Spark](http://spark.apache.org/) is an open-source parallel-processing framework that supports in-memory processing tooboost hello performance of big data analytics applications.</span></span> <span data-ttu-id="a7b32-116">hello Spark program sebességét, a könnyű, valamint a kifinomult analytics lett tervezve.</span><span class="sxs-lookup"><span data-stu-id="a7b32-116">hello Spark processing engine is built for speed, ease of use, and sophisticated analytics.</span></span> <span data-ttu-id="a7b32-117">A Spark memóriában elosztott tárolt számítási képességei jól funkcionálnak a iteratív algoritmusaival a machine learning és a graph számítások.</span><span class="sxs-lookup"><span data-stu-id="a7b32-117">Spark's in-memory distributed computation capabilities make it a good choice for iterative algorithms in machine learning and graph computations.</span></span> <span data-ttu-id="a7b32-118">Hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) csomag egységes teszi lehetővé a magas szintű API-k adatokat, melyek segíthetnek keretek létrehozása és gyakorlati gépi tanulási a folyamatok hangolására platformra épül.</span><span class="sxs-lookup"><span data-stu-id="a7b32-118">hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) package provides a uniform set of high-level APIs built on top of data frames that can help you create and tune practical machine learning pipelines.</span></span> <span data-ttu-id="a7b32-119">[MLlib](http://spark.apache.org/mllib/) a Spark méretezhető gépi tanulás függvénytár, amely modellezési képességekkel toothis elosztott környezetekben.</span><span class="sxs-lookup"><span data-stu-id="a7b32-119">[MLlib](http://spark.apache.org/mllib/) is Spark's scalable machine learning library, which brings modeling capabilities toothis distributed environment.</span></span>

<span data-ttu-id="a7b32-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) hello Azure által üzemeltetett elérhető nyílt forráskódú Spark van.</span><span class="sxs-lookup"><span data-stu-id="a7b32-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is hello Azure-hosted offering of open-source Spark.</span></span> <span data-ttu-id="a7b32-121">Is támogatja a Jupyter Scala notebookok hello Spark-fürtön, és futtatása Spark SQL interaktív lekérdezések tootransform szűrheti, és az Azure Blob storage szolgáltatásban tárolt adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a7b32-121">It also includes support for Jupyter Scala notebooks on hello Spark cluster, and can run Spark SQL interactive queries tootransform, filter, and visualize data stored in Azure Blob storage.</span></span> <span data-ttu-id="a7b32-122">hello Scala kódtöredékek ebben a cikkben, amely hello megoldást nyújtanak, és megfelelő előkészítésére hello toovisualize hello adatok megjelenítése futtatása Jupyter notebookok hello Spark-fürtjei telepítve.</span><span class="sxs-lookup"><span data-stu-id="a7b32-122">hello Scala code snippets in this article that provide hello solutions and show hello relevant plots toovisualize hello data run in Jupyter notebooks installed on hello Spark clusters.</span></span> <span data-ttu-id="a7b32-123">hello modellezési lépéseket a következő témakörök rendelkezik code, hogy bemutatja, hogyan tootrain, értékelje ki, mentse és felhasználását egyes beírta modell.</span><span class="sxs-lookup"><span data-stu-id="a7b32-123">hello modeling steps in these topics have code that shows you how tootrain, evaluate, save, and consume each type of model.</span></span>

<span data-ttu-id="a7b32-124">hello beállítási lépéseket és kód ebben a cikkben az Azure HDInsight 3.4 Spark 1.6-os vannak.</span><span class="sxs-lookup"><span data-stu-id="a7b32-124">hello setup steps and code in this article are for Azure HDInsight 3.4 Spark 1.6.</span></span> <span data-ttu-id="a7b32-125">Ebben a cikkben és a hello kód azonban hello [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) általánosak, és a Spark-fürt működnek.</span><span class="sxs-lookup"><span data-stu-id="a7b32-125">However, hello code in this article and in hello [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) are generic and should work on any Spark cluster.</span></span> <span data-ttu-id="a7b32-126">hello fürt beállítása és felügyeleti lépéseket előfordulhat, hogy mi is megjelennek ebben a cikkben nem használata a HDInsight Spark némileg eltérő.</span><span class="sxs-lookup"><span data-stu-id="a7b32-126">hello cluster setup and management steps might be slightly different from what is shown in this article if you are not using HDInsight Spark.</span></span>

> [!NOTE]
> <span data-ttu-id="a7b32-127">Ez a témakör azt ismerteti, hogyan toouse Scala helyett Python toocomplete feladatokat egy végpontok közötti Adattudomány folyamat, lásd: [Spark on Azure HDInsight használatának Adattudomány](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a7b32-127">For a topic that shows you how toouse Python rather than Scala toocomplete tasks for an end-to-end Data Science process, see [Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a7b32-128">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7b32-128">Prerequisites</span></span>
* <span data-ttu-id="a7b32-129">Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a7b32-129">You must have an Azure subscription.</span></span> <span data-ttu-id="a7b32-130">Ha még nem rendelkezik egy, [egy Azure ingyenes próbaverzió beszerzése](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a7b32-130">If you do not already have one, [get an Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="a7b32-131">Az alábbi eljárásokat Azure HDInsight 3.4 Spark 1.6-os fürt toocomplete hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a7b32-131">You need an Azure HDInsight 3.4 Spark 1.6 cluster toocomplete hello following procedures.</span></span> <span data-ttu-id="a7b32-132">a fürt toocreate hello utasításait lásd: [első lépések: hozzon létre Apache Spark on Azure Hdinsighttal](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="a7b32-132">toocreate a cluster, see hello instructions in [Get started: Create Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="a7b32-133">Hello fürt típusa és verzió beállítása hello **fürt típusának kiválasztása** menü.</span><span class="sxs-lookup"><span data-stu-id="a7b32-133">Set hello cluster type and version on hello **Select Cluster Type** menu.</span></span>

![A HDInsight fürt típus konfigurálása](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

<span data-ttu-id="a7b32-135">Hello NYC taxi vissza adatokat, valamint útmutatást a hogyan tooexecute hello Spark-fürt a Jupyter notebook a kód leírását hello megfelelő szakaszaiban talál [áttekintése adatok tudományos Spark on Azure HDInsight használatának](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a7b32-135">For a description of hello NYC taxi trip data and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster, see hello relevant sections in [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a><span data-ttu-id="a7b32-136">A Spark-fürt hello Jupyter notebook hajt végre Scala kódot</span><span class="sxs-lookup"><span data-stu-id="a7b32-136">Execute Scala code from a Jupyter notebook on hello Spark cluster</span></span>
<span data-ttu-id="a7b32-137">Az Azure-portálon hello Jupyter notebook indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="a7b32-137">You can launch a Jupyter notebook from hello Azure portal.</span></span> <span data-ttu-id="a7b32-138">Spark-fürt hello az irányítópulton található, és kattintson az tooenter hello felügyelet lapon a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="a7b32-138">Find hello Spark cluster on your dashboard, and then click it tooenter hello management page for your cluster.</span></span> <span data-ttu-id="a7b32-139">Ezután kattintson **fürt irányítópultok**, és kattintson a **Jupyter Notebook** tooopen hello notebook társított hello Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="a7b32-139">Next, click **Cluster Dashboards**, and then click **Jupyter Notebook** tooopen hello notebook associated with hello Spark cluster.</span></span>

![Fürt-irányítópult és a Jupyter notebookok](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

<span data-ttu-id="a7b32-141">Jupyter notebookok: https:// is hozzáférhet&lt;clustername&gt;.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="a7b32-141">You also can access Jupyter notebooks at https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="a7b32-142">Cserélje le *clustername* hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="a7b32-142">Replace *clustername* with hello name of your cluster.</span></span> <span data-ttu-id="a7b32-143">A rendszergazdai fiók tooaccess hello Jupyter notebookok hello jelszó szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7b32-143">You need hello password for your administrator account tooaccess hello Jupyter notebooks.</span></span>

![Nyissa meg tooJupyter notebookok hello fürt neve](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

<span data-ttu-id="a7b32-145">Válassza ki **Scala** toosee egy könyvtárat, amely rendelkezik néhány példa a előre csomagolt notebookok adott használata hello PySpark API.</span><span class="sxs-lookup"><span data-stu-id="a7b32-145">Select **Scala** toosee a directory that has a few examples of prepackaged notebooks that use hello PySpark API.</span></span> <span data-ttu-id="a7b32-146">Ennek a programcsomagnak Spark témakörökből áll rendelkezésre a hello mintakódok tartalmazó Scala.ipynb notebook használatával feltárása modellezéséhez és pontozás hello [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span><span class="sxs-lookup"><span data-stu-id="a7b32-146">hello Exploration Modeling and Scoring using Scala.ipynb notebook that contains hello code samples for this suite of Spark topics is available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span></span>

<span data-ttu-id="a7b32-147">Közvetlenül a Githubból toohello Jupyter Notebook server hello notebook feltöltheti a Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="a7b32-147">You can upload hello notebook directly from GitHub toohello Jupyter Notebook server on your Spark cluster.</span></span> <span data-ttu-id="a7b32-148">A Jupyter kezdőlapján kattintson a hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7b32-148">On your Jupyter home page, click hello **Upload** button.</span></span> <span data-ttu-id="a7b32-149">A Fájlkezelőben hello hello Scala jegyzetfüzet hello GitHub (nyers tartalom) URL-cím illeszthető be, és kattintson **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="a7b32-149">In hello file explorer, paste hello GitHub (raw content) URL of hello Scala notebook, and then click **Open**.</span></span> <span data-ttu-id="a7b32-150">a következő URL-cím hello hello Scala notebook érhető el:</span><span class="sxs-lookup"><span data-stu-id="a7b32-150">hello Scala notebook is available at hello following URL:</span></span>

[<span data-ttu-id="a7b32-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span><span class="sxs-lookup"><span data-stu-id="a7b32-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span></span>](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a><span data-ttu-id="a7b32-152">Telepítő: Előre definiált Spark és Hive-környezetek, Spark magics és Spark-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="a7b32-152">Setup: Preset Spark and Hive contexts, Spark magics, and Spark libraries</span></span>
### <a name="preset-spark-and-hive-contexts"></a><span data-ttu-id="a7b32-153">Spark és Hive-környezetek az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="a7b32-153">Preset Spark and Hive contexts</span></span>
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


<span data-ttu-id="a7b32-154">a Jupyter notebookok biztosított hello Spark mag beállításkészletet kontextusban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a7b32-154">hello Spark kernels that are provided with Jupyter notebooks have preset contexts.</span></span> <span data-ttu-id="a7b32-155">Set hello Spark- vagy Hive környezetek tooexplicitly nincs szükség a fejlesztői hello alkalmazás használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="a7b32-155">You don't need tooexplicitly set hello Spark or Hive contexts before you start working with hello application you are developing.</span></span> <span data-ttu-id="a7b32-156">hello beállításkészletet kontextusban a következők:</span><span class="sxs-lookup"><span data-stu-id="a7b32-156">hello preset contexts are:</span></span>

* <span data-ttu-id="a7b32-157">`sc`a SparkContext</span><span class="sxs-lookup"><span data-stu-id="a7b32-157">`sc` for SparkContext</span></span>
* <span data-ttu-id="a7b32-158">`sqlContext`a HiveContext</span><span class="sxs-lookup"><span data-stu-id="a7b32-158">`sqlContext` for HiveContext</span></span>

### <a name="spark-magics"></a><span data-ttu-id="a7b32-159">Spark magics</span><span class="sxs-lookup"><span data-stu-id="a7b32-159">Spark magics</span></span>
<span data-ttu-id="a7b32-160">hello Spark kernel tartalmaz néhány előre definiált "magics", amelyeket meghívhatja a különleges parancsok `%%`.</span><span class="sxs-lookup"><span data-stu-id="a7b32-160">hello Spark kernel provides some predefined “magics,” which are special commands that you can call with `%%`.</span></span> <span data-ttu-id="a7b32-161">Ezek a parancsok közül kettő a következő mintakódok hello használnak.</span><span class="sxs-lookup"><span data-stu-id="a7b32-161">Two of these commands are used in hello following code samples.</span></span>

* <span data-ttu-id="a7b32-162">`%%local`Meghatározza, hogy egymás utáni sorok hello kód végrehajtja helyileg.</span><span class="sxs-lookup"><span data-stu-id="a7b32-162">`%%local` specifies that hello code in subsequent lines will be executed locally.</span></span> <span data-ttu-id="a7b32-163">hello kód érvényes Scala-kódot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a7b32-163">hello code must be valid Scala code.</span></span>
* <span data-ttu-id="a7b32-164">`%%sql -o <variable name>`végrehajtja a Hive-lekérdezések elleni `sqlContext`.</span><span class="sxs-lookup"><span data-stu-id="a7b32-164">`%%sql -o <variable name>` executes a Hive query against `sqlContext`.</span></span> <span data-ttu-id="a7b32-165">Ha hello `-o` paramétert, a hello megőrződjenek hello hello lekérdezés eredménye `%%local` adatok keretként Spark Scala környezetben.</span><span class="sxs-lookup"><span data-stu-id="a7b32-165">If hello `-o` parameter is passed, hello result of hello query is persisted in hello `%%local` Scala context as a Spark data frame.</span></span>

<span data-ttu-id="a7b32-166">A Jupyter notebookokból és az előre definiált hello kernelek kapcsolatos további információk "magics", amely meghívja a `%%` (például `%%local`), lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a fürtök HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="a7b32-166">For more information about hello kernels for Jupyter notebooks and their predefined "magics" that you call with `%%` (for example, `%%local`), see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

### <a name="import-libraries"></a><span data-ttu-id="a7b32-167">Könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-167">Import libraries</span></span>
<span data-ttu-id="a7b32-168">Importálja a hello Spark, MLlib és egyéb szalagtárak, szüksége lesz a következő kód hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="a7b32-168">Import hello Spark, MLlib, and other libraries you'll need by using hello following code.</span></span>

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a><span data-ttu-id="a7b32-169">Adatfeldolgozás</span><span class="sxs-lookup"><span data-stu-id="a7b32-169">Data ingestion</span></span>
<span data-ttu-id="a7b32-170">hello hello Adattudomány folyamat első lépése az tooingest hello adatai, amelyet az tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="a7b32-170">hello first step in hello Data Science process is tooingest hello data that you want tooanalyze.</span></span> <span data-ttu-id="a7b32-171">Kapcsolása hello adatok külső forrásból vagy rendszerek helyét az adatok feltárása és modellezési környezetbe.</span><span class="sxs-lookup"><span data-stu-id="a7b32-171">You bring hello data from external sources or systems where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="a7b32-172">Ebben a cikkben hello meg a betöltési adata illesztett 0,1 % minta hello taxi út és a jegy ára fájl (.tsv fájlként tárolja).</span><span class="sxs-lookup"><span data-stu-id="a7b32-172">In this article, hello data you ingest is a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file).</span></span> <span data-ttu-id="a7b32-173">hello adatok feltárása és modellezési környezete Spark.</span><span class="sxs-lookup"><span data-stu-id="a7b32-173">hello data exploration and modeling environment is Spark.</span></span> <span data-ttu-id="a7b32-174">Ez a szakasz hello kód toocomplete hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a7b32-174">This section contains hello code toocomplete hello following series of tasks:</span></span>

1. <span data-ttu-id="a7b32-175">Adatok és a modell tárolás a könyvtár elérési útvonalak beállítása.</span><span class="sxs-lookup"><span data-stu-id="a7b32-175">Set directory paths for data and model storage.</span></span>
2. <span data-ttu-id="a7b32-176">Olvassa el a hello bemeneti adatkészletben (.tsv fájlként tárolja).</span><span class="sxs-lookup"><span data-stu-id="a7b32-176">Read in hello input data set (stored as a .tsv file).</span></span>
3. <span data-ttu-id="a7b32-177">Adja meg a hello és tiszta hello adatainak sémát.</span><span class="sxs-lookup"><span data-stu-id="a7b32-177">Define a schema for hello data and clean hello data.</span></span>
4. <span data-ttu-id="a7b32-178">Megtisztított adatok keret létrehozása, és a gyorsítótár, a memória.</span><span class="sxs-lookup"><span data-stu-id="a7b32-178">Create a cleaned data frame and cache it in memory.</span></span>
5. <span data-ttu-id="a7b32-179">Hello adatok regisztrálható az SQLContext ideiglenes táblájába.</span><span class="sxs-lookup"><span data-stu-id="a7b32-179">Register hello data as a temporary table in SQLContext.</span></span>
6. <span data-ttu-id="a7b32-180">Hello tábla lekérdezése, és importálja a hello eredmények adatok keretbe.</span><span class="sxs-lookup"><span data-stu-id="a7b32-180">Query hello table and import hello results into a data frame.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a><span data-ttu-id="a7b32-181">Az Azure Blob storage tárolási helyek könyvtár elérési útja beállítása</span><span class="sxs-lookup"><span data-stu-id="a7b32-181">Set directory paths for storage locations in Azure Blob storage</span></span>
<span data-ttu-id="a7b32-182">Spark és írhat tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a7b32-182">Spark can read and write tooAzure Blob storage.</span></span> <span data-ttu-id="a7b32-183">Spark tooprocess használja, a meglévő adatok valamelyikét, és tárolására is használható majd hello eredmények újra a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a7b32-183">You can use Spark tooprocess any of your existing data, and then store hello results again in Blob storage.</span></span>

<span data-ttu-id="a7b32-184">toosave modellek vagy a Blob Storage tárolóban fájlok tooproperly kell hello elérési utat adjon meg.</span><span class="sxs-lookup"><span data-stu-id="a7b32-184">toosave models or files in Blob storage, you need tooproperly specify hello path.</span></span> <span data-ttu-id="a7b32-185">Hivatkozás hello alapértelmezett tároló csatolt toohello Spark-fürt kezdődő elérési úttal `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="a7b32-185">Reference hello default container attached toohello Spark cluster by using a path that begins with `wasb:///`.</span></span> <span data-ttu-id="a7b32-186">Egyéb helyek hivatkozni használatával `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="a7b32-186">Reference other locations by using `wasb://`.</span></span>

<span data-ttu-id="a7b32-187">hello alábbi kódminta helyét adja meg hello hello bemeneti adatok toobe olvassa el és hello elérési tooBlob tárolókhoz csatolt toohello Spark-fürt hello modell menteni.</span><span class="sxs-lookup"><span data-stu-id="a7b32-187">hello following code sample specifies hello location of hello input data toobe read and hello path tooBlob storage that is attached toohello Spark cluster where hello model will be saved.</span></span>

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a><span data-ttu-id="a7b32-188">Adatok importálása, hozzon létre egy RDD, és olyan adatokat, keret toohello séma szerint</span><span class="sxs-lookup"><span data-stu-id="a7b32-188">Import data, create an RDD, and define a data frame according toohello schema</span></span>
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello SCHEMA BASED ON hello HEADER OF hello FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING toohello SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE hello CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER hello DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-189">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-189">**Output:**</span></span>

<span data-ttu-id="a7b32-190">Idő toorun hello cella: 8 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-190">Time toorun hello cell: 8 seconds.</span></span>

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a><span data-ttu-id="a7b32-191">Hello tábla lekérdezése, és adatok keretben eredmények importálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-191">Query hello table and import results in a data frame</span></span>
<span data-ttu-id="a7b32-192">Mellett, hello lekérdezéstábla jegy ára, utas és tipp adatok; szűrő sérült, és a külső adatai; és nyomtathatják ki több sort.</span><span class="sxs-lookup"><span data-stu-id="a7b32-192">Next, query hello table for fare, passenger, and tip data; filter out corrupt and outlying data; and print several rows.</span></span>

    # QUERY hello DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY hello TOP THREE ROWS
    sqlResultsDF.show(3)

<span data-ttu-id="a7b32-193">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-193">**Output:**</span></span>

| <span data-ttu-id="a7b32-194">fare_amount</span><span class="sxs-lookup"><span data-stu-id="a7b32-194">fare_amount</span></span> | <span data-ttu-id="a7b32-195">passenger_count</span><span class="sxs-lookup"><span data-stu-id="a7b32-195">passenger_count</span></span> | <span data-ttu-id="a7b32-196">tip_amount</span><span class="sxs-lookup"><span data-stu-id="a7b32-196">tip_amount</span></span> | <span data-ttu-id="a7b32-197">Formabontó</span><span class="sxs-lookup"><span data-stu-id="a7b32-197">tipped</span></span> |
| --- | --- | --- | --- |
|        <span data-ttu-id="a7b32-198">13.5</span><span class="sxs-lookup"><span data-stu-id="a7b32-198">13.5</span></span> |<span data-ttu-id="a7b32-199">1.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-199">1.0</span></span> |<span data-ttu-id="a7b32-200">2.9</span><span class="sxs-lookup"><span data-stu-id="a7b32-200">2.9</span></span> |<span data-ttu-id="a7b32-201">1.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-201">1.0</span></span> |
|        <span data-ttu-id="a7b32-202">16.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-202">16.0</span></span> |<span data-ttu-id="a7b32-203">2.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-203">2.0</span></span> |<span data-ttu-id="a7b32-204">3.4</span><span class="sxs-lookup"><span data-stu-id="a7b32-204">3.4</span></span> |<span data-ttu-id="a7b32-205">1.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-205">1.0</span></span> |
|        <span data-ttu-id="a7b32-206">10.5</span><span class="sxs-lookup"><span data-stu-id="a7b32-206">10.5</span></span> |<span data-ttu-id="a7b32-207">2.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-207">2.0</span></span> |<span data-ttu-id="a7b32-208">1.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-208">1.0</span></span> |<span data-ttu-id="a7b32-209">1.0</span><span class="sxs-lookup"><span data-stu-id="a7b32-209">1.0</span></span> |

## <a name="data-exploration-and-visualization"></a><span data-ttu-id="a7b32-210">Az adatok feltárása és -megjelenítésre</span><span class="sxs-lookup"><span data-stu-id="a7b32-210">Data exploration and visualization</span></span>
<span data-ttu-id="a7b32-211">Hello adatok beolvasása a Spark, miután hello hello Adattudomány folyamat következő lépése toogain bemutatják hello az adatok feltárása és -megjelenítésre.</span><span class="sxs-lookup"><span data-stu-id="a7b32-211">After you bring hello data into Spark, hello next step in hello Data Science process is toogain a deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="a7b32-212">Ebben a szakaszban hello taxi adatokat az SQL-lekérdezések használatával ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="a7b32-212">In this section, you examine hello taxi data by using SQL queries.</span></span> <span data-ttu-id="a7b32-213">Ezt követően importálása hello eredményeit azokat egy adatok keret tooplot hello cél változók és a visual hálózatfelügyeleti potenciális funkcióit az Jupyter hello automatikus-képi megjelenítés szolgáltatásával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-213">Then, import hello results into a data frame tooplot hello target variables and prospective features for visual inspection by using hello auto-visualization feature of Jupyter.</span></span>

### <a name="use-local-and-sql-magic-tooplot-data"></a><span data-ttu-id="a7b32-214">Helyi és az SQL magic tooplot adatainak használata</span><span class="sxs-lookup"><span data-stu-id="a7b32-214">Use local and SQL magic tooplot data</span></span>
<span data-ttu-id="a7b32-215">Alapértelmezés szerint hello bármely kódrészletet, amely futtatja a Jupyter notebook eredménye hello környezetben hello munkamenet fennállásának hello munkavégző csomóponton belül érhető el.</span><span class="sxs-lookup"><span data-stu-id="a7b32-215">By default, hello output of any code snippet that you run from a Jupyter notebook is available within hello context of hello session that is persisted on hello worker nodes.</span></span> <span data-ttu-id="a7b32-216">Ha toosave egy út toohello munkavégző csomópontokhoz minden számításhoz, és minden hello adatokat, amelyekre szüksége van a számítási áll rendelkezésre helyi kiszolgáló-csomóponton hello Jupyter (amely hello átjárócsomópont), ha használható hello `%%local` magic toorun hello kódot részlet hello Jupyter kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="a7b32-216">If you want toosave a trip toohello worker nodes for every computation, and if all hello data that you need for your computation is available locally on hello Jupyter server node (which is hello head node), you can use hello `%%local` magic toorun hello code snippet on hello Jupyter server.</span></span>

* <span data-ttu-id="a7b32-217">**SQL magic** (`%%sql`).</span><span class="sxs-lookup"><span data-stu-id="a7b32-217">**SQL magic** (`%%sql`).</span></span> <span data-ttu-id="a7b32-218">HDInsight Spark kernel hello az SQLContext könnyen beágyazott HiveQL lekérdezéseket támogatja.</span><span class="sxs-lookup"><span data-stu-id="a7b32-218">hello HDInsight Spark kernel supports easy inline HiveQL queries against SQLContext.</span></span> <span data-ttu-id="a7b32-219">hello (`-o VARIABLE_NAME`) argumentum hello SQL-lekérdezés kimenetét hello hello Jupyter kiszolgálón Pandas adatok keretként továbbra is fennáll..</span><span class="sxs-lookup"><span data-stu-id="a7b32-219">hello (`-o VARIABLE_NAME`) argument persists hello output of hello SQL query as a Pandas data frame on hello Jupyter server.</span></span> <span data-ttu-id="a7b32-220">Ez azt jelenti, hogy lesz hello helyi módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="a7b32-220">This means it'll be available in hello local mode.</span></span>
* <span data-ttu-id="a7b32-221">`%%local`**magic**.</span><span class="sxs-lookup"><span data-stu-id="a7b32-221">`%%local` **magic**.</span></span> <span data-ttu-id="a7b32-222">Hello `%%local` magic hello kód futtatása helyben hello Jupyter kiszolgálón, amely hello hello HDInsight-fürt átjárócsomópontjához.</span><span class="sxs-lookup"><span data-stu-id="a7b32-222">hello `%%local` magic runs hello code locally on hello Jupyter server, which is hello head node of hello HDInsight cluster.</span></span> <span data-ttu-id="a7b32-223">Általában akkor használják `%%local` magic hello együtt `%%sql` a hello magic `-o` paraméter.</span><span class="sxs-lookup"><span data-stu-id="a7b32-223">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with hello `-o` parameter.</span></span> <span data-ttu-id="a7b32-224">Hello `-o` paraméter szeretné megőrizni a hello kimeneti helyileg, a hello SQL lekérdezést, majd `%%local` magic kiváltották hello következő készlete kód részlet toorun helyileg hello kimeneti hello SQL lekérdezések helyileg fennállásának ellen.</span><span class="sxs-lookup"><span data-stu-id="a7b32-224">hello `-o` parameter would persist hello output of hello SQL query locally, and then `%%local` magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally.</span></span>

### <a name="query-hello-data-by-using-sql"></a><span data-ttu-id="a7b32-225">Hello adatait SQL használatával</span><span class="sxs-lookup"><span data-stu-id="a7b32-225">Query hello data by using SQL</span></span>
<span data-ttu-id="a7b32-226">Ez a lekérdezés lekéri a jegy ára, utas száma, és tipp által hello taxi való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="a7b32-226">This query retrieves hello taxi trips by fare amount, passenger count, and tip amount.</span></span>

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

<span data-ttu-id="a7b32-227">A következő kód hello, hello `%%local` magic létrehozza a helyi adatok, sqlResults.</span><span class="sxs-lookup"><span data-stu-id="a7b32-227">In hello following code, hello `%%local` magic creates a local data frame, sqlResults.</span></span> <span data-ttu-id="a7b32-228">SqlResults tooplot matplotlib segítségével is használhatók.</span><span class="sxs-lookup"><span data-stu-id="a7b32-228">You can use sqlResults tooplot by using matplotlib.</span></span>

> [!TIP]
> <span data-ttu-id="a7b32-229">Helyi magic ebben a cikkben több alkalommal van használva.</span><span class="sxs-lookup"><span data-stu-id="a7b32-229">Local magic is used multiple times in this article.</span></span> <span data-ttu-id="a7b32-230">Ha az adatkészlet túl nagy, adjon minta toocreate elfér a helyi memória adatok keret.</span><span class="sxs-lookup"><span data-stu-id="a7b32-230">If your data set is large, please sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

### <a name="plot-hello-data"></a><span data-ttu-id="a7b32-231">Hello adatok ábrázolása</span><span class="sxs-lookup"><span data-stu-id="a7b32-231">Plot hello data</span></span>
<span data-ttu-id="a7b32-232">Dolgozunk is után hello adatok keret helyi környezetben Pandas adatok keretként Python kód használatával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-232">You can plot by using Python code after hello data frame is in local context as a Pandas data frame.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 <span data-ttu-id="a7b32-233">hello Spark kernel automatikusan visualizes hello (HiveQL) az SQL-lekérdezések eredményének, hello kód futtatása után.</span><span class="sxs-lookup"><span data-stu-id="a7b32-233">hello Spark kernel automatically visualizes hello output of SQL (HiveQL) queries after you run hello code.</span></span> <span data-ttu-id="a7b32-234">Számos különböző típusú megjelenítések közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a7b32-234">You can choose between several types of visualizations:</span></span>

* <span data-ttu-id="a7b32-235">Tábla</span><span class="sxs-lookup"><span data-stu-id="a7b32-235">Table</span></span>
* <span data-ttu-id="a7b32-236">Torta</span><span class="sxs-lookup"><span data-stu-id="a7b32-236">Pie</span></span>
* <span data-ttu-id="a7b32-237">Vonal</span><span class="sxs-lookup"><span data-stu-id="a7b32-237">Line</span></span>
* <span data-ttu-id="a7b32-238">Terület</span><span class="sxs-lookup"><span data-stu-id="a7b32-238">Area</span></span>
* <span data-ttu-id="a7b32-239">Sáv</span><span class="sxs-lookup"><span data-stu-id="a7b32-239">Bar</span></span>

<span data-ttu-id="a7b32-240">Itt az hello kód tooplot hello adatai:</span><span class="sxs-lookup"><span data-stu-id="a7b32-240">Here's hello code tooplot hello data:</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


<span data-ttu-id="a7b32-241">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-241">**Output:**</span></span>

![Tipp összeg hisztogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tipp jegy ára mennyiséggel összeg](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a><span data-ttu-id="a7b32-245">Funkciók létrehozása és átalakítási szolgáltatások és funkciók modellezési a bemeneti adatok majd előkészítése</span><span class="sxs-lookup"><span data-stu-id="a7b32-245">Create features and transform features, and then prep data for input into modeling functions</span></span>
<span data-ttu-id="a7b32-246">Fa-alapú modellezési funkciók Spark ML és MLlib meg kell tooprepare cél és a szolgáltatások módszerek, például a dobozolás indexelő, egy közbeni kódolás vagy vectorization használatával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-246">For tree-based modeling functions from Spark ML and MLlib, you have tooprepare target and features by using a variety of techniques, such as binning, indexing, one-hot encoding, and vectorization.</span></span> <span data-ttu-id="a7b32-247">Ebben a szakaszban az alábbiakban hello eljárások toofollow:</span><span class="sxs-lookup"><span data-stu-id="a7b32-247">Here are hello procedures toofollow in this section:</span></span>

1. <span data-ttu-id="a7b32-248">Hozzon létre egy új szolgáltatás által **dobozolás** üzemideje (óra) a forgalom gyűjtők idő.</span><span class="sxs-lookup"><span data-stu-id="a7b32-248">Create a new feature by **binning** hours into traffic time buckets.</span></span>
2. <span data-ttu-id="a7b32-249">Alkalmazása **indexelő és egy közbeni kódolás** toocategorical szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-249">Apply **indexing and one-hot encoding** toocategorical features.</span></span>
3. <span data-ttu-id="a7b32-250">**A minta és a megosztott adatkészlet hello** tanítási és tesztelési törtek be.</span><span class="sxs-lookup"><span data-stu-id="a7b32-250">**Sample and split hello data set** into training and test fractions.</span></span>
4. <span data-ttu-id="a7b32-251">**Adja meg a képzés változó és a szolgáltatások**, és majd létre indexelt vagy egy közbeni kódolású betanítása és rugalmas bemeneti címkézett pont tesztelés elosztott adatkészletek (RDDs) vagy adatkeretek.</span><span class="sxs-lookup"><span data-stu-id="a7b32-251">**Specify training variable and features**, and then create indexed or one-hot encoded training and testing input labeled point resilient distributed datasets (RDDs) or data frames.</span></span>
5. <span data-ttu-id="a7b32-252">Automatikusan **kategorizálását és szolgáltatásait és célok vectorize** toouse machine learning modellek bemeneteként.</span><span class="sxs-lookup"><span data-stu-id="a7b32-252">Automatically **categorize and vectorize features and targets** toouse as inputs for machine learning models.</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="a7b32-253">Hozzon létre egy új szolgáltatás a forgalom idő gyűjtők dobozolás óránként</span><span class="sxs-lookup"><span data-stu-id="a7b32-253">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="a7b32-254">Ez a kód bemutatja, hogyan toocreate egy új szolgáltatás által óra dobozolás forgalom idő az időszakok, és hogyan toocache hello eredményül kapott adatok keret a memóriában.</span><span class="sxs-lookup"><span data-stu-id="a7b32-254">This code shows you how toocreate a new feature by binning hours into traffic time buckets and how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="a7b32-255">Amennyiben RDDs és az adatok keretek ismételten használnak, tooimproved végrehajtásának lassúságát gyorsítótárazás vezet.</span><span class="sxs-lookup"><span data-stu-id="a7b32-255">Where RDDs and data frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="a7b32-256">Ennek megfelelően RDDs és az adatok keretek lesz gyorsítótár a következő eljárások hello több szakaszaiban.</span><span class="sxs-lookup"><span data-stu-id="a7b32-256">Accordingly, you'll cache RDDs and data frames at several stages in hello following procedures.</span></span>

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE hello DATA FRAME IN MEMORY AND MATERIALIZE hello DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a><span data-ttu-id="a7b32-257">Indexelő és egy közbeni kódolás kategorikus funkciók</span><span class="sxs-lookup"><span data-stu-id="a7b32-257">Indexing and one-hot encoding of categorical features</span></span>
<span data-ttu-id="a7b32-258">hello modellezési, és előre jelezni MLlib feladatai szükséges a bemeneti adatok kategorikus toobe funkcióinak indexelt, vagy korábbi toouse kódolású.</span><span class="sxs-lookup"><span data-stu-id="a7b32-258">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="a7b32-259">Ez a szakasz bemutatja, hogyan tooindex vagy funkciók modellezési hello a bemenő kategorikus funkciói kódolása.</span><span class="sxs-lookup"><span data-stu-id="a7b32-259">This section shows you how tooindex or encode categorical features for input into hello modeling functions.</span></span>

<span data-ttu-id="a7b32-260">A modellek kódolása hello modelltől függően különböző módon vagy tooindex szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7b32-260">You need tooindex or encode your models in different ways, depending on hello model.</span></span> <span data-ttu-id="a7b32-261">Logisztikai és lineáris regressziós modellt megkövetelni például, egy közbeni kódolást.</span><span class="sxs-lookup"><span data-stu-id="a7b32-261">For example, logistic and linear regression models require one-hot encoding.</span></span> <span data-ttu-id="a7b32-262">Például három kategóriába szolgáltatás három funkció oszlopokba bővíthetők.</span><span class="sxs-lookup"><span data-stu-id="a7b32-262">For example, a feature with three categories can be expanded into three feature columns.</span></span> <span data-ttu-id="a7b32-263">Egyes oszlopok 0 vagy 1 attól függően, hogy egy megfigyelési hello kategória tartalmazna.</span><span class="sxs-lookup"><span data-stu-id="a7b32-263">Each column would contain 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="a7b32-264">MLlib biztosít hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) egy közbeni kódolási funkció.</span><span class="sxs-lookup"><span data-stu-id="a7b32-264">MLlib provides hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function for one-hot encoding.</span></span> <span data-ttu-id="a7b32-265">A kódoló leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="a7b32-265">This encoder maps a column of label indices tooa column of binary vectors with at most a single one-value.</span></span> <span data-ttu-id="a7b32-266">A kódolás numerikus fontos funkciók, például logisztikai regresszió várt algoritmusokat lehet alkalmazott toocategorical szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a7b32-266">With this encoding, algorithms that expect numerical valued features, such as logistic regression, can be applied toocategorical features.</span></span>

<span data-ttu-id="a7b32-267">Itt csak négy változók tooshow példák, amelyek karakterláncok alakít át.</span><span class="sxs-lookup"><span data-stu-id="a7b32-267">Here you transform only four variables tooshow examples, which are character strings.</span></span> <span data-ttu-id="a7b32-268">Más változók, például a hét napja, numerikus érték, mint kategorikus változók által képviselt is indexelheti.</span><span class="sxs-lookup"><span data-stu-id="a7b32-268">You also can index other variables, such as weekday, represented by numerical values, as categorical variables.</span></span>

<span data-ttu-id="a7b32-269">Az indexelő, használja `StringIndexer()`, és egy közbeni kódolását, használja a `OneHotEncoder()` MLlib funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-269">For indexing, use `StringIndexer()`, and for one-hot encoding, use `OneHotEncoder()` functions from MLlib.</span></span> <span data-ttu-id="a7b32-270">Itt található kód tooindex hello és kódolása kategorikus szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="a7b32-270">Here is hello code tooindex and encode categorical features:</span></span>

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-271">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-271">**Output:**</span></span>

<span data-ttu-id="a7b32-272">Idő toorun hello cella: 4 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-272">Time toorun hello cell: 4 seconds.</span></span>

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a><span data-ttu-id="a7b32-273">A minta és a megosztott adatkészlet hello a tanítási és tesztelési törtek</span><span class="sxs-lookup"><span data-stu-id="a7b32-273">Sample and split hello data set into training and test fractions</span></span>
<span data-ttu-id="a7b32-274">Ez a kód hoz létre egy véletlenszerű mintavételi hello adatok (25 %, ebben a példában).</span><span class="sxs-lookup"><span data-stu-id="a7b32-274">This code creates a random sampling of hello data (25%, in this example).</span></span> <span data-ttu-id="a7b32-275">Mintavételi, de nem szükséges ehhez a példához hello adatkészlet toohello mérete miatt hello a cikk bemutatja, hogyan lehet mintát a megállapításához, hogy hogyan toouse azt a saját problémákat, amikor szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7b32-275">Although sampling is not required for this example due toohello size of hello data set, hello article shows you how you can sample so that you know how toouse it for your own problems when needed.</span></span> <span data-ttu-id="a7b32-276">Nagy minták esetén ez jelentős időt takaríthat meg a modellek betanítása közben.</span><span class="sxs-lookup"><span data-stu-id="a7b32-276">When samples are large, this can save significant time while you train models.</span></span> <span data-ttu-id="a7b32-277">A következő hello minta felosztása tanítási része (ebben a példában a 75 %) és egy tesztelési része (25 %, ebben a példában) toouse besorolási és regressziós modellezéshez hatékony.</span><span class="sxs-lookup"><span data-stu-id="a7b32-277">Next, split hello sample into a training part (75%, in this example) and a testing part (25%, in this example) toouse in classification and regression modeling.</span></span>

<span data-ttu-id="a7b32-278">Egy sort ad hozzá (0 és 1) közé eső véletlenszerű szám tooeach (a "VÉL" oszlop), amelyek betanítás során használt tooselect kereszt-ellenőrzési modellrészt lehetnek.</span><span class="sxs-lookup"><span data-stu-id="a7b32-278">Add a random number (between 0 and 1) tooeach row (in a "rand" column) that can be used tooselect cross-validation folds during training.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT hello SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-279">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-279">**Output:**</span></span>

<span data-ttu-id="a7b32-280">Idő toorun hello cella: 2 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-280">Time toorun hello cell: 2 seconds.</span></span>

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a><span data-ttu-id="a7b32-281">Adja meg a képzés változó és a szolgáltatások, és hozza létre indexelt vagy egy közbeni kódolású a modell betanítására és tesztelésére bemeneti pont RDDs vagy adatok keretek címkével</span><span class="sxs-lookup"><span data-stu-id="a7b32-281">Specify training variable and features, and then create indexed or one-hot encoded training and testing input labeled point RDDs or data frames</span></span>
<span data-ttu-id="a7b32-282">Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adatait, mivel a címkézett adattípus mutasson, és így tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell használhatja kódolása kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a7b32-282">This section contains code that shows you how tooindex categorical text data as a labeled point data type, and encode it so you can use it tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="a7b32-283">Címkézett pont objektum RDDs formázott oly módon, hogy a legtöbb gépi tanulási algoritmusok MLlib a bemeneti adatként van szükség.</span><span class="sxs-lookup"><span data-stu-id="a7b32-283">Labeled point objects are RDDs that are formatted in a way that is needed as input data by most of machine learning algorithms in MLlib.</span></span> <span data-ttu-id="a7b32-284">A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.</span><span class="sxs-lookup"><span data-stu-id="a7b32-284">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="a7b32-285">Ez a kód hello (függő) célváltozó és hello szolgáltatások toouse tootrain modellek kell megadni.</span><span class="sxs-lookup"><span data-stu-id="a7b32-285">In this code, you specify hello target (dependent) variable and hello features toouse tootrain models.</span></span> <span data-ttu-id="a7b32-286">Ezután létrehoz indexelt vagy egy közbeni kódolású a modell betanítására és tesztelésére bemeneti pont RDDs vagy adatok keretek címkével.</span><span class="sxs-lookup"><span data-stu-id="a7b32-286">Then, you create indexed or one-hot encoded training and testing input labeled point RDDs or data frames.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY hello TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-287">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-287">**Output:**</span></span>

<span data-ttu-id="a7b32-288">Idő toorun hello cella: 4 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-288">Time toorun hello cell: 4 seconds.</span></span>

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a><span data-ttu-id="a7b32-289">Automatikusan kategorizálását és szolgáltatásait és célok toouse vectorize machine learning modellek bemeneteként</span><span class="sxs-lookup"><span data-stu-id="a7b32-289">Automatically categorize and vectorize features and targets toouse as inputs for machine learning models</span></span>
<span data-ttu-id="a7b32-290">Fa-alapú modellezési funkciók használata Spark ML toocategorize hello cél és a szolgáltatások toouse.</span><span class="sxs-lookup"><span data-stu-id="a7b32-290">Use Spark ML toocategorize hello target and features toouse in tree-based modeling functions.</span></span> <span data-ttu-id="a7b32-291">hello kód két feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="a7b32-291">hello code completes two tasks:</span></span>

* <span data-ttu-id="a7b32-292">Egy bináris osztályozás cél értéke csak 0 vagy 1 tooeach adatpont 0 és 1 közötti hozzárendelése egy küszöbértéket 0,5 használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7b32-292">Creates a binary target for classification by assigning a value of 0 or 1 tooeach data point between 0 and 1 by using a threshold value of 0.5.</span></span>
* <span data-ttu-id="a7b32-293">Automatikusan kategorizálja szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-293">Automatically categorizes features.</span></span> <span data-ttu-id="a7b32-294">Ha az összes olyan szolgáltatás különböző numerikus értékeket hello száma kisebb, mint 32, adott szolgáltatás kategorizálta.</span><span class="sxs-lookup"><span data-stu-id="a7b32-294">If hello number of distinct numerical values for any feature is less than 32, that feature is categorized.</span></span>

<span data-ttu-id="a7b32-295">Ez a két feladatok hello kódja.</span><span class="sxs-lookup"><span data-stu-id="a7b32-295">Here's hello code for these two tasks.</span></span>

    # CATEGORIZE FEATURES AND BINARIZE hello TARGET FOR hello BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR hello REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a><span data-ttu-id="a7b32-296">Bináris osztályozási modell: előre jelezni, hogy kell fordítani tipp:</span><span class="sxs-lookup"><span data-stu-id="a7b32-296">Binary classification model: Predict whether a tip should be paid</span></span>
<span data-ttu-id="a7b32-297">Ebben a szakaszban hozzon létre három típusú-e tipp kell fordítani a bináris osztályozási modellek toopredict:</span><span class="sxs-lookup"><span data-stu-id="a7b32-297">In this section, you create three types of binary classification models toopredict whether or not a tip should be paid:</span></span>

* <span data-ttu-id="a7b32-298">A **logisztikai regresszió modell** hello Spark ML használatával `LogisticRegression()` függvény</span><span class="sxs-lookup"><span data-stu-id="a7b32-298">A **logistic regression model** by using hello Spark ML `LogisticRegression()` function</span></span>
* <span data-ttu-id="a7b32-299">A **véletlenszerű erdő besorolási modell** hello Spark ML használatával `RandomForestClassifier()` függvény</span><span class="sxs-lookup"><span data-stu-id="a7b32-299">A **random forest classification model** by using hello Spark ML `RandomForestClassifier()` function</span></span>
* <span data-ttu-id="a7b32-300">A **átmenetes kiemelési fa besorolási modell** hello MLlib használatával `GradientBoostedTrees()` függvény</span><span class="sxs-lookup"><span data-stu-id="a7b32-300">A **gradient boosting tree classification model** by using hello MLlib `GradientBoostedTrees()` function</span></span>

### <a name="create-a-logistic-regression-model"></a><span data-ttu-id="a7b32-301">Logisztikai regressziós modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7b32-301">Create a logistic regression model</span></span>
<span data-ttu-id="a7b32-302">Ezután hozzon létre egy logisztikai regresszió modell hello Spark ML használatával `LogisticRegression()` függvény.</span><span class="sxs-lookup"><span data-stu-id="a7b32-302">Next, create a logistic regression model by using hello Spark ML `LogisticRegression()` function.</span></span> <span data-ttu-id="a7b32-303">A lépések egy sorozatát kód felépítése hello modell hoz létre:</span><span class="sxs-lookup"><span data-stu-id="a7b32-303">You create hello model building code in a series of steps:</span></span>

1. <span data-ttu-id="a7b32-304">**Hello tanítási** adatok egy paraméter-készlettel.</span><span class="sxs-lookup"><span data-stu-id="a7b32-304">**Train hello model** data with one parameter set.</span></span>
2. <span data-ttu-id="a7b32-305">**Hello modell kiértékelése** a metrikák a teszt adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="a7b32-305">**Evaluate hello model** on a test data set with metrics.</span></span>
3. <span data-ttu-id="a7b32-306">**Hello modell mentése** Blob Storage a jövőbeni felhasználásához.</span><span class="sxs-lookup"><span data-stu-id="a7b32-306">**Save hello model** in Blob storage for future consumption.</span></span>
4. <span data-ttu-id="a7b32-307">**Pontszám hello modell** vizsgálati adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="a7b32-307">**Score hello model** against test data.</span></span>
5. <span data-ttu-id="a7b32-308">**Hello eredmények ábrázolhatók** a jellemző (ROC) görbék működő fogadó.</span><span class="sxs-lookup"><span data-stu-id="a7b32-308">**Plot hello results** with receiver operating characteristic (ROC) curves.</span></span>

<span data-ttu-id="a7b32-309">Ezek az eljárások hello kódját a következő:</span><span class="sxs-lookup"><span data-stu-id="a7b32-309">Here's hello code for these procedures:</span></span>

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON hello TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE hello MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

<span data-ttu-id="a7b32-310">Betöltése, pontozása és hello-eredményeket menteni.</span><span class="sxs-lookup"><span data-stu-id="a7b32-310">Load, score, and save hello results.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD hello SAVED MODEL AND SCORE hello TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON hello TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC RESULTS
    println("ROC on test data = " + ROC)


<span data-ttu-id="a7b32-311">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-311">**Output:**</span></span>

<span data-ttu-id="a7b32-312">A tesztadatokat ROC = 0.9827381497557599</span><span class="sxs-lookup"><span data-stu-id="a7b32-312">ROC on test data = 0.9827381497557599</span></span>

<span data-ttu-id="a7b32-313">Helyi Pandas adatok keretek tooplot hello: ROC-görbe Python használja.</span><span class="sxs-lookup"><span data-stu-id="a7b32-313">Use Python on local Pandas data frames tooplot hello ROC curve.</span></span>

    # QUERY hello RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT hello ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT hello ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


<span data-ttu-id="a7b32-314">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-314">**Output:**</span></span>

![Tipp vagy nincs tipp: ROC-görbe](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a><span data-ttu-id="a7b32-316">Hozzon létre egy véletlenszerű besorolás erdőmodell</span><span class="sxs-lookup"><span data-stu-id="a7b32-316">Create a random forest classification model</span></span>
<span data-ttu-id="a7b32-317">Ezután hozzon létre egy véletlenszerű erdő besorolási modell hello Spark ML használatával `RandomForestClassifier()` működik, és értékelje ki hello modell a tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-317">Next, create a random forest classification model by using hello Spark ML `RandomForestClassifier()` function, and then evaluate hello model on test data.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE hello RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT hello MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE hello MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


<span data-ttu-id="a7b32-318">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-318">**Output:**</span></span>

<span data-ttu-id="a7b32-319">A tesztadatokat ROC = 0.9847103571552683</span><span class="sxs-lookup"><span data-stu-id="a7b32-319">ROC on test data = 0.9847103571552683</span></span>

### <a name="create-a-gbt-classification-model"></a><span data-ttu-id="a7b32-320">GBT besorolási modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7b32-320">Create a GBT classification model</span></span>
<span data-ttu-id="a7b32-321">Ezután hozzon létre egy GBT besorolási modell MLlib tartozó használatával `GradientBoostedTrees()` működik, és értékelje ki hello modell a tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-321">Next, create a GBT classification model by using MLlib's `GradientBoostedTrees()` function, and then evaluate hello model on test data.</span></span>

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN hello MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE hello MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE hello MODEL ON TEST INSTANCES AND hello COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS tooEVALUATE hello MODEL ON hello TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


<span data-ttu-id="a7b32-322">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-322">**Output:**</span></span>

<span data-ttu-id="a7b32-323">: ROC-görbe alatt: 0.9846895479241554</span><span class="sxs-lookup"><span data-stu-id="a7b32-323">Area under ROC curve: 0.9846895479241554</span></span>

## <a name="regression-model-predict-tip-amount"></a><span data-ttu-id="a7b32-324">Regressziós modell: tipp összeg előrejelzése</span><span class="sxs-lookup"><span data-stu-id="a7b32-324">Regression model: Predict tip amount</span></span>
<span data-ttu-id="a7b32-325">Ebben a szakaszban hozzon létre két típusú regressziós modell toopredict hello tipp összeg:</span><span class="sxs-lookup"><span data-stu-id="a7b32-325">In this section, you create two types of regression models toopredict hello tip amount:</span></span>

* <span data-ttu-id="a7b32-326">A **rendeződik lineáris regressziós modell** hello Spark ML használatával `LinearRegression()` függvény.</span><span class="sxs-lookup"><span data-stu-id="a7b32-326">A **regularized linear regression model** by using hello Spark ML `LinearRegression()` function.</span></span> <span data-ttu-id="a7b32-327">Hello modell mentése lesz, és értékelje ki a hello modell a tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-327">You'll save hello model and evaluate hello model on test data.</span></span>
* <span data-ttu-id="a7b32-328">A **színátmenetes kiemelése fa regressziós modell** hello Spark ML használatával `GBTRegressor()` függvény.</span><span class="sxs-lookup"><span data-stu-id="a7b32-328">A **gradient-boosting tree regression model** by using hello Spark ML `GBTRegressor()` function.</span></span>

### <a name="create-a-regularized-linear-regression-model"></a><span data-ttu-id="a7b32-329">Rendeződik lineáris regressziós modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7b32-329">Create a regularized linear regression model</span></span>
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING hello SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT hello MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE hello MODEL OVER hello TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE hello MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT hello COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-330">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-330">**Output:**</span></span>

<span data-ttu-id="a7b32-331">Idő toorun hello cella: 13 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-331">Time toorun hello cell: 13 seconds.</span></span>

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("R-sqr on test data = " + r2)


<span data-ttu-id="a7b32-332">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-332">**Output:**</span></span>

<span data-ttu-id="a7b32-333">R-sqr a tesztadatokat = 0.5960320470835743</span><span class="sxs-lookup"><span data-stu-id="a7b32-333">R-sqr on test data = 0.5960320470835743</span></span>

<span data-ttu-id="a7b32-334">A következő lekérdezés hello a vizsgálati eredmények adatok keret és AutoVizWidget és matplotlib toovisualize használja azt.</span><span class="sxs-lookup"><span data-stu-id="a7b32-334">Next, query hello test results as a data frame and use AutoVizWidget and matplotlib toovisualize it.</span></span>

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

<span data-ttu-id="a7b32-335">hello kód létrehoz egy helyi adatok keret hello lekérdezés kimeneti és tevékenységtérkép hello adatok.</span><span class="sxs-lookup"><span data-stu-id="a7b32-335">hello code creates a local data frame from hello query output and plots hello data.</span></span> <span data-ttu-id="a7b32-336">Hello `%%local` magic létrehoz egy helyi adatok keret `sqlResults`, mely tooplot matplotlib használható.</span><span class="sxs-lookup"><span data-stu-id="a7b32-336">hello `%%local` magic creates a local data frame, `sqlResults`, which you can use tooplot with matplotlib.</span></span>

> [!NOTE]
> <span data-ttu-id="a7b32-337">A Spark magic ebben a cikkben több alkalommal van használva.</span><span class="sxs-lookup"><span data-stu-id="a7b32-337">This Spark magic is used multiple times in this article.</span></span> <span data-ttu-id="a7b32-338">Nagy adatmennyiség hello esetén meg kell minta toocreate elfér a helyi memória adatok keret.</span><span class="sxs-lookup"><span data-stu-id="a7b32-338">If hello amount of data is large, you should sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

<span data-ttu-id="a7b32-339">Hozzon létre előkészítésére Python matplotlib.</span><span class="sxs-lookup"><span data-stu-id="a7b32-339">Create plots by using Python matplotlib.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT hello RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

<span data-ttu-id="a7b32-340">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-340">**Output:**</span></span>

![Tipp összeg: tényleges és előre jelzett](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a><span data-ttu-id="a7b32-342">GBT regressziós modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7b32-342">Create a GBT regression model</span></span>
<span data-ttu-id="a7b32-343">Hozzon létre egy GBT regressziós modell hello Spark ML `GBTRegressor()` működik, és értékelje ki hello modell a tesztadatokat.</span><span class="sxs-lookup"><span data-stu-id="a7b32-343">Create a GBT regression model by using hello Spark ML `GBTRegressor()` function, and then evaluate hello model on test data.</span></span>

<span data-ttu-id="a7b32-344">[Fák színátmenetes súlyozott](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák.</span><span class="sxs-lookup"><span data-stu-id="a7b32-344">[Gradient-boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="a7b32-345">GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt.</span><span class="sxs-lookup"><span data-stu-id="a7b32-345">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="a7b32-346">Használhat GBTs regressziós és besorolás.</span><span class="sxs-lookup"><span data-stu-id="a7b32-346">You can use GBTs for regression and classification.</span></span> <span data-ttu-id="a7b32-347">Azok kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés funkció és nonlinearities és a szolgáltatás kapcsolati.</span><span class="sxs-lookup"><span data-stu-id="a7b32-347">They can handle categorical features, do not require feature scaling, and can capture nonlinearities and feature interactions.</span></span> <span data-ttu-id="a7b32-348">Is használhatja őket a multiclass-besorolás beállításban.</span><span class="sxs-lookup"><span data-stu-id="a7b32-348">You also can use them in a multiclass-classification setting.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="a7b32-349">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-349">**Output:**</span></span>

<span data-ttu-id="a7b32-350">Teszt R-sqr van: 0.7655383534596654</span><span class="sxs-lookup"><span data-stu-id="a7b32-350">Test R-sqr is: 0.7655383534596654</span></span>

## <a name="advanced-modeling-utilities-for-optimization"></a><span data-ttu-id="a7b32-351">Speciális modellezési segédprogramok energiaoptimalizálás</span><span class="sxs-lookup"><span data-stu-id="a7b32-351">Advanced modeling utilities for optimization</span></span>
<span data-ttu-id="a7b32-352">Ebben a szakaszban használhatja a machine learning segédprogramok használó fejlesztők gyakran modell optimalizálásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="a7b32-352">In this section, you use machine learning utilities that developers frequently use for model optimization.</span></span> <span data-ttu-id="a7b32-353">Pontosabban optimalizálhatja a machine learning modellek három különböző módon paraméter abszolút és kereszt-ellenőrzési használatával:</span><span class="sxs-lookup"><span data-stu-id="a7b32-353">Specifically, you can optimize machine learning models three different ways by using parameter sweeping and cross-validation:</span></span>

* <span data-ttu-id="a7b32-354">Hello adatok felosztása tanítási és érvényesítési beállítása hello modell használatával hyper paraméter abszolút egy gyakorlókészlethez optimalizálása és értékelődik ki az érvényesítési készletének (lineáris regresszió)</span><span class="sxs-lookup"><span data-stu-id="a7b32-354">Split hello data into train and validation sets, optimize hello model by using hyper-parameter sweeping on a training set, and evaluate on a validation set (linear regression)</span></span>
* <span data-ttu-id="a7b32-355">Hello modell optimalizálhatja a kereszt-ellenőrzési és hyper paraméter abszolút Spark ML CrossValidator függvény (bináris osztályozás) használatával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-355">Optimize hello model by using cross-validation and hyper-parameter sweeping by using Spark ML's CrossValidator function (binary classification)</span></span>
* <span data-ttu-id="a7b32-356">Gépi tanulás funkció és paraméter beállítása (lineáris regresszió) használatával egyéni kereszt-ellenőrzési és paraméter abszolút kód toouse hello modell optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-356">Optimize hello model by using custom cross-validation and parameter-sweeping code toouse any machine learning function and parameter set (linear regression)</span></span>

<span data-ttu-id="a7b32-357">**Kereszt-ellenőrzési** egy technika, amely értékeli a milyen mértékben a modellek betanítása adatokat egy ismert csoportján fog generalize toopredict hello részeit, amelyen az még nincs betanítva adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="a7b32-357">**Cross-validation** is a technique that assesses how well a model trained on a known set of data will generalize toopredict hello features of data sets on which it has not been trained.</span></span> <span data-ttu-id="a7b32-358">hello általános ezzel a technikával mögött lényege, hogy egy modell betanítása ismert adatok a megfelelő adatokat, és majd hello az előrejelzés pontosságát lett tesztelve egy független adatkészlet ellen.</span><span class="sxs-lookup"><span data-stu-id="a7b32-358">hello general idea behind this technique is that a model is trained on a data set of known data, and then hello accuracy of its predictions is tested against an independent data set.</span></span> <span data-ttu-id="a7b32-359">A közös megvalósítása toodivide az adatkészlet *k*-modellrészt, és ezután a ciklikus multiplexeléssel a egy hello modellrészt hello modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="a7b32-359">A common implementation is toodivide a data set into *k*-folds, and then train hello model in a round-robin fashion on all but one of hello folds.</span></span>

<span data-ttu-id="a7b32-360">**Hyper-paraméter optimalizálási** hello probléma lépett fel a hyper-paraméterek készletét a tanulási algoritmus, általában a hello célja egy független adatkészlet hello algoritmus teljesítményének biztosítása optimalizálása kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a7b32-360">**Hyper-parameter optimization** is hello problem of choosing a set of hyper-parameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="a7b32-361">A hyper-paraméter értéke kívül hello modell betanítási eljárást kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="a7b32-361">A hyper-parameter is a value that you must specify outside hello model training procedure.</span></span> <span data-ttu-id="a7b32-362">A hyper-paraméterértékek feltételezéseket hatással lehet a hello rugalmasságot és hello modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="a7b32-362">Assumptions about hyper-parameter values can affect hello flexibility and accuracy of hello model.</span></span> <span data-ttu-id="a7b32-363">Döntési fák például rendelkezik hyper-paraméterek, például a hello szükséges mélységében és hello fában leaves száma.</span><span class="sxs-lookup"><span data-stu-id="a7b32-363">Decision trees have hyper-parameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="a7b32-364">Meg kell adni egy téves besorolás szövegminősítési kifejezés támogatási vektoros gépek (SVM).</span><span class="sxs-lookup"><span data-stu-id="a7b32-364">You must set a misclassification penalty term for a support vector machine (SVM).</span></span>

<span data-ttu-id="a7b32-365">A közös módon tooperform hyper paraméter optimalizálása toouse rács keresés – más néven egy **paraméter ismétlés**.</span><span class="sxs-lookup"><span data-stu-id="a7b32-365">A common way tooperform hyper-parameter optimization is toouse a grid search, also called a **parameter sweep**.</span></span> <span data-ttu-id="a7b32-366">A rács keresés a részletes keresést hello értékek hello hyper paraméter terület tanulási algoritmus megadott alhálózatát keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="a7b32-366">In a grid search, an exhaustive search is performed through hello values of a specified subset of hello hyper-parameter space for a learning algorithm.</span></span> <span data-ttu-id="a7b32-367">Kereszt-ellenőrzési megadhatja a teljesítmény metrika toosort hello optimális eredmények hello rács keresési algoritmus által előállított ki.</span><span class="sxs-lookup"><span data-stu-id="a7b32-367">Cross-validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="a7b32-368">Korlát problémák tootraining modelladatok overfitting például segíthet a kereszt-ellenőrzési hyper paraméter abszolút használatakor.</span><span class="sxs-lookup"><span data-stu-id="a7b32-368">If you use cross-validation hyper-parameter sweeping, you can help limit problems like overfitting a model tootraining data.</span></span> <span data-ttu-id="a7b32-369">Ezzel a módszerrel hello modell olyan hello kapacitás tooapply toohello általános adatok készletét, mely hello betanítási adatok ki lett olvasni.</span><span class="sxs-lookup"><span data-stu-id="a7b32-369">This way, hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a><span data-ttu-id="a7b32-370">A hyper-paraméter abszolút egy lineáris regressziós modellt optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-370">Optimize a linear regression model with hyper-parameter sweeping</span></span>
<span data-ttu-id="a7b32-371">A következő adatok felosztása tanítási és érvényesítési beállítása, használja a hyper-paraméter egy képzési abszolút toooptimize hello modell beállítva, és értékelje érvényesítési megfelelő (lineáris regresszió).</span><span class="sxs-lookup"><span data-stu-id="a7b32-371">Next, split data into train and validation sets, use hyper-parameter sweeping on a training set toooptimize hello model, and evaluate on a validation set (linear regression).</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE hello ESTIMATOR FUNCTION: `hello LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE hello PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET), AND THEN hello SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="a7b32-372">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-372">**Output:**</span></span>

<span data-ttu-id="a7b32-373">Teszt R-sqr van: 0.6226484708501209</span><span class="sxs-lookup"><span data-stu-id="a7b32-373">Test R-sqr is: 0.6226484708501209</span></span>

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a><span data-ttu-id="a7b32-374">A kereszt-ellenőrzési és a hyper-paraméter abszolút hello bináris osztályozási modell optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-374">Optimize hello binary classification model by using cross-validation and hyper-parameter sweeping</span></span>
<span data-ttu-id="a7b32-375">Ez a szakasz bemutatja, hogyan toooptimize bináris osztályozás modell kereszt-ellenőrzési és a hyper-paraméter abszolút használatával.</span><span class="sxs-lookup"><span data-stu-id="a7b32-375">This section shows you how toooptimize a binary classification model by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="a7b32-376">Ez a Spark ML hello használja `CrossValidator` függvény.</span><span class="sxs-lookup"><span data-stu-id="a7b32-376">This uses hello Spark ML `CrossValidator` function.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS tooUSE WITH hello TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE hello ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY hello NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE hello TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE hello TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="a7b32-377">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-377">**Output:**</span></span>

<span data-ttu-id="a7b32-378">Idő toorun hello cella: 33 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-378">Time toorun hello cell: 33 seconds.</span></span>

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a><span data-ttu-id="a7b32-379">Egyéni kereszt-ellenőrzési és paraméter abszolút kód használatával hello lineáris regressziós modell optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a7b32-379">Optimize hello linear regression model by using custom cross-validation and parameter-sweeping code</span></span>
<span data-ttu-id="a7b32-380">A következő hello modell optimalizálása egyéni kód használatával, és hello legjobb Modellparaméterek azonosíthatja a legmagasabb pontossági hello feltétel.</span><span class="sxs-lookup"><span data-stu-id="a7b32-380">Next, optimize hello model by using custom code, and identify hello best model parameters by using hello criterion of highest accuracy.</span></span> <span data-ttu-id="a7b32-381">Ezután hozzon létre hello végső modell, hello modell a tesztadatokat kiértékeléséhez és hello modell mentése a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a7b32-381">Then, create hello final model, evaluate hello model on test data, and save hello model in Blob storage.</span></span> <span data-ttu-id="a7b32-382">Végezetül hello modell betöltése, vizsgálati adatok pontozása és értékeléséhez pontosságát.</span><span class="sxs-lookup"><span data-stu-id="a7b32-382">Finally, load hello model, score test data, and evaluate accuracy.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello PARAMETER GRID AND hello NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY hello NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND hello PARAMETER GRID tooGET AND IDENTIFY hello BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 too(nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 too(numModels-1)) {
            for (nParams <- 0 too(numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET hello BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 too(numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE hello BEST MODEL WITH hello BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE hello BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON hello TRAINING SET WITH hello BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


    # LOAD hello MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST hello MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


<span data-ttu-id="a7b32-383">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="a7b32-383">**Output:**</span></span>

<span data-ttu-id="a7b32-384">Idő toorun hello cella: 61 másodperc.</span><span class="sxs-lookup"><span data-stu-id="a7b32-384">Time toorun hello cell: 61 seconds.</span></span>

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a><span data-ttu-id="a7b32-385">Spark-beépített machine learning modellek automatikusan Scala felhasználása</span><span class="sxs-lookup"><span data-stu-id="a7b32-385">Consume Spark-built machine learning models automatically with Scala</span></span>
<span data-ttu-id="a7b32-386">Témakörök, amelyek végigvezetik Önt az Azure-ban hello Adattudomány folyamat alkotó hello feladatok áttekintéséhez lásd: [Team adatok tudományos folyamat](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="a7b32-386">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="a7b32-387">[Vonja össze az adatokat tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) ismerteti más végpont forgatókönyvek, amelyek hello hello Team adatok tudományos folyamat az adott forgatókönyveket szükséges lépések bemutatása.</span><span class="sxs-lookup"><span data-stu-id="a7b32-387">[Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) describes other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios.</span></span> <span data-ttu-id="a7b32-388">hello forgatókönyvek is bemutatják, hogyan toocombine felhőalapú és helyszíni eszközök és szolgáltatások a munkafolyamatok és folyamat toocreate intelligens kérelmet.</span><span class="sxs-lookup"><span data-stu-id="a7b32-388">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>

<span data-ttu-id="a7b32-389">[Spark-beépített gépi tanulási modell pontozása](machine-learning-data-science-spark-model-consumption.md) bemutatja, hogyan toouse Scala kód tooautomatically betölteni, és új adatkészletek pontozása gépi tanulási modellek Spark a beépített és az Azure Blob storage mentve.</span><span class="sxs-lookup"><span data-stu-id="a7b32-389">[Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) shows you how toouse Scala code tooautomatically load and score new data sets with machine learning models built in Spark and saved in Azure Blob storage.</span></span> <span data-ttu-id="a7b32-390">Nincs megadott hello utasításokat, és egyszerűen cserélje le hello Python kódját Scala kódra ebben a cikkben a automatizált felhasználásához.</span><span class="sxs-lookup"><span data-stu-id="a7b32-390">You can follow hello instructions provided there, and simply replace hello Python code with Scala code in this article for automated consumption.</span></span>

