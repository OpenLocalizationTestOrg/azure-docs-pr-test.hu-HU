---
title: a PowerShell - Azure hdinsight clusters aaaManage Hadoop |} Microsoft Docs
description: "Ismerje meg, hogyan tooperform felügyeleti feladatokat hello Azure PowerShell használatával hdinsight Hadoop-fürtök."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a><span data-ttu-id="1a41c-103">Hdinsight Hadoop-fürtök kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1a41c-103">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="1a41c-104">Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése az Azure-ban a feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="1a41c-104">Azure PowerShell is a powerful scripting environment that you can use toocontrol and automate hello deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="1a41c-105">Ebből a cikkből megtudhatja, hogyan toomanage Hadoop-fürtök az Azure HDInsight egy helyi Azure PowerShell-konzolon keresztül hello Windows PowerShell használja.</span><span class="sxs-lookup"><span data-stu-id="1a41c-105">In this article, you will learn how toomanage Hadoop clusters in Azure HDInsight by using a local Azure PowerShell console through hello use of Windows PowerShell.</span></span> <span data-ttu-id="1a41c-106">HDInsight PowerShell-parancsmagok hello hello listájáért lásd: [HDInsight parancsmag-referencia][hdinsight-powershell-reference].</span><span class="sxs-lookup"><span data-stu-id="1a41c-106">For hello list of hello HDInsight PowerShell cmdlets, see [HDInsight cmdlet reference][hdinsight-powershell-reference].</span></span>

<span data-ttu-id="1a41c-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="1a41c-107">**Prerequisites**</span></span>

<span data-ttu-id="1a41c-108">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="1a41c-108">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="1a41c-109">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="1a41c-109">**An Azure subscription**.</span></span> <span data-ttu-id="1a41c-110">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1a41c-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="1a41c-111">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="1a41c-111">Install Azure PowerShell</span></span>
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="1a41c-112">Ha telepítette az Azure PowerShell 0,9 verziója x, el kell távolítani egy újabb verzió telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="1a41c-112">If you have installed Azure PowerShell version 0.9x, you must uninstall it before installing a newer version.</span></span>

<span data-ttu-id="1a41c-113">hello toocheck hello verziója PowerShell telepítve:</span><span class="sxs-lookup"><span data-stu-id="1a41c-113">toocheck hello version of hello installed PowerShell:</span></span>

    Get-Module *azure*

<span data-ttu-id="1a41c-114">toouninstall hello régebbi verziójú, a hello Vezérlőpult Programok és szolgáltatások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1a41c-114">toouninstall hello older version, run Programs and Features in hello control panel.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="1a41c-115">Fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1a41c-115">Create clusters</span></span>
<span data-ttu-id="1a41c-116">Lásd: [fürtök létrehozása Linux-alapú hdinsight Azure PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="1a41c-116">See [Create Linux-based clusters in HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="1a41c-117">Lista fürtök</span><span class="sxs-lookup"><span data-stu-id="1a41c-117">List clusters</span></span>
<span data-ttu-id="1a41c-118">Használja a következő parancs toolist hello összes fürt ebben az előfizetésben hello:</span><span class="sxs-lookup"><span data-stu-id="1a41c-118">Use hello following command toolist all clusters in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a><span data-ttu-id="1a41c-119">Fürt megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1a41c-119">Show cluster</span></span>
<span data-ttu-id="1a41c-120">Használja a következő parancs tooshow adatok egy adott fürt ebben az előfizetésben hello hello:</span><span class="sxs-lookup"><span data-stu-id="1a41c-120">Use hello following command tooshow details of a specific cluster in hello current subscription:</span></span>

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a><span data-ttu-id="1a41c-121">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="1a41c-121">Delete clusters</span></span>
<span data-ttu-id="1a41c-122">A következő parancs toodelete fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="1a41c-122">Use hello following command toodelete a cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

<span data-ttu-id="1a41c-123">A fürt hello fürt tartalmazó erőforráscsoportot hello eltávolításával is törli.</span><span class="sxs-lookup"><span data-stu-id="1a41c-123">You can also delete a cluster by removing hello resource group that contains hello cluster.</span></span> <span data-ttu-id="1a41c-124">Vegye figyelembe, többek között az alapértelmezett tárfiók hello hello csoport minden hello erőforrásának törölni szeretné.</span><span class="sxs-lookup"><span data-stu-id="1a41c-124">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="1a41c-125">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="1a41c-125">Scale clusters</span></span>
<span data-ttu-id="1a41c-126">hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1a41c-126">hello cluster scaling feature allows you toochange hello number of worker nodes used by a cluster that is running in Azure HDInsight without having toore-create hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1a41c-127">Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak.</span><span class="sxs-lookup"><span data-stu-id="1a41c-127">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="1a41c-128">Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.</span><span class="sxs-lookup"><span data-stu-id="1a41c-128">If you are unsure of hello version of your cluster, you can check hello Properties page.</span></span>  <span data-ttu-id="1a41c-129">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="1a41c-129">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="1a41c-130">a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:</span><span class="sxs-lookup"><span data-stu-id="1a41c-130">hello impact of changing hello number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="1a41c-131">Hadoop</span><span class="sxs-lookup"><span data-stu-id="1a41c-131">Hadoop</span></span>

    <span data-ttu-id="1a41c-132">Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="1a41c-132">You can seamlessly increase hello number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="1a41c-133">Új feladatokat is küldheti el, amíg hello művelet van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="1a41c-133">New jobs can also be submitted while hello operation is in progress.</span></span> <span data-ttu-id="1a41c-134">A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.</span><span class="sxs-lookup"><span data-stu-id="1a41c-134">Failures in a scaling operation are gracefully handled so that hello cluster is always left in a functional state.</span></span>

    <span data-ttu-id="1a41c-135">A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul.</span><span class="sxs-lookup"><span data-stu-id="1a41c-135">When a Hadoop cluster is scaled down by reducing hello number of data nodes, some of hello services in hello cluster are restarted.</span></span> <span data-ttu-id="1a41c-136">Ennek hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő.</span><span class="sxs-lookup"><span data-stu-id="1a41c-136">This causes all running and pending jobs toofail at hello completion of hello scaling operation.</span></span> <span data-ttu-id="1a41c-137">Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="1a41c-137">You can, however, resubmit hello jobs once hello operation is complete.</span></span>
* <span data-ttu-id="1a41c-138">HBase</span><span class="sxs-lookup"><span data-stu-id="1a41c-138">HBase</span></span>

    <span data-ttu-id="1a41c-139">Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása.</span><span class="sxs-lookup"><span data-stu-id="1a41c-139">You can seamlessly add or remove nodes tooyour HBase cluster while it is running.</span></span> <span data-ttu-id="1a41c-140">A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását.</span><span class="sxs-lookup"><span data-stu-id="1a41c-140">Regional Servers are automatically balanced within a few minutes of completing hello scaling operation.</span></span> <span data-ttu-id="1a41c-141">Azonban Ön kézzel is eloszthatja hello területi kiszolgálók történő naplózásának révén a fürt és a következő parancsok parancssori ablakból futó hello toohello headnode:</span><span class="sxs-lookup"><span data-stu-id="1a41c-141">However, you can also manually balance hello regional servers by logging in toohello headnode of cluster and running hello following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="1a41c-142">Storm</span><span class="sxs-lookup"><span data-stu-id="1a41c-142">Storm</span></span>

    <span data-ttu-id="1a41c-143">Zökkenőmentesen hozzáadása vagy eltávolítása adatok csomópontok tooyour Storm-fürt futása közben is.</span><span class="sxs-lookup"><span data-stu-id="1a41c-143">You can seamlessly add or remove data nodes tooyour Storm cluster while it is running.</span></span> <span data-ttu-id="1a41c-144">De hello skálázás művelet sikeres befejezése után kell toorebalance hello topológia.</span><span class="sxs-lookup"><span data-stu-id="1a41c-144">But after a successful completion of hello scaling operation, you will need toorebalance hello topology.</span></span>

    <span data-ttu-id="1a41c-145">Kétféle módon valósítható meg újraelosztás:</span><span class="sxs-lookup"><span data-stu-id="1a41c-145">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="1a41c-146">A Storm webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="1a41c-146">Storm web UI</span></span>
  * <span data-ttu-id="1a41c-147">Parancssori felület (CLI) eszköz</span><span class="sxs-lookup"><span data-stu-id="1a41c-147">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="1a41c-148">Tekintse meg a toohello [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="1a41c-148">Please refer toohello [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="1a41c-149">HDInsight-fürt hello hello Storm webes felhasználói felület érhető el:</span><span class="sxs-lookup"><span data-stu-id="1a41c-149">hello Storm web UI is available on hello HDInsight cluster:</span></span>

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    <span data-ttu-id="1a41c-151">Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:</span><span class="sxs-lookup"><span data-stu-id="1a41c-151">Here is an example how toouse hello CLI command toorebalance hello Storm topology:</span></span>

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="1a41c-152">toochange hello Azure PowerShell, futtassa a parancsot követő ügyfélgépről hello használatával Hadoop-fürt mérete:</span><span class="sxs-lookup"><span data-stu-id="1a41c-152">toochange hello Hadoop cluster size by using Azure PowerShell, run hello following command from a client machine:</span></span>

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a><span data-ttu-id="1a41c-153">Hozzáférés biztosítása/visszavonása</span><span class="sxs-lookup"><span data-stu-id="1a41c-153">Grant/revoke access</span></span>
<span data-ttu-id="1a41c-154">A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="1a41c-154">HDInsight clusters have hello following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="1a41c-155">ODBC</span><span class="sxs-lookup"><span data-stu-id="1a41c-155">ODBC</span></span>
* <span data-ttu-id="1a41c-156">JDBC</span><span class="sxs-lookup"><span data-stu-id="1a41c-156">JDBC</span></span>
* <span data-ttu-id="1a41c-157">Ambari</span><span class="sxs-lookup"><span data-stu-id="1a41c-157">Ambari</span></span>
* <span data-ttu-id="1a41c-158">Oozie</span><span class="sxs-lookup"><span data-stu-id="1a41c-158">Oozie</span></span>
* <span data-ttu-id="1a41c-159">Lépni a Templeton</span><span class="sxs-lookup"><span data-stu-id="1a41c-159">Templeton</span></span>

<span data-ttu-id="1a41c-160">Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított.</span><span class="sxs-lookup"><span data-stu-id="1a41c-160">By default, these services are granted for access.</span></span> <span data-ttu-id="1a41c-161">Akkor is a visszavonási/grant hello hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1a41c-161">You can revoke/grant hello access.</span></span> <span data-ttu-id="1a41c-162">toorevoke:</span><span class="sxs-lookup"><span data-stu-id="1a41c-162">toorevoke:</span></span>

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

<span data-ttu-id="1a41c-163">toogrant:</span><span class="sxs-lookup"><span data-stu-id="1a41c-163">toogrant:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> <span data-ttu-id="1a41c-164">Hello hozzáférés biztosítása/visszavonása, visszaállítja, hello fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="1a41c-164">By granting/revoking hello access, you will reset hello cluster user name and password.</span></span>
>
>

<span data-ttu-id="1a41c-165">Ez hello portálon keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="1a41c-165">This can also be done via hello Portal.</span></span> <span data-ttu-id="1a41c-166">Lásd: [felügyeletéhez HDInsight használatával hello Azure-portálon][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="1a41c-166">See [Administer HDInsight by using hello Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="1a41c-167">HTTP-felhasználó hitelesítő adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="1a41c-167">Update HTTP user credentials</span></span>
<span data-ttu-id="1a41c-168">Hello azonos művelet [Grant/revoke HTTP access](#grant/revoke-access). Ha hello fürt hello HTTP hozzáféréssel rendelkezik, akkor kell először vonja vissza.</span><span class="sxs-lookup"><span data-stu-id="1a41c-168">It is hello same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If hello cluster has been granted hello HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="1a41c-169">És új HTTP felhasználói hitelesítő adatokkal majd hello hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1a41c-169">And then grant hello access with new HTTP user credentials.</span></span>

## <a name="find-hello-default-storage-account"></a><span data-ttu-id="1a41c-170">Hello alapértelmezett tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="1a41c-170">Find hello default storage account</span></span>
<span data-ttu-id="1a41c-171">a következő Powershell-parancsfájl hello bemutatja, hogyan tooget hello alapértelmezett tárfiók neve és hello alapértelmezett tárfiók kulcsa fürt.</span><span class="sxs-lookup"><span data-stu-id="1a41c-171">hello following Powershell script demonstrates how tooget hello default storage account name and hello default storage account key for a cluster.</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a><span data-ttu-id="1a41c-172">Hello erőforráscsoportban található</span><span class="sxs-lookup"><span data-stu-id="1a41c-172">Find hello resource group</span></span>
<span data-ttu-id="1a41c-173">Hello erőforrás-kezelő módban mindegyik HDInsight-fürt tooan Azure erőforráscsoport tartozik.</span><span class="sxs-lookup"><span data-stu-id="1a41c-173">In hello Resource Manager mode, each HDInsight cluster belongs tooan Azure resource group.</span></span>  <span data-ttu-id="1a41c-174">toofind hello erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="1a41c-174">toofind hello resource group:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a><span data-ttu-id="1a41c-175">Feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="1a41c-175">Submit jobs</span></span>
<span data-ttu-id="1a41c-176">**toosubmit MapReduce-feladatok**</span><span class="sxs-lookup"><span data-stu-id="1a41c-176">**toosubmit MapReduce jobs**</span></span>

<span data-ttu-id="1a41c-177">Lásd: [Windows-alapú hdinsight Hadoop-MapReduce futtatása minták](hdinsight-run-samples.md).</span><span class="sxs-lookup"><span data-stu-id="1a41c-177">See [Run Hadoop MapReduce samples in Windows-based HDInsight](hdinsight-run-samples.md).</span></span>

<span data-ttu-id="1a41c-178">**toosubmit Hive-feladatok**</span><span class="sxs-lookup"><span data-stu-id="1a41c-178">**toosubmit Hive jobs**</span></span>

<span data-ttu-id="1a41c-179">Lásd: [PowerShell használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1a41c-179">See [Run Hive queries using PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span></span>

<span data-ttu-id="1a41c-180">**toosubmit Pig-feladatokhoz**</span><span class="sxs-lookup"><span data-stu-id="1a41c-180">**toosubmit Pig jobs**</span></span>

<span data-ttu-id="1a41c-181">Lásd: [PowerShell használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="1a41c-181">See [Run Pig jobs using PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span></span>

<span data-ttu-id="1a41c-182">**toosubmit Sqoop feladatok**</span><span class="sxs-lookup"><span data-stu-id="1a41c-182">**toosubmit Sqoop jobs**</span></span>

<span data-ttu-id="1a41c-183">Lásd: [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span><span class="sxs-lookup"><span data-stu-id="1a41c-183">See [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span></span>

<span data-ttu-id="1a41c-184">**toosubmit Oozie feladatok**</span><span class="sxs-lookup"><span data-stu-id="1a41c-184">**toosubmit Oozie jobs**</span></span>

<span data-ttu-id="1a41c-185">Lásd: [Hadoop toodefine és futtatása egy munkafolyamat hdinsight használata Oozie](hdinsight-use-oozie.md).</span><span class="sxs-lookup"><span data-stu-id="1a41c-185">See [Use Oozie with Hadoop toodefine and run a workflow in HDInsight](hdinsight-use-oozie.md).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="1a41c-186">Töltse fel az adatok tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="1a41c-186">Upload data tooAzure Blob storage</span></span>
<span data-ttu-id="1a41c-187">Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="1a41c-187">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="1a41c-188">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1a41c-188">See Also</span></span>
* <span data-ttu-id="1a41c-189">[HDInsight parancsmag-referencia dokumentáció][hdinsight-powershell-reference]</span><span class="sxs-lookup"><span data-stu-id="1a41c-189">[HDInsight cmdlet reference documentation][hdinsight-powershell-reference]</span></span>
* <span data-ttu-id="1a41c-190">[HDInsight felügyelete hello Azure-portál használatával][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="1a41c-190">[Administer HDInsight by using hello Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="1a41c-191">[HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="1a41c-191">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="1a41c-192">[A HDInsight-fürtök létrehozása][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="1a41c-192">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="1a41c-193">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="1a41c-193">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="1a41c-194">[Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="1a41c-194">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="1a41c-195">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="1a41c-195">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
