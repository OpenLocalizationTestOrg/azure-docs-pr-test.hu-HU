---
title: "aaaBuild az első adat-előállítóban (PowerShell) |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy minta Azure Data Factory-folyamatot az Azure PowerShell használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a><span data-ttu-id="e8b1c-103">Oktatóanyag: Az első Azure data factory létrehozása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e8b1c-103">Tutorial: Build your first Azure data factory using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8b1c-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e8b1c-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="e8b1c-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e8b1c-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="e8b1c-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8b1c-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="e8b1c-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8b1c-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="e8b1c-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="e8b1c-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="e8b1c-109">REST API</span><span class="sxs-lookup"><span data-stu-id="e8b1c-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

<span data-ttu-id="e8b1c-110">Ez a cikk használhatja az Azure PowerShell toocreate az első az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-110">In this article, you use Azure PowerShell toocreate your first Azure data factory.</span></span> <span data-ttu-id="e8b1c-111">toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="e8b1c-112">Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="e8b1c-113">Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="e8b1c-114">hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="e8b1c-115">hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="e8b1c-116">Azt nem másolja az adatokat egy forrás adatokat tároló tooa cél adattárból.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-116">It does not copy data from a source data store tooa destination data store.</span></span> <span data-ttu-id="e8b1c-117">Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-117">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="e8b1c-118">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="e8b1c-119">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="e8b1c-120">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8b1c-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e8b1c-121">Prerequisites</span></span>
* <span data-ttu-id="e8b1c-122">Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="e8b1c-123">Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="e8b1c-124">(választható) Ez a cikk nem foglalkozik minden hello Data Factory-parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-124">(optional) This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="e8b1c-125">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) (Data Factory-parancsmagok referenciája) című cikket.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-125">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on Data Factory cmdlets.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="e8b1c-126">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-126">Create data factory</span></span>
<span data-ttu-id="e8b1c-127">Ezt a lépést használhatja az Azure PowerShell toocreate egy Azure Data Factory nevű **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-127">In this step, you use Azure PowerShell toocreate an Azure Data Factory named **FirstDataFactoryPSH**.</span></span> <span data-ttu-id="e8b1c-128">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-128">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="e8b1c-129">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-129">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="e8b1c-130">Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-130">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="e8b1c-131">Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-131">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="e8b1c-132">Indítsa el az Azure PowerShell, és futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-132">Start Azure PowerShell and run hello following command.</span></span> <span data-ttu-id="e8b1c-133">Azure PowerShell nyitva hagyja az oktatóanyag hello végéig.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-133">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="e8b1c-134">Zárja be és nyissa meg újra, ha szüksége toorun ezek a parancsok újra.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-134">If you close and reopen, you need toorun these commands again.</span></span>
   * <span data-ttu-id="e8b1c-135">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * <span data-ttu-id="e8b1c-136">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-136">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * <span data-ttu-id="e8b1c-137">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="e8b1c-138">Ez az előfizetés ugyanaz, mint egy Azure-portálon hello használt hello kell hello.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-138">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. <span data-ttu-id="e8b1c-139">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-139">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    <span data-ttu-id="e8b1c-140">Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-140">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="e8b1c-141">Ha egy másik erőforráscsoportban található használatához szüksége van-e toouse helyett, ebben az oktatóanyagban ADFTutorialResourceGroup azt.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-141">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="e8b1c-142">Futtassa a hello **New-AzureRmDataFactory** parancsmag által létrehozott egy adat-előállító nevű **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-142">Run hello **New-AzureRmDataFactory** cmdlet that creates a data factory named **FirstDataFactoryPSH**.</span></span>

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
<span data-ttu-id="e8b1c-143">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-143">Note hello following points:</span></span>

* <span data-ttu-id="e8b1c-144">Azure Data Factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-144">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="e8b1c-145">Ha hello hibaüzenet **nem érhető el adat-előállító "FirstDataFactoryPSH"**, módosítsa hello nevét (például yournameFirstDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-145">If you receive hello error **Data factory name “FirstDataFactoryPSH” is not available**, change hello name (for example, yournameFirstDataFactoryPSH).</span></span> <span data-ttu-id="e8b1c-146">Használja ezt az ADFTutorialFactoryPSH helyett az oktatóanyag lépéseinek végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-146">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="e8b1c-147">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-147">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="e8b1c-148">toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda</span><span class="sxs-lookup"><span data-stu-id="e8b1c-148">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="e8b1c-149">hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-149">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
* <span data-ttu-id="e8b1c-150">Ha hello hibaüzenet: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**", hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-150">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="e8b1c-151">Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-151">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
      <span data-ttu-id="e8b1c-152">A következő parancs tooconfirm hello adott hello szolgáltató regisztrálva van a Data Factory futtathatja:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-152">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="e8b1c-153">Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-153">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="e8b1c-154">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-154">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="e8b1c-155">Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-155">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="e8b1c-156">Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolására, adja meg a bemeneti és kimeneti adatkészletek toorepresent bemeneti/kimeneti adatai csatolt adatok áruházakból és majd hozzon létre egy tevékenység által használt ezek az adatkészletek hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-156">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="e8b1c-157">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-157">Create linked services</span></span>
<span data-ttu-id="e8b1c-158">Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-158">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="e8b1c-159">hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-159">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="e8b1c-160">HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-160">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="e8b1c-161">Azonosíthatja az adatokat tároló/számítási szolgáltatások a forgatókönyvben használt, és ezen szolgáltatások toohello adat-előállító hivatkozás összekapcsolt szolgáltatások létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-161">Identify what data store/compute services are used in your scenario and link those services toohello data factory by creating linked services.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="e8b1c-162">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-162">Create Azure Storage linked service</span></span>
<span data-ttu-id="e8b1c-163">Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-163">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="e8b1c-164">Hello használata azonos Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-164">You use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="e8b1c-165">A következő hello hello C:\ADFGetStarted mappában StorageLinkedService.json nevű JSON-fájl tartalmának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-165">Create a JSON file named StorageLinkedService.json in hello C:\ADFGetStarted folder with hello following content.</span></span> <span data-ttu-id="e8b1c-166">Hozzon létre hello mappát ADFGetStarted, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-166">Create hello folder ADFGetStarted if it does not already exist.</span></span>

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
    <span data-ttu-id="e8b1c-167">Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-167">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="e8b1c-168">toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-168">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
2. <span data-ttu-id="e8b1c-169">Azure PowerShell, a kapcsoló toohello ADFGetStarted mappa.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-169">In Azure PowerShell, switch toohello ADFGetStarted folder.</span></span>
3. <span data-ttu-id="e8b1c-170">Használhatja a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott összekapcsolt szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-170">You can use hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates a linked service.</span></span> <span data-ttu-id="e8b1c-171">Ez a parancsmag és egyéb adat-előállító parancsmagok használata ebben az oktatóanyagban meg toopass paraméterértékek szükségesek hello *ResourceGroupName* és *DataFactoryName* paraméterek.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-171">This cmdlet and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello *ResourceGroupName* and *DataFactoryName* parameters.</span></span> <span data-ttu-id="e8b1c-172">Másik megoldásként használhatja **Get-AzureRmDataFactory** tooget egy **DataFactory** objektumot, és adja át a hello objektum beírása nélkül *ResourceGroupName* és  *DataFactoryName* minden alkalommal, amikor futtatja a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-172">Alternatively, you can use **Get-AzureRmDataFactory** tooget a **DataFactory** object and pass hello object without typing *ResourceGroupName* and *DataFactoryName* each time you run a cmdlet.</span></span> <span data-ttu-id="e8b1c-173">Futtatási hello következő parancsot a tooassign hello kimenete hello **Get-AzureRmDataFactory** parancsmag tooa **$df** változó.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-173">Run hello following command tooassign hello output of hello **Get-AzureRmDataFactory** cmdlet tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. <span data-ttu-id="e8b1c-174">Most, futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott hello kapcsolódó **StorageLinkedService** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-174">Now, run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked **StorageLinkedService** service.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="e8b1c-175">Hello volt futtatásakor **Get-AzureRmDataFactory** parancsmag és a hozzárendelt hello kimeneti toohello **$df** változó, akkor egy hello toospecify értékeinek *ResourceGroupName*és *DataFactoryName* paramétereket az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-175">If you hadn't run hello **Get-AzureRmDataFactory** cmdlet and assigned hello output toohello **$df** variable, you would have toospecify values for hello *ResourceGroupName* and *DataFactoryName* parameters as follows.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="e8b1c-176">Ha bezárja az Azure PowerShell hello oktatóanyag hello középső, rendelkezik-e toorun hello **Get-AzureRmDataFactory** parancsmag Azure PowerShell toocomplete hello oktatóanyag következő indításakor.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-176">If you close Azure PowerShell in hello middle of hello tutorial, you have toorun hello **Get-AzureRmDataFactory** cmdlet next time you start Azure PowerShell toocomplete hello tutorial.</span></span>

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="e8b1c-177">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-177">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="e8b1c-178">Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-178">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="e8b1c-179">az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-179">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="e8b1c-180">Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-180">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="e8b1c-181">További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-181">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="e8b1c-182">Hozzon létre egy JSON fájlt **HDInsightOnDemandLinkedService**a hello .JSON kiterjesztésű **C:\ADFGetStarted** hello tartalom a következő mappában.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-182">Create a JSON file named **HDInsightOnDemandLinkedService**.json in hello **C:\ADFGetStarted** folder with hello following content.</span></span>

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
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    <span data-ttu-id="e8b1c-183">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="e8b1c-184">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8b1c-184">Property</span></span> | <span data-ttu-id="e8b1c-185">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b1c-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="e8b1c-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="e8b1c-186">ClusterSize</span></span> |<span data-ttu-id="e8b1c-187">HDInsight-fürt hello hello méretét adja meg.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="e8b1c-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="e8b1c-188">TimeToLive</span></span> |<span data-ttu-id="e8b1c-189">Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="e8b1c-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="e8b1c-190">linkedServiceName</span></span> |<span data-ttu-id="e8b1c-191">Adja meg, amely a HDInsight által létrehozott használt toostore hello naplók hello storage-fiók</span><span class="sxs-lookup"><span data-stu-id="e8b1c-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

    <span data-ttu-id="e8b1c-192">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-192">Note hello following points:</span></span>

   * <span data-ttu-id="e8b1c-193">hello adat-előállító létrehoz egy **Linux-alapú** a hello JSON meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="e8b1c-194">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="e8b1c-195">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="e8b1c-196">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="e8b1c-197">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="e8b1c-198">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="e8b1c-199">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-199">This behavior is by design.</span></span> <span data-ttu-id="e8b1c-200">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="e8b1c-201">hello feldolgozása hello fürt automatikusan törlődnek.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="e8b1c-202">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="e8b1c-203">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="e8b1c-204">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp".</span><span class="sxs-lookup"><span data-stu-id="e8b1c-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="e8b1c-205">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="e8b1c-206">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
2. <span data-ttu-id="e8b1c-207">Futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott hello társított HDInsightOnDemandLinkedService nevű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-207">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked service called HDInsightOnDemandLinkedService.</span></span>
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a><span data-ttu-id="e8b1c-208">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-208">Create datasets</span></span>
<span data-ttu-id="e8b1c-209">Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-209">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="e8b1c-210">Ezek az adatkészletek tekintse meg a toohello **StorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-210">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="e8b1c-211">hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-211">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="e8b1c-212">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-212">Create input dataset</span></span>
1. <span data-ttu-id="e8b1c-213">Hozzon létre egy JSON fájlt **InputTable.json** a hello **C:\ADFGetStarted** hello tartalom a következő mappában:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-213">Create a JSON file named **InputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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
    <span data-ttu-id="e8b1c-214">hello JSON meghatározása nevű adatkészlete **AzureBlobInput**, amely jelenti, hogy egy tevékenység hello folyamat a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-214">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="e8b1c-215">Emellett meghatározza, hogy hello bemeneti adatok nevű hello blob tárolóban található **adfgetstarted** és nevű hello mappát **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-215">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

    <span data-ttu-id="e8b1c-216">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-216">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="e8b1c-217">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="e8b1c-217">Property</span></span> | <span data-ttu-id="e8b1c-218">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b1c-218">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="e8b1c-219">type</span><span class="sxs-lookup"><span data-stu-id="e8b1c-219">type</span></span> |<span data-ttu-id="e8b1c-220">mivel az adatok találhatók az Azure blob storage hello type tulajdonság beállítása tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-220">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
   | <span data-ttu-id="e8b1c-221">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="e8b1c-221">linkedServiceName</span></span> |<span data-ttu-id="e8b1c-222">a korábban létrehozott StorageLinkedService toohello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-222">refers toohello StorageLinkedService you created earlier.</span></span> |
   | <span data-ttu-id="e8b1c-223">fileName</span><span class="sxs-lookup"><span data-stu-id="e8b1c-223">fileName</span></span> |<span data-ttu-id="e8b1c-224">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-224">This property is optional.</span></span> <span data-ttu-id="e8b1c-225">Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-225">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="e8b1c-226">Ebben az esetben csak hello input.log dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-226">In this case, only hello input.log is processed.</span></span> |
   | <span data-ttu-id="e8b1c-227">type</span><span class="sxs-lookup"><span data-stu-id="e8b1c-227">type</span></span> |<span data-ttu-id="e8b1c-228">hello naplófájlok szöveges formátumú, így szöveges használjuk.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-228">hello log files are in text format, so we use TextFormat.</span></span> |
   | <span data-ttu-id="e8b1c-229">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="e8b1c-229">columnDelimiter</span></span> |<span data-ttu-id="e8b1c-230">oszlopok hello naplófájlokban határolja hello. vesszővel (,) karakter.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-230">columns in hello log files are delimited by hello comma character (,).</span></span> |
   | <span data-ttu-id="e8b1c-231">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="e8b1c-231">frequency/interval</span></span> |<span data-ttu-id="e8b1c-232">gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-232">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="e8b1c-233">external</span><span class="sxs-lookup"><span data-stu-id="e8b1c-233">external</span></span> |<span data-ttu-id="e8b1c-234">Ez a tulajdonság beállítása tootrue hello bemeneti adatok nem generálja hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-234">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |
2. <span data-ttu-id="e8b1c-235">Futtassa a következő parancs az Azure PowerShell toocreate hello adat-előállító dataset hello:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-235">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="e8b1c-236">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-236">Create output dataset</span></span>
<span data-ttu-id="e8b1c-237">Hello kimeneti adatkészlet toorepresent hello kimeneti tárolt adatok hello Azure Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-237">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="e8b1c-238">Hozzon létre egy JSON fájlt **OutputTable.json** a hello **C:\ADFGetStarted** hello tartalom a következő mappában:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-238">Create a JSON file named **OutputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
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
    <span data-ttu-id="e8b1c-239">hello JSON meghatározása nevű adatkészlete **AzureBlobOutput**, amely jelenti, hogy egy tevékenység hello folyamat kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-239">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="e8b1c-240">Emellett meghatározza, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfgetstarted** és nevű hello mappát **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-240">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="e8b1c-241">Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-241">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>
2. <span data-ttu-id="e8b1c-242">Futtassa a következő parancs az Azure PowerShell toocreate hello adat-előállító dataset hello:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-242">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a><span data-ttu-id="e8b1c-243">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-243">Create pipeline</span></span>
<span data-ttu-id="e8b1c-244">Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-244">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="e8b1c-245">Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-245">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="e8b1c-246">hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-246">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="e8b1c-247">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-247">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="e8b1c-248">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-248">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="e8b1c-249">a következő JSON hello használt hello tulajdonságok hello végén ebben a szakaszban lévő magyarázatát olvashatja.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-249">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="e8b1c-250">A következő hello hello C:\ADFGetStarted mappában MyFirstPipelinePSH.json nevű JSON-fájl tartalmának létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-250">Create a JSON file named MyFirstPipelinePSH.json in hello C:\ADFGetStarted folder with hello following content:</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="e8b1c-251">Cserélje le **storageaccountname** hello nevű hello JSON a storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-251">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
   >
   >

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
                        "scriptLinkedService": "StorageLinkedService",
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
    <span data-ttu-id="e8b1c-252">Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatokat a HDInsight-fürtök létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-252">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="e8b1c-253">hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **StorageLinkedService**), majd a **parancsfájl**  hello tároló mappa **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-253">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="e8b1c-254">Hello **meghatározása** szakaszban használt toospecify hello futásidejű beállításokat, lehet, toohello hive parancsfájl át Hive konfigurációs értékeket (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-254">hello **defines** section is used toospecify hello runtime settings that be passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="e8b1c-255">Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-255">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="e8b1c-256">Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-256">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e8b1c-257">"Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) hello példában használt JSON tulajdonságokat vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-257">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties that are used in hello example.</span></span>

2. <span data-ttu-id="e8b1c-258">Ellenőrizze, hogy látható-e hello **input.log** hello fájlban **adfgetstarted/inputdata** hello Azure blob Storage tárolóban, és futtassa a következő parancs toodeploy hello csővezeték hello mappájában.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-258">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="e8b1c-259">Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-259">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. <span data-ttu-id="e8b1c-260">Gratulálunk, sikeresen létrehozta első folyamatát az Azure PowerShell használatával!</span><span class="sxs-lookup"><span data-stu-id="e8b1c-260">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="e8b1c-261">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="e8b1c-261">Monitor pipeline</span></span>
<span data-ttu-id="e8b1c-262">Ezt a lépést használhatja az Azure PowerShell toomonitor egy az Azure data factory lesz.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-262">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="e8b1c-263">Futtatás **Get-AzureRmDataFactory** , és rendelje hozzá a hello kimeneti tooa **$df** változó.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-263">Run **Get-AzureRmDataFactory** and assign hello output tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. <span data-ttu-id="e8b1c-264">Futtatás **Get-AzureRmDataFactorySlice** hello az összes szeletek tooget adatait **EmpSQLTable**, vagyis hello eredménytábla hello folyamatának.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-264">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **EmpSQLTable**, which is hello output table of hello pipeline.</span></span>

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    <span data-ttu-id="e8b1c-265">Figyelje meg, hogy az itt megadott StartDateTime hello azonos indítsa el a kérdéses hello adatcsatorna JSON hello.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-265">Notice that hello StartDateTime you specify here is hello same start time specified in hello pipeline JSON.</span></span> <span data-ttu-id="e8b1c-266">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-266">Here is hello sample output:</span></span>

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="e8b1c-267">Futtatás **Get-AzureRmDataFactoryRun** tooget hello részletek tevékenység egy adott szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-267">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a specific slice.</span></span>

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    <span data-ttu-id="e8b1c-268">Itt egy hello minta kimenet:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-268">Here is hello sample output:</span></span> 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    <span data-ttu-id="e8b1c-269">Akkor is tartsa futtatja ezt a parancsmagot amíg megjelenik a hello szelet **készen** állapota vagy **sikertelen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-269">You can keep running this cmdlet until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="e8b1c-270">Ha hello szelet készen állapotban van, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-270">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="e8b1c-271">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-271">Creation of an on-demand HDInsight cluster usually takes some time.</span></span>

    ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="e8b1c-273">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-273">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="e8b1c-274">Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-274">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
> <span data-ttu-id="e8b1c-275">hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-275">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="e8b1c-276">Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-276">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

## <a name="summary"></a><span data-ttu-id="e8b1c-277">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e8b1c-277">Summary</span></span>
<span data-ttu-id="e8b1c-278">Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-278">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="e8b1c-279">A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-279">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="e8b1c-280">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-280">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="e8b1c-281">Létrehozott két **társított szolgáltatást**:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-281">Created two **linked services**:</span></span>
   1. <span data-ttu-id="e8b1c-282">**Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-282">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="e8b1c-283">**Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-283">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="e8b1c-284">Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-284">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="e8b1c-285">Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-285">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="e8b1c-286">Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-286">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8b1c-287">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8b1c-287">Next steps</span></span>
<span data-ttu-id="e8b1c-288">Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti Azure HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-288">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="e8b1c-289">Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="e8b1c-289">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="e8b1c-290">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e8b1c-290">See Also</span></span>
| <span data-ttu-id="e8b1c-291">Témakör</span><span class="sxs-lookup"><span data-stu-id="e8b1c-291">Topic</span></span> | <span data-ttu-id="e8b1c-292">Leírás</span><span class="sxs-lookup"><span data-stu-id="e8b1c-292">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="e8b1c-293">A Data Factory parancsmagjainak leírása</span><span class="sxs-lookup"><span data-stu-id="e8b1c-293">Data Factory Cmdlet Reference</span></span>](/powershell/module/azurerm.datafactories) |<span data-ttu-id="e8b1c-294">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-294">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="e8b1c-295">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="e8b1c-295">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="e8b1c-296">Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-296">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="e8b1c-297">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="e8b1c-297">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="e8b1c-298">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-298">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="e8b1c-299">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="e8b1c-299">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="e8b1c-300">Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-300">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="e8b1c-301">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="e8b1c-301">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="e8b1c-302">Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e8b1c-302">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
