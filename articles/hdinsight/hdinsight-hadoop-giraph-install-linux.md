---
title: "aaaInstall és Giraph használata a HDInsight (Hadoop) - Azure-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall Giraph a Linux-alapú HDInsight clusters Parancsfájlműveletek használatával. A Parancsfájlműveletek lehetővé teszik toocustomize hello fürt létrehozásakor fürtkonfiguráció módosításával, vagy a szolgáltatások és segédprogramok telepítése."
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
ms.openlocfilehash: 0f195b65cebf5e24d1808ef33b95b4d362555521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-tooprocess-large-scale-graphs"></a><span data-ttu-id="6fe8d-104">HDInsight Hadoop-fürtök Giraph telepítse, és Giraph tooprocess nagyméretű diagramjait használata</span><span class="sxs-lookup"><span data-stu-id="6fe8d-104">Install Giraph on HDInsight Hadoop clusters, and use Giraph tooprocess large-scale graphs</span></span>

<span data-ttu-id="6fe8d-105">Megtudhatja, hogyan tooinstall Apache Giraph a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-105">Learn how tooinstall Apache Giraph on an HDInsight cluster.</span></span> <span data-ttu-id="6fe8d-106">hello parancsfájl művelet a HDInsight lehetővé teszi toocustomize a fürt bash parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-106">hello script action feature of HDInsight allows you toocustomize your cluster by running a bash script.</span></span> <span data-ttu-id="6fe8d-107">Parancsfájlok lehet használt toocustomize fürtök alatt és a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-107">Scripts can be used toocustomize clusters during and after cluster creation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6fe8d-108">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-108">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="6fe8d-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6fe8d-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="6fe8d-111"><a name="whatis"></a>Mi az a Giraph</span><span class="sxs-lookup"><span data-stu-id="6fe8d-111"><a name="whatis"></a>What is Giraph</span></span>

<span data-ttu-id="6fe8d-112">[Apache Giraph](http://giraph.apache.org/) lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-112">[Apache Giraph](http://giraph.apache.org/) allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span> <span data-ttu-id="6fe8d-113">Diagramokat objektumok közötti kapcsolatok modellezésére.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-113">Graphs model relationships between objects.</span></span> <span data-ttu-id="6fe8d-114">Például hello kapcsolatok nagy hálózati útválasztók között, mint például hello Internet, vagy a közösségi hálózatokkal személyek közötti kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-114">For example, hello connections between routers on a large network like hello Internet, or relationships between people on social networks.</span></span> <span data-ttu-id="6fe8d-115">Graph feldolgozás lehetővé teszi a tooreason kapcsolatos hello kapcsolatai egy grafikonon objektumok, mint:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-115">Graph processing allows you tooreason about hello relationships between objects in a graph, such as:</span></span>

* <span data-ttu-id="6fe8d-116">A jelenlegi kapcsolatok alapján lehetséges ismerősök azonosítása.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-116">Identifying potential friends based on your current relationships.</span></span>

* <span data-ttu-id="6fe8d-117">Azonosító hello legrövidebb útvonal hálózatban két számítógép között.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-117">Identifying hello shortest route between two computers in a network.</span></span>

* <span data-ttu-id="6fe8d-118">A weblapok hello lap indulva számított rangját kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-118">Calculating hello page rank of webpages.</span></span>

> [!WARNING]
> <span data-ttu-id="6fe8d-119">Hello HDInsight-fürt összetevői teljes mértékben támogatottak, mert a Microsoft Support tooisolate segítségével, és hárítsa el a problémákat kapcsolódó toothese összetevőket.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-119">Components provided with hello HDInsight cluster are fully supported - Microsoft Support helps tooisolate and resolve issues related toothese components.</span></span>
>
> <span data-ttu-id="6fe8d-120">Egyéni összetevők, például Giraph, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-120">Custom components, such as Giraph, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="6fe8d-121">Microsoft Support képes tooresolving hello probléma lehet.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-121">Microsoft Support may be able tooresolving hello issue.</span></span> <span data-ttu-id="6fe8d-122">Ha nem, ahol részletes segítséget, hogy a technológiát található nyílt forráskódú Közösségek kell tájékozódnia.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-122">If not, you must consult open source communities where deep expertise for that technology is found.</span></span> <span data-ttu-id="6fe8d-123">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-123">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>


## <a name="what-hello-script-does"></a><span data-ttu-id="6fe8d-124">Milyen hello parancsprogram</span><span class="sxs-lookup"><span data-stu-id="6fe8d-124">What hello script does</span></span>

<span data-ttu-id="6fe8d-125">Ez a parancsfájl hello a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-125">This script performs hello following actions:</span></span>

* <span data-ttu-id="6fe8d-126">Giraph túl telepíti`/usr/hdp/current/giraph`</span><span class="sxs-lookup"><span data-stu-id="6fe8d-126">Installs Giraph too`/usr/hdp/current/giraph`</span></span>

* <span data-ttu-id="6fe8d-127">Másolatot hello `giraph-examples.jar` fájltárolás toodefault (WASB) a fürt:`/example/jars/giraph-examples.jar`</span><span class="sxs-lookup"><span data-stu-id="6fe8d-127">Copies hello `giraph-examples.jar` file toodefault storage (WASB) for your cluster: `/example/jars/giraph-examples.jar`</span></span>

## <span data-ttu-id="6fe8d-128"><a name="install"></a>A Parancsfájlműveletek segítségével Giraph telepítése</span><span class="sxs-lookup"><span data-stu-id="6fe8d-128"><a name="install"></a>Install Giraph using Script Actions</span></span>

<span data-ttu-id="6fe8d-129">Egy minta parancsfájlt tooinstall Giraph egy HDInsight-fürt hello a következő helyen érhető el:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-129">A sample script tooinstall Giraph on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

<span data-ttu-id="6fe8d-130">Ez a szakasz útmutatás hogyan toouse hello hello Azure-portál használatával hello fürt létrehozásakor minta parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-130">This section provides instructions on how toouse hello sample script while creating hello cluster by using hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="6fe8d-131">A parancsfájlművelet hello a következő módszerek bármelyikével alkalmazhatók:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-131">A script action can be applied using any of hello following methods:</span></span>
> * <span data-ttu-id="6fe8d-132">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6fe8d-132">Azure PowerShell</span></span>
> * <span data-ttu-id="6fe8d-133">hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="6fe8d-133">hello Azure CLI</span></span>
> * <span data-ttu-id="6fe8d-134">hello HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="6fe8d-134">hello HDInsight .NET SDK</span></span>
> * <span data-ttu-id="6fe8d-135">Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="6fe8d-135">Azure Resource Manager templates</span></span>
> 
> <span data-ttu-id="6fe8d-136">A futó fürtök parancsfájl műveletek tooalready is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-136">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="6fe8d-137">További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-137">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="6fe8d-138">Indítsa el a fürt létrehozása a hello lépések segítségével [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md), de nem hajtja végre létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-138">Start creating a cluster by using hello steps in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md), but do not complete creation.</span></span>

2. <span data-ttu-id="6fe8d-139">A hello **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-139">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="6fe8d-140">**NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-140">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="6fe8d-141">**PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="6fe8d-141">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh</span></span>

   * <span data-ttu-id="6fe8d-142">**HEAD**: Ez a bejegyzés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6fe8d-142">**HEAD**: Check this entry</span></span>

   * <span data-ttu-id="6fe8d-143">**MUNKAVÉGZŐ**: hagyja bejelölve ezt a bejegyzést</span><span class="sxs-lookup"><span data-stu-id="6fe8d-143">**WORKER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="6fe8d-144">**ZOOKEEPER**: hagyja bejelölve ezt a bejegyzést</span><span class="sxs-lookup"><span data-stu-id="6fe8d-144">**ZOOKEEPER**: Leave this entry unchecked</span></span>

   * <span data-ttu-id="6fe8d-145">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="6fe8d-145">**PARAMETERS**: Leave this field blank</span></span>

3. <span data-ttu-id="6fe8d-146">Hello hello alján **Parancsfájlműveletek**, hello használata **válasszon** gombok toosave hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-146">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="6fe8d-147">Végezetül a hello használata **kiválasztása** hello hello alján gomb **opcionális konfigurációs** panel toosave hello opcionális konfigurációs információkat.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-147">Finally, use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

4. <span data-ttu-id="6fe8d-148">Hello fürtöt hoz létre, a folytatáshoz [létrehozása Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-148">Continue creating hello cluster as described in [Create Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

## <span data-ttu-id="6fe8d-149"><a name="usegiraph"></a>Giraph használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="6fe8d-149"><a name="usegiraph"></a>How do I use Giraph in HDInsight?</span></span>

<span data-ttu-id="6fe8d-150">Hello fürt létrehozása után használja a következő lépéseket toorun hello SimpleShortestPathsComputation példa Giraph mellékelt hello.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-150">Once hello cluster has been created, use hello following steps toorun hello SimpleShortestPathsComputation example included with Giraph.</span></span> <span data-ttu-id="6fe8d-151">Ez a példa hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) megvalósítása a hello legrövidebb elérési út egy grafikonon objektumok közötti kereséshez.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-151">This example uses hello basic [Pregel](http://people.apache.org/~edwardyoon/documents/pregel.pdf) implementation for finding hello shortest path between objects in a graph.</span></span>

1. <span data-ttu-id="6fe8d-152">Csatlakozzon az SSH használatával toohello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-152">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="6fe8d-153">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-153">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="6fe8d-154">Használjon hello következő parancsot a toocreate nevű fájlt **tiny_graph.txt**:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-154">Use hello following command toocreate a file named **tiny_graph.txt**:</span></span>

    ```bash
    nano tiny_graph.txt
    ```

    <span data-ttu-id="6fe8d-155">Szöveg hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-155">Use hello following text as hello contents of this file:</span></span>

    ```text
    [0,0,[[1,1],[3,3]]]
    [1,0,[[0,1],[2,2],[3,1]]]
    [2,0,[[1,2],[4,4]]]
    [3,0,[[0,3],[1,1],[4,4]]]
    [4,0,[[3,4],[2,4]]]
    ```

    <span data-ttu-id="6fe8d-156">Ezek az adatok egy irányított gráf hello formátum használatával az objektumok közötti kapcsolatot ismerteti `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-156">This data describes a relationship between objects in a directed graph, by using hello format `[source_id, source_value,[[dest_id], [edge_value],...]]`.</span></span> <span data-ttu-id="6fe8d-157">Minden egyes sorban közötti kapcsolatot jelent a `source_id` objektum és egy vagy több `dest_id` objektumok.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-157">Each line represents a relationship between a `source_id` object and one or more `dest_id` objects.</span></span> <span data-ttu-id="6fe8d-158">Hello `edge_value` -re hello erőssége vagy hello kapcsolat közötti távolság `source_id` és `dest\_id`.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-158">hello `edge_value` can be thought of as hello strength or distance of hello connection between `source_id` and `dest\_id`.</span></span>

    <span data-ttu-id="6fe8d-159">Jelenik meg, és objektumok közötti távolság hello hello érték (vagy súly) használ, hello adatok nézhet ki például a következő diagram hello:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-159">Drawn out, and using hello value (or weight) as hello distance between objects, hello data might look like hello following diagram:</span></span>

    ![különböző távolságát sornyi kör Megrajzolás tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

3. <span data-ttu-id="6fe8d-161">toosave hello fájl használata **Ctrl + X**, majd **Y**, és végül **Enter** tooaccept hello fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-161">toosave hello file, use **Ctrl+X**, then **Y**, and finally **Enter** tooaccept hello file name.</span></span>

4. <span data-ttu-id="6fe8d-162">A HDInsight-fürthöz elsődleges tárba toostore hello adatokat követő hello használata:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-162">Use hello following toostore hello data into primary storage for your HDInsight cluster:</span></span>

    ```bash
    hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt
    ```

5. <span data-ttu-id="6fe8d-163">Futtassa a hello SimpleShortestPathsComputation példa hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-163">Run hello SimpleShortestPathsComputation example using hello following command:</span></span>

    ```bash
    yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2
    ```

    <span data-ttu-id="6fe8d-164">Ez a parancs paramétereit hello hello a következő táblázat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-164">hello parameters used with this command are described in hello following table:</span></span>

   | <span data-ttu-id="6fe8d-165">Paraméter</span><span class="sxs-lookup"><span data-stu-id="6fe8d-165">Parameter</span></span> | <span data-ttu-id="6fe8d-166">Funkció</span><span class="sxs-lookup"><span data-stu-id="6fe8d-166">What it does</span></span> |
   | --- | --- |
   | `jar` |<span data-ttu-id="6fe8d-167">hello jar-fájlt tartalmazó hello példák.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-167">hello jar file containing hello examples.</span></span> |
   | `org.apache.giraph.GiraphRunner` |<span data-ttu-id="6fe8d-168">hello osztályt használja a toostart hello példák.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-168">hello class used toostart hello examples.</span></span> |
   | `org.apache.giraph.examples.SimpleShortestPathsCoputation` |<span data-ttu-id="6fe8d-169">hello példára.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-169">hello example that is used.</span></span> <span data-ttu-id="6fe8d-170">Ebben a példában kiszámítja hello legrövidebb elérési út azonosítója 1 és más hello Graph azonosítók között.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-170">In this example, it computes hello shortest path between ID 1 and all other IDs in hello graph.</span></span> |
   | `-ca mapred.job.tracker` |<span data-ttu-id="6fe8d-171">hello headnode hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-171">hello headnode for hello cluster.</span></span> |
   | `-vif` |<span data-ttu-id="6fe8d-172">hello bemeneti formátum toouse hello bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-172">hello input format toouse for hello input data.</span></span> |
   | `-vip` |<span data-ttu-id="6fe8d-173">hello bemeneti adatfájlt.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-173">hello input data file.</span></span> |
   | `-vof` |<span data-ttu-id="6fe8d-174">hello kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-174">hello output format.</span></span> <span data-ttu-id="6fe8d-175">A példa, azonosítója és egyszerű szövegként érték.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-175">In this example, ID and value as plain text.</span></span> |
   | `-op` |<span data-ttu-id="6fe8d-176">hello kimeneti helyen.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-176">hello output location.</span></span> |
   | `-w 2` |<span data-ttu-id="6fe8d-177">feldolgozók toouse hello száma.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-177">hello number of workers toouse.</span></span> <span data-ttu-id="6fe8d-178">Ebben a példában, 2.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-178">In this example, 2.</span></span> |

    <span data-ttu-id="6fe8d-179">Ezeket és más Giraph minták használt paraméterek további információkért lásd: hello [Giraph gyors üzembe helyezés](http://giraph.apache.org/quick_start.html).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-179">For more information on these, and other parameters used with Giraph samples, see hello [Giraph quickstart](http://giraph.apache.org/quick_start.html).</span></span>

6. <span data-ttu-id="6fe8d-180">Miután hello feladat befejeződött, hello eredmények hello vannak tárolva **/example/out/shotestpaths** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-180">Once hello job has finished, hello results are stored in hello **/example/out/shotestpaths** directory.</span></span> <span data-ttu-id="6fe8d-181">hello kimeneti fájl kezdődő **rész-m -** és végződhet azonosítószámát hello először, másrészt fájl stb.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-181">hello output file names begin with **part-m-** and end with a number indicating hello first, second, etc. file.</span></span> <span data-ttu-id="6fe8d-182">A következő parancs tooview hello kimeneti hello használata:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-182">Use hello following command tooview hello output:</span></span>

    ```bash
    hdfs dfs -text /example/output/shortestpaths/*
    ```

    <span data-ttu-id="6fe8d-183">hello kimeneti megjelenjen-e a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="6fe8d-183">hello output should appear similar toohello following text:</span></span>

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    <span data-ttu-id="6fe8d-184">hello SimpleShortestPathComputation példa nél nagyobb toostart objektum azonosítója 1 és hello legrövidebb elérési tooother objektumok keresése.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-184">hello SimpleShortestPathComputation example is hard coded toostart with object ID 1 and find hello shortest path tooother objects.</span></span> <span data-ttu-id="6fe8d-185">hello kimeneti hello formátumban van `destination_id` és `distance`.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-185">hello output is in hello format of `destination_id` and `distance`.</span></span> <span data-ttu-id="6fe8d-186">Hello `distance` , de hello érték (súly) amennyi hello szegélye között objektum azonosítója 1 és hello cél azonosítót.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-186">hello `distance` is hello value (or weight) of hello edges traveled between object ID 1 and hello target ID.</span></span>

    <span data-ttu-id="6fe8d-187">Ezek az adatok megjelenítése, ellenőrizheti hello eredmények hello legrövidebb elérési utak utazás azonosítója 1 és egyéb objektumok között.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-187">Visualizing this data, you can verify hello results by traveling hello shortest paths between ID 1 and all other objects.</span></span> <span data-ttu-id="6fe8d-188">hello legrövidebb azonosítója 1 és 4 azonosító közötti elérési út 5.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-188">hello shortest path between ID 1 and ID 4 is 5.</span></span> <span data-ttu-id="6fe8d-189">Az értéket nem hello teljes közötti távolság szerint <span style="color:orange">azonosítója 1. és 3</span>, majd <span style="color:red">azonosító 3. és 4</span>.</span><span class="sxs-lookup"><span data-stu-id="6fe8d-189">This value is hello total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.</span></span>

    ![Az objektumok rajzolási körök mint a legrövidebb elérési utak között](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)

## <a name="next-steps"></a><span data-ttu-id="6fe8d-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6fe8d-191">Next steps</span></span>

* <span data-ttu-id="6fe8d-192">[Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-192">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span>

* <span data-ttu-id="6fe8d-193">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="6fe8d-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span>
