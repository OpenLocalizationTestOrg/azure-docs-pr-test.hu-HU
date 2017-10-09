---
title: "R aaaUse toocustomize HDInsight - fürtök Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall R használata parancsfájl-művelet és a HDInsight-fürtök R történő használatra."
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
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="2c600-103">Az R környezet telepítése és használata HDInsight-beli Hadoop-fürtökön</span><span class="sxs-lookup"><span data-stu-id="2c600-103">Install and use R on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="2c600-104">Ismerje meg, hogyan toocustomize Windows-alapú HDInsight-fürt az R parancsfájl műveletével, és hogyan toouse R a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="2c600-104">Learn how toocustomize Windows based HDInsight cluster with R using Script Action, and how toouse R on HDInsight clusters.</span></span> <span data-ttu-id="2c600-105">Hello [HDInsight ajánlat](https://azure.microsoft.com/pricing/details/hdinsight/) tartalmaz R Server a HDInsight-fürt részeként.</span><span class="sxs-lookup"><span data-stu-id="2c600-105">hello [HDInsight offering](https://azure.microsoft.com/pricing/details/hdinsight/) includes R Server as part of your HDInsight cluster.</span></span> <span data-ttu-id="2c600-106">Ez lehetővé teszi a R parancsfájlok toouse MapReduce és Spark toorun elosztott számítások.</span><span class="sxs-lookup"><span data-stu-id="2c600-106">This allows R scripts toouse MapReduce and Spark toorun distributed computations.</span></span> <span data-ttu-id="2c600-107">További információk: [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (R Server on HDInsight – első lépések).</span><span class="sxs-lookup"><span data-stu-id="2c600-107">For more information, see [Get started using R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md).</span></span> <span data-ttu-id="2c600-108">Az R használatával egy Linux-alapú fürttel információkért lásd: [telepítése és R használata a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2c600-108">For information on using R with a Linux-based cluster, see [Install and use R on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).</span></span>

<span data-ttu-id="2c600-109">Telepíthető R bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*.</span><span class="sxs-lookup"><span data-stu-id="2c600-109">You can install R on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="2c600-110">Egy minta parancsfájlt tooinstall R egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="2c600-110">A sample script tooinstall R on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

<span data-ttu-id="2c600-111">**Kapcsolódó cikkek**</span><span class="sxs-lookup"><span data-stu-id="2c600-111">**Related articles**</span></span>

* [<span data-ttu-id="2c600-112">Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="2c600-112">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="2c600-113">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c600-113">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="2c600-114">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="2c600-114">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="2c600-115">A HDInsight parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="2c600-115">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a><span data-ttu-id="2c600-116">Mi az az R?</span><span class="sxs-lookup"><span data-stu-id="2c600-116">What is R?</span></span>
<span data-ttu-id="2c600-117">Hello <a href="http://www.r-project.org/" target="_blank">R-projektjét, amely statisztikai számításokhoz</a> egy megnyitott adatforrás nyelvi és statisztikai számításokhoz környezet.</span><span class="sxs-lookup"><span data-stu-id="2c600-117">hello <a href="http://www.r-project.org/" target="_blank">R Project for Statistical Computing</a> is an open source language and environment for statistical computing.</span></span> <span data-ttu-id="2c600-118">R biztosít több száz beépített statisztikai függvények és a saját programozási nyelv, amely egyesíti a működési és objektumorientált programozási aspektusait.</span><span class="sxs-lookup"><span data-stu-id="2c600-118">R provides hundreds of build-in statistical functions and its own programming language that combines aspects of functional and object-oriented programming.</span></span> <span data-ttu-id="2c600-119">Nagy mennyiségű grafikus képességeket is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2c600-119">It also provides extensive graphical capabilities.</span></span> <span data-ttu-id="2c600-120">R hello előnyben részesített programozási környezetet legtöbb szakmai statisztikusok és a mezők számos különböző kutatók.</span><span class="sxs-lookup"><span data-stu-id="2c600-120">R is hello preferred programming environment for most professional statisticians and scientists in a wide variety of fields.</span></span>

<span data-ttu-id="2c600-121">R rendszer kompatibilis Azure Blob Storage (WASB), így az ott tárolt adatok R használata a HDInsight dolgozhatók.</span><span class="sxs-lookup"><span data-stu-id="2c600-121">R is compatible with Azure Blob Storage (WASB) so that data that is stored there can be processed using R on HDInsight.</span></span>  

## <a name="install-r"></a><span data-ttu-id="2c600-122">R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="2c600-122">Install R</span></span>
<span data-ttu-id="2c600-123">A [mintaparancsfájl](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R a HDInsight-fürtök az egy csak olvasható az Azure Storage-blobból érhető el.</span><span class="sxs-lookup"><span data-stu-id="2c600-123">A [sample script](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R on an HDInsight cluster is available from a read-only blob in Azure Storage.</span></span> <span data-ttu-id="2c600-124">Ez a szakasz bemutatja, hogyan toouse hello hello Azure portál használatával hello fürt létrehozása során minta parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="2c600-124">This section provides instructions about how toouse hello sample script while creating hello cluster using hello Azure Portal.</span></span>

> [!NOTE]
> <span data-ttu-id="2c600-125">HDInsight-fürt verziószáma 3.1 hello mintaparancsfájl jelent.</span><span class="sxs-lookup"><span data-stu-id="2c600-125">hello sample script was introduced with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="2c600-126">A HDInsight fürt verzióival kapcsolatos további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="2c600-126">For more information about  HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>
>
>

1. <span data-ttu-id="2c600-127">Amikor HDInsight-fürtöt hoz létre hello portál, kattintson a **opcionális konfigurációs**, és kattintson a **Parancsfájlműveletek**.</span><span class="sxs-lookup"><span data-stu-id="2c600-127">When you create an HDInsight cluster from hello Portal, click **Optional Configuration**, and then click **Script Actions**.</span></span>
2. <span data-ttu-id="2c600-128">A hello **Parancsfájlműveletek** lapján adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="2c600-128">On hello **Script Actions** page, enter hello following values:</span></span>

    <span data-ttu-id="2c600-129">![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "használata parancsfájlművelet toocustomize fürt")</span><span class="sxs-lookup"><span data-stu-id="2c600-129">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="2c600-130">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2c600-130">Property</span></span></th><th><span data-ttu-id="2c600-131">Érték</span><span class="sxs-lookup"><span data-stu-id="2c600-131">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="2c600-132">Név</span><span class="sxs-lookup"><span data-stu-id="2c600-132">Name</span></span></td>
            <td><span data-ttu-id="2c600-133">Nevezze el hello parancsfájlművelet, például <b>R telepítéséhez</b>.</span><span class="sxs-lookup"><span data-stu-id="2c600-133">Specify a name for hello script action, for example, <b>Install R</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="2c600-134">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="2c600-134">Script URI</span></span></td>
            <td><span data-ttu-id="2c600-135">Adja meg a hello URI toohello parancsfájl, amely meghívott toocustomize hello fürt, például <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span><span class="sxs-lookup"><span data-stu-id="2c600-135">Specify hello URI toohello script that is invoked toocustomize hello cluster, for example, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="2c600-136">Csomóponttípus</span><span class="sxs-lookup"><span data-stu-id="2c600-136">Node Type</span></span></td>
            <td><span data-ttu-id="2c600-137">Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="2c600-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="2c600-138">Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>munkavégző csomópontokhoz</b> csak.</span><span class="sxs-lookup"><span data-stu-id="2c600-138">You can choose <b>All Nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes</b> only.</span></span>
        <tr><td><span data-ttu-id="2c600-139">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2c600-139">Parameters</span></span></td>
            <td><span data-ttu-id="2c600-140">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="2c600-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="2c600-141">Hello parancsfájl tooinstall R azonban nem szükséges paramétereket, ezért üresen hagyhatja, ez.</span><span class="sxs-lookup"><span data-stu-id="2c600-141">However, hello script tooinstall R does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="2c600-142">Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2c600-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="2c600-143">Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart crating hello fürt.</span><span class="sxs-lookup"><span data-stu-id="2c600-143">After you have added hello scripts, click hello check mark toostart crating hello cluster.</span></span>

<span data-ttu-id="2c600-144">A HDInsight is használhatja hello parancsfájl tooinstall R az Azure PowerShell vagy a hello HDInsight .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="2c600-144">You can also use hello script tooinstall R on HDInsight by using Azure PowerShell or hello HDInsight .NET SDK.</span></span> <span data-ttu-id="2c600-145">Ezek az eljárások az utasításokat a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="2c600-145">Instructions for these procedures are provided later in this article.</span></span>

## <a name="run-r-scripts"></a><span data-ttu-id="2c600-146">R parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="2c600-146">Run R scripts</span></span>
<span data-ttu-id="2c600-147">Ez a szakasz ismerteti, hogyan hello Hadoop toorun egy R parancsfájlt a HDInsight fürt.</span><span class="sxs-lookup"><span data-stu-id="2c600-147">This section describes how toorun an R script on hello Hadoop cluster with HDInsight.</span></span>

1. <span data-ttu-id="2c600-148">**A távoli asztali kapcsolat toohello fürt létrehozására**: hello portálon, a távoli asztal engedélyezése a telepített r létrehozott hello fürthöz, és csatlakoztassa a toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="2c600-148">**Establish a Remote Desktop connection toohello cluster**: From hello Portal, enable Remote Desktop for hello cluster you created with R installed, and then connect toohello cluster.</span></span> <span data-ttu-id="2c600-149">Útmutatásért lásd: [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="2c600-149">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="2c600-150">**Nyissa meg hello R konzol**: hello R-telepítés helyezi a hivatkozás toohello R konzol hello központi csomópont hello asztalon.</span><span class="sxs-lookup"><span data-stu-id="2c600-150">**Open hello R console**: hello R installation puts a link toohello R console on hello desktop of hello head node.</span></span> <span data-ttu-id="2c600-151">Kattintson rá tooopen hello R konzol.</span><span class="sxs-lookup"><span data-stu-id="2c600-151">Click on it tooopen hello R console.</span></span>
3. <span data-ttu-id="2c600-152">**Hello R-parancsfájl futtatása**: hello R parancsfájlt közvetlenül hello R konzolról illeszti, ha kiválasztja, és nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="2c600-152">**Run hello R script**: hello R script can be run directly from hello R console by pasting it, selecting it, and pressing ENTER.</span></span> <span data-ttu-id="2c600-153">Ez egy egyszerű példa parancsfájlt, amely hello számok 1 too100 állít elő, majd 2 szorzatát.</span><span class="sxs-lookup"><span data-stu-id="2c600-153">Here is a simple example script that generates hello numbers 1 too100 and then multiplies them by 2.</span></span>

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

<span data-ttu-id="2c600-154">hello első két sorok hívás hello RHadoop szalagtárak telepített R. hello végső sor megrendelése hello eredmények toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="2c600-154">hello first two lines call hello RHadoop libraries that are installed with R. hello final line prints hello results toohello console.</span></span> <span data-ttu-id="2c600-155">hello kimeneti kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="2c600-155">hello output should look like this:</span></span>

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a><span data-ttu-id="2c600-156">Segítségével a következőkre PowerShell R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="2c600-156">Install R using Aure PowerShell</span></span>
<span data-ttu-id="2c600-157">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="2c600-157">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="2c600-158">hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="2c600-158">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="2c600-159">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span><span class="sxs-lookup"><span data-stu-id="2c600-159">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).</span></span>

## <a name="install-r-using-net-sdk"></a><span data-ttu-id="2c600-160">.NET SDK használatával R telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="2c600-160">Install R using .NET SDK</span></span>
<span data-ttu-id="2c600-161">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="2c600-161">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="2c600-162">hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello.</span><span class="sxs-lookup"><span data-stu-id="2c600-162">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="2c600-163">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span><span class="sxs-lookup"><span data-stu-id="2c600-163">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).</span></span>

## <a name="see-also"></a><span data-ttu-id="2c600-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2c600-164">See also</span></span>
* [<span data-ttu-id="2c600-165">Telepítheti és használhatja R HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="2c600-165">Install and use R on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-r-scripts-linux.md)
* <span data-ttu-id="2c600-166">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md): általános információk a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c600-166">[Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): general information on creating HDInsight clusters</span></span>
* <span data-ttu-id="2c600-167">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: általános információk a HDInsight-parancsfájlművelet fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="2c600-167">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action</span></span>
* [<span data-ttu-id="2c600-168">A HDInsight parancsfájlművelet-parancsfájlok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="2c600-168">Develop Script Action scripts for HDInsight</span></span>](hdinsight-hadoop-script-actions.md)
* <span data-ttu-id="2c600-169">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-minta</span><span class="sxs-lookup"><span data-stu-id="2c600-169">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark</span></span>
* <span data-ttu-id="2c600-170">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítéséről</span><span class="sxs-lookup"><span data-stu-id="2c600-170">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph</span></span>
* <span data-ttu-id="2c600-171">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md): parancsfájlművelet minta Solr telepítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2c600-171">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md): Script Action sample about installing Solr.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
