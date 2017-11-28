---
title: "Oktatóanyag: Használata REST API toocreate egy Azure Data Factory-folyamathoz |} Microsoft Docs"
description: "Ebben az oktatóanyagban a REST API toocreate egy Azure Data Factory-folyamat az az Azure blob storage Azure SQL-adatbázis a másolási tevékenység toocopy adatokkal használhatja."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="cb3ae-103">Oktatóanyag: Használata REST API toocreate egy Azure Data Factory-folyamat toocopy adatok</span><span class="sxs-lookup"><span data-stu-id="cb3ae-103">Tutorial: Use REST API toocreate an Azure Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cb3ae-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb3ae-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="cb3ae-105">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="cb3ae-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="cb3ae-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cb3ae-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="cb3ae-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb3ae-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="cb3ae-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cb3ae-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="cb3ae-109">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="cb3ae-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="cb3ae-110">REST API</span><span class="sxs-lookup"><span data-stu-id="cb3ae-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="cb3ae-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="cb3ae-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="cb3ae-112">Ebből a cikkből megismerheti, hogyan toouse REST API toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-112">In this article, you learn how toouse REST API toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="cb3ae-113">Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="cb3ae-114">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="cb3ae-115">hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="cb3ae-116">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="cb3ae-117">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="cb3ae-118">További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="cb3ae-119">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="cb3ae-120">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="cb3ae-121">További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="cb3ae-122">Ez a cikk nem foglalkozik az összes hello Data Factory REST API.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-122">This article does not cover all hello Data Factory REST API.</span></span> <span data-ttu-id="cb3ae-123">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory REST API Reference](/rest/api/datafactory/) (Data Factory REST API referenciája) című cikket.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-123">See [Data Factory REST API Reference](/rest/api/datafactory/) for comprehensive documentation on Data Factory cmdlets.</span></span>
>  
> <span data-ttu-id="cb3ae-124">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="cb3ae-125">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb3ae-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cb3ae-126">Prerequisites</span></span>
* <span data-ttu-id="cb3ae-127">Lépkedjen végig [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) és teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-127">Go through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="cb3ae-128">Telepítse gépére a [Curl](https://curl.haxx.se/dlwiz/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-128">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="cb3ae-129">A további parancsok toocreate egy adat-előállító hello Curl eszközt kell használni.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-129">You use hello Curl tool with REST commands toocreate a data factory.</span></span> 
* <span data-ttu-id="cb3ae-130">Az [ebben a cikkben](../azure-resource-manager/resource-group-create-service-principal-portal.md) szereplő utasításokat követve végezze el a következőket:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-130">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span> 
  1. <span data-ttu-id="cb3ae-131">Hozzon létre egy **ADFCopyTutorialApp** nevű webalkalmazást az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-131">Create a Web application named **ADFCopyTutorialApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="cb3ae-132">Szerezze be az **ügyfél-azonosítót** és a **titkos kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-132">Get **client ID** and **secret key**.</span></span> 
  3. <span data-ttu-id="cb3ae-133">Szerezze be a **bérlőazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-133">Get **tenant ID**.</span></span> 
  4. <span data-ttu-id="cb3ae-134">Rendelje hozzá a hello **ADFCopyTutorialApp** alkalmazás toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-134">Assign hello **ADFCopyTutorialApp** application toohello **Data Factory Contributor** role.</span></span>  
* <span data-ttu-id="cb3ae-135">Telepítse az [Azure PowerShellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-135">Install [Azure PowerShell](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="cb3ae-136">Indítsa el **PowerShell** és hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-136">Launch **PowerShell** and do hello following steps.</span></span> <span data-ttu-id="cb3ae-137">Azure PowerShell nyitva hagyja az oktatóanyag hello végéig.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-137">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="cb3ae-138">Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-138">If you close and reopen, you need toorun hello commands again.</span></span>
  
  1. <span data-ttu-id="cb3ae-139">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-139">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. <span data-ttu-id="cb3ae-140">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-140">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. <span data-ttu-id="cb3ae-141">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-141">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="cb3ae-142">Cserélje le  **&lt;NameOfAzureSubscription** &gt; hello nevet, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-142">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span> 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. <span data-ttu-id="cb3ae-143">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello hello PowerShell parancsot a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-143">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      <span data-ttu-id="cb3ae-144">Ha hello erőforráscsoportban már létezik, akkor adja meg, hogy tooupdate (Y), vagy legyen (n).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-144">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span> 
     
      <span data-ttu-id="cb3ae-145">Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-145">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="cb3ae-146">Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-146">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="cb3ae-147">JSON-definíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-147">Create JSON definitions</span></span>
<span data-ttu-id="cb3ae-148">Következő hello mappáját curl.exe a JSON-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-148">Create following JSON files in hello folder where curl.exe is located.</span></span> 

### <a name="datafactoryjson"></a><span data-ttu-id="cb3ae-149">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-149">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cb3ae-150">Egyedinek kell lennie globálisan, ezért érdemes tooprefix/utótag ADFCopyTutorialDF toomake azt egy egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-150">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span> 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="cb3ae-151">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-151">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cb3ae-152">Az **accountname** és az **accountkey** kifejezés helyére írja be Azure Storage-tárfiókja nevére, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-152">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="cb3ae-153">toolearn hogyan tooget tárhelyét férnek hozzá, tekintse meg [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-153">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>

```JSON
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

<span data-ttu-id="cb3ae-154">A JSON tulajdonságokról további részleteket tartalmaz az [Azure Storage társított szolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) című cikk.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-154">For details about JSON properties, see [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span></span>

### <a name="azuersqllinkedservicejson"></a><span data-ttu-id="cb3ae-155">azuersqllinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-155">azuersqllinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="cb3ae-156">Cserélje le **kiszolgálónév**, **databasename**, **felhasználónév**, és **jelszó** az Azure SQL Server, SQL-adatbázis neve nevű felhasználó fiók és hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-156">Replace **servername**, **databasename**, **username**, and **password** with name of your Azure SQL server, name of SQL database, user account, and password for hello account.</span></span>  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

<span data-ttu-id="cb3ae-157">A JSON tulajdonságokról további részleteket tartalmaz az [Azure SQL társított szolgáltatás](data-factory-azure-sql-connector.md#linked-service-properties) című cikk.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-157">For details about JSON properties, see [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="cb3ae-158">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-158">inputdataset.json</span></span>

```JSON
{
  "name": "AzureBlobInput",
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
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
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

<span data-ttu-id="cb3ae-159">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-159">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="cb3ae-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb3ae-160">Property</span></span> | <span data-ttu-id="cb3ae-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb3ae-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cb3ae-162">type</span><span class="sxs-lookup"><span data-stu-id="cb3ae-162">type</span></span> | <span data-ttu-id="cb3ae-163">hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-163">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
| <span data-ttu-id="cb3ae-164">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cb3ae-164">linkedServiceName</span></span> | <span data-ttu-id="cb3ae-165">Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-165">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="cb3ae-166">folderPath</span><span class="sxs-lookup"><span data-stu-id="cb3ae-166">folderPath</span></span> | <span data-ttu-id="cb3ae-167">Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-167">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="cb3ae-168">Ebben az oktatóanyagban adftutorial hello blobtárolót és hello legfelső szintű mappa.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-168">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
| <span data-ttu-id="cb3ae-169">fileName</span><span class="sxs-lookup"><span data-stu-id="cb3ae-169">fileName</span></span> | <span data-ttu-id="cb3ae-170">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-170">This property is optional.</span></span> <span data-ttu-id="cb3ae-171">Ha kihagyja ezt a tulajdonságot, leltárhoz hello folderPath lévő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-171">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="cb3ae-172">Ebben az oktatóanyagban **emp.txt** hello fájlnév, hogy csak adott fájl van felvételre feldolgozásra számára megadott.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-172">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
| <span data-ttu-id="cb3ae-173">formátum -> típus</span><span class="sxs-lookup"><span data-stu-id="cb3ae-173">format -> type</span></span> |<span data-ttu-id="cb3ae-174">hello bemeneti fájl hello szöveg formátumban van, így használjuk **szöveges**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-174">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
| <span data-ttu-id="cb3ae-175">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="cb3ae-175">columnDelimiter</span></span> | <span data-ttu-id="cb3ae-176">hello bemeneti fájl hello oszlopai határolja **vesszővel karakter (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-176">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
| <span data-ttu-id="cb3ae-177">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="cb3ae-177">frequency/interval</span></span> | <span data-ttu-id="cb3ae-178">hello gyakoriságának beállítása túl**óra** és időköz értéke túl**1**, ami azt jelenti, hogy hello bemeneti szeletek érhetők el **óránkénti**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-178">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="cb3ae-179">Más szóval hello Data Factory szolgáltatásnak megkeresi a bemeneti adatok óránként blob tároló hello gyökérmappájában (**adftutorial**) megadott.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-179">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="cb3ae-180">Hello adatainak hello folyamat kezdési és befejezési időpontokat, nem előtt vagy után ezekben az időszakokban keresi.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-180">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
| <span data-ttu-id="cb3ae-181">external</span><span class="sxs-lookup"><span data-stu-id="cb3ae-181">external</span></span> | <span data-ttu-id="cb3ae-182">Ez a tulajdonság értéke túl**igaz** hello adatok nem jön létre, ez az adatcsatorna.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-182">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="cb3ae-183">Ebben az oktatóanyagban hello a bemeneti adatok nem jön létre a folyamat, így ez a tulajdonság tootrue hivatott hello emp.txt fájl van.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-183">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

<span data-ttu-id="cb3ae-184">Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-184">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>

### <a name="outputdatasetjson"></a><span data-ttu-id="cb3ae-185">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-185">outputdataset.json</span></span>

```JSON
{
  "name": "AzureSqlOutput",
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
<span data-ttu-id="cb3ae-186">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-186">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="cb3ae-187">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="cb3ae-187">Property</span></span> | <span data-ttu-id="cb3ae-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb3ae-188">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="cb3ae-189">type</span><span class="sxs-lookup"><span data-stu-id="cb3ae-189">type</span></span> | <span data-ttu-id="cb3ae-190">hello type tulajdonság beállítása túl**AzureSqlTable** mert adatok másolt tooa tábla Azure SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-190">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
| <span data-ttu-id="cb3ae-191">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="cb3ae-191">linkedServiceName</span></span> | <span data-ttu-id="cb3ae-192">Toohello hivatkozik **AzureSqlLinkedService** korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-192">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="cb3ae-193">tableName</span><span class="sxs-lookup"><span data-stu-id="cb3ae-193">tableName</span></span> | <span data-ttu-id="cb3ae-194">Megadott hello **tábla** toowhich hello adatok másolását.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-194">Specified hello **table** toowhich hello data is copied.</span></span> | 
| <span data-ttu-id="cb3ae-195">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="cb3ae-195">frequency/interval</span></span> | <span data-ttu-id="cb3ae-196">hello gyakoriságának beállítása túl**óra** és időköz **1**, ami azt jelenti, hogy hello kimeneti szeletek előállítása **óránkénti** közötti hello folyamat kezdési és befejezési időpontokat, előtte vagy Miután ezekben az időszakokban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-196">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

<span data-ttu-id="cb3ae-197">Három oszlop – **azonosító**, **Keresztnév**, és **Vezetéknév** – hello üres tábla hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-197">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="cb3ae-198">Azonosító: azonosító oszlopot, ezért meg kell, hogy csak toospecify **Keresztnév** és **Vezetéknév** itt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-198">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

<span data-ttu-id="cb3ae-199">További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-199">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="cb3ae-200">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="cb3ae-200">pipeline.json</span></span>

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
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

<span data-ttu-id="cb3ae-201">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-201">Note hello following points:</span></span>

- <span data-ttu-id="cb3ae-202">Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-202">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="cb3ae-203">Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-203">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="cb3ae-204">A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-204">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
- <span data-ttu-id="cb3ae-205">Adjon meg a hello tevékenység értéke túl**AzureBlobInput** és hello tevékenység túl van-e állítva a kimeneti**AzureSqlOutput**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-205">Input for hello activity is set too**AzureBlobInput** and output for hello activity is set too**AzureSqlOutput**.</span></span> 
- <span data-ttu-id="cb3ae-206">A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-206">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="cb3ae-207">Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-207">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="cb3ae-208">Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-208">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
 
<span data-ttu-id="cb3ae-209">Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-209">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="cb3ae-210">Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-210">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="cb3ae-211">Például "2017-02-03", amely túl egyenértékű "2017-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="cb3ae-211">For example, "2017-02-03", which is equivalent too"2017-02-03T00:00:00Z"</span></span>
 
<span data-ttu-id="cb3ae-212">Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-212">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="cb3ae-213">Például: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-213">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="cb3ae-214">Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-214">hello **end** time is optional, but we use it in this tutorial.</span></span> 
 
<span data-ttu-id="cb3ae-215">Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**".</span><span class="sxs-lookup"><span data-stu-id="cb3ae-215">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="cb3ae-216">toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-216">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
 
<span data-ttu-id="cb3ae-217">A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-217">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

<span data-ttu-id="cb3ae-218">A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-218">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="cb3ae-219">A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-219">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="cb3ae-220">A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-220">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="cb3ae-221">Az SqlSink által támogatott JSON-tulajdonságok leírása az [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md) című cikkben található.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-221">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="set-global-variables"></a><span data-ttu-id="cb3ae-222">Globális változók beállítása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-222">Set global variables</span></span>
<span data-ttu-id="cb3ae-223">Az Azure PowerShell-lel hajtható végre hello parancsok után hello értékeket a saját cserélje le a következő:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-223">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb3ae-224">Az ügyfél-azonosító, a titkos ügyfélkód, a bérlőazonosító és az előfizetés-azonosító beszerzésével kapcsolatban olvassa el az [Előfeltételek](#prerequisites) című fejezetet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-224">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

<span data-ttu-id="cb3ae-225">Parancs hello adat-előállító nevét hello frissítése után a következő futtatási hello használata esetén:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-225">Run hello following command after updating hello name of hello data factory you are using:</span></span> 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a><span data-ttu-id="cb3ae-226">Hitelesítés az AAD segítségével</span><span class="sxs-lookup"><span data-stu-id="cb3ae-226">Authenticate with AAD</span></span>
<span data-ttu-id="cb3ae-227">Futtassa a következő parancs tooauthenticate az Azure Active Directory (AAD) hello:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-227">Run hello following command tooauthenticate with Azure Active Directory (AAD):</span></span> 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a><span data-ttu-id="cb3ae-228">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-228">Create data factory</span></span>
<span data-ttu-id="cb3ae-229">Ebben a lépésben egy **ADFCopyTutorialDF** nevű Azure-adatelőállítót fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-229">In this step, you create an Azure Data Factory named **ADFCopyTutorialDF**.</span></span> <span data-ttu-id="cb3ae-230">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-230">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="cb3ae-231">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-231">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="cb3ae-232">Például egy tooa cél forrásadatok másolási tevékenység toocopy adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-232">For example, a Copy Activity toocopy data from a source tooa destination data store.</span></span> <span data-ttu-id="cb3ae-233">A HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform bemeneti adatok tooproduct kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-233">A HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="cb3ae-234">Futtassa a következő parancsok toocreate hello adat-előállító hello:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-234">Run hello following commands toocreate hello data factory:</span></span> 

1. <span data-ttu-id="cb3ae-235">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-235">Assign hello command toovariable named **cmd**.</span></span> 
   
    > [!IMPORTANT]
    > <span data-ttu-id="cb3ae-236">Győződjön meg arról, hogy az itt megadott (ADFCopyTutorialDF) egyező hello hello megadott név hello adat-előállító hello neve **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-236">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-237">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-237">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-238">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-238">View hello results.</span></span> <span data-ttu-id="cb3ae-239">Ha hello adat-előállító létrehozása sikeresen befejeződött, megjelenik az hello data factory hello a JSON hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-239">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>  
   
    ```
    Write-Host $results
    ```

<span data-ttu-id="cb3ae-240">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-240">Note hello following points:</span></span>

* <span data-ttu-id="cb3ae-241">Azure Data Factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-241">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="cb3ae-242">Ha az eredmények hello hibát látja: **nem érhető el adat-előállító "ADFCopyTutorialDF"**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-242">If you see hello error in results: **Data factory name “ADFCopyTutorialDF” is not available**, do hello following steps:</span></span>  
  
  1. <span data-ttu-id="cb3ae-243">Hello nevének módosítása (például yournameADFCopyTutorialDF) a hello **datafactory.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-243">Change hello name (for example, yournameADFCopyTutorialDF) in hello **datafactory.json** file.</span></span>
  2. <span data-ttu-id="cb3ae-244">Az első parancs hello ahol hello **$cmd** változó értéket kapja, cserélje le a ADFCopyTutorialDF hello új névvel és hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-244">In hello first command where hello **$cmd** variable is assigned a value, replace ADFCopyTutorialDF with hello new name and run hello command.</span></span> 
  3. <span data-ttu-id="cb3ae-245">Futtassa a hello következő két parancsok tooinvoke hello REST API toocreate hello data factory és nyomtatás hello hello művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-245">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span> 
     
     <span data-ttu-id="cb3ae-246">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-246">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="cb3ae-247">toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda</span><span class="sxs-lookup"><span data-stu-id="cb3ae-247">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="cb3ae-248">hello adat-előállító nevét hello előfordulhat, hogy a jövőbeli hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-248">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="cb3ae-249">Ha hello hibaüzenet: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**", hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-249">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="cb3ae-250">Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-250">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="cb3ae-251">A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-251">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="cb3ae-252">Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-252">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="cb3ae-253">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-253">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="cb3ae-254">Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-254">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="cb3ae-255">Először létre kell hoznia összekapcsolt szolgáltatások toolink forrás és cél adatokat tárolja tooyour adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-255">You first create linked services toolink source and destination data stores tooyour data store.</span></span> <span data-ttu-id="cb3ae-256">Ezután határozzon meg a bemeneti és kimeneti adatkészletek toorepresent adatok csatolt adatok tárolja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-256">Then, define input and output datasets toorepresent data in linked data stores.</span></span> <span data-ttu-id="cb3ae-257">Végezetül hozza létre egy tevékenységet, ezek az adatkészletek használó hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-257">Finally, create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="cb3ae-258">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-258">Create linked services</span></span>
<span data-ttu-id="cb3ae-259">A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-259">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="cb3ae-260">Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-260">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="cb3ae-261">Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-261">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> <span data-ttu-id="cb3ae-262">Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-262">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="cb3ae-263">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-263">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="cb3ae-264">Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-264">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="cb3ae-265">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-265">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="cb3ae-266">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-266">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="cb3ae-267">Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-267">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="cb3ae-268">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-268">Create Azure Storage linked service</span></span>
<span data-ttu-id="cb3ae-269">Ebben a lépésben a az Azure storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-269">In this step, you link your Azure storage account tooyour data factory.</span></span> <span data-ttu-id="cb3ae-270">Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-270">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="cb3ae-271">Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-271">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>  

1. <span data-ttu-id="cb3ae-272">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-272">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-273">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-273">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-274">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-274">View hello results.</span></span> <span data-ttu-id="cb3ae-275">Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-275">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a><span data-ttu-id="cb3ae-276">Azure SQL társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-276">Create Azure SQL linked service</span></span>
<span data-ttu-id="cb3ae-277">Ebben a lépésben az Azure SQL adatbázis tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-277">In this step, you link your Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="cb3ae-278">Ebben a szakaszban megadhatja hello Azure SQL-kiszolgáló neve, az adatbázis nevét, a felhasználónév és a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-278">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="cb3ae-279">Lásd: [Azure SQL társított szolgáltatásnak](data-factory-azure-sql-connector.md#linked-service-properties) JSON használt tulajdonságok toodefine vonatkozó további információért az Azure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-279">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>

1. <span data-ttu-id="cb3ae-280">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-280">Assign hello command toovariable named **cmd**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-281">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-281">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-282">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-282">View hello results.</span></span> <span data-ttu-id="cb3ae-283">Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-283">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="cb3ae-284">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-284">Create datasets</span></span>
<span data-ttu-id="cb3ae-285">Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-285">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="cb3ae-286">Ebben a lépésben AzureBlobInput és AzureSqlOutput, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-286">In this step, you define two datasets named AzureBlobInput and AzureSqlOutput that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="cb3ae-287">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-287">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="cb3ae-288">És hello bemeneti blob-adathalmazra (AzureBlobInput) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-288">And, hello input blob dataset (AzureBlobInput) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="cb3ae-289">Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-289">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="cb3ae-290">És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-290">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="cb3ae-291">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-291">Create input dataset</span></span>
<span data-ttu-id="cb3ae-292">Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) AzureBlobInput nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-292">In this step, you create a dataset named AzureBlobInput that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="cb3ae-293">Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-293">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="cb3ae-294">Ebben az oktatóanyagban hello fájlnév értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-294">In this tutorial, you specify a value for hello fileName.</span></span> 

1. <span data-ttu-id="cb3ae-295">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-295">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-296">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-296">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-297">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-297">View hello results.</span></span> <span data-ttu-id="cb3ae-298">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-298">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="cb3ae-299">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-299">Create output dataset</span></span>
<span data-ttu-id="cb3ae-300">hello csatolt Azure SQL Database szolgáltatás hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-300">hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="cb3ae-301">hello kimeneti SQL táblázat dataset (OututDataset) hoz létre ebben a lépésben meghatározza hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-301">hello output SQL table dataset (OututDataset) you create in this step specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

1. <span data-ttu-id="cb3ae-302">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-302">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-303">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-303">Run hello command by using **Invoke-Command**.</span></span>
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-304">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-304">View hello results.</span></span> <span data-ttu-id="cb3ae-305">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-305">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a><span data-ttu-id="cb3ae-306">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="cb3ae-306">Create pipeline</span></span>
<span data-ttu-id="cb3ae-307">Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **AzureBlobInput**, kimenetként az **AzureSqlOutput** adatkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-307">In this step, you create a pipeline with a **copy activity** that uses **AzureBlobInput** as an input and **AzureSqlOutput** as an output.</span></span>

<span data-ttu-id="cb3ae-308">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-308">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="cb3ae-309">Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-309">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="cb3ae-310">hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-310">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="cb3ae-311">Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-311">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="cb3ae-312">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-312">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="cb3ae-313">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-313">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="cb3ae-314">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-314">View hello results.</span></span> <span data-ttu-id="cb3ae-315">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-315">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>  

    ```PowerShell   
    Write-Host $results
    ```

<span data-ttu-id="cb3ae-316">**Gratulálunk!**</span><span class="sxs-lookup"><span data-stu-id="cb3ae-316">**Congratulations!**</span></span> <span data-ttu-id="cb3ae-317">Sikeresen létrehozott egy Azure data factory a egy folyamatot, amely másolja az adatokat az Azure Blob Storage tooAzure SQL-adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-317">You have successfully created an Azure data factory, with a pipeline that copies data from Azure Blob Storage tooAzure SQL database.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="cb3ae-318">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="cb3ae-318">Monitor pipeline</span></span>
<span data-ttu-id="cb3ae-319">Ebben a lépésben a Data Factory REST API toomonitor szeletek hello folyamat alatt előállított funkcióval.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-319">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> <span data-ttu-id="cb3ae-320">Ellenőrizze, hogy hello kezdési és befejezési időpontokat a következő megadott hello hello indítása parancsot, és befejezési időpontja hello folyamatának.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-320">Make sure that hello start and end times specified in hello following command match hello start and end times of hello pipeline.</span></span> 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

<span data-ttu-id="cb3ae-321">Végezzen hello Invoke-Command és hello egy látja, hogy a szelet **készen** állapota vagy **sikertelen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-321">Run hello Invoke-Command and hello next one until you see a slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="cb3ae-322">Ha hello szelet készen állapotban van, ellenőrizze a hello **üres** hello kimeneti adatok az Azure SQL-adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-322">When hello slice is in Ready state, check hello **emp** table in your Azure SQL database for hello output data.</span></span> 

<span data-ttu-id="cb3ae-323">Minden szelet két sornyi adatot hello forrásfájlból másolt toohello üres táblázat hello Azure SQL adatbázis esetén.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-323">For each slice, two rows of data from hello source file are copied toohello emp table in hello Azure SQL database.</span></span> <span data-ttu-id="cb3ae-324">Ezért akkor látni 24 új rekordok hello üres tábla összes hello szeletek sikeresen feldolgozott (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="cb3ae-324">Therefore, you see 24 new records in hello emp table when all hello slices are successfully processed (in Ready state).</span></span> 

## <a name="summary"></a><span data-ttu-id="cb3ae-325">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cb3ae-325">Summary</span></span>
<span data-ttu-id="cb3ae-326">Ebben az oktatóanyagban használt REST API toocreate egy Azure blob tooan Azure SQL-adatbázis az Azure data factory toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-326">In this tutorial, you used REST API toocreate an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="cb3ae-327">Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-327">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="cb3ae-328">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-328">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="cb3ae-329">**Társított szolgáltatásokat** hozott létre:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-329">Created **linked services**:</span></span>
   1. <span data-ttu-id="cb3ae-330">Egy Azure Storage társított szolgáltatás toolink az Azure Storage-fiók, amely a bemeneti adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-330">An Azure Storage linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="cb3ae-331">Egy Azure SQL társított szolgáltatás toolink az Azure SQL-adatbázis, amely tárolja a hello kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-331">An Azure SQL linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="cb3ae-332">**Adatkészleteket** hozott létre, amelyek az adatcsatorna bemeneti és kimeneti adatait írják le.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-332">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="cb3ae-333">Létrehozott egy másolási tevékenységgel ellátott **adatcsatornát**, ahol a BlobSource a forrás, az SqlSink pedig a fogadó.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-333">Created a **pipeline** with a Copy Activity with BlobSource as source and SqlSink as sink.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cb3ae-334">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb3ae-334">Next steps</span></span>
<span data-ttu-id="cb3ae-335">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-335">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="cb3ae-336">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="cb3ae-336">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="cb3ae-337">Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.</span><span class="sxs-lookup"><span data-stu-id="cb3ae-337">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
