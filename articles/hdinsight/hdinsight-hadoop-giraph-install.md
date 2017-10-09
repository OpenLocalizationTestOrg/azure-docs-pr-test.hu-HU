---
title: "aaaInstall és -felhasználási Giraph a Hadoop fürtök a HDInsight - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan Giraph toocustomize HDInsight-fürtöt, és hogyan toouse Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="90a96-103">Telepítheti és használhatja a Windows-alapú HDInsight-fürtök Giraph</span><span class="sxs-lookup"><span data-stu-id="90a96-103">Install and use Giraph on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="90a96-104">Megtudhatja, hogyan toocustomize Windows alapú HDInsight-fürt a parancsfájl műveletével Giraph, és hogyan toouse Giraph tooprocess nagyméretű diagramjait.</span><span class="sxs-lookup"><span data-stu-id="90a96-104">Learn how toocustomize Windows based HDInsight cluster with Giraph using Script Action, and how toouse Giraph tooprocess large-scale graphs.</span></span> <span data-ttu-id="90a96-105">Egy Linux-alapú fürttel Giraph használatáról információkért lásd: [Giraph telepítése a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-105">For information on using Giraph with a Linux-based cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90a96-106">Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="90a96-106">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="90a96-107">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="90a96-107">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="90a96-108">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="90a96-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="90a96-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="90a96-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="90a96-110">Kapcsolatos információk a Linux-alapú HDInsight-fürt Giraph tooinstall lásd [Giraph telepítése a HDInsight Hadoop-fürtök (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-110">For information on how tooinstall Giraph on a Linux-based HDInsight cluster, see [Install Giraph on HDInsight Hadoop clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).</span></span>


<span data-ttu-id="90a96-111">Telepíthető Giraph bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*.</span><span class="sxs-lookup"><span data-stu-id="90a96-111">You can install Giraph on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="90a96-112">Egy minta parancsfájlt tooinstall Giraph egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="90a96-112">A sample script tooinstall Giraph on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span> <span data-ttu-id="90a96-113">hello mintaparancsfájl csak HDInsight-fürt verziószáma 3.1-es verziójával működik.</span><span class="sxs-lookup"><span data-stu-id="90a96-113">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="90a96-114">A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-114">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="90a96-115">**Kapcsolódó cikkek**</span><span class="sxs-lookup"><span data-stu-id="90a96-115">**Related articles**</span></span>

* [<span data-ttu-id="90a96-116">Giraph telepítse a HDInsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="90a96-116">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="90a96-117">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90a96-117">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="90a96-118">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="90a96-118">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="90a96-119">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-119">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-giraph"></a><span data-ttu-id="90a96-120">Mi az a Giraph?</span><span class="sxs-lookup"><span data-stu-id="90a96-120">What is Giraph?</span></span>
<span data-ttu-id="90a96-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="90a96-121"><a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="90a96-122">Diagramokat modellhez tartozó objektumok, például hello kapcsolatainak hello Internet, például a nagy hálózaton útválasztók közötti kapcsolatokat, vagy a közösségi hálózatokon (néha hivatkozott tooas közösségi grafikon) személyek közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="90a96-122">Graphs model relationships between objects, such as hello connections between routers on a large network like hello Internet, or relationships between people on social networks (sometimes referred tooas a social graph).</span></span> <span data-ttu-id="90a96-123">Graph feldolgozás lehetővé teszi a tooreason kapcsolatos hello kapcsolatai egy grafikonon objektumok, mint:</span><span class="sxs-lookup"><span data-stu-id="90a96-123">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="90a96-124">A jelenlegi kapcsolatok alapján lehetséges ismerősök azonosítása.</span><span class="sxs-lookup"><span data-stu-id="90a96-124">Identifying potential friends based on your current relationships.</span></span>
* <span data-ttu-id="90a96-125">Azonosító hello legrövidebb útvonal hálózatban két számítógép között.</span><span class="sxs-lookup"><span data-stu-id="90a96-125">Identifying hello shortest route between two computers in a network.</span></span>
* <span data-ttu-id="90a96-126">A weblapok hello lap indulva számított rangját kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="90a96-126">Calculating hello page rank of webpages.</span></span>

## <a name="install-giraph-using-portal"></a><span data-ttu-id="90a96-127">Telepítse a portál használatával Giraph</span><span class="sxs-lookup"><span data-stu-id="90a96-127">Install Giraph using portal</span></span>
1. <span data-ttu-id="90a96-128">Indítsa el a fürt létrehozása hello segítségével **egyéni létrehozás** beállítást, a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-128">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="90a96-129">A hello **Parancsfájlműveletek** lap hello varázsló, kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="90a96-129">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="90a96-130">![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "használata parancsfájlművelet toocustomize fürt")</span><span class="sxs-lookup"><span data-stu-id="90a96-130">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="90a96-131">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="90a96-131">Property</span></span></th><th><span data-ttu-id="90a96-132">Érték</span><span class="sxs-lookup"><span data-stu-id="90a96-132">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="90a96-133">Név</span><span class="sxs-lookup"><span data-stu-id="90a96-133">Name</span></span></td>
            <td><span data-ttu-id="90a96-134">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="90a96-134">Specify a name for hello script action.</span></span> <span data-ttu-id="90a96-135">Például <b>telepítése Giraph</b>.</span><span class="sxs-lookup"><span data-stu-id="90a96-135">For example, <b>Install Giraph</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="90a96-136">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="90a96-136">Script URI</span></span></td>
            <td><span data-ttu-id="90a96-137">Hello egységes erőforrás-azonosító (URI) toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="90a96-137">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="90a96-138">Például <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="90a96-138">For example, <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="90a96-139">Csomóponttípus</span><span class="sxs-lookup"><span data-stu-id="90a96-139">Node Type</span></span></td>
            <td><span data-ttu-id="90a96-140">Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="90a96-140">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="90a96-141">Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>csak a feldolgozó csomópontok</b>.</span><span class="sxs-lookup"><span data-stu-id="90a96-141">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="90a96-142">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="90a96-142">Parameters</span></span></td>
            <td><span data-ttu-id="90a96-143">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="90a96-143">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="90a96-144">hello parancsfájl tooinstall Giraph kell paramétert, így ez üresen hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="90a96-144">hello script tooinstall Giraph does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="90a96-145">Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="90a96-145">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="90a96-146">Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="90a96-146">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-giraph"></a><span data-ttu-id="90a96-147">A Giraph használata</span><span class="sxs-lookup"><span data-stu-id="90a96-147">Use Giraph</span></span>
<span data-ttu-id="90a96-148">Hello SimpleShortestPathsComputation példa toodemonstrate hello basic használjuk <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> megvalósítása a hello legrövidebb elérési út egy grafikonon objektumok közötti kereséshez.</span><span class="sxs-lookup"><span data-stu-id="90a96-148">We use hello SimpleShortestPathsComputation example toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementation for finding hello shortest path between objects in a graph.</span></span> <span data-ttu-id="90a96-149">A következő lépéseket tooupload hello minta adatok és hello minta jar, egy feladat futtatása hello SimpleShortestPathsComputation példa, majd az eredmények megtekintése hello segítségével hello használata.</span><span class="sxs-lookup"><span data-stu-id="90a96-149">Use hello following steps tooupload hello sample data and hello sample jar, run a job by using hello SimpleShortestPathsComputation example, and then view hello results.</span></span>

1. <span data-ttu-id="90a96-150">Töltsön fel egy minta adatok fájl tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="90a96-150">Upload a sample data file tooAzure Blob storage.</span></span> <span data-ttu-id="90a96-151">A helyi munkaállomáson, hozzon létre egy új fájlt **tiny_graph.txt**.</span><span class="sxs-lookup"><span data-stu-id="90a96-151">On your local workstation, create a new file named **tiny_graph.txt**.</span></span> <span data-ttu-id="90a96-152">A következő sorokat hello kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="90a96-152">It should contain hello following lines:</span></span>

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    <span data-ttu-id="90a96-153">Töltse fel a hello tiny_graph.txt toohello elsődleges-tároló a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="90a96-153">Upload hello tiny_graph.txt file toohello primary storage for your HDInsight cluster.</span></span> <span data-ttu-id="90a96-154">Útmutatást tooupload adatokat, lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-154">For instructions on how tooupload data, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

    <span data-ttu-id="90a96-155">Ezek az adatok egy irányított gráf hello formátum használatával az objektumok közötti kapcsolatot ismerteti [forrás\_azonosító, a forrás\_érték, [[cél\_azonosítója], [peremhálózati\_érték],...]]. Minden egyes sorban közötti kapcsolatot jelent a **forrás\_azonosító** objektum és egy vagy több **cél\_azonosító** objektumok.</span><span class="sxs-lookup"><span data-stu-id="90a96-155">This data describes a relationship between objects in a directed graph, by using hello format [source\_id, source\_value,[[dest\_id], [edge\_value],...]]. Each line represents a relationship between a **source\_id** object and one or more **dest\_id** objects.</span></span> <span data-ttu-id="90a96-156">Hello **peremhálózati\_érték** (vagy súly)-re hello erőssége vagy hello kapcsolat közötti távolság **source_id** és **cél\_azonosító**.</span><span class="sxs-lookup"><span data-stu-id="90a96-156">hello **edge\_value** (or weight) can be thought of as hello strength or distance of hello connection between **source_id** and **dest\_id**.</span></span>

    <span data-ttu-id="90a96-157">Jelenik meg, és objektumok közötti távolság hello hello érték (vagy súly) használ, fenti adatok hello nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="90a96-157">Drawn out, and using hello value (or weight) as hello distance between objects, hello above data might look like this:</span></span>

    ![különböző távolságát sornyi kör Megrajzolás tiny_graph.txt](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. <span data-ttu-id="90a96-159">Hello SimpleShortestPathsComputation példa futtatásához.</span><span class="sxs-lookup"><span data-stu-id="90a96-159">Run hello SimpleShortestPathsComputation example.</span></span> <span data-ttu-id="90a96-160">Hello Azure PowerShell parancsmagok toorun hello példa következő hello tiny_graph.txt fájllal bemenetként használja.</span><span class="sxs-lookup"><span data-stu-id="90a96-160">Use hello following Azure PowerShell cmdlets toorun hello example by using hello tiny_graph.txt file as input.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="90a96-161">A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt.</span><span class="sxs-lookup"><span data-stu-id="90a96-161">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and was removed on January 1, 2017.</span></span> <span data-ttu-id="90a96-162">a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="90a96-162">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="90a96-163">Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója.</span><span class="sxs-lookup"><span data-stu-id="90a96-163">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="90a96-164">Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="90a96-164">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    <span data-ttu-id="90a96-165">A fenti példa hello, cserélje le a **clustername** a HDInsight-fürt, amelyen telepítve Giraph hello nevű.</span><span class="sxs-lookup"><span data-stu-id="90a96-165">In hello above example, replace **clustername** with hello name of your HDInsight cluster that has Giraph installed.</span></span>
3. <span data-ttu-id="90a96-166">Hello eredményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="90a96-166">View hello results.</span></span> <span data-ttu-id="90a96-167">Miután hello feladat befejeződött, hello eredményeket fogja tárolni hello két a kimeneti fájlok **wasb: / / / Példa/out/shotestpaths** mappa.</span><span class="sxs-lookup"><span data-stu-id="90a96-167">Once hello job has finished, hello results will be stored in two output files in hello **wasb:///example/out/shotestpaths** folder.</span></span> <span data-ttu-id="90a96-168">hello fájlok nevezzük **rész-m-00001** és **rész-m-00002**.</span><span class="sxs-lookup"><span data-stu-id="90a96-168">hello files are called **part-m-00001** and **part-m-00002**.</span></span> <span data-ttu-id="90a96-169">Hajtsa végre a következő lépéseket toodownload, és tekintse meg a hello kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="90a96-169">Perform hello following steps toodownload and view hello output:</span></span>

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    <span data-ttu-id="90a96-170">Ezzel létrehoz hello **példa/kimeneti/shortestpaths** könyvtárstruktúrát hello aktuális könyvtárhoz a munkaállomáson, a két hello kimeneti fájlok toothat céljából.</span><span class="sxs-lookup"><span data-stu-id="90a96-170">This will create hello **example/output/shortestpaths** directory structure in hello current directory on your workstation, and download hello two output files toothat location.</span></span>

    <span data-ttu-id="90a96-171">Használjon hello **Cat** parancsmag toodisplay hello tartalmát hello fájlok:</span><span class="sxs-lookup"><span data-stu-id="90a96-171">Use hello **Cat** cmdlet toodisplay hello contents of hello files:</span></span>

        Cat example/output/shortestpaths/part*

    <span data-ttu-id="90a96-172">hello kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="90a96-172">hello output should appear similar toohello following:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="90a96-173">hello SimpleShortestPathComputation példa nél nagyobb toostart objektum azonosítója 1 és hello legrövidebb elérési tooother objektumok keresése.</span><span class="sxs-lookup"><span data-stu-id="90a96-173">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="90a96-174">Így hello kimeneti értelmezhető `destination_id distance`, ahol a távolság hello érték (vagy súly) amennyi hello szegélye között objektum azonosítója 1 és hello cél azonosítót.</span><span class="sxs-lookup"><span data-stu-id="90a96-174">So hello output should be read as `destination_id distance`, where distance is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="90a96-175">Ez megjelenítése, ellenőrizheti hello eredmények hello legrövidebb elérési utak utazás azonosítója 1 és egyéb objektumok között.</span><span class="sxs-lookup"><span data-stu-id="90a96-175">Visualizing this, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="90a96-176">Vegye figyelembe, hogy hello azonosítója 1 és 4 azonosító közötti legrövidebb elérési út pedig az 5.</span><span class="sxs-lookup"><span data-stu-id="90a96-176">Note that hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="90a96-177">Ez a hello teljes közötti távolság szerint <span style="color:orange">azonosítója 1. és 3</span>, majd <span style="color:red">azonosító 3. és 4</span>.</span><span class="sxs-lookup"><span data-stu-id="90a96-177">This is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Az objektumok rajzolási körök mint a legrövidebb elérési utak között](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a><span data-ttu-id="90a96-179">Giraph segítségével a következőkre PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="90a96-179">Install Giraph using Aure PowerShell</span></span>
<span data-ttu-id="90a96-180">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="90a96-180">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="90a96-181">hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="90a96-181">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="90a96-182">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="90a96-182">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="install-giraph-using-net-sdk"></a><span data-ttu-id="90a96-183">Telepítse a .NET SDK használatával Giraph</span><span class="sxs-lookup"><span data-stu-id="90a96-183">Install Giraph using .NET SDK</span></span>
<span data-ttu-id="90a96-184">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="90a96-184">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="90a96-185">hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello.</span><span class="sxs-lookup"><span data-stu-id="90a96-185">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="90a96-186">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="90a96-186">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="90a96-187">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="90a96-187">See also</span></span>
* [<span data-ttu-id="90a96-188">Giraph telepítse a HDInsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="90a96-188">Install Giraph on HDInsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-giraph-install-linux.md)
* <span data-ttu-id="90a96-189">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90a96-189">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="90a96-190">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="90a96-190">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="90a96-191">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="90a96-191">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="90a96-192">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-mintát.</span><span class="sxs-lookup"><span data-stu-id="90a96-192">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="90a96-193">[A HDInsight-fürtökön R telepítéséhez][hdinsight-install-r]: parancsfájlművelet minta R. telepítéséről</span><span class="sxs-lookup"><span data-stu-id="90a96-193">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="90a96-194">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install.md): parancsfájlművelet minta Solr telepítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="90a96-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md): Script Action sample about installing Solr.</span></span>

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
