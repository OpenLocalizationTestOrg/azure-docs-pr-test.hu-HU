---
title: "aaaCreate az Apache Spark on Azure hdinsight fürt |} Microsoft Docs"
description: "Hogyan toocreate az Apache Spark on hdinsight fürt a HDInsight Spark gyorsindítási."
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
ms.openlocfilehash: 002f71b3cd4fb315d4a556cebc9263026515ec4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="d30b5-104">Apache Spark-fürt létrehozása az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="d30b5-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="d30b5-105">Ebből a cikkből megismerheti, hogyan toocreate az Apache Spark on Azure hdinsight fürt.</span><span class="sxs-lookup"><span data-stu-id="d30b5-105">In this article, you learn how toocreate an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="d30b5-106">A Spark on HDInsight további információiért lásd: [Áttekintés: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d30b5-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="d30b5-107">![Gyors üzembe helyezés diagram lépéseket toocreate Apache Spark-fürt leíró on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark gyors üzembe helyezés, Apache Spark on HDInsight használatával. Szemléltetett lépések: fürt létrehozása; interaktív Spark-lekérdezés futtatása")</span><span class="sxs-lookup"><span data-stu-id="d30b5-107">![Quickstart diagram describing steps toocreate an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d30b5-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d30b5-108">Prerequisites</span></span>

* <span data-ttu-id="d30b5-109">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="d30b5-109">**An Azure subscription**.</span></span> <span data-ttu-id="d30b5-110">Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="d30b5-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="d30b5-111">Lásd: [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="d30b5-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="d30b5-112">HDInsight Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d30b5-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="d30b5-113">Ebben a szakaszban egy HDInsight Spark-fürtöt hozhat létre egy [Azure Resource Manager-sablonnal](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="d30b5-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="d30b5-114">Egyéb fürtlétrehozási módszerek: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="d30b5-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="d30b5-115">Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="d30b5-115">Click hello following image tooopen hello template in hello Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="d30b5-116">Adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="d30b5-116">Enter hello following values:</span></span>

    <span data-ttu-id="d30b5-117">![HDInsight Spark-fürt létrehozása egy Azure Resource Manager-sablonnal](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-fürt létrehozása a HDInsightban egy Azure Resource Manager-sablonnal")</span><span class="sxs-lookup"><span data-stu-id="d30b5-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="d30b5-118">**Előfizetés**: Válassza ki a fürthöz használni kívánt Azure-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="d30b5-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="d30b5-119">**Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="d30b5-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="d30b5-120">Erőforráscsoport van használt toomanage a projektek Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d30b5-120">Resource group is used toomanage Azure resources for your projects.</span></span>
    * <span data-ttu-id="d30b5-121">**Hely**: hello erőforráscsoport helyének kiválasztására.</span><span class="sxs-lookup"><span data-stu-id="d30b5-121">**Location**: Select a location for hello resource group.</span></span> <span data-ttu-id="d30b5-122">hello sablon hello fürt létrehozásának, valamint mint hello alapértelmezett fürttároló ezen a helyen használja.</span><span class="sxs-lookup"><span data-stu-id="d30b5-122">hello template uses this location for creating hello cluster as well as for hello default cluster storage.</span></span>
    * <span data-ttu-id="d30b5-123">**ClusterName**: Adjon meg egy nevet, amelyet az toocreate hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d30b5-123">**ClusterName**: Enter a name for hello HDInsight cluster that you want toocreate.</span></span>
    * <span data-ttu-id="d30b5-124">**Spark-verziójú**: válasszon **2.0** , amelyet az hello fürtön tooinstall hello verzióként.</span><span class="sxs-lookup"><span data-stu-id="d30b5-124">**Spark version**: Select **2.0** as hello version that you want tooinstall on hello cluster.</span></span>
    * <span data-ttu-id="d30b5-125">**A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az admin.</span><span class="sxs-lookup"><span data-stu-id="d30b5-125">**Cluster login name and password**: hello default login name is admin.</span></span>
    * <span data-ttu-id="d30b5-126">**SSH-felhasználónév és -jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d30b5-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="d30b5-127">Jegyezze fel ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d30b5-127">Write down these values.</span></span>  <span data-ttu-id="d30b5-128">Már szükség hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d30b5-128">You need them later in hello tutorial.</span></span>

3. <span data-ttu-id="d30b5-129">Válassza ki **toohello feltételek és kikötések fenti elfogadom**, jelölje be **PIN-kód toodashboard**, és kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="d30b5-129">Select **I agree toohello terms and conditions stated above**, select **Pin toodashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="d30b5-130">Ekkor egy új csempe jelenik meg Submitting deployment for Template deployment (Üzemelő példány elküldése sablon üzemelő példányhoz) címmel.</span><span class="sxs-lookup"><span data-stu-id="d30b5-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="d30b5-131">Körülbelül 20 percet toocreate hello fürt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d30b5-131">It takes about 20 minutes toocreate hello cluster.</span></span>

<span data-ttu-id="d30b5-132">Ha a HDInsight-fürtök létrehozása problémát tapasztal, lehet, hogy nem rendelkezik megfelelő engedélyekkel toodo hello így.</span><span class="sxs-lookup"><span data-stu-id="d30b5-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have hello right permissions toodo so.</span></span> <span data-ttu-id="d30b5-133">További információért tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="d30b5-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="d30b5-134">Ebben a cikkben létrehoz egy Spark-fürt által használt [Azure Storage Blobs, hello tárolási fürt](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="d30b5-134">This article creates a Spark cluster that uses [Azure Storage Blobs as hello cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="d30b5-135">Létrehozhat egy Spark-fürt által használt [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) hello alapértelmezett tárolóként.</span><span class="sxs-lookup"><span data-stu-id="d30b5-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as hello default storage.</span></span> <span data-ttu-id="d30b5-136">Útmutatás: [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (HDInsight-fürt létrehozása a Data Lake Store-ral).</span><span class="sxs-lookup"><span data-stu-id="d30b5-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="d30b5-137">Hive-lekérdezés futtatása a Spark SQL-lel</span><span class="sxs-lookup"><span data-stu-id="d30b5-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="d30b5-138">A HDInsight Spark-fürt konfigurált Jupyter notebook használatakor egy előre definiált kap `sqlContext` használható toorun Hive-lekérdezések Spark SQL használatával.</span><span class="sxs-lookup"><span data-stu-id="d30b5-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use toorun Hive queries using Spark SQL.</span></span> <span data-ttu-id="d30b5-139">Ebben a szakaszban megismerheti, hogyan toostart Jupyter notebook, majd futtassa az alapszintű Hive-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="d30b5-139">In this section, you learn how toostart a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="d30b5-140">Nyissa meg hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d30b5-140">Open hello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="d30b5-141">Ha toopin hello fürt toohello irányítópult választotta, kattintson a hello fürt csempe hello irányítópult toolaunch hello fürt paneljén.</span><span class="sxs-lookup"><span data-stu-id="d30b5-141">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="d30b5-142">Ha nem volt rögzíti hello fürt toohello irányítópult, hello bal oldali ablaktáblában kattintson **a HDInsight-fürtök**, majd kattintson a létrehozott hello fürt.</span><span class="sxs-lookup"><span data-stu-id="d30b5-142">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="d30b5-143">A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre.</span><span class="sxs-lookup"><span data-stu-id="d30b5-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="d30b5-144">Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d30b5-144">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="d30b5-145">![Nyissa meg Jupyter notebook toorun interaktív Spark SQL-lekérdezés](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "nyitott Jupyter notebook toorun interaktív Spark SQL-lekérdezés")</span><span class="sxs-lookup"><span data-stu-id="d30b5-145">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="d30b5-146">A fürt URL-címet a böngészőben a következő megnyitásakor hello által hello Jupyter notebook is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="d30b5-146">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="d30b5-147">Cserélje le **CLUSTERNAME** hello néven a fürt:</span><span class="sxs-lookup"><span data-stu-id="d30b5-147">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="d30b5-148">Hozzon létre egy notebookot.</span><span class="sxs-lookup"><span data-stu-id="d30b5-148">Create a notebook.</span></span> <span data-ttu-id="d30b5-149">Kattintson a **New** (Új), majd a **PySpark** elemre.</span><span class="sxs-lookup"><span data-stu-id="d30b5-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="d30b5-150">![A Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "a Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d30b5-150">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="d30b5-151">Új notebook létrejött, és hello nevű Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="d30b5-151">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="d30b5-152">Hello notebook neve hello tetején kattintson, és adja meg egy rövid nevet, ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="d30b5-152">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="d30b5-153">![Adjon meg egy nevet hello Jupter notebook toorun interaktív Spark-lekérdezést](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "adjon meg egy nevet hello Jupter notebook toorun interaktív Spark lekérdezése")</span><span class="sxs-lookup"><span data-stu-id="d30b5-153">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5.  <span data-ttu-id="d30b5-154">Beillesztés hello következő kód egy üres cellába, és nyomja le az **SHIFT + ENTER** toorun hello kódot.</span><span class="sxs-lookup"><span data-stu-id="d30b5-154">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="d30b5-155">Az alábbi kód hello `%%sql` (hívott hello sql magic) közli a Jupyter notebook toouse hello beállított `sqlContext` toorun hello Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="d30b5-155">In hello code below `%%sql` (called hello sql magic) tells Jupyter notebook toouse hello preset `sqlContext` toorun hello Hive query.</span></span> <span data-ttu-id="d30b5-156">hello lekérdezés hello első 10 sorok egy táblából struktúra (**hivesampletable**), amely alapértelmezés szerint mindig elérhető az összes HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="d30b5-156">hello query retrieves hello top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="d30b5-157">![Hive-lekérdezés a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-lekérdezés a HDInsight Sparkban")</span><span class="sxs-lookup"><span data-stu-id="d30b5-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="d30b5-158">További információ a hello `%%sql` magic és hello beállított környezeteket, lásd: [Jupyter kernelek a HDInsight-fürtök rendelkezésre](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="d30b5-158">For more information on hello `%%sql` magic and hello preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d30b5-159">Minden alkalommal, amikor lekérdezést futtat a Jupyter, a webböngésző ablakának címsorában látható egy **(foglalt)** állapot hello notebook neve mellett.</span><span class="sxs-lookup"><span data-stu-id="d30b5-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="d30b5-160">Egy teli kör következő toohello is látni **PySpark** hello jobb felső sarokban lévő szöveg.</span><span class="sxs-lookup"><span data-stu-id="d30b5-160">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="d30b5-161">Hello feladat befejezése után tooa jel üres körre változik.</span><span class="sxs-lookup"><span data-stu-id="d30b5-161">After hello job is completed, it changes tooa hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="d30b5-162">üdvözlő képernyőt tooshow hello lekérdezés kimeneti kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="d30b5-162">hello screen should refresh tooshow hello query output.</span></span>

    <span data-ttu-id="d30b5-163">![Hive-lekérdezés kimenete a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-lekérdezés kimenete a HDInsight Sparkban")</span><span class="sxs-lookup"><span data-stu-id="d30b5-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="d30b5-164">Hello notebook toorelease hello fürterőforrások leállítása hello alkalmazást futtató befejezése után.</span><span class="sxs-lookup"><span data-stu-id="d30b5-164">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="d30b5-165">toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.</span><span class="sxs-lookup"><span data-stu-id="d30b5-165">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="d30b5-166">Ha toocomplete hello lépések egy későbbi időpontban, ellenőrizze, hogy ez a cikk létrehozott hello HDInsight-fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="d30b5-166">If you plan toocomplete hello next steps at a later time, make sure you delete hello HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="d30b5-167">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="d30b5-167">Next step</span></span> 

<span data-ttu-id="d30b5-168">Ebben a cikkben megtanulta, hogyan toocreate egy HDInsight Spark fürt és a Futtatás alapvető Spark SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="d30b5-168">In this article you learned how toocreate an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="d30b5-169">Előzetes toohello hogyan toouse egy HDInsight Spark fürt tovább cikk toolearn toorun interaktív lekérdezések a mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="d30b5-169">Advance toohello next article toolearn how toouse an HDInsight Spark cluster toorun interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="d30b5-170">Interaktív lekérdezések futtatása HDInsight Spark-fürtön</span><span class="sxs-lookup"><span data-stu-id="d30b5-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



