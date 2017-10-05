---
title: "A PowerShell - Azure hdinsight Hadoop-fürtök kezelése |} Microsoft Docs"
description: "Útmutató az Azure PowerShell hdinsight Hadoop-fürtök felügyeleti feladatokat hajthat végre."
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
ms.openlocfilehash: c47dabd7c4aa4ba0be08c419989e536711f03677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a><span data-ttu-id="512b8-103">Hdinsight Hadoop-fürtök kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="512b8-103">Manage Hadoop clusters in HDInsight by using Azure PowerShell</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="512b8-104">Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti a munkaterhelések az Azure-ban, és szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="512b8-104">Azure PowerShell is a powerful scripting environment that you can use to control and automate the deployment and management of your workloads in Azure.</span></span> <span data-ttu-id="512b8-105">Ön ebből a cikkből megtudhatja, hogyan Azure hdinsight Hadoop-fürtök kezelése a helyi Azure PowerShell-konzolban révén Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="512b8-105">In this article, you will learn how to manage Hadoop clusters in Azure HDInsight by using a local Azure PowerShell console through the use of Windows PowerShell.</span></span> <span data-ttu-id="512b8-106">A HDInsight PowerShell-parancsmagok listáját lásd: [HDInsight parancsmag-referencia][hdinsight-powershell-reference].</span><span class="sxs-lookup"><span data-stu-id="512b8-106">For the list of the HDInsight PowerShell cmdlets, see [HDInsight cmdlet reference][hdinsight-powershell-reference].</span></span>

<span data-ttu-id="512b8-107">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="512b8-107">**Prerequisites**</span></span>

<span data-ttu-id="512b8-108">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="512b8-108">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="512b8-109">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="512b8-109">**An Azure subscription**.</span></span> <span data-ttu-id="512b8-110">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="512b8-110">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="512b8-111">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="512b8-111">Install Azure PowerShell</span></span>
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="512b8-112">Ha telepítette az Azure PowerShell 0,9 verziója x, el kell távolítani egy újabb verzió telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="512b8-112">If you have installed Azure PowerShell version 0.9x, you must uninstall it before installing a newer version.</span></span>

<span data-ttu-id="512b8-113">A telepített PowerShell verziójának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="512b8-113">To check the version of the installed PowerShell:</span></span>

    Get-Module *azure*

<span data-ttu-id="512b8-114">Távolítsa el a régebbi verziót, hogy futtassa a Vezérlőpult Programok és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="512b8-114">To uninstall the older version, run Programs and Features in the control panel.</span></span>

## <a name="create-clusters"></a><span data-ttu-id="512b8-115">Fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="512b8-115">Create clusters</span></span>
<span data-ttu-id="512b8-116">Lásd: [fürtök létrehozása Linux-alapú hdinsight Azure PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="512b8-116">See [Create Linux-based clusters in HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>

## <a name="list-clusters"></a><span data-ttu-id="512b8-117">Lista fürtök</span><span class="sxs-lookup"><span data-stu-id="512b8-117">List clusters</span></span>
<span data-ttu-id="512b8-118">Használja a következő parancsot a jelenlegi előfizetés az összes fürt listázásához:</span><span class="sxs-lookup"><span data-stu-id="512b8-118">Use the following command to list all clusters in the current subscription:</span></span>

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a><span data-ttu-id="512b8-119">Fürt megjelenítése</span><span class="sxs-lookup"><span data-stu-id="512b8-119">Show cluster</span></span>
<span data-ttu-id="512b8-120">A következő paranccsal egy adott fürt részleteinek megjelenítése az aktuális előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="512b8-120">Use the following command to show details of a specific cluster in the current subscription:</span></span>

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a><span data-ttu-id="512b8-121">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="512b8-121">Delete clusters</span></span>
<span data-ttu-id="512b8-122">Az alábbi parancs segítségével törölheti a fürtöt:</span><span class="sxs-lookup"><span data-stu-id="512b8-122">Use the following command to delete a cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

<span data-ttu-id="512b8-123">A fürt a fürt tartalmazó erőforráscsoportot eltávolításával is törli.</span><span class="sxs-lookup"><span data-stu-id="512b8-123">You can also delete a cluster by removing the resource group that contains the cluster.</span></span> <span data-ttu-id="512b8-124">Vegye figyelembe, többek között az alapértelmezett tárfiók csoportban található összes erőforrást törölni szeretné.</span><span class="sxs-lookup"><span data-stu-id="512b8-124">Please note, this will delete all the resources in the group including the default storage account.</span></span>

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="512b8-125">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="512b8-125">Scale clusters</span></span>
<span data-ttu-id="512b8-126">A fürt skálázás funkciót lehetővé teszi, hogy anélkül, hogy újra létre kell hoznia a fürt fut az Azure HDInsight fürt által használt feldolgozó csomópontok számának módosítása.</span><span class="sxs-lookup"><span data-stu-id="512b8-126">The cluster scaling feature allows you to change the number of worker nodes used by a cluster that is running in Azure HDInsight without having to re-create the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="512b8-127">Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak.</span><span class="sxs-lookup"><span data-stu-id="512b8-127">Only clusters with HDInsight version 3.1.3 or higher are supported.</span></span> <span data-ttu-id="512b8-128">Ha biztos benne, hogy a fürt verzióját, a Tulajdonságok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="512b8-128">If you are unsure of the version of your cluster, you can check the Properties page.</span></span>  <span data-ttu-id="512b8-129">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="512b8-129">See [List and show clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).</span></span>
>
>

<span data-ttu-id="512b8-130">A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosítása következményei:</span><span class="sxs-lookup"><span data-stu-id="512b8-130">The impact of changing the number of data nodes for each type of cluster supported by HDInsight:</span></span>

* <span data-ttu-id="512b8-131">Hadoop</span><span class="sxs-lookup"><span data-stu-id="512b8-131">Hadoop</span></span>

    <span data-ttu-id="512b8-132">Zökkenőmentesen növelheti adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát.</span><span class="sxs-lookup"><span data-stu-id="512b8-132">You can seamlessly increase the number of worker nodes in a Hadoop cluster that is running without impacting any pending or running jobs.</span></span> <span data-ttu-id="512b8-133">Új feladatokat is küldheti el, amíg a művelet folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="512b8-133">New jobs can also be submitted while the operation is in progress.</span></span> <span data-ttu-id="512b8-134">A méretezési művelet sikertelen szabályosan kezeli, hogy a fürt mindig működőképes állapotban marad.</span><span class="sxs-lookup"><span data-stu-id="512b8-134">Failures in a scaling operation are gracefully handled so that the cluster is always left in a functional state.</span></span>

    <span data-ttu-id="512b8-135">A Hadoop fürtök adatok csomópontok számának csökkentésével átméretezi, ha néhány, a fürt a szolgáltatások újraindításáig.</span><span class="sxs-lookup"><span data-stu-id="512b8-135">When a Hadoop cluster is scaled down by reducing the number of data nodes, some of the services in the cluster are restarted.</span></span> <span data-ttu-id="512b8-136">Ennek hatására a összes futó és függőben lévő feladatok meghiúsulhatnak, a méretezési művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="512b8-136">This causes all running and pending jobs to fail at the completion of the scaling operation.</span></span> <span data-ttu-id="512b8-137">Akkor is, azonban küldje el újra a feladatok a művelet végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="512b8-137">You can, however, resubmit the jobs once the operation is complete.</span></span>
* <span data-ttu-id="512b8-138">HBase</span><span class="sxs-lookup"><span data-stu-id="512b8-138">HBase</span></span>

    <span data-ttu-id="512b8-139">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához a HBase-fürtöt a futtatása.</span><span class="sxs-lookup"><span data-stu-id="512b8-139">You can seamlessly add or remove nodes to your HBase cluster while it is running.</span></span> <span data-ttu-id="512b8-140">Területi kiszolgálók automatikus elosztását a méretezési művelet befejezését néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="512b8-140">Regional Servers are automatically balanced within a few minutes of completing the scaling operation.</span></span> <span data-ttu-id="512b8-141">Azonban Ön is manuálisan is elosztása a regionális kiszolgálók fürt headnode való bejelentkezés, és futtatja a következő parancsokat egy parancssori ablakot:</span><span class="sxs-lookup"><span data-stu-id="512b8-141">However, you can also manually balance the regional servers by logging in to the headnode of cluster and running the following commands from a command prompt window:</span></span>

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* <span data-ttu-id="512b8-142">Storm</span><span class="sxs-lookup"><span data-stu-id="512b8-142">Storm</span></span>

    <span data-ttu-id="512b8-143">Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához adatok Storm fürthöz való futtatása során.</span><span class="sxs-lookup"><span data-stu-id="512b8-143">You can seamlessly add or remove data nodes to your Storm cluster while it is running.</span></span> <span data-ttu-id="512b8-144">De a méretezési művelet sikeres befejezését követően szüksége lesz a topológia egyensúlyba.</span><span class="sxs-lookup"><span data-stu-id="512b8-144">But after a successful completion of the scaling operation, you will need to rebalance the topology.</span></span>

    <span data-ttu-id="512b8-145">Kétféle módon valósítható meg újraelosztás:</span><span class="sxs-lookup"><span data-stu-id="512b8-145">Rebalancing can be accomplished in two ways:</span></span>

  * <span data-ttu-id="512b8-146">A Storm webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="512b8-146">Storm web UI</span></span>
  * <span data-ttu-id="512b8-147">Parancssori felület (CLI) eszköz</span><span class="sxs-lookup"><span data-stu-id="512b8-147">Command-line interface (CLI) tool</span></span>

    <span data-ttu-id="512b8-148">Tekintse meg a [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="512b8-148">Please refer to the [Apache Storm documentation](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) for more details.</span></span>

    <span data-ttu-id="512b8-149">A Storm webes felhasználói felület érhető el a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="512b8-149">The Storm web UI is available on the HDInsight cluster:</span></span>

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    <span data-ttu-id="512b8-151">Íme egy példa a CLI parancs használata a Storm-topológia egyensúlyba:</span><span class="sxs-lookup"><span data-stu-id="512b8-151">Here is an example how to use the CLI command to rebalance the Storm topology:</span></span>

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

<span data-ttu-id="512b8-152">Ha módosítani szeretné a Hadoop-fürt mérete Azure PowerShell használatával, a következő parancsot a egy ügyfélszámítógépen:</span><span class="sxs-lookup"><span data-stu-id="512b8-152">To change the Hadoop cluster size by using Azure PowerShell, run the following command from a client machine:</span></span>

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a><span data-ttu-id="512b8-153">Hozzáférés biztosítása/visszavonása</span><span class="sxs-lookup"><span data-stu-id="512b8-153">Grant/revoke access</span></span>
<span data-ttu-id="512b8-154">A HDInsight-fürtök a következő HTTP webszolgáltatásokat (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="512b8-154">HDInsight clusters have the following HTTP web services (all of these services have RESTful endpoints):</span></span>

* <span data-ttu-id="512b8-155">ODBC</span><span class="sxs-lookup"><span data-stu-id="512b8-155">ODBC</span></span>
* <span data-ttu-id="512b8-156">JDBC</span><span class="sxs-lookup"><span data-stu-id="512b8-156">JDBC</span></span>
* <span data-ttu-id="512b8-157">Ambari</span><span class="sxs-lookup"><span data-stu-id="512b8-157">Ambari</span></span>
* <span data-ttu-id="512b8-158">Oozie</span><span class="sxs-lookup"><span data-stu-id="512b8-158">Oozie</span></span>
* <span data-ttu-id="512b8-159">Lépni a Templeton</span><span class="sxs-lookup"><span data-stu-id="512b8-159">Templeton</span></span>

<span data-ttu-id="512b8-160">Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított.</span><span class="sxs-lookup"><span data-stu-id="512b8-160">By default, these services are granted for access.</span></span> <span data-ttu-id="512b8-161">Meg is visszavonási/engedélyezze a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="512b8-161">You can revoke/grant the access.</span></span> <span data-ttu-id="512b8-162">Visszavonni:</span><span class="sxs-lookup"><span data-stu-id="512b8-162">To revoke:</span></span>

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

<span data-ttu-id="512b8-163">Megadását:</span><span class="sxs-lookup"><span data-stu-id="512b8-163">To grant:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> <span data-ttu-id="512b8-164">A hozzáférés biztosítása/visszavonása, visszaállítja, a fürt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="512b8-164">By granting/revoking the access, you will reset the cluster user name and password.</span></span>
>
>

<span data-ttu-id="512b8-165">Ez a portálon keresztül is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="512b8-165">This can also be done via the Portal.</span></span> <span data-ttu-id="512b8-166">Lásd: [felügyelheti a HDInsight az Azure portál használatával][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="512b8-166">See [Administer HDInsight by using the Azure portal][hdinsight-admin-portal].</span></span>

## <a name="update-http-user-credentials"></a><span data-ttu-id="512b8-167">HTTP-felhasználó hitelesítő adatainak frissítése</span><span class="sxs-lookup"><span data-stu-id="512b8-167">Update HTTP user credentials</span></span>
<span data-ttu-id="512b8-168">Ugyanezt az eljárást, mint a [Grant/revoke HTTP access](#grant/revoke-access). Ha a fürt megadták a HTTP-hozzáférést, meg kell először vonja vissza.</span><span class="sxs-lookup"><span data-stu-id="512b8-168">It is the same procedure as [Grant/revoke HTTP access](#grant/revoke-access).If the cluster has been granted the HTTP access, you must first revoke it.</span></span>  <span data-ttu-id="512b8-169">És adja meg a hozzáférés új HTTP felhasználói hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="512b8-169">And then grant the access with new HTTP user credentials.</span></span>

## <a name="find-the-default-storage-account"></a><span data-ttu-id="512b8-170">Az alapértelmezett tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="512b8-170">Find the default storage account</span></span>
<span data-ttu-id="512b8-171">A következő Powershell-parancsfájl bemutatja, hogyan kell az alapértelmezett tárfiók neve és a alapértelmezett tárfiók hívóbetűjét fürt.</span><span class="sxs-lookup"><span data-stu-id="512b8-171">The following Powershell script demonstrates how to get the default storage account name and the default storage account key for a cluster.</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-the-resource-group"></a><span data-ttu-id="512b8-172">Az erőforráscsoport keresése</span><span class="sxs-lookup"><span data-stu-id="512b8-172">Find the resource group</span></span>
<span data-ttu-id="512b8-173">Az erőforrás-kezelő módban egy Azure-erőforráscsoportot minden HDInsight-fürthöz tartozik.</span><span class="sxs-lookup"><span data-stu-id="512b8-173">In the Resource Manager mode, each HDInsight cluster belongs to an Azure resource group.</span></span>  <span data-ttu-id="512b8-174">Az erőforráscsoport megkeresése:</span><span class="sxs-lookup"><span data-stu-id="512b8-174">To find the resource group:</span></span>

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a><span data-ttu-id="512b8-175">Feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="512b8-175">Submit jobs</span></span>
<span data-ttu-id="512b8-176">**Elküldeni a MapReduce-feladatok**</span><span class="sxs-lookup"><span data-stu-id="512b8-176">**To submit MapReduce jobs**</span></span>

<span data-ttu-id="512b8-177">Lásd: [Windows-alapú hdinsight Hadoop-MapReduce futtatása minták](hdinsight-run-samples.md).</span><span class="sxs-lookup"><span data-stu-id="512b8-177">See [Run Hadoop MapReduce samples in Windows-based HDInsight](hdinsight-run-samples.md).</span></span>

<span data-ttu-id="512b8-178">**Elküldeni a Hive-feladatok**</span><span class="sxs-lookup"><span data-stu-id="512b8-178">**To submit Hive jobs**</span></span>

<span data-ttu-id="512b8-179">Lásd: [PowerShell használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="512b8-179">See [Run Hive queries using PowerShell](hdinsight-hadoop-use-hive-powershell.md).</span></span>

<span data-ttu-id="512b8-180">**Elküldeni a Pig-feladatokhoz**</span><span class="sxs-lookup"><span data-stu-id="512b8-180">**To submit Pig jobs**</span></span>

<span data-ttu-id="512b8-181">Lásd: [PowerShell használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="512b8-181">See [Run Pig jobs using PowerShell](hdinsight-hadoop-use-pig-powershell.md).</span></span>

<span data-ttu-id="512b8-182">**Sqoop feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="512b8-182">**To submit Sqoop jobs**</span></span>

<span data-ttu-id="512b8-183">Lásd: [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span><span class="sxs-lookup"><span data-stu-id="512b8-183">See [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).</span></span>

<span data-ttu-id="512b8-184">**Oozie feladatok küldéséhez**</span><span class="sxs-lookup"><span data-stu-id="512b8-184">**To submit Oozie jobs**</span></span>

<span data-ttu-id="512b8-185">Lásd: [hadooppal megadásához és a munkafolyamat futtatása hdinsight használata Oozie](hdinsight-use-oozie.md).</span><span class="sxs-lookup"><span data-stu-id="512b8-185">See [Use Oozie with Hadoop to define and run a workflow in HDInsight](hdinsight-use-oozie.md).</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="512b8-186">Adatok feltöltése az Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="512b8-186">Upload data to Azure Blob storage</span></span>
<span data-ttu-id="512b8-187">Lásd: [Adatok feltöltése a HDInsightba][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="512b8-187">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

## <a name="see-also"></a><span data-ttu-id="512b8-188">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="512b8-188">See Also</span></span>
* <span data-ttu-id="512b8-189">[HDInsight parancsmag-referencia dokumentáció][hdinsight-powershell-reference]</span><span class="sxs-lookup"><span data-stu-id="512b8-189">[HDInsight cmdlet reference documentation][hdinsight-powershell-reference]</span></span>
* <span data-ttu-id="512b8-190">[HDInsight felügyelete az Azure-portál használatával][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="512b8-190">[Administer HDInsight by using the Azure portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="512b8-191">[HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]</span><span class="sxs-lookup"><span data-stu-id="512b8-191">[Administer HDInsight using a command-line interface][hdinsight-admin-cli]</span></span>
* <span data-ttu-id="512b8-192">[A HDInsight-fürtök létrehozása][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="512b8-192">[Create HDInsight clusters][hdinsight-provision]</span></span>
* <span data-ttu-id="512b8-193">[Adatok feltöltése a HDInsightba][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="512b8-193">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="512b8-194">[Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]</span><span class="sxs-lookup"><span data-stu-id="512b8-194">[Submit Hadoop jobs programmatically][hdinsight-submit-jobs]</span></span>
* <span data-ttu-id="512b8-195">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="512b8-195">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>

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
