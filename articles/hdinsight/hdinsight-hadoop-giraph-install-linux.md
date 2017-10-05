---
title: "Telepítheti és használhatja Giraph HDInsight (Hadoop) - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan Giraph telepítése Linux-alapú HDInsight-fürtök Parancsfájlműveletek használatával. A Parancsfájlműveletek engedélyezi, hogy testre szabhatja a fürt létrehozásakor fürtkonfiguráció módosításával, vagy a szolgáltatások és segédprogramok telepítése."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9fcac906-8f06-4002-9fe8-473e42f8fd0f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 6e2f6983e00f874420f7f0907dbc68185f0af713
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a><span data-ttu-id="7cb29-104">HDInsight Hadoop-fürtök Giraph telepítse, és nagy méretű diagramjait feldolgozásához Giraph használni</span><span class="sxs-lookup"><span data-stu-id="7cb29-104">Install Giraph on HDInsight Hadoop clusters, and use Giraph to process large-scale graphs</span></span>

<span data-ttu-id="7cb29-105">Útmutató Apache Giraph telepítse a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-105">Learn how to install Apache Giraph on an HDInsight cluster.</span></span> <span data-ttu-id="7cb29-106">A HDInsight a parancsfájl művelet a szolgáltatás lehetővé teszi a fürt testreszabását bash parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7cb29-106">The script action feature of HDInsight allows you to customize your cluster by running a bash script.</span></span> <span data-ttu-id="7cb29-107">Parancsfájlok segítségével testre szabhatja a fürtök alatt és a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="7cb29-107">Scripts can be used to customize clusters during and after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7cb29-108">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="7cb29-108">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="7cb29-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="7cb29-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7cb29-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7cb29-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="7cb29-111"><a name="whatis"></a>Mi az a Giraph</span><span class="sxs-lookup"><span data-stu-id="7cb29-111"><a name="whatis"></a>What is Giraph</span></span>

<span data-ttu-id="7cb29-112">[Apache Giraph](http://giraph.apache.org/) lehetővé teszi a végrehajtását diagramfeldolgozás Hadoop használatával, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="7cb29-112">[Apache Giraph](http://giraph.apache.org/) allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="7cb29-113">Diagramokat objektumok közötti kapcsolatok modellezésére.</span><span class="sxs-lookup"><span data-stu-id="7cb29-113">Graphs model relationships between objects.</span></span> <span data-ttu-id="7cb29-114">Például a nagy hálózati útválasztók közötti kapcsolatok, mint például az internetről, vagy a közösségi hálózatokkal személyek közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="7cb29-114">For example, the connections between routers on a large network like the Internet, or relationships between people on social networks.</span></span> <span data-ttu-id="7cb29-115">Graph feldolgozás lehetővé teszi a ok egy grafikonon objektumok közötti kapcsolatok, mint:</span><span class="sxs-lookup"><span data-stu-id="7cb29-115">Graph processing allows you to reason about the relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="7cb29-116">A jelenlegi kapcsolatok alapján lehetséges ismerősök azonosítása.</span><span class="sxs-lookup"><span data-stu-id="7cb29-116">Identifying potential friends based on your current relationships.</span></span>

* <span data-ttu-id="7cb29-117">Azonosítja a legrövidebb útvonal hálózatban két számítógép között.</span><span class="sxs-lookup"><span data-stu-id="7cb29-117">Identifying the shortest route between two computers in a network.</span></span>

* <span data-ttu-id="7cb29-118">A lap rangot a weblapok kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="7cb29-118">Calculating the page rank of webpages.</span></span>

> [!WARNING]
> <span data-ttu-id="7cb29-119">A HDInsight-fürt összetevői teljes mértékben támogatottak, mert a Microsoft Support segít elkülöníteni, és ezeket az összetevőket kapcsolatos problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="7cb29-119">Components provided with the HDInsight cluster are fully supported - Microsoft Support helps to isolate and resolve issues related to these components.</span></span>
>
> <span data-ttu-id="7cb29-120">Egyéni összetevők, például Giraph, minden üzleti szempontból ésszerű terméktámogatási segítséget nyújtanak a probléma további hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="7cb29-120">Custom components, such as Giraph, receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="7cb29-121">Microsoft Support lehet a probléma megoldását.</span><span class="sxs-lookup"><span data-stu-id="7cb29-121">Microsoft Support may be able to resolving the issue.</span></span> <span data-ttu-id="7cb29-122">Ha nem, ahol részletes segítséget, hogy a technológiát található nyílt forráskódú Közösségek kell tájékozódnia.</span><span class="sxs-lookup"><span data-stu-id="7cb29-122">If not, you must consult open source communities where deep expertise for that technology is found.</span></span> <span data-ttu-id="7cb29-123">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="7cb29-123">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="7cb29-124">Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="7cb29-124">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>


## <a name="what-the-script-does"></a><span data-ttu-id="7cb29-125">A parancsfájl funkciója</span><span class="sxs-lookup"><span data-stu-id="7cb29-125">What the script does</span></span>

<span data-ttu-id="7cb29-126">Ezt a parancsfájlt a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="7cb29-126">This script performs the following actions:</span></span>

* <span data-ttu-id="7cb29-127">Giraph történő telepítése`/usr/hdp/current/giraph`</span><span class="sxs-lookup"><span data-stu-id="7cb29-127">Installs Giraph to `/usr/hdp/current/giraph`</span></span>

* <span data-ttu-id="7cb29-128">Másolja a `giraph-examples.jar` fájl alapértelmezett Storage (WASB) a fürt:`/example/jars/giraph-examples.jar`</span><span class="sxs-lookup"><span data-stu-id="7cb29-128">Copies the `giraph-examples.jar` file to default storage (WASB) for your cluster: `/example/jars/giraph-examples.jar`</span></span>

## <span data-ttu-id="7cb29-129"><a name="install"></a>A Parancsfájlműveletek segítségével Giraph telepítése</span><span class="sxs-lookup"><span data-stu-id="7cb29-129"><a name="install"></a>Install Giraph using Script Actions</span></span>

<span data-ttu-id="7cb29-130">Egy HDInsight-fürtök Giraph telepítendő parancsfájlt a következő helyen érhető el:</span><span class="sxs-lookup"><span data-stu-id="7cb29-130">A sample script to install Giraph on an HDInsight cluster is available at the following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

<span data-ttu-id="7cb29-131">Ez a szakasz utasításokat biztosít a minta-parancsfájl használata a fürt létrehozásakor az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="7cb29-131">This section provides instructions on how to use the sample script while creating the cluster by using the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="7cb29-132">A parancsfájl művelet a következő módszerekkel történő alkalmazhatók:</span><span class="sxs-lookup"><span data-stu-id="7cb29-132">A script action can be applied using any of the following methods:</span></span>
> * <span data-ttu-id="7cb29-133">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cb29-133">Azure PowerShell</span></span>
> * <span data-ttu-id="7cb29-134">Az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="7cb29-134">The Azure CLI</span></span>
> * <span data-ttu-id="7cb29-135">A HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7cb29-135">The HDInsight .NET SDK</span></span>
> * <span data-ttu-id="7cb29-136">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="7cb29-136">Azure Resource Manager templates</span></span>
> 
> <span data-ttu-id="7cb29-137">Már fut a fürtök Parancsfájlműveletek is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7cb29-137">You can also apply script actions to already running clusters.</span></span> <span data-ttu-id="7cb29-138">További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7cb29-138">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="7cb29-139">Indítsa el a fürt létrehozása a lépések segítségével [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md), de nem hajtja végre létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cb29-139">Start creating a cluster by using the steps in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md), but do not complete creation.</span></span>

2. <span data-ttu-id="7cb29-140">Az a **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="7cb29-140">On the **Optional Configuration** blade, select **Script Actions**, and provide the following information:</span></span>

   * <span data-ttu-id="7cb29-141">**NÉV**: Adja meg a parancsfájlművelet rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="7cb29-141">**NAME**: Enter a friendly name for the script action.</span></span>

   * <span data-ttu-id="7cb29-142">**PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="7cb29-142">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span></span>

   * <span data-ttu-id="7cb29-143">**HEAD**: Ez a bejegyzés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7cb29-143">**HEAD**: Check this entry</span></span>

   * <span data-ttu-id="7cb29-144">**MUNKAVÉGZŐ**: hagyja bejelölve ezt a bejegyzést</span><span class="sxs-lookup"><span data-stu-id="7cb29-144">**WORKER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="7cb29-145">**ZOOKEEPER**: hagyja bejelölve ezt a bejegyzést</span><span class="sxs-lookup"><span data-stu-id="7cb29-145">**ZOOKEEPER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="7cb29-146">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="7cb29-146">**PARAMETERS**: Leave this field blank</span></span>

3. <span data-ttu-id="7cb29-147">Alján a **Parancsfájlműveletek**, használja a **válasszon** gombra kattintva mentse a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-147">At the bottom of the **Script Actions**, use the **Select** button to save the configuration.</span></span> <span data-ttu-id="7cb29-148">Végül a **válasszon** gomb alján a **opcionális konfigurációs** panelt, és mentse a nem kötelező konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cb29-148">Finally, use the **Select** button at the bottom of the **Optional Configuration** blade to save the optional configuration information.</span></span>

4. <span data-ttu-id="7cb29-149">A fürt létrehozása, a folytatáshoz [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7cb29-149">Continue creating the cluster as described in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

## <span data-ttu-id="7cb29-150"><a name="usegiraph"></a>Giraph használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="7cb29-150"><a name="usegiraph"></a>How do I use Giraph in HDInsight?</span></span>

<span data-ttu-id="7cb29-151">Ha a fürt létrejött, a következő lépésekkel a mellékelt Giraph SimpleShortestPathsComputation példa futtatásához.</span><span class="sxs-lookup"><span data-stu-id="7cb29-151">Once the cluster has been created, use the following steps to run the SimpleShortestPathsComputation example included with Giraph.</span></span> <span data-ttu-id="7cb29-152">Ez a példa a basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) megvalósítása a legrövidebb elérési út egy grafikonon objektumok közötti kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="7cb29-152">This example uses the basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementation for finding the shortest path between objects in a graph.</span></span>

1. <span data-ttu-id="7cb29-153">Csatlakozzon SSH-val a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="7cb29-153">Connect to the HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="7cb29-154">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7cb29-154">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="7cb29-155">Az alábbi parancs segítségével hozzon létre egy fájlt **tiny_graph.txt**:</span><span class="sxs-lookup"><span data-stu-id="7cb29-155">Use the following command to create a file named **tiny_graph.txt**:</span></span>

    ```bash
    nano tiny_graph.txt
    ```

    <span data-ttu-id="7cb29-156">Ez a fájl tartalmát a következő szöveg használata:</span><span class="sxs-lookup"><span data-stu-id="7cb29-156">Use the following text as the contents of this file:</span></span>

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    <span data-ttu-id="7cb29-157">Ezek az adatok irányított gráf, a következő formátumban objektumok közötti kapcsolatot ismerteti `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span><span class="sxs-lookup"><span data-stu-id="7cb29-157">This data describes a relationship between objects in a directed graph, by using the format `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span></span> <span data-ttu-id="7cb29-158">Minden egyes sorban közötti kapcsolatot jelent a `source_id` objektum és egy vagy több `dest_id` objektumok.</span><span class="sxs-lookup"><span data-stu-id="7cb29-158">Each line represents a relationship between a `source_id` object and one or more `dest_id` objects.</span></span> <span data-ttu-id="7cb29-159">A `edge_value` -re a erőssége vagy a kapcsolat közötti távolság `source_id` és `dest\_id`.</span><span class="sxs-lookup"><span data-stu-id="7cb29-159">The `edge_value` can be thought of as the strength or distance of the connection between `source_id` and `dest\_id`.</span></span>

    <span data-ttu-id="7cb29-160">Jelenik meg, és objektumok közötti távolságot a érték (vagy súly) használ, az adatok nézhet ki például az alábbi ábra:</span><span class="sxs-lookup"><span data-stu-id="7cb29-160">Drawn out, and using the value (or weight) as the distance between objects, the data might look like the following diagram:</span></span>

    ![különböző távolságát sornyi kör Megrajzolás tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. <span data-ttu-id="7cb29-162">Mentse a fájlt, használja a **Ctrl + X**, majd **Y**, és végül **Enter** fogadja el a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-162">To save the file, use **Ctrl+X**, then **Y**, and finally **Enter** to accept the file name.</span></span>

4. <span data-ttu-id="7cb29-163">A HDInsight-fürtjéhez elsődleges tárba. az adatok tárolásához használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="7cb29-163">Use the following to store the data into primary storage for your HDInsight cluster:</span></span>

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. <span data-ttu-id="7cb29-164">Futtassa a SimpleShortestPathsComputation például a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7cb29-164">Run the SimpleShortestPathsComputation example using the following command:</span></span>

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    <span data-ttu-id="7cb29-165">Ezzel a paranccsal használt paraméterek a következő táblázat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="7cb29-165">The parameters used with this command are described in the following table:</span></span>

   | <span data-ttu-id="7cb29-166">Paraméter</span><span class="sxs-lookup"><span data-stu-id="7cb29-166">Parameter</span></span> | <span data-ttu-id="7cb29-167">Funkció</span><span class="sxs-lookup"><span data-stu-id="7cb29-167">What it does</span></span> |
   | --- | --- |
   | `jar` |<span data-ttu-id="7cb29-168">A példák tartalmazó jar-fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-168">The jar file containing the examples.</span></span> |
   | `org.apache.giraph.GiraphRunner` |<span data-ttu-id="7cb29-169">Az osztály a példák indításához használt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-169">The class used to start the examples.</span></span> |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |<span data-ttu-id="7cb29-170">A példában használt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-170">The example that is used.</span></span> <span data-ttu-id="7cb29-171">Ebben a példában kiszámítja a legrövidebb elérési azonosítója 1 és a Graph más azonosítók között.</span><span class="sxs-lookup"><span data-stu-id="7cb29-171">In this example, it computes the shortest path between ID 1 and all other IDs in the graph.</span></span> |
   | `-ca mapred.job.tracker` |<span data-ttu-id="7cb29-172">A fürt headnode.</span><span class="sxs-lookup"><span data-stu-id="7cb29-172">The headnode for the cluster.</span></span> |
   | `-vif` |<span data-ttu-id="7cb29-173">A bemeneti az formátumát a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="7cb29-173">The input format to use for the input data.</span></span> |
   | `-vip` |<span data-ttu-id="7cb29-174">A bemeneti adatfájlt.</span><span class="sxs-lookup"><span data-stu-id="7cb29-174">The input data file.</span></span> |
   | `-vof` |<span data-ttu-id="7cb29-175">A kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="7cb29-175">The output format.</span></span> <span data-ttu-id="7cb29-176">A példa, azonosítója és egyszerű szövegként érték.</span><span class="sxs-lookup"><span data-stu-id="7cb29-176">In this example, ID and value as plain text.</span></span> |
   | `-op` |<span data-ttu-id="7cb29-177">A kimeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="7cb29-177">The output location.</span></span> |
   | `-w 2` |<span data-ttu-id="7cb29-178">A munkavállalók használandó száma.</span><span class="sxs-lookup"><span data-stu-id="7cb29-178">The number of workers to use.</span></span> <span data-ttu-id="7cb29-179">Ebben a példában, 2.</span><span class="sxs-lookup"><span data-stu-id="7cb29-179">In this example, 2.</span></span> |

    <span data-ttu-id="7cb29-180">Ezeket és más Giraph minták használt paraméterek további információkért lásd: a [Giraph gyors üzembe helyezés](http://giraph.apache.org/quick_start.html).</span><span class="sxs-lookup"><span data-stu-id="7cb29-180">For more information on these, and other parameters used with Giraph samples, see the [Giraph quickstart](http://giraph.apache.org/quick_start.html).</span></span>

6. <span data-ttu-id="7cb29-181">Ha a feladat befejeződött, az eredmények tárolódnak a **/example/out/shotestpaths** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7cb29-181">Once the job has finished, the results are stored in the **/example/out/shotestpaths** directory.</span></span> <span data-ttu-id="7cb29-182">A kimeneti fájl kezdődő **rész-m -** és véget a száma, amelyben az első, a második, a fájl stb.</span><span class="sxs-lookup"><span data-stu-id="7cb29-182">The output file names begin with **part-m-** and end with a number indicating the first, second, etc. file.</span></span> <span data-ttu-id="7cb29-183">A következő paranccsal eredményének megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="7cb29-183">Use the following command to view the output:</span></span>

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    <span data-ttu-id="7cb29-184">A kimenet az alábbihoz hasonló kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="7cb29-184">The output should appear similar to the following text:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="7cb29-185">A kódolt kezdődnie példája rögzített SimpleShortestPathComputation azonosítója 1 objektum, és keresse meg a legrövidebb más objektumok elérési útja.</span><span class="sxs-lookup"><span data-stu-id="7cb29-185">The SimpleShortestPathComputation example is hard coded to start with object ID 1 and find the shortest path to other objects.</span></span> <span data-ttu-id="7cb29-186">A kimeneti formátumban van `destination_id` és `distance`.</span><span class="sxs-lookup"><span data-stu-id="7cb29-186">The output is in the format of `destination_id` and `distance`.</span></span> <span data-ttu-id="7cb29-187">A `distance` objektum azonosítója 1 és a célként megadott azonosító közötti távolság széleinek érték (vagy súlya)</span><span class="sxs-lookup"><span data-stu-id="7cb29-187">The `distance` is the value (or weight) of the edges traveled between object ID 1 and the target ID.</span></span>

    <span data-ttu-id="7cb29-188">Ezek az adatok megjelenítése, ellenőrizheti az eredményeket a legrövidebb elérési utak utazás azonosítója 1 és egyéb objektumok között.</span><span class="sxs-lookup"><span data-stu-id="7cb29-188">Visualizing this data, you can verify the results by traveling the shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="7cb29-189">A legrövidebb azonosítója 1 és 4 azonosító közötti elérési út 5.</span><span class="sxs-lookup"><span data-stu-id="7cb29-189">The shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="7cb29-190">Az értéket nem teljes távolságát <span style="color:orange">azonosítója 1. és 3</span>, majd <span style="color:red">azonosító 3. és 4</span>.</span><span class="sxs-lookup"><span data-stu-id="7cb29-190">This value is the total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Az objektumok rajzolási körök mint a legrövidebb elérési utak között](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a><span data-ttu-id="7cb29-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cb29-192">Next steps</span></span>

* <span data-ttu-id="7cb29-193">[Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7cb29-193">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span>

* <span data-ttu-id="7cb29-194">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7cb29-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span>
