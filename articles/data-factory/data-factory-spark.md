---
title: az Azure Data Factory programok aaaInvoke Spark |} Microsoft Docs
description: "Ismerje meg, hogyan tooinvoke Spark programok egy az Azure data factory használatával hello MapReduce művelethez."
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
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a><span data-ttu-id="79cab-103">Az Azure Data Factory folyamatok Spark programok meghívása</span><span class="sxs-lookup"><span data-stu-id="79cab-103">Invoke Spark programs from Azure Data Factory pipelines</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="79cab-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-104">Hive Activity</span></span>](data-factory-hive-activity.md)
> * [<span data-ttu-id="79cab-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="79cab-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="79cab-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="79cab-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="79cab-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="79cab-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="79cab-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="79cab-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="79cab-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="79cab-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="79cab-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="79cab-114">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="79cab-114">Introduction</span></span>
<span data-ttu-id="79cab-115">Spark tevékenység egyike hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) Azure Data Factory által támogatott.</span><span class="sxs-lookup"><span data-stu-id="79cab-115">Spark Activity is one of hello [data transformation activities](data-factory-data-transformation-activities.md) supported by Azure Data Factory.</span></span> <span data-ttu-id="79cab-116">Ez a tevékenység fut megadott hello az Apache Spark-fürt Azure hdinsight Spark programot.</span><span class="sxs-lookup"><span data-stu-id="79cab-116">This activity runs hello specified Spark program on your Apache Spark cluster in Azure HDInsight.</span></span>    

> [!IMPORTANT]
> - <span data-ttu-id="79cab-117">Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.</span><span class="sxs-lookup"><span data-stu-id="79cab-117">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
> - <span data-ttu-id="79cab-118">Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürtjei (saját).</span><span class="sxs-lookup"><span data-stu-id="79cab-118">Spark Activity supports only existing (your own) HDInsight Spark clusters.</span></span> <span data-ttu-id="79cab-119">Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="79cab-119">It does not support an on-demand HDInsight linked service.</span></span>

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a><span data-ttu-id="79cab-120">Forgatókönyv: hozzon létre egy folyamatot Spark tevékenység</span><span class="sxs-lookup"><span data-stu-id="79cab-120">Walkthrough: create a pipeline with Spark activity</span></span>
<span data-ttu-id="79cab-121">Az alábbiakban hello jellemzően előforduló lépéseket toocreate a Data Factory-folyamathoz Spark tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="79cab-121">Here are hello typical steps toocreate a Data Factory pipeline with a Spark activity.</span></span>  

1. <span data-ttu-id="79cab-122">Adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-122">Create a data factory.</span></span>
2. <span data-ttu-id="79cab-123">Hozzon létre egy Azure Storage társított szolgáltatás toolink a az Azure storage társított a HDInsight Spark-fürt toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-123">Create an Azure Storage linked service toolink your Azure storage that is associated with your HDInsight Spark cluster toohello data factory.</span></span>     
2. <span data-ttu-id="79cab-124">Hozzon létre egy Azure HDInsight kapcsolódó szolgáltatás toolink az Apache Spark-fürt Azure HDInsight toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-124">Create an Azure HDInsight linked service toolink your Apache Spark cluster in Azure HDInsight toohello data factory.</span></span>
3. <span data-ttu-id="79cab-125">Hozzon létre egy adatkészlet hivatkozó toohello Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="79cab-125">Create a dataset that refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="79cab-126">Jelenleg adjon meg egy kimeneti adatkészlet a tevékenység akkor is, ha nincs folyamatban létrehozott kimenet van.</span><span class="sxs-lookup"><span data-stu-id="79cab-126">Currently, you must specify an output dataset for an activity even if there is no output being produced.</span></span>  
4. <span data-ttu-id="79cab-127">Hozzon létre egy folyamatot toohello HDInsight társított szolgáltatás létrehozása a #2 hivatkozó Spark-tevékenység.</span><span class="sxs-lookup"><span data-stu-id="79cab-127">Create a pipeline with Spark activity that refers toohello HDInsight linked service created in #2.</span></span> <span data-ttu-id="79cab-128">hello konfigurálta, egy kimeneti adatkészlet hello előző lépésben létrehozott hello az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="79cab-128">hello activity is configured with hello dataset you created in hello previous step as an output dataset.</span></span> <span data-ttu-id="79cab-129">hello kimeneti adatkészlet milyen meghajtók hello ütemezés (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="79cab-129">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="79cab-130">Hello kimeneti adatkészletet, ezért meg kell adnia annak ellenére, hogy hello tevékenység készít valóban kimenettel.</span><span class="sxs-lookup"><span data-stu-id="79cab-130">Therefore, you must specify hello output dataset even though hello activity does not really produce an output.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="79cab-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="79cab-131">Prerequisites</span></span>
1. <span data-ttu-id="79cab-132">Hozzon létre egy **általános célú Azure Storage-fiók** hello forgatókönyv a következő utasítással: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="79cab-132">Create a **general-purpose Azure Storage Account** by following instructions in hello walkthrough: [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
2. <span data-ttu-id="79cab-133">Hozzon létre egy **Azure HDInsight az Apache Spark-fürt** hello oktatóanyag utasításai a következő szerint: [Azure HDInsight létrehozása Apache Spark-fürt](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="79cab-133">Create an **Apache Spark cluster in Azure HDInsight** by following instructions in hello tutorial: [Create Apache Spark cluster in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="79cab-134">Ezzel a fürttel #1. lépésben létrehozott hello Azure storage-fiók társítása.</span><span class="sxs-lookup"><span data-stu-id="79cab-134">Associate hello Azure storage account you created in step #1 with this cluster.</span></span>  
3. <span data-ttu-id="79cab-135">Töltse le, és tekintse át a python-parancsfájl hello **test.py** helyen található: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span><span class="sxs-lookup"><span data-stu-id="79cab-135">Download and review hello python script file **test.py** located at: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span></span>  
3.  <span data-ttu-id="79cab-136">Töltse fel **test.py** toohello **pyFiles** hello mappájában **adfspark** az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-136">Upload **test.py** toohello **pyFiles** folder in hello **adfspark** container in your Azure Blob storage.</span></span> <span data-ttu-id="79cab-137">Hello tároló és hello mappa létrehozása, ha azok nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="79cab-137">Create hello container and hello folder if they do not exist.</span></span>

### <a name="create-data-factory"></a><span data-ttu-id="79cab-138">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-138">Create data factory</span></span>
<span data-ttu-id="79cab-139">Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79cab-139">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="79cab-140">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="79cab-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="79cab-141">Kattintson a **új** hello bal oldali menüben kattintson **adatok + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="79cab-141">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="79cab-142">A hello **új adat-előállító** panelen adja meg **SparkDF** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="79cab-142">In hello **New data factory** blade, enter **SparkDF** for hello Name.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="79cab-143">az Azure data factory hello hello nevének kell lennie **globálisan egyedi**.</span><span class="sxs-lookup"><span data-stu-id="79cab-143">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="79cab-144">Ha hello hibát látja: **nem érhető el adat-előállító "SparkDF"**.</span><span class="sxs-lookup"><span data-stu-id="79cab-144">If you see hello error: **Data factory name “SparkDF” is not available**.</span></span> <span data-ttu-id="79cab-145">Hello adat-előállítóban (például yournameSparkDFdate, és próbálja meg újra létrehozni hello nevének módosítása.</span><span class="sxs-lookup"><span data-stu-id="79cab-145">Change hello name of hello data factory (for example, yournameSparkDFdate, and try creating again.</span></span> <span data-ttu-id="79cab-146">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="79cab-146">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
4. <span data-ttu-id="79cab-147">Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.</span><span class="sxs-lookup"><span data-stu-id="79cab-147">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="79cab-148">Válasszon ki egy létező **erőforráscsoport** , vagy hozzon létre egy Azure-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="79cab-148">Select an existing **resource group** or create an Azure resource group.</span></span>
6. <span data-ttu-id="79cab-149">Válassza ki **PIN-kód toodashboard** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="79cab-149">Select **Pin toodashboard** option.</span></span>  
6. <span data-ttu-id="79cab-150">Kattintson a **létrehozása** a hello **új adat-előállító** panelen.</span><span class="sxs-lookup"><span data-stu-id="79cab-150">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="79cab-151">toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="79cab-151">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
7. <span data-ttu-id="79cab-152">Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="79cab-152">You see hello data factory being created in hello **dashboard** of hello Azure portal as follows:</span></span>   
8. <span data-ttu-id="79cab-153">Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.</span><span class="sxs-lookup"><span data-stu-id="79cab-153">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span> <span data-ttu-id="79cab-154">Ha hello data factory oldalon nem látja, kattintson a hello csempe az irányítópulton hello a data factory.</span><span class="sxs-lookup"><span data-stu-id="79cab-154">If you do not see hello data factory page, click hello tile for your data factory on hello dashboard.</span></span>

    ![A Data Factory panel](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a><span data-ttu-id="79cab-156">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-156">Create linked services</span></span>
<span data-ttu-id="79cab-157">Ebben a lépésben hozzon létre két csatolt szolgáltatást, egy toolink a Spark-fürt tooyour adat-előállító és a hello más toolink az Azure storage tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-157">In this step, you create two linked services, one toolink your Spark cluster tooyour data factory, and hello other toolink your Azure storage tooyour data factory.</span></span>  

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="79cab-158">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-158">Create Azure Storage linked service</span></span>
<span data-ttu-id="79cab-159">Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="79cab-159">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="79cab-160">A DataSet adatkészlet hoz létre, később a forgatókönyvben egy lépésben toothis kapcsolódó szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="79cab-160">A dataset you create in a step later in this walkthrough refers toothis linked service.</span></span> <span data-ttu-id="79cab-161">hello hello következő lépésben meghatározó kapcsolódó HDInsight-szolgáltatás túl toothis kapcsolódó szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="79cab-161">hello HDInsight linked service that you define in hello next step refers toothis linked service too.</span></span>  

1. <span data-ttu-id="79cab-162">Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** a data factory paneljét.</span><span class="sxs-lookup"><span data-stu-id="79cab-162">Click **Author and deploy** on hello **Data Factory** blade for your data factory.</span></span> <span data-ttu-id="79cab-163">Hello Data Factory Editor kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="79cab-163">You should see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="79cab-164">Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.</span><span class="sxs-lookup"><span data-stu-id="79cab-164">Click **New data store** and choose **Azure storage**.</span></span>

   ![Új adattár – Azure Storage – menü](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="79cab-166">Megtekintheti az hello **JSON-parancsfájl** létrehozásához egy Azure Storage társított hello szerkesztőben szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="79cab-166">You should see hello **JSON script** for creating an Azure Storage linked service in hello editor.</span></span>

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="79cab-168">Cserélje le **fióknév** és **fiókkulcs** a hello nevét és hívóbetűjét az Azure-tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="79cab-168">Replace **account name** and **account key** with hello name and access key of your Azure storage account.</span></span> <span data-ttu-id="79cab-169">toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="79cab-169">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="79cab-170">toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="79cab-170">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="79cab-171">Hello társított szolgáltatás telepítése után sikeresen hello **vázlat-1** ablak kell eltűnnek, és látni **AzureStorageLinkedService** a hello bal oldali fanézetben hello.</span><span class="sxs-lookup"><span data-stu-id="79cab-171">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

#### <a name="create-hdinsight-linked-service"></a><span data-ttu-id="79cab-172">A HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-172">Create HDInsight linked service</span></span>
<span data-ttu-id="79cab-173">Ebben a lépésben a HDInsight Spark-fürt toohello data factory kapcsolt Azure HDInsight szolgáltatás toolink hoz létre.</span><span class="sxs-lookup"><span data-stu-id="79cab-173">In this step, you create Azure HDInsight linked service toolink your HDInsight Spark cluster toohello data factory.</span></span> <span data-ttu-id="79cab-174">hello HDInsight-fürthöz használt toorun hello Spark program hello Spark tevékenység hello folyamatának Ez a példa megadott.</span><span class="sxs-lookup"><span data-stu-id="79cab-174">hello HDInsight cluster is used toorun hello Spark program specified in hello Spark activity of hello pipeline in this sample.</span></span>  

1. <span data-ttu-id="79cab-175">Ha nem látja ezt a gombot, kattintson a három pontot  **További** hello eszköztáron kattintson **új számítási**, és kattintson a **HDInsight-fürt**.</span><span class="sxs-lookup"><span data-stu-id="79cab-175">Click **... More** on hello toolbar, click **New compute**, and then click **HDInsight cluster**.</span></span>

    ![A HDInsight társított szolgáltatás létrehozása](media/data-factory-spark/new-hdinsight-linked-service.png)
2. <span data-ttu-id="79cab-177">Másolja és illessze be a következő kódrészletet toohello hello **vázlat-1** ablak.</span><span class="sxs-lookup"><span data-stu-id="79cab-177">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="79cab-178">Az hello hello JSON-szerkesztőben, a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="79cab-178">In hello JSON editor, do hello following steps:</span></span>
    1. <span data-ttu-id="79cab-179">Adja meg a hello **URI** hello HDInsight Spark fürt.</span><span class="sxs-lookup"><span data-stu-id="79cab-179">Specify hello **URI** for hello HDInsight Spark cluster.</span></span> <span data-ttu-id="79cab-180">Például: `https://<sparkclustername>.azurehdinsight.net/`.</span><span class="sxs-lookup"><span data-stu-id="79cab-180">For example: `https://<sparkclustername>.azurehdinsight.net/`.</span></span>
    2. <span data-ttu-id="79cab-181">Adja meg hello hello **felhasználói** kik hozzáférés toohello Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="79cab-181">Specify hello name of hello **user** who has access toohello Spark cluster.</span></span>
    3. <span data-ttu-id="79cab-182">Adja meg a hello **jelszó** felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="79cab-182">Specify hello **password** for user.</span></span>
    4. <span data-ttu-id="79cab-183">Adja meg a hello **Azure Storage társított szolgáltatásnak** társított hello HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="79cab-183">Specify hello **Azure Storage linked service** that is associated with hello HDInsight Spark cluster.</span></span> <span data-ttu-id="79cab-184">Az ebben a példában is: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="79cab-184">In this example, it is: **AzureStorageLinkedService**.</span></span>

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
    > - <span data-ttu-id="79cab-185">Spark tevékenység nem támogatja a HDInsight Spark-fürtjei használja az Azure Data Lake Store elsődleges tárolására.</span><span class="sxs-lookup"><span data-stu-id="79cab-185">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
    > - <span data-ttu-id="79cab-186">Spark tevékenység támogatja a csak meglévő HDInsight Spark-fürt (saját).</span><span class="sxs-lookup"><span data-stu-id="79cab-186">Spark Activity supports only existing (your own) HDInsight Spark cluster.</span></span> <span data-ttu-id="79cab-187">Egy igény szerinti HDInsight társított szolgáltatás nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="79cab-187">It does not support an on-demand HDInsight linked service.</span></span>

    <span data-ttu-id="79cab-188">Lásd: [HDInsight társított szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) hello vonatkozó további információért HDInsight társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="79cab-188">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details about hello HDInsight linked service.</span></span>
3.  <span data-ttu-id="79cab-189">toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="79cab-189">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="79cab-190">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-190">Create output dataset</span></span>
<span data-ttu-id="79cab-191">hello kimeneti adatkészlet milyen meghajtók hello ütemezés (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="79cab-191">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="79cab-192">Ezért meg kell adnia egy kimeneti adatkészlet hello spark tevékenység hello folyamat annak ellenére, hogy hello tevékenység valóban nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="79cab-192">Therefore, you must specify an output dataset for hello spark activity in hello pipeline even though hello activity does not really produce any output.</span></span> <span data-ttu-id="79cab-193">Egy bemeneti adatkészlet hello tevékenység megadása nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="79cab-193">Specifying an input dataset for hello activity is optional.</span></span>

1. <span data-ttu-id="79cab-194">A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="79cab-194">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="79cab-195">Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello.</span><span class="sxs-lookup"><span data-stu-id="79cab-195">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="79cab-196">hello JSON részlet nevű adatkészlet meghatározása **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="79cab-196">hello JSON snippet defines a dataset called **OutputDataset**.</span></span> <span data-ttu-id="79cab-197">Emellett megadhatja, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfspark** és nevű hello mappát **pyFiles/kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="79cab-197">In addition, you specify that hello results are stored in hello blob container called **adfspark** and hello folder called **pyFiles/output**.</span></span> <span data-ttu-id="79cab-198">Amint azt korábban említettük, ez az adatkészlet egy üres adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="79cab-198">As mentioned earlier, this dataset is a dummy dataset.</span></span> <span data-ttu-id="79cab-199">Ebben a példában hello Spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="79cab-199">hello Spark program in this example does not produce any output.</span></span> <span data-ttu-id="79cab-200">Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet naponta jön létre.</span><span class="sxs-lookup"><span data-stu-id="79cab-200">hello **availability** section specifies that hello output dataset is produced daily.</span></span>  

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
3. <span data-ttu-id="79cab-201">toodeploy hello dataset, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="79cab-201">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span>


### <a name="create-pipeline"></a><span data-ttu-id="79cab-202">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="79cab-202">Create pipeline</span></span>
<span data-ttu-id="79cab-203">Ebben a lépésben a folyamat létrehoz egy **HDInsightSpark** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="79cab-203">In this step, you create a pipeline with a **HDInsightSpark** activity.</span></span> <span data-ttu-id="79cab-204">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="79cab-204">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="79cab-205">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="79cab-205">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="79cab-206">Ezért bemeneti adatkészlet ebben a példában van megadva.</span><span class="sxs-lookup"><span data-stu-id="79cab-206">Therefore, no input dataset is specified in this example.</span></span>

1. <span data-ttu-id="79cab-207">A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon, és kattintson a **új adatcsatorna**.</span><span class="sxs-lookup"><span data-stu-id="79cab-207">In hello **Data Factory Editor**, click **… More** on hello command bar, and then click **New pipeline**.</span></span>
2. <span data-ttu-id="79cab-208">Cserélje le a következő parancsfájl hello hello parancsfájl hello vázlat-1 ablakban:</span><span class="sxs-lookup"><span data-stu-id="79cab-208">Replace hello script in hello Draft-1 window with hello following script:</span></span>

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
    <span data-ttu-id="79cab-209">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="79cab-209">Note hello following points:</span></span>
    - <span data-ttu-id="79cab-210">Hello **típus** tulajdonsága túl**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="79cab-210">hello **type** property is set too**HDInsightSpark**.</span></span>
    - <span data-ttu-id="79cab-211">Hello **rootPath** értéke túl**adfspark\\pyFiles** ahol adfspark hello Azure Blob-tároló és pyFiles pedig az adott tároló finom mappát.</span><span class="sxs-lookup"><span data-stu-id="79cab-211">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="79cab-212">Ebben a példában a hello Azure Blob Storage egy Spark-fürt hello társított hello.</span><span class="sxs-lookup"><span data-stu-id="79cab-212">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="79cab-213">Feltöltheti a hello fájl tooa különböző Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="79cab-213">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="79cab-214">Ha így tesz, hozzon létre egy Azure Storage társított szolgáltatás toolink adott tárolási fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-214">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="79cab-215">Ezt követően adja a hello társított szolgáltatás neve hello hello értékeként **sparkJobLinkedService** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="79cab-215">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="79cab-216">Lásd: [Spark-tevékenység tulajdonságai](#spark-activity-properties) Ez a tulajdonság és az egyéb hello Spark tevékenység által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="79cab-216">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>  
    - <span data-ttu-id="79cab-217">Hello **entryFilePath** értéke toohello **test.py**, vagyis hello python-fájl.</span><span class="sxs-lookup"><span data-stu-id="79cab-217">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span>
    - <span data-ttu-id="79cab-218">Hello **getDebugInfo** tulajdonsága túl**mindig**, ami azt jelenti, hogy hello naplófájlok helyét a rendszer mindig generált (sikeres vagy sikertelen).</span><span class="sxs-lookup"><span data-stu-id="79cab-218">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="79cab-219">Azt javasoljuk, hogy nincs megadva ez a tulajdonság túl`Always` éles környezetben kivéve, ha a probléma hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="79cab-219">We recommend that you do not set this property too`Always` in a production environment unless you are troubleshooting an issue.</span></span>
    - <span data-ttu-id="79cab-220">Hello **kimenete** szakasz tartalmaz egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="79cab-220">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="79cab-221">Meg kell adnia egy kimeneti adatkészletet, még akkor is, ha hello spark program nem ad kimenetet.</span><span class="sxs-lookup"><span data-stu-id="79cab-221">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="79cab-222">hello kimeneti adatkészlet meghajtók hello ütemezés hello adatcsatorna (óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="79cab-222">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>  

        <span data-ttu-id="79cab-223">Spark tevékenység által támogatott hello tulajdonságok kapcsolatos részletekért lásd: [tevékenység tulajdonságai Spark](#spark-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="79cab-223">For details about hello properties supported by Spark activity, see [Spark activity properties](#spark-activity-properties) section.</span></span>
3. <span data-ttu-id="79cab-224">toodeploy hello sorban, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="79cab-224">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="79cab-225">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="79cab-225">Monitor pipeline</span></span>
1. <span data-ttu-id="79cab-226">Kattintson a **X** tooclose Data Factory Editor paneleken és toonavigate biztonsági toohello adat-előállító kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="79cab-226">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory home page.</span></span> <span data-ttu-id="79cab-227">Kattintson a **figyelése és kezelése** toolaunch hello alkalmazás egy másik lapon.</span><span class="sxs-lookup"><span data-stu-id="79cab-227">Click **Monitor and Manage** toolaunch hello monitoring application in another tab.</span></span>

    ![Megfigyelés és kezelés csempe](media/data-factory-spark/monitor-and-manage-tile.png)
2. <span data-ttu-id="79cab-229">Módosítás hello **kezdési időpont** szűrése hello felső túl**2/1/2017**, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="79cab-229">Change hello **Start time** filter at hello top too**2/1/2017**, and click **Apply**.</span></span>
3. <span data-ttu-id="79cab-230">Csak egy tevékenység ablakban, csak egy nap közötti hello kezdési (2017-02-01) és befejezési időpontja (2017-02-02) hello folyamatának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="79cab-230">You should see only one activity window as there is only one day between hello start (2017-02-01) and end times (2017-02-02) of hello pipeline.</span></span> <span data-ttu-id="79cab-231">Győződjön meg arról, hogy hello adatszelet a **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="79cab-231">Confirm that hello data slice is in **ready** state.</span></span>

    ![A figyelő hello folyamat](media/data-factory-spark/monitor-and-manage-app.png)    
4. <span data-ttu-id="79cab-233">Jelölje be hello **tevékenység ablakban** hello tevékenységfuttatási toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="79cab-233">Select hello **activity window** toosee details about hello activity run.</span></span> <span data-ttu-id="79cab-234">Ha nem sikerül, akkor vonatkozó részletes azt hello jobb oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="79cab-234">If there is an error, you see details about it in hello right pane.</span></span>

### <a name="verify-hello-results"></a><span data-ttu-id="79cab-235">Hello eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="79cab-235">Verify hello results</span></span>

1. <span data-ttu-id="79cab-236">Indítsa el **Jupyter notebook** navigáljon a HDInsight Spark-fürt esetében: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="79cab-236">Launch **Jupyter notebook** for your HDInsight Spark cluster by navigating to: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="79cab-237">Is indítsa el a fürt-irányítópult a HDInsight Spark-fürt számára, majd indítsa el **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="79cab-237">You can also launch cluster dashboard for your HDInsight Spark cluster, and then launch **Jupyter Notebook**.</span></span>
2. <span data-ttu-id="79cab-238">Kattintson a **új** -> **PySpark** toostart egy újat.</span><span class="sxs-lookup"><span data-stu-id="79cab-238">Click **New** -> **PySpark** toostart a new notebook.</span></span>

    ![Új Jupyter notebook](media/data-factory-spark/jupyter-new-book.png)
3. <span data-ttu-id="79cab-240">Futtatási hello parancs következő másolás/beillesztés hello szöveget, majd nyomja le az **SHIFT + ENTER** hello második utasítás hello végén.</span><span class="sxs-lookup"><span data-stu-id="79cab-240">Run hello following command by copy/pasting hello text and pressing **SHIFT + ENTER** at hello end of hello second statement.</span></span>  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. <span data-ttu-id="79cab-241">Győződjön meg arról, hogy megjelenik-e hello hvac tábla hello adatait:</span><span class="sxs-lookup"><span data-stu-id="79cab-241">Confirm that you see hello data from hello hvac table:</span></span>  

    ![Jupyter lekérdezés eredményei](media/data-factory-spark/jupyter-notebook-results.png)

<span data-ttu-id="79cab-243">Lásd: [Spark SQL-lekérdezés futtatható](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) szakaszban részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="79cab-243">See [Run a Spark SQL query](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) section for detailed instructions.</span></span> 

### <a name="troubleshooting"></a><span data-ttu-id="79cab-244">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="79cab-244">Troubleshooting</span></span>
<span data-ttu-id="79cab-245">Óta beállított **getDebugInfo** túl**mindig**, megjelenik egy **napló** hello almappájában **pyFiles** mappa az Azure Blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="79cab-245">Since you set **getDebugInfo** too**Always**, you see a **log** subfolder in hello **pyFiles** folder in your Azure Blob container.</span></span> <span data-ttu-id="79cab-246">hello naplófájl hello napló mappában található további részleteket biztosít.</span><span class="sxs-lookup"><span data-stu-id="79cab-246">hello log file in hello log folder provides additional details.</span></span> <span data-ttu-id="79cab-247">Ez a naplófájl akkor különösen akkor hasznos, ha nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="79cab-247">This log file is especially useful when there is an error.</span></span> <span data-ttu-id="79cab-248">Termelési környezetben érdemes lehet a tooset azt túl**hiba**.</span><span class="sxs-lookup"><span data-stu-id="79cab-248">In a production environment, you may want tooset it too**Failure**.</span></span>

<span data-ttu-id="79cab-249">Az hello további hibaelhárítási, a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="79cab-249">For further troubleshooting, do hello following steps:</span></span>


1. <span data-ttu-id="79cab-250">Keresse meg a túl`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span><span class="sxs-lookup"><span data-stu-id="79cab-250">Navigate too`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span></span>

    ![Alkalmazás a YARN felhasználói felületen](media/data-factory-spark/yarnui-application.png)  
2. <span data-ttu-id="79cab-252">Kattintson a **naplók** egy hello futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="79cab-252">Click **Logs** for one of hello run attempts.</span></span>

    ![Alkalmazások lap](media/data-factory-spark/yarn-applications.png)
3. <span data-ttu-id="79cab-254">Meg kell jelennie a hibával kapcsolatos további információkat a hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="79cab-254">You should see additional error information in hello log page.</span></span>

    ![Naplózási hiba](media/data-factory-spark/yarnui-application-error.png)

<span data-ttu-id="79cab-256">a következő szakaszok hello adat-előállító entitások toouse Apache Spark-fürt és a data factory Spark tevékenység kapcsolatos adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="79cab-256">hello following sections provide information about Data Factory entities toouse Apache Spark cluster and Spark Activity in your data factory.</span></span>

## <a name="spark-activity-properties"></a><span data-ttu-id="79cab-257">A Spark-tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="79cab-257">Spark activity properties</span></span>
<span data-ttu-id="79cab-258">Hello minta JSON-definícióból Spark tevékenységet adatcsatorna itt található:</span><span class="sxs-lookup"><span data-stu-id="79cab-258">Here is hello sample JSON definition of a pipeline with Spark Activity:</span></span>    

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
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

<span data-ttu-id="79cab-259">hello következő táblázat hello JSON-definícióból használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="79cab-259">hello following table describes hello JSON properties used in hello JSON definition:</span></span>

| <span data-ttu-id="79cab-260">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="79cab-260">Property</span></span> | <span data-ttu-id="79cab-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="79cab-261">Description</span></span> | <span data-ttu-id="79cab-262">Szükséges</span><span class="sxs-lookup"><span data-stu-id="79cab-262">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="79cab-263">név</span><span class="sxs-lookup"><span data-stu-id="79cab-263">name</span></span> | <span data-ttu-id="79cab-264">Hello tevékenység hello folyamat nevét.</span><span class="sxs-lookup"><span data-stu-id="79cab-264">Name of hello activity in hello pipeline.</span></span> | <span data-ttu-id="79cab-265">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-265">Yes</span></span> |
| <span data-ttu-id="79cab-266">leírás</span><span class="sxs-lookup"><span data-stu-id="79cab-266">description</span></span> | <span data-ttu-id="79cab-267">Milyen hello tevékenységet leíró szöveg-e.</span><span class="sxs-lookup"><span data-stu-id="79cab-267">Text describing what hello activity does.</span></span> | <span data-ttu-id="79cab-268">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-268">No</span></span> |
| <span data-ttu-id="79cab-269">type</span><span class="sxs-lookup"><span data-stu-id="79cab-269">type</span></span> | <span data-ttu-id="79cab-270">Ez a tulajdonság tooHDInsightSpark kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="79cab-270">This property must be set tooHDInsightSpark.</span></span> | <span data-ttu-id="79cab-271">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-271">Yes</span></span> |
| <span data-ttu-id="79cab-272">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="79cab-272">linkedServiceName</span></span> | <span data-ttu-id="79cab-273">Hello HDInsight neve társított szolgáltatás mely hello Spark a program futtatása.</span><span class="sxs-lookup"><span data-stu-id="79cab-273">Name of hello HDInsight linked service on which hello Spark program runs.</span></span> | <span data-ttu-id="79cab-274">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-274">Yes</span></span> |
| <span data-ttu-id="79cab-275">rootPath</span><span class="sxs-lookup"><span data-stu-id="79cab-275">rootPath</span></span> | <span data-ttu-id="79cab-276">hello Azure blobtároló és hello Spark fájlt tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="79cab-276">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="79cab-277">hello fájlnév a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="79cab-277">hello file name is case-sensitive.</span></span> | <span data-ttu-id="79cab-278">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-278">Yes</span></span> |
| <span data-ttu-id="79cab-279">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="79cab-279">entryFilePath</span></span> | <span data-ttu-id="79cab-280">Relatív elérési út toohello gyökérmappájában hello Spark kódcsomag.</span><span class="sxs-lookup"><span data-stu-id="79cab-280">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="79cab-281">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-281">Yes</span></span> |
| <span data-ttu-id="79cab-282">Osztálynév</span><span class="sxs-lookup"><span data-stu-id="79cab-282">className</span></span> | <span data-ttu-id="79cab-283">Az alkalmazás Java/Spark fő osztály</span><span class="sxs-lookup"><span data-stu-id="79cab-283">Application's Java/Spark main class</span></span> | <span data-ttu-id="79cab-284">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-284">No</span></span> |
| <span data-ttu-id="79cab-285">Argumentumok</span><span class="sxs-lookup"><span data-stu-id="79cab-285">arguments</span></span> | <span data-ttu-id="79cab-286">Parancssori argumentumok toohello Spark program listáját.</span><span class="sxs-lookup"><span data-stu-id="79cab-286">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="79cab-287">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-287">No</span></span> |
| <span data-ttu-id="79cab-288">proxyUser</span><span class="sxs-lookup"><span data-stu-id="79cab-288">proxyUser</span></span> | <span data-ttu-id="79cab-289">hello felhasználói fiók tooimpersonate tooexecute hello Spark program</span><span class="sxs-lookup"><span data-stu-id="79cab-289">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="79cab-290">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-290">No</span></span> |
| <span data-ttu-id="79cab-291">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="79cab-291">sparkConfig</span></span> | <span data-ttu-id="79cab-292">Adja meg a Spark konfigurációs tulajdonságának hello témakörben felsorolt: [Spark konfigurációs - alkalmazás tulajdonságainak](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span><span class="sxs-lookup"><span data-stu-id="79cab-292">Specify values for Spark configuration properties listed in hello topic: [Spark Configuration - Application properties](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span></span> | <span data-ttu-id="79cab-293">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-293">No</span></span> |
| <span data-ttu-id="79cab-294">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="79cab-294">getDebugInfo</span></span> | <span data-ttu-id="79cab-295">Itt adhatja meg, amikor hello Spark naplófájlok másolt toohello az Azure storage HDInsight-fürt által használt (vagy) leírt módon sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="79cab-295">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="79cab-296">Megengedett értékek: None, mindig, vagy sikertelen.</span><span class="sxs-lookup"><span data-stu-id="79cab-296">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="79cab-297">Alapértelmezett érték: nincs.</span><span class="sxs-lookup"><span data-stu-id="79cab-297">Default value: None.</span></span> | <span data-ttu-id="79cab-298">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-298">No</span></span> |
| <span data-ttu-id="79cab-299">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="79cab-299">sparkJobLinkedService</span></span> | <span data-ttu-id="79cab-300">hello Azure tárolás társított szolgáltatása, amely tárolja a hello Spark feladat fájl, a függőségeket és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="79cab-300">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="79cab-301">Ha nem ad meg egy értéket ehhez a tulajdonsághoz, HDInsight-fürthöz társított hello tárolót használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="79cab-301">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="79cab-302">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-302">No</span></span> |

## <a name="folder-structure"></a><span data-ttu-id="79cab-303">Mappaszerkezet</span><span class="sxs-lookup"><span data-stu-id="79cab-303">Folder structure</span></span>
<span data-ttu-id="79cab-304">hello Spark tevékenység nem támogatja a Pig-egy sor parancsprogramot, és Hive tevékenységek tegye.</span><span class="sxs-lookup"><span data-stu-id="79cab-304">hello Spark activity does not support an in-line script as Pig and Hive activities do.</span></span> <span data-ttu-id="79cab-305">Spark feladatok akkor is több bővíthető, mint a Pig vagy Hive-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="79cab-305">Spark jobs are also more extensible than Pig/Hive jobs.</span></span> <span data-ttu-id="79cab-306">A Spark-feladatok biztosíthat több függőség például jar (hello java CLASSPATH helyezett) csomagok, python-fájlok (hello PYTHONPATH helyezve) és egyéb fájlokat.</span><span class="sxs-lookup"><span data-stu-id="79cab-306">For Spark jobs, you can provide multiple dependencies such as jar packages (placed in hello java CLASSPATH), python files (placed on hello PYTHONPATH), and any other files.</span></span>

<span data-ttu-id="79cab-307">A következő gyökérmappa-szerkezetében lévő hello Azure Blob Storage tárolóban hello kapcsolódó HDInsight-szolgáltatás által hivatkozott hello létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79cab-307">Create hello following folder structure in hello Azure Blob storage referenced by hello HDInsight linked service.</span></span> <span data-ttu-id="79cab-308">Ezt követően töltse fel a függő fájlokat toohello megfelelő almappák által képviselt hello gyökérmappában **entryFilePath**.</span><span class="sxs-lookup"><span data-stu-id="79cab-308">Then, upload dependent files toohello appropriate sub folders in hello root folder represented by **entryFilePath**.</span></span> <span data-ttu-id="79cab-309">Például feltöltése python fájlok toohello pyFiles almappát, és jar fájlok toohello JAR-fájlok kivételével hello legfelső szintű mappa almappája.</span><span class="sxs-lookup"><span data-stu-id="79cab-309">For example, upload python files toohello pyFiles subfolder and jar files toohello jars subfolder of hello root folder.</span></span> <span data-ttu-id="79cab-310">Futásidőben a Data Factory szolgáltatásnak vár hello hello Azure Blob Storage tárolót a mappastruktúra a következő:</span><span class="sxs-lookup"><span data-stu-id="79cab-310">At runtime, Data Factory service expects hello following folder structure in hello Azure Blob storage:</span></span>     

| <span data-ttu-id="79cab-311">Elérési út</span><span class="sxs-lookup"><span data-stu-id="79cab-311">Path</span></span> | <span data-ttu-id="79cab-312">Leírás</span><span class="sxs-lookup"><span data-stu-id="79cab-312">Description</span></span> | <span data-ttu-id="79cab-313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="79cab-313">Required</span></span> | <span data-ttu-id="79cab-314">Típus</span><span class="sxs-lookup"><span data-stu-id="79cab-314">Type</span></span> |
| ---- | ----------- | -------- | ---- |
| <span data-ttu-id="79cab-315">.</span><span class="sxs-lookup"><span data-stu-id="79cab-315">.</span></span> | <span data-ttu-id="79cab-316">hello gyökérútvonalát tekintve hello tárolás társított szolgáltatásának hello Spark feladat</span><span class="sxs-lookup"><span data-stu-id="79cab-316">hello root path of hello Spark job in hello storage linked service</span></span>    | <span data-ttu-id="79cab-317">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-317">Yes</span></span> | <span data-ttu-id="79cab-318">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-318">Folder</span></span> |
| <span data-ttu-id="79cab-319">&lt;felhasználó által definiált&gt;</span><span class="sxs-lookup"><span data-stu-id="79cab-319">&lt;user defined &gt;</span></span> | <span data-ttu-id="79cab-320">toohello bejegyzés fájl hello Spark feladat rendszerfájlokra hello elérési út</span><span class="sxs-lookup"><span data-stu-id="79cab-320">hello path pointing toohello entry file of hello Spark job</span></span> | <span data-ttu-id="79cab-321">Igen</span><span class="sxs-lookup"><span data-stu-id="79cab-321">Yes</span></span> | <span data-ttu-id="79cab-322">Fájl</span><span class="sxs-lookup"><span data-stu-id="79cab-322">File</span></span> |
| <span data-ttu-id="79cab-323">. / jars</span><span class="sxs-lookup"><span data-stu-id="79cab-323">./jars</span></span> | <span data-ttu-id="79cab-324">Ebben a mappában található összes fájl feltöltése és hello java classpath hello fürt helyezve</span><span class="sxs-lookup"><span data-stu-id="79cab-324">All files under this folder are uploaded and placed on hello java classpath of hello cluster</span></span> | <span data-ttu-id="79cab-325">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-325">No</span></span> | <span data-ttu-id="79cab-326">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-326">Folder</span></span> |
| <span data-ttu-id="79cab-327">. / pyFiles</span><span class="sxs-lookup"><span data-stu-id="79cab-327">./pyFiles</span></span> | <span data-ttu-id="79cab-328">Ebben a mappában található összes fájl feltöltése és hello hello fürt PYTHONPATH helyezve</span><span class="sxs-lookup"><span data-stu-id="79cab-328">All files under this folder are uploaded and placed on hello PYTHONPATH of hello cluster</span></span> | <span data-ttu-id="79cab-329">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-329">No</span></span> | <span data-ttu-id="79cab-330">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-330">Folder</span></span> |
| <span data-ttu-id="79cab-331">. / fájlok</span><span class="sxs-lookup"><span data-stu-id="79cab-331">./files</span></span> | <span data-ttu-id="79cab-332">Ebben a mappában található összes fájl feltöltése és végrehajtó munkakönyvtár helyezve</span><span class="sxs-lookup"><span data-stu-id="79cab-332">All files under this folder are uploaded and placed on executor working directory</span></span> | <span data-ttu-id="79cab-333">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-333">No</span></span> | <span data-ttu-id="79cab-334">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-334">Folder</span></span> |
| <span data-ttu-id="79cab-335">. / archiválja</span><span class="sxs-lookup"><span data-stu-id="79cab-335">./archives</span></span> | <span data-ttu-id="79cab-336">Ebben a mappában található összes fájl tömörítetlen.</span><span class="sxs-lookup"><span data-stu-id="79cab-336">All files under this folder are uncompressed</span></span> | <span data-ttu-id="79cab-337">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-337">No</span></span> | <span data-ttu-id="79cab-338">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-338">Folder</span></span> |
| <span data-ttu-id="79cab-339">. / naplói</span><span class="sxs-lookup"><span data-stu-id="79cab-339">./logs</span></span> | <span data-ttu-id="79cab-340">hello származó Spark-fürt hello naplók tároló mappa.</span><span class="sxs-lookup"><span data-stu-id="79cab-340">hello folder where logs from hello Spark cluster are stored.</span></span>| <span data-ttu-id="79cab-341">Nem</span><span class="sxs-lookup"><span data-stu-id="79cab-341">No</span></span> | <span data-ttu-id="79cab-342">Mappa</span><span class="sxs-lookup"><span data-stu-id="79cab-342">Folder</span></span> |

<span data-ttu-id="79cab-343">Íme egy példa egy tárolási hello Azure Blob Storage HDInsight kapcsolódó szolgáltatás hello által hivatkozott két Spark feladat fájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="79cab-343">Here is an example for a storage containing two Spark job files in hello Azure Blob Storage referenced by hello HDInsight linked service.</span></span>

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
