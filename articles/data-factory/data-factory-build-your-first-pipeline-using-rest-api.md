---
title: "aaaBuild az első adat-előállítóban (REST) |} Microsoft Docs"
description: "Az oktatóanyag során egy minta Azure Data Factory-adatcsatornát fogunk létrehozni a Data Factory REST API-val."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a><span data-ttu-id="00f47-103">Oktatóanyag: Az első data factory létrehozása a Data Factory REST API használatával</span><span class="sxs-lookup"><span data-stu-id="00f47-103">Tutorial: Build your first Azure data factory using Data Factory REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="00f47-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00f47-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="00f47-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00f47-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="00f47-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="00f47-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="00f47-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="00f47-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="00f47-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="00f47-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="00f47-109">REST API</span><span class="sxs-lookup"><span data-stu-id="00f47-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


<span data-ttu-id="00f47-110">Ebben a cikkben az első az Azure data factory Data Factory REST API toocreate meg használja.</span><span class="sxs-lookup"><span data-stu-id="00f47-110">In this article, you use Data Factory REST API toocreate your first Azure data factory.</span></span> <span data-ttu-id="00f47-111">toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="00f47-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="00f47-112">Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="00f47-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="00f47-113">Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="00f47-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="00f47-114">hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.</span><span class="sxs-lookup"><span data-stu-id="00f47-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span>

> [!NOTE]
> <span data-ttu-id="00f47-115">Ez a cikk nem foglalkozik az összes hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="00f47-115">This article does not cover all hello REST API.</span></span> <span data-ttu-id="00f47-116">A REST API átfogó dokumentációjáért lásd: [A Data Factory REST API-jának leírása](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="00f47-116">For comprehensive documentation on REST API, see [Data Factory REST API Reference](/rest/api/datafactory/).</span></span>
> 
> <span data-ttu-id="00f47-117">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="00f47-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="00f47-118">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="00f47-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="00f47-119">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="00f47-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="00f47-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00f47-120">Prerequisites</span></span>
* <span data-ttu-id="00f47-121">Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="00f47-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="00f47-122">Telepítse gépére a [Curl](https://curl.haxx.se/dlwiz/) eszközt.</span><span class="sxs-lookup"><span data-stu-id="00f47-122">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="00f47-123">A további parancsok toocreate egy adat-előállító hello CURL eszközt kell használni.</span><span class="sxs-lookup"><span data-stu-id="00f47-123">You use hello CURL tool with REST commands toocreate a data factory.</span></span>
* <span data-ttu-id="00f47-124">Az [ebben a cikkben](../azure-resource-manager/resource-group-create-service-principal-portal.md) szereplő utasításokat követve végezze el a következőket:</span><span class="sxs-lookup"><span data-stu-id="00f47-124">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span>
  1. <span data-ttu-id="00f47-125">Hozzon létre egy **ADFGetStartedApp** nevű webalkalmazást az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="00f47-125">Create a Web application named **ADFGetStartedApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="00f47-126">Szerezze be az **ügyfél-azonosítót** és a **titkos kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="00f47-126">Get **client ID** and **secret key**.</span></span>
  3. <span data-ttu-id="00f47-127">Szerezze be a **bérlőazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="00f47-127">Get **tenant ID**.</span></span>
  4. <span data-ttu-id="00f47-128">Rendelje hozzá a hello **ADFGetStartedApp** alkalmazás toohello **Data Factory közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="00f47-128">Assign hello **ADFGetStartedApp** application toohello **Data Factory Contributor** role.</span></span>
* <span data-ttu-id="00f47-129">Telepítse az [Azure PowerShellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="00f47-129">Install [Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="00f47-130">Indítsa el **PowerShell** és futtatási hello a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="00f47-130">Launch **PowerShell** and run hello following command.</span></span> <span data-ttu-id="00f47-131">Azure PowerShell nyitva hagyja az oktatóanyag hello végéig.</span><span class="sxs-lookup"><span data-stu-id="00f47-131">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="00f47-132">Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="00f47-132">If you close and reopen, you need toorun hello commands again.</span></span>
  1. <span data-ttu-id="00f47-133">Futtatás **Login-AzureRmAccount** , és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="00f47-133">Run **Login-AzureRmAccount** and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
  2. <span data-ttu-id="00f47-134">Futtatás **Get-AzureRmSubscription** tooview összes hello előfizetések ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="00f47-134">Run **Get-AzureRmSubscription** tooview all hello subscriptions for this account.</span></span>
  3. <span data-ttu-id="00f47-135">Futtatás **Get-AzureRmSubscription – SubscriptionName NameOfAzureSubscription |} Set-AzureRmContext** ki a toowork tooselect hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="00f47-135">Run **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="00f47-136">Cserélje le **NameOfAzureSubscription** hello nevet, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="00f47-136">Replace **NameOfAzureSubscription** with hello name of your Azure subscription.</span></span>
* <span data-ttu-id="00f47-137">Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello hello PowerShell parancsot a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="00f47-137">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   <span data-ttu-id="00f47-138">Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát.</span><span class="sxs-lookup"><span data-stu-id="00f47-138">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="00f47-139">Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="00f47-139">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="00f47-140">JSON-definíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-140">Create JSON definitions</span></span>
<span data-ttu-id="00f47-141">Következő hello mappáját curl.exe a JSON-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-141">Create following JSON files in hello folder where curl.exe is located.</span></span>

### <a name="datafactoryjson"></a><span data-ttu-id="00f47-142">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="00f47-142">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00f47-143">Egyedinek kell lennie globálisan, ezért érdemes tooprefix/utótag ADFCopyTutorialDF toomake azt egy egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="00f47-143">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span>
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="00f47-144">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="00f47-144">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00f47-145">Az **accountname** és az **accountkey** kifejezés helyére írja be Azure Storage-tárfiókja nevére, illetve kulcsát.</span><span class="sxs-lookup"><span data-stu-id="00f47-145">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="00f47-146">toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="00f47-146">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
>
>

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

### <a name="hdinsightondemandlinkedservicejson"></a><span data-ttu-id="00f47-147">hdinsightondemandlinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="00f47-147">hdinsightondemandlinkedservice.json</span></span>

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

<span data-ttu-id="00f47-148">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="00f47-148">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="00f47-149">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="00f47-149">Property</span></span> | <span data-ttu-id="00f47-150">Leírás</span><span class="sxs-lookup"><span data-stu-id="00f47-150">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="00f47-151">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="00f47-151">ClusterSize</span></span> |<span data-ttu-id="00f47-152">Hello HDInsight-fürt méretét.</span><span class="sxs-lookup"><span data-stu-id="00f47-152">Size of hello HDInsight cluster.</span></span> |
| <span data-ttu-id="00f47-153">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="00f47-153">TimeToLive</span></span> |<span data-ttu-id="00f47-154">Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt.</span><span class="sxs-lookup"><span data-stu-id="00f47-154">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
| <span data-ttu-id="00f47-155">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="00f47-155">linkedServiceName</span></span> |<span data-ttu-id="00f47-156">Adja meg, amely a HDInsight által létrehozott használt toostore hello naplók hello storage-fiók</span><span class="sxs-lookup"><span data-stu-id="00f47-156">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

<span data-ttu-id="00f47-157">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="00f47-157">Note hello following points:</span></span>

* <span data-ttu-id="00f47-158">hello adat-előállító létrehoz egy **Linux-alapú** a fenti JSON hello meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="00f47-158">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="00f47-159">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="00f47-159">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
* <span data-ttu-id="00f47-160">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="00f47-160">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="00f47-161">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="00f47-161">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="00f47-162">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="00f47-162">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="00f47-163">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="00f47-163">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="00f47-164">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="00f47-164">This behavior is by design.</span></span> <span data-ttu-id="00f47-165">Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet feldolgozása, kivéve, ha egy meglévő élő fürthöz (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="00f47-165">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

    <span data-ttu-id="00f47-166">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="00f47-166">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="00f47-167">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="00f47-167">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="00f47-168">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp".</span><span class="sxs-lookup"><span data-stu-id="00f47-168">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="00f47-169">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="00f47-169">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="00f47-170">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="00f47-170">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="00f47-171">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="00f47-171">inputdataset.json</span></span>

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

<span data-ttu-id="00f47-172">hello JSON meghatározása nevű adatkészlete **AzureBlobInput**, amely jelenti, hogy egy tevékenység hello folyamat a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="00f47-172">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="00f47-173">Emellett meghatározza, hogy hello bemeneti adatok nevű hello blob tárolóban található **adfgetstarted** és nevű hello mappát **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="00f47-173">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

<span data-ttu-id="00f47-174">hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="00f47-174">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="00f47-175">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="00f47-175">Property</span></span> | <span data-ttu-id="00f47-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="00f47-176">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="00f47-177">type</span><span class="sxs-lookup"><span data-stu-id="00f47-177">type</span></span> |<span data-ttu-id="00f47-178">mivel az adatok találhatók az Azure blob storage hello type tulajdonság beállítása tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="00f47-178">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
| <span data-ttu-id="00f47-179">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="00f47-179">linkedServiceName</span></span> |<span data-ttu-id="00f47-180">a korábban létrehozott StorageLinkedService toohello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="00f47-180">refers toohello StorageLinkedService you created earlier.</span></span> |
| <span data-ttu-id="00f47-181">fileName</span><span class="sxs-lookup"><span data-stu-id="00f47-181">fileName</span></span> |<span data-ttu-id="00f47-182">Ez a tulajdonság nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="00f47-182">This property is optional.</span></span> <span data-ttu-id="00f47-183">Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz.</span><span class="sxs-lookup"><span data-stu-id="00f47-183">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="00f47-184">Ebben az esetben csak hello input.log dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="00f47-184">In this case, only hello input.log is processed.</span></span> |
| <span data-ttu-id="00f47-185">type</span><span class="sxs-lookup"><span data-stu-id="00f47-185">type</span></span> |<span data-ttu-id="00f47-186">hello naplófájlok szöveges formátumú, így szöveges használjuk.</span><span class="sxs-lookup"><span data-stu-id="00f47-186">hello log files are in text format, so we use TextFormat.</span></span> |
| <span data-ttu-id="00f47-187">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="00f47-187">columnDelimiter</span></span> |<span data-ttu-id="00f47-188">oszlopok hello naplófájlokban határolja vesszővel (,) karakter</span><span class="sxs-lookup"><span data-stu-id="00f47-188">columns in hello log files are delimited by a comma character (,)</span></span> |
| <span data-ttu-id="00f47-189">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="00f47-189">frequency/interval</span></span> |<span data-ttu-id="00f47-190">gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi.</span><span class="sxs-lookup"><span data-stu-id="00f47-190">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
| <span data-ttu-id="00f47-191">external</span><span class="sxs-lookup"><span data-stu-id="00f47-191">external</span></span> |<span data-ttu-id="00f47-192">Ez a tulajdonság beállítása tootrue hello bemeneti adatok nem generálja hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="00f47-192">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |

### <a name="outputdatasetjson"></a><span data-ttu-id="00f47-193">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="00f47-193">outputdataset.json</span></span>

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

<span data-ttu-id="00f47-194">hello JSON meghatározása nevű adatkészlete **AzureBlobOutput**, amely jelenti, hogy egy tevékenység hello folyamat kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="00f47-194">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="00f47-195">Emellett meghatározza, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfgetstarted** és nevű hello mappát **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="00f47-195">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="00f47-196">Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.</span><span class="sxs-lookup"><span data-stu-id="00f47-196">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="00f47-197">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="00f47-197">pipeline.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="00f47-198">Cserélje a **storageaccountname** kifejezést Azure Storage-fiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="00f47-198">Replace **storageaccountname** with name of your Azure storage account.</span></span>
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="00f47-199">Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatok egy HDInsight-fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="00f47-199">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess data on a HDInsight cluster.</span></span>

<span data-ttu-id="00f47-200">hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **StorageLinkedService**), majd a **parancsfájl**  hello tároló mappa **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="00f47-200">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

<span data-ttu-id="00f47-201">Hello **meghatározása** szakasz Hive értékként toohello hive parancsfájl átadott futásidejű beállításokat határoz meg, (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).</span><span class="sxs-lookup"><span data-stu-id="00f47-201">hello **defines** section specifies runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

<span data-ttu-id="00f47-202">Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-202">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

<span data-ttu-id="00f47-203">Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="00f47-203">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

> [!NOTE]
> <span data-ttu-id="00f47-204">"Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) használt a fenti példa hello JSON-tulajdonságok vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="00f47-204">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello preceding example.</span></span>
>
>

## <a name="set-global-variables"></a><span data-ttu-id="00f47-205">Globális változók beállítása</span><span class="sxs-lookup"><span data-stu-id="00f47-205">Set global variables</span></span>
<span data-ttu-id="00f47-206">Az Azure PowerShell-lel hajtható végre hello parancsok után hello értékeket a saját cserélje le a következő:</span><span class="sxs-lookup"><span data-stu-id="00f47-206">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00f47-207">Az ügyfél-azonosító, a titkos ügyfélkód, a bérlőazonosító és az előfizetés-azonosító beszerzésével kapcsolatban olvassa el az [Előfeltételek](#prerequisites) című fejezetet.</span><span class="sxs-lookup"><span data-stu-id="00f47-207">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a><span data-ttu-id="00f47-208">Hitelesítés az AAD segítségével</span><span class="sxs-lookup"><span data-stu-id="00f47-208">Authenticate with AAD</span></span>

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a><span data-ttu-id="00f47-209">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-209">Create data factory</span></span>
<span data-ttu-id="00f47-210">Ebben a lépésben egy **FirstDataFactoryREST** nevű Azure-adatelőállítót fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="00f47-210">In this step, you create an Azure Data Factory named **FirstDataFactoryREST**.</span></span> <span data-ttu-id="00f47-211">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="00f47-211">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="00f47-212">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="00f47-212">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="00f47-213">Például a másolási tevékenység toocopy adatait a forrás tooa cél tárolóban és a HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat.</span><span class="sxs-lookup"><span data-stu-id="00f47-213">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform data.</span></span> <span data-ttu-id="00f47-214">Futtassa a következő parancsok toocreate hello adat-előállító hello:</span><span class="sxs-lookup"><span data-stu-id="00f47-214">Run hello following commands toocreate hello data factory:</span></span>

1. <span data-ttu-id="00f47-215">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-215">Assign hello command toovariable named **cmd**.</span></span>

    <span data-ttu-id="00f47-216">Győződjön meg arról, hogy az itt megadott (ADFCopyTutorialDF) egyező hello hello megadott név hello adat-előállító hello neve **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="00f47-216">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-217">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-217">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-218">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-218">View hello results.</span></span> <span data-ttu-id="00f47-219">Ha hello adat-előállító létrehozása sikeresen befejeződött, megjelenik az hello data factory hello a JSON hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-219">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

<span data-ttu-id="00f47-220">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="00f47-220">Note hello following points:</span></span>

* <span data-ttu-id="00f47-221">Azure Data Factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="00f47-221">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="00f47-222">Ha az eredmények hello hibát látja: **nem érhető el adat-előállító "FirstDataFactoryREST"**, hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="00f47-222">If you see hello error in results: **Data factory name “FirstDataFactoryREST” is not available**, do hello following steps:</span></span>
  1. <span data-ttu-id="00f47-223">Hello nevének módosítása (például yournameFirstDataFactoryREST) a hello **datafactory.json** fájlt.</span><span class="sxs-lookup"><span data-stu-id="00f47-223">Change hello name (for example, yournameFirstDataFactoryREST) in hello **datafactory.json** file.</span></span> <span data-ttu-id="00f47-224">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="00f47-224">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
  2. <span data-ttu-id="00f47-225">Az első parancs hello ahol hello **$cmd** változó értéket kapja, cserélje le a FirstDataFactoryREST hello új névvel és hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="00f47-225">In hello first command where hello **$cmd** variable is assigned a value, replace FirstDataFactoryREST with hello new name and run hello command.</span></span>
  3. <span data-ttu-id="00f47-226">Futtassa a hello következő két parancsok tooinvoke hello REST API toocreate hello data factory és nyomtatás hello hello művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="00f47-226">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span>
* <span data-ttu-id="00f47-227">toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda</span><span class="sxs-lookup"><span data-stu-id="00f47-227">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="00f47-228">hello adat-előállító nevét hello előfordulhat, hogy a jövőbeli hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.</span><span class="sxs-lookup"><span data-stu-id="00f47-228">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="00f47-229">Ha hello hibaüzenet: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**", hello alábbi, és próbálja meg újra közzétenni:</span><span class="sxs-lookup"><span data-stu-id="00f47-229">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="00f47-230">Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:</span><span class="sxs-lookup"><span data-stu-id="00f47-230">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

      <span data-ttu-id="00f47-231">A következő parancs tooconfirm hello adott hello szolgáltató regisztrálva van a Data Factory futtathatja:</span><span class="sxs-lookup"><span data-stu-id="00f47-231">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="00f47-232">Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="00f47-232">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="00f47-233">Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="00f47-233">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="00f47-234">Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először.</span><span class="sxs-lookup"><span data-stu-id="00f47-234">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="00f47-235">Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolásához, adja meg a bemeneti és kimeneti adatkészletek toorepresent adatai a csatolt adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="00f47-235">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent data in linked data stores.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="00f47-236">Társított szolgáltatások létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-236">Create linked services</span></span>
<span data-ttu-id="00f47-237">Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="00f47-237">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="00f47-238">hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="00f47-238">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="00f47-239">HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="00f47-239">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="00f47-240">Azure Storage társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-240">Create Azure Storage linked service</span></span>
<span data-ttu-id="00f47-241">Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="00f47-241">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="00f47-242">Ebben az oktatóanyagban a hello használhatja ugyanazon Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="00f47-242">With this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="00f47-243">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-243">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-244">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-244">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-245">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-245">View hello results.</span></span> <span data-ttu-id="00f47-246">Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-246">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="00f47-247">Azure HDInsight társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-247">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="00f47-248">Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="00f47-248">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="00f47-249">az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="00f47-249">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="00f47-250">Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat.</span><span class="sxs-lookup"><span data-stu-id="00f47-250">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="00f47-251">További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="00f47-251">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="00f47-252">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-252">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-253">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-253">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-254">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-254">View hello results.</span></span> <span data-ttu-id="00f47-255">Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-255">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="00f47-256">Adatkészletek létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-256">Create datasets</span></span>
<span data-ttu-id="00f47-257">Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="00f47-257">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="00f47-258">Ezek az adatkészletek tekintse meg a toohello **StorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott.</span><span class="sxs-lookup"><span data-stu-id="00f47-258">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="00f47-259">hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="00f47-259">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="00f47-260">Bemeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-260">Create input dataset</span></span>
<span data-ttu-id="00f47-261">Ebben a lépésben hello bemeneti adatkészlet toorepresent bemeneti adatok hello Azure Blob storage-ban tárolt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00f47-261">In this step, you create hello input dataset toorepresent input data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="00f47-262">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-262">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-263">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-263">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-264">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-264">View hello results.</span></span> <span data-ttu-id="00f47-265">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-265">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="00f47-266">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-266">Create output dataset</span></span>
<span data-ttu-id="00f47-267">Ebben a lépésben hello kimeneti adatkészlet toorepresent kimeneti tárolt adatok hello Azure Blob Storage tárolóban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00f47-267">In this step, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="00f47-268">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-268">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-269">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-269">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-270">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-270">View hello results.</span></span> <span data-ttu-id="00f47-271">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-271">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a><span data-ttu-id="00f47-272">Folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="00f47-272">Create pipeline</span></span>
<span data-ttu-id="00f47-273">Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="00f47-273">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="00f47-274">Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly.</span><span class="sxs-lookup"><span data-stu-id="00f47-274">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="00f47-275">hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="00f47-275">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="00f47-276">Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="00f47-276">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="00f47-277">Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="00f47-277">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span>

<span data-ttu-id="00f47-278">Ellenőrizze, hogy látható-e hello **input.log** hello fájlban **adfgetstarted/inputdata** hello Azure blob Storage tárolóban, és futtassa a következő parancs toodeploy hello csővezeték hello mappájában.</span><span class="sxs-lookup"><span data-stu-id="00f47-278">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="00f47-279">Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.</span><span class="sxs-lookup"><span data-stu-id="00f47-279">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

1. <span data-ttu-id="00f47-280">Rendelje hozzá a hello parancs toovariable nevű **cmd**.</span><span class="sxs-lookup"><span data-stu-id="00f47-280">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="00f47-281">Hello parancs futtatásával **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="00f47-281">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="00f47-282">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="00f47-282">View hello results.</span></span> <span data-ttu-id="00f47-283">Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="00f47-283">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```
4. <span data-ttu-id="00f47-284">Gratulálunk, sikeresen létrehozta első folyamatát az Azure PowerShell használatával!</span><span class="sxs-lookup"><span data-stu-id="00f47-284">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="00f47-285">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="00f47-285">Monitor pipeline</span></span>
<span data-ttu-id="00f47-286">Ebben a lépésben a Data Factory REST API toomonitor szeletek hello folyamat alatt előállított funkcióval.</span><span class="sxs-lookup"><span data-stu-id="00f47-286">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> <span data-ttu-id="00f47-287">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="00f47-287">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="00f47-288">Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="00f47-288">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
>

<span data-ttu-id="00f47-289">Futtatás hello Invoke-Command és hello egy amíg megjelenik a hello szelet **készen** állapota vagy **sikertelen** állapota.</span><span class="sxs-lookup"><span data-stu-id="00f47-289">Run hello Invoke-Command and hello next one until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="00f47-290">Ha hello szelet készen állapotban van, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="00f47-290">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="00f47-291">igény szerinti HDInsight-fürtök létrehozása hello általában némi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="00f47-291">hello creation of an on-demand HDInsight cluster usually takes some time.</span></span>

![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="00f47-293">hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="00f47-293">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="00f47-294">Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.</span><span class="sxs-lookup"><span data-stu-id="00f47-294">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

<span data-ttu-id="00f47-295">Használja az Azure portál toomonitor szeletek is, és hárítsa el.</span><span class="sxs-lookup"><span data-stu-id="00f47-295">You can also use Azure portal toomonitor slices and troubleshoot any issues.</span></span> <span data-ttu-id="00f47-296">További információk: [Monitor pipelines using Azure portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) (Adatcsatornák figyelése az Azure Portal használatával).</span><span class="sxs-lookup"><span data-stu-id="00f47-296">See [Monitor pipelines using Azure portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) details.</span></span>

## <a name="summary"></a><span data-ttu-id="00f47-297">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="00f47-297">Summary</span></span>
<span data-ttu-id="00f47-298">Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="00f47-298">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="00f47-299">A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:</span><span class="sxs-lookup"><span data-stu-id="00f47-299">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="00f47-300">Létrehozott egy Azure **data factoryt**.</span><span class="sxs-lookup"><span data-stu-id="00f47-300">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="00f47-301">Létrehozott két **társított szolgáltatást**:</span><span class="sxs-lookup"><span data-stu-id="00f47-301">Created two **linked services**:</span></span>
   1. <span data-ttu-id="00f47-302">**Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.</span><span class="sxs-lookup"><span data-stu-id="00f47-302">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="00f47-303">**Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="00f47-303">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="00f47-304">Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="00f47-304">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="00f47-305">Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="00f47-305">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="00f47-306">Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="00f47-306">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="00f47-307">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00f47-307">Next steps</span></span>
<span data-ttu-id="00f47-308">Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti Azure HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="00f47-308">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="00f47-309">Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="00f47-309">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="00f47-310">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="00f47-310">See Also</span></span>
| <span data-ttu-id="00f47-311">Témakör</span><span class="sxs-lookup"><span data-stu-id="00f47-311">Topic</span></span> | <span data-ttu-id="00f47-312">Leírás</span><span class="sxs-lookup"><span data-stu-id="00f47-312">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="00f47-313">Data Factory REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="00f47-313">Data Factory REST API Reference</span></span>](/rest/api/datafactory/) |<span data-ttu-id="00f47-314">A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentáció.</span><span class="sxs-lookup"><span data-stu-id="00f47-314">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="00f47-315">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="00f47-315">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="00f47-316">Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket.</span><span class="sxs-lookup"><span data-stu-id="00f47-316">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="00f47-317">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="00f47-317">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="00f47-318">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="00f47-318">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="00f47-319">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="00f47-319">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="00f47-320">Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait.</span><span class="sxs-lookup"><span data-stu-id="00f47-320">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="00f47-321">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="00f47-321">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="00f47-322">Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="00f47-322">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
