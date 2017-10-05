---
title: "A Parancsfájlműveletek - Azure HDInsight-fürtök testreszabása |} Microsoft Docs"
description: "Ismerje meg a parancsfájlművelet HDInsight-fürtök testreszabása."
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
ms.openlocfilehash: ec95b6d66c71b4278dd1e16807fcc75f5e8b1c36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a><span data-ttu-id="bc996-103">Parancsfájlművelet Windows-alapú HDInsight-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="bc996-103">Customize Windows-based HDInsight clusters using Script Action</span></span>
<span data-ttu-id="bc996-104">**Parancsfájl-művelet** alkalmazásához használt [egyéni parancsfájlok](hdinsight-hadoop-script-actions.md) a fürt további szoftver telepítése a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="bc996-104">**Script Action** can be used to invoke [custom scripts](hdinsight-hadoop-script-actions.md) during the cluster creation process for installing additional software on a cluster.</span></span>

<span data-ttu-id="bc996-105">Ebben a cikkben szereplő információk csak a Windows-alapú HDInsight-fürtök az elő.</span><span class="sxs-lookup"><span data-stu-id="bc996-105">The information in this article is specific to Windows-based HDInsight clusters.</span></span> <span data-ttu-id="bc996-106">Linux-alapú fürtök esetében lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-106">For Linux-based clusters, see [Customize Linux-based HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc996-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="bc996-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bc996-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bc996-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="bc996-109">További Azure Storage-fiókok ideértve például a HDInsight-fürtök testre szabható más módszerekkel is, számos, a Hadoop módosítása konfigurációs fájljai (core-site.xml, hive-site.xml stb.), vagy a megosztott függvénytárak (például a Hive, az Oozie) hozzáadása a fürt közös helyen való.</span><span class="sxs-lookup"><span data-stu-id="bc996-109">HDInsight clusters can be customized in a variety of other ways as well, such as including additional Azure Storage accounts, changing the Hadoop configuration files (core-site.xml, hive-site.xml, etc.), or adding shared libraries (e.g., Hive, Oozie) into common locations in the cluster.</span></span> <span data-ttu-id="bc996-110">Ezek a testreszabások végezhető el az Azure PowerShell, az Azure HDInsight .NET SDK vagy az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bc996-110">These customizations can be done through Azure PowerShell, the Azure HDInsight .NET SDK, or the Azure portal.</span></span> <span data-ttu-id="bc996-111">További információkért lásd: [Hadoop létrehozása a HDInsight-fürtök][hdinsight-provision-cluster].</span><span class="sxs-lookup"><span data-stu-id="bc996-111">For more information, see [Create Hadoop clusters in HDInsight][hdinsight-provision-cluster].</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a><span data-ttu-id="bc996-112">A Fürtlétrehozási folyamat parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="bc996-112">Script Action in the cluster creation process</span></span>
<span data-ttu-id="bc996-113">Parancsfájl művelet csak akkor használatos, miközben a fürt létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="bc996-113">Script Action is only used while a cluster is in the process of being created.</span></span> <span data-ttu-id="bc996-114">A következő ábra szemlélteti a parancsfájl művelet végrehajtásakor a létrehozási folyamat során:</span><span class="sxs-lookup"><span data-stu-id="bc996-114">The following diagram illustrates when Script Action is executed during the creation process:</span></span>

<span data-ttu-id="bc996-115">![A HDInsight fürt testreszabási és fürt létrehozása során szakaszai][img-hdi-cluster-states]</span><span class="sxs-lookup"><span data-stu-id="bc996-115">![HDInsight cluster customization and stages during cluster creation][img-hdi-cluster-states]</span></span>

<span data-ttu-id="bc996-116">A parancsprogram futtatásakor kerül, a fürt a **ClusterCustomization** szakaszban.</span><span class="sxs-lookup"><span data-stu-id="bc996-116">When the script is running, the cluster enters the **ClusterCustomization** stage.</span></span> <span data-ttu-id="bc996-117">Ebben a szakaszban a parancsfájlt a fürt összes az adott csomóponton párhuzamosan a rendszer rendszergazdai fiók alatt fut, és teljes rendszergazdai jogosultságokat biztosít a csomópontokon.</span><span class="sxs-lookup"><span data-stu-id="bc996-117">At this stage, the script is run under the system admin account, in parallel on all the specified nodes in the cluster, and provides full admin privileges on the nodes.</span></span>

> [!NOTE]
> <span data-ttu-id="bc996-118">Mivel a rendszergazdai jogosultságokkal rendelkezik a fürtcsomópontokon során a **ClusterCustomization** szakasza alatt a parancsfájlt használhatja például a szolgáltatások, beleértve a Hadoop-kapcsolatos szolgáltatások indítása és leállítása műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="bc996-118">Because you have admin privileges on the cluster nodes during the **ClusterCustomization** stage, you can use the script to perform operations like stopping and starting services, including Hadoop-related services.</span></span> <span data-ttu-id="bc996-119">Igen, a parancsfájl gondoskodnia kell arról, hogy az Ambari és egyéb Hadoop-kapcsolódó szolgáltatás megfelelően működik, mielőtt a parancsfájl befejezése után történik.</span><span class="sxs-lookup"><span data-stu-id="bc996-119">So, as part of the script, you must ensure that the Ambari services and other Hadoop-related services are up and running before the script finishes running.</span></span> <span data-ttu-id="bc996-120">Ezek a szolgáltatások szükségesek állapotát és a fürt állapotának sikeresen megállapítása közben létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="bc996-120">These services are required to successfully ascertain the health and state of the cluster while it is being created.</span></span> <span data-ttu-id="bc996-121">Ha módosítja a fürtön, ezeket a szolgáltatásokat érintő beállításra, a súgófunkciókat által biztosított kell használnia.</span><span class="sxs-lookup"><span data-stu-id="bc996-121">If you change any configuration on the cluster that affects these services, you must use the helper functions that are provided.</span></span> <span data-ttu-id="bc996-122">Súgófunkciókat kapcsolatos további információkért lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="bc996-122">For more information about helper functions, see [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>
>
>

<span data-ttu-id="bc996-123">A kimeneti és a hibanaplókat a parancsfájl az alapértelmezett tárfiókot, a fürt számára megadott vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="bc996-123">The output and the error logs for the script are stored in the default Storage account you specified for the cluster.</span></span> <span data-ttu-id="bc996-124">A naplók tárolódnak a nevű tábla **u < \cluster-name-fragment >< \time-stamp > setuplog**.</span><span class="sxs-lookup"><span data-stu-id="bc996-124">The logs are stored in a table with the name **u<\cluster-name-fragment><\time-stamp>setuplog**.</span></span> <span data-ttu-id="bc996-125">Ezek a összesített napló, a parancsfájl futhat az összes (átjárócsomópont és feldolgozó csomópontokat) a fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="bc996-125">These are aggregate logs from the script run on all the nodes (head node and worker nodes) in the cluster.</span></span>

<span data-ttu-id="bc996-126">Az egyes fürtökön fogadhatnak kerül meghívásra, amely a megadott sorrendben több parancsfájl-műveletek.</span><span class="sxs-lookup"><span data-stu-id="bc996-126">Each cluster can accept multiple script actions that are invoked in the order in which they are specified.</span></span> <span data-ttu-id="bc996-127">Egy parancsfájlt is futott, az átjárócsomópont, a munkavégző csomópontokhoz vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="bc996-127">A script can be ran on the head node, the worker nodes, or both.</span></span>

<span data-ttu-id="bc996-128">HDInsight több parancsprogramokat a HDInsight-fürtök az alábbi összetevők telepítése itt:</span><span class="sxs-lookup"><span data-stu-id="bc996-128">HDInsight provides several scripts to install the following components on HDInsight clusters:</span></span>

| <span data-ttu-id="bc996-129">Név</span><span class="sxs-lookup"><span data-stu-id="bc996-129">Name</span></span> | <span data-ttu-id="bc996-130">Szkript</span><span class="sxs-lookup"><span data-stu-id="bc996-130">Script</span></span> |
| --- | --- |
| <span data-ttu-id="bc996-131">**Spark telepítése**</span><span class="sxs-lookup"><span data-stu-id="bc996-131">**Install Spark**</span></span> |<span data-ttu-id="bc996-132">https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1.</span><span class="sxs-lookup"><span data-stu-id="bc996-132">https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1.</span></span> <span data-ttu-id="bc996-133">Lásd: [telepítése és használata a HDInsight Spark-fürtök][hdinsight-install-spark].</span><span class="sxs-lookup"><span data-stu-id="bc996-133">See [Install and use Spark on HDInsight clusters][hdinsight-install-spark].</span></span> |
| <span data-ttu-id="bc996-134">**R telepítéséhez**</span><span class="sxs-lookup"><span data-stu-id="bc996-134">**Install R**</span></span> |<span data-ttu-id="bc996-135">https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1.</span><span class="sxs-lookup"><span data-stu-id="bc996-135">https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1.</span></span> <span data-ttu-id="bc996-136">Lásd: [telepítése és használata R HDInsight-fürtök][hdinsight-install-r].</span><span class="sxs-lookup"><span data-stu-id="bc996-136">See [Install and use R on HDInsight clusters][hdinsight-install-r].</span></span> |
| <span data-ttu-id="bc996-137">**Solr telepítése**</span><span class="sxs-lookup"><span data-stu-id="bc996-137">**Install Solr**</span></span> |<span data-ttu-id="bc996-138">https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="bc996-138">https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1.</span></span> <span data-ttu-id="bc996-139">Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-139">See [Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span> |
| <span data-ttu-id="bc996-140">- **Giraph telepítése**</span><span class="sxs-lookup"><span data-stu-id="bc996-140">- **Install Giraph**</span></span> |<span data-ttu-id="bc996-141">https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="bc996-141">https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1.</span></span> <span data-ttu-id="bc996-142">Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-142">See [Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span> |
| <span data-ttu-id="bc996-143">**Hive-könyvtárakhoz előzetes betöltése**</span><span class="sxs-lookup"><span data-stu-id="bc996-143">**Pre-load Hive libraries**</span></span> |<span data-ttu-id="bc996-144">https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1.</span><span class="sxs-lookup"><span data-stu-id="bc996-144">https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1.</span></span> <span data-ttu-id="bc996-145">Lásd: [kódtárak hozzáadása Hive HDInsight-fürtök](hdinsight-hadoop-add-hive-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="bc996-145">See [Add Hive libraries on HDInsight clusters](hdinsight-hadoop-add-hive-libraries.md)</span></span> |

## <a name="call-scripts-using-the-azure-portal"></a><span data-ttu-id="bc996-146">Az Azure portál használatával hívás parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="bc996-146">Call scripts using the Azure portal</span></span>
<span data-ttu-id="bc996-147">**Az Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="bc996-147">**From the Azure portal**</span></span>

1. <span data-ttu-id="bc996-148">Indítsa el a fürt létrehozása a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-148">Start creating a cluster as described at [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
2. <span data-ttu-id="bc996-149">Választható konfiguráció alatt az a **Parancsfájlműveletek** panelen kattintson **parancsfájlművelet hozzáadása** adni a parancsfájlművelet alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="bc996-149">Under Optional Configuration, for the **Script Actions** blade, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="bc996-150">![Parancsfájlművelet használni ahhoz, hogy a fürt](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "parancsfájlművelet használja a fürt testreszabása")</span><span class="sxs-lookup"><span data-stu-id="bc996-150">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="bc996-151">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bc996-151">Property</span></span></th><th><span data-ttu-id="bc996-152">Érték</span><span class="sxs-lookup"><span data-stu-id="bc996-152">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="bc996-153">Név</span><span class="sxs-lookup"><span data-stu-id="bc996-153">Name</span></span></td>
            <td><span data-ttu-id="bc996-154">Adja meg a parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="bc996-154">Specify a name for the script action.</span></span></td></tr>
        <tr><td><span data-ttu-id="bc996-155">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="bc996-155">Script URI</span></span></td>
            <td><span data-ttu-id="bc996-156">Adja meg az URI-t a parancsfájlt, amelyet a fürt testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="bc996-156">Specify the URI to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="bc996-157">s</span><span class="sxs-lookup"><span data-stu-id="bc996-157">s</span></span></td></tr>
        <tr><td><span data-ttu-id="bc996-158">HEAD/munkavégző</span><span class="sxs-lookup"><span data-stu-id="bc996-158">Head/Worker</span></span></td>
            <td><span data-ttu-id="bc996-159">Adja meg a csomópontok (**Head** vagy **munkavégző**) a testreszabási parancsfájl futtatása a.</b>.</span><span class="sxs-lookup"><span data-stu-id="bc996-159">Specify the nodes (**Head** or **Worker**) on which the customization script is run.</b>.</span></span>
        <tr><td><span data-ttu-id="bc996-160">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bc996-160">Parameters</span></span></td>
            <td><span data-ttu-id="bc996-161">Adja meg a paraméterek, ha a parancsfájl által igényelt.</span><span class="sxs-lookup"><span data-stu-id="bc996-161">Specify the parameters, if required by the script.</span></span></td></tr>
    </table>

    <span data-ttu-id="bc996-162">Nyomja le az ENTER billentyűt több összetevőinek telepítése a fürt több parancsfájlművelet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="bc996-162">Press ENTER to add more than one script action to install multiple components on the cluster.</span></span>
3. <span data-ttu-id="bc996-163">Kattintson a **válasszon** a parancsfájl művelet konfiguráció mentéséhez, majd folytassa a fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bc996-163">Click **Select** to save the script action configuration and continue with cluster creation.</span></span>

## <a name="call-scripts-using-azure-powershell"></a><span data-ttu-id="bc996-164">Hívja meg az Azure PowerShell parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="bc996-164">Call scripts using Azure PowerShell</span></span>
<span data-ttu-id="bc996-165">A következő PowerShell-parancsfájl bemutatja, hogyan kell külső telepítse a Windows-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="bc996-165">This following PowerShell script demonstrates how to install Spark on Windows based HDInsight cluster.</span></span>  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
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

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
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


<span data-ttu-id="bc996-166">Más szoftverek telepítése, akkor cserélje le a parancsfájlt a parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="bc996-166">To install other software, you will need to replace the script file in the script:</span></span>

<span data-ttu-id="bc996-167">Amikor a rendszer kéri, adja meg a hitelesítő adatok a fürt.</span><span class="sxs-lookup"><span data-stu-id="bc996-167">When prompted, enter the credentials for the cluster.</span></span> <span data-ttu-id="bc996-168">A fürt létrehozása előtt több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="bc996-168">It can take several minutes before the cluster is created.</span></span>

## <a name="call-scripts-using-net-sdk"></a><span data-ttu-id="bc996-169">Hívás parancsfájlok .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="bc996-169">Call scripts using .NET SDK</span></span>
<span data-ttu-id="bc996-170">A következő példa bemutatja, hogyan Spark telepítse a Windows-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="bc996-170">The following sample demonstrates how to install Spark on Windows based HDInsight cluster.</span></span> <span data-ttu-id="bc996-171">Más szoftver telepítéséhez, akkor cserélje le a parancsfájlt a kódban.</span><span class="sxs-lookup"><span data-stu-id="bc996-171">To install other software, you will need to replace the script file in the code.</span></span>

<span data-ttu-id="bc996-172">**A Spark HDInsight-fürtök létrehozása**</span><span class="sxs-lookup"><span data-stu-id="bc996-172">**To create an HDInsight cluster with Spark**</span></span>

1. <span data-ttu-id="bc996-173">Hozzon létre egy C# konzolalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bc996-173">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="bc996-174">A Nuget-Csomagkezelő konzolról a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="bc996-174">From the Nuget Package Manager Console, run the following command.</span></span>

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. <span data-ttu-id="bc996-175">Használja a következő using utasításokat a Program.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="bc996-175">Use the following using statements in the Program.cs file:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. <span data-ttu-id="bc996-176">Helyezze a kódot az osztály a következő:</span><span class="sxs-lookup"><span data-stu-id="bc996-176">Place the code in the class with the following:</span></span>

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
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
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
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
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. <span data-ttu-id="bc996-177">Az alkalmazás futtatásához nyomja le az **F5** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bc996-177">Press **F5** to run the application.</span></span>

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a><span data-ttu-id="bc996-178">A HDInsight-fürtökön használt nyílt forráskódú szoftvereket támogatása</span><span class="sxs-lookup"><span data-stu-id="bc996-178">Support for open-source software used on HDInsight clusters</span></span>
<span data-ttu-id="bc996-179">A Microsoft Azure HDInsight-szolgáltatás rugalmas platform, amely lehetővé teszi a felhőalapú big data-alkalmazások összeállítását az ökoszisztéma formátumú-e körül Hadoop nyílt forráskódú technológiák használatával.</span><span class="sxs-lookup"><span data-stu-id="bc996-179">The Microsoft Azure HDInsight service is a flexible platform that enables you to build big-data applications in the cloud by using an ecosystem of open-source technologies formed around Hadoop.</span></span> <span data-ttu-id="bc996-180">A Microsoft Azure biztosít, általános támogatási nyílt forráskódú technológiák leírtaknak megfelelően a **támogatja a hatókör** szakasza a <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure támogatási gyakori Kérdéseinek webhelyre</a>.</span><span class="sxs-lookup"><span data-stu-id="bc996-180">Microsoft Azure provides a general level of support for open-source technologies, as discussed in the **Support Scope** section of the <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure Support FAQ website</a>.</span></span> <span data-ttu-id="bc996-181">A HDInsight-szolgáltatás egy további szintű egyes összetevői, támogatást biztosít, az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="bc996-181">The HDInsight service provides an additional level of support for some of the components, as described below.</span></span>

<span data-ttu-id="bc996-182">A HDInsight szolgáltatásban elérhető nyílt forráskódú összetevőinek két típusa van:</span><span class="sxs-lookup"><span data-stu-id="bc996-182">There are two types of open-source components that are available in the HDInsight service:</span></span>

* <span data-ttu-id="bc996-183">**A beépített összetevők** -ezeket az összetevőket a HDInsight-fürtök előre telepített, és adja meg a fürt alapvető funkcióit.</span><span class="sxs-lookup"><span data-stu-id="bc996-183">**Built-in components** - These components are pre-installed on HDInsight clusters and provide core functionality of the cluster.</span></span> <span data-ttu-id="bc996-184">Például YARN erőforrás-kezelő, a Hive lekérdezési nyelv (HiveQL) és a Mahout könyvtárban a kategóriába tartoznak.</span><span class="sxs-lookup"><span data-stu-id="bc996-184">For example, YARN ResourceManager, the Hive query language (HiveQL), and the Mahout library belong to this category.</span></span> <span data-ttu-id="bc996-185">A kiszolgálófürt-összetevők teljes listáját a érhető [What's new in HDInsight által biztosított Hadoop-fürt verziók?](hdinsight-component-versioning.md) </a>.</span><span class="sxs-lookup"><span data-stu-id="bc996-185">A full list of cluster components is available in [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md)</a>.</span></span>
* <span data-ttu-id="bc996-186">**Egyéni összetevők** -Ön, mint a felhasználó a fürt telepítése vagy használata előtt a munkaterhelésnek valamelyik összetevő a közösségi érhető el, vagy Ön által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="bc996-186">**Custom components** - You, as a user of the cluster, can install or use in your workload any component available in the community or created by you.</span></span>

<span data-ttu-id="bc996-187">A beépített összetevők teljes mértékben támogatottak, és a Microsoft Support fog help elkülönítésére, és ezeket az összetevőket kapcsolatos problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="bc996-187">Built-in components are fully supported, and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>

> [!WARNING]
> <span data-ttu-id="bc996-188">A HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog help elkülönítésére, és ezeket az összetevőket kapcsolatos problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="bc996-188">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="bc996-189">Egyéni összetevők kapnak minden üzleti szempontból ésszerű támogatási segítséget nyújtanak a probléma további hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="bc996-189">Custom components receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="bc996-190">A probléma megoldását, vagy kéri fel, a nyílt forráskódú technológiák, ahol a részletes segítséget, hogy a technológiát található elérhető csatorna végezhetnek eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="bc996-190">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="bc996-191">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="bc996-191">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="bc996-192">Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="bc996-192">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).</span></span>
>
>

<span data-ttu-id="bc996-193">A HDInsight-szolgáltatás többféleképpen is egyéni összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="bc996-193">The HDInsight service provides several ways to use custom components.</span></span> <span data-ttu-id="bc996-194">Függetlenül attól, milyen összetevőt használja vagy a fürt telepíteni az azonos szintű támogatást vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="bc996-194">Regardless of how a component is used or installed on the cluster, the same level of support applies.</span></span> <span data-ttu-id="bc996-195">Alább felsoroljuk, hogy használható-e az egyéni összetevőkben HDInsight-fürtök leggyakoribb módja:</span><span class="sxs-lookup"><span data-stu-id="bc996-195">Below is a list of the most common ways that custom components can be used on HDInsight clusters:</span></span>

1. <span data-ttu-id="bc996-196">Feladat elküldése - Hadoop és egyéb feladatok végrehajtása, vagy használjon egyéni összetevők küldheti el a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="bc996-196">Job submission - Hadoop or other types of jobs that execute or use custom components can be submitted to the cluster.</span></span>
2. <span data-ttu-id="bc996-197">Fürt testreszabási - fürt létrehozása során megadhatja további beállításokat és a fürtcsomópontokon telepített egyéni összetevők.</span><span class="sxs-lookup"><span data-stu-id="bc996-197">Cluster customization - During cluster creation, you can specify additional settings and custom components that will be installed on the cluster nodes.</span></span>
3. <span data-ttu-id="bc996-198">Minták – a népszerű egyéni összetevők, a Microsoft és a mások által biztosított mintákat ezeket az összetevőket hogyan használható a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="bc996-198">Samples - For popular custom components, Microsoft and others may provide samples of how these components can be used on the HDInsight clusters.</span></span> <span data-ttu-id="bc996-199">Ezeket a mintákat támogatás nélkül van megadva.</span><span class="sxs-lookup"><span data-stu-id="bc996-199">These samples are provided without support.</span></span>

## <a name="develop-script-action-scripts"></a><span data-ttu-id="bc996-200">Parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="bc996-200">Develop Script Action scripts</span></span>
<span data-ttu-id="bc996-201">Lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].</span><span class="sxs-lookup"><span data-stu-id="bc996-201">See [Develop Script Action scripts for HDInsight][hdinsight-write-script].</span></span>

## <a name="see-also"></a><span data-ttu-id="bc996-202">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="bc996-202">See also</span></span>
* <span data-ttu-id="bc996-203">[Hdinsight Hadoop-fürtök létrehozása] [ hdinsight-provision-cluster] HDInsight-fürtök létrehozása más egyéni beállítások használatával kapcsolatos utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bc996-203">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how to create an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="bc996-204">[A HDInsight parancsfájlművelet-parancsfájlok fejlesztése][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="bc996-204">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="bc996-205">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="bc996-205">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="bc996-206">[Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="bc996-206">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="bc996-207">[Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-207">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="bc996-208">[Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="bc996-208">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


<span data-ttu-id="bc996-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"</span><span class="sxs-lookup"><span data-stu-id="bc996-209">[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"</span></span>
