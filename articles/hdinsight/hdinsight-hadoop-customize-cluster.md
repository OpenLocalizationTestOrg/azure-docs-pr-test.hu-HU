---
title: "aaaCustomize a HDInsight-fürtök használata parancsfájl-műveletek - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize HDInsight clusters parancsfájlművelet használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 076fff23e016db47bc7e9963582a545ad638e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="89091-103">Parancsfájlművelet Windows-alapú HDInsight-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="89091-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="89091-104">**Parancsfájl-művelet** lehet használt tooinvoke [egyéni parancsfájlok](hdinsight-hadoop-script-actions.md) hello Fürtlétrehozási folyamat a fürt további szoftver telepítése során.</span><span class="sxs-lookup"><span data-stu-id="89091-104">**Script Action** can be used tooinvoke [custom scripts](hdinsight-hadoop-script-actions.md) during hello cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="89091-105">a cikkben szereplő információkat hello tooWindows-alapú HDInsight-fürtök adott.</span><span class="sxs-lookup"><span data-stu-id="89091-105">hello information in this article is specific tooWindows-based HDInsight clusters.</span></span> <span data-ttu-id="89091-106">Linux-alapú fürtök esetében lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="89091-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89091-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="89091-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="89091-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="89091-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="89091-109">További Azure Storage-fiókok ideértve például a HDInsight-fürtök testre szabható más módszerekkel is, számos, hello Hadoop módosítása konfigurációs fájljai (core-site.xml, hive-site.xml stb.), vagy hozzáadása a megosztott függvénytárak (például a Hive, az Oozie) be általános helyek hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="89091-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing hello Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in hello cluster.</span></span> <span data-ttu-id="89091-110">Ezek a testreszabások Azure PowerShell, hello Azure HDInsight .NET SDK használatával, vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="89091-110">These customizations can be done through Azure PowerShell, hello Azure HDInsight .NET SDK, or hello Azure portal.</span></span> <span data-ttu-id="89091-111">További információkért lásd: [Hadoop létrehozása a HDInsight-fürtök][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="89091-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-hello-cluster-creation-process"></a><span data-ttu-id="89091-112">Hello fürtlétrehozás parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="89091-112">Script Action in hello cluster creation process</span></span>
<span data-ttu-id="89091-113">Parancsfájlművelet csak akkor használható, amíg hello folyamatának létrehozása folyamatban van egy fürt.</span><span class="sxs-lookup"><span data-stu-id="89091-113">Script Action is only used while a cluster is in hello process of being created.</span></span> <span data-ttu-id="89091-114">hello alábbi ábrán látható parancsfájl hello létrehozási folyamata során kerül végrehajtásra:</span><span class="sxs-lookup"><span data-stu-id="89091-114">hello following diagram illustrates when Script Action is executed during hello creation process:</span></span>

<span data-ttu-id="89091-115">![A HDInsight fürt testreszabási és fürt létrehozása során szakaszai][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="89091-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="89091-116">Hello fürt hello parancsprogram futtatásakor kerül, hello **ClusterCustomization** szakaszban.</span><span class="sxs-lookup"><span data-stu-id="89091-116">When hello script is running, hello cluster enters hello **ClusterCustomization** stage.</span></span> <span data-ttu-id="89091-117">Ebben a szakaszban a hello parancsfájl hello rendszer rendszergazdai fiók alatt fut, minden hello párhuzamos megadott hello fürt csomópontja, és teljes rendszergazdai jogosultságokat biztosít hello csomópontján.</span><span class="sxs-lookup"><span data-stu-id="89091-117">At this stage, hello script is run under hello system admin account, in parallel on all hello specified nodes in hello cluster, and provides full admin privileges on hello nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="89091-118">Mivel a rendszergazdai jogosultságokkal rendelkezik a fürtcsomópontokon hello során a **ClusterCustomization** -szakaszban is használhat hello parancsfájl tooperform műveletek, például a szolgáltatások, beleértve a Hadoop-kapcsolatos szolgáltatások indítása és leállítása.</span><span class="sxs-lookup"><span data-stu-id="89091-118">Because you have admin privileges on hello cluster nodes during the **ClusterCustomization** stage, you can use hello script tooperform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="89091-119">Így hello parancsfájl részeként meg kell győződnie arról, hogy hello Ambari és egyéb Hadoop-kapcsolódó szolgáltatás megfelelően működik előtt hello parancsfájl befejezése után történik.</span><span class="sxs-lookup"><span data-stu-id="89091-119">So, as part of hello script, you must ensure that hello Ambari services and other Hadoop-related services are up and running before hello script finishes running.</span></span> <span data-ttu-id="89091-120">Ezek a szolgáltatások szükségesek toosuccessfully hello állapotának és hello fürt állapotának megállapítása közben létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="89091-120">These services are required toosuccessfully ascertain hello health and state of hello cluster while it is being created.</span></span> <span data-ttu-id="89091-121">Ha módosítja a fürtön, ezeket a szolgáltatásokat érintő beállításra, hello súgófunkciókat által biztosított kell használnia.</span><span class="sxs-lookup"><span data-stu-id="89091-121">If you change any configuration on the cluster that affects these services, you must use hello helper functions that are provided.</span></span> <span data-ttu-id="89091-122">Súgófunkciókat kapcsolatos további információkért lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="89091-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="89091-123">hello kimeneti és hello hibanaplókat hello parancsfájl hello alapértelmezett tárfiók hello fürt megadott vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="89091-123">hello output and hello error logs for hello script are stored in hello default Storage account you specified for hello cluster.</span></span> <span data-ttu-id="89091-124">hello naplófájljainak tárolása a hello nevű tábla **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="89091-124">hello logs are stored in a table with hello name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="89091-125">Ezek a hello parancsfájl futtatása minden csomóponton hello (átjárócsomópont és feldolgozó csomópontokat) hello fürt származó összesített naplók.</span><span class="sxs-lookup"><span data-stu-id="89091-125">These are aggregate logs from hello script run on all hello nodes (head node and worker nodes) in hello cluster.</span></span>

<span data-ttu-id="89091-126">Az egyes fürtökön fogadhatnak több Parancsfájlműveletek megadott vannak hello sorrendben definiálásra.</span><span class="sxs-lookup"><span data-stu-id="89091-126">Each cluster can accept multiple script actions that are invoked in hello order in which they are specified.</span></span> <span data-ttu-id="89091-127">Egy parancsfájlt is futott, hello átjárócsomópont, hello munkavégző csomópontokhoz vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="89091-127">A script can be ran on hello head node, hello worker nodes, or both.</span></span>

<span data-ttu-id="89091-128">HDInsight biztosít több parancsfájlok tooinstall hello összetevők HDInsight-fürtök a következő:</span><span class="sxs-lookup"><span data-stu-id="89091-128">HDInsight provides several scripts tooinstall hello following components on HDInsight clusters:</span></span>

| <span data-ttu-id="89091-129">Név</span><span class="sxs-lookup"><span data-stu-id="89091-129">Name</span></span> | <span data-ttu-id="89091-130">Szkript</span><span class="sxs-lookup"><span data-stu-id="89091-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="89091-131">**Spark telepítése**</span><span class="sxs-lookup"><span data-stu-id="89091-131">**Install Spark**</span></span> |<span data-ttu-id="89091-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="89091-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="89091-133">Lásd: [telepítése és használata a HDInsight Spark-fürtök][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="89091-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="89091-134">**R telepítéséhez**</span><span class="sxs-lookup"><span data-stu-id="89091-134">**Install R**</span></span> |<span data-ttu-id="89091-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="89091-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="89091-136">Lásd: [telepítése és használata R HDInsight-fürtök][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="89091-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="89091-137">**Solr telepítése**</span><span class="sxs-lookup"><span data-stu-id="89091-137">**Install Solr**</span></span> |<span data-ttu-id="89091-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89091-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="89091-139">Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="89091-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="89091-140">- **Giraph telepítése**</span><span class="sxs-lookup"><span data-stu-id="89091-140">- **Install Giraph**</span></span> |<span data-ttu-id="89091-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89091-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="89091-142">Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="89091-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="89091-143">**Hive-könyvtárakhoz előzetes betöltése**</span><span class="sxs-lookup"><span data-stu-id="89091-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="89091-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="89091-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="89091-145">Lásd: [kódtárak hozzáadása Hive HDInsight-fürtök](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="89091-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-hello-azure-portal"></a><span data-ttu-id="89091-146">Hívás parancsfájlok hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="89091-146">Call scripts using hello Azure portal</span></span>
<span data-ttu-id="89091-147">**A hello Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="89091-147">**From hello Azure portal**</span></span>

1. <span data-ttu-id="89091-148">Indítsa el a fürt létrehozása a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="89091-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="89091-149">A választható konfigurációs, hello **Parancsfájlműveletek** panelen kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="89091-149">Under Optional Configuration, for hello **Script Actions** blade, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="89091-150">![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "használata parancsfájlművelet toocustomize fürt")</span><span class="sxs-lookup"><span data-stu-id="89091-150">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="89091-151">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="89091-151">Property</span></span></th><th><span data-ttu-id="89091-152">Érték</span><span class="sxs-lookup"><span data-stu-id="89091-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="89091-153">Név</span><span class="sxs-lookup"><span data-stu-id="89091-153">Name</span></span></td>
            <td><span data-ttu-id="89091-154">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="89091-154">Specify a name for hello script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="89091-155">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="89091-155">Script URI</span></span></td>
            <td><span data-ttu-id="89091-156">Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="89091-156">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="89091-157">s</span><span class="sxs-lookup"><span data-stu-id="89091-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="89091-158">HEAD/munkavégző</span><span class="sxs-lookup"><span data-stu-id="89091-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="89091-159">Adja meg a hello csomópontok (**Head** vagy **munkavégző**) mely hello testreszabási a parancsfájl futtatása.</b>.</span><span class="sxs-lookup"><span data-stu-id="89091-159">Specify hello nodes (**Head** or **Worker**) on which hello customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="89091-160">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="89091-160">Parameters</span></span></td>
            <td><span data-ttu-id="89091-161">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="89091-161">Specify hello parameters, if required by hello script.</span></span></td></tr>
    </table>

    <span data-ttu-id="89091-162">Nyomja le az ENTER tooadd több mint egy parancsfájl művelet tooinstall több összetevő hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="89091-162">Press ENTER tooadd more than one script action tooinstall multiple components on hello cluster.</span></span>
3. <span data-ttu-id="89091-163">Kattintson a **válasszon** toosave hello parancsprogram művelet konfigurációját, és folytassa a fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="89091-163">Click **Select** toosave hello script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="89091-164">Hívja meg az Azure PowerShell parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="89091-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="89091-165">A következő PowerShell-parancsfájl bemutatja, hogyan tooinstall a Spark on Windows alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="89091-165">This following PowerShell script demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" toolist IDs.

    $nameToken = "<Enter A Name Token>"  # hello token is use toocreate Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect tooAzure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare hello dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify hello configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action toohello cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


<span data-ttu-id="89091-166">tooinstall egyéb szoftvereihez, szüksége lesz tooreplace hello parancsfájl hello parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="89091-166">tooinstall other software, you will need tooreplace hello script file in hello script:</span></span>

<span data-ttu-id="89091-167">Amikor a rendszer kéri, adja meg hello hitelesítő hello fürt.</span><span class="sxs-lookup"><span data-stu-id="89091-167">When prompted, enter hello credentials for hello cluster.</span></span> <span data-ttu-id="89091-168">Hello fürt létrehozása előtt több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="89091-168">It can take several minutes before hello cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="89091-169">Hívás parancsfájlok .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="89091-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="89091-170">hello következő minta bemutatja, hogyan tooinstall a Spark on Windows alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="89091-170">hello following sample demonstrates how tooinstall Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="89091-171">tooinstall egyéb szoftvereihez, szüksége lesz tooreplace hello parancsfájl hello kódot.</span><span class="sxs-lookup"><span data-stu-id="89091-171">tooinstall other software, you will need tooreplace hello script file in hello code.</span></span>

<span data-ttu-id="89091-172">**a Spark HDInsight-fürtök toocreate**</span><span class="sxs-lookup"><span data-stu-id="89091-172">**toocreate an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="89091-173">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="89091-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="89091-174">Hello Nuget Package Manager Console futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="89091-174">From hello Nuget Package Manager Console, run hello following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="89091-175">A következő using utasításokat a Program.cs fájlban hello használata hello:</span><span class="sxs-lookup"><span data-stu-id="89091-175">Use hello following using statements in hello Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="89091-176">Helyezze hello kódot hello osztály hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="89091-176">Place hello code in hello class with hello following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate tooan Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">hello AAD tenant ID</param>
        /// <param name="ClientId">hello AAD client ID</param>
        /// <param name="SubscriptionId">hello Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for hello Resource manager and set hello subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register hello HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="89091-177">Nyomja le az **F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="89091-177">Press **F5** toorun hello application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="89091-178">A HDInsight-fürtökön használt nyílt forráskódú szoftvereket támogatása</span><span class="sxs-lookup"><span data-stu-id="89091-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="89091-179">a Microsoft Azure HDInsight szolgáltatás hello egy rugalmas platform, amely lehetővé teszi az ökoszisztéma formátumú-e körül Hadoop nyílt forráskódú technológiák használatával toobuild big data-alkalmazások hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="89091-179">hello Microsoft Azure HDInsight service is a flexible platform that enables you toobuild big-data applications in hello cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="89091-180">A Microsoft Azure biztosít, általános támogatási nyílt forráskódú technológiák hello leírtaknak megfelelően **támogatja a hatókör** hello szakasza <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure támogatási gyakori Kérdéseinek webhelyre</a>.</span><span class="sxs-lookup"><span data-stu-id="89091-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in hello **Support Scope** section of hello <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="89091-181">HDInsight szolgáltatás hello biztosít egy további néhány hello összetevőjét támogatása az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="89091-181">hello HDInsight service provides an additional level of support for some of hello components, as described below.</span></span>

<span data-ttu-id="89091-182">Nyílt forráskódú összetevők hello HDInsight szolgáltatás elérhető két típusa van:</span><span class="sxs-lookup"><span data-stu-id="89091-182">There are two types of open-source components that are available in hello HDInsight service:</span></span>

* <span data-ttu-id="89091-183">**A beépített összetevők** -ezeket az összetevőket a HDInsight-fürtök előre telepített, és alapfunkciókat hello fürt adja meg.</span><span class="sxs-lookup"><span data-stu-id="89091-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of hello cluster.</span></span> <span data-ttu-id="89091-184">YARN ResourceManager, hello Hive query language (HiveQL) és hello Mahout könyvtár például toothis kategóriába tartozik.</span><span class="sxs-lookup"><span data-stu-id="89091-184">For example, YARN ResourceManager, hello Hive query language (HiveQL), and hello Mahout library belong toothis category.</span></span> <span data-ttu-id="89091-185">A kiszolgálófürt-összetevők teljes listáját a érhető [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="89091-185">A full list of cluster components is available in [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="89091-186">**Egyéni összetevők** -hello fürt felhasználóként, telepítése vagy használata előtt a munkaterhelésnek valamelyik összetevő a hello közösségi vagy Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="89091-186">**Custom components** - You, as a user of hello cluster, can install or use in your workload any component available in hello community or created by you.</span></span>

<span data-ttu-id="89091-187">A beépített összetevők teljes mértékben támogatottak, és Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="89091-187">Built-in components are fully supported, and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>

> [!WARNING]
> <span data-ttu-id="89091-188">Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="89091-188">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="89091-189">Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez.</span><span class="sxs-lookup"><span data-stu-id="89091-189">Custom components receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="89091-190">Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="89091-190">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="89091-191">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="89091-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="89091-192">HDInsight-szolgáltatás hello toouse egyéni összetevők számos lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="89091-192">hello HDInsight service provides several ways toouse custom components.</span></span> <span data-ttu-id="89091-193">Függetlenül attól, milyen összetevő használt vagy hello fürt telepíteni hello azonos szintű támogatást vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="89091-193">Regardless of how a component is used or installed on hello cluster, hello same level of support applies.</span></span> <span data-ttu-id="89091-194">Az alábbiakban olvashat egy listát hello leggyakoribb módon, hogy a HDInsight-fürtök egyéni összetevők használható:</span><span class="sxs-lookup"><span data-stu-id="89091-194">Below is a list of hello most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="89091-195">Feladat elküldése - Hadoop és egyéb feladatok végrehajtása, vagy használjon egyéni összetevők elküldött toohello tárolófürt is lehet.</span><span class="sxs-lookup"><span data-stu-id="89091-195">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted toohello cluster.</span></span>
2. <span data-ttu-id="89091-196">Fürt testreszabási - fürt létrehozása során megadhatja további beállításokat és egyéni hello fürtcsomópontokon telepített összetevők.</span><span class="sxs-lookup"><span data-stu-id="89091-196">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on hello cluster nodes.</span></span>
3. <span data-ttu-id="89091-197">Minták – a népszerű egyéni összetevők, a Microsoft és a mások által biztosított mintákat ezeket az összetevőket hogyan használható a hello HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="89091-197">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on hello HDInsight clusters.</span></span> <span data-ttu-id="89091-198">Ezeket a mintákat támogatás nélkül van megadva.</span><span class="sxs-lookup"><span data-stu-id="89091-198">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="89091-199">Parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="89091-199">Develop Script Action scripts</span></span>
<span data-ttu-id="89091-200">Lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="89091-200">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="89091-201">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="89091-201">See also</span></span>
* <span data-ttu-id="89091-202">[Hdinsight Hadoop-fürtök létrehozása] [ hdinsight-provision-cluster] hogyan toocreate egy HDInsight fürt más egyéni beállítások használatával kapcsolatos utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="89091-202">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how toocreate an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="89091-203">[A HDInsight parancsfájlművelet-parancsfájlok fejlesztése][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="89091-203">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="89091-204">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="89091-204">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="89091-205">[Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="89091-205">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="89091-206">[Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="89091-206">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="89091-207">[Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="89091-207">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"
