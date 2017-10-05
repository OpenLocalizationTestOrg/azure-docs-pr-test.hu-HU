---
title: "Oktatóanyag: Folyamat létrehozása adatok áthelyezéséhez az Azure PowerShell használatával | Microsoft Docs"
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
ms.openlocfilehash: 81efe7c6af29af778686e1f6bcf62fedc9711052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="c1636-103">Oktatóanyag: Data Factory-folyamat létrehozása adatok áthelyezéséhez az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c1636-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c1636-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1636-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="c1636-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="c1636-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="c1636-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c1636-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="c1636-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1636-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="c1636-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c1636-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="c1636-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="c1636-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="c1636-110">REST API</span><span class="sxs-lookup"><span data-stu-id="c1636-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="c1636-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="c1636-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="c1636-112">A cikk útmutatást nyújt adat-előállítók PowerShell használatával való létrehozására olyan folyamatokkal, amelyek az Azure Blob Storage-ból másolnak adatokat az Azure SQL Database-be.</span><span class="sxs-lookup"><span data-stu-id="c1636-112">In this article, you learn how to use PowerShell to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="c1636-113">Ha még csak ismerkedik az Azure Data Factory szolgáltatással, olvassa el a [Bevezetés az Azure Data Factory használatába](data-factory-introduction.md) című cikket az oktatóanyag elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="c1636-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="c1636-114">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="c1636-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="c1636-115">A másolási tevékenység adatokat másol a forrásadattárból egy támogatott fogadó adattárba.</span><span class="sxs-lookup"><span data-stu-id="c1636-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="c1636-116">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c1636-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="c1636-117">A tevékenységet egy globálisan elérhető szolgáltatás működteti, amely biztonságos, megbízható és skálázható módon másolja az adatokat a különböző adattárak között.</span><span class="sxs-lookup"><span data-stu-id="c1636-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="c1636-118">További információ a másolási tevékenységről: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="c1636-119">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="c1636-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="c1636-120">Ezenkívül össze is fűzhet két tevékenységet (egymás után futtathatja őket), ha az egyik tevékenység kimeneti adatkészletét a másik tevékenység bemeneti adatkészleteként állítja be.</span><span class="sxs-lookup"><span data-stu-id="c1636-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="c1636-121">További információ az [egy folyamaton belüli több tevékenységről](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) szóló témakörben található.</span><span class="sxs-lookup"><span data-stu-id="c1636-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="c1636-122">Ez a cikk nem tárgyalja az összes Data Factory-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="c1636-122">This article does not cover all the Data Factory cmdlets.</span></span> <span data-ttu-id="c1636-123">A parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory-parancsmagok referenciáját](/powershell/module/azurerm.datafactories).</span><span class="sxs-lookup"><span data-stu-id="c1636-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="c1636-124">Az oktatóanyagban található adatfeldolgozási folyamat adatokat másol egy forrásadattárból egy céladattárba.</span><span class="sxs-lookup"><span data-stu-id="c1636-124">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="c1636-125">Az adatok Azure Data Factory használatával történő átalakításának útmutatásáért olvassa el [az adatok Hadoop-fürt segítségével történő átalakítására szolgáló folyamat létrehozását ismertető oktatóanyagot](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-125">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1636-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1636-126">Prerequisites</span></span>
- <span data-ttu-id="c1636-127">Hajtsa végre [Az oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) című cikkben előfeltételként felsorolt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c1636-127">Complete prerequisites listed in the [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="c1636-128">Telepítse az **Azure PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="c1636-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="c1636-129">Kövesse [az Azure PowerShell telepítését és konfigurálását](../powershell-install-configure.md) ismertető cikkben szereplő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="c1636-129">Follow the instructions in [How to install and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="c1636-130">Lépések</span><span class="sxs-lookup"><span data-stu-id="c1636-130">Steps</span></span>
<span data-ttu-id="c1636-131">Az oktatóanyag során a következő lépéseket fogja elvégezni:</span><span class="sxs-lookup"><span data-stu-id="c1636-131">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="c1636-132">Azure **adat-előállító** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c1636-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="c1636-133">Ebben a lépésben egy adat-előállítót hoz létre ADFTutorialDataFactoryPSH néven.</span><span class="sxs-lookup"><span data-stu-id="c1636-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="c1636-134">Hozzon létre **társított szolgáltatásokat** az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c1636-134">Create **linked services** in the data factory.</span></span> <span data-ttu-id="c1636-135">Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c1636-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="c1636-136">Az AzureStorageLinkedService az Azure Storage-fiókot társítja az adat-előállítóval.</span><span class="sxs-lookup"><span data-stu-id="c1636-136">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="c1636-137">Létrehozott egy tárolót, és adatokat töltött fel ebbe a tárfiókba az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként.</span><span class="sxs-lookup"><span data-stu-id="c1636-137">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="c1636-138">Az AzureSqlLinkedService az Azure SQL Database-t társítja az adat-előállítóval.</span><span class="sxs-lookup"><span data-stu-id="c1636-138">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="c1636-139">A blobtárolóból másolt adatokat a rendszer ebben az adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-139">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="c1636-140">Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c1636-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="c1636-141">Hozza létre a bemeneti és kimeneti **adatkészleteket** az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c1636-141">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="c1636-142">Az Azure Storage társított szolgáltatása határozza meg azt a kapcsolati sztringet, amelyet futtatáskor a Data Factory szolgáltatás az Azure Storage-fiók csatlakoztatásához használ.</span><span class="sxs-lookup"><span data-stu-id="c1636-142">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="c1636-143">A bemeneti blob adatkészlete pedig a tárolót és a bemeneti adatokat tartalmazó mappát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-143">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="c1636-144">Ehhez hasonlóan az Azure SQL Database társított szolgáltatása határozza meg azt a kapcsolati sztringet, amelyet futtatáskor a Data Factory szolgáltatás az Azure SQL Database csatlakoztatásához használ.</span><span class="sxs-lookup"><span data-stu-id="c1636-144">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="c1636-145">Az SQL-tábla kimeneti adatkészlete határozza meg azt az adatbázistáblát, amelybe a rendszer a blobtárolóból származó adatokat másolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-145">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
4. <span data-ttu-id="c1636-146">Hozzon létre egy **folyamatot** az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c1636-146">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="c1636-147">Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c1636-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="c1636-148">A másolási tevékenység adatokat másol az Azure Blob Storage-ból az Azure SQL Database egyik táblájába.</span><span class="sxs-lookup"><span data-stu-id="c1636-148">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="c1636-149">A folyamat másolási tevékenységével adatokat másolhat bármely támogatott forrásból bármely támogatott célhelyre.</span><span class="sxs-lookup"><span data-stu-id="c1636-149">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="c1636-150">A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c1636-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="c1636-151">A folyamat figyelése.</span><span class="sxs-lookup"><span data-stu-id="c1636-151">Monitor the pipeline.</span></span> <span data-ttu-id="c1636-152">Ebben a lépésben a bemeneti és a kimeneti adatkészletek szeleteit **figyeli** PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c1636-152">In this step, you **monitor** the slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="c1636-153">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c1636-154">Hajtsa végre az [oktatóanyag előfeltételeinek lépéseit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), ha még nem tette volna meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-154">Complete [prerequisites for the tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="c1636-155">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="c1636-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="c1636-156">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="c1636-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="c1636-157">Például egy olyan másolási tevékenység, amely adatokat másol egy forrásadattárból egy céladattárba, és egy HDInsight Hive-tevékenység, amely egy Hive-szkript futtatásával alakítja át a bemeneti adatokat kimeneti adatokká.</span><span class="sxs-lookup"><span data-stu-id="c1636-157">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="c1636-158">Ebben a lépésben létrehozzuk a data factoryt.</span><span class="sxs-lookup"><span data-stu-id="c1636-158">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="c1636-159">Indítsa el a **PowerShellt**.</span><span class="sxs-lookup"><span data-stu-id="c1636-159">Launch **PowerShell**.</span></span> <span data-ttu-id="c1636-160">Az Azure PowerShellt hagyja megnyitva az oktatóanyag végéig.</span><span class="sxs-lookup"><span data-stu-id="c1636-160">Keep Azure PowerShell open until the end of this tutorial.</span></span> <span data-ttu-id="c1636-161">Ha bezárja és újra megnyitja, akkor újra futtatnia kell a parancsokat.</span><span class="sxs-lookup"><span data-stu-id="c1636-161">If you close and reopen, you need to run the commands again.</span></span>

    <span data-ttu-id="c1636-162">Futtassa a következő parancsot, és adja meg az Azure Portalra való bejelentkezéshez használt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c1636-162">Run the following command, and enter the user name and password that you use to sign in to the Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="c1636-163">Futtassa a következő parancsot a fiókhoz tartozó előfizetések megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="c1636-163">Run the following command to view all the subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="c1636-164">Futtassa a következő parancsot a használni kívánt előfizetés kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="c1636-164">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="c1636-165">Cserélje a **&lt;NameOfAzureSubscription**&gt; kifejezést az Azure-előfizetése nevére.</span><span class="sxs-lookup"><span data-stu-id="c1636-165">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="c1636-166">Hozzon létre egy Azure-erőforráscsoportot **ADFTutorialResourceGroup** néven a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c1636-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="c1636-167">Az oktatóanyag különböző lépései során feltételezzük, hogy az **ADFTutorialResourceGroup** elnevezésű erőforráscsoportot használja.</span><span class="sxs-lookup"><span data-stu-id="c1636-167">Some of the steps in this tutorial assume that you use the resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="c1636-168">Ha másik erőforráscsoportot használ, akkor az oktatóanyagban azt használja az ADFTutorialResourceGroup helyett.</span><span class="sxs-lookup"><span data-stu-id="c1636-168">If you use a different resource group, you need to use it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="c1636-169">Futtassa a **New-AzureRmDataFactory** parancsmagot, és hozzon létre egy új data factoryt **ADFTutorialDataFactoryPSH** néven.</span><span class="sxs-lookup"><span data-stu-id="c1636-169">Run the **New-AzureRmDataFactory** cmdlet to create a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="c1636-170">Előfordulhat, hogy ez a név már foglalt.</span><span class="sxs-lookup"><span data-stu-id="c1636-170">This name may already have been taken.</span></span> <span data-ttu-id="c1636-171">Ezt elkerülendő tegye egyedivé az adat-előállító nevét egy elő- vagy utótag hozzáfűzésével (például: ADFTutorialDataFactoryPSH05152017), majd futtassa újra a parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1636-171">Therefore, make the name of the data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run the command again.</span></span>  

<span data-ttu-id="c1636-172">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="c1636-172">Note the following points:</span></span>

* <span data-ttu-id="c1636-173">Az Azure data factory nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c1636-173">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="c1636-174">Ha a következő hibaüzenetet kapja, módosítsa a nevet (például sajátnévADFTutorialDataFactoryPSH-ra).</span><span class="sxs-lookup"><span data-stu-id="c1636-174">If you receive the following error, change the name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="c1636-175">Használja ezt az ADFTutorialFactoryPSH helyett az oktatóanyag lépéseinek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="c1636-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="c1636-176">A Data Factory-összetevők részleteit a [Data Factory elnevezési szabályait](data-factory-naming-rules.md) ismertető témakörben találja.</span><span class="sxs-lookup"><span data-stu-id="c1636-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="c1636-177">Data Factory-példányok létrehozásához az Azure-előfizetés közreműködőjének vagy rendszergazdájának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c1636-177">To create Data Factory instances, you must be a contributor or administrator of the Azure subscription.</span></span>
* <span data-ttu-id="c1636-178">Az adat-előállító neve később DNS-névként regisztrálható, így nyilvánosan láthatóvá tehető.</span><span class="sxs-lookup"><span data-stu-id="c1636-178">The name of the data factory may be registered as a DNS name in the future, and hence become publicly visible.</span></span>
* <span data-ttu-id="c1636-179">A következő hibaüzenet jelenhet meg: „**This subscription is not registered to use namespace Microsoft.DataFactory**” (Az előfizetés nem jogosult használni a Microsoft.DataFactory névteret).</span><span class="sxs-lookup"><span data-stu-id="c1636-179">You may receive the following error: "**This subscription is not registered to use namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="c1636-180">Tegye a következők egyikét, és próbálkozzon újra a közzététellel:</span><span class="sxs-lookup"><span data-stu-id="c1636-180">Do one of the following, and try publishing again:</span></span>

  * <span data-ttu-id="c1636-181">Az Azure PowerShellben futtassa az alábbi parancsot a Data Factory-szolgáltató regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="c1636-181">In Azure PowerShell, run the following command to register the Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="c1636-182">Az alábbi parancs futtatásával ellenőrizheti, hogy a Data Factory-szolgáltató regisztrálva van-e.</span><span class="sxs-lookup"><span data-stu-id="c1636-182">Run the following command to confirm that the Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="c1636-183">Az Azure-előfizetés használatával jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c1636-183">Sign in by using the Azure subscription to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c1636-184">Navigáljon egy Data Factory-panelre, vagy hozzon létre egy data factoryt az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="c1636-184">Go to a Data Factory blade, or create a data factory in the Azure portal.</span></span> <span data-ttu-id="c1636-185">Ezzel a művelettel automatikusan regisztrálja a szolgáltatót.</span><span class="sxs-lookup"><span data-stu-id="c1636-185">This action automatically registers the provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="c1636-186">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-186">Create linked services</span></span>
<span data-ttu-id="c1636-187">Társított szolgáltatásokat hoz létre egy adat-előállítóban az adattárak és a számítási szolgáltatások adat-előállítóval történő társításához.</span><span class="sxs-lookup"><span data-stu-id="c1636-187">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="c1636-188">Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics).</span><span class="sxs-lookup"><span data-stu-id="c1636-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="c1636-189">Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél).</span><span class="sxs-lookup"><span data-stu-id="c1636-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="c1636-190">Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).</span><span class="sxs-lookup"><span data-stu-id="c1636-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="c1636-191">Az AzureStorageLinkedService az Azure Storage-fiókot társítja az adat-előállítóval.</span><span class="sxs-lookup"><span data-stu-id="c1636-191">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="c1636-192">Ebben a tárfiókban hozta létre a tárolót, és ebbe töltötte fel az adatokat az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként.</span><span class="sxs-lookup"><span data-stu-id="c1636-192">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="c1636-193">Az AzureSqlLinkedService az Azure SQL Database-t társítja az adat-előállítóval.</span><span class="sxs-lookup"><span data-stu-id="c1636-193">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="c1636-194">A blobtárolóból másolt adatokat a rendszer ebben az adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-194">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="c1636-195">Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az emp táblát az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="c1636-195">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="c1636-196">Társított szolgáltatás létrehozása Azure Storage-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="c1636-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="c1636-197">Ebben a lépésben társítja az Azure Storage-fiókot az adat-előállítójához.</span><span class="sxs-lookup"><span data-stu-id="c1636-197">In this step, you link your Azure storage account to your data factory.</span></span>

1. <span data-ttu-id="c1636-198">Hozza létre az **AzureStorageLinkedService.json** nevű JSON-fájlt a **C:\ADFGetStartedPSH** mappában a következő tartalommal: (Hozza létre az ADFGetStartedPSH mappát, ha az még nem létezik.)</span><span class="sxs-lookup"><span data-stu-id="c1636-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with the following content: (Create the folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c1636-199">A fájl mentése előtt az &lt;accountname&gt; és az &lt;accountkey&gt; kifejezés helyére írja be Azure Storage-tárfiókja nevét, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="c1636-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving the file.</span></span> 

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
2. <span data-ttu-id="c1636-200">Az **Azure PowerShellben** váltson az **ADFGetStartedPSH** mappára.</span><span class="sxs-lookup"><span data-stu-id="c1636-200">In **Azure PowerShell**, switch to the **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="c1636-201">Futtassa a **New-AzureRmDataFactoryLinkedService** parancsmagot az **AzureStorageLinkedService** társított szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1636-201">Run the **New-AzureRmDataFactoryLinkedService** cmdlet to create the linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="c1636-202">Ehhez, valamint az oktatóanyagban használt többi Data Factory-parancsmaghoz is meg kell adnia értékeket a **ResourceGroupName** és a **DataFactoryName** paraméterek számára.</span><span class="sxs-lookup"><span data-stu-id="c1636-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you to pass values for the **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="c1636-203">A New-AzureRmDataFactory parancsmag által visszaadott DataFactory-objektum anélkül is átadható, hogy minden egyes alkalommal meg kellene adnia a ResourceGroupName és a DataFactoryName értékeket a parancsmag futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="c1636-203">Alternatively, you can pass the DataFactory object returned by the New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="c1636-204">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-204">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="c1636-205">A társított szolgáltatás létrehozásának egy másik módja egy erőforráscsoport és egy adat-előállító nevének megadása a DataFactory-objektum helyett.</span><span class="sxs-lookup"><span data-stu-id="c1636-205">Other way of creating this linked service is to specify resource group name and data factory name instead of specifying the DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="c1636-206">Társított szolgáltatás létrehozása Azure SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="c1636-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="c1636-207">Ebben a lépésben társítani fogja az Azure SQL-adatbázist az adat-előállítóhoz.</span><span class="sxs-lookup"><span data-stu-id="c1636-207">In this step, you link your Azure SQL database to your data factory.</span></span>

1. <span data-ttu-id="c1636-208">Hozzon létre egy AzureSqlLinkedService.json nevű JSON-fájlt a C:\ADFGetStartedPSH mappában az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="c1636-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with the following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c1636-209">A &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; és &lt;password&gt; paraméterek értékét cserélje le az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a jelszóra.</span><span class="sxs-lookup"><span data-stu-id="c1636-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
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
2. <span data-ttu-id="c1636-210">Futtassa az alábbi parancsot egy társított szolgáltatás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1636-210">Run the following command to create a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="c1636-211">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-211">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="c1636-212">Győződjön meg arról, hogy az **Allow access to Azure services** (Azure-szolgáltatásokhoz való hozzáférés engedélyezése) beállítás BE van kapcsolva az SQL Database-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c1636-212">Confirm that **Allow access to Azure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="c1636-213">Az ellenőrzéséhez és bekapcsolásához hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c1636-213">To verify and turn it on, do the following steps:</span></span>

    1. <span data-ttu-id="c1636-214">Jelentkezzen be az [Azure Portalra](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c1636-214">Log in to the [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="c1636-215">Kattintson a bal oldalon a **További szolgáltatások>** menüpontra, majd az **SQL kiszolgálók**ra az **ADATBÁZISOK** kategóriában.</span><span class="sxs-lookup"><span data-stu-id="c1636-215">Click **More services >** on the left, and click **SQL servers** in the **DATABASES** category.</span></span>
    3. <span data-ttu-id="c1636-216">Válassza ki a kiszolgálóját az SQL-kiszolgálók listájából.</span><span class="sxs-lookup"><span data-stu-id="c1636-216">Select your server in the list of SQL servers.</span></span>
    4. <span data-ttu-id="c1636-217">Az SQL-kiszolgáló panelen kattintson a **Tűzfal-beállítások mutatása** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="c1636-217">On the SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="c1636-218">A **Tűzfalbeállítások** panelen kattintson a **BE** kapcsolóra az **Azure-szolgáltatások hozzáférésének engedélyezése** beállítás mellett.</span><span class="sxs-lookup"><span data-stu-id="c1636-218">In the **Firewall settings** blade, click **ON** for **Allow access to Azure services**.</span></span>
    6. <span data-ttu-id="c1636-219">Kattintson az eszköztár **Mentés** elemére.</span><span class="sxs-lookup"><span data-stu-id="c1636-219">Click **Save** on the toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="c1636-220">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-220">Create datasets</span></span>
<span data-ttu-id="c1636-221">Az előző lépésben létrehozta az Azure Storage-fiók és az Azure SQL Database összekapcsolását végző társított szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="c1636-221">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="c1636-222">Ebben a lépésben két adatkészletet határoz meg – InputDataset és OutputDataset néven –, amelyek az AzureStorageLinkedService és az AzureSqlLinkedService szolgáltatás által hivatkozott bemeneti és kimeneti adatokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="c1636-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="c1636-223">Az Azure Storage társított szolgáltatása határozza meg azt a kapcsolati sztringet, amelyet futtatáskor a Data Factory szolgáltatás az Azure Storage-fiók csatlakoztatásához használ.</span><span class="sxs-lookup"><span data-stu-id="c1636-223">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="c1636-224">A bemeneti blob adatkészlete (InputDataset) pedig a tárolót és a bemeneti adatokat tartalmazó mappát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-224">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="c1636-225">Ehhez hasonlóan az Azure SQL Database társított szolgáltatása határozza meg azt a kapcsolati sztringet, amelyet futtatáskor a Data Factory szolgáltatás az Azure SQL Database csatlakoztatásához használ.</span><span class="sxs-lookup"><span data-stu-id="c1636-225">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="c1636-226">Az SQL-tábla kimeneti adatkészlete (OututDataset) határozza meg azt az adatbázistáblát, amelybe a rendszer a blobtárolóból származó adatokat másolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-226">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="c1636-227">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-227">Create an input dataset</span></span>
<span data-ttu-id="c1636-228">Ebben a lépésben hozza létre az InputDataset nevű adatkészletet, amely az AzureStorageLinkedService társított szolgáltatás által hivatkozott Azure Storage blobtárolójának (adftutorial) gyökérmappájában található blobfájlra mutat (emp.txt).</span><span class="sxs-lookup"><span data-stu-id="c1636-228">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="c1636-229">Ha nem ad meg értéket a fájlnévnek (vagy kihagyja azt), a rendszer a bemeneti mappában található összes blob adatát a célhelyre másolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-229">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="c1636-230">Ebben az oktatóanyagban a fileName értékét adja meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-230">In this tutorial, you specify a value for the fileName.</span></span>  

1. <span data-ttu-id="c1636-231">Hozzon létre egy JSON-fájlt **InputDataset.json** néven a **C:\ADFGetStartedPSH** mappában az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="c1636-231">Create a JSON file named **InputDataset.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

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

    <span data-ttu-id="c1636-232">Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="c1636-232">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="c1636-233">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c1636-233">Property</span></span> | <span data-ttu-id="c1636-234">Leírás</span><span class="sxs-lookup"><span data-stu-id="c1636-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="c1636-235">type</span><span class="sxs-lookup"><span data-stu-id="c1636-235">type</span></span> | <span data-ttu-id="c1636-236">A tulajdonság beállításának értéke **AzureBlob**, mert az adatok egy Azure Blob Storage-tárban találhatók.</span><span class="sxs-lookup"><span data-stu-id="c1636-236">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="c1636-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="c1636-237">linkedServiceName</span></span> | <span data-ttu-id="c1636-238">A korábban létrehozott **AzureStorageLinkedService** szolgáltatásra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c1636-238">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="c1636-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="c1636-239">folderPath</span></span> | <span data-ttu-id="c1636-240">A **blobtárolót** és a bemeneti blobokat tartalmazó **mappát** határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-240">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="c1636-241">Ebben az oktatóanyagban az adftutorial a blobtároló és a folder a gyökérmappa.</span><span class="sxs-lookup"><span data-stu-id="c1636-241">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="c1636-242">fileName</span><span class="sxs-lookup"><span data-stu-id="c1636-242">fileName</span></span> | <span data-ttu-id="c1636-243">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="c1636-243">This property is optional.</span></span> <span data-ttu-id="c1636-244">Ha kihagyja, a rendszer a folderPath elérési úton található összes fájlt kiválasztja.</span><span class="sxs-lookup"><span data-stu-id="c1636-244">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="c1636-245">Ebben az oktatóanyagban az **emp.txt** a fileName értéke, így a rendszer csak ezt a fájlt használja a feldolgozáshoz.</span><span class="sxs-lookup"><span data-stu-id="c1636-245">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="c1636-246">formátum -> típus</span><span class="sxs-lookup"><span data-stu-id="c1636-246">format -> type</span></span> |<span data-ttu-id="c1636-247">A bemeneti fájl szöveges formátumú, ezért a **TextFormat** értéket használjuk.</span><span class="sxs-lookup"><span data-stu-id="c1636-247">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="c1636-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="c1636-248">columnDelimiter</span></span> | <span data-ttu-id="c1636-249">A bemeneti fájlban **vesszővel (`,`)** vannak elválasztva az oszlopok.</span><span class="sxs-lookup"><span data-stu-id="c1636-249">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="c1636-250">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="c1636-250">frequency/interval</span></span> | <span data-ttu-id="c1636-251">A frequency (gyakoriság) beállítása **Hour** (Óra), az interval (időköz) beállítása pedig **1**, ami azt jelenti, hogy a bemeneti szeletek **óránként** érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c1636-251">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="c1636-252">Vagyis a Data Factory szolgáltatás óránként keres bemeneti adatokat a megadott blobtároló (**adftutorial**) gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="c1636-252">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="c1636-253">A szolgáltatás a folyamat kezdő és befejező időpontja közti időszakban – és nem azon kívül – keres adatokat.</span><span class="sxs-lookup"><span data-stu-id="c1636-253">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="c1636-254">external</span><span class="sxs-lookup"><span data-stu-id="c1636-254">external</span></span> | <span data-ttu-id="c1636-255">Ez a tulajdonság a **true** (igaz) értékre van állítva, ha az adatokat nem ez a folyamat hozta létre.</span><span class="sxs-lookup"><span data-stu-id="c1636-255">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="c1636-256">Az oktatóanyagban használt bemeneti adatok az emp.txt fájlban találhatók, amelyet nem ez a folyamat hoz létre, ezért ezt a tulajdonságot true (igaz) értékre állítottuk.</span><span class="sxs-lookup"><span data-stu-id="c1636-256">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="c1636-257">További információ ezekről a JSON-tulajdonságokról: [Azure Blob-összekötő](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c1636-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="c1636-258">A Data Factory-adatkészlet létrehozásához futtassa az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1636-258">Run the following command to create the Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="c1636-259">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-259">Here is the sample output:</span></span>

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

### <a name="create-an-output-dataset"></a><span data-ttu-id="c1636-260">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-260">Create an output dataset</span></span>
<span data-ttu-id="c1636-261">A lépés ezen részében egy kimeneti adatkészletet hoz létre **OutputDataset** néven.</span><span class="sxs-lookup"><span data-stu-id="c1636-261">In this part of the step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="c1636-262">Ez az adathalmaz egy SQL-táblára mutat abban az Azure SQL Database-adatbázisban, amelyet az **AzureSqlLinkedService** jelöl.</span><span class="sxs-lookup"><span data-stu-id="c1636-262">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="c1636-263">Hozzon létre egy JSON-fájlt **OutputDataset.json** néven a **C:\ADFGetStartedPSH** mappában az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="c1636-263">Create a JSON file named **OutputDataset.json** in the **C:\ADFGetStartedPSH** folder with the following content:</span></span>

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

    <span data-ttu-id="c1636-264">Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="c1636-264">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="c1636-265">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c1636-265">Property</span></span> | <span data-ttu-id="c1636-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="c1636-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="c1636-267">type</span><span class="sxs-lookup"><span data-stu-id="c1636-267">type</span></span> | <span data-ttu-id="c1636-268">A type tulajdonság beállítása **AzureSqlTable**, mert az adatok másolása az Azure SQL Database egyik táblájába történik.</span><span class="sxs-lookup"><span data-stu-id="c1636-268">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="c1636-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="c1636-269">linkedServiceName</span></span> | <span data-ttu-id="c1636-270">A korábban létrehozott **AzureSqlLinkedService** szolgáltatásra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c1636-270">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="c1636-271">tableName</span><span class="sxs-lookup"><span data-stu-id="c1636-271">tableName</span></span> | <span data-ttu-id="c1636-272">Azt a **táblát** határozza meg, amelybe a rendszer az adatokat másolja.</span><span class="sxs-lookup"><span data-stu-id="c1636-272">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="c1636-273">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="c1636-273">frequency/interval</span></span> | <span data-ttu-id="c1636-274">A frequency (gyakoriság) értéke **Hour** (Óra), az interval (időköz) értéke pedig **1**, azaz a rendszer a kimeneti szeleteket **óránként** állítja elő a folyamat kezdő és befejező időpontja közti időszakban (és nem azon kívül).</span><span class="sxs-lookup"><span data-stu-id="c1636-274">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="c1636-275">Az adatbázis emp táblájában három oszlop van – **ID**, **FirstName** és **LastName**.</span><span class="sxs-lookup"><span data-stu-id="c1636-275">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="c1636-276">Az ID azonosítóoszlop, ezért itt csak a **FirstName** és **LastName** tulajdonságokat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="c1636-276">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="c1636-277">További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c1636-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="c1636-278">A data factory-adatkészlet létrehozásához futtassa az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1636-278">Run the following command to create the data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="c1636-279">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-279">Here is the sample output:</span></span>

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

## <a name="create-a-pipeline"></a><span data-ttu-id="c1636-280">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1636-280">Create a pipeline</span></span>
<span data-ttu-id="c1636-281">Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="c1636-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="c1636-282">Jelenleg a kimeneti adatkészlet határozza meg az ütemezést.</span><span class="sxs-lookup"><span data-stu-id="c1636-282">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="c1636-283">Az oktatóanyagban a kimeneti adatkészletet úgy konfiguráljuk, hogy a szeletek létrehozása óránként történjen meg.</span><span class="sxs-lookup"><span data-stu-id="c1636-283">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="c1636-284">A folyamat kezdő és befejező időpontja között egy nap, azaz 24 óra telik el.</span><span class="sxs-lookup"><span data-stu-id="c1636-284">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="c1636-285">Ezért a folyamat a kimeneti adatkészletből 24 szeletet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c1636-285">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 


1. <span data-ttu-id="c1636-286">Hozzon létre egy JSON-fájlt **ADFTutorialPipeline.json** néven a **C:\ADFGetStartedPSH** mappában az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="c1636-286">Create a JSON file named **ADFTutorialPipeline.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
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
    <span data-ttu-id="c1636-287">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="c1636-287">Note the following points:</span></span>
   
    - <span data-ttu-id="c1636-288">A tevékenységek szakaszban csak egyetlen tevékenység van, amelynek a **típusa** **Copy** értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="c1636-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="c1636-289">További információ a másolási tevékenységről: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-289">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="c1636-290">A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.</span><span class="sxs-lookup"><span data-stu-id="c1636-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="c1636-291">A tevékenység bemenetének beállítása **InputDataset**, a kimeneté pedig **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="c1636-291">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="c1636-292">A **typeProperties** szakaszban forrástípusként a **BlobSource**, fogadótípusként pedig az **SqlSink** érték van megadva.</span><span class="sxs-lookup"><span data-stu-id="c1636-292">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="c1636-293">A másolási tevékenység által forrásként és fogadóként támogatott adattárak teljes listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c1636-293">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="c1636-294">Egy forrásként/fogadóként támogatott konkrét adattár használatával kapcsolatos útmutatóért kattintson a tábla adott hivatkozására.</span><span class="sxs-lookup"><span data-stu-id="c1636-294">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
     
    <span data-ttu-id="c1636-295">A **start** (kezdés) tulajdonság értékét cserélje az aktuális, az **end** (befejezés) tulajdonság értékét pedig a következő napra.</span><span class="sxs-lookup"><span data-stu-id="c1636-295">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="c1636-296">Azt is megteheti, hogy a dátum-időpont paraméternek csak a dátum részét adja meg, az időpont részét pedig kihagyja.</span><span class="sxs-lookup"><span data-stu-id="c1636-296">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="c1636-297">Megadhatja például a „2016-02-03” értéket, amely a következőnek felel meg: „2016-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="c1636-297">For example, "2016-02-03", which is equivalent to "2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="c1636-298">Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni.</span><span class="sxs-lookup"><span data-stu-id="c1636-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="c1636-299">Például: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="c1636-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="c1636-300">Az **end** (befejező) időpont megadása opcionális, a jelen oktatóanyagban azonban azt is használjuk.</span><span class="sxs-lookup"><span data-stu-id="c1636-300">The **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="c1636-301">Ha nem adja meg az **end** (befejezés) tulajdonság értékét, akkor a rendszer a „**kezdő időpont + 48 óra**” számítással határozza meg azt.</span><span class="sxs-lookup"><span data-stu-id="c1636-301">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="c1636-302">A folyamat határozatlan ideig történő futtatásához adja meg a **9999-09-09** értéket az **end** (befejezés) tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="c1636-302">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="c1636-303">Az előző példában 24 adatszelet van, mert a rendszer óránként létrehoz egy adatszeletet.</span><span class="sxs-lookup"><span data-stu-id="c1636-303">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="c1636-304">A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c1636-305">A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="c1636-306">A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="c1636-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="c1636-307">Az SqlSink által támogatott JSON-tulajdonságok leírása az [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md) című cikkben található.</span><span class="sxs-lookup"><span data-stu-id="c1636-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="c1636-308">A data factory-tábla létrehozásához futtassa az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="c1636-308">Run the following command to create the data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="c1636-309">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-309">Here is the sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="c1636-310">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="c1636-310">**Congratulations!**</span></span> <span data-ttu-id="c1636-311">Sikeresen létrehozott egy Azure-beli adat-előállítót egy olyan folyamattal, amely az Azure Blob Storage-ból az Azure SQL Database-be másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="c1636-311">You have successfully created an Azure data factory with a pipeline to copy data from an Azure blob storage to an Azure SQL database.</span></span> 

## <a name="monitor-the-pipeline"></a><span data-ttu-id="c1636-312">A folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="c1636-312">Monitor the pipeline</span></span>
<span data-ttu-id="c1636-313">Ebben a lépésben az Azure PowerShell használatával figyeli az Azure data factory eseményeit.</span><span class="sxs-lookup"><span data-stu-id="c1636-313">In this step, you use Azure PowerShell to monitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="c1636-314">A &lt;DataFactoryName&gt; helyére írja be az adat-előállító nevét, futtassa a **Get-AzureRmDataFactory** parancsot, majd társítsa a kimenetet a $df változóhoz.</span><span class="sxs-lookup"><span data-stu-id="c1636-314">Replace &lt;DataFactoryName&gt; with the name of your data factory and run **Get-AzureRmDataFactory**, and assign the output to a variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="c1636-315">Példa:</span><span class="sxs-lookup"><span data-stu-id="c1636-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="c1636-316">Ez után futtassa le a $df tartalmát a következő kimenet előállításához:</span><span class="sxs-lookup"><span data-stu-id="c1636-316">Then, run print the contents of $df to see the following output:</span></span> 
    
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
2. <span data-ttu-id="c1636-317">A **Get-AzureRmDataFactorySlice** parancs futtatásával hívja le az összes szelet adatait a folyamat **OutputDataset** nevű kimeneti adatkészletében.</span><span class="sxs-lookup"><span data-stu-id="c1636-317">Run **Get-AzureRmDataFactorySlice** to get details about all slices of the **OutputDataset**, which is the output dataset of the pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="c1636-318">Ennek a beállításnak egyeznie kell a **Start** (Kezdés) értékkel a folyamat JSON-fájljában.</span><span class="sxs-lookup"><span data-stu-id="c1636-318">This setting should match the **Start** value in the pipeline JSON.</span></span> <span data-ttu-id="c1636-319">24 szeletet kell látnia, éjféltől másnap éjfélig.</span><span class="sxs-lookup"><span data-stu-id="c1636-319">You should see 24 slices, one for each hour from 12 AM of the current day to 12 AM of the next day.</span></span>

   <span data-ttu-id="c1636-320">Itt látható három példaszelet a kimenetből:</span><span class="sxs-lookup"><span data-stu-id="c1636-320">Here are three sample slices from the output:</span></span> 

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
3. <span data-ttu-id="c1636-321">A **Get-AzureRmDataFactoryRun** parancs futtatásával kérje le egy **adott** szelet tevékenységfuttatásainak részleteit.</span><span class="sxs-lookup"><span data-stu-id="c1636-321">Run **Get-AzureRmDataFactoryRun** to get the details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="c1636-322">Az előbbi parancs kimenetéből kimásolt dátum-idő értékkel adjon értéket a StartDateTime paraméternek.</span><span class="sxs-lookup"><span data-stu-id="c1636-322">Copy the date-time value from the output of the previous command to specify the value for the StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="c1636-323">Itt látható a kimenet mintája:</span><span class="sxs-lookup"><span data-stu-id="c1636-323">Here is the sample output:</span></span> 

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

<span data-ttu-id="c1636-324">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációt a [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) (Data Factory-parancsmagok referenciája) című cikk tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c1636-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="c1636-325">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c1636-325">Summary</span></span>
<span data-ttu-id="c1636-326">Az oktatóanyag során létrehozott egy Azure data factoryt, hogy adatokat másoljon egy Azure-blobból egy Azure SQL Database-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="c1636-326">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="c1636-327">A PowerShellt használta a data factory, a társított szolgáltatások, az adatkészletek és a folyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c1636-327">You used PowerShell to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="c1636-328">Az oktatóanyag során a következő főbb lépéseket végezte el:</span><span class="sxs-lookup"><span data-stu-id="c1636-328">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="c1636-329">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="c1636-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="c1636-330">**Társított szolgáltatásokat** hozott létre:</span><span class="sxs-lookup"><span data-stu-id="c1636-330">Created **linked services**:</span></span>

   <span data-ttu-id="c1636-331">a.</span><span class="sxs-lookup"><span data-stu-id="c1636-331">a.</span></span> <span data-ttu-id="c1636-332">Egy **Azure Storage** társított szolgáltatást a bemeneti adatokat tároló Azure Storage-fiók társításához.</span><span class="sxs-lookup"><span data-stu-id="c1636-332">An **Azure Storage** linked service to link your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="c1636-333">b.</span><span class="sxs-lookup"><span data-stu-id="c1636-333">b.</span></span> <span data-ttu-id="c1636-334">Egy **Azure SQL** társított szolgáltatást a kimeneti adatokat tároló SQL Database társításához.</span><span class="sxs-lookup"><span data-stu-id="c1636-334">An **Azure SQL** linked service to link your SQL database that holds the output data.</span></span>
3. <span data-ttu-id="c1636-335">**Adatkészleteket** hozott létre, amelyek a folyamat bemeneti és kimeneti adatait írják le.</span><span class="sxs-lookup"><span data-stu-id="c1636-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="c1636-336">Létrehozott egy **folyamatot** egy **Másolási tevékenységgel**, ahol a **BlobSource** a forrás, az **SqlSink** pedig a fogadó.</span><span class="sxs-lookup"><span data-stu-id="c1636-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as the source and **SqlSink** as the sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1636-337">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1636-337">Next steps</span></span>
<span data-ttu-id="c1636-338">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="c1636-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="c1636-339">Az alábbi táblázatban a másolási tevékenység által támogatott forrásadattárak és céladattárak listája látható:</span><span class="sxs-lookup"><span data-stu-id="c1636-339">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="c1636-340">A táblázatban lévő adattárak hivatkozására kattintva megismerheti az adattárakba és az adattárakból történő adatmásolás módszereit.</span><span class="sxs-lookup"><span data-stu-id="c1636-340">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span> 

