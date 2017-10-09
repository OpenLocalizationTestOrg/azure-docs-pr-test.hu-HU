---
title: "aaaBuild az első adat-előállítóban (Azure-portál) |} Microsoft Docs"
description: "Ebben az oktatóanyagban létrehoz egy minta Azure Data Factory-folyamathoz Data Factory Editor hello Azure-portál használatával."
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
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="1cc73-103">Oktatóanyag: az első Azure data factory létrehozása az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="1cc73-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1cc73-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cc73-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="1cc73-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1cc73-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="1cc73-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cc73-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="1cc73-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1cc73-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="1cc73-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="1cc73-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="1cc73-109">REST API</span><span class="sxs-lookup"><span data-stu-id="1cc73-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="1cc73-110">Ebből a cikkből megismerheti, hogyan toouse [Azure-portálon](https://portal.azure.com/) toocreate az első az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="1cc73-110">In this article, you learn how toouse [Azure portal](https://portal.azure.com/) toocreate your first Azure data factory.</span></span> <span data-ttu-id="1cc73-111">toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="1cc73-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span> 

<span data-ttu-id="1cc73-112">Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="1cc73-113">Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="1cc73-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="1cc73-114">hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.</span><span class="sxs-lookup"><span data-stu-id="1cc73-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="1cc73-115">hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="1cc73-116">Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="1cc73-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="1cc73-117">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="1cc73-118">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="1cc73-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="1cc73-119">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="1cc73-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cc73-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1cc73-120">Prerequisites</span></span>
1. <span data-ttu-id="1cc73-121">Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="1cc73-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
2. <span data-ttu-id="1cc73-122">Ez a cikk nem nyújt áttekintést hello Azure Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="1cc73-122">This article does not provide a conceptual overview of hello Azure Data Factory service.</span></span> <span data-ttu-id="1cc73-123">Javasoljuk, hogy olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk részletes áttekintés hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1cc73-123">We recommend that you go through [Introduction tooAzure Data Factory](data-factory-introduction.md) article for a detailed overview of hello service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="1cc73-124">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-124">Create data factory</span></span>
<span data-ttu-id="1cc73-125">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="1cc73-126">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="1cc73-127">Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-127">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="1cc73-128">Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1cc73-128">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="1cc73-129">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1cc73-129">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1cc73-130">Kattintson a **új** hello bal oldali menüben kattintson **adatok + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-130">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![A Create (Létrehozás) panel](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="1cc73-132">A hello **új adat-előállító** panelen adja meg **GetStartedDF** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1cc73-132">In hello **New data factory** blade, enter **GetStartedDF** for hello Name.</span></span>

   ![A New data factory (Új data factory) panel](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="1cc73-134">az Azure data factory hello hello nevének kell lennie **globálisan egyedi**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-134">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="1cc73-135">Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "GetStartedDF"**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-135">If you receive hello error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="1cc73-136">Módosítsa a hello adat-előállítóban (például yournameGetStartedDF) hello nevét, majd próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1cc73-136">Change hello name of hello data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="1cc73-137">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="1cc73-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="1cc73-138">hello adat-előállító nevét hello regisztrálva előfordulhat, hogy legyen egy **DNS** hello jövőben nevet, és ezért a nyilvánosan láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="1cc73-138">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="1cc73-139">Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.</span><span class="sxs-lookup"><span data-stu-id="1cc73-139">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="1cc73-140">Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="1cc73-141">Az oktatóanyagban hello nevű erőforráscsoport létrehozása: **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-141">For hello tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="1cc73-142">Jelölje be hello **hely** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="1cc73-142">Select hello **location** for hello data factory.</span></span> <span data-ttu-id="1cc73-143">Csak a Data Factory szolgáltatásnak hello által támogatott régiók hello legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1cc73-143">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
7. <span data-ttu-id="1cc73-144">Válassza ki **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-144">Select **Pin toodashboard**.</span></span> 
8. <span data-ttu-id="1cc73-145">Kattintson a **létrehozása** a hello **új adat-előállító** panelen.</span><span class="sxs-lookup"><span data-stu-id="1cc73-145">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1cc73-146">toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="1cc73-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="1cc73-147">Hello irányítópult állapotú csempe hello következő látja: telepítését adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-147">On hello dashboard, you see hello following tile with status: Deploying data factory.</span></span>    

   ![A data factory létrehozásának állapota](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="1cc73-149">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1cc73-149">Congratulations!</span></span> <span data-ttu-id="1cc73-150">Sikeresen létrehozta első data factoryjét.</span><span class="sxs-lookup"><span data-stu-id="1cc73-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="1cc73-151">Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.</span><span class="sxs-lookup"><span data-stu-id="1cc73-151">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>     

    ![A Data Factory panel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="1cc73-153">Mielőtt létrehozna egy folyamat hello adat-előállítóban, kell toocreate néhány adat-előállító entitások először.</span><span class="sxs-lookup"><span data-stu-id="1cc73-153">Before creating a pipeline in hello data factory, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="1cc73-154">Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolására, adja meg a bemeneti és kimeneti adatkészletek toorepresent bemeneti/kimeneti adatai csatolt adatok áruházakból és majd hozzon létre egy tevékenység által használt ezek az adatkészletek hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-154">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="1cc73-155">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-155">Create linked services</span></span>
<span data-ttu-id="1cc73-156">Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="1cc73-157">hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="1cc73-157">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="1cc73-158">HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1cc73-158">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="1cc73-159">Mi azonosítása [adattár](data-factory-data-movement-activities.md)/[szolgáltatások számítási](data-factory-compute-linked-services.md) a forgatókönyvben használt, és ezen szolgáltatások toohello adat-előállító hivatkozás összekapcsolt szolgáltatások létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="1cc73-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services toohello data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="1cc73-160">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="1cc73-161">Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="1cc73-161">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="1cc73-162">Ebben az oktatóanyagban hello használata azonos Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1cc73-162">In this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="1cc73-163">Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** paneljén **GetStartedDF**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-163">Click **Author and deploy** on hello **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="1cc73-164">Hello Data Factory Editor kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1cc73-164">You should see hello Data Factory Editor.</span></span>

   ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="1cc73-166">Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Új adattár – Azure Storage – menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="1cc73-168">Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="1cc73-168">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="1cc73-170">Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="1cc73-170">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="1cc73-171">toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="1cc73-171">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="1cc73-172">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1cc73-172">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![A Deploy (Üzembe helyezés) gomb](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="1cc73-174">Hello társított szolgáltatás telepítése után sikeresen hello **vázlat-1** ablak kell eltűnnek, és látni **AzureStorageLinkedService** a hello bal oldali fanézetben hello.</span><span class="sxs-lookup"><span data-stu-id="1cc73-174">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

    ![Storage társított szolgáltatás a menüben](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="1cc73-176">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="1cc73-177">Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="1cc73-177">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="1cc73-178">az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1cc73-178">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span>

1. <span data-ttu-id="1cc73-179">A hello **Data Factory Editor**, kattintson a **... More** (... További), kattintson a **New compute** (Új számítási példány) elemre, majd válassza az **On-demand HDInsight cluster** (Igény szerinti HDInsight-fürt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1cc73-179">In hello **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![A New compute (Új számítás) elem](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="1cc73-181">Másolja és illessze be a következő kódrészletet toohello hello **vázlat-1** ablak.</span><span class="sxs-lookup"><span data-stu-id="1cc73-181">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="1cc73-182">hello JSON részlet használt toocreate hello HDInsight fürt igény hello tulajdonságokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1cc73-182">hello JSON snippet describes hello properties that are used toocreate hello HDInsight cluster on-demand.</span></span>

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

    <span data-ttu-id="1cc73-183">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="1cc73-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="1cc73-184">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1cc73-184">Property</span></span> | <span data-ttu-id="1cc73-185">Leírás</span><span class="sxs-lookup"><span data-stu-id="1cc73-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1cc73-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="1cc73-186">ClusterSize</span></span> |<span data-ttu-id="1cc73-187">HDInsight-fürt hello hello méretét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1cc73-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="1cc73-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="1cc73-188">TimeToLive</span></span> | <span data-ttu-id="1cc73-189">Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="1cc73-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="1cc73-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="1cc73-190">linkedServiceName</span></span> | <span data-ttu-id="1cc73-191">Megadja a hello tárfiókja, amely a HDInsight által létrehozott használt toostore hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="1cc73-192">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="1cc73-192">Note hello following points:</span></span>

   * <span data-ttu-id="1cc73-193">hello adat-előállító létrehoz egy **Linux-alapú** a hello JSON meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1cc73-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="1cc73-194">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="1cc73-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="1cc73-195">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="1cc73-196">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="1cc73-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="1cc73-197">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="1cc73-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="1cc73-198">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="1cc73-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="1cc73-199">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="1cc73-199">This behavior is by design.</span></span> <span data-ttu-id="1cc73-200">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="1cc73-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="1cc73-201">hello feldolgozása hello fürt automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="1cc73-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="1cc73-202">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="1cc73-203">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="1cc73-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="1cc73-204">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp".</span><span class="sxs-lookup"><span data-stu-id="1cc73-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="1cc73-205">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="1cc73-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="1cc73-206">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="1cc73-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="1cc73-207">Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1cc73-207">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![Igény szerinti HDInsight társított szolgáltatás üzembe helyezése](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="1cc73-209">Ellenőrizze, hogy látható mindkét **AzureStorageLinkedService** és **HDInsightOnDemandLinkedService** a hello bal oldali fanézetben hello.</span><span class="sxs-lookup"><span data-stu-id="1cc73-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in hello tree view on hello left.</span></span>

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="1cc73-211">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-211">Create datasets</span></span>
<span data-ttu-id="1cc73-212">Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="1cc73-212">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="1cc73-213">Ezek az adatkészletek tekintse meg a toohello **AzureStorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1cc73-213">These datasets refer toohello **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="1cc73-214">hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-214">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="1cc73-215">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-215">Create input dataset</span></span>
1. <span data-ttu-id="1cc73-216">A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-216">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![New dataset (Új adatkészlet)](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="1cc73-218">Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello.</span><span class="sxs-lookup"><span data-stu-id="1cc73-218">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="1cc73-219">Hello JSON részlet, nevű adatkészlet létrehozásához **AzureBlobInput** hello feldolgozási soros tevékenység bemeneti adatokat képvisel, amelyek.</span><span class="sxs-lookup"><span data-stu-id="1cc73-219">In hello JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="1cc73-220">Emellett megadhatja, hogy hello bemeneti adatok nevű hello blob tárolóban található **adfgetstarted** és nevű hello mappát **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-220">In addition, you specify that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

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
    <span data-ttu-id="1cc73-221">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="1cc73-221">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="1cc73-222">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1cc73-222">Property</span></span> | <span data-ttu-id="1cc73-223">Leírás</span><span class="sxs-lookup"><span data-stu-id="1cc73-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="1cc73-224">type</span><span class="sxs-lookup"><span data-stu-id="1cc73-224">type</span></span> |<span data-ttu-id="1cc73-225">hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-225">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="1cc73-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="1cc73-226">linkedServiceName</span></span> |<span data-ttu-id="1cc73-227">Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1cc73-227">Refers toohello **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="1cc73-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="1cc73-228">folderPath</span></span> | <span data-ttu-id="1cc73-229">Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB.</span><span class="sxs-lookup"><span data-stu-id="1cc73-229">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="1cc73-230">fileName</span><span class="sxs-lookup"><span data-stu-id="1cc73-230">fileName</span></span> |<span data-ttu-id="1cc73-231">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="1cc73-231">This property is optional.</span></span> <span data-ttu-id="1cc73-232">Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz.</span><span class="sxs-lookup"><span data-stu-id="1cc73-232">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="1cc73-233">Ebben az oktatóanyagban csak hello **input.log** dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="1cc73-233">In this tutorial, only hello **input.log** is processed.</span></span> |
   | <span data-ttu-id="1cc73-234">type</span><span class="sxs-lookup"><span data-stu-id="1cc73-234">type</span></span> |<span data-ttu-id="1cc73-235">hello naplófájlok szöveges formátumú, így használjuk **szöveges**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-235">hello log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="1cc73-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="1cc73-236">columnDelimiter</span></span> |<span data-ttu-id="1cc73-237">oszlopok hello naplófájlokban határolja **vesszővel karakter (`,`)**</span><span class="sxs-lookup"><span data-stu-id="1cc73-237">columns in hello log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="1cc73-238">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="1cc73-238">frequency/interval</span></span> |<span data-ttu-id="1cc73-239">gyakoriságának beállítása túl**hónap** és időköz **1**, ami azt jelenti, hogy hello bemeneti szeletek havi érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1cc73-239">frequency set too**Month** and interval is **1**, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="1cc73-240">external</span><span class="sxs-lookup"><span data-stu-id="1cc73-240">external</span></span> | <span data-ttu-id="1cc73-241">Ez a tulajdonság értéke túl**igaz** hello bemeneti adatok nem jön létre, ez az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="1cc73-241">This property is set too**true** if hello input data is not generated by this pipeline.</span></span> <span data-ttu-id="1cc73-242">Ebben az oktatóanyagban hello input.log fájl nem jön létre ez az adatcsatorna, így hello tulajdonság tootrue hivatott.</span><span class="sxs-lookup"><span data-stu-id="1cc73-242">In this tutorial, hello input.log file is not generated by this pipeline, so we set hello property tootrue.</span></span> |

    <span data-ttu-id="1cc73-243">Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.</span><span class="sxs-lookup"><span data-stu-id="1cc73-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="1cc73-244">Kattintson a **telepítés** a toodeploy hello az újonnan létrehozott adatkészlet hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="1cc73-244">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span> <span data-ttu-id="1cc73-245">Meg kell jelennie a hello bal oldali fanézetben hello hello adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-245">You should see hello dataset in hello tree view on hello left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="1cc73-246">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-246">Create output dataset</span></span>
<span data-ttu-id="1cc73-247">Hello kimeneti adatkészlet toorepresent hello kimeneti tárolt adatok hello Azure Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-247">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="1cc73-248">A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-248">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="1cc73-249">Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello.</span><span class="sxs-lookup"><span data-stu-id="1cc73-249">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="1cc73-250">Hello JSON részlet, nevű adatkészlet létrehozásához **AzureBlobOutput**, és hello struktúra hello Hive parancsfájl által létrehozott hello adatok megadásával.</span><span class="sxs-lookup"><span data-stu-id="1cc73-250">In hello JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying hello structure of hello data that is produced by hello Hive script.</span></span> <span data-ttu-id="1cc73-251">Emellett megadhatja, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfgetstarted** és nevű hello mappát **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-251">In addition, you specify that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="1cc73-252">Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.</span><span class="sxs-lookup"><span data-stu-id="1cc73-252">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

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
    <span data-ttu-id="1cc73-253">Lásd: **létrehozni hello bemeneti adatkészletet** szakasz ezeket a tulajdonságokat leírását.</span><span class="sxs-lookup"><span data-stu-id="1cc73-253">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="1cc73-254">Be kell állítani nem hello külső tulajdonság egy kimeneti adatkészletet, hello dataset hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="1cc73-254">You do not set hello external property on an output dataset as hello dataset is produced by hello Data Factory service.</span></span>
3. <span data-ttu-id="1cc73-255">Kattintson a **telepítés** a toodeploy hello az újonnan létrehozott adatkészlet hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="1cc73-255">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span>
4. <span data-ttu-id="1cc73-256">Győződjön meg arról, hogy hello adatkészlet sikeresen létrejött-e.</span><span class="sxs-lookup"><span data-stu-id="1cc73-256">Verify that hello dataset is created successfully.</span></span>

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="1cc73-258">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cc73-258">Create pipeline</span></span>
<span data-ttu-id="1cc73-259">Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="1cc73-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="1cc73-260">Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly.</span><span class="sxs-lookup"><span data-stu-id="1cc73-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="1cc73-261">hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="1cc73-261">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="1cc73-262">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-262">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="1cc73-263">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-263">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="1cc73-264">a következő JSON hello használt hello tulajdonságok hello végén ebben a szakaszban lévő magyarázatát olvashatja.</span><span class="sxs-lookup"><span data-stu-id="1cc73-264">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="1cc73-265">A hello **Data Factory Editor**, kattintson a **három ponttal (…) További parancsok** majd **új adatcsatorna**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-265">In hello **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![A New pipeline (Új folyamat) gomb](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="1cc73-267">Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello.</span><span class="sxs-lookup"><span data-stu-id="1cc73-267">Copy and paste hello following snippet toohello Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1cc73-268">Cserélje le **storageaccountname** hello nevű hello JSON a storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="1cc73-268">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
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

    <span data-ttu-id="1cc73-269">Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatokat a HDInsight-fürtök létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1cc73-269">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="1cc73-270">hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **AzureStorageLinkedService**), majd a  **parancsfájl** hello tároló mappa **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-270">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="1cc73-271">Hello **meghatározása** szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).</span><span class="sxs-lookup"><span data-stu-id="1cc73-271">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="1cc73-272">Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1cc73-272">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="1cc73-273">Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-273">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1cc73-274">"Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) hello példában használt JSON-tulajdonságok vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="1cc73-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello example.</span></span>
   >
   >
3. <span data-ttu-id="1cc73-275">Erősítse meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1cc73-275">Confirm hello following:</span></span>

   1. <span data-ttu-id="1cc73-276">**input.log** fájl megtalálható-e hello **inputdata** mappában található hello **adfgetstarted** hello Azure blob storage tárolója</span><span class="sxs-lookup"><span data-stu-id="1cc73-276">**input.log** file exists in hello **inputdata** folder of hello **adfgetstarted** container in hello Azure blob storage</span></span>
   2. <span data-ttu-id="1cc73-277">**partitionweblogs.hql** fájl megtalálható-e hello **parancsfájl** mappában található hello **adfgetstarted** hello Azure blob storage tárolója.</span><span class="sxs-lookup"><span data-stu-id="1cc73-277">**partitionweblogs.hql** file exists in hello **script** folder of hello **adfgetstarted** container in hello Azure blob storage.</span></span> <span data-ttu-id="1cc73-278">Teljes hello előfeltételként szükséges lépések hello [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) Ha nem látja ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-278">Complete hello prerequisite steps in hello [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="1cc73-279">Győződjön meg arról, hogy lecseréli **storageaccountname** hello nevet, a tárfiók hello a csővezeték-JSON.</span><span class="sxs-lookup"><span data-stu-id="1cc73-279">Confirm that you replaced **storageaccountname** with hello name of your storage account in hello pipeline JSON.</span></span>
4. <span data-ttu-id="1cc73-280">Kattintson a **telepítés** a toodeploy hello csővezeték hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="1cc73-280">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span> <span data-ttu-id="1cc73-281">Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.</span><span class="sxs-lookup"><span data-stu-id="1cc73-281">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="1cc73-282">Ellenőrizze, hogy látható-e hello fanézetben hello csővezeték-e.</span><span class="sxs-lookup"><span data-stu-id="1cc73-282">Confirm that you see hello pipeline in hello tree view.</span></span>

    ![A fanézet a folyamattal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="1cc73-284">Gratulálunk, sikeresen létrehozta első folyamatát!</span><span class="sxs-lookup"><span data-stu-id="1cc73-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="1cc73-285">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="1cc73-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="1cc73-286">Folyamat figyelése diagramnézetben</span><span class="sxs-lookup"><span data-stu-id="1cc73-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="1cc73-287">Kattintson a **X** tooclose Data Factory Editor paneleken toonavigate biztonsági toohello adat-előállító panelt, és kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-287">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="1cc73-289">Hello Diagram nézet hello folyamatok, és ebben az oktatóanyagban használt adatkészletek áttekintése látható.</span><span class="sxs-lookup"><span data-stu-id="1cc73-289">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="1cc73-291">az összes tevékenység hello sorban, kattintson a jobb gombbal kimenetátirányítási mechanizmusával hello ábra, majd kattintson a nyitott folyamat tooview.</span><span class="sxs-lookup"><span data-stu-id="1cc73-291">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="1cc73-293">Győződjön meg arról, hogy megjelenik-e hello adatcsatorna hello HDInsightHive tevékenysége.</span><span class="sxs-lookup"><span data-stu-id="1cc73-293">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="1cc73-295">toonavigate biztonsági toohello előző nézetével, kattintson a **adat-előállító** hello navigációs menüjében hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1cc73-295">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
5. <span data-ttu-id="1cc73-296">A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-296">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="1cc73-297">Győződjön meg arról, hogy hello szelet a **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="1cc73-297">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="1cc73-298">Azt is tarthat néhány percet a hello szelet tooshow üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-298">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="1cc73-299">Várja meg a némi várakozás után történik, ha megjelenítéséhez hello bemeneti fájl (input.log) hello jobb tároló (adfgetstarted) és a mappa (inputdata).</span><span class="sxs-lookup"><span data-stu-id="1cc73-299">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="1cc73-301">Kattintson a **X** tooclose **AzureBlobInput** panelen.</span><span class="sxs-lookup"><span data-stu-id="1cc73-301">Click **X** tooclose **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="1cc73-302">A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-302">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="1cc73-303">Láthatja, hogy hello szelet, amely folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="1cc73-303">You see that hello slice that is currently being processed.</span></span>

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="1cc73-305">Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="1cc73-305">When processing is done, you see hello slice in **Ready** state.</span></span>

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="1cc73-307">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="1cc73-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="1cc73-308">Ezért várt hello folyamat túl érvénybe **körülbelül 30 percet** tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="1cc73-308">Therefore, expect hello pipeline too      take **approximately 30 minutes** tooprocess hello slice.</span></span>
   >
   >

9. <span data-ttu-id="1cc73-309">Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-309">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

   ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="1cc73-311">Hello szelet toosee részleteit a kattintson egy **adatszelet** panelen.</span><span class="sxs-lookup"><span data-stu-id="1cc73-311">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

   ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="1cc73-313">Hello futtatása tevékenység kattintson **tevékenység fut lista** toosee adatait egy tevékenység futtatását (esetünkben Hive tevékenység) egy **tevékenység fut részletek** ablak.</span><span class="sxs-lookup"><span data-stu-id="1cc73-313">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="1cc73-315">Hello naplófájlokból láthatja, hogy végre lett hajtva hello Hive-lekérdezések és állapotára vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-315">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="1cc73-316">A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="1cc73-317">További részletekért tekintse meg a [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) (Folyamatok figyelése és felügyelete az Azure Portal paneleinek használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="1cc73-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1cc73-318">hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="1cc73-318">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="1cc73-319">Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.</span><span class="sxs-lookup"><span data-stu-id="1cc73-319">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="1cc73-320">Folyamat figyelése a Monitor & Manage alkalmazással</span><span class="sxs-lookup"><span data-stu-id="1cc73-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="1cc73-321">Is figyelheti, & alkalmazás toomonitor kezelése a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="1cc73-321">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="1cc73-322">Az alkalmazás használatával kapcsolatos részletes információkért olvassa el a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="1cc73-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="1cc73-323">Kattintson a **figyelő & kezelése** hello kezdőlap a data factory csempére.</span><span class="sxs-lookup"><span data-stu-id="1cc73-323">Click **Monitor & Manage** tile on hello home page for your data factory.</span></span>

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="1cc73-325">Meg kell jelennie a **Monitor & Manage alkalmazásnak**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="1cc73-326">Változás hello **kezdési időpont** és **befejező időpontja** toomatch indítsa el és befejezési időpontja, a folyamat, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-326">Change hello **Start time** and **End time** toomatch start and end times of your pipeline, and click **Apply**.</span></span>

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="1cc73-328">Egy tevékenység ablakban válassza a hello **tevékenység Windows** listában toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="1cc73-328">Select an activity window in hello **Activity Windows** list toosee details about it.</span></span>

    ![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="1cc73-330">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="1cc73-330">Summary</span></span>
<span data-ttu-id="1cc73-331">Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1cc73-331">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="1cc73-332">A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:</span><span class="sxs-lookup"><span data-stu-id="1cc73-332">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="1cc73-333">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="1cc73-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="1cc73-334">Létrehozott két **társított szolgáltatást**:</span><span class="sxs-lookup"><span data-stu-id="1cc73-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="1cc73-335">**Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.</span><span class="sxs-lookup"><span data-stu-id="1cc73-335">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="1cc73-336">**Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-336">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="1cc73-337">Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1cc73-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="1cc73-338">Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="1cc73-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="1cc73-339">Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="1cc73-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1cc73-340">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1cc73-340">Next Steps</span></span>
<span data-ttu-id="1cc73-341">Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="1cc73-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="1cc73-342">Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="1cc73-342">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="1cc73-343">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1cc73-343">See Also</span></span>
| <span data-ttu-id="1cc73-344">Témakör</span><span class="sxs-lookup"><span data-stu-id="1cc73-344">Topic</span></span> | <span data-ttu-id="1cc73-345">Leírás</span><span class="sxs-lookup"><span data-stu-id="1cc73-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="1cc73-346">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="1cc73-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="1cc73-347">Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket.</span><span class="sxs-lookup"><span data-stu-id="1cc73-347">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="1cc73-348">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="1cc73-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="1cc73-349">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="1cc73-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="1cc73-350">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="1cc73-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="1cc73-351">Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait.</span><span class="sxs-lookup"><span data-stu-id="1cc73-351">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="1cc73-352">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="1cc73-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="1cc73-353">Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1cc73-353">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
