---
title: "Oktatóanyag: Azure PowerShell használatával hozzon létre egy folyamat toomove adatok |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy Azure Data Factory-folyamatot másolási tevékenységgel az Azure PowerShell használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="83eba-103">Oktatóanyag: Data Factory-folyamat létrehozása adatok áthelyezéséhez az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="83eba-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="83eba-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83eba-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="83eba-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="83eba-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="83eba-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="83eba-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="83eba-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83eba-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="83eba-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="83eba-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="83eba-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="83eba-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="83eba-110">REST API</span><span class="sxs-lookup"><span data-stu-id="83eba-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="83eba-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="83eba-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="83eba-112">Ebből a cikkből megismerheti, hogyan toouse PowerShell toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="83eba-112">In this article, you learn how toouse PowerShell toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="83eba-113">Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="83eba-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="83eba-114">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="83eba-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="83eba-115">hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="83eba-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="83eba-116">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="83eba-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="83eba-117">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="83eba-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="83eba-118">További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="83eba-119">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="83eba-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="83eba-120">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="83eba-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="83eba-121">További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="83eba-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="83eba-122">Ez a cikk nem foglalkozik minden hello Data Factory-parancsmag.</span><span class="sxs-lookup"><span data-stu-id="83eba-122">This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="83eba-123">A parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory-parancsmagok referenciáját](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="83eba-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="83eba-124">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="83eba-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="83eba-125">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83eba-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83eba-126">Prerequisites</span></span>
- <span data-ttu-id="83eba-127">Végezze el a témakörben ismertetett előfeltételeknek: hello [oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="83eba-127">Complete prerequisites listed in hello [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="83eba-128">Telepítse az **Azure PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="83eba-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="83eba-129">Hello utasításait követve [hogyan tooinstall és konfigurálja az Azure Powershellt](../powershell-install-configure.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-129">Follow hello instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="83eba-130">Lépések</span><span class="sxs-lookup"><span data-stu-id="83eba-130">Steps</span></span>
<span data-ttu-id="83eba-131">Ez az oktatóanyag részeként végrehajtandó hello lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="83eba-131">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="83eba-132">Hozzon létre egy folyamatot az **adat-előállítóban**.</span><span class="sxs-lookup"><span data-stu-id="83eba-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="83eba-133">Ebben a lépésben egy adat-előállítót hoz létre ADFTutorialDataFactoryPSH néven.</span><span class="sxs-lookup"><span data-stu-id="83eba-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="83eba-134">Hozzon létre **összekapcsolt szolgáltatások** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-134">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="83eba-135">Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="83eba-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="83eba-136">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-136">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="83eba-137">Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-137">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="83eba-138">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="83eba-138">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="83eba-139">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="83eba-139">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="83eba-140">Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="83eba-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="83eba-141">Hozzon létre a bemeneti és kimeneti **adatkészletek** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-141">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="83eba-142">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="83eba-142">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="83eba-143">És hello bemeneti blob-adathalmazra hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-143">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="83eba-144">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-144">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="83eba-145">És hello kimeneti SQL táblázat dataset megadja hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.</span><span class="sxs-lookup"><span data-stu-id="83eba-145">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
4. <span data-ttu-id="83eba-146">Hozzon létre egy **csővezeték** hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-146">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="83eba-147">Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.</span><span class="sxs-lookup"><span data-stu-id="83eba-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="83eba-148">hello másolási tevékenység hello Azure blob storage tooa tábla hello Azure SQL-adatbázis egy blobot másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="83eba-148">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="83eba-149">A másolási tevékenység használható egy folyamat toocopy adatokat bármely támogatott forrás támogatott tooany cél.</span><span class="sxs-lookup"><span data-stu-id="83eba-149">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="83eba-150">A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="83eba-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="83eba-151">A figyelő hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="83eba-151">Monitor hello pipeline.</span></span> <span data-ttu-id="83eba-152">Ebben a lépésben meg **figyelő** hello szeletek bemeneti és kimeneti adatkészletek a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="83eba-152">In this step, you **monitor** hello slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="83eba-153">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="83eba-154">Teljes [hello oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Ha még nem tette meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-154">Complete [prerequisites for hello tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="83eba-155">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="83eba-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="83eba-156">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="83eba-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="83eba-157">Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="83eba-157">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="83eba-158">Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="83eba-158">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="83eba-159">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="83eba-159">Launch **PowerShell**.</span></span> <span data-ttu-id="83eba-160">Azure PowerShell nyitva hagyja az oktatóanyag hello végéig.</span><span class="sxs-lookup"><span data-stu-id="83eba-160">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="83eba-161">Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="83eba-161">If you close and reopen, you need toorun hello commands again.</span></span>

    <span data-ttu-id="83eba-162">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="83eba-162">Run hello following command, and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="83eba-163">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="83eba-163">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="83eba-164">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="83eba-164">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="83eba-165">Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéséhez:</span><span class="sxs-lookup"><span data-stu-id="83eba-165">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="83eba-166">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="83eba-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="83eba-167">Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot használni **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="83eba-167">Some of hello steps in this tutorial assume that you use hello resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="83eba-168">Ha egy másik erőforráscsoportban található használatához szüksége van-e toouse helyett, ebben az oktatóanyagban ADFTutorialResourceGroup azt.</span><span class="sxs-lookup"><span data-stu-id="83eba-168">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="83eba-169">Futtassa a hello **New-AzureRmDataFactory** parancsmag toocreate nevű adat-előállító **ADFTutorialDataFactoryPSH**:</span><span class="sxs-lookup"><span data-stu-id="83eba-169">Run hello **New-AzureRmDataFactory** cmdlet toocreate a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="83eba-170">Előfordulhat, hogy ez a név már foglalt.</span><span class="sxs-lookup"><span data-stu-id="83eba-170">This name may already have been taken.</span></span> <span data-ttu-id="83eba-171">Ezért egyedivé hello hello adat-előállító nevét egy előtag vagy utótag hozzáadásával (például: ADFTutorialDataFactoryPSH05152017), és futtassa újra a hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="83eba-171">Therefore, make hello name of hello data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run hello command again.</span></span>  

<span data-ttu-id="83eba-172">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="83eba-172">Note hello following points:</span></span>

* <span data-ttu-id="83eba-173">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="83eba-173">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="83eba-174">Ha hiba történt a következő hello, módosítsa a hello nevét (például yournameADFTutorialDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="83eba-174">If you receive hello following error, change hello name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="83eba-175">Használja ezt az ADFTutorialFactoryPSH helyett az oktatóanyag lépéseinek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="83eba-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="83eba-176">A Data Factory-összetevők részleteit a [Data Factory elnevezési szabályait](data-factory-naming-rules.md) ismertető témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="83eba-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="83eba-177">toocreate adat-előállító példányok, közreműködő vagy hello Azure-előfizetés rendszergazdájának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="83eba-177">toocreate Data Factory instances, you must be a contributor or administrator of hello Azure subscription.</span></span>
* <span data-ttu-id="83eba-178">hello adat-előállító nevét hello előfordulhat, hogy a jövőbeni hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.</span><span class="sxs-lookup"><span data-stu-id="83eba-178">hello name of hello data factory may be registered as a DNS name in hello future, and hence become publicly visible.</span></span>
* <span data-ttu-id="83eba-179">Hello a következő hiba jelenhet meg: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory.**"</span><span class="sxs-lookup"><span data-stu-id="83eba-179">You may receive hello following error: "**This subscription is not registered toouse namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="83eba-180">Hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="83eba-180">Do one of hello following, and try publishing again:</span></span>

  * <span data-ttu-id="83eba-181">Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:</span><span class="sxs-lookup"><span data-stu-id="83eba-181">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="83eba-182">Futtassa a következő parancs tooconfirm hello adott hello Data Factory szolgáltató regisztrálva van:</span><span class="sxs-lookup"><span data-stu-id="83eba-182">Run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="83eba-183">Jelentkezzen be a hello Azure-előfizetés toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="83eba-183">Sign in by using hello Azure subscription toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="83eba-184">Nyissa meg tooa adat-előállító panelen, vagy hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="83eba-184">Go tooa Data Factory blade, or create a data factory in hello Azure portal.</span></span> <span data-ttu-id="83eba-185">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="83eba-185">This action automatically registers hello provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="83eba-186">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-186">Create linked services</span></span>
<span data-ttu-id="83eba-187">A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="83eba-187">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="83eba-188">Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics).</span><span class="sxs-lookup"><span data-stu-id="83eba-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="83eba-189">Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél).</span><span class="sxs-lookup"><span data-stu-id="83eba-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="83eba-190">Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).</span><span class="sxs-lookup"><span data-stu-id="83eba-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="83eba-191">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-191">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="83eba-192">Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-192">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="83eba-193">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="83eba-193">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="83eba-194">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="83eba-194">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="83eba-195">Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-195">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="83eba-196">Társított szolgáltatás létrehozása Azure Storage-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="83eba-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="83eba-197">Ebben a lépésben a az Azure storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="83eba-197">In this step, you link your Azure storage account tooyour data factory.</span></span>

1. <span data-ttu-id="83eba-198">Hozzon létre egy JSON fájlt **AzureStorageLinkedService.json** a **C:\ADFGetStartedPSH** hello tartalom a következő mappában: (hello mappa létrehozása ADFGetStartedPSH, ha még nem létezik.)</span><span class="sxs-lookup"><span data-stu-id="83eba-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with hello following content: (Create hello folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="83eba-199">Cserélje le &lt;accountname&gt; és &lt;accountkey&gt; nevű és hello fájl mentése előtt az Azure storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="83eba-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving hello file.</span></span> 

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
     }
    ``` 
2. <span data-ttu-id="83eba-200">A **Azure PowerShell**, toohello kapcsoló **ADFGetStartedPSH** mappa.</span><span class="sxs-lookup"><span data-stu-id="83eba-200">In **Azure PowerShell**, switch toohello **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="83eba-201">Futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag toocreate hello társított szolgáltatás: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="83eba-201">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet toocreate hello linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="83eba-202">Ez a parancsmag és egyéb adat-előállító parancsmagok ebben az oktatóanyagban használata akkor toopass paraméterértékek szükségesek hello **ResourceGroupName** és **DataFactoryName** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="83eba-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="83eba-203">Másik lehetőségként át ResourceGroupName és DataFactoryName parancsmag minden futtatásakor beírása nélkül hello New-AzureRmDataFactory parancsmag által visszaadott hello DataFactory objektum.</span><span class="sxs-lookup"><span data-stu-id="83eba-203">Alternatively, you can pass hello DataFactory object returned by hello New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="83eba-204">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-204">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="83eba-205">Más szolgáltatásnak létrehozásának módja a toospecify erőforráscsoport-név és az adat-előállító hello DataFactory objektum megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="83eba-205">Other way of creating this linked service is toospecify resource group name and data factory name instead of specifying hello DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="83eba-206">Társított szolgáltatás létrehozása Azure SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="83eba-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="83eba-207">Ebben a lépésben az Azure SQL adatbázis tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="83eba-207">In this step, you link your Azure SQL database tooyour data factory.</span></span>

1. <span data-ttu-id="83eba-208">A következő hello C:\ADFGetStartedPSH mappában AzureSqlLinkedService.json nevű JSON-fájl tartalmának létrehozása:</span><span class="sxs-lookup"><span data-stu-id="83eba-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with hello following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="83eba-209">A &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; és &lt;password&gt; paraméterek értékét cserélje le az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a jelszóra.</span><span class="sxs-lookup"><span data-stu-id="83eba-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. <span data-ttu-id="83eba-210">Futtassa a következő parancs toocreate hello összekapcsolt szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="83eba-210">Run hello following command toocreate a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="83eba-211">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-211">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="83eba-212">Ellenőrizze, hogy **hozzáférést tooAzure szolgáltatások** beállítás engedélyezve van az SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="83eba-212">Confirm that **Allow access tooAzure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="83eba-213">tooverify, és kapcsolja be, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="83eba-213">tooverify and turn it on, do hello following steps:</span></span>

    1. <span data-ttu-id="83eba-214">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="83eba-214">Log in toohello [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="83eba-215">Kattintson a **további szolgáltatások >** hello balra, majd kattintson a **SQL Server-kiszolgálók** a hello **ADATBÁZISOK** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="83eba-215">Click **More services >** on hello left, and click **SQL servers** in hello **DATABASES** category.</span></span>
    3. <span data-ttu-id="83eba-216">Az SQL-kiszolgálók hello listában jelölje ki a kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="83eba-216">Select your server in hello list of SQL servers.</span></span>
    4. <span data-ttu-id="83eba-217">Hello SQL server paneljén kattintson **tűzfal beállításainak megjelenítése** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="83eba-217">On hello SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="83eba-218">A hello **tűzfalbeállítások** panelen kattintson a **ON** a **hozzáférést tooAzure szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="83eba-218">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
    6. <span data-ttu-id="83eba-219">Kattintson a **mentése** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="83eba-219">Click **Save** on hello toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="83eba-220">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-220">Create datasets</span></span>
<span data-ttu-id="83eba-221">Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-221">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="83eba-222">Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="83eba-223">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="83eba-223">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="83eba-224">És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-224">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="83eba-225">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-225">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="83eba-226">És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="83eba-226">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="83eba-227">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-227">Create an input dataset</span></span>
<span data-ttu-id="83eba-228">Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello.</span><span class="sxs-lookup"><span data-stu-id="83eba-228">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="83eba-229">Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="83eba-229">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="83eba-230">Ebben az oktatóanyagban hello fájlnév értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="83eba-230">In this tutorial, you specify a value for hello fileName.</span></span>  

1. <span data-ttu-id="83eba-231">Hozzon létre egy JSON fájlt **InputDataset.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappába:</span><span class="sxs-lookup"><span data-stu-id="83eba-231">Create a JSON file named **InputDataset.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

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
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "emp.txt",
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

    <span data-ttu-id="83eba-232">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="83eba-232">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="83eba-233">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="83eba-233">Property</span></span> | <span data-ttu-id="83eba-234">Leírás</span><span class="sxs-lookup"><span data-stu-id="83eba-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="83eba-235">type</span><span class="sxs-lookup"><span data-stu-id="83eba-235">type</span></span> | <span data-ttu-id="83eba-236">hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="83eba-236">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="83eba-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="83eba-237">linkedServiceName</span></span> | <span data-ttu-id="83eba-238">Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="83eba-238">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="83eba-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="83eba-239">folderPath</span></span> | <span data-ttu-id="83eba-240">Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB.</span><span class="sxs-lookup"><span data-stu-id="83eba-240">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="83eba-241">Ebben az oktatóanyagban adftutorial hello blobtárolót és hello legfelső szintű mappa.</span><span class="sxs-lookup"><span data-stu-id="83eba-241">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="83eba-242">fileName</span><span class="sxs-lookup"><span data-stu-id="83eba-242">fileName</span></span> | <span data-ttu-id="83eba-243">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="83eba-243">This property is optional.</span></span> <span data-ttu-id="83eba-244">Ha kihagyja ezt a tulajdonságot, leltárhoz hello folderPath lévő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="83eba-244">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="83eba-245">Ebben az oktatóanyagban **emp.txt** hello fájlnév, hogy csak adott fájl van felvételre feldolgozásra számára megadott.</span><span class="sxs-lookup"><span data-stu-id="83eba-245">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="83eba-246">formátum -> típus</span><span class="sxs-lookup"><span data-stu-id="83eba-246">format -> type</span></span> |<span data-ttu-id="83eba-247">hello bemeneti fájl hello szöveg formátumban van, így használjuk **szöveges**.</span><span class="sxs-lookup"><span data-stu-id="83eba-247">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="83eba-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="83eba-248">columnDelimiter</span></span> | <span data-ttu-id="83eba-249">hello bemeneti fájl hello oszlopai határolja **vesszővel karakter (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="83eba-249">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="83eba-250">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="83eba-250">frequency/interval</span></span> | <span data-ttu-id="83eba-251">hello gyakoriságának beállítása túl**óra** és időköz értéke túl**1**, ami azt jelenti, hogy hello bemeneti szeletek érhetők el **óránkénti**.</span><span class="sxs-lookup"><span data-stu-id="83eba-251">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="83eba-252">Más szóval hello Data Factory szolgáltatásnak megkeresi a bemeneti adatok óránként blob tároló hello gyökérmappájában (**adftutorial**) megadott.</span><span class="sxs-lookup"><span data-stu-id="83eba-252">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="83eba-253">Hello adatainak hello folyamat kezdési és befejezési időpontokat, nem előtt vagy után ezekben az időszakokban keresi.</span><span class="sxs-lookup"><span data-stu-id="83eba-253">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="83eba-254">external</span><span class="sxs-lookup"><span data-stu-id="83eba-254">external</span></span> | <span data-ttu-id="83eba-255">Ez a tulajdonság értéke túl**igaz** hello adatok nem jön létre, ez az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="83eba-255">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="83eba-256">Ebben az oktatóanyagban hello a bemeneti adatok nem jön létre a folyamat, így ez a tulajdonság tootrue hivatott hello emp.txt fájl van.</span><span class="sxs-lookup"><span data-stu-id="83eba-256">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="83eba-257">Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.</span><span class="sxs-lookup"><span data-stu-id="83eba-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="83eba-258">Futtassa a következő parancs toocreate hello adat-előállító dataset hello.</span><span class="sxs-lookup"><span data-stu-id="83eba-258">Run hello following command toocreate hello Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="83eba-259">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-259">Here is hello sample output:</span></span>

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a><span data-ttu-id="83eba-260">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-260">Create an output dataset</span></span>
<span data-ttu-id="83eba-261">Ez a kijelző hello lépés hoz létre egy kimeneti adatkészlet nevű **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="83eba-261">In this part of hello step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="83eba-262">Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="83eba-262">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="83eba-263">Hozzon létre egy JSON fájlt **OutputDataset.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappában:</span><span class="sxs-lookup"><span data-stu-id="83eba-263">Create a JSON file named **OutputDataset.json** in hello **C:\ADFGetStartedPSH** folder with hello following content:</span></span>

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
            "linkedServiceName": "AzureSqlLinkedService",
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

    <span data-ttu-id="83eba-264">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="83eba-264">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="83eba-265">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="83eba-265">Property</span></span> | <span data-ttu-id="83eba-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="83eba-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="83eba-267">type</span><span class="sxs-lookup"><span data-stu-id="83eba-267">type</span></span> | <span data-ttu-id="83eba-268">hello type tulajdonság beállítása túl**AzureSqlTable** mert adatok másolt tooa tábla Azure SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="83eba-268">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="83eba-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="83eba-269">linkedServiceName</span></span> | <span data-ttu-id="83eba-270">Toohello hivatkozik **AzureSqlLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="83eba-270">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="83eba-271">tableName</span><span class="sxs-lookup"><span data-stu-id="83eba-271">tableName</span></span> | <span data-ttu-id="83eba-272">Megadott hello **tábla** toowhich hello adatok másolását.</span><span class="sxs-lookup"><span data-stu-id="83eba-272">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="83eba-273">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="83eba-273">frequency/interval</span></span> | <span data-ttu-id="83eba-274">hello gyakoriságának beállítása túl**óra** és időköz **1**, ami azt jelenti, hogy hello kimeneti szeletek előállítása **óránkénti** közötti hello folyamat kezdési és befejezési időpontokat, előtte vagy Miután ezekben az időszakokban.</span><span class="sxs-lookup"><span data-stu-id="83eba-274">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="83eba-275">Három oszlop – **azonosító**, **Keresztnév**, és **Vezetéknév** – hello üres tábla hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="83eba-275">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="83eba-276">Azonosító: azonosító oszlopot, ezért meg kell, hogy csak toospecify **Keresztnév** és **Vezetéknév** itt.</span><span class="sxs-lookup"><span data-stu-id="83eba-276">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="83eba-277">További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="83eba-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="83eba-278">Futtassa a következő parancs toocreate hello data factory dataset hello.</span><span class="sxs-lookup"><span data-stu-id="83eba-278">Run hello following command toocreate hello data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="83eba-279">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-279">Here is hello sample output:</span></span>

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a><span data-ttu-id="83eba-280">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="83eba-280">Create a pipeline</span></span>
<span data-ttu-id="83eba-281">Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="83eba-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="83eba-282">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést.</span><span class="sxs-lookup"><span data-stu-id="83eba-282">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="83eba-283">Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet.</span><span class="sxs-lookup"><span data-stu-id="83eba-283">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="83eba-284">hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra.</span><span class="sxs-lookup"><span data-stu-id="83eba-284">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="83eba-285">Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="83eba-285">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 


1. <span data-ttu-id="83eba-286">Hozzon létre egy JSON fájlt **ADFTutorialPipeline.json** a hello **C:\ADFGetStartedPSH** hello tartalom a következő mappába:</span><span class="sxs-lookup"><span data-stu-id="83eba-286">Create a JSON file named **ADFTutorialPipeline.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

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
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```
    <span data-ttu-id="83eba-287">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="83eba-287">Note hello following points:</span></span>
   
    - <span data-ttu-id="83eba-288">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="83eba-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="83eba-289">Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-289">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="83eba-290">A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.</span><span class="sxs-lookup"><span data-stu-id="83eba-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="83eba-291">Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="83eba-291">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="83eba-292">A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.</span><span class="sxs-lookup"><span data-stu-id="83eba-292">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="83eba-293">Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="83eba-293">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="83eba-294">Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.</span><span class="sxs-lookup"><span data-stu-id="83eba-294">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="83eba-295">Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket.</span><span class="sxs-lookup"><span data-stu-id="83eba-295">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="83eba-296">Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja.</span><span class="sxs-lookup"><span data-stu-id="83eba-296">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="83eba-297">Például "2016-02-03", amely túl egyenértékű "2016-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="83eba-297">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="83eba-298">Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni.</span><span class="sxs-lookup"><span data-stu-id="83eba-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="83eba-299">Például: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="83eba-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="83eba-300">Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk.</span><span class="sxs-lookup"><span data-stu-id="83eba-300">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="83eba-301">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**".</span><span class="sxs-lookup"><span data-stu-id="83eba-301">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="83eba-302">toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="83eba-302">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="83eba-303">A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.</span><span class="sxs-lookup"><span data-stu-id="83eba-303">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="83eba-304">A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="83eba-305">A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="83eba-306">A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="83eba-307">Az SqlSink által támogatott JSON-tulajdonságok leírásáért lásd: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="83eba-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="83eba-308">Futtassa a következő parancs toocreate hello data factory-tábla hello.</span><span class="sxs-lookup"><span data-stu-id="83eba-308">Run hello following command toocreate hello data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="83eba-309">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-309">Here is hello sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="83eba-310">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="83eba-310">**Congratulations!**</span></span> <span data-ttu-id="83eba-311">Sikeresen létrehozott egy az Azure data factory egy folyamat toocopy Azure blob storage tooan Azure SQL-adatbázis adatait.</span><span class="sxs-lookup"><span data-stu-id="83eba-311">You have successfully created an Azure data factory with a pipeline toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

## <a name="monitor-hello-pipeline"></a><span data-ttu-id="83eba-312">A figyelő hello folyamat</span><span class="sxs-lookup"><span data-stu-id="83eba-312">Monitor hello pipeline</span></span>
<span data-ttu-id="83eba-313">Ezt a lépést használhatja az Azure PowerShell toomonitor egy az Azure data factory lesz.</span><span class="sxs-lookup"><span data-stu-id="83eba-313">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="83eba-314">Cserélje le &lt;DataFactoryName&gt; az adat-előállító és a futtató hello nevű **Get-AzureRmDataFactory**, és rendelje hozzá a hello kimeneti tooa változó $df.</span><span class="sxs-lookup"><span data-stu-id="83eba-314">Replace &lt;DataFactoryName&gt; with hello name of your data factory and run **Get-AzureRmDataFactory**, and assign hello output tooa variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="83eba-315">Példa:</span><span class="sxs-lookup"><span data-stu-id="83eba-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="83eba-316">Ezután futtassa a következő kimeneti $df toosee hello nyomtatási hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="83eba-316">Then, run print hello contents of $df toosee hello following output:</span></span> 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. <span data-ttu-id="83eba-317">Futtatás **Get-AzureRmDataFactorySlice** hello az összes szeletek tooget adatait **OutputDataset**, vagyis hello kimeneti adatkészlet hello folyamatának.</span><span class="sxs-lookup"><span data-stu-id="83eba-317">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **OutputDataset**, which is hello output dataset of hello pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="83eba-318">Ez a beállítás meg kell felelnie a hello **Start** hello adatcsatorna JSON értéket.</span><span class="sxs-lookup"><span data-stu-id="83eba-318">This setting should match hello **Start** value in hello pipeline JSON.</span></span> <span data-ttu-id="83eba-319">24 szeletek, megjelenik egy a minden órában az aktuális nap too12 hello éjféltől hello a VAGYOK másnap.</span><span class="sxs-lookup"><span data-stu-id="83eba-319">You should see 24 slices, one for each hour from 12 AM of hello current day too12 AM of hello next day.</span></span>

   <span data-ttu-id="83eba-320">Az alábbiakban a három minta szeletek hello kimenetből:</span><span class="sxs-lookup"><span data-stu-id="83eba-320">Here are three sample slices from hello output:</span></span> 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="83eba-321">Futtatás **Get-AzureRmDataFactoryRun** tooget hello részletek tevékenység fut egy **adott** szelet.</span><span class="sxs-lookup"><span data-stu-id="83eba-321">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="83eba-322">Hello dátum-idő érték másolása hello kimenete hello előző parancs toospecify hello hello StartDateTime paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="83eba-322">Copy hello date-time value from hello output of hello previous command toospecify hello value for hello StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="83eba-323">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="83eba-323">Here is hello sample output:</span></span> 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

<span data-ttu-id="83eba-324">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációt a [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) (Data Factory-parancsmagok referenciája) című cikk tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="83eba-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="83eba-325">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="83eba-325">Summary</span></span>
<span data-ttu-id="83eba-326">Ebben az oktatóanyagban létrehozott egy Azure data factory toocopy adatok Azure blob tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="83eba-326">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="83eba-327">PowerShell toocreate hello adat-előállító, a társított szolgáltatások, a adatkészletek és a folyamat használta.</span><span class="sxs-lookup"><span data-stu-id="83eba-327">You used PowerShell toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="83eba-328">Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="83eba-328">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="83eba-329">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="83eba-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="83eba-330">**Társított szolgáltatásokat** hozott létre:</span><span class="sxs-lookup"><span data-stu-id="83eba-330">Created **linked services**:</span></span>

   <span data-ttu-id="83eba-331">a.</span><span class="sxs-lookup"><span data-stu-id="83eba-331">a.</span></span> <span data-ttu-id="83eba-332">Egy **Azure Storage** kapcsolódó szolgáltatás toolink az Azure storage-fiók, amely a bemeneti adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="83eba-332">An **Azure Storage** linked service toolink your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="83eba-333">b.</span><span class="sxs-lookup"><span data-stu-id="83eba-333">b.</span></span> <span data-ttu-id="83eba-334">Egy **Azure SQL** kapcsolódó szolgáltatás toolink az SQL-adatbázis, amely tárolja a hello kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="83eba-334">An **Azure SQL** linked service toolink your SQL database that holds hello output data.</span></span>
3. <span data-ttu-id="83eba-335">**Adatkészleteket** hozott létre, amelyek a folyamat bemeneti és kimeneti adatait írják le.</span><span class="sxs-lookup"><span data-stu-id="83eba-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="83eba-336">Létrehozott egy **csővezeték** a **másolási tevékenység**, a **BlobSource** hello forrásként és **SqlSink** , hello fogadó.</span><span class="sxs-lookup"><span data-stu-id="83eba-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as hello source and **SqlSink** as hello sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83eba-337">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83eba-337">Next steps</span></span>
<span data-ttu-id="83eba-338">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="83eba-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="83eba-339">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="83eba-339">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="83eba-340">Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.</span><span class="sxs-lookup"><span data-stu-id="83eba-340">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span> 

