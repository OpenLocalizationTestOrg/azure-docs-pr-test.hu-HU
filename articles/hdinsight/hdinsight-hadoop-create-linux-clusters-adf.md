---
title: "Igény szerinti használatával a Data Factory - Azure HDInsight Hadoop-fürtök létrehozása |} Microsoft Docs"
description: "Ismerje meg, igény szerinti Hadoop-fürtök létrehozása a HDInsight az Azure Data Factory használatával."
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
ms.openlocfilehash: e68f1d72965d9516e0552c84d03d234c21739390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a><span data-ttu-id="73b9b-103">Igény szerinti Hadoop-fürtök létrehozása a Hdinsightban Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="73b9b-103">Create on-demand Hadoop clusters in HDInsight using Azure Data Factory</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="73b9b-104">[Az Azure Data Factory](../data-factory/data-factory-introduction.md) egy felhőalapú integrációs szolgáltatás koordinálja és automatizálja az adatátviteli és az adatok átalakítása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-104">[Azure Data Factory](../data-factory/data-factory-introduction.md) is a cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="73b9b-105">Létrehozhat egy HDInsight Hadoop fürthöz közvetlenül az időponthoz kötött egy bemeneti adatszelet feldolgozni, és törölheti a fürtöt, ha feldolgozása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="73b9b-105">It can create a HDInsight Hadoop cluster just-in-time to process an input data slice and delete the cluster when the processing is complete.</span></span> <span data-ttu-id="73b9b-106">Egy igény szerinti HDInsight Hadoop-fürt használatának előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="73b9b-106">Some of the benefits of using an on-demand HDInsight Hadoop cluster are:</span></span>

- <span data-ttu-id="73b9b-107">Ön egyetlen fizetési idő feladat fut. a HDInsight Hadoop-fürt (és egy rövid konfigurálható üresjárati idő).</span><span class="sxs-lookup"><span data-stu-id="73b9b-107">You only pay for the time job is running on the HDInsight Hadoop cluster (plus a brief configurable idle time).</span></span> <span data-ttu-id="73b9b-108">A HDInsight-fürtök számlázása pro-értékelés percenként történik, függetlenül attól, hogy azokat, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="73b9b-108">The billing for HDInsight clusters is pro-rated per minute, whether you are using them or not.</span></span> <span data-ttu-id="73b9b-109">A Data Factory egy igény szerinti HDInsight kapcsolódó szolgáltatás használatakor a fürtök igény jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="73b9b-109">When you use an on-demand HDInsight linked service in Data Factory, the clusters are created on-demand.</span></span> <span data-ttu-id="73b9b-110">És a fürt automatikusan törlődnek a feladatok elvégzésekor.</span><span class="sxs-lookup"><span data-stu-id="73b9b-110">And the clusters are deleted automatically when the jobs are completed.</span></span> <span data-ttu-id="73b9b-111">Ezért csak kell fizetnie a feladat fut, és a rövid üresjárati idő (live idő beállítása).</span><span class="sxs-lookup"><span data-stu-id="73b9b-111">Therefore, you only pay for the job running time and the brief idle time (time-to-live setting).</span></span>
- <span data-ttu-id="73b9b-112">Használatával a Data Factory-folyamathoz Munkafolyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="73b9b-112">You can create a workflow using a Data Factory pipeline.</span></span> <span data-ttu-id="73b9b-113">Például lehet egy helyi SQL Server adatainak másolása az Azure blob Storage tárolóban, az adatok feldolgozása a Hive parancsfájlok és a Pig-parancsprogram futtatásával az igény szerinti HDInsight Hadoop-fürt folyamat.</span><span class="sxs-lookup"><span data-stu-id="73b9b-113">For example, you can have the pipeline to copy data from an on-premises SQL Server to an Azure blob storage, process the data by running a Hive script and a Pig script on an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="73b9b-114">Ezután másolja az eredményadatok Azure SQL Data Warehouse BI alkalmazások felhasználását.</span><span class="sxs-lookup"><span data-stu-id="73b9b-114">Then, copy the result data to an Azure SQL Data Warehouse for BI applications to consume.</span></span>
- <span data-ttu-id="73b9b-115">A munkafolyamat futtatását időnként (óránként, naponta, hetente, havonta, stb.) is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="73b9b-115">You can schedule the workflow to run periodically (hourly, daily, weekly, monthly, etc.).</span></span>

<span data-ttu-id="73b9b-116">Az Azure Data Factoryben adat-előállító rendelkezhet egy vagy több adat folyamatok.</span><span class="sxs-lookup"><span data-stu-id="73b9b-116">In Azure Data Factory, a data factory can have one or more data pipelines.</span></span> <span data-ttu-id="73b9b-117">Adatok folyamat rendelkezik egy vagy több tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="73b9b-117">A data pipeline has one or more activities.</span></span> <span data-ttu-id="73b9b-118">Tevékenységek két típusa van: [adatok mozgása tevékenységek](../data-factory/data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységek](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-118">There are two types of activities: [Data Movement Activities](../data-factory/data-factory-data-movement-activities.md) and [Data Transformation Activities](../data-factory/data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="73b9b-119">Adatok mozgása tevékenységek (jelenleg csak másolási tevékenység) használatával helyezze át az adatokat egy forrás adattárból a cél-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-119">You use data movement activities (currently, only Copy Activity) to move data from a source data store to a destination data store.</span></span> <span data-ttu-id="73b9b-120">Adatok átalakítása tevékenységek segítségével átalakítási/folyamat adatokat.</span><span class="sxs-lookup"><span data-stu-id="73b9b-120">You use data transformation activities to transform/process data.</span></span> <span data-ttu-id="73b9b-121">HDInsight Hive tevékenység a Data Factory által támogatott átalakítása tevékenységek egyike.</span><span class="sxs-lookup"><span data-stu-id="73b9b-121">HDInsight Hive Activity is one of the transformation activities supported by Data Factory.</span></span> <span data-ttu-id="73b9b-122">Ebben az oktatóanyagban a Hive átalakítási tevékenységet használja.</span><span class="sxs-lookup"><span data-stu-id="73b9b-122">You use the Hive transformation activity in this tutorial.</span></span>

<span data-ttu-id="73b9b-123">A hive tevékenység saját HDInsight Hadoop-fürt vagy egy igény szerinti HDInsight Hadoop-fürt használatára konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="73b9b-123">You can configure a hive activity to use your own HDInsight Hadoop cluster or an on-demand HDInsight Hadoop cluster.</span></span> <span data-ttu-id="73b9b-124">Ebben az oktatóanyagban a data factory-folyamat a Hive tevékenység igény szerinti HDInsight-fürt használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="73b9b-124">In this tutorial, the Hive activity in the data factory pipeline is configured to use an on-demand HDInsight cluster.</span></span> <span data-ttu-id="73b9b-125">Ezért amikor a tevékenység fut egy adatszelet feldolgozni, ez történik:</span><span class="sxs-lookup"><span data-stu-id="73b9b-125">Therefore, when the activity runs to process a data slice, here is what happens:</span></span>

1. <span data-ttu-id="73b9b-126">A HDInsight Hadoop-fürt automatikusan létrejön, közvetlenül az időponthoz kötött a szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-126">A HDInsight Hadoop cluster is automatically created for you just-in-time to process the slice.</span></span>  
2. <span data-ttu-id="73b9b-127">A bemeneti adatokat dolgozza fel a fürtön fut egy HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-127">The input data is processed by running a HiveQL script on the cluster.</span></span>
3. <span data-ttu-id="73b9b-128">A HDInsight Hadoop-fürtök törlése a feldolgozás befejezése után, a fürt üresjáratban a beállított időn (timeToLive-beállítást).</span><span class="sxs-lookup"><span data-stu-id="73b9b-128">The HDInsight Hadoop cluster is deleted after the processing is complete and the cluster is idle for the configured amount of time (timeToLive setting).</span></span> <span data-ttu-id="73b9b-129">A timeToLive üresjárati idő feldolgozás a következő adatszelet érhető el, ha ugyanabban a fürtben szolgál a szelet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-129">If the next data slice is available for processing with in this timeToLive idle time, the same cluster is used to process the slice.</span></span>  

<span data-ttu-id="73b9b-130">Ebben az oktatóanyagban a hive tevékenységhez társított HiveQL-parancsfájlt az alábbi műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="73b9b-130">In this tutorial, the HiveQL script associated with the hive activity performs the following actions:</span></span>

1. <span data-ttu-id="73b9b-131">Létrehoz egy külső táblát, amely a nyers webes naplóadatokat, egy Azure Blob storage-ban tárolt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="73b9b-131">Creates an external table that references the raw web log data stored in an Azure Blob storage.</span></span>
2. <span data-ttu-id="73b9b-132">Partíciók évben és hónapban a nyers adatok.</span><span class="sxs-lookup"><span data-stu-id="73b9b-132">Partitions the raw data by year and month.</span></span>
3. <span data-ttu-id="73b9b-133">A particionált adatot tárol az Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="73b9b-133">Stores the partitioned data in the Azure blob storage.</span></span>

<span data-ttu-id="73b9b-134">Ebben az oktatóanyagban a HiveQL-parancsfájlt a hive tevékenységhez társított létrehoz egy külső táblát, amely a nyers webes naplóadatokat, az Azure Blob Storage tárolóban tárolt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="73b9b-134">In this tutorial, the HiveQL script associated with the hive activity creates an external table that references the raw web log data stored in the Azure Blob Storage.</span></span> <span data-ttu-id="73b9b-135">Az alábbiakban a minta sorok minden hónapban a bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="73b9b-135">Here are the sample rows for each month in the input file.</span></span>

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

<span data-ttu-id="73b9b-136">A HiveQL-parancsfájlt a nyers adatok particionálja a hónap és év szerint.</span><span class="sxs-lookup"><span data-stu-id="73b9b-136">The HiveQL script partitions the raw data by year and month.</span></span> <span data-ttu-id="73b9b-137">Három kimeneti mappa az előző bemeneti alapján hoz létre.</span><span class="sxs-lookup"><span data-stu-id="73b9b-137">It creates three output folders based on the previous input.</span></span> <span data-ttu-id="73b9b-138">Minden mappa minden hónap bejegyzéseinek fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="73b9b-138">Each folder contains a file with entries from each month.</span></span>

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

<span data-ttu-id="73b9b-139">Adat-előállító átalakítása tevékenységek struktúrát tevékenység mellett listájáért lásd: [alakít át és elemez az Azure Data Factory használatával](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-139">For a list of Data Factory data transformation activities in addition to Hive activity, see [Transform and analyze using Azure Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="73b9b-140">Jelenleg csak létrehozhat HDInsight fürt 3.2-es verziójú Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="73b9b-140">Currently, you can only create HDInsight cluster version 3.2 from Azure Data Factory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73b9b-141">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73b9b-141">Prerequisites</span></span>
<span data-ttu-id="73b9b-142">A jelen cikkben lévő utasítások megkezdése előtt rendelkeznie kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="73b9b-142">Before you begin the instructions in this article, you must have the following items:</span></span>

* <span data-ttu-id="73b9b-143">[Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="73b9b-143">[Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="73b9b-144">Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="73b9b-144">Azure PowerShell.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a><span data-ttu-id="73b9b-145">Készítse elő a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="73b9b-145">Prepare storage account</span></span>
<span data-ttu-id="73b9b-146">Ebben a forgatókönyvben legfeljebb három storage-fiókok is használhatja:</span><span class="sxs-lookup"><span data-stu-id="73b9b-146">You can use up to three storage accounts in this scenario:</span></span>

- <span data-ttu-id="73b9b-147">a HDInsight-fürthöz az alapértelmezett tárfiók</span><span class="sxs-lookup"><span data-stu-id="73b9b-147">default storage account for the HDInsight cluster</span></span>
- <span data-ttu-id="73b9b-148">a bemeneti adatok tárfiók</span><span class="sxs-lookup"><span data-stu-id="73b9b-148">storage account for the input data</span></span>
- <span data-ttu-id="73b9b-149">a kimeneti adatok Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="73b9b-149">storage account for the output data</span></span>

<span data-ttu-id="73b9b-150">Az oktatóanyag leegyszerűsítése segítségével egy tárfiókot a három célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="73b9b-150">To simplify the tutorial, you use one storage account to serve the three purposes.</span></span> <span data-ttu-id="73b9b-151">Az Azure PowerShell-parancsfájlpélda ebben a szakaszban található a következő feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="73b9b-151">The Azure PowerShell sample script found in this section performs the following tasks:</span></span>

1. <span data-ttu-id="73b9b-152">Jelentkezzen be az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="73b9b-152">Log in to Azure.</span></span>
2. <span data-ttu-id="73b9b-153">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b9b-153">Create an Azure resource group.</span></span>
3. <span data-ttu-id="73b9b-154">Hozzon létre egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="73b9b-154">Create an Azure Storage account.</span></span>
4. <span data-ttu-id="73b9b-155">A tárfiók egy Blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b9b-155">Create a Blob container in the storage account</span></span>
5. <span data-ttu-id="73b9b-156">A következő két fájlt másolja a Blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="73b9b-156">Copy the following two files to the Blob container:</span></span>

   * <span data-ttu-id="73b9b-157">A bemeneti adatfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span><span class="sxs-lookup"><span data-stu-id="73b9b-157">Input data file: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)</span></span>
   * <span data-ttu-id="73b9b-158">HiveQL-parancsfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span><span class="sxs-lookup"><span data-stu-id="73b9b-158">HiveQL script: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)</span></span>

     <span data-ttu-id="73b9b-159">Mindkét fájljai a következő nyilvános blobtárolóban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-159">Both files are stored in a public Blob container.</span></span>


<span data-ttu-id="73b9b-160">**A tárterület előkészítése és Azure PowerShell használatával másolhat fájlokat:**</span><span class="sxs-lookup"><span data-stu-id="73b9b-160">**To prepare the storage and copy the files using Azure PowerShell:**</span></span>
> [!IMPORTANT]
> <span data-ttu-id="73b9b-161">Adjon meg nevet az Azure erőforráscsoport és az Azure storage-fiók, a parancsfájl által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="73b9b-161">Specify names for the Azure resource group and the Azure storage account that will be created by the script.</span></span>
> <span data-ttu-id="73b9b-162">Írja le **erőforráscsoport-név**, **tárfióknév**, és **tárfiók kulcsa** outputted a parancsfájl által.</span><span class="sxs-lookup"><span data-stu-id="73b9b-162">Write down **resource group name**, **storage account name**, and **storage account key** outputted by the script.</span></span> <span data-ttu-id="73b9b-163">Már szükség a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-163">You need them in the next section.</span></span>

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
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

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

<span data-ttu-id="73b9b-164">Ha a PowerShell parancsfájl segítségre van szüksége, tekintse meg a [az Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-164">If you need help with the PowerShell script, see [Using the Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span> <span data-ttu-id="73b9b-165">Ha szeretné, hogy az Azure parancssori felület használata, tekintse meg a [függelék](#appendix) szakaszban az Azure CLI-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-165">If you like to use Azure CLI instead, see the [Appendix](#appendix) section for the Azure CLI script.</span></span>

<span data-ttu-id="73b9b-166">**A tárfiók és a tartalom vizsgálata**</span><span class="sxs-lookup"><span data-stu-id="73b9b-166">**To examine the storage account and the contents**</span></span>

1. <span data-ttu-id="73b9b-167">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="73b9b-167">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="73b9b-168">Kattintson a **erőforráscsoportok** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="73b9b-168">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="73b9b-169">Kattintson duplán a PowerShell parancsfájl létrehozott az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="73b9b-169">Double-click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="73b9b-170">A szűrő használja, ha túl sok erőforrás-csoportok szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="73b9b-170">Use the filter if you have too many resource groups listed.</span></span>
4. <span data-ttu-id="73b9b-171">Az a **erőforrások** csempe, kell rendelkeznie kivéve, ha az erőforráscsoport megosztása más projektek felsorolt egy erőforrást.</span><span class="sxs-lookup"><span data-stu-id="73b9b-171">On the **Resources** tile, you shall have one resource listed unless you share the resource group with other projects.</span></span> <span data-ttu-id="73b9b-172">Adott erőforrás a korábban megadott nevű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="73b9b-172">That resource is the storage account with the name you specified earlier.</span></span> <span data-ttu-id="73b9b-173">Kattintson a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="73b9b-173">Click the storage account name.</span></span>
5. <span data-ttu-id="73b9b-174">Kattintson a **Blobok** csempék.</span><span class="sxs-lookup"><span data-stu-id="73b9b-174">Click the **Blobs** tiles.</span></span>
6. <span data-ttu-id="73b9b-175">Kattintson a **adfgetstarted** tároló.</span><span class="sxs-lookup"><span data-stu-id="73b9b-175">Click the **adfgetstarted** container.</span></span> <span data-ttu-id="73b9b-176">Két mappák látja: **inputdata** és **parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-176">You see two folders: **inputdata** and **script**.</span></span>
7. <span data-ttu-id="73b9b-177">Nyissa meg a mappát, és a mappákban található fájlokat.</span><span class="sxs-lookup"><span data-stu-id="73b9b-177">Open the folder and check the files in the folders.</span></span> <span data-ttu-id="73b9b-178">A inputdata tartalmazza a input.log fájl bemeneti adatokkal, és a parancsfájl mappa tartalmazza a HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-178">The inputdata contains the input.log file with input data and the script folder contains the HiveQL script file.</span></span>

## <a name="create-a-data-factory-using-resource-manager-template"></a><span data-ttu-id="73b9b-179">A Resource Manager sablonnal adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="73b9b-179">Create a data factory using Resource Manager template</span></span>
<span data-ttu-id="73b9b-180">A tárfiók, a bemeneti adatok és a HiveQL-parancsfájlt készített készen áll az Azure data factory létrehozása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-180">With the storage account, the input data, and the HiveQL script prepared, you are ready to create an Azure data factory.</span></span> <span data-ttu-id="73b9b-181">Többféleképpen az adat-előállító létrehozása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-181">There are several methods for creating data factory.</span></span> <span data-ttu-id="73b9b-182">Ebben az oktatóanyagban létrehoz egy adat-előállító telepítésével az Azure Resource Manager-sablon az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="73b9b-182">In this tutorial, you create a data factory by deploying an Azure Resource Manager template using the Azure portal.</span></span> <span data-ttu-id="73b9b-183">A Resource Manager-sablon használatával is telepítheti [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) és [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="73b9b-183">You can also deploy a Resource Manager template by using [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) and [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span> <span data-ttu-id="73b9b-184">Más data factory létrehozási módszert, lásd: [oktatóanyag: az első adat-előállító létrehozása](../data-factory/data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-184">For other data factory creation methods, see [Tutorial: Build your first data factory](../data-factory/data-factory-build-your-first-pipeline.md).</span></span>

1. <span data-ttu-id="73b9b-185">Az alábbi képre kattintva jelentkezzen be az Azure-ba, és nyissa meg a Resource Manager-sablont az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="73b9b-185">Click the following image to sign in to Azure and open the Resource Manager template in the Azure portal.</span></span> <span data-ttu-id="73b9b-186">A sablon https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="73b9b-186">The template is located at https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json.</span></span> <span data-ttu-id="73b9b-187">Tekintse meg a [adat-előállító bejegyzései szerepelnek a sablon](#data-factory-entities-in-the-template) szakasz a sablonban definiált entitások kapcsolatos részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="73b9b-187">See the [Data Factory entities in the template](#data-factory-entities-in-the-template) section for detailed information about entities defined in the template.</span></span> 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. <span data-ttu-id="73b9b-188">Válassza ki **használata meglévő** választás a **erőforráscsoport** beállításával, és válassza ki a nevét (a PowerShell-parancsfájl használatával), az előző lépésben létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="73b9b-188">Select **Use existing** option for the **Resource group** setting, and select the name of the resource group you created in the previous step (using PowerShell script).</span></span>
3. <span data-ttu-id="73b9b-189">Adjon meg egy nevet az adat-előállítóban (**adat-előállító**).</span><span class="sxs-lookup"><span data-stu-id="73b9b-189">Enter a name for the data factory (**Data Factory Name**).</span></span> <span data-ttu-id="73b9b-190">Ez a név globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="73b9b-190">This name must be globally unique.</span></span>
4. <span data-ttu-id="73b9b-191">Adja meg a **tárfióknév** és **tárfiók kulcsa** az előző lépésben megírt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-191">Enter the **storage account name** and **storage account key** you wrote down in the previous step.</span></span>
5. <span data-ttu-id="73b9b-192">Válassza ki **elfogadom a feltételeket és kikötéseket** elolvasása után említettük **feltételek és kikötések**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-192">Select **I agree to the terms and conditions** stated above after reading through **terms and conditions**.</span></span>
6. <span data-ttu-id="73b9b-193">Válassza ki **rögzítés az irányítópulton** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="73b9b-193">Select **Pin to dashboard** option.</span></span>
6. <span data-ttu-id="73b9b-194">Kattintson a **beszerzés/létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-194">Click **Purchase/Create**.</span></span> <span data-ttu-id="73b9b-195">Megjelenik az Irányítópulton egy csempére nevű **telepítése sablon-üzembehelyezés**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-195">You see a tile on the Dashboard called **Deploying Template deployment**.</span></span> <span data-ttu-id="73b9b-196">Várjon, amíg a **erőforráscsoport** az erőforráscsoport panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="73b9b-196">Wait until the **Resource group** blade for your resource group opens.</span></span> <span data-ttu-id="73b9b-197">A csempe jelenik meg az erőforráscsoport neve az erőforráscsoport panel megnyitásához, is kattinthat.</span><span class="sxs-lookup"><span data-stu-id="73b9b-197">You can also click the tile titled as your resource group name to open the resource group blade.</span></span>
6. <span data-ttu-id="73b9b-198">Kattintson a csempére kattintva nyissa meg az erőforráscsoportot, ha az erőforráscsoport panel még nincs megnyitva.</span><span class="sxs-lookup"><span data-stu-id="73b9b-198">Click the tile to open the resource group if the resource group blade is not already open.</span></span> <span data-ttu-id="73b9b-199">Most látni fog egy további adatokat előállító szereplő erőforrásra a tárolási fiók erőforrások mellett.</span><span class="sxs-lookup"><span data-stu-id="73b9b-199">Now you shall see one more data factory resource listed in addition to the storage account resource.</span></span>
7. <span data-ttu-id="73b9b-200">Kattintson a data factory nevét (a megadott értékre a **adat-előállító** paraméter).</span><span class="sxs-lookup"><span data-stu-id="73b9b-200">Click the name of your data factory (value you specified for the **Data Factory Name** parameter).</span></span>
8. <span data-ttu-id="73b9b-201">A Data Factory panelen kattintson a **Diagram** csempére.</span><span class="sxs-lookup"><span data-stu-id="73b9b-201">In the Data Factory blade, click the **Diagram** tile.</span></span> <span data-ttu-id="73b9b-202">Az ábrán látható egy bemeneti adatkészlet, és egy kimeneti adatkészlet egy tevékenységet:</span><span class="sxs-lookup"><span data-stu-id="73b9b-202">The diagram shows one activity with an input dataset, and an output dataset:</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat diagramja](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    <span data-ttu-id="73b9b-204">A nevek a Resource Manager-sablon vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="73b9b-204">The names are defined in the Resource Manager template.</span></span>
9. <span data-ttu-id="73b9b-205">Kattintson duplán a **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-205">Double-click **AzureBlobOutput**.</span></span>
10. <span data-ttu-id="73b9b-206">Az a **Recent frissítése szeletek**, ekkor megjelenik egy szelet.</span><span class="sxs-lookup"><span data-stu-id="73b9b-206">On the **Recent updated slices**, you shall see one slice.</span></span> <span data-ttu-id="73b9b-207">Ha az állapot **folyamatban**, várjon, amíg értékre módosul **készen**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-207">If the status is **In progress**, wait until it is changed to **Ready**.</span></span> <span data-ttu-id="73b9b-208">Általában vesz igénybe **20 perc** hozhat létre HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-208">It usually takes about **20 minutes** to create an HDInsight cluster.</span></span>

### <a name="check-the-data-factory-output"></a><span data-ttu-id="73b9b-209">Ellenőrizze a data factory kimenet</span><span class="sxs-lookup"><span data-stu-id="73b9b-209">Check the data factory output</span></span>

1. <span data-ttu-id="73b9b-210">A legutóbbi munkamenet ugyanazzal az eljárással használatával ellenőrizze a tárolók a adfgetstarted tároló.</span><span class="sxs-lookup"><span data-stu-id="73b9b-210">Use the same procedure in the last session to check the containers of the adfgetstarted container.</span></span> <span data-ttu-id="73b9b-211">Nincsenek kívül két új tárolók **adfgetsarted**:</span><span class="sxs-lookup"><span data-stu-id="73b9b-211">There are two new containers in addition to **adfgetsarted**:</span></span>

   * <span data-ttu-id="73b9b-212">Egy tároló megadásával, amely mintát követi: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="73b9b-212">A container with name that follows the pattern: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`.</span></span> <span data-ttu-id="73b9b-213">Ebben a tárolóban az alapértelmezett tároló a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="73b9b-213">This container is the default container for the HDInsight cluster.</span></span>
   * <span data-ttu-id="73b9b-214">adfjobs: Ebben a tárolóban az ADF feladatnaplóit az alkalmazása tárolója.</span><span class="sxs-lookup"><span data-stu-id="73b9b-214">adfjobs: This container is the container for the ADF job logs.</span></span>

     <span data-ttu-id="73b9b-215">A data factory kimeneti tárolódik **afgetstarted** a Resource Manager sablon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="73b9b-215">The data factory output is stored in **afgetstarted** as you configured in the Resource Manager template.</span></span>
2. <span data-ttu-id="73b9b-216">Kattintson a **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-216">Click **adfgetstarted**.</span></span>
3. <span data-ttu-id="73b9b-217">Kattintson duplán a **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-217">Double-click **partitioneddata**.</span></span> <span data-ttu-id="73b9b-218">Megjelenik egy **év = 2014** mappa, mert a webalkalmazás-naplók dátuma 2014-ben.</span><span class="sxs-lookup"><span data-stu-id="73b9b-218">You see a **year=2014** folder because all the web logs are dated in year 2014.</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    <span data-ttu-id="73b9b-220">Ha a lista részletező, január, február és március kell tekintse meg a három mappa.</span><span class="sxs-lookup"><span data-stu-id="73b9b-220">If you drill down the list, you shall see three folders for January, February, and March.</span></span> <span data-ttu-id="73b9b-221">És minden hónapban van egy naplófájl.</span><span class="sxs-lookup"><span data-stu-id="73b9b-221">And there is a log for each month.</span></span>

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a><span data-ttu-id="73b9b-223">Data Factory-entitások a sablonban</span><span class="sxs-lookup"><span data-stu-id="73b9b-223">Data Factory entities in the template</span></span>
<span data-ttu-id="73b9b-224">Itt látható, hogyan néz egy adat-előállító legfelső szintű erőforrás-kezelő sablonjának:</span><span class="sxs-lookup"><span data-stu-id="73b9b-224">Here is how the top-level Resource Manager template for a data factory looks like:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="73b9b-225">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="73b9b-225">Define data factory</span></span>
<span data-ttu-id="73b9b-226">A data factoryt a Resource Manager-sablonban definiálhatja az alábbi minta szerint:</span><span class="sxs-lookup"><span data-stu-id="73b9b-226">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
<span data-ttu-id="73b9b-227">A dataFactoryName, adja meg a sablon telepítésekor adat-előállító nevét.</span><span class="sxs-lookup"><span data-stu-id="73b9b-227">The dataFactoryName is the name of the data factory you specify when you deploy the template.</span></span> <span data-ttu-id="73b9b-228">Adat-előállító jelenleg csak az USA keleti régiója, USA nyugati régiója és Észak-Európa régiókban támogatott.</span><span class="sxs-lookup"><span data-stu-id="73b9b-228">Data factory is currently only supported in the East US, West US, and North Europe regions.</span></span>

### <a name="defining-entities-within-the-data-factory"></a><span data-ttu-id="73b9b-229">Az adat-előállítóban belüli definiálása</span><span class="sxs-lookup"><span data-stu-id="73b9b-229">Defining entities within the data factory</span></span>
<span data-ttu-id="73b9b-230">Az alábbi Data Factory-entitások a JSON-sablonban vannak definiálva:</span><span class="sxs-lookup"><span data-stu-id="73b9b-230">The following Data Factory entities are defined in the JSON template:</span></span>

* [<span data-ttu-id="73b9b-231">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73b9b-231">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="73b9b-232">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73b9b-232">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="73b9b-233">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="73b9b-233">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="73b9b-234">Azure blobkimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="73b9b-234">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="73b9b-235">Másolási tevékenységgel rendelkező adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="73b9b-235">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="73b9b-236">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73b9b-236">Azure Storage linked service</span></span>
<span data-ttu-id="73b9b-237">Az Azure Storage társított szolgáltatás az Azure tárfiókot társítja az adat-előállítóval.</span><span class="sxs-lookup"><span data-stu-id="73b9b-237">The Azure Storage linked service links your Azure storage account to the data factory.</span></span> <span data-ttu-id="73b9b-238">Ebben az oktatóanyagban ugyanabban a tárfiókban lesz az alapértelmezett HDInsight tárfiókot, a bemeneti adatokat tároló és a kimeneti adatok tárolási.</span><span class="sxs-lookup"><span data-stu-id="73b9b-238">In this tutorial, the same storage account is used as the default HDInsight storage account, input data storage, and output data storage.</span></span> <span data-ttu-id="73b9b-239">Ezért adja meg csak egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="73b9b-239">Therefore, you define only one Azure Storage linked service.</span></span> <span data-ttu-id="73b9b-240">A társított szolgáltatás definíciójának neve és kulcsa az Azure storage-fiókot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="73b9b-240">In the linked service definition, you specify the name and key of your Azure storage account.</span></span> <span data-ttu-id="73b9b-241">Az Azure Storage társított szolgáltatás definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Storage társított szolgáltatás](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-241">See [Azure Storage linked service](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span>

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
<span data-ttu-id="73b9b-242">A **connectionString** a storageAccountName és storageAccountKey paramétereket használja.</span><span class="sxs-lookup"><span data-stu-id="73b9b-242">The **connectionString** uses the storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="73b9b-243">A sablon telepítése során adja meg a paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="73b9b-243">You specify values for these parameters while deploying the template.</span></span>  

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="73b9b-244">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="73b9b-244">HDInsight on-demand linked service</span></span>
<span data-ttu-id="73b9b-245">Igény szerinti HDInsight kapcsolódó szolgáltatásdefinícióban meg futásidőben egy HDInsight Hadoop-fürt létrehozása a Data Factory szolgáltatásnak által használt konfigurációs paraméterek értékét.</span><span class="sxs-lookup"><span data-stu-id="73b9b-245">In the on-demand HDInsight linked service definition, you specify values for configuration parameters that are used by the Data Factory service to create a HDInsight Hadoop cluster at runtime.</span></span> <span data-ttu-id="73b9b-246">A HDInsight igény szerinti társított szolgáltatás definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg a [Számítási társított szolgáltatás](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) című cikket.</span><span class="sxs-lookup"><span data-stu-id="73b9b-246">See [Compute linked services](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used to define an HDInsight on-demand linked service.</span></span>  

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
<span data-ttu-id="73b9b-247">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="73b9b-247">Note the following points:</span></span>

* <span data-ttu-id="73b9b-248">A Data Factory létrehoz egy **Linux-alapú** meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="73b9b-248">The Data Factory creates a **Linux-based** HDInsight cluster for you.</span></span>
* <span data-ttu-id="73b9b-249">A HDInsight Hadoop-fürt és a tárfiók ugyanabban a régióban jön létre.</span><span class="sxs-lookup"><span data-stu-id="73b9b-249">The HDInsight Hadoop cluster is created in the same region as the storage account.</span></span>
* <span data-ttu-id="73b9b-250">Figyelje meg a *timeToLive* beállítást.</span><span class="sxs-lookup"><span data-stu-id="73b9b-250">Notice the *timeToLive* setting.</span></span> <span data-ttu-id="73b9b-251">A data factory automatikusan törli a fürt, miután a fürt 30 percig inaktív alatt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-251">The data factory deletes the cluster automatically after the cluster is being idle for 30 minutes.</span></span>
* <span data-ttu-id="73b9b-252">A HDInsight-fürt létrehoz egy **alapértelmezett tárolót** a JSON-fájlban megadott blob-tárolóban (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="73b9b-252">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="73b9b-253">A fürt törlésekor a HDInsight nem törli ezt a tárolót.</span><span class="sxs-lookup"><span data-stu-id="73b9b-253">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="73b9b-254">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="73b9b-254">This behavior is by design.</span></span> <span data-ttu-id="73b9b-255">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer mindig létrehoz egy HDInsight-fürt, amikor fel kell dolgozni egy szeletet, kivéve, ha van meglévő élő fürt (**timeToLive**), majd a feldolgozás végén a rendszer törli a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-255">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (**timeToLive**) and is deleted when the processing is done.</span></span>

<span data-ttu-id="73b9b-256">További információkért lásd: [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="73b9b-256">See [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73b9b-257">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-257">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="73b9b-258">Ha nincs szüksége rájuk a feladatokkal kapcsolatos hibaelhárításhoz, törölheti őket a tárolási költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="73b9b-258">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="73b9b-259">A tárolók neve a következő mintát követi: „adf**yourdatafactoryname**-**linkedservicename**-datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="73b9b-259">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="73b9b-260">Az Azure Blob Storage-tárból olyan eszközökkel törölheti a tárolókat, mint például a [Microsoft Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="73b9b-260">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="73b9b-261">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="73b9b-261">Azure blob input dataset</span></span>
<span data-ttu-id="73b9b-262">A bemeneti adatkészlet-definícióban blob tároló mappa és a bemeneti adatokat tartalmazó fájl nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="73b9b-262">In the input dataset definition, you specify the names of blob container, folder, and file that contains the input data.</span></span> <span data-ttu-id="73b9b-263">Az Azure Blob-adatkészletek definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Blob-adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-263">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>

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

<span data-ttu-id="73b9b-264">Figyelje meg a JSON-definícióból adott alábbi beállításait:</span><span class="sxs-lookup"><span data-stu-id="73b9b-264">Notice the following specific settings in the JSON definition:</span></span>

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="73b9b-265">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="73b9b-265">Azure Blob output dataset</span></span>
<span data-ttu-id="73b9b-266">A kimeneti adatkészlet-definícióban és a blob tároló a kimeneti adatokat tartalmazó mappa nevét kell megadni.</span><span class="sxs-lookup"><span data-stu-id="73b9b-266">In the output dataset definition, you specify the names of blob container and folder that holds the output data.</span></span> <span data-ttu-id="73b9b-267">Az Azure Blob-adatkészletek definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Blob-adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-267">See [Azure Blob dataset properties](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>  

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

<span data-ttu-id="73b9b-268">A folderPath határozza meg a mappa elérési útját, amely tárolja az adatokat:</span><span class="sxs-lookup"><span data-stu-id="73b9b-268">The folderPath specifies the path to the folder that holds the output data:</span></span>

```json
"folderPath": "adfgetstarted/partitioneddata",
```

<span data-ttu-id="73b9b-269">A [adatkészlet rendelkezésre állási](../data-factory/data-factory-create-datasets.md#dataset-availability) beállítás a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="73b9b-269">The [dataset availability](../data-factory/data-factory-create-datasets.md#dataset-availability) setting is as follows:</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

<span data-ttu-id="73b9b-270">Az Azure Data Factoryben kimeneti adatkészlet rendelkezésre állási meghajtók a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="73b9b-270">In Azure Data Factory, output dataset availability drives the pipeline.</span></span> <span data-ttu-id="73b9b-271">Ebben a példában a szelet (EndOfInterval) hónap utolsó napján havonta jön létre.</span><span class="sxs-lookup"><span data-stu-id="73b9b-271">In this example, the slice is produced monthly on the last day of month (EndOfInterval).</span></span> <span data-ttu-id="73b9b-272">További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-272">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

#### <a name="data-pipeline"></a><span data-ttu-id="73b9b-273">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="73b9b-273">Data pipeline</span></span>
<span data-ttu-id="73b9b-274">Megadhatja egy folyamatot, amely átalakítja az adatok igény szerinti Azure HDInsight-fürtök a Hive parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="73b9b-274">You define a pipeline that transforms data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="73b9b-275">A példában található folyamat definiálásához használt JSON-elemek leírásához tekintse meg [A folyamat JSON-fájlja](../data-factory/data-factory-create-pipelines.md#pipeline-json) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-275">See [Pipeline JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used to define a pipeline in this example.</span></span>

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

<span data-ttu-id="73b9b-276">A folyamat egy tevékenységet, HDInsightHive tevékenységet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="73b9b-276">The pipeline contains one activity, HDInsightHive activity.</span></span> <span data-ttu-id="73b9b-277">Mivel a kezdő és záró dátumát 2016. január adatokat (a szeletek) feldolgozva csak egy hónap.</span><span class="sxs-lookup"><span data-stu-id="73b9b-277">As both start and end dates are in January 2016, data for only one month (a slice) is processed.</span></span> <span data-ttu-id="73b9b-278">Mindkét *start* és *end* múltbeli dátum, a tevékenység rendelkezik, így a Data Factory az adatokat közvetlenül az adott hónap dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="73b9b-278">Both *start* and *end* of the activity have a past date, so the Data Factory processes data for the month immediately.</span></span> <span data-ttu-id="73b9b-279">Ha vége a jövőben, az adat-előállítóban létrehoz egy másik szelet Ha idő.</span><span class="sxs-lookup"><span data-stu-id="73b9b-279">If the end is a future date, the data factory creates another slice when the time comes.</span></span> <span data-ttu-id="73b9b-280">További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-280">For more information, see [Data Factory Scheduling and Execution](../data-factory/data-factory-scheduling-and-execution.md).</span></span>

## <a name="clean-up-the-tutorial"></a><span data-ttu-id="73b9b-281">Az oktatóanyag tartalmának törlése</span><span class="sxs-lookup"><span data-stu-id="73b9b-281">Clean up the tutorial</span></span>

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a><span data-ttu-id="73b9b-282">A blob-tárolók hozta létre igény szerinti HDInsight-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="73b9b-282">Delete the blob containers created by on-demand HDInsight cluster</span></span>
<span data-ttu-id="73b9b-283">Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén egy HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz (élettartam); és a fürt akkor törlődnek, ha a feldolgozás történik.</span><span class="sxs-lookup"><span data-stu-id="73b9b-283">With on-demand HDInsight linked service, an HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (timeToLive); and the cluster is deleted when the processing is done.</span></span> <span data-ttu-id="73b9b-284">Az egyes fürtökön Azure Data Factory egy blob-tároló az Azure blob storage a fürthöz az alapértelmezett stroage fiókként használt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="73b9b-284">For each cluster, Azure Data Factory creates a blob container in the Azure blob storage used as the default stroage account for the cluster.</span></span> <span data-ttu-id="73b9b-285">Annak ellenére, hogy a HDInsight-fürtök törlése az alapértelmezett blob tárolókat, és a kapcsolódó tárfiók nem törlődik.</span><span class="sxs-lookup"><span data-stu-id="73b9b-285">Even though HDInsight cluster is deleted, the default blob storage container and the associated storage account are not deleted.</span></span> <span data-ttu-id="73b9b-286">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="73b9b-286">This behavior is by design.</span></span> <span data-ttu-id="73b9b-287">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-287">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="73b9b-288">Ha nincs szüksége rájuk a feladatokkal kapcsolatos hibaelhárításhoz, törölheti őket a tárolási költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="73b9b-288">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="73b9b-289">A tárolók neve a következő mintát követi: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="73b9b-289">The names of these containers follow a pattern: `adfyourdatafactoryname-linkedservicename-datetimestamp`.</span></span>

<span data-ttu-id="73b9b-290">Törölje a **adfjobs** és **adfyourdatafactoryname-linkedservicename-datetimestamp** mappák.</span><span class="sxs-lookup"><span data-stu-id="73b9b-290">Delete the **adfjobs** and **adfyourdatafactoryname-linkedservicename-datetimestamp** folders.</span></span> <span data-ttu-id="73b9b-291">A adfjobs tároló feladatnaplóit az Azure Data Factory tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="73b9b-291">The adfjobs container contains job logs from Azure Data Factory.</span></span>

### <a name="delete-the-resource-group"></a><span data-ttu-id="73b9b-292">Az erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="73b9b-292">Delete the resource group</span></span>
<span data-ttu-id="73b9b-293">[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) központi telepítése, kezelése és a megoldás csoportként figyelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="73b9b-293">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) is used to deploy, manage, and monitor your solution as a group.</span></span>  <span data-ttu-id="73b9b-294">Erőforráscsoport törlése összetevőit a csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="73b9b-294">Deleting a resource group deletes all the components inside the group.</span></span>  

1. <span data-ttu-id="73b9b-295">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="73b9b-295">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="73b9b-296">Kattintson a **erőforráscsoportok** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="73b9b-296">Click **Resource groups** on the left pane.</span></span>
3. <span data-ttu-id="73b9b-297">Kattintson a PowerShell parancsfájl létrehozott az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="73b9b-297">Click the resource group name you created in your PowerShell script.</span></span> <span data-ttu-id="73b9b-298">A szűrő használja, ha túl sok erőforrás-csoportok szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="73b9b-298">Use the filter if you have too many resource groups listed.</span></span> <span data-ttu-id="73b9b-299">Az erőforráscsoport egy új panelen nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="73b9b-299">It opens the resource group in a new blade.</span></span>
4. <span data-ttu-id="73b9b-300">Az a **erőforrások** csempe, akkor az alapértelmezett tárfiók és rendelkezik a data factory, kivéve, ha az erőforráscsoport megosztása más projektek felsorolt.</span><span class="sxs-lookup"><span data-stu-id="73b9b-300">On the **Resources** tile, you shall have the default storage account and the data factory listed unless you share the resource group with other projects.</span></span>
5. <span data-ttu-id="73b9b-301">Kattintson a **törlése** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="73b9b-301">Click **Delete** on the top of the blade.</span></span> <span data-ttu-id="73b9b-302">Ezzel törli a tárfiókot és a storage-fiókban tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="73b9b-302">Doing so deletes the storage account and the data stored in the storage account.</span></span>
6. <span data-ttu-id="73b9b-303">Adja meg a törlés jóváhagyásához, és kattintson az erőforráscsoport neve **törlése**.</span><span class="sxs-lookup"><span data-stu-id="73b9b-303">Enter the resource group name to confirm deletion, and then click **Delete**.</span></span>

<span data-ttu-id="73b9b-304">Abban az esetben, ha nem szeretné törölni a tárfiókot, ha törli az erőforráscsoportot, fontolja meg a következő architektúra által az üzleti adatok mappától az alapértelmezett tárfiók.</span><span class="sxs-lookup"><span data-stu-id="73b9b-304">In case you don't want to delete the storage account when you delete the resource group, consider the following architecture by separating the business data from the default storage account.</span></span> <span data-ttu-id="73b9b-305">Ebben az esetben egy erőforráscsoportot a tárfiók az üzleti adatok rendelkezik, és az alapértelmezett tárfiókot, a HDInsight tartozó többi erőforráscsoport kapcsolódó szolgáltatás és az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="73b9b-305">In this case, you have one resource group for the storage account with the business data, and the other resource group for the default storage account for HDInsight linked service and the data factory.</span></span> <span data-ttu-id="73b9b-306">A második erőforráscsoport törlése nem érinti az üzleti adatok tárfiók.</span><span class="sxs-lookup"><span data-stu-id="73b9b-306">When you delete the second resource group, it does not impact the business data storage account.</span></span> <span data-ttu-id="73b9b-307">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="73b9b-307">To do so:</span></span>

* <span data-ttu-id="73b9b-308">Adja hozzá a következő a legfelső szintű erőforráscsoporttal együtt a Resource Manager-sablon az Microsoft.DataFactory/datafactories erőforrás.</span><span class="sxs-lookup"><span data-stu-id="73b9b-308">Add the following to the top-level resource group along with the Microsoft.DataFactory/datafactories resource in your Resource Manager template.</span></span> <span data-ttu-id="73b9b-309">Létrehoz egy tárfiókot:</span><span class="sxs-lookup"><span data-stu-id="73b9b-309">It creates a storage account:</span></span>

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
* <span data-ttu-id="73b9b-310">Vegyen fel egy új társított szolgáltatás pontot az új tárolási fiók:</span><span class="sxs-lookup"><span data-stu-id="73b9b-310">Add a new linked service point to the new storage account:</span></span>

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
* <span data-ttu-id="73b9b-311">Konfigurálja a HDInsight kapcsolódó ondemand-szolgáltatás egy további dependsOn és egy additionalLinkedServiceNames:</span><span class="sxs-lookup"><span data-stu-id="73b9b-311">Configure the HDInsight ondemand linked service with an additional dependsOn and an additionalLinkedServiceNames:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="73b9b-312">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73b9b-312">Next steps</span></span>
<span data-ttu-id="73b9b-313">Ebben a cikkben rendelkezik megtanulta, hogyan használható az Azure Data Factory igény szerinti HDInsight-fürt feldolgozni a Hive-feladatok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="73b9b-313">In this article, you have learned how to use Azure Data Factory to create on-demand HDInsight cluster to process Hive jobs.</span></span> <span data-ttu-id="73b9b-314">Ha többet:</span><span class="sxs-lookup"><span data-stu-id="73b9b-314">To read more:</span></span>

* [<span data-ttu-id="73b9b-315">Hadoop oktatóanyag: hdinsight Linux-alapú Hadoop használatának megkezdése</span><span class="sxs-lookup"><span data-stu-id="73b9b-315">Hadoop tutorial: Get started using Linux-based Hadoop in HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="73b9b-316">Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="73b9b-316">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="73b9b-317">HDInsight-dokumentáció</span><span class="sxs-lookup"><span data-stu-id="73b9b-317">HDInsight documentation</span></span>](https://azure.microsoft.com/documentation/services/hdinsight/)
* [<span data-ttu-id="73b9b-318">Data factory dokumentáció</span><span class="sxs-lookup"><span data-stu-id="73b9b-318">Data factory documentation</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a><span data-ttu-id="73b9b-319">Függelék:</span><span class="sxs-lookup"><span data-stu-id="73b9b-319">Appendix</span></span>

### <a name="azure-cli-script"></a><span data-ttu-id="73b9b-320">Az Azure CLI-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="73b9b-320">Azure CLI script</span></span>
<span data-ttu-id="73b9b-321">Az oktatóanyag elvégzéséhez Azure PowerShell használata helyett az Azure parancssori felület is használhatja.</span><span class="sxs-lookup"><span data-stu-id="73b9b-321">You can use Azure CLI instead of using Azure PowerShell to do the tutorial.</span></span> <span data-ttu-id="73b9b-322">Az Azure parancssori felület használatához először telepítenie kell az Azure parancssori felület szerint az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="73b9b-322">To use Azure CLI, first install Azure CLI as per the following instructions:</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a><span data-ttu-id="73b9b-323">A tárterület előkészítése, és másolja a fájlokat az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="73b9b-323">Use Azure CLI to prepare the storage and copy the files</span></span>

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

<span data-ttu-id="73b9b-324">A tároló neve *adfgetstarted*.</span><span class="sxs-lookup"><span data-stu-id="73b9b-324">The container name is *adfgetstarted*.</span></span> <span data-ttu-id="73b9b-325">Legyen ez.</span><span class="sxs-lookup"><span data-stu-id="73b9b-325">Keep it as it is.</span></span> <span data-ttu-id="73b9b-326">Ellenkező esetben frissítenie kell a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="73b9b-326">Otherwise you need to update the Resource Manager template.</span></span> <span data-ttu-id="73b9b-327">Ha a parancssori felület parancsfájllal segítségre van szüksége, tekintse meg [az Azure parancssori felület használatával az Azure Storage](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="73b9b-327">If you need help with this CLI script, see [Using the Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>
