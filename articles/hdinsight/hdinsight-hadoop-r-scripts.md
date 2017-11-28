---
title: "R használata a HDInsight-fürtök - Azure testreszabása |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse az R parancsfájl művelet segítségével, és a HDInsight-fürtök R használhatja."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 5b9b793d49217acd9f0c6c518596a7afb5600d69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="72fff-103">Az R környezet telepítése és használata HDInsight-beli Hadoop-fürtökön</span><span class="sxs-lookup"><span data-stu-id="72fff-103">Install and use R on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="72fff-104">Ismerje meg, hogyan szabhatja testre a Windows alapú HDInsight-fürt az R parancsfájl műveletével, és R használata a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="72fff-104">Learn how to customize Windows based HDInsight cluster with R using Script Action, and how to use R on HDInsight clusters.</span></span> <span data-ttu-id="72fff-105">A [HDInsight ajánlat](https://azure.microsoft.com/pricing/details/hdinsight/) tartalmaz R Server a HDInsight-fürt részeként.</span><span class="sxs-lookup"><span data-stu-id="72fff-105">The [HDInsight offering](https://azure.microsoft.com/pricing/details/hdinsight/) includes R Server as part of your HDInsight cluster.</span></span> <span data-ttu-id="72fff-106">Ez lehetővé teszi az R parancsfájlok használata a MapReduce és Spark elosztott számítások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="72fff-106">This allows R scripts to use MapReduce and Spark to run distributed computations.</span></span> <span data-ttu-id="72fff-107">További információk: [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (R Server on HDInsight – első lépések).</span><span class="sxs-lookup"><span data-stu-id="72fff-107">For more information, see [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span> <span data-ttu-id="72fff-108">Az R használatával egy Linux-alapú fürttel információkért lásd: [telepítése és R használata a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span><span class="sxs-lookup"><span data-stu-id="72fff-108">For information on using R with a Linux-based cluster, see [Install and use R on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span></span>

<span data-ttu-id="72fff-109">Telepíthető R bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*.</span><span class="sxs-lookup"><span data-stu-id="72fff-109">You can install R on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="72fff-110">Egy minta parancsfájlt a HDInsight-fürtök R telepítéséhez érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="72fff-110">A sample script to install R on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

<span data-ttu-id="72fff-111">**Kapcsolódó cikkek**</span><span class="sxs-lookup"><span data-stu-id="72fff-111">**Related articles**</span></span>

* [<span data-ttu-id="72fff-112">Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="72fff-112">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="72fff-113">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="72fff-113">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="72fff-114">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="72fff-114">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="72fff-115">A HDInsight parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="72fff-115">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a><span data-ttu-id="72fff-116">Mi az az R?</span><span class="sxs-lookup"><span data-stu-id="72fff-116">What is R?</span></span>
<span data-ttu-id="72fff-117">A <a href="http://www.r-project.org/" target="_blank">R-projektjét, amely statisztikai számításokhoz</a> egy megnyitott adatforrás nyelvi és statisztikai számításokhoz környezet.</span><span class="sxs-lookup"><span data-stu-id="72fff-117">The <a href="http://www.r-project.org/" target="_blank">R Project for Statistical Computing</a> is an open source language and environment for statistical computing.</span></span> <span data-ttu-id="72fff-118">R biztosít több száz beépített statisztikai függvények és a saját programozási nyelv, amely egyesíti a működési és objektumorientált programozási aspektusait.</span><span class="sxs-lookup"><span data-stu-id="72fff-118">R provides hundreds of build-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming.</span></span> <span data-ttu-id="72fff-119">Nagy mennyiségű grafikus képességeket is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="72fff-119">It also provides extensive graphical capabilities.</span></span> <span data-ttu-id="72fff-120">R a legtöbb szakmai statisztikusok és a mezők számos különböző kutatók az előnyben részesített programozási környezetet.</span><span class="sxs-lookup"><span data-stu-id="72fff-120">R is the preferred programming environment for most professional statisticians and scientists in a wide variety of fields.</span></span>

<span data-ttu-id="72fff-121">R rendszer kompatibilis Azure Blob Storage (WASB), így az ott tárolt adatok R használata a HDInsight dolgozhatók.</span><span class="sxs-lookup"><span data-stu-id="72fff-121">R is compatible with Azure Blob Storage (WASB) so that data that is stored there can be processed using R on HDInsight.</span></span>  

## <a name="install-r"></a><span data-ttu-id="72fff-122">R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="72fff-122">Install R</span></span>
<span data-ttu-id="72fff-123">A [mintaparancsfájl](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R telepítéséhez egy hdinsight fürt érhető el az egy csak olvasható az Azure Storage-blobból.</span><span class="sxs-lookup"><span data-stu-id="72fff-123">A [sample script](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) to install R on an HDInsight cluster is available from a read-only blob in Azure Storage.</span></span> <span data-ttu-id="72fff-124">Ez a szakasz leírja a minta-parancsprogram használatáról az Azure portál használata a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="72fff-124">This section provides instructions about how to use the sample script while creating the cluster using the Azure Portal.</span></span>

> [!NOTE]
> <span data-ttu-id="72fff-125">A parancsfájlpéldát a HDInsight-fürt verziószáma 3.1-ban jelent.</span><span class="sxs-lookup"><span data-stu-id="72fff-125">The sample script was introduced with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="72fff-126">A HDInsight fürt verzióival kapcsolatos további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="72fff-126">For more information about  HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

1. <span data-ttu-id="72fff-127">Amikor HDInsight fürtöt hoz létre a portálról, kattintson **opcionális konfigurációs**, és kattintson a **Parancsfájlműveletek**.</span><span class="sxs-lookup"><span data-stu-id="72fff-127">When you create an HDInsight cluster from the Portal, click **Optional Configuration**, and then click **Script Actions**.</span></span>
2. <span data-ttu-id="72fff-128">Az a **Parancsfájlműveletek** lapján adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="72fff-128">On the **Script Actions** page, enter the following values:</span></span>

    <span data-ttu-id="72fff-129">![Parancsfájlművelet használni ahhoz, hogy a fürt](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "parancsfájlművelet használja a fürt testreszabása")</span><span class="sxs-lookup"><span data-stu-id="72fff-129">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="72fff-130">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="72fff-130">Property</span></span></th><th><span data-ttu-id="72fff-131">Érték</span><span class="sxs-lookup"><span data-stu-id="72fff-131">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="72fff-132">Név</span><span class="sxs-lookup"><span data-stu-id="72fff-132">Name</span></span></td>
            <td><span data-ttu-id="72fff-133">Adja meg a parancsfájlművelet nevét, például <b>R telepítéséhez</b>.</span><span class="sxs-lookup"><span data-stu-id="72fff-133">Specify a name for the script action, for example, <b>Install R</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="72fff-134">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="72fff-134">Script URI</span></span></td>
            <td><span data-ttu-id="72fff-135">Adja meg az URI-t a parancsfájlt, amelyet a fürt, például testreszabásához <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span><span class="sxs-lookup"><span data-stu-id="72fff-135">Specify the URI to the script that is invoked to customize the cluster, for example, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="72fff-136">Csomóponttípus</span><span class="sxs-lookup"><span data-stu-id="72fff-136">Node Type</span></span></td>
            <td><span data-ttu-id="72fff-137">Adja meg a csomópontok, amelyen fut a testreszabási parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="72fff-137">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="72fff-138">Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>munkavégző csomópontokhoz</b> csak.</span><span class="sxs-lookup"><span data-stu-id="72fff-138">You can choose <b>All Nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes</b> only.</span></span>
        <tr><td><span data-ttu-id="72fff-139">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="72fff-139">Parameters</span></span></td>
            <td><span data-ttu-id="72fff-140">Adja meg a paraméterek, ha a parancsfájl által igényelt.</span><span class="sxs-lookup"><span data-stu-id="72fff-140">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="72fff-141">R telepítéséhez a parancsfájl azonban nem szükséges paramétereket, ezért üresen hagyhatja, ez.</span><span class="sxs-lookup"><span data-stu-id="72fff-141">However, the script to install R does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="72fff-142">Több összetevőinek telepítése a fürt több parancsfájlművelet adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="72fff-142">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="72fff-143">Miután hozzáadta a parancsfájlok, kattintson a pipa jelre a fürt crating elindításához.</span><span class="sxs-lookup"><span data-stu-id="72fff-143">After you have added the scripts, click the check mark to start crating the cluster.</span></span>

<span data-ttu-id="72fff-144">A parancsfájl segítségével R telepítése a HDInsight az Azure PowerShell vagy a HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="72fff-144">You can also use the script to install R on HDInsight by using Azure PowerShell or the HDInsight .NET SDK.</span></span> <span data-ttu-id="72fff-145">Ezek az eljárások az utasításokat a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="72fff-145">Instructions for these procedures are provided later in this article.</span></span>

## <a name="run-r-scripts"></a><span data-ttu-id="72fff-146">R parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="72fff-146">Run R scripts</span></span>
<span data-ttu-id="72fff-147">Ez a szakasz ismerteti a Hadoop-fürt hdinsightban az R-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="72fff-147">This section describes how to run an R script on the Hadoop cluster with HDInsight.</span></span>

1. <span data-ttu-id="72fff-148">**Távoli asztali kapcsolat létrehozása a fürt**: a portál a, a távoli asztal engedélyezése a fürthöz létrehozott r telepítve, és csatlakozzon a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="72fff-148">**Establish a Remote Desktop connection to the cluster**: From the Portal, enable Remote Desktop for the cluster you created with R installed, and then connect to the cluster.</span></span> <span data-ttu-id="72fff-149">Útmutatásért lásd: [csatlakozás RDP Funkciót használnak a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="72fff-149">For instructions, see [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="72fff-150">**Nyissa meg az R konzolt**: az R-telepítés helyezi egy hivatkozást az R-konzolra átjárócsomópontjához asztalán.</span><span class="sxs-lookup"><span data-stu-id="72fff-150">**Open the R console**: The R installation puts a link to the R console on the desktop of the head node.</span></span> <span data-ttu-id="72fff-151">Kattintson rá az R-konzol megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="72fff-151">Click on it to open the R console.</span></span>
3. <span data-ttu-id="72fff-152">**Az R-parancsfájl futtatása**: az R parancsfájlt közvetlenül az R-konzolról illeszti, ha kiválasztja, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="72fff-152">**Run the R script**: The R script can be run directly from the R console by pasting it, selecting it, and pressing ENTER.</span></span> <span data-ttu-id="72fff-153">Ez egy egyszerű példa parancsfájlt, amely állít elő, 1 és 100 közötti számokat, majd 2 szorzatát.</span><span class="sxs-lookup"><span data-stu-id="72fff-153">Here is a simple example script that generates the numbers 1 to 100 and then multiplies them by 2.</span></span>

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

<span data-ttu-id="72fff-154">Az első két sorok hívható meg a telepített R. RHadoop könyvtárak A végső sor kiírja az eredmények a konzol.</span><span class="sxs-lookup"><span data-stu-id="72fff-154">The first two lines call the RHadoop libraries that are installed with R. The final line prints the results to the console.</span></span> <span data-ttu-id="72fff-155">A kimeneti kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="72fff-155">The output should look like this:</span></span>

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a><span data-ttu-id="72fff-156">Segítségével a következőkre PowerShell R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="72fff-156">Install R using Aure PowerShell</span></span>
<span data-ttu-id="72fff-157">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="72fff-157">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="72fff-158">A Spark az Azure PowerShell telepítése mutatja be.</span><span class="sxs-lookup"><span data-stu-id="72fff-158">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="72fff-159">Meg kell adnia, hogy a használandó parancsfájlt [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="72fff-159">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

## <a name="install-r-using-net-sdk"></a><span data-ttu-id="72fff-160">.NET SDK használatával R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="72fff-160">Install R using .NET SDK</span></span>
<span data-ttu-id="72fff-161">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="72fff-161">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="72fff-162">A minta bemutatja, hogyan telepítse a .NET SDK használatával Spark.</span><span class="sxs-lookup"><span data-stu-id="72fff-162">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="72fff-163">Meg kell adnia, hogy a használandó parancsfájlt [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span><span class="sxs-lookup"><span data-stu-id="72fff-163">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span></span>

## <a name="see-also"></a><span data-ttu-id="72fff-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="72fff-164">See also</span></span>
* [<span data-ttu-id="72fff-165">Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="72fff-165">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="72fff-166">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="72fff-166">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="72fff-167">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="72fff-167">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="72fff-168">A HDInsight parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="72fff-168">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)
* <span data-ttu-id="72fff-169">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-minta</span><span class="sxs-lookup"><span data-stu-id="72fff-169">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark</span></span>
* <span data-ttu-id="72fff-170">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítéséről</span><span class="sxs-lookup"><span data-stu-id="72fff-170">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph</span></span>
* <span data-ttu-id="72fff-171">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md): parancsfájlművelet minta Solr telepítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="72fff-171">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): Script Action sample about installing Solr.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
