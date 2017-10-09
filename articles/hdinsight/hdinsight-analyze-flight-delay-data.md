---
title: "aaaAnalyze repülési késleltetés adatok hadooppal a Hdinsightban - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy Windows PowerShell parancsfájl toocreate HDInsight-fürtöt, a Hive feladat futtatása Sqoop feladatot futtatni, és hello fürt törlése."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6ebaee65d9b270e5dc2141dd1265011d372f497d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a><span data-ttu-id="f9df1-103">Repülési késleltetés adatok elemzése a Hive HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="f9df1-103">Analyze flight delay data by using Hive in HDInsight</span></span>
<span data-ttu-id="f9df1-104">Hive lehetővé teszi egy SQL-szerű nevű programozási nyelv használatával a feladatok Hadoop MapReduce futó  *[HiveQL][hadoop-hiveql]*, amelyek alkalmazhatók felé összefoglalójához, kérdez le, és nagy mennyiségű adatot elemzése.</span><span class="sxs-lookup"><span data-stu-id="f9df1-104">Hive provides a means of running Hadoop MapReduce jobs through an SQL-like scripting language called *[HiveQL][hadoop-hiveql]*, which can be applied towards summarizing, querying, and analyzing large volumes of data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9df1-105">hello jelen dokumentumban leírt lépések szükséges egy Windows-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-105">hello steps in this document require a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="f9df1-106">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f9df1-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f9df1-107">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f9df1-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="f9df1-108">A Linux-alapú fürt lépéseiért lásd: [repülési késleltetés adatok elemzése a Hive HDInsight (Linux) használatával](hdinsight-analyze-flight-delay-data-linux.md).</span><span class="sxs-lookup"><span data-stu-id="f9df1-108">For steps that work with a Linux-based cluster, see [Analyze flight delay data by using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).</span></span>

<span data-ttu-id="f9df1-109">Adatok tárolási és számítási hello elkülönítése egyik fő előnye hello Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f9df1-109">One of hello major benefits of Azure HDInsight is hello separation of data storage and compute.</span></span> <span data-ttu-id="f9df1-110">HDInsight Azure Blob storage-tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="f9df1-110">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="f9df1-111">Egy tipikus munkába beletartozik a három részből áll:</span><span class="sxs-lookup"><span data-stu-id="f9df1-111">A typical job involves three parts:</span></span>

1. <span data-ttu-id="f9df1-112">**Adatok tárolása az Azure Blob Storage tárolóban.**</span><span class="sxs-lookup"><span data-stu-id="f9df1-112">**Store data in Azure Blob storage.**</span></span>  <span data-ttu-id="f9df1-113">Például időjárás adatok, érzékelőadatait, webes naplókat, és ebben az esetben repülési késleltetés adatok menti azokat Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f9df1-113">For example, weather data, sensor data, web logs, and in this case, flight delay data are saved into Azure Blob storage.</span></span>
2. <span data-ttu-id="f9df1-114">**Feladatok futtatása.**</span><span class="sxs-lookup"><span data-stu-id="f9df1-114">**Run jobs.**</span></span> <span data-ttu-id="f9df1-115">Ha tooprocess hello időadatok, futtatja egy Windows PowerShell-parancsfájlt (vagy egy ügyfélalkalmazás) toocreate HDInsight-fürtöt, feladatok futtatása, és hello fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="f9df1-115">When it is time tooprocess hello data, you run a Windows PowerShell script (or a client application) toocreate an HDInsight cluster, run jobs, and delete hello cluster.</span></span> <span data-ttu-id="f9df1-116">Mentse a kimeneti adatok tooAzure Blob-tároló hello feladatok.</span><span class="sxs-lookup"><span data-stu-id="f9df1-116">hello jobs save output data tooAzure Blob storage.</span></span> <span data-ttu-id="f9df1-117">hello kimeneti adatok hello fürtök törlése után is megmarad.</span><span class="sxs-lookup"><span data-stu-id="f9df1-117">hello output data is retained even after hello cluster is deleted.</span></span> <span data-ttu-id="f9df1-118">Ezzel a módszerrel kell fizetnie csak néhány Ön által igénybe vett.</span><span class="sxs-lookup"><span data-stu-id="f9df1-118">This way, you pay for only what you have consumed.</span></span>
3. <span data-ttu-id="f9df1-119">**Hello kimeneti beolvasása az Azure Blob storage**, vagy az oktatóanyagban hello adatok tooan Azure SQL adatbázis-exportálási.</span><span class="sxs-lookup"><span data-stu-id="f9df1-119">**Retrieve hello output from Azure Blob storage**, or in this tutorial, export hello data tooan Azure SQL database.</span></span>

<span data-ttu-id="f9df1-120">hello következő diagram azt ábrázolja, hello forgatókönyv és ebben az oktatóanyagban hello szerkezete:</span><span class="sxs-lookup"><span data-stu-id="f9df1-120">hello following diagram illustrates hello scenario and hello structure of this tutorial:</span></span>

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

<span data-ttu-id="f9df1-122">Vegye figyelembe, hogy hello számok hello diagramon megfelelnek-e toohello szakaszok címét.</span><span class="sxs-lookup"><span data-stu-id="f9df1-122">Note that hello numbers in hello diagram correspond toohello section titles.</span></span> <span data-ttu-id="f9df1-123">**M** hello fő folyamat jelenti.</span><span class="sxs-lookup"><span data-stu-id="f9df1-123">**M** stands for hello main process.</span></span> <span data-ttu-id="f9df1-124">**A** hello tartalom hello függelékben jelenti.</span><span class="sxs-lookup"><span data-stu-id="f9df1-124">**A** stands for hello content in hello appendix.</span></span>

<span data-ttu-id="f9df1-125">hello fő része hello az oktatóanyag bemutatja, hogyan toouse egy Windows PowerShell parancsfájl tooperform hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="f9df1-125">hello main portion of hello tutorial shows you how toouse one Windows PowerShell script tooperform hello following tasks:</span></span>

* <span data-ttu-id="f9df1-126">HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f9df1-126">Create an HDInsight cluster.</span></span>
* <span data-ttu-id="f9df1-127">Hello fürt toocalculate átlagos késleltetése repülőtereken egy Hive-feladat fut.</span><span class="sxs-lookup"><span data-stu-id="f9df1-127">Run a Hive job on hello cluster toocalculate average delays at airports.</span></span> <span data-ttu-id="f9df1-128">egy Azure Blob storage-fiók hello repülési késleltetés adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="f9df1-128">hello flight delay data is stored in an Azure Blob storage account.</span></span>
* <span data-ttu-id="f9df1-129">Sqoop feladat tooexport hello Hive feladat kimeneti tooan Azure SQL-adatbázis futtatásához.</span><span class="sxs-lookup"><span data-stu-id="f9df1-129">Run a Sqoop job tooexport hello Hive job output tooan Azure SQL database.</span></span>
* <span data-ttu-id="f9df1-130">Hello HDInsight-fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="f9df1-130">Delete hello HDInsight cluster.</span></span>

<span data-ttu-id="f9df1-131">Hello függelékben találhat repülési késleltetés adatok feltöltése, a Hive lekérdezési karakterlánc létrehozása/feltöltése és hello Azure SQL-adatbázis előkészítése hello Sqoop feladat hello útmutatót.</span><span class="sxs-lookup"><span data-stu-id="f9df1-131">In hello appendixes, you can find hello instructions for uploading flight delay data, creating/uploading a Hive query string, and preparing hello Azure SQL database for hello Sqoop job.</span></span>

> [!NOTE]
> <span data-ttu-id="f9df1-132">hello jelen dokumentumban leírt lépések olyan adott tooWindows-alapú HDInsight-fürtök esetében.</span><span class="sxs-lookup"><span data-stu-id="f9df1-132">hello steps in this document are specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="f9df1-133">A Linux-alapú fürt lépéseiért lásd: [adatelemzés repülési késleltetés segítségével Hive HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span><span class="sxs-lookup"><span data-stu-id="f9df1-133">For steps that work with a Linux-based cluster, see [Analyze flight delay data using Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f9df1-134">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f9df1-134">Prerequisites</span></span>
<span data-ttu-id="f9df1-135">Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="f9df1-135">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="f9df1-136">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="f9df1-136">**An Azure subscription**.</span></span> <span data-ttu-id="f9df1-137">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f9df1-137">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="f9df1-138">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="f9df1-138">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f9df1-139">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-139">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="f9df1-140">a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="f9df1-140">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="f9df1-141">Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója.</span><span class="sxs-lookup"><span data-stu-id="f9df1-141">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="f9df1-142">Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-142">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

<span data-ttu-id="f9df1-143">**Ebben az oktatóanyagban használt fájlok**</span><span class="sxs-lookup"><span data-stu-id="f9df1-143">**Files used in this tutorial**</span></span>

<span data-ttu-id="f9df1-144">Ez az oktatóanyag használja hello időben teljesítményének légitársaság felé továbbított adatok [kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda vagy RITA][rita-website].</span><span class="sxs-lookup"><span data-stu-id="f9df1-144">This tutorial uses hello on-time performance of airline flight data from [Research and Innovative Technology Administration, Bureau of Transportation Statistics or RITA][rita-website].</span></span>
<span data-ttu-id="f9df1-145">Hello adatok másolatát feltöltött tooan Azure Blob storage tárolók hello nyilvános Blob hozzáférési engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="f9df1-145">A copy of hello data has been uploaded tooan Azure Blob storage container with hello Public Blob access permission.</span></span>
<span data-ttu-id="f9df1-146">A PowerShell parancsfájl részét hello adatok hello nyilvános blob tárolóban toohello alapértelmezett blob tároló a fürt másolja át.</span><span class="sxs-lookup"><span data-stu-id="f9df1-146">A part of your PowerShell script copies hello data from hello public blob container toohello default blob container of your cluster.</span></span> <span data-ttu-id="f9df1-147">hello HiveQL-parancsfájlt is van másolt toohello Blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="f9df1-147">hello HiveQL script is also copied toohello same Blob container.</span></span>
<span data-ttu-id="f9df1-148">Ha azt szeretné, hogy toolearn hogyan tooget/feltöltés hello adatok tooyour saját tárfiókot, és hogyan toocreate/feltöltés hello HiveQL parancsfájl-fájlt, tekintse meg a [függelék](#appendix-a) és [B függelék](#appendix-b).</span><span class="sxs-lookup"><span data-stu-id="f9df1-148">If you want toolearn how tooget/upload hello data tooyour own Storage account, and how toocreate/upload hello HiveQL script file, see [Appendix A](#appendix-a) and [Appendix B](#appendix-b).</span></span>

<span data-ttu-id="f9df1-149">hello következő táblázat sorolja fel ebben az oktatóanyagban használt hello fájlok:</span><span class="sxs-lookup"><span data-stu-id="f9df1-149">hello following table lists hello files used in this tutorial:</span></span>

<table border="1">
<tr><th><span data-ttu-id="f9df1-150">Fájlok</span><span class="sxs-lookup"><span data-stu-id="f9df1-150">Files</span></span></th><th><span data-ttu-id="f9df1-151">Leírás</span><span class="sxs-lookup"><span data-stu-id="f9df1-151">Description</span></span></th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td><span data-ttu-id="f9df1-152">hello Hive feladat használják hello HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-152">hello HiveQL script file used by hello Hive job.</span></span> <span data-ttu-id="f9df1-153">Ez a parancsfájl lett feltöltött tooan hello nyilvános hozzáférés az Azure Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f9df1-153">This script has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="f9df1-154"><a href="#appendix-b">B függelék</a> előkészítése, majd ismét feltölteni a fájlt tooyour saját Azure Blob storage-fiók utasításokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f9df1-154"><a href="#appendix-b">Appendix B</a> has instructions on preparing and uploading this file tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td><span data-ttu-id="f9df1-155">A bemeneti adatok hello Hive feladat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-155">Input data for hello Hive job.</span></span> <span data-ttu-id="f9df1-156">hello már feltöltött tooan hello nyilvános hozzáférés az Azure Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f9df1-156">hello data has been uploaded tooan Azure Blob storage account with hello public access.</span></span> <span data-ttu-id="f9df1-157"><a href="#appendix-a">A függelék</a> utasításokat rendelkezzen hello adatokat, majd ismét feltölteni a hello adatok tooyour saját Azure Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f9df1-157"><a href="#appendix-a">Appendix A</a> has instructions on getting hello data and uploading hello data tooyour own Azure Blob storage account.</span></span></td></tr>
<tr><td><span data-ttu-id="f9df1-158">\tutorials\flightdelays\output</span><span class="sxs-lookup"><span data-stu-id="f9df1-158">\tutorials\flightdelays\output</span></span></td><td><span data-ttu-id="f9df1-159">hello kimeneti elérési út hello Hive feladat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-159">hello output path for hello Hive job.</span></span> <span data-ttu-id="f9df1-160">hello alapértelmezett tároló hello kimeneti adatok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="f9df1-160">hello default container is used for storing hello output data.</span></span></td></tr>
<tr><td><span data-ttu-id="f9df1-161">\tutorials\flightdelays\jobstatus</span><span class="sxs-lookup"><span data-stu-id="f9df1-161">\tutorials\flightdelays\jobstatus</span></span></td><td><span data-ttu-id="f9df1-162">hello Hive feladat állapota mappájába hello alapértelmezett tárolót.</span><span class="sxs-lookup"><span data-stu-id="f9df1-162">hello Hive job status folder on hello default container.</span></span></td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a><span data-ttu-id="f9df1-163">Fürt létrehozása és Hive/Sqoop feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="f9df1-163">Create cluster and run Hive/Sqoop jobs</span></span>
<span data-ttu-id="f9df1-164">Hadoop-MapReduce kötegelt feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="f9df1-164">Hadoop MapReduce is batch processing.</span></span> <span data-ttu-id="f9df1-165">hello legtöbb költséghatékony toorun egy Hive-feladat toocreate egy fürt hello feladat, és hello feladat törlése hello feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="f9df1-165">hello most cost-effective way toorun a Hive job is toocreate a cluster for hello job, and delete hello job after hello job is completed.</span></span> <span data-ttu-id="f9df1-166">a következő parancsfájl magában foglalja az hello teljes folyamat hello.</span><span class="sxs-lookup"><span data-stu-id="f9df1-166">hello following script covers hello whole process.</span></span>
<span data-ttu-id="f9df1-167">A HDInsight-fürtök létrehozása és Hive-feladatok futtatása további információkért lásd: [Hadoop létrehozása a HDInsight-fürtök] [ hdinsight-provision] és [használata a HDInsight Hive] [hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="f9df1-167">For more information on creating an HDInsight cluster and running Hive jobs, see [Create Hadoop clusters in HDInsight][hdinsight-provision] and [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

<span data-ttu-id="f9df1-168">**toorun hello Hive-lekérdezések Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="f9df1-168">**toorun hello Hive queries by Azure PowerShell**</span></span>

1. <span data-ttu-id="f9df1-169">Hozzon létre egy Azure SQL database és hello tábla hello Sqoop feladat kimeneti hello utasításait [C függelék](#appendix-c).</span><span class="sxs-lookup"><span data-stu-id="f9df1-169">Create an Azure SQL database and hello table for hello Sqoop job output by using hello instructions in [Appendix C](#appendix-c).</span></span>
2. <span data-ttu-id="f9df1-170">Nyissa meg a Windows PowerShell ISE, és a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-170">Open Windows PowerShell ISE, and run hello following script:</span></span>

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure hello follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on hello version, hello folder can be different

    ###########################################
    # (Optional) configure hello following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter hello Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use hello cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare hello HiveQL script and source data
    ###########################################

    # Create hello temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download hello sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data toodefault container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit hello Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit hello Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete hello cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. <span data-ttu-id="f9df1-171">Csatlakozás tooyour SQL-adatbázis, és átlagos repülési késések hello AvgDelays tábla városonként lásd:</span><span class="sxs-lookup"><span data-stu-id="f9df1-171">Connect tooyour SQL database and see average flight delays by city in hello AvgDelays table:</span></span>

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <span data-ttu-id="f9df1-173"><a id="appendix-a"></a>A függelék – feltöltés repülési késleltetés adatok tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="f9df1-173"><a id="appendix-a"></a>Appendix A - Upload flight delay data tooAzure Blob storage</span></span>
<span data-ttu-id="f9df1-174">Hello adatfájl és -hello HiveQL parancsfájl fájlok feltöltése (lásd: [B függelék](#appendix-b)) is szükség van néhány.</span><span class="sxs-lookup"><span data-stu-id="f9df1-174">Uploading hello data file and hello HiveQL script files (see [Appendix B](#appendix-b)) requires some planning.</span></span> <span data-ttu-id="f9df1-175">hello lényege toostore hello az adatfájlok és hello HiveQL fájl HDInsight fürtök létrehozásával és hello Hive feladat futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-175">hello idea is toostore hello data files and hello HiveQL file before creating an HDInsight cluster and running hello Hive job.</span></span> <span data-ttu-id="f9df1-176">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="f9df1-176">You have two options:</span></span>

* <span data-ttu-id="f9df1-177">**Használja az azonos hello hello alapértelmezett fájlrendszer hello HDInsight-fürt által használandó Azure Storage-fiók.**</span><span class="sxs-lookup"><span data-stu-id="f9df1-177">**Use hello same Azure Storage account that will be used by hello HDInsight cluster as hello default file system.**</span></span> <span data-ttu-id="f9df1-178">Mivel a HDInsight-fürt hello hello tárfiók_elérési_kulcsa lesz, a további módosításokat toomake nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f9df1-178">Because hello HDInsight cluster will have hello Storage account access key, you don't need toomake any additional changes.</span></span>
* <span data-ttu-id="f9df1-179">**Hello HDInsight fürt alapértelmezett fájlrendszer egy másik Azure Storage-fiókot használni.**</span><span class="sxs-lookup"><span data-stu-id="f9df1-179">**Use a different Azure Storage account from hello HDInsight cluster default file system.**</span></span> <span data-ttu-id="f9df1-180">Ha ez helyzet hello, módosítania kell a Windows PowerShell parancsfájl található hello hello létrehozása részét [hozzon létre HDInsight-fürtöt és futtatási Hive/Sqoop feladatok](#runjob) toolink hello tárfiók egy további tárfiókkal.</span><span class="sxs-lookup"><span data-stu-id="f9df1-180">If this is hello case, you must modify hello creation part of hello Windows PowerShell script found in [Create HDInsight cluster and run Hive/Sqoop jobs](#runjob) toolink hello Storage account as an additional Storage account.</span></span> <span data-ttu-id="f9df1-181">Útmutatásért lásd: [Hadoop létrehozása a HDInsight-fürtök][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="f9df1-181">For instructions, see [Create Hadoop clusters in HDInsight][hdinsight-provision].</span></span> <span data-ttu-id="f9df1-182">hello HDInsight-fürt majd tudja hello tárfiók hello a hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f9df1-182">hello HDInsight cluster then knows hello access key for hello Storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="f9df1-183">hello Blob storage a hello adatfájl elérési út rögzített a kódolt hello HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-183">hello Blob storage path for hello data file is hard coded in hello HiveQL script file.</span></span> <span data-ttu-id="f9df1-184">Ennek megfelelően kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="f9df1-184">You must update it accordingly.</span></span>

<span data-ttu-id="f9df1-185">**toodownload hello felé továbbított adatok**</span><span class="sxs-lookup"><span data-stu-id="f9df1-185">**toodownload hello flight data**</span></span>

1. <span data-ttu-id="f9df1-186">Keresse meg a túl[kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda][rita-website].</span><span class="sxs-lookup"><span data-stu-id="f9df1-186">Browse too[Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].</span></span>
2. <span data-ttu-id="f9df1-187">Hello lapon jelölje be a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-187">On hello page, select hello following values:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="f9df1-188">Név</span><span class="sxs-lookup"><span data-stu-id="f9df1-188">Name</span></span></th><th><span data-ttu-id="f9df1-189">Érték</span><span class="sxs-lookup"><span data-stu-id="f9df1-189">Value</span></span></th></tr>
    <tr><td><span data-ttu-id="f9df1-190">Szűrő év</span><span class="sxs-lookup"><span data-stu-id="f9df1-190">Filter Year</span></span></td><td><span data-ttu-id="f9df1-191">2013</span><span class="sxs-lookup"><span data-stu-id="f9df1-191">2013</span></span> </td></tr>
    <tr><td><span data-ttu-id="f9df1-192">Időszak szűrése</span><span class="sxs-lookup"><span data-stu-id="f9df1-192">Filter Period</span></span></td><td><span data-ttu-id="f9df1-193">Január</span><span class="sxs-lookup"><span data-stu-id="f9df1-193">January</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-194">Mezők</span><span class="sxs-lookup"><span data-stu-id="f9df1-194">Fields</span></span></td><td><span data-ttu-id="f9df1-195">*Év*, *FlightDate*, *UniqueCarrier*, *szolgáltatónként*, *FlightNum*, *OriginAirportID* , *Származási*, *OriginCityName*, *OriginState*, *DestAirportID*, *cél* , *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (a többi mező törlése)</span><span class="sxs-lookup"><span data-stu-id="f9df1-195">*Year*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (clear all other fields)</span></span></td></tr>
    </table><span data-ttu-id="f9df1-196">
3.Kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="f9df1-196">
3. Click **Download**.</span></span>
<span data-ttu-id="f9df1-197">4.</span><span class="sxs-lookup"><span data-stu-id="f9df1-197">4.</span></span> <span data-ttu-id="f9df1-198">Bontsa ki a hello fájl toohello **C:\Tutorials\FlightDelay\2013Data** mappa.</span><span class="sxs-lookup"><span data-stu-id="f9df1-198">Unzip hello file toohello **C:\Tutorials\FlightDelay\2013Data** folder.</span></span> <span data-ttu-id="f9df1-199">Minden fájlt egy CSV-fájl, és körülbelül 60 GB-nál.</span><span class="sxs-lookup"><span data-stu-id="f9df1-199">Each file is a CSV file and is approximately 60GB in size.</span></span>
<span data-ttu-id="f9df1-200">5.</span><span class="sxs-lookup"><span data-stu-id="f9df1-200">5.</span></span> <span data-ttu-id="f9df1-201">Nevezze át a toohello fájlnév hello hello hónap, amely adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f9df1-201">Rename hello file toohello name of hello month that it contains data for.</span></span> <span data-ttu-id="f9df1-202">Hello január adatokat tartalmazó hello fájlt volna neve például *January.csv*.</span><span class="sxs-lookup"><span data-stu-id="f9df1-202">For example, hello file containing hello January data would be named *January.csv*.</span></span>
<span data-ttu-id="f9df1-203">6.</span><span class="sxs-lookup"><span data-stu-id="f9df1-203">6.</span></span> <span data-ttu-id="f9df1-204">Ismételje meg a 2. és 5 toodownload fájl az egyes hello 2013 12 hónapig.</span><span class="sxs-lookup"><span data-stu-id="f9df1-204">Repeat steps 2 and 5 toodownload a file for each of hello 12 months in 2013.</span></span> <span data-ttu-id="f9df1-205">Szüksége lesz legalább egy fájl toorun hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f9df1-205">You will need a minimum of one file toorun hello tutorial.</span></span>

<span data-ttu-id="f9df1-206">**tooupload hello repülési késleltetés adatok tooAzure Blob-tároló**</span><span class="sxs-lookup"><span data-stu-id="f9df1-206">**tooupload hello flight delay data tooAzure Blob storage**</span></span>

1. <span data-ttu-id="f9df1-207">Készítse elő a hello paraméterek:</span><span class="sxs-lookup"><span data-stu-id="f9df1-207">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="f9df1-208">Változó neve</span><span class="sxs-lookup"><span data-stu-id="f9df1-208">Variable Name</span></span></th><th><span data-ttu-id="f9df1-209">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f9df1-209">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="f9df1-210">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="f9df1-210">$storageAccountName</span></span></td><td><span data-ttu-id="f9df1-211">hello Azure Storage-fiókra, amelyhez tooupload hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-211">hello Azure Storage account where you want tooupload hello data to.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-212">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="f9df1-212">$blobContainerName</span></span></td><td><span data-ttu-id="f9df1-213">hello Blob tároló, ahol szeretné tooupload hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-213">hello Blob container where you want tooupload hello data to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="f9df1-214">Nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="f9df1-214">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="f9df1-215">3.</span><span class="sxs-lookup"><span data-stu-id="f9df1-215">3.</span></span> <span data-ttu-id="f9df1-216">Illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-216">Paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # hello source folder
    $destFolder = "tutorials/flightdelay/2013data"     #hello blob name prefix for hello files toobe uploaded
    #EndRegion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy hello file from local workstation tooAzure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName too$blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "hello source folder on hello workstation doesn't exist" -ForegroundColor Red
    }

    # List hello uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. <span data-ttu-id="f9df1-217">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f9df1-217">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="f9df1-218">Ha úgy dönt, toouse hello fájlok feltöltése más módszert, ellenőrizze, hogy hello fájl elérési útja oktatóanyagok/flightdelay/adatokat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-218">If you choose toouse a different method for uploading hello files, please make sure hello file path is tutorials/flightdelay/data.</span></span> <span data-ttu-id="f9df1-219">hello hello fájlokhoz való hozzáférést szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="f9df1-219">hello syntax for accessing hello files is:</span></span>

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

<span data-ttu-id="f9df1-220">hello elérési oktatóanyagok/flightdelay/adata hello hello fájlok feltöltött létrehozott virtuális mappát.</span><span class="sxs-lookup"><span data-stu-id="f9df1-220">hello path tutorials/flightdelay/data is hello virtual folder you created when you uploaded hello files.</span></span> <span data-ttu-id="f9df1-221">Győződjön meg arról, hogy vannak-e egy minden hónapban a 12 fájlt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-221">Verify that there are 12 files, one for each month.</span></span>

> [!NOTE]
> <span data-ttu-id="f9df1-222">Hello Hive lekérdezés tooread hello új helyről kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="f9df1-222">You must update hello Hive query tooread from hello new location.</span></span>
>
> <span data-ttu-id="f9df1-223">Hello tároló hozzáférési engedélyt toobe nyilvános konfigurálásához, vagy kötési hello tárolási fiók toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f9df1-223">You must either configure hello container access permission toobe public or bind hello Storage account toohello HDInsight cluster.</span></span> <span data-ttu-id="f9df1-224">Ellenkező esetben hello Hive lekérdezés-karakterlánc nem lesz képes tooaccess hello adatfájlokat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-224">Otherwise, hello Hive query string will not be able tooaccess hello data files.</span></span>

- - -

## <span data-ttu-id="f9df1-225"><a id="appendix-b"></a>B függelék – hozzon létre, és töltse fel a HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="f9df1-225"><a id="appendix-b"></a>Appendix B - Create and upload a HiveQL script</span></span>
<span data-ttu-id="f9df1-226">Az Azure PowerShell használatával is futtathatja több HiveQL utasítást egy időben, vagy csomag hello HiveQL utasítás egy parancsfájl-fájlba.</span><span class="sxs-lookup"><span data-stu-id="f9df1-226">Using Azure PowerShell, you can run multiple HiveQL statements one at a time, or package hello HiveQL statement into a script file.</span></span> <span data-ttu-id="f9df1-227">Ez a szakasz bemutatja, hogyan toocreate HiveQL-parancsfájlt és a feltöltés hello parancsfájl tooAzure a Blob storage Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f9df1-227">This section shows you how toocreate a HiveQL script and upload hello script tooAzure Blob storage by using Azure PowerShell.</span></span> <span data-ttu-id="f9df1-228">Hive hello HiveQL parancsfájlok toobe Azure Blob storage-ban tárolt igényel.</span><span class="sxs-lookup"><span data-stu-id="f9df1-228">Hive requires hello HiveQL scripts toobe stored in Azure Blob storage.</span></span>

<span data-ttu-id="f9df1-229">hello HiveQL-parancsfájlt hello következőket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="f9df1-229">hello HiveQL script will perform hello following:</span></span>

1. <span data-ttu-id="f9df1-230">**Hello delays_raw table DROP**, abban az esetben hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="f9df1-230">**Drop hello delays_raw table**, in case hello table already exists.</span></span>
2. <span data-ttu-id="f9df1-231">**Hello delays_raw külső Hive tábla létrehozásához** toohello Blob tároló helye mutató hello repülési késleltetés fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="f9df1-231">**Create hello delays_raw external Hive table** pointing toohello Blob storage location with hello flight delay files.</span></span> <span data-ttu-id="f9df1-232">Ez a lekérdezés határozza meg, hogy mezők határolja ",", és hogy sorok "\n" vannak-e állítva.</span><span class="sxs-lookup"><span data-stu-id="f9df1-232">This query specifies that fields are delimited by "," and that lines are terminated by "\n".</span></span> <span data-ttu-id="f9df1-233">Ez problémát jelent, amikor a mezőértékek tartalmazhat vesszőket, mert struktúra nem különbözteti meg, amely a mezőhatároló vesszővel és a mezőérték része egy (vagyis mezőjének értékét származási hello nagybetűkkel\_VÁROS\_nevét, és a cél\_ VÁROS\_neve).</span><span class="sxs-lookup"><span data-stu-id="f9df1-233">This poses a problem when field values contain commas because Hive cannot differentiate between a comma that is a field delimiter and a one that is part of a field value (which is hello case in field values for ORIGIN\_CITY\_NAME and DEST\_CITY\_NAME).</span></span> <span data-ttu-id="f9df1-234">tooaddress a, hello lekérdezés adatokat hoz létre TEMP oszlopok toohold helytelenül osztott oszlopok.</span><span class="sxs-lookup"><span data-stu-id="f9df1-234">tooaddress this, hello query creates TEMP columns toohold data that is incorrectly split into columns.</span></span>
3. <span data-ttu-id="f9df1-235">**Hello késések table DROP**, abban az esetben hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="f9df1-235">**Drop hello delays table**, in case hello table already exists.</span></span>
4. <span data-ttu-id="f9df1-236">**Hello késések tábla létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f9df1-236">**Create hello delays table**.</span></span> <span data-ttu-id="f9df1-237">További feldolgozás előtt is hasznos tooclean hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="f9df1-237">It is helpful tooclean up hello data before further processing.</span></span> <span data-ttu-id="f9df1-238">Ez a lekérdezés táblát hoz létre új *késések*, hello delays_raw táblából.</span><span class="sxs-lookup"><span data-stu-id="f9df1-238">This query creates a new table, *delays*, from hello delays_raw table.</span></span> <span data-ttu-id="f9df1-239">Vegye figyelembe, hogy a rendszer nem másolja hello átmeneti oszlopokban (ahogy korábban említettük), és adott hello **substring** függvény használt tooremove idézőjelek hello adatokból.</span><span class="sxs-lookup"><span data-stu-id="f9df1-239">Note that hello TEMP columns (as mentioned previously) are not copied, and that hello **substring** function is used tooremove quotation marks from hello data.</span></span>
5. <span data-ttu-id="f9df1-240">**Számítási hello átlagos időjárási késleltetés és a csoportok hello eredmények város név szerint.**</span><span class="sxs-lookup"><span data-stu-id="f9df1-240">**Compute hello average weather delay and groups hello results by city name.**</span></span> <span data-ttu-id="f9df1-241">Azt is kimeneteként hello eredmények tooBlob tároló.</span><span class="sxs-lookup"><span data-stu-id="f9df1-241">It will also output hello results tooBlob storage.</span></span> <span data-ttu-id="f9df1-242">Vegye figyelembe a hello lekérdezés eltávolítja az aposztrófot hello adatokból, és kihagyja a sort, ahol hello értékének **weather_delay** null értékű.</span><span class="sxs-lookup"><span data-stu-id="f9df1-242">Note that hello query will remove apostrophes from hello data and will exclude rows where hello value for **weather_delay** is null.</span></span> <span data-ttu-id="f9df1-243">Erre akkor szükség, mert a Sqoop, az oktatóanyag későbbi részében használt nem kezeli az ezeket az értékeket szabályosan alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f9df1-243">This is necessary because Sqoop, used later in this tutorial, doesn't handle those values gracefully by default.</span></span>

<span data-ttu-id="f9df1-244">Hello HiveQL parancsok teljes listáját lásd: [Hive adatdefiníciós nyelv][hadoop-hiveql].</span><span class="sxs-lookup"><span data-stu-id="f9df1-244">For a full list of hello HiveQL commands, see [Hive Data Definition Language][hadoop-hiveql].</span></span> <span data-ttu-id="f9df1-245">Minden HiveQL parancs pontosvesszővel kell állítanunk.</span><span class="sxs-lookup"><span data-stu-id="f9df1-245">Each HiveQL command must terminate with a semicolon.</span></span>

<span data-ttu-id="f9df1-246">**toocreate HiveQL-parancsfájlt**</span><span class="sxs-lookup"><span data-stu-id="f9df1-246">**toocreate a HiveQL script file**</span></span>

1. <span data-ttu-id="f9df1-247">Készítse elő a hello paraméterek:</span><span class="sxs-lookup"><span data-stu-id="f9df1-247">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="f9df1-248">Változó neve</span><span class="sxs-lookup"><span data-stu-id="f9df1-248">Variable Name</span></span></th><th><span data-ttu-id="f9df1-249">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f9df1-249">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="f9df1-250">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="f9df1-250">$storageAccountName</span></span></td><td><span data-ttu-id="f9df1-251">hello tooupload hello HiveQL-parancsfájlt a kívánt Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="f9df1-251">hello Azure Storage account where you want tooupload hello HiveQL script to.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-252">$blobContainerName</span><span class="sxs-lookup"><span data-stu-id="f9df1-252">$blobContainerName</span></span></td><td><span data-ttu-id="f9df1-253">Ha azt szeretné, hogy tooupload hello HiveQL-parancsfájlt a Blob-tároló hello.</span><span class="sxs-lookup"><span data-stu-id="f9df1-253">hello Blob container where you want tooupload hello HiveQL script to.</span></span></td></tr>
    </table>
2. <span data-ttu-id="f9df1-254">Nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="f9df1-254">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="f9df1-255">3.</span><span class="sxs-lookup"><span data-stu-id="f9df1-255">3.</span></span> <span data-ttu-id="f9df1-256">Másolja és illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-256">Copy and paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # hello HiveQL script file is exported as this file before it's uploaded tooBlob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # hello HiveQL script file will be uploaded tooBlob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by hello HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate hello file and file path

    # Check if a file with hello same file name already exists on hello workstation
    Write-Host "`nvalidating hello folder structure on hello workstation for saving hello HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'hello file, ' $hqlLocalFileName ', exists.  Do you want toooverwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create hello folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write hello Hive script into a local file
    Write-Host "`nWriting hello Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload hello Hive script toohello default Blob container
    Write-Host "`nUploading hello Hive script toohello default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload hello file from local workstation tooBlob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

    <span data-ttu-id="f9df1-257">Az alábbiakban hello parancsfájlban használt hello változók:</span><span class="sxs-lookup"><span data-stu-id="f9df1-257">Here are hello variables used in hello script:</span></span>

   * <span data-ttu-id="f9df1-258">**$hqlLocalFileName** -hello parancsfájl menti hello HiveQL-parancsfájlt helyileg feltöltés előtt tooBlob tároló.</span><span class="sxs-lookup"><span data-stu-id="f9df1-258">**$hqlLocalFileName** - hello script saves hello HiveQL script file locally before uploading it tooBlob storage.</span></span> <span data-ttu-id="f9df1-259">Ez az hello fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="f9df1-259">This is hello file name.</span></span> <span data-ttu-id="f9df1-260">hello alapértelmezett értéke <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span><span class="sxs-lookup"><span data-stu-id="f9df1-260">hello default value is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.</span></span>
   * <span data-ttu-id="f9df1-261">**$hqlBlobName** -hello HiveQL parancsfájl fájl blob neve szerepel a hello Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f9df1-261">**$hqlBlobName** - This is hello HiveQL script file blob name used in hello Azure Blob storage.</span></span> <span data-ttu-id="f9df1-262">hello alapértelmezett értéke tutorials/flightdelay/flightdelays.hql.</span><span class="sxs-lookup"><span data-stu-id="f9df1-262">hello default value is tutorials/flightdelay/flightdelays.hql.</span></span> <span data-ttu-id="f9df1-263">Hello fájlba lesz írva közvetlenül tooAzure Blob-tároló, mert nincs olyan "/" hello blob neve hello elején.</span><span class="sxs-lookup"><span data-stu-id="f9df1-263">Because hello file will be written directly tooAzure Blob storage, there is NOT a "/" at hello beginning of hello blob name.</span></span> <span data-ttu-id="f9df1-264">Ha azt szeretné, hogy a Blob storage tooaccess hello fájlt, akkor tooadd, a "/" hello fájlnév hello elején.</span><span class="sxs-lookup"><span data-stu-id="f9df1-264">If you want tooaccess hello file from Blob storage, you will need tooadd a "/" at hello beginning of hello file name.</span></span>
   * <span data-ttu-id="f9df1-265">**$srcDataFolder** és **$dstDataFolder** -= "oktatóanyagok/flightdelay/adatok" = "oktatóanyagok/flightdelay/kimeneti"</span><span class="sxs-lookup"><span data-stu-id="f9df1-265">**$srcDataFolder** and **$dstDataFolder** - = "tutorials/flightdelay/data" = "tutorials/flightdelay/output"</span></span>

- - -
## <span data-ttu-id="f9df1-266"><a id="appendix-c"></a>C függelék – hello Sqoop feladatkiemenetét Azure SQL-adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="f9df1-266"><a id="appendix-c"></a>Appendix C - Prepare an Azure SQL database for hello Sqoop job output</span></span>
<span data-ttu-id="f9df1-267">**tooprepare hello SQL-adatbázis (egyesítés ezzel a Sqoop parancsfájl hello)**</span><span class="sxs-lookup"><span data-stu-id="f9df1-267">**tooprepare hello SQL database (merge this with hello Sqoop script)**</span></span>

1. <span data-ttu-id="f9df1-268">Készítse elő a hello paraméterek:</span><span class="sxs-lookup"><span data-stu-id="f9df1-268">Prepare hello parameters:</span></span>

    <table border="1">
    <tr><th><span data-ttu-id="f9df1-269">Változó neve</span><span class="sxs-lookup"><span data-stu-id="f9df1-269">Variable Name</span></span></th><th><span data-ttu-id="f9df1-270">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f9df1-270">Notes</span></span></th></tr>
    <tr><td><span data-ttu-id="f9df1-271">$sqlDatabaseServerName</span><span class="sxs-lookup"><span data-stu-id="f9df1-271">$sqlDatabaseServerName</span></span></td><td><span data-ttu-id="f9df1-272">hello hello Azure SQL adatbázis-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="f9df1-272">hello name of hello Azure SQL database server.</span></span> <span data-ttu-id="f9df1-273">Adja meg, mit toocreate egy új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="f9df1-273">Enter nothing toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-274">$sqlDatabaseUsername</span><span class="sxs-lookup"><span data-stu-id="f9df1-274">$sqlDatabaseUsername</span></span></td><td><span data-ttu-id="f9df1-275">hello bejelentkezési név hello Azure SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f9df1-275">hello login name for hello Azure SQL database server.</span></span> <span data-ttu-id="f9df1-276">Ha $sqlDatabaseServerName egy meglévő kiszolgálóról, hello bejelentkezési és a bejelentkezés jelszó is használt tooauthenticate hello kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="f9df1-276">If $sqlDatabaseServerName is an existing server, hello login and login password are used tooauthenticate with hello server.</span></span> <span data-ttu-id="f9df1-277">Ellenkező esetben azok használt toocreate egy új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="f9df1-277">Otherwise they are used toocreate a new server.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-278">$sqlDatabasePassword</span><span class="sxs-lookup"><span data-stu-id="f9df1-278">$sqlDatabasePassword</span></span></td><td><span data-ttu-id="f9df1-279">a bejelentkezés jelszó hello hello Azure SQL adatbázis-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="f9df1-279">hello login password for hello Azure SQL database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-280">$sqlDatabaseLocation</span><span class="sxs-lookup"><span data-stu-id="f9df1-280">$sqlDatabaseLocation</span></span></td><td><span data-ttu-id="f9df1-281">Ezt az értéket csak akkor, ha hoz létre egy új Azure-adatbázis-kiszolgálót használja.</span><span class="sxs-lookup"><span data-stu-id="f9df1-281">This value is used only when you're creating a new Azure database server.</span></span></td></tr>
    <tr><td><span data-ttu-id="f9df1-282">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="f9df1-282">$sqlDatabaseName</span></span></td><td><span data-ttu-id="f9df1-283">SQL-adatbázis hello toocreate hello AvgDelays tábla hello Sqoop feladathoz használt.</span><span class="sxs-lookup"><span data-stu-id="f9df1-283">hello SQL database used toocreate hello AvgDelays table for hello Sqoop job.</span></span> <span data-ttu-id="f9df1-284">Hagyja üresen HDISqoop nevű adatbázist fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f9df1-284">Leaving it blank will create a database called HDISqoop.</span></span> <span data-ttu-id="f9df1-285">hello táblaneve hello Sqoop feladatkiemenetét AvgDelays.</span><span class="sxs-lookup"><span data-stu-id="f9df1-285">hello table name for hello Sqoop job output is AvgDelays.</span></span> </td></tr>
    </table>
2. <span data-ttu-id="f9df1-286">Nyissa meg az Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="f9df1-286">Open Azure PowerShell ISE.</span></span>
<span data-ttu-id="f9df1-287">3.</span><span class="sxs-lookup"><span data-stu-id="f9df1-287">3.</span></span> <span data-ttu-id="f9df1-288">Másolja és illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-288">Copy and paste hello following script into hello script pane:</span></span>

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello region toocreate hello Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify hello database name if you have one created. Otherwise use "" toohave hello script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command toocreate hello AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > <span data-ttu-id="f9df1-289">hello parancsfájl használ egy representational állapot transfer (REST) szolgáltatás, http://bot.whatismyipaddress.com, tooretrieve a külső IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f9df1-289">hello script uses a representational state transfer (REST) service, http://bot.whatismyipaddress.com, tooretrieve your external IP address.</span></span> <span data-ttu-id="f9df1-290">hello IP-címet az SQL database-kiszolgálóhoz egy tűzfalszabályt létrehozására szolgál.</span><span class="sxs-lookup"><span data-stu-id="f9df1-290">hello IP address is used for creating a firewall rule for your SQL database server.</span></span>

    <span data-ttu-id="f9df1-291">Az alábbiakban néhány hello parancsfájlban használt változók:</span><span class="sxs-lookup"><span data-stu-id="f9df1-291">Here are some variables used in hello script:</span></span>

   * <span data-ttu-id="f9df1-292">**$ipAddressRestService** -hello alapértelmezett értéke http://bot.whatismyipaddress.com. Egy nyilvános IP-címet a külső IP-cím beolvasása a REST-szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="f9df1-292">**$ipAddressRestService** - hello default value is http://bot.whatismyipaddress.com. It is a public IP address REST service for getting your external IP address.</span></span> <span data-ttu-id="f9df1-293">Más szolgáltatások is használhatja, ha azt szeretné.</span><span class="sxs-lookup"><span data-stu-id="f9df1-293">You can use other services if you want.</span></span> <span data-ttu-id="f9df1-294">hello hello szolgáltatás segítségével külső IP-cím lesz használt toocreate az Azure SQL adatbázis-kiszolgáló, egy tűzfalszabályt, így (a Windows PowerShell-parancsfájl használatával) a munkaállomáson hello adatbázis végezheti el.</span><span class="sxs-lookup"><span data-stu-id="f9df1-294">hello external IP address retrieved through hello service will be used toocreate a firewall rule for your Azure SQL database server, so that you can access hello database from your workstation (by using a Windows PowerShell script).</span></span>
   * <span data-ttu-id="f9df1-295">**$fireWallRuleName** -hello Azure SQL adatbázis-kiszolgáló hello hello tűzfalszabály neve.</span><span class="sxs-lookup"><span data-stu-id="f9df1-295">**$fireWallRuleName** - This is hello name of hello firewall rule for hello Azure SQL database server.</span></span> <span data-ttu-id="f9df1-296">hello alapértelmezés szerint ez <u>FlightDelay</u>.</span><span class="sxs-lookup"><span data-stu-id="f9df1-296">hello default name is <u>FlightDelay</u>.</span></span> <span data-ttu-id="f9df1-297">Ha azt szeretné, átnevezheti.</span><span class="sxs-lookup"><span data-stu-id="f9df1-297">You can rename it if you want.</span></span>
   * <span data-ttu-id="f9df1-298">**$sqlDatabaseMaxSizeGB** -Ez az érték csak egy új Azure SQL adatbázis-kiszolgáló létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="f9df1-298">**$sqlDatabaseMaxSizeGB** - This value is used only when you're creating a new Azure SQL database server.</span></span> <span data-ttu-id="f9df1-299">hello alapértelmezett értéke 10 GB-os.</span><span class="sxs-lookup"><span data-stu-id="f9df1-299">hello default value is 10GB.</span></span> <span data-ttu-id="f9df1-300">10 GB-os is elegendő ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="f9df1-300">10GB is sufficient for this tutorial.</span></span>
   * <span data-ttu-id="f9df1-301">**$sqlDatabaseName** -Ez az érték csak egy új Azure SQL-adatbázis létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="f9df1-301">**$sqlDatabaseName** - This value is used only when you're creating a new Azure SQL database.</span></span> <span data-ttu-id="f9df1-302">hello alapértelmezett értéke HDISqoop.</span><span class="sxs-lookup"><span data-stu-id="f9df1-302">hello default value is HDISqoop.</span></span> <span data-ttu-id="f9df1-303">Ha az Átnevezés hello Sqoop Windows PowerShell-parancsfájl ennek megfelelően kell frissíteni.</span><span class="sxs-lookup"><span data-stu-id="f9df1-303">If you rename it, you must update hello Sqoop Windows PowerShell script accordingly.</span></span>
4. <span data-ttu-id="f9df1-304">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f9df1-304">Press **F5** toorun hello script.</span></span>
5. <span data-ttu-id="f9df1-305">Hello parancsfájl kimenetében ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="f9df1-305">Validate hello script output.</span></span> <span data-ttu-id="f9df1-306">Ellenőrizze, hogy hello parancsprogram sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="f9df1-306">Make sure hello script ran successfully.</span></span>

## <span data-ttu-id="f9df1-307"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9df1-307"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="f9df1-308">Most már megismerte hogyan tooupload egy fájl tooAzure Blob-tároló, hogyan toopopulate egy Hive tábla hello adatok az Azure Blob storage használatával hogyan toorun Hive lekérdezések, és hogyan toouse HDFS tooan Azure SQL adatbázis Sqoop tooexport adatait.</span><span class="sxs-lookup"><span data-stu-id="f9df1-308">Now you understand how tooupload a file tooAzure Blob storage, how toopopulate a Hive table by using hello data from Azure Blob storage, how toorun Hive queries, and how toouse Sqoop tooexport data from HDFS tooan Azure SQL database.</span></span> <span data-ttu-id="f9df1-309">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="f9df1-309">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="f9df1-310">[Első lépései a hdinsight eszközzel][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="f9df1-310">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="f9df1-311">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f9df1-311">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="f9df1-312">[Oozie használata a hdinsight eszközzel][hdinsight-use-oozie]</span><span class="sxs-lookup"><span data-stu-id="f9df1-312">[Use Oozie with HDInsight][hdinsight-use-oozie]</span></span>
* <span data-ttu-id="f9df1-313">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="f9df1-313">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="f9df1-314">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="f9df1-314">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="f9df1-315">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="f9df1-315">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
