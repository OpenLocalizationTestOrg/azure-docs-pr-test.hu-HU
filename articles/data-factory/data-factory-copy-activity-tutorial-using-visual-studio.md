---
title: "Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Visual Studio használatával | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre másolási tevékenységgel rendelkező Azure Data Factory-folyamatot a Visual Studio használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a><span data-ttu-id="42d00-103">Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="42d00-103">Tutorial: Create a pipeline with Copy Activity using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="42d00-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42d00-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="42d00-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="42d00-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="42d00-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42d00-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="42d00-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42d00-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="42d00-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42d00-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="42d00-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="42d00-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="42d00-110">REST API</span><span class="sxs-lookup"><span data-stu-id="42d00-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="42d00-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="42d00-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="42d00-112">Ebből a cikkből megismerheti, hogyan toouse hello Microsoft Visual Studio toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="42d00-112">In this article, you learn how toouse hello Microsoft Visual Studio toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="42d00-113">Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="42d00-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="42d00-114">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="42d00-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="42d00-115">hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="42d00-116">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="42d00-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="42d00-117">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="42d00-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="42d00-118">További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="42d00-119">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="42d00-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="42d00-120">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="42d00-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="42d00-121">További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="42d00-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE] 
> <span data-ttu-id="42d00-122">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-122">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="42d00-123">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-123">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42d00-124">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42d00-124">Prerequisites</span></span>
1. <span data-ttu-id="42d00-125">Olvassa végig [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="42d00-125">Read through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article and complete hello **prerequisite** steps.</span></span>       
2. <span data-ttu-id="42d00-126">toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="42d00-126">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
3. <span data-ttu-id="42d00-127">A számítógépen telepített hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="42d00-127">You must have hello following installed on your computer:</span></span> 
   * <span data-ttu-id="42d00-128">Visual Studio 2013 vagy Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="42d00-128">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="42d00-129">Töltse le az Azure SDK-t a Visual Studio 2013-hoz vagy a Visual Studio 2015-höz.</span><span class="sxs-lookup"><span data-stu-id="42d00-129">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="42d00-130">Keresse meg a túl[Azure letöltési oldalát](https://azure.microsoft.com/downloads/) kattintson **VS 2013** vagy **VS 2015** a hello **.NET** szakasz.</span><span class="sxs-lookup"><span data-stu-id="42d00-130">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="42d00-131">Töltse le a legfrissebb hello Azure Data Factory beépülő modul a Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="42d00-131">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="42d00-132">Hello beépülő modult is frissítheti hello lépések végrehajtásával: hello menüjének **eszközök** -> **bővítmények és frissítések** -> **Online**  ->  **Visual Studio galériában** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="42d00-132">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

## <a name="steps"></a><span data-ttu-id="42d00-133">Lépések</span><span class="sxs-lookup"><span data-stu-id="42d00-133">Steps</span></span>
<span data-ttu-id="42d00-134">Ez az oktatóanyag részeként végrehajtandó hello lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="42d00-134">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="42d00-135">Hozzon létre **összekapcsolt szolgáltatások** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-135">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="42d00-136">Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="42d00-136">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="42d00-137">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-137">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="42d00-138">Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-138">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="42d00-139">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="42d00-139">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="42d00-140">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="42d00-140">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="42d00-141">Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="42d00-141">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>     
2. <span data-ttu-id="42d00-142">Hozzon létre a bemeneti és kimeneti **adatkészletek** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-142">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="42d00-143">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="42d00-143">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="42d00-144">És hello bemeneti blob-adathalmazra hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-144">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="42d00-145">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-145">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="42d00-146">És hello kimeneti SQL táblázat dataset megadja hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.</span><span class="sxs-lookup"><span data-stu-id="42d00-146">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
3. <span data-ttu-id="42d00-147">Hozzon létre egy **csővezeték** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-147">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="42d00-148">Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.</span><span class="sxs-lookup"><span data-stu-id="42d00-148">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="42d00-149">hello másolási tevékenység hello Azure blob storage tooa tábla hello Azure SQL-adatbázis egy blobot másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-149">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="42d00-150">A másolási tevékenység használható egy folyamat toocopy adatokat bármely támogatott forrás támogatott tooany cél.</span><span class="sxs-lookup"><span data-stu-id="42d00-150">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="42d00-151">A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="42d00-151">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
4. <span data-ttu-id="42d00-152">Hozzon létre egy Azure **adat-előállítót** Data Factory-entitások (összekapcsolt szolgáltatások adatkészletek/táblák és folyamatok) telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="42d00-152">Create an Azure **data factory** when deploying Data Factory entities (linked services, datasets/tables, and pipelines).</span></span> 

## <a name="create-visual-studio-project"></a><span data-ttu-id="42d00-153">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="42d00-154">Indítsa el a **Visual Studio 2015-öt**.</span><span class="sxs-lookup"><span data-stu-id="42d00-154">Launch **Visual Studio 2015**.</span></span> <span data-ttu-id="42d00-155">Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="42d00-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="42d00-156">Megtekintheti az hello **új projekt** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="42d00-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="42d00-157">A hello **új projekt** párbeszédpanelen jelölje be hello **DataFactory** sablont, majd kattintson **üres Data Factory-projektekhez**.</span><span class="sxs-lookup"><span data-stu-id="42d00-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>  
   
    ![A New project (Új projekt) párbeszédpanel](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. <span data-ttu-id="42d00-159">Adja meg a hello projekt, a hely hello megoldás hello nevét, és hello megoldás nevét, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="42d00-159">Specify hello name of hello project, location for hello solution, and name of hello solution, and then click **OK**.</span></span>
   
    ![Megoldáskezelő](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a><span data-ttu-id="42d00-161">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-161">Create linked services</span></span>
<span data-ttu-id="42d00-162">A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="42d00-162">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="42d00-163">Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics).</span><span class="sxs-lookup"><span data-stu-id="42d00-163">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="42d00-164">Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél).</span><span class="sxs-lookup"><span data-stu-id="42d00-164">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="42d00-165">Ezért a következő két típusú társított szolgáltatást hozza létre: AzureStorage és AzureSQLDatabase.</span><span class="sxs-lookup"><span data-stu-id="42d00-165">Therefore, you create two linked services of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="42d00-166">hello Azure Storage társított szolgáltatás hivatkozások a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-166">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="42d00-167">Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-167">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="42d00-168">Az Azure SQL társított szolgáltatás hivatkozások az Azure SQL adatbázis toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-168">Azure SQL linked service links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="42d00-169">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="42d00-169">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="42d00-170">Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-170">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="42d00-171">Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási.</span><span class="sxs-lookup"><span data-stu-id="42d00-171">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="42d00-172">Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott.</span><span class="sxs-lookup"><span data-stu-id="42d00-172">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="42d00-173">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája.</span><span class="sxs-lookup"><span data-stu-id="42d00-173">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span> <span data-ttu-id="42d00-174">Az oktatóanyag során ne használjon számítási szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-174">In this tutorial, you do not use any compute service.</span></span> 

### <a name="create-hello-azure-storage-linked-service"></a><span data-ttu-id="42d00-175">Hello Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-175">Create hello Azure Storage linked service</span></span>
1. <span data-ttu-id="42d00-176">A **Megoldáskezelőben**, kattintson a jobb gombbal **összekapcsolt szolgáltatások**, pont túl**Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-176">In **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="42d00-177">A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Storage társított szolgáltatás** hello listából, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="42d00-177">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span> 
   
    ![Új társított szolgáltatás](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. <span data-ttu-id="42d00-179">Cserélje le `<accountname>` és `<accountkey>`* hello nevű Azure-tárfiókot és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="42d00-179">Replace `<accountname>` and `<accountkey>`* with hello name of your Azure storage account and its key.</span></span> 
   
    ![Azure Storage társított szolgáltatás](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. <span data-ttu-id="42d00-181">Mentse a hello **AzureStorageLinkedService1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-181">Save hello **AzureStorageLinkedService1.json** file.</span></span>

    <span data-ttu-id="42d00-182">JSON-tulajdonságokat hello társított szolgáltatás definíciójának kapcsolatos további információkért lásd: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk.</span><span class="sxs-lookup"><span data-stu-id="42d00-182">For more information about JSON properties in hello linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-hello-azure-sql-linked-service"></a><span data-ttu-id="42d00-183">Hello Azure SQL társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-183">Create hello Azure SQL linked service</span></span>
1. <span data-ttu-id="42d00-184">Kattintson a jobb gombbal a **összekapcsolt szolgáltatások** hello csomópontja **Megoldáskezelőben** újra, mutasson a túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-184">Right-click on **Linked Services** node in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span> 
2. <span data-ttu-id="42d00-185">Ezúttal válassza a **Azure SQL Linked Service** (Azure SQL társított szolgáltatás) lehetőséget, és kattintson az **Add** (Hozzáadás) elemre.</span><span class="sxs-lookup"><span data-stu-id="42d00-185">This time, select **Azure SQL Linked Service**, and click **Add**.</span></span> 
3. <span data-ttu-id="42d00-186">A hello **AzureSqlLinkedService1.json fájl**, cserélje le `<servername>`, `<databasename>`, `<username@servername>`, és `<password>` nevekkel az Azure SQL server, az adatbázis, a felhasználói fiókot, és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="42d00-186">In hello **AzureSqlLinkedService1.json file**, replace `<servername>`, `<databasename>`, `<username@servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span>    
4. <span data-ttu-id="42d00-187">Mentse a hello **AzureSqlLinkedService1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-187">Save hello **AzureSqlLinkedService1.json** file.</span></span> 
    
    <span data-ttu-id="42d00-188">További információ a JSON-tulajdonságokról: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="42d00-188">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>


## <a name="create-datasets"></a><span data-ttu-id="42d00-189">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-189">Create datasets</span></span>
<span data-ttu-id="42d00-190">Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-190">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="42d00-191">Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService1 és AzureSqlLinkedService1 által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-191">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService1 and AzureSqlLinkedService1 respectively.</span></span>

<span data-ttu-id="42d00-192">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="42d00-192">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="42d00-193">És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-193">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="42d00-194">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-194">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="42d00-195">És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-195">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="42d00-196">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-196">Create input dataset</span></span>
<span data-ttu-id="42d00-197">Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService1 kapcsolódó szolgáltatás által képviselt hello.</span><span class="sxs-lookup"><span data-stu-id="42d00-197">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService1 linked service.</span></span> <span data-ttu-id="42d00-198">Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="42d00-198">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="42d00-199">Ebben az oktatóanyagban hello fájlnév értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="42d00-199">In this tutorial, you specify a value for hello fileName.</span></span> 

<span data-ttu-id="42d00-200">Itt használja a hello kifejezés "tábla" helyett "adatkészletek".</span><span class="sxs-lookup"><span data-stu-id="42d00-200">Here, you use hello term "tables" rather than "datasets".</span></span> <span data-ttu-id="42d00-201">Egy tábla egy négyszögletes dataset és hello csak ilyen típusú adatkészlet most támogatott.</span><span class="sxs-lookup"><span data-stu-id="42d00-201">A table is a rectangular dataset and is hello only type of dataset supported right now.</span></span> 

1. <span data-ttu-id="42d00-202">Kattintson a jobb gombbal **táblák** a hello **megoldáskezelő**, pont túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-202">Right-click **Tables** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="42d00-203">A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Blob**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="42d00-203">In hello **Add New Item** dialog box, select **Azure Blob**, and click **Add**.</span></span>   
3. <span data-ttu-id="42d00-204">Cserélje le a következő szöveg hello hello JSON-szöveg, és mentse a hello **AzureBlobLocation1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-204">Replace hello JSON text with hello following text and save hello **AzureBlobLocation1.json** file.</span></span> 

  ```json   
  {
    "name": "InputDataset",
    "properties": {
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
      "type": "AzureBlob",
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
        "format": {
          "type": "TextFormat",
          "columnDelimiter": ","
        }
      },
      "external": true,
      "availability": {
        "frequency": "Hour",
        "interval": 1
      }
    }
  }
  ``` 
    <span data-ttu-id="42d00-205">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="42d00-205">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="42d00-206">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="42d00-206">Property</span></span> | <span data-ttu-id="42d00-207">Leírás</span><span class="sxs-lookup"><span data-stu-id="42d00-207">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="42d00-208">type</span><span class="sxs-lookup"><span data-stu-id="42d00-208">type</span></span> | <span data-ttu-id="42d00-209">hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="42d00-209">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="42d00-210">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="42d00-210">linkedServiceName</span></span> | <span data-ttu-id="42d00-211">Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="42d00-211">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="42d00-212">folderPath</span><span class="sxs-lookup"><span data-stu-id="42d00-212">folderPath</span></span> | <span data-ttu-id="42d00-213">Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB.</span><span class="sxs-lookup"><span data-stu-id="42d00-213">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="42d00-214">Ebben az oktatóanyagban adftutorial hello blobtárolót és hello legfelső szintű mappa.</span><span class="sxs-lookup"><span data-stu-id="42d00-214">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="42d00-215">fileName</span><span class="sxs-lookup"><span data-stu-id="42d00-215">fileName</span></span> | <span data-ttu-id="42d00-216">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="42d00-216">This property is optional.</span></span> <span data-ttu-id="42d00-217">Ha kihagyja ezt a tulajdonságot, leltárhoz hello folderPath lévő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-217">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="42d00-218">Ebben az oktatóanyagban **emp.txt** hello fájlnév, hogy csak adott fájl van felvételre feldolgozásra számára megadott.</span><span class="sxs-lookup"><span data-stu-id="42d00-218">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="42d00-219">formátum -> típus</span><span class="sxs-lookup"><span data-stu-id="42d00-219">format -> type</span></span> |<span data-ttu-id="42d00-220">hello bemeneti fájl hello szöveg formátumban van, így használjuk **szöveges**.</span><span class="sxs-lookup"><span data-stu-id="42d00-220">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="42d00-221">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="42d00-221">columnDelimiter</span></span> | <span data-ttu-id="42d00-222">hello bemeneti fájl hello oszlopai határolja **vesszővel karakter (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="42d00-222">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="42d00-223">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="42d00-223">frequency/interval</span></span> | <span data-ttu-id="42d00-224">hello gyakoriságának beállítása túl**óra** és időköz értéke túl**1**, ami azt jelenti, hogy hello bemeneti szeletek érhetők el **óránkénti**.</span><span class="sxs-lookup"><span data-stu-id="42d00-224">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="42d00-225">Más szóval hello Data Factory szolgáltatásnak megkeresi a bemeneti adatok óránként blob tároló hello gyökérmappájában (**adftutorial**) megadott.</span><span class="sxs-lookup"><span data-stu-id="42d00-225">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="42d00-226">Hello adatainak hello folyamat kezdési és befejezési időpontokat, nem előtt vagy után ezekben az időszakokban keresi.</span><span class="sxs-lookup"><span data-stu-id="42d00-226">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="42d00-227">external</span><span class="sxs-lookup"><span data-stu-id="42d00-227">external</span></span> | <span data-ttu-id="42d00-228">Ez a tulajdonság értéke túl**igaz** hello adatok nem jön létre, ez az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="42d00-228">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="42d00-229">Ebben az oktatóanyagban hello a bemeneti adatok nem jön létre a folyamat, így ez a tulajdonság tootrue hivatott hello emp.txt fájl van.</span><span class="sxs-lookup"><span data-stu-id="42d00-229">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="42d00-230">Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.</span><span class="sxs-lookup"><span data-stu-id="42d00-230">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>   

### <a name="create-output-dataset"></a><span data-ttu-id="42d00-231">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-231">Create output dataset</span></span>
<span data-ttu-id="42d00-232">Ebben a lépésben egy kimeneti adatkészletet hoz létre **OutputDataset** néven.</span><span class="sxs-lookup"><span data-stu-id="42d00-232">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="42d00-233">Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="42d00-233">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService1**.</span></span> 

1. <span data-ttu-id="42d00-234">Kattintson a jobb gombbal **táblák** hello a **Megoldáskezelőben** újra, mutasson a túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-234">Right-click **Tables** in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="42d00-235">A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure SQL**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="42d00-235">In hello **Add New Item** dialog box, select **Azure SQL**, and click **Add**.</span></span> 
3. <span data-ttu-id="42d00-236">Cserélje le a következő JSON hello hello JSON-szöveg, és mentse a hello **AzureSqlTableLocation1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-236">Replace hello JSON text with hello following JSON and save hello **AzureSqlTableLocation1.json** file.</span></span>

  ```json
    {
     "name": "OutputDataset",
     "properties": {
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
       "type": "AzureSqlTable",
       "linkedServiceName": "AzureSqlLinkedService1",
       "typeProperties": {
         "tableName": "emp"
       },
       "availability": {
         "frequency": "Hour",
         "interval": 1
       }
     }
    }
    ```
    <span data-ttu-id="42d00-237">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="42d00-237">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="42d00-238">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="42d00-238">Property</span></span> | <span data-ttu-id="42d00-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="42d00-239">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="42d00-240">type</span><span class="sxs-lookup"><span data-stu-id="42d00-240">type</span></span> | <span data-ttu-id="42d00-241">hello type tulajdonság beállítása túl**AzureSqlTable** mert adatok másolt tooa tábla Azure SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="42d00-241">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="42d00-242">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="42d00-242">linkedServiceName</span></span> | <span data-ttu-id="42d00-243">Toohello hivatkozik **AzureSqlLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="42d00-243">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="42d00-244">tableName</span><span class="sxs-lookup"><span data-stu-id="42d00-244">tableName</span></span> | <span data-ttu-id="42d00-245">Megadott hello **tábla** toowhich hello adatok másolását.</span><span class="sxs-lookup"><span data-stu-id="42d00-245">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="42d00-246">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="42d00-246">frequency/interval</span></span> | <span data-ttu-id="42d00-247">hello gyakoriságának beállítása túl**óra** és időköz **1**, ami azt jelenti, hogy hello kimeneti szeletek előállítása **óránkénti** közötti hello folyamat kezdési és befejezési időpontokat, előtte vagy Miután ezekben az időszakokban.</span><span class="sxs-lookup"><span data-stu-id="42d00-247">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="42d00-248">Három oszlop – **azonosító**, **Keresztnév**, és **Vezetéknév** – hello üres tábla hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="42d00-248">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="42d00-249">Azonosító: azonosító oszlopot, ezért meg kell, hogy csak toospecify **Keresztnév** és **Vezetéknév** itt.</span><span class="sxs-lookup"><span data-stu-id="42d00-249">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="42d00-250">További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="42d00-250">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

## <a name="create-pipeline"></a><span data-ttu-id="42d00-251">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="42d00-251">Create pipeline</span></span>
<span data-ttu-id="42d00-252">Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="42d00-252">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="42d00-253">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést.</span><span class="sxs-lookup"><span data-stu-id="42d00-253">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="42d00-254">Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet.</span><span class="sxs-lookup"><span data-stu-id="42d00-254">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="42d00-255">hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra.</span><span class="sxs-lookup"><span data-stu-id="42d00-255">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="42d00-256">Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="42d00-256">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="42d00-257">Kattintson a jobb gombbal **folyamatok** hello a **Megoldáskezelőben**, pont túl**hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-257">Right-click **Pipelines** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>  
2. <span data-ttu-id="42d00-258">Válassza ki **másolási adatok csővezeték** a hello **új elem hozzáadása** párbeszédpanel megnyitásához, és kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="42d00-258">Select **Copy Data Pipeline** in hello **Add New Item** dialog box and click **Add**.</span></span> 
3. <span data-ttu-id="42d00-259">Hello JSON cserélje le a következő JSON hello, és mentse a hello **CopyActivity1.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="42d00-259">Replace hello JSON with hello following JSON and save hello **CopyActivity1.json** file.</span></span>

  ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob tooAzure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
             }
           ],
           "typeProperties": {
             "source": {
               "type": "BlobSource"
             },
             "sink": {
               "type": "SqlSink",
               "writeBatchSize": 10000,
               "writeBatchTimeout": "60:00:00"
             }
           },
           "Policy": {
             "concurrency": 1,
             "executionPriorityOrder": "NewestFirst",
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - <span data-ttu-id="42d00-260">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="42d00-260">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="42d00-261">Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-261">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="42d00-262">A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.</span><span class="sxs-lookup"><span data-stu-id="42d00-262">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="42d00-263">Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="42d00-263">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="42d00-264">A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.</span><span class="sxs-lookup"><span data-stu-id="42d00-264">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="42d00-265">Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="42d00-265">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="42d00-266">Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.</span><span class="sxs-lookup"><span data-stu-id="42d00-266">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="42d00-267">Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket.</span><span class="sxs-lookup"><span data-stu-id="42d00-267">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="42d00-268">Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja.</span><span class="sxs-lookup"><span data-stu-id="42d00-268">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="42d00-269">Például "2016-02-03", amely túl egyenértékű "2016-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="42d00-269">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="42d00-270">Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni.</span><span class="sxs-lookup"><span data-stu-id="42d00-270">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="42d00-271">Például: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="42d00-271">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="42d00-272">Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk.</span><span class="sxs-lookup"><span data-stu-id="42d00-272">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="42d00-273">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**".</span><span class="sxs-lookup"><span data-stu-id="42d00-273">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="42d00-274">toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="42d00-274">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="42d00-275">A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.</span><span class="sxs-lookup"><span data-stu-id="42d00-275">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="42d00-276">A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-276">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="42d00-277">A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-277">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="42d00-278">A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-278">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="42d00-279">Az SqlSink által támogatott JSON-tulajdonságok leírása az [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md) című cikkben található.</span><span class="sxs-lookup"><span data-stu-id="42d00-279">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="42d00-280">Data Factory-entitások közzététele/üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="42d00-280">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="42d00-281">Ebben a lépésben a korábban létrehozott Data Factory-entitásokat (társított szolgáltatások, adatkészletek és folyamat) teszi közzé.</span><span class="sxs-lookup"><span data-stu-id="42d00-281">In this step, you publish Data Factory entities (linked services, datasets, and pipeline) you created earlier.</span></span> <span data-ttu-id="42d00-282">Is meg hello új data factory toobe hello neve létrehozott toohold ezeket az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-282">You also specify hello name of hello new data factory toobe created toohold these entities.</span></span>  

1. <span data-ttu-id="42d00-283">Kattintson a jobb gombbal a projektre a Solution Explorer hello, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="42d00-283">Right-click project in hello Solution Explorer, and click **Publish**.</span></span> 
2. <span data-ttu-id="42d00-284">Ha látja **jelentkezzen be Microsoft-fiók tooyour** párbeszédpanelen adja meg Azure-előfizetéssel rendelkező hello fiók hitelesítő adatait, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="42d00-284">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="42d00-285">A következő párbeszédpanel hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="42d00-285">You should see hello following dialog box:</span></span>
   
   ![Publish (Közzététel) párbeszédpanel](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. <span data-ttu-id="42d00-287">Hello konfigurálása data factory lapon hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="42d00-287">In hello Configure data factory page, do hello following steps:</span></span> 
   
   1. <span data-ttu-id="42d00-288">Válassza a **Create New Data Factory** (Új data factory létrehozása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="42d00-288">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="42d00-289">A **Name** (Név) mezőbe írja be a következőt: **VSTutorialFactory**.</span><span class="sxs-lookup"><span data-stu-id="42d00-289">Enter **VSTutorialFactory** for **Name**.</span></span>  
      
      > [!IMPORTANT]
      > <span data-ttu-id="42d00-290">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="42d00-290">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="42d00-291">Ha közzétételekor adat-előállító nevét hello kapcsolatos hibaüzenetet kap, hello data factory (például yournameVSTutorialFactory), és próbálja meg újra közzétenni hello nevének módosítása.</span><span class="sxs-lookup"><span data-stu-id="42d00-291">If you receive an error about hello name of data factory when publishing, change hello name of hello data factory (for example, yournameVSTutorialFactory) and try publishing again.</span></span> <span data-ttu-id="42d00-292">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="42d00-292">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>        
      > 
      > 
   3. <span data-ttu-id="42d00-293">Válassza ki az Azure-előfizetéshez tartozó hello **előfizetés** mező.</span><span class="sxs-lookup"><span data-stu-id="42d00-293">Select your Azure subscription for hello **Subscription** field.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="42d00-294">Ha nem látja bármely előfizetés, győződjön meg arról, hogy olyan fiókkal, amely egy rendszergazdai vagy társadminisztrátori hello előfizetés naplóba.</span><span class="sxs-lookup"><span data-stu-id="42d00-294">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>  
      > 
      > 
   4. <span data-ttu-id="42d00-295">Jelölje be hello **erőforráscsoport** a hello data factory toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="42d00-295">Select hello **resource group** for hello data factory toobe created.</span></span> 
   5. <span data-ttu-id="42d00-296">Jelölje be hello **régió** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="42d00-296">Select hello **region** for hello data factory.</span></span> <span data-ttu-id="42d00-297">Csak a Data Factory szolgáltatásnak hello által támogatott régiók hello legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="42d00-297">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
   6. <span data-ttu-id="42d00-298">Kattintson a **következő** tooswitch toohello **elemek közzététele** lap.</span><span class="sxs-lookup"><span data-stu-id="42d00-298">Click **Next** tooswitch toohello **Publish Items** page.</span></span>
      
       ![Configure data factory (Data factory konfigurálása) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. <span data-ttu-id="42d00-300">A hello **elemek közzététele** lapon, győződjön meg arról, hogy az összes hello adat-előállítók entitások van kiválasztva, és kattintson a **következő** tooswitch toohello **összegzés** lap.</span><span class="sxs-lookup"><span data-stu-id="42d00-300">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>
   
   ![Publish items (Elemek közzététele) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. <span data-ttu-id="42d00-302">Olvassa el az összesítő hello és a **következő** toostart hello központi telepítési folyamat és a nézet hello **központi telepítési állapot**.</span><span class="sxs-lookup"><span data-stu-id="42d00-302">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>
   
   ![Publish summary (Összefoglaló közzététele) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. <span data-ttu-id="42d00-304">A hello **központi telepítési állapot** lapon hello telepítési folyamatának állapotát hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="42d00-304">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="42d00-305">Hello a telepítés befejezése után kattintson a Befejezés gombra.</span><span class="sxs-lookup"><span data-stu-id="42d00-305">Click Finish after hello deployment is done.</span></span>
 
   ![Üzembe helyezés állapota lap](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

<span data-ttu-id="42d00-307">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="42d00-307">Note hello following points:</span></span> 

* <span data-ttu-id="42d00-308">Ha hello hibaüzenet: "Ez az előfizetés nem regisztrált toouse névtér Microsoft.DataFactory", hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="42d00-308">If you receive hello error: "This subscription is not registered toouse namespace Microsoft.DataFactory", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="42d00-309">Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="42d00-309">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="42d00-310">A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="42d00-310">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="42d00-311">Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="42d00-311">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="42d00-312">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="42d00-312">This action automatically registers hello provider for you.</span></span>
* <span data-ttu-id="42d00-313">hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="42d00-313">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42d00-314">toocreate adat-előállító esetekben kell toobe egy rendszergazdai vagy társadminisztrátori hello Azure-előfizetés az</span><span class="sxs-lookup"><span data-stu-id="42d00-314">toocreate Data Factory instances, you need toobe a admin/co-admin of hello Azure subscription</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="42d00-315">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="42d00-315">Monitor pipeline</span></span>
<span data-ttu-id="42d00-316">Keresse meg a data factory toohello kezdőlapját:</span><span class="sxs-lookup"><span data-stu-id="42d00-316">Navigate toohello home page for your data factory:</span></span>

1. <span data-ttu-id="42d00-317">Jelentkezzen be túl[Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="42d00-317">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="42d00-318">Kattintson a **további szolgáltatások** hello bal oldali menüben, és kattintson a **adat-előállítók**.</span><span class="sxs-lookup"><span data-stu-id="42d00-318">Click **More services** on hello left menu, and click **Data factories**.</span></span>

    ![Data factoryk tallózása](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. <span data-ttu-id="42d00-320">Kezdje el beírni a data factory hello nevét.</span><span class="sxs-lookup"><span data-stu-id="42d00-320">Start typing hello name of your data factory.</span></span>

    ![Adat-előállító neve](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. <span data-ttu-id="42d00-322">Kattintson a data factory a hello eredmények lista toosee hello kezdőlapját a data factory.</span><span class="sxs-lookup"><span data-stu-id="42d00-322">Click your data factory in hello results list toosee hello home page for your data factory.</span></span>

    ![Data factory kezdőlap](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. <span data-ttu-id="42d00-324">Kövesse az utasításokat [adatkészletek és a folyamat figyelése](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello feldolgozási sorban lévő és adatkészletek ebben az oktatóanyagban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="42d00-324">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="42d00-325">A Visual Studio jelenleg nem támogatja a Data Factory-folyamatok monitorozását.</span><span class="sxs-lookup"><span data-stu-id="42d00-325">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span> 

## <a name="summary"></a><span data-ttu-id="42d00-326">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="42d00-326">Summary</span></span>
<span data-ttu-id="42d00-327">Ebben az oktatóanyagban létrehozott egy Azure data factory toocopy adatok Azure blob tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="42d00-327">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="42d00-328">Visual Studio toocreate hello adat-előállító, a társított szolgáltatások, a adatkészletek és a folyamat használta.</span><span class="sxs-lookup"><span data-stu-id="42d00-328">You used Visual Studio toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="42d00-329">Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42d00-329">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="42d00-330">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="42d00-330">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="42d00-331">**Társított szolgáltatásokat** hozott létre:</span><span class="sxs-lookup"><span data-stu-id="42d00-331">Created **linked services**:</span></span>
   1. <span data-ttu-id="42d00-332">Egy **Azure Storage** kapcsolódó szolgáltatás toolink az Azure Storage-fiók, amely a bemeneti adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="42d00-332">An **Azure Storage** linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="42d00-333">Egy **Azure SQL** kapcsolódó szolgáltatás toolink az Azure SQL-adatbázis, amely tárolja a hello kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-333">An **Azure SQL** linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="42d00-334">**Adatkészleteket** hozott létre, amelyek az adatcsatorna bemeneti és kimeneti adatait írják le.</span><span class="sxs-lookup"><span data-stu-id="42d00-334">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="42d00-335">Létrehozott egy **folyamatot** egy **Másolási tevékenységgel**, ahol a **BlobSource** a forrás, az **SqlSink** pedig a fogadó.</span><span class="sxs-lookup"><span data-stu-id="42d00-335">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span> 

<span data-ttu-id="42d00-336">Hogyan toouse egy HDInsight Hive tevékenység tootransform adatok Azure HDInsight-fürt használatával: toosee [ oktatóanyag: az első adatcsatorna tootransform adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="42d00-336">toosee how toouse a HDInsight Hive Activity tootransform data by using Azure HDInsight cluster, see [ Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="42d00-337">Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után).</span><span class="sxs-lookup"><span data-stu-id="42d00-337">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="42d00-338">Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="42d00-338">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 

## <a name="view-all-data-factories-in-server-explorer"></a><span data-ttu-id="42d00-339">Az összes adat-előállító megjelenítése a Kiszolgálókezelőben (Server Explorer)</span><span class="sxs-lookup"><span data-stu-id="42d00-339">View all data factories in Server Explorer</span></span>
<span data-ttu-id="42d00-340">Ez a szakasz ismerteti, hogyan toouse tooview Visual Studio Server Explorer hello összes hello adat-előállítók az Azure-előfizetése, és valamelyik adat-előállítót alapján Visual Studio-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="42d00-340">This section describes how toouse hello Server Explorer in Visual Studio tooview all hello data factories in your Azure subscription and create a Visual Studio project based on an existing data factory.</span></span> 

1. <span data-ttu-id="42d00-341">A **Visual Studio**, kattintson a **nézet** hello menü, és kattintson a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="42d00-341">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="42d00-342">Hello Server Explorer-ablakban bontsa ki a **Azure** csomópontot **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="42d00-342">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="42d00-343">Ha látja **tooVisual Studio bejelentkezés**, adja meg a hello **fiók** társított a Azure-előfizetéssel, és kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="42d00-343">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="42d00-344">Adja meg a **jelszót**, és kattintson a **Sign in** (Bejelentkezés) elemre.</span><span class="sxs-lookup"><span data-stu-id="42d00-344">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="42d00-345">A Visual Studio megpróbál az előfizetés az összes Azure adat-előállítók tooget információt.</span><span class="sxs-lookup"><span data-stu-id="42d00-345">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="42d00-346">Ez a művelet hello hello állapotát látja **Data Factory feladatlista** ablak.</span><span class="sxs-lookup"><span data-stu-id="42d00-346">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Server Explorer (Kiszolgálókezelő)](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a><span data-ttu-id="42d00-348">Visual Studio-projekt létrehozása egy meglévő adat-előállító alapján</span><span class="sxs-lookup"><span data-stu-id="42d00-348">Create a Visual Studio project for an existing data factory</span></span>

- <span data-ttu-id="42d00-349">Kattintson a jobb gombbal a Server Explorer egy adat-előállítót, és válassza ki **exportálása adat-előállító tooNew projekt** toocreate Visual Studio-projekt valamelyik adat-előállítót alapján.</span><span class="sxs-lookup"><span data-stu-id="42d00-349">Right-click a data factory in Server Explorer, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Exportálja a data factory tooa VS projektet](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="42d00-351">Visual Studióhoz készült Data Factory-eszközök frissítése</span><span class="sxs-lookup"><span data-stu-id="42d00-351">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="42d00-352">tooupdate Azure Data Factory tools for Visual Studio hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="42d00-352">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="42d00-353">Kattintson a **eszközök** hello menüre, majd válassza a **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="42d00-353">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span> 
2. <span data-ttu-id="42d00-354">Válassza ki **frissítések** a hello bal oldali ablaktáblán, és válassza ki **Visual Studio galériában**.</span><span class="sxs-lookup"><span data-stu-id="42d00-354">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="42d00-355">Válassza az **Azure Data Factory tools for Visual Studio** lehetőséget, és kattintson az **Update** (Frissítés) elemre.</span><span class="sxs-lookup"><span data-stu-id="42d00-355">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="42d00-356">Ez a bejegyzés nem látható, ha már rendelkezik hello hello eszközök legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="42d00-356">If you do not see this entry, you already have hello latest version of hello tools.</span></span> 

## <a name="use-configuration-files"></a><span data-ttu-id="42d00-357">Konfigurációs fájlok használata</span><span class="sxs-lookup"><span data-stu-id="42d00-357">Use configuration files</span></span>
<span data-ttu-id="42d00-358">Minden környezet eltérően a szolgáltatások/táblák/folyamatok csatolt konfigurációs fájlokat a Visual Studio tooconfigure tulajdonságai használható.</span><span class="sxs-lookup"><span data-stu-id="42d00-358">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="42d00-359">Vegye figyelembe a következő az Azure tárolás társított szolgáltatásának JSON-definícióból hello.</span><span class="sxs-lookup"><span data-stu-id="42d00-359">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="42d00-360">toospecify **connectionString** accountname és az accountkey elemeket hello (fejlesztési/tesztelési/éles) környezetben toowhich alapján különböző értékekkel a Data Factory entitások telepít.</span><span class="sxs-lookup"><span data-stu-id="42d00-360">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="42d00-361">Az ilyen működést úgy érheti el, hogy minden környezethez külön konfigurációs fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="42d00-361">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="42d00-362">Konfigurációs fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42d00-362">Add a configuration file</span></span>
<span data-ttu-id="42d00-363">Minden környezet konfigurációs fájl felvétele hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="42d00-363">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="42d00-364">Hello adat-előállító projektre a Visual Studio megoldásnak a jobb gombbal túl**Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="42d00-364">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="42d00-365">Válassza ki **Config** hello bal oldali telepített sablonok hello listában jelölje ki **konfigurációs fájl**, adjon meg egy **neve** hello konfiguráció fájlt, és kattintson a **Hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="42d00-365">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Konfigurációs fájl hozzáadása](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="42d00-367">Adja hozzá a következő formátumban hello konfigurációs paramétereit és azok értékét:</span><span class="sxs-lookup"><span data-stu-id="42d00-367">Add configuration parameters and their values in hello following format:</span></span>

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

    <span data-ttu-id="42d00-368">Ez a példa konfigurálja egy Azure Storage társított szolgáltatás és egy Azure SQL társított szolgáltatás connectionString tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="42d00-368">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="42d00-369">Figyelje meg, hogy hello nevét megadó szintaxisa a következő [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="42d00-369">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="42d00-370">Ha JSON, amely rendelkezik egy tömböt, ahogy az a következő kód hello tulajdonsággal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="42d00-370">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

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

    <span data-ttu-id="42d00-371">Ahogy az következő konfigurációs fájl (használata nulla alapú indexelést) hello tulajdonságainak konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="42d00-371">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="42d00-372">Tulajdonságnevek szóközökkel</span><span class="sxs-lookup"><span data-stu-id="42d00-372">Property names with spaces</span></span>
<span data-ttu-id="42d00-373">Ha egy tulajdonság neve szóközöket tartalmaz, használja a kapcsos zárójeleket látható módon a következő példa (adatbázis-kiszolgáló neve) hello:</span><span class="sxs-lookup"><span data-stu-id="42d00-373">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="42d00-374">Megoldás üzembe helyezése konfiguráció használatával</span><span class="sxs-lookup"><span data-stu-id="42d00-374">Deploy solution using a configuration</span></span>
<span data-ttu-id="42d00-375">Ha az Azure Data Factory entitások tesznek közzé a Visual STUDIO, hello konfigurációs toouse használandó közzétételi művelet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="42d00-375">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="42d00-376">a konfigurációs fájl használata az Azure Data Factory-projektek entitások toopublish:</span><span class="sxs-lookup"><span data-stu-id="42d00-376">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="42d00-377">Kattintson a jobb gombbal a Data Factory-projektet, és kattintson a **közzététel** toosee hello **elemek közzététele** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="42d00-377">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="42d00-378">Válassza ki valamelyik adat-előállítót, vagy adjon meg egy adat-előállító létrehozása a hello értékeinek **konfigurálása adat-előállító** lapon, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="42d00-378">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="42d00-379">A hello **elemek közzététele** lap: megjelenik egy legördülő listából válassza ki az elérhető konfigurációk hello **a telepítési konfiguráció kiválasztása** mező.</span><span class="sxs-lookup"><span data-stu-id="42d00-379">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Konfigurációs fájl kiválasztása](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="42d00-381">Jelölje be hello **konfigurációs fájl** , hogy kívánja toouse, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="42d00-381">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="42d00-382">Ellenőrizze, hogy látható-e a hello JSON-fájl neve hello **összegzés** lapot, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="42d00-382">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="42d00-383">Kattintson a **Befejezés** hello központi telepítési művelet befejezése után.</span><span class="sxs-lookup"><span data-stu-id="42d00-383">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="42d00-384">Üzembe helyezésekor hello hello konfigurációs fájlból származó értékek használt tooset értékek hello JSON-fájlok tulajdonságok előtt hello entitások telepített tooAzure Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="42d00-384">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="42d00-385">Az Azure Key Vault használata</span><span class="sxs-lookup"><span data-stu-id="42d00-385">Use Azure Key Vault</span></span>
<span data-ttu-id="42d00-386">Már nem ajánlott, és gyakran biztonsági házirend toocommit érzékeny adatok, például kapcsolati karakterláncok toohello kód tárház ellen.</span><span class="sxs-lookup"><span data-stu-id="42d00-386">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="42d00-387">Lásd: [ADF biztonságos közzétételéhez](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) mintát a Githubon toolearn bizalmas adatok tárolása az Azure Key Vault és használhassa a Data Factory entitások közzététele közben.</span><span class="sxs-lookup"><span data-stu-id="42d00-387">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="42d00-388">hello bővítmény biztonságos közzététele a Visual Studio lehetővé teszi, hogy a hello titkok toobe Key Vault tárolja, és csak a hivatkozások toothem meg van határozva a társított szolgáltatások / szolgáltatástelepítési konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="42d00-388">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="42d00-389">Ha közzéteszi a Data Factory entitások tooAzure feloldása az ezeket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="42d00-389">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="42d00-390">Ezek a fájlok majd lehet véglegesíteni toosource tárház anélkül, hogy a titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="42d00-390">These files can then be committed toosource repository without exposing any secrets.</span></span>


## <a name="next-steps"></a><span data-ttu-id="42d00-391">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42d00-391">Next steps</span></span>
<span data-ttu-id="42d00-392">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="42d00-392">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="42d00-393">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="42d00-393">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="42d00-394">Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.</span><span class="sxs-lookup"><span data-stu-id="42d00-394">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
