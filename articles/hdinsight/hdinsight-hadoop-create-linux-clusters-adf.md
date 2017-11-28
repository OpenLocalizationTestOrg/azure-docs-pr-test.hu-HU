---
title: "igény szerinti Hadoop-fürtök használata a Data Factory - Azure HDInsight aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate igény szerinti Hadoop-fürtök a HDInsight az Azure Data Factory használatával."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="f6e28-103">Igény szerinti Hadoop-fürtök létrehozása a Hdinsightban Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="f6e28-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="f6e28-104">[Az Azure Data Factory](../data-factory/data-factory-introduction.md) egy felhőalapú integrációs szolgáltatás koordinálja és hello mozgás és adatok átalakítása automatizálja.</span><span class="sxs-lookup"><span data-stu-id="f6e28-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="f6e28-105">Hozzon létre egy HDInsight Hadoop fürthöz just-in-time tooprocess egy bemeneti adatszelet képes és hello fürt törlése, ha hello feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f6e28-105">It can create a HDInsight Hadoop cluster just-in-time tooprocess an input data slice and delete hello cluster when hello processing is complete.</span></span> <span data-ttu-id="f6e28-106">Egy igény szerinti HDInsight Hadoop-fürt használatának hello előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="f6e28-106">Some of hello benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="f6e28-107">Ön egyetlen fizetési hello idő feladat fut hello HDInsight Hadoop cluster (és egy rövid konfigurálható üresjárati idő).</span><span class="sxs-lookup"><span data-stu-id="f6e28-107">You only pay for hello time job is running on hello HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="f6e28-108">a HDInsight-fürtök hello számlázás pro-értékelés percenként történik, függetlenül attól, hogy azokat, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="f6e28-108">hello billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="f6e28-109">Egy igény szerinti HDInsight kapcsolódó szolgáltatás használatakor a Data Factory hello fürtök igény jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-109">When you use an on-demand HDInsight linked service in Data Factory, hello clusters are created on-demand.</span></span> <span data-ttu-id="f6e28-110">És hello fürtök automatikusan törlődnek hello feladatok elvégzésekor.</span><span class="sxs-lookup"><span data-stu-id="f6e28-110">And hello clusters are deleted automatically when hello jobs are completed.</span></span> <span data-ttu-id="f6e28-111">Ezért csak kell fizetnie hello feladat fut, és hello rövid üresjárati idő (live idő beállítása).</span><span class="sxs-lookup"><span data-stu-id="f6e28-111">Therefore, you only pay for hello job running time and hello brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="f6e28-112">Használatával a Data Factory-folyamathoz Munkafolyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f6e28-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="f6e28-113">Például lehet hello adatcsatorna toocopy adatait egy helyi SQL Server tooan Azure blob-tároló, a folyamat hello adatok az igény szerinti HDInsight Hadoop-fürt a Hive parancsfájlok és a Pig-parancsprogram futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f6e28-113">For example, you can have hello pipeline toocopy data from an on-premises SQL Server tooan Azure blob storage, process hello data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f6e28-114">Ezután másolja hello eredmény adatok tooan Azure SQL Data Warehouse BI alkalmazások tooconsume.</span><span class="sxs-lookup"><span data-stu-id="f6e28-114">Then, copy hello result data tooan Azure SQL Data Warehouse for BI applications tooconsume.</span></span>
- <span data-ttu-id="f6e28-115">Ütemezheti az hello munkafolyamat toorun időnként (óránként, naponta, hetente, havonta, stb.).</span><span class="sxs-lookup"><span data-stu-id="f6e28-115">You can schedule hello workflow toorun periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="f6e28-116">Az Azure Data Factoryben adat-előállító rendelkezhet egy vagy több adat folyamatok.</span><span class="sxs-lookup"><span data-stu-id="f6e28-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="f6e28-117">Adatok folyamat rendelkezik egy vagy több tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="f6e28-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="f6e28-118">Tevékenységek két típusa van: [adatok mozgása tevékenységek](../data-factory/data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységek](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="f6e28-119">Adatok (jelenleg csak másolási tevékenység) tevékenységek toomove adat egy forrás adatokat tároló tooa cél adattárból használhatja.</span><span class="sxs-lookup"><span data-stu-id="f6e28-119">You use data movement activities (currently, only Copy Activity) toomove data from a source data store tooa destination data store.</span></span> <span data-ttu-id="f6e28-120">Adatok átalakítása tevékenységek tootransform/folyamat adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="f6e28-120">You use data transformation activities tootransform/process data.</span></span> <span data-ttu-id="f6e28-121">HDInsight Hive tevékenység a Data Factory által támogatott hello átalakítása tevékenységek egyike.</span><span class="sxs-lookup"><span data-stu-id="f6e28-121">HDInsight Hive Activity is one of hello transformation activities supported by Data Factory.</span></span> <span data-ttu-id="f6e28-122">Ebben az oktatóanyagban hello Hive átalakítási tevékenységet használja.</span><span class="sxs-lookup"><span data-stu-id="f6e28-122">You use hello Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="f6e28-123">Konfigurálhatja a hive tevékenység toouse saját HDInsight Hadoop-fürt vagy egy igény szerinti HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-123">You can configure a hive activity toouse your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="f6e28-124">Ebben az oktatóanyagban hello hello data factory-folyamathoz Hive tevékenység konfigurált toouse igény szerinti HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-124">In this tutorial, hello Hive activity in hello data factory pipeline is configured toouse an on-demand HDInsight cluster.</span></span> <span data-ttu-id="f6e28-125">Ezért amikor hello tevékenység fut tooprocess egy adatszelet, ez történik:</span><span class="sxs-lookup"><span data-stu-id="f6e28-125">Therefore, when hello activity runs tooprocess a data slice, here is what happens:</span></span>

1. <span data-ttu-id="f6e28-126">HDInsight Hadoop-fürthöz, közvetlenül az időponthoz kötött tooprocess hello szelet automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="f6e28-126">A HDInsight Hadoop cluster is automatically created for you just-in-time tooprocess hello slice.</span></span>  
2. <span data-ttu-id="f6e28-127">hello bemeneti adatokat dolgozza fel a HiveQL-parancsfájlt hello fürtben futó.</span><span class="sxs-lookup"><span data-stu-id="f6e28-127">hello input data is processed by running a HiveQL script on hello cluster.</span></span>
3. <span data-ttu-id="f6e28-128">HDInsight Hadoop-fürt hello hello feldolgozás befejezése után, hello fürt üresjáratban konfigurálva hello időn (timeToLive-beállítást) törlődik.</span><span class="sxs-lookup"><span data-stu-id="f6e28-128">hello HDInsight Hadoop cluster is deleted after hello processing is complete and hello cluster is idle for hello configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="f6e28-129">A timeToLive üresjárati idő feldolgozásra hello következő adatszelet érhető el, hello ugyanabban a fürtben esetén használt tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="f6e28-129">If hello next data slice is available for processing with in this timeToLive idle time, hello same cluster is used tooprocess hello slice.</span></span>  

<span data-ttu-id="f6e28-130">Ebben az oktatóanyagban hello hello hive tevékenységhez társított HiveQL-parancsfájlt hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f6e28-130">In this tutorial, hello HiveQL script associated with hello hive activity performs hello following actions:</span></span>

1. <span data-ttu-id="f6e28-131">Létrehoz egy külső táblát, amely a hivatkozások hello egy Azure Blob storage-ban tárolt nyers webes naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="f6e28-131">Creates an external table that references hello raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="f6e28-132">Partíciók hello nyers adatok hónap és év szerint.</span><span class="sxs-lookup"><span data-stu-id="f6e28-132">Partitions hello raw data by year and month.</span></span>
3. <span data-ttu-id="f6e28-133">Tárolók hello particionált adatok hello Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-133">Stores hello partitioned data in hello Azure blob storage.</span></span>

<span data-ttu-id="f6e28-134">Ebben az oktatóanyagban hello hello hive tevékenységhez társított HiveQL-parancsfájlt, amely hivatkozások hello hello Azure Blob Storage tárolóban tárolt nyers webes naplóadatokat külső táblát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-134">In this tutorial, hello HiveQL script associated with hello hive activity creates an external table that references hello raw web log data stored in hello Azure Blob Storage.</span></span> <span data-ttu-id="f6e28-135">Az alábbiakban hello minta sorok havonta hello bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="f6e28-135">Here are hello sample rows for each month in hello input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="f6e28-136">hello HiveQL parancsfájl partíciók hello nyers adatok hónap és év szerint.</span><span class="sxs-lookup"><span data-stu-id="f6e28-136">hello HiveQL script partitions hello raw data by year and month.</span></span> <span data-ttu-id="f6e28-137">Három kimeneti mappa hello előző bemeneti alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-137">It creates three output folders based on hello previous input.</span></span> <span data-ttu-id="f6e28-138">Minden mappa minden hónap bejegyzéseinek fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="f6e28-139">Adat-előállító átalakítása tevékenységek hozzáadása tooHive tevékenységben listájáért lásd: [alakít át és elemez az Azure Data Factory használatával](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-139">For a list of Data Factory data transformation activities in addition tooHive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f6e28-140">Jelenleg csak létrehozhat HDInsight fürt 3.2-es verziójú Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f6e28-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f6e28-141">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f6e28-141">Prerequisites</span></span>
<span data-ttu-id="f6e28-142">Ez a cikk utasításait hello megkezdése előtt a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f6e28-142">Before you begin hello instructions in this article, you must have hello following items:</span></span>

* <span data-ttu-id="f6e28-143">[Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f6e28-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="f6e28-144">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6e28-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="f6e28-145">Készítse elő a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="f6e28-145">Prepare storage account</span></span>
<span data-ttu-id="f6e28-146">Ebben a forgatókönyvben toothree storage-fiók mentése használhatja:</span><span class="sxs-lookup"><span data-stu-id="f6e28-146">You can use up toothree storage accounts in this scenario:</span></span>

- <span data-ttu-id="f6e28-147">alapértelmezett tárfiók hello HDInsight-fürthöz</span><span class="sxs-lookup"><span data-stu-id="f6e28-147">default storage account for hello HDInsight cluster</span></span>
- <span data-ttu-id="f6e28-148">a bemeneti adatok hello Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="f6e28-148">storage account for hello input data</span></span>
- <span data-ttu-id="f6e28-149">tárfiók hello kimeneti adatai</span><span class="sxs-lookup"><span data-stu-id="f6e28-149">storage account for hello output data</span></span>

<span data-ttu-id="f6e28-150">toosimplify hello oktatóanyagban egy tárolási fiók tooserve hello három célra használja.</span><span class="sxs-lookup"><span data-stu-id="f6e28-150">toosimplify hello tutorial, you use one storage account tooserve hello three purposes.</span></span> <span data-ttu-id="f6e28-151">hello ebben a szakaszban található Azure PowerShell-parancsfájlpélda hello a következő feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f6e28-151">hello Azure PowerShell sample script found in this section performs hello following tasks:</span></span>

1. <span data-ttu-id="f6e28-152">Jelentkezzen be tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f6e28-152">Log in tooAzure.</span></span>
2. <span data-ttu-id="f6e28-153">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6e28-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="f6e28-154">Hozzon létre egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="f6e28-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="f6e28-155">Egy Blob-tároló hello storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6e28-155">Create a Blob container in hello storage account</span></span>
5. <span data-ttu-id="f6e28-156">Másolja a következő két fájlok toohello Blob tároló hello:</span><span class="sxs-lookup"><span data-stu-id="f6e28-156">Copy hello following two files toohello Blob container:</span></span>

   * <span data-ttu-id="f6e28-157">A bemeneti adatfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="f6e28-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="f6e28-158">HiveQL-parancsfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="f6e28-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="f6e28-159">Mindkét fájljai a következő nyilvános blobtárolóban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="f6e28-160">**tooprepare hello tárolási, és másolja hello fájlok Azure PowerShell használatával:**</span><span class="sxs-lookup"><span data-stu-id="f6e28-160">**tooprepare hello storage and copy hello files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f6e28-161">Adjon meg nevet hello Azure erőforráscsoport és hello parancsfájl által létrehozott hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f6e28-161">Specify names for hello Azure resource group and hello Azure storage account that will be created by hello script.</span></span>
> <span data-ttu-id="f6e28-162">Írja le **erőforráscsoport-név**, **tárfióknév**, és **tárfiók kulcsa** outputted hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f6e28-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by hello script.</span></span> <span data-ttu-id="f6e28-163">Már szükség hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-163">You need them in hello next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="f6e28-164">Ha a PowerShell-parancsfájllal hello segítségre van szüksége, tekintse meg [Using hello az Azure Storage Azure PowerShell](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-164">If you need help with hello PowerShell script, see [Using hello Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="f6e28-165">Ha például az Azure parancssori felület toouse ehelyett látható hello [függelék](#appendix) szakasz hello Azure CLI-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-165">If you like toouse Azure CLI instead, see hello [Appendix](#appendix) section for hello Azure CLI script.</span></span>

<span data-ttu-id="f6e28-166">**tooexamine hello tárolási fiók és hello tartalma**</span><span class="sxs-lookup"><span data-stu-id="f6e28-166">**tooexamine hello storage account and hello contents**</span></span>

1. <span data-ttu-id="f6e28-167">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f6e28-167">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f6e28-168">Kattintson a **erőforráscsoportok** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="f6e28-168">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="f6e28-169">Kattintson duplán az erőforráscsoport neve hello a PowerShell parancsfájl létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f6e28-169">Double-click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="f6e28-170">Hello szűrőt használhat, ha túl sok erőforrás-csoportok szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="f6e28-170">Use hello filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="f6e28-171">A hello **erőforrások** csempe, kell rendelkeznie kivéve, ha hello erőforráscsoport megosztása más projektek felsorolt egy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="f6e28-171">On hello **Resources** tile, you shall have one resource listed unless you share hello resource group with other projects.</span></span> <span data-ttu-id="f6e28-172">Adott erőforrás hello tárfiók a korábban meghatározott hello nevű.</span><span class="sxs-lookup"><span data-stu-id="f6e28-172">That resource is hello storage account with hello name you specified earlier.</span></span> <span data-ttu-id="f6e28-173">Kattintson a hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="f6e28-173">Click hello storage account name.</span></span>
5. <span data-ttu-id="f6e28-174">Kattintson a hello **Blobok** csempék.</span><span class="sxs-lookup"><span data-stu-id="f6e28-174">Click hello **Blobs** tiles.</span></span>
6. <span data-ttu-id="f6e28-175">Kattintson a hello **adfgetstarted** tároló.</span><span class="sxs-lookup"><span data-stu-id="f6e28-175">Click hello **adfgetstarted** container.</span></span> <span data-ttu-id="f6e28-176">Két mappák látja: **inputdata** és **parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="f6e28-177">Nyissa meg a hello mappa, és ellenőrizze a hello mappákban hello fájlok.</span><span class="sxs-lookup"><span data-stu-id="f6e28-177">Open hello folder and check hello files in hello folders.</span></span> <span data-ttu-id="f6e28-178">hello inputdata tartalmaz hello input.log fájl bemeneti adatokkal, és hello parancsfájlmappa hello HiveQL-parancsfájlt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f6e28-178">hello inputdata contains hello input.log file with input data and hello script folder contains hello HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="f6e28-179">A Resource Manager sablonnal adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6e28-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="f6e28-180">Hello tárfiókot, hello bemeneti adatok és hello előkészített HiveQL-parancsfájlt készen áll a toocreate egy az Azure data factory áll.</span><span class="sxs-lookup"><span data-stu-id="f6e28-180">With hello storage account, hello input data, and hello HiveQL script prepared, you are ready toocreate an Azure data factory.</span></span> <span data-ttu-id="f6e28-181">Többféleképpen az adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f6e28-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="f6e28-182">Ebben az oktatóanyagban létrehoz egy adat-előállító hello Azure-portál használatával egy Azure Resource Manager-sablon üzembe helyezésével.</span><span class="sxs-lookup"><span data-stu-id="f6e28-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using hello Azure portal.</span></span> <span data-ttu-id="f6e28-183">A Resource Manager-sablon használatával is telepítheti [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) és [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="f6e28-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="f6e28-184">Más data factory létrehozási módszert, lásd: [oktatóanyag: az első adat-előállító létrehozása](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="f6e28-185">Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="f6e28-185">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> <span data-ttu-id="f6e28-186">hello sablon https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="f6e28-186">hello template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="f6e28-187">Lásd: hello [adat-előállító entitások hello sablonban](#data-factory-entities-in-the-template) szakasz hello sablon entitásokból kapcsolatos részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="f6e28-187">See hello [Data Factory entities in hello template](#data-factory-entities-in-the-template) section for detailed information about entities defined in hello template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. <span data-ttu-id="f6e28-188">Válassza ki **használata meglévő** hello beállítása **erőforráscsoport** beállítás, és válassza hello neve (a PowerShell-parancsfájl használatával) hello előző lépésben létrehozott hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f6e28-188">Select **Use existing** option for hello **Resource group** setting, and select hello name of hello resource group you created in hello previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="f6e28-189">Adjon meg egy nevet az hello data factory (**adat-előállító**).</span><span class="sxs-lookup"><span data-stu-id="f6e28-189">Enter a name for hello data factory (**Data Factory Name**).</span></span> <span data-ttu-id="f6e28-190">Ez a név globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f6e28-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="f6e28-191">Adja meg a hello **tárfióknév** és **tárfiók kulcsa** hello előző lépésben megírt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-191">Enter hello **storage account name** and **storage account key** you wrote down in hello previous step.</span></span>
5. <span data-ttu-id="f6e28-192">Válassza ki **toohello feltételek és kikötések elfogadom** elolvasása után említettük **feltételek és kikötések**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-192">Select **I agree toohello terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="f6e28-193">Válassza ki **PIN-kód toodashboard** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f6e28-193">Select **Pin toodashboard** option.</span></span>
6. <span data-ttu-id="f6e28-194">Kattintson a **beszerzés/létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="f6e28-195">Megtekintheti a csempe irányítópult nevű hello **telepítése sablon-üzembehelyezés**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-195">You see a tile on hello Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="f6e28-196">Várjon, amíg hello **erőforráscsoport** az erőforráscsoport panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="f6e28-196">Wait until hello **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="f6e28-197">Hello csempe jelenik meg, az erőforráscsoport neve tooopen hello erőforrás csoport panel is kattinthat.</span><span class="sxs-lookup"><span data-stu-id="f6e28-197">You can also click hello tile titled as your resource group name tooopen hello resource group blade.</span></span>
6. <span data-ttu-id="f6e28-198">Ha hello erőforráscsoport panel még nincs megnyitva, kattintson a hello csempe tooopen hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="f6e28-198">Click hello tile tooopen hello resource group if hello resource group blade is not already open.</span></span> <span data-ttu-id="f6e28-199">Most megjelenik egy további data factory erőforrás továbbá felsorolt toohello tárolási fiók erőforrás.</span><span class="sxs-lookup"><span data-stu-id="f6e28-199">Now you shall see one more data factory resource listed in addition toohello storage account resource.</span></span>
7. <span data-ttu-id="f6e28-200">Kattintson a data factory hello nevére (hello megadott érték **adat-előállító** paraméter).</span><span class="sxs-lookup"><span data-stu-id="f6e28-200">Click hello name of your data factory (value you specified for hello **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="f6e28-201">Hello adat-előállító paneljén kattintson hello **Diagram** csempére.</span><span class="sxs-lookup"><span data-stu-id="f6e28-201">In hello Data Factory blade, click hello **Diagram** tile.</span></span> <span data-ttu-id="f6e28-202">hello ábrán egy bemeneti adatkészlet, és egy kimeneti adatkészlet egy tevékenységet:</span><span class="sxs-lookup"><span data-stu-id="f6e28-202">hello diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat diagramja](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="f6e28-204">hello nevek hello Resource Manager-sablon vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="f6e28-204">hello names are defined in hello Resource Manager template.</span></span>
9. <span data-ttu-id="f6e28-205">Kattintson duplán a **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="f6e28-206">A hello **Recent frissítése szeletek**, ekkor megjelenik egy szelet.</span><span class="sxs-lookup"><span data-stu-id="f6e28-206">On hello **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="f6e28-207">Ha hello állapota **folyamatban**, várja meg, amíg nem módosítják túl**készen**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-207">If hello status is **In progress**, wait until it is changed too**Ready**.</span></span> <span data-ttu-id="f6e28-208">Általában vesz igénybe **20 perc** toocreate HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-208">It usually takes about **20 minutes** toocreate an HDInsight cluster.</span></span>

### <a name="check-hello-data-factory-output"></a><span data-ttu-id="f6e28-209">Hello data factory kimeneti ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="f6e28-209">Check hello data factory output</span></span>

1. <span data-ttu-id="f6e28-210">Használja az azonos hello hello utolsó munkamenet toocheck hello tárolók hello adfgetstarted tároló eljárását.</span><span class="sxs-lookup"><span data-stu-id="f6e28-210">Use hello same procedure in hello last session toocheck hello containers of hello adfgetstarted container.</span></span> <span data-ttu-id="f6e28-211">Nincsenek két új tárolók továbbá túl**adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="f6e28-211">There are two new containers in addition too**adfgetsarted**:</span></span>

   * <span data-ttu-id="f6e28-212">Egy tároló hello mintája nevű: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="f6e28-212">A container with name that follows hello pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="f6e28-213">Ez a tároló egy hello alapértelmezett tároló hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-213">This container is hello default container for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="f6e28-214">adfjobs: Ebben a tárolóban hello ADF feladatnaplóit hello tárolója.</span><span class="sxs-lookup"><span data-stu-id="f6e28-214">adfjobs: This container is hello container for hello ADF job logs.</span></span>

     <span data-ttu-id="f6e28-215">hello data factory kimeneti tárolódik **afgetstarted** hello Resource Manager sablon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="f6e28-215">hello data factory output is stored in **afgetstarted** as you configured in hello Resource Manager template.</span></span>
2. <span data-ttu-id="f6e28-216">Kattintson a **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="f6e28-217">Kattintson duplán a **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="f6e28-218">Megjelenik egy **év = 2014** mappa mert összes hello webes naplókat dátuma 2014-ben.</span><span class="sxs-lookup"><span data-stu-id="f6e28-218">You see a **year=2014** folder because all hello web logs are dated in year 2014.</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="f6e28-220">Ha Ön is részletekbe menően tárhatják hello lista, január, február és március kell tekintse meg a három mappa.</span><span class="sxs-lookup"><span data-stu-id="f6e28-220">If you drill down hello list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="f6e28-221">És minden hónapban van egy naplófájl.</span><span class="sxs-lookup"><span data-stu-id="f6e28-221">And there is a log for each month.</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="f6e28-223">Data Factory entitások hello sablonban</span><span class="sxs-lookup"><span data-stu-id="f6e28-223">Data Factory entities in hello template</span></span>
<span data-ttu-id="f6e28-224">Itt látható, hogyan néz hello legfelső szintű erőforrás-kezelő sablon egy adat-előállító:</span><span class="sxs-lookup"><span data-stu-id="f6e28-224">Here is how hello top-level Resource Manager template for a data factory looks like:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a><span data-ttu-id="f6e28-225">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="f6e28-225">Define data factory</span></span>
<span data-ttu-id="f6e28-226">Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:</span><span class="sxs-lookup"><span data-stu-id="f6e28-226">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="f6e28-227">hello dataFactoryName hello adat-előállító hello sablon telepítésekor megadott hello neve.</span><span class="sxs-lookup"><span data-stu-id="f6e28-227">hello dataFactoryName is hello name of hello data factory you specify when you deploy hello template.</span></span> <span data-ttu-id="f6e28-228">Adat-előállító jelenleg csak a hello USA keleti régiója, USA nyugati régiója és Észak-Európa következő régiókban támogatott.</span><span class="sxs-lookup"><span data-stu-id="f6e28-228">Data factory is currently only supported in hello East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-hello-data-factory"></a><span data-ttu-id="f6e28-229">Hello adat-előállító belüli definiálása</span><span class="sxs-lookup"><span data-stu-id="f6e28-229">Defining entities within hello data factory</span></span>
<span data-ttu-id="f6e28-230">hello következő adat-előállító entitások definiált hello JSON-sablon:</span><span class="sxs-lookup"><span data-stu-id="f6e28-230">hello following Data Factory entities are defined in hello JSON template:</span></span>

* [<span data-ttu-id="f6e28-231">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6e28-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="f6e28-232">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6e28-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="f6e28-233">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f6e28-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="f6e28-234">Azure blobkimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f6e28-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="f6e28-235">Másolási tevékenységgel rendelkező adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="f6e28-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="f6e28-236">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6e28-236">Azure Storage linked service</span></span>
<span data-ttu-id="f6e28-237">hello Azure Storage társított szolgáltatás hivatkozások a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-237">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="f6e28-238">Ebben az oktatóanyagban hello ugyanabban a tárfiókban lesz hello alapértelmezett HDInsight tárfiókot, a bemeneti adatokat tároló és a kimeneti adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="f6e28-238">In this tutorial, hello same storage account is used as hello default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="f6e28-239">Ezért adja meg csak egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="f6e28-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="f6e28-240">A hello társított szolgáltatás definíciójának akkor adja meg a hello és az Azure storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="f6e28-240">In hello linked service definition, you specify hello name and key of your Azure storage account.</span></span> <span data-ttu-id="f6e28-241">Lásd: [Azure Storage társított szolgáltatásnak](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="f6e28-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
<span data-ttu-id="f6e28-242">Hello **connectionString** használ hello storageAccountName és storageAccountKey paraméterek.</span><span class="sxs-lookup"><span data-stu-id="f6e28-242">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="f6e28-243">Ezek a paraméterek értékeit hello sablon telepítése során adja meg.</span><span class="sxs-lookup"><span data-stu-id="f6e28-243">You specify values for these parameters while deploying hello template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="f6e28-244">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6e28-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="f6e28-245">A hello igény szerinti HDInsight társított szolgáltatás definíciójának, megadhatja a hello adat-előállító szolgáltatás toocreate egy HDInsight Hadoop futásidőben fürt által használt konfigurációs paraméterek értékét.</span><span class="sxs-lookup"><span data-stu-id="f6e28-245">In hello on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by hello Data Factory service toocreate a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="f6e28-246">Lásd: [összekapcsolt szolgáltatások számítási](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért cikk egy HDInsight igény társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f6e28-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
<span data-ttu-id="f6e28-247">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="f6e28-247">Note hello following points:</span></span>

* <span data-ttu-id="f6e28-248">hello adat-előállító létrehoz egy **Linux-alapú** meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-248">hello Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="f6e28-249">hello HDInsight Hadoop-fürt jön létre hello azonos hello tárfiók és a régióban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-249">hello HDInsight Hadoop cluster is created in hello same region as hello storage account.</span></span>
* <span data-ttu-id="f6e28-250">Értesítés hello *timeToLive* beállítást.</span><span class="sxs-lookup"><span data-stu-id="f6e28-250">Notice hello *timeToLive* setting.</span></span> <span data-ttu-id="f6e28-251">hello adat-előállító hello fürt üresjárati folyamatban van 30 perc után automatikusan törli hello fürt.</span><span class="sxs-lookup"><span data-stu-id="f6e28-251">hello data factory deletes hello cluster automatically after hello cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="f6e28-252">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="f6e28-252">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="f6e28-253">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="f6e28-253">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="f6e28-254">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="f6e28-254">This behavior is by design.</span></span> <span data-ttu-id="f6e28-255">Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz toobe (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

<span data-ttu-id="f6e28-256">További információkért lásd: [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="f6e28-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6e28-257">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="f6e28-258">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="f6e28-258">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="f6e28-259">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp".</span><span class="sxs-lookup"><span data-stu-id="f6e28-259">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="f6e28-260">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="f6e28-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="f6e28-261">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f6e28-261">Azure blob input dataset</span></span>
<span data-ttu-id="f6e28-262">Hello bemeneti adatkészlet-definícióban blob tároló, mappa, és hello bemeneti adatokat tartalmazó fájlt hello nevét kell megadni.</span><span class="sxs-lookup"><span data-stu-id="f6e28-262">In hello input dataset definition, you specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="f6e28-263">Lásd: [Azure-Blob adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="f6e28-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
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

<span data-ttu-id="f6e28-264">Figyelje meg a következő hello JSON-definícióban meghatározott beállításokat hello:</span><span class="sxs-lookup"><span data-stu-id="f6e28-264">Notice hello following specific settings in hello JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="f6e28-265">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="f6e28-265">Azure Blob output dataset</span></span>
<span data-ttu-id="f6e28-266">Hello kimeneti adatkészlet-definícióban és a blob tároló hello kimeneti adatokat tartalmazó mappa hello nevét kell megadni.</span><span class="sxs-lookup"><span data-stu-id="f6e28-266">In hello output dataset definition, you specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="f6e28-267">Lásd: [Azure-Blob adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="f6e28-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

<span data-ttu-id="f6e28-268">hello folderPath hello elérési toohello mappa hello kimeneti adatokat tartalmazó határozza meg:</span><span class="sxs-lookup"><span data-stu-id="f6e28-268">hello folderPath specifies hello path toohello folder that holds hello output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="f6e28-269">Hello [adatkészlet rendelkezésre állási](../data-factory/data-factory-create-datasets.md#dataset-availability) beállítás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f6e28-269">hello [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="f6e28-270">Az Azure Data Factoryben, kimeneti adatkészlet rendelkezésre állási meghajtók hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="f6e28-270">In Azure Data Factory, output dataset availability drives hello pipeline.</span></span> <span data-ttu-id="f6e28-271">Ebben a példában hello szelet jön létre havonta hello (EndOfInterval) hónap utolsó napján meg.</span><span class="sxs-lookup"><span data-stu-id="f6e28-271">In this example, hello slice is produced monthly on hello last day of month (EndOfInterval).</span></span> <span data-ttu-id="f6e28-272">További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="f6e28-273">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="f6e28-273">Data pipeline</span></span>
<span data-ttu-id="f6e28-274">Megadhatja egy folyamatot, amely átalakítja az adatok igény szerinti Azure HDInsight-fürtök a Hive parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="f6e28-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="f6e28-275">Lásd: [adatcsatorna JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását.</span><span class="sxs-lookup"><span data-stu-id="f6e28-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span>

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
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
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="f6e28-276">hello adatcsatorna egy tevékenységet, HDInsightHive tevékenységet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-276">hello pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="f6e28-277">Mivel a kezdő és záró dátumát 2016. január adatokat (a szeletek) feldolgozva csak egy hónap.</span><span class="sxs-lookup"><span data-stu-id="f6e28-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="f6e28-278">Mindkét *start* és *end* múltbeli dátum, hello tevékenység rendelkezik, így a Data Factory hello hello hónap azonnal adatokat dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="f6e28-278">Both *start* and *end* of hello activity have a past date, so hello Data Factory processes data for hello month immediately.</span></span> <span data-ttu-id="f6e28-279">Ha hello célból egy jövőbeli dátumot, hello adat-előállító létrehoz egy másik szelet, ha hello idő.</span><span class="sxs-lookup"><span data-stu-id="f6e28-279">If hello end is a future date, hello data factory creates another slice when hello time comes.</span></span> <span data-ttu-id="f6e28-280">További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-hello-tutorial"></a><span data-ttu-id="f6e28-281">Hello oktatóanyag tisztítása</span><span class="sxs-lookup"><span data-stu-id="f6e28-281">Clean up hello tutorial</span></span>

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="f6e28-282">Igény szerinti HDInsight-fürt által létrehozott hello blobtárolók törlése</span><span class="sxs-lookup"><span data-stu-id="f6e28-282">Delete hello blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="f6e28-283">Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén egy HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz (élettartam); toobe és hello fürt akkor törlődnek, ha nem hello hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (timeToLive); and hello cluster is deleted when hello processing is done.</span></span> <span data-ttu-id="f6e28-284">Az egyes fürtökön Azure Data Factory egy blob-tároló hello hello fürthöz hello alapértelmezett stroage fiókként használt Azure blob Storage tárolóban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6e28-284">For each cluster, Azure Data Factory creates a blob container in hello Azure blob storage used as hello default stroage account for hello cluster.</span></span> <span data-ttu-id="f6e28-285">Annak ellenére, hogy a HDInsight-fürtök törlése hello alapértelmezett blob tároló és a kapcsolódó hello tárfiók nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="f6e28-285">Even though HDInsight cluster is deleted, hello default blob storage container and hello associated storage account are not deleted.</span></span> <span data-ttu-id="f6e28-286">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="f6e28-286">This behavior is by design.</span></span> <span data-ttu-id="f6e28-287">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="f6e28-288">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="f6e28-288">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="f6e28-289">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="f6e28-289">hello names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="f6e28-290">Törölje a hello **adfjobs** és **adfyourdatafactoryname-linkedservicename-datetimestamp** mappák.</span><span class="sxs-lookup"><span data-stu-id="f6e28-290">Delete hello **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="f6e28-291">hello adfjobs tároló feladatnaplóit az Azure Data Factory tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-291">hello adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-hello-resource-group"></a><span data-ttu-id="f6e28-292">Hello erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="f6e28-292">Delete hello resource group</span></span>
<span data-ttu-id="f6e28-293">[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) van használt toodeploy, kezelheti és figyelheti a megoldás csoportként.</span><span class="sxs-lookup"><span data-stu-id="f6e28-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used toodeploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="f6e28-294">Erőforráscsoport törlése az összes hello összetevő hello csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="f6e28-294">Deleting a resource group deletes all hello components inside hello group.</span></span>  

1. <span data-ttu-id="f6e28-295">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f6e28-295">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f6e28-296">Kattintson a **erőforráscsoportok** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="f6e28-296">Click **Resource groups** on hello left pane.</span></span>
3. <span data-ttu-id="f6e28-297">Kattintson az erőforráscsoport neve hello a PowerShell parancsfájl létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f6e28-297">Click hello resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="f6e28-298">Hello szűrőt használhat, ha túl sok erőforrás-csoportok szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="f6e28-298">Use hello filter if you have too many resource groups listed.</span></span> <span data-ttu-id="f6e28-299">Hello erőforráscsoportot egy új panelen nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="f6e28-299">It opens hello resource group in a new blade.</span></span>
4. <span data-ttu-id="f6e28-300">A hello **erőforrások** csempe, kell rendelkeznie, hello alapértelmezett tárfiókot és felsorolt, kivéve, ha hello erőforráscsoport megosztása más projektek hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="f6e28-300">On hello **Resources** tile, you shall have hello default storage account and hello data factory listed unless you share hello resource group with other projects.</span></span>
5. <span data-ttu-id="f6e28-301">Kattintson a **törlése** hello felül hello panelről.</span><span class="sxs-lookup"><span data-stu-id="f6e28-301">Click **Delete** on hello top of hello blade.</span></span> <span data-ttu-id="f6e28-302">Ezzel törli a hello tárfiók és a storage-fiók hello hello adataihoz.</span><span class="sxs-lookup"><span data-stu-id="f6e28-302">Doing so deletes hello storage account and hello data stored in hello storage account.</span></span>
6. <span data-ttu-id="f6e28-303">Írja be a hello erőforrás csoport neve tooconfirm törlése, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="f6e28-303">Enter hello resource group name tooconfirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="f6e28-304">Abban az esetben, ha nem kívánja toodelete hello tárfiók hello erőforráscsoport törlésekor, fontolja meg a következő architektúra hello alapértelmezett tárfiókból hello üzleti adatok elválasztva hello.</span><span class="sxs-lookup"><span data-stu-id="f6e28-304">In case you don't want toodelete hello storage account when you delete hello resource group, consider hello following architecture by separating hello business data from hello default storage account.</span></span> <span data-ttu-id="f6e28-305">Ebben az esetben rendelkezik egy erőforráscsoport hello tárfiók hello üzleti adatok, és egy másik erőforráscsoportban hello alapértelmezett tárfiók HDInsight csatolt szolgáltatás és hello adat-előállító hello.</span><span class="sxs-lookup"><span data-stu-id="f6e28-305">In this case, you have one resource group for hello storage account with hello business data, and hello other resource group for hello default storage account for HDInsight linked service and hello data factory.</span></span> <span data-ttu-id="f6e28-306">Hello második erőforráscsoport törlése nem érinti hello üzleti adatok tárfiók.</span><span class="sxs-lookup"><span data-stu-id="f6e28-306">When you delete hello second resource group, it does not impact hello business data storage account.</span></span> <span data-ttu-id="f6e28-307">toodo így:</span><span class="sxs-lookup"><span data-stu-id="f6e28-307">toodo so:</span></span>

* <span data-ttu-id="f6e28-308">Adja hozzá a következő toohello legfelső szintű erőforráscsoport együtt hello Microsoft.DataFactory/datafactories erőforrás a Resource Manager sablon hello.</span><span class="sxs-lookup"><span data-stu-id="f6e28-308">Add hello following toohello top-level resource group along with hello Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="f6e28-309">Létrehoz egy tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="f6e28-309">It creates a storage account:</span></span>

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* <span data-ttu-id="f6e28-310">Új társított szolgáltatás pont toohello új tárfiók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f6e28-310">Add a new linked service point toohello new storage account:</span></span>

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* <span data-ttu-id="f6e28-311">Egy további dependsOn és egy additionalLinkedServiceNames hello HDInsight ondemand kapcsolódó szolgáltatás konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="f6e28-311">Configure hello HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a><span data-ttu-id="f6e28-312">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6e28-312">Next steps</span></span>
<span data-ttu-id="f6e28-313">Ebben a cikkben megtanulta, hogyan toouse Azure Data Factory toocreate igény szerinti HDInsight-fürt tooprocess Hive-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="f6e28-313">In this article, you have learned how toouse Azure Data Factory toocreate on-demand HDInsight cluster tooprocess Hive jobs.</span></span> <span data-ttu-id="f6e28-314">További tooread:</span><span class="sxs-lookup"><span data-stu-id="f6e28-314">tooread more:</span></span>

* [<span data-ttu-id="f6e28-315">Hadoop oktatóanyag: hdinsight Linux-alapú Hadoop használatának megkezdése</span><span class="sxs-lookup"><span data-stu-id="f6e28-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="f6e28-316">Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="f6e28-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="f6e28-317">HDInsight-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="f6e28-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="f6e28-318">Data factory dokumentáció</span><span class="sxs-lookup"><span data-stu-id="f6e28-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="f6e28-319">Függelék:</span><span class="sxs-lookup"><span data-stu-id="f6e28-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="f6e28-320">Az Azure CLI-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="f6e28-320">Azure CLI script</span></span>
<span data-ttu-id="f6e28-321">Használhatja az Azure CLI Azure PowerShell toodo hello oktatóanyag használata helyett.</span><span class="sxs-lookup"><span data-stu-id="f6e28-321">You can use Azure CLI instead of using Azure PowerShell toodo hello tutorial.</span></span> <span data-ttu-id="f6e28-322">toouse Azure CLI-t, először telepítse az Azure parancssori felület hello utasításai szerint:</span><span class="sxs-lookup"><span data-stu-id="f6e28-322">toouse Azure CLI, first install Azure CLI as per hello following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a><span data-ttu-id="f6e28-323">Az Azure parancssori felület tooprepare hello tárhelyet használja, és hello fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="f6e28-323">Use Azure CLI tooprepare hello storage and copy hello files</span></span>

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

<span data-ttu-id="f6e28-324">hello tároló neve *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="f6e28-324">hello container name is *adfgetstarted*.</span></span> <span data-ttu-id="f6e28-325">Legyen ez.</span><span class="sxs-lookup"><span data-stu-id="f6e28-325">Keep it as it is.</span></span> <span data-ttu-id="f6e28-326">Ellenkező esetben tooupdate hello Resource Manager-sablon van szüksége.</span><span class="sxs-lookup"><span data-stu-id="f6e28-326">Otherwise you need tooupdate hello Resource Manager template.</span></span> <span data-ttu-id="f6e28-327">Ha a parancssori felület parancsfájllal segítségre van szüksége, tekintse meg [az Azure Storage Azure CLI használata hello](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f6e28-327">If you need help with this CLI script, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
