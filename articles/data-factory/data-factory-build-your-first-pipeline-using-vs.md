---
title: "aaaBuild az első adat-előállítóban (Visual Studio) |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy mintául szolgáló Azure Data Factory-folyamatot a Visual Studio használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a><span data-ttu-id="4fa59-103">Oktatóanyag: adat-előállító létrehozása a Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="4fa59-103">Tutorial: Create a data factory by using Visual Studio</span></span>
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [<span data-ttu-id="4fa59-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4fa59-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="4fa59-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4fa59-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="4fa59-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4fa59-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="4fa59-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4fa59-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="4fa59-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="4fa59-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="4fa59-109">REST API</span><span class="sxs-lookup"><span data-stu-id="4fa59-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)

<span data-ttu-id="4fa59-110">Az oktatóanyag bemutatja, hogyan toocreate egy az Azure data factory Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="4fa59-110">This tutorial shows you how toocreate an Azure data factory by using Visual Studio.</span></span> <span data-ttu-id="4fa59-111">Hello adat-előállító projektsablon használatával Visual Studio-projekt létrehozása, JSON formátumban adja meg a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és pipeline) és majd közzététele/telepítheti ezeket entitások toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="4fa59-111">You create a Visual Studio project using hello Data Factory project template, define Data Factory entities (linked services, datasets, and pipeline) in JSON format, and then publish/deploy these entities toohello cloud.</span></span> 

<span data-ttu-id="4fa59-112">Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="4fa59-113">Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="4fa59-114">hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.</span><span class="sxs-lookup"><span data-stu-id="4fa59-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fa59-115">Ez az oktatóanyag nem tartalmazza az adatok Azure Data Factory használatával történő másolásának leírását.</span><span class="sxs-lookup"><span data-stu-id="4fa59-115">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="4fa59-116">Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4fa59-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="4fa59-117">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="4fa59-118">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="4fa59-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="4fa59-119">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="4fa59-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="walkthrough-create-and-publish-data-factory-entities"></a><span data-ttu-id="4fa59-120">Útmutató: Data Factory-entitások létrehozása és közzététele</span><span class="sxs-lookup"><span data-stu-id="4fa59-120">Walkthrough: Create and publish Data Factory entities</span></span>
<span data-ttu-id="4fa59-121">Ez a forgatókönyv során végrehajtandó hello lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="4fa59-121">Here are hello steps you perform as part of this walkthrough:</span></span>

1. <span data-ttu-id="4fa59-122">Létrehozza a következő két társított szolgáltatást: **AzureStorageLinkedService1** és **HDInsightOnDemandLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-122">Create two linked services: **AzureStorageLinkedService1** and **HDInsightOnDemandLinkedService1**.</span></span> 
   
    <span data-ttu-id="4fa59-123">Ebben az oktatóanyagban a bemeneti és kimeneti adatok a hello hive tevékenység a hello azonos Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="4fa59-123">In this tutorial, both input and output data for hello hive activity are in hello same Azure Blob Storage.</span></span> <span data-ttu-id="4fa59-124">Egy igény szerinti HDInsight fürt tooprocess meglévő bemeneti adatok tooproduce kimeneti adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="4fa59-124">You use an on-demand HDInsight cluster tooprocess existing input data tooproduce output data.</span></span> <span data-ttu-id="4fa59-125">hello igény szerinti HDInsight-fürt automatikusan létrejön az Ön által az Azure Data Factory futási időben feldolgozott készen toobe hello bemeneti adatok esetén.</span><span class="sxs-lookup"><span data-stu-id="4fa59-125">hello on-demand HDInsight cluster is automatically created for you by Azure Data Factory at run time when hello input data is ready toobe processed.</span></span> <span data-ttu-id="4fa59-126">Az adatokat tárolja, vagy tooyour adat-előállító kiszámítja, hogy hello Data Factory szolgáltatásnak csatlakozhassanak a futási időben toothem toolink van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4fa59-126">You need toolink your data stores or computes tooyour data factory so that hello Data Factory service can connect toothem at runtime.</span></span> <span data-ttu-id="4fa59-127">Ezért akkor az Azure Storage-fiók toohello adat-előállító hello AzureStorageLinkedService1 használatával hivatkozásra, és hello HDInsightOnDemandLinkedService1 segítségével igény szerinti HDInsight-fürtök hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="4fa59-127">Therefore, you link your Azure Storage Account toohello data factory by using hello AzureStorageLinkedService1, and link an on-demand HDInsight cluster by using hello HDInsightOnDemandLinkedService1.</span></span> <span data-ttu-id="4fa59-128">Közzétételekor, hello data factory toobe létrehozott hello nevét vagy valamelyik adat-előállítót adja meg.</span><span class="sxs-lookup"><span data-stu-id="4fa59-128">When publishing, you specify hello name for hello data factory toobe created or an existing data factory.</span></span>  
2. <span data-ttu-id="4fa59-129">Hozzon létre két adatkészletet: **InputDataset** és **OutputDataset**, amely képviseli hello Azure blob Storage tárolóban tárolt hello bemeneti/kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-129">Create two datasets: **InputDataset** and **OutputDataset**, which represent hello input/output data that is stored in hello Azure blob storage.</span></span> 
   
    <span data-ttu-id="4fa59-130">A dataset definíciók tekintse meg a hello előző lépésben létrehozott toohello Azure tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="4fa59-130">These dataset definitions refer toohello Azure Storage linked service you created in hello previous step.</span></span> <span data-ttu-id="4fa59-131">A hello InputDataset hello blob-tároló (adfgetstarted) megadása, és egy blobot a hello bemeneti adatokat tartalmazó mappát (inptutdata) hello.</span><span class="sxs-lookup"><span data-stu-id="4fa59-131">For hello InputDataset, you specify hello blob container (adfgetstarted) and hello folder (inptutdata) that contains a blob with hello input data.</span></span> <span data-ttu-id="4fa59-132">A hello OutputDataset hello blob tároló (adfgetstarted) ad meg, és hello mappa (partitioneddata), amely tartalmazza a hello kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-132">For hello OutputDataset, you specify hello blob container (adfgetstarted) and hello folder (partitioneddata) that holds hello output data.</span></span> <span data-ttu-id="4fa59-133">Egyéb tulajdonságokat is megad, például a szerkezetet, rendelkezésre állást és a szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="4fa59-133">You also specify other properties such as structure, availability, and policy.</span></span>
3. <span data-ttu-id="4fa59-134">Hozzon létre egy folyamatot **MyFirstPipeline** néven.</span><span class="sxs-lookup"><span data-stu-id="4fa59-134">Create a pipeline named **MyFirstPipeline**.</span></span> 
  
    <span data-ttu-id="4fa59-135">Ebben a bemutatóban hello folyamat csak egy tevékenység rendelkezik: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-135">In this walkthrough, hello pipeline has only one activity: **HDInsight Hive Activity**.</span></span> <span data-ttu-id="4fa59-136">A tevékenység transzformáció adatok tooproduce kimeneti adatokat adjon meg egy igény szerinti HDInsight-fürt hive-parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="4fa59-136">This activity transform input data tooproduce output data by running a hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4fa59-137">További információ a hive tevékenység toolearn lásd: [Hive tevékenység](data-factory-hive-activity.md)</span><span class="sxs-lookup"><span data-stu-id="4fa59-137">toolearn more about hive activity, see [Hive Activity](data-factory-hive-activity.md)</span></span> 
4. <span data-ttu-id="4fa59-138">Létrehoz egy **DataFactoryUsingVS** nevű data factoryt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-138">Create a data factory named **DataFactoryUsingVS**.</span></span> <span data-ttu-id="4fa59-139">Telepítse a hello data factory és az összes adat-előállító entitások (a társított szolgáltatások, a táblák és a hello folyamat).</span><span class="sxs-lookup"><span data-stu-id="4fa59-139">Deploy hello data factory and all Data Factory entities (linked services, tables, and hello pipeline).</span></span>
5. <span data-ttu-id="4fa59-140">Miután közzéteszi, az Azure portál paneleken és figyelés & a felügyeleti alkalmazás toomonitor hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-140">After you publish, you use Azure portal blades and Monitoring & Management App toomonitor hello pipeline.</span></span> 
  
### <a name="prerequisites"></a><span data-ttu-id="4fa59-141">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4fa59-141">Prerequisites</span></span>
1. <span data-ttu-id="4fa59-142">Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-142">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span> <span data-ttu-id="4fa59-143">Igény szerint kiválaszthatja hello **áttekintése és előfeltételek** beállítás hello felső tooswitch toohello cikk hello legördülő listáról.</span><span class="sxs-lookup"><span data-stu-id="4fa59-143">You can also select hello **Overview and prerequisites** option in hello drop-down list at hello top tooswitch toohello article.</span></span> <span data-ttu-id="4fa59-144">Hello Előfeltételek befejezése után váltson vissza toothis cikk kiválasztásával **Visual Studio** hello legördülő lista beállítását.</span><span class="sxs-lookup"><span data-stu-id="4fa59-144">After you complete hello prerequisites, switch back toothis article by selecting **Visual Studio** option in hello drop-down list.</span></span>
2. <span data-ttu-id="4fa59-145">toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="4fa59-145">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>  
3. <span data-ttu-id="4fa59-146">A számítógépen telepített hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="4fa59-146">You must have hello following installed on your computer:</span></span>
   * <span data-ttu-id="4fa59-147">Visual Studio 2013 vagy Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="4fa59-147">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="4fa59-148">Töltse le az Azure SDK-t a Visual Studio 2013-hoz vagy a Visual Studio 2015-höz.</span><span class="sxs-lookup"><span data-stu-id="4fa59-148">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="4fa59-149">Keresse meg a túl[Azure letöltési oldalát](https://azure.microsoft.com/downloads/) kattintson **VS 2013** vagy **VS 2015** a hello **.NET** szakasz.</span><span class="sxs-lookup"><span data-stu-id="4fa59-149">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="4fa59-150">Töltse le a legfrissebb hello Azure Data Factory beépülő modul a Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="4fa59-150">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="4fa59-151">Hello beépülő modult is frissítheti hello lépések végrehajtásával: hello menüjének **eszközök** -> **bővítmények és frissítések** -> **Online**  ->  **Visual Studio galériában** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-151">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

<span data-ttu-id="4fa59-152">Most tegyük a Visual Studio toocreate egy az Azure data factory használja.</span><span class="sxs-lookup"><span data-stu-id="4fa59-152">Now, let's use Visual Studio toocreate an Azure data factory.</span></span>

### <a name="create-visual-studio-project"></a><span data-ttu-id="4fa59-153">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="4fa59-154">Indítsa el a **Visual Studio 2013-at** vagy a **Visual Studio 2015-öt**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-154">Launch **Visual Studio 2013** or **Visual Studio 2015**.</span></span> <span data-ttu-id="4fa59-155">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="4fa59-156">Megtekintheti az hello **új projekt** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="4fa59-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="4fa59-157">A hello **új projekt** párbeszédpanelen jelölje be hello **DataFactory** sablont, majd kattintson **üres Data Factory-projektekhez**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>   

    ![A New project (Új projekt) párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. <span data-ttu-id="4fa59-159">Adja meg egy **neve** hello projekt **hely**, és hello nevét **megoldás**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-159">Enter a **name** for hello project, **location**, and a name for hello **solution**, and click **OK**.</span></span>

    ![Megoldáskezelő](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a><span data-ttu-id="4fa59-161">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-161">Create linked services</span></span>
<span data-ttu-id="4fa59-162">Ebben a lépésben létrehozza a következő két társított szolgáltatást: **Azure Storage** és **igény szerinti HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-162">In this step, you create two linked services: **Azure Storage** and **HDInsight on-demand**.</span></span> 

<span data-ttu-id="4fa59-163">hello Azure Storage az Azure Storage-fiók toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat, adja meg a hello kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-163">hello Azure Storage linked service links your Azure Storage account toohello data factory by providing hello connection information.</span></span> <span data-ttu-id="4fa59-164">Data Factory szolgáltatásnak hello kapcsolati karakterláncot, kapcsolódó hello szolgáltatásbeállítás tooconnect toohello az Azure storage futtatás közben használ.</span><span class="sxs-lookup"><span data-stu-id="4fa59-164">Data Factory service uses hello connection string from hello linked service setting tooconnect toohello Azure storage at runtime.</span></span> <span data-ttu-id="4fa59-165">Ez a tároló tárolja a bemeneti és kimeneti adatok hello pipeline és hello hive parancsfájl hello hive tevékenység által használt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-165">This storage holds input and output data for hello pipeline, and hello hive script file used by hello hive activity.</span></span> 

<span data-ttu-id="4fa59-166">Igény szerinti HDInsight kapcsolódó szolgáltatás, a HDInsight-fürt hello automatikusan megtörténik futásidőben hello bemeneti adata készen áll a tooprocessed.</span><span class="sxs-lookup"><span data-stu-id="4fa59-166">With on-demand HDInsight linked service, hello HDInsight cluster is automatically created at runtime when hello input data is ready tooprocessed.</span></span> <span data-ttu-id="4fa59-167">hello fürtök törlése után feldolgozásra és üresjárati hello megadott időtartamig.</span><span class="sxs-lookup"><span data-stu-id="4fa59-167">hello cluster is deleted after it is done processing and idle for hello specified amount of time.</span></span> 

> [!NOTE]
> <span data-ttu-id="4fa59-168">Létrehozhat egy adat-előállító hello helyreállításkor közzététele a Data Factory megoldás nevét és beállítások megadásával.</span><span class="sxs-lookup"><span data-stu-id="4fa59-168">You create a data factory by specifying its name and settings at hello time of publishing your Data Factory solution.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="4fa59-169">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-169">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="4fa59-170">Kattintson a jobb gombbal **összekapcsolt szolgáltatások** hello megoldáskezelőben pontot túl**Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-170">Right-click **Linked Services** in hello solution explorer, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="4fa59-171">A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Storage társított szolgáltatás** hello listából, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-171">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span>
    <span data-ttu-id="4fa59-172">![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="4fa59-172">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span></span>
3. <span data-ttu-id="4fa59-173">Cserélje le `<accountname>` és `<accountkey>` hello nevű Azure-tárfiókot és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="4fa59-173">Replace `<accountname>` and `<accountkey>` with hello name of your Azure storage account and its key.</span></span> <span data-ttu-id="4fa59-174">toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="4fa59-174">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
    <span data-ttu-id="4fa59-175">![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="4fa59-175">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span></span>
4. <span data-ttu-id="4fa59-176">Mentse a hello **AzureStorageLinkedService1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-176">Save hello **AzureStorageLinkedService1.json** file.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="4fa59-177">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-177">Create Azure HDInsight linked service</span></span>
1. <span data-ttu-id="4fa59-178">A hello **Solution Explorer**, kattintson a jobb gombbal **összekapcsolt szolgáltatások**, pont túl**Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-178">In hello **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4fa59-179">Válassza a **HDInsight On Demand Linked Service** (HDInsight igény szerinti társított szolgáltatás) lehetőséget, és kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="4fa59-179">Select **HDInsight On Demand Linked Service**, and click **Add**.</span></span>
3. <span data-ttu-id="4fa59-180">Cserélje le a hello **JSON** a következő JSON hello:</span><span class="sxs-lookup"><span data-stu-id="4fa59-180">Replace hello **JSON** with hello following JSON:</span></span>

     ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
        "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    <span data-ttu-id="4fa59-181">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="4fa59-181">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="4fa59-182">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4fa59-182">Property</span></span> | <span data-ttu-id="4fa59-183">Leírás</span><span class="sxs-lookup"><span data-stu-id="4fa59-183">Description</span></span>
    -------- | ----------- 
    <span data-ttu-id="4fa59-184">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="4fa59-184">ClusterSize</span></span> | <span data-ttu-id="4fa59-185">HDInsight Hadoop-fürt hello hello méretét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4fa59-185">Specifies hello size of hello HDInsight Hadoop cluster.</span></span>
    <span data-ttu-id="4fa59-186">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="4fa59-186">TimeToLive</span></span> | <span data-ttu-id="4fa59-187">Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-187">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span>
    <span data-ttu-id="4fa59-188">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="4fa59-188">linkedServiceName</span></span> | <span data-ttu-id="4fa59-189">Megadja a hello tárfiókja, amely a HDInsight Hadoop-fürt által létrehozott használt toostore hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-189">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight Hadoop cluster.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="4fa59-190">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** hello hello JSON (linkedServiceName) a megadott blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-190">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="4fa59-191">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="4fa59-191">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="4fa59-192">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="4fa59-192">This behavior is by design.</span></span> <span data-ttu-id="4fa59-193">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="4fa59-193">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="4fa59-194">hello feldolgozása hello fürt automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="4fa59-194">hello cluster is automatically deleted when hello processing is done.</span></span>
    > 
    > <span data-ttu-id="4fa59-195">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-195">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="4fa59-196">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-196">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="4fa59-197">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-197">hello names of these containers follow a pattern: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span></span> <span data-ttu-id="4fa59-198">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="4fa59-198">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

    <span data-ttu-id="4fa59-199">A JSON-tulajdonságokkal kapcsolatos további információkért lásd a [Társított szolgáltatások számítása](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) című cikket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-199">For more information about JSON properties, see [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article.</span></span> 
4. <span data-ttu-id="4fa59-200">Mentse a hello **HDInsightOnDemandLinkedService1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-200">Save hello **HDInsightOnDemandLinkedService1.json** file.</span></span>

### <a name="create-datasets"></a><span data-ttu-id="4fa59-201">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-201">Create datasets</span></span>
<span data-ttu-id="4fa59-202">Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="4fa59-202">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="4fa59-203">Ezek az adatkészletek tekintse meg a toohello **AzureStorageLinkedService1** Ez az oktatóanyag korábbi részében létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4fa59-203">These datasets refer toohello **AzureStorageLinkedService1** you have created earlier in this tutorial.</span></span> <span data-ttu-id="4fa59-204">hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-204">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

#### <a name="create-input-dataset"></a><span data-ttu-id="4fa59-205">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-205">Create input dataset</span></span>
1. <span data-ttu-id="4fa59-206">A hello **megoldáskezelő**, kattintson a jobb gombbal **táblák**, pont túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-206">In hello **Solution Explorer**, right-click **Tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4fa59-207">Válassza ki **Azure Blob** hello listából hello nevének módosítása hello fájl túl**InputDataSet.json**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-207">Select **Azure Blob** from hello list, change hello name of hello file too**InputDataSet.json**, and click **Add**.</span></span>
3. <span data-ttu-id="4fa59-208">Cserélje le a hello **JSON** hello szerkesztőben a következő JSON részlet hello:</span><span class="sxs-lookup"><span data-stu-id="4fa59-208">Replace hello **JSON** in hello editor with hello following JSON snippet:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    <span data-ttu-id="4fa59-209">A JSON-részlet nevű adatkészlet meghatározása **AzureBlobInput** képviselő hello hive tevékenység hello folyamat a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="4fa59-209">This JSON snippet defines a dataset called **AzureBlobInput** that represents input data for hello hive activity in hello pipeline.</span></span> <span data-ttu-id="4fa59-210">Megadja, hogy hello bemeneti adatok nevű hello blob tárolóban található `adfgetstarted` és nevű hello mappát `inputdata`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-210">You specify that hello input data is located in hello blob container called `adfgetstarted` and hello folder called `inputdata`.</span></span>

    <span data-ttu-id="4fa59-211">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="4fa59-211">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="4fa59-212">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4fa59-212">Property</span></span> | <span data-ttu-id="4fa59-213">Leírás</span><span class="sxs-lookup"><span data-stu-id="4fa59-213">Description</span></span> |
    -------- | ----------- |
    <span data-ttu-id="4fa59-214">type</span><span class="sxs-lookup"><span data-stu-id="4fa59-214">type</span></span> |<span data-ttu-id="4fa59-215">hello type tulajdonság beállítása túl**AzureBlob** adatokat az Azure Blob Storage helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="4fa59-215">hello type property is set too**AzureBlob** because data resides in Azure Blob Storage.</span></span>
    <span data-ttu-id="4fa59-216">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="4fa59-216">linkedServiceName</span></span> | <span data-ttu-id="4fa59-217">A korábban létrehozott AzureStorageLinkedService1 toohello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="4fa59-217">Refers toohello AzureStorageLinkedService1 you created earlier.</span></span>
    <span data-ttu-id="4fa59-218">fileName</span><span class="sxs-lookup"><span data-stu-id="4fa59-218">fileName</span></span> |<span data-ttu-id="4fa59-219">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="4fa59-219">This property is optional.</span></span> <span data-ttu-id="4fa59-220">Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz.</span><span class="sxs-lookup"><span data-stu-id="4fa59-220">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="4fa59-221">Ebben az esetben csak hello input.log dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="4fa59-221">In this case, only hello input.log is processed.</span></span>
    <span data-ttu-id="4fa59-222">type</span><span class="sxs-lookup"><span data-stu-id="4fa59-222">type</span></span> | <span data-ttu-id="4fa59-223">hello naplófájlok szöveges formátumú, így szöveges használjuk.</span><span class="sxs-lookup"><span data-stu-id="4fa59-223">hello log files are in text format, so we use TextFormat.</span></span> |
    <span data-ttu-id="4fa59-224">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="4fa59-224">columnDelimiter</span></span> | <span data-ttu-id="4fa59-225">oszlopok hello naplófájlokban határolja hello vesszővel karakter (`,`)</span><span class="sxs-lookup"><span data-stu-id="4fa59-225">columns in hello log files are delimited by hello comma character (`,`)</span></span>
    <span data-ttu-id="4fa59-226">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="4fa59-226">frequency/interval</span></span> | <span data-ttu-id="4fa59-227">gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi.</span><span class="sxs-lookup"><span data-stu-id="4fa59-227">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span>
    <span data-ttu-id="4fa59-228">external</span><span class="sxs-lookup"><span data-stu-id="4fa59-228">external</span></span> | <span data-ttu-id="4fa59-229">Ez a tulajdonság beállítása tootrue hello bemeneti adatok hello tevékenység hello az adatcsatorna nem jön létre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-229">This property is set tootrue if hello input data for hello activity is not generated by hello pipeline.</span></span> <span data-ttu-id="4fa59-230">Ez a tulajdonság csak a bemeneti adatkészleteken van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="4fa59-230">This property is only specified on input datasets.</span></span> <span data-ttu-id="4fa59-231">Hello bemeneti adatkészlet hello első tevékenység mindig állítani tootrue.</span><span class="sxs-lookup"><span data-stu-id="4fa59-231">For hello input dataset of hello first activity, always set it tootrue.</span></span>
4. <span data-ttu-id="4fa59-232">Mentse a hello **InputDataset.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-232">Save hello **InputDataset.json** file.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="4fa59-233">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-233">Create output dataset</span></span>
<span data-ttu-id="4fa59-234">Hello kimeneti adatkészlet toorepresent kimeneti tárolt adatok hello Azure Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-234">Now, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="4fa59-235">A hello **megoldáskezelő**, kattintson a jobb gombbal **táblák**, pont túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-235">In hello **Solution Explorer**, right-click **tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4fa59-236">Válassza ki **Azure Blob** hello listából hello nevének módosítása hello fájl túl**OutputDataset.json**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-236">Select **Azure Blob** from hello list, change hello name of hello file too**OutputDataset.json**, and click **Add**.</span></span>
3. <span data-ttu-id="4fa59-237">Cserélje le a hello **JSON** hello szerkesztőben a következő JSON hello:</span><span class="sxs-lookup"><span data-stu-id="4fa59-237">Replace hello **JSON** in hello editor with hello following JSON:</span></span>
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    <span data-ttu-id="4fa59-238">hello JSON részlet nevű adatkészlet meghatározása **AzureBlobOutput** , hogy jelöli kimeneti hello folyamat hello hive tevékenység által létrehozott adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-238">hello JSON snippet defines a dataset called **AzureBlobOutput** that represents output data produced by hello hive activity in hello pipeline.</span></span> <span data-ttu-id="4fa59-239">Megadja, hogy hello kimeneti adatok hozzák hello hive tevékenység hello blob tároló nevű kerül `adfgetstarted` és nevű hello mappát `partitioneddata`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-239">You specify that hello output data is produced by hello hive activity is placed in hello blob container called `adfgetstarted` and hello folder called `partitioneddata`.</span></span> 
    
    <span data-ttu-id="4fa59-240">Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-240">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span> <span data-ttu-id="4fa59-241">hello kimeneti adatkészlet meghajtók hello ütemezés hello folyamatának.</span><span class="sxs-lookup"><span data-stu-id="4fa59-241">hello output dataset drives hello schedule of hello pipeline.</span></span> <span data-ttu-id="4fa59-242">hello folyamat fut havi a kezdő és záró időpont között.</span><span class="sxs-lookup"><span data-stu-id="4fa59-242">hello pipeline runs monthly between its start and end times.</span></span> 

    <span data-ttu-id="4fa59-243">Lásd: **létrehozni hello bemeneti adatkészletet** szakasz ezeket a tulajdonságokat leírását.</span><span class="sxs-lookup"><span data-stu-id="4fa59-243">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="4fa59-244">Be kell állítani nem hello külső tulajdonság egy kimeneti adatkészletet, hello dataset hozzák hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-244">You do not set hello external property on an output dataset as hello dataset is produced by hello pipeline.</span></span>
4. <span data-ttu-id="4fa59-245">Mentse a hello **OutputDataset.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-245">Save hello **OutputDataset.json** file.</span></span>

### <a name="create-pipeline"></a><span data-ttu-id="4fa59-246">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fa59-246">Create pipeline</span></span>
<span data-ttu-id="4fa59-247">Hello Azure tárolás társított szolgáltatása, és a bemeneti és kimeneti adatkészletek eddig hozott létre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-247">You have created hello Azure Storage linked service, and input and output datasets so far.</span></span> <span data-ttu-id="4fa59-248">Most létrehozhat egy folyamatot egy **HDInsightHive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="4fa59-248">Now, you create a pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="4fa59-249">Hello **bemeneti** a hello hive tevékenység túl van beállítva**AzureBlobInput** és **kimeneti** értéke túl**AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-249">hello **input** for hello hive activity is set too**AzureBlobInput** and **output** is set too**AzureBlobOutput**.</span></span> <span data-ttu-id="4fa59-250">A szelet egy bemeneti adatkészlet érhető el havonta (gyakoriság: hónap, időköz: 1), és hello kimeneti szelet jön létre havonta túl.</span><span class="sxs-lookup"><span data-stu-id="4fa59-250">A slice of an input dataset is available monthly (frequency: Month, interval: 1), and hello output slice is produced monthly too.</span></span> 

1. <span data-ttu-id="4fa59-251">A hello **Solution Explorer**, kattintson a jobb gombbal **folyamatok**, pont túl**hozzáadása**, és kattintson a **új cikk.**</span><span class="sxs-lookup"><span data-stu-id="4fa59-251">In hello **Solution Explorer**, right-click **Pipelines**, point too**Add**, and click **New Item.**</span></span>
2. <span data-ttu-id="4fa59-252">Válassza ki **Hive átalakítási folyamat** hello listából, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-252">Select **Hive Transformation Pipeline** from hello list, and click **Add**.</span></span>
3. <span data-ttu-id="4fa59-253">Cserélje le a hello **JSON** a hello a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="4fa59-253">Replace hello **JSON** with hello following snippet:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4fa59-254">Cserélje le `<storageaccountname>` hello a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="4fa59-254">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService1",
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
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="4fa59-255">Cserélje le `<storageaccountname>` hello a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="4fa59-255">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

    <span data-ttu-id="4fa59-256">hello JSON részlet definiál egy folyamatot, amely egy adott tevékenység (Hive tevékenység) áll.</span><span class="sxs-lookup"><span data-stu-id="4fa59-256">hello JSON snippet defines a pipeline that consists of a single activity (Hive Activity).</span></span> <span data-ttu-id="4fa59-257">Ez a tevékenység a Hive parancsfájl tooprocess bemeneti adatok az igény szerinti HDInsight fürt tooproduce kimeneti adatok futtatja.</span><span class="sxs-lookup"><span data-stu-id="4fa59-257">This activity runs a Hive script tooprocess input data on an on-demand HDInsight cluster tooproduce output data.</span></span> <span data-ttu-id="4fa59-258">Hello adatcsatorna JSON hello tevékenységek területen látható hello tömb csak egy tevékenység típusa túl a**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-258">In hello activities section of hello pipeline JSON, you see only one activity in hello array with type set too**HDInsightHive**.</span></span> 

    <span data-ttu-id="4fa59-259">A hello típusú tulajdonságokhoz adott tooHDInsight Hive tevékenység megadhatja, milyen Azure tárolás társított szolgáltatásának hello hive parancsfájl, a hello elérési toohello parancsfájlt és a paraméterek toohello parancsfájl rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4fa59-259">In hello type properties that are specific tooHDInsight Hive activity, you specify what Azure Storage linked service has hello hive script file, hello path toohello script file, and parameters toohello script file.</span></span> 

    <span data-ttu-id="4fa59-260">hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók (hello scriptLinkedService által megadott), és a hello tárolt `script` hello tároló mappa `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-260">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService), and in hello `script` folder in hello container `adfgetstarted`.</span></span>

    <span data-ttu-id="4fa59-261">Hello `defines` szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-261">hello `defines` section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span></span>

    <span data-ttu-id="4fa59-262">Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4fa59-262">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span> <span data-ttu-id="4fa59-263">Konfigurált hello dataset toobe előállított havonta, ezért csak akkor, ha a szelet hozzák hello csővezeték (mivel ugyanazt a kezdő és befejező dátumok hello hónap).</span><span class="sxs-lookup"><span data-stu-id="4fa59-263">You configured hello dataset toobe produced monthly, therefore, only once slice is produced by hello pipeline (because hello month is same in start and end dates).</span></span>

    <span data-ttu-id="4fa59-264">Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-264">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>
4. <span data-ttu-id="4fa59-265">Mentse a hello **HiveActivity1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-265">Save hello **HiveActivity1.json** file.</span></span>

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a><span data-ttu-id="4fa59-266">A partitionweblogs.hql és az input.log fájl hozzáadása függőségként</span><span class="sxs-lookup"><span data-stu-id="4fa59-266">Add partitionweblogs.hql and input.log as a dependency</span></span>
1. <span data-ttu-id="4fa59-267">Kattintson a jobb gombbal **függőségek** hello a **Solution Explorer** ablakban pont túl**Hozzáadás**, és kattintson a **a létező elemet**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-267">Right-click **Dependencies** in hello **Solution Explorer** window, point too**Add**, and click **Existing Item**.</span></span>  
2. <span data-ttu-id="4fa59-268">Keresse meg a toohello **C:\ADFGettingStarted** válassza **partitionweblogs.hql**, **input.log** fájlokat, majd kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-268">Navigate toohello **C:\ADFGettingStarted** and select **partitionweblogs.hql**, **input.log** files, and click **Add**.</span></span> <span data-ttu-id="4fa59-269">Ezeket a fájlokat két hello Előfeltételek részeként létrehozott [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="4fa59-269">You created these two files as part of prerequisites from hello [Tutorial Overview](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="4fa59-270">Hello megoldás hello következő lépésben közzétételekor hello **partitionweblogs.hql** fájl feltöltött toohello **parancsfájl** hello mappájában `adfgetstarted` blob tároló.</span><span class="sxs-lookup"><span data-stu-id="4fa59-270">When you publish hello solution in hello next step, hello **partitionweblogs.hql** file is uploaded toohello **script** folder in hello `adfgetstarted` blob container.</span></span>   

### <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="4fa59-271">Data Factory-entitások közzététele/üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="4fa59-271">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="4fa59-272">Ebben a lépésben közzététele hello adat-előállító entitások (összekapcsolt szolgáltatások adatkészletek és pipeline) a projekt toohello Azure Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-272">In this step, you publish hello Data Factory entities (linked services, datasets, and pipeline) in your project toohello Azure Data Factory service.</span></span> <span data-ttu-id="4fa59-273">A közzététel folyamatban hello adja meg a data factory hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4fa59-273">In hello process of publishing, you specify hello name for your data factory.</span></span> 

1. <span data-ttu-id="4fa59-274">Kattintson a jobb gombbal a projektre a Solution Explorer hello, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-274">Right-click project in hello Solution Explorer, and click **Publish**.</span></span>
2. <span data-ttu-id="4fa59-275">Ha látja **jelentkezzen be Microsoft-fiók tooyour** párbeszédpanelen adja meg Azure-előfizetéssel rendelkező hello fiók hitelesítő adatait, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-275">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="4fa59-276">A következő párbeszédpanel hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4fa59-276">You should see hello following dialog box:</span></span>

   ![Publish (Közzététel) párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. <span data-ttu-id="4fa59-278">A hello **konfigurálása adat-előállító** lapján hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4fa59-278">In hello **Configure data factory** page, do hello following steps:</span></span>

    ![Közzététel – Új data factory beállításai](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. <span data-ttu-id="4fa59-280">Válassza a **Create New Data Factory** (Új data factory létrehozása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4fa59-280">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="4fa59-281">Adjon meg egy egyedi **neve** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="4fa59-281">Enter a unique **name** for hello data factory.</span></span> <span data-ttu-id="4fa59-282">Például: **DataFactoryUsingVS09152016**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-282">For example: **DataFactoryUsingVS09152016**.</span></span> <span data-ttu-id="4fa59-283">hello nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4fa59-283">hello name must be globally unique.</span></span>
   3. <span data-ttu-id="4fa59-284">Válassza ki a megfelelő előfizetés hello hello **előfizetés** mező.</span><span class="sxs-lookup"><span data-stu-id="4fa59-284">Select hello right subscription for hello **Subscription** field.</span></span> 
        > [!IMPORTANT]
        > <span data-ttu-id="4fa59-285">Ha nem látja bármely előfizetés, győződjön meg arról, hogy olyan fiókkal, amely egy rendszergazdai vagy társadminisztrátori hello előfizetés naplóba.</span><span class="sxs-lookup"><span data-stu-id="4fa59-285">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>
   4. <span data-ttu-id="4fa59-286">Jelölje be hello **erőforráscsoport** a hello data factory toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4fa59-286">Select hello **resource group** for hello data factory toobe created.</span></span>
   5. <span data-ttu-id="4fa59-287">Jelölje be hello **régió** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="4fa59-287">Select hello **region** for hello data factory.</span></span>
   6. <span data-ttu-id="4fa59-288">Kattintson a **következő** tooswitch toohello **elemek közzététele** lap.</span><span class="sxs-lookup"><span data-stu-id="4fa59-288">Click **Next** tooswitch toohello **Publish Items** page.</span></span> <span data-ttu-id="4fa59-289">(Nyomja meg az **lapon** kívül hello neve mező tooif hello toomove **következő** gomb le van tiltva.)</span><span class="sxs-lookup"><span data-stu-id="4fa59-289">(Press **TAB** toomove out of hello Name field tooif hello **Next** button is disabled.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4fa59-290">Ha hello hibaüzenet **nem érhető el adat-előállító "DataFactoryUsingVS"** közzétételekor, módosítsa a hello nevét (például yournameDataFactoryUsingVS).</span><span class="sxs-lookup"><span data-stu-id="4fa59-290">If you receive hello error **Data factory name “DataFactoryUsingVS” is not available** when publishing, change hello name (for example, yournameDataFactoryUsingVS).</span></span> <span data-ttu-id="4fa59-291">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="4fa59-291">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
1. <span data-ttu-id="4fa59-292">A hello **elemek közzététele** lapon, győződjön meg arról, hogy az összes hello adat-előállítók entitások van kiválasztva, és kattintson a **következő** tooswitch toohello **összegzés** lap.</span><span class="sxs-lookup"><span data-stu-id="4fa59-292">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>

    ![Publish items (Elemek közzététele) oldal](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. <span data-ttu-id="4fa59-294">Olvassa el az összesítő hello és a **következő** toostart hello központi telepítési folyamat és a nézet hello **központi telepítési állapot**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-294">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>

    ![Összefoglaló lap](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. <span data-ttu-id="4fa59-296">A hello **központi telepítési állapot** lapon hello telepítési folyamatának állapotát hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="4fa59-296">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="4fa59-297">Hello a telepítés befejezése után kattintson a Befejezés gombra.</span><span class="sxs-lookup"><span data-stu-id="4fa59-297">Click Finish after hello deployment is done.</span></span>

<span data-ttu-id="4fa59-298">Fontos pontok toonote:</span><span class="sxs-lookup"><span data-stu-id="4fa59-298">Important points toonote:</span></span>

- <span data-ttu-id="4fa59-299">Ha hello hibaüzenetet kapja: **ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**, hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="4fa59-299">If you receive hello error: **This subscription is not registered toouse namespace Microsoft.DataFactory**, do one of hello following and try publishing again:</span></span>
    - <span data-ttu-id="4fa59-300">Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="4fa59-300">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span>
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        <span data-ttu-id="4fa59-301">A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="4fa59-301">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span>

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - <span data-ttu-id="4fa59-302">Bejelentkezés használatával hello Azure-előfizetés toohello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4fa59-302">Login using hello Azure subscription in toohello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="4fa59-303">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="4fa59-303">This action automatically registers hello provider for you.</span></span>
- <span data-ttu-id="4fa59-304">hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-304">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
- <span data-ttu-id="4fa59-305">toocreate adat-előállító példányok, toobe rendszergazdája vagy szükséges hello Azure-előfizetés társadminisztrátornak</span><span class="sxs-lookup"><span data-stu-id="4fa59-305">toocreate Data Factory instances, you need toobe an admin or co-admin of hello Azure subscription</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="4fa59-306">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="4fa59-306">Monitor pipeline</span></span>
<span data-ttu-id="4fa59-307">Ebben a lépésben hello csővezeték hello adat-előállítót a Diagram nézet használata a figyelheti meg.</span><span class="sxs-lookup"><span data-stu-id="4fa59-307">In this step, you monitor hello pipeline using Diagram View of hello data factory.</span></span> 

#### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="4fa59-308">Folyamat figyelése diagramnézetben</span><span class="sxs-lookup"><span data-stu-id="4fa59-308">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="4fa59-309">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/), hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4fa59-309">Log in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>
   1. <span data-ttu-id="4fa59-310">Kattintson a **További szolgáltatások**, majd az **Adat-előállítók** elemre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-310">Click **More services** and click **Data factories**.</span></span>
       
        ![Data factoryk tallózása](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. <span data-ttu-id="4fa59-312">A data factory válassza hello nevét (például: **DataFactoryUsingVS09152016**) adat-előállítók hello listája.</span><span class="sxs-lookup"><span data-stu-id="4fa59-312">Select hello name of your data factory (for example: **DataFactoryUsingVS09152016**) from hello list of data factories.</span></span>
   
       ![A data factory kiválasztása](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. <span data-ttu-id="4fa59-314">A data factory hello kezdőlapján kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-314">In hello home page for your data factory, click **Diagram**.</span></span>

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. <span data-ttu-id="4fa59-316">Hello Diagram nézet hello folyamatok, és ebben az oktatóanyagban használt adatkészletek áttekintése látható.</span><span class="sxs-lookup"><span data-stu-id="4fa59-316">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. <span data-ttu-id="4fa59-318">az összes tevékenység hello sorban, kattintson a jobb gombbal kimenetátirányítási mechanizmusával hello ábra, majd kattintson a nyitott folyamat tooview.</span><span class="sxs-lookup"><span data-stu-id="4fa59-318">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. <span data-ttu-id="4fa59-320">Győződjön meg arról, hogy megjelenik-e hello adatcsatorna hello HDInsightHive tevékenysége.</span><span class="sxs-lookup"><span data-stu-id="4fa59-320">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    <span data-ttu-id="4fa59-322">toonavigate biztonsági toohello előző nézetével, kattintson a **adat-előállító** hello navigációs menüjében hello tetején.</span><span class="sxs-lookup"><span data-stu-id="4fa59-322">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
6. <span data-ttu-id="4fa59-323">A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-323">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="4fa59-324">Győződjön meg arról, hogy hello szelet a **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="4fa59-324">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="4fa59-325">Azt is tarthat néhány percet a hello szelet tooshow üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-325">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="4fa59-326">Ha nem így lenne után egy kis ideig, lásd: Ha hello bemeneti fájl (input.log) hello megfelelő tárolóban van (`adfgetstarted`) és a mappa (`inputdata`).</span><span class="sxs-lookup"><span data-stu-id="4fa59-326">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (`adfgetstarted`) and folder (`inputdata`).</span></span> <span data-ttu-id="4fa59-327">És győződjön meg arról, hogy hello **külső** hello bemeneti adatkészlethez tulajdonsága túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-327">And, make sure that hello **external** property on hello input dataset is set too**true**.</span></span> 

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. <span data-ttu-id="4fa59-329">Kattintson a **X** tooclose **AzureBlobInput** panelen.</span><span class="sxs-lookup"><span data-stu-id="4fa59-329">Click **X** tooclose **AzureBlobInput** blade.</span></span>
8. <span data-ttu-id="4fa59-330">A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-330">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="4fa59-331">Láthatja, hogy hello szelet, amely folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="4fa59-331">You see that hello slice that is currently being processed.</span></span>

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. <span data-ttu-id="4fa59-333">Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="4fa59-333">When processing is done, you see hello slice in **Ready** state.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4fa59-334">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="4fa59-334">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="4fa59-335">Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="4fa59-335">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>  
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. <span data-ttu-id="4fa59-337">Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello `partitioneddata` hello mappájában `adfgetstarted` a blob-tároló hello tárolóhoz kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-337">When hello slice is in **Ready** state, check hello `partitioneddata` folder in hello `adfgetstarted` container in your blob storage for hello output data.</span></span>  

    ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. <span data-ttu-id="4fa59-339">Hello szelet toosee részleteit a kattintson egy **adatszelet** panelen.</span><span class="sxs-lookup"><span data-stu-id="4fa59-339">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

    ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. <span data-ttu-id="4fa59-341">Hello futtatása tevékenység kattintson **tevékenység fut lista** toosee adatait egy tevékenység futtatását (esetünkben Hive tevékenység) egy **tevékenység fut részletek** ablak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-341">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span> 
  
    ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    <span data-ttu-id="4fa59-343">Hello naplófájlokból láthatja, hogy végre lett hajtva hello Hive-lekérdezések és állapotára vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-343">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="4fa59-344">A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-344">These logs are useful for troubleshooting any issues.</span></span>  

<span data-ttu-id="4fa59-345">Lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) hogyan toouse hello Azure portál toomonitor hello feldolgozási sorban lévő és adatkészletek létrehozott ebben az oktatóanyagban kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-345">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

#### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="4fa59-346">Folyamat figyelése a Monitor & Manage alkalmazással</span><span class="sxs-lookup"><span data-stu-id="4fa59-346">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="4fa59-347">Is figyelheti, & alkalmazás toomonitor kezelése a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="4fa59-347">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="4fa59-348">Az alkalmazás használatával kapcsolatos részletes információkért olvassa el a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-348">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="4fa59-349">Kattintson a Monitor & Manage csempére.</span><span class="sxs-lookup"><span data-stu-id="4fa59-349">Click Monitor & Manage tile.</span></span>

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. <span data-ttu-id="4fa59-351">Meg kell jelennie a Monitor & Manage alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-351">You should see Monitor & Manage application.</span></span> <span data-ttu-id="4fa59-352">Változás hello **kezdési időpont** és **befejező időpontja** toomatch start (04-01-2016 12:00-kor) és befejezési időpontja (04-02-2016 12:00-kor) a feldolgozási sor, majd kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-352">Change hello **Start time** and **End time** toomatch start (04-01-2016 12:00 AM) and end times (04-02-2016 12:00 AM) of your pipeline, and click **Apply**.</span></span>

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. <span data-ttu-id="4fa59-354">toosee adatait egy tevékenység ablakban jelölje ki a hello **tevékenység Windows lista** toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="4fa59-354">toosee details about an activity window, select it in hello **Activity Windows list** toosee details about it.</span></span>
    <span data-ttu-id="4fa59-355">![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="4fa59-355">![Activity window details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4fa59-356">hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="4fa59-356">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="4fa59-357">Ezért, ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltése hello bemeneti fájl (input.log) toohello `inputdata` mappában található hello `adfgetstarted` tároló.</span><span class="sxs-lookup"><span data-stu-id="4fa59-357">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello `inputdata` folder of hello `adfgetstarted` container.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="4fa59-358">További megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4fa59-358">Additional notes</span></span>
- <span data-ttu-id="4fa59-359">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4fa59-359">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="4fa59-360">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="4fa59-360">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="4fa59-361">Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-361">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="4fa59-362">Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fa59-362">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="4fa59-363">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája.</span><span class="sxs-lookup"><span data-stu-id="4fa59-363">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span>
- <span data-ttu-id="4fa59-364">Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási.</span><span class="sxs-lookup"><span data-stu-id="4fa59-364">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="4fa59-365">Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fa59-365">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="4fa59-366">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája és [átalakítása tevékenységek](data-factory-data-transformation-activities.md) , futtathatja őket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-366">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory and [transformation activities](data-factory-data-transformation-activities.md) that can run on them.</span></span>
- <span data-ttu-id="4fa59-367">Lásd: [helyezi át az adatokat a Blob-tooAzure /](data-factory-azure-blob-connector.md#azure-storage-linked-service) hello használt JSON-tulajdonságok vonatkozó további információért Azure Storage társított szolgáltatás definíciójának.</span><span class="sxs-lookup"><span data-stu-id="4fa59-367">See [Move data from/tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used in hello Azure Storage linked service definition.</span></span>
- <span data-ttu-id="4fa59-368">Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-368">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4fa59-369">További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="4fa59-369">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>
-  <span data-ttu-id="4fa59-370">hello adat-előállító létrehoz egy **Linux-alapú** rendelkező JSON megelőző hello meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="4fa59-370">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello preceding JSON.</span></span> <span data-ttu-id="4fa59-371">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="4fa59-371">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
- <span data-ttu-id="4fa59-372">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** hello hello JSON (linkedServiceName) a megadott blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-372">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="4fa59-373">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="4fa59-373">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="4fa59-374">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="4fa59-374">This behavior is by design.</span></span> <span data-ttu-id="4fa59-375">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="4fa59-375">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="4fa59-376">hello feldolgozása hello fürt automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="4fa59-376">hello cluster is automatically deleted when hello processing is done.</span></span>
    
    <span data-ttu-id="4fa59-377">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-377">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="4fa59-378">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-378">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="4fa59-379">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="4fa59-379">hello names of these containers follow a pattern: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span></span> <span data-ttu-id="4fa59-380">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="4fa59-380">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>
- <span data-ttu-id="4fa59-381">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="4fa59-381">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="4fa59-382">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="4fa59-382">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 
- <span data-ttu-id="4fa59-383">Ez az oktatóanyag nem tartalmazza az adatok Azure Data Factory használatával történő másolásának leírását.</span><span class="sxs-lookup"><span data-stu-id="4fa59-383">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="4fa59-384">Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4fa59-384">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>


## <a name="use-server-explorer-tooview-data-factories"></a><span data-ttu-id="4fa59-385">Használja a Server Explorer tooview adat-előállítók</span><span class="sxs-lookup"><span data-stu-id="4fa59-385">Use Server Explorer tooview data factories</span></span>
1. <span data-ttu-id="4fa59-386">A **Visual Studio**, kattintson a **nézet** hello menü, és kattintson a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-386">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="4fa59-387">Hello Server Explorer-ablakban bontsa ki a **Azure** csomópontot **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-387">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="4fa59-388">Ha látja **tooVisual Studio bejelentkezés**, adja meg a hello **fiók** társított a Azure-előfizetéssel, és kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-388">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="4fa59-389">Adja meg a **jelszót**, és kattintson a **Sign in** (Bejelentkezés) elemre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-389">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="4fa59-390">A Visual Studio megpróbál az előfizetés az összes Azure adat-előállítók tooget információt.</span><span class="sxs-lookup"><span data-stu-id="4fa59-390">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="4fa59-391">Ez a művelet hello hello állapotát látja **Data Factory feladatlista** ablak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-391">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Server Explorer (Kiszolgálókezelő)](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. <span data-ttu-id="4fa59-393">Kattintson a jobb gombbal egy adat-előállítót, és válassza ki **exportálása adat-előállító tooNew projekt** toocreate Visual Studio-projekt valamelyik adat-előállítót alapján.</span><span class="sxs-lookup"><span data-stu-id="4fa59-393">You can right-click a data factory, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Export data factory (Data factory exportálása) menüelem](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="4fa59-395">Visual Studióhoz készült Data Factory-eszközök frissítése</span><span class="sxs-lookup"><span data-stu-id="4fa59-395">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="4fa59-396">tooupdate Azure Data Factory tools for Visual Studio hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4fa59-396">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="4fa59-397">Kattintson a **eszközök** hello menüre, majd válassza a **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-397">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="4fa59-398">Válassza ki **frissítések** a hello bal oldali ablaktáblán, és válassza ki **Visual Studio galériában**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-398">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="4fa59-399">Válassza az **Azure Data Factory tools for Visual Studio** lehetőséget, és kattintson az **Update** (Frissítés) elemre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-399">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="4fa59-400">Ez a bejegyzés nem látható, ha már rendelkezik hello hello eszközök legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="4fa59-400">If you do not see this entry, you already have hello latest version of hello tools.</span></span>

## <a name="use-configuration-files"></a><span data-ttu-id="4fa59-401">Konfigurációs fájlok használata</span><span class="sxs-lookup"><span data-stu-id="4fa59-401">Use configuration files</span></span>
<span data-ttu-id="4fa59-402">Minden környezet eltérően a szolgáltatások/táblák/folyamatok csatolt konfigurációs fájlokat a Visual Studio tooconfigure tulajdonságai használható.</span><span class="sxs-lookup"><span data-stu-id="4fa59-402">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="4fa59-403">Vegye figyelembe a következő az Azure tárolás társított szolgáltatásának JSON-definícióból hello.</span><span class="sxs-lookup"><span data-stu-id="4fa59-403">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="4fa59-404">toospecify **connectionString** accountname és az accountkey elemeket hello (fejlesztési/tesztelési/éles) környezetben toowhich alapján különböző értékekkel a Data Factory entitások telepít.</span><span class="sxs-lookup"><span data-stu-id="4fa59-404">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="4fa59-405">Az ilyen működést úgy érheti el, hogy minden környezethez külön konfigurációs fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="4fa59-405">You can achieve this behavior by using separate configuration file for each environment.</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a><span data-ttu-id="4fa59-406">Konfigurációs fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4fa59-406">Add a configuration file</span></span>
<span data-ttu-id="4fa59-407">Minden környezet konfigurációs fájl felvétele hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="4fa59-407">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="4fa59-408">Hello adat-előállító projektre a Visual Studio megoldásnak a jobb gombbal túl**Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-408">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="4fa59-409">Válassza ki **Config** hello bal oldali telepített sablonok hello listában jelölje ki **konfigurációs fájl**, adjon meg egy **neve** hello konfiguráció fájlt, és kattintson a **Hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-409">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Konfigurációs fájl hozzáadása](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="4fa59-411">Adja hozzá a következő formátumban hello konfigurációs paramétereit és azok értékét:</span><span class="sxs-lookup"><span data-stu-id="4fa59-411">Add configuration parameters and their values in hello following format:</span></span>

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    <span data-ttu-id="4fa59-412">Ez a példa konfigurálja egy Azure Storage társított szolgáltatás és egy Azure SQL társított szolgáltatás connectionString tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="4fa59-412">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="4fa59-413">Figyelje meg, hogy hello nevét megadó szintaxisa a következő [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="4fa59-413">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="4fa59-414">Ha JSON, amely rendelkezik egy tömböt, ahogy az a következő kód hello tulajdonsággal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4fa59-414">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    <span data-ttu-id="4fa59-415">Ahogy az következő konfigurációs fájl (használata nulla alapú indexelést) hello tulajdonságainak konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="4fa59-415">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a><span data-ttu-id="4fa59-416">Tulajdonságnevek szóközökkel</span><span class="sxs-lookup"><span data-stu-id="4fa59-416">Property names with spaces</span></span>
<span data-ttu-id="4fa59-417">Ha egy tulajdonság neve szóközöket tartalmaz, használja a kapcsos zárójeleket látható módon a következő példa (adatbázis-kiszolgáló neve) hello:</span><span class="sxs-lookup"><span data-stu-id="4fa59-417">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="4fa59-418">Megoldás üzembe helyezése konfiguráció használatával</span><span class="sxs-lookup"><span data-stu-id="4fa59-418">Deploy solution using a configuration</span></span>
<span data-ttu-id="4fa59-419">Ha az Azure Data Factory entitások tesznek közzé a Visual STUDIO, hello konfigurációs toouse használandó közzétételi művelet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-419">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="4fa59-420">a konfigurációs fájl használata az Azure Data Factory-projektek entitások toopublish:</span><span class="sxs-lookup"><span data-stu-id="4fa59-420">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="4fa59-421">Kattintson a jobb gombbal a Data Factory-projektet, és kattintson a **közzététel** toosee hello **elemek közzététele** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="4fa59-421">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="4fa59-422">Válassza ki valamelyik adat-előállítót, vagy adjon meg egy adat-előállító létrehozása a hello értékeinek **konfigurálása adat-előállító** lapon, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-422">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="4fa59-423">A hello **elemek közzététele** lap: megjelenik egy legördülő listából válassza ki az elérhető konfigurációk hello **a telepítési konfiguráció kiválasztása** mező.</span><span class="sxs-lookup"><span data-stu-id="4fa59-423">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Konfigurációs fájl kiválasztása](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="4fa59-425">Jelölje be hello **konfigurációs fájl** , hogy kívánja toouse, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-425">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="4fa59-426">Ellenőrizze, hogy látható-e a hello JSON-fájl neve hello **összegzés** lapot, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-426">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="4fa59-427">Kattintson a **Befejezés** hello központi telepítési művelet befejezése után.</span><span class="sxs-lookup"><span data-stu-id="4fa59-427">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="4fa59-428">Üzembe helyezésekor hello hello konfigurációs fájlból származó értékek használt tooset értékek hello JSON-fájlok tulajdonságok előtt hello entitások telepített tooAzure Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="4fa59-428">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="4fa59-429">Az Azure Key Vault használata</span><span class="sxs-lookup"><span data-stu-id="4fa59-429">Use Azure Key Vault</span></span>
<span data-ttu-id="4fa59-430">Már nem ajánlott, és gyakran biztonsági házirend toocommit érzékeny adatok, például kapcsolati karakterláncok toohello kód tárház ellen.</span><span class="sxs-lookup"><span data-stu-id="4fa59-430">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="4fa59-431">Lásd: [ADF biztonságos közzétételéhez](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) mintát a Githubon toolearn bizalmas adatok tárolása az Azure Key Vault és használhassa a Data Factory entitások közzététele közben.</span><span class="sxs-lookup"><span data-stu-id="4fa59-431">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="4fa59-432">hello bővítmény biztonságos közzététele a Visual Studio lehetővé teszi, hogy a hello titkok toobe Key Vault tárolja, és csak a hivatkozások toothem meg van határozva a társított szolgáltatások / szolgáltatástelepítési konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="4fa59-432">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="4fa59-433">Ha közzéteszi a Data Factory entitások tooAzure feloldása az ezeket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-433">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="4fa59-434">Ezek a fájlok majd lehet véglegesíteni toosource tárház anélkül, hogy a titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="4fa59-434">These files can then be committed toosource repository without exposing any secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="4fa59-435">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4fa59-435">Summary</span></span>
<span data-ttu-id="4fa59-436">Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4fa59-436">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="4fa59-437">A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:</span><span class="sxs-lookup"><span data-stu-id="4fa59-437">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="4fa59-438">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="4fa59-438">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="4fa59-439">Létrehozott két **társított szolgáltatást**:</span><span class="sxs-lookup"><span data-stu-id="4fa59-439">Created two **linked services**:</span></span>
   1. <span data-ttu-id="4fa59-440">**Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.</span><span class="sxs-lookup"><span data-stu-id="4fa59-440">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="4fa59-441">**Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-441">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="4fa59-442">Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4fa59-442">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="4fa59-443">Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fa59-443">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="4fa59-444">Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="4fa59-444">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="4fa59-445">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fa59-445">Next Steps</span></span>
<span data-ttu-id="4fa59-446">Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="4fa59-446">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4fa59-447">Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4fa59-447">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="4fa59-448">Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után).</span><span class="sxs-lookup"><span data-stu-id="4fa59-448">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="4fa59-449">Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-449">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 


## <a name="see-also"></a><span data-ttu-id="4fa59-450">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4fa59-450">See Also</span></span>
| <span data-ttu-id="4fa59-451">Témakör</span><span class="sxs-lookup"><span data-stu-id="4fa59-451">Topic</span></span> | <span data-ttu-id="4fa59-452">Leírás</span><span class="sxs-lookup"><span data-stu-id="4fa59-452">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="4fa59-453">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="4fa59-453">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="4fa59-454">Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket.</span><span class="sxs-lookup"><span data-stu-id="4fa59-454">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="4fa59-455">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="4fa59-455">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="4fa59-456">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="4fa59-456">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="4fa59-457">Adatátalakítási tevékenységek</span><span class="sxs-lookup"><span data-stu-id="4fa59-457">Data Transformation Activities</span></span>](data-factory-data-transformation-activities.md) |<span data-ttu-id="4fa59-458">Ez a cikk felsorolja az Azure Data Factory által támogatott adatátalakítási tevékenységeket (mint például a jelen oktatóanyagban használt HDInsight Hive-átalakítás).</span><span class="sxs-lookup"><span data-stu-id="4fa59-458">This article provides a list of data transformation activities (such as HDInsight Hive transformation you used in this tutorial) supported by Azure Data Factory.</span></span> |
| [<span data-ttu-id="4fa59-459">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="4fa59-459">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="4fa59-460">Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait.</span><span class="sxs-lookup"><span data-stu-id="4fa59-460">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="4fa59-461">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="4fa59-461">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="4fa59-462">Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4fa59-462">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
