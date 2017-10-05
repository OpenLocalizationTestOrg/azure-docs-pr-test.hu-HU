---
title: "Az első data factory létrehozása (Azure Portal) | Microsoft Docs"
description: "Az oktatóanyag során létre fog hozni egy minta Azure Data Factory-folyamatot az Azure Portal Data Factory Editor eszközével."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9c958aecb841fa02349c6b9e5e1984f6ba4fb611
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="584b0-103">Oktatóanyag: az első Azure data factory létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="584b0-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="584b0-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="584b0-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="584b0-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="584b0-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="584b0-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="584b0-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="584b0-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="584b0-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="584b0-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="584b0-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="584b0-109">REST API</span><span class="sxs-lookup"><span data-stu-id="584b0-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="584b0-110">Ez a cikk bemutatja, hogyan hozhatja létre az első Azure-os adat-előállítóját az [Azure Portal](https://portal.azure.com/) használatával.</span><span class="sxs-lookup"><span data-stu-id="584b0-110">In this article, you learn how to use [Azure portal](https://portal.azure.com/) to create your first Azure data factory.</span></span> <span data-ttu-id="584b0-111">Ha ezt az oktatóanyagot más eszközök/SDK-k használatával szeretné elvégezni, válassza ki az egyik lehetőséget a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="584b0-111">To do the tutorial using other tools/SDKs, select one of the options from the drop-down list.</span></span> 

<span data-ttu-id="584b0-112">A jelen oktatóanyagban szereplő folyamat egyetlen tevékenységet tartalmaz: ez a **HDInsight Hive-tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="584b0-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="584b0-113">A tevékenység egy hive-szkriptet futtat egy Azure HDInsight fürtön, amely a bemeneti adatokat átalakítja a kimeneti adatok előállításához.</span><span class="sxs-lookup"><span data-stu-id="584b0-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="584b0-114">A folyamat úgy van ütemezve, hogy havonta egyszer fusson a megadott kezdő és befejező időpontok közt.</span><span class="sxs-lookup"><span data-stu-id="584b0-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="584b0-115">Az oktatóanyagban található adatfolyamat átalakítja a bemeneti adatokat, hogy ezzel kimeneti adatokat hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="584b0-115">The data pipeline in this tutorial transforms input data to produce output data.</span></span> <span data-ttu-id="584b0-116">Az adatok Azure Data Factory használatával történő másolásának útmutatásáért olvassa el [az adatok Blob Storage-ból SQL Database-be történő másolását ismertető oktatóanyagot](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="584b0-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="584b0-117">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="584b0-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="584b0-118">Ezenkívül össze is fűzhet két tevékenységet (egymás után futtathatja őket), ha az egyik tevékenység kimeneti adatkészletét a másik tevékenység bemeneti adatkészleteként állítja be.</span><span class="sxs-lookup"><span data-stu-id="584b0-118">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="584b0-119">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="584b0-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="584b0-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="584b0-120">Prerequisites</span></span>
1. <span data-ttu-id="584b0-121">Olvassa el [Az oktatóanyag áttekintése](data-factory-build-your-first-pipeline.md) című cikket, és hajtsa végre az **előfeltételként** felsorolt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="584b0-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span>
2. <span data-ttu-id="584b0-122">Ez a cikk nem nyújt fogalmi áttekintést az Azure Data Factory szolgáltatásról.</span><span class="sxs-lookup"><span data-stu-id="584b0-122">This article does not provide a conceptual overview of the Azure Data Factory service.</span></span> <span data-ttu-id="584b0-123">Javasoljuk, hogy a szolgáltatás részletes áttekintéséhez olvassa el az [Introduction to Azure Data Factory](data-factory-introduction.md) (Az Azure Data Factory bemutatása) című cikket.</span><span class="sxs-lookup"><span data-stu-id="584b0-123">We recommend that you go through [Introduction to Azure Data Factory](data-factory-introduction.md) article for a detailed overview of the service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="584b0-124">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-124">Create data factory</span></span>
<span data-ttu-id="584b0-125">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="584b0-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="584b0-126">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="584b0-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="584b0-127">Például egy olyan másolási tevékenység, amely adatokat másol egy forrásadattárból egy céladattárba, és egy HDInsight Hive-tevékenység, amely egy Hive-szkript futtatásával alakítja át a bemeneti adatokat kimeneti adatokká.</span><span class="sxs-lookup"><span data-stu-id="584b0-127">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="584b0-128">Ebben a lépésben létrehozzuk a data factoryt.</span><span class="sxs-lookup"><span data-stu-id="584b0-128">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="584b0-129">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="584b0-129">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="584b0-130">Kattintson a **NEW** (Új) lehetőségre a bal oldali menüben, kattintson az **Data + Analytics** (Adatok + analitika) pontra, majd a **Data Factory** elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-130">Click **NEW** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![A Create (Létrehozás) panel](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="584b0-132">A **New data factory** (Új data factory) panelen adja meg a **GetStartedDF** nevet.</span><span class="sxs-lookup"><span data-stu-id="584b0-132">In the **New data factory** blade, enter **GetStartedDF** for the Name.</span></span>

   ![A New data factory (Új data factory) panel](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="584b0-134">Az Azure data factory nevének **globálisan egyedinek** kell lennie.</span><span class="sxs-lookup"><span data-stu-id="584b0-134">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="584b0-135">Ha a **Data factory name “GetStartedDF” is not available** (A „GetStartedDF” data factory-név nem érhető el) hibaüzenetet kapja:</span><span class="sxs-lookup"><span data-stu-id="584b0-135">If you receive the error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="584b0-136">Módosítsa a nevet (például sajátnévGetStartedDF-re) és próbálkozzon újra a létrehozással.</span><span class="sxs-lookup"><span data-stu-id="584b0-136">Change the name of the data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="584b0-137">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="584b0-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="584b0-138">A data factory neve később **DNS**-névként regisztrálható, így nyilvánosan láthatóvá válhat.</span><span class="sxs-lookup"><span data-stu-id="584b0-138">The name of the data factory may be registered as a **DNS** name in the future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="584b0-139">Jelölje ki azt az **Azure-előfizetést**, ahol létre szeretné hozni a data factoryt.</span><span class="sxs-lookup"><span data-stu-id="584b0-139">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="584b0-140">Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="584b0-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="584b0-141">Az oktatóanyag elvégzéséhez hozzon létre egy erőforráscsoportot a következő névvel: **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="584b0-141">For the tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="584b0-142">Válassza ki a Data Factory **helyét**.</span><span class="sxs-lookup"><span data-stu-id="584b0-142">Select the **location** for the data factory.</span></span> <span data-ttu-id="584b0-143">A legördülő listában csak a Data Factory szolgáltatás által támogatott régiók jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-143">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
7. <span data-ttu-id="584b0-144">Válassza a **Rögzítés az irányítópulton** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="584b0-144">Select **Pin to dashboard**.</span></span> 
8. <span data-ttu-id="584b0-145">Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.</span><span class="sxs-lookup"><span data-stu-id="584b0-145">Click **Create** on the **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="584b0-146">Data Factory-példány létrehozásához a [Data Factory közreműködője](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör tagjának kell lennie az előfizetés/erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="584b0-146">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="584b0-147">Az irányítópulton megjelenő csempén a következő állapotleírás látható: Data Factory üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="584b0-147">On the dashboard, you see the following tile with status: Deploying data factory.</span></span>    

   ![A data factory létrehozásának állapota](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="584b0-149">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="584b0-149">Congratulations!</span></span> <span data-ttu-id="584b0-150">Sikeresen létrehozta első data factoryjét.</span><span class="sxs-lookup"><span data-stu-id="584b0-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="584b0-151">A data factory sikeres létrehozása után megjelenik a data factory oldal, amely megjeleníti a data factory tartalmát.</span><span class="sxs-lookup"><span data-stu-id="584b0-151">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>     

    ![A Data Factory panel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="584b0-153">A data factoryban a folyamat létrehozása előtt először létre kell hoznia néhány Data Factory-entitást.</span><span class="sxs-lookup"><span data-stu-id="584b0-153">Before creating a pipeline in the data factory, you need to create a few Data Factory entities first.</span></span> <span data-ttu-id="584b0-154">Először társított szolgáltatásokat kell létrehoznia, amelyek adattárakat/számítási szolgáltatásokat társítanak az adattárhoz, majd bemeneti és kimeneti adatkészleteket kell meghatároznia, amelyek a társított adattárakban lévő bemeneti/kimeneti adatokat képviselik, végül létrehozhatja a folyamatot egy olyan tevékenységgel, amely ezeket az adatkészleteket használja.</span><span class="sxs-lookup"><span data-stu-id="584b0-154">You first create linked services to link data stores/computes to your data store, define input and output datasets to represent input/output data in linked data stores, and then create the pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="584b0-155">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-155">Create linked services</span></span>
<span data-ttu-id="584b0-156">Ebben a lépésben az Azure Storage-fiókját és egy igény szerinti Azure HDInsight-fürtöt társít az adat-előállítóhoz.</span><span class="sxs-lookup"><span data-stu-id="584b0-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster to your data factory.</span></span> <span data-ttu-id="584b0-157">Ebben a példában az Azure Storage-fiók a bemeneti és a kimeneti adatokat tárolja a folyamathoz.</span><span class="sxs-lookup"><span data-stu-id="584b0-157">The Azure Storage account holds the input and output data for the pipeline in this sample.</span></span> <span data-ttu-id="584b0-158">A HDInsight társított szolgáltatás a mintában szereplő folyamat tevékenységében meghatározott Hive-szkriptet futtatja.</span><span class="sxs-lookup"><span data-stu-id="584b0-158">The HDInsight linked service is used to run a Hive script specified in the activity of the pipeline in this sample.</span></span> <span data-ttu-id="584b0-159">Határozza meg, hogy mely [adattárat](data-factory-data-movement-activities.md)/[számítási szolgáltatásokat](data-factory-compute-linked-services.md) használja a forgatókönyvben, és társítsa ezeket a szolgáltatások a data factoryhoz társított szolgáltatások létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="584b0-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services to the data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="584b0-160">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="584b0-161">Ebben a lépésben társítja az Azure Storage-fiókot a data factoryjához.</span><span class="sxs-lookup"><span data-stu-id="584b0-161">In this step, you link your Azure Storage account to your data factory.</span></span> <span data-ttu-id="584b0-162">A jelen oktatóanyag esetében ugyanazt az Azure Storage-fiókot fogja használni a bemeneti/kimeneti adatok és a HQL-parancsfájl tárolásához.</span><span class="sxs-lookup"><span data-stu-id="584b0-162">In this tutorial, you use the same Azure Storage account to store input/output data and the HQL script file.</span></span>

1. <span data-ttu-id="584b0-163">A **GetStartedDF** **DATA FACTORY** panelén kattintson az **Author and deploy** (Fejlesztés és üzembe helyezés) elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-163">Click **Author and deploy** on the **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="584b0-164">Ekkor meg kell jelennie a Data Factory Editornak.</span><span class="sxs-lookup"><span data-stu-id="584b0-164">You should see the Data Factory Editor.</span></span>

   ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="584b0-166">Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.</span><span class="sxs-lookup"><span data-stu-id="584b0-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Új adattár – Azure Storage – menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="584b0-168">A szerkesztőben megjelenik az Azure Storage társított szolgáltatás létrehozására szolgáló JSON-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="584b0-168">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="584b0-170">Az **account name** kifejezést cserélje az Azure Storage-fiókja nevére, az **account key** kifejezést pedig az Azure Storage-fiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="584b0-170">Replace **account name** with the name of your Azure storage account and **account key** with the access key of the Azure storage account.</span></span> <span data-ttu-id="584b0-171">A tárelérési kulcs lekéréséről többet is megtudhat, ha elolvassa a tárelérési kulcsok megtekintésével, másolásával és újragenerálásával kapcsolatos információkat [A tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account) című részben.</span><span class="sxs-lookup"><span data-stu-id="584b0-171">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="584b0-172">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="584b0-172">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![A Deploy (Üzembe helyezés) gomb](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="584b0-174">A társított szolgáltatás sikeres üzembe helyezését követően megjelenik a **Draft-1** (Vázlat-1) ablak, amelynek bal oldalán, fanézetben látható az **AzureStorageLinkedService** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="584b0-174">After the linked service is deployed successfully, the **Draft-1** window should disappear and you see **AzureStorageLinkedService** in the tree view on the left.</span></span>

    ![Storage társított szolgáltatás a menüben](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="584b0-176">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="584b0-177">Ebben a lépésben egy igény szerinti HDInsight-fürtöt társít a data factoryhoz.</span><span class="sxs-lookup"><span data-stu-id="584b0-177">In this step, you link an on-demand HDInsight cluster to your data factory.</span></span> <span data-ttu-id="584b0-178">A HDInsight-fürtöt a rendszer automatikusan létrehozza a futásidő során, majd törli a feldolgozás befejezését követően, miután egy adott ideig tétlen volt.</span><span class="sxs-lookup"><span data-stu-id="584b0-178">The HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for the specified amount of time.</span></span>

1. <span data-ttu-id="584b0-179">A **Data Factory Editorban** kattintson ide: **... More** (... További), kattintson a **New compute** (Új számítási példány) elemre, majd válassza az **On-demand HDInsight cluster** (Igény szerinti HDInsight-fürt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="584b0-179">In the **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![A New compute (Új számítás) elem](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="584b0-181">Másolja és illessze be a következő kódrészletet a **Draft-1** (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="584b0-181">Copy and paste the following snippet to the **Draft-1** window.</span></span> <span data-ttu-id="584b0-182">A JSON-kódrészlet megadja az igény szerinti HDInsight-fürt létrehozásához használt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="584b0-182">The JSON snippet describes the properties that are used to create the HDInsight cluster on-demand.</span></span>

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    <span data-ttu-id="584b0-183">Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="584b0-183">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="584b0-184">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="584b0-184">Property</span></span> | <span data-ttu-id="584b0-185">Leírás</span><span class="sxs-lookup"><span data-stu-id="584b0-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="584b0-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="584b0-186">ClusterSize</span></span> |<span data-ttu-id="584b0-187">Megadja a HDInsight-fürt méretét.</span><span class="sxs-lookup"><span data-stu-id="584b0-187">Specifies the size of the HDInsight cluster.</span></span> |
   | <span data-ttu-id="584b0-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="584b0-188">TimeToLive</span></span> | <span data-ttu-id="584b0-189">Megadja, hogy a HDInsight-fürt mennyi ideig lehet tétlen, mielőtt törölné a rendszer.</span><span class="sxs-lookup"><span data-stu-id="584b0-189">Specifies that the idle time for the HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="584b0-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="584b0-190">linkedServiceName</span></span> | <span data-ttu-id="584b0-191">Ez megadja a HDInsight által előállított naplók tárolására szolgáló tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="584b0-191">Specifies the storage account that is used to store the logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="584b0-192">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="584b0-192">Note the following points:</span></span>

   * <span data-ttu-id="584b0-193">A Data Factory létrehoz egy **Linux-alapú** HDInsight-fürtöt a JSON-fájllal.</span><span class="sxs-lookup"><span data-stu-id="584b0-193">The Data Factory creates a **Linux-based** HDInsight cluster for you with the JSON.</span></span> <span data-ttu-id="584b0-194">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="584b0-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="584b0-195">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="584b0-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="584b0-196">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="584b0-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="584b0-197">A HDInsight-fürt létrehoz egy **alapértelmezett tárolót** a JSON-fájlban megadott blob-tárolóban (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="584b0-197">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="584b0-198">A fürt törlésekor a HDInsight nem törli ezt a tárolót.</span><span class="sxs-lookup"><span data-stu-id="584b0-198">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="584b0-199">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="584b0-199">This behavior is by design.</span></span> <span data-ttu-id="584b0-200">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="584b0-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="584b0-201">A fürt automatikusan törlődik a feldolgozás megtörténtekor.</span><span class="sxs-lookup"><span data-stu-id="584b0-201">The cluster is automatically deleted when the processing is done.</span></span>

       <span data-ttu-id="584b0-202">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="584b0-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="584b0-203">Ha nincs szüksége rájuk a feladatokkal kapcsolatos hibaelhárításhoz, törölheti őket a tárolási költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="584b0-203">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="584b0-204">A tárolók neve a következő mintát követi: „adf**yourdatafactoryname**-**linkedservicename**-datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="584b0-204">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="584b0-205">Az Azure Blob Storage-tárból olyan eszközökkel törölheti a tárolókat, mint például a [Microsoft Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="584b0-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="584b0-206">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="584b0-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="584b0-207">A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="584b0-207">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![Igény szerinti HDInsight társított szolgáltatás üzembe helyezése](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="584b0-209">Győződjön meg arról, hogy a bal oldali fanézetben megjelenik az **AzureStorageLinkedService** és a **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="584b0-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in the tree view on the left.</span></span>

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="584b0-211">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-211">Create datasets</span></span>
<span data-ttu-id="584b0-212">Ebben a lépésben adatkészleteket hoz létre, amelyek a Hive-feldolgozás bemeneti és kimeneti adatait képviselik.</span><span class="sxs-lookup"><span data-stu-id="584b0-212">In this step, you create datasets to represent the input and output data for Hive processing.</span></span> <span data-ttu-id="584b0-213">Ezek az adatkészletek az oktatóanyag során korábban létrehozott **AzureStorageLinkedService** szolgáltatásra hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="584b0-213">These datasets refer to the **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="584b0-214">A társított szolgáltatás egy Azure Storage-fiókra mutat, az adatkészletek pedig meghatározzák a bemeneti és kimeneti adatokat tartalmazó tárban lévő tárolót, mappát és fájlnevet.</span><span class="sxs-lookup"><span data-stu-id="584b0-214">The linked service points to an Azure Storage account and datasets specify container, folder, file name in the storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="584b0-215">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-215">Create input dataset</span></span>
1. <span data-ttu-id="584b0-216">A **Data Factory Editorban** kattintson ide: **... More** (... További) a parancssávon, kattintson a **New dataset** (Új adathalmaz) elemre, és válassza az **Azure Blob Storage** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="584b0-216">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![New dataset (Új adatkészlet)](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="584b0-218">Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="584b0-218">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="584b0-219">A JSON-kódrészletben hozza létre az **AzureBlobInput** nevű adatkészletet, amely a folyamat egyik tevékenységének bemeneti adatait képviseli.</span><span class="sxs-lookup"><span data-stu-id="584b0-219">In the JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in the pipeline.</span></span> <span data-ttu-id="584b0-220">Emellett határozza meg azt is, hogy a bemeneti adatok az **adfgetstarted** nevű blob-tárolóban és az **inputdata** nevű mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="584b0-220">In addition, you specify that the input data is located in the blob container called **adfgetstarted** and the folder called **inputdata**.</span></span>

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    <span data-ttu-id="584b0-221">Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="584b0-221">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="584b0-222">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="584b0-222">Property</span></span> | <span data-ttu-id="584b0-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="584b0-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="584b0-224">type</span><span class="sxs-lookup"><span data-stu-id="584b0-224">type</span></span> |<span data-ttu-id="584b0-225">A tulajdonság beállításának értéke **AzureBlob**, mert az adatok egy Azure Blob Storage-tárban találhatók.</span><span class="sxs-lookup"><span data-stu-id="584b0-225">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="584b0-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="584b0-226">linkedServiceName</span></span> |<span data-ttu-id="584b0-227">A korábban létrehozott **AzureStorageLinkedService** szolgáltatásra vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="584b0-227">Refers to the **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="584b0-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="584b0-228">folderPath</span></span> | <span data-ttu-id="584b0-229">A bemeneti blobokat tartalmazó **blobtárolót** és **mappát** adja meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-229">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="584b0-230">fileName</span><span class="sxs-lookup"><span data-stu-id="584b0-230">fileName</span></span> |<span data-ttu-id="584b0-231">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="584b0-231">This property is optional.</span></span> <span data-ttu-id="584b0-232">Ha kihagyja, az összes fájl ki lesz választva a folderPath útvonalról.</span><span class="sxs-lookup"><span data-stu-id="584b0-232">If you omit this property, all the files from the folderPath are picked.</span></span> <span data-ttu-id="584b0-233">Ebben az oktatóanyagban csak az **input.log** feldolgozása történik meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-233">In this tutorial, only the **input.log** is processed.</span></span> |
   | <span data-ttu-id="584b0-234">type</span><span class="sxs-lookup"><span data-stu-id="584b0-234">type</span></span> |<span data-ttu-id="584b0-235">Mivel a naplófájlok szövegformátumúak, a **TextFormat** típust kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="584b0-235">The log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="584b0-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="584b0-236">columnDelimiter</span></span> |<span data-ttu-id="584b0-237">A naplófájlokban lévő oszlopok **vesszővel (`,`)** vannak elválasztva.</span><span class="sxs-lookup"><span data-stu-id="584b0-237">columns in the log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="584b0-238">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="584b0-238">frequency/interval</span></span> |<span data-ttu-id="584b0-239">A frequency (gyakoriság) beállítás értéke **Month** (Hónap), az interval (időköz) beállítás értéke **1**, tehát a bemeneti szeletek havonta érhetők el.</span><span class="sxs-lookup"><span data-stu-id="584b0-239">frequency set to **Month** and interval is **1**, which means that the input slices are available monthly.</span></span> |
   | <span data-ttu-id="584b0-240">external</span><span class="sxs-lookup"><span data-stu-id="584b0-240">external</span></span> | <span data-ttu-id="584b0-241">Ez a tulajdonság a **true** (igaz) értékre van állítva, ha a bemeneti adatokat nem ez a folyamat hozta létre.</span><span class="sxs-lookup"><span data-stu-id="584b0-241">This property is set to **true** if the input data is not generated by this pipeline.</span></span> <span data-ttu-id="584b0-242">Mivel ebben az oktatóanyagban a bemeneti naplófájlt nem ez a folyamat hozta létre, a tulajdonság értéke „igaz”.</span><span class="sxs-lookup"><span data-stu-id="584b0-242">In this tutorial, the input.log file is not generated by this pipeline, so we set the property to true.</span></span> |

    <span data-ttu-id="584b0-243">Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.</span><span class="sxs-lookup"><span data-stu-id="584b0-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="584b0-244">Az újonnan létrehozott adatkészlet üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="584b0-244">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span> <span data-ttu-id="584b0-245">Az adatkészletnek meg kell jelennie a bal oldali fanézetben.</span><span class="sxs-lookup"><span data-stu-id="584b0-245">You should see the dataset in the tree view on the left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="584b0-246">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-246">Create output dataset</span></span>
<span data-ttu-id="584b0-247">Most a kimeneti adatkészletet hozza létre, amely az Azure Blob Storage-tárban tárolt kimeneti adatokat jelöli.</span><span class="sxs-lookup"><span data-stu-id="584b0-247">Now, you create the output dataset to represent the output data stored in the Azure Blob storage.</span></span>

1. <span data-ttu-id="584b0-248">A **Data Factory Editorban** kattintson ide: **... More** (... További) a parancssávon, kattintson a **New dataset** (Új adathalmaz) elemre, és válassza az **Azure Blob Storage** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="584b0-248">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="584b0-249">Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="584b0-249">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="584b0-250">A JSON-kódrészletben hozza létre az **AzureBlobOutput** nevű adatkészletet, és határozza meg a Hive-parancsfájl által előállított adatok szerkezetét.</span><span class="sxs-lookup"><span data-stu-id="584b0-250">In the JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying the structure of the data that is produced by the Hive script.</span></span> <span data-ttu-id="584b0-251">Emellett határozza meg azt is, hogy az eredmények tárolása az **adfgetstarted** nevű blob-tárolóban és a **partitioneddata** nevű mappában történjen.</span><span class="sxs-lookup"><span data-stu-id="584b0-251">In addition, you specify that the results are stored in the blob container called **adfgetstarted** and the folder called **partitioneddata**.</span></span> <span data-ttu-id="584b0-252">Az **availability** (rendelkezésre állás) szakasz meghatározza, hogy a kimeneti adatkészlet előállítása havonta történik.</span><span class="sxs-lookup"><span data-stu-id="584b0-252">The **availability** section specifies that the output dataset is produced on a monthly basis.</span></span>

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    <span data-ttu-id="584b0-253">A tulajdonságok leírását a **Bemeneti adatkészlet létrehozása** című szakaszban tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-253">See **Create the input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="584b0-254">Külső adatkészlet esetén nem kell beállítani az external (külső) tulajdonságot, mert az adatkészletet a Data Factory szolgáltatás állítja elő.</span><span class="sxs-lookup"><span data-stu-id="584b0-254">You do not set the external property on an output dataset as the dataset is produced by the Data Factory service.</span></span>
3. <span data-ttu-id="584b0-255">Az újonnan létrehozott adatkészlet üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="584b0-255">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span>
4. <span data-ttu-id="584b0-256">Ellenőrizze az adatkészlet létrehozása sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="584b0-256">Verify that the dataset is created successfully.</span></span>

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="584b0-258">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="584b0-258">Create pipeline</span></span>
<span data-ttu-id="584b0-259">Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="584b0-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="584b0-260">A bemeneti szelet havonta érhető el (frequency: Month, interval: 1), a kimeneti szelet előállítása havonta történik, és a tevékenység scheduler (ütemező) tulajdonsága szintén a hónap értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="584b0-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and the scheduler property for the activity is also set to monthly.</span></span> <span data-ttu-id="584b0-261">A kimeneti adatkészlet és a tevékenységütemező beállításainak egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="584b0-261">The settings for the output dataset and the activity scheduler must match.</span></span> <span data-ttu-id="584b0-262">Jelenleg a kimeneti adatkészlet vezérli az ütemezést, ezért kimeneti adatkészletet akkor is létre kell hoznia, ha a tevékenység nem állít elő semmilyen kimenetet.</span><span class="sxs-lookup"><span data-stu-id="584b0-262">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="584b0-263">Ha a tevékenység nem fogad semmilyen bemenetet, kihagyhatja a bemeneti adatkészlet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="584b0-263">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> <span data-ttu-id="584b0-264">Az alábbi JSON-fájlban használt tulajdonságok magyarázata a szakasz végén található.</span><span class="sxs-lookup"><span data-stu-id="584b0-264">The properties used in the following JSON are explained at the end of this section.</span></span>

1. <span data-ttu-id="584b0-265">A **Data Factory Editorban** kattintson a **további parancsokat jelölő három pontra (...)**, majd kattintson a **New pipeline** (Új folyamat) elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-265">In the **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![A New pipeline (Új folyamat) gomb](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="584b0-267">Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba.</span><span class="sxs-lookup"><span data-stu-id="584b0-267">Copy and paste the following snippet to the Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="584b0-268">A JSON-fájlban cserélje a **storageaccountname** kifejezést a tárfiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="584b0-268">Replace **storageaccountname** with the name of your storage account in the JSON.</span></span>
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    <span data-ttu-id="584b0-269">A JSON-kódrészletben létrehoz egy folyamatot, amely egyetlen tevékenységből áll, és a tevékenység a Hive használatával dolgozza fel az adatokat egy HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="584b0-269">In the JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive to process Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="584b0-270">A **partitionweblogs.hql** Hive-parancsfájl tárolása az Azure Storage-fiókban (az **AzureStorageLinkedService** nevű scriptLinkedService szolgáltatás által megadva), és az **adfgetstarted** tároló **script** mappájában történik.</span><span class="sxs-lookup"><span data-stu-id="584b0-270">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in the container **adfgetstarted**.</span></span>

    <span data-ttu-id="584b0-271">A **defines** (meghatározza) szakasz meghatározza a futásidő beállításait, amelyek Hive konfigurációs értékekként (például ${hiveconf:inputtable}, ${hiveconf:partitionedtable}) lesznek átadva a Hive-parancsfájlnak.</span><span class="sxs-lookup"><span data-stu-id="584b0-271">The **defines** section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="584b0-272">A folyamat **start** (kezdés) és **end** (befejezés) tulajdonságai a folyamat aktív időszakát határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-272">The **start** and **end** properties of the pipeline specifies the active period of the pipeline.</span></span>

    <span data-ttu-id="584b0-273">A tevékenység JSON-fájljában meg van határozva, hogy a Hive-parancsfájl a **linkedServiceName** – **HDInsightOnDemandLinkedService** által meghatározott számítási szolgáltatáson fut.</span><span class="sxs-lookup"><span data-stu-id="584b0-273">In the activity JSON, you specify that the Hive script runs on the compute specified by the **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="584b0-274">A példában használt JSON-tulajdonságokkal kapcsolatos részletekért lásd „A folyamat JSON-fájlja” szakaszt a [Folyamatok és tevékenységek az Azure Data Factoryban](data-factory-create-pipelines.md) című témakörben.</span><span class="sxs-lookup"><span data-stu-id="584b0-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in the example.</span></span>
   >
   >
3. <span data-ttu-id="584b0-275">Ellenőrizze az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="584b0-275">Confirm the following:</span></span>

   1. <span data-ttu-id="584b0-276">Az **input.log** fájl létezik az Azure blob-tároló **adfgetstarted** tárolójának **inputdata** mappájában.</span><span class="sxs-lookup"><span data-stu-id="584b0-276">**input.log** file exists in the **inputdata** folder of the **adfgetstarted** container in the Azure blob storage</span></span>
   2. <span data-ttu-id="584b0-277">A **partitionweblogs.hql** fájl létezik az Azure blob-tároló **adfgetstarted** tárolójának **script** mappájában.</span><span class="sxs-lookup"><span data-stu-id="584b0-277">**partitionweblogs.hql** file exists in the **script** folder of the **adfgetstarted** container in the Azure blob storage.</span></span> <span data-ttu-id="584b0-278">Ha nem látja ezeket a fájlokat, hajtsa végre [Az oktatóanyag áttekintése](data-factory-build-your-first-pipeline.md) című cikkben előfeltételként felsorolt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="584b0-278">Complete the prerequisite steps in the [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="584b0-279">Ellenőrizze, hogy a folyamat JSON-fájljában lecserélte-e a **storageaccountname** kifejezést a tárfiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="584b0-279">Confirm that you replaced **storageaccountname** with the name of your storage account in the pipeline JSON.</span></span>
4. <span data-ttu-id="584b0-280">A folyamat üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.</span><span class="sxs-lookup"><span data-stu-id="584b0-280">Click **Deploy** on the command bar to deploy the pipeline.</span></span> <span data-ttu-id="584b0-281">Mivel a **start** (kezdés) és az **end** (befejezés) időpontok múltbeli értékekre vannak beállítva, és az **isPaused** tulajdonság értéke false (hamis), a folyamat (a folyamatban foglalt tevékenység) az üzembe helyezés után azonnal fut.</span><span class="sxs-lookup"><span data-stu-id="584b0-281">Since the **start** and **end** times are set in the past and **isPaused** is set to false, the pipeline (activity in the pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="584b0-282">Győződjön meg arról, hogy a folyamat megjelenik a fanézetben.</span><span class="sxs-lookup"><span data-stu-id="584b0-282">Confirm that you see the pipeline in the tree view.</span></span>

    ![A fanézet a folyamattal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="584b0-284">Gratulálunk, sikeresen létrehozta első folyamatát!</span><span class="sxs-lookup"><span data-stu-id="584b0-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="584b0-285">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="584b0-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="584b0-286">Folyamat figyelése diagramnézetben</span><span class="sxs-lookup"><span data-stu-id="584b0-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="584b0-287">A Data Factory Editor paneljeinek a bezárásához és a Data Factory panelre való visszatéréshez kattintson az **X**, majd a **Diagram** elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-287">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="584b0-289">A diagramnézet áttekintést nyújt az oktatóanyagban használt folyamatokról és adatkészletekről.</span><span class="sxs-lookup"><span data-stu-id="584b0-289">In the Diagram View, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="584b0-291">A folyamat összes tevékenységének megtekintéséhez kattintson a jobb gombbal a folyamatra a diagramban, majd kattintson a Feldolgozási sor megnyitása elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-291">To view all activities in the pipeline, right-click pipeline in the diagram and click Open Pipeline.</span></span>

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="584b0-293">Győződjön meg arról, hogy a HDInsightHive tevékenység megjelenik a folyamatban.</span><span class="sxs-lookup"><span data-stu-id="584b0-293">Confirm that you see the HDInsightHive activity in the pipeline.</span></span>

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="584b0-295">Az előző nézethez való visszatéréshez az oldal tetején lévő navigációs menüben kattintson a **Data factory** elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-295">To navigate back to the previous view, click **Data factory** in the breadcrumb menu at the top.</span></span>
5. <span data-ttu-id="584b0-296">A **diagramnézetben** kattintson duplán az **AzureBlobInput** adatkészletre.</span><span class="sxs-lookup"><span data-stu-id="584b0-296">In the **Diagram View**, double-click the dataset **AzureBlobInput**.</span></span> <span data-ttu-id="584b0-297">Győződjön meg arról, hogy a szelet **Ready** (Kész) állapotú.</span><span class="sxs-lookup"><span data-stu-id="584b0-297">Confirm that the slice is in **Ready** state.</span></span> <span data-ttu-id="584b0-298">Eltarthat néhány percig, amíg a szelet Ready (Kész) állapotúként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="584b0-298">It may take a couple of minutes for the slice to show up in Ready state.</span></span> <span data-ttu-id="584b0-299">Ha ez azután sem történik meg, hogy vár néhány percet, ellenőrizze, hogy a megfelelő tárolóba (adfgetstarted) és mappába (inputdata) helyezte-e a bemeneti fájlt (input.log).</span><span class="sxs-lookup"><span data-stu-id="584b0-299">If it does not happen after you wait for sometime, see if you have the input file (input.log) placed in the right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="584b0-301">Kattintson az **X** elemre az **AzureBlobInput** panel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="584b0-301">Click **X** to close **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="584b0-302">A **diagramnézetben** kattintson duplán az **AzureBlobOutput** adatkészletre.</span><span class="sxs-lookup"><span data-stu-id="584b0-302">In the **Diagram View**, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="584b0-303">Látni fogja, hogy a szelet feldolgozás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="584b0-303">You see that the slice that is currently being processed.</span></span>

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="584b0-305">A feldolgozás befejeztével a szelet **Ready** (Kész) állapotúra vált.</span><span class="sxs-lookup"><span data-stu-id="584b0-305">When processing is done, you see the slice in **Ready** state.</span></span>

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="584b0-307">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="584b0-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="584b0-308">Ezért a folyamat várhatóan       **körülbelül 30 perc** alatt dolgozza fel a szeletet.</span><span class="sxs-lookup"><span data-stu-id="584b0-308">Therefore, expect the pipeline to       take **approximately 30 minutes** to process the slice.</span></span>
   >
   >

9. <span data-ttu-id="584b0-309">Ha a szelet **Ready** (Kész) állapotú, a kimeneti adatok **adfgetstarted** Blob Storage-tárolójában ellenőrizze a **partitioneddata** mappát.</span><span class="sxs-lookup"><span data-stu-id="584b0-309">When the slice is in **Ready** state, check the **partitioneddata** folder in the **adfgetstarted** container in your blob storage for the output data.</span></span>  

   ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="584b0-311">A szelet részleteinek az **Adatszelet** panelen való megtekintéséhez kattintson a szeletre.</span><span class="sxs-lookup"><span data-stu-id="584b0-311">Click the slice to see details about it in a **Data slice** blade.</span></span>

   ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="584b0-313">Kattintson egy tevékenységfuttatásra az **Activity runs list** (Tevékenységfuttatások listája) területen a tevékenységfuttatás (ebben az esetben Hive-tevékenység) részleteinek az **Activity run details** (Tevékenységfuttatás részletei) ablakban való megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="584b0-313">Click an activity run in the **Activity runs list** to see details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="584b0-315">A naplófájlokban láthatja a végrehajtott Hive-lekérdezést és az állapotadatokat.</span><span class="sxs-lookup"><span data-stu-id="584b0-315">From the log files, you can see the Hive query that was executed and status information.</span></span> <span data-ttu-id="584b0-316">A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.</span><span class="sxs-lookup"><span data-stu-id="584b0-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="584b0-317">További részletekért tekintse meg a [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) (Folyamatok figyelése és felügyelete az Azure Portal paneleinek használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="584b0-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="584b0-318">A szelet sikeres feldolgozásakor a rendszer törli a bemeneti fájlt.</span><span class="sxs-lookup"><span data-stu-id="584b0-318">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="584b0-319">Ezért ha újra le szeretné futtatni a szeletet, vagy újra el szeretné végezni az oktatóanyagban foglaltakat, töltse fel a bemeneti fájlt (input.log) az adfgetstarted tároló inputdata mappájába.</span><span class="sxs-lookup"><span data-stu-id="584b0-319">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the inputdata folder of the adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="584b0-320">Folyamat figyelése a Monitor & Manage alkalmazással</span><span class="sxs-lookup"><span data-stu-id="584b0-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="584b0-321">A folyamatok figyeléséhez a Monitor & Manage alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="584b0-321">You can also use Monitor & Manage application to monitor your pipelines.</span></span> <span data-ttu-id="584b0-322">Az alkalmazás használatával kapcsolatos részletes információkért tekintse meg a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="584b0-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="584b0-323">Kattintson a **Monitor & Manage** csempére a Data Factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="584b0-323">Click **Monitor & Manage** tile on the home page for your data factory.</span></span>

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="584b0-325">Meg kell jelennie a **Monitor & Manage alkalmazásnak**.</span><span class="sxs-lookup"><span data-stu-id="584b0-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="584b0-326">Módosítsa a **kezdési idő** és a **befejezési idő** értékét, hogy megfeleljen a folyamat kezdési és befejezési idejének, és kattintson az **Apply** (Alkalmaz) elemre.</span><span class="sxs-lookup"><span data-stu-id="584b0-326">Change the **Start time** and **End time** to match start and end times of your pipeline, and click **Apply**.</span></span>

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="584b0-328">Válasszon egy tevékenységablakot az **Activity Windows** (Tevékenységablakok) listában a részleteinek a megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="584b0-328">Select an activity window in the **Activity Windows** list to see details about it.</span></span>

    ![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="584b0-330">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="584b0-330">Summary</span></span>
<span data-ttu-id="584b0-331">Az oktatóanyag során létrehozott egy Azure data factoryt, amely egy HDInsight Hadoop-fürtön futtatott Hive-parancsfájllal dolgozza fel az adatokat.</span><span class="sxs-lookup"><span data-stu-id="584b0-331">In this tutorial, you created an Azure data factory to process data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="584b0-332">Az Azure Portal Data Factory Editor eszközét használta a következő lépések végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="584b0-332">You used the Data Factory Editor in the Azure portal to do the following steps:</span></span>  

1. <span data-ttu-id="584b0-333">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="584b0-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="584b0-334">Létrehozott két **társított szolgáltatást**:</span><span class="sxs-lookup"><span data-stu-id="584b0-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="584b0-335">Az **Azure Storage** társított szolgáltatást, amely a bemeneti és kimeneti adatokat tároló Azure blob-tárolót társítja a data factoryval.</span><span class="sxs-lookup"><span data-stu-id="584b0-335">**Azure Storage** linked service to link your Azure blob storage that holds input/output files to the data factory.</span></span>
   2. <span data-ttu-id="584b0-336">Az **Azure HDInsight** igény szerinti társított szolgáltatást, amely egy igény szerinti HDInsight Hadoop-fürtöt társít a data factoryval.</span><span class="sxs-lookup"><span data-stu-id="584b0-336">**Azure HDInsight** on-demand linked service to link an on-demand HDInsight Hadoop cluster to the data factory.</span></span> <span data-ttu-id="584b0-337">Az Azure Data Factory létrehoz egy HDInsight Hadoop-fürtöt, amely igény szerint dolgozza fel a bemeneti adatokat és állítja elő a kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="584b0-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time to process input data and produce output data.</span></span>
3. <span data-ttu-id="584b0-338">Létrehozott két **adatkészletet**, amelyek leírják a bemeneti és kimeneti adatokat az adatcsatorna HDInsight Hive-tevékenysége számára.</span><span class="sxs-lookup"><span data-stu-id="584b0-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in the pipeline.</span></span>
4. <span data-ttu-id="584b0-339">Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="584b0-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="584b0-340">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="584b0-340">Next Steps</span></span>
<span data-ttu-id="584b0-341">Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="584b0-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="584b0-342">Ha tudni szeretné, hogyan használhatja a Másolás tevékenységet az adatok Azure-blobból Azure SQL Database adatbázisba történő másolásához, tekintse meg a következő cikket: [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Oktatóanyag: adatok másolása Azure-blobból Azure SQL Database adatbázisba).</span><span class="sxs-lookup"><span data-stu-id="584b0-342">To see how to use a Copy Activity to copy data from an Azure Blob to Azure SQL, see [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="584b0-343">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="584b0-343">See Also</span></span>
| <span data-ttu-id="584b0-344">Témakör</span><span class="sxs-lookup"><span data-stu-id="584b0-344">Topic</span></span> | <span data-ttu-id="584b0-345">Leírás</span><span class="sxs-lookup"><span data-stu-id="584b0-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="584b0-346">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="584b0-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="584b0-347">Ennek a cikknek a segítségével megismerheti a Azure Data Factory folyamatait és tevékenységeit, és megtudhatja, hogyan hozhat létre velük teljes körű, adatvezérelt munkafolyamatokat saját forgatókönyvéhez vagy vállalkozásához.</span><span class="sxs-lookup"><span data-stu-id="584b0-347">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="584b0-348">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="584b0-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="584b0-349">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="584b0-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="584b0-350">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="584b0-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="584b0-351">Ez a cikk ismerteti az Azure Data Factory-alkalmazásmodell ütemezési és végrehajtási aspektusait.</span><span class="sxs-lookup"><span data-stu-id="584b0-351">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="584b0-352">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="584b0-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="584b0-353">Ez a cikk ismerteti, hogyan figyelheti és felügyelheti a folyamatokat, illetve hogyan kereshet bennük hibákat a Monitoring & Management App használatával.</span><span class="sxs-lookup"><span data-stu-id="584b0-353">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |
