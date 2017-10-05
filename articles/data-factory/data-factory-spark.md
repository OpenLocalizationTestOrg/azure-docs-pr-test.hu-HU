---
title: "Az Azure Data Factory Spark programok meghívása |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet meghívni egy az Azure data factory használatával a MapReduce művelethez Spark programok."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 57894bbdd9208f8c32eb65e29f04e2ae723780ca
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a><span data-ttu-id="334e9-103">Az Azure Data Factory folyamatok Spark programok meghívása</span><span class="sxs-lookup"><span data-stu-id="334e9-103">Invoke Spark programs from Azure Data Factory pipelines</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="334e9-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-104">Hive Activity</span></span>](data-factory-hive-activity.md)
> * [<span data-ttu-id="334e9-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="334e9-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="334e9-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="334e9-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="334e9-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="334e9-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="334e9-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="334e9-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="334e9-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="334e9-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="334e9-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="334e9-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="334e9-114">Introduction</span></span>
<span data-ttu-id="334e9-115">Spark tevékenység egyike a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) Azure Data Factory által támogatott.</span><span class="sxs-lookup"><span data-stu-id="334e9-115">Spark Activity is one of the [data transformation activities](data-factory-data-transformation-activities.md) supported by Azure Data Factory.</span></span> <span data-ttu-id="334e9-116">Ez a tevékenység fut a megadott Spark program Azure hdinsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="334e9-116">This activity runs the specified Spark program on your Apache Spark cluster in Azure HDInsight.</span></span>    

> [!IMPORTANT]
> - <span data-ttu-id="334e9-117">Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.</span><span class="sxs-lookup"><span data-stu-id="334e9-117">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
> - <span data-ttu-id="334e9-118">Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürtjei (saját).</span><span class="sxs-lookup"><span data-stu-id="334e9-118">Spark Activity supports only existing (your own) HDInsight Spark clusters.</span></span> <span data-ttu-id="334e9-119">Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="334e9-119">It does not support an on-demand HDInsight linked service.</span></span>

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a><span data-ttu-id="334e9-120">Forgatókönyv: hozzon létre egy folyamatot Spark tevékenység</span><span class="sxs-lookup"><span data-stu-id="334e9-120">Walkthrough: create a pipeline with Spark activity</span></span>
<span data-ttu-id="334e9-121">A Data Factory-folyamat létrehozása egy Spark tevékenységet a szokásos lépései a következők.</span><span class="sxs-lookup"><span data-stu-id="334e9-121">Here are the typical steps to create a Data Factory pipeline with a Spark activity.</span></span>  

1. <span data-ttu-id="334e9-122">Egy adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="334e9-122">Create a data factory.</span></span>
2. <span data-ttu-id="334e9-123">Az az Azure storage társított a HDInsight Spark-fürt data factoryval való csatolásához Azure Storage társított szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="334e9-123">Create an Azure Storage linked service to link your Azure storage that is associated with your HDInsight Spark cluster to the data factory.</span></span>     
2. <span data-ttu-id="334e9-124">Az Azure HDInsight az Apache Spark-fürt összekapcsolása a data factory kapcsolt Azure HDInsight szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="334e9-124">Create an Azure HDInsight linked service to link your Apache Spark cluster in Azure HDInsight to the data factory.</span></span>
3. <span data-ttu-id="334e9-125">Hozzon létre egy adatkészlet hivatkozik az Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="334e9-125">Create a dataset that refers to the Azure Storage linked service.</span></span> <span data-ttu-id="334e9-126">Jelenleg adjon meg egy kimeneti adatkészlet a tevékenység akkor is, ha nincs folyamatban létrehozott kimenet van.</span><span class="sxs-lookup"><span data-stu-id="334e9-126">Currently, you must specify an output dataset for an activity even if there is no output being produced.</span></span>  
4. <span data-ttu-id="334e9-127">Hozzon létre egy folyamatot Spark tevékenység, amely a #2 létrehozott HDInsight kapcsolódó szolgáltatásra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="334e9-127">Create a pipeline with Spark activity that refers to the HDInsight linked service created in #2.</span></span> <span data-ttu-id="334e9-128">A tevékenység az adatkészletben, mint egy kimeneti adatkészletet az előző lépésben létrehozott van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="334e9-128">The activity is configured with the dataset you created in the previous step as an output dataset.</span></span> <span data-ttu-id="334e9-129">A kimeneti adatkészlet működik az ütemezés (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="334e9-129">The output dataset is what drives the schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="334e9-130">A kimeneti adatkészletet, ezért meg kell adnia annak ellenére, hogy a tevékenység nem eredményez valóban kimenettel.</span><span class="sxs-lookup"><span data-stu-id="334e9-130">Therefore, you must specify the output dataset even though the activity does not really produce an output.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="334e9-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="334e9-131">Prerequisites</span></span>
1. <span data-ttu-id="334e9-132">Hozzon létre egy **általános célú Azure Storage-fiók** a forgatókönyv a következő utasítással: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="334e9-132">Create a **general-purpose Azure Storage Account** by following instructions in the walkthrough: [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
2. <span data-ttu-id="334e9-133">Hozzon létre egy **Azure HDInsight az Apache Spark-fürt** által az oktatóanyag utasításai a következő: [Azure HDInsight létrehozása Apache Spark-fürt](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="334e9-133">Create an **Apache Spark cluster in Azure HDInsight** by following instructions in the tutorial: [Create Apache Spark cluster in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="334e9-134">Társítsa a #1. lépésben a fürthöz létrehozott Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="334e9-134">Associate the Azure storage account you created in step #1 with this cluster.</span></span>  
3. <span data-ttu-id="334e9-135">Töltse le, és tekintse át a python-parancsfájl **test.py** helyen található: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span><span class="sxs-lookup"><span data-stu-id="334e9-135">Download and review the python script file **test.py** located at: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span></span>  
3.  <span data-ttu-id="334e9-136">Töltse fel **test.py** számára a **pyFiles** mappájában a **adfspark** az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="334e9-136">Upload **test.py** to the **pyFiles** folder in the **adfspark** container in your Azure Blob storage.</span></span> <span data-ttu-id="334e9-137">A tároló és a mappa létrehozása, ha azok nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="334e9-137">Create the container and the folder if they do not exist.</span></span>

### <a name="create-data-factory"></a><span data-ttu-id="334e9-138">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-138">Create data factory</span></span>
<span data-ttu-id="334e9-139">Ebben a lépésben létrehozzuk a data factoryt.</span><span class="sxs-lookup"><span data-stu-id="334e9-139">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="334e9-140">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="334e9-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="334e9-141">Kattintson a **NEW** (Új) lehetőségre a bal oldali menüben, kattintson az **Data + Analytics** (Adatok + analitika) pontra, majd a **Data Factory** elemre.</span><span class="sxs-lookup"><span data-stu-id="334e9-141">Click **NEW** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="334e9-142">Az a **új adat-előállító** panelen adjon meg **SparkDF** nevét.</span><span class="sxs-lookup"><span data-stu-id="334e9-142">In the **New data factory** blade, enter **SparkDF** for the Name.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="334e9-143">Az Azure data factory nevének **globálisan egyedinek** kell lennie.</span><span class="sxs-lookup"><span data-stu-id="334e9-143">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="334e9-144">Ha a hibát látja: **nem érhető el adat-előállító "SparkDF"**.</span><span class="sxs-lookup"><span data-stu-id="334e9-144">If you see the error: **Data factory name “SparkDF” is not available**.</span></span> <span data-ttu-id="334e9-145">Az adat-előállítóban (például yournameSparkDFdate, és próbálja meg újra létrehozni nevének módosítása.</span><span class="sxs-lookup"><span data-stu-id="334e9-145">Change the name of the data factory (for example, yournameSparkDFdate, and try creating again.</span></span> <span data-ttu-id="334e9-146">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="334e9-146">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
4. <span data-ttu-id="334e9-147">Jelölje ki azt az **Azure-előfizetést**, ahol létre szeretné hozni a data factoryt.</span><span class="sxs-lookup"><span data-stu-id="334e9-147">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="334e9-148">Válasszon ki egy létező **erőforráscsoport** , vagy hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="334e9-148">Select an existing **resource group** or create an Azure resource group.</span></span>
6. <span data-ttu-id="334e9-149">Válassza ki **rögzítés az irányítópulton** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="334e9-149">Select **Pin to dashboard** option.</span></span>  
6. <span data-ttu-id="334e9-150">Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.</span><span class="sxs-lookup"><span data-stu-id="334e9-150">Click **Create** on the **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="334e9-151">Data Factory-példány létrehozásához a [Data Factory közreműködője](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör tagjának kell lennie az előfizetés/erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="334e9-151">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
7. <span data-ttu-id="334e9-152">Megjelenik a data factory létrehozása a **irányítópult** az Azure portál, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="334e9-152">You see the data factory being created in the **dashboard** of the Azure portal as follows:</span></span>   
8. <span data-ttu-id="334e9-153">A data factory sikeres létrehozása után megjelenik a data factory oldal, amely megjeleníti a data factory tartalmát.</span><span class="sxs-lookup"><span data-stu-id="334e9-153">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span> <span data-ttu-id="334e9-154">Ha nem látja a data factory lap, kattintson az irányítópulton a data factory tartozó csempére.</span><span class="sxs-lookup"><span data-stu-id="334e9-154">If you do not see the data factory page, click the tile for your data factory on the dashboard.</span></span>

    ![A Data Factory panel](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a><span data-ttu-id="334e9-156">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-156">Create linked services</span></span>
<span data-ttu-id="334e9-157">Ebben a lépésben létrehoz két társított szolgáltatások, a Spark-fürt összekapcsolása a data factory, míg a másik az Azure tárterületet összekapcsolása a data factory egyik.</span><span class="sxs-lookup"><span data-stu-id="334e9-157">In this step, you create two linked services, one to link your Spark cluster to your data factory, and the other to link your Azure storage to your data factory.</span></span>  

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="334e9-158">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-158">Create Azure Storage linked service</span></span>
<span data-ttu-id="334e9-159">Ebben a lépésben társítja az Azure Storage-fiókot a data factoryjához.</span><span class="sxs-lookup"><span data-stu-id="334e9-159">In this step, you link your Azure Storage account to your data factory.</span></span> <span data-ttu-id="334e9-160">A DataSet adatkészlet hoz létre, később a forgatókönyvben egy lépésben szolgáltatásnak hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="334e9-160">A dataset you create in a step later in this walkthrough refers to this linked service.</span></span> <span data-ttu-id="334e9-161">A HDInsight csatolt szolgáltatás a következő lépésben meghatározó túl szolgáltatásnak hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="334e9-161">The HDInsight linked service that you define in the next step refers to this linked service too.</span></span>  

1. <span data-ttu-id="334e9-162">Kattintson a **Szerző és központi telepítése** a a **adat-előállító** a data factory paneljét.</span><span class="sxs-lookup"><span data-stu-id="334e9-162">Click **Author and deploy** on the **Data Factory** blade for your data factory.</span></span> <span data-ttu-id="334e9-163">Ekkor meg kell jelennie a Data Factory Editornak.</span><span class="sxs-lookup"><span data-stu-id="334e9-163">You should see the Data Factory Editor.</span></span>
2. <span data-ttu-id="334e9-164">Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.</span><span class="sxs-lookup"><span data-stu-id="334e9-164">Click **New data store** and choose **Azure storage**.</span></span>

   ![Új adattár – Azure Storage – menü](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="334e9-166">Megjelenik a **JSON-parancsfájl** létrehozásához egy Azure Storage társított a szolgáltatás-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="334e9-166">You should see the **JSON script** for creating an Azure Storage linked service in the editor.</span></span>

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="334e9-168">Cserélje le **fióknév** és **fiókkulcs** az Azure storage-fiók nevét és a hozzáférési kulccsal.</span><span class="sxs-lookup"><span data-stu-id="334e9-168">Replace **account name** and **account key** with the name and access key of your Azure storage account.</span></span> <span data-ttu-id="334e9-169">A tárelérési kulcs lekéréséről többet is megtudhat, ha elolvassa a tárelérési kulcsok megtekintésével, másolásával és újragenerálásával kapcsolatos információkat [A tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account) című részben.</span><span class="sxs-lookup"><span data-stu-id="334e9-169">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="334e9-170">A társított szolgáltatás telepítéséhez kattintson **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="334e9-170">To deploy the linked service, click **Deploy** on the command bar.</span></span> <span data-ttu-id="334e9-171">A társított szolgáltatás sikeres üzembe helyezését követően megjelenik a **Draft-1** (Vázlat-1) ablak, amelynek bal oldalán, fanézetben látható az **AzureStorageLinkedService** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="334e9-171">After the linked service is deployed successfully, the **Draft-1** window should disappear and you see **AzureStorageLinkedService** in the tree view on the left.</span></span>

#### <a name="create-hdinsight-linked-service"></a><span data-ttu-id="334e9-172">A HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-172">Create HDInsight linked service</span></span>
<span data-ttu-id="334e9-173">Ebben a lépésben hozzon létre csatolt Azure HDInsight szolgáltatást a HDInsight Spark-fürt csatolása az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="334e9-173">In this step, you create Azure HDInsight linked service to link your HDInsight Spark cluster to the data factory.</span></span> <span data-ttu-id="334e9-174">A HDInsight-fürthöz, ez a példa a folyamatának Spark tevékenységben megadott Spark program futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="334e9-174">The HDInsight cluster is used to run the Spark program specified in the Spark activity of the pipeline in this sample.</span></span>  

1. <span data-ttu-id="334e9-175">Ha nem látja ezt a gombot, kattintson a három pontot  **További** kattintson az eszköztár **új számítási**, és kattintson a **HDInsight-fürt**.</span><span class="sxs-lookup"><span data-stu-id="334e9-175">Click **... More** on the toolbar, click **New compute**, and then click **HDInsight cluster**.</span></span>

    ![A HDInsight társított szolgáltatás létrehozása](media/data-factory-spark/new-hdinsight-linked-service.png)
2. <span data-ttu-id="334e9-177">Másolja és illessze be a következő kódrészletet a **Draft-1** (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="334e9-177">Copy and paste the following snippet to the **Draft-1** window.</span></span> <span data-ttu-id="334e9-178">A JSON-szerkesztőben tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="334e9-178">In the JSON editor, do the following steps:</span></span>
    1. <span data-ttu-id="334e9-179">Adja meg a **URI** a HDInsight Spark-fürt számára.</span><span class="sxs-lookup"><span data-stu-id="334e9-179">Specify the **URI** for the HDInsight Spark cluster.</span></span> <span data-ttu-id="334e9-180">Például: `https://<sparkclustername>.azurehdinsight.net/`.</span><span class="sxs-lookup"><span data-stu-id="334e9-180">For example: `https://<sparkclustername>.azurehdinsight.net/`.</span></span>
    2. <span data-ttu-id="334e9-181">Adja meg a nevét a **felhasználói** kik férhetnek hozzá a Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="334e9-181">Specify the name of the **user** who has access to the Spark cluster.</span></span>
    3. <span data-ttu-id="334e9-182">Adja meg a **jelszó** felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="334e9-182">Specify the **password** for user.</span></span>
    4. <span data-ttu-id="334e9-183">Adja meg a **Azure Storage társított szolgáltatásnak** társított a HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="334e9-183">Specify the **Azure Storage linked service** that is associated with the HDInsight Spark cluster.</span></span> <span data-ttu-id="334e9-184">Az ebben a példában is: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="334e9-184">In this example, it is: **AzureStorageLinkedService**.</span></span>

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - <span data-ttu-id="334e9-185">Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.</span><span class="sxs-lookup"><span data-stu-id="334e9-185">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
    > - <span data-ttu-id="334e9-186">Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürt (saját).</span><span class="sxs-lookup"><span data-stu-id="334e9-186">Spark Activity supports only existing (your own) HDInsight Spark cluster.</span></span> <span data-ttu-id="334e9-187">Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="334e9-187">It does not support an on-demand HDInsight linked service.</span></span>

    <span data-ttu-id="334e9-188">Lásd: [HDInsight társított szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) a HDInsight adatait a társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="334e9-188">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details about the HDInsight linked service.</span></span>
3.  <span data-ttu-id="334e9-189">A társított szolgáltatás telepítéséhez kattintson **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="334e9-189">To deploy the linked service, click **Deploy** on the command bar.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="334e9-190">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-190">Create output dataset</span></span>
<span data-ttu-id="334e9-191">A kimeneti adatkészlet működik az ütemezés (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="334e9-191">The output dataset is what drives the schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="334e9-192">Ezért meg kell adnia egy kimeneti adatkészletet, a spark tevékenység az adatcsatorna annak ellenére, hogy a tevékenység valóban nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="334e9-192">Therefore, you must specify an output dataset for the spark activity in the pipeline even though the activity does not really produce any output.</span></span> <span data-ttu-id="334e9-193">Egy bemeneti adatkészlet a tevékenység megadása nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="334e9-193">Specifying an input dataset for the activity is optional.</span></span>

1. <span data-ttu-id="334e9-194">A **Data Factory Editorban** kattintson ide: **... More** (... További) a parancssávon, kattintson a **New dataset** (Új adathalmaz) elemre, és válassza az **Azure Blob Storage** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="334e9-194">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="334e9-195">Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="334e9-195">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="334e9-196">A JSON-részlet nevű adatkészlet meghatározása **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="334e9-196">The JSON snippet defines a dataset called **OutputDataset**.</span></span> <span data-ttu-id="334e9-197">Emellett megadhatja, hogy az eredmények kell-e tárolni a blob-tárolóban nevű **adfspark** és a mappa neve **pyFiles/kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="334e9-197">In addition, you specify that the results are stored in the blob container called **adfspark** and the folder called **pyFiles/output**.</span></span> <span data-ttu-id="334e9-198">Amint azt korábban említettük, ez az adatkészlet egy üres adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="334e9-198">As mentioned earlier, this dataset is a dummy dataset.</span></span> <span data-ttu-id="334e9-199">Ebben a példában a Spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="334e9-199">The Spark program in this example does not produce any output.</span></span> <span data-ttu-id="334e9-200">A **rendelkezésre állási** szakaszban határozza meg, hogy a kimeneti adatkészlet naponta jön létre.</span><span class="sxs-lookup"><span data-stu-id="334e9-200">The **availability** section specifies that the output dataset is produced daily.</span></span>  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="334e9-201">A dataset telepítéséhez kattintson **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="334e9-201">To deploy the dataset, click **Deploy** on the command bar.</span></span>


### <a name="create-pipeline"></a><span data-ttu-id="334e9-202">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="334e9-202">Create pipeline</span></span>
<span data-ttu-id="334e9-203">Ebben a lépésben a folyamat létrehoz egy **HDInsightSpark** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="334e9-203">In this step, you create a pipeline with a **HDInsightSpark** activity.</span></span> <span data-ttu-id="334e9-204">Jelenleg a kimeneti adatkészlet vezérli az ütemezést, ezért kimeneti adatkészletet akkor is létre kell hoznia, ha a tevékenység nem állít elő semmilyen kimenetet.</span><span class="sxs-lookup"><span data-stu-id="334e9-204">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="334e9-205">Ha a tevékenység nem fogad semmilyen bemenetet, kihagyhatja a bemeneti adatkészlet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="334e9-205">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> <span data-ttu-id="334e9-206">Ezért bemeneti adatkészlet ebben a példában van megadva.</span><span class="sxs-lookup"><span data-stu-id="334e9-206">Therefore, no input dataset is specified in this example.</span></span>

1. <span data-ttu-id="334e9-207">Az a **Data Factory Editor**, kattintson a **... További** a parancssávon, majd **új adatcsatorna**.</span><span class="sxs-lookup"><span data-stu-id="334e9-207">In the **Data Factory Editor**, click **… More** on the command bar, and then click **New pipeline**.</span></span>
2. <span data-ttu-id="334e9-208">Cserélje le a parancsfájlt a Vázlat-1 ablakban az alábbi parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="334e9-208">Replace the script in the Draft-1 window with the following script:</span></span>

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    <span data-ttu-id="334e9-209">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="334e9-209">Note the following points:</span></span>
    - <span data-ttu-id="334e9-210">A **típus** tulajdonsága **HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="334e9-210">The **type** property is set to **HDInsightSpark**.</span></span>
    - <span data-ttu-id="334e9-211">A **rootPath** értéke **adfspark\\pyFiles** ahol adfspark az Azure Blob-tárolóba, pyFiles pedig az adott tároló finom mappát.</span><span class="sxs-lookup"><span data-stu-id="334e9-211">The **rootPath** is set to **adfspark\\pyFiles** where adfspark is the Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="334e9-212">Ebben a példában az Azure Blob Storage lesz a Spark-fürt társított.</span><span class="sxs-lookup"><span data-stu-id="334e9-212">In this example, the Azure Blob Storage is the one that is associated with the Spark cluster.</span></span> <span data-ttu-id="334e9-213">Feltöltheti a fájlt egy másik Azure-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="334e9-213">You can upload the file to a different Azure Storage.</span></span> <span data-ttu-id="334e9-214">Ha így tesz, hozzon létre egy Azure tárolás társított szolgáltatása, hogy a storage-fiók összekapcsolása az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="334e9-214">If you do so, create an Azure Storage linked service to link that storage account to the data factory.</span></span> <span data-ttu-id="334e9-215">Ezt követően adja meg a társított szolgáltatás neve értékeként a **sparkJobLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="334e9-215">Then, specify the name of the linked service as a value for the **sparkJobLinkedService** property.</span></span> <span data-ttu-id="334e9-216">Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb Spark tevékenység által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="334e9-216">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by the Spark Activity.</span></span>  
    - <span data-ttu-id="334e9-217">A **entryFilePath** értékre van állítva a **test.py**, vagyis a python-fájl.</span><span class="sxs-lookup"><span data-stu-id="334e9-217">The **entryFilePath** is set to the **test.py**, which is the python file.</span></span>
    - <span data-ttu-id="334e9-218">A **getDebugInfo** tulajdonsága **mindig**, ami azt jelenti, a naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).</span><span class="sxs-lookup"><span data-stu-id="334e9-218">The **getDebugInfo** property is set to **Always**, which means the log files are always generated (success or failure).</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="334e9-219">Azt javasoljuk, hogy nincs megadva ez a tulajdonság `Always` éles környezetben kivéve, ha a probléma hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="334e9-219">We recommend that you do not set this property to `Always` in a production environment unless you are troubleshooting an issue.</span></span>
    - <span data-ttu-id="334e9-220">A **kimenete** szakasz tartalmaz egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="334e9-220">The **outputs** section has one output dataset.</span></span> <span data-ttu-id="334e9-221">Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha a spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="334e9-221">You must specify an output dataset even if the spark program does not produce any output.</span></span> <span data-ttu-id="334e9-222">A kimeneti adatkészlet meghajtók az ütemezés a következő feldolgozási sor (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="334e9-222">The output dataset drives the schedule for the pipeline (hourly, daily, etc.).</span></span>  

        <span data-ttu-id="334e9-223">A Spark tevékenység által támogatott tulajdonságokról kapcsolatos részletekért lásd: [tevékenység tulajdonságai Spark](#spark-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="334e9-223">For details about the properties supported by Spark activity, see [Spark activity properties](#spark-activity-properties) section.</span></span>
3. <span data-ttu-id="334e9-224">Kattintson újra az adatcsatornát, **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="334e9-224">To deploy the pipeline, click **Deploy** on the command bar.</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="334e9-225">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="334e9-225">Monitor pipeline</span></span>
1. <span data-ttu-id="334e9-226">Kattintson a **X** zárja be a Data Factory Editor paneleken, és navigáljon a Data Factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="334e9-226">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory home page.</span></span> <span data-ttu-id="334e9-227">Kattintson a **figyelése és kezelése** a figyelési alkalmazást egy másik lapon.</span><span class="sxs-lookup"><span data-stu-id="334e9-227">Click **Monitor and Manage** to launch the monitoring application in another tab.</span></span>

    ![Megfigyelés és kezelés csempe](media/data-factory-spark/monitor-and-manage-tile.png)
2. <span data-ttu-id="334e9-229">Módosítsa a **kezdési időpont** legfelül szűrő **2/1/2017**, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="334e9-229">Change the **Start time** filter at the top to **2/1/2017**, and click **Apply**.</span></span>
3. <span data-ttu-id="334e9-230">Mivel csak egy nap kezdete (2017-02-01) és befejezési időpontjával (2017-02-02) a feldolgozási folyamat között csak egy tevékenység ablakban kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="334e9-230">You should see only one activity window as there is only one day between the start (2017-02-01) and end times (2017-02-02) of the pipeline.</span></span> <span data-ttu-id="334e9-231">Győződjön meg arról, hogy az adatszelet az **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="334e9-231">Confirm that the data slice is in **ready** state.</span></span>

    ![A folyamat figyelése](media/data-factory-spark/monitor-and-manage-app.png)    
4. <span data-ttu-id="334e9-233">Válassza ki a **tevékenység ablakban** a tevékenységfuttatási kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="334e9-233">Select the **activity window** to see details about the activity run.</span></span> <span data-ttu-id="334e9-234">Ha nem sikerül, részleteit a jobb oldali ablaktáblában tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="334e9-234">If there is an error, you see details about it in the right pane.</span></span>

### <a name="verify-the-results"></a><span data-ttu-id="334e9-235">Az eredményeket</span><span class="sxs-lookup"><span data-stu-id="334e9-235">Verify the results</span></span>

1. <span data-ttu-id="334e9-236">Indítsa el **Jupyter notebook** navigáljon a HDInsight Spark-fürt esetében: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="334e9-236">Launch **Jupyter notebook** for your HDInsight Spark cluster by navigating to: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="334e9-237">Is indítsa el a fürt-irányítópult a HDInsight Spark-fürt számára, majd indítsa el **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="334e9-237">You can also launch cluster dashboard for your HDInsight Spark cluster, and then launch **Jupyter Notebook**.</span></span>
2. <span data-ttu-id="334e9-238">Kattintson a **új** -> **PySpark** új notebook elindításához.</span><span class="sxs-lookup"><span data-stu-id="334e9-238">Click **New** -> **PySpark** to start a new notebook.</span></span>

    ![Új Jupyter notebook](media/data-factory-spark/jupyter-new-book.png)
3. <span data-ttu-id="334e9-240">Futtassa a következő parancsot a másolás/beillesztés a szöveget, majd nyomja le az **SHIFT + ENTER** a második utasítás végén.</span><span class="sxs-lookup"><span data-stu-id="334e9-240">Run the following command by copy/pasting the text and pressing **SHIFT + ENTER** at the end of the second statement.</span></span>  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. <span data-ttu-id="334e9-241">Győződjön meg arról, hogy megjelenik-e a hvac tábla adatait:</span><span class="sxs-lookup"><span data-stu-id="334e9-241">Confirm that you see the data from the hvac table:</span></span>  

    ![Jupyter lekérdezés eredményei](media/data-factory-spark/jupyter-notebook-results.png)

<span data-ttu-id="334e9-243">Lásd: [Spark SQL-lekérdezés futtatható](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) szakaszban részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="334e9-243">See [Run a Spark SQL query](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) section for detailed instructions.</span></span> 

### <a name="troubleshooting"></a><span data-ttu-id="334e9-244">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="334e9-244">Troubleshooting</span></span>
<span data-ttu-id="334e9-245">Óta beállított **getDebugInfo** való **mindig**, megjelenik egy **napló** almappájában a **pyFiles** mappa az Azure Blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="334e9-245">Since you set **getDebugInfo** to **Always**, you see a **log** subfolder in the **pyFiles** folder in your Azure Blob container.</span></span> <span data-ttu-id="334e9-246">A napló mappában a naplófájl további részleteket biztosít.</span><span class="sxs-lookup"><span data-stu-id="334e9-246">The log file in the log folder provides additional details.</span></span> <span data-ttu-id="334e9-247">Ez a naplófájl akkor különösen akkor hasznos, ha nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="334e9-247">This log file is especially useful when there is an error.</span></span> <span data-ttu-id="334e9-248">Termelési környezetben érdemes lehet állítsa **hiba**.</span><span class="sxs-lookup"><span data-stu-id="334e9-248">In a production environment, you may want to set it to **Failure**.</span></span>

<span data-ttu-id="334e9-249">További hibaelhárítási, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="334e9-249">For further troubleshooting, do the following steps:</span></span>


1. <span data-ttu-id="334e9-250">Navigáljon a `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span><span class="sxs-lookup"><span data-stu-id="334e9-250">Navigate to `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span></span>

    ![Alkalmazás a YARN felhasználói felületen](media/data-factory-spark/yarnui-application.png)  
2. <span data-ttu-id="334e9-252">Kattintson a **naplók** egy Futtatás kísérletek.</span><span class="sxs-lookup"><span data-stu-id="334e9-252">Click **Logs** for one of the run attempts.</span></span>

    ![Alkalmazások lap](media/data-factory-spark/yarn-applications.png)
3. <span data-ttu-id="334e9-254">Meg kell jelennie a hibával kapcsolatos további információkat a napló lapon.</span><span class="sxs-lookup"><span data-stu-id="334e9-254">You should see additional error information in the log page.</span></span>

    ![Naplózási hiba](media/data-factory-spark/yarnui-application-error.png)

<span data-ttu-id="334e9-256">Az alábbi szakaszok ismertetik a Data Factory entitások Apache Spark-fürt és a Spark-tevékenység használata a data factory.</span><span class="sxs-lookup"><span data-stu-id="334e9-256">The following sections provide information about Data Factory entities to use Apache Spark cluster and Spark Activity in your data factory.</span></span>

## <a name="spark-activity-properties"></a><span data-ttu-id="334e9-257">A Spark-tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="334e9-257">Spark activity properties</span></span>
<span data-ttu-id="334e9-258">Ez a minta Spark tevékenység az adatcsatorna JSON-definícióból:</span><span class="sxs-lookup"><span data-stu-id="334e9-258">Here is the sample JSON definition of a pipeline with Spark Activity:</span></span>    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

<span data-ttu-id="334e9-259">A következő táblázat a JSON-definícióból használt JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="334e9-259">The following table describes the JSON properties used in the JSON definition:</span></span>

| <span data-ttu-id="334e9-260">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="334e9-260">Property</span></span> | <span data-ttu-id="334e9-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="334e9-261">Description</span></span> | <span data-ttu-id="334e9-262">Szükséges</span><span class="sxs-lookup"><span data-stu-id="334e9-262">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="334e9-263">név</span><span class="sxs-lookup"><span data-stu-id="334e9-263">name</span></span> | <span data-ttu-id="334e9-264">A feldolgozási tevékenység nevét.</span><span class="sxs-lookup"><span data-stu-id="334e9-264">Name of the activity in the pipeline.</span></span> | <span data-ttu-id="334e9-265">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-265">Yes</span></span> |
| <span data-ttu-id="334e9-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="334e9-266">description</span></span> | <span data-ttu-id="334e9-267">A tevékenység mit leíró szöveg.</span><span class="sxs-lookup"><span data-stu-id="334e9-267">Text describing what the activity does.</span></span> | <span data-ttu-id="334e9-268">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-268">No</span></span> |
| <span data-ttu-id="334e9-269">type</span><span class="sxs-lookup"><span data-stu-id="334e9-269">type</span></span> | <span data-ttu-id="334e9-270">Ez a tulajdonság HDInsightSpark kell állítani.</span><span class="sxs-lookup"><span data-stu-id="334e9-270">This property must be set to HDInsightSpark.</span></span> | <span data-ttu-id="334e9-271">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-271">Yes</span></span> |
| <span data-ttu-id="334e9-272">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="334e9-272">linkedServiceName</span></span> | <span data-ttu-id="334e9-273">A Spark program fut. HDInsight társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="334e9-273">Name of the HDInsight linked service on which the Spark program runs.</span></span> | <span data-ttu-id="334e9-274">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-274">Yes</span></span> |
| <span data-ttu-id="334e9-275">rootPath</span><span class="sxs-lookup"><span data-stu-id="334e9-275">rootPath</span></span> | <span data-ttu-id="334e9-276">Az Azure Blob-tároló és a Spark-fájlt tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="334e9-276">The Azure Blob container and folder that contains the Spark file.</span></span> <span data-ttu-id="334e9-277">A fájlnév pedig kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="334e9-277">The file name is case-sensitive.</span></span> | <span data-ttu-id="334e9-278">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-278">Yes</span></span> |
| <span data-ttu-id="334e9-279">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="334e9-279">entryFilePath</span></span> | <span data-ttu-id="334e9-280">A gyökérmappában található azon a Spark kódcsomag relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="334e9-280">Relative path to the root folder of the Spark code/package.</span></span> | <span data-ttu-id="334e9-281">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-281">Yes</span></span> |
| <span data-ttu-id="334e9-282">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="334e9-282">className</span></span> | <span data-ttu-id="334e9-283">Az alkalmazás Java/Spark fő osztály</span><span class="sxs-lookup"><span data-stu-id="334e9-283">Application's Java/Spark main class</span></span> | <span data-ttu-id="334e9-284">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-284">No</span></span> |
| <span data-ttu-id="334e9-285">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="334e9-285">arguments</span></span> | <span data-ttu-id="334e9-286">A Spark program parancssori argumentumokat listáját.</span><span class="sxs-lookup"><span data-stu-id="334e9-286">A list of command-line arguments to the Spark program.</span></span> | <span data-ttu-id="334e9-287">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-287">No</span></span> |
| <span data-ttu-id="334e9-288">proxyUser</span><span class="sxs-lookup"><span data-stu-id="334e9-288">proxyUser</span></span> | <span data-ttu-id="334e9-289">A Spark program végrehajtásának megszemélyesíteni a felhasználói fiók</span><span class="sxs-lookup"><span data-stu-id="334e9-289">The user account to impersonate to execute the Spark program</span></span> | <span data-ttu-id="334e9-290">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-290">No</span></span> |
| <span data-ttu-id="334e9-291">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="334e9-291">sparkConfig</span></span> | <span data-ttu-id="334e9-292">Adja meg a témakörben ismertetett Spark konfigurációs tulajdonságok értékeit: [Spark konfigurációs - alkalmazás tulajdonságainak](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span><span class="sxs-lookup"><span data-stu-id="334e9-292">Specify values for Spark configuration properties listed in the topic: [Spark Configuration - Application properties](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span></span> | <span data-ttu-id="334e9-293">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-293">No</span></span> |
| <span data-ttu-id="334e9-294">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="334e9-294">getDebugInfo</span></span> | <span data-ttu-id="334e9-295">Itt adhatja meg, amikor a Spark naplófájlok kerülnek a HDInsight-fürt által használt Azure storage (vagy) leírt módon sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="334e9-295">Specifies when the Spark log files are copied to the Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="334e9-296">Megengedett értékek: None, mindig, vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="334e9-296">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="334e9-297">Alapértelmezett érték: nincs.</span><span class="sxs-lookup"><span data-stu-id="334e9-297">Default value: None.</span></span> | <span data-ttu-id="334e9-298">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-298">No</span></span> |
| <span data-ttu-id="334e9-299">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="334e9-299">sparkJobLinkedService</span></span> | <span data-ttu-id="334e9-300">Az Azure Storage társított szolgáltatás, amely tárolja a Spark feladat fájl, a függőségeket és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="334e9-300">The Azure Storage linked service that holds the Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="334e9-301">Ha nem ad meg egy értéket ehhez a tulajdonsághoz, a HDInsight-fürthöz társított tárolót használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="334e9-301">If you do not specify a value for this property, the storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="334e9-302">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-302">No</span></span> |

## <a name="folder-structure"></a><span data-ttu-id="334e9-303">Mappaszerkezet</span><span class="sxs-lookup"><span data-stu-id="334e9-303">Folder structure</span></span>
<span data-ttu-id="334e9-304">A Spark tevékenység nem támogatja a Pig-egy sor parancsprogramot, és Hive tevékenységek tegye.</span><span class="sxs-lookup"><span data-stu-id="334e9-304">The Spark activity does not support an in-line script as Pig and Hive activities do.</span></span> <span data-ttu-id="334e9-305">Spark feladatok akkor is több bővíthető, mint a Pig vagy Hive-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="334e9-305">Spark jobs are also more extensible than Pig/Hive jobs.</span></span> <span data-ttu-id="334e9-306">A Spark-feladatok biztosíthat több függőség például jar (a java CLASSPATH helyezett) csomagok, python-fájlok (a PYTHONPATH helyezve) és egyéb fájlokat.</span><span class="sxs-lookup"><span data-stu-id="334e9-306">For Spark jobs, you can provide multiple dependencies such as jar packages (placed in the java CLASSPATH), python files (placed on the PYTHONPATH), and any other files.</span></span>

<span data-ttu-id="334e9-307">A következő mappaszerkezet létrehozása az Azure Blob Storage HDInsight kapcsolódó szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="334e9-307">Create the following folder structure in the Azure Blob storage referenced by the HDInsight linked service.</span></span> <span data-ttu-id="334e9-308">Ezután a megfelelő mappákat a gyökérmappában által képviselt függő fájlok feltöltése **entryFilePath**.</span><span class="sxs-lookup"><span data-stu-id="334e9-308">Then, upload dependent files to the appropriate sub folders in the root folder represented by **entryFilePath**.</span></span> <span data-ttu-id="334e9-309">Python pyFiles almappájába és jar-fájlok például a legfelső szintű JAR-fájlok kivételével almappája feltöltése.</span><span class="sxs-lookup"><span data-stu-id="334e9-309">For example, upload python files to the pyFiles subfolder and jar files to the jars subfolder of the root folder.</span></span> <span data-ttu-id="334e9-310">Futásidőben a Data Factory szolgáltatásnak a következő gyökérmappa-szerkezetében vár az Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="334e9-310">At runtime, Data Factory service expects the following folder structure in the Azure Blob storage:</span></span>     

| <span data-ttu-id="334e9-311">Elérési út</span><span class="sxs-lookup"><span data-stu-id="334e9-311">Path</span></span> | <span data-ttu-id="334e9-312">Leírás</span><span class="sxs-lookup"><span data-stu-id="334e9-312">Description</span></span> | <span data-ttu-id="334e9-313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="334e9-313">Required</span></span> | <span data-ttu-id="334e9-314">Típus</span><span class="sxs-lookup"><span data-stu-id="334e9-314">Type</span></span> |
| ---- | ----------- | -------- | ---- |
| <span data-ttu-id="334e9-315">.</span><span class="sxs-lookup"><span data-stu-id="334e9-315">.</span></span> | <span data-ttu-id="334e9-316">A Spark feladatot a tárolás társított szolgáltatásának elérési útjának gyökeréhez</span><span class="sxs-lookup"><span data-stu-id="334e9-316">The root path of the Spark job in the storage linked service</span></span>  | <span data-ttu-id="334e9-317">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-317">Yes</span></span> | <span data-ttu-id="334e9-318">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-318">Folder</span></span> |
| <span data-ttu-id="334e9-319">&lt;felhasználó által definiált&gt;</span><span class="sxs-lookup"><span data-stu-id="334e9-319">&lt;user defined &gt;</span></span> | <span data-ttu-id="334e9-320">Az elérési út a Spark feladat bejegyzés fájlra hivatkozik</span><span class="sxs-lookup"><span data-stu-id="334e9-320">The path pointing to the entry file of the Spark job</span></span> | <span data-ttu-id="334e9-321">Igen</span><span class="sxs-lookup"><span data-stu-id="334e9-321">Yes</span></span> | <span data-ttu-id="334e9-322">Fájl</span><span class="sxs-lookup"><span data-stu-id="334e9-322">File</span></span> |
| <span data-ttu-id="334e9-323">. / jars</span><span class="sxs-lookup"><span data-stu-id="334e9-323">./jars</span></span> | <span data-ttu-id="334e9-324">Ebben a mappában található összes fájl feltöltése, és a fürt a java classpath helyezve</span><span class="sxs-lookup"><span data-stu-id="334e9-324">All files under this folder are uploaded and placed on the java classpath of the cluster</span></span> | <span data-ttu-id="334e9-325">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-325">No</span></span> | <span data-ttu-id="334e9-326">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-326">Folder</span></span> |
| <span data-ttu-id="334e9-327">. / pyFiles</span><span class="sxs-lookup"><span data-stu-id="334e9-327">./pyFiles</span></span> | <span data-ttu-id="334e9-328">Ebben a mappában található összes fájl feltöltése, és a fürt PYTHONPATH helyezve</span><span class="sxs-lookup"><span data-stu-id="334e9-328">All files under this folder are uploaded and placed on the PYTHONPATH of the cluster</span></span> | <span data-ttu-id="334e9-329">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-329">No</span></span> | <span data-ttu-id="334e9-330">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-330">Folder</span></span> |
| <span data-ttu-id="334e9-331">. / fájlok</span><span class="sxs-lookup"><span data-stu-id="334e9-331">./files</span></span> | <span data-ttu-id="334e9-332">Ebben a mappában található összes fájl feltöltése és végrehajtó munkakönyvtár helyezve</span><span class="sxs-lookup"><span data-stu-id="334e9-332">All files under this folder are uploaded and placed on executor working directory</span></span> | <span data-ttu-id="334e9-333">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-333">No</span></span> | <span data-ttu-id="334e9-334">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-334">Folder</span></span> |
| <span data-ttu-id="334e9-335">. / archiválja</span><span class="sxs-lookup"><span data-stu-id="334e9-335">./archives</span></span> | <span data-ttu-id="334e9-336">Ebben a mappában található összes fájl tömörítetlen.</span><span class="sxs-lookup"><span data-stu-id="334e9-336">All files under this folder are uncompressed</span></span> | <span data-ttu-id="334e9-337">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-337">No</span></span> | <span data-ttu-id="334e9-338">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-338">Folder</span></span> |
| <span data-ttu-id="334e9-339">. / naplói</span><span class="sxs-lookup"><span data-stu-id="334e9-339">./logs</span></span> | <span data-ttu-id="334e9-340">A Spark-fürt származó naplók tárolására szolgáló mappa.</span><span class="sxs-lookup"><span data-stu-id="334e9-340">The folder where logs from the Spark cluster are stored.</span></span>| <span data-ttu-id="334e9-341">Nem</span><span class="sxs-lookup"><span data-stu-id="334e9-341">No</span></span> | <span data-ttu-id="334e9-342">Mappa</span><span class="sxs-lookup"><span data-stu-id="334e9-342">Folder</span></span> |

<span data-ttu-id="334e9-343">Íme egy példa egy tárolás az Azure Blob Storage a HDInsight kapcsolódó szolgáltatás által hivatkozott két Spark feladat fájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="334e9-343">Here is an example for a storage containing two Spark job files in the Azure Blob Storage referenced by the HDInsight linked service.</span></span>

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
