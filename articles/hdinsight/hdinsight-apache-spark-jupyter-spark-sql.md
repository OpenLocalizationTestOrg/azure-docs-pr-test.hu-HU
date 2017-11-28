---
title: "Apache Spark-fürt létrehozása az Azure HDInsightban | Microsoft Docs"
description: "HDInsight Spark rövid útmutató az Apache Spark-fürtök HDInsightban történő létrehozásáról."
keywords: "spark gyorsútmutató,interaktív spark,interaktív lekérdezés,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: ad4330a1fc7f8de154d9aaa8df3acc2ab59b9dc1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="63f7d-104">Apache Spark-fürt létrehozása az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="63f7d-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="63f7d-105">Ebből a cikkből megtudhatja, hogyan hozható létre egy Apache Spark-fürt az Azure HDInsightban.</span><span class="sxs-lookup"><span data-stu-id="63f7d-105">In this article, you learn how to create an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="63f7d-106">A Spark on HDInsight további információiért lásd: [Áttekintés: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="63f7d-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="63f7d-107">![Gyorsútmutató-diagram, amely leírja az Apache Spark-fürtök Azure HDInsight rendszeren való létrehozásának lépéseit](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark gyorsútmutató az Apache Spark használatához a HDInsight rendszeren. Szemléltetett lépések: fürt létrehozása; interaktív Spark-lekérdezés futtatása")</span><span class="sxs-lookup"><span data-stu-id="63f7d-107">![Quickstart diagram describing steps to create an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63f7d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="63f7d-108">Prerequisites</span></span>

* <span data-ttu-id="63f7d-109">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="63f7d-109">**An Azure subscription**.</span></span> <span data-ttu-id="63f7d-110">Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="63f7d-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="63f7d-111">Lásd: [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="63f7d-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="63f7d-112">HDInsight Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="63f7d-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="63f7d-113">Ebben a szakaszban egy HDInsight Spark-fürtöt hozhat létre egy [Azure Resource Manager-sablonnal](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="63f7d-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="63f7d-114">Egyéb fürtlétrehozási módszerek: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="63f7d-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="63f7d-115">Az alábbi képre kattintva megnyithatja a sablont az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="63f7d-115">Click the following image to open the template in the Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. <span data-ttu-id="63f7d-116">Írja be a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="63f7d-116">Enter the following values:</span></span>

    <span data-ttu-id="63f7d-117">![HDInsight Spark-fürt létrehozása egy Azure Resource Manager-sablonnal](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-fürt létrehozása a HDInsightban egy Azure Resource Manager-sablonnal")</span><span class="sxs-lookup"><span data-stu-id="63f7d-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="63f7d-118">**Előfizetés**: Válassza ki a fürthöz használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="63f7d-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="63f7d-119">**Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="63f7d-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="63f7d-120">Az erőforráscsoport kezeli a projektek Azure-erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="63f7d-120">Resource group is used to manage Azure resources for your projects.</span></span>
    * <span data-ttu-id="63f7d-121">**Hely**: Válasszon egy helyet az erőforráscsoportnak.</span><span class="sxs-lookup"><span data-stu-id="63f7d-121">**Location**: Select a location for the resource group.</span></span> <span data-ttu-id="63f7d-122">A sablon ezt a helyet használja a fürt létrehozásához, valamint az alapértelmezett fürttárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="63f7d-122">The template uses this location for creating the cluster as well as for the default cluster storage.</span></span>
    * <span data-ttu-id="63f7d-123">**ClusterName**: Adjon nevet a létrehozni kívánt HDInsight-fürtnek.</span><span class="sxs-lookup"><span data-stu-id="63f7d-123">**ClusterName**: Enter a name for the HDInsight cluster that you want to create.</span></span>
    * <span data-ttu-id="63f7d-124">**Spark-verzió**Válassza ki a **2.0**-s verziót a fürtön telepíteni kívánt verzióként.</span><span class="sxs-lookup"><span data-stu-id="63f7d-124">**Spark version**: Select **2.0** as the version that you want to install on the cluster.</span></span>
    * <span data-ttu-id="63f7d-125">**A fürt bejelentkezési neve és jelszava**: Az alapértelmezett bejelentkezési név az admin.</span><span class="sxs-lookup"><span data-stu-id="63f7d-125">**Cluster login name and password**: The default login name is admin.</span></span>
    * <span data-ttu-id="63f7d-126">**SSH-felhasználónév és -jelszó**.</span><span class="sxs-lookup"><span data-stu-id="63f7d-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="63f7d-127">Jegyezze fel ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="63f7d-127">Write down these values.</span></span>  <span data-ttu-id="63f7d-128">Az oktatóanyag későbbi részében szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="63f7d-128">You need them later in the tutorial.</span></span>

3. <span data-ttu-id="63f7d-129">Jelölje be az **Elfogadom a fenti feltételeket** és a **Rögzítés az irányítópulton** lehetőséget, majd kattintson a **Vásárlás** elemre.</span><span class="sxs-lookup"><span data-stu-id="63f7d-129">Select **I agree to the terms and conditions stated above**, select **Pin to dashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="63f7d-130">Ekkor egy új csempe jelenik meg Submitting deployment for Template deployment (Üzemelő példány elküldése sablon üzemelő példányhoz) címmel.</span><span class="sxs-lookup"><span data-stu-id="63f7d-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="63f7d-131">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="63f7d-131">It takes about 20 minutes to create the cluster.</span></span>

<span data-ttu-id="63f7d-132">Ha problémába ütközik a HDInsight-fürtök létrehozása során, előfordulhat, hogy nem rendelkezik a szükséges engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="63f7d-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have the right permissions to do so.</span></span> <span data-ttu-id="63f7d-133">További információért tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="63f7d-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="63f7d-134">A cikkben leírt eljárás létrehoz egy Spark-fürtöt, amely [Azure Storage-blobokat használ fürttárolóként](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="63f7d-134">This article creates a Spark cluster that uses [Azure Storage Blobs as the cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="63f7d-135">Olyan Spark-fürtöt is létrehozhat, amely az [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md)-t használja alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="63f7d-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as the default storage.</span></span> <span data-ttu-id="63f7d-136">Útmutatás: [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (HDInsight-fürt létrehozása a Data Lake Store-ral).</span><span class="sxs-lookup"><span data-stu-id="63f7d-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="63f7d-137">Hive-lekérdezés futtatása a Spark SQL-lel</span><span class="sxs-lookup"><span data-stu-id="63f7d-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="63f7d-138">Ha a HDInsight Spark-fürthöz konfigurált Jupyter notebookot használja, egy előre beállított `sqlContext` elemet kap, amelyet Hive-lekérdezések Spark SQL-lel végzett futtatásához használhat.</span><span class="sxs-lookup"><span data-stu-id="63f7d-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use to run Hive queries using Spark SQL.</span></span> <span data-ttu-id="63f7d-139">Ebben a szakaszban megtudhatja, hogyan indíthat egy Jupyter notebookot, és ezután hogyan futtathat egy alapszintű Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="63f7d-139">In this section, you learn how to start a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="63f7d-140">Nyissa meg az [Azure portált](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="63f7d-140">Open the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="63f7d-141">Ha rögzítette a fürtöt az irányítópulton, a fürt paneljének megnyitásához kattintson a fürt csempéjére az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="63f7d-141">If you opted to pin the cluster to the dashboard, click the cluster tile from the dashboard to launch the cluster blade.</span></span>

    <span data-ttu-id="63f7d-142">Ha nem rögzítette a fürtöt az irányítópulton, a bal oldali panelen kattintson a **HDInsight-fürtök** elemre, majd a létrehozott fürtre.</span><span class="sxs-lookup"><span data-stu-id="63f7d-142">If you did not pin the cluster to the dashboard, from the left pane, click **HDInsight clusters**, and then click the cluster you created.</span></span>

3. <span data-ttu-id="63f7d-143">A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="63f7d-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="63f7d-144">Ha a rendszer felkéri rá, adja meg a fürthöz tartozó rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="63f7d-144">If prompted, enter the admin credentials for the cluster.</span></span>

   <span data-ttu-id="63f7d-145">![A Jupyter notebook megnyitása interaktív Spark SQL-lekérdezés futtatásához](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "A Jupyter notebook megnyitása interaktív Spark SQL-lekérdezés futtatásához")</span><span class="sxs-lookup"><span data-stu-id="63f7d-145">![Open Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="63f7d-146">A fürthöz tartozó Jupyter notebookot az alábbi URL-cím böngészőben történő megnyitásával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="63f7d-146">You may also access the Jupyter notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="63f7d-147">Cserélje le a **CLUSTERNAME** elemet a fürt nevére:</span><span class="sxs-lookup"><span data-stu-id="63f7d-147">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="63f7d-148">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="63f7d-148">Create a notebook.</span></span> <span data-ttu-id="63f7d-149">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="63f7d-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="63f7d-150">![Jupyter notebook létrehozása interaktív Spark SQL-lekérdezés futtatásához](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Jupyter notebook létrehozása interaktív Spark SQL-lekérdezés futtatásához")</span><span class="sxs-lookup"><span data-stu-id="63f7d-150">![Create a Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")</span></span>

   <span data-ttu-id="63f7d-151">Az új notebook létrejött, és Untitled(Untitled.pynb) néven nyílt meg.</span><span class="sxs-lookup"><span data-stu-id="63f7d-151">A new notebook is created and opened with the name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="63f7d-152">Ha a felső részen a notebook nevére kattint, megadhat egy könnyen megjegyezhető nevet.</span><span class="sxs-lookup"><span data-stu-id="63f7d-152">Click the notebook name at the top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="63f7d-153">![A Jupyter notebook elnevezése, amelyből interaktív Spark lekérdezést futtat](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "A Jupyter notebook elnevezése, amelyből interaktív Spark lekérdezést futtat")</span><span class="sxs-lookup"><span data-stu-id="63f7d-153">![Provide a name for the Jupter notebook to run interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")</span></span>

5.  <span data-ttu-id="63f7d-154">Illessze be a következő kódot egy üres cellába, majd nyomja le a **SHIFT + ENTER** billentyűkombinációt annak futtatásához.</span><span class="sxs-lookup"><span data-stu-id="63f7d-154">Paste the following code in an empty cell, and then press **SHIFT + ENTER** to run the code.</span></span> <span data-ttu-id="63f7d-155">Az `%%sql` alatti (sql magic nevű) kód megadja a Jupyter notebook számára, hogy az előre beállított `sqlContext` elemet használja a Hive-lekérdezés futtatásához.</span><span class="sxs-lookup"><span data-stu-id="63f7d-155">In the code below `%%sql` (called the sql magic) tells Jupyter notebook to use the preset `sqlContext` to run the Hive query.</span></span> <span data-ttu-id="63f7d-156">A lekérdezés lekérdezi az első 10 sort egy Hive-táblából (**hivesampletable**), amely alapértelmezés szerint minden HDInsight-fürtben elérhető.</span><span class="sxs-lookup"><span data-stu-id="63f7d-156">The query retrieves the top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="63f7d-157">![Hive-lekérdezés a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-lekérdezés a HDInsight Sparkban")</span><span class="sxs-lookup"><span data-stu-id="63f7d-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="63f7d-158">Az `%%sql` magicről és az előre beállított környezetekről a [HDInsight-fürtökhöz elérhető Jupyter-kerneleket ismertető cikkben](hdinsight-apache-spark-jupyter-notebook-kernels.md) talál további információt.</span><span class="sxs-lookup"><span data-stu-id="63f7d-158">For more information on the `%%sql` magic and the preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="63f7d-159">Minden alkalommal, amikor a Jupyterben lekérdezést futtat, a webböngésző ablakának címsorában **(Foglalt)** állapot jelenik meg a notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="63f7d-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="63f7d-160">A jobb felső sarokban lévő **PySpark** felirat mellett ekkor egy teli kör is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="63f7d-160">You also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="63f7d-161">A feladat befejezése után ez a jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="63f7d-161">After the job is completed, it changes to a hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="63f7d-162">A képernyő frissül, és megjeleníti a lekérdezés kimenetelét.</span><span class="sxs-lookup"><span data-stu-id="63f7d-162">The screen should refresh to show the query output.</span></span>

    <span data-ttu-id="63f7d-163">![Hive-lekérdezés kimenete a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-lekérdezés kimenete a HDInsight Sparkban")</span><span class="sxs-lookup"><span data-stu-id="63f7d-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="63f7d-164">Az alkalmazás futtatása után állítsa le a notebookot a fürt erőforrásainak felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="63f7d-164">Shut down the notebook to release the cluster resources after you have finished running the application.</span></span> <span data-ttu-id="63f7d-165">Ehhez a notebook **File** (Fájl) menüjében kattintson a **Close and Halt** (Bezárás és leállítás) elemre.</span><span class="sxs-lookup"><span data-stu-id="63f7d-165">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="63f7d-166">Ha később tervezi végrehajtani a következő lépéseket, törölje a jelen cikkben létrehozott HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="63f7d-166">If you plan to complete the next steps at a later time, make sure you delete the HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="63f7d-167">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="63f7d-167">Next step</span></span> 

<span data-ttu-id="63f7d-168">Ebből a cikkből megtudhatta, hogyan hozható létre egy HDInsight Spark-fürt és hogyan futtatható egy alapszintű Spark SQL-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="63f7d-168">In this article you learned how to create an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="63f7d-169">Folytassa a következő cikkel, amelyben megtudhatja, hogyan használhatja a HDInsight Spark-fürtöt interaktív lekérdezések mintaadatokon való futtatására.</span><span class="sxs-lookup"><span data-stu-id="63f7d-169">Advance to the next article to learn how to use an HDInsight Spark cluster to run interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="63f7d-170">Interaktív lekérdezések futtatása HDInsight Spark-fürtön</span><span class="sxs-lookup"><span data-stu-id="63f7d-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



